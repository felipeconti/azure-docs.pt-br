---
title: 'Tutorial: Integração do Azure Active Directory ao Clarizen | Microsoft Docs'
description: Saiba como configurar o logon único entre o Active Directory do Azure e o Clarizen.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: jeedes
ms.openlocfilehash: 510bf383848725f3864c40af02c2b309370237f0
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39438079"
---
# <a name="tutorial-azure-active-directory-integration-with-clarizen"></a>Tutorial: integração do Active Directory do Azure ao Clarizen

Neste tutorial, você aprenderá a integrar o Azure AD (Azure Active Directory) ao Clarizen. Essa integração oferece os seguintes benefícios:

- No Azure AD, é possível controlar quem tem acesso ao Clarizen.
- Você pode permitir que seus usuários entrem automaticamente no Clarizen (logon único) com suas contas do Azure AD.
- Você pode gerenciar suas contas em um único local, o portal clássico do Azure.

O cenário deste tutorial consiste em duas tarefas principais:

1. Adicione o Clarizen da galeria.
1. Configurar e testar logon único do Azure AD.

Para conhecer mais detalhadamente a integração de aplicativos de SaaS (software como serviço) ao Azure AD, consulte [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos
Para configurar a integração do Azure AD ao Clarizen, você precisará dos seguintes itens:

- Uma assinatura do AD do Azure
- Uma assinatura de Clarizen está habilitada para logon único

Para testar as etapas neste tutorial, siga estas recomendações:

- Teste o logon único do Azure AD em um ambiente de teste. Não use o ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de teste do Azure AD, você poderá [obter uma versão de avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="add-clarizen-from-the-gallery"></a>Adicionar o Clarizen da galeria
Para configurar a integração do Clarizen ao Azure AD, adicione o Clarizen da galeria à sua lista de aplicativos SaaS gerenciados.

1. No [portal do Azure](https://portal.azure.com), no painel esquerdo, clique no ícone **Azure Active Directory**.

    ![Ícone do Azure Active Directory][1]

1. Clique em **Aplicativos empresariais**. Em seguida, clique em **Todos os aplicativos**.

    ![Clicar em "Aplicativos corporativos" e em "Todos os aplicativos"][2]

1. Clique no botão **Adicionar** na parte superior da caixa de diálogo.

    ![O botão “Adicionar”][3]

1. Na caixa de pesquisa, digite **Clarizen**.

    ![Digitar "Clarizen" na caixa de pesquisa](./media/clarizen-tutorial/tutorial_clarizen_000.png)

1. No painel de resultados, selecione **Clarizen** e clique em **Adicionar** para adicionar o aplicativo.

    ![Selecionar o Clarizen no painel de resultados](./media/clarizen-tutorial/tutorial_clarizen_0001.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar logon único do Azure AD
Nas seções a seguir, você configurará e testará o logon único do Azure AD com o Clarizen, com base no usuário de teste Brenda Fernandes.

Para que o logon único funcione, o Azure AD precisa saber qual usuário do Clarizen é equivalente a um usuário do Azure AD. Em outras palavras, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado no Clarizen. Você estabelece essa relação de vínculo atribuindo o valor de **nome de usuário** no Azure AD como o valor de **Nome de usuário** no Clarizen.

Para configurar e testar o logon único do Azure AD com o Clarizen, você precisará concluir os seguintes blocos de construção:

1. **[Configure o logon único do Azure AD](#configure-azure-ad-single-sign-on)** para permitir que seus usuários usem esse recurso.
1. **[Crie um usuário de teste do Azure AD](#create-an-azure-ad-test-user)** para testar o logon único do Azure AD com Brenda Fernandes.
1. **[Criar um usuário de teste do Clarizen](#create-a-clarizen-test-user)**: para ter um equivalente de Brenda Fernandes no Clarizen que esteja vinculado à representação dela no Azure AD.
1. **[Atribua o usuário de teste do Azure AD](#assign-the-azure-ad-test-user)** para permitir que Brenda Fernandes use o logon único do Azure AD.
1. **[Teste o logon único](#test-single-sign-on)** para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o logon único do Azure AD
Habilite o logon único do Azure AD no portal do Azure e configura o logon único em seu aplicativo Clarizen.

1. No portal do Azure, na página de integração do aplicativo **Clarizen**, clique em **Logon único**.

    ![Clicar em "Logon único"][4]

1. Na caixa de diálogo **Logon único**, como **Modo**, selecione **Logon baseado em SAML** para habilitar o logon único.

    ![Selecionar "Logon único baseado em SAML"](./media/clarizen-tutorial/tutorial_clarizen_01.png)

1. Na seção **URLs e Domínio do Clarizen**, execute as seguintes etapas:

    ![Caixas de URL de resposta e o identificador](./media/clarizen-tutorial/tutorial_clarizen_02.png)

    a. Na caixa **Identificador**, digite o valor como: **Clarizen**

    b. Na caixa **URL de Resposta**, digite uma URL usando o seguinte padrão: **https://<company name>.clarizen.com/Clarizen/Pages/Integrations/SAML/SamlResponse.aspx**

    > [!NOTE]
    > Esses não são os valores reais. Você precisa usar o identificador real e a URL de resposta. Aqui, sugerimos que você use o valor exclusivo de uma cadeia de caracteres como o identificador. Para obter os valores reais, entre em contato com a [equipe de suporte do Clarizen](https://success.clarizen.com/hc/en-us/requests/new).

1. Na seção **Certificado de Autenticação SAML**, clique em **Criar novo certificado**.

    ![Clicar em "Criar novo certificado"](./media/clarizen-tutorial/tutorial_clarizen_03.png)    

1. Na caixa de diálogo **Criar um Novo Certificado**, clique no ícone de calendário e selecione uma data de expiração. Em seguida, clique em **Salvar**.

    ![Selecionar e salvar uma data de expiração](./media/clarizen-tutorial/tutorial_general_300.png)

1. Na seção **Certificado de Autenticação SAML**, selecione **Ativar o novo certificado** e clique em **Salvar**.

    ![Marcar a caixa de seleção para ativar o novo certificado](./media/clarizen-tutorial/tutorial_clarizen_04.png)

1. Na caixa de diálogo **Certificado de substituição**, clique em **OK**.

    ![Clicar em "OK" para confirmar que você deseja ativar o certificado](./media/clarizen-tutorial/tutorial_general_400.png)

1. Na seção **Certificado de Autenticação SAML**, clique em **Certificado (Base64)** e salve o arquivo do certificado em seu computador.

    ![Clicar em "Certificado (Base64)" para iniciar o download](./media/clarizen-tutorial/tutorial_clarizen_05.png)

1. Na seção **Configuração do Clarizen**, clique em **Configurar o Clarizen** para abrir a janela **Configurar logon**.

    ![Clicar em "Configurar Clarizen"](./media/clarizen-tutorial/tutorial_clarizen_06.png)

    ![Janela "Configurar o logon", incluindo URLs e arquivos](./media/clarizen-tutorial/tutorial_clarizen_07.png)

1. Em outra janela do navegador da Web, entre em seu site de empresa do Clarizen como administrador.

1. Clique no nome de usuário e em **Configurações**.

    ![Clicar em "Configurações" em seu nome de usuário](./media/clarizen-tutorial/tutorial_clarizen_001.png "Configurações")

1. Clique na guia **Configurações Globais**. Em seguida, próximo a **Autenticação Federada**, clique em **editar**.

    ![Guia "Configurações Globais"](./media/clarizen-tutorial/tutorial_clarizen_002.png "Configurações Globais")

1. Na caixa de diálogo **Autenticação Federada**, execute as seguintes etapas:

    ![Caixa de diálogo "Autenticação Federada"](./media/clarizen-tutorial/tutorial_clarizen_003.png "Autenticação Federada")

    a. Selecione **Habilitar Autenticação Federada**.

    b. Clique em **Carregar** para carregar o certificado baixado.

    c. Na caixa de texto **URL de Entrada**, insira o valor da **URL de Serviço de Logon Único SAML** do assistente de configuração de aplicativo do Azure AD.

    d. Na caixa de texto **URL de Saída**, insira o valor da **URL de Saída** do assistente de configuração de aplicativo do Azure AD.

    e. Selecione **Usar POST**.

    f. Clique em **Salvar**.

### <a name="create-an-azure-ad-test-user"></a>Criar um usuário de teste do Azure AD
No portal do Azure, crie um usuário de teste chamado Brenda Fernandes.

![Nome e endereço de email do usuário de teste do Azure AD][100]

1. No portal do Azure, no painel esquerdo, clique no ícone do **Azure Active Directory**.

    ![Ícone do Azure Active Directory](./media/clarizen-tutorial/create_aaduser_01.png)

1. Clique em **Usuários e grupos** e clique em **Todos os usuários** para exibir a lista de usuários.

    ![Clicar em "Usuários e grupos" e "Todos os usuários"](./media/clarizen-tutorial/create_aaduser_02.png)

1. Na parte superior da caixa de diálogo, clique em **Adicionar** para abrir a caixa de diálogo **Usuário**.

    ![O botão “Adicionar”](./media/clarizen-tutorial/create_aaduser_03.png)

1. Na caixa de diálogo **Usuário**, execute as seguintes etapas:

    ![Caixa de diálogo "Usuário" com o nome, p endereço de email e a senha preenchidos](./media/clarizen-tutorial/create_aaduser_04.png)

    a. Na caixa **Nome**, digite **BrendaFernandes**.

    b. Na caixa **Nome de usuário**, digite o endereço de email da conta de Brenda Fernandes.

    c. Selecione **Mostrar Senha** e anote o valor de **Senha**.

    d. Clique em **Criar**.

### <a name="create-a-clarizen-test-user"></a>Criar um usuário de teste do Clarizen

O objetivo desta seção é criar um usuário chamado Brenda Fernandes no Clarizen. O Clarizen dá suporte ao provisionamento automático do usuário, que está habilitado por padrão. Você pode encontrar [aqui](clarizen-provisioning-tutorial.md) mais detalhes de como configurar o provisionamento automático de usuário.

**Se você precisar criar um usuário manualmente, siga estas etapas:**

Para permitir que os usuários do Azure AD entrem no Clarizen, você deverá provisionar contas de usuário. No caso do Clarizen, o provisionamento é uma tarefa manual.

1. Entre em seu site de empresa do Clarizen como administrador.

1. Clique em **Pessoas**.

    ![Clicar em "Pessoas"](./media/clarizen-tutorial/create_aaduser_001.png "Pessoas")

1. Clique em **Convidar Usuário**.

    ![Botão "Convidar Usuário"](./media/clarizen-tutorial/create_aaduser_002.png "Convidar Usuários")

1. Na página **Convidar Pessoas**, execute as seguintes etapas:

    ![Caixa de diálogo "Convidar Pessoas"](./media/clarizen-tutorial/create_aaduser_003.png "Convidar Pessoas")

    a. Na caixa **Email**, digite o endereço de email da conta de Brenda Fernandes.

    b. Clique em **Convidar**.

    > [!NOTE]
    > O titular da conta do Active Directory do Azure receberá um email e seguirá um link para confirmar a conta antes que ela se torne ativa.

### <a name="assign-the-azure-ad-test-user"></a>Atribuir o usuário de teste do Azure AD
Permita que Brenda Fernandes use o logon único do Azure concedendo a ela acesso ao Clarizen.

![Usuário de teste atribuído][200]

1. No portal do Azure, abra a exibição de aplicativos, navegue até a exibição de diretório, clique em **Aplicativos empresariais** e então clique em **Todos os aplicativos**.

    ![Clicar em "Aplicativos corporativos" e em "Todos os aplicativos"][201]

1. Na lista de aplicativos, selecione **Clarizen**.

    ![Selecionar Clarizen na lista](./media/clarizen-tutorial/tutorial_clarizen_50.png)

1. No painel esquerdo, clique em **Usuários e grupos**.

    ![Clicar em “Usuário e grupos”][202]

1. Clique no botão **Adicionar** . Em seguida, na caixa de diálogo **Adicionar Atribuição**, selecione **Usuários e grupos**.

    ![O botão "Adicionar" e a caixa de diálogo "Adicionar Atribuição"][203]

1. Na caixa de diálogo **Usuários e grupos**, selecione **Brenda Fernandes** na lista de usuários.

1. Na caixa de diálogo **Usuários e grupos**, clique no botão **Selecionar**.

1. Na caixa de diálogo **Adicionar Atribuição**, clique no botão **Atribuir**.

### <a name="test-single-sign-on"></a>Testar logon único
Teste sua configuração de logon único do Azure AD usando o Painel de Acesso.

Ao clicar no bloco do Clarizen no Painel de Acesso, você deve ser conectado automaticamente ao aplicativo do Clarizen.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicativos SaaS ao Active Directory do Azure](tutorial-list.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
* [Configurar Provisionamento de Usuário](clarizen-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/clarizen-tutorial/tutorial_general_01.png
[2]: ./media/clarizen-tutorial/tutorial_general_02.png
[3]: ./media/clarizen-tutorial/tutorial_general_03.png
[4]: ./media/clarizen-tutorial/tutorial_general_04.png

[100]: ./media/clarizen-tutorial/tutorial_general_100.png

[200]: ./media/clarizen-tutorial/tutorial_general_200.png
[201]: ./media/clarizen-tutorial/tutorial_general_201.png
[202]: ./media/clarizen-tutorial/tutorial_general_202.png
[203]: ./media/clarizen-tutorial/tutorial_general_203.png