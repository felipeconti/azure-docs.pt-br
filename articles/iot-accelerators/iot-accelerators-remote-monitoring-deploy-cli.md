---
title: Implantar a solução Java de Monitoramento Remoto – Azure | Microsoft Docs
description: Este tutorial mostra como provisionar o acelerador de solução de Monitoramento Remoto usando a CLI.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 01/29/2018
ms.topic: conceptual
ms.openlocfilehash: dd696330c9ee78ef84ac9fcf85946c837ad5b824
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2018
ms.locfileid: "39187991"
---
# <a name="deploy-the-remote-monitoring-solution-accelerator-using-the-cli"></a>Implantar o acelerador de solução de Monitoramento Remoto usando a CLI

Este tutorial mostra como provisionar o acelerador da solução de Monitoramento Remoto. Implante a solução usando a CLI. Também é possível implantar a solução usando a interface do usuário na Web em azureiotsuite.com. Para saber mais sobre essa opção, confira [Implantar o acelerador de solução de Monitoramento Remoto](quickstart-remote-monitoring-deploy.md).

## <a name="prerequisites"></a>Pré-requisitos

Para implantar o acelerador de solução de Monitoramento Remoto, você precisará de uma assinatura ativa do Azure.

Se você não tiver uma conta, poderá criar uma conta de avaliação gratuita em apenas alguns minutos. Para obter detalhes, consulte [Avaliação gratuita do Azure](http://azure.microsoft.com/pricing/free-trial/).

Para executar a CLI, você precisará do [Node.js](https://nodejs.org/) instalado no seu computador local.

## <a name="install-the-cli"></a>Instalar a CLI

Para instalar a CLI, execute o seguinte comando no seu ambiente de linha de comando:

```cmd/sh
npm install iot-solutions -g
```

## <a name="sign-in-to-the-cli"></a>Entrar na CLI

Antes de implantar o acelerador de solução, você deve entrar na sua assinatura do Azure usando a CLI como mostrado a seguir:

```cmd/sh
pcs login
```

Siga as instruções na tela para concluir o processo de entrada.

## <a name="deployment-options"></a>Opções de implantação

Há várias opções para configurar o processo de implantação ao implantar o acelerador de solução:

| Opção | Valores | DESCRIÇÃO |
| ------ | ------ | ----------- |
| SKU    | `basic`, `standard`, `local` | Uma implantação _básica_ destina-se a testes e demonstrações, e implanta todos os microsserviços em uma única máquina virtual. Uma implantação _padrão_ destina-se à produção e implanta os microsserviços em diversas máquinas virtuais. Uma implantação _local_ configura um contêiner do Docker para executar o microservices em seu computador local e usa os serviços do Azure, como armazenamento e o Cosmos DB, na nuvem. |
| Tempo de execução | `dotnet`, `java` | Seleciona a implementação de linguagem dos microsserviços. |

Para saber mais sobre como usar a implantação local, confira [Executar a solução de Monitoramento Remota localmente](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Running-the-Remote-Monitoring-Solution-Locally#deploy-azure-services-and-set-environment-variables).

## <a name="basic-vs-standard-deployments"></a>Implantações Basic vs. Standard

### <a name="basic"></a>Basic
Implantação Basic é voltada para a apresentação da solução. Para reduzir o custo desta demonstração, todos os microsserviços são implantados em uma única máquina virtual. Isso não é considerado uma arquitetura pronta para produção.

Nossa opção de implantação Standard deve ser usada quando você estiver pronto para personalizar uma arquitetura pronta para produção, criada ser de grande escala e ter extensibilidade.

Criar uma solução Basic resultará no provisionamento dos seguintes serviços do Azure em sua assinatura do Azure, com custo: 

| Contagem | Recurso                       | Tipo         | Usadas para |
|-------|--------------------------------|--------------|----------|
| 1     | [Máquina Virtual Linux](https://azure.microsoft.com/services/virtual-machines/) | Standard D1 V2  | Hospedar microsserviços |
| 1     | [Hub IoT do Azure](https://azure.microsoft.com/services/iot-hub/)                  | S1 – Camada Padrão | Comunicação e gerenciamento de dispositivo |
| 1     | [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/)              | Standard        | Armazenar dados de configuração e telemetria do dispositivo como regras, alarmes e mensagens |  
| 1     | [Conta de Armazenamento do Azure](https://docs.microsoft.com/azure/storage/common/storage-introduction#types-of-storage-accounts)  | Standard        | Armazenamento de VM e pontos de verificação de streaming |
| 1     | [Aplicativo Web](https://azure.microsoft.com/services/app-service/web/)        |                 | Hospedar o aplicativo Web front-end |

### <a name="standard"></a>Standard
A implantação Standard é uma implantação pronta para produção que um desenvolvedor pode personalizar e estender para atender às suas necessidades. Para confiabilidade e escala, os microsserviços de aplicativo são compilados como contêineres do Docker e implantados usando um orquestrador ([Kubernetes](https://kubernetes.io/) por padrão). O orquestrador é responsável pela implantação, colocação em escala e gerenciamento do aplicativo.

Criar uma solução Standard resultará no provisionamento dos seguintes serviços do Azure em sua assinatura do Azure, com custo:

| Contagem | Recurso                                     | SKU / Tamanho      | Usadas para |
|-------|----------------------------------------------|-----------------|----------|
| 4     | [Máquinas Virtuais do Linux](https://azure.microsoft.com/services/virtual-machines/)   | Standard D2 V2  | 1 mestre e 3 agentes para hospedar microsserviços com redundância |
| 1     | [Serviço de Contêiner do Azure](https://azure.microsoft.com/services/container-service/) |                 | Orquestrador do [Kubernetes](https://kubernetes.io) |
| 1     | [Hub IoT do Azure][https://azure.microsoft.com/services/iot-hub/]                     | S2 – Camada Padrão | Controle, comando e gerenciamento de dispositivos |
| 1     | [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/)                 | Standard        | Armazenar dados de configuração e telemetria do dispositivo como regras, alarmes e mensagens |
| 5     | [Contas de Armazenamento do Azure](https://docs.microsoft.com/azure/storage/common/storage-introduction#types-of-storage-accounts)    | Standard        | 4 para armazenamento de máquina virtual e 1 para os pontos de verificação de streaming |
| 1     | [Serviço de Aplicativo](https://azure.microsoft.com/services/app-service/web/)             | S1 Standard     | Gateway de aplicativo sobre SSL |

> Informações sobre preços para esses serviços podem ser encontradas [aqui](https://azure.microsoft.com/pricing). Quantidade de uso e detalhes de cobrança para sua assinatura podem ser encontrados no [Portal do Azure](https://portal.azure.com/).

## <a name="deploy-the-solution-accelerator"></a>Implantar o acelerador de solução

### <a name="example-deploy-net-version"></a>Exemplo: implantar a versão .NET

O exemplo a seguir mostra como implantar a versão .NET básica do acelerador de solução de Monitoramento Remoto:

```cmd/sh
pcs -t remotemonitoring -s basic -r dotnet
```

### <a name="example-deploy-java-version"></a>Exemplo: implantar a versão Java

O exemplo a seguir mostra como implantar a versão Standard em Java do acelerador de solução de Monitoramento Remoto:

```cmd/sh
pcs -t remotemonitoring -s standard -r java
```

### <a name="pcs-command-options"></a>Opções de comando pcs

Quando você executa o comando `pcs` para implantar uma solução, será solicitado que você informe:

- Um nome para a solução. Esse nome deve ser exclusivo.
- A assinatura do Azure a utilizar.
- Um local.
- Credenciais para as máquinas virtuais que hospedam o microsserviços. Você pode usar essas credenciais para acessar as máquinas virtuais para solução de problemas.

Quando o comando `pcs` for concluído, ele exibirá a URL da sua nova implantação do acelerador de solução. O comando `pcs` também cria um arquivo `{deployment-name}-output.json` com informações adicionais, como o nome do Hub IoT provisionado para você.

Para saber mais sobre os parâmetros de linha de comando, execute:

```cmd/sh
pcs -h
```

Para obter mais informações sobre a CLI, consulte [Como usar a CLI](https://github.com/Azure/pcs-cli/blob/master/README.md).

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você aprendeu como:

> [!div class="checklist"]
> * Configurar o acelerador de solução
> * Implantar o acelerador de solução
> * Entrar no acelerador de solução

Agora que você implantou a solução de Monitoramento Remoto, a próxima etapa será [explorar os recursos do painel da solução](./quickstart-remote-monitoring-deploy.md).

<!-- Next tutorials in the sequence -->
