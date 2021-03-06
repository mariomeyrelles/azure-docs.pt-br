---
title: Esquema de evento grupo de recursos da Grade de Eventos do Azure
description: Descreve as propriedades que são fornecidas para eventos de grupos de recursos com a Grade de Eventos do Azure
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: reference
ms.date: 01/30/2018
ms.author: tomfitz
ms.openlocfilehash: 163c32bdb8a3fdc278404b9e26fdc3097797d16c
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/18/2018
---
# <a name="azure-event-grid-event-schema-for-resource-groups"></a>Esquema de eventos para assinatura da Grade de Eventos do Azure

Este artigo fornece as propriedades e o esquema de eventos de grupo de recursos. Para obter uma introdução a esquemas de evento, consulte [esquema de grade de eventos do Azure](event-schema.md).

Grupos de recursos e as assinaturas do Azure emitem os mesmos tipos de evento. Os tipos de eventos relacionados às alterações em recursos. A principal diferença é que grupos de recursos de emissão de eventos para os recursos no grupo de recursos e as assinaturas do Azure emitem eventos de recursos entre a assinatura. 

## <a name="available-event-types"></a>Tipos de evento disponíveis

Os Grupos de Recursos agora podem emitir eventos de gerenciamento do Azure Resource Manager, como quando uma VM é criada ou uma conta de armazenamento é excluída.

| Tipo de evento | DESCRIÇÃO |
| ---------- | ----------- |
| Microsoft.Resources.ResourceWriteSuccess | Gerado quando um recurso cria ou operação de atualização será bem-sucedida. |
| Microsoft.Resources.ResourceWriteFailure | Gerado quando um recurso cria ou operação de atualização será falha. |
| Microsoft.Resources.ResourceWriteCancel | Gerado quando um recurso cria ou operação de atualização é cancelada. |
| Microsoft.Resources.ResourceDeleteSuccess | Gerado quando um recurso cria ou operação de atualização é bem-sucedida. |
| Microsoft.Resources.ResourceDeleteFailure | Gerado quando um recurso cria ou operação de atualização é falha. |
| Microsoft.Resources.ResourceDeleteCancel | Gerado quando um recurso cria ou operação de atualização é cancelada. Esse evento ocorre quando a implantação de um modelo é cancelada. |

## <a name="example-event"></a>Exemplo de evento

O exemplo a seguir mostra o esquema de um recurso criado de evento: 

```json
[
  {
    "topic":"/subscriptions/{subscription-id}/resourceGroups/{resource-group}",
    "subject":"/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventGrid/eventSubscriptions/LogicAppdd584bdf-8347-49c9-b9a9-d1f980783501",
    "eventType":"Microsoft.Resources.ResourceWriteSuccess",
    "eventTime":"2017-08-16T03:54:38.2696833Z",
    "id":"25b3b0d0-d79b-44d5-9963-440d4e6a9bba",
    "data": {
        "authorization":"{azure_resource_manager_authorizations}",
        "claims":"{azure_resource_manager_claims}",
        "correlationId":"54ef1e39-6a82-44b3-abc1-bdeb6ce4d3c6",
        "httpRequest":"{request-operation}",
        "resourceProvider":"Microsoft.EventGrid",
        "resourceUri":"/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventGrid/eventSubscriptions/LogicAppdd584bdf-8347-49c9-b9a9-d1f980783501",
        "operationName":"Microsoft.EventGrid/eventSubscriptions/write",
        "status":"Succeeded",
        "subscriptionId":"{subscription-id}",
        "tenantId":"72f988bf-86f1-41af-91ab-2d7cd011db47"
    },
    "dataVersion": "",
    "metadataVersion": "1"
  }
]
```

O esquema para um evento de recurso excluído é semelhante:

```json
[{
  "topic":"/subscriptions/{subscription-id}/resourceGroups/{resource-group}",
  "subject": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventGrid/eventSubscriptions/LogicApp0ecd6c02-2296-4d7c-9865-01532dc99c93",
  "eventType": "Microsoft.Resources.ResourceDeleteSuccess",
  "eventTime": "2017-11-07T21:24:19.6959483Z",
  "id": "7995ecce-39d4-4851-b9d7-a7ef87a06bf5",
  "data": {
    "authorization": "{azure_resource_manager_authorizations}",
    "claims": "{azure_resource_manager_claims}",
    "correlationId": "7995ecce-39d4-4851-b9d7-a7ef87a06bf5",
    "httpRequest": "{request-operation}",
    "resourceProvider": "Microsoft.EventGrid",
    "resourceUri": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventGrid/eventSubscriptions/LogicAppdd584bdf-8347-49c9-b9a9-d1f980783501",
    "operationName": "Microsoft.EventGrid/eventSubscriptions/delete",
    "status": "Succeeded",
    "subscriptionId": "{subscription-id}",
    "tenantId": "72f988bf-86f1-41af-91ab-2d7cd011db47"
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

## <a name="event-properties"></a>Propriedades do evento

Um evento tem os seguintes dados de nível superior:

| Propriedade | type | DESCRIÇÃO |
| -------- | ---- | ----------- |
| topic | string | Caminho de recurso completo para a origem do evento. Esse campo não é gravável. Grade de Eventos fornece esse valor. |
| subject | string | Caminho definido pelo fornecedor para o assunto do evento. |
| eventType | string | Um dos tipos de evento registrados para a origem do evento. |
| eventTime | string | A hora em que o evento é gerado com base na hora UTC do provedor. |
| ID | string | Identificador exclusivo do evento. |
| data | objeto | Dados de evento do grupo de recursos. |
| dataVersion | string | A versão do esquema do objeto de dados. O fornecedor define a versão do esquema. |
| metadataVersion | string | A versão do esquema do metadados de evento. Grade de Eventos define o esquema de propriedades de nível superior. Grade de Eventos fornece esse valor. |

O objeto de dados tem as seguintes propriedades:

| Propriedade | type | DESCRIÇÃO |
| -------- | ---- | ----------- |
| autorização | string | A autorização solicitada para a operação. |
| declarações | string | As propriedades da declaração. |
| correlationId | string | Uma ID de operação para solução de problemas. |
| httpRequest | string | Os detalhes da operação. |
| ResourceProvider | string | O provedor de recursos executando a operação. |
| resourceUri | string | O URI do recurso na operação. |
| operationName | string | A operação que foi executada. |
| status | string | O status da operação. |
| subscriptionId | string | A ID da assinatura do recurso. |
| tenantId | string | A ID do locatário do recurso. |

## <a name="next-steps"></a>Próximas etapas

* Para ver uma introdução à Grade de Eventos do Azure, confira [O que é uma Grade de eventos?](overview.md)
* Para obter mais informações sobre como criar uma assinatura da Grade de Eventos do Azure, confira [Event Grid subscription schema](subscription-creation-schema.md) (Esquema de assinatura da Grade de Eventos).
