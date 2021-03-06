---
title: "Usuários sinalizados para o relatório de risco na segurança no portal do Azure Active Directory | Microsoft Docs"
description: "Saiba mais sobre os usuários sinalizados para o relatório de risco na segurança no portal do Azure Active Directory"
services: active-directory
author: MarkusVi
manager: femila
ms.assetid: addd60fe-d5ac-4b8b-983c-0736c80ace02
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/24/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: ed6201e9edcef39b14b948b6b2f6e0b5da01ec60
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/05/2018
---
# <a name="remediate-users-flagged-for-risk-in-the-azure-active-directory-portal"></a>Corrigir usuários sinalizados como de risco no portal do Azure Active Directory

Com os relatórios de segurança no Azure Active Directory (Azure AD), você pode obter informações sobre a probabilidade de contas de usuário comprometidas em seu ambiente. Um [usuário sinalizado como de risco](active-directory-identityprotection.md#users-flagged-for-risk) é um indicador de que uma conta de usuário pode ter sido comprometida.

A Microsoft está empenhada em ajudar a manter seus ambientes protegidos. Como parte desse compromisso, a Microsoft monitora continuamente atividades incomuns ou consistentes com padrões de ataques conhecidos. 


Se forem detectadas atividades incomuns que possam indicar o acesso não autorizado a algumas das suas contas de usuário, você receberá notificações permitindo a tomada de medidas. As notificações não significam que os próprios sistemas da Microsoft foram comprometidos.
 

## <a name="azure-active-directory-report-access"></a>Acesso ao relatório do Azure Active Directory

Você pode examinar os usuários sinalizados como de risco em um relatório online do Azure Active Directory. Se você não é um assinante do Azure, pode passar pelo processo de assinatura, sem nenhum custo, em [http://aka.ms/AccessAAD](http://aka.ms/AccessAAD).  
Uma vez concluído, você pode usar suas credenciais do Office 365 para acessar o Centro de administração do Azure. Observe que, no nível de assinatura básica, a quantidade de detalhes fornecida é limitada. Outras análises e dados estão disponíveis para assinantes Premium do Azure. Para saber mais, confira o [Relatório de segurança de usuários sinalizados como de risco no portal do Azure Active Directory](active-directory-reporting-security-user-at-risk.md).

Após ativar seu acesso ao Azure AD, você será redirecionado para o [portal do Azure AD](https://portal.azure.com). Para ir diretamente para o relatório, navegue até a seguinte URL: [https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/UsersAtRisk](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/UsersAtRisk).

**Para acessar os relatórios de Usuários sinalizados como de risco no Centro de administração do Office 365:**

1.  No menu de navegação à esquerda, clique em **Centros de Administração**. 
2.  Clique em **Azure AD**.
3.  Entre no **centro de administração do Azure Active Directory**.
4.  Se um banner for exibido na parte superior da página dizendo **Confira o novo portal**, clique no link.
4.  No menu de navegação à esquerda, clique em **Azure Active Directory**. 
5.  No painel de navegação, em **Segurança**, clique em **Usuários sinalizados como de risco**.

Analise as informações exibidas aqui. Você deve redefinir a senha para as contas listadas aqui. 

## <a name="remediation-actions"></a>Ações de correção

Execute as seguintes ações para ajudar a corrigir as contas afetadas e proteger seu ambiente:

1.  [Validar](http://aka.ms/MFAValid) informações corretas para redefinição de senha de autoatendimento e autenticação multifator. 
2.  [Habilite](http://aka.ms/MFAuth) a autenticação multifator para todos os usuários. 
3.  Usando este [script de correção](http://aka.ms/remediate), para cada conta afetada você pode executar automaticamente as etapas abaixo: 

    a. Redefinir a senha para proteger a conta e fechar as sessões ativas.

    b. Remover representantes de caixa de correio.

    c. Desabilitar regras de encaminhamento de mensagens para domínios externos.

    d. Remover a propriedade de encaminhamento de email global na caixa de correio.

    e. Habilitar a MFA na conta do usuário.

    f. Definir a complexidade da senha na conta como alta.

    g. Habilitar auditoria na caixa de correio.

    h. Gerar a trilha de auditoria para o administrador analisar.

4. Investigue o seu locatário do Office 365 e outras infraestruturas de TI, incluindo uma revisão de todas as configurações de locatário, contas de usuário e configurações por usuário para ver as modificações possíveis. Verifique se há indicadores de métodos de persistência e indicadores de que um invasor possa se aproveitado de alguma forma para obter credenciais de VPN ou acesso a outros recursos organizacionais. 

5.  Como parte de sua investigação, estude se deve ou é obrigado a notificar autoridades governamentais, inclusive por lei.

Além disso, você deve:

- Ler e implementar esta [orientação](http://aka.ms/fixaccount) sobre como tratar atividades incomuns. 
- [Habilitar o pipeline de auditoria](http://aka.ms/improvesecurity) para ajudar você a analisar a atividade na sua locação. Uma vez concluído, o armazenamento de auditoria começa a ser preenchido com todos os logs de atividade. Aqui você também pode aproveitar o setor de [Pesquisa e investigação do Centro de conformidade e segurança](http://aka.ms/sccsearch). 
- Use este [script](http://aka.ms/mailboxaudit1) para habilitar a auditoria de caixa de correio em todas as suas contas. 
- Examine as permissões de representante e as regras de encaminhamento para todas as suas caixas de correio de email. Você pode usar este [script do PowerShell](http://aka.ms/delegateforwardrules) para executar a tarefa. 



## <a name="next-steps"></a>Próximas etapas

- Para saber mais sobre o Azure Active Directory Identity Protection, veja [Azure Active Directory Identity Protection](active-directory-identityprotection.md).

