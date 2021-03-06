---
title: Configurar uma Identidade de Serviço Gerenciada em um conjunto de dimensionamento de máquinas virtuais do Azure usando o Portal do Azure
description: Instruções passo a passo para configurar uma Identidade de Serviço Gerenciada em VMSS do Azure usando o Portal do Azure.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/20/2018
ms.author: daveba
ms.openlocfilehash: 93c532cf2864db28b580303ecefec8b6dbed65f6
ms.sourcegitcommit: c2c64fc9c24a1f7bd7c6c91be4ba9d64b1543231
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/26/2018
ms.locfileid: "39257752"
---
# <a name="configure-a-virtual-machine-scale-set-managed-service-identity-using-the-azure-portal"></a>Configurar uma Identidade de Serviço Gerenciada do conjunto de dimensionamento de máquinas virtuais usando o portal do Azure

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

A Identidade de Serviço Gerenciado fornece aos serviços do Azure uma identidade gerenciada automaticamente no Active Directory do Azure. Você pode usar essa identidade para autenticar em qualquer serviço que dá suporte à autenticação do Azure AD, incluindo o Key Vault, sem ter as credenciais no seu código. 

Neste artigo, você aprenderá a habilitar e a desabilitar a identidade atribuída pelo sistema para um conjunto de dimensionamento de máquinas virtuais usando o portal do Azure. No momento, não há suporte para atribuir e remover identidades atribuídas pelo usuário de um conjunto de dimensionamento de máquinas virtuais do Azure usando o portal do Azure.

> [!NOTE]
> No momento, não há suporte para operações de identidade atribuída pelo usuário por meio do Portal do Azure. Procure novamente por atualizações.

## <a name="prerequisites"></a>Pré-requisitos

- Se você não estiver familiarizado com a Identidade de Serviço Gerenciada, consulte a [seção de visão geral](overview.md).
- Se você ainda não tiver uma conta do Azure, [inscreva-se em uma conta gratuita](https://azure.microsoft.com/free/) antes de continuar.
- Para realizar as operações de gerenciamento deste artigo, sua conta precisará da seguinte atribuição de função:
    - [Colaborador de Máquina Virtual](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) para habilitar e remover identidade gerenciada atribuída ao sistema de um conjunto de dimensionamento de máquinas virtuais.

## <a name="managed-service-identity-during-creation-of-an-azure-virtual-machine-scale-set"></a>Habilitar a Identidade de Serviço Gerenciada durante a criação de um conjunto de dimensionamento de máquinas virtuais do Azure

No momento, não há suporte de criação de VM por meio do Portal do Azure para operações de Identidade de Serviço Gerenciada. Em vez disso, consulte o artigo de Início Rápido da criação do conjunto de dimensionamento de máquinas virtuais do Azure para primeiro criar um conjunto de dimensionamento de máquinas virtuais do Azure:

- [Criar um conjunto de dimensionamento de máquinas virtuais no Portal do Azure](../../virtual-machine-scale-sets/quick-create-portal.md)  

Vá para a próxima seção para obter detalhes sobre como habilitar a Identidade de Serviço Gerenciada no conjunto de dimensionamento de máquinas virtuais.

## <a name="enable-managed-service-identity-on-an-existing-azure-vmms"></a>Habilitar a Identidade de Serviço Gerenciada em uma VMMS existente do Azure

Para habilitar a identidade atribuída pelo sistema em uma VM que foi originalmente provisionada sem ela:

1. Entre no [Portal do Azure](https://portal.azure.com) usando uma conta associada à assinatura do Azure que contém o conjunto de dimensionamento de máquinas virtuais.

2. Navegue até o conjunto de dimensionamento de máquinas virtuais desejado.

3. Habilite a identidade atribuída pelo sistema na VM selecionando "Sim" em "Identidade de serviço gerenciada" e clique em **Salvar**. Esta operação pode demorar 60 segundos ou mais para ser concluída:

   [![Captura de tela da página de configuração](../managed-service-identity/media/msi-qs-configure-portal-windows-vmss/create-windows-vmss-portal-configuration-blade.png)](../managed-service-identity/media/msi-qs-configure-portal-windows-vmss/create-windows-vmss-portal-configuration-blade.png#lightbox)  

## <a name="remove-managed-service-identity-from-an-azure-virtual-machine-scale-set"></a>Remover a Identidade de Serviço Gerenciada de um conjunto de dimensionamento de máquinas virtuais do Azure

Se você tiver um conjunto de dimensionamento de máquinas virtuais que não precise mais de uma Identidade de Serviço Gerenciada:

1. Entre no [Portal do Azure](https://portal.azure.com) usando uma conta associada à assinatura do Azure que contém o conjunto de dimensionamento de máquinas virtuais. Certifique-se também de que sua conta pertence a uma função que concede permissões de gravação no conjunto de dimensionamento de máquinas virtuais.

2. Navegue até o conjunto de dimensionamento de máquinas virtuais desejado.

3. Desabilite a identidade atribuída pelo sistema na VM selecionando "Não" em "Identidade de serviço gerenciado" e clique em Salvar. Esta operação pode demorar 60 segundos ou mais para ser concluída:

   ![Captura de tela da página de configuração](../managed-service-identity/media/msi-qs-configure-portal-windows-vmss/disable-windows-vmss-portal-configuration-blade.png)  

## <a name="related-content"></a>Conteúdo relacionado

- Para obter uma visão geral da Identidade de Serviço Gerenciada, confira s [visão geral](overview.md).

## <a name="next-steps"></a>Próximas etapas

- Usando o portal do Azure, conceda a uma Identidade de Serviço Gerenciada do conjunto de dimensionamento de máquinas virtuais do Azure [acesso a outro recurso do Azure](howto-assign-access-portal.md).

Use a seção de comentários a seguir para fornecer seus comentários e nos ajudar a aprimorar e adaptar nosso conteúdo.
