---
title: 'Tutorial: Integração do Azure Active Directory ao Reflektive | Microsoft Docs'
description: Saiba como configurar o logon único entre o Azure Active Directory e o Reflektive.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 799a08b9-1ce6-46d1-9064-aa9f36f6604e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/24/2017
ms.author: jeedes
ms.openlocfilehash: 92649829b1f646009891c15bea743482ec0b5db9
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-azure-active-directory-integration-with-reflektive"></a>Tutorial: integração do Azure Active Directory com o Reflektive

Neste tutorial, você aprenderá a integrar o Reflektive ao Azure AD (Azure Active Directory).

A integração do Reflektive ao Azure AD oferece os seguintes benefícios:

- No Azure AD, é possível controlar quem tem acesso ao Reflektive.
- Você pode permitir que os usuários façam logon automaticamente no Reflektive (Logon Único) com suas contas do Azure AD.
- Você pode gerenciar suas contas em um único local central – o portal do Azure.

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao Azure AD, consulte [o que é o acesso a aplicativos e logon único com o Azure Active Directory](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>pré-requisitos

Para configurar a integração do Azure AD com o Reflektive, você precisa dos seguintes itens:

- Uma assinatura do AD do Azure
- Uma assinatura do Reflektive com logon único habilitado

> [!NOTE]
> Para testar as etapas deste tutorial, nós não recomendamos o uso de um ambiente de produção.

Para testar as etapas deste tutorial, você deve seguir estas recomendações:

- Não use o ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, você pode [obter uma versão de avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, você testará o logon único do Azure AD em um ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Reflektive da Galeria
2. configurar e testar o logon único do AD do Azure

## <a name="adding-reflektive-from-the-gallery"></a>Adicionando Reflektive da Galeria
Para configurar a integração do Reflektive com o Azure AD, você precisará adicionar o Reflektive à sua lista de aplicativos SaaS gerenciados por meio da galeria.

**Para adicionar o Reflektive por meio da galeria, execute as seguintes etapas:**

1. No **[Portal do Azure](https://portal.azure.com)**, no painel navegação à esquerda, clique no ícone **Azure Active Directory**. 

    ![O botão Azure Active Directory][1]

2. Navegue até **aplicativos empresariais**. Em seguida, vá para **todos os aplicativos**.

    ![A folha Aplicativos empresariais][2]
    
3. Clique no botão **Novo aplicativo** na parte superior da caixa de diálogo para adicionar o novo aplicativo.

    ![O botão Novo aplicativo][3]

4. Na caixa de pesquisa, digite **Reflektive**, selecione **Reflektive** no painel de resultados e, em seguida, clique no botão **Adicionar** para adicionar o aplicativo.

    ![Reflektive na lista de resultados](./media/active-directory-saas-reflektive-tutorial/tutorial_reflektive_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar logon único do Azure AD

Nesta seção, você configurará e testará o logon único do Azure AD com o Reflektive, com base em um usuário de teste chamado "Brenda Fernandes".

Para que o logon único funcione, o Azure AD precisa saber qual usuário do Reflektive é equivalente a um usuário do Azure AD. Em outras palavras, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado no Reflektive.

No Reflektive, atribua o valor do **nome de usuário** no Azure AD como o valor do **Nome de usuário** para estabelecer a relação de vínculo.

Para configurar e testar o logon único do Azure AD com o Reflektive, você precisa concluir os seguintes blocos de construção:

1. **[Configurar o logon único do Azure AD](#configure-azure-ad-single-sign-on)** – para habilitar seus usuários a usar esse recurso.
2. **[Criar um usuário de teste do Azure AD](#create-an-azure-ad-test-user)** – para testar o logon único do Azure AD com Brenda Fernandes.
3. **[Criar um usuário de teste do Reflektive](#create-a-reflektive-test-user)** – para ter um equivalente de Brenda Fernandes no Reflektive que esteja vinculado à representação de usuário no Azure AD.
4. **[Atribuir o usuário de teste do Azure AD](#assign-the-azure-ad-test-user)** – para permitir que Brenda Fernandes use o logon único do Azure AD.
5. **[Teste o logon único](#test-single-sign-on)** – para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o logon único do Azure AD

Nesta seção, você habilitará o logon único do Azure AD no Portal do Azure e configurará o logon único no aplicativo Reflektive.

**Para configurar o logon único do Azure AD com o Reflektive, execute as seguintes etapas:**

1. No Portal do Azure, na página de integração de aplicativos do **Reflektive**, clique em **Logon único**.

    ![Link Configurar logon único][4]

2. Na caixa de diálogo **Logon único**, selecione **Modo** como **Logon baseado em SAML** para habilitar o logon único.
 
    ![Caixa de diálogo Logon único](./media/active-directory-saas-reflektive-tutorial/tutorial_reflektive_samlbase.png)

3. Na seção **Domínio e URLs do Reflektive**, realize as seguintes etapas se desejar configurar o aplicativo no modo iniciado pelo IDP:

    ![Informações de logon único de Domínio e URLs do Reflektive](./media/active-directory-saas-reflektive-tutorial/tutorial_reflektive_url.png)

    Na caixa de texto **Identificador**, use uma das URLs abaixo de acordo com a confirmação da equipe de suporte do Reflektive:
    | |
    |--|
    | `reflektive.com` |
    | `https://www.reflektive.com/saml/metadata` |

4. Marque **Mostrar configurações avançadas de URL** e realize a seguinte etapa se quiser configurar o aplicativo no modo iniciado pelo **SP**:

    ![Informações de logon único de Domínio e URLs do Reflektive](./media/active-directory-saas-reflektive-tutorial/tutorial_reflektive_url1.png)

    Na caixa de texto **URL de Logon**, digite uma URL: `https://www.reflektive.com/app`

    > [!NOTE]
    > Para o modo SP, você precisa obter a id de email registrada com a [Equipe de suporte do Reflektive](https://support@reflektive.com). Quando você insere sua ID na caixa de texto **Email**, a opção de logon único é habilitada.
         
5. Na seção **Certificado de Autenticação SAML**, clique em **Metadados XML** e, em seguida, salve o arquivo de metadados em seu computador.

    ![O link de download do Certificado](./media/active-directory-saas-reflektive-tutorial/tutorial_reflektive_certificate.png) 

6. Clique no botão **Salvar** .

    ![Botão Salvar em Configurar Logon Único](./media/active-directory-saas-reflektive-tutorial/tutorial_general_400.png)

7. Para configurar o logon único no lado do **Reflektive**, é necessário enviar o **XML de Metadados** baixado para a [equipe de suporte do Reflektive](https://support@reflektive.com). Eles definem essa configuração para ter a conexão de SSO de SAML definida corretamente em ambos os lados.

> [!TIP]
> É possível ler uma versão concisa dessas instruções no [Portal do Azure](https://portal.azure.com), enquanto você estiver configurando o aplicativo!  Depois de adicionar esse aplicativo da seção **Active Directory > Aplicativos Empresariais**, basta clicar na guia **Logon Único** e acessar a documentação inserida por meio da seção **Configuração** na parte inferior. Saiba mais sobre a funcionalidade de documentação inserida aqui: [Documentação inserida do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Criar um usuário de teste do Azure AD

O objetivo desta seção é criar um usuário de teste no Portal do Azure chamado Brenda Fernandes.

   ![Criar um usuário de teste do Azure AD][100]

**Para criar um usuário de teste no AD do Azure, execute as seguintes etapas:**

1. No portal do Azure, no painel esquerdo, clique no botão **Azure Active Directory**.

    ![O botão Azure Active Directory](./media/active-directory-saas-reflektive-tutorial/create_aaduser_01.png)

2. Para exibir a lista de usuários, acesse **Usuários e grupos** e, depois, clique em **Todos os usuários**.

    ![Os links “Usuários e grupos” e “Todos os usuários”](./media/active-directory-saas-reflektive-tutorial/create_aaduser_02.png)

3. Para abrir a caixa de diálogo **Usuário**, clique em **Adicionar** na parte superior da caixa de diálogo **Todos os Usuários**.

    ![O botão Adicionar](./media/active-directory-saas-reflektive-tutorial/create_aaduser_03.png)

4. Na caixa de diálogo **Usuário**, execute as seguintes etapas:

    ![A caixa de diálogo Usuário](./media/active-directory-saas-reflektive-tutorial/create_aaduser_04.png)

    a. Na caixa **Nome**, digite **BrendaFernandes**.

    b. Na caixa **Nome de usuário**, digite o endereço de email do usuário Brenda Fernandes.

    c. Marque a caixa de seleção **Mostrar Senha** e, em seguida, anote o valor exibido na caixa **Senha**.

    d. Clique em **Criar**.
 
### <a name="create-a-reflektive-test-user"></a>Criar um usuário de teste do Reflektive

Nesta seção, você criará um usuário chamado Brenda Fernandes no Reflektive. Trabalhe com a [equipe de suporte o Reflektive](mailto:support@reflektive.com) para adicionar os usuários na plataforma do Reflektive. Os usuários devem ser criados e ativados antes de usar o logon único. 

### <a name="assign-the-azure-ad-test-user"></a>Atribuir o usuário de teste do Azure AD

Nesta seção, você permitirá que Brenda Fernandes use o logon único do Azure concedendo acesso ao Reflektive.

![Atribuir a função de usuário][200] 

**Para atribuir Brenda Fernandes ao Reflektive, execute as seguintes etapas:**

1. No Portal do Azure, abra a exibição de aplicativos e, em seguida, navegue até a exibição de diretório e vá para **Aplicativos Empresariais** e clique em **Todos os aplicativos**.

    ![Atribuir usuário][201] 

2. Na lista de aplicativos, escolha **Reflektive**.

    ![O link do Reflektive na lista de Aplicativos](./media/active-directory-saas-reflektive-tutorial/tutorial_reflektive_app.png)  

3. No menu à esquerda, clique em **usuários e grupos**.

    ![O link “Usuários e grupos”][202]

4. Clique no botão **Adicionar**. Em seguida, selecione **usuários e grupos** na **Adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar Atribuição][203]

5. Em **usuários e grupos** caixa de diálogo, selecione **Britta Simon** na lista de usuários.

6. Clique em **selecione** botão **usuários e grupos** caixa de diálogo.

7. Clique em **atribuir** botão **Adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar logon único

Nesta seção, você testará sua configuração de logon único do Azure AD usando o Painel de Acesso.

Ao clicar no bloco do Reflektive no Painel de Acesso, você deverá ser conectado automaticamente ao seu aplicativo do Reflektive.
Para saber mais sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/active-directory-saas-reflektive-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-reflektive-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-reflektive-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-reflektive-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-reflektive-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-reflektive-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-reflektive-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-reflektive-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-reflektive-tutorial/tutorial_general_203.png

