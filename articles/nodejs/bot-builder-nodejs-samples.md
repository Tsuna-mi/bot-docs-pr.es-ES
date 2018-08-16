---
title: Bots de ejemplo para Bot Builder SDK para Node.js | Microsoft Docs
description: Explore una amplia selección de bots de ejemplo que pueden ayudarle a comenzar a desarrollar bots con Bot Builder SDK para Node.js.
author: v-ducvo
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: 0eb14f4bf52a293d3405762c786259f771aecad6
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305720"
---
# <a name="bot-builder-sdk-for-nodejs-samples"></a><span data-ttu-id="dc17a-103">Ejemplos de Bot Builder SDK para Node.js</span><span class="sxs-lookup"><span data-stu-id="dc17a-103">Bot Builder SDK for Node.js samples</span></span>

<span data-ttu-id="dc17a-104">En estos ejemplos se describen bots basados en tareas en los que se muestra cómo aprovechar las características de Bot Builder SDK para Node.js.</span><span class="sxs-lookup"><span data-stu-id="dc17a-104">These samples demonstrate task-focused bots that show how to take advantage of features in the Bot Builder SDK for Node.js.</span></span> <span data-ttu-id="dc17a-105">Puede usar los ejemplos para empezar a trabajar rápidamente en la compilación de bots excelentes con funcionalidades enriquecidas.</span><span class="sxs-lookup"><span data-stu-id="dc17a-105">You can use the samples to help you quickly get started with building great bots with rich capabilities.</span></span>

## <a name="get-the-samples"></a><span data-ttu-id="dc17a-106">Obtención de los ejemplos</span><span class="sxs-lookup"><span data-stu-id="dc17a-106">Get the samples</span></span>
<span data-ttu-id="dc17a-107">Para obtener los ejemplos, clone el repositorio [BotBuilder-Samples](https://github.com/Microsoft/BotBuilder-Samples) de GitHub mediante Git.</span><span class="sxs-lookup"><span data-stu-id="dc17a-107">To get the samples, clone the [BotBuilder-Samples](https://github.com/Microsoft/BotBuilder-Samples) GitHub repository using Git.</span></span>

```cmd
git clone https://github.com/Microsoft/BotBuilder-Samples.git
cd BotBuilder-Samples
```

<span data-ttu-id="dc17a-108">Los bots de ejemplo creados con Bot Builder SDK para Node.js se organizan en el directorio **Node**.</span><span class="sxs-lookup"><span data-stu-id="dc17a-108">The sample bots built with the Bot Builder SDK for Node.js are organized in the **Node** directory.</span></span>

<span data-ttu-id="dc17a-109">También puede ver los ejemplos en GitHub e implementarlos directamente en Azure.</span><span class="sxs-lookup"><span data-stu-id="dc17a-109">You can also view the samples on GitHub and deploy them to Azure directly.</span></span>

## <a name="core"></a><span data-ttu-id="dc17a-110">Núcleo</span><span class="sxs-lookup"><span data-stu-id="dc17a-110">Core</span></span>
<span data-ttu-id="dc17a-111">En estos ejemplos se muestran las técnicas básicas para la creación de bots enriquecidos y completos.</span><span class="sxs-lookup"><span data-stu-id="dc17a-111">These samples show the basic techniques for building rich and capable bots.</span></span>

<span data-ttu-id="dc17a-112">Muestra</span><span class="sxs-lookup"><span data-stu-id="dc17a-112">Sample</span></span> | <span data-ttu-id="dc17a-113">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="dc17a-113">Description</span></span>
------------ | ------------- 
[<span data-ttu-id="dc17a-114">Send Attachment</span><span class="sxs-lookup"><span data-stu-id="dc17a-114">Send Attachment</span></span>](https://github.com/Microsoft/BotBuilder-Samples/tree/master/Node/core-SendAttachment) | <span data-ttu-id="dc17a-115">Bot de ejemplo que envía al usuario datos adjuntos de elementos multimedia sencillos (imágenes).</span><span class="sxs-lookup"><span data-stu-id="dc17a-115">A sample bot that sends simple media attachments (images) to the user.</span></span> 
[<span data-ttu-id="dc17a-116">Receive Attachment</span><span class="sxs-lookup"><span data-stu-id="dc17a-116">Receive Attachment</span></span>](https://github.com/Microsoft/BotBuilder-Samples/tree/master/Node/core-ReceiveAttachment) | <span data-ttu-id="dc17a-117">Bot de ejemplo que recibe los datos adjuntos enviados por el usuario y los descarga.</span><span class="sxs-lookup"><span data-stu-id="dc17a-117">A sample bot that receives attachments sent by the user and downloads them.</span></span> 
[<span data-ttu-id="dc17a-118">Create New Conversation</span><span class="sxs-lookup"><span data-stu-id="dc17a-118">Create New Conversation</span></span>](https://github.com/Microsoft/BotBuilder-Samples/tree/master/Node/core-CreateNewConversation)  | <span data-ttu-id="dc17a-119">Bot de ejemplo que inicia una conversación nueva usando una dirección de usuario almacenada previamente.</span><span class="sxs-lookup"><span data-stu-id="dc17a-119">A sample bot that starts a new conversation using a previously stored user address.</span></span>
[<span data-ttu-id="dc17a-120">Get Members of a Conversation</span><span class="sxs-lookup"><span data-stu-id="dc17a-120">Get Members of a Conversation</span></span>](https://github.com/Microsoft/BotBuilder-Samples/tree/master/Node/core-GetConversationMembers) | <span data-ttu-id="dc17a-121">Bot de ejemplo que recupera la lista de miembros de la conversación y detecta cuando cambia.</span><span class="sxs-lookup"><span data-stu-id="dc17a-121">A sample bot that retrieves the conversation's members list and detects when it changes.</span></span> 
[<span data-ttu-id="dc17a-122">Direct Line</span><span class="sxs-lookup"><span data-stu-id="dc17a-122">Direct Line</span></span>](https://github.com/Microsoft/BotBuilder-Samples/tree/master/Node/core-DirectLine) | <span data-ttu-id="dc17a-123">Bot de ejemplo y cliente personalizado que se comunican entre sí usando Direct Line API.</span><span class="sxs-lookup"><span data-stu-id="dc17a-123">A sample bot and a custom client that communicate with each other using the Direct Line API.</span></span> 
[<span data-ttu-id="dc17a-124">Direct Line (WebSockets)</span><span class="sxs-lookup"><span data-stu-id="dc17a-124">Direct Line (WebSockets)</span></span>](https://github.com/Microsoft/BotBuilder-Samples/tree/master/Node/core-DirectLineWebSockets) | <span data-ttu-id="dc17a-125">Bot de ejemplo y cliente personalizado que se comunican entre sí mediante Direct Line API + WebSockets.</span><span class="sxs-lookup"><span data-stu-id="dc17a-125">A sample bot and a custom client that communicate with each other using the Direct Line API + WebSockets.</span></span> 
[<span data-ttu-id="dc17a-126">Multi Dialogs</span><span class="sxs-lookup"><span data-stu-id="dc17a-126">Multi Dialogs</span></span>](https://github.com/Microsoft/BotBuilder-Samples/tree/master/Node/core-MultiDialogs) | <span data-ttu-id="dc17a-127">Bot de ejemplo que muestra varios tipos de cuadros de diálogo.</span><span class="sxs-lookup"><span data-stu-id="dc17a-127">A sample bot that shows various kinds of dialogs.</span></span>
[<span data-ttu-id="dc17a-128">State API</span><span class="sxs-lookup"><span data-stu-id="dc17a-128">State API</span></span>](https://github.com/Microsoft/BotBuilder-Samples/tree/master/Node/core-State) | <span data-ttu-id="dc17a-129">Bot de ejemplo sin estado que realiza el seguimiento del contexto de una conversación.</span><span class="sxs-lookup"><span data-stu-id="dc17a-129">A stateless sample bot that tracks the context of a conversation.</span></span>
[<span data-ttu-id="dc17a-130">Custom State API</span><span class="sxs-lookup"><span data-stu-id="dc17a-130">Custom State API</span></span>](https://github.com/Microsoft/BotBuilder-Samples/tree/master/Node/core-CustomState) | <span data-ttu-id="dc17a-131">Bot de ejemplo sin estado que realiza el seguimiento del contexto de una conversación mediante un proveedor de almacenamiento personalizado.</span><span class="sxs-lookup"><span data-stu-id="dc17a-131">A stateless sample bot that tracks the context of a conversation using a custom storage provider.</span></span>
[<span data-ttu-id="dc17a-132">ChannelData</span><span class="sxs-lookup"><span data-stu-id="dc17a-132">ChannelData</span></span>](https://github.com/Microsoft/BotBuilder-Samples/tree/master/Node/core-ChannelData) | <span data-ttu-id="dc17a-133">Bot de ejemplo que envía metadatos nativos a Facebook mediante ChannelData.</span><span class="sxs-lookup"><span data-stu-id="dc17a-133">A sample bot that sends native metadata to Facebook using ChannelData.</span></span>
[<span data-ttu-id="dc17a-134">AppInsights</span><span class="sxs-lookup"><span data-stu-id="dc17a-134">AppInsights</span></span>](https://github.com/Microsoft/BotBuilder-Samples/tree/master/Node/core-AppInsights) | <span data-ttu-id="dc17a-135">Bot de ejemplo que registra datos de telemetría en una instancia de Application Insights.</span><span class="sxs-lookup"><span data-stu-id="dc17a-135">A sample bot that logs telemetry to an Application Insights instance.</span></span>

## <a name="search"></a><span data-ttu-id="dc17a-136">Search</span><span class="sxs-lookup"><span data-stu-id="dc17a-136">Search</span></span>
<span data-ttu-id="dc17a-137">En este ejemplo se muestra cómo aprovechar Azure Search en bots controlados por datos.</span><span class="sxs-lookup"><span data-stu-id="dc17a-137">This sample shows how to leverage Azure Search in data-driven bots.</span></span>

<span data-ttu-id="dc17a-138">Muestra</span><span class="sxs-lookup"><span data-stu-id="dc17a-138">Sample</span></span> | <span data-ttu-id="dc17a-139">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="dc17a-139">Description</span></span>
------------ | -------------
[<span data-ttu-id="dc17a-140">Azure Search</span><span class="sxs-lookup"><span data-stu-id="dc17a-140">Azure Search</span></span>](https://github.com/Microsoft/BotBuilder-Samples/tree/master/Node/demo-Search) | <span data-ttu-id="dc17a-141">Dos bots de ejemplo que ayudan al usuario a navegar por grandes cantidades de contenido.</span><span class="sxs-lookup"><span data-stu-id="dc17a-141">Two sample bots that help the user navigate large amounts of content.</span></span>


## <a name="cards"></a><span data-ttu-id="dc17a-142">Cards</span><span class="sxs-lookup"><span data-stu-id="dc17a-142">Cards</span></span>
<span data-ttu-id="dc17a-143">En estos ejemplos se muestra cómo enviar tarjetas enriquecidas en Bot Framework.</span><span class="sxs-lookup"><span data-stu-id="dc17a-143">These samples show how to send rich cards in the Bot Framework.</span></span>

<span data-ttu-id="dc17a-144">Muestra</span><span class="sxs-lookup"><span data-stu-id="dc17a-144">Sample</span></span> | <span data-ttu-id="dc17a-145">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="dc17a-145">Description</span></span>
------------ | -------------
[<span data-ttu-id="dc17a-146">Rich Cards</span><span class="sxs-lookup"><span data-stu-id="dc17a-146">Rich Cards</span></span>](https://github.com/Microsoft/BotBuilder-Samples/tree/master/Node/cards-RichCards) | <span data-ttu-id="dc17a-147">Bot de ejemplo que envía varios tipos de tarjetas enriquecidas.</span><span class="sxs-lookup"><span data-stu-id="dc17a-147">A sample bot that sends several different types of rich cards.</span></span>
[<span data-ttu-id="dc17a-148">Carousel of Cards</span><span class="sxs-lookup"><span data-stu-id="dc17a-148">Carousel of Cards</span></span>](https://github.com/Microsoft/BotBuilder-Samples/tree/master/Node/cards-CarouselCards) | <span data-ttu-id="dc17a-149">Bot de ejemplo que envía varias tarjetas enriquecidas dentro de un solo mensaje con el diseño de carrusel.</span><span class="sxs-lookup"><span data-stu-id="dc17a-149">A sample bot that sends multiple rich cards within a single message using the Carousel layout.</span></span>

## <a name="intelligence"></a><span data-ttu-id="dc17a-150">Inteligencia</span><span class="sxs-lookup"><span data-stu-id="dc17a-150">Intelligence</span></span>
<span data-ttu-id="dc17a-151">En estos ejemplos se muestra cómo agregar funcionalidades de inteligencia artificial a un bot con las API de Bing y Microsoft Cognitive Services.</span><span class="sxs-lookup"><span data-stu-id="dc17a-151">These samples show how to add artificial intelligence capabilities to a bot using Bing and Microsoft Cognitive Services APIs.</span></span>

<span data-ttu-id="dc17a-152">Muestra</span><span class="sxs-lookup"><span data-stu-id="dc17a-152">Sample</span></span> | <span data-ttu-id="dc17a-153">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="dc17a-153">Description</span></span>
------------ | -------------
[<span data-ttu-id="dc17a-154">LUIS</span><span class="sxs-lookup"><span data-stu-id="dc17a-154">LUIS</span></span>](https://github.com/Microsoft/BotBuilder-Samples/tree/master/Node/intelligence-LUIS) | <span data-ttu-id="dc17a-155">Bot de ejemplo que usa LuisDialog para integrarse con una aplicación LUIS.ai.</span><span class="sxs-lookup"><span data-stu-id="dc17a-155">A sample bot that uses LuisDialog to integrate with a LUIS.ai application.</span></span>
[<span data-ttu-id="dc17a-156">Image Caption</span><span class="sxs-lookup"><span data-stu-id="dc17a-156">Image Caption</span></span>](https://github.com/Microsoft/BotBuilder-Samples/tree/master/Node/intelligence-ImageCaption) | <span data-ttu-id="dc17a-157">Bot de ejemplo que obtiene un título de imagen mediante Vision API de Microsoft Cognitive Services.</span><span class="sxs-lookup"><span data-stu-id="dc17a-157">A sample bot that gets an image caption using the Microsoft Cognitive Services Vision API.</span></span>
[<span data-ttu-id="dc17a-158">Speech To Text</span><span class="sxs-lookup"><span data-stu-id="dc17a-158">Speech To Text</span></span>](https://github.com/Microsoft/BotBuilder-Samples/tree/master/Node/intelligence-SpeechToText)  | <span data-ttu-id="dc17a-159">Bot de ejemplo que obtiene texto de audio mediante Bing Speech API.</span><span class="sxs-lookup"><span data-stu-id="dc17a-159">A sample bot that gets text from audio using the Bing Speech API.</span></span>
[<span data-ttu-id="dc17a-160">Similar Products</span><span class="sxs-lookup"><span data-stu-id="dc17a-160">Similar Products</span></span>](https://github.com/Microsoft/BotBuilder-Samples/tree/master/Node/intelligence-SimilarProducts) | <span data-ttu-id="dc17a-161">Bot de ejemplo que busca productos visualmente similares con Bing Image Search API.</span><span class="sxs-lookup"><span data-stu-id="dc17a-161">A sample bot that finds visually similar products using the Bing Image Search API.</span></span> 
[<span data-ttu-id="dc17a-162">Zummer</span><span class="sxs-lookup"><span data-stu-id="dc17a-162">Zummer</span></span>](https://github.com/Microsoft/BotBuilder-Samples/tree/master/Node/intelligence-Zummer) | <span data-ttu-id="dc17a-163">Bot de ejemplo que busca artículos de Wikipedia mediante Bing Search API.</span><span class="sxs-lookup"><span data-stu-id="dc17a-163">A sample bot that finds wikipedia articles using the Bing Search API.</span></span>

## <a name="reference-implementation"></a><span data-ttu-id="dc17a-164">Implementación de referencia</span><span class="sxs-lookup"><span data-stu-id="dc17a-164">Reference implementation</span></span>
<span data-ttu-id="dc17a-165">Este ejemplo está diseñado para presentar un escenario de principio a fin.</span><span class="sxs-lookup"><span data-stu-id="dc17a-165">This sample is designed to showcase an end-to-end scenario.</span></span> <span data-ttu-id="dc17a-166">Es una fuente excelente de fragmentos de código si está pensando en implementar características más complejas en el bot.</span><span class="sxs-lookup"><span data-stu-id="dc17a-166">It's a great source of code fragments if you're looking to implement more complex features in your bot.</span></span>


<span data-ttu-id="dc17a-167">Muestra</span><span class="sxs-lookup"><span data-stu-id="dc17a-167">Sample</span></span> | <span data-ttu-id="dc17a-168">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="dc17a-168">Description</span></span>
------------ | -------------
[<span data-ttu-id="dc17a-169">Contoso Flowers</span><span class="sxs-lookup"><span data-stu-id="dc17a-169">Contoso Flowers</span></span>](https://github.com/Microsoft/BotBuilder-Samples/tree/master/Node/demo-ContosoFlowers) | <span data-ttu-id="dc17a-170">Bot de ejemplo que usa numerosas características de Bot Framework.</span><span class="sxs-lookup"><span data-stu-id="dc17a-170">A sample bot that uses many features of the Bot Framework.</span></span>

