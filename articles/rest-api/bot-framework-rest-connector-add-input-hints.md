---
title: Incorporación de sugerencias de entrada a mensajes | Microsoft Docs
description: Obtenga información sobre cómo agregar sugerencias de entrada a los mensajes mediante el servicio Bot Connector.
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ms.openlocfilehash: 66c6bc20013ff2de82e29af76e9c99898c8b13d9
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305769"
---
# <a name="add-input-hints-to-messages"></a><span data-ttu-id="27d9d-103">Incorporación de sugerencias de entrada a mensajes</span><span class="sxs-lookup"><span data-stu-id="27d9d-103">Add input hints to messages</span></span>
> [!div class="op_single_selector"]
> - [.NET](../dotnet/bot-builder-dotnet-add-input-hints.md)
> - [Node.js](../nodejs/bot-builder-nodejs-send-input-hints.md)
> - [REST](../rest-api/bot-framework-rest-connector-add-input-hints.md)

<span data-ttu-id="27d9d-107">Al especificar una sugerencia de entrada para un mensaje, puede indicar si el bot acepta, espera o ignora la entrada del usuario después de que el mensaje se haya entregado al cliente.</span><span class="sxs-lookup"><span data-stu-id="27d9d-107">By specifying an input hint for a message, you can indicate whether your bot is accepting, expecting, or ignoring user input after the message is delivered to the client.</span></span> <span data-ttu-id="27d9d-108">En muchos canales, esto permite a los clientes establecer en consecuencia el estado de los controles de entrada de usuario.</span><span class="sxs-lookup"><span data-stu-id="27d9d-108">For many channels, this enables clients to set the state of user input controls accordingly.</span></span> <span data-ttu-id="27d9d-109">Por ejemplo, si la sugerencia de entrada de un mensaje indica que el bot ignora la entrada de usuario, el cliente podría desactivar el micrófono y deshabilitar el cuadro de entrada para impedir que el usuario proporcione una entrada.</span><span class="sxs-lookup"><span data-stu-id="27d9d-109">For example, if a message's input hint indicates that the bot is ignoring user input, the client may close the microphone and disable the input box to prevent the user from providing input.</span></span>

## <a name="accepting-input"></a><span data-ttu-id="27d9d-110">Aceptar entradas</span><span class="sxs-lookup"><span data-stu-id="27d9d-110">Accepting input</span></span>

<span data-ttu-id="27d9d-111">Para indicar que el bot está preparado pasivamente para la entrada, pero que no espera ninguna respuesta del usuario, establezca la propiedad `inputHint` en **acceptingInput** en el objeto [Activity][Activity] que representa el mensaje.</span><span class="sxs-lookup"><span data-stu-id="27d9d-111">To indicate that your bot is passively ready for input but is not awaiting a response from the user, set the `inputHint` property to **acceptingInput** within the [Activity][Activity] object that represents your message.</span></span> <span data-ttu-id="27d9d-112">En muchos canales, esto hará que se habilite el cuadro de entrada del cliente y que el micrófono se desactive, sin dejar de estar accesible para el usuario.</span><span class="sxs-lookup"><span data-stu-id="27d9d-112">On many channels, this will cause the client's input box to be enabled and microphone to be closed, but still accessible to the user.</span></span> <span data-ttu-id="27d9d-113">Por ejemplo, Cortana activará el micrófono para aceptar la entrada del usuario si este mantiene presionado el botón de micrófono.</span><span class="sxs-lookup"><span data-stu-id="27d9d-113">For example, Cortana will open the microphone to accept input from the user if the user holds down the microphone button.</span></span> 

<span data-ttu-id="27d9d-114">En el ejemplo siguiente se muestra una solicitud que envía un mensaje y especifica que el bot acepta la entrada.</span><span class="sxs-lookup"><span data-stu-id="27d9d-114">The following example shows a request that sends a message and specifies that the bot is accepting input.</span></span> <span data-ttu-id="27d9d-115">En esta solicitud de ejemplo, `https://smba.trafficmanager.net/apis` representa el URI base; los URI base para las solicitudes que emite el bot pueden ser diferentes.</span><span class="sxs-lookup"><span data-stu-id="27d9d-115">In this example request, `https://smba.trafficmanager.net/apis` represents the base URI; the base URI for requests that your bot issues may be different.</span></span> <span data-ttu-id="27d9d-116">Para más información sobre cómo establecer el URI base, vea [Referencia de la API](bot-framework-rest-connector-api-reference.md#base-uri).</span><span class="sxs-lookup"><span data-stu-id="27d9d-116">For details about setting the base URI, see [API Reference](bot-framework-rest-connector-api-reference.md#base-uri).</span></span>

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
    "text": "Here's a picture of the house I was telling you about.",
    "inputHint": "acceptingInput",
    "replyToId": "5d5cdc723"
}
```

## <a name="expecting-input"></a><span data-ttu-id="27d9d-117">Esperar entradas</span><span class="sxs-lookup"><span data-stu-id="27d9d-117">Expecting input</span></span>

<span data-ttu-id="27d9d-118">Para indicar que el bot está esperando una respuesta del usuario, establezca la propiedad `inputHint` en **expectingInput** en el objeto [Activity][Activity] que representa el mensaje.</span><span class="sxs-lookup"><span data-stu-id="27d9d-118">To indicate that your bot is awaiting a response from the user, set the `inputHint` property to **expectingInput** within the [Activity][Activity] object that represents your message.</span></span> <span data-ttu-id="27d9d-119">En muchos canales, esto hará que se habilite el cuadro de entrada del cliente y que el micrófono se active.</span><span class="sxs-lookup"><span data-stu-id="27d9d-119">On many channels, this will cause the client's input box to be enabled and microphone to be open.</span></span> 

<span data-ttu-id="27d9d-120">En el ejemplo siguiente se muestra una solicitud que envía un mensaje y especifica que el bot está esperando la entrada.</span><span class="sxs-lookup"><span data-stu-id="27d9d-120">The following example shows a request that sends a message and specifies that the bot is expecting input.</span></span> <span data-ttu-id="27d9d-121">En esta solicitud de ejemplo, `https://smba.trafficmanager.net/apis` representa el URI base; los URI base para las solicitudes que emite el bot pueden ser diferentes.</span><span class="sxs-lookup"><span data-stu-id="27d9d-121">In this example request, `https://smba.trafficmanager.net/apis` represents the base URI; the base URI for requests that your bot issues may be different.</span></span> <span data-ttu-id="27d9d-122">Para más información sobre cómo establecer el URI base, vea [Referencia de la API](bot-framework-rest-connector-api-reference.md#base-uri).</span><span class="sxs-lookup"><span data-stu-id="27d9d-122">For details about setting the base URI, see [API Reference](bot-framework-rest-connector-api-reference.md#base-uri).</span></span>

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
    "text": "What is your favorite color?",
    "inputHint": "expectingInput",
    "replyToId": "5d5cdc723"
}
```

## <a name="ignoring-input"></a><span data-ttu-id="27d9d-123">Ignorar entradas</span><span class="sxs-lookup"><span data-stu-id="27d9d-123">Ignoring input</span></span>
 
<span data-ttu-id="27d9d-124">Para indicar que el bot no está preparado para recibir una entrada del usuario, establezca la propiedad `inputHint` en **ignoringInput** en el objeto [Activity][Activity] que representa el mensaje.</span><span class="sxs-lookup"><span data-stu-id="27d9d-124">To indicate that your bot is not ready to receive input from the user, set the `inputHint` property to **ignoringInput** within the [Activity][Activity] object that represents your message.</span></span> <span data-ttu-id="27d9d-125">En muchos canales, esto hará que se deshabilite el cuadro de entrada del cliente y que el micrófono se desactive.</span><span class="sxs-lookup"><span data-stu-id="27d9d-125">On many channels, this will cause the client's input box to be disabled and microphone to be closed.</span></span> 

<span data-ttu-id="27d9d-126">En el ejemplo siguiente se muestra una solicitud que envía un mensaje y especifica que el bot está ignorando la entrada.</span><span class="sxs-lookup"><span data-stu-id="27d9d-126">The following example shows a request that sends a message and specifies that the bot is ignoring input.</span></span> <span data-ttu-id="27d9d-127">En esta solicitud de ejemplo, `https://smba.trafficmanager.net/apis` representa el URI base; los URI base para las solicitudes que emite el bot pueden ser diferentes.</span><span class="sxs-lookup"><span data-stu-id="27d9d-127">In this example request, `https://smba.trafficmanager.net/apis` represents the base URI; the base URI for requests that your bot issues may be different.</span></span> <span data-ttu-id="27d9d-128">Para más información sobre cómo establecer el URI base, vea [Referencia de la API](bot-framework-rest-connector-api-reference.md#base-uri).</span><span class="sxs-lookup"><span data-stu-id="27d9d-128">For details about setting the base URI, see [API Reference](bot-framework-rest-connector-api-reference.md#base-uri).</span></span>

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
    "text": "Please hold while I perform the calculation.",
    "inputHint": "ignoringInput",
    "replyToId": "5d5cdc723"
}
```

## <a name="additional-resources"></a><span data-ttu-id="27d9d-129">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="27d9d-129">Additional resources</span></span>

- [<span data-ttu-id="27d9d-130">Creación de mensajes</span><span class="sxs-lookup"><span data-stu-id="27d9d-130">Create messages</span></span>](bot-framework-rest-connector-create-messages.md)
- [<span data-ttu-id="27d9d-131">Envío y recepción de mensajes</span><span class="sxs-lookup"><span data-stu-id="27d9d-131">Send and receive messages</span></span>](bot-framework-rest-connector-send-and-receive-messages.md)

[Activity]: bot-framework-rest-connector-api-reference.md#activity-object