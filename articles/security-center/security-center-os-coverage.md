---
title: Plataformas com suporte na Central de Segurança do Azure | Microsoft Docs
description: Este documento fornece uma lista de sistemas operacionais Windows e Linux com suporte na Central de Segurança do Azure.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: 70c076ef-3ad4-4000-a0c1-0ac0c9796ff1
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2018
ms.author: terrylan
ms.openlocfilehash: b82b152ae4f44e475b5c3e9bda9d604f405503e0
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/28/2018
ms.locfileid: "43123724"
---
# <a name="supported-platforms-in-azure-security-center"></a>Plataformas com suporte na Central de Segurança do Azure
As recomendações e o monitoramento do estado de segurança estão disponíveis para VMs (máquinas virtuais), criadas usando ambos os modelos de implantação do Resource Manager e clássico, e computadores.

> [!NOTE]
> Saiba mais sobre os [modelos de implantação clássica e do Gerenciador de Recursos](../azure-classic-rm.md) para recursos do Azure.
>
>

## <a name="supported-platforms-for-windows-computers-and-vms"></a>Plataformas com suporte para computadores com Windows e VMs
Sistemas operacionais Windows com suporte:

* Windows Server 2008
* Windows Server 2008 R2
* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016


## <a name="supported-platforms-for-linux-computers-and-vms"></a>Plataformas com suporte para computadores Linux e VMs
Sistemas operacionais Linux com suporte:

* Ubuntu versões 12.04 LTS, 14.04 LTS, 16.04 LTS
* Debian versões 6, 7, 8, 9
* CentOS versões 5, 6, 7
* Red Hat Enterprise Linux (RHEL) versões 5, 6, 7
* SUSE Linux Enterprise Server (SLES) versões 11, 12
* Oracle Linux versões 5, 6, 7
* Amazon Linux 2012.09 a 2017
* OpenSSL 1.1.0 só tem suporte em plataformas x86_64 (64 bits)

> [!NOTE]
> A análise de comportamento de máquina virtual ainda não está disponível para sistemas operacionais Linux.
>
>

## <a name="vms-and-cloud-services"></a>VMs e Serviços de Nuvem
Também há suporte para VMs em execução em um serviço de nuvem. Apenas serviços de nuvem da Web e funções de trabalho em execução em slots de produção são monitorados. Para saber mais sobre o serviço de nuvem, confira [Visão geral dos Serviços de Nuvem](../cloud-services/cloud-services-choose-me.md).

## <a name="next-steps"></a>Próximas etapas

- [Guia de Operações e Planejamento da Central de Segurança do Azure](security-center-planning-and-operations-guide.md) – saiba como planejar e entender as considerações de design para adotar a Central de Segurança do Azure
- [Alertas de segurança por tipo na Central de segurança do Azure](security-center-alerts-type.md#virtual-machine-behavioral-analysis) – Saiba mais sobre a análise comportamental da máquina virtual e análise de memória de despejo de memória na Central de Segurança
- [Perguntas Frequentes sobre a Central de Segurança do Azure](security-center-faq.md) – encontre as perguntas frequentes sobre como usar o serviço
- [Blog de Segurança do Azure](http://blogs.msdn.com/b/azuresecurity/) – encontre postagens no blog sobre conformidade e segurança do Azure
