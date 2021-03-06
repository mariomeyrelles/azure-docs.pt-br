---
title: O que são Hubs de Notificação do Azure?
description: Saiba como adicionar recursos de notificação por push com Hubs de Notificação do Azure.
author: dimazaid
manager: kpiteira
editor: spelluru
services: notification-hubs
documentationcenter: ''
ms.assetid: fcfb0ce8-0e19-4fa8-b777-6b9f9cdda178
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: multiple
ms.devlang: multiple
ms.topic: overview
ms.custom: mvc
ms.date: 04/14/2018
ms.author: dimazaid
ms.openlocfilehash: 44086bc20966d9c01ff27dda68f837101c71a778
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/07/2018
---
# <a name="what-is-azure-notification-hubs"></a>O que são Hubs de Notificação do Azure?
Os Hubs de Notificação do Azure fornecem um mecanismo de push expansível e fácil de usar que permite que você envie notificações para qualquer plataforma (iOS, Android, Windows, Kindle, Baidu etc.) de qualquer back-end (nuvem ou local). Os Hubs de Notificação funcionam bem tanto para cenários empresariais quanto para cenários de consumidor. Aqui estão alguns exemplos de cenários:

- Para enviar notificações sobre as novidades para milhões de pessoas com baixa latência.
- Para enviar cupons baseados na localização para segmentos de usuários interessados.
- Para enviar notificações de eventos para usuários ou grupos em aplicativos de mídia/esportes/finanças/jogos.
- Para enviar por push conteúdos promocionais para aplicativos com o objetivo de atrair e vender para os clientes.
- Para notificar os usuários sobre eventos corporativos, como novas mensagens e itens de trabalho.
- Para enviar códigos para Autenticação Multifator.

## <a name="what-are-push-notifications"></a>O que são notificações por push?
Notificações por push são uma forma de comunicação do aplicativo para o usuário na qual os usuários de aplicativos móveis recebem notificações sobre determinadas informações desejadas, geralmente em uma caixa de diálogo ou pop-up. Geralmente, os usuários podem optar por exibir ou ignorar a mensagem. Escolher a primeira opção abre o aplicativo móvel que comunicou a notificação.

As notificações por push são fundamentais para os aplicativos direcionados ao consumidor no aumento da interação e do uso do aplicativo e para os aplicativos empresariais na comunicação de informações atualizadas da empresa. É a melhor forma de comunicação de aplicativos para o usuário, porque oferece baixo consumo de bateria para dispositivos móveis, é flexível para os remetentes das notificações e permanece disponível mesmo quando os aplicativos correspondentes não estiverem ativos.

Saiba mais sobre as notificações por push em algumas plataformas populares, consulte os tópicos a seguir: 
* [iOS](https://developer.apple.com/notifications/)
* [Android](https://developer.android.com/guide/topics/ui/notifiers/notifications.html)
* [Windows](http://msdn.microsoft.com/library/windows/apps/hh779725.aspx)

## <a name="how-push-notifications-work"></a>Como as notificações por push funcionam?
As notificações por push são fornecidas por meio de infraestruturas específicas à plataforma chamadas de *Sistemas de Notificação de Plataforma* (PNS). Eles oferecem funcionalidades de push barebone para enviar mensagens para um dispositivo com um identificador fornecido e não contam com interfaces comuns. Para enviar uma notificação para todos os clientes através das versões iOS, Android e Windows de um aplicativo, o desenvolvedor deve trabalhar com APNS (Apple Push Notification Service), FCM (Firebase Cloud Messaging) e WNS (serviço de notificação do Windows).

Em um alto nível, é assim que o push funciona:

1. O aplicativo cliente decide que deseja receber a notificação. Portanto, ele contata o Sistema de Notificação de Plataforma (PNS) correspondente para recuperar seu identificador push exclusivo e temporário. O tipo de identificador depende do sistema (por exemplo, o WNS tem URIs, enquanto o APNS tem tokens).
2. O aplicativo cliente armazena esse identificador no back-end do aplicativo ou provedor.
3. Para enviar uma notificação por push, o back-end do aplicativo entra em contato com o PNS usando o identificador para selecionar um aplicativo cliente específico.
4. O PNS encaminha a notificação para o dispositivo especificado pelo identificador.

![Fluxo de trabalho de notificação por push](./media/notification-hubs-overview/registration-diagram.png)

## <a name="the-challenges-of-push-notifications"></a>Os desafios de notificações por push
Os PNSes são eficientes. No entanto, ainda há muito trabalho para o desenvolvedor do aplicativo para implementar até cenários comuns de notificação por push, como a transmissão de notificações por push a usuários segmentados.

As notificações por push requerem uma infraestrutura complexa que não está relacionada à lógica de negócios principal do aplicativo. Alguns dos desafios de infraestrutura são:

- **Dependência de plataforma**
    - O back-end deve ter uma lógica dependente de plataforma complexa e difícil de manter para enviar notificações para dispositivos em várias plataformas, já que os PNS não são unificados.
- **Escala**
    - De acordo com as diretrizes de PNS, os tokens de dispositivo deverão ser atualizados sempre que o aplicativo for iniciado. O back-end está lidando com uma grande quantidade de tráfego e acesso ao banco de dados apenas para manter os tokens atualizados. Quando o número de dispositivos aumenta para milhões e milhares de milhões, o custo da criação e manutenção dessa infraestrutura é enorme.
    - A maioria dos PNS não dá suporte à transmissão para vários dispositivos. Uma transmissão simples para um milhão de dispositivos resulta em um milhão de chamadas para o PNS. Escalar essa quantidade de tráfego com latência mínima é uma tarefa difícil.
- **Roteamento** 
    - Embora os PNS forneçam uma maneira de enviar mensagens para dispositivos, a maioria das notificações de aplicativos são destinadas a usuários ou grupos de interesse. O back-end deve manter um registro para associar os dispositivos com grupos de interesse, usuários, propriedades, etc. Essa sobrecarga aumenta o tempo total de colocação no mercado e os custos de manutenção de um aplicativo.

## <a name="why-use-azure-notification-hubs"></a>Por que usar Hubs de Notificação do Azure?
Os Hubs de Notificação eliminam todas as complexidades associadas a enviar notificações por push por sua conta de seu aplicativo de back-end. Sua infraestrutura de notificação por push multiplataforma e dimensionável reduz os códigos de push e simplifica o seu back-end. Com os Hubs de Notificação, os dispositivos são responsáveis somente por registrar os identificadores de PNS com o hub, enquanto o back-end envia mensagens para os usuários ou grupos de interesse, como mostra a figura a seguir:

![Diagrama do Hub de Notificação](./media/notification-hubs-overview/notification-hub-diagram.png)

Os Hubs de Notificação são o seu mecanismo por push pronto para uso com as seguintes vantagens:

- **Plataformas cruzadas**
    - Suporte para todas as plataformas de push principais, incluindo iOS, Android, Windows e Kindle e Baidu.
    - Uma interface comum para enviar notificações por push para todas as plataformas em formatos específicos de plataforma ou independentes da plataforma, sem trabalho específico da plataforma.
    - Gerenciamento de identificador de dispositivo em um só local.
- **Back-ends cruzados**
    - Em nuvem ou local
    - .NET, Node.js, Java, etc.
- **Conjunto avançado de padrões de entrega**
    - Transmissão para uma ou várias plataformas: você pode fazer transmissões instantâneas para milhões de dispositivos de diversas plataformas com uma única chamada à API.
    - Enviar notificação por push para um dispositivo: você pode enviar notificações para dispositivos individuais.
    - Enviar notificação por push para um usuário: recursos de marcações e modelos ajudam você a acessar todos os dispositivos de plataforma cruzada de um usuário.
    - Enviar notificação por push para o segmento com marcações dinâmicas: o recurso de marcações ajuda você a segmentar dispositivos e enviar notificações por push a eles de acordo com as suas necessidades, esteja você enviando para um segmento ou uma expressão de segmentos (por exemplo, ativo E reside em Seattle, NÃO novo usuário). Em vez de ficar limitado a opções de publicação/assinatura, você pode atualizar as marcações de dispositivo em qualquer lugar e a qualquer momento.
    - Notificações por push localizadas: recurso de modelos que ajuda você a alcançar uma localização sem afetar o código de back-end.
    - Notificações por push silenciosas: você pode habilitar o padrão push e pull ao enviar notificações silenciosas para dispositivos e disparando-os para concluir determinadas ações ou pulls.
    - Notificações por push agendadas: você pode agendar o envio de notificações a qualquer momento.
    - Notificações por push diretas: você pode ignorar o registro de dispositivos com o serviço dos Hubs de Notificações e distribuir em lote por push diretamente para uma lista de identificadores de dispositivos.
    - Notificações por push personalizadas: as variáveis de envio a dispositivo ajudam você a enviar notificações por push personalizadas específicas para o dispositivo com pares de valor-chave personalizados.
- **Telemetria avançada**
    - A telemetria geral de push, dispositivo, erro e operação está disponível no portal do Azure e programaticamente.
    - A telemetria por mensagem controla cada notificação por push da sua chamada de solicitação inicial para o serviço de Hubs de Notificações que obteve êxito no envio em lote.
    - Os comentários do Sistema de Notificação de Plataforma comunicam todos os comentários dos sistemas de notificação de plataforma para ajudar na depuração.
- **Escalabilidade** 
    - Envie mensagens rápidas para milhões de dispositivos sem fragmentação de dispositivo ou rearquitetura.
- **Segurança**
    - Segredo de acesso compartilhado (SAS) ou autenticação federada.

## <a name="integration-with-app-service-mobile-apps"></a>Integração com Aplicativos Móveis do Serviço de Aplicativo
Para facilitar uma experiência integrada e unificada nos serviços do Azure, os [Aplicativos Móveis do Serviço de Aplicativo](../app-service-mobile/app-service-mobile-value-prop.md) possui suporte interno para notificações de push usando Hubs de notificação. [Aplicativos Móveis do Serviço de Aplicativo](../app-service-mobile/app-service-mobile-value-prop.md) oferecem uma plataforma de desenvolvimento de aplicativos móveis altamente escalonável, disponível globalmente para os desenvolvedores corporativos e integradores de sistema e que traz um conjunto rico de recursos para desenvolvedores de aplicativos móveis.

Os desenvolvedores de aplicativos móveis podem utilizar Hubs de notificação com o fluxo de trabalho a seguir:

1. Recuperar o identificador de PNS do dispositivo
2. Registrar o dispositivo com Hubs de Notificação por meio da conveniente API de registro do SDK do cliente de Aplicativos Móveis

    > [!NOTE]
    > Observe que os aplicativos móveis removem imediatamente todas as marcas nos registros para fins de segurança. Trabalhar com Hubs de notificação do seu back-end diretamente para associar marcas a dispositivos.
1. Enviar notificações do seu back-end de aplicativo com Hubs de notificação

Aqui estão alguns recursos convenientes para desenvolvedores com essa integração:

- **SDKs do cliente de Aplicativos Móveis**: estes SDKs de várias plataformas fornecem APIs simples para registro e se comunicam com o hub de notificação vinculado automaticamente com o aplicativo móvel. Os desenvolvedores não precisam se aprofundar nas credenciais de Hubs de Notificação e trabalhar com um serviço adicional.
    - *Notificações por push para um usuário*: os SDKs marcam automaticamente o dispositivo fornecido com a ID de usuário autenticada de Aplicativos Móveis para habilitar o envio de notificações por push para o cenário do usuário.
    - *Notificações por push para um dispositivo*: os SDKs usam automaticamente a ID de instalação de Aplicativos Móveis como GUID para se registrar com os Hubs de Notificação, poupando os desenvolvedores da dificuldade de manter vários GUIDs de serviço.
- **Modelo de instalação**: os Aplicativos móveis funcionam com o modelo de push mais recente dos Hubs de Notificação para representar todas as propriedades de push associadas a um dispositivo em uma instalação de JSON que se alinha com os Serviços de Notificação por Push e é fácil de usar.
- **Flexibilidade**: os desenvolvedores sempre podem optar por trabalhar com os Hubs de Notificação diretamente até mesmo com a integração em vigor.
- **Experiência integrada no [portal do Azure](https://portal.azure.com)**: o Push como uma capacidade é representado visualmente em Aplicativos Móveis e os desenvolvedores podem facilmente trabalhar com o hub de notificação associado por meio de Aplicativos Móveis.

## <a name="next-steps"></a>Próximas etapas
Introdução à criação e ao uso de um hub de notificação, seguindo o [Tutorial: notificações por push para aplicativos móveis](notification-hubs-android-push-notification-google-fcm-get-started.md). [0]: ./media/notification-hubs-overview/registration-diagram.png [1]: ./media/notification-hubs-overview/notification-hub-diagram.png [Como os clientes estão usando os Hubs de notificação]: http://azure.microsoft.com/services/notification-hubs [Guias e tutoriais dos Hubs de notificação]: http://azure.microsoft.com/documentation/services/notification-hubs [iOS]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started [Android]: http://azure.microsoft.com/documentation/articles/notification-hubs-android-get-started [Windows Universal]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started [Windows Phone]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-phone-get-started [Kindle]: http://azure.microsoft.com/documentation/articles/notification-hubs-kindle-get-started [Xamarin.iOS]: http://azure.microsoft.com/documentation/articles/partner-xamarin-notification-hubs-ios-get-started [Xamarin.Android]: http://azure.microsoft.com/documentation/articles/partner-xamarin-notification-hubs-android-get-started [Microsoft.WindowsAzure.Messaging.NotificationHub]: http://msdn.microsoft.com/library/microsoft.windowsazure.messaging.notificationhub.aspx [Microsoft.ServiceBus.Notifications]: http://msdn.microsoft.com/library/microsoft.servicebus.notifications.aspx [Aplicativos móveis do serviço de aplicativo]: https://azure.microsoft.com/documentation/articles/app-service-mobile-value-prop/ [modelos]: notification-hubs-templates-cross-platform-push-messages.md [portal do Azure]: https://portal.azure.com [marcações]: (http://msdn.microsoft.com/library/azure/dn530749.aspx)
