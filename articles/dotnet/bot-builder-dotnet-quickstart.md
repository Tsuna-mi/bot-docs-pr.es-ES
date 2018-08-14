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
ms.openlocfilehash: fcb21fa38750c09f110a3c71f763a941c4437979
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39306197"
---
# <a name="create-a-bot-with-the-bot-builder-sdk-for-net"></a><span data-ttu-id="bd9df-104">Creación de un bot con Bot Builder SDK para .NET</span><span class="sxs-lookup"><span data-stu-id="bd9df-104">Create a bot with the Bot Builder SDK for .NET</span></span>
> [!div class="op_single_selector"]
> - [.NET](../dotnet/bot-builder-dotnet-quickstart.md)
> - [Node.js](../nodejs/bot-builder-nodejs-quickstart.md)
> - [Bot Service](../bot-service-quickstart.md)
> - [REST](../rest-api/bot-framework-rest-connector-quickstart.md)

<span data-ttu-id="bd9df-109"><a href="https://github.com/Microsoft/BotBuilder" target="_blank">Bot Builder SDK para .NET</a> es un marco fácil de usar para el desarrollo de bots con Visual Studio y Windows.</span><span class="sxs-lookup"><span data-stu-id="bd9df-109">The <a href="https://github.com/Microsoft/BotBuilder" target="_blank">Bot Builder SDK for .NET</a> is an easy-to-use framework for developing bots using Visual Studio and Windows.</span></span> <span data-ttu-id="bd9df-110">El SDK utiliza C# para proporcionar una manera conocida de crear bots eficaces para los desarrolladores de .NET.</span><span class="sxs-lookup"><span data-stu-id="bd9df-110">The SDK leverages C# to provide a familiar way for .NET developers to create powerful bots.</span></span>


<span data-ttu-id="bd9df-111">En este tutorial se ofrece información sobre cómo crear un bot con la plantilla de aplicación bot y Bot Builder SDK para .NET, y después se prueba con Bot Framework Emulator.</span><span class="sxs-lookup"><span data-stu-id="bd9df-111">This tutorial walks you through building a bot by using the Bot Application template and the Bot Builder SDK for .NET, and then testing it with the Bot Framework Emulator.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bd9df-112">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="bd9df-112">Prerequisites</span></span>
1. <span data-ttu-id="bd9df-113">Visual Studio [2017](https://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="bd9df-113">Visual Studio [2017](https://www.visualstudio.com/).</span></span>

2. <span data-ttu-id="bd9df-114">En Visual Studio, actualice todas las [extensiones](https://docs.microsoft.com/en-us/visualstudio/extensibility/how-to-update-a-visual-studio-extension) a sus versiones más recientes.</span><span class="sxs-lookup"><span data-stu-id="bd9df-114">In Visual Studio, update all [extensions](https://docs.microsoft.com/en-us/visualstudio/extensibility/how-to-update-a-visual-studio-extension) to their latest versions.</span></span>

3. <span data-ttu-id="bd9df-115">Plantilla de bot para [C#](https://marketplace.visualstudio.com/items?itemName=BotBuilder.BotBuilderV3).</span><span class="sxs-lookup"><span data-stu-id="bd9df-115">Bot template for [C#](https://marketplace.visualstudio.com/items?itemName=BotBuilder.BotBuilderV3).</span></span>

> [!TIP]
> El directorio de plantillas de proyecto de Visual Studio 2017 suele ubicarse en `%USERPROFILE%\Documents\Visual Studio 2017\Templates\ProjectTemplates\Visual C#\` y el directorio de plantillas de elemento se encuentra en `%USERPROFILE%\Documents\Visual Studio 2017\Templates\ItemTemplates\Visual C#\`

## <a name="create-your-bot"></a><span data-ttu-id="bd9df-117">Creación del bot</span><span class="sxs-lookup"><span data-stu-id="bd9df-117">Create your bot</span></span>

<span data-ttu-id="bd9df-118">Abra Visual Studio y cree un proyecto de C#.</span><span class="sxs-lookup"><span data-stu-id="bd9df-118">Open Visual Studio and create a new C# project.</span></span> <span data-ttu-id="bd9df-119">Elija la plantilla **Simple Echo Bot Application** (Aplicación de bot de echo sencilla) para el proyecto nuevo.</span><span class="sxs-lookup"><span data-stu-id="bd9df-119">Choose the **Simple Echo Bot Application** template for your new project.</span></span>

![Creación de un proyecto en Visual Studio](../media/connector-getstarted-create-project.png)

> [!NOTE]
> Visual Studio podría indicar que debe descargar e instalar [IIS Express](https://www.microsoft.com/en-us/download/details.aspx?id=48264). 

<span data-ttu-id="bd9df-122">Gracias a la plantilla de la aplicación Bot, el proyecto contiene todo el código que es necesario para crear el bot en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="bd9df-122">Thanks to the Bot Application template, your project contains all of the code that's necessary to create the bot in this tutorial.</span></span> <span data-ttu-id="bd9df-123">Realmente no necesita escribir ningún código adicional.</span><span class="sxs-lookup"><span data-stu-id="bd9df-123">You won't actually need to write any additional code.</span></span> <span data-ttu-id="bd9df-124">Sin embargo, antes de pasar a las pruebas de un bot, eche un vistazo rápido a parte del código que proporciona la plantilla de la aplicación Bot.</span><span class="sxs-lookup"><span data-stu-id="bd9df-124">However, before we move on to testing your bot, take a quick look at some of the code that the Bot Application template provided.</span></span>

> [!TIP] 
> Si es necesario, actualice los [paquetes NuGet](https://docs.microsoft.com/en-us/nuget/quickstart/install-and-use-a-package-in-visual-studio).

## <a name="explore-the-code"></a><span data-ttu-id="bd9df-126">Exploración del código</span><span class="sxs-lookup"><span data-stu-id="bd9df-126">Explore the code</span></span>

<span data-ttu-id="bd9df-127">En primer lugar, el método `Post` de **Controllers\MessagesController.cs** recibe el mensaje del usuario e invoca el diálogo raíz.</span><span class="sxs-lookup"><span data-stu-id="bd9df-127">First, the `Post` method within **Controllers\MessagesController.cs** receives the message from the user and invokes the root dialog.</span></span>

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

<span data-ttu-id="bd9df-128">El diálogo raíz procesa el mensaje y genera una respuesta.</span><span class="sxs-lookup"><span data-stu-id="bd9df-128">The root dialog processes the message and generates a response.</span></span> <span data-ttu-id="bd9df-129">El método `MessageReceivedAsync` de **Dialogs\RootDialog.cs** envía una respuesta que reproduce el mensaje del usuario, con el prefijo "Usted envió" y que termina con el texto "que tenía *##* caracteres", donde *##* representa el número de caracteres del mensaje del usuario.</span><span class="sxs-lookup"><span data-stu-id="bd9df-129">The `MessageReceivedAsync` method within **Dialogs\RootDialog.cs** sends a reply that echos back the user's message, prefixed with the text 'You sent' and ending in the text 'which was *##* characters', where *##* represents the number of characters in the user's message.</span></span>

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

## <a name="test-your-bot"></a><span data-ttu-id="bd9df-130">Prueba del bot</span><span class="sxs-lookup"><span data-stu-id="bd9df-130">Test your bot</span></span>

[!INCLUDE [Get started test your bot](../includes/snippet-getstarted-test-bot.md)]

### <a name="start-your-bot"></a><span data-ttu-id="bd9df-131">Inicio del bot</span><span class="sxs-lookup"><span data-stu-id="bd9df-131">Start your bot</span></span>

<span data-ttu-id="bd9df-132">Después de instalar el emulador, inicie el bot en Visual Studio mediante un explorador como el host de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bd9df-132">After installing the emulator, start your bot in Visual Studio by using a browser as the application host.</span></span>
<span data-ttu-id="bd9df-133">Esta captura de pantalla de Visual Studio muestra que, cuando se hace clic en el botón de ejecución, se iniciará el bot en Microsoft Edge.</span><span class="sxs-lookup"><span data-stu-id="bd9df-133">This Visual Studio screenshot shows that the bot will launch in Microsoft Edge when the run button is clicked.</span></span>

![Ejecución del proyecto en Visual Studio](../media/connector-getstarted-start-bot-locally.png)

<span data-ttu-id="bd9df-135">Al hacer clic en el botón de ejecución, Visual Studio compila la aplicación, la implementa en localhost e inicia el explorador web para mostrar la página **default.htm** de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bd9df-135">When you click the run button, Visual Studio will build the application, deploy it to localhost, and launch the web browser to display the application's **default.htm** page.</span></span>
<span data-ttu-id="bd9df-136">Por ejemplo, esta es la página **default.htm** de la aplicación que se muestra en Microsoft Edge:</span><span class="sxs-lookup"><span data-stu-id="bd9df-136">For example, here's the application's **default.htm** page shown in Microsoft Edge:</span></span>

![Localhost de ejecución del bot en Visual Studio](../media/connector-getstarted-bot-running-localhost.png)

> [!NOTE]
> Puede modificar el archivo **default.htm** en el proyecto para especificar el nombre y la descripción de la aplicación bot.

### <a name="start-the-emulator-and-connect-your-bot"></a><span data-ttu-id="bd9df-139">Inicio del emulador y conexión del bot</span><span class="sxs-lookup"><span data-stu-id="bd9df-139">Start the emulator and connect your bot</span></span>

<span data-ttu-id="bd9df-140">En este momento, el bot se ejecuta de forma local.</span><span class="sxs-lookup"><span data-stu-id="bd9df-140">At this point, your bot is running locally.</span></span>
<span data-ttu-id="bd9df-141">A continuación, inicie el emulador y, después, conéctese al bot en el emulador:</span><span class="sxs-lookup"><span data-stu-id="bd9df-141">Next, start the emulator and then connect to your bot in the emulator:</span></span>

1. <span data-ttu-id="bd9df-142">Cree una configuración de bot.</span><span class="sxs-lookup"><span data-stu-id="bd9df-142">Create a new bot configuration.</span></span> <span data-ttu-id="bd9df-143">Escriba `http://localhost:port-number/api/messages` en la barra de dirección, donde *port-number* coincide con el número de puerto mostrado en el explorador donde se ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bd9df-143">Type `http://localhost:port-number/api/messages` into the address bar, where *port-number* matches the port number shown in the browser where your application is running.</span></span>

2. <span data-ttu-id="bd9df-144">Haga clic en **Save and connect** (Guardar y conectar).</span><span class="sxs-lookup"><span data-stu-id="bd9df-144">Click **Save and connect**.</span></span> <span data-ttu-id="bd9df-145">No tendrá que especificar el **id. de la aplicación de Microsoft** ni la **contraseña de la aplicación de Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="bd9df-145">You won't need to specify **Microsoft App ID** and **Microsoft App Password**.</span></span> <span data-ttu-id="bd9df-146">Por ahora puede dejar estos campos en blanco.</span><span class="sxs-lookup"><span data-stu-id="bd9df-146">You can leave these fields blank for now.</span></span> <span data-ttu-id="bd9df-147">Más adelante obtendrá esta información al [registrar el bot](~/bot-service-quickstart-registration.md).</span><span class="sxs-lookup"><span data-stu-id="bd9df-147">You'll get this information later when you [register your bot](~/bot-service-quickstart-registration.md).</span></span>

### <a name="test-your-bot"></a><span data-ttu-id="bd9df-148">Prueba del bot</span><span class="sxs-lookup"><span data-stu-id="bd9df-148">Test your bot</span></span>

<span data-ttu-id="bd9df-149">Ahora que el bot se ejecuta localmente y está conectado al emulador, escriba algunos mensajes en el emulador para probar el bot.</span><span class="sxs-lookup"><span data-stu-id="bd9df-149">Now that your bot is running locally and is connected to the emulator, test your bot by typing a few messages in the emulator.</span></span>
<span data-ttu-id="bd9df-150">Debe observar que el bot responde a cada mensaje enviado al reproducir el mensaje con el prefijo "Usted envío" y que termina con el texto "que tenía *##* caracteres", donde *##* es el número total de caracteres del mensaje enviado.</span><span class="sxs-lookup"><span data-stu-id="bd9df-150">You should see that the bot responds to each message you send by echoing back your message prefixed with the text 'You sent' and ending with the text 'which was *##* characters', where *##* is the total number of characters in the message that you sent.</span></span>


> [!TIP]
> En el emulador, haga clic en cualquier burbuja de voz de la conversación. Los detalles sobre el mensaje aparecerán en el panel de detalles, en formato JSON.

<span data-ttu-id="bd9df-153">Ha creado correctamente un bot con la plantilla de aplicación Bot y Bot Builder SDK para. NET.</span><span class="sxs-lookup"><span data-stu-id="bd9df-153">You've successfully created a bot by using the Bot Application template Bot Builder SDK for .NET!</span></span>

## <a name="next-steps"></a><span data-ttu-id="bd9df-154">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="bd9df-154">Next steps</span></span>

<span data-ttu-id="bd9df-155">En este tutorial, creó un bot simple mediante la plantilla de aplicación Bot y Bot Builder SDK para .NET y verificó la funcionalidad del bot mediante Bot Framework Emulator.</span><span class="sxs-lookup"><span data-stu-id="bd9df-155">In this quickstart, you created a simple bot by using the Bot Application template and the Bot Builder SDK for .NET and verified the bot's functionality by using the Bot Framework Emulator.</span></span>

<span data-ttu-id="bd9df-156">A continuación, aprenderá los conceptos clave sobre Bot Builder SDK para .NET.</span><span class="sxs-lookup"><span data-stu-id="bd9df-156">Next, learn about the key concepts of the Bot Builder SDK for .NET.</span></span>

> [!div class="nextstepaction"]
> [Conceptos clave de Bot Builder SDK para .NET](bot-builder-dotnet-concepts.md)
