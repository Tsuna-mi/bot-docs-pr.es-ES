---
title: Petición de datos de entrada a los usuarios mediante la biblioteca Dialogs | Microsoft Docs
description: Obtenga información sobre cómo solicitar datos de entrada a los usuarios en Bot Builder SDK para Node.js.
keywords: mensajes, cuadros de diálogo, AttachmentPrompt, ChoicePrompt, ConfirmPrompt, DatetimePrompt, NumberPrompt, TextPrompt, volver a solicitar, validación
author: v-ducvo
ms.author: v-ducvo
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 9/25/2018
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: 27066f76db29a82b4ab9dd75bf5eee01dcce3116
ms.sourcegitcommit: 3cb288cf2f09eaede317e1bc8d6255becf1aec61
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2018
ms.locfileid: "47389714"
---
# <a name="prompt-users-for-input-using-the-dialogs-library"></a><span data-ttu-id="94404-104">Petición de datos de entrada a los usuarios con la biblioteca Dialogs</span><span class="sxs-lookup"><span data-stu-id="94404-104">Prompt users for input using the Dialogs library</span></span>

[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]

<span data-ttu-id="94404-105">La recopilación de información mediante la publicación de preguntas es una de las principales formas de interacción de un bot con los usuarios.</span><span class="sxs-lookup"><span data-stu-id="94404-105">Gathering information by posting questions is one of the main ways a bot interacts with users.</span></span> <span data-ttu-id="94404-106">Se puede hacer directamente mediante el método [send activity](bot-builder-concept-activity-processing.md#turn-context) del objeto _turn context_ y luego procesar el mensaje entrante siguiente como la respuesta.</span><span class="sxs-lookup"><span data-stu-id="94404-106">It is possible to do this directly by using the [turn context](bot-builder-concept-activity-processing.md#turn-context) object's _send activity_ method and then process the next incoming message as the response.</span></span> <span data-ttu-id="94404-107">Sin embargo, el SDK Bot Builder proporciona una biblioteca **Dialogs** que contiene métodos diseñados para facilitar la realización de preguntas y para asegurarse de que la respuesta se ajusta a un tipo de datos concreto o cumple reglas de validación personalizadas.</span><span class="sxs-lookup"><span data-stu-id="94404-107">However, the Bot Builder SDK provides a **dialogs** library that provides methods designed to make it easier to ask questions, and to make sure the response matches a specific data type or meets custom validation rules.</span></span> <span data-ttu-id="94404-108">En este tema se detalla cómo hacerlo mediante el uso de **preguntas** para pedir al usuario la información deseada.</span><span class="sxs-lookup"><span data-stu-id="94404-108">This topic details how to achieve this using **prompts** to ask a user for input.</span></span>

<span data-ttu-id="94404-109">En este artículo se describe cómo usar las preguntas dentro de un cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="94404-109">This article describes how to use prompts within a dialog.</span></span> <span data-ttu-id="94404-110">Para obtener información sobre el uso de los diálogos en general, consulte [Uso de diálogos para administrar un flujo de conversación simple](bot-builder-dialog-manage-conversation-flow.md).</span><span class="sxs-lookup"><span data-stu-id="94404-110">For information on using dialogs in general, see [using dialogs to manage simple conversation flow](bot-builder-dialog-manage-conversation-flow.md).</span></span>

## <a name="prompt-types"></a><span data-ttu-id="94404-111">Tipos de avisos</span><span class="sxs-lookup"><span data-stu-id="94404-111">Prompt types</span></span>

<span data-ttu-id="94404-112">La biblioteca Dialogs ofrece varios tipos de preguntas, y cada uno de ellos se usa para recopilar un tipo de respuesta diferente.</span><span class="sxs-lookup"><span data-stu-id="94404-112">The dialogs library offers a number of different types of prompts, each used for collecting a different type of response.</span></span>

| <span data-ttu-id="94404-113">Prompt</span><span class="sxs-lookup"><span data-stu-id="94404-113">Prompt</span></span> | <span data-ttu-id="94404-114">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="94404-114">Description</span></span> |
|:----|:----|
| <span data-ttu-id="94404-115">**AttachmentPrompt**</span><span class="sxs-lookup"><span data-stu-id="94404-115">**AttachmentPrompt**</span></span> | <span data-ttu-id="94404-116">Solicita al usuario un archivo adjunto como un documento o una imagen.</span><span class="sxs-lookup"><span data-stu-id="94404-116">Prompt the user for an attachment such as a document or image.</span></span> |
| <span data-ttu-id="94404-117">**ChoicePrompt**</span><span class="sxs-lookup"><span data-stu-id="94404-117">**ChoicePrompt**</span></span> | <span data-ttu-id="94404-118">Solicita al usuario que elija entre un conjunto de opciones.</span><span class="sxs-lookup"><span data-stu-id="94404-118">Prompt the user to choose from a set of options.</span></span> |
| <span data-ttu-id="94404-119">**ConfirmPrompt**</span><span class="sxs-lookup"><span data-stu-id="94404-119">**ConfirmPrompt**</span></span> | <span data-ttu-id="94404-120">Solicita al usuario que confirme su acción.</span><span class="sxs-lookup"><span data-stu-id="94404-120">Prompt the user to confirm their action.</span></span> |
| <span data-ttu-id="94404-121">**DatetimePrompt**</span><span class="sxs-lookup"><span data-stu-id="94404-121">**DatetimePrompt**</span></span> | <span data-ttu-id="94404-122">Solicita al usuario una fecha y una hora.</span><span class="sxs-lookup"><span data-stu-id="94404-122">Prompt the user for a date-time.</span></span> <span data-ttu-id="94404-123">Los usuarios puedan responder con lenguaje natural, como "Mañana a las 8 p.m." o "Viernes a las 10 a.m.".</span><span class="sxs-lookup"><span data-stu-id="94404-123">Users can respond using natural language such as "Tomorrow at 8pm" or "Friday at 10am".</span></span> <span data-ttu-id="94404-124">El SDK de Bot Framework usa la entidad precompilada `builtin.datetimeV2` de LUIS.</span><span class="sxs-lookup"><span data-stu-id="94404-124">The Bot Framework SDK uses the LUIS `builtin.datetimeV2` prebuilt entity.</span></span> <span data-ttu-id="94404-125">Para obtener más información, vea [builtin.datetimev2](https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-prebuilt-entities#builtindatetimev2).</span><span class="sxs-lookup"><span data-stu-id="94404-125">For more information, see [builtin.datetimev2](https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-prebuilt-entities#builtindatetimev2).</span></span> |
| <span data-ttu-id="94404-126">**NumberPrompt**</span><span class="sxs-lookup"><span data-stu-id="94404-126">**NumberPrompt**</span></span> | <span data-ttu-id="94404-127">Solicita al usuario un número.</span><span class="sxs-lookup"><span data-stu-id="94404-127">Prompt the user for a number.</span></span> <span data-ttu-id="94404-128">El usuario puede responder con "10" o "diez".</span><span class="sxs-lookup"><span data-stu-id="94404-128">The user can respond with either "10" or "ten".</span></span> <span data-ttu-id="94404-129">Si la respuesta es "diez", por ejemplo, la pregunta convertirá la respuesta en un número y devolverá `10` como resultado.</span><span class="sxs-lookup"><span data-stu-id="94404-129">If the response is "ten", for example, the prompt will convert the response into a number and return `10` as a result.</span></span> |
| <span data-ttu-id="94404-130">**TextPrompt**</span><span class="sxs-lookup"><span data-stu-id="94404-130">**TextPrompt**</span></span> | <span data-ttu-id="94404-131">Solicita al usuario una cadena de texto.</span><span class="sxs-lookup"><span data-stu-id="94404-131">Prompt user for a string of text.</span></span> |

## <a name="add-references-to-prompt-library"></a><span data-ttu-id="94404-132">Agregar referencias a la biblioteca de preguntas</span><span class="sxs-lookup"><span data-stu-id="94404-132">Add references to prompt library</span></span>

<span data-ttu-id="94404-133">Para tener la biblioteca **Dialogs** agregue el paquete **botbuilder-dialogs** al bot.</span><span class="sxs-lookup"><span data-stu-id="94404-133">You can get the **dialogs** library by adding the **botbuilder-dialogs** package to your bot.</span></span> <span data-ttu-id="94404-134">Trataremos los diálogos en [Uso de diálogos para administrar un flujo de conversación simple](bot-builder-dialog-manage-conversation-flow.md), pero vamos a usar diálogos en nuestros avisos.</span><span class="sxs-lookup"><span data-stu-id="94404-134">We cover dialogs in [using dialogs to manage simple conversation flow](bot-builder-dialog-manage-conversation-flow.md), but we'll use dialogs for our prompts.</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="94404-135">C#</span><span class="sxs-lookup"><span data-stu-id="94404-135">C#</span></span>](#tab/csharp)

<span data-ttu-id="94404-136">Instale el paquete **Microsoft.Bot.Builder.Dialogs** de NuGet.</span><span class="sxs-lookup"><span data-stu-id="94404-136">Install the **Microsoft.Bot.Builder.Dialogs** package from NuGet.</span></span>

<span data-ttu-id="94404-137">Luego, incluya la referencia a la biblioteca desde el código del bot.</span><span class="sxs-lookup"><span data-stu-id="94404-137">Then, include reference to the library from your bot code.</span></span>

```cs
using Microsoft.Bot.Builder.Dialogs;
```

<span data-ttu-id="94404-138">Tendrá que configurar el estado de los diálogos de conversación mediante descriptores de acceso.</span><span class="sxs-lookup"><span data-stu-id="94404-138">You'll need to set up the conversation dialog state via accessors.</span></span> <span data-ttu-id="94404-139">Por el momento no vamos a profundizar más en este código, pero pueden encontrar más detalles al respecto en el artículo acerca del [estado](bot-builder-howto-v4-state.md).</span><span class="sxs-lookup"><span data-stu-id="94404-139">We won't dive into this code much here, but more on that can be found in the [state](bot-builder-howto-v4-state.md) article.</span></span>

<span data-ttu-id="94404-140">En las opciones del bot en **Startup.cs**, primero debe definir los objetos de estado y después agregar el elemento singleton que proporciona la clase del descriptor de acceso al constructor del bot.</span><span class="sxs-lookup"><span data-stu-id="94404-140">Within your bot options in **Startup.cs**, first define your state objects, then add the singleton to provide the accessor class to the bot constructor.</span></span> <span data-ttu-id="94404-141">La clase de `BotAccessor` simplemente almacena el estado del usuario y de la conversación, junto con los descriptores de acceso de cada uno de esos elementos.</span><span class="sxs-lookup"><span data-stu-id="94404-141">The class for `BotAccessor` simply stores the conversation and user state, along with accessors for each of those items.</span></span> <span data-ttu-id="94404-142">La definición completa de la clase se puede encontrar en el ejemplo vinculado al final de este artículo.</span><span class="sxs-lookup"><span data-stu-id="94404-142">The full class definition is provided in the sample linked at the end of this article.</span></span> 

```cs
    services.AddBot<MultiTurnPromptsBot>(options =>
    {
        InitCredentialProvider(options);

        // Create and add conversation state.
        var convoState = new ConversationState(dataStore);
        options.State.Add(convoState);

        // Create and add user state.
        var userState = new UserState(dataStore);
        options.State.Add(userState);
    });

    services.AddSingleton(sp =>
    {
        // We need to grab the conversationState we added on the options in the previous step
        var options = sp.GetRequiredService<IOptions<BotFrameworkOptions>>().Value;
        if (options == null)
        {
            throw new InvalidOperationException("BotFrameworkOptions must be configured prior to setting up the State Accessors");
        }

        var conversationState = options.State.OfType<ConversationState>().FirstOrDefault();
        if (conversationState == null)
        {
            throw new InvalidOperationException("ConversationState must be defined and added before adding conversation-scoped state accessors.");
        }

        var userState = options.State.OfType<UserState>().FirstOrDefault();
        if (userState == null)
        {
            throw new InvalidOperationException("UserState must be defined and added before adding user-scoped state accessors.");
        }

        // The dialogs will need a state store accessor. Creating it here once (on-demand) allows the dependency injection
        // to hand it to our IBot class that is create per-request.
        var accessors = new BotAccessors(conversationState, userState)
        {
            ConversationDialogState = conversationState.CreateProperty<DialogState>("DialogState"),
            UserProfile = userState.CreateProperty<UserProfile>("UserProfile"),
        };

        return accessors;
    });
```

<span data-ttu-id="94404-143">Después, en el código del bot, defina los siguientes objetos del conjunto de diálogos.</span><span class="sxs-lookup"><span data-stu-id="94404-143">Then, in your bot code, define the following objects for the dialog set.</span></span>

```cs
    private readonly BotAccessors _accessors;

    /// <summary>
    /// The <see cref="DialogSet"/> that contains all the Dialogs that can be used at runtime.
    /// </summary>
    private DialogSet _dialogs;

    /// <summary>
    /// Initializes a new instance of the <see cref="MultiTurnPromptsBot"/> class.
    /// </summary>
    /// <param name="accessors">A class containing <see cref="IStatePropertyAccessor{T}"/> used to manage state.</param>
    public MultiTurnPromptsBot(BotAccessors accessors)
    {
        _accessors = accessors ?? throw new ArgumentNullException(nameof(accessors));

        // The DialogSet needs a DialogState accessor, it will call it when it has a turn context.
        _dialogs = new DialogSet(accessors.ConversationDialogState);

        // ...
        // other constructor items
        // ...
    }
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="94404-144">JavaScript</span><span class="sxs-lookup"><span data-stu-id="94404-144">JavaScript</span></span>](#tab/javascript)

<span data-ttu-id="94404-145">Instale el paquete dialogs desde NPM:</span><span class="sxs-lookup"><span data-stu-id="94404-145">Install the dialogs package from NPM:</span></span>

```cmd
npm install --save botbuilder-dialogs
```

<span data-ttu-id="94404-146">Para usar **dialogs** en el bot, inclúyalo en el código del bot.</span><span class="sxs-lookup"><span data-stu-id="94404-146">To use **dialogs** in your bot, include it in the bot code.</span></span>

<span data-ttu-id="94404-147">En el archivo app.js, agregue lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="94404-147">In the app.js file, add the following.</span></span>

```javascript
// Import components from the dialogs library.
const { DialogSet } = require("botbuilder-dialogs");
// Import components from the main Bot Builder library.
const { ConversationState, MemoryStorage } = require('botbuilder');

// Set up a memory storage system to store information.
const storage = new MemoryStorage();
// We'll use ConversationState to track the state of the dialogs.
const conversationState = new ConversationState(storage);
// Create a property used to track state.
const dialogState = conversationState.createProperty('dialogState');

// Create a dialog set to control our prompts, store the state in dialogState
const dialogs = new DialogSet(dialogState);
```

---

## <a name="prompt-the-user"></a><span data-ttu-id="94404-148">Preguntar al usuario</span><span class="sxs-lookup"><span data-stu-id="94404-148">Prompt the user</span></span>

<span data-ttu-id="94404-149">Para solicitar información al usuario, defina una pregunta mediante una de las clases integradas, como **TextPrompt**, agréguela al conjunto de diálogos y asígnele un identificador de diálogo.</span><span class="sxs-lookup"><span data-stu-id="94404-149">To prompt a user for input, define a prompt using one of the built-in classes like **TextPrompt**, then add it to your dialog set and assign it a dialog ID.</span></span>

<span data-ttu-id="94404-150">Una vez que se agrega una pregunta, úsela en un dialogo en cascada de dos pasos (un diálogo en *cascada* es una manera de definir una secuencia de pasos).</span><span class="sxs-lookup"><span data-stu-id="94404-150">Once a prompt is added, use it in a two-step waterfall dialog, A *waterfall* dialog is a way to define a sequence of steps.</span></span> <span data-ttu-id="94404-151">Se pueden encadenar varias preguntas para crear conversaciones con varios pasos.</span><span class="sxs-lookup"><span data-stu-id="94404-151">Multiple prompts can be chained together to create multi-step conversations.</span></span> <span data-ttu-id="94404-152">Para más información, consulte la sección [Uso de diálogos](bot-builder-dialog-manage-conversation-flow.md#using-dialogs-to-guide-the-user-through-steps) en [Uso de diálogos para administrar un flujo de conversación simple](bot-builder-dialog-manage-conversation-flow.md).</span><span class="sxs-lookup"><span data-stu-id="94404-152">For more information, see the [using dialogs](bot-builder-dialog-manage-conversation-flow.md#using-dialogs-to-guide-the-user-through-steps) section of [manage simple conversation flow with dialogs](bot-builder-dialog-manage-conversation-flow.md).</span></span>

<span data-ttu-id="94404-153">Por ejemplo, el siguiente diálogo pregunta el nombre al usuario y usa la respuesta para saludarle.</span><span class="sxs-lookup"><span data-stu-id="94404-153">For example, the following dialog prompts the user for their name, and then uses the response to greet them.</span></span> <span data-ttu-id="94404-154">En el primer turno, el diálogo pregunta el nombre al usuario.</span><span class="sxs-lookup"><span data-stu-id="94404-154">On the first turn, the dialog prompts the user for their name.</span></span> <span data-ttu-id="94404-155">La respuesta del usuario se pasa como parámetro a la función del segundo paso, que procesa la información recibida y envía el saludo personalizado.</span><span class="sxs-lookup"><span data-stu-id="94404-155">The user's response is passed as a parameter to the second step function, which processes the input and sends the personalized greeting.</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="94404-156">C#</span><span class="sxs-lookup"><span data-stu-id="94404-156">C#</span></span>](#tab/csharp)

<span data-ttu-id="94404-157">Cada pregunta que se usa en el cuadro de diálogo también tiene un nombre, que el cuadro de diálogo usa en el bot para acceder a la pregunta.</span><span class="sxs-lookup"><span data-stu-id="94404-157">Each prompt you use in your dialog is also given a name, used by the dialog or your bot to access the prompt.</span></span> <span data-ttu-id="94404-158">En todos estos ejemplos, los identificadores de pregunta se exponen como constantes.</span><span class="sxs-lookup"><span data-stu-id="94404-158">In all of these samples, we are exposing the prompt IDs as constants.</span></span>

<span data-ttu-id="94404-159">En el constructor del bot, agregue las definiciones de la cascada de dos pasos y la pregunta que debe usar el diálogo.</span><span class="sxs-lookup"><span data-stu-id="94404-159">Within your bot constructor, add definitions for your two step waterfall and the prompt for the dialog to use.</span></span> <span data-ttu-id="94404-160">Aquí se van a agregar como funciones independientes, pero si se prefiere, se pueden definir como un elemento lambda insertado.</span><span class="sxs-lookup"><span data-stu-id="94404-160">Here we're adding them as independent functions, but they can be defined as an inline lambda if preferred.</span></span>

```csharp
 public MultiTurnPromptsBot(BotAccessors accessors)
{
    _accessors = accessors ?? throw new ArgumentNullException(nameof(accessors));

    // The DialogSet needs a DialogState accessor, it will call it when it has a turn context.
    _dialogs = new DialogSet(accessors.ConversationDialogState);

    // This array defines how the Waterfall will execute.
    var waterfallSteps = new WaterfallStep[]
    {
        NameStepAsync,
        SayHiAsync,
    };

    _dialogs.Add(new WaterfallDialog("details", waterfallSteps));
    _dialogs.Add(new TextPrompt("name"));
}
```

<span data-ttu-id="94404-161">Después, defina los dos pasos de la cascada en el bot.</span><span class="sxs-lookup"><span data-stu-id="94404-161">Then, define your two waterfall steps within your bot.</span></span> <span data-ttu-id="94404-162">Para el mensaje de texto va a especificar el identificador del *nombre* del elemento `TextPrompt` que ha definido anteriormente.</span><span class="sxs-lookup"><span data-stu-id="94404-162">For the text prompt, you're specifying the *name* ID of the `TextPrompt` you defined above.</span></span> <span data-ttu-id="94404-163">Observe que los nombres de método coinciden con los de `WaterfallStep[]`.</span><span class="sxs-lookup"><span data-stu-id="94404-163">Notice the method names match those of the `WaterfallStep[]` above.</span></span> <span data-ttu-id="94404-164">Los próximos ejemplos no incluirán dicho código, pero si desea incorporar más tendrá que agregar el nombre del método en el orden correcto en `WaterfallStep[]`.</span><span class="sxs-lookup"><span data-stu-id="94404-164">Future samples here won't include that code, but know that for additional steps you need to add the method name in the correct order in that `WaterfallStep[]`.</span></span>

```cs
    private static async Task<DialogTurnResult> NameStepAsync(WaterfallStepContext stepContext, CancellationToken cancellationToken)
    {
        // WaterfallStep always finishes with the end of the Waterfall or with another dialog; here it is a Prompt Dialog.
        // Running a prompt here means the next WaterfallStep will be run when the users response is received.
        return await stepContext.PromptAsync("name", new PromptOptions { Prompt = MessageFactory.Text("Please enter your name.") }, cancellationToken);
    }

    private static async Task<DialogTurnResult> SayHiAsync(WaterfallStepContext stepContext, CancellationToken cancellationToken)
    {
        await stepContext.Context.SendActivityAsync($"Hi {stepContext.Result}");

        return await stepContext.EndDialogAsync(cancellationToken);
    }
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="94404-165">JavaScript</span><span class="sxs-lookup"><span data-stu-id="94404-165">JavaScript</span></span>](#tab/javascript)

<span data-ttu-id="94404-166">Importe la clase TextPrompt en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="94404-166">Import the TextPrompt class into your app.</span></span>

```javascript
const { TextPrompt } = require("botbuilder-dialogs");
```

<span data-ttu-id="94404-167">Cree la nueva pregunta y agréguela al conjunto de diálogos.</span><span class="sxs-lookup"><span data-stu-id="94404-167">Create the new prompt, and add it to the dialog set.</span></span>

```javascript
// Greet user:
// Ask for the user name and then greet them by name.
dialogs.add(new TextPrompt('textPrompt'));
dialogs.add('greetings', [
    async function (step){
        // the results of this prompt will be passed to the next step
        return await step.prompt('textPrompt', 'What is your name?');
    },
    async function(step) {
        // step.result is the result of the prompt defined above
        const userName = step.result;
        await step.context.sendActivity(`Hi ${userName}!`);
        return await step.endDialog();
    }
]);
```

---

> [!NOTE]
> <span data-ttu-id="94404-168">Para iniciar un cuadro de diálogo, obtenga un contexto de cuadro de diálogo y use su método _begin_.</span><span class="sxs-lookup"><span data-stu-id="94404-168">To start a dialog, get a dialog context, and use its _begin_ method.</span></span> <span data-ttu-id="94404-169">Para más información, consulte [Uso de diálogos para administrar un flujo de conversación simple](./bot-builder-dialog-manage-conversation-flow.md).</span><span class="sxs-lookup"><span data-stu-id="94404-169">For more information, see [use dialogs to manage simple conversation flow](./bot-builder-dialog-manage-conversation-flow.md).</span></span>

## <a name="reusable-prompts"></a><span data-ttu-id="94404-170">Preguntas reutilizables</span><span class="sxs-lookup"><span data-stu-id="94404-170">Reusable prompts</span></span>

<span data-ttu-id="94404-171">Un mensaje se puede reutilizar para formular distintas preguntas, siempre y cuando las respuestas sean del mismo tipo.</span><span class="sxs-lookup"><span data-stu-id="94404-171">A prompt can be reused to ask different questions as long as the answers are the same type.</span></span> <span data-ttu-id="94404-172">Por ejemplo, en el código de ejemplo anterior se definía una pregunta de texto y se usaba para preguntarle el nombre al usuario.</span><span class="sxs-lookup"><span data-stu-id="94404-172">For example, the sample code above defined a text prompt and used it to ask the user for their name.</span></span> <span data-ttu-id="94404-173">También puede usar esa misma pregunta para pedir al usuario otra cadena de texto, como "Where do you work?" (¿Dónde trabajas?).</span><span class="sxs-lookup"><span data-stu-id="94404-173">You can also use that same prompt to ask the user for another text string such as, "Where do you work?".</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="94404-174">C#</span><span class="sxs-lookup"><span data-stu-id="94404-174">C#</span></span>](#tab/csharp)

<span data-ttu-id="94404-175">En el ejemplo, el identificador de nuestro mensaje de texto, *name*, no aumenta la claridad del código.</span><span class="sxs-lookup"><span data-stu-id="94404-175">In the example, the ID for our text prompt, *name*, isn't helpful for code clarity.</span></span> <span data-ttu-id="94404-176">Sin embargo, es un buen ejemplo que el identificador del mensaje puede lo que desee.</span><span class="sxs-lookup"><span data-stu-id="94404-176">However, it is a good example that your prompt ID can be whatever you choose.</span></span>

<span data-ttu-id="94404-177">Ahora, nuestros métodos incluyen un tercer paso, en el que se pregunta dónde trabaja el usuario.</span><span class="sxs-lookup"><span data-stu-id="94404-177">Now, our methods include a third step to ask where our user works.</span></span>

```cs
    private static async Task<DialogTurnResult> NameStepAsync(WaterfallStepContext stepContext, CancellationToken cancellationToken)
    {
        // WaterfallStep always finishes with the end of the Waterfall or with another dialog; here it is a Prompt Dialog.
        // Running a prompt here means the next WaterfallStep will be run when the users response is received.
        return await stepContext.PromptAsync("name", new PromptOptions { Prompt = MessageFactory.Text("Please enter your name.") }, cancellationToken);
    }

    private static async Task<DialogTurnResult> WorkAsync(WaterfallStepContext stepContext, CancellationToken cancellationToken)
    {
        await stepContext.Context.SendActivityAsync($"Hi {stepContext.Result}!");

        return await stepContext.PromptAsync("name", new PromptOptions { Prompt = MessageFactory.Text("Where do you work?") }, cancellationToken);
    }

    private static async Task<DialogTurnResult> SayHiAsync(WaterfallStepContext stepContext, CancellationToken cancellationToken)
    {
        await stepContext.Context.SendActivityAsync($"{stepContext.Result} is a cool place!");

        return await stepContext.EndDialogAsync(cancellationToken);
    }
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="94404-178">JavaScript</span><span class="sxs-lookup"><span data-stu-id="94404-178">JavaScript</span></span>](#tab/javascript)

```javascript
// Greet user:
// Ask for the user name and then greet them by name.
// Ask them where they work.
dialogs.add(new TextPrompt('textPrompt'));
dialogs.add('greetings',[
    async function (step){
        // Use the textPrompt to ask for a name.
        return await step.prompt('textPrompt', 'What is your name?');
    },
    async function (step){
        const userName = step.result;
        await step.context.sendActivity(`Hi ${ userName }!`);

        // Now, reuse the same prompt to ask them where they work.
        return await step.prompt('textPrompt', 'Where do you work?');
    },
    async function(step) {
        const workPlace = step.result;
        await step.context.sendActivity(`${ workPlace } is a cool place!`);

        return await step.endDialog();
    }
]);
```

---

<span data-ttu-id="94404-179">Si tiene que utilizar varios mensajes diferentes, asigne un *dialogId* a cada uno.</span><span class="sxs-lookup"><span data-stu-id="94404-179">If you need to use multiple different prompts, give each prompt a unique *dialogId*.</span></span> <span data-ttu-id="94404-180">Cada diálogo o mensaje que se agrega a un conjunto de diálogos necesita un identificador único.</span><span class="sxs-lookup"><span data-stu-id="94404-180">Each dialog or prompt added to a dialog set needs a unique ID.</span></span> <span data-ttu-id="94404-181">También puede crear diálogos con varios **mensajes** del mismo tipo.</span><span class="sxs-lookup"><span data-stu-id="94404-181">You can also create multiple **prompt** dialogs of the same type.</span></span> <span data-ttu-id="94404-182">Por ejemplo, se podrían crear dos cuadros de diálogo **TextPrompt** para el ejemplo anterior:</span><span class="sxs-lookup"><span data-stu-id="94404-182">For example, you could create two **TextPrompt** dialogs for the example above:</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="94404-183">C#</span><span class="sxs-lookup"><span data-stu-id="94404-183">C#</span></span>](#tab/csharp)

```cs
_dialogs.Add(new WaterfallDialog("details", waterfallSteps));
_dialogs.Add(new TextPrompt("name"));
_dialogs.Add(new TextPrompt("workplace"));
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="94404-184">JavaScript</span><span class="sxs-lookup"><span data-stu-id="94404-184">JavaScript</span></span>](#tab/javascript)

```javascript
dialogs.add(new TextPrompt('namePrompt'));
dialogs.add(new TextPrompt('workPlacePrompt'));
```

---

<span data-ttu-id="94404-185">Para facilitar la reutilización del código, la definición de un solo elemento `TextPrompt` valdría para estas preguntas, ya que todas ellas esperan una cadena de texto como respuesta.</span><span class="sxs-lookup"><span data-stu-id="94404-185">For the sake of code reusability, defining a single `TextPrompt` would work for all these prompts because they all expect a text as a response.</span></span> <span data-ttu-id="94404-186">La capacidad de asignar nombres a los diálogos resulta útil cuando hay que aplicar distintas reglas de validación a la entrada de las preguntas.</span><span class="sxs-lookup"><span data-stu-id="94404-186">The ability to name dialogs comes in handy when you need to apply different validation rules to the input of the prompts.</span></span> <span data-ttu-id="94404-187">Veamos cómo se pueden validar las respuestas mediante `NumberPrompt`.</span><span class="sxs-lookup"><span data-stu-id="94404-187">Let's take a look at how you can validate prompt responses using a `NumberPrompt`.</span></span>

## <a name="specify-prompt-options"></a><span data-ttu-id="94404-188">Especificar las opciones de las preguntas</span><span class="sxs-lookup"><span data-stu-id="94404-188">Specify prompt options</span></span>

<span data-ttu-id="94404-189">Cuando se usa una pregunta en un paso de cuadro de diálogo, también se pueden proporcionar opciones de pregunta, como una cadena de nueva pregunta.</span><span class="sxs-lookup"><span data-stu-id="94404-189">When you use a prompt within a dialog step, you can also provide prompt options, such as a reprompt string.</span></span>

<span data-ttu-id="94404-190">Especificar una cadena de nueva pregunta es útil cuando la entrada del usuario no puede satisfacer una pregunta, ya sea porque está en un formato que no se puede analizar, como "mañana" para una pregunta de símbolo, o bien porque la entrada no cumpla un criterio de validación.</span><span class="sxs-lookup"><span data-stu-id="94404-190">Specifying a reprompt string is useful when user input can fail to satisfy a prompt, either because it is in a format that the prompt can not parse, such as "tomorrow" for a number prompt, or the input fails a validation criteria.</span></span> <span data-ttu-id="94404-191">La pregunta de número puede interpretar una amplia variedad de entradas, "doce" o "un cuarto", así como "12" y "0,25".</span><span class="sxs-lookup"><span data-stu-id="94404-191">The number prompt can interpret a wide variety of input, such as "twelve" or "one quarter", as well as "12" and "0.25".</span></span>

<span data-ttu-id="94404-192">El local es un parámetro opcional de determinados mensajes, como **NumberPrompt**.</span><span class="sxs-lookup"><span data-stu-id="94404-192">The local is an optional parameter on certain prompts, like **NumberPrompt**.</span></span> <span data-ttu-id="94404-193">Puede facilitar al mensaje a analizar la entrada con mayor precisión, pero no es obligatorio.</span><span class="sxs-lookup"><span data-stu-id="94404-193">This can help the prompt parse input more accurately, but is not required.</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="94404-194">C#</span><span class="sxs-lookup"><span data-stu-id="94404-194">C#</span></span>](#tab/csharp)

<span data-ttu-id="94404-195">En el código siguiente se agrega una pregunta numérica a un conjunto de diálogos existente, **_dialogs**.</span><span class="sxs-lookup"><span data-stu-id="94404-195">The following code would add a number prompt to an existing dialog set, **_dialogs**.</span></span>

```csharp
_dialogs.Add(new NumberPrompt<int>("age"));
```

<span data-ttu-id="94404-196">En un paso de cuadro de diálogo, el código siguiente podría solicitar entradas al usuario y proporcionar una cadena de nueva pregunta para usarla en caso de que la entrada no se pudiera interpretar como un número.</span><span class="sxs-lookup"><span data-stu-id="94404-196">Within a dialog step, the following code would prompt the user for input and provide a reprompt string to use if their input cannot be interpreted as a number.</span></span>

```csharp
return await stepContext.PromptAsync(
    "age",
    new PromptOptions {
        Prompt = MessageFactory.Text("Please enter your age."),
        RetryPrompt = MessageFactory.Text("I didn't get that. Please enter a valid age."),
    },
    cancellationToken);
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="94404-197">JavaScript</span><span class="sxs-lookup"><span data-stu-id="94404-197">JavaScript</span></span>](#tab/javascript)

```javascript
// Import the NumberPrompt class from the dialog library.
const { NumberPrompt } = require("botbuilder-dialogs");

// Add a NumberPrompt to our dialog set and give it the ID numberPrompt.
dialogs.add(new NumberPrompt('numberPrompt'));

// Call the numberPrompt dialog with the (optional) retryPrompt parameter.
await dc.prompt('numberPrompt', 'How many people in your party?', { retryPrompt: `Sorry, please specify the number of people in your party.` })
```

---

<span data-ttu-id="94404-198">El mensaje acerca de las opciones tiene un parámetro adicional necesario: la lista de opciones entre las que puede elegir el usuario.</span><span class="sxs-lookup"><span data-stu-id="94404-198">The choice prompt has an additional required parameter: the list of choices available to the user.</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="94404-199">C#</span><span class="sxs-lookup"><span data-stu-id="94404-199">C#</span></span>](#tab/csharp)

<span data-ttu-id="94404-200">Si se usa **ChoicePrompt** para pedir al usuario que elija entre un conjunto de opciones, es preciso proporcionar el mensaje con dicho conjunto, que se incluye en un objeto **PromptOptions**.</span><span class="sxs-lookup"><span data-stu-id="94404-200">When we use the **ChoicePrompt** to ask the user to choose between a set of options, we have to provide the prompt with that set of options, provided within a **PromptOptions** object.</span></span> <span data-ttu-id="94404-201">En este caso, se usa **ChoiceFactory** para convertir una lista de opciones en un formato adecuado.</span><span class="sxs-lookup"><span data-stu-id="94404-201">Here, we use the **ChoiceFactory** to convert a list of options into an appropriate format.</span></span>

```csharp
private static async Task<DialogTurnResult> FavoriteColorAsync(WaterfallStepContext stepContext, CancellationToken cancellationToken)
{
    await stepContext.Context.SendActivityAsync($"Hi {stepContext.Result}!");

    return await stepContext.PromptAsync(
        "color",
        new PromptOptions {
            Prompt = MessageFactory.Text("What's your favorite color?"),
            Choices = ChoiceFactory.ToChoices(new List<string> { "blue", "green", "red" }),
        },
        cancellationToken);
}
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="94404-202">JavaScript</span><span class="sxs-lookup"><span data-stu-id="94404-202">JavaScript</span></span>](#tab/javascript)

```javascript
// Import the ChoicePrompt class into your app from the dialogs library.
const { ChoicePrompt } = require("botbuilder-dialogs");
```

```javascript
// Add a ChoicePrompt to the dialog set and give it an ID of choicePrompt.
dialogs.add(new ChoicePrompt('choicePrompt'));
```

```javascript
// Call the choicePrompt into action, passing in an array of options.
const list = ['green', 'blue', 'red', 'yellow'];
await dc.prompt('choicePrompt', 'Please make a choice', list, { retryPrompt: 'Please choose a color.' });
```

---

## <a name="validate-a-prompt-response"></a><span data-ttu-id="94404-203">Validar la respuesta de una pregunta</span><span class="sxs-lookup"><span data-stu-id="94404-203">Validate a prompt response</span></span>

<span data-ttu-id="94404-204">Puede validar una respuesta antes de devolver el valor al siguiente paso de la **cascada**.</span><span class="sxs-lookup"><span data-stu-id="94404-204">You can validate a prompt response before returning the value to the next step of the **waterfall**.</span></span> <span data-ttu-id="94404-205">Por ejemplo, para validar **NumberPrompt** en un intervalo de números entre **6** y **20**, debería incluir una función de validación similar a la siguiente:</span><span class="sxs-lookup"><span data-stu-id="94404-205">For example, to validate a **NumberPrompt** within a range of numbers between **6** and **20**, you would include a validation function similar to this:</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="94404-206">C#</span><span class="sxs-lookup"><span data-stu-id="94404-206">C#</span></span>](#tab/csharp)

<span data-ttu-id="94404-207">Cambie cuando el mensaje se agregue al conjunto de diálogos para incluir la función de validador</span><span class="sxs-lookup"><span data-stu-id="94404-207">Change when the prompt is added to your dialog set to include the validator function</span></span>

```cs
_dialogs.Add(new NumberPrompt<int>("partySize", PartySizeValidatorAsync));
```

<span data-ttu-id="94404-208">A partir de ese momento, la validación se define como su propio método e indica true o false en función de que haya pasado la validación, o no.</span><span class="sxs-lookup"><span data-stu-id="94404-208">Validation is then defined as its own method, indicating true or false depending on if it passed the validation.</span></span> <span data-ttu-id="94404-209">Si se devuelve false, volverá a preguntar al usuario.</span><span class="sxs-lookup"><span data-stu-id="94404-209">If it gets a false returned, it will reprompt the user.</span></span>

```cs
private Task<bool> PartySizeValidatorAsync(PromptValidatorContext<int> promptContext, CancellationToken cancellationToken)
{
    var result = promptContext.Recognized.Value;

    if (result < 6 || result > 20)
    {
        return Task.FromResult(false);
    }

    return Task.FromResult(true);
}
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="94404-210">JavaScript</span><span class="sxs-lookup"><span data-stu-id="94404-210">JavaScript</span></span>](#tab/javascript)

```javascript
// Customized prompts with validations
// A number prompt with validation for valid party size within a range.
dialogs.add(new NumberPrompt('partySizePrompt', async (promptContext) => {
    // Check to make sure a value was recognized.
    if (promptContext.recognized.succeeded) {
        const value = promptContext.recognized.value;
        try {
            if (value < 6 ) {
                throw new Error('Party size too small.');
            } else if (value > 20) {
                throw new Error('Party size too big.')
            } else {
                return true; // Indicate that this is a valid value.
            }
        } catch (err) {
            await promptContext.context.sendActivity(`${ err.message } <br/>Please provide a valid number between 6 and 20.`);
            return false; // Indicate that this is invalid.
        }
    } else {
        return false;
    }
}));
```

---

<span data-ttu-id="94404-211">Del mismo modo, si quiere validar una respuesta **DatetimePrompt** para una fecha y hora en el futuro, puede tener lógica de validación similar a la siguiente:</span><span class="sxs-lookup"><span data-stu-id="94404-211">Likewise, if you want to validate a **DatetimePrompt** response for a date and time in the future, you can have validation logic similar to this:</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="94404-212">C#</span><span class="sxs-lookup"><span data-stu-id="94404-212">C#</span></span>](#tab/csharp)

```cs
    private Task<bool> DateTimeValidatorAsync(PromptValidatorContext<IList<DateTimeResolution>> prompt, CancellationToken cancellationToken)
    {
        if (prompt.Recognized.Succeeded)
        {
            var resolution = prompt.Recognized.Value.First();

            // Verify that the Timex received is within the desired bounds, compared to today.
            var now = DateTime.Now;
            DateTime.TryParse(resolution.Value, out var time);

            if (time < now)
            {
                return Task.FromResult(false);
            }

            return Task.FromResult(true);
        }

        return Task.FromResult(false);
    }
```

```csharp
_dialogs.Add(new DateTimePrompt("date", DateTimeValidatorAsync));
```

<span data-ttu-id="94404-213">Encontrará más ejemplos en el [repositorio de ejemplos](https://aka.ms/bot-samples-readme).</span><span class="sxs-lookup"><span data-stu-id="94404-213">Further examples can be found in our [samples repo](https://aka.ms/bot-samples-readme).</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="94404-214">JavaScript</span><span class="sxs-lookup"><span data-stu-id="94404-214">JavaScript</span></span>](#tab/javascript)

```JavaScript
// A date and time prompt with validation for date/time in the future.
dialogs.add(new atetimePrompt('dateTimePrompt', async (promptContext) => {
    if (promptContext.recognized.succeeded) {
        const values = promptContext.recognized.value;
        try {
            if (values.length < 0) { throw new Error('missing time') }
            if (values[0].type !== 'date') { throw new Error('unsupported type') }
            const value = new Date(values[0].value);
            if (value.getTime() < new Date().getTime()) { throw new Error('in the past') }

            // update the return value of the prompt to be a real date object
            promptContext.recognized.value = value;
            return true; // indicate valid 
        } catch (err) {
            await promptContext.context.sendActivity(`Please enter a valid time in the future like "tomorrow at 9am".`);
            return false; // indicate invalid
        }
    } else {
        return false;
    }
}));
```

<span data-ttu-id="94404-215">Encontrará más ejemplos en el [repositorio de ejemplos](https://aka.ms/bot-samples-readme).</span><span class="sxs-lookup"><span data-stu-id="94404-215">Further examples can be found in our [samples repo](https://aka.ms/bot-samples-readme).</span></span>

---

> [!TIP]
> <span data-ttu-id="94404-216">Si el usuario proporciona una respuesta ambigua a las preguntas de fecha y hora, estas se pueden resolver en varias fechas diferentes.</span><span class="sxs-lookup"><span data-stu-id="94404-216">Date time prompts can resolve to a few different dates if the user gives an ambiguous answer.</span></span> <span data-ttu-id="94404-217">En función del uso previsto, puede que le interese comprobar todas las resoluciones proporcionadas por el resultado de la pregunta, en lugar de simplemente la primera.</span><span class="sxs-lookup"><span data-stu-id="94404-217">Depending on what you're using it for, you may want to check all the resolutions provided by the prompt result, instead of just the first.</span></span>

<span data-ttu-id="94404-218">Se pueden usar técnicas similares para validar las respuestas para cualquiera de los tipos de pregunta.</span><span class="sxs-lookup"><span data-stu-id="94404-218">You can use the similar techniques to validate prompt responses for any of the prompt types.</span></span>

## <a name="save-user-data"></a><span data-ttu-id="94404-219">Guardar los datos del usuario</span><span class="sxs-lookup"><span data-stu-id="94404-219">Save user data</span></span>

<span data-ttu-id="94404-220">Al solicitar entradas al usuario, tiene varias opciones sobre cómo controlarlas.</span><span class="sxs-lookup"><span data-stu-id="94404-220">When you prompt for user input, you have several options on how to handle that input.</span></span> <span data-ttu-id="94404-221">Por ejemplo, puede consumir y descartar la entrada, guardarla en una variable global, guardarla en un contenedor de almacenamiento volátil o en memoria, guardarla en un archivo, o bien en una base de datos externa.</span><span class="sxs-lookup"><span data-stu-id="94404-221">For instance, you can consume and discard the input, you can save it to a global variable, you can save it to a volatile or in-memory storage container, you can save it to a file, or you can save it to an external database.</span></span> <span data-ttu-id="94404-222">Para obtener más información sobre cómo guardar los datos del usuario, vea [Administración de los datos del usuario](bot-builder-howto-v4-state.md).</span><span class="sxs-lookup"><span data-stu-id="94404-222">For more information on how to save user data, see [Manage user data](bot-builder-howto-v4-state.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="94404-223">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="94404-223">Additional resources</span></span>

<span data-ttu-id="94404-224">Para ver un ejemplo completo del uso de algunos de estos mensajes, vea el bot de mensajes para varios turnos para [C#](https://aka.ms/cs-multi-prompts-sample) o [JavaScript](https://aka.ms/js-multi-prompts-sample).</span><span class="sxs-lookup"><span data-stu-id="94404-224">For a complete sample using some of these prompts, see the the Multi Turn Prompt Bot for [C#](https://aka.ms/cs-multi-prompts-sample) or [JavaScript](https://aka.ms/js-multi-prompts-sample).</span></span>

## <a name="next-steps"></a><span data-ttu-id="94404-225">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="94404-225">Next steps</span></span>

<span data-ttu-id="94404-226">Ahora que sabe cómo solicitar entradas al usuario, vamos a mejorar la experiencia de usuario y el código de bot mediante la administración de varios flujos de conversación a través de cuadros de diálogo.</span><span class="sxs-lookup"><span data-stu-id="94404-226">Now that you know how to prompt a user for input, lets enhance the bot code and user experience by managing various conversation flows through dialogs.</span></span>
