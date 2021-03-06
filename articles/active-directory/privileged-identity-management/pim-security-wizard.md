---
title: O Assistente de Segurança do Privileged Identity Management do Azure AD
description: Na primeira vez que você usar a extensão Privileged Identity Management do Azure Active Directory, verá um assistente de segurança. Este artigo descreve as etapas para usar o assistente.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: pim
ms.date: 02/27/2017
ms.author: rolyon
ms.custom: pim ; H1Hack27Feb2017
ms.openlocfilehash: 19eb2b36b217dc67fabcc3c2c4721fb13b2224ec
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39616999"
---
# <a name="using-the-security-wizard-in-azure-ad-privileged-identity-management"></a>Usando o assistente de segurança no Azure AD Privileged Identity Management 
Se você for a primeira pessoa a executar o Azure PIM (Privileged Identity Management) para sua organização, verá um assistente. O assistente ajuda você a entender os riscos à segurança de identidades com privilégios e como usar o PIM para reduzir esses riscos. Você não precisará fazer nenhuma alteração nas atribuições de função existentes no assistente, se preferir fazer isso posteriormente.

## <a name="what-to-expect"></a>O que esperar
Antes de sua organização começar a usar o PIM, todas as atribuições de função são permanentes: os usuários sempre estarão sempre nessas funções, mesmo se não precisarem dos privilégios no momento.  A primeira etapa do assistente mostra uma lista de funções com privilégios altos e quantos usuários atualmente estão nessas funções. Você poderá analisar uma função específica para saber mais sobre os usuários se um ou mais não forem familiares.

A segunda etapa do assistente lhe fornece a oportunidade de alterar as atribuições de função do administrador.  

> [!WARNING]
> É importante que você tenha pelo menos um administrador global e mais de um administrador de função com privilégios com uma conta organizacional (não uma conta da Microsoft). Se houver apenas um administrador de função com privilégios, a organização não poderá gerenciar o PIM se essa conta for excluída.
> Além disso, mantenha as atribuições de função permanentes se um usuário tiver uma conta da Microsoft (uma conta que ele usa para entrar nos serviços Microsoft como o Outlook.com e o Skype). Se você planejar exigir o MFA para ativação para essa função, esse usuário será bloqueado.
> 
> 

Depois de ter feito alterações, o assistente não aparecerá mais. Na próxima vez que você ou outro administrador de função com privilégios usar o PIM, você verá o painel do PIM.  

* Se você desejar adicionar ou remover usuários das funções ou alterar as atribuições de permanentes para qualificadas, leia mais em [como adicionar ou remover uma função de usuário](pim-how-to-add-role-to-user.md).
* Se desejar fornecer aos usuários mais acesso para gerenciar o PIM, leia mais em [como conceder acesso para gerenciar no PIM](pim-how-to-give-access-to-pim.md).

## <a name="next-steps"></a>Próximas etapas
[!INCLUDE [active-directory-privileged-identity-management-toc](../../../includes/active-directory-privileged-identity-management-toc.md)]

