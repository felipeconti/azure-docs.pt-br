---
title: Responder a eventos com alertas do Azure Log Analytics | Microsoft Docs
description: Este tutorial ajuda-o a reconhecer alertas com Log Analytics para identificar informações importantes no espaço de trabalho e notificá-lo proativamente sobre problemas ou invocar ações para tentar corrigi-los.
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: abb07f6c-b356-4f15-85f5-60e4415d0ba2
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/30/2018
ms.author: magoedte
ms.custom: mvc
ms.component: na
ms.openlocfilehash: c6c7b3f897e38fbd67098c9f881380bc073f13da
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39432643"
---
# <a name="respond-to-events-with-azure-monitor-alerts"></a>Responder a eventos com alertas do Azure Monitor
As regras de pesquisa de log são criadas pelos Alertas do Azure para executar consultas de log automaticamente em intervalos regulares.  Se os resultados da consulta de log corresponderem a critérios específicos, um registro de alerta será criado. A regra pode executar automaticamente uma ou mais ações usando os [Grupos de Ação](../monitoring-and-diagnostics/monitoring-action-groups.md).  Este tutorial é uma continuação do tutorial [Criar e compartilhar painéis de dados do Log Analytics](log-analytics-tutorial-dashboards.md).   

Neste tutorial, você aprenderá como:

> [!div class="checklist"]
> * Criar uma regra de alerta
> * Configurar um Grupo de Ações para enviar uma notificação por email

Para concluir o exemplo neste tutorial, você deve ter uma máquina virtual existente [conectada ao espaço de trabalho do Log Analytics](log-analytics-quick-collect-azurevm.md).

## <a name="sign-in-to-azure-portal"></a>Entre no portal do Azure
Faça logon no Portal do Azure em [https://portal.azure.com](https://portal.azure.com).

## <a name="create-alerts"></a>Criar alertas
Alertas são criados por regras de alerta no Azure Monitor e podem executar automaticamente consultas salvas ou pesquisas de logs personalizadas em intervalos regulares.  Crie alertas com base em métricas de desempenho específicas ou quando determinados eventos são criados, na ausência de um evento ou quando um número de eventos são criados em uma janela de tempo específica.  Por exemplo, alertas podem ser utilizados para notificá-lo quando o uso médio de CPU exceder um determinado limite, quando uma atualização ausente for detectada ou quando um evento for gerado ao detectar que um daemon do Linux ou serviço Windows específico não está em execução.  Se os resultados da pesquisa de logs corresponderem a critérios específicos, um alerta será criado. Em seguida, a regra poderá executar automaticamente uma ou mais ações como notificá-lo sobre o alerta ou invocar outro processo.

No exemplo a seguir, você criará uma regra de alerta de medição métrica com base na consulta de *VMs do Azure - Utilização do processador*  salva no [tutorial Visualizar dados](log-analytics-tutorial-dashboards.md). Um alerta será criado para cada máquina virtual que exceder um limite de 90%.

1. No portal do Azure, clique em **Todos os serviços**. Na lista de recursos, digite **Monitor**. Quando você começa a digitar, a lista é filtrada com base em sua entrada. Selecione **Monitor**.
1. No painel esquerdo, selecione **Alertas** e, em seguida, clique em **Nova Regra de Alerta** na parte superior da página para criar um novo alerta.

    ![Criar nova regra de alerta](./media/log-analytics-tutorial-response/alert-rule-02.png)

1. Para a primeira etapa, na seção **Criar Alerta** você selecionará o espaço de trabalho do Log Analytics como o recurso, pois esse é um sinal de alerta baseado em log.  Filtre os resultados, escolhendo a **Assinatura** específica na lista suspensa, se houver mais de uma, a qual contém a VM e o espaço de trabalho do Log Analytics criado anteriormente.  Filtre o **Tipo de Recurso**, selecionando o **Log Analytics** na lista suspensa.  Por fim, selecione **Recurso** **DefaultLAWorkspace** e, em seguida, clique em **Concluído**.

    ![Criar tarefa da etapa 1 de alerta](./media/log-analytics-tutorial-response/alert-rule-03.png)

1. Na seção **Critérios de Alerta**, clique em **Adicionar Critérios** para definir a consulta salva e, em seguida, especifique a lógica que a regra de alerta segue. No painel **Configurar lógica de sinal**, selecione **Pesquisa de logs personalizada** como nome de sinal e insira sua consulta na **Consulta de pesquisa**.

    Por exemplo: 
    ```
    Perf
    | where CounterName == "% Processor Time" and ObjectName == "Processor" and InstanceName == "_Total"
    | summarize AggregatedValue=avg(CounterValue) by bin(TimeGenerated, 1m)
    ```

    O painel é atualizado para apresentar as configurações do alerta.  Na parte superior, mostra o conjunto de resultados dos últimos 30 minutos do sinal selecionado.

1. Configure o alerta com as informações a seguir:  
   a. Na lista suspensa **Com base em*, selecione **Medição métrica**.  Uma medição métrica criará um alerta para cada objeto na consulta com um valor que exceda o limite especificado.  
   b. Para a **Condição**, selecione **Maior que** e insira **90** para o **Limite**.  
   c. Na seção Disparar Alerta com Base Em, selecione **Violações consecutivas** e na lista suspensa selecione **Maior que** e insira um valor de 3.  
   d. Na seção Avaliação com Base Em, aceite os padrões. A regra executará a cada cinco minutos e retornará registros criados dentro desse intervalo da hora atual.  
1. Clique em **Concluído** para concluir a regra de alerta.

    ![Configurar sinal de alerta](./media/log-analytics-tutorial-response/alert-signal-logic-02.png)

1. Agora, passando para a segunda etapa, forneça um nome de alerta no campo **Nome da regra de alerta**, como **Percentual de CPU maior que 90%**.  Especifique uma **Descrição** com detalhes específicos para o alerta e selecione **Critical(Sev 0)** para o valor de **Gravidade** das opções fornecidas.

    ![Configurar detalhes do alerta](./media/log-analytics-tutorial-response/alert-signal-logic-04.png)

1. Para ativar imediatamente a regra de alerta na criação, aceite o valor padrão para **Habilitar regra após a criação**.  
1. Na terceira e última etapa, você especifica um **Grupo de Ações**, o qual garantirá que as mesmas ações sejam executadas sempre que um alerta for disparado e poderá ser utilizado para cada regra definida.  Configure um novo grupo de ações com as informações a seguir:  
   a. Selecione **Novo grupo de ações** e o painel **Adicionar grupo de ações** aparece.  
   b. Para **Nome do grupo de ações**, especifique um nome como **Operações de TI - Notificar** e um **Nome curto** como **itops-n**.  
   c. Verifique se os valores padrão para **Assinatura** e **Grupo de recursos** estão corretos. Caso contrário, selecione um correto na lista suspensa.  
   d. Na seção Ações, especifique um nome para a ação, como **Enviar Email** e em **Tipo de Ação** selecione **Email/SMS/Push/Voz** na lista suspensa. O painel de propriedades **Email/SMS/Push/Voz** será aberto à direita para fornecer informações adicionais.  
   e. No painel **Email/SMS/Push/Voz**, habilite **Email** e forneça um endereço SMTP de email válido para entregar a mensagem.  
   f. Clique em **OK** para salvar as alterações.  
       ![Criar novo grupo de ações](./media/log-analytics-tutorial-response/action-group-properties-01.png)

1. Clique em **OK** para concluir o grupo de ações.
1. Clique em **Criar regra de alerta** para concluir a regra de alerta. Ela começa a ser executada imediatamente.

    ![Concluir criação da nova regra de alerta](./media/log-analytics-tutorial-response/alert-rule-01.png)

## <a name="view-your-alerts-in-azure-portal"></a>Exibir os alertas no portal do Azure
Agora que você criou um alerta, será possível exibir os alertas do Azure em um único painel e gerenciar todas as regras de alerta nas assinaturas do Azure. Ela lista todas as regras de alerta (habilitadas ou desabilitadas) e estas podem ser classificadas com base em grupos de recursos, recursos de destino, nome da regra ou status. Incluem-se uma agregação/resumo de todos os alertas disparados e o total de regras de alerta configuradas/habilitadas.

![Página de status de Alertas do Azure](./media/log-analytics-tutorial-response/azure-alerts-02.png)

Quando o alerta é disparado, a tabela reflete a condição e quantas vezes ocorreu dentro do intervalo de tempo selecionado (o padrão é as últimas seis horas).  Deverá haver um email correspondente na caixa de entrada semelhante ao exemplo a seguir, mostrando a máquina virtual incorreta e os principais resultados que corresponderam à consulta de pesquisa nesse caso.

![Exemplo de ação de email de alerta](./media/log-analytics-tutorial-response/azure-alert-email-notification-01.png)

## <a name="next-steps"></a>Próximas etapas
Neste tutorial, você aprendeu como as regras de alerta podem identificar de forma proativa e responder a um problema quando executarem as pesquisas de log em intervalos agendados e correspondem a determinado critério.

Siga este link para ver amostras de script do Log Analytics pré-criadas.

> [!div class="nextstepaction"]
> [Amostras de script do Log Analytics](powershell-samples.md)
