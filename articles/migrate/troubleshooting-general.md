---
title: Solucionar problemas das Migrações para Azure | Microsoft Docs
description: Fornece uma visão geral dos problemas conhecidos no serviço de Migrações para Azure e dicas de solução de erros comuns.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: troubleshooting
ms.date: 05/15/2018
ms.author: raynew
ms.openlocfilehash: a878bab2bef31ff853dbad503a706e1a8d5803fe
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/16/2018
---
# <a name="troubleshoot-azure-migrate"></a>Solucionar problemas das Migrações para Azure

## <a name="troubleshoot-common-errors"></a>Solução de problemas comuns

As [Migrações para Azure](migrate-overview.md) avaliam as cargas de trabalho locais para migração para o Azure. Use este artigo para solucionar problemas ao implementar e usar as Migrações para Azure.


**O coletor não é capaz de se conectar à internet**

Isso pode acontecer quando a máquina que você está usando estiver atrás de um proxy. Certifique-se de fornecer as credenciais de autorização se precisar de um proxy.
Se você estiver usando qualquer proxy firewall baseado em URL para controlar a conectividade de saída, certifique-se de permitir estes URLs exigidos:

**URL** | **Finalidade**  
--- | ---
*. portal.azure.com | É necessário verificar a conectividade com o serviço do Azure e validar problemas sincronização de hora.
*. oneget.org | É necessário fazer o download do powershell com base no módulo vCenter PowerCLI.

**O coletor não consegue se conectar ao projeto usando a ID e a chave do projeto copiados do portal.**

Confira se você copiou e colou as informações corretas. Para solucionar o problema, instale o Microsoft Monitoring Agent (MMA) e verifique se o MMA pode se conectar ao projeto da seguinte maneira:

1. Na VM do coletor, baixe o [MMA](https://go.microsoft.com/fwlink/?LinkId=828603).
2. Para iniciar a instalação, clique duas vezes no arquivo baixado.
3. Na instalação, na página **Boas-vindas**, clique em **Avançar**. Na página **Termos de Licença**, clique em **Concordo** para aceitar a licença.
4. Em **Pasta de Destino**, mantenha ou modifique a pasta de instalação padrão > **Avançar**.
5. Em **Opções de Configuração do Agente**, selecione **Azure Log Analytics** > **Avançar**.
6. Clique em **Adicionar** para adicionar um espaço de trabalho do Log Analytics. Cole a ID do projeto e a chave que você copiou. Em seguida, clique em **Próximo**.
7. Verifique se o agente pode se conectar ao projeto. Se não for possível, verifique as configurações. Se o agente pode se conectar, mas não o coletor, contate o suporte.


**Erro 802: recebo um erro de sincronização de data e hora.**

O relógio do servidor pode estar fora de sincronização com a hora atual em mais de cinco minutos. Altere a hora do relógio na VM do coletor para corresponder à hora atual da seguinte maneira:

1. Abra um prompt de comando de administrador na VM.
2. Para verificar o fuso horário, execute w32tm /tz.
3. Para sincronizar a hora, execute w32tm /resync.

**Minha chave de projeto tem símbolos “==” até o fim. Esses são codificados com outros caracteres alfanuméricos pelo coletor. Isso é esperado?**

Sim, todas as chaves de projeto terminam com "==". O coletor criptografa a chave de projeto antes de processá-la.

**Os dados de desempenho para discos e adaptadores de rede mostram como zeros**

Isso poderá ocorrer se o nível de configuração de estatísticas no servidor do vCenter estiver definido como menos de três. No nível três ou superior, o vCenter armazena o histórico de desempenho da VM para computação, armazenamento e rede. Para menos do que o nível três, o vCenter não armazena dados de armazenamento e rede, mas somente dados de CPU e memória. Nesse cenário, os dados de desempenho são exibidos como zero nas Migrações para Azure e as Migrações para Azure fornecem recomendações de tamanho para discos e redes com base nos metadados coletados nas máquinas locais.

Para habilitar a coleta de dados de desempenho de disco e rede, altere o nível de configurações de estatísticas para três. Em seguida, aguarde pelo menos um dia para descobrir o seu ambiente e avaliá-lo.

**Eu instalei agentes e usei a visualização de dependência para criar grupos. Agora, após o failover, as máquinas mostram a ação de "Instalar o agente" em vez de "Exibir dependências"**
* O failover pós-planejado ou não planejado, as máquinas locais são desativadas e as máquinas equivalentes são criadas no Azure. Essas máquinas adquirem um endereço MAC diferente. Elas podem adquirir um endereço IP diferente com base em se o usuário optou por reter o endereço IP local ou não. Se os endereços IP e MAC forem diferentes, as Migrações para Azure não associarão as máquinas locais com os dados de dependência do Mapa do Serviço e solicitarão que o usuário instale os agentes em vez de exibir as dependências.
* Após o failover de teste, as máquinas locais permanecem ativadas conforme o esperado. As máquinas equivalentes criadas no Azure adquirem um endereço MAC diferente e podem adquirir um endereço IP diferente. A menos que o usuário bloqueie o tráfego de Log Analytics de saída desses computadores, as Migrações para Azure não associarão os computadores locais com os dados de dependência do Mapa do Serviço e solicitarão que o usuário instale os agentes em vez de exibir as dependências.


## <a name="troubleshoot-readiness-issues"></a>Solucionar problemas de preparação

**Problema** | **Correção**
--- | ---
Tipo de inicialização sem suporte | O Azure não oferece suporte a VMs com o tipo de inicialização EFI. É recomendável converter o tipo de inicialização em BIOS antes de executar uma migração. <br/><br/>Você pode usar o [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/tutorial-migrate-on-premises-to-azure) para fazer a migração de tais VMs conforme ele converter o tipo de inicialização da VM em BIOS durante a migração.
SO Windows com suporte condicional | O sistema operacional atingiu o fim da data de suporte e precisa de um Contrato de Suporte Personalizado (CSA) para [suporte no Azure](https://aka.ms/WSosstatement), considere o upgrade do sistema operacional antes de migrar para o Azure.
SO Windows sem suporte | O Azure suporta apenas as [versões de SO Windows selecionadas](https://aka.ms/WSosstatement), considere atualizar o sistema operacional do computador antes de migrar para o Azure.
SO Linux condicionalmente endossado | O Azure endossa apenas as [versões de SO Linux selecionadas](../virtual-machines/linux/endorsed-distros.md), considere atualizar o sistema operacional do computador antes de migrar para o Azure.
SO Linux não endossado | O computador pode ser inicializado no Azure, mas nenhum suporte de sistema operacional é fornecido pelo Azure, considere atualizar o sistema operacional para uma [versão do Linux endossada](../virtual-machines/linux/endorsed-distros.md) antes de migrar para o Azure
Sistema operacional desconhecido | O sistema operacional da VM foi especificado como 'Outro' no vCenter Server. Por esse motivo, o Migrações para Azure não consegue identificar a prontidão do Azure da VM. Certifique-se de que o sistema operacional em execução na máquina seja [compatível](https://aka.ms/azureoslist) com o Azure antes de migrar a máquina.
Número de bit do sistema operacional sem suporte | VMs com sistemas operacionais de 32 bits podem ser inicializadas no Azure, mas é recomendável atualizar o sistema operacional da VM de 32 bits para 64 bits antes de migrar para o Azure.
Exige uma assinatura do Visual Studio. | Os computadores têm um sistema operacional cliente Windows em execução com suporte apenas na assinatura do Visual Studio.
A VM não foi encontrada para o desempenho de armazenamento necessário. | O desempenho do armazenamento (IOPS/taxa de transferência) necessário para a máquina excede o suporte de VM do Azure. Reduza os requisitos de armazenamento para a máquina antes da migração.
A VM não foi encontrada para o desempenho de rede necessário. | O desempenho de rede (entrada/saída) necessário para a máquina excede o suporte de VM do Azure. Reduza os requisitos de rede para a máquina.
A VM não foi encontrada no tipo de preço especificado. | Se o tipo de preço for definido como Padrão, considere a possibilidade de redução do tamanho da VM antes de migrar para o Azure. Se a camada de dimensionamento for Básico, considere a alteração do tipo de preço da avaliação para Padrão.
A VM não foi encontrada no local especificado. | Use um local de destino diferente antes da migração.
Um ou mais discos inadequados. | Um ou mais discos anexados à VM não atendem aos requisitos do Azure. Para cada disco anexado à VM, certifique-se de que o tamanho do disco seja < 4 TB, caso contrário, reduza o tamanho de disco antes de migrar para o Azure. Certifique-se de que o desempenho (IOPS/taxa de transferência) necessário para cada disco seja compatível com os [discos de máquina virtual gerenciada](https://docs.microsoft.com/azure/azure-subscription-service-limits#storage-limits) do Azure.   
Um ou mais adaptadores de rede inadequados. | Remova os adaptadores de rede não utilizados do computador antes da migração.
A contagem de disco excede o limite | Remova os discos não utilizados do computador antes da migração.
O tamanho do disco excede o limite | O Azure oferece suporte a discos com tamanho de até 4 TB. Reduza discos para menos de 4 TB antes da migração.
O disco não está disponível no local especificado | Verifique se o disco está em seu local de destino antes de migrar.
O disco não está disponível para a redundância especificada | O disco deve usar o tipo de armazenamento de redundância definido nas configurações de avaliação (LRS por padrão).
Não foi possível determinar a adequação do disco devido a um erro interno | Tente criar uma nova avaliação para o grupo.
VM com núcleos necessários e memória não encontrada | O Azure não pôde encontrar um tipo adequado de VM. Reduza a memória e o número de núcleos da máquina local antes de migrar.
Não foi possível determinar a adequação da VM devido a um erro interno. | Tente criar uma nova avaliação para o grupo.
Não foi possível determinar a adequação de um ou mais discos devido a um erro interno. | Tente criar uma nova avaliação para o grupo.
Não foi possível determinar a adequação de um ou mais adaptadores de rede devido a um erro interno. | Tente criar uma nova avaliação para o grupo.


## <a name="collect-logs"></a>Coletar logs

**Como faço para coletar logs na VM do coletor?**

O registro em logs é habilitado por padrão. Os logs estão localizados da seguinte maneira:

- C:\Profiler\ProfilerEngineDB.sqlite
- C:\Profiler\Service.log
- C:\Profiler\WebApp.log

Para coletar Rastreamento de Eventos para Windows, faça o seguinte:

1. Na VM do coletor, abra uma janela de comando do PowerShell.
2. Execute **Aplicativo Get-EventLog -LogName | export-csv eventlog.csv**.

**Como faço para coletar logs de tráfego da rede do portal?**

1. Abra o navegador e navegue e faça logon [no portal](https://portal.azure.com).
2. Pressione F12 para iniciar as Ferramentas para Desenvolvedores. Se necessário, desmarque a configuração **Limpar entradas na navegação**.
3. Clique na guia **Rede** e inicie a captura do tráfego de rede:
 - No Chrome, selecione **Preservar log**. A gravação deve ser iniciada automaticamente. Um círculo vermelho indica que o tráfego está sendo capturado. Se não for exibido, clique no círculo preto para iniciar
 - No Edge/IE, a gravação deve ser iniciada automaticamente. Se não estiver, clique no botão verde para executar.
4. Tente reproduzir o erro.
5. Depois que você tiver encontrado o erro durante a gravação, interrompa a gravação e salve uma cópia da atividade registrada:
 - No Chrome, clique com o botão direito e clique em **Salvar como HAR com conteúdo**. Isso compacta e exporta os logs como um arquivo .har.
 - No IE/Edge, clique no ícone **Exportar tráfego capturado**. Isso compacta e exporta o log.
6. Navegue até a guia **Console** para verificar se há avisos ou erros. Para salvar o log do console:
 - No Chrome, clique com o botão direito em qualquer lugar no log do console. Selecione **Salvar como**, para exportar e compactar o log.
 - No Edge/IE, clique com o botão direito nos erros e selecione **Copiar tudo**.
7. Fechar as Ferramentas para Desenvolvedores.


## <a name="vcenter-errors"></a>Erros do vCenter

### <a name="error-unhandledexception-internal-error-occured-systemiofilenotfoundexception"></a>Ocorreu um erro UnhandledException Internal: System.IO.FileNotFoundException

É um problema visto nas versões do Coletor inferiores a 1.0.9.5. Se você estiver usando uma versão do Coletor 1.0.9.2 ou anteriores ao GA como 1.0.8.59, terá esse problema. Siga o [link fornecido aqui para obter uma resposta detalhada nos fóruns](https://social.msdn.microsoft.com/Forums/azure/en-US/c1f59456-7ba1-45e7-9d96-bae18112fb52/azure-migrate-connect-to-vcenter-server-error?forum=AzureMigrate).

[Atualize o Coletor para corrigir o problema](https://aka.ms/migrate/col/checkforupdates).

### <a name="error-unabletoconnecttoserver"></a>Erro UnableToConnectToServer

Não é possível conectar o vCenter Server "Servername.com:9443" devido ao erro: Nenhum ponto de extremidade atendendo em https://Servername.com:9443/sdk, que poderia aceitar a mensagem.

Isso acontece quando a máquina do Coletor não consegue resolver o nome do servidor vCenter especificado ou a porta especificada está errada. Por padrão, se a porta não for especificada, o Coletor tentará conectar o número da porta 443.

1. Tente executar o ping Servername.com a partir da máquina do Coletor.
2. Se a etapa 1 falhar, tente conectar o servidor vCenter no endereço IP.
3. Identifique o número correto da porta para conectar o vCenter.
4. Finalmente, verifique se o servidor vCenter está em execução.

## <a name="collector-error-codes-and-recommended-actions"></a>Códigos de erro de coletor e ações recomendadas

|           |                                |                                                                               |                                                                                                       |                                                                                                                                            | 
|-----------|--------------------------------|-------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------| 
| Código do Erro | Nome do erro                      | Mensagem                                                                       | Possíveis causas                                                                                        | Ação recomendada                                                                                                                          | 
| 601       | CollectorExpired               | O coletor expirou.                                                        | O coletor expirou.                                                                                    | Faça o download de uma nova versão do coletor e tente novamente.                                                                                      | 
| 751       | UnableToConnectToServer        | Não foi possível se conectar ao vCenter Server '%Name;' devido ao erro: %ErrorMessage;     | Verifique a mensagem de erro para obter mais detalhes.                                                             | Resolva o problema e tente novamente.                                                                                                           | 
| 752       | InvalidvCenterEndpoint         | O servidor '%Name;' não é um vCenter Server.                                  | Forneça os detalhes do vCenter Server.                                                                       | Tente a operação novamente com os detalhes corretos do vCenter Server.                                                                                   | 
| 753       | InvalidLoginCredentials        | Não foi possível se conectar ao vCenter Server '%Name;' devido ao erro: %ErrorMessage; | A conexão com o vCenter Server falhou devido a credenciais de logon inválidas.                             | Certifique-se de que as credenciais de logon fornecidas estejam corretas.                                                                                    | 
| 754       | NoPerfDataAvaialable           | Os dados de desempenho não estão disponíveis.                                               | Verifique o nível de estatísticas no vCenter Server. Ele deve ser definido como 3 para que os dados de desempenho estejam disponíveis. | Altere o nível de Estatísticas para 3 (para 5 minutos, 30 minutos e a duração de 2 horas) e tente depois de aguardar pelo menos um dia.                   | 
| 756       | NullInstanceUUID               | Encontrada uma máquina com InstanceUUID nulo                                  | O vCenter Server pode ter um objeto inadequado.                                                      | Resolva o problema e tente novamente.                                                                                                           | 
| 757       | VMNotFound                     | A máquina virtual não foi encontrada                                                  | A Máquina virtual pode ser excluída: %VMID;                                                                | Certifique-se de que as máquinas virtuais selecionadas ao investigar o inventário do vCenter existam durante a descoberta                                      | 
| 758       | GetPerfDataTimeout             | A solicitação do VCenter atingiu o tempo limite. Mensagem %Message;                                  | As credenciais do vCenter Server estão incorretas                                                              | Verifique as credenciais do vCenter Server e certifique-se de que esse vCenter Server esteja acessível. Repita a operação. Se o problema persistir, contate o suporte. | 
| 759       | VmwareDllNotFound              | DLL VMWare.Vim não encontrada.                                                     | O PowerCLI não está instalado corretamente.                                                                   | Verifique se o PowerCLI está instalado corretamente. Repita a operação. Se o problema persistir, contate o suporte.                               | 
| 800       | ServiceError                   | O serviço Coletor de Migrações para Azure não está em execução.                               | O serviço Coletor de Migrações para Azure não está em execução.                                                       | Use services.msc para iniciar o serviço e repita a operação.                                                                             | 
| 801       | PowerCLIError                  | Falha na instalação do PowerCLI do VMware.                                          | Falha na instalação do PowerCLI do VMware.                                                                  | Repita a operação. Se o problema persistir, instale-o manualmente e repita a operação.                                                   | 
| 802       | TimeSyncError                  | O horário não está sincronizado com o servidor de horário na Internet.                            | O horário não está sincronizado com o servidor de horário na Internet.                                                    | Certifique-se de que o tempo no computador esteja definido corretamente em relação ao fuso horário do computador e repita a operação.                                 | 
| 702       | OMSInvalidProjectKey           | A chave especificada do projeto é inválida.                                                | A chave especificada do projeto é inválida.                                                                        | Tente a operação novamente com a chave de projeto correta.                                                                                              | 
| 703       | OMSHttpRequestException        | Erro ao enviar a solicitação. Mensagem %Message;                                | Confira a ID e a chave do projeto e verifique se o ponto de extremidade é acessível.                                       | Repita a operação. Se o problema persistir, contate o Suporte da Microsoft.                                                                     | 
| 704       | OMSHttpRequestTimeoutException | A solicitação de HTTP atingiu o tempo limite. Mensagem %Message;                                     | Confira a ID e a chave do projeto e verifique se o ponto de extremidade é acessível.                                       | Repita a operação. Se o problema persistir, contate o Suporte da Microsoft.                                                                     | 
