---
title: Incorporación de voz a los mensajes | Microsoft Docs
description: Obtenga información sobre cómo agregar voz a los mensajes mediante Bot Builder SDK para Node.js.
author: v-ducvo
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: 58086bfb29846e39a219beb2f7f0e8d977415559
ms.sourcegitcommit: 2dc75701b169d822c9499e393439161bc87639d2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42904920"
---
# <a name="add-speech-to-messages"></a><span data-ttu-id="244d4-103">Incorporación de voz a los mensajes</span><span class="sxs-lookup"><span data-stu-id="244d4-103">Add speech to messages</span></span>

[!INCLUDE [pre-release-label](../includes/pre-release-label-v3.md)]

> [!div class="op_single_selector"]
> - [.NET](../dotnet/bot-builder-dotnet-text-to-speech.md)
> - [Node.js](../nodejs/bot-builder-nodejs-text-to-speech.md)
> - [REST](../rest-api/bot-framework-rest-connector-text-to-speech.md)

<span data-ttu-id="244d4-107">Si está creando un bot para un canal habilitado para voz, como Cortana, puede construir mensajes que especifiquen el texto que dirá el bot.</span><span class="sxs-lookup"><span data-stu-id="244d4-107">If you are building a bot for a speech-enabled channel such as Cortana, you can construct messages that specify the text to be spoken by your bot.</span></span> <span data-ttu-id="244d4-108">Para intentar influir en el estado del micrófono del cliente, puede especificar una [sugerencia de entrada](bot-builder-nodejs-send-input-hints.md) para indicar si el bot acepta, espera o ignora la entrada del usuario.</span><span class="sxs-lookup"><span data-stu-id="244d4-108">You can also attempt to influence the state of the client's microphone by specifying an [input hint](bot-builder-nodejs-send-input-hints.md) to indicate whether your bot is accepting, expecting, or ignoring user input.</span></span>

## <a name="specify-text-to-be-spoken-by-your-bot"></a><span data-ttu-id="244d4-109">Especificación del texto que dirá el bot</span><span class="sxs-lookup"><span data-stu-id="244d4-109">Specify text to be spoken by your bot</span></span>

<span data-ttu-id="244d4-110">Mediante Bot Builder SDK para Node.js, hay varias maneras de especificar el texto que dirá el bot en un canal habilitado para voz.</span><span class="sxs-lookup"><span data-stu-id="244d4-110">Using the Bot Builder SDK for Node.js, there are multiple ways to specify the text to be spoken by your bot on a speech-enabled channel.</span></span> <span data-ttu-id="244d4-111">Puede establecer la propiedad `IMessage.speak` y enviar el mensaje con el método `session.send()`, enviar el mensaje con el método `session.say()` (pasando los parámetros que especifican el texto para mostrar, el texto para hablar y las opciones) o enviar uno de los avisos integrados (especificando las opciones `speak` y `retrySpeak`).</span><span class="sxs-lookup"><span data-stu-id="244d4-111">You can set the `IMessage.speak` property and send the message using the `session.send()` method, send the message using the `session.say()` method (passing parameters that specify display text, speech text, and options), or send the message using a built-in prompt (specifying options `speak` and `retrySpeak`).</span></span>

### <a id="message-speak"></a> <span data-ttu-id="244d4-112">IMessage.speak</span><span class="sxs-lookup"><span data-stu-id="244d4-112">IMessage.speak</span></span> 

<span data-ttu-id="244d4-113">Si está creando un mensaje que se va a enviar mediante el método `session.send()`, establezca la propiedad `speak` para especificar el texto que dirá el bot.</span><span class="sxs-lookup"><span data-stu-id="244d4-113">If you are creating a message that will be sent using the `session.send()` method, set the `speak` property to specify the text to be spoken by your bot.</span></span> <span data-ttu-id="244d4-114">El siguiente ejemplo de código crea un mensaje que especifica el texto que se dirá e indica que el bot [espera la entrada del usuario](bot-builder-nodejs-send-input-hints.md).</span><span class="sxs-lookup"><span data-stu-id="244d4-114">The following code example creates a message that specifies text to be spoken and indicates that the bot is [accepting user input](bot-builder-nodejs-send-input-hints.md).</span></span>

[!code-javascript[IMessage.speak](../includes/code/node-text-to-speech.js#IMessageSpeak)]

### <a id="session-say"></a> <span data-ttu-id="244d4-115">session.say()</span><span class="sxs-lookup"><span data-stu-id="244d4-115">session.say()</span></span>

<span data-ttu-id="244d4-116">Como alternativa al uso de `session.send()`, puede llamar al método `session.say()` para crear y enviar un mensaje que especifica el texto que se dirá, además del texto que se mostrará y otras opciones.</span><span class="sxs-lookup"><span data-stu-id="244d4-116">As an alternative to using `session.send()`, you can call the `session.say()` method to create and send a message that specifies the text to be spoken, in addition to the text to be displayed and other options.</span></span> <span data-ttu-id="244d4-117">El método se define como sigue:</span><span class="sxs-lookup"><span data-stu-id="244d4-117">The method is defined as follows:</span></span>

`session.say(displayText: string, speechText: string, options?: object)`

| <span data-ttu-id="244d4-118">Parámetro</span><span class="sxs-lookup"><span data-stu-id="244d4-118">Parameter</span></span> | <span data-ttu-id="244d4-119">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="244d4-119">Description</span></span> |
|----|----|
| `displayText` | <span data-ttu-id="244d4-120">Texto para mostrar.</span><span class="sxs-lookup"><span data-stu-id="244d4-120">The text to be displayed.</span></span> |
| `speechText` | <span data-ttu-id="244d4-121">El texto (en texto sin formato o en formato <a href="https://msdn.microsoft.com/en-us/library/hh378377(v=office.14).aspx" target="_blank">SSML</a>) que el bot dirá.</span><span class="sxs-lookup"><span data-stu-id="244d4-121">The text (in plain text or <a href="https://msdn.microsoft.com/en-us/library/hh378377(v=office.14).aspx" target="_blank">SSML</a> format) to be spoken.</span></span> |
| `options` | <span data-ttu-id="244d4-122">Un objeto [IMessage][IMessage] que puede contener datos adjuntos o una [sugerencia de entrada](bot-builder-nodejs-send-input-hints.md).</span><span class="sxs-lookup"><span data-stu-id="244d4-122">An [IMessage][IMessage] object that can contain an attachment or [input hint](bot-builder-nodejs-send-input-hints.md).</span></span> |

<span data-ttu-id="244d4-123">El siguiente ejemplo de código envía un mensaje que especifica el texto que se mostrará y el texto que se dirá e indica que el bot [ignora la entrada del usuario](bot-builder-nodejs-send-input-hints.md).</span><span class="sxs-lookup"><span data-stu-id="244d4-123">The following code example sends a message that specifies text to be displayed and text to be spoken and indicates that the bot is [ignoring user input](bot-builder-nodejs-send-input-hints.md).</span></span>

[!code-javascript[Session.say()](../includes/code/node-text-to-speech.js#SessionSay)]

### <a id="prompt-options"></a> <span data-ttu-id="244d4-124">Opciones de aviso</span><span class="sxs-lookup"><span data-stu-id="244d4-124">Prompt options</span></span>

<span data-ttu-id="244d4-125">Con cualquiera de los avisos integrados, puede establecer las opciones `speak` y `retrySpeak` para especificar el texto que dirá el bot.</span><span class="sxs-lookup"><span data-stu-id="244d4-125">Using any of the built-in prompts, you can set the options `speak` and `retrySpeak` to specify the text to be spoken by your bot.</span></span> <span data-ttu-id="244d4-126">En el ejemplo de código siguiente se crea un aviso que especifica el texto que se mostrará, el texto que se dirá inicialmente y el texto que se dirá después de esperar un tiempo los datos de entrada del usuario.</span><span class="sxs-lookup"><span data-stu-id="244d4-126">The following code example creates a prompt that specifies text to be displayed, text to be spoken initially, and text to be spoken after waiting a while for user input.</span></span> <span data-ttu-id="244d4-127">Indica que el bot [espera la entrada del usuario](bot-builder-nodejs-send-input-hints.md) y usa el formato [SSML](#ssml) para especificar que la palabra "sure" (seguro) se debe decir con un énfasis moderado.</span><span class="sxs-lookup"><span data-stu-id="244d4-127">It indicates that the bot is [expecting user input](bot-builder-nodejs-send-input-hints.md) and uses [SSML](#ssml) formatting to specify that the word "sure" should be spoken with a moderate amount of emphasis.</span></span>

[!code-javascript[Prompt](../includes/code/node-text-to-speech.js#Prompt)]

## <a id="ssml"></a> <span data-ttu-id="244d4-128">Lenguaje de marcado de síntesis de voz (SSML)</span><span class="sxs-lookup"><span data-stu-id="244d4-128">Speech Synthesis Markup Language (SSML)</span></span>

<span data-ttu-id="244d4-129">Para especificar el texto que dirá el bot, puede usar una cadena de texto sin formato o en una cadena con formato de lenguaje de marcado de síntesis de voz (SSML), un lenguaje de marcado basado en XML que le permite controlar diversas características de la voz del bot, como la voz, la velocidad, el volumen, la pronunciación, el tono y mucho más.</span><span class="sxs-lookup"><span data-stu-id="244d4-129">To specify text to be spoken by your bot, you can use either a plain text string or a string that is formatted as Speech Synthesis Markup Language (SSML), an XML-based markup language that enables you to control various characteristics of your bot's speech such as voice, rate, volume, pronunciation, pitch, and more.</span></span> <span data-ttu-id="244d4-130">Para más información acerca de SSML, consulte <a href="https://msdn.microsoft.com/en-us/library/hh378377(v=office.14).aspx" target="_blank">Referencia del lenguaje de marcado de síntesis de voz</a>.</span><span class="sxs-lookup"><span data-stu-id="244d4-130">For details about SSML, see <a href="https://msdn.microsoft.com/en-us/library/hh378377(v=office.14).aspx" target="_blank">Speech Synthesis Markup Language Reference</a>.</span></span>

> [!TIP]
> Use una <a href="https://www.npmjs.com/search?q=ssml" target="_blank">biblioteca de SSML</a> para crear SSML con el formato correcto.

## <a name="input-hints"></a><span data-ttu-id="244d4-132">Sugerencias de entrada</span><span class="sxs-lookup"><span data-stu-id="244d4-132">Input hints</span></span>

<span data-ttu-id="244d4-133">Cuando envía un mensaje en un canal habilitado para voz, puede intentar influir en el estado del micrófono del cliente e incluir una sugerencia de entrada para indicar si el bot acepta, espera o ignora la entrada del usuario.</span><span class="sxs-lookup"><span data-stu-id="244d4-133">When you send a message on a speech-enabled channel, you can attempt to influence the state of the client's microphone by also including an input hint to indicate whether your bot is accepting, expecting, or ignoring user input.</span></span> <span data-ttu-id="244d4-134">Para más información, consulte [Incorporación de sugerencias de entrada a los mensajes](bot-builder-nodejs-send-input-hints.md).</span><span class="sxs-lookup"><span data-stu-id="244d4-134">For more information, see [Add input hints to messages](bot-builder-nodejs-send-input-hints.md).</span></span>

## <a name="sample-code"></a><span data-ttu-id="244d4-135">Código de ejemplo</span><span class="sxs-lookup"><span data-stu-id="244d4-135">Sample code</span></span> 

<span data-ttu-id="244d4-136">Para un ejemplo completo en el que se muestra cómo crear un bot habilitado para voz mediante Bot Builder SDK para. NET, consulte el ejemplo <a href="https://github.com/Microsoft/BotBuilder-Samples/tree/master/Node/demo-RollerSkill" target="_blank">Roller</a> en GitHub.</span><span class="sxs-lookup"><span data-stu-id="244d4-136">For a complete sample that shows how to create a speech-enabled bot using the Bot Builder SDK for .NET, see the <a href="https://github.com/Microsoft/BotBuilder-Samples/tree/master/Node/demo-RollerSkill" target="_blank">Roller sample</a> in GitHub.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="244d4-137">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="244d4-137">Additional resources</span></span>

- <span data-ttu-id="244d4-138"><a href="https://msdn.microsoft.com/en-us/library/hh378377(v=office.14).aspx" target="_blank">Lenguaje de marcado de síntesis de voz (SSML)</a></span><span class="sxs-lookup"><span data-stu-id="244d4-138"><a href="https://msdn.microsoft.com/en-us/library/hh378377(v=office.14).aspx" target="_blank">Speech Synthesis Markup Language (SSML)</a></span></span>
- <span data-ttu-id="244d4-139"><a href="https://github.com/Microsoft/BotBuilder-Samples/tree/master/Node/demo-RollerSkill" target="_blank">Ejemplo Roller (GitHub)</a></span><span class="sxs-lookup"><span data-stu-id="244d4-139"><a href="https://github.com/Microsoft/BotBuilder-Samples/tree/master/Node/demo-RollerSkill" target="_blank">Roller sample (GitHub)</a></span></span>
- <span data-ttu-id="244d4-140">[Referencia de Bot Builder SDK para Node.js][SDKReference]</span><span class="sxs-lookup"><span data-stu-id="244d4-140">[Bot Builder SDK for Node.js Reference][SDKReference]</span></span>

[SDKReference]: https://docs.botframework.com/en-us/node/builder/chat-reference/modules/_botbuilder_d_.html

[Message]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.message

[IMessage]: http://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.imessage
