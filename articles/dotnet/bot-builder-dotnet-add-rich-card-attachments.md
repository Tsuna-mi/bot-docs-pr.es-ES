---
title: Incorporación de datos adjuntos de tarjetas enriquecidas a mensajes | Microsoft Docs
description: Obtenga información sobre cómo agregar tarjetas enriquecidas a los mensajes mediante el SDK de Bot Builder para. NET.
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: 9eb07a4ac63816b84830956bca0c3a3910669e0d
ms.sourcegitcommit: 67445b42796d90661afc643c6bb6533e9a662cbc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2018
ms.locfileid: "39574541"
---
# <a name="add-rich-card-attachments-to-messages"></a><span data-ttu-id="5fa5c-103">Incorporación de datos adjuntos de tarjetas enriquecidas a mensajes</span><span class="sxs-lookup"><span data-stu-id="5fa5c-103">Add rich card attachments to messages</span></span>

[!INCLUDE [pre-release-label](../includes/pre-release-label-v3.md)]

> [!div class="op_single_selector"]
> - [.NET](../dotnet/bot-builder-dotnet-add-rich-card-attachments.md)
> - [Node.js](../nodejs/bot-builder-nodejs-send-rich-cards.md)
> - [REST](../rest-api/bot-framework-rest-connector-add-rich-cards.md)

<span data-ttu-id="5fa5c-107">Un intercambio de mensajes entre el usuario y el bot puede contener una o varias tarjetas enriquecidas que se presentan como una lista o un carrusel.</span><span class="sxs-lookup"><span data-stu-id="5fa5c-107">A message exchange between user and bot can contain one or more rich cards rendered as a list or carousel.</span></span> <span data-ttu-id="5fa5c-108">La propiedad `Attachments` del objeto <a href="https://docs.botframework.com/en-us/csharp/builder/sdkreference/dc/d2f/class_microsoft_1_1_bot_1_1_connector_1_1_activity.html" target="_blank">Activity</a> contiene una matriz de objetos <a href="https://docs.microsoft.com/en-us/dotnet/api/microsoft.bot.connector.attachments?view=botconnector-3.12.2.4" target="_blank">Attachment</a> que presentan las tarjetas enriquecidas y los datos adjuntos con elementos multimedia dentro del mensaje.</span><span class="sxs-lookup"><span data-stu-id="5fa5c-108">The `Attachments` property of the <a href="https://docs.botframework.com/en-us/csharp/builder/sdkreference/dc/d2f/class_microsoft_1_1_bot_1_1_connector_1_1_activity.html" target="_blank">Activity</a> object contains an array of <a href="https://docs.microsoft.com/en-us/dotnet/api/microsoft.bot.connector.attachments?view=botconnector-3.12.2.4" target="_blank">Attachment</a> objects that represent the rich cards and media attachments within the message.</span></span> 

> [!NOTE]
> Para información sobre cómo agregar datos adjuntos con elementos multimedia a los mensajes, consulte [Incorporación de datos adjuntos con elementos multimedia a mensajes](bot-builder-dotnet-add-media-attachments.md).

## <a name="types-of-rich-cards"></a><span data-ttu-id="5fa5c-110">Tipos de tarjetas enriquecidas</span><span class="sxs-lookup"><span data-stu-id="5fa5c-110">Types of rich cards</span></span>

<span data-ttu-id="5fa5c-111">Bot Framework admite actualmente ocho tipos de tarjetas enriquecidas:</span><span class="sxs-lookup"><span data-stu-id="5fa5c-111">The Bot Framework currently supports eight types of rich cards:</span></span> 

| <span data-ttu-id="5fa5c-112">Tipo de tarjeta</span><span class="sxs-lookup"><span data-stu-id="5fa5c-112">Card type</span></span> | <span data-ttu-id="5fa5c-113">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="5fa5c-113">Description</span></span> |
|----|----|
| <span data-ttu-id="5fa5c-114"><a href="/adaptive-cards/get-started/bots">Tarjeta adaptable</a></span><span class="sxs-lookup"><span data-stu-id="5fa5c-114"><a href="/adaptive-cards/get-started/bots">Adaptive Card</a></span></span> | <span data-ttu-id="5fa5c-115">Una tarjeta personalizable que puede contener cualquier combinación de texto, voz, imágenes, botones y campos de entrada.</span><span class="sxs-lookup"><span data-stu-id="5fa5c-115">A customizable card that can contain any combination of text, speech, images, buttons, and input fields.</span></span> <span data-ttu-id="5fa5c-116">Consulte la [compatibilidad por canal](/adaptive-cards/get-started/bots#channel-status).</span><span class="sxs-lookup"><span data-stu-id="5fa5c-116">See [per-channel support](/adaptive-cards/get-started/bots#channel-status).</span></span>  |
| <span data-ttu-id="5fa5c-117">[Tarjeta de animación][animationCard]</span><span class="sxs-lookup"><span data-stu-id="5fa5c-117">[Animation Card][animationCard]</span></span> | <span data-ttu-id="5fa5c-118">Una tarjeta que puede reproducir archivos GIF animados o vídeos cortos.</span><span class="sxs-lookup"><span data-stu-id="5fa5c-118">A card that can play animated GIFs or short videos.</span></span> |
| <span data-ttu-id="5fa5c-119">[Tarjeta de audio][audioCard]</span><span class="sxs-lookup"><span data-stu-id="5fa5c-119">[Audio Card][audioCard]</span></span> | <span data-ttu-id="5fa5c-120">Una tarjeta que se puede reproducir un archivo de audio.</span><span class="sxs-lookup"><span data-stu-id="5fa5c-120">A card that can play an audio file.</span></span> |
| <span data-ttu-id="5fa5c-121">[Tarjeta de imagen prominente][heroCard]</span><span class="sxs-lookup"><span data-stu-id="5fa5c-121">[Hero Card][heroCard]</span></span> | <span data-ttu-id="5fa5c-122">Una tarjeta que normalmente contiene una sola imagen grande, uno o varios botones y texto.</span><span class="sxs-lookup"><span data-stu-id="5fa5c-122">A card that typically contains a single large image, one or more buttons, and text.</span></span> |
| <span data-ttu-id="5fa5c-123">[Tarjeta de miniatura][thumbnailCard]</span><span class="sxs-lookup"><span data-stu-id="5fa5c-123">[Thumbnail Card][thumbnailCard]</span></span> | <span data-ttu-id="5fa5c-124">Una tarjeta que normalmente contiene una sola imagen miniatura, uno o varios botones y texto.</span><span class="sxs-lookup"><span data-stu-id="5fa5c-124">A card that typically contains a single thumbnail image, one or more buttons, and text.</span></span> |
| <span data-ttu-id="5fa5c-125">[Tarjeta de recepción][receiptCard]</span><span class="sxs-lookup"><span data-stu-id="5fa5c-125">[Receipt Card][receiptCard]</span></span> | <span data-ttu-id="5fa5c-126">Una tarjeta que permite que un bot proporcione una recepción al usuario.</span><span class="sxs-lookup"><span data-stu-id="5fa5c-126">A card that enables a bot to provide a receipt to the user.</span></span> <span data-ttu-id="5fa5c-127">Normalmente, contiene la lista de elementos que se incluyen en el recibo, la información de impuestos y del total y texto adicional.</span><span class="sxs-lookup"><span data-stu-id="5fa5c-127">It typically contains the list of items to include on the receipt, tax and total information, and other text.</span></span> |
| <span data-ttu-id="5fa5c-128">[Tarjeta de inicio de sesión][signinCard]</span><span class="sxs-lookup"><span data-stu-id="5fa5c-128">[SignIn Card][signinCard]</span></span> | <span data-ttu-id="5fa5c-129">Una tarjeta que permite al bot solicitar el inicio de sesión del usuario.</span><span class="sxs-lookup"><span data-stu-id="5fa5c-129">A card that enables a bot to request that a user sign-in.</span></span> <span data-ttu-id="5fa5c-130">Normalmente contiene texto y uno o más botones en los cuales el usuario puede hacer clic para iniciar el proceso de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="5fa5c-130">It typically contains text and one or more buttons that the user can click to initiate the sign-in process.</span></span> |
| <span data-ttu-id="5fa5c-131">[Tarjeta de videollamada][videoCard]</span><span class="sxs-lookup"><span data-stu-id="5fa5c-131">[Video Card][videoCard]</span></span> | <span data-ttu-id="5fa5c-132">Una tarjeta que puede reproducir vídeos.</span><span class="sxs-lookup"><span data-stu-id="5fa5c-132">A card that can play videos.</span></span> |

> [!TIP]
> Para mostrar varias tarjetas enriquecidas en formato de lista, establezca la propiedad `AttachmentLayout` de la actividad en "lista". Para mostrar varias tarjetas enriquecidas en formato de carrusel, establezca la propiedad `AttachmentLayout` de la actividad en "carrusel". Si el canal no admite el formato de carrusel, mostrará las tarjetas enriquecidas en el formato de lista, incluso si la propiedad `AttachmentLayout` especifica "carrusel".

## <a name="process-events-within-rich-cards"></a><span data-ttu-id="5fa5c-136">Procesamiento de eventos dentro de tarjetas enriquecidas</span><span class="sxs-lookup"><span data-stu-id="5fa5c-136">Process events within rich cards</span></span>

<span data-ttu-id="5fa5c-137">Para procesar eventos dentro de tarjetas enriquecidas, defina los objetos `CardAction` para especificar qué debe ocurrir cuando el usuario hace clic en un botón o pulsa una sección de la tarjeta.</span><span class="sxs-lookup"><span data-stu-id="5fa5c-137">To process events within rich cards, define `CardAction` objects to specify what should happen when the user clicks a button or taps a section of the card.</span></span> <span data-ttu-id="5fa5c-138">Cada objeto `CardAction` contiene estas propiedades:</span><span class="sxs-lookup"><span data-stu-id="5fa5c-138">Each `CardAction` object contains these properties:</span></span>

| <span data-ttu-id="5fa5c-139">Propiedad</span><span class="sxs-lookup"><span data-stu-id="5fa5c-139">Property</span></span> | <span data-ttu-id="5fa5c-140">Escriba</span><span class="sxs-lookup"><span data-stu-id="5fa5c-140">Type</span></span> | <span data-ttu-id="5fa5c-141">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="5fa5c-141">Description</span></span> | 
|----|----|----|
| <span data-ttu-id="5fa5c-142">Escriba</span><span class="sxs-lookup"><span data-stu-id="5fa5c-142">Type</span></span> | <span data-ttu-id="5fa5c-143">string</span><span class="sxs-lookup"><span data-stu-id="5fa5c-143">string</span></span> | <span data-ttu-id="5fa5c-144">tipo de acción (uno de los valores especificados en la tabla siguiente)</span><span class="sxs-lookup"><span data-stu-id="5fa5c-144">type of action (one of the values specified in the table below)</span></span> |
| <span data-ttu-id="5fa5c-145">Título</span><span class="sxs-lookup"><span data-stu-id="5fa5c-145">Title</span></span> | <span data-ttu-id="5fa5c-146">string</span><span class="sxs-lookup"><span data-stu-id="5fa5c-146">string</span></span> | <span data-ttu-id="5fa5c-147">título del botón</span><span class="sxs-lookup"><span data-stu-id="5fa5c-147">title of the button</span></span> |
| <span data-ttu-id="5fa5c-148">Imagen</span><span class="sxs-lookup"><span data-stu-id="5fa5c-148">Image</span></span> | <span data-ttu-id="5fa5c-149">string</span><span class="sxs-lookup"><span data-stu-id="5fa5c-149">string</span></span> | <span data-ttu-id="5fa5c-150">dirección URL de la imagen del botón</span><span class="sxs-lookup"><span data-stu-id="5fa5c-150">image URL for the button</span></span> |
| <span data-ttu-id="5fa5c-151">Valor</span><span class="sxs-lookup"><span data-stu-id="5fa5c-151">Value</span></span> | <span data-ttu-id="5fa5c-152">string</span><span class="sxs-lookup"><span data-stu-id="5fa5c-152">string</span></span> | <span data-ttu-id="5fa5c-153">valor necesario para realizar el tipo de acción especificado</span><span class="sxs-lookup"><span data-stu-id="5fa5c-153">value needed to perform the specified type of action</span></span> |

> [!NOTE]
> Los botones dentro de las tarjetas adaptables no se crean mediante el uso de objetos `CardAction`, sino que mediante el esquema que definen las <a href="http://adaptivecards.io" target="_blank">tarjetas adaptables</a>. Consulte [Add an Adaptive Card to a message](#adaptive-card) (Incorporación de una tarjeta adaptable a un mensaje) para ver un ejemplo que muestra cómo agregar botones a una tarjeta adaptable.

<span data-ttu-id="5fa5c-156">En esta tabla se enumeran los valores válidos para `CardAction.Type` y se describe el contenido esperado de `CardAction.Value` para cada tipo:</span><span class="sxs-lookup"><span data-stu-id="5fa5c-156">This table lists the valid values for `CardAction.Type` and describes the expected contents of `CardAction.Value` for each type:</span></span>

| <span data-ttu-id="5fa5c-157">CardAction.Type</span><span class="sxs-lookup"><span data-stu-id="5fa5c-157">CardAction.Type</span></span> | <span data-ttu-id="5fa5c-158">CardAction.Value</span><span class="sxs-lookup"><span data-stu-id="5fa5c-158">CardAction.Value</span></span> | 
|----|----|
| <span data-ttu-id="5fa5c-159">openUrl</span><span class="sxs-lookup"><span data-stu-id="5fa5c-159">openUrl</span></span> | <span data-ttu-id="5fa5c-160">Dirección URL que se abrirá en el explorador integrado.</span><span class="sxs-lookup"><span data-stu-id="5fa5c-160">URL to be opened in the built-in browser</span></span> |
| <span data-ttu-id="5fa5c-161">imBack</span><span class="sxs-lookup"><span data-stu-id="5fa5c-161">imBack</span></span> | <span data-ttu-id="5fa5c-162">Texto del mensaje para enviar al bot (desde el usuario que hizo clic en el botón o que pulsó la tarjeta).</span><span class="sxs-lookup"><span data-stu-id="5fa5c-162">Text of the message to send to the bot (from the user who clicked the button or tapped the card).</span></span> <span data-ttu-id="5fa5c-163">Este mensaje (del usuario al bot) será visible para todos los participantes de la conversación a través de la aplicación cliente que hospeda la conversación.</span><span class="sxs-lookup"><span data-stu-id="5fa5c-163">This message (from user to bot) will be visible to all conversation participants via the client application that is hosting the conversation.</span></span> |
| <span data-ttu-id="5fa5c-164">postBack</span><span class="sxs-lookup"><span data-stu-id="5fa5c-164">postBack</span></span> | <span data-ttu-id="5fa5c-165">Texto del mensaje para enviar al bot (desde el usuario que hizo clic en el botón o que pulsó la tarjeta).</span><span class="sxs-lookup"><span data-stu-id="5fa5c-165">Text of the message to send to the bot (from the user who clicked the button or tapped the card).</span></span> <span data-ttu-id="5fa5c-166">Algunas aplicaciones cliente pueden mostrar este texto en la fuente del mensaje, donde estará visible para todos los participantes de la conversación.</span><span class="sxs-lookup"><span data-stu-id="5fa5c-166">Some client applications may display this text in the message feed, where it will be visible to all conversation participants.</span></span> |
| <span data-ttu-id="5fa5c-167">llamada</span><span class="sxs-lookup"><span data-stu-id="5fa5c-167">call</span></span> | <span data-ttu-id="5fa5c-168">Destino de una llamada telefónica con este formato: **tel:123123123123**</span><span class="sxs-lookup"><span data-stu-id="5fa5c-168">Destination for a phone call in this format: **tel:123123123123**</span></span> |
| <span data-ttu-id="5fa5c-169">playAudio</span><span class="sxs-lookup"><span data-stu-id="5fa5c-169">playAudio</span></span> | <span data-ttu-id="5fa5c-170">Dirección URL del audio que se va a reproducir.</span><span class="sxs-lookup"><span data-stu-id="5fa5c-170">URL of audio to be played</span></span> |
| <span data-ttu-id="5fa5c-171">playVideo</span><span class="sxs-lookup"><span data-stu-id="5fa5c-171">playVideo</span></span> | <span data-ttu-id="5fa5c-172">Dirección URL del vídeo que se va a reproducir.</span><span class="sxs-lookup"><span data-stu-id="5fa5c-172">URL of video to be played</span></span> |
| <span data-ttu-id="5fa5c-173">showImage</span><span class="sxs-lookup"><span data-stu-id="5fa5c-173">showImage</span></span> | <span data-ttu-id="5fa5c-174">Dirección URL de la imagen que se va a mostrar.</span><span class="sxs-lookup"><span data-stu-id="5fa5c-174">URL of image to be displayed</span></span> |
| <span data-ttu-id="5fa5c-175">DownloadFile</span><span class="sxs-lookup"><span data-stu-id="5fa5c-175">downloadFile</span></span> | <span data-ttu-id="5fa5c-176">Dirección URL del archivo que se va a descargar.</span><span class="sxs-lookup"><span data-stu-id="5fa5c-176">URL of file to be downloaded</span></span> |
| <span data-ttu-id="5fa5c-177">signin</span><span class="sxs-lookup"><span data-stu-id="5fa5c-177">signin</span></span> | <span data-ttu-id="5fa5c-178">Dirección URL del flujo de OAuth que se va a iniciar.</span><span class="sxs-lookup"><span data-stu-id="5fa5c-178">URL of OAuth flow to be initiated</span></span> |

## <a name="add-a-hero-card-to-a-message"></a><span data-ttu-id="5fa5c-179">Incorporación de una tarjeta de imagen prominente a un mensaje</span><span class="sxs-lookup"><span data-stu-id="5fa5c-179">Add a Hero card to a message</span></span>

<span data-ttu-id="5fa5c-180">La tarjeta de imagen prominente normalmente contiene una sola imagen grande, uno o varios botones y texto.</span><span class="sxs-lookup"><span data-stu-id="5fa5c-180">The Hero card typically contains a single large image, one or more buttons, and text.</span></span> 

<span data-ttu-id="5fa5c-181">Este ejemplo de código muestra cómo crear un mensaje de respuesta que contiene tres tarjetas de imagen prominente presentadas en formato de carrusel:</span><span class="sxs-lookup"><span data-stu-id="5fa5c-181">This code example shows how to create a reply message that contains three Hero cards rendered in carousel format:</span></span> 

[!code-csharp[Add HeroCard attachment](../includes/code/dotnet-add-attachments.cs#addHeroCardAttachment)]

## <a name="add-a-thumbnail-card-to-a-message"></a><span data-ttu-id="5fa5c-182">Incorporación de una tarjeta de miniatura a un mensaje</span><span class="sxs-lookup"><span data-stu-id="5fa5c-182">Add a Thumbnail card to a message</span></span>

<span data-ttu-id="5fa5c-183">La tarjeta de miniatura normalmente contiene una sola imagen en miniatura, uno o varios botones y texto.</span><span class="sxs-lookup"><span data-stu-id="5fa5c-183">The Thumbnail card typically contains a single thumbnail image, one or more buttons, and text.</span></span> 

<span data-ttu-id="5fa5c-184">Este ejemplo de código muestra cómo crear un mensaje de respuesta que contiene dos tarjetas de miniatura presentadas en formato de lista:</span><span class="sxs-lookup"><span data-stu-id="5fa5c-184">This code example shows how to create a reply message that contains two Thumbnail cards rendered in list format:</span></span> 

[!code-csharp[Add ThumbnailCard attachment](../includes/code/dotnet-add-attachments.cs#addThumbnailCardAttachment)]

## <a name="add-a-receipt-card-to-a-message"></a><span data-ttu-id="5fa5c-185">Incorporación de una tarjeta de recepción a un mensaje</span><span class="sxs-lookup"><span data-stu-id="5fa5c-185">Add a Receipt card to a message</span></span>

<span data-ttu-id="5fa5c-186">La tarjeta de recepción permite que un bot proporcione una recepción al usuario.</span><span class="sxs-lookup"><span data-stu-id="5fa5c-186">The Receipt card enables a bot to provide a receipt to the user.</span></span> <span data-ttu-id="5fa5c-187">Normalmente, contiene la lista de elementos que se incluyen en el recibo, la información de impuestos y del total y texto adicional.</span><span class="sxs-lookup"><span data-stu-id="5fa5c-187">It typically contains the list of items to include on the receipt, tax and total information, and other text.</span></span> 

<span data-ttu-id="5fa5c-188">Este ejemplo de código muestra cómo crear un mensaje de respuesta que contiene una tarjeta de recepción:</span><span class="sxs-lookup"><span data-stu-id="5fa5c-188">This code example shows how to create a reply message that contains a Receipt card:</span></span> 

[!code-csharp[Add ReceiptCard attachment](../includes/code/dotnet-add-attachments.cs#addReceiptCardAttachment)]

## <a name="add-a-sign-in-card-to-a-message"></a><span data-ttu-id="5fa5c-189">Incorporación de una tarjeta de inicio de sesión a un mensaje</span><span class="sxs-lookup"><span data-stu-id="5fa5c-189">Add a Sign-in card to a message</span></span>

<span data-ttu-id="5fa5c-190">La tarjeta de inicio de sesión permite al bot solicitar el inicio de sesión del usuario.</span><span class="sxs-lookup"><span data-stu-id="5fa5c-190">The Sign-in card enables a bot to request that a user sign-in.</span></span> <span data-ttu-id="5fa5c-191">Normalmente contiene texto y uno o más botones en los cuales el usuario puede hacer clic para iniciar el proceso de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="5fa5c-191">It typically contains text and one or more buttons that the user can click to initiate the sign-in process.</span></span> 

<span data-ttu-id="5fa5c-192">Este ejemplo de código muestra cómo crear un mensaje de respuesta que contiene una tarjeta de inicio de sesión:</span><span class="sxs-lookup"><span data-stu-id="5fa5c-192">This code example shows how to create a reply message that contains a Sign-in card:</span></span>

[!code-csharp[Add SignInCard attachment](../includes/code/dotnet-add-attachments.cs#addSignInCardAttachment)]

## <a id="adaptive-card"></a> <span data-ttu-id="5fa5c-193">Incorporación de una tarjeta adaptable a un mensaje</span><span class="sxs-lookup"><span data-stu-id="5fa5c-193">Add an Adaptive card to a message</span></span>

<span data-ttu-id="5fa5c-194">Una tarjeta adaptable puede contener cualquier combinación de texto, voz, imágenes, botones y campos de entrada.</span><span class="sxs-lookup"><span data-stu-id="5fa5c-194">The Adaptive Card can contain any combination of text, speech, images, buttons, and input fields.</span></span> <span data-ttu-id="5fa5c-195">Las tarjetas adaptables se crean con el formato JSON especificado en <a href="http://adaptivecards.io" target="_blank">Adaptive Cards</a> (Tarjetas adaptables), que proporciona control total sobre el contenido y el formato de la tarjeta.</span><span class="sxs-lookup"><span data-stu-id="5fa5c-195">Adaptive Cards are created using the JSON format specified in <a href="http://adaptivecards.io" target="_blank">Adaptive Cards</a>, which gives you full control over card content and format.</span></span> 

<span data-ttu-id="5fa5c-196">Para crear una tarjeta adaptable con. NET, instale el paquete `Microsoft.AdaptiveCards` de NuGet.</span><span class="sxs-lookup"><span data-stu-id="5fa5c-196">To create an Adaptive Card using .NET, install the `Microsoft.AdaptiveCards` NuGet package.</span></span> <span data-ttu-id="5fa5c-197">Luego, use la información del sitio <a href="http://adaptivecards.io" target="_blank">Adaptive Cards</a> (Tarjetas adaptables) a fin de comprender el esquema de la tarjeta adaptable, explorar los elementos de la tarjeta adaptable y ver ejemplos de JSON que puede usar para crear tarjetas de diferente composición y complejidad.</span><span class="sxs-lookup"><span data-stu-id="5fa5c-197">Then, leverage the information within the <a href="http://adaptivecards.io" target="_blank">Adaptive Cards</a> site to understand Adaptive Card schema, explore Adaptive Card elements, and see JSON samples that can be used to create cards of varying composition and complexity.</span></span> <span data-ttu-id="5fa5c-198">Además, puede usar el visualizador interactivo para diseñar cargas útiles de la tarjeta adaptable y obtener una vista previa del resultado de la tarjeta.</span><span class="sxs-lookup"><span data-stu-id="5fa5c-198">Additionally, you can use the Interactive Visualizer to design Adaptive Card payloads and preview card output.</span></span>

<span data-ttu-id="5fa5c-199">En este ejemplo de código se muestra cómo crear un mensaje que contiene una tarjeta adaptable para un recordatorio del calendario:</span><span class="sxs-lookup"><span data-stu-id="5fa5c-199">This code example shows how to create a message that contains an Adaptive Card for a calendar reminder:</span></span> 

[!code-csharp[Add Adaptive Card attachment](../includes/code/dotnet-add-attachments.cs#addAdaptiveCardAttachment)]

<span data-ttu-id="5fa5c-200">La tarjeta resultante contiene tres bloques de texto, un campo de entrada (lista de opciones) y tres botones:</span><span class="sxs-lookup"><span data-stu-id="5fa5c-200">The resulting card contains three blocks of text, an input field (choice list), and three buttons:</span></span>

![Recordatorio del calendario de tarjeta adaptable](../media/adaptive-card-reminder.png)

## <a name="additional-resources"></a><span data-ttu-id="5fa5c-202">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="5fa5c-202">Additional resources</span></span>

- <span data-ttu-id="5fa5c-203">[Preview features with the Channel Inspector][inspector] (Vista previa de las características con el Inspector de canales)</span><span class="sxs-lookup"><span data-stu-id="5fa5c-203">[Preview features with the Channel Inspector][inspector]</span></span>
- <span data-ttu-id="5fa5c-204"><a href="http://adaptivecards.io" target="_blank">Adaptive Cards</a> (Tarjetas adaptables)</span><span class="sxs-lookup"><span data-stu-id="5fa5c-204"><a href="http://adaptivecards.io" target="_blank">Adaptive Cards</a></span></span>
- <span data-ttu-id="5fa5c-205">[Activities overview](bot-builder-dotnet-activities.md) (Introducción a las actividades)</span><span class="sxs-lookup"><span data-stu-id="5fa5c-205">[Activities overview](bot-builder-dotnet-activities.md)</span></span>
- [<span data-ttu-id="5fa5c-206">Creación de mensajes</span><span class="sxs-lookup"><span data-stu-id="5fa5c-206">Create messages</span></span>](bot-builder-dotnet-create-messages.md)
- [<span data-ttu-id="5fa5c-207">Incorporación de datos adjuntos con elementos multimedia a mensajes</span><span class="sxs-lookup"><span data-stu-id="5fa5c-207">Add media attachments to messages</span></span>](bot-builder-dotnet-add-media-attachments.md)
- <span data-ttu-id="5fa5c-208"><a href="https://docs.botframework.com/en-us/csharp/builder/sdkreference/dc/d2f/class_microsoft_1_1_bot_1_1_connector_1_1_activity.html" target="_blank">Clase Activity</a></span><span class="sxs-lookup"><span data-stu-id="5fa5c-208"><a href="https://docs.botframework.com/en-us/csharp/builder/sdkreference/dc/d2f/class_microsoft_1_1_bot_1_1_connector_1_1_activity.html" target="_blank">Activity class</a></span></span>
- <span data-ttu-id="5fa5c-209"><a href="https://docs.microsoft.com/en-us/dotnet/api/microsoft.bot.connector.attachments?view=botconnector-3.12.2.4" target="_blank">Clase Attachment</a></span><span class="sxs-lookup"><span data-stu-id="5fa5c-209"><a href="https://docs.microsoft.com/en-us/dotnet/api/microsoft.bot.connector.attachments?view=botconnector-3.12.2.4" target="_blank">Attachment class</a></span></span>

[animationCard]: /dotnet/api/microsoft.bot.connector.animationcard

[audioCard]: /dotnet/api/microsoft.bot.connector.audiocard 

[heroCard]: /dotnet/api/microsoft.bot.connector.herocard 

[thumbnailCard]: /dotnet/api/microsoft.bot.connector.thumbnailcard 

[receiptCard]: /dotnet/api/microsoft.bot.connector.receiptcard 

[signinCard]: /dotnet/api/microsoft.bot.connector.signincard 

[videoCard]: /dotnet/api/microsoft.bot.connector.videocard

[inspector]: ../bot-service-channel-inspector.md
