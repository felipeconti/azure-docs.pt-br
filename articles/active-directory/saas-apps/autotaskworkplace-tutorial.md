---
title: 'Tutorial: Integração do Azure Active Directory ao Autotask Workplace | Microsoft Docs'
description: Saiba como configurar o logon único entre o Azure Active Directory e o Autotask Workplace.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: a9a7ff71-c389-4169-aafd-d7a505244797
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: cc1ee04c9d614e895c4e8a021564e9b9405fa8c0
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39438953"
---
# <a name="tutorial-azure-active-directory-integration-with-autotask-workplace"></a>Tutorial: Integração do Azure Active Directory ao Autotask Workplace

Neste tutorial, você aprende a integrar o Autotask Workplace ao Azure AD (Azure Active Directory).

A integração do Autotask Workplace ao Azure AD oferece os seguintes benefícios:

- No Azure AD, é possível controlar quem tem acesso ao Autotask Workplace
- É possível permitir que os usuários se conectem automaticamente ao Autotask Workplace (Logon Único) com suas contas do Azure AD
- Você pode gerenciar suas contas em um única localização: o Portal do Azure

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao Azure AD, consulte [o que é o acesso a aplicativos e logon único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD ao Autotask Workplace, você precisa dos seguintes itens:

- Uma assinatura do AD do Azure
- Uma assinatura habilitada para logon único do Autotask Workplace
- Você deve ser um administrador ou superadministrador no local de trabalho.
- Você deve ter uma conta Administrador no Azure AD.
- Os usuários que utilizam esse recurso devem ter contas no local de trabalho e no Azure AD e os endereços de email deles para ambos devem corresponder.

> [!NOTE]
> Para testar as etapas deste tutorial, nós não recomendamos o uso de um ambiente de produção.

Para testar as etapas deste tutorial, você deve seguir estas recomendações:

- Não use o ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, você pode [obter uma versão de avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, você testará o logon único do Azure AD em um ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando o Autotask Workplace por meio da galeria
1. configurar e testar o logon único do AD do Azure

## <a name="adding-autotask-workplace-from-the-gallery"></a>Adicionando o Autotask Workplace por meio da galeria
Para configurar a integração do Autotask Workplace ao Azure AD, você precisa adicionar o Autotask Workplace à lista de aplicativos SaaS gerenciados por meio da galeria.

**Para adicionar o Autotask Workplace por meio da galeria, realize as seguintes etapas:**

1. No **[Portal do Azure](https://portal.azure.com)**, no painel navegação à esquerda, clique no ícone **Azure Active Directory**. 

    ![O botão Azure Active Directory][1]

1. Navegue até **aplicativos empresariais**. Em seguida, vá para **todos os aplicativos**.

    ![A folha Aplicativos empresariais][2]
    
1. Clique no botão **Novo aplicativo** na parte superior da caixa de diálogo para adicionar o novo aplicativo.

    ![O botão Novo aplicativo][3]

1. Na caixa de pesquisa, digite **Autotask Workplace**, selecione **Autotask Workplace** no painel de resultados e, depois, clique no botão **Adicionar** para adicionar o aplicativo.

    ![Autotask Workplace na lista de resultados](./media/autotaskworkplace-tutorial/tutorial_autotaskworkplace_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar logon único do Azure AD

Nesta seção, você configura e testa o logon único do Azure AD com o Autotask Workplace, com base em um usuário de teste chamado “Brenda Fernandes”.

Para que o logon único funcione, o Azure AD precisa saber qual usuário do Autotask Workplace é equivalente a um usuário do Azure AD. Em outras palavras, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado do Autotask Workplace.

No Autotask Workplace, atribua o valor do **nome de usuário** no Azure AD como o valor do **Nome de usuário** para estabelecer a relação de vínculo.

Para configurar e testar o logon único do Azure AD com o Autotask Workplace, você precisa concluir os seguintes blocos de construção:

1. **[Configurar o logon único do Azure AD](#configure-azure-ad-single-sign-on)** – para habilitar seus usuários a usar esse recurso.
1. **[Criar um usuário de teste do Azure AD](#create-an-azure-ad-test-user)** – para testar o logon único do Azure AD com Brenda Fernandes.
1. **[Criar um usuário de teste do Autotask Workplace](#create-an-autotask-workplace-test-user)** – para ter um equivalente de Brenda Fernandes no Autotask Workplace vinculado à representação do usuário no Azure AD.
1. **[Atribuir o usuário de teste do Azure AD](#assign-the-azure-ad-test-user)** – para permitir que Brenda Fernandes use o logon único do Azure AD.
1. **[Testar o logon único](#test-single-sign-on)** – para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o logon único do Azure AD

Nesta seção, você habilita o logon único do Azure AD no portal do Azure e configura o logon único no aplicativo Autotask Workplace.

**Para configurar o logon único do Azure AD com o Autotask Workplace, realize as seguintes etapas:**

1. No portal do Azure, na página de integração do aplicativo do **Autotask Workplace**, clique em **Logon único**.

    ![Link Configurar logon único][4]

1. Na caixa de diálogo **Logon único**, selecione **Modo** como **Logon baseado em SAML** para habilitar o logon único.
 
    ![Caixa de diálogo Logon único](./media/autotaskworkplace-tutorial/tutorial_autotaskworkplace_samlbase.png)

1. Se desejar configurar o aplicativo no modo iniciado pelo **IDP**, realize as seguintes etapas na seção **Domínio e URLs do Autotask Workplace**:

    ![Informações de logon único de Domínio e URLs do Autotask Workplace para IdP](./media/autotaskworkplace-tutorial/tutorial_autotaskworkplace_url.png)

    a. Na caixa de texto **Identificador**, digite uma URL usando o seguinte padrão: `https://<subdomain>.awp.autotask.net/singlesignon/saml/metadata`

    b. Na caixa de texto **URL de resposta**, digite uma URL no seguinte padrão: `https://<subdomain>.awp.autotask.net/singlesignon/saml/SSO`

1. Se desejar configurar o aplicativo no modo iniciado pelo **SP**, marque a opção **Mostrar configurações de URL avançadas** e realize as seguintes etapas:

    ![Informações de logon único de Domínio e URLs do Autotask Workplace para SP](./media/autotaskworkplace-tutorial/tutorial_autotaskworkplace_url1.png)

    Na caixa de texto **URL de Logon**, digite uma URL usando o seguinte padrão: `https://<subdomain>.awp.autotask.net/loginsso`
     
    > [!NOTE] 
    > Esses valores não são reais. Atualize esses valores com o Identificador real, a URL de Resposta e a URL de Entrada. Contate a [equipe de suporte ao Cliente do Autotask Workplace](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) para obter esses valores. 

1. Na seção **Certificado de Autenticação SAML**, clique em **Metadados XML** e, em seguida, salve o arquivo de metadados em seu computador.

    ![O link de download do Certificado](./media/autotaskworkplace-tutorial/tutorial_autotaskworkplace_certificate.png) 

1. Clique no botão **Salvar** .

    ![Botão Salvar em Configurar Logon Único](./media/autotaskworkplace-tutorial/tutorial_general_400.png)

1. Em uma janela de navegador da Web diferente, faça logon no Workplace Online usando as credenciais de administrador.

    >[!Note]
    >Ao configurar o IdP, um subdomínio precisará ser especificado. Para confirmar o subdomínio correto, faça logon no Workplace Online. Depois de conectado, tome nota do subdomínio na URL.
    >O subdomínio é a parte entre o "https://" e ".awp.autotask.net/" e deve ser us, eu, ca or au.

1. Vá até **Configuração** > **Logon Único** e execute as seguintes etapas:

    ![Configuração de Logon Único do Autotask](./media/autotaskworkplace-tutorial/tutorial_autotaskssoconfig1.png)
 
    a. Selecione a opção **Arquivo de Metadados XML** e, em seguida, carregue o **XML de Metadados** baixado do Portal do Azure.

    b. Clique em **Habilitar SSO**.
    
    ![Configuração de aprovação de Logon Único do Autotask](./media/autotaskworkplace-tutorial/tutorial_autotaskssoconfig2.png)

    c. Selecione a caixa de seleção **Confirmo que essas informações estão corretas e eu confio neste IdP**.

    d. Clique em **Aprovar**.
     
>[!Note]
>Se você precisar de assistência para configurar o Autotask Workplace, consulte [essa página](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) para obter ajuda com sua conta do Workplace.

> [!TIP]
> É possível ler uma versão concisa dessas instruções no [Portal do Azure](https://portal.azure.com), enquanto você estiver configurando o aplicativo!  Depois de adicionar esse aplicativo da seção **Active Directory > Aplicativos Empresariais**, basta clicar na guia **Logon Único** e acessar a documentação inserida por meio da seção **Configuração** na parte inferior. Saiba mais sobre a funcionalidade de documentação inserida aqui: [Documentação inserida do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Criar um usuário de teste do Azure AD

O objetivo desta seção é criar um usuário de teste no Portal do Azure chamado Brenda Fernandes.

   ![Criar um usuário de teste do Azure AD][100]

**Para criar um usuário de teste no AD do Azure, execute as seguintes etapas:**

1. No portal do Azure, no painel esquerdo, clique no botão **Azure Active Directory**.

    ![O botão Azure Active Directory](./media/autotaskworkplace-tutorial/create_aaduser_01.png)

1. Para exibir a lista de usuários, acesse **Usuários e grupos** e, depois, clique em **Todos os usuários**.

    ![Os links “Usuários e grupos” e “Todos os usuários”](./media/autotaskworkplace-tutorial/create_aaduser_02.png)

1. Para abrir a caixa de diálogo **Usuário**, clique em **Adicionar** na parte superior da caixa de diálogo **Todos os Usuários**.

    ![O botão Adicionar](./media/autotaskworkplace-tutorial/create_aaduser_03.png)

1. Na caixa de diálogo **Usuário**, execute as seguintes etapas:

    ![A caixa de diálogo Usuário](./media/autotaskworkplace-tutorial/create_aaduser_04.png)

    a. Na caixa **Nome**, digite **BrendaFernandes**.

    b. Na caixa **Nome de usuário**, digite o endereço de email do usuário Brenda Fernandes.

    c. Marque a caixa de seleção **Mostrar Senha** e, em seguida, anote o valor exibido na caixa **Senha**.

    d. Clique em **Criar**.

### <a name="create-an-autotask-workplace-test-user"></a>Criar um usuário de teste do Autotask Workplace

Nesta seção, você criará uma usuária chamada Brenda Fernandes no Autotask. Trabalhe com a [equipe de suporte do Autotask Workplace](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) para adicionar os usuários à plataforma Autotask Workplace.

### <a name="assign-the-azure-ad-test-user"></a>Atribuir o usuário de teste do Azure AD

Nesta seção, você permite que Brenda Fernandes use o logon único do Azure concedendo acesso ao Autotask Workplace.

![Atribuir a função de usuário][200] 

**Para atribuir Brenda Fernandes ao Autotask Workplace, realize as seguintes etapas:**

1. No Portal do Azure, abra a exibição de aplicativos e, em seguida, navegue até a exibição de diretório e vá para **Aplicativos Empresariais** e clique em **Todos os aplicativos**.

    ![Atribuir usuário][201] 

1. Na lista de aplicativos, selecione **Autotask Workplace**.

    ![O link do Autotask Workplace na lista de Aplicativos](./media/autotaskworkplace-tutorial/tutorial_autotaskworkplace_app.png) 

1. No menu à esquerda, clique em **usuários e grupos**.

    ![O link “Usuários e grupos”][202]

1. Clique no botão **Adicionar**. Em seguida, selecione **usuários e grupos** na **Adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar Atribuição][203]

1. Em **usuários e grupos** caixa de diálogo, selecione **Britta Simon** na lista de usuários.

1. Clique em **selecione** botão **usuários e grupos** caixa de diálogo.

1. Clique em **atribuir** botão **Adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar logon único

Nesta seção, você testará sua configuração de logon único do Azure AD usando o Painel de Acesso.

Quando você clicar no bloco Autotask Workplace no Painel de Acesso, deverá ser automaticamente conectado ao aplicativo Autotask Workplace.
Para saber mais sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](tutorial-list.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)


<!--Image references-->

[1]: ./media/autotaskworkplace-tutorial/tutorial_general_01.png
[2]: ./media/autotaskworkplace-tutorial/tutorial_general_02.png
[3]: ./media/autotaskworkplace-tutorial/tutorial_general_03.png
[4]: ./media/autotaskworkplace-tutorial/tutorial_general_04.png

[100]: ./media/autotaskworkplace-tutorial/tutorial_general_100.png

[200]: ./media/autotaskworkplace-tutorial/tutorial_general_200.png
[201]: ./media/autotaskworkplace-tutorial/tutorial_general_201.png
[202]: ./media/autotaskworkplace-tutorial/tutorial_general_202.png
[203]: ./media/autotaskworkplace-tutorial/tutorial_general_203.png

