---
title: 'Tutorial: Integração do Azure Active Directory ao SafetyNet | Microsoft Docs'
description: Saiba como configurar o logon único entre o Azure Active Directory e o SafetyNet.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: caa96ea2-da21-4529-8fab-0e06367beb40
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/16/2018
ms.author: jeedes
ms.openlocfilehash: 7756e943d25a02b3ec3e5a9150bb5eec8485eda7
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39421525"
---
# <a name="tutorial-azure-active-directory-integration-with-safetynet"></a>Tutorial: Integração do Azure Active Directory do com o SafetyNet

Neste tutorial, você aprenderá a integrar o SafetyNet ao Azure Active Directory (Azure AD).

A integração do SafetyNet ao Azure AD oferece os seguintes benefícios:

- Você pode controlar no Azure AD quem terá acesso ao SafetyNet.
- Você pode permitir que seus usuários façam logon automaticamente no SafetyNet (logon único) com as contas do Microsoft Azure Active Directory deles.
- Você pode gerenciar suas contas em um único local central – o portal do Azure.

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao Azure AD, consulte [o que é o acesso a aplicativos e logon único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD ao SafetyNet, você precisará dos seguintes itens:

- Uma assinatura do AD do Azure
- Uma assinatura do SafetyNet com logon único habilitado

> [!NOTE]
> Para testar as etapas deste tutorial, nós não recomendamos o uso de um ambiente de produção.

Para testar as etapas deste tutorial, você deve seguir estas recomendações:

- Não use o ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, você pode [obter uma versão de avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, você testará o logon único do Azure AD em um ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionar o SafetyNet da Galeria
1. configurar e testar o logon único do AD do Azure

## <a name="adding-safetynet-from-the-gallery"></a>Adicionar o SafetyNet da Galeria
Para configurar a integração do SafetyNet ao Azure AD, você precisará adicionar o SafetyNet da galeria à sua lista de aplicativos SaaS gerenciados.

**Para adicionar o SafetyNet por meio da galeria, execute as seguintes etapas:**

1. No **[Portal do Azure](https://portal.azure.com)**, no painel navegação à esquerda, clique no ícone **Azure Active Directory**. 

    ![O botão Azure Active Directory][1]

1. Navegue até **aplicativos empresariais**. Em seguida, vá para **todos os aplicativos**.

    ![A folha Aplicativos empresariais][2]
    
1. Clique no botão **Novo aplicativo** na parte superior da caixa de diálogo para adicionar o novo aplicativo.

    ![O botão Novo aplicativo][3]

1. Na caixa de pesquisa, digite **SafetyNet**, selecione **SafetyNet** no painel de resultados e, em seguida, clique no botão **Adicionar** para adicionar o aplicativo.

    ![SafetyNet na lista de resultados](./media/safetynet-tutorial/tutorial_safetynet_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar logon único do Azure AD

Nesta seção, você vai configurar e testar o logon único do Azure AD com o SafetyNet com base em um usuário de teste chamado "Brenda Fernandes".

Para que o logon único funcione, o Azure AD precisa saber qual usuário do SafetyNet é equivalente a um usuário do Azure AD. Em outras palavras, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado no SafetyNet.

Para configurar e testar o logon único do Azure AD com o SafetyNet, você precisará concluir os seguintes blocos de construção:

1. **[Configurar o logon único do Azure AD](#configure-azure-ad-single-sign-on)** – para habilitar seus usuários a usar esse recurso.
1. **[Criar um usuário de teste do Azure AD](#create-an-azure-ad-test-user)** – para testar o logon único do Azure AD com Brenda Fernandes.
1. **[Criar de um usuário de teste do SafetyNet](#create-a-safetynet-test-user)** : para ter um equivalente de Brenda Fernandes no SafetyNet que esteja vinculado à representação de usuário no Azure AD.
1. **[Atribuir o usuário de teste do Azure AD](#assign-the-azure-ad-test-user)** – para permitir que Brenda Fernandes use o logon único do Azure AD.
1. **[Teste o logon único](#test-single-sign-on)** – para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o logon único do Azure AD

Nesta seção, você habilitará o logon único do Azure AD no Portal do Azure e configurará o logon único no aplicativo portal SafetyNet.

**Para configurar o logon único do Azure AD com o SafetyNet, execute as seguintes etapas:**

1. No portal do Azure, na página de integração do aplicativo **SafetyNet**, clique em **Logon único**.

    ![Link Configurar logon único][4]

1. Na caixa de diálogo **Logon único**, selecione **Modo** como **Logon baseado em SAML** para habilitar o logon único.
 
    ![Caixa de diálogo Logon único](./media/safetynet-tutorial/tutorial_safetynet_samlbase.png)

1. Na seção **Domínio e URLs do SafetyNet**, realize as seguintes etapas se desejar configurar o aplicativo no modo iniciado pelo **IDP**:

    ![Informações de logon único de Domínio e URLs do SafetyNet](./media/safetynet-tutorial/tutorial_safetynet_url.png)

    a. Na caixa de texto **Identificador**, digite uma URL usando o seguinte padrão: `https://<subdomain>.predictivesolutions.com/sp`

    b. Na caixa de texto **URL de resposta**, digite uma URL no seguinte padrão: `https://<subdomain>.predictivesolutions.com/CRMApp/saml/SSO`

1. Marque **Mostrar configurações avançadas de URL** e realize a seguinte etapa se quiser configurar o aplicativo no modo iniciado pelo **SP**:

    ![Informações de logon único de Domínio e URLs do SafetyNet](./media/safetynet-tutorial/tutorial_safetynet_url1.png)

    Na caixa de texto **URL de Logon**, digite uma URL usando o seguinte padrão: `https://<subdomain>.predictivesolutions.com`
     
    > [!NOTE] 
    > Esses valores não são reais. Atualize esses valores com o Identificador real, a URL de Resposta e a URL de Entrada. Contate a [equipe de suporte do Cliente SafetyNet](mailto:dev@predictivesolutions.com) para obter esses valores.

1. Na seção **Certificado de Autenticação SAML**, clique no botão copiar para copiar a **URL de metadados de federação do aplicativo** e cole-a no bloco de notas.

    ![O link de download do Certificado](./media/safetynet-tutorial/tutorial_safetynet_certificate.png)

1. Clique no botão **Salvar** .

    ![Botão Salvar em Configurar Logon Único](./media/safetynet-tutorial/tutorial_general_400.png)

1. Para configurar o logon único no lado do **SafetyNet**, é necessário enviar a **URL de metadados da federação do aplicativo** para a [equipe de suporte do SafetyNet](mailto:dev@predictivesolutions.com). Eles definem essa configuração para ter a conexão de SSO de SAML definida corretamente em ambos os lados.

### <a name="create-an-azure-ad-test-user"></a>Criar um usuário de teste do Azure AD

O objetivo desta seção é criar um usuário de teste no Portal do Azure chamado Brenda Fernandes.

   ![Criar um usuário de teste do Azure AD][100]

**Para criar um usuário de teste no AD do Azure, execute as seguintes etapas:**

1. No portal do Azure, no painel esquerdo, clique no botão **Azure Active Directory**.

    ![O botão Azure Active Directory](./media/safetynet-tutorial/create_aaduser_01.png)

1. Para exibir a lista de usuários, acesse **Usuários e grupos** e, depois, clique em **Todos os usuários**.

    ![Os links “Usuários e grupos” e “Todos os usuários”](./media/safetynet-tutorial/create_aaduser_02.png)

1. Para abrir a caixa de diálogo **Usuário**, clique em **Adicionar** na parte superior da caixa de diálogo **Todos os Usuários**.

    ![O botão Adicionar](./media/safetynet-tutorial/create_aaduser_03.png)

1. Na caixa de diálogo **Usuário**, execute as seguintes etapas:

    ![A caixa de diálogo Usuário](./media/safetynet-tutorial/create_aaduser_04.png)

    a. Na caixa **Nome**, digite **BrendaFernandes**.

    b. Na caixa **Nome de usuário**, digite o endereço de email do usuário Brenda Fernandes.

    c. Marque a caixa de seleção **Mostrar Senha** e, em seguida, anote o valor exibido na caixa **Senha**.

    d. Clique em **Criar**.
 
### <a name="create-a-safetynet-test-user"></a>Criar um usuário de teste do SafetyNet

Nesta seção, você criará uma usuária chamado Brenda Fernandes no SafetyNet. Trabalhe com a [equipe de suporte do SafetyNet](mailto:dev@predictivesolutions.com) para adicionar os usuários na plataforma do SafetyNet. Os usuários devem ser criados e ativados antes de você usar o logon único

### <a name="assign-the-azure-ad-test-user"></a>Atribuir o usuário de teste do Azure AD

Nesta seção, você permitirá que Brenda Fernandes use o logon único do Azure concedendo-lhe acesso ao SafetyNet.

![Atribuir a função de usuário][200] 

**Para atribuir Brenda Fernandes ao SafetyNet, realize as seguintes etapas:**

1. No Portal do Azure, abra a exibição de aplicativos e, em seguida, navegue até a exibição de diretório e vá para **Aplicativos Empresariais** e clique em **Todos os aplicativos**.

    ![Atribuir usuário][201] 

1. Na lista de aplicativos, selecione **SafetyNet**.

    ![O link do SafetyNet na lista de Aplicativos](./media/safetynet-tutorial/tutorial_safetynet_app.png)  

1. No menu à esquerda, clique em **usuários e grupos**.

    ![O link “Usuários e grupos”][202]

1. Clique no botão **Adicionar**. Em seguida, selecione **usuários e grupos** na **Adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar Atribuição][203]

1. Em **usuários e grupos** caixa de diálogo, selecione **Britta Simon** na lista de usuários.

1. Clique em **selecione** botão **usuários e grupos** caixa de diálogo.

1. Clique em **atribuir** botão **Adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar logon único

Nesta seção, você testará sua configuração de logon único do Azure AD usando o Painel de Acesso.

Ao clicar no bloco do SafetyNet no Painel de Acesso, você deverá ser conectado automaticamente ao seu aplicativo SafetyNet.
Para saber mais sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](tutorial-list.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/safetynet-tutorial/tutorial_general_01.png
[2]: ./media/safetynet-tutorial/tutorial_general_02.png
[3]: ./media/safetynet-tutorial/tutorial_general_03.png
[4]: ./media/safetynet-tutorial/tutorial_general_04.png

[100]: ./media/safetynet-tutorial/tutorial_general_100.png

[200]: ./media/safetynet-tutorial/tutorial_general_200.png
[201]: ./media/safetynet-tutorial/tutorial_general_201.png
[202]: ./media/safetynet-tutorial/tutorial_general_202.png
[203]: ./media/safetynet-tutorial/tutorial_general_203.png

