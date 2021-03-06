---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 06/03/2018
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: b011dd5993e63fe9bce36ec8b8c1b4739dbf704b
ms.sourcegitcommit: 04fc1781fe897ed1c21765865b73f941287e222f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39037856"
---
# <a name="azure-managed-disks-overview"></a>Visão geral do Azure Managed Disks

O Azure Managed Disks simplifica o gerenciamento de discos para VMs IaaS do Azure por meio do gerenciamento de [contas de armazenamento](../articles/storage/common/storage-introduction.md) associadas aos discos de VM. Você só precisa especificar o tipo ([Standard HDD](../articles/virtual-machines/windows/standard-storage.md), Standard SSD, ou [Premium SSD](../articles/virtual-machines/windows/premium-storage.md)) e o tamanho do disco que você precisar e o Azure cria e gerencia o disco para você.

## <a name="benefits-of-managed-disks"></a>Benefícios dos discos gerenciados

Vamos conferir alguns dos benefícios que você obtém usando discos gerenciados, começando com este vídeo do Canal 9, [Melhor resiliência de VM do Azure com o Managed Disks](https://channel9.msdn.com/Blogs/Azure/Managed-Disks-for-Azure-Resiliency).
<br/>
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Managed-Disks-for-Azure-Resiliency/player]

### <a name="simple-and-scalable-vm-deployment"></a>Implantação simples e escalonável de VM

O Managed Disks lida com o armazenamento para você nos bastidores. Anteriormente, era necessário criar contas de armazenamento para armazenar os discos (arquivos VHD) para suas VMs do Azure. Ao aumentar, era necessário certificar-se de que você havia criado contas de armazenamento adicionais para não exceder o limite de IOPS do armazenamento com qualquer um de seus discos. Com o Managed Disks lidando com o armazenamento, não há mais os limites de conta de armazenamento (como 20.000 IOPS/conta). Também não é mais necessário copiar as imagens personalizadas (arquivos VHD) em várias contas de armazenamento. Você pode gerenciá-las em um local central, uma conta de armazenamento por região do Azure, e usá-las para criar centenas de VMs em uma assinatura.

O Managed Disks permitirá que você crie até 50.000 **discos** de um tipo em uma assinatura por região, o que lhe permitirá criar milhares de **VMs** em uma única assinatura. Esse recurso também aumenta a escalabilidade dos [Conjuntos de Dimensionamento de Máquinas Virtuais](../articles/virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md), permitindo que você crie até mil VMs em um conjunto de dimensionamento de máquina virtual usando uma imagem do Marketplace. 

### <a name="better-reliability-for-availability-sets"></a>Maior confiabilidade para Conjuntos de disponibilidade

O Managed Disks fornece melhor confiabilidade para os Conjuntos de disponibilidade, garantindo que os discos das [VMs em um Conjunto de disponibilidade](../articles/virtual-machines/windows/manage-availability.md#use-managed-disks-for-vms-in-an-availability-set) estejam suficientemente isolados entre si para evitar pontos de falha. Os discos são automaticamente colocados em unidades de escala de armazenamento diferentes (carimbos). Se um carimbo falhar devido a uma falha de hardware ou de software, somente as instâncias da VM com discos nesses carimbos falharão. Por exemplo, vamos supor que você tenha um aplicativo em execução em cinco VMs, e que as VMs estejam em um Conjunto de Disponibilidade. Os discos dessas VMs não serão armazenados no mesmo stamp, portanto, se um stamp ficar inativo, as outras instâncias do aplicativo continuarão em execução.

### <a name="highly-durable-and-available"></a>Altamente durável e disponível

Os Discos do Azure foram projetados para oferecer uma disponibilidade de 99,999%. Fique tranquilo sabendo que você tem três réplicas dos seus dados, o que proporciona alta durabilidade. Se uma ou duas réplicas apresentarem problemas, as réplicas restantes ajudarão a garantir a persistência dos seus dados e a alta tolerância contra falhas. Esta arquitetura ajudou o Azure a proporcionar durabilidade de nível empresarial de modo consistente para os discos de IaaS, com uma taxa de falha anualizada líder do setor de ZERO POR CENTO. 

### <a name="granular-access-control"></a>Controle de acesso granular

Use o [RBAC (Controle de acesso baseado em função do Azure)](../articles/role-based-access-control/overview.md) para atribuir permissões específicas a um disco gerenciado a um ou mais usuários. O Managed Disks expõe uma variedade de operações, incluindo leitura, gravação (criar/atualizar), exclusão e recuperação de um [URI de assinatura de acesso compartilhado (SAS)](../articles/storage/common/storage-dotnet-shared-access-signature-part-1.md) para o disco. Conceda acesso somente às operações que uma pessoa necessita para executar seu trabalho. Por exemplo, se não quiser que uma pessoa copie um disco gerenciado em uma conta de armazenamento, opte por não conceder acesso à ação de exportação para esse disco gerenciado. Da mesma forma, se não quiser que uma pessoa use um URI de SAS para copiar um disco gerenciado, opte por não conceder essa permissão ao disco gerenciado.

### <a name="azure-backup-service-support"></a>Suporte de serviço do Backup do Azure
Use o serviço de Backup do Azure com o Managed Disks para criar um trabalho de backup com backups baseados em tempo, fácil restauração de VM e políticas de retenção de backup. O Managed Disks dá suporte apenas a Armazenamento com Redundância Local (LRS) de acordo com a opção de replicação. Três cópias dos dados são mantidas dentro de uma única região. Para a recuperação de desastre regional, é necessário fazer backup dos discos da VM em outra região usando o [serviço de Backup do Azure](../articles/backup/backup-introduction-to-azure-backup.md) e uma conta de armazenamento GRS como o cofre de backup. Atualmente o Backup do Azure oferece suporte a todos os tamanhos de disco, incluindo os discos de 4TB. Você precisa [atualização da pilha de backup de VM para V2](../articles/backup/backup-upgrade-to-vm-backup-stack-v2.md) para oferecer suporte a discos de 4 TB. Para obter mais informações, consulte [Usando o serviço de Backup do Azure para VMs com o Managed Disks](../articles/backup/backup-introduction-to-azure-backup.md#using-managed-disk-vms-with-azure-backup).

## <a name="pricing-and-billing"></a>Preços e cobrança

Ao usar o Managed Disks, as seguintes considerações de cobrança se aplicam:
* Tipo de armazenamento

* Tamanho do disco

* Número de transações

* Transferências de dados de saída

* Instantâneos do disco gerenciado (cópia completa do disco)

Vamos examinar essas opções com mais detalhes.

**Tipo de armazenamento:** o Managed Disks oferece três níveis de desempenho: [Standard HDD](../articles/virtual-machines/windows/standard-storage.md), Standard SSD (versão prévia) e [Premium](../articles/virtual-machines/windows/premium-storage.md). A cobrança de um disco gerenciado depende de qual tipo de armazenamento você selecionou para o disco.


**Tamanho do disco**: a cobrança do disco gerenciado depende do tamanho do disco provisionado. O Azure mapeia o tamanho provisionado (arredondado) para a opção mais próxima do Managed Disks, conforme especificado na tabela abaixo. Cada disco gerenciado será mapeado para um dos tamanhos provisionados com suporte e será cobrado adequadamente. Por exemplo, se você criar um disco gerenciado padrão e especificar um tamanho provisionado de 200 GB, será cobrado de acordo com o preço do tipo de disco S15.

Estes são os tamanhos de disco disponíveis para um disco gerenciado premium:

| **Tipo de Premium Managed <br>Disk** | **P4** | **P6** |**P10** | **P15** | **P20** | **P30** | **P40** | **P50** | 
|------------------|---------|---------|---------|---------|---------|----------------|----------------|----------------|  
| Tamanho do disco        | 32 GiB   | 64 GiB   | 128 GiB  | 256 GiB  | 512 GiB  | 1024 GiB (1 TiB) | 2048 GiB (2 TiB) | 4095 GiB (4 TiB) | 

Estes são os tamanhos de disco disponíveis para um disco gerenciado SSD padrão:

| **Standard SSD Managed <br>Tipo de Disco** | **E10** | **E15** | **E20** | **E30** | **E40** | **E50** |
|------------------|--------|--------|--------|----------------|----------------|----------------| 
| Tamanho do disco        | 128 GiB | 256 GiB | 512 GiB | 1024 GiB (1 TiB) | 2048 GiB (2 TiB) | 4095 GiB (4 TiB) | 

Estes são os tamanhos de disco disponíveis para um disco HDD gerenciado padrão:

| **Standard HDD Managed <br>Tipo de Disco** | **S4** | **S6** | **S10** | **S15** | **S20** | **S30** | **S40** | **S50** |
|------------------|---------|---------|--------|--------|--------|----------------|----------------|----------------| 
| Tamanho do disco        | 32 GiB  | 64 GiB  | 128 GiB | 256 GiB | 512 GiB | 1024 GiB (1 TiB) | 2048 GiB (2 TiB) | 4095 GiB (4 TiB) | 


**Número de transações**: você será cobrado pelo número de transações executadas em um disco gerenciado padrão.

Discos SSD padrão usam o tamanho da unidade de e/s de 256KB. Se os dados transferidos forem menores que 256 KB, serão considerados uma unidade de E/S. Tamanhos maiores de e/s são contados como várias entradas e saídas de tamanho de 256 KB. Por exemplo, uma E/S de 1.1000 KB é contada como cinco unidades de E/S.

Não há custo para transações de um disco gerenciado premium.

**Transferências de dados de saída**: as [transferências de dados de saída](https://azure.microsoft.com/pricing/details/data-transfers/) (dados saindo dos datacenters do Azure) incorrem em cobrança por uso de largura de banda.

Para obter informações detalhadas sobre os preços para Managed Disks, confira [Preços do Managed Disks](https://azure.microsoft.com/pricing/details/managed-disks).


## <a name="managed-disk-snapshots"></a>Instantâneos de disco gerenciado

Um Instantâneo Gerenciado é uma cópia completa somente leitura de um disco gerenciado que, por padrão, é armazenado como um disco gerenciado padrão. Com os instantâneos, você pode fazer backup de seus discos gerenciados a qualquer momento. Esses instantâneos existem independentemente do disco de origem e podem ser usados para criar novos Managed Disks. Eles são cobrados com base no tamanho usado. Se você criar um instantâneo de um disco gerenciado com capacidade de aprovisionamento de 64 GiB e tamanho de dados usados atual de 10GiB, o instantâneo será orçado somente para o tamanho de dados usados de 10 GiB.  

[Instantâneos incrementais](../articles/virtual-machines/windows/incremental-snapshots.md) não têm suporte para Discos Gerenciados.

Para saber mais sobre como criar instantâneos com o Managed Disks, consulte os recursos a seguir:

* [Criar cópia de VHD armazenado como um Disco Gerenciado usando instantâneos no Windows](../articles/virtual-machines/windows/snapshot-copy-managed-disk.md)
* [Criar cópia de VHD armazenado como um Disco Gerenciado usando instantâneos no Linux](../articles/virtual-machines/linux/snapshot-copy-managed-disk.md)


## <a name="images"></a>Imagens

O Managed Disks também oferece suporte à criação de uma imagem personalizada gerenciada. Crie uma imagem de seu VHD personalizado em uma conta de armazenamento ou diretamente de uma VM generalizada (por meio do Sysprep). Este processo captura em uma única imagem todos os discos gerenciados associados a uma VM, incluindo o sistema operacional e os discos de dados. Esta imagem personalizada gerenciada permite a criação de centenas de VMs usando sua imagem personalizada, sem a necessidade de copiar ou gerenciar contas de armazenamento.

Para saber mais sobre a criação de imagens, confira os artigos a seguir:
* [Como capturar uma imagem gerenciada de uma VM generalizada no Azure](../articles/virtual-machines/windows/capture-image-resource.md)
* [Como generalizar e capturar uma máquina virtual Linux usando a CLI 2.0 do Azure](../articles/virtual-machines/linux/capture-image.md)

## <a name="images-versus-snapshots"></a>Imagens versus instantâneos

É comum ver a palavra "imagem" usada com VMs e, agora, você também vê a palavra "instantâneos". É importante compreender a diferença entre esses termos. Com o Managed Disks, você pode capturar uma imagem de uma VM generalizada que foi desalocada. Esta imagem incluirá todos os discos anexados à VM. Use essa imagem para criar uma nova VM e ela incluirá todos os discos.

Um instantâneo é uma cópia de um disco no momento exato em que é tirado. Ele só se aplica a um disco. Se você tiver uma VM com apenas um disco (o SO), será possível tirar um instantâneo ou uma imagem dele e criar uma VM a partir do instantâneo ou da imagem.

E se uma VM tiver cinco discos e eles estiverem distribuídos? Você pode tirar um instantâneo de cada um dos discos, mas não há reconhecimento dentro da VM do estado dos discos – os instantâneos dizem respeito apenas àquele disco. Nesse caso, os instantâneos precisariam ser coordenados entre si, e não há suporte para isso no momento.

## <a name="managed-disks-and-encryption"></a>Managed Disks e criptografia

Há dois tipos de criptografia a serem abordados em referência a discos gerenciados. O primeiro é o SSE (Criptografia do Serviço de Armazenamento), que é executado pelo serviço de armazenamento. O segundo é o Azure Disk Encryption, que pode ser habilitado nos discos do sistema operacional e de dados das VMs.

### <a name="storage-service-encryption-sse"></a>SSE (Criptografia do Serviço de Armazenamento)

A [Criptografia do serviço de armazenamento do Azure](../articles/storage/common/storage-service-encryption.md) fornece criptografia em repouso e proteger seus dados para atender aos compromissos de conformidade e segurança da organização. O SSE está habilitado por padrão para todos o Managed Disks, Instantâneos e Imagens em todas as regiões nas quais o discos gerenciados está disponível. Começando em 10 de junho de 2017, todos os novos discos gerenciados/instantâneos/imagens e novos dados gravados em discos gerenciados existentes são automaticamente criptografados em repouso com chaves gerenciadas pela Microsoft, por padrão. Visite o [página de Perguntas frequentes do Managed Disks](../articles/virtual-machines/windows/faq-for-disks.md#managed-disks-and-storage-service-encryption) para obter mais detalhes.


### <a name="azure-disk-encryption-ade"></a>ADE (Azure Disk Encryption)

O Azure Disk Encryption permite criptografar os discos do sistema operacional e os discos de dados usados por uma Máquina Virtual IaaS. Essa criptografia inclui discos gerenciados. No Windows, as unidades são criptografadas usando a tecnologia de criptografia BitLocker padrão do setor. No Linux, os discos são criptografados usando a tecnologia DM-Crypt. Esse processo de criptografia é integrado ao Azure Key Vault para permitir que você controle e gerencie as chaves de criptografia de disco. Para saber mais, confira [Azure Disk Encryption para Windows e VMs IaaS Linux](../articles/security/azure-security-disk-encryption.md).

## <a name="next-steps"></a>Próximas etapas

Para saber mais sobre o Managed Disks, confira os artigos a seguir.

### <a name="get-started-with-managed-disks"></a>Introdução aos Discos Gerenciados

* [Criar uma VM do Windows usando o Gerenciador de Recursos e o PowerShell](../articles/virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm.md)

* [Criar uma VM Linux usando a CLI 2.0 do Azure](../articles/virtual-machines/linux/quick-create-cli.md)

* [Anexar um disco de dados gerenciado a uma VM do Windows usando o PowerShell](../articles/virtual-machines/windows/attach-disk-ps.md)

* [Adicionar um disco gerenciado a uma VM Linux](../articles/virtual-machines/linux/add-disk.md)

* [Scripts de exemplo do PowerShell de discos gerenciados](https://github.com/Azure-Samples/managed-disks-powershell-getting-started)

* [Utilizar o Managed Disks nos modelos do Azure Resource Manager](../articles/virtual-machines/windows/using-managed-disks-template-deployments.md)

### <a name="compare-managed-disks-storage-options"></a>Comparar as opções de armazenamento de Managed Disks

* [Discos SSD Premium](../articles/virtual-machines/windows/premium-storage.md)

* [Discos padrão SSD e HDD](../articles/virtual-machines/windows/standard-storage.md)

### <a name="operational-guidance"></a>Diretrizes operacionais

* [Migração do AWS e outras plataformas para os Azure Managed Disks](../articles/virtual-machines/windows/on-prem-to-azure.md)

* [Converter VMs do Azure para discos gerenciados no Azure](../articles/virtual-machines/windows/migrate-to-managed-disks.md)
