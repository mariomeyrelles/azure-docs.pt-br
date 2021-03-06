---
title: 'Tutorial: integração do Azure Active Directory com o BeeLine | Microsoft Docs'
description: Saiba como configurar o logon único entre o Azure Active Directory e o BeeLine.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 0726859d-1dac-44a0-810b-da56d89039ee
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 385866434af29a3f377003ac33c199f091a282b8
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-azure-active-directory-integration-with-beeline"></a>Tutorial: Integração do Azure Active Directory ao Beeline

Neste tutorial, você aprenderá a integrar o BeeLine ao Azure AD (Azure Active Directory).

A integração do BeeLine ao Azure AD oferece os seguintes benefícios:

- Você pode controlar no Azure AD quem tem acesso ao BeeLine
- Você pode permitir que os usuários façam logon automaticamente no BeeLine (Logon Único) com suas contas do Azure AD
- Você pode gerenciar suas contas em um única localização: o Portal do Azure

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao Azure AD, consulte [o que é o acesso a aplicativos e logon único com o Azure Active Directory](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>pré-requisitos

Para configurar a integração do Azure AD com o BeeLine, você precisará dos seguintes itens:

- Uma assinatura do AD do Azure
- Uma assinatura habilitada para logon único do BeeLine

> [!NOTE]
> Para testar as etapas deste tutorial, nós não recomendamos o uso de um ambiente de produção.

Para testar as etapas deste tutorial, você deve seguir estas recomendações:

- Não use o ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do AD do Azure, você pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, você testará o logon único do Azure AD em um ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Como adicionar o BeeLine por meio da galeria
2. configurar e testar o logon único do AD do Azure

## <a name="adding-beeline-from-the-gallery"></a>Como adicionar o BeeLine por meio da galeria
Para configurar a integração do BeeLine ao Azure AD, você precisará adicionar o BeeLine à sua lista de aplicativos SaaS gerenciados por meio da galeria.

**Para adicionar o BeeLine por meio da galeria, execute as seguintes etapas:**

1. No **[Portal do Azure](https://portal.azure.com)**, no painel navegação à esquerda, clique no ícone **Azure Active Directory**. 

    ![Active Directory][1]

2. Navegue até **aplicativos empresariais**. Em seguida, vá para **todos os aplicativos**.

    ![APLICATIVOS][2]
    
3. Clique no botão **Novo aplicativo** na parte superior da caixa de diálogo para adicionar o novo aplicativo.

    ![APLICATIVOS][3]

4. Na caixa de pesquisa, digite **BeeLine**.

    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_search.png)

5. No painel de resultados, selecione **BeeLine** e, em seguida, clique no botão **Adicionar** para adicionar o aplicativo.

    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>configurar e testar o logon único do AD do Azure
Nesta seção, você configurará e testará o logon único do Azure AD com o BeeLine, com base em um usuário de teste chamado “Brenda Fernandes”.

Para que o logon único funcione, o Azure AD precisa saber qual usuário do BeeLine é equivalente a um usuário do Azure AD. Em outras palavras, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado do BeeLine.

No BeeLine, atribua o valor do **nome de usuário** no Azure AD como o valor do **Nome de usuário** para estabelecer a relação de vínculo.

Para configurar e testar o logon único do Azure AD com o BeeLine, você precisará concluir os seguintes blocos de construção:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - para habilitar seus usuários a usar esse recurso.
2. **[Criação de um usuário de teste do AD do Azure](#creating-an-azure-ad-test-user)** : para testar o logon único do Azure AD com Brenda Fernandes.
3. **[Criação de um usuário de teste do BeeLine](#creating-a-beeline-test-user)**: para ter um equivalente de Brenda Fernandes no BeeLine que esteja vinculado à representação do usuário no Azure AD.
4. **[Atribuição do usuário de teste do AD do Azure](#assigning-the-azure-ad-test-user)** – para permitir que Brenda Fernandes use o logon único do AD do Azure.
5. **[Teste do logon único](#testing-single-sign-on)** : para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do logon único do Azure AD

Nesta seção, você habilitará o logon único do Azure AD no Portal do Azure e configurará o logon único em seu aplicativo BeeLine.

**Para configurar o logon único do Azure AD com o BeeLine, execute as seguintes etapas:**

1. No Portal do Azure, na página de integração de aplicativos do **BeeLine**, clique em **Logon único**.

    ![Configurar o logon único][4]

2. Na caixa de diálogo **Logon único**, selecione **Modo** como **Logon baseado em SAML** para habilitar o logon único.
 
    ![Configurar o logon único](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_samlbase.png)

3. Na seção **URLs e Domínio do BeeLine**, execute as seguintes etapas:

    ![Configurar o logon único](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_url.png)

    a. Na caixa de texto **Identificador**, digite uma URL usando o seguinte padrão: `https://projects.beeline.net/<instancename>`

    b. Na caixa de texto **URL de resposta** , digite uma URL no seguinte padrão:
    | |
    |--|
    | `https://projects.beeline.net/<instancename>/SSO_External.ashx`|
    | `https://projects.beeline.net/<companyname>/SSO_External.ashx` |

    > [!NOTE] 
    > Esses valores não são reais. Atualize esses valores com o Identificador e a URL de Resposta reais. Entre em contato com a [equipe de suporte do BeeLine](https://www.beeline.com/contact-us/) para obter esses valores.
 
4. Na seção **Certificado de Autenticação SAML**, clique em **Metadados XML** e, em seguida, salve o arquivo de metadados em seu computador.

    ![Configurar o logon único](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_certificate.png) 

5. O aplicativo Beeline espera que as declarações SAML estejam em um formato específico. Trabalhe com a [equipe de suporte do BeeLine](https://www.beeline.com/contact-us/) primeiro para verificar o identificador de usuário correto que será mapeado no aplicativo. Também siga as diretrizes da [equipe de suporte do BeeLine](https://www.beeline.com/contact-us/) sobre o atributo que deseja usar para mapeamento. Você pode gerenciar o valor desse atributo na guia **Atributos do Usuário** do aplicativo. A captura de tela a seguir mostra um exemplo disso. Aqui mapeamos a declaração do **Identificador de Usuário** com o atributo **userprincipalname**, que fornece uma ID de usuário único, que será enviada para o aplicativo BeeLine em cada resposta SAML com êxito.

    ![Configurar o logon único](./media/active-directory-saas-beeline-tutorial/tutorial_attribute.png)  

6. Clique no botão **Salvar** .

    ![Configurar o logon único](./media/active-directory-saas-beeline-tutorial/tutorial_general_400.png)

7. Na seção **Configuração do BeeLine**, clique em **Configurar BeeLine** para abrir a janela **Configuração de logon**. Copie a **URL de Logoff** e a **ID da Entidade do SAML** da **seção Referência Rápida.**

    ![Configurar o logon único](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_configure.png) 

8. Para configurar logon único no lado do **BeeLine**, é necessário enviar o **XML de Metadados**, a **ID da Entidade SAML** e a **URL de Saída** baixados para a [equipe de suporte do BeeLine](https://www.beeline.com/contact-us/).

> [!TIP]
> É possível ler uma versão concisa dessas instruções no [Portal do Azure](https://portal.azure.com), enquanto você estiver configurando o aplicativo!  Depois de adicionar esse aplicativo da seção **Active Directory > Aplicativos Empresariais**, basta clicar na guia **Logon Único** e acessar a documentação inserida por meio da seção **Configuração** na parte inferior. Saiba mais sobre a funcionalidade de documentação inserida aqui: [Documentação inserida do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Criação de um usuário de teste do AD do Azure
O objetivo desta seção é criar um usuário de teste no Portal do Azure chamado Brenda Fernandes.

![Criar um usuário do AD do Azure][100]

**Para criar um usuário de teste no AD do Azure, execute as seguintes etapas:**

1. No **Portal do Azure**, no painel de navegação esquerdo, clique no ícone **Azure Active Directory**.

    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-beeline-tutorial/create_aaduser_01.png) 

2. Vá para **Usuários e grupos** e clique em **Todos os usuários** para exibir a lista de usuários.
    
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-beeline-tutorial/create_aaduser_02.png) 

3. Para abrir a caixa de diálogo **Usuário**, clique em **Adicionar** na parte superior da caixa de diálogo.
 
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-beeline-tutorial/create_aaduser_03.png) 

4. Na página do diálogo **Usuário**, execute as seguintes etapas:
 
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-beeline-tutorial/create_aaduser_04.png) 

    a. Na caixa de texto **Nome**, digite **Brenda Fernandes**.

    b. Na caixa de texto **Nome de usuário**, digite o **endereço de email** da conta de Brenda Fernandes.

    c. Selecione **Mostrar senha** e anote o valor de **senha**.

    d. Clique em **Criar**.
 
### <a name="creating-a-beeline-test-user"></a>Criação de um usuário de teste do BeeLine

Nesta seção, você criará um usuário chamado Brenda Fernandes no Beeline. O aplicativo BeeLine precisa que todos os usuários sejam provisionados no aplicativo antes de fazer o Logon Único. Então trabalhe com a [equipe de suporte do BeeLine](https://www.beeline.com/contact-us/) para provisionar todos esses usuários no aplicativo. 

### <a name="assigning-the-azure-ad-test-user"></a>Atribuição do usuário de teste do AD do Azure

Nesta seção, você permitirá que Brenda Fernandes use o logon único do Azure ao conceder acesso ao BeeLine.

![Atribuir usuário][200] 

**Para atribuir Brenda Fernandes ao BeeLine, execute as seguintes etapas:**

1. No Portal do Azure, abra a exibição de aplicativos e, em seguida, navegue até a exibição de diretório e vá para **Aplicativos Empresariais** e clique em **Todos os aplicativos**.

    ![Atribuir usuário][201] 

2. Na lista de aplicativos, escolha **BeeLine**.

    ![Configurar o logon único](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_app.png) 

3. No menu à esquerda, clique em **usuários e grupos**.

    ![Atribuir usuário][202] 

4. Clique no botão **Adicionar**. Em seguida, selecione **usuários e grupos** na **Adicionar atribuição** caixa de diálogo.

    ![Atribuir usuário][203]

5. Em **usuários e grupos** caixa de diálogo, selecione **Britta Simon** na lista de usuários.

6. Clique em **selecione** botão **usuários e grupos** caixa de diálogo.

7. Clique em **atribuir** botão **Adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste do logon único

Nesta seção, você testará sua configuração de logon único do Azure AD usando o Painel de Acesso. Ao clicar no bloco Beeline no Painel de Acesso, você deverá ser conectado automaticamente a seu aplicativo Beeline.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_203.png

