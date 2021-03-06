---
title: Extensão de Diagnóstico do Azure para Windows | Microsoft Docs
description: Monitorar VMs do Windows no Azure usando a extensão de Diagnóstico do Azure para Windows
services: virtual-machines-windows
documentationcenter: ''
author: johnkemnetz
manager: ashwink
editor: ''
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 04/06/2018
ms.author: johnkem
ms.openlocfilehash: a9621f6f93d8e14e53cfe05ca4a714e88b9d8572
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
# <a name="windows-azure-diagnostics-extension"></a>Extensão de Diagnóstico do Azure

## <a name="overview"></a>Visão geral

A extensão de VM de Diagnóstico do Azure para Windows permite coletar dados de monitoramento, como contadores de desempenho e logs de eventos, da VM do Windows. Você pode especificar gradualmente quais dados deseja coletar e onde você deseja que os dados vão, como uma conta de Armazenamento do Azure ou um Hub de Eventos do Azure. Você também pode usar esses dados para criar gráficos no Portal do Azure ou criar alertas de métrica.

## <a name="prerequisites"></a>pré-requisitos

### <a name="operating-system"></a>Sistema operacional

A Extensão de Diagnóstico do Azure para Windows pode ser executada no Windows 10 Client e Windows Server 2008 R2, 2012, 2012 R2 e 2016.

### <a name="internet-connectivity"></a>Conectividade com a Internet

A Extensão de Diagnóstico do Azure para Windows requer que a máquina virtual de destino esteja conectada à Internet. 

## <a name="extension-schema"></a>Esquema de extensão

[Os valores de propriedade e o esquema da Extensão de Diagnóstico do Azure para Windows são descritos neste documento.](../../monitoring-and-diagnostics/azure-diagnostics-schema-1dot3-and-later.md)

## <a name="template-deployment"></a>Implantação de modelo

Extensões de VM do Azure podem ser implantadas com modelos do Azure Resource Manager. O esquema JSON detalhado na seção anterior pode ser usado em um modelo do Azure Resource Manager para executar a extensão de Diagnóstico do Azure para Windows durante uma implantação de modelo do Azure Resource Manager. Consulte [Usar monitoramento e diagnóstico com uma VM Windows e modelos do Azure Resource Manager](extensions-diagnostics-template.md).

## <a name="azure-cli-deployment"></a>Implantação da CLI do Azure

A CLI do Azure pode ser usado para implantar a extensão de Diagnóstico do Azure para Windows para uma máquina virtual existente. Substitua as configurações protegidas e propriedades de configurações por JSON válido do esquema da extensão acima. 

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name IaaSDiagnostics \
  --publisher Microsoft.Azure.Diagnostics \
  --version 1.9.0.0 --protected-settings protected-settings.json \
  --settings public-settings.json 
```

## <a name="powershell-deployment"></a>Implantação do PowerShell

O comando `Set-AzureRmVMDiagnosticsExtension` pode ser usado para adicionar a extensão de Diagnóstico do Azure para Windows para uma máquina virtual existente. Consulte também [Usar o PowerShell para habilitar o Diagnóstico do Azure em uma máquina virtual que executa o Windows](ps-extensions-diagnostics.md).
```powershell
$vm_resourcegroup = "myvmresourcegroup"
$vm_name = "myvm"
$diagnosticsconfig_path = "DiagnosticsPubConfig.xml"

Set-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup `
  -VMName $vm_name `
  -DiagnosticsConfigurationPath $diagnosticsconfig_path
```

## <a name="troubleshoot-and-support"></a>Solução de problemas e suporte

### <a name="troubleshoot"></a>Solucionar problemas

Dados sobre o estado das implantações de extensão podem ser recuperados do Portal do Azure usando a CLI do Azure. Para ver o estado da implantação das extensões de uma determinada VM, execute o comando a seguir usando a CLI do Azure.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

[Consulte este artigo](../../monitoring-and-diagnostics/azure-diagnostics-troubleshooting.md) para obter um guia de solução de problemas mais abrangente para a extensão de Diagnóstico do Azure para Windows.

### <a name="support"></a>Suporte

Caso precise de mais ajuda em qualquer ponto deste artigo, entre em contato com os especialistas do Azure nos [fóruns do Azure e do Stack Overflow no MSDN](https://azure.microsoft.com/support/forums/). Como alternativa, você pode registrar um incidente de suporte do Azure. Vá para o [site de suporte do Azure](https://azure.microsoft.com/support/options/) e selecione Obter suporte. Para saber mais sobre como usar o suporte do Azure, leia as [Perguntas frequentes sobre o suporte do Microsoft Azure](https://azure.microsoft.com/support/faq/).

## <a name="next-steps"></a>Próximas etapas
* [Saiba mais sobre a Extensão de Diagnóstico do Azure para Windows](../../monitoring-and-diagnostics/azure-diagnostics.md)
* [Examine o esquema e as versões de extensão](../../monitoring-and-diagnostics/azure-diagnostics-schema.md)
