---
title: Inicio de una conversación | Microsoft Docs
description: Obtenga información sobre cómo iniciar una conversación mediante Direct Line API v1.1.
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ms.openlocfilehash: 36645ce3811c77a3ca7ed697eeae63027fa1644a
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305416"
---
# <a name="start-a-conversation"></a><span data-ttu-id="ba989-103">Inicio de una conversación</span><span class="sxs-lookup"><span data-stu-id="ba989-103">Start a conversation</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ba989-104">En este artículo se describe cómo iniciar una conversación mediante Direct Line API 1.1.</span><span class="sxs-lookup"><span data-stu-id="ba989-104">This article describes how to start a conversation using Direct Line API 1.1.</span></span> <span data-ttu-id="ba989-105">Si va a crear una nueva conexión entre la aplicación cliente y el bot, use [Direct Line API 3.0](bot-framework-rest-direct-line-3-0-start-conversation.md) en su lugar.</span><span class="sxs-lookup"><span data-stu-id="ba989-105">If you are creating a new connection between your client application and bot, use [Direct Line API 3.0](bot-framework-rest-direct-line-3-0-start-conversation.md) instead.</span></span>

<span data-ttu-id="ba989-106">Los clientes pueden abrir las conversaciones de Direct Line explícitamente y estas se pueden ejecutar siempre y cuando el cliente y el bot participen y tengan credenciales válidas.</span><span class="sxs-lookup"><span data-stu-id="ba989-106">Direct Line conversations are explicitly opened by clients and may run as long as the bot and client participate and have valid credentials.</span></span> <span data-ttu-id="ba989-107">Mientras la conversación está abierta, el bot y el cliente pueden enviar mensajes.</span><span class="sxs-lookup"><span data-stu-id="ba989-107">While the conversation is open, both the bot and client may send messages.</span></span> <span data-ttu-id="ba989-108">Más de un cliente puede conectarse a una conversación determinada y cada cliente puede participar en nombre de varios usuarios.</span><span class="sxs-lookup"><span data-stu-id="ba989-108">More than one client may connect to a given conversation and each client may participate on behalf of multiple users.</span></span>

## <a name="open-a-new-conversation"></a><span data-ttu-id="ba989-109">Apertura de una nueva conversación</span><span class="sxs-lookup"><span data-stu-id="ba989-109">Open a new conversation</span></span>

<span data-ttu-id="ba989-110">Para abrir una nueva conversación con un bot, emita esta solicitud:</span><span class="sxs-lookup"><span data-stu-id="ba989-110">To open a new conversation with a bot, issue this request:</span></span>

```http
POST https://directline.botframework.com/api/conversations
Authorization: Bearer SECRET_OR_TOKEN
```

<span data-ttu-id="ba989-111">Los fragmentos de código siguientes proporcionan un ejemplo de la solicitud y respuesta de Iniciar conversación.</span><span class="sxs-lookup"><span data-stu-id="ba989-111">The following snippets provide an example of the Start Conversation request and response.</span></span>

### <a name="request"></a><span data-ttu-id="ba989-112">Solicitud</span><span class="sxs-lookup"><span data-stu-id="ba989-112">Request</span></span>

```http
POST https://directline.botframework.com/api/conversations
Authorization: Bearer RCurR_XV9ZA.cwA.BKA.iaJrC8xpy8qbOF5xnR2vtCX7CZj0LdjAPGfiCpg4Fv0y8qbOF5xPGfiCpg4Fv0y8qqbOF5x8qbOF5xn
```

### <a name="response"></a><span data-ttu-id="ba989-113">Response</span><span class="sxs-lookup"><span data-stu-id="ba989-113">Response</span></span>

<span data-ttu-id="ba989-114">Si la solicitud es correcta, la respuesta contendrá un identificadorpara la conversación, un token y un valor que indica el número de segundos hasta que el token expira.</span><span class="sxs-lookup"><span data-stu-id="ba989-114">If the request is successful, the response will contain an ID for the conversation, a token, and a value that indicates the number of seconds until the token expires.</span></span>

```http
HTTP/1.1 200 OK
[other headers]
```

```json
{
  "conversationId": "abc123",
  "token": "RCurR_XV9ZA.cwA.BKA.iaJrC8xpy8qbOF5xnR2vtCX7CZj0LdjAPGfiCpg4Fv0y8qbOF5xPGfiCpg4Fv0y8qqbOF5x8qbOF5xn",
  "expires_in": 1800
}
```

## <a name="start-conversation-versus-generate-token"></a><span data-ttu-id="ba989-115">Iniciar conversación frente a Generar token</span><span class="sxs-lookup"><span data-stu-id="ba989-115">Start Conversation versus Generate Token</span></span>

<span data-ttu-id="ba989-116">La operación Iniciar conversación (`POST /api/conversations`) se parece a la operación [Generar token](bot-framework-rest-direct-line-1-1-authentication.md#generate-token) (`POST /api/tokens/conversation`) en que ambas operaciones devuelven un `token` que puede usarse para acceder a una única conversación.</span><span class="sxs-lookup"><span data-stu-id="ba989-116">The Start Conversation operation (`POST /api/conversations`) is similar to the [Generate Token](bot-framework-rest-direct-line-1-1-authentication.md#generate-token) operation (`POST /api/tokens/conversation`) in that both operations return a `token` that can be used to access a single conversation.</span></span> <span data-ttu-id="ba989-117">Sin embargo, la operación Iniciar conversación también inicia la conversación y se pone en contacto con el bot, mientras que la operación Generar token no hace ninguna de estas cosas.</span><span class="sxs-lookup"><span data-stu-id="ba989-117">However, the Start Conversation operation also starts the conversation and contacts the bot, whereas the Generate Token operation does neither of these things.</span></span> 

<span data-ttu-id="ba989-118">Si quiere iniciar la conversación inmediatamente, use la operación Iniciar conversación.</span><span class="sxs-lookup"><span data-stu-id="ba989-118">If you intend to start the conversation immediately, use the Start Conversation operation.</span></span> <span data-ttu-id="ba989-119">Si va a distribuir el token a los clientes y quiere que inicien la conversación, use en su lugar la operación [Generar token](bot-framework-rest-direct-line-1-1-authentication.md#generate-token).</span><span class="sxs-lookup"><span data-stu-id="ba989-119">If you plan to distribute the token to clients and want them to initiate the conversation, use the [Generate Token](bot-framework-rest-direct-line-1-1-authentication.md#generate-token) operation instead.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="ba989-120">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="ba989-120">Additional resources</span></span>

- [<span data-ttu-id="ba989-121">Conceptos clave</span><span class="sxs-lookup"><span data-stu-id="ba989-121">Key concepts</span></span>](bot-framework-rest-direct-line-1-1-concepts.md)
- [<span data-ttu-id="ba989-122">Autenticación</span><span class="sxs-lookup"><span data-stu-id="ba989-122">Authentication</span></span>](bot-framework-rest-direct-line-1-1-authentication.md)