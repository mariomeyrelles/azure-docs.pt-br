---
title: 'Tutorial: Integração do Azure Active Directory com o The Funding Portal | Microsoft Docs'
description: Saiba como configurar o logon único entre o Azure Active Drectory e o The Funding Portal.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 4663cc8a-976a-4c6c-b3b4-1e5df9b66744
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jeedes
ms.openlocfilehash: 8d7c945436b0e6069614f9b687af81e80a5eb160
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-azure-active-directory-integration-with-the-funding-portal"></a>Tutorial: Integração do Azure Active Directory ao The Funding Portal

Neste tutorial, você aprenderá a integrar o The Funding Portal ao Azure AD (Azure Active Directory).

A integração do The Funding Portal ao Azure AD oferece os seguintes benefícios:

- Você pode controlar no Azure AD quem tem acesso ao The Funding Portal
- Você pode permitir que os usuários entrem automaticamente no The Funding Portal (Logon Único) com suas contas do Azure AD
- Você pode gerenciar suas contas em um única localização: o Portal do Azure

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao Azure AD, consulte [o que é o acesso a aplicativos e logon único com o Azure Active Directory](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>pré-requisitos

Para configurar a integração do Azure AD com o The Funding Portal, você precisará dos seguintes itens:

- Uma assinatura do AD do Azure
- Uma assinatura do The Funding Portal habilitada para logon único

> [!NOTE]
> Para testar as etapas deste tutorial, nós não recomendamos o uso de um ambiente de produção.

Para testar as etapas deste tutorial, você deve seguir estas recomendações:

- Não use o ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do AD do Azure, você pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, você testará o logon único do Azure AD em um ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adição do The Funding Portal da galeria
2. configurar e testar o logon único do AD do Azure

## <a name="adding-the-funding-portal-from-the-gallery"></a>Adição do The Funding Portal da galeria
Para configurar a integração do The Funding Portal ao Azure AD, você precisa adicionar o The Funding Portal por meio da galeria à sua lista de aplicativos SaaS gerenciados.

**Para adicionar o The Funding Portal da galeria, execute as seguintes etapas:**

1. No **[Portal do Azure](https://portal.azure.com)**, no painel navegação à esquerda, clique no ícone **Azure Active Directory**. 

    ![Active Directory][1]

2. Navegue até **aplicativos empresariais**. Em seguida, vá para **todos os aplicativos**.

    ![APLICATIVOS][2]
    
3. Clique no botão **Novo aplicativo** na parte superior da caixa de diálogo para adicionar o novo aplicativo.

    ![APLICATIVOS][3]

4. Na caixa de pesquisa, digite **The Funding Portal**.

    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_search.png)

5. No painel de resultados, selecione **The Funding Portal** e clique no botão **Adicionar** para adicionar o aplicativo.

    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>configurar e testar o logon único do AD do Azure
Nesta seção, você configurará e testará o logon único do Azure AD com o The Funding Portal, com base em um usuário de teste chamado “Brenda Fernandes”.

Para que o logon único funcione, o Azure AD precisa saber qual usuário do The Funding Portal é equivalente a um usuário do Azure AD. Em outras palavras, é necessário estabelecer uma relação de vinculação entre um usuário do Azure AD e o usuário relacionado do The Funding Portal.

No The Funding Portal, atribua o valor do **nome de usuário** no Azure AD como o valor do **Nome de usuário** para estabelecer a relação de vínculo.

Para configurar e testar o logon único do Azure AD com o The Funding Portal, você precisa concluir os seguintes blocos de construção:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - para habilitar seus usuários a usar esse recurso.
2. **[Criação de um usuário de teste do AD do Azure](#creating-an-azure-ad-test-user)** : para testar o logon único do Azure AD com Brenda Fernandes.
3. **[Criação do usuário de teste do The Funding Portal](#creating-the-funding-portal-test-user)** – para ter um equivalente de Brenda Fernandes no The Funding Portal que esteja vinculado à representação de usuário no Azure AD.
4. **[Atribuição do usuário de teste do AD do Azure](#assigning-the-azure-ad-test-user)** – para permitir que Brenda Fernandes use o logon único do AD do Azure.
5. **[Teste do logon único](#testing-single-sign-on)** : para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do logon único do Azure AD

Nesta seção, você habilitará o logon único do Azure AD no Portal do Azure e configurará o logon único no aplicativo The Funding Portal.

**Para configurar o logon único do Azure AD com o The Funding Portal, execute as seguintes etapas:**

1. No Portal do Azure, na página de integração de aplicativos do **The Funding Portal**, clique em **Logon único**.

    ![Configurar o logon único][4]

2. Na caixa de diálogo **Logon único**, selecione **Modo** como **Logon baseado em SAML** para habilitar o logon único.
 
    ![Configurar o logon único](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_samlbase.png)

3. Na seção **URLs e Domínio do The Funding Portal**, siga as etapas abaixo:

    ![Configurar o logon único](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_url.png)

    a. Na caixa de texto **URL de Logon**, digite uma URL usando o seguinte padrão: `https://<subdomain>.regenteducation.net/`

    b. Na caixa de texto **Identificador**, digite uma URL usando o seguinte padrão: `https://<subdomain>.regenteducation.net`

    > [!NOTE] 
    > Esses valores não são reais. Atualize esses valores com a URL de Entrada e o Identificador reais. Contate a [equipe de suporte ao cliente do The Funding Portal](mailto:info@regenteducation.com) para obter esses valores. 

4. O aplicativo The Funding Portal espera que as asserções de SAML contenham um atributo chamado "externalId1". O valor de "externalId1" deve ser uma studentID reconhecida. Configure a declaração "externalId1" para esse aplicativo. É possível gerenciar os valores desses atributos na em **Atributos de Usuário** do aplicativo. A captura de tela a seguir mostra um exemplo disso.

    ![Configurar o logon único](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_attribute.png)

5. Na seção **Atributos do Usuário**, na caixa de diálogo **Logon único**, configure o atributo do token SAML como mostra a imagem e execute as etapas a seguir:

    | Nome do atributo | Valor do atributo |
    | ------------------- | ---------------- |
    | externalId1 | user.extensionattribute1 |

    a. Clique em **Adicionar atributo** para abrir o diálogo **Adicionar Atributo**.

    ![Configurar o logon único](./media/active-directory-saas-thefundingportal-tutorial/tutorial_attribute_04.png)

    ![Configurar o logon único](./media/active-directory-saas-thefundingportal-tutorial/tutorial_attribute_05.png)

    b. Na caixa de texto **Nome** , digite o nome do atributo mostrado para essa linha.

    c. Na lista **Valor do Atributo** , selecione o atributo que você deseja usar na sua implementação. Por exemplo, se você tiver armazenado o valor de StudentID em ExtensionAttribute1, então selecione user.extensionattribute1.
    
    d. Clique em **OK**.
 
6. Na seção **Certificado de Autenticação SAML**, clique em **Metadados XML** e, em seguida, salve o arquivo de metadados em seu computador.

    ![Configurar o logon único](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_certificate.png) 

7. Clique no botão **Salvar** .

    ![Configurar o logon único](./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_400.png)

8. Para configurar o logon único no lado do **The Funding Portal**, é necessário enviar o **XML de metadados** baixado para a [equipe de suporte do The Funding Portal](mailto:info@regenteducation.com). Eles definem essa configuração para ter a conexão de SSO de SAML definida corretamente em ambos os lados.

> [!TIP]
> É possível ler uma versão concisa dessas instruções no [Portal do Azure](https://portal.azure.com), enquanto você estiver configurando o aplicativo!  Depois de adicionar esse aplicativo da seção **Active Directory > Aplicativos Empresariais**, basta clicar na guia **Logon Único** e acessar a documentação inserida por meio da seção **Configuração** na parte inferior. Saiba mais sobre a funcionalidade de documentação inserida aqui: [Documentação inserida do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Criação de um usuário de teste do AD do Azure
O objetivo desta seção é criar um usuário de teste no Portal do Azure chamado Brenda Fernandes.

![Criar um usuário do AD do Azure][100]

**Para criar um usuário de teste no AD do Azure, execute as seguintes etapas:**

1. No **Portal do Azure**, no painel de navegação esquerdo, clique no ícone **Azure Active Directory**.

    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_01.png) 

2. Vá para **Usuários e grupos** e clique em **Todos os usuários** para exibir a lista de usuários.
    
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_02.png) 

3. Para abrir a caixa de diálogo **Usuário**, clique em **Adicionar** na parte superior da caixa de diálogo.
 
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_03.png) 

4. Na página do diálogo **Usuário**, execute as seguintes etapas:
 
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_04.png) 

    a. Na caixa de texto **Nome**, digite **Brenda Fernandes**.

    b. Na caixa de texto **Nome de usuário**, digite o **endereço de email** da conta de Brenda Fernandes.

    c. Selecione **Mostrar senha** e anote o valor de **senha**.

    d. Clique em **Criar**.
 
### <a name="creating-the-funding-portal-test-user"></a>Criação de um usuário de teste do The Funding Portal

Nesta seção, você criará uma usuária chamada Brenda Fernandes no The Funding Portal. Trabalhe com a [equipe de suporte do The Funding Portal](mailto:info@regenteducation.com) para adicionar o usuário de teste e habilitar o SSO.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuição do usuário de teste do AD do Azure

Nesta seção, você permitirá que Brenda Fernandes use o logon único do Azure concedendo acesso ao The Funding Portal.

![Atribuir usuário][200] 

**Para atribuir Brenda Fernandes ao The Funding Portal, execute as seguintes etapas:**

1. No Portal do Azure, abra a exibição de aplicativos e, em seguida, navegue até a exibição de diretório e vá para **Aplicativos Empresariais** e clique em **Todos os aplicativos**.

    ![Atribuir usuário][201] 

2. Na lista de aplicativos, selecione **The Funding Portal**.

    ![Configurar o logon único](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_app.png) 

3. No menu à esquerda, clique em **usuários e grupos**.

    ![Atribuir usuário][202] 

4. Clique no botão **Adicionar**. Em seguida, selecione **usuários e grupos** na **Adicionar atribuição** caixa de diálogo.

    ![Atribuir usuário][203]

5. Em **usuários e grupos** caixa de diálogo, selecione **Britta Simon** na lista de usuários.

6. Clique em **selecione** botão **usuários e grupos** caixa de diálogo.

7. Clique em **atribuir** botão **Adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste do logon único

O objetivo desta seção é testar sua configuração de logon único do Azure AD usando o Painel de Acesso.

Quando você clicar no bloco do The Funding Portal no Painel de Acesso, deverá entrar automaticamente no seu aplicativo do The Funding Portal.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_203.png

