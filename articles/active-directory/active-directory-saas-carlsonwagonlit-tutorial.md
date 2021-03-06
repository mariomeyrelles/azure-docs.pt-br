---
title: 'Tutorial: integração do Azure Active Directory ao Carlson Wagonlit Travel | Microsoft Docs'
description: Saiba como configurar o logon único entre o Azure Active Directory e o Carlson Wagonlit Travel.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 2745e165-94ab-43b1-970a-4547b4e5b501
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/19/2018
ms.author: jeedes
ms.openlocfilehash: 80873f9dfe94a5e83865b3617ad8dba3198a2dd7
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-azure-active-directory-integration-with-carlson-wagonlit-travel"></a>Tutorial: integração do Azure Active Directory ao Carlson Wagonlit Travel

Neste tutorial, você aprenderá a integrar o Carlson Wagonlit Travel ao Azure AD (Azure Active Directory).

A integração do Carlson Wagonlit Travel ao Azure AD oferece os seguintes benefícios:

- É possível controlar no Azure AD quem tem acesso ao Carlson Wagonlit Travel.
- Você pode permitir que seus usuários façam logon automaticamente no Carlson Wagonlit Travel (logon único) com as contas do Azure AD deles.
- Você pode gerenciar suas contas em um único local central – o portal do Azure.

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao Azure AD, consulte [o que é o acesso a aplicativos e logon único com o Azure Active Directory](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>pré-requisitos

Para configurar a integração do Azure AD ao Carlson Wagonlit Travel, você precisa dos seguintes itens:

- Uma assinatura do AD do Azure
- Uma assinatura com logon único habilitado para o Carlson Wagonlit Travel

> [!NOTE]
> Para testar as etapas deste tutorial, nós não recomendamos o uso de um ambiente de produção.

Para testar as etapas deste tutorial, você deve seguir estas recomendações:

- Não use o ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, você pode [obter uma versão de avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, você testará o logon único do Azure AD em um ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionar Carlson Wagonlit Travel da Galeria
2. configurar e testar o logon único do AD do Azure

## <a name="adding-carlson-wagonlit-travel-from-the-gallery"></a>Adicionar Carlson Wagonlit Travel da Galeria
Para configurar a integração do Carlson Wagonlit Travel ao Azure AD, você precisa adicionar o Carlson Wagonlit Travel por meio da galeria à sua lista de aplicativos SaaS gerenciados.

**Para adicionar o Carlson Wagonlit Travel da galeria, execute as seguintes etapas:**

1. No **[Portal do Azure](https://portal.azure.com)**, no painel navegação à esquerda, clique no ícone **Azure Active Directory**. 

    ![O botão Azure Active Directory][1]

2. Navegue até **aplicativos empresariais**. Em seguida, vá para **todos os aplicativos**.

    ![A folha Aplicativos empresariais][2]
    
3. Clique no botão **Novo aplicativo** na parte superior da caixa de diálogo para adicionar o novo aplicativo.

    ![O botão Novo aplicativo][3]

4. Na caixa de pesquisa, digite **Carlson Wagonlit Travel**, selecione **Carlson Wagonlit Travel** no painel de resultados e clique no botão **Adicionar** para adicionar o aplicativo.

    ![Carlson Wagonlit Travel na lista de resultados](./media/active-directory-saas-carlsonwagonlittravel-tutorial/tutorial_carlsonwagonlittravel_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar logon único do Azure AD

Nesta seção, você configurará e testará o logon único do Azure AD com o Carlson Wagonlit Travel, com base em uma usuária de teste chamada "Brenda Fernandes".

Para que o logon único funcione, o Azure AD precisa saber qual usuário do Carlson Wagonlit Travel é equivalente a um usuário do Azure AD. Em outras palavras, é necessário estabelecer uma relação de vinculação entre um usuário do Azure AD e o usuário relacionado do Carlson Wagonlit Travel.

No Carlson Wagonlit Travel, atribua o valor do **nome de usuário** no Azure AD como o valor do **Nome de usuário** para estabelecer a relação de vínculo.

Para configurar e testar o logon único do Azure AD com o Carlson Wagonlit Travel, você precisa concluir os seguintes blocos de construção:

1. **[Configurar o logon único do Azure AD](#configure-azure-ad-single-sign-on)** – para habilitar seus usuários a usar esse recurso.
2. **[Criar um usuário de teste do Azure AD](#create-an-azure-ad-test-user)** – para testar o logon único do Azure AD com Brenda Fernandes.
3. **[Criar um usuário de teste Carlson Wagonlit Travel](#create-a-carlson-wagonlit-travel-test-user)** – para ter um equivalente de Brenda Fernandes no Carlson Wagonlit Travel vinculado à representação do usuário do Azure AD.
4. **[Atribuir o usuário de teste do Azure AD](#assign-the-azure-ad-test-user)** – para permitir que Brenda Fernandes use o logon único do Azure AD.
5. **[Teste o logon único](#test-single-sign-on)** – para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o logon único do Azure AD

Nesta seção, você habilitará o logon único do Azure AD no Portal do Azure e configurará o logon único no aplicativo Carlson Wagonlit Travel.

**Para configurar o logon único do Azure AD com o Carlson Wagonlit Travel, execute as seguintes etapas:**

1. No portal do Azure, na página de integração de aplicativos do **Carlson Wagonlit Travel**, clique em **Logon único**.

    ![Link Configurar logon único][4]

2. Na caixa de diálogo **Logon único**, selecione **Modo** como **Logon baseado em SAML** para habilitar o logon único.
 
    ![Caixa de diálogo Logon único](./media/active-directory-saas-carlsonwagonlittravel-tutorial/tutorial_carlsonwagonlittravel_samlbase.png)

3. Na seção **URLs e Domínio do Carlson Wagonlit Travel**, siga as etapas abaixo:

    ![Informações de logon único em Domínio e URLs do Carlson Wagonlit Travel](./media/active-directory-saas-carlsonwagonlittravel-tutorial/tutorial_carlsonwagonlittravel_url.png)

    Na caixa de texto **Identificador**, digite o valor: `cwt-stage`

4. Na seção **Certificado de Autenticação SAML**, clique em **Metadados XML** e, em seguida, salve o arquivo de metadados em seu computador.

    ![O link de download do Certificado](./media/active-directory-saas-carlsonwagonlittravel-tutorial/tutorial_carlsonwagonlittravel_certificate.png) 

5. Clique no botão **Salvar** .

    ![Botão Salvar em Configurar Logon Único](./media/active-directory-saas-carlsonwagonlittravel-tutorial/tutorial_general_400.png)

6. Para configurar o logon único no lado do **Carlson Wagonlit Travel**, é necessário enviar o **XML de metadados** baixado para a [equipe de suporte do Carlson Wagonlit Travel](http://www.carlsonwagonlit.in/content/cwt/in/en/technical-assistance.html). Eles definem essa configuração para ter a conexão de SSO de SAML definida corretamente em ambos os lados.

> [!TIP]
> É possível ler uma versão concisa dessas instruções no [Portal do Azure](https://portal.azure.com), enquanto você estiver configurando o aplicativo!  Depois de adicionar esse aplicativo da seção **Active Directory > Aplicativos Empresariais**, basta clicar na guia **Logon Único** e acessar a documentação inserida por meio da seção **Configuração** na parte inferior. Saiba mais sobre a funcionalidade de documentação inserida aqui: [Documentação inserida do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Criar um usuário de teste do Azure AD

O objetivo desta seção é criar um usuário de teste no Portal do Azure chamado Brenda Fernandes.

   ![Criar um usuário de teste do Azure AD][100]

**Para criar um usuário de teste no AD do Azure, execute as seguintes etapas:**

1. No portal do Azure, no painel esquerdo, clique no botão **Azure Active Directory**.

    ![O botão Azure Active Directory](./media/active-directory-saas-carlsonwagonlittravel-tutorial/create_aaduser_01.png)

2. Para exibir a lista de usuários, acesse **Usuários e grupos** e, depois, clique em **Todos os usuários**.

    ![Os links “Usuários e grupos” e “Todos os usuários”](./media/active-directory-saas-carlsonwagonlittravel-tutorial/create_aaduser_02.png)

3. Para abrir a caixa de diálogo **Usuário**, clique em **Adicionar** na parte superior da caixa de diálogo **Todos os Usuários**.

    ![O botão Adicionar](./media/active-directory-saas-carlsonwagonlittravel-tutorial/create_aaduser_03.png)

4. Na caixa de diálogo **Usuário**, execute as seguintes etapas:

    ![A caixa de diálogo Usuário](./media/active-directory-saas-carlsonwagonlittravel-tutorial/create_aaduser_04.png)

    a. Na caixa **Nome**, digite **BrendaFernandes**.

    b. Na caixa **Nome de usuário**, digite o endereço de email do usuário Brenda Fernandes.

    c. Marque a caixa de seleção **Mostrar Senha** e, em seguida, anote o valor exibido na caixa **Senha**.

    d. Clique em **Criar**.
 
### <a name="create-a-carlson-wagonlit-travel-test-user"></a>Criar um usuário de teste Carlson Wagonlit Travel

Nesta seção, você criará um usuário chamado Brenda Fernandes no Carlson Wagonlit Travel. Trabalhar com [a equipe de suporte Carlson Wagonlit Travel](http://www.carlsonwagonlit.in/content/cwt/in/en/technical-assistance.html) para adicionar os usuários na plataforma Carlson Wagonlit Travel. Os usuários devem ser criados e ativados antes de usar o logon único. 

### <a name="assign-the-azure-ad-test-user"></a>Atribuir o usuário de teste do Azure AD

Nesta seção, você permitirá que Brenda Fernandes use o logon único do Azure concedendo acesso ao Carlson Wagonlit Travel.

![Atribuir a função de usuário][200] 

**Para atribuir Brenda Fernandes ao Carlson Wagonlit Travel, execute as seguintes etapas:**

1. No Portal do Azure, abra a exibição de aplicativos e, em seguida, navegue até a exibição de diretório e vá para **Aplicativos Empresariais** e clique em **Todos os aplicativos**.

    ![Atribuir usuário][201] 

2. Na lista de aplicativos, selecione **Carlson Wagonlit Travel**.

    ![O link Carlson Wagonlit Travel na lista de aplicativos](./media/active-directory-saas-carlsonwagonlittravel-tutorial/tutorial_carlsonwagonlittravel_app.png)  

3. No menu à esquerda, clique em **usuários e grupos**.

    ![O link “Usuários e grupos”][202]

4. Clique no botão **Adicionar**. Em seguida, selecione **usuários e grupos** na **Adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar Atribuição][203]

5. Em **usuários e grupos** caixa de diálogo, selecione **Britta Simon** na lista de usuários.

6. Clique em **selecione** botão **usuários e grupos** caixa de diálogo.

7. Clique em **atribuir** botão **Adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar logon único

Nesta seção, você testará sua configuração de logon único do Azure AD usando o Painel de Acesso.

Ao clicar no bloco do Carlson Wagonlit Travel no Painel de Acesso, você deverá ser conectado automaticamente a seu aplicativo do Carlson Wagonlit Travel.
Para saber mais sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/active-directory-saas-carlsonwagonlittravel-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-carlsonwagonlittravel-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-carlsonwagonlittravel-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-carlsonwagonlittravel-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-carlsonwagonlittravel-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-carlsonwagonlittravel-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-carlsonwagonlittravel-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-carlsonwagonlittravel-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-carlsonwagonlittravel-tutorial/tutorial_general_203.png

