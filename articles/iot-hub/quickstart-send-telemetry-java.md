---
title: 'Início Rápido: Enviar telemetria ao Hub IoT do Azure (Java) | Microsoft Docs'
description: Neste início rápido, você executa dois aplicativos Java de exemplo para enviar telemetria simulada a um hub IoT e ler a telemetria do hub IoT para processamento na nuvem.
services: iot-hub
author: dominicbetts
manager: timlt
editor: ''
ms.service: iot-hub
ms.devlang: java
ms.topic: quickstart
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: ns
ms.date: 04/30/2018
ms.author: dobett
ms.openlocfilehash: d887c690a5f0bc8120daa74d6076083634da08f6
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/07/2018
---
# <a name="quickstart-send-telemetry-from-a-device-to-an-iot-hub-and-read-the-telemetry-from-the-hub-with-a-back-end-application-java"></a>Início Rápido: Enviar telemetria de um dispositivo para um Hub IoT, e ler a telemetria do hub com um aplicativo de back-end (Java)

[!INCLUDE [iot-hub-quickstarts-1-selector](../../includes/iot-hub-quickstarts-1-selector.md)]

O Hub IoT é um serviço do Azure que permite a ingestão de grandes volumes de telemetria de seus dispositivos IoT pela nuvem para processamento ou armazenamento. Neste início rápido, você envia telemetria de um aplicativo de dispositivo simulado, por meio do Hub IoT, para um aplicativo de back-end para processamento.

O início rápido usa dois aplicativos Java previamente escritos, um para enviar a telemetria e outro para ler a telemetria do hub. Antes de executar esses dois aplicativos, você cria um hub IoT e registra um dispositivo com o hub.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="prerequisites"></a>pré-requisitos

Os dois exemplos de aplicativo executados neste início rápido são escritos usando o Java. Você precisa do Java SE 8 ou posterior em seu computador de desenvolvimento.

Você pode fazer o download do Java para várias plataformas a partir do [Oracle](http://www.oracle.com/technetwork/java/javase/downloads/index.html).

Verifique a versão atual do Java no computador de desenvolvimento usando o seguinte comando:

```cmd/sh
java --version
```

Para criar os exemplos, você precisa instalar o Maven 3. Você pode fazer o download do Maven para várias plataformas a partir do [Apache Maven](https://maven.apache.org/download.cgi).

Você pode verificar a versão atual do Maven no computador de desenvolvimento usando o seguinte comando:

```cmd/sh
mvn --version
```

Faça o download do projeto de exemplo em Java do https://github.com/Azure-Samples/azure-iot-samples-java/archive/master.zip e extraia o arquivo ZIP.

## <a name="create-an-iot-hub"></a>Crie um hub IoT

[!INCLUDE [iot-hub-quickstarts-create-hub](../../includes/iot-hub-quickstarts-create-hub.md)]

## <a name="register-a-device"></a>Registrar um dispositivo

Um dispositivo deve ser registrado no hub IoT antes de poder se conectar. Neste início rápido, você usa a CLI do Azure para registrar um dispositivo simulado.

1. Adicione a extensão CLI do Hub IoT e crie a identidade do dispositivo. Substitua `{YourIoTHubName}` pelo nome escolhido para o seu hub IoT:

    ```azurecli-interactive
    az extension add --name azure-cli-iot-ext
    az iot hub device-identity create --hub-name {YourIoTHubName}--device-id MyJavaDevice
    ```

1. Execute o seguinte comando para obter a _cadeia de conexão de dispositivo_ do dispositivo que você acabou de registrar:

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name {YourIoTHubName} --device-id MyJavaDevice --output table
    ```

    Anote a cadeia de conexão do dispositivo, que se parece com `Hostname=...=`. Você usará esse valor posteriormente no início rápido.

1. Você também precisa do _ponto de extremidade compatível com Hubs de Evento_, _caminho compatível dos Hubs de Evento_ e _Chave primária iothubowner_ de seu hub IoT para ativar o aplicativo back-end a fim de se conectar ao seu hub IoT e recuperar as mensagens. Os comandos a seguir recuperam esses valores para o seu hub IoT:

    ```azurecli-interactive
    az iot hub show --query properties.eventHubEndpoints.events.endpoint --name {YourIoTHubName}

    az iot hub show --query properties.eventHubEndpoints.events.path --name {YourIoTHubName}

    az iot hub policy show --name iothubowner --query primaryKey --hub-name {your IoT Hub name}
    ```

    Anote esses três valores, que você usará posteriormente no início rápido.

## <a name="send-simulated-telemetry"></a>Enviar telemetria simulada

O aplicativo de dispositivo simulado se conecta a um ponto de extremidade específico do dispositivo em seu hub IoT e envia telemetria simulada de temperatura e umidade.

1. Em uma janela de terminal, navegue até a pasta raiz do projeto Java de exemplo. Em seguida, navegue até a pasta **Quickstarts\simulated-device**.

1. Abra o arquivo **src/main/java/com/microsoft/docs/iothub/samples/SimulatedDevice.java** em um editor de texto de sua escolha.

    Substitua o valor da variável `connString` pela cadeia de conexão do dispositivo que você anotou anteriormente. Salve as alterações no arquivo **SimulatedDevice.java**.

1. Na janela de terminal, execute os seguintes comandos para instalar as bibliotecas necessárias e compilar o aplicativo de dispositivo simulado:

    ```cmd/sh
    mvn clean package
    ```

1. Na janela de terminal, execute os seguintes comandos para executar o aplicativo de dispositivo simulado:

    ```cmd/sh
    java -jar target/simulated-device-1.0.0-with-deps.jar
    ```

    A captura de tela a seguir mostra a saída à medida que o aplicativo de dispositivo simulado envia telemetria para o seu hub IoT:

    ![Executar o dispositivo simulado](media/quickstart-send-telemetry-java/SimulatedDevice.png)

## <a name="read-the-telemetry-from-your-hub"></a>Ler a telemetria do hub

O aplicativo de back-end se conecta ao ponto de extremidade **Eventos** do lado do serviço em seu Hub IoT. O aplicativo recebe as mensagens do dispositivo para a nuvem, enviadas de seu dispositivo simulado. Um aplicativo de back-end do Hub IoT normalmente é executado na nuvem para receber e processar as mensagens do dispositivo para a nuvem.

1. Em outra janela de terminal, navegue até a pasta raiz do projeto Java de exemplo. Em seguida, navegue até a pasta **Quickstarts\read-d2c-messages**.

1. Abra o arquivo **src/main/java/com/microsoft/docs/iothub/samples/ReadDeviceToCloudMessages.java** em um editor de texto de sua escolha.

    Substitua o valor da variável `eventHubsCompatibleEndpoint` pelo ponto de extremidade compatível com os Hubs de Evento que você anotou anteriormente.

    Substitua o valor da variável `eventHubsCompatiblePath` pelo caminho compatível com os Hubs de Eventos que você anotou anteriormente.

    Substitua o valor da variável `iotHubSasKey` pela chave primária iothubowner que você anotou anteriormente. Depois, salve suas alterações no arquivo **ReadDeviceToCloudMessages.java**.

1. Na janela de terminal, execute os seguintes comandos para instalar as bibliotecas necessárias e compilar o aplicativo de back-end:

    ```cmd/sh
    mvn clean package
    ```

1. Na janela do terminal, execute os comandos a seguir para executar o aplicativo back-end:

    ```cmd/sh
    java -jar target/read-d2c-messages-1.0.0-with-deps.jar
    ```

    A captura de tela a seguir mostra a saída à medida que o aplicativo de back-end recebe a telemetria enviada pelo dispositivo simulado para o hub:

    ![Executar o aplicativo de back-end](media/quickstart-send-telemetry-java/ReadDeviceToCloud.png)

## <a name="clean-up-resources"></a>Limpar recursos

Se você planeja concluir o próximo início rápido, mantenha o grupo de recursos e o hub IoT e reutilize-os posteriormente.

Se você não precisar mais do hub IoT, exclua-o junto com o grupo de recursos no portal. Para isso, selecione o grupo de recursos **qs-iot-hub-rg** que contém o Hub IoT e clique em **Excluir**.

## <a name="next-steps"></a>Próximas etapas

Neste início rápido, você configurou um hub IoT, registrou um dispositivo, enviou telemetria simulada para o hub usando um aplicativo Java e leu a telemetria do hub usando um aplicativo de back-end simples.

Para saber como controlar o dispositivo simulado em um aplicativo de back-end, continue no próximo início rápido.

> [!div class="nextstepaction"]
> [Início Rápido: Controlar um dispositivo conectado a um hub IoT](quickstart-control-device-java.md)