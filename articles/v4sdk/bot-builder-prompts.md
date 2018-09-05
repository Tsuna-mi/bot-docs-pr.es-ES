---
title: Petición de datos de entrada a los usuarios con la biblioteca Dialogs | Microsoft Docs
description: Obtenga información sobre cómo solicitar datos de entrada a los usuarios en Bot Builder SDK para Node.js.
keywords: mensajes, cuadros de diálogo, AttachmentPrompt, ChoicePrompt, ConfirmPrompt, DatetimePrompt, NumberPrompt, TextPrompt, volver a solicitar, validación
author: v-ducvo
ms.author: v-ducvo
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 4/10/2018
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: 0b238ed510fd1d6fda82734af373f344b0dc28e3
ms.sourcegitcommit: 2dc75701b169d822c9499e393439161bc87639d2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42905369"
---
# <a name="prompt-users-for-input-using-the-dialogs-library"></a><span data-ttu-id="0d875-104">Petición de datos de entrada a los usuarios con la biblioteca Dialogs</span><span class="sxs-lookup"><span data-stu-id="0d875-104">Prompt users for input using the Dialogs library</span></span>

[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]

<span data-ttu-id="0d875-105">A menudo los bots recopilan su información a través de preguntas que se realizan a los usuarios.</span><span class="sxs-lookup"><span data-stu-id="0d875-105">Often bots gather their information through questions posed to the user.</span></span> <span data-ttu-id="0d875-106">Puede enviar un mensaje estándar al usuario mediante el método _send activity_ del objeto del [contexto de turno](bot-builder-concept-activity-processing.md#turn-context) para pedir una cadena de entrada; sin embargo, Bot Builder SDK ofrece la biblioteca **Dialogs** que se puede usar para solicitar otros tipos de información.</span><span class="sxs-lookup"><span data-stu-id="0d875-106">You can simply send the user a standard message by using the [turn context](bot-builder-concept-activity-processing.md#turn-context) object's _send activity_ method to ask for a string input; however, the Bot Builder SDK provides a **dialogs** library that you can use to ask for different types for information.</span></span> <span data-ttu-id="0d875-107">En este tema se detalla cómo usar **preguntas** para pedir entradas al usuario.</span><span class="sxs-lookup"><span data-stu-id="0d875-107">This topic details how to use **prompts** to ask a user for input.</span></span>

<span data-ttu-id="0d875-108">En este artículo se describe cómo usar las preguntas dentro de un cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="0d875-108">This article describes how to use prompts within a dialog.</span></span> <span data-ttu-id="0d875-109">Para obtener información sobre el uso de los diálogos en general, consulte [Uso de diálogos para administrar un flujo de conversación simple](bot-builder-dialog-manage-conversation-flow.md).</span><span class="sxs-lookup"><span data-stu-id="0d875-109">For information on using dialogs in general, see [using dialogs to manage simple conversation flow](bot-builder-dialog-manage-conversation-flow.md).</span></span>

## <a name="prompt-types"></a><span data-ttu-id="0d875-110">Tipos de avisos</span><span class="sxs-lookup"><span data-stu-id="0d875-110">Prompt types</span></span>

<span data-ttu-id="0d875-111">La biblioteca de cuadros de diálogo ofrece una serie de tipos de preguntas, cada una para solicitar un tipo de respuesta diferente.</span><span class="sxs-lookup"><span data-stu-id="0d875-111">The dialogs library offers a number of different types of prompts, each requesting a different type of response.</span></span>

| <span data-ttu-id="0d875-112">Prompt</span><span class="sxs-lookup"><span data-stu-id="0d875-112">Prompt</span></span> | <span data-ttu-id="0d875-113">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="0d875-113">Description</span></span> |
|:----|:----|
| <span data-ttu-id="0d875-114">**AttachmentPrompt**</span><span class="sxs-lookup"><span data-stu-id="0d875-114">**AttachmentPrompt**</span></span> | <span data-ttu-id="0d875-115">Solicita al usuario un archivo adjunto como un documento o una imagen.</span><span class="sxs-lookup"><span data-stu-id="0d875-115">Prompt the user for an attachment such as a document or image.</span></span> |
| <span data-ttu-id="0d875-116">**ChoicePrompt**</span><span class="sxs-lookup"><span data-stu-id="0d875-116">**ChoicePrompt**</span></span> | <span data-ttu-id="0d875-117">Solicita al usuario que elija entre un conjunto de opciones.</span><span class="sxs-lookup"><span data-stu-id="0d875-117">Prompt the user to choose from a set of options.</span></span> |
| <span data-ttu-id="0d875-118">**ConfirmPrompt**</span><span class="sxs-lookup"><span data-stu-id="0d875-118">**ConfirmPrompt**</span></span> | <span data-ttu-id="0d875-119">Solicita al usuario que confirme su acción.</span><span class="sxs-lookup"><span data-stu-id="0d875-119">Prompt the user to confirm their action.</span></span> |
| <span data-ttu-id="0d875-120">**DatetimePrompt**</span><span class="sxs-lookup"><span data-stu-id="0d875-120">**DatetimePrompt**</span></span> | <span data-ttu-id="0d875-121">Solicita al usuario una fecha y una hora.</span><span class="sxs-lookup"><span data-stu-id="0d875-121">Prompt the user for a date-time.</span></span> <span data-ttu-id="0d875-122">Los usuarios puedan responder con lenguaje natural, como "Mañana a las 8 p.m." o "Viernes a las 10 a.m.".</span><span class="sxs-lookup"><span data-stu-id="0d875-122">Users can respond using natural language such as "Tomorrow at 8pm" or "Friday at 10am".</span></span> <span data-ttu-id="0d875-123">El SDK de Bot Framework usa la entidad precompilada `builtin.datetimeV2` de LUIS.</span><span class="sxs-lookup"><span data-stu-id="0d875-123">The Bot Framework SDK uses the LUIS `builtin.datetimeV2` prebuilt entity.</span></span> <span data-ttu-id="0d875-124">Para obtener más información, vea [builtin.datetimev2](https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-prebuilt-entities#builtindatetimev2).</span><span class="sxs-lookup"><span data-stu-id="0d875-124">For more information, see [builtin.datetimev2](https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-prebuilt-entities#builtindatetimev2).</span></span> |
| <span data-ttu-id="0d875-125">**NumberPrompt**</span><span class="sxs-lookup"><span data-stu-id="0d875-125">**NumberPrompt**</span></span> | <span data-ttu-id="0d875-126">Solicita al usuario un número.</span><span class="sxs-lookup"><span data-stu-id="0d875-126">Prompt the user for a number.</span></span> <span data-ttu-id="0d875-127">El usuario puede responder con "10" o "diez".</span><span class="sxs-lookup"><span data-stu-id="0d875-127">The user can respond with either "10" or "ten".</span></span> <span data-ttu-id="0d875-128">Si la respuesta es "diez", por ejemplo, la pregunta convertirá la respuesta en un número y devolverá `10` como resultado.</span><span class="sxs-lookup"><span data-stu-id="0d875-128">If the response is "ten", for example, the prompt will convert the response into a number and return `10` as a result.</span></span> |
| <span data-ttu-id="0d875-129">**TextPrompt**</span><span class="sxs-lookup"><span data-stu-id="0d875-129">**TextPrompt**</span></span> | <span data-ttu-id="0d875-130">Solicita al usuario una cadena de texto.</span><span class="sxs-lookup"><span data-stu-id="0d875-130">Prompt user for a string of text.</span></span> |

## <a name="add-references-to-prompt-library"></a><span data-ttu-id="0d875-131">Agregar referencias a la biblioteca de preguntas</span><span class="sxs-lookup"><span data-stu-id="0d875-131">Add references to prompt library</span></span>

<span data-ttu-id="0d875-132">Puede obtener la biblioteca **dialogs** si agrega el paquete **dialogs** al bot.</span><span class="sxs-lookup"><span data-stu-id="0d875-132">You can get the **dialogs** library by adding the **dialogs** package to your bot.</span></span> <span data-ttu-id="0d875-133">Trataremos los diálogos en [Uso de diálogos para administrar un flujo de conversación simple](bot-builder-dialog-manage-conversation-flow.md), pero vamos a usar diálogos en nuestros avisos.</span><span class="sxs-lookup"><span data-stu-id="0d875-133">We cover dialogs in [using dialogs to manage simple conversation flow](bot-builder-dialog-manage-conversation-flow.md), but we'll use dialogs for our prompts.</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="0d875-134">C#</span><span class="sxs-lookup"><span data-stu-id="0d875-134">C#</span></span>](#tab/csharp)

<span data-ttu-id="0d875-135">Instale el paquete **Microsoft.Bot.Builder.Dialogs** de NuGet.</span><span class="sxs-lookup"><span data-stu-id="0d875-135">Install the **Microsoft.Bot.Builder.Dialogs** package from NuGet.</span></span>

<span data-ttu-id="0d875-136">Después, incluya la referencia a la biblioteca desde el código del bot.</span><span class="sxs-lookup"><span data-stu-id="0d875-136">Then, include reference the library from your bot code.</span></span>

```cs
using Microsoft.Bot.Builder.Dialogs;
```

<span data-ttu-id="0d875-137">Puede definir un cuadro de diálogo como una clase o una propiedad insertada en el archivo de código del bot.</span><span class="sxs-lookup"><span data-stu-id="0d875-137">You can define a dialog as a class or in-line as a property in your bot code file.</span></span>

<span data-ttu-id="0d875-138">El código de este artículo está escrito para un cuadro de diálogo que se define como una clase.</span><span class="sxs-lookup"><span data-stu-id="0d875-138">The code in this article is written for a dialog defined as a class.</span></span>
<span data-ttu-id="0d875-139">En los ejemplos siguientes se supone que va a agregar código al constructor del cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="0d875-139">The following examples assume you are adding code to the dialog's constructor.</span></span>

<span data-ttu-id="0d875-140">El flujo principal del cuadro de diálogo es su colección de pasos y se le debe proporcionar un identificador.</span><span class="sxs-lookup"><span data-stu-id="0d875-140">The main flow of your dialog is its collection of steps and needs to be given an ID.</span></span> <span data-ttu-id="0d875-141">El bot usa este identificador para recuperar el cuadro de diálogo, por lo que es una buena práctica exponerlo como una constante.</span><span class="sxs-lookup"><span data-stu-id="0d875-141">Your bot uses this ID to retrieve the dialog, so it is a good practice to expose this as a constant.</span></span>

```cs
public class MyDialog : DialogSet
{
    public const string Name = "mainDialog";

    public MyDialog()
    {
        // Define your dialog's prompts and steps here.
    }
}
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="0d875-142">JavaScript</span><span class="sxs-lookup"><span data-stu-id="0d875-142">JavaScript</span></span>](#tab/javascript)

<span data-ttu-id="0d875-143">Instale el paquete dialogs desde NPM:</span><span class="sxs-lookup"><span data-stu-id="0d875-143">Install the dialogs package from NPM:</span></span>

```cmd
npm install --save botbuilder-dialogs@preview
```

<span data-ttu-id="0d875-144">Para usar **dialogs** en el bot, inclúyalo en el código del bot.</span><span class="sxs-lookup"><span data-stu-id="0d875-144">To use **dialogs** in your bot, include it in the bot code.</span></span>

<span data-ttu-id="0d875-145">En el archivo app.js, agregue lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="0d875-145">In the app.js file, add the following.</span></span>

```javascript
const {DialogSet} = require("botbuilder-dialogs");
const dialogs = new DialogSet();
```

---

## <a name="prompt-the-user"></a><span data-ttu-id="0d875-146">Preguntar al usuario</span><span class="sxs-lookup"><span data-stu-id="0d875-146">Prompt the user</span></span>

<span data-ttu-id="0d875-147">Para solicitar entradas al usuario, puede agregar una pregunta al cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="0d875-147">To prompt a user for input, you can add a prompt to your dialog.</span></span> <span data-ttu-id="0d875-148">Por ejemplo, puede definir una pregunta de tipo **TextPrompt** y asignarle un identificador de cuadro de diálogo de **textPrompt**:</span><span class="sxs-lookup"><span data-stu-id="0d875-148">For example, you can define a prompt of type **TextPrompt** and give it a dialog ID of **textPrompt**:</span></span>

<span data-ttu-id="0d875-149">Después de agregar un cuadro de diálogo de pregunta, se puede usar en un cuadro de diálogo de cascada de dos pasos simple o usar varias preguntas juntas en una cascada de varios pasos.</span><span class="sxs-lookup"><span data-stu-id="0d875-149">Once a prompt dialog is added, you can use it in a simple two step waterfall dialog or use multiple prompts together in a multi-step waterfall.</span></span> <span data-ttu-id="0d875-150">Un cuadro de diálogo de *cascada* es simplemente una manera de definir una secuencia de pasos.</span><span class="sxs-lookup"><span data-stu-id="0d875-150">A *waterfall* dialog is simply a way to define a sequence of steps.</span></span> <span data-ttu-id="0d875-151">Para más información, consulte la sección [Uso de diálogos](bot-builder-dialog-manage-conversation-flow.md#using-dialogs-to-guide-the-user-through-steps) en [Uso de diálogos para administrar un flujo de conversación simple](bot-builder-dialog-manage-conversation-flow.md).</span><span class="sxs-lookup"><span data-stu-id="0d875-151">For more information, see the [using dialogs](bot-builder-dialog-manage-conversation-flow.md#using-dialogs-to-guide-the-user-through-steps) section of [manage simple conversation flow with dialogs](bot-builder-dialog-manage-conversation-flow.md).</span></span>

<span data-ttu-id="0d875-152">En el primer turno, el cuadro de diálogo solicita el nombre al usuario y, en el segundo, procesa la entrada del usuario como respuesta a la solicitud.</span><span class="sxs-lookup"><span data-stu-id="0d875-152">On the first turn, the dialog prompts the user for their name, and on the second turn, the dialog processes the user input as an answer to the prompt.</span></span>

<span data-ttu-id="0d875-153">Por ejemplo, en el cuadro de diálogo siguiente se le pide el nombre al usuario y, después, se le saluda por su nombre:</span><span class="sxs-lookup"><span data-stu-id="0d875-153">For example, the following dialog prompts the user for their name and then greets them by name:</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="0d875-154">C#</span><span class="sxs-lookup"><span data-stu-id="0d875-154">C#</span></span>](#tab/csharp)

<span data-ttu-id="0d875-155">Cada pregunta que se usa en el cuadro de diálogo también tiene un nombre, que el cuadro de diálogo usa en el bot para acceder a la pregunta.</span><span class="sxs-lookup"><span data-stu-id="0d875-155">Each prompt you use in your dialog is also given a name, used by the dialog or your bot to access the prompt.</span></span> <span data-ttu-id="0d875-156">En todos estos ejemplos, los identificadores de pregunta se exponen como constantes.</span><span class="sxs-lookup"><span data-stu-id="0d875-156">In all of these samples, we are exposing the prompt IDs as constants.</span></span>

<span data-ttu-id="0d875-157">Una llamada al método **Prompt** o **End** del contexto del cuadro de diálogo señala el final del paso al cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="0d875-157">A call to the dialog context's **Prompt** or **End** method signals the end of the step to the dialog.</span></span> <span data-ttu-id="0d875-158">Sin estas instrucciones, el cuadro de diálogo no se ejecutará correctamente.</span><span class="sxs-lookup"><span data-stu-id="0d875-158">Without these statements, the dialog will not run properly.</span></span>

```csharp
/// <summary>Defines a simple greeting dialog that asks for the user's name.</summary>
public class MyDialog : DialogSet
{
    /// <summary>The ID of the main dialog in the set.</summary>
    public const string Name = "mainDialog";

    /// <summary>Defines the IDs of the prompts in the set.</summary>
    public struct Inputs
    {
        /// <summary>The ID of the text prompt.</summary>
        public const string Text = "textPrompt";
    }

    /// <summary>Defines the prompts and steps of the dialog.</summary>
    public MyDialog()
    {
        Add(Inputs.Text, new TextPrompt());
        Add(Name, new WaterfallStep[]
        {
            // Each step takes in a dialog context, arguments, and the next delegate.
            async (dc, args, next) =>
            {
                // Prompt for the user's name.
                await dc.Prompt(Inputs.Text, "What is your name?");
            },
            async(dc, args, next) =>
            {
                var user = (string)args["Text"];
                await dc.Context.SendActivity($"Hi {user}!");
                await dc.End();
            }
        });
    }
}
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="0d875-159">JavaScript</span><span class="sxs-lookup"><span data-stu-id="0d875-159">JavaScript</span></span>](#tab/javascript)

```javascript
const {TextPrompt} = require("botbuilder-dialogs");
```

```javascript
// Greet user:
// Ask for the user name and then greet them by name.
dialogs.add('textPrompt', new TextPrompt());
dialogs.add('greetings', [
    async function (dc){
        await dc.prompt('textPrompt', 'What is your name?');
    },
    async function(dc, userName){
        await dc.context.sendActivity(`Hi ${userName}!`);
        await dc.end();
    }
]);
```

---

> [!NOTE]
> <span data-ttu-id="0d875-160">Para iniciar un cuadro de diálogo, obtenga un contexto de cuadro de diálogo y use su método _begin_.</span><span class="sxs-lookup"><span data-stu-id="0d875-160">To start a dialog, get a dialog context, and use its _begin_ method.</span></span> <span data-ttu-id="0d875-161">Para más información, consulte [Uso de diálogos para administrar un flujo de conversación simple](./bot-builder-dialog-manage-conversation-flow.md).</span><span class="sxs-lookup"><span data-stu-id="0d875-161">For more information, see [use dialogs to manage simple conversation flow](./bot-builder-dialog-manage-conversation-flow.md).</span></span>

## <a name="reusable-prompts"></a><span data-ttu-id="0d875-162">Preguntas reutilizables</span><span class="sxs-lookup"><span data-stu-id="0d875-162">Reusable prompts</span></span>

<span data-ttu-id="0d875-163">Una pregunta se puede reutilizar para solicitar otra información con el mismo tipo de pregunta.</span><span class="sxs-lookup"><span data-stu-id="0d875-163">A prompt can be reused to ask for different information using the same type of prompt.</span></span> <span data-ttu-id="0d875-164">Por ejemplo, en el código de ejemplo anterior se definía una pregunta de texto y se usaba para preguntarle el nombre al usuario.</span><span class="sxs-lookup"><span data-stu-id="0d875-164">For example, the sample code above defined a text prompt and used it to ask the user for their name.</span></span> <span data-ttu-id="0d875-165">Si quisiera, por ejemplo, también podría usar esa misma pregunta para pedir al usuario otra cadena de texto, como "Where do you work?" (¿Dónde trabajas?).</span><span class="sxs-lookup"><span data-stu-id="0d875-165">If you wanted to, for example, you can also use that same prompt to ask the user for another text string; such as, "Where do you work?".</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="0d875-166">C#</span><span class="sxs-lookup"><span data-stu-id="0d875-166">C#</span></span>](#tab/csharp)

```cs
/// <summary>Defines a simple greeting dialog that asks for the user's name and place of work.</summary>
public class MyDialog : DialogSet
{
    /// <summary>The ID of the main dialog in the set.</summary>
    public const string Name = "mainDialog";

    /// <summary>Defines the IDs of the prompts in the set.</summary>
    public struct Inputs
    {
        /// <summary>The ID of the text prompt.</summary>
        public const string Text = "textPrompt";
    }

    /// <summary>Defines the prompts and steps of the dialog.</summary>
    public MyDialog()
    {
        Add(Inputs.Text, new TextPrompt());
        Add(Name, new WaterfallStep[]
        {
            async (dc, args, next) =>
            {
                // Prompt for the user's name.
                await dc.Prompt(Inputs.Text, "What is your name?");
            },
            async(dc, args, next) =>
            {
                var user = (string)args["Text"];

                // Ask them where they work.
                await dc.Prompt(Inputs.Text, $"Hi {user}! Where do you work?");
            },
            async(dc, args, next) =>
            {
                var workplace = (string)args["Text"];

                await dc.Context.SendActivity($"{workplace} is a cool place!");
                await dc.End();
            }
        });
    }
}
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="0d875-167">JavaScript</span><span class="sxs-lookup"><span data-stu-id="0d875-167">JavaScript</span></span>](#tab/javascript)

```javascript
// Greet user:
// Ask for the user name and then greet them by name.
// Ask them where they work.
dialogs.add('textPrompt', new botbuilder_dialogs.TextPrompt());
dialogs.add('greetings',[
    async function (dc){
        await dc.prompt('textPrompt', 'What is your name?');
    },
    async function(dc, userName){
        await dc.context.sendActivity(`Hi ${userName}!`);

        // Ask them where they work.
        await dc.prompt('textPrompt', 'Where do you work?');
    },
    async function(dc, workPlace){
        await dc.context.sendActivity(`${workPlace} is a cool place!`);

        await dc.end();
    }
]);
```

---

<span data-ttu-id="0d875-168">Pero si quiere emparejar la pregunta con el valor esperado que se solicita, puede asignar a cada pregunta un valor *dialogId* único.</span><span class="sxs-lookup"><span data-stu-id="0d875-168">However, if you wish to pair the prompt to the expected value that prompt is asking, you could give each prompt a unique *dialogId*.</span></span> <span data-ttu-id="0d875-169">Un cuadro de diálogo se agrega con un identificador único.</span><span class="sxs-lookup"><span data-stu-id="0d875-169">A dialog is added with a unique ID.</span></span> <span data-ttu-id="0d875-170">Al usar otros identificadores, también se pueden crear varios cuadros de diálogo **prompt** del mismo tipo.</span><span class="sxs-lookup"><span data-stu-id="0d875-170">Using different IDs, you can also create multiple **prompt** dialogs of the same type.</span></span> <span data-ttu-id="0d875-171">Por ejemplo, se podrían crear dos cuadros de diálogo **TextPrompt** para el ejemplo anterior:</span><span class="sxs-lookup"><span data-stu-id="0d875-171">For example, you could create two **TextPrompt** dialogs for the example above:</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="0d875-172">C#</span><span class="sxs-lookup"><span data-stu-id="0d875-172">C#</span></span>](#tab/csharp)

```cs
/// <summary>The ID of the main dialog in the set.</summary>
public const string Name = "mainDialog";

/// <summary>Defines the IDs of the prompts in the set.</summary>
public struct Inputs
{
    /// <summary>The ID of the name prompt.</summary>
    public const string Name = "namePrompt";

    /// <summary>The ID of the work prompt.</summary>
    public const string Work = "workPrompt";
}

/// <summary>Defines the prompts and steps of the dialog.</summary>
public MyDialog()
{
    Add(Inputs.Name, new TextPrompt());
    Add(Inputs.Work, new TextPrompt());
    Add(Name, new WaterfallStep[]
    {
        async (dc, args, next) =>
        {
            // Prompt for the user's name.
            await dc.Prompt(Inputs.Name, "What is your name?");
        },
        async(dc, args, next) =>
        {
            var user = (string)args["Text"];

            // Ask them where they work.
            await dc.Prompt(Inputs.Work, $"Hi {user}! Where do you work?");
        },
        async(dc, args, next) =>
        {
            var workplace = (string)args["Text"];

            await dc.Context.SendActivity($"{workplace} is a cool place!");
            await dc.End();
        }
    });
}
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="0d875-173">JavaScript</span><span class="sxs-lookup"><span data-stu-id="0d875-173">JavaScript</span></span>](#tab/javascript)

```javascript
dialogs.add('namePrompt', new TextPrompt());
dialogs.add('workPlacePrompt', new TextPrompt());
```

---

<span data-ttu-id="0d875-174">Para la reutilización del código, la definición de un solo `textPrompt` funcionaría para todas estas preguntas porque preguntan por una cadena de texto como respuesta.</span><span class="sxs-lookup"><span data-stu-id="0d875-174">For the sake of code reusability, defining a single `textPrompt` would work for all these prompts because they ask for a text string as a response.</span></span> <span data-ttu-id="0d875-175">Pero la capacidad de asignar nombres a los cuadros de diálogo resulta útil cuando es necesario validar la entrada de la pregunta.</span><span class="sxs-lookup"><span data-stu-id="0d875-175">However, where the ability to name dialogs comes in handy is when you need to validate the input of the prompt.</span></span> <span data-ttu-id="0d875-176">En este caso, es posible que las preguntas usen **TextPrompt** pero que cada una busque un conjunto de valores diferente.</span><span class="sxs-lookup"><span data-stu-id="0d875-176">In which case, the prompts may be using **TextPrompt** but each is looking for a different set of values.</span></span> <span data-ttu-id="0d875-177">Veamos cómo se pueden validar las respuestas de las preguntas mediante un `NumberPrompt`.</span><span class="sxs-lookup"><span data-stu-id="0d875-177">Lets take a look at how you can validate prompt responses using a `NumberPrompt`.</span></span>

## <a name="specify-prompt-options"></a><span data-ttu-id="0d875-178">Especificar las opciones de las preguntas</span><span class="sxs-lookup"><span data-stu-id="0d875-178">Specify prompt options</span></span>

<span data-ttu-id="0d875-179">Cuando se usa una pregunta en un paso de cuadro de diálogo, también se pueden proporcionar opciones de pregunta, como una cadena de nueva pregunta.</span><span class="sxs-lookup"><span data-stu-id="0d875-179">When you use a prompt within a dialog step, you can also provide prompt options, such as a reprompt string.</span></span>

<span data-ttu-id="0d875-180">Especificar una cadena de nueva pregunta es útil cuando la entrada del usuario no puede satisfacer una pregunta, ya sea porque está en un formato que no se puede analizar, como "mañana" para una pregunta de símbolo, o bien porque la entrada no cumpla un criterio de validación.</span><span class="sxs-lookup"><span data-stu-id="0d875-180">Specifying a reprompt string is useful when user input can fail to satisfy a prompt, either because it is in a format that the prompt can not parse, such as "tomorrow" for a number prompt, or the input fails a validation criteria.</span></span>

> [!TIP]
> <span data-ttu-id="0d875-181">Cuando se crea una pregunta de número, es necesario especificar la referencia cultural de entrada que va a usar.</span><span class="sxs-lookup"><span data-stu-id="0d875-181">When you create a number prompt, you need to specify the input culture it will use.</span></span> <span data-ttu-id="0d875-182">La pregunta de número puede interpretar una amplia variedad de entradas, "doce" o "un cuarto", así como "12" y "0,25".</span><span class="sxs-lookup"><span data-stu-id="0d875-182">The number prompt can interpret a wide variety of input, such as "twelve" or "one quarter", as well as "12" and "0.25".</span></span> <span data-ttu-id="0d875-183">La referencia cultural de la entrada ayuda a que la pregunta interprete la entrada del usuario más correctamente.</span><span class="sxs-lookup"><span data-stu-id="0d875-183">The input culture helps the prompt more correctly interpret the user's input.</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="0d875-184">C#</span><span class="sxs-lookup"><span data-stu-id="0d875-184">C#</span></span>](#tab/csharp)

<span data-ttu-id="0d875-185">Las referencias culturales de las entradas se definen en una biblioteca adicional.</span><span class="sxs-lookup"><span data-stu-id="0d875-185">Input cultures are defined in an additional library.</span></span>

```csharp
using Microsoft.Bot.Builder.Dialogs;
using Microsoft.Recognizers.Text;
```

<span data-ttu-id="0d875-186">En el código siguiente se agregaría una pregunta de número a un conjunto de cuadros de diálogo existente, **dialogs**.</span><span class="sxs-lookup"><span data-stu-id="0d875-186">The following code would add a number prompt to an existing dialog set, **dialogs**.</span></span>

```csharp
dialogs.Add("numberPrompt", new NumberPrompt<int>(Culture.English));
```

<span data-ttu-id="0d875-187">En un paso de cuadro de diálogo, el código siguiente podría solicitar entradas al usuario y proporcionar una cadena de nueva pregunta para usarla en caso de que la entrada no se pudiera interpretar como un número.</span><span class="sxs-lookup"><span data-stu-id="0d875-187">Within a dialog step, the following code would prompt the user for input and provide a reprompt string to use if their input cannot be interpreted as a number.</span></span>

```csharp
await dc.Prompt("numberPrompt", "How many people are in your party?", new PromptOptions()
{
    RetryPromptString = "Sorry, please specify the number of people in your party."
});
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="0d875-188">JavaScript</span><span class="sxs-lookup"><span data-stu-id="0d875-188">JavaScript</span></span>](#tab/javascript)

```javascript
const {NumberPrompt} = require("botbuilder-dialogs");
```

```javascript
await dc.prompt('numberPrompt', 'How many people in your party?', { retryPrompt: `Sorry, please specify the number of people in your party.` })
```

```javascript
dialogs.add('numberPrompt', new NumberPrompt());
```

---

<span data-ttu-id="0d875-189">En concreto, la pregunta de opciones requiere información adicional, la lista de opciones disponibles para el usuario.</span><span class="sxs-lookup"><span data-stu-id="0d875-189">In particular, the choice prompt requires some additional information, the list of choices available to the user.</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="0d875-190">C#</span><span class="sxs-lookup"><span data-stu-id="0d875-190">C#</span></span>](#tab/csharp)

<span data-ttu-id="0d875-191">En este ejemplo se usan tipos de los espacios de nombres siguientes.</span><span class="sxs-lookup"><span data-stu-id="0d875-191">This example uses types from the following namespaces.</span></span>

```csharp
using Microsoft.Bot.Builder.Core.Extensions;
using Microsoft.Bot.Builder.Dialogs;
using Microsoft.Bot.Builder.Prompts.Choices;
using Microsoft.Bot.Schema;
using Microsoft.Recognizers.Text;
using System.Collections.Generic;
```


<span data-ttu-id="0d875-192">Cuando se usa **ChoicePrompt** para pedir al usuario que elija entre un conjunto de opciones, se debe proporcionar a la pregunta ese conjunto de opciones, dentro de un objeto **ChoicePromptOptions**.</span><span class="sxs-lookup"><span data-stu-id="0d875-192">When we use the **ChoicePrompt** to ask the user to choose between a set of options, we have to provide the prompt with that set of options, provided within a **ChoicePromptOptions** object.</span></span> <span data-ttu-id="0d875-193">En este caso, se usa **ChoiceFactory** para convertir una lista de opciones en un formato adecuado.</span><span class="sxs-lookup"><span data-stu-id="0d875-193">Here, we use the **ChoiceFactory** to convert a list of options into an appropriate format.</span></span>

<span data-ttu-id="0d875-194">También se usa una actividad **SuggestedActions** como nueva pregunta, como una manera de volver a proporcionar las opciones de entrada al usuario.</span><span class="sxs-lookup"><span data-stu-id="0d875-194">We are also using a **SuggestedActions** activity as the reprompt, as a way of providing the input options to the user again.</span></span>


```csharp
/// <summary>Defines a dialog that asks for a choice of color.</summary>
public class MyDialog : DialogSet
{
    /// <summary>The ID of the main dialog in the set.</summary>
    public const string Name = "mainDialog";

    /// <summary>Defines the IDs of the prompts in the set.</summary>
    public struct Inputs
    {
        /// <summary>The ID of the color prompt.</summary>
        public const string Color = "colorPrompt";
    }

    /// <summary>The available colors to choose from.</summary>
    public List<string> Colors = new List<string> { "Green", "Blue" };

    /// <summary>Defines the prompts and steps of the dialog.</summary>
    public MyDialog()
    {
        Add(Inputs.Color, new ChoicePrompt(Culture.English));
        Add(Name, new WaterfallStep[]
        {
            async (dc, args, next) =>
            {
                // Prompt for a color. A choice prompt requires that you specify choice options.
                await dc.Prompt(Inputs.Color, "Please make a choice.", new ChoicePromptOptions()
                {
                    Choices = ChoiceFactory.ToChoices(Colors),
                    RetryPromptActivity =
                        MessageFactory.SuggestedActions(Colors, "Please choose a color.") as Activity
                });
            },
            async(dc, args, next) =>
            {
                var color = (FoundChoice)args["Value"];

                await dc.Context.SendActivity($"You chose {color.Value}.");
                await dc.End();
            }
        });
    }
}
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="0d875-195">JavaScript</span><span class="sxs-lookup"><span data-stu-id="0d875-195">JavaScript</span></span>](#tab/javascript)

```javascript
const {ChoicePrompt} = require("botbuilder-dialogs");
```

```javascript
dialogs.add('choicePrompt', new ChoicePrompt());
```

```javascript
// A choice prompt requires that you specify choice options.
const list = ['green', 'blue'];
await dc.prompt('choicePrompt', 'Please make a choice', list, {retryPrompt: 'Please choose a color.'});
```

---

## <a name="validate-a-prompt-response"></a><span data-ttu-id="0d875-196">Validar la respuesta de una pregunta</span><span class="sxs-lookup"><span data-stu-id="0d875-196">Validate a prompt response</span></span>

<span data-ttu-id="0d875-197">Puede validar la respuesta de una pregunta antes de devolver el valor válido al paso siguiente de la **cascada**.</span><span class="sxs-lookup"><span data-stu-id="0d875-197">You can validate a prompt response before returning the valid value to the next step of the **waterfall**.</span></span> <span data-ttu-id="0d875-198">Por ejemplo, para validar un **NumberPrompt** dentro de un intervalo de números entre **6** y **20**, puede tener lógica de validación similar a la siguiente:</span><span class="sxs-lookup"><span data-stu-id="0d875-198">For example, to validate a **NumberPrompt** within a range of numbers between **6** and **20**, you can have validation logic similar to this:</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="0d875-199">C#</span><span class="sxs-lookup"><span data-stu-id="0d875-199">C#</span></span>](#tab/csharp)

```cs
using Microsoft.Bot.Builder.Dialogs;
using Microsoft.Recognizers.Text;
using PromptStatus = Microsoft.Bot.Builder.Prompts.PromptStatus;
```

```cs
/// <summary>Defines a dialog that asks for the number of people in a party.</summary>
public class MyDialog : DialogSet
{
    /// <summary>The ID of the main dialog in the set.</summary>
    public const string Name = "mainDialog";

    /// <summary>Defines the IDs of the prompts in the set.</summary>
    public struct Inputs
    {
        /// <summary>The ID of the party size prompt.</summary>
        public const string Size = "parytySize";
    }

    /// <summary>Defines the prompts and steps of the dialog.</summary>
    public MyDialog()
    {
        // Include a validation function for the party size prompt.
        Add(Inputs.Size, new NumberPrompt<int>(Culture.English, async (context, result) =>
        {
            if (result.Value < 6 || result.Value > 20)
            {
                result.Status = PromptStatus.OutOfRange;
            }
        }));
        Add(Name, new WaterfallStep[]
        {
            async (dc, args, next) =>
            {
                // Prompt for the party size.
                await dc.Prompt(Inputs.Size, "How many people are in your party?", new PromptOptions()
                {
                    RetryPromptString = "Please specify party size between 6 and 20."
                });
            },
            async(dc, args, next) =>
            {
                var size = (int)args["Value"];

                await dc.Context.SendActivity($"Okay, {size} people!");
                await dc.End();
            }
        });
    }
}
```

<span data-ttu-id="0d875-200">La validación también se puede encapsular dentro de su propio método privado, y agregarse de esa forma.</span><span class="sxs-lookup"><span data-stu-id="0d875-200">Validation can also be encapsulated within it's own private method, and added that way.</span></span>

```cs
/// <summary>Validates input for the partySize prompt.</summary>
/// <param name="context">The context object for the current turn of the bot.</param>
/// <param name="result">The recognition result from the prompt.</param>
/// <returns>An updated recognition result.</returns>
private static async Task PartySizeValidator(ITurnContext context, Int32Result result)
{
    if (result.Value < 6 || result.Value > 20)
    {
        result.Status = PromptStatus.OutOfRange;
    }
}
```

<span data-ttu-id="0d875-201">En el cuadro de diálogo, especifique el método que se va a usar para validar la entrada.</span><span class="sxs-lookup"><span data-stu-id="0d875-201">Within your dialog, specify the method to use to validate input.</span></span>

```cs
// Include a validation function for the party size prompt.
Add(Inputs.Size, new NumberPrompt<int>(Culture.English, PartySizeValidator));
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="0d875-202">JavaScript</span><span class="sxs-lookup"><span data-stu-id="0d875-202">JavaScript</span></span>](#tab/javascript)

```javascript
// Customized prompts with validations
// A number prompt with validation for valid party size within a range.
dialogs.add('partySizePrompt', new botbuilder_dialogs.NumberPrompt( async (context, value) => {
    try {
        if(value < 6 ){
            throw new Error('Party size too small.');
        }
        else if(value > 20){
            throw new Error('Party size too big.')
        }
        else {
            return value; // Return the valid value
        }
    } catch (err) {
        await context.sendActivity(`${err.message} <br/>Please provide a valid number between 6 and 20.`);
        return undefined;
    }
}));
```

---

<span data-ttu-id="0d875-203">Del mismo modo, si quiere validar una respuesta **DatetimePrompt** para una fecha y hora en el futuro, puede tener lógica de validación similar a la siguiente:</span><span class="sxs-lookup"><span data-stu-id="0d875-203">Likewise, if you want to validate a **DatetimePrompt** response for a date and time in the future, you can have validation logic similar to this:</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="0d875-204">C#</span><span class="sxs-lookup"><span data-stu-id="0d875-204">C#</span></span>](#tab/csharp)

```cs
using Microsoft.Bot.Builder;
using Microsoft.Bot.Builder.Dialogs;
using Microsoft.Recognizers.Text;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using DateTimeResult = Microsoft.Bot.Builder.Prompts.DateTimeResult;
using PromptStatus = Microsoft.Bot.Builder.Prompts.PromptStatus;
```

```cs
/// <summary>Validates input for the reservationTime prompt.</summary>
/// <param name="context">The context object for the current turn of the bot.</param>
/// <param name="result">The recognition result from the prompt.</param>
/// <returns>An updated recognition result.</returns>
private static async Task TimeValidator(ITurnContext context, DateTimeResult result)
{
    if (result.Resolution.Count == 0)
    {
        await context.SendActivity("Sorry, I did not recognize the time that you entered.");
        result.Status = PromptStatus.NotRecognized;
    }

    // Find any recognized time that is not in the past.
    var now = DateTime.Now;
    DateTime time = default(DateTime);
    var resolution = result.Resolution.FirstOrDefault(
        res => DateTime.TryParse(res.Value, out time) && time > now);

    if (resolution != null)
    {
        // If found, keep only that result.
        result.Resolution.Clear();
        result.Resolution.Add(resolution);
    }
    else
    {
        // Otherwise, flag the input as out of range.
        await context.SendActivity("Please enter a time in the future, such as \"tomorrow at 9am\"");
        result.Status = PromptStatus.OutOfRange;
    }
}
```

```csharp
Add(Inputs.Time, new DateTimePrompt(Culture.English, TimeValidator));
```

<span data-ttu-id="0d875-205">Encontrará más ejemplos en el [repositorio de ejemplos](https://github.com/Microsoft/botbuilder-dotnet).</span><span class="sxs-lookup"><span data-stu-id="0d875-205">Further examples can be found in our [samples repo](https://github.com/Microsoft/botbuilder-dotnet).</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="0d875-206">JavaScript</span><span class="sxs-lookup"><span data-stu-id="0d875-206">JavaScript</span></span>](#tab/javascript)

```JavaScript
// A date and time prompt with validation for date/time in the future.
dialogs.add('dateTimePrompt', new botbuilder_dialogs.DatetimePrompt( async (context, values) => {
    try {
        if (values.length < 0) { throw new Error('missing time') }
        if (values[0].type !== 'datetime') { throw new Error('unsupported type') }
        const value = new Date(values[0].value);
        if (value.getTime() < new Date().getTime()) { throw new Error('in the past') }
        return value;
    } catch (err) {
        await context.sendActivity(`Please enter a valid time in the future like "tomorrow at 9am".`);
        return undefined;
    }
}));
```

<span data-ttu-id="0d875-207">Encontrará más ejemplos en el [repositorio de ejemplos](https://github.com/Microsoft/botbuilder-js).</span><span class="sxs-lookup"><span data-stu-id="0d875-207">Further examples can be found in our [samples repo](https://github.com/Microsoft/botbuilder-js).</span></span>

---

> [!TIP]
> <span data-ttu-id="0d875-208">Las preguntas de fecha y hora se pueden resolver en varias fechas diferentes si el usuario proporciona una respuesta ambigua.</span><span class="sxs-lookup"><span data-stu-id="0d875-208">Date time prompts can resolve to a few different dates if the user gives an ambigious answer.</span></span> <span data-ttu-id="0d875-209">En función del uso previsto, puede que le interese comprobar todas las resoluciones proporcionadas por el resultado de la pregunta, en lugar de simplemente la primera.</span><span class="sxs-lookup"><span data-stu-id="0d875-209">Depending on what you're using it for, you may want to check all the resolutions provided by the prompt result, instead of just the first.</span></span>

<span data-ttu-id="0d875-210">Se pueden usar técnicas similares para validar las respuestas para cualquiera de los tipos de pregunta.</span><span class="sxs-lookup"><span data-stu-id="0d875-210">You can use the similar techniques to validate prompt responses for any of the prompt types.</span></span>

## <a name="save-user-data"></a><span data-ttu-id="0d875-211">Guardar los datos del usuario</span><span class="sxs-lookup"><span data-stu-id="0d875-211">Save user data</span></span>

<span data-ttu-id="0d875-212">Al solicitar entradas al usuario, tiene varias opciones sobre cómo controlarlas.</span><span class="sxs-lookup"><span data-stu-id="0d875-212">When you prompt for user input, you have several options on how to handle that input.</span></span> <span data-ttu-id="0d875-213">Por ejemplo, puede consumir y descartar la entrada, guardarla en una variable global, guardarla en un contenedor de almacenamiento volátil o en memoria, guardarla en un archivo, o bien en una base de datos externa.</span><span class="sxs-lookup"><span data-stu-id="0d875-213">For instance, you can consume and discard the input, you can save it to a global variable, you can save it to a volatile or in-memory storage container, you can save it to a file, or you can save it to an external database.</span></span> <span data-ttu-id="0d875-214">Para obtener más información sobre cómo guardar los datos del usuario, vea [Administración de los datos del usuario](bot-builder-howto-v4-state.md).</span><span class="sxs-lookup"><span data-stu-id="0d875-214">For more information on how to save user data, see [Manage user data](bot-builder-howto-v4-state.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="0d875-215">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="0d875-215">Next steps</span></span>

<span data-ttu-id="0d875-216">Ahora que sabe cómo solicitar entradas al usuario, vamos a mejorar la experiencia de usuario y el código de bot mediante la administración de varios flujos de conversación a través de cuadros de diálogo.</span><span class="sxs-lookup"><span data-stu-id="0d875-216">Now that you know how to prompt a user for input, lets enhance the bot code and user experience by managing various conversation flows through dialogs.</span></span>


