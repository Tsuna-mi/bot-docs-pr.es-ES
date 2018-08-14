---
title: Bot Builder SDK para .NET | Microsoft Docs
description: Empieza a trabajar con el Bot Builder SDK para. NET, un marco eficaz y fácil de usar para la creación de bots.
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: 9a2103247f0637246c4d6540981a38b9fb4528b2
ms.sourcegitcommit: 67445b42796d90661afc643c6bb6533e9a662cbc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2018
ms.locfileid: "39574991"
---
# <a name="bot-builder-sdk-for-net"></a><span data-ttu-id="3de8a-103">SDK de Bot Builder para .NET</span><span class="sxs-lookup"><span data-stu-id="3de8a-103">Bot Builder SDK for .NET</span></span>

[!INCLUDE [pre-release-label](../includes/pre-release-label-v3.md)]

> [!div class="op_single_selector"]
> - [.NET](../dotnet/bot-builder-dotnet-overview.md)
> - [Node.js](../nodejs/bot-builder-nodejs-overview.md)
> - [REST](../rest-api/bot-framework-rest-overview.md)

<span data-ttu-id="3de8a-107">El Bot Builder SDK para .NET es un marco eficaz para crear bots que pueden controlar tanto interacciones de forma libre como conversaciones más guiadas, donde el usuario selecciona entre valores posibles.</span><span class="sxs-lookup"><span data-stu-id="3de8a-107">The Bot Builder SDK for .NET is a powerful framework for constructing bots that can handle both free-form interactions and more guided conversations where the user selects from possible values.</span></span> <span data-ttu-id="3de8a-108">Es fácil de usar y aprovecha C# para proporcionar una manera conocida de escribir bots para los desarrolladores de .NET.</span><span class="sxs-lookup"><span data-stu-id="3de8a-108">It is easy to use and leverages C# to provide a familiar way for .NET developers to write bots.</span></span>

<span data-ttu-id="3de8a-109">Mediante el SDK, puede crear bots que aprovechen las características siguientes:</span><span class="sxs-lookup"><span data-stu-id="3de8a-109">Using the SDK, you can build bots that take advantage of the following SDK features:</span></span> 

- <span data-ttu-id="3de8a-110">Sistema eficaz de cuadros de diálogo con cuadros que están aislados y que admiten composición</span><span class="sxs-lookup"><span data-stu-id="3de8a-110">Powerful dialog system with dialogs that are isolated and composable</span></span>
- <span data-ttu-id="3de8a-111">Avisos integrados para cosas sencillas, como Sí/No, cadenas, números y enumeraciones</span><span class="sxs-lookup"><span data-stu-id="3de8a-111">Built-in prompts for simple things such as Yes/No, strings, numbers, and enumerations</span></span>
- <span data-ttu-id="3de8a-112">Cuadros de diálogo integrados que usan marcos de inteligencia artificial eficaces, como <a href="http://luis.ai" target="_blank">LUIS</a></span><span class="sxs-lookup"><span data-stu-id="3de8a-112">Built-in dialogs that utilize powerful AI frameworks such as <a href="http://luis.ai" target="_blank">LUIS</a></span></span>
- <span data-ttu-id="3de8a-113">FormFlow para generar un bot automáticamente (desde una clase de C#), que guía al usuario a través de la conversación, brindando ayuda, navegación, aclaraciones y confirmaciones según sea necesario</span><span class="sxs-lookup"><span data-stu-id="3de8a-113">FormFlow for automatically generating a bot (from a C# class) that guides the user through the conversation, providing help, navigation, clarification, and confirmation as needed</span></span>

> [!IMPORTANT]
> El 31 de julio de 2017 se implementaron cambios importantes en el protocolo de seguridad de Bot Framework. Para evitar que estos cambios afecten negativamente a su bot, debe asegurarse de que la aplicación use el Bot Builder SDK v3.5 o posterior. Si ha creado un bot mediante un SDK que obtuvo antes del 5 de enero de 2017 (la fecha de lanzamiento del Bot Builder SDK 3.5), asegúrese de actualizar a Bot Builder SDK v3.5 o posterior.

## <a name="get-the-sdk"></a><span data-ttu-id="3de8a-117">Obtención del SDK</span><span class="sxs-lookup"><span data-stu-id="3de8a-117">Get the SDK</span></span>

<span data-ttu-id="3de8a-118">El SDK está disponible como paquete de NuGet y como código abierto en <a href="https://github.com/Microsoft/BotBuilder" target="_blank">GitHub</a>.</span><span class="sxs-lookup"><span data-stu-id="3de8a-118">The SDK is available as a NuGet package and as open source on <a href="https://github.com/Microsoft/BotBuilder" target="_blank">GitHub</a>.</span></span>

> [!IMPORTANT]
> El Bot Builder SDK para .NET requiere .NET Framework 4.6 o posterior. Si va a agregar el SDK a un proyecto existente que tiene como destino una versión anterior de .NET Framework, deberá actualizar primero el proyecto que tienen como destino .NET Framework 4.6.

<span data-ttu-id="3de8a-121">Para instalar el SDK dentro de un proyecto de Visual Studio, complete los pasos siguientes:</span><span class="sxs-lookup"><span data-stu-id="3de8a-121">To install the SDK within a Visual Studio project, complete the following steps:</span></span>

1. <span data-ttu-id="3de8a-122">En el **Explorador de soluciones**, haga clic con el botón derecho en el nombre del proyecto y seleccione **Administrar paquetes NuGet…**.</span><span class="sxs-lookup"><span data-stu-id="3de8a-122">In **Solution Explorer**, right-click the project name and select **Manage NuGet Packages...**.</span></span>
2. <span data-ttu-id="3de8a-123">En la pestaña **Examinar**, escriba "Microsoft.Bot.Builder" en el cuadro de búsqueda.</span><span class="sxs-lookup"><span data-stu-id="3de8a-123">On the **Browse** tab, type "Microsoft.Bot.Builder" into the search box.</span></span>
3. <span data-ttu-id="3de8a-124">Seleccione **Microsoft.Bot.Builder** en la lista de resultados, haga clic en **Instalar** y acepte los cambios.</span><span class="sxs-lookup"><span data-stu-id="3de8a-124">Select **Microsoft.Bot.Builder** in the list of results, click **Install**, and accept the changes.</span></span>

## <a name="get-code-samples"></a><span data-ttu-id="3de8a-125">Obtención de ejemplos de código</span><span class="sxs-lookup"><span data-stu-id="3de8a-125">Get code samples</span></span>

<span data-ttu-id="3de8a-126">Este SDK incluye [código fuente de ejemplo](bot-builder-dotnet-samples.md) que usa las características del Bot Builder SDK para. NET.</span><span class="sxs-lookup"><span data-stu-id="3de8a-126">This SDK includes [sample source code](bot-builder-dotnet-samples.md) that uses the features of the Bot Builder SDK for .NET.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3de8a-127">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="3de8a-127">Next steps</span></span>

<span data-ttu-id="3de8a-128">Para obtener más información sobre la creación de bots con el Bot Builder SDK para .NET, revise los artículos a lo largo de esta sección, que comienzan con:</span><span class="sxs-lookup"><span data-stu-id="3de8a-128">Learn more about building bots using the Bot Builder SDK for .NET by reviewing articles throughout this section, beginning with:</span></span>

- <span data-ttu-id="3de8a-129">[Guía de inicio rápido](bot-builder-dotnet-quickstart.md): compile y pruebe rápidamente un bot simple siguiendo las instrucciones de este tutorial paso a paso.</span><span class="sxs-lookup"><span data-stu-id="3de8a-129">[Quickstart](bot-builder-dotnet-quickstart.md): Quickly build and test a simple bot by following instructions in this step-by-step tutorial.</span></span>
- <span data-ttu-id="3de8a-130">[Conceptos clave](bot-builder-dotnet-concepts.md): aprenda sobre los conceptos clave del Bot Builder SDK para .NET.</span><span class="sxs-lookup"><span data-stu-id="3de8a-130">[Key concepts](bot-builder-dotnet-concepts.md): Learn about key concepts in the Bot Builder SDK for .NET.</span></span>

<span data-ttu-id="3de8a-131">Si tiene problemas o sugerencias sobre el Bot Builder SDK para .NET, consulte el [soporte técnico](../bot-service-resources-links-help.md) para obtener una lista de los recursos disponibles.</span><span class="sxs-lookup"><span data-stu-id="3de8a-131">If you encounter problems or have suggestions regarding the Bot Builder SDK for .NET, see [Support](../bot-service-resources-links-help.md) for a list of available resources.</span></span> 
