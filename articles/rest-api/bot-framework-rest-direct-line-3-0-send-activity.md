---
title: Envío de una actividad al bot | Microsoft Docs
description: Obtenga información sobre cómo enviar una actividad al bot con Direct Line API v3.0.
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ms.openlocfilehash: 3f881f353f04be95ce3785c2fd82b724dd58cb88
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305561"
---
# <a name="send-an-activity-to-the-bot"></a><span data-ttu-id="bcd1d-103">Envío de una actividad al bot</span><span class="sxs-lookup"><span data-stu-id="bcd1d-103">Send an activity to the bot</span></span>

<span data-ttu-id="bcd1d-104">Mediante el protocolo Direct Line 3.0, los clientes y los bots pueden intercambiar varios tipos diferentes de [actividades](bot-framework-rest-connector-activities.md), incluidas las actividades de **mensaje**, las de **escritura** y las actividades personalizadas que admita el bot.</span><span class="sxs-lookup"><span data-stu-id="bcd1d-104">Using the Direct Line 3.0 protocol, clients and bots may exchange several different types of [activities](bot-framework-rest-connector-activities.md), including **message** activities, **typing** activities, and custom activities that the bot supports.</span></span> <span data-ttu-id="bcd1d-105">Un cliente puede enviar una sola actividad por solicitud.</span><span class="sxs-lookup"><span data-stu-id="bcd1d-105">A client may send a single activity per request.</span></span> 

## <a name="send-an-activity"></a><span data-ttu-id="bcd1d-106">Envío de una actividad</span><span class="sxs-lookup"><span data-stu-id="bcd1d-106">Send an activity</span></span>

<span data-ttu-id="bcd1d-107">Para enviar una actividad al bot, el cliente debe crear un objeto [Activity](bot-framework-rest-connector-api-reference.md#activity-object) para definir la actividad y, a continuación, emitir una solicitud `POST` a `https://directline.botframework.com/v3/directline/conversations/{conversationId}/activities`, especificando el objeto Activity en el cuerpo de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="bcd1d-107">To send an activity to the bot, the client must create an [Activity](bot-framework-rest-connector-api-reference.md#activity-object) object to define the activity and then issue a `POST` request to `https://directline.botframework.com/v3/directline/conversations/{conversationId}/activities`, specifying the Activity object in the body of the request.</span></span>

<span data-ttu-id="bcd1d-108">Los fragmentos de código siguientes proporcionan un ejemplo de la solicitud de envío de la actividad y la respuesta.</span><span class="sxs-lookup"><span data-stu-id="bcd1d-108">The following snippets provide an example of the Send Activity request and response.</span></span>

### <a name="request"></a><span data-ttu-id="bcd1d-109">Solicitud</span><span class="sxs-lookup"><span data-stu-id="bcd1d-109">Request</span></span>

```http
POST https://directline.botframework.com/v3/directline/conversations/abc123/activities
Authorization: Bearer RCurR_XV9ZA.cwA.BKA.iaJrC8xpy8qbOF5xnR2vtCX7CZj0LdjAPGfiCpg4Fv0
Content-Type: application/json
[other headers]
```

```json
{
    "type": "message",
    "from": {
        "id": "user1"
    },
    "text": "hello"
}
```

### <a name="response"></a><span data-ttu-id="bcd1d-110">Response</span><span class="sxs-lookup"><span data-stu-id="bcd1d-110">Response</span></span>

<span data-ttu-id="bcd1d-111">Cuando se entrega la actividad al bot, el servicio responde con un código de estado HTTP que refleja el código de estado del bot.</span><span class="sxs-lookup"><span data-stu-id="bcd1d-111">When the activity is delivered to the bot, the service responds with an HTTP status code that reflects the bot's status code.</span></span> <span data-ttu-id="bcd1d-112">Si el bot genera un error, se devuelve una respuesta HTTP 502 ("Puerta de enlace incorrecta") al cliente en respuesta a su solicitud de envío de actividad.</span><span class="sxs-lookup"><span data-stu-id="bcd1d-112">If the bot generates an error, an HTTP 502 response ("Bad Gateway") is returned to the client in response to its Send Activity request.</span></span> <span data-ttu-id="bcd1d-113">Si la solicitud POST se realiza correctamente, la respuesta contiene una carga JSON que especifica el identificador de la actividad que se envió al bot.</span><span class="sxs-lookup"><span data-stu-id="bcd1d-113">If the POST is successful, the response contains a JSON payload that specifies the ID of the Activity that was sent to the bot.</span></span>

```http
HTTP/1.1 200 OK
[other headers]
```

```json
{
    "id": "0001"
}
```

### <a name="total-time-for-the-send-activity-requestresponse"></a><span data-ttu-id="bcd1d-114">Tiempo total para la solicitud y respuesta del envío de la actividad</span><span class="sxs-lookup"><span data-stu-id="bcd1d-114">Total time for the Send Activity request/response</span></span>

<span data-ttu-id="bcd1d-115">El tiempo total para el envío POST de un mensaje en una conversación de Direct Line es la suma de lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="bcd1d-115">The total time to POST a message to a Direct Line conversation is the sum of the following:</span></span>

- <span data-ttu-id="bcd1d-116">Tiempo de tránsito de la solicitud HTTP para desplazarse desde el cliente al servicio Direct Line</span><span class="sxs-lookup"><span data-stu-id="bcd1d-116">Transit time for the HTTP request to travel from the client to the Direct Line service</span></span>
- <span data-ttu-id="bcd1d-117">Tiempo de procesamiento interno en Direct Line (normalmente menos de 120 ms)</span><span class="sxs-lookup"><span data-stu-id="bcd1d-117">Internal processing time within Direct Line (typically less than 120ms)</span></span>
- <span data-ttu-id="bcd1d-118">Tiempo de tránsito desde el servicio Direct Line al bot</span><span class="sxs-lookup"><span data-stu-id="bcd1d-118">Transit time from the Direct Line service to the bot</span></span>
- <span data-ttu-id="bcd1d-119">Tiempo de procesamiento en el bot</span><span class="sxs-lookup"><span data-stu-id="bcd1d-119">Processing time within the bot</span></span>
- <span data-ttu-id="bcd1d-120">Tiempo de tránsito de la respuesta HTTP hasta el cliente</span><span class="sxs-lookup"><span data-stu-id="bcd1d-120">Transit time for the HTTP response to travel back to the client</span></span>

## <a name="send-attachments-to-the-bot"></a><span data-ttu-id="bcd1d-121">Envío de datos adjuntos al bot</span><span class="sxs-lookup"><span data-stu-id="bcd1d-121">Send attachment(s) to the bot</span></span>

<span data-ttu-id="bcd1d-122">En algunas situaciones, un cliente debe enviar datos adjuntos al bot, como imágenes o documentos.</span><span class="sxs-lookup"><span data-stu-id="bcd1d-122">In some situations, a client may need to send attachments to the bot such as images or documents.</span></span> <span data-ttu-id="bcd1d-123">Un cliente puede enviar datos adjuntos al bot ya sea [especificando las direcciones URL](#send-by-url) de los datos adjuntos en el objeto [Activity](bot-framework-rest-connector-api-reference.md#activity-object) que envía mediante `POST /v3/directline/conversations/{conversationId}/activities` o [cargando los datos adjuntos](#upload-attachments) mediante `POST /v3/directline/conversations/{conversationId}/upload`.</span><span class="sxs-lookup"><span data-stu-id="bcd1d-123">A client may send attachments to the bot either by [specifying the URL(s)](#send-by-url) of the attachment(s) within the [Activity](bot-framework-rest-connector-api-reference.md#activity-object) object that it sends using `POST /v3/directline/conversations/{conversationId}/activities` or by [uploading attachment(s)](#upload-attachments) using `POST /v3/directline/conversations/{conversationId}/upload`.</span></span>

## <a id="send-by-url"></a> <span data-ttu-id="bcd1d-124">Envío de datos adjuntos por dirección URL</span><span class="sxs-lookup"><span data-stu-id="bcd1d-124">Send attachment(s) by URL</span></span>

<span data-ttu-id="bcd1d-125">Para enviar uno o varios archivos adjuntos como parte del objeto [Activity](bot-framework-rest-connector-api-reference.md#activity-object) mediante `POST /v3/directline/conversations/{conversationId}/activities`, basta con incluir uno o varios objetos [Attachment](bot-framework-rest-connector-api-reference.md#attachment-object) dentro del objeto Activity y establecer la propiedad `contentUrl` de cada objeto Attachment para especificar la dirección HTTP o HTTPS o el identificador URI `data` de los datos adjuntos.</span><span class="sxs-lookup"><span data-stu-id="bcd1d-125">To send one or more attachments as part of the [Activity](bot-framework-rest-connector-api-reference.md#activity-object) object using `POST /v3/directline/conversations/{conversationId}/activities`, simply include one or more [Attachment](bot-framework-rest-connector-api-reference.md#attachment-object) objects within the Activity object and set the `contentUrl` property of each Attachment object to specify the HTTP, HTTPS, or `data` URI of the attachment.</span></span>

## <a id="upload-attachments"></a> <span data-ttu-id="bcd1d-126">Envío de datos adjuntos mediante carga</span><span class="sxs-lookup"><span data-stu-id="bcd1d-126">Send attachment(s) by upload</span></span>

<span data-ttu-id="bcd1d-127">A menudo, un cliente puede tener imágenes o documentos en un dispositivo que desea enviar al bot, pero no hay direcciones URL correspondientes a esos archivos.</span><span class="sxs-lookup"><span data-stu-id="bcd1d-127">Often, a client may have image(s) or document(s) on a device that it wants to send to the bot, but no URLs corresponding to those files.</span></span> <span data-ttu-id="bcd1d-128">En esta situación, un cliente puede emitir una solicitud `POST /v3/directline/conversations/{conversationId}/upload` para enviar datos adjuntos al bot mediante carga.</span><span class="sxs-lookup"><span data-stu-id="bcd1d-128">In this situation, a client can can issue a `POST /v3/directline/conversations/{conversationId}/upload` request to send attachments to the bot by upload.</span></span> <span data-ttu-id="bcd1d-129">El formato y el contenido de la solicitud dependerán de si el cliente [envía un único dato adjunto](#upload-one-attachment) o [envía varios datos adjuntos](#upload-multiple-attachments).</span><span class="sxs-lookup"><span data-stu-id="bcd1d-129">The format and contents of the request will depend upon whether the client is [sending a single attachment](#upload-one-attachment) or [sending multiple attachments](#upload-multiple-attachments).</span></span>

### <a id="upload-one-attachment"></a> <span data-ttu-id="bcd1d-130">Envío de un único dato adjunto mediante carga</span><span class="sxs-lookup"><span data-stu-id="bcd1d-130">Send a single attachment by upload</span></span>

<span data-ttu-id="bcd1d-131">Para enviar un único dato adjunto mediante carga, emita esta solicitud:</span><span class="sxs-lookup"><span data-stu-id="bcd1d-131">To send a single attachment by upload, issue this request:</span></span> 

```http
POST https://directline.botframework.com/v3/directline/conversations/{conversationId}/upload?userId={userId}
Authorization: Bearer SECRET_OR_TOKEN
Content-Type: TYPE_OF_ATTACHMENT
Content-Disposition: ATTACHMENT_INFO
[other headers]

[file content]
```

<span data-ttu-id="bcd1d-132">En este identificador URI de solicitud, reemplace **{conversationId}** por el identificador de la conversación y **{userId}** por el identificador del usuario que envía el mensaje.</span><span class="sxs-lookup"><span data-stu-id="bcd1d-132">In this request URI, replace **{conversationId}** with the ID of the conversation and **{userId}** with the ID of the user that is sending the message.</span></span> <span data-ttu-id="bcd1d-133">El parámetro `userId` es obligatorio.</span><span class="sxs-lookup"><span data-stu-id="bcd1d-133">The `userId` parameter is required.</span></span> <span data-ttu-id="bcd1d-134">En los encabezados de solicitud, establezca `Content-Type` para especificar el tipo de datos adjuntos y establezca `Content-Disposition` para especificar el nombre de archivo de los datos adjuntos.</span><span class="sxs-lookup"><span data-stu-id="bcd1d-134">In the request headers, set `Content-Type` to specify the attachment's type and set `Content-Disposition` to specify the attachment's filename.</span></span>

<span data-ttu-id="bcd1d-135">Los fragmentos de código siguientes proporcionan un ejemplo de la solicitud de envío de un único dato adjunto y la respuesta.</span><span class="sxs-lookup"><span data-stu-id="bcd1d-135">The following snippets provide an example of the Send (single) Attachment request and response.</span></span>

#### <a name="request"></a><span data-ttu-id="bcd1d-136">Solicitud</span><span class="sxs-lookup"><span data-stu-id="bcd1d-136">Request</span></span>

```http
POST https://directline.botframework.com/v3/directline/conversations/abc123/upload?userId=user1
Authorization: Bearer RCurR_XV9ZA.cwA.BKA.iaJrC8xpy8qbOF5xnR2vtCX7CZj0LdjAPGfiCpg4Fv0
Content-Type: image/jpeg
Content-Disposition: name="file"; filename="badjokeeel.jpg"
[other headers]

[JPEG content]
```

#### <a name="response"></a><span data-ttu-id="bcd1d-137">Response</span><span class="sxs-lookup"><span data-stu-id="bcd1d-137">Response</span></span>

<span data-ttu-id="bcd1d-138">Si la solicitud es correcta, se envía un **mensaje** Activity al bot cuando finaliza la carga y la respuesta que recibe el cliente incluirá el identificador de la actividad que se envió.</span><span class="sxs-lookup"><span data-stu-id="bcd1d-138">If the request is successful, a **message** Activity is sent to the bot when the upload completes and the response that the client receives will contain the ID of the Activity that was sent.</span></span>

```http
HTTP/1.1 200 OK
[other headers]
```

```json
{
  "id": "0003"
}
```

### <a id="upload-multiple-attachments"></a> <span data-ttu-id="bcd1d-139">Envío de varios datos adjuntos mediante carga</span><span class="sxs-lookup"><span data-stu-id="bcd1d-139">Send multiple attachments by upload</span></span>

<span data-ttu-id="bcd1d-140">Para enviar varios datos adjuntos mediante carga, emita una solicitud `POST` de varias partes al punto de conexión `/v3/directline/conversations/{conversationId}/upload`.</span><span class="sxs-lookup"><span data-stu-id="bcd1d-140">To send multiple attachments by upload, `POST` a multipart request to the `/v3/directline/conversations/{conversationId}/upload` endpoint.</span></span> <span data-ttu-id="bcd1d-141">Establezca el encabezado `Content-Type` de la solicitud en `multipart/form-data` e incluya el encabezado `Content-Type` y el encabezado `Content-Disposition` para que cada parte especifique el tipo y el nombre de archivo de los datos adjuntos.</span><span class="sxs-lookup"><span data-stu-id="bcd1d-141">Set the `Content-Type` header of the request to `multipart/form-data` and include the `Content-Type` header and `Content-Disposition` header for each part to specify each attachment's type and filename.</span></span> <span data-ttu-id="bcd1d-142">En el identificador URI de la solicitud, establezca el parámetro `userId` en el identificador del usuario que envía el mensaje.</span><span class="sxs-lookup"><span data-stu-id="bcd1d-142">In the request URI, set the `userId` parameter to the ID of the user that is sending the message.</span></span> 

<span data-ttu-id="bcd1d-143">Puede incluir un objeto [Activity](bot-framework-rest-connector-api-reference.md#activity-object) en la solicitud con la adición de una parte que especifica como encabezado `Content-Type` el valor `application/vnd.microsoft.activity`.</span><span class="sxs-lookup"><span data-stu-id="bcd1d-143">You may include an [Activity](bot-framework-rest-connector-api-reference.md#activity-object) object within the request by adding a part that specifies the `Content-Type` header value `application/vnd.microsoft.activity`.</span></span> <span data-ttu-id="bcd1d-144">Si la solicitud incluye una actividad, los datos adjuntos especificados por otras partes de la carga se agregan como datos adjuntos a esa actividad antes de enviarla.</span><span class="sxs-lookup"><span data-stu-id="bcd1d-144">If the request includes an Activity, the attachments that are specified by other parts of the payload are added as attachments to that Activity before it is sent.</span></span> <span data-ttu-id="bcd1d-145">Si la solicitud no incluye una actividad, se crea una actividad vacía para servir como el contenedor en el que se envían los datos adjuntos especificados.</span><span class="sxs-lookup"><span data-stu-id="bcd1d-145">If the request does not include an Activity, an empty Activity is created to serve as the container in which the specified attachments are sent.</span></span>

<span data-ttu-id="bcd1d-146">Los fragmentos de código siguientes proporcionan un ejemplo de la solicitud de envío de varios datos adjuntos y la respuesta.</span><span class="sxs-lookup"><span data-stu-id="bcd1d-146">The following snippets provide an example of the Send (multiple) Attachments request and response.</span></span> <span data-ttu-id="bcd1d-147">En este ejemplo, la solicitud envía un mensaje que contiene texto y un único adjunto de imagen.</span><span class="sxs-lookup"><span data-stu-id="bcd1d-147">In this example, the request sends a message that contains some text and a single image attachment.</span></span> <span data-ttu-id="bcd1d-148">Se pueden agregar partes adicionales a la solicitud para incluir varios datos adjuntos en este mensaje.</span><span class="sxs-lookup"><span data-stu-id="bcd1d-148">Additional parts could be added to the request to include multiple attachments in this message.</span></span>

#### <a name="request"></a><span data-ttu-id="bcd1d-149">Solicitud</span><span class="sxs-lookup"><span data-stu-id="bcd1d-149">Request</span></span>

```http
POST https://directline.botframework.com/v3/directline/conversations/abc123/upload?userId=user1
Authorization: Bearer RCurR_XV9ZA.cwA.BKA.iaJrC8xpy8qbOF5xnR2vtCX7CZj0LdjAPGfiCpg4Fv0
Content-Type: multipart/form-data; boundary=----DD4E5147-E865-4652-B662-F223701A8A89
[other headers]

----DD4E5147-E865-4652-B662-F223701A8A89
Content-Type: image/jpeg
Content-Disposition: form-data; name="file"; filename="badjokeeel.jpg"
[other headers]

[JPEG content]

----DD4E5147-E865-4652-B662-F223701A8A89
Content-Type: application/vnd.microsoft.activity
[other headers]

{
  "type": "message",
  "from": {
    "id": "user1"
  },
  "text": "Hey I just IM'd you\n\nand this is crazy\n\nbut here's my webhook\n\nso POST me maybe"
}

----DD4E5147-E865-4652-B662-F223701A8A89
```

#### <a name="response"></a><span data-ttu-id="bcd1d-150">Response</span><span class="sxs-lookup"><span data-stu-id="bcd1d-150">Response</span></span>

<span data-ttu-id="bcd1d-151">Si la solicitud es correcta, se envía un mensaje Activity al bot cuando finaliza la carga y la respuesta que recibe el cliente incluirá el identificador de la actividad que se envió.</span><span class="sxs-lookup"><span data-stu-id="bcd1d-151">If the request is successful, a message Activity is sent to the bot when the upload completes and the response that the client receives will contain the ID of the Activity that was sent.</span></span>

```http
HTTP/1.1 200 OK
[other headers]
```

```json
{
    "id": "0004"
}
```

## <a name="additional-resources"></a><span data-ttu-id="bcd1d-152">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="bcd1d-152">Additional resources</span></span>

- [<span data-ttu-id="bcd1d-153">Conceptos clave</span><span class="sxs-lookup"><span data-stu-id="bcd1d-153">Key concepts</span></span>](bot-framework-rest-direct-line-3-0-concepts.md)
- [<span data-ttu-id="bcd1d-154">Autenticación</span><span class="sxs-lookup"><span data-stu-id="bcd1d-154">Authentication</span></span>](bot-framework-rest-direct-line-3-0-authentication.md)
- [<span data-ttu-id="bcd1d-155">Inicio de una conversación</span><span class="sxs-lookup"><span data-stu-id="bcd1d-155">Start a conversation</span></span>](bot-framework-rest-direct-line-3-0-start-conversation.md)
- [<span data-ttu-id="bcd1d-156">Volver a conectarse a una conversación</span><span class="sxs-lookup"><span data-stu-id="bcd1d-156">Reconnect to a conversation</span></span>](bot-framework-rest-direct-line-3-0-reconnect-to-conversation.md)
- [<span data-ttu-id="bcd1d-157">Recepción de actividades del bot</span><span class="sxs-lookup"><span data-stu-id="bcd1d-157">Receive activities from the bot</span></span>](bot-framework-rest-direct-line-3-0-receive-activities.md)
- [<span data-ttu-id="bcd1d-158">Finalización de una conversación</span><span class="sxs-lookup"><span data-stu-id="bcd1d-158">End a conversation</span></span>](bot-framework-rest-direct-line-3-0-end-conversation.md)
