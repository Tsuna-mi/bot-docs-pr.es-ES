---
title: Envío y recepción de mensajes | Microsoft Docs
description: Obtenga información sobre cómo enviar y recibir mensajes mediante el servicio Bot Connector.
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ms.openlocfilehash: 60d96297ea4bdc6ba920b4f8f990fabb0af8b8d9
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39304738"
---
# <a name="send-and-receive-messages"></a><span data-ttu-id="8b234-103">Envío y recepción de mensajes</span><span class="sxs-lookup"><span data-stu-id="8b234-103">Send and receive messages</span></span>

<span data-ttu-id="8b234-104">El servicio Bot Connector le permite a un bot comunicarse a través de varios canales, como Skype, correo electrónico, Slack y mucho más.</span><span class="sxs-lookup"><span data-stu-id="8b234-104">The Bot Connector service enables a bot to communicate across multiple channels such as Skype, Email, Slack, and more.</span></span> <span data-ttu-id="8b234-105">Facilita la comunicación entre el bot y el usuario mediante la retransmisión de [actividades](bot-framework-rest-connector-activities.md) desde el bot hacia el canal y viceversa.</span><span class="sxs-lookup"><span data-stu-id="8b234-105">It facilitates communication between bot and user, by relaying [activities](bot-framework-rest-connector-activities.md) from bot to channel and from channel to bot.</span></span> <span data-ttu-id="8b234-106">Cada actividad contiene información utilizada para enrutar el mensaje al destino adecuado junto con información sobre quién creó el mensaje, cuál es su contexto y quién es el destinatario del mismo.</span><span class="sxs-lookup"><span data-stu-id="8b234-106">Every activity contains information used for routing the message to the appropriate destination along with information about who created the message, the context of the message, and the recipient of the message.</span></span> <span data-ttu-id="8b234-107">En este artículo se describe cómo usar el servicio Bot Connector para intercambiar actividades de **mensaje** entre el bot y el usuario en un canal.</span><span class="sxs-lookup"><span data-stu-id="8b234-107">This article describes how to use the Bot Connector service to exchange **message** activities between bot and user on a channel.</span></span> 

## <a id="create-reply"></a> <span data-ttu-id="8b234-108">Respuestas para un mensaje</span><span class="sxs-lookup"><span data-stu-id="8b234-108">Reply to a message</span></span>

### <a name="create-a-reply"></a><span data-ttu-id="8b234-109">Creación de una respuesta</span><span class="sxs-lookup"><span data-stu-id="8b234-109">Create a reply</span></span> 

<span data-ttu-id="8b234-110">Cuando el usuario envía un mensaje al bot, el bot recibirá el mensaje como un objeto [Activity][Activity] de tipo **message**.</span><span class="sxs-lookup"><span data-stu-id="8b234-110">When the user sends a message to your bot, your bot will receive the message as an [Activity][Activity] object of type **message**.</span></span> <span data-ttu-id="8b234-111">Para crear una respuesta para el mensaje de un usuario, cree un nuevo objeto [Activity][Activity] y comience por establecer estas propiedades:</span><span class="sxs-lookup"><span data-stu-id="8b234-111">To create a reply to a user's message, create a new [Activity][Activity] object and start by setting these properties:</span></span>

| <span data-ttu-id="8b234-112">Propiedad</span><span class="sxs-lookup"><span data-stu-id="8b234-112">Property</span></span> | <span data-ttu-id="8b234-113">Valor</span><span class="sxs-lookup"><span data-stu-id="8b234-113">Value</span></span> |
|----|----|
| <span data-ttu-id="8b234-114">conversation</span><span class="sxs-lookup"><span data-stu-id="8b234-114">conversation</span></span> | <span data-ttu-id="8b234-115">Establezca esta propiedad como el contenido de la propiedad `conversation` en el mensaje del usuario.</span><span class="sxs-lookup"><span data-stu-id="8b234-115">Set this property to the contents of the `conversation` property in the user's message.</span></span> |
| <span data-ttu-id="8b234-116">De</span><span class="sxs-lookup"><span data-stu-id="8b234-116">from</span></span> | <span data-ttu-id="8b234-117">Establezca esta propiedad como el contenido de la propiedad `recipient` en el mensaje del usuario.</span><span class="sxs-lookup"><span data-stu-id="8b234-117">Set this property to the contents of the `recipient` property in the user's message.</span></span> |
| <span data-ttu-id="8b234-118">locale</span><span class="sxs-lookup"><span data-stu-id="8b234-118">locale</span></span> | <span data-ttu-id="8b234-119">Establezca esta propiedad como el contenido de la propiedad `locale` en el mensaje del usuario, si está especificada.</span><span class="sxs-lookup"><span data-stu-id="8b234-119">Set this property to the contents of the `locale` property in the user's message, if specified.</span></span> |
| <span data-ttu-id="8b234-120">recipient</span><span class="sxs-lookup"><span data-stu-id="8b234-120">recipient</span></span> | <span data-ttu-id="8b234-121">Establezca esta propiedad como el contenido de la propiedad `from` en el mensaje del usuario.</span><span class="sxs-lookup"><span data-stu-id="8b234-121">Set this property to the contents of the `from` property in the user's message.</span></span> |
| <span data-ttu-id="8b234-122">replyToId</span><span class="sxs-lookup"><span data-stu-id="8b234-122">replyToId</span></span> | <span data-ttu-id="8b234-123">Establezca esta propiedad como el contenido de la propiedad `id` en el mensaje del usuario.</span><span class="sxs-lookup"><span data-stu-id="8b234-123">Set this property to the contents of the `id` property in the user's message.</span></span> |
| <span data-ttu-id="8b234-124">Tipo</span><span class="sxs-lookup"><span data-stu-id="8b234-124">type</span></span> | <span data-ttu-id="8b234-125">Establezca esta propiedad como **message**.</span><span class="sxs-lookup"><span data-stu-id="8b234-125">Set this property to **message**.</span></span> |

<span data-ttu-id="8b234-126">A continuación, establezca las propiedades que especifican la información que desea comunicarle al usuario.</span><span class="sxs-lookup"><span data-stu-id="8b234-126">Next, set the properties that specify the information that you want to communicate to the user.</span></span> <span data-ttu-id="8b234-127">Por ejemplo, puede establecer la propiedad `text` para especificar el texto que se mostrará en el mensaje, la propiedad `speak` para especificar el texto que su bot dirá en voz alta y la propiedad `attachments` para especificar los adjuntos multimedia o las tarjetas enriquecidas que se incluirán en el mensaje.</span><span class="sxs-lookup"><span data-stu-id="8b234-127">For example, you can set the `text` property to specify the text to be displayed in the message, set the `speak` property to specify text to be spoken by your bot, and set the `attachments` property to specify media attachments or rich cards to include in the message.</span></span> <span data-ttu-id="8b234-128">Para obtener información detallada acerca de las propiedades de mensaje más usadas, consulte [Create messages](bot-framework-rest-connector-create-messages.md) (Creación de mensajes).</span><span class="sxs-lookup"><span data-stu-id="8b234-128">For detailed information about commonly-used message properties, see [Create messages](bot-framework-rest-connector-create-messages.md).</span></span>

### <a name="send-the-reply"></a><span data-ttu-id="8b234-129">Envío de la respuesta</span><span class="sxs-lookup"><span data-stu-id="8b234-129">Send the reply</span></span>

<span data-ttu-id="8b234-130">Use la propiedad `serviceUrl` de la solicitud entrante para [identificar la URI base](bot-framework-rest-connector-api-reference.md#base-uri) que el bot debería usar para emitir su respuesta.</span><span class="sxs-lookup"><span data-stu-id="8b234-130">Use the `serviceUrl` property in the incoming activity to [identify the base URI](bot-framework-rest-connector-api-reference.md#base-uri) that your bot should use to issue its response.</span></span> 

<span data-ttu-id="8b234-131">Para enviar una respuesta, emita esta solicitud:</span><span class="sxs-lookup"><span data-stu-id="8b234-131">To send the reply, issue this request:</span></span> 

```http
POST /v3/conversations/{conversationId}/activities/{activityId}
```

<span data-ttu-id="8b234-132">En este URI de solicitud, reemplace **{conversationId}** por el valor de la propiedad `id` del objeto `conversation` dentro de la actividad (respuesta) y reemplace **{activityId}** por el valor de la propiedad `replyToId` dentro de la actividad (respuesta).</span><span class="sxs-lookup"><span data-stu-id="8b234-132">In this request URI, replace **{conversationId}** with the value of the `conversation` object's `id` property within your (reply) Activity and replace **{activityId}** with the value of the `replyToId` property within your (reply) Activity.</span></span> <span data-ttu-id="8b234-133">Establezca el cuerpo de la solicitud como el objeto [Activity][Activity] que creó para representar el mensaje de respuesta.</span><span class="sxs-lookup"><span data-stu-id="8b234-133">Set the body of the request to the [Activity][Activity] object that you created to represent your reply.</span></span>

<span data-ttu-id="8b234-134">En el ejemplo siguiente se muestra una solicitud que envía una respuesta sencilla de solo texto al mensaje de un usuario.</span><span class="sxs-lookup"><span data-stu-id="8b234-134">The following example shows a request that sends a simple text-only reply to a user's message.</span></span> <span data-ttu-id="8b234-135">En la solicitud del ejemplo, `https://smba.trafficmanager.net/apis` representa el URI base; el URI base para solicitudes que su bot emita puede ser distinto.</span><span class="sxs-lookup"><span data-stu-id="8b234-135">In this example request, `https://smba.trafficmanager.net/apis` represents the base URI; the base URI for requests that your bot issues may be different.</span></span> <span data-ttu-id="8b234-136">Para obtener más información sobre cómo establecer el URI base, consulte [API Reference](bot-framework-rest-connector-api-reference.md#base-uri) (Referencia de la API).</span><span class="sxs-lookup"><span data-stu-id="8b234-136">For details about setting the base URI, see [API Reference](bot-framework-rest-connector-api-reference.md#base-uri).</span></span>

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
        "name": "Pepper's News Feed"
    },
    "conversation": {
        "id": "abcd1234",
        "name": "Convo1"
   },
   "recipient": {
        "id": "1234abcd",
        "name": "SteveW"
    },
    "text": "My bot's reply",
    "replyToId": "5d5cdc723"
}
```

## <a id="send-message"></a> <span data-ttu-id="8b234-137">Envío de un mensaje (sin respuesta)</span><span class="sxs-lookup"><span data-stu-id="8b234-137">Send a (non-reply) message</span></span>

<span data-ttu-id="8b234-138">La mayoría de los mensajes que el bot envíe serán una respuesta a los mensajes que recibe del usuario.</span><span class="sxs-lookup"><span data-stu-id="8b234-138">A majority of the messages that your bot sends will be in reply to messages that it receives from the user.</span></span> <span data-ttu-id="8b234-139">Sin embargo, puede haber ocasiones en las que su bot requiera enviar a la conversación un mensaje que no es una respuesta directa a algún mensaje del usuario.</span><span class="sxs-lookup"><span data-stu-id="8b234-139">However, there may be times when your bot needs to send a message to the conversation that is not a direct reply to any message from the user.</span></span> <span data-ttu-id="8b234-140">Por ejemplo, es posible que su bot requiera iniciar un nuevo tema de conversación o enviar un mensaje de despedida al final de la conversación.</span><span class="sxs-lookup"><span data-stu-id="8b234-140">For example, your bot may need to start a new topic of conversation or send a goodbye message at the end of the conversation.</span></span> 

<span data-ttu-id="8b234-141">Para enviar a la conversación un mensaje que no sea una respuesta directa a algún mensaje del usuario, emita esta solicitud:</span><span class="sxs-lookup"><span data-stu-id="8b234-141">To send a message to a conversation that is not a direct reply to any message from the user, issue this request:</span></span> 

```http
POST /v3/conversations/{conversationId}/activities
```

<span data-ttu-id="8b234-142">En este URI de solicitud, reemplace **{conversationId}** por el identificador de la conversación.</span><span class="sxs-lookup"><span data-stu-id="8b234-142">In this request URI, replace **{conversationId}** with the ID of the conversation.</span></span> 
    
<span data-ttu-id="8b234-143">Establezca el cuerpo de la solicitud como un objeto [Activity][Activity] que se crea para representar la respuesta.</span><span class="sxs-lookup"><span data-stu-id="8b234-143">Set the body of the request to an [Activity][Activity] object that you create to represent your reply.</span></span>

> [!NOTE]
> <span data-ttu-id="8b234-144">Bot Framework no impone ninguna restricción en el número de mensajes que puede enviar un bot.</span><span class="sxs-lookup"><span data-stu-id="8b234-144">The Bot Framework does not impose any restrictions on the number of messages that a bot may send.</span></span> <span data-ttu-id="8b234-145">Sin embargo, la mayoría de los canales aplican valores de limitación para impedir que los bots envíen un gran número de mensajes en un breve período de tiempo.</span><span class="sxs-lookup"><span data-stu-id="8b234-145">However, most channels enforce throttling limits to restrict bots from sending a large number of messages in a short period of time.</span></span> <span data-ttu-id="8b234-146">Además, si el bot envía varios mensajes en una sucesión rápida, puede que el canal no siempre procese los mensajes en el orden correcto.</span><span class="sxs-lookup"><span data-stu-id="8b234-146">Additionally, if the bot sends multiple messages in quick succession, the channel may not always render the messages in the proper sequence.</span></span>

## <a name="start-a-conversation"></a><span data-ttu-id="8b234-147">Inicio de una conversación</span><span class="sxs-lookup"><span data-stu-id="8b234-147">Start a conversation</span></span>

<span data-ttu-id="8b234-148">Puede haber ocasiones en las que su bot debe iniciar una conversación con uno o varios usuarios.</span><span class="sxs-lookup"><span data-stu-id="8b234-148">There may be times when your bot needs to initiate a conversation with one or more users.</span></span> <span data-ttu-id="8b234-149">Para iniciar una conversación con algún usuario en un canal, su bot debe conocer la información de su cuenta y la información de la cuenta del usuario en dicho canal.</span><span class="sxs-lookup"><span data-stu-id="8b234-149">To start a conversation with a user on a channel, your bot must know its account information and the user's account information on that channel.</span></span> 

> [!TIP]
> <span data-ttu-id="8b234-150">Si es posible que su bot deba iniciar conversaciones con sus usuarios en el futuro, copie en caché la información de la cuenta de usuario, además de otra información pertinente, como las preferencias del usuario, la configuración regional y la dirección URL del servicio (para usarla como la URI base en la solicitud de inicio de la conversación).</span><span class="sxs-lookup"><span data-stu-id="8b234-150">If your bot may need to start conversations with its users in the future, cache user account information, other relevant information such as user preferences and locale, and the service URL (to use as the base URI in the Start Conversation request).</span></span> 

<span data-ttu-id="8b234-151">Para iniciar una conversación, emita esta solicitud:</span><span class="sxs-lookup"><span data-stu-id="8b234-151">To start a conversation, issue this request:</span></span> 

```http
POST /v3/conversations
```

<span data-ttu-id="8b234-152">Establezca el cuerpo de la solicitud como un objeto [Conversation][Conversation] que especifica la información de la cuenta de su bot y la información de la cuenta de los usuarios que se van a incluir en la conversación.</span><span class="sxs-lookup"><span data-stu-id="8b234-152">Set the body of the request to a [Conversation][Conversation] object that specifies your bot's account information and the account information of the user(s) that you want to include in the conversation.</span></span>

> [!NOTE]
> <span data-ttu-id="8b234-153">No todos los canales son compatibles con las conversaciones en grupo.</span><span class="sxs-lookup"><span data-stu-id="8b234-153">Not all channels support group conversations.</span></span> <span data-ttu-id="8b234-154">Consulte la documentación del canal para determinar si un canal es compatible con las conversaciones de grupo e identificar el número máximo de participantes que un canal permite en una conversación.</span><span class="sxs-lookup"><span data-stu-id="8b234-154">Consult the channel's documentation to determine whether a channel supports group conversations and to identify the maximum number of participants that a channel allows in a conversation.</span></span>

<span data-ttu-id="8b234-155">En el ejemplo siguiente se muestra una solicitud que inicia una conversación.</span><span class="sxs-lookup"><span data-stu-id="8b234-155">The following example shows a request that starts a conversation.</span></span> <span data-ttu-id="8b234-156">En la solicitud del ejemplo, `https://smba.trafficmanager.net/apis` representa el URI base; el URI base para solicitudes que su bot emita puede ser distinto.</span><span class="sxs-lookup"><span data-stu-id="8b234-156">In this example request, `https://smba.trafficmanager.net/apis` represents the base URI; the base URI for requests that your bot issues may be different.</span></span> <span data-ttu-id="8b234-157">Para obtener más información sobre cómo establecer el URI base, consulte [API Reference](bot-framework-rest-connector-api-reference.md#base-uri) (Referencia de la API).</span><span class="sxs-lookup"><span data-stu-id="8b234-157">For details about setting the base URI, see [API Reference](bot-framework-rest-connector-api-reference.md#base-uri).</span></span>

```http
POST https://smba.trafficmanager.net/apis/v3/conversations 
Authorization: Bearer ACCESS_TOKEN
Content-Type: application/json
```

```json
{
    "bot": {
        "id": "12345678",
        "name": "bot's name"
    },
    "isGroup": false,
    "members": [
        {
            "id": "1234abcd",
            "name": "recipient's name"
        }
    ],
    "topicName": "News Alert"
}
```

<span data-ttu-id="8b234-158">Si se establece la conversación con los usuarios especificados, la respuesta contendrá un identificador para identificar la conversación.</span><span class="sxs-lookup"><span data-stu-id="8b234-158">If the conversation is established with the specified users, the response will contain an ID that identifies the conversation.</span></span> 

```json
{
    "id": "abcd1234"
}
```

<span data-ttu-id="8b234-159">Por lo tanto, el bot puede usar este identificador de conversación para [enviar un mensaje](#send-message) a los usuarios dentro de la conversación.</span><span class="sxs-lookup"><span data-stu-id="8b234-159">Your bot can then use this conversation ID to [send a message](#send-message) to the user(s) within the conversation.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8b234-160">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="8b234-160">Additional resources</span></span>

- <span data-ttu-id="8b234-161">[Activities overview](bot-framework-rest-connector-activities.md) (Introducción a las actividades)</span><span class="sxs-lookup"><span data-stu-id="8b234-161">[Activities overview](bot-framework-rest-connector-activities.md)</span></span>
- <span data-ttu-id="8b234-162">[Create messages](bot-framework-rest-connector-create-messages.md) (Creación de mensajes)</span><span class="sxs-lookup"><span data-stu-id="8b234-162">[Create messages](bot-framework-rest-connector-create-messages.md)</span></span>

[Activity]: bot-framework-rest-connector-api-reference.md#activity-object
[ConversationAccount]: bot-framework-rest-connector-api-reference.md#conversationaccount-object
[Conversation]: bot-framework-rest-connector-api-reference.md#conversation-object

