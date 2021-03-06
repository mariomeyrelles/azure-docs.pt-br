---
title: Introdução ao log de fluxo dos grupos de segurança da rede com o Observador de Rede do Azure | Microsoft Docs
description: Este artigo explica como usar o recurso dos logs de fluxo NSG do Observador de Rede do Azure.
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: 47d91341-16f1-45ac-85a5-e5a640f5d59e
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: c6a24fbca37d6aa1d775a70c708a139dfb70b813
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2018
ms.locfileid: "32182418"
---
# <a name="introduction-to-flow-logging-for-network-security-groups"></a>Introdução ao log de fluxo dos grupos de segurança da rede

Logs de fluxo do grupo de segurança de rede (NSG) são um recurso do Observador de Rede permite que você exiba informações sobre o tráfego IP de entrada e saída por meio de um NSG. Os logs de fluxo são escritos no formato json e mostram os fluxos de entrada e de saída por regra, a interface de rede (NIC) à qual o fluxo se aplica, as informações de cinco tuplas sobre o fluxo (IP de origem/destino, porta de origem/destino, e protocolo) e se o tráfego foi permitido ou negado.

![visão geral dos logs de fluxo](./media/network-watcher-nsg-flow-logging-overview/figure1.png)

Embora os logs de fluxo sejam destinados aos NSGs, eles não são exibidos como os outros logs. Os logs de fluxo são armazenados apenas em uma conta de armazenamento e seguem o caminho do log mostrado no exemplo a seguir:

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId=/SUBSCRIPTIONS/{subscriptionID}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/{nsgName}/y={year}/m={month}/d={day}/h={hour}/m=00/macAddress={macAddress}/PT1H.json
```

As mesmas políticas de retenção vistas para outros logs aplicam-se aos logs de fluxo. Você pode definir a política de retenção de log de 1 dia, 365 dias. Se uma política de retenção não for definida, os logs serão mantidos para sempre.

## <a name="log-file"></a>Arquivo de log

Logs de fluxo incluem as seguintes propriedades:

* **time** - a hora em que o evento foi registrado
* **systemId** - a ID do recurso Grupo de Segurança da Rede.
* **category** - a categoria do evento. A categoria é sempre **NetworkSecurityGroupFlowEvent**
* **resourceid** - a ID do recurso do NSG
* **operationName** - é sempre NetworkSecurityGroupFlowEvents
* **properties** - uma coleção de propriedades do fluxo
    * **Version** - o número de versão do esquema do evento Log de Fluxo
    * **flows** - uma coleção de fluxos. Essa propriedade tem várias entradas para diferentes regras
        * **rule** - a regra para a qual os fluxos são listados
            * **flows** - uma coleção de fluxos
                * **mac** - o endereço MAC da NIC para a VM na qual o fluxo foi coletado
                * **flowTuples** - uma cadeia de caracteres que contém várias propriedades para a tupla de fluxo no formato separado por vírgulas
                    * **Time Stamp** - esse valor é o carimbo de hora de quando ocorreu o fluxo no formato UNIX EPOCH
                    * **Source IP** - o IP de origem
                    * **Destination IP** - o IP de destino
                    * **Source Port** - a porta de origem
                    * **Destination Port** - a porta de destino
                    * **Protocol** - o protocolo do fluxo. Os valores válidos são **T** para TCP e **U** para UDP
                    * **Traffic Flow** - a direção do fluxo do tráfego. Os valores válidos são **I** para entrada e **O** para saída.
                    * **Traffic** - se o tráfego foi permitido ou negado. Os valores válidos são **A** para permitido e **D** para negado.

O texto que segue é um exemplo de um log de fluxo. Como você pode ver, há vários registros que seguem a lista de propriedades descrita na seção anterior.

> [!NOTE]
> Os valores na propriedade **flowTuples* são uma lista separada por vírgulas.
 
```json
{
    "records":
    [
        
        {
             "time": "2017-02-16T22:00:32.8950000Z",
             "systemId": "2c002c16-72f3-4dc5-b391-3444c3527434",
             "category": "NetworkSecurityGroupFlowEvent",
             "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG",
             "operationName": "NetworkSecurityGroupFlowEvents",
             "properties": {"Version":1,"flows":[{"rule":"DefaultRule_DenyAllInBound","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282421,42.119.146.95,10.1.0.4,51529,5358,T,I,D"]}]},{"rule":"UserRule_default-allow-rdp","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282370,163.28.66.17,10.1.0.4,61771,3389,T,I,A","1487282393,5.39.218.34,10.1.0.4,58596,3389,T,I,A","1487282393,91.224.160.154,10.1.0.4,61540,3389,T,I,A","1487282423,13.76.89.229,10.1.0.4,53163,3389,T,I,A"]}]}]}
        }
        ,
        {
             "time": "2017-02-16T22:01:32.8960000Z",
             "systemId": "2c002c16-72f3-4dc5-b391-3444c3527434",
             "category": "NetworkSecurityGroupFlowEvent",
             "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG",
             "operationName": "NetworkSecurityGroupFlowEvents",
             "properties": {"Version":1,"flows":[{"rule":"DefaultRule_DenyAllInBound","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282481,195.78.210.194,10.1.0.4,53,1732,U,I,D"]}]},{"rule":"UserRule_default-allow-rdp","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282435,61.129.251.68,10.1.0.4,57776,3389,T,I,A","1487282454,84.25.174.170,10.1.0.4,59085,3389,T,I,A","1487282477,77.68.9.50,10.1.0.4,65078,3389,T,I,A"]}]}]}
        }
        ,
        {
             "time": "2017-02-16T22:02:32.9040000Z",
             "systemId": "2c002c16-72f3-4dc5-b391-3444c3527434",
             "category": "NetworkSecurityGroupFlowEvent",
             "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG",
             "operationName": "NetworkSecurityGroupFlowEvents",
             "properties": {"Version":1,"flows":[{"rule":"DefaultRule_DenyAllInBound","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282492,175.182.69.29,10.1.0.4,28918,5358,T,I,D","1487282505,71.6.216.55,10.1.0.4,8080,8080,T,I,D"]}]},{"rule":"UserRule_default-allow-rdp","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282512,91.224.160.154,10.1.0.4,59046,3389,T,I,A"]}]}]}
        }
        ,
        ...
```

## <a name="next-steps"></a>Próximas etapas

- Para saber como habilitar os logs de fluxo, consulte [Habilitar o log de fluxo NSG](network-watcher-nsg-flow-logging-portal.md).
- Para saber mais sobre o log de NSG, consulte [Log Analytics para os grupos de segurança da rede (NSGs)](../virtual-network/virtual-network-nsg-manage-log.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).
- Para determinar se o tráfego é permitido ou negado para ou de uma VM, consulte [Diagnosticar um problema de filtro de tráfego de rede VM](diagnose-vm-network-traffic-filtering-problem.md)