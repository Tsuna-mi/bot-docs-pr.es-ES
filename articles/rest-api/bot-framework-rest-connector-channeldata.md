---
title: Implementar una funcionalidad específica de canal | Microsoft Docs
description: Aprenda a implementar una funcionalidad específica de canal mediante Bot Connector API.
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ms.openlocfilehash: 3cb6f552bee4857d3562e637b2a5728b30ac48a5
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39304956"
---
# <a name="implement-channel-specific-functionality"></a><span data-ttu-id="4d682-103">Implementar una funcionalidad específica de canal</span><span class="sxs-lookup"><span data-stu-id="4d682-103">Implement channel-specific functionality</span></span>

<span data-ttu-id="4d682-104">Algunos canales proporcionan características que no se pueden implementar usando únicamente el [texto y los datos adjuntos del mensaje](bot-framework-rest-connector-create-messages.md).</span><span class="sxs-lookup"><span data-stu-id="4d682-104">Some channels provide features that cannot be implemented by using only [message text and attachments](bot-framework-rest-connector-create-messages.md).</span></span> <span data-ttu-id="4d682-105">Para implementar una funcionalidad específica de canal, puede pasar los metadatos nativos a un canal en la propiedad `channelData` del objeto [Activity][Activity].</span><span class="sxs-lookup"><span data-stu-id="4d682-105">To implement channel-specific functionality, you can pass native metadata to a channel in the [Activity][Activity] object's `channelData` property.</span></span> <span data-ttu-id="4d682-106">Por ejemplo, el bot puede usar la propiedad `channelData` para indicar a Telegram que envíe un adhesivo o para indicar a Office 365 que envíe un correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="4d682-106">For example, your bot can use the `channelData` property to instruct Telegram to send a sticker or to instruct Office365 to send an email.</span></span>

<span data-ttu-id="4d682-107">En este artículo se describe cómo usar la propiedad `channelData` de la actividad de un mensaje para implementar esta funcionalidad específica de canal:</span><span class="sxs-lookup"><span data-stu-id="4d682-107">This article describes how to use a message activity's `channelData` property to implement this channel-specific functionality:</span></span>

| <span data-ttu-id="4d682-108">Canal</span><span class="sxs-lookup"><span data-stu-id="4d682-108">Channel</span></span> | <span data-ttu-id="4d682-109">Funcionalidad</span><span class="sxs-lookup"><span data-stu-id="4d682-109">Functionality</span></span> |
|----|----|
| <span data-ttu-id="4d682-110">Email</span><span class="sxs-lookup"><span data-stu-id="4d682-110">Email</span></span> | <span data-ttu-id="4d682-111">Envío y recepción de un correo electrónico que contiene metadatos de importancia, cuerpo y asunto.</span><span class="sxs-lookup"><span data-stu-id="4d682-111">Send and receive an email that contains body, subject, and importance metadata</span></span> |
| <span data-ttu-id="4d682-112">Slack</span><span class="sxs-lookup"><span data-stu-id="4d682-112">Slack</span></span> | <span data-ttu-id="4d682-113">Envío de mensajes de Slack de plena fidelidad.</span><span class="sxs-lookup"><span data-stu-id="4d682-113">Send full fidelity Slack messages</span></span> |
| <span data-ttu-id="4d682-114">Facebook</span><span class="sxs-lookup"><span data-stu-id="4d682-114">Facebook</span></span> | <span data-ttu-id="4d682-115">Envío de notificaciones de Facebook de forma nativa.</span><span class="sxs-lookup"><span data-stu-id="4d682-115">Send Facebook notifications natively</span></span> |
| <span data-ttu-id="4d682-116">Telegram</span><span class="sxs-lookup"><span data-stu-id="4d682-116">Telegram</span></span> | <span data-ttu-id="4d682-117">Acciones específicas de Telegram, como compartir una nota de voz o un adhesivo.</span><span class="sxs-lookup"><span data-stu-id="4d682-117">Perform Telegram-specific actions, such as sharing a voice memo or a sticker</span></span> |
| <span data-ttu-id="4d682-118">Kik</span><span class="sxs-lookup"><span data-stu-id="4d682-118">Kik</span></span> | <span data-ttu-id="4d682-119">Envío y recepción de mensajes nativos de Kik.</span><span class="sxs-lookup"><span data-stu-id="4d682-119">Send and receive native Kik messages</span></span> | 

> [!NOTE]
> <span data-ttu-id="4d682-120">El valor de la propiedad `channelData` de un objeto [Activity][Activity] es un objeto JSON.</span><span class="sxs-lookup"><span data-stu-id="4d682-120">The value of an [Activity][Activity] object's `channelData` property is a JSON object.</span></span> <span data-ttu-id="4d682-121">La estructura del objeto JSON variará según el canal y la funcionalidad que se implementa, tal como se describe a continuación.</span><span class="sxs-lookup"><span data-stu-id="4d682-121">The structure of the JSON object will vary according to the channel and the functionality being implemented, as described below.</span></span> 

## <a name="create-a-custom-email-message"></a><span data-ttu-id="4d682-122">Crear un mensaje de correo electrónico personalizado</span><span class="sxs-lookup"><span data-stu-id="4d682-122">Create a custom Email message</span></span>

<span data-ttu-id="4d682-123">Para crear un mensaje de correo electrónico, establezca la propiedad `channelData` del objeto [Activity][Activity] en un objeto JSON que contenga estas propiedades:</span><span class="sxs-lookup"><span data-stu-id="4d682-123">To create an email message, set the [Activity][Activity] object's `channelData` property to a JSON object that contains these properties:</span></span>

[!INCLUDE [Email channelData table](~/includes/snippet-channelData-email.md)]

<span data-ttu-id="4d682-124">En este fragmento de código se muestra un ejemplo de la propiedad `channelData` para un mensaje de correo electrónico personalizado.</span><span class="sxs-lookup"><span data-stu-id="4d682-124">This snippet shows an example of the `channelData` property for a custom email message.</span></span>

```json
"channelData":
{
    "htmlBody": "<html><body style = /"font-family: Calibri; font-size: 11pt;/" >This is more than awesome.</body></html>",
    "subject": "Super awesome message subject",
    "importance": "high",
    "ccRecipients": "Yasemin@adatum.com;Temel@adventure-works.com"
}
```

## <a name="create-a-full-fidelity-slack-message"></a><span data-ttu-id="4d682-125">Crear un mensaje de Slack de plena fidelidad</span><span class="sxs-lookup"><span data-stu-id="4d682-125">Create a full-fidelity Slack message</span></span>

<span data-ttu-id="4d682-126">Para crear un mensaje de Slack de plena fidelidad, establezca la propiedad `channelData` del objeto [Activity][Activity] en un objeto JSON que especifique <a href="https://api.slack.com/docs/messages" target="_blank">mensajes de Slack</a>, <a href="https://api.slack.com/docs/message-attachments" target="_blank">elementos adjuntos de Slack</a> o <a href="https://api.slack.com/docs/message-buttons" target="_blank">botones de Slack</a>.</span><span class="sxs-lookup"><span data-stu-id="4d682-126">To create a full-fidelity Slack message, set the [Activity][Activity] object's `channelData` property to a JSON object that specifies <a href="https://api.slack.com/docs/messages" target="_blank">Slack messages</a>, <a href="https://api.slack.com/docs/message-attachments" target="_blank">Slack attachments</a>, and/or <a href="https://api.slack.com/docs/message-buttons" target="_blank">Slack buttons</a>.</span></span> 

> [!NOTE]
> <span data-ttu-id="4d682-127">Para admitir botones en los mensajes de Slack, debe habilitar **Mensajes interactivos** cuando [conecte el bot](../bot-service-manage-channels.md) al canal de Slack.</span><span class="sxs-lookup"><span data-stu-id="4d682-127">To support buttons in Slack messages, you must enable **Interactive Messages** when you [connect your bot](../bot-service-manage-channels.md) to the Slack channel.</span></span>

<span data-ttu-id="4d682-128">En este fragmento de código se muestra un ejemplo de la propiedad `channelData` para un mensaje de Slack personalizado.</span><span class="sxs-lookup"><span data-stu-id="4d682-128">This snippet shows an example of the `channelData` property for a custom Slack message.</span></span>

```json
"channelData": {
   "text": "Now back in stock! :tada:",
   "attachments": [
        {
            "title": "The Further Adventures of Slackbot",
            "author_name": "Stanford S. Strickland",
            "author_icon": "https://api.slack.com/img/api/homepage_custom_integrations-2x.png",
            "image_url": "http://i.imgur.com/OJkaVOI.jpg?1"
        },
        {
            "fields": [
                {
                    "title": "Volume",
                    "value": "1",
                    "short": true
                },
                {
                    "title": "Issue",
                    "value": "3",
                    "short": true
                }
            ]
        },
        {
            "title": "Synopsis",
            "text": "After @episod pushed exciting changes to a devious new branch back in Issue 1, Slackbot notifies @don about an unexpected deploy..."
        },
        {
            "fallback": "Would you recommend it to customers?",
            "title": "Would you recommend it to customers?",
            "callback_id": "comic_1234_xyz",
            "color": "#3AA3E3",
            "attachment_type": "default",
            "actions": [
                {
                    "name": "recommend",
                    "text": "Recommend",
                    "type": "button",
                    "value": "recommend"
                },
                {
                    "name": "no",
                    "text": "No",
                    "type": "button",
                    "value": "bad"
                }
            ]
        }
    ]
}
```

<span data-ttu-id="4d682-129">Cuando un usuario hace clic en un botón dentro de un mensaje de Slack, el bot recibirá un mensaje de respuesta en el que la propiedad `channelData` se rellena con un objeto JSON `payload`.</span><span class="sxs-lookup"><span data-stu-id="4d682-129">When a user clicks a button within a Slack message, your bot will receive a response message in which the `channelData` property is populated with a `payload` JSON object.</span></span> <span data-ttu-id="4d682-130">El objeto `payload` especifica el contenido del mensaje original, identifica el botón donde se hizo clic e identifica al usuario que hizo clic en el botón.</span><span class="sxs-lookup"><span data-stu-id="4d682-130">The `payload` object specifies contents of the original message, identifies the button that was clicked, and identifies the user who clicked the button.</span></span> 

<span data-ttu-id="4d682-131">En este fragmento de código se muestra un ejemplo de la propiedad `channelData` en el mensaje que recibe un bot cuando un usuario hace clic en un botón en el mensaje de Slack.</span><span class="sxs-lookup"><span data-stu-id="4d682-131">This snippet shows an example of the `channelData` property in the message that a bot receives when a user clicks a button in the Slack message.</span></span>

```json
"channelData": {
    "payload": {
        "actions": [
            {
                "name": "recommend",
                "value": "yes"
            }
        ],
        . . .
        "original_message": "{…}",
        "response_url": "https://hooks.slack.com/actions/..."
    }
}
```

<span data-ttu-id="4d682-132">El bot puede responder a este mensaje de la [forma normal](bot-framework-rest-connector-send-and-receive-messages.md#create-reply) o puede registrar su respuesta directamente en punto de conexión que haya especificado la propiedad `response_url` del objeto `payload`.</span><span class="sxs-lookup"><span data-stu-id="4d682-132">Your bot can reply to this message in the [normal manner](bot-framework-rest-connector-send-and-receive-messages.md#create-reply), or it can post its response directly to the endpoint that is specified by the `payload` object's `response_url` property.</span></span> <span data-ttu-id="4d682-133">Para obtener información sobre cuándo y cómo publicar una respuesta en `response_url`, consulte <a href="https://api.slack.com/docs/message-buttons" target="_blank">Botones de Slack</a>.</span><span class="sxs-lookup"><span data-stu-id="4d682-133">For information about when and how to post a response to the `response_url`, see <a href="https://api.slack.com/docs/message-buttons" target="_blank">Slack Buttons</a>.</span></span> 

## <a name="create-a-facebook-notification"></a><span data-ttu-id="4d682-134">Crear una notificación de Facebook</span><span class="sxs-lookup"><span data-stu-id="4d682-134">Create a Facebook notification</span></span>

<span data-ttu-id="4d682-135">Para crear una notificación de Facebook, establezca la propiedad `channelData` del objeto [Activity][Activity] en un objeto JSON que especifique estas propiedades:</span><span class="sxs-lookup"><span data-stu-id="4d682-135">To create a Facebook notification, set the [Activity][Activity] object's `channelData` property to a JSON object that specifies these properties:</span></span> 

| <span data-ttu-id="4d682-136">Propiedad</span><span class="sxs-lookup"><span data-stu-id="4d682-136">Property</span></span> | <span data-ttu-id="4d682-137">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="4d682-137">Description</span></span> |
|----|----|
| <span data-ttu-id="4d682-138">notification_type</span><span class="sxs-lookup"><span data-stu-id="4d682-138">notification_type</span></span> | <span data-ttu-id="4d682-139">El tipo de notificación (por ejemplo, **REGULAR**, **SILENT_PUSH**, **NO_PUSH**).</span><span class="sxs-lookup"><span data-stu-id="4d682-139">The type of notification (e.g., **REGULAR**, **SILENT_PUSH**, **NO_PUSH**).</span></span>
| <span data-ttu-id="4d682-140">attachment</span><span class="sxs-lookup"><span data-stu-id="4d682-140">attachment</span></span> | <span data-ttu-id="4d682-141">Un elemento adjunto que especifica una imagen, un vídeo u otro tipo de elemento multimedia, o un adjunto con plantilla como, por ejemplo, un recibo.</span><span class="sxs-lookup"><span data-stu-id="4d682-141">An attachment that specifies an image, video, or other multimedia type, or a templated attachment such as a receipt.</span></span> |

> [!NOTE]
> <span data-ttu-id="4d682-142">Para obtener más información sobre el formato y el contenido de la propiedad `notification_type` y la propiedad `attachment`, consulte la <a href="https://developers.facebook.com/docs/messenger-platform/send-api-reference#guidelines" target="_blank">documentación de la API de Facebook</a>.</span><span class="sxs-lookup"><span data-stu-id="4d682-142">For details about format and contents of the `notification_type` property and `attachment` property, see the <a href="https://developers.facebook.com/docs/messenger-platform/send-api-reference#guidelines" target="_blank">Facebook API documentation</a>.</span></span> 

<span data-ttu-id="4d682-143">En este fragmento de código se muestra un ejemplo de la propiedad `channelData` para un elemento adjunto con recibo de Facebook.</span><span class="sxs-lookup"><span data-stu-id="4d682-143">This snippet shows an example of the `channelData` property for a Facebook receipt attachment.</span></span>

```json
"channelData": {
    "notification_type": "NO_PUSH",
    "attachment": {
        "type": "template"
        "payload": {
            "template_type": "receipt",
            . . .
        }
    }
}
```

## <a name="create-a-telegram-message"></a><span data-ttu-id="4d682-144">Crear un mensaje de Telegram</span><span class="sxs-lookup"><span data-stu-id="4d682-144">Create a Telegram message</span></span>

<span data-ttu-id="4d682-145">Para crear un mensaje que implemente las acciones específicas de Telegram, como compartir una nota de voz o un sticker, establezca la propiedad `channelData` del objeto [Activity][Activity] en un objeto JSON que especifique estas propiedades:</span><span class="sxs-lookup"><span data-stu-id="4d682-145">To create a message that implements Telegram-specific actions, such as sharing a voice memo or a sticker, set the [Activity][Activity] object's `channelData` property to a JSON object that specifies these properties:</span></span> 

| <span data-ttu-id="4d682-146">Propiedad</span><span class="sxs-lookup"><span data-stu-id="4d682-146">Property</span></span> | <span data-ttu-id="4d682-147">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="4d682-147">Description</span></span> |
|----|----|
| <span data-ttu-id="4d682-148">estático</span><span class="sxs-lookup"><span data-stu-id="4d682-148">method</span></span> | <span data-ttu-id="4d682-149">El método de Telegram Bot API al que se llamará.</span><span class="sxs-lookup"><span data-stu-id="4d682-149">The Telegram Bot API method to call.</span></span> |
| <span data-ttu-id="4d682-150">parameters</span><span class="sxs-lookup"><span data-stu-id="4d682-150">parameters</span></span> | <span data-ttu-id="4d682-151">Los parámetros del método especificado.</span><span class="sxs-lookup"><span data-stu-id="4d682-151">The parameters of the specified method.</span></span> |

<span data-ttu-id="4d682-152">Se admiten los métodos de Telegram siguientes:</span><span class="sxs-lookup"><span data-stu-id="4d682-152">These Telegram methods are supported:</span></span> 

- <span data-ttu-id="4d682-153">answerInlineQuery</span><span class="sxs-lookup"><span data-stu-id="4d682-153">answerInlineQuery</span></span>
- <span data-ttu-id="4d682-154">editMessageCaption</span><span class="sxs-lookup"><span data-stu-id="4d682-154">editMessageCaption</span></span>
- <span data-ttu-id="4d682-155">editMessageReplyMarkup</span><span class="sxs-lookup"><span data-stu-id="4d682-155">editMessageReplyMarkup</span></span>
- <span data-ttu-id="4d682-156">editMessageText</span><span class="sxs-lookup"><span data-stu-id="4d682-156">editMessageText</span></span>
- <span data-ttu-id="4d682-157">forwardMessage</span><span class="sxs-lookup"><span data-stu-id="4d682-157">forwardMessage</span></span>
- <span data-ttu-id="4d682-158">kickChatMember</span><span class="sxs-lookup"><span data-stu-id="4d682-158">kickChatMember</span></span>
- <span data-ttu-id="4d682-159">sendAudio</span><span class="sxs-lookup"><span data-stu-id="4d682-159">sendAudio</span></span>
- <span data-ttu-id="4d682-160">sendChatAction</span><span class="sxs-lookup"><span data-stu-id="4d682-160">sendChatAction</span></span>
- <span data-ttu-id="4d682-161">sendContact</span><span class="sxs-lookup"><span data-stu-id="4d682-161">sendContact</span></span>
- <span data-ttu-id="4d682-162">sendDocument</span><span class="sxs-lookup"><span data-stu-id="4d682-162">sendDocument</span></span>
- <span data-ttu-id="4d682-163">sendLocation</span><span class="sxs-lookup"><span data-stu-id="4d682-163">sendLocation</span></span>
- <span data-ttu-id="4d682-164">sendMessage</span><span class="sxs-lookup"><span data-stu-id="4d682-164">sendMessage</span></span>
- <span data-ttu-id="4d682-165">sendPhoto</span><span class="sxs-lookup"><span data-stu-id="4d682-165">sendPhoto</span></span>
- <span data-ttu-id="4d682-166">sendSticker</span><span class="sxs-lookup"><span data-stu-id="4d682-166">sendSticker</span></span>
- <span data-ttu-id="4d682-167">sendVenue</span><span class="sxs-lookup"><span data-stu-id="4d682-167">sendVenue</span></span>
- <span data-ttu-id="4d682-168">sendVideo</span><span class="sxs-lookup"><span data-stu-id="4d682-168">sendVideo</span></span>
- <span data-ttu-id="4d682-169">sendVoice</span><span class="sxs-lookup"><span data-stu-id="4d682-169">sendVoice</span></span>
- <span data-ttu-id="4d682-170">unbanChateMember</span><span class="sxs-lookup"><span data-stu-id="4d682-170">unbanChateMember</span></span>

<span data-ttu-id="4d682-171">Para obtener más información sobre estos métodos de Telegram y sus parámetros, consulte la <a href="https://core.telegram.org/bots/api#available-methods" target="_blank">documentación de Telegram Bot API</a>.</span><span class="sxs-lookup"><span data-stu-id="4d682-171">For details about these Telegram methods and their parameters, see the <a href="https://core.telegram.org/bots/api#available-methods" target="_blank">Telegram Bot API documentation</a>.</span></span>

> [!NOTE]
> <ul><li><span data-ttu-id="4d682-172">El parámetro <code>chat_id</code> es común a todos los métodos de Telegram.</span><span class="sxs-lookup"><span data-stu-id="4d682-172">The <code>chat_id</code> parameter is common to all Telegram methods.</span></span> <span data-ttu-id="4d682-173">Si no especifica <code>chat_id</code> como parámetro, el marco proporcionará el identificador automáticamente.</span><span class="sxs-lookup"><span data-stu-id="4d682-173">If you do not specify <code>chat_id</code> as a parameter, the framework will provide the ID for you.</span></span></li>
> <li><span data-ttu-id="4d682-174">En lugar de pasar el contenido del archivo insertado, especifique el archivo mediante una dirección URL y el tipo de medio, tal como se muestra en el ejemplo siguiente.</span><span class="sxs-lookup"><span data-stu-id="4d682-174">Instead of passing file contents inline, specify the file using a URL and media type as shown in the example below.</span></span></li>
> <li><span data-ttu-id="4d682-175">Dentro de cada mensaje que recibe su bot del canal de Telegram, la propiedad <code>channelData</code> incluirá el mensaje que su bot envió anteriormente.</span><span class="sxs-lookup"><span data-stu-id="4d682-175">Within each message that your bot receives from the Telegram channel, the <code>channelData</code> property will include the message that your bot sent previously.</span></span></li></ul>

<span data-ttu-id="4d682-176">En este fragmento de código se muestra un ejemplo de una propiedad `channelData` que especifica un único método de Telegram.</span><span class="sxs-lookup"><span data-stu-id="4d682-176">This snippet shows an example of a `channelData` property that specifies a single Telegram method.</span></span>

```json
"channelData": {
    "method": "sendSticker",
    "parameters": {
        "sticker": {
            "url": "https://domain.com/path/gif",
            "mediaType": "image/gif",
        }
    }
}
```

<span data-ttu-id="4d682-177">En este fragmento de código se muestra un ejemplo de una propiedad `channelData` que especifica una matriz de métodos de Telegram.</span><span class="sxs-lookup"><span data-stu-id="4d682-177">This snippet shows an example of a `channelData` property that specifies an array of Telegram methods.</span></span>

```json
"channelData": [
    {
        "method": "sendSticker",
        "parameters": {
            "sticker": {
                "url": "https://domain.com/path/gif",
                "mediaType": "image/gif",
            }
        }
    },
    {
        "method": "sendMessage",
        "parameters": {
            "text": "<b>This message is HTML formatted.</b>",
            "parse_mode": "HTML"
        }
    }
]
```

## <a name="create-a-native-kik-message"></a><span data-ttu-id="4d682-178">Crear un mensaje de Kik nativo</span><span class="sxs-lookup"><span data-stu-id="4d682-178">Create a native Kik message</span></span>

<span data-ttu-id="4d682-179">Para crear un mensaje de Kik nativo, establezca la propiedad `channelData` del objeto [Activity][Activity] en un objeto JSON que especifique esta propiedad:</span><span class="sxs-lookup"><span data-stu-id="4d682-179">To create a native Kik message, set the [Activity][Activity] object's `channelData` property to a JSON object that specifies this property:</span></span> 

| <span data-ttu-id="4d682-180">Propiedad</span><span class="sxs-lookup"><span data-stu-id="4d682-180">Property</span></span> | <span data-ttu-id="4d682-181">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="4d682-181">Description</span></span> |
|----|----|
| <span data-ttu-id="4d682-182">messages</span><span class="sxs-lookup"><span data-stu-id="4d682-182">messages</span></span> | <span data-ttu-id="4d682-183">Una matriz de mensajes de Kik.</span><span class="sxs-lookup"><span data-stu-id="4d682-183">An array of Kik messages.</span></span> <span data-ttu-id="4d682-184">Para obtener más información sobre el formato de los mensajes de Kik, consulte los <a href="https://dev.kik.com/#/docs/messaging#message-formats" target="_blank">formatos de mensaje de Kik</a>.</span><span class="sxs-lookup"><span data-stu-id="4d682-184">For details about Kik message format, see <a href="https://dev.kik.com/#/docs/messaging#message-formats" target="_blank">Kik Message Formats</a>.</span></span> |

<span data-ttu-id="4d682-185">En este fragmento de código se muestra un ejemplo de la propiedad `channelData` para un mensaje de Kik nativo.</span><span class="sxs-lookup"><span data-stu-id="4d682-185">This snippet shows an example of the `channelData` property for a native Kik message.</span></span>

```json
"channelData": {
    "messages": [
        {
            "chatId": "c6dd8165…",
            "type": "link",
            "to": "kikhandle",
            "title": "My Webpage",
            "text": "Some text to display",
            "url": "http://botframework.com",
            "picUrl": "http://lorempixel.com/400/200/",
            "attribution": {
                "name": "My App",
                "iconUrl": "http://lorempixel.com/50/50/"
            },
            "noForward": true,
            "kikJsData": {
                    "key": "value"
                }
        }
    ]
}
```

## <a name="additional-resources"></a><span data-ttu-id="4d682-186">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="4d682-186">Additional resources</span></span>

- [<span data-ttu-id="4d682-187">Introducción a las actividades</span><span class="sxs-lookup"><span data-stu-id="4d682-187">Activities overview</span></span>](bot-framework-rest-connector-activities.md)
- [<span data-ttu-id="4d682-188">Creación de mensajes</span><span class="sxs-lookup"><span data-stu-id="4d682-188">Create messages</span></span>](bot-framework-rest-connector-create-messages.md)
- [<span data-ttu-id="4d682-189">Envío y recepción de mensajes</span><span class="sxs-lookup"><span data-stu-id="4d682-189">Send and receive messages</span></span>](bot-framework-rest-connector-send-and-receive-messages.md)
- <span data-ttu-id="4d682-190">[Preview features with the Channel Inspector](../bot-service-channel-inspector.md) (Vista previa de las características con el Inspector de canales)</span><span class="sxs-lookup"><span data-stu-id="4d682-190">[Preview features with the Channel Inspector](../bot-service-channel-inspector.md)</span></span>

[Activity]: bot-framework-rest-connector-api-reference.md#activity-object
