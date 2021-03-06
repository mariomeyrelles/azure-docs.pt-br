---
title: Elemento de interface do usuário PublicIpAddressCombo do Azure | Microsoft Docs
description: Descreve o elemento de interface do usuário Microsoft.Network.PublicIpAddressCombo para o Portal do Azure.
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
ms.date: 04/30/2018
ms.author: tomfitz
ms.openlocfilehash: bf0ef5be609fba14ab12e1e6f9f97bc63f032aae
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/17/2018
---
# <a name="microsoftnetworkpublicipaddresscombo-ui-element"></a>Elemento de interface do usuário Microsoft.Network.PublicIpAddressCombo
Um grupo de controles para selecionar um endereço IP público novo ou existente.

## <a name="ui-sample"></a>Exemplo de interface do usuário
![Microsoft.Network.PublicIpAddressCombo](./media/managed-application-elements/microsoft.network.publicipaddresscombo.png)

- Se o usuário selecionar 'Nenhum' como endereço IP público, a caixa de texto de rótulo do nome de domínio ficará oculta.
- Se o usuário selecionar um endereço IP público existente, a caixa de texto de rótulo do nome de domínio ficará desabilitada. Seu valor é o rótulo de nome de domínio do endereço IP selecionado.
- O sufixo do nome de domínio (por exemplo, westus.cloudapp.azure.com) é atualizado automaticamente com base no local selecionado.

## <a name="schema"></a>Esquema
```json
{
  "name": "element1",
  "type": "Microsoft.Network.PublicIpAddressCombo",
  "label": {
    "publicIpAddress": "Public IP address",
    "domainNameLabel": "Domain name label"
  },
  "toolTip": {
    "publicIpAddress": "",
    "domainNameLabel": ""
  },
  "defaultValue": {
    "publicIpAddressName": "ip01",
    "domainNameLabel": "mydomain"
  },
  "constraints": {
    "required": {
      "domainNameLabel": true
    }
  },
  "options": {
    "hideNone": false,
    "hideDomainNameLabel": false,
    "hideExisting": false,
    "zone": 3
  },
  "visible": true
}
```

## <a name="remarks"></a>Comentários
- Se `constraints.required.domainNameLabel` é definido como **true**, o usuário deve fornecer um rótulo de nome de domínio ao criar um novo endereço IP público. Endereços IP públicos existentes sem um rótulo não ficam disponíveis para seleção.
- Se `options.hideNone` é definido como **true**, a opção de selecionar **Nenhum** para o endereço IP público fica oculta. O valor padrão é **false**.
- Se `options.hideDomainNameLabel` é definido como **true**, a caixa de texto do rótulo de nome de domínio fica oculta. O valor padrão é **false**.
- Se `options.hideExisting` é true, o usuário não pode escolher um endereço IP público existente. O valor padrão é **false**.
- Para `zone`, os endereços IP públicos estão disponíveis somente para endereços IP públicos para a zona especificada ou zona resiliente.

## <a name="sample-output"></a>Saída de exemplo
Se o usuário não seleciona nenhum endereço IP público,a seguinte saída é esperada:
```json
{
  "newOrExistingOrNone": "none"
}
```

Se o usuário seleciona um endereço IP novo ou existente,a seguinte saída é esperada:
```json
{
  "name": "ip01",
  "resourceGroup": "rg01",
  "domainNameLabel": "mydomain",
  "publicIPAllocationMethod": "Dynamic",
  "newOrExistingOrNone": "new"
}
```
- Quando `options.hideNone` for especificado como **true**, `newOrExistingOrNone` terá somente o valor **new** ou **existing**.
- Quando `options.hideDomainNameLabel` for especificado como **true**, `domainNameLabel` não será declarado.

## <a name="next-steps"></a>Próximas etapas
* Para obter uma introdução à criação de definições de interface do usuário, consulte [Introdução ao CreateUiDefinition](create-uidefinition-overview.md).
* Para obter uma descrição das propriedades comuns em elementos de interface do usuário, consulte [Elementos de CreateUiDefinition](create-uidefinition-elements.md).
