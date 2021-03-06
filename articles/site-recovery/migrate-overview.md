---
title: Sobre a migração no Azure Site Recovery | Microsoft Docs
description: Este artigo descreve como migrar VMs locais e do Azure que usam o serviço Azure Site Recovery.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: raynew
ms.openlocfilehash: 2cc030bf9724e98c1ef0a43134c5066c81b38b6b
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2018
ms.locfileid: "39213081"
---
# <a name="about-migration"></a>Sobre a migração

Leia este artigo para obter uma visão geral de como o serviço [Azure Site Recovery](site-recovery-overview.md) ajuda você a migrar as máquinas. 

Veja o que você pode migrar com a Site Recovery:

- **Migrar do local para o Azure**: migrar cargas de trabalho em VMs do Hyper-V locais, VMs VMware e servidores físicos para o Azure. Após a migração, as cargas de trabalho em execução nas máquinas locais serão executadas em VMs do Azure. 
- **Migrar dentro do Azure**: Migrar VMs do Azure entre regiões do Azure. 
- **Migrar AWS**: Migrar instâncias do Windows AWS para VMs do IaaS do Azure. 


## <a name="what-do-we-mean-by-migration"></a>O que queremos dizer por migração?

Além de usar o Site Recovery para recuperação de desastre de VMs locais e do Azure, você poderá usar o serviço Site Recovery para migrá-las. Qual é a diferença?

- Para a recuperação de desastre, você pode replicar máquinas regularmente no Azure. Quando ocorre uma paralisação, há o failover de máquinas do site primário para o site secundário do Azure e você as acessa a partir daí. Quando o site primário estiver disponível novamente, você executar o failback a partir do Azure.
- Para a migração, você replica as máquinas locais para o Azure ou as VMs do Azure para uma região secundária. Em seguida, faça o failover da VM do site primário para o secundário e conclua o processo de migração. Não há nenhum failback envolvido.  


## <a name="migration-scenarios"></a>Cenários de migração

**Cenário** | **Detalhes**
--- | ---
**Migrar do local para o Azure** | Você pode migrar máquinas virtuais do VMware locais, VMs do Hyper-V e servidores físicos para o Azure. Para fazer isso, você conclui quase as mesmas etapas que usaria para a recuperação de desastre completo. Você simplesmente não faz failover de máquinas do Azure para o site local.
**Migrar entre regiões do Azure** | Você pode migrar máquinas virtuais do Azure de uma região do Azure para outra. Após a conclusão da migração, você poderá configurar a recuperação de desastre para VMs do Azure agora na região secundária para a qual migrou.
**Migrar AWS para o Azure** | Você pode migrar instâncias do AWS para VMs do Azure. O Site Recovery trata instâncias do AWS como servidores físicos para fins de migração. 

## <a name="next-steps"></a>Próximas etapas

- [Migrar máquinas locais para o Azure](migrate-tutorial-on-premises-azure.md)
- [Migrar máquinas virtuais de uma região do Azure para outra](azure-to-azure-tutorial-migrate.md)
- [Migrar AWS para o Azure](migrate-tutorial-aws-azure.md)
