---
title: Agregar acciones sugeridas a los mensajes | Microsoft Docs
description: Obtenga información sobre cómo agregar acciones sugeridas a los mensajes mediante el SDK de Bot Builder para Node.js.
author: v-ducvo
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 06/06/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: 510a340506416e61f906228ccd26c0bdd6e5d4d3
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305033"
---
# <a name="add-suggested-actions-to-messages"></a><span data-ttu-id="1afb0-103">Agregar acciones sugeridas a los mensajes</span><span class="sxs-lookup"><span data-stu-id="1afb0-103">Add suggested actions to messages</span></span>
> [!div class="op_single_selector"]
> - [.NET](../dotnet/bot-builder-dotnet-add-suggested-actions.md)
> - [Node.js](../nodejs/bot-builder-nodejs-send-suggested-actions.md)
> - [REST](../rest-api/bot-framework-rest-connector-add-suggested-actions.md)

[!INCLUDE [Introduction to suggested actions](../includes/snippet-suggested-actions-intro.md)]

> [!TIP]
> Use el [Inspector de canal][channelInspector] para ver la apariencia de las acciones sugeridas y cómo funcionan en varios canales.

## <a name="suggested-actions-example"></a><span data-ttu-id="1afb0-108">Ejemplo de acciones sugeridas</span><span class="sxs-lookup"><span data-stu-id="1afb0-108">Suggested actions example</span></span>

<span data-ttu-id="1afb0-109">Para agregar acciones sugeridas a un mensaje, establezca la propiedad `suggestedActions` del mensaje en una lista de [acciones de tarjeta][ICardAction] que representa los botones que se presentarán al usuario.</span><span class="sxs-lookup"><span data-stu-id="1afb0-109">To add suggested actions to a message, set the `suggestedActions` property of the message to a list of [card actions][ICardAction] that represent the buttons to be presented to the user.</span></span>

<span data-ttu-id="1afb0-110">Este ejemplo de código muestra cómo enviar un mensaje que presenta tres acciones sugeridas al usuario:</span><span class="sxs-lookup"><span data-stu-id="1afb0-110">This code example shows how to send a message that presents three suggested actions to the user:</span></span>

[!code-javascript[Send suggested actions](../includes/code/node-send-suggested-actions.js#sendSuggestedActions)]

<span data-ttu-id="1afb0-111">Cuando el usuario pulsa en una de las acciones sugeridas, el bot recibirá un mensaje del usuario que contiene el elemento `value` de la acción correspondiente.</span><span class="sxs-lookup"><span data-stu-id="1afb0-111">When the user taps one of the suggested actions, the bot will receive a message from the user that contains the `value` of the corresponding action.</span></span>

<span data-ttu-id="1afb0-112">Tenga en cuenta que el método `imBack` registrará el elemento `value` en la ventana de chat del canal que use.</span><span class="sxs-lookup"><span data-stu-id="1afb0-112">Be aware that the `imBack` method will post the `value` to the chat window of the channel you are using.</span></span> <span data-ttu-id="1afb0-113">Si este no es el efecto deseado, puede usar el método `postBack`, que seguirá publicando la selección en su bot, pero no mostrará la selección en la ventana de chat.</span><span class="sxs-lookup"><span data-stu-id="1afb0-113">If this is not the desired effect, you can use the `postBack` method, which will still post the selection back to your bot, but will not display the selection in the chat window.</span></span> <span data-ttu-id="1afb0-114">Sin embargo, algunos canales no admiten `postBack` y, en esos casos, el método se comportará como `imBack`.</span><span class="sxs-lookup"><span data-stu-id="1afb0-114">Some channels do not support `postBack`, however, and in those instances the method will behave like `imBack`.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1afb0-115">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="1afb0-115">Additional resources</span></span>

* <span data-ttu-id="1afb0-116">[Características en versión preliminar con el Inspector de canales][inspector]</span><span class="sxs-lookup"><span data-stu-id="1afb0-116">[Preview features with the Channel Inspector][inspector]</span></span>
* <span data-ttu-id="1afb0-117">[IMessage][IMessage]</span><span class="sxs-lookup"><span data-stu-id="1afb0-117">[IMessage][IMessage]</span></span>
* <span data-ttu-id="1afb0-118">[ICardAction][ICardAction]</span><span class="sxs-lookup"><span data-stu-id="1afb0-118">[ICardAction][ICardAction]</span></span>
* <span data-ttu-id="1afb0-119">[session.send][SessionSend]</span><span class="sxs-lookup"><span data-stu-id="1afb0-119">[session.send][SessionSend]</span></span>

[IMessage]: http://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.imessage

[SessionSend]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.session.html#send

[ICardAction]: https://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.icardaction.html

[inspector]: ../bot-service-channel-inspector.md

[channelInspector]: ../bot-service-channel-inspector.md
