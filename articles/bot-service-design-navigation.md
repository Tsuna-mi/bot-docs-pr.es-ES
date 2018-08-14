---
title: Diseño de navegación de bots | Microsoft Docs
description: Obtenga información sobre cómo diseñar una estructura de navegación adecuada para el bot y cómo evitar los errores más comunes de diseño de navegación.
keywords: navegación, información general, bot terco, bot despistado, bot misterioso, bot de capitán obviedad, bot que no puede olvidar
author: matvelloso
ms.author: mateusv
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ms.openlocfilehash: 223a822a1a309b8d89f0554eed241a4ae376ea23
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305085"
---
# <a name="design-bot-navigation"></a><span data-ttu-id="0777d-104">Diseño de navegación de bots</span><span class="sxs-lookup"><span data-stu-id="0777d-104">Design bot navigation</span></span>

<span data-ttu-id="0777d-105">Los usuarios pueden navegar por sitios web con las rutas de navegación, por aplicaciones con los menús y por navegadores web con botones, como **adelante** y **atrás**.</span><span class="sxs-lookup"><span data-stu-id="0777d-105">Users can navigate websites using breadcrumbs, apps using menus, and web browsers using buttons like **forward** and **back**.</span></span> <span data-ttu-id="0777d-106">Sin embargo, ninguna de estas técnicas de navegación estandarizados aborda por completo los requisitos de navegación dentro de un bot.</span><span class="sxs-lookup"><span data-stu-id="0777d-106">However, none of these well-established navigation techniques entirely address navigation requirements within a bot.</span></span> <span data-ttu-id="0777d-107">Como se explicó [anteriormente](~/bot-service-design-conversation-flow.md#handle-interruptions), los usuarios interactúan a menudo con bots de forma que no es lineal, lo que dificulta el diseño de una navegación de bots que siempre ofrezca una experiencia de usuario excelente.</span><span class="sxs-lookup"><span data-stu-id="0777d-107">As discussed [previously](~/bot-service-design-conversation-flow.md#handle-interruptions), users often interact with bots in a non-linear fashion, making it challenging to design bot navigation that consistently delivers a great user experience.</span></span> 

<span data-ttu-id="0777d-108">Considere los dilemas siguientes:</span><span class="sxs-lookup"><span data-stu-id="0777d-108">Consider the following dilemmas:</span></span>

- <span data-ttu-id="0777d-109">¿Cómo se asegura de que un usuario no se pierda en una conversación con un bot?</span><span class="sxs-lookup"><span data-stu-id="0777d-109">How do you ensure that a user doesn't get lost in a conversation with a bot?</span></span> 
- <span data-ttu-id="0777d-110">¿Puede un usuario navegar "atrás" en una conversación con un bot?</span><span class="sxs-lookup"><span data-stu-id="0777d-110">Can a user navigate "back" in a conversation with a bot?</span></span> 
- <span data-ttu-id="0777d-111">¿Cómo hace un usuario para ir al "menú principal" durante una conversación con un bot?</span><span class="sxs-lookup"><span data-stu-id="0777d-111">How does a user navigate to the "main menu" during a conversation with a bot?</span></span> 
- <span data-ttu-id="0777d-112">¿Cómo hace un usuario para "cancelar" una operación durante una conversación con un bot?</span><span class="sxs-lookup"><span data-stu-id="0777d-112">How does a user "cancel" an operation during a conversation with a bot?</span></span> 

<span data-ttu-id="0777d-113">Los detalles de diseño de navegación de su bot dependerán en gran medida las características y funcionalidad que admita el bot.</span><span class="sxs-lookup"><span data-stu-id="0777d-113">The specifics of your bot's navigation design will depend largely upon the features and functionality that your bot supports.</span></span> <span data-ttu-id="0777d-114">Independientemente del tipo de bot que esté desarrollando, querrá evitar los escollos comunes de las interfaces conversacionales mal diseñadas.</span><span class="sxs-lookup"><span data-stu-id="0777d-114">Regardless of the type of bot you're developing, you'll want to avoid the common pitfalls of poorly designed conversational interfaces.</span></span> <span data-ttu-id="0777d-115">En este artículo se describen estos riesgos en cuanto a cinco personalidades: el "bot terco", el "bot despistado", el "bot misterioso", el "bot capitán obviedad" y el"bot que no puede olvidar".</span><span class="sxs-lookup"><span data-stu-id="0777d-115">This article describes these pitfalls in terms of five personalities: the "stubborn bot", the "clueless bot", the "mysterious bot", the "captain obvious bot", and the "bot that can't forget."</span></span> 

> [!TIP]
> <span data-ttu-id="0777d-116">Mitigar cada una de estas personalidades en su bot suele ser posible si se [controlan interrupciones de los usuarios](v4sdk/bot-builder-howto-handle-user-interrupt.md) correctamente.</span><span class="sxs-lookup"><span data-stu-id="0777d-116">Mitgating each type of these personalities for your bot can often be done by correctly [handling user interruptions](v4sdk/bot-builder-howto-handle-user-interrupt.md).</span></span>

## <a name="the-stubborn-bot"></a><span data-ttu-id="0777d-117">El "bot terco"</span><span class="sxs-lookup"><span data-stu-id="0777d-117">The "stubborn bot"</span></span>

<span data-ttu-id="0777d-118">El bot terco insiste en mantener el curso actual de la conversación, incluso cuando el usuario intenta dirigir las cosas en otra dirección.</span><span class="sxs-lookup"><span data-stu-id="0777d-118">The stubborn bot insists upon maintaining the current course of conversation, even when the user attempts to steer things in a different direction.</span></span> 

<span data-ttu-id="0777d-119">Considere el siguiente escenario:</span><span class="sxs-lookup"><span data-stu-id="0777d-119">Consider the following scenario:</span></span> 

![bot](~/media/bot-service-design-navigation/stubborn-bot-new.png)

<span data-ttu-id="0777d-121">A menudo, los usuarios cambian de parecer, deciden cancelar o a veces desean volver empezar por completo.</span><span class="sxs-lookup"><span data-stu-id="0777d-121">Users often change their minds, decide to cancel or sometimes they want to start over altogether.</span></span> 

> [!TIP]
> <span data-ttu-id="0777d-122"><b>Sí</b>: diseñe el bot para que tenga en cuenta que un usuario puede intentar cambiar el curso de la conversación en cualquier momento.</span><span class="sxs-lookup"><span data-stu-id="0777d-122"><b>Do</b>: Design your bot to consider that a user might attempt to change the course of the conversation at any time.</span></span> 
>
> <span data-ttu-id="0777d-123"><b>No</b>: diseñe el bot para ignorar la entrada del usuario y seguir repitiendo la misma pregunta en un bucle interminable.</span><span class="sxs-lookup"><span data-stu-id="0777d-123"><b>Don't</b>: Design your bot to ignore user input and keep repeating the same question in an endless loop.</span></span> 

<span data-ttu-id="0777d-124">Existen muchos métodos para evitar este problema, pero quizás la manera más fácil de impedir que un bot haga la misma pregunta constantemente sea simplemente especificar un número máximo de reintentos para cada pregunta.</span><span class="sxs-lookup"><span data-stu-id="0777d-124">There are many methods of avoiding this pitfall, but perhaps the easiest way to prevent a bot from asking the same question endlessly is to simply specify a maximum number of retry attempts for each question.</span></span> <span data-ttu-id="0777d-125">Si se diseña de este modo, el bot no hace nada "inteligente" para comprender la entrada del usuario y responder de manera apropiada, pero al menos evitará hacer la misma pregunta en un bucle infinito.</span><span class="sxs-lookup"><span data-stu-id="0777d-125">If designed in this manner, the bot is not doing anything "smart" to understand the user's input and respond appropriately but will at least avoid asking the same question in an endless loop.</span></span> 

## <a name="the-clueless-bot"></a><span data-ttu-id="0777d-126">El "bot despistado"</span><span class="sxs-lookup"><span data-stu-id="0777d-126">The "clueless bot"</span></span>

<span data-ttu-id="0777d-127">El bot despistado responde de manera absurda cuando no entiende el intento de un usuario por acceder a cierta funcionalidad.</span><span class="sxs-lookup"><span data-stu-id="0777d-127">The clueless bot responds in a nonsensical manner when it doesn't understand a user's attempt to access certain functionality.</span></span> <span data-ttu-id="0777d-128">Un usuario puede intentar comandos comunes de palabra clave, como "ayuda" o "cancelar" con la expectativa razonable de que el bot responderá correctamente.</span><span class="sxs-lookup"><span data-stu-id="0777d-128">A user may try common keyword commands like "help" or "cancel" with reasonable expectations that the bot will respond appropriately.</span></span>

<span data-ttu-id="0777d-129">Considere el siguiente escenario:</span><span class="sxs-lookup"><span data-stu-id="0777d-129">Consider the following scenario:</span></span> 

![bot](~/media/bot-service-design-navigation/clueless-bot.png)

<span data-ttu-id="0777d-131">Aunque es posible que se vea tentado a diseñar cada cuadro de diálogo en el bot para que escuche ciertas palabras clave y responda de manera apropiada a ellas, no se recomienda este enfoque.</span><span class="sxs-lookup"><span data-stu-id="0777d-131">Although you may be tempted to design every dialog within your bot to listen for, and respond appropriately to, certain keywords, this approach is not recommended.</span></span> 

> [!TIP]
> <span data-ttu-id="0777d-132"><b>Sí</b>: implemente [software intermedio](v4sdk/bot-builder-create-middleware.md) que examine la entrada del usuario para las palabras clave que especifique (por ejemplo, "ayuda", "cancelar", "empezar de nuevo", etc.) y responder según corresponda.</span><span class="sxs-lookup"><span data-stu-id="0777d-132"><b>Do</b>: Implement [middleware](v4sdk/bot-builder-create-middleware.md) that will examine user input for the keywords that you specify (ex: "help", "cancel", "start over", etc.) and respond appropriately.</span></span> 
> 
> <span data-ttu-id="0777d-133"><b>No</b>: diseñe cada cuadro de diálogo para examinar la entrada del usuario en busca de una lista de palabras clave.</span><span class="sxs-lookup"><span data-stu-id="0777d-133"><b>Don't</b>: Design every dialog to examine user input for a list of keywords.</span></span> 

<span data-ttu-id="0777d-134">Al definir la lógica en el **software intermedio**, la hará accesible a cada intercambio con el usuario.</span><span class="sxs-lookup"><span data-stu-id="0777d-134">By defining the logic in your **middleware**, you're making it accessible to every exchange with the user.</span></span> <span data-ttu-id="0777d-135">Con este enfoque, los avisos y cuadros de diálogo individuales pueden crearse para pasar por alto las palabras clave con seguridad, si es necesario.</span><span class="sxs-lookup"><span data-stu-id="0777d-135">Using this approach, individual dialogs and prompts can be made to safely ignore the keywords, if necessary.</span></span>

## <a name="the-mysterious-bot"></a><span data-ttu-id="0777d-136">El "bot misterioso"</span><span class="sxs-lookup"><span data-stu-id="0777d-136">The "mysterious bot"</span></span>

<span data-ttu-id="0777d-137">El bot misterioso no reconoce de inmediato la entrada del usuario de ninguna manera.</span><span class="sxs-lookup"><span data-stu-id="0777d-137">The mysterious bot fails to immediately acknowledge the user's input in any way.</span></span> 

<span data-ttu-id="0777d-138">Considere el siguiente escenario:</span><span class="sxs-lookup"><span data-stu-id="0777d-138">Consider the following scenario:</span></span> 

![bot](~/media/bot-service-design-navigation/mysterious-bot.png)

<span data-ttu-id="0777d-140">En algunos casos, esta situación podría ser un indicador de que el bot tiene una interrupción del servicio.</span><span class="sxs-lookup"><span data-stu-id="0777d-140">In some cases, this situation might be an indication that the bot is having an outage.</span></span> <span data-ttu-id="0777d-141">Sin embargo, podría ser simplemente que el bot está ocupado procesando la entrada del usuario y aún no ha terminado de compilar la respuesta.</span><span class="sxs-lookup"><span data-stu-id="0777d-141">However, it could just be that the bot is busy processing the user's input and hasn't yet finished compiling its response.</span></span> 

> [!TIP]
> <span data-ttu-id="0777d-142"><b>Sí</b>: diseñe el bot para que reconozca de inmediato la entrada del usuario, incluso en casos donde el bot pueda tardar algún tiempo en compilar la respuesta.</span><span class="sxs-lookup"><span data-stu-id="0777d-142"><b>Do</b>: Design your bot to immediately acknowledge user input, even in cases where the bot may take some time to compile its response.</span></span> 
> 
> <span data-ttu-id="0777d-143"><b>No</b>: diseñe el bot para posponer la confirmación de la entrada del usuario hasta que el bot termine de compilar la respuesta.</span><span class="sxs-lookup"><span data-stu-id="0777d-143"><b>Don't</b>: Design your bot to postpone acknowledgement of user input until the bot finishes compiling its response.</span></span>

<span data-ttu-id="0777d-144">Al reconocer de inmediato la entrada del usuario, elimina cualquier posibilidad de confusión en cuanto al estado del bot.</span><span class="sxs-lookup"><span data-stu-id="0777d-144">By immediately acknowledging the user's input, you eliminate any potential for confusion as to the state of the bot.</span></span> <span data-ttu-id="0777d-145">Si la respuesta tarda mucho tiempo en compilarse, considere la posibilidad de enviar un mensaje de "escritura" para indicar que el bot está trabajando y, a continuación, envíe un [mensaje proactivo](v4sdk/bot-builder-howto-proactive-message.md).</span><span class="sxs-lookup"><span data-stu-id="0777d-145">If your response takes a long time to compile, consider sending a "typing" message to indicate your bot is working, and then following up with a [proactive message](v4sdk/bot-builder-howto-proactive-message.md)</span></span>

## <a name="the-captain-obvious-bot"></a><span data-ttu-id="0777d-146">El "bot capitán obviedad"</span><span class="sxs-lookup"><span data-stu-id="0777d-146">The "captain obvious bot"</span></span>

<span data-ttu-id="0777d-147">El bot capitán obviedad proporciona información que no se pidió y que es totalmente obvia y, por tanto, inútil para el usuario.</span><span class="sxs-lookup"><span data-stu-id="0777d-147">The captain obvious bot provides unsolicited information that is completely obvious and therefore useless to the user.</span></span> 

<span data-ttu-id="0777d-148">Considere el siguiente escenario:</span><span class="sxs-lookup"><span data-stu-id="0777d-148">Consider the following scenario:</span></span>

![bot](~/media/bot-service-design-navigation/captainobvious-bot.png)

> [!TIP]
> <span data-ttu-id="0777d-150"><b>Sí</b>: diseñe el bot para proporcionar información que sea útil para el usuario.</span><span class="sxs-lookup"><span data-stu-id="0777d-150"><b>Do</b>: Design your bot to provide information that will be useful to the user.</span></span> 
> 
> <span data-ttu-id="0777d-151"><b>No</b>: diseñe el bot para proporcionar información que el usuario no pidió y probablemente le resulte inútil.</span><span class="sxs-lookup"><span data-stu-id="0777d-151"><b>Don't</b>: Design your bot to provide unsolicited information that is unlikely to be useful to the user.</span></span>

<span data-ttu-id="0777d-152">Al diseñar el bot para proporcionar información útil, aumentará la probabilidad de que el usuario interactúe con el bot.</span><span class="sxs-lookup"><span data-stu-id="0777d-152">By designing your bot to provide useful information, you're increasing the odds that the user will engage with your bot.</span></span>

## <a name="the-bot-that-cant-forget"></a><span data-ttu-id="0777d-153">El "bot que no puede olvidar"</span><span class="sxs-lookup"><span data-stu-id="0777d-153">The "bot that can't forget"</span></span>

<span data-ttu-id="0777d-154">El bot que no puede olvidar integra de manera incorrecta información de conversaciones anteriores a la conversación actual.</span><span class="sxs-lookup"><span data-stu-id="0777d-154">The bot that can't forget inappropriately integrates information from past conversations into the current conversation.</span></span> 

<span data-ttu-id="0777d-155">Considere el siguiente escenario:</span><span class="sxs-lookup"><span data-stu-id="0777d-155">Consider the following scenario:</span></span>

![bot](~/media/bot-service-design-navigation/rememberall-bot.png)

> [!TIP]
> <span data-ttu-id="0777d-157"><b>Sí</b>: diseñe el bot para que mantenga el tema actual de la conversación, a menos/hasta que el usuario exprese su deseo de volver a un tema anterior.</span><span class="sxs-lookup"><span data-stu-id="0777d-157"><b>Do</b>: Design your bot to maintain the current topic of conversation, unless/until the user expresses a desire to revisit a prior topic.</span></span> 
> 
> <span data-ttu-id="0777d-158"><b>No</b>: diseñe el bot para interponer información de las conversaciones anteriores cuando no sea pertinente para la conversación actual.</span><span class="sxs-lookup"><span data-stu-id="0777d-158"><b>Don't</b>: Design your bot to interject information from past conversations when it is not relevant to the current conversation.</span></span>

<span data-ttu-id="0777d-159">Al mantener el tema actual de la conversación, reduce la posibilidad de confusión y frustración, así como aumenta la probabilidad de que el usuario continúe interactuando con el bot.</span><span class="sxs-lookup"><span data-stu-id="0777d-159">By maintaining the current topic of conversation, you reduce the potential for confusion and frustration as well as increasing the odds that the user will continue to engage with your bot.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0777d-160">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="0777d-160">Next steps</span></span>

<span data-ttu-id="0777d-161">Al diseñar el bot para que evite estos problemas comunes de interfaces conversacionales mal diseñadas, dará un paso importante para garantizar una experiencia de usuario excelente.</span><span class="sxs-lookup"><span data-stu-id="0777d-161">By designing your bot to avoid these common pitfalls of poorly designed conversational interfaces, you're taking an important step toward ensuring a great user experience.</span></span> 

<span data-ttu-id="0777d-162">A continuación, obtenga más información sobre los [elementos de la experiencia de usuario](~/bot-service-design-user-experience.md) en que los bots se basan normalmente para intercambiar información con los usuarios.</span><span class="sxs-lookup"><span data-stu-id="0777d-162">Next, learn more about the [UX elements](~/bot-service-design-user-experience.md) that bots most typically rely upon to exchange information with users.</span></span> 
