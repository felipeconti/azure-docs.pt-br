---
title: Noções básicas sobre o tempo de execução do Azure IoT Edge | Microsoft Docs
description: Saiba mais sobre o tempo de execução do Azure IoT Edge e como ele fortalece seus dispositivos de borda
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 08/13/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: f832b05969c028880f6e375ff4a2ee8dc7a7eaf4
ms.sourcegitcommit: 4ea0cea46d8b607acd7d128e1fd4a23454aa43ee
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/15/2018
ms.locfileid: "42141820"
---
# <a name="understand-the-azure-iot-edge-runtime-and-its-architecture"></a>Reconhecer o tempo de execução do Azure IoT Edge e sua arquitetura

O tempo de execução do IoT Edge é uma coleção de programas que precisam ser instalados em um dispositivo para ser considerado um dispositivo IoT Edge. Coletivamente, os componentes do tempo de execução do IoT Edge permitem que dispositivos IoT Edge recebam o código para executar na borda e informem os resultados. 

O tempo de execução do IoT Edge executa as seguintes funções em dispositivos IoT Edge:

* Instala e atualiza as cargas de trabalho no dispositivo.
* Mantém os padrões de segurança do Azure IoT Edge no dispositivo.
* Garante que os módulos do [IoT Edge][lnk-modules] sempre estejam em execução.
* Fornece um relatório sobre a integridade do módulo para a nuvem para o monitoramento remoto.
* Facilita a comunicação entre os dispositivos de folha de downstream e os dispositivos IoT Edge.
* Facilita a comunicação entre os módulos e o dispositivo IoT Edge.
* Facilita a comunicação entre o dispositivo IoT Edge e a nuvem.

![O tempo de execução do IoT Edge comunica insights e integridade de módulo para o Hub IoT][1]

As responsabilidades do tempo de execução do IoT Edge se enquadram em duas categorias: gerenciamento de módulo e comunicação. Essas duas funções são executadas por dois componentes que representam o tempo de execução do IoT Edge. O hub do IoT Edge é responsável pela comunicação, enquanto o agente do IoT Edge gerencia a implantação e o monitoramento de módulos. 

O agente do Edge e o hub do Edge são módulos, assim como qualquer outro módulo em execução em um dispositivo IoT Edge. 

## <a name="iot-edge-hub"></a>Hub do IoT Edge

O hub do Edge é um dos dois módulos que compõem o tempo de execução do Azure IoT Edge. Ele atua como um proxy local para o Hub IoT expondo os mesmos pontos de extremidade de protocolo que o Hub IoT. Essa consistência significa que os clientes (sejam dispositivos ou módulos) podem se conectar no tempo de execução do IoT Edge como faria para no Hub IoT. 

>[!NOTE]
>O hub do Edge dá suporte a clientes que se conectam usando MQTT ou AMQP. Ele não oferece suporte a clientes que usam HTTP. 

O hub do Edge não é uma versão completa do Hub IoT executado localmente. Há algumas coisas que o hub do Edge delega para o Hub IoT silenciosamente. Por exemplo, o hub do Edge encaminha solicitações de autenticação para o Hub IoT quando um dispositivo tenta se conectar. Depois que a primeira conexão é estabelecida, as informações de segurança são armazenadas em cache localmente pelo hub do Edge. São permitidas conexões subsequentes desse dispositivo sem ele precisar se autenticar na nuvem. 

>[!NOTE]
>O tempo de execução deve ser conectado toda vez que tenta autenticar um dispositivo.

Para reduzir a largura de banda que a solução IoT Edge usa, o hub do Edge otimiza quantas conexões reais são feitas com a nuvem. O hub do Edge usa conexões lógicas de clientes como módulos ou dispositivos de folha e combina-os em uma única conexão física com a nuvem. Os detalhes desse processo são transparentes para o restante da solução. Os clientes pensam que têm sua própria conexão para a nuvem, mesmo que estejam todos sendo enviados pela mesma conexão. 

![O hub do Edge funciona como um gateway entre vários dispositivos físicos e a nuvem][2]

O hub do Edge pode determinar se ele está conectado ao Hub IoT. Se a conexão for perdida, o hub do Edge salvará mensagens ou as atualizações duplicadas localmente. Depois que uma conexão é restabelecida, ela sincroniza todos os dados. O local usado para esse cache temporário é determinado por uma propriedade do gêmeo do módulo do hub do Edge. O tamanho do cache não tem limite e aumentará até atingir toda a capacidade de armazenamento do dispositivo. 

### <a name="module-communication"></a>Comunicação do módulo

O hub do Edge facilita a comunicação de módulo para módulo. O uso do hub do Edge como um agente de mensagem mantém os módulos independentes uns dos outros. Os módulos precisam apenas especificar as entradas nas quais eles aceitam mensagens e as saídas nas quais eles gravam mensagens. Um desenvolvedor de soluções junta essas entradas e saídas para que os módulos processem os dados na ordem específica para essa solução. 

![O hub do Edge facilita a comunicação de módulo para módulo][3]

Para enviar dados ao hub do Edge, um módulo chamará o método SendEventAsync. O primeiro argumento especifica em qual saída enviar a mensagem. O pseudocódigo a seguir envia uma mensagem na saída1:

   ```csharp
   ModuleClient client = new ModuleClient.CreateFromEnvironmentAsync(transportSettings); 
   await client.OpenAsync(); 
   await client.SendEventAsync(“output1”, message); 
   ```

Para receber uma mensagem, registre um retorno de chamada que processa as mensagens recebidas em uma entrada específica. O pseudocódigo a seguir registra a função messageProcessor a ser usada para processar todas as mensagens recebidas em entrada1:

   ```csharp
   await client.SetEventHandlerAsync(“input1”, messageProcessor, userContext);
   ```

O desenvolvedor da solução é responsável por especificar as regras que determinam como o hub do Edge passa mensagens entre os módulos. As regras de roteamento são definidas na nuvem e propagadas para o hub do Edge no seu dispositivo gêmeo. A mesma sintaxe para rotas do Hub IoT é usada para definir rotas entre módulos no Azure IoT Edge. 

<!--- For more info on how to declare routes between modules, see []. --->   

![Rotas entre módulos][4]

## <a name="iot-edge-agent"></a>Agente do IoT Edge

O agente do IoT Edge é o outro módulo que compõe o tempo de execução do Azure IoT Edge. Ele é responsável por instanciar módulos, fazendo com que continuem a funcionar, e indicar o status dos módulos para o Hub IoT. Assim como qualquer outro módulo, o agente do Edge usa seu módulo gêmeo para armazenar esses dados de configuração. 

O [daemon de segurança do IoT Edge](iot-edge-security-manager.md) inicia o agente do Edge na inicialização do dispositivo. O agente recupera seu módulo gêmeo do Hub IoT e inspeciona o manifesto de implantação. O manifesto de implantação é um arquivo JSON que declara os módulos que precisam ser iniciados. 

Cada item no manifesto de implantação contém informações específicas sobre um módulo e é usado pelo agente o Edge para controlar o ciclo de vida do módulo. Estas são algumas das propriedades mais interessantes: 

* **Settings.Image**: a imagem de contêiner que o agente do Edge usa para iniciar o módulo. O agente do Edge deverá ser configurado com as credenciais para o registro de contêiner se a imagem estiver protegida por senha. As credenciais para o registro de contêiner pode ser configurado remotamente usando o manifesto de implantação ou no próprio dispositivo Edge atualizando o `config.yaml` arquivo na pasta de programa do IoT Edge.
* **settings.createOptions**: uma cadeia de caracteres que é passada diretamente para o daemon do Docker ao iniciar o contêiner do módulo. A adição de opções de Docker a esta propriedade permite opções avançadas, como encaminhamento de porta ou montagem de volumes no contêiner do módulo.  
* **status**: o estado no qual o agente do Edge coloca o módulo. Geralmente, esse valor é definido como *executando*, já que a maioria das pessoas deseja que o agente do Edge inicie imediatamente todos os módulos no dispositivo. No entanto, você pode especificar o estado inicial de um módulo para ser interrompido e aguardar para mandar o agente do Edge iniciar um módulo. O agente do Edge relata o status de cada módulo para a nuvem nas propriedades relatadas. Uma diferença entre a propriedade desejada e a propriedade relatada é um indicador de um dispositivo com comportamento inadequado. Os status com suporte são:
   * Baixando
   * Executando
   * Não Íntegro
   * Com falha
   * Parado
* **restartPolicy**: como o agente do Edge reinicia um módulo. Os valores possíveis incluem:
   * Nunca: o agente do Edge nunca reinicia o módulo.
   * onFailure: se o módulo falhar, o agente do Edge o reiniciará. Se o módulo é desligado corretamente, o agente do Edge não o reinicia.
   * Não íntegro: se o módulo falha ou é considerado não íntegro, o agente do Edge o reinicia.
   * Sempre: se o módulo falha, é considerado não íntegro ou é desligado de alguma forma, o agente do Edge o reinicia. 

O agente do IoT Edge envia a resposta de tempo de execução para o Hub IoT. Aqui está uma lista das possíveis respostas:
  * 200 - OK
  * 400 - A configuração de implantação está malformada ou inválida.
  * 417 - O dispositivo não tem uma configuração de implantação definida.
  * 412 - A versão do esquema na configuração de implantação é inválida.
  * 406 - O dispositivo de borda está offline ou não está enviando relatórios de status.
  * 500 - Ocorreu um erro no tempo de execução de borda.

### <a name="security"></a>Segurança

O agente do IoT Edge desempenha um papel fundamental na segurança de um dispositivo IoT Edge. Por exemplo, ele executa ações como verificar a imagem de um módulo antes de iniciá-lo. 

Para obter mais informações sobre a estrutura de segurança do Azure IoT Edge, leia sobre o [Gerenciador de segurança do IoT Edge](iot-edge-security-manager.md)

## <a name="next-steps"></a>Próximas etapas

[Entender os módulos do Azure IoT Edge][lnk-modules]

<!-- Images -->
[1]: ./media/iot-edge-runtime/Pipeline.png
[2]: ./media/iot-edge-runtime/Gateway.png
[3]: ./media/iot-edge-runtime/ModuleEndpoints.png
[4]: ./media/iot-edge-runtime/ModuleEndpointsWithRoutes.png

<!-- Links -->
[lnk-modules]: iot-edge-modules.md
