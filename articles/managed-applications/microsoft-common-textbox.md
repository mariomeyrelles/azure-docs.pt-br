---
title: Elemento de interface do usuário TextBox UI do Azure | Microsoft Docs
description: Descreve o elemento de interface do usuário Microsoft.Common.TextBox para o Portal do Azure.
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
ms.openlocfilehash: f11098f4af2e85df16e00ce6dac6e7c4b5b34e2b
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/17/2018
---
# <a name="microsoftcommontextbox-ui-element"></a>Elemento de interface do usuário Microsoft.Common.TextBox
Um controle que pode ser usado para editar texto não formatado.

## <a name="ui-sample"></a>Exemplo de interface do usuário
![Microsoft.Common.TextBox](./media/managed-application-elements/microsoft.common.textbox.png)

## <a name="schema"></a>Esquema
```json
{
  "name": "element1",
  "type": "Microsoft.Common.TextBox",
  "label": "Some text box",
  "defaultValue": "my value",
  "toolTip": "Use only allowed characters",
  "constraints": {
    "required": true,
    "regex": "^[a-z0-9A-Z]{1,30}$",
    "validationMessage": "Only alphanumeric characters are allowed, and the value must be 1-30 characters long."
  },
  "visible": true
}
```

## <a name="remarks"></a>Comentários
- Se `constraints.required` é definido como **true**, a caixa de texto deve conter um valor a ser validado com êxito. O valor padrão é **false**.
- `constraints.regex` é um padrão de expressão regular JavaScript. Se especificado, o valor da caixa de texto deve corresponder ao padrão para validar com êxito. O valor padrão é **null**.
- `constraints.validationMessage` é uma cadeia de caracteres a ser exibida quando o valor da caixa de texto falha na validação. Se não for especificado, as mensagens de validação internas da caixa de texto serão usadas. O valor padrão é **null**.
- É possível especificar um valor para `constraints.regex` quando `constraints.required` é definido como **false**. Nesse cenário, um valor não é necessário para a caixa de texto validar com êxito. Se um valor for especificado, ele deve corresponder ao padrão da expressão regular.

## <a name="sample-output"></a>Saída de exemplo

```json
"my value"
```

## <a name="next-steps"></a>Próximas etapas
* Para obter uma introdução à criação de definições de interface do usuário, consulte [Introdução ao CreateUiDefinition](create-uidefinition-overview.md).
* Para obter uma descrição das propriedades comuns em elementos de interface do usuário, consulte [Elementos de CreateUiDefinition](create-uidefinition-elements.md).
