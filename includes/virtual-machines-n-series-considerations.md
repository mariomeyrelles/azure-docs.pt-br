---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: virtual-machines-linux
author: dlepow
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 03/19/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 3267f649e360c512a5523ce1d5948719a1969934
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2018
---
## <a name="deployment-considerations"></a>Considerações de implantação

* Para ver a disponibilidade das VMs da Série N, veja [Produtos disponíveis por região](https://azure.microsoft.com/regions/services/).

* As VMs da Série N só podem ser implantadas no modelo de implantação do Resource Manager.

* As VMs da série N diferem no tipo de Armazenamento do Microsoft Azure que dão suporte aos discos. As VMs NC e NV dão suporte somente a discos de VM que executam backup em HDD (Armazenamento em Disco Standard). As VMs NCv2, ND e NCv3 somente dão suporte a discos de VM que executam backup em SSD (Armazenamento em Disco Premium).

* Se você quiser implantar mais do que algumas VMs da Série N, considere uma assinatura pré-paga ou outras opções de compra. Se estiver usando uma [conta gratuita do Azure](https://azure.microsoft.com/free/), você poderá usar apenas um número limitado de núcleos de computação do Azure.

* Talvez seja necessário aumentar a cota de núcleos (por região) em sua assinatura do Azure, e aumentar a cota separada para núcleos NC, NCv2, NCv3, ND ou NV. Para solicitar um aumento de cota, [abra uma solicitação de atendimento ao cliente online](../articles/azure-supportability/how-to-create-azure-support-request.md) gratuitamente. Os limites padrão podem variar dependendo de sua categoria de assinatura.




