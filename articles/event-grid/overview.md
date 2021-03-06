---
title: Visão geral da Grade de Eventos do Azure
description: Descreve a Grade de Eventos do Azure e seus conceitos.
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: overview
ms.date: 04/27/2018
ms.author: babanisa
ms.openlocfilehash: f1d235fe431cfe14019ffef7c043dfbc367bb2bc
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/18/2018
ms.locfileid: "34303970"
---
# <a name="an-introduction-to-azure-event-grid"></a>Uma introdução à Grade de Eventos do Azure

A Grade de Eventos do Azure permite compilar facilmente aplicativos com as arquiteturas baseadas em evento. Você seleciona o recurso do Azure que quer assinar e fornece o manipulador de eventos ou o ponto de extremidade do WebHook ao qual enviar o evento. A Grade de Eventos tem suporte interno para eventos provenientes de serviços do Azure, como blobs de armazenamento e grupos de recursos. A Grade de Eventos também tem suporte personalizado para eventos de aplicativo e de terceiros, usando webhooks e tópicos personalizados. 

Você pode usar filtros para encaminhar eventos específicos a pontos de extremidade diferentes, multicast para vários pontos de extremidade e verificar se os eventos são distribuídos de forma confiável. A Grade de Eventos também tem suporte interno para eventos personalizados e de terceiros.

No momento, a Grade de Eventos oferece suporte às seguintes regiões:

* Sudeste da Ásia
* Leste da Ásia
* Leste da Austrália
* Sudeste da Austrália
* Centro dos EUA
*   Leste dos EUA
*   Leste dos EUA 2
* Europa Ocidental
* Norte da Europa
* Leste do Japão
* Oeste do Japão
*   Centro-Oeste dos EUA
*   Oeste dos EUA
*   Oeste dos EUA 2

Este artigo fornece uma visão geral da Grade de Eventos do Azure. Se você quiser começar a usar a Grade de Eventos, confira [Criar e rotear eventos personalizados com a Grade de Eventos do Azure](custom-event-quickstart.md). A imagem a seguir mostra como a Grade de Eventos conecta fontes e manipuladores, mas não fornece uma lista abrangente de opções com suporte.

![Modelo funcional da Grade de Eventos](./media/overview/functional-model.png)

## <a name="event-sources"></a>Origens de eventos

Atualmente, os serviços do Azure a seguir dão suporte para envio de eventos à Grade de Eventos:

* Assinaturas do Azure (operações de gerenciamento)
* Tópicos personalizados
* Hubs de Eventos
* Hub IoT
* Serviços de mídia
* Grupos de recursos (operações de gerenciamento)
* Barramento de Serviço
* Blob de Armazenamento
* Uso geral v2 (GPv2) do Armazenamento

Para obter links para artigos que mostram como usar cada origem de evento, consulte [Origens de evento na Grade de Eventos do Azure](event-sources.md).

## <a name="event-handlers"></a>Manipuladores de eventos

Atualmente, os serviços do Azure a seguir dão suporte para manipulação de eventos da Grade de Eventos: 

* Automação do Azure
* Funções do Azure
* Hubs de Eventos
* Conexões Híbridas
* Aplicativos Lógicos
* Microsoft Flow
* Armazenamento de Filas
* WebHooks

Ao usar o Azure Functions como o manipulador, use o gatilho Grade de Eventos em vez de gatilhos HTTP genéricos. A Grade de Eventos valida automaticamente os gatilhos de Função da Grade de Eventos. Com os gatilhos HTTP genéricos, você deve implementar a [resposta de validação](security-authentication.md#webhook-event-delivery).

Para obter links para artigos que mostram como usar cada manipulador de evento, consulte [Manipuladores de evento na Grade de Eventos do Azure](event-handlers.md).

## <a name="concepts"></a>Conceitos

Há cinco conceitos na Grade de Eventos do Azure para sua utilização:

* **Eventos**: o que aconteceu.
* **Fontes/publicadores de evento**: onde o evento ocorreu.
* **Tópicos**: o ponto de extremidade onde os publicadores enviam eventos.
* **Assinaturas de evento**: o ponto de extremidade ou o mecanismo interno para encaminhar eventos, às vezes, para vários manipuladores. As assinaturas também são usadas por manipuladores para filtrar de forma inteligente os eventos de entrada.
* **Manipuladores de eventos**: o aplicativo ou serviço que reage ao evento.

Para saber mais sobre esses conceitos, confira [Conceitos na Grade de Eventos do Azure](concepts.md).

## <a name="capabilities"></a>Funcionalidades

Aqui estão alguns dos principais recursos da Grade de Eventos do Azure:

* **Simplicidade**: aponte e clique para direcionar eventos de seus recursos do Azure para qualquer manipulador de eventos ou ponto de extremidade.
* **Filtragem avançada**: filtre com tipo de evento ou caminho de publicação do evento para garantir que os manipuladores de eventos recebam apenas eventos relevantes.
* **Fan-out**: inscreva vários pontos de extremidade no mesmo evento para enviar cópias do evento ao máximo de locais possível.
* **Confiabilidade**: utilize repetição por 24 horas com retirada exponencial para garantir a entrega dos eventos.
* **Pagamento por evento**: pague apenas pela quantidade usada da Grade de Eventos.
* **Alta taxa de transferência**: crie cargas de trabalho de alto volume na Grade de Eventos com suporte para milhões de eventos por segundo.
* **Eventos internos**: comece a executar rapidamente com eventos internos definidos pelo recurso.
* **Eventos personalizados**: use eventos personalizados de rota, filtro e entrega confiável da de Grade de Eventos em seu aplicativo.

Para obter uma comparação de Grade de Eventos, Hubs de Eventos e Barramento de Serviço, consulte [Escolher entre os serviços do Azure que entregam mensagens](compare-messaging-services.md).

## <a name="what-can-i-do-with-event-grid"></a>O que posso fazer com a Grade de Eventos?

A Grade de Eventos do Azure fornece vários recursos que melhoram muito o trabalho sem servidor, de automação de operações e de integração: 

### <a name="serverless-application-architectures"></a>Arquiteturas de aplicativo sem servidor

![Aplicativo sem servidor](./media/overview/serverless_web_app.png)

A Grade de Eventos conecta fontes de dados e manipuladores de eventos. Por exemplo, use a Grade de Eventos para disparar instantaneamente uma função sem servidor para executar a análise de imagem sempre que uma nova foto é adicionada a um contêiner de armazenamento de blobs. 

### <a name="ops-automation"></a>Automação de operações

![Automação de operações](./media/overview/Ops_automation.png)

A Grade de Eventos permite que você acelere a automação e simplifique a imposição de políticas. Por exemplo, a Grade de Eventos pode notificar a Automação do Azure quando uma máquina virtual é criada ou um Banco de Dados SQL é criado. Esses eventos podem ser usados para verificar automaticamente se as configurações do serviço estão em conformidade, colocar os metadados em ferramentas de operações, marcar máquinas virtuais ou arquivar itens de trabalho.

### <a name="application-integration"></a>Integração de aplicativos

![Integração de aplicativos](./media/overview/app_integration.png)

A Grade de Eventos conecta seu aplicativo a outros serviços. Por exemplo, crie um tópico personalizado para enviar os dados de evento do aplicativo para a Grade de Eventos e aproveite a entrega confiável, o roteamento avançado e a integração com o Azure. Como alternativa, você pode usar a Grade de Eventos com Aplicativos Lógicos para processar dados em qualquer lugar, sem escrever código. 

## <a name="how-much-does-event-grid-cost"></a>Quanto custa a Grade de Eventos?

A Grade de Eventos do Azure usa um modelo de preço de pagamento por evento, para que você pague só pelo que usa. As primeiras 100.000 operações por mês são gratuitas. As operações são definidas como ingresso de evento, correspondência avançada, tentativa de entrega e chamadas de gerenciamento. Para obter detalhes, consulte a [página de preços](https://azure.microsoft.com/pricing/details/event-grid/).

## <a name="next-steps"></a>Próximas etapas

* [Eventos de blob de armazenamento de rota](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json)  
  Responda a eventos de blob de armazenamento usando a Grade de Eventos.
* [Criar e assinar eventos personalizados](custom-event-quickstart.md)  
  Comece imediatamente a enviar seus próprios eventos personalizados para qualquer ponto de extremidade usando o início rápido da Grade de Eventos do Azure.
* [Usando Aplicativos Lógicos como um Manipulador de eventos](monitor-virtual-machine-changes-event-grid-logic-app.md)  
  Um tutorial sobre como compilar um aplicativo usando os Aplicativos Lógicos para reagir a eventos enviados por push pela Grade de Eventos.
* [Transmitir Big Data para um data warehouse](event-grid-event-hubs-integration.md)  
  Um tutorial que usa o Azure Functions para transmitir dados dos Hubs de Eventos para o SQL Data Warehouse.
* [Referência da API REST da Grade de Eventos](/rest/api/eventgrid)  
  Fornece mais informações técnicas sobre a Grade de Eventos do Azure e uma referência para o gerenciamento de assinaturas, roteamento e filtragem de Evento.