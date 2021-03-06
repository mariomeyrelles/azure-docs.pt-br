---
title: Criar, alterar ou excluir um emparelhamento de rede virtual do Azure | Microsoft Docs
description: Saiba como criar, alterar ou excluir um emparelhamento de rede virtual.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/09/2018
ms.author: jdial;anavin
ms.openlocfilehash: 11726b274d72f263ff3defeb7eb7b80594681e15
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/12/2018
ms.locfileid: "34077370"
---
# <a name="create-change-or-delete-a-virtual-network-peering"></a>Criar, alterar ou excluir um emparelhamento da rede virtual

Saiba como criar, alterar ou excluir um emparelhamento de rede virtual. O emparelhamento de rede virtual permite que você conecte duas redes virtuais por meio da rede de backbone do Azure. Depois de emparelhadas, as duas redes virtuais ainda são gerenciadas como recursos separados. Se você é novo em emparelhamento de redes virtuais, pode saber mais sobre isso na [visão geral de emparelhamento de rede virtual](virtual-network-peering-overview.md) ou concluindo um [tutorial](tutorial-connect-virtual-networks-portal.md).

## <a name="before-you-begin"></a>Antes de começar

Conclua as seguintes tarefas antes de concluir as etapas em qualquer seção deste artigo:

- Caso ainda não tenha uma conta do Azure, inscreva-se para obter uma [conta de avaliação gratuita](https://azure.microsoft.com/free).
- Se estiver usando o portal, abra https://portal.azure.come faça logon com uma conta que tenha as [permissões necessárias](#permissions) para trabalhar com emparelhamentos.
- Se usar os comandos do PowerShell para concluir as tarefas neste artigo, execute os comandos no [Azure Cloud Shell](https://shell.azure.com/powershell) ou então executando o PowerShell do computador. O Azure Cloud Shell é um shell interativo grátis que pode ser usado para executar as etapas neste artigo. Ele tem ferramentas do Azure instaladas e configuradas para usar com sua conta. Este tutorial exige o módulo do Azure PowerShell versão 5.7.0 ou posterior. Execute `Get-Module -ListAvailable AzureRM` para localizar a versão instalada. Se você precisa atualizar, consulte [Instalar o módulo do Azure PowerShell](/powershell/azure/install-azurerm-ps). Se você estiver executando o PowerShell localmente, também precisará executar `Connect-AzureRmAccount` com uma conta que tenha [permissões necessárias](#permissions) para trabalhar com o emparelhamento, para criar uma conexão com o Azure.
- Se usar os comandos da CLI (interface de linha de comando) do Azure para concluir as tarefas neste artigo, execute os comandos no [Azure Cloud Shell](https://shell.azure.com/bash) ou então executando a CLI do computador. Este tutorial requer a CLI do Azure versão 2.0.31 ou posterior. Execute `az --version` para localizar a versão instalada. Se você precisa instalar ou atualizar, consulte [Instalar a CLI 2.0 do Azure](/cli/azure/install-azure-cli). Se você estiver executando a CLI do Azure localmente, também precisará executar `az login` com uma conta que tenha [permissões necessárias](#permissions) para trabalhar com o emparelhamento, para criar uma conexão com o Azure.

A conta em que você realizou o logon, ou se conectou ao Azure, deve estar atribuída à função do [contribuidor de rede](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) ou a uma [função personalizada](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) que é atribuída a ações adequadas listadas em [Permissões](#permissions).

## <a name="create-a-peering"></a>Criar um emparelhamento

Antes de criar um emparelhamento, familiarize-se com os [requisitos e restrições](#requirements-and-contstraints) e as [permissões necessárias](#permissions).

1. Na caixa de pesquisa na parte superior do Portal do Azure, digite *redes virtuais* na caixa de pesquisa. Quando **Redes virtuais** aparecer nos resultados da pesquisa, selecione essa opção. Não selecione **redes virtuais (clássicas)** se for exibido na lista, pois não é possível criar um emparelhamento de uma rede virtual implantada por meio do modelo de implantação clássico.
2. Selecione na lista a rede virtual para a qual você deseja criar um emparelhamento.
3. Na lista de redes virtuais, selecione a rede virtual para a qual deseja criar um emparelhamento.
4. Em **CONFIGURAÇÕES**, selecione **Emparelhamentos**.
5. Selecione **+ Adicionar**. 
6. <a name="add-peering"></a>Insira ou selecione valores para as seguintes configurações:
    - **Nome:** O nome para o emparelhamento deve ser exclusivo na rede virtual.
    - **Modelo de implantação de rede virtual:** Selecione com qual modelo de implantação a rede virtual que você deseja emparelhar foi implantada.
    - **Sei minha ID de recurso:** Se você tem acesso de leitura para a rede virtual que você deseja emparelhar, deixe essa caixa de seleção desmarcada. Se você não tiver acesso de leitura para a rede virtual ou a assinatura que você deseja emparelhar, marque essa caixa de seleção. Insira a ID de recurso completo da rede virtual que você deseja emparelhar na caixa **ID de Recurso** exibida quando você tiver marcado a caixa. A ID de recurso que você digitar deverá ser para uma rede virtual existente na mesma [região](https://azure.microsoft.com/regions) ou região [diferente com suporte](#requirements-and-constraints) do Azure que essa rede virtual. A ID de recurso completo é semelhante a /subscriptions/<Id>/resourceGroups/<resource-group-name>/providers/Microsoft.Network/virtualNetworks/<virtual-network-name>. Você pode obter a ID de recurso para uma rede virtual ao exibir as propriedades de uma rede virtual. Para saber como exibir as propriedades de uma rede virtual, confira [Gerenciar as redes virtuais](manage-virtual-network.md#view-virtual-networks-and-settings).
    - **Assinatura:** Selecione a [assinatura](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) da rede virtual que você deseja emparelhar. Uma ou mais assinaturas estão listadas, dependendo de quantas assinaturas sua conta tem acesso de leitura. Se você tiver marcado a caixa de seleção **ID de Recurso**, essa configuração não estará disponível.
    - **Rede virtual:** Selecione a rede virtual que você deseja emparelhar. Você pode selecionar uma rede virtual criada por meio de qualquer um dos modelos de implantação do Azure. Se você deseja selecionar uma rede virtual em uma região diferente, deve selecionar uma rede virtual em um [região com suporte](#cross-region). Você deve ter acesso de leitura para a rede virtual para que ela fique visível na lista. Se uma rede virtual estiver listada, mas esmaecida, pode ser porque o espaço de endereço para a rede virtual se sobrepõe ao espaço de endereço para essa rede virtual. Se os espaços de endereço de rede virtual se sobrepõem, eles não podem ser emparelhados. Se você tiver marcado a caixa de seleção **ID de Recurso**, essa configuração não estará disponível.
    - **Permitir acesso à rede virtual:** Selecione **Habilitado** (padrão), se você deseja habilitar a comunicação entre as duas redes virtuais. Habilitar a comunicação entre redes virtuais permite que os recursos conectados a qualquer rede virtual se comuniquem entre si com a mesma largura de banda e latência como se estivessem conectados à mesma rede virtual. Todas as comunicações entre os recursos nas duas redes virtuais estão na rede privada do Azure. A marca padrão de **Rede Virtual** para os grupos de segurança de rede abrange a rede virtual e a rede virtual emparelhada. Para saber mais sobre grupos de segurança de rede, leia o artigo [Visão geral dos grupos de segurança de rede](virtual-networks-nsg.md#default-tags).  Selecione **Desabilitado** se não quiser que o tráfego flua para a rede virtual emparelhada. Você pode selecionar **Desabilitado** se você tiver emparelhadas uma rede virtual com outra rede virtual, mas, ocasionalmente, deseja desabilitar o fluxo do tráfego entre as duas redes virtuais. Você pode achar que a habilitação e desabilitação é mais conveniente que excluir e recriar emparelhamentos. Quando essa configuração estiver desabilitada, o tráfego não flui entre as redes virtuais emparelhadas.
    - **Permitir trafego encaminhado:** marque essa caixa para permitir que o tráfego *encaminhado* por um dispositivo virtual de rede em uma rede virtual (que não tenha como origem a rede virtual) flua para essa rede virtual por meio de um emparelhamento. Por exemplo, considere três redes virtuais chamadas Spoke1, Spoke2 e Hub. Existe um emparelhamento entre cada rede virtual spoke e a rede virtual hub, mas não existem emparelhamentos entre as redes virtuais spoke. Um dispositivo de rede virtual é implantado na rede virtual Hub e as rotas definidas pelo usuário são aplicadas a cada rede virtual Spoke que roteia tráfego entre as sub-redes por meio do dispositivo de rede virtual. Se essa caixa de seleção não for marcada para o emparelhamento entre cada rede virtual spoke e a rede virtual hub, o tráfego não fluirá entre as redes virtuais spoke porque o hub encaminhará o tráfego entre as redes virtuais. No entanto habilitar esta funcionalidade permite o tráfego encaminhado por meio do emparelhamento, não cria nenhuma rota definida pelo usuário ou dispositivo de rede virtual. Rotas definidas pelo usuário e dispositivos de rede virtual são criados separadamente. Saiba mais sobre [rotas definidas pelo usuário](virtual-networks-udr-overview.md#user-defined). Não será necessário verificar essa configuração, se o tráfego for encaminhado entre redes virtuais por meio de um Gateway de VPN do Azure.
    - **Permitir tráfego de gateway:** Marque essa caixa de seleção se você tiver um gateway de rede virtual conectado a essa rede virtual e deseja permitir o tráfego de rede virtual emparelhado para fluir através do gateway. Por exemplo, esta rede virtual pode ser anexada a uma rede local por meio de um gateway de rede virtual. O gateway pode ser um gateway de VPN ou de ExpressRoute. Marcar essa caixa permite que o tráfego da rede virtual emparelhada flua pelo gateway anexado a esta rede virtual até a rede local. Se você marcar essa caixa, a rede virtual emparelhada não pode ter um gateway configurado. A rede virtual emparelhada deve ter a caixa de seleção **Usar gateways remotos** marcada ao configurar o emparelhamento de outra rede virtual para esta rede virtual. Se você deixar essa caixa desmarcada (padrão), o tráfego da rede virtual emparelhada continuará fluindo para esta rede virtual, mas não poderá fluir através de um gateway de rede virtual conectado a essa rede virtual. Se o emparelhamento estiver entre uma rede virtual (Gerenciador de Recursos) e uma rede virtual (clássica), o gateway deve estar na rede virtual (Gerenciador de Recursos). Você não pode habilitar essa opção se estiver emparelhamento redes virtuais em regiões diferentes.
    
        Além de encaminhar o tráfego para uma rede local, um gateway de VPN pode encaminhar o tráfego de rede entre as redes virtuais emparelhadas com a rede virtual em que o gateway está, sem precisar que as redes virtuais sejam emparelhadas umas com as outras. Usar um gateway VPN para encaminhar o tráfego é útil quando você deseja usar um gateway de VPN em uma rede virtual de hub (consulte o exemplo de hub e de spoke descrito para **Permitir o tráfego encaminhado**) para rotear o tráfego entre as redes virtuais de spoke não emparelhadas umas com as outras. Para saber mais sobre como permitir o uso de um gateway para trânsito, consulte [Configurar um gateway de VPN para trânsito em um emparelhamento de rede virtual](../vpn-gateway/vpn-gateway-peering-gateway-transit.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Esse cenário requer a implementação de rotas definidas pelo usuário que especificam o gateway de rede virtual como o tipo de próximo de salto. Saiba mais sobre [rotas definidas pelo usuário](virtual-networks-udr-overview.md#user-defined). Você só pode especificar um gateway de VPN como o próximo tipo de salto em uma rota definida pelo usuário, não é possível especificar um gateway de ExpressRoute como o próximo tipo de salto em uma rota definida pelo usuário. Você não pode habilitar essa opção se estiver emparelhamento redes virtuais em regiões diferentes.

    - **Usar gateway remotos:** Marque essa caixa de seleção para permitir o tráfego desta rede virtual fluir por meio de um gateway de rede virtual conectado a uma rede virtual que você estiver emparelhando. Por exemplo, a rede virtual que você está emparelhando tem um gateway de VPN anexado que permite a comunicação com uma rede local.  Marcar essa caixa permite que o tráfego da rede virtual flua por meio do gateway de VPN conectado à rede virtual emparelhada. Se você marcar essa caixa, a rede virtual emparelhada deve ter um gateway de rede virtual anexado a ela e deve ter a caixa de seleção **Permitir tráfego de gateway** marcada. Se você deixar essa caixa desmarcada (padrão), o tráfego da rede virtual emparelhada poderá continuar fluindo para esta rede virtual, mas não poderá fluir através de um gateway de rede virtual conectado a essa rede virtual. 
    Apenas um emparelhamento para essa rede virtual pode ter essa configuração habilitada.

        Você não pode usar gateways remotos se já tiver um gateway configurado na rede virtual. Você não pode habilitar essa opção se estiver emparelhamento redes virtuais em regiões diferentes. Para saber mais sobre como usar um gateway para trânsito, consulte [Configurar um gateway de VPN para trânsito em um emparelhamento de rede virtual](../vpn-gateway/vpn-gateway-peering-gateway-transit.md?toc=%2fazure%2fvirtual-network%2ftoc.json)

7. Selecione **OK** para adicionar o emparelhamento à rede virtual que você selecionou.

Para obter instruções passo a passo para a implementação de emparelhamento entre redes virtuais em diferentes assinaturas e modelos de implantação, consulte [próximas etapas](#next-steps). 


### <a name="commands"></a>Comandos

- **CLI do Azure**: [az network vnet peering create](/cli/azure/network/vnet/peering#create)
- **PowerShell**: [Add-AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/add-azurermvirtualnetworkpeering)

## <a name="view-or-change-peering-settings"></a>Exibir ou alterar as configurações de emparelhamento

Antes de alterar um emparelhamento, familiarize-se com os [requisitos e restrições](#requirements-and-contstraints) e as [permissões necessárias](#permissions).

1. Na caixa de pesquisa na parte superior do portal, digite *redes virtuais* na caixa de pesquisa. Quando **Redes virtuais** aparecer nos resultados da pesquisa, selecione essa opção. Não selecione **redes virtuais (clássicas)** se for exibido na lista, pois não é possível criar um emparelhamento de uma rede virtual implantada por meio do modelo de implantação clássico.
2. Selecione na lista a rede virtual para a qual você deseja alterar as configurações de emparelhamento.
3. Na lista de redes virtuais, selecione a rede virtual para a qual deseja alterar as configurações de emparelhamento.
4. Em **CONFIGURAÇÕES**, selecione **Emparelhamentos**.
5. Selecione o emparelhamento que você deseja exibir ou alterar as configurações.
6. Altere a configuração apropriada. Leia sobre as opções para cada configuração na [etapa 6](#add-peering) de Criar um emparelhamento. 
7. Clique em **Salvar**.

**Comandos**

- **CLI do Azure**: [az network vnet peering list](/cli/azure/network/vnet/peering#az_network_vnet_peering_list) para listar emparelhamentos para uma rede virtual, [az network vnet peering show](/cli/azure/network/vnet/peering#az_network_vnet_peering_show) para mostrar as configurações de um emparelhamento específico e [az network vnet peering update](/cli/azure/network/vnet/peering#az_network_vnet_peering_update) para alterar as configurações de emparelhamento.|
- **PowerShell**: [Get-AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/get-azurermvirtualnetworkpeering) para recuperar as configurações de emparelhamento do modo de exibição e [Set-AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/set-azurermvirtualnetworkpeering) para alterar as configurações.

## <a name="delete-a-peering"></a>Excluir um emparelhamento

Antes de excluir um emparelhamento, certifique-se de que sua conta tenha as [permissões necessárias](#permissions).

Quando um emparelhamento é excluído, o tráfego de uma rede virtual não flui mais para a rede virtual emparelhada. Quando as redes virtuais implantadas por meio do Resource Manager são emparelhadas, cada rede virtual tem um emparelhamento para a outra rede virtual. Embora excluindo o emparelhamento de uma rede virtual desabilita a comunicação entre as redes virtuais, ele não exclui o emparelhamento de outra rede virtual. O status de emparelhamento para o emparelhamento existente em outra rede virtual é **Desconectado**. Você não pode recriar o emparelhamento até que recrie o emparelhamento na primeira rede virtual e o status de emparelhamento para ambas as redes virtuais alterar para *Conectado*. 

Se você deseja que as redes virtuais se comuniquem, às vezes, mas não sempre, em vez de excluir um emparelhamento, você pode definir a configuração **Permitir acesso à rede virtual** para **desabilitado**. Para saber como, leia a etapa 6 da seção [Criar um emparelhamento](#create-peering) deste artigo. Você pode achar que desabilitar e habilitar o acesso à rede é mais fácil do que excluir e recriar emparelhamentos.

1. Na caixa de pesquisa na parte superior do portal, digite *redes virtuais* na caixa de pesquisa. Quando **Redes virtuais** aparecer nos resultados da pesquisa, selecione essa opção. Não selecione **redes virtuais (clássicas)** se for exibido na lista, pois não é possível criar um emparelhamento de uma rede virtual implantada por meio do modelo de implantação clássico.
2. Selecione na lista a rede virtual para a qual você deseja excluir um emparelhamento.
3. Na lista de redes virtuais, selecione a rede virtual para a qual deseja excluir um emparelhamento.
4. Em **CONFIGURAÇÕES**, selecione **Emparelhamentos**.
5. No lado direito do emparelhamento que deseja excluir, selecione **...**, selecione **Excluir** e, em seguida, selecione **Sim** para excluir o emparelhamento da primeira rede virtual.
6. Conclua as etapas anteriores para excluir o emparelhamento de outra rede virtual no emparelhamento.

**Comandos**

- **CLI do Azure**: [az network vnet peering delete](/cli/azure/network/vnet/peering#az_network_vnet_peering_delete)
- **PowerShell**: [Remove-AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/remove-azurermvirtualnetworkpeering)

## <a name="requirements-and-constraints"></a>Requisitos e restrições 

- As redes virtuais que você emparelhar devem ter espaços de endereço IP não sobrepostos.
- Você não pode adicionar ou excluir intervalos de endereços do espaço de endereço de uma rede virtual após ela ser emparelhada com outra rede virtual. Para adicionar ou remover intervalos de endereço, exclua o emparelhamento, adicione ou remova os intervalos de endereço e, em seguida, recrie o emparelhamento. Para adicionar ou remover intervalos de endereços de redes virtuais, consulte [Gerenciar redes virtuais](manage-virtual-network.md).
- Você pode emparelhar duas redes virtuais implantadas por meio do Resource Manager ou uma rede virtual implantado por meio do Resource Manager com uma rede virtual implantada por meio do modelo de implantação clássico. Não é possível emparelhar duas redes virtuais criadas por meio do modelo de implantação clássico. Se você não estiver familiarizado com os modelos de implantação do Azure, leia o artigo [Entender os modelos de implantação do Azure](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Você pode usar um [Gateway de VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#V2V) para conectar duas redes virtuais criadas usando o modelo de implantação clássico.
- Ao emparelhar duas redes virtuais criadas por meio do Resource Manager, um emparelhamento deve ser configurado para cada rede virtual no emparelhamento. Você verá um dos seguintes tipos de status de emparelhamento: 
    - *Iniciado:* quando você cria o emparelhamento para a segunda rede virtual da primeira rede virtual, o status de emparelhamento é *Iniciado*. 
    - *Conectado:* quando você cria o emparelhamento da segunda rede virtual para a primeira rede virtual, seu status de emparelhamento é *Conectado*. Se você exibir o status de emparelhamento para a primeira rede virtual, você verá seu status alterado de *Iniciado* para *Conectado*. O emparelhamento não é estabelecido com êxito até que o status de emparelhamento para ambos os emparelhamentos de rede virtual seja *Conectado*.
- Ao emparelhar duas redes virtuais criadas por meio do Resource Manager com uma rede virtual criada por meio do modelo de implantação clássico, você configura apenas um emparelhamento para a rede virtual implantada por meio do Resource Manager. Não é possível configurar o emparelhamento de uma rede virtual (clássica) ou entre duas redes virtuais implantadas por meio do modelo de implantação clássico. Quando você cria o emparelhamento de rede virtual (Resource Manager) para a rede virtual (clássica), o status de emparelhamento é *Atualizando*, em seguida altera rapidamente para *Conectado*.
- Um emparelhamento é estabelecido entre duas redes virtuais. Os emparelhamentos não são provisórios. Se você criar emparelhamentos entre:
    - VirtualNetwork1 e VirtualNetwork2
    - VirtualNetwork2 e VirtualNetwork3

  Não há nenhum emparelhamento entre VirtualNetwork1 e VirtualNetwork3 por meio de VirtualNetwork2. Se você quiser criar um emparelhamento de rede virtual entre VirtualNetwork1 e VirtualNetwork3, precisará criar um emparelhamento entre VirtualNetwork1 e VirtualNetwork3.
- Você não pode resolver nomes em redes virtuais emparelhadas usando a resolução de nomes do Azure padrão. Para resolver nomes em outras redes virtuais, você deve usar o [DNS do Azure para domínios privados](../dns/private-dns-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ou um servidor DNS personalizado. Para saber como configurar seu próprio servidor DNS, consulte [Resolução de nome usando seu próprio servidor DNS](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server).
- Recursos em redes virtuais emparelhadas na mesma região podem se comunicar entre si com a mesma largura de banda e latência como se estivessem na mesma rede virtual. No entanto, o tamanho de cada máquina virtual tem sua própria largura de banda de rede máxima. Para saber mais sobre a largura de banda de rede máxima para diferentes tamanhos de máquina virtual, consulte os tamanhos de máquina virtual do [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ou do [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
- As redes virtuais podem estar na mesma assinatura ou em assinaturas diferentes. Quando as redes virtuais estão em diferentes assinaturas, ambas as assinaturas devem ser associadas ao mesmo locatário do Azure Active Directory. Se você ainda não tiver um locatário do AD, [crie um](../active-directory/develop/active-directory-howto-tenant.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-a-new-azure-ad-tenant) rapidamente. Você pode usar um [Gateway de VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#V2V) para conectar duas redes virtuais que existem em diferentes assinaturas associadas a diferentes locatários do Active Directory.
- Uma rede virtual pode ser emparelhada a outra rede virtual e também estar conectada a outra rede virtual com um gateway de rede virtual do Azure. Quando as redes virtuais são conectadas por meio do emparelhamento e um gateway, fluxos de tráfego entre as redes virtuais por meio da configuração de emparelhamento, em vez do gateway.
- Há um custo nominal para tráfego de entrada e saída que utiliza um emparelhamento de rede virtual. Para saber mais, confira a [página de preço](https://azure.microsoft.com/pricing/details/virtual-network).
* <a name="cross-region"></a>Você pode parear redes virtuais na mesma região ou em regiões diferentes. As seguintes restrições não se aplicam quando ambas as redes virtuais estão na *mesma* região, mas sim quando elas estão pareadas globalmente: 
    - As redes virtuais podem existir em qualquer região de nuvem pública do Azure, mas não em nuvens nacionais do Azure.
    - Os recursos em uma rede virtual não podem se comunicar com o endereço IP de um balanceador de carga interno do Azure na rede virtual emparelhada. O balanceador de carga e os recursos que se comunicam com ele devem estar na mesma rede virtual.
    - Você não pode usar gateways remotos ou permitir o tráfego de gateway. Para usar gateways remotos ou permitir tráfego de gateway, ambas as redes virtuais no emparelhamento devem existir na mesma região. 

## <a name="permissions"></a>Permissões

As contas que você pode usar para trabalhar com o emparelhamento de rede virtual devem ser atribuídas às seguintes funções:

- [Colaborador de Rede](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor): para uma rede virtual implantada por meio do Gerenciador de Recursos.
- [Colaborador de Rede Clássico](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor): para uma rede virtual implantada por meio do modelo de implantação clássico.

Se sua conta não está atribuída a uma das funções anteriores, ela deve ser atribuída a uma [função personalizada](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) que recebe atribuição para as ações necessárias da tabela a seguir:

| Ação | NOME |
|---|---|
| Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write  | Necessário para criar um emparelhamento de a rede virtual A com a rede virtual B. A rede virtual A um deve ser uma rede virtual (Gerenciador de Recursos)                            |
| Microsoft.Network/virtualNetworks/peer/action                   | Necessário para criar um emparelhamento de rede virtual B (Gerenciador de Recursos) com a rede virtual A                                                                                |
| Microsoft.ClassicNetwork/virtualNetworks/peer                   | Necessário para criar um emparelhamento de rede virtual B (clássica) com a rede virtual A                                                                                    |
| Microsoft.Network/virtualNetworks/virtualNetworkPeerings/read   | Ler um emparelhamento de rede virtual   |
| Microsoft.Network/virtualNetworks/virtualNetworkPeerings/delete | Excluir um emparelhamento de rede virtual |

## <a name="next-steps"></a>Próximas etapas

* Um emparelhamento de rede virtual é criado entre redes virtuais criadas com modelos de implantação iguais ou diferentes que existem nas mesmas assinaturas ou em assinaturas diferentes. Conclua um tutorial para um dos seguintes cenários:

    |Modelo de implantação do Azure             | Assinatura  |
    |---------                          |---------|
    |Ambos Resource Manager              |[Idêntica](tutorial-connect-virtual-networks-portal.md)|
    |                                   |[Diferente](create-peering-different-subscriptions.md)|
    |Um Resource Manager, um clássico  |[Idêntica](create-peering-different-deployment-models.md)|
    |                                   |[Diferente](create-peering-different-deployment-models-subscriptions.md)|

* Saiba como criar uma [topologia de rede de hub e spoke](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#virtual network-peering)
* Criar um emparelhamento de rede virtual usando [PowerShell](powershell-samples.md) ou os scripts de exemplo da [CLI do Azure](cli-samples.md) ou usando os modelos do [Azure Resource Manager](template-samples.md)
* Criar e aplicar a [Política do Azure](policy-samples.md) para redes virtuais
