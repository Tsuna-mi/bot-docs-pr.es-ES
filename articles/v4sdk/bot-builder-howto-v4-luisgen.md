---
title: Extraer resultados LUIS escritos | Microsoft Docs
description: Obtenga información sobre cómo se usa LUIS para extraer entidades con el SDK de Bot Builder.
keywords: intenciones, entidades, LUISGen, extraer
author: DeniseMak
ms.author: v-demak
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 5/16/17
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: 6a88b0a7f44f43d0676ba88314fbba7c486e6be4
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39304964"
---
# <a name="extract-intents-and-entities-using-luisgen"></a><span data-ttu-id="6f427-104">Extraer intenciones y entidades mediante LUISGen</span><span class="sxs-lookup"><span data-stu-id="6f427-104">Extract intents and entities using LUISGen</span></span>

<span data-ttu-id="6f427-105">Además de reconocer la intención, una aplicación de LUIS puede extraer entidades (es decir, palabras importantes) para cumplir la solicitud de un usuario.</span><span class="sxs-lookup"><span data-stu-id="6f427-105">Besides recognizing intent, a LUIS app can also extract entities, which are important words for fulfilling a user's request.</span></span> <span data-ttu-id="6f427-106">Por ejemplo, en una reserva de restaurante, la aplicación LUIS puede extraer el tamaño del grupo, la fecha de la reserva o la ubicación del restaurante del mensaje del usuario.</span><span class="sxs-lookup"><span data-stu-id="6f427-106">For example, in the example of a restaurant reservation, the LUIS app might be able to extract the party size, reservation date or restaurant location from the user's message.</span></span> 


<span data-ttu-id="6f427-107">Puede usar la [herramienta LUISGen](https://github.com/Microsoft/botbuilder-tools/tree/master/LUISGen) para crear clases que faciliten la extracción de las entidades de LUIS en el código del bot.</span><span class="sxs-lookup"><span data-stu-id="6f427-107">You can use the [LUISGen tool](https://github.com/Microsoft/botbuilder-tools/tree/master/LUISGen) to generate classes that make it easier to extract entities from LUIS in your bot's code.</span></span>

<span data-ttu-id="6f427-108">En una línea de comandos de Node.js, instale `luisgen` en la ruta de acceso global.</span><span class="sxs-lookup"><span data-stu-id="6f427-108">At a Node.js command line, install `luisgen` to the global path.</span></span>
```
npm install -g luisgen
```

# <a name="ctabcs"></a>[<span data-ttu-id="6f427-109">C#</span><span class="sxs-lookup"><span data-stu-id="6f427-109">C#</span></span>](#tab/cs)

## <a name="generate-a-luis-results-class"></a><span data-ttu-id="6f427-110">Generar una clase de resultados de LUIS</span><span class="sxs-lookup"><span data-stu-id="6f427-110">Generate a LUIS results class</span></span>

<span data-ttu-id="6f427-111">Descargue el [ejemplo CafeBot LUIS](https://aka.ms/contosocafebot-luis) y, en su carpeta raíz, ejecute LUISGen:</span><span class="sxs-lookup"><span data-stu-id="6f427-111">Download the [CafeBot LUIS sample](https://aka.ms/contosocafebot-luis), and in its root folder, run LUISGen:</span></span>

```
luisgen Assets\LU\models\LUIS\cafeLUISModel.json -cs ContosoCafeBot.CafeLUISModel
```

## <a name="examine-the-generated-code"></a><span data-ttu-id="6f427-112">Examinar el código generado</span><span class="sxs-lookup"><span data-stu-id="6f427-112">Examine the generated code</span></span>
<span data-ttu-id="6f427-113">Esto genera **cafeLUISModel.cs**, que puede agregar al proyecto.</span><span class="sxs-lookup"><span data-stu-id="6f427-113">This generates **cafeLUISModel.cs**, which you can add to your project.</span></span> <span data-ttu-id="6f427-114">Proporciona una clase `cafeLuisModel` para obtener resultados fuertemente tipados de LUIS.</span><span class="sxs-lookup"><span data-stu-id="6f427-114">It provides a `cafeLuisModel` class for getting strongly-typed results from LUIS.</span></span>

<span data-ttu-id="6f427-115">Esta clase tiene un valor enum para obtener las intenciones definidas en la aplicación LUIS.</span><span class="sxs-lookup"><span data-stu-id="6f427-115">This class has an enum for getting the intents defined in the LUIS app.</span></span>
```cs
public enum Intent {
    Book_Table, 
    Greeting, 
    None, 
    Who_are_you_intent
};
```
<span data-ttu-id="6f427-116">Asimismo, también tiene una propiedad `Entities`.</span><span class="sxs-lookup"><span data-stu-id="6f427-116">It also has an `Entities` property.</span></span> <span data-ttu-id="6f427-117">Dado que puede haber varias repeticiones de una entidad en el mensaje de un usuario, la clase `_Entities` define una matriz para cada tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="6f427-117">Since there can be multiple occurences of an entity in a user's message, the `_Entities` class defines an array for each type of entity.</span></span> 
```cs
public class _Entities
{
    // Simple entities
    public string[] partySize;

    // Built-in entities
    public Microsoft.Bot.Builder.Ai.LUIS.DateTimeSpec[] datetime;
    public double[] number;

    // Lists
    public string[][] cafeLocation;

    // Instance
    public class _Instance
    {
        public Microsoft.Bot.Builder.Ai.LUIS.InstanceData[] partySize;
        public Microsoft.Bot.Builder.Ai.LUIS.InstanceData[] datetime;
        public Microsoft.Bot.Builder.Ai.LUIS.InstanceData[] number;
        public Microsoft.Bot.Builder.Ai.LUIS.InstanceData[] cafeLocation;
    }
    [JsonProperty("$instance")]
    public _Instance _instance;
}
public _Entities Entities;
```

> [!NOTE]
> <span data-ttu-id="6f427-118">Todos los tipos de entidades son matrices, ya que LUIS puede detectar más de una entidad de un tipo especificado en la declaración de un usuario.</span><span class="sxs-lookup"><span data-stu-id="6f427-118">All the entity types are arrays, because LUIS may detect more than one entity of a specified type in a user's utterance.</span></span> <span data-ttu-id="6f427-119">Por ejemplo, si el usuario dice "hacer reservas para mañana a las 5 de la tarde, y a las 9 de la noche el sábado siguiente", "mañana a las 5 de la tarde" y "a las 9 de la tarde el sábado siguiente" se devuelven en los resultados `datetime`.</span><span class="sxs-lookup"><span data-stu-id="6f427-119">For example, if the user says "make reservations for 5pm tomorrow and 9pm next Saturday", both "5pm tomorrow" and "9pm next Saturday" are returned in the `datetime` results.</span></span>
>

|<span data-ttu-id="6f427-120">Entidad</span><span class="sxs-lookup"><span data-stu-id="6f427-120">Entity</span></span> | <span data-ttu-id="6f427-121">Escriba</span><span class="sxs-lookup"><span data-stu-id="6f427-121">Type</span></span> | <span data-ttu-id="6f427-122">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="6f427-122">Example</span></span> | <span data-ttu-id="6f427-123">Notas</span><span class="sxs-lookup"><span data-stu-id="6f427-123">Notes</span></span> |
|-------|-----|------|---|
|<span data-ttu-id="6f427-124">partySize</span><span class="sxs-lookup"><span data-stu-id="6f427-124">partySize</span></span>| <span data-ttu-id="6f427-125">string[]</span><span class="sxs-lookup"><span data-stu-id="6f427-125">string[]</span></span>| <span data-ttu-id="6f427-126">Grupo de `four`.</span><span class="sxs-lookup"><span data-stu-id="6f427-126">Party of `four`</span></span>| <span data-ttu-id="6f427-127">Una entidad sencilla reconoce las cadenas.</span><span class="sxs-lookup"><span data-stu-id="6f427-127">A simple entity recognizes strings.</span></span> <span data-ttu-id="6f427-128">En este ejemplo, Entities.partySize[0] es `"four"`.</span><span class="sxs-lookup"><span data-stu-id="6f427-128">In this example, Entities.partySize[0] is `"four"`.</span></span>
|<span data-ttu-id="6f427-129">Datetime</span><span class="sxs-lookup"><span data-stu-id="6f427-129">datetime</span></span>| <span data-ttu-id="6f427-130">[DateTimeSpec](https://docs.microsoft.com/en-us/dotnet/api/microsoft.bot.builder.ai.luis.datetimespec?view=botbuilder-4.0.0-alpha)[]</span><span class="sxs-lookup"><span data-stu-id="6f427-130">[DateTimeSpec](https://docs.microsoft.com/en-us/dotnet/api/microsoft.bot.builder.ai.luis.datetimespec?view=botbuilder-4.0.0-alpha)[]</span></span>| <span data-ttu-id="6f427-131">realizar una reserva para el `9pm tomorrow`.</span><span class="sxs-lookup"><span data-stu-id="6f427-131">reservation for at `9pm tomorrow`</span></span>| <span data-ttu-id="6f427-132">Cada objeto **DateTimeSpec** tiene un campo timex con el valor posible de horas especificadas en formato **timex**.</span><span class="sxs-lookup"><span data-stu-id="6f427-132">Each **DateTimeSpec** object has a timex field with the possible value of times specified in **timex** format.</span></span> <span data-ttu-id="6f427-133">Puede obtener más información sobre timex aquí: http://www.timeml.org/publications/timeMLdocs/timeml_1.2.1.html#timex3      Puede obtener más información sobre la biblioteca que realiza el reconocimiento aquí: https://github.com/Microsoft/Recognizers-Text.</span><span class="sxs-lookup"><span data-stu-id="6f427-133">More information on timex can be found here: http://www.timeml.org/publications/timeMLdocs/timeml_1.2.1.html#timex3      More information on the library which does the recognition can be found here: https://github.com/Microsoft/Recognizers-Text</span></span>
|<span data-ttu-id="6f427-134">número</span><span class="sxs-lookup"><span data-stu-id="6f427-134">number</span></span>| <span data-ttu-id="6f427-135">double[]</span><span class="sxs-lookup"><span data-stu-id="6f427-135">double[]</span></span>| <span data-ttu-id="6f427-136">Un grupo de `four` que incluye `2` niños.</span><span class="sxs-lookup"><span data-stu-id="6f427-136">Party of `four` which includes `2` children</span></span> | <span data-ttu-id="6f427-137">`number` identificará todos los números, no sólo de tamaño del grupo.</span><span class="sxs-lookup"><span data-stu-id="6f427-137">`number` will identify all numbers, not just size of the party.</span></span> <br/> <span data-ttu-id="6f427-138">En la declaración "El grupo de cuatro incluye 2 niños" `Entities.number[0]` es 4, y `Entities.number[1]` es 2.</span><span class="sxs-lookup"><span data-stu-id="6f427-138">In the utterance "Party of four which includes 2 children", `Entities.number[0]` is 4, and `Entities.number[1]` is 2.</span></span>
|<span data-ttu-id="6f427-139">cafelocation</span><span class="sxs-lookup"><span data-stu-id="6f427-139">cafelocation</span></span>| <span data-ttu-id="6f427-140">string[][]</span><span class="sxs-lookup"><span data-stu-id="6f427-140">string[][]</span></span> | <span data-ttu-id="6f427-141">Reserva en la ubicación `Seattle`.</span><span class="sxs-lookup"><span data-stu-id="6f427-141">Reservation at the `Seattle` location.</span></span>| <span data-ttu-id="6f427-142">cafeLocation es una entidad de la lista, lo que significa que contiene miembros reconocidos de listas.</span><span class="sxs-lookup"><span data-stu-id="6f427-142">cafeLocation is a list entity, which means that it contains recognized members of lists.</span></span> <span data-ttu-id="6f427-143">Es una matriz de matrices, en caso de que una entidad reconocida sea miembro de más de una lista.</span><span class="sxs-lookup"><span data-stu-id="6f427-143">It is an array of arrays, in case a recognized entity is a member of more than one list.</span></span> <span data-ttu-id="6f427-144">Por ejemplo, "reserva en Washington" puede corresponder a una lista que hace referencia al estado de Washington y a Washington D.C.</span><span class="sxs-lookup"><span data-stu-id="6f427-144">For example, "reservation in Washington" could correspond to a list for Washington state and for Washington D.C.</span></span>

<span data-ttu-id="6f427-145">La propiedad `_Instance` proporciona [InstanceData](https://docs.microsoft.com/en-us/dotnet/api/microsoft.bot.builder.ai.luis.instancedata?view=botbuilder-4.0.0-alpha) para ofrecer más detalles en cada entidad reconocida.</span><span class="sxs-lookup"><span data-stu-id="6f427-145">The `_Instance` property provides [InstanceData](https://docs.microsoft.com/en-us/dotnet/api/microsoft.bot.builder.ai.luis.instancedata?view=botbuilder-4.0.0-alpha) for more detail on each recognized entity.</span></span>

## <a name="check-intents-in-your-bot"></a><span data-ttu-id="6f427-146">Comprobar las intenciones en el bot</span><span class="sxs-lookup"><span data-stu-id="6f427-146">Check intents in your bot</span></span>
<span data-ttu-id="6f427-147">En **CafeBot.cs**, eche un vistazo al código de `OnTurn`.</span><span class="sxs-lookup"><span data-stu-id="6f427-147">In **CafeBot.cs**, Take a look at the code within `OnTurn`.</span></span> <span data-ttu-id="6f427-148">Puede ver que el bot llama a LUIS y comprueba las intenciones para decidir con qué cuadro de diálogo debe comenzar.</span><span class="sxs-lookup"><span data-stu-id="6f427-148">You can see where the bot calls LUIS and checks intents to decide which dialog to begin.</span></span> <span data-ttu-id="6f427-149">Los resultados de LUIS de la llamada a [`Recognize`](https://docs.microsoft.com/en-us/dotnet/api/microsoft.bot.builder.ai.luis.luisrecognizer?view=botbuilder-4.0.0-alpha#methods) se pasan como un argumento al diálogo `BookTable`.</span><span class="sxs-lookup"><span data-stu-id="6f427-149">The LUIS results from the call to [`Recognize`](https://docs.microsoft.com/en-us/dotnet/api/microsoft.bot.builder.ai.luis.luisrecognizer?view=botbuilder-4.0.0-alpha#methods) are passed as an argument to the `BookTable` dialog.</span></span>



```cs
if(!context.Responded)
{
    // call LUIS and get results
    LuisRecognizer lRecognizer = createLUISRecognizer();
    // Use the generated class as the type parameter to Recognize()
    cafeLUISModel lResult = await lRecognizer.Recognize<cafeLUISModel>(utterance, ct);
    Dictionary<string,object> lD = new Dictionary<string,object>();
    if(lResult != null) {
        lD.Add("luisResult", lResult);
    }
    
    // top level dispatch
    switch (lResult.TopIntent().intent)
    {
        case cafeLUISModel.Intent.Greeting:
            await context.SendActivity("Hello!");
            if (userState.sendCards) await context.SendActivity(CreateCardResponse(context.Activity, createWelcomeCardAttachment()));
            break;

        case cafeLUISModel.Intent.Book_Table:
            await dc.Begin("BookTable", lD);
            break;

        case cafeLUISModel.Intent.Who_are_you_intent:
            await context.sendActivity("I'm the Contoso Cafe bot.");
            break;

        case cafeLUISModel.Intent.None:
        default:
            await getQnAResult(context);
            break;
    }
}
```

## <a name="extract-entities-in-a-dialog"></a><span data-ttu-id="6f427-150">Extraer entidades de un diálogo</span><span class="sxs-lookup"><span data-stu-id="6f427-150">Extract entities in a dialog</span></span>

<span data-ttu-id="6f427-151">Eche un vistazo a `Dialogs/BookTable.cs`.</span><span class="sxs-lookup"><span data-stu-id="6f427-151">Now take a look at `Dialogs/BookTable.cs`.</span></span> <span data-ttu-id="6f427-152">El cuadro de diálogo `BookTable` contiene una secuencia de pasos en cascada, cada uno de los cuales busca una entidad en los resultados de LUIS que se pasaron a `args`.</span><span class="sxs-lookup"><span data-stu-id="6f427-152">The `BookTable` dialog contains a sequence of waterfall steps, each of which checks for an entity in the LUIS results passed to `args`.</span></span> <span data-ttu-id="6f427-153">Si no se encuentra la entidad, el cuadro de diálogo se salta la solicitud llamando a `next()`.</span><span class="sxs-lookup"><span data-stu-id="6f427-153">If the entity isn't found, the dialog skips prompting for it by calling `next()`.</span></span> <span data-ttu-id="6f427-154">Si se encuentra, el cuadro de diálogo lo solicita y la respuesta del usuario a la solicitud se recibe en el siguiente paso en cascada.</span><span class="sxs-lookup"><span data-stu-id="6f427-154">If it's found, the dialog prompts for it, and the user's answer to the prompt is received in the next waterfall step.</span></span>

```cs
    Dialogs.Add("BookTable",
        new WaterfallStep[]
        {
            async (dc, args, next) =>
            {
                dc.ActiveDialog.State = new Dictionary<string, object>();
                IDictionary<string,object> state = dc.ActiveDialog.State;

                // add any LUIS entities to active dialog state.
                if(args.ContainsKey("luisResult")) {
                    cafeLUISModel lResult = (cafeLUISModel)args["luisResult"];
                    updateContextWithLUIS(lResult, ref state);
                }
                
                // prompt if we do not already have cafelocation
                if(state.ContainsKey("cafeLocation")) {
                    state["bookingLocation"] = state["cafeLocation"];
                    await next();
                } else {
                    await dc.Prompt("choicePrompt", "Which of our locations would you like?", promptOptions);
                }
            },
            async (dc, args, next) =>
            {
                var state = dc.ActiveDialog.State;
                if(!state.ContainsKey("cafeLocation")) {
                    var choiceResult = (FoundChoice)args["Value"];
                    state["bookingLocation"] = choiceResult.Value;
                }
                bool promptForDateTime = true;
                if(state.ContainsKey("datetime")) {
                    // validate timex
                    var inputdatetime = new string[] {(string)state["datetime"]};
                    var results = evaluateTimeX((string[])inputdatetime);
                    if(results.Count != 0) {
                        var timexResolution = results.First().TimexValue;
                        var timexProperty = new TimexProperty(timexResolution.ToString());
                        var bookingDateTime = $"{timexProperty.ToNaturalLanguage(DateTime.Now)}";
                        state["bookingDateTime"] = bookingDateTime;
                        promptForDateTime = false;
                    }
                }
                // prompt if we do not already have date and time
                if(promptForDateTime) {
                    await dc.Prompt("timexPrompt", "When would you like to arrive? (We open at 4PM.)",
                                    new PromptOptions { RetryPromptString = "We only accept reservations for the next 2 weeks and in the evenings between 4PM - 8PM" });
                } else {
                    await next();
                }                       
                
            },
            async (dc, args, next) =>
            {
                var state = dc.ActiveDialog.State;
                if(!state.ContainsKey("datetime")) { 
                    var timexResult = (TimexResult)args;
                    var timexResolution = timexResult.Resolutions.First();
                    var timexProperty = new TimexProperty(timexResolution.ToString());
                    var bookingDateTime = $"{timexProperty.ToNaturalLanguage(DateTime.Now)}";
                    state["bookingDateTime"] = bookingDateTime;
                }
                // prompt if we already do not have party size
                if(state.ContainsKey("partySize")) {
                    state["bookingGuestCount"] = state["partySize"];
                    await next();
                } else {
                    await dc.Prompt("numberPrompt", "How many in your party?");
                }
            },
            async (dc, args, next) =>
            {
                var state = dc.ActiveDialog.State;
                if(!state.ContainsKey("partySize")) {
                    state["bookingGuestCount"] = args["Value"];
                }

                await dc.Prompt("confirmationPrompt", $"Thanks, Should I go ahead and book a table for {state["bookingGuestCount"].ToString()} guests at our {state["bookingLocation"].ToString()} location for {state["bookingDateTime"].ToString()} ?");
            },
            async (dc, args, next) =>
            {
                var dialogState = dc.ActiveDialog.State;

                // TODO: Verify user said yes to confirmation prompt

                // TODO: book the table! 

                await dc.Context.SendActivity($"Thanks, I have {dialogState["bookingGuestCount"].ToString()} guests booked for our {dialogState["bookingLocation"].ToString()} location for {dialogState["bookingDateTime"].ToString()}.");
            }
        }
    );
}

// This helper method updates dialog state with any LUIS results
private void updateContextWithLUIS(cafeLUISModel lResult, ref IDictionary<string,object> dialogContext) {
    if(lResult.Entities.cafeLocation != null && lResult.Entities.cafeLocation.GetLength(0) > 0) {
        dialogContext.Add("cafeLocation", lResult.Entities.cafeLocation[0][0]);
    }
    if(lResult.Entities.partySize != null && lResult.Entities.partySize.GetLength(0) > 0) {
        dialogContext.Add("partySize", lResult.Entities.partySize[0][0]);
    } else {
        if(lResult.Entities.number != null && lResult.Entities.number.GetLength(0) > 0) {
            dialogContext.Add("partySize", lResult.Entities.number[0]);
        }
    }
    if(lResult.Entities.datetime != null && lResult.Entities.datetime.GetLength(0) > 0) {
        dialogContext.Add("datetime", lResult.Entities.datetime[0].Expressions[0]);
    }
}
```
## <a name="run-the-sample"></a><span data-ttu-id="6f427-155">Ejecución del ejemplo</span><span class="sxs-lookup"><span data-stu-id="6f427-155">Run the sample</span></span>

<span data-ttu-id="6f427-156">Abra `ContosoCafeBot.sln` en Visual Studio 2017 y ejecute el bot.</span><span class="sxs-lookup"><span data-stu-id="6f427-156">Open `ContosoCafeBot.sln` in Visual Studio 2017, and run the bot.</span></span> <span data-ttu-id="6f427-157">Use [Bot Framework Emulator](https://docs.microsoft.com/en-us/azure/bot-service/bot-service-debug-emulator) para conectarse al bot de muestra.</span><span class="sxs-lookup"><span data-stu-id="6f427-157">Use the [Bot Framework Emulator](https://docs.microsoft.com/en-us/azure/bot-service/bot-service-debug-emulator) to connect to the sample bot.</span></span>

<span data-ttu-id="6f427-158">En el emulador, diga `reserve a table` para iniciar el cuadro de diálogo de reserva.</span><span class="sxs-lookup"><span data-stu-id="6f427-158">In the emulator, say `reserve a table` to start the reservation dialog.</span></span>

![ejecute el bot](media/how-to-luisgen/run-bot.png)

# <a name="typescripttabjs"></a>[<span data-ttu-id="6f427-160">TypeScript</span><span class="sxs-lookup"><span data-stu-id="6f427-160">TypeScript</span></span>](#tab/js)

<span data-ttu-id="6f427-161">Descargue el [ejemplo CafeBot_LUIS](https://aka.ms/contosocafebot-typescript-luis-dialogs) y, en su carpeta raíz, ejecute LUISGen:</span><span class="sxs-lookup"><span data-stu-id="6f427-161">Download the [CafeBot_LUIS sample](https://aka.ms/contosocafebot-typescript-luis-dialogs), and in its root folder, run LUISGen:</span></span>

```
luisgen cafeLUISModel.json -ts CafeLUISModel
```

<span data-ttu-id="6f427-162">Esto genera **CafeLUISModel.ts**, que puede agregar al proyecto.</span><span class="sxs-lookup"><span data-stu-id="6f427-162">This generates **CafeLUISModel.ts**, which you can add to your project.</span></span> <span data-ttu-id="6f427-163">Puede obtener un resultado escrito del reconocedor LUIS utilizando los tipos del archivo generado.</span><span class="sxs-lookup"><span data-stu-id="6f427-163">You can get a typed result from the LUIS recognizer using the types in the generated file.</span></span>


```typescript
// call LUIS and get typed results
await luisRec.recognize(context).then(async (res : any) => 
{    
    // get a typed result
    var typedresult = res as CafeLUISModel;   
    
```

## <a name="pass-the-typed-result-to-a-dialog"></a><span data-ttu-id="6f427-164">Pasar el resultado escrito a un cuadro de diálogo</span><span class="sxs-lookup"><span data-stu-id="6f427-164">Pass the typed result to a dialog</span></span>

<span data-ttu-id="6f427-165">Examine el código en **luisbot.ts**.</span><span class="sxs-lookup"><span data-stu-id="6f427-165">Examine the code in **luisbot.ts**.</span></span> <span data-ttu-id="6f427-166">En el controlador `processActivity`, el bot pasa el resultado escrito a un cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="6f427-166">In the `processActivity` handler, the bot passes the typed result to a dialog.</span></span>

```typescript
// Listen for incoming requests 
server.post('/api/messages', (req, res) => {
    // Route received request to adapter for processing
    adapter.processActivity(req, res, async (context) => {
        const isMessage = context.activity.type === 'message';

        // Create dialog context 
        const state = conversationState.get(context);
        const dc = dialogs.createContext(context, state);
            
        if (!isMessage) {
            await context.sendActivity(`[${context.activity.type} event detected]`);
        }

        // Check to see if anyone replied. 
        if (!context.responded) {
            await dc.continue();
            // if the dialog didn't send a response
            if (!context.responded && isMessage) {

                
                await luisRec.recognize(context).then(async (res : any) => 
                {    
                    var typedresult = res as CafeLUISModel;                
                    let topIntent = LuisRecognizer.topIntent(res);    
                    switch (topIntent)
                    {
                        case Intents.Book_Table: {
                            await context.sendActivity("Top intent is Book_Table ");                          
                            await dc.begin('reserveTable', typedresult);
                            break;
                        }
                        
                        case Intents.Greeting: {
                            await context.sendActivity("Top intent is Greeting");
                            break;
                        }
    
                        case Intents.Who_are_you_intent: {
                            await context.sendActivity("Top intent is Who_are_you_intent");
                            break;
                        }
                        default: {
                            await dc.begin('default', topIntent);
                            break;
                        }
                    }
    
                }, (err) => {
                    // there was some error
                    console.log(err);
                }
                );                                
            }
        }
    });
});
```

## <a name="check-for-existing-entities-in-a-dialog"></a><span data-ttu-id="6f427-167">Buscar las entidades existentes en un cuadro de diálogo</span><span class="sxs-lookup"><span data-stu-id="6f427-167">Check for existing entities in a dialog</span></span>

<span data-ttu-id="6f427-168">En **luisbot.ts**, el diálogo `reserveTable` llama a una función auxiliar `SaveEntities` para comprobar las entidades que detectó la aplicación LUIS.</span><span class="sxs-lookup"><span data-stu-id="6f427-168">In **luisbot.ts**, the `reserveTable` dialog calls a `SaveEntities` helper function to check for entities detected by the LUIS app.</span></span> <span data-ttu-id="6f427-169">Si se encuentran las entidades, se guardan en el estado del diálogo.</span><span class="sxs-lookup"><span data-stu-id="6f427-169">If the entities are found, they're saved to dialog state.</span></span> <span data-ttu-id="6f427-170">Cada paso en cascada del diálogo comprueba si una entidad se guardó en el estado del diálogo; si no es así, lo solicita.</span><span class="sxs-lookup"><span data-stu-id="6f427-170">Each waterfall step in the dialog checks if an entity was saved to dialog state, and if not, prompts for it.</span></span>

```typescript
dialogs.add('reserveTable', [
    async function(dc, args, next){
        var typedresult = args as CafeLUISModel;

        // Call a helper function to save the entities in the LUIS result
        // to dialog state
        await SaveEntities(dc, typedresult);

        await dc.context.sendActivity("Welcome to the reservation service.");
        
        if (dc.activeDialog.state.dateTime) {
            await next();     
        }
        else {
            await dc.prompt('dateTimePrompt', "Please provide a reservation date and time.");
        }
    },
    async function(dc, result, next){
        if (!dc.activeDialog.state.dateTime) {
            // Save the dateTimePrompt result to dialog state
            dc.activeDialog.state.dateTime = result[0].value;
        }

        // If we don't have party size, ask for it next
        if (!dc.activeDialog.state.partySize) {
            await dc.prompt('textPrompt', "How many people are in your party?");
        } else {
            await next();
        }
    },
    async function(dc, result, next){
        if (!dc.activeDialog.state.partySize) {
            dc.activeDialog.state.partySize = result;
        }
        // Ask for the reservation name next
        await dc.prompt('textPrompt', "Whose name will this be under?");
    },
    async function(dc, result){
        dc.activeDialog.state.Name = result;

        // Save data to conversation state
        var state = conversationState.get(dc.context);

        // Copy the dialog state to the conversation state
        state = dc.activeDialog.state;

        // TODO: Add in <br/>Location: ${state.cafeLocation}
        var msg = `Reservation confirmed. Reservation details:             
            <br/>Date/Time: ${state.dateTime} 
            <br/>Party size: ${state.partySize} 
            <br/>Reservation name: ${state.Name}`;
            
        await dc.context.sendActivity(msg);
        await dc.end();
    }
]);
```

<span data-ttu-id="6f427-171">La función de ayuda `SaveEntities` busca entidades `datetime` y `partysize`.</span><span class="sxs-lookup"><span data-stu-id="6f427-171">The `SaveEntities` helper function checks for `datetime` and `partysize` entities.</span></span> <span data-ttu-id="6f427-172">La entidad `datetime` es una [entidad precompilada](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/luis-reference-prebuilt-entities#builtindatetimev2).</span><span class="sxs-lookup"><span data-stu-id="6f427-172">The `datetime` entity is a [prebuilt entity](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/luis-reference-prebuilt-entities#builtindatetimev2).</span></span>

```typescript
// Helper function that saves any entities found in the LUIS result
// to the dialog state
async function SaveEntities( dc: DialogContext<TurnContext>, typedresult) {
    // Resolve entities returned from LUIS, and save these to state
    if (typedresult.entities)
    {
        console.log(`Entities found.`);
        let datetime = typedresult.entities.datetime;

        if (datetime) {
            // Use the first date or time found in the utterance
            if (datetime[0].timex) {
                timexValues = datetime[0].timex;
                // Datetime results from LUIS are represented in timex format:
                // http://www.timeml.org/publications/timeMLdocs/timeml_1.2.1.html#timex3                                
                // More information on the library which does the recognition can be found here: 
                // https://github.com/Microsoft/Recognizers-Text

                if (datetime[0].type === "datetime") {
                    // To parse timex, here you use the resolve and creator from
                    // @microsoft/recognizers-text-data-types-timex-expression
                    // The second parameter is an array of constraints the results must satisfy
                    var resolution = Resolver.evaluate(
                        // array of timex values to evaluate. There may be more than one: "today at 6" can be 6AM or 6PM.
                        timexValues,
                        // constrain results to times between 4pm and 8pm                        
                        [Creator.evening]);
                    if (resolution[0]) {
                        // toNaturalLanguage takes the current date into account to create a friendly string
                        dc.activeDialog.state.dateTime = resolution[0].toNaturalLanguage(new Date());
                        // You can also use resolution.toString() to format the date/time.
                    } else {
                        // time didn't satisfy constraint.
                        dc.activeDialog.state.dateTime = null;
                    }
                } 
                else  {
                    console.log(`Type ${datetime[0].type} is not yet supported. Provide both the date and the time.`);
                }
            }                                                
        }
        let partysize = typedresult.entities.partySize;
        if (partysize) {
            console.log(`partysize entity detected.${partysize}`);
            // use first partySize entity that was found in utterance
            dc.activeDialog.state.partySize = partysize[0];
        }
        let cafelocation = typedresult.entities.cafeLocation;

        if (cafelocation) {
            console.log(`location entity detected.${cafelocation}`);
            // use first cafeLocation entity that was found in utterance
            dc.activeDialog.state.cafeLocation = cafelocation[0][0];
        }
    } 
}
```

## <a name="run-the-sample"></a><span data-ttu-id="6f427-173">Ejecución del ejemplo</span><span class="sxs-lookup"><span data-stu-id="6f427-173">Run the sample</span></span>

1. <span data-ttu-id="6f427-174">Si no tiene instalado el compilador de TypeScript, instálelo con este comando:</span><span class="sxs-lookup"><span data-stu-id="6f427-174">If you don't have the TypeScript compiler installed, install it using this command:</span></span>

```
npm install --global typescript
```

2. <span data-ttu-id="6f427-175">Instale las dependencias antes de ejecutar el bot; para ello, ejecute `npm install` en el directorio raíz de la muestra:</span><span class="sxs-lookup"><span data-stu-id="6f427-175">Install dependencies before you run the bot, by running `npm install` in the root directory of the sample:</span></span>

```
npm install
```

3. <span data-ttu-id="6f427-176">Desde el directorio raíz, compile la muestra usando `tsc`.</span><span class="sxs-lookup"><span data-stu-id="6f427-176">From the root directory, build the sample using `tsc`.</span></span> <span data-ttu-id="6f427-177">Esto generará `luisbot.js`.</span><span class="sxs-lookup"><span data-stu-id="6f427-177">This will generate `luisbot.js`.</span></span>

4. <span data-ttu-id="6f427-178">Ejecute `luisbot.js` en el directorio `lib`.</span><span class="sxs-lookup"><span data-stu-id="6f427-178">Run `luisbot.js` in the `lib` directory.</span></span>

5. <span data-ttu-id="6f427-179">Use [Bot Framework Emulator](https://docs.microsoft.com/en-us/azure/bot-service/bot-service-debug-emulator) para ejecutar la muestra.</span><span class="sxs-lookup"><span data-stu-id="6f427-179">Use the [Bot Framework Emulator](https://docs.microsoft.com/en-us/azure/bot-service/bot-service-debug-emulator) to run the sample.</span></span>

6. <span data-ttu-id="6f427-180">En el emulador, diga `reserve a table` para iniciar el cuadro de diálogo de reserva.</span><span class="sxs-lookup"><span data-stu-id="6f427-180">In the emulator, say `reserve a table` to start the reservation dialog.</span></span>

![ejecute el bot](media/how-to-luisgen/run-bot.png)

---


## <a name="additional-resources"></a><span data-ttu-id="6f427-182">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="6f427-182">Additional resources</span></span>

<span data-ttu-id="6f427-183">Para más información sobre LUIS, consulte [Language Understanding](./bot-builder-concept-luis.md).</span><span class="sxs-lookup"><span data-stu-id="6f427-183">For more background on LUIS, see [Language Understanding](./bot-builder-concept-luis.md).</span></span>


## <a name="next-steps"></a><span data-ttu-id="6f427-184">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="6f427-184">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="6f427-185">Combinar LUIS y QnA con la herramienta de distribución</span><span class="sxs-lookup"><span data-stu-id="6f427-185">Combine LUIS and QnA using the Dispatch tool</span></span>](./bot-builder-tutorial-dispatch.md)


