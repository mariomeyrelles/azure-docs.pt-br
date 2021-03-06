---
title: Coletar dados do seu ambiente com o Azure Log Analytics | Microsoft Docs
description: Este tópico ajuda você a entender como coletar dados e monitorar computadores hospedados em suas instalações ou em outro ambiente de nuvem com o Log Analytics.
services: log-analytics
documentationcenter: ''
author: MGoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/02/2018
ms.author: magoedte
ms.openlocfilehash: 2597b434bc6db0d5639709a9ce869462c3e47f56
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/10/2018
---
# <a name="collect-data-from-computers-in-your-environment-with-log-analytics"></a>Coletar dados de computadores em seu ambiente com o Log Analytics

O Log Analytics do Azure pode coletar e agir sobre dados de computadores Windows ou Linux que residem em:

* [Máquinas virtuais do Azure](log-analytics-quick-collect-azurevm.md) usando a extensão de Log Analytics VM 
* Seu data center como servidores físicos ou máquinas virtuais
* Máquinas virtuais em um serviço hospedado em nuvem como os serviços do Amazon Web Services (AWS)

Computadores hospedados no seu ambiente podem ser conectados diretamente ao Log Analytics, ou se você já estiver monitorando esses computadores com o System Center Operations Manager 2012 R2, 2016 ou a versão 1801, você pode integrar seu grupo de gerenciamento Operations Manage com o Logs Analytics e continue mantendo deus processos de operações de serviço de TI.  

## <a name="overview"></a>Visão geral

![log-analytics-agent-direct-connect-diagram](media/log-analytics-concept-hybrid/log-analytics-on-prem-comms.png)

Antes de analisar e atuar em dados coletados, você precisa instalar e conectar agentes para todos os computadores que deseja enviar dados ao serviço do Log Analytics. Você pode instalar agentes nos seus computadores locais usando a Instalação, a linha de comando ou com o DSC (Configuração de Estado Desejado) na Automação do Azure. 

O agente para Linux e Windows se comunica na saída com o serviço Log Analytics na porta TCP 443 e, caso o computador se conecte a um firewall ou servidor proxy para se comunicar pela Internet, veja a [seção de pré-requisitos](#prerequisites) para entender a configuração de rede necessária.  Se suas políticas de segurança de TI não permitirem que os computadores na rede se conectem à Internet, você poderá configurar um [Gateway OMS](log-analytics-oms-gateway.md) e, em seguida, configurar o agente para se conectar por meio do gateway ao Log Analytics. Assim, o agente pode receber informações de configuração e enviar dados coletados dependendo das regras e soluções de coleta de dados habilitadas. 

Se você estiver monitorando o computador com o System Center 2016 – Operations Manager ou Operations Manager 2012 R2, ele poderá ser multihomed com o serviço Log Analytics para coletar dados e encaminhe para o serviço e ainda ser monitorado pelo [Operations Manager](log-analytics-om-agents.md). Os computadores Linux monitorados por um grupo de gerenciamento do Operations Manager integrado ao Log Analytics não recebem a configuração de fontes de dados e encaminham os dados coletados pelo grupo de gerenciamento. O agente para Windows pode relatar até quatro espaços de trabalho, enquanto o agente do Linux oferece suporte a relatórios somente para um único espaço de trabalho.  

O agente para Linux e Windows não serve apenas para a conexão com o Log Analytics, ele também oferece suporte à Automação do Azure para hospedar a função Hybrid Runbook Worker, e soluções de gerenciamento, como o Controle de Alterações e o Gerenciamento de Atualizações.  Para obter mais informações sobre a função Hybrid Runbook Worker, consulte [Hybrid Runbook Worker de Automação do Azure](../automation/automation-hybrid-runbook-worker.md).

## <a name="prerequisites"></a>pré-requisitos
Antes de começar, examine os detalhes a seguir para verificar se você atende aos requisitos mínimos do sistema.

### <a name="windows-operating-system"></a>Sistema operacional Windows
Há suporte oficial para as seguintes versões do sistema operacional Windows para o agente para Windows:

* Windows Server 2008 Service Pack 1 (SP1) ou posterior
* Windows 7 SP1 e posterior.

> [!NOTE]
> O agente para Windows dá suporte apenas ao protocolo TLS (Transport Layer Security) 1.0 e 1.1.  

#### <a name="network-configuration"></a>Configuração de rede
As informações abaixo listam as informações de configuração de proxy e firewall necessárias para que o agente do Windows se comunique com o Log Analytics. O tráfego é de saída da rede para o serviço Log Analytics. 

| Recurso de agente | Portas | Ignorar a inspeção de HTTPS|
|----------------|-------|------------------------|
|*.ods.opinsights.azure.com |443 | sim |
|*.oms.opinsights.azure.com | 443 | sim | 
|*.blob.core.windows.net | 443 | sim | 
|*.azure-automation.net | 443 | sim | 

### <a name="linux-operating-systems"></a>Sistemas operacionais Linux
As seguintes distribuições Linux têm suporte oficialmente.  No entanto, o agente para Linux também pode ser executado em outras distribuições não listadas.  Salvo indicação em contrário, todas as versões secundárias são compatíveis com cada versão principal listada.  

* Amazon Linux 2012.09 a 2015.09 (x86/x64)
* CentOS Linux 5, 6 e 7 (x86/x64)  
* Oracle Linux 5, 6 e 7 (x86/x64) 
* Red Hat Enterprise Linux Server 5, 6 e 7 (x86/x64)
* Debian GNU/Linux 6, 7 e 8 (x86/x64)
* Ubuntu 12.04 LTS, 14.04 LTS, 16.04 LTS (x86/x64)
* SUSE Linux Enterprise Server 11 e 12 (x86/x64)

#### <a name="network-configuration"></a>Configuração de rede
As informações abaixo listam as informações de configuração de proxy e firewall necessárias para que o agente do Linux se comunique com o Log Analytics.  

|Recurso de agente| Portas | Direção |  
|------|---------|--------|  
|*.ods.opinsights.azure.com | Porta 443 | Entrada e saída|  
|*.oms.opinsights.azure.com | Porta 443 | Entrada e saída|  
|*.blob.core.windows.net | Porta 443 | Entrada e saída|  
|*.azure-automation.net | Porta 443 | Entrada e saída|  

O agente para Linux dá suporte à comunicação por meio de um servidor proxy ou de um gateway do OMS para o serviço Log Analytics usando o protocolo HTTPS.  Há suporte para a autenticação anônima e básica (nome de usuário/senha).  O servidor proxy pode ser especificado durante a instalação ou modificando o arquivo de configuração proxy.conf após a instalação.  

O valor de configuração de proxy tem a seguinte sintaxe:

`[protocol://][user:password@]proxyhost[:port]`

> [!NOTE]
> Se seu servidor proxy não exigir autenticação, o agente para Linux exigirá mesmo assim fornecendo um pseudo usuário/senha. Isso pode ser qualquer nome de usuário ou senha.

|Propriedade| DESCRIÇÃO |
|--------|-------------|
|Protocolo | HTTPS |
|usuário | Nome de usuário opcional para autenticação de proxy |
|Senha | Senha opcional para autenticação de proxy |
|proxyhost | Endereço ou FQDN do servidor proxy/Gateway do OMS |
|porta | Número da porta opcional para o servidor proxy/OMS do Gateway |

Por exemplo: `https://user01:password@proxy01.contoso.com:30443`

> [!NOTE]
> Se você usar caracteres especiais, como \"\@\" em sua senha, você receberá um erro de conexão de proxy porque o valor é analisado incorretamente.  Para contornar esse problema, codifique a senha na URL usando uma ferramenta como [URLDecode](https://www.urldecoder.org/).  

## <a name="install-and-configure-agent"></a>Instalar e configurar o agente 
Conectar seus computadores locais diretamente com o Log Analytics pode ser feito usando métodos diferentes, dependendo dos seus requisitos. A tabela a seguir realça cada método para determinar o que funciona melhor em sua organização.

|Fonte | Método | DESCRIÇÃO|
|-------|-------------|-------------|
| Computador com Windows|- [Instalação manual](log-analytics-agent-windows.md)<br>- [DSC de Automação do Azure](log-analytics-agent-windows.md#install-the-agent-using-dsc-in-azure-automation)<br>- [Modelo do Resource Manager com o Azure Stack](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/MicrosoftMonitoringAgent-ext-win) |Instalar o agente Microsoft Monitoring da linha de comando ou usando um método automatizado como DSC de automação do Azure, [System Center Configuration Manager](https://docs.microsoft.com/sccm/apps/deploy-use/deploy-applications), ou com um modelo do Azure Resource Manager, se você implantou o Microsoft Azure Stack no seu datacenter.| 
|Computador Linux| [Instalação manual](log-analytics-quick-collect-linux-computer.md)|Instale o agente para Linux chamando um script de wrapper hospedado no GitHub. | 
| System Center Operations Manager|[Conectar o Operations Manager ao Log Analytics](log-analytics-om-agents.md) | Configure a integração entre o Operations Manager e o Log Analytics para encaminhar dados coletados de computadores Linux e Windows subordinados a um grupo de gerenciamento.|  

## <a name="next-steps"></a>Próximas etapas

* Consultar as [fontes de dados](log-analytics-data-sources.md) para entender as fontes de dados disponíveis para coletar dados do sistema Windows ou Linux. 

* Saiba mais sobre [pesquisas de log](log-analytics-log-searches.md) para analisar os dados coletados de fontes de dados e soluções. 

* Saiba mais sobre [soluções](log-analytics-add-solutions.md) que adicionam funcionalidades ao Log Analytics e também coletam dados para o repositório do OMS.