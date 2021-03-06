---
title: Usar o Conector do SharePoint Server em seus Aplicativos Lógicos | Microsoft Docs
description: Introdução ao uso do Conector do SharePoint Server em seus Aplicativos lógicos
services: logic-apps
documentationcenter: ''
author: ecfan
manager: anneta
editor: ''
tags: connectors
ms.assetid: 0238a060-d592-4719-b7a2-26064c437a1a
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2016
ms.author: estfan; ladocs
ms.openlocfilehash: d342b3c4f84c5dab212b9327d6a72759934d0ae5
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/16/2018
---
# <a name="get-started-with-the-sharepoint-connector"></a>Introdução ao conector do SharePoint
O Conector do SharePoint fornece uma maneira de trabalhar com listas do SharePoint.

Comece pela criação de um aplicativo lógico; veja [Criar um aplicativo lógico](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-a-connection-to-sharepoint"></a>Criar uma conexão com o SharePoint
Para usar o Conector do SharePoint, crie primeiro uma **conexão** e, então, forneça os detalhes dessas propriedades: 

| Propriedade | Obrigatório | DESCRIÇÃO |
| --- | --- | --- |
| A criptografia do token |sim |Fornecer credenciais do SharePoint |

Para se conectar ao **SharePoint**, insira sua identidade (nome de usuário e senha, credenciais do cartão inteligente e assim por diante). Depois de se autenticar, você poderá continuar e usar o Conector do SharePoint em seu aplicativo lógico. 

No designer do aplicativo lógico, use as etapas a seguir para entrar e criar a **conexão** que será usada em seu aplicativo lógico:

1. Insira SharePoint na caixa de pesquisa e aguarde até que a pesquisa retorne todas as entradas com SharePoint no nome:    
   ![Configurar o SharePoint][1]  
2. Selecione **SharePoint – Quando um arquivo é criado**   
3. Selecione **Entrar no SharePoint**:   
   ![Configurar o SharePoint][2]    
4. Forneça suas credenciais do SharePoint para entrar e se autenticar com o SharePoint    
   ![Configurar o SharePoint][3]     
5. Após a conclusão da autenticação, você é redirecionado ao seu aplicativo lógico para concluí-lo por meio da configuração da caixa de diálogo **Quando um arquivo é criado** do SharePoint.          
   ![Configurar o SharePoint][4]  
6. Em seguida, é possível adicionar outros gatilhos e outras ações necessárias para concluir seu aplicativo lógico.   
7. Salve seu trabalho selecionando **Salvar** no menu (na parte superior).

## <a name="connector-specific-details"></a>Detalhes específicos do conector

Exiba os gatilhos e ações definidos no swagger e também os limites nos [detalhes do conector](/connectors/sharepoint/).

## <a name="more-connectors"></a>Mais conectores
Volte para a [Lista de APIs](apis-list.md).

[1]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig1.png  
[2]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig2.png 
[3]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig3.png
[4]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig4.png
[5]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig5.png
