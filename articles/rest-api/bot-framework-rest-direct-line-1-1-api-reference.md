---
title: 'Referencia de API: Direct Line API 1.1 | Microsoft Docs'
description: Obtenga información sobre encabezados, códigos de estado HTTP, esquemas, operaciones y objetos en Direct Line API 1.1.
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ms.openlocfilehash: 3569e3bfbb3be51cf9023b4686ed4693e90ed50c
ms.sourcegitcommit: ee63d9dc1944a6843368bdabf5878950229f61d0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/23/2018
ms.locfileid: "42795184"
---
# <a name="api-reference---direct-line-api-11"></a><span data-ttu-id="9ac6c-103">Referencia de API: Direct Line API 1.1</span><span class="sxs-lookup"><span data-stu-id="9ac6c-103">API reference - Direct Line API 1.1</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9ac6c-104">Este artículo contiene información de referencia para Direct Line API 1.1.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-104">This article contains reference information for Direct Line API 1.1.</span></span> <span data-ttu-id="9ac6c-105">Si va a crear una nueva conexión entre la aplicación cliente y el bot, use [Direct Line API 3.0](bot-framework-rest-direct-line-3-0-api-reference.md) en su lugar.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-105">If you are creating a new connection between your client application and bot, use [Direct Line API 3.0](bot-framework-rest-direct-line-3-0-api-reference.md) instead.</span></span>

<span data-ttu-id="9ac6c-106">Para permitir que su aplicación cliente se comunique con su bot, puede usar Direct Line API 1.1.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-106">You can enable your client application to communicate with your bot by using Direct Line API 1.1.</span></span> <span data-ttu-id="9ac6c-107">Direct Line API 1.1 usa REST y JSON estándar del sector mediante HTTPS.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-107">Direct Line API 1.1 uses industry-standard REST and JSON over HTTPS.</span></span>

## <a name="base-uri"></a><span data-ttu-id="9ac6c-108">URI base</span><span class="sxs-lookup"><span data-stu-id="9ac6c-108">Base URI</span></span>

<span data-ttu-id="9ac6c-109">Para acceder a Direct Line API 1.1, use este URI base para todas las solicitudes de API:</span><span class="sxs-lookup"><span data-stu-id="9ac6c-109">To access Direct Line API 1.1, use this base URI for all API requests:</span></span>

`https://directline.botframework.com`

## <a name="headers"></a><span data-ttu-id="9ac6c-110">encabezados</span><span class="sxs-lookup"><span data-stu-id="9ac6c-110">Headers</span></span>

<span data-ttu-id="9ac6c-111">Además de los encabezados de solicitud HTTP estándar, una solicitud de Direct Line API debe incluir un encabezado `Authorization` que especifique un secreto o un token para autenticar al cliente que emite la solicitud.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-111">In addition to the standard HTTP request headers, a Direct Line API request must include an `Authorization` header that specifies a secret or token to authenticate the client that is issuing the request.</span></span> <span data-ttu-id="9ac6c-112">Puede especificar el encabezado `Authorization` mediante el esquema "Bearer" o "BotConnector".</span><span class="sxs-lookup"><span data-stu-id="9ac6c-112">You can specify the `Authorization` header using either the "Bearer" scheme or the "BotConnector" scheme.</span></span> 

<span data-ttu-id="9ac6c-113">**Esquema Bearer**:</span><span class="sxs-lookup"><span data-stu-id="9ac6c-113">**Bearer scheme**:</span></span>
```http
Authorization: Bearer SECRET_OR_TOKEN
```

<span data-ttu-id="9ac6c-114">**Esquema BotConnector**:</span><span class="sxs-lookup"><span data-stu-id="9ac6c-114">**BotConnector scheme**:</span></span>
```http
Authorization: BotConnector SECRET_OR_TOKEN
```

<span data-ttu-id="9ac6c-115">Para más información sobre cómo obtener un secreto o un token que el cliente pueda usar para autenticar sus solicitudes de Direct Line API, consulte [Autenticación](bot-framework-rest-direct-line-1-1-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="9ac6c-115">For details about how to obtain a secret or token that your client can use to authenticate its Direct Line API requests, see [Authentication](bot-framework-rest-direct-line-1-1-authentication.md).</span></span>

## <a name="http-status-codes"></a><span data-ttu-id="9ac6c-116">Códigos de estado HTTP</span><span class="sxs-lookup"><span data-stu-id="9ac6c-116">HTTP status codes</span></span>

<span data-ttu-id="9ac6c-117">El <a href="http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html" target="_blank">código de estado HTTP</a> que se devuelve con cada respuesta indica el resultado de la solicitud correspondiente.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-117">The <a href="http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html" target="_blank">HTTP status code</a> that is returned with each response indicates the outcome of the corresponding request.</span></span> 

| <span data-ttu-id="9ac6c-118">Código de estado HTTP</span><span class="sxs-lookup"><span data-stu-id="9ac6c-118">HTTP status code</span></span> | <span data-ttu-id="9ac6c-119">Significado</span><span class="sxs-lookup"><span data-stu-id="9ac6c-119">Meaning</span></span> |
|----|----|
| <span data-ttu-id="9ac6c-120">200</span><span class="sxs-lookup"><span data-stu-id="9ac6c-120">200</span></span> | <span data-ttu-id="9ac6c-121">La solicitud finalizó correctamente.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-121">The request succeeded.</span></span> |
| <span data-ttu-id="9ac6c-122">204</span><span class="sxs-lookup"><span data-stu-id="9ac6c-122">204</span></span> | <span data-ttu-id="9ac6c-123">La solicitud se realizó correctamente, pero no se devolvió ningún contenido.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-123">The request succeeded but no content was returned.</span></span> |
| <span data-ttu-id="9ac6c-124">400</span><span class="sxs-lookup"><span data-stu-id="9ac6c-124">400</span></span> | <span data-ttu-id="9ac6c-125">La solicitud tenía formato incorrecto o era incorrecta por otro motivo.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-125">The request was malformed or otherwise incorrect.</span></span> |
| <span data-ttu-id="9ac6c-126">401</span><span class="sxs-lookup"><span data-stu-id="9ac6c-126">401</span></span> | <span data-ttu-id="9ac6c-127">El cliente no está autorizado a realizar solicitudes.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-127">The client is not authorized to make the request.</span></span> <span data-ttu-id="9ac6c-128">A menudo, este código de estado se produce porque falta el encabezado `Authorization` o tiene un formato incorrecto.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-128">Often this status code occurs because the `Authorization` header is missing or malformed.</span></span> |
| <span data-ttu-id="9ac6c-129">403</span><span class="sxs-lookup"><span data-stu-id="9ac6c-129">403</span></span> | <span data-ttu-id="9ac6c-130">El cliente no tiene permitido llevar a cabo la operación solicitada.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-130">The client is not allowed to perform the requested operation.</span></span> <span data-ttu-id="9ac6c-131">A menudo, este código de estado se produce porque el encabezado `Authorization` especifica un token o un secreto no válidos.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-131">Often this status code occurs because the `Authorization` header specifies an invalid token or secret.</span></span> |
| <span data-ttu-id="9ac6c-132">404</span><span class="sxs-lookup"><span data-stu-id="9ac6c-132">404</span></span> | <span data-ttu-id="9ac6c-133">No se encontró el recurso solicitado.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-133">The requested resource was not found.</span></span> <span data-ttu-id="9ac6c-134">Normalmente, este código de estado indica un URI de solicitud no válido.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-134">Typically this status code indicates an invalid request URI.</span></span> |
| <span data-ttu-id="9ac6c-135">500</span><span class="sxs-lookup"><span data-stu-id="9ac6c-135">500</span></span> | <span data-ttu-id="9ac6c-136">Se ha producido un error interno del servidor en el servicio de Direct Line</span><span class="sxs-lookup"><span data-stu-id="9ac6c-136">An internal server error occurred within the Direct Line service</span></span> |
| <span data-ttu-id="9ac6c-137">502</span><span class="sxs-lookup"><span data-stu-id="9ac6c-137">502</span></span> | <span data-ttu-id="9ac6c-138">Se produjo un error en el bot; el bot no está disponible o devolvió un error.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-138">A failure occurred within the bot; the bot is unavailable or returned an error.</span></span>  <span data-ttu-id="9ac6c-139">**Se trata de un código de error común.**</span><span class="sxs-lookup"><span data-stu-id="9ac6c-139">**This is a common error code.**</span></span> |

## <a name="token-operations"></a><span data-ttu-id="9ac6c-140">Operaciones con tokens</span><span class="sxs-lookup"><span data-stu-id="9ac6c-140">Token operations</span></span> 
<span data-ttu-id="9ac6c-141">Use estas operaciones para crear o actualizar un token que un cliente pueda utilizar para acceder a una conversación única.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-141">Use these operations to create or refresh a token that a client can use to access a single conversation.</span></span>

| <span data-ttu-id="9ac6c-142">Operación</span><span class="sxs-lookup"><span data-stu-id="9ac6c-142">Operation</span></span> | <span data-ttu-id="9ac6c-143">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="9ac6c-143">Description</span></span> |
|----|----|
| [<span data-ttu-id="9ac6c-144">Generar token</span><span class="sxs-lookup"><span data-stu-id="9ac6c-144">Generate Token</span></span>](#generate-token) | <span data-ttu-id="9ac6c-145">Genera un token para una conversación nueva.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-145">Generate a token for a new conversation.</span></span> | 
| [<span data-ttu-id="9ac6c-146">Actualizar token</span><span class="sxs-lookup"><span data-stu-id="9ac6c-146">Refresh Token</span></span>](#refresh-token) | <span data-ttu-id="9ac6c-147">Actualiza un token.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-147">Refresh a token.</span></span> | 

### <a name="generate-token"></a><span data-ttu-id="9ac6c-148">Generar token</span><span class="sxs-lookup"><span data-stu-id="9ac6c-148">Generate Token</span></span>
<span data-ttu-id="9ac6c-149">Genera un token válido para una conversación.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-149">Generates a token that is valid for one conversation.</span></span> 
```http 
POST /api/tokens/conversation
```

| | |
|----|----|
| <span data-ttu-id="9ac6c-150">**Cuerpo de la solicitud**</span><span class="sxs-lookup"><span data-stu-id="9ac6c-150">**Request body**</span></span> | <span data-ttu-id="9ac6c-151">N/D</span><span class="sxs-lookup"><span data-stu-id="9ac6c-151">n/a</span></span> |
| <span data-ttu-id="9ac6c-152">**Devuelve**</span><span class="sxs-lookup"><span data-stu-id="9ac6c-152">**Returns**</span></span> | <span data-ttu-id="9ac6c-153">Una cadena que representa el token</span><span class="sxs-lookup"><span data-stu-id="9ac6c-153">A string that represents the token</span></span> | 

### <a name="refresh-token"></a><span data-ttu-id="9ac6c-154">Actualizar token</span><span class="sxs-lookup"><span data-stu-id="9ac6c-154">Refresh Token</span></span>
<span data-ttu-id="9ac6c-155">Actualiza el token.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-155">Refreshes the token.</span></span> 
```http 
GET /api/tokens/{conversationId}/renew
```

| | |
|----|----|
| <span data-ttu-id="9ac6c-156">**Cuerpo de la solicitud**</span><span class="sxs-lookup"><span data-stu-id="9ac6c-156">**Request body**</span></span> | <span data-ttu-id="9ac6c-157">N/D</span><span class="sxs-lookup"><span data-stu-id="9ac6c-157">n/a</span></span> |
| <span data-ttu-id="9ac6c-158">**Devuelve**</span><span class="sxs-lookup"><span data-stu-id="9ac6c-158">**Returns**</span></span> | <span data-ttu-id="9ac6c-159">Una cadena que representa el nuevo token</span><span class="sxs-lookup"><span data-stu-id="9ac6c-159">A string that represents the new token</span></span> | 

## <a name="conversation-operations"></a><span data-ttu-id="9ac6c-160">Operaciones con conversaciones</span><span class="sxs-lookup"><span data-stu-id="9ac6c-160">Conversation operations</span></span> 
<span data-ttu-id="9ac6c-161">Use estas operaciones para abrir una conversación con el bot e intercambiar mensajes entre el cliente y el bot.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-161">Use these operations to open a conversation with your bot and exchange messages between client and bot.</span></span>

| <span data-ttu-id="9ac6c-162">Operación</span><span class="sxs-lookup"><span data-stu-id="9ac6c-162">Operation</span></span> | <span data-ttu-id="9ac6c-163">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="9ac6c-163">Description</span></span> |
|----|----|
| [<span data-ttu-id="9ac6c-164">Iniciar conversación</span><span class="sxs-lookup"><span data-stu-id="9ac6c-164">Start Conversation</span></span>](#start-conversation) | <span data-ttu-id="9ac6c-165">Abre una nueva conversación con el bot.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-165">Opens a new conversation with the bot.</span></span> | 
| [<span data-ttu-id="9ac6c-166">Get Messages</span><span class="sxs-lookup"><span data-stu-id="9ac6c-166">Get Messages</span></span>](#get-messages) | <span data-ttu-id="9ac6c-167">Recibe mensajes del bot.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-167">Retrieves messages from the bot.</span></span> |
| [<span data-ttu-id="9ac6c-168">Enviar un mensaje</span><span class="sxs-lookup"><span data-stu-id="9ac6c-168">Send a Message</span></span>](#send-a-message) | <span data-ttu-id="9ac6c-169">Envía un mensaje al bot.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-169">Sends a message to the bot.</span></span> | 
| [<span data-ttu-id="9ac6c-170">Cargar y enviar archivos</span><span class="sxs-lookup"><span data-stu-id="9ac6c-170">Upload and Send File(s)</span></span>](#upload-send-files) | <span data-ttu-id="9ac6c-171">Carga y envía archivos como adjuntos.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-171">Uploads and sends file(s) as attachment(s).</span></span> |

### <a name="start-conversation"></a><span data-ttu-id="9ac6c-172">Iniciar conversación</span><span class="sxs-lookup"><span data-stu-id="9ac6c-172">Start Conversation</span></span>
<span data-ttu-id="9ac6c-173">Abre una nueva conversación con el bot.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-173">Opens a new conversation with the bot.</span></span> 
```http 
POST /api/conversations
```

| | |
|----|----|
| <span data-ttu-id="9ac6c-174">**Cuerpo de la solicitud**</span><span class="sxs-lookup"><span data-stu-id="9ac6c-174">**Request body**</span></span> | <span data-ttu-id="9ac6c-175">N/D</span><span class="sxs-lookup"><span data-stu-id="9ac6c-175">n/a</span></span> |
| <span data-ttu-id="9ac6c-176">**Devuelve**</span><span class="sxs-lookup"><span data-stu-id="9ac6c-176">**Returns**</span></span> | <span data-ttu-id="9ac6c-177">Un objeto [Conversation](#conversation-object)</span><span class="sxs-lookup"><span data-stu-id="9ac6c-177">A [Conversation](#conversation-object) object</span></span> | 

### <a name="get-messages"></a><span data-ttu-id="9ac6c-178">Obtener mensajes</span><span class="sxs-lookup"><span data-stu-id="9ac6c-178">Get Messages</span></span>
<span data-ttu-id="9ac6c-179">Recupera los mensajes del bot para la conversación especificada.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-179">Retrieves messages from the bot for the specified conversation.</span></span> <span data-ttu-id="9ac6c-180">Establezca el parámetro `watermark` en el URI de la solicitud para indicar el mensaje más reciente visto por el cliente.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-180">Set the `watermark` parameter in the request URI to indicate the most recent message seen by the client.</span></span> 

```http
GET /api/conversations/{conversationId}/messages?watermark={watermark_value}
```

| | |
|----|----|
| <span data-ttu-id="9ac6c-181">**Cuerpo de la solicitud**</span><span class="sxs-lookup"><span data-stu-id="9ac6c-181">**Request body**</span></span> | <span data-ttu-id="9ac6c-182">N/D</span><span class="sxs-lookup"><span data-stu-id="9ac6c-182">n/a</span></span> |
| <span data-ttu-id="9ac6c-183">**Devuelve**</span><span class="sxs-lookup"><span data-stu-id="9ac6c-183">**Returns**</span></span> | <span data-ttu-id="9ac6c-184">Un objeto [MessageSet](#messageset-object).</span><span class="sxs-lookup"><span data-stu-id="9ac6c-184">A [MessageSet](#messageset-object) object.</span></span> <span data-ttu-id="9ac6c-185">La respuesta contiene `watermark` como propiedad del objeto `MessageSet`.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-185">The response contains `watermark` as a property of the `MessageSet` object.</span></span> <span data-ttu-id="9ac6c-186">Los clientes deben desplazarse por los mensajes disponibles haciendo avanzar el valor `watermark` hasta que no se devuelvan mensajes.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-186">Clients should page through the available messages by advancing the `watermark` value until no messages are returned.</span></span> | 

### <a name="send-a-message"></a><span data-ttu-id="9ac6c-187">Enviar un mensaje</span><span class="sxs-lookup"><span data-stu-id="9ac6c-187">Send a Message</span></span>
<span data-ttu-id="9ac6c-188">Envía un mensaje al bot.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-188">Sends a message to the bot.</span></span> 
```http 
POST /api/conversations/{conversationId}/messages
```

| | |
|----|----|
| <span data-ttu-id="9ac6c-189">**Cuerpo de la solicitud**</span><span class="sxs-lookup"><span data-stu-id="9ac6c-189">**Request body**</span></span> | <span data-ttu-id="9ac6c-190">Un objeto [Message](#message-object)</span><span class="sxs-lookup"><span data-stu-id="9ac6c-190">A [Message](#message-object) object</span></span> |
| <span data-ttu-id="9ac6c-191">**Devuelve**</span><span class="sxs-lookup"><span data-stu-id="9ac6c-191">**Returns**</span></span> | <span data-ttu-id="9ac6c-192">No se devuelven datos en el cuerpo de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-192">No data is returned in body of the response.</span></span> <span data-ttu-id="9ac6c-193">El servicio responde con un código de estado HTTP 204, si el mensaje se envió correctamente.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-193">The service responds with an HTTP 204 status code if the message was sent successfully.</span></span> <span data-ttu-id="9ac6c-194">El cliente puede obtener su mensaje enviado (junto con los mensajes que el bot ha enviado al cliente) mediante la operación [Obtener mensajes](#get-messages).</span><span class="sxs-lookup"><span data-stu-id="9ac6c-194">The client may obtain its sent message (along with any messages that the bot has sent to the client) by using the [Get Messages](#get-messages) operation.</span></span> |

### <a id="upload-send-files"></a> <span data-ttu-id="9ac6c-195">Cargar y enviar archivos</span><span class="sxs-lookup"><span data-stu-id="9ac6c-195">Upload and Send File(s)</span></span>
<span data-ttu-id="9ac6c-196">Carga y envía archivos como adjuntos.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-196">Uploads and sends file(s) as attachment(s).</span></span> <span data-ttu-id="9ac6c-197">Establezca el parámetro `userId` en el URI de la solicitud para especificar el identificador del usuario que envía los datos adjuntos.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-197">Set the `userId` parameter in the request URI to specify the ID of the user that is sending the attachments.</span></span>
```http 
POST /api/conversations/{conversationId}/upload?userId={userId}
```

| | |
|----|----|
| <span data-ttu-id="9ac6c-198">**Cuerpo de la solicitud**</span><span class="sxs-lookup"><span data-stu-id="9ac6c-198">**Request body**</span></span> | <span data-ttu-id="9ac6c-199">Para un único dato adjunto, rellene el cuerpo de la solicitud con el contenido del archivo.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-199">For a single attachment, populate the request body with the file contents.</span></span> <span data-ttu-id="9ac6c-200">Para varios datos adjuntos, cree un cuerpo de solicitud de varias partes que contenga una parte para cada dato adjunto y, además (de manera opcional), una parte para el objeto [Message](#message-object) que debe actuar como contenedor para los datos adjuntos especificados.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-200">For multiple attachments, create a multipart request body that contains one part for each attachment, and also (optionally) one part for the [Message](#message-object) object that should serve as the container for the specified attachment(s).</span></span> <span data-ttu-id="9ac6c-201">Para más información, consulte [Envío de una actividad al bot](bot-framework-rest-direct-line-1-1-send-message.md).</span><span class="sxs-lookup"><span data-stu-id="9ac6c-201">For more information, see [Send a message to the bot](bot-framework-rest-direct-line-1-1-send-message.md).</span></span> |
| <span data-ttu-id="9ac6c-202">**Devuelve**</span><span class="sxs-lookup"><span data-stu-id="9ac6c-202">**Returns**</span></span> | <span data-ttu-id="9ac6c-203">No se devuelven datos en el cuerpo de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-203">No data is returned in body of the response.</span></span> <span data-ttu-id="9ac6c-204">El servicio responde con un código de estado HTTP 204, si el mensaje se envió correctamente.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-204">The service responds with an HTTP 204 status code if the message was sent successfully.</span></span> <span data-ttu-id="9ac6c-205">El cliente puede obtener su mensaje enviado (junto con los mensajes que el bot ha enviado al cliente) mediante la operación [Obtener mensajes](#get-messages).</span><span class="sxs-lookup"><span data-stu-id="9ac6c-205">The client may obtain its sent message (along with any messages that the bot has sent to the client) by using the [Get Messages](#get-messages) operation.</span></span> | 

> [!NOTE]
> <span data-ttu-id="9ac6c-206">Los archivos cargados se eliminan después de 24 horas.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-206">Uploaded files are deleted after 24 hours.</span></span>

## <a name="schema"></a><span data-ttu-id="9ac6c-207">Esquema</span><span class="sxs-lookup"><span data-stu-id="9ac6c-207">Schema</span></span>

<span data-ttu-id="9ac6c-208">El esquema de Direct Line 1.1 es una copia simplificada del esquema de Bot Framework v1 que incluye los siguientes objetos.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-208">Direct Line 1.1 schema is a simplified copy of the Bot Framework v1 schema that includes the following objects.</span></span>

### <a name="message-object"></a><span data-ttu-id="9ac6c-209">Objeto de mensaje</span><span class="sxs-lookup"><span data-stu-id="9ac6c-209">Message object</span></span>

<span data-ttu-id="9ac6c-210">Define un mensaje que un cliente envía a un bot o recibe de un bot.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-210">Defines a message that a client sends to a bot or receives from a bot.</span></span>

| <span data-ttu-id="9ac6c-211">Propiedad</span><span class="sxs-lookup"><span data-stu-id="9ac6c-211">Property</span></span> | <span data-ttu-id="9ac6c-212">Escriba</span><span class="sxs-lookup"><span data-stu-id="9ac6c-212">Type</span></span> | <span data-ttu-id="9ac6c-213">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="9ac6c-213">Description</span></span> |
|----|----|----|
| <span data-ttu-id="9ac6c-214">**id**</span><span class="sxs-lookup"><span data-stu-id="9ac6c-214">**id**</span></span> | <span data-ttu-id="9ac6c-215">string</span><span class="sxs-lookup"><span data-stu-id="9ac6c-215">string</span></span> | <span data-ttu-id="9ac6c-216">Identificador que identifica de forma única el mensaje (asignado por Direct Line).</span><span class="sxs-lookup"><span data-stu-id="9ac6c-216">ID that uniquely identifies the message (assigned by Direct Line).</span></span> | 
| <span data-ttu-id="9ac6c-217">**conversationId**</span><span class="sxs-lookup"><span data-stu-id="9ac6c-217">**conversationId**</span></span> | <span data-ttu-id="9ac6c-218">string</span><span class="sxs-lookup"><span data-stu-id="9ac6c-218">string</span></span> | <span data-ttu-id="9ac6c-219">Identificador que identifica la conversación.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-219">ID that identifies the conversation.</span></span>  | 
| <span data-ttu-id="9ac6c-220">**created**</span><span class="sxs-lookup"><span data-stu-id="9ac6c-220">**created**</span></span> | <span data-ttu-id="9ac6c-221">string</span><span class="sxs-lookup"><span data-stu-id="9ac6c-221">string</span></span> | <span data-ttu-id="9ac6c-222">Fecha y hora en que se creó el mensaje, expresado en el formato <a href="https://en.wikipedia.org/wiki/ISO_8601" target="_blank">ISO-8601</a>.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-222">Date and time that the message was created, expressed in <a href="https://en.wikipedia.org/wiki/ISO_8601" target="_blank">ISO-8601</a> format.</span></span> | 
| <span data-ttu-id="9ac6c-223">**from**</span><span class="sxs-lookup"><span data-stu-id="9ac6c-223">**from**</span></span> | <span data-ttu-id="9ac6c-224">string</span><span class="sxs-lookup"><span data-stu-id="9ac6c-224">string</span></span> | <span data-ttu-id="9ac6c-225">Identificador que identifica el usuario que es el remitente del mensaje.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-225">ID that identifies the user that is the sender of the message.</span></span> <span data-ttu-id="9ac6c-226">Al crear un mensaje, los clientes deben establecer esta propiedad en un identificador de usuario estable.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-226">When creating a message, clients should set this property to a stable user ID.</span></span> <span data-ttu-id="9ac6c-227">Aunque Direct Line asignará un identificador de usuario si no se proporciona ninguno, esto da lugar normalmente a un comportamiento inesperado.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-227">Although Direct Line will assign a user ID if none is supplied, this typically results in unexpected behavior.</span></span> | 
| <span data-ttu-id="9ac6c-228">**text**</span><span class="sxs-lookup"><span data-stu-id="9ac6c-228">**text**</span></span> | <span data-ttu-id="9ac6c-229">string</span><span class="sxs-lookup"><span data-stu-id="9ac6c-229">string</span></span> | <span data-ttu-id="9ac6c-230">Texto del mensaje que se envía del usuario al bot o del bot al usuario.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-230">Text of the message that is sent from user to bot or bot to user.</span></span> | 
| <span data-ttu-id="9ac6c-231">**channelData**</span><span class="sxs-lookup"><span data-stu-id="9ac6c-231">**channelData**</span></span> | <span data-ttu-id="9ac6c-232">objeto</span><span class="sxs-lookup"><span data-stu-id="9ac6c-232">object</span></span> | <span data-ttu-id="9ac6c-233">Objeto que incluye contenido específico de canal.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-233">An object that contains channel-specific content.</span></span> <span data-ttu-id="9ac6c-234">Algunos canales proporcionan características que requieren información adicional que no se puede representar mediante el esquema de datos adjuntos.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-234">Some channels provide features that require additional information that cannot be represented using the attachment schema.</span></span> <span data-ttu-id="9ac6c-235">Para esos casos, establezca esta propiedad en el contenido específico de canal tal como se define en la documentación del canal.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-235">For those cases, set this property to the channel-specific content as defined in the channel's documentation.</span></span> <span data-ttu-id="9ac6c-236">Estos datos se envían sin modificar entre el cliente y el bot.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-236">This data is sent unmodified between client and bot.</span></span> <span data-ttu-id="9ac6c-237">Esta propiedad se debe establecer en un objeto complejo o dejarse vacía.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-237">This property must either be set to a complex object or left empty.</span></span> <span data-ttu-id="9ac6c-238">No la establezca en una cadena, número u otro tipo simple.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-238">Do not set it to a string, number, or other simple type.</span></span> | 
| <span data-ttu-id="9ac6c-239">**images**</span><span class="sxs-lookup"><span data-stu-id="9ac6c-239">**images**</span></span> | <span data-ttu-id="9ac6c-240">string[]</span><span class="sxs-lookup"><span data-stu-id="9ac6c-240">string[]</span></span> | <span data-ttu-id="9ac6c-241">Matriz de cadenas que contiene las direcciones URL para las imágenes que contiene el mensaje.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-241">Array of strings that contains the URL(s) for the image(s) that the message contains.</span></span> <span data-ttu-id="9ac6c-242">En algunos casos, las cadenas de esta matriz pueden ser direcciones URL relativas.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-242">In some cases, strings in this array may be relative URLs.</span></span> <span data-ttu-id="9ac6c-243">Si cualquier cadena de esta matriz no comienza con "http" o "https", anteponga `https://directline.botframework.com` a la cadena para formar la dirección URL completa.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-243">If any string in this array does not begin with either "http" or "https", prepend `https://directline.botframework.com` to the string to form the complete URL.</span></span> | 
| <span data-ttu-id="9ac6c-244">**attachments**</span><span class="sxs-lookup"><span data-stu-id="9ac6c-244">**attachments**</span></span> | <span data-ttu-id="9ac6c-245">[Attachment](#attachment-object)[]</span><span class="sxs-lookup"><span data-stu-id="9ac6c-245">[Attachment](#attachment-object)[]</span></span> | <span data-ttu-id="9ac6c-246">Matriz de objetos **Attachment** que representan los datos adjuntos que no son imágenes que contiene el mensaje.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-246">Array of **Attachment** objects that represent the non-image attachments that the message contains.</span></span> <span data-ttu-id="9ac6c-247">Cada objeto de la matriz contiene una propiedad `url` y una propiedad `contentType`.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-247">Each object in the array contains a `url` property and a `contentType` property.</span></span> <span data-ttu-id="9ac6c-248">En los mensajes que recibe un cliente de un bot, la propiedad `url` puede especificar a veces una dirección URL relativa.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-248">In messages that a client receives from a bot, the `url` property may sometimes specify a relative URL.</span></span> <span data-ttu-id="9ac6c-249">Para cualquier valor de propiedad `url` que no comience por "http" o "https", anteponga `https://directline.botframework.com` a la cadena para formar la dirección URL completa.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-249">For any `url` property value that does not begin with either "http" or "https", prepend `https://directline.botframework.com` to the string to form the complete URL.</span></span> | 

<span data-ttu-id="9ac6c-250">En el ejemplo siguiente se muestra un objeto Message que contiene todas las propiedades posibles.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-250">The following example shows a Message object that contains all possible properties.</span></span> <span data-ttu-id="9ac6c-251">En la mayoría de los casos al crear un mensaje, el cliente solo tiene que proporcionar la propiedad `from` y al menos una propiedad de contenido (por ejemplo, `text`, `images`, `attachments` o `channelData`).</span><span class="sxs-lookup"><span data-stu-id="9ac6c-251">In most cases when creating a message, the client only needs to supply the `from` property and at least one content property (e.g., `text`, `images`, `attachments`, or `channelData`).</span></span>

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

### <a name="messageset-object"></a><span data-ttu-id="9ac6c-252">Objeto MessageSet</span><span class="sxs-lookup"><span data-stu-id="9ac6c-252">MessageSet object</span></span> 
<span data-ttu-id="9ac6c-253">Define un conjunto de mensajes.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-253">Defines a set of messages.</span></span><br/><br/>

| <span data-ttu-id="9ac6c-254">Propiedad</span><span class="sxs-lookup"><span data-stu-id="9ac6c-254">Property</span></span> | <span data-ttu-id="9ac6c-255">Escriba</span><span class="sxs-lookup"><span data-stu-id="9ac6c-255">Type</span></span> | <span data-ttu-id="9ac6c-256">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="9ac6c-256">Description</span></span> |
|----|----|----|
| <span data-ttu-id="9ac6c-257">**messages**</span><span class="sxs-lookup"><span data-stu-id="9ac6c-257">**messages**</span></span> | <span data-ttu-id="9ac6c-258">[Message](#message-object)[]</span><span class="sxs-lookup"><span data-stu-id="9ac6c-258">[Message](#message-object)[]</span></span> | <span data-ttu-id="9ac6c-259">Matriz de objetos **Message**.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-259">Array of **Message** objects.</span></span> |
| <span data-ttu-id="9ac6c-260">**watermark**</span><span class="sxs-lookup"><span data-stu-id="9ac6c-260">**watermark**</span></span> | <span data-ttu-id="9ac6c-261">string</span><span class="sxs-lookup"><span data-stu-id="9ac6c-261">string</span></span> | <span data-ttu-id="9ac6c-262">Marca de agua máxima de mensajes dentro del conjunto.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-262">Maximum watermark of messages within the set.</span></span> <span data-ttu-id="9ac6c-263">Un cliente puede usar el valor `watermark` para indicar el mensaje más reciente que ha visto al [recuperar mensajes del bot](bot-framework-rest-direct-line-1-1-receive-messages.md).</span><span class="sxs-lookup"><span data-stu-id="9ac6c-263">A client may use the `watermark` value to indicate the most recent message it has seen when [retrieving messages from the bot](bot-framework-rest-direct-line-1-1-receive-messages.md).</span></span> |

### <a name="attachment-object"></a><span data-ttu-id="9ac6c-264">Objeto Attachment</span><span class="sxs-lookup"><span data-stu-id="9ac6c-264">Attachment object</span></span>
<span data-ttu-id="9ac6c-265">Define un dato adjunto que no es una imagen.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-265">Defines a non-image attachment.</span></span><br/><br/> 

| <span data-ttu-id="9ac6c-266">Propiedad</span><span class="sxs-lookup"><span data-stu-id="9ac6c-266">Property</span></span> | <span data-ttu-id="9ac6c-267">Escriba</span><span class="sxs-lookup"><span data-stu-id="9ac6c-267">Type</span></span> | <span data-ttu-id="9ac6c-268">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="9ac6c-268">Description</span></span> |
|----|----|----|
| <span data-ttu-id="9ac6c-269">**contentType**</span><span class="sxs-lookup"><span data-stu-id="9ac6c-269">**contentType**</span></span> | <span data-ttu-id="9ac6c-270">string</span><span class="sxs-lookup"><span data-stu-id="9ac6c-270">string</span></span> | <span data-ttu-id="9ac6c-271">Tipo de elemento multimedia del contenido de los datos adjuntos.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-271">The media type of the content in the attachment.</span></span> |
| <span data-ttu-id="9ac6c-272">**url**</span><span class="sxs-lookup"><span data-stu-id="9ac6c-272">**url**</span></span> | <span data-ttu-id="9ac6c-273">string</span><span class="sxs-lookup"><span data-stu-id="9ac6c-273">string</span></span> | <span data-ttu-id="9ac6c-274">Dirección URL del contenido de los datos adjuntos.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-274">URL for the content of the attachment.</span></span> |

### <a name="conversation-object"></a><span data-ttu-id="9ac6c-275">Objeto Conversation</span><span class="sxs-lookup"><span data-stu-id="9ac6c-275">Conversation object</span></span>
<span data-ttu-id="9ac6c-276">Define una conversación de Direct Line.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-276">Defines a Direct Line conversation.</span></span><br/><br/>

| <span data-ttu-id="9ac6c-277">Propiedad</span><span class="sxs-lookup"><span data-stu-id="9ac6c-277">Property</span></span> | <span data-ttu-id="9ac6c-278">Escriba</span><span class="sxs-lookup"><span data-stu-id="9ac6c-278">Type</span></span> | <span data-ttu-id="9ac6c-279">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="9ac6c-279">Description</span></span> |
|----|----|----|
| <span data-ttu-id="9ac6c-280">**conversationId**</span><span class="sxs-lookup"><span data-stu-id="9ac6c-280">**conversationId**</span></span> | <span data-ttu-id="9ac6c-281">string</span><span class="sxs-lookup"><span data-stu-id="9ac6c-281">string</span></span> | <span data-ttu-id="9ac6c-282">Identificador que identifica de forma única la conversación para la cual el token especificado es válido.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-282">ID that uniquely identifies the conversation for which the specified token is valid.</span></span> |
| <span data-ttu-id="9ac6c-283">**token**</span><span class="sxs-lookup"><span data-stu-id="9ac6c-283">**token**</span></span> | <span data-ttu-id="9ac6c-284">string</span><span class="sxs-lookup"><span data-stu-id="9ac6c-284">string</span></span> | <span data-ttu-id="9ac6c-285">Token válido para la conversación especificada.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-285">Token that is valid for the specified conversation.</span></span> |
| <span data-ttu-id="9ac6c-286">**expires_in**</span><span class="sxs-lookup"><span data-stu-id="9ac6c-286">**expires_in**</span></span> | <span data-ttu-id="9ac6c-287">número</span><span class="sxs-lookup"><span data-stu-id="9ac6c-287">number</span></span> | <span data-ttu-id="9ac6c-288">Número de segundos hasta que expira el token.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-288">Number of seconds until the token expires.</span></span> |

### <a name="error-object"></a><span data-ttu-id="9ac6c-289">Objeto Error</span><span class="sxs-lookup"><span data-stu-id="9ac6c-289">Error object</span></span>
<span data-ttu-id="9ac6c-290">Define un error.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-290">Defines an error.</span></span><br/><br/> 

| <span data-ttu-id="9ac6c-291">Propiedad</span><span class="sxs-lookup"><span data-stu-id="9ac6c-291">Property</span></span> | <span data-ttu-id="9ac6c-292">Escriba</span><span class="sxs-lookup"><span data-stu-id="9ac6c-292">Type</span></span> | <span data-ttu-id="9ac6c-293">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="9ac6c-293">Description</span></span> |
|----|----|----|
| <span data-ttu-id="9ac6c-294">**code**</span><span class="sxs-lookup"><span data-stu-id="9ac6c-294">**code**</span></span> | <span data-ttu-id="9ac6c-295">string</span><span class="sxs-lookup"><span data-stu-id="9ac6c-295">string</span></span> | <span data-ttu-id="9ac6c-296">Código de error.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-296">Error code.</span></span> <span data-ttu-id="9ac6c-297">Uno de estos valores: **MissingProperty**, **MalformedData**, **NotFound**, **ServiceError**, **Internal**, **InvalidRange**, **NotSupported**, **NotAllowed**, **BadCertificate**.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-297">One of these values: **MissingProperty**, **MalformedData**, **NotFound**, **ServiceError**, **Internal**, **InvalidRange**, **NotSupported**, **NotAllowed**, **BadCertificate**.</span></span> |
| <span data-ttu-id="9ac6c-298">**message**</span><span class="sxs-lookup"><span data-stu-id="9ac6c-298">**message**</span></span> | <span data-ttu-id="9ac6c-299">string</span><span class="sxs-lookup"><span data-stu-id="9ac6c-299">string</span></span> | <span data-ttu-id="9ac6c-300">Descripción del error.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-300">A description of the error.</span></span> |
| <span data-ttu-id="9ac6c-301">**statusCode**</span><span class="sxs-lookup"><span data-stu-id="9ac6c-301">**statusCode**</span></span> | <span data-ttu-id="9ac6c-302">número</span><span class="sxs-lookup"><span data-stu-id="9ac6c-302">number</span></span> | <span data-ttu-id="9ac6c-303">Código de estado.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-303">Status code.</span></span> |

### <a name="errormessage-object"></a><span data-ttu-id="9ac6c-304">Objeto ErrorMessage</span><span class="sxs-lookup"><span data-stu-id="9ac6c-304">ErrorMessage object</span></span>
<span data-ttu-id="9ac6c-305">Una carga estandarizada de errores de mensajes.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-305">A standardized message error payload.</span></span><br/><br/> 


|        <span data-ttu-id="9ac6c-306">Propiedad</span><span class="sxs-lookup"><span data-stu-id="9ac6c-306">Property</span></span>        |          <span data-ttu-id="9ac6c-307">Escriba</span><span class="sxs-lookup"><span data-stu-id="9ac6c-307">Type</span></span>          |                                 <span data-ttu-id="9ac6c-308">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="9ac6c-308">Description</span></span>                                 |
|------------------------|------------------------|-----------------------------------------------------------------------------|
| <span data-ttu-id="9ac6c-309"><strong>error</strong></span><span class="sxs-lookup"><span data-stu-id="9ac6c-309"><strong>error</strong></span></span> | [<span data-ttu-id="9ac6c-310">Error</span><span class="sxs-lookup"><span data-stu-id="9ac6c-310">Error</span></span>](#error-object) | <span data-ttu-id="9ac6c-311">Un objeto <strong>Error</strong> que contiene información sobre el error.</span><span class="sxs-lookup"><span data-stu-id="9ac6c-311">An <strong>Error</strong> object that contains information about the error.</span></span> |

