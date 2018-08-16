---
title: Incorporación de datos adjuntos de tarjetas enriquecidas a mensajes | Microsoft Docs
description: Aprenda a agregar tarjetas enriquecidas a los mensajes mediante el servicio Bot Connector.
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ms.openlocfilehash: 04f70777003ef5298de264f5ee8685b3a5005395
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305825"
---
# <a name="add-rich-card-attachments-to-messages"></a><span data-ttu-id="dbe72-103">Incorporación de datos adjuntos de tarjetas enriquecidas a mensajes</span><span class="sxs-lookup"><span data-stu-id="dbe72-103">Add rich card attachments to messages</span></span>
> [!div class="op_single_selector"]
> - [.NET](../dotnet/bot-builder-dotnet-add-rich-card-attachments.md)
> - [Node.js](../nodejs/bot-builder-nodejs-send-rich-cards.md)
> - [REST](../rest-api/bot-framework-rest-connector-add-rich-cards.md)

<span data-ttu-id="dbe72-107">Los bots y los canales suelen intercambiar cadenas de texto, pero algunos canales también admiten el intercambio de datos adjuntos, lo que permite a un bot enviar mensajes más completos a los usuarios.</span><span class="sxs-lookup"><span data-stu-id="dbe72-107">Bots and channels typically exchange text strings but some channels also support exchanging attachments, which lets your bot send richer messages to users.</span></span> <span data-ttu-id="dbe72-108">Por ejemplo, el bot puede enviar datos adjuntos de elementos multimedia y tarjetas enriquecidas (como imágenes, vídeos, audio, archivos).</span><span class="sxs-lookup"><span data-stu-id="dbe72-108">For example, your bot can send rich cards and media attachments (e.g., images, videos, audio, files).</span></span> <span data-ttu-id="dbe72-109">En este artículo se describe cómo agregar datos adjuntos de tarjetas enriquecidas a mensajes mediante el servicio Bot Connector.</span><span class="sxs-lookup"><span data-stu-id="dbe72-109">This article describes how to add rich card attachments to messages using the Bot Connector service.</span></span>

> [!NOTE]
> Para información sobre cómo agregar datos adjuntos con elementos multimedia a los mensajes, consulte [Incorporación de datos adjuntos con elementos multimedia a mensajes](bot-framework-rest-connector-add-media-attachments.md).

## <a id="types-of-cards"></a> <span data-ttu-id="dbe72-111">Tipos de tarjetas enriquecidas</span><span class="sxs-lookup"><span data-stu-id="dbe72-111">Types of rich cards</span></span>

<span data-ttu-id="dbe72-112">Una tarjeta enriquecida consta de un título, una descripción, un vínculo e imágenes.</span><span class="sxs-lookup"><span data-stu-id="dbe72-112">A rich card comprises a title, description, link, and images.</span></span> <span data-ttu-id="dbe72-113">Un mensaje puede contener varias tarjetas enriquecidas, que se muestran en formato de lista o de carrusel.</span><span class="sxs-lookup"><span data-stu-id="dbe72-113">A message can contain multiple rich cards, displayed in either list format or carousel format.</span></span>
<span data-ttu-id="dbe72-114">Bot Framework admite actualmente ocho tipos de tarjetas enriquecidas:</span><span class="sxs-lookup"><span data-stu-id="dbe72-114">The Bot Framework currently supports eight types of rich cards:</span></span> 

| <span data-ttu-id="dbe72-115">Tipo de tarjeta</span><span class="sxs-lookup"><span data-stu-id="dbe72-115">Card type</span></span> | <span data-ttu-id="dbe72-116">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="dbe72-116">Description</span></span> |
|----|----|
| <span data-ttu-id="dbe72-117"><a href="/adaptive-cards/get-started/bots">AdaptiveCard</a></span><span class="sxs-lookup"><span data-stu-id="dbe72-117"><a href="/adaptive-cards/get-started/bots">AdaptiveCard</a></span></span> | <span data-ttu-id="dbe72-118">Una tarjeta personalizable que puede contener cualquier combinación de texto, voz, imágenes, botones y campos de entrada.</span><span class="sxs-lookup"><span data-stu-id="dbe72-118">A customizable card that can contain any combination of text, speech, images, buttons, and input fields.</span></span> <span data-ttu-id="dbe72-119">Consulte la [compatibilidad por canal](/adaptive-cards/get-started/bots#channel-status).</span><span class="sxs-lookup"><span data-stu-id="dbe72-119">See [per-channel support](/adaptive-cards/get-started/bots#channel-status).</span></span>  |
| <span data-ttu-id="dbe72-120">[AnimationCard][animationCard]</span><span class="sxs-lookup"><span data-stu-id="dbe72-120">[AnimationCard][animationCard]</span></span> | <span data-ttu-id="dbe72-121">Una tarjeta que puede reproducir archivos GIF animados o vídeos cortos.</span><span class="sxs-lookup"><span data-stu-id="dbe72-121">A card that can play animated GIFs or short videos.</span></span> |
| <span data-ttu-id="dbe72-122">[AudioCard][audioCard]</span><span class="sxs-lookup"><span data-stu-id="dbe72-122">[AudioCard][audioCard]</span></span> | <span data-ttu-id="dbe72-123">Una tarjeta que se puede reproducir un archivo de audio.</span><span class="sxs-lookup"><span data-stu-id="dbe72-123">A card that can play an audio file.</span></span> |
| <span data-ttu-id="dbe72-124">[HeroCard][heroCard]</span><span class="sxs-lookup"><span data-stu-id="dbe72-124">[HeroCard][heroCard]</span></span> | <span data-ttu-id="dbe72-125">Una tarjeta que normalmente contiene una sola imagen grande, uno o varios botones y texto.</span><span class="sxs-lookup"><span data-stu-id="dbe72-125">A card that typically contains a single large image, one or more buttons, and text.</span></span> |
| <span data-ttu-id="dbe72-126">[ThumbnailCard][thumbnailCard]</span><span class="sxs-lookup"><span data-stu-id="dbe72-126">[ThumbnailCard][thumbnailCard]</span></span> | <span data-ttu-id="dbe72-127">Una tarjeta que normalmente contiene una sola imagen miniatura, uno o varios botones, y texto.</span><span class="sxs-lookup"><span data-stu-id="dbe72-127">A card that typically contains a single thumbnail image, one or more buttons, and text.</span></span> |
| <span data-ttu-id="dbe72-128">[ReceiptCard][receiptCard]</span><span class="sxs-lookup"><span data-stu-id="dbe72-128">[ReceiptCard][receiptCard]</span></span> | <span data-ttu-id="dbe72-129">Una tarjeta que permite que un bot proporcione un recibo al usuario.</span><span class="sxs-lookup"><span data-stu-id="dbe72-129">A card that enables a bot to provide a receipt to the user.</span></span> <span data-ttu-id="dbe72-130">Normalmente, contiene la lista de elementos que se incluyen en el recibo, la información de impuestos y del total, y texto adicional.</span><span class="sxs-lookup"><span data-stu-id="dbe72-130">It typically contains the list of items to include on the receipt, tax and total information, and other text.</span></span> |
| <span data-ttu-id="dbe72-131">[SignInCard][signinCard]</span><span class="sxs-lookup"><span data-stu-id="dbe72-131">[SignInCard][signinCard]</span></span> | <span data-ttu-id="dbe72-132">Una tarjeta que permite al bot solicitar que un usuario inicie sesión.</span><span class="sxs-lookup"><span data-stu-id="dbe72-132">A card that enables a bot to request that a user sign-in.</span></span> <span data-ttu-id="dbe72-133">Normalmente contiene texto y uno o varios botones en los que el usuario puede hacer clic para iniciar el proceso de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="dbe72-133">It typically contains text and one or more buttons that the user can click to initiate the sign-in process.</span></span> |
| <span data-ttu-id="dbe72-134">[VideoCard][videoCard]</span><span class="sxs-lookup"><span data-stu-id="dbe72-134">[VideoCard][videoCard]</span></span> | <span data-ttu-id="dbe72-135">Una tarjeta que puede reproducir vídeos.</span><span class="sxs-lookup"><span data-stu-id="dbe72-135">A card that can play videos.</span></span> |

> [!TIP]
> Para determinar el tipo de tarjetas enriquecidas que admite un canal y ver cómo las presenta, consulte el [Inspector de canales][ChannelInspector]. Consulte la documentación del canal para obtener información sobre las limitaciones del contenido de las tarjetas (por ejemplo, el número máximo de botones o la longitud máxima del título).

## <a name="process-events-within-rich-cards"></a><span data-ttu-id="dbe72-138">Procesamiento de eventos dentro de tarjetas enriquecidas</span><span class="sxs-lookup"><span data-stu-id="dbe72-138">Process events within rich cards</span></span>

<span data-ttu-id="dbe72-139">Para procesar eventos dentro de tarjetas enriquecidas, use los objetos [CardAction][CardAction] para especificar qué debe ocurrir cuando el usuario hace clic en un botón o pulsa una sección de la tarjeta.</span><span class="sxs-lookup"><span data-stu-id="dbe72-139">To process events within rich cards, use [CardAction][CardAction] objects to specify what should happen when the user clicks a button or taps a section of the card.</span></span> <span data-ttu-id="dbe72-140">Cada objeto [CardAction][CardAction] contiene estas propiedades:</span><span class="sxs-lookup"><span data-stu-id="dbe72-140">Each [CardAction][CardAction] object contains these properties:</span></span>

| <span data-ttu-id="dbe72-141">Propiedad</span><span class="sxs-lookup"><span data-stu-id="dbe72-141">Property</span></span> | <span data-ttu-id="dbe72-142">Escriba</span><span class="sxs-lookup"><span data-stu-id="dbe72-142">Type</span></span> | <span data-ttu-id="dbe72-143">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="dbe72-143">Description</span></span> | 
|----|----|----|
| <span data-ttu-id="dbe72-144">Tipo</span><span class="sxs-lookup"><span data-stu-id="dbe72-144">type</span></span> | <span data-ttu-id="dbe72-145">string</span><span class="sxs-lookup"><span data-stu-id="dbe72-145">string</span></span> | <span data-ttu-id="dbe72-146">tipo de acción (uno de los valores especificados en la tabla siguiente)</span><span class="sxs-lookup"><span data-stu-id="dbe72-146">type of action (one of the values specified in the table below)</span></span> |
| <span data-ttu-id="dbe72-147">título</span><span class="sxs-lookup"><span data-stu-id="dbe72-147">title</span></span> | <span data-ttu-id="dbe72-148">string</span><span class="sxs-lookup"><span data-stu-id="dbe72-148">string</span></span> | <span data-ttu-id="dbe72-149">título del botón</span><span class="sxs-lookup"><span data-stu-id="dbe72-149">title of the button</span></span> |
| <span data-ttu-id="dbe72-150">imagen</span><span class="sxs-lookup"><span data-stu-id="dbe72-150">image</span></span> | <span data-ttu-id="dbe72-151">string</span><span class="sxs-lookup"><span data-stu-id="dbe72-151">string</span></span> | <span data-ttu-id="dbe72-152">dirección URL de la imagen del botón</span><span class="sxs-lookup"><span data-stu-id="dbe72-152">image URL for the button</span></span> |
| <span data-ttu-id="dbe72-153">value</span><span class="sxs-lookup"><span data-stu-id="dbe72-153">value</span></span> | <span data-ttu-id="dbe72-154">string</span><span class="sxs-lookup"><span data-stu-id="dbe72-154">string</span></span> | <span data-ttu-id="dbe72-155">valor necesario para realizar el tipo de acción especificado</span><span class="sxs-lookup"><span data-stu-id="dbe72-155">value needed to perform the specified type of action</span></span> |

> [!NOTE]
> Los botones dentro de las tarjetas adaptables no se crean mediante el uso de objetos `CardAction`, sino que mediante el esquema que definen las <a href="http://adaptivecards.io" target="_blank">tarjetas adaptables</a>. Consulte [Incorporación de una tarjeta adaptable a un mensaje](#adaptive-card) para ver un ejemplo que muestra cómo agregar botones a una tarjeta adaptable.

<span data-ttu-id="dbe72-158">En esta tabla se enumeran los valores válidos para la propiedad `type` de un objeto de [CardAction][CardAction] y describe el contenido esperado de la propiedad `value` de cada tipo:</span><span class="sxs-lookup"><span data-stu-id="dbe72-158">This table lists the valid values for the `type` property of a [CardAction][CardAction] object and describes the expected contents of the `value` property for each type:</span></span>

| <span data-ttu-id="dbe72-159">Tipo</span><span class="sxs-lookup"><span data-stu-id="dbe72-159">type</span></span> | <span data-ttu-id="dbe72-160">value</span><span class="sxs-lookup"><span data-stu-id="dbe72-160">value</span></span> | 
|----|----|
| <span data-ttu-id="dbe72-161">openUrl</span><span class="sxs-lookup"><span data-stu-id="dbe72-161">openUrl</span></span> | <span data-ttu-id="dbe72-162">Dirección URL que se abrirá en el explorador integrado</span><span class="sxs-lookup"><span data-stu-id="dbe72-162">URL to be opened in the built-in browser</span></span> |
| <span data-ttu-id="dbe72-163">imBack</span><span class="sxs-lookup"><span data-stu-id="dbe72-163">imBack</span></span> | <span data-ttu-id="dbe72-164">Texto del mensaje para enviar al bot (desde el usuario que hizo clic en el botón o que pulsó la tarjeta).</span><span class="sxs-lookup"><span data-stu-id="dbe72-164">Text of the message to send to the bot (from the user who clicked the button or tapped the card).</span></span> <span data-ttu-id="dbe72-165">Este mensaje (del usuario al bot) será visible para todos los participantes de la conversación mediante la aplicación cliente que hospeda la conversación.</span><span class="sxs-lookup"><span data-stu-id="dbe72-165">This message (from user to bot) will be visible to all conversation participants via the client application that is hosting the conversation.</span></span> |
| <span data-ttu-id="dbe72-166">postBack</span><span class="sxs-lookup"><span data-stu-id="dbe72-166">postBack</span></span> | <span data-ttu-id="dbe72-167">Texto del mensaje para enviar al bot (desde el usuario que hizo clic en el botón o que pulsó la tarjeta).</span><span class="sxs-lookup"><span data-stu-id="dbe72-167">Text of the message to send to the bot (from the user who clicked the button or tapped the card).</span></span> <span data-ttu-id="dbe72-168">Algunas aplicaciones cliente pueden mostrar este texto en la fuente del mensaje, donde estará visible para todos los participantes de la conversación.</span><span class="sxs-lookup"><span data-stu-id="dbe72-168">Some client applications may display this text in the message feed, where it will be visible to all conversation participants.</span></span> |
| <span data-ttu-id="dbe72-169">llamada</span><span class="sxs-lookup"><span data-stu-id="dbe72-169">call</span></span> | <span data-ttu-id="dbe72-170">Destino de una llamada telefónica con este formato: **tel:123123123123**</span><span class="sxs-lookup"><span data-stu-id="dbe72-170">Destination for a phone call in this format: **tel:123123123123**</span></span> |
| <span data-ttu-id="dbe72-171">playAudio</span><span class="sxs-lookup"><span data-stu-id="dbe72-171">playAudio</span></span> | <span data-ttu-id="dbe72-172">Dirección URL del audio que se va a reproducir</span><span class="sxs-lookup"><span data-stu-id="dbe72-172">URL of audio to be played</span></span> |
| <span data-ttu-id="dbe72-173">playVideo</span><span class="sxs-lookup"><span data-stu-id="dbe72-173">playVideo</span></span> | <span data-ttu-id="dbe72-174">Dirección URL del vídeo que se va a reproducir</span><span class="sxs-lookup"><span data-stu-id="dbe72-174">URL of video to be played</span></span> |
| <span data-ttu-id="dbe72-175">showImage</span><span class="sxs-lookup"><span data-stu-id="dbe72-175">showImage</span></span> | <span data-ttu-id="dbe72-176">Dirección URL de la imagen que se va a mostrar</span><span class="sxs-lookup"><span data-stu-id="dbe72-176">URL of image to be displayed</span></span> |
| <span data-ttu-id="dbe72-177">DownloadFile</span><span class="sxs-lookup"><span data-stu-id="dbe72-177">downloadFile</span></span> | <span data-ttu-id="dbe72-178">Dirección URL del archivo que se va a descargar</span><span class="sxs-lookup"><span data-stu-id="dbe72-178">URL of file to be downloaded</span></span> |
| <span data-ttu-id="dbe72-179">signin</span><span class="sxs-lookup"><span data-stu-id="dbe72-179">signin</span></span> | <span data-ttu-id="dbe72-180">Dirección URL del flujo de OAuth que se va a iniciar</span><span class="sxs-lookup"><span data-stu-id="dbe72-180">URL of OAuth flow to be initiated</span></span> |

## <a name="add-a-hero-card-to-a-message"></a><span data-ttu-id="dbe72-181">Incorporación de una tarjeta de imagen prominente a un mensaje</span><span class="sxs-lookup"><span data-stu-id="dbe72-181">Add a Hero card to a message</span></span>

<span data-ttu-id="dbe72-182">Para agregar un archivo adjunto de tarjeta enriquecido a un mensaje, cree primero un objeto que corresponda al [tipo de tarjeta](#types-of-cards) que desee agregar al mensaje.</span><span class="sxs-lookup"><span data-stu-id="dbe72-182">To add a rich card attachment to a message, first create an object that corresponds to the [type of card](#types-of-cards) that you want to add to the message.</span></span> <span data-ttu-id="dbe72-183">A continuación, cree un objeto [Attachment][Attachment], establezca su propiedad `contentType` en el tipo de elemento multimedia de la tarjeta y su propiedad `content` como el objeto que ha creado para representar la tarjeta.</span><span class="sxs-lookup"><span data-stu-id="dbe72-183">Then create an [Attachment][Attachment] object, set its `contentType` property to the card's media type and its `content` property to the object you created to represent the card.</span></span> <span data-ttu-id="dbe72-184">Especifique su objeto [Attachment][Attachment] dentro de la matriz `attachments` del mensaje.</span><span class="sxs-lookup"><span data-stu-id="dbe72-184">Specify your [Attachment][Attachment] object within the `attachments` array of the message.</span></span>

> [!TIP]
> Los mensajes que contienen datos adjuntos de tarjeta enriquecida normalmente no especifican `text`.

<span data-ttu-id="dbe72-186">Algunos canales permiten agregar varias tarjetas enriquecidas a la matriz `attachments` dentro de un mensaje.</span><span class="sxs-lookup"><span data-stu-id="dbe72-186">Some channels allow you to add multiple rich cards to the `attachments` array within a message.</span></span> <span data-ttu-id="dbe72-187">Esta funcionalidad puede ser útil en escenarios donde desee proporcionar al usuario varias opciones.</span><span class="sxs-lookup"><span data-stu-id="dbe72-187">This capability can be useful in scenarios where you want to provide the user with multiple options.</span></span> <span data-ttu-id="dbe72-188">Por ejemplo, si el bot permite a los usuarios reservar habitaciones de hotel, podría presentar al usuario una lista de tarjetas enriquecidas que muestre los tipos de habitaciones disponibles.</span><span class="sxs-lookup"><span data-stu-id="dbe72-188">For example, if your bot lets users book hotel rooms, it could present the user with a list of rich cards that shows the types of available rooms.</span></span> <span data-ttu-id="dbe72-189">Cada tarjeta podría contener una imagen y una lista de servicios correspondientes al tipo de habitación y el usuario podría seleccionar una punteando en una tarjeta o haciendo clic en un botón.</span><span class="sxs-lookup"><span data-stu-id="dbe72-189">Each card could contain a picture and list of amenities corresponding to the room type and the user could select a room type by tapping a card or clicking a button.</span></span>

> [!TIP]
> Para mostrar varias tarjetas enriquecidas en formato de lista, establezca la propiedad `attachmentLayout` del objeto [Activity][Activity] en "list" (lista). Para mostrar varias tarjetas enriquecidas en formato de carrusel, establezca la propiedad `attachmentLayout` del objeto [Activity][Activity] en "carousel" (carrusel). Si el canal no admite el formato de carrusel, mostrará las tarjetas enriquecidas en el formato de lista, incluso si la propiedad `attachmentLayout` especifica "carousel" (carrusel).

<span data-ttu-id="dbe72-193">En el ejemplo siguiente se muestra una solicitud que envía un mensaje que contiene datos adjuntos con una sola tarjeta de imagen prominente.</span><span class="sxs-lookup"><span data-stu-id="dbe72-193">The following example shows a request that sends a message containing a single Hero card attachment.</span></span> <span data-ttu-id="dbe72-194">En esta solicitud de ejemplo, `https://smba.trafficmanager.net/apis` representa el URI base; los URI base para las solicitudes que emite el bot pueden ser diferentes.</span><span class="sxs-lookup"><span data-stu-id="dbe72-194">In this example request, `https://smba.trafficmanager.net/apis` represents the base URI; the base URI for requests that your bot issues may be different.</span></span> <span data-ttu-id="dbe72-195">Para más información sobre cómo establecer el URI base, consulte [Referencia de la API](bot-framework-rest-connector-api-reference.md#base-uri).</span><span class="sxs-lookup"><span data-stu-id="dbe72-195">For details about setting the base URI, see [API Reference](bot-framework-rest-connector-api-reference.md#base-uri).</span></span>

```http
POST https://smba.trafficmanager.net/apis/v3/conversations/abcd1234/activities/5d5cdc723 
Authorization: Bearer ACCESS_TOKEN
Content-Type: application/json
```

```json
{
    "type": "message",
    "from": {
        "id": "12345678",
        "name": "sender's name"
    },
    "conversation": {
        "id": "abcd1234",
        "name": "conversation's name"
    },
    "recipient": {
        "id": "1234abcd",
        "name": "recipient's name"
    },
    "attachments": [
        {
            "contentType": "application/vnd.microsoft.card.hero",
            "content": {
                "title": "title goes here",
                "subtitle": "subtitle goes here",
                "text": "descriptive text goes here",
                "images": [
                    {
                        "url": "http://aka.ms/Fo983c",
                        "alt": "picture of a duck",
                        "tap": {
                            "type": "playAudio",
                            "value": "url to an audio track of a duck call goes here"
                        }
                    }
                ],
                "buttons": [
                    {
                        "type": "playAudio",
                        "title": "Duck Call",
                        "value": "url to an audio track of a duck call goes here"
                    },
                    {
                        "type": "openUrl",
                        "title": "Watch Video",
                        "image": "http://aka.ms/Fo983c",
                        "value": "url goes here of the duck in flight"
                    }
                ]
            }
        }
    ],
    "replyToId": "5d5cdc723"
}
```

## <a id="adaptive-card"></a> <span data-ttu-id="dbe72-196">Incorporación de una tarjeta adaptable a un mensaje</span><span class="sxs-lookup"><span data-stu-id="dbe72-196">Add an Adaptive card to a message</span></span>

<span data-ttu-id="dbe72-197">Una tarjeta adaptable puede contener cualquier combinación de texto, voz, imágenes, botones y campos de entrada.</span><span class="sxs-lookup"><span data-stu-id="dbe72-197">The Adaptive Card can contain any combination of text, speech, images, buttons, and input fields.</span></span> <span data-ttu-id="dbe72-198">Las tarjetas adaptables se crean con el formato JSON especificado en <a href="http://adaptivecards.io" target="_blank">Adaptive Cards</a> (Tarjetas adaptables), que proporciona control total sobre el contenido y el formato de la tarjeta.</span><span class="sxs-lookup"><span data-stu-id="dbe72-198">Adaptive Cards are created using the JSON format specified in <a href="http://adaptivecards.io" target="_blank">Adaptive Cards</a>, which gives you full control over card content and format.</span></span> 

<span data-ttu-id="dbe72-199">Use la información del sitio <a href="http://adaptivecards.io" target="_blank">Adaptive Cards</a> (Tarjetas adaptables) a fin de comprender el esquema de la tarjeta adaptable, explorar los elementos de la tarjeta adaptable y ver ejemplos de JSON que puede usar para crear tarjetas de diferente composición y complejidad.</span><span class="sxs-lookup"><span data-stu-id="dbe72-199">Leverage the information within the <a href="http://adaptivecards.io" target="_blank">Adaptive Cards</a> site to understand Adaptive Card schema, explore Adaptive Card elements, and see JSON samples that can be used to create cards of varying composition and complexity.</span></span> <span data-ttu-id="dbe72-200">Además, puede usar el visualizador interactivo para diseñar cargas útiles de la tarjeta adaptable y obtener una vista previa de la salida de la tarjeta.</span><span class="sxs-lookup"><span data-stu-id="dbe72-200">Additionally, you can use the Interactive Visualizer to design Adaptive Card payloads and preview card output.</span></span>

<span data-ttu-id="dbe72-201">En el ejemplo siguiente se muestra una solicitud que envía un mensaje que contiene una tarjeta adaptable para un recordatorio de calendario.</span><span class="sxs-lookup"><span data-stu-id="dbe72-201">The following example shows a request that sends a message containing a single Adaptive Card for a calendar reminder.</span></span> <span data-ttu-id="dbe72-202">En esta solicitud de ejemplo, `https://smba.trafficmanager.net/apis` representa el URI base; los URI base para las solicitudes que emite el bot pueden ser diferentes.</span><span class="sxs-lookup"><span data-stu-id="dbe72-202">In this example request, `https://smba.trafficmanager.net/apis` represents the base URI; the base URI for requests that your bot issues may be different.</span></span> <span data-ttu-id="dbe72-203">Para más información sobre cómo establecer el URI base, consulte [Referencia de la API](bot-framework-rest-connector-api-reference.md#base-uri).</span><span class="sxs-lookup"><span data-stu-id="dbe72-203">For details about setting the base URI, see [API Reference](bot-framework-rest-connector-api-reference.md#base-uri).</span></span>

```http
POST https://smba.trafficmanager.net/apis/v3/conversations/abcd1234/activities/5d5cdc723 
Authorization: Bearer ACCESS_TOKEN
Content-Type: application/json
```

```json
{
    "type": "message",
    "from": {
        "id": "12345678",
        "name": "sender's name"
    },
    "conversation": {
        "id": "abcd1234",
        "name": "conversation's name"
    },
    "recipient": {
        "id": "1234abcd",
        "name": "recipient's name"
    },
    "attachments": [
        {
            "contentType": "application/vnd.microsoft.card.adaptive",
            "content": {
                "type": "AdaptiveCard",
                "body": [
                    {
                        "type": "TextBlock",
                        "text": "Adaptive Card design session",
                        "size": "large",
                        "weight": "bolder"
                    },
                    {
                        "type": "TextBlock",
                        "text": "Conf Room 112/3377 (10)"
                    },
                    {
                        "type": "TextBlock",
                        "text": "12:30 PM - 1:30 PM"
                    },
                    {
                        "type": "TextBlock",
                        "text": "Snooze for"
                    },
                    {
                        "type": "Input.ChoiceSet",
                        "id": "snooze",
                        "style": "compact",
                        "choices": [
                            {
                                "title": "5 minutes",
                                "value": "5",
                                "isSelected": true
                            },
                            {
                                "title": "15 minutes",
                                "value": "15"
                            },
                            {
                                "title": "30 minutes",
                                "value": "30"
                            }
                        ]
                    }
                ],
                "actions": [
                    {
                        "type": "Action.Http",
                        "method": "POST",
                        "url": "http://foo.com",
                        "title": "Snooze"
                    },
                    {
                        "type": "Action.Http",
                        "method": "POST",
                        "url": "http://foo.com",
                        "title": "I'll be late"
                    },
                    {
                        "type": "Action.Http",
                        "method": "POST",
                        "url": "http://foo.com",
                        "title": "Dismiss"
                    }
                ]
            }
        }
    ],
    "replyToId": "5d5cdc723"
}
```

<span data-ttu-id="dbe72-204">La tarjeta resultante contiene tres bloques de texto, un campo de entrada (lista de opciones) y tres botones:</span><span class="sxs-lookup"><span data-stu-id="dbe72-204">The resulting card contains three blocks of text, an input field (choice list), and three buttons:</span></span>

![Recordatorio del calendario de tarjeta adaptable](../media/adaptive-card-reminder.png)


## <a name="additional-resources"></a><span data-ttu-id="dbe72-206">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="dbe72-206">Additional resources</span></span>

- [<span data-ttu-id="dbe72-207">Creación de mensajes</span><span class="sxs-lookup"><span data-stu-id="dbe72-207">Create messages</span></span>](bot-framework-rest-connector-create-messages.md)
- [<span data-ttu-id="dbe72-208">Envío y recepción de mensajes</span><span class="sxs-lookup"><span data-stu-id="dbe72-208">Send and receive messages</span></span>](bot-framework-rest-connector-send-and-receive-messages.md)
- [<span data-ttu-id="dbe72-209">Incorporación de datos adjuntos con elementos multimedia a mensajes</span><span class="sxs-lookup"><span data-stu-id="dbe72-209">Add media attachments to messages</span></span>](bot-framework-rest-connector-add-media-attachments.md)
- <span data-ttu-id="dbe72-210">[Inspector de canales][ChannelInspector]</span><span class="sxs-lookup"><span data-stu-id="dbe72-210">[Channel Inspector][ChannelInspector]</span></span>
- <span data-ttu-id="dbe72-211"><a href="http://adaptivecards.io" target="_blank">Tarjetas adaptables</a></span><span class="sxs-lookup"><span data-stu-id="dbe72-211"><a href="http://adaptivecards.io" target="_blank">Adaptive Cards</a></span></span>

[ChannelInspector]: ../bot-service-channel-inspector.md

[animationCard]: bot-framework-rest-connector-api-reference.md#animationcard-object
[audioCard]: bot-framework-rest-connector-api-reference.md#audiocard-object
[heroCard]: bot-framework-rest-connector-api-reference.md#herocard-object
[thumbnailCard]: bot-framework-rest-connector-api-reference.md#thumbnailcard-object
[receiptCard]: bot-framework-rest-connector-api-reference.md#receiptcard-object
[signinCard]: bot-framework-rest-connector-api-reference.md#signincard-object
[videoCard]: bot-framework-rest-connector-api-reference.md#videocard-object

[CardAction]: bot-framework-rest-connector-api-reference.md#cardaction-object
[Activity]: bot-framework-rest-connector-api-reference.md#activity-object
[Attachment]: bot-framework-rest-connector-api-reference.md#attachment-object