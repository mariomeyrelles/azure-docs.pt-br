---
title: Migrar Conta de Automação e recursos
description: Este artigo descreve como mover uma conta de Automação na Automação do Azure e recursos associados de uma assinatura para outra.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 75d973388bb8a37cc0b796b1621f847b047d70f9
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/16/2018
---
# <a name="migrate-automation-account-and-resources"></a>Migrar de Conta de Automação e recursos
Para contas de Automação e para os recursos associados (ou seja, ativos, runbooks, módulos etc.) que você criou no portal do Azure e que deseja migrar de um grupo de recursos para outro ou de uma assinatura para outra, é possível fazer isso facilmente com a funcionalidade [mover recursos](../azure-resource-manager/resource-group-move-resources.md) disponível no portal do Azure. No entanto, antes de prosseguir com esta ação, consulte primeiro a seguinte [lista de verificação antes de mover recursos](../azure-resource-manager/resource-group-move-resources.md#checklist-before-moving-resources) e, além dela, a lista abaixo específica à Automação.  

1. A assinatura/grupo de recursos de destino deve estar na mesma região que a origem. Isso significa que contas de Automação não podem ser movidas entre regiões.
2. Ao mover recursos (por exemplo, runbooks, trabalhos, etc.), ambos o grupo de origem e o grupo de destino estão bloqueados pela duração da operação. As operações de gravação e exclusão são bloqueadas nos grupos até que a migração seja concluída. 
3. Qualquer runbook ou variável que fizer referência a um recurso ou ID da assinatura existente precisará ser atualizado depois que a migração for concluída.  

> [!NOTE]
> Esse recurso não dá suporte à movimentação de recursos de automação Clássicos.
>
>

## <a name="to-move-the-automation-account-using-the-portal"></a>Para mover a conta de Automação usando o portal
1. Em sua conta de Automação, clique em **Mover** na parte superior da página.<br> ![Opção de mover](media/automation-migrate-account-subscription/automation-menu-move.png)<br>
2. Escolha esta opção para mover a conta de automação para outro grupo de recursos ou outra assinatura.
3. No painel **Mover recursos**, observe que ele apresenta recursos relacionados à sua conta de Automação e a seus grupos de recursos. Selecione a **assinatura** e o **grupo de recursos** nas listas suspensas ou selecione a opção **criar um novo grupo de recursos** e digite um novo nome de grupo de recursos no campo fornecido. 
4. Examine e marque a caixa de seleção para confirmar que você *entende que as ferramentas e scripts precisarão ser atualizados para usar as novas ID de recurso depois de mover recursos* e clique em **OK**.<br> ![Painel Mover recursos](media/automation-migrate-account-subscription/automation-move-resources-blade.png)<br>   

Essa ação pode levar vários minutos para ser concluída. Em **Notificações**, será exibido um status de cada ação que ocorrer, incluindo validação, migração e, por fim, quando estiver concluído.    

## <a name="to-move-the-automation-account-using-powershell"></a>Para mover a Conta de Automação usando o PowerShell
Para mover os recursos de Automação existentes para outro grupo de recursos ou assinatura, use o cmdlet **Get-AzureRmResource** para obter a conta de Automação específica e, em seguida, o cmdlet **Move-AzureRmResource** para realizar a movimentação.

O primeiro exemplo mostra como mover uma conta de Automação para um novo grupo de recursos.

   ```
    $resource = Get-AzureRmResource -ResourceName "TestAutomationAccount" -ResourceGroupName "ResourceGroup01"
    Move-AzureRmResource -ResourceId $resource.ResourceId -DestinationResourceGroupName "NewResourceGroup"
   ```

Depois de executar o exemplo de código acima, você deverá confirmar se deseja executar esta ação. Após clicar em **Sim** e permitir que o script continue, você não recebe nenhuma notificação durante a execução da migração. 

Para mover para uma nova assinatura, inclua um valor para o parâmetro *DestinationSubscriptionId* .

   ```
    $resource = Get-AzureRmResource -ResourceName "TestAutomationAccount" -ResourceGroupName "ResourceGroup01"
    Move-AzureRmResource -ResourceId $resource.ResourceId -DestinationResourceGroupName "NewResourceGroup" -DestinationSubscriptionId "SubscriptionId"
   ```

Assim como no exemplo anterior, será solicitado que você confirme a mudança. 

## <a name="next-steps"></a>Próximas etapas
* Para saber mais sobre como mover os recursos para um novo grupo de recursos ou assinatura, consulte [Mover recursos para um novo grupo de recursos ou assinatura](../azure-resource-manager/resource-group-move-resources.md)
* Para saber mais sobre o Controle de Acesso baseado em Função na Automação do Azure, consulte [Controle de acesso baseado em função na Automação do Azure](automation-role-based-access-control.md).
* Para saber mais sobre os cmdlets do PowerShell para gerenciar sua assinatura, consulte [Como usar o Azure PowerShell com o Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md)
* Para saber mais sobre os recursos do portal para gerenciar sua assinatura, veja [Usar o Portal do Azure para gerenciar recursos](../azure-resource-manager/resource-group-portal.md).
