---
title: Início Rápido – Criar uma VM Windows com o Azure PowerShell | Microsoft Docs
description: Neste início rápido, você aprende a usar o Azure PowerShell para criar uma máquina virtual Windows
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 04/24/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: b09a85686e19ae92c3e437bedff54bff8371784f
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2018
---
# <a name="quickstart-create-a-windows-virtual-machine-with-the-azure-cli-20"></a>Início Rápido: Criar uma máquina virtual do Windows com a CLI 2.0 do Azure

A CLI do Azure 2.0 é usada para criar e gerenciar recursos do Azure da linha de comando ou em scripts. Este início rápido mostra como usar a CLI 2.0 do Azure para implantar uma VM (máquina virtual) no Azure que executa o Windows Server 2016. Para ver a VM em ação, você habilita o protocolo RDP na VM e instala o servidor Web do IIS.

Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Se você optar por instalar e usar a CLI localmente, este início rápido exigirá a execução da CLI do Azure versão 2.0.30 ou posterior. Execute `az --version` para encontrar a versão. Se você precisa instalar ou atualizar, consulte [Instalar a CLI 2.0 do Azure]( /cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Crie um grupo de recursos com o comando [az group create](/cli/azure/group#az_group_create). Um grupo de recursos do Azure é um contêiner lógico no qual os recursos do Azure são implantados e gerenciados. O exemplo a seguir cria um grupo de recursos chamado *myResourceGroup* na localização *eastus*:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

## <a name="create-virtual-machine"></a>Criar máquina virtual

Crie uma VM com [az vm create](/cli/azure/vm#az_vm_create). O exemplo a seguir cria uma VM chamada *myVM*. Este exemplo usa o *azureuser* para um nome de usuário administrativo e *myPassword12* como a senha. Atualize esses valores para algo apropriado para seu ambiente. Estes valores são necessários quando você se conectar à máquina virtual.

```azurecli-interactive
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image win2016datacenter \
    --admin-username azureuser \
    --admin-password myPassword12
```

A criação da VM e dos recursos suportados demora alguns minutos. O seguinte exemplo de saída mostra que a operação de criação de VM foi bem-sucedida.

```azurecli-interactive
{
  "fqdns": "",
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "52.174.34.95",
  "resourceGroup": "myResourceGroup"
}
```

Observe a sua própria `publicIpAddress` na saída da sua VM. Este endereço é usado para acessar a VM na próxima etapa.

## <a name="open-port-80-for-web-traffic"></a>Abra a porta 80 para tráfego da Web

Por padrão, somente conexões de RDP são abertas quando você criar uma VM do Windows no Azure. Use [az vm open-port](/cli/azure/vm#az_vm_open_port) para abrir a porta TCP 80 para uso com o servidor web IIS:

```azurecli-interactive
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```

## <a name="connect-to-virtual-machine"></a>Conectar-se à máquina virtual

Use o comando a seguir para criar uma sessão de área de trabalho remota no computador local. Substitua o endereço IP pelo endereço IP público da VM. Quando solicitado, insira as credenciais usadas quando a VM foi criada:

```powershell
mstsc /v:publicIpAddress
```

## <a name="install-web-server"></a>Instalar servidor Web

Para ver a VM em ação, instale o servidor Web do IIS. Abra um prompt do PowerShell na VM e execute o seguinte comando:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

Quando terminar, feche a conexão RDP com a VM.

## <a name="view-the-web-server-in-action"></a>Ver o servidor web em ação

Com o IIS instalado e a porta 80 agora aberta na VM pela Internet, use um navegador da Web de sua escolha para exibir a página inicial padrão do IIS. Use o endereço IP público da VM obtido em uma etapa anterior. O seguinte exemplo mostra o site padrão do IIS:

![Site do IIS padrão](./media/quick-create-powershell/default-iis-website.png)

## <a name="clean-up-resources"></a>Limpar recursos

Quando não for mais necessário, você pode usar o comando [az group delete](/cli/azure/group#az_group_delete) para remover o grupo de recursos, a VM e todos os recursos relacionados:

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Próximas etapas

Neste início rápido, você implantou uma máquina virtual simples, abriu uma porta de rede para o tráfego da Web e instalou um servidor Web básico. Para saber mais sobre máquinas virtuais do Azure, continue o tutorial para VMs do Windows.

> [!div class="nextstepaction"]
> [Tutoriais de máquina virtual do Windows Azure](./tutorial-manage-vm.md)
