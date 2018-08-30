---
title: Creación de un bot habilitado por voz con habilidades de Cortana | Microsoft Docs
description: Obtenga información sobre cómo crear un bot habilitado por voz con habilidades de Cortana y Bot Builder SDK para Node.js.
author: DeniseMak
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: c213c1155b1eef5f5c776ba42a221d95b74f99a5
ms.sourcegitcommit: 2dc75701b169d822c9499e393439161bc87639d2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42906086"
---
# <a name="build-a-speech-enabled-bot-with-cortana-skills"></a><span data-ttu-id="7a57e-103">Creación de un bot habilitado para la voz con habilidades de Cortana</span><span class="sxs-lookup"><span data-stu-id="7a57e-103">Build a speech-enabled bot with Cortana skills</span></span>

[!INCLUDE [pre-release-label](../includes/pre-release-label-v3.md)]

> [!div class="op_single_selector"]
> - [.NET](../dotnet/bot-builder-dotnet-cortana-skill.md)
> - [Node.js](../nodejs/bot-builder-nodejs-cortana-skill.md)

<span data-ttu-id="7a57e-106">El SDK de Bot Builder para Node.js le permite crear un bot habilitado por voz al conectarlo al canal de Cortana como una habilidad de Cortana.</span><span class="sxs-lookup"><span data-stu-id="7a57e-106">The Bot Builder SDK for Node.js enables you to a build speech-enabled bot by connecting it to the Cortana channel as a Cortana skill.</span></span> <span data-ttu-id="7a57e-107">Las habilidades de Cortana permiten proporcionar funcionalidades mediante Cortana como respuesta a entradas de voz de un usuario.</span><span class="sxs-lookup"><span data-stu-id="7a57e-107">Cortana skills let you provide functionality through Cortana in response to spoken input from a user.</span></span>

> [!TIP]
> Para más información sobre qué es una habilidad y qué puede hacer, vea [The Cortana Skills Kit][CortanaGetStarted] (El kit de habilidades de Cortana).

<span data-ttu-id="7a57e-109">La creación de una habilidad de Cortana con Bot Framework requiere muy poco conocimiento específico de Cortana y se compone principalmente de la creación de un bot.</span><span class="sxs-lookup"><span data-stu-id="7a57e-109">Creating a Cortana skill using Bot Framework requires very little Cortana-specific knowledge and primarily consists of building a bot.</span></span> <span data-ttu-id="7a57e-110">Una de las diferencias principales respecto a otros bots que ha creado es que Cortana tiene componentes visuales y de audio.</span><span class="sxs-lookup"><span data-stu-id="7a57e-110">One of the key differences from other bots that you may have created is that Cortana has both visual and audio components.</span></span> <span data-ttu-id="7a57e-111">Para el componente visual, Cortana proporciona un área del lienzo para representar el contenido, como tarjetas.</span><span class="sxs-lookup"><span data-stu-id="7a57e-111">For the visual component, Cortana provides an area of the canvas for rendering content such as cards.</span></span> <span data-ttu-id="7a57e-112">Para el componente de audio, proporcione texto o SSML en los mensajes del bot para que Cortana los lea al usuario, dándole una voz.</span><span class="sxs-lookup"><span data-stu-id="7a57e-112">For the audio component, you provide text or SSML in your bot's messages, which Cortana reads to the user, giving your bot a voice.</span></span> 

> [!NOTE]
> Cortana está disponible en muchos dispositivos diferentes. Algunos tienen una pantalla mientras que otros, como los altavoces independientes, puede que no. Asegúrese de que su bot es capaz de actuar en ambos escenarios. Consulte las [entidades específicas de Cortana][CortanaSpecificEntities] para obtener información sobre cómo comprobar la información del dispositivo.

## <a name="adding-speech-to-your-bot"></a><span data-ttu-id="7a57e-117">Adición de voz al bot</span><span class="sxs-lookup"><span data-stu-id="7a57e-117">Adding speech to your bot</span></span>

<span data-ttu-id="7a57e-118">Los mensajes de voz de su bot se representan en el lenguaje de marcado de síntesis de voz (SSML).</span><span class="sxs-lookup"><span data-stu-id="7a57e-118">Spoken messages from your bot are represented as Speech Synthesis Markup Language (SSML).</span></span> <span data-ttu-id="7a57e-119">Bot Builder SDK permite incluir SSML en las respuestas del bot para controlar lo que dice el bot y lo que muestra.</span><span class="sxs-lookup"><span data-stu-id="7a57e-119">The Bot Builder SDK lets you include SSML in your bot's responses to control what the bot says, in addition to what it shows.</span></span>

### <a name="sessionsay"></a><span data-ttu-id="7a57e-120">session.say</span><span class="sxs-lookup"><span data-stu-id="7a57e-120">session.say</span></span>

<span data-ttu-id="7a57e-121">El bot usa el método **session.say** para hablar con el usuario, en lugar de **session.send**.</span><span class="sxs-lookup"><span data-stu-id="7a57e-121">Your bot uses the **session.say** method to speak to the user, in place of **session.send**.</span></span> <span data-ttu-id="7a57e-122">Incluye parámetros opcionales para enviar salidas de SSML, así como datos adjuntos como tarjetas.</span><span class="sxs-lookup"><span data-stu-id="7a57e-122">It includes optional parameters for sending SSML output, as well as attachments like cards.</span></span> 

<span data-ttu-id="7a57e-123">El método tiene este formato:</span><span class="sxs-lookup"><span data-stu-id="7a57e-123">The method has this format:</span></span>

```session.say(displayText: string, speechText: string, options?: object)```

| <span data-ttu-id="7a57e-124">Parámetro</span><span class="sxs-lookup"><span data-stu-id="7a57e-124">Parameter</span></span> | <span data-ttu-id="7a57e-125">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="7a57e-125">Description</span></span> |
|------|------|
| <span data-ttu-id="7a57e-126">**displayText**</span><span class="sxs-lookup"><span data-stu-id="7a57e-126">**displayText**</span></span> | <span data-ttu-id="7a57e-127">Un mensaje de texto para mostrar en la interfaz de usuario de Cortana.</span><span class="sxs-lookup"><span data-stu-id="7a57e-127">A textual message to display in Cortana's UI.</span></span>|
| <span data-ttu-id="7a57e-128">**speechText**</span><span class="sxs-lookup"><span data-stu-id="7a57e-128">**speechText**</span></span> | <span data-ttu-id="7a57e-129">El texto o SSML que Cortana lee al usuario.</span><span class="sxs-lookup"><span data-stu-id="7a57e-129">The text or SSML that Cortana reads to the user.</span></span> |
| <span data-ttu-id="7a57e-130">**options**</span><span class="sxs-lookup"><span data-stu-id="7a57e-130">**options**</span></span> | <span data-ttu-id="7a57e-131">Un objeto [IMessage][IMessage] que puede contener una sugerencia de entrada o datos adjuntos.</span><span class="sxs-lookup"><span data-stu-id="7a57e-131">An [IMessage][IMessage] object that can contain an attachment or input hint.</span></span> <span data-ttu-id="7a57e-132">Las sugerencias de entrada indican si el bot acepta, espera o ignora la entrada.</span><span class="sxs-lookup"><span data-stu-id="7a57e-132">Input hints indicate whether the bot is accepting, expecting, or ignoring input.</span></span> <span data-ttu-id="7a57e-133">Los datos adjuntos de la tarjeta se muestran en el lienzo de Cortana a continuación de la información **displayText**.</span><span class="sxs-lookup"><span data-stu-id="7a57e-133">Card attachments are displayed in Cortana’s canvas below the **displayText** information.</span></span>   |

<span data-ttu-id="7a57e-134">La propiedad **InputHint** ayuda a indicar a Cortana si su bot está esperando una entrada del usuario.</span><span class="sxs-lookup"><span data-stu-id="7a57e-134">The **inputHint** property helps indicate to Cortana whether your bot is expecting input.</span></span> <span data-ttu-id="7a57e-135">Si usa un mensaje integrado, este valor se establece automáticamente en el valor predeterminado **expectingInput**.</span><span class="sxs-lookup"><span data-stu-id="7a57e-135">If you're using a built-in prompt, this value is automatically set to the default of **expectingInput**.</span></span>


| <span data-ttu-id="7a57e-136">Valor</span><span class="sxs-lookup"><span data-stu-id="7a57e-136">Value</span></span> | <span data-ttu-id="7a57e-137">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="7a57e-137">Description</span></span> |
|------|------|
| <span data-ttu-id="7a57e-138">**acceptingInput**</span><span class="sxs-lookup"><span data-stu-id="7a57e-138">**acceptingInput**</span></span> | <span data-ttu-id="7a57e-139">El bot está listo para recibir información de forma pasiva, pero no está esperando una respuesta.</span><span class="sxs-lookup"><span data-stu-id="7a57e-139">Your bot is passively ready for input but is not waiting on a response.</span></span> <span data-ttu-id="7a57e-140">Cortana acepta la entrada del usuario si el usuario mantiene presionado el botón del micrófono.</span><span class="sxs-lookup"><span data-stu-id="7a57e-140">Cortana accepts input from the user if the user holds down the microphone button.</span></span>|
| <span data-ttu-id="7a57e-141">**expectingInput**</span><span class="sxs-lookup"><span data-stu-id="7a57e-141">**expectingInput**</span></span> | <span data-ttu-id="7a57e-142">Indica que el bot está esperando una respuesta del usuario de forma activa.</span><span class="sxs-lookup"><span data-stu-id="7a57e-142">Indicates that the bot is actively expecting a response from the user.</span></span> <span data-ttu-id="7a57e-143">Cortana escucha hasta que el usuario habla por el micrófono.</span><span class="sxs-lookup"><span data-stu-id="7a57e-143">Cortana listens for the user to speak into the microphone.</span></span>  |
| <span data-ttu-id="7a57e-144">**ignoringInput**</span><span class="sxs-lookup"><span data-stu-id="7a57e-144">**ignoringInput**</span></span> | <span data-ttu-id="7a57e-145">Cortana está ignorando las entradas.</span><span class="sxs-lookup"><span data-stu-id="7a57e-145">Cortana is ignoring input.</span></span> <span data-ttu-id="7a57e-146">Su bot puede enviar esta sugerencia si se está procesando activamente una solicitud e ignorará la entrada de los usuarios hasta que se complete la solicitud.</span><span class="sxs-lookup"><span data-stu-id="7a57e-146">Your bot may send this hint if it is actively processing a request and will ignore input from users until the request is complete.</span></span>  |


<span data-ttu-id="7a57e-147">El ejemplo siguiente muestra cómo Cortana lee texto sin formato o SSML:</span><span class="sxs-lookup"><span data-stu-id="7a57e-147">The following example shows how Cortana reads plain text or SSML:</span></span>

```javascript
// Have Cortana read plain text
session.say('This is the text that Cortana displays', 'This is the text that is spoken by Cortana.');

// Have Cortana read SSML
session.say('This is the text that Cortana displays', '<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis" xml:lang="en-US">This is the text that is spoken by Cortana.</speak>');
```

<span data-ttu-id="7a57e-148">Este ejemplo muestra cómo informar a Cortana que se espera la intervención del usuario.</span><span class="sxs-lookup"><span data-stu-id="7a57e-148">This example shows how to let Cortana know that user input is expected.</span></span> <span data-ttu-id="7a57e-149">El micrófono quedará encendido.</span><span class="sxs-lookup"><span data-stu-id="7a57e-149">The microphone will be left open.</span></span>
```javascript
// Add an InputHint to let Cortana know to expect user input
session.say('Hi there', 'Hi, what’s your name?', {
    inputHint: builder.InputHint.expectingInput
});
```
<!-- TODO: tip about time limit and batching -->


### <a name="prompts"></a><span data-ttu-id="7a57e-150">Mensajes</span><span class="sxs-lookup"><span data-stu-id="7a57e-150">Prompts</span></span>

<span data-ttu-id="7a57e-151">Además de usar el método **session.say()**, también puede pasar texto o SSML a mensajes integrados con las opciones **speak** y **retrySpeak**.</span><span class="sxs-lookup"><span data-stu-id="7a57e-151">In addition to using the **session.say()** method you can also pass text or SSML to built-in prompts using the **speak** and **retrySpeak** options.</span></span>  

```javascript
builder.Prompts.text(session, 'text based prompt', {                                    
    speak: 'Cortana reads this out initially',                                               
    retrySpeak: 'This message is repeated by Cortana after waiting a while for user input',  
    inputHint: builder.InputHint.expectingInput                                              
});
```



<!-- TODO: Link to SSML library -->

<span data-ttu-id="7a57e-152">Para presentar al usuario una lista de opciones, utilice **Prompts.choice**.</span><span class="sxs-lookup"><span data-stu-id="7a57e-152">To present the user with a list of choices, use **Prompts.choice**.</span></span> <span data-ttu-id="7a57e-153">La opción **synonyms** permite tener más flexibilidad en el reconocimiento de grabaciones de voz del usuario.</span><span class="sxs-lookup"><span data-stu-id="7a57e-153">The **synonyms** option allows for more flexibility in recognizing user utterances.</span></span> <span data-ttu-id="7a57e-154">La opción **value** se devuelve en **results.response.entity**.</span><span class="sxs-lookup"><span data-stu-id="7a57e-154">The **value** option is returned in **results.response.entity**.</span></span> <span data-ttu-id="7a57e-155">La opción **action** especifica la etiqueta que el bot muestra para la opción.</span><span class="sxs-lookup"><span data-stu-id="7a57e-155">The **action** option specifies the label that your bot displays for the choice.</span></span>

<span data-ttu-id="7a57e-156">**Prompts.choice** admite opciones ordinales.</span><span class="sxs-lookup"><span data-stu-id="7a57e-156">**Prompts.choice** supports ordinal choices.</span></span> <span data-ttu-id="7a57e-157">Esto significa que el usuario puede decir "el primero", "el segundo" o "el tercero" para elegir un elemento de una lista.</span><span class="sxs-lookup"><span data-stu-id="7a57e-157">This means that the user can say "the first", "the second" or "the third" to choose an item in a list.</span></span> <span data-ttu-id="7a57e-158">Por ejemplo, dado el siguiente mensaje, si el usuario pide a Cortana "la segunda opción", el mensaje devolverá el valor 8.</span><span class="sxs-lookup"><span data-stu-id="7a57e-158">For example, given the following prompt, if the user asked Cortana for "the second option", the prompt will return the value of 8.</span></span>

```javascript
        var choices = [
            { value: '4', action: { title: '4 Sides' }, synonyms: 'four|for|4 sided|4 sides' },
            { value: '8', action: { title: '8 Sides' }, synonyms: 'eight|ate|8 sided|8 sides' },
            { value: '12', action: { title: '12 Sides' }, synonyms: 'twelve|12 sided|12 sides' },
            { value: '20', action: { title: '20 Sides' }, synonyms: 'twenty|20 sided|20 sides' },
        ];
        builder.Prompts.choice(session, 'choose_sides', choices, { 
            speak: speak(session, 'choose_sides_ssml') // use helper function to format SSML
        });
```

<span data-ttu-id="7a57e-159">En el ejemplo anterior, el SSML de la propiedad **speak** del mensaje adopta el formato con el uso de cadenas almacenadas en un archivo de mensajes localizados con el formato siguiente.</span><span class="sxs-lookup"><span data-stu-id="7a57e-159">In the previous example, the SSML for the prompt's **speak** property is formatted by using strings stored in a localized prompts file with the following format.</span></span> 

```json
{
    "choose_sides": "__Number of Sides__",
    "choose_sides_ssml": [
        "How many sides on the dice?",
        "Pick your poison.",
        "All the standard sizes are supported."
    ]
}
```


<span data-ttu-id="7a57e-160">Una función de la aplicación auxiliar compila después el elemento raíz necesario del documento de lenguaje de marcado de síntesis de voz (SSML).</span><span class="sxs-lookup"><span data-stu-id="7a57e-160">A helper function then builds the required root element of a Speech Synthesis Markup Language (SSML) document.</span></span> 

```javascript

module.exports.speak = function (template, params, options) {
    options = options || {};
    var output = '<speak xmlns="http://www.w3.org/2001/10/synthesis" ' +
        'version="' + (options.version || '1.0') + '" ' +
        'xml:lang="' + (options.lang || 'en-US') + '">';
    output += module.exports.vsprintf(template, params);
    output += '</speak>';
    return output;
}
```

> [!TIP]
> Puede encontrar un módulo de utilidad pequeño (ssml.js) para generar respuestas basadas en SSML del bot en la [habilidad de ejemplo Roller](https://github.com/Microsoft/BotBuilder-Samples/tree/master/Node/demo-RollerSkill).
> También hay varias bibliotecas útiles de SSML disponibles en [npm](https://www.npmjs.com/search?q=ssml) que facilitan la creación de SSML con un formato adecuado.

## <a name="display-cards-in-cortana"></a><span data-ttu-id="7a57e-163">Visualización de tarjetas en Cortana</span><span class="sxs-lookup"><span data-stu-id="7a57e-163">Display cards in Cortana</span></span>

<span data-ttu-id="7a57e-164">Además de las respuestas de voz, Cortana puede mostrar datos adjuntos de tarjeta.</span><span class="sxs-lookup"><span data-stu-id="7a57e-164">In addition to spoken responses, Cortana can also display card attachments.</span></span> <span data-ttu-id="7a57e-165">Cortana es compatible con las siguientes tarjetas enriquecidas:</span><span class="sxs-lookup"><span data-stu-id="7a57e-165">Cortana supports the following rich cards:</span></span>
* [<span data-ttu-id="7a57e-166">HeroCard</span><span class="sxs-lookup"><span data-stu-id="7a57e-166">HeroCard</span></span>](https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.herocard.html)
* [<span data-ttu-id="7a57e-167">ReceiptCard</span><span class="sxs-lookup"><span data-stu-id="7a57e-167">ReceiptCard</span></span>](https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.receiptcard.html)
* [<span data-ttu-id="7a57e-168">ThumbnailCard</span><span class="sxs-lookup"><span data-stu-id="7a57e-168">ThumbnailCard</span></span>](https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.thumbnailcard.html)

<span data-ttu-id="7a57e-169">Consulte los [procedimientos recomendados para el diseño de tarjetas][CardDesign] para ver el diseño de estas tarjetas en Cortana.</span><span class="sxs-lookup"><span data-stu-id="7a57e-169">See [Card design best practices][CardDesign] to see what these cards look like inside Cortana.</span></span> <span data-ttu-id="7a57e-170">Para obtener un ejemplo de cómo agregar una tarjeta enriquecida a un bot, vea [Envío de tarjetas enriquecidas](bot-builder-nodejs-send-rich-cards.md).</span><span class="sxs-lookup"><span data-stu-id="7a57e-170">For an example of how to add a rich card to a bot, see [Send rich cards](bot-builder-nodejs-send-rich-cards.md).</span></span> 

<span data-ttu-id="7a57e-171">El código siguiente muestra cómo agregar las propiedades **speak** y **inputHint** a un mensaje que contiene una tarjeta Hero.</span><span class="sxs-lookup"><span data-stu-id="7a57e-171">The following code demonstrates how to add the **speak** and **inputHint** properties to a message containing a Hero card.</span></span>

```javascript 

bot.dialog('HelpDialog', function (session) {
    var card = new builder.HeroCard(session)
        .title('help_title')
        .buttons([
            builder.CardAction.imBack(session, 'roll some dice', 'Roll Dice'),
            builder.CardAction.imBack(session, 'play yahtzee', 'Play Yahtzee')
        ]);
    var msg = new builder.Message(session)
        .speak(speak(session, 'I\'m roller, the dice rolling bot. You can say \'roll some dice\''))
        .addAttachment(card)
        .inputHint(builder.InputHint.acceptingInput); // Tell Cortana to accept input
    session.send(msg).endDialog();
}).triggerAction({ matches: /help/i });


/** This helper function builds the required root element of a Speech Synthesis Markup Language (SSML) document. */
module.exports.speak = function (template, params, options) {
    options = options || {};
    var output = '<speak xmlns="http://www.w3.org/2001/10/synthesis" ' +
        'version="' + (options.version || '1.0') + '" ' +
        'xml:lang="' + (options.lang || 'en-US') + '">';
    output += module.exports.vsprintf(template, params);
    output += '</speak>';
    return output;
}

```
## <a name="sample-rollerskill"></a><span data-ttu-id="7a57e-172">Ejemplo: RollerSkill</span><span class="sxs-lookup"><span data-stu-id="7a57e-172">Sample: RollerSkill</span></span>
<span data-ttu-id="7a57e-173">El código en las secciones siguientes procede de una habilidad de Cortana de ejemplo para lanzar los dados.</span><span class="sxs-lookup"><span data-stu-id="7a57e-173">The code in the following sections comes from a sample Cortana skill for rolling dice.</span></span> <span data-ttu-id="7a57e-174">Descargue el código completo para el bot desde el [repositorio de ejemplos de Bot Builder](https://github.com/Microsoft/BotBuilder-Samples/tree/master/Node/demo-RollerSkill).</span><span class="sxs-lookup"><span data-stu-id="7a57e-174">Download the full code for the bot from the [BotBuilder-Samples repository](https://github.com/Microsoft/BotBuilder-Samples/tree/master/Node/demo-RollerSkill).</span></span>

<span data-ttu-id="7a57e-175">La habilidad se invoca al decir el [nombre de invocación][InvocationNameGuidelines] a Cortana.</span><span class="sxs-lookup"><span data-stu-id="7a57e-175">You invoke the skill by saying its [invocation name][InvocationNameGuidelines] to Cortana.</span></span> <span data-ttu-id="7a57e-176">Para la habilidad Roller, después de que [agregue el bot al canal de Cortana][CortanaChannel] y lo registre como una habilidad de Cortana, puede invocarlo si dice a Cortana “Pedir a Roller” o “Pedir a Roller que lance los dados”.</span><span class="sxs-lookup"><span data-stu-id="7a57e-176">For the roller skill, after you [add the bot to the Cortana channel][CortanaChannel] and register it as a Cortana skill, you can invoke it by telling Cortana "Ask Roller" or "Ask Roller to roll dice".</span></span>

### <a name="explore-the-code"></a><span data-ttu-id="7a57e-177">Exploración del código</span><span class="sxs-lookup"><span data-stu-id="7a57e-177">Explore the code</span></span>

<span data-ttu-id="7a57e-178">El ejemplo RollerSkill empieza con la apertura de una tarjeta mediante algunos botones para decir al usuario qué opciones tiene a su disposición.</span><span class="sxs-lookup"><span data-stu-id="7a57e-178">The RollerSkill sample starts by opening a card with some buttons to tell the user which options are available to them.</span></span>

```javascript
/**
 *   Create your bot with a default message handler that receive messages from the user.
 * - This function is be called anytime the user's utterance isn't
 *   recognized by the other handlers in the bot.
 */
var bot = new builder.UniversalBot(connector, function (session) {
    // Just redirect to our 'HelpDialog'.
    session.replaceDialog('HelpDialog');
});

//...

bot.dialog('HelpDialog', function (session) {
    var card = new builder.HeroCard(session)
        .title('help_title')
        .buttons([
            builder.CardAction.imBack(session, 'roll some dice', 'Roll Dice'),
            builder.CardAction.imBack(session, 'play craps', 'Play Craps')
        ]);
    var msg = new builder.Message(session)
        .speak(speak(session, 'help_ssml'))
        .addAttachment(card)
        .inputHint(builder.InputHint.acceptingInput);
    session.send(msg).endDialog();
}).triggerAction({ matches: /help/i });
```

### <a name="prompt-the-user-for-input"></a><span data-ttu-id="7a57e-179">Solicitud de entrada al usuario</span><span class="sxs-lookup"><span data-stu-id="7a57e-179">Prompt the user for input</span></span>

<span data-ttu-id="7a57e-180">El diálogo siguiente configura un juego personalizado para que el bot lo juegue.</span><span class="sxs-lookup"><span data-stu-id="7a57e-180">The following dialog sets up a custom game for the bot to play.</span></span>  <span data-ttu-id="7a57e-181">Pregunta al usuario cuántos lados quiere que tengan los dados y cuántos quiere lanzar.</span><span class="sxs-lookup"><span data-stu-id="7a57e-181">It asks the user how many sides they want the dice to have and then how many should be rolled.</span></span> <span data-ttu-id="7a57e-182">Una vez creada la estructura del juego, la pasará a un "PlayGameDialog" independiente.</span><span class="sxs-lookup"><span data-stu-id="7a57e-182">Once it has built the game structure it will pass it to a separate 'PlayGameDialog'.</span></span>

<span data-ttu-id="7a57e-183">Para iniciar el diálogo, el controlador **triggerAction()** de este diálogo permite a un usuario decir algo como "Me gustaría lanzar algunos dados".</span><span class="sxs-lookup"><span data-stu-id="7a57e-183">To start the dialog, the **triggerAction()** handler on this dialog allows a user to say something like "I'd like to roll some dice".</span></span> <span data-ttu-id="7a57e-184">Usa una expresión regular para que coincida con la entrada del usuario, pero podría usar fácilmente solo una [intención de LUIS](./bot-builder-nodejs-recognize-intent-luis.md).</span><span class="sxs-lookup"><span data-stu-id="7a57e-184">It uses a regular expression to match the user's input but you could just as easily use a [LUIS intent](./bot-builder-nodejs-recognize-intent-luis.md).</span></span> 


```javascript


bot.dialog('CreateGameDialog', [
    function (session) {
        // Initialize game structure.
        // - dialogData gives us temporary storage of this data in between
        //   turns with the user.
        var game = session.dialogData.game = { 
            type: 'custom', 
            sides: null, 
            count: null,
            turns: 0
        };

        var choices = [
            { value: '4', action: { title: '4 Sides' }, synonyms: 'four|for|4 sided|4 sides' },
            { value: '6', action: { title: '6 Sides' }, synonyms: 'six|sex|6 sided|6 sides' },
            { value: '8', action: { title: '8 Sides' }, synonyms: 'eight|8 sided|8 sides' },
            { value: '10', action: { title: '10 Sides' }, synonyms: 'ten|10 sided|10 sides' },
            { value: '12', action: { title: '12 Sides' }, synonyms: 'twelve|12 sided|12 sides' },
            { value: '20', action: { title: '20 Sides' }, synonyms: 'twenty|20 sided|20 sides' },
        ];
        builder.Prompts.choice(session, 'choose_sides', choices, { 
            speak: speak(session, 'choose_sides_ssml') 
        });
    },
    function (session, results) {
        // Store users input
        // - The response comes back as a find result with index & entity value matched.
        var game = session.dialogData.game;
        game.sides = Number(results.response.entity);

        /**
         * Ask for number of dice.
         */
        var prompt = session.gettext('choose_count', game.sides);
        builder.Prompts.number(session, prompt, {
            speak: speak(session, 'choose_count_ssml'),
            minValue: 1,
            maxValue: 100,
            integerOnly: true
        });
    },
    function (session, results) {
        // Store users input
        // - The response is already a number.
        var game = session.dialogData.game;
        game.count = results.response;

        /**
         * Play the game we just created.
         * 
         * replaceDialog() ends the current dialog and start a new
         * one in its place. We can pass arguments to dialogs so we'll pass the
         * 'PlayGameDialog' the game we created.
         */
        session.replaceDialog('PlayGameDialog', { game: game });
    }
]).triggerAction({ matches: [
    /(roll|role|throw|shoot).*(dice|die|dye|bones)/i,
    /new game/i
 ]});
```

### <a name="render-results"></a><span data-ttu-id="7a57e-185">Representación de resultados</span><span class="sxs-lookup"><span data-stu-id="7a57e-185">Render results</span></span>

 <span data-ttu-id="7a57e-186">Este diálogo es nuestro bucle de juego principal.</span><span class="sxs-lookup"><span data-stu-id="7a57e-186">This dialog is our main game loop.</span></span> <span data-ttu-id="7a57e-187">El bot almacena la estructura del juego en **session.conversationData**, de tal forma que, cuando el usuario diga "volver a lanzar", podremos volver a lanzar el mismo conjunto de dados.</span><span class="sxs-lookup"><span data-stu-id="7a57e-187">The bot stores the game structure in **session.conversationData** so that should the user say "roll again" we can just re-roll the same set of dice again.</span></span>

```javascript

bot.dialog('PlayGameDialog', function (session, args) {
    // Get current or new game structure.
    var game = args.game || session.conversationData.game;
    if (game) {
        // Generate rolls
        var total = 0;
        var rolls = [];
        for (var i = 0; i < game.count; i++) {
            var roll = Math.floor(Math.random() * game.sides) + 1;
            if (roll > game.sides) {
                // Accounts for 1 in a million chance random() generated a 1.0
                roll = game.sides;
            }
            total += roll;
            rolls.push(roll);
        }

        // Format roll results
        var results = '';
        var multiLine = rolls.length > 5;
        for (var i = 0; i < rolls.length; i++) {
            if (i > 0) {
                results += ' . ';
            }
            results += rolls[i];
        }

        // Render results using a card
        var card = new builder.HeroCard(session)
            .subtitle(game.count > 1 ? 'card_subtitle_plural' : 'card_subtitle_singular', game)
            .buttons([
                builder.CardAction.imBack(session, 'roll again', 'Roll Again'),
                builder.CardAction.imBack(session, 'new game', 'New Game')
            ]);
        if (multiLine) {
            //card.title('card_title').text('\n\n' + results + '\n\n');
            card.text(results);
        } else {
            card.title(results);
        }
        var msg = new builder.Message(session).addAttachment(card);

        // Determine bots reaction for speech purposes
        var reaction = 'normal';
        var min = game.count;
        var max = game.count * game.sides;
        var score = total/max;
        if (score == 1.0) {
            reaction = 'best';
        } else if (score == 0) {
            reaction = 'worst';
        } else if (score <= 0.3) {
            reaction = 'bad';
        } else if (score >= 0.8) {
            reaction = 'good';
        }

        // Check for special craps rolls
        if (game.type == 'craps') {
            switch (total) {
                case 2:
                case 3:
                case 12:
                    reaction = 'craps_lose';
                    break;
                case 7:
                    reaction = 'craps_seven';
                    break;
                case 11:
                    reaction = 'craps_eleven';
                    break;
                default:
                    reaction = 'craps_retry';
                    break;
            }
        }

        // Build up spoken response
        var spoken = '';
        if (game.turn == 0) {
            spoken += session.gettext('start_' + game.type + '_game_ssml') + ' ';
        } 
        spoken += session.gettext(reaction + '_roll_reaction_ssml');
        msg.speak(ssml.speak(spoken));

        // Increment number of turns and store game to roll again
        game.turn++;
        session.conversationData.game = game;

        /**
         * Send card and bot's reaction to user. 
         */

        msg.inputHint(builder.InputHint.acceptingInput);
        session.send(msg).endDialog();
    } else {
        // User started session with "roll again" so let's just send them to
        // the 'CreateGameDialog'
        session.replaceDialog('CreateGameDialog');
    }
}).triggerAction({ matches: /(roll|role|throw|shoot) again/i });
```

## <a name="next-steps"></a><span data-ttu-id="7a57e-188">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="7a57e-188">Next steps</span></span>
<span data-ttu-id="7a57e-189">Si su bot se ejecuta localmente o se implementa en la nube, puede invocarlo desde Cortana.</span><span class="sxs-lookup"><span data-stu-id="7a57e-189">If you have a bot running locally or deployed in the cloud, you can invoke it from Cortana.</span></span> <span data-ttu-id="7a57e-190">Consulte [Prueba de una habilidad de Cortana](../bot-service-debug-cortana-skill.md) para conocer los pasos necesarios para probar sus habilidades de Cortana.</span><span class="sxs-lookup"><span data-stu-id="7a57e-190">See [Test a Cortana skill](../bot-service-debug-cortana-skill.md) for the steps required to try out your Cortana Skill.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="7a57e-191">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="7a57e-191">Additional resources</span></span>
* <span data-ttu-id="7a57e-192">[Kit de habilidades de Cortana][CortanaGetStarted]</span><span class="sxs-lookup"><span data-stu-id="7a57e-192">[The Cortana Skills Kit][CortanaGetStarted]</span></span>
* [<span data-ttu-id="7a57e-193">Incorporación de voz a mensajes</span><span class="sxs-lookup"><span data-stu-id="7a57e-193">Add speech to messages</span></span>](bot-builder-nodejs-text-to-speech.md)
* <span data-ttu-id="7a57e-194">[Referencias de SSML][SSMLRef]</span><span class="sxs-lookup"><span data-stu-id="7a57e-194">[SSML Reference][SSMLRef]</span></span>
* <span data-ttu-id="7a57e-195">[Procedimientos recomendados para el diseño de voz de Cortana][VoiceDesign]</span><span class="sxs-lookup"><span data-stu-id="7a57e-195">[Voice design best practices for Cortana][VoiceDesign]</span></span>
* <span data-ttu-id="7a57e-196">[Procedimientos recomendados para el diseño de tarjetas de Cortana][CardDesign]</span><span class="sxs-lookup"><span data-stu-id="7a57e-196">[Card design best practices for Cortana][CardDesign]</span></span>
* <span data-ttu-id="7a57e-197">[Centro de desarrollo de Cortana][CortanaDevCenter]</span><span class="sxs-lookup"><span data-stu-id="7a57e-197">[Cortana Dev Center][CortanaDevCenter]</span></span>
* <span data-ttu-id="7a57e-198">[Procedimientos recomendados para probar y depurar a Cortana][Cortana-TestBestPractice]</span><span class="sxs-lookup"><span data-stu-id="7a57e-198">[Testing and debugging best practices for Cortana][Cortana-TestBestPractice]</span></span>


[CortanaGetStarted]: /cortana/getstarted
[BFPortal]: https://dev.botframework.com/


[SSMLRef]: https://aka.ms/cortana-ssml
[IMessage]: https://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.imessage.html
[Send]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.session#send
[CortanaDevCenter]: https://developer.microsoft.com/en-us/cortana

[CortanaSpecificEntities]: https://aka.ms/lgvcto
[CortanaAuth]: https://aka.ms/vsdqcj

[InvocationNameGuidelines]: https://aka.ms/cortana-invocation-guidelines
[VoiceDesign]: https://aka.ms/cortana-design-voice
[CardDesign]: https://aka.ms/cortana-design-card
[Cortana-Debug]: https://aka.ms/cortana-enable-debug
[Cortana-Publish]: https://aka.ms/cortana-publish


[CortanaTry]: https://aka.ms/try-cortana-bot
[CortanaChannel]: https://aka.ms/bot-cortana-channel
[Cortana-TestBestPractice]: https://aka.ms/cortana-test-best-practice
