---
title: Monitoramento de nível de aplicativo do Service Fabric do Azure | Microsoft Docs
description: Saiba mais sobre eventos e logs de nível de aplicativo e serviço usados para monitorar e diagnosticar clusters do Service Fabric do Azure.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/20/2018
ms.author: dekapur
ms.openlocfilehash: b8118d83e2be452c6aa5bbc8b7a3c220d26903a1
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/16/2018
---
# <a name="application-and-service-level-logging"></a>Registro em log em nível de aplicativo e de serviço

Instrumentar o código é a base para a maioria dos outros aspectos do monitoramento de seus serviços. A instrumentação é a única maneira que você possa saber que algo está errado e para diagnosticar o que precisa ser corrigido. Embora tecnicamente é possível conectar um depurador a um serviço de produção, ele não é uma prática comum. Portanto, é importante ter dados de instrumentação detalhados.

Alguns produtos instrumentam automaticamente seu código. Embora essas soluções podem funcionar bem, quase sempre a instrumentação manual é necessária. No fim das contas, você deve ter informações suficientes para depurar legalmente o aplicativo. Este documento descreve as diferentes abordagens para instrumentar seu código quando for necessário escolher uma abordagem em vez de outra.

Para exemplos de como usar essas sugestões, consulte [Adicionar registro em log ao aplicativo do Service Fabric](service-fabric-how-to-diagnostics-log.md).

## <a name="eventsource"></a>EventSource

Quando você cria uma solução do Service Fabric de um modelo do Visual Studio, uma classe derivada de **EventSource** (**ServiceEventSource** ou **ActorEventSource**) é gerada. Um modelo é criado, no qual você pode adicionar eventos para seu aplicativo ou serviço. O nome **EventSource** **deve** ser exclusivo e deve ser renomeado na cadeia de caracteres do modelo padrão MyCompany-&lt;solution&gt;-&lt;project&gt;. Ter várias definições de **EventSource** que usam o mesmo nome causa um problema no tempo de execução. Cada evento definido deve ter um identificador exclusivo. Se um identificador não for exclusivo, ocorrerá uma falha de tempo de execução. Algumas organizações atribuem antecipadamente intervalos de valores para evitar conflitos entre equipes de desenvolvimento separadas. Para saber mais, veja [blog do Vance](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/) ou a [documentação do MSDN](https://msdn.microsoft.com/library/dn774985(v=pandp.20).aspx).

## <a name="aspnet-core-logging"></a>Registro em log de ASP.NET Core

É importante planejar cuidadosamente como você instrumentará o seu código. O plano de instrumentação certa pode ajudar a evitar potencialmente desestabilizar sua base de código e, em seguida, precisar reinstrument o código. Para reduzir o risco, você pode escolher uma biblioteca de instrumentação como [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/), que faz parte do Microsoft ASP.NET Core. Núcleo do ASP.NET tem um [ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) interface que você pode usar com o provedor de sua escolha, minimizando o efeito no código existente. Você pode usar o código ASP.NET Core no Windows e Linux e no .NET Framework completo, então o código de instrumentação é padronizado.

## <a name="application-insights-sdk"></a>SDK do Application Insights

O Application Insights tem uma integração avançada com o Service Fabric pronta para uso. Os usuários podem adicionar os pacotes NuGet do Service Fabric do IA e receber dados e logs criados e coletados visíveis no Portal do Azure. Além disso, os usuários são incentivados a adicionar sua própria telemetria para diagnosticar e depurar os aplicativos e rastrear quais serviços e partes do aplicativo são mais utilizados. A classe [TelemetryClient](https://docs.microsoft.com/dotnet/api/microsoft.applicationinsights.telemetryclient?view=azure-dotnet) na SDK fornece muitas maneiras de rastrear a telemetria nos aplicativos. Confira um exemplo de como instrumentar e adicionar Application Insights ao aplicativo no tutorial [monitorar e diagnosticar um aplicativo .NET](service-fabric-tutorial-monitoring-aspnet.md)


## <a name="next-steps"></a>Próximas etapas

Após escolher o provedor de log para instrumentar os aplicativos e serviços, os logs e eventos precisam ser agregados para que possam ser enviados a qualquer plataforma de análise. Leia sobre o [Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md), [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md) e [WAD](service-fabric-diagnostics-event-aggregation-wad.md) para entender melhor algumas das opções recomendadas.
