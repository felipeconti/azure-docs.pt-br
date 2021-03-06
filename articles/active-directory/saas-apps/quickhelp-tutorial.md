---
title: 'Tutorial: Integração do Azure Active Directory com o QuickHelp | Microsoft Docs'
description: Saiba como configurar o logon único entre o Active Directory do Azure e o QuickHelp.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 655c9ad3-2076-4e2c-8e47-9ed3bf04be56
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2017
ms.author: jeedes
ms.openlocfilehash: c99be60301085dddfd5c658ee1eed81b88238e54
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39421579"
---
# <a name="tutorial-azure-active-directory-integration-with-quickhelp"></a>Tutorial: Integração do Active Directory do Azure com o QuickHelp

Neste tutorial, você aprende a integrar o QuickHelp ao Azure AD (Azure Active Directory).

A integração do QuickHelp ao Azure AD oferece os seguintes benefícios:

- Você pode controlar no AD do Azure quem tem acesso ao QuickHelp
- Você pode habilitar seus usuários a fazerem logon automaticamente no QuickHelp (logon único) com suas contas do AD do Azure
- Você pode gerenciar suas contas em um única localização: o Portal do Azure

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao Azure AD, consulte [o que é o acesso a aplicativos e logon único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do AD do Azure ao QuickHelp, você precisa dos seguintes itens:

- Uma assinatura do AD do Azure
- Uma assinatura habilitada para logon único do QuickHelp

> [!NOTE]
> Para testar as etapas deste tutorial, nós não recomendamos o uso de um ambiente de produção.

Para testar as etapas deste tutorial, você deve seguir estas recomendações:

- Não use o ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do AD do Azure, você pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, você testará o logon único do Azure AD em um ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionar QuickHelp a partir da galeria
1. configurar e testar o logon único do AD do Azure

## <a name="adding-quickhelp-from-the-gallery"></a>Adicionar QuickHelp a partir da galeria
Para configurar a integração do QuickHelp ao AD do Azure, você precisa adicionar o QuickHelp a partir da galeria à sua lista de aplicativos de SaaS gerenciados.

**Para adicionar o QuickHelp a partir da galeria, execute as seguintes etapas:**

1. No **[Portal do Azure](https://portal.azure.com)**, no painel navegação à esquerda, clique no ícone **Azure Active Directory**. 

    ![Active Directory][1]

1. Navegue até **aplicativos empresariais**. Em seguida, vá para **todos os aplicativos**.

    ![APLICATIVOS][2]
    
1. Clique no botão **Novo aplicativo** na parte superior da caixa de diálogo para adicionar o novo aplicativo.

    ![APLICATIVOS][3]

1. Na caixa de pesquisa, digite **QuickHelp**.

    ![Criação de um usuário de teste do AD do Azure](./media/quickhelp-tutorial/tutorial_quickhelp_search.png)

1. No painel de resultados, selecione **QuickHelp** e, depois, clique no botão **Adicionar** para adicionar o aplicativo.

    ![Criação de um usuário de teste do AD do Azure](./media/quickhelp-tutorial/tutorial_quickhelp_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>configurar e testar o logon único do AD do Azure
Nesta seção, você configura e testa o logon único do Azure AD com o QuickHelp, com base em um usuário de teste chamado “Brenda Fernandes”.

Para que o logon único funcione, o Azure AD precisa saber qual usuário do QuickHelp é equivalente a um usuário do Azure AD. Em outras palavras, é necessário estabelecer uma relação de vínculo entre um usuário do AD do Azure e o usuário relacionado no QuickHelp.

No QuickHelp, atribua o valor do **nome de usuário** no Azure AD como o valor do **Nome de usuário** para estabelecer a relação de vínculo.

Para configurar e testar o logon único do AD do Azure com o QuickHelp, você precisa concluir os seguintes blocos de construção:

1. **[Configuração do logon único do AD do Azure](#configuring-azure-ad-single-sign-on)** - para permitir que seus usuários usem esse recurso.
1. **[Criação de um usuário de teste do AD do Azure](#creating-an-azure-ad-test-user)** – para testar o logon único do AD do Azure com Brenda Fernandes.
1. **[Criando um usuário de teste do QuickHelp](#creating-a-quickhelp-test-user)** – para ter um equivalente de Brenda Fernandes no QuickHelp que esteja vinculado à representação de usuário do Azure AD.
1. **[Atribuição do usuário de teste do AD do Azure](#assigning-the-azure-ad-test-user)** – para permitir que Brenda Fernandes use o logon único do AD do Azure.
1. **[Teste do logon único](#testing-single-sign-on)** : para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do logon único do Azure AD

Nesta seção, você habilita o logon único do Azure AD no portal do Azure e configura o logon único no aplicativo QuickHelp.

**Para configurar o logon único do AD do Azure com o QuickHelp, execute as seguintes etapas:**

1. No portal do Azure, na página de integração do aplicativo **QuickHelp**, clique em **Logon único**.

    ![Configurar o logon único][4]

1. Na caixa de diálogo **Logon único**, selecione **Modo** como **Logon baseado em SAML** para habilitar o logon único.
 
    ![Configurar o logon único](./media/quickhelp-tutorial/tutorial_quickhelp_samlbase.png)

1. Na seção **Domínio e URLs do QuickHelp**, realize as seguintes etapas:

    ![Configurar o logon único](./media/quickhelp-tutorial/tutorial_quickhelp_url.png)

    a. Na caixa de texto **URL de Logon**, digite uma URL usando o seguinte padrão: `https://quickhelp.com/<ROUTEURL>`

    b. Na caixa de texto **Identificador**, digite uma URL: `https://auth.quickhelp.com`

    > [!NOTE] 
    > O valor da URL de logon não é real. Atualize o valor com a URL de Logon real. Contate o administrador da ajuda rápida da organização ou o gerente de sucesso de cliente BrainStorm para obter o valor.
 
1. Na seção **Certificado de Autenticação SAML**, clique em **Metadados XML** e, em seguida, salve o arquivo de metadados em seu computador.

    ![Configurar o logon único](./media/quickhelp-tutorial/tutorial_quickhelp_certificate.png) 

1. Clique no botão **Salvar** .

    ![Configurar o logon único](./media/quickhelp-tutorial/tutorial_general_400.png) 

1. Faça logon no site do QuickHelp da sua empresa como administrador.

1. No menu na parte superior, clique em **Administrador**.
   
    ![Configurar o logon único][21]

1. No menu **Administrador do QuickHelp**, clique em **Configurações**.
   
    ![Configurar o logon único][22]

1. Clique em **Configurações da Autenticação**.

1. Na página **Configurações de Autenticação** , execute as etapas a seguir
   
    ![Configurar o logon único][23]
   
    a. Como **Tipo de SSO**, selecione **WSFederation**.
   
    b. Para carregar o arquivo de metadados do Azure baixado, clique em **Procurar**, navegue até o arquivo e clique em **Carregar Metadados**.
   
    c. Na caixa de texto **Email**, digite `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.
   
    d. Na caixa de texto **Nome**, `type http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.
   
    e. Na caixa de texto **Sobrenome**, digite `type http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.
   
    f. Na **Barra de Ações**, clique em **Salvar**.

### <a name="creating-an-azure-ad-test-user"></a>Criação de um usuário de teste do AD do Azure
O objetivo desta seção é criar um usuário de teste no Portal do Azure chamado Brenda Fernandes.

![Criar um usuário do AD do Azure][100]

**Para criar um usuário de teste no AD do Azure, execute as seguintes etapas:**

1. No **Portal do Azure**, no painel de navegação esquerdo, clique no ícone **Azure Active Directory**.

    ![Criação de um usuário de teste do AD do Azure](./media/quickhelp-tutorial/create_aaduser_01.png) 

1. Vá para **Usuários e grupos** e clique em **Todos os usuários** para exibir a lista de usuários.
    
    ![Criação de um usuário de teste do AD do Azure](./media/quickhelp-tutorial/create_aaduser_02.png) 

1. Para abrir a caixa de diálogo **Usuário**, clique em **Adicionar** na parte superior da caixa de diálogo.
 
    ![Criação de um usuário de teste do AD do Azure](./media/quickhelp-tutorial/create_aaduser_03.png) 

1. Na página do diálogo **Usuário**, execute as seguintes etapas:
 
    ![Criação de um usuário de teste do AD do Azure](./media/quickhelp-tutorial/create_aaduser_04.png) 

    a. Na caixa de texto **Nome**, digite **Brenda Fernandes**.

    b. Na caixa de texto **Nome de usuário**, digite o **endereço de email** da conta de Brenda Fernandes.

    c. Selecione **Mostrar senha** e anote o valor de **senha**.

    d. Clique em **Criar**.
 
### <a name="creating-a-quickhelp-test-user"></a>Criando um usuário de teste do QuickHelp

O objetivo desta seção é criar um usuário chamado Brenda Fernandes no QuickHelp.
Para que o logon único funcione, o Azure AD precisa saber qual usuário do QuickHelp é equivalente a um usuário do Azure AD. Em outras palavras, é necessário estabelecer uma relação de vínculo entre um usuário do AD do Azure e o usuário relacionado no QuickHelp.

O QuickHelp dá suporte ao provisionamento just-in-time. Isso significa que, se necessário, uma conta de usuário é criada automaticamente no QuickHelp e a conta é vinculada à conta do Azure AD.

Não há itens de ação para você nesta seção.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuição do usuário de teste do AD do Azure

Nesta seção, você permite que Brenda Fernandes use o logon único do Azure concedendo acesso ao QuickHelp.

![Atribuir usuário][200] 

**Para atribuir Brenda Fernandes ao QuickHelp, execute as seguintes etapas:**

1. No Portal do Azure, abra a exibição de aplicativos e, em seguida, navegue até a exibição de diretório e vá para **Aplicativos Empresariais** e clique em **Todos os aplicativos**.

    ![Atribuir usuário][201] 

1. Na lista de aplicativos, selecione **QuickHelp**.

    ![Configurar o logon único](./media/quickhelp-tutorial/tutorial_quickhelp_app.png) 

1. No menu à esquerda, clique em **usuários e grupos**.

    ![Atribuir usuário][202] 

1. Clique no botão **Adicionar**. Em seguida, selecione **usuários e grupos** na **Adicionar atribuição** caixa de diálogo.

    ![Atribuir usuário][203]

1. Em **usuários e grupos** caixa de diálogo, selecione **Britta Simon** na lista de usuários.

1. Clique em **selecione** botão **usuários e grupos** caixa de diálogo.

1. Clique em **atribuir** botão **Adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste do logon único

O objetivo desta seção é testar sua configuração de logon único do Azure AD usando o Painel de Acesso.  

Quando você clica no bloco QuickHelp no Painel de Acesso, deve fazer logon automaticamente no seu aplicativo QuickHelp.


## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](tutorial-list.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/quickhelp-tutorial/tutorial_general_01.png
[2]: ./media/quickhelp-tutorial/tutorial_general_02.png
[3]: ./media/quickhelp-tutorial/tutorial_general_03.png
[4]: ./media/quickhelp-tutorial/tutorial_general_04.png

[100]: ./media/quickhelp-tutorial/tutorial_general_100.png

[200]: ./media/quickhelp-tutorial/tutorial_general_200.png
[201]: ./media/quickhelp-tutorial/tutorial_general_201.png
[202]: ./media/quickhelp-tutorial/tutorial_general_202.png
[203]: ./media/quickhelp-tutorial/tutorial_general_203.png
[21]: ./media/quickhelp-tutorial/tutorial_quickhelp_05.png
[22]: ./media/quickhelp-tutorial/tutorial_quickhelp_06.png
[23]: ./media/quickhelp-tutorial/tutorial_quickhelp_07.png
