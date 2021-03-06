---
title: Erros de nome da conta de armazenamento do Azure | Microsoft Docs
description: Descreve os erros com que você pode se deparar ao especificar um nome de conta de armazenamento.
services: azure-resource-manager
documentationcenter: ''
author: tfitzmac
manager: timlt
editor: ''
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 03/09/2018
ms.author: tomfitz
ms.openlocfilehash: da7147439bba668c6c614c19d91a22ee78275c69
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/20/2018
---
# <a name="resolve-errors-for-storage-account-names"></a>Solucione erros de nomes de conta de armazenamento

Este artigo descreve erros de nomenclatura com que você pode se deparar durante a implantação de uma conta de armazenamento.

## <a name="symptom"></a>Sintoma

Se o nome da conta de armazenamento incluir caracteres proibidos, você receberá um erro semelhante a:

```
Code=AccountNameInvalid
Message=S!torageckrexph7isnoc is not a valid storage account name. Storage account name must be 
between 3 and 24 characters in length and use numbers and lower-case letters only.
```

Para contas de armazenamento, você deve fornecer um nome para o recurso que é exclusivo no Azure. Se você não fornecer um nome exclusivo, receberá um erro como:

```
Code=StorageAccountAlreadyTaken
Message=The storage account named mystorage is already taken.
```

Se você implantar uma conta de armazenamento com o mesmo nome que uma conta de armazenamento existente na sua assinatura, mas fornecer um local diferente, você receberá um erro indicando que a conta de armazenamento já existe em um local diferente. Exclua a conta de armazenamento existente ou forneça o mesmo local da conta de armazenamento existente.

## <a name="cause"></a>Causa

Os nomes da conta de armazenamento devem ter entre 3 e 24 caracteres, usar números e apenas letras minúsculas. O nome deve ser exclusivo.

## <a name="solution"></a>Solução

O nome da conta de armazenamento deve ser exclusivo. Você pode criar um nome exclusivo concatenando a convenção de nomenclatura com o resultado da função [uniqueString](resource-group-template-functions-string.md#uniquestring) .

```json
"name": "[concat('storage', uniqueString(resourceGroup().id))]",
"type": "Microsoft.Storage/storageAccounts",
```

Verifique se o nome da conta de armazenamento não ultrapassa 24 caracteres. O [uniqueString](resource-group-template-functions-string.md#uniquestring) função retorna 13 caracteres. Se você concatenar um prefixo ou sufixo ao resultado **uniqueString**, forneça um valor que tenha 11 caracteres ou menos.

```json
"parameters": {
    "storageNamePrefix": {
      "type": "string",
      "maxLength": 11,
      "defaultValue": "storage",
      "metadata": {
        "description": "The value to use for starting the storage account name."
      }
    }
}
```

Verifique se o nome da conta de armazenamento não inclui letras maiúsculas ou caracteres especiais.