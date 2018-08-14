---
title: 'Referencia de API: Direct Line API 3.0 | Microsoft Docs'
description: Obtenga información sobre encabezados, códigos de estado HTTP, esquemas, operaciones y objetos en Direct Line API 3.0.
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ms.openlocfilehash: 2e47591b04a91ce02cfeb6bd6485080426d201b5
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305093"
---
# <a name="api-reference---direct-line-api-30"></a><span data-ttu-id="fd9bc-103">Referencia de API: Direct Line API 3.0</span><span class="sxs-lookup"><span data-stu-id="fd9bc-103">API reference - Direct Line API 3.0</span></span>

<span data-ttu-id="fd9bc-104">Para permitir que su aplicación cliente se comunique con su bot, puede usar Direct Line API 3.0.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-104">You can enable your client application to communicate with your bot by using Direct Line API 3.0.</span></span> <span data-ttu-id="fd9bc-105">Direct Line API 3.0 usa REST y JSON estándares a través de HTTPS.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-105">Direct Line API 3.0 uses industry-standard REST and JSON over HTTPS.</span></span>

## <a name="base-uri"></a><span data-ttu-id="fd9bc-106">URI base</span><span class="sxs-lookup"><span data-stu-id="fd9bc-106">Base URI</span></span>

<span data-ttu-id="fd9bc-107">Para obtener acceso a Direct Line API 3.0, use este URI base para todas las solicitudes de API:</span><span class="sxs-lookup"><span data-stu-id="fd9bc-107">To access Direct Line API 3.0, use this base URI for all API requests:</span></span>

`https://directline.botframework.com`

## <a name="headers"></a><span data-ttu-id="fd9bc-108">encabezados</span><span class="sxs-lookup"><span data-stu-id="fd9bc-108">Headers</span></span>

<span data-ttu-id="fd9bc-109">Además de los encabezados de solicitud HTTP estándares, una solicitud de Direct Line API debe incluir un encabezado `Authorization` que especifique un secreto o un token para autenticar al cliente que emite la solicitud.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-109">In addition to the standard HTTP request headers, a Direct Line API request must include an `Authorization` header that specifies a secret or token to authenticate the client that is issuing the request.</span></span> <span data-ttu-id="fd9bc-110">Especifique el encabezado `Authorization` con este formato:</span><span class="sxs-lookup"><span data-stu-id="fd9bc-110">Specify the `Authorization` header using this format:</span></span>

```http
Authorization: Bearer SECRET_OR_TOKEN
```

<span data-ttu-id="fd9bc-111">Para obtener más información sobre cómo obtener un secreto o un token que el cliente pueda usar para autenticar sus solicitudes de Direct Line API, consulte [Autenticación](bot-framework-rest-direct-line-3-0-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="fd9bc-111">For details about how to obtain a secret or token that your client can use to authenticate its Direct Line API requests, see [Authentication](bot-framework-rest-direct-line-3-0-authentication.md).</span></span>

## <a name="http-status-codes"></a><span data-ttu-id="fd9bc-112">Códigos de estado HTTP</span><span class="sxs-lookup"><span data-stu-id="fd9bc-112">HTTP status codes</span></span>

<span data-ttu-id="fd9bc-113">El <a href="http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html" target="_blank">código de estado HTTP</a> que se devuelve con cada respuesta indica el resultado de la solicitud correspondiente.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-113">The <a href="http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html" target="_blank">HTTP status code</a> that is returned with each response indicates the outcome of the corresponding request.</span></span> 

| <span data-ttu-id="fd9bc-114">Código de estado HTTP</span><span class="sxs-lookup"><span data-stu-id="fd9bc-114">HTTP status code</span></span> | <span data-ttu-id="fd9bc-115">Significado</span><span class="sxs-lookup"><span data-stu-id="fd9bc-115">Meaning</span></span> |
|----|----|
| <span data-ttu-id="fd9bc-116">200</span><span class="sxs-lookup"><span data-stu-id="fd9bc-116">200</span></span> | <span data-ttu-id="fd9bc-117">La solicitud finalizó correctamente.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-117">The request succeeded.</span></span> |
| <span data-ttu-id="fd9bc-118">201</span><span class="sxs-lookup"><span data-stu-id="fd9bc-118">201</span></span> | <span data-ttu-id="fd9bc-119">La solicitud finalizó correctamente.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-119">The request succeeded.</span></span> |
| <span data-ttu-id="fd9bc-120">202</span><span class="sxs-lookup"><span data-stu-id="fd9bc-120">202</span></span> | <span data-ttu-id="fd9bc-121">La solicitud se ha aceptado para procesamiento.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-121">The request has been accepted for processing.</span></span> |
| <span data-ttu-id="fd9bc-122">204</span><span class="sxs-lookup"><span data-stu-id="fd9bc-122">204</span></span> | <span data-ttu-id="fd9bc-123">La solicitud se realizó correctamente, pero no se devolvió ningún contenido.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-123">The request succeeded but no content was returned.</span></span> |
| <span data-ttu-id="fd9bc-124">400</span><span class="sxs-lookup"><span data-stu-id="fd9bc-124">400</span></span> | <span data-ttu-id="fd9bc-125">La solicitud tenía formato incorrecto o era incorrecta por otro motivo.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-125">The request was malformed or otherwise incorrect.</span></span> |
| <span data-ttu-id="fd9bc-126">401</span><span class="sxs-lookup"><span data-stu-id="fd9bc-126">401</span></span> | <span data-ttu-id="fd9bc-127">El cliente no está autorizado a realizar solicitudes.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-127">The client is not authorized to make the request.</span></span> <span data-ttu-id="fd9bc-128">A menudo, este código de estado se produce porque falta el encabezado `Authorization` o tiene un formato incorrecto.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-128">Often this status code occurs because the `Authorization` header is missing or malformed.</span></span> |
| <span data-ttu-id="fd9bc-129">403</span><span class="sxs-lookup"><span data-stu-id="fd9bc-129">403</span></span> | <span data-ttu-id="fd9bc-130">El cliente no tiene permitido llevar a cabo la operación solicitada.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-130">The client is not allowed to perform the requested operation.</span></span> <span data-ttu-id="fd9bc-131">Si la solicitud especificaba un token que previamente era válido, pero ha expirado, la propiedad `code` del [Error](bot-framework-rest-connector-api-reference.md#error-object) que se devuelve dentro del objeto [ErrorResponse](bot-framework-rest-connector-api-reference.md#errorresponse-object) se establece en `TokenExpired`.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-131">If the request specified a token that was previously valid but has expired, the `code` property of the [Error](bot-framework-rest-connector-api-reference.md#error-object) that is returned within the [ErrorResponse](bot-framework-rest-connector-api-reference.md#errorresponse-object) object is set to `TokenExpired`.</span></span> |
| <span data-ttu-id="fd9bc-132">404</span><span class="sxs-lookup"><span data-stu-id="fd9bc-132">404</span></span> | <span data-ttu-id="fd9bc-133">No se encontró el recurso solicitado.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-133">The requested resource was not found.</span></span> <span data-ttu-id="fd9bc-134">Normalmente, este código de estado indica un URI de solicitud no válido.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-134">Typically this status code indicates an invalid request URI.</span></span> |
| <span data-ttu-id="fd9bc-135">500</span><span class="sxs-lookup"><span data-stu-id="fd9bc-135">500</span></span> | <span data-ttu-id="fd9bc-136">Se ha producido un error interno del servidor en el servicio de Direct Line.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-136">An internal server error occurred within the Direct Line service.</span></span> |
| <span data-ttu-id="fd9bc-137">502</span><span class="sxs-lookup"><span data-stu-id="fd9bc-137">502</span></span> | <span data-ttu-id="fd9bc-138">El bot no está disponible o devolvió un error.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-138">The bot is unavailable or returned an error.</span></span> <span data-ttu-id="fd9bc-139">**Se trata de un código de error común.**</span><span class="sxs-lookup"><span data-stu-id="fd9bc-139">**This is a common error code.**</span></span> |

> [!NOTE]
> <span data-ttu-id="fd9bc-140">El código de estado HTTP 101 se usa en la ruta de acceso de conexión de WebSocket, aunque es probable que el cliente de WebSocket controle esto.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-140">HTTP status code 101 is used in the WebSocket connection path, although this is likely handled by your WebSocket client.</span></span>

### <a name="errors"></a><span data-ttu-id="fd9bc-141">Errors</span><span class="sxs-lookup"><span data-stu-id="fd9bc-141">Errors</span></span>

<span data-ttu-id="fd9bc-142">Cualquier respuesta que especifique un código de estado HTTP en el rango de 4xx o 5xx incluirá un objeto [ErrorResponse](bot-framework-rest-connector-api-reference.md#errorresponse-object) en el cuerpo de la respuesta que proporciona información sobre el error.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-142">Any response that specifies an HTTP status code in the 4xx range or 5xx range will include an [ErrorResponse](bot-framework-rest-connector-api-reference.md#errorresponse-object) object in the body of the response that provides information about the error.</span></span> <span data-ttu-id="fd9bc-143">Si recibe una respuesta de error en el rango de 4xx, inspeccione el objeto **ErrorResponse** para identificar la causa del error y resolver el problema antes de volver a enviar la solicitud.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-143">If you receive an error response in the 4xx range, inspect the **ErrorResponse** object to identify the cause of the error and resolve your issue prior to resubmitting the request.</span></span>

> [!NOTE]
> <span data-ttu-id="fd9bc-144">Los códigos de estado HTTP y los valores especificados en la propiedad `code` dentro del objeto **ErrorResponse** son estables.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-144">HTTP status codes and values specified in the `code` property inside the **ErrorResponse** object are stable.</span></span> <span data-ttu-id="fd9bc-145">Los valores especificados en la propiedad `message` dentro del objeto **ErrorResponse** pueden cambiar con el tiempo.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-145">Values specified in the `message` property inside the **ErrorResponse** object may change over time.</span></span>

<span data-ttu-id="fd9bc-146">Los fragmentos de código siguientes muestran un ejemplo de solicitud y la respuesta de error resultante.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-146">The following snippets show an example request and the resulting error response.</span></span>

#### <a name="request"></a><span data-ttu-id="fd9bc-147">Solicitud</span><span class="sxs-lookup"><span data-stu-id="fd9bc-147">Request</span></span>

```http
POST https://directline.botframework.com/v3/directline/conversations/abc123/activities
[detail omitted]
```

#### <a name="response"></a><span data-ttu-id="fd9bc-148">Response</span><span class="sxs-lookup"><span data-stu-id="fd9bc-148">Response</span></span>
```http
HTTP/1.1 502 Bad Gateway
[other headers]
```
```json
{
    "error": {
        "code": "BotRejectedActivity",
        "message": "Failed to send activity: bot returned an error"
    }
}
```

## <a name="token-operations"></a><span data-ttu-id="fd9bc-149">Operaciones con tokens</span><span class="sxs-lookup"><span data-stu-id="fd9bc-149">Token operations</span></span> 
<span data-ttu-id="fd9bc-150">Utilice estas operaciones para crear o actualizar un token que un cliente pueda utilizar para acceder a una conversación única.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-150">Use these operations to create or refresh a token that a client can use to access a single conversation.</span></span>

| <span data-ttu-id="fd9bc-151">Operación</span><span class="sxs-lookup"><span data-stu-id="fd9bc-151">Operation</span></span> | <span data-ttu-id="fd9bc-152">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="fd9bc-152">Description</span></span> |
|----|----|
| [<span data-ttu-id="fd9bc-153">Generar token</span><span class="sxs-lookup"><span data-stu-id="fd9bc-153">Generate Token</span></span>](#generate-token) | <span data-ttu-id="fd9bc-154">Genera un token para una conversación nueva.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-154">Generate a token for a new conversation.</span></span> | 
| [<span data-ttu-id="fd9bc-155">Actualizar token</span><span class="sxs-lookup"><span data-stu-id="fd9bc-155">Refresh Token</span></span>](#refresh-token) | <span data-ttu-id="fd9bc-156">Actualiza un token.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-156">Refresh a token.</span></span> | 

### <a name="generate-token"></a><span data-ttu-id="fd9bc-157">Generar token</span><span class="sxs-lookup"><span data-stu-id="fd9bc-157">Generate Token</span></span>
<span data-ttu-id="fd9bc-158">Genera un token válido para una conversación.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-158">Generates a token that is valid for one conversation.</span></span> 
```http 
POST /v3/directline/tokens/generate
```

| | |
|----|----|
| <span data-ttu-id="fd9bc-159">**Cuerpo de la solicitud**</span><span class="sxs-lookup"><span data-stu-id="fd9bc-159">**Request body**</span></span> | <span data-ttu-id="fd9bc-160">N/D</span><span class="sxs-lookup"><span data-stu-id="fd9bc-160">n/a</span></span> |
| <span data-ttu-id="fd9bc-161">**Devuelve**</span><span class="sxs-lookup"><span data-stu-id="fd9bc-161">**Returns**</span></span> | <span data-ttu-id="fd9bc-162">Un objeto [Conversation](#conversation-object)</span><span class="sxs-lookup"><span data-stu-id="fd9bc-162">A [Conversation](#conversation-object) object</span></span> | 

### <a name="refresh-token"></a><span data-ttu-id="fd9bc-163">Actualizar token</span><span class="sxs-lookup"><span data-stu-id="fd9bc-163">Refresh Token</span></span>
<span data-ttu-id="fd9bc-164">Actualiza el token.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-164">Refreshes the token.</span></span> 
```http 
POST /v3/directline/tokens/refresh
```

| | |
|----|----|
| <span data-ttu-id="fd9bc-165">**Cuerpo de la solicitud**</span><span class="sxs-lookup"><span data-stu-id="fd9bc-165">**Request body**</span></span> | <span data-ttu-id="fd9bc-166">N/D</span><span class="sxs-lookup"><span data-stu-id="fd9bc-166">n/a</span></span> |
| <span data-ttu-id="fd9bc-167">**Devuelve**</span><span class="sxs-lookup"><span data-stu-id="fd9bc-167">**Returns**</span></span> | <span data-ttu-id="fd9bc-168">Un objeto [Conversation](#conversation-object)</span><span class="sxs-lookup"><span data-stu-id="fd9bc-168">A [Conversation](#conversation-object) object</span></span> | 

## <a name="conversation-operations"></a><span data-ttu-id="fd9bc-169">Operaciones con conversaciones</span><span class="sxs-lookup"><span data-stu-id="fd9bc-169">Conversation operations</span></span> 
<span data-ttu-id="fd9bc-170">Use estas operaciones para abrir una conversación con sus bot e intercambie actividades entre el cliente y el bot.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-170">Use these operations to open a conversation with your bot and exchange activities between client and bot.</span></span>

| <span data-ttu-id="fd9bc-171">Operación</span><span class="sxs-lookup"><span data-stu-id="fd9bc-171">Operation</span></span> | <span data-ttu-id="fd9bc-172">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="fd9bc-172">Description</span></span> |
|----|----|
| [<span data-ttu-id="fd9bc-173">Iniciar conversación</span><span class="sxs-lookup"><span data-stu-id="fd9bc-173">Start Conversation</span></span>](#start-conversation) | <span data-ttu-id="fd9bc-174">Abre una nueva conversación con el bot.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-174">Opens a new conversation with the bot.</span></span> | 
| [<span data-ttu-id="fd9bc-175">Obtener información de la conversación</span><span class="sxs-lookup"><span data-stu-id="fd9bc-175">Get Conversation Information</span></span>](#get-conversation-information) | <span data-ttu-id="fd9bc-176">Le permite obtener información sobre una conversación existente.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-176">Gets information about an existing conversation.</span></span> <span data-ttu-id="fd9bc-177">Esta operación genera una nueva dirección URL de flujo de WebSocket que un cliente puede utilizar para [reconectarse](bot-framework-rest-direct-line-3-0-reconnect-to-conversation.md) a una conversación.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-177">This operation generates a new WebSocket stream URL that a client may use to [reconnect](bot-framework-rest-direct-line-3-0-reconnect-to-conversation.md) to a conversation.</span></span> |
| [<span data-ttu-id="fd9bc-178">Obtener actividades</span><span class="sxs-lookup"><span data-stu-id="fd9bc-178">Get Activities</span></span>](#get-activities) | <span data-ttu-id="fd9bc-179">Recupera actividades del bot.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-179">Retrieves activities from the bot.</span></span> |
| [<span data-ttu-id="fd9bc-180">Enviar una actividad</span><span class="sxs-lookup"><span data-stu-id="fd9bc-180">Send an Activity</span></span>](#send-an-activity) | <span data-ttu-id="fd9bc-181">Envía una actividad al bot.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-181">Sends an activity to the bot.</span></span> | 
| [<span data-ttu-id="fd9bc-182">Cargar y enviar archivos</span><span class="sxs-lookup"><span data-stu-id="fd9bc-182">Upload and Send File(s)</span></span>](#upload-send-files) | <span data-ttu-id="fd9bc-183">Carga y envía archivos como adjuntos.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-183">Uploads and sends file(s) as attachment(s).</span></span> |

### <a name="start-conversation"></a><span data-ttu-id="fd9bc-184">Iniciar conversación</span><span class="sxs-lookup"><span data-stu-id="fd9bc-184">Start Conversation</span></span>
<span data-ttu-id="fd9bc-185">Abre una nueva conversación con el bot.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-185">Opens a new conversation with the bot.</span></span> 
```http 
POST /v3/directline/conversations
```

| | |
|----|----|
| <span data-ttu-id="fd9bc-186">**Cuerpo de la solicitud**</span><span class="sxs-lookup"><span data-stu-id="fd9bc-186">**Request body**</span></span> | <span data-ttu-id="fd9bc-187">N/D</span><span class="sxs-lookup"><span data-stu-id="fd9bc-187">n/a</span></span> |
| <span data-ttu-id="fd9bc-188">**Devuelve**</span><span class="sxs-lookup"><span data-stu-id="fd9bc-188">**Returns**</span></span> | <span data-ttu-id="fd9bc-189">Un objeto [Conversation](#conversation-object)</span><span class="sxs-lookup"><span data-stu-id="fd9bc-189">A [Conversation](#conversation-object) object</span></span> | 

### <a name="get-conversation-information"></a><span data-ttu-id="fd9bc-190">Obtener información de la conversación</span><span class="sxs-lookup"><span data-stu-id="fd9bc-190">Get Conversation Information</span></span>
<span data-ttu-id="fd9bc-191">Obtiene información sobre una conversación existente y también genera una nueva dirección URL de flujo de WebSocket que un cliente puede utilizar para [reconectarse](bot-framework-rest-direct-line-3-0-reconnect-to-conversation.md) a una conversación.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-191">Gets information about an existing conversation and also generates a new WebSocket stream URL that a client may use to [reconnect](bot-framework-rest-direct-line-3-0-reconnect-to-conversation.md) to a conversation.</span></span> <span data-ttu-id="fd9bc-192">Como alternativa, se puede proporcionar el parámetro `watermark` en el URI de la solicitud para indicar el mensaje más reciente visto por el cliente.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-192">You may optionally supply the `watermark` parameter in the request URI to indicate the most recent message seen by the client.</span></span>
```http 
GET /v3/directline/conversations/{conversationId}?watermark={watermark_value}
```

| | |
|----|----|
| <span data-ttu-id="fd9bc-193">**Cuerpo de la solicitud**</span><span class="sxs-lookup"><span data-stu-id="fd9bc-193">**Request body**</span></span> | <span data-ttu-id="fd9bc-194">N/D</span><span class="sxs-lookup"><span data-stu-id="fd9bc-194">n/a</span></span> |
| <span data-ttu-id="fd9bc-195">**Devuelve**</span><span class="sxs-lookup"><span data-stu-id="fd9bc-195">**Returns**</span></span> | <span data-ttu-id="fd9bc-196">Un objeto [Conversation](#conversation-object)</span><span class="sxs-lookup"><span data-stu-id="fd9bc-196">A [Conversation](#conversation-object) object</span></span> | 

### <a name="get-activities"></a><span data-ttu-id="fd9bc-197">Obtener actividades</span><span class="sxs-lookup"><span data-stu-id="fd9bc-197">Get Activities</span></span>
<span data-ttu-id="fd9bc-198">Recupera las actividades del bot para la conversación especificada.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-198">Retrieves activities from the bot for the specified conversation.</span></span> <span data-ttu-id="fd9bc-199">Como alternativa, se puede proporcionar el parámetro `watermark` en el URI de la solicitud para indicar el mensaje más reciente visto por el cliente.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-199">You may optionally supply the `watermark` parameter in the request URI to indicate the most recent message seen by the client.</span></span> 

```http 
GET /v3/directline/conversations/{conversationId}/activities?watermark={watermark_value}
```

| | |
|----|----|
| <span data-ttu-id="fd9bc-200">**Cuerpo de la solicitud**</span><span class="sxs-lookup"><span data-stu-id="fd9bc-200">**Request body**</span></span> | <span data-ttu-id="fd9bc-201">N/D</span><span class="sxs-lookup"><span data-stu-id="fd9bc-201">n/a</span></span> |
| <span data-ttu-id="fd9bc-202">**Devuelve**</span><span class="sxs-lookup"><span data-stu-id="fd9bc-202">**Returns**</span></span> | <span data-ttu-id="fd9bc-203">Un objeto [ActivitySet](#activityset-object).</span><span class="sxs-lookup"><span data-stu-id="fd9bc-203">An [ActivitySet](#activityset-object) object.</span></span> <span data-ttu-id="fd9bc-204">La respuesta contiene `watermark` como propiedad del objeto `ActivitySet`.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-204">The response contains `watermark` as a property of the `ActivitySet` object.</span></span> <span data-ttu-id="fd9bc-205">Los clientes deben desplazarse por las actividades disponibles haciendo avanzar el valor `watermark` hasta que no se devuelvan actividades.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-205">Clients should page through the available activities by advancing the `watermark` value until no activities are returned.</span></span> | 

### <a name="send-an-activity"></a><span data-ttu-id="fd9bc-206">Enviar una actividad</span><span class="sxs-lookup"><span data-stu-id="fd9bc-206">Send an Activity</span></span>
<span data-ttu-id="fd9bc-207">Envía una actividad al bot.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-207">Sends an activity to the bot.</span></span> 
```http 
POST /v3/directline/conversations/{conversationId}/activities
```

| | |
|----|----|
| <span data-ttu-id="fd9bc-208">**Cuerpo de la solicitud**</span><span class="sxs-lookup"><span data-stu-id="fd9bc-208">**Request body**</span></span> | <span data-ttu-id="fd9bc-209">Un objeto [Activity](bot-framework-rest-connector-api-reference.md#activity-object)</span><span class="sxs-lookup"><span data-stu-id="fd9bc-209">An [Activity](bot-framework-rest-connector-api-reference.md#activity-object) object</span></span> |
| <span data-ttu-id="fd9bc-210">**Devuelve**</span><span class="sxs-lookup"><span data-stu-id="fd9bc-210">**Returns**</span></span> | <span data-ttu-id="fd9bc-211">Un objeto [ResourceResponse](bot-framework-rest-connector-api-reference.md#resourceresponse-object) que contiene una propiedad `id` que especifica el id. de la actividad que se envió al bot.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-211">A [ResourceResponse](bot-framework-rest-connector-api-reference.md#resourceresponse-object) that contains an `id` property which specifies the ID of the Activity that was sent to the bot.</span></span> | 

### <a id="upload-send-files"></a> <span data-ttu-id="fd9bc-212">Cargar y enviar archivos</span><span class="sxs-lookup"><span data-stu-id="fd9bc-212">Upload and Send File(s)</span></span>
<span data-ttu-id="fd9bc-213">Carga y envía archivos como adjuntos.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-213">Uploads and sends file(s) as attachment(s).</span></span> <span data-ttu-id="fd9bc-214">Establecer el parámetro `userId` en el URI de la solicitud para especificar el id. del usuario que envía los adjuntos.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-214">Set the `userId` parameter in the request URI to specify the ID of the user that is sending the attachment(s).</span></span>
```http 
POST /v3/directline/conversations/{conversationId}/upload?userId={userId}
```

| | |
|----|----|
| <span data-ttu-id="fd9bc-215">**Cuerpo de la solicitud**</span><span class="sxs-lookup"><span data-stu-id="fd9bc-215">**Request body**</span></span> | <span data-ttu-id="fd9bc-216">Para un único archivo adjunto, rellene el cuerpo de la solicitud con el contenido del archivo.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-216">For a single attachment, populate the request body with the file contents.</span></span> <span data-ttu-id="fd9bc-217">Para varios archivos adjuntos, cree un cuerpo de solicitud de varias partes que contenga una parte para cada archivo adjunto y, además (de manera opcional), una parte para el objeto [Activity](bot-framework-rest-connector-api-reference.md#activity-object) que debe actuar como contenedor para los archivos adjuntos especificados.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-217">For multiple attachments, create a multipart request body that contains one part for each attachment, and also (optionally) one part for the [Activity](bot-framework-rest-connector-api-reference.md#activity-object) object that should serve as the container for the specified attachment(s).</span></span> <span data-ttu-id="fd9bc-218">Para obtener más información, consulte [Envío de una actividad al bot](bot-framework-rest-direct-line-3-0-send-activity.md).</span><span class="sxs-lookup"><span data-stu-id="fd9bc-218">For more information, see [Send an activity to the bot](bot-framework-rest-direct-line-3-0-send-activity.md).</span></span> |
| <span data-ttu-id="fd9bc-219">**Devuelve**</span><span class="sxs-lookup"><span data-stu-id="fd9bc-219">**Returns**</span></span> | <span data-ttu-id="fd9bc-220">Un objeto [ResourceResponse](bot-framework-rest-connector-api-reference.md#resourceresponse-object) que contiene una propiedad `id` que especifica el id. de la actividad que se envió al bot.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-220">A [ResourceResponse](bot-framework-rest-connector-api-reference.md#resourceresponse-object) that contains an `id` property which specifies the ID of the Activity that was sent to the bot.</span></span> | 

> [!NOTE]
> <span data-ttu-id="fd9bc-221">Los archivos cargados se eliminan después de 24 horas.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-221">Uploaded files are deleted after 24 hours.</span></span>

## <a name="schema"></a><span data-ttu-id="fd9bc-222">Esquema</span><span class="sxs-lookup"><span data-stu-id="fd9bc-222">Schema</span></span>

<span data-ttu-id="fd9bc-223">El esquema de Direct Line 3.0 incluye todos los objetos definidos por el [esquema de Bot Framework v3](bot-framework-rest-connector-api-reference.md#objects), así como el objeto `ActivitySet` y el objeto `Conversation`.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-223">Direct Line 3.0 schema includes all of the objects that are defined by the [Bot Framework v3 schema](bot-framework-rest-connector-api-reference.md#objects) as well as the `ActivitySet` object and the `Conversation` object.</span></span>

### <a name="activityset-object"></a><span data-ttu-id="fd9bc-224">Objeto ActivitySet</span><span class="sxs-lookup"><span data-stu-id="fd9bc-224">ActivitySet object</span></span> 
<span data-ttu-id="fd9bc-225">Define un conjunto de actividades.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-225">Defines a set of activities.</span></span><br/><br/>

| <span data-ttu-id="fd9bc-226">Propiedad</span><span class="sxs-lookup"><span data-stu-id="fd9bc-226">Property</span></span> | <span data-ttu-id="fd9bc-227">Escriba</span><span class="sxs-lookup"><span data-stu-id="fd9bc-227">Type</span></span> | <span data-ttu-id="fd9bc-228">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="fd9bc-228">Description</span></span> |
|----|----|----|
| <span data-ttu-id="fd9bc-229">**activities**</span><span class="sxs-lookup"><span data-stu-id="fd9bc-229">**activities**</span></span> | <span data-ttu-id="fd9bc-230">[Actividad](bot-framework-rest-connector-api-reference.md#activity-object)[]</span><span class="sxs-lookup"><span data-stu-id="fd9bc-230">[Activity](bot-framework-rest-connector-api-reference.md#activity-object)[]</span></span> | <span data-ttu-id="fd9bc-231">Matriz de objetos **Activity**.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-231">Array of **Activity** objects.</span></span> |
| <span data-ttu-id="fd9bc-232">**watermark**</span><span class="sxs-lookup"><span data-stu-id="fd9bc-232">**watermark**</span></span> | <span data-ttu-id="fd9bc-233">string</span><span class="sxs-lookup"><span data-stu-id="fd9bc-233">string</span></span> | <span data-ttu-id="fd9bc-234">Marca de agua máxima de actividades dentro del conjunto.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-234">Maximum watermark of activities within the set.</span></span> <span data-ttu-id="fd9bc-235">Un cliente puede utilizar el valor `watermark` para indicar el mensaje más reciente que ha visto al [recuperar actividades del bot](bot-framework-rest-direct-line-3-0-receive-activities.md#http-get) o al [generar una nueva dirección URL del flujo de WebSocket](bot-framework-rest-direct-line-3-0-reconnect-to-conversation.md).</span><span class="sxs-lookup"><span data-stu-id="fd9bc-235">A client may use the `watermark` value to indicate the most recent message it has seen either when [retrieving activities from the bot](bot-framework-rest-direct-line-3-0-receive-activities.md#http-get) or when [generating a new WebSocket stream URL](bot-framework-rest-direct-line-3-0-reconnect-to-conversation.md).</span></span> |

### <a name="conversation-object"></a><span data-ttu-id="fd9bc-236">Objeto Conversation</span><span class="sxs-lookup"><span data-stu-id="fd9bc-236">Conversation object</span></span>
<span data-ttu-id="fd9bc-237">Define una conversación de Direct Line.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-237">Defines a Direct Line conversation.</span></span><br/><br/>

| <span data-ttu-id="fd9bc-238">Propiedad</span><span class="sxs-lookup"><span data-stu-id="fd9bc-238">Property</span></span> | <span data-ttu-id="fd9bc-239">Escriba</span><span class="sxs-lookup"><span data-stu-id="fd9bc-239">Type</span></span> | <span data-ttu-id="fd9bc-240">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="fd9bc-240">Description</span></span> |
|----|----|----|
| <span data-ttu-id="fd9bc-241">**conversationId**</span><span class="sxs-lookup"><span data-stu-id="fd9bc-241">**conversationId**</span></span> | <span data-ttu-id="fd9bc-242">string</span><span class="sxs-lookup"><span data-stu-id="fd9bc-242">string</span></span> | <span data-ttu-id="fd9bc-243">Id. que identifica de forma única la conversación para la cual el token especificado es válido.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-243">ID that uniquely identifies the conversation for which the specified token is valid.</span></span> |
| <span data-ttu-id="fd9bc-244">**expires_in**</span><span class="sxs-lookup"><span data-stu-id="fd9bc-244">**expires_in**</span></span> | <span data-ttu-id="fd9bc-245">número</span><span class="sxs-lookup"><span data-stu-id="fd9bc-245">number</span></span> | <span data-ttu-id="fd9bc-246">Número de segundos hasta que expira el token.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-246">Number of seconds until the token expires.</span></span> |
| <span data-ttu-id="fd9bc-247">**streamUrl**</span><span class="sxs-lookup"><span data-stu-id="fd9bc-247">**streamUrl**</span></span> | <span data-ttu-id="fd9bc-248">string</span><span class="sxs-lookup"><span data-stu-id="fd9bc-248">string</span></span> | <span data-ttu-id="fd9bc-249">Dirección URL del flujo de mensajes de la conversación.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-249">URL for the conversation's message stream.</span></span> |
| <span data-ttu-id="fd9bc-250">**token**</span><span class="sxs-lookup"><span data-stu-id="fd9bc-250">**token**</span></span> | <span data-ttu-id="fd9bc-251">string</span><span class="sxs-lookup"><span data-stu-id="fd9bc-251">string</span></span> | <span data-ttu-id="fd9bc-252">Token válido para la conversación especificada.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-252">Token that is valid for the specified conversation.</span></span> |

### <a name="activities"></a><span data-ttu-id="fd9bc-253">Actividades</span><span class="sxs-lookup"><span data-stu-id="fd9bc-253">Activities</span></span>

<span data-ttu-id="fd9bc-254">Para cada [Activity](bot-framework-rest-connector-api-reference.md#activity-object) que un cliente recibe de un bot a través de Direct Line:</span><span class="sxs-lookup"><span data-stu-id="fd9bc-254">For each [Activity](bot-framework-rest-connector-api-reference.md#activity-object) that a client receives from a bot via Direct Line:</span></span>

- <span data-ttu-id="fd9bc-255">Se conservan las tarjetas de archivos adjuntos.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-255">Attachment cards are preserved.</span></span>
- <span data-ttu-id="fd9bc-256">Las direcciones URL para los archivos adjuntos cargados se ocultan con un vínculo privado.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-256">URLs for uploaded attachments are hidden with a private link.</span></span>
- <span data-ttu-id="fd9bc-257">La propiedad `channelData` se conserva sin modificaciones.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-257">The `channelData` property is preserved without modification.</span></span> 

<span data-ttu-id="fd9bc-258">Es posible que los clientes [reciban](bot-framework-rest-direct-line-3-0-receive-activities.md) varias actividades del bot como parte de un objeto [ActivitySet](#activityset-object).</span><span class="sxs-lookup"><span data-stu-id="fd9bc-258">Clients may [receive](bot-framework-rest-direct-line-3-0-receive-activities.md) multiple activities from the bot as part of an [ActivitySet](#activityset-object).</span></span> 

<span data-ttu-id="fd9bc-259">Cuando un cliente envía un objeto [Activity](bot-framework-rest-connector-api-reference.md#activity-object) a un bot a través de Direct Line:</span><span class="sxs-lookup"><span data-stu-id="fd9bc-259">When a client sends an [Activity](bot-framework-rest-connector-api-reference.md#activity-object) to a bot via Direct Line:</span></span>

- <span data-ttu-id="fd9bc-260">La propiedad `type` especifica el tipo de actividad que envía (normalmente **mensaje**).</span><span class="sxs-lookup"><span data-stu-id="fd9bc-260">The `type` property specifies the type activity it is sending (typically **message**).</span></span>
- <span data-ttu-id="fd9bc-261">La propiedad `from` se debe rellenar con un id. de usuario elegido por el cliente.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-261">The `from` property must be populated with a user ID, chosen by the client.</span></span>
- <span data-ttu-id="fd9bc-262">Los archivos adjuntos pueden contener direcciones URL a los recursos existentes o direcciones URL cargadas a través del punto de conexión de los archivos adjuntos de Direct Line.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-262">Attachments may contain URLs to existing resources or URLs uploaded through the Direct Line attachment endpoint.</span></span>
- <span data-ttu-id="fd9bc-263">La propiedad `channelData` se conserva sin modificaciones.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-263">The `channelData` property is preserved without modification.</span></span>

<span data-ttu-id="fd9bc-264">Es posible que los clientes [envíen](bot-framework-rest-direct-line-3-0-send-activity.md) una única actividad por solicitud.</span><span class="sxs-lookup"><span data-stu-id="fd9bc-264">Clients may [send](bot-framework-rest-direct-line-3-0-send-activity.md) a single activity per request.</span></span> 
