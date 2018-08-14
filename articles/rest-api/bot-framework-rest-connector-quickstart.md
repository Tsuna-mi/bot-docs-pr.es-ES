---
title: Crear un bot con el servicio Bot Connector | Microsoft Docs
description: Cree un bot con el servicio Bot Connector.
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ms.openlocfilehash: 3baf5bde772e67084a6046a8d2a8e7d631b245f6
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305032"
---
# <a name="create-a-bot-with-the-bot-connector-service"></a><span data-ttu-id="10d05-103">Crear un bot con el servicio Bot Connector</span><span class="sxs-lookup"><span data-stu-id="10d05-103">Create a bot with the Bot Connector service</span></span>
> [!div class="op_single_selector"]
> - [.NET](../dotnet/bot-builder-dotnet-quickstart.md)
> - [Node.js](../nodejs/bot-builder-nodejs-quickstart.md)
> - [Bot Service](../bot-service-quickstart.md)
> - [REST](../rest-api/bot-framework-rest-connector-quickstart.md)

<span data-ttu-id="10d05-108">El servicio Bot Connector permite que su bot intercambie mensajes con los canales configurados en <a href="https://dev.botframework.com/" target="_blank">Bot Framework Portal</a>, mediante los estándares del sector JSON y REST a través de HTTPS.</span><span class="sxs-lookup"><span data-stu-id="10d05-108">The Bot Connector service enables your bot to exchange messages with channels that are configured in the <a href="https://dev.botframework.com/" target="_blank">Bot Framework Portal</a>, by using industry-standard REST and JSON over HTTPS.</span></span> <span data-ttu-id="10d05-109">Este tutorial le guiará a través del proceso para obtener un token de acceso de Bot Framework y utilizar el servicio Bot Connector para intercambiar mensajes con el usuario.</span><span class="sxs-lookup"><span data-stu-id="10d05-109">This tutorial walks you through the process of obtaining an access token from the Bot Framework and using the Bot Connector service to exchange messages with the user.</span></span>

## <a id="get-token"></a> <span data-ttu-id="10d05-110">Obtener un token de acceso</span><span class="sxs-lookup"><span data-stu-id="10d05-110">Get an access token</span></span>

> [!IMPORTANT]
> Si aún no lo ha hecho, deberá [registrar el bot](../bot-service-quickstart-registration.md) con Bot Framework para obtener su identificador de aplicación y la contraseña. Necesitará el identificador de la aplicación del bot y la contraseña para obtener un token de acceso.

<span data-ttu-id="10d05-113">Para comunicarse con el servicio Bot Connector, debe especificar un token de acceso en el encabezado `Authorization` de cada solicitud de API, con este formato:</span><span class="sxs-lookup"><span data-stu-id="10d05-113">To communicate with the Bot Connector service, you must specify an access token in the `Authorization` header of each API request, using this format:</span></span> 

```http
Authorization: Bearer ACCESS_TOKEN
```

<span data-ttu-id="10d05-114">Puede obtener el token de acceso para el bot si emite una solicitud de API.</span><span class="sxs-lookup"><span data-stu-id="10d05-114">You can obtain the access token for your bot by issuing an API request.</span></span>

### <a name="request"></a><span data-ttu-id="10d05-115">Solicitud</span><span class="sxs-lookup"><span data-stu-id="10d05-115">Request</span></span>

<span data-ttu-id="10d05-116">Para solicitar un token de acceso que se pueda usar para autenticar las solicitudes al servicio Bot Connector, emita la siguiente solicitud, reemplace **MICROSOFT-APP-ID** y **MICROSOFT-APP-PASSWORD** con el identificador de aplicación y la contraseña que obtuvo cuando [registró](../bot-service-quickstart-registration.md) su bot con Bot Framework.</span><span class="sxs-lookup"><span data-stu-id="10d05-116">To request an access token that can be used to authenticate requests to the Bot Connector service, issue the following request, replacing **MICROSOFT-APP-ID** and **MICROSOFT-APP-PASSWORD** with the App ID and password that you obtained when you [registered](../bot-service-quickstart-registration.md) your bot with the Bot Framework.</span></span>

```http
POST https://login.microsoftonline.com/botframework.com/oauth2/v2.0/token
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&client_id=MICROSOFT-APP-ID&client_secret=MICROSOFT-APP-PASSWORD&scope=https%3A%2F%2Fapi.botframework.com%2F.default
```

### <a name="response"></a><span data-ttu-id="10d05-117">Response</span><span class="sxs-lookup"><span data-stu-id="10d05-117">Response</span></span>

<span data-ttu-id="10d05-118">Si la solicitud se realiza correctamente, recibirá una respuesta HTTP 200 que especifica el token de acceso e información sobre la fecha de expiración.</span><span class="sxs-lookup"><span data-stu-id="10d05-118">If the request succeeds, you will receive an HTTP 200 response that specifies the access token and information about its expiration.</span></span> 

```json
{
    "token_type":"Bearer",
    "expires_in":3600,
    "ext_expires_in":3600,
    "access_token":"eyJhbGciOiJIUzI1Ni..."
}
```

> [!TIP]
> Para obtener más información sobre la autenticación en el servicio Bot Connector, consulte [Autenticación](bot-framework-rest-connector-authentication.md).

## <a name="exchange-messages-with-the-user"></a><span data-ttu-id="10d05-120">Intercambiar mensajes con el usuario</span><span class="sxs-lookup"><span data-stu-id="10d05-120">Exchange messages with the user</span></span>

<span data-ttu-id="10d05-121">Una conversación es una serie de mensajes que se intercambian un usuario y su bot.</span><span class="sxs-lookup"><span data-stu-id="10d05-121">A conversation is a series of messages exchanged between a user and your bot.</span></span> 

### <a name="receive-a-message-from-the-user"></a><span data-ttu-id="10d05-122">Recibir un mensaje del usuario</span><span class="sxs-lookup"><span data-stu-id="10d05-122">Receive a message from the user</span></span>

<span data-ttu-id="10d05-123">Cuando el usuario envía un mensaje, Bot Framework Connector envía una solicitud al punto de conexión que se especificó cuando se [registró](../bot-service-quickstart-registration.md) el bot.</span><span class="sxs-lookup"><span data-stu-id="10d05-123">When the user sends a message, the Bot Framework Connector POSTs a request to the endpoint that you specified when you [registered](../bot-service-quickstart-registration.md) your bot.</span></span> <span data-ttu-id="10d05-124">El cuerpo de la solicitud es un objeto [Activity][Activity].</span><span class="sxs-lookup"><span data-stu-id="10d05-124">The body of the request is an [Activity][Activity] object.</span></span> <span data-ttu-id="10d05-125">En el siguiente ejemplo se muestra el cuerpo de la solicitud que recibe un bot cuando el usuario le envía un mensaje simple.</span><span class="sxs-lookup"><span data-stu-id="10d05-125">The following example shows the request body that a bot receives when the user sends a simple message to the bot.</span></span> 

```json
{
    "type": "message",
    "id": "bf3cc9a2f5de...",
    "timestamp": "2016-10-19T20:17:52.2891902Z",
    "serviceUrl": "https://smba.trafficmanager.net/apis",
    "channelId": "channel's name/id",
    "from": {
        "id": "1234abcd",
        "name": "user's name"
    },
    "conversation": {
        "id": "abcd1234",
        "name": "conversation's name"
    },
    "recipient": {
        "id": "12345678",
        "name": "bot's name"
    },
    "text": "Haircut on Saturday"
}
```

### <a name="reply-to-the-users-message"></a><span data-ttu-id="10d05-126">Responder al mensaje del usuario</span><span class="sxs-lookup"><span data-stu-id="10d05-126">Reply to the user's message</span></span>

<span data-ttu-id="10d05-127">Cuando el punto de conexión del bot recibe una solicitud `POST` que representa un mensaje del usuario (es decir, `type` = **mensaje**), use la información de esa solicitud para crear el objeto [Activity][Activity] de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="10d05-127">When your bot's endpoint receives a `POST` request that represents a message from the user (i.e., `type` = **message**), use the information in that request to create the [Activity][Activity] object for your response.</span></span>

1. <span data-ttu-id="10d05-128">Establezca la propiedad **conversation** en el contenido de la propiedad **conversation** del mensaje del usuario.</span><span class="sxs-lookup"><span data-stu-id="10d05-128">Set the **conversation** property to the contents of the **conversation** property in the user's message.</span></span>
2. <span data-ttu-id="10d05-129">Establezca la propiedad **from** en el contenido de la propiedad **recipient** del mensaje del usuario.</span><span class="sxs-lookup"><span data-stu-id="10d05-129">Set the **from** property to the contents of the **recipient** property in the user's message.</span></span>
3. <span data-ttu-id="10d05-130">Establezca la propiedad **recipient** en el contenido de la propiedad **from** del mensaje del usuario.</span><span class="sxs-lookup"><span data-stu-id="10d05-130">Set the **recipient** property to the contents of the **from** property in the user's message.</span></span>
4. <span data-ttu-id="10d05-131">Establezca las propiedades **text** y **attachments** según corresponda.</span><span class="sxs-lookup"><span data-stu-id="10d05-131">Set the **text** and **attachments** properties as appropriate.</span></span>

<span data-ttu-id="10d05-132">Use la propiedad `serviceUrl` de la solicitud entrante para [identificar el URI base](bot-framework-rest-connector-api-reference.md#base-uri) que el bot debería usar para emitir su respuesta.</span><span class="sxs-lookup"><span data-stu-id="10d05-132">Use the `serviceUrl` property in the incoming request to [identify the base URI](bot-framework-rest-connector-api-reference.md#base-uri) that your bot should use to issue its response.</span></span> 

<span data-ttu-id="10d05-133">Para enviar la respuesta, `POST` el objeto [Activity][Activity] a `/v3/conversations/{conversationId}/activities/{activityId}`, tal como se muestra en el siguiente ejemplo.</span><span class="sxs-lookup"><span data-stu-id="10d05-133">To send the response, `POST` your [Activity][Activity] object to `/v3/conversations/{conversationId}/activities/{activityId}`, as shown in the following example.</span></span> <span data-ttu-id="10d05-134">El cuerpo de esta solicitud es un objeto [Activity][Activity] que solicita al usuario que seleccione una hora de cita disponible.</span><span class="sxs-lookup"><span data-stu-id="10d05-134">The body of this request is an [Activity][Activity] object that prompts the user to select an available appointment time.</span></span>

```http
POST https://smba.trafficmanager.net/apis/v3/conversations/abcd1234/activities/bf3cc9a2f5de... 
Authorization: Bearer eyJhbGciOiJIUzI1Ni...
Content-Type: application/json
```

```json
{
    "type": "message",
    "from": {
        "id": "12345678",
        "name": "bot's name"
    },
    "conversation": {
        "id": "abcd1234",
        "name": "conversation's name"
   },
   "recipient": {
        "id": "1234abcd",
        "name": "user's name"
    },
    "text": "I have these times available:",
    "replyToId": "bf3cc9a2f5de..."
}
```

<span data-ttu-id="10d05-135">En esta solicitud de ejemplo, `https://smba.trafficmanager.net/apis` representa el URI base; los URI base para las solicitudes que emite el bot pueden ser diferentes.</span><span class="sxs-lookup"><span data-stu-id="10d05-135">In this example request, `https://smba.trafficmanager.net/apis` represents the base URI; the base URI for requests that your bot issues may be different.</span></span> <span data-ttu-id="10d05-136">Para obtener más información sobre cómo establecer el URI base, consulte [Referencia de la API](bot-framework-rest-connector-api-reference.md#base-uri).</span><span class="sxs-lookup"><span data-stu-id="10d05-136">For details about setting the base URI, see [API Reference](bot-framework-rest-connector-api-reference.md#base-uri).</span></span> 

> [!IMPORTANT]
> Tal como se muestra en este ejemplo, el encabezado `Authorization` de cada solicitud de API que envíe debe contener la palabra **Bearer** (portador) seguida del token de acceso que [obtuvo en Bot Framework](#get-token).

<span data-ttu-id="10d05-138">Para enviar otro mensaje que permita al usuario seleccionar una hora de cita disponible al hacer clic en un botón, `POST` otra solicitud para el mismo punto de conexión:</span><span class="sxs-lookup"><span data-stu-id="10d05-138">To send another message that enables a user to select an available appointment time by clicking a button, `POST` another request to the same endpoint:</span></span>

```http
POST https://smba.trafficmanager.net/apis/v3/conversations/abcd1234/activities/bf3cc9a2f5de... 
Authorization: Bearer eyJhbGciOiJIUzI1Ni...
Content-Type: application/json
```

```json
{
    "type": "message",
    "from": {
        "id": "12345678",
        "name": "bot's name"
    },
    "conversation": {
        "id": "abcd1234",
        "name": "conversation's name"
   },
   "recipient": {
        "id": "1234abcd",
        "name": "user's name"
    },
    "attachmentLayout": "list",
    "attachments": [
      {
        "contentType": "application/vnd.microsoft.card.thumbnail",
        "content": {
          "buttons": [
            {
              "type": "imBack",
              "title": "10:30",
              "value": "10:30"
            },
            {
              "type": "imBack",
              "title": "11:30",
              "value": "11:30"
            },
            {
              "type": "openUrl",
              "title": "See more",
              "value": "http://www.contososalon.com/scheduling"
            }
          ]
        }
      }
    ],
    "replyToId": "bf3cc9a2f5de..."
}
```   

## <a name="next-steps"></a><span data-ttu-id="10d05-139">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="10d05-139">Next steps</span></span>

<span data-ttu-id="10d05-140">En este tutorial obtuvo un token de acceso de Bot Framework y usó el servicio Bot Connector para intercambiar mensajes con el usuario.</span><span class="sxs-lookup"><span data-stu-id="10d05-140">In this tutorial, you obtained an access token from the Bot Framework and used the Bot Connector service to exchange messages with the user.</span></span> <span data-ttu-id="10d05-141">Puede usar [Bot Framework Emulator](../bot-service-debug-emulator.md) para probar y depurar el bot.</span><span class="sxs-lookup"><span data-stu-id="10d05-141">You can use the [Bot Framework Emulator](../bot-service-debug-emulator.md) to test and debug your bot.</span></span> <span data-ttu-id="10d05-142">Si desea compartir su bot con otros usuarios, deberá [configurarlo](../bot-service-manage-channels.md) para que se ejecute en uno o más canales.</span><span class="sxs-lookup"><span data-stu-id="10d05-142">If you'd like to share your bot with others, you'll need to [configure](../bot-service-manage-channels.md) it to run on one or more channels.</span></span>


[Activity]: bot-framework-rest-connector-api-reference.md#activity-object