---
title: Gatilho de Grade de Eventos para o Azure Functions
description: Entenda como manipular a Grade de Eventos no Azure Functions.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
editor: ''
tags: ''
keywords: ''
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/23/2018
ms.author: glenga
ms.openlocfilehash: 850b30ff42b77fe0ab527a54b62ba0f77027f932
ms.sourcegitcommit: b5ac31eeb7c4f9be584bb0f7d55c5654b74404ff
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/23/2018
ms.locfileid: "42746239"
---
# <a name="event-grid-trigger-for-azure-functions"></a>Gatilho de Grade de Eventos para o Azure Functions

Este artigo explica como manipular com eventos de [Grade de Eventos](../event-grid/overview.md) no Azure Functions.

A Grade de Eventos é um serviço do Azure que envia solicitações HTTP para notificá-lo sobre eventos que acontecem nos *publicadores*. Um publicador é o serviço ou recurso que origina o evento. Por exemplo, uma conta de armazenamento de Blobs do Azure é um publicador, e [uma exclusão ou upload de blob é um evento](../storage/blobs/storage-blob-event-overview.md). Alguns [serviços do Azure têm suporte interno para publicar eventos na Grade de Eventos](../event-grid/overview.md#event-sources). 

Os *manipuladores* de eventos recebem e processam eventos. O Azure Functions é um dos vários serviços do[Azure que possuem suporte interno para manipular eventos da Grande de Eventos](../event-grid/overview.md#event-handlers). Neste artigo, você aprende a usar um gatilho de Grade de Eventos para invocar uma função quando um evento é recebido da Grade de Eventos.

Se você preferir, é possível utilizar um gatilho HTTP para manipular eventos da Grade de Eventos, consultado [Usar um gatilho HTTP como um gatilho de Grade de Eventos](#use-an-http-trigger-as-an-event-grid-trigger), posteriormente neste artigo. No momento, não é possível usar um gatilho da Grade de Eventos para um aplicativo do Azure Functions quando o evento é entregue no [esquema CloudEvents](../event-grid/cloudevents-schema.md). Em vez disso, use um gatilho HTTP.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>Pacotes - Functions 1. x

O gatilho de grade de eventos é fornecido no [Microsoft.Azure.WebJobs.Extensions.EventGrid](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.EventGrid) pacote NuGet, versão 1. x. O código-fonte do pacote está no repositório GitHub [azure-functions-eventgrid-extension](https://github.com/Azure/azure-functions-eventgrid-extension/tree/master).

[!INCLUDE [functions-package](../../includes/functions-package.md)]

## <a name="packages---functions-2x"></a>Pacotes - Functions 2. x

O gatilho de grade de eventos é fornecido no [Microsoft.Azure.WebJobs.Extensions.EventGrid](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.EventGrid) pacote NuGet, versão 2. x. O código-fonte do pacote está no repositório GitHub [azure-functions-eventgrid-extension](https://github.com/Azure/azure-functions-eventgrid-extension/tree/v2.x).

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="example"></a>Exemplo

Consulte o exemplo específico do idioma para um gatilho de Grade de Eventos:

* [C#](#c-example)
* [Script do C# (.csx)](#c-script-example)
* [JavaScript](#javascript-example)
* [Java](#trigger---java-example)

Para um exemplo de gatilho HTTP, consulte [Como usar o gatilho HTTP](#use-an-http-trigger-as-an-event-grid-trigger), posteriormente neste artigo.

### <a name="c-example"></a>Exemplo de C#

O exemplo a seguir mostra um funções 1. x [função C#](functions-dotnet-class-library.md) que associa a `JObject`:

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.EventGrid;
using Microsoft.Azure.WebJobs.Host;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;

namespace Company.Function
{
    public static class EventGridTriggerCSharp
    {
        [FunctionName("EventGridTriggerCSharp")]
        public static void Run([EventGridTrigger]JObject eventGridEvent, TraceWriter log)
        {
            log.Info(eventGridEvent.ToString(Formatting.Indented));
        }
    }
}
```

O exemplo a seguir mostra um funções 2. x [função C#](functions-dotnet-class-library.md) que associa a `EventGridEvent`:

```cs
using Microsoft.Azure.EventGrid.Models;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.EventGrid;
using Microsoft.Azure.WebJobs.Host;

namespace Company.Function
{
    public static class EventGridTriggerCSharp
    {
        [FunctionName("EventGridTest")]
        public static void EventGridTest([EventGridTrigger]EventGridEvent eventGridEvent, TraceWriter log)
        {
            log.Info(eventGridEvent.Data.ToString());
        }
    }
}
```

Para obter mais informações, consulte [Pacotes](#packages), [Atributos](#attributes), [Configuração](#configuration) e [Uso](#usage).

### <a name="c-script-example"></a>Exemplo 2 de C# script

O exemplo a seguir mostra uma associação de gatilho em um arquivo *function.json* e uma [função de script de C#](functions-reference-csharp.md) que usa a associação.

Aqui estão os dados de associação no arquivo *function.json*:

```json
{
  "bindings": [
    {
      "type": "eventGridTrigger",
      "name": "eventGridEvent",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

Aqui está o código de script de 1. x C# funções que associa a `JObject`:

```cs
#r "Newtonsoft.Json"

using Newtonsoft.Json;
using Newtonsoft.Json.Linq;

public static void Run(JObject eventGridEvent, TraceWriter log)
{
    log.Info(eventGridEvent.ToString(Formatting.Indented));
}
```

Aqui está o código de script de 2. x C# funções que associa a `EventGridEvent`:

```csharp
#r "Microsoft.Azure.EventGrid"
using Microsoft.Azure.EventGrid.Models;

public static void Run(EventGridEvent eventGridEvent, TraceWriter log)
{
    log.Info(eventGridEvent.Data.ToString());
}
```

Para obter mais informações, consulte [Pacotes](#packages), [Atributos](#attributes), [Configuração](#configuration) e [Uso](#usage).

### <a name="javascript-example"></a>Exemplo de JavaScript

O exemplo a seguir mostra uma associação de gatilho em um arquivo *function.json* e uma [função JavaScript](functions-reference-node.md) que usa a associação.

Aqui estão os dados de associação no arquivo *function.json*:

```json
{
  "bindings": [
    {
      "type": "eventGridTrigger",
      "name": "eventGridEvent",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

Aqui está o código JavaScript:

```javascript
module.exports = function (context, eventGridEvent) {
    context.log("JavaScript Event Grid function processed a request.");
    context.log("Subject: " + eventGridEvent.subject);
    context.log("Time: " + eventGridEvent.eventTime);
    context.log("Data: " + JSON.stringify(eventGridEvent.data));
    context.done();
};
```

### <a name="trigger---java-example"></a>Gatilho - exemplo Java

O exemplo a seguir mostra uma ligação de acionador em um arquivo *function.json* e uma [função Java](functions-reference-java.md) que usa a ligação e imprime um evento.

```json
{
  "bindings": [
    {
      "type": "eventGridTrigger",
      "name": "eventGridEvent",
      "direction": "in"
    }
  ]
}
```

Aqui está o código Java:

```java
@FunctionName("eventGridMonitor")
  public void logEvent(
     @EventGridTrigger(name = "event") String content,
      final ExecutionContext context
  ) { 
      context.getLogger().info(content);
    }
```

No [biblioteca de tempo de execução de funções Java](/java/api/overview/azure/functions/runtime), use o `EventGridTrigger` anotação em parâmetros cujo valor virá do EventGrid. Parâmetros com essas anotações fazem com que a função seja executada quando um evento é recebido.  Essa anotação pode ser usada com tipos nativos do Java, POJOs ou valores que permitem valor nulos usando `Optional<T>`. 
     
## <a name="attributes"></a>Atributos

Em [bibliotecas de classes de C#](functions-dotnet-class-library.md), utilize o atributo [EventGridTrigger](https://github.com/Azure/azure-functions-eventgrid-extension/blob/master/src/EventGridExtension/EventGridTriggerAttribute.cs).

Aqui está um atributo `EventGridTrigger` em uma assinatura de método:

```csharp
[FunctionName("EventGridTest")]
public static void EventGridTest([EventGridTrigger] JObject eventGridEvent, TraceWriter log)
{
    ...
}
 ```

Para ver um exemplo completo, consulte [Exemplo de C#](#c-example).

## <a name="configuration"></a>Configuração

A tabela a seguir explica as propriedades de configuração de associação que você define no arquivo *function.json*. Não há parâmetros ou propriedades do construtor para definir o atributo `EventGridTrigger`.

|Propriedade function.json |DESCRIÇÃO|
|---------|---------|----------------------|
| **tipo** | Obrigatório – deve ser definido como `eventGridTrigger`. |
| **direction** | Obrigatório – deve ser definido como `in`. |
| **name** | Obrigatório - o nome da variável usado no código de função para o parâmetro que recebe os dados de eventos. |

## <a name="usage"></a>Uso

Para C# e F# funções no Azure funciona 1. x, você pode usar os seguintes tipos de parâmetro para o disparador de grade de eventos:

* `JObject`
* `string`

Para C# e F# funções nas funções do Azure 2. x, você também tem a opção de usar o seguinte tipo de parâmetro para o disparador de grade de eventos:

* `Microsoft.Azure.EventGrid.Models.EventGridEvent`- Define propriedades para os campos comuns a todos os tipos de eventos.

> [!NOTE]
> Em funções v1 se você tentar associar ao `Microsoft.Azure.WebJobs.Extensions.EventGrid.EventGridEvent`, o compilador exibirá uma mensagem "substituído" e avisá-lo para usar `Microsoft.Azure.EventGrid.Models.EventGridEvent` em vez disso. Para usar o tipo mais recente, fazer referência a [Microsoft.Azure.EventGrid](https://www.nuget.org/packages/Microsoft.Azure.EventGrid) NuGet empacotar e qualificar totalmente o `EventGridEvent` nome do tipo, prefixando-o com `Microsoft.Azure.EventGrid.Models`. Para obter informações sobre como fazer referência a pacotes do NuGet em uma função de script C#, consulte [pacotes usando o NuGet](functions-reference-csharp.md#using-nuget-packages)

Para funções JavaScript, o parâmetro nomeado pela propriedade *function.json* `name` tem uma referência ao objeto de evento.

## <a name="event-schema"></a>Esquema do evento

Os dados de uma Grade de Eventos são recebidos como um objeto JSON no corpo de uma solicitação HTTP. O JSON é semelhante ao exemplo a seguir:

```json
[{
  "topic": "/subscriptions/{subscriptionid}/resourceGroups/eg0122/providers/Microsoft.Storage/storageAccounts/egblobstore",
  "subject": "/blobServices/default/containers/{containername}/blobs/blobname.jpg",
  "eventType": "Microsoft.Storage.BlobCreated",
  "eventTime": "2018-01-23T17:02:19.6069787Z",
  "id": "{guid}",
  "data": {
    "api": "PutBlockList",
    "clientRequestId": "{guid}",
    "requestId": "{guid}",
    "eTag": "0x8D562831044DDD0",
    "contentType": "application/octet-stream",
    "contentLength": 2248,
    "blobType": "BlockBlob",
    "url": "https://egblobstore.blob.core.windows.net/{containername}/blobname.jpg",
    "sequencer": "000000000000272D000000000003D60F",
    "storageDiagnostics": {
      "batchId": "{guid}"
    }
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

O exemplo mostrado é uma matriz de um elemento. A Grade de Eventos envia sempre uma matriz e pode enviar mais de um evento na matriz. O tempo de execução invoca sua função uma vez para cada elemento da matriz.

As propriedades de nível superior nos dados JSON de evento serão as mesmas entre todos os tipos de eventos, enquanto os conteúdos da propriedade `data` estiverem especificados para cada tipo de evento. O exemplo mostrado é para um evento de armazenamento de Blobs.

Para obter explicações sobre as propriedades comuns e específicas de evento, consulte [Propriedades do evento](../event-grid/event-schema.md#event-properties) na documentação da Grade de Eventos.

O tipo `EventGridEvent` define apenas as propriedades de nível superior; a propriedade `Data` é um `JObject`. 

## <a name="create-a-subscription"></a>Criar uma assinatura

Para iniciar o recebimento de solicitações HTTP de Grade de Eventos, crie uma assinatura na Grade de Eventos que especifique a URL do ponto de extremidade que invoca a função.

### <a name="azure-portal"></a>Portal do Azure

Para as funções que você desenvolve no Portal do Azure com o gatilho de Grade de Eventos, selecione **Adicionar assinatura da Grade de Eventos**.

![Criar assinatura no portal](media/functions-bindings-event-grid/portal-sub-create.png)

Ao selecionar esse link, o portal abrirá a página **Criar Assinatura de Evento**  com a URL do ponto de extremidade preenchida.

![URL do ponto de extremidade preenchida](media/functions-bindings-event-grid/endpoint-url.png)

Para obter mais informações sobre como criar assinaturas usando o Portal do Azure, consulte [Criar evento personalizado - Portal do Azure](../event-grid/custom-event-quickstart-portal.md) na documentação da Grade de Eventos.

### <a name="azure-cli"></a>CLI do Azure

Para criar uma assinatura usando [a CLI do Azure](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest), use o comando [az eventgrid event-subscription create](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az-eventgrid-event-subscription-create).

O comando requer a URL do ponto de extremidade que invoca a função. O exemplo a seguir mostra o padrão de URL:

```
https://{functionappname}.azurewebsites.net/admin/extensions/EventGridExtensionConfig?functionName={functionname}&code={systemkey}
```

A chave do sistema é uma chave de autorização que deve ser incluída na URL do ponto de extremidade para um gatilho de Grade de Eventos. A seção a seguir explica como obter a chave do sistema.

Apresentamos aqui um exemplo que assina em uma conta de armazenamento de Blobs (com um espaço reservado para a chave do sistema):

```azurecli
az eventgrid resource event-subscription create -g myResourceGroup \
--provider-namespace Microsoft.Storage --resource-type storageAccounts \
--resource-name glengablobstorage --name myFuncSub  \
--included-event-types Microsoft.Storage.BlobCreated \
--subject-begins-with /blobServices/default/containers/images/blobs/ \
--endpoint https://glengastorageevents.azurewebsites.net/admin/extensions/EventGridExtensionConfig?functionName=imageresizefunc&code=LUwlnhIsNtSiUjv/sNtSiUjvsNtSiUjvsNtSiUjvYb7XDonDUr/RUg==
```

Para obter mais informações sobre como criar uma assinatura, consulte o [Guia de início rápido do armazenamento de blobs](../storage/blobs/storage-blob-event-quickstart.md#subscribe-to-your-storage-account) ou outros guias de início rápido da Grade de Eventos.

### <a name="get-the-system-key"></a>Obter a chave do sistema

Você pode obter a chave do sistema usando a seguinte API (HTTP GET):

```
http://{functionappname}.azurewebsites.net/admin/host/systemkeys/eventgridextensionconfig_extension?code={adminkey}
```

Esta é uma API de administração, por isso, requer sua [chave mestre](functions-bindings-http-webhook.md#authorization-keys) do aplicativo. Não confunda a chave do sistema (para invocar uma função de gatilho de grade de eventos) com a chave mestra (para executar tarefas administrativas no aplicativo de funções). Ao assinar em um tópico da Grade de Eventos, certifique-se de usar a chave do sistema. 

Aqui, está um exemplo da resposta que fornece a chave do sistema:

```
{
  "name": "eventgridextensionconfig_extension",
  "value": "{the system key for the function}",
  "links": [
    {
      "rel": "self",
      "href": "{the URL for the function, without the system key}"
    }
  ]
}
```

Para obter mais informações, consulte [Chaves de autorização](functions-bindings-http-webhook.md#authorization-keys) no artigo de referência de gatilho HTTP. 

Como alternativa, você mesmo pode enviar uma HTTP PUT para especificar o valor da chave.

## <a name="local-testing-with-viewer-web-app"></a>Teste local com o aplicativo Web visualizador

Para testar um gatilho de Grade de Eventos localmente, você deve receber solicitações HTTP de Grade de Eventos entre suas origens na nuvem para sua máquina local. Uma maneira de fazer isso é capturar solicitações online e manualmente reenviá-las em sua máquina local:

2. [Criar um aplicativo Web visualizador](#create-a-viewer-web-app) que captura as mensagens de evento.
3. [Criar uma assinatura da Grade de Eventos](#create-an-event-grid-subscription) que envia eventos para o aplicativo visualizador.
4. [Gerar uma solicitação](#generate-a-request) e copiar o corpo da solicitação do aplicativo visualizador.
5. [Postar manualmente a solicitação](#manually-post-the-request) para a URL localhost da sua função de gatilho da Grade de Eventos.

Quando terminar de testar, você poderá usar a mesma assinatura para a produção atualizando o ponto de extremidade. Use o comando da CLI do Azure[az eventgrid event-subscription update](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az-eventgrid-event-subscription-update).

### <a name="create-a-viewer-web-app"></a>Criar um aplicativo Web visualizador

Para simplificar as mensagens de evento de captura, implante um [aplicativo Web predefinido](https://github.com/Azure-Samples/azure-event-grid-viewer) que exibe as mensagens de evento. A solução implantada inclui um plano do Serviço de Aplicativo, um aplicativo Web do Aplicativo do Serviço de e o código-fonte do GitHub.

Selecione **Implantar no Azure** para implantar a solução na sua assinatura. No portal do Azure, forneça os valores para os parâmetros.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fazure-event-grid-viewer%2Fmaster%2Fazuredeploy.json" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a>

A implantação pode levar alguns minutos para ser concluída. Depois que a implantação for bem-sucedida, exiba seu aplicativo Web para garantir que ele esteja em execução. Em um navegador da Web, navegue até: `https://<your-site-name>.azurewebsites.net`

Você verá o site, mas nenhum evento ainda estará publicado.

![Exibir novo site](media/functions-bindings-event-grid/view-site.png)

### <a name="create-an-event-grid-subscription"></a>Criar uma assinatura na Grade de Eventos

Crie uma assinatura da Grade de Eventos do tipo que você deseja testar e forneça a ela a URL do aplicativo Web como o ponto de extremidade para a notificação de eventos. O ponto de extremidade para seu aplicativo Web deve incluir o sufixo `/api/updates/`. Portanto, a URL completa é `https://<your-site-name>.azurewebsites.net/api/updates`

Para obter mais informações sobre como criar assinaturas usando o portal do Azure, confira [Criar um evento personalizado – portal do Azure](../event-grid/custom-event-quickstart-portal.md) na documentação da Grade de Eventos.

### <a name="generate-a-request"></a>Gerar uma solicitação

Dispare um evento que gerará tráfego HTTP para o ponto de extremidade do aplicativo Web.  Por exemplo, se você criou uma assinatura de armazenamento de Blobs, faça upload ou exclua um blob. Quando uma solicitação for exibida no aplicativo Web, copie o corpo da solicitação.

A solicitação de validação de assinatura será recebida primeiro. Ignore quaisquer solicitações de validação e copie a solicitação de evento.

![Copiar o corpo da solicitação do aplicativo Web](media/functions-bindings-event-grid/view-results.png)

### <a name="manually-post-the-request"></a>Postar manualmente a solicitação

Execute sua função de Grade de Eventos localmente.

Use uma ferramenta como [Postman](https://www.getpostman.com/) ou [curl](https://curl.haxx.se/docs/httpscripting.html) para criar uma solicitação HTTP POST:

* Defina um cabeçalho `Content-Type: application/json`.
* Defina um cabeçalho `aeg-event-type: Notification`.
* Cole os dados RequestBin no corpo da solicitação. 
* Poste na URL da sua função de gatilho de Grade de Eventos, usando o seguinte padrão:

```
http://localhost:7071/admin/extensions/EventGridExtensionConfig?functionName={functionname}
``` 

O parâmetro `functionName` deverá ser o nome especificado no atributo `FunctionName`.

As capturas de tela a seguir mostram os cabeçalhos e o corpo da solicitação em Postman:

![Cabeçalhos em Postman](media/functions-bindings-event-grid/postman2.png)

![Corpo da solicitação em Postman](media/functions-bindings-event-grid/postman.png)

A função de gatilho da Grade de Eventos executa e mostra logs semelhantes ao exemplo a seguir:

![Amostra de logs da função de gatilho de Grade de Eventos](media/functions-bindings-event-grid/eg-output.png)

## <a name="local-testing-with-ngrok"></a>Teste local com ngrok

Outra maneira de testar um gatilho de Grade de Eventos localmente é automatizar a conexão HTTP entre a Internet e o computador de desenvolvimento. Você pode fazer isso com uma ferramenta de software livre nomeada [ngrok](https://ngrok.com/):

3. [Criar um ponto de extremidade ngrok](#create-an-ngrok-endpoint).
4. [Executar a função de gatilho de Grade de Eventos ](#run-the-event-grid-trigger-function).
5. [Criar uma assinatura na Grade de Eventos](#create-a-subscription) que envia eventos para o ponto de extremidade ngrok.
6. [Disparar um evento](#trigger-an-event).

Quando terminar de testar, você poderá usar a mesma assinatura para a produção atualizando o ponto de extremidade. Use o comando da CLI do Azure[az eventgrid event-subscription update](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az-eventgrid-event-subscription-update).

### <a name="create-an-ngrok-endpoint"></a>Criar um ponto de extremidade ngrok

Faça o download do *ngrok.exe* do [ngrok](https://ngrok.com/) e execute com o seguinte comando:

```
ngrok http -host-header=localhost 7071
```

O parâmetro -host-header é necessário porque o tempo de execução das funções espera solicitações do localhost quando é executado no localhost. 7071 é o número de porta padrão quando o tempo de execução é executado localmente.

O comando cria saída semelhante à seguinte:

```
Session Status                online
Version                       2.2.8
Region                        United States (us)
Web Interface                 http://127.0.0.1:4040
Forwarding                    http://263db807.ngrok.io -> localhost:7071
Forwarding                    https://263db807.ngrok.io -> localhost:7071

Connections                   ttl     opn     rt1     rt5     p50     p90
                              0       0       0.00    0.00    0.00    0.00
```

Você usará o URL `https://{subdomain}.ngrok.io` para sua inscrição na grade de eventos.

### <a name="run-the-event-grid-trigger-function"></a>Executar a função de gatilho de Grade de Eventos

A URL ngrok não recebe tratamento especial pela Grade de Eventos, portanto, sua função deverá ser executada localmente quando a assinatura for criada. Caso contrário, a resposta de validação não será enviada e a criação da assinatura falhará.

### <a name="create-a-subscription"></a>Criar uma assinatura

Crie uma assinatura de grade de eventos do tipo que você deseja testar e forneça seu ponto de extremidade ngrok.

Use este padrão de terminal para Funções 1.x:
```
https://{subdomain}.ngrok.io/admin/extensions/EventGridExtensionConfig?functionName={functionname}
``` 
Use este padrão de terminal para Funções 2.x:
```
https://{subdomain}.ngrok.io/runtime/webhooks/EventGridExtensionConfig?functionName={functionName}
``` 
O parâmetro `functionName` deverá ser o nome especificado no atributo `FunctionName`.

Aqui, está um exemplo usando a CLI do Azure:

```
az eventgrid event-subscription create --resource-id /subscriptions/aeb4b7cb-b7cb-b7cb-b7cb-b7cbb6607f30/resourceGroups/eg0122/providers/Microsoft.Storage/storageAccounts/egblobstor0122 --name egblobsub0126 --endpoint https://263db807.ngrok.io/admin/extensions/EventGridExtensionConfig?functionName=EventGridTrigger
```

Para obter informações sobre como criar uma assinatura, consulte [Criar uma assinatura](#create-a-subscription), posteriormente neste artigo.

### <a name="trigger-an-event"></a>Disparar um evento

Dispare um evento que gerará tráfego HTTP para o ponto de extremidade ngrok.  Por exemplo, se você criou uma assinatura de armazenamento de Blobs, faça upload ou exclua um blob.

A função de gatilho da Grade de Eventos executa e mostra logs semelhantes ao exemplo a seguir:

![Amostra de logs da função de gatilho de Grade de Eventos](media/functions-bindings-event-grid/eg-output.png)

## <a name="use-an-http-trigger-as-an-event-grid-trigger"></a>Use um gatilho HTTP como um gatilho de Grade de Eventos

Os eventos da Grade de Eventos são recebidos como solicitações HTTP, para que você possa manipular eventos usando um gatilho HTTP em vez de um gatilho de Grade de Eventos. Um possível motivo para isso é obter mais controle sobre a URL do ponto de extremidade que invoca a função. Outro motivo é quando você precisa receber eventos no [esquema CloudEvents](../event-grid/cloudevents-schema.md). Atualmente, o gatilho da Grade de Eventos não dá suporte ao esquema CloudEvents. Os exemplos desta seção mostram soluções para o esquema da Grade de Eventos e o esquema CloudEvents.

Se você usar um gatilho HTTP, será necessário gravar o código para o que o gatilho da Grade de Eventos automaticamente:

* Envie uma resposta de validação para [uma solicitação de validação de assinatura](../event-grid/security-authentication.md#webhook-event-delivery).
* Invoque a função uma vez por elemento da matriz de eventos contida no corpo da solicitação.

Para obter informações sobre a URL a ser utilizada para invocar a função localmente ou quando for executada no Azure, consulte a [documentação de referência de associação de gatilho HTTP](functions-bindings-http-webhook.md)

### <a name="event-grid-schema"></a>Esquema da Grade de Eventos

O código C# de exemplo a seguir para um gatilho HTTP simula o comportamento do gatilho da Grade de Eventos. Use este exemplo para eventos entregues no esquema da Grade de Eventos.

```csharp
[FunctionName("HttpTrigger")]
public static async Task<HttpResponseMessage> Run(
    [HttpTrigger(AuthorizationLevel.Anonymous, "post")]HttpRequestMessage req,
    TraceWriter log)
{
    log.Info("C# HTTP trigger function processed a request.");

    var messages = await req.Content.ReadAsAsync<JArray>();

    // If the request is for subscription validation, send back the validation code.
    if (messages.Count > 0 && string.Equals((string)messages[0]["eventType"], 
        "Microsoft.EventGrid.SubscriptionValidationEvent", 
        System.StringComparison.OrdinalIgnoreCase))
    {
        log.Info("Validate request received");
        return req.CreateResponse<object>(new
        {
            validationResponse = messages[0]["data"]["validationCode"]
        });
    }

    // The request is not for subscription validation, so it's for one or more events.
    foreach (JObject message in messages)
    {
        // Handle one event.
        EventGridEvent eventGridEvent = message.ToObject<EventGridEvent>();
        log.Info($"Subject: {eventGridEvent.Subject}");
        log.Info($"Time: {eventGridEvent.EventTime}");
        log.Info($"Event data: {eventGridEvent.Data.ToString()}");
    }

    return req.CreateResponse(HttpStatusCode.OK);
}
```

O código JavaScript de exemplo a seguir para um gatilho HTTP simula o comportamento do gatilho da Grade de Eventos. Use este exemplo para eventos entregues no esquema da Grade de Eventos.

```javascript
module.exports = function (context, req) {
    context.log('JavaScript HTTP trigger function processed a request.');

    var messages = req.body;
    // If the request is for subscription validation, send back the validation code.
    if (messages.length > 0 && messages[0].eventType == "Microsoft.EventGrid.SubscriptionValidationEvent") {
        context.log('Validate request received');
        var code = messages[0].data.validationCode;
        context.res = { status: 200, body: { "ValidationResponse": code } };
    }
    else {
        // The request is not for subscription validation, so it's for one or more events.
        // Event Grid schema delivers events in an array.
        for (var i = 0; i < messages.length; i++) {
            // Handle one event.
            var message = messages[i];
            context.log('Subject: ' + message.subject);
            context.log('Time: ' + message.eventTime);
            context.log('Data: ' + JSON.stringify(message.data));
        }
    }
    context.done();
};
```

O código de manipulação de eventos está dentro do loop através da matriz `messages`.

### <a name="cloudevents-schema"></a>Esquema CloudEvents

O código C# de exemplo a seguir para um gatilho HTTP simula o comportamento do gatilho da Grade de Eventos.  Use este exemplo para eventos entregues no esquema CloudEvents.

```csharp
[FunctionName("HttpTrigger")]
public static async Task<HttpResponseMessage> Run([HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)]HttpRequestMessage req, TraceWriter log)
{
    log.Info("C# HTTP trigger function processed a request.");

    var requestmessage = await req.Content.ReadAsStringAsync();
    var message = JToken.Parse(requestmessage);

    if (message.Type == JTokenType.Array)
    {
        // If the request is for subscription validation, send back the validation code.
        if (string.Equals((string)message[0]["eventType"],
        "Microsoft.EventGrid.SubscriptionValidationEvent",
        System.StringComparison.OrdinalIgnoreCase))
        {
            log.Info("Validate request received");
            return req.CreateResponse<object>(new
            {
                validationResponse = message[0]["data"]["validationCode"]
            });
        }
    }
    else
    {
        // The request is not for subscription validation, so it's for an event.
        // CloudEvents schema delivers one event at a time.
        log.Info($"Source: {message["source"]}");
        log.Info($"Time: {message["eventTime"]}");
        log.Info($"Event data: {message["data"].ToString()}");
    }

    return req.CreateResponse(HttpStatusCode.OK);
}
```

O código JavaScript de exemplo a seguir para um gatilho HTTP simula o comportamento do gatilho da Grade de Eventos. Use este exemplo para eventos entregues no esquema CloudEvents.

```javascript
module.exports = function (context, req) {
    context.log('JavaScript HTTP trigger function processed a request.');

    var message = req.body;
    // If the request is for subscription validation, send back the validation code.
    if (message.length > 0 && message[0].eventType == "Microsoft.EventGrid.SubscriptionValidationEvent") {
        context.log('Validate request received');
        var code = message[0].data.validationCode;
        context.res = { status: 200, body: { "ValidationResponse": code } };
    }
    else {
        // The request is not for subscription validation, so it's for an event.
        // CloudEvents schema delivers one event at a time.
        var event = JSON.parse(message);
        context.log('Source: ' + event.source);
        context.log('Time: ' + event.eventTime);
        context.log('Data: ' + JSON.stringify(event.data));
    }
    context.done();
};
```

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Aprenda mais sobre gatilhos e de associações do Azure Functions](functions-triggers-bindings.md)

> [!div class="nextstepaction"]
> [Saiba mais sobre a Grade de Eventos](../event-grid/overview.md)
