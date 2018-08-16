---
title: Recepción de actividades del bot | Microsoft Docs
description: Aprenda a recibir actividades del bot mediante Direct Line API v3.0.
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ms.openlocfilehash: 2993b75a26ed987a472c241133a62727e3b285d2
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305909"
---
# <a name="receive-activities-from-the-bot"></a><span data-ttu-id="515cc-103">Recepción de actividades del bot</span><span class="sxs-lookup"><span data-stu-id="515cc-103">Receive activities from the bot</span></span>

<span data-ttu-id="515cc-104">Con el protocolo Direct Line 3.0, los clientes pueden recibir actividades mediante una secuencia de `WebSocket` o recuperar actividades mediante la emisión de solicitudes `HTTP GET`.</span><span class="sxs-lookup"><span data-stu-id="515cc-104">Using the Direct Line 3.0 protocol, clients can receive activities via `WebSocket` stream or retrieve activities by issuing `HTTP GET` requests.</span></span> 

## <a name="websocket-vs-http-get"></a><span data-ttu-id="515cc-105">WebSocket frente a HTTP GET</span><span class="sxs-lookup"><span data-stu-id="515cc-105">WebSocket vs HTTP GET</span></span>

<span data-ttu-id="515cc-106">Un protocolo WebSocket de streaming inserta mensajes en los clientes, siempre y cuando la interfaz GET permita que los clientes soliciten mensajes explícitamente.</span><span class="sxs-lookup"><span data-stu-id="515cc-106">A streaming WebSocket efficiently pushes messages to clients, whereas the GET interface enables clients to explicitly request messages.</span></span> <span data-ttu-id="515cc-107">Aunque con frecuencia se prefiere el mecanismo WebSocket dada su eficacia, el mecanismo GET puede ser útil para los clientes que no pueden usar WebSockets o para los clientes que quieren recuperar el historial de conversaciones.</span><span class="sxs-lookup"><span data-stu-id="515cc-107">Although the WebSocket mechanism is often preferred due to its efficiency, the GET mechanism can be useful for clients that are unable to use WebSockets or for clients that want to retrieve conversation history.</span></span> 

<span data-ttu-id="515cc-108">No todos los [tipos de actividad](bot-framework-rest-connector-activities.md) están disponibles mediante WebSocket y HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="515cc-108">Not all [activity types](bot-framework-rest-connector-activities.md) are available both via WebSocket and via HTTP GET.</span></span> <span data-ttu-id="515cc-109">En la tabla siguiente se describe la disponibilidad de los distintos tipos de actividad para los clientes que usan el protocolo Direct Line.</span><span class="sxs-lookup"><span data-stu-id="515cc-109">The following table describes the availability of the various activity types for clients that use the Direct Line protocol.</span></span>

| <span data-ttu-id="515cc-110">Tipo de actividad</span><span class="sxs-lookup"><span data-stu-id="515cc-110">Activity type</span></span> | <span data-ttu-id="515cc-111">Disponibilidad</span><span class="sxs-lookup"><span data-stu-id="515cc-111">Availability</span></span> | 
|----|----|
| <span data-ttu-id="515cc-112">Mensaje</span><span class="sxs-lookup"><span data-stu-id="515cc-112">message</span></span> | <span data-ttu-id="515cc-113">HTTP GET y WebSocket</span><span class="sxs-lookup"><span data-stu-id="515cc-113">HTTP GET and WebSocket</span></span> |
| <span data-ttu-id="515cc-114">typing</span><span class="sxs-lookup"><span data-stu-id="515cc-114">typing</span></span> | <span data-ttu-id="515cc-115">Solo WebSocket</span><span class="sxs-lookup"><span data-stu-id="515cc-115">WebSocket only</span></span> |
| <span data-ttu-id="515cc-116">conversationUpdate</span><span class="sxs-lookup"><span data-stu-id="515cc-116">conversationUpdate</span></span> | <span data-ttu-id="515cc-117">No enviados o recibidos mediante el cliente</span><span class="sxs-lookup"><span data-stu-id="515cc-117">Not sent/received via client</span></span> |
| <span data-ttu-id="515cc-118">contactRelationUpdate</span><span class="sxs-lookup"><span data-stu-id="515cc-118">contactRelationUpdate</span></span> | <span data-ttu-id="515cc-119">No se admite en Direct Line</span><span class="sxs-lookup"><span data-stu-id="515cc-119">Not supported in Direct Line</span></span> |
| <span data-ttu-id="515cc-120">endOfConversation</span><span class="sxs-lookup"><span data-stu-id="515cc-120">endOfConversation</span></span> | <span data-ttu-id="515cc-121">HTTP GET y WebSocket</span><span class="sxs-lookup"><span data-stu-id="515cc-121">HTTP GET and WebSocket</span></span> |
| <span data-ttu-id="515cc-122">todos los demás tipos de actividad</span><span class="sxs-lookup"><span data-stu-id="515cc-122">all other activity types</span></span> | <span data-ttu-id="515cc-123">HTTP GET y WebSocket</span><span class="sxs-lookup"><span data-stu-id="515cc-123">HTTP GET and WebSocket</span></span> |

## <a id="connect-via-websocket"></a> <span data-ttu-id="515cc-124">Recepción de actividades mediante una secuencia de WebSocket</span><span class="sxs-lookup"><span data-stu-id="515cc-124">Receive activities via WebSocket stream</span></span>

<span data-ttu-id="515cc-125">Cuando un cliente envía una solicitud [Iniciar conversación](bot-framework-rest-direct-line-3-0-start-conversation.md) para abrir una conversación con un bot, la respuesta del servicio incluye una propiedad `streamUrl` que el cliente puede usar posteriormente para conectarse por medio de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="515cc-125">When a client sends a [Start Conversation](bot-framework-rest-direct-line-3-0-start-conversation.md) request to open a conversation with a bot, the service's response includes a `streamUrl` property that the client can subsequently use to connect via WebSocket.</span></span> <span data-ttu-id="515cc-126">La dirección URL de la secuencia se autoriza previamente y, por tanto, la solicitud del cliente para conectarse mediante WebSocket NO requiere un encabezado `Authorization`.</span><span class="sxs-lookup"><span data-stu-id="515cc-126">The stream URL is preauthorized and therefore the client's request to connect via WebSocket does NOT require an `Authorization` header.</span></span>

<span data-ttu-id="515cc-127">En el ejemplo siguiente se muestra una solicitud que usa `streamUrl` para conectarse por medio de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="515cc-127">The following example shows a request that uses a `streamUrl` to connect via WebSocket.</span></span>

```http
-- connect to wss://directline.botframework.com --
GET /v3/directline/conversations/abc123/stream?t=RCurR_XV9ZA.cwA..."
Upgrade: websocket
Connection: upgrade
[other headers]
```

<span data-ttu-id="515cc-128">El servicio responde con el código de estado HTTP 101 ("Cambiando protocolos").</span><span class="sxs-lookup"><span data-stu-id="515cc-128">The service responds with status code HTTP 101 ("Switching Protocols").</span></span>

```http
HTTP/1.1 101 Switching Protocols
[other headers]
```

### <a name="receive-messages"></a><span data-ttu-id="515cc-129">Recepción de mensajes</span><span class="sxs-lookup"><span data-stu-id="515cc-129">Receive messages</span></span>

<span data-ttu-id="515cc-130">Después de conectarse mediante WebSocket, un cliente puede recibir estos tipos de mensajes del servicio Direct Line:</span><span class="sxs-lookup"><span data-stu-id="515cc-130">After it connects via WebSocket, a client may receive these types of messages from the Direct Line service:</span></span>

- <span data-ttu-id="515cc-131">Un mensaje que contiene un elemento [ActivitySet](bot-framework-rest-direct-line-3-0-api-reference.md#activityset-object) que incluye una o varias actividades y una marca de agua (se describe a continuación).</span><span class="sxs-lookup"><span data-stu-id="515cc-131">A message that contains an [ActivitySet](bot-framework-rest-direct-line-3-0-api-reference.md#activityset-object) that includes one or more activities and a watermark (described below).</span></span>
- <span data-ttu-id="515cc-132">Un mensaje vacío, que usa el servicio Direct Line para garantizar que la conexión sigue siendo válida.</span><span class="sxs-lookup"><span data-stu-id="515cc-132">An empty message, which the Direct Line service uses to ensure the connection is still valid.</span></span>
- <span data-ttu-id="515cc-133">Tipos adicionales, que se definirán más adelante.</span><span class="sxs-lookup"><span data-stu-id="515cc-133">Additional types, to be defined later.</span></span> <span data-ttu-id="515cc-134">Estos tipos se identifican mediante las propiedades de la raíz JSON.</span><span class="sxs-lookup"><span data-stu-id="515cc-134">These types are identified by the properties in the JSON root.</span></span>

<span data-ttu-id="515cc-135">Un elemento `ActivitySet` contiene los mensajes que envían el bot y todos los usuarios de la conversación.</span><span class="sxs-lookup"><span data-stu-id="515cc-135">An `ActivitySet` contains messages sent by the bot and by all users in the conversation.</span></span> <span data-ttu-id="515cc-136">En el ejemplo siguiente se muestra un elemento `ActivitySet` que contiene un único mensaje.</span><span class="sxs-lookup"><span data-stu-id="515cc-136">The following example shows an `ActivitySet` that contains a single message.</span></span>

```json
{
    "activities": [
        {
            "type": "message",
            "channelId": "directline",
            "conversation": {
                "id": "abc123"
            },
            "id": "abc123|0000",
            "from": {
                "id": "user1"
            },
            "text": "hello"
        }
    ],
    "watermark": "0000a-42"
}
```

### <a name="process-messages"></a><span data-ttu-id="515cc-137">Procesamiento de mensajes</span><span class="sxs-lookup"><span data-stu-id="515cc-137">Process messages</span></span>

<span data-ttu-id="515cc-138">Un cliente debe realizar un seguimiento del valor `watermark` que recibe en cada [ActivitySet](bot-framework-rest-direct-line-3-0-api-reference.md#activityset-object), de modo que pueda usar la marca de agua para garantizar que no se pierde ningún mensaje en caso de que se pierda la conexión y sea necesario [volver a conectarse a la conversación](bot-framework-rest-direct-line-3-0-reconnect-to-conversation.md).</span><span class="sxs-lookup"><span data-stu-id="515cc-138">A client should keep track of the `watermark` value that it receives in each [ActivitySet](bot-framework-rest-direct-line-3-0-api-reference.md#activityset-object), so that it may use the watermark to guarantee that no messages are lost if it loses its connection and needs to [reconnect to the conversation](bot-framework-rest-direct-line-3-0-reconnect-to-conversation.md).</span></span> <span data-ttu-id="515cc-139">Si el cliente recibe un elemento `ActivitySet` donde la propiedad `watermark` falta o es `null`, debe omitir ese valor y no sobrescribir la marca de agua anterior que recibió.</span><span class="sxs-lookup"><span data-stu-id="515cc-139">If the client receives an `ActivitySet` wherein the `watermark` property is `null` or missing, it should ignore that value and not overwrite the prior watermark that it received.</span></span>

<span data-ttu-id="515cc-140">Un cliente debe omitir los mensajes vacíos que recibe del servicio Direct Line.</span><span class="sxs-lookup"><span data-stu-id="515cc-140">A client should ignore empty messages that it receives from the Direct Line service.</span></span>

<span data-ttu-id="515cc-141">Un cliente puede enviar mensajes vacíos al servicio Direct Line para comprobar la conectividad.</span><span class="sxs-lookup"><span data-stu-id="515cc-141">A client may send empty messages to the Direct Line service to verify connectivity.</span></span> <span data-ttu-id="515cc-142">El servicio Direct Line pasará por alto los mensajes vacíos que recibe del cliente.</span><span class="sxs-lookup"><span data-stu-id="515cc-142">The Direct Line service will ignore empty messages that it receives from the client.</span></span>

<span data-ttu-id="515cc-143">El servicio Direct Line puede forzar el cierre de la conexión de WebSocket en determinadas condiciones.</span><span class="sxs-lookup"><span data-stu-id="515cc-143">The Direct Line service may forcibly close the WebSocket connection under certain conditions.</span></span> <span data-ttu-id="515cc-144">Si el cliente no ha recibido una actividad `endOfConversation`, es posible que [genere una nueva dirección URL de secuencia de WebSocket](bot-framework-rest-direct-line-3-0-reconnect-to-conversation.md) que puede usar para volver a conectarse a la conversación.</span><span class="sxs-lookup"><span data-stu-id="515cc-144">If the client has not received an `endOfConversation` activity, it may [generate a new WebSocket stream URL](bot-framework-rest-direct-line-3-0-reconnect-to-conversation.md) that it can use to reconnect to the conversation.</span></span> 

<span data-ttu-id="515cc-145">La secuencia de WebSocket contiene actualizaciones directas y mensajes muy recientes (desde que se emitió la llamada para conectarse mediante WebSocket) pero no incluye los mensajes enviados antes de la solicitud `POST` más reciente a `/v3/directline/conversations/{id}`.</span><span class="sxs-lookup"><span data-stu-id="515cc-145">The WebSocket stream contains live updates and very recent messages (since the call to connect via WebSocket was issued) but it does not include messages that were sent prior to the most recent `POST` to `/v3/directline/conversations/{id}`.</span></span> <span data-ttu-id="515cc-146">Para recuperar los mensajes enviados anteriormente en la conversación, use `HTTP GET` tal como se describe a continuación.</span><span class="sxs-lookup"><span data-stu-id="515cc-146">To retrieve messages that were sent earlier in the conversation, use `HTTP GET` as described below.</span></span>

## <a id="http-get"></a> <span data-ttu-id="515cc-147">Recuperación de actividades con HTTP GET</span><span class="sxs-lookup"><span data-stu-id="515cc-147">Retrieve activities with HTTP GET</span></span>

<span data-ttu-id="515cc-148">Los clientes que no pueden usar WebSockets o los clientes que desean obtener el historial de conversaciones pueden recuperar las actividades mediante `HTTP GET`.</span><span class="sxs-lookup"><span data-stu-id="515cc-148">Clients that are unable to use WebSockets or clients that want to get conversation history can retrieve activities by using `HTTP GET`.</span></span>

<span data-ttu-id="515cc-149">Para recuperar mensajes de una conversación específica, emita una solicitud `GET` al punto de conexión `/v3/directline/conversations/{conversationId}/activities` y, si lo desea, especifique el parámetro `watermark` para indicar el mensaje más reciente visto por el cliente.</span><span class="sxs-lookup"><span data-stu-id="515cc-149">To retrieve messages for a specific conversation, issue a `GET` request to the `/v3/directline/conversations/{conversationId}/activities` endpoint, optionally specifying the `watermark` parameter to indicate the most recent message seen by the client.</span></span> 

<span data-ttu-id="515cc-150">Los fragmentos de código siguientes proporcionan un ejemplo de la solicitud y respuesta de Obtener actividades de conversación.</span><span class="sxs-lookup"><span data-stu-id="515cc-150">The following snippets provide an example of the Get Conversation Activities request and response.</span></span> <span data-ttu-id="515cc-151">La respuesta Obtener actividades de conversación contiene `watermark` como propiedad de [ActivitySet](bot-framework-rest-direct-line-3-0-api-reference.md#activityset-object).</span><span class="sxs-lookup"><span data-stu-id="515cc-151">The Get Conversation Activities response contains `watermark` as a property of the [ActivitySet](bot-framework-rest-direct-line-3-0-api-reference.md#activityset-object).</span></span> <span data-ttu-id="515cc-152">Los clientes deben desplazarse por las actividades disponibles haciendo avanzar el valor `watermark` hasta que no se devuelvan actividades.</span><span class="sxs-lookup"><span data-stu-id="515cc-152">Clients should page through the available activities by advancing the `watermark` value until no activities are returned.</span></span>

### <a name="request"></a><span data-ttu-id="515cc-153">Solicitud</span><span class="sxs-lookup"><span data-stu-id="515cc-153">Request</span></span>

```http
GET https://directline.botframework.com/v3/directline/conversations/abc123/activities?watermark=0001a-94
Authorization: Bearer RCurR_XV9ZA.cwA.BKA.iaJrC8xpy8qbOF5xnR2vtCX7CZj0LdjAPGfiCpg4Fv0
```

### <a name="response"></a><span data-ttu-id="515cc-154">Response</span><span class="sxs-lookup"><span data-stu-id="515cc-154">Response</span></span>

```http
HTTP/1.1 200 OK
[other headers]
```

```json
{
    "activities": [
        {
            "type": "message",
            "channelId": "directline",
            "conversation": {
                "id": "abc123"
            },
            "id": "abc123|0000",
            "from": {
                "id": "user1"
            },
            "text": "hello"
        }, 
        {
            "type": "message",
            "channelId": "directline",
            "conversation": {
                "id": "abc123"
            },
            "id": "abc123|0001",
            "from": {
                "id": "bot1"
            },
            "text": "Nice to see you, user1!"
        }
    ],
    "watermark": "0001a-95"
}
```

## <a name="timing-considerations"></a><span data-ttu-id="515cc-155">Consideraciones de tiempo</span><span class="sxs-lookup"><span data-stu-id="515cc-155">Timing considerations</span></span>

<span data-ttu-id="515cc-156">La mayoría de los clientes desean conservar un historial completo de mensajes.</span><span class="sxs-lookup"><span data-stu-id="515cc-156">Most clients wish to retain a complete message history.</span></span> <span data-ttu-id="515cc-157">Aunque Direct Line es un protocolo de varias partes con posibles diferencias temporales, el protocolo y el servicio están diseñados para que sea fácil crear un cliente confiable.</span><span class="sxs-lookup"><span data-stu-id="515cc-157">Even though Direct Line is a multi-part protocol with potential timing gaps, the protocol and service is designed to make it easy to build a reliable client.</span></span>

- <span data-ttu-id="515cc-158">La propiedad `watermark` que se envía en la secuencia de WebSocket y en la respuesta de Obtener actividades de conversación es confiable.</span><span class="sxs-lookup"><span data-stu-id="515cc-158">The `watermark` property that is sent in the WebSocket stream and Get Conversation Activities response is reliable.</span></span> <span data-ttu-id="515cc-159">Un cliente no perderá ningún mensaje siempre que se reproduzca la marca de agua textualmente.</span><span class="sxs-lookup"><span data-stu-id="515cc-159">A client will not miss any messages as long as it replays the watermark verbatim.</span></span>

- <span data-ttu-id="515cc-160">Cuando un cliente inicia una conversación y se conecta a la secuencia de WebSocket, las actividades que se envían después de POST, pero antes de que se abra el socket, se reproducen antes que las nuevas actividades.</span><span class="sxs-lookup"><span data-stu-id="515cc-160">When a client starts a conversation and connects to the WebSocket stream, any activities that are sent after the POST but before the socket is opened are replayed before new activities.</span></span>

- <span data-ttu-id="515cc-161">Cuando un cliente emite una solicitud Obtener actividades de conversación (para actualizar el historial) mientras se conecta a la secuencia de WebSocket, las actividades pueden estar duplicadas en ambos canales.</span><span class="sxs-lookup"><span data-stu-id="515cc-161">When a client issues a Get Conversation Activities request (to refresh history) while it is connected to the WebSocket stream, activities may be duplicated across both channels.</span></span> <span data-ttu-id="515cc-162">Los clientes deben mantener el seguimiento de todos los identificadores de actividad conocidos para que puedan rechazar las actividades duplicadas, en caso de producirse.</span><span class="sxs-lookup"><span data-stu-id="515cc-162">Clients should keep track of all known activity IDs so that they are able to reject duplicate activities, should they occur.</span></span>

<span data-ttu-id="515cc-163">Los clientes que sondean con `HTTP GET` deben elegir un intervalo de sondeo que coincida con su uso previsto.</span><span class="sxs-lookup"><span data-stu-id="515cc-163">Clients that poll using `HTTP GET` should choose a polling interval that matches their intended use.</span></span>

- <span data-ttu-id="515cc-164">Las aplicaciones de servicio a servicio suelen usar un intervalo de sondeo de 5 o 10 s.</span><span class="sxs-lookup"><span data-stu-id="515cc-164">Service-to-service applications often use a polling interval of 5s or 10s.</span></span>

- <span data-ttu-id="515cc-165">Las aplicaciones orientadas al cliente a menudo usan un intervalo de sondeo de 1 segundo y emiten una solicitud adicional ~ 300 ms después de todos los mensajes que envía el cliente (para recuperar rápidamente la respuesta de un bot).</span><span class="sxs-lookup"><span data-stu-id="515cc-165">Client-facing applications often use a polling interval of 1s, and issue an additional request ~300ms after every message that the client sends (to rapidly retrieve a bot's response).</span></span> <span data-ttu-id="515cc-166">Este retraso de 300 ms se debe ajustar en función del tiempo de tránsito y la velocidad del bot.</span><span class="sxs-lookup"><span data-stu-id="515cc-166">This 300ms delay should be adjusted based on the bot's speed and transit time.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="515cc-167">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="515cc-167">Additional resources</span></span>

- [<span data-ttu-id="515cc-168">Conceptos clave</span><span class="sxs-lookup"><span data-stu-id="515cc-168">Key concepts</span></span>](bot-framework-rest-direct-line-3-0-concepts.md)
- [<span data-ttu-id="515cc-169">Autenticación</span><span class="sxs-lookup"><span data-stu-id="515cc-169">Authentication</span></span>](bot-framework-rest-direct-line-3-0-authentication.md)
- [<span data-ttu-id="515cc-170">Inicio de una conversación</span><span class="sxs-lookup"><span data-stu-id="515cc-170">Start a conversation</span></span>](bot-framework-rest-direct-line-3-0-start-conversation.md)
- [<span data-ttu-id="515cc-171">Volver a conectarse a una conversación</span><span class="sxs-lookup"><span data-stu-id="515cc-171">Reconnect to a conversation</span></span>](bot-framework-rest-direct-line-3-0-reconnect-to-conversation.md)
- [<span data-ttu-id="515cc-172">Envío de una actividad al bot</span><span class="sxs-lookup"><span data-stu-id="515cc-172">Send an activity to the bot</span></span>](bot-framework-rest-direct-line-3-0-send-activity.md)
- [<span data-ttu-id="515cc-173">Finalización de una conversación</span><span class="sxs-lookup"><span data-stu-id="515cc-173">End a conversation</span></span>](bot-framework-rest-direct-line-3-0-end-conversation.md)