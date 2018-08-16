---
title: Envío de un indicador de escritura | Microsoft Docs
description: Aprenda a agregar un indicador "Espere" para indicar a un usuario que un bot está procesando una solicitud mediante Bot Builder SDK para Node.js
author: DeniseMak
ms.author: v-demak
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: aff2509a426fb42f136fb9d2b4a2df9ec1accda0
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39306117"
---
# <a name="send-a-typing-indicator"></a><span data-ttu-id="872d3-103">Envío de un indicador de escritura</span><span class="sxs-lookup"><span data-stu-id="872d3-103">Send a typing indicator</span></span> 


<span data-ttu-id="872d3-104">Los usuarios esperan una respuesta a tiempo a sus mensajes.</span><span class="sxs-lookup"><span data-stu-id="872d3-104">Users expect a timely response to their messages.</span></span> <span data-ttu-id="872d3-105">Si el bot realiza alguna tarea de larga ejecución como llamar a un servidor o ejecutar una consulta sin proporcionar al usuario ninguna indicación de que el bot le ha escuchado, el usuario podría impacientarse o simplemente suponer que el bot no funciona.</span><span class="sxs-lookup"><span data-stu-id="872d3-105">If your bot performs some long-running task like calling a server or executing a query without giving the user some indication that the bot heard them, the user could get impatient and send additional messages or just assume the bot is broken.</span></span>
<span data-ttu-id="872d3-106">Muchos canales admiten el envío de una indicación de escritura para mostrar al usuario que el mensaje se recibió y que se está procesando.</span><span class="sxs-lookup"><span data-stu-id="872d3-106">Many channels support the sending of a typing indication to show the user that the message was received and is being processed.</span></span>


## <a name="typing-indicator-example"></a><span data-ttu-id="872d3-107">Ejemplo de indicador de escritura</span><span class="sxs-lookup"><span data-stu-id="872d3-107">Typing indicator example</span></span>

<span data-ttu-id="872d3-108">En el ejemplo siguiente se muestra cómo enviar una indicación de escritura mediante [session.sendTyping()][SendTyping].</span><span class="sxs-lookup"><span data-stu-id="872d3-108">The following example demonstrates how to send a typing indication using [session.sendTyping()][SendTyping].</span></span>  <span data-ttu-id="872d3-109">Puede probar esto con el emulador de Bot Framework.</span><span class="sxs-lookup"><span data-stu-id="872d3-109">You can test this with the Bot Framework Emulator.</span></span>


```javascript

// Create bot and default message handler
var bot = new builder.UniversalBot(connector, function (session) {
    session.sendTyping();
    setTimeout(function () {
        session.send("Hello there...");
    }, 3000);
});
```

<span data-ttu-id="872d3-110">Los indicadores de escritura también son útiles cuando se inserta un retraso de mensaje para evitar que los mensajes que contienen imágenes se envíen desordenados.</span><span class="sxs-lookup"><span data-stu-id="872d3-110">Typing indicators are also useful when inserting a message delay to prevent messages that contain images from being sent out of order.</span></span>

<span data-ttu-id="872d3-111">Para más información, consulte [How to send a rich card](bot-builder-nodejs-send-rich-cards.md) (Envío de una tarjeta enriquecida).</span><span class="sxs-lookup"><span data-stu-id="872d3-111">To learn more, see [How to send a rich card](bot-builder-nodejs-send-rich-cards.md).</span></span>


## <a name="additional-resources"></a><span data-ttu-id="872d3-112">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="872d3-112">Additional resources</span></span>

* <span data-ttu-id="872d3-113">[sendTyping][SendTyping]</span><span class="sxs-lookup"><span data-stu-id="872d3-113">[sendTyping][SendTyping]</span></span>


[SendTyping]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.session#sendtyping
[IMessage]: http://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.imessage