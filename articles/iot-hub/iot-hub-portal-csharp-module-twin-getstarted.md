---
title: Introdução à identidade do módulo e ao módulo gêmeo do Hub IoT do Azure (portal e .NET) | Microsoft Docs
description: Saiba como criar a identidade do módulo e atualizar o módulo gêmeo usando o portal e .NET.
services: iot-hub
documentationcenter: .net
author: chrissie926
manager: timlt
editor: ''
ms.assetid: f40604ff-8fd6-4969-9e99-8574fbcf036c
ms.service: iot-hub
ms.devlang: dotnet
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/26/2018
ms.author: dobett
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 30a53620a640b0122286a6ac69c0f98cd0bbde40
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34361681"
---
# <a name="get-started-with-iot-hub-module-identity-and-module-twin-using-the-portal-and-net-device"></a>Introdução à identidade do módulo e ao módulo gêmeo do Hub IoT usando o portal e o dispositivo .NET

> [!NOTE]
> [As identidades do módulo e os módulos gêmeos](iot-hub-devguide-module-twins.md) são semelhantes à identidade do dispositivo e ao dispositivo gêmeo do Hub IoT do Azure, mas fornecem melhor granularidade. Enquanto a identidade do dispositivo e o dispositivo gêmeo do Hub IoT do Azure permitem que o aplicativo de back-end configure um dispositivo e forneça visibilidade às condições do dispositivo, uma identidade de módulo e módulo gêmeo fornecem esses recursos para componentes individuais de um dispositivo. Em dispositivos habilitados com vários componentes, como dispositivos baseados em sistema operacional ou dispositivos de firmware, permite condições e configuração isolada para cada componente.

Neste tutorial, você aprenderá a 
1. como criar uma identidade de módulo no portal. 
2. como usar o SDK do dispositivo .NET para atualizar o módulo gêmeo a partir do seu dispositivo.

> [!NOTE]
> Para obter informações sobre os SDKs de IoT do Azure que você pode usar para criar aplicativos executados em dispositivos e no back-end da solução, consulte [Azure IoT SDKs][lnk-hub-sdks].

Para concluir este tutorial, você precisará do seguinte:

* Visual Studio 2015 ou Visual Studio 2017.
* Uma conta ativa do Azure. (Se você não possui uma conta, é possível criar uma [conta gratuita] [lnk-free-trial] em apenas alguns minutos.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

## <a name="create-a-device-identity-in-the-portal"></a>Criar uma identidade de dispositivo no portal

Agora você tem um Hub IoT. Abra o [portal](https://portal.azure.com) e navegue até o Hub IoT. Clique nos Dispositivos IoT e depois clique em adicionar para criar uma identidade de dispositivo. Nomeie-o **MyFirstDevice**. 

![Create device identity][8]

Depois de salvar, na sua lista de identidades de dispositivos, você verá que a identidade MyFirstDevice foi criada com êxito.

![Device id created][11]

Agora, clique na linha. Você verá os detalhes do dispositivo.

![Device details][10]

## <a name="create-a-module-identity-in-the-portal"></a>Criar uma identidade de módulo no portal

Em uma identidade de dispositivo, você pode criar até 20 identidades de módulo. Clique no botão **Adicionar Identidade de Módulo** na parte superior para criar sua primeira identidade de módulo chamada **myFirstModule**. 

![Device details][9]

Salve e clique na identidade do módulo que você acabou de criar. Você pode ver os detalhes de identidade do módulo. Salve a cadeia de conexão - chave primária. Ele será usado na próxima seção, na qual você configurará seu módulo no dispositivo.

![Device details][12]

<a id="D2C_csharp"></a>
## <a name="update-the-module-twin-using-net-device-sdk"></a>Atualizar o módulo gêmeo usando o SDK do dispositivo .NET

Você criou com êxito a identidade do módulo em seu Hub IoT. Vamos tentar se comunicar com a nuvem pelo dispositivo simulado. Depois de criar uma identidade de módulo, um módulo gêmeo é implicitamente criado no Hub IoT. Nesta seção, você criará um aplicativo de console .NET em seu dispositivo simulado que atualiza as propriedades relatadas do módulo gêmeo.

1. **Criar um projeto do Visual Studio** - No Visual Studio, adicione um projeto da Área de Trabalho Clássica do Windows no Visual C# à solução existente usando o modelo de projeto **Aplicativo do Console (.NET Framework)**. Verifique se a versão do .NET Framework é 4.6.1 ou posterior. Nomeie o projeto **UpdateModuleTwinReportedProperties**.

    ![Create a visual studio project][13]

2. **Instalar SDK V1.16.0-preview-005 do dispositivo .NET do Hub IoT do Azure** - Identidade do módulo e módulo gêmeo estão em versão prévia pública. Está disponível apenas nos SDKs do dispositivo do Hub IoT pré-lançamento. No Visual Studio, abra ferramentas > gerenciamento de pacotes Nuget > gerenciar pacotes Nuget para solução. Pesquise Microsoft.Azure.Devices.Client. Verifique se você marcou a caixa de seleção de incluir pré-lançamento. Selecione a versão V1.16.0-preview-005 e instale. Agora você tem acesso a todos os recursos do módulo. 

    ![Install Azure IoT Hub .NET service SDK V1.16.0-preview-005][14]

3. **Obter a cadeia de conexão do módulo** – agora se fizer logon no [Azure portal][lnk-portal]. Navegue até seu Hub IoT e clique em Dispositivos IoT. Localize myFirstDevice, abra-o e você verá que myFirstModule foi criado com êxito. Copie a cadeia de conexão do módulo. Ela será necessária na próxima etapa.

    ![Azure portal module detail][15]

4. **Criar aplicativo de console UpdateModuleTwinReportedProperties** Adicione as seguintes instruções `using` à parte superior do arquivo **Program.cs**:

    ```csharp
    using Microsoft.Azure.Devices.Client;
    using Microsoft.Azure.Devices.Shared;
    ```

    Adicione os seguintes campos à classe **Program** . Substitua o valor de espaço reservado pela cadeia de conexão do módulo.

    ```csharp
    private const string ModuleConnectionString = "<Your module connection string>“;
    private static ModuleClient Client = null;
    ```

    Adicione o seguinte método **OnDesiredPropertyChanged** à classe de **Programa**:

    ```csharp
    private static async Task OnDesiredPropertyChanged(TwinCollection desiredProperties, object userContext)
        {
            Console.WriteLine("desired property change:");
            Console.WriteLine(JsonConvert.SerializeObject(desiredProperties));
            Console.WriteLine("Sending current time as reported property");
            TwinCollection reportedProperties = new TwinCollection
            {
                ["DateTimeLastDesiredPropertyChangeReceived"] = DateTime.Now
            };

            await Client.UpdateReportedPropertiesAsync(reportedProperties).ConfigureAwait(false);
        }
    ```

    Por fim, adicione as seguintes linhas ao método **Principal** :

    ```csharp
    static void Main(string[] args)
    {
        Microsoft.Azure.Devices.Client.TransportType transport = Microsoft.Azure.Devices.Client.TransportType.Amqp;

        try
        {
            Client = ModuleClient.CreateFromConnectionString(ModuleConnectionString, transport);
            Client.SetConnectionStatusChangesHandler(ConnectionStatusChangeHandler);
            Client.SetDesiredPropertyUpdateCallbackAsync(OnDesiredPropertyChanged, null).Wait();

            Console.WriteLine("Retrieving twin");
            var twinTask = Client.GetTwinAsync();
            twinTask.Wait();
            var twin = twinTask.Result;
            Console.WriteLine(JsonConvert.SerializeObject(twin));

            Console.WriteLine("Sending app start time as reported property");
            TwinCollection reportedProperties = new TwinCollection();
            reportedProperties["DateTimeLastAppLaunch"] = DateTime.Now;

            Client.UpdateReportedPropertiesAsync(reportedProperties);
        }
        catch (AggregateException ex)
        {
            Console.WriteLine("Error in sample: {0}", ex);
        }

        Console.WriteLine("Waiting for Events.  Press enter to exit...");
        Client.CloseAsync().Wait();
    }
    ```

    Este exemplo de código mostra como recuperar o módulo gêmeo e atualizar as propriedades relatadas com o protocolo AMQP. Na versão prévia pública, só há suporte a AMQP para operações de módulo gêmeo.
    ```

## Run the apps

You are now ready to run the apps. In Visual Studio, in Solution Explorer, right-click your solution, and then click **Set StartUp projects**. Select **Multiple startup projects**, and then select **Start** as the action for the console app. And then press F5 to start both apps running. 

## Next steps

To continue getting started with IoT Hub and to explore other IoT scenarios, see:

* [Get started with IoT Hub module identity and module twin using .NET backup and .NET device][lnk-csharp-csharp-getstarted]
* [Getting started with IoT Edge][lnk-iot-edge]


<!-- Images. -->
[8]:./media\iot-hub-portal-csharp-module-twin-getstarted/create-device-id.JPG
[9]:./media\iot-hub-portal-csharp-module-twin-getstarted/create-module-id.JPG
[10]:./media\iot-hub-portal-csharp-module-twin-getstarted/device-details.JPG
[11]:./media\iot-hub-portal-csharp-module-twin-getstarted/device-id-created.JPG
[12]:./media\iot-hub-portal-csharp-module-twin-getstarted/module-details.JPG
[13]: ./media\iot-hub-csharp-csharp-module-twin-getstarted/update-twins-csharp1.JPG
[14]: ./media\iot-hub-csharp-csharp-module-twin-getstarted/install-sdk.png
[15]: ./media\iot-hub-csharp-csharp-module-twin-getstarted/module-detail.JPG
<!-- Links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-csharp-csharp-getstarted]: iot-hub-csharp-csharp-module-twin-getstarted.md
[lnk-iot-edge]: ../iot-edge/tutorial-simulate-device-linux.md
