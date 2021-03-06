---
title: Neste tutorial, você cria uma VM de pilha do Azure usando um modelo | Microsoft Docs
description: Descreve como usar o ASDK para criar uma VM usando um modelo de predfined e um modelo personalizado do GitHub.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 08/15/2018
ms.author: sethm
ms.reviewer: ''
ms.openlocfilehash: 5026a7a753ec744d281266b2fb30a70a66a7f9db
ms.sourcegitcommit: d2f2356d8fe7845860b6cf6b6545f2a5036a3dd6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "42139702"
---
# <a name="tutorial-create-a-vm-using-a-community-template"></a>Tutorial: criar uma VM usando um modelo da comunidade
Como um usuário ou operador do Azure Stack, você pode criar uma VM usando [modelos de início rápido do GitHub personalizados](https://github.com/Azure/AzureStack-QuickStart-Templates) em vez de implantar um manualmente no marketplace do Azure Stack.

Neste tutorial, você aprenderá como:

> [!div class="checklist"]
> * Saiba mais sobre modelos de início rápido do Azure Stack 
> * Criar uma VM usando um modelo personalizado do GitHub
> * Inicie o minikube e instalar um aplicativo

## <a name="learn-about-azure-stack-quickstart-templates"></a>Saiba mais sobre modelos de início rápido do Azure Stack
Modelos de início rápido do Azure Stack são armazenados na [repositório de modelos de início rápido AzureStack público](https://github.com/Azure/AzureStack-QuickStart-Templates) no GitHub. Esse repositório contém modelos de implantação do Azure Resource Manager que foram testados com o Microsoft Azure Stack desenvolvimento ASDK (Kit). Você pode usá-los para tornar mais fácil para você avaliar o Azure Stack e use o ambiente de ASDK. 

Ao longo do tempo muitos usuários GitHub contribuíram para o repositório, resultando em uma grande coleção de modelos de implantação de mais de 400. Esse repositório é um ótimo ponto de partida para obter uma melhor compreensão de como você pode implantar vários tipos de ambientes para o Azure Stack. 

>[!IMPORTANT]
> Alguns desses modelos são criados por membros da comunidade e não pela Microsoft. Cada modelo é licenciado sob um contrato de licença de seu proprietário e não da Microsoft. A Microsoft não é responsável por esses modelos e não avalia sua segurança, compatibilidade ou desempenho. Modelos da comunidade não têm suporte em qualquer serviço ou programa de suporte da Microsoft e são disponibilizados "Como estão" sem garantias de qualquer tipo.

Se você quiser contribuir com seus modelos do Azure Resource Manager para o GitHub, você deve fazer sua contribuição para o [repositório azure-quickstart-templates](https://github.com/Azure/AzureStack-QuickStart-Templates).

Para saber mais sobre o repositório do GitHub e como Contribuir com ele, consulte o [arquivo de Leiame do repositório](https://github.com/Azure/AzureStack-QuickStart-Templates/blob/master/README.md). 


## <a name="create-a-vm-using-a-custom-github-template"></a>Criar uma VM usando um modelo personalizado do GitHub
Neste tutorial de exemplo, o [101-vm-linux-minikube](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-vm-linux-minikube) modelo de início rápido do Azure Stack é usado para implantar uma máquina de virtual do Ubuntu 16.04 no AzureStack executando Minikube para gerenciar um cluster kubenetes.

Minikube é uma ferramenta que torna mais fácil de executar o Kubernetes localmente. Minikube executa um cluster do Kubernetes de nó único dentro de uma VM para usuários que desejam experimentar Kubernetes ou desenvolver com ele diárias. Ele dá suporte a um simples, o cluster do Kubernetes de um nó em execução em uma VM do Linux. É a maneira mais rápida e mais simples de obter um cluster Kubernetes totalmente funcional em execução. Ele permite que os desenvolvedores a desenvolver e testar suas implantações de aplicativo baseado no Kubernetes em suas máquinas locais. Em termos de arquitetura, Minikube VM executa mestre e componentes do agente do nó localmente:
- Componentes do nó mestre como servidor de API, Agendador e etcd Server são executados em um único processo do Linux chamado LocalKube.
- Componentes do agente do nó são executados dentro de contêineres do docker, exatamente como eles seriam executados em um nó de agente normal. De um ponto de vista de implantação do aplicativo, não há nenhuma diferença quando o aplicativo é implantado em um cluster de Kubernetes regular ou o Minikube.

Esse modelo instala os seguintes componentes:

- VM do Ubuntu 16.04 LTS
- [Docker-CE](https://download.docker.com/linux/ubuntu) 
- [Kubectl](https://storage.googleapis.com/kubernetes-release/release/v1.8.0/bin/linux/amd64/kubectl)
- [Minikube](https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64)
- xFCE4
- xRDP

> [!IMPORTANT]
> A imagem de VM do Ubuntu (Ubuntu Server 16.04 LTS neste exemplo) deve já ter sido adicionada para o Azure Stack marketplace antes de começar estas etapas.

1.  Clique em **+ novo** > **personalizado** > **implantação de modelo**.

    ![](media/azure-stack-create-vm-template/1.PNG) 

2. Clique em **Editar modelo**.

   ![](media/azure-stack-create-vm-template/2.PNG) 

3.  Clique em **modelo de início rápido**.

       ![](media/azure-stack-create-vm-template/3.PNG)

4. Selecione **101-vm-linux-minikube** dos modelos disponíveis usando o **selecionar um modelo** lista suspensa e, em seguida, clique **Okey**.  

   ![](media/azure-stack-create-vm-template/4.PNG)

5. Se você quiser fazer modificações ao modelo JSON, você pode fazer isso, se não, ou ao concluir, clique em **salvar** para fechar a caixa de diálogo Editar modelo.

   ![](media/azure-stack-create-vm-template/5.PNG) 

6.  Clique em **parâmetros**, preencha ou modifique os campos disponíveis conforme o necessário e, em seguida, clique em **Okey**. Escolha a assinatura para usar, crie ou escolha um nome de grupo de recursos existente e, em seguida, clique em **criar** para iniciar a implantação de modelo.

       ![](media/azure-stack-create-vm-template/6.PNG)

7. Escolha a assinatura para usar, crie ou escolha um nome de grupo de recursos existente e, em seguida, clique em **criar** para iniciar a implantação de modelo.

   ![](media/azure-stack-create-vm-template/7.PNG)

8. A implantação do grupo de recursos leva vários minutos para criar a VM personalizada com base no modelo. Você pode monitorar o status da instalação por meio de notificações e de propriedades do grupo de recursos. 

   ![](media/azure-stack-create-vm-template/8.PNG)

   >[!NOTE]
   > A VM será executado quando a implantação for concluída. 

## <a name="start-minikube-and-install-an-application"></a>Inicie o minikube e instalar um aplicativo
Agora que a VM do Linux foi criada com êxito, você pode entrar para iniciar o minikube e instalar um aplicativo. 

1. Depois que a implantação for concluída, clique em **Connect** para exibir o endereço IP público que será usado para conectar-se à VM do Linux. 

   ![](media/azure-stack-create-vm-template/9.PNG)

2. Em um prompt de comando com privilégios elevados, execute **mstsc.exe** para abrir a Conexão de área de trabalho remota e se conectar ao endereço IP público de Linux VM descoberto na etapa anterior. Quando solicitado a entrar no xRDP, use as credenciais que você especificou ao criar a VM.

   ![](media/azure-stack-create-vm-template/10.PNG)

3. Abra o emulador de Terminal e insira os seguintes comandos para iniciar o minikube:

    >    `sudo minikube start --vm-driver=none`
    >   
    >    `sudo minikube addons enable dashboard`
    >    
    >    `sudo minikube dashboard --url`

   ![](media/azure-stack-create-vm-template/11.PNG)

4. Abra o navegador da web e visite o endereço do painel de kubernetes. Parabéns, agora você tem uma totalmente kubernetes instalação funcional usando minikube!

   ![](media/azure-stack-create-vm-template/12.PNG)

5. Se você deseja implantar um aplicativo de exemplo, visite a página de documentação oficial do kubernetes, ignore a seção "Criar Cluster do Minikube", pois já tiver criado uma acima. Basta ir para a seção "Criar seu aplicativo Node. js" em https://kubernetes.io/docs/tutorials/stateless-application/hello-minikube/.

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você aprendeu como:

> [!div class="checklist"]
> * Saiba mais sobre modelos de início rápido do Azure Stack 
> * Criar uma VM usando um modelo personalizado do GitHub
> * Inicie o minikube e instalar um aplicativo

