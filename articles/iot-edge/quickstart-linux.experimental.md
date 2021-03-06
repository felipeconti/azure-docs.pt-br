---
title: Simular o Azure IoT Edge no Linux | Microsoft Docs
description: Neste início rápido, aprenda a implantar código pré-compilado remotamente em um dispositivo IoT Edge.
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 07/02/2018
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: dfbe931bbe5887e9c0545558c4d2b2565718dd0a
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2018
ms.locfileid: "39578483"
---
# <a name="quickstart-deploy-your-first-iot-edge-module-to-a-linux-x64-device"></a>Início Rápido: implantar seu primeiro módulo IoT Edge em um dispositivo Linux x64

O Azure IoT Edge permite executar análise e processamento de dados em seus dispositivos em vez de enviar por push todos os dados para a nuvem. Os tutoriais do IoT Edge demonstram como implantar diferentes tipos de módulos, mas primeiro você precisa de um dispositivo para teste. 

Neste guia de início rápido, você aprende a:

1. Crie um Hub IoT.
2. Registrar um dispositivo IoT Edge em seu Hub IoT.
3. Iniciar o tempo de execução do IoT Edge.
4. Implantar um módulo em um dispositivo IoT Edge remotamente.

![Arquitetura do tutorial][2]

Este guia de início rápido transforma seu computador máquina virtual Linux em um dispositivo IoT Edge. Em seguida, você pode implantar um módulo do portal do Azure em seu dispositivo. O módulo implantado neste guia de início rápido é um sensor simulado que gera dados de temperatura, umidade e pressão. Os outros tutoriais do Azure IoT Edge se baseiam no trabalho feito aqui com a implantação de módulos que analisam os dados simulados para obter informações de negócios. 

Se você não tiver uma assinatura do Azure ativa, crie uma [conta gratuita][lnk-account] antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Este início rápido usa um computador Linux como dispositivo do IoT Edge. Se você não tiver uma disponível para teste, siga as instruções em [Criar uma máquina virtual Linux no portal do Azure](../virtual-machines/linux/quick-create-portal.md). 
* Você não precisa seguir as etapas para instalar e executar o servidor Web. Depois que você se conectar à sua máquina virtual, poderá parar.  
* Crie sua máquina virtual em um novo grupo de recursos, que você poderá usar ao criar o restante dos recursos do Azure para este início rápido. Use um nome reconhecível, como *IoTEdgeResources*. 
* Não é necessário um computador virtual muito grande para testar o IoT Edge. Um tamanho como **B1ms** é o suficiente. 

## <a name="create-an-iot-hub"></a>Crie um hub IoT

Comece o início rápido criando o Hub IoT no portal do Azure.
![Criar o Hub IoT][3]

[!INCLUDE [iot-hub-create-hub](../../includes/iot-hub-create-hub.md)]

## <a name="register-an-iot-edge-device"></a>Registrar um dispositivo IoT Edge

Registre um dispositivo IoT Edge no Hub IoT recém-criado.
![Registrar um dispositivo][4]

[!INCLUDE [iot-edge-register-device](../../includes/iot-edge-register-device.md)]


## <a name="install-and-start-the-iot-edge-runtime"></a>Instalar e iniciar o tempo de execução do IoT Edge

Instale e inicie o tempo de execução do Azure IoT Edge no dispositivo. 
![Registrar um dispositivo][5]

O tempo de execução do IoT Edge é implantado em todos os dispositivos IoT Edge. Tem três componentes. O **daemon de segurança do IoT Edge** é iniciado sempre que um dispositivo Edge é iniciado e inicializa o dispositivo inicializando o agente do IoT Edge. O **agente do IoT Edge** facilita a implantação e o monitoramento de módulos no dispositivo IoT Edge, incluindo o hub do IoT Edge. O **hub IoT Edge** gerencia a comunicação entre os módulos no dispositivo IoT Edge e entre o dispositivo e o Hub IoT. 

Siga as etapas a seguir no computador Linux ou VM que você preparou para este início rápido. 

### <a name="register-your-device-to-use-the-software-repository"></a>Registrar o dispositivo para usar o repositório de software

Os pacotes para executar o tempo de execução do IoT Edge são gerenciados em um repositório de software. Configure seu dispositivo IoT Edge para acessar esse repositório. 

As etapas nesta seção são para dispositivos que executam **16.04 Ubuntu**. Para acessar o repositório de software em outras versões do Linux, confira [Instalar o tempo de execução do Azure IoT Edge no Linux (x64)](how-to-install-iot-edge-linux.md) ou [Instalar o tempo de execução do Azure IoT Edge no Linux (ARM32v7/armhf)](how-to-install-iot-edge-linux-arm.md).

1. No computador que você está usando como um dispositivo IoT Edge, instale a configuração do repositório.

   ```bash
   curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > ./microsoft-prod.list
   sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/
   ```

2. Instale uma chave pública para acessar o repositório.

   ```bash
   curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
   sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
   ```

### <a name="install-a-container-runtime"></a>Instale um tempo de execução do contêiner

O tempo de execução do IoT Edge é um conjunto de contêineres, e a lógica que você implanta em seu dispositivo IoT Edge é empacotada como contêineres. Prepare o dispositivo para esses componentes instalando um tempo de execução do contêiner.

Atualize **apt-get**.

   ```bash
   sudo apt-get update
   ```

Instale o **Moby**, um tempo de execução de contêiner.

   ```bash
   sudo apt-get install moby-engine
   ```

Instale os comandos da CLI para Moby. 

   ```bash
   sudo apt-get install moby-cli
   ```

### <a name="install-and-configure-the-iot-edge-security-daemon"></a>Instalar e configurar o daemon de segurança do IoT Edge

O daemon de segurança é instalado como um serviço do sistema para que o tempo de execução do IoT Edge seja iniciado sempre que o dispositivo é inicializado. A instalação também inclui uma versão de **hsmlib** que permite que o daemon de segurança interaja com a segurança de hardware do dispositivo. 

1. Baixe e instale o Daemon de Segurança do IoT Edge. 

   ```bash
   sudo apt-get update
   sudo apt-get install iotedge
   ```

2. Abra o arquivo de configuração do IoT Edge. Ele é um arquivo protegido; talvez seja necessário usar privilégios elevados para acessá-lo.
   
   ```bash
   sudo nano /etc/iotedge/config.yaml
   ```

3. Adicione a cadeia de conexão do dispositivo do IoT Edge. Procure a variável **device_connection_string** e atualize seu valor com a cadeia de caracteres que você copiou após registrar seu dispositivo.

4. Salve e feche o arquivo. 

   `CTRL + X`, `Y`, `Enter`

4. Reinicie o daemon de segurança do IoT Edge.

   ```bash
   sudo systemctl restart iotedge
   ```

5. Verifique se o daemon de segurança do Edge está em execução como um serviço do sistema.

   ```bash
   sudo systemctl status iotedge
   ```

   ![Veja o Daemon do Edge em execução como um serviço do sistema](./media/quickstart-linux/iotedged-running.png)

   Você também pode ver os logs do Daemon de Segurança do Edge executando o seguinte comando:

   ```bash
   journalctl -u iotedge
   ```

6. Visualizar os módulos em execução no seu dispositivo. 

   >[!TIP]
   >Você precisa usar *sudo* para executar comandos `iotedge` inicialmente. Saia do seu computador e entre novamente para atualizar as permissões, em seguida execute os comandos `iotedge` sem privilégios elevados. 

   ```bash
   sudo iotedge list
   ```

   ![Exibir um módulo no dispositivo](./media/quickstart-linux/iotedge-list-1.png)

## <a name="deploy-a-module"></a>Implantar um módulo

Gerencie o dispositivo Azure IoT Edge na nuvem para implantar um módulo que enviará dados telemétricos ao Hub IoT.
![Registrar um dispositivo][6]

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]


## <a name="view-generated-data"></a>Exibir os dados gerados

Neste guia de início rápido, você criou um novo dispositivo IoT Edge e instalou o tempo de execução de IoT Edge nele. Em seguida, você usou o Portal do Azure para enviar por push um módulo do IoT Edge para ser executado no dispositivo sem precisar fazer alterações no próprio dispositivo. Nesse caso, o módulo enviado por push cria dados de ambiente que podem ser usados para os tutoriais. 

Abra o prompt de comando no computador executando o seu dispositivo simulado novamente. Confirme se o módulo implantado da nuvem está em execução no seu dispositivo IoT Edge:

   ```bash
   sudo iotedge list
   ```

   ![Exibir três módulos no seu dispositivo](./media/quickstart-linux/iotedge-list-2.png)

Exiba as mensagens que estão sendo enviadas do módulo tempSensor:

  ```bash
   sudo iotedge logs tempSensor -f 
   ```

Após fazer logoff e logon, *sudo* não é necessário para o comando acima.

![Exibir os dados do seu módulo](./media/quickstart-linux/iotedge-logs.png)

O módulo de sensor de temperatura poderá estar aguardando para se conectar ao Hub do Edge se a última linha que você vê no log for `Using transport Mqtt_Tcp_Only`. Tente eliminar o módulo e deixar que o agente do Edge o reinicie. Você pode encerrá-la com o comando `sudo docker stop tempSensor`.

Também é possível exibir a telemetria que o dispositivo está enviando, usando a [Extensão do Kit de Ferramentas do Azure IoT para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit). 

## <a name="clean-up-resources"></a>Limpar recursos

Se você deseja prosseguir para os tutoriais do IoT Edge, pode usar o dispositivo registrado e configurado neste guia de início rápido. Caso contrário, é possível excluir os recursos do Azure que você criou e remover o tempo de execução do IoT Edge do seu dispositivo. 

### <a name="delete-azure-resources"></a>Excluir recursos do Azure

Se você tiver criado a sua máquina virtual e o Hub IoT em um novo grupo de recursos, é possível excluir esse grupo e todos os recursos associados. Se houver alguma coisa dentro desse grupo de recursos que você deseje manter, então exclua somente os recursos específicos que você deseja apagar. 

Para remover um grupo de recursos, siga as etapas a seguir: 

1. Entre no [portal do Azure](https://portal.azure.com) e clique em **Grupos de recursos**.
2. Na caixa de texto **Filtrar por nome...**, digite o nome do grupo de recursos que contém seu Hub IoT. 
3. À direita do seu grupo de recursos, na lista de resultados, clique em **...**, depois em **Excluir grupo de recursos**.
4. Você receberá uma solicitação para confirmar a exclusão do grupo de recursos. Digite o nome do grupo de recursos novamente para confirmar e clique em **Excluir**. Após alguns instantes, o grupo de recursos, e todos os recursos contidos nele, serão excluídos.

### <a name="remove-the-iot-edge-runtime"></a>Remover o tempo de execução do IoT Edge

Se você deseja remover as instalações do dispositivo, use os comandos a seguir.  

Remova o tempo de execução do IoT Edge.

   ```bash
   sudo apt-get remove --purge iotedge
   ```

Quando o tempo de execução do IoT Edge for removido, os contêineres criados por ele são interrompidos, mas ainda existem no seu dispositivo. Visualizar todos os contêineres.

   ```bash
   sudo docker ps -a
   ```

Exclua os contêineres que foram criados no seu dispositivo pelo tempo de execução do IoT Edge. Altere o nome do contêiner tempSensor se você deu um outro nome para ele. 

   ```bash
   sudo docker rm -f tempSensor
   sudo docker rm -f edgeHub
   sudo docker rm -f edgeAgent
   ```

Remova o tempo de execução do contêiner.

   ```bash
   sudo apt-get remove --purge moby
   ```

## <a name="next-steps"></a>Próximas etapas

Neste início rápido, você criou um novo dispositivo IoT Edge e usou a interface de nuvem do Azure IoT Edge para implantar código no dispositivo. Agora, você tem um dispositivo simulado que gera dados brutos sobre seu ambiente. 

Este guia de início rápido é pré-requisito para todos os tutoriais do IoT Edge. Prossiga para outro tutorial a fim de descobrir como o Azure IoT Edge pode ajudá-lo a transformar esses dados em informações de negócios na borda.

> [!div class="nextstepaction"]
> [Filtrar dados de sensor usando uma função do Azure](tutorial-deploy-function.md)


<!-- Images -->
[1]: ./media/quickstart-linux/view-module.png
[2]: ./media/quickstart-linux/install-edge-full.png
[3]: ./media/quickstart-linux/create-iot-hub.png
[4]: ./media/quickstart-linux/register-device.png
[5]: ./media/quickstart-linux/start-runtime.png
[6]: ./media/quickstart-linux/deploy-module.png
[7]: ./media/quickstart-linux/iotedged-running.png
[8]: ./media/tutorial-simulate-device-linux/running-modules.png
[9]: ./media/tutorial-simulate-device-linux/sensor-data.png

<!-- Links -->
[lnk-account]: https://azure.microsoft.com/free
[lnk-docker-ubuntu]: https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/ 
