---
title: Intercambio de información mediante el control web | Microsoft Docs
description: Obtenga información sobre cómo intercambiar información entre el bot y una página web mediante el SDK de Bot Builder para Node.js.
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: b4c050ba785edcf700de4a6e4f76056464a9f649
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39304737"
---
# <a name="use-the-backchannel-mechanism"></a><span data-ttu-id="524e5-103">Use el mecanismo Backchannel</span><span class="sxs-lookup"><span data-stu-id="524e5-103">Use the backchannel mechanism</span></span>

[!INCLUDE [Introduction to backchannel mechanism](../includes/snippet-backchannel.md)]

## <a name="walk-through"></a><span data-ttu-id="524e5-104">Tutorial</span><span class="sxs-lookup"><span data-stu-id="524e5-104">Walk through</span></span>

<span data-ttu-id="524e5-105">El control del chat web de código abierto tiene acceso a la API de Direct Line mediante una clase de JavaScript denominada <a href="https://github.com/microsoft/botframework-DirectLinejs" target="_blank">DirectLineJS</a>.</span><span class="sxs-lookup"><span data-stu-id="524e5-105">The open source web chat control accesses the Direct Line API by using a JavaScript class called <a href="https://github.com/microsoft/botframework-DirectLinejs" target="_blank">DirectLineJS</a>.</span></span> <span data-ttu-id="524e5-106">El control puede crear su propia instancia de Direct Line o puede compartir una con la página de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="524e5-106">The control can either create its own instance of Direct Line, or it can share one with the hosting page.</span></span> <span data-ttu-id="524e5-107">Si el control comparte una instancia de Direct Line con la página de hospedaje, el control y la página podrán enviar y recibir actividades.</span><span class="sxs-lookup"><span data-stu-id="524e5-107">If the control shares an instance of Direct Line with the hosting page, both the control and the page will be capable of sending and receiving activities.</span></span> <span data-ttu-id="524e5-108">En el siguiente diagrama se muestra la arquitectura de alto nivel de un sitio web que es compatible con la funcionalidad de bot mediante el control (de chat) web de código abierto y la API de Direct Line.</span><span class="sxs-lookup"><span data-stu-id="524e5-108">The following diagram shows the high-level architecture of a website that supports bot functionality by using the open source web (chat) control and the Direct Line API.</span></span> 

![Backchannel](../media/designing-bots/patterns/back-channel.png)

### <a name="sample-code"></a><span data-ttu-id="524e5-110">Código de ejemplo</span><span class="sxs-lookup"><span data-stu-id="524e5-110">Sample code</span></span> 

<span data-ttu-id="524e5-111">En este ejemplo, el bot y una página web utilizarán el mecanismo Backchannel para intercambiar información que no es visible para el usuario.</span><span class="sxs-lookup"><span data-stu-id="524e5-111">In this example, the bot and web page will use the backchannel mechanism to exchange information that is invisible to the user.</span></span> <span data-ttu-id="524e5-112">El bot solicitará que la página web cambie su color de fondo y la página web notificará al bot cuando el usuario haga clic en un botón de la página.</span><span class="sxs-lookup"><span data-stu-id="524e5-112">The bot will request that the web page change its background color, and the web page will notify the bot when the user clicks a button on the page.</span></span> 

> [!NOTE]
> <span data-ttu-id="524e5-113">Los fragmentos de código en este artículo vienen del <a href="https://github.com/Microsoft/BotFramework-WebChat/blob/master/samples/backchannel/index.html" target="_blank">ejemplo de Backchannel</a> y del <a href="https://github.com/ryanvolum/backChannelBot" target="_blank">bot de Backchannel</a>.</span><span class="sxs-lookup"><span data-stu-id="524e5-113">The code snippets in this article originate from the <a href="https://github.com/Microsoft/BotFramework-WebChat/blob/master/samples/backchannel/index.html" target="_blank">backchannel sample</a> and the <a href="https://github.com/ryanvolum/backChannelBot" target="_blank">backchannel bot</a>.</span></span> 

#### <a name="client-side-code"></a><span data-ttu-id="524e5-114">Código del lado cliente</span><span class="sxs-lookup"><span data-stu-id="524e5-114">Client-side code</span></span>

<span data-ttu-id="524e5-115">En primer lugar, la página web crea un objeto **DirectLine**.</span><span class="sxs-lookup"><span data-stu-id="524e5-115">First, the web page creates a **DirectLine** object.</span></span>

```javascript
var botConnection = new BotChat.DirectLine(...);
```

<span data-ttu-id="524e5-116">A continuación, comparte el objeto **DirectLine** cuando crea la instancia de WebChat.</span><span class="sxs-lookup"><span data-stu-id="524e5-116">Then, it shares the **DirectLine** object when creating the WebChat instance.</span></span>

```javascript
BotChat.App({
    botConnection: botConnection,
    user: user,
    bot: bot
}, document.getElementById("BotChatGoesHere"));
```

<span data-ttu-id="524e5-117">Cuando el usuario hace clic en un botón de la página web, esta envía una actividad de tipo "evento" para notificar al bot que se presionó el botón.</span><span class="sxs-lookup"><span data-stu-id="524e5-117">When the user clicks a button on the web page, the web page posts an activity of type "event" to notify the bot that the button was clicked.</span></span>

```javascript
const postButtonMessage = () => {
    botConnection
        .postActivity({type: "event", value: "", from: {id: "me" }, name: "buttonClicked"})
        .subscribe(id => console.log("success"));
    }
```

> [!TIP]
> <span data-ttu-id="524e5-118">Use los atributos `name` y `value` para comunicarle al bot cualquier información que pueda necesitar con el fin de interpretar correctamente el evento o para responder al mismo.</span><span class="sxs-lookup"><span data-stu-id="524e5-118">Use the attributes `name` and `value` to communicate any information that the bot may need in order to properly interpret and/or respond to the event.</span></span> 

<span data-ttu-id="524e5-119">Por último, la página web también está a la escucha de un evento específico del bot.</span><span class="sxs-lookup"><span data-stu-id="524e5-119">Finally, the web page also listens for a specific event from the bot.</span></span>
<span data-ttu-id="524e5-120">En este ejemplo, la página web está a la escucha de una actividad donde type ="event" y name="changeBackground".</span><span class="sxs-lookup"><span data-stu-id="524e5-120">In this example, the web page listens for an activity where type="event" and name="changeBackground".</span></span> <span data-ttu-id="524e5-121">Cuando recibe este tipo de actividad, cambia el color de fondo de la página web al `value` especificado por la actividad.</span><span class="sxs-lookup"><span data-stu-id="524e5-121">When it receives this type of activity, it changes the background color of the web page to the `value` specified by the activity.</span></span> 

```javascript
botConnection.activity$
    .filter(activity => activity.type === "event" && activity.name === "changeBackground")
    .subscribe(activity => changeBackgroundColor(activity.value))
```

#### <a name="server-side-code"></a><span data-ttu-id="524e5-122">Código del lado servidor</span><span class="sxs-lookup"><span data-stu-id="524e5-122">Server-side code</span></span>

<span data-ttu-id="524e5-123">El <a href="https://github.com/ryanvolum/backChannelBot" target="_blank">bot de Backchannel</a> crea un evento mediante el uso de una función auxiliar.</span><span class="sxs-lookup"><span data-stu-id="524e5-123">The <a href="https://github.com/ryanvolum/backChannelBot" target="_blank">backchannel bot</a> creates an event by using a helper function.</span></span>

```javascript
var bot = new builder.UniversalBot(connector, 
    function (session) {
        var reply = createEvent("changeBackground", session.message.text, session.message.address);
        session.endDialog(reply);
    }
);

const createEvent = (eventName, value, address) => {
    var msg = new builder.Message().address(address);
    msg.data.type = "event";
    msg.data.name = eventName;
    msg.data.value = value;
    return msg;
}
```

<span data-ttu-id="524e5-124">Del mismo modo, el bot también escucha los eventos desde el cliente.</span><span class="sxs-lookup"><span data-stu-id="524e5-124">Likewise, the bot also listens for events from the client.</span></span> <span data-ttu-id="524e5-125">En este ejemplo, si el bot recibe un evento con `name="buttonClicked"`, enviará un mensaje al usuario para que diga "veo que hizo clic en un botón".</span><span class="sxs-lookup"><span data-stu-id="524e5-125">In this example, if the bot receives an event with `name="buttonClicked"`, it will send a message to the user to say "I see that you clicked a button."</span></span>

```javascript
bot.on("event", function (event) {
    var msg = new builder.Message().address(event.address);
    msg.data.textLocale = "en-us";
    if (event.name === "buttonClicked") {
        msg.data.text = "I see that you clicked a button.";
    }
    bot.send(msg);
})
```

## <a name="additional-resources"></a><span data-ttu-id="524e5-126">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="524e5-126">Additional resources</span></span>

- <span data-ttu-id="524e5-127">[Direct Line API][directLineAPI] (API de Direct Line)</span><span class="sxs-lookup"><span data-stu-id="524e5-127">[Direct Line API][directLineAPI]</span></span>
- <span data-ttu-id="524e5-128"><a href="https://github.com/Microsoft/BotFramework-WebChat" target="_blank">Control Chat en web de Microsoft Bot Framework</a></span><span class="sxs-lookup"><span data-stu-id="524e5-128"><a href="https://github.com/Microsoft/BotFramework-WebChat" target="_blank">Microsoft Bot Framework WebChat control</a></span></span>
- <span data-ttu-id="524e5-129"><a href="https://github.com/Microsoft/BotFramework-WebChat/blob/master/samples/backchannel/index.html" target="_blank">Ejemplo de Backchannel</a></span><span class="sxs-lookup"><span data-stu-id="524e5-129"><a href="https://github.com/Microsoft/BotFramework-WebChat/blob/master/samples/backchannel/index.html" target="_blank">Backchannel sample</a></span></span>
- <span data-ttu-id="524e5-130"><a href="https://github.com/ryanvolum/backChannelBot" target="_blank">Bot de Backchannel</a></span><span class="sxs-lookup"><span data-stu-id="524e5-130"><a href="https://github.com/ryanvolum/backChannelBot" target="_blank">Backchannel bot</a></span></span>

[directLineAPI]: https://docs.botframework.com/en-us/restapi/directline3/#navtitle