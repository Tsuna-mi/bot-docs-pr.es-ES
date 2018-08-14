---
title: Envío y recepción de actividades | Microsoft Docs
description: Obtenga información sobre cómo intercambiar información con un usuario a través de diversos canales con el servicio Connector a través del SDK de Bot Builder para. NET.
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: 2a5c9dcad0d9fd70caaf28ff7ac95830bd47e2d6
ms.sourcegitcommit: 67445b42796d90661afc643c6bb6533e9a662cbc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2018
ms.locfileid: "39574601"
---
# <a name="send-and-receive-activities"></a><span data-ttu-id="b00c1-103">Envío y recepción de actividades</span><span class="sxs-lookup"><span data-stu-id="b00c1-103">Send and receive activities</span></span>

[!INCLUDE [pre-release-label](../includes/pre-release-label-v3.md)]

<span data-ttu-id="b00c1-104">Bot Framework Connector proporciona una única API de REST que permite que un bot se comunique a través de varios canales, como Skype, correo electrónico, Slack y mucho más.</span><span class="sxs-lookup"><span data-stu-id="b00c1-104">The Bot Framework Connector provides a single REST API that enables a bot to communicate across multiple channels such as Skype, Email, Slack, and more.</span></span> <span data-ttu-id="b00c1-105">Facilita la comunicación entre el bot y el usuario mediante la retransmisión de mensajes del bot al canal y viceversa.</span><span class="sxs-lookup"><span data-stu-id="b00c1-105">It facilitates communication between bot and user, by relaying messages from bot to channel and from channel to bot.</span></span> 

<span data-ttu-id="b00c1-106">En este artículo se describe cómo usar el conector a través del SDK de Bot Builder para .NET con el fin de intercambiar información entre el bot y el usuario en un canal.</span><span class="sxs-lookup"><span data-stu-id="b00c1-106">This article describes how to use the Connector via the Bot Builder SDK for .NET to exchange information between bot and user on a channel.</span></span> 

> [!NOTE]
> <span data-ttu-id="b00c1-107">Si bien es posible construir un bot mediante solo con las técnicas que se describen en este artículo, el SDK de Bot Builder proporciona características adicionales como [diálogos](bot-builder-dotnet-dialogs.md) y [FormFlow](bot-builder-dotnet-formflow.md) que pueden optimizar el proceso de administrar el estado y el flujo de una conversación para simplificar la incorporación de Cognitive Services, como Language Understanding.</span><span class="sxs-lookup"><span data-stu-id="b00c1-107">While it is possible to construct a bot by exclusively using the techniques that are described in this article, the Bot Builder SDK provides additional features like [dialogs](bot-builder-dotnet-dialogs.md) and [FormFlow](bot-builder-dotnet-formflow.md) that can streamline the process of managing conversation flow and state and make it simpler to incorporate cognitive services such as language understanding.</span></span>

## <a name="create-a-connector-client"></a><span data-ttu-id="b00c1-108">Creación de un cliente de conector</span><span class="sxs-lookup"><span data-stu-id="b00c1-108">Create a connector client</span></span>

<span data-ttu-id="b00c1-109">La clase [ConnectorClient][ConnectorClient] incluye los métodos que un bot usa para comunicarse con un usuario en un canal.</span><span class="sxs-lookup"><span data-stu-id="b00c1-109">The [ConnectorClient][ConnectorClient] class contains the methods that a bot uses to communicate with a user on a channel.</span></span> <span data-ttu-id="b00c1-110">Cuando el bot recibe un objeto <a href="https://docs.botframework.com/en-us/csharp/builder/sdkreference/dc/d2f/class_microsoft_1_1_bot_1_1_connector_1_1_activity.html" target="_blank">Activity</a> desde el conector, debe usar el objeto `ServiceUrl` especificado para esa actividad para crear el cliente de conector que usará posteriormente para generar una respuesta.</span><span class="sxs-lookup"><span data-stu-id="b00c1-110">When your bot receives an <a href="https://docs.botframework.com/en-us/csharp/builder/sdkreference/dc/d2f/class_microsoft_1_1_bot_1_1_connector_1_1_activity.html" target="_blank">Activity</a> object from the Connector, it should use the `ServiceUrl` specified for that activity to create the connector client that it'll subsequently use to generate a response.</span></span> 

[!code-csharp[Create connector client](../includes/code/dotnet-send-and-receive.cs#createConnectorClient)]

> [!TIP]
> <span data-ttu-id="b00c1-111">Dado que es posible que el punto de conexión de un canal no sea estable, el bot debe dirigir las comunicaciones al punto de conexión que el conector especifica en el objeto `Activity`, siempre que sea posible (en lugar de basarse en un punto de conexión almacenado en caché).</span><span class="sxs-lookup"><span data-stu-id="b00c1-111">Because a channel's endpoint may not be stable, your bot should direct communications to the endpoint that the Connector specifies in the `Activity` object, whenever possible (rather than relying upon a cached endpoint).</span></span> 
>
> <span data-ttu-id="b00c1-112">Si el bot debe iniciar la conversación, puede usar un punto de conexión almacenado en caché para el canal especificado (debido a que no habrá ningún objeto `Activity` entrante en ese escenario), pero deberá actualizar a menudo los puntos de conexión almacenados en caché.</span><span class="sxs-lookup"><span data-stu-id="b00c1-112">If your bot needs to initiate the conversation, it can use a cached endpoint for the specified channel (since there will be no incoming `Activity` object in that scenario), but it should refresh cached endpoints often.</span></span> 

## <a id="create-reply"></a> <span data-ttu-id="b00c1-113">Creación de una respuesta</span><span class="sxs-lookup"><span data-stu-id="b00c1-113">Create a reply</span></span>

<span data-ttu-id="b00c1-114">El conector usa un objeto [Activity](bot-builder-dotnet-activities.md) para pasar información de un lado a otro entre el bot y el canal (usuario).</span><span class="sxs-lookup"><span data-stu-id="b00c1-114">The Connector uses an [Activity](bot-builder-dotnet-activities.md) object to pass information back and forth between bot and channel (user).</span></span> <span data-ttu-id="b00c1-115">Cada actividad contiene información utilizada para enrutar el mensaje al destino adecuado junto con información sobre quién creó el mensaje (propiedad `From`), cuál es el contexto del mensaje y quién es el destinatario del mismo (propiedad `Recipient`).</span><span class="sxs-lookup"><span data-stu-id="b00c1-115">Every activity contains information used for routing the message to the appropriate destination along with information about who created the message (`From` property), the context of the message, and the recipient of the message (`Recipient` property).</span></span>

<span data-ttu-id="b00c1-116">Cuando el bot recibe una actividad del conector, la propiedad `Recipient` de la actividad entrante especifica la identidad del bot en esa conversación.</span><span class="sxs-lookup"><span data-stu-id="b00c1-116">When your bot receives an activity from the Connector, the incoming activity's `Recipient` property specifies the bot's identity in that conversation.</span></span> <span data-ttu-id="b00c1-117">Como algunos canales (por ejemplo, Slack) asignan una identidad nueva al bot cuando se agrega a una conversación, el bot siempre debe usar el valor de la propiedad `Recipient` de la actividad entrante como el valor de la propiedad `From` en su respuesta.</span><span class="sxs-lookup"><span data-stu-id="b00c1-117">Because some channels (e.g., Slack) assign the bot a new identity when it's added to a conversation, the bot should always use the value of the incoming activity's `Recipient` property as the value of the `From` property in its response.</span></span>

<span data-ttu-id="b00c1-118">Si bien el mismo usuario puede crear e inicializar el objeto `Activity` saliente desde cero, el SDK de Bot Builder proporciona una manera más sencilla de crear una respuesta.</span><span class="sxs-lookup"><span data-stu-id="b00c1-118">Although you can create and initialize the outgoing `Activity` object yourself from scratch, the Bot Builder SDK provides an easier way of creating a reply.</span></span> <span data-ttu-id="b00c1-119">Con el método `CreateReply` de la actividad entrante, simplemente debe especificar el texto del mensaje para la respuesta y la actividad saliente se crea con las propiedades `Recipient`, `From` y `Conversation` que se rellenan automáticamente.</span><span class="sxs-lookup"><span data-stu-id="b00c1-119">By using the incoming activity's `CreateReply` method, you simply specify the message text for the response, and the outgoing activity is created with the `Recipient`, `From`, and `Conversation` properties automatically populated.</span></span>

[!code-csharp[Create reply](../includes/code/dotnet-send-and-receive.cs#createReply)]

## <a name="send-a-reply"></a><span data-ttu-id="b00c1-120">Envío de una respuesta</span><span class="sxs-lookup"><span data-stu-id="b00c1-120">Send a reply</span></span>

<span data-ttu-id="b00c1-121">Una vez que crea una respuesta, puede enviarla si llama al método `ReplyToActivity` del cliente de conector.</span><span class="sxs-lookup"><span data-stu-id="b00c1-121">Once you've created a reply, you can send it by calling the connector client's `ReplyToActivity` method.</span></span> <span data-ttu-id="b00c1-122">El conector entregará la respuesta a través de la semántica de canal correspondiente.</span><span class="sxs-lookup"><span data-stu-id="b00c1-122">The Connector will deliver the reply using the appropriate channel semantics.</span></span> 

[!code-csharp[Send reply](../includes/code/dotnet-send-and-receive.cs#sendReply)]

> [!TIP]
> <span data-ttu-id="b00c1-123">Si el bot responde el mensaje de un usuario, use siempre el método `ReplyToActivity`.</span><span class="sxs-lookup"><span data-stu-id="b00c1-123">If your bot is replying to a user's message, always use the `ReplyToActivity` method.</span></span>

## <a name="send-a-non-reply-message"></a><span data-ttu-id="b00c1-124">Envío de un mensaje (de no respuesta)</span><span class="sxs-lookup"><span data-stu-id="b00c1-124">Send a (non-reply) message</span></span> 

<span data-ttu-id="b00c1-125">Si el bot es parte de una conversación, puede enviar un mensaje que no es una respuesta directa a ningún mensaje del usuario si llama al método `SendToConversation`.</span><span class="sxs-lookup"><span data-stu-id="b00c1-125">If your bot is part of a conversation, it can send a message that is not a direct reply to any message from the user by calling the `SendToConversation` method.</span></span> 

[!code-csharp[Send non-reply message](../includes/code/dotnet-send-and-receive.cs#sendNonReplyMessage)]

<span data-ttu-id="b00c1-126">Puede usar el método `CreateReply` para inicializar el mensaje nuevo (que establecería automáticamente las propiedades `Recipient`, `From` y `Conversation` del mensaje).</span><span class="sxs-lookup"><span data-stu-id="b00c1-126">You may use the `CreateReply` method to initialize the new message (which would automatically set the `Recipient`, `From`, and `Conversation` properties for the message).</span></span> <span data-ttu-id="b00c1-127">Como alternativa, puede usar el método `CreateMessageActivity` para crear el nuevo mensaje y establecer todos los valores de propiedad usted mismo.</span><span class="sxs-lookup"><span data-stu-id="b00c1-127">Alternatively, you could use the `CreateMessageActivity` method to create the new message and set all property values yourself.</span></span>

> [!NOTE]
> <span data-ttu-id="b00c1-128">Bot Framework no impone ninguna restricción en el número de mensajes que puede enviar un bot.</span><span class="sxs-lookup"><span data-stu-id="b00c1-128">The Bot Framework does not impose any restrictions on the number of messages that a bot may send.</span></span> <span data-ttu-id="b00c1-129">Sin embargo, la mayoría de los canales aplican valores de limitación para impedir que los bots envíen un gran número de mensajes en un breve período de tiempo.</span><span class="sxs-lookup"><span data-stu-id="b00c1-129">However, most channels enforce throttling limits to restrict bots from sending a large number of messages in a short period of time.</span></span> <span data-ttu-id="b00c1-130">Además, si el bot envía varios mensajes en una sucesión rápida, puede que el canal no siempre procese los mensajes en el orden correcto.</span><span class="sxs-lookup"><span data-stu-id="b00c1-130">Additionally, if the bot sends multiple messages in quick succession, the channel may not always render the messages in the proper sequence.</span></span>

## <a name="start-a-conversation"></a><span data-ttu-id="b00c1-131">Inicio de una conversación</span><span class="sxs-lookup"><span data-stu-id="b00c1-131">Start a conversation</span></span>

<span data-ttu-id="b00c1-132">Puede haber ocasiones en las que su bot debe iniciar una conversación con uno o varios usuarios.</span><span class="sxs-lookup"><span data-stu-id="b00c1-132">There may be times when your bot needs to initiate a conversation with one or more users.</span></span> <span data-ttu-id="b00c1-133">Puede iniciar una conversación llamando al método `CreateDirectConversation` (para una conversación privada con un solo usuario) o al método `CreateConversation` (para una conversación grupal con varios usuarios) para recuperar un objeto`ConversationAccount`.</span><span class="sxs-lookup"><span data-stu-id="b00c1-133">You can start a conversation by calling either the `CreateDirectConversation` method (for a private conversation with a single user) or the `CreateConversation` method (for a group conversation with multiple users) to retrieve a `ConversationAccount` object.</span></span> <span data-ttu-id="b00c1-134">Luego, cree el mensaje y envíelo mediante una llamada al método `SendToConversation`.</span><span class="sxs-lookup"><span data-stu-id="b00c1-134">Then, create the message and send it by calling the `SendToConversation` method.</span></span> <span data-ttu-id="b00c1-135">Para usar el método `CreateDirectConversation` o el método `CreateConversation`, primero debe [crear el cliente de conector](#create-a-connector-client) mediante el uso de la dirección URL de servicio del canal de destino (que podría recuperar desde la caché, si se conservó a partir de mensajes anteriores).</span><span class="sxs-lookup"><span data-stu-id="b00c1-135">To use either the `CreateDirectConversation` method or the `CreateConversation` method, you must first [create the connector client](#create-a-connector-client) by using the target channel's service URL (which you may retrieve from cache, if you've persisted it from previous messages).</span></span> 

> [!NOTE]
> <span data-ttu-id="b00c1-136">No todos los canales admiten conversaciones de grupo.</span><span class="sxs-lookup"><span data-stu-id="b00c1-136">Not all channels support group conversations.</span></span> <span data-ttu-id="b00c1-137">Para determinar si un canal admite las conversaciones grupales, consulte la documentación del canal.</span><span class="sxs-lookup"><span data-stu-id="b00c1-137">To determine whether a channel supports group conversations, consult the channel's documentation.</span></span>

<span data-ttu-id="b00c1-138">Este ejemplo de código utiliza el método `CreateDirectConversation` para crear una conversación privada con un solo usuario.</span><span class="sxs-lookup"><span data-stu-id="b00c1-138">This code example uses the `CreateDirectConversation` method to create a private conversation with a single user.</span></span>

[!code-csharp[Start private conversation](../includes/code/dotnet-send-and-receive.cs#startPrivateConversation)]

<span data-ttu-id="b00c1-139">Este ejemplo de código utiliza el método `CreateConversation` para crear una conversación grupal con varios usuarios.</span><span class="sxs-lookup"><span data-stu-id="b00c1-139">This code example uses the `CreateConversation` method to create a group conversation with multiple users.</span></span>

[!code-csharp[Start group conversation](../includes/code/dotnet-send-and-receive.cs#startGroupConversation)]

## <a name="additional-resources"></a><span data-ttu-id="b00c1-140">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="b00c1-140">Additional resources</span></span>

- <span data-ttu-id="b00c1-141">[Activities overview](bot-builder-dotnet-activities.md) (Introducción a las actividades)</span><span class="sxs-lookup"><span data-stu-id="b00c1-141">[Activities overview](bot-builder-dotnet-activities.md)</span></span>
- [<span data-ttu-id="b00c1-142">Creación de mensajes</span><span class="sxs-lookup"><span data-stu-id="b00c1-142">Create messages</span></span>](bot-builder-dotnet-create-messages.md)
- <span data-ttu-id="b00c1-143"><a href="/dotnet/api/?view=botbuilder-3.11.0" target="_blank">Referencia del SDK de Bot Builder para .NET</a></span><span class="sxs-lookup"><span data-stu-id="b00c1-143"><a href="/dotnet/api/?view=botbuilder-3.11.0" target="_blank">Bot Builder SDK for .NET Reference</a></span></span>
- <span data-ttu-id="b00c1-144"><a href="https://docs.botframework.com/en-us/csharp/builder/sdkreference/dc/d2f/class_microsoft_1_1_bot_1_1_connector_1_1_activity.html" target="_blank">Clase Activity</a></span><span class="sxs-lookup"><span data-stu-id="b00c1-144"><a href="https://docs.botframework.com/en-us/csharp/builder/sdkreference/dc/d2f/class_microsoft_1_1_bot_1_1_connector_1_1_activity.html" target="_blank">Activity class</a></span></span>
- <span data-ttu-id="b00c1-145"><a href="/dotnet/api/microsoft.bot.connector.connectorclient" target="_blank">Clase ConnectorClient</a></span><span class="sxs-lookup"><span data-stu-id="b00c1-145"><a href="/dotnet/api/microsoft.bot.connector.connectorclient" target="_blank">ConnectorClient class</a></span></span>

[ConnectorClient]: /dotnet/api/microsoft.bot.connector.connectorclient
