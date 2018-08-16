---
title: Recepción de mensajes del bot | Microsoft Docs
description: Aprenda a recibir mensajes del bot mediante Direct Line API v1.1.
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ms.openlocfilehash: d9f1821767e5bd26c9a8bfdf3927f257077f0e79
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305828"
---
# <a name="receive-messages-from-the-bot"></a><span data-ttu-id="700b4-103">Recepción de mensajes del bot</span><span class="sxs-lookup"><span data-stu-id="700b4-103">Receive messages from the bot</span></span>

> [!IMPORTANT]
> <span data-ttu-id="700b4-104">En este artículo se describe cómo recibir mensajes del bot mediante Direct Line API v1.1.</span><span class="sxs-lookup"><span data-stu-id="700b4-104">This article describes how to receive messages from the bot using Direct Line API 1.1.</span></span> <span data-ttu-id="700b4-105">Si va a crear una nueva conexión entre la aplicación cliente y el bot, use [Direct Line API 3.0](bot-framework-rest-direct-line-3-0-receive-activities.md) en su lugar.</span><span class="sxs-lookup"><span data-stu-id="700b4-105">If you are creating a new connection between your client application and bot, use [Direct Line API 3.0](bot-framework-rest-direct-line-3-0-receive-activities.md) instead.</span></span>

<span data-ttu-id="700b4-106">Mediante el protocolo Direct Line 1.1, los clientes deben sondear una interfaz `HTTP GET` para recibir mensajes.</span><span class="sxs-lookup"><span data-stu-id="700b4-106">Using the Direct Line 1.1 protocol, clients must poll an `HTTP GET` interface to receive messages.</span></span> 

## <a name="retrieve-messages-with-http-get"></a><span data-ttu-id="700b4-107">Recuperación de mensajes con HTTP GET</span><span class="sxs-lookup"><span data-stu-id="700b4-107">Retrieve messages with HTTP GET</span></span>

<span data-ttu-id="700b4-108">Para recuperar mensajes de una conversación específica, emita una solicitud `GET` al punto de conexión `api/conversations/{conversationId}/messages` y, si lo desea, especifique el parámetro `watermark` para indicar el mensaje más reciente visto por el cliente.</span><span class="sxs-lookup"><span data-stu-id="700b4-108">To retrieve messages for a specific conversation, issue a `GET` request to the `api/conversations/{conversationId}/messages` endpoint, optionally specifying the `watermark` parameter to indicate the most recent message seen by the client.</span></span> <span data-ttu-id="700b4-109">Se devolverá un valor `watermark` en la respuesta JSON, incluso si no se incluyen mensajes.</span><span class="sxs-lookup"><span data-stu-id="700b4-109">An updated `watermark` value will be returned in the JSON response, even if no messages are included.</span></span>

<span data-ttu-id="700b4-110">Los fragmentos de código siguientes proporcionan un ejemplo de la solicitud Get Messages y la respuesta.</span><span class="sxs-lookup"><span data-stu-id="700b4-110">The following snippets provide an example of the Get Messages request and response.</span></span> <span data-ttu-id="700b4-111">Contiene la respuesta de Get Messages `watermark` como una propiedad de [MessageSet](bot-framework-rest-direct-line-1-1-api-reference.md#messageset-object).</span><span class="sxs-lookup"><span data-stu-id="700b4-111">The Get Messages response contains `watermark` as a property of the [MessageSet](bot-framework-rest-direct-line-1-1-api-reference.md#messageset-object).</span></span> <span data-ttu-id="700b4-112">Los clientes deben desplazarse por los mensajes disponibles haciendo avanzar el valor `watermark` hasta que no se devuelvan mensajes.</span><span class="sxs-lookup"><span data-stu-id="700b4-112">Clients should page through the available messages by advancing the `watermark` value until no messages are returned.</span></span> 

### <a name="request"></a><span data-ttu-id="700b4-113">Solicitud</span><span class="sxs-lookup"><span data-stu-id="700b4-113">Request</span></span>

```http
GET https://directline.botframework.com/api/conversations/abc123/messages?watermark=0001a-94
Authorization: Bearer RCurR_XV9ZA.cwA.BKA.iaJrC8xpy8qbOF5xnR2vtCX7CZj0LdjAPGfiCpg4Fv0
```

### <a name="response"></a><span data-ttu-id="700b4-114">Response</span><span class="sxs-lookup"><span data-stu-id="700b4-114">Response</span></span>

```http
HTTP/1.1 200 OK
[other headers]
```

```json
{
    "messages": [
        {
            "conversation": "abc123",
            "id": "abc123|0000",
            "text": "hello",
            "from": "user1"
        }, 
        {
            "conversation": "abc123",
            "id": "abc123|0001",
            "text": "Nice to see you, user1!",
            "from": "bot1"
        }
    ],
    "watermark": "0001a-95"
}
```

## <a name="timing-considerations"></a><span data-ttu-id="700b4-115">Consideraciones de tiempo</span><span class="sxs-lookup"><span data-stu-id="700b4-115">Timing considerations</span></span>

<span data-ttu-id="700b4-116">Aunque Direct Line es un protocolo de varias partes con posibles diferencias temporales, el protocolo y el servicio están diseñados para que sea fácil crear un cliente confiable.</span><span class="sxs-lookup"><span data-stu-id="700b4-116">Even though Direct Line is a multi-part protocol with potential timing gaps, the protocol and service is designed to make it easy to build a reliable client.</span></span> <span data-ttu-id="700b4-117">La propiedad `watermark` que se envía en la respuesta de Get Messages es confiable.</span><span class="sxs-lookup"><span data-stu-id="700b4-117">The `watermark` property that is sent in the Get Messages response is reliable.</span></span> <span data-ttu-id="700b4-118">Un cliente no perderá ningún mensaje siempre que se reproduzca la marca de agua textualmente.</span><span class="sxs-lookup"><span data-stu-id="700b4-118">A client will not miss any messages as long as it replays the watermark verbatim.</span></span>

<span data-ttu-id="700b4-119">Los clientes deben elegir un intervalo de sondeo que coincida con su uso previsto.</span><span class="sxs-lookup"><span data-stu-id="700b4-119">Clients should choose a polling interval that matches their intended use.</span></span>

- <span data-ttu-id="700b4-120">Las aplicaciones de servicio a servicio suelen usar un intervalo de sondeo de 5 o 10 s.</span><span class="sxs-lookup"><span data-stu-id="700b4-120">Service-to-service applications often use a polling interval of 5s or 10s.</span></span>

- <span data-ttu-id="700b4-121">Las aplicaciones orientadas al cliente a menudo usan un intervalo de sondeo de 1 segundo y emiten una solicitud adicional ~ 300 ms después de todos los mensajes que envía el cliente (para recuperar rápidamente la respuesta de un bot).</span><span class="sxs-lookup"><span data-stu-id="700b4-121">Client-facing applications often use a polling interval of 1s, and issue an additional request ~300ms after every message that the client sends (to rapidly retrieve a bot's response).</span></span> <span data-ttu-id="700b4-122">Este retraso de 300 ms se debe ajustar en función del tiempo de tránsito y la velocidad del bot.</span><span class="sxs-lookup"><span data-stu-id="700b4-122">This 300ms delay should be adjusted based on the bot's speed and transit time.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="700b4-123">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="700b4-123">Additional resources</span></span>

- [<span data-ttu-id="700b4-124">Conceptos clave</span><span class="sxs-lookup"><span data-stu-id="700b4-124">Key concepts</span></span>](bot-framework-rest-direct-line-1-1-concepts.md)
- [<span data-ttu-id="700b4-125">Autenticación</span><span class="sxs-lookup"><span data-stu-id="700b4-125">Authentication</span></span>](bot-framework-rest-direct-line-1-1-authentication.md)
- [<span data-ttu-id="700b4-126">Inicio de una conversación</span><span class="sxs-lookup"><span data-stu-id="700b4-126">Start a conversation</span></span>](bot-framework-rest-direct-line-1-1-start-conversation.md)
- [<span data-ttu-id="700b4-127">Envío de un mensaje al bot</span><span class="sxs-lookup"><span data-stu-id="700b4-127">Send a message to the bot</span></span>](bot-framework-rest-direct-line-1-1-send-message.md)