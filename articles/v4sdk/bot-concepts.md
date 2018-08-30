---
title: Canales y Bot Connector Service | Microsoft Docs
description: Conceptos clave de Bot Builder SDK.
keywords: actividades, conversación
author: jonathanfingold
ms.author: jonathanfingold
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 03/28/2018
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: eaf1a8f714ea8711985b732f797951d241abbca2
ms.sourcegitcommit: 44f100a588ffda19c275b118f4f97029f12d1449
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/26/2018
ms.locfileid: "42928303"
---
# <a name="channels-and-the-bot-connector-service"></a><span data-ttu-id="742ea-104">Canales y Bot Connector Service</span><span class="sxs-lookup"><span data-stu-id="742ea-104">Channels and the Bot Connector Service</span></span>

[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]

<span data-ttu-id="742ea-105">Los canales son el punto de conexión desde el que un usuario se conecta a nuestro bot, como Facebook, Skype, correo electrónico, Slack, etc. Bot Connector Service, que se configura a través de [Azure Portal](https://portal.azure.com), conecta su bot a estos canales y facilita la comunicación entre el bot y el usuario.</span><span class="sxs-lookup"><span data-stu-id="742ea-105">Channels are the endpoint that a user is connecting to our bot from, such as Facebook, Skype, Email, Slack, etc. The Bot Connector Service, configured through the [Azure portal](https://portal.azure.com), connects your bot to these channels and facilitates communication between your bot and the user.</span></span> 

<span data-ttu-id="742ea-106">Además de los canales estándar proporcionados con Bot Connector Service, también puede conectar el bot a su propia aplicación cliente con [línea directa](bot-builder-howto-direct-line.md) como canal.</span><span class="sxs-lookup"><span data-stu-id="742ea-106">In addition to standard channels provided with the Bot Connector Service, you can also connect your bot to your own client application using [Direct Line](bot-builder-howto-direct-line.md) as your channel.</span></span>

<span data-ttu-id="742ea-107">Bot Connector Service le permite desarrollar su bot de forma independiente del canal mediante la normalización de los mensajes que el bot envía a un canal.</span><span class="sxs-lookup"><span data-stu-id="742ea-107">The Bot Connector Service allows you to develop your bot in a channel-agnostic way by normalizing messages that the bot sends to a channel.</span></span> <span data-ttu-id="742ea-108">Esto implica convertirlo del esquema del generador de bots al esquema del canal.</span><span class="sxs-lookup"><span data-stu-id="742ea-108">This involves converting it from the bot builder schema into the channel’s schema.</span></span> <span data-ttu-id="742ea-109">Sin embargo, si el canal no es compatible con todos los aspectos del esquema del generador de bots, el servicio intentará convertir el mensaje a un formato que sea compatible con el canal.</span><span class="sxs-lookup"><span data-stu-id="742ea-109">However, if the channel does not support all aspects of the bot builder schema, the service will try to convert the message to a format that the channel does support.</span></span> <span data-ttu-id="742ea-110">Por ejemplo, si el bot envía al canal SMS un mensaje que contiene una tarjeta con los botones de acción, el conector puede enviar la tarjeta como imagen e incluir las acciones como vínculos en el texto del mensaje.</span><span class="sxs-lookup"><span data-stu-id="742ea-110">For example, if the bot sends a message that contains a card with action buttons to the SMS channel, the connector may send the card as an image and include the actions as links in the message’s text.</span></span>

## <a name="activities-and-conversations"></a><span data-ttu-id="742ea-111">Actividades y conversaciones</span><span class="sxs-lookup"><span data-stu-id="742ea-111">Activities and conversations</span></span>


<span data-ttu-id="742ea-112">Bot Connector Service usa JSON para intercambiar información entre el bot y el usuario, y el Bot Builder SDK encapsula esta información en un objeto de actividad específico del idioma.</span><span class="sxs-lookup"><span data-stu-id="742ea-112">The Bot Connector Service uses JSON to exchange information between the bot and the user, and the Bot Builder SDK wraps this information in a language-specific activity object.</span></span> <span data-ttu-id="742ea-113">Los [tipos de actividad](../bot-service-activities-entities.md) se mencionaron al analizar la [interacción con el bot](bot-builder-basics.md#interaction-with-your-bot), donde el tipo más común de actividad es un mensaje, pero también son importantes los otros tipos de actividad.</span><span class="sxs-lookup"><span data-stu-id="742ea-113">[Activity types](../bot-service-activities-entities.md) were mentioned when discussing [interaction with your bot](bot-builder-basics.md#interaction-with-your-bot), with the most common type of activity is a message, but the other activity types are important too.</span></span> <span data-ttu-id="742ea-114">Se incluyen una actualización de conversación, actualización de la relación de contacto, eliminar datos de usuario, fin de la conversación, escribir, reacción del mensaje y algunas otras actividades específicas del bot que probablemente el usuario nunca verá.</span><span class="sxs-lookup"><span data-stu-id="742ea-114">These include a conversation update, contact relation update, delete user data, end of conversation, typing, message reaction, and a couple other bot specific activites that the user will likely never see.</span></span> <span data-ttu-id="742ea-115">La información detallada sobre cada uno de ellos y cuándo se producen puede encontrarse en nuestro contenido de referencia de las actividades.</span><span class="sxs-lookup"><span data-stu-id="742ea-115">Details on each of these and when they happen can be found in our activity reference content.</span></span>

<span data-ttu-id="742ea-116">Cada turno y su actividad asociada pertenecen a una **conversación** lógica, que representa una interacción entre uno o varios de los bots y un usuario específico o un grupo de usuarios.</span><span class="sxs-lookup"><span data-stu-id="742ea-116">Every turn and its associated activity belongs to a logical **conversation**, which represents an interaction between one or more bots and a specific user or group of users.</span></span> <span data-ttu-id="742ea-117">Una conversación es específica de un canal y tiene un identificador exclusivo para ese canal.</span><span class="sxs-lookup"><span data-stu-id="742ea-117">A conversation is specific to a channel and has an ID that is unique to that channel.</span></span>

## <a name="next-steps"></a><span data-ttu-id="742ea-118">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="742ea-118">Next steps</span></span>

<span data-ttu-id="742ea-119">Ahora que está familiarizado con algunos conceptos clave de un bot, vamos a profundizar en las diferentes formas de conversación que puede usar nuestro bot.</span><span class="sxs-lookup"><span data-stu-id="742ea-119">Now that you're familiar with some key concepts of a bot, let's dive into the different forms of conversation our bot may use.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="742ea-120">Formularios de conversación</span><span class="sxs-lookup"><span data-stu-id="742ea-120">Conversation forms</span></span>](bot-builder-conversations.md)
