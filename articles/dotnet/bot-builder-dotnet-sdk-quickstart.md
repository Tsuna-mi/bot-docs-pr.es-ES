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
# <a name="create-a-bot-with-the-bot-builder-sdk-v4-for-net"></a><span data-ttu-id="4ec3e-104">Creación de un bot con Bot Builder SDK v4 para .NET</span><span class="sxs-lookup"><span data-stu-id="4ec3e-104">Create a bot with the Bot Builder SDK v4 for .NET</span></span>
[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]

<span data-ttu-id="4ec3e-105">En este inicio rápido se ofrece información sobre cómo crear un bot con la plantilla de aplicación bot y Bot Builder SDK para .NET, y después se prueba con Bot Framework Emulator.</span><span class="sxs-lookup"><span data-stu-id="4ec3e-105">This quickstart walks you through building a bot by using the Bot Application template and the Bot Builder SDK for .NET, and then testing it with the Bot Framework Emulator.</span></span> <span data-ttu-id="4ec3e-106">Esto se basa en [Microsoft Bot Builder SDK v4](https://github.com/Microsoft/botbuilder-dotnet).</span><span class="sxs-lookup"><span data-stu-id="4ec3e-106">This is based off the [Microsoft Bot Builder SDK v4](https://github.com/Microsoft/botbuilder-dotnet).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4ec3e-107">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="4ec3e-107">Prerequisites</span></span>
- <span data-ttu-id="4ec3e-108">Visual Studio [2017](https://www.visualstudio.com/downloads)</span><span class="sxs-lookup"><span data-stu-id="4ec3e-108">Visual Studio [2017](https://www.visualstudio.com/downloads)</span></span>
- <span data-ttu-id="4ec3e-109">Plantilla de Bot Builder SDK v4 para [C#](https://marketplace.visualstudio.com/items?itemName=BotBuilder.botbuilderv4)</span><span class="sxs-lookup"><span data-stu-id="4ec3e-109">Bot Builder SDK v4 template for [C#](https://marketplace.visualstudio.com/items?itemName=BotBuilder.botbuilderv4)</span></span>
- [<span data-ttu-id="4ec3e-110">Bot Framework Emulator</span><span class="sxs-lookup"><span data-stu-id="4ec3e-110">Bot Framework Emulator</span></span>](https://github.com/Microsoft/BotFramework-Emulator/releases)
- <span data-ttu-id="4ec3e-111">Conocimientos sobre [ASP.Net Core](https://docs.microsoft.com/aspnet/core/) y la programación asincrónica en [C#](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/async/index)</span><span class="sxs-lookup"><span data-stu-id="4ec3e-111">Knowledge of [ASP.Net Core](https://docs.microsoft.com/aspnet/core/) and asynchronous programming in [C#](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/async/index)</span></span>

## <a name="create-a-bot"></a><span data-ttu-id="4ec3e-112">Creación de un bot</span><span class="sxs-lookup"><span data-stu-id="4ec3e-112">Create a bot</span></span>
<span data-ttu-id="4ec3e-113">Instale la plantilla BotBuilderVSIX.vsix que descargó en la sección de requisitos previos.</span><span class="sxs-lookup"><span data-stu-id="4ec3e-113">Install BotBuilderVSIX.vsix template that you downloaded in the prerequisite section.</span></span> 

<span data-ttu-id="4ec3e-114">En Visual Studio, cree un proyecto de bot.</span><span class="sxs-lookup"><span data-stu-id="4ec3e-114">In Visual Studio, create a new bot project.</span></span>

![Proyecto de Visual Studio](../media/azure-bot-quickstarts/bot-builder-dotnet-project.png)

> [!TIP] 
> <span data-ttu-id="4ec3e-116">Si es necesario, actualice los [paquetes NuGet](https://docs.microsoft.com/en-us/nuget/quickstart/install-and-use-a-package-in-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="4ec3e-116">If needed, update [NuGet packages](https://docs.microsoft.com/en-us/nuget/quickstart/install-and-use-a-package-in-visual-studio).</span></span>

## <a name="explore-code"></a><span data-ttu-id="4ec3e-117">Exploración del código</span><span class="sxs-lookup"><span data-stu-id="4ec3e-117">Explore code</span></span>
<span data-ttu-id="4ec3e-118">Abra el archivo Startup.cs para revisar el código en el método `ConfigureServices(IServiceCollection services)`.</span><span class="sxs-lookup"><span data-stu-id="4ec3e-118">Open Startup.cs file to review code in the `ConfigureServices(IServiceCollection services)` method.</span></span> <span data-ttu-id="4ec3e-119">El software intermedio `CatchExceptionMiddleware` se agrega a la canalización de mensajería.</span><span class="sxs-lookup"><span data-stu-id="4ec3e-119">The `CatchExceptionMiddleware` middleware is added to the messaging pipeline.</span></span> <span data-ttu-id="4ec3e-120">Controla las excepciones producidas por otro software intermedio o por el método OnTurn.</span><span class="sxs-lookup"><span data-stu-id="4ec3e-120">It handles any exceptions thrown by other middleware, or by OnTurn method.</span></span> 

```cs
options.Middleware.Add(new CatchExceptionMiddleware<Exception>(async (context, exception) =>
{
    await context.TraceActivity("EchoBot Exception", exception);
    await context.SendActivity("Sorry, it looks like something went wrong!");
}));
```

<span data-ttu-id="4ec3e-121">El software intermedio de estado de la conversación usa el almacenamiento en memoria.</span><span class="sxs-lookup"><span data-stu-id="4ec3e-121">The conversation state middleware uses in-memory storage.</span></span> <span data-ttu-id="4ec3e-122">Lee y escribe el objeto EchoState en el almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="4ec3e-122">It reads and writes the EchoState to storage.</span></span>  <span data-ttu-id="4ec3e-123">El contador de turnos de la clase EchoState realiza el seguimiento del número de mensajes que se envían al bot.</span><span class="sxs-lookup"><span data-stu-id="4ec3e-123">The turn count in EchoState class keeps track of the number of messages sent to the bot.</span></span> <span data-ttu-id="4ec3e-124">Puede usar una técnica similar para mantener el estado entre los turnos.</span><span class="sxs-lookup"><span data-stu-id="4ec3e-124">You can use a similar technique to maintain state in between turns.</span></span>

```cs
 IStorage dataStore = new MemoryStorage();
 options.Middleware.Add(new ConversationState<EchoState>(dataStore));
```

<span data-ttu-id="4ec3e-125">El método `Configure(IApplicationBuilder app, IHostingEnvironment env)` llama a `.UseBotFramework` para enrutar las actividades entrantes al adaptador de bot.</span><span class="sxs-lookup"><span data-stu-id="4ec3e-125">The `Configure(IApplicationBuilder app, IHostingEnvironment env)` method calls `.UseBotFramework` to route incoming activities to the bot adapter.</span></span> 

<span data-ttu-id="4ec3e-126">El método `OnTurn(ITurnContext context)` del archivo EchoBot.cs se usa para comprobar el tipo de actividad entrante y enviar una respuesta al usuario.</span><span class="sxs-lookup"><span data-stu-id="4ec3e-126">The `OnTurn(ITurnContext context)` method in the EchoBot.cs file is used to check the incoming activity type and send a reply to the user.</span></span> 

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
## <a name="start-your-bot"></a><span data-ttu-id="4ec3e-127">Inicio del bot</span><span class="sxs-lookup"><span data-stu-id="4ec3e-127">Start your bot</span></span>

- <span data-ttu-id="4ec3e-128">La página `default.html` se mostrará en un explorador.</span><span class="sxs-lookup"><span data-stu-id="4ec3e-128">Your `default.html` page will be displayed in a browser.</span></span>
- <span data-ttu-id="4ec3e-129">Anote el número de puerto de host local de la página.</span><span class="sxs-lookup"><span data-stu-id="4ec3e-129">Note the localhost port number for the page.</span></span> <span data-ttu-id="4ec3e-130">Necesitará esta información para interactuar con el bot.</span><span class="sxs-lookup"><span data-stu-id="4ec3e-130">You will need this information to interact with your bot.</span></span>

### <a name="start-the-emulator-and-connect-your-bot"></a><span data-ttu-id="4ec3e-131">Inicio del emulador y conexión del bot</span><span class="sxs-lookup"><span data-stu-id="4ec3e-131">Start the emulator and connect your bot</span></span>

<span data-ttu-id="4ec3e-132">En este momento, el bot se ejecuta de forma local.</span><span class="sxs-lookup"><span data-stu-id="4ec3e-132">At this point, your bot is running locally.</span></span>
<span data-ttu-id="4ec3e-133">A continuación, inicie el emulador y, después, conéctese al bot en el emulador:</span><span class="sxs-lookup"><span data-stu-id="4ec3e-133">Next, start the emulator and then connect to your bot in the emulator:</span></span>

1. <span data-ttu-id="4ec3e-134">Haga clic en el vínculo **create a new bot configuration** (Crear configuración de bot) en la pestaña "Welcome" (Bienvenido) del emulador.</span><span class="sxs-lookup"><span data-stu-id="4ec3e-134">Click **create a new bot configuration** link in the emulator "Welcome" tab.</span></span> 

2. <span data-ttu-id="4ec3e-135">Escriba un **nombre de bot** y la ruta de acceso del directorio en el código del bot.</span><span class="sxs-lookup"><span data-stu-id="4ec3e-135">Enter a **Bot name** and enter the directory path to your bot code.</span></span> <span data-ttu-id="4ec3e-136">El archivo de configuración del bot se guardará en esta ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="4ec3e-136">The bot configuration file will be saved to this path.</span></span>

3. <span data-ttu-id="4ec3e-137">Escriba `http://localhost:port-number/api/messages` en el campo **Dirección URL del punto de conexión**, donde *port-number* coincide con el número de puerto que se muestra en el explorador en el que se ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4ec3e-137">Type `http://localhost:port-number/api/messages` into the **Endpoint URL** field, where *port-number* matches the port number shown in the browser where your application is running.</span></span>

4. <span data-ttu-id="4ec3e-138">Haga clic en **Connect** (Conectar) para conectarse al bot.</span><span class="sxs-lookup"><span data-stu-id="4ec3e-138">Click **Connect** to connect to your bot.</span></span> <span data-ttu-id="4ec3e-139">No tendrá que especificar el **id. de la aplicación de Microsoft** ni la **contraseña de la aplicación de Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="4ec3e-139">You won't need to specify **Microsoft App ID** and **Microsoft App Password**.</span></span> <span data-ttu-id="4ec3e-140">Por ahora puede dejar estos campos en blanco.</span><span class="sxs-lookup"><span data-stu-id="4ec3e-140">You can leave these fields blank for now.</span></span> <span data-ttu-id="4ec3e-141">Obtendrá esta información más adelante al registrar el bot.</span><span class="sxs-lookup"><span data-stu-id="4ec3e-141">You'll get this information later when you register your bot.</span></span>

## <a name="interact-with-your-bot"></a><span data-ttu-id="4ec3e-142">Interacción con el bot</span><span class="sxs-lookup"><span data-stu-id="4ec3e-142">Interact with your bot</span></span>

<span data-ttu-id="4ec3e-143">Envíe "Hi" (Hola) al bot y el bot responderá al mensaje con "Turn 1: You sent Hi" (Tuno 1: Ha enviado Hola).</span><span class="sxs-lookup"><span data-stu-id="4ec3e-143">Send "Hi" to your bot, and the bot will respond with "Turn 1: You sent Hi" to the message.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4ec3e-144">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="4ec3e-144">Next steps</span></span>

<span data-ttu-id="4ec3e-145">A continuación, [implemente el bot en Azure](../bot-builder-howto-deploy-azure.md) o vaya a los conceptos que explican qué es un bot y cómo funciona.</span><span class="sxs-lookup"><span data-stu-id="4ec3e-145">Next, [deploy your bot to azure](../bot-builder-howto-deploy-azure.md) or jump into the concepts that explain a bot and how it works.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="4ec3e-146">Conceptos básicos de bot</span><span class="sxs-lookup"><span data-stu-id="4ec3e-146">Basic Bot concepts</span></span>](../v4sdk/bot-builder-basics.md)
