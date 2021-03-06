---
title: Benefício do Híbrido do Azure para Windows Server | Microsoft Docs
description: Saiba como maximizar os benefícios do Windows Software Assurance para colocar as licenças locais no Azure
services: virtual-machines-windows
documentationcenter: ''
author: kmouss
manager: jeconnoc
editor: ''
ms.assetid: 332583b6-15a3-4efb-80c3-9082587828b0
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 11/22/2017
ms.author: kmouss
ms.openlocfilehash: bfad3ff71be07026cf4a7dd6ad75df399349f844
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2018
---
# <a name="azure-hybrid-benefit-for-windows-server"></a>Benefício Híbrido do Azure para Windows Server
Para clientes com o Software Assurance, o Benefício Híbrido do Azure para Windows Server permite usar as licenças locais do Windows Server e executar máquinas virtuais do Windows no Azure por um custo reduzido. Você pode usar o Benefício Híbrido do Azure para Windows Server para implantar novas máquinas virtuais de qualquer plataforma com suporte do Azure, imagem do Windows Server ou imagens personalizadas do Windows. Este artigo percorre as etapas sobre como implantar novas VMs com o Benefício Híbrido do Azure para Windows Server e como você pode atualizar VMs existentes e em execução. Para saber mais sobre licenciamento e economia de custo do Benefício Híbrido do Azure para Windows Server, consulte a [página de licenciamento do Benefício Híbrido do Azure para Windows Server](https://azure.microsoft.com/pricing/hybrid-use-benefit/).

> [!IMPORTANT]
> As imagens herdadas '[HUB]' do Windows Server que foram publicadas para clientes com o Contrato Enterprise no Azure Marketplace foram desativadas a partir de 11/9/2017. Use o Windows Server padrão com a opção "Economize Dinheiro" no Portal para usar o Benefício Híbrido do Azure para Windows Server. Para obter mais informações, consulte este [artigo.](https://support.microsoft.com/en-us/help/4036360/retirement-azure-hybrid-use-benefit-images-for-ea-subscriptions)
>

> [!NOTE]
> Está sendo implementado o uso do Benefício Híbrido do Azure para Windows Server com VMs que são cobradas por software adicional, como SQL Server ou qualquer uma das imagens de terceiros no marketplace. Se receber um erro 409 do tipo: “Não é permitido alterar a propriedade 'LicenseType'”, você está tentando converter ou implantar uma nova VM do Windows Server que tem custo adicional por software, o que pode não ter suporte nessa região. Mesmo se você tentar procurar a opção de configuração do portal para fazer a conversão e não puder vê-la para a VM.
>


> [!NOTE]
> Para VMs clássicas, há suporte apenas para a implantação de uma nova VM a partir de imagens personalizadas locais. Para aproveitar os recursos com suporte neste artigo, você deve primeiro migrar as VMs clássicas para o modelo do Resource Manager.
>


## <a name="ways-to-use-azure-hybrid-benefit-for-windows-server"></a>Formas de usar o Benefício Híbrido do Azure para Windows Server
Há algumas maneiras de usar as máquinas virtuais do Windows com o Benefício Híbrido do Azure:

1. Você pode implantar VMs de uma das [imagens do Windows Server fornecidos no Azure Marketplace](#https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.WindowsServer?tab=Overview)
2. Você pode [carregar uma VM personalizada](#upload-a-windows-vhd) e [implantar usando um modelo do Resource Manager](#deploy-a-vm-via-resource-manager) ou [do Azure PowerShell](#detailed-powershell-deployment-walkthrough)
3. Você pode alternar e converter uma VM existente entre executar com o Benefício Híbrido do Azure ou pagar o custo sob demanda para Windows Server
4. Você também pode implantar um novo conjunto de dimensionamento de máquinas virtuais com o Benefício Híbrido do Azure para Windows Server

> [!NOTE]
> Não há suporte para a conversão de um conjunto de dimensionamento de máquinas virtuais para usar o Benefício Híbrido do Azure para Windows Server
>

## <a name="deploy-a-vm-from-a-windows-server-marketplace-image"></a>Implantar uma VM de uma imagem do Windows Server Marketplace
Todas as imagens do Windows Server que estão disponíveis no Azure Marketplace são habilitadas com o Benefício Híbrido do Azure para Windows Server. Por exemplo, o Windows Server 2016, o Windows Server 2012 R2, o Windows Server 2012 e o Windows Server 2008 SP1 e muito mais. Use essas imagens para implantar VMs diretamente do Portal do Azure, de modelos do Resource Manager, do Azure PowerShell ou de outros SDKs.

É possível implantar essas imagens diretamente por meio do portal do Azure. Para uso em modelos do Resource Manager e com o Azure PowerShell, exiba a lista de imagens da seguinte maneira:

### <a name="powershell"></a>Powershell
```powershell
Get-AzureRmVMImagesku -Location westus -PublisherName MicrosoftWindowsServer -Offer WindowsServer
```
Você pode executar as etapas para [Criar uma máquina virtual do Windows com o PowerShell](#https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-powershell?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Ftoc.json) e passar LicenseType = "Windows_Server". Essa opção permite que você use sua licença existente do Windows Server no Azure.

### <a name="portal"></a>Portal
Você pode executar as etapas para [Criar uma máquina virtual do Windows com o Portal do Azure](#https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal) e selecionar a opção para usar sua licença existente do Windows Server.

## <a name="convert-an-existing-vm-using-azure-hybrid-benefit-for-windows-server"></a>Converter uma VM existente usando o Benefício Híbrido do Azure para Windows Server
Se houver uma VM existente que você deseja converter para tirar proveito do Benefício Híbrido do Azure para Windows Server, é possível atualizar o tipo de licença da VM da seguinte maneira:

### <a name="convert-to-using-azure-hybrid-benefit-for-windows-server"></a>Converter para usar o Benefício Híbrido do Azure para Windows Server
```powershell
$vm = Get-AzureRmVM -ResourceGroup "rg-name" -Name "vm-name"
$vm.LicenseType = "Windows_Server"
Update-AzureRmVM -ResourceGroupName rg-name -VM $vm
```

### <a name="convert-back-to-pay-as-you-go"></a>Reverter conversão para pré-pago
```powershell
$vm = Get-AzureRmVM -ResourceGroup "rg-name" -Name "vm-name"
$vm.LicenseType = "None"
Update-AzureRmVM -ResourceGroupName rg-name -VM $vm
```

### <a name="portal"></a>Portal
Na folha VM do portal, você pode atualizar a VM para usar o Benefício Híbrido do Azure selecionando a opção de “Configuração” e alternar a opção “Benefício Híbrido do Azure”

> [!NOTE]
> Se você não visualizar a opção para alternar "Benefício híbrido do Azure" em "Configuração", é porque a conversão ainda não tem suporte pelo tipo da VM selecionada (por exemplo, uma VM compilada a partir de imagem personalizada ou de uma imagem que tenha software pago adicional, como software de terceiros do Microsoft Azure Marketplace ou SQL Server).
>

## <a name="upload-a-windows-server-vhd"></a>Carregar um VHD do Windows Server
Para implantar uma VM do Windows Server no Azure, primeiro você precisa criar um VHD que contém a compilação base do Windows. Esse VHD deve estar preparado adequadamente por meio do Sysprep antes de carregá-lo no Azure. Você pode [ler mais sobre os requisitos de VHD e o processo Sysprep](upload-generalized-managed.md) e [Suporte do Sysprep para funções de servidor](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles). Faça backup da VM antes de executar o Sysprep. 

Depois de preparar o VHD, carregue-o em sua conta de Armazenamento do Azure da seguinte maneira:

```powershell
Add-AzureRmVhd -ResourceGroupName "myResourceGroup" -LocalFilePath "C:\Path\To\myvhd.vhd" `
    -Destination "https://mystorageaccount.blob.core.windows.net/vhds/myvhd.vhd"
```

> [!NOTE]
> O Microsoft SQL Server, SharePoint Server e Dynamics também podem utilizar o licenciamento Software Assurance. Você precisa preparar a imagem do Windows Server instalando os componentes do aplicativo e fornecendo as chaves de licença corretamente e, então, carregando a imagem do disco no Azure. Examine a documentação apropriada para executar o Sysprep com seu aplicativo, como [Considerações para Instalar o SQL Server usando o Sysprep](https://msdn.microsoft.com/library/ee210754.aspx) ou [Compilar uma Imagem de Referência do SharePoint Server 2016 (Sysprep)](http://social.technet.microsoft.com/wiki/contents/articles/33789.build-a-sharepoint-server-2016-reference-image-sysprep.aspx).
>
>

Você também pode ler mais sobre como [carregar o VHD no processo do Azure](upload-generalized-managed.md#upload-the-vhd-to-your-storage-account)

## <a name="deploy-a-vm-via-resource-manager-template"></a>Implantar uma VM por meio de um modelo do Resource Manager
Nos modelos do Resource Manager, um parâmetro adicional para `licenseType` deve ser especificado. Você pode ler mais sobre a [criação de modelos do Azure Resource Manager](../../resource-group-authoring-templates.md). Quando o VHD for carregado no Azure, edite o modelo do Resource Manager para incluir o tipo de licença como parte do provedor de computação e implantar o modelo como normal:

```json
"properties": {  
   "licenseType": "Windows_Server",
   "hardwareProfile": {
        "vmSize": "[variables('vmSize')]"
   }
```

## <a name="deploy-a-vm-via-powershell-quickstart"></a>Implantar uma VM por meio do início rápido do PowerShell
Ao implantar a VM do Windows Server por meio do PowerShell, você tem um parâmetro adicional `-LicenseType`. Depois de carregar o VHD no Azure, você cria uma VM usando `New-AzureRmVM` e especifica o tipo de licenciamento da seguinte maneira:

Para Windows Server:
```powershell
New-AzureRmVM -ResourceGroupName "myResourceGroup" -Location "West US" -VM $vm -LicenseType "Windows_Server"
```

Leia um guia mais descritivo sobre as diferentes etapas para [criar uma VM do Windows usando o Resource Manager e o PowerShell](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="verify-your-vm-is-utilizing-the-licensing-benefit"></a>Verifique se que sua VM está utilizando o benefício de licenciamento
Depois de implantar sua VM por meio do PowerShell, modelo do Resource Manager ou Portal, verifique o tipo de licença com `Get-AzureRmVM`, da seguinte maneira:

```powershell
Get-AzureRmVM -ResourceGroup "myResourceGroup" -Name "myVM"
```

A saída é semelhante ao seguinte exemplo do Windows Server:

```powershell
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              : Windows_Server
```

Esta saída contrasta com a seguinte VM implantada sem licenciamento do Benefício Híbrido do Azure para Windows Server:

```powershell
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              :
```

## <a name="list-all-azure-hybrid-benefit-for-windows-server-vms-in-a-subscription"></a>Listar todas as VMs do Benefício Híbrido do Azure para Windows Server em uma assinatura

Para ver e contar todas as máquinas virtuais implantadas com o Benefício Híbrido do Azure para Windows Server, execute o comando a seguir em sua assinatura:

```powershell
$vms = Get-AzureRMVM 
foreach ($vm in $vms) {"VM Name: " + $vm.Name, "   Azure Hybrid Benefit for Windows Server: "+ $vm.LicenseType}
```

## <a name="deploy-a-virtual-machine-scale-set-with-azure-hybrid-benefit-for-windows-server"></a>Implantar um conjunto de dimensionamento de máquinas virtuais com o Benefício Híbrido do Azure para Windows Server
Nos modelos do Resource Manager de seu conjunto de dimensionamento de máquinas virtuais, um parâmetro adicional `licenseType` deve ser especificado. Você pode ler mais sobre a [criação de modelos do Azure Resource Manager](../../resource-group-authoring-templates.md). Edite seu modelo do Resource Manager para incluir a propriedade licenseType como parte do virtualMachineProfile de seu conjunto de dimensionamento e implantar o modelo como normal – consulte o exemplo a seguir usando a imagem do Windows Server 2016:


```json
"virtualMachineProfile": {
    "storageProfile": {
        "osDisk": {
            "createOption": "FromImage"
        },
        "imageReference": {
            "publisher": "MicrosoftWindowsServer",
            "offer": "WindowsServer",
            "sku": "2016-Datacenter",
            "version": "latest"
        }
    },
    "licenseType": "Windows_Server",
    "osProfile": {
            "computerNamePrefix": "[parameters('vmssName')]",
            "adminUsername": "[parameters('adminUsername')]",
            "adminPassword": "[parameters('adminPassword')]"
    }
```
Você também pode [Criar e implantar um conjunto de dimensionamento de máquinas virtuais](#https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-create) e definir a propriedade LicenseType

## <a name="next-steps"></a>Próximas etapas
Leia mais sobre [Como economizar dinheiro com o Benefício Híbrido do Azure](https://azure.microsoft.com/pricing/hybrid-use-benefit/)

Leia mais sobre [Orientação detalhada sobre licenciamento do Benefício Híbrido do Azure para Windows Server](https://docs.microsoft.com/windows-server/get-started/azure-hybrid-benefit)

Saiba mais sobre como [Usar os modelos do Resource Manager](../../azure-resource-manager/resource-group-overview.md)

Saiba mais sobre como o [Benefício Híbrido do Azure para Windows Server e o Azure Site Recovery tornam a migração de aplicativos para o Azure ainda mais econômica](https://azure.microsoft.com/blog/hybrid-use-benefit-migration-with-asr/)

Saiba mais sobre o [Windows 10 no Azure com Direitos de Hospedagem Multilocatário](https://docs.microsoft.com/azure/virtual-machines/windows/windows-desktop-multitenant-hosting-deployment)

Leia sobre as [Perguntas frequentes](#https://azure.microsoft.com/pricing/hybrid-use-benefit/faq/)
