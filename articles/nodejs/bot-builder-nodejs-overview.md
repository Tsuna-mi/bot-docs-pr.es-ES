---
title: Bot Builder SDK para Node.js | Microsoft Docs
description: Explore el Bot Builder SDK para Node.js, un marco de creación de bots eficaz y fácil de usar.
author: v-ducvo
ms.author: v-ducvo
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: 24da2cc90908a1f22bee9d2cd007607b116ac2cc
ms.sourcegitcommit: 2dc75701b169d822c9499e393439161bc87639d2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42904208"
---
# <a name="bot-builder-sdk-for-nodejs"></a><span data-ttu-id="1e812-103">Bot Builder SDK para Node.js</span><span class="sxs-lookup"><span data-stu-id="1e812-103">Bot Builder SDK for Node.js</span></span>

[!INCLUDE [pre-release-label](../includes/pre-release-label-v3.md)]

> [!div class="op_single_selector"]
> - [.NET](../dotnet/bot-builder-dotnet-overview.md)
> - [Node.js](../nodejs/bot-builder-nodejs-overview.md)
> - [REST](../rest-api/bot-framework-rest-overview.md)

<span data-ttu-id="1e812-107">Bot Builder SDK para Node.js es un marco eficaz y fácil de usar que proporciona una manera conocida para que los desarrolladores de Node.js escriban bots.</span><span class="sxs-lookup"><span data-stu-id="1e812-107">Bot Builder SDK for Node.js is a powerful, easy-to-use framework that provides a familiar way for Node.js developers to write bots.</span></span>
<span data-ttu-id="1e812-108">Puede usarlo para crear una amplia variedad de interfaces de usuario conversacionales, desde avisos sencillos hasta conversaciones de forma libre.</span><span class="sxs-lookup"><span data-stu-id="1e812-108">You can use it to build a wide variety of conversational user interfaces, from simple prompts to free-form conversations.</span></span>

<span data-ttu-id="1e812-109">La lógica conversacional para el bot se aloja como servicio web.</span><span class="sxs-lookup"><span data-stu-id="1e812-109">The conversational logic for your bot is hosted as a web service.</span></span> <span data-ttu-id="1e812-110">El Bot Builder SDK usa <a href="http://restify.com">restify</a>, un marco conocido para la creación de servicios web, para crear el servidor web del bot.</span><span class="sxs-lookup"><span data-stu-id="1e812-110">The Bot Builder SDK uses <a href="http://restify.com">restify</a>, a popular framework for building web services, to create the bot's web server.</span></span> <span data-ttu-id="1e812-111">El SDK también es compatible con <a href="http://expressjs.com/">Express</a>, y el uso de otros marcos de aplicación web es posible con algunas adaptaciones.</span><span class="sxs-lookup"><span data-stu-id="1e812-111">The SDK is also compatible with <a href="http://expressjs.com/">Express</a> and the use of other web app frameworks is possible with some adaption.</span></span> 

<span data-ttu-id="1e812-112">Mediante el SDK, puede sacar provecho de las características siguientes:</span><span class="sxs-lookup"><span data-stu-id="1e812-112">Using the SDK, you can take advantage of the following SDK features:</span></span> 

- <span data-ttu-id="1e812-113">Sistema eficaz para crear cuadros de diálogo para encapsular la lógica conversacional.</span><span class="sxs-lookup"><span data-stu-id="1e812-113">Powerful system for building dialogs to encapsulate conversational logic.</span></span>
- <span data-ttu-id="1e812-114">Avisos integrados para cosas sencillas, como Sí/No, cadenas, números y enumeraciones, así como compatibilidad con mensajes que contienen imágenes y adjuntos, y tarjetas enriquecidas que contiene botones.</span><span class="sxs-lookup"><span data-stu-id="1e812-114">Built-in prompts for simple things such as Yes/No, strings, numbers, and enumerations, as well as support for messages containing images and attachments, and rich cards containing buttons.</span></span>
- <span data-ttu-id="1e812-115">Compatibilidad integrada con plataformas de inteligencia artificial como eficaces, como <a href="http://luis.ai" target="_blank">LUIS</a>.</span><span class="sxs-lookup"><span data-stu-id="1e812-115">Built-in support for powerful AI frameworks such as <a href="http://luis.ai" target="_blank">LUIS</a>.</span></span>
- <span data-ttu-id="1e812-116">Reconocedores integrados y controladores de eventos que guían al usuario a través de la conversación, brindando ayuda, navegación, aclaraciones y confirmaciones según sea necesario.</span><span class="sxs-lookup"><span data-stu-id="1e812-116">Built-in recognizers and event handlers that guide the user through the conversation, providing help, navigation, clarification, and confirmation as needed.</span></span>

## <a name="get-started"></a><span data-ttu-id="1e812-117">Introducción</span><span class="sxs-lookup"><span data-stu-id="1e812-117">Get started</span></span>

<span data-ttu-id="1e812-118">Si no está familiarizado con la escritura de bots, [cree su primer bot con Node.js](bot-builder-nodejs-quickstart.md) con instrucciones paso a paso que le ayudarán a configurar el proyecto, instalar el SDK y ejecutar su primer bot.</span><span class="sxs-lookup"><span data-stu-id="1e812-118">If you are new to writing bots, [create your first bot with Node.js](bot-builder-nodejs-quickstart.md) with step-by-step instructions to help you set up your project, install the SDK, and run your first bot.</span></span> 

<span data-ttu-id="1e812-119">Si no está familiarizado con el Bot Builder SDK para Node.js, puede comenzar con los conceptos clave que le ayudarán a comprender los componentes principales del Bot Builder SDK, consulte [Conceptos clave](bot-builder-nodejs-concepts.md).</span><span class="sxs-lookup"><span data-stu-id="1e812-119">If you are new to the Bot Builder SDK for Node.js, you can start with key concepts that help you understand the major components of the Bot Builder SDK, see [Key concepts](bot-builder-nodejs-concepts.md).</span></span>

<span data-ttu-id="1e812-120">Para asegurarse de que el bot aborde los escenarios de usuario principales, revise los [principios de diseño](../bot-service-design-principles.md) y [explore los patrones](../bot-service-design-pattern-task-automation.md) para obtener instrucciones.</span><span class="sxs-lookup"><span data-stu-id="1e812-120">To ensure your bot addresses the top user scenarios, review the [design principles](../bot-service-design-principles.md) and [explore patterns](../bot-service-design-pattern-task-automation.md) for guidance.</span></span>

## <a name="get-samples"></a><span data-ttu-id="1e812-121">Obtención de ejemplos</span><span class="sxs-lookup"><span data-stu-id="1e812-121">Get samples</span></span>

<span data-ttu-id="1e812-122">En los [ejemplos del Bot Builder SDK para Node.js](bot-builder-nodejs-samples.md) se describen bots basados en tareas en los que se muestra cómo aprovechar las características del Bot Builder SDK para Node.js.</span><span class="sxs-lookup"><span data-stu-id="1e812-122">The [Bot Builder SDK for Node.js samples](bot-builder-nodejs-samples.md) demonstrate task-focused bots that show how to take advantage of features in the Bot Builder SDK for Node.js.</span></span> <span data-ttu-id="1e812-123">Puede usar los ejemplos para empezar a trabajar rápidamente en la compilación de bots excelentes con funcionalidades enriquecidas.</span><span class="sxs-lookup"><span data-stu-id="1e812-123">You can use the samples to help you quickly get started with building great bots with rich capabilities.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1e812-124">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="1e812-124">Next steps</span></span>
> [!div class="nextstepaction"]
> [Conceptos clave](bot-builder-nodejs-concepts.md)

## <a name="additional-resources"></a><span data-ttu-id="1e812-126">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="1e812-126">Additional resources</span></span>

<span data-ttu-id="1e812-127">Las siguientes guías de procedimientos enfocada en tareas demuestran diversas características del Bot Builder SDK para Node.js.</span><span class="sxs-lookup"><span data-stu-id="1e812-127">The following task-focused how-to guides demonstrate various features of the Bot Builder SDK for Node.js.</span></span>

* [<span data-ttu-id="1e812-128">Responder a mensajes</span><span class="sxs-lookup"><span data-stu-id="1e812-128">Respond to messages</span></span>](bot-builder-nodejs-use-default-message-handler.md)
* [<span data-ttu-id="1e812-129">Control de acciones del usuario</span><span class="sxs-lookup"><span data-stu-id="1e812-129">Handle user actions</span></span>](bot-builder-nodejs-dialog-actions.md)
* [<span data-ttu-id="1e812-130">Reconocimiento de intenciones del usuario</span><span class="sxs-lookup"><span data-stu-id="1e812-130">Recognize user intent</span></span>](bot-builder-nodejs-recognize-intent-messages.md)
* [<span data-ttu-id="1e812-131">Envío de una tarjeta enriquecida</span><span class="sxs-lookup"><span data-stu-id="1e812-131">Send a rich card</span></span>](bot-builder-nodejs-send-rich-cards.md)
* [<span data-ttu-id="1e812-132">Envío de archivos adjuntos</span><span class="sxs-lookup"><span data-stu-id="1e812-132">Send attachments</span></span>](bot-builder-nodejs-send-receive-attachments.md)
* [<span data-ttu-id="1e812-133">Guardado de datos de usuario</span><span class="sxs-lookup"><span data-stu-id="1e812-133">Saving user data</span></span>](bot-builder-nodejs-save-user-data.md)


<span data-ttu-id="1e812-134">Si tiene problemas o sugerencias sobre el Bot Builder SDK para Node.js, consulte el [soporte técnico](../bot-service-resources-links-help.md) para obtener una lista de los recursos disponibles.</span><span class="sxs-lookup"><span data-stu-id="1e812-134">If you encounter problems or have suggestions regarding the Bot Builder SDK for Node.js, see [Support](../bot-service-resources-links-help.md) for a list of available resources.</span></span> 


[DesignGuide]: ../bot-service-design-principles.md 
[DesignPatterns]: ../bot-service-design-pattern-task-automation.md 
[HowTo]: bot-builder-nodejs-use-default-message-handler.md 
