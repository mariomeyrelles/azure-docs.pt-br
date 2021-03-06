---
title: 'Azure PowerShell: criar um Banco de Dados SQL | Microsoft Docs'
description: Aprenda a criar um servidor lógico do Banco de Dados SQL, uma regra de firewall no nível de servidor e bancos de dados com o portal do Azure.
keywords: tutorial do banco de dados SQL, criar um banco de dados SQL
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: mvc,DBs & servers
ms.devlang: PowerShell
ms.topic: quickstart
ms.date: 04/01/2018
ms.author: carlrab
ms.openlocfilehash: 91c92de4d7c94cceec69b19647b1fe0bf31915c4
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2018
---
# <a name="create-a-single-azure-sql-database-using-powershell"></a>Criar um único Banco de Dados SQL do Azure usando o PowerShell

O PowerShell é usado para criar e gerenciar recursos do Azure da linha de comando ou em scripts. Este guia detalha o uso do PowerShell para implantar um Banco de Dados SQL do Azure em um [grupo de recursos do Azure](../azure-resource-manager/resource-group-overview.md) em um [servidor lógico do Banco de Dados SQL do Azure](sql-database-features.md).

Se você não tiver uma assinatura do Azure, crie uma conta [gratuita](https://azure.microsoft.com/free/) antes de começar.

Este tutorial requer o módulo do Azure PowerShell, versão 4.0 ou posterior. Execute ` Get-Module -ListAvailable AzureRM` para encontrar a versão. Se você precisar instalá-lo ou atualizá-lo, confira [Instalar o módulo do Azure PowerShell](/powershell/azure/install-azurerm-ps).

## <a name="log-in-to-azure"></a>Fazer logon no Azure

Faça logon em sua assinatura do Azure usando o comando [Connect-AzureRmAccount](/powershell/module/azurerm.profile/connect-azurermaccount) e siga as instruções na tela.

```powershell
Connect-AzureRmAccount
```

## <a name="create-variables"></a>Criar variáveis

Defina quais variáveis usar nos scripts neste início rápido.

```powershell
# The data center and resource name for your resources
$resourcegroupname = "myResourceGroup"
$location = "WestEurope"
# The logical server name: Use a random value or replace with your own value (do not capitalize)
$servername = "server-$(Get-Random)"
# Set an admin login and password for your database
# The login information for the server
$adminlogin = "ServerAdmin"
$password = "ChangeYourAdminPassword1"
# The ip address range that you want to allow to access your server - change as appropriate
$startip = "0.0.0.0"
$endip = "0.0.0.0"
# The database name
$databasename = "mySampleDatabase"
```

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Crie um [grupo de recursos do Azure](../azure-resource-manager/resource-group-overview.md) usando o comando [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Um grupo de recursos é um contêiner lógico no qual os recursos do Azure são implantados e gerenciados em grupo. O exemplo a seguir cria um grupo de recursos denominado `myResourceGroup` no local `westeurope`.

```powershell
New-AzureRmResourceGroup -Name $resourcegroupname -Location $location
```
## <a name="create-a-logical-server"></a>Criar um servidor lógico

Crie um [servidor lógico do Banco de Dados SQL do Azure](sql-database-features.md) usando o comando [New-AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver). Um servidor lógico contém um grupo de bancos de dados gerenciados conjuntamente. O exemplo a seguir cria um servidor nomeado aleatoriamente no seu grupo de recursos com logon de administrador `ServerAdmin` e senha `ChangeYourAdminPassword1`. Substitua esses valores predefinidos como desejado.

```powershell
New-AzureRmSqlServer -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -Location $location `
    -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
```

## <a name="configure-a-server-firewall-rule"></a>Configurar uma regra de firewall de servidor

Crie uma [regra de firewall no nível do servidor do Banco de Dados SQL do Azure](sql-database-firewall-configure.md) usando o comando [New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule). Uma regra de firewall no nível de servidor permite que um aplicativo externo, como o SQL Server Management Studio ou o utilitário SQLCMD, se conecte ao banco de dados SQL através do firewall do serviço de Banco de Dados SQL. No exemplo a seguir, o firewall está aberto somente para os outros recursos do Azure. Para habilitar a conectividade externa, altere o endereço IP para um endereço apropriado para seu ambiente. Para abrir todos os endereços IP, use 0.0.0.0 como o endereço IP inicial e 255.255.255.255 como o endereço final.

```powershell
New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -FirewallRuleName "AllowSome" -StartIpAddress $startip -EndIpAddress $endip
```

> [!NOTE]
> O Banco de Dados SQL se comunica pela porta 1433. Se você estiver tentando conectar-se a partir de uma rede corporativa, o tráfego de saída pela porta 1433 poderá não ser permitido pelo firewall de sua rede. Se isto acontecer, você não conseguirá conectar seu servidor do Banco de Dados SQL do Azure, a menos que o departamento de TI abra a porta 1433.
>

## <a name="create-a-database-in-the-server-with-sample-data"></a>Criar um banco de dados no servidor com dados de exemplo

Crie um banco de dados com um [Nível de desempenho do S0](sql-database-service-tiers-dtu.md) no servidor usando o comando [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase). O exemplo a seguir cria um banco de dados denominado `mySampleDatabase` e carrega os dados de exemplo AdventureWorksLT nesse banco de dados. Substitua os valores predefinidos conforme desejado (outros inícios rápidos nesta coleção aproveitam os valores deste início rápido).

```powershell
New-AzureRmSqlDatabase  -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -DatabaseName $databasename `
    -SampleName "AdventureWorksLT" `
    -RequestedServiceObjectiveName "S0"
```

## <a name="clean-up-resources"></a>Limpar recursos

Outro início rápido nesta coleção baseado neste início rápido.

> [!TIP]
> Se você pretende continuar para trabalhar com os inícios rápidos seguintes, não limpe os recursos criados neste início rápido. Caso contrário, siga estas etapas para excluir todos os recursos criados por esse início rápido no Portal do Azure.
>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $resourcegroupname
```

## <a name="next-steps"></a>Próximas etapas

- Agora que você tem um banco de dados, é possível [se conectar e consultar](sql-database-connect-query.md) usando uma das suas ferramentas ou linguagens favoritas.
- Para saber como projetar seu primeiro banco de dados, criar tabelas e inserir dados, consulte um destes tutoriais:
 - [Projetar seu primeiro banco de dados SQL do Azure usando o SSMS](sql-database-design-first-database.md)
  - [Criar um banco de dados SQL do Azure e conectar-se com C# e o ADO.NET](sql-database-design-first-database-csharp.md)

