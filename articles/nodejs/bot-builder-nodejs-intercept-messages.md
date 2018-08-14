---
title: Interceptación de mensajes | Microsoft Docs
description: Aprenda a crear registros mediante la interceptación y el procesamiento de intercambios de información con el SDK de Bot Builder para Node.js.
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: 9160e81f9086f88f808b41e8eb01745776e0fc9f
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305289"
---
# <a name="intercept-messages"></a><span data-ttu-id="10245-103">Interceptación de mensajes</span><span class="sxs-lookup"><span data-stu-id="10245-103">Intercept messages</span></span>
> [!div class="op_single_selector"]
> - [.NET](../dotnet/bot-builder-dotnet-middleware.md)
> - [Node.js](../nodejs/bot-builder-nodejs-intercept-messages.md)

[!INCLUDE [Introduction to message logging](../includes/snippet-message-logging-intro.md)]

## <a name="example"></a><span data-ttu-id="10245-106">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="10245-106">Example</span></span>

<span data-ttu-id="10245-107">En el ejemplo de código siguiente se muestra cómo interceptar los mensajes intercambiados entre un usuario y un bot por medio del concepto de **middleware** en el SDK de Bot Builder para Node.js.</span><span class="sxs-lookup"><span data-stu-id="10245-107">The following code sample shows how to intercept messages that are exchanged between user and bot by using the concept of **middleware** in the Bot Builder SDK for Node.js.</span></span> 

<span data-ttu-id="10245-108">En primer lugar, configure el controlador de mensajes entrantes (`botbuilder`) y mensajes salientes (`send`).</span><span class="sxs-lookup"><span data-stu-id="10245-108">First, configure the handler for incoming messages (`botbuilder`) and for outgoing messages (`send`).</span></span>

```javascript
server.post('/api/messages', connector.listen());
var bot = new builder.UniversalBot(connector);
bot.use({
    botbuilder: function (session, next) {
        myMiddleware.logIncomingMessage(session, next);
    },
    send: function (event, next) {
        myMiddleware.logOutgoingMessage(event, next);
    }
})
```

<span data-ttu-id="10245-109">A continuación, implemente cada uno de los controladores para definir la acción que se realizará con cada mensaje que se intercepte.</span><span class="sxs-lookup"><span data-stu-id="10245-109">Then, implement each of the handlers to define the action to take for each message that is intercepted.</span></span>

```javascript
module.exports = {
    logIncomingMessage: function (session, next) {
        console.log(session.message.text);
        next();
    },
    logOutgoingMessage: function (event, next) {
        console.log(event.text);
        next();
    }
}
```

<span data-ttu-id="10245-110">Ahora, cada mensaje entrante (del usuario al bot) desencadenará `logIncomingMessage`, y cada mensaje saliente (del bot al usuario) desencadenará `logOutgoingMessage`.</span><span class="sxs-lookup"><span data-stu-id="10245-110">Now, every inbound message (from user to bot) will trigger `logIncomingMessage`, and every outbound message (from bot to user) will trigger `logOutgoingMessage`.</span></span>
<span data-ttu-id="10245-111">En este ejemplo, el bot simplemente imprime información sobre cada mensaje, pero puede actualizar `logIncomingMessage` y `logOutgoingMessage` según sea necesario para definir las acciones que quiere realizar con cada mensaje.</span><span class="sxs-lookup"><span data-stu-id="10245-111">In this example, the bot simply prints some information about each message, but you can update `logIncomingMessage` and `logOutgoingMessage` as necessary to define the actions that you want to take for each message.</span></span> 

## <a name="sample-code"></a><span data-ttu-id="10245-112">Código de ejemplo</span><span class="sxs-lookup"><span data-stu-id="10245-112">Sample code</span></span>

<span data-ttu-id="10245-113">Para ver un ejemplo completo que muestra cómo interceptar y registrar mensajes mediante el SDK de Bot Builder para Node.js, consulte el <a href="https://github.com/Microsoft/BotBuilder-Samples/tree/master/Node/capability-middlewareLogging" target="_blank">ejemplo de middleware y registro</a> en GitHub.</span><span class="sxs-lookup"><span data-stu-id="10245-113">For a complete sample that shows how to intercept and log messages using the Bot Builder SDK for Node.js, see the <a href="https://github.com/Microsoft/BotBuilder-Samples/tree/master/Node/capability-middlewareLogging" target="_blank">Middleware and Logging sample</a> in GitHub.</span></span>
