---
title: Reconocimiento de intenciones a partir del contenido del mensaje | Microsoft Docs
description: Aprenda a reconocer la intención del usuario mediante expresiones regulares o comprobando el contenido del mensaje.
author: DeniseMak
ms.author: v-demak
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: 67dc8a196393458b37bb6447ceaa8f36a28a564a
ms.sourcegitcommit: 2dc75701b169d822c9499e393439161bc87639d2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42905936"
---
# <a name="recognize-user-intent-from-message-content"></a><span data-ttu-id="52faa-103">Reconocimiento de intenciones del usuario a partir del contenido del mensaje</span><span class="sxs-lookup"><span data-stu-id="52faa-103">Recognize user intent from message content</span></span>

[!INCLUDE [pre-release-label](../includes/pre-release-label-v3.md)]

<span data-ttu-id="52faa-104">Cuando su bot recibe un mensaje de un usuario, el bot puede usar un **reconocedor** para examinar el mensaje y determinar la intención.</span><span class="sxs-lookup"><span data-stu-id="52faa-104">When your bot receives a message from a user, your bot can use a **recognizer** to examine the message and determine intent.</span></span> <span data-ttu-id="52faa-105">La intención proporciona una asignación de mensajes a diálogos que se puede invocar.</span><span class="sxs-lookup"><span data-stu-id="52faa-105">The intent provides a mapping from messages to dialogs to invoke.</span></span> <span data-ttu-id="52faa-106">En este artículo se explica cómo reconocer la intención mediante expresiones regulares o inspeccionando el contenido de un mensaje.</span><span class="sxs-lookup"><span data-stu-id="52faa-106">This article explains how to recognize intent using regular expressions or by inspecting the content of a message.</span></span> <span data-ttu-id="52faa-107">Por ejemplo, un bot puede utilizar expresiones regulares para comprobar si un mensaje contiene la palabra "ayuda" e invocar un diálogo de ayuda.</span><span class="sxs-lookup"><span data-stu-id="52faa-107">For example, a bot can use regular expressions to check if a message contains the word "help" and invoke a help dialog.</span></span> <span data-ttu-id="52faa-108">Un bot también puede comprobar las propiedades del mensaje del usuario, por ejemplo, para ver si el usuario envió una imagen en lugar de texto e invocar un diálogo de procesamiento de imágenes.</span><span class="sxs-lookup"><span data-stu-id="52faa-108">A bot can also check the properties of the user message, for example, to see if the user sent an image instead of text and invoke an image processing dialog.</span></span> 

> [!NOTE]
> <span data-ttu-id="52faa-109">Para más información acerca del reconocimiento de intenciones mediante LUIS, consulte [Reconocimiento de intenciones y entidades con LUIS](bot-builder-nodejs-recognize-intent-luis.md).</span><span class="sxs-lookup"><span data-stu-id="52faa-109">For information on recognizing intent using LUIS, see [Recognize intents and entities with LUIS](bot-builder-nodejs-recognize-intent-luis.md)</span></span> 


## <a name="use-the-built-in-regular-expression-recognizer"></a><span data-ttu-id="52faa-110">Uso del reconocedor de expresiones regulares integrado</span><span class="sxs-lookup"><span data-stu-id="52faa-110">Use the built-in regular expression recognizer</span></span>
<span data-ttu-id="52faa-111">Use [RegExpRecognizer][RegExpRecognizer] para detectar la intención del usuario mediante una expresión regular.</span><span class="sxs-lookup"><span data-stu-id="52faa-111">Use [RegExpRecognizer][RegExpRecognizer] to detect the user's intent using a regular expression.</span></span> <span data-ttu-id="52faa-112">Puede pasar varias expresiones al reconocedor para admitir varios idiomas.</span><span class="sxs-lookup"><span data-stu-id="52faa-112">You can pass multiple expressions to the recognizer to support multiple languages.</span></span> 

> [!TIP]
> <span data-ttu-id="52faa-113">Consulte [Compatibilidad con la localización](bot-builder-nodejs-localization.md) para más información sobre la localización del bot.</span><span class="sxs-lookup"><span data-stu-id="52faa-113">See [Support localization](bot-builder-nodejs-localization.md) for more information on localizing your bot.</span></span>

<span data-ttu-id="52faa-114">El código siguiente crea un reconocedor de expresiones regulares denominado `CancelIntent` y lo agrega al bot.</span><span class="sxs-lookup"><span data-stu-id="52faa-114">The following code creates a regular expression recognizer named `CancelIntent` and adds it to your bot.</span></span> <span data-ttu-id="52faa-115">El reconocedor de este ejemplo proporciona expresiones regulares para las configuraciones regionales `en_us` y `ja_jp`.</span><span class="sxs-lookup"><span data-stu-id="52faa-115">The recognizer in this example provides regular expressions for both the `en_us` and `ja_jp` locales.</span></span> 

[!code-js[Add a regular expression recognizer (JavaScript)](../includes/code/node-regex-recognizer.js#addRegexRecognizer)]

<span data-ttu-id="52faa-116">Una vez que se agrega el reconocedor al bot, adjunte una acción [triggerAction][triggerAction] al diálogo que desea que el bot invoque cuando el reconocedor detecte la intención.</span><span class="sxs-lookup"><span data-stu-id="52faa-116">Once the recognizer is added to your bot, attach a [triggerAction][triggerAction] to the dialog that you want the bot to invoke when the recognizer detects the intent.</span></span> <span data-ttu-id="52faa-117">Use la opción [coincidencias][matches] para especificar el nombre de la intención, tal como se muestra en el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="52faa-117">Use the [matches][matches] option to specify the intent name, as shown in the following code:</span></span>

[!code-js[Map the CancelIntent recognizer to a cancel dialog (JavaScript)](../includes/code/node-regex-recognizer.js#bindCancelDialogToRegexRecognizer)]

<span data-ttu-id="52faa-118">Los reconocedores de intenciones son globales, lo que significa que el reconocedor se ejecutará para cada mensaje recibido del usuario.</span><span class="sxs-lookup"><span data-stu-id="52faa-118">Intent recognizers are global, which means that the recognizer will run for every message received from the user.</span></span> <span data-ttu-id="52faa-119">Si un reconocedor detecta una intención que está enlazada a un diálogo mediante `triggerAction`, puede desencadenar la interrupción del diálogo que está activo actualmente.</span><span class="sxs-lookup"><span data-stu-id="52faa-119">If a recognizer detects an intent that is bound to a dialog using a `triggerAction`, it can trigger interruption of the currently active dialog.</span></span> <span data-ttu-id="52faa-120">Permitir y controlar interrupciones es un diseño flexible que representa lo que el usuario hace realmente.</span><span class="sxs-lookup"><span data-stu-id="52faa-120">Allowing and handling interruptions is a flexible design that accounts for what users really do.</span></span>

> [!TIP] 
> <span data-ttu-id="52faa-121">Para aprender como funciona un `triggerAction` con diálogos, consulte [Administración del flujo de conversación](bot-builder-nodejs-manage-conversation-flow.md).</span><span class="sxs-lookup"><span data-stu-id="52faa-121">To learn how a `triggerAction` works with dialogs, see [Managing conversation flow](bot-builder-nodejs-manage-conversation-flow.md).</span></span> <span data-ttu-id="52faa-122">Para más información sobre las diversas acciones que puede asociar con una intención reconocida, consulte [Controlar acciones del usuario](bot-builder-nodejs-dialog-actions.md).</span><span class="sxs-lookup"><span data-stu-id="52faa-122">To learn more about the various actions you can associate with a recognized intent, [Handle user actions](bot-builder-nodejs-dialog-actions.md).</span></span>

## <a name="register-a-custom-intent-recognizer"></a><span data-ttu-id="52faa-123">Registro de un reconocedor de intenciones personalizado</span><span class="sxs-lookup"><span data-stu-id="52faa-123">Register a custom intent recognizer</span></span>
<span data-ttu-id="52faa-124">También puede implementar un reconocedor personalizado.</span><span class="sxs-lookup"><span data-stu-id="52faa-124">You can also implement a custom recognizer.</span></span> <span data-ttu-id="52faa-125">En este ejemplo se agrega un reconocedor simple que comprueba si el usuario ha dicho "ayuda" o "adiós", pero podría agregar fácilmente un reconocedor que realizara un procesamiento más complejo como, por ejemplo, uno que reconozca cuándo envía el usuario una imagen.</span><span class="sxs-lookup"><span data-stu-id="52faa-125">This example adds a simple recognizer that looks for the user to say 'help' or 'goodbye' but you could easily add a recognizer that does more complex processing, such as one that recognizes when the user sends an image.</span></span> 


[!code-js[Add a custom recognizer (JavaScript)](../includes/code/node-howto-recognize-intent.js#addCustomRecognizer)]

<span data-ttu-id="52faa-126">Una vez que haya registrado un reconocedor, puede asociarlo con una acción mediante una cláusula `matches`.</span><span class="sxs-lookup"><span data-stu-id="52faa-126">Once you've registered a recognizer, you can associate the recognizer with an action using a `matches` clause.</span></span>

[!code-js[Bind intents to actions (JavaScript)](../includes/code/node-howto-recognize-intent.js#bindIntentsToActions)]

## <a name="disambiguate-between-multiple-intents"></a><span data-ttu-id="52faa-127">Eliminación de ambigüedad entre varias intenciones</span><span class="sxs-lookup"><span data-stu-id="52faa-127">Disambiguate between multiple intents</span></span>

<span data-ttu-id="52faa-128">El bot puede registrar más de un reconocedor.</span><span class="sxs-lookup"><span data-stu-id="52faa-128">Your bot can register more than one recognizer.</span></span> <span data-ttu-id="52faa-129">Tenga en cuenta que el ejemplo del reconocedor personalizado incluye la asignación de una puntuación numérica a cada intención.</span><span class="sxs-lookup"><span data-stu-id="52faa-129">Notice that the custom recognizer example involves assigning a numerical score to each intent.</span></span> <span data-ttu-id="52faa-130">Esto se hace porque el bot puede tener más de un reconocedor y Bot Builder SDK proporciona una lógica integrada para eliminar la ambigüedad entre las intenciones que devuelven los diversos reconocedores.</span><span class="sxs-lookup"><span data-stu-id="52faa-130">This is done since your bot may have more than one recognizer, and the Bot Builder SDK provides built-in logic to disambiguate between intents returned by multiple recognizers.</span></span> <span data-ttu-id="52faa-131">La puntuación asignada a una intención suele estar comprendida entre 0.0 y 1.0, pero un reconocedor personalizado puede definir una intención superior a 1.1 para asegurarse de que la lógica de eliminación de ambigüedades de Bot Builder SDK la elija siempre.</span><span class="sxs-lookup"><span data-stu-id="52faa-131">The score assigned to an intent is typically between 0.0 and 1.0, but a custom recognizer may define an intent greater than 1.1 to ensure that that intent will always be chosen by the Bot Builder SDK's disambiguation logic.</span></span> 

<span data-ttu-id="52faa-132">De forma predeterminada, los reconocedores se ejecutan en paralelo, pero puede establecer recognizeOrder [IIntentRecognizerSetOptions][IntentRecognizerSetOptions] para que el proceso se cierre en cuanto el bot encuentre uno con una puntuación de 1.0.</span><span class="sxs-lookup"><span data-stu-id="52faa-132">By default, recognizers run in parallel, but you can set recognizeOrder in [IIntentRecognizerSetOptions][IntentRecognizerSetOptions] so the process quits as soon as your bot finds one that gives a score of 1.0.</span></span>

<span data-ttu-id="52faa-133">Bot Builder SDK incluye un [ejemplo][DisambiguationSample] que muestra cómo proporcionar una lógica personalizada de eliminación de ambigüedades en su bot implementando [IDisambiguateRouteHandler][IDisambiguateRouteHandler].</span><span class="sxs-lookup"><span data-stu-id="52faa-133">The Bot Builder SDK includes a [sample][DisambiguationSample] that demonstrates how to provide custom disambiguation logic in your bot by implementing [IDisambiguateRouteHandler][IDisambiguateRouteHandler].</span></span>

## <a name="next-steps"></a><span data-ttu-id="52faa-134">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="52faa-134">Next steps</span></span>
<span data-ttu-id="52faa-135">La lógica para usar expresiones regulares e inspeccionar contenidos de mensajes puede ser compleja, especialmente si el flujo de conversación del bot es de final abierto.</span><span class="sxs-lookup"><span data-stu-id="52faa-135">The logic for using regular expressions and inspecting message contents can become complex, especially if your bot's conversational flow is open-ended.</span></span> <span data-ttu-id="52faa-136">Para ayudar a su bot a controlar una amplia variedad de entradas de texto y voz de los usuarios, puede usar un servicio de reconocimiento de intenciones como [LUIS][LUIS] para agregar el reconocimiento del lenguaje natural al bot.</span><span class="sxs-lookup"><span data-stu-id="52faa-136">To help your bot handle a wider variety of textual and spoken input from users, you can use an intent recognition service like [LUIS][LUIS] to add natural language understanding to your bot.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="52faa-137">Reconocimiento de intenciones y entidades con LUIS</span><span class="sxs-lookup"><span data-stu-id="52faa-137">Recognize intents and entities with LUIS</span></span>](bot-builder-nodejs-recognize-intent-luis.md)


[LUIS]: https://www.luis.ai/

[triggerAction]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.dialog.html#triggeraction

[matches]: https://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.itriggeractionoptions.html#matches

[node-js-bot-how-to]: bot-builder-nodejs-recognize-intent-luis.md

[LUISAzureDocs]: /azure/cognitive-services/LUIS/Home

[IMessage]: http://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.imessage

[IntentRecognizerSetOptions]: https://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.iintentrecognizersetoptions.html

[LuisRecognizer]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.luisrecognizer

[LUISSample]: https://github.com/Microsoft/BotBuilder/blob/master/Node/examples/basics-naturalLanguage/app.js

[LUISConcepts]: https://docs.botframework.com/en-us/node/builder/guides/understanding-natural-language/

[DisambiguationSample]: https://github.com/Microsoft/BotBuilder/tree/master/Node/examples/feature-onDisambiguateRoute

[IDisambiguateRouteHandler]: https://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.idisambiguateroutehandler.html

[RegExpRecognizer]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.regexprecognizer.html

[AlarmBot]: https://github.com/Microsoft/BotBuilder/blob/master/Node/examples/basics-naturalLanguage/app.js

[LUISBotSample]: https://github.com/Microsoft/BotBuilder-Samples/tree/master/Node/intelligence-LUIS
