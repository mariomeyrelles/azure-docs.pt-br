---
title: Consulta de eventos de cluster usando as APIs EventStore em clusters do Azure Service Fabric | Microsoft Docs
description: Saiba como usar as APIs EventStore do Azure Service Fabric para consultar eventos de plataforma
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/25/2018
ms.author: dekapur
ms.openlocfilehash: 5c184841602f269555ce2196ef660faba14dbf8a
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/16/2018
---
# <a name="query-eventstore-apis-for-cluster-events"></a>Consultar APIs do EventStore para eventos de cluster

Este artigo mostra como consultar as APIs EventStore que estão disponíveis no Service Fabric versão 6.2 e posterior - se você quiser saber mais sobre o serviço EventStore, confira a [Visão geral do serviço EventStore](service-fabric-diagnostics-eventstore.md). No momento, o serviço EventStore só pode acessar dados para os últimos sete dias (isso se baseia na política de retenção de dados de diagnóstico do cluster).

>[!NOTE]
>A partir do Service Fabric 6.2. As APIs do EventStore estão atualmente em versão prévia para clusters do Windows somente em execução no Azure. Estamos trabalhando mover essa funcionalidade para Linux, assim como para nossos clusters independentes.

As APIs EventStore pode ser acessada diretamente por meio de um ponto de extremidade REST ou programaticamente. Dependendo da consulta, há vários parâmetros necessários para coletar os dados certos. Isso geralmente inclui:
* `api-version`: a versão das APIs EventStore que você está usando
* `StartTimeUtc`: define o início do período que você está interessado em examinar
* `EndTimeUtc`: final do período de tempo

Além dessas, há parâmetros opcionais disponíveis, como:
* `timeout`: substitua o tempo limite padrão de 60 segundos para executar a operação de solicitação
* `eventstypesfilter`: isso dá a você a opção para filtrar os tipos de evento específicos
* `ExcludeAnalysisEvents`: não retornam eventos do 'Analysis'. Por padrão, as consultas EventStore retornarão com eventos de "análise" onde possível - os eventos de análise são eventos de canal operacional mais avançados que contêm informações ou contexto adicional além de um evento normal do Service Fabric e fornecem mais detalhes.
* `SkipCorrelationLookup`: não procura por eventos correlacionados potenciais no cluster. Por padrão, o EventStore tentará correlacionar eventos em um cluster e vincular os eventos sempre que possível. 

As entidades em um cluster podem ser consultas de eventos. Você também pode consultar eventos para todas as entidades do tipo. Por exemplo, você pode consultar eventos de um nó específico ou de todos os nós no cluster. É o conjunto atual de entidades para o qual você pode consultar eventos (com como a consulta seria estruturada):
* Cluster: `/EventsStore/Cluster/Events`
* Nós: `/EventsStore/Nodes/Events`
* Nó: `/EventsStore/Nodes/<NodeName>/$/Events`
* Aplicativos: `/EventsStore/Applications/Events`
* Aplicativo: `/EventsStore/Applications/<AppName>/$/Events`
* Serviços: `/EventsStore/Services/Events`
* Serviço: `/EventsStore/Services/<ServiceName>/$/Events`
* Partições: `/EventsStore/Partitions/Events`
* Partição: `/EventsStore/Partitions/<PartitionID>/$/Events`
* Réplicas: `/EventsStore/Partitions/<PartitionID>/$/Replicas/Events`
* Réplica: `/EventsStore/Partitions/<PartitionID>/$/Replicas/<ReplicaID>/$/Events`

>[!NOTE]
>Ao fazer referência a um aplicativo ou nome do serviço, a consulta não precisará incluir o prefixo "fabric:/". Além disso, se os nomes do seu aplicativo ou serviço tiver um "/" neles, alterne para um "~" para manter a consulta funcionando. Por exemplo, se seu aplicativo aparecer como "fabric:/App1/FrontendApp", as consultas específicas de aplicativo seriam estruturadas como `/EventsStore/Applications/App1~FrontendApp/$/Events`.
>Além disso, os relatórios de integridade para serviços hoje são mostrados sob o aplicativo correspondente, portanto você consultaria eventos `DeployedServiceHealthReportCreated` para a entidade de aplicativo correta. 

## <a name="query-the-eventstore-via-rest-api-endpoints"></a>Consultar EventStore por meio de pontos de extremidade da API REST

Você pode consultar o EventStore diretamente por meio de um ponto de extremidade REST, fazendo solicitações `GET` para: `<your cluster address>/EventsStore/<entity>/Events/`.

Por exemplo, para consultar todos os eventos de Cluster entre `2018-04-03T18:00:00Z` e `2018-04-04T18:00:00Z`, sua solicitação seria semelhante a:

```
Method: GET 
URL: http://mycluster:19080/EventsStore/Cluster/Events?api-version=6.2-preview&StartTimeUtc=2018-04-03T18:00:00Z&EndTimeUtc=2018-04-04T18:00:00Z
```

Isso poderia retornar um erro se não houver eventos disponíveis ou, se a consulta for bem-sucedida, você verá eventos retornados em json:

```json
Response: 200
Body:
[
  {
    "Kind": "ClusterUpgradeStart",
    "CurrentClusterVersion": "0.0.0.0:",
    "TargetClusterVersion": "6.2:1.0",
    "UpgradeType": "Rolling",
    "RollingUpgradeMode": "UnmonitoredAuto",
    "FailureAction": "Manual",
    "EventInstanceId": "090add3c-8f56-4d35-8d57-a855745b6064",
    "TimeStamp": "2018-04-03T20:18:59.4313064Z",
    "HasCorrelatedEvents": false
  },
  {
    "Kind": "ClusterUpgradeDomainComplete",
    "TargetClusterVersion": "6.2:1.0",
    "UpgradeState": "RollingForward",
    "UpgradeDomains": "(0 1 2)",
    "UpgradeDomainElapsedTimeInMs": "78.5288",
    "EventInstanceId": "090add3c-8f56-4d35-8d57-a855745b6064",
    "TimeStamp": "2018-04-03T20:19:59.5729953Z",
    "HasCorrelatedEvents": false
  },
  {
    "Kind": "ClusterUpgradeDomainComplete",
    "TargetClusterVersion": "6.2:1.0",
    "UpgradeState": "RollingForward",
    "UpgradeDomains": "(3 4)",
    "UpgradeDomainElapsedTimeInMs": "0",
    "EventInstanceId": "090add3c-8f56-4d35-8d57-a855745b6064",
    "TimeStamp": "2018-04-03T20:20:59.6271949Z",
    "HasCorrelatedEvents": false
  },
  {
    "Kind": "ClusterUpgradeComplete",
    "TargetClusterVersion": "6.2:1.0",
    "OverallUpgradeElapsedTimeInMs": "120196.5212",
    "EventInstanceId": "090add3c-8f56-4d35-8d57-a855745b6064",
    "TimeStamp": "2018-04-03T20:20:59.8134457Z",
    "HasCorrelatedEvents": false
  }
]
```

Aqui, podemos ver que entre `2018-04-03T18:00:00Z` e `2018-04-04T18:00:00Z`, este cluster concluiu com êxito seu primeira upgrade quando ele foi posicionado pela primeira vez, de `"CurrentClusterVersion": "0.0.0.0:"` para `"TargetClusterVersion": "6.2:1.0"`, em `"OverallUpgradeElapsedTimeInMs": "120196.5212"`.

## <a name="query-the-eventstore-programmatically"></a>Consultar EventStore programaticamente

Você também pode consultar EventStore programaticamente, por meio da [biblioteca de cliente do Service Fabric](https://docs.microsoft.com/dotnet/api/overview/azure/service-fabric?view=azure-dotnet#client-library).

Assim que seu Cliente do Service Fabric for configurado, você poderá consultar eventos acessando o EventStore desta maneira: ` sfhttpClient.EventStore.<request>`

Aqui está uma solicitação de exemplo para todos os eventos do cluster `2018-04-03T18:00:00Z` e `2018-04-04T18:00:00Z`, por meio da função `GetClusterEventListAsync`.

```csharp
var sfhttpClient = ServiceFabricClientFactory.Create(clusterUrl, settings);

var clstrEvents = sfhttpClient.EventsStore.GetClusterEventListAsync(
    "2018-04-03T18:00:00Z",
    "2018-04-04T18:00:00Z")
    .GetAwaiter()
    .GetResult()
    .ToList();
```

## <a name="sample-scenarios-and-queries"></a>Cenários e consultas de exemplo

Aqui estão alguns exemplos sobre como você pode chamar as APIs REST EventStore para reconhecer o status do cluster.

*Upgrades de cluster:*

Para ver a última vez em que o upgrade do cluster foi bem-sucedido ou foi tentado na semana passada, você poderá consultar as APIs para os upgrades concluídos recentemente para seu cluster consultando os eventos "ClusterUpgradeComplete" no EventStore: `https://mycluster.cloudapp.azure.com:19080/EventsStore/Cluster/Events?api-version=6.2-preview&starttimeutc=2017-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z&EventsTypesFilter=ClusterUpgradeComplete`

*Problemas de upgrade de cluster:*

Da mesma forma, se houver problemas com uma atualização recente do cluster, você poderia consultar todos os eventos para a entidade de cluster. Você verá diversos eventos, incluindo o início de upgrades e cada UD para os quais o upgrade foi distribuído com êxito. Você também verá eventos para o ponto em que a reversão foi iniciada e os eventos de integridade correspondentes. Aqui está a consulta que você deseja usar para isso: `https://mycluster.cloudapp.azure.com:19080/EventsStore/Cluster/Events?api-version=6.2-preview&starttimeutc=2017-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z`

*Alterações de status de nó:*

Para ver as alterações do status do seu nó nos últimos dias - quando os nós são ativados ou desativados (pela plataforma, o serviço caos ou da entrada do usuário) - use a seguinte consulta: `https://mycluster.cloudapp.azure.com:19080/EventsStore/Nodes/Events?api-version=6.2-preview&starttimeutc=2017-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z`

*Eventos de aplicativo:*

Você também pode acompanhar suas implantações e upgrades recentes de aplicativos. Use a consulta a seguir para ver todos os aplicativos relacionados a eventos em seu cluster: `https://mycluster.cloudapp.azure.com:19080/EventsStore/Applications/Events?api-version=6.2-preview&starttimeutc=2017-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z`

*Integridade do histórico de um aplicativo:*

Além de ver apenas eventos de ciclo de vida do aplicativo, você talvez queira ver os dados históricos sobre a integridade de um aplicativo específico. Você pode fazer isso especificando o nome do aplicativo para o qual você deseja coletar os dados. Use esta consulta para obter todos os eventos de integridade do aplicativo: `https://mycluster.cloudapp.azure.com:19080/EventsStore/Applications/myApp/$/Events?api-version=6.2-preview&starttimeutc=2018-03-24T17:01:51Z&endtimeutc=2018-03-29T17:02:51Z&EventsTypesFilter=ProcessApplicationReport`. Se você deseja incluir eventos de integridade que possam ter expirado (o tempo de vida (TTL) passou), adicione `,ExpiredDeployedApplicationEvent` ao final da consulta para filtrar dois tipos de eventos.

*Integridade do histórico para todos os serviços em "myApp":*

Atualmente, os eventos de relatório de integridade para serviços aparecem como eventos do `DeployedServiceHealthReportCreated` sob a entidade correspondente do aplicativo. Para ver como seus serviços têm feito para "App1", use a seguinte consulta: `https://winlrc-staging-10.southcentralus.cloudapp.azure.com:19080/EventsStore/Applications/myapp/$/Events?api-version=6.2-preview&starttimeutc=2017-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z&EventsTypesFilter=DeployedServiceHealthReportCreated`

*Reconfiguração da partição:*

Para ver todos os movimentos de partição que ocorreram em seu cluster, consulte o evento `ReconfigurationCompleted`. Isso pode ajudá-lo a descobrir quais cargas de trabalho foram executadas em qual nó em horários específicos ao diagnosticar problemas no seu cluster. Aqui está um exemplo de consulta que faz isso: `https://mycluster.cloudapp.azure.com:19080/EventsStore/Partitions/Events?api-version=6.2-preview&starttimeutc=2018-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z&EventsTypesFilter=PartitionReconfigurationCompleted`

*Serviço Caos:*

Há um evento para quando o serviço Caos é iniciado ou interrompido que é exposto no nível do cluster. Para ver seu uso recente do serviço Caos, use a seguinte consulta: `https://mycluster.cloudapp.azure.com:19080/EventsStore/Cluster/Events?api-version=6.2-preview&starttimeutc=2017-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z&EventsTypesFilter=ChaosStarted,ChaosStopped`

