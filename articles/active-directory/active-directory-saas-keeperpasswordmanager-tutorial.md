---
title: 'Tutorial: Integração do Azure Active Directory ao Gerenciador de Senhas Protetor e Cofre Digital | Microsoft Docs'
description: Saiba como configurar o logon único entre o Azure Active Directory e o Gerenciador de Senhas Protetor e Cofre Digital.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: e1a98f6a-2dae-4734-bdbf-4fba742a61d2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 2c76edea4498e32363a3a4952056e69c012bd468
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-azure-active-directory-integration-with-keeper-password-manager--digital-vault"></a>Tutorial: integração do Azure Active Directory ao Gerenciador de Senhas Protetor e Cofre Digital

Neste tutorial, você aprenderá a integrar o Gerenciador de Senhas Protetor e o Cofre Digital ao Azure Active Directory (Azure AD).

Integrar do Gerenciador de Senhas Protetor e do Cofre Digital ao Azure AD oferece os seguintes benefícios:

- Você pode controlar no Azure AD quem tem acesso ao Gerenciador de Senhas Protetor e Cofre Digital
- Você pode habilitar os usuários para serem automaticamente conectados ao Gerenciador de Senhas Protetor e Cofre Digital (logon único) com suas contas do Azure AD
- Você pode gerenciar suas contas em um única localização: o Portal do Azure

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao Azure AD, consulte [o que é o acesso a aplicativos e logon único com o Azure Active Directory](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>pré-requisitos

Para configurar a integração do Azure AD ao Gerenciador de Senhas Protetor e Cofre Digital, você precisa dos seguintes itens:

- Uma assinatura do AD do Azure
- Uma assinatura habilitada para logon único do Gerenciador de Senhas Protetor e Cofre Digital

> [!NOTE]
> Para testar as etapas deste tutorial, nós não recomendamos o uso de um ambiente de produção.

Para testar as etapas deste tutorial, você deve seguir estas recomendações:

- Não use o ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do AD do Azure, você pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, você testará o logon único do Azure AD em um ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Gerenciador de Senhas Protetor e Cofre Digital da galeria
2. configurar e testar o logon único do AD do Azure

## <a name="adding-keeper-password-manager--digital-vault-from-the-gallery"></a>Adicionando Gerenciador de Senhas Protetor e Cofre Digital da galeria
Para configurar a integração do Gerenciador de Senhas Protetor e Cofre Digital no Azure AD, você precisa adicionar o Gerenciador de Senhas Protetor e Cofre Digital da galeria à sua lista de aplicativos SaaS gerenciados.

**Para adicionar o Gerenciador de Senhas Protetor e Cofre Digital da galeria, execute as seguintes etapas:**

1. No **[Portal do Azure](https://portal.azure.com)**, no painel navegação à esquerda, clique no ícone **Azure Active Directory**. 

    ![Active Directory][1]

2. Navegue até **aplicativos empresariais**. Em seguida, vá para **todos os aplicativos**.

    ![APLICATIVOS][2]
    
3. Clique no botão **Novo aplicativo** na parte superior da caixa de diálogo para adicionar o novo aplicativo.

    ![APLICATIVOS][3]

4. Na caixa de pesquisa, digite **Gerenciador de Senhas Protetor e Cofre Digital**.

    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_keeper_search.png)

5. No painel de resultados, selecione **Gerenciador de Senhas Protetor e Cofre Digital** e, em seguida, clique no botão **Adicionar** para adicionar o aplicativo.

    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_keeper_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>configurar e testar o logon único do AD do Azure
Nesta seção, você pode configurar e testar o logon único do Azure AD com o Gerenciador de Senhas Protetor e Cofre Digital com base em um usuário de teste chamado "Brenda Fernandes".

Para o logon único funcionar, o Azure AD precisa saber qual é o usuário correspondente no Gerenciador de Senhas Protetor e Cofre Digital para um usuário no Azure AD. Em outras palavras, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado no Gerenciador de Senhas Protetor e Cofre Digital.

No Gerenciador de Senhas Protetor e Cofre Digital, atribua o valor do **nome de usuário** no Azure AD como o valor de **Nome de usuário** para estabelecer a relação de vínculo.

Para configurar e testar o logon único do Azure AD com o Gerenciador de Senhas Protetor e Cofre Digital, você precisa concluir os blocos de construção a seguir:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - para habilitar seus usuários a usar esse recurso.
2. **[Criação de um usuário de teste do AD do Azure](#creating-an-azure-ad-test-user)** : para testar o logon único do Azure AD com Brenda Fernandes.
3. **[Como criar um usuário de teste do Gerenciador de Senhas Protetor e Cofre Digital](#creating-a-keeperpasswordmanager-test-user)** – para ter um equivalente de Brenda Fernandes no Gerenciador de Senhas Protetor e Cofre Digital vinculado à representação de usuário do Azure AD.
4. **[Atribuição do usuário de teste do AD do Azure](#assigning-the-azure-ad-test-user)** – para permitir que Brenda Fernandes use o logon único do AD do Azure.
5. **[Teste do logon único](#testing-single-sign-on)** : para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do logon único do Azure AD

Nesta seção, você habilita o logon único do Azure AD no portal do Azure e configura o logon único no aplicativo Gerenciador de Senhas Protetor e Cofre Digital.

**Para configurar o logon único do Azure AD com o Gerenciador de Senhas Protetor e Cofre Digital, execute as seguintes etapas:**

1. No portal do Azure, na página de integração do aplicativo do **Gerenciador de Senhas Protetor e Cofre Digital**, clique em **Logon único**.

    ![Configurar o logon único][4]

2. Na caixa de diálogo **Logon único**, selecione **Modo** como **Logon baseado em SAML** para habilitar o logon único.
 
    ![Configurar o logon único](./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_keeper_samlbase.png)

3. Na seção **Gerenciador de Senhas Protetor e Domínio do Cofre Digital e URLs**, execute as seguintes etapas:

    ![Configurar o logon único](./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_keeper_url.png)

    a. Na caixa de texto **URL de Logon**, digite uma URL usando o seguinte padrão: `https://{SSO CONNECT SERVER}/sso-connect/saml/login`

    b. Na caixa de texto **URL de resposta**, digite uma URL no seguinte padrão: `https://{SSO CONNECT SERVER}/sso-connect/saml/sso`

    c. Na caixa de texto **Identificador**, digite uma URL usando o seguinte padrão: `https://{SSO CONNECT SERVER}/sso-connect`

    > [!NOTE] 
    > Esses valores não são reais. Atualize esses valores com a URL de Resposta e a URL de Logon. Entre em contato com a [equipe de suporte do Gerenciador de Senhas Protetor e Cliente do Cofre Digital](https://keepersecurity.com/contact.html) para obter esses valores. 

4. Na seção **Certificado de Autenticação SAML**, clique em **Metadados XML** e, em seguida, salve o arquivo de metadados em seu computador.

    ![Configurar o logon único](./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_keeper_certificate.png) 

5. Clique no botão **Salvar** .

    ![Configurar o logon único](./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_general_400.png)
    
6. Na seção **Configuração do Gerenciador de Senhas Protetor e Cofre Digital**, clique em **Configurar Gerenciador de Senhas Protetor e Cofre Digital** para abrir a janela **Configurar logon**. Copie a **URL de saída, a ID da Entidade SAML e a URL do Serviço de Logon Único SAML** da **seção de Referência Rápida.**

    ![Configurar o logon único](./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_keeper_configure.png) 

7. Para configurar o logon único no lado da **Configuração do Gerenciador de Senhas Protetor e Cofre Digital**, siga as orientações fornecidas no [Guia de Suporte do Protetor](https://keepersecurity.com/assets/pdf/SettingupAzurewithKeeperSSOConnect.pdf)

> [!TIP]
> É possível ler uma versão concisa dessas instruções no [Portal do Azure](https://portal.azure.com), enquanto você estiver configurando o aplicativo!  Depois de adicionar esse aplicativo da seção **Active Directory > Aplicativos Empresariais**, basta clicar na guia **Logon Único** e acessar a documentação inserida por meio da seção **Configuração** na parte inferior. Saiba mais sobre a funcionalidade de documentação inserida aqui: [Documentação inserida do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Criação de um usuário de teste do AD do Azure
O objetivo desta seção é criar um usuário de teste no Portal do Azure chamado Brenda Fernandes.

![Criar um usuário do AD do Azure][100]

**Para criar um usuário de teste no AD do Azure, execute as seguintes etapas:**

1. No **Portal do Azure**, no painel de navegação esquerdo, clique no ícone **Azure Active Directory**.

    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-keeperpasswordmanager-tutorial/create_aaduser_01.png) 

2. Vá para **Usuários e grupos** e clique em **Todos os usuários** para exibir a lista de usuários.
    
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-keeperpasswordmanager-tutorial/create_aaduser_02.png) 

3. Para abrir a caixa de diálogo **Usuário**, clique em **Adicionar** na parte superior da caixa de diálogo.
 
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-keeperpasswordmanager-tutorial/create_aaduser_03.png) 

4. Na página do diálogo **Usuário**, execute as seguintes etapas:
 
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-keeperpasswordmanager-tutorial/create_aaduser_04.png) 

    a. Na caixa de texto **Nome**, digite **Brenda Fernandes**.

    b. Na caixa de texto **Nome de usuário**, digite o **endereço de email** da conta de Brenda Fernandes.

    c. Selecione **Mostrar senha** e anote o valor de **senha**.

    d. Clique em **Criar**.
 
### <a name="creating-a-keeper-password-manager--digital-vault-test-user"></a>Como criar um usuário de teste do Gerenciador de Senhas Protetor Cofre Digital

Para permitir que os usuários do Azure AD fazer logon no Gerenciador de Senhas Protetor e Cofre Digital, eles devem ser provisionados no Gerenciador de Senhas Protetor e Cofre Digital. O aplicativo dá suporte ao provisionamento de usuário just-in-time e, após a autenticação, os usuários serão automaticamente criados no aplicativo. Você poderá contatar [Suporte Protetor](https://keepersecurity.com/contact.html) se quiser configurar usuários manualmente.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuição do usuário de teste do AD do Azure

Nesta seção, você habilita Brenda Fernandes a usar logon único do Azure concedendo acesso ao Gerenciador de Senhas Protetor e Cofre Digital.

![Atribuir usuário][200] 

**Para atribuir Brenda Fernandes ao Gerenciador de Senhas Protetor e Cofre Digital, execute as seguintes etapas:**

1. No Portal do Azure, abra a exibição de aplicativos e, em seguida, navegue até a exibição de diretório e vá para **Aplicativos Empresariais** e clique em **Todos os aplicativos**.

    ![Atribuir usuário][201] 

2. Na lista de aplicativos, selecione **Gerenciador de Senhas Protetor e Cofre Digital**.

    ![Configurar o logon único](./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_keeper_app.png) 

3. No menu à esquerda, clique em **usuários e grupos**.

    ![Atribuir usuário][202] 

4. Clique no botão **Adicionar**. Em seguida, selecione **usuários e grupos** na **Adicionar atribuição** caixa de diálogo.

    ![Atribuir usuário][203]

5. Em **usuários e grupos** caixa de diálogo, selecione **Britta Simon** na lista de usuários.

6. Clique em **selecione** botão **usuários e grupos** caixa de diálogo.

7. Clique em **atribuir** botão **Adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste do logon único

Nesta seção, você testará sua configuração de logon único do Azure AD usando o Painel de Acesso.

Quando você clicar no bloco Gerenciador de Senhas Protetor e Cofre Digital no Painel de Acesso, deverá obter a página de logon do aplicativo Gerenciador de Senhas Protetor e Cofre Digital. Após a autenticação bem-sucedida, você deverá entrar no aplicativo. Para saber mais sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_general_203.png

