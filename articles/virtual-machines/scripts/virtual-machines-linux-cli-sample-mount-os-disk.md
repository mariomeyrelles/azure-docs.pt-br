---
title: Exemplo de Script CLI do Azure - montar o disco do sistema operacional | Microsoft Docs
description: Exemplo de Script CLI do Azure - montar o disco do sistema operacional
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/27/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 7b9f1624426c7f401756310cd4fbe2789c29999d
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/09/2018
---
# <a name="troubleshoot-a-vms-operating-system-disk"></a>Solucionar problemas de um disco de sistema operacional de VMs

Esse script monta o disco do sistema operacional de uma máquina virtual com falha ou um problema como um disco de dados para uma segunda máquina virtual. Isso pode ser útil ao solucionar problemas de disco problemas ou recuperação de dados. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script de exemplo

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/mount-os-disk/mount-os-disk.sh "Quick Create VM")]

## <a name="script-explanation"></a>Explicação sobre o script

Este script usa os comandos a seguir para criar um grupo de recursos, uma máquina virtual e todos os recursos relacionados. Cada comando na tabela redireciona para a documentação específica do comando.

| Get-Help | Observações |
|---|---|
| [az vm show](https://docs.microsoft.com/cli/azure/vm#az_vm_show) | Retorne a lista de máquinas virtuais. Nesse caso, a opção de consulta é usada para retornar o disco de sistema operacional da máquina virtual. Este valor é adicionado a um nome de variável 'uri'. |
| [az vm delete](https://docs.microsoft.com/cli/azure/vm#az_vm_delete) | Exclui uma máquina virtual. |
| [az vm create](https://docs.microsoft.com/cli/azure/vm#az_vm_create) | Cria uma máquina virtual.  |
| [az vm disk attach](https://docs.microsoft.com/cli/azure/vm/disk#az_vm_disk_attach) | Anexa um disco a uma máquina virtual. |
| [az vm list-ip-addresses](https://docs.microsoft.com/cli/azure/vm#az_vm_list_ip_addresses) | Retorna os endereços IP de uma máquina virtual. |

## <a name="next-steps"></a>Próximas etapas

Para saber mais sobre a CLI do Azure, veja a [documentação da CLI do Azure](https://docs.microsoft.com/cli/azure).

Os exemplos de script da CLI de máquina virtual adicionais podem ser encontrados na [documentação da VM Linux do Azure](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
