---
title: Criar um Load Balancer Standard público com frontend de endereço IP público com redundância de zona usando a CLI do Azure | Microsoft Docs
description: Saiba como criar um Load Balancer Standard público com frontend de endereço IP público com redundância de zona usando a CLI do Azure
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2018
ms.author: kumud
ms.openlocfilehash: 785041b41395af385dcbbdc30df30e9d83db9e73
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39447574"
---
#  <a name="create-a-public-load-balancer-standard-with-zone-redundant-frontend-using-azure-cli"></a>Criar um Load Balancer Standard público com frontend com redundância de zona usando a CLI do Azure

Este artigo segue as etapas para a criação de um [Load Balancer Standard](https://aka.ms/azureloadbalancerstandard) público com um front-end com redundância de zona usando um endereço IP Público Standard.

Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Se você optar por instalar e usar a CLI localmente, certifique-se de que você instalou a versão mais recente da [CLI do Azure 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) e entrou em uma conta do Azure com [az login](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest#az-login).

> [!NOTE]
 O suporte para Zonas de Disponibilidade está disponível para selecionar recursos e regiões do Azure e famílias de tamanho de VM. Para obter mais informações sobre como começar e com quais recursos, regiões e famílias de tamanhos de VM do Azure você pode experimentar as zonas de disponibilidade, confira [Visão geral das Zonas de Disponibilidade](https://docs.microsoft.com/azure/availability-zones/az-overview). Para obter suporte, entre em contato em [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) ou [abra um tíquete de suporte do Azure](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json). 


## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Crie um grupo de recursos usando o seguinte comando:

```azurecli-interactive
az group create --name myResourceGroupSLB --location westeurope
```

## <a name="create-a-public-ip-standard"></a>Criar um IP público Standard

Crie um IP Público Standard usando o seguinte comando:

```azurecli-interactive
az network public-ip create --resource-group myResourceGroupSLB --name myPublicIP --sku Standard
```

## <a name="create-a-load-balancer"></a>Criar um balanceador de carga

Crie um Load Balancer Standard Público com o IP Público Standard que você criou na etapa anterior usando o seguinte comando:

```azurecli-interactive
az network lb create --resource-group myResourceGroupSLB --name myLoadBalancer --public-ip-address myPublicIP --frontend-ip-name myFrontEnd --backend-pool-name myBackEndPool --sku Standard
```

## <a name="create-an-lb-probe-on-port-80"></a>Criar uma investigação do balanceador de carga na porta 80

Crie uma investigação de integridade do balanceador de carga usando o seguinte comando:

```azurecli-interactive
az network lb probe create --resource-group myResourceGroupSLB --lb-name myLoadBalancer \
  --name myHealthProbe --protocol tcp --port 80
```

## <a name="create-an-lb-rule-for-port-80"></a>Criar uma regra de Load Balancer para a porta 80

Crie uma regra do balanceador de carga usando o seguinte comando:

```azurecli-interactive
az network lb rule create --resource-group myResourceGroup --lb-name myLoadBalancer --name myLoadBalancerRuleWeb \
  --protocol tcp --frontend-port 80 --backend-port 80 --frontend-ip-name myFrontEnd \
  --backend-pool-name myBackEndPool --probe-name myHealthProbe
```

## <a name="next-steps"></a>Próximas etapas
- Saiba mais sobre o [Load Balancer Standard e Zonas de Disponibilidade](load-balancer-standard-availability-zones.md).



