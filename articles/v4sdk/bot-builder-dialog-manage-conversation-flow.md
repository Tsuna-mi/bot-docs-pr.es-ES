---
title: Administración de un flujo de conversación simple con diálogos | Microsoft Docs
description: Aprenda a administrar un flujo de conversación simple con diálogos en Bot Builder SDK para Node.js.
keywords: flujo de conversación simple, diálogos, avisos, cascadas, conjunto de diálogos
author: v-ducvo
ms.author: v-ducvo
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 8/2/2018
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: 77162601f542e6faa8908bc71abc971eb99fcc93
ms.sourcegitcommit: 1abc32353c20acd103e0383121db21b705e5eec3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/21/2018
ms.locfileid: "42756769"
---
# <a name="manage-simple-conversation-flow-with-dialogs"></a><span data-ttu-id="73e77-104">Administración de un flujo de conversación simple con diálogos</span><span class="sxs-lookup"><span data-stu-id="73e77-104">Manage simple conversation flow with dialogs</span></span>

[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]

<span data-ttu-id="73e77-105">Puede administrar flujos de conversación simples y complejos mediante la biblioteca Dialogs.</span><span class="sxs-lookup"><span data-stu-id="73e77-105">You can manage both simple and complex conversation flows using the dialogs library.</span></span> <span data-ttu-id="73e77-106">En un flujo de conversación simple, el usuario comienza desde el primer paso de una *cascada*, continúa hasta el último paso y el intercambio de conversación finaliza.</span><span class="sxs-lookup"><span data-stu-id="73e77-106">In a simple conversation flow, the user starts from the first step of a *waterfall*, continues through to the last step, and the conversational exchange finishes.</span></span> <span data-ttu-id="73e77-107">Los diálogos también pueden controlar [flujos de conversación complejos](~/v4sdk/bot-builder-dialog-manage-complex-conversation-flow.md) en los que partes del diálogo se pueden bifurcar y repetir en bucle.</span><span class="sxs-lookup"><span data-stu-id="73e77-107">Dialogs can also handle [complex conversation flows](~/v4sdk/bot-builder-dialog-manage-complex-conversation-flow.md), where portions of the dialog can branch and loop.</span></span>

<!-- TODO: We need a dialogs conceptual topic to link to, so we can reference that here, in place of describing what they are and what their features are in a how-to topic. -->

<span data-ttu-id="73e77-108"><!-- TODO: This paragraph belongs in a conceptual topic. --> Un diálogo es similar a una función de un programa.</span><span class="sxs-lookup"><span data-stu-id="73e77-108"><!-- TODO: This paragraph belongs in a conceptual topic. --> A dialog is like a function in a program.</span></span> <span data-ttu-id="73e77-109">Por lo general está diseñado para realizar una operación específica en un orden específico y se puede invocar con la frecuencia que sea necesaria.</span><span class="sxs-lookup"><span data-stu-id="73e77-109">It is generally designed to perform a specific operation, in a specific order, and it can be invoked as often as it is needed.</span></span> <span data-ttu-id="73e77-110">El uso de diálogos permite al desarrollador de bots guiar el flujo de la conversación.</span><span class="sxs-lookup"><span data-stu-id="73e77-110">Using dialogs enables the bot developer to guide conversational flow.</span></span> <span data-ttu-id="73e77-111">Puede encadenar varios diálogos juntos para así controlar casi cualquier flujo de conversación que quiera que administre el bot.</span><span class="sxs-lookup"><span data-stu-id="73e77-111">You can chain multiple dialogs together to handle just about any conversation flow that you want your bot to handle.</span></span> <span data-ttu-id="73e77-112">La biblioteca **Dialogs** de Bot Builder SDK incluye características integradas como _avisos_ y _diálogos en cascada_ para ayudarle a administrar el flujo de la conversación.</span><span class="sxs-lookup"><span data-stu-id="73e77-112">The **Dialogs** library in the Bot Builder SDK includes built-in features such as _prompts_ and _waterfall dialogs_ to help you manage conversation flow.</span></span> <span data-ttu-id="73e77-113">Puede utilizar avisos para pedir a los usuarios distintos tipos de información.</span><span class="sxs-lookup"><span data-stu-id="73e77-113">You can use prompts to ask users for different types of information.</span></span> <span data-ttu-id="73e77-114">Puede usar una cascada para combinar varios pasos en una secuencia.</span><span class="sxs-lookup"><span data-stu-id="73e77-114">You can use a waterfall to combine multiple steps together in a sequence.</span></span>

<span data-ttu-id="73e77-115">En este artículo, usamos _conjuntos de diálogos_ para crear un flujo de conversación que contiene tanto pasos de aviso como de cascada.</span><span class="sxs-lookup"><span data-stu-id="73e77-115">In this article, we use _dialog sets_ to create a conversation flow that contains both prompts and waterfall steps.</span></span> <span data-ttu-id="73e77-116">Tenemos dos diálogos de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="73e77-116">We have two example dialogs.</span></span> <span data-ttu-id="73e77-117">El primero es un diálogo de un solo paso que realiza una operación que no requiere ninguna entrada del usuario.</span><span class="sxs-lookup"><span data-stu-id="73e77-117">The first is a one-step dialog that performs an operation that requires no user input.</span></span> <span data-ttu-id="73e77-118">El segundo es un diálogo de varios pasos que solicita al usuario cierta información.</span><span class="sxs-lookup"><span data-stu-id="73e77-118">The second is a multi-step dialog that prompts the user for some information.</span></span>

## <a name="install-the-dialogs-library"></a><span data-ttu-id="73e77-119">Instalación de la biblioteca de diálogos</span><span class="sxs-lookup"><span data-stu-id="73e77-119">Install the dialogs library</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="73e77-120">C#</span><span class="sxs-lookup"><span data-stu-id="73e77-120">C#</span></span>](#tab/csharp)

<span data-ttu-id="73e77-121">Comenzaremos a partir de una plantilla EchoBot básica.</span><span class="sxs-lookup"><span data-stu-id="73e77-121">We'll start from a basic EchoBot template.</span></span> <span data-ttu-id="73e77-122">Para obtener instrucciones, consulte la [guía de inicio rápido para .NET](~/dotnet/bot-builder-dotnet-quickstart.md).</span><span class="sxs-lookup"><span data-stu-id="73e77-122">For instructions, see the [quickstart for .NET](~/dotnet/bot-builder-dotnet-quickstart.md).</span></span>

<span data-ttu-id="73e77-123">Para usar diálogos, instale el paquete NuGet `Microsoft.Bot.Builder.Dialogs` para su proyecto o solución.</span><span class="sxs-lookup"><span data-stu-id="73e77-123">To use dialogs, install the `Microsoft.Bot.Builder.Dialogs` NuGet package for your project or solution.</span></span>
<span data-ttu-id="73e77-124">A continuación, haga referencia a la biblioteca Dialogs en las instrucciones using de los archivos de código según sea necesario.</span><span class="sxs-lookup"><span data-stu-id="73e77-124">Then reference the dialogs library in using statements in your code files as necessary.</span></span>

```csharp
using Microsoft.Bot.Builder.Dialogs;
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="73e77-125">JavaScript</span><span class="sxs-lookup"><span data-stu-id="73e77-125">JavaScript</span></span>](#tab/javascript)

<span data-ttu-id="73e77-126">La biblioteca `botbuilder-dialogs` se puede descargar desde NPM.</span><span class="sxs-lookup"><span data-stu-id="73e77-126">The `botbuilder-dialogs` library can be downloaded from NPM.</span></span> <span data-ttu-id="73e77-127">Para instalar la biblioteca `botbuilder-dialogs`, ejecute el siguiente comando de NPM:</span><span class="sxs-lookup"><span data-stu-id="73e77-127">To install the `botbuilder-dialogs` library, run the following NPM command:</span></span>

```cmd
npm install --save botbuilder-dialogs@preview
```

<span data-ttu-id="73e77-128">Para usar **diálogos** en el bot debe incluir esto en el archivo **app.js**.</span><span class="sxs-lookup"><span data-stu-id="73e77-128">To use **dialogs** in your bot, include this in your **app.js** file.</span></span>

```javascript
const botbuilder_dialogs = require('botbuilder-dialogs');
```

---

## <a name="create-a-dialog-stack"></a><span data-ttu-id="73e77-129">Creación de una pila de diálogos</span><span class="sxs-lookup"><span data-stu-id="73e77-129">Create a dialog stack</span></span>

<span data-ttu-id="73e77-130">En este primer ejemplo, vamos a crear un diálogo de un paso que puede sumar dos números y mostrar el resultado.</span><span class="sxs-lookup"><span data-stu-id="73e77-130">In this first example, we'll create a one-step dialog that can add two numbers together and display the result.</span></span>

<span data-ttu-id="73e77-131">Para usar diálogos, debe crear primero un *conjunto de diálogos*.</span><span class="sxs-lookup"><span data-stu-id="73e77-131">To use dialogs, you must first create a *dialog set*.</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="73e77-132">C#</span><span class="sxs-lookup"><span data-stu-id="73e77-132">C#</span></span>](#tab/csharp)

<span data-ttu-id="73e77-133">La biblioteca `Microsoft.Bot.Builder.Dialogs` proporciona una clase `DialogSet`.</span><span class="sxs-lookup"><span data-stu-id="73e77-133">The `Microsoft.Bot.Builder.Dialogs` library provides a `DialogSet` class.</span></span>
<span data-ttu-id="73e77-134">Cree una clase **AdditionDialog** y agregue las instrucciones using que necesitaremos.</span><span class="sxs-lookup"><span data-stu-id="73e77-134">Create an **AdditionDialog** class, and add the using statements we'll need.</span></span>
<span data-ttu-id="73e77-135">Puede agregar diálogos con nombre y conjuntos de diálogos a un conjunto de diálogos y luego acceder a ellos más tarde por su nombre.</span><span class="sxs-lookup"><span data-stu-id="73e77-135">You can add named dialogs and sets of dialogs to a dialog set, and then access them by name later.</span></span>

```csharp
using Microsoft.Bot.Builder.Dialogs;
```

<span data-ttu-id="73e77-136">Derive la clase de **DialogSet** y defina los identificadores y las claves que vamos a usar para identificar los diálogos y la información de entrada para este conjunto de diálogos.</span><span class="sxs-lookup"><span data-stu-id="73e77-136">Derive the class from **DialogSet**, and define the IDs and keys we'll use to identify the dialogs and input information for this dialog set.</span></span>

```csharp
/// <summary>Defines a simple dialog for adding two numbers together.</summary>
public class AdditionDialog : DialogSet
{
    /// <summary>The ID of the main dialog in the set.</summary>
    public const string Main = "additionDialog";

    /// <summary>Defines the IDs of the input arguments.</summary>
    public struct Inputs
    {
        public const string First = "first";
        public const string Second = "second";
    }
}
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="73e77-137">JavaScript</span><span class="sxs-lookup"><span data-stu-id="73e77-137">JavaScript</span></span>](#tab/javascript)

<span data-ttu-id="73e77-138">La biblioteca `botbuilder-dialogs` proporciona una clase `DialogSet`.</span><span class="sxs-lookup"><span data-stu-id="73e77-138">The `botbuilder-dialogs` library provides a `DialogSet` class.</span></span>
<span data-ttu-id="73e77-139">La clase **DialogSet** define una **pila de diálogos** y le ofrece una sencilla interfaz para administrar la pila.</span><span class="sxs-lookup"><span data-stu-id="73e77-139">The **DialogSet** class defines a **dialog stack** and gives you a simple interface to manage the stack.</span></span>
<span data-ttu-id="73e77-140">El SDK de Bot Builder implementa la pila como una matriz.</span><span class="sxs-lookup"><span data-stu-id="73e77-140">The Bot Builder SDK implements the stack as an array.</span></span>

<span data-ttu-id="73e77-141">Para crear un elemento **DialogSet**, realice lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="73e77-141">To create a **DialogSet**, do the following:</span></span>

```javascript
const dialogs = new botbuilder_dialogs.DialogSet();
```

<span data-ttu-id="73e77-142">La llamada anterior creará un elemento **DialogSet** con una **pila de diálogos** predeterminada llamada `dialogStack`.</span><span class="sxs-lookup"><span data-stu-id="73e77-142">The call above will create a **DialogSet** with a default **dialog stack** named `dialogStack`.</span></span>
<span data-ttu-id="73e77-143">Si quiere asignar un nombre a la pila, puede pasarla como un parámetro a **DialogSet()**.</span><span class="sxs-lookup"><span data-stu-id="73e77-143">If you want to name your stack, you can pass it in as a parameter to **DialogSet()**.</span></span> <span data-ttu-id="73e77-144">Por ejemplo: </span><span class="sxs-lookup"><span data-stu-id="73e77-144">For example:</span></span>

```javascript
const dialogs = new botbuilder_dialogs.DialogSet("myStack");
```

---

<span data-ttu-id="73e77-145">Al crear un diálogo solo se agrega la definición del diálogo al conjunto.</span><span class="sxs-lookup"><span data-stu-id="73e77-145">Creating a dialog only adds the dialog definition to the set.</span></span> <span data-ttu-id="73e77-146">El diálogo no se ejecuta hasta que se inserta en la pila de diálogos mediante la llamada a un método _begin_ o _replace_.</span><span class="sxs-lookup"><span data-stu-id="73e77-146">The dialog is not run until it is pushed onto the dialog stack by calling a _begin_ or _replace_ method.</span></span>

<span data-ttu-id="73e77-147">El nombre del diálogo (por ejemplo, `addTwoNumbers`) debe ser único dentro de cada conjunto de diálogos.</span><span class="sxs-lookup"><span data-stu-id="73e77-147">The dialog name (for example, `addTwoNumbers`) must be unique within each dialog set.</span></span> <span data-ttu-id="73e77-148">Puede definir tantos diálogos como sea necesario dentro de cada conjunto.</span><span class="sxs-lookup"><span data-stu-id="73e77-148">You can define as many dialogs as necessary within each set.</span></span> <span data-ttu-id="73e77-149">Si desea crear varios conjuntos de diálogos y hacer que funcionen juntos sin problemas, consulte [Creación de lógica de bot modular](bot-builder-compositcontrol.md).</span><span class="sxs-lookup"><span data-stu-id="73e77-149">If you want to create multiple dialog sets and have them work seemlessly together, see [Create modular bot logic](bot-builder-compositcontrol.md).</span></span>

<span data-ttu-id="73e77-150">La biblioteca de diálogos define los siguientes diálogos:</span><span class="sxs-lookup"><span data-stu-id="73e77-150">The dialog library defines the following dialogs:</span></span>

* <span data-ttu-id="73e77-151">Un diálogo de **mensaje** donde el diálogo usa al menos dos funciones, una para pedir al usuario que intervenga y la otra para procesar la entrada.</span><span class="sxs-lookup"><span data-stu-id="73e77-151">A **prompt** dialog where the dialog uses at least two functions, one to prompt the user for input and the other to process the input.</span></span> <span data-ttu-id="73e77-152">Los diálogos se pueden encadenar juntos con el modelo de **cascada**.</span><span class="sxs-lookup"><span data-stu-id="73e77-152">You can string these together using the **waterfall** model.</span></span>
* <span data-ttu-id="73e77-153">Un diálogo de **cascada** define una secuencia de _pasos de cascada_, que se ejecutan en orden.</span><span class="sxs-lookup"><span data-stu-id="73e77-153">A **waterfall** dialog defines a sequence of _waterfall steps_, which run in order.</span></span> <span data-ttu-id="73e77-154">Un diálogo de cascada puede tener un solo paso, en cuyo caso se puede considerar como un diálogo sencillo de un único paso.</span><span class="sxs-lookup"><span data-stu-id="73e77-154">A waterfall dialog can have a single step, in which case it can be thought of as a simple, one-step dialog.</span></span>

## <a name="create-a-single-step-dialog"></a><span data-ttu-id="73e77-155">Creación de un diálogo de un único paso</span><span class="sxs-lookup"><span data-stu-id="73e77-155">Create a single-step dialog</span></span>

<span data-ttu-id="73e77-156">Los diálogos de un único paso pueden ser útiles para capturar flujos conversacionales de un solo turno.</span><span class="sxs-lookup"><span data-stu-id="73e77-156">Single-step dialogs can be useful for capturing single-turn conversational flows.</span></span> <span data-ttu-id="73e77-157">En este ejemplo se crea un bot que puede detectar si el usuario dice algo parecido a "1 + 2" y se inicia un diálogo `addTwoNumbers` para responder con "1 + 2 = 3".</span><span class="sxs-lookup"><span data-stu-id="73e77-157">This example creates a bot that can detect if the user says something like "1 + 2", and starts an `addTwoNumbers` dialog to reply with "1 + 2 = 3".</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="73e77-158">C#</span><span class="sxs-lookup"><span data-stu-id="73e77-158">C#</span></span>](#tab/csharp)

<span data-ttu-id="73e77-159">Se pasan valores a los diálogos y se devuelven de estos como contenedores de propiedades `IDictionary<string,object>`.</span><span class="sxs-lookup"><span data-stu-id="73e77-159">Values are passed into and returned from dialogs as `IDictionary<string,object>` property bags.</span></span>

<span data-ttu-id="73e77-160">Para crear un diálogo sencillo dentro de un conjunto de diálogos, use el método `Add`.</span><span class="sxs-lookup"><span data-stu-id="73e77-160">To create a simple dialog within a dialog set, use the `Add` method.</span></span> <span data-ttu-id="73e77-161">El siguiente código agrega una cascada de un solo paso llamada `addTwoNumbers`.</span><span class="sxs-lookup"><span data-stu-id="73e77-161">The following adds a one-step waterfall named `addTwoNumbers`.</span></span>

<span data-ttu-id="73e77-162">En este paso se da por hecho que los argumentos de diálogo que se pasan contienen las propiedades `first` y `second` que representan los números que se van a agregar.</span><span class="sxs-lookup"><span data-stu-id="73e77-162">This step assumes that the dialog arguments getting passed in contain `first` and `second` properties that represent the numbers to be added.</span></span>

<span data-ttu-id="73e77-163">Agregue el siguiente constructor a la clase **AdditionDialog**.</span><span class="sxs-lookup"><span data-stu-id="73e77-163">Add the following constructor to the **AdditionDialog** class.</span></span>

```csharp
/// <summary>Defines the steps of the dialog.</summary>
public AdditionDialog()
{
    Add(Main, new WaterfallStep[]
    {
        async (dc, args, next) =>
        {
            // Get the input from the arguments to the dialog and add them.
            var x =(double)args[Inputs.First];
            var y =(double)args[Inputs.Second];
            var sum = x + y;

            // Display the result to the user.
            await dc.Context.SendActivity($"{x} + {y} = {sum}");

            // End the dialog.
            await dc.End();
        }
    });
}
```

### <a name="pass-arguments-to-the-dialog"></a><span data-ttu-id="73e77-164">Paso de argumentos al diálogo</span><span class="sxs-lookup"><span data-stu-id="73e77-164">Pass arguments to the dialog</span></span>

<span data-ttu-id="73e77-165">En el código del bot, actualice las instrucciones using.</span><span class="sxs-lookup"><span data-stu-id="73e77-165">In your bot code, update your using statements.</span></span>

```cs
using Microsoft.Bot;
using Microsoft.Bot.Builder;
using Microsoft.Bot.Builder.Core.Extensions;
using Microsoft.Bot.Schema;
using System.Collections.Generic;
using System.Text.RegularExpressions;
using System.Threading.Tasks;
```

<span data-ttu-id="73e77-166">Agregue una propiedad estática a la clase para el diálogo de adición.</span><span class="sxs-lookup"><span data-stu-id="73e77-166">Add a static property to the class for the addition dialog.</span></span>

```cs
private static AdditionDialog AddTwoNumbers { get; } = new AdditionDialog();
```

<span data-ttu-id="73e77-167">Para llamar al diálogo desde dentro del método `OnTurn` del bot, modifique `OnTurn` para que contenga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="73e77-167">To call the dialog from within your bot's `OnTurn` method, modify `OnTurn` to contain the following:</span></span>

```cs
public async Task OnTurn(ITurnContext context)
{
    // Handle any message activity from the user.
    if (context.Activity.Type is ActivityTypes.Message)
    {
        // Get the conversation state from the turn context.
        var conversationState = context.GetConversationState<ConversationData>();

        // Generate a dialog context for the addition dialog.
        var dc = AddTwoNumbers.CreateContext(context, conversationState.DialogState);

        // Call a helper function that identifies if the user says something
        // like "2 + 3" or "1.25 + 3.28" and extract the numbers to add.
        if (TryParseAddingTwoNumbers(context.Activity.Text, out double first, out double second))
        {
            // Start the dialog, passing in the numbers to add.
            var args = new Dictionary<string, object>
            {
                [AdditionDialog.Inputs.First] = first,
                [AdditionDialog.Inputs.Second] = second
            };
            await dc.Begin(AdditionDialog.Main, args);
        }
        else
        {
            // Echo back to the user whatever they typed.
            await context.SendActivity($"You said '{context.Activity.Text}'");
        }
    }
}
```

<span data-ttu-id="73e77-168">Agregue una función auxiliar **TryParseAddingTwoNumbers** a la clase del bot.</span><span class="sxs-lookup"><span data-stu-id="73e77-168">Add a **TryParseAddingTwoNumbers** helper function to the bot class.</span></span> <span data-ttu-id="73e77-169">La función auxiliar simplemente usa una expresión regular sencilla para detectar si el mensaje del usuario es una solicitud para sumar 2 números.</span><span class="sxs-lookup"><span data-stu-id="73e77-169">The helper function just uses a simple regex to detect if the user's message is a request to add 2 numbers.</span></span>

```cs
// Recognizes if the message is a request to add 2 numbers, in the form: number + number,
// where number may have optionally have a decimal point.: 1 + 1, 123.99 + 45, 0.4+7.
// For the sake of simplicity it doesn't handle negative numbers or numbers like 1,000 that contain a comma.
// If you need more robust number recognition, try System.Recognizers.Text
public static bool TryParseAddingTwoNumbers(string message, out double first, out double second)
{
    // captures a number with optional -/+ and optional decimal portion
    const string NUMBER_REGEXP = "([-+]?(?:[0-9]+(?:\\.[0-9]+)?|\\.[0-9]+))";

    // matches the plus sign with optional spaces before and after it
    const string PLUSSIGN_REGEXP = "(?:\\s*)\\+(?:\\s*)";

    const string ADD_TWO_NUMBERS_REGEXP = NUMBER_REGEXP + PLUSSIGN_REGEXP + NUMBER_REGEXP;

    var regex = new Regex(ADD_TWO_NUMBERS_REGEXP);
    var matches = regex.Matches(message);

    first = 0;
    second = 0;
    if (matches.Count > 0)
    {
        var matched = matches[0];
        if (double.TryParse(matched.Groups[1].Value, out first)
            && double.TryParse(matched.Groups[2].Value, out second))
        {
            return true;
        }
    }
    return false;
}
```

<span data-ttu-id="73e77-170">Si usa la plantilla EchoBot, cambie el nombre de la clase **EchoState** a **ConversationData** y modifíquelo para que contenga lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="73e77-170">If you're using the EchoBot template, change the name of the **EchoState** class to **ConversationData** and modify it to contain the following.</span></span>

```cs
using System.Collections.Generic;

/// <summary>
/// Class for storing conversation state.
/// </summary>
public class ConversationData
{
    /// <summary>Property for storing dialog state.</summary>
    public Dictionary<string, object> DialogState { get; set; } = new Dictionary<string, object>();
}
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="73e77-171">JavaScript</span><span class="sxs-lookup"><span data-stu-id="73e77-171">JavaScript</span></span>](#tab/javascript)

<span data-ttu-id="73e77-172">Comience con la plantilla JS descrita en [Create a bot with the Bot Builder SDK v4](../javascript/bot-builder-javascript-quickstart.md) (Creación de un bot con el SDK de Bot Builder v4).</span><span class="sxs-lookup"><span data-stu-id="73e77-172">Start with the JS template described in [Create a bot with the Bot Builder SDK v4](../javascript/bot-builder-javascript-quickstart.md).</span></span> <span data-ttu-id="73e77-173">En **app.js**, agregue una instrucción para exigir `botbuilder-dialogs`.</span><span class="sxs-lookup"><span data-stu-id="73e77-173">In **app.js**, add a statement to require `botbuilder-dialogs`.</span></span>

```js
const {DialogSet} = require('botbuilder-dialogs');
```

<span data-ttu-id="73e77-174">En **app.js**, agregue el código siguiente que define un diálogo sencillo llamado `addTwoNumbers` que pertenece al conjunto `dialogs`:</span><span class="sxs-lookup"><span data-stu-id="73e77-174">In **app.js**, add the following code that defines a simple dialog named `addTwoNumbers` that belongs to the `dialogs` set:</span></span>

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

<span data-ttu-id="73e77-175">Reemplace el código de **app.js** para procesar la actividad entrante por el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="73e77-175">Replace the code in **app.js** for processing the incoming activity with the following.</span></span> <span data-ttu-id="73e77-176">El bot llama a una función auxiliar para comprobar si el mensaje entrante se parece a una solicitud para sumar dos números.</span><span class="sxs-lookup"><span data-stu-id="73e77-176">The bot calls a helper function to check if the incoming message looks like a request for adding two numbers.</span></span> <span data-ttu-id="73e77-177">En caso afirmativo, pasa los números del argumento a `DialogContext.begin`.</span><span class="sxs-lookup"><span data-stu-id="73e77-177">If it does, it passes the numbers in the argument to `DialogContext.begin`.</span></span>

```js
// Listen for incoming activity 
server.post('/api/messages', (req, res) => {
    // Route received activity to adapter for processing
    adapter.processActivity(req, res, async (context) => {
        const isMessage = context.activity.type === 'message';
        // State will store all of your information
        const convoState = conversationState.get(context);
        const dc = dialogs.createContext(context, convoState);

        if (isMessage) {
            // TryParseAddingTwoNumbers checks if the message matches a regular expression
            // and if it does, returns an array of the numbers to add
            var numbers = await TryParseAddingTwoNumbers(context.activity.text); 
            if (numbers != null && numbers.length >=2 )
            {
                await dc.begin('addTwoNumbers', numbers);
            }
            else {
                // Just echo back the user's message if they're not adding numbers
                const count = (convoState.count === undefined ? convoState.count = 0 : ++convoState.count);
                return context.sendActivity(`Turn ${count}: You said "${context.activity.text}"`);
            }
        }
        else {
            return context.sendActivity(`[${context.activity.type} event detected]`);
        }

        if (!context.responded) {
            await dc.continue();
            // if the dialog didn't send a response
            if (!context.responded && isMessage) {
                await dc.context.sendActivity(`Hi! I'm the add 2 numbers bot. Say something like "What's 2+3?"`);
            }
        }
    });
});

```

<span data-ttu-id="73e77-178">Agregue la función auxiliar a **app.js**.</span><span class="sxs-lookup"><span data-stu-id="73e77-178">Add the helper function to **app.js**.</span></span> <span data-ttu-id="73e77-179">La función auxiliar simplemente usa una expresión regular sencilla para detectar si el mensaje del usuario es una solicitud para sumar 2 números.</span><span class="sxs-lookup"><span data-stu-id="73e77-179">The helper function just uses a simple regular expression to detect if the user's message is a request to add 2 numbers.</span></span> <span data-ttu-id="73e77-180">Si la expresión regular coincide, devuelve una matriz que contiene los números que se van a sumar.</span><span class="sxs-lookup"><span data-stu-id="73e77-180">If the regular expression matches, it returns an array that contains the numbers to add.</span></span>

```javascript
async function TryParseAddingTwoNumbers(message) {
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

### <a name="run-the-bot"></a><span data-ttu-id="73e77-181">Ejecución del bot</span><span class="sxs-lookup"><span data-stu-id="73e77-181">Run the bot</span></span>

<span data-ttu-id="73e77-182">Intente ejecutar el bot en Bot Framework Emulator y dígale algo como </span><span class="sxs-lookup"><span data-stu-id="73e77-182">Try running the bot in the Bot Framework Emulator, and say things like "what's 1+1?"</span></span> <span data-ttu-id="73e77-183">"¿Cuánto es 1 + 1?"</span><span class="sxs-lookup"><span data-stu-id="73e77-183">to it.</span></span>

![ejecutar el bot](./media/how-to-dialogs/bot-output-add-numbers.png)

## <a name="using-dialogs-to-guide-the-user-through-steps"></a><span data-ttu-id="73e77-185">Uso de diálogos para guiar al usuario paso a paso</span><span class="sxs-lookup"><span data-stu-id="73e77-185">Using dialogs to guide the user through steps</span></span>

<span data-ttu-id="73e77-186">En el ejemplo siguiente, creamos un diálogo de varios pasos para solicitar información al usuario.</span><span class="sxs-lookup"><span data-stu-id="73e77-186">In this next example, we create a multi-step dialog to prompt the user for information.</span></span>

### <a name="create-a-dialog-with-waterfall-steps"></a><span data-ttu-id="73e77-187">Creación de un diálogo con pasos de cascada</span><span class="sxs-lookup"><span data-stu-id="73e77-187">Create a dialog with waterfall steps</span></span>

<span data-ttu-id="73e77-188">Una **cascada** es una implementación específica de un diálogo que se usa normalmente para recopilar información del usuario o guiarlo en una serie de tareas.</span><span class="sxs-lookup"><span data-stu-id="73e77-188">A **waterfall** is a specific implementation of a dialog that is most commonly used to collect information from the user or guide the user through a series of tasks.</span></span> <span data-ttu-id="73e77-189">Las tareas se implementan como una matriz de funciones, donde el resultado de la primera función se pasa como argumentos a la función siguiente y así sucesivamente.</span><span class="sxs-lookup"><span data-stu-id="73e77-189">The tasks are implemented as an array of functions where the result of the first function is passed as arguments into the next function, and so on.</span></span> <span data-ttu-id="73e77-190">Cada función representa normalmente un paso del proceso general.</span><span class="sxs-lookup"><span data-stu-id="73e77-190">Each function typically represents one step in the overall process.</span></span> <span data-ttu-id="73e77-191">En cada paso, el bot [pide al usuario que intervenga](bot-builder-prompts.md), espera una respuesta y, a continuación, pasa el resultado al paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="73e77-191">At each step, the bot [prompts the user for input](bot-builder-prompts.md), waits for a response, and then passes the result to the next step.</span></span>

<span data-ttu-id="73e77-192">Por ejemplo, el ejemplo de código siguiente define tres funciones en una matriz que representan los tres pasos de una **cascada**.</span><span class="sxs-lookup"><span data-stu-id="73e77-192">For example, the following code sample defines three functions in an array that represents the three steps of a **waterfall**.</span></span> <span data-ttu-id="73e77-193">Después de cada aviso, el bot confirma la entrada del usuario, pero no guarda la entrada.</span><span class="sxs-lookup"><span data-stu-id="73e77-193">After each prompt, the bot acknowledges the user's input but did not save the input.</span></span> <span data-ttu-id="73e77-194">Si desea conservar las entradas del usuario, consulte [Conservación de datos de usuario](bot-builder-tutorial-persist-user-inputs.md) para más detalles.</span><span class="sxs-lookup"><span data-stu-id="73e77-194">If you want to persist user inputs, see [Persist user data](bot-builder-tutorial-persist-user-inputs.md) for more details.</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="73e77-195">C#</span><span class="sxs-lookup"><span data-stu-id="73e77-195">C#</span></span>](#tab/csharp)

<span data-ttu-id="73e77-196">Esto muestra un constructor para un diálogo de saludo, en el que **GreetingDialog** deriva de **DialogSet**, **Inputs.Text** contiene el identificador que vamos a usar para el objeto **TextPrompt** y **Main** contiene el identificador del propio diálogo de saludo.</span><span class="sxs-lookup"><span data-stu-id="73e77-196">This shows a constructor for a greeting dialog, where **GreetingDialog** derives from **DialogSet**, **Inputs.Text** contains the ID we're using for the **TextPrompt** object, and **Main** contains the ID for the greeting dialog itself.</span></span>

```csharp
public GreetingDialog()
{
    // Include a text prompt.
    Add(Inputs.Text, new TextPrompt());

    // Define the dialog logic for greeting the user.
    Add(Main, new WaterfallStep[]
    {
        async (dc, args, next) =>
        {
            // Ask for their name.
            await dc.Prompt(Inputs.Text, "What is your name?");
        },
        async (dc, args, next) =>
        {
            // Get the prompt result.
            var name = args["Text"] as string;

            // Acknowledge their input.
            await dc.Context.SendActivity($"Hi, {name}!");

            // Ask where they work.
            await dc.Prompt(Inputs.Text, "Where do you work?");
        },
        async (dc, args, next) =>
        {
            // Get the prompt result.
            var work = args["Text"] as string;

            // Acknowledge their input.
            await dc.Context.SendActivity($"{work} is a fun place.");

            // End the dialog.
            await dc.End();
        }
    });
}
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="73e77-197">JavaScript</span><span class="sxs-lookup"><span data-stu-id="73e77-197">JavaScript</span></span>](#tab/javascript)

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

---

<span data-ttu-id="73e77-198">La signatura del paso de una **cascada** es de la manera siguiente:</span><span class="sxs-lookup"><span data-stu-id="73e77-198">The signature for a **waterfall** step is as follows:</span></span>

| <span data-ttu-id="73e77-199">Parámetro</span><span class="sxs-lookup"><span data-stu-id="73e77-199">Parameter</span></span> | <span data-ttu-id="73e77-200">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="73e77-200">Description</span></span> |
| :---- | :----- |
| `dc` | <span data-ttu-id="73e77-201">El contexto del diálogo.</span><span class="sxs-lookup"><span data-stu-id="73e77-201">The dialog context.</span></span> |
| `args` | <span data-ttu-id="73e77-202">Opcional, contiene los argumentos pasados en el paso.</span><span class="sxs-lookup"><span data-stu-id="73e77-202">Optional, contains the arguments passed into the step.</span></span> |
| `next` | <span data-ttu-id="73e77-203">Opcional, un método que le permite continuar con el paso siguiente de la cascada sin pedir confirmación.</span><span class="sxs-lookup"><span data-stu-id="73e77-203">Optional, a method that allows you to proceed to the next step of the waterfall without prompting.</span></span> <span data-ttu-id="73e77-204">Puede proporcionar un argumento *args* cuando llame a este método, lo que le permite pasar los argumentos al siguiente paso de la cascada.</span><span class="sxs-lookup"><span data-stu-id="73e77-204">You can provide an *args* argument when you call this method, This allows you to pass arguments to the next step in the waterfall.</span></span> |

<span data-ttu-id="73e77-205">Cada paso debe llamar a uno de los métodos siguientes antes de volver: el delegado *next()* o uno de los métodos del contexto de diálogo, *begin*, *end*, *prompt* o *replace*; de lo contrario, el bot se detendrá en ese paso.</span><span class="sxs-lookup"><span data-stu-id="73e77-205">Each step must call one of the following methods before returning: the *next()* delegate or one of the dialog context methods *begin*, *end*, *prompt*, or *replace*; otherwise, the bot will be stuck in that step.</span></span> <span data-ttu-id="73e77-206">Es decir, si una función no finaliza con uno de estos métodos, todas las intervenciones de los usuarios harán que se vuelva a ejecutar este paso cada vez que el usuario envía al bot un mensaje.</span><span class="sxs-lookup"><span data-stu-id="73e77-206">That is, if a function does not finish with one of these methods then all user input will cause this step to be re-executed each time the user sends the bot a message.</span></span>

<span data-ttu-id="73e77-207">Cuando se alcanza el final de la cascada, es recomendable volver con el método _end_ para que el diálogo se pueda sacar de la pila.</span><span class="sxs-lookup"><span data-stu-id="73e77-207">When you reached the end of the waterfall, it is best practice to return with the _end_ method so that the dialog can be popped off the stack.</span></span> <span data-ttu-id="73e77-208">Consulte la sección [Finalización de un diálogo](#end-a-dialog) para más información.</span><span class="sxs-lookup"><span data-stu-id="73e77-208">See [End a dialog](#end-a-dialog) section below for more information.</span></span> <span data-ttu-id="73e77-209">Igualmente, para continuar desde un paso al siguiente, el paso de cascada debe finalizar con un aviso o llamar explícitamente al delegado _next_ para avanzar por la cascada.</span><span class="sxs-lookup"><span data-stu-id="73e77-209">Likewise, to proceed from one step to the next, the waterfall step must end with either a prompt or explicitly call the _next_ delegate to advance the waterfall.</span></span>

## <a name="start-a-dialog"></a><span data-ttu-id="73e77-210">Inicio de un diálogo</span><span class="sxs-lookup"><span data-stu-id="73e77-210">Start a dialog</span></span>

<span data-ttu-id="73e77-211">Para iniciar un diálogo, pase el valor de *dialogId* que desea iniciar en el método _begin_, _prompt_ o _replace_ del contexto de diálogo.</span><span class="sxs-lookup"><span data-stu-id="73e77-211">To start a dialog, pass the *dialogId* you want to start into the dialog context's _begin_, _prompt_, or _replace_ method.</span></span> <span data-ttu-id="73e77-212">El método _begin_ insertará el diálogo al principio de la pila, mientras que el método _replace_ sacará el diálogo actual de la pila e insertará el diálogo sustituto en ella.</span><span class="sxs-lookup"><span data-stu-id="73e77-212">The _begin_ method will push the dialog onto the top of the stack, while the _replace_ method will pop the current dialog off the stack and push the replacing dialog onto the stack.</span></span>

<span data-ttu-id="73e77-213">Para iniciar un diálogo sin argumentos:</span><span class="sxs-lookup"><span data-stu-id="73e77-213">To start a dialog without arguments:</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="73e77-214">C#</span><span class="sxs-lookup"><span data-stu-id="73e77-214">C#</span></span>](#tab/csharp)

```csharp
// Start the greetings dialog.
await dc.Begin("greetings");
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="73e77-215">JavaScript</span><span class="sxs-lookup"><span data-stu-id="73e77-215">JavaScript</span></span>](#tab/javascript)

```javascript
// Start the 'greetings' dialog.
await dc.begin('greetings');
```

---

<span data-ttu-id="73e77-216">Para iniciar un diálogo con argumentos:</span><span class="sxs-lookup"><span data-stu-id="73e77-216">To start a dialog with arguments:</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="73e77-217">C#</span><span class="sxs-lookup"><span data-stu-id="73e77-217">C#</span></span>](#tab/csharp)

```csharp
// Start the greetings dialog, passing in a property bag.
await dc.Begin("greetings", args);
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="73e77-218">JavaScript</span><span class="sxs-lookup"><span data-stu-id="73e77-218">JavaScript</span></span>](#tab/javascript)

```javascript
// Start the 'greetings' dialog with the 'userName' passed in.
await dc.begin('greetings', userName);
```

---

<span data-ttu-id="73e77-219">Para iniciar un diálogo de **mensajes**:</span><span class="sxs-lookup"><span data-stu-id="73e77-219">To start a **prompt** dialog:</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="73e77-220">C#</span><span class="sxs-lookup"><span data-stu-id="73e77-220">C#</span></span>](#tab/csharp)

<span data-ttu-id="73e77-221">En este caso, **Inputs.Text** contiene el identificador de un **TextPrompt**, que está en el mismo conjunto de diálogos.</span><span class="sxs-lookup"><span data-stu-id="73e77-221">Here, **Inputs.Text** contains the ID of a **TextPrompt** that is in the same dialog set.</span></span>

```csharp
// Ask a user for their name.
await dc.Prompt(Inputs.Text, "What is your name?");
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="73e77-222">JavaScript</span><span class="sxs-lookup"><span data-stu-id="73e77-222">JavaScript</span></span>](#tab/javascript)

```javascript
// Ask a user for their name.
await dc.prompt('textPrompt', "What is your name?");
```

---

<span data-ttu-id="73e77-223">Según el tipo de símbolo de mensaje que inicie, la signatura del argumento del mensaje puede ser diferente.</span><span class="sxs-lookup"><span data-stu-id="73e77-223">Depending on the type of prompt you are starting, the prompt's argument signature may be different.</span></span> <span data-ttu-id="73e77-224">El método **DialogSet.prompt** es un método auxiliar.</span><span class="sxs-lookup"><span data-stu-id="73e77-224">The **DialogSet.prompt** method is a helper method.</span></span> <span data-ttu-id="73e77-225">Este método toma argumentos y construye las opciones adecuadas para el mensaje; a continuación, llama al método **begin** para iniciar el diálogo de mensajes.</span><span class="sxs-lookup"><span data-stu-id="73e77-225">This method takes in arguments and constructs the appropriate options for the prompt; then, it calls the **begin** method to start the prompt dialog.</span></span> <span data-ttu-id="73e77-226">Para más información sobre los avisos, consulte [Petición de datos de entrada al usuario](bot-builder-prompts.md).</span><span class="sxs-lookup"><span data-stu-id="73e77-226">For more information on prompts, see [Prompt user for input](bot-builder-prompts.md).</span></span>

## <a name="end-a-dialog"></a><span data-ttu-id="73e77-227">Finalización de un diálogo</span><span class="sxs-lookup"><span data-stu-id="73e77-227">End a dialog</span></span>

<span data-ttu-id="73e77-228">Para finalizar un diálogo, el método _end_ lo retira de la pila y devuelve un resultado opcional al diálogo primario.</span><span class="sxs-lookup"><span data-stu-id="73e77-228">The _end_ method ends a dialog by popping it off the stack and returns an optional result to the parent dialog.</span></span>

<span data-ttu-id="73e77-229">Es recomendable llamar explícitamente al método _end_ al final del diálogo; sin embargo, no es obligatorio porque el diálogo se saca automáticamente de la pila cuando se llega al final de la cascada.</span><span class="sxs-lookup"><span data-stu-id="73e77-229">It is best practice to explicitly call the _end_ method at the end of the dialog; however, it is not required because the dialog will automatically be popped off the stack for you when you reach the end of the waterfall.</span></span>

<span data-ttu-id="73e77-230">Para finalizar un diálogo:</span><span class="sxs-lookup"><span data-stu-id="73e77-230">To end a dialog:</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="73e77-231">C#</span><span class="sxs-lookup"><span data-stu-id="73e77-231">C#</span></span>](#tab/csharp)

```csharp
// End the current dialog by popping it off the stack.
await dc.End();
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="73e77-232">JavaScript</span><span class="sxs-lookup"><span data-stu-id="73e77-232">JavaScript</span></span>](#tab/javascript)

```javascript
// End the current dialog by popping it off the stack
await dc.end();
```

---

<span data-ttu-id="73e77-233">Para finalizar un diálogo y devolver información al diálogo primario, incluya un argumento de bolsa de propiedades.</span><span class="sxs-lookup"><span data-stu-id="73e77-233">To end a dialog and return information to the parent dialog, include a property bag argument.</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="73e77-234">C#</span><span class="sxs-lookup"><span data-stu-id="73e77-234">C#</span></span>](#tab/csharp)

```csharp
// End the current dialog and return information to the parent dialog.
await dc.end(new Dictionary<string, object>
    {
        ["property1"] = value1,
        ["property2"] = value2
    });
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="73e77-235">JavaScript</span><span class="sxs-lookup"><span data-stu-id="73e77-235">JavaScript</span></span>](#tab/javascript)

```javascript
// End the current dialog and pass a result to the parent dialog
await dc.end({
    "property1": value1,
    "property2": value2
});
```

---

## <a name="clear-the-dialog-stack"></a><span data-ttu-id="73e77-236">Borrado de la pila de diálogos</span><span class="sxs-lookup"><span data-stu-id="73e77-236">Clear the dialog stack</span></span>

<span data-ttu-id="73e77-237">Si quiere sacar todos los diálogos de la pila, llame al método _end all_ para borrar la pila de diálogos.</span><span class="sxs-lookup"><span data-stu-id="73e77-237">If you want to pop all dialogs off the stack, you can clear the dialog stack by calling the _end all_ method.</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="73e77-238">C#</span><span class="sxs-lookup"><span data-stu-id="73e77-238">C#</span></span>](#tab/csharp)

```csharp
// Pop all dialogs from the current stack.
await dc.EndAll();
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="73e77-239">JavaScript</span><span class="sxs-lookup"><span data-stu-id="73e77-239">JavaScript</span></span>](#tab/javascript)

```javascript
// Pop all dialogs from the current stack.
await dc.endAll();
```

---

## <a name="repeat-a-dialog"></a><span data-ttu-id="73e77-240">Repetición de un diálogo</span><span class="sxs-lookup"><span data-stu-id="73e77-240">Repeat a dialog</span></span>

<span data-ttu-id="73e77-241">Para repetir un diálogo, use el método _replace_.</span><span class="sxs-lookup"><span data-stu-id="73e77-241">To repeat a dialog, use the _replace_ method.</span></span> <span data-ttu-id="73e77-242">El método *replace* del contexto de diálogo retira el diálogo actual de la pila, inserta el diálogo sustituto en la parte superior de la pila y comienza dicho diálogo.</span><span class="sxs-lookup"><span data-stu-id="73e77-242">The dialog context's *replace* method will pop the current dialog off the stack and push the replacing dialog onto the top of the stack and begin that dialog.</span></span> <span data-ttu-id="73e77-243">Es una excelente manera de controlar [flujos de conversación complejos](~/v4sdk/bot-builder-dialog-manage-complex-conversation-flow.md) y una buena técnica para administrar menús.</span><span class="sxs-lookup"><span data-stu-id="73e77-243">This is a great way to handle [complex conversation flows](~/v4sdk/bot-builder-dialog-manage-complex-conversation-flow.md) and a good technique to manage menus.</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="73e77-244">C#</span><span class="sxs-lookup"><span data-stu-id="73e77-244">C#</span></span>](#tab/csharp)

```csharp
// End the current dialog and start the main menu dialog.
await dc.Replace("mainMenu");
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="73e77-245">JavaScript</span><span class="sxs-lookup"><span data-stu-id="73e77-245">JavaScript</span></span>](#tab/javascript)

```javascript
// End the current dialog and start the 'mainMenu' dialog.
await dc.replace('mainMenu');
```

---

## <a name="next-steps"></a><span data-ttu-id="73e77-246">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="73e77-246">Next steps</span></span>

<span data-ttu-id="73e77-247">Ahora que ha aprendido a administrar flujos de conversación simples, consulte cómo se puede aprovechar el método _replace_ para controlar flujos de conversación complejos.</span><span class="sxs-lookup"><span data-stu-id="73e77-247">Now that you've learned how to manage simple conversation flows, lets take a look at how you can leverage the _replace_ method to handle complex conversation flows.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="73e77-248">Administración de flujos de conversación complejos</span><span class="sxs-lookup"><span data-stu-id="73e77-248">Manage complex conversation flow</span></span>](bot-builder-dialog-manage-complex-conversation-flow.md)
