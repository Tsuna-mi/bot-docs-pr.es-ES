---
title: Incorporación de acciones sugeridas a mensajes | Microsoft Docs
description: Obtenga información sobre cómo agregar acciones sugeridas a mensajes mediante el servicio Bot Connector.
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ms.openlocfilehash: d162bdd3f34848b16380317c776f445bc4611157
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305773"
---
# <a name="add-suggested-actions-to-messages"></a><span data-ttu-id="8d85c-103">Incorporación de acciones sugeridas a mensajes</span><span class="sxs-lookup"><span data-stu-id="8d85c-103">Add suggested actions to messages</span></span>
> [!div class="op_single_selector"]
> - [.NET](../dotnet/bot-builder-dotnet-add-suggested-actions.md)
> - [Node.js](../nodejs/bot-builder-nodejs-send-suggested-actions.md)
> - [REST](../rest-api/bot-framework-rest-connector-add-suggested-actions.md)

[!INCLUDE [Introduction to suggested actions](../includes/snippet-suggested-actions-intro.md)]

> [!TIP]
> Para obtener información sobre diferentes canales representan acciones sugeridas, vea [Channel Inspector][channelInspector].

## <a name="send-suggested-actions"></a><span data-ttu-id="8d85c-108">Envío de acciones sugeridas</span><span class="sxs-lookup"><span data-stu-id="8d85c-108">Send suggested actions</span></span>

<span data-ttu-id="8d85c-109">Para agregar acciones sugeridas a un mensaje, establezca la propiedad `suggestedActions` de la [actividad][Activity] para especificar la lista de objetos [CardAction][CardAction] que representan los botones que se presentarán al usuario.</span><span class="sxs-lookup"><span data-stu-id="8d85c-109">To add suggested actions to a message, set the `suggestedActions` property of the [Activity][Activity] to specify the list of [CardAction][CardAction] objects that represent the buttons to be presented to the user.</span></span> 

<span data-ttu-id="8d85c-110">La siguiente solicitud envía un mensaje que presenta tres acciones sugeridas al usuario.</span><span class="sxs-lookup"><span data-stu-id="8d85c-110">The following request sends a message that presents three suggested actions to the user.</span></span> <span data-ttu-id="8d85c-111">En esta solicitud de ejemplo, `https://smba.trafficmanager.net/apis` representa el URI base; los URI base para las solicitudes que emite el bot pueden ser diferentes.</span><span class="sxs-lookup"><span data-stu-id="8d85c-111">In this example request, `https://smba.trafficmanager.net/apis` represents the base URI; the base URI for requests that your bot issues may be different.</span></span> <span data-ttu-id="8d85c-112">Para más información sobre cómo establecer el URI base, vea [Referencia de la API](bot-framework-rest-connector-api-reference.md#base-uri).</span><span class="sxs-lookup"><span data-stu-id="8d85c-112">For details about setting the base URI, see [API Reference](bot-framework-rest-connector-api-reference.md#base-uri).</span></span>

```http
POST https://smba.trafficmanager.net/apis/v3/conversations/abcd1234/activities/5d5cdc723
Authorization: Bearer ACCESS_TOKEN
Content-Type: application/json
```

```json
{
    "type": "message",
    "from": {
        "id": "12345678",
        "name": "sender's name"
    },
    "conversation": {
        "id": "abcd1234",
        "name": "conversation's name"
   },
   "recipient": {
        "id": "1234abcd",
        "name": "recipient's name"
    },
    "text": "I have colors in mind, but need your help to choose the best one.",
    "inputHint": "expectingInput",
    "suggestedActions": {
        "actions": [
            {
                "type": "imBack",
                "title": "Blue",
                "value": "Blue"
            },
            {
                "type": "imBack",
                "title": "Red",
                "value": "Red"
            },
            {
                "type": "imBack",
                "title": "Green",
                "value": "Green"
            }
        ]
    },
    "replyToId": "5d5cdc723"
}
```

<span data-ttu-id="8d85c-113">Cuando el usuario pulsa en una de las acciones sugeridas, el bot recibirá un mensaje del usuario que contiene el elemento `value` de la acción correspondiente.</span><span class="sxs-lookup"><span data-stu-id="8d85c-113">When the user taps one of the suggested actions, the bot will receive a message from the user that contains the `value` of the corresponding action.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8d85c-114">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="8d85c-114">Additional resources</span></span>

- [<span data-ttu-id="8d85c-115">Creación de mensajes</span><span class="sxs-lookup"><span data-stu-id="8d85c-115">Create messages</span></span>](bot-framework-rest-connector-create-messages.md)
- [<span data-ttu-id="8d85c-116">Envío y recepción de mensajes</span><span class="sxs-lookup"><span data-stu-id="8d85c-116">Send and receive messages</span></span>](bot-framework-rest-connector-send-and-receive-messages.md)

[channelInspector]: ../bot-service-channel-inspector.md

[Activity]: bot-framework-rest-connector-api-reference.md#activity-object

[CardAction]: bot-framework-rest-connector-api-reference.md#cardaction-object

[SuggestedAction]: bot-framework-rest-connector-api-reference.md#suggestedactions-object