---
title: Gerenciar contas de armazenamento do Azure pilha | Microsoft Docs
description: Saiba como localizar, gerenciar, recuperar e recuperar as contas de armazenamento do Azure pilha
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 627d355b-4812-45cb-bc1e-ce62476dab34
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: get-started-article
ms.date: 05/10/2018
ms.author: mabrigg
ms.reviewer: xiaofmao
ms.openlocfilehash: 2ae2b628b2e61893a5289151c3b405e7412e7d13
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/12/2018
---
# <a name="manage-storage-accounts-in-azure-stack"></a>Gerenciar contas de armazenamento na pilha do Azure
Saiba como gerenciar contas de armazenamento na pilha do Azure para localizar, recuperar e recuperar a capacidade de armazenamento com base nas necessidades de negócios.

## <a name="find"></a>Localizar uma conta de armazenamento
A lista de contas de armazenamento na região pode ser exibida na pilha do Azure por:

1. Em um navegador da Internet, navegue até https://adminportal.local.azurestack.external.
2. Entre no portal de administração do Azure pilha como um operador de nuvem (usando as credenciais fornecidas durante a implantação)
3. No painel padrão – localizar o **gerenciamento região** lista e selecione a região que você deseja explorar, por exemplo **(local**).
   
   ![](media/azure-stack-manage-storage-accounts/image1.png)
4. Selecione **armazenamento** do **provedores de recursos** lista.
   
   ![](media/azure-stack-manage-storage-accounts/image2.png)
5. Agora, no painel de administrador do provedor de recursos de armazenamento – Role para baixo até o **contas de armazenamento** guia e selecioná-lo.
   
   ![](media/azure-stack-manage-storage-accounts/image3.png)
   
   A página resultante é a lista de contas de armazenamento nessa região.
   
   ![](media/azure-stack-manage-storage-accounts/image4.png)

Por padrão, as contas de 10 primeiros são exibidas. Você pode escolher buscar mais clicando o **carregar mais** link na parte inferior da lista.

OU

Se você estiver interessado em uma determinada conta de armazenamento – você pode **filtrar e buscar as contas relevantes** somente.


**Para filtrar contas:**

1. Selecione **filtro** na parte superior do painel.
2. Painel filtro, ele permite que você especifique **nome da conta**, * * ID de assinatura, ou **status** para ajustar a lista de contas de armazenamento a ser exibido. Use-os conforme apropriado.
3. Selecione **Atualização**. Atualize a lista adequadamente.
   
    ![](media/azure-stack-manage-storage-accounts/image5.png)
4. Para redefinir o filtro: selecione **filtro**, limpar as seleções e atualizar.

A caixa de texto de pesquisa (na parte superior do painel de lista de contas de armazenamento) permite que você realce o texto selecionado na lista de contas. Você pode usar isso quando o nome completo ou a ID não está facilmente disponível.

Você pode usar texto livre aqui para ajudar a localizar a conta que você está interessado.

![](media/azure-stack-manage-storage-accounts/image6.png)

## <a name="look-at-account-details"></a>Examine os detalhes da conta
Depois de localizar as contas que você está interessado em Visualizar, você pode selecionar a conta específica para exibir alguns detalhes. Um novo painel é aberta com os detalhes da conta, como: o tipo da conta, hora de criação, local, etc.

![](media/azure-stack-manage-storage-accounts/image7.png)

## <a name="recover-a-deleted-account"></a>Recuperar uma conta excluída
Você pode estar em uma situação em que você precisa recuperar uma conta excluída.

Na pilha do Azure, há uma maneira simples de fazer isso:

1. Navegue até a lista de contas de armazenamento. Consulte [encontrar uma conta de armazenamento](#find) neste tópico para obter mais informações.
2. Localize a conta específica na lista. Você pode precisar filtrar.
3. Verifique o *estado* da conta. Ele deve indicar **excluído**.
4. Selecione a conta, que abre o painel de detalhes da conta.
5. Na parte superior deste painel, localize o **recuperar** botão e selecione-o.
6. Clique em **Sim** para confirmar.
   
   ![](media/azure-stack-manage-storage-accounts/image8.png)
7. A recuperação está agora no *processar... Aguarde* para uma indicação de que ela foi bem-sucedida.
   Você também pode selecionar o ícone de "sino" na parte superior do portal para exibir indicações de andamento.
   
   ![](media/azure-stack-manage-storage-accounts/image9.png)
   
   Depois que a conta recuperada é sincronizada com êxito, ele pode ser usado novamente.

### <a name="some-gotchas"></a>Algumas armadilhas
* Sua conta excluída mostra estado como **fora de retenção**.
  
  Fora da retenção significa que a conta excluída excedeu o período de retenção e não pode ser recuperada.
* Sua conta excluída não será mostrado na lista de contas.
  
  Sua conta pode não aparecer na lista de contas quando a conta excluída já foi coletado como lixo. Nesse caso, ele não pode ser recuperado. Consulte [recuperar a capacidade](#reclaim) neste tópico.

## <a name="set-the-retention-period"></a>Definir o período de retenção
A configuração do período de retenção permite que um operador de nuvem especificar um período de tempo em dias (entre 0 e 9999 dias) durante o qual qualquer conta excluída potencialmente pode ser recuperada. O período de retenção padrão é definido como 0 dias. Definir o valor como "0" significa que qualquer conta excluída imediatamente está fora de retenção e marcado para coleta de lixo periódica.

**Para alterar o período de retenção:**

1. Em um navegador da internet, navegue até https://adminportal.local.azurestack.external.
2. Entre no portal de administração do Azure pilha como um operador de nuvem (usando as credenciais fornecidas durante a implantação)
3. No painel padrão – localizar o **gerenciamento região** lista e selecione a região que você deseja explorar – por exemplo **(local**).
4. Selecione **armazenamento** do **provedores de recursos** lista.
5. Selecione **configurações** na parte superior para abrir o painel de configuração.
6. Selecione **configuração** , em seguida, edite o valor do período de retenção.

   Definir o número de dias e, em seguida, salvá-lo.
   
   Esse valor estará em vigor imediatamente e é definido para a sua região inteira.

   ![](media/azure-stack-manage-storage-accounts/image10.png)

## <a name="reclaim"></a>Recuperar a capacidade
Um dos efeitos colaterais de ter um período de retenção é que uma conta excluída continua a consumir capacidade até que ele fique fora do período de retenção. Como um operador de nuvem, talvez seja necessário uma maneira de recuperar o espaço de conta excluída mesmo que o período de retenção ainda não expirou.

Você pode recuperar a capacidade usando o portal ou o PowerShell.

**Para recuperar a capacidade de usar o portal:**
1. Navegue até o painel de contas de armazenamento. Consulte [encontrar uma conta de armazenamento](#find).
2. Selecione **recuperar espaço** na parte superior do painel.
3. Ler a mensagem e, em seguida, selecione **Okey**.

    ![](media/azure-stack-manage-storage-accounts/image11.png)
4. Aguarde até ver o ícone de sino no portal de notificação de êxito.

    ![](media/azure-stack-manage-storage-accounts/image12.png)
5. Atualize a página de contas de armazenamento. As contas excluídas não são mostradas na lista porque eles foram removidos.

Você pode também usar o PowerShell para substituir explicitamente o período de retenção e recuperar a capacidade imediatamente.

**Para recuperar a capacidade de usar o PowerShell:**   

1. Confirme que você tenha instalado e configurado o PowerShell do Azure. Caso contrário, use as instruções a seguir: 
   * Para instalar a versão mais recente do PowerShell do Azure e associá-lo a sua assinatura do Azure, consulte [como instalar e configurar o Azure PowerShell](http://azure.microsoft.com/documentation/articles/powershell-install-configure/).
   Para obter mais informações sobre os cmdlets do Gerenciador de recursos do Azure, consulte [usando o PowerShell do Azure com o Gerenciador de recursos do Azure](http://go.microsoft.com/fwlink/?LinkId=394767)
2. Execute os seguintes cmdlets:

> [!NOTE]
> Se você executar esses cmdlets, você excluir permanentemente a conta e seu conteúdo. Ele não é recuperável. Use isso com cuidado.

```PowerShell  
    $farm_name = (Get-AzsStorageFarm)[0].name
    Start-AzsReclaimStorageCapacity -FarmName $farm_name
````

Para obter mais informações, consulte [documentação do PowerShell de pilha do Azure.](https://msdn.microsoft.com/library/mt637964.aspx)
 

## <a name="next-steps"></a>Próximas etapas

 - Para obter informações sobre como gerenciar permissões, consulte [Manage Role-Based o controle de acesso](azure-stack-manage-permissions.md).
 - Para obter informações sobre como gerenciar a capacidade de armazenamento para a pilha do Azure, consulte [gerenciar a capacidade de armazenamento do Azure pilha](azure-stack-manage-storage-shares.md).