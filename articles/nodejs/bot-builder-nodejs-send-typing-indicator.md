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
# <a name="send-a-typing-indicator"></a>Envío de un indicador de escritura 


Los usuarios esperan una respuesta a tiempo a sus mensajes. Si el bot realiza alguna tarea de larga ejecución como llamar a un servidor o ejecutar una consulta sin proporcionar al usuario ninguna indicación de que el bot le ha escuchado, el usuario podría impacientarse o simplemente suponer que el bot no funciona.
Muchos canales admiten el envío de una indicación de escritura para mostrar al usuario que el mensaje se recibió y que se está procesando.


## <a name="typing-indicator-example"></a>Ejemplo de indicador de escritura

En el ejemplo siguiente se muestra cómo enviar una indicación de escritura mediante [session.sendTyping()][SendTyping].  Puede probar esto con el emulador de Bot Framework.


```javascript

// Create bot and default message handler
var bot = new builder.UniversalBot(connector, function (session) {
    session.sendTyping();
    setTimeout(function () {
        session.send("Hello there...");
    }, 3000);
});
```

Los indicadores de escritura también son útiles cuando se inserta un retraso de mensaje para evitar que los mensajes que contienen imágenes se envíen desordenados.

Para más información, consulte [How to send a rich card](bot-builder-nodejs-send-rich-cards.md) (Envío de una tarjeta enriquecida).


## <a name="additional-resources"></a>Recursos adicionales

* [sendTyping][SendTyping]


[SendTyping]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.session#sendtyping
[IMessage]: http://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.imessage