---
title: 'Tutorial: Integração do Azure Active Directory com o Splunk Enterprise e o Splunk Cloud | Microsoft Docs'
description: Saiba como configurar o logon único entre o Azure Active Directory e o Splunk Enterprise e o Splunk Cloud.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: b3e2b4a9-749c-4895-813d-db46f8dfdbf8
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/02/2017
ms.author: jeedes
ms.openlocfilehash: b1eb8f198b0d9e25a6514e19794c9b1d54a4cd3e
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39436076"
---
# <a name="tutorial-azure-active-directory-integration-with-splunk-enterprise-and-splunk-cloud"></a>Tutorial: Integração do Azure Active Directory com o Splunk Enterprise e o Splunk Cloud

Neste tutorial, você aprenderá como integrar o Splunk Enterprise e o Splunk Cloud ao Azure AD (Azure Active Directory).

A integração do Splunk Enterprise e do Splunk Cloud ao Azure AD oferece os seguintes benefícios:

- Você pode controlar no Azure AD quem tem acesso ao Splunk Enterprise e ao Splunk Cloud
- Você pode habilitar seus usuários a fazerem logon automaticamente no Splunk Enterprise e no Splunk Cloud (Logon Único) com suas contas do Azure AD
- Você pode gerenciar suas contas em um única localização: o Portal do Azure

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao Azure AD, consulte [o que é o acesso a aplicativos e logon único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o Splunk Enterprise e o Splunk Cloud, você precisará dos seguintes itens:

- Uma assinatura do AD do Azure
- Uma assinatura do Splunk Enterprise e do Splunk Cloud habilitada para logon único

> [!NOTE]
> Para testar as etapas deste tutorial, nós não recomendamos o uso de um ambiente de produção.

Para testar as etapas deste tutorial, você deve seguir estas recomendações:

- Não use o ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do AD do Azure, você pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, você testará o logon único do Azure AD em um ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionar o Splunk Enterprise e o Splunk Cloud da galeria
1. configurar e testar o logon único do AD do Azure

## <a name="adding-splunk-enterprise-and-splunk-cloud-from-the-gallery"></a>Adicionar o Splunk Enterprise e o Splunk Cloud da galeria
Para configurar a integração do Splunk Enterprise e do Splunk Cloud com o Azure AD, você precisará adicionar o Splunk Enterprise e o Splunk Cloud à sua lista de aplicativos SaaS gerenciados por meio da galeria.

**Para adicionar o Splunk Enterprise e o Splunk Cloud por meio da galeria, execute as seguintes etapas:**

1. No **[Portal do Azure](https://portal.azure.com)**, no painel navegação à esquerda, clique no ícone **Azure Active Directory**. 

    ![Active Directory][1]

1. Navegue até **aplicativos empresariais**. Em seguida, vá para **todos os aplicativos**.

    ![APLICATIVOS][2]
    
1. Clique no botão **Novo aplicativo** na parte superior da caixa de diálogo para adicionar o novo aplicativo.

    ![APLICATIVOS][3]

1. Na caixa de pesquisa, digite **Splunk Enterprise e Splunk Cloud**.

    ![Criação de um usuário de teste do AD do Azure](./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_splunkenterpriseandsplunkcloud_search.png)

1. No painel de resultados, selecione **Splunk Enterprise e Splunk Cloud** e clique em **Adicionar** para adicionar o aplicativo.

    ![Criação de um usuário de teste do AD do Azure](./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_splunkenterpriseandsplunkcloud_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>configurar e testar o logon único do AD do Azure
Nesta seção, você configurará e testará o logon único do Azure AD com o Splunk Enterprise e o Splunk Cloud, com base em uma usuária de teste chamada "Brenda Fernandes".

Para que o logon único funcione, o Azure AD precisa saber qual usuário do Splunk Enterprise e do Splunk Cloud é equivalente a um usuário do Azure AD. Em outras palavras, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado do Splunk Enterprise e do Splunk Cloud.

No Splunk Enterprise e no Splunk Cloud, atribua o valor do **nome de usuário** no Azure AD como o valor do **Nome de usuário** para estabelecer a relação de vínculo.

Para configurar e testar o logon único do Azure AD com o Splunk Enterprise e o Splunk Cloud, você precisa concluir os seguintes blocos de construção:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - para habilitar seus usuários a usar esse recurso.
1. **[Criação de um usuário de teste do AD do Azure](#creating-an-azure-ad-test-user)** – para testar o logon único do AD do Azure com Brenda Fernandes.
1. **[Criação de um usuário de teste do Splunk Enterprise e do Splunk Cloud](#creating-a-splunk-enterprise-and-splunk-cloud-test-user)** – para ter um equivalente de Brenda Fernandes no Splunk Enterprise e no Splunk Cloud que esteja vinculado à representação dela no Azure AD.
1. **[Atribuição do usuário de teste do AD do Azure](#assigning-the-azure-ad-test-user)** – para permitir que Brenda Fernandes use o logon único do AD do Azure.
1. **[Teste do logon único](#testing-single-sign-on)** : para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do logon único do Azure AD

Nesta seção, você habilitará o logon único do Azure AD no portal do Azure e configurará o logon único em seu aplicativo Splunk Enterprise e Splunk Cloud.

**Para configurar o logon único do Azure AD com o Splunk Enterprise e o Splunk Cloud, execute as seguintes etapas:**

1. No portal do Azure, na página de integração de aplicativos do **Splunk Enterprise e Splunk Cloud**, clique em **Logon único**.

    ![Configurar o logon único][4]

1. Na caixa de diálogo **Logon único**, selecione **Modo** como **Logon baseado em SAML** para habilitar o logon único.
 
    ![Configurar o logon único](./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_splunkenterpriseandsplunkcloud_samlbase.png)

1. Na seção **Domínio e URLs do Splunk Enterprise e Splunk Cloud**, se você desejar configurar o aplicativo no modo iniciado pelo **IDP**:

    ![Configurar o logon único](./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_splunkenterpriseandsplunkcloud_url.png)

    a. Na caixa de texto **URL de Logon**, digite uma URL usando o seguinte padrão: `https://<splunkserverUrl>/en-US/app/launcher/home`
    
    b. Na caixa de texto **Identificador**, digite a URL de seu Servidor Splunk.

    c. Na caixa de texto **URL de resposta**, digite uma URL no seguinte padrão: `https://<splunkserver>/saml/acs`

    > [!NOTE] 
    > Esses valores não são reais. Atualize esses valores com o Identificador real, a URL de Resposta e a URL de Entrada. Aqui, sugerimos que você use o valor exclusivo de cadeia de caracteres no Identificador. Contate a [equipe de suporte ao Cliente do Splunk Enterprise e Splunk Cloud](https://www.splunk.com/content/splunkcom/en_us/about-us/contact.html#tabs/customer-support) para obter esses valores. 

1. Na seção **Certificado de Autenticação SAML**, clique em **Metadados XML** e, em seguida, salve o arquivo de metadados em seu computador.

    ![Configurar o logon único](./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_splunkenterpriseandsplunkcloud_certificate.png) 

1. Clique no botão **Salvar** .

    ![Configurar o logon único](./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_general_400.png)

1. Para configurar o logon único no lado do **Splunk Enterprise e Splunk Cloud**, você precisa enviar o **XML de metadados** baixado para a [equipe de suporte do Splunk Enterprise e do Splunk Cloud](https://www.splunk.com/content/splunkcom/en_us/about-us/contact.html#tabs/customer-support).

> [!TIP]
> É possível ler uma versão concisa dessas instruções no [Portal do Azure](https://portal.azure.com), enquanto você estiver configurando o aplicativo!  Depois de adicionar esse aplicativo da seção **Active Directory > Aplicativos Empresariais**, basta clicar na guia **Logon Único** e acessar a documentação inserida por meio da seção **Configuração** na parte inferior. Saiba mais sobre a funcionalidade de documentação inserida aqui: [Documentação inserida do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Criação de um usuário de teste do AD do Azure
O objetivo desta seção é criar um usuário de teste no Portal do Azure chamado Brenda Fernandes.

![Criar um usuário do AD do Azure][100]

**Para criar um usuário de teste no AD do Azure, execute as seguintes etapas:**

1. No **Portal do Azure**, no painel de navegação esquerdo, clique no ícone **Azure Active Directory**.

    ![Criação de um usuário de teste do AD do Azure](./media/splunkenterpriseandsplunkcloud-tutorial/create_aaduser_01.png) 

1. Vá para **Usuários e grupos** e clique em **Todos os usuários** para exibir a lista de usuários.
    
    ![Criação de um usuário de teste do AD do Azure](./media/splunkenterpriseandsplunkcloud-tutorial/create_aaduser_02.png) 

1. Para abrir a caixa de diálogo **Usuário**, clique em **Adicionar** na parte superior da caixa de diálogo.
 
    ![Criação de um usuário de teste do AD do Azure](./media/splunkenterpriseandsplunkcloud-tutorial/create_aaduser_03.png) 

1. Na página do diálogo **Usuário**, execute as seguintes etapas:
 
    ![Criação de um usuário de teste do AD do Azure](./media/splunkenterpriseandsplunkcloud-tutorial/create_aaduser_04.png) 

    a. Na caixa de texto **Nome**, digite **Brenda Fernandes**.

    b. Na caixa de texto **Nome de usuário**, digite o **endereço de email** da conta de Brenda Fernandes.

    c. Selecione **Mostrar senha** e anote o valor de **senha**.

    d. Clique em **Criar**.
 
### <a name="creating-a-splunk-enterprise-and-splunk-cloud-test-user"></a>Criação de um usuário de teste do Splunk Enterprise e do Splunk Cloud

Nesta seção, você criará uma usuária chamada Brenda Fernandes no Splunk Enterprise e no Splunk Cloud. Trabalhe com a [equipe de suporte do Splunk Enterprise e do Splunk Cloud](https://www.splunk.com/content/splunkcom/en_us/about-us/contact.html#tabs/customer-support) para adicionar os usuários à plataforma do Splunk Enterprise e do Splunk Cloud.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuição do usuário de teste do AD do Azure

Nesta seção, você habilitará Brenda Fernandes a usar o logon único do Azure concedendo-lhe acesso ao Splunk Enterprise e ao Splunk Cloud.

![Atribuir usuário][200] 

**Para adicionar Brenda Fernandes ao Splunk Enterprise e o Splunk Cloud, execute as seguintes etapas:**

1. No Portal do Azure, abra a exibição de aplicativos e, em seguida, navegue até a exibição de diretório e vá para **Aplicativos Empresariais** e clique em **Todos os aplicativos**.

    ![Atribuir usuário][201] 

1. Na lista de aplicativos, selecione **Splunk Enterprise e Splunk Cloud**.

    ![Configurar o logon único](./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_splunkenterpriseandsplunkcloud_app.png) 

1. No menu à esquerda, clique em **usuários e grupos**.

    ![Atribuir usuário][202] 

1. Clique no botão **Adicionar**. Em seguida, selecione **usuários e grupos** na **Adicionar atribuição** caixa de diálogo.

    ![Atribuir usuário][203]

1. Em **usuários e grupos** caixa de diálogo, selecione **Britta Simon** na lista de usuários.

1. Clique em **selecione** botão **usuários e grupos** caixa de diálogo.

1. Clique em **atribuir** botão **Adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste do logon único

Nesta seção, você testará sua configuração de SSO do Azure AD usando o Painel de Acesso.

Ao clicar no bloco do Splunk Enterprise e do Splunk Cloud no Painel de Acesso, você deverá ser conectado automaticamente a seu aplicativo Splunk Enterprise e Splunk Cloud.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](tutorial-list.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)


<!--Image references-->

[1]: ./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_general_01.png
[2]: ./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_general_02.png
[3]: ./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_general_03.png
[4]: ./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_general_04.png

[100]: ./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_general_100.png

[200]: ./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_general_200.png
[201]: ./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_general_201.png
[202]: ./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_general_202.png
[203]: ./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_general_203.png

