---
title: Adicionar soluções de gerenciamento de cluster HDInsight ao Log Analytics do Azure | Microsoft Docs
description: Saiba como usar o Azure Log Analytics para criar exibições personalizadas para clusters HDInsight.
services: hdinsight
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 02/21/2018
ms.author: nitinme
ms.openlocfilehash: caa10682f8419cfbc0837e5069357e02ee1e5f64
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/16/2018
---
# <a name="add-hdinsight-cluster-management-solutions-to-log-analytics"></a>Adicionar soluções de gerenciamento de cluster HDInsight para o Log Analytics

O HDInsight fornece soluções de gerenciamento específicas para cluster que podem ser adicionadas para o Azure Log Analytics. As [soluções de gerenciamento](../log-analytics/log-analytics-add-solutions.md) adicionam funcionalidade ao Log Analytics, fornecendo dados adicionais e ferramentas de análise. Essas soluções coletam métricas de desempenho importantes de seus clusters HDInsight e fornecem as ferramentas para pesquisá-las. Essas soluções também fornecem visualizações e painéis para a maioria dos tipos de cluster com suporte no HDInsight. Usando as métricas que coleta com a solução, você pode criar alertas e regras de monitoramentos personalizadas. 

Neste artigo, você aprenderá como adicionar soluções de gerenciamento específicas do cluster a um espaço de trabalho do Log Analytics.

## <a name="prerequisites"></a>pré-requisitos

* É necessário ter configurado um cluster HDInsight para usar o Azure Log Analytics. Para obter instruções, consulte [Use Azure Log Analytics with HDInsight clusters](hdinsight-hadoop-oms-log-analytics-tutorial.md) (Usar o Azure Log Analytics com clusters HDInsight).

## <a name="add-cluster-specific-management-solutions"></a>Adicionar soluções de gerenciamento específicas para cluster

Nesta seção, você adiciona uma solução de gerenciamento de cluster HBase a um espaço de trabalho do Log Analytics existente.

1. Abra um Cluster HDInsight no Portal do Azure, clique em **Monitoramento** e, em seguida, clique em **Abra painel do OMS**.

    ![Abrir o painel do Operations Management Suite](./media/hdinsight-hadoop-oms-log-analytics-management-solutions/hdinsight-log-analytics-open-oms-dashboard.png "Abrir painel do OMS")

1. No painel, clique em **Galeria de Soluções** ou no ícone do **Designer de Exibição** no painel esquerdo.

    ![Adicionar solução de gerenciamento no Log Analytics](./media/hdinsight-hadoop-oms-log-analytics-management-solutions/hdinsight-add-management-solution-oms-portal.png "Adicionar solução de gerenciamento no Operations Management Suite")

2. Na Galeria de Soluções, clique em uma das imagens a seguir:

    - Monitoramento do Hadoop no HDInsight
    - Monitoramento do HBase no HDInsight (Versão Prévia)
    - Monitoramento do Kafka no HDInsight
    - Monitoramento do Storm no HDInsight
    - Monitoramento do Spark no HDInsight

3. Na próxima tela, clique em **Adicionar**.  A captura de tela a seguir mostra o botão Adicionar para o HBase Monitoring.

     ![Adicionar a solução de gerenciamento do HBase](./media/hdinsight-hadoop-oms-log-analytics-management-solutions/add-hbase-management-solution.png "Adicionar a solução de gerenciamento do HBase")

4. Você pode ver um bloco no painel para a solução de gerenciamento do HBase. Se o cluster associado ao Operations Management Suite (como parte do pré-requisito para este artigo) for um cluster HBase, o bloco mostrará o nome do cluster e o número de nós nele.

    ![Solução de gerenciamento do HBase adicionada](./media/hdinsight-hadoop-oms-log-analytics-management-solutions/added-hbase-management-solution.png "Solução de gerenciamento do HBase adicionada")

## <a name="next-steps"></a>Próximas etapas

* [Consultar o Azure Log Analytics ara monitorar clusters HDInsight](hdinsight-hadoop-oms-log-analytics-use-queries.md)

## <a name="see-also"></a>Consulte também

* [Trabalhando com o Log Analytics](https://blogs.msdn.microsoft.com/wei_out_there_with_system_center/2016/07/03/oms-log-analytics-create-tiles-drill-ins-and-dashboards-with-the-view-designer/)
* [Criar regras de alerta no Log Analytics](../log-analytics/log-analytics-alerts-creating.md)
