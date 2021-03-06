---
title: Compilar um aplicativo Web Docker Python e PostgreSQL no Azure | Microsoft Docs
description: Saiba como fazer com que um aplicativo Docker Python funcione no Azure com conexão a um banco de dados PostgreSQL.
services: app-service\web
documentationcenter: python
author: berndverst
manager: cfowler
ms.service: app-service-web
ms.workload: web
ms.devlang: python
ms.topic: tutorial
ms.date: 01/28/2018
ms.author: beverst;cephalin
ms.custom: mvc
ms.openlocfilehash: 2728c354a84c4b13b0ad8509d038837733251975
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/23/2018
---
# <a name="build-a-docker-python-and-postgresql-web-app-in-azure"></a>Compilar um aplicativo Web Docker Python e PostgreSQL no Azure

O Aplicativo Web para Contêineres fornece um serviço de hospedagem na Web altamente escalonável e com aplicação automática de patches. Este tutorial mostra como criar um aplicativo Web Docker Python básico no Azure. Você conectará esse aplicativo a um banco de dados PostgreSQL. Quando terminar, você terá um aplicativo Python Flask em execução dentro de um contêiner do Docker no [Serviço de Aplicativo no Linux](app-service-linux-intro.md).

![Aplicativo Python Flask do Docker no Serviço de Aplicativo no Linux](./media/tutorial-docker-python-postgresql-app/docker-flask-in-azure.png)

Neste tutorial, você aprenderá como:

> [!div class="checklist"]
> * Criar um servidor de banco de dados PostgreSQL no Azure
> * Conectar um aplicativo Python ao PostgreSQL
> * Implantar o aplicativo no Azure
> * Atualizar o modelo de dados e reimplantar o aplicativo
> * Gerenciar o aplicativo no portal do Azure

Você pode seguir as etapas deste artigo no macOS. As instruções do Linux e do Windows são as mesmas na maioria dos casos, mas as diferenças não são detalhadas neste tutorial.
 
[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>pré-requisitos

Para concluir este tutorial:

1. [Instalar o Git](https://git-scm.com/)
1. [Instalar o Python](https://www.python.org/downloads/)
1. [Instale e execute o PostgreSQL](https://www.postgresql.org/download/)
1. [Instale o Docker Community Edition](https://www.docker.com/community-edition)

## <a name="test-local-postgresql-installation-and-create-a-database"></a>Testar a instalação do PostgreSQL local e criar um banco de dados

Abra a janela do terminal e execute `psql` para se conectar ao servidor PostgreSQL local.

```bash
sudo -u postgres psql
```

Se sua conexão tiver êxito, o banco de dados PostgreSQL já estará em execução. Caso contrário, certifique-se de que o banco de dados PostgresQL local seja iniciado seguindo as etapas em [Baixe - PostgreSQL Core Distribution](https://www.postgresql.org/download/).

Crie um banco de dados chamado *eventregistration* e configure um usuário de banco de dados separado chamado *manager* com a senha *supersecretpass*.

```bash
CREATE DATABASE eventregistration;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE eventregistration TO manager;
```
Digite `\q` para sair do cliente do PostgreSQL. 

<a name="step2"></a>

## <a name="create-local-python-flask-application"></a>Criar um aplicativo Python Flask local

Nesta etapa, você configura o projeto Python Flask local.

### <a name="clone-the-sample-application"></a>Clonar o aplicativo de exemplo

Abra a janela do terminal e `CD` para um diretório de trabalho.

Execute os comandos a seguir para clonar o repositório de exemplo e vá até a versão *0.1-initialapp*.

```bash
git clone https://github.com/Azure-Samples/docker-flask-postgres.git
cd docker-flask-postgres
git checkout tags/0.1-initialapp
```

Esse repositório de exemplo contém um aplicativo [Flask](http://flask.pocoo.org/). 

### <a name="run-the-application"></a>Executar o aplicativo

> [!NOTE] 
> Em uma etapa posterior, você simplificará esse processo criando um contêiner do Docker para ser usado com o nosso banco de dados de produção.

Instale os pacotes necessários e inicie o aplicativo.

```bash
pip install virtualenv
virtualenv venv
source venv/bin/activate
pip install -r requirements.txt
cd app
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

Quando o aplicativo estiver totalmente carregado, você verá algo semelhante à seguinte mensagem:

```bash
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
INFO  [alembic.runtime.migration] Running upgrade  -> 791cd7d80402, empty message
 * Serving Flask app "app"
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
```

Navegue até `http://localhost:5000` em um navegador. Clique em **Registrar!** e crie um usuário de teste.

![Aplicativo Python Flask em execução localmente](./media/tutorial-docker-python-postgresql-app/local-app.png)

O aplicativo de exemplo Flask armazena dados do usuário no banco de dados. Se você tiver êxito ao registrar um usuário, seu aplicativo está gravando os dados no banco de dados PostgreSQL local.

Para parar o servidor Flask a qualquer momento, digite Ctrl+C no terminal. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-production-postgresql-database"></a>Criar um banco de dados PostgreSQL de produção

Nesta etapa, você cria um banco de dados PostgreSQL no Azure. Quando seu aplicativo é implantado no Azure, ele usa esse banco de dados na nuvem.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

### <a name="create-a-resource-group"></a>Criar um grupo de recursos

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-linux-no-h.md)] 

### <a name="create-an-azure-database-for-postgresql-server"></a>Criar um Banco de Dados do Azure para o servidor PostgreSQL

Criar um servidor PostgreSQL com o comando [`az postgres server create`](/cli/azure/postgres/server?view=azure-cli-latest#az_postgres_server_create).

No comando a seguir, substitua um nome do servidor exclusivo para o espaço reservado  *\<postgresql_name >* e um nome de usuário para o espaço reservado  *\<admin_username >*. Esse nome do servidor é usado como parte de seu ponto de extremidade do PostgreSQL (`https://<postgresql_name>.postgres.database.azure.com`), portanto, ele precisa ser exclusivo entre todos os servidores no Azure. O nome de usuário é necessário para a conta do usuário administrador de banco de dados inicial. É solicitado que você escolha uma senha para esse usuário.

```azurecli-interactive
az postgres server create --resource-group myResourceGroup --name <postgresql_name> --admin-user <admin_username>  --storage-size 51200
```

Quando o servidor PostgreSQL do Banco de Dados do Azure for criado, a CLI do Azure mostrará informações semelhantes ao exemplo a seguir:

```json
{
  "administratorLogin": "<my_admin_username>",
  "fullyQualifiedDomainName": "<postgresql_name>.postgres.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforPostgreSQL/servers/<postgresql_name>",
  "location": "westus",
  "name": "<postgresql_name>",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "capacity": 100,
    "family": null,
    "name": "PGSQLS3M100",
    "size": null,
    "tier": "Basic"
  },
  "sslEnforcement": null,
  "storageMb": 2048,
  "tags": null,
  "type": "Microsoft.DBforPostgreSQL/servers",
  "userVisibleState": "Ready",
  "version": null
}
```

### <a name="create-a-firewall-rule-for-the-azure-database-for-postgresql-server"></a>Criando uma regra de firewall para o Banco de Dados do Azure para o servidor PostgreSQL

Execute o seguinte comando da CLI do Azure para permitir o acesso ao banco de dados a partir de todos os endereços IP. Quando o IP inicial e o IP final estiverem definidos como 0.0.0.0, o firewall estará aberto somente para outros recursos do Azure. 

```azurecli-interactive
az postgres server firewall-rule create --resource-group myResourceGroup --server-name <postgresql_name> --start-ip-address=0.0.0.0 --end-ip-address=0.0.0.0 --name AllowAzureIPs
```

A CLI do Azure confirma a criação da regra de firewall com saída semelhante ao exemplo a seguir:

```json
{
  "endIpAddress": "0.0.0.0",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforPostgreSQL/servers/<postgresql_name>/firewallRules/AllowAzureIPs",
  "name": "AllowAzureIPs",
  "resourceGroup": "myResourceGroup",
  "startIpAddress": "0.0.0.0",
  "type": "Microsoft.DBforPostgreSQL/servers/firewallRules"
}
```

> [!TIP] 
> Você pode ser ainda mais restritivo na regra de firewall ao [usar somente os endereços de IP de saída que seu aplicativo usa](../app-service-ip-addresses.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#find-outbound-ips).
>

## <a name="connect-your-python-flask-application-to-the-database"></a>Conectar o aplicativo Python Flask ao banco de dados

Nesta etapa, você conecta seu aplicativo de exemplo Python Flask ao Banco de Dados do Azure para o servidor PostgreSQL criado.

### <a name="create-an-empty-database-and-set-up-a-new-database-application-user"></a>Criando um banco de dados vazio e configurando um novo usuário do aplicativo de banco de dados

Crie um usuário de banco de dados com acesso somente a um banco de dados individual. Você usará essas credenciais para evitar fornecer acesso completo ao aplicativo para o servidor.

Conecte-se ao banco de dados (sua senha de administrador será solicitada).

```bash
psql -h <postgresql_name>.postgres.database.azure.com -U <my_admin_username>@<postgresql_name> postgres
```

Crie o banco de dados e o usuário usando a CLI do PostgreSQL.

```bash
CREATE DATABASE eventregistration;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE eventregistration TO manager;
```

Digite `\q` para sair do cliente do PostgreSQL.

### <a name="test-the-application-locally-against-the-azure-postgresql-database"></a>Teste o aplicativo localmente no banco de dados PostgreSQL do Azure

Voltando para a pasta *aplicativo* do repositório do Github clonado, podemos executar o aplicativo Python Flask atualizando as variáveis de ambiente do banco de dados.

```bash
FLASK_APP=app.py DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

Quando o aplicativo estiver totalmente carregado, você verá algo semelhante à seguinte mensagem:

```bash
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
INFO  [alembic.runtime.migration] Running upgrade  -> 791cd7d80402, empty message
 * Serving Flask app "app"
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
```

Navegue até http://localhost:5000 em um navegador. Clique em **Registrar!** e crie um registro de teste. Agora, você está gravando dados no banco de dados no Azure.

![Aplicativo Python Flask em execução localmente](./media/tutorial-docker-python-postgresql-app/local-app.png)

### <a name="running-the-application-from-a-docker-container"></a>Executando o aplicativo de um contêiner do Docker

Criar a imagem de contêiner do Docker.

```bash
cd ..
docker build -t flask-postgresql-sample .
```

O Docker exibe uma confirmação de que criou o contêiner com êxito.

```bash
Successfully built 7548f983a36b
```

Na raiz do repositório, adicione um arquivo de variável de ambiente chamado _db.env_ e, em seguida, adicione as seguintes variáveis de ambiente de banco de dados a ele. O aplicativo se conecta ao banco de dados de produção do Banco de dados do Azure para PostgreSQL.

```text
DBHOST=<postgresql_name>.postgres.database.azure.com
DBUSER=manager@<postgresql_name>
DBNAME=eventregistration
DBPASS=supersecretpass
```

Execute o aplicativo de dentro do contêiner do Docker. O seguinte comando especifica o arquivo de variável de ambiente e mapeia a porta 5000 padrão do Flask para nossa porta 5000 local.

```bash
docker run -it --env-file db.env -p 5000:5000 flask-postgresql-sample
```

A saída é semelhante ao que você viu anteriormente. No entanto, a migração de banco de dados inicial não precisa mais ser executada e, portanto, é ignorada.

```bash
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
 * Serving Flask app "app"
 * Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)
```

O banco de dados já contém o registro que você criou anteriormente.

![Aplicativo Python Flask baseado em contêiner do Docker em execução localmente](./media/tutorial-docker-python-postgresql-app/local-docker.png)

## <a name="upload-the-docker-container-to-a-container-registry"></a>Carregar o contêiner do Docker em um registro de contêiner

Nesta etapa, faça upload do contêiner do Docker em um registro de contêiner. Use o Registro de Contêiner do Azure, mas também pode usar outros registros populares, como o Hub do Docker.

### <a name="create-an-azure-container-registry"></a>Criar um Registro de Contêiner do Azure

No seguinte comando para criar um registro de contêiner, substitua *\<registry_name>* por um nome de registro de contêiner do Azure exclusivo de sua escolha.

```azurecli-interactive
az acr create --name <registry_name> --resource-group myResourceGroup --location "West US" --sku Basic
```

Saída

```json
{
  "adminUserEnabled": false,
  "creationDate": "2017-05-04T08:50:55.635688+00:00",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/<registry_name>",
  "location": "westus",
  "loginServer": "<registry_name>.azurecr.io",
  "name": "<registry_name>",
  "provisioningState": "Succeeded",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "storageAccount": {
    "name": "<registry_name>01234"
  },
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

### <a name="retrieve-the-registry-credentials-for-pushing-and-pulling-docker-images"></a>Recuperar as credenciais de registro para enviar e receber imagens do Docker

Para mostrar as credenciais de registro, habilite o modo de administração pela primeira vez.

```azurecli-interactive
az acr update --name <registry_name> --admin-enabled true
az acr credential show -n <registry_name>
```

Você verá duas senhas. Tome nota do nome de usuário e da primeira senha.

```json
{
  "passwords": [
    {
      "name": "password",
      "value": "<registry_password>"
    },
    {
      "name": "password2",
      "value": "<registry_password2>"
    }
  ],
  "username": "<registry_name>"
}
```

### <a name="upload-your-docker-container-to-azure-container-registry"></a>Carregue o contêiner do Docker no Registro de Contêiner do Azure

Faça logon em seu Registro. Mediante solicitação, forneça a senha que você recuperou.

```bash
docker login <registry_name>.azurecr.io -u <registry_name>
```

Envie por push sua imagem do docker para o Registro.

```bash
docker tag flask-postgresql-sample <registry_name>.azurecr.io/flask-postgresql-sample
docker push <registry_name>.azurecr.io/flask-postgresql-sample
```

## <a name="deploy-the-docker-python-flask-application-to-azure"></a>Implantar o aplicativo Docker Python Flask no Azure

Nesta etapa, você implanta seu aplicativo Python Flask baseado em contêiner do Docker no Serviço de Aplicativo do Azure.

### <a name="create-an-app-service-plan"></a>Criar um plano de Serviço de Aplicativo

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

### <a name="create-a-web-app"></a>Criar um aplicativo Web

Crie um aplicativo Web no plano do Serviço de Aplicativo do *myAppServicePlan* com o comando [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create).

O aplicativo Web fornece um espaço de hospedagem para implantar seu código e fornecer uma URL para exibir o aplicativo implantado. Use para criar o aplicativo Web.

No seguinte comando, substitua o espaço reservado *\<app_name>* por um nome de aplicativo exclusivo. Esse nome é parte da URL padrão para o aplicativo Web, portanto, o nome precisa ser exclusivo em todos os aplicativos no Serviço de Aplicativo do Azure.

```azurecli
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan --deployment-container-image-name "<registry_name>.azurecr.io/flask-postgresql-sample"
```

Quando o aplicativo Web tiver sido criado, a CLI do Azure mostrará informações semelhantes ao exemplo a seguir:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "enabled": true,
  ...
  < Output has been truncated for readability >
}
```

### <a name="configure-the-database-environment-variables"></a>Configurar as variáveis de ambiente do banco de dados

No início do tutorial, você definiu as variáveis de ambiente para se conectar a seu banco de dados PostgreSQL.

No Serviço de Aplicativo, defina as variáveis de ambiente como _configurações do aplicativo_ usando o comando [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az_webapp_config_appsettings_set).

O seguinte exemplo especifica os detalhes da conexão de banco de dados como configurações do aplicativo. Ele também usa a variável *PORT* ao mapa PORT 5000 do seu Contêiner do Docker para receber o tráfego HTTP na PORT 80.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBPASS="supersecretpass" DBNAME="eventregistration" PORT=5000
```

### <a name="configure-docker-container-deployment"></a>Configurar a implantação do contêiner do Docker

O Serviço de Aplicativo pode baixar e executar automaticamente um contêiner do Docker.

```azurecli
az webapp config container set --resource-group myResourceGroup --name <app_name> --docker-registry-server-user "<registry_name>" --docker-registry-server-password "<registry_password>" --docker-registry-server-url "https://<registry_name>.azurecr.io"
```

Sempre que você atualizar o contêiner do Docker ou alterar as configurações, reinicie o aplicativo. Reiniciar garante que todas as configurações serão aplicadas e o último contêiner será extraído do registro.

```azurecli-interactive
az webapp restart --resource-group myResourceGroup --name <app_name>
```

### <a name="browse-to-the-azure-web-app"></a>Navegar até o aplicativo Web do Azure 

Navegue até o aplicativo Web implantado usando o navegador da Web. 

```bash 
http://<app_name>.azurewebsites.net 
```
> [!NOTE]
> O aplicativo Web leva mais tempo para carregar porque o contêiner deve ser baixado e iniciado depois que a configuração do contêiner for alterada.

Você verá convidados registrados anteriormente que foram salvos no banco de dados de produção do Azure na etapa anterior.

![Aplicativo Python Flask baseado em contêiner do Docker em execução localmente](./media/tutorial-docker-python-postgresql-app/docker-app-deployed.png)

**Parabéns!** Você está executando um aplicativo Python Flask baseado em um contêiner do Docker no Serviço de Aplicativo do Azure.

## <a name="update-data-model-and-redeploy"></a>Atualizar o modelo de dados e reimplantar

Nesta etapa, você adiciona o número de participantes a cada registro de evento atualizando o modelo de Convidado.

Veja a versão *0.2-migration* com o seguinte comando Git:

```bash
git checkout tags/0.2-migration
```

Essa versão já fez as alterações necessárias nos modos de exibição, controladores e modelo. Ela também inclui uma migração de banco de dados gerada por meio de *alembic* (`flask db migrate`). É possível ver todas as alterações feitas por meio do seguinte comando Git:

```bash
git diff 0.1-initialapp 0.2-migration
```

### <a name="test-your-changes-locally"></a>Testar suas alterações localmente

Execute os comandos a seguir para testar as alterações localmente executando o servidor Flask.

```bash
source venv/bin/activate
cd app
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

Navegue até http://localhost:5000 no navegador para exibir as alterações. Crie um registro de teste.

![Aplicativo Python Flask baseado em contêiner do Docker em execução localmente](./media/tutorial-docker-python-postgresql-app/local-app-v2.png)

### <a name="publish-changes-to-azure"></a>Publicar alterações no Azure

Crie a nova imagem do Docker, envie-a para o registro de contêiner e reinicie o aplicativo.

```bash
cd ..
docker build -t flask-postgresql-sample .
docker tag flask-postgresql-sample <registry_name>.azurecr.io/flask-postgresql-sample
docker push <registry_name>.azurecr.io/flask-postgresql-sample
az appservice web restart --resource-group myResourceGroup --name <app_name>
```

Navegue até seu aplicativo Web do Azure e teste a nova funcionalidade novamente. Crie outro registro de evento.

```bash 
http://<app_name>.azurewebsites.net 
```

![Aplicativo Docker Python Flask no Serviço de Aplicativo do Azure](./media/tutorial-docker-python-postgresql-app/docker-flask-in-azure.png)

## <a name="manage-your-azure-web-app"></a>Gerenciar seu aplicativo Web do Azure

Vá para o [portal do Azure](https://portal.azure.com) para ver o aplicativo Web que você criou.

No menu à esquerda, clique em **Serviço de Aplicativos** e, em seguida, clique no nome do seu aplicativo Web do Azure.

![Navegação do portal para o aplicativo Web do Azure](./media/tutorial-docker-python-postgresql-app/app-resource.png)

Por padrão, o portal mostra a página **Visão geral** do seu aplicativo Web . Esta página fornece uma visão de como está seu aplicativo. Aqui, você também pode executar tarefas básicas de gerenciamento como procurar, parar, iniciar, reiniciar e excluir. As guias no lado esquerdo da página mostram as páginas de configuração diferentes que você pode abrir.

![Página Serviço de Aplicativo no portal do Azure](./media/tutorial-docker-python-postgresql-app/app-mgmt.png)

## <a name="next-steps"></a>Próximas etapas

Vá para o próximo tutorial para saber como mapear um nome DNS personalizado para o seu aplicativo Web.

> [!div class="nextstepaction"]
> [Mapear um nome DNS personalizado existente para aplicativos Web do Azure](../app-service-web-tutorial-custom-domain.md)
