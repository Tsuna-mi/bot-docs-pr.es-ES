---
title: Administración de un flujo de conversación con avisos primitivos | Microsoft Docs
description: Aprenda a administrar un flujo de conversación con avisos primitivos en el SDK de Bot Builder.
keywords: flujo de conversación, avisos
author: v-ducvo
ms.author: v-ducvo
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 7/20/2018
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: 3014e6cd8b18ab44ff343373a034c392e44bca1d
ms.sourcegitcommit: 3bf3dbb1a440b3d83e58499c6a2ac116fe04b2f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/23/2018
ms.locfileid: "46707371"
---
# <a name="prompt-users-for-input-using-your-own-prompts"></a><span data-ttu-id="32a70-104">Petición de datos de entrada a los usuarios mediante sus propios mensajes</span><span class="sxs-lookup"><span data-stu-id="32a70-104">Prompt users for input using your own prompts</span></span>

[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]

<span data-ttu-id="32a70-105">A menudo, una conversación entre un usuario y un bot implica mostrar preguntas (avisos) al usuario para obtener información, analizar la respuesta del usuario y, a continuación, actuar en dicha información.</span><span class="sxs-lookup"><span data-stu-id="32a70-105">A conversation between a bot and a user often involves asking (prompting) the user for information, parsing the user's response, and then acting on that information.</span></span> <span data-ttu-id="32a70-106">En el tema sobre cómo [preguntar a los usuarios mediante la biblioteca Dialogs](bot-builder-prompts.md), se detalla cómo pedir datos de entrada a los usuarios mediante la biblioteca **Dialogs**.</span><span class="sxs-lookup"><span data-stu-id="32a70-106">The topic about [prompting users using the Dialogs library](bot-builder-prompts.md) details how to prompt users for input using the **Dialogs** library.</span></span> <span data-ttu-id="32a70-107">Entre otras cosas, la biblioteca **Dialogs** se encarga del seguimiento de la conversación actual y de la pregunta que se está planteando en cada momento.</span><span class="sxs-lookup"><span data-stu-id="32a70-107">Among other things, the **Dialogs** library takes care of tracking the current conversation and the current question being asked.</span></span> <span data-ttu-id="32a70-108">También proporciona métodos para solicitar y validar diferentes tipos de información como texto, números, fecha y hora, etc.</span><span class="sxs-lookup"><span data-stu-id="32a70-108">It also provides methods for requesting and validating different types of information such as text, number, date and time, and so on.</span></span>

<span data-ttu-id="32a70-109">En determinadas situaciones, puede que la biblioteca integrada **Dialogs** no sea ser la solución adecuada para su bot; **Dialogs** podría agregar demasiada sobrecarga para bots simples, ser demasiado rígida o, por el contrario, no conseguir lo que su bot necesita hacer.</span><span class="sxs-lookup"><span data-stu-id="32a70-109">In certain situations, the built in **Dialogs** may not be the right solution for your bot; **Dialogs** might add too much overhead for simple bots, be too rigid, or otherwise not achieve what you need your bot to do.</span></span> <span data-ttu-id="32a70-110">En esos casos, puede omitir la biblioteca e implementar su propia lógica de preguntas.</span><span class="sxs-lookup"><span data-stu-id="32a70-110">In those cases, you can skip the library and implement your own prompting logic.</span></span> <span data-ttu-id="32a70-111">En este tema se mostrará cómo configurar un *bot Echo* básico para que pueda administrar la conversación con sus propios avisos.</span><span class="sxs-lookup"><span data-stu-id="32a70-111">This topic will show you how to setup your basic *Echo bot* so that you can manage conversation using your own prompts.</span></span>

## <a name="track-prompt-states"></a><span data-ttu-id="32a70-112">Seguimiento del estado de los avisos</span><span class="sxs-lookup"><span data-stu-id="32a70-112">Track prompt states</span></span>

<span data-ttu-id="32a70-113">En una conversación controlada por avisos, necesita controlar dónde está en la conversación y qué pregunta se está haciendo en cada momento.</span><span class="sxs-lookup"><span data-stu-id="32a70-113">In a prompt-driven conversation, you need to track where in the conversation you currently are, and what question is currently being asked.</span></span> <span data-ttu-id="32a70-114">En el código, esto se traduce en administrar un par de marcas.</span><span class="sxs-lookup"><span data-stu-id="32a70-114">In code, this translates to managing a couple of flags.</span></span>

<span data-ttu-id="32a70-115">Por ejemplo, puede crear un par de propiedades que desea controlar.</span><span class="sxs-lookup"><span data-stu-id="32a70-115">For example, you can create a couple of properties you want to track.</span></span>

<span data-ttu-id="32a70-116">Estos estados realizan un seguimiento de qué tema y qué aviso están actualmente activados.</span><span class="sxs-lookup"><span data-stu-id="32a70-116">These states keep track of which topic and which prompt we are currently on.</span></span> <span data-ttu-id="32a70-117">Para asegurarse de que estas marcas funcionarán según lo esperado cuando se implementen en la nube, vamos a almacenarlas en el [estado de la conversación](bot-builder-howto-v4-state.md).</span><span class="sxs-lookup"><span data-stu-id="32a70-117">To ensure these flags function as expected when deployed to the cloud, we store them in the [conversation state](bot-builder-howto-v4-state.md).</span></span> 

# <a name="ctabcsharp"></a>[<span data-ttu-id="32a70-118">C#</span><span class="sxs-lookup"><span data-stu-id="32a70-118">C#</span></span>](#tab/csharp)

<span data-ttu-id="32a70-119">Vamos a crear dos clases para realizar el seguimiento del estado.</span><span class="sxs-lookup"><span data-stu-id="32a70-119">We are creating two classes to track state.</span></span> <span data-ttu-id="32a70-120">**TopicState** para realizar un seguimiento del progreso de los avisos de la conversación y **UserProfile** para realizar el seguimiento de la información que proporciona el usuario.</span><span class="sxs-lookup"><span data-stu-id="32a70-120">**TopicState** to track the progress of the conversational prompts, and **UserProfile** to track the information the user provides.</span></span> <span data-ttu-id="32a70-121">Esta información se va a almacenar en el [estado](bot-builder-howto-v4-state.md) del bot en un paso posterior.</span><span class="sxs-lookup"><span data-stu-id="32a70-121">We'll be storing this information in our bot [state](bot-builder-howto-v4-state.md) in a later step.</span></span>

```csharp
/// <summary>
/// Contains conversation state information about the conversation in progress.
/// </summary>
public class TopicState
{
    /// <summary>
    /// Identifies the current "topic" of conversation.
    /// </summary>
    public string Topic { get; set; }

    /// <summary>
    /// Indicates whether we asked the user a question last turn, and
    /// if so, what it was.
    /// </summary>
    public string Prompt { get; set; }
}
```

```csharp
/// <summary>
/// Contains user state information for the user's profile.
/// </summary>
public class UserProfile
{
    public string UserName { get; set; }

    public int? Age { get; set; }

    public string WorkPlace { get; set; }
}
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="32a70-122">JavaScript</span><span class="sxs-lookup"><span data-stu-id="32a70-122">JavaScript</span></span>](#tab/javascript)

<span data-ttu-id="32a70-123">**app.js**</span><span class="sxs-lookup"><span data-stu-id="32a70-123">**app.js**</span></span>

```javascript
const storage = new MemoryStorage(); // Volatile memory
const conversationState = new ConversationState(storage);
const userState = new UserState(storage);
const dialogState = conversationState.createProperty('dialogState');
const userProfile = userState.createProperty('userProfile');
```

<span data-ttu-id="32a70-124">Coloque este código dentro de la lógica principal del bot.</span><span class="sxs-lookup"><span data-stu-id="32a70-124">Place this code within your main bot logic.</span></span>

```javascript
// Pull the state of the dialog out of the conversation state manager.
const convo = await dialogState.get(context, {
    prompt: undefined,
    topic: 'profile'
});

// Pull the user profile out of the user state manager.
const userProfile = await userProfile.get(context, {  // Define the user's profile object
        "userName": undefined,
        "age": undefined,
        "workPlace": undefined
    }
);
```

---

## <a name="manage-a-topic-of-conversation"></a><span data-ttu-id="32a70-125">Administración de un tema de conversación</span><span class="sxs-lookup"><span data-stu-id="32a70-125">Manage a topic of conversation</span></span>

<span data-ttu-id="32a70-126">En una conversación secuencial, como aquellas en las que se reúne información del usuario, necesita saber cuándo ha escrito el usuario la conversación y en qué parte de ella está.</span><span class="sxs-lookup"><span data-stu-id="32a70-126">In a sequential conversation, such as one where you gather information from the user, you need to know when the user has entered the conversation and where in the conversation the user is.</span></span> <span data-ttu-id="32a70-127">Puede realizar un seguimiento de esto en el controlador principal de turnos del bot. Para ello, puede establecer y comprobar las marcas del aviso definidas anteriormente y, a continuación, actuar en consecuencia.</span><span class="sxs-lookup"><span data-stu-id="32a70-127">You can track this in the main bot's turn handler by setting and checking the prompt flags defined above, then acting accordingly.</span></span> <span data-ttu-id="32a70-128">El ejemplo siguiente muestra cómo puede usar estas marcas para recopilar información del perfil del usuario a través de la conversación.</span><span class="sxs-lookup"><span data-stu-id="32a70-128">The sample below shows how you can use these flags to gather the user's profile information through the conversation.</span></span>

<span data-ttu-id="32a70-129">El código del bot se presenta aquí y se describe en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="32a70-129">The bot code is presented here, and discussed in the next section.</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="32a70-130">C#</span><span class="sxs-lookup"><span data-stu-id="32a70-130">C#</span></span>](#tab/csharp)

<span data-ttu-id="32a70-131">Para ASP.NET Core, debemos configurar el bot y la inyección de dependencias en primer lugar.</span><span class="sxs-lookup"><span data-stu-id="32a70-131">For ASP.NET Core, we need to setup our bot and dependency injection first.</span></span>

<span data-ttu-id="32a70-132">Cambie el nombre del archivo **EchoWithCounterBot.cs** a **PrimitivePromptsBot.cs** y actualice también el nombre de la clase.</span><span class="sxs-lookup"><span data-stu-id="32a70-132">Rename the file **EchoWithCounterBot.cs** to **PrimitivePromptsBot.cs**, and also update the class name.</span></span> <span data-ttu-id="32a70-133">Esta clase contiene la lógica del bot y volveremos para actualizarla en breve.</span><span class="sxs-lookup"><span data-stu-id="32a70-133">This class holds our bot logic, and we'll get back to updating this shortly.</span></span>

<span data-ttu-id="32a70-134">Cambie el nombre del archivo **EchoBotAccessors.cs** a **BotAccessors.cs** y actualice también el nombre de la clase.</span><span class="sxs-lookup"><span data-stu-id="32a70-134">Rename the file **EchoBotAccessors.cs** to **BotAccessors.cs**, and also update the class name.</span></span> <span data-ttu-id="32a70-135">Esta clase contiene los objetos de administración de estado y los descriptores de acceso de la propiedad de estado del bot.</span><span class="sxs-lookup"><span data-stu-id="32a70-135">This class holds the state management objects and the state property accessors for our bot.</span></span> <span data-ttu-id="32a70-136">Actualice la definición según lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="32a70-136">Update the definition to the following.</span></span>

```csharp
using Microsoft.Bot.Builder;
using System;

/// <summary>
/// Contains the state and state property accessors for the primitive prompts bot.
/// </summary>
public class BotAccessors
{
    public const string TopicStateName = "PrimitivePrompts.TopicStateAccessor";

    public const string UserProfileName = "PrimitivePrompts.UserProfileAccessor";

    public ConversationState ConversationState { get; }

    public UserState UserState { get; }

    public IStatePropertyAccessor<TopicState> TopicStateAccessor { get; set; }

    public IStatePropertyAccessor<UserProfile> UserProfileAccessor { get; set; }

    public BotAccessors(ConversationState conversationState, UserState userState)
    {
        if (conversationState is null)
        {
            throw new ArgumentNullException(nameof(conversationState));
        }

        if (userState is null)
        {
            throw new ArgumentNullException(nameof(userState));
        }

        this.ConversationState = conversationState;
        this.UserState = userState;
    }
}
```

<span data-ttu-id="32a70-137">En el archivo **Startup.cs**, actualice el método `ConfigureServices` a partir de donde se configura el objeto `IStorage`.</span><span class="sxs-lookup"><span data-stu-id="32a70-137">Update the **Startup.cs** file's `ConfigureServices` method, starting from where you set up the `IStorage` object.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddBot<PrimitivePromptsBot>(options =>
    {
        // ...

        // The Memory Storage used here is for local bot debugging only. When the bot
        // is restarted, everything stored in memory will be gone.
        IStorage dataStore = new MemoryStorage();

        var conversationState = new ConversationState(dataStore);
        options.State.Add(conversationState);

        var userState = new UserState(dataStore);
        options.State.Add(userState);
    });

    // Create and register state accessors.
    // Accessors created here are passed into the IBot-derived class on every turn.
    services.AddSingleton<BotAccessors>(sp =>
    {
        var options = sp.GetRequiredService<IOptions<BotFrameworkOptions>>().Value;
        var conversationState = options.State.OfType<ConversationState>().FirstOrDefault();
        var userState = options.State.OfType<UserState>().FirstOrDefault();

        // Create the custom state accessor.
        // State accessors enable other components to read and write individual properties of state.
        var accessors = new BotAccessors(conversationState, userState)
        {
            TopicStateAccessor = conversationState.CreateProperty<TopicState>(BotAccessors.TopicStateName),
            UserProfileAccessor = userState.CreateProperty<UserProfile>(BotAccessors.UserProfileName),
        };

        return accessors;
    });
}
```

<span data-ttu-id="32a70-138">Por último, actualice la lógica del bot en el archivo **PrimitivePromptsBot.cs**.</span><span class="sxs-lookup"><span data-stu-id="32a70-138">Finally, update the bot logic in your **PrimitivePromptsBot.cs** file.</span></span>

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.Bot.Builder;
using Microsoft.Bot.Schema;

public class PrimitivePromptsBot : IBot
{
    public const string ProfileTopic = "profile";

    /// <summary>
    /// Describes a field in the user profile.
    /// </summary>
    private class UserFieldInfo
    {
        /// <summary>
        /// The ID to use for this field.
        /// </summary>
        public string Key { get; set; }

        /// <summary>
        /// The prompt to use to ask for a value for this field.
        /// </summary>
        public string Prompt { get; set; }

        /// <summary>
        /// Gets the value of the corresponding field.
        /// </summary>
        public Func<UserProfile, string> GetValue { get; set; }

        /// <summary>
        /// Sets the value of the corresponding field.
        /// </summary>
        public Action<UserProfile, string> SetValue { get; set; }
    }

    /// <summary>
    /// The prompts for the user profile, indexed by field name.
    /// </summary>
    private static List<UserFieldInfo> UserFields { get; } = new List<UserFieldInfo>
    {
        new UserFieldInfo {
            Key = nameof(UserProfile.UserName),
            Prompt = "What is your name?",
            GetValue = (profile) => profile.UserName,
            SetValue = (profile, value) => profile.UserName = value,
        },
        new UserFieldInfo {
            Key = nameof(UserProfile.Age),
            Prompt = "How old are you?",
            GetValue = (profile) => profile.Age.HasValue? profile.Age.Value.ToString() : null,
            SetValue = (profile, value) =>
            {
                if (int.TryParse(value, out int age))
                {
                    profile.Age = age;
                }
            },
        },
        new UserFieldInfo {
            Key = nameof(UserProfile.WorkPlace),
            Prompt = "Where do you work?",
            GetValue = (profile) => profile.WorkPlace,
            SetValue = (profile, value) => profile.WorkPlace = value,
        },
    };

    /// <summary>
    /// The state and state accessors for the bot.
    /// </summary>
    private BotAccessors Accessors { get; }

    public PrimitivePromptsBot(BotAccessors accessors)
    {
        Accessors = accessors ?? throw new ArgumentNullException(nameof(accessors));
    }

    public async Task OnTurnAsync(ITurnContext turnContext, CancellationToken cancellationToken = default(CancellationToken))
    {
        if (turnContext.Activity.Type is ActivityTypes.Message)
        {
            // Use the state property accessors to get the topic state and user profile.
            TopicState topicState = await Accessors.TopicStateAccessor.GetAsync(
                turnContext,
                () => new TopicState { Topic = ProfileTopic, Prompt = null },
                cancellationToken);
            UserProfile userProfile = await Accessors.UserProfileAccessor.GetAsync(
                turnContext,
                () => new UserProfile(),
                cancellationToken);

            // Check whether we need more information.
            if (topicState.Topic is ProfileTopic)
            {
                // If we're expecting input, record it in the user's profile.
                if (topicState.Prompt != null)
                {
                    UserFieldInfo field = UserFields.First(f => f.Key.Equals(topicState.Prompt));
                    field.SetValue(userProfile, turnContext.Activity.Text.Trim());
                }

                // Determine which fields are not yet set.
                List<UserFieldInfo> emptyFields = UserFields.Where(f => f.GetValue(userProfile) is null).ToList();

                if (emptyFields.Any())
                {
                    // If all the fields are empty, send a welcome message.
                    if (emptyFields.Count == UserFields.Count)
                    {
                        await turnContext.SendActivityAsync("Welcome new user, please fill out your profile information.");
                    }

                    // We have at least one empty field. Prompt for the next empty field,
                    // and update the prompt flag to indicate which prompt we just sent,
                    // so that the response can be captured at the beginning of the next turn.
                    UserFieldInfo field = emptyFields.First();
                    await turnContext.SendActivityAsync(field.Prompt);
                    topicState.Prompt = field.Key;
                }
                else
                {
                    // Our user profile is complete!
                    await turnContext.SendActivityAsync($"Thank you, {userProfile.UserName}. Your profile is complete.");
                    topicState.Prompt = null;
                    topicState.Topic = null;
                }
            }
            else if (turnContext.Activity.Text.Trim().Equals("hi", StringComparison.InvariantCultureIgnoreCase))
            {
                await turnContext.SendActivityAsync($"Hi. {userProfile.UserName}.");
            }
            else
            {
                await turnContext.SendActivityAsync("Hi. I'm the Contoso cafe bot.");
            }

            // Use the state property accessors to update the topic state and user profile.
            await Accessors.TopicStateAccessor.SetAsync(turnContext, topicState, cancellationToken);
            await Accessors.UserProfileAccessor.SetAsync(turnContext, userProfile, cancellationToken);

            // Save any state changes to storage.
            await Accessors.ConversationState.SaveChangesAsync(turnContext, false, cancellationToken);
            await Accessors.UserState.SaveChangesAsync(turnContext, false, cancellationToken);
        }
    }
}
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="32a70-139">JavaScript</span><span class="sxs-lookup"><span data-stu-id="32a70-139">JavaScript</span></span>](#tab/javascript)

<span data-ttu-id="32a70-140">**app.js**</span><span class="sxs-lookup"><span data-stu-id="32a70-140">**app.js**</span></span>

```javascript

server.post('/api/messages', (req, res) => {
    adapter.processActivity(req, res, async (context) => {
        const isMessage = (context.activity.type === ActivityTypes.Message);

        // Set up a list of fields that we need to collect from the user.
        const fields = {
            userName: "What is your name?",
            age: "How old are you?",
            workPlace: "Where do you work?"
        }

        // Pull the state of the dialog out of the conversation state manager.
        const convo = await dialogState.get(context, {
            topic: 'profile',
            prompt: undefined
        });

        // Pull the user profile out of the user state manager.
        const userProfile = await userProfile.get(context, {  // Define the user's profile object
                "userName": undefined,
                "age": undefined,
                "workPlace": undefined
            }
        );

        if (isMessage) {

            if (convo.topic === 'profile') {
                // If a prompt flag is set in the conversation state, use it to capture the incoming value
                // into the appropriate field of the user profile.
                if (convo.prompt) {
                    userProfile[convo.prompt] = context.activity.text;
                }

                 // Determine which fields are not yet set.
                 const empty_fields = [];
                 Object.keys(fields).forEach(function(key) {
                    if (!userProfile[key]) {
                        empty_fields.push({
                           key: key,
                           prompt: fields[key]
                        });
                     }
                 });

                 if (empty_fields.length) {

                    // If all the fields are empty, send a welcome message.
                    if (empty_fields.length == Object.keys(fields).length) {
                        await context.sendActivity('Welcome new user, please fill out your profile information.');
                    }

                    // We have at least one empty field. Prompt for the next empty field.
                    await context.sendActivity(empty_fields[0].prompt);

                    // update the flag to indicate which prompt we just sent
                    // so that the response can be captured at the beginning of the next turn.
                    convo.prompt = empty_fields[0].key;

                 } else {
                    // Our user profile is complete!
                    await context.sendActivity('Thank you. Your profile is complete.');
                    convo.prompt = null;
                    convo.topic = null;

                 }
            } else if (context.activity.text && context.activity.text.match(/hi/ig)) {
                // Check to see if the user said "hi" and respond with a greeting
                await context.sendActivity(`Hi ${ userProfile.userName }.`);
            } else {
                // Default message
                await context.sendActivity("Hi. I'm the Contoso bot.");
            }
        }

        // End the turn by writing state changes back to storage
        await conversationState.saveChanges(context);
        await userState.saveChanges(context);
    });
});

```

---

<span data-ttu-id="32a70-141">El ejemplo de código anterior inicializa la marca _topic_ como `profile` con el fin de iniciar la conversación de recopilación de perfil.</span><span class="sxs-lookup"><span data-stu-id="32a70-141">The sample code above initializes the _topic_ flag to `profile` in order to start the profile collection conversation.</span></span> <span data-ttu-id="32a70-142">Para avanzar en la conversación, el bot actualiza la marca _prompt_ a un valor que representa la pregunta actual.</span><span class="sxs-lookup"><span data-stu-id="32a70-142">The bot moves forward through the conversation by updating the _prompt_ flag to a value representing the current question being asked.</span></span> <span data-ttu-id="32a70-143">Cuando esta marca está establecida en el valor apropiado, el bot sabrá qué hacer con el siguiente mensaje recibido del usuario y procesarlo en consecuencia.</span><span class="sxs-lookup"><span data-stu-id="32a70-143">With this flag set to the proper value, the bot will know what to do with the next message received from the user and process it accordingly.</span></span>

<span data-ttu-id="32a70-144">Por último, las marcas se restablecen cuando el bot termina de pedir información.</span><span class="sxs-lookup"><span data-stu-id="32a70-144">Lastly, the flags are reset when the bot is done asking for information.</span></span> <span data-ttu-id="32a70-145">En caso contrario, el bot quedaría en un bucle, sin moverse después de la última pregunta.</span><span class="sxs-lookup"><span data-stu-id="32a70-145">Otherwise, your bot will be trapped in a loop, never moving on from the final question.</span></span>

<span data-ttu-id="32a70-146">Puede extender este patrón para administrar flujos de conversación más complejos mediante la definición de otras marcas o la bifurcación de la conversación en función de la entrada del usuario.</span><span class="sxs-lookup"><span data-stu-id="32a70-146">You can extend this pattern to manage more complex conversation flows by defining other flags, or branching the conversation based on user input.</span></span>

## <a name="manage-multiple-topics-of-conversations"></a><span data-ttu-id="32a70-147">Administración de varios temas de conversación</span><span class="sxs-lookup"><span data-stu-id="32a70-147">Manage multiple topics of conversations</span></span>

<span data-ttu-id="32a70-148">Una vez que es capaz de controlar un tema de conversación, puede ampliar la lógica del bot para controlar varios temas de conversación.</span><span class="sxs-lookup"><span data-stu-id="32a70-148">Once you are able to handle one topic of conversation, you can extend the bot's logic to handle multiple topics of conversation.</span></span> <span data-ttu-id="32a70-149">Se pueden controlar varios temas comprobando condiciones adicionales para, después, tomar la ruta adecuada.</span><span class="sxs-lookup"><span data-stu-id="32a70-149">Multiple topics can be handled by checking for additional conditions, then taking the appropriate path.</span></span>

<span data-ttu-id="32a70-150">Puede ampliar el ejemplo anterior para permitir otras características y temas de conversación, como reservar una mesa o realizar un pedido.</span><span class="sxs-lookup"><span data-stu-id="32a70-150">You can extend the example above to allow for other features and conversation topics, such as reserving a table or placing an order.</span></span> <span data-ttu-id="32a70-151">Agregue marcas adicionales al estado del tema según sea necesario para realizar un seguimiento de la conversación.</span><span class="sxs-lookup"><span data-stu-id="32a70-151">Add additional flags to the topic state as needed to keep track of the conversation.</span></span>

<span data-ttu-id="32a70-152">También resultará útil dividir el código en funciones o métodos independientes, lo que facilita seguir el flujo de la conversación.</span><span class="sxs-lookup"><span data-stu-id="32a70-152">You may also find it helpful to split code into independent functions or methods, making it easier to follow the flow of conversation.</span></span> <span data-ttu-id="32a70-153">Un patrón común es hacer que el bot evalúe el mensaje y el estado y, a continuación, delegar el control a las funciones que implementan los detalles de la característica.</span><span class="sxs-lookup"><span data-stu-id="32a70-153">A common pattern is to have the bot evaluate the message and state, and then delegate control to functions which implement details of the feature.</span></span>

<span data-ttu-id="32a70-154">Para ayudar a los usuarios a moverse mejor entre varios temas de conversación, considere proporcionar un menú principal.</span><span class="sxs-lookup"><span data-stu-id="32a70-154">To help your users better navigate multiple topics of conversation, consider providing a main menu.</span></span> <span data-ttu-id="32a70-155">Por ejemplo, mediante el uso de [acciones sugeridas](bot-builder-howto-add-suggested-actions.md) puede dar al usuario la opción de qué tema elegir en lugar de tener que averiguar lo que puede hacer el bot.</span><span class="sxs-lookup"><span data-stu-id="32a70-155">For example, using [suggested actions](bot-builder-howto-add-suggested-actions.md), you can give your user a choice of which topic they could to engage in, rather than guessing at what your bot can do.</span></span>

## <a name="validate-user-input"></a><span data-ttu-id="32a70-156">Validación de entradas de usuario</span><span class="sxs-lookup"><span data-stu-id="32a70-156">Validate user input</span></span>

<span data-ttu-id="32a70-157">La biblioteca **Dialogs** proporciona formas integradas para validar la entrada del usuario, pero también podemos hacerlo con nuestros propios avisos.</span><span class="sxs-lookup"><span data-stu-id="32a70-157">The **Dialogs** library provides built in ways to validate the user's input, but we can also do so with our own prompts.</span></span> <span data-ttu-id="32a70-158">Por ejemplo, si se solicita la edad del usuario, deseamos asegurarnos de que se obtiene un número y no algo como "Bob" como respuesta.</span><span class="sxs-lookup"><span data-stu-id="32a70-158">For example, if we ask for the user's age, we want to make sure we get a number, not something like "Bob" as a response.</span></span>

<span data-ttu-id="32a70-159">El análisis de un número o de una fecha y hora es una tarea compleja que queda fuera del ámbito de este tema.</span><span class="sxs-lookup"><span data-stu-id="32a70-159">Parsing a number or a date and time is a complex task that's beyond the scope of this topic.</span></span> <span data-ttu-id="32a70-160">Afortunadamente, hay una biblioteca que podemos aprovechar.</span><span class="sxs-lookup"><span data-stu-id="32a70-160">Fortunately, there is a library we can leverage.</span></span> <span data-ttu-id="32a70-161">Para analizar esta información, usamos la biblioteca [Text Recognizer de Microsoft](https://github.com/Microsoft/Recognizers-Text).</span><span class="sxs-lookup"><span data-stu-id="32a70-161">To parse this information, we use the [Microsoft's Text Recognizer](https://github.com/Microsoft/Recognizers-Text) library.</span></span> <span data-ttu-id="32a70-162">Este paquete está disponible mediante NuGet o también puede descargarlo desde el repositorio.</span><span class="sxs-lookup"><span data-stu-id="32a70-162">This package is available through NuGet, or downloading it from the repository.</span></span> <span data-ttu-id="32a70-163">(Se incluye en la biblioteca **Dialogs**, que vale la pena mencionar, aunque no la vamos a usar aquí).</span><span class="sxs-lookup"><span data-stu-id="32a70-163">(It's included in the **Dialogs** library too, which is worth noting even though we are not using it here.)</span></span>

<span data-ttu-id="32a70-164">Esta biblioteca es especialmente útil para analizar entradas complejas, como fechas, horas o números de teléfono.</span><span class="sxs-lookup"><span data-stu-id="32a70-164">This library is particularly useful for parsing complex input like dates, times, or phone numbers.</span></span> <span data-ttu-id="32a70-165">En este ejemplo, se valida un número para el tamaño de la reserva de una cena, pero la misma idea se puede extender para validaciones mucho más complejas.</span><span class="sxs-lookup"><span data-stu-id="32a70-165">In this sample, we're  validating a number for a dinner reservation party size, but the same idea can be extended for more complex validation operations.</span></span>

<span data-ttu-id="32a70-166">En el ejemplo siguiente solo se muestra el uso de `RecognizeNumber`.</span><span class="sxs-lookup"><span data-stu-id="32a70-166">In the following sample we only show the use of `RecognizeNumber`.</span></span> <span data-ttu-id="32a70-167">Encontrará detalles sobre cómo usar otros métodos de Recognizer de la biblioteca en la [documentación del repositorio](https://github.com/Microsoft/Recognizers-Text/blob/master/README.md).</span><span class="sxs-lookup"><span data-stu-id="32a70-167">Details on how to use other Recognizer methods from the library can be found in that [repository's documentation](https://github.com/Microsoft/Recognizers-Text/blob/master/README.md).</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="32a70-168">C#</span><span class="sxs-lookup"><span data-stu-id="32a70-168">C#</span></span>](#tab/csharp)

<span data-ttu-id="32a70-169">Para usar la biblioteca **Microsoft.Recognizers.Text.Number**, incluya el paquete de NuGet y agréguelo a las instrucciones using.</span><span class="sxs-lookup"><span data-stu-id="32a70-169">To use the **Microsoft.Recognizers.Text.Number** library, include the NuGet package and add it to your using statements.</span></span>

```csharp
using Microsoft.Recognizers.Text.Number;
using Microsoft.Recognizers.Text;
using System.Linq;
```

<span data-ttu-id="32a70-170">A continuación, defina una función que realmente realiza la validación.</span><span class="sxs-lookup"><span data-stu-id="32a70-170">Then, define a function that actually does the validation.</span></span>

```csharp
private async Task<bool> ValidatePartySize(ITurnContext context, string value)
{
    try
    {
        // Recognize the input as a number. This works for responses such as
        // "twelve" as well as "12"
        var result = NumberRecognizer.RecognizeNumber(value, Culture.English);

        // Attempt to convert the Recognizer result to an integer
        int.TryParse(result.First().Text, out int partySize);

        if (partySize < 6)
        {
            throw new Exception("Party size too small.");
        }
        else if (partySize > 20)
        {
            throw new Exception("Party size too big.");
        }

        // If we got through this, the number is valid
        return true;
    }
    catch (Exception)
    {
        await context.SendActivityAsync("Error with your party size. < br /> Please specify a number between 6 - 20.");
        return false;
    }
}
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="32a70-171">JavaScript</span><span class="sxs-lookup"><span data-stu-id="32a70-171">JavaScript</span></span>](#tab/javascript)

<span data-ttu-id="32a70-172">Para usar la biblioteca **Recognizers**, debe incluirla en una instrucción require en **app.js**:</span><span class="sxs-lookup"><span data-stu-id="32a70-172">To use the **Recognizers** library, require it in **app.js**:</span></span>

```javascript
// Required packages for this bot
var Recognizers = require('@microsoft/recognizers-text-suite');
```

<span data-ttu-id="32a70-173">A continuación, defina una función que realmente realiza la validación.</span><span class="sxs-lookup"><span data-stu-id="32a70-173">Then, define a function that actually does the validation.</span></span>

```javascript
// Support party size between 6 and 20 only
async function validatePartySize(context, input){
    try {
        // Recognize the input as a number. This works for responses such as
        // "twelve" as well as "12"
        var result = Recognizers.recognizeNumber(input, Recognizers.Culture.English);
        var value = parseInt(results[0].resolution.value);

        if (value < 6) {
            throw new Error(`Party size too small.`);
        } else if(value > 20){
            throw new Error(`Party size too big.`);
        }
        return true; // Return the valid value
    } catch (err){
        await context.sendActivity(`${err.message} <br/>Please specify a number between 6 - 20.`);
        return false;
    }
}
```

---

<span data-ttu-id="32a70-174">Al procesar la respuesta del usuario al aviso, llame a la función de validación antes de continuar con el siguiente aviso.</span><span class="sxs-lookup"><span data-stu-id="32a70-174">While processing the user's response to the prompt, call the validation function before moving on to the next prompt.</span></span> <span data-ttu-id="32a70-175">Si se produce un error en la validación, formule la pregunta de nuevo.</span><span class="sxs-lookup"><span data-stu-id="32a70-175">If the validation fails, ask the question again.</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="32a70-176">C#</span><span class="sxs-lookup"><span data-stu-id="32a70-176">C#</span></span>](#tab/csharp)

```csharp
if (topicState.Prompt == "partySize")
{
    if (await ValidatePartySize(turnContext, turnContext.Activity.Text))
    {
        // Save user's response in our state, ReservationInfo, which
        // is a new class we've added to our state
        // UserFieldInfo partySize;
        partySize.SetValue(userProfile, turnContext.Activity.Text);

        // Ask next question.
        topicState.Prompt = "reserveName";
        await turnContext.SendActivityAsync("Who's name will this be under?");
    }
    else
    {
        // Ask again.
        await turnContext.SendActivityAsync("How many people are in your party?");
    }
}
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="32a70-177">JavaScript</span><span class="sxs-lookup"><span data-stu-id="32a70-177">JavaScript</span></span>](#tab/javascript)

<span data-ttu-id="32a70-178">**app.js**</span><span class="sxs-lookup"><span data-stu-id="32a70-178">**app.js**</span></span>

```javascript
// ...
if (convo.prompt == "partySize") {
    if (await validatePartySize(context, context.activity.text)) {
        // Save user's response
        reservationInfo.partySize = context.activity.text;

        // Ask next question
        convo.prompt = "reserveName";
        await context.sendActivity("Who's name will this be under?");
    } else {
        // Ask again
        await context.sendActivity("How many people are in your party?");
    }
}
// ...
```

---

## <a name="next-step"></a><span data-ttu-id="32a70-179">Paso siguiente</span><span class="sxs-lookup"><span data-stu-id="32a70-179">Next step</span></span>

<span data-ttu-id="32a70-180">Ahora que conoce cómo administrar los estados del aviso, echemos un vistazo a cómo aprovechar la biblioteca **Dialogs** para pedir datos de entrada a los usuarios.</span><span class="sxs-lookup"><span data-stu-id="32a70-180">Now that you have a handle on how to manage the prompt states yourself, let's take a look at how you leverage the **Dialogs** library to prompt users for input.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="32a70-181">Petición de datos de entrada a los usuarios mediante el uso de Dialogs</span><span class="sxs-lookup"><span data-stu-id="32a70-181">Prompt users for input using Dialogs</span></span>](bot-builder-prompts.md)
