---
title: Tutorial sobre como criar um aplicativo de LUIS para obter os dados de correspondência da expressão regular – Azure | Microsoft Docs
description: Neste tutorial, saiba como criar um aplicativo de LUIS simples usando intenções e uma entidade de expressão regular para extrair dados.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 08/02/2018
ms.author: diberry
ms.openlocfilehash: 994bd6f2a041e25d15c7e0b4a216952cec4101fa
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/03/2018
ms.locfileid: "39492816"
---
# <a name="tutorial-3-add-regular-expression-entity"></a>Tutorial: 3. Adicionar entidade de expressão regular
Neste tutorial, você criará um aplicativo que demonstra como extrair dados formatados de forma consistente a partir de um enunciado usando a entidade de **Expressão Regular**.


<!-- green checkmark -->
> [!div class="checklist"]
> * Compreender entidades de expressão regular 
> * Usar um aplicativo de LUIS para um domínio de recursos humanos (RH) com a intenção FindForm
> * Adicionar entidade de expressão regular para extrair o número do formulário do enunciado
> * Treinar e publicar o aplicativo
> * Consulte ponto de extremidade do aplicativo para ver a resposta JSON do LUIS

[!include[LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="before-you-begin"></a>Antes de começar
Caso não tenha o aplicativo de recursos humanos do tutorial de [entidades predefinidas](luis-tutorial-prebuilt-intents-entities.md), [importe](luis-how-to-start-new-app.md#import-new-app) o JSON em um novo aplicativo no site do [LUIS](luis-reference-regions.md#luis-website) do repositório Github de [exemplos do LUIS](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-prebuilts-HumanResources.json).

Caso queira manter o aplicativo de recursos humanos original, clone a versão na página [Configurações](luis-how-to-manage-versions.md#clone-a-version) e nomeie-a como `regex`. A clonagem é uma ótima maneira de testar vários recursos de LUIS sem afetar a versão original. 


## <a name="purpose-of-the-regular-expression-entity"></a>Finalidade da entidade de expressão regular
A finalidade de uma entidade é obter dados importantes contidos no enunciado. O uso da entidade de expressão regular pelo aplicativo é para obter números formatados de formulário de recursos humanos (RH) de um enunciado. Não se trata de aprendizado de máquina. 

Enunciados simples de exemplo incluem:

```
Where is HRF-123456?
Who authored HRF-123234?
HRF-456098 is published in French?
```

Versões abreviadas ou com gírias de enunciados incluem:

```
HRF-456098
HRF-456098 date?
HRF-456098 title?
```
 
A entidade de expressão regular para corresponder ao número de formulário é `hrf-[0-9]{6}`. Essa expressão regular corresponde aos caracteres literais `hrf -`, mas ignora variantes de caixa e cultura. Ela corresponde aos dígitos 0-9 de exatamente 6 dígitos.

HRF significa formulário de recursos humanos.

### <a name="tokenization-with-hyphens"></a>Geração de tokens com hifens
O LUIS cria tokens do enunciado quando a expressão é adicionada a uma intenção. A geração de tokens desses enunciados adiciona espaços antes e depois do hífen `Where is HRF - 123456?` A expressão regular é aplicada ao enunciado em formato bruto antes que sejam criados tokens para ele. Por ser aplicada ao formulário _bruto_, a expressão regular não precisa lidar com limites de palavras. 


## <a name="add-findform-intent"></a>Adicionar intenção FindForm

1. Verifique se o seu aplicativo de recursos humanos está na seção **Compilar** do LUIS. Você pode alterar essa seção selecionando **Compilar** na barra de menus da parte superior direita. 

2. Selecione **Criar nova intenção**. 

3. Insira `FindForm` na caixa de diálogo pop-up, depois selecione **Concluído**. 

    ![Captura de tela da caixa de diálogo Criar nova tentativa com utilitários na caixa de pesquisa](./media/luis-quickstart-intents-regex-entity/create-new-intent-ddl.png)

4. Adicione enunciados de exemplo para a intenção.

    |Exemplo de enunciados|
    |--|
    |Qual é a URL para hrf-123456?|
    |Onde está a hrf-345678?|
    |Quando hrf-456098 foi atualizada?|
    |John Smith atualizou a hrf-234639 na semana passada?|
    |Quantas versões da hrf-345123 existem?|
    |Quem deve autorizar o formulário hrf-123456?|
    |Quantas pessoas precisam se desconectar da hrf-345678?|
    |Data da hrf-234123?|
    |Autor da hrf-546234?|
    |Título da hrf-456234?|

    [ ![Captura de tela da página Intenção com novos enunciados realçados](./media/luis-quickstart-intents-regex-entity/findform-intent.png) ](./media/luis-quickstart-intents-regex-entity/findform-intent.png#lightbox)

    O aplicativo tem o número da entidade predefinida adicionado do tutorial anterior, portanto, cada número de formulário está marcado. Isso pode ser suficiente para seu aplicativo cliente, mas o número não será rotulado com o tipo de número. Criar uma nova entidade com um nome apropriado permite que o aplicativo cliente processe a entidade adequadamente ao ser retornado do LUIS.

## <a name="create-an-hrf-number-regular-expression-entity"></a>Criar uma entidade de expressão regular com número HRF 
Crie uma entidade de expressão regular para informar ao LUIS o que é um formato HRF-número nas etapas a seguir:

1. Selecione **Entidades** no painel esquerdo.

2. Selecione o botão **Criar nova entidade** na página Entidades. 

3. Na caixa de diálogo pop-up, insira o novo nome de entidade `HRF-number`, selecione **RegEx** como o tipo de entidade, insira `hrf-[0-9]{6}` como o Regex e, depois, selecione **Concluído**.

    ![Captura de tela com caixa de diálogo pop-up configurando as propriedades da entidade](./media/luis-quickstart-intents-regex-entity/create-regex-entity.png)

4. Selecione **Intenções**, em seguida, a intenção **FindForm** para ver a expressão regular rotulada nos enunciados. 

    [![Captura de tela do enunciado Rótulo com um padrão existente de entidade e regex](./media/luis-quickstart-intents-regex-entity/labeled-utterances-for-entity.png)](./media/luis-quickstart-intents-regex-entity/labeled-utterances-for-entity.png#lightbox)

    Como a entidade não é uma entidade de aprendizado de máquina, o rótulo é aplicado aos enunciados e exibido no site LUIS assim que ela for criada.

## <a name="train-the-luis-app"></a>Treinar o aplicativo LUIS

[!include[LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Publicar o aplicativo para obter a URL do ponto de extremidade

[!include[LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="query-the-endpoint-with-a-different-utterance"></a>Consultar o ponto de extremidade com um enunciado diferente

1. [!include[LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Vá até o final da URL no endereço e insira `When were HRF-123456 and hrf-234567 published in the last year?`. O último parâmetro de querystring é `q`, o enunciado **consulta**. Esse enunciado não é igual a nenhum dos enunciados rotulados, portanto, ele é um bom teste e deve retornar a intenção `FindForm` com os números de formulário de `HRF-123456` e `hrf-234567`.

    ```
    {
      "query": "When were HRF-123456 and hrf-234567 published in the last year?",
      "topScoringIntent": {
        "intent": "FindForm",
        "score": 0.9993477
      },
      "intents": [
        {
          "intent": "FindForm",
          "score": 0.9993477
        },
        {
          "intent": "ApplyForJob",
          "score": 0.0206110049
        },
        {
          "intent": "GetJobInformation",
          "score": 0.00533067342
        },
        {
          "intent": "Utilities.StartOver",
          "score": 0.004215215
        },
        {
          "intent": "Utilities.Help",
          "score": 0.00209096959
        },
        {
          "intent": "None",
          "score": 0.0017655947
        },
        {
          "intent": "Utilities.Stop",
          "score": 0.00109490135
        },
        {
          "intent": "Utilities.Confirm",
          "score": 0.0005704638
        },
        {
          "intent": "Utilities.Cancel",
          "score": 0.000525338168
        }
      ],
      "entities": [
        {
          "entity": "last year",
          "type": "builtin.datetimeV2.daterange",
          "startIndex": 53,
          "endIndex": 61,
          "resolution": {
            "values": [
              {
                "timex": "2017",
                "type": "daterange",
                "start": "2017-01-01",
                "end": "2018-01-01"
              }
            ]
          }
        },
        {
          "entity": "hrf-123456",
          "type": "HRF-number",
          "startIndex": 10,
          "endIndex": 19
        },
        {
          "entity": "hrf-234567",
          "type": "HRF-number",
          "startIndex": 25,
          "endIndex": 34
        },
        {
          "entity": "-123456",
          "type": "builtin.number",
          "startIndex": 13,
          "endIndex": 19,
          "resolution": {
            "value": "-123456"
          }
        },
        {
          "entity": "-234567",
          "type": "builtin.number",
          "startIndex": 28,
          "endIndex": 34,
          "resolution": {
            "value": "-234567"
          }
        }
      ]
    }
    ```

    Os números no enunciado são retornados duas vezes: uma vez como a nova entidade `hrf-number` e uma vez como uma entidade predefinida `number`. Um enunciado pode ter mais de uma entidade e mais de uma entidade do mesmo tipo, como mostra o exemplo. Usando uma entidade de expressão regular, o LUIS extrai dados nomeados, o que é mais programaticamente útil para o aplicativo cliente recebendo a resposta JSON.

## <a name="what-has-this-luis-app-accomplished"></a>O que esse aplicativo de LUIS realizou?
Esse aplicativo identificou a intenção e retornou os dados extraídos. 

Agora seu chatbot tem informações suficientes para determinar a ação primária, `FindForm`, e quais números de formulário estavam na pesquisa. 

## <a name="where-is-this-luis-data-used"></a>Onde esses dados do LUIS são usados? 
O LUIS é feito com essa solicitação. O aplicativo de chamada, como um chatbot, pode pegar o resultado de topScoringIntent e os números de formulário e pesquisar uma API de terceiros. O LUIS não faz esse trabalho. O LUIS apenas determina qual é a intenção do usuário e extrai os dados sobre essa intenção. 

## <a name="clean-up-resources"></a>Limpar recursos

[!include[LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Saiba mais sobre a entidade de lista](luis-quickstart-intent-and-list-entity.md)

