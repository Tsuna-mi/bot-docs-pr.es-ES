---
title: Diseño de la primera interacción del usuario de un bot | Microsoft Docs
description: Obtenga información sobre lo que es una excelente primera experiencia del usuario y cómo diseñar los bots para una operación correcta.
keywords: primera impresión, inicio, lenguaje frente a menú
author: matvelloso
ms.author: mateusv
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ms.openlocfilehash: d36f043ec3e268b7c56abef7253ddf6274a71a7e
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305421"
---
# <a name="design-a-bots-first-user-interaction"></a><span data-ttu-id="cb89e-104">Diseño de la primera interacción del usuario de un bot</span><span class="sxs-lookup"><span data-stu-id="cb89e-104">Design a bot's first user interaction</span></span>

## <a name="first-impressions-matter"></a><span data-ttu-id="cb89e-105">Las primeras impresiones son importantes</span><span class="sxs-lookup"><span data-stu-id="cb89e-105">First impressions matter</span></span>

<span data-ttu-id="cb89e-106">La primera interacción entre el usuario y el bot es fundamental para la experiencia del usuario.</span><span class="sxs-lookup"><span data-stu-id="cb89e-106">The very first interaction between the user and bot is critical to the user experience.</span></span> <span data-ttu-id="cb89e-107">Al diseñar su bot, tenga en cuenta que ese primer mensaje es más que solo decir "Hola".</span><span class="sxs-lookup"><span data-stu-id="cb89e-107">When designing your bot, keep in mind that there is more to that first message than just saying "hi."</span></span> <span data-ttu-id="cb89e-108">Al compilar una aplicación, la primera pantalla se diseña de manera que brinde importantes indicaciones de [navegación](bot-service-design-navigation.md). Los usuarios deben entender de manera intuitiva aspectos como dónde se ubica el menú y cómo funciona, dónde obtener ayuda, cuál es la directiva de privacidad, etc.</span><span class="sxs-lookup"><span data-stu-id="cb89e-108">When you build an app, you design the first screen to provide important [navigation](bot-service-design-navigation.md) cues. Users should intuitively understand things such as where the menu is located and how it works, where to go for help, what the privacy policy is, and so on.</span></span> <span data-ttu-id="cb89e-109">Cuando se diseña un bot, la primera interacción del usuario con el bot debe proporcionar el mismo tipo de información.</span><span class="sxs-lookup"><span data-stu-id="cb89e-109">When you design a bot, the user's first interaction with the bot should provide that same type of information.</span></span> 

## <a name="language-versus-menus"></a><span data-ttu-id="cb89e-110">Lenguaje frente a menús</span><span class="sxs-lookup"><span data-stu-id="cb89e-110">Language versus menus</span></span> 

<span data-ttu-id="cb89e-111">Considere estos dos diseños:</span><span class="sxs-lookup"><span data-stu-id="cb89e-111">Consider the following two designs:</span></span>

### <a name="design-1"></a><span data-ttu-id="cb89e-112">Diseño 1</span><span class="sxs-lookup"><span data-stu-id="cb89e-112">Design 1</span></span>

![bot](~/media/bot-service-design-first-interaction/hello1.png)


### <a name="design-2"></a><span data-ttu-id="cb89e-114">Diseño 2</span><span class="sxs-lookup"><span data-stu-id="cb89e-114">Design 2</span></span>

![bot](~/media/bot-service-design-first-interaction/hello2.png)

<span data-ttu-id="cb89e-116">Por lo general, no se recomienda iniciar el bot con una pregunta abierta</span><span class="sxs-lookup"><span data-stu-id="cb89e-116">Starting the bot with an open-ended question such as "How can I help you?"</span></span> <span data-ttu-id="cb89e-117">como "¿En qué puedo ayudarlo?"</span><span class="sxs-lookup"><span data-stu-id="cb89e-117">is generally not recommended.</span></span> <span data-ttu-id="cb89e-118">Si el bot puede hacer cien cosas distintas, lo más probable es que los usuarios no puedan adivinar la mayoría de ellas.</span><span class="sxs-lookup"><span data-stu-id="cb89e-118">If your bot has a hundred different things it can do, chances are users won’t be able to guess most of them.</span></span> <span data-ttu-id="cb89e-119">El bot no les indica lo que puede hacer, así es que es difícil que ellos puedan saberlo.</span><span class="sxs-lookup"><span data-stu-id="cb89e-119">Your bot didn’t tell them what it can do, so how can they possibly know?</span></span>

<span data-ttu-id="cb89e-120">Los menús proporcionan una solución sencilla para ese problema.</span><span class="sxs-lookup"><span data-stu-id="cb89e-120">Menus provide a simple solution to that problem.</span></span> <span data-ttu-id="cb89e-121">En primer lugar, al enumerar las opciones disponibles, el bot transmite sus funcionalidades al usuario.</span><span class="sxs-lookup"><span data-stu-id="cb89e-121">First, by listing the available options, your bot is conveying its capabilities to the user.</span></span> <span data-ttu-id="cb89e-122">En segundo lugar, los menús evitan que el usuario tenga que escribir mucho; en lugar de eso, simplemente pueden hacer clic.</span><span class="sxs-lookup"><span data-stu-id="cb89e-122">Second, menus spare the user from having to type too much, instead they can just click.</span></span> <span data-ttu-id="cb89e-123">Por último, el uso de los menús puede simplificar considerablemente los modelos de lenguaje natural al limitar el ámbito de la entrada que el bot podría recibir por parte del usuario.</span><span class="sxs-lookup"><span data-stu-id="cb89e-123">Finally, the use of menus can significantly simplify your natural language models by narrowing the scope of input that the bot could receive from the user.</span></span> 

> [!TIP]
> <span data-ttu-id="cb89e-124">Los menús son una herramienta valiosa en el momento de diseñar los bots para lograr una excelente experiencia del usuario; no los descarte por no ser "lo suficientemente inteligentes".</span><span class="sxs-lookup"><span data-stu-id="cb89e-124">Menus are a valuable tool when designing bots for a great user experience; don’t dismiss them as not being "smart enough."</span></span> <span data-ttu-id="cb89e-125">Puede diseñar el bot para que use los menús a la vez que mantiene la compatibilidad con la entrada de forma libre.</span><span class="sxs-lookup"><span data-stu-id="cb89e-125">You can design your bot to use menus while still supporting free form input.</span></span> <span data-ttu-id="cb89e-126">Si un usuario escribe para responder al menú inicial en lugar de seleccionar una opción del menú, el bot podría intentar analizar la entrada de texto del usuario.</span><span class="sxs-lookup"><span data-stu-id="cb89e-126">If a user responds to the initial menu by typing rather than selecting a menu option, your bot could attempt to parse the user's text input.</span></span> 

<span data-ttu-id="cb89e-127">También puede formular preguntas más precisas para guiar al usuario si el bot tiene una función específica.</span><span class="sxs-lookup"><span data-stu-id="cb89e-127">Alternatively, you can ask more pointed questions to lead the user if the bot has a specific function.</span></span> <span data-ttu-id="cb89e-128">Por ejemplo, si el bot es responsable de tomar pedidos de sándwiches, la primera interacción podría ser "Hola,</span><span class="sxs-lookup"><span data-stu-id="cb89e-128">For example, if your bot is responsible for taking sandwich orders, your first interaction could be "Hi!</span></span> <span data-ttu-id="cb89e-129">voy a tomar su pedido.</span><span class="sxs-lookup"><span data-stu-id="cb89e-129">I'm here to take your sandwich order.</span></span> <span data-ttu-id="cb89e-130">¿Qué tipo de pan le gustaría?</span><span class="sxs-lookup"><span data-stu-id="cb89e-130">What kind of bread would you like?</span></span> <span data-ttu-id="cb89e-131">Tenemos pan blanco, de trigo o centeno".</span><span class="sxs-lookup"><span data-stu-id="cb89e-131">We have white, wheat, or rye."</span></span> <span data-ttu-id="cb89e-132">De este modo, el usuario sabe cómo responder y recibe indicaciones de navegación a través de la conversación.</span><span class="sxs-lookup"><span data-stu-id="cb89e-132">That way, the user knows how to respond and is given navigational cues through the conversation.</span></span>

## <a name="other-considerations"></a><span data-ttu-id="cb89e-133">Otras consideraciones</span><span class="sxs-lookup"><span data-stu-id="cb89e-133">Other considerations</span></span>

<span data-ttu-id="cb89e-134">Además de proporcionar una primera interacción intuitiva y fácil de navegar, un bot bien diseñado proporciona al usuario acceso a información sobre su directiva de privacidad y sus términos de uso.</span><span class="sxs-lookup"><span data-stu-id="cb89e-134">In addition to providing an intuitive and easily navigated first interaction, a well-designed bot provides the user with access to information about its privacy policy and terms of use.</span></span> 

> [!TIP]
> <span data-ttu-id="cb89e-135">Si el bot recopila información personal del usuario, es importante que se transmita y se describa lo que se hará con los datos.</span><span class="sxs-lookup"><span data-stu-id="cb89e-135">If your bot collects personal data from the user, it's important to convey that and to describe what will be done with the data.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cb89e-136">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="cb89e-136">Next steps</span></span>

<span data-ttu-id="cb89e-137">Ahora que está familiarizado con algunos principios básicos del diseño de la primera interacción entre el usuario y el bot, obtenga más información sobre cómo [diseñar el flujo de la conversación](~/bot-service-design-conversation-flow.md).</span><span class="sxs-lookup"><span data-stu-id="cb89e-137">Now that you're familiar with some basic principles for designing the first interaction between user and bot, learn more about [designing the flow of conversation](~/bot-service-design-conversation-flow.md).</span></span>
