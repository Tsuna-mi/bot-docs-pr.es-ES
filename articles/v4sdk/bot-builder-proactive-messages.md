---
title: Mensajes proactivos | Microsoft Docs
description: Descubra cómo enviar mensajes de manera proactiva.
keywords: dar la bienvenida al usuario, iniciar conversación
author: ivorb
ms.author: v-ivorb
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 03/21/2018
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: f311f70e9bfb72db780546b5e289f09d803589dc
ms.sourcegitcommit: 1abc32353c20acd103e0383121db21b705e5eec3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/21/2018
ms.locfileid: "42756581"
---
# <a name="proactive-messages"></a><span data-ttu-id="7056c-104">Mensajes proactivos</span><span class="sxs-lookup"><span data-stu-id="7056c-104">Proactive messages</span></span>
[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]
<!--
When you think about the exchange of messages between your bot and the user, you're probably thinking about the scenario where the user sends a message to your bot and your bot then replies to the user with a message of its own. We call this _reactive messaging_ and it's by far the most common flow that you should optimize your bot's code for.

It is possible, however, for your bot to initiate a conversation with the user by sending them a message first. We call this _proactive messaging_ and while the code you'll write to send a proactive message is very similar to what you'd write in the reactive case, there are a few differences that are worth exploring.

The first thing to note is that before you can send a proactive message to a user, the user will have to send at least one reactive style message to your bot. There are two reasons for this.

1. You need to get the user's `ConversationReference` and save it somewhere for future use. You can think of the conversation reference as the user's address, as it contains information about the channel they came in on, their user ID, the conversation ID, and even the server that should receive any future messages. This object is simple JSON and should be saved whole without tampering.
2. Most channels by policy won't let a bot initiate conversations with users they've never spoken to before. Depending on the channel the user might need to explicitly add the bot to a conversation or at a minimum send an initial message to the bot.

> ![NOTE]
> This bot currently runs properly only when deployed to Azure. However, you can test the bot without publishing it.

A common case of proactive messaging comes when our bot is performing a time-consuming task. In this case, we send a **typing** activity indicates to the user that the bot is in a *processing* mode, and then follow it up with a proactive message once our processing has completed.
-->

[!include[Introduction to proactive messages - part 1](../includes/snippet-proactive-messages-intro-1.md)] 

## <a name="types-of-proactive-messages"></a><span data-ttu-id="7056c-105">Tipos de mensajes automáticos</span><span class="sxs-lookup"><span data-stu-id="7056c-105">Types of proactive messages</span></span> 

[!include[Introduction to proactive messages - part 2](../includes/snippet-proactive-messages-intro-2.md)] 

## <a name="next-steps"></a><span data-ttu-id="7056c-106">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="7056c-106">Next steps</span></span>

<span data-ttu-id="7056c-107">Ahora que está familiarizado con las actividades, la mensajería y el flujo de conversación, echemos un vistazo a otros aspectos importantes de su bot comenzando por la comprensión del lenguaje con LUIS.</span><span class="sxs-lookup"><span data-stu-id="7056c-107">Now that you're familiar with activites, messaging, and conversation flow, lets look at other important aspects of your bot starting with language understanding using LUIS.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="7056c-108">Comprensión del lenguaje</span><span class="sxs-lookup"><span data-stu-id="7056c-108">Language understanding</span></span>](bot-builder-concept-luis.md)
