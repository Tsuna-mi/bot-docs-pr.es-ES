---
title: Creación de un bot con Bot Builder SDK para .NET | Microsoft Docs
description: Cree un bot con Bot Builder SDK para .NET, un marco de trabajo eficaz para la creación de bots.
keywords: Bot Builder SDK, creación de un bot, inicio rápido, .NET, introducción
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: get-started-article
ms.prod: bot-framework
ms.date: 04/27/2018
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: 98c6103f0cd38bf6d3f477735dafcf01952304d5
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39306236"
---
# <a name="create-a-bot-with-the-bot-builder-sdk-v4-for-net"></a>Creación de un bot con Bot Builder SDK v4 para .NET
[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]

En este inicio rápido se ofrece información sobre cómo crear un bot con la plantilla de aplicación bot y Bot Builder SDK para .NET, y después se prueba con Bot Framework Emulator. Esto se basa en [Microsoft Bot Builder SDK v4](https://github.com/Microsoft/botbuilder-dotnet).

## <a name="prerequisites"></a>Requisitos previos
- Visual Studio [2017](https://www.visualstudio.com/downloads)
- Plantilla de Bot Builder SDK v4 para [C#](https://marketplace.visualstudio.com/items?itemName=BotBuilder.botbuilderv4)
- [Bot Framework Emulator](https://github.com/Microsoft/BotFramework-Emulator/releases)
- Conocimientos sobre [ASP.Net Core](https://docs.microsoft.com/aspnet/core/) y la programación asincrónica en [C#](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/async/index)

## <a name="create-a-bot"></a>Creación de un bot
Instale la plantilla BotBuilderVSIX.vsix que descargó en la sección de requisitos previos. 

En Visual Studio, cree un proyecto de bot.

![Proyecto de Visual Studio](../media/azure-bot-quickstarts/bot-builder-dotnet-project.png)

> [!TIP] 
> Si es necesario, actualice los [paquetes NuGet](https://docs.microsoft.com/en-us/nuget/quickstart/install-and-use-a-package-in-visual-studio).

## <a name="explore-code"></a>Exploración del código
Abra el archivo Startup.cs para revisar el código en el método `ConfigureServices(IServiceCollection services)`. El software intermedio `CatchExceptionMiddleware` se agrega a la canalización de mensajería. Controla las excepciones producidas por otro software intermedio o por el método OnTurn. 

```cs
options.Middleware.Add(new CatchExceptionMiddleware<Exception>(async (context, exception) =>
{
    await context.TraceActivity("EchoBot Exception", exception);
    await context.SendActivity("Sorry, it looks like something went wrong!");
}));
```

El software intermedio de estado de la conversación usa el almacenamiento en memoria. Lee y escribe el objeto EchoState en el almacenamiento.  El contador de turnos de la clase EchoState realiza el seguimiento del número de mensajes que se envían al bot. Puede usar una técnica similar para mantener el estado entre los turnos.

```cs
 IStorage dataStore = new MemoryStorage();
 options.Middleware.Add(new ConversationState<EchoState>(dataStore));
```

El método `Configure(IApplicationBuilder app, IHostingEnvironment env)` llama a `.UseBotFramework` para enrutar las actividades entrantes al adaptador de bot. 

El método `OnTurn(ITurnContext context)` del archivo EchoBot.cs se usa para comprobar el tipo de actividad entrante y enviar una respuesta al usuario. 

```cs
public async Task OnTurn(ITurnContext context)
{
    // This bot is only handling Messages
    if (context.Activity.Type == ActivityTypes.Message)
    {
        // Get the conversation state from the turn context
        var state = context.GetConversationState<EchoState>();

        // Bump the turn count. 
        state.TurnCount++;

        // Echo back to the user whatever they typed.
        await context.SendActivity($"Turn {state.TurnCount}: You sent '{context.Activity.Text}'");
    }
}
```
## <a name="start-your-bot"></a>Inicio del bot

- La página `default.html` se mostrará en un explorador.
- Anote el número de puerto de host local de la página. Necesitará esta información para interactuar con el bot.

### <a name="start-the-emulator-and-connect-your-bot"></a>Inicio del emulador y conexión del bot

En este momento, el bot se ejecuta de forma local.
A continuación, inicie el emulador y, después, conéctese al bot en el emulador:

1. Haga clic en el vínculo **create a new bot configuration** (Crear configuración de bot) en la pestaña "Welcome" (Bienvenido) del emulador. 

2. Escriba un **nombre de bot** y la ruta de acceso del directorio en el código del bot. El archivo de configuración del bot se guardará en esta ruta de acceso.

3. Escriba `http://localhost:port-number/api/messages` en el campo **Dirección URL del punto de conexión**, donde *port-number* coincide con el número de puerto que se muestra en el explorador en el que se ejecuta la aplicación.

4. Haga clic en **Connect** (Conectar) para conectarse al bot. No tendrá que especificar el **id. de la aplicación de Microsoft** ni la **contraseña de la aplicación de Microsoft**. Por ahora puede dejar estos campos en blanco. Obtendrá esta información más adelante al registrar el bot.

## <a name="interact-with-your-bot"></a>Interacción con el bot

Envíe "Hi" (Hola) al bot y el bot responderá al mensaje con "Turn 1: You sent Hi" (Tuno 1: Ha enviado Hola).

## <a name="next-steps"></a>Pasos siguientes

A continuación, [implemente el bot en Azure](../bot-builder-howto-deploy-azure.md) o vaya a los conceptos que explican qué es un bot y cómo funciona.

> [!div class="nextstepaction"]
> [Conceptos básicos de bot](../v4sdk/bot-builder-basics.md)
