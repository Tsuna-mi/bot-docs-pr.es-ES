---
title: Incorporación de datos adjuntos de elementos multimedia a mensajes | Microsoft Docs
description: Aprenda a agregar datos adjuntos de elementos multimedia a mensajes mediante el servicio Bot Connector.
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ms.openlocfilehash: 39247ec3be4da7129989041269e930de8fa766ae
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305305"
---
# <a name="add-media-attachments-to-messages"></a><span data-ttu-id="111e5-103">Incorporación de datos adjuntos de elementos multimedia a mensajes</span><span class="sxs-lookup"><span data-stu-id="111e5-103">Add media attachments to messages</span></span>
> [!div class="op_single_selector"]
> - [.NET](../dotnet/bot-builder-dotnet-add-media-attachments.md)
> - [Node.js](../nodejs/bot-builder-nodejs-send-receive-attachments.md)
> - [REST](../rest-api/bot-framework-rest-connector-add-media-attachments.md)

<span data-ttu-id="111e5-107">Los bots y los canales intercambian normalmente cadenas de texto, pero algunos canales también admiten el intercambio de datos adjuntos, lo que permite a su bot enviar mensajes más completos a los usuarios.</span><span class="sxs-lookup"><span data-stu-id="111e5-107">Bots and channels typically exchange text strings but some channels also support exchanging attachments, which lets your bot send richer messages to users.</span></span> <span data-ttu-id="111e5-108">Por ejemplo, el bot puede enviar datos adjuntos de elementos multimedia (como imágenes, vídeos, audio, archivos) y [tarjetas enriquecidas](bot-framework-rest-connector-add-rich-cards.md).</span><span class="sxs-lookup"><span data-stu-id="111e5-108">For example, your bot can send media attachments (e.g., images, videos, audio, files) and [rich cards](bot-framework-rest-connector-add-rich-cards.md).</span></span> <span data-ttu-id="111e5-109">En este artículo se describe cómo agregar datos adjuntos de elementos multimedia a mensajes mediante el servicio Bot Connector.</span><span class="sxs-lookup"><span data-stu-id="111e5-109">This article describes how to add media attachments to messages using the Bot Connector service.</span></span>

> [!TIP]
> Para determinar el tipo y el número de datos adjuntos que admite un canal, y cómo el canal representa los datos adjuntos, consulte [Channel Inspector][ChannelInspector].

## <a name="add-a-media-attachment"></a><span data-ttu-id="111e5-111">Incorporación de datos adjuntos de elementos multimedia</span><span class="sxs-lookup"><span data-stu-id="111e5-111">Add a media attachment</span></span>  

<span data-ttu-id="111e5-112">Para agregar datos adjuntos de elementos multimedia a un mensaje, cree un objeto [Attachment][Attachment], establezca la propiedad `name`, establezca la propiedad `contentUrl` en la dirección URL del archivo multimedia y establezca la propiedad `contentType` en el tipo de elemento multimedia adecuado (por ejemplo, **imagen/png**, **audio/wav**, **vídeo/mp4**).</span><span class="sxs-lookup"><span data-stu-id="111e5-112">To add a media attachment to a message, create an [Attachment][Attachment] object, set the `name` property, set the `contentUrl` property to the URL of the media file, and set the `contentType` property to the appropriate media type (e.g., **image/png**, **audio/wav**, **video/mp4**).</span></span> <span data-ttu-id="111e5-113">A continuación, en el objeto [Activity][Activity] que representa el mensaje, especifique el objeto [Attachment][Attachment] dentro de la matriz `attachments`.</span><span class="sxs-lookup"><span data-stu-id="111e5-113">Then within the [Activity][Activity] object that represents your message, specify your [Attachment][Attachment] object within the `attachments` array.</span></span> 

<span data-ttu-id="111e5-114">En el ejemplo siguiente se muestra una solicitud que envía un mensaje que contiene texto y datos adjuntos con una sola imagen.</span><span class="sxs-lookup"><span data-stu-id="111e5-114">The following example shows a request that sends a message containing text and a single image attachment.</span></span> <span data-ttu-id="111e5-115">En esta solicitud de ejemplo, `https://smba.trafficmanager.net/apis` representa el URI base; los URI base para las solicitudes que emite el bot pueden ser diferentes.</span><span class="sxs-lookup"><span data-stu-id="111e5-115">In this example request, `https://smba.trafficmanager.net/apis` represents the base URI; the base URI for requests that your bot issues may be different.</span></span> <span data-ttu-id="111e5-116">Para más información sobre cómo establecer el URI base, consulte [Referencia de la API](bot-framework-rest-connector-api-reference.md#base-uri).</span><span class="sxs-lookup"><span data-stu-id="111e5-116">For details about setting the base URI, see [API Reference](bot-framework-rest-connector-api-reference.md#base-uri).</span></span>

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
    "text": "Here's a picture of the duck I was telling you about.",
    "attachments": [
        {
            "contentType": "image/png",
            "contentUrl": "http://aka.ms/Fo983c",
            "name": "duck-on-a-rock.jpg"
        }
    ],
    "replyToId": "5d5cdc723"
}
```

<span data-ttu-id="111e5-117">En el caso de canales que admiten datos binarios en línea de una imagen, puede establecer la propiedad `contentUrl` del objeto `Attachment` en un archivo binario de base64 de la imagen (por ejemplo, **datos:imagen/png;base64, iVBORw0KGgo...** ).</span><span class="sxs-lookup"><span data-stu-id="111e5-117">For channels that support inline binaries of an image, you can set the `contentUrl` property of the `Attachment` to a base64 binary of the image (for example, **data:image/png;base64,iVBORw0KGgo…**).</span></span> <span data-ttu-id="111e5-118">El canal mostrará la imagen o la dirección URL de la imagen junto a la cadena de texto del mensaje.</span><span class="sxs-lookup"><span data-stu-id="111e5-118">The channel will display the image or the image's URL next to the message's text string.</span></span>

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
    "text": "Here's a picture of the duck I was telling you about.",
    "attachments": [
        {
            "contentType": "image/png",
            "contentUrl": "data:image/png;base64,iVBORw0KGgo…",
            "name": "duck-on-a-rock.jpg"
        }
    ],
    "replyToId": "5d5cdc723"
}
```

<span data-ttu-id="111e5-119">Puede adjuntar un archivo de vídeo o de audio a un mensaje siguiendo el mismo proceso descrito anteriormente para un archivo de imagen.</span><span class="sxs-lookup"><span data-stu-id="111e5-119">You can attach a video file or audio file to a message by using the same process as described above for an image file.</span></span> <span data-ttu-id="111e5-120">Dependiendo del canal, el vídeo y el audio pueden reproducirse en línea o se pueden mostrar como un vínculo.</span><span class="sxs-lookup"><span data-stu-id="111e5-120">Depending on the channel, the video and audio may be played inline or it may be displayed as a link.</span></span>

> [!NOTE] 
> Su bot también puede recibir mensajes que contengan datos adjuntos de elementos multimedia.
> Por ejemplo, un mensaje que recibe el bot puede contener datos adjuntos si el canal permite al usuario cargar una foto para analizar o un documento para almacenar.

## <a name="add-an-audiocard-attachment"></a><span data-ttu-id="111e5-123">Incorporación de datos adjuntos de tarjeta de audio</span><span class="sxs-lookup"><span data-stu-id="111e5-123">Add an AudioCard attachment</span></span>

<span data-ttu-id="111e5-124">Agregar datos adjuntos de [tarjeta de audio](bot-framework-rest-connector-api-reference.md#audiocard-object) o [tarjeta de vídeo](bot-framework-rest-connector-api-reference.md#videocard-object) es lo mismo que agregar datos adjuntos de elementos multimedia.</span><span class="sxs-lookup"><span data-stu-id="111e5-124">Adding an [AudioCard](bot-framework-rest-connector-api-reference.md#audiocard-object) or [VideoCard](bot-framework-rest-connector-api-reference.md#videocard-object) attachment is the same as adding a media attachment.</span></span> <span data-ttu-id="111e5-125">Por ejemplo, el siguiente código JSON muestra cómo agregar una tarjeta de audio en los datos adjuntos de elementos multimedia.</span><span class="sxs-lookup"><span data-stu-id="111e5-125">For example, the following JSON shows how to add an audio card in the media attachment.</span></span>

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
      "contentType": "application/vnd.microsoft.card.audio",
      "content": {
        "title": "Allegro in C Major",
        "subtitle": "Allegro Duet",
        "text": "No Image, No Buttons, Autoloop, Autostart, Sharable",
        "media": [
          {
            "url": "https://contoso.com/media/AllegrofromDuetinCMajor.mp3"
          }
        ],
        "shareable": true,
        "autoloop": true,
        "autostart": true,
        "value": {
            // Supplementary parameter for this card
        }
      }
    }],
    "replyToId": "5d5cdc723"
}
```

<span data-ttu-id="111e5-126">Una vez que el canal recibe estos datos adjuntos, comenzará a reproducirse el archivo de audio.</span><span class="sxs-lookup"><span data-stu-id="111e5-126">Once the channel receives this attachment, it will start playing the audio file.</span></span> <span data-ttu-id="111e5-127">Si un usuario interactúa con el audio y hace clic en el botón **Pausar**, por ejemplo, el canal enviará una devolución de llamada al bot con un código JSON similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="111e5-127">If a user interacts with audio by clicking the **Pause** button, for example, the channel will send a callback to the bot with a JSON that look something like this:</span></span>

```json
{
    ...
    "type": "event",
    "name": "media/pause",
    "value": {
        "url": // URL for media
        "cardValue": {
            // Supplementary parameter for this card
        }
    }
}
```

<span data-ttu-id="111e5-128">El nombre del evento multimedia **media/pause** aparecerá en el campo `activity.name`.</span><span class="sxs-lookup"><span data-stu-id="111e5-128">The media event name **media/pause** will appear in the `activity.name` field.</span></span> <span data-ttu-id="111e5-129">Tome como referencia la tabla siguiente para obtener una lista de todos los nombres de eventos multimedia.</span><span class="sxs-lookup"><span data-stu-id="111e5-129">Reference the below table for a list of all media event names.</span></span>

| <span data-ttu-id="111e5-130">Evento</span><span class="sxs-lookup"><span data-stu-id="111e5-130">Event</span></span> | <span data-ttu-id="111e5-131">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="111e5-131">Description</span></span> |
| ---- | ---- |
| <span data-ttu-id="111e5-132">**media/next**</span><span class="sxs-lookup"><span data-stu-id="111e5-132">**media/next**</span></span> | <span data-ttu-id="111e5-133">El cliente saltó al siguiente elemento multimedia</span><span class="sxs-lookup"><span data-stu-id="111e5-133">The client skipped to the next media</span></span> |
| <span data-ttu-id="111e5-134">**media/pause**</span><span class="sxs-lookup"><span data-stu-id="111e5-134">**media/pause**</span></span> | <span data-ttu-id="111e5-135">El cliente pausó la reproducción del elemento multimedia</span><span class="sxs-lookup"><span data-stu-id="111e5-135">The client paused playing media</span></span> |
| <span data-ttu-id="111e5-136">**media/play**</span><span class="sxs-lookup"><span data-stu-id="111e5-136">**media/play**</span></span> | <span data-ttu-id="111e5-137">El cliente inició la reproducción del elemento multimedia</span><span class="sxs-lookup"><span data-stu-id="111e5-137">The client began playing media</span></span> |
| <span data-ttu-id="111e5-138">**media/previous**</span><span class="sxs-lookup"><span data-stu-id="111e5-138">**media/previous**</span></span> | <span data-ttu-id="111e5-139">El cliente saltó al elemento multimedia anterior</span><span class="sxs-lookup"><span data-stu-id="111e5-139">The client skipped to the previous media</span></span> |
| <span data-ttu-id="111e5-140">**media/resume**</span><span class="sxs-lookup"><span data-stu-id="111e5-140">**media/resume**</span></span> | <span data-ttu-id="111e5-141">El cliente reanudó la reproducción del elemento multimedia</span><span class="sxs-lookup"><span data-stu-id="111e5-141">The client resumed playing media</span></span> |
| <span data-ttu-id="111e5-142">**media/stop**</span><span class="sxs-lookup"><span data-stu-id="111e5-142">**media/stop**</span></span> | <span data-ttu-id="111e5-143">El cliente detuvo la reproducción del elemento multimedia</span><span class="sxs-lookup"><span data-stu-id="111e5-143">The client stopped playing media</span></span> |

## <a name="additional-resources"></a><span data-ttu-id="111e5-144">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="111e5-144">Additional resources</span></span>

- [<span data-ttu-id="111e5-145">Creación de mensajes</span><span class="sxs-lookup"><span data-stu-id="111e5-145">Create messages</span></span>](bot-framework-rest-connector-create-messages.md)
- [<span data-ttu-id="111e5-146">Envío y recepción de mensajes</span><span class="sxs-lookup"><span data-stu-id="111e5-146">Send and receive messages</span></span>](bot-framework-rest-connector-send-and-receive-messages.md)
- [<span data-ttu-id="111e5-147">Incorporación de tarjetas enriquecidas a mensajes</span><span class="sxs-lookup"><span data-stu-id="111e5-147">Add rich cards to messages</span></span>](bot-framework-rest-connector-add-rich-cards.md)
- <span data-ttu-id="111e5-148">[Channel Inspector][ChannelInspector]</span><span class="sxs-lookup"><span data-stu-id="111e5-148">[Channel Inspector][ChannelInspector]</span></span>

[ChannelInspector]: ../bot-service-channel-inspector.md

[Activity]: bot-framework-rest-connector-api-reference.md#activity-object
[Attachment]: bot-framework-rest-connector-api-reference.md#attachment-object
