---
title: Creación de un bot con Bot Builder SDK para .NET | Microsoft Docs
description: Cree un bot con Bot Builder SDK para .NET, un marco de trabajo eficaz para la creación de bots.
keywords: Bot Builder SDK, creación de un bot, inicio rápido, introducción
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: get-started-article
ms.prod: bot-framework
ms.date: 04/27/2018
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: 16ca658dddbba987545b7aa18c1e8f63f13983f1
ms.sourcegitcommit: 97bb24f15041caccef4ca5736aa14f144881e0c6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2018
ms.locfileid: "39567574"
---
# <a name="create-a-bot-with-the-bot-builder-sdk-for-net"></a>Creación de un bot con Bot Builder SDK para .NET

[!INCLUDE [pre-release-label](../includes/pre-release-label-v3.md)]

> [!div class="op_single_selector"]
> - [.NET](../dotnet/bot-builder-dotnet-quickstart.md)
> - [Node.js](../nodejs/bot-builder-nodejs-quickstart.md)
> - [Bot Service](../bot-service-quickstart.md)
> - [REST](../rest-api/bot-framework-rest-connector-quickstart.md)

<a href="https://github.com/Microsoft/BotBuilder" target="_blank">Bot Builder SDK para .NET</a> es un marco fácil de usar para el desarrollo de bots con Visual Studio y Windows. El SDK utiliza C# para proporcionar una manera conocida de crear bots eficaces para los desarrolladores de .NET.


En este tutorial se ofrece información sobre cómo crear un bot con la plantilla de aplicación bot y Bot Builder SDK para .NET, y después se prueba con Bot Framework Emulator.

## <a name="prerequisites"></a>Requisitos previos
1. Visual Studio [2017](https://www.visualstudio.com/).

2. En Visual Studio, actualice todas las [extensiones](https://docs.microsoft.com/en-us/visualstudio/extensibility/how-to-update-a-visual-studio-extension) a sus versiones más recientes.

3. Plantilla de bot para [C#](https://marketplace.visualstudio.com/items?itemName=BotBuilder.BotBuilderV3).

> [!TIP]
> El directorio de plantillas de proyecto de Visual Studio 2017 suele ubicarse en `%USERPROFILE%\Documents\Visual Studio 2017\Templates\ProjectTemplates\Visual C#\` y el directorio de plantillas de elemento se encuentra en `%USERPROFILE%\Documents\Visual Studio 2017\Templates\ItemTemplates\Visual C#\`

## <a name="create-your-bot"></a>Creación del bot

Abra Visual Studio y cree un proyecto de C#. Elija la plantilla **Simple Echo Bot Application** (Aplicación de bot de echo sencilla) para el proyecto nuevo.

![Creación de un proyecto en Visual Studio](../media/connector-getstarted-create-project.png)

> [!NOTE]
> Visual Studio podría indicar que debe descargar e instalar [IIS Express](https://www.microsoft.com/en-us/download/details.aspx?id=48264). 

Gracias a la plantilla de la aplicación Bot, el proyecto contiene todo el código que es necesario para crear el bot en este tutorial. Realmente no necesita escribir ningún código adicional. Sin embargo, antes de pasar a las pruebas de un bot, eche un vistazo rápido a parte del código que proporciona la plantilla de la aplicación Bot.

> [!TIP] 
> Si es necesario, actualice los [paquetes NuGet](https://docs.microsoft.com/en-us/nuget/quickstart/install-and-use-a-package-in-visual-studio).

## <a name="explore-the-code"></a>Exploración del código

En primer lugar, el método `Post` de **Controllers\MessagesController.cs** recibe el mensaje del usuario e invoca el diálogo raíz.

```csharp
public async Task<HttpResponseMessage> Post([FromBody]Activity activity)
{
    if (activity.GetActivityType() == ActivityTypes.Message)
    {
        await Conversation.SendAsync(activity, () => new Dialogs.RootDialog());
    }
    else
    {
        HandleSystemMessage(activity);
    }
    var response = Request.CreateResponse(HttpStatusCode.OK);
    return response;
}

```

El diálogo raíz procesa el mensaje y genera una respuesta. El método `MessageReceivedAsync` de **Dialogs\RootDialog.cs** envía una respuesta que reproduce el mensaje del usuario, con el prefijo "Usted envió" y que termina con el texto "que tenía *##* caracteres", donde *##* representa el número de caracteres del mensaje del usuario.

```csharp
public class RootDialog : IDialog<object>
{
    public Task StartAsync(IDialogContext context)
    {
        context.Wait(MessageReceivedAsync);

        return Task.CompletedTask;
    }

    private async Task MessageReceivedAsync(IDialogContext context, IAwaitable<object> result)
    {
        var activity = await result as Activity;

        // Calculate something for us to return
        int length = (activity.Text ?? string.Empty).Length;

        // Return our reply to the user
        await context.PostAsync($"You sent {activity.Text} which was {length} characters");

        context.Wait(MessageReceivedAsync);
    }
}
```

## <a name="test-your-bot"></a>Prueba del bot

[!INCLUDE [Get started test your bot](../includes/snippet-getstarted-test-bot.md)]

### <a name="start-your-bot"></a>Inicio del bot

Después de instalar el emulador, inicie el bot en Visual Studio mediante un explorador como el host de la aplicación.
Esta captura de pantalla de Visual Studio muestra que, cuando se hace clic en el botón de ejecución, se iniciará el bot en Microsoft Edge.

![Ejecución del proyecto en Visual Studio](../media/connector-getstarted-start-bot-locally.png)

Al hacer clic en el botón de ejecución, Visual Studio compila la aplicación, la implementa en localhost e inicia el explorador web para mostrar la página **default.htm** de la aplicación.
Por ejemplo, esta es la página **default.htm** de la aplicación que se muestra en Microsoft Edge:

![Localhost de ejecución del bot en Visual Studio](../media/connector-getstarted-bot-running-localhost.png)

> [!NOTE]
> Puede modificar el archivo **default.htm** en el proyecto para especificar el nombre y la descripción de la aplicación bot.

### <a name="start-the-emulator-and-connect-your-bot"></a>Inicio del emulador y conexión del bot

En este momento, el bot se ejecuta de forma local.
A continuación, inicie el emulador y, después, conéctese al bot en el emulador:

1. Cree una configuración de bot. Escriba `http://localhost:port-number/api/messages` en la barra de dirección, donde *port-number* coincide con el número de puerto mostrado en el explorador donde se ejecuta la aplicación.

2. Haga clic en **Save and connect** (Guardar y conectar). No tendrá que especificar el **id. de la aplicación de Microsoft** ni la **contraseña de la aplicación de Microsoft**. Por ahora puede dejar estos campos en blanco. Más adelante obtendrá esta información al [registrar el bot](~/bot-service-quickstart-registration.md).

### <a name="test-your-bot"></a>Prueba del bot

Ahora que el bot se ejecuta localmente y está conectado al emulador, escriba algunos mensajes en el emulador para probar el bot.
Debe observar que el bot responde a cada mensaje enviado al reproducir el mensaje con el prefijo "Usted envío" y que termina con el texto "que tenía *##* caracteres", donde *##* es el número total de caracteres del mensaje enviado.


> [!TIP]
> En el emulador, haga clic en cualquier burbuja de voz de la conversación. Los detalles sobre el mensaje aparecerán en el panel de detalles, en formato JSON.

Ha creado correctamente un bot con la plantilla de aplicación Bot y Bot Builder SDK para. NET.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, creó un bot simple mediante la plantilla de aplicación Bot y Bot Builder SDK para .NET y verificó la funcionalidad del bot mediante Bot Framework Emulator.

A continuación, aprenderá los conceptos clave sobre Bot Builder SDK para .NET.

> [!div class="nextstepaction"]
> [Conceptos clave de Bot Builder SDK para .NET](bot-builder-dotnet-concepts.md)
