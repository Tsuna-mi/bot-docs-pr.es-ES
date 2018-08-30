---
title: Control de eventos de usuario y conversación | Microsoft Docs
description: Aprenda a controlar eventos como la unión de un usuario a una conversación mediante el SDK de Bot Builder para Node.js.
author: DucVo
ms.author: v-ducvo
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: c37823b94a5cc4715dd1278bba196335de3e3bdb
ms.sourcegitcommit: 2dc75701b169d822c9499e393439161bc87639d2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42906028"
---
# <a name="handle-user-and-conversation-events"></a><span data-ttu-id="907ea-103">Control de eventos de usuario y conversación</span><span class="sxs-lookup"><span data-stu-id="907ea-103">Handle user and conversation events</span></span>

[!INCLUDE [pre-release-label](../includes/pre-release-label-v3.md)]

<span data-ttu-id="907ea-104">En este artículo se muestra cómo el bot puede controlar eventos, como la unión de un usuario a una conversación, agregar un bot a una lista de contactos o decir **Adiós** cuando se quita un bot de una conversación.</span><span class="sxs-lookup"><span data-stu-id="907ea-104">This article demonstrates how your bot can handle events such as a user joining a conversation, adding a bot to a contacts list, or saying **Goodbye** when a bot is being removed from a conversation.</span></span>


## <a name="greet-a-user-on-conversation-join"></a><span data-ttu-id="907ea-105">Saludo a un usuario al unirse a la conversación</span><span class="sxs-lookup"><span data-stu-id="907ea-105">Greet a user on conversation join</span></span>
<span data-ttu-id="907ea-106">Bot Framework proporciona el evento [conversationUpdate][conversationUpdate] para notificar a su bot cada vez que un miembro se une a una conversación o la abandona.</span><span class="sxs-lookup"><span data-stu-id="907ea-106">The Bot Framework provides the [conversationUpdate][conversationUpdate] event for notifying your bot whenever a member joins or leaves a conversation.</span></span> <span data-ttu-id="907ea-107">Un miembro de la conversación puede ser un usuario o un bot.</span><span class="sxs-lookup"><span data-stu-id="907ea-107">A conversation member can be a user or a bot.</span></span>

<span data-ttu-id="907ea-108">El siguiente fragmento de código permite al bot saludar a los nuevos miembros de una conversación o decir **Adiós** si se quitan de la conversación.</span><span class="sxs-lookup"><span data-stu-id="907ea-108">The following code snippet allows the bot to greet new member(s) to a conversation or say **Goodbye** if it is being removed from the conversation.</span></span>

[!INCLUDE [conversationUpdate sample Node.js](../includes/snippet-code-node-conversationupdate-1.md)]

## <a name="acknowledge-add-to-contacts-list"></a><span data-ttu-id="907ea-109">Confirmación de incorporación a la lista de contactos</span><span class="sxs-lookup"><span data-stu-id="907ea-109">Acknowledge add to contacts list</span></span>

<span data-ttu-id="907ea-110">El evento [contactRelationUpdate][contactRelationUpdate] notifica al bot que un usuario le ha agregado a su lista de contactos.</span><span class="sxs-lookup"><span data-stu-id="907ea-110">The [contactRelationUpdate][contactRelationUpdate] event notifies your bot that a user has added you to their contacts list.</span></span>

[!INCLUDE [contactRelationUpdate sample Node.js](../includes/snippet-code-node-contactrelationupdate-1.md)]

## <a name="add-a-first-run-dialog"></a><span data-ttu-id="907ea-111">Incorporación de un primer diálogo de ejecución</span><span class="sxs-lookup"><span data-stu-id="907ea-111">Add a first-run dialog</span></span>

<span data-ttu-id="907ea-112">Dado que los eventos **conversationUpdate** y **contactRelationUpdate** no se admiten en todos los canales, una manera universal de saludar a un usuario que se une a una conversación es agregar un primer diálogo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="907ea-112">Since the **conversationUpdate** and the **contactRelationUpdate** event are not supported by all channels, a universal way to greet a user who joins a conversation is to add a first-run dialog.</span></span>

<span data-ttu-id="907ea-113">En el ejemplo siguiente, agregamos una función que desencadena el diálogo cada vez que nunca hemos visto antes a un usuario.</span><span class="sxs-lookup"><span data-stu-id="907ea-113">In the following example we’ve added a function that triggers the dialog any time we’ve never seen a user before.</span></span> <span data-ttu-id="907ea-114">Puede personalizar la manera en que se desencadena una acción si proporciona un controlador [onFindAction] [onFindAction] para la acción.</span><span class="sxs-lookup"><span data-stu-id="907ea-114">You can customize the way an action is triggered by providing an [onFindAction][onFindAction] handler for your action.</span></span> 

[!INCLUDE [first-run sample Node.js](../includes/snippet-code-node-first-run-dialog-1.md)]

<span data-ttu-id="907ea-115">También puede personalizar lo que hace una acción después de que se ha desencadenado si proporciona un controlador [onSelectAction][onSelectAction].</span><span class="sxs-lookup"><span data-stu-id="907ea-115">You can also customize what an action does after its been triggered by providing an [onSelectAction][onSelectAction] handler.</span></span> <span data-ttu-id="907ea-116">Para desencadenar acciones, puede proporcionar un controlador [onInterrupted][onInterrupted] para interceptar una interrupción antes de que ocurra.</span><span class="sxs-lookup"><span data-stu-id="907ea-116">For trigger actions you can provide an [onInterrupted][onInterrupted] handler to intercept an interruption before it occurs.</span></span> <span data-ttu-id="907ea-117">Para más información, consulte [Control de acciones del usuario](bot-builder-nodejs-dialog-actions.md).</span><span class="sxs-lookup"><span data-stu-id="907ea-117">For more information, see [Handle user actions](bot-builder-nodejs-dialog-actions.md)</span></span>

## <a name="additional-resources"></a><span data-ttu-id="907ea-118">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="907ea-118">Additional resources</span></span>

* <span data-ttu-id="907ea-119">[conversationUpdate][conversationUpdate]</span><span class="sxs-lookup"><span data-stu-id="907ea-119">[conversationUpdate][conversationUpdate]</span></span>
* <span data-ttu-id="907ea-120">[contactRelationUpdate][contactRelationUpdate]</span><span class="sxs-lookup"><span data-stu-id="907ea-120">[contactRelationUpdate][contactRelationUpdate]</span></span>

[conversationUpdate]: https://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.iconversationupdate.html
[contactRelationUpdate]: https://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.icontactrelationupdate.html

[onFindAction]: https://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.itriggeractionoptions#onfindaction
[onSelectAction]: https://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.itriggeractionoptions#onselectaction
[onInterrupted]: https://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.itriggeractionoptions#oninterrupted

[SendTyping]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.session#sendtyping
[IMessage]: http://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.imessage
[ChatConnector]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.chatconnector.html
[session_userData]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.session.html#userdata
