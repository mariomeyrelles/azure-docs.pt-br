---
title: Tutorial – Balancear a carga de máquinas virtuais do Windows no Azure | Microsoft Docs
description: Neste tutorial, você aprenderá a usar o Azure PowerShell para criar um balanceador de carga para um aplicativo altamente disponível e seguro em três máquinas virtuais do Windows
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/09/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 35de71f3cc7d865cf2235a21bebf1cfcd7f1850d
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2018
---
# <a name="tutorial-load-balance-windows-virtual-machines-in-azure-to-create-a-highly-available-application-with-azure-powershell"></a>Tutorial: Balancear a carga de máquinas virtuais do Windows no Azure para criar um aplicativo altamente disponível com o Azure PowerShell
O balanceamento de carga fornece um nível mais alto de disponibilidade, distribuindo as solicitações de entrada em várias máquinas virtuais. Neste tutorial, você aprenderá sobre os diferentes componentes do balanceador de carga do Azure que distribuem o tráfego e fornecem alta disponibilidade. Você aprenderá como:

> [!div class="checklist"]
> * Criar um balanceador de carga do Azure
> * Criar uma investigação de integridade do balanceador de carga
> * Criar regras de tráfego para o balanceador de carga
> * Usar a Extensão de Script Personalizado para criar um site do IIS básico
> * Criar máquinas virtuais e anexar a um balanceador de carga
> * Exibir um balanceador de carga em ação
> * Adicionar e remover as VMs de um balanceador de carga

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Se você optar por instalar e usar o PowerShell localmente, este tutorial exigirá o módulo do Azure PowerShell versão 5.7.0 ou posterior. Execute `Get-Module -ListAvailable AzureRM` para encontrar a versão. Se você precisa atualizar, consulte [Instalar o módulo do Azure PowerShell](/powershell/azure/install-azurerm-ps). Se você estiver executando o PowerShell localmente, também precisará executar o `Connect-AzureRmAccount` para criar uma conexão com o Azure.


## <a name="azure-load-balancer-overview"></a>Visão geral do Balanceador de Carga do Azure
Um Azure Load Balancer é um balanceador de carga de Camada 4 (TCP, UDP) que fornece alta disponibilidade distribuindo o tráfego de entrada entre VMs íntegros. Uma investigação de integridade do balanceador de carga monitora uma determinada porta em cada VM e distribui o tráfego somente para uma VM operacional.

Você define uma configuração de IP de front-end que contém um ou mais endereços IP públicos. Essa configuração de IP de front-end permite que seu balanceador de carga e os aplicativos estejam acessíveis pela Internet. 

As máquinas virtuais conectam-se a um balanceador de carga usando a placa de interface de rede virtual (NIC). Para distribuir o tráfego para as máquinas virtuais, um pool de endereços de back-end contém os endereços IP das NICs virtuais conectadas ao balanceador de carga.

Para controlar o fluxo do tráfego, você precisará definir regras do balanceador de carga para portas específicas e protocolos que mapeiam para suas VMs.


## <a name="create-azure-load-balancer"></a>Criar o Azure Load Balancer
Esta seção fornece detalhes sobre como criar e configurar cada componente do balanceador de carga. Antes de criar seu balanceador de carga, crie um grupo de recursos com [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). O seguinte exemplo cria um grupo de recursos chamado *myResourceGroupLoadBalancer* no local *EastUS*:

```azurepowershell-interactive
New-AzureRmResourceGroup `
  -ResourceGroupName "myResourceGroupLoadBalancer" `
  -Location "EastUS"
```

### <a name="create-a-public-ip-address"></a>Criar um endereço IP público
Para acessar seu aplicativo na Internet, você precisará de um endereço IP público para o balanceador de carga. Crie um endereço IP público com [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress). O exemplo a seguir cria um endereço IP público chamado *myPublicIP* no grupo de recursos *myResourceGroupLoadBalancer*:

```azurepowershell-interactive
$publicIP = New-AzureRmPublicIpAddress `
  -ResourceGroupName "myResourceGroupLoadBalancer" `
  -Location "EastUS" `
  -AllocationMethod "Static" `
  -Name "myPublicIP"
```

### <a name="create-a-load-balancer"></a>Criar um balanceador de carga
Crie um pool de IPs de front-end com [New-AzureRmLoadBalancerFrontendIpConfig](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig). O seguinte exemplo cria um pool de IPs de front-end chamado *myFrontEndPool* e anexa o endereço *myPublicIP*: 

```azurepowershell-interactive
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig `
  -Name "myFrontEndPool" `
  -PublicIpAddress $publicIP
```

Crie um pool de endereços de back-end com [New-AzureRmLoadBalancerBackendAddressPoolConfig](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig). As VMs são anexadas a este pool de back-end nas etapas restantes. O seguinte exemplo cria um pool de endereços de back-end chamado *myBackEndPool*:

```azurepowershell-interactive
$backendPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "myBackEndPool"
```

Agora, crie o balanceador de carga com [New-AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer). O exemplo a seguir cria um balanceador de carga denominado *myLoadBalancer* usando os pools de IP de front-end e back-end criados nas etapas anteriores:

```azurepowershell-interactive
$lb = New-AzureRmLoadBalancer `
  -ResourceGroupName "myResourceGroupLoadBalancer" `
  -Name "myLoadBalancer" `
  -Location "EastUS" `
  -FrontendIpConfiguration $frontendIP `
  -BackendAddressPool $backendPool
```

### <a name="create-a-health-probe"></a>Criar uma investigação de integridade
Para permitir que o balanceador de carga monitore o status de seu aplicativo, use uma investigação de integridade. A investigação de integridade adiciona ou remove dinamicamente VMs da rotação do balanceador de carga com base na resposta às verificações de integridade. Por padrão, uma VM é removida da distribuição do balanceador de carga após duas falhas consecutivas em intervalos de 15 segundos. Crie uma investigação de integridade com base em um protocolo ou página de verificação de integridade específica ao seu aplicativo. 

O exemplo a seguir cria uma investigação de TCP. Você também pode criar investigações de HTTP personalizadas para obter verificações de integridade mais refinadas. Ao usar uma investigação de HTTP personalizada, você deverá criar a página de verificação de integridade, como *healthcheck.aspx*. A investigação deve retornar a reposta **HTTP 200 OK** para o balanceador de carga a fim de manter o host em rotação.

Para criar uma investigação de integridade de TCP, use [Add-AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/add-azurermloadbalancerprobeconfig). O exemplo a seguir cria uma investigação de integridade chamada *myHealthProbe* que monitora cada VM na porta *TCP* *80*:

```azurepowershell-interactive
Add-AzureRmLoadBalancerProbeConfig `
  -Name "myHealthProbe" `
  -LoadBalancer $lb `
  -Protocol tcp `
  -Port 80 `
  -IntervalInSeconds 15 `
  -ProbeCount 2
```

Para aplicar a investigação de integridade, atualize o balanceador de carga com [Set-AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer):

```azurepowershell-interactive
Set-AzureRmLoadBalancer -LoadBalancer $lb
```

### <a name="create-a-load-balancer-rule"></a>Criar uma regra de balanceador de carga
Uma regra de balanceador de carga é usada para definir como o tráfego é distribuído para as VMs. Definir a configuração de IP de front-end para o tráfego de entrada e o pool de IP de back-end para receber o tráfego, junto com as portas de origem e de destino necessárias. Para ter certeza de que apenas VMs íntegras recebem tráfego, defina também a investigação de integridade a ser usada.

Crie uma regra de balanceador de carga com [Add-AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/add-azurermloadbalancerruleconfig). O seguinte exemplo cria uma regra de balanceador de carga chamada *myLoadBalancerRule* e faz o balanceamento de carga do tráfego na porta *TCP* *80*:

```azurepowershell-interactive
$probe = Get-AzureRmLoadBalancerProbeConfig -LoadBalancer $lb -Name "myHealthProbe"

Add-AzureRmLoadBalancerRuleConfig `
  -Name "myLoadBalancerRule" `
  -LoadBalancer $lb `
  -FrontendIpConfiguration $lb.FrontendIpConfigurations[0] `
  -BackendAddressPool $lb.BackendAddressPools[0] `
  -Protocol Tcp `
  -FrontendPort 80 `
  -BackendPort 80 `
  -Probe $probe
```

Atualize o balanceador de carga com [Set-AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer):

```azurepowershell-interactive
Set-AzureRmLoadBalancer -LoadBalancer $lb
```

## <a name="configure-virtual-network"></a>Configurar rede virtual
Antes de implantar algumas VMs e poder testar o balanceador, crie os recursos de suporte de rede virtual. Para saber mais sobre redes virtuais, veja o tutorial [Gerenciar Redes Virtuais do Azure](tutorial-virtual-network.md).

### <a name="create-network-resources"></a>Criar recursos da rede
Crie uma rede virtual com [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork). O exemplo a seguir cria uma rede virtual chamada *myVnet* com uma sub-rede chamada *mySubnet*:

```azurepowershell-interactive
# Create subnet config
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
  -Name "mySubnet" `
  -AddressPrefix 192.168.1.0/24

# Create the virtual network
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName "myResourceGroupLoadBalancer" `
  -Location "EastUS" `
  -Name "myVnet" `
  -AddressPrefix 192.168.0.0/16 `
  -Subnet $subnetConfig
```

As NICs virtuais são criadas com [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface). O exemplo a seguir cria três NICs virtuais. (Uma NIC virtual para cada VM criada para seu aplicativo nas etapas a seguir). Você pode criar VMs e NICs virtuais adicionais a qualquer momento e adicioná-las ao balanceador de carga:

```azurepowershell-interactive
for ($i=1; $i -le 3; $i++)
{
   New-AzureRmNetworkInterface `
     -ResourceGroupName "myResourceGroupLoadBalancer" `
     -Name myVM$i `
     -Location "EastUS" `
     -Subnet $vnet.Subnets[0] `
     -LoadBalancerBackendAddressPool $lb.BackendAddressPools[0]
}
```


## <a name="create-virtual-machines"></a>Criar máquinas virtuais
Para melhorar a alta disponibilidade do seu aplicativo, coloque suas VMs em um conjunto de disponibilidade.

Crie um conjunto de disponibilidade com [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset). O exemplo a seguir cria um conjunto de disponibilidade chamado *myAvailabilitySet*:

```azurepowershell-interactive
$availabilitySet = New-AzureRmAvailabilitySet `
  -ResourceGroupName "myResourceGroupLoadBalancer" `
  -Name "myAvailabilitySet" `
  -Location "EastUS" `
  -Sku aligned `
  -PlatformFaultDomainCount 2 `
  -PlatformUpdateDomainCount 2
```

Defina o nome de usuário e a senha de um administrador para as VMs com [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```azurepowershell-interactive
$cred = Get-Credential
```

Agora você pode criar VMs com [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). O exemplo a seguir cria três VMs e os componentes de rede virtual exigidos, se eles ainda não existirem:

```azurepowershell-interactive
for ($i=1; $i -le 3; $i++)
{
    New-AzureRmVm `
        -ResourceGroupName "myResourceGroupLoadBalancer" `
        -Name "myVM$i" `
        -Location "East US" `
        -VirtualNetworkName "myVnet" `
        -SubnetName "mySubnet" `
        -SecurityGroupName "myNetworkSecurityGroup" `
        -OpenPorts 80 `
        -AvailabilitySetName "myAvailabilitySet" `
        -Credential $cred `
        -AsJob
}
```

O parâmetro `-AsJob` cria a VM como uma tarefa em segundo plano, para que os prompts do PowerShell sejam exibidos de volta para você. Você pode exibir os detalhes de trabalhos em segundo plano com o cmdelt `Job`. Demora alguns minutos para criar e configurar todas as três VMs.


### <a name="install-iis-with-custom-script-extension"></a>Instalar o IIS com a extensão de script personalizado
Em um tutorial anterior sobre [Como personalizar uma máquina virtual do Windows](tutorial-automate-vm-deployment.md), você aprendeu a automatizar a personalização de VM com a Extensão do Script Personalizado para Windows. Você pode usar a mesma abordagem para instalar e configurar o IIS em suas VMs.

Use [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) para instalar a extensão de script personalizado. A extensão executa `powershell Add-WindowsFeature Web-Server` para instalar o servidor Web do IIS e, em seguida, atualiza a página *Default.htm* para mostrar o nome do host da VM:

```azurepowershell-interactive
for ($i=1; $i -le 3; $i++)
{
   Set-AzureRmVMExtension `
     -ResourceGroupName "myResourceGroupLoadBalancer" `
     -ExtensionName "IIS" `
     -VMName myVM$i `
     -Publisher Microsoft.Compute `
     -ExtensionType CustomScriptExtension `
     -TypeHandlerVersion 1.8 `
     -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
     -Location EastUS
}
```

## <a name="test-load-balancer"></a>Testar o balanceador de carga
Obtenha o endereço IP público do balanceador de carga com [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress). O exemplo a seguir obtém o endereço IP para *myPublicIP* criado anteriormente:

```azurepowershell-interactive
Get-AzureRmPublicIPAddress `
  -ResourceGroupName "myResourceGroupLoadBalancer" `
  -Name "myPublicIP" | select IpAddress
```

Você pode inserir o endereço IP público em um navegador da Web. O site é exibido, incluindo o nome do host da VM para a qual o balanceador de carga distribui o tráfego como no exemplo a seguir:

![Site do IIS em execução](./media/tutorial-load-balancer/running-iis-website.png)

Para ver o balanceador de carga distribuir tráfego entre todas as três VMs que executam seu aplicativo, você poderá forçar a atualização de seu navegador da Web.


## <a name="add-and-remove-vms"></a>Adicionar e remover as VMs
Talvez seja necessário fazer a manutenção nas VMs que executam seu aplicativo, como a instalação de atualizações do sistema operacional. Para lidar com o aumento de tráfego em seu aplicativo, talvez seja necessário adicionar outras VMs. Esta seção mostra como remover ou adicionar uma VM do balanceador de carga.

### <a name="remove-a-vm-from-the-load-balancer"></a>Remover uma VM do balanceador de carga
Obtenha a placa de interface de rede com [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface), em seguida, defina a propriedade *LoadBalancerBackendAddressPools* da NIC virtual como *$null*. Por fim, atualize a NIC virtual:

```azurepowershell-interactive
$nic = Get-AzureRmNetworkInterface `
    -ResourceGroupName "myResourceGroupLoadBalancer" `
    -Name "myVM2"
$nic.Ipconfigurations[0].LoadBalancerBackendAddressPools=$null
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

Para ver o balanceador de carga distribuir tráfego entre as duas VMs restantes que executam seu aplicativo, você poderá forçar a atualização de seu navegador da Web. Agora você pode executar a manutenção na VM, como instalação de atualizações do sistema operacional ou execução de uma reinicialização da VM.

### <a name="add-a-vm-to-the-load-balancer"></a>Adicionar uma VM ao balanceador de carga
Após executar a manutenção da máquina virtual ou se precisar expandir a capacidade, defina a propriedade *LoadBalancerBackendAddressPools* da NIC virtual para o *BackendAddressPool* de [Get-AzureRMLoadBalancer](/powershell/module/azurerm.network/get-azurermloadbalancer):

Obtenha o balanceador de carga:

```azurepowershell-interactive
$lb = Get-AzureRMLoadBalancer `
    -ResourceGroupName myResourceGroupLoadBalancer `
    -Name myLoadBalancer 
$nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$lb.BackendAddressPools[0]
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você criou um balanceador de carga e anexou VMs. Você aprendeu como:

> [!div class="checklist"]
> * Criar um balanceador de carga do Azure
> * Criar uma investigação de integridade do balanceador de carga
> * Criar regras de tráfego para o balanceador de carga
> * Usar a Extensão de Script Personalizado para criar um site do IIS básico
> * Criar máquinas virtuais e anexar a um balanceador de carga
> * Exibir um balanceador de carga em ação
> * Adicionar e remover as VMs de um balanceador de carga

Avance para o próximo tutorial para aprender a gerenciar a rede de VM.

> [!div class="nextstepaction"]
> [Gerencie VMs e redes virtuais](./tutorial-virtual-network.md)
