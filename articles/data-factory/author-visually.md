---
title: Criação visual no Azure Data Factory | Microsoft Docs
description: Saiba como usar a criação visual no Azure Data Factory
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/14/2018
ms.author: shlo
ms.openlocfilehash: b457d1ae01e523ac99c6171fa8d2123023ebcd2c
ms.sourcegitcommit: 4ea0cea46d8b607acd7d128e1fd4a23454aa43ee
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/15/2018
ms.locfileid: "42142223"
---
# <a name="visual-authoring-in-azure-data-factory"></a>Criação visual no Azure Data Factory
A experiência (UX) da interface do usuário do Azure Data Factory permite criar e implantar visualmente recursos para seu data factory sem ter que gravar nenhum código. Você pode arrastar atividades para uma tela de pipeline, realizar execuções de teste, depurar iterativamente e implantar e monitorar as execuções do pipeline. Há duas abordagens para usar a UX para executar a criação visual:

- Criar diretamente com o serviço de Data Factory.
- Criar com a integração do Git do VSTS (Visual Studio Team Services) para colaboração, controle do código-fonte e controle de versão.

## <a name="author-directly-with-the-data-factory-service"></a>Criar diretamente com o serviço de Data Factory
A criação visual com o serviço de Data Factory difere da criação visual de com VSTS de duas maneiras:

- O serviço de Data Factory não inclui um repositório para armazenar as entidades JSON para as alterações.
- O serviço de Data Factory não está otimizado para colaboração ou controle de versão.

![Configurar o serviço do Data Factory ](media/author-visually/configure-data-factory.png)

Quando você usa a **Tela de criação** UX para criar diretamente com o serviço do Data Factory, somente o modo **Publica Tudo** estará disponível. As alterações feitas são publicadas diretamente no serviço do Data Factory.

![Modo Publicar](media/author-visually/data-factory-publish.png)

## <a name="author-with-vsts-git-integration"></a>Criar com a integração do Git do VSTS
A criação visual com a integração do Git do VSTS oferece suporte ao controle do código-fonte e à colaboração para trabalhar em seus pipelines de data factory. Você pode associar um data factory com um repositório de conta do Git do VSTS para controle do código-fonte, colaboração, controle de versão e assim por diante. Uma única conta do Git do VSTS pode ter vários repositórios, mas um repositório do Git do VSTS pode ser associado somente a um data factory. Se não tiver uma conta ou repositório do VSTS, siga [estas instruções](https://docs.microsoft.com/vsts/accounts/create-account-msa-or-work-student) para criar seus recursos.

> [!NOTE]
> Você pode armazenar arquivos de dados e scripts em um repositório Git do VSTS. No entanto, você precisa carregar os arquivos manualmente para o Armazenamento do Azure. Um pipeline do Data Factory não carrega automaticamente os arquivos de dados ou scripts armazenados em um repositório Git do VSTS para o Armazenamento do Azure.

### <a name="configure-a-vsts-git-repository-with-azure-data-factory"></a>Configurar o repositório do Git do VSTS com o Azure Data Factory
Você pode configurar um repositório do Git do VSTS com um data factory por meio de dois métodos.

#### <a name="method1"></a> Método de configuração 1 (repositório Git do VSTS): página Vamos começar

No Azure Data Factory, vá para a página **Vamos começar**. Selecione **Configurar Repositório de Código**:

![Configurar um repositório de código de VSTS](media/author-visually/configure-repo.png)

O painel de configuração **Definições do repositório** é exibido:

![Configurar as definições do repositório de código](media/author-visually/repo-settings.png)

O painel mostra as definições do repositório de código do VSTS a seguir:

| Configuração | DESCRIÇÃO | Valor |
|:--- |:--- |:--- |
| **Tipo de repositório** | O tipo de repositório de código do VSTS.<br/>**Observação**: atualmente, não há suporte para o GitHub. | Git do Visual Studio Team Services |
| **Azure Active Directory** | Seu nome de locatário do Microsoft Azure AD. | <your tenant name> |
| **Conta do Visual Studio Team Services** | O novo nome da conta do VSTS. Você pode localizar o nome da conta do VSTS em `https://{account name}.visualstudio.com`. Você pode [entrar em sua conta do VSTS](https://www.visualstudio.com/team-services/git/) para acessar seu perfil do Visual Studio e ver seus repositórios e projetos. | <your account name> |
| **ProjectName** | O nome do projeto VSTS. Você pode localizar o nome do projeto VSTS em `https://{account name}.visualstudio.com/{project name}`. | <your VSTS project name> |
| **RepositoryName** | O nome do repositório de código do VSTS. Os projetos do VSTS contêm repositórios Git para gerenciar seu código-fonte, à medida que o projeto se expande. Você pode criar um novo repositório ou usar um existente que já esteja no projeto. | <your VSTS code repository name> |
| **Ramificação de colaboração** | Sua ramificação de colaboração do VSTS usada para publicação. Por padrão, é `master`. Altere essa configuração se você desejar publicar recursos de outra ramificação. | <your collaboration branch name> |
| **Pasta raiz** | Sua pasta raiz em sua ramificação de colaboração VSTS. | <your root folder name> |
| **Importar recursos existentes do Data Factory para o repositório** | Especifica se deve-se importar recursos do data factory existentes da UX **Tela de criação** em um repositório do Git do VSTS. Selecione a caixa para importar os recursos do data factory para o repositório do Git associado no formato JSON. Esta ação exporta cada recurso individualmente (ou seja, os serviços vinculados e conjuntos de dados são exportados para JSONs separados). Quando essa caixa não está selecionada, os recursos existentes não são importados. | Selecionada (padrão) |

#### <a name="configuration-method-2--vsts-git-repo-ux-authoring-canvas"></a>Método de configuração 2 (repositório Git do VSTS): tela de criação da UX
Na UX **Tela de criação** do Azure Data Factory, localize seu data factory. Selecione o menu suspenso **Data Factory** e selecione **Configurar repositório de código**.

Um painel de configuração é exibido. Para obter detalhes sobre as definições de configuração, consulte as descrições no <a href="#method1">Método de configuração 1</a>.

![Configurar as definições do repositório de código para a criação de UX](media/author-visually/configure-repo-2.png)

## <a name="use-a-different-azure-active-directory-tenant"></a>Usar um locatário do Azure Active Directory diferente

Você pode criar um repositório Git do VSTS em um locatário do Azure Active Directory diferente. Para especificar outro locatário do Azure AD, você precisa ter permissões de administrador para a assinatura do Azure que está usando.

## <a name="switch-to-a-different-git-repo"></a>Alternar para um repositório Git diferente

Para alternar para um repositório Git diferente, localize o ícone no canto superior direito da página de visão geral da Data Factory, conforme mostrado na seguinte captura de tela. Se você não vir o ícone, limpe o cache do navegador local. Selecione o ícone para remover a associação com o repositório atual.

Depois de remover a associação com o repositório atual, você pode configurar as configurações de Git para usar um repositório diferente. Em seguida, você pode importar recursos da Data Factory existentes para o novo repositório.

![Remover a associação com o repositório Git atual](media/author-visually/remove-repo.png)

## <a name="use-version-control"></a>Usar controle de versão
Os sistemas de controle de versão (também conhecidos como _controle do código-fonte_) permitem aos desenvolvedores colaborar em código e acompanhar as alterações feitas no código base. O controle do código-fonte é uma ferramenta essencial para projetos de vários desenvolvedores.

Cada repositório Git do VSTS associado a um data factory tem uma ramificação de colaboração. (`master` é a ramificação de colaboração padrão). Os usuários criam ramificações do recurso clicando **+ Nova ramificação** e fazem o desenvolvimento nas ramificações do recurso.

![Alterar o código sincronizando ou publicando](media/author-visually/sync-publish.png)

Quando estiver pronto com o desenvolvimento de recurso em sua ramificação de recurso, clique em **Criar solicitação pull**. Essa ação o levará para Git do VSTS, onde você pode gerar solicitações pull, revisões de código e mesclar alterações para a ramificação de colaboração. (`master` é o padrão). Você só tem permissão para publicar no serviço do Data Factory de sua ramificação de colaboração. 

![Criar uma nova solicitação pull](media/author-visually/create-pull-request.png)

## <a name="publish-code-changes"></a>Publicar alterações de código
Depois de ter mesclado alterações para a ramificação de colaboração (`master` é o padrão), selecione **Publicar** para publicar manualmente as alterações de código na ramificação mestre para o serviço do Data Factory.

![Publicar as alterações no serviço do Data Factory](media/author-visually/publish-changes.png)

> [!IMPORTANT]
> O branch mestre não é representativo do que é implantado no serviço de Data Factory. O branch mestre *deve* ser publicado manualmente no serviço de Data Factory.

## <a name="author-with-github-integration"></a>Criar com a integração do GitHub

A criação visual com a integração do GitHub oferece suporte ao controle do código-fonte e à colaboração para trabalhar em seus pipelines de data factory. Você pode associar um data factory com um repositório de conta do GitHub para controle do código-fonte, colaboração e controle de versão. Uma única conta do GitHub pode ter vários repositórios, mas um repositório do GitHub pode ser associado somente a um data factory. Se não tiver uma conta ou repositório do GitHub, siga [estas instruções](https://github.com/join) para criar seus recursos. A integração do GitHub com o Data Factory dá suporte ao GitHub público e ao GitHub Enterprise.

Para configurar um repositório GitHub, você precisa ter permissões de administrador para a assinatura do Azure que está usando.

Para ver uma introdução de nove minutos e uma demonstração desse recurso, assista ao seguinte vídeo:

> [!VIDEO https://channel9.msdn.com/shows/azure-friday/Azure-Data-Factory-visual-tools-now-integrated-with-GitHub/player]

### <a name="limitations"></a>Limitações

- Você pode armazenar arquivos de dados e scripts em um repositório GitHub. No entanto, você precisa carregar os arquivos manualmente para o Armazenamento do Azure. Um pipeline do Data Factory não carrega automaticamente os arquivos de dados ou scripts armazenados em um repositório GitHub para o Armazenamento do Azure.

- O GitHub Enterprise com uma versão mais antiga que 2.14.0 não funciona no navegador Microsoft Edge.

- A integração do GitHub com as ferramentas de criação visual do Data Factory funciona apenas na versão geralmente disponível do Data Factory.

### <a name="configure-a-public-github-repository-with-azure-data-factory"></a>Configurar um repositório do GitHub público com o Azure Data Factory

Você pode configurar um repositório do GitHub com um data factory por meio de dois métodos.

**Método de configuração 1 (repositório público): página Vamos começar**

No Azure Data Factory, vá para a página **Vamos começar**. Selecione **Configurar Repositório de Código**:

![Página Introdução ao Data Factory](media/author-visually/github-integration-image1.png)

O painel de configuração **Definições do repositório** é exibido:

![Configurações do repositório do GitHub](media/author-visually/github-integration-image2.png)

O painel mostra as definições do repositório de código do VSTS a seguir:

| **Configuração**                                              | **Descrição**                                                                                                                                                                                                                                                                                                                                                                                                                   | **Valor**          |
|----------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------|
| **Tipo de repositório**                                      | O tipo de repositório de código do VSTS.                                                                                                                                                                                                                                                                                                                                                                                             | GitHub             |
| **Conta do GitHub**                                       | Seu nome de conta do GitHub. Esse nome pode ser encontrado em https://github.com/{account nome}/{nome do repositório}. Navegar até essa página solicita que você insira as credenciais do GitHub OAuth para sua conta do GitHub.                                                                                                                                                                                                                                               |                    |
| **RepositoryName**                                       | O nome do repositório de código do GitHub. As contas do GitHub contêm repositórios Git para gerenciar seu código-fonte. Você pode criar um novo repositório ou usar um existente que já esteja na conta.                                                                                                                                                                                                                              |                    |
| **Ramificação de colaboração**                                 | Sua ramificação de colaboração do GitHub usada para publicação. Por padrão, é mestre. Altere essa configuração se você desejar publicar recursos de outra ramificação.                                                                                                                                                                                                                                                               |                    |
| **Pasta raiz**                                          | Sua pasta raiz em sua ramificação de colaboração GitHub.                                                                                                                                                                                                                                                                                                                                                                             |                    |
| **Importar recursos existentes do Data Factory para o repositório** | Especifica se deve-se importar recursos do data factory existentes da UX **Tela de criação** em um repositório do GitHub. Selecione a caixa para importar os recursos do data factory para o repositório do Git associado no formato JSON. Esta ação exporta cada recurso individualmente (ou seja, os serviços vinculados e conjuntos de dados são exportados para JSONs separados). Quando essa caixa não está selecionada, os recursos existentes não são importados. | Selecionada (padrão) |
| **Branch para importar o recurso**                       | Especifica em qual branch os recursos do data factory (pipelines, conjuntos de dados, serviços vinculados etc.) serão importados. Você pode importar recursos para um dos seguintes branches: a. Colaboração b. Criar novo c. Usar Existente                                                                                                                                                                                                     |                    |

#### <a name="configuration-method-2-public-repo-ux-authoring-canvas"></a>Método de configuração 2 (repositório público): tela de criação da UX

Na UX **Tela de criação** do Azure Data Factory, localize seu data factory. Selecione o menu suspenso **Data Factory** e selecione **Configurar repositório de código**.

Um painel de configuração é exibido. Para obter detalhes sobre as definições de configuração, consulte as descrições no *Método de configuração 1* acima.

### <a name="configure-a-github-enterprise-repository-with-azure-data-factory"></a>Configurar um repositório do GitHub Enterprise com o Azure Data Factory

Você pode configurar um repositório do GitHub Enterprise com um data factory por meio de dois métodos.

 #### <a name="configuration-method-1-enterprise-repo-lets-get-started-page"></a>Método de configuração 1 (repositório Enterprise): página Vamos começar

No Azure Data Factory, vá para a página **Vamos começar**. Selecione **Configurar Repositório de Código**:

![Página Introdução ao Data Factory](media/author-visually/github-integration-image1.png)

O painel de configuração **Definições do repositório** é exibido:

![Configurações do repositório do GitHub](media/author-visually/github-integration-image3.png)

O painel mostra as definições do repositório de código do VSTS a seguir:

| **Configuração**                                              | **Descrição**                                                                                                                                                                                                                                                                                                                                                                                                                   | **Valor**          |
|----------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------|
| **Tipo de repositório**                                      | O tipo de repositório de código do VSTS.                                                                                                                                                                                                                                                                                                                                                                                             | GitHub             |
| **Usar GitHub Enterprise**                                | Caixa de seleção para selecionar o GitHub Enterprise                                                                                                                                                                                                                                                                                                                                                                                              |                    |
| **URL do GitHub Enterprise**                                | A URL raiz do GitHub Enterprise. Por exemplo: https://github.mydomain.com                                                                                                                                                                                                                                                                                                                                                          |                    |
| **Conta do GitHub**                                       | Seu nome de conta do GitHub. Esse nome pode ser encontrado em https://github.com/{account nome}/{nome do repositório}. Navegar até essa página solicita que você insira as credenciais do GitHub OAuth para sua conta do GitHub.                                                                                                                                                                                                                                               |                    |
| **RepositoryName**                                       | O nome do repositório de código do GitHub. As contas do GitHub contêm repositórios Git para gerenciar seu código-fonte. Você pode criar um novo repositório ou usar um existente que já esteja na conta.                                                                                                                                                                                                                              |                    |
| **Ramificação de colaboração**                                 | Sua ramificação de colaboração do GitHub usada para publicação. Por padrão, é mestre. Altere essa configuração se você desejar publicar recursos de outra ramificação.                                                                                                                                                                                                                                                               |                    |
| **Pasta raiz**                                          | Sua pasta raiz em sua ramificação de colaboração GitHub.                                                                                                                                                                                                                                                                                                                                                                             |                    |
| **Importar recursos existentes do Data Factory para o repositório** | Especifica se deve-se importar recursos do data factory existentes da UX **Tela de criação** em um repositório do GitHub. Selecione a caixa para importar os recursos do data factory para o repositório do Git associado no formato JSON. Esta ação exporta cada recurso individualmente (ou seja, os serviços vinculados e conjuntos de dados são exportados para JSONs separados). Quando essa caixa não está selecionada, os recursos existentes não são importados. | Selecionada (padrão) |
| **Branch para importar o recurso**                       | Especifica em qual branch os recursos do data factory (pipelines, conjuntos de dados, serviços vinculados etc.) serão importados. Você pode importar recursos para um dos seguintes branches: a. Colaboração b. Criar novo c. Usar Existente                                                                                                                                                                                                     |                    |

#### <a name="configuration-method-2-enterprise-repo-ux-authoring-canvas"></a>Método de configuração 2 (repositório Enterprise): tela de criação da UX

Na UX **Tela de criação** do Azure Data Factory, localize seu data factory. Selecione o menu suspenso **Data Factory** e selecione **Configurar repositório de código**.

Um painel de configuração é exibido. Para obter detalhes sobre as definições de configuração, consulte as descrições no *Método de configuração 1* acima.

## <a name="use-the-expression-language"></a>Usar linguagem de expressão
Você pode especificar expressões para valores de propriedade usando a linguagem de expressão com suporte do Azure Data Factory.

Especifique as expressões para valores de propriedade selecionando **Adicionar Conteúdo Dinâmico**:

![Usar linguagem de expressão](media/author-visually/dynamic-content-1.png)

## <a name="use-functions-and-parameters"></a>Usar funções e parâmetros

Você pode usar funções ou especificar parâmetros para pipelines e conjunto de dados no **construtor de expressões** do Data Factory:

Para obter informações sobre as expressões compatíveis, consulte [Expressões e funções no Azure Data Factory](control-flow-expression-language-functions.md).

![Adicionar conteúdo dinâmico](media/author-visually/dynamic-content-2.png)

## <a name="provide-feedback"></a>Fornecer comentários
Selecione **Feedback** para comentar sobre os recursos ou notificar a Microsoft sobre problemas com a ferramenta:

![Comentários](media/author-visually/provide-feedback.png)

## <a name="next-steps"></a>Próximas etapas
Para saber mais sobre o monitoramento e o gerenciamento de pipelines, c [Monitorar e gerenciar os pipelines programaticamente](monitor-programmatically.md).
