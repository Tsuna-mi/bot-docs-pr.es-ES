---
title: 'Referencia de API: Direct Line API 1.1 | Microsoft Docs'
description: Obtenga información sobre encabezados, códigos de estado HTTP, esquemas, operaciones y objetos en Direct Line API 1.1.
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ms.openlocfilehash: 2f688b9c80e762b93c2eba8f4671ff1760f624f9
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305957"
---
# <a name="api-reference---direct-line-api-11"></a><span data-ttu-id="bb7a4-103">Referencia de API: Direct Line API 1.1</span><span class="sxs-lookup"><span data-stu-id="bb7a4-103">API reference - Direct Line API 1.1</span></span>

> [!IMPORTANT]
> <span data-ttu-id="bb7a4-104">Este artículo contiene información de referencia para Direct Line API 1.1.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-104">This article contains reference information for Direct Line API 1.1.</span></span> <span data-ttu-id="bb7a4-105">Si va a crear una nueva conexión entre la aplicación cliente y el bot, use [Direct Line API 3.0](bot-framework-rest-direct-line-3-0-api-reference.md) en su lugar.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-105">If you are creating a new connection between your client application and bot, use [Direct Line API 3.0](bot-framework-rest-direct-line-3-0-api-reference.md) instead.</span></span>

<span data-ttu-id="bb7a4-106">Para permitir que su aplicación cliente se comunique con su bot, puede usar Direct Line API 1.1.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-106">You can enable your client application to communicate with your bot by using Direct Line API 1.1.</span></span> <span data-ttu-id="bb7a4-107">Direct Line API 1.1 usa REST y JSON estándar del sector mediante HTTPS.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-107">Direct Line API 1.1 uses industry-standard REST and JSON over HTTPS.</span></span>

## <a name="base-uri"></a><span data-ttu-id="bb7a4-108">URI base</span><span class="sxs-lookup"><span data-stu-id="bb7a4-108">Base URI</span></span>

<span data-ttu-id="bb7a4-109">Para acceder a Direct Line API 1.1, use este URI base para todas las solicitudes de API:</span><span class="sxs-lookup"><span data-stu-id="bb7a4-109">To access Direct Line API 1.1, use this base URI for all API requests:</span></span>

`https://directline.botframework.com`

## <a name="headers"></a><span data-ttu-id="bb7a4-110">encabezados</span><span class="sxs-lookup"><span data-stu-id="bb7a4-110">Headers</span></span>

<span data-ttu-id="bb7a4-111">Además de los encabezados de solicitud HTTP estándar, una solicitud de Direct Line API debe incluir un encabezado `Authorization` que especifique un secreto o un token para autenticar al cliente que emite la solicitud.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-111">In addition to the standard HTTP request headers, a Direct Line API request must include an `Authorization` header that specifies a secret or token to authenticate the client that is issuing the request.</span></span> <span data-ttu-id="bb7a4-112">Puede especificar el encabezado `Authorization` mediante el esquema "Bearer" o "BotConnector".</span><span class="sxs-lookup"><span data-stu-id="bb7a4-112">You can specify the `Authorization` header using either the "Bearer" scheme or the "BotConnector" scheme.</span></span> 

<span data-ttu-id="bb7a4-113">**Esquema Bearer**:</span><span class="sxs-lookup"><span data-stu-id="bb7a4-113">**Bearer scheme**:</span></span>
```http
Authorization: Bearer SECRET_OR_TOKEN
```

<span data-ttu-id="bb7a4-114">**Esquema BotConnector**:</span><span class="sxs-lookup"><span data-stu-id="bb7a4-114">**BotConnector scheme**:</span></span>
```http
Authorization: BotConnector SECRET_OR_TOKEN
```

<span data-ttu-id="bb7a4-115">Para más información sobre cómo obtener un secreto o un token que el cliente pueda usar para autenticar sus solicitudes de Direct Line API, consulte [Autenticación](bot-framework-rest-direct-line-1-1-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="bb7a4-115">For details about how to obtain a secret or token that your client can use to authenticate its Direct Line API requests, see [Authentication](bot-framework-rest-direct-line-1-1-authentication.md).</span></span>

## <a name="http-status-codes"></a><span data-ttu-id="bb7a4-116">Códigos de estado HTTP</span><span class="sxs-lookup"><span data-stu-id="bb7a4-116">HTTP status codes</span></span>

<span data-ttu-id="bb7a4-117">El <a href="http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html" target="_blank">código de estado HTTP</a> que se devuelve con cada respuesta indica el resultado de la solicitud correspondiente.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-117">The <a href="http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html" target="_blank">HTTP status code</a> that is returned with each response indicates the outcome of the corresponding request.</span></span> 

| <span data-ttu-id="bb7a4-118">Código de estado HTTP</span><span class="sxs-lookup"><span data-stu-id="bb7a4-118">HTTP status code</span></span> | <span data-ttu-id="bb7a4-119">Significado</span><span class="sxs-lookup"><span data-stu-id="bb7a4-119">Meaning</span></span> |
|----|----|
| <span data-ttu-id="bb7a4-120">200</span><span class="sxs-lookup"><span data-stu-id="bb7a4-120">200</span></span> | <span data-ttu-id="bb7a4-121">La solicitud finalizó correctamente.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-121">The request succeeded.</span></span> |
| <span data-ttu-id="bb7a4-122">204</span><span class="sxs-lookup"><span data-stu-id="bb7a4-122">204</span></span> | <span data-ttu-id="bb7a4-123">La solicitud se realizó correctamente, pero no se devolvió ningún contenido.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-123">The request succeeded but no content was returned.</span></span> |
| <span data-ttu-id="bb7a4-124">400</span><span class="sxs-lookup"><span data-stu-id="bb7a4-124">400</span></span> | <span data-ttu-id="bb7a4-125">La solicitud tenía formato incorrecto o era incorrecta por otro motivo.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-125">The request was malformed or otherwise incorrect.</span></span> |
| <span data-ttu-id="bb7a4-126">401</span><span class="sxs-lookup"><span data-stu-id="bb7a4-126">401</span></span> | <span data-ttu-id="bb7a4-127">El cliente no está autorizado a realizar solicitudes.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-127">The client is not authorized to make the request.</span></span> <span data-ttu-id="bb7a4-128">A menudo, este código de estado se produce porque falta el encabezado `Authorization` o tiene un formato incorrecto.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-128">Often this status code occurs because the `Authorization` header is missing or malformed.</span></span> |
| <span data-ttu-id="bb7a4-129">403</span><span class="sxs-lookup"><span data-stu-id="bb7a4-129">403</span></span> | <span data-ttu-id="bb7a4-130">El cliente no tiene permitido llevar a cabo la operación solicitada.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-130">The client is not allowed to perform the requested operation.</span></span> <span data-ttu-id="bb7a4-131">A menudo, este código de estado se produce porque el encabezado `Authorization` especifica un token o un secreto no válidos.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-131">Often this status code occurs because the `Authorization` header specifies an invalid token or secret.</span></span> |
| <span data-ttu-id="bb7a4-132">404</span><span class="sxs-lookup"><span data-stu-id="bb7a4-132">404</span></span> | <span data-ttu-id="bb7a4-133">No se encontró el recurso solicitado.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-133">The requested resource was not found.</span></span> <span data-ttu-id="bb7a4-134">Normalmente, este código de estado indica un URI de solicitud no válido.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-134">Typically this status code indicates an invalid request URI.</span></span> |
| <span data-ttu-id="bb7a4-135">500</span><span class="sxs-lookup"><span data-stu-id="bb7a4-135">500</span></span> | <span data-ttu-id="bb7a4-136">Se ha producido un error interno del servidor en el servicio Direct Line o se ha producido un error en el bot.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-136">Either an internal server error occurred within the Direct Line service or a failure occurred within the bot.</span></span> <span data-ttu-id="bb7a4-137">Si recibe un error 500 al enviar una solicitud POST para enviar un mensaje a un bot, es posible que el error se desencadenara por un error en el bot.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-137">If you receive a 500 error when POSTing a message to a bot, it is possible that the error was triggered by a failure in the bot.</span></span> <span data-ttu-id="bb7a4-138">**Se trata de un código de error común.**</span><span class="sxs-lookup"><span data-stu-id="bb7a4-138">**This is a common error code.**</span></span> |

## <a name="token-operations"></a><span data-ttu-id="bb7a4-139">Operaciones con tokens</span><span class="sxs-lookup"><span data-stu-id="bb7a4-139">Token operations</span></span> 
<span data-ttu-id="bb7a4-140">Use estas operaciones para crear o actualizar un token que un cliente pueda utilizar para acceder a una conversación única.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-140">Use these operations to create or refresh a token that a client can use to access a single conversation.</span></span>

| <span data-ttu-id="bb7a4-141">Operación</span><span class="sxs-lookup"><span data-stu-id="bb7a4-141">Operation</span></span> | <span data-ttu-id="bb7a4-142">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="bb7a4-142">Description</span></span> |
|----|----|
| [<span data-ttu-id="bb7a4-143">Generar token</span><span class="sxs-lookup"><span data-stu-id="bb7a4-143">Generate Token</span></span>](#generate-token) | <span data-ttu-id="bb7a4-144">Genera un token para una conversación nueva.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-144">Generate a token for a new conversation.</span></span> | 
| [<span data-ttu-id="bb7a4-145">Actualizar token</span><span class="sxs-lookup"><span data-stu-id="bb7a4-145">Refresh Token</span></span>](#refresh-token) | <span data-ttu-id="bb7a4-146">Actualiza un token.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-146">Refresh a token.</span></span> | 

### <a name="generate-token"></a><span data-ttu-id="bb7a4-147">Generar token</span><span class="sxs-lookup"><span data-stu-id="bb7a4-147">Generate Token</span></span>
<span data-ttu-id="bb7a4-148">Genera un token válido para una conversación.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-148">Generates a token that is valid for one conversation.</span></span> 
```http 
POST /api/tokens/conversation
```

| | |
|----|----|
| <span data-ttu-id="bb7a4-149">**Cuerpo de la solicitud**</span><span class="sxs-lookup"><span data-stu-id="bb7a4-149">**Request body**</span></span> | <span data-ttu-id="bb7a4-150">N/D</span><span class="sxs-lookup"><span data-stu-id="bb7a4-150">n/a</span></span> |
| <span data-ttu-id="bb7a4-151">**Devuelve**</span><span class="sxs-lookup"><span data-stu-id="bb7a4-151">**Returns**</span></span> | <span data-ttu-id="bb7a4-152">Una cadena que representa el token</span><span class="sxs-lookup"><span data-stu-id="bb7a4-152">A string that represents the token</span></span> | 

### <a name="refresh-token"></a><span data-ttu-id="bb7a4-153">Actualizar token</span><span class="sxs-lookup"><span data-stu-id="bb7a4-153">Refresh Token</span></span>
<span data-ttu-id="bb7a4-154">Actualiza el token.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-154">Refreshes the token.</span></span> 
```http 
GET /api/tokens/{conversationId}/renew
```

| | |
|----|----|
| <span data-ttu-id="bb7a4-155">**Cuerpo de la solicitud**</span><span class="sxs-lookup"><span data-stu-id="bb7a4-155">**Request body**</span></span> | <span data-ttu-id="bb7a4-156">N/D</span><span class="sxs-lookup"><span data-stu-id="bb7a4-156">n/a</span></span> |
| <span data-ttu-id="bb7a4-157">**Devuelve**</span><span class="sxs-lookup"><span data-stu-id="bb7a4-157">**Returns**</span></span> | <span data-ttu-id="bb7a4-158">Una cadena que representa el nuevo token</span><span class="sxs-lookup"><span data-stu-id="bb7a4-158">A string that represents the new token</span></span> | 

## <a name="conversation-operations"></a><span data-ttu-id="bb7a4-159">Operaciones con conversaciones</span><span class="sxs-lookup"><span data-stu-id="bb7a4-159">Conversation operations</span></span> 
<span data-ttu-id="bb7a4-160">Use estas operaciones para abrir una conversación con el bot e intercambiar mensajes entre el cliente y el bot.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-160">Use these operations to open a conversation with your bot and exchange messages between client and bot.</span></span>

| <span data-ttu-id="bb7a4-161">Operación</span><span class="sxs-lookup"><span data-stu-id="bb7a4-161">Operation</span></span> | <span data-ttu-id="bb7a4-162">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="bb7a4-162">Description</span></span> |
|----|----|
| [<span data-ttu-id="bb7a4-163">Iniciar conversación</span><span class="sxs-lookup"><span data-stu-id="bb7a4-163">Start Conversation</span></span>](#start-conversation) | <span data-ttu-id="bb7a4-164">Abre una nueva conversación con el bot.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-164">Opens a new conversation with the bot.</span></span> | 
| [<span data-ttu-id="bb7a4-165">Get Messages</span><span class="sxs-lookup"><span data-stu-id="bb7a4-165">Get Messages</span></span>](#get-messages) | <span data-ttu-id="bb7a4-166">Recibe mensajes del bot.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-166">Retrieves messages from the bot.</span></span> |
| [<span data-ttu-id="bb7a4-167">Enviar un mensaje</span><span class="sxs-lookup"><span data-stu-id="bb7a4-167">Send a Message</span></span>](#send-a-message) | <span data-ttu-id="bb7a4-168">Envía un mensaje al bot.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-168">Sends a message to the bot.</span></span> | 
| [<span data-ttu-id="bb7a4-169">Cargar y enviar archivos</span><span class="sxs-lookup"><span data-stu-id="bb7a4-169">Upload and Send File(s)</span></span>](#upload-send-files) | <span data-ttu-id="bb7a4-170">Carga y envía archivos como adjuntos.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-170">Uploads and sends file(s) as attachment(s).</span></span> |

### <a name="start-conversation"></a><span data-ttu-id="bb7a4-171">Iniciar conversación</span><span class="sxs-lookup"><span data-stu-id="bb7a4-171">Start Conversation</span></span>
<span data-ttu-id="bb7a4-172">Abre una nueva conversación con el bot.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-172">Opens a new conversation with the bot.</span></span> 
```http 
POST /api/conversations
```

| | |
|----|----|
| <span data-ttu-id="bb7a4-173">**Cuerpo de la solicitud**</span><span class="sxs-lookup"><span data-stu-id="bb7a4-173">**Request body**</span></span> | <span data-ttu-id="bb7a4-174">N/D</span><span class="sxs-lookup"><span data-stu-id="bb7a4-174">n/a</span></span> |
| <span data-ttu-id="bb7a4-175">**Devuelve**</span><span class="sxs-lookup"><span data-stu-id="bb7a4-175">**Returns**</span></span> | <span data-ttu-id="bb7a4-176">Un objeto [Conversation](#conversation-object)</span><span class="sxs-lookup"><span data-stu-id="bb7a4-176">A [Conversation](#conversation-object) object</span></span> | 

### <a name="get-messages"></a><span data-ttu-id="bb7a4-177">Obtener mensajes</span><span class="sxs-lookup"><span data-stu-id="bb7a4-177">Get Messages</span></span>
<span data-ttu-id="bb7a4-178">Recupera los mensajes del bot para la conversación especificada.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-178">Retrieves messages from the bot for the specified conversation.</span></span> <span data-ttu-id="bb7a4-179">Establezca el parámetro `watermark` en el URI de la solicitud para indicar el mensaje más reciente visto por el cliente.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-179">Set the `watermark` parameter in the request URI to indicate the most recent message seen by the client.</span></span> 

```http
GET /api/conversations/{conversationId}/messages?watermark={watermark_value}
```

| | |
|----|----|
| <span data-ttu-id="bb7a4-180">**Cuerpo de la solicitud**</span><span class="sxs-lookup"><span data-stu-id="bb7a4-180">**Request body**</span></span> | <span data-ttu-id="bb7a4-181">N/D</span><span class="sxs-lookup"><span data-stu-id="bb7a4-181">n/a</span></span> |
| <span data-ttu-id="bb7a4-182">**Devuelve**</span><span class="sxs-lookup"><span data-stu-id="bb7a4-182">**Returns**</span></span> | <span data-ttu-id="bb7a4-183">Un objeto [MessageSet](#messageset-object).</span><span class="sxs-lookup"><span data-stu-id="bb7a4-183">A [MessageSet](#messageset-object) object.</span></span> <span data-ttu-id="bb7a4-184">La respuesta contiene `watermark` como propiedad del objeto `MessageSet`.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-184">The response contains `watermark` as a property of the `MessageSet` object.</span></span> <span data-ttu-id="bb7a4-185">Los clientes deben desplazarse por los mensajes disponibles haciendo avanzar el valor `watermark` hasta que no se devuelvan mensajes.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-185">Clients should page through the available messages by advancing the `watermark` value until no messages are returned.</span></span> | 

### <a name="send-a-message"></a><span data-ttu-id="bb7a4-186">Enviar un mensaje</span><span class="sxs-lookup"><span data-stu-id="bb7a4-186">Send a Message</span></span>
<span data-ttu-id="bb7a4-187">Envía un mensaje al bot.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-187">Sends a message to the bot.</span></span> 
```http 
POST /api/conversations/{conversationId}/messages
```

| | |
|----|----|
| <span data-ttu-id="bb7a4-188">**Cuerpo de la solicitud**</span><span class="sxs-lookup"><span data-stu-id="bb7a4-188">**Request body**</span></span> | <span data-ttu-id="bb7a4-189">Un objeto [Message](#message-object)</span><span class="sxs-lookup"><span data-stu-id="bb7a4-189">A [Message](#message-object) object</span></span> |
| <span data-ttu-id="bb7a4-190">**Devuelve**</span><span class="sxs-lookup"><span data-stu-id="bb7a4-190">**Returns**</span></span> | <span data-ttu-id="bb7a4-191">No se devuelven datos en el cuerpo de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-191">No data is returned in body of the response.</span></span> <span data-ttu-id="bb7a4-192">El servicio responde con un código de estado HTTP 204, si el mensaje se envió correctamente.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-192">The service responds with an HTTP 204 status code if the message was sent successfully.</span></span> <span data-ttu-id="bb7a4-193">El cliente puede obtener su mensaje enviado (junto con los mensajes que el bot ha enviado al cliente) mediante la operación [Obtener mensajes](#get-messages).</span><span class="sxs-lookup"><span data-stu-id="bb7a4-193">The client may obtain its sent message (along with any messages that the bot has sent to the client) by using the [Get Messages](#get-messages) operation.</span></span> |

### <a id="upload-send-files"></a> <span data-ttu-id="bb7a4-194">Cargar y enviar archivos</span><span class="sxs-lookup"><span data-stu-id="bb7a4-194">Upload and Send File(s)</span></span>
<span data-ttu-id="bb7a4-195">Carga y envía archivos como adjuntos.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-195">Uploads and sends file(s) as attachment(s).</span></span> <span data-ttu-id="bb7a4-196">Establezca el parámetro `userId` en el URI de la solicitud para especificar el identificador del usuario que envía los datos adjuntos.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-196">Set the `userId` parameter in the request URI to specify the ID of the user that is sending the attachments.</span></span>
```http 
POST /api/conversations/{conversationId}/upload?userId={userId}
```

| | |
|----|----|
| <span data-ttu-id="bb7a4-197">**Cuerpo de la solicitud**</span><span class="sxs-lookup"><span data-stu-id="bb7a4-197">**Request body**</span></span> | <span data-ttu-id="bb7a4-198">Para un único dato adjunto, rellene el cuerpo de la solicitud con el contenido del archivo.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-198">For a single attachment, populate the request body with the file contents.</span></span> <span data-ttu-id="bb7a4-199">Para varios datos adjuntos, cree un cuerpo de solicitud de varias partes que contenga una parte para cada dato adjunto y, además (de manera opcional), una parte para el objeto [Message](#message-object) que debe actuar como contenedor para los datos adjuntos especificados.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-199">For multiple attachments, create a multipart request body that contains one part for each attachment, and also (optionally) one part for the [Message](#message-object) object that should serve as the container for the specified attachment(s).</span></span> <span data-ttu-id="bb7a4-200">Para más información, consulte [Envío de una actividad al bot](bot-framework-rest-direct-line-1-1-send-message.md).</span><span class="sxs-lookup"><span data-stu-id="bb7a4-200">For more information, see [Send a message to the bot](bot-framework-rest-direct-line-1-1-send-message.md).</span></span> |
| <span data-ttu-id="bb7a4-201">**Devuelve**</span><span class="sxs-lookup"><span data-stu-id="bb7a4-201">**Returns**</span></span> | <span data-ttu-id="bb7a4-202">No se devuelven datos en el cuerpo de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-202">No data is returned in body of the response.</span></span> <span data-ttu-id="bb7a4-203">El servicio responde con un código de estado HTTP 204, si el mensaje se envió correctamente.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-203">The service responds with an HTTP 204 status code if the message was sent successfully.</span></span> <span data-ttu-id="bb7a4-204">El cliente puede obtener su mensaje enviado (junto con los mensajes que el bot ha enviado al cliente) mediante la operación [Obtener mensajes](#get-messages).</span><span class="sxs-lookup"><span data-stu-id="bb7a4-204">The client may obtain its sent message (along with any messages that the bot has sent to the client) by using the [Get Messages](#get-messages) operation.</span></span> | 

> [!NOTE]
> <span data-ttu-id="bb7a4-205">Los archivos cargados se eliminan después de 24 horas.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-205">Uploaded files are deleted after 24 hours.</span></span>

## <a name="schema"></a><span data-ttu-id="bb7a4-206">Esquema</span><span class="sxs-lookup"><span data-stu-id="bb7a4-206">Schema</span></span>

<span data-ttu-id="bb7a4-207">El esquema de Direct Line 1.1 es una copia simplificada del esquema de Bot Framework v1 que incluye los siguientes objetos.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-207">Direct Line 1.1 schema is a simplified copy of the Bot Framework v1 schema that includes the following objects.</span></span>

### <a name="message-object"></a><span data-ttu-id="bb7a4-208">Objeto de mensaje</span><span class="sxs-lookup"><span data-stu-id="bb7a4-208">Message object</span></span>

<span data-ttu-id="bb7a4-209">Define un mensaje que un cliente envía a un bot o recibe de un bot.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-209">Defines a message that a client sends to a bot or receives from a bot.</span></span>

| <span data-ttu-id="bb7a4-210">Propiedad</span><span class="sxs-lookup"><span data-stu-id="bb7a4-210">Property</span></span> | <span data-ttu-id="bb7a4-211">Escriba</span><span class="sxs-lookup"><span data-stu-id="bb7a4-211">Type</span></span> | <span data-ttu-id="bb7a4-212">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="bb7a4-212">Description</span></span> |
|----|----|----|
| <span data-ttu-id="bb7a4-213">**id**</span><span class="sxs-lookup"><span data-stu-id="bb7a4-213">**id**</span></span> | <span data-ttu-id="bb7a4-214">string</span><span class="sxs-lookup"><span data-stu-id="bb7a4-214">string</span></span> | <span data-ttu-id="bb7a4-215">Identificador que identifica de forma única el mensaje (asignado por Direct Line).</span><span class="sxs-lookup"><span data-stu-id="bb7a4-215">ID that uniquely identifies the message (assigned by Direct Line).</span></span> | 
| <span data-ttu-id="bb7a4-216">**conversationId**</span><span class="sxs-lookup"><span data-stu-id="bb7a4-216">**conversationId**</span></span> | <span data-ttu-id="bb7a4-217">string</span><span class="sxs-lookup"><span data-stu-id="bb7a4-217">string</span></span> | <span data-ttu-id="bb7a4-218">Identificador que identifica la conversación.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-218">ID that identifies the conversation.</span></span>  | 
| <span data-ttu-id="bb7a4-219">**created**</span><span class="sxs-lookup"><span data-stu-id="bb7a4-219">**created**</span></span> | <span data-ttu-id="bb7a4-220">string</span><span class="sxs-lookup"><span data-stu-id="bb7a4-220">string</span></span> | <span data-ttu-id="bb7a4-221">Fecha y hora en que se creó el mensaje, expresado en el formato <a href="https://en.wikipedia.org/wiki/ISO_8601" target="_blank">ISO-8601</a>.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-221">Date and time that the message was created, expressed in <a href="https://en.wikipedia.org/wiki/ISO_8601" target="_blank">ISO-8601</a> format.</span></span> | 
| <span data-ttu-id="bb7a4-222">**from**</span><span class="sxs-lookup"><span data-stu-id="bb7a4-222">**from**</span></span> | <span data-ttu-id="bb7a4-223">string</span><span class="sxs-lookup"><span data-stu-id="bb7a4-223">string</span></span> | <span data-ttu-id="bb7a4-224">Identificador que identifica el usuario que es el remitente del mensaje.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-224">ID that identifies the user that is the sender of the message.</span></span> <span data-ttu-id="bb7a4-225">Al crear un mensaje, los clientes deben establecer esta propiedad en un identificador de usuario estable.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-225">When creating a message, clients should set this property to a stable user ID.</span></span> <span data-ttu-id="bb7a4-226">Aunque Direct Line asignará un identificador de usuario si no se proporciona ninguno, esto da lugar normalmente a un comportamiento inesperado.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-226">Although Direct Line will assign a user ID if none is supplied, this typically results in unexpected behavior.</span></span> | 
| <span data-ttu-id="bb7a4-227">**text**</span><span class="sxs-lookup"><span data-stu-id="bb7a4-227">**text**</span></span> | <span data-ttu-id="bb7a4-228">string</span><span class="sxs-lookup"><span data-stu-id="bb7a4-228">string</span></span> | <span data-ttu-id="bb7a4-229">Texto del mensaje que se envía del usuario al bot o del bot al usuario.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-229">Text of the message that is sent from user to bot or bot to user.</span></span> | 
| <span data-ttu-id="bb7a4-230">**channelData**</span><span class="sxs-lookup"><span data-stu-id="bb7a4-230">**channelData**</span></span> | <span data-ttu-id="bb7a4-231">objeto</span><span class="sxs-lookup"><span data-stu-id="bb7a4-231">object</span></span> | <span data-ttu-id="bb7a4-232">Objeto que incluye contenido específico de canal.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-232">An object that contains channel-specific content.</span></span> <span data-ttu-id="bb7a4-233">Algunos canales proporcionan características que requieren información adicional que no se puede representar mediante el esquema de datos adjuntos.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-233">Some channels provide features that require additional information that cannot be represented using the attachment schema.</span></span> <span data-ttu-id="bb7a4-234">Para esos casos, establezca esta propiedad en el contenido específico de canal tal como se define en la documentación del canal.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-234">For those cases, set this property to the channel-specific content as defined in the channel's documentation.</span></span> <span data-ttu-id="bb7a4-235">Estos datos se envían sin modificar entre el cliente y el bot.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-235">This data is sent unmodified between client and bot.</span></span> <span data-ttu-id="bb7a4-236">Esta propiedad se debe establecer en un objeto complejo o dejarse vacía.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-236">This property must either be set to a complex object or left empty.</span></span> <span data-ttu-id="bb7a4-237">No la establezca en una cadena, número u otro tipo simple.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-237">Do not set it to a string, number, or other simple type.</span></span> | 
| <span data-ttu-id="bb7a4-238">**images**</span><span class="sxs-lookup"><span data-stu-id="bb7a4-238">**images**</span></span> | <span data-ttu-id="bb7a4-239">string[]</span><span class="sxs-lookup"><span data-stu-id="bb7a4-239">string[]</span></span> | <span data-ttu-id="bb7a4-240">Matriz de cadenas que contiene las direcciones URL para las imágenes que contiene el mensaje.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-240">Array of strings that contains the URL(s) for the image(s) that the message contains.</span></span> <span data-ttu-id="bb7a4-241">En algunos casos, las cadenas de esta matriz pueden ser direcciones URL relativas.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-241">In some cases, strings in this array may be relative URLs.</span></span> <span data-ttu-id="bb7a4-242">Si cualquier cadena de esta matriz no comienza con "http" o "https", anteponga `https://directline.botframework.com` a la cadena para formar la dirección URL completa.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-242">If any string in this array does not begin with either "http" or "https", prepend `https://directline.botframework.com` to the string to form the complete URL.</span></span> | 
| <span data-ttu-id="bb7a4-243">**attachments**</span><span class="sxs-lookup"><span data-stu-id="bb7a4-243">**attachments**</span></span> | <span data-ttu-id="bb7a4-244">[Attachment](#attachment-object)[]</span><span class="sxs-lookup"><span data-stu-id="bb7a4-244">[Attachment](#attachment-object)[]</span></span> | <span data-ttu-id="bb7a4-245">Matriz de objetos **Attachment** que representan los datos adjuntos que no son imágenes que contiene el mensaje.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-245">Array of **Attachment** objects that represent the non-image attachments that the message contains.</span></span> <span data-ttu-id="bb7a4-246">Cada objeto de la matriz contiene una propiedad `url` y una propiedad `contentType`.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-246">Each object in the array contains a `url` property and a `contentType` property.</span></span> <span data-ttu-id="bb7a4-247">En los mensajes que recibe un cliente de un bot, la propiedad `url` puede especificar a veces una dirección URL relativa.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-247">In messages that a client receives from a bot, the `url` property may sometimes specify a relative URL.</span></span> <span data-ttu-id="bb7a4-248">Para cualquier valor de propiedad `url` que no comience por "http" o "https", anteponga `https://directline.botframework.com` a la cadena para formar la dirección URL completa.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-248">For any `url` property value that does not begin with either "http" or "https", prepend `https://directline.botframework.com` to the string to form the complete URL.</span></span> | 

<span data-ttu-id="bb7a4-249">En el ejemplo siguiente se muestra un objeto Message que contiene todas las propiedades posibles.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-249">The following example shows a Message object that contains all possible properties.</span></span> <span data-ttu-id="bb7a4-250">En la mayoría de los casos al crear un mensaje, el cliente solo tiene que proporcionar la propiedad `from` y al menos una propiedad de contenido (por ejemplo, `text`, `images`, `attachments` o `channelData`).</span><span class="sxs-lookup"><span data-stu-id="bb7a4-250">In most cases when creating a message, the client only needs to supply the `from` property and at least one content property (e.g., `text`, `images`, `attachments`, or `channelData`).</span></span>

```json
{
    "id": "CuvLPID4kDb|000000000000000004",
    "conversationId": "CuvLPID4kDb",
    "created": "2016-10-28T21:19:51.0357965Z",
    "from": "examplebot",
    "text": "Hello!",
    "channelData": {
        "examplefield": "abc123"
    },
    "images": [
        "/attachments/CuvLPID4kDb/0.jpg?..."
    ],
    "attachments": [
        {
            "url": "https://example.com/example.docx",
            "contentType": "application/vnd.openxmlformats-officedocument.wordprocessingml.document"
        }, 
        {
            "url": "https://example.com/example.doc",
            "contentType": "application/msword"
        }
    ]
}
```

### <a name="messageset-object"></a><span data-ttu-id="bb7a4-251">Objeto MessageSet</span><span class="sxs-lookup"><span data-stu-id="bb7a4-251">MessageSet object</span></span> 
<span data-ttu-id="bb7a4-252">Define un conjunto de mensajes.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-252">Defines a set of messages.</span></span><br/><br/>

| <span data-ttu-id="bb7a4-253">Propiedad</span><span class="sxs-lookup"><span data-stu-id="bb7a4-253">Property</span></span> | <span data-ttu-id="bb7a4-254">Escriba</span><span class="sxs-lookup"><span data-stu-id="bb7a4-254">Type</span></span> | <span data-ttu-id="bb7a4-255">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="bb7a4-255">Description</span></span> |
|----|----|----|
| <span data-ttu-id="bb7a4-256">**messages**</span><span class="sxs-lookup"><span data-stu-id="bb7a4-256">**messages**</span></span> | <span data-ttu-id="bb7a4-257">[Message](#message-object)[]</span><span class="sxs-lookup"><span data-stu-id="bb7a4-257">[Message](#message-object)[]</span></span> | <span data-ttu-id="bb7a4-258">Matriz de objetos **Message**.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-258">Array of **Message** objects.</span></span> |
| <span data-ttu-id="bb7a4-259">**watermark**</span><span class="sxs-lookup"><span data-stu-id="bb7a4-259">**watermark**</span></span> | <span data-ttu-id="bb7a4-260">string</span><span class="sxs-lookup"><span data-stu-id="bb7a4-260">string</span></span> | <span data-ttu-id="bb7a4-261">Marca de agua máxima de mensajes dentro del conjunto.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-261">Maximum watermark of messages within the set.</span></span> <span data-ttu-id="bb7a4-262">Un cliente puede usar el valor `watermark` para indicar el mensaje más reciente que ha visto al [recuperar mensajes del bot](bot-framework-rest-direct-line-1-1-receive-messages.md).</span><span class="sxs-lookup"><span data-stu-id="bb7a4-262">A client may use the `watermark` value to indicate the most recent message it has seen when [retrieving messages from the bot](bot-framework-rest-direct-line-1-1-receive-messages.md).</span></span> |

### <a name="attachment-object"></a><span data-ttu-id="bb7a4-263">Objeto Attachment</span><span class="sxs-lookup"><span data-stu-id="bb7a4-263">Attachment object</span></span>
<span data-ttu-id="bb7a4-264">Define un dato adjunto que no es una imagen.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-264">Defines a non-image attachment.</span></span><br/><br/> 

| <span data-ttu-id="bb7a4-265">Propiedad</span><span class="sxs-lookup"><span data-stu-id="bb7a4-265">Property</span></span> | <span data-ttu-id="bb7a4-266">Escriba</span><span class="sxs-lookup"><span data-stu-id="bb7a4-266">Type</span></span> | <span data-ttu-id="bb7a4-267">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="bb7a4-267">Description</span></span> |
|----|----|----|
| <span data-ttu-id="bb7a4-268">**contentType**</span><span class="sxs-lookup"><span data-stu-id="bb7a4-268">**contentType**</span></span> | <span data-ttu-id="bb7a4-269">string</span><span class="sxs-lookup"><span data-stu-id="bb7a4-269">string</span></span> | <span data-ttu-id="bb7a4-270">Tipo de elemento multimedia del contenido de los datos adjuntos.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-270">The media type of the content in the attachment.</span></span> |
| <span data-ttu-id="bb7a4-271">**url**</span><span class="sxs-lookup"><span data-stu-id="bb7a4-271">**url**</span></span> | <span data-ttu-id="bb7a4-272">string</span><span class="sxs-lookup"><span data-stu-id="bb7a4-272">string</span></span> | <span data-ttu-id="bb7a4-273">Dirección URL del contenido de los datos adjuntos.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-273">URL for the content of the attachment.</span></span> |

### <a name="conversation-object"></a><span data-ttu-id="bb7a4-274">Objeto Conversation</span><span class="sxs-lookup"><span data-stu-id="bb7a4-274">Conversation object</span></span>
<span data-ttu-id="bb7a4-275">Define una conversación de Direct Line.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-275">Defines a Direct Line conversation.</span></span><br/><br/>

| <span data-ttu-id="bb7a4-276">Propiedad</span><span class="sxs-lookup"><span data-stu-id="bb7a4-276">Property</span></span> | <span data-ttu-id="bb7a4-277">Escriba</span><span class="sxs-lookup"><span data-stu-id="bb7a4-277">Type</span></span> | <span data-ttu-id="bb7a4-278">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="bb7a4-278">Description</span></span> |
|----|----|----|
| <span data-ttu-id="bb7a4-279">**conversationId**</span><span class="sxs-lookup"><span data-stu-id="bb7a4-279">**conversationId**</span></span> | <span data-ttu-id="bb7a4-280">string</span><span class="sxs-lookup"><span data-stu-id="bb7a4-280">string</span></span> | <span data-ttu-id="bb7a4-281">Identificador que identifica de forma única la conversación para la cual el token especificado es válido.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-281">ID that uniquely identifies the conversation for which the specified token is valid.</span></span> |
| <span data-ttu-id="bb7a4-282">**token**</span><span class="sxs-lookup"><span data-stu-id="bb7a4-282">**token**</span></span> | <span data-ttu-id="bb7a4-283">string</span><span class="sxs-lookup"><span data-stu-id="bb7a4-283">string</span></span> | <span data-ttu-id="bb7a4-284">Token válido para la conversación especificada.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-284">Token that is valid for the specified conversation.</span></span> |
| <span data-ttu-id="bb7a4-285">**expires_in**</span><span class="sxs-lookup"><span data-stu-id="bb7a4-285">**expires_in**</span></span> | <span data-ttu-id="bb7a4-286">número</span><span class="sxs-lookup"><span data-stu-id="bb7a4-286">number</span></span> | <span data-ttu-id="bb7a4-287">Número de segundos hasta que expira el token.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-287">Number of seconds until the token expires.</span></span> |

### <a name="error-object"></a><span data-ttu-id="bb7a4-288">Objeto Error</span><span class="sxs-lookup"><span data-stu-id="bb7a4-288">Error object</span></span>
<span data-ttu-id="bb7a4-289">Define un error.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-289">Defines an error.</span></span><br/><br/> 

| <span data-ttu-id="bb7a4-290">Propiedad</span><span class="sxs-lookup"><span data-stu-id="bb7a4-290">Property</span></span> | <span data-ttu-id="bb7a4-291">Escriba</span><span class="sxs-lookup"><span data-stu-id="bb7a4-291">Type</span></span> | <span data-ttu-id="bb7a4-292">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="bb7a4-292">Description</span></span> |
|----|----|----|
| <span data-ttu-id="bb7a4-293">**code**</span><span class="sxs-lookup"><span data-stu-id="bb7a4-293">**code**</span></span> | <span data-ttu-id="bb7a4-294">string</span><span class="sxs-lookup"><span data-stu-id="bb7a4-294">string</span></span> | <span data-ttu-id="bb7a4-295">Código de error.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-295">Error code.</span></span> <span data-ttu-id="bb7a4-296">Uno de estos valores: **MissingProperty**, **MalformedData**, **NotFound**, **ServiceError**, **Internal**, **InvalidRange**, **NotSupported**, **NotAllowed**, **BadCertificate**.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-296">One of these values: **MissingProperty**, **MalformedData**, **NotFound**, **ServiceError**, **Internal**, **InvalidRange**, **NotSupported**, **NotAllowed**, **BadCertificate**.</span></span> |
| <span data-ttu-id="bb7a4-297">**message**</span><span class="sxs-lookup"><span data-stu-id="bb7a4-297">**message**</span></span> | <span data-ttu-id="bb7a4-298">string</span><span class="sxs-lookup"><span data-stu-id="bb7a4-298">string</span></span> | <span data-ttu-id="bb7a4-299">Descripción del error.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-299">A description of the error.</span></span> |
| <span data-ttu-id="bb7a4-300">**statusCode**</span><span class="sxs-lookup"><span data-stu-id="bb7a4-300">**statusCode**</span></span> | <span data-ttu-id="bb7a4-301">número</span><span class="sxs-lookup"><span data-stu-id="bb7a4-301">number</span></span> | <span data-ttu-id="bb7a4-302">Código de estado.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-302">Status code.</span></span> |

### <a name="errormessage-object"></a><span data-ttu-id="bb7a4-303">Objeto ErrorMessage</span><span class="sxs-lookup"><span data-stu-id="bb7a4-303">ErrorMessage object</span></span>
<span data-ttu-id="bb7a4-304">Una carga estandarizada de errores de mensajes.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-304">A standardized message error payload.</span></span><br/><br/> 


|        <span data-ttu-id="bb7a4-305">Propiedad</span><span class="sxs-lookup"><span data-stu-id="bb7a4-305">Property</span></span>        |          <span data-ttu-id="bb7a4-306">Escriba</span><span class="sxs-lookup"><span data-stu-id="bb7a4-306">Type</span></span>          |                                 <span data-ttu-id="bb7a4-307">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="bb7a4-307">Description</span></span>                                 |
|------------------------|------------------------|-----------------------------------------------------------------------------|
| <span data-ttu-id="bb7a4-308"><strong>error</strong></span><span class="sxs-lookup"><span data-stu-id="bb7a4-308"><strong>error</strong></span></span> | [<span data-ttu-id="bb7a4-309">Error</span><span class="sxs-lookup"><span data-stu-id="bb7a4-309">Error</span></span>](#error-object) | <span data-ttu-id="bb7a4-310">Un objeto <strong>Error</strong> que contiene información sobre el error.</span><span class="sxs-lookup"><span data-stu-id="bb7a4-310">An <strong>Error</strong> object that contains information about the error.</span></span> |

