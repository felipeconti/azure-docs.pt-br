---
title: Comparação do Azure Data Lake Store com o Armazenamento de Blobs do Azure | Microsoft Docs
description: Comparação do Azure Data Lake Store com o Armazenamento de Blobs do Azure
services: data-lake-store
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: b199525b-84de-4f79-9eb6-69a613b8b217
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 03/26/2018
ms.author: nitinme
ms.openlocfilehash: 0b374e92a1e1d9828bc8c095e29e1dfdfd13275b
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/03/2018
ms.locfileid: "39492904"
---
# <a name="comparing-azure-data-lake-store-and-azure-blob-storage"></a>Comparar o Azure Data Lake Store e o Armazenamento de Blobs do Azure
A tabela neste artigo resume as diferenças entre o Azure Data Lake Store e o Armazenamento de Blobs do Azure em alguns aspectos fundamentais do processamento de big data. O Armazenamento de Blobs do Azure é um repositório de objetos escalonável de finalidade geral que foi criado para uma ampla variedade de cenários de armazenamento. O Azure Data Lake Store é um repositório de grande escala otimizado para cargas de trabalho de análise de big data.

|  | Repositório Azure Data Lake | Armazenamento do Blobs do Azure |
| --- | --- | --- |
| Finalidade |Armazenamento otimizado para cargas de trabalho de análise de big data |Repositório de objeto de uso geral para uma grande variedade de cenários de armazenamento, incluindo análise big data |
| Casos de uso |Análise de streaming interativa em lotes e dados de aprendizado de máquina, como arquivos de log, dados da IoT, fluxos de cliques, grandes conjuntos de dados |Qualquer tipo de dados de texto ou binários, como back-end de aplicativo, dados de backup, armazenamento de mídia para streaming e dados de uso geral. Adicionalmente, suporte total para cargas de trabalho de análise; lote, interativo, análises de streaming e dados do Azure Machine Learning, como arquivos de log, dados IoT, fluxos de cliques, grandes conjuntos de dados |
| Principais Conceitos |A conta do Data Lake Store contém pastas, que, por sua vez, contêm dados armazenados como arquivos |A conta de armazenamento tem contêineres, que, por sua vez, têm dados na forma de blobs |
| Estrutura |Sistema de arquivos hierárquico |Repositório de objetos com namespace simples |
| API |API REST sobre HTTPS |API REST sobre HTTP/HTTPS |
| API no servidor |[API REST compatível com WebHDFS](https://msdn.microsoft.com/library/azure/mt693424.aspx) |[API REST do Armazenamento de Blobs do Azure](https://msdn.microsoft.com/library/azure/dd135733.aspx) |
| Cliente do sistema de arquivos Hadoop |SIM |SIM |
| Operações de dados – autenticação |Com base em [Identidades do Azure Active Directory](../active-directory/develop/authentication-scenarios.md) |Com base em segredos compartilhados – [Chaves de Acesso de Conta](../storage/common/storage-create-storage-account.md#manage-your-storage-account) e [Chaves de Assinatura de Acesso Compartilhado](../storage/common/storage-dotnet-shared-access-signature-part-1.md). |
| Operações de dados – protocolo de autenticação |OAuth 2.0. As chamadas devem conter um JWT (Token Web JSON) válido emitido pelo Azure Active Directory |HMAC (Código de Autenticação de Mensagem Baseado em Hash). As chamadas devem conter um hash SHA-256 codificado na Base64 em uma parte da solicitação HTTP. |
| Operações de dados – autorização |ACLs (Listas de Controle de Acesso) POSIX.  ACLs baseadas em identidades do Azure Active Directory podem ser definidas no arquivo e nível de pasta. |Para autorização no nível de conta — use [Chaves de Acesso da Conta](../storage/common/storage-create-storage-account.md#manage-your-storage-account)<br>Para autorização de conta, contêiner ou blob — use [Chaves de Assinatura de Acesso Compartilhado](../storage/common/storage-dotnet-shared-access-signature-part-1.md) |
| Operações de dados – auditoria |Disponível. Veja mais informações [aqui](data-lake-store-diagnostic-logs.md) . |Disponível |
| Dados de criptografia em repouso |<ul><li>Transparente, lado do servidor</li> <ul><li>Com chaves gerenciadas pelo serviço</li><li>Com chaves gerenciados pelo cliente no Cofre de Chaves do Azure</li></ul></ul> |<ul><li>Transparente, lado do servidor</li> <ul><li>Com chaves gerenciadas pelo serviço</li><li>Com chaves gerenciadas pelo cliente no Azure KeyVault (versão prévia)</li></ul><li>Criptografia do cliente</li></ul> |
| Operações de gerenciamento (por exemplo, Criar Conta) |[Controle de acesso baseado em função](../role-based-access-control/overview.md) ) fornecido pelo Azure para gerenciamento de contas |[Controle de acesso baseado em função](../role-based-access-control/overview.md) ) fornecido pelo Azure para gerenciamento de contas |
| SDKs de desenvolvedor |.NET, Java, Python, Node.js |.Net, Java, Python, Node.js, C++, Ruby, PHP, Go, Android, iOS |
| Desempenho de Carga de Trabalho de Análise |Desempenho otimizado para cargas de trabalho de análise paralela. Alta taxa de transferência e IOPS. |Desempenho otimizado para cargas de trabalho de análise paralela. |
| Limites de tamanho |Sem limites para tamanhos de conta, tamanhos de arquivo ou número de arquivos |Limites específicos documentados [aqui](../storage/common/storage-scalability-targets.md). Limites de contas maiores disponíveis contactando [Suporte do Azure](https://azure.microsoft.com/support/faq/) |
| Redundância geográfica |Com redundância local (várias cópias dos dados em uma região do Azure) |LRS (redundância local), ZRS (redundância de zona), GRS (redundância global), RA-GRS (redundância global com acesso de leitura). Veja mais informações [aqui](../storage/common/storage-redundancy.md) |
| Estado do serviço |Disponível para o público geral |Disponível para o público geral |
| Disponibilidade regional |Veja [aqui](https://azure.microsoft.com/regions/#services) |Disponível em todas as regiões do Azure |
| Preço |Veja [preços](https://azure.microsoft.com/pricing/details/data-lake-store/) |Veja [preços](https://azure.microsoft.com/pricing/details/storage/) |


