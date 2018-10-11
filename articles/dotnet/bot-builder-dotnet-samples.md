---
title: Bots de ejemplo para el SDK de Bot Builder para .NET | Microsoft Docs
description: Explore los bots de ejemplo que pueden ayudar a comenzar el desarrollo de bots con el SDK de Bot Builder para .NET.
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 09/13/2018
ms.openlocfilehash: 9f80fc551cac2f1994b0d398cd640d2ad3d78e61
ms.sourcegitcommit: 3bf3dbb1a440b3d83e58499c6a2ac116fe04b2f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/23/2018
ms.locfileid: "46707381"
---
# <a name="bot-builder-sdk-for-net-samples"></a><span data-ttu-id="01455-103">Ejemplos del SDK de Bot Builder para .NET</span><span class="sxs-lookup"><span data-stu-id="01455-103">Bot Builder SDK for .NET samples</span></span>

::: moniker range="azure-bot-service-3.0"

[!INCLUDE [pre-release-label](../includes/pre-release-label-v3.md)]

<span data-ttu-id="01455-104">En estos ejemplos se describen bots basados en tareas en los que se muestra cómo aprovechar las características del SDK de Bot Builder para .NET.</span><span class="sxs-lookup"><span data-stu-id="01455-104">These samples demonstrate task-focused bots that show how to take advantage of features in the Bot Builder SDK for .NET.</span></span> <span data-ttu-id="01455-105">Puede usar los ejemplos para empezar a trabajar rápidamente en la compilación de bots excelentes con funcionalidades enriquecidas.</span><span class="sxs-lookup"><span data-stu-id="01455-105">You can use the samples to help you quickly get started with building great bots with rich capabilities.</span></span>

## <a name="get-the-samples"></a><span data-ttu-id="01455-106">Obtención de los ejemplos</span><span class="sxs-lookup"><span data-stu-id="01455-106">Get the samples</span></span>
<span data-ttu-id="01455-107">Para obtener los ejemplos, clone el repositorio [BotBuilder-Samples](https://github.com/Microsoft/BotBuilder-Samples) de GitHub mediante Git.</span><span class="sxs-lookup"><span data-stu-id="01455-107">To get the samples, clone the [BotBuilder-Samples](https://github.com/Microsoft/BotBuilder-Samples) GitHub repository using Git.</span></span>

```cmd
git clone https://github.com/Microsoft/BotBuilder-Samples.git
cd BotBuilder-Samples
```

<span data-ttu-id="01455-108">Los bots de ejemplo creados con el SDK de Bot Builder para .NET se organizan en el directorio **CSharp**.</span><span class="sxs-lookup"><span data-stu-id="01455-108">The sample bots built with the Bot Builder SDK for .NET are organized in the **CSharp** directory.</span></span>

<span data-ttu-id="01455-109">También puede ver los ejemplos en GitHub e implementarlos directamente en Azure.</span><span class="sxs-lookup"><span data-stu-id="01455-109">You can also view the samples on GitHub and deploy them to Azure directly.</span></span>

## <a name="core"></a><span data-ttu-id="01455-110">Núcleo</span><span class="sxs-lookup"><span data-stu-id="01455-110">Core</span></span>
<span data-ttu-id="01455-111">En estos ejemplos se muestran las técnicas básicas para la creación de bots enriquecidos y completos.</span><span class="sxs-lookup"><span data-stu-id="01455-111">These samples show the basic techniques for building rich and capable bots.</span></span>

<span data-ttu-id="01455-112">Muestra</span><span class="sxs-lookup"><span data-stu-id="01455-112">Sample</span></span> | <span data-ttu-id="01455-113">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="01455-113">Description</span></span>
------------ | ------------- 
[<span data-ttu-id="01455-114">Send Attachment</span><span class="sxs-lookup"><span data-stu-id="01455-114">Send Attachment</span></span>](https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/core-SendAttachment) | <span data-ttu-id="01455-115">Bot de ejemplo que envía al usuario datos adjuntos de elementos multimedia sencillos (imágenes).</span><span class="sxs-lookup"><span data-stu-id="01455-115">A sample bot that sends simple media attachments (images) to the user.</span></span> 
[<span data-ttu-id="01455-116">Receive Attachment</span><span class="sxs-lookup"><span data-stu-id="01455-116">Receive Attachment</span></span>](https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/core-ReceiveAttachment) | <span data-ttu-id="01455-117">Bot de ejemplo que recibe los datos adjuntos enviados por el usuario y los descarga.</span><span class="sxs-lookup"><span data-stu-id="01455-117">A sample bot that receives attachments sent by the user and downloads them.</span></span> 
[<span data-ttu-id="01455-118">Create New Conversation</span><span class="sxs-lookup"><span data-stu-id="01455-118">Create New Conversation</span></span>](https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/core-CreateNewConversation)  | <span data-ttu-id="01455-119">Bot de ejemplo que inicia una conversación nueva usando una dirección de usuario almacenada previamente.</span><span class="sxs-lookup"><span data-stu-id="01455-119">A sample bot that starts a new conversation using a previously stored user address.</span></span>
[<span data-ttu-id="01455-120">Get Members of a Conversation</span><span class="sxs-lookup"><span data-stu-id="01455-120">Get Members of a Conversation</span></span>](https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/core-GetConversationMembers) | <span data-ttu-id="01455-121">Bot de ejemplo que recupera la lista de miembros de la conversación y detecta cuando cambia.</span><span class="sxs-lookup"><span data-stu-id="01455-121">A sample bot that retrieves the conversation's members list and detects when it changes.</span></span> 
[<span data-ttu-id="01455-122">Direct Line</span><span class="sxs-lookup"><span data-stu-id="01455-122">Direct Line</span></span>](https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/core-DirectLine) | <span data-ttu-id="01455-123">Bot de ejemplo y cliente personalizado que se comunican entre sí usando Direct Line API.</span><span class="sxs-lookup"><span data-stu-id="01455-123">A sample bot and a custom client that communicate with each other using the Direct Line API.</span></span> 
[<span data-ttu-id="01455-124">Direct Line (WebSockets)</span><span class="sxs-lookup"><span data-stu-id="01455-124">Direct Line (WebSockets)</span></span>](https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/core-DirectLineWebSockets) | <span data-ttu-id="01455-125">Bot de ejemplo y cliente personalizado que se comunican entre sí mediante Direct Line API + WebSockets.</span><span class="sxs-lookup"><span data-stu-id="01455-125">A sample bot and a custom client that communicate with each other using the Direct Line API + WebSockets.</span></span> 
[<span data-ttu-id="01455-126">Multi Dialogs</span><span class="sxs-lookup"><span data-stu-id="01455-126">Multi Dialogs</span></span>](https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/core-MultiDialogs) | <span data-ttu-id="01455-127">Bot de ejemplo que muestra varios tipos de cuadros de diálogo.</span><span class="sxs-lookup"><span data-stu-id="01455-127">A sample bot that shows various kinds of dialogs.</span></span>
[<span data-ttu-id="01455-128">State API</span><span class="sxs-lookup"><span data-stu-id="01455-128">State API</span></span>](https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/core-State) | <span data-ttu-id="01455-129">Bot de ejemplo sin estado que realiza el seguimiento del contexto de una conversación.</span><span class="sxs-lookup"><span data-stu-id="01455-129">A stateless sample bot that tracks the context of a conversation.</span></span>
[<span data-ttu-id="01455-130">Custom State API</span><span class="sxs-lookup"><span data-stu-id="01455-130">Custom State API</span></span>](https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/core-CustomState) | <span data-ttu-id="01455-131">Bot de ejemplo sin estado que realiza el seguimiento del contexto de una conversación mediante un proveedor de almacenamiento personalizado.</span><span class="sxs-lookup"><span data-stu-id="01455-131">A stateless sample bot that tracks the context of a conversation using a custom storage provider.</span></span>
[<span data-ttu-id="01455-132">ChannelData</span><span class="sxs-lookup"><span data-stu-id="01455-132">ChannelData</span></span>](https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/core-ChannelData) | <span data-ttu-id="01455-133">Bot de ejemplo que envía metadatos nativos a Facebook mediante ChannelData.</span><span class="sxs-lookup"><span data-stu-id="01455-133">A sample bot that sends native metadata to Facebook using ChannelData.</span></span>
[<span data-ttu-id="01455-134">AppInsights</span><span class="sxs-lookup"><span data-stu-id="01455-134">AppInsights</span></span>](https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/core-AppInsights) | <span data-ttu-id="01455-135">Bot de ejemplo que registra datos de telemetría en una instancia de Application Insights.</span><span class="sxs-lookup"><span data-stu-id="01455-135">A sample bot that logs telemetry to an Application Insights instance.</span></span>

## <a name="search"></a><span data-ttu-id="01455-136">Search</span><span class="sxs-lookup"><span data-stu-id="01455-136">Search</span></span>
<span data-ttu-id="01455-137">En este ejemplo se muestra cómo aprovechar Azure Search en bots controlados por datos.</span><span class="sxs-lookup"><span data-stu-id="01455-137">This sample shows how to leverage Azure Search in data-driven bots.</span></span>

<span data-ttu-id="01455-138">Muestra</span><span class="sxs-lookup"><span data-stu-id="01455-138">Sample</span></span> | <span data-ttu-id="01455-139">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="01455-139">Description</span></span>
------------ | -------------
[<span data-ttu-id="01455-140">Azure Search</span><span class="sxs-lookup"><span data-stu-id="01455-140">Azure Search</span></span>](https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/demo-Search) | <span data-ttu-id="01455-141">Dos bots de ejemplo que ayudan al usuario a navegar por grandes cantidades de contenido.</span><span class="sxs-lookup"><span data-stu-id="01455-141">Two sample bots that help the user navigate large amounts of content.</span></span>


## <a name="cards"></a><span data-ttu-id="01455-142">Cards</span><span class="sxs-lookup"><span data-stu-id="01455-142">Cards</span></span>
<span data-ttu-id="01455-143">En estos ejemplos se muestra cómo enviar tarjetas enriquecidas en Bot Framework.</span><span class="sxs-lookup"><span data-stu-id="01455-143">These samples show how to send rich cards in the Bot Framework.</span></span>

<span data-ttu-id="01455-144">Muestra</span><span class="sxs-lookup"><span data-stu-id="01455-144">Sample</span></span> | <span data-ttu-id="01455-145">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="01455-145">Description</span></span>
------------ | -------------
[<span data-ttu-id="01455-146">Rich Cards</span><span class="sxs-lookup"><span data-stu-id="01455-146">Rich Cards</span></span>](https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/cards-RichCards) | <span data-ttu-id="01455-147">Bot de ejemplo que envía varios tipos de tarjetas enriquecidas.</span><span class="sxs-lookup"><span data-stu-id="01455-147">A sample bot that sends several different types of rich cards.</span></span>
[<span data-ttu-id="01455-148">Carousel of Cards</span><span class="sxs-lookup"><span data-stu-id="01455-148">Carousel of Cards</span></span>](https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/cards-CarouselCards) | <span data-ttu-id="01455-149">Bot de ejemplo que envía varias tarjetas enriquecidas dentro de un solo mensaje con el diseño de carrusel.</span><span class="sxs-lookup"><span data-stu-id="01455-149">A sample bot that sends multiple rich cards within a single message using the Carousel layout.</span></span>

## <a name="intelligence"></a><span data-ttu-id="01455-150">Inteligencia</span><span class="sxs-lookup"><span data-stu-id="01455-150">Intelligence</span></span>
<span data-ttu-id="01455-151">En estos ejemplos se muestra cómo agregar funcionalidades de inteligencia artificial a un bot con las API de Bing y Microsoft Cognitive Services.</span><span class="sxs-lookup"><span data-stu-id="01455-151">These samples show how to add artificial intelligence capabilities to a bot using Bing and Microsoft Cognitive Services APIs.</span></span>

<span data-ttu-id="01455-152">Muestra</span><span class="sxs-lookup"><span data-stu-id="01455-152">Sample</span></span> | <span data-ttu-id="01455-153">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="01455-153">Description</span></span>
------------ | -------------
[<span data-ttu-id="01455-154">LUIS</span><span class="sxs-lookup"><span data-stu-id="01455-154">LUIS</span></span>](https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/intelligence-LUIS) | <span data-ttu-id="01455-155">Bot de ejemplo que usa LuisDialog para integrarse con una aplicación LUIS.ai.</span><span class="sxs-lookup"><span data-stu-id="01455-155">A sample bot that uses LuisDialog to integrate with a LUIS.ai application.</span></span>
[<span data-ttu-id="01455-156">Image Caption</span><span class="sxs-lookup"><span data-stu-id="01455-156">Image Caption</span></span>](https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/intelligence-ImageCaption) | <span data-ttu-id="01455-157">Bot de ejemplo que obtiene un título de imagen mediante Vision API de Microsoft Cognitive Services.</span><span class="sxs-lookup"><span data-stu-id="01455-157">A sample bot that gets an image caption using the Microsoft Cognitive Services Vision API.</span></span>
[<span data-ttu-id="01455-158">Speech To Text</span><span class="sxs-lookup"><span data-stu-id="01455-158">Speech To Text</span></span>](https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/intelligence-SpeechToText)  | <span data-ttu-id="01455-159">Bot de ejemplo que obtiene texto de audio mediante Bing Speech API.</span><span class="sxs-lookup"><span data-stu-id="01455-159">A sample bot that gets text from audio using the Bing Speech API.</span></span>
[<span data-ttu-id="01455-160">Similar Products</span><span class="sxs-lookup"><span data-stu-id="01455-160">Similar Products</span></span>](https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/intelligence-SimilarProducts) | <span data-ttu-id="01455-161">Bot de ejemplo que busca productos visualmente similares con Bing Image Search API.</span><span class="sxs-lookup"><span data-stu-id="01455-161">A sample bot that finds visually similar products using the Bing Image Search API.</span></span> 
[<span data-ttu-id="01455-162">Zummer</span><span class="sxs-lookup"><span data-stu-id="01455-162">Zummer</span></span>](https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/intelligence-Zummer) | <span data-ttu-id="01455-163">Bot de ejemplo que busca artículos de Wikipedia mediante Bing Search API.</span><span class="sxs-lookup"><span data-stu-id="01455-163">A sample bot that finds wikipedia articles using the Bing Search API.</span></span>

## <a name="reference-implementation"></a><span data-ttu-id="01455-164">Implementación de referencia</span><span class="sxs-lookup"><span data-stu-id="01455-164">Reference implementation</span></span>
<span data-ttu-id="01455-165">Este ejemplo está diseñado para presentar un escenario de principio a fin.</span><span class="sxs-lookup"><span data-stu-id="01455-165">This sample is designed to showcase an end-to-end scenario.</span></span> <span data-ttu-id="01455-166">Es una fuente excelente de fragmentos de código si está pensando en implementar características más complejas en el bot.</span><span class="sxs-lookup"><span data-stu-id="01455-166">It's a great source of code fragments if you're looking to implement more complex features in your bot.</span></span>


<span data-ttu-id="01455-167">Muestra</span><span class="sxs-lookup"><span data-stu-id="01455-167">Sample</span></span> | <span data-ttu-id="01455-168">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="01455-168">Description</span></span>
------------ | -------------
[<span data-ttu-id="01455-169">Contoso Flowers</span><span class="sxs-lookup"><span data-stu-id="01455-169">Contoso Flowers</span></span>](https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/demo-ContosoFlowers) | <span data-ttu-id="01455-170">Bot de ejemplo que usa numerosas características de Bot Framework.</span><span class="sxs-lookup"><span data-stu-id="01455-170">A sample bot that uses many features of the Bot Framework.</span></span>

::: moniker-end

::: moniker range="azure-bot-service-4.0"

[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]

<span data-ttu-id="01455-171">Los ejemplos del repositorio de ejemplos de Bot Builder describen bots basados en tareas en los que se muestra cómo aprovechar las características proporcionadas en el SDK para .NET.</span><span class="sxs-lookup"><span data-stu-id="01455-171">Samples in the Bot Builder Samples repo demonstrate task-focused bots that show how to take advantage of features provided in the SDK for .NET.</span></span> <span data-ttu-id="01455-172">Puede usar los ejemplos para empezar a trabajar rápidamente en la compilación de bots excelentes con funcionalidades enriquecidas.</span><span class="sxs-lookup"><span data-stu-id="01455-172">You can use the samples to help you quickly get started with building great bots with rich capabilities.</span></span> <span data-ttu-id="01455-173">Consulte el archivo [Léame](https://github.com/Microsoft/BotBuilder-Samples/blob/master/README.md) para obtener una lista de ejemplos e información adicional.</span><span class="sxs-lookup"><span data-stu-id="01455-173">Refer to [readme](https://github.com/Microsoft/BotBuilder-Samples/blob/master/README.md) file for sample list and additional informaiton.</span></span>

<span data-ttu-id="01455-174">Para obtener los ejemplos, clone el repositorio [botbuilder-samples](https://github.com/Microsoft/botbuilder-samples) de GitHub mediante Git.</span><span class="sxs-lookup"><span data-stu-id="01455-174">To get the samples, clone the [botbuilder-samples](https://github.com/Microsoft/botbuilder-samples) GitHub repository using Git.</span></span>
```cmd
git clone https://github.com/Microsoft/botbuilder-dotnet.git
```

::: moniker-end

