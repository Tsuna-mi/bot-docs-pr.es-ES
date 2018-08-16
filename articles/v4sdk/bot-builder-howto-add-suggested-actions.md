---
title: Incorporación de acciones sugeridas a mensajes | Microsoft Docs
description: Aprenda a enviar acciones sugeridas dentro de mensajes mediante Bot Builder SDK para JavaScript.
keywords: acciones sugeridas, botones, entrada adicional
author: Kaiqb
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 03/13/2018
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: 802d245112baa6d5fb10f4e992d9f6722b70a224
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39306077"
---
# <a name="add-suggested-actions-to-messages"></a><span data-ttu-id="5900a-104">Incorporación de acciones sugeridas a mensajes</span><span class="sxs-lookup"><span data-stu-id="5900a-104">Add suggested actions to messages</span></span>

[!include[Introduction to suggested actions](../includes/snippet-suggested-actions-intro.md)] 

## <a name="send-suggested-actions"></a><span data-ttu-id="5900a-105">Envío de acciones sugeridas</span><span class="sxs-lookup"><span data-stu-id="5900a-105">Send suggested actions</span></span>

<span data-ttu-id="5900a-106">Puede crear una lista de acciones sugeridas (también conocidas como "respuestas rápidas") que se mostrarán al usuario para un único turno de la conversación:</span><span class="sxs-lookup"><span data-stu-id="5900a-106">You can create a list of suggested actions (also known as "quick replies") that will be shown to the user for a single turn of the conversation:</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="5900a-107">C#</span><span class="sxs-lookup"><span data-stu-id="5900a-107">C#</span></span>](#tab/csharp)

```csharp
using Microsoft.Bot.Builder;
using Microsoft.Bot.Builder.Core.Extensions;
using Microsoft.Bot.Schema;

// Create the activity and add suggested actions.
var activity = MessageFactory.SuggestedActions(
    new CardAction[]
    {
        new CardAction(title: "red", type: ActionTypes.ImBack, value: "red"),
        new CardAction( title: "green", type: ActionTypes.ImBack, value: "green"),
        new CardAction(title: "blue", type: ActionTypes.ImBack, value: "blue")
    }, text: "Choose a color");

// Send the activity as a reply to the user.
await context.SendActivity(activity);
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="5900a-108">JavaScript</span><span class="sxs-lookup"><span data-stu-id="5900a-108">JavaScript</span></span>](#tab/javascript)

```javascript
// Require MessageFactory from botbuilder.
const {MessageFactory} = require('botbuilder');

//  Initialize the message object.
const basicMessage = MessageFactory.suggestedActions(['red', 'green', 'blue'], 'Choose a color');

await context.sendActivity(basicMessage);
```

---

## <a name="additional-resources"></a><span data-ttu-id="5900a-109">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="5900a-109">Additional resources</span></span>

<span data-ttu-id="5900a-110">Para obtener una vista previa de las características, puede usar [Channel Inspector](../bot-service-channel-inspector.md)</span><span class="sxs-lookup"><span data-stu-id="5900a-110">To preview features, you can use the [Channel Inspector](../bot-service-channel-inspector.md)</span></span>
