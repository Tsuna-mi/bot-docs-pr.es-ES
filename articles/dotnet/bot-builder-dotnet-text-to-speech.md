---
title: Incorporación de voz a los mensajes | Microsoft Docs
description: Aprenda a agregar voz a los mensajes mediante Bot Builder SDK para .NET.
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: 0c972b913ab0639363a1e2f1307bcaa4bea60c54
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39306080"
---
# <a name="add-speech-to-messages"></a><span data-ttu-id="7b4a6-103">Incorporación de voz a los mensajes</span><span class="sxs-lookup"><span data-stu-id="7b4a6-103">Add speech to messages</span></span>
> [!div class="op_single_selector"]
> - [.NET](../dotnet/bot-builder-dotnet-text-to-speech.md)
> - [Node.js](../nodejs/bot-builder-nodejs-text-to-speech.md)
> - [REST](../rest-api/bot-framework-rest-connector-text-to-speech.md)

<span data-ttu-id="7b4a6-107">Si está creando un bot para un canal habilitado para voz, como Cortana, puede construir mensajes que especifiquen el texto que dirá el bot.</span><span class="sxs-lookup"><span data-stu-id="7b4a6-107">If you are building a bot for a speech-enabled channel such as Cortana, you can construct messages that specify the text to be spoken by your bot.</span></span> <span data-ttu-id="7b4a6-108">Para intentar influir en el estado del micrófono del cliente, puede especificar una [sugerencia de entrada](bot-builder-dotnet-add-input-hints.md) para indicar si el bot acepta, espera o ignora la entrada del usuario.</span><span class="sxs-lookup"><span data-stu-id="7b4a6-108">You can also attempt to influence the state of the client's microphone by specifying an [input hint](bot-builder-dotnet-add-input-hints.md) to indicate whether your bot is accepting, expecting, or ignoring user input.</span></span>

## <a name="specify-text-to-be-spoken-by-your-bot"></a><span data-ttu-id="7b4a6-109">Especificación del texto que dirá el bot</span><span class="sxs-lookup"><span data-stu-id="7b4a6-109">Specify text to be spoken by your bot</span></span>

<span data-ttu-id="7b4a6-110">Con Bot Builder SDK para. NET, hay varias maneras de especificar el texto que dirá el bot en un canal habilitado para voz.</span><span class="sxs-lookup"><span data-stu-id="7b4a6-110">Using the Bot Builder SDK for .NET, there are multiple ways to specify the text to be spoken by your bot on a speech-enabled channel.</span></span> <span data-ttu-id="7b4a6-111">Puede establecer la propiedad `Speak` del [mensaje][IMessageActivity], llamar al método `IDialogContext.SayAsync()` o especificar las opciones de aviso `speak` y `retrySpeak` al enviar un mensaje con un aviso integrado.</span><span class="sxs-lookup"><span data-stu-id="7b4a6-111">You can set the `Speak` property of the [message][IMessageActivity], call the `IDialogContext.SayAsync()` method, or specify prompt options `speak` and `retrySpeak` when sending a message using a built-in prompt.</span></span>

### <a id="message-speak"></a> <span data-ttu-id="7b4a6-112">IMessageActivity.Speak</span><span class="sxs-lookup"><span data-stu-id="7b4a6-112">IMessageActivity.Speak</span></span>

<span data-ttu-id="7b4a6-113">Si va a crear un [mensaje][IMessageActivity] y a configurar sus propiedades individuales, puede establecer la propiedad `Speak` del mensaje para especificar el texto que dirá el bot.</span><span class="sxs-lookup"><span data-stu-id="7b4a6-113">If you are creating a [message][IMessageActivity] and setting its individual properties, you can set the `Speak` property of the message to specify the text to be spoken by your bot.</span></span> <span data-ttu-id="7b4a6-114">El siguiente ejemplo de código crea un mensaje que especifica el texto que se mostrará y el texto que se dirá, e indica que el bot [acepta la entrada del usuario](bot-builder-dotnet-add-input-hints.md).</span><span class="sxs-lookup"><span data-stu-id="7b4a6-114">The following code example creates a message that specifies text to be displayed and text to be spoken and indicates that the bot is [accepting user input](bot-builder-dotnet-add-input-hints.md).</span></span>

[!code-csharp[Set speak property](../includes/code/dotnet-text-to-speech.cs#Speak1)]

### <a id="say-async"></a> <span data-ttu-id="7b4a6-115">IDialogContext.SayAsync()</span><span class="sxs-lookup"><span data-stu-id="7b4a6-115">IDialogContext.SayAsync()</span></span>

<span data-ttu-id="7b4a6-116">Si usa [diálogos](bot-builder-dotnet-dialogs.md), puede llamar al método `SayAsync()` para crear y enviar un mensaje que especifique el texto que se dirá, además del texto que se mostrará y otras opciones.</span><span class="sxs-lookup"><span data-stu-id="7b4a6-116">If you are using [dialogs](bot-builder-dotnet-dialogs.md), you can call the `SayAsync()` method to create and send a message that specifies the text to be spoken, in addition to the text to be displayed and other options.</span></span> <span data-ttu-id="7b4a6-117">En el ejemplo de código siguiente se crea un mensaje que especifica el texto que se mostrará y el texto que se hablará.</span><span class="sxs-lookup"><span data-stu-id="7b4a6-117">The following code example creates a message that specifies text to be displayed and text to be spoken.</span></span>

[!code-csharp[Call SayAsync()](../includes/code/dotnet-text-to-speech.cs#Speak2)]

### <a id="prompt-options"></a> <span data-ttu-id="7b4a6-118">Opciones de aviso</span><span class="sxs-lookup"><span data-stu-id="7b4a6-118">Prompt options</span></span>

<span data-ttu-id="7b4a6-119">Con cualquiera de los avisos integrados, puede establecer las opciones `speak` y `retrySpeak` para especificar el texto que dirá el bot.</span><span class="sxs-lookup"><span data-stu-id="7b4a6-119">Using any of the built-in prompts, you can set the options `speak` and `retrySpeak` to specify the text to be spoken by your bot.</span></span> <span data-ttu-id="7b4a6-120">En el ejemplo de código siguiente se crea un aviso que especifica el texto que se mostrará, el texto que se dirá inicialmente y el texto que se dirá después de esperar un tiempo los datos de entrada del usuario.</span><span class="sxs-lookup"><span data-stu-id="7b4a6-120">The following code example creates a prompt that specifies text to be displayed, text to be spoken initially, and text to be spoken after waiting a while for user input.</span></span> <span data-ttu-id="7b4a6-121">Se usa el formato [SSML](#ssml) para indicar que la palabra "claro" se dirá con una cantidad moderada de énfasis.</span><span class="sxs-lookup"><span data-stu-id="7b4a6-121">It uses [SSML](#ssml) formatting to indicate that the word "sure" should be spoken with a moderate amount of emphasis.</span></span>

[!code-csharp[Set Prompt options](../includes/code/dotnet-text-to-speech.cs#Speak3)]

## <a id="ssml"></a><span data-ttu-id="7b4a6-122">Lenguaje de marcado de síntesis de voz (SSML)</span><span class="sxs-lookup"><span data-stu-id="7b4a6-122">Speech Synthesis Markup Language (SSML)</span></span>

<span data-ttu-id="7b4a6-123">Para especificar el texto que dirá el bot, puede usar una cadena de texto sin formato o en una cadena con formato de lenguaje de marcado de síntesis de voz (SSML), un lenguaje de marcado basado en XML que le permite controlar diversas características de la voz del bot, como la voz, la velocidad, el volumen, la pronunciación, el tono y mucho más.</span><span class="sxs-lookup"><span data-stu-id="7b4a6-123">To specify text to be spoken by your bot, you can use either a plain text string or a string that is formatted as Speech Synthesis Markup Language (SSML), an XML-based markup language that enables you to control various characteristics of your bot's speech such as voice, rate, volume, pronunciation, pitch, and more.</span></span> <span data-ttu-id="7b4a6-124">Para más información sobre SSML, consulte <a href="https://msdn.microsoft.com/en-us/library/hh378377(v=office.14).aspx" target="_blank">Referencia del lenguaje de marcado de síntesis de voz</a>.</span><span class="sxs-lookup"><span data-stu-id="7b4a6-124">For details about SSML, see <a href="https://msdn.microsoft.com/en-us/library/hh378377(v=office.14).aspx" target="_blank">Speech Synthesis Markup Language Reference</a>.</span></span>

## <a name="input-hints"></a><span data-ttu-id="7b4a6-125">Sugerencias de entrada</span><span class="sxs-lookup"><span data-stu-id="7b4a6-125">Input hints</span></span>

<span data-ttu-id="7b4a6-126">Cuando envía un mensaje en un canal habilitado para voz, puede intentar influir en el estado del micrófono del cliente e incluir una sugerencia de entrada para indicar si el bot acepta, espera o ignora la entrada del usuario.</span><span class="sxs-lookup"><span data-stu-id="7b4a6-126">When you send a message on a speech-enabled channel, you can attempt to influence the state of the client's microphone by also including an input hint to indicate whether your bot is accepting, expecting, or ignoring user input.</span></span> <span data-ttu-id="7b4a6-127">Para más información, consulte [Incorporación de sugerencias de entrada a los mensajes](bot-builder-dotnet-add-input-hints.md).</span><span class="sxs-lookup"><span data-stu-id="7b4a6-127">For more information, see [Add input hints to messages](bot-builder-dotnet-add-input-hints.md).</span></span>

## <a name="sample-code"></a><span data-ttu-id="7b4a6-128">Código de ejemplo</span><span class="sxs-lookup"><span data-stu-id="7b4a6-128">Sample code</span></span> 

<span data-ttu-id="7b4a6-129">Para ver un ejemplo completo en el que se muestra cómo crear un bot habilitado para voz mediante Bot Builder SDK para. NET, consulte el ejemplo <a href="https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/demo-RollerSkill" target="_blank">Roller Skill</a> en GitHub.</span><span class="sxs-lookup"><span data-stu-id="7b4a6-129">For a complete sample that shows how to create a speech-enabled bot using the Bot Builder SDK for .NET, see the <a href="https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/demo-RollerSkill" target="_blank">Roller Skill sample</a> in GitHub.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7b4a6-130">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="7b4a6-130">Additional resources</span></span>

- [<span data-ttu-id="7b4a6-131">Creación de mensajes</span><span class="sxs-lookup"><span data-stu-id="7b4a6-131">Create messages</span></span>](bot-builder-dotnet-create-messages.md)
- [<span data-ttu-id="7b4a6-132">Incorporación de sugerencias de entrada a los mensajes</span><span class="sxs-lookup"><span data-stu-id="7b4a6-132">Add input hints to messages</span></span>](bot-builder-dotnet-add-input-hints.md)
- <span data-ttu-id="7b4a6-133"><a href="https://msdn.microsoft.com/en-us/library/hh378377(v=office.14).aspx" target="_blank">Lenguaje de marcado de síntesis de voz (SSML)</a></span><span class="sxs-lookup"><span data-stu-id="7b4a6-133"><a href="https://msdn.microsoft.com/en-us/library/hh378377(v=office.14).aspx" target="_blank">Speech Synthesis Markup Language (SSML)</a></span></span>
- <span data-ttu-id="7b4a6-134"><a href="https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/demo-RollerSkill" target="_blank">Ejemplo de Roller Skill (GitHub)</a></span><span class="sxs-lookup"><span data-stu-id="7b4a6-134"><a href="https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/demo-RollerSkill" target="_blank">Roller Skill sample (GitHub)</a></span></span>
- <span data-ttu-id="7b4a6-135"><a href="https://docs.botframework.com/en-us/csharp/builder/sdkreference/dc/d2f/class_microsoft_1_1_bot_1_1_connector_1_1_activity.html" target="_blank">Clase Activity</a></span><span class="sxs-lookup"><span data-stu-id="7b4a6-135"><a href="https://docs.botframework.com/en-us/csharp/builder/sdkreference/dc/d2f/class_microsoft_1_1_bot_1_1_connector_1_1_activity.html" target="_blank">Activity class</a></span></span>
- <span data-ttu-id="7b4a6-136"><a href="/dotnet/api/microsoft.bot.connector.imessageactivity" target="_blank">Interfaz IMessageActivity interface</a></span><span class="sxs-lookup"><span data-stu-id="7b4a6-136"><a href="/dotnet/api/microsoft.bot.connector.imessageactivity" target="_blank">IMessageActivity interface</a></span></span>
- <span data-ttu-id="7b4a6-137"><a href="/dotnet/api/microsoft.bot.builder.dialogs.internals.dialogcontext" target="_blank">Clase DialogContext</a></span><span class="sxs-lookup"><span data-stu-id="7b4a6-137"><a href="/dotnet/api/microsoft.bot.builder.dialogs.internals.dialogcontext" target="_blank">DialogContext class</a></span></span>
- <span data-ttu-id="7b4a6-138"><a href="/dotnet/api/microsoft.bot.builder.dialogs.internals.prompt-2" target="_blank">Clase Prompt</a></span><span class="sxs-lookup"><span data-stu-id="7b4a6-138"><a href="/dotnet/api/microsoft.bot.builder.dialogs.internals.prompt-2" target="_blank">Prompt class</a></span></span>

[IMessageActivity]: /dotnet/api/microsoft.bot.connector.imessageactivity

