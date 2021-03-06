---
title: Introdução ao Azure AD Privileged Identity Management | Microsoft Docs
description: Saiba como gerenciar identidades privilegiadas com o aplicativo Azure Active Directory Privileged Identity Management no portal do Azure.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: pim
ms.topic: conceptual
ms.workload: identity
ms.date: 09/17/2017
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 4eeb58e6e7377c2c0ec7db850a84bf1e296500d2
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39623044"
---
# <a name="start-using-azure-ad-privileged-identity-management"></a>Começar a usar o Azure AD Privileged Identity Management

Com o Privileged Identity Management do Azure Active Directory (AD), você pode gerenciar, controlar e monitorar o acesso em sua organização. Esse escopo inclui o acesso a recursos do Azure, Azure AD e outros serviços online da Microsoft, como o Office 365 ou o Microsoft Intune.

Este artigo lhe mostra como adicionar o aplicativo do Azure AD PIM o painel do portal do Azure.

## <a name="add-the-privileged-identity-management-application"></a>Adicionar o aplicativo Privileged Identity Management

Antes de usar o Azure AD Privileged Identity Management, você precisa adicionar o aplicativo ao painel do portal do Azure.

1. Entre no [portal do Azure](https://portal.azure.com/) como um administrador global do seu diretório.
2. Se sua organização tiver mais de um diretório, selecione seu nome de usuário no canto superior direito do portal do Azure. Selecione o diretório onde você deseja usar o PIM.
3. Selecione **Todos os serviços** e use a caixa de texto Filtrar para pesquisar o **Azure AD Privileged Identity Management**.
4. Marque **Fixar no painel** e então clique em **Criar**. O aplicativo Privileged Identity Management é aberto.

Se você for a primeira pessoa a usar o Azure AD Privileged Identity Management em seu diretório, receberá automaticamente as funções **Administrador de segurança** e **Administrador com privilégios de função** no diretório. Somente os administradores com privilégios de função podem gerenciar atribuições de função de usuários do diretório do Azure AD. Além disso, você pode optar por executar o [assistente de segurança.](pim-security-wizard.md) que orienta você durante a experiência inicial de detecção e atribuição.

## <a name="navigate-to-your-tasks"></a>Navegue até as tarefas

Depois que o Azure AD Privileged Identity Management estiver configurado, você verá a folha de navegação sempre que abrir o aplicativo. Use essa folha para realizar suas tarefas de gerenciamento de identidade.

![Tarefas de nível superior para o PIM - captura de tela](./media/pim-getting-started/PIM_Tasks_New.png)

- **Minhas funções** exibe uma lista de funções qualificadas e ativas atribuídas a você. É aqui que você pode ativar as funções qualificadas atribuídas.
- **Aprovar solicitações (Visualização)** exibe uma lista de solicitações para ativar funções qualificadas do diretório do Azure AD por usuários em seu diretório, que você pode aprovar. [Saiba mais.](./azure-ad-pim-approval-workflow.md)
- **Solicitações pendentes (Visualização)** exibe qualquer uma das suas solicitações pendentes para ativar atribuições de função qualificada.
- **Revisar Acesso** lista as revisões de acesso ativas atribuídas a você para completar, esteja você revisando o acesso para si mesmo ou para outra pessoa.
- As **funções de diretório do Azure AD** localizadas na seção gerenciar do menu de navegação esquerdo exibem o painel onde os administradores de função com privilégios gerenciam atribuições de função, alteram configurações de ativação de função, iniciam revisões de acesso e muito mais. Esse painel é desabilitado para todos que não forem administradores de função com privilégios. Esses usuários têm acesso a um painel especial denominado Minha exibição. O painel Minha exibição exibe somente informações sobre o usuário que acessa o painel, não o locatário inteiro.
- **Funções de recurso do Azure (Visualização)** localizadas na seção gerenciar do menu de navegação esquerdo exibem uma lista de recursos de assinatura que você tem atribuição de função 

## <a name="next-steps"></a>Próximas etapas
A [visão geral do Azure AD Privileged Identity Management](pim-configure.md) inclui mais detalhes sobre como você pode gerenciar o acesso administrativo em sua organização.

[!INCLUDE [active-directory-privileged-identity-management-toc](../../../includes/active-directory-privileged-identity-management-toc.md)]
