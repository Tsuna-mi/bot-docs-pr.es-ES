---
title: Inicio de una conversación | Microsoft Docs
description: Aprenda a iniciar una conversación mediante Direct Line API v3.0.
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ms.openlocfilehash: ead16c0ff41ae93daff8952ca135fa0771bbbe78
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305256"
---
# <a name="start-a-conversation"></a><span data-ttu-id="0563f-103">Inicio de una conversación</span><span class="sxs-lookup"><span data-stu-id="0563f-103">Start a conversation</span></span>

<span data-ttu-id="0563f-104">Los clientes pueden abrir las conversaciones de Direct Line explícitamente y estas se pueden ejecutar siempre y cuando el cliente y el bot participen y tengan credenciales válidas.</span><span class="sxs-lookup"><span data-stu-id="0563f-104">Direct Line conversations are explicitly opened by clients and may run as long as the bot and client participate and have valid credentials.</span></span> <span data-ttu-id="0563f-105">Mientras la conversación está abierta, el bot y el cliente pueden enviar mensajes.</span><span class="sxs-lookup"><span data-stu-id="0563f-105">While the conversation is open, both the bot and client may send messages.</span></span> <span data-ttu-id="0563f-106">Más de un cliente puede conectarse a una conversación determinada y cada cliente puede participar en nombre de varios usuarios.</span><span class="sxs-lookup"><span data-stu-id="0563f-106">More than one client may connect to a given conversation and each client may participate on behalf of multiple users.</span></span>

## <a name="open-a-new-conversation"></a><span data-ttu-id="0563f-107">Apertura de una nueva conversación</span><span class="sxs-lookup"><span data-stu-id="0563f-107">Open a new conversation</span></span>

<span data-ttu-id="0563f-108">Para abrir una nueva conversación con un bot, emita esta solicitud:</span><span class="sxs-lookup"><span data-stu-id="0563f-108">To open a new conversation with a bot, issue this request:</span></span>

```http
POST https://directline.botframework.com/v3/directline/conversations
Authorization: Bearer SECRET_OR_TOKEN
```

<span data-ttu-id="0563f-109">Los fragmentos de código siguientes proporcionan un ejemplo de la solicitud y respuesta de Iniciar conversación.</span><span class="sxs-lookup"><span data-stu-id="0563f-109">The following snippets provide an example of the Start Conversation request and response.</span></span>

### <a name="request"></a><span data-ttu-id="0563f-110">Solicitud</span><span class="sxs-lookup"><span data-stu-id="0563f-110">Request</span></span>

```http
POST https://directline.botframework.com/v3/directline/conversations
Authorization: Bearer RCurR_XV9ZA.cwA.BKA.iaJrC8xpy8qbOF5xnR2vtCX7CZj0LdjAPGfiCpg4Fv0y8qbOF5xPGfiCpg4Fv0y8qqbOF5x8qbOF5xn
```

### <a name="response"></a><span data-ttu-id="0563f-111">Response</span><span class="sxs-lookup"><span data-stu-id="0563f-111">Response</span></span>

<span data-ttu-id="0563f-112">Si la solicitud es correcta, la respuesta contendrá un identificador para la conversación, un token, un valor que indica el número de segundos hasta que expire el token y una dirección URL de secuencia que el cliente puede usar para [recibir actividades mediante la secuencia de WebSocket](bot-framework-rest-direct-line-3-0-receive-activities.md#connect-via-websocket).</span><span class="sxs-lookup"><span data-stu-id="0563f-112">If the request is successful, the response will contain an ID for the conversation, a token, a value that indicates the number of seconds until the token expires, and a stream URL that the client may use to [receive activities via WebSocket stream](bot-framework-rest-direct-line-3-0-receive-activities.md#connect-via-websocket).</span></span>

```http
HTTP/1.1 201 Created
[other headers]
```

```json
{
  "conversationId": "abc123",
  "token": "RCurR_XV9ZA.cwA.BKA.iaJrC8xpy8qbOF5xnR2vtCX7CZj0LdjAPGfiCpg4Fv0y8qbOF5xPGfiCpg4Fv0y8qqbOF5x8qbOF5xn",
  "expires_in": 1800,
  "streamUrl": "https://directline.botframework.com/v3/directline/conversations/abc123/stream?t=RCurR_XV9ZA.cwA..."
}
```

<span data-ttu-id="0563f-113">Normalmente, una solicitud Iniciar conversación se usa para abrir una nueva conversación y, si la nueva conversación se inicia correctamente, se devuelve un código de estado **HTTP 201**.</span><span class="sxs-lookup"><span data-stu-id="0563f-113">Typically, a Start Conversation request is used to open a new conversation and an **HTTP 201** status code is returned if the new conversation is successfully started.</span></span> <span data-ttu-id="0563f-114">Sin embargo, si un cliente envía una solicitud Iniciar conversación con un token de Direct Line en el encabezado `Authorization` que se ha usado anteriormente para iniciar una conversación mediante la operación Iniciar conversación, se devuelve un código de estado **HTTP 200** para indicar que la solicitud era aceptable, pero no se ha creado ninguna conversación (porque ya existía).</span><span class="sxs-lookup"><span data-stu-id="0563f-114">However, if a client submits a Start Conversation request with a Direct Line token in the `Authorization` header that has previously been used to start a conversation using the Start Conversation operation, an **HTTP 200** status code will be returned to indicate that the request was acceptable but no conversation was created (as it already existed).</span></span>

> [!TIP]
> <span data-ttu-id="0563f-115">Tendrá 60 segundos para conectarse a la dirección URL de secuencia de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="0563f-115">You have 60 seconds to connect to the WebSocket stream URL.</span></span> <span data-ttu-id="0563f-116">Si no se puede establecer la conexión durante este tiempo, puede [volver a conectarse a la conversación](bot-framework-rest-direct-line-3-0-reconnect-to-conversation.md) para generar una nueva dirección URL de secuencia.</span><span class="sxs-lookup"><span data-stu-id="0563f-116">If the connection cannot be established during this time, you can [reconnect to the conversation](bot-framework-rest-direct-line-3-0-reconnect-to-conversation.md) to generate a new stream URL.</span></span>

## <a name="start-conversation-versus-generate-token"></a><span data-ttu-id="0563f-117">Iniciar conversación frente a Generar token</span><span class="sxs-lookup"><span data-stu-id="0563f-117">Start Conversation versus Generate Token</span></span>

<span data-ttu-id="0563f-118">La operación Iniciar conversación (`POST /v3/directline/conversations`) se parece a la operación [Generar token](bot-framework-rest-direct-line-3-0-authentication.md#generate-token) (`POST /v3/directline/tokens/generate`) en que ambas operaciones devuelven un `token` que puede usarse para acceder a una única conversación.</span><span class="sxs-lookup"><span data-stu-id="0563f-118">The Start Conversation operation (`POST /v3/directline/conversations`) is similar to the [Generate Token](bot-framework-rest-direct-line-3-0-authentication.md#generate-token) operation (`POST /v3/directline/tokens/generate`) in that both operations return a `token` that can be used to access a single conversation.</span></span> <span data-ttu-id="0563f-119">Sin embargo, la operación Iniciar conversación también inicia la conversación, se pone en contacto con el bot y crea una dirección URL de secuencia de WebSocket, mientras que la operación Generar token no hace ninguna de estas cosas.</span><span class="sxs-lookup"><span data-stu-id="0563f-119">However, the Start Conversation operation also starts the conversation, contacts the bot, and creates a WebSocket stream URL, whereas the Generate Token operation does none of these things.</span></span> 

<span data-ttu-id="0563f-120">Si quiere iniciar la conversación inmediatamente, use la operación Iniciar conversación.</span><span class="sxs-lookup"><span data-stu-id="0563f-120">If you intend to start the conversation immediately, use the Start Conversation operation.</span></span> <span data-ttu-id="0563f-121">Si va a distribuir el token a los clientes y quiere que inicien la conversación, use en su lugar la operación [Generar token](bot-framework-rest-direct-line-3-0-authentication.md#generate-token).</span><span class="sxs-lookup"><span data-stu-id="0563f-121">If you plan to distribute the token to clients and want them to initiate the conversation, use the [Generate Token](bot-framework-rest-direct-line-3-0-authentication.md#generate-token) operation instead.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="0563f-122">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="0563f-122">Additional resources</span></span>

- [<span data-ttu-id="0563f-123">Conceptos clave</span><span class="sxs-lookup"><span data-stu-id="0563f-123">Key concepts</span></span>](bot-framework-rest-direct-line-3-0-concepts.md)
- [<span data-ttu-id="0563f-124">Autenticación</span><span class="sxs-lookup"><span data-stu-id="0563f-124">Authentication</span></span>](bot-framework-rest-direct-line-3-0-authentication.md)
- <span data-ttu-id="0563f-125">[Receive activities via WebSocket stream](bot-framework-rest-direct-line-3-0-receive-activities.md#connect-via-websocket) (Recepción de actividades mediante una secuencia de WebSocket)</span><span class="sxs-lookup"><span data-stu-id="0563f-125">[Receive activities via WebSocket stream](bot-framework-rest-direct-line-3-0-receive-activities.md#connect-via-websocket)</span></span>
- [<span data-ttu-id="0563f-126">Volver a conectarse a una conversación</span><span class="sxs-lookup"><span data-stu-id="0563f-126">Reconnect to a conversation</span></span>](bot-framework-rest-direct-line-3-0-reconnect-to-conversation.md)