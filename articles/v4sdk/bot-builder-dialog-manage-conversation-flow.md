---
title: Administración de un flujo de conversación con diálogos | Microsoft Docs
description: Aprenda a administrar un flujo de conversación con diálogos en el SDK de Bot Builder para Node.js.
keywords: flujo de conversación, diálogos, mensajes, cascadas, conjunto de diálogos
author: v-ducvo
ms.author: v-ducvo
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 5/8/2018
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: 99184ba71072c159c598c7f68289c42a51926795
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305264"
---
# <a name="manage-conversation-flow-with-dialogs"></a><span data-ttu-id="d7b34-104">Administración del flujo de conversación con diálogos</span><span class="sxs-lookup"><span data-stu-id="d7b34-104">Manage conversation flow with dialogs</span></span>
[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]


<span data-ttu-id="d7b34-105">Administrar el flujo de conversación es una tarea esencial en la creación de bots.</span><span class="sxs-lookup"><span data-stu-id="d7b34-105">Managing conversation flow is an essential task in building bots.</span></span> <span data-ttu-id="d7b34-106">Con el SDK de Bot Builder, puede administrar el flujo de conversación mediante **diálogos**.</span><span class="sxs-lookup"><span data-stu-id="d7b34-106">With the Bot Builder SDK, you can manage conversation flow using **dialogs**.</span></span>

<span data-ttu-id="d7b34-107">Un diálogo es similar a una función de un programa.</span><span class="sxs-lookup"><span data-stu-id="d7b34-107">A dialog is like a function in a program.</span></span> <span data-ttu-id="d7b34-108">Por lo general está diseñado para realizar una operación específica y se puede invocar con la frecuencia que sea necesaria.</span><span class="sxs-lookup"><span data-stu-id="d7b34-108">It is generally designed to perform a specific operation and it can be invoked as often as it is needed.</span></span> <span data-ttu-id="d7b34-109">Puede encadenar varios diálogos juntos para así controlar casi cualquier flujo de conversación que quiera que administre el bot.</span><span class="sxs-lookup"><span data-stu-id="d7b34-109">You can chain multiple dialogs together to handle just about any conversation flow that you want your bot to handle.</span></span> <span data-ttu-id="d7b34-110">La biblioteca de **diálogos** del SDK de Bot Builder incluye características integradas como **mensajes** y **cascadas** para ayudarle a administrar el flujo de conversación mediante diálogos.</span><span class="sxs-lookup"><span data-stu-id="d7b34-110">The **dialogs** library in the Bot Builder SDK includes built-in features such as **prompts** and **waterfalls** to help you manage conversation flow through dialogs.</span></span> <span data-ttu-id="d7b34-111">La biblioteca de mensajes proporciona varios mensajes, que puede usar para pedir a los usuarios distintos tipos de información.</span><span class="sxs-lookup"><span data-stu-id="d7b34-111">The prompts library provides various prompts you can use to ask users for different types of information.</span></span> <span data-ttu-id="d7b34-112">Las cascadas proporcionan una manera de combinar varios pasos en una secuencia.</span><span class="sxs-lookup"><span data-stu-id="d7b34-112">The waterfalls provide a way for you to combine multiple steps together in a sequence.</span></span>

<span data-ttu-id="d7b34-113">En este artículo se muestra cómo crear un objeto de diálogos e insertar mensajes y pasos de cascada en un conjunto de diálogos para administrar flujos de conversación sencillos y complejos.</span><span class="sxs-lookup"><span data-stu-id="d7b34-113">This article will show you how to create a dialogs object and add prompts and waterfall steps into a dialog set to manage both simple conversation flows and complex conversation flows.</span></span> 

## <a name="install-the-dialogs-library"></a><span data-ttu-id="d7b34-114">Instalación de la biblioteca de diálogos</span><span class="sxs-lookup"><span data-stu-id="d7b34-114">Install the dialogs library</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="d7b34-115">C#</span><span class="sxs-lookup"><span data-stu-id="d7b34-115">C#</span></span>](#tab/csharp)
<span data-ttu-id="d7b34-116">Para usar diálogos, instale el paquete NuGet `Microsoft.Bot.Builder.Dialogs` para su proyecto o solución.</span><span class="sxs-lookup"><span data-stu-id="d7b34-116">To use dialogs, install the `Microsoft.Bot.Builder.Dialogs` NuGet package for your project or solution.</span></span>
<span data-ttu-id="d7b34-117">A continuación, haga referencia a la biblioteca de diálogos mediante instrucciones en los archivos de código.</span><span class="sxs-lookup"><span data-stu-id="d7b34-117">Then reference the dialogs library in using statements in your code files.</span></span> <span data-ttu-id="d7b34-118">Por ejemplo: </span><span class="sxs-lookup"><span data-stu-id="d7b34-118">For example:</span></span>

```csharp
using Microsoft.Bot.Builder.Dialogs;
```

# <a name="javascripttabjs"></a>[<span data-ttu-id="d7b34-119">JavaScript</span><span class="sxs-lookup"><span data-stu-id="d7b34-119">JavaScript</span></span>](#tab/js)
<span data-ttu-id="d7b34-120">La biblioteca `botbuilder-dialogs` se puede descargar desde NPM.</span><span class="sxs-lookup"><span data-stu-id="d7b34-120">The `botbuilder-dialogs` library can be downloaded from NPM.</span></span> <span data-ttu-id="d7b34-121">Para instalar la biblioteca `botbuilder-dialogs`, ejecute el siguiente comando de NPM:</span><span class="sxs-lookup"><span data-stu-id="d7b34-121">To install the `botbuilder-dialogs` library, run the following NPM command:</span></span>

```cmd
npm install --save botbuilder-dialogs
```

<span data-ttu-id="d7b34-122">Para usar **dialogs** en el bot, inclúyalo en el código del bot.</span><span class="sxs-lookup"><span data-stu-id="d7b34-122">To use **dialogs** in your bot, include it in the bot code.</span></span> <span data-ttu-id="d7b34-123">Por ejemplo: </span><span class="sxs-lookup"><span data-stu-id="d7b34-123">For example:</span></span>

<span data-ttu-id="d7b34-124">**app.js**</span><span class="sxs-lookup"><span data-stu-id="d7b34-124">**app.js**</span></span>

```javascript
const botbuilder_dialogs = require('botbuilder-dialogs');
```
---

## <a name="create-a-dialog-stack"></a><span data-ttu-id="d7b34-125">Creación de una pila de diálogos</span><span class="sxs-lookup"><span data-stu-id="d7b34-125">Create a dialog stack</span></span>

<span data-ttu-id="d7b34-126">Para usar diálogos, debe crear primero un *conjunto de diálogos*.</span><span class="sxs-lookup"><span data-stu-id="d7b34-126">To use dialogs, you must first create a *dialog set*.</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="d7b34-127">C#</span><span class="sxs-lookup"><span data-stu-id="d7b34-127">C#</span></span>](#tab/csharp)

<span data-ttu-id="d7b34-128">La biblioteca `Microsoft.Bot.Builder.Dialogs` proporciona una clase `DialogSet`.</span><span class="sxs-lookup"><span data-stu-id="d7b34-128">The `Microsoft.Bot.Builder.Dialogs` library provides a `DialogSet` class.</span></span>
<span data-ttu-id="d7b34-129">Para un conjunto de diálogos, puede agregar diálogos y conjuntos de diálogos con nombre y luego acceder a ellos más tarde por su nombre.</span><span class="sxs-lookup"><span data-stu-id="d7b34-129">To a dialog set you can add named dialogs and sets of dialogs and then access them by name later.</span></span>

```csharp
IDialog dialog = null;
// Initialize dialog.

DialogSet dialogs = new DialogSet();
dialogs.Add("dialog name", dialog);
```

# <a name="javascripttabjs"></a>[<span data-ttu-id="d7b34-130">JavaScript</span><span class="sxs-lookup"><span data-stu-id="d7b34-130">JavaScript</span></span>](#tab/js)

<span data-ttu-id="d7b34-131">La biblioteca `botbuilder-dialogs` proporciona una clase `DialogSet`.</span><span class="sxs-lookup"><span data-stu-id="d7b34-131">The `botbuilder-dialogs` library provides a `DialogSet` class.</span></span>
<span data-ttu-id="d7b34-132">La clase **DialogSet** define una **pila de diálogos** y le ofrece una sencilla interfaz para administrar la pila.</span><span class="sxs-lookup"><span data-stu-id="d7b34-132">The **DialogSet** class defines a **dialog stack** and gives you a simple interface to manage the stack.</span></span>
<span data-ttu-id="d7b34-133">El SDK de Bot Builder implementa la pila como una matriz.</span><span class="sxs-lookup"><span data-stu-id="d7b34-133">The Bot Builder SDK implements the stack as an array.</span></span>

<span data-ttu-id="d7b34-134">Para crear un elemento **DialogSet**, realice lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="d7b34-134">To create a **DialogSet**, do the following:</span></span>

```javascript
const dialogs = new botbuilder_dialogs.DialogSet();
```

<span data-ttu-id="d7b34-135">La llamada anterior creará un elemento **DialogSet** con una **pila de diálogos** predeterminada llamada `dialogStack`.</span><span class="sxs-lookup"><span data-stu-id="d7b34-135">The call above will create a **DialogSet** with a default **dialog stack** named `dialogStack`.</span></span>
<span data-ttu-id="d7b34-136">Si quiere asignar un nombre a la pila, puede pasarla como un parámetro a **DialogSet()**.</span><span class="sxs-lookup"><span data-stu-id="d7b34-136">If you want to name your stack, you can pass it in as a parameter to **DialogSet()**.</span></span> <span data-ttu-id="d7b34-137">Por ejemplo: </span><span class="sxs-lookup"><span data-stu-id="d7b34-137">For example:</span></span>

```javascript
const dialogs = new botbuilder_dialogs.DialogSet("myStack");
```
---

<span data-ttu-id="d7b34-138">Al crear un diálogo solo se agrega la definición del diálogo al conjunto.</span><span class="sxs-lookup"><span data-stu-id="d7b34-138">Creating a dialog only adds the dialog definition to the set.</span></span> <span data-ttu-id="d7b34-139">El diálogo no se ejecuta hasta que se inserta en la pila de diálogos mediante la llamada a un método _begin_ o _replace_.</span><span class="sxs-lookup"><span data-stu-id="d7b34-139">The dialog is not run until it is pushed onto the dialog stack by calling a _begin_ or _replace_ method.</span></span> 

<span data-ttu-id="d7b34-140">El nombre del diálogo (por ejemplo, `addTwoNumbers`) debe ser único dentro de cada conjunto de diálogos.</span><span class="sxs-lookup"><span data-stu-id="d7b34-140">The dialog name (for example, `addTwoNumbers`) must be unique within each dialog set.</span></span> <span data-ttu-id="d7b34-141">Puede definir tantos diálogos como sea necesario dentro de cada conjunto.</span><span class="sxs-lookup"><span data-stu-id="d7b34-141">You can define as many dialogs as necessary within each set.</span></span>

<span data-ttu-id="d7b34-142">La biblioteca de diálogos define los siguientes diálogos:</span><span class="sxs-lookup"><span data-stu-id="d7b34-142">The dialog library defines the following dialogs:</span></span>
-   <span data-ttu-id="d7b34-143">Un diálogo de **mensaje** donde el diálogo usa al menos dos funciones, una para pedir al usuario que intervenga y la otra para procesar la entrada.</span><span class="sxs-lookup"><span data-stu-id="d7b34-143">A **prompt** dialog where the dialog uses at least two functions, one to prompt the user for input and the other to process the input.</span></span>
    <span data-ttu-id="d7b34-144">Los diálogos se pueden encadenar juntos con el modelo de **cascada**.</span><span class="sxs-lookup"><span data-stu-id="d7b34-144">You can string these together using the **waterfall** model.</span></span>
-   <span data-ttu-id="d7b34-145">Un diálogo de **cascada** define una secuencia de _pasos de cascada_, que se ejecutan en orden.</span><span class="sxs-lookup"><span data-stu-id="d7b34-145">A **waterfall** dialog defines a sequence of _waterfall steps_, which run in order.</span></span>
    <span data-ttu-id="d7b34-146">Un diálogo de cascada puede tener un solo paso, en cuyo caso se puede considerar como un diálogo sencillo de un único paso.</span><span class="sxs-lookup"><span data-stu-id="d7b34-146">A waterfall dialog can have a single step, in which case it can be thought of as a simple, one-step dialog.</span></span>

## <a name="create-a-single-step-dialog"></a><span data-ttu-id="d7b34-147">Creación de un diálogo de un único paso</span><span class="sxs-lookup"><span data-stu-id="d7b34-147">Create a single-step dialog</span></span>

<span data-ttu-id="d7b34-148">Los diálogos de un único paso pueden ser útiles para capturar flujos conversacionales de un solo turno.</span><span class="sxs-lookup"><span data-stu-id="d7b34-148">Single-step dialogs can be useful for capturing single-turn conversational flows.</span></span> <span data-ttu-id="d7b34-149">En este ejemplo se crea un bot que puede detectar si el usuario dice algo parecido a "1 + 2" y se inicia un diálogo `addTwoNumbers` para responder con "1 + 2 = 3".</span><span class="sxs-lookup"><span data-stu-id="d7b34-149">This example creates a bot that can detect if the user says something like "1 + 2", and starts an `addTwoNumbers` dialog to reply with "1 + 2 = 3".</span></span> 

# <a name="ctabcsharp"></a>[<span data-ttu-id="d7b34-150">C#</span><span class="sxs-lookup"><span data-stu-id="d7b34-150">C#</span></span>](#tab/csharp)

<span data-ttu-id="d7b34-151">Se pasan valores a los diálogos y se devuelven de estos como contenedores de propiedades `IDictionary<string,object>`.</span><span class="sxs-lookup"><span data-stu-id="d7b34-151">Values are passed into and returned from dialogs as `IDictionary<string,object>` property bags.</span></span>

<span data-ttu-id="d7b34-152">Para crear un diálogo sencillo dentro de un conjunto de diálogos, use el método `Add`.</span><span class="sxs-lookup"><span data-stu-id="d7b34-152">To create a simple dialog within a dialog set, use the `Add` method.</span></span> <span data-ttu-id="d7b34-153">El siguiente código agrega una cascada de un solo paso llamada `addTwoNumbers`.</span><span class="sxs-lookup"><span data-stu-id="d7b34-153">The following adds a one-step waterfall named `addTwoNumbers`.</span></span>

<span data-ttu-id="d7b34-154">En este paso se da por hecho que los argumentos de diálogo que se pasan contienen las propiedades `first` y `second` que representan los números que se van a agregar.</span><span class="sxs-lookup"><span data-stu-id="d7b34-154">This step assumes that the dialog arguments getting passed in contain `first` and `second` properties that represent the numbers to be added.</span></span>

<span data-ttu-id="d7b34-155">Comience con la plantilla EchoBot.</span><span class="sxs-lookup"><span data-stu-id="d7b34-155">Start with the EchoBot template.</span></span> <span data-ttu-id="d7b34-156">A continuación, agregue código en la clase de bot para agregar el diálogo en el constructor.</span><span class="sxs-lookup"><span data-stu-id="d7b34-156">Then add code in your bot class to add the dialog in the constructor.</span></span>
```csharp
public class EchoBot : IBot
{
    private DialogSet _dialogs;

    public EchoBot()
    {
        _dialogs = new DialogSet();
        _dialogs.Add("addTwoNumbers", new WaterfallStep[]
        {              
            async (dc, args, next) =>
            {
                double sum = (double)args["first"] + (double)args["second"];
                await dc.Context.SendActivity($"{args["first"]} + {args["second"]} = {sum}");
                await dc.End();
            }
        });
    }

    // The rest of the class definition is omitted here but would include OnTurn()
}

```

### <a name="pass-arguments-to-the-dialog"></a><span data-ttu-id="d7b34-157">Paso de argumentos al diálogo</span><span class="sxs-lookup"><span data-stu-id="d7b34-157">Pass arguments to the dialog</span></span>

<span data-ttu-id="d7b34-158">Para llamar al diálogo desde dentro del método `OnTurn` del bot, modifique `OnTurn` para que contenga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="d7b34-158">To call the dialog from within your bot's `OnTurn` method, modify `OnTurn` to contain the following:</span></span>
```cs
public async Task OnTurn(ITurnContext context)
{
    // This bot is only handling Messages
    if (context.Activity.Type == ActivityTypes.Message)
    {
        // Get the conversation state from the turn context
        var state = context.GetConversationState<EchoState>();

        // create a dialog context
        var dialogCtx = _dialogs.CreateContext(context, state);

        // Bump the turn count. 
        state.TurnCount++;

        await dialogCtx.Continue();
        if (!context.Responded)
        {
            // Call a helper function that identifies if the user says something 
            // like "2 + 3" or "1.25 + 3.28" and extract the numbers to add            
            if (TryParseAddingTwoNumbers(context.Activity.Text, out double first, out double second))
            { 
                var dialogArgs = new Dictionary<string, object>
                {
                    ["first"] = first,
                    ["second"] = second
                };                        
                await dialogCtx.Begin("addTwoNumbers", dialogArgs);
            }
            else
            {
                // Echo back to the user whatever they typed.
                await context.SendActivity($"Turn: {state.TurnCount}. You said '{context.Activity.Text}'");
            }
        }
    }
}
```

<span data-ttu-id="d7b34-159">Agregue la función auxiliar a la clase de bot.</span><span class="sxs-lookup"><span data-stu-id="d7b34-159">Add the helper function to the bot class.</span></span> <span data-ttu-id="d7b34-160">La función auxiliar simplemente usa una expresión regular sencilla para detectar si el mensaje del usuario es una solicitud para sumar 2 números.</span><span class="sxs-lookup"><span data-stu-id="d7b34-160">The helper function just uses a simple regex to detect if the user's message is a request to add 2 numbers.</span></span>

```cs
// Recognizes if the message is a request to add 2 numbers, in the form: number + number, 
// where number may have optionally have a decimal point.: 1 + 1, 123.99 + 45, 0.4+7. 
// For the sake of simplicity it doesn't handle negative numbers or numbers like 1,000 that contain a comma.
// If you need more robust number recognition, try System.Recognizers.Text
public bool TryParseAddingTwoNumbers(string message, out double first, out double second)
{
    // captures a number with optional -/+ and optional decimal portion
    const string NUMBER_REGEXP = "([-+]?(?:[0-9]+(?:\\.[0-9]+)?|\\.[0-9]+))";
    // matches the plus sign with optional spaces before and after it
    const string PLUSSIGN_REGEXP = "(?:\\s*)\\+(?:\\s*)";
    const string ADD_TWO_NUMBERS_REGEXP = NUMBER_REGEXP + PLUSSIGN_REGEXP + NUMBER_REGEXP;
    var regex = new Regex(ADD_TWO_NUMBERS_REGEXP);
    var matches = regex.Matches(message);
    var succeeded = false;
    first = 0;
    second = 0;
    if (matches.Count == 0)
    {
        succeeded = false;
    }
    else
    {
        var matched = matches[0];
        if ( System.Double.TryParse(matched.Groups[1].Value, out first) 
            && System.Double.TryParse(matched.Groups[2].Value, out second))
        {
            succeeded = true;
        } 
    }
    return succeeded;
}
```

<span data-ttu-id="d7b34-161">Si está usando la plantilla EchoBot, modifique la clase `EchoState` en **EchoState.cs** de la manera siguiente:</span><span class="sxs-lookup"><span data-stu-id="d7b34-161">If you're using the EchoBot template, modify the `EchoState` class in **EchoState.cs** as follows:</span></span>

```cs
/// <summary>
/// Class for storing conversation state.
/// This bot only stores the turn count in order to echo it to the user
/// </summary>
public class EchoState: Dictionary<string, object>
{
    private const string TurnCountKey = "TurnCount";
    public EchoState()
    {
        this[TurnCountKey] = 0;            
    }

    public int TurnCount
    {
        get { return (int)this[TurnCountKey]; }
        set { this[TurnCountKey] = value; }
    }
}
```

# <a name="javascripttabjs"></a>[<span data-ttu-id="d7b34-162">JavaScript</span><span class="sxs-lookup"><span data-stu-id="d7b34-162">JavaScript</span></span>](#tab/js)

<span data-ttu-id="d7b34-163">Comience con la plantilla JS descrita en [Create a bot with the Bot Builder SDK v4](../javascript/bot-builder-javascript-quickstart.md) (Creación de un bot con el SDK de Bot Builder v4).</span><span class="sxs-lookup"><span data-stu-id="d7b34-163">Start with the JS template described in [Create a bot with the Bot Builder SDK v4](../javascript/bot-builder-javascript-quickstart.md).</span></span> <span data-ttu-id="d7b34-164">En **app.js**, agregue una instrucción para exigir `botbuilder-dialogs`.</span><span class="sxs-lookup"><span data-stu-id="d7b34-164">In **app.js**, add a statement to require `botbuilder-dialogs`.</span></span>
```js
const {DialogSet} = require('botbuilder-dialogs');
```

<span data-ttu-id="d7b34-165">En **app.js**, agregue el código siguiente que define un diálogo sencillo llamado `addTwoNumbers` que pertenece al conjunto `dialogs`:</span><span class="sxs-lookup"><span data-stu-id="d7b34-165">In **app.js**, add the following code that defines a simple dialog named `addTwoNumbers` that belongs to the `dialogs` set:</span></span>

```javascript
const dialogs = new DialogSet("myDialogStack");

// Show the sum of two numbers.
dialogs.add('addTwoNumbers', [async function (dc, numbers){
        var sum = Number.parseFloat(numbers[0]) + Number.parseFloat(numbers[1]);
        await dc.context.sendActivity(`${numbers[0]} + ${numbers[1]} = ${sum}`);
        await dc.end();
    }]
);
```

<span data-ttu-id="d7b34-166">Reemplace el código de **app.js** para procesar la actividad entrante por el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="d7b34-166">Replace the code in **app.js** for processing the incoming activity with the following.</span></span> <span data-ttu-id="d7b34-167">El bot llama a una función auxiliar para comprobar si el mensaje entrante se parece a una solicitud para sumar dos números.</span><span class="sxs-lookup"><span data-stu-id="d7b34-167">The bot calls a helper function to check if the incoming message looks like a request for adding two numbers.</span></span> <span data-ttu-id="d7b34-168">En caso afirmativo, pasa los números del argumento a `DialogContext.begin`.</span><span class="sxs-lookup"><span data-stu-id="d7b34-168">If it does, it passes the numbers in the argument to `DialogContext.begin`.</span></span>

```js
// Listen for incoming activity 
server.post('/api/messages', (req, res) => {
    // Route received activity to adapter for processing
    adapter.processActivity(req, res, async (context) => {
        const isMessage = context.activity.type === 'message';
        if (isMessage) {
            const state = conversationState.get(context);
            const count = state.count === undefined ? state.count = 0 : ++state.count;

            // create a dialog context
            const dc = dialogs.createContext(context, state);

            // MatchesAdd2Numbers checks if the message matches a regular expression
            // and if it does, returns an array of the numbers to add
            var numbers = await MatchesAdd2Numbers(context.activity.text); 
            if (numbers != null && numbers.length >=2 )
            {    
                await dc.begin('addTwoNumbers', numbers);
            }
            else {
                // Just echo back the user's message if they're not adding numbers
                return context.sendActivity(`Turn ${count}: You said "${context.activity.text}"`); 
            }           
        } else {
            return context.sendActivity(`[${context.activity.type} event detected]`);
        }
        if (!context.responded) {
            await dc.continue();
            // if the dialog didn't send a response
            if (!context.responded && isMessage) {
                await dc.context.sendActivity(`Hi! I'm the add 2 numbers bot. Say something like "what's 1+2?"`);
            }
        }
    });
});
```

<span data-ttu-id="d7b34-169">Agregue la función auxiliar a **app.js**.</span><span class="sxs-lookup"><span data-stu-id="d7b34-169">Add the helper function to **app.js**.</span></span> <span data-ttu-id="d7b34-170">La función auxiliar simplemente usa una expresión regular sencilla para detectar si el mensaje del usuario es una solicitud para sumar 2 números.</span><span class="sxs-lookup"><span data-stu-id="d7b34-170">The helper function just uses a simple regular expression to detect if the user's message is a request to add 2 numbers.</span></span> <span data-ttu-id="d7b34-171">Si la expresión regular coincide, devuelve una matriz que contiene los números que se van a sumar.</span><span class="sxs-lookup"><span data-stu-id="d7b34-171">If the regular expression matches, it returns an array that contains the numbers to add.</span></span>

```javascript
async function MatchesAdd2Numbers(message) {
    const ADD_NUMBERS_REGEXP = /([-+]?(?:[0-9]+(?:\.[0-9]+)?|\.[0-9]+))(?:\s*)\+(?:\s*)([-+]?(?:[0-9]+(?:\.[0-9]+)?|\.[0-9]+))/i;
    let matched = ADD_NUMBERS_REGEXP.exec(message);
    if (!matched) {
        // message wasn't a request to add 2 numbers
        return null;
    }
    else {
        var numbers = [matched[1], matched[2]];
        return numbers;
    }
}
```

---

### <a name="run-the-bot"></a><span data-ttu-id="d7b34-172">Ejecución del bot</span><span class="sxs-lookup"><span data-stu-id="d7b34-172">Run the bot</span></span>

<span data-ttu-id="d7b34-173">Intente ejecutar el bot en Bot Framework Emulator y dígale algo como </span><span class="sxs-lookup"><span data-stu-id="d7b34-173">Try running the bot in the Bot Framework Emulator, and say things like "what's 1+1?"</span></span> <span data-ttu-id="d7b34-174">"¿Cuánto es 1 + 1?"</span><span class="sxs-lookup"><span data-stu-id="d7b34-174">to it.</span></span>

![ejecutar el bot](./media/how-to-dialogs/bot-output-add-numbers.png)



## <a name="using-dialogs-to-guide-the-user-through-steps"></a><span data-ttu-id="d7b34-176">Uso de diálogos para guiar al usuario paso a paso</span><span class="sxs-lookup"><span data-stu-id="d7b34-176">Using dialogs to guide the user through steps</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="d7b34-177">C#</span><span class="sxs-lookup"><span data-stu-id="d7b34-177">C#</span></span>](#tab/csharp)

### <a name="create-a-composite-dialog"></a><span data-ttu-id="d7b34-178">Creación de un diálogo compuesto</span><span class="sxs-lookup"><span data-stu-id="d7b34-178">Create a composite dialog</span></span>

<span data-ttu-id="d7b34-179">Los siguientes fragmentos de código se toman del ejemplo de código [Microsoft.Bot.Samples.Dialog.Prompts](https://github.com/Microsoft/botbuilder-dotnet/tree/master/samples/MIcrosoft.Bot.Samples.Dialog.Prompts) del repositorio botbuilder-dotnet.</span><span class="sxs-lookup"><span data-stu-id="d7b34-179">The following snippets are taken from the [Microsoft.Bot.Samples.Dialog.Prompts](https://github.com/Microsoft/botbuilder-dotnet/tree/master/samples/MIcrosoft.Bot.Samples.Dialog.Prompts) sample code in the botbuilder-dotnet repo.</span></span>

<span data-ttu-id="d7b34-180">En Startup.cs:</span><span class="sxs-lookup"><span data-stu-id="d7b34-180">In Startup.cs:</span></span>
1.  <span data-ttu-id="d7b34-181">Cambie el nombre del bot por `DialogContainerBot`.</span><span class="sxs-lookup"><span data-stu-id="d7b34-181">Rename your bot to `DialogContainerBot`.</span></span>
1.  <span data-ttu-id="d7b34-182">Use un diccionario sencillo como contenedor de propiedades para el estado de conversación del bot.</span><span class="sxs-lookup"><span data-stu-id="d7b34-182">Use a simple dictionary as a property bag for the conversation state for the bot.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddBot<DialogContainerBot>(options =>
    {
        options.CredentialProvider = new ConfigurationCredentialProvider(Configuration);
        options.Middleware.Add(new ConversationState<Dictionary<string, object>>(new MemoryStorage()));
    });
}
```

<span data-ttu-id="d7b34-183">Cambie el nombre de `EchoBot` por `DialogContainerBot`.</span><span class="sxs-lookup"><span data-stu-id="d7b34-183">Rename your `EchoBot` to `DialogContainerBot`.</span></span>

<span data-ttu-id="d7b34-184">En `DialogContainerBot.cs`, defina una clase para un diálogo de perfil.</span><span class="sxs-lookup"><span data-stu-id="d7b34-184">In `DialogContainerBot.cs`, define a class for a profile dialog.</span></span>

```csharp
public class ProfileControl : DialogContainer
{
    public ProfileControl()
        : base("fillProfile")
    {
        Dialogs.Add("fillProfile", 
            new WaterfallStep[]
            {
                async (dc, args, next) =>
                {
                    dc.ActiveDialog.State = new Dictionary<string, object>();
                    await dc.Prompt("textPrompt", "What's your name?");
                },
                async (dc, args, next) =>
                {
                    dc.ActiveDialog.State["name"] = args["Value"];
                    await dc.Prompt("textPrompt", "What's your phone number?");
                },
                async (dc, args, next) =>
                {
                    dc.ActiveDialog.State["phone"] = args["Value"];
                    await dc.End(dc.ActiveDialog.State);
                }
            }
        );
        Dialogs.Add("textPrompt", new Builder.Dialogs.TextPrompt());
    }
}
```

<span data-ttu-id="d7b34-185">A continuación, en la definición de bot, declare un campo como diálogo principal del bot e inicialícelo en el constructor del bot.</span><span class="sxs-lookup"><span data-stu-id="d7b34-185">Then, within the bot definition, declare a field for the bot's main dialog and initialize it in the bot's constructor.</span></span>
<span data-ttu-id="d7b34-186">El diálogo principal del bot incluye el diálogo de perfil.</span><span class="sxs-lookup"><span data-stu-id="d7b34-186">The bot's main dialog includes the profile dialog.</span></span>

```csharp
private DialogSet _dialogs;

public DialogContainerBot()
{
    _dialogs = new DialogSet();

    _dialogs.Add("getProfile", new ProfileControl());
    _dialogs.Add("firstRun",
        new WaterfallStep[]
        {
            async (dc, args, next) =>
            {
                    await dc.Context.SendActivity("Welcome! We need to ask a few questions to get started.");
                    await dc.Begin("getProfile");
            },
            async (dc, args, next) =>
            {
                await dc.Context.SendActivity($"Thanks {args["name"]} I have your phone number as {args["phone"]}!");
                await dc.End();
            }
        }
    );
}
```

<span data-ttu-id="d7b34-187">En el método `OnTurn` del bot:</span><span class="sxs-lookup"><span data-stu-id="d7b34-187">In the bot's `OnTurn` method:</span></span>
-   <span data-ttu-id="d7b34-188">Salude al usuario cuando se inicie la conversación.</span><span class="sxs-lookup"><span data-stu-id="d7b34-188">Greet the user when the conversation starts.</span></span>
-   <span data-ttu-id="d7b34-189">Inicialice y _continúe_ el diálogo principal cada vez que se reciba un mensaje del usuario.</span><span class="sxs-lookup"><span data-stu-id="d7b34-189">Initialize and _continue_ the main dialog whenever we get a message from the user.</span></span>

    <span data-ttu-id="d7b34-190">Si el diálogo no ha generado una respuesta, debe asumir que lo completó anteriormente o que aún no se ha iniciado y _comenzarlo_, mediante la especificación del nombre de diálogo del conjunto con el que empezar.</span><span class="sxs-lookup"><span data-stu-id="d7b34-190">If the dialog hasn't generated a response, assume that it completed earlier or hasn't started yet and _begin_ it, specifying the name of the dialog in the set to start with.</span></span>

```csharp
public async Task OnTurn(ITurnContext turnContext)
{
    try
    {
        switch (turnContext.Activity.Type)
        {
            case ActivityTypes.ConversationUpdate:
                foreach (var newMember in turnContext.Activity.MembersAdded)
                {
                    if (newMember.Id != turnContext.Activity.Recipient.Id)
                    {
                        await turnContext.SendActivity("Hello and welcome to the Composite Control bot.");
                    }
                }
                break;

            case ActivityTypes.Message:
                var state = ConversationState<Dictionary<string, object>>.Get(turnContext);
                var dc = _dialogs.CreateContext(turnContext, state);

                await dc.Continue();

                if (!turnContext.Responded)
                {
                    await dc.Begin("firstRun");
                }

                break;
        }
    }
    catch (Exception e)
    {
        await turnContext.SendActivity($"Exception: {e.Message}");
    }
}

```

# <a name="javascripttabjs"></a>[<span data-ttu-id="d7b34-191">JavaScript</span><span class="sxs-lookup"><span data-stu-id="d7b34-191">JavaScript</span></span>](#tab/js)

### <a name="create-a-dialog-with-waterfall-steps"></a><span data-ttu-id="d7b34-192">Creación de un diálogo con pasos de cascada</span><span class="sxs-lookup"><span data-stu-id="d7b34-192">Create a dialog with waterfall steps</span></span>

<span data-ttu-id="d7b34-193">Una conversación se compone de una serie de mensajes intercambiados entre un usuario y un bot.</span><span class="sxs-lookup"><span data-stu-id="d7b34-193">A conversation consists of a series of messages exchanged between user and bot.</span></span> <span data-ttu-id="d7b34-194">Cuando el objetivo del bot es llevar al usuario a través de una serie de pasos, puede usar un modelo de **cascada** para definir los pasos de la conversación.</span><span class="sxs-lookup"><span data-stu-id="d7b34-194">When the bot's objective is to lead the user through a series of steps, you can use a **waterfall** model to define the steps of the conversation.</span></span>

<span data-ttu-id="d7b34-195">Una **cascada** es una implementación específica de un diálogo que se usa normalmente para recopilar información del usuario o guiarlo en una serie de tareas.</span><span class="sxs-lookup"><span data-stu-id="d7b34-195">A **waterfall** is a specific implementation of a dialog that is most commonly used to collect information from the user or guide the user through a series of tasks.</span></span> <span data-ttu-id="d7b34-196">Las tareas se implementan como una matriz de funciones, donde el resultado de la primera función se pasa como argumentos a la función siguiente y así sucesivamente.</span><span class="sxs-lookup"><span data-stu-id="d7b34-196">The tasks are implemented as an array of functions where the result of the first function is passed as arguments into the next function, and so on.</span></span> <span data-ttu-id="d7b34-197">Cada función representa normalmente un paso del proceso general.</span><span class="sxs-lookup"><span data-stu-id="d7b34-197">Each function typically represents one step in the overall process.</span></span> <span data-ttu-id="d7b34-198">En cada paso, el bot [pide al usuario que intervenga](bot-builder-prompts.md), espera una respuesta y, a continuación, pasa el resultado al paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="d7b34-198">At each step, the bot [prompts the user for input](bot-builder-prompts.md), waits for a response, and then passes the result to the next step.</span></span>

<span data-ttu-id="d7b34-199">Por ejemplo, el ejemplo de código siguiente define tres funciones en una matriz que representa los tres pasos de una **cascada**:</span><span class="sxs-lookup"><span data-stu-id="d7b34-199">For example, the following code sample defines three functions in an array that represents the three steps of a **waterfall**:</span></span>

```javascript
// Greet user:
// Ask for the user name and then greet them by name.
// Ask them where they work.
dialogs.add('greetings',[
    async function (dc){
        await dc.prompt('textPrompt', 'What is your name?');
    },
    async function(dc, results){
        var userName = results;
        await dc.context.sendActivity(`Hi ${userName}!`);
        await dc.prompt('textPrompt', 'Where do you work?');
    },
    async function(dc, results){
        var workPlace = results;
        await dc.context.sendActivity(`${workPlace} is a fun place.`);
        await dc.end(); // Ends the dialog
    }
]);

dialogs.add('textPrompt', new botbuilder_dialogs.TextPrompt());
```

<span data-ttu-id="d7b34-200">La signatura del paso de una **cascada** es de la manera siguiente:</span><span class="sxs-lookup"><span data-stu-id="d7b34-200">The signature for a **waterfall** step is as follows:</span></span>

| <span data-ttu-id="d7b34-201">Parámetro</span><span class="sxs-lookup"><span data-stu-id="d7b34-201">Parameter</span></span> | <span data-ttu-id="d7b34-202">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="d7b34-202">Description</span></span> |
| ---- | ----- |
| `context` | <span data-ttu-id="d7b34-203">El contexto del diálogo.</span><span class="sxs-lookup"><span data-stu-id="d7b34-203">The dialog context.</span></span> |
| `args` | <span data-ttu-id="d7b34-204">Opcional, contiene los argumentos pasados en el paso.</span><span class="sxs-lookup"><span data-stu-id="d7b34-204">Optional, contains argument(s) passed into the step.</span></span> |
| `next` | <span data-ttu-id="d7b34-205">Opcional, un método que le permite continuar con el paso siguiente de la cascada.</span><span class="sxs-lookup"><span data-stu-id="d7b34-205">Optional, a method that allows you to proceed to the next step of the waterfall.</span></span> <span data-ttu-id="d7b34-206">Puede proporcionar un argumento *args* cuando llame a este método, lo que le permite pasar los argumentos al siguiente paso de la cascada.</span><span class="sxs-lookup"><span data-stu-id="d7b34-206">You can provide an *args* argument when you call this mehtod, allowing you to pass argument(s) to the next step in the waterfall.</span></span> |

<span data-ttu-id="d7b34-207">Cada paso debe llamar a uno de los métodos siguientes antes de devolver: *next()*, *dialogs.prompt()*, *dialogs.end()*, *dialogs.begin()* o *Promise.resolve()*; de lo contrario, el bot se detendrá en ese paso.</span><span class="sxs-lookup"><span data-stu-id="d7b34-207">Each step must call one of the following methods before returning: *next()*, *dialogs.prompt()*, *dialogs.end()*, *dialogs.begin()*, or *Promise.resolve()*; otherwise, the bot will be stuck in that step.</span></span> <span data-ttu-id="d7b34-208">Es decir, si una función no devuelve uno de estos métodos, todas las intervenciones de los usuarios harán que se vuelva a ejecutar este paso cada vez que el usuario envía al bot un mensaje.</span><span class="sxs-lookup"><span data-stu-id="d7b34-208">That is, if a function does not return one of these methods then all user input will cause this step to be re-executed each time the user sent the bot a message.</span></span>

<span data-ttu-id="d7b34-209">Cuando se alcanza el final de la cascada, es recomendable volver con el método `end()` para que el diálogo se pueda sacar de la pila.</span><span class="sxs-lookup"><span data-stu-id="d7b34-209">When you reached the end of the waterfall, it is best practice to return with the `end()` method so that the dialog can be popped off the stack.</span></span> <span data-ttu-id="d7b34-210">Consulte la sección [Finalización de un diálogo](#end-a-dialog) para más información.</span><span class="sxs-lookup"><span data-stu-id="d7b34-210">See [End a dialog](#end-a-dialog) section for more information.</span></span> <span data-ttu-id="d7b34-211">Igualmente, para continuar desde un paso al siguiente, el paso de cascada debe finalizar con un mensaje o llamar explícitamente a la función `next()` para avanzar por la cascada.</span><span class="sxs-lookup"><span data-stu-id="d7b34-211">Likewise, to proceed from one step to the next, the waterfall step must end with either a prompt or explicitly call the `next()` function to advance the waterfall.</span></span> 

### <a name="start-a-dialog"></a><span data-ttu-id="d7b34-212">Inicio de un diálogo</span><span class="sxs-lookup"><span data-stu-id="d7b34-212">Start a dialog</span></span>

<span data-ttu-id="d7b34-213">Para iniciar un diálogo, pase el elemento *dialogId* que quiere iniciar a los métodos `begin()`, `prompt()` o `replace()`.</span><span class="sxs-lookup"><span data-stu-id="d7b34-213">To start a dialog, pass the *dialogId* you want to start into the `begin()`, `prompt()`, or `replace()` methods.</span></span> <span data-ttu-id="d7b34-214">El método **begin** insertará el diálogo al principio de la pila, mientras que el método **replace** sacará el diálogo actual de la pila e insertará el diálogo sustituto en ella.</span><span class="sxs-lookup"><span data-stu-id="d7b34-214">The **begin** method will push the dialog onto the top of the stack while the **replace** method will pop the current dialog off the stack and pushes the replacing dialog onto the stack.</span></span>

<span data-ttu-id="d7b34-215">Para iniciar un diálogo sin argumentos:</span><span class="sxs-lookup"><span data-stu-id="d7b34-215">To start a dialog without arguments:</span></span>

```javascript
// Start the 'greetings' dialog.
await dc.begin('greetings');
```

<span data-ttu-id="d7b34-216">Para iniciar un diálogo con argumentos:</span><span class="sxs-lookup"><span data-stu-id="d7b34-216">To start a dialog with arguments:</span></span>

```javascript
// Start the 'greetings' dialog with the 'userName' passed in. 
await dc.begin('greetings', userName);
```

<span data-ttu-id="d7b34-217">Para iniciar un diálogo de **mensajes**:</span><span class="sxs-lookup"><span data-stu-id="d7b34-217">To start a **prompt** dialog:</span></span>

```javascript
// Start a 'choicePrompt' dialog with choices passed in as an array of colors to choose from.
await dc.prompt('choicePrompt', `choice: select a color`, ['red', 'green', 'blue']);
```

<span data-ttu-id="d7b34-218">Según el tipo de símbolo de mensaje que inicie, la signatura del argumento del mensaje puede ser diferente.</span><span class="sxs-lookup"><span data-stu-id="d7b34-218">Depending on the type of prompt you are starting, the prompt's argument signature may be different.</span></span> <span data-ttu-id="d7b34-219">El método **DialogSet.prompt** es un método auxiliar.</span><span class="sxs-lookup"><span data-stu-id="d7b34-219">The **DialogSet.prompt** method is a helper method.</span></span> <span data-ttu-id="d7b34-220">Este método toma argumentos y construye las opciones adecuadas para el mensaje; a continuación, llama al método **begin** para iniciar el diálogo de mensajes.</span><span class="sxs-lookup"><span data-stu-id="d7b34-220">This method takes in arguments and constructs the appropriate options for the prompt; then, it calls the **begin** method to start the prompt dialog.</span></span>

<span data-ttu-id="d7b34-221">Para reemplazar un diálogo de la pila:</span><span class="sxs-lookup"><span data-stu-id="d7b34-221">To replace a dialog on the stack:</span></span>

```javascript
// End the current dialog and start the 'mainMenu' dialog.
await dc.replace('mainMenu'); // Can optionally passed in an 'args' as the second argument.
```

<span data-ttu-id="d7b34-222">Puede encontrar más información sobre cómo usar el método **replace()** en las secciones [Repetición de un diálogo](#repeat-a-dialog) y [Bucles de diálogos](#dialog-loops) a continuación.</span><span class="sxs-lookup"><span data-stu-id="d7b34-222">More details on how to use the **replace()** method in the [Repeat a dialog](#repeat-a-dialog) and [Dialog loops](#dialog-loops) sections below.</span></span>

## <a name="end-a-dialog"></a><span data-ttu-id="d7b34-223">Finalización de un diálogo</span><span class="sxs-lookup"><span data-stu-id="d7b34-223">End a dialog</span></span>

<span data-ttu-id="d7b34-224">Para finaliza un diálogo, sáquelo de la pila y devuelva un resultado opcional al diálogo principal.</span><span class="sxs-lookup"><span data-stu-id="d7b34-224">End a dialog by popping it off the stack and returns an optional result to the parent dialog.</span></span> <span data-ttu-id="d7b34-225">El método **Dialog.resume()** del diálogo principal se invoca con cualquier resultado devuelto.</span><span class="sxs-lookup"><span data-stu-id="d7b34-225">The parent dialog will have its **Dialog.resume()** method invoked with any returned result.</span></span>

<span data-ttu-id="d7b34-226">Es recomendable llamar explícitamente al método `end()` al final del diálogo; sin embargo, no es obligatorio porque el diálogo se saca automáticamente de la pila cuando se llega al final de la cascada.</span><span class="sxs-lookup"><span data-stu-id="d7b34-226">It is best practice to explicitly call the `end()` method at the end of the dialog; however, it is not required because the dialog will automatically be popped off the stack for you when you reach the end of the waterfall.</span></span>

<span data-ttu-id="d7b34-227">Para finalizar un diálogo:</span><span class="sxs-lookup"><span data-stu-id="d7b34-227">To end a dialog:</span></span>

```javascript
// End the current dialog by popping it off the stack
await dc.end();
```

<span data-ttu-id="d7b34-228">Para finalizar un diálogo con argumentos opcionales pasados al diálogo principal:</span><span class="sxs-lookup"><span data-stu-id="d7b34-228">To end a dialog with optional argument(s) passed to the parent dialog:</span></span>

```javascript
// End the current dialog and pass a result to the parent dialog
await dc.end(result);
```

<span data-ttu-id="d7b34-229">Como alternativa, también puede devolver una promesa resuelta para finalizar el diálogo:</span><span class="sxs-lookup"><span data-stu-id="d7b34-229">Alternatively, you may also end the dialog by returning a resolved promise:</span></span>

```javascript
await Promise.resolve();
```

<span data-ttu-id="d7b34-230">La llamada a `Promise.resolve()` dará lugar a que finalice el dialogo y se saque de la pila.</span><span class="sxs-lookup"><span data-stu-id="d7b34-230">The call to `Promise.resolve()` will result in the dialog ending and popping off the stack.</span></span> <span data-ttu-id="d7b34-231">Sin embargo, este método no llama al diálogo principal para reanudar la ejecución.</span><span class="sxs-lookup"><span data-stu-id="d7b34-231">However, this method does not call the parent dialog to resume execution.</span></span> <span data-ttu-id="d7b34-232">Después de llamar a `Promise.resolve()`, la ejecución se detiene y el bot reanuda donde se deja el diálogo principal cuando el usuario envía al bot un mensaje.</span><span class="sxs-lookup"><span data-stu-id="d7b34-232">After the call to `Promise.resolve()`, execution stops, and the bot will resume where the parent dialog left off when the user sends the bot a message.</span></span> <span data-ttu-id="d7b34-233">Puede que esta no sea la experiencia idónea para finalizar un diálogo.</span><span class="sxs-lookup"><span data-stu-id="d7b34-233">This may not be the ideal user experience to end a dialog.</span></span> <span data-ttu-id="d7b34-234">Considere la posibilidad de finalizar un diálogo con `end()` o `replace()` para que el bot pueda continuar interactuando con el usuario.</span><span class="sxs-lookup"><span data-stu-id="d7b34-234">Consider ending a dialog with either `end()` or `replace()` so your bot can continue interacting with the user.</span></span>

### <a name="clear-the-dialog-stack"></a><span data-ttu-id="d7b34-235">Borrado de la pila de diálogos</span><span class="sxs-lookup"><span data-stu-id="d7b34-235">Clear the dialog stack</span></span>

<span data-ttu-id="d7b34-236">Si quiere sacar todos los diálogos de la pila, puede borrar la pila de diálogos mediante la llamada al método `dc.endAll()`.</span><span class="sxs-lookup"><span data-stu-id="d7b34-236">If you want to pop all dialogs off the stack, you can clear the dialog stack by calling the `dc.endAll()` method.</span></span>

```javascript
// Pop all dialogs from the current stack.
await dc.endAll();
```

### <a name="repeat-a-dialog"></a><span data-ttu-id="d7b34-237">Repetición de un diálogo</span><span class="sxs-lookup"><span data-stu-id="d7b34-237">Repeat a dialog</span></span>

<span data-ttu-id="d7b34-238">Para repetir un diálogo, use el método `dialogs.replace()`.</span><span class="sxs-lookup"><span data-stu-id="d7b34-238">To repeat a dialog, use the `dialogs.replace()` method.</span></span>

```javascript
// End the current dialog and start the 'mainMenu' dialog.
await dc.replace('mainMenu'); 
```

<span data-ttu-id="d7b34-239">Si quiere mostrar el menú principal de forma predeterminada, puede crear un diálogo `mainMenu` con los pasos siguientes:</span><span class="sxs-lookup"><span data-stu-id="d7b34-239">If you want to show the main menu by default, you can create a `mainMenu` dialog with the following steps:</span></span>

```javascript
// Display a menu and ask user to choose a menu item. Direct user to the item selected.
dialogs.add('mainMenu', [
    async function(dc){
        await dc.context.sendActivity("Welcome to Contoso Hotel and Resort.");
        await dc.prompt('choicePrompt', "How may we serve you today?", ['Order Dinner', 'Reserve a table']);
    },
    async function(dc, result){
        if(result.value.match(/order dinner/ig)){
            await dc.begin('orderDinner');
        }
        else if(result.value.match(/reserve a table/ig)){
            await dc.begin('reserveTable');
        }
        else {
            // Repeat the menu
            await dc.replace('mainMenu');
        }
    },
    async function(dc, result){
        // Start over
        await dc.endAll().begin('mainMenu');
    }
]);

dialogs.add('choicePrompt', new botbuilder_dialogs.ChoicePrompt());
```

<span data-ttu-id="d7b34-240">Este diálogo usa un elemento `ChoicePrompt` para mostrar el menú y espera a que el usuario elija una opción.</span><span class="sxs-lookup"><span data-stu-id="d7b34-240">This dialog uses a `ChoicePrompt` to display the menu and waits for the user to choose an option.</span></span> <span data-ttu-id="d7b34-241">Cuando el usuario elige `Order Dinner` o `Reserve a table`, se inicia el diálogo de la opción adecuada, y cuando se realiza esa tarea, este diálogo se repite, en lugar de simplemente finalizarlo en el último paso.</span><span class="sxs-lookup"><span data-stu-id="d7b34-241">When the user chooses either `Order Dinner` or `Reserve a table`, it starts the dialog for the appropriate choice and when that task is done, instead of just ending the dialog in the last step, this dialog repeats itself.</span></span>

### <a name="dialog-loops"></a><span data-ttu-id="d7b34-242">Bucles de diálogos</span><span class="sxs-lookup"><span data-stu-id="d7b34-242">Dialog loops</span></span>

<span data-ttu-id="d7b34-243">Otra forma de usar el método `replace()` es mediante la emulación de bucles.</span><span class="sxs-lookup"><span data-stu-id="d7b34-243">Another way to use the `replace()` method is by emulating loops.</span></span> <span data-ttu-id="d7b34-244">Tomemos como ejemplo este escenario.</span><span class="sxs-lookup"><span data-stu-id="d7b34-244">Take this scenario for example.</span></span> <span data-ttu-id="d7b34-245">Si quiere permitir que el usuario agregue varios elementos de menú a un carrito, puede crea un bucle con las opciones de menú hasta que el usuario finalice el pedido.</span><span class="sxs-lookup"><span data-stu-id="d7b34-245">If you want to allow the user to add multiple menu items to a cart, you can loop the menu choices until the user is done ordering.</span></span>

```javascript
// Order dinner:
// Help user order dinner from a menu

var dinnerMenu = {
    choices: ["Potato Salad - $5.99", "Tuna Sandwich - $6.89", "Clam Chowder - $4.50", 
        "More info", "Process order", "Cancel", "Help"],
    "Potato Salad - $5.99": {
        Description: "Potato Salad",
        Price: 5.99
    },
    "Tuna Sandwich - $6.89": {
        Description: "Tuna Sandwich",
        Price: 6.89
    },
    "Clam Chowder - $4.50": {
        Description: "Clam Chowder",
        Price: 4.50
    }

}

// The order cart
var orderCart = {
    orders: [],
    total: 0,
    clear: function(dc) {
        this.orders = [];
        this.total = 0;
        dc.context.activity.conversation.orderCart = null;
    }
};

dialogs.add('orderDinner', [
    async function (dc){
        await dc.context.sendActivity("Welcome to our Dinner order service.");
        orderCart.clear(dc); // Clears the cart.

        await dc.begin('orderPrompt'); // Prompt for orders
    },
    async function (dc, result) {
        if(result == "Cancel"){
            await dc.end();
        }
        else { 
            await dc.prompt('numberPrompt', "What is your room number?");
        }
    },
    async function(dc, result){
        await dc.context.sendActivity(`Thank you. Your order will be delivered to room ${result} within 45 minutes.`);
        await dc.end();
    }
]);

// Helper dialog to repeatedly prompt user for orders
dialogs.add('orderPrompt', [
    async function(dc){
        await dc.prompt('choicePrompt', "What would you like?", dinnerMenu.choices);
    },
    async function(dc, choice){
        if(choice.value.match(/process order/ig)){
            if(orderCart.orders.length > 0) {
                // Process the order
                // ...
                await dc.end();
            }
            else {
                await dc.context.sendActivity("Your cart was empty. Please add at least one item to the cart.");
                // Ask again
                await dc.replace('orderPrompt');
            }
        }
        else if(choice.value.match(/cancel/ig)){
            orderCart.clear(context);
            await dc.context.sendActivity("Your order has been canceled.");
            await dc.end(choice.value);
        }
        else if(choice.value.match(/more info/ig)){
            var msg = "More info: <br/>Potato Salad: contains 330 calaries per serving. <br/>"
                + "Tuna Sandwich: contains 700 calaries per serving. <br/>" 
                + "Clam Chowder: contains 650 calaries per serving."
            await dc.context.sendActivity(msg);
            
            // Ask again
            await dc.replace('orderPrompt');
        }
        else if(choice.value.match(/help/ig)){
            var msg = `Help: <br/>To make an order, add as many items to your cart as you like then choose the "Process order" option to check out.`
            await dc.context.sendActivity(msg);
            
            // Ask again
            await dc.replace('orderPrompt');
        }
        else {
            var choice = dinnerMenu[choice.value];

            // Only proceed if user chooses an item from the menu
            if(!choice){
                await dc.context.sendActivity("Sorry, that is not a valid item. Please pick one from the menu.");
                
                // Ask again
                await dc.replace('orderPrompt');
            }
            else {
                // Add the item to cart
                orderCart.orders.push(choice);
                orderCart.total += dinnerMenu[choice.value].Price;

                await dc.context.sendActivity(`Added to cart: ${choice.value}. <br/>Current total: $${orderCart.total}`);

                // Ask again
                await dc.replace('orderPrompt');
            }
        }
    }
]);

// Define prompts
// Generic prompts
dialogs.add('textPrompt', new botbuilder_dialogs.TextPrompt());
dialogs.add('numberPrompt', new botbuilder_dialogs.NumberPrompt());
dialogs.add('dateTimePrompt', new botbuilder_dialogs.DatetimePrompt());
dialogs.add('choicePrompt', new botbuilder_dialogs.ChoicePrompt());

```

<span data-ttu-id="d7b34-246">El código de ejemplo anterior muestra que el diálogo principal `orderDinner` usa un diálogo auxiliar llamado `orderPrompt` para controlar las opciones del usuario.</span><span class="sxs-lookup"><span data-stu-id="d7b34-246">The sample code above shows that the main `orderDinner` dialog uses a helper dialog named `orderPrompt` to handle user choices.</span></span> <span data-ttu-id="d7b34-247">El diálogo `orderPrompt` muestra el menú, pide al usuario que elija un artículo, agrega el artículo al carro y le pregunta de nuevo.</span><span class="sxs-lookup"><span data-stu-id="d7b34-247">The `orderPrompt` dialog displays the menu, asks the user to choose an item, add the item to cart and prompts again.</span></span> <span data-ttu-id="d7b34-248">Esto permite al usuario agregar varios artículos a su pedido.</span><span class="sxs-lookup"><span data-stu-id="d7b34-248">This allows the user to add multiple items to their order.</span></span> <span data-ttu-id="d7b34-249">El diálogo se repite hasta que el usuario elige `Process order` o `Cancel`.</span><span class="sxs-lookup"><span data-stu-id="d7b34-249">The dialog loops until the user chooses `Process order` or `Cancel`.</span></span> <span data-ttu-id="d7b34-250">En ese momento, la ejecución se transfiere al diálogo principal (p. ej.: `orderDinner`).</span><span class="sxs-lookup"><span data-stu-id="d7b34-250">At which point, execution is handed back to the parent dialog (e.g.: `orderDinner`).</span></span> <span data-ttu-id="d7b34-251">El diálogo `orderDinner` realiza algunas tareas de última hora si el usuario quiere procesar el pedido; en caso contrario, finaliza y devuelve la ejecución de nuevo a su diálogo principal (p. ej.: `mainMenu`).</span><span class="sxs-lookup"><span data-stu-id="d7b34-251">The `orderDinner` dialog does some last minute house keeping if the user wants to process the order; otherwise, it ends and returns execution back to its parent dialog (e.g.: `mainMenu`).</span></span> <span data-ttu-id="d7b34-252">El diálogo `mainMenu` a su vez continúa ejecutando el último paso que es simplemente volver a mostrar las opciones del menú principal.</span><span class="sxs-lookup"><span data-stu-id="d7b34-252">The `mainMenu` dialog in turn continues executing the last step which is to simply redisplay the main menu choices.</span></span>

---

## <a name="next-steps"></a><span data-ttu-id="d7b34-253">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="d7b34-253">Next steps</span></span>

<span data-ttu-id="d7b34-254">Ahora que sabe cómo usar **diálogos**, **mensajes** y **cascadas** para administrar el flujo de conversación, echemos un vistazo a cómo podemos dividir nuestros diálogos en tareas modulares en lugar de amontonarlos en el objeto `dialogs` de la lógica del bot.</span><span class="sxs-lookup"><span data-stu-id="d7b34-254">Now that you learn how to use **dialogs**, **prompts**, and **waterfalls** to manage conversation flow, let's take a look at how we can break our dialogs into modular tasks instead of lumping them all together in the main bot logic's `dialogs` object.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="d7b34-255">Creación de lógica de bot modular con control compuesto</span><span class="sxs-lookup"><span data-stu-id="d7b34-255">Create modular bot logic with Composite Control</span></span>](bot-builder-compositcontrol.md)