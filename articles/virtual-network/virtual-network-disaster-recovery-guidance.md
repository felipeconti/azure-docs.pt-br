---
title: Continuidade dos negócios da rede virtual | Microsoft Docs
description: Saiba o que fazer caso uma interrupção de serviço do Azure afete as Redes Virtuais do Azure.
services: virtual-network
documentationcenter: ''
author: NarayanAnnamalai
manager: jefco
editor: ''
ms.assetid: ad260ab9-d873-43b3-8896-f9a1db9858a5
ms.service: virtual-network
ms.workload: virtual-network
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2016
ms.author: narayan;aglick
ms.openlocfilehash: d993144006d1fb17d79ffee4f2da538264a309a4
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/09/2018
ms.locfileid: "29854148"
---
# <a name="virtual-network--business-continuity"></a>Rede Virtual – Continuidade de Negócios

## <a name="overview"></a>Visão geral
Uma Rede Virtual (VNet) é uma representação da sua rede na nuvem. Ela permite que você defina seu próprio espaço de endereço IP privado e segmente a rede em sub-redes. As VNets servem como um limite de confiança para hospedar seus recursos de computação, como as Máquinas Virtuais e os Serviços de Nuvem do Azure (funções web/de trabalho). Uma VNet permite a comunicação de IP privada direta entre os recursos hospedados nela. Vincule uma rede virtual a uma rede local por meio de um Gateway de VPN ou do ExpressRoute.

Uma VNet é criada dentro do escopo de uma região. Você pode criar VNETs com o mesmo espaço de endereço em duas regiões diferentes (por exemplo, Leste dos EUA e Oeste dos EUA), mas não pode conectá-las entre si. 

## <a name="business-continuity"></a>Continuidade dos negócios

O seu aplicativo pode ser interrompido de várias maneiras diferentes. Uma região pode ser completamente cortada devido a um desastre natural ou parcial, devido a uma falha de vários dispositivos ou serviços. O impacto sobre o serviço de Rede Virtual é diferente em cada uma dessas situações.

**P: Se ocorrer uma interrupção de uma região inteira, o que fazer? Por exemplo, se uma região for completamente cortada devido a um desastre natural? O que acontece com as redes virtuais hospedadas na região?**

R: A rede virtual e os recursos na região afetada permanecem inacessíveis durante o tempo de interrupção do serviço.

![Diagrama de Rede Virtual Simples](./media/virtual-network-disaster-recovery-guidance/vnet.png)

**P: O que posso fazer para recriar a mesma rede virtual em outra região?**

R: As redes virtuais são recursos relativamente leves. Você pode invocar as APIs do Azure para criar uma VNet com o mesmo espaço de endereço em uma região diferente. Para recriar o mesmo ambiente que estava presente na região afetada, faça chamadas à API para reimplantar as funções web e de trabalho dos Serviços de Nuvem e as máquinas virtuais que você tinha. Se você tiver conectividade local, como em uma implantação híbrida, precisará implantar um novo Gateway de VPN e se conectar à sua rede local.

Para criar uma rede virtual, consulte [Criar uma rede virtual](manage-virtual-network.md#create-a-virtual-network).

**P: Uma réplica de uma VNet em uma determinada região pode ser recriada em outra região antecipadamente?**

R: Sim, você pode criar duas VNets usando o mesmo espaço de endereço IP privado e os recursos em duas regiões diferentes antecipadamente. Se você estiver hospedando serviços voltados para a Internet na VNET, pode configurar o Gerenciador de Tráfego para distribuir o tráfego geograficamente para a região que está ativa. No entanto, você não pode conectar duas VNETs com o mesmo espaço de endereço à rede local, uma vez que isso causará problemas de roteamento. No momento de um desastre e da perda de uma VNET em uma região, você pode conectar a outra VNET na região disponível com o espaço de endereço correspondente à rede local.

Para criar uma rede virtual, consulte [Criar uma rede virtual](manage-virtual-network.md#create-a-virtual-network).

