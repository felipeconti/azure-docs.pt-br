---
title: Configurar locais nomeados no Azure Active Directory | Microsoft Docs
description: Saiba como configurar locais nomeados.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.component: protection
ms.assetid: f56e042a-78d5-4ea3-be33-94004f2a0fc3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 62a55672a4326df585fc84699dfd72424be362dc
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39627484"
---
# <a name="configure-named-locations-in-azure-active-directory"></a>Configurar locais nomeados no Azure Active Directory

Com locais nomeados, você pode rotular os intervalos de endereços IP confiáveis em sua organização. O Azure Active Directory usa localizações nomeadas nos seguintes contextos:

- A detecção de [eventos de risco](reports-monitoring/concept-risk-events.md) para reduzir o número de falsos positivos relatados.  

- [Acesso condicional com base em localização](conditional-access/location-condition.md).


Este artigo explica como você pode configurar localizações nomeadas em seu ambiente.


## <a name="entry-points"></a>Pontos de entrada

Você pode acessar a página de configuração da localização nomeada na seção de **Segurança** da página do Active Directory do Azure clicando em:

![Pontos de entrada](./media/active-directory-named-locations/34.png)

- **Acesso condicional:**

    - Na seção **Gerenciar**, clique em **Localizações nomeadas**.
    
        ![O comando Localizações nomeadas](./media/active-directory-named-locations/06.png)

- **Entradas de risco:**

    - Na barra de ferramentas na parte superior, clique em **Adicionar intervalos de endereços IP conhecidos**.

       ![O comando Localizações nomeadas](./media/active-directory-named-locations/35.png)



## <a name="configuration-example"></a>Exemplo de configuração

**Para configurar uma localização nomeada:**

1. Entre no [Portal do Azure](https://portal.azure.com) como administrador global.

2. No painel esquerdo, clique em **Azure Active Directory**.

    ![O link do Azure Active Directory no painel esquerdo](./media/active-directory-named-locations/01.png)

3. Na página do **Active Directory do Azure**, na seção **Segurança**, clique em **Acesso condicional**.

    ![O comando Acesso condicional](./media/active-directory-named-locations/05.png)


4. Na página **Acesso Condicional**, na seção **Gerenciar**, clique em **Localizações nomeadas**.

    ![O comando Localizações nomeadas](./media/active-directory-named-locations/06.png)


5. Na página **Localizações nomeadas**, clique em **Novo local**.

    ![O comando Novo local](./media/active-directory-named-locations/07.png)


6. Na página **Novo**, faça o seguinte:

    ![A Nova folha](./media/active-directory-named-locations/61.png)

    a. Na caixa **Nome**, digite um nome para a localização nomeada.

    b. Na caixa **Intervalos de IP**, digite um intervalo de IP. O intervalo de IP precisa estar no formato *CIDR* (Roteamento Entre Domínios Sem Classe).  

    c. Clique em **Criar**.



## <a name="next-steps"></a>Próximas etapas

Para obter mais informações, consulte:

- [Acesso condicional no Azure Active Directory](active-directory-conditional-access-azure-portal.md).

- [Condições de local no acesso condicional do Azure Active Directory](conditional-access/location-condition.md)

- [Eventos de risco do Azure Active Directory](reports-monitoring/concept-risk-events.md).

- [Relatório de entradas arriscadas no portal do Azure Active Directory](reports-monitoring/concept-risky-sign-ins.md).  
