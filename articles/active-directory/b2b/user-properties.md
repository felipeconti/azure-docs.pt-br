---
title: Propriedades de um usuário de colaboração B2B do Azure Active Directory | Microsoft Docs
description: As propriedades do usuário de colaboração B2B do Azure Active Directory podem ser configuradas
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 05/25/2017
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 0cfd7888acf942e4af875c37c2472ff086f9119b
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39057888"
---
# <a name="properties-of-an-azure-active-directory-b2b-collaboration-user"></a>Propriedades de um usuário de colaboração B2B do Azure Active Directory

Um usuário de colaboração entre empresas (B2B) do Azure Active Directory (AD do Azure) é um usuário com UserType = Convidado. Normalmente, esse usuário convidado representa uma organização parceira e, por padrão, tem privilégios limitados no diretório que convida.

Dependendo das necessidades da organização que convida, um usuário de colaboração B2B do AD do Azure pode estar em um dos seguintes estados de conta:

- Estado 1: hospedado em uma instância externa do AD do Azure, representado como um usuário convidado na organização que convida. Nesse caso, o usuário B2B entra usando uma conta do Azure AD que pertence ao locatário convidado. Se a organização do parceiro não usar o Azure AD, o usuário convidado no Azure AD ainda será criado. Os requisitos são o resgate do convite e a verificação do endereço de email pelo Azure AD. Essa disposição também é chamada de locação JIT (just-in-time) ou, às vezes, de locação "viral".

- Estado 2: hospedado na conta da Microsoft e representado como um usuário convidado na organização host. Nesse caso, o usuário convidado entra com uma conta da Microsoft. A identidade social do usuário convidado (google.com ou semelhante), que não é uma conta da Microsoft, é criada como uma conta da Microsoft durante o resgate da oferta.

- Estado 3: hospedado no Active Directory local da organização host e sincronizado com o AD do Azure da organização host. Durante esta versão, você deve usar o PowerShell para alterar manualmente o UserType desses usuários na nuvem.

- Estado 4: hospedado na organização do Azure AD com UserType = Convidado e com credenciais que gerenciam a organização host.

  ![Exibição das iniciais do emissor do convite](media/user-properties/redemption-diagram.png)


Agora, vamos ver a aparência de um usuário de colaboração B2B do AD do Azure no Estado 1 no Azure AD.

### <a name="before-invitation-redemption"></a>antes do resgate do convite

![Antes do resgate de oferta](media/user-properties/before-redemption.png)

### <a name="after-invitation-redemption"></a>após o resgate do convite

![Depois do resgate de oferta](media/user-properties/after-redemption.png)

## <a name="key-properties-of-the-azure-ad-b2b-collaboration-user"></a>As principais propriedades do usuário de colaboração B2B do AD do Azure
### <a name="usertype"></a>UserType
Essa propriedade indica a relação entre o usuário e o locatário do host. Essa propriedade pode assumir dois valores:
- Membro: este valor indica um funcionário da organização host e um usuário na folha de pagamento da organização. Por exemplo, provavelmente esse usuário poderá acessar somente os sites internos. Esse usuário não seria considerado um colaborador externo.

- Convidado: esse valor indica um usuário que não é considerado interno à empresa, como um colaborador externo, parceiro, cliente ou usuário semelhante. Um usuário como esse não deve receber um memorando interno do CEO, ou benefícios da empresa, por exemplo.

  > [!NOTE]
  > O UserType não tem nenhuma relação com o tipo de acesso do usuário, nem com a função do diretório do usuário e assim por diante. Essa propriedade só indica a relação do usuário com a organização host, e permite que a organização aplique as políticas que dependem desse atributo.

### <a name="source"></a>Fonte
Essa propriedade indica o tipo de acesso do usuário.

- Usuário convidado: esse usuário foi convidado, mas ainda não resgatou seu convite.

- Active Directory externo: esse usuário está hospedado em uma organização externa e é autenticado com uma conta do AD do Azure que pertence a outra organização. Esse tipo de acesso corresponde ao Estado 1.

- Conta da Microsoft: este usuário está hospedado em uma conta da Microsoft e é autenticado usando uma conta da Microsoft. Esse tipo de acesso corresponde ao Estado 2.

- Active Directory do Windows Server: este usuário faz logon no Active Directory local que pertence a esta organização. Esse tipo de acesso corresponde ao Estado 3.

- Azure Active Directory: este usuário é autenticado usando uma conta do Azure AD que pertence a esta organização. Esse tipo de acesso corresponde ao Estado 4.
  > [!NOTE]
  > A origem e o UserType são propriedades independentes. O valor de origem não envolve um valor específico para o UserType.

## <a name="can-azure-ad-b2b-users-be-added-as-members-instead-of-guests"></a>Os usuários B2B do AD do Azure podem ser adicionados como membros em vez de convidados?
Normalmente, um usuário B2B do Azure AD e o usuário convidado são sinônimos. Portanto, um usuário de colaboração B2B do AD do Azure é adicionado por padrão como um usuário com UserType = Convidado. No entanto, em alguns casos, a organização parceira é membra de uma organização maior a qual a organização host também pertence. Se esse for o caso, talvez a organização host queira tratar os usuários na organização parceira como membros e não convidados. Use as APIs do gerenciador de convites B2B do AD do Azure para adicionar ou convidar um usuário da organização parceira para a organização host como um membro.

## <a name="filter-for-guest-users-in-the-directory"></a>Filtragem de usuários convidados no diretório

![Como filtrar usuários convidados](media/user-properties/filter-guest-users.png)

## <a name="convert-usertype"></a>Converter UserType
Atualmente, os usuários podem usar o PowerShell para converter o UserType de membro para convidado e vice-versa. No entanto, a propriedade do UserType deve representar a relação do usuário com a organização. Portanto, o valor dessa propriedade só pode ser alterado se a relação entre o usuário e a organização mudar. Se o relacionamento do usuário mudar, questões como alteração do nome UPN também devem ser abordadas? O usuário poderá acessar os mesmos recursos? Uma caixa de correio deve ser atribuída? Portanto, não recomendamos alterar o UserType no PowerShell como uma atividade atômica. Além disso, caso essa propriedade se torne imutável usando o PowerShell, é melhor não assumir uma dependência desse valor.

## <a name="remove-guest-user-limitations"></a>Como remover limitações do usuário convidado
Em alguns casos, talvez você queira dar privilégios mais altos aos usuários convidados. Você pode adicionar um usuário convidado a qualquer função e até mesmo remover as restrições do usuário convidado padrão no diretório a fim de fornecer os mesmos privilégios como membros.

É possível desativar as limitações do usuário convidado padrão para que um usuário convidado no diretório da empresa receba as mesmas permissões que um membro.

![Como remover limitações do usuário convidado](media/user-properties/remove-guest-limitations.png)

## <a name="can-i-make-guest-users-visible-in-the-exchange-global-address-list"></a>É possível tornar os usuários convidados visíveis na Lista de Endereços Global do Exchange?
Sim. Por padrão, os objetos convidados não estão visíveis na lista de endereços global da organização, mas é possível usar o PowerShell do Azure Active Directory para torná-los visíveis. Para obter mais detalhes, consulte **É possível tornar os objetos convidados visíveis na lista de endereços global?** em [Acesso para convidado em Grupos do Office 365](https://support.office.com/article/guest-access-in-office-365-groups-bfc7a840-868f-4fd6-a390-f347bf51aff6#PickTab=FAQ). 

## <a name="next-steps"></a>Próximas etapas

* [O que é a colaboração B2B do AD do Azure?](what-is-b2b.md)
* [Tokens de usuário de colaboração B2B](user-token.md)
* [Mapeamento de declarações de usuário de colaboração B2B](claims-mapping.md)
