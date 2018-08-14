---
title: Comenzar a crear bots con Bot Builder | Microsoft Docs
description: Comience a crear bots eficaces con Bot Builder.
author: robstand
ms.author: kamrani
manager: kamrani
ms.topic: get-started-article
ms.prod: bot-framework
ms.date: 12/13/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: 21186c5d3b0769311e4703ca1dab2f48a0a0081a
ms.sourcegitcommit: 67445b42796d90661afc643c6bb6533e9a662cbc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2018
ms.locfileid: "39574891"
---
# <a name="develop-bots-with-bot-builder"></a><span data-ttu-id="0e72a-103">Desarrollo de bots con Bot Builder</span><span class="sxs-lookup"><span data-stu-id="0e72a-103">Develop bots with Bot Builder</span></span>

[!INCLUDE [pre-release-label](includes/pre-release-label-v3.md)]

<span data-ttu-id="0e72a-104">Bot Builder proporciona un SDK, bibliotecas, ejemplos y herramientas que ayudan a compilar y depurar los bots.</span><span class="sxs-lookup"><span data-stu-id="0e72a-104">The Bot Builder provides an SDK, libraries, samples, and tools to help you build and debug bots.</span></span> <span data-ttu-id="0e72a-105">Cuando se compila un bot con Bot Service, el bot está respaldado por el SDK de Bot Builder.</span><span class="sxs-lookup"><span data-stu-id="0e72a-105">When you build a bot with Bot Service, your bot is backed by the Bot Builder SDK.</span></span> <span data-ttu-id="0e72a-106">También puede usar el SDK de Bot Builder para crear un bot desde cero mediante C# o Node.js.</span><span class="sxs-lookup"><span data-stu-id="0e72a-106">You can also use the Bot Builder SDK to create a bot from scratch using C# or Node.js.</span></span> <span data-ttu-id="0e72a-107">Bot Builder incluye Bot Framework Emulator para probar los bots, y el inspector de canales para previsualizar la experiencia del usuario del bot en distintos canales.</span><span class="sxs-lookup"><span data-stu-id="0e72a-107">Bot Builder includes the Bot Framework Emulator for testing your bots and the Channel Inspector for previewing your bot's user experience on different channels.</span></span>

<span data-ttu-id="0e72a-108">Si quiere crear un bot con cualquier lenguaje de programación, puede usar la API REST.</span><span class="sxs-lookup"><span data-stu-id="0e72a-108">If you want to build a bot using any programming language you want, you can use the REST API.</span></span>

## <a name="bot-builder-sdk-for-net"></a><span data-ttu-id="0e72a-109">SDK de Bot Builder para .NET</span><span class="sxs-lookup"><span data-stu-id="0e72a-109">Bot Builder SDK for .NET</span></span>
<span data-ttu-id="0e72a-110">El SDK de Bot Builder para .NET aprovecha C# para proporcionar una manera conocida de escribir bots para los desarrolladores de .NET.</span><span class="sxs-lookup"><span data-stu-id="0e72a-110">The Bot Builder SDK for .NET leverages C# to provide a familiar way for .NET developers to write bots.</span></span> <span data-ttu-id="0e72a-111">Es un marco eficaz para crear bots que pueden controlar tanto interacciones de forma libre como conversaciones más guiadas, donde el usuario selecciona entre valores posibles.</span><span class="sxs-lookup"><span data-stu-id="0e72a-111">It is a powerful framework for constructing bots that can handle both free-form interactions and more guided conversations where the user selects from possible values.</span></span> 

<span data-ttu-id="0e72a-112">El [inicio rápido de .NET](~/dotnet/bot-builder-dotnet-quickstart.md) le guiará a través de la creación de un bot con el SDK de Bot Builder para .NET.</span><span class="sxs-lookup"><span data-stu-id="0e72a-112">The [.NET Quickstart](~/dotnet/bot-builder-dotnet-quickstart.md) will guide you through creating a bot with the Bot Builder SDK for .NET.</span></span>

<span data-ttu-id="0e72a-113">EL SDK de Bot Builder para .NET está disponible como un [paquete NuGet](https://www.nuget.org/packages/Microsoft.Bot.Builder/).</span><span class="sxs-lookup"><span data-stu-id="0e72a-113">The Bot Builder SDK for .NET is available as a [NuGet package](https://www.nuget.org/packages/Microsoft.Bot.Builder/).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0e72a-114">El SDK de Bot Builder para .NET requiere .NET Framework 4.6 o posterior.</span><span class="sxs-lookup"><span data-stu-id="0e72a-114">The Bot Builder SDK for .NET requires .NET Framework 4.6 or newer.</span></span> <span data-ttu-id="0e72a-115">Si va a agregar el SDK a un proyecto existente que tiene como destino una versión anterior de .NET Framework, deberá actualizar primero el proyecto para que se dirija a .NET Framework 4.6.</span><span class="sxs-lookup"><span data-stu-id="0e72a-115">If you are adding the SDK to an existing project targeting a lower version of the .NET Framework, you will need to update your project to target .NET Framework 4.6 first.</span></span>

<span data-ttu-id="0e72a-116">Para instalar el SDK en un proyecto existente de Visual Studio, complete los pasos siguientes:</span><span class="sxs-lookup"><span data-stu-id="0e72a-116">To install the SDK in an existing Visual Studio project, complete the following steps:</span></span>

1. <span data-ttu-id="0e72a-117">En el **Explorador de soluciones**, haga clic con el botón derecho en el nombre del proyecto y seleccione **Administrar paquetes NuGet…**.</span><span class="sxs-lookup"><span data-stu-id="0e72a-117">In **Solution Explorer**, right-click the project name and select **Manage NuGet Packages...**.</span></span>
2. <span data-ttu-id="0e72a-118">En la pestaña **Examinar**, escriba "Microsoft.Bot.Builder" en el cuadro de búsqueda.</span><span class="sxs-lookup"><span data-stu-id="0e72a-118">On the **Browse** tab, type "Microsoft.Bot.Builder" into the search box.</span></span>
3. <span data-ttu-id="0e72a-119">Seleccione **Microsoft.Bot.Builder** en la lista de resultados, haga clic en **Instalar** y acepte los cambios.</span><span class="sxs-lookup"><span data-stu-id="0e72a-119">Select **Microsoft.Bot.Builder** in the list of results, click **Install**, and accept the changes.</span></span>

<span data-ttu-id="0e72a-120">También puede descargar el [código fuente](https://github.com/Microsoft/BotBuilder/tree/master/CSharp) del SDK de Bot Builder para .NET desde GitHub.</span><span class="sxs-lookup"><span data-stu-id="0e72a-120">You can also download the Bot Builder SDK for .NET [source code](https://github.com/Microsoft/BotBuilder/tree/master/CSharp) from GitHub.</span></span>

### <a name="visual-studio-project-templates"></a><span data-ttu-id="0e72a-121">Plantillas de proyecto de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0e72a-121">Visual Studio project templates</span></span>
<span data-ttu-id="0e72a-122">Descargue las plantillas de proyecto de Visual Studio para acelerar el desarrollo de bots.</span><span class="sxs-lookup"><span data-stu-id="0e72a-122">Download Visual Studio project templates to accelerate bot development.</span></span>

* <span data-ttu-id="0e72a-123">[Plantilla de bot de Visual Studio][bot-template] para el desarrollo de bots con C#</span><span class="sxs-lookup"><span data-stu-id="0e72a-123">[Bot template for Visual Studio][bot-template] for developing bots with C#</span></span>
* <span data-ttu-id="0e72a-124">[Plantilla de habilidades de Cortana de Visual Studio][cortana-template] para el desarrollo de habilidades de Cortana con C#</span><span class="sxs-lookup"><span data-stu-id="0e72a-124">[Cortana skill template for Visual Studio][cortana-template] for developing Cortana skills with C#</span></span>

> [!TIP]
> <span data-ttu-id="0e72a-125"><a href="/visualstudio/ide/how-to-locate-and-organize-project-and-item-templates" target="_blank">Obtenga más información</a> sobre cómo instalar plantillas de proyecto de Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="0e72a-125"><a href="/visualstudio/ide/how-to-locate-and-organize-project-and-item-templates" target="_blank">Learn more</a> about how to install Visual Studio 2017 project templates.</span></span>

## <a name="bot-builder-sdk-for-nodejs"></a><span data-ttu-id="0e72a-126">SDK de Bot Builder para Node.js</span><span class="sxs-lookup"><span data-stu-id="0e72a-126">Bot Builder SDK for Node.js</span></span>
<span data-ttu-id="0e72a-127">El SDK de Bot Builder para Node.js proporciona una manera conocida de escribir bots para los desarrolladores de Node.js.</span><span class="sxs-lookup"><span data-stu-id="0e72a-127">The Bot Builder SDK for Node.js provides a familiar way for Node.js developers to write bots.</span></span> <span data-ttu-id="0e72a-128">Puede usarlo para crear una amplia variedad de interfaces de usuario conversacionales, desde indicaciones sencillas hasta conversaciones de forma libre.</span><span class="sxs-lookup"><span data-stu-id="0e72a-128">You can use it to build a wide variety of conversational user interfaces, from simple prompts to free-form conversations.</span></span> <span data-ttu-id="0e72a-129">El SDK de Bot Builder para Node.js usa restify, un marco conocido para la creación de servicios web, para crear el servidor web del bot.</span><span class="sxs-lookup"><span data-stu-id="0e72a-129">The Bot Builder SDK for Node.js uses restify, a popular framework for building web services, to create the bot's web server.</span></span> <span data-ttu-id="0e72a-130">El SDK también es compatible con Express.</span><span class="sxs-lookup"><span data-stu-id="0e72a-130">The SDK is also compatible with Express.</span></span> 

<span data-ttu-id="0e72a-131">El [inicio rápido de Node.js](~/nodejs/bot-builder-nodejs-quickstart.md) le guiará a través de la creación de un bot con el SDK de Bot Builder para Node.js.</span><span class="sxs-lookup"><span data-stu-id="0e72a-131">The [Node.js Quickstart](~/nodejs/bot-builder-nodejs-quickstart.md) will guide you through creating a bot with the Bot Builder SDK for Node.js.</span></span> 

<span data-ttu-id="0e72a-132">El SDK de Bot Builder para Node.js está disponible como un paquete npm.</span><span class="sxs-lookup"><span data-stu-id="0e72a-132">The Bot Builder SDK for Node.js is available as an npm package.</span></span> <span data-ttu-id="0e72a-133">Para instalar el SDK de Bot Builder para Node.js y sus dependencias, primero cree una carpeta para el bot, acceda a ella y ejecute el comando **npm** siguiente:</span><span class="sxs-lookup"><span data-stu-id="0e72a-133">To install the Bot Builder for Node.js SDK and its dependencies, first create a folder for your bot, navigate to it, and run the following **npm** command:</span></span>

```nodejs
npm init
```

<span data-ttu-id="0e72a-134">A continuación, instale el SDK de Bot Builder para Node.js y los módulos de <a href="http://restify.com/" target="_blank">Restify</a> mediante la ejecución de los comandos **npm** siguientes:</span><span class="sxs-lookup"><span data-stu-id="0e72a-134">Next, install the Bot Builder SDK for Node.js and <a href="http://restify.com/" target="_blank">Restify</a> modules by running the following **npm** commands:</span></span>

```nodejs
npm install --save botbuilder
npm install --save restify
```

<span data-ttu-id="0e72a-135">También puede descargar el [código fuente](https://github.com/Microsoft/BotBuilder/tree/master/Node) del SDK de Bot Builder para Node.js desde GitHub.</span><span class="sxs-lookup"><span data-stu-id="0e72a-135">You can also download the Bot Builder SDK for Node.js [source code](https://github.com/Microsoft/BotBuilder/tree/master/Node) from GitHub.</span></span>

## <a name="rest-api"></a><span data-ttu-id="0e72a-136">API DE REST</span><span class="sxs-lookup"><span data-stu-id="0e72a-136">REST API</span></span>

<span data-ttu-id="0e72a-137">Puede crear un bot con cualquier lenguaje de programación mediante la API REST de Bot Framework.</span><span class="sxs-lookup"><span data-stu-id="0e72a-137">You can create a bot with any programming language by using the Bot Framework REST API.</span></span> <span data-ttu-id="0e72a-138">El [inicio rápido de REST](rest-api/bot-framework-rest-connector-quickstart.md) le guiará a través de la creación de un bot con REST.</span><span class="sxs-lookup"><span data-stu-id="0e72a-138">The [REST Quickstart](rest-api/bot-framework-rest-connector-quickstart.md) will guide you through creating a bot with REST.</span></span>

<span data-ttu-id="0e72a-139">Tres de las API REST están en Bot Framework:</span><span class="sxs-lookup"><span data-stu-id="0e72a-139">There are three REST APIs in the Bot Framework:</span></span>

 - <span data-ttu-id="0e72a-140">La [API REST de Bot Connector][connectorAPI] permite que el bot envíe y reciba mensajes en los canales configurados en el [portal de Bot Framework](https://dev.botframework.com/).</span><span class="sxs-lookup"><span data-stu-id="0e72a-140">The [Bot Connector REST API][connectorAPI] enables your bot to send and receive messages to channels configured in the [Bot Framework Portal](https://dev.botframework.com/).</span></span> 

- <span data-ttu-id="0e72a-141">La [API REST de Bot State][stateAPI] permite que el bot almacene y recupere el estado asociado a las conversaciones que se llevan a cabo a través de la API REST de Bot Connector.</span><span class="sxs-lookup"><span data-stu-id="0e72a-141">The [Bot State REST API][stateAPI] enables your bot to store and retrieve state associated with the conversations that are conducted through the Bot Connector REST API.</span></span>

- <span data-ttu-id="0e72a-142">La [API REST de Direct Line][directLineAPI] le permite conectar su propia aplicación, por ejemplo, una aplicación cliente, un control Chat en web o una aplicación móvil, directamente a un bot único.</span><span class="sxs-lookup"><span data-stu-id="0e72a-142">The [Direct Line REST API][directLineAPI] enables you to connect your own application, such as a client application, web chat control, or mobile app, directly to a single bot.</span></span>

## <a name="bot-framework-emulator"></a><span data-ttu-id="0e72a-143">Bot Framework Emulator</span><span class="sxs-lookup"><span data-stu-id="0e72a-143">Bot Framework Emulator</span></span>
<span data-ttu-id="0e72a-144">El Bot Framework Emulator es una aplicación de escritorio que le permite probar y depurar sus bots de forma local o remota.</span><span class="sxs-lookup"><span data-stu-id="0e72a-144">The Bot Framework Emulator is a desktop application that allows you to test and debug you bots, either locally or remotely.</span></span> <span data-ttu-id="0e72a-145">Con el emulador, puede chatear con el bot e inspeccionar los mensajes que este envía y recibe.</span><span class="sxs-lookup"><span data-stu-id="0e72a-145">Using the emulator, you can chat with your bot and inspect the messages that your bot sends and receives.</span></span> <span data-ttu-id="0e72a-146">[Obtenga más información sobre el emulador de Bot Framework](~/bot-service-debug-emulator.md) y [descargue el emulador](http://emulator.botframework.com) para su plataforma.</span><span class="sxs-lookup"><span data-stu-id="0e72a-146">[Learn more about the Bot Framework emulator](~/bot-service-debug-emulator.md) and [download the emulator](http://emulator.botframework.com) for your platform.</span></span>

## <a name="bot-framework-channel-inspector"></a><span data-ttu-id="0e72a-147">Bot Framework Channel Inspector</span><span class="sxs-lookup"><span data-stu-id="0e72a-147">Bot Framework Channel Inspector</span></span>
<span data-ttu-id="0e72a-148">Vea rápidamente lasqué aspecto tendrán las características del bot en los distintos canales con Bot Framework [Channel Inspector](bot-service-channel-inspector.md).</span><span class="sxs-lookup"><span data-stu-id="0e72a-148">Quickly see what bot features will look like on different channels with the Bot Framework [Channel Inspector](bot-service-channel-inspector.md).</span></span>

[bot-template]: http://aka.ms/bf-bc-vstemplate
[cortana-template]: https://aka.ms/bf-cortanaskill-template


[connectorAPI]: https://docs.botframework.com/en-us/restapi/connector/#navtitle
 
[stateAPI]: https://docs.botframework.com/en-us/restapi/state/#navtitle

[directLineAPI]: https://docs.botframework.com/en-us/restapi/directline3/#navtitle
