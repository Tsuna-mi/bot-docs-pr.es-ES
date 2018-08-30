---
title: Creación de una habilidad de Cortana mediante Node.js | Microsoft Docs
description: Obtenga información sobre los conceptos básicos para la creación de una habilidad de Cortana en Bot Builder SDK para Node.js.
keywords: Bot Framework, habilidad de Cortana, voz, Node.js, Bot Builder, SDK, conceptos clave, conceptos básicos
author: DeniseMak
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: 38fa3811c079f07f847fbfb0fad1b9ed9f695c51
ms.sourcegitcommit: 2dc75701b169d822c9499e393439161bc87639d2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42905851"
---
# <a name="key-concepts-for-building-a-bot-for-cortana-skills-using-nodejs"></a><span data-ttu-id="aac18-104">Conceptos básicos para crear un bot para habilidades de Cortana con Node.js</span><span class="sxs-lookup"><span data-stu-id="aac18-104">Key concepts for building a bot for Cortana skills using Node.js</span></span>
 
[!INCLUDE [pre-release-label](../includes/pre-release-label-v3.md)]

> [!NOTE]
> <span data-ttu-id="aac18-105">Este artículo es contenido y se actualizará.</span><span class="sxs-lookup"><span data-stu-id="aac18-105">This article is preliminary content and will be updated.</span></span>

<span data-ttu-id="aac18-106">En este artículo se describen los conceptos clave para crear una habilidad de Cortana en Bot Builder SDK para Node.js.</span><span class="sxs-lookup"><span data-stu-id="aac18-106">This article introduces key concepts for building a Cortana skill in the Bot Builder SDK for Node.js.</span></span> 

## <a name="what-is-a-cortana-skill"></a><span data-ttu-id="aac18-107">¿Qué es una habilidad de Cortana?</span><span class="sxs-lookup"><span data-stu-id="aac18-107">What is a Cortana skill?</span></span>
<span data-ttu-id="aac18-108">Una habilidad de Cortana es un bot que se puede invocar con el uso de un cliente de Cortana, como el que está integrado en Windows 10.</span><span class="sxs-lookup"><span data-stu-id="aac18-108">A Cortana skill is a bot you can invoke by using a Cortana client, like the one built in to Windows 10.</span></span> <span data-ttu-id="aac18-109">El usuario lanza el bot diciendo algunas palabras clave o frases asociadas con el bot.</span><span class="sxs-lookup"><span data-stu-id="aac18-109">The user launches the bot by saying some keywords or phrases associated with the bot.</span></span> <span data-ttu-id="aac18-110">En el portal de Framework Bot puede configurar qué palabras clave se usan para lanzar el bot.</span><span class="sxs-lookup"><span data-stu-id="aac18-110">You use the Bot Framework Portal to configure which keywords are used to launch your bot.</span></span> 

<span data-ttu-id="aac18-111">Cortana se puede considerar como un canal habilitado para voz que puede enviar y recibir mensajes de voz, además de tener conversaciones por escrito.</span><span class="sxs-lookup"><span data-stu-id="aac18-111">Cortana can be thought of as speech-enabled channel that can send and receive voice messages in addition to textual conversation.</span></span> <span data-ttu-id="aac18-112">Un bot que se publica como una habilidad de Cortana debe diseñarse para voz y texto.</span><span class="sxs-lookup"><span data-stu-id="aac18-112">A bot that is published as a Cortana skill should be designed for speech as well as text.</span></span> <span data-ttu-id="aac18-113">Bot Framework proporciona métodos para especificar el lenguaje de marcado de síntesis de voz (SSML) para definir los mensajes de voz que su bot envía.</span><span class="sxs-lookup"><span data-stu-id="aac18-113">The Bot Framework provides methods for specifying Speech Synthesis Markup Language (SSML) to define spoken messages that your bot sends.</span></span>

## <a name="acknowledge-user-utterances"></a><span data-ttu-id="aac18-114">Reconocimiento de grabaciones de voz del usuario</span><span class="sxs-lookup"><span data-stu-id="aac18-114">Acknowledge user utterances</span></span> 

<!-- Establishing conversational understanding -->
<!-- Placeholder: In this section, describe how you have to write your speech to sound natural -->


<span data-ttu-id="aac18-115">Al crear un bot habilitado para voz, debe tratar de establecer fundamentos comunes y un entendimiento mutuo en la conversación.</span><span class="sxs-lookup"><span data-stu-id="aac18-115">When you create a speech-enabled bot, you should try to establish common ground and mutual understanding in the conversation.</span></span> <span data-ttu-id="aac18-116">El bot debe "fundamentar" las grabaciones de voz del usuario con la indicación de que se ha escuchado y entendido al usuario.</span><span class="sxs-lookup"><span data-stu-id="aac18-116">The bot should "ground" the user's utterances by indicating that the user was heard and understood.</span></span>

<span data-ttu-id="aac18-117">Los usuarios se sienten confundidos si se produce un error del sistema al fundamentar sus grabaciones de voz.</span><span class="sxs-lookup"><span data-stu-id="aac18-117">Users get confused if a system fails to ground their utterances.</span></span> <span data-ttu-id="aac18-118">Por ejemplo, la conversación siguiente puede ser un poco confusa cuando el bot pregunta "¿Qué es lo siguiente?":</span><span class="sxs-lookup"><span data-stu-id="aac18-118">For example, the following conversation can be a bit confusing when the bot asks "What's next?":</span></span>

```
Agent: Did you want to review some more of your profile?

User: No.

Agent: What's next?
```

<span data-ttu-id="aac18-119">Si el bot agrega "De acuerdo" como reconocimiento, es más descriptivo para el usuario:</span><span class="sxs-lookup"><span data-stu-id="aac18-119">If the bot adds an "Okay" as acknowledgment, it's friendlier for the user:</span></span>

```
Agent: Did you want to review some more of your profile?

User: No.

Agent: **Okay**, what's next?
```


<span data-ttu-id="aac18-120">Grados de fundamentación, desde el más débil al más sólido:</span><span class="sxs-lookup"><span data-stu-id="aac18-120">Degrees of grounding, from weakest to strongest:</span></span>
1. <span data-ttu-id="aac18-121">Atención continuada</span><span class="sxs-lookup"><span data-stu-id="aac18-121">Continued attention</span></span>
2. <span data-ttu-id="aac18-122">Próxima contribución pertinente</span><span class="sxs-lookup"><span data-stu-id="aac18-122">Next relevant contribution</span></span>
3. <span data-ttu-id="aac18-123">Reconocimiento: Respuesta mínima o continuador: "sí", "ajá", "de acuerdo" o "genial"</span><span class="sxs-lookup"><span data-stu-id="aac18-123">Acknowledgment: Minimal response or continuer: "yeah", "uh-huh", "okay", "great"</span></span>
4. <span data-ttu-id="aac18-124">Demostración: Indicar que se ha comprendido mediante reformulación o conclusión.</span><span class="sxs-lookup"><span data-stu-id="aac18-124">Demonstrate: Indicate understanding by reformulation, completion.</span></span>
5. <span data-ttu-id="aac18-125">Mostrar: Repetir todo o solo una parte.</span><span class="sxs-lookup"><span data-stu-id="aac18-125">Display: Repeat all or part.</span></span>

#### <a name="acknowledgement-and-next-relevant-contribution"></a><span data-ttu-id="aac18-126">Reconocimiento y próxima contribución pertinente</span><span class="sxs-lookup"><span data-stu-id="aac18-126">Acknowledgement and next relevant contribution</span></span>
<span data-ttu-id="aac18-127">Usuario: ... Tengo que viajar en mayo.</span><span class="sxs-lookup"><span data-stu-id="aac18-127">User: ... I need to travel in May.</span></span>
<span data-ttu-id="aac18-128">Agente: **Y**, ¿qué día de mayo quiere viajar?</span><span class="sxs-lookup"><span data-stu-id="aac18-128">Agent: **And**, what day in May did you want to travel?</span></span>
<span data-ttu-id="aac18-129">Usuario: Pues bien, tengo que estar allí del 12 al 15.</span><span class="sxs-lookup"><span data-stu-id="aac18-129">User: OK I need to be there from the 12th to the 15th?</span></span>
<span data-ttu-id="aac18-130">Agente: **Y**, ¿a qué ciudad va a viajar?</span><span class="sxs-lookup"><span data-stu-id="aac18-130">Agent: **And**, you're flying into what city?</span></span>

#### <a name="grounding-by-demonstration"></a><span data-ttu-id="aac18-131">Fundamentación mediante demostración</span><span class="sxs-lookup"><span data-stu-id="aac18-131">Grounding by demonstration</span></span>
<span data-ttu-id="aac18-132">Usuario: ... Tengo que viajar en mayo.</span><span class="sxs-lookup"><span data-stu-id="aac18-132">User: ... I need to travel in May.</span></span>
<span data-ttu-id="aac18-133">Agente: Y, ¿**quía día** de mayo quiere viajar?</span><span class="sxs-lookup"><span data-stu-id="aac18-133">Agent: And, **what day** in May did you want to travel?</span></span>
<span data-ttu-id="aac18-134">Usuario: Pues bien, tengo que estar allí del 12 al 15.</span><span class="sxs-lookup"><span data-stu-id="aac18-134">User: OK I need to be there from the 12th to the 15th?</span></span>
<span data-ttu-id="aac18-135">Agente: **Y**, ¿a qué ciudad va a viajar?</span><span class="sxs-lookup"><span data-stu-id="aac18-135">Agent: **And**, you're flying into what city?</span></span>


### <a name="closure"></a><span data-ttu-id="aac18-136">Cierre</span><span class="sxs-lookup"><span data-stu-id="aac18-136">Closure</span></span>

<span data-ttu-id="aac18-137">El bot que realiza una acción debe presentar pruebas de su correcta ejecución.</span><span class="sxs-lookup"><span data-stu-id="aac18-137">The bot performing an action should present evidence of successful performance.</span></span>
<span data-ttu-id="aac18-138">También es importante que indique si ha producido algún error o si se ha entendido bien o no.</span><span class="sxs-lookup"><span data-stu-id="aac18-138">It's also important to indicate failure or understanding.</span></span> 
* <span data-ttu-id="aac18-139">Cierre sin voz: Si inserta un botón elevador, su luz se enciende.</span><span class="sxs-lookup"><span data-stu-id="aac18-139">Non-speech closure: If you push an elevator button, its light turns on.</span></span>
<span data-ttu-id="aac18-140">Proceso de dos pasos:</span><span class="sxs-lookup"><span data-stu-id="aac18-140">Two step process:</span></span>
* <span data-ttu-id="aac18-141">Presentación</span><span class="sxs-lookup"><span data-stu-id="aac18-141">Presentation</span></span> 
* <span data-ttu-id="aac18-142">Aceptación</span><span class="sxs-lookup"><span data-stu-id="aac18-142">Acceptance</span></span>


### <a name="differences-in-content-presentation"></a><span data-ttu-id="aac18-143">Diferencias en la presentación del contenido</span><span class="sxs-lookup"><span data-stu-id="aac18-143">Differences in content presentation</span></span>
<span data-ttu-id="aac18-144">Al diseñar su bot habilitado para voz, tenga en cuenta que el diálogo hablado no suele ser el mismo que los mensajes de texto que el bot envía.</span><span class="sxs-lookup"><span data-stu-id="aac18-144">When designing your speech-enabled bot, keep in mind that that the spoken dialog is often not the same as the textual messages your bot sends.</span></span>
<!-- If there are differences in what the bot will say, in the text vs the speak fields of a prompt or in a waterfall, for example, discuss them here.

## Speech

You bot uses the **session.say** method to speak to the user. The speak method has three overloads:
* If you pass only one parameter to **session.say**, it can be a text parameter.
* If you pass two parameters to **session.say**, it can take text and SSML.
* If you pass three parameters, the third parameter takes an options structure that specifies all the options you can pass to build an **IMessage** object.

```javascript
var bot = new builder.UniversalBot(connector, function (session) {
    session.say("Hello... I'm a decision making bot.'.", 
        ssml.speak("Hello. I can help you answer all of life's tough questions."));
    session.replaceDialog('rootMenu');
});

```
## Speech in messages

The **IMessage** object provides a **speak** property for SSML. It can be used to play a .wav file.

The **inputHint** property helps indicate to Cortana whether your bot is expecting input. If you're using a built-in prompt, this value is automatically set to the default of **expectingInput**.

The **inputHint** property can take the following values: 
* **expectingInput**: Indicates that the bot is actively expecting a response from the user. Cortana listens for the user to speak into the microphone.
* **acceptingInput**: Indicates that the bot is passively ready for input but is not waiting on a response. Cortana accepts input from the user if the user holds down the microphone button.
* **ignoringInput**: Cortana is ignoring input. Your bot may send this hint if it is actively processing a request and will ignore input from users until the request is complete.

Prompts can take a `speak:` or `retrySpeak` option.

```javascript
        builder.Prompts.choice(session, "Decision Options", choices, {
            listStyle: builder.ListStyle.button,
            speak: ssml.speak("How would you like me to decide?")
        });
```

Prompts.number has *ordinal support*, meaning that you can say "the last", "the first", "the next-to-last" to choose an item in a list.




## Using synonyms

<!-- Axl Rose example -->     
```javascript   
         var choices = [
            { 
                value: 'flipCoinDialog',
                action: { title: "Flip A Coin" },
                synonyms: 'toss coin|flip quarter|toss quarter'
            },
            {
                value: 'rollDiceDialog',
                action: { title: "Roll Dice" },
                synonyms: 'roll die|shoot dice|shoot die'
            },
            {
                value: 'magicBallDialog',
                action: { title: "Magic 8-Ball" },
                synonyms: 'shake ball'
            },
            {
                value: 'quit',
                action: { title: "Quit" },
                synonyms: 'exit|stop|end'
            }
        ];
        builder.Prompts.choice(session, "Decision Options", choices, {
            listStyle: builder.ListStyle.button,
            speak: ssml.speak("How would you like me to decide?")
        });
```


## <a name="configuring-your-bot"></a><span data-ttu-id="aac18-145">Configuración del bot</span><span class="sxs-lookup"><span data-stu-id="aac18-145">Configuring your bot</span></span>

## <a name="prompts"></a><span data-ttu-id="aac18-146">Mensajes</span><span class="sxs-lookup"><span data-stu-id="aac18-146">Prompts</span></span>


## <a name="additional-resources"></a><span data-ttu-id="aac18-147">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="aac18-147">Additional resources</span></span>

[CortanaGetstarted]: /cortana/getstarted
[SSMLRef]: https://msdn.microsoft.com/en-us/library/hh378377(v=office.14).aspx
