---
title: Tutorial do Kubernetes no Azure - Dimensionar Aplicativo
description: Tutorial de AKS – dimensionar aplicativo
services: container-service
author: dlepow
manager: jeconnoc
ms.service: container-service
ms.topic: tutorial
ms.date: 02/22/2018
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: 7b962ccd8349996cd33cc3960391cba8fce549ad
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/10/2018
---
# <a name="tutorial-scale-application-in-azure-kubernetes-service-aks"></a>Tutorial: dimensionar o aplicativo no AKS (Serviço de Kubernetes do Azure)

Se você estiver seguindo os tutoriais, terá um cluster de Kubernetes operacional no AKS e já implantou o aplicativo Azure Voting.

Neste tutorial, parte cinco de oito, você expandirá os pods no aplicativo e experimentará o dimensionamento automático de pod. Você também aprenderá como dimensionar o número de nós da VM do Azure para alterar a capacidade do cluster para hospedagem de cargas de trabalho. As tarefas concluídas incluem:

> [!div class="checklist"]
> * Dimensionar os nós do Azure no Kubernetes
> * Dimensionamento manual de pods Kubernetes
> * Configuração de pods com dimensionamento automático executando o front-end do aplicativo

Nos tutoriais subsequentes, o aplicativo Azure Vote é atualizado e o Log Analytics é configurado para monitorar o cluster Kubernetes.

## <a name="before-you-begin"></a>Antes de começar

Nos tutoriais anteriores, um aplicativo foi empacotado em uma imagem de contêiner, essa imagem foi carregada no Registro de Contêiner do Azure e um cluster Kubernetes foi criado. Em seguida, o aplicativo foi executado no cluster Kubernetes.

Se você ainda não realizou essas etapas e deseja continuar acompanhando, retorne ao [Tutorial 1 – Criar imagens de contêiner][aks-tutorial-prepare-app].

## <a name="scale-aks-nodes"></a>Dimensionar nós do AKS

Se você criou o cluster Kubernetes usando os comandos no tutorial anterior, ele tem um nó. Você pode ajustar o número de nós manualmente se planejar ter mais ou menos cargas de trabalho de contêiner no cluster.

O exemplo a seguir aumenta o número de nós para três no cluster Kubernetes chamado *myAKSCluster*. Esse comando leva alguns minutos para ser concluído.

```azurecli
az aks scale --resource-group=myResourceGroup --name=myAKSCluster --node-count 3
```

A saída deverá ser semelhante a:

```
"agentPoolProfiles": [
  {
    "count": 3,
    "dnsPrefix": null,
    "fqdn": null,
    "name": "myAKSCluster",
    "osDiskSizeGb": null,
    "osType": "Linux",
    "ports": null,
    "storageProfile": "ManagedDisks",
    "vmSize": "Standard_D2_v2",
    "vnetSubnetId": null
  }
```

## <a name="manually-scale-pods"></a>Dimensionar pods manualmente

Até o momento, a instância de Redis e front-end do Azure Vote foi implantada, cada uma com uma réplica única. Para verificar, execute o comando [kubectl get][kubectl-get].

```azurecli
kubectl get pods
```

Saída:

```
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-2549686872-4d2r5   1/1       Running   0          31m
azure-vote-front-848767080-tf34m   1/1       Running   0          31m
```

Altere manualmente o número de pods na `azure-vote-front` implantação usando o comando [kubectl scale][kubectl-scale]. Este exemplo aumenta o número para 5.

```azurecli
kubectl scale --replicas=5 deployment/azure-vote-front
```

Execute [kubectl get pods][kubectl-get] para verificar se o Kubernetes está criando os pods. Após aproximadamente um minuto, os pods adicionais estão em execução:

```azurecli
kubectl get pods
```

Saída:

```
NAME                                READY     STATUS    RESTARTS   AGE
azure-vote-back-2606967446-nmpcf    1/1       Running   0          15m
azure-vote-front-3309479140-2hfh0   1/1       Running   0          3m
azure-vote-front-3309479140-bzt05   1/1       Running   0          3m
azure-vote-front-3309479140-fvcvm   1/1       Running   0          3m
azure-vote-front-3309479140-hrbf2   1/1       Running   0          15m
azure-vote-front-3309479140-qphz8   1/1       Running   0          3m
```

## <a name="autoscale-pods"></a>Dimensionamento automático de pods

O Kubernetes dá suporte ao [dimensionamento automático horizontal de pods][kubernetes-hpa] para ajustar o número de pods em uma implantação, dependendo da utilização da CPU ou de outras métricas selecionadas.

Para usar o dimensionador automático, seus pods devem ter limites e solicitações de CPU definidos. Na implantação, `azure-vote-front` o contêiner de front-end solicita 0,25 CPU, com um limite de 0,5 CPU. As configurações têm a seguinte aparência:

```YAML
resources:
  requests:
     cpu: 250m
  limits:
     cpu: 500m
```

O exemplo a seguir usa o comando [kubectl autoscale][kubectl-autoscale] para dimensionar automaticamente o número de pods na implantação `azure-vote-front`. Aqui, se a utilização da CPU exceder 50%, o dimensionador automático aumenta os pods para um máximo de 10.


```azurecli
kubectl autoscale deployment azure-vote-front --cpu-percent=50 --min=3 --max=10
```

Execute o seguinte comando para ver o status do dimensionador automático:

```azurecli
kubectl get hpa
```

Saída:

```
NAME               REFERENCE                     TARGETS    MINPODS   MAXPODS   REPLICAS   AGE
azure-vote-front   Deployment/azure-vote-front   0% / 50%   3         10        3          2m
```

Após alguns minutos, com carga mínima no aplicativo Azure Vote, o número de réplicas de pod diminui automaticamente para 3.

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você usou diferentes recursos de colocação em escala em seu cluster Kubernetes. As tarefas abordadas incluíram:

> [!div class="checklist"]
> * Dimensionamento manual de pods Kubernetes
> * Configuração de pods com dimensionamento automático executando o front-end do aplicativo
> * Dimensionar os nós do Azure no Kubernetes

Avance para o próximo tutorial para saber mais sobre como atualizar um aplicativo no Kubernetes.

> [!div class="nextstepaction"]
> [Atualizar um aplicativo no Kubernetes][aks-tutorial-update-app]

<!-- LINKS - external -->
[kubectl-autoscale]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#autoscale
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-scale]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#scale
[kubernetes-hpa]: https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/

<!-- LINKS - internal -->
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[aks-tutorial-update-app]: ./tutorial-kubernetes-app-update.md
