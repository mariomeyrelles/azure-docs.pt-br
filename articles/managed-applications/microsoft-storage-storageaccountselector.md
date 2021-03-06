---
title: Elemento de interface do usuário StorageAccountSelector do Azure | Microsoft Docs
description: Descreve o elemento de interface do usuário Microsoft.Storage.StorageAccountSelector para o Portal do Azure.
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: managed-applications
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2018
ms.author: tomfitz
ms.openlocfilehash: 4a18d629b959e4f1abf4ec2df28a31180efec89f
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/17/2018
---
# <a name="microsoftstoragestorageaccountselector-ui-element"></a>Elemento de interface do usuário Microsoft.Storage.StorageAccountSelector
Um controle para selecionar uma conta de armazenamento nova ou existente.

## <a name="ui-sample"></a>Exemplo de interface do usuário
![Microsoft.Storage.StorageAccountSelector](./media/managed-application-elements/microsoft.storage.storageaccountselector.png)

## <a name="schema"></a>Esquema
```json
{
  "name": "element1",
  "type": "Microsoft.Storage.StorageAccountSelector",
  "label": "Storage account",
  "toolTip": "",
  "defaultValue": {
    "name": "storageaccount01",
    "type": "Premium_LRS"
  },
  "constraints": {
    "allowedTypes": [],
    "excludedTypes": []
  },
  "options": {
    "hideExisting": false
  },
  "visible": true
}
```

## <a name="remarks"></a>Comentários
- Se especificado, `defaultValue.name` é validados automaticamente por exclusividade. Se o nome da conta de armazenamento não for exclusivo, o usuário deverá especificar um nome diferente ou escolher uma conta de armazenamento existente.
- O valor padrão para `defaultValue.type` é **Premium_LRS**.
- Os tipos não especificados em `constraints.allowedTypes` ficam ocultos e os tipos não especificados em `constraints.excludedTypes` são mostrados.
`constraints.allowedTypes` e `constraints.excludedTypes` são opcionais, mas não podem ser usados simultaneamente.
- Se `options.hideExisting` é **true**, o usuário não pode escolher uma conta de armazenamento existente. O valor padrão é **false**.


## <a name="sample-output"></a>Saída de exemplo
```json
{
  "name": "storageaccount01",
  "resourceGroup": "rg01",
  "type": "Premium_LRS",
  "newOrExisting": "new"
}
```

## <a name="next-steps"></a>Próximas etapas
* Para obter uma introdução à criação de definições de interface do usuário, consulte [Introdução ao CreateUiDefinition](create-uidefinition-overview.md).
* Para obter uma descrição das propriedades comuns em elementos de interface do usuário, consulte [Elementos de CreateUiDefinition](create-uidefinition-elements.md).
