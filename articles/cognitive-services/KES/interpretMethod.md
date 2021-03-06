---
title: Método Interpretar na API de Serviço de Exploração de Conhecimento | Microsoft Docs
description: Saiba como usar o método Interpretar na API de KES (Serviço de Exploração de Conhecimento) em serviços Cognitivos.
services: cognitive-services
author: bojunehsu
manager: stesp
ms.service: cognitive-services
ms.component: knowledge-exploration
ms.topic: article
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: ef68d98dacf393abf8d030b9312217ea380947d2
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35363462"
---
# <a name="interpret-method"></a>Método Interpretar
O método *interpretar* utiliza uma cadeia de caracteres de consulta de linguagem natural e retorna interpretações formatadas de intenção do usuário com base nos dados do índice e de gramática.  Para fornecer uma experiência de pesquisa interativa, esse método pode ser chamado como cada caractere é digitado pelo usuário com o parâmetro *completo* definido como 1 para habilitar sugestões de preenchimento automático.

## <a name="request"></a>Solicitação
`http://<host>/interpret?query=<query>[&<options>]`

NOME|Valor| DESCRIÇÃO
----|----|----
query    | Cadeia de caracteres de texto | Consulta inserida pelo usuário.  Se o parâmetro completo estiver definido como 1, a consulta será interpretada como um prefixo para gerar sugestões de preenchimento automático de consulta.        
concluído | 0 (padrão) ou 1 | 1 significa que as sugestões de preenchimento automático são geradas com base nos dados de índice e gramática.         
count    | Número (padrão = 10) | Número máximo de interpretações para retornar.         
deslocamento   | Número (padrão = 0) | Índice da primeira interpretação para retornar.  Por exemplo, *count=2&offset=0* retorna as interpretações 0 e 1. *count=2&offset=2* retorna interpretações 2 e 3.       
Tempo limite  | Número (padrão = 1000) | Tempo limite em milissegundos. Somente interpretações localizadas antes que o tempo limite tenha decorrido serão retornadas.

Usando os parâmetros *contagem* e *deslocamento*, um grande número de resultados pode ser obtido incrementalmente por várias solicitações.

## <a name="response-json"></a>Resposta (JSON)
JSONPath     | DESCRIÇÃO
---------|---------
$.query |O parâmetro *query* da solicitação.
$.interpretations   |Matriz de 0 ou mais maneiras de corresponder a consulta de entrada em relação à gramática.
$.interpretations[\*].logprob   |A probabilidade de log relativa da interpretação (<=0).  Valores maiores são mais prováveis.
$.interpretations[\*].parse |Cadeia de caracteres XML que mostra como cada parte da consulta foi interpretada.
$.interpretations[\*].rules |Matriz de 1 ou mais regras definidas na gramática invocadas durante a interpretação.
$.interpretations[\*].rules[\*].name    |Nome da regra.
$.interpretations[\*].rules[\*].output  |Saída semântica da regra.
$.interpretations[\*].rules[\*].output.type |Tipo de dados de saída semântica.
$.interpretations[\*].rules[\*].output.value|Valores da saída semântica.  
$.aborted | Verdadeiro se a solicitação atingiu o tempo limite.

### <a name="parse-xml"></a>Análise XML
Análise XML anota a consulta (concluída) com informações sobre como ele corresponde com as regras na gramática e atributos no índice.  Abaixo está um exemplo do domínio de publicações acadêmicas:

```xml
<rule name="#GetPapers">
  papers by 
  <attr name="academic#Author.Name" canonical="heungyeung shum">harry shum</attr>
  <rule name="#GetPaperYear">
    written in
    <attr name="academic#Year">2000</attr>
  </rule>
</rule>
```

O elemento `<rule>` delimita o intervalo da consulta correspondente a regra especificada pelo seu atributo `name`.  Podem ser aninhados quando a análise envolve referências de regra na gramática.

O elemento `<attr>` delimita o intervalo da consulta correspondente ao atributo de índice especificado pelo seu atributo `name`.  Quando a correspondência envolve um sinônimo na consulta de entrada, o atributo `canonical` conterá o valor canônico sinônimo do índice de correspondência.

## <a name="example"></a>Exemplo
No exemplo de publicações acadêmicas, a solicitação a seguir retorna até 2 sugestões de preenchimento automático para a consulta de prefixo "artigos de jaime":

`http://<host>/interpret?query=papers by jaime&complete=1&count=2`

A resposta contém as duas principais (count=2) interpretações mais prováveis que completam a entrada parcial do usuário “artigos de jaime”: “artigos de jaime teevan” e “artigos de jaime green”.  O serviço gerou consultas concluídas em vez de considerar apenas correspondências exatas para o autor “jaime” porque a solicitação especificou “complete=1”. Observe que o valor canônico “j l green” foi correspondido por meio do sinônimo “jamie green”, conforme indicado na análise.


```json
{
  "query": "papers by jaime",
  "interpretations": [
    {
      "logprob": -5.615,
      "parse": "<rule name=\"#GetPapers\">papers by <attr name=\"academic#Author.Name\">jaime teevan</attr></rule>",
      "rules": [
        {
          "name": "#GetPapers",
          "output": {
            "type": "query",
            "value": "Composite(Author.Name=='jaime teevan')"
          }
        }
      ]
    },
    {
      "logprob": -5.849,
      "parse": "<rule name=\"#GetPapers\">papers by <attr name=\"academic#Author.Name\" canonical=\"j l green\">jaime green</attr></rule>",
      "rules": [
        {
          "name": "#GetPapers",
          "output": {
            "type": "query",
            "value": "Composite(Author.Name=='j l green')"
          }
        }
      ]
    }
  ]
}
```  

Quando o tipo de saída semântica é "consulta", como neste exemplo, os objetos correspondentes podem ser recuperados, passando *output.value* para o [*avaliar*](evaluateMethod.md) API por meio de parâmetro *expr*.

`http://<host>/evaluate?expr=Composite(AA.AuN=='jaime teevan')`
  
