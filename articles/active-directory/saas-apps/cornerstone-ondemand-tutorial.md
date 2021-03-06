---
title: 'Tutorial: integração do Azure Active Directory com o Cornerstone OnDemand | Microsoft Docs'
description: Saiba como configurar o logon único entre o Azure Active Directory e o Cornerstone OnDemand.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: f57c5fef-49b0-4591-91ef-fc0de6d654ab
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: jeedes
ms.openlocfilehash: 4927421afeddc337856c027b3ed32539f4f8c1fc
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39441690"
---
# <a name="tutorial-azure-active-directory-integration-with-cornerstone-ondemand"></a>Tutorial: integração do Active Directory do Azure ao Cornerstone OnDemand

Neste tutorial, você aprenderá a integrar o Cornerstone OnDemand ao Azure Active Directory (Azure AD).

A integração do Cornerstone OnDemand ao Azure AD oferece os seguintes benefícios:

- Você pode controlar no Azure AD quem tem acesso ao Cornerstone OnDemand
- Você pode permitir que os usuários façam logon automaticamente no Cornerstone OnDemand (Logon Único) com suas contas do Azure AD
- Você pode gerenciar suas contas em um única localização: o Portal do Azure

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao Azure AD, consulte [o que é o acesso a aplicativos e logon único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD ao Cornerstone OnDemand, você precisa dos seguintes itens:

- Uma assinatura do AD do Azure
- Uma assinatura habilitada para logon único do Cornerstone OnDemand

> [!NOTE]
> Para testar as etapas deste tutorial, nós não recomendamos o uso de um ambiente de produção.

Para testar as etapas deste tutorial, você deve seguir estas recomendações:

- Não use o ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do AD do Azure, você pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, você testará o logon único do Azure AD em um ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Cornerstone OnDemand da galeria
1. configurar e testar o logon único do AD do Azure

## <a name="adding-cornerstone-ondemand-from-the-gallery"></a>Adicionando Cornerstone OnDemand da galeria
Para configurar a integração do Cornerstone OnDemand ao Azure AD, você precisa adicionar o Cornerstone OnDemand da galeria à sua lista de aplicativos SaaS gerenciados.

**Para adicionar o Cornerstone OnDemand da galeria, execute as seguintes etapas:**

1. No **[Portal do Azure](https://portal.azure.com)**, no painel navegação à esquerda, clique no ícone **Azure Active Directory**. 

    ![Active Directory][1]

1. Navegue até **aplicativos empresariais**. Em seguida, vá para **todos os aplicativos**.

    ![APLICATIVOS][2]

1. Clique no botão **Novo aplicativo** na parte superior da caixa de diálogo para adicionar o novo aplicativo.

    ![APLICATIVOS][3]

1. Na caixa de pesquisa, digite **Cornerstone OnDemand**.

    ![Criação de um usuário de teste do AD do Azure](./media/cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_search.png)

1. No painel de resultados, selecione **Cornerstone OnDemand**e, em seguida, clique no botão **Adicionar** para adicionar o aplicativo.

    ![Criação de um usuário de teste do AD do Azure](./media/cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>configurar e testar o logon único do AD do Azure
Nesta seção, você configura e testa o logon único do Azure AD com o Cornerstone OnDemand com base em um usuário de teste chamado “Brenda Fernandes”.

Para que o logon único funcione, o Azure AD precisa saber qual usuário do Cornerstone OnDemand é equivalente a um usuário do Azure AD. Em outras palavras, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado do Cornerstone OnDemand.

No Cornerstone OnDemand, atribua o valor do **nome de usuário** no Azure AD como o valor do **Nome de usuário** para estabelecer a relação de vínculo.

Para configurar e testar o logon único do Azure AD com o Cornerstone OnDemand, você precisa concluir os seguintes blocos de construção:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - para habilitar seus usuários a usar esse recurso.
1. **[Criação de um usuário de teste do AD do Azure](#creating-an-azure-ad-test-user)** – para testar o logon único do AD do Azure com Brenda Fernandes.
1. **[Criar um usuário de teste do Cornerstone OnDemand](#creating-a-cornerstone-ondemand-test-user)** – para ter um equivalente de Brenda Fernandes no Cornerstone OnDemand vinculado à representação de usuário do Azure AD.
1. **[Atribuição do usuário de teste do AD do Azure](#assigning-the-azure-ad-test-user)** – para permitir que Brenda Fernandes use o logon único do AD do Azure.
1. **[Teste do logon único](#testing-single-sign-on)** : para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do logon único do Azure AD

Nesta seção, você habilita o logon único do Azure AD no portal do Azure e configura o logon único no aplicativo Cornerstone OnDemand.

**Para configurar o logon único do Azure AD com o Cornerstone OnDemand, execute as seguintes etapas:**

1. No portal do Azure, na página de integração de aplicativos do **Cornerstone OnDemand**, clique em **Logon único**.

    ![Configurar o logon único][4]

1. Na caixa de diálogo **Logon único**, selecione **Modo** como **Logon baseado em SAML** para habilitar o logon único.

    ![Configurar o logon único](./media/cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_samlbase.png)

1. Na seção **Domínio e URLs do Cornerstone OnDemand**, execute a etapa a seguir:

    ![Configurar o logon único](./media/cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_url.png)

    a. Na caixa de texto **URL de Logon**, digite uma URL usando o seguinte padrão: `https://<company>.csod.com`

    b. Na caixa de texto **Identificador**, digite uma URL usando o seguinte padrão: `https://<company>.csod.com`

    > [!NOTE] 
    > Esses valores não são reais. Atualize esses valores com a URL de Entrada e o Identificador reais. Entre em contato com a [equipe de suporte ao cliente do Cornerstone OnDemand](mailTo:moreinfo@csod.com) para obter esses valores.

1. Na seção **Certificado de Autenticação do SAML**, clique em **Certificado (Base64)** e, em seguida, salve o arquivo do certificado no computador.

    ![Configurar o logon único](./media/cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_certificate.png) 

1. Clique no botão **Salvar** .

    ![Configurar o logon único](./media/cornerstone-ondemand-tutorial/tutorial_general_400.png)

1. Na seção **Configuração do Cornerstone OnDemand**, clique em **Configurar Cornerstone OnDemand** para abrir a janela **Configurar logon**. Copie a **URL do serviço de logon único do SAML e a URL de logoff** da **seção de Referência Rápida.**

    ![Configurar o logon único](./media/cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_configure.png) 

1. Para configurar o logon único no lado do **Cornerstone OnDemand**, você precisa enviar o **Certificado** baixado, a **URL de logoff** e a **URL de Logon Único de Serviço do SAML** para a [equipe de suporte do Cornerstone OnDemand](mailTo:moreinfo@csod.com). Eles definem essa configuração para ter a conexão de SSO de SAML definida corretamente em ambos os lados.

### <a name="creating-an-azure-ad-test-user"></a>Criação de um usuário de teste do AD do Azure
O objetivo desta seção é criar um usuário de teste no Portal do Azure chamado Brenda Fernandes.

![Criar um usuário do AD do Azure][100]

**Para criar um usuário de teste no AD do Azure, execute as seguintes etapas:**

1. No **Portal do Azure**, no painel de navegação esquerdo, clique no ícone **Azure Active Directory**.

    ![Criação de um usuário de teste do AD do Azure](./media/cornerstone-ondemand-tutorial/create_aaduser_01.png)

1. Vá para **Usuários e grupos** e clique em **Todos os usuários** para exibir a lista de usuários.

    ![Criação de um usuário de teste do AD do Azure](./media/cornerstone-ondemand-tutorial/create_aaduser_02.png) 

1. Para abrir a caixa de diálogo **Usuário**, clique em **Adicionar** na parte superior da caixa de diálogo.

    ![Criação de um usuário de teste do AD do Azure](./media/cornerstone-ondemand-tutorial/create_aaduser_03.png)

1. Na página do diálogo **Usuário**, execute as seguintes etapas:

    ![Criação de um usuário de teste do AD do Azure](./media/cornerstone-ondemand-tutorial/create_aaduser_04.png) 

    a. Na caixa de texto **Nome**, digite **Brenda Fernandes**.

    b. Na caixa de texto **Nome de usuário**, digite o **endereço de email** da conta de Brenda Fernandes.

    c. Selecione **Mostrar senha** e anote o valor de **senha**.

    d. Clique em **Criar**.

### <a name="creating-a-cornerstone-ondemand-test-user"></a>Como criar um usuário de teste do Cornerstone OnDemand

O objetivo desta seção é criar um usuário chamado Brenda Fernandes no Cornerstone OnDemand. O Cornerstone OnDemand dá suporte ao provisionamento automático do usuário, que está habilitado por padrão. Você pode encontrar [aqui](cornerstone-ondemand-provisioning-tutorial.md) mais detalhes de como configurar o provisionamento automático de usuário.

**Se você precisar criar o usuário manualmente, execute as seguintes etapas:**

Para configurar o provisionamento de usuário, envie as informações (por exemplo: Nome, Email) sobre o usuário do Azure AD que você deseja provisionar para a [equipe de suporte do Cornerstone OnDemand](mailTo:moreinfo@csod.com).

>[!NOTE]
>É possível usar qualquer outra ferramenta de criação da conta de usuário do Cornerstone OnDemand ou APIs fornecidas pelo Cornerstone OnDemand para provisionar as contas de usuário do AAD.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuição do usuário de teste do AD do Azure

Nesta seção, você permite que Brenda Fernandes use o logon único do Azure concedendo acesso ao Cornerstone OnDemand.

![Atribuir usuário][200] 

**Para atribuir Brenda Fernandes ao Cornerstone OnDemand, execute as seguintes etapas:**

1. No Portal do Azure, abra a exibição de aplicativos e, em seguida, navegue até a exibição de diretório e vá para **Aplicativos Empresariais** e clique em **Todos os aplicativos**.

    ![Atribuir usuário][201] 

1. Na lista de aplicativos, selecione **Cornerstone OnDemand**.

    ![Configurar o logon único](./media/cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_app.png) 

1. No menu à esquerda, clique em **usuários e grupos**.

    ![Atribuir usuário][202] 

1. Clique no botão **Adicionar**. Em seguida, selecione **usuários e grupos** na **Adicionar atribuição** caixa de diálogo.

    ![Atribuir usuário][203]

1. Em **usuários e grupos** caixa de diálogo, selecione **Britta Simon** na lista de usuários.

1. Clique em **selecione** botão **usuários e grupos** caixa de diálogo.

1. Clique em **atribuir** botão **Adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste do logon único

Nesta seção, você testará sua configuração de logon único do Azure AD usando o Painel de Acesso.

Quando você clicar no bloco do Cornerstone OnDemand no Painel de Acesso, deverá ser automaticamente conectado ao seu aplicativo Cornerstone OnDemand.
Para saber mais sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](tutorial-list.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
* [Configurar Provisionamento de Usuário](cornerstone-ondemand-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/cornerstone-ondemand-tutorial/tutorial_general_01.png
[2]: ./media/cornerstone-ondemand-tutorial/tutorial_general_02.png
[3]: ./media/cornerstone-ondemand-tutorial/tutorial_general_03.png
[4]: ./media/cornerstone-ondemand-tutorial/tutorial_general_04.png

[100]: ./media/cornerstone-ondemand-tutorial/tutorial_general_100.png

[200]: ./media/cornerstone-ondemand-tutorial/tutorial_general_200.png
[201]: ./media/cornerstone-ondemand-tutorial/tutorial_general_201.png
[202]: ./media/cornerstone-ondemand-tutorial/tutorial_general_202.png
[203]: ./media/cornerstone-ondemand-tutorial/tutorial_general_203.png
