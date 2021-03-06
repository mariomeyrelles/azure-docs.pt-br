---
title: Termos de Uso do Azure Active Directory | Microsoft Docs
description: Os Termos de Uso do Azure AD permitirão que você e sua empresa possam fornecer termos de uso aos usuários de serviços do Azure AD.
services: active-directory
author: billmath
manager: mtillman
editor: ''
ms.assetid: d55872ef-7e45-4de5-a9a0-3298e3de3565
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/29/2018
ms.author: billmath
ms.openlocfilehash: ea68bad3a2c5e905ccf705404dff0049b451268e
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/18/2018
---
# <a name="azure-active-directory-terms-of-use-feature"></a>Recurso Termos de Uso do Azure Active Directory
Os Termos de Uso do Azure AD fornecem um método simples que as organizações podem usar para apresentar informações aos usuários finais.  Essa apresentação faz com que os usuários vejam os avisos de isenção de responsabilidade relevantes para os requisitos de conformidade ou legais.

O conteúdo dos Termos de Uso do Azure AD são apresentados em formato PDF.   O PDF pode ter qualquer conteúdo, por exemplo, contratos existentes, o que permite que você colete os contratos do usuário final na entrada do usuário.  Você pode usar os termos de uso para aplicativos, grupos de usuários ou, se for o caso, vários termos de uso para finalidades diferentes.

O restante deste documento descreve como usar os Termos de Uso do Azure AD.  

## <a name="why-use-azure-ad-terms-of-use"></a>Por que usar os Termos de Uso do Azure AD
Está com dificuldades para que os seus funcionários ou convidados aceitem seus termos de uso antes de obter acesso? Precisa de ajuda para saber quem concordou ou não com os termos de uso da empresa?  Os Termos de Uso do Azure AD fornecem um método simples que as organizações podem usar para apresentar informações aos usuários finais.  Isso faz com que eles vejam os avisos de isenção de responsabilidade relevantes para os requisitos de conformidade ou legais.

Os Termos de Uso do Azure AD podem ser usados nos seguintes cenários:
-   Termos gerais de uso para todos os usuários em sua organização.
-   Termos específicos de uso com base em atributos de um usuário (por exemplo, médicos vs enfermeiras ou funcionários locais vs internacionais, feitos por [grupos dinâmicos](https://azure.microsoft.com/updates/azure-active-directory-dynamic-membership-for-groups)).
-   Termos específicos de uso baseados no acesso de aplicativos de alto impacto nos negócios, como o Salesforce.


## <a name="prerequisites"></a>pré-requisitos
Use as etapas a seguir para configurar os Termos de Uso do Azure AD:

1. Entre no Azure AD usando um administrador global, administrador de segurança ou um administrador de acesso condicional para o diretório que você deseja configurar os Termos de Uso Azure AD.
2. Certifique-se de que o diretório possui uma assinatura do Azure AD Premium P1, P2, EMS E3 ou EMS E5.  Caso contrário, [Obtenha o Azure AD Premium](active-directory-get-started-premium.md) ou [inicie uma avaliação gratuita](https://azure.microsoft.com/trial/get-started-active-directory/).
3. Exiba o painel dos Termos de Uso do Azure AD em [ https://aka.ms/catou ](https://aka.ms/catou).

>[!IMPORTANT]
>Controles de política de acesso condicional (incluindo termos de uso) não dão suporte à imposição em contas de serviço.  Recomenda-se excluir todas as contas de serviço da política de acesso condicional.

## <a name="add-company-terms-of-use"></a>Adicionar Termos de Uso da empresa
Depois de preparar os seus Termos de Uso, use o procedimento a seguir para adicioná-lo.

### <a name="to-add-terms-of-use"></a>Para adicionar os Termos de Uso
1. Navegue até o painel em [https://aka.ms/catou](https://aka.ms/catou)
2. Clique em Adicionar.</br>
![Adicionar Termos de Uso](media/active-directory-tou/tou12.png)
3. Insira o **Nome** para os Termos de Uso
4. Insira o **Nome de exibição**.  O cabeçalho é que os usuários visualizam quando entram.
5. **Navegue** até o PDF com os seus termos de uso finalizados e selecione-o.  O tamanho de fonte recomendado é 24.
6. **Selecione** um idioma para os termos de uso.  A opção de idioma permite carregar vários termos de uso, cada um com um idioma diferente.  A versão dos termos de uso que um usuário final verá terá base em suas preferências de navegador.
7. Selecione ativar ou desativar para **Exigir que os usuários expandam os termos de uso**.  Se for ativado, os usuários finais precisarão ler os termos de uso antes de aceitá-los.
8. Em **Acesso Condicional**, você pode **Impor** os termos de uso carregados selecionando um modelo ou uma política de acesso condicional personalizada no menu suspenso.  As políticas de acesso condicional personalizadas permitem termos de uso granulares, para até um aplicativo de nuvem ou um grupo de usuários específicos.  Para obter mais informações, consulte [Configurando as políticas de acesso condicional personalizadas](active-directory-conditional-access-best-practices.md)
9. Clique em **Criar**.
10. Se você selecionou um modelo de acesso condicional personalizado, uma nova tela é exibida e permite que você personalize a política de autoridade de certificação.
11. Agora você deve ver seus novos Termos de Uso.</br>

![Adicionar Termos de Uso](media/active-directory-tou/tou3.png)

## <a name="delete-terms-of-use"></a>Excluir Termos de Uso
Você pode remover ou excluir o termos de uso antigos usando o procedimento a seguir:

### <a name="to-delete-terms-of-use"></a>Para excluir os Termos de Uso
1. Navegue até o painel em [https://aka.ms/catou](https://aka.ms/catou)
2. Selecione os termos de uso que você deseja remover.
3. Clique em **Excluir**.
4. Agora você não deve mais ver seus novos Termos de Uso.


## <a name="viewing-current-user-status"></a>Exibindo o status atual do usuário
Você observará que os termos de uso mostram uma contagem de usuários que aceitaram e recusaram.

![Auditar evento](media/active-directory-tou/tou15.png)

Você pode clicar nos números de **aceito** ou **recusado** para exibir o estado atual dos usuários.

![Auditar evento](media/active-directory-tou/tou16.png)

## <a name="audit-terms-of-use"></a>Auditar Termos de Uso
Se desejar exibir o histórico aceitações e recusas e não apenas o status atual, os Termos de Uso do Azure AD oferecem uma auditoria fácil de usar.  Essa auditoria permite que você veja quem aceitou os termos de uso e quando eles foram aceitos.  

Há duas maneiras de usar a auditoria, dependendo que você está tentando fazer atualmente.  


Para começar a usar a auditoria use o procedimento a seguir:

### <a name="to-audit-terms-of-use"></a>Para auditar os Termos de Uso
1. Navegue até o painel em [https://aka.ms/catou](https://aka.ms/catou)
2. Clique em Exibir logs de auditoria.</br>
![Auditar evento](media/active-directory-tou/tou8.png)
3.  Você pode filtrar as informações usando as listas suspensas fornecidas para buscar informações de log de auditoria específicas na tela de logs de auditoria do Azure AD.
[Auditar evento](media/active-directory-tou/tou9.png)
4.  Você também pode baixar as informações em um arquivo .csv para uso local.

## 

## <a name="what-users-see"></a>O que os usuários visualizam
Os usuários que estão no escopo visualizarão o seguinte após os termos de uso terem sido criados e aplicados.  Eles visualizarão essas telas durante a entrada.
-   A prática recomendada é usar a fonte dentro do PDF em tamanho 24.
![Auditar evento](media/active-directory-tou/tou10.png)
-   Esta é a tela exibida nos celulares</br></br>
![Auditar evento](media/active-directory-tou/tou11.png)

### <a name="review-terms-of-use"></a>Revisar termos de uso
Os usuários podem examinar e ver os termos de uso que eles tiverem aceitado.  Para examinar os termos de uso, use o seguinte procedimento:

1. Navegue até [ https://myapps.microsoft.com ](https://myapps.microsoft.com) e entre.
2. No canto superior direito, clique em seu nome e selecione **Perfil** no menu suspenso.
![Perfil](media/active-directory-tou/tou14.png)

3. Em seu perfil, clique em **Revisar os termos de uso**.
![Auditar evento](media/active-directory-tou/tou13a.png)

4.  A partir daí, você pode revisar os termos de uso aceitos. 


## <a name="additional-information"></a>Informações adicionais
As informações a seguir devem ser consideradas e podem ajudar na utilização dos termos de uso.

>[!IMPORTANT]
> Os usuários que estão no escopo precisarão sair e entrar novamente para atender a uma nova política se:
> - uma política de acesso condicional for habilitada nos termos de uso
> - ou se outros termos de uso forem criados
>
>As políticas de acesso condicional entram em vigor imediatamente. Quando isso acontece o administrador começará a ver um ícone de "nuvem triste" ou "Problemas de token do Azure AD". O administrador deve sair e entrar novamente para atender à nova política.





## <a name="frequently-asked-questions"></a>Perguntas frequentes

**P: Como ver quando/se um usuário aceitou os termos de uso?**</br>
R: você pode simplesmente clicar no número abaixo de aceito ao lado dos termos de uso.  Para saber mais, confira [Exibindo o status atual do usuário](#viewing-current-user-status).  Além disso, um usuário que aceita os termos de uso é gravado no log de auditoria. Você pode pesquisar o log de auditoria do Azure AD para ver os resultados.  

**P: Se os termos de uso forem alterados, os usuários precisam aceitá-los novamente?**</br>
R: sim, um administrador pode alterar os termos de uso e isso requer que os novos termos também sejam aceitos.

**P: os termos de uso possuem suporte para vários idiomas?**</br>
R: Sim.  Atualmente há 18 idiomas diferentes que um administrador pode configurar para um mesmo documento de termos de uso. 

**P: Quando os termos de uso são acionados?**</br>
R: Os termos de uso são acionados acionada no momento em que o usuário faz o logon.

**P: Para quais aplicativos posso empregar os termos de uso?**</br>
R: Você pode criar uma política de acesso condicional nos aplicativos empresariais usando autenticação moderna.  Para obter mais informações, consulte [aplicativos empresariais](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-view-azure-portal).

**P: Posso adicionar vários termos de uso para um determinado usuário ou aplicativo?**</br>
R: Sim, criando várias políticas de acesso condicional, direcionadas a esses grupos ou aplicativos. Se um usuário estiver no escopo de vários termos de uso eles devem concordar com os termos de uso um por um.
 
**P: O que acontece se um usuário recusar os termos de uso?**</br>
R: O usuário é impedido de acessar o aplicativo. O usuário precisa entrar novamente e concordar com os termos para obter acesso.
