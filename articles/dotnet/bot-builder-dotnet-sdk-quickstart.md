---
title: Creación de un bot con Bot Builder SDK para .NET | Microsoft Docs
description: Cree un bot con Bot Builder SDK para .NET, un marco de trabajo eficaz para la creación de bots.
keywords: SDK Bot Builder, creación de un bot, guía de inicio rápido, .NET, introducción, bot de C#
author: kamrani
ms.author: kamrani
manager: kamrani
ms.topic: get-started-article
ms.prod: bot-framework
ms.date: 09/23/2018
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: 5f3a02783242697fccf267bef2d56ed453880c67
ms.sourcegitcommit: 3bf3dbb1a440b3d83e58499c6a2ac116fe04b2f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/23/2018
ms.locfileid: "46707981"
---
# <a name="create-a-bot-with-the-bot-builder-sdk-for-net"></a><span data-ttu-id="7fef7-104">Creación de un bot con Bot Builder SDK para .NET</span><span class="sxs-lookup"><span data-stu-id="7fef7-104">Create a bot with the Bot Builder SDK for .NET</span></span>
[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]

<span data-ttu-id="7fef7-105">Esta guía de inicio rápido le orienta en el desarrollo de un bot con la plantilla de C# y, a continuación, con la prueba con Bot Framework Emulator.</span><span class="sxs-lookup"><span data-stu-id="7fef7-105">This quickstart walks you through building a bot by using the C# template, and then testing it with the Bot Framework Emulator.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="7fef7-106">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="7fef7-106">Prerequisites</span></span>
- <span data-ttu-id="7fef7-107">Visual Studio [2017](https://www.visualstudio.com/downloads)</span><span class="sxs-lookup"><span data-stu-id="7fef7-107">Visual Studio [2017](https://www.visualstudio.com/downloads)</span></span>
- <span data-ttu-id="7fef7-108">Plantilla de Bot Builder SDK v4 para [C#](https://botbuilder.myget.org/feed/aitemplates/package/vsix/BotBuilderV4.fbe0fc50-a6f1-4500-82a2-189314b7bea2)</span><span class="sxs-lookup"><span data-stu-id="7fef7-108">Bot Builder SDK v4 template for [C#](https://botbuilder.myget.org/feed/aitemplates/package/vsix/BotBuilderV4.fbe0fc50-a6f1-4500-82a2-189314b7bea2)</span></span>
- <span data-ttu-id="7fef7-109">Bot Framework [Emulator](https://github.com/Microsoft/BotFramework-Emulator/releases)</span><span class="sxs-lookup"><span data-stu-id="7fef7-109">Bot Framework [Emulator](https://github.com/Microsoft/BotFramework-Emulator/releases)</span></span>
- <span data-ttu-id="7fef7-110">Conocimientos sobre [ASP.Net Core](https://docs.microsoft.com/aspnet/core/) y la programación asincrónica en [C#](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/async/index)</span><span class="sxs-lookup"><span data-stu-id="7fef7-110">Knowledge of [ASP.Net Core](https://docs.microsoft.com/aspnet/core/) and asynchronous programming in [C#](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/async/index)</span></span>

## <a name="create-a-bot"></a><span data-ttu-id="7fef7-111">Creación de un bot</span><span class="sxs-lookup"><span data-stu-id="7fef7-111">Create a bot</span></span>
<span data-ttu-id="7fef7-112">Instale la plantilla BotBuilderVSIX.vsix que descargó en la sección de requisitos previos.</span><span class="sxs-lookup"><span data-stu-id="7fef7-112">Install BotBuilderVSIX.vsix template that you downloaded in the prerequisites section.</span></span> 

<span data-ttu-id="7fef7-113">En Visual Studio, cree un proyecto de bot.</span><span class="sxs-lookup"><span data-stu-id="7fef7-113">In Visual Studio, create a new bot project.</span></span>

![Proyecto de Visual Studio](../media/azure-bot-quickstarts/bot-builder-dotnet-project.png)

> [!TIP] 
> <span data-ttu-id="7fef7-115">Si es necesario, actualice los [paquetes NuGet](https://docs.microsoft.com/en-us/nuget/quickstart/install-and-use-a-package-in-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="7fef7-115">If needed, update [NuGet packages](https://docs.microsoft.com/en-us/nuget/quickstart/install-and-use-a-package-in-visual-studio).</span></span>

<span data-ttu-id="7fef7-116">Gracias a la plantilla, el proyecto contiene todo el código necesario para crear el bot en esta guía de inicio rápido.</span><span class="sxs-lookup"><span data-stu-id="7fef7-116">Thanks to the template, your project contains all of the code that's necessary to create the bot in this quickstart.</span></span> <span data-ttu-id="7fef7-117">Realmente no necesita escribir ningún código adicional.</span><span class="sxs-lookup"><span data-stu-id="7fef7-117">You won't actually need to write any additional code.</span></span>

## <a name="start-your-bot-in-visual-studio"></a><span data-ttu-id="7fef7-118">Inicio del bot en Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7fef7-118">Start your bot in Visual Studio</span></span>

<span data-ttu-id="7fef7-119">Al hacer clic en el botón de ejecución, Visual Studio compila la aplicación, la implementa en localhost e inicia el explorador web para mostrar la página `default.htm` de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7fef7-119">When you click the run button, Visual Studio will build the application, deploy it to localhost, and launch the web browser to display the application's `default.htm` page.</span></span> <span data-ttu-id="7fef7-120">En este momento, el bot se ejecuta de forma local.</span><span class="sxs-lookup"><span data-stu-id="7fef7-120">At this point, your bot is running locally.</span></span>

## <a name="start-the-emulator-and-connect-your-bot"></a><span data-ttu-id="7fef7-121">Inicio del emulador y conexión del bot</span><span class="sxs-lookup"><span data-stu-id="7fef7-121">Start the emulator and connect your bot</span></span>

<span data-ttu-id="7fef7-122">A continuación, inicie el emulador y, después, conéctese al bot en el emulador:</span><span class="sxs-lookup"><span data-stu-id="7fef7-122">Next, start the emulator and then connect to your bot in the emulator:</span></span>

1. <span data-ttu-id="7fef7-123">Haga clic en el vínculo **Open Bot** (Abrir bot) de la pestaña de bienvenida del emulador.</span><span class="sxs-lookup"><span data-stu-id="7fef7-123">Click the **Open Bot** link in the emulator "Welcome" tab.</span></span> 
2. <span data-ttu-id="7fef7-124">Seleccione el archivo .bot ubicado en el directorio donde se creó la solución de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7fef7-124">Select the .bot file located in the directory where you created the Visual Studio solution.</span></span>

## <a name="interact-with-your-bot"></a><span data-ttu-id="7fef7-125">Interacción con el bot</span><span class="sxs-lookup"><span data-stu-id="7fef7-125">Interact with your bot</span></span>

<span data-ttu-id="7fef7-126">Envíe un mensaje al bot y este responderá con un mensaje.</span><span class="sxs-lookup"><span data-stu-id="7fef7-126">Send a message to your bot, and the bot will respond back with a message.</span></span>
<span data-ttu-id="7fef7-127">![Emulador en ejecución](../media/emulator-v4/emulator-running.png)</span><span class="sxs-lookup"><span data-stu-id="7fef7-127">![Emulator running](../media/emulator-v4/emulator-running.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="7fef7-128">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="7fef7-128">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="7fef7-129">Funcionamiento de los bots</span><span class="sxs-lookup"><span data-stu-id="7fef7-129">How bots work</span></span>](../v4sdk/bot-builder-basics.md) 
