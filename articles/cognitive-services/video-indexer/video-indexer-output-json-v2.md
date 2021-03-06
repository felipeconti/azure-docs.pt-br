---
title: Examine a saída do Indexador de Vídeo do Azure produzida pela API v2 | Microsoft Docs
description: Este tópico examina a saída do Video Indexer produzida pela API v2.
services: cognitive services
documentationcenter: ''
author: juliako
manager: cfowler
ms.service: cognitive-services
ms.topic: article
ms.date: 07/25/2018
ms.author: juliako
ms.openlocfilehash: 43cc02417fad8a2fa46bd309235951393cd55b8a
ms.sourcegitcommit: d2f2356d8fe7845860b6cf6b6545f2a5036a3dd6
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "40187364"
---
# <a name="examine-the-video-indexer-output-produced-by-v2-api"></a>Examine a saída do Video Indexer produzida pela API v2

> [!Note]
> A API do Video Indexer V1 foi descontinuada em 1º de agosto de 2018. Agora, você deve usar a API do indexador de vídeo v2. <br/>Para desenvolver com as APIs do Video Indexer v2, consulte as instruções encontradas [aqui](https://api-portal.videoindexer.ai/). 

Quando você chama o **obter índice de vídeo** API e o status da resposta está Okey, você obterá uma saída detalhada do JSON como o conteúdo da resposta. O conteúdo JSON contém detalhes das informações do vídeo especificado. Os insights incluem dimensões como: transcrições, ocrs, faces, tópicos, blocos etc. As dimensões têm instâncias de intervalos de tempo que mostram quando cada dimensão apareceu no vídeo.  

Você também visualmente pode examinar informações de resumidas do vídeo pressionando a **reproduzir** botão no vídeo no portal do indexador de vídeo. Para obter mais informações, consulte [insights de vídeo de exibir e editar](video-indexer-view-edit.md).

![Insights](./media/video-indexer-output-json/video-indexer-summarized-insights.png)

Este artigo examina o conteúdo JSON retornado pela **API Get Video Index**. 

> [!NOTE]
> Expiração de todos os tokens de acesso no indexador de vídeo é uma hora.


## <a name="root-elements"></a>Elementos raiz

|NOME|DESCRIÇÃO|
|---|---|
|accountId|ID da conta VI da lista de reprodução.|
|ID|ID. da lista de reprodução.|
|Nome|Nome da lista de reprodução.|
|Descrição|Descrição da lista de reprodução.|
|userName|O nome do usuário que criou a lista de reprodução.|
|criado|Hora de criação da lista de reprodução.|
|privacyMode|Modo de privacidade da lista de reprodução (pública/privada).|
|state|A playlist (upload, processamento, processamento, falha, quarentena).|
|isOwned|Indica se a lista de reprodução foi criada pelo usuário atual.|
|isEditable|Indica se o usuário atual está autorizado a editar a lista de reprodução.|
|isBase|Indica se a lista de reprodução é uma lista de reprodução básica (um vídeo) ou uma lista de reprodução de outros vídeos (derivados).|
|durationInSeconds|A duração total da lista de reprodução.|
|summarizedInsights|Contém um [summarizedInsights](#summarizedinsights).
|Vídeos|Uma lista dos [vídeos](#videos) construir a lista de reprodução.<br/>Se esta lista de reprodução de intervalos de tempo de outros vídeos (derivados), os vídeos nesta lista conterão apenas dados dos intervalos de tempo incluídos.|

```json
{
  "accountId": "bca61527-1221-bca6-1527-a90000002000",
  "id": "abc3454321",
  "name": "My first video",
  "description": "I am trying VI",
  "userName": "Some name",
  "created": "2018/2/2 18:00:00.000",
  "privacyMode": "Private",
  "state": "Processed",
  "isOwned": true,
  "isEditable": false,
  "isBase": false,
  "durationInSeconds": 120, 
  "summarizedInsights" : { . . . }
  "videos": [{ . . . }]
}
```

## <a name="summarizedinsights"></a>summarizedInsights

Esta seção mostra o resumo das informações.

|Atributo | DESCRIÇÃO|
|---|---|
|Nome|O nome do vídeo. Por exemplo, o Azure Monitor.|
|shortId|A ID do vídeo. Por exemplo, 63c6d532ff.|
|privacyMode|Seu detalhamento pode ter um dos seguintes modos: **Particular**, **Público**. **Público**: o vídeo é visível para todos na sua conta e para qualquer pessoa que tenha um link para o vídeo. **Privada** -o vídeo é visível para todos em sua conta.|
|duration|Contém uma duração que descreve o tempo que uma percepção ocorreu. Duração é em segundos.|
|thumbnailVideoId|A ID do vídeo da qual a miniatura foi tirada.
|thumbnailId|A ID da miniatura do vídeo. Para obter a miniatura real, chame Get-Thumbnail (https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-thumbnail) e passe para thumbnailVideoId e thumbnailId.|
|faces|Pode conter zero ou mais faces. Para obter mais informações, consulte [faces](#faces).|
|palavras-chave|Pode conter zero ou mais palavras-chave. Para obter mais informações, consulte [palavras-chave](#keywords).|
|sentiments|Pode conter zero ou mais sentimentos. Para obter mais informações, consulte [sentimentos](#sentiments).|
|audioEffects| Pode conter zero ou mais audioEffects. Para informações mais detalhadas, consulte [audioEffects](#audioeffects).|
|rótulos| Pode conter zero ou mais rótulos. Para obter mais informações, consulte [rótulos](#labels).|
|marcas| Pode conter zero ou mais marcas. Para informações mais detalhadas, consulte [marcas](#brands).|
|Estatísticas | Para obter mais informações, consulte [estatísticas](#statistics).|

## <a name="videos"></a>Vídeos

|NOME|DESCRIÇÃO|
|---|---|
|accountId|A ID da conta VI do vídeo.|
|ID|ID do vídeo.|
|Nome|Nome do vídeo.
|state|Estado do vídeo (carregado, processado, processado, com falha, em quarentena).|
|processingProgress|O progresso do processamento durante o processamento (por exemplo, 20%).|
|failureCode|O código de falha se não for processado (por exemplo, 'UnsupportedFileType').|
|failureMessage|A mensagem de falha se não for processada.|
|externalId|O ID externo do vídeo (se especificado pelo usuário).|
|externalUrl|O URL externo do vídeo (se especificado pelo usuário).|
|metadata|Os metadados externos do vídeo (se especificado pelo usuário).|
|isAdult|Indica se o vídeo foi revisado manualmente e identificado como um vídeo adulto.|
|Insights|O objeto de insights. Para obter mais informações, consulte [insights](#insights).|
|thumbnailId|A ID da miniatura do vídeo. Para obter a miniatura real, chame Get-Thumbnail (https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-thumbnail) e passe-a para a ID do vídeo e para thumbnailId.|
|publishedUrl|Uma URL para transmitir o vídeo.|
|publishedUrlProxy|Uma URL para transmitir o vídeo (para dispositivos Apple).|
|viewToken|Um símbolo de visualização de curta duração para transmitir o vídeo.|
|sourceLanguage|O idioma de origem do vídeo.|
|Linguagem|O idioma real do vídeo (tradução).|
|indexingPreset|A predefinição usada para indexar o vídeo.|
|streamingPreset|A predefinição usada para publicar o vídeo.|
|linguisticModelId|O modelo CRIS usado para transcrever o vídeo.|
|Estatísticas | Para obter mais informações, consulte [estatísticas](#statistics).|

```json
{
    "videos": [{
        "accountId": "2cbbed36-1972-4506-9bc7-55367912df2d",
        "id": "142a356aa6",
        "state": "Processed",
        "privacyMode": "Private",
        "processingProgress": "100%",
        "failureCode": "General",
        "failureMessage": "",
        "externalId": null,
        "externalUrl": null,
        "metadata": null,
        "insights": {. . . },
        "thumbnailId": "89d7192c-1dab-4377-9872-473eac723845",
        "publishedUrl": "https://videvmediaservices.streaming.mediaservices.windows.net:443/d88a652d-334b-4a66-a294-3826402100cd/Xamarine.ism/manifest",
        "publishedProxyUrl": null,
        "viewToken": "Bearer=<token>",
        "sourceLanguage": "En-US",
        "language": "En-US",
        "indexingPreset": "Default",
        "linguisticModelId": "00000000-0000-0000-0000-000000000000"
    }],
}
```
### <a name="insights"></a>Insights

Os insights são um conjunto de dimensões (por exemplo, linhas transcritas, faces, marcas, etc.), em que cada dimensão é uma lista de elementos exclusivos (por exemplo, face1, face2, face3) e cada elemento tem seus próprios metadados e uma lista de suas instâncias (que são intervalos de tempo com metadados opcionais adicionais).

Um rosto pode ter um ID, um nome, uma miniatura, outros metadados e uma lista de suas instâncias temporais (por exemplo: 00:00:05 - 00:00:10, 00:01:00 - 00:02:30 e 00:41:21 - 00:41:49.) Cada instância temporal pode ter metadados adicionais. Por exemplo, o retângulo da face coordena (20,230,60,60).

|Versão|A versão do código|
|---|---|
|sourceLanguage|O idioma de origem do vídeo (assumindo um idioma mestre). Na forma de um [BCP-47](https://tools.ietf.org/html/bcp47) cadeia de caracteres.|
|Linguagem|A linguagem de insights (traduzida do idioma de origem). Na forma de um [BCP-47](https://tools.ietf.org/html/bcp47) cadeia de caracteres.|
|transcript|A dimensão da [transcrição](#transcript).|
|ocr|A dimensão [ocr](#ocr).|
|palavras-chave|A dimensão [palavras-chave](#keywords).|
|Blocos|Pode conter um ou mais [blocos](#blocks)|
|faces|A dimensão [faces](#faces).|
|rótulos|A dimensão de [rótulos](#labels).|
|shots|Dimensão [tiros](#shots).|
|marcas|A dimensão das [marcas](#brands).|
|audioEffects|A dimensão [audioEffects](#audioEffects).|
|sentiments|A dimensão dos [sentimentos](#sentiments).|
|visualContentModeration|A dimensão [visualContentModeration](#visualcontentmoderation).|
|textualConentModeration|A dimensão [textualConentModeration](#textualconentmoderation).|

Exemplo:

```json
{
  "version": "0.9.0.0",
  "sourceLanguage": "en-US",
  "language": "es-ES",
  "transcript": ...,
  "ocr": ...,
  "keywords": ...,
  "faces": ...,
  "labels": ...,
  "shots": ...,
  "brands": ...,
  "audioEffects": ...,
  "sentiments": ...,
  "visualContentModeration": ...,
  "textualConentModeration": ...
}
```

#### <a name="blocks"></a>Blocos

Atributo | DESCRIÇÃO
---|---
ID|ID do bloco.|
instances|Uma lista de intervalos de tempo deste bloco.|

#### <a name="transcript"></a>transcript

|NOME|DESCRIÇÃO|
|---|---|
|ID|A ID da linha.|
|text|A própria transcrição.|
|Linguagem|O idioma da transcrição. Tem o objetivo dar suporte à transcrição na qual cada linha pode ter um idioma diferente.|
|instances|Uma lista com os intervalos de tempo nos quais essa linha apareceu. Se a instância for transcrita, ela terá apenas 1 instância.|

Exemplo:

```json
"transcript": [
{
    "id": 0,
    "text": "Hi I'm Doug from office.",
    "language": "en-US",
    "instances": [
    {
        "start": "00:00:00.5100000",
        "end": "00:00:02.7200000"
    }
    ]
},
{
    "id": 1,
    "text": "I have a guest. It's Michelle.",
    "language": "en-US",
    "instances": [
    {
        "start": "00:00:02.7200000",
        "end": "00:00:03.9600000"
    }
    ]
}
] 
```

#### <a name="ocr"></a>ocr

|NOME|DESCRIÇÃO|
|---|---|
|ID|A ID da linha de OCR.|
|text|O texto de OCR.|
|confidence|A confiança do reconhecimento.|
|Linguagem|O idioma do OCR.|
|instances|Uma lista de intervalos de tempo nos quais essa OCR apareceu (o mesmo OCR pode aparecer várias vezes).|

```json
"ocr": [
    {
      "id": 0,
      "text": "LIVE FROM NEW YORK",
      "confidence": 0.91,
      "language": "en-US",
      "instances": [
        {
          "start": "00:00:26",
          "end": "00:00:52"
        }
      ]
    },
    {
      "id": 1,
      "text": "NOTICIAS EN VIVO",
      "confidence": 0.9,
      "language": "es-ES",
      "instances": [
        {
          "start": "00:00:26",
          "end": "00:00:28"
        },
        {
          "start": "00:00:32",
          "end": "00:00:38"
        }
      ]
    }
  ],
```

#### <a name="keywords"></a>palavras-chave

|NOME|DESCRIÇÃO|
|---|---|
|ID|A ID da palavra-chave.|
|text|O texto da palavra-chave.|
|confidence|A confiança do reconhecimento da palavra-chave.|
|Linguagem|O idioma da palavra-chave (quando traduzida).|
|instances|Uma lista de intervalos de tempo nos quais essa palavra-chave apareceu (uma palavra-chave pode aparecer várias vezes).|

```json
"keywords": [
{
    "id": 0,
    "text": "office",
    "confidence": 1.6666666666666667,
    "language": "en-US",
    "instances": [
    {
        "start": "00:00:00.5100000",
        "end": "00:00:02.7200000"
    },
    {
        "start": "00:00:03.9600000",
        "end": "00:00:12.2700000"
    }
    ]
},
{
    "id": 1,
    "text": "icons",
    "confidence": 1.4,
    "language": "en-US",
    "instances": [
    {
        "start": "00:00:03.9600000",
        "end": "00:00:12.2700000"
    },
    {
        "start": "00:00:13.9900000",
        "end": "00:00:15.6100000"
    }
    ]
}
] 

```

#### <a name="faces"></a>faces

|NOME|DESCRIÇÃO|
|---|---|
|ID|A ID da face.|
|Nome|O nome da face. Pode ser "Desconhecido #0", uma celebridade identificada ou uma pessoa treinada pelo cliente.|
|confidence|A confiança de identificação da face.|
|Descrição|Uma descrição da celebridade. |
|thumbnalId|O ID da miniatura dessa face.|
|knownPersonId|Se é uma pessoa conhecida, o seu ID interno.|
|referenceId|Se for uma celebridade do Bing, o seu ID do Bing.|
|referenceType|No momento, apenas Bing.|
|título|Se é uma celebridade, seu título (por exemplo, "CEO da Microsoft").|
|imageUrl|Se é uma celebridade, o seu URL de imagem.|
|instances|Essas são as ocorrências do aparecimento da face no intervalo de tempo determinado. Cada ocorrência também tem uma thumbnailsId. |

```json
"faces": [{
    "id": 2002,
    "name": "Xam 007",
    "confidence": 0.93844,
    "description": null,
    "thumbnailId": "00000000-aee4-4be2-a4d5-d01817c07955",
    "knownPersonId": "8340004b-5cf5-4611-9cc4-3b13cca10634",
    "referenceId": null,
    "title": null,
    "imageUrl": null,
    "instances": [{
        "thumbnailsIds": ["00000000-9f68-4bb2-ab27-3b4d9f2d998e",
        "cef03f24-b0c7-4145-94d4-a84f81bb588c"],
        "adjustedStart": "00:00:07.2400000",
        "adjustedEnd": "00:00:45.6780000",
        "start": "00:00:07.2400000",
        "end": "00:00:45.6780000"
    },
    {
        "thumbnailsIds": ["00000000-51e5-4260-91a5-890fa05c68b0"],
        "adjustedStart": "00:10:23.9570000",
        "adjustedEnd": "00:10:39.2390000",
        "start": "00:10:23.9570000",
        "end": "00:10:39.2390000"
    }]
}]
```

#### <a name="labels"></a>rótulos

|NOME|DESCRIÇÃO|
|---|---|
|ID|A ID do rótulo.|
|Nome|O nome do rótulo (por exemplo, "Computador", "TV").|
|Linguagem|O idioma do nome do rótulo (quando traduzido). BCP-47|
|instances|Uma lista de intervalos de tempo nos quais esse rótulo apareceu (um rótulo pode aparecer várias vezes). Cada instância tem um campo de confiança. |


```json
"labels": [
    {
      "id": 0,
      "name": "person",
      "language": "en-US",
      "instances": [
        {
          "confidence": 1.0,
          "start": "00: 00: 00.0000000",
          "end": "00: 00: 25.6000000"
        },
        {
          "confidence": 1.0,
          "start": "00: 01: 33.8670000",
          "end": "00: 01: 39.2000000"
        }
      ]
    },
    {
      "name": "indoor",
      "language": "en-US",
      "id": 1,
      "instances": [
        {
          "confidence": 1.0,
          "start": "00: 00: 06.4000000",
          "end": "00: 00: 07.4670000"
        },
        {
          "confidence": 1.0,
          "start": "00: 00: 09.6000000",
          "end": "00: 00: 10.6670000"
        },
        {
          "confidence": 1.0,
          "start": "00: 00: 11.7330000",
          "end": "00: 00: 20.2670000"
        },
        {
          "confidence": 1.0,
          "start": "00: 00: 21.3330000",
          "end": "00: 00: 25.6000000"
        }
      ]
    }
  ] 
```

#### <a name="shots"></a>shots

|NOME|DESCRIÇÃO|
|---|---|
|ID|A ID da captura.|
|keyFrames|Uma lista com os quadros-chave dentro da captura (cada um tem uma ID e uma lista de intervalos de tempo de instâncias). As instâncias de frames principais têm um campo thumbnailId com o ID de miniatura da keyFrame.|
|instances|Uma lista com os intervalos de tempo desta captura (as capturas têm apenas 1 instância).|

```json
"Shots": [
    {
      "id": 0,
      "keyFrames": [
        {
          "id": 0,
          "instances": [
            {
          "thumbnailId": "00000000-0000-0000-0000-000000000000",
              "start": "00: 00: 00.1670000",
              "end": "00: 00: 00.2000000"
            }
          ]
        }
      ],
      "instances": [
        {
       "thumbnailId": "00000000-0000-0000-0000-000000000000",   
          "start": "00: 00: 00.2000000",
          "end": "00: 00: 05.0330000"
        }
      ]
    },
    {
      "id": 1,
      "keyFrames": [
        {
          "id": 1,
          "instances": [
            {
          "thumbnailId": "00000000-0000-0000-0000-000000000000",        
              "start": "00: 00: 05.2670000",
              "end": "00: 00: 05.3000000"
            }
          ]
        }
      ],
      "instances": [
        {
      "thumbnailId": "00000000-0000-0000-0000-000000000000",
          "start": "00: 00: 05.2670000",
          "end": "00: 00: 10.3000000"
        }
      ]
    }
  ]
```

#### <a name="brands"></a>marcas

Nomes de marcas comerciais e de produtos detectados na fala para transcrição de texto e / ou Vídeo OCR. Isso não inclui reconhecimento visual de marcas ou detecção de logotipo.

|NOME|DESCRIÇÃO|
|---|---|
|ID|A ID de marca.|
|Nome|O nome de marcas.|
|referenceId | O sufixo do URL da Wikipédia da marca. Por exemplo, "Target_Corporation" é o sufixo de [https://en.wikipedia.org/wiki/Target_Corporation](https://en.wikipedia.org/wiki/Target_Corporation).
|referenceUrl | A marca da url da Wikipedia, se existir. Por exemplo, [https://en.wikipedia.org/wiki/Target_Corporation](https://en.wikipedia.org/wiki/Target_Corporation).
|Descrição|A descrição de marcas.|
|marcas|Uma lista de tags predefinidas associadas a essa marca.|
|confidence|O valor de confiança do detector de marca indexador de vídeo (0-1).|
|instances|Uma lista de intervalos de tempo desta marca. Cada instância tem um brandType, que indica se essa marca apareceu na transcrição ou no OCR.|

```json
"brands": [
{
    "id": 0,
    "name": "MicrosoftExcel",
    "referenceId": "Microsoft_Excel",
    "referenceUrl": "http: //en.wikipedia.org/wiki/Microsoft_Excel",
    "referenceType": "Wiki",
    "description": "Microsoft Excel is a sprea..",
    "tags": [],
    "confidence": 0.975,
    "instances": [
    {
        "brandType": "Transcript",
        "start": "00: 00: 31.3000000",
        "end": "00: 00: 39.0600000"
    }
    ]
},
{
    "id": 1,
    "name": "Microsoft",
    "referenceId": "Microsoft",
    "referenceUrl": "http: //en.wikipedia.org/wiki/Microsoft",
    "description": "Microsoft Corporation is...",
    "tags": [
    "competitors",
    "technology"
    ],
    "confidence": 1.0,
    "instances": [
    {
        "brandType": "Transcript",
        "start": "00: 01: 44",
        "end": "00: 01: 45.3670000"
    },
    {
        "brandType": "Ocr",
        "start": "00: 01: 54",
        "end": "00: 02: 45.3670000"
    }
    ]
}
]
```

#### <a name="statistics"></a>Estatísticas

|NOME|DESCRIÇÃO|
|---|---|
|CorrespondenceCount|Número de correspondências no vídeo.|
|WordCount|O número de palavras por alto-falante.|
|SpeakerNumberOfFragments|A quantidade de fragmentos que o orador tem em um vídeo.|
|SpeakerLongestMonolog|O mais longo monólogo do orador. Se o falante tiver silêncios dentro do monólogo, ele será incluído. O silêncio no início e no final do monólogo é removido.| 
|SpeakerTalkToListenRatio|O cálculo é baseado no tempo gasto no monólogo do locutor (sem o silêncio intermediário) dividido pelo tempo total do vídeo. A hora é arredondada para o terceiro ponto decimal.|

#### <a name="audioeffects"></a>audioEffects

|NOME|DESCRIÇÃO|
|---|---|
|ID|A ID do efeito de áudio.|
|Tipo|O tipo de efeito de áudio (por exemplo, Palmas, Fala, Silêncio).|
|instances|Uma lista com os intervalos de tempo nos quais esse efeito de áudio apareceu.|

```json
"audioEffects": [
{
    "id": 0,
    "type": "Clapping",
    "instances": [
    {
        "start": "00:00:00",
        "end": "00:00:03"
    },
    {
        "start": "00:01:13",
        "end": "00:01:21"
    }
    ]
}
]
```

#### <a name="sentiments"></a>sentiments

Os sentimentos são agregadas de acordo com seu campo sentimentType (Positivo/Neutro/Negativo). Por exemplo, 0-0.1, 0.1-0.2.

|NOME|DESCRIÇÃO|
|---|---|
|ID|A ID do sentimento.|
|averageScore |A média de todas as pontuações de todas as instâncias desse tipo de sentimento - Neutral/positivo/negativo|
|instances|Uma lista com os intervalos de tempo nos quais esse sentimento apareceu.|
|sentimentType |O tipo pode ser 'Positivo', 'Neutro' ou 'Negativo'.|

```json
"sentiments": [
{
    "id": 0,
    "averageScore": 0.87,
    "sentimentType": "Positive",
    "instances": [
    {
        "start": "00:00:23",
        "end": "00:00:41"
    }
    ]
}, {
    "id": 1,
    "averageScore": 0.11,
    "sentimentType": "Positive",
    "instances": [
    {
        "start": "00:00:13",
        "end": "00:00:21"
    }
    ]
}
]
```

#### <a name="visualcontentmoderation"></a>visualContentModeration

O bloco visualContentModeration contém intervalos de tempo que o Video Indexer encontrou para potencialmente ter conteúdo adulto. Se visualContentModeration estiver vazio, não haverá conteúdo adulto identificado.

Os vídeos que contêm conteúdo adulto ou atraente podem estar disponíveis apenas para visualização privada. Os usuários têm a opção de enviar uma solicitação para uma revisão humana do conteúdo. Nesse caso, o atributo IsAdult conterá o resultado da revisão humana.

|NOME|DESCRIÇÃO|
|---|---|
|ID|A ID de moderação de conteúdo visual.|
|adultScore|A pontuação de conteúdo adulta (do moderador de conteúdo).|
|racyScore|A pontuação racista (de moderação de conteúdo).|
|instances|Uma lista de intervalos de tempo em que apareceu esse visual moderação de conteúdo.|

```json
"VisualContentModeration": [
{
    "id": 0,
    "adultScore": 0.00069,
    "racyScore": 0.91129,
    "instances": [
    {
        "start": "00:00:25.4840000",
        "end": "00:00:25.5260000"
    }
    ]
},
{
    "id": 1,
    "adultScore": 0.99231,
    "racyScore": 0.99912,
    "instances": [
    {
        "start": "00:00:35.5360000",
        "end": "00:00:35.5780000"
    }
    ]
}
] 
```

#### <a name="textualconentmoderation"></a>textualConentModeration 

|NOME|DESCRIÇÃO|
|---|---|
|ID|O ID de moderação do conteúdo textual.|
|bannedWordsCount |O número de palavras proibidas.|
|bannedWordsRatio |A proporção do número total de palavras.|


## <a name="next-steps"></a>Próximas etapas

[API de indexador de vídeo](https://api-portal.videoindexer.ai)

Para obter informações sobre como incorporar widgets em seu aplicativo, consulte [Incorporar widgets do Video Indexer aos seus aplicativos](video-indexer-embed-widgets.md). 

