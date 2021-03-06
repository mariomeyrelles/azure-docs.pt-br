---
title: Criar e implantar um modelo de previsão usando o Pacote do Azure Machine Learning para Previsão.
description: Saiba como criar, treinar, testar e implantar um modelo de previsão usando o Pacote do Azure Machine Learning para Previsão.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: mattcon
author: matthewconners
ms.date: 05/07/2018
ms.openlocfilehash: 71b817c92fe007f36645f28acae7421667dcb13b
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/07/2018
---
# <a name="build-and-deploy-forecasting-models-with-azure-machine-learning"></a>Criar e implantar modelos de previsão com o Azure Machine Learning

Neste artigo, saiba como usar o **AMLPF** (Pacote do Azure Machine Learning para Previsão) para criar e implantar um modelo de previsão rapidamente. O fluxo de trabalho é o seguinte:

1. Carregar e explorar dados
2. Criar recursos
3. Treinar e selecionar o melhor modelo
4. Implantar o modelo e consumir o serviço Web

Consulte a [documentação de referência do pacote](https://aka.ms/aml-packages/forecasting) para ver a lista completa de transformadores e modelos, além da referência detalhada de cada módulo e classe.

## <a name="prerequisites"></a>pré-requisitos

1. Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

1. As contas e os aplicativos abaixo devem ser configurados e instalados:
   - Uma conta de Experimentação do Azure Machine Learning 
   - Conta do Gerenciamento de Modelos do Azure Machine Learning
   - O Azure Machine Learning Workbench instalado

   Se esses três ainda não foram criados ou instalados, siga o artigo [Instalação do Início Rápido e do Azure Machine Learning Workbench](../service/quickstart-installation.md). 

1. O Pacote do Azure Machine Learning para Previsão deve estar instalado. Saiba como [instalar esse pacote aqui](https://aka.ms/aml-packages/forecasting).

## <a name="sample-data-and-jupyter-notebook"></a>Dados de exemplo e Jupyter Notebook

### <a name="sample-workflow"></a>Fluxo de trabalho de exemplo 
O exemplo segue o fluxo de trabalho:
 
1. **Ingestão de dados**: carregue o conjunto de dados e converta-os em TimeSeriesDataFrame. Esse dataframe é uma estrutura de dados de série temporal fornecida pelo Pacote do Azure Machine Learning para Previsão, chamado aqui de **AMLPF**.

2. **Criar recursos**: use vários transformadores de recursos fornecidos pelo AMLPF para criar recursos.

3. **Treinar e selecionar o melhor modelo**: compare o desempenho de vários modelos de série temporal monovariável e modelos de aprendizado de máquina. 

4. **Implantar o modelo**: implante o pipeline treinado como um serviço Web por meio do Azure Machine Learning Workbench para que ele possa ser consumido por outras pessoas.

### <a name="get-the-jupyter-notebook"></a>Obter o Jupyter Notebook

Baixe o notebook para executar os exemplos de código descritos por conta própria.

> [!div class="nextstepaction"]
> [Obter o Jupyter Notebook](https://aka.ms/aml-packages/forecasting/notebooks/financial_forecasting)

### <a name="explore-the-sample-data"></a>Explorar o código de exemplo

Os exemplos de previsão de aprendizado de máquina no código a seguir usam o [conjunto de dados Alimentos mais refinados do Dominick’s, da Universidade de Chicago](https://research.chicagobooth.edu/kilts/marketing-databases/dominicks) para prever as vendas de suco de laranja. Dominick’s era uma cadeia de supermercados na área metropolitana de Chicago.

### <a name="import-any-dependencies-for-this-sample"></a>Importar dependências para este exemplo

Essas dependências devem ser importadas para os exemplos de código a seguir.

```python
import pandas as pd
import numpy as np
import math
import pkg_resources
from datetime import timedelta
import matplotlib
matplotlib.use('agg')
from matplotlib import pyplot as plt

from sklearn.linear_model import Lasso, ElasticNet
from sklearn.ensemble import RandomForestRegressor, GradientBoostingRegressor
from sklearn.neighbors import KNeighborsRegressor

from ftk import TimeSeriesDataFrame, ForecastDataFrame, AzureMLForecastPipeline
from ftk.tsutils import last_n_periods_split

from ftk.transforms import LagLeadOperator, TimeSeriesImputer, TimeIndexFeaturizer, DropColumns
from ftk.transforms.grain_index_featurizer import GrainIndexFeaturizer
from ftk.models import Arima, SeasonalNaive, Naive, RegressionForecaster, ETS
from ftk.models.forecasterunion import ForecasterUnion
from ftk.model_selection import TSGridSearchCV, RollingOriginValidator

from azuremltkbase.deployment import AMLSettings
from ftk.operationalization.forecast_webservice_factory import ForecastWebserviceFactory
from ftk.operationalization import ScoreContext

from ftk.data import get_a_year_of_daily_weather_data
print('imports done')
```

    imports done
    

## <a name="load-data-and-explore"></a>Carregar dados e explorar

Este trecho de código mostra o processo típico de inicialização com um conjunto de dados bruto, nesse caso, os [dados sobre Alimentos Mais Refinados do Dominick’s](https://research.chicagobooth.edu/kilts/marketing-databases/dominicks).  Você também pode usar a função de conveniência [load_dominicks_oj_data](https://docs.microsoft.com/en-us/python/api/ftk.data.dominicks_oj.load_dominicks_oj_data).

```python
# Load the data into a pandas DataFrame
csv_path = pkg_resources.resource_filename('ftk', 'data/dominicks_oj/dominicks_oj.csv')
whole_df = pd.read_csv(csv_path, low_memory = False)
whole_df.head()
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>store</th>
      <th>brand</th>
      <th>week</th>
      <th>logmove</th>
      <th>feat</th>
      <th>preço</th>
      <th>AGE60</th>
      <th>EDUC</th>
      <th>ETHNIC</th>
      <th>INCOME</th>
      <th>HHLARGE</th>
      <th>WORKWOM</th>
      <th>HVAL150</th>
      <th>SSTRDIST</th>
      <th>SSTRVOL</th>
      <th>CPDIST5</th>
      <th>CPWVOL5</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2</td>
      <td>tropicana</td>
      <td>40</td>
      <td>9,02</td>
      <td>0</td>
      <td>3,87</td>
      <td>0,23</td>
      <td>0,25</td>
      <td>0,11</td>
      <td>10,55</td>
      <td>0.10</td>
      <td>0,30</td>
      <td>0,46</td>
      <td>2.11</td>
      <td>1,14</td>
      <td>1,93</td>
      <td>0,38</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>tropicana</td>
      <td>46</td>
      <td>8,72</td>
      <td>0</td>
      <td>3,87</td>
      <td>0,23</td>
      <td>0,25</td>
      <td>0,11</td>
      <td>10,55</td>
      <td>0.10</td>
      <td>0,30</td>
      <td>0,46</td>
      <td>2.11</td>
      <td>1,14</td>
      <td>1,93</td>
      <td>0,38</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>tropicana</td>
      <td>47</td>
      <td>8,25</td>
      <td>0</td>
      <td>3,87</td>
      <td>0,23</td>
      <td>0,25</td>
      <td>0,11</td>
      <td>10,55</td>
      <td>0.10</td>
      <td>0,30</td>
      <td>0,46</td>
      <td>2.11</td>
      <td>1,14</td>
      <td>1,93</td>
      <td>0,38</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2</td>
      <td>tropicana</td>
      <td>48</td>
      <td>8,99</td>
      <td>0</td>
      <td>3,87</td>
      <td>0,23</td>
      <td>0,25</td>
      <td>0,11</td>
      <td>10,55</td>
      <td>0.10</td>
      <td>0,30</td>
      <td>0,46</td>
      <td>2.11</td>
      <td>1,14</td>
      <td>1,93</td>
      <td>0,38</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2</td>
      <td>tropicana</td>
      <td>50</td>
      <td>9.09</td>
      <td>0</td>
      <td>3,87</td>
      <td>0,23</td>
      <td>0,25</td>
      <td>0,11</td>
      <td>10,55</td>
      <td>0.10</td>
      <td>0,30</td>
      <td>0,46</td>
      <td>2.11</td>
      <td>1,14</td>
      <td>1,93</td>
      <td>0,38</td>
    </tr>
  </tbody>
</table>

Os dados consistem em vendas semanais por marca e loja. O logaritmo da quantidade vendida está na coluna _logmove_. Os dados também incluem alguns recursos de dados demográficos do cliente. 

Para o modelo de série temporal, você precisa extrair os seguintes elementos do dataframe: 
+ Um eixo de data/hora 
+ A quantidade de vendas a ser prevista

```python
# The sales are contained in the 'logmove' column. 
# Values are logarithmic, so exponentiate and round them to get quantity sold

def expround(x):
    return math.floor(math.exp(x) + 0.5)
whole_df['Quantity'] = whole_df['logmove'].apply(expround)

# The time axis is in the 'week' column
# This is the week offset from the week of 1989-09-07 through 1989-09-13 inclusive
# Create new datetime columns containing the start and end of each week period

weekZeroStart = pd.to_datetime('1989-09-07 00:00:00')
weekZeroEnd = pd.to_datetime('1989-09-13 23:59:59')
whole_df['WeekFirstDay'] = whole_df['week'].apply(lambda n: weekZeroStart + timedelta(weeks=n))
whole_df['WeekLastDay'] = whole_df['week'].apply(lambda n: weekZeroEnd + timedelta(weeks=n))
whole_df[['store','brand','WeekLastDay','Quantity']].head()
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>store</th>
      <th>brand</th>
      <th>WeekLastDay</th>
      <th>Quantidade</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2</td>
      <td>tropicana</td>
      <td>1990-06-20 23:59:59</td>
      <td>8256</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>tropicana</td>
      <td>1990-08-01 23:59:59</td>
      <td>6144</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>tropicana</td>
      <td>1990-08-08 23:59:59</td>
      <td>3840</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2</td>
      <td>tropicana</td>
      <td>1990-08-15 23:59:59</td>
      <td>8000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2</td>
      <td>tropicana</td>
      <td>1990-08-29 23:59:59</td>
      <td>8896</td>
    </tr>
  </tbody>
</table>


Os dados contêm aproximadamente 250 combinações diferentes de loja e marca em uma estrutura de dados. Cada combinação define sua própria série temporal de vendas. 

Você pode usar a classe [TimeSeriesDataFrame](https://docs.microsoft.com/python/api/ftk.dataframets.timeseriesdataframe) para modelar convenientemente várias séries em uma estrutura de dados simples usando a _granulação_. O detalhamento é especificado pelas colunas `store` e `brand`.

```python
nseries = whole_df.groupby(['store', 'brand']).ngroups
print('{} time series in the data frame.'.format(nseries))
```

    249 time series in the data frame.
    


A diferença entre _granulação_ e _agrupamento_ é que a granulação é sempre fisicamente relevante no mundo real, mas o grupo não tem de ser. As funções do pacote internas usam agrupamento para criar um único modelo de várias séries temporais se o usuário acha que esse agrupamento ajuda a melhorar o desempenho do modelo. Por padrão, grupo está definido para ser igual ao detalhamento, e um único modelo é criado para cada intervalo. 


```python
# Create a TimeSeriesDataFrame
# Use end of period as the time index
# Store and brand combinations label the grain 
# i.e. there is one time series for each unique pair of store and grain
whole_tsdf = TimeSeriesDataFrame(whole_df, 
                                 grain_colnames=['store', 'brand'],
                                 time_colname='WeekLastDay', 
                                 ts_value_colname='Quantity',
                                 group_colnames='store')

whole_tsdf[['Quantity']].head()
```


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th></th>
      <th>Quantidade</th>
    </tr>
    <tr>
      <th>WeekLastDay</th>
      <th>store</th>
      <th>brand</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1990-06-20 23:59:59</th>
      <th>2</th>
      <th>tropicana</th>
      <td>8256</td>
    </tr>
    <tr>
      <th>1990-08-01 23:59:59</th>
      <th>2</th>
      <th>tropicana</th>
      <td>6144</td>
    </tr>
    <tr>
      <th>1990-08-08 23:59:59</th>
      <th>2</th>
      <th>tropicana</th>
      <td>3840</td>
    </tr>
    <tr>
      <th>1990-08-15 23:59:59</th>
      <th>2</th>
      <th>tropicana</th>
      <td>8000</td>
    </tr>
    <tr>
      <th>1990-08-29 23:59:59</th>
      <th>2</th>
      <th>tropicana</th>
      <td>8896</td>
    </tr>
  </tbody>
</table>


Na representação TimeSeriesDataFrame, o eixo de tempo e a granulação agora fazem parte do índice de quadro de dados e permitem o acesso fácil à funcionalidade de divisão de datetime.


```python
# sort so we can slice
whole_tsdf.sort_index(inplace=True)

# Get sales of dominick's brand orange juice from store 2 during summer 1990
whole_tsdf.loc[pd.IndexSlice['1990-06':'1990-09', 2, 'dominicks'], ['Quantity']]
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th></th>
      <th>Quantidade</th>
    </tr>
    <tr>
      <th>WeekLastDay</th>
      <th>store</th>
      <th>brand</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1990-06-20 23:59:59</th>
      <th>2</th>
      <th>dominicks</th>
      <td>10560</td>
    </tr>
    <tr>
      <th>1990-08-01 23:59:59</th>
      <th>2</th>
      <th>dominicks</th>
      <td>8000</td>
    </tr>
    <tr>
      <th>1990-08-08 23:59:59</th>
      <th>2</th>
      <th>dominicks</th>
      <td>6848</td>
    </tr>
    <tr>
      <th>1990-08-15 23:59:59</th>
      <th>2</th>
      <th>dominicks</th>
      <td>2880</td>
    </tr>
    <tr>
      <th>1990-08-29 23:59:59</th>
      <th>2</th>
      <th>dominicks</th>
      <td>1600</td>
    </tr>
    <tr>
      <th>1990-09-05 23:59:59</th>
      <th>2</th>
      <th>dominicks</th>
      <td>25344</td>
    </tr>
    <tr>
      <th>1990-09-12 23:59:59</th>
      <th>2</th>
      <th>dominicks</th>
      <td>10752</td>
    </tr>
    <tr>
      <th>1990-09-19 23:59:59</th>
      <th>2</th>
      <th>dominicks</th>
      <td>6656</td>
    </tr>
    <tr>
      <th>1990-09-26 23:59:59</th>
      <th>2</th>
      <th>dominicks</th>
      <td>6592</td>
    </tr>
  </tbody>
</table>



A função [TimeSeriesDataFrame.ts_report](https://docs.microsoft.com/en-us/python/api/ftk.dataframets.timeseriesdataframe#ts-report) gera um relatório abrangente do quadro de dados da série temporal. O relatório inclui tanto uma descrição geral de dados quanto estatísticas específicas para dados de série temporal. 


```python
%matplotlib inline
whole_tsdf.ts_report()
```

    --------------------------------  Data Overview  ---------------------------------
    <class 'ftk.dataframets.TimeSeriesDataFrame'>
    MultiIndex: 28947 entries, (1990-06-20 23:59:59, 2, dominicks) to (1992-10-07 23:59:59, 137, tropicana)
    Data columns (total 17 columns):
    week            28947 non-null int64
    logmove         28947 non-null float64
    feat            28947 non-null int64
    price           28947 non-null float64
    AGE60           28947 non-null float64
    EDUC            28947 non-null float64
    ETHNIC          28947 non-null float64
    INCOME          28947 non-null float64
    HHLARGE         28947 non-null float64
    WORKWOM         28947 non-null float64
    HVAL150         28947 non-null float64
    SSTRDIST        28947 non-null float64
    SSTRVOL         28947 non-null float64
    CPDIST5         28947 non-null float64
    CPWVOL5         28947 non-null float64
    Quantity        28947 non-null int64
    WeekFirstDay    28947 non-null datetime64[ns]
    dtypes: datetime64[ns](1), float64(13), int64(3)
    memory usage: 3.8+ MB
    --------------------------  Numerical Variable Summary  --------------------------
              week  logmove     feat    price    AGE60     EDUC   ETHNIC   INCOME  \
    count 28947.00 28947.00 28947.00 28947.00 28947.00 28947.00 28947.00 28947.00   
    mean    100.46     9.17     0.24     2.28     0.17     0.23     0.16    10.62   
    std      34.69     1.02     0.43     0.65     0.06     0.11     0.19     0.28   
    min      40.00     4.16     0.00     0.52     0.06     0.05     0.02     9.87   
    25%      70.00     8.49     0.00     1.79     0.12     0.15     0.04    10.46   
    50%     101.00     9.03     0.00     2.17     0.17     0.23     0.07    10.64   
    75%     130.00     9.76     0.00     2.73     0.21     0.28     0.19    10.80   
    max     160.00    13.48     1.00     3.87     0.31     0.53     1.00    11.24   
    
           HHLARGE  WORKWOM  HVAL150  SSTRDIST  SSTRVOL  CPDIST5  CPWVOL5  \
    count 28947.00 28947.00 28947.00  28947.00 28947.00 28947.00 28947.00   
    mean      0.12     0.36     0.34      5.10     1.21     2.12     0.44   
    std       0.03     0.05     0.24      3.47     0.53     0.73     0.22   
    min       0.01     0.24     0.00      0.13     0.40     0.77     0.09   
    25%       0.10     0.31     0.12      2.77     0.73     1.63     0.27   
    50%       0.11     0.36     0.35      4.65     1.12     1.96     0.38   
    75%       0.14     0.40     0.53      6.65     1.54     2.53     0.56   
    max       0.22     0.47     0.92     17.86     2.57     4.11     1.14   
    
           Quantity  
    count  28947.00  
    mean   17312.21  
    std    27477.66  
    min       64.00  
    25%     4864.00  
    50%     8384.00  
    75%    17408.00  
    max   716416.00  
    ------------------------  Non-Numerical Variable Summary  -----------------------
                   WeekFirstDay
    count                 28947
    unique                  121
    top     1992-03-12 00:00:00
    freq                    249
    first   1990-06-14 00:00:00
    last    1992-10-01 00:00:00
    ------------------------------  Time Series Summary  -----------------------------
    Number of time series                 249
    Minimum time                    1990-06-20 23:59:59
    Maximum time                    1992-10-07 23:59:59
    
    Inferred frequencies
    Number of ['store', 'brand']s with frequency W-WED     249
    Use get_frequency_dict() method to explore ['store', 'brand']s with unusual frequency and clean data
    
    Detected seasonalities
    Number of ['store', 'brand']s with seasonality 1         190
    Number of ['store', 'brand']s with seasonality 15        15
    Number of ['store', 'brand']s with seasonality 14        11
    Number of ['store', 'brand']s with seasonality 7         9
    Number of ['store', 'brand']s with seasonality 6         8
    Number of ['store', 'brand']s with seasonality 8         5
    Number of ['store', 'brand']s with seasonality 2         4
    Number of ['store', 'brand']s with seasonality 23        2
    Number of ['store', 'brand']s with seasonality 3         1
    Number of ['store', 'brand']s with seasonality 11        1
    Number of ['store', 'brand']s with seasonality 12        1
    Number of ['store', 'brand']s with seasonality 13        1
    Number of ['store', 'brand']s with seasonality 47        1
    Use get_seasonality_dict() method to explore ['store', 'brand']s with unusual seasonality and clean data
    -----------------------------  Value Column Summary  -----------------------------
    Value column                        Quantity
    Percentage of missing values        0.00
    Percentage of zero values           0.00
    Mean coefficient of variation       31688.52
    Median coefficient of variation     24000.20
    Minimum coefficient of variation    ['store', 'brand'] (48, 'tropicana'): 4475.53
    Maximum coefficient of variation    ['store', 'brand'] (111, 'dominicks'): 193333.55
    ------------------------------  Correlation Matrix  ------------------------------
        week  logmove  feat  price  AGE60  EDUC  ETHNIC  INCOME  HHLARGE  WORKWOM  \
    0   1.00     0.10  0.04  -0.21  -0.01  0.01    0.00    0.00     0.01    -0.00   
    1   0.10     1.00  0.54  -0.43   0.09  0.00    0.06   -0.04    -0.06    -0.08   
    2   0.04     0.54  1.00  -0.29  -0.00  0.00    0.00   -0.00    -0.00     0.00   
    3  -0.21    -0.43 -0.29   1.00   0.04  0.02    0.04   -0.03    -0.04    -0.02   
    4  -0.01     0.09 -0.00   0.04   1.00 -0.31   -0.09   -0.15    -0.32    -0.63   
    5   0.01     0.00  0.00   0.02  -0.31  1.00   -0.34    0.66    -0.39     0.56   
    6   0.00     0.06  0.00   0.04  -0.09 -0.34    1.00   -0.72     0.25    -0.29   
    7   0.00    -0.04 -0.00  -0.03  -0.15  0.66   -0.72    1.00    -0.08     0.40   
    8   0.01    -0.06 -0.00  -0.04  -0.32 -0.39    0.25   -0.08     1.00    -0.28   
    9  -0.00    -0.08  0.00  -0.02  -0.63  0.56   -0.29    0.40    -0.28     1.00   
    10  0.01     0.02  0.00   0.04  -0.11  0.89   -0.42    0.64    -0.48     0.45   
    11  0.01    -0.00  0.00   0.08   0.07 -0.12    0.54   -0.41     0.06    -0.19   
    12 -0.01    -0.09 -0.00   0.03  -0.05 -0.13    0.23   -0.26     0.04    -0.06   
    13 -0.01     0.02 -0.00  -0.06   0.09 -0.20   -0.22    0.21     0.19    -0.13   
    14 -0.00    -0.12 -0.00  -0.01  -0.09  0.28   -0.38    0.36    -0.20     0.37   
    15  0.03     0.76  0.47  -0.36   0.08 -0.04    0.11   -0.08    -0.00    -0.10   
    
        HVAL150  SSTRDIST  SSTRVOL  CPDIST5  CPWVOL5  Quantity  
    0      0.01      0.01    -0.01    -0.01    -0.00      0.03  
    1      0.02     -0.00    -0.09     0.02    -0.12      0.76  
    2      0.00      0.00    -0.00    -0.00    -0.00      0.47  
    3      0.04      0.08     0.03    -0.06    -0.01     -0.36  
    4     -0.11      0.07    -0.05     0.09    -0.09      0.08  
    5      0.89     -0.12    -0.13    -0.20     0.28     -0.04  
    6     -0.42      0.54     0.23    -0.22    -0.38      0.11  
    7      0.64     -0.41    -0.26     0.21     0.36     -0.08  
    8     -0.48      0.06     0.04     0.19    -0.20     -0.00  
    9      0.45     -0.19    -0.06    -0.13     0.37     -0.10  
    10     1.00     -0.17    -0.24    -0.22     0.27     -0.04  
    11    -0.17      1.00     0.17    -0.11    -0.40      0.06  
    12    -0.24      0.17     1.00    -0.05     0.36     -0.02  
    13    -0.22     -0.11    -0.05     1.00     0.02     -0.00  
    14     0.27     -0.40     0.36     0.02     1.00     -0.11  
    15    -0.04      0.06    -0.02    -0.00    -0.11      1.00  
    You may call TimeSeriesDataFrame.plot_scatter_matrix() to get a correlation matrix plot. However, this
    is not recommended if your data is big.
    


![png](./media/how-to-build-deploy-forecast-models/output_15_1.png)



![png](./media/how-to-build-deploy-forecast-models/output_15_2.png)



![png](./media/how-to-build-deploy-forecast-models/output_15_3.png)



![png](./media/how-to-build-deploy-forecast-models/output_15_4.png)



![png](./media/how-to-build-deploy-forecast-models/output_15_5.png)



![png](./media/how-to-build-deploy-forecast-models/output_15_6.png)


## <a name="integrate-with-external-data"></a>Integrar a dados externos

Às vezes é útil integrar dados externos como recursos adicionais para a previsão. Nesse exemplo de código, você pode unir TimeSeriesDataFrame a dados externos relacionados ao clima.


```python
# Load weather data
weather_1990 = get_a_year_of_daily_weather_data(year=1990)
weather_1991 = get_a_year_of_daily_weather_data(year=1991)
weather_1992 = get_a_year_of_daily_weather_data(year=1992)

# Preprocess weather data
weather_all = pd.concat([weather_1990, weather_1991, weather_1992])
weather_all.reset_index(inplace=True)

# Only use a subset of columns
weather_all = weather_all[['TEMP', 'DEWP', 'WDSP', 'PRCP']]

# Compute the WeekLastDay column, in order to merge with sales data
weather_all['WeekLastDay'] = pd.Series(
    weather_all.time_index - weekZeroStart, 
    index=weather_all.time_index).apply(lambda n: weekZeroEnd + timedelta(weeks=math.floor(n.days/7)))

# Resample daily weather data to weekly data
weather_all = weather_all.groupby('WeekLastDay').mean()

# Set WeekLastDay as new time index
weather_all = TimeSeriesDataFrame(weather_all, time_colname='WeekLastDay')

# Merge weather data with sales data
whole_tsdf = whole_tsdf.merge(weather_all, how='left', on='WeekLastDay')
whole_tsdf.head()
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th></th>
      <th>week</th>
      <th>logmove</th>
      <th>feat</th>
      <th>preço</th>
      <th>AGE60</th>
      <th>EDUC</th>
      <th>ETHNIC</th>
      <th>INCOME</th>
      <th>HHLARGE</th>
      <th>WORKWOM</th>
      <th>...</th>
      <th>SSTRDIST</th>
      <th>SSTRVOL</th>
      <th>CPDIST5</th>
      <th>CPWVOL5</th>
      <th>Quantidade</th>
      <th>WeekFirstDay</th>
      <th>TEMP</th>
      <th>DEWP</th>
      <th>WDSP</th>
      <th>PRCP</th>
    </tr>
    <tr>
      <th>WeekLastDay</th>
      <th>store</th>
      <th>brand</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="5" valign="top">1990-06-20 23:59:59</th>
      <th rowspan="3" valign="top">2</th>
      <th>dominicks</th>
      <td>40</td>
      <td>9,26</td>
      <td>1</td>
      <td>1,59</td>
      <td>0,23</td>
      <td>0,25</td>
      <td>0,11</td>
      <td>10,55</td>
      <td>0.10</td>
      <td>0,30</td>
      <td>...</td>
      <td>2.11</td>
      <td>1,14</td>
      <td>1,93</td>
      <td>0,38</td>
      <td>10560</td>
      <td>1990-06-14</td>
      <td>72,00</td>
      <td>61,87</td>
      <td>9,74</td>
      <td>0,19</td>
    </tr>
    <tr>
      <th>minute.maid</th>
      <td>40</td>
      <td>8,41</td>
      <td>0</td>
      <td>3,17</td>
      <td>0,23</td>
      <td>0,25</td>
      <td>0,11</td>
      <td>10,55</td>
      <td>0.10</td>
      <td>0,30</td>
      <td>...</td>
      <td>2.11</td>
      <td>1,14</td>
      <td>1,93</td>
      <td>0,38</td>
      <td>4480</td>
      <td>1990-06-14</td>
      <td>72,00</td>
      <td>61,87</td>
      <td>9,74</td>
      <td>0,19</td>
    </tr>
    <tr>
      <th>tropicana</th>
      <td>40</td>
      <td>9,02</td>
      <td>0</td>
      <td>3,87</td>
      <td>0,23</td>
      <td>0,25</td>
      <td>0,11</td>
      <td>10,55</td>
      <td>0.10</td>
      <td>0,30</td>
      <td>...</td>
      <td>2.11</td>
      <td>1,14</td>
      <td>1,93</td>
      <td>0,38</td>
      <td>8256</td>
      <td>1990-06-14</td>
      <td>72,00</td>
      <td>61,87</td>
      <td>9,74</td>
      <td>0,19</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">5</th>
      <th>dominicks</th>
      <td>40</td>
      <td>7,49</td>
      <td>1</td>
      <td>1,59</td>
      <td>0,12</td>
      <td>0,32</td>
      <td>0,05</td>
      <td>10,92</td>
      <td>0.10</td>
      <td>0,41</td>
      <td>...</td>
      <td>3,80</td>
      <td>0,68</td>
      <td>1,60</td>
      <td>0,74</td>
      <td>1792</td>
      <td>1990-06-14</td>
      <td>72,00</td>
      <td>61,87</td>
      <td>9,74</td>
      <td>0,19</td>
    </tr>
    <tr>
      <th>minute.maid</th>
      <td>40</td>
      <td>8,35</td>
      <td>0</td>
      <td>2,99</td>
      <td>0,12</td>
      <td>0,32</td>
      <td>0,05</td>
      <td>10,92</td>
      <td>0.10</td>
      <td>0,41</td>
      <td>...</td>
      <td>3,80</td>
      <td>0,68</td>
      <td>1,60</td>
      <td>0,74</td>
      <td>4224</td>
      <td>1990-06-14</td>
      <td>72,00</td>
      <td>61,87</td>
      <td>9,74</td>
      <td>0,19</td>
    </tr>
  </tbody>
</table>



## <a name="preprocess-data-and-impute-missing-values"></a>Pré-processar dados e imputar valores ausentes

Comece dividindo os dados em um conjunto de treinamento e um conjunto de teste com a função de utilitário [ftk.tsutils.last_n_periods_split](https://docs.microsoft.com/python/api/ftk.tsutils). O conjunto de teste resultante contém as últimas 40 observações de cada série temporal. 


```python
train_tsdf, test_tsdf = last_n_periods_split(whole_tsdf, 40)
```

Os modelos de série temporal básicos exigem a séries temporais contíguas. Verifique se a série está regular, o que significa que ela tem um índice de tempo de amostra em intervalos regulares, usando a função [check_regularity_by_grain](https://docs.microsoft.compython/api/ftk.dataframets.timeseriesdataframe).


```python
ts_regularity = train_tsdf.check_regularity_by_grain()
print(ts_regularity[ts_regularity['regular'] == False])
```

                                              problems  regular
    store brand                                                
    2     dominicks    [Irregular datetime gaps exist]    False
          minute.maid  [Irregular datetime gaps exist]    False
          tropicana    [Irregular datetime gaps exist]    False
    5     dominicks    [Irregular datetime gaps exist]    False
          minute.maid  [Irregular datetime gaps exist]    False
          tropicana    [Irregular datetime gaps exist]    False
    8     dominicks    [Irregular datetime gaps exist]    False
          minute.maid  [Irregular datetime gaps exist]    False
          tropicana    [Irregular datetime gaps exist]    False
    9     dominicks    [Irregular datetime gaps exist]    False
          minute.maid  [Irregular datetime gaps exist]    False
          tropicana    [Irregular datetime gaps exist]    False
    12    dominicks    [Irregular datetime gaps exist]    False
          minute.maid  [Irregular datetime gaps exist]    False
          tropicana    [Irregular datetime gaps exist]    False
    14    dominicks    [Irregular datetime gaps exist]    False
          minute.maid  [Irregular datetime gaps exist]    False
          tropicana    [Irregular datetime gaps exist]    False
    18    dominicks    [Irregular datetime gaps exist]    False
          minute.maid  [Irregular datetime gaps exist]    False
          tropicana    [Irregular datetime gaps exist]    False
    21    dominicks    [Irregular datetime gaps exist]    False
          minute.maid  [Irregular datetime gaps exist]    False
          tropicana    [Irregular datetime gaps exist]    False
    28    dominicks    [Irregular datetime gaps exist]    False
          minute.maid  [Irregular datetime gaps exist]    False
          tropicana    [Irregular datetime gaps exist]    False
    33    dominicks    [Irregular datetime gaps exist]    False
          minute.maid  [Irregular datetime gaps exist]    False
          tropicana    [Irregular datetime gaps exist]    False
    ...                                            ...      ...
    119   dominicks    [Irregular datetime gaps exist]    False
          minute.maid  [Irregular datetime gaps exist]    False
          tropicana    [Irregular datetime gaps exist]    False
    121   dominicks    [Irregular datetime gaps exist]    False
          minute.maid  [Irregular datetime gaps exist]    False
          tropicana    [Irregular datetime gaps exist]    False
    123   dominicks    [Irregular datetime gaps exist]    False
          minute.maid  [Irregular datetime gaps exist]    False
          tropicana    [Irregular datetime gaps exist]    False
    126   dominicks    [Irregular datetime gaps exist]    False
          minute.maid  [Irregular datetime gaps exist]    False
          tropicana    [Irregular datetime gaps exist]    False
    128   dominicks    [Irregular datetime gaps exist]    False
          minute.maid  [Irregular datetime gaps exist]    False
          tropicana    [Irregular datetime gaps exist]    False
    129   dominicks    [Irregular datetime gaps exist]    False
          minute.maid  [Irregular datetime gaps exist]    False
          tropicana    [Irregular datetime gaps exist]    False
    130   dominicks    [Irregular datetime gaps exist]    False
          minute.maid  [Irregular datetime gaps exist]    False
          tropicana    [Irregular datetime gaps exist]    False
    131   dominicks    [Irregular datetime gaps exist]    False
          minute.maid  [Irregular datetime gaps exist]    False
          tropicana    [Irregular datetime gaps exist]    False
    134   dominicks    [Irregular datetime gaps exist]    False
          minute.maid  [Irregular datetime gaps exist]    False
          tropicana    [Irregular datetime gaps exist]    False
    137   dominicks    [Irregular datetime gaps exist]    False
          minute.maid  [Irregular datetime gaps exist]    False
          tropicana    [Irregular datetime gaps exist]    False
    
    [213 rows x 2 columns]
    

Você pode ver que a maior parte da série (213 de 249) é irregular. Uma [transformação de imputação](https://docs.microsoft.com/python/api/ftk.transforms.tsimputer.timeseriesimputer) é necessária para preencher os valores de quantidade de vendas ausentes. Embora existam muitas opções de imputação, o código de exemplo a seguir usa uma interpolação linear.


```python
# Use a TimeSeriesImputer to linearly interpolate missing values
imputer = TimeSeriesImputer(input_column='Quantity', 
                            option='interpolate',
                            method='linear',
                            freq='W-WED')

train_imputed_tsdf = imputer.transform(train_tsdf)
```

Depois que o código de imputação é executado, a série tem toda uma frequência regular:


```python
ts_regularity_imputed = train_imputed_tsdf.check_regularity_by_grain()
print(ts_regularity_imputed[ts_regularity_imputed['regular'] == False])
```

    Empty DataFrame
    Columns: [problems, regular]
    Index: []
    

## <a name="univariate-time-series-models"></a>Modelos de série temporal monovariável

Agora que você limpou os dados, poderá começar a modelagem.  Comece criando três modelos de monovariável: o modelo "naive", o modelo "seasonal naive" e um modelo "ARIMA".
* O algoritmo de previsão Naive usa o valor de variável de destino real do último período como o valor previsto do período atual.

* O algoritmo Seasonal Naive usa o valor da variável de destino real do mesmo ponto de tempo da estação anterior como o valor previsto do ponto de tempo atual. Alguns exemplos incluem o uso de um valor real do mesmo mês do ano passado para prever os meses do ano atual; o uso da mesma hora de ontem para prever as horas de hoje. 

* O algoritmo de ETS (atenuação exponencial) gera previsões calculando as médias ponderadas de observações anteriores, com os pesos reduzidos exponencialmente com a idade das observações. 

* O algoritmo ARIMA (média de movimentação integrada de regressão automática) captura a correlação automática em dados de série temporal. Para saber mais sobre ARIMA, confira [este link](https://en.wikipedia.org/wiki/Autoregressive_integrated_moving_average)

Comece definindo determinados parâmetros de modelo com base na exploração dos dados. 


```python
oj_series_freq = 'W-WED'
oj_series_seasonality = 52
```

### <a name="initialize-models"></a>Inicializar modelos


```python
# Initialize naive model.
naive_model = Naive(freq=oj_series_freq)

# Initialize seasonal naive model. 
seasonal_naive_model = SeasonalNaive(freq=oj_series_freq, 
                                     seasonality=oj_series_seasonality)

# Initialize ETS model.
ets_model = ETS(freq=oj_series_freq, seasonality=oj_series_seasonality)

# Initialize ARIMA(p,d,q) model.
arima_order = [2, 1, 0]
arima_model = Arima(oj_series_freq, arima_order)
```

### <a name="combine-multiple-models"></a>Combinar vários modelos

O avaliador [ForecasterUnion](https://docs.microsoft.com/python/api/ftk.models.forecasterunion.forecasterunion) permite combinar vários avaliadores e fazer ajustes/previsão com eles usando uma linha de código.


```python
forecaster_union = ForecasterUnion(
    forecaster_list=[('naive', naive_model), ('seasonal_naive', seasonal_naive_model), 
                     ('ets', ets_model)]) 
```

### <a name="fit-and-predict"></a>Ajustar e prever
Os avaliadores no AMLPF seguem a mesma API que os avaliadores do scikit-learn: um método de ajuste para treinamento de modelos e um método de previsão para gerar previsões. 

**Treinar modelos**  
Como esses modelos são todos modelos de monovariável, um modelo se ajusta a cada intervalo de dados. Usando o AMLPF, todos os 249 modelos podem ser ajustados com apenas uma chamada de função.


```python
forecaster_union_fitted = forecaster_union.fit(train_imputed_tsdf)
arima_model_fitted = arima_model.fit(train_imputed_tsdf)
```

**Prever vendas em dados de teste**  
Como o método de ajuste, você pode criar previsões para todas as 249 séries no conjunto de dados de teste com uma chamada para a função `predict`. 


```python
forecaster_union_prediction = forecaster_union_fitted.predict(test_tsdf, retain_feature_column=True)
arima_prediction= arima_model_fitted.predict(test_tsdf)
```

**Avaliar desempenho do modelo**

Agora você pode calcular os erros de previsão no conjunto de teste. Você pode usar o MAPE (erro de porcentagem absoluta média) aqui. MAPE é o erro de média absoluta em relação os valores de vendas reais. A função ```calc_error``` fornece algumas funções internas para métricas de erro mais usadas. Você também pode definir sua função de erro personalizada e passá-la para o argumento ```err_fun```.


```python
forecaster_union_error = forecaster_union_prediction.calc_error(err_name='MAPE', by='ModelName')
arima_error = pd.DataFrame({'ModelName': 'arima','MAPE': arima_prediction.calc_error(err_name='MAPE')}, 
                           index=[len(forecaster_union_error)])

all_model_errors = pd.concat([forecaster_union_error, arima_error])
print(all_model_errors)
```

        MAPE       ModelName
    0 187.89             ets
    1 103.57           naive
    2 180.54  seasonal_naive
    3 126.57           arima
    

Ele é útil para ver a distribuição desses erros pelas 249 séries temporais nos dados. Para ver a distribuição de erros, use o argumento `by` em `calc_error` para calcular os erros de cada série. Em seguida, crie um gráfico de caixa para visualizá-los.


```python
# Compute MAPE by grain for each model
forecaster_union_error_bygrain = forecaster_union_prediction.calc_error(err_name='MAPE',
                                                                        by=['ModelName'] + test_tsdf.grain_colnames)
arima_error_bygrain = arima_prediction.calc_error(err_name='MAPE', 
                                                  by=test_tsdf.grain_colnames)
arima_error_bygrain['ModelName'] = 'arima'

error_bygrain_univariate = pd.concat([forecaster_union_error_bygrain, arima_error_bygrain])

# Display a boxplot to visualize the forecast error distributions
error_bygrain_univariate[['ModelName', 'MAPE']].groupby('ModelName').boxplot(subplots=False, figsize=[10, 8])
```




    <matplotlib.axes._subplots.AxesSubplot at 0x15b5d60d4a8>




![png](./media/how-to-build-deploy-forecast-models/output_41_1.png)


Em geral, o modelo Naive parece ser o melhor para fazer previsões, apesar de algumas exceções que são menos precisas. 

## <a name="build-machine-learning-models"></a>Compilar modelos de aprendizado de máquina
Além dos modelos de monovariável tradicionais, o Pacote do Azure Machine Learning para Previsão também permite a você criar modelos de aprendizado de máquina. S

No caso desse modelo, comece criando recursos.

### <a name="feature-engineering"></a>Engenharia de recursos
**Transformadores**   
O pacote fornece muitos transformadores para pré-processamento e transformação de recursos de dados de série temporal. Os exemplos a seguir demonstram algumas das funcionalidades de pré-processamento e transformação de recursos.


```python
# DropColumns: Drop columns that should not be included for modeling. `logmove` is the log of the number of 
# units sold, so providing this number would be cheating. `WeekFirstDay` would be 
# redundant since we already have a feature for the last day of the week.
columns_to_drop = ['logmove', 'WeekFirstDay', 'week']
column_dropper = DropColumns(columns_to_drop)

# TimeSeriesImputer: Fill missing values in the features
# First, we need to create a dictionary with key as column names and value as values used to fill missing 
# values for that column. We are going to use the mean to fill missing values for each column.
columns_with_missing_values = train_imputed_tsdf.columns[pd.DataFrame(train_imputed_tsdf).isnull().any()].tolist()
columns_with_missing_values = [c for c in columns_with_missing_values if c not in columns_to_drop]
missing_value_imputation_dictionary = {}
for c in columns_with_missing_values:
    missing_value_imputation_dictionary[c] = train_imputed_tsdf[c].mean()
fillna_imputer = TimeSeriesImputer(option='fillna', 
                                   input_column=columns_with_missing_values,
                                   value=missing_value_imputation_dictionary)

# TimeIndexFeaturizer: extract temporal features from timestamps
time_index_featurizer = TimeIndexFeaturizer(correlation_cutoff=0.1, overwrite_columns=True)

# LagOperator: create features from the past values of the series
lag_operator = LagLeadOperator({'price': [1, 2]}, dropna=True)

# GrainIndexFeaturizer: create indicator variables for stores and brands
grain_featurizer = GrainIndexFeaturizer(overwrite_columns=True, ts_frequency=oj_series_freq)
```

**Pipelines**   
Os objetos de pipeline facilitam o salvamento de um conjunto de etapas para que elas possam ser usadas repetidamente em objetos diferentes. Além disso, os objetos do pipeline podem ser apurados a fim de ficarem mais portáteis para implantação em outros computadores. É possível encadear todos os transformadores criados até agora usando um pipeline. 


```python
pipeline_ml = AzureMLForecastPipeline([('drop_columns', column_dropper), 
                                       ('fillna_imputer', fillna_imputer),
                                       ('time_index_featurizer', time_index_featurizer),
                                       ('lag_operator', lag_operator),
                                       ('grain_featurizer', grain_featurizer)
                                      ])

train_feature_tsdf = pipeline_ml.fit_transform(train_imputed_tsdf)
test_feature_tsdf = pipeline_ml.transform(test_tsdf)

# Let's get a look at our new feature set
print(train_feature_tsdf.head())
```

    F1 2018-04-26 17:02:33,633 INFO azureml.timeseries - pipeline fit_transform started. 
    F1 2018-04-26 17:03:51,556 INFO azureml.timeseries - pipeline fit_transform finished. Time elapsed 0:01:17.920637
    F1 2018-04-26 17:03:51,602 INFO azureml.timeseries - pipeline fit_transform started. 
    F1 2018-04-26 17:04:40,372 INFO azureml.timeseries - pipeline fit_transform finished. Time elapsed 0:00:48.770162
                                                               AGE60  CPDIST5  \
    WeekLastDay         store brand       origin                                
    1990-07-04 23:59:59 2     dominicks   1990-06-27 23:59:59   0.17     2.12   
                              minute.maid 1990-06-27 23:59:59   0.17     2.12   
                              tropicana   1990-06-27 23:59:59   0.17     2.12   
    1990-07-11 23:59:59 2     dominicks   1990-07-04 23:59:59   0.17     2.12   
                              minute.maid 1990-07-04 23:59:59   0.17     2.12   
    
                                                               CPWVOL5  DEWP  \
    WeekLastDay         store brand       origin                               
    1990-07-04 23:59:59 2     dominicks   1990-06-27 23:59:59     0.44 43.23   
                              minute.maid 1990-06-27 23:59:59     0.44 43.23   
                              tropicana   1990-06-27 23:59:59     0.44 43.23   
    1990-07-11 23:59:59 2     dominicks   1990-07-04 23:59:59     0.44 43.23   
                              minute.maid 1990-07-04 23:59:59     0.44 43.23   
    
                                                               EDUC  ETHNIC  \
    WeekLastDay         store brand       origin                              
    1990-07-04 23:59:59 2     dominicks   1990-06-27 23:59:59  0.22    0.16   
                              minute.maid 1990-06-27 23:59:59  0.22    0.16   
                              tropicana   1990-06-27 23:59:59  0.22    0.16   
    1990-07-11 23:59:59 2     dominicks   1990-07-04 23:59:59  0.22    0.16   
                              minute.maid 1990-07-04 23:59:59  0.22    0.16   
    
                                                               HHLARGE  HVAL150  \
    WeekLastDay         store brand       origin                                  
    1990-07-04 23:59:59 2     dominicks   1990-06-27 23:59:59     0.12     0.34   
                              minute.maid 1990-06-27 23:59:59     0.12     0.34   
                              tropicana   1990-06-27 23:59:59     0.12     0.34   
    1990-07-11 23:59:59 2     dominicks   1990-07-04 23:59:59     0.12     0.34   
                              minute.maid 1990-07-04 23:59:59     0.12     0.34   
    
                                                               INCOME  PRCP  \
    WeekLastDay         store brand       origin                              
    1990-07-04 23:59:59 2     dominicks   1990-06-27 23:59:59   10.62  0.11   
                              minute.maid 1990-06-27 23:59:59   10.62  0.11   
                              tropicana   1990-06-27 23:59:59   10.62  0.11   
    1990-07-11 23:59:59 2     dominicks   1990-07-04 23:59:59   10.62  0.11   
                              minute.maid 1990-07-04 23:59:59   10.62  0.11   
    
                                                                    ...        \
    WeekLastDay         store brand       origin                    ...         
    1990-07-04 23:59:59 2     dominicks   1990-06-27 23:59:59       ...         
                              minute.maid 1990-06-27 23:59:59       ...         
                              tropicana   1990-06-27 23:59:59       ...         
    1990-07-11 23:59:59 2     dominicks   1990-07-04 23:59:59       ...         
                              minute.maid 1990-07-04 23:59:59       ...         
    
                                                               WORKWOM  day  feat  \
    WeekLastDay         store brand       origin                                    
    1990-07-04 23:59:59 2     dominicks   1990-06-27 23:59:59     0.36    4  0.23   
                              minute.maid 1990-06-27 23:59:59     0.36    4  0.23   
                              tropicana   1990-06-27 23:59:59     0.36    4  0.23   
    1990-07-11 23:59:59 2     dominicks   1990-07-04 23:59:59     0.36   11  0.23   
                              minute.maid 1990-07-04 23:59:59     0.36   11  0.23   
    
                                                               price  year  \
    WeekLastDay         store brand       origin                             
    1990-07-04 23:59:59 2     dominicks   1990-06-27 23:59:59   2.33  1990   
                              minute.maid 1990-06-27 23:59:59   2.33  1990   
                              tropicana   1990-06-27 23:59:59   2.33  1990   
    1990-07-11 23:59:59 2     dominicks   1990-07-04 23:59:59   2.33  1990   
                              minute.maid 1990-07-04 23:59:59   2.33  1990   
    
                                                               price_lag1  \
    WeekLastDay         store brand       origin                            
    1990-07-04 23:59:59 2     dominicks   1990-06-27 23:59:59        2.33   
                              minute.maid 1990-06-27 23:59:59        2.33   
                              tropicana   1990-06-27 23:59:59        2.33   
    1990-07-11 23:59:59 2     dominicks   1990-07-04 23:59:59        2.33   
                              minute.maid 1990-07-04 23:59:59        2.33   
    
                                                               price_lag2  \
    WeekLastDay         store brand       origin                            
    1990-07-04 23:59:59 2     dominicks   1990-06-27 23:59:59        1.59   
                              minute.maid 1990-06-27 23:59:59        3.17   
                              tropicana   1990-06-27 23:59:59        3.87   
    1990-07-11 23:59:59 2     dominicks   1990-07-04 23:59:59        2.33   
                              minute.maid 1990-07-04 23:59:59        2.33   
    
                                                               grain_brand  \
    WeekLastDay         store brand       origin                             
    1990-07-04 23:59:59 2     dominicks   1990-06-27 23:59:59    dominicks   
                              minute.maid 1990-06-27 23:59:59  minute.maid   
                              tropicana   1990-06-27 23:59:59    tropicana   
    1990-07-11 23:59:59 2     dominicks   1990-07-04 23:59:59    dominicks   
                              minute.maid 1990-07-04 23:59:59  minute.maid   
    
                                                               grain_store  \
    WeekLastDay         store brand       origin                             
    1990-07-04 23:59:59 2     dominicks   1990-06-27 23:59:59            2   
                              minute.maid 1990-06-27 23:59:59            2   
                              tropicana   1990-06-27 23:59:59            2   
    1990-07-11 23:59:59 2     dominicks   1990-07-04 23:59:59            2   
                              minute.maid 1990-07-04 23:59:59            2   
    
                                                               horizon_origin  
    WeekLastDay         store brand       origin                               
    1990-07-04 23:59:59 2     dominicks   1990-06-27 23:59:59               1  
                              minute.maid 1990-06-27 23:59:59               1  
                              tropicana   1990-06-27 23:59:59               1  
    1990-07-11 23:59:59 2     dominicks   1990-07-04 23:59:59               1  
                              minute.maid 1990-07-04 23:59:59               1  
    
    [5 rows x 25 columns]
    

**RegressionForecaster**

A função [RegressionForecaster](https://docs.microsoft.com/python/api/ftk.models.regressionforecaster.regressionforecaster) encapsula avaliadores de regressão sklearn para que eles podem ser treinados no TimeSeriesDataFrame. O forecaster encapsulado também coloca cada grupo (neste caso, o repositório) no mesmo modelo. O forecaster pode aprender um modelo para um grupo de séries que foram consideradas semelhantes e podem ser agrupadas. Um modelo para um grupo de séries geralmente usa os dados da série mais longa para melhorar as previsões das séries curtas. Use os modelos `Lasso` e `RandomForest` diretamente do `scikit-learn`.  Você pode substituir esses modelos por outros modelos na biblioteca que dá suporte à regressão. 


```python
lasso_model = RegressionForecaster(estimator=Lasso())
elastic_net_model = RegressionForecaster(estimator=ElasticNet())
knn_model = RegressionForecaster(estimator=KNeighborsRegressor())
random_forest_model = RegressionForecaster(estimator=RandomForestRegressor())
boosted_trees_model = RegressionForecaster(estimator=GradientBoostingRegressor())

ml_union = ForecasterUnion(forecaster_list=[
    ('lasso', lasso_model), 
    ('elastic_net', elastic_net_model), 
    ('knn', knn_model), 
    ('random_forest', random_forest_model), 
    ('boosted_trees', boosted_trees_model)
]) 
```


```python
warnings.filterwarnings("ignore") 
ml_union.fit(train_feature_tsdf, y=train_feature_tsdf.ts_value)
ml_results = ml_union.predict(test_feature_tsdf, retain_feature_column=True)
```


```python
ml_results_by_grain = ml_results.calc_error(err_name='MAPE', by=ml_results.slice_key_colnames)
ml_results_no_origin = ml_results_by_grain.copy()
del ml_results_no_origin['ForecastOriginTime']
all_results = pd.concat([error_bygrain_univariate, ml_results_no_origin])
all_results[['ModelName', 'MAPE']].groupby('ModelName').median()
```



<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>MAPE</th>
    </tr>
    <tr>
      <th>ModelName</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>arima</th>
      <td>81,21</td>
    </tr>
    <tr>
      <th>boosted_trees</th>
      <td>45,31</td>
    </tr>
    <tr>
      <th>elastic_net</th>
      <td>65,70</td>
    </tr>
    <tr>
      <th>ets</th>
      <td>159,17</td>
    </tr>
    <tr>
      <th>knn</th>
      <td>67,94</td>
    </tr>
    <tr>
      <th>lasso</th>
      <td>63,18</td>
    </tr>
    <tr>
      <th>naive</th>
      <td>61,64</td>
    </tr>
    <tr>
      <th>random_forest</th>
      <td>43,56</td>
    </tr>
    <tr>
      <th>seasonal_naive</th>
      <td>154,60</td>
    </tr>
  </tbody>
</table>



O modelo de aprendizado de máquina foi capaz de aproveitar os recursos adicionados e as semelhanças entre as séries uma previsão mais precisa.

**Validação cruzada e limpeza de parâmetros**

O pacote adapta algumas funções tradicionais de aprendizado de máquina para uso em previsões.  [RollingOriginValidator](https://docs.microsoft.com/python/api/ftk.model_selection.cross_validation.rollingoriginvalidator) não faz validação cruzada temporalmente, respeitando o que seria e não seria conhecido em uma estrutura de previsão. 

Na figura a seguir, cada quadrado representa dados de um ponto de tempo. Os quadrados em azul representam treinamento e os quadrados em laranja representam testes em cada partição. Os dados de teste devem vir de pontos de tempo após o ponto de tempo de treinamento mais longo. Caso contrário, os dados futuros serão vazados para os dados de treinamento, fazendo com que a avaliação do modelo seja invalidada. 

![png](./media/how-to-build-deploy-forecast-models/cv_figure.PNG)


```python
# Set up the `RollingOriginValidator` to do 2 folds of rolling origin cross-validation
rollcv = RollingOriginValidator(n_splits=2)
randomforest_model_for_cv = RegressionForecaster(estimator=RandomForestRegressor(),
                                                 make_grain_features=False)

# Set up our parameter grid and feed it to our grid search algorithm
param_grid_rf = {'estimator__n_estimators': np.array([10, 50, 100])}
grid_cv_rf = TSGridSearchCV(randomforest_model_for_cv, param_grid_rf, cv=rollcv)

# fit and predict
randomforest_cv_fitted= grid_cv_rf.fit(train_feature_tsdf, y=train_feature_tsdf.ts_value)
print('Best paramter: {}'.format(randomforest_cv_fitted.best_params_))
```

    Best paramter: {'estimator__n_estimators': 100}
    


**Criar o pipeline final**   
Agora que você identificou o melhor modelo, pode criar e ajustar o pipeline final com todos os transformadores e o melhor modelo. 


```python
random_forest_model_final = RegressionForecaster(estimator=RandomForestRegressor(n_estimators=100))
pipeline_ml.add_pipeline_step('random_forest_estimator', random_forest_model_final)
pipeline_ml_fitted = pipeline_ml.fit(train_imputed_tsdf)
final_prediction = pipeline_ml_fitted.predict(test_tsdf)
```

## <a name="operationalization-deploy-and-consume"></a>Operacionalização: implantar e consumir

Nesta seção, você implanta um pipeline como um serviço Web do Azure Machine Learning e o consome em treinamento e pontuação.
Os pipelines não estão adequados para implantação. A pontuação do serviço Web implantado treina novamente o modelo e gera previsões sobre os novos dados. 

### <a name="set-model-deployment-parameters"></a>Definir parâmetros de implantação do modelo
Altere os parâmetros a seguir com seus próprios valores. Verifique se o ambiente do Azure Machine Learning, a conta de gerenciamento de modelos e o grupo de recursos estão localizados na mesma região.


```python
azure_subscription = '<subscription name>'

# Two deployment modes are supported: 'local' and 'cluster'. 
# 'local' deployment deploys to a local docker container.
# 'cluster' deployment deploys to a Azure Container Service Kubernetes-based cluster
deployment_type = '<deployment mode>'

# The deployment environment name. 
# This could be an existing environment or a new environment to be created automatically.
aml_env_name = '<deployment env name>'

# The resource group that contains the Azure resources related to the AML environment.
aml_env_resource_group = '<env resource group name>'

# The location where the Azure resources related to the AML environment are located at.
aml_env_location = '<env resource location>'

# The AML model management account name. This could be an existing model management account a new model management 
# account to be created automatically. 
model_management_account_name = '<model management account name>'

# The resource group that contains the Azure resources related to the model management account.
model_management_account_resource_group = '<model management account resource group>'

# The location where the Azure resources related to the model management account are located at.
model_management_account_location = '<model management account location>'

# The name of the deployment/web service.
deployment_name = '<web service name?'

# The directory to store deployment related files, such as pipeline pickle file, score script, 
# and conda dependencies file. 
deployment_working_directory = '<local working directory>'
```

### <a name="define-the-azure-machine-learning-environment-and-deployment"></a>Definir o ambiente e a implantação do Azure Machine Learning


```python
aml_settings = AMLSettings(azure_subscription=azure_subscription,
                     env_name=aml_env_name, 
                     env_resource_group=aml_env_resource_group,
                     env_location=aml_env_location, 
                     model_management_account_name=model_management_account_name, 
                     model_management_account_resource_group=model_management_account_resource_group,
                     model_management_account_location=model_management_account_location,
                     cluster=deployment_type)

pipeline_deploy = AzureMLForecastPipeline([('drop_columns', column_dropper), 
                                           ('fillna_imputer', fillna_imputer),
                                           ('time_index_featurizer', time_index_featurizer),
                                           ('random_forest_estimator', random_forest_model_final)
                                          ])

aml_deployment = ForecastWebserviceFactory(deployment_name=deployment_name,
                                           aml_settings=aml_settings, 
                                           pipeline=pipeline_deploy,
                                           deployment_working_directory=deployment_working_directory,
                                           ftk_wheel_loc='https://azuremlftkrelease.blob.core.windows.net/latest/azuremlftk-1.0.0b1-py3-none-any.whl')
```

### <a name="create-the-web-service"></a>Criar o serviço Web


```python
# This step can take 5 to 20 minutes if a new cluster needs to be provisioned
aml_deployment.deploy()
```

### <a name="score-the-web-service"></a>Classificar o serviço Web
Para classificar um pequeno conjunto de dados, use o método [score](https://docs.microsoft.com/python/api/ftk.operationalization.deployment.amlwebservice) para enviar uma chamada ao serviço Web para todos os dados.


```python
# Need to add empty prediction columns to the validation data frame and create a ForecastDataFrame.
# The scoring API will be updated in later versions to take TimeSeriesDataFrame directly. 
validate_tsdf = test_tsdf.assign(PointForecast=0.0, DistributionForecast=np.nan)
validate_fcast = ForecastDataFrame(validate_tsdf, pred_point='PointForecast', pred_dist='DistributionForecast')

# Define Score Context
score_context = ScoreContext(input_training_data_tsdf=train_imputed_tsdf,
                             input_scoring_data_fcdf=validate_fcast, 
                             pipeline_execution_type='train_predict')

# Get deployed web service
aml_web_service = aml_deployment.get_deployment()

# Score the web service
results = aml_web_service.score(score_context=score_context)

```

Para classificar um conjunto de dados grande, use o modo [pontuação paralela](https://docs.microsoft.com/python/api/ftk.operationalization.deployment.amlwebservice) para enviar varias chamadas a serviços Web, uma para cada grupo de dados.


```python
results = aml_web_service.score(score_context=score_context, method='parallel')
```

## <a name="next-steps"></a>Próximas etapas

Saiba mais sobre o Pacote do Azure Machine Learning para Previsão nestes artigos:

+ Leia a [visão geral do pacote e aprenda a instalá-lo](https://aka.ms/aml-packages/forecasting).

+ Explore os [documentos de referência](https://aka.ms/aml-packages/forecasting) do pacote.

+ Saiba mais sobre [outros pacotes do Python para o Azure Machine Learning](reference-python-package-overview.md).