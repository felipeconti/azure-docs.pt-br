---
title: Notificações por email no PIM do Azure AD | Microsoft Docs
description: Descreve notificações por email no Azure AD PIM (Privileged Identity Management)
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: pim
ms.date: 07/24/2018
ms.author: rolyon
ms.reviewer: hanki
ms.custom: pim
ms.openlocfilehash: 7943b4fb8c2027b50ce04c30d21f1b0a58f98ace
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39621575"
---
# <a name="email-notifications-in-azure-ad-pim"></a>Notificações por email no Azure AD PIM

Quando ocorrem eventos importantes no Azure AD PIM (Privileged Identity Management), são enviadas notificações por email para o usuário ou administrador relevante. Por exemplo, o PIM envia emails referentes aos seguintes eventos:

- Quando uma ativação de função com privilégios está com aprovação pendente
- Quando uma solicitação de ativação de função com privilégios é aprovada
- Quando uma função com privilégios é ativada
- Quando uma função com privilégios é atribuída
- Quando o Azure AD PIM é habilitado

A partir do final de julho de 2018, as notificações por email enviadas por meio do PIM terão um novo endereço de email de remetente e um novo design visual. Essa atualização afetará o Azure AD PIM e o PIM dos recursos do Azure. Todos os eventos que disparavam uma notificação por email continuarão enviando um email. Alguns emails terão conteúdo atualizado fornecendo mais informações direcionadas.

## <a name="sender-email-address"></a>Endereço de email do remetente

A partir do final de julho de 2018, as notificações por email terão o seguinte endereço:

- Endereço de email:  **azure-noreply@microsoft.com**
- Nome de exibição: Microsoft Azure

Anteriormente, as notificações por email tinham o seguinte endereço:

- Endereço de email:  **azureadnotifications@microsoft.com**
- Nome de exibição: Serviço de Notificação do Microsoft Azure AD

## <a name="email-subject-line"></a>Linha do assunto do email

Iniciando no final de julho de 2018, as notificações por email para as funções de recurso do Azure e Azure AD terão um prefixo **PIM** na linha do assunto. Aqui está um exemplo:

- PIM: Alain Charon foi atribuído permanentemente à função de Leitor de Backup.

## <a name="pim-emails-for-azure-ad-roles"></a>Emails do PIM para funções do Azure AD

A partir do final de julho de 2018, as notificações por email do PIM para funções do Azure AD têm um design atualizado. Veja a seguir um email de exemplo que é enviado quando um usuário ativa uma função com privilégios para a organização fictícia Contoso.

![Novo email do PIM para funções do Azure AD](./media/pim-email-notifications/email-directory-new.png)

Anteriormente, quando um usuário ativava uma função com privilégios, o email tinha a aparência a seguir.

![Antigo email do PIM para funções do Azure AD](./media/pim-email-notifications/email-directory-old.png)

## <a name="pim-emails-for-azure-resource-roles"></a>Emails do PIM para funções de recurso do Azure

A partir do final de julho de 2018, as notificações por email do PIM para funções de recurso do Azure têm um design atualizado. Veja a seguir um email de exemplo que é enviado quando é atribuída a um usuário uma função com privilégios para a organização fictícia Contoso.

![Novo email do PIM para funções de recurso do Azure](./media/pim-email-notifications/email-resources-new.png)

Anteriormente, quando era atribuída a um usuário uma função com privilégios, o email tinha a aparência a seguir.

![Antigo email do PIM para funções de recurso do Azure](./media/pim-email-notifications/email-resources-old.png)

## <a name="next-steps"></a>Próximas etapas

- [Como gerenciar configurações de ativação de função no Azure AD PIM](pim-how-to-change-default-settings.md)
- [Aprovações no Azure AD PIM](azure-ad-pim-approval-workflow.md)
