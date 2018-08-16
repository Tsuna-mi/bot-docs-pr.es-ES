---
title: Envío de mensajes automáticos | Microsoft Docs
description: Aprenda a interrumpir el flujo de la conversación actual con un mensaje automático mediante Bot Builder SDK para Node.js
author: v-ducvo
ms.author: v-ducvo
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: f432a570f5a8393a2aef3e4ec97c5e7e8cbaa43f
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305804"
---
# <a name="send-proactive-messages"></a><span data-ttu-id="1a7b2-103">Envío de mensajes automáticos</span><span class="sxs-lookup"><span data-stu-id="1a7b2-103">Send proactive messages</span></span>
> [!div class="op_single_selector"]
> - [.NET](../dotnet/bot-builder-dotnet-proactive-messages.md)
> - [Node.js](../nodejs/bot-builder-nodejs-proactive-messages.md)

[!INCLUDE [Introduction to proactive messages - part 1](../includes/snippet-proactive-messages-intro-1.md)]

## <a name="types-of-proactive-messages"></a><span data-ttu-id="1a7b2-106">Tipos de mensajes automáticos</span><span class="sxs-lookup"><span data-stu-id="1a7b2-106">Types of proactive messages</span></span>

[!INCLUDE [Introduction to proactive messages - part 2](../includes/snippet-proactive-messages-intro-2.md)]

## <a name="send-an-ad-hoc-proactive-message"></a><span data-ttu-id="1a7b2-107">Envío de un mensaje automático ad hoc</span><span class="sxs-lookup"><span data-stu-id="1a7b2-107">Send an ad hoc proactive message</span></span>

<span data-ttu-id="1a7b2-108">Los ejemplos de código siguientes muestran cómo enviar un mensaje automático ad hoc con Bot Builder SDK para Node.js.</span><span class="sxs-lookup"><span data-stu-id="1a7b2-108">The following code samples show how to send an ad hoc proactive message by using the Bot Builder SDK for Node.js.</span></span>

<span data-ttu-id="1a7b2-109">Para poder enviar un mensaje ad hoc a un usuario, en primer lugar, el bot debe recopilar y almacenar información sobre el usuario de la conversación actual.</span><span class="sxs-lookup"><span data-stu-id="1a7b2-109">To be able to send an ad hoc message to a user, the bot must first collect and save information about the user from the current conversation.</span></span> <span data-ttu-id="1a7b2-110">La propiedad **dirección** del mensaje incluye toda la información que el bot necesitará para enviar un mensaje ad hoc al usuario más adelante.</span><span class="sxs-lookup"><span data-stu-id="1a7b2-110">The **address** property of the message includes all of the information that the bot will need to send an ad hoc message to the user later.</span></span> 

```javascript
bot.dialog('adhocDialog', function(session, args) {
    var savedAddress = session.message.address;

    // (Save this information somewhere that it can be accessed later, such as in a database, or session.userData)
    session.userData.savedAddress = savedAddress;

    var message = 'Hello user, good to meet you! I now know your address and can send you notifications in the future.';
    session.send(message);
})
```

> [!NOTE]
> El bot puede almacenar los datos de usuario de cualquier manera, siempre que pueda acceder a ellos más adelante.

<span data-ttu-id="1a7b2-112">Una vez que el bot ha recopilado la información sobre el usuario, puede enviar un mensaje automático ad hoc al usuario en cualquier momento.</span><span class="sxs-lookup"><span data-stu-id="1a7b2-112">After the bot has collected information about the user, it can send an ad hoc proactive message to the user at any time.</span></span> <span data-ttu-id="1a7b2-113">Para ello, simplemente recupera los datos de usuario que almacenó previamente, construye el mensaje y lo envía.</span><span class="sxs-lookup"><span data-stu-id="1a7b2-113">To do so, it simply retrieves the user data that it stored previously, constructs the message, and sends it.</span></span>

```javascript
var inMemoryStorage = new builder.MemoryBotStorage();

var bot = new builder.UniversalBot(connector)
                .set('storage', inMemoryStorage); // Register in-memory storage 

function sendProactiveMessage(address) {
    var msg = new builder.Message().address(address);
    msg.text('Hello, this is a notification');
    msg.textLocale('en-US');
    bot.send(msg);
}
```

> [!TIP]
> Un mensaje ad hoc automático se puede iniciar del mismo modo que desde activadores asincrónicos, como solicitudes HTTP, temporizadores, colas o desde cualquier otro lugar que el desarrollador elija.

## <a name="send-a-dialog-based-proactive-message"></a><span data-ttu-id="1a7b2-115">Envío de un mensaje automático basado en diálogos</span><span class="sxs-lookup"><span data-stu-id="1a7b2-115">Send a dialog-based proactive message</span></span>

<span data-ttu-id="1a7b2-116">Los ejemplos de código siguientes muestran cómo enviar un mensaje automático basado en diálogos con Bot Builder SDK para Node.js.</span><span class="sxs-lookup"><span data-stu-id="1a7b2-116">The following code samples show how to send a dialog-based proactive message by using the Bot Builder SDK for Node.js.</span></span> <span data-ttu-id="1a7b2-117">Puede encontrar el ejemplo completo en la carpeta [Microsoft/BotBuilder-Samples/nodo/core-proactiveMessages/startNewDialog](https://github.com/Microsoft/BotBuilder-Samples/tree/master/Node/core-proactiveMessages/startNewDialog).</span><span class="sxs-lookup"><span data-stu-id="1a7b2-117">You can find the complete working example in the [Microsoft/BotBuilder-Samples/Node/core-proactiveMessages/startNewDialog](https://github.com/Microsoft/BotBuilder-Samples/tree/master/Node/core-proactiveMessages/startNewDialog) folder.</span></span>

<span data-ttu-id="1a7b2-118">Para poder enviar un mensaje basado en diálogos a un usuario, en primer lugar el bot debe recopilar y guardar información de la conversación actual.</span><span class="sxs-lookup"><span data-stu-id="1a7b2-118">To be able to send a dialog-based message to a user, the bot must first collect (and save) information from the current conversation.</span></span> <span data-ttu-id="1a7b2-119">El objeto `session.message.address` incluye toda la información que el bot necesitará para enviar un mensaje automático basado en diálogos al usuario.</span><span class="sxs-lookup"><span data-stu-id="1a7b2-119">The `session.message.address` object includes all of the information that the bot will need to send a dialog-based proactive message to the user.</span></span> 

```javascript
// proactiveDialog dialog
bot.dialog('proactiveDialog', function (session, args) {

    savedAddress = session.message.address;

    var message = 'Hey there, I\'m going to interrupt our conversation and start a survey in five seconds...';
    session.send(message);

    message = `You can also make me send a message by accessing: http://localhost:${server.address().port}/api/CustomWebApi`;
    session.send(message);

    setTimeout(() => {
        startProactiveDialog(savedAddress);
    }, 5000);
});
```

<span data-ttu-id="1a7b2-120">Cuando llega el momento de enviar el mensaje, el bot crea un diálogo y lo agrega a la parte superior de la pila del diálogo.</span><span class="sxs-lookup"><span data-stu-id="1a7b2-120">When it is time to send the message, the bot creates a new dialog and adds it to the top of the dialog stack.</span></span> <span data-ttu-id="1a7b2-121">El nuevo diálogo controla la conversación, entrega el mensaje automático, lo cierra y luego devuelve el control al diálogo anterior de la pila.</span><span class="sxs-lookup"><span data-stu-id="1a7b2-121">The new dialog takes control of the conversation, delivers the proactive message, closes, and then returns control to the previous dialog in the stack.</span></span> 

```javascript
// initiate a dialog proactively 
function startProactiveDialog(address) {
    bot.beginDialog(address, "*:survey");
}
```

> [!NOTE]
> El ejemplo de código anterior requiere un archivo personalizado, **botadapter.js**, que puede [descargar desde GitHub](https://github.com/Microsoft/BotBuilder-Samples/blob/master/Node/core-proactiveMessages/startNewDialog/botadapter.js).

<span data-ttu-id="1a7b2-123">El diálogo de encuesta controla la conversación hasta que termina.</span><span class="sxs-lookup"><span data-stu-id="1a7b2-123">The survey dialog controls the conversation until it finishes.</span></span> <span data-ttu-id="1a7b2-124">A continuación, se cierra (mediante una llamada a `session.endDialog()`), con lo que el control se devuelve al diálogo anterior.</span><span class="sxs-lookup"><span data-stu-id="1a7b2-124">Then, it closes (by calling `session.endDialog()`), thereby returning control back to the previous dialog.</span></span> 


```javascript
// handle the proactive initiated dialog
bot.dialog('survey', function (session, args, next) {
  if (session.message.text === "done") {
    session.send("Great, back to the original conversation");
    session.endDialog();
  } else {
    session.send('Hello, I\'m the survey dialog. I\'m interrupting your conversation to ask you a question. Type "done" to resume');
  }
});
```

## <a name="sample-code"></a><span data-ttu-id="1a7b2-125">Código de ejemplo</span><span class="sxs-lookup"><span data-stu-id="1a7b2-125">Sample code</span></span>

<span data-ttu-id="1a7b2-126">Para obtener un ejemplo completo en el que se muestre cómo enviar mensajes automáticos con Bot Builder SDK para Node.js, consulte el <a href="https://github.com/Microsoft/BotBuilder-Samples/tree/master/Node/core-proactiveMessages" target="_blank">ejemplo de mensajes automáticos</a> en GitHub.</span><span class="sxs-lookup"><span data-stu-id="1a7b2-126">For a complete sample that shows how to send proactive messages using the Bot Builder SDK for Node.js, see the <a href="https://github.com/Microsoft/BotBuilder-Samples/tree/master/Node/core-proactiveMessages" target="_blank">Proactive Messages sample</a> in GitHub.</span></span> <span data-ttu-id="1a7b2-127">En el ejemplo de mensajes automáticos, <a href="https://github.com/Microsoft/BotBuilder-Samples/tree/master/Node/core-proactiveMessages/simpleSendMessage" target="_blank">simpleSendMessage</a> muestra cómo enviar un mensaje automático ad hoc y <a href="https://github.com/Microsoft/BotBuilder-Samples/tree/master/Node/core-proactiveMessages/startNewDialog" target="_blank">startNewDialog</a> muestra cómo enviar un mensaje automático basado en diálogos.</span><span class="sxs-lookup"><span data-stu-id="1a7b2-127">Within the Proactive Messages sample, <a href="https://github.com/Microsoft/BotBuilder-Samples/tree/master/Node/core-proactiveMessages/simpleSendMessage" target="_blank">simpleSendMessage</a> shows how to send an ad-hoc proactive message and <a href="https://github.com/Microsoft/BotBuilder-Samples/tree/master/Node/core-proactiveMessages/startNewDialog" target="_blank">startNewDialog</a> shows how to send a dialog-based proactive message.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1a7b2-128">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="1a7b2-128">Additional resources</span></span>

- [<span data-ttu-id="1a7b2-129">Diseño del flujo de conversación</span><span class="sxs-lookup"><span data-stu-id="1a7b2-129">Designing conversation flow</span></span>](../bot-service-design-conversation-flow.md)
