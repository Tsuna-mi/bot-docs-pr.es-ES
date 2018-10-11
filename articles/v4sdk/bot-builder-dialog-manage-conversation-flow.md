---
title: Administración de un flujo de conversación simple con diálogos | Microsoft Docs
description: Aprenda a administrar un flujo de conversación simple con diálogos en Bot Builder SDK para Node.js.
keywords: flujo de conversación simple, diálogos, avisos, cascadas, conjunto de diálogos
author: v-ducvo
ms.author: v-ducvo
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 9/25/2018
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: c70711d747e9646acf63b6ee206d0b8db25ef202
ms.sourcegitcommit: 3cb288cf2f09eaede317e1bc8d6255becf1aec61
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2018
ms.locfileid: "47389694"
---
# <a name="manage-simple-conversation-flow-with-dialogs"></a><span data-ttu-id="c434f-104">Administración de un flujo de conversación simple con diálogos</span><span class="sxs-lookup"><span data-stu-id="c434f-104">Manage simple conversation flow with dialogs</span></span>

[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]

<span data-ttu-id="c434f-105">Mediante la biblioteca Dialogs es posible administrar flujos de conversación simples y complejos.</span><span class="sxs-lookup"><span data-stu-id="c434f-105">You can manage simple and complex conversation flows using the dialogs library.</span></span> <span data-ttu-id="c434f-106">En un flujo de conversación simple, el usuario comienza por el primer paso de una *cascada*, continúa hasta el último y la conversación finaliza.</span><span class="sxs-lookup"><span data-stu-id="c434f-106">In a simple conversation flow, the user starts from the first step of a *waterfall*, continues through to the last step, and the conversation finishes.</span></span> <span data-ttu-id="c434f-107">Los [flujos de conversación complejos](~/v4sdk/bot-builder-dialog-manage-complex-conversation-flow.md) incluyen ramas y bucles.</span><span class="sxs-lookup"><span data-stu-id="c434f-107">[Complex conversation flows](~/v4sdk/bot-builder-dialog-manage-complex-conversation-flow.md) include branches and loop.</span></span>

<!-- TODO: This paragraph belongs in a conceptual topic. -->

<span data-ttu-id="c434f-108">Los diálogos son estructuras del bot que actúan como funciones en el programa del bot.</span><span class="sxs-lookup"><span data-stu-id="c434f-108">Dialogs are structures in your bot that act like a functions in your bot's program.</span></span> <span data-ttu-id="c434f-109">Los diálogos crean los mensajes que envía el bot y llevan a cabo las tareas de cálculo necesarias.</span><span class="sxs-lookup"><span data-stu-id="c434f-109">Dialogs build the messages your bot sends, and carry out the computational tasks required.</span></span> <span data-ttu-id="c434f-110">Están diseñados para realizar operaciones específicas, en un orden concreto.</span><span class="sxs-lookup"><span data-stu-id="c434f-110">They are designed to perform a specific operations, in a specific order.</span></span> <span data-ttu-id="c434f-111">Se pueden invocar de varias formas, unas veces en respuesta a un usuario y otras en respuesta a algún estímulo exterior, o por otros diálogos.</span><span class="sxs-lookup"><span data-stu-id="c434f-111">They can be invoked in different ways - sometimes in response to a user, sometimes in response to some outside stimuli, or by other dialogs.</span></span>

<span data-ttu-id="c434f-112">El uso de diálogos permite al desarrollador de bots guiar el flujo de la conversación.</span><span class="sxs-lookup"><span data-stu-id="c434f-112">Using dialogs enables the bot developer to guide the conversational flow.</span></span> <span data-ttu-id="c434f-113">Puede crear varios diálogos y vincularlos para crear cualquier flujo de conversación que desee que administre el bot.</span><span class="sxs-lookup"><span data-stu-id="c434f-113">You can create multiple dialogs and link them together to create any conversation flow that you want your bot to handle.</span></span> <span data-ttu-id="c434f-114">La biblioteca **Dialogs** del SDK Bot Builder incluye características integradas como _mensajes_, _diálogos en cascada_ y _diálogos de componentes_ que le ayudan a administrar el flujo de la conversación.</span><span class="sxs-lookup"><span data-stu-id="c434f-114">The **Dialogs** library in the Bot Builder SDK includes built-in features such as _prompts_, _waterfall dialogs_ and _component dialogs_ to help you manage conversation flow.</span></span> <span data-ttu-id="c434f-115">Puede utilizar avisos para pedir a los usuarios distintos tipos de información.</span><span class="sxs-lookup"><span data-stu-id="c434f-115">You can use prompts to ask users for different types of information.</span></span> <span data-ttu-id="c434f-116">Puede usar una cascada para combinar varios pasos en una secuencia.</span><span class="sxs-lookup"><span data-stu-id="c434f-116">You can use a waterfall to combine multiple steps together in a sequence.</span></span> <span data-ttu-id="c434f-117">Y puede usar los diálogos de componentes para crear sistemas de diálogo modulares que contienen varios diálogos secundarios.</span><span class="sxs-lookup"><span data-stu-id="c434f-117">And you can use component dialogs to create modular dialog systems containing multiple sub-dialogs.</span></span>

<span data-ttu-id="c434f-118">En este artículo, se usan _conjuntos de diálogos_ para crear un flujo de conversación que contiene tanto mensajes como cascadas.</span><span class="sxs-lookup"><span data-stu-id="c434f-118">In this article, we use _dialog sets_ to create a conversation flow that contains both prompts and waterfalls.</span></span> <span data-ttu-id="c434f-119">Vamos a recurrir al código del **ejemplo de mensaje de varios turnos**  [[C#](https://aka.ms/cs-multi-prompts-sample)|[JS](https://aka.ms/js-multi-prompts-sample)].</span><span class="sxs-lookup"><span data-stu-id="c434f-119">We will draw on code from the **multi-turn prompt** [[C#](https://aka.ms/cs-multi-prompts-sample)|[JS](https://aka.ms/js-multi-prompts-sample)] sample.</span></span>

<span data-ttu-id="c434f-120">Para obtener información general acerca de los diálogos, vea [biblioteca Dialogs](bot-builder-concept-dialog.md) y [estado de diálogos](bot-builder-dialog-state.md).</span><span class="sxs-lookup"><span data-stu-id="c434f-120">For an overview of dialogs, see [dialogs library](bot-builder-concept-dialog.md) and [dialogs state](bot-builder-dialog-state.md).</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="c434f-121">C#</span><span class="sxs-lookup"><span data-stu-id="c434f-121">C#</span></span>](#tab/csharp)

<span data-ttu-id="c434f-122">Para usar diálogos en general, necesita el paquete NuGet `Microsoft.Bot.Builder.Dialogs` en su proyecto o solución.</span><span class="sxs-lookup"><span data-stu-id="c434f-122">To use dialogs in general, you need the `Microsoft.Bot.Builder.Dialogs` NuGet package for your project or solution.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="c434f-123">JavaScript</span><span class="sxs-lookup"><span data-stu-id="c434f-123">JavaScript</span></span>](#tab/javascript)

<span data-ttu-id="c434f-124">Para usar diálogos en general, necesita la biblioteca `botbuilder-dialogs`, que puede descargarse de NPM.</span><span class="sxs-lookup"><span data-stu-id="c434f-124">To use dialogs in general, you need the `botbuilder-dialogs` library, which can be downloaded from NPM.</span></span>

---

## <a name="using-dialogs-to-guide-the-user-through-steps"></a><span data-ttu-id="c434f-125">Uso de diálogos para guiar al usuario paso a paso</span><span class="sxs-lookup"><span data-stu-id="c434f-125">Using dialogs to guide the user through steps</span></span>

<span data-ttu-id="c434f-126">En este ejemplo, se crea un diálogo de varios pasos para solicitar información al usuario mediante un conjunto de diálogos.</span><span class="sxs-lookup"><span data-stu-id="c434f-126">In this example, we create a multi-step dialog to prompt the user for information utilizing a dialog set.</span></span>

### <a name="create-a-dialog-with-waterfall-steps"></a><span data-ttu-id="c434f-127">Creación de un diálogo con pasos de cascada</span><span class="sxs-lookup"><span data-stu-id="c434f-127">Create a dialog with waterfall steps</span></span>

<span data-ttu-id="c434f-128">**WaterfallDialog** es una implementación concreta de un diálogo que se usa normalmente para recopilar información del usuario o guiarlo en una serie de tareas.</span><span class="sxs-lookup"><span data-stu-id="c434f-128">A **WaterfallDialog** is a specific implementation of a dialog that is commonly used to collect information from the user or guide the user through a series of tasks.</span></span> <span data-ttu-id="c434f-129">Cada uno de los pasos de la conversación se implementa como una función.</span><span class="sxs-lookup"><span data-stu-id="c434f-129">Each step of the conversation is implemented as a function.</span></span> <span data-ttu-id="c434f-130">En cada paso, el bot [pide al usuario que intervenga](bot-builder-prompts.md), espera una respuesta y, a continuación, pasa el resultado al paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="c434f-130">At each step, the bot [prompts the user for input](bot-builder-prompts.md), waits for a response, and then passes the result to the next step.</span></span> <span data-ttu-id="c434f-131">El resultado de la primera función se pasa como argumento a la función siguiente y así sucesivamente.</span><span class="sxs-lookup"><span data-stu-id="c434f-131">The result of the first function is passed as an argument into the next function, and so on.</span></span>

<span data-ttu-id="c434f-132">Por ejemplo, el siguiente ejemplo de código define una matriz de delegados que representan los pasos de una **cascada**.</span><span class="sxs-lookup"><span data-stu-id="c434f-132">For example, the following code sample defines an array of delegates that represent the steps of a **waterfall**.</span></span> <span data-ttu-id="c434f-133">Después de cada pregunta, el bot confirma que ha recibido la información del usuario.</span><span class="sxs-lookup"><span data-stu-id="c434f-133">After each prompt, the bot acknowledges the user's input.</span></span> <span data-ttu-id="c434f-134">Hay muchas maneras de guardar la información que se recopila en un diálogo.</span><span class="sxs-lookup"><span data-stu-id="c434f-134">There are many ways you could persist the input you collect in a dialog.</span></span> <span data-ttu-id="c434f-135">Para ver algunas de las opciones, consulte [Conservación de datos de usuario](bot-builder-tutorial-persist-user-inputs.md).</span><span class="sxs-lookup"><span data-stu-id="c434f-135">See [Persist user data](bot-builder-tutorial-persist-user-inputs.md) for some of the options.</span></span>

<span data-ttu-id="c434f-136">En este ejemplo se escribe la información directamente en el perfil del usuario tal como se recopila en el diálogo.</span><span class="sxs-lookup"><span data-stu-id="c434f-136">This sample writes information directly to the user's profile as it is collected in the dialog.</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="c434f-137">C#</span><span class="sxs-lookup"><span data-stu-id="c434f-137">C#</span></span>](#tab/csharp)

<span data-ttu-id="c434f-138">En este ejemplo, el diálogo en cascada se define en el archivo del bot.</span><span class="sxs-lookup"><span data-stu-id="c434f-138">In this sample, the waterfall dialog is defined within the bot file.</span></span>

<span data-ttu-id="c434f-139">Haga referencia a los espacios de nombres que se utilizan en este archivo.</span><span class="sxs-lookup"><span data-stu-id="c434f-139">Reference the namespaces used in this file.</span></span>

```csharp
using System;
using System.Linq;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.Bot.Builder;
using Microsoft.Bot.Builder.Dialogs;
using Microsoft.Bot.Schema;
```

<span data-ttu-id="c434f-140">Definir una propiedad de instancia para el conjunto de diálogos.</span><span class="sxs-lookup"><span data-stu-id="c434f-140">Define an instance property for the dialog set.</span></span>

```csharp
/// <summary>
/// The <see cref="DialogSet"/> that contains all the Dialogs that can be used at runtime.
/// </summary>
private DialogSet _dialogs;
```

<span data-ttu-id="c434f-141">Cree el conjunto de diálogos dentro del constructor del bot y agregue tanto los mensajes como el diálogo en cascada al conjunto.</span><span class="sxs-lookup"><span data-stu-id="c434f-141">Create the dialog set within the bot's constructor, adding the prompts and the waterfall dialog to the set.</span></span>

```csharp
/// <summary>
/// Initializes a new instance of the <see cref="MultiTurnPromptsBot"/> class.
/// </summary>
/// <param name="accessors">A class containing <see cref="IStatePropertyAccessor{T}"/> used to manage state.</param>
public MultiTurnPromptsBot(MultiTurnPromptsBotAccessors accessors)
{
    _accessors = accessors ?? throw new ArgumentNullException(nameof(accessors));

    // The DialogSet needs a DialogState accessor, it will call it when it has a turn context.
    _dialogs = new DialogSet(accessors.ConversationDialogState);

    // This array defines how the Waterfall will execute.
    var waterfallSteps = new WaterfallStep[]
    {
        NameStepAsync,
        NameConfirmStepAsync,
        AgeStepAsync,
        ConfirmStepAsync,
        SummaryStepAsync,
    };

    // Add named dialogs to the DialogSet. These names are saved in the dialog state.
    _dialogs.Add(new WaterfallDialog("details", waterfallSteps));
    _dialogs.Add(new TextPrompt("name"));
    _dialogs.Add(new NumberPrompt<int>("age"));
    _dialogs.Add(new ConfirmPrompt("confirm"));
}
```

<span data-ttu-id="c434f-142">Y, por último, defina cada paso como un método independiente.</span><span class="sxs-lookup"><span data-stu-id="c434f-142">And, define each step as a separate method.</span></span> <span data-ttu-id="c434f-143">También puede definir los pasos en línea mediante expresiones lambda.</span><span class="sxs-lookup"><span data-stu-id="c434f-143">You could also define the steps in-line using lambda expressions.</span></span>

```csharp
/// <summary>
/// One of the functions that make up the <see cref="WaterfallDialog"/>.
/// </summary>
/// <param name="stepContext">The <see cref="WaterfallStepContext"/> gives access to the executing dialog runtime.</param>
/// <param name="cancellationToken">A <see cref="CancellationToken"/>.</param>
/// <returns>A <see cref="DialogTurnResult"/> to communicate some flow control back to the containing WaterfallDialog.</returns>
private static async Task<DialogTurnResult> NameStepAsync(WaterfallStepContext stepContext, CancellationToken cancellationToken)
{
    // WaterfallStep always finishes with the end of the Waterfall or with another dialog; here it is a Prompt Dialog.
    // Running a prompt here means the next WaterfallStep will be run when the users response is received.
    return await stepContext.PromptAsync("name", new PromptOptions { Prompt = MessageFactory.Text("Please enter your name.") }, cancellationToken);
}

/// <summary>
/// One of the functions that make up the <see cref="WaterfallDialog"/>.
/// </summary>
/// <param name="stepContext">The <see cref="WaterfallStepContext"/> gives access to the executing dialog runtime.</param>
/// <param name="cancellationToken">A <see cref="CancellationToken"/>.</param>
/// <returns>A <see cref="DialogTurnResult"/> to communicate some flow control back to the containing WaterfallDialog.</returns>
private async Task<DialogTurnResult> NameConfirmStepAsync(WaterfallStepContext stepContext, CancellationToken cancellationToken)
{
    // Get the current profile object from user state.
    var userProfile = await _accessors.UserProfile.GetAsync(stepContext.Context, () => new UserProfile(), cancellationToken);

    // Update the profile.
    userProfile.Name = (string)stepContext.Result;

    // We can send messages to the user at any point in the WaterfallStep.
    await stepContext.Context.SendActivityAsync(MessageFactory.Text($"Thanks {stepContext.Result}."), cancellationToken);

    // WaterfallStep always finishes with the end of the Waterfall or with another dialog; here it is a Prompt Dialog.
    return await stepContext.PromptAsync("confirm", new PromptOptions { Prompt = MessageFactory.Text("Would you like to give your age?") }, cancellationToken);
}

/// <summary>
/// One of the functions that make up the <see cref="WaterfallDialog"/>.
/// </summary>
/// <param name="stepContext">The <see cref="WaterfallStepContext"/> gives access to the executing dialog runtime.</param>
/// <param name="cancellationToken">A <see cref="CancellationToken"/>.</param>
/// <returns>A <see cref="DialogTurnResult"/> to communicate some flow control back to the containing WaterfallDialog.</returns>
private async Task<DialogTurnResult> AgeStepAsync(WaterfallStepContext stepContext, CancellationToken cancellationToken)
{
    if ((bool)stepContext.Result)
    {
        // User said "yes" so we will be prompting for the age.

        // Get the current profile object from user state.
        var userProfile = await _accessors.UserProfile.GetAsync(stepContext.Context, () => new UserProfile(), cancellationToken);

        // WaterfallStep always finishes with the end of the Waterfall or with another dialog, here it is a Prompt Dialog.
        return await stepContext.PromptAsync("age", new PromptOptions { Prompt = MessageFactory.Text("Please enter your age.") }, cancellationToken);
    }
    else
    {
        // User said "no" so we will skip the next step. Give -1 as the age.
        return await stepContext.NextAsync(-1, cancellationToken);
    }
}

/// <summary>
/// One of the functions that make up the <see cref="WaterfallDialog"/>.
/// </summary>
/// <param name="stepContext">The <see cref="WaterfallStepContext"/> gives access to the executing dialog runtime.</param>
/// <param name="cancellationToken">A <see cref="CancellationToken"/>.</param>
/// <returns>A <see cref="DialogTurnResult"/> to communicate some flow control back to the containing WaterfallDialog.</returns>
private async Task<DialogTurnResult> ConfirmStepAsync(WaterfallStepContext stepContext, CancellationToken cancellationToken)
{
    // Get the current profile object from user state.
    var userProfile = await _accessors.UserProfile.GetAsync(stepContext.Context, () => new UserProfile(), cancellationToken);

    // Update the profile.
    userProfile.Age = (int)stepContext.Result;

    // We can send messages to the user at any point in the WaterfallStep.
    if (userProfile.Age == -1)
    {
        await stepContext.Context.SendActivityAsync(MessageFactory.Text($"No age given."), cancellationToken);
    }
    else
    {
        // We can send messages to the user at any point in the WaterfallStep.
        await stepContext.Context.SendActivityAsync(MessageFactory.Text($"I have your age as {userProfile.Age}."), cancellationToken);
    }

    // WaterfallStep always finishes with the end of the Waterfall or with another dialog, here it is a Prompt Dialog.
    return await stepContext.PromptAsync("confirm", new PromptOptions { Prompt = MessageFactory.Text("Is this ok?") }, cancellationToken);
}

/// <summary>
/// One of the functions that make up the <see cref="WaterfallDialog"/>.
/// </summary>
/// <param name="stepContext">The <see cref="WaterfallStepContext"/> gives access to the executing dialog runtime.</param>
/// <param name="cancellationToken">A <see cref="CancellationToken"/>.</param>
/// <returns>A <see cref="DialogTurnResult"/> to communicate some flow control back to the containing WaterfallDialog.</returns>
private async Task<DialogTurnResult> SummaryStepAsync(WaterfallStepContext stepContext, CancellationToken cancellationToken)
{
    if ((bool)stepContext.Result)
    {
        // Get the current profile object from user state.
        var userProfile = await _accessors.UserProfile.GetAsync(stepContext.Context, () => new UserProfile(), cancellationToken);

        // We can send messages to the user at any point in the WaterfallStep.
        if (userProfile.Age == -1)
        {
            await stepContext.Context.SendActivityAsync(MessageFactory.Text($"I have your name as {userProfile.Name}."), cancellationToken);
        }
        else
        {
            await stepContext.Context.SendActivityAsync(MessageFactory.Text($"I have your name as {userProfile.Name} and age as {userProfile.Age}."), cancellationToken);
        }
    }
    else
    {
        // We can send messages to the user at any point in the WaterfallStep.
        await stepContext.Context.SendActivityAsync(MessageFactory.Text("Thanks. Your profile will not be kept."), cancellationToken);
    }

    // WaterfallStep always finishes with the end of the Waterfall or with another dialog, here it is the end.
    return await stepContext.EndDialogAsync(cancellationToken: cancellationToken);
}
```

<span data-ttu-id="c434f-144">El diálogo se ejecuta desde el bot en el controlador de turnos, que primero crea un contexto de diálogo y continúa o comienza el diálogo, según corresponda, y, después, guarda la conversación y el estado de usuario al final del turno.</span><span class="sxs-lookup"><span data-stu-id="c434f-144">The dialog is run from the bot's on turn handler, which first creates a dialog context and continues or begins the dialog as appropriate, and then saves conversation and user state at the end of the turn.</span></span>

```csharp
// Run the DialogSet - let the framework identify the current state of the dialog from
// the dialog stack and figure out what (if any) is the active dialog.
var dialogContext = await _dialogs.CreateContextAsync(turnContext, cancellationToken);
var results = await dialogContext.ContinueDialogAsync(cancellationToken);

// If the DialogTurnStatus is Empty we should start a new dialog.
if (results.Status == DialogTurnStatus.Empty)
{
    await dialogContext.BeginDialogAsync("details", null, cancellationToken);
}
```

```csharp
// Save the dialog state into the conversation state.
await _accessors.ConversationState.SaveChangesAsync(turnContext, false, cancellationToken);

// Save the user profile updates into the user state.
await _accessors.UserState.SaveChangesAsync(turnContext, false, cancellationToken);
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="c434f-145">JavaScript</span><span class="sxs-lookup"><span data-stu-id="c434f-145">JavaScript</span></span>](#tab/javascript)

<span data-ttu-id="c434f-146">En este ejemplo, el diálogo en cascada se define en el archivo **bot.js**.</span><span class="sxs-lookup"><span data-stu-id="c434f-146">In this sample, the waterfall dialog is defined within the **bot.js** file.</span></span>

<span data-ttu-id="c434f-147">Importe los objetos que necesita para el código.</span><span class="sxs-lookup"><span data-stu-id="c434f-147">Import the objects you need for the code.</span></span>

```javascript
const { ActivityTypes } = require('botbuilder');
const { ChoicePrompt, DialogSet, NumberPrompt, TextPrompt, WaterfallDialog } = require('botbuilder-dialogs');
```

<span data-ttu-id="c434f-148">Defina y cree el conjunto de diálogos en el constructor del bot y agregue tanto los mensajes como el diálogo en cascada al conjunto.</span><span class="sxs-lookup"><span data-stu-id="c434f-148">Define and create the dialog set in the bot's constructor, adding the prompts and the waterfall dialog to the set.</span></span>

```javascript
/**
*
* @param {ConversationState} conversationState A ConversationState object used to store the dialog state.
* @param {UserState} userState A UserState object used to store values specific to the user.
*/
constructor(conversationState, userState) {
    // Create a new state accessor property. See https://aka.ms/about-bot-state-accessors to learn more about bot state and state accessors.
    this.conversationState = conversationState;
    this.userState = userState;

    this.dialogState = this.conversationState.createProperty(DIALOG_STATE_PROPERTY);

    this.userProfile = this.userState.createProperty(USER_PROFILE_PROPERTY);

    this.dialogs = new DialogSet(this.dialogState);

    // Add prompts that will be used by the main dialogs.
    this.dialogs.add(new TextPrompt(NAME_PROMPT));
    this.dialogs.add(new ChoicePrompt(CONFIRM_PROMPT));
    this.dialogs.add(new NumberPrompt(AGE_PROMPT, async (prompt) => {
        if (prompt.recognized.succeeded) {
            if (prompt.recognized.value <= 0) {
                await prompt.context.sendActivity(`Your age can't be less than zero.`);
                return false;
            } else {
                return true;
            }
        }

        return false;
    }));

    // Create a dialog that asks the user for their name.
    this.dialogs.add(new WaterfallDialog(WHO_ARE_YOU, [
        this.promptForName.bind(this),
        this.confirmAgePrompt.bind(this),
        this.promptForAge.bind(this),
        this.captureAge.bind(this)
    ]));

    // Create a dialog that displays a user name after it has been collected.
    this.dialogs.add(new WaterfallDialog(HELLO_USER, [
        this.displayProfile.bind(this)
    ]));
}
```

<span data-ttu-id="c434f-149">Y, por último, defina cada paso como un método independiente.</span><span class="sxs-lookup"><span data-stu-id="c434f-149">And, define each step as a separate method.</span></span> <span data-ttu-id="c434f-150">También puede definir los pasos en línea mediante expresiones lambda.</span><span class="sxs-lookup"><span data-stu-id="c434f-150">You could also define the steps in-line using lambda expressions.</span></span>

```javascript
// This step in the dialog prompts the user for their name.
async promptForName(step) {
    return await step.prompt(NAME_PROMPT, `What is your name, human?`);
}

// This step captures the user's name, then prompts whether or not to collect an age.
async confirmAgePrompt(step) {
    const user = await this.userProfile.get(step.context, {});
    user.name = step.result;
    await this.userProfile.set(step.context, user);
    await step.prompt(CONFIRM_PROMPT, 'Do you want to give your age?', ['yes', 'no']);
}

// This step checks the user's response - if yes, the bot will proceed to prompt for age.
// Otherwise, the bot will skip the age step.
async promptForAge(step) {
    if (step.result && step.result.value === 'yes') {
        return await step.prompt(AGE_PROMPT, `What is your age?`,
            {
                retryPrompt: 'Sorry, please specify your age as a positive number or say cancel.'
            }
        );
    } else {
        return await step.next(-1);
    }
}

// This step captures the user's age.
async captureAge(step) {
    const user = await this.userProfile.get(step.context, {});
    if (step.result !== -1) {
        user.age = step.result;
        await this.userProfile.set(step.context, user);
        await step.context.sendActivity(`I will remember that you are ${ step.result } years old.`);
    } else {
        await step.context.sendActivity(`No age given.`);
    }
    return await step.endDialog();
}

// This step displays the captured information back to the user.
async displayProfile(step) {
    const user = await this.userProfile.get(step.context, {});
    if (user.age) {
        await step.context.sendActivity(`Your name is ${ user.name } and you are ${ user.age } years old.`);
    } else {
        await step.context.sendActivity(`Your name is ${ user.name } and you did not share your age.`);
    }
    return await step.endDialog();
}
```

<span data-ttu-id="c434f-151">El diálogo se ejecuta desde el bot en el controlador de turnos, que primero crea un contexto de diálogo y continúa o comienza el diálogo, según corresponda, y, después, guarda la conversación y el estado de usuario al final del turno.</span><span class="sxs-lookup"><span data-stu-id="c434f-151">The dialog is run from the bot's on turn handler, which first creates a dialog context and continues or begins the dialog as appropriate, and then saves conversation and user state at the end of the turn.</span></span>

```javascript
// Create a dialog context object.
const dc = await this.dialogs.createContext(turnContext);
```

```javascript
// If the bot has not yet responded, continue processing the current dialog.
await dc.continueDialog();
```

```javascript
// Start the sample dialog in response to any other input.
if (!turnContext.responded) {
    const user = await this.userProfile.get(dc.context, {});
    if (user.name) {
        await dc.beginDialog(HELLO_USER);
    } else {
        await dc.beginDialog(WHO_ARE_YOU);
    }
}
```

```javascript
// Save changes to the user state.
await this.userState.saveChanges(turnContext);

// End this turn by saving changes to the conversation state.
await this.conversationState.saveChanges(turnContext);
```

---

## <a name="dialog-context-and-waterfall-step-context-objects"></a><span data-ttu-id="c434f-152">Contexto de diálogo y contexto de ambiente de los pasos de la cascada</span><span class="sxs-lookup"><span data-stu-id="c434f-152">Dialog context and waterfall step context objects</span></span>

<span data-ttu-id="c434f-153">Use el contexto de ambiente del diálogo para interactuar con un conjunto de diálogos desde el controlador de turnos del bot.</span><span class="sxs-lookup"><span data-stu-id="c434f-153">Use the dialog context object to interact with a dialog set from within your bot's turn handler.</span></span>
<span data-ttu-id="c434f-154">Use el contexto de ambiente del paso de cascada para interactuar con un conjunto de diálogos desde un paso de cascada.</span><span class="sxs-lookup"><span data-stu-id="c434f-154">Use the waterfall step context object to interact with a dialog set from within a waterfall step.</span></span>

## <a name="to-start-a-dialog"></a><span data-ttu-id="c434f-155">Para iniciar un diálogo</span><span class="sxs-lookup"><span data-stu-id="c434f-155">To start a dialog</span></span>

<span data-ttu-id="c434f-156">Para iniciar un diálogo, pase el valor de *dialogId* que desea iniciar en el método _beginDialog_, _prompt_ o _replaceDialog_ del contexto de diálogo.</span><span class="sxs-lookup"><span data-stu-id="c434f-156">To start a dialog, pass the *dialogId* you want to start into the dialog context's _beginDialog_, _prompt_, or _replaceDialog_ method.</span></span> <span data-ttu-id="c434f-157">El método _beginDialog_ insertará el diálogo al principio de la pila, mientras que el método _replaceDialog_ sacará el diálogo actual de la pila e insertará el diálogo sustituto en ella.</span><span class="sxs-lookup"><span data-stu-id="c434f-157">The _beginDialog_ method will push the dialog onto the top of the stack, while the _replaceDialog_ method will pop the current dialog off the stack and push the replacing dialog onto the stack.</span></span>

<span data-ttu-id="c434f-158">El método _prompt_ del contexto del diálogo es un método asistente que toma argumentos y construye las opciones adecuadas para el mensaje y, después, comienza el diálogo de mensajes.</span><span class="sxs-lookup"><span data-stu-id="c434f-158">The dialog context's _prompt_ method is a helper method that takes in arguments and constructs the appropriate options for the prompt; then, it begins the prompt dialog.</span></span> <span data-ttu-id="c434f-159">Para más información sobre los avisos, consulte [Petición de datos de entrada al usuario](bot-builder-prompts.md).</span><span class="sxs-lookup"><span data-stu-id="c434f-159">For more information on prompts, see [Prompt user for input](bot-builder-prompts.md).</span></span>

## <a name="to-end-a-dialog"></a><span data-ttu-id="c434f-160">Para finalizar un diálogo</span><span class="sxs-lookup"><span data-stu-id="c434f-160">To end a dialog</span></span>

<span data-ttu-id="c434f-161">El método _end dialog_ finaliza un diálogo retirándolo de la pila y devuelve un resultado opcional al diálogo primario.</span><span class="sxs-lookup"><span data-stu-id="c434f-161">The _end dialog_ method ends a dialog by popping it off the stack and returns an optional result to the parent dialog.</span></span>

<span data-ttu-id="c434f-162">Es aconsejable llamar explícitamente al método _endDialog_ al final del diálogo.</span><span class="sxs-lookup"><span data-stu-id="c434f-162">It is best practice to explicitly call the _endDialog_ method at the end of the dialog.</span></span>

## <a name="to-clear-the-dialog-stack"></a><span data-ttu-id="c434f-163">Para borrar la pila de diálogos</span><span class="sxs-lookup"><span data-stu-id="c434f-163">To clear the dialog stack</span></span>

<span data-ttu-id="c434f-164">Si desea quitar todos los diálogos de la pila, puede borrar la pila de diálogos, para lo que debe llamar al método _cancel all dialogs_ del contexto del diálogo.</span><span class="sxs-lookup"><span data-stu-id="c434f-164">If you want to pop all dialogs off the stack, you can clear the dialog stack by calling the dialog context's _cancel all dialogs_ method.</span></span>

## <a name="to-repeat-a-dialog"></a><span data-ttu-id="c434f-165">Para repetir un diálogo</span><span class="sxs-lookup"><span data-stu-id="c434f-165">To repeat a dialog</span></span>

<span data-ttu-id="c434f-166">Para repetir un diálogo, use el método _replace dialog_, que quitará el diálogo actual de la pila, insertará el diálogo sustituto al principio de la pila y comenzará dicho diálogo.</span><span class="sxs-lookup"><span data-stu-id="c434f-166">To repeat a dialog, use the _replace dialog_ method, which will pop the current dialog off the stack and push the replacing dialog onto the top of the stack and begin that dialog.</span></span> <span data-ttu-id="c434f-167">Es una excelente manera de controlar [flujos de conversación complejos](~/v4sdk/bot-builder-dialog-manage-complex-conversation-flow.md) y una buena técnica para administrar menús.</span><span class="sxs-lookup"><span data-stu-id="c434f-167">This is a great way to handle [complex conversation flows](~/v4sdk/bot-builder-dialog-manage-complex-conversation-flow.md) and a good technique to manage menus.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c434f-168">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="c434f-168">Next steps</span></span>

<span data-ttu-id="c434f-169">Ahora que ha aprendido a administrar flujos de conversación simples, vamos a examinar cómo se puede sacar provecho del método _replace dialog_ para controlar flujos de conversación complejos.</span><span class="sxs-lookup"><span data-stu-id="c434f-169">Now that you've learned how to manage simple conversation flows, let's take a look at how you can leverage the _replace dialog_ method to handle complex conversation flows.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="c434f-170">Administración de flujos de conversación complejos</span><span class="sxs-lookup"><span data-stu-id="c434f-170">Manage complex conversation flow</span></span>](bot-builder-dialog-manage-complex-conversation-flow.md)
