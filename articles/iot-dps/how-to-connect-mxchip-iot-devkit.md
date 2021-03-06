---
title: Como usar o provisionamento automático do Serviço de Provisionamento de Dispositivos no Hub IoT do Azure para registrar o MXChip IoT DevKit com o Hub IoT | Microsoft Docs
description: Como usar o provisionamento automático do Serviço de Provisionamento de Dispositivos no Hub IoT do Azure para registrar o MXChip IoT DevKit com o Hub IoT.
author: liydu
ms.author: liydu
ms.date: 04/04/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: jeffya
ms.openlocfilehash: d8912a5da8c4df2069d8bc53454748b5fb3d5c39
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/20/2018
ms.locfileid: "42145471"
---
# <a name="use-azure-iot-hub-device-provisioning-service-auto-provisioning-to-register-the-mxchip-iot-devkit-with-iot-hub"></a>Use o provisionamento automático do Serviço de Provisionamento de Dispositivos no Hub IoT do Azure para registrar o MXChip IoT DevKit com o Hub IoT

Este artigo descreve como usar o [provisionamento automático](concepts-auto-provisioning.md) do Serviço de Provisionamento de Dispositivos no Hub IoT do Azure para registrar o MXChip IoT DevKit com o Hub IoT do Azure. Neste tutorial, você aprenderá como:

* Configure o terminal global do serviço de provisionamento de dispositivos em um dispositivo.
* Use UDS (Unique Device Secret) para gerar um certificado X.509.
* Inscreva um dispositivo individual.
* Verifique se o dispositivo está registrado.

O [ Kit de Desenvolvimento da IoT para MXChip](https://aka.ms/iot-devkit) é uma placa compatível com Arduino all-in-one periféricos e sensores avançados. É possível desenvolvê-lo usando a [extensão do Visual Studio Code para Arduino](https://aka.ms/arduino). O Kit de Desenvolvimento vem com um [catálogo de projetos](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/) crescente para guiá-lo no protótipo de soluções da IoT (Internet das Coisas) que aproveitam os serviços do Azure.

## <a name="before-you-begin"></a>Antes de começar

Para concluir as etapas neste tutorial, faça primeiro as tarefas a seguir:

* Prepare o seu Kit de Desenvolvimento, seguindo as etapas em [Conectar Kit de Desenvolvimento da IoT AZ3166 ao Hub IoT do Azure na nuvem](/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started).
* Atualize para o firmware mais recente (1.3.0 ou posterior) com o tutorial [Atualização do firmware do Kit de Desenvolvimento](https://microsoft.github.io/azure-iot-developer-kit/docs/firmware-upgrading/).
* Crie e vincule um Hub IoT a uma instância do serviço de provisionamento de dispositivo seguindo as etapas em [Configurar o serviço de provisionamento de dispositivo do Hub IoT com o portal do Azure](/azure/iot-dps/quick-setup-auto-provision).

## <a name="build-and-deploy-auto-provisioning-registration-software-to-the-device"></a>Compilar e implantar o software de registro de provisionamento automático para o dispositivo

Para conectar o DevKit à instância do serviço de Provisionamento de Dispositivos que você criou:

1. No portal do Azure, selecione a **visão geral** painel de seu serviço de provisionamento de dispositivos e anote o **ponto de extremidade do dispositivo Global** e **escopo da ID** valores.
  ![Endpoint global e escopo de ID do serviço de provisionamento de dispositivo](./media/how-to-connect-mxchip-iot-devkit/dps-global-endpoint.png)

2. Verifique se você `git`instalou em seu computador e se foi adicionado às variáveis de ambiente que podem ser acessadas pela janela de comando. Consulte [ferramentas de cliente de Software Freedom Conservancy's Git ](https://git-scm.com/download/) para ter a versão mais recente instalada.

3. Abra um prompt de comando. Clone o repositório GitHub para o código de amostra do serviço de provisionamento de dispositivos:
  ```bash
  git clone https://github.com/DevKitExamples/DevKitDPS.git
  ```

4. Abra o Visual Studio Code, conecte o DevKit ao computador e abra a pasta contendo o código clonado.

5. Abra **DevKitDPS.ino**. Localize e substitua `[Global Device Endpoint]` e `[ID Scope]` pelos valores anotados.
  ![Ponto de extremidade do serviço de provisionamento de dispositivos](./media/how-to-connect-mxchip-iot-devkit/endpoint.png) Você pode deixar o **registrationId** em branco. O aplicativo irá gerar um com base no endereço MAC e na versão do firmware. Se você quiser personalizá-la, a ID de Registro deve usar combinações alfanuméricas, minúsculas e hifens com no máximo 128 caracteres. Para obter mais informações, consulte [Gerenciar registros de dispositivos com o Portal do Azure](https://docs.microsoft.com/azure/iot-dps/how-to-manage-enrollments).

6. Use a Abertura Rápida no VS Code (Windows: `Ctrl+P`, macOS: `Cmd+P`) e insira *task device-upload* para compilar e carregar o código para o Kit de Desenvolvimento.

7. A janela de saída mostra se a tarefa foi bem-sucedida.

## <a name="save-a-unique-device-secret-on-an-stsafe-security-chip"></a>Salvar o unique sevice secret no circuito integrado de segurança STSAFE

O provisionamento automático pode ser configurado em um dispositivo baseado no [mecanismo de atestado](concepts-security.md#attestation-mechanism) do dispositivo. O Kit de Desenvolvimento da IoT para MXChip usa [Device Identity Composition Engine](https://trustedcomputinggroup.org/wp-content/uploads/Foundational-Trust-for-IOT-and-Resource-Constrained-Devices.pdf) do [ TCG (Trusted Computing Group)](https://trustedcomputinggroup.org). Um *unique device secret* (UDS) salvo no circuito integrado de segurança STSAFE no DevKit é utilizado para gerar o [certificado X.509](concepts-security.md#x509-certificates) exclusivo do dispositivo. O certificado é usado posteriormente para o processo de inscrição no serviço de provisionamento de dispositivos e durante o registro em tempo de execução.

Um unique device secret típico é uma cadeia de 64 caracteres como mostrado no exemplo a seguir:

```
19e25a259d0c2be03a02d416c05c48ccd0cc7d1743458aae1cb488b074993eae
```

A cadeia de caracteres é dividida em pares de caracteres que são usados no cálculo de segurança. O exemplo UDS acima é resolvido para: `0x19`, `0xe2`, `0x5a`, `0x25`, `0x9d`, `0x0c`, `0x2b`, `0xe0`, `0x3a`, `0x02`, `0xd4`, `0x16`, `0xc0`, `0x5c`, `0x48`, `0xcc`, `0xd0`, `0xcc`, `0x7d`, `0x17`, `0x43`, `0x45`, `0x8a`, `0xae`, `0x1c`, `0xb4`, `0x88`, `0xb0`, `0x74`, `0x99`, `0x3e`, `0xae`.

Para salvar o unique device secret no Kit de Desenvolvimento:

1. Abra o monitor serial usando uma ferramenta como o Putty. Consulte [Usar o modo de configuração](https://microsoft.github.io/azure-iot-developer-kit/docs/use-configuration-mode/) para obter detalhes.

2. Com o Kit de Desenvolvimento conectado ao computador, mantenha pressionado o botão **A** e, em seguida, pressione e solte o botão de **Reinicialização** para entrar no modo de configuração. A tela exibe a ID do Kit de Desenvolvimento e Configuração.

3. Na cadeia de caracteres do UDS de exemplo e altere um ou mais caracteres para outros valores entre `0` e `f` para seu próprio UDS.

4. Na janela do monitor serial, digite *set_dps_uds [your_own_uds_value]* e selecione Enter.
  > [!NOTE]
  > Por exemplo, se você definir seu próprio UDS alterando os últimos dois caracteres para `f`, será preciso inserir um comando como este: `set_dps_uds 19e25a259d0c2be03a02d416c05c48ccd0cc7d1743458aae1cb488b074993eff`.

5. Sem fechar a janela do monitor serial, pressione o botão **de reinicialização** no Kit de Desenvolvimento.

6. Anote o valor do **Endereço do Kit de Desenvolvimento MAC** e os valores **Versão do Firmware do Kit de Desenvolvimento**.
  ![Versão do Firmware](./media/how-to-connect-mxchip-iot-devkit/firmware-version.png)

## <a name="generate-an-x509-certificate"></a>Gerar um certificado X.509

Agora você precisa gerar um certificado X.609. 

### <a name="windows"></a>Windows

1. Abra o File Explorer e vá para a pasta que contém o código de exemplo do Serviço de Aprovisionamento de Dispositivos que você clonou anteriormente. Na pasta **.build**, localize e copie **DPS.ino.bin** e **DPS.ino.map**.
  ![Arquivos gerados](./media/how-to-connect-mxchip-iot-devkit/generated-files.png)
  > [!NOTE]
  > Se você tiver alterado a `built.path` configuração do Arduino para outra pasta, será necessário localizar esses arquivos na pasta configurada.

2. Cole esses dois arquivos na pasta **ferramentas** no mesmo nível com a pasta **.build**.

3. Execute **dps_cert_gen.exe**. Siga os prompts para inserir o seu **UDS**, **endereço MAC** para o Kit de Desenvolvimento e a **versão do firmware** para gerar o certificado X.509.
  ![Executar dps-cert-gen.exe](./media/how-to-connect-mxchip-iot-devkit/dps-cert-gen.png)

4. Depois que o certificado x. 509 for gerado, um certificado **.pem** é salvo na mesma pasta.

## <a name="create-a-device-enrollment-entry-in-the-device-provisioning-service"></a>Criar uma entrada de registro do dispositivo no serviço de provisionamento de dispositivos

1. No portal do Azure, acesse a instância do serviço de provisionamento de dispositivo. Selecione **Gerenciar registros**, em seguida, selecione a guia **Registros individuais**. ![Registros individuais](./media/how-to-connect-mxchip-iot-devkit/individual-enrollments.png)

2. Selecione **Adicionar**.

3. No painel "Adicionar inscrição":

   - Selecione **X.509** sob **mecanismo**.
   - Clique em "Selecionar um arquivo" em **Arquivo .pem ou .cer do certificado primário**.
   - Na caixa de diálogo Abrir arquivo, navegue até e carregue o **.pem** certificado que você acabou de ser gerado.
   - Deixe o restante como padrão e clique em **Salvar**.

   ![Carregar um certificado](./media/how-to-connect-mxchip-iot-devkit/upload-cert.png)

  > [!NOTE]
  > Se houver um erro com esta mensagem:
  >
  > `{"message":"BadRequest:{\r\n \"errorCode\": 400004,\r\n \"trackingId\": \"1b82d826-ccb4-4e54-91d3-0b25daee8974\",\r\n \"message\": \"The certificate is not a valid base64 string value\",\r\n \"timestampUtc\": \"2018-05-09T13:52:42.7122256Z\"\r\n}"}`
  >
  > Abra o arquivo de certificado **.pem** como texto (abra com o Bloco de Notas ou qualquer editor de texto) e exclua as linhas:
  >
  > `"-----BEGIN CERTIFICATE-----"` e `"-----END CERTIFICATE-----"`.
  >

## <a name="start-the-devkit"></a>Iniciar o DevKit

1. Abra o VS Code e abra o monitor serial.

2. Pressione o botão de **Reinicialização** no seu DevKit.

Você vê o DevKit iniciar o registro com o serviço de provisionamento de dispositivos.

![Saída do VS Code](./media/how-to-connect-mxchip-iot-devkit/vscode-output.png)

## <a name="verify-that-the-devkit-is-registered-with-azure-iot-hub"></a>Verifique se o Kit de Desenvolvimento está registrado com o Hub IoT do Azure

Quando o dispositivo inicializar, as ações a seguir ocorrerão:

1. O dispositivo envia uma solicitação de registro para o Serviço de Provisionamento de Dispositivos.
2. O serviço de provisionamento de dispositivos envia de volta um desafio de registro ao qual seu dispositivo responde.
3. No registro bem-sucedido, o serviço de provisionamento de dispositivos envia o URI do Hub da Internet das coisas, o ID do dispositivo e a chave criptografada de volta ao dispositivo.
4. O aplicativo cliente do Hub IoT no dispositivo se conecta ao seu hub.
5. Após a conexão bem-sucedida ao hub, você verá o dispositivo aparecer no Device Explorer do Hub IoT.
  ![Dispositivo registrado](./media/how-to-connect-mxchip-iot-devkit/device-registered.png)

## <a name="problems-and-feedback"></a>Comentários e problemas

Se você encontrar problemas, consulte as [Perguntas frequentes](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) do Kit de Desenvolvimento ou entre em contato com conosco pelos canais abaixo para obter suporte:

* [Gitter.im](http://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stackoverflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você aprendeu a registrar um dispositivo com segurança no Serviço de Aprovisionamento de Dispositivos usando o Mecanismo de Composição de Identidade do Dispositivo, para que o dispositivo possa se registrar automaticamente no Azure Hub do IoT. 

Em resumo, você aprendeu a:

> [!div class="checklist"]
> * Configure o terminal global do serviço de provisionamento de dispositivos em um dispositivo.
> * Usar um unique device secret para gerar um certificado X.509.
> * Inscreva um dispositivo individual.
> * Verifique se o dispositivo está registrado.

Saiba como [Criar e provisionar um dispositivo simulado](./quick-create-simulated-device.md).

