---
title: Monitoramento de desempenho do Azure Service Fabric | Microsoft Docs
description: Saiba mais sobre os contadores de desempenho para o monitoramento e diagnóstico de clusters do Azure Service Fabric.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/16/2018
ms.author: srrengar
ms.openlocfilehash: 9e740dd3acce842f888e5994fe8f46222477adc1
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/16/2018
---
# <a name="performance-metrics"></a>Métricas de desempenho

As métricas devem ser coletadas para entender o desempenho de seu cluster, bem como os aplicativos em execução nele. Para clusters do Service Fabric, recomendamos coletar os seguintes contadores de desempenho.

## <a name="nodes"></a>Nós

Para as máquinas em seu cluster, considere a possibilidade de coletar os seguintes contadores de desempenho a fim de entender melhor a carga em cada computador e tomar decisões apropriadas sobre o dimensionamento do cluster.

| Categoria do Contador | Nome do contador |
| --- | --- |
| PhysicalDisk(per Disco) | Média Tamanho de Fila de Leitura de Disco |
| PhysicalDisk(per Disco) | Média Tamanho de Fila de Gravação de Disco |
| PhysicalDisk(per Disco) | Média de segundos/Leitura do Disco |
| PhysicalDisk(per Disco) | Média de segundos/Gravação do Disco |
| PhysicalDisk(per Disco) | Leituras de Disco/s  |
| PhysicalDisk(per Disco) | Bytes Lidos no Disco/s  |
| PhysicalDisk(per Disco) | Gravações de Disco/s |
| PhysicalDisk(per Disco) | Bytes Gravados no Disco/s |
| Memória | MBytes Disponíveis |
| PagingFile | % Uso |
| Processador(total) | % Tempo do Processador |
| Processo (por serviço) | % Tempo do Processador |
| Processo (por serviço) | ID do Processo |
| Processo (por serviço) | Bytes Particulares |
| Processo (por serviço) | Contagem de Threads |
| Processo (por serviço) | Bytes Virtuais |
| Processo (por serviço) | Conjunto de Trabalho |
| Processo (por serviço) | Conjunto de Trabalho - Particular |
| Interface de rede(todas as instâncias) | Tamanho da Fila de Saída |
| Interface de rede(todas as instâncias) | Pacotes de Saída Descartados |
| Interface de rede(todas as instâncias) | Pacotes Recebidos Descartados |
| Interface de rede(todas as instâncias) | Erros de Pacotes de Saída |
| Interface de rede(todas as instâncias) | Erros de Pacotes Recebidos |

## <a name="net-applications-and-services"></a>Aplicativos e serviços de .NET

Colete os contadores a seguir se você estiver implantando serviços .NET em seu cluster. 

| Categoria do Contador | Nome do contador |
| --- | --- |
| Memória CLR .NET (por serviço) | ID do Processo |
| Memória CLR .NET (por serviço) | Nº Total de Bytes confirmados |
| Memória CLR .NET (por serviço) | N º Total de Bytes reservados |
| Memória CLR .NET (por serviço) | N º de Bytes em todos os Heaps |
| Memória CLR .NET (por serviço) | Nº de Coleções Geração 0 |
| Memória CLR .NET (por serviço) | Nº de Coleções Geração 1 |
| Memória CLR .NET (por serviço) | Nº de Coleções Geração 2 |
| Memória CLR .NET (por serviço) | % de Tempo na GC |

### <a name="service-fabrics-custom-performance-counters"></a>Contadores de desempenho personalizados do Service Fabric

O Service Fabric gera uma quantidade significativa de contadores de desempenho personalizados. Se o SDK estiver instalado, você poderá ver a lista abrangente em seu computador com Windows em seu aplicativo de Monitor de Desempenho (Iniciar > Monitor de Desempenho). 

Nos aplicativos que você está implantando em seu cluster, se você estiver usando Reliable Actors, adicione contadores das categorias `Service Fabric Actor` e `Service Fabric Actor Method` (consulte [Diagnósticos de Reliable Actors do Service Fabric](service-fabric-reliable-actors-diagnostics.md)).

Se você usar os Reliable Services, teremos as categorias de contadores `Service Fabric Service` e `Service Fabric Service Method` das quais você deverá coletar contadores. 

Se você usar Coleções Confiáveis, recomendamos adicionar o `Avg. Transaction ms/Commit` do `Service Fabric Transactional Replicator` para coletar a latência média de confirmação por métrica da transação.


## <a name="next-steps"></a>Próximas etapas

* Saiba mais sobre a [geração de eventos no nível de plataforma](service-fabric-diagnostics-event-generation-infra.md) do Service Fabric
* Coletar métricas de desempenho por meio do [Agente do OMS](service-fabric-diagnostics-oms-agent.md)
