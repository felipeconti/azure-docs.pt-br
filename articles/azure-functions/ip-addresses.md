---
title: Endereços IP no Azure
description: Saiba como encontrar endereços IP de entrada e saída para aplicativos de função e o que faz com que eles sejam alterados.
services: functions
documentationcenter: ''
author: ggailey777
manager: cfowler
editor: ''
ms.service: functions
ms.workload: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2018
ms.author: glenga
ms.openlocfilehash: c2e066ddf444cb81d7f0033641a300d18b266f62
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/01/2018
ms.locfileid: "39397840"
---
# <a name="ip-addresses-in-azure-functions"></a>Endereços IP no Azure

Este artigo explica os seguintes tópicos relacionados a endereços IP de aplicativos de função:

* Como encontrar os endereços IP atualmente em uso por um aplicativo de função.
* O que faz com que os endereços IP de um aplicativo de função sejam alterados.
* Como restringir os endereços IP que podem acessar um aplicativo de função.
* Como obter endereços IP dedicados para um aplicativo de função.

Os endereços IP estão associados a aplicativos de função, não a funções individuais. Solicitações HTTP de entrada não podem usar o endereço IP de entrada para chamar funções individuais; eles devem usar o nome de domínio padrão (functionappname.azurewebsites.net) ou um nome de domínio personalizado.

## <a name="function-app-inbound-ip-address"></a>Endereço IP de entrada do aplicativo de função

Cada aplicativo de função possui um único endereço IP de entrada. Para encontrar esse endereço IP:

1. Entre no [Portal do Azure](https://portal.azure.com).
2. Navegue até o aplicativo de função.
3. Selecione **recursos da plataforma**.
4. Selecione **Propriedades** e o endereço IP de entrada será exibido em **Endereço IP virtual**.

## <a name="function-app-outbound-ip-addresses"></a>Função de endereços IP de saída do aplicativo

Cada aplicativo de função tem um conjunto de endereços IP de saída disponíveis. Qualquer conexão de saída de uma função, como um banco de dados back-end, usa um dos endereços IP de saída disponíveis como o endereço IP de origem. Você não pode saber de antemão qual endereço IP uma determinada conexão usará. Por esse motivo, seu serviço de back-end deve abrir seu firewall para todos os endereços IP de saída do aplicativo de função.

Para encontrar os endereços IP de saída disponíveis para um aplicativo de função:

1. Faça login no [Azure Resource Explorer](https://resources.azure.com).
2. Selecione **assinaturas> {sua assinatura}> provedores> Microsoft.Web> sites**.
3. No painel JSON, encontre o site com uma propriedade `id` que termine no nome do seu aplicativo de função.
4. Veja `outboundIpAddresses` e `possibleOutboundIpAddresses`. 

O conjunto de `outboundIpAddresses` está atualmente disponível para o aplicativo de função. O conjunto de `possibleOutboundIpAddresses` inclui endereços IP que estarão disponíveis somente se o aplicativo de função [for dimensionado para outras camadas de preços](#outbound-ip-address-changes).

Uma maneira alternativa de encontrar os endereços IP de saída disponíveis é usando o [Cloud Shell](../cloud-shell/quickstart.md):

```azurecli-interactive
az webapp show --resource-group <group_name> --name <app_name> --query outboundIpAddresses --output tsv
az webapp show --resource-group <group_name> --name <app_name> --query possibleOutboundIpAddresses --output tsv
```

## <a name="data-center-outbound-ip-addresses"></a>Endereços IP de saída do data center

Se você precisar colocar na lista de permissões os endereços IP de saída usados pelos seus aplicativos de função, outra opção é colocar na lista de permissões o data center dos aplicativos de função (região do Azure). Você pode [fazer o download de um arquivo XML que lista endereços IP para todos os datacenters do Azure](https://www.microsoft.com/en-us/download/details.aspx?id=41653). Em seguida, localize o elemento XML que se aplica à região em que seu aplicativo de função é executado.

Por exemplo, é assim que o elemento XML da Europa Ocidental pode parecer:

```
  <Region Name="europewest">
    <IpRange Subnet="13.69.0.0/17" />
    <IpRange Subnet="13.73.128.0/18" />
    <!-- Some IP addresses not shown here -->
    <IpRange Subnet="213.199.180.192/27" />
    <IpRange Subnet="213.199.183.0/24" />
  </Region>
```

 Para obter informações sobre quando este arquivo é atualizado e quando os endereços IP são alterados, expanda a seção **Detalhes** da [página do Centro de Download](https://www.microsoft.com/en-us/download/details.aspx?id=41653).

## <a name="inbound-ip-address-changes"></a>Mudanças no endereço IP de entrada

 O endereço IP de entrada **pode** mudar quando você:

- Exclua um aplicativo de função e recrie-o em um grupo de recursos diferente.
- Exclua o último aplicativo de função em uma combinação de grupo de recursos e região e recrie-o.
- Exclua uma ligação SSL, como durante a [renovação do certificado](../app-service/app-service-web-tutorial-custom-ssl.md#renew-certificates)).

O endereço IP de entrada também pode mudar quando você não realizou nenhuma ação, como as listadas.

## <a name="outbound-ip-address-changes"></a>Mudanças no endereço IP de saída

O conjunto de endereços IP de saída disponíveis para um aplicativo de função pode mudar quando você:

* Execute qualquer ação que possa alterar o endereço IP de entrada.
* Altere a camada de preços do seu plano de serviço do aplicativo. A lista de todos os possíveis endereços IP de saída que seu aplicativo pode usar, para todas as camadas de preços, está na `possibleOutboundIPAddresses`propriedade. Consulte [Localizar IPs de saída](#find-outbound-ip-addresses).

O endereço IP de entrada também pode mudar quando você não realizou nenhuma ação, como as listadas.

Para forçar deliberadamente uma alteração de endereço IP de saída:

1. Expanda seu plano de serviço de aplicativos para cima ou para baixo entre os níveis de preços Padrão e Premium v2.
2. Esperar 10 minutos.
3. Volte para onde você começou.

## <a name="ip-address-restrictions"></a>Restrições de endereço IP

Você pode configurar uma lista de endereços IP que você deseja permitir ou negar acesso a um aplicativo de função. Para obter mais informações, consulte [Restrições de IP estático do Serviço de Aplicativo do Azure](../app-service/app-service-ip-restrictions.md).

## <a name="dedicated-ip-addresses"></a>Endereços IP dedicados

Se você precisar de endereços IP dedicados e estáticos, recomendamos [Ambientes do Serviço de Aplicativo](../app-service/environment/intro.md) (o [Nível isolado](https://azure.microsoft.com/pricing/details/app-service/) dos planos do Serviço de Aplicativo). Para obter mais informações, consulte [Endereços IP do Ambiente de Serviço de Aplicativo](../app-service/environment/network-info.md#ase-ip-addresses) e [Como controlar o tráfego de entrada para um Ambiente de Serviço de Aplicativo](../app-service/environment/app-service-app-service-environment-control-inbound-traffic.md).

Para descobrir se seu aplicativo de função é executado em um Ambiente de Serviço de Aplicativo:

1. Entre no [Portal do Azure](https://portal.azure.com).
2. Navegue até o aplicativo de função.
3. Selecione o guia **Visão geral**.
4. A camada do plano de Serviço de Aplicativo aparece em **Plano de serviço de aplicativo / camada de preço**. A camada de preços do Ambiente de Serviço de Aplicativo é **Isolado**.
 
Como alternativa, você pode usar o [Cloud Shell](../cloud-shell/quickstart.md):

```azurecli-interactive
az webapp show --resource-group <group_name> --name <app_name> --query sku --output tsv
```

O Ambiente do Serviço de Aplicativo `sku` é `Isolated`.

## <a name="next-steps"></a>Próximas etapas

Uma causa comum de alterações de IP é a função de escala de aplicativos. [Saiba mais sobre o dimensionamento do aplicativo de função](functions-scale.md).
