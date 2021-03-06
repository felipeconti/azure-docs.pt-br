---
title: Conectar ao Dropbox – Aplicativo Lógico do Azure | Microsoft Docs
description: Carregar e gerenciar arquivos com as APIs REST do Dropbox e o Aplicativo Lógico do Azure
author: ecfan
manager: jeconnoc
ms.author: estfan
ms.date: 07/15/2016
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: 256a0b34d5050e17abe5bb98ca0c13ab0b61787e
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43094431"
---
# <a name="get-started-with-the-dropbox-connector"></a>Introdução ao conector do Dropbox
Conecte-se ao Dropbox para gerenciar seus arquivos. Você pode executar várias ações, como carregar, atualizar, obter e excluir arquivos no Dropbox.

Para usar [qualquer conector](apis-list.md), primeiro é preciso criar um aplicativo lógico. Você pode começar [criando um aplicativo lógico agora](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="connect-to-dropbox"></a>Conectar-se ao Dropbox
Para que o aplicativo lógico possa acessar qualquer serviço, crie primeiro uma *conexão* com o serviço. Uma conexão fornece uma conectividade entre um aplicativo lógico e outro serviço. Por exemplo, para se conectar ao Dropbox, é preciso ter uma *conexão* com o Dropbox. Para criar uma conexão, é preciso fornecer as credenciais normalmente usadas para acessar o serviço ao qual você deseja se conectar. Desse modo, no exemplo do Dropbox, são necessárias as credenciais da sua conta no Dropbox para criar a conexão com o Dropbox. 

### <a name="create-a-connection-to-dropbox"></a>Criar uma conexão com o Dropbox
> [!INCLUDE [Steps to create a connection to Dropbox](../../includes/connectors-create-api-dropbox.md)]
> 
> 

## <a name="use-a-dropbox-trigger"></a>Usar um gatilho do Dropbox
Um gatilho é um evento que pode ser usado para iniciar o fluxo de trabalho definido em um aplicativo lógico. [Saiba mais sobre gatilhos](../logic-apps/logic-apps-overview.md#logic-app-concepts).

Neste exemplo, usaremos o gatilho **Quando um arquivo é criado**. Quando esse gatilho ocorrer, chamaremos a ação do Dropbox **Obter conteúdo do arquivo usando o caminho**. 

1. Insira *dropbox* na caixa de pesquisa no designer de Aplicativos Lógicos e selecione o gatilho **Dropbox - Quando um arquivo é criado**.      
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-trigger.PNG)  
2. Escolha a pasta na qual você deseja controlar a criação do arquivo. Escolha ... (identificado na caixa vermelha) e navegue até a pasta que deseja selecionar para entrada do gatilho.  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-trigger-2.PNG)  

## <a name="use-a-dropbox-action"></a>Usar uma ação do Dropbox
Uma ação é uma operação executada pelo fluxo de trabalho definido em um aplicativo lógico. [Saiba mais sobre ações](../logic-apps/logic-apps-overview.md#logic-app-concepts).

Agora que o gatilho foi adicionado, siga estas etapas para adicionar uma ação que obterá o novo conteúdo do arquivo.

1. Escolha **+ Nova Etapa** para adicionar a ação que deseja executar quando um novo arquivo for criado.  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action.PNG)
2. Escolha **Adicionar uma ação**. Isso abre a caixa de pesquisa, na qual é possível procurar qualquer ação que você deseja realizar.  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action-2.PNG)
3. Insira *dropbox* para pesquisar as ações relacionadas ao Dropbox.  
4. Escolha **Dropbox - Obter conteúdo do arquivo usando o caminho** como a ação a ser executada quando um novo arquivo for criado na pasta Dropbox selecionada. O bloco de controle de ação é aberto. Será solicitado que você autorize o aplicativo lógico a acessar sua conta no Dropbox, caso não tenha feito isso antes.  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action-3.PNG)  
5. Escolha ... (localizado no lado direito do controle **Caminho do Arquivo**) e navegue até o caminho do arquivo que deseja usar. Ou use o token do **caminho do arquivo** para agilizar a criação do aplicativo lógico.  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action-4.PNG)  
6. Salve seu trabalho e crie um novo arquivo no Dropbox para ativar o fluxo de trabalho.  

## <a name="connector-specific-details"></a>Detalhes específicos do conector

Exiba os gatilhos e ações definidos no swagger e também os limites nos [detalhes do conector](/connectors/dropbox/).

## <a name="more-connectors"></a>Mais conectores
Volte para a [Lista de APIs](apis-list.md).
