---
title: Volver a conectarse a una conversación | Microsoft Docs
description: Obtenga información sobre cómo volver a conectarse a una conversación mediante Direct Line API v3.0.
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ms.openlocfilehash: 2c6b3a7e9f0fdc7d5227fc8112cb6f3e330a2bcc
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305025"
---
# <a name="reconnect-to-a-conversation"></a><span data-ttu-id="8e09b-103">Volver a conectarse a una conversación</span><span class="sxs-lookup"><span data-stu-id="8e09b-103">Reconnect to a conversation</span></span>

<span data-ttu-id="8e09b-104">Si un cliente usa la [interfaz WebSocket](bot-framework-rest-direct-line-3-0-receive-activities.md#connect-via-websocket) para recibir mensajes pero pierde su conexión, es posible que tenga que volver a conectarse.</span><span class="sxs-lookup"><span data-stu-id="8e09b-104">If a client is using the [WebSocket interface](bot-framework-rest-direct-line-3-0-receive-activities.md#connect-via-websocket) to receive messages but loses its connection, it may need to reconnect.</span></span> <span data-ttu-id="8e09b-105">En este caso, el cliente debe generar una nueva URL de secuencia de WebSocket que pueda usar para volver a conectarse a la conversación.</span><span class="sxs-lookup"><span data-stu-id="8e09b-105">In this scenario, the client must generate a new WebSocket stream URL that it can use to reconnect to the conversation.</span></span>

## <a name="generate-a-new-websocket-stream-url"></a><span data-ttu-id="8e09b-106">Generar una nueva dirección URL de secuencia de WebSocket</span><span class="sxs-lookup"><span data-stu-id="8e09b-106">Generate a new WebSocket stream URL</span></span>

<span data-ttu-id="8e09b-107">Para generar una nueva URL de secuencia de WebSocket que se pueda usar para volver a conectarse a una conversación existente, emita esta solicitud:</span><span class="sxs-lookup"><span data-stu-id="8e09b-107">To generate a new WebSocket stream URL that can be used to reconnect to an existing conversation, issue this request:</span></span> 

```http
GET https://directline.botframework.com/v3/directline/conversations/{conversationId}?watermark={watermark_value}
Authorization: Bearer SECRET_OR_TOKEN
```

<span data-ttu-id="8e09b-108">En este URI de solicitud, reemplace **{conversationId}** con el id. de conversación y reemplace **{watermark_value}** con el valor de la marca de agua (si el parámetro `watermark` se ha proporcionado).</span><span class="sxs-lookup"><span data-stu-id="8e09b-108">In this request URI, replace **{conversationId}** with the conversation ID and replace **{watermark_value}** with the watermark value (if the `watermark` parameter is supplied).</span></span> <span data-ttu-id="8e09b-109">El `watermark` es opcional.</span><span class="sxs-lookup"><span data-stu-id="8e09b-109">The `watermark` parameter is optional.</span></span> <span data-ttu-id="8e09b-110">Si el parámetro `watermark` se especifica en el URI de solicitud, la conversación se reproduce desde la marca de agua, lo que garantiza que ningún mensaje se pierda.</span><span class="sxs-lookup"><span data-stu-id="8e09b-110">If the `watermark` parameter is specified in the request URI, the conversation replays from the watermark, guaranteeing that no messages are lost.</span></span> <span data-ttu-id="8e09b-111">Si se omite el parámetro `watermark` en el URI de la solicitud, se reproducen únicamente los mensajes recibidos tras la solicitud de reconexión.</span><span class="sxs-lookup"><span data-stu-id="8e09b-111">If the `watermark` parameter is omitted from the request URI, only messages received after the reconnection request are replayed.</span></span>

<span data-ttu-id="8e09b-112">Los fragmentos de código siguientes proporcionan un ejemplo de la solicitud de reconexión y la respuesta.</span><span class="sxs-lookup"><span data-stu-id="8e09b-112">The following snippets provide an example of the Reconnect request and response.</span></span>

### <a name="request"></a><span data-ttu-id="8e09b-113">Solicitud</span><span class="sxs-lookup"><span data-stu-id="8e09b-113">Request</span></span>

```http
GET https://directline.botframework.com/v3/directline/conversations/abc123?watermark=0000a-42
Authorization: Bearer RCurR_XV9ZA.cwA.BKA.iaJrC8xpy8qbOF5xnR2vtCX7CZj0LdjAPGfiCpg4Fv0y8qbOF5xPGfiCpg4Fv0y8qqbOF5x8qbOF5xn
```

### <a name="response"></a><span data-ttu-id="8e09b-114">Response</span><span class="sxs-lookup"><span data-stu-id="8e09b-114">Response</span></span>

<span data-ttu-id="8e09b-115">Si la solicitud es correcta, la respuesta contendrá un identificador para la conversación, un token y una nueva dirección URL de secuencia de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="8e09b-115">If the request is successful, the response will contain an ID for the conversation, a token, and a new WebSocket stream URL.</span></span>

```http
HTTP/1.1 200 OK
[other headers]
```

```json
{
  "conversationId": "abc123",
  "token": "RCurR_XV9ZA.cwA.BKA.iaJrC8xpy8qbOF5xnR2vtCX7CZj0LdjAPGfiCpg4Fv0y8qbOF5xPGfiCpg4Fv0y8qqbOF5x8qbOF5xn",
  "streamUrl": "https://directline.botframework.com/v3/directline/conversations/abc123/stream?watermark=000a-4&amp;t=RCurR_XV9ZA.cwA..."
}
```

## <a name="reconnect-to-the-conversation"></a><span data-ttu-id="8e09b-116">Volver a conectarse a una conversación</span><span class="sxs-lookup"><span data-stu-id="8e09b-116">Reconnect to the conversation</span></span>

<span data-ttu-id="8e09b-117">El cliente debe usar la nueva dirección URL de secuencia de WebSocket para [volver a conectarse a la conversación](bot-framework-rest-direct-line-3-0-receive-activities.md#connect-via-websocket) en 60 segundos.</span><span class="sxs-lookup"><span data-stu-id="8e09b-117">The client must use the new WebSocket stream URL to [reconnect to the conversation](bot-framework-rest-direct-line-3-0-receive-activities.md#connect-via-websocket) within 60 seconds.</span></span> <span data-ttu-id="8e09b-118">Si no se puede establecer la conexión durante este tiempo, el cliente debe emitir otra solicitud de reconexión para generar una nueva dirección URL de secuencia.</span><span class="sxs-lookup"><span data-stu-id="8e09b-118">If the connection cannot be established during this time, the client must issue another Reconnect request to generate a new stream URL.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8e09b-119">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="8e09b-119">Additional resources</span></span>

- [<span data-ttu-id="8e09b-120">Conceptos clave</span><span class="sxs-lookup"><span data-stu-id="8e09b-120">Key concepts</span></span>](bot-framework-rest-direct-line-3-0-concepts.md)
- [<span data-ttu-id="8e09b-121">Autenticación</span><span class="sxs-lookup"><span data-stu-id="8e09b-121">Authentication</span></span>](bot-framework-rest-direct-line-3-0-authentication.md)
- [<span data-ttu-id="8e09b-122">Recibir las actividades a través de la secuencia de WebSocket</span><span class="sxs-lookup"><span data-stu-id="8e09b-122">Receive activities via WebSocket stream</span></span>](bot-framework-rest-direct-line-3-0-receive-activities.md#connect-via-websocket)