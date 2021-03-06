---
title: 'Tutorial: Integração do Azure Active Directory com o Mimecast Admin Console | Microsoft Docs'
description: Saiba como configurar o logon único entre o Azure Active Directory e o Mimecast Admin Console.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 81c50614-f49b-4bbc-97d5-3cf77154305f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2017
ms.author: jeedes
ms.openlocfilehash: bef0312a3ee44a441f44eb2ae7e4292b966cec3f
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-azure-active-directory-integration-with-mimecast-admin-console"></a>Tutorial: Integração do Azure Active Directory com o Mimecast Admin Console

Neste tutorial, você aprende a integrar o Mimecast Admin Console ao Azure AD (Azure Active Directory).

A integração do Mimecast Admin Console ao Azure AD oferece os seguintes benefícios:

- No Azure AD, é possível controlar quem tem acesso ao Mimecast Admin Console.
- Você pode permitir que seus usuários façam logon automaticamente no Mimecast Admin Console usando logon único com suas contas do Azure AD.
- Você pode gerenciar suas contas em um único local central – o portal do Azure.

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao Azure AD, consulte [o que é o acesso a aplicativos e logon único com o Azure Active Directory](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>pré-requisitos

Para configurar a integração do Azure AD ao Mimecast Admin Console, você precisa dos seguintes itens:

- Uma assinatura do AD do Azure
- Uma assinatura do Mimecast Admin Console com logon único habilitado

> [!NOTE]
> Para testar as etapas deste tutorial, nós não recomendamos o uso de um ambiente de produção.

Para testar as etapas deste tutorial, você deve seguir estas recomendações:

- Não use o ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, você pode [obter uma versão de avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, você testará o logon único do Azure AD em um ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionar o Mimecast Admin Console por meio da galeria
2. configurar e testar o logon único do AD do Azure

## <a name="adding-mimecast-admin-console-from-the-gallery"></a>Adicionar o Mimecast Admin Console por meio da galeria
Para configurar a integração do Mimecast Admin Console com o Azure AD, você precisará adicionar o Mimecast Admin Console à sua lista de aplicativos SaaS gerenciados por meio da galeria.

**Para adicionar o Mimecast Admin Console por meio da galeria, execute as seguintes etapas:**

1. No **[Portal do Azure](https://portal.azure.com)**, no painel navegação à esquerda, clique no ícone **Azure Active Directory**. 

    ![O botão Azure Active Directory][1]

2. Navegue até **aplicativos empresariais**. Em seguida, vá para **todos os aplicativos**.

    ![A folha Aplicativos empresariais][2]
    
3. Clique no botão **Novo aplicativo** na parte superior da caixa de diálogo para adicionar o novo aplicativo.

    ![O botão Novo aplicativo][3]

4. Na caixa de pesquisa, digite **Mimecast Admin Console**, selecione **Mimecast Admin Console** no painel de resultados e, depois, clique no botão **Adicionar** para adicionar o aplicativo.

    ![Mimecast Admin Console na lista de resultados](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar logon único do Azure AD

Nesta seção, você configura e testa o logon único do Azure AD com o Mimecast Admin Console, com base em um usuário de teste chamado “Brenda Fernandes”.

Para que o logon único funcione, o Azure AD precisa saber qual usuário do Mimecast Admin Console é equivalente a um usuário do Azure AD. Em outras palavras, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado do Mimecast Admin Console.

No Mimecast Admin Console, atribua o valor do **nome de usuário** no Azure AD como o valor do **Nome de usuário** para estabelecer a relação de vínculo.

Para configurar e testar o logon único do Azure AD com o Mimecast Admin Console, você precisa concluir os seguintes blocos de construção:

1. **[Configurar o logon único do Azure AD](#configure-azure-ad-single-sign-on)** – para habilitar seus usuários a usar esse recurso.
2. **[Criar um usuário de teste do Azure AD](#create-an-azure-ad-test-user)** – para testar o logon único do Azure AD com Brenda Fernandes.
3. **[Criando um usuário de teste do Mimecast Admin Console](#create-a-mimecast-admin-console-test-user)** – para ter um equivalente de Brenda Fernandes no Mimecast Admin Console que esteja vinculado à representação de usuário do Azure AD.
4. **[Atribuir o usuário de teste do Azure AD](#assign-the-azure-ad-test-user)** – para permitir que Brenda Fernandes use o logon único do Azure AD.
5. **[Teste o logon único](#test-single-sign-on)** – para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o logon único do Azure AD

Nesta seção, você habilita o logon único do Azure AD no Portal do Azure e configura o logon único no aplicativo Mimecast Admin Console.

**Para configurar o logon único do Azure AD com o Mimecast Admin Console, realize as seguintes etapas:**

1. No Portal do Azure, na página de integração do aplicativo **Mimecast Admin Console**, clique em **Logon único**.

    ![Link Configurar logon único][4]

2. Na caixa de diálogo **Logon único**, selecione **Modo** como **Logon baseado em SAML** para habilitar o logon único.
 
    ![Caixa de diálogo Logon único](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_samlbase.png)

3. Na seção **Domínio e URLs do Mimecast Admin Console**, execute as seguintes etapas:

    ![Informações de logon único de Domínio e URLs do Mimecast Admin Console](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_url.png)

    Na caixa de texto **URL de Logon**, digite a URL:
    | |
    | -- |
    | `https://webmail-uk.mimecast.com`|
    | `https://webmail-us.mimecast.com`|

    > [!NOTE] 
    > A URL de logon é específica para a região.

4. Na seção **Certificado de Autenticação do SAML**, clique em **Certificado (Base64)** e, em seguida, salve o arquivo do certificado no computador.

    ![O link de download do Certificado](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_certificate.png) 

5. Clique no botão **Salvar** .

    ![Botão Salvar em Configurar Logon Único](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_400.png)

6. Na seção **Configuração do Mimecast Admin Console**, clique em **Configurar o Mimecast Admin Console** para abrir a janela **Configurar logon**. Copie a **ID da Entidade SAML e a URL do Serviço de Logon Único SAML** da **seção Referência Rápida.**

    ![Configuração do Mimecast Admin Console](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_configure.png) 

7. Em outra janela do navegador da Web, faça logon em seu Mimecast Admin Console como um administrador.

8. Vá para **Serviços \> Aplicativo**.

    ![Serviços](./media/active-directory-saas-mimecast-admin-console-tutorial/ic794998.png "Serviços")

9. Clique em **Perfis de Autenticação**.

    ![Perfis de Autenticação](./media/active-directory-saas-mimecast-admin-console-tutorial/ic794999.png "Perfis de Autenticação")
    
10. Clique em **Novo Perfil de Autenticação**.

    ![Novos Perfis de Autenticação](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795000.png "Novos Perfis de Autenticação")

11. Na seção **Perfil de Autenticação** , realize as seguintes etapas:

    ![Perfil de Autenticação](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795015.png "Perfil de Autenticação")
    
    a. Na caixa de texto **Descrição** , digite um nome para a sua configuração.
    
    b. Selecione **Impor Autenticação SAML para o Mimecast Admin Console**.
    
    c. Como **Provedor**, selecione **Azure Active Directory**.
    
    d. Cole a **ID de Entidade do SAML** que você copiou do Portal do Azure na caixa de texto **URL do Emissor**.
    
    e. Cole a **URL do Serviço de Logon Único SAML**, que você copiou do Portal do Azure na a caixa de texto **URL de Logon**.

    f. Cole a **URL do Serviço de Logon Único SAML**, que você copiou do Portal do Azure na a caixa de texto **URL de Logoff**.
    
    >[!NOTE]
    >O valor da URL de logon e da URL de logoff para o Mimecast Admin Console são as mesmas.
    
    g. Abra seu certificado codificado em base 64 baixado do Portal do Azure no bloco de notas, remova a primeira linha ("*--*") e a última linha ("*--*"), copie o conteúdo restante para a área de transferência e cole-o na caixa de texto **Certificado de Provedor de Identidade (Metadados)**.
    
    h. Selecione **Permitir Logon Único**.
    
    i. Clique em **Salvar**.

> [!TIP]
> É possível ler uma versão concisa dessas instruções no [Portal do Azure](https://portal.azure.com), enquanto você estiver configurando o aplicativo!  Depois de adicionar esse aplicativo da seção **Active Directory > Aplicativos Empresariais**, basta clicar na guia **Logon Único** e acessar a documentação inserida por meio da seção **Configuração** na parte inferior. Saiba mais sobre a funcionalidade de documentação inserida aqui: [Documentação inserida do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985) 

### <a name="create-an-azure-ad-test-user"></a>Criar um usuário de teste do Azure AD

O objetivo desta seção é criar um usuário de teste no Portal do Azure chamado Brenda Fernandes.

   ![Criar um usuário de teste do Azure AD][100]

**Para criar um usuário de teste no AD do Azure, execute as seguintes etapas:**

1. No portal do Azure, no painel esquerdo, clique no botão **Azure Active Directory**.

    ![O botão Azure Active Directory](./media/active-directory-saas-mimecast-admin-console-tutorial/create_aaduser_01.png)

2. Para exibir a lista de usuários, acesse **Usuários e grupos** e, depois, clique em **Todos os usuários**.

    ![Os links “Usuários e grupos” e “Todos os usuários”](./media/active-directory-saas-mimecast-admin-console-tutorial/create_aaduser_02.png)

3. Para abrir a caixa de diálogo **Usuário**, clique em **Adicionar** na parte superior da caixa de diálogo **Todos os Usuários**.

    ![O botão Adicionar](./media/active-directory-saas-mimecast-admin-console-tutorial/create_aaduser_03.png)

4. Na caixa de diálogo **Usuário**, execute as seguintes etapas:

    ![A caixa de diálogo Usuário](./media/active-directory-saas-mimecast-admin-console-tutorial/create_aaduser_04.png)

    a. Na caixa **Nome**, digite **BrendaFernandes**.

    b. Na caixa **Nome de usuário**, digite o endereço de email do usuário Brenda Fernandes.

    c. Marque a caixa de seleção **Mostrar Senha** e, em seguida, anote o valor exibido na caixa **Senha**.

    d. Clique em **Criar**.
 
### <a name="create-a-mimecast-admin-console-test-user"></a>Criar um usuário de teste do Mimecast Admin Console

Para permitir que os usuários do Azure AD façam logon no Mimecast Admin Console, eles devem ser provisionados no Mimecast Admin Console. No caso do Mimecast Admin Console, o provisionamento é uma tarefa manual.

* Você precisa registrar um domínio antes de criar usuários.

**Para configurar o provisionamento de usuários, execute as seguintes etapas:**

1. Faça logon no **Mimecast Admin Console** como administrador.
2. Vá para **Diretórios \> Interno**.
   
   ![Diretórios](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795003.png "Diretórios")
3. Clique em **Registrar Novo Domínio**.
   
   ![Registrar Novo Domínio](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795004.png "Registrar Novo Domínio")
4. Depois de criar o novo domínio, clique em **Novo Endereço**.
   
   ![Novo Endereço](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795005.png "Novo Endereço")
5. Na caixa de diálogo do novo endereço, execute as seguintes etapas:
   
   ![Salvar](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795006.png "Salvar")
   
   a. Digite os atributos de **Endereço de Email**, **Nome Global**, **Senha** e **Confirmar Senha** de uma conta válida do Azure AD que você deseja provisionar nas caixas de texto relacionadas.

   b. Clique em **Salvar**.

>[!NOTE]
>É possível usar qualquer outra ferramenta de criação da conta de usuário do Mimecast Admin Console ou as APIs fornecidas pelo Mimecast Admin Console para provisionar as contas de usuário do Azure AD. 

### <a name="assign-the-azure-ad-test-user"></a>Atribuir o usuário de teste do Azure AD

Nesta seção, você permite que Brenda Fernandes use o logon único do Azure concedendo a ela acesso ao Mimecast Admin Console.

![Atribuir a função de usuário][200] 

**Para atribuir Brenda Fernandes ao Mimecast Admin Console, execute as seguintes etapas:**

1. No Portal do Azure, abra a exibição de aplicativos e, em seguida, navegue até a exibição de diretório e vá para **Aplicativos Empresariais** e clique em **Todos os aplicativos**.

    ![Atribuir usuário][201] 

2. Na lista de aplicativos, selecione **Mimecast Admin Console**.

    ![O link do Mimecast Admin Console na lista de Aplicativos](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_app.png)  

3. No menu à esquerda, clique em **usuários e grupos**.

    ![O link “Usuários e grupos”][202]

4. Clique no botão **Adicionar**. Em seguida, selecione **usuários e grupos** na **Adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar Atribuição][203]

5. Em **usuários e grupos** caixa de diálogo, selecione **Britta Simon** na lista de usuários.

6. Clique em **selecione** botão **usuários e grupos** caixa de diálogo.

7. Clique em **atribuir** botão **Adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar logon único

Nesta seção, você testará sua configuração de logon único do Azure AD usando o Painel de Acesso.

Quando você clicar no bloco do Mimecast Admin Console no Painel de Acesso, deverá ser conectado automaticamente ao aplicativo Mimecast Admin Console.
Para saber mais sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_203.png

