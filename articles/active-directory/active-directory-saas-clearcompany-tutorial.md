---
title: 'Tutorial: Integração do Azure Active Directory ao ClearCompany | Microsoft Docs'
description: Saiba como configurar o logon único entre o Azure Active Directory e o ClearCompany.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 2819da18-c7eb-43cf-aac3-1403a540bf6e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: jeedes
ms.openlocfilehash: ab2628c1caa0350e95e3c11461c3d1bad0659470
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-azure-active-directory-integration-with-clearcompany"></a>Tutorial: integração do Azure Active Directory ao ClearCompany

Neste tutorial, você aprenderá a integrar o ClearCompany ao Azure AD (Azure Active Directory).

A integração do ClearCompany ao Azure AD oferece os seguintes benefícios:

- no Azure AD, é possível controlar quem tem acesso ao ClearCompany.
- Você pode permitir que seus usuários façam logon automaticamente no ClearCompany (logon único) com as respectivas contas do Azure AD.
- Você pode gerenciar suas contas em um único local central – o portal do Azure.

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao Azure AD, consulte [o que é o acesso a aplicativos e logon único com o Azure Active Directory](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>pré-requisitos

Para configurar a integração do Azure AD ao ClearCompany, você precisará dos seguintes itens:

- Uma assinatura do AD do Azure
- Uma assinatura do ClearCompany habilitada para logon único

> [!NOTE]
> Para testar as etapas deste tutorial, nós não recomendamos o uso de um ambiente de produção.

Para testar as etapas deste tutorial, você deve seguir estas recomendações:

- Não use o ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, você pode [obter uma versão de avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, você testará o logon único do Azure AD em um ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando o ClearCompany da galeria
2. configurar e testar o logon único do AD do Azure

## <a name="adding-clearcompany-from-the-gallery"></a>Adicionando o ClearCompany da galeria
Para configurar a integração do ClearCompany ao Azure AD, você precisará adicionar o ClearCompany da galeria à sua lista de aplicativos SaaS gerenciados.

**Para adicionar o ClearCompany por meio da galeria, execute as seguintes etapas:**

1. No **[Portal do Azure](https://portal.azure.com)**, no painel navegação à esquerda, clique no ícone **Azure Active Directory**. 

    ![O botão Azure Active Directory][1]

2. Navegue até **aplicativos empresariais**. Em seguida, vá para **todos os aplicativos**.

    ![A folha Aplicativos empresariais][2]
    
3. Clique no botão **Novo aplicativo** na parte superior da caixa de diálogo para adicionar o novo aplicativo.

    ![O botão Novo aplicativo][3]

4. Na caixa de pesquisa, digite **ClearCompany**, selecione **ClearCompany** no painel de resultados e, em seguida, clique no botão **Adicionar** para adicionar o aplicativo.

    ![ClearCompany na lista de resultados](./media/active-directory-saas-clearcompany-tutorial/tutorial_clearcompany_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar logon único do Azure AD

Nesta seção, você configurará e testará o logon único do Azure AD com o ClearCompany, com base em uma usuária de teste chamada "Brenda Fernandes".

Para que o logon único funcione, o Azure AD precisa saber qual usuário do ClearCompany é equivalente a um usuário do Azure AD. Em outras palavras, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado do ClearCompany.

No ClearCompany, atribua o valor do **nome de usuário** no Azure AD como o valor do **Nome de usuário** para estabelecer a relação de vínculo.

Para configurar e testar o logon único do Azure AD com o ClearCompany, você precisará concluir os seguintes blocos de construção:

1. **[Configurar o logon único do Azure AD](#configure-azure-ad-single-sign-on)** – para habilitar seus usuários a usar esse recurso.
2. **[Criar um usuário de teste do Azure AD](#create-an-azure-ad-test-user)** – para testar o logon único do Azure AD com Brenda Fernandes.
3. **[Criar um usuário de teste do ClearCompany](#create-a-clearcompany-test-user)** – para ter um equivalente de Brenda Fernandes no ClearCompany vinculado à representação do usuário do Azure AD.
4. **[Atribuir o usuário de teste do Azure AD](#assign-the-azure-ad-test-user)** – para permitir que Brenda Fernandes use o logon único do Azure AD.
5. **[Teste o logon único](#test-single-sign-on)** – para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o logon único do Azure AD

Nesta seção, você habilita o logon único do Azure AD no Portal do Azure e configura o logon único no aplicativo ClearCompany.

**Para configurar o logon único do Azure AD com o ClearCompany, realize as seguintes etapas:**

1. No Portal do Azure, na página de integração do aplicativo **ClearCompany**, clique em **Logon único**.

    ![Link Configurar logon único][4]

2. Na caixa de diálogo **Logon único**, selecione **Modo** como **Logon baseado em SAML** para habilitar o logon único.
 
    ![Caixa de diálogo Logon único](./media/active-directory-saas-clearcompany-tutorial/tutorial_clearcompany_samlbase.png)

3. Na seção **Domínio e URLs do ClearCompany**, realize as seguintes etapas se desejar configurar o aplicativo no modo iniciado pelo IDP:

    ![Informações de logon único de Domínio e URLs do ClearCompany](./media/active-directory-saas-clearcompany-tutorial/tutorial_clearcompany_url1.png)

    Na caixa de texto **Identificador**, digite a URL: `https://api.clearcompany.com`

4. Marque **Mostrar configurações avançadas de URL** e realize a seguinte etapa se quiser configurar o aplicativo no modo iniciado pelo **SP**:

    ![Informações de logon único de Domínio e URLs do ClearCompany](./media/active-directory-saas-clearcompany-tutorial/tutorial_clearcompany_url2.png)

    Na caixa de texto **URL de Logon**, digite uma URL usando o seguinte padrão: `https://<companyname>.clearcompany.com`
    
    > [!NOTE] 
    > O valor da URL de logon não é um valor real. Atualize esse valor com a URL de Logon real. Contate a [equipe de suporte ao cliente do ClearCompany](http://www.clearcompany.com/support) para obter esse valor. 

5. Na seção **Certificado de Autenticação do SAML**, clique em **Certificado (Base64)** e, em seguida, salve o arquivo do certificado no computador.

    ![O link de download do Certificado](./media/active-directory-saas-clearcompany-tutorial/tutorial_clearcompany_certificate.png) 

6. Clique no botão **Salvar** .

    ![Botão Salvar em Configurar Logon Único](./media/active-directory-saas-clearcompany-tutorial/tutorial_general_400.png)
    
7. Na seção **Configuração do ClearCompany**, clique em **Configurar ClearCompany** para abrir a janela **Configurar logon**. Copie a **URL de serviço de logon único SAML** da **seção de Referência Rápida.**

    ![Configuração do ClearCompany](./media/active-directory-saas-clearcompany-tutorial/tutorial_clearcompany_configure.png) 

8. Para configurar o logon único no lado do **ClearCompany**, é necessário enviar o **Certificado (Base64)** baixado e a **URL do Serviço de Logon Único SAML** para a [equipe de suporte do ClearCompany](http://www.clearcompany.com/support). Eles definem essa configuração para ter a conexão de SSO de SAML definida corretamente em ambos os lados.

> [!TIP]
> É possível ler uma versão concisa dessas instruções no [Portal do Azure](https://portal.azure.com), enquanto você estiver configurando o aplicativo!  Depois de adicionar esse aplicativo da seção **Active Directory > Aplicativos Empresariais**, basta clicar na guia **Logon Único** e acessar a documentação inserida por meio da seção **Configuração** na parte inferior. Saiba mais sobre a funcionalidade de documentação inserida aqui: [Documentação inserida do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Criar um usuário de teste do Azure AD

O objetivo desta seção é criar um usuário de teste no Portal do Azure chamado Brenda Fernandes.

   ![Criar um usuário de teste do Azure AD][100]

**Para criar um usuário de teste no AD do Azure, execute as seguintes etapas:**

1. No portal do Azure, no painel esquerdo, clique no botão **Azure Active Directory**.

    ![O botão Azure Active Directory](./media/active-directory-saas-clearcompany-tutorial/create_aaduser_01.png)

2. Para exibir a lista de usuários, acesse **Usuários e grupos** e, depois, clique em **Todos os usuários**.

    ![Os links “Usuários e grupos” e “Todos os usuários”](./media/active-directory-saas-clearcompany-tutorial/create_aaduser_02.png)

3. Para abrir a caixa de diálogo **Usuário**, clique em **Adicionar** na parte superior da caixa de diálogo **Todos os Usuários**.

    ![O botão Adicionar](./media/active-directory-saas-clearcompany-tutorial/create_aaduser_03.png)

4. Na caixa de diálogo **Usuário**, execute as seguintes etapas:

    ![A caixa de diálogo Usuário](./media/active-directory-saas-clearcompany-tutorial/create_aaduser_04.png)

    a. Na caixa **Nome**, digite **BrendaFernandes**.

    b. Na caixa **Nome de usuário**, digite o endereço de email do usuário Brenda Fernandes.

    c. Marque a caixa de seleção **Mostrar Senha** e, em seguida, anote o valor exibido na caixa **Senha**.

    d. Clique em **Criar**.
 
### <a name="create-a-clearcompany-test-user"></a>Criar um usuário de teste do ClearCompany

Nesta seção, você criará um usuário chamado Brenda Fernandes no ClearCompany. Trabalhe com a [equipe de suporte do ClearCompany](http://www.clearcompany.com/support) para adicionar os usuários à plataforma ClearCompany. Os usuários devem ser criados e ativados antes de usar o logon único.

### <a name="assign-the-azure-ad-test-user"></a>Atribuir o usuário de teste do Azure AD

Nesta seção, você permite que Brenda Fernandes use o logon único do Azure concedendo a ela acesso ao ClearCompany.

![Atribuir a função de usuário][200] 

**Para atribuir Brenda Fernandes ao ClearCompany, execute as seguintes etapas:**

1. No Portal do Azure, abra a exibição de aplicativos e, em seguida, navegue até a exibição de diretório e vá para **Aplicativos Empresariais** e clique em **Todos os aplicativos**.

    ![Atribuir usuário][201] 

2. Na lista de aplicativos, selecione **ClearCompany**.

    ![O link do ClearCompany na lista de Aplicativos](./media/active-directory-saas-clearcompany-tutorial/tutorial_clearcompany_app.png)  

3. No menu à esquerda, clique em **usuários e grupos**.

    ![O link “Usuários e grupos”][202]

4. Clique no botão **Adicionar**. Em seguida, selecione **usuários e grupos** na **Adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar Atribuição][203]

5. Em **usuários e grupos** caixa de diálogo, selecione **Britta Simon** na lista de usuários.

6. Clique em **selecione** botão **usuários e grupos** caixa de diálogo.

7. Clique em **atribuir** botão **Adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar logon único

Nesta seção, você testará sua configuração de logon único do Azure AD usando o Painel de Acesso.

Quando você clica no bloco ClearCompany no Painel de Acesso, você deve fazer logon automaticamente no seu aplicativo ClearCompany.
Para saber mais sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/active-directory-saas-clearcompany-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-clearcompany-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-clearcompany-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-clearcompany-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-clearcompany-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-clearcompany-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-clearcompany-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-clearcompany-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-clearcompany-tutorial/tutorial_general_203.png

