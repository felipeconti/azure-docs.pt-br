---
title: Criar um Hub IoT usando Kit de Ferramentas do Azure IoT para VS Code | Microsoft Docs
description: Como usar Kit de Ferramentas do Azure IoT para VS Code para criar um Hub IoT.
author: formulahendry
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/30/2018
ms.author: junhan
ms.openlocfilehash: af31f9375d6a41e13a9122e9730ba9532d3d52c5
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/09/2018
ms.locfileid: "40003062"
---
# <a name="create-an-iot-hub-using-the-azure-iot-toolkit-for-visual-studio-code"></a>Criar um Hub IoT usando o Kit de Ferramentas do Azure IoT para Visual Studio Code

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

É possível usar o [Kit de Ferramentas do Azure IoT para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) para criar Hubs IoT. Este artigo mostra como criar um Hub IoT com Kit de Ferramentas do Azure IoT.

Para concluir este artigo, você precisa do seguinte:

* Uma conta ativa do Azure.
- [Visual Studio Code](https://code.visualstudio.com/)
- [Kit de Ferramentas do Azure IoT](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit)

## <a name="create-an-iot-hub"></a>Crie um hub IoT

1. No Visual Studio Code, abra a exibição do **Explorer**.

2. Na parte inferior do Explorer, expanda a seção **Dispositivos do Hub IoT**. 

   ![Expanda Dispositivos do Azure Hub IoT](./media/iot-hub-create-use-iot-toolkit/azure-iot-hub-devices.png)

3. Clique em **...** no cabeçalho da seção **Dispositivos do Hub IoT**. Se você não visualizar as reticências, passe o mouse sobre o cabeçalho. 

4. Escolha **Criar Hub IoT**.

5. Um pop-up aparecerá no canto inferior direito para permitir que você entre no Azure pela primeira vez.

6. Selecione a assinatura do Azure. 

7. Selecione o grupo de recursos.

8. Selecione o local.

9. Selecione o tipo de preço.

10. Insira um nome exclusivo globalmente para o Hub IoT.

11. Aguarde alguns minutos até que o Hub IoT seja criado.

## <a name="next-steps"></a>Próximas etapas

Agora que você implantou um Hub IoT usando Kit de Ferramentas do Azure IoT para Visual Studio Code, explore ainda mais:

* [Use a extensão do Kit de Ferramentas do Azure IoT para Visual Studio Code para enviar e receber mensagens entre o dispositivo e o Hub IoT](iot-hub-vscode-iot-toolkit-cloud-device-messaging.md).
* [Use a extensão do Kit de Ferramentas do Azure IoT para Visual Studio Code para o gerenciamento do dispositivo de Hub IoT do Azure](iot-hub-device-management-iot-toolkit.md)
* [Página Wiki](https://github.com/microsoft/vscode-azure-iot-toolkit/wiki) para Kit de Ferramentas do Azure IoT.
