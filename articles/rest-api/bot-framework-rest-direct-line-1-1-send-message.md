---
title: Envío de un mensaje al bot | Microsoft Docs
description: Obtenga información sobre cómo enviar un mensaje al bot mediante Direct Line API v1.1.
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ms.openlocfilehash: 3bc56d08f45ffd1e389a2dca1868a788d65e087e
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39306109"
---
# <a name="send-a-message-to-the-bot"></a><span data-ttu-id="42bed-103">Envío de un mensaje al bot</span><span class="sxs-lookup"><span data-stu-id="42bed-103">Send a message to the bot</span></span>

> [!IMPORTANT]
> <span data-ttu-id="42bed-104">En este artículo se describe cómo enviar un mensaje al bot mediante Direct Line API v1.1.</span><span class="sxs-lookup"><span data-stu-id="42bed-104">This article describes how to send a message to the bot using Direct Line API 1.1.</span></span> <span data-ttu-id="42bed-105">Si va a crear una conexión entre la aplicación cliente y el bot, use [Direct Line API 3.0](bot-framework-rest-direct-line-3-0-send-activity.md) en su lugar.</span><span class="sxs-lookup"><span data-stu-id="42bed-105">If you are creating a new connection between your client application and bot, use [Direct Line API 3.0](bot-framework-rest-direct-line-3-0-send-activity.md) instead.</span></span>

<span data-ttu-id="42bed-106">Mediante el protocolo de Direct Line 1.1, los clientes pueden intercambiar mensajes con los bots.</span><span class="sxs-lookup"><span data-stu-id="42bed-106">Using the Direct Line 1.1 protocol, clients can exchange messages with bots.</span></span> <span data-ttu-id="42bed-107">Estos mensajes se convierten en el esquema que es compatible con el bot (Bot Framework v1 o Bot Framework v3).</span><span class="sxs-lookup"><span data-stu-id="42bed-107">These messages are converted to the schema that the bot supports (Bot Framework v1 or Bot Framework v3).</span></span> <span data-ttu-id="42bed-108">Un cliente puede enviar un único mensaje por solicitud.</span><span class="sxs-lookup"><span data-stu-id="42bed-108">A client may send a single message per request.</span></span> 

## <a name="send-a-message"></a><span data-ttu-id="42bed-109">Envío de un mensaje</span><span class="sxs-lookup"><span data-stu-id="42bed-109">Send a message</span></span>

<span data-ttu-id="42bed-110">Para enviar un mensaje al bot, el cliente debe crear un objeto [Message](bot-framework-rest-direct-line-1-1-api-reference.md#message-object) para definir el mensaje y, a continuación, emitir una solicitud `POST` a `https://directline.botframework.com/api/conversations/{conversationId}/messages`, especificando el objeto Mensaje en el cuerpo de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="42bed-110">To send a message to the bot, the client must create a [Message](bot-framework-rest-direct-line-1-1-api-reference.md#message-object) object to define the message and then issue a `POST` request to `https://directline.botframework.com/api/conversations/{conversationId}/messages`, specifying the Message object in the body of the request.</span></span>

<span data-ttu-id="42bed-111">Los fragmentos de código siguientes proporcionan un ejemplo de la solicitud de envío del mensaje y la respuesta.</span><span class="sxs-lookup"><span data-stu-id="42bed-111">The following snippets provide an example of the Send Message request and response.</span></span>

### <a name="request"></a><span data-ttu-id="42bed-112">Solicitud</span><span class="sxs-lookup"><span data-stu-id="42bed-112">Request</span></span>

```http
POST https://directline.botframework.com/api/conversations/abc123/messages
Authorization: Bearer RCurR_XV9ZA.cwA.BKA.iaJrC8xpy8qbOF5xnR2vtCX7CZj0LdjAPGfiCpg4Fv0
[other headers]
```

```json
{
  "text": "hello",
  "from": "user1"
}
```

### <a name="response"></a><span data-ttu-id="42bed-113">Response</span><span class="sxs-lookup"><span data-stu-id="42bed-113">Response</span></span>

<span data-ttu-id="42bed-114">Cuando se entrega el mensaje al bot, el servicio responde con un código de estado HTTP que refleja el código de estado del bot.</span><span class="sxs-lookup"><span data-stu-id="42bed-114">When the message is delivered to the bot, the service responds with an HTTP status code that reflects the bot's status code.</span></span> <span data-ttu-id="42bed-115">Si el bot genera un error, se devuelve una respuesta HTTP 500 ("Error interno del servidor") al cliente en respuesta a su solicitud de envío de mensaje.</span><span class="sxs-lookup"><span data-stu-id="42bed-115">If the bot generates an error, an HTTP 500 response ("Internal Server Error") is returned to the client in response to its Send Message request.</span></span> <span data-ttu-id="42bed-116">Si la PUBLICACIÓN se realiza correctamente, el servicio devuelve un código de estado HTTP 204.</span><span class="sxs-lookup"><span data-stu-id="42bed-116">If the POST is successful, the service returns an HTTP 204 status code.</span></span> <span data-ttu-id="42bed-117">En el cuerpo de la respuesta no se devuelve ningún dato.</span><span class="sxs-lookup"><span data-stu-id="42bed-117">No data is returned in body of the response.</span></span> <span data-ttu-id="42bed-118">El mensaje del cliente y los mensajes enviados por el bot pueden obtenerse mediante [sondeo](bot-framework-rest-direct-line-1-1-receive-messages.md).</span><span class="sxs-lookup"><span data-stu-id="42bed-118">The client's message and any messages from the bot can be obtained via [polling](bot-framework-rest-direct-line-1-1-receive-messages.md).</span></span> 

```http
HTTP/1.1 204 No Content
[other headers]
```

### <a name="total-time-for-the-send-message-requestresponse"></a><span data-ttu-id="42bed-119">Tiempo total para la solicitud de envío de mensaje/respuesta</span><span class="sxs-lookup"><span data-stu-id="42bed-119">Total time for the Send Message request/response</span></span>

<span data-ttu-id="42bed-120">El tiempo total para PUBLICAR un mensaje en una conversación de Direct Line es la suma de lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="42bed-120">The total time to POST a message to a Direct Line conversation is the sum of the following:</span></span>

- <span data-ttu-id="42bed-121">Tiempo de tránsito de la solicitud HTTP para desplazarse desde el cliente al servicio Direct Line</span><span class="sxs-lookup"><span data-stu-id="42bed-121">Transit time for the HTTP request to travel from the client to the Direct Line service</span></span>
- <span data-ttu-id="42bed-122">Tiempo de procesamiento interno en Direct Line (normalmente menos de 120 ms)</span><span class="sxs-lookup"><span data-stu-id="42bed-122">Internal processing time within Direct Line (typically less than 120ms)</span></span>
- <span data-ttu-id="42bed-123">Tiempo de tránsito desde el servicio Direct Line al bot</span><span class="sxs-lookup"><span data-stu-id="42bed-123">Transit time from the Direct Line service to the bot</span></span>
- <span data-ttu-id="42bed-124">Tiempo de procesamiento en el bot</span><span class="sxs-lookup"><span data-stu-id="42bed-124">Processing time within the bot</span></span>
- <span data-ttu-id="42bed-125">Tiempo de tránsito de la respuesta HTTP hasta el cliente</span><span class="sxs-lookup"><span data-stu-id="42bed-125">Transit time for the HTTP response to travel back to the client</span></span>

## <a name="send-attachments-to-the-bot"></a><span data-ttu-id="42bed-126">Envío de datos adjuntos al bot</span><span class="sxs-lookup"><span data-stu-id="42bed-126">Send attachment(s) to the bot</span></span>

<span data-ttu-id="42bed-127">En algunas situaciones, un cliente debe enviar datos adjuntos al bot, como imágenes o documentos.</span><span class="sxs-lookup"><span data-stu-id="42bed-127">In some situations, a client may need to send attachments to the bot such as images or documents.</span></span> <span data-ttu-id="42bed-128">Un cliente puede enviar datos adjuntos al bot ya sea [especificando las direcciones URL](#send-by-url) de los datos adjuntos en el objeto [Message](bot-framework-rest-direct-line-1-1-api-reference.md#message-object) que envía mediante `POST /api/conversations/{conversationId}/messages` o [cargando datos adjuntos](#upload-attachments) mediante `POST /api/conversations/{conversationId}/upload`.</span><span class="sxs-lookup"><span data-stu-id="42bed-128">A client may send attachments to the bot either by [specifying the URL(s)](#send-by-url) of the attachment(s) within the [Message](bot-framework-rest-direct-line-1-1-api-reference.md#message-object) object that it sends using `POST /api/conversations/{conversationId}/messages` or by [uploading attachment(s)](#upload-attachments) using `POST /api/conversations/{conversationId}/upload`.</span></span>

## <a id="send-by-url"></a> <span data-ttu-id="42bed-129">Envío de datos adjuntos por dirección URL</span><span class="sxs-lookup"><span data-stu-id="42bed-129">Send attachment(s) by URL</span></span>

<span data-ttu-id="42bed-130">Para enviar uno o varios datos adjuntos como pate del objeto [Message](bot-framework-rest-direct-line-1-1-api-reference.md#message-object) con el uso de `POST /api/conversations/{conversationId}/messages`, especifique las direcciones URL de los datos adjuntos en las matrices `images` o `attachments` del mensaje.</span><span class="sxs-lookup"><span data-stu-id="42bed-130">To send one or more attachments as part of the [Message](bot-framework-rest-direct-line-1-1-api-reference.md#message-object) object using `POST /api/conversations/{conversationId}/messages`, specify the attachment URL(s) within the message's `images` array and/or `attachments` array.</span></span>

## <a id="upload-attachments"></a> <span data-ttu-id="42bed-131">Envío de datos adjuntos mediante carga</span><span class="sxs-lookup"><span data-stu-id="42bed-131">Send attachment(s) by upload</span></span>

<span data-ttu-id="42bed-132">A menudo, un cliente puede tener imágenes o documentos en un dispositivo que desea enviar al bot, pero no hay direcciones URL correspondientes a esos archivos.</span><span class="sxs-lookup"><span data-stu-id="42bed-132">Often, a client may have image(s) or document(s) on a device that it wants to send to the bot, but no URLs corresponding to those files.</span></span> <span data-ttu-id="42bed-133">En esta situación, un cliente puede emitir una solicitud `POST /api/conversations/{conversationId}/upload` para enviar datos adjuntos al bot mediante carga.</span><span class="sxs-lookup"><span data-stu-id="42bed-133">In this situation, a client can can issue a `POST /api/conversations/{conversationId}/upload` request to send attachments to the bot by upload.</span></span> <span data-ttu-id="42bed-134">El formato y el contenido de la solicitud dependerán de si el cliente [envía un único dato adjunto](#upload-one-attachment) o [varios datos adjuntos](#upload-multiple-attachments).</span><span class="sxs-lookup"><span data-stu-id="42bed-134">The format and contents of the request will depend upon whether the client is [sending a single attachment](#upload-one-attachment) or [sending multiple attachments](#upload-multiple-attachments).</span></span>

### <a id="upload-one-attachment"></a> <span data-ttu-id="42bed-135">Envío de un único dato adjunto mediante carga</span><span class="sxs-lookup"><span data-stu-id="42bed-135">Send a single attachment by upload</span></span>

<span data-ttu-id="42bed-136">Para enviar un único dato adjunto mediante carga, emita esta solicitud:</span><span class="sxs-lookup"><span data-stu-id="42bed-136">To send a single attachment by upload, issue this request:</span></span> 

```http
POST https://directline.botframework.com/api/conversations/{conversationId}/upload?userId={userId}
Authorization: Bearer SECRET_OR_TOKEN
Content-Type: TYPE_OF_ATTACHMENT
Content-Disposition: ATTACHMENT_INFO
[other headers]

[file content]
```

<span data-ttu-id="42bed-137">En este URI de solicitud, reemplace **{conversationId}** con el identificador de la conversación y **{userId}** con el identificador del usuario que envía el mensaje.</span><span class="sxs-lookup"><span data-stu-id="42bed-137">In this request URI, replace **{conversationId}** with the ID of the conversation and **{userId}** with the ID of the user that is sending the message.</span></span> <span data-ttu-id="42bed-138">En los encabezados de solicitud, establezca `Content-Type` para especificar el tipo de datos adjuntos y defina `Content-Disposition` para especificar el nombre de archivo de los datos adjuntos.</span><span class="sxs-lookup"><span data-stu-id="42bed-138">In the request headers, set `Content-Type` to specify the attachment's type and set `Content-Disposition` to specify the attachment's filename.</span></span>

<span data-ttu-id="42bed-139">Los fragmentos de código siguientes proporcionan un ejemplo de la solicitud de envío de un único dato adjunto y la respuesta.</span><span class="sxs-lookup"><span data-stu-id="42bed-139">The following snippets provide an example of the Send (single) Attachment request and response.</span></span>

#### <a name="request"></a><span data-ttu-id="42bed-140">Solicitud</span><span class="sxs-lookup"><span data-stu-id="42bed-140">Request</span></span>

```http
POST https://directline.botframework.com/api/conversations/abc123/upload?userId=user1
Authorization: Bearer RCurR_XV9ZA.cwA.BKA.iaJrC8xpy8qbOF5xnR2vtCX7CZj0LdjAPGfiCpg4Fv0
Content-Type: image/jpeg
Content-Disposition: name="file"; filename="badjokeeel.jpg"
[other headers]

[JPEG content]
```

#### <a name="response"></a><span data-ttu-id="42bed-141">Response</span><span class="sxs-lookup"><span data-stu-id="42bed-141">Response</span></span>

<span data-ttu-id="42bed-142">Si la solicitud es correcta, se envía un mensaje al bot cuando se completa la carga y el servicio devuelve un código de estado HTTP 204.</span><span class="sxs-lookup"><span data-stu-id="42bed-142">If the request is successful, a message is sent to the bot when the upload completes and the service returns an HTTP 204 status code.</span></span>

```http
HTTP/1.1 204 No Content
[other headers]
```

### <a id="upload-multiple-attachments"></a> <span data-ttu-id="42bed-143">Envío de varios datos adjuntos mediante carga</span><span class="sxs-lookup"><span data-stu-id="42bed-143">Send multiple attachments by upload</span></span>

<span data-ttu-id="42bed-144">Para enviar varios datos adjuntos mediante carga, emita una solicitud `POST` de varias partes al punto de conexión `/api/conversations/{conversationId}/upload`.</span><span class="sxs-lookup"><span data-stu-id="42bed-144">To send multiple attachments by upload, `POST` a multipart request to the `/api/conversations/{conversationId}/upload` endpoint.</span></span> <span data-ttu-id="42bed-145">Establezca el encabezado `Content-Type` de la solicitud en `multipart/form-data` e incluya el encabezado `Content-Type` y el encabezado `Content-Disposition` para que cada parte especifique el tipo y el nombre de archivo de los datos adjuntos.</span><span class="sxs-lookup"><span data-stu-id="42bed-145">Set the `Content-Type` header of the request to `multipart/form-data` and include the `Content-Type` header and `Content-Disposition` header for each part to specify each attachment's type and filename.</span></span> <span data-ttu-id="42bed-146">En el URI de solicitud, establezca el parámetro `userId` como el identificador del usuario que envía el mensaje.</span><span class="sxs-lookup"><span data-stu-id="42bed-146">In the request URI, set the `userId` parameter to the ID of the user that is sending the message.</span></span> 

<span data-ttu-id="42bed-147">Puede incluir un objeto [Message](bot-framework-rest-direct-line-1-1-api-reference.md#message-object) en la solicitud con la adición de una parte que especifica como encabezado `Content-Type` el valor `application/vnd.microsoft.bot.message`.</span><span class="sxs-lookup"><span data-stu-id="42bed-147">You may include a [Message](bot-framework-rest-direct-line-1-1-api-reference.md#message-object) object within the request by adding a part that specifies the `Content-Type` header value `application/vnd.microsoft.bot.message`.</span></span> <span data-ttu-id="42bed-148">Esto permite al cliente personalizar el mensaje que contiene los datos adjuntos.</span><span class="sxs-lookup"><span data-stu-id="42bed-148">This allows the client to customize the message that contains the attachment(s).</span></span> <span data-ttu-id="42bed-149">Si la solicitud incluye un mensaje, los datos adjuntos especificados por otras partes de la carga se agregan como datos adjuntos a ese mensaje antes de enviarlo.</span><span class="sxs-lookup"><span data-stu-id="42bed-149">If the request includes a Message, the attachments that are specified by other parts of the payload are added as attachments to that Message before it is sent.</span></span> 

<span data-ttu-id="42bed-150">Los fragmentos de código siguientes proporcionan un ejemplo de la solicitud de envío de varios datos adjuntos y la respuesta.</span><span class="sxs-lookup"><span data-stu-id="42bed-150">The following snippets provide an example of the Send (multiple) Attachments request and response.</span></span> <span data-ttu-id="42bed-151">En este ejemplo, la solicitud envía un mensaje que contiene texto y un único dato adjunto de imagen.</span><span class="sxs-lookup"><span data-stu-id="42bed-151">In this example, the request sends a message that contains some text and a single image attachment.</span></span> <span data-ttu-id="42bed-152">Se pueden agregar partes adicionales a la solicitud para incluir varios datos adjuntos en este mensaje.</span><span class="sxs-lookup"><span data-stu-id="42bed-152">Additional parts could be added to the request to include multiple attachments in this message.</span></span>

#### <a name="request"></a><span data-ttu-id="42bed-153">Solicitud</span><span class="sxs-lookup"><span data-stu-id="42bed-153">Request</span></span>

```http
POST https://directline.botframework.com/api/conversations/abc123/upload?userId=user1
Authorization: Bearer RCurR_XV9ZA.cwA.BKA.iaJrC8xpy8qbOF5xnR2vtCX7CZj0LdjAPGfiCpg4Fv0
Content-Type: multipart/form-data; boundary=----DD4E5147-E865-4652-B662-F223701A8A89
[other headers]

----DD4E5147-E865-4652-B662-F223701A8A89
Content-Type: image/jpeg
Content-Disposition: form-data; name="file"; filename="badjokeeel.jpg"
[other headers]

[JPEG content]

----DD4E5147-E865-4652-B662-F223701A8A89
Content-Type: application/vnd.microsoft.bot.message
[other headers]

{
  "text": "Hey I just IM'd you\n\nand this is crazy\n\nbut here's my webhook\n\nso POST me maybe",
  "from": "user1"
}

----DD4E5147-E865-4652-B662-F223701A8A89
```

#### <a name="response"></a><span data-ttu-id="42bed-154">Response</span><span class="sxs-lookup"><span data-stu-id="42bed-154">Response</span></span>

<span data-ttu-id="42bed-155">Si la solicitud es correcta, se envía un mensaje al bot cuando se completa la carga y el servicio devuelve un código de estado HTTP 204.</span><span class="sxs-lookup"><span data-stu-id="42bed-155">If the request is successful, a message is sent to the bot when the upload completes and the service returns an HTTP 204 status code.</span></span>

```http
HTTP/1.1 204 No Content
[other headers]
```

## <a name="additional-resources"></a><span data-ttu-id="42bed-156">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="42bed-156">Additional resources</span></span>

- [<span data-ttu-id="42bed-157">Conceptos clave</span><span class="sxs-lookup"><span data-stu-id="42bed-157">Key concepts</span></span>](bot-framework-rest-direct-line-1-1-concepts.md)
- [<span data-ttu-id="42bed-158">Autenticación</span><span class="sxs-lookup"><span data-stu-id="42bed-158">Authentication</span></span>](bot-framework-rest-direct-line-1-1-authentication.md)
- [<span data-ttu-id="42bed-159">Inicio de una conversación</span><span class="sxs-lookup"><span data-stu-id="42bed-159">Start a conversation</span></span>](bot-framework-rest-direct-line-1-1-start-conversation.md)
- [<span data-ttu-id="42bed-160">Recepción de mensajes del bot</span><span class="sxs-lookup"><span data-stu-id="42bed-160">Receive messages from the bot</span></span>](bot-framework-rest-direct-line-1-1-receive-messages.md)