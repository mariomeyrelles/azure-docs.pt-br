---
title: Introdução ao Serviço Conectado do Key Vault no Visual Studio (projetos ASP.NET) | Microsoft Docs
description: Use este tutorial para aprender a adicionar suporte do Key Vault para um aplicativo Web ASP.NET ou ASP.NET Core.
services: key-vault
author: ghogen
manager: douge
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure
ms.topic: conceptual
ms.date: 04/15/2018
ms.author: ghogen
ms.openlocfilehash: cd305801f10c899682aa6d751e48f30b6e8303fa
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/07/2018
---
# <a name="get-started-with-key-vault-connected-service-in-visual-studio-aspnet-projects"></a>Introdução ao Serviço Conectado do Key Vault no Visual Studio (projetos ASP.NET)

> [!div class="op_single_selector"]
> - [Introdução](vs-key-vault-aspnet-get-started.md)
> - [O que aconteceu](vs-key-vault-aspnet-what-happened.md)

Este artigo fornece orientações adicionais depois de você ter adicionado o Key Vault a um projeto do ASP.NET MVC por meio do comando **Adicionar Serviços Conectados** do Visual Studio. Se ainda não tiver adicionado o serviço ao seu projeto, faça isso a qualquer momento seguindo as instruções em [Adicionar Key Vault ao seu aplicativo Web usando os Serviços Conectados do Visual Studio](vs-key-vault-add-connected-service.md).

Consulte [O que aconteceu com meu projeto do ASP.NET?](vs-key-vault-aspnet-core-what-happened.md) para as alterações feitas ao seu projeto ao adicionar o serviço conectado.

## <a name="after-you-connect"></a>Depois de se conectar

1. Adicione um segredo ao seu Key Vault no Azure. Para chegar ao local correto no portal, clique no link para **Gerenciar segredos armazenados neste Key Vault**. Se você fechou a página ou o projeto, poderá navegar no [Portal do Azure](https://portal.azure.com) escolhendo **Todos os serviços**. Em **Segurança**, escolha **Key Vault** e, em seguida, selecione o Key Vault que você acabou de criar.

   ![Navegar para o portal](media/vs-key-vault-add-connected-service/manage-secrets-link.jpg)

1. Na seção do Key Vault para o cofre de chaves que você criou, selecione **Segredos** e, em seguida, **Gerar/importar**.

   ![Gerar/Importar um segredo](media/vs-key-vault-add-connected-service/generate-secrets.jpg)

1. Digite um segredo, como **MySecret**, e dê a ele qualquer valor de cadeia de caracteres como um teste e clique no botão **Criar**.

   ![Criar um segredo](media/vs-key-vault-add-connected-service/create-a-secret.jpg)
 
1. (opcional) Insira outro segredo, mas, desta vez, coloque-o em uma categoria nomeando-o como **Segredos--MySecret**. Esta sintaxe especifica uma categoria **Segredos** que contém um segredo **MySecret**.

1. Modifique o web.config da seguinte maneira. As chaves são espaços reservados que serão substituídos pelo AzureKeyVault ConfigurationBuilder com os valores dos segredos no Key Vault.

   ```xml
     <appSettings configBuilders="AzureKeyVault">
       <add key="webpages:Version" value="3.0.0.0" />
       <add key="webpages:Enabled" value="false" />
       <add key="ClientValidationEnabled" value="true" />
       <add key="UnobtrusiveJavaScriptEnabled" value="true" />
       <add key="MySecret" value="dummy1"/>
       <add key="Secrets--MySecret" value="dummy2"/>
     </appSettings>
   ```

1. No HomeController, no método Sobre o controlador, adicione as linhas a seguir para recuperar o segredo e armazená-lo em ViewBag.
 
   ```csharp
            var secret = ConfigurationManager.AppSettings["MySecret"];
            var secret2 = ConfigurationManager.AppSettings["Secrets--MySecret"];
            ViewBag.Secret = $"Secret: {secret}";
            ViewBag.Secret2 = $"Secret2: {secret2}";
   ```

1. Na exibição About.cshtml, adicione o seguinte para exibir o valor do segredo (somente para teste).

   ```csharp
      <h3>@ViewBag.Secret</h3>
      <h3>@ViewBag.Secret2</h3>
   ```

Parabéns! Você confirmou que seu aplicativo Web pode usar o Key Vault para acessar os segredos armazenados com segurança.

## <a name="clean-up-resources"></a>Limpar recursos

Quando não for mais necessário, exclua o grupo de recursos. Isso exclui o Key Vault e os recursos relacionados. Para excluir o grupo de recursos pelo portal:

1. Insira o nome do grupo de recursos na caixa de pesquisa na parte superior do portal. Quando você vir o grupo de recursos usado neste início rápido nos resultados da pesquisa, selecione-o.
2. Selecione **Excluir grupo de recursos**.
3. Na caixa **DIGITE O NOME DO GRUPO DE RECURSOS:**, digite o nome do grupo de recursos e selecione **Excluir**.

# <a name="next-steps"></a>Próximas etapas

Saiba mais sobre o desenvolvimento com Key Vault no [Guia de Desenvolvedor do Key Vault](key-vault-developers-guide.md)