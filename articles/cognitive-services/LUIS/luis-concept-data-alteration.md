---
title: Noções básicas sobre conceitos de alteração de dados no LUIS – Azure | Microsoft Docs
description: Saiba como os dados podem ser alterados antes das previsões no LUIS (Reconhecimento vocal)
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/26/2018
ms.author: diberry
ms.openlocfilehash: d8421114bb5a7416ad2523fe9b0353f03f672619
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2018
ms.locfileid: "39223976"
---
# <a name="data-alterations"></a>Alterações de dados
O LUIS fornece maneiras de manipular a declaração antes ou durante a previsão. 

## <a name="correct-spelling-errors-in-utterance"></a>Corrigir erros de ortografia na declaração
O LUIS usa a [API de Verificação Ortográfica do Bing V7](https://azure.microsoft.com/services/cognitive-services/spell-check/) para corrigir erros de ortografia na declaração. O LUIS precisa da chave associada a esse serviço. Crie a chave e adicione-a como um parâmetro querystring no [ponto de extremidade](https://aka.ms/luis-endpoint-apis). 

Também é possível corrigir erros de ortografia no painel **Testar** [inserindo a chave](luis-interactive-test.md#view-bing-spell-check-corrections-in-test-panel). A chave é mantida como uma variável de sessão no navegador para o painel Testar. Adicione a chave ao painel Testar em cada sessão do navegador para a qual você deseja corrigir a ortografia. 

O uso da chave no painel de teste e no ponto de extremidade são contados na cota de [uso de chave](https://azure.microsoft.com/pricing/details/cognitive-services/spellcheck-api/). O LUIS implementa os limites da Verificação Ortográfica do Bing para o comprimento do texto. 

O ponto de extremidade requer dois parâmetros para as correções ortográfica funcionarem:

|Param|Valor|
|--|--|
|`spellCheck`|booleano|
|`bing-spell-check-subscription-key`|Chave de ponto de extremidade de [API de Verificação Ortográfica do Bing V7](https://azure.microsoft.com/services/cognitive-services/spell-check/)|

Quando a [API de Verificação Ortográfica do Bing V7](https://azure.microsoft.com/services/cognitive-services/spell-check/) detecta um erro, a declaração original e a declaração corrigida são retornadas junto com as previsões do ponto de extremidade.

```JSON
{
  "query": "Book a flite to London?",
  "alteredQuery": "Book a flight to London?",
  "topScoringIntent": {
    "intent": "BookFlight",
    "score": 0.780123
  },
  "entities": []
}
```
 
## <a name="change-time-zone-of-prebuilt-datetimev2-entity"></a>Alterar o fuso horário da entidade datetimeV2 predefinida
Quando um aplicativo LUIS usa a entidade datetimeV2 predefinida, um valor datetime pode ser retornado na resposta da previsão. O fuso horário da solicitação é usado para determinar o datetime correto a ser retornado. Se a solicitação for proveniente de um bot ou de outro aplicativo centralizado antes de chegar ao LUIS, corrija o fuso horário que o LUIS usa. 

### <a name="endpoint-querystring-parameter"></a>Parâmetro querystring do ponto de extremidade
O fuso horário é corrigido adicionando o fuso horário do usuário ao [ponto de extremidade](https://aka.ms/luis-endpoint-apis) usando o parâmetro `timezoneOffset`. O valor de `timezoneOffset` deve ser o número positivo ou negativo, em minutos, para alterar a hora.  

|Param|Valor|
|--|--|
|`timezoneOffset`|O número positivo ou negativo, em minutos|

### <a name="daylight-savings-example"></a>Exemplo de horário de verão
Se você precisar que o datetimeV2 predefinido ajuste o horário de verão, deverá usar o parâmetro querystring `timezoneOffset` com um valor +/- em minutos para a consulta de [ponto de extremidade](https://aka.ms/luis-endpoint-apis).

Adicionar 60 minutos: 

https://{region}.api.cognitive.microsoft.com/luis/v2.0/apps/{appId}?q=Turn the lights on?**timezoneOffset=60**&verbose={boolean}&spellCheck={boolean}&staging={boolean}&bing-spell-check-subscription-key={string}&log={boolean}

Remover 60 minutos: 

https://{region}.api.cognitive.microsoft.com/luis/v2.0/apps/{appId}?q=Turn the lights on?**timezoneOffset=-60**&verbose={boolean}&spellCheck={boolean}&staging={boolean}&bing-spell-check-subscription-key={string}&log={boolean}

## <a name="c-code-determines-correct-value-of-timezoneoffset"></a>O código em C# determina o valor correto de timezoneOffset
O código em C# a seguir usa o método [FindSystemTimeZoneById](https://docs.microsoft.com/dotnet/api/system.timezoneinfo.findsystemtimezonebyid?view=netframework-4.7.1#examples) da classe [TimeZoneInfo](https://docs.microsoft.com/dotnet/api/system.timezoneinfo?view=netframework-4.7.1) para determinar o `timezoneOffset` correto com base na hora do sistema:

```CSharp
// Get CST zone id
TimeZoneInfo targetZone = TimeZoneInfo.FindSystemTimeZoneById("Central Standard Time");

// Get local machine's value of Now
DateTime utcDatetime = DateTime.UtcNow;

// Get Central Standard Time value of Now
DateTime cstDatetime = TimeZoneInfo.ConvertTimeFromUtc(utcDatetime, targetZone);

// Find timezoneOffset
int timezoneOffset = (int)((cstDatetime - utcDatetime).TotalMinutes);
```

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Corrigir erros de ortografia com este tutorial](luis-tutorial-bing-spellcheck.md)