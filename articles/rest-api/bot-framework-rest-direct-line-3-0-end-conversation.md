---
title: Finalización de una conversación | Microsoft Docs
description: Aprenda a finalizar una conversación utilizando Direct Line API v3.0.
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ms.openlocfilehash: ac984609acfdd8f85088bd47ccded1f45e953b2c
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305573"
---
# <a name="end-a-conversation"></a><span data-ttu-id="54894-103">Finalización de una conversación</span><span class="sxs-lookup"><span data-stu-id="54894-103">End a conversation</span></span>

<span data-ttu-id="54894-104">Un cliente o un bot pueden indicar el final de una conversación de Direct Line enviando una [actividad](bot-framework-rest-connector-activities.md) **endOfConversation**.</span><span class="sxs-lookup"><span data-stu-id="54894-104">Either a client or a bot may signal the end of a Direct Line conversation by sending an **endOfConversation** [activity](bot-framework-rest-connector-activities.md).</span></span> 

## <a name="send-an-endofconversation-activity"></a><span data-ttu-id="54894-105">Envío de una actividad endOfConversation</span><span class="sxs-lookup"><span data-stu-id="54894-105">Send an endOfConversation activity</span></span>

<span data-ttu-id="54894-106">Una actividad **endOfConversation** finaliza la comunicación entre bot y cliente.</span><span class="sxs-lookup"><span data-stu-id="54894-106">An **endOfConversation** activity ends communication between bot and client.</span></span> <span data-ttu-id="54894-107">Después de que se haya enviado una actividad **endOfConversation**, el cliente aún puede [recuperar mensajes](bot-framework-rest-direct-line-3-0-receive-activities.md#http-get) mediante `HTTP GET`, pero ni el cliente ni el bot pueden enviar ningún mensaje adicional a la conversación.</span><span class="sxs-lookup"><span data-stu-id="54894-107">After an **endOfConversation** activity has been sent, the client may still [retrieve messages](bot-framework-rest-direct-line-3-0-receive-activities.md#http-get) using `HTTP GET`, but neither the client nor the bot can send any additional messages to the conversation.</span></span> 

<span data-ttu-id="54894-108">Para finalizar una conversación, solo tiene que emitir una solicitud POST para enviar una actividad **endOfConversation**.</span><span class="sxs-lookup"><span data-stu-id="54894-108">To end a conversation, simply issue a POST request to send an **endOfConversation** activity.</span></span>

### <a name="request"></a><span data-ttu-id="54894-109">Solicitud</span><span class="sxs-lookup"><span data-stu-id="54894-109">Request</span></span>

```http
POST https://directline.botframework.com/v3/directline/conversations/abc123/activities
Authorization: Bearer RCurR_XV9ZA.cwA.BKA.iaJrC8xpy8qbOF5xnR2vtCX7CZj0LdjAPGfiCpg4Fv0
[other headers]
```

```json
{
    "type": "endOfConversation",
    "from": {
        "id": "user1"
    }
}
```

### <a name="response"></a><span data-ttu-id="54894-110">Response</span><span class="sxs-lookup"><span data-stu-id="54894-110">Response</span></span>

<span data-ttu-id="54894-111">Si la solicitud es correcta, la respuesta contendrá un identificador para la actividad que se envió.</span><span class="sxs-lookup"><span data-stu-id="54894-111">If the request is successful, the response will contain an ID for the activity that was sent.</span></span>

```http
HTTP/1.1 200 OK
[other headers]
```

```json
{
  "id": "0004"
}
```

## <a name="additional-resources"></a><span data-ttu-id="54894-112">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="54894-112">Additional resources</span></span>

- [<span data-ttu-id="54894-113">Conceptos clave</span><span class="sxs-lookup"><span data-stu-id="54894-113">Key concepts</span></span>](bot-framework-rest-direct-line-3-0-concepts.md)
- [<span data-ttu-id="54894-114">Autenticación</span><span class="sxs-lookup"><span data-stu-id="54894-114">Authentication</span></span>](bot-framework-rest-direct-line-3-0-authentication.md)
- [<span data-ttu-id="54894-115">Envío de una actividad al bot</span><span class="sxs-lookup"><span data-stu-id="54894-115">Send an activity to the bot</span></span>](bot-framework-rest-direct-line-3-0-send-activity.md)
