---
title: Realización de llamadas de audio con Skype | Microsoft Docs
description: Obtenga información sobre cómo realizar llamadas de audio con Skype con Bot Builder SDK para .NET.
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: 9e555de909275a991294cf093c4a76b7035b4a13
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305557"
---
# <a name="conduct-audio-calls-with-skype"></a><span data-ttu-id="73a1c-103">Realización de llamadas de audio con Skype</span><span class="sxs-lookup"><span data-stu-id="73a1c-103">Conduct audio calls with Skype</span></span>

[!INCLUDE [Introduction to conducting audio calls](../includes/snippet-audio-call-intro.md)]

<span data-ttu-id="73a1c-104">La arquitectura de un bot que admita llamadas de audio es muy similar a la de un bot típico.</span><span class="sxs-lookup"><span data-stu-id="73a1c-104">The architecture for a bot that supports audio calls is very similar to that of a typical bot.</span></span> <span data-ttu-id="73a1c-105">Los ejemplos de código siguientes muestran cómo habilitar la compatibilidad con llamadas de audio mediante Skype con Bot Builder SDK para .NET.</span><span class="sxs-lookup"><span data-stu-id="73a1c-105">The following code samples show how to enable support for audio calls via Skype with the Bot Builder SDK for .NET.</span></span> 

## <a name="enable-support-for-audio-calls"></a><span data-ttu-id="73a1c-106">Habilitación de la compatibilidad con llamadas de audio</span><span class="sxs-lookup"><span data-stu-id="73a1c-106">Enable support for audio calls</span></span>

<span data-ttu-id="73a1c-107">Para habilitar un bot para que admita llamadas de audio, defina el elemento `CallingController`.</span><span class="sxs-lookup"><span data-stu-id="73a1c-107">To enable a bot to support audio calls, define the `CallingController`.</span></span>

```cs
[BotAuthentication]
[RoutePrefix("api/calling")]
public class CallingController : ApiController
{
    public CallingController() : base()
    {
        CallingConversation.RegisterCallingBot(callingBotService => new IVRBot(callingBotService));
    }

    [Route("callback")]
    public async Task<HttpResponseMessage> ProcessCallingEventAsync()
    {
        return await CallingConversation.SendAsync(this.Request, CallRequestType.CallingEvent);
    }

    [Route("call")]
    public async Task<HttpResponseMessage> ProcessIncomingCallAsync()
    {
        return await CallingConversation.SendAsync(this.Request, CallRequestType.IncomingCall);
    }
}
```

> [!NOTE]
> <span data-ttu-id="73a1c-108">Además del elemento `CallingController`, que admite llamadas de audio, un bot también puede contener un elemento `MessagesController` para admitir mensajes.</span><span class="sxs-lookup"><span data-stu-id="73a1c-108">In addition to the `CallingController`, which supports audio calls, a bot may also contain a `MessagesController` to support messages.</span></span> <span data-ttu-id="73a1c-109">Proporcionar ambas opciones permite a los usuarios interactuar con el bot de la manera que prefieran.</span><span class="sxs-lookup"><span data-stu-id="73a1c-109">Providing both options allows users to interact with the bot in the way that they prefer.</span></span> <!-- docs on MessagesController are where? -->

##  <a name="answer-the-call"></a><span data-ttu-id="73a1c-110">Responder la llamada</span><span class="sxs-lookup"><span data-stu-id="73a1c-110">Answer the call</span></span>

<span data-ttu-id="73a1c-111">La tarea `ProcessIncomingCallAsync` se ejecutará cada vez que un usuario inicia una llamada a este bot desde Skype.</span><span class="sxs-lookup"><span data-stu-id="73a1c-111">The `ProcessIncomingCallAsync` task will execute whenever a user initiates a call to this bot from Skype.</span></span>
<span data-ttu-id="73a1c-112">El constructor registra la clase `IVRBot`, que tiene un controlador predefinido para el elemento `incomingCallEvent`.</span><span class="sxs-lookup"><span data-stu-id="73a1c-112">The constructor registers the `IVRBot` class, which has a predefined handler for the `incomingCallEvent`.</span></span>

<span data-ttu-id="73a1c-113">La primera acción del flujo de trabajo debe determinar si el bot responde o rechaza la llamada entrante.</span><span class="sxs-lookup"><span data-stu-id="73a1c-113">The first action within the workflow should determine if the bot answers or rejects the incoming call.</span></span> <span data-ttu-id="73a1c-114">Este flujo de trabajo le indica al bot que responda a la llamada entrante y, a continuación, reproduzca un mensaje de bienvenida.</span><span class="sxs-lookup"><span data-stu-id="73a1c-114">This workflow instructs the bot to answer the incoming call and then play a welcome message.</span></span> 

```cs
private Task OnIncomingCallReceived(IncomingCallEvent incomingCallEvent)
{
    this.callStateMap[incomingCallEvent.IncomingCall.Id] = new CallState(incomingCallEvent.IncomingCall.Participants);

    incomingCallEvent.ResultingWorkflow.Actions = new List<ActionBase>
    {
        new Answer { OperationId = Guid.NewGuid().ToString() },
        GetPromptForText(WelcomeMessage)
    };

    return Task.FromResult(true);
}
```

## <a name="after-the-bot-answers"></a><span data-ttu-id="73a1c-115">Después de que el bot responda</span><span class="sxs-lookup"><span data-stu-id="73a1c-115">After the bot answers</span></span>

<span data-ttu-id="73a1c-116">Si el bot responde a la llamada, las acciones posteriores especificadas en el flujo de trabajo indicarán a la **plataforma de llamadas para bots de Skype** que reproduzca un aviso, grabe audio, realice un reconocimiento de voz o recopile dígitos desde el panel de marcado.</span><span class="sxs-lookup"><span data-stu-id="73a1c-116">If the bot answers the call, subsequent actions specified within the workflow will instruct the **Skype Bot Platform for Calling** to play prompt, record audio, recognize speech, or collect digits from a dial pad.</span></span> <span data-ttu-id="73a1c-117">La acción final del flujo de trabajo podría ser finalizar la llamada.</span><span class="sxs-lookup"><span data-stu-id="73a1c-117">The final action of the workflow might be to end the call.</span></span> 

<span data-ttu-id="73a1c-118">Este ejemplo de código define un controlador que establecerá un menú al finalizar el mensaje de bienvenida.</span><span class="sxs-lookup"><span data-stu-id="73a1c-118">This code sample defines a handler that will set up a menu after the welcome message completes.</span></span>

```cs
private Task OnPlayPromptCompleted(PlayPromptOutcomeEvent playPromptOutcomeEvent)
{
    var callState = this.callStateMap[playPromptOutcomeEvent.ConversationResult.Id];
    SetupInitialMenu(playPromptOutcomeEvent.ResultingWorkflow);
    return Task.FromResult(true);
}
```

<span data-ttu-id="73a1c-119">El método `CreateIvrOptions` define el menú que se presentará al usuario.</span><span class="sxs-lookup"><span data-stu-id="73a1c-119">The `CreateIvrOptions` method defines that menu that will be presented to the user.</span></span>

```cs
private static Recognize CreateIvrOptions(string textToBeRead, int numberOfOptions, bool includeBack)
{
    if (numberOfOptions > 9)
    {
        throw new Exception("too many options specified");
    }

    var choices = new List<RecognitionOption>();

    for (int i = 1; i <= numberOfOptions; i++)
    {
        choices.Add(new RecognitionOption { Name = Convert.ToString(i), DtmfVariation = (char)('0' + i) });
    }

    if (includeBack)
    {
        choices.Add(new RecognitionOption { Name = "#", DtmfVariation = '#' });
    }

    var recognize = new Recognize
    {
        OperationId = Guid.NewGuid().ToString(),
        PlayPrompt = GetPromptForText(textToBeRead),
        BargeInAllowed = true,
        Choices = choices
    };

    return recognize;
}
```

<span data-ttu-id="73a1c-120">La clase `RecognitionOption` define la respuesta de voz y la variación del doble tono multifrecuencia (DTMF) correspondiente.</span><span class="sxs-lookup"><span data-stu-id="73a1c-120">The `RecognitionOption` class defines both the spoken answer as well as the corresponding Dual-Tone Multi-Frequency (DTMF) variation.</span></span> <span data-ttu-id="73a1c-121">DTMF permite al usuario responder escribiendo los dígitos correspondientes en el teclado en lugar de hablar.</span><span class="sxs-lookup"><span data-stu-id="73a1c-121">DTMF enables the user to answer by typing the corresponding digits on the keypad instead of speaking.</span></span>

<span data-ttu-id="73a1c-122">El método `OnRecognizeCompleted` procesa la selección del usuario y el parámetro de entrada `recognizeOutcomeEvent` contiene el valor de la selección del usuario.</span><span class="sxs-lookup"><span data-stu-id="73a1c-122">The `OnRecognizeCompleted` method processes the user's selection, and the input parameter `recognizeOutcomeEvent` contains the value of the user's selection.</span></span>

```cs
private Task OnRecognizeCompleted(RecognizeOutcomeEvent recognizeOutcomeEvent)
{
    var callState = this.callStateMap[recognizeOutcomeEvent.ConversationResult.Id];
    ProcessMainMenuSelection(recognizeOutcomeEvent, callState);
    return Task.FromResult(true);
}
```

## <a name="support-natural-language"></a><span data-ttu-id="73a1c-123">Compatibilidad con el lenguaje natural</span><span class="sxs-lookup"><span data-stu-id="73a1c-123">Support natural language</span></span>
<span data-ttu-id="73a1c-124">También se puede diseñar el bot para que admita respuestas de lenguaje natural.</span><span class="sxs-lookup"><span data-stu-id="73a1c-124">The bot can also be designed to support natural language responses.</span></span> <span data-ttu-id="73a1c-125">**Bing Speech API** permite al bot reconocer palabras de la respuesta hablada del usuario.</span><span class="sxs-lookup"><span data-stu-id="73a1c-125">The **Bing Speech API** enables the bot to recognize words in the user's spoken reply.</span></span>

```cs
private async Task OnRecordCompleted(RecordOutcomeEvent recordOutcomeEvent)
{
    recordOutcomeEvent.ResultingWorkflow.Actions = new List<ActionBase>
    {
        GetPromptForText(EndingMessage),
        new Hangup { OperationId = Guid.NewGuid().ToString() }
    };

    // Convert the audio to text
    if (recordOutcomeEvent.RecordOutcome.Outcome == Outcome.Success)
    {
        var record = await recordOutcomeEvent.RecordedContent;
        string text = await this.GetTextFromAudioAsync(record);

        var callState = this.callStateMap[recordOutcomeEvent.ConversationResult.Id];

        await this.SendSTTResultToUser("We detected the following audio: " + text, callState.Participants);
    }

    recordOutcomeEvent.ResultingWorkflow.Links = null;
    this.callStateMap.Remove(recordOutcomeEvent.ConversationResult.Id);
}
```

## <a name="sample-code"></a><span data-ttu-id="73a1c-126">Código de ejemplo</span><span class="sxs-lookup"><span data-stu-id="73a1c-126">Sample code</span></span>

<span data-ttu-id="73a1c-127">Para obtener un ejemplo completo que muestra cómo se admiten llamadas de audio con Skype mediante Bot Builder SDK para .NET, consulte el <a href="https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/skype-CallingBot" target="_blank">ejemplo Skype Calling Bot</a> en GitHub.</span><span class="sxs-lookup"><span data-stu-id="73a1c-127">For a complete sample that shows how to support audio calls with Skype using the Bot Builder SDK for .NET, see the <a href="https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/skype-CallingBot" target="_blank">Skype Calling Bot sample</a> in GitHub.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="73a1c-128">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="73a1c-128">Additional resources</span></span>

- <span data-ttu-id="73a1c-129"><a href="/dotnet/api/?view=botbuilder-3.11.0" target="_blank">Referencia de Bot Builder SDK para .NET</a></span><span class="sxs-lookup"><span data-stu-id="73a1c-129"><a href="/dotnet/api/?view=botbuilder-3.11.0" target="_blank">Bot Builder SDK for .NET Reference</a></span></span>
- <span data-ttu-id="73a1c-130"><a href="https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/skype-CallingBot" target="_blank">Ejemplo Skype Calling Bot (GitHub)</a></span><span class="sxs-lookup"><span data-stu-id="73a1c-130"><a href="https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/skype-CallingBot" target="_blank">Skype Calling Bot sample (GitHub)</a></span></span>
