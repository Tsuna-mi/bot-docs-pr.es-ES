---
title: Creación de mensajes | Microsoft Docs
description: Obtenga información sobre cómo crear mensajes con el SDK de Bot Builder para Node.js.
author: v-ducvo
ms.author: v-ducvo
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 09/7/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: e7dfb72f69202011c4fda06c3d55e0afa8d3d045
ms.sourcegitcommit: dcbc8ad992a3e242a11ebcdf0ee99714d919a877
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/30/2018
ms.locfileid: "39352914"
---
# <a name="create-messages"></a><span data-ttu-id="0cb72-103">Creación de mensajes</span><span class="sxs-lookup"><span data-stu-id="0cb72-103">Create messages</span></span>
<span data-ttu-id="0cb72-104">La comunicación entre el bot y el usuario se realiza a través de mensajes.</span><span class="sxs-lookup"><span data-stu-id="0cb72-104">Communication between the bot and the user is through messages.</span></span> <span data-ttu-id="0cb72-105">El bot enviará actividades de mensajes para comunicar información a los usuarios y, del mismo modo, recibirá actividades de mensajes de los usuarios.</span><span class="sxs-lookup"><span data-stu-id="0cb72-105">Your bot will send message activities to communicate information to users, and likewise, will also receive message activities from users.</span></span> <span data-ttu-id="0cb72-106">Algunos mensajes pueden constar simplemente de texto sin formato, mientras que otros pueden incluir contenido más enriquecido, como texto que se va a decir, acciones sugeridas, datos adjuntos multimedia, tarjetas enriquecidas y datos específicos del canal.</span><span class="sxs-lookup"><span data-stu-id="0cb72-106">Some messages may simply consist of plain text, while others may contain richer content such as text to be spoken, suggested actions, media attachments, rich cards, and channel-specific data.</span></span>

<span data-ttu-id="0cb72-107">En este artículo se describen algunos de los métodos de mensaje usados con frecuencia que puede utilizar para mejorar la experiencia del usuario.</span><span class="sxs-lookup"><span data-stu-id="0cb72-107">This article describes some of the commonly-used message methods you can use to enhance your user experience.</span></span>

## <a name="default-message-handler"></a><span data-ttu-id="0cb72-108">Controlador de mensajes predeterminado</span><span class="sxs-lookup"><span data-stu-id="0cb72-108">Default message handler</span></span>

<span data-ttu-id="0cb72-109">El SDK de Bot Builder para Node.js incluye un controlador de mensajes predeterminado integrado en el objeto [`session`](https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.session.html).</span><span class="sxs-lookup"><span data-stu-id="0cb72-109">The Bot Builder SDK for Node.js comes with a default message handler built into the [`session`](https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.session.html) object.</span></span> <span data-ttu-id="0cb72-110">Este controlador de mensajes permite enviar y recibir mensajes de texto entre el bot y el usuario.</span><span class="sxs-lookup"><span data-stu-id="0cb72-110">This message handler allows you to send and receive text messages between the bot and the user.</span></span>

### <a name="send-a-text-message"></a><span data-ttu-id="0cb72-111">Enviar un mensaje de texto</span><span class="sxs-lookup"><span data-stu-id="0cb72-111">Send a text message</span></span>

<span data-ttu-id="0cb72-112">El envío de un mensaje de texto mediante el controlador de mensajes predeterminado es sencillo, simplemente llame a `session.send` y pase una **cadena**.</span><span class="sxs-lookup"><span data-stu-id="0cb72-112">Sending a text message using the default message handler is simple, just call `session.send` and pass in a **string**.</span></span>

<span data-ttu-id="0cb72-113">En este ejemplo se muestra cómo se puede enviar un mensaje de texto para saludar al usuario.</span><span class="sxs-lookup"><span data-stu-id="0cb72-113">This sample shows how you can send a text message to greet the user.</span></span>
```javascript
session.send("Good morning.");
```

<span data-ttu-id="0cb72-114">En este ejemplo se muestra cómo se puede enviar un mensaje de texto mediante la plantilla de cadena de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="0cb72-114">This sample shows how you can send a text message using JavaScript string template.</span></span>
```javascript
var msg = `You ordered: ${order.Description} for a total of $${order.Price}.`;
session.send(msg); //msg: "You ordered: Potato Salad for a total of $5.99."
```

### <a name="receive-a-text-message"></a><span data-ttu-id="0cb72-115">Recibir un mensaje de texto</span><span class="sxs-lookup"><span data-stu-id="0cb72-115">Receive a text message</span></span>

<span data-ttu-id="0cb72-116">Cuando un usuario envía un mensaje al bot, el bot recibe el mensaje a través de la propiedad `session.message`.</span><span class="sxs-lookup"><span data-stu-id="0cb72-116">When a user sends the bot a message, the bot receives the message through the `session.message` property.</span></span>

<span data-ttu-id="0cb72-117">En este ejemplo se muestra cómo acceder al mensaje del usuario.</span><span class="sxs-lookup"><span data-stu-id="0cb72-117">This sample shows how to access the user's message.</span></span>
```javascript
var userMessage = session.message.text;
```

## <a name="customizing-a-message"></a><span data-ttu-id="0cb72-118">Personalización de un mensaje</span><span class="sxs-lookup"><span data-stu-id="0cb72-118">Customizing a message</span></span>

<span data-ttu-id="0cb72-119">Para tener más control sobre el formato del texto de los mensajes, puede crear un objeto [`message`](https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.message.html) personalizado y establecer las propiedades necesarias antes de enviarlo al usuario.</span><span class="sxs-lookup"><span data-stu-id="0cb72-119">To have more control over the text formatting of your messages, you can create a custom [`message`](https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.message.html) object and set the properties necessary before sending it to the user.</span></span>

<span data-ttu-id="0cb72-120">En este ejemplo se muestra cómo crear un objeto `message` personalizado y establecer las propiedades `text`, `textFormat` y `textLocale`.</span><span class="sxs-lookup"><span data-stu-id="0cb72-120">This sample shows how to create a custom `message` object and set the `text`, `textFormat`, and `textLocale` properties.</span></span>

```javascript
var customMessage = new builder.Message(session)
    .text("Hello!")
    .textFormat("plain")
    .textLocale("en-us");
session.send(customMessage);
```

<span data-ttu-id="0cb72-121">En casos donde el objeto `session` no está en el ámbito, se puede usar el método `bot.send` para enviar un mensaje con formato al usuario.</span><span class="sxs-lookup"><span data-stu-id="0cb72-121">In cases where you do not have the `session` object in scope, you can use `bot.send` method to send a formatted message to the user.</span></span>

<span data-ttu-id="0cb72-122">La propiedad `textFormat` de un mensaje se puede usar para especificar el formato del texto.</span><span class="sxs-lookup"><span data-stu-id="0cb72-122">The `textFormat` property of a message can be used to specify the format of the text.</span></span> <span data-ttu-id="0cb72-123">La propiedad `textFormat` se puede establecer en **plain**, **markdown** o **xml**.</span><span class="sxs-lookup"><span data-stu-id="0cb72-123">The `textFormat` property can be set to **plain**, **markdown**, or **xml**.</span></span> <span data-ttu-id="0cb72-124">El valor predeterminado de `textFormat` es **markdown**.</span><span class="sxs-lookup"><span data-stu-id="0cb72-124">The default value for `textFormat` is **markdown**.</span></span> 

<span data-ttu-id="0cb72-125">Para obtener una lista del formato de texto normalmente compatible, vea [Text formatting](../bot-service-channel-inspector.md#text-formatting) (Formato de texto).</span><span class="sxs-lookup"><span data-stu-id="0cb72-125">For a list of commonly supported text formatting, see [Text formatting](../bot-service-channel-inspector.md#text-formatting).</span></span> <span data-ttu-id="0cb72-126">Para asegurarse de que las características que quiere usar son compatibles con el canal de destino, obtenga una vista previa de las características mediante el [Inspector de canales](../bot-service-channel-inspector.md).</span><span class="sxs-lookup"><span data-stu-id="0cb72-126">To ensure that the feature(s) you want to use is supported by the target channel, preview the feature(s) using the [Channel Inspector](../bot-service-channel-inspector.md).</span></span>

## <a name="message-property"></a><span data-ttu-id="0cb72-127">Propiedad de mensajes</span><span class="sxs-lookup"><span data-stu-id="0cb72-127">Message property</span></span>

<span data-ttu-id="0cb72-128">El objeto [`Message`](https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.message.html) tiene una propiedad **data** interna que usa para administrar el mensaje que se envía.</span><span class="sxs-lookup"><span data-stu-id="0cb72-128">The [`Message`](https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.message.html) object has an internal **data** property that it uses to manage the message being sent.</span></span> <span data-ttu-id="0cb72-129">El resto de propiedades se establecen a través de los distintos métodos que expone este objeto.</span><span class="sxs-lookup"><span data-stu-id="0cb72-129">Other properties you set are through the different methods this object expose to you.</span></span> 

## <a name="message-methods"></a><span data-ttu-id="0cb72-130">Métodos de mensaje</span><span class="sxs-lookup"><span data-stu-id="0cb72-130">Message methods</span></span>

<span data-ttu-id="0cb72-131">Las propiedades de mensaje se establecen y recuperan a través de los métodos del objeto.</span><span class="sxs-lookup"><span data-stu-id="0cb72-131">Message properties are set and retrieved through the object's methods.</span></span> <span data-ttu-id="0cb72-132">En la tabla siguiente se proporciona una lista de los métodos que se pueden llamar para establecer y obtener las diferentes propiedades **Message**.</span><span class="sxs-lookup"><span data-stu-id="0cb72-132">The table below provide a list of the methods you can call to set/get the different **Message** properties.</span></span>

| <span data-ttu-id="0cb72-133">Método</span><span class="sxs-lookup"><span data-stu-id="0cb72-133">Method</span></span> | <span data-ttu-id="0cb72-134">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="0cb72-134">Description</span></span> |
| ---- | ---- | 
| [`addAttachment(attachment:AttachmentType)`](https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.message.html#addattachment) | <span data-ttu-id="0cb72-135">Agrega datos adjuntos a un mensaje</span><span class="sxs-lookup"><span data-stu-id="0cb72-135">Adds an attachment to a message</span></span>|
| [`addEntity(obj:Object)`](https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.message.html#addentity) | <span data-ttu-id="0cb72-136">Agrega una entidad al mensaje.</span><span class="sxs-lookup"><span data-stu-id="0cb72-136">Adds an entity to the message.</span></span> |
| [`address(adr:IAddress)`](https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.message.html#address) | <span data-ttu-id="0cb72-137">Información de enrutamiento de direcciones para el mensaje.</span><span class="sxs-lookup"><span data-stu-id="0cb72-137">Address routing information for the message.</span></span> <span data-ttu-id="0cb72-138">Para enviar un mensaje automático al usuario, guarde la dirección del mensaje en el contenedor userData.</span><span class="sxs-lookup"><span data-stu-id="0cb72-138">To send user a proactive message, save the message's address in the userData bag.</span></span> |
| [`attachmentLayout(style:string)`](https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.message.html#attachmentlayout) | <span data-ttu-id="0cb72-139">Sugerencia sobre cómo deben diseñar los clientes varios datos adjuntos.</span><span class="sxs-lookup"><span data-stu-id="0cb72-139">Hint for how clients should layout multiple attachments.</span></span> <span data-ttu-id="0cb72-140">El valor predeterminado es "list".</span><span class="sxs-lookup"><span data-stu-id="0cb72-140">The default value is 'list'.</span></span> |
| [`attachments(list:AttachmentType)`](https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.message.html#attachments) | <span data-ttu-id="0cb72-141">Una lista de tarjetas o imágenes para enviar al usuario.</span><span class="sxs-lookup"><span data-stu-id="0cb72-141">A list of cards or images to send to the user.</span></span> |
| [`compose(prompts:string[], ...args:any[])`](https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.message.html#compose) | <span data-ttu-id="0cb72-142">Crea una respuesta compleja y aleatoria para el usuario.</span><span class="sxs-lookup"><span data-stu-id="0cb72-142">Composes a complex and randomized reply to the user.</span></span> |
| [`entities(list:Object[])`](https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.message.html#entities) | <span data-ttu-id="0cb72-143">Objetos estructurados que se pasan al bot o al usuario.</span><span class="sxs-lookup"><span data-stu-id="0cb72-143">Structured objects passed to the bot or user.</span></span> |
| [`inputHint(hint:string)`](https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.message.html#inputhint) | <span data-ttu-id="0cb72-144">Sugerencia enviada al usuario para informarle de si el bot está esperando una entrada adicional o no.</span><span class="sxs-lookup"><span data-stu-id="0cb72-144">Hint sent to user letting them know if the bot is expecting further input or not.</span></span> <span data-ttu-id="0cb72-145">Las solicitudes integradas rellenarán automáticamente este valor para los mensajes salientes.</span><span class="sxs-lookup"><span data-stu-id="0cb72-145">The built-in prompts will automatically populate this value for outgoing messages.</span></span> |
| [`localTimeStamp((optional)time:string)`](https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.message.html#localtimestamp) | <span data-ttu-id="0cb72-146">Hora local del envío del mensaje (establecida por el cliente o el bot, por ejemplo: 2016-09-23T13:07:49.4714686-07:00).</span><span class="sxs-lookup"><span data-stu-id="0cb72-146">Local time when message was sent (set by client or bot, Ex: 2016-09-23T13:07:49.4714686-07:00.)</span></span> |
| [`originalEvent(event:any)`](https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.message.html#originalevent) | <span data-ttu-id="0cb72-147">Mensaje en formato original o nativo del canal para los mensajes entrantes.</span><span class="sxs-lookup"><span data-stu-id="0cb72-147">Message in original/native format of the channel for incoming messages.</span></span> |
| [`sourceEvent(map:ISourceEventMap)`](https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.message.html#sourceevent) | <span data-ttu-id="0cb72-148">Para los mensajes salientes se puede usar para pasar datos de eventos específicos de origen como datos adjuntos personalizados.</span><span class="sxs-lookup"><span data-stu-id="0cb72-148">For outgoing messages can be used to pass source specific event data like custom attachments.</span></span> |
| [`speak(ssml:TextType, ...args:any[])`](https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.message.html#speak) | <span data-ttu-id="0cb72-149">Establece el campo de voz del mensaje como *Lenguaje de marcado de síntesis de voz (SSML)*.</span><span class="sxs-lookup"><span data-stu-id="0cb72-149">Sets the speak field of the message as *Speech Synthesis Markup Language (SSML)*.</span></span> <span data-ttu-id="0cb72-150">Esto se proporcionará de forma hablada al usuario en los dispositivos compatibles.</span><span class="sxs-lookup"><span data-stu-id="0cb72-150">This will be spoken to the user on supported devices.</span></span> |
| [<span data-ttu-id="0cb72-151">`suggestedActions(suggestions:ISuggestedActions `&#124;` IIsSuggestedActions)`</span><span class="sxs-lookup"><span data-stu-id="0cb72-151">`suggestedActions(suggestions:ISuggestedActions `&#124;` IIsSuggestedActions)`</span></span>](https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.message.html#suggestedactions) | <span data-ttu-id="0cb72-152">Acciones sugeridas opcionales para enviar al usuario.</span><span class="sxs-lookup"><span data-stu-id="0cb72-152">Optional suggested actions to send to the user.</span></span> <span data-ttu-id="0cb72-153">Las acciones sugeridas solo se mostrarán en los canales que admiten acciones sugeridas.</span><span class="sxs-lookup"><span data-stu-id="0cb72-153">Suggested actions will be displayed only on the channels that support suggested actions.</span></span> |
| [`summary(text:TextType, ...argus:any[])`](https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.message.html#summary) | <span data-ttu-id="0cb72-154">El texto se mostrará como reserva y como una descripción breve del contenido del mensaje (por ejemplo: lista de conversaciones recientes).</span><span class="sxs-lookup"><span data-stu-id="0cb72-154">Text to be displayed as fall-back and as short description of the message content in (e.g.: List of recent conversations.)</span></span> |
| [`text(text:TextType, ...args:any[])`](https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.message.html#text) | <span data-ttu-id="0cb72-155">Establece el texto del mensaje.</span><span class="sxs-lookup"><span data-stu-id="0cb72-155">Sets the message text.</span></span> |
| [`textFormat(style:string)`](https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.message.html#textformat) | <span data-ttu-id="0cb72-156">Establece el formato de texto.</span><span class="sxs-lookup"><span data-stu-id="0cb72-156">Set the text format.</span></span> <span data-ttu-id="0cb72-157">El formato predeterminado es **markdown**.</span><span class="sxs-lookup"><span data-stu-id="0cb72-157">Default format is **markdown**.</span></span> |
| [`textLocale(locale:string)`](https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.message.html#textlocale) | <span data-ttu-id="0cb72-158">Establece el idioma de destino del mensaje.</span><span class="sxs-lookup"><span data-stu-id="0cb72-158">Set the target language of the message.</span></span> |
| [`toMessage()`](https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.message.html#tomessage) | <span data-ttu-id="0cb72-159">Obtiene el código JSON para el mensaje.</span><span class="sxs-lookup"><span data-stu-id="0cb72-159">Gets the JSON for the message.</span></span> |
| [`composePrompt(session:Session, prompts:string[], args?:any[])`](https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.message.html#composeprompt-1) | <span data-ttu-id="0cb72-160">Combina una matriz de mensajes en un único mensaje localizado y, después, rellena las ranuras de plantilla de mensajes de forma opcional con los argumentos que se pasan.</span><span class="sxs-lookup"><span data-stu-id="0cb72-160">Combines an array of prompts into a single localized prompt and then optionally fills the prompts template slots with the passed in arguments.</span></span> |
| [`randomPrompt(prompts:TextType)`](https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.message.html#randomprompt) | <span data-ttu-id="0cb72-161">Obtiene un mensaje aleatorio de la matriz de \**prompts* que se pasa.</span><span class="sxs-lookup"><span data-stu-id="0cb72-161">Gets a random prompt from the array of \**prompts* that is passed in.</span></span> |

## <a name="next-step"></a><span data-ttu-id="0cb72-162">Paso siguiente</span><span class="sxs-lookup"><span data-stu-id="0cb72-162">Next step</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="0cb72-163">Envío y recepción de archivos adjuntos</span><span class="sxs-lookup"><span data-stu-id="0cb72-163">Send and receive attachments</span></span>](bot-builder-nodejs-send-receive-attachments.md)

