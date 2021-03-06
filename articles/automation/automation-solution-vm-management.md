---
title: Iniciar/Parar VMs durante a solução de fora do horário comercial
description: Essa solução de gerenciamento de VM inicia e para suas máquinas virtuais do Azure Resource Manager de acordo com um agendamento, e faz o monitoramento delas proativamente no Log Analytics.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 08/1/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: f272ac7ee6432b43d0c9a72daf620a46e52366f8
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/01/2018
ms.locfileid: "39399042"
---
# <a name="startstop-vms-during-off-hours-solution-in-azure-automation"></a>Solução Iniciar/Parar VMs fora do horário comercial na Automação do Azure

A solução Iniciar/Parar VMs fora do horário comercial inicia e para as máquinas virtuais do Azure de acordo com agendamentos definidos pelo usuário, fornece insights por meio do Log Analytics do Azure e envia emails opcionais usando [grupos de ações](../monitoring-and-diagnostics/monitoring-action-groups.md). A solução dá suporte ao Azure Resource Manager e VMs clássicas na maioria dos cenários.

Essa solução fornece uma opção de automação de baixo custo descentralizada para usuários que desejam otimizar seus custos de VM. Com essa solução, você pode:

- Agendar VMs para iniciar e parar.
- Agendar VMs para iniciar e parar em ordem crescente usando Marcas do Azure (incompatível com VMs clássicas).
- Parar VMs automaticamente com base no baixo uso da CPU.

A seguir, são limitações para a solução atual:

- Essa solução gerencia VMs em qualquer região, mas só pode ser usada na mesma assinatura que sua conta do Azure Automation.
- Esta solução está disponível no Azure e no AzureGov para qualquer região que ofereça suporte a um espaço de trabalho do Log Analytics, uma conta do Azure Automation e Alertas. As regiões do AzureGov atualmente não suportam a funcionalidade de e-mail.

## <a name="prerequisites"></a>Pré-requisitos

Os runbooks para esta solução funcionam com uma conta do [Azure Run As](automation-create-runas-account.md). A conta Executar como é o método de autenticação preferido, pois ela usa a autenticação de certificado em vez de uma senha que poderá expirar ou ser alterada com frequência.

## <a name="deploy-the-solution"></a>Implantar a solução

Execute as seguintes etapas para adicionar a solução Iniciar/Parar VMs fora do horário comercial à sua conta de Automação e, em seguida, configure as variáveis para personalizar a solução.

1. Em uma conta de automação, selecione **Iniciar/Parar VM** em **Recursos relacionados**. A partir daqui, você pode clicar em **Saiba mais sobre e habilite a solução**. Se você já tiver uma solução iniciar/parar VM implantada, você pode clicar em **gerenciar a solução** ser levado a uma lista das soluções implantadas e selecioná-lo a partir daí.

   ![Habilitar a conta de automação](./media/automation-solution-vm-management/enable-from-automation-account.png)

   > [!NOTE]
   > Você também pode criá-lo em qualquer lugar no portal do Azure, clicando em **criar um recurso**. Na página Marketplace, digite uma palavra-chave, como **Iniciar** ou **Iniciar/Parar**. Quando você começa a digitar, a lista é filtrada com base em sua entrada. Como alternativa, você pode digitar uma ou mais palavras-chave do nome completo da solução e, em seguida, pressionar Enter. Selecione **Iniciar/Parar VMs fora do horário** nos resultados da pesquisa.
1. Na página **Iniciar/Parar VMs durante as horas de folga** para a solução selecionada, revise as informações de resumo e clique em **Criar**.

   ![Portal do Azure](media/automation-solution-vm-management/azure-portal-01.png)

1. A página **Adicionar Solução** é exibida. Você será solicitado a configurar a solução antes de importá-la na sua assinatura da Automação.

   ![Página Adicionar Solução de Gerenciamento de VM](media/automation-solution-vm-management/azure-portal-add-solution-01.png)

1. Na página **Adicionar Solução**, selecione **Espaço de Trabalho**. Selecione um espaço de trabalho do Log Analytics que esteja vinculada à mesma assinatura do Azure na qual a conta de Automação está. Se você não tiver um espaço de trabalho, selecione **Criar Novo Espaço de Trabalho**. Sobre o **espaço de trabalho do OMS** , execute as seguintes etapas:
   - Especifique um nome para o novo **Espaço de Trabalho do OMS**.
   - Selecione uma **Assinatura** à qual se vincular, escolhendo na lista suspensa, caso a assinatura selecionada por padrão não seja adequada.
   - Em **Grupo de Recursos**, você pode criar um novo grupo de recursos ou selecionar um existente.
   - Selecione um **Local**. No momento, os únicos locais disponíveis são: **Sudeste da Austrália**, **Canadá Central**, **Índia Central**, **Leste dos EUA**, **Leste do Japão**, **Sudeste da Ásia**, **Sul do Reino Unido** e **Europa Ocidental**.
   - Selecione um **tipo de preço**. Escolha a opção **Por GB (autônomo)**. O Log Analytics atualizou o [preço](https://azure.microsoft.com/pricing/details/log-analytics/) e a camada Por GB é a única opção.

1. Depois de fornecer as informações necessárias na página **Espaço de Trabalho do OMS**, clique em **Criar**. Você pode acompanhar o progresso em **Notificações** no menu, que retornará a página **Adicionar Solução** ao terminar.
1. Na página **Adicionar Solução**, selecione **Conta de automação**. Se você estiver criando uma nova área de trabalho do Log Analytics, poderá criar uma nova conta de automação para associá-la ou selecionar uma conta de automação existente que ainda não esteja vinculada a uma área de trabalho do Log Analystics. Selecione uma conta de automação existente ou clique em **Criar uma conta de automação** e, na página **Adicionar automação da conta**, forneça as seguintes informações:
   - No campo **Nome**, digite o nome da conta de Automação.

    Todas as outras opções são preenchidas automaticamente com base no espaço de trabalho do Log Analytics selecionado. Essas opções não podem ser modificadas. Uma conta Executar como do Azure é o método de autenticação padrão para os runbooks incluídos nesta solução. Depois de clicar em **OK**, as opções de configuração serão validadas e a conta de Automação será criada. Você pode acompanhar o progresso em **Notificações** no menu.

1. Por fim, na página **Adicionar Solução**, selecione **Configuração**. A página **Parâmetros** é exibida.

   ![Página de parâmetros para a solução](media/automation-solution-vm-management/azure-portal-add-solution-02.png)

   Aqui, você será solicitado a:
   - Especificar os **Nomes do ResourceGroup de destino**. Esses são os nomes de grupos de recursos que contêm VMs a serem gerenciadas por essa solução. Você pode inserir mais de um nome e separá-los por vírgula (os valores não diferenciam maiúsculas de minúsculas). O uso de um caractere curinga tem suporte para selecionar VMs em todos os grupos de recursos na assinatura. Esse valor é armazenado nas variáveis **External_Start_ResourceGroupNames** e **External_Stop_ResourceGroupNames**.
   - Especifique a **Lista de exclusão de VM (cadeia de caracteres)**. Ela é o nome de uma ou mais máquinas virtuais do grupo de recursos de destino. Você pode inserir mais de um nome e separá-los por vírgula (os valores não diferenciam maiúsculas de minúsculas). O uso de caracteres curingas é aceito. Esse valor é armazenado na variável **External_ExcludeVMNames**.
   - Selecione um **Agendamento**. Este agendamento é uma data e hora recorrentes para iniciar e parar as VMs no grupo de recursos de destino. Por padrão, o agendamento é configurado para 30 minutos a partir de agora. A seleção de uma região diferente não está disponível. Para configurar o agendamento de acordo com seu fuso horário específico após a configuração da solução, confira [Modificando o agendamento de inicialização e desligamento](#modify-the-startup-and-shutdown-schedule).
   - Para receber **Notificações por email** de um grupo de ações, aceite o valor padrão **Sim** e forneça um endereço de email válido. Se você selecionar **Não**, mas decidir mais tarde que deseja receber notificações por email, atualize o [grupo de ações](../monitoring-and-diagnostics/monitoring-action-groups.md) que foi criado, com endereços de email válidos separados por vírgula. Você também precisará habilitar as regras de alerta a seguir:

     - AutoStop_VM_Child
     - Scheduled_StartStop_Parent
     - Sequenced_StartStop_Parent

     > [!IMPORTANT]
     > O valor padrão para **Nomes do ResourceGroup de destino** é um **&ast;**. Isso direciona todas as VMs em uma assinatura. Se você não quiser que a solução direcione todas as VMs em sua assinatura, esse valor precisará ser atualizado para uma lista de nomes de grupos de recursos antes de habilitar os agendamentos.

1. Depois de configurar as definições iniciais necessárias para a solução, clique em **OK** para fechar a página **Parâmetros** e selecione **Criar**. Depois que todas as configurações forem validadas, a solução será implantada em sua assinatura. Esse processo pode levar vários segundos para ser finalizado e você pode acompanhar o progresso em **Notificações** no menu.

## <a name="scenarios"></a>Cenários

A solução contém três cenários distintos. Esses cenários são:

### <a name="scenario-1-startstop-vms-on-a-schedule"></a>Cenário 1: Iniciar/parar VMs em um agendamento

Essa é a configuração padrão quando a solução é implantada pela primeira vez. Por exemplo, você pode configurar a solução para parar todas as VMs em uma assinatura ao sair do trabalho à noite e iniciá-las pela manhã ao retornar ao escritório. Quando configurados durante a implantação, os agendamentos **Scheduled-StartVM** e **Scheduled-StopVM** iniciam e param VMs de destino. A configuração desta solução para simplesmente parar VMs tem suporte, consulte [Modificar agendamentos de inicialização e desligamento](#modify-the-startup-and-shutdown-schedules) para aprender a configurar um agendamento personalizado.

> [!NOTE]
> O fuso horário é o seu fuso horário atual de quando o parâmetro de hora do agendamento foi configurado. No entanto, o parâmetro é armazenado no formato UTC na Automação do Azure. Não é necessário executar nenhuma conversão de fuso horário, pois isso acontece durante a implantação.

Para controlar quais VMs estão no escopo, configure as seguintes variáveis: **External_Start_ResourceGroupNames**, **External_Stop_ResourceGroupNames** e **External_ExcludeVMNames**.

Você pode habilitar o direcionamento da ação a uma assinatura e um grupo de recursos ou direcionar a uma lista específica de VMs, mas não ambos.

#### <a name="target-the-start-and-stop-actions-against-a-subscription-and-resource-group"></a>Direcionar a ação de início e parada para uma assinatura e um grupo de recursos

1. Configure as variáveis **External_Stop_ResourceGroupNames** e **External_ExcludeVMNames** para especificar as VMs de destino.
1. Habilite e atualize os agendamentos **Scheduled-StartVM** e **Scheduled-StopVM**.
1. Execute o runbook **ScheduledStartStop_Parent** com o parâmetro ACTION definido como **iniciar** e o parâmetro WHATIF definido como **True** para visualizar as alterações.

#### <a name="target-the-start-and-stop-action-by-vm-list"></a>Direcionar a ação de início e parada por lista de VMs

1. Execute o runbook **ScheduledStartStop_Parent** com o parâmetro ACTION definido como **iniciar**, adicione uma lista de VMs separadas por vírgula no parâmetro *VMList* e defina o parâmetro WHATIF como **True**. Visualize as alterações.
1. Configure o parâmetro **External_ExcludeVMNames** com uma lista de VMs separadas por vírgula (VM1,VM2,VM3).
1. Esse cenário não segue as variáveis **External_Start_ResourceGroupNames** e **External_Stop_ResourceGroupnames**. Para este cenário, é necessário criar seu próprio agendamento da Automação. Para obter detalhes, consulte [Agendamento de runbooks na Automação do Azure](../automation/automation-schedules.md).

> [!NOTE]
> O valor de **Target ResourceGroup Names** é armazenado como o valor de **External_Start_ResourceGroupNames** e **External_Stop_ResourceGroupNames**. Para obter uma maior granularidade, você pode modificar cada uma das variáveis para direcionar diferentes grupos de recursos. Para iniciar a ação, use **External_Start_ResourceGroupNames** e, para parar a ação, use **External_Stop_ResourceGroupNames**. As VMs são adicionadas automaticamente aos agendamentos de início e parada.

### <a name="scenario-2-startstop-vms-in-sequence-by-using-tags"></a>Cenário 2: Iniciar/parar VMs na sequência usando marcas

Em um ambiente que inclui dois ou mais componentes em várias VMs compatíveis com cargas de trabalho distribuídas, é importante dar suporte ao sequenciamento de quais componentes são iniciados/parados em ordem. Para fazer isso, execute as etapas a seguir:

#### <a name="target-the-start-and-stop-actions-against-a-subscription-and-resource-group"></a>Direcionar a ação de início e parada para uma assinatura e um grupo de recursos

1. Adicione uma marca **SequenceStart** e uma **SequenceStop** com um valor inteiro positivo às VMs que são direcionadas nas variáveis **External_Start_ResourceGroupNames** e **External_Stop_ResourceGroupNames**. As ações iniciar e parar são executadas em ordem crescente. Para saber como marcar uma VM, consulte [Marcar uma Máquina Virtual do Windows no Azure](../virtual-machines/windows/tag.md) e [Marcar uma Máquina Virtual do Linux no Azure](../virtual-machines/linux/tag.md).
1. Modifique os agendamentos **Sequenced-StartVM** e **Sequenced-StopVM** de acordo com a data e hora que atendem às suas exigências e habilite o agendamento.
1. Execute o runbook **SequencedStartStop_Parent** com o parâmetro ACTION definido como **iniciar** e o parâmetro WHATIF definido como **True** para visualizar as alterações.
1. Visualize a ação e faça as alterações necessárias antes de implementar em VMs de produção. Quando estiver pronto, execute o runbook manualmente com o parâmetro definido como **False** ou permita que os agendamentos **Sequenced-StartVM** e **Sequenced-StopVM** da Automação sejam executados automaticamente de acordo com o agendamento prescrito.

#### <a name="target-the-start-and-stop-action-by-vm-list"></a>Direcionar a ação de início e parada por lista de VMs

1. Adicionar uma marca **SequenceStart** e uma **SequenceStop** com um valor inteiro positivo para VMs que você planeja adicionar à variável **VMList**. 
1. Execute o runbook **SequencedStartStop_Parent** com o parâmetro ACTION definido como **iniciar**, adicione uma lista de VMs separadas por vírgula no parâmetro *VMList* e defina o parâmetro WHATIF como **True**. Visualize as alterações.
1. Configure o parâmetro **External_ExcludeVMNames** com uma lista de VMs separadas por vírgula (VM1,VM2,VM3).
1. Esse cenário não segue as variáveis **External_Start_ResourceGroupNames** e **External_Stop_ResourceGroupnames**. Para este cenário, é necessário criar seu próprio agendamento da Automação. Para obter detalhes, consulte [Agendamento de runbooks na Automação do Azure](../automation/automation-schedules.md).
1. Visualize a ação e faça as alterações necessárias antes de implementar em VMs de produção. Quando estiver pronto, execute o runbook monitoring-and-diagnostics/monitoring-action-groups manualmente com o parâmetro definido como **False** ou permita que a Automação agende a execução de **Sequenced-StartVM** e de **Sequenced-StopVM** automaticamente após o agendamento prescrito.

### <a name="scenario-3-startstop-automatically-based-on-cpu-utilization"></a>Cenário 3: Iniciar/parar automaticamente com base na utilização da CPU

Essa solução pode ajudar a gerenciar o custo de execução de máquinas virtuais em sua assinatura, avaliando as VMs do Azure que não são usadas durante os períodos mais calmos, por exemplo, depois do horário comercial, e desligando-as automaticamente se a utilização do processador for menor que x%.

Por padrão, a solução é pré-configurada para avaliar a métrica percentual da CPU para ver se a utilização média é de 5% ou menos. Isso é controlado pelas seguintes variáveis e pode ser modificado caso os valores padrão não atendam às suas exigências:

- External_AutoStop_MetricName
- External_AutoStop_Threshold
- External_AutoStop_TimeAggregationOperator
- External_AutoStop_TimeWindow

Você pode habilitar o direcionamento da ação a uma assinatura e um grupo de recursos ou direcionar a uma lista específica de VMs, mas não ambos.

#### <a name="target-the-stop-action-against-a-subscription-and-resource-group"></a>Direcione a ação de parada a uma assinatura e grupo de recursos

1. Configure as variáveis **External_Stop_ResourceGroupNames** e **External_ExcludeVMNames** para especificar as VMs de destino.
1. Habilite e atualize a agenda **Schedule_AutoStop_CreateAlert_Parent**.
1. Execute o runbook **AutoStop_CreateAlert_Parent** com o parâmetro ACTION definido como **iniciar** e o parâmetro WHATIF definido como **True** para visualizar as alterações.

#### <a name="target-the-start-and-stop-action-by-vm-list"></a>Direcionar a ação de início e parada por lista de VMs

1. Execute o runbook **AutoStop_CreateAlert_Parent** com o parâmetro ACTION definido como **iniciar**, adicione uma lista de VMs separadas por vírgula no parâmetro *VMList* e defina o parâmetro WHATIF como **True**. Visualize as alterações.
1. Configure o parâmetro **External_ExcludeVMNames** com uma lista de VMs separadas por vírgula (VM1,VM2,VM3).
1. Esse cenário não segue as variáveis **External_Start_ResourceGroupNames** e **External_Stop_ResourceGroupnames**. Para este cenário, é necessário criar seu próprio agendamento da Automação. Para obter detalhes, consulte [Agendamento de runbooks na Automação do Azure](../automation/automation-schedules.md).

Agora que você tem um agendamento para parar VMs com base na utilização da CPU, é preciso habilitar um dos agendamento abaixo para iniciá-las.

- Direcione a ação de início por assinatura e grupo de recursos. Consulte as etapas no [Cenário 1](#scenario-1-startstop-vms-on-a-schedule) para testar e habilitar agendamentos **Scheduled-StartVM**.
- Direcionar ação de início por assinatura, grupo de recursos e marca. Consulte as etapas em [Cenário 2](#scenario-2-startstop-vms-in-sequence-by-using-tags) para testar e habilitar agendamentos **Sequenced-StartVM**.

## <a name="solution-components"></a>Componentes da solução

A solução inclui runbooks pré-configurados, agendamentos e integração com Log Analytics para que você possa personalizar a inicialização e o desligamento das máquinas virtuais para atender às suas necessidades de negócios.

### <a name="runbooks"></a>Runbooks

A tabela a seguir lista os runbooks implantados em sua conta da Automação por esta solução. Você não deve fazer alterações no código do runbook. Em vez disso, escreva seu próprio runbook para obter novas funcionalidades.

> [!IMPORTANT]
> Não execute diretamente nenhum runbook com "child" acrescentado ao nome.

Todos os runbooks pai incluem o parâmetro _WhatIf_. Quando definido como **True**, o _WhatIf_ é compatível com o detalhamento do comportamento exato que o runbook assume quando executado sem o parâmetro _WhatIf_ e valida as VMs corretas que estão sendo direcionadas. Um runbook só executa suas ações definidas quando o parâmetro _WhatIf_ é definido como **False**.

|Runbook | parâmetros | DESCRIÇÃO|
| --- | --- | ---|
|AutoStop_CreateAlert_Child | VMObject <br> AlertAction <br> WebHookURI | Chamada a partir do runbook pai. Este runbook cria alertas por recurso para o cenário AutoStop.|
|AutoStop_CreateAlert_Parent | VMList<br> WhatIf: Verdadeiro ou Falso  | Cria ou atualiza regras de alerta do Azure em VMs nos grupos de assinatura ou de recursos de destino. <br> VMList: lista de VMs separadas por vírgula. Por exemplo, _vm1,vm2,vm3_.<br> *WhatIf* valida a lógica de runbook sem execução.|
|AutoStop_Disable | Nenhum | Desabilita os alertas de AutoStop e o agendamento padrão.|
|AutoStop_StopVM_Child | WebHookData | Chamada a partir do runbook pai. As regras de alerta chamam esse runbook para parar a VM.|
|Bootstrap_Main | Nenhum | Usado uma vez para definir configurações de inicialização, como webhookURI, que normalmente não podem ser acessadas no Azure Resource Manager. Este runbook é removido automaticamente após a implantação bem-sucedida.|
|ScheduledStartStop_Child | VMName <br> Ação: Iniciar ou Parar <br> ResourceGroupName | Chamada a partir do runbook pai. Executa uma ação de iniciar ou parar para a parada agendada.|
|ScheduledStartStop_Parent | Ação: Iniciar ou Parar <br>VMList <br> WhatIf: Verdadeiro ou Falso | Isso afeta todas as VMs na assinatura. Edite o **External_Start_ResourceGroupNames** e **External_Stop_ResourceGroupNames** para executar apenas nesses grupos de recursos de destino. Você também pode excluir VMs específicas atualizando a variável **External_ExcludeVMNames**.<br> VMList: lista de VMs separadas por vírgula. Por exemplo, _vm1,vm2,vm3_.<br> _WhatIf_ valida a lógica de runbook sem execução.|
|SequencedStartStop_Parent | Ação: Iniciar ou Parar <br> WhatIf: Verdadeiro ou Falso<br>VMList| Cria marcas com o nome **SequenceStart** e **SequenceStop** em cada VM para as quais você deseja sequenciar a atividade de iniciar/parar. O valor da marca deve ser um inteiro positivo (1, 2, 3) que corresponde à ordem em que você deseja iniciar ou parar. <br> VMList: lista de VMs separadas por vírgula. Por exemplo, _vm1,vm2,vm3_. <br> _WhatIf_ valida a lógica de runbook sem execução. <br> **Observação**: as VMs devem fazer parte dos grupos de recursos definidos como External_Start_ResourceGroupNames, External_Stop_ResourceGroupNames e External_ExcludeVMNames em variáveis da Automação do Azure. Elas devem ter as marcas apropriadas para que as ações entrem em vigor.|

### <a name="variables"></a>variáveis

A tabela a seguir lista as variáveis criadas na sua conta da Automação. Você só deve modificar variáveis com prefixo **External**. Modificar variáveis prefixadas com **Internal** causará efeitos indesejáveis.

|Variável | DESCRIÇÃO|
|---------|------------|
|External_AutoStop_Condition | O operador condicional exigido para configurar a condição antes de disparar um alerta. Os valores aceitáveis são **GreaterThan**, **GreaterThanOrEqual**, **LessThan** e **LessThanOrEqual**.|
|External_AutoStop_Description | O alerta para parar a VM se o percentual da CPU exceder o limite.|
|External_AutoStop_MetricName | O nome da métrica de desempenho para a qual a regra de alerta do Azure deverá ser configurada.|
|External_AutoStop_Threshold | O limite para a regra de alerta do Azure especificada na variável _External_AutoStop_MetricName_. Os valores percentuais podem variar de 1 a 100.|
|External_AutoStop_TimeAggregationOperator | O operador de agregação de tempo que é aplicado ao tamanho de janela selecionado para avaliar a condição. Os valores aceitáveis são **Médio**, **Mínimo**, **Máximo**, **Total** e **Último**.|
|External_AutoStop_TimeWindow | O tamanho da janela durante o qual o Azure analisa a métrica selecionada para disparar um alerta. Esse parâmetro aceita a entrada no formato Intervalo de tempo. Os valores possíveis são de 5 minutos até 6 horas.|
|External_ExcludeVMNames | Insira os nomes das VMs a serem excluídas, separando-os por vírgula, sem espaços.|
|External_Start_ResourceGroupNames | Especifica um ou mais grupos de recursos, separando os valores por vírgula, direcionados a ações de iniciar.|
|External_Stop_ResourceGroupNames | Especifica um ou mais grupos de recursos, separando os valores por vírgula, direcionados a ações de parar.|
|Internal_AutomationAccountName | Especifica o nome da conta de Automação.|
|Internal_AutoSnooze_WebhookUri | Especifica o URI do Webhook chamado para o cenário AutoStop.|
|Internal_AzureSubscriptionId | Especifica a ID da Assinatura do Azure.|
|Internal_ResourceGroupName | Especifica o nome do grupo de recursos da conta da Automação.|

Em todos os cenários, as variáveis **External_Start_ResourceGroupNames**, **External_Stop_ResourceGroupNames** e **External_ExcludeVMNames** são necessárias para direcionar VMs, com exceção da disponibilização de uma lista de VMs separadas por vírgula para os runbooks **AutoStop_CreateAlert_Parent**, **SequencedStartStop_Parent** e **ScheduledStartStop_Parent**. Em outras palavras, suas VMs devem residir em grupos de recursos de destino para que as ações iniciar e parar ocorram. A lógica funciona semelhante à política do Azure, isto é, você pode direcionar a assinatura ou o grupo de recursos e fazer com que as ações sejam herdadas por VMs recém-criadas. Com essa abordagem, não é necessário ter um agendamento distinto para cada VM nem gerenciar as ações iniciar e parar em escala.

### <a name="schedules"></a>Agendas

A tabela a seguir lista cada uma das agendas padrão criadas em sua conta de Automação. É possível modificá-las ou criar suas próprias agendas personalizadas. Por padrão, cada uma dessas é desabilitada, exceto **Scheduled_StartVM** e **Scheduled-StopVM**.

Você não deve habilitar todas os agendamentos, porque isso poderá criar ações de agendamento sobrepostas. É melhor determinar quais otimizações você deseja executar e modificar de acordo. Consulte os exemplos de cenários na seção Visão geral para obter explicações adicionais.

|Nome da agenda | Frequência | DESCRIÇÃO|
|--- | --- | ---|
|Schedule_AutoStop_CreateAlert_Parent | A cada 8 horas | Executa o runbook AutoStop_CreateAlert_Parent a cada 8 horas, que, por sua vez, interrompe os valores baseados em VM em External_Start_ResourceGroupNames, External_Stop_ResourceGroupNames e External_ExcludeVMNames nas variáveis da Automação do Azure. Como alternativa, você pode especificar uma lista de VMs separadas por vírgula utilizando o parâmetro VMList.|
|Scheduled_StopVM | Definida pelo usuário, diariamente | Executa o runbook Scheduled_Parent com um parâmetro de _Parar_ todos os dias no horário especificado. Interrompe automaticamente todas as VMs que atendem às regras definidas por variáveis de ativo. Você deve habilitar o agendamento relacionado, **Scheduled-StartVM**.|
|Scheduled_StartVM | Definida pelo usuário, diariamente | Executa o runbook Scheduled_Parent com um parâmetro de _Iniciar_ todos os dias no horário especificado. Inicia automaticamente todas as VMs que atendem às regras definidas pelas variáveis adequadas. Você deve habilitar o agendamento relacionado, **Scheduled-StopVM**.|
|Sequenced-StopVM | 1h (UTC), toda sexta-feira | Executa o runbook Scheduled_Parent com um parâmetro de _Parar_ toda sexta-feira no horário especificado. Para sequencialmente (em ordem crescente) todas as VMs com uma marca de **SequenceStop** definida pelas variáveis adequadas. Consulte a seção de Runbooks para obter mais detalhes sobre valores de marcas e variáveis de ativos. Você deve habilitar o agendamento relacionado, **Sequenced-StartVM**.|
|Sequenced-StartVM | 13h (UTC), toda segunda-feira | Executa o runbook Scheduled_Parent com um parâmetro de _Parar_ toda segunda-feira no horário determinado. Inicia sequencialmente (em ordem decrescente) todas as VMs com uma marca de **SequenceStart** definida pelas variáveis adequadas. Consulte a seção de Runbooks para obter mais detalhes sobre valores de marcas e variáveis de ativos. Você deve habilitar o agendamento relacionado, **Sequenced-StopVM**.|

## <a name="log-analytics-records"></a>Registros do Log Analytics

A Automação cria dois tipos de registros no espaço de trabalho do Log Analytics: logs de trabalho e fluxos de trabalho.

### <a name="job-logs"></a>Logs de trabalho

|Propriedade | DESCRIÇÃO|
|----------|----------|
|Chamador |  Quem iniciou a operação. Os valores possíveis são um endereço de email ou o sistema para trabalhos agendados.|
|Categoria | Classificação do tipo de dados. Para a Automação, o valor é JobLogs.|
|CorrelationId | O GUID que é a ID de correlação do trabalho de runbook.|
|JobId | GUID que é a ID do trabalho de runbook.|
|operationName | Especifica o tipo de operação realizada no Azure. Para a Automação, o valor é Job.|
|ResourceId | Especifica o tipo de recurso no Azure. Para a Automação, o valor é a conta da Automação associada ao runbook.|
|ResourceGroup | Especifica o nome do grupo de recursos do trabalho do runbook.|
|ResourceProvider | Especifica o serviço do Azure que fornece os recursos que você pode implantar e gerenciar. Para Automação, o valor é Automação do Azure.|
|ResourceType | Especifica o tipo de recurso no Azure. Para a Automação, o valor é a conta da Automação associada ao runbook.|
|resultType | O status do trabalho de runbook. Os valores possíveis são:<br>- Iniciado<br>- Parado<br>- Suspenso<br>- Com falha<br>- Êxito|
|resultDescription | Descreve o estado de resultado do trabalho de runbook. Os valores possíveis são:<br>- O trabalho foi iniciado<br>- O trabalho falhou<br>- Trabalho Concluído|
|RunbookName | Especifica o nome do runbook.|
|SourceSystem | Especifica o sistema de origem dos dados enviados. Para a Automação, o valor é OpsManager|
|StreamType | Especifica o tipo de evento. Os valores possíveis são:<br>- Detalhado<br>- Saída<br>- Erro<br>- Aviso|
|SubscriptionId | Especifica a ID da assinatura do trabalho.
|Hora | Data e hora da execução do trabalho de runbook.|

### <a name="job-streams"></a>Transmissões de trabalho

|Propriedade | DESCRIÇÃO|
|----------|----------|
|Chamador |  Quem iniciou a operação. Os valores possíveis são um endereço de email ou o sistema para trabalhos agendados.|
|Categoria | Classificação do tipo de dados. Para a Automação, o valor é JobStreams.|
|JobId | GUID que é a ID do trabalho de runbook.|
|operationName | Especifica o tipo de operação realizada no Azure. Para a Automação, o valor é Job.|
|ResourceGroup | Especifica o nome do grupo de recursos do trabalho do runbook.|
|ResourceId | Especifica o tipo de ID de recurso no Azure. Para a Automação, o valor é a conta da Automação associada ao runbook.|
|ResourceProvider | Especifica o serviço do Azure que fornece os recursos que você pode implantar e gerenciar. Para Automação, o valor é Automação do Azure.|
|ResourceType | Especifica o tipo de recurso no Azure. Para a Automação, o valor é a conta da Automação associada ao runbook.|
|resultType | O resultado do trabalho de runbook no momento em que o evento foi gerado. Um valor possível é:<br>- InProgress|
|resultDescription | Inclui o fluxo de saída do runbook.|
|RunbookName | O nome do runbook.|
|SourceSystem | Especifica o sistema de origem dos dados enviados. Para a Automação, o valor é OpsManager.|
|StreamType | O tipo de fluxo de trabalho. Os valores possíveis são:<br>- Andamento<br>- Saída<br>- Aviso<br>- Erro<br>- Depurar<br>- Detalhado|
|Hora | Data e hora da execução do trabalho de runbook.|

Quando você executa uma pesquisa de logs que retorna registros da categoria de **JobLogs** ou **JobStreams**, pode selecionar a exibição **JobLogs** ou **JobStreams**, que exibe um conjunto de blocos resumindo as atualizações retornadas pela pesquisa.

## <a name="sample-log-searches"></a>Pesquisas de log de exemplo

A tabela a seguir fornece pesquisas de log de exemplo para os registros de alerta coletados por essa solução.

|Consultar | DESCRIÇÃO|
|----------|----------|
|Localizar trabalhos para o runbook ScheduledStartStop_Parent que foram finalizados com êxito | Pesquisar Category = = "JobLogs" &#124; onde (RunbookName_s = = "ScheduledStartStop_Parent") &#124; onde (ResultType = = "Concluído") &#124; resumir |AggregatedValue = Count () por ResultType, bin (TimeGenerated, 1h) &#124; classificar por TimeGenerated desc|
|Localizar trabalhos para o runbook SequencedStartStop_Parent que foram finalizados com êxito | Pesquisar Category = = "JobLogs" &#124; onde (RunbookName_s = = "SequencedStartStop_Parent") &#124; onde (ResultType = = "Concluído") &#124; resumir |AggregatedValue = Count () por ResultType, bin (TimeGenerated, 1h) &#124; classificar por TimeGenerated desc

## <a name="viewing-the-solution"></a>Exibindo a solução

Para acessar a solução, navegue até sua conta de automação, selecione **Espaço de trabalho** em **RECURSOS RELACIONADOS**. Na página do Log Analytics, selecione **Soluções** em **GERAL**. Na página **Soluções**, selecione a solução **Start-Stop-VM[workspace]** na lista.

A seleção da solução exibe a página de soluções **Start-Stop-VM [workspace]**. Aqui você pode analisar detalhes importantes, como o bloco **StartStopVM**. Como no seu espaço de trabalho do Log Analytics, esse bloco exibe uma contagem e uma representação gráfica dos trabalhos de runbook da solução que foi iniciada e encerrada com êxito.

![Página da solução Gerenciamento de Atualizações de Automação](media/automation-solution-vm-management/azure-portal-vmupdate-solution-01.png)

Daqui, você pode executar outras análises dos registros de trabalho clicando no bloco de rosca. O painel da solução mostra o histórico de trabalhos e as consultas de pesquisa de logs pré-definidas. Acesse o Portal avançado do Log Analytics para pesquisar com base em suas consultas de pesquisa.

## <a name="configure-email-notifications"></a>Configurar notificações por email

Para alterar as notificações por email depois que a solução for implantada, modifique o grupo de ações que foi criado durante a implantação.  

No portal do Azure, navegue até Monitorar -> grupos de ações. Selecione o grupo de ações intitulado **StartStop_VM_Notication**.

![Página da solução Gerenciamento de Atualizações de Automação](media/automation-solution-vm-management/azure-monitor.png)

Na página **StartStop_VM_Notification**, clique em **Editar detalhes** em **Detalhes**. Isso abre a página **Email/SMS/Push/Voz**. Atualize o endereço de email e clique em **OK** para salvar suas alterações.

![Página da solução Gerenciamento de Atualizações de Automação](media/automation-solution-vm-management/change-email.png)

Como alternativa, você pode adicionar mais ações no grupo de ações. Para saber mais sobre grupos de ações, confira [grupos de ações](../monitoring-and-diagnostics/monitoring-action-groups.md)

A seguir está um email de exemplo que é enviado quando a solução desliga as máquinas virtuais.

![Página da solução Gerenciamento de Atualizações de Automação](media/automation-solution-vm-management/email.png)

## <a name="modify-the-startup-and-shutdown-schedules"></a>Modificar as agendas de inicialização e desligamento

O gerenciamento das agendas de inicialização e desligamento nesta solução segue as mesmas etapas descritas em [Agendando um runbook na Automação do Azure](automation-schedules.md).

Há suporte para configurar a solução para simplesmente parar as VMs em um determinado momento. Para fazer isso, você precisa:

1. Certifique-se de que você tenha adicionado os grupos de recursos das VMs para desligar na variável **External_Start_ResourceGroupNames**.
1. Crie sua própria agenda para a hora em que você deseja desligar as máquinas virtuais.
1. Navegue até o runbook **ScheduledStartStop_Parent** e clique em **Agenda**. Isso permite que você selecione a agenda que criou na etapa anterior.
1. Selecione **Parâmetros e configurações de execução** e defina o parâmetro ACTION como "Stop".
1. Clique em **OK** para salvar as alterações.

## <a name="update-the-solution"></a>Atualizar a solução

Se você tiver implantado uma versão anterior dessa solução, será preciso primeiro excluí-la da sua conta antes de implantar uma versão atualizada. Siga as etapas para [remover a solução](#remove-the-solution) e, em seguida, siga as etapas acima para [implantar a solução](#deploy-the-solution).

## <a name="remove-the-solution"></a>Remover a solução

Se decidir que não precisa mais usar a solução, você poderá excluí-la da conta de Automação. A exclusão da solução remove apenas os runbooks. Ela não exclui os agendamentos ou as variáveis que foram criadas quando a solução foi adicionada. Esses ativos deverão ser excluídos manualmente se não estiverem sendo usados com outros runbooks.

Para excluir a solução, execute as etapas a seguir:

1. Na sua conta de Automação, selecione **Espaço de Trabalho** na página esquerda.
1. Na página **Soluções**, selecione a solução **Start-Stop-VM[Workspace]**. Na página **VMManagementSolution[Workspace]**, no menu, selecione **Excluir**.<br><br> ![Excluir a Solução de Gerenciamento de VM](media/automation-solution-vm-management/vm-management-solution-delete.png)
1. Na janela **Excluir Solução**, confirme que deseja excluir a solução.
1. Enquanto as informações são verificadas e a solução é excluída, você pode acompanhar seu progresso no menu **Notificações**. Você é levado de volta à página **Soluções** após o início do processo de remoção da solução.

A conta de Automação e o espaço de trabalho do Log Analytics não serão excluídos como parte desse processo. Se você não deseja manter o espaço de trabalho do Log Analytics, é necessário excluí-lo manualmente. Isso pode ser feito no portal do Azure:

1. Na tela inicial do Portal do Azure, selecione **Log Analytics**.
1. Na página **Log Analytics**, selecione o espaço de trabalho.
1. Selecione **Excluir** no menu da página de configurações do espaço de trabalho.

Se você não deseja manter os componentes de conta de automação do Azure, você poderá excluir cada um manualmente. Para obter a lista de runbooks, variáveis e agendamentos criados pela solução, consulte a [componentes da solução](#solution-components).

## <a name="next-steps"></a>Próximas etapas

- Para saber mais sobre como construir consultas de pesquisa diferentes e examinar os logs de trabalho de Automação com o Log Analytics, confira [Efetuar pesquisas no Log Analytics](../log-analytics/log-analytics-log-searches.md).
- Para saber mais sobre a execução de runbooks, como monitorar trabalhos de runbook e outros detalhes técnicos, confira [Acompanhar um trabalho de runbook](automation-runbook-execution.md).
- Para saber mais sobre o Log Analytics e fontes de coleta de dados, confira [Coletar dados do Armazenamento do Azure na visão geral do Log Analytics](../log-analytics/log-analytics-azure-storage.md).