---
title: Migrações para Azure - Perguntas frequentes (FAQ) | Microsoft Docs
description: Apresenta respostas para perguntas frequentes sobre Migrações para Azure
author: snehaamicrosoft
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/10/2018
ms.author: snehaa
ms.openlocfilehash: 3f035f38b1ad68e9e39d151ffad3fc650a0a1d80
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37952742"
---
# <a name="azure-migrate---frequently-asked-questions-faq"></a>Migrações para Azure - Perguntas frequentes (FAQ)

Este artigo contém perguntas frequentes sobre o Migrações para Azure. Se você tiver mais perguntas após a leitura deste artigo, publique-as no [fórum do Migrações para Azure](http://aka.ms/AzureMigrateForum).

## <a name="general"></a>Geral

### <a name="does-azure-migrate-support-assessment-of-only-vmware-workloads"></a>As Migrações para Azure dão suporte apenas à avaliação de cargas de trabalho VMware?

Sim, Migrações para Azure atualmente dão suporte apenas à avaliação de cargas de trabalho do VMware. O suporte para Hyper-V e servidores físicos será habilitado no futuro.

### <a name="does-azure-migrate-need-vcenter-server-to-discover-a-vmware-environment"></a>O Migrações para Azure precisa do vCenter Server para descobrir um ambiente VMware?

Sim, o Migrações para Azure precisa do vCenter Server para descobrir um ambiente VMware. Ele não oferece suporte à descoberta de hosts ESXi que não são gerenciados por um vCenter Server.

### <a name="how-is-azure-migrate-different-from-azure-site-recovery"></a>Como o Migrações para Azure é diferente do Azure Site Recovery?

O Migrações para Azure é um serviço de avaliação que ajuda você a descobrir suas cargas de trabalho locais e planejar a migração para o Azure. O [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/migrate-tutorial-on-premises-azure), além de ser uma solução de recuperação de desastre, ajuda a migrar cargas de trabalho locais para VMs de IaaS no Azure.

### <a name="whats-the-difference-between-using-azure-migrate-for-assessments-and-the-map-toolkit"></a>Qual é a diferença entre o uso de Migrações para Azure para avaliações e o Kit de Ferramentas Map?

[As Migrações para Azure](migrate-overview.md) fornecem avaliação de migração especificamente para ajudar na preparação para migração e avaliação de cargas de trabalho locais no Azure. O [Kit de Ferramentas MAP (Microsoft Assessment and Planning)](https://www.microsoft.com/en-us/download/details.aspx?id=7826) tem outras funcionalidades. Por exemplo, planejamento de migração para versões mais recentes de sistemas operacionais Windows de cliente e servidor, controle de uso de software etc. Para esses cenários, continue a usar o Kit de ferramentas MAP.


### <a name="how-is-azure-migrate-different-from-azure-site-recovery-deployment-planner"></a>Como o Migrações para Azure é diferente do Planejador de Implantações do Azure Site Recovery?

O Migrações para Azure é uma ferramenta de planejamento da migração e o Planejador de Implantações do Azure Recovery Site é uma ferramenta de planejamento de recuperação de desastre (DR).

**Migração do VMware para Azure**: Se você pretende migrar suas cargas de trabalho locais para o Azure, use o Migrações para Azure para planejamento de migração. O Migrações para Azure avalia as cargas de trabalho locais e fornece diretrizes, insights e mecanismos para ajudá-lo a migrar para o Azure. Após preparar o seu plano de migração, você pode usar serviços como o Azure Site Recovery e o Serviço de Migração de Banco de Dados do Azure para migrar as máquinas para o Azure.

**Migração do Hyper-V para o Azure**: O Migrações para Azure atualmente só oferece suporte para avaliação de máquinas virtuais VMware para migração para o Azure. O suporte para Hyper-V está em nossos planos para o Migrações para Azure. Enquanto isso, você pode usar o Planejador de Implantação da Recuperação de Site. Após habilitar o suporte para Hyper-V no Migrações para Azure, você pode usar o Migrações para Azure para planejar a migração das cargas de trabalho do Hyper-V.

**Recuperação de desastre do VMware/Hyper-V para o Azure**: Se você pretende fazer a recuperação de desastre (DR) no Azure usando o Azure Site Recovery, use o Planejador de Implantações para o planejamento de DR. O Planejador de Implantações do Site Recovery faz uma avaliação detalhada específica para o ASR do seu ambiente local. Ele fornece recomendações requeridas pelo Site Recovery para operações de recuperação de desastre com êxito como, por exemplo, replicação e failover das suas máquinas virtuais.  

### <a name="which-azure-regions-are-supported-by-azure-migrate"></a>Quais regiões do Azure têm suporte para Migrações para Azure?

Atualmente, as Migrações para Azure dá suporte ao Leste dos EUA e Centro-oeste dos EUA como locais de projeto de migração. Observe que, embora você possa criar apenas projetos de migração no Centro-oeste dos EUA e no Leste dos EUA, ainda é possível avaliar os computadores para [vários locais de destino](https://docs.microsoft.com/azure/migrate/how-to-modify-assessment#edit-assessment-properties). O local do projeto é usado apenas para armazenar os dados descobertos.

### <a name="how-does-the-on-premises-site-connect-to-azure-migrate"></a>Como o site local se conecta a Migrações para Azure?

A conexão pode ser pela Internet ou usar o ExpressRoute com emparelhamento público.

### <a name="can-i-harden-the-vm-set-up-with-the-ova-template"></a>Posso proteger a VM configurada com o Modelo .OVA?

Componentes adicionais (por exemplo, um antivírus) podem ser adicionados ao. Modelo OVA, desde que as regras de comunicação e firewall necessárias para o dispositivo de Migrações para Azure funcionar sejam deixadas como estão.   

## <a name="discovery-and-assessment"></a>Detecção e avaliação

### <a name="what-data-is-collected-by-azure-migrate"></a>Quais dados são coletados pelo Migrações para Azure?

O Migrações para Azure oferece suporte a dois tipos de descoberta, descoberta de dispositivo e descoberta de agente.
A descoberta de dispositivo coleta metadados sobre as VMs locais, a lista completa de metadados coletados pelo dispositivo está abaixo:

**Dados de configuração da VM**
- Nome de exibição da VM (no vCenter)
- Caminho de inventário da VM (host/cluster/pasta no vCenter)
- Endereço IP
- Endereço MAC
- Sistema operacional
- Número de núcleos, discos, NICs
- Tamanho da memória, tamanhos de disco

**Dados de desempenho da VM**
- Uso da CPU
- Uso de memória
- Para cada disco anexado à VM:
  - Taxa de transferência de leitura do disco
  - Taxa de transferência de gravações de disco
  - Operações de leitura do disco por segundo
  - Operações de gravação do disco por segundo
- Para cada adaptador de rede conectado à VM:
  - Rede no
  - Limite de rede

A descoberta de agente é uma opção disponível sobre a descoberta de dispositivo e ajuda os clientes a [visualizar dependências](how-to-create-group-machine-dependencies.md) das máquinas virtuais locais. Os agentes de dependência coletam detalhes como FQDN, SO, endereço IP, endereço MAC, processos em execução dentro da VM e as conexões TCP de entrada/saída da VM. A descoberta do agente é opcional e você pode optar por não instalar os agentes se não deseja visualizar as dependências das VMs.

### <a name="where-is-the-collected-data-stored-and-for-how-long"></a>Onde e por quanto tempo os dados coletados são armazenados?

Os dados coletados pelo dispositivo coletor são armazenados no local do Azure que você especificar durante a criação do projeto de migração. Os dados são armazenados com segurança em uma assinatura da Microsoft e são excluídos quando o usuário exclui o projeto do Migrações para Azure.

Para visualização de dependência, se você instalar agentes em máquinas virtuais, os dados coletados pelos agentes de dependência são armazenados nos Estados Unidos em um espaço de trabalho do OMS criado na assinatura do usuário. Esses dados são excluídos quando você exclui o espaço de trabalho do OMS em sua assinatura. [Saiba mais](https://docs.microsoft.com/azure/migrate/concepts-dependency-visualization).

### <a name="is-the-data-encrypted-at-rest-and-while-in-transit"></a>Os dados são criptografados em repouso e em trânsito?

Sim, os dados coletados são criptografados em repouso e em trânsito. Os metadados coletados pelo dispositivo são enviados para o serviço do Migrações para Azure pela internet por meio de https. Os metadados coletados são armazenados no [Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/database-encryption-at-rest) e no [armazenamento de Blobs do Azure](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) em uma assinatura Microsoft e são criptografados em repouso.

Os dados coletados pelos agentes de dependência também são criptografados em trânsito (canal https seguro) e são armazenados em um espaço de trabalho do Log Agente na assinatura do usuário. Eles também são criptografados em repouso.

### <a name="how-does-the-collector-communicate-with-the-vcenter-server-and-the-azure-migrate-service"></a>Como o coletor se comunica com o vCenter Server e o serviço do Migrações para Azure?

O dispositivo coletor se conecta ao vCenter Server (porta 443) usando as credenciais fornecidas pelo usuário no dispositivo. Ele consulta o vCenter Server usando a VMware PowerCLI para coletar metadados sobre as máquinas virtuais gerenciadas pelo vCenter Server. Ele coleta os dados de configuração sobre VMs (núcleos, memória, discos, NIC, etc.), bem como o histórico de desempenho de cada máquina virtual para o último mês do vCenter Server. Os metadados coletados são enviados para o serviço do Migrações para Azure (pela internet por meio de https) para avaliação. [Saiba mais](concepts-collector.md)

### <a name="can-i-connect-the-same-collector-appliance-to-multiple-vcenter-servers"></a>Posso conectar o mesmo dispositivo coletor em vários servidores do vCenter?

Sim, um único dispositivo coletor pode ser usado para descobrir vários servidores vCenter, mas não ao mesmo tempo. Você precisará executar as descobertas uma após a outra.

### <a name="is-the-ova-template-used-by-site-recovery-integrated-with-the-ova-used-by-azure-migrate"></a>O modelo .OVA usado pelo Site Recovery é integrado ao .OVA usado pelas Migrações para Azure?

Atualmente, não há integração. O modelo .OVA no Site Recovery é usado para configurar um servidor de configuração do Site Recovery para replicação de VM VMware/servidor físico. O .OVA usado pelas Migrações para Azure é usado para descobrir máquinas virtuais VMware gerenciadas por um vCenter server, para fins de avaliação de migração.

### <a name="i-changed-my-machine-size-can-i-rerun-the-assessment"></a>Alterei o tamanho da máquina. Posso executar novamente a avaliação?

Se você alterar as configurações em uma VM que deseje avaliar, dispare a detecção novamente usando o dispositivo coletor. No dispositivo, use a opção **Iniciar coleta novamente** para fazer isso. Depois que a coleção for concluída, selecione a opção **Recalcular** para a avaliação no portal, para obter os resultados atualizados da avaliação.

### <a name="how-can-i-discover-a-multi-tenant-environment-in-azure-migrate"></a>Como é possível descobrir um ambiente multilocatário no Migrações para Azure?

Se você tiver um ambiente compartilhado entre locatários e não quiser descobrir as VMs de um locatário na assinatura de outro locatário, poderá usar o campo Escopo no coletor para escopo da descoberta. Se os locatários estiverem compartilhando hosts, crie uma credencial que tenha acesso somente leitura somente às VMs pertencentes ao locatário específico e, em seguida, use essa credencial no dispositivo do coletor e especifique o Escopo como o host a realizar a descoberta. Alternativamente, também é possível criar pastas no vCenter Server (ou seja, pasta1 para locatário1 e pasta2 para locatário2), no host compartilhado, mova as VMs para locatário1 na pasta1 e para locatário2 na pasta2 e, em seguida, especifique as descobertas no coletor especificando a pasta apropriada.

### <a name="how-many-virtual-machines-can-be-discovered-in-a-single-migration-project"></a>Quantas máquinas virtuais podem ser descobertas em um único projeto de migração?

Você pode descobrir 1500 máquinas virtuais em um único projeto de migração. Se você tiver mais máquinas em seu ambiente local, [saiba mais](how-to-scale-assessment.md) sobre como você pode descobrir um ambiente grande no Migrações para Azure.

## <a name="dependency-visualization"></a>Visualização de dependência

### <a name="do-i-need-to-pay-to-use-the-dependency-visualization-feature"></a>É necessário pagar para usar o recurso de visualização de dependência?

As Migrações para Azure estão disponíveis sem custo adicional. Saiba mais sobre os preços de Migrações para Azure [aqui](https://azure.microsoft.com/pricing/details/azure-migrate/).

### <a name="can-i-use-an-existing-workspace-for-dependency-visualization"></a>Posso usar um espaço de trabalho existente para visualização de dependência?

O Migrações para Azure não suporta o uso de um espaço de trabalho existente para visualização de dependência, no entanto, o Microsoft Monitoring Agent (MMA) oferece suporte à hospedagem múltipla e permite que você envie dados para vários espaços de trabalho. Então se você já tiver agentes implantados e configurados para um espaço de trabalho, você pode aproveitar a hospedagem múltipla no agente MMA e configurá-lo para o espaço de trabalho do Migrações para Azure (além do espaço de trabalho existente) e fazê-lo funcionar. [Aqui](https://blogs.technet.microsoft.com/msoms/2016/05/26/oms-log-analytics-agent-multi-homing-support/) está um blog sobre como você pode habilitar a hospedagem múltipla em um agente MMA.

## <a name="next-steps"></a>Próximas etapas

- Leia a [visão geral do Migrações para Azure](migrate-overview.md)
- Saiba como você pode [descobrir e avaliar](tutorial-assessment-vmware.md) um ambiente VMware
