---
title: Administración del estado de la conversación y del usuario | Microsoft Docs
description: Obtenga información sobre cómo guardar y recuperar datos con el SDK de Bot Builder para .NET.
keywords: estado de la conversación, estado de usuario, flujo de la conversación
author: ivorb
ms.author: v-ivorb
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 09/18/18
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: 44ec9274e2edcb05a069d353ee5caffec66bb3ca
ms.sourcegitcommit: 3cb288cf2f09eaede317e1bc8d6255becf1aec61
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2018
ms.locfileid: "47389754"
---
# <a name="manage-conversation-and-user-state"></a><span data-ttu-id="28e35-104">Administración del estado de la conversación y del usuario</span><span class="sxs-lookup"><span data-stu-id="28e35-104">Manage conversation and user state</span></span>

[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]

<span data-ttu-id="28e35-105">Una clave del diseño correcto de bots consiste en realizar el seguimiento del contexto de una conversación, para que el bot recuerde cosas como las respuestas a preguntas anteriores.</span><span class="sxs-lookup"><span data-stu-id="28e35-105">A key to good bot design is to track the context of a conversation, so that your bot remembers things like the answers to previous questions.</span></span> <span data-ttu-id="28e35-106">En función del uso previsto del bot, es posible que incluso sea necesario realizar el seguimiento de la información de estado o almacenamiento durante más tiempo que la duración de la conversación.</span><span class="sxs-lookup"><span data-stu-id="28e35-106">Depending on what your bot is used for, you may even need to keep track of state or store information for longer than the lifetime of the conversation.</span></span> <span data-ttu-id="28e35-107">El *estado* de un bot es la información que recuerda con el fin de responder de forma adecuada a los mensajes entrantes.</span><span class="sxs-lookup"><span data-stu-id="28e35-107">A bot's *state* is information it remembers in order to respond appropriately to incoming messages.</span></span> <span data-ttu-id="28e35-108">El SDK Bot Builder proporciona dos clases para almacenar y recuperar datos de estado como un objeto asociado a un usuario o una conversación.</span><span class="sxs-lookup"><span data-stu-id="28e35-108">The Bot Builder SDK provides two classes for storing and retrieving state data as an object associated with a user or a conversation.</span></span>

- <span data-ttu-id="28e35-109">El **estado de la conversación** ayuda al bot a realizar el seguimiento de la conversación actual que tiene con el usuario.</span><span class="sxs-lookup"><span data-stu-id="28e35-109">**Conversation state** help your bot keep track of the current conversation the bot is having with the user.</span></span> <span data-ttu-id="28e35-110">Si es necesario que el bot complete una secuencia de pasos o que cambie entre temas de conversación, se pueden usar las propiedades de la conversación para administrar los pasos en una secuencia o realizar el seguimiento del tema actual.</span><span class="sxs-lookup"><span data-stu-id="28e35-110">If your bot needs to complete a sequence of steps or switch between conversation topics, you can use conversation properties to manage steps in a sequence or track the current topic.</span></span> 

- <span data-ttu-id="28e35-111">El **estado de usuario** se puede usar para muchos propósitos, como determinar dónde dejó la conversación anterior el usuario o simplemente saludar a un usuario por su nombre cuando regrese.</span><span class="sxs-lookup"><span data-stu-id="28e35-111">**User state** can be used for many purposes, such as determining where the user's prior conversation left off or simply greeting a returning user by name.</span></span> <span data-ttu-id="28e35-112">Si almacena las preferencias del usuario, puede usar esa información para personalizar la conversación la próxima vez que chatee.</span><span class="sxs-lookup"><span data-stu-id="28e35-112">If you store a user's preferences, you can use that information to customize the conversation the next time you chat.</span></span> <span data-ttu-id="28e35-113">Por ejemplo, podría alertar al usuario sobre un artículo de noticias que trate un tema que le interese, o bien cuando haya una cita disponible.</span><span class="sxs-lookup"><span data-stu-id="28e35-113">For example, you might alert the user to a news article about a topic that interests her, or alert a user when an appointment becomes available.</span></span> 

<span data-ttu-id="28e35-114">Las clases `ConversationState` y `UserState` son clases de estado que son especializaciones de la clase `BotState` con directivas que controlan la vigencia y el alcance de los objetos almacenados en ellas.</span><span class="sxs-lookup"><span data-stu-id="28e35-114">The `ConversationState` and `UserState` are state classes that are specializations of `BotState` class with policies that control the lifetime and scope of the objects stored in them.</span></span> <span data-ttu-id="28e35-115">Componentes que necesitan almacenar el estado, crean y registran una propiedad con un tipo, y usan el descriptor de acceso de la propiedad para acceder al estado.</span><span class="sxs-lookup"><span data-stu-id="28e35-115">Components that need to store state create and register a property with a type, and use property accessor to access the state.</span></span> <span data-ttu-id="28e35-116">El administrador de estado del bot puede utilizar el almacenamiento de memoria, CosmosDB y Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="28e35-116">The bot state manager can use memory storage, CosmosDB, and Blob Storage.</span></span> 

> [!NOTE] 
> <span data-ttu-id="28e35-117">Utilice el administrador de estado del bot para almacenar las preferencias, el nombre de usuario o lo último que haya pedido, pero no lo utilice para almacenar datos empresariales críticos.</span><span class="sxs-lookup"><span data-stu-id="28e35-117">Use bot state manager to store preferences, user name, or the last thing they ordered, but do not use it to store critical business data.</span></span> <span data-ttu-id="28e35-118">Para los datos críticos, cree sus propios componentes de almacenamiento o escríbalos directamente en el [almacenamiento](bot-builder-howto-v4-storage.md).</span><span class="sxs-lookup"><span data-stu-id="28e35-118">For critical data, create your own storage components or write directly to [storage](bot-builder-howto-v4-storage.md).</span></span>
> <span data-ttu-id="28e35-119">El almacenamiento de datos en memoria solo está pensado para realizar pruebas.</span><span class="sxs-lookup"><span data-stu-id="28e35-119">In-memory data storage is intended for testing only.</span></span> <span data-ttu-id="28e35-120">Este tipo de almacenamiento es volátil y temporal.</span><span class="sxs-lookup"><span data-stu-id="28e35-120">This storage is volatile and temporary.</span></span> <span data-ttu-id="28e35-121">Los datos se borran cada vez que se reinicia el bot.</span><span class="sxs-lookup"><span data-stu-id="28e35-121">The data is cleared each time the bot is restarted.</span></span>

## <a name="using-conversation-state-and-user-state-to-direct-conversation-flow"></a><span data-ttu-id="28e35-122">Uso del estado de la conversación y estado de usuario para dirigir el flujo de la conversación</span><span class="sxs-lookup"><span data-stu-id="28e35-122">Using conversation state and user state to direct conversation flow</span></span>
<span data-ttu-id="28e35-123">Al diseñar un flujo de conversación, es útil definir un indicador de estado para dirigir el flujo de la conversación.</span><span class="sxs-lookup"><span data-stu-id="28e35-123">In designing a conversation flow, it is useful to define a state flag to direct the conversation flow.</span></span> <span data-ttu-id="28e35-124">La marca puede ser un simple tipo booleano o un tipo que incluya el nombre del tema actual.</span><span class="sxs-lookup"><span data-stu-id="28e35-124">The flag can be a simple boolean type or a type that includes the name of the current topic.</span></span> <span data-ttu-id="28e35-125">La marca puede ayudar a realizar el seguimiento de dónde se encuentra en una conversación.</span><span class="sxs-lookup"><span data-stu-id="28e35-125">The flag can help you track where in a conversation you are.</span></span> <span data-ttu-id="28e35-126">Por ejemplo, un indicador de tipo booleano puede indicar si se está en una conversación o no, mientras que una propiedad de nombre de tema puede indicar en qué conversación se está actualmente.</span><span class="sxs-lookup"><span data-stu-id="28e35-126">For example, a boolean type flag can tell you whether you are in a conversation or not, while a topic name property can tell you which conversation you are currently in.</span></span>



# <a name="ctabcsharp"></a>[<span data-ttu-id="28e35-127">C#</span><span class="sxs-lookup"><span data-stu-id="28e35-127">C#</span></span>](#tab/csharp)
### <a name="conversation-and-user-state"></a><span data-ttu-id="28e35-128">Estado de la conversación y de usuario</span><span class="sxs-lookup"><span data-stu-id="28e35-128">Conversation and User state</span></span>
<span data-ttu-id="28e35-129">Puede usar el [ejemplo Echo Bot With Counter](https://aka.ms/EchoBot-With-Counter) como punto inicial de este tema de procedimientos.</span><span class="sxs-lookup"><span data-stu-id="28e35-129">You can use [Echo Bot With Counter sample](https://aka.ms/EchoBot-With-Counter) as the start point for this how-to.</span></span> <span data-ttu-id="28e35-130">En primer lugar, cree la clase `TopicState` para administrar el tema actual de la conversación en `TopicState.cs`, tal como se muestra a continuación:</span><span class="sxs-lookup"><span data-stu-id="28e35-130">First, create `TopicState` class to manage the current topic of conversation in `TopicState.cs` as shown below:</span></span>

```csharp
public class TopicState
{
   public string Prompt { get; set; } = "askName";
}
``` 
<span data-ttu-id="28e35-131">Después, cree la clase `UserProfile` en `UserProfile.cs` para administrar el estado de usuario.</span><span class="sxs-lookup"><span data-stu-id="28e35-131">Then create `UserProfile` class in `UserProfile.cs` to manage user state.</span></span>
```csharp
public class UserProfile
{
    public string UserName { get; set; }
    public string TelephoneNumber { get; set; }
}
``` 
<span data-ttu-id="28e35-132">La clase `TopicState` tiene una marca para realizar un seguimiento de dónde estamos en la conversación y utiliza el estado de la conversación para almacenarla.</span><span class="sxs-lookup"><span data-stu-id="28e35-132">The `TopicState` class has a flag to keep track of where we are in the conversation and uses conversation state to store it.</span></span> <span data-ttu-id="28e35-133">El aviso se inicializa como "askName" para iniciar la conversación.</span><span class="sxs-lookup"><span data-stu-id="28e35-133">The Prompt is initialized as "askName" to initiate the conversation.</span></span> <span data-ttu-id="28e35-134">Cuando el bot recibe la respuesta del usuario, el aviso se redefinirá como "askNumber" para iniciar la siguiente conversación.</span><span class="sxs-lookup"><span data-stu-id="28e35-134">Once the bot receives response from the user, Prompt will be redefined as "askNumber" to initiate the next conversation.</span></span> <span data-ttu-id="28e35-135">La clase `UserProfile` realiza el seguimiento del nombre de usuario y número de teléfono y los almacena en el estado de usuario.</span><span class="sxs-lookup"><span data-stu-id="28e35-135">`UserProfile` class tracks user name and phone number and stores it in user state.</span></span>

### <a name="property-accessors"></a><span data-ttu-id="28e35-136">Descriptores de acceso de propiedad</span><span class="sxs-lookup"><span data-stu-id="28e35-136">Property accessors</span></span>
<span data-ttu-id="28e35-137">La clase `EchoBotAccessors` en nuestro ejemplo se crea como un singleton y se pasa al constructor `class EchoWithCounterBot : IBot` a través de la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="28e35-137">The `EchoBotAccessors` class in our example is created as a singleton and passed into the `class EchoWithCounterBot : IBot` constructor through dependency injection.</span></span> <span data-ttu-id="28e35-138">El constructor de la clase `EchoBotAccessors` inicializa una nueva instancia de la clase `EchoBotAccessors`.</span><span class="sxs-lookup"><span data-stu-id="28e35-138">The `EchoBotAccessors` class constructor initializes a new instance of the `EchoBotAccessors` class.</span></span> <span data-ttu-id="28e35-139">Contiene las clases `ConversationState`, `UserState` y `IStatePropertyAccessor` asociadas.</span><span class="sxs-lookup"><span data-stu-id="28e35-139">It contains the `ConversationState`, `UserState`, and associated `IStatePropertyAccessor`.</span></span> <span data-ttu-id="28e35-140">El objeto `conversationState` almacena el estado del tema y el objeto `userState` que almacena la información de perfil de usuario.</span><span class="sxs-lookup"><span data-stu-id="28e35-140">The `conversationState` object stores the topic state and `userState` object that stores the user profile information.</span></span> <span data-ttu-id="28e35-141">Los objetos `ConversationState` y `UserState` se crean en el archivo Startup.cs.</span><span class="sxs-lookup"><span data-stu-id="28e35-141">The `ConversationState` and `UserState` objects are created in the Startup.cs file.</span></span> <span data-ttu-id="28e35-142">Los objetos de conversación y de estado de usuario son los que persisten en la conversación y en el ámbito del usuario.</span><span class="sxs-lookup"><span data-stu-id="28e35-142">The conversation and user state objects are where we persist anything at the conversation and user scope.</span></span> 

<span data-ttu-id="28e35-143">Actualice el constructor para incluir `UserState`, tal como se muestra a continuación:</span><span class="sxs-lookup"><span data-stu-id="28e35-143">Updated the constructor to include `UserState` as shown below:</span></span>
```csharp
public EchoBotAccessors(ConversationState conversationState, UserState userState)
{
    ConversationState = conversationState ?? throw new ArgumentNullException(nameof(conversationState));
    UserState = userState ?? throw new ArgumentNullException(nameof(userState));
}
```
<span data-ttu-id="28e35-144">Cree nombres únicos para los descriptores de acceso `TopicState` y `UserProfile`.</span><span class="sxs-lookup"><span data-stu-id="28e35-144">Create unique names for `TopicState` and `UserProfile` accessors.</span></span>
```csharp
public static string UserProfileName { get; } = $"{nameof(EchoBotAccessors)}.UserProfile";
public static string TopicStateName { get; } = $"{nameof(EchoBotAccessors)}.TopicState";
```
<span data-ttu-id="28e35-145">Después, defina dos descriptores de acceso.</span><span class="sxs-lookup"><span data-stu-id="28e35-145">Next, define two accessors.</span></span> <span data-ttu-id="28e35-146">El primero almacena el tema de la conversación y el segundo almacena el nombre del usuario y el número de teléfono.</span><span class="sxs-lookup"><span data-stu-id="28e35-146">The first one stores the topic of the conversation, and the second stores user's name and phone number.</span></span>
```csharp
public IStatePropertyAccessor<TopicState> TopicState { get; set; }
public IStatePropertyAccessor<UserProfile> UserProfile { get; set; }
```

<span data-ttu-id="28e35-147">Las propiedades para obtener el estado de conversación ya están definidas, pero necesitará agregar `UserState`, como se muestra a continuación:</span><span class="sxs-lookup"><span data-stu-id="28e35-147">Properties to get the ConversationState is already defined, but you'll need to add `UserState` as shown below:</span></span>
```csharp
public ConversationState ConversationState { get; }
public UserState UserState { get; }
```
<span data-ttu-id="28e35-148">Después de realizar los cambios, guarde el archivo.</span><span class="sxs-lookup"><span data-stu-id="28e35-148">After making the changes, save the file.</span></span> <span data-ttu-id="28e35-149">A continuación, vamos a actualizar la clase Startup para crear el objeto `UserState` para que conserve cualquier cosa en el ámbito de usuario.</span><span class="sxs-lookup"><span data-stu-id="28e35-149">Next, we will update the Startup class to create `UserState` object to persist anything at the user-scope.</span></span> <span data-ttu-id="28e35-150">El `ConversationState` ya existe.</span><span class="sxs-lookup"><span data-stu-id="28e35-150">The `ConversationState` is already exists.</span></span> 
```csharp

services.AddBot<EchoWithCounterBot>(options =>
{
    ...

    IStorage dataStore = new MemoryStorage();
    
    var conversationState = new ConversationState(dataStore);
    options.State.Add(conversationState);
        
    var userState = new UserState(dataStore);  
    options.State.Add(userState);
});
```
<span data-ttu-id="28e35-151">La línea `options.State.Add(ConversationState);` y `options.State.Add(userState);` agregan el estado de la conversación y el estado de usuario, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="28e35-151">The line `options.State.Add(ConversationState);` and `options.State.Add(userState);` add the conversation state and user state, respectively.</span></span> <span data-ttu-id="28e35-152">Después, cree y registre los descriptores de acceso de estado.</span><span class="sxs-lookup"><span data-stu-id="28e35-152">Next, create and register state accesssors.</span></span> <span data-ttu-id="28e35-153">Los descriptores de acceso creados aquí se pasan a la clase derivada de IBot en cada turno.</span><span class="sxs-lookup"><span data-stu-id="28e35-153">Acessors created here are passed into the IBot-derived class on every turn.</span></span> <span data-ttu-id="28e35-154">Modifique el código para incluir el estado de usuario, tal como se muestra a continuación:</span><span class="sxs-lookup"><span data-stu-id="28e35-154">Modift the code to inclue user state as shown below:</span></span>
```csharp
services.AddSingleton<EchoBotAccessors>(sp =>
{
   ...
    var userState = options.State.OfType<UserState>().FirstOrDefault();
    if (userState == null)
    {
        throw new InvalidOperationException("UserState must be defined and added before adding user-scoped state accessors.");
    }
   ...
 });
```

<span data-ttu-id="28e35-155">A continuación, cree los dos descriptores de acceso mediante `TopicState` y `UserProfile`, y páselos a la clase `class EchoWithCounterBot : IBot` mediante la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="28e35-155">Next, create the two accessors using `TopicState` and `UserProfile` and pass it into the `class EchoWithCounterBot : IBot` class through dependency injection.</span></span>
```csharp
services.AddSingleton<EchoBotAccessors>(sp =>
{
   ...
    var accessors = new BotAccessors(conversationState, userState)
    {
        TopicState = conversationState.CreateProperty<TopicState>("TopicState"),
        UserProfile = userState.CreateProperty<UserProfile>("UserProfile"),
     };

     return accessors;
 });
```

<span data-ttu-id="28e35-156">La conversación y el estado del usuario se vinculan a un singleton mediante el bloque de código `services.AddSingleton` y se guardan mediante un descriptor de acceso de almacenamiento de estado en el código que comienza con `var accessors = new BotAccessor(conversationState, userState)`.</span><span class="sxs-lookup"><span data-stu-id="28e35-156">The conversation and user state are linked to a singleton via the `services.AddSingleton` code block and saved via a state store accessor in the code starting with `var accessors = new BotAccessor(conversationState, userState)`.</span></span>

### <a name="use-conversation-and-user-state-properties"></a><span data-ttu-id="28e35-157">Uso de propiedades de estado de conversación y usuario</span><span class="sxs-lookup"><span data-stu-id="28e35-157">Use conversation and user state properties</span></span> 
<span data-ttu-id="28e35-158">En el controlador `OnTurnAsync` de la clase `EchoWithCounterBot : IBot`, modifique el código para solicitar el nombre de usuario y después el número de teléfono.</span><span class="sxs-lookup"><span data-stu-id="28e35-158">In the `OnTurnAsync` handler of the `EchoWithCounterBot : IBot` class, modify the code to prompt for user name and then phone number.</span></span> <span data-ttu-id="28e35-159">Para realizar el seguimiento de dónde estamos en la conversación, usamos la propiedad Prompt que se define en el estado del tema.</span><span class="sxs-lookup"><span data-stu-id="28e35-159">To track where we are in the conversation, we use the Prompt property defined in the TopicState.</span></span> <span data-ttu-id="28e35-160">Esta propiedad se ha inicializado como "askName".</span><span class="sxs-lookup"><span data-stu-id="28e35-160">This property was initialized a "askName".</span></span> <span data-ttu-id="28e35-161">Cuando se obtiene el nombre de usuario, se establece en "askNumber" y se configura con el nombre de usuario escrito.</span><span class="sxs-lookup"><span data-stu-id="28e35-161">Once we get the user name, we set it to "askNumber" and set the UserName to the name user typed in.</span></span> <span data-ttu-id="28e35-162">Después de recibir el número de teléfono, envía un mensaje de confirmación y establece la pregunta en "confirmación" porque se encuentra al final de la conversación.</span><span class="sxs-lookup"><span data-stu-id="28e35-162">After the phone number is received, you send a confirmation message and set the prompt to 'confirmation' because you are at the end of the conversation.</span></span>

```csharp
if (turnContext.Activity.Type == ActivityTypes.Message)
{
    // Get the conversation state from the turn context.
    var convo = await _accessors.TopicState.GetAsync(turnContext, () => new TopicState());
    
    // Get the user state from the turn context.
    var user = await _accessors.UserProfile.GetAsync(turnContext, () => new UserProfile());
    
    // Ask user name. The Prompt was initialiazed as "askName" in the TopicState.cs file.
    if (convo.Prompt == "askName")
    {
        await turnContext.SendActivityAsync("What is your name?");
        
        // Set the Prompt to ask the next question for this conversation
        convo.Prompt = "askNumber";
        
        // Set the property using the accessor
        await _accessors.TopicState.SetAsync(turnContext, convo);
        
        //Save the new prompt into the conversation state.
        await _accessors.ConversationState.SaveChangesAsync(turnContext);
    }
    else if (convo.Prompt == "askNumber")
    {
        // Set the UserName that is defined in the UserProfile class
        user.UserName = turnContext.Activity.Text;
        
        // Use the user name to prompt the user for phone number
        await turnContext.SendActivityAsync($"Hello, {user.UserName}. What's your telephone number?");
        
        // Set the Prompt now that we have collected all the data
        convo.Prompt = "confirmation";
                 
        await _accessors.TopicState.SetAsync(turnContext, convo);
        await _accessors.ConversationState.SaveChangesAsync(turnContext);

        await _accessors.UserProfile.SetAsync(turnContext, user);
        await _accessors.UserState.SaveChangesAsync(turnContext);
    }
    else if (convo.Prompt == "confirmation")
    { 
        // Set the TelephoneNumber that is defined in the UserProfile class
        user.TelephoneNumber = turnContext.Activity.Text;

        await turnContext.SendActivityAsync($"Got it, {user.UserName}. I'll call you later.");

        // initialize prompt
        convo.Prompt = ""; // End of conversation
        await _accessors.TopicState.SetAsync(turnContext, convo);
        await _accessors.ConversationState.SaveChangesAsync(turnContext);
    }
}
```   

# <a name="javascripttabjs"></a>[<span data-ttu-id="28e35-163">JavaScript</span><span class="sxs-lookup"><span data-stu-id="28e35-163">JavaScript</span></span>](#tab/js)

### <a name="conversation-and-user-state"></a><span data-ttu-id="28e35-164">Estado de la conversación y de usuario</span><span class="sxs-lookup"><span data-stu-id="28e35-164">Conversation and User state</span></span>

<span data-ttu-id="28e35-165">Puede usar el [ejemplo Echo Bot With Counter](https://aka.ms/EchoBot-With-Counter-JS) como punto inicial de este tema de procedimientos.</span><span class="sxs-lookup"><span data-stu-id="28e35-165">You can use [Echo Bot With Counter sample](https://aka.ms/EchoBot-With-Counter-JS) as the starting point for this how-to.</span></span> <span data-ttu-id="28e35-166">Este ejemplo ya utiliza el objeto `ConversationState` para almacenar el número de mensajes.</span><span class="sxs-lookup"><span data-stu-id="28e35-166">This sample already uses the `ConversationState` to store the message count.</span></span> <span data-ttu-id="28e35-167">Necesitaremos agregar un objeto `TopicStates` para realizar el seguimiento del estado de conversación y el objeto `UserState` para realizar el seguimiento de la información del usuario en un objeto `userProfile`.</span><span class="sxs-lookup"><span data-stu-id="28e35-167">We will need to add a `TopicStates` object to track our conversation state and `UserState` to track user information in a `userProfile` object.</span></span> 

<span data-ttu-id="28e35-168">En el archivo `index.js` del bot principal, agregue el objeto `UserState` a la lista requerida:</span><span class="sxs-lookup"><span data-stu-id="28e35-168">In the main bot's `index.js` file, add the `UserState` to the require list:</span></span>

<span data-ttu-id="28e35-169">**index.js**</span><span class="sxs-lookup"><span data-stu-id="28e35-169">**index.js**</span></span>

```javascript
// Import required bot services. See https://aka.ms/bot-services to learn more about the different parts of a bot.
const { BotFrameworkAdapter, MemoryStorage, ConversationState, UserState } = require('botbuilder');
```

<span data-ttu-id="28e35-170">Después, cree `UserState` mediante `MemoryStorage` como proveedor de almacenamiento y páselo como segundo argumento de la clase `MainDialog`.</span><span class="sxs-lookup"><span data-stu-id="28e35-170">Next, create the `UserState` using `MemoryStorage` as the storage provider then pass it as the second argument to the `MainDialog` class.</span></span>

<span data-ttu-id="28e35-171">**index.js**</span><span class="sxs-lookup"><span data-stu-id="28e35-171">**index.js**</span></span>

```javascript
// Create conversation state with in-memory storage provider. 
const conversationState = new ConversationState(memoryStorage);
const userState = new UserState(memoryStorage);
// Create the main dialog.
const mainDlg = new MainDialog(conversationState, userState);
```

<span data-ttu-id="28e35-172">En el archivo `dialogs/mainDialog/index.js`, actualice el constructor para que acepte `userState` como segundo argumento.</span><span class="sxs-lookup"><span data-stu-id="28e35-172">In your `dialogs/mainDialog/index.js` file, update the constructor to accept the `userState` as the second argument.</span></span> <span data-ttu-id="28e35-173">Después, cree una propiedad `topicStates` desde `conversationState` y cree una propiedad `userProfile` desde `userState`.</span><span class="sxs-lookup"><span data-stu-id="28e35-173">Then create a `topicStates` property from the `conversationState` and create a `userProfile` property from the `userState`.</span></span>

<span data-ttu-id="28e35-174">**dialogs/mainDialog/index.js**</span><span class="sxs-lookup"><span data-stu-id="28e35-174">**dialogs/mainDialog/index.js**</span></span>

```javascript
constructor (conversationState, userState) {
    // creates a new state accessor property.see https://aka.ms/about-bot-state-accessors to learn more about the bot state and state accessors 
    this.conversationState = conversationState;
    this.topicState = this.conversationState.createProperty(TOPIC_STATE);

    // User state
    this.userState = userState;
    this.userProfile = this.userState.createProperty(USER_PROFILE);
}
```

### <a name="use-conversation-and-user-state-properties"></a><span data-ttu-id="28e35-175">Uso de propiedades de estado de conversación y usuario</span><span class="sxs-lookup"><span data-stu-id="28e35-175">Use conversation and user state properties</span></span>

<span data-ttu-id="28e35-176">En el controlador `onTurn` de la clase `MainDialog`, modifique el código para solicitar el nombre de usuario y después el número de teléfono.</span><span class="sxs-lookup"><span data-stu-id="28e35-176">In the `onTurn` handler of the `MainDialog` class, modify the code to prompt for user name and then phone number.</span></span> <span data-ttu-id="28e35-177">Para realizar el seguimiento de dónde estamos en la conversación, usamos la propiedad `prompt` que se define en `topicState`.</span><span class="sxs-lookup"><span data-stu-id="28e35-177">To track where we are in the conversation, we use the `prompt` property defined in the `topicState`.</span></span> <span data-ttu-id="28e35-178">Esta propiedad se inicializa en "askName".</span><span class="sxs-lookup"><span data-stu-id="28e35-178">This property is initialized to "askName".</span></span> <span data-ttu-id="28e35-179">Cuando se obtiene el nombre de usuario, se establece en "askNumber" y se configura con el nombre de usuario escrito.</span><span class="sxs-lookup"><span data-stu-id="28e35-179">Once we get the user name, we set it to "askNumber" and set the UserName to the name user typed in.</span></span> <span data-ttu-id="28e35-180">Después de recibir el número de teléfono, envía un mensaje de confirmación y establece la pregunta en `undefined` porque se encuentra al final de la conversación.</span><span class="sxs-lookup"><span data-stu-id="28e35-180">After the phone number is received, you send a confirmation message and set the prompt to `undefined` because you are at the end of the conversation.</span></span>

<span data-ttu-id="28e35-181">**dialogs/mainDialog/index.js**</span><span class="sxs-lookup"><span data-stu-id="28e35-181">**dialogs/mainDialog/index.js**</span></span>

```javascript
// see https://aka.ms/about-bot-activity-message to learn more about the message and other activity types
if (context.activity.type === 'message') {
    // read from state and set default object if object does not exist in storage.
    let topicState = await this.topicState.get(context, {
        //Define the topic state object
        prompt: "askName"
    });
    let userProfile = await this.userProfile.get(context, {  
        // Define the user's profile object
        "userName": undefined,
        "telephoneNumber": undefined
    });

    if(topicState.prompt == "askName"){
        await context.sendActivity("What is your name?");

        // Set next prompt state
        topicState.prompt = "askNumber";

        // Update state
        await this.topicState.set(context, topicState);
    }
    else if(topicState.prompt == "askNumber"){
        // Set the UserName that is defined in the UserProfile class
        userProfile.userName = context.activity.text;

        // Use the user name to prompt the user for phone number
        await context.sendActivity(`Hello, ${userProfile.userName}. What's your telephone number?`);

        // Set next prompt state
        topicState.prompt = "confirmation";

        // Update states
        await this.topicState.set(context, topicState);
        await this.userProfile.set(context, userProfile);
    }
    else if(topicState.prompt == "confirmation"){
        // Set the phone number
        userProfile.telephoneNumber = context.activity.text;

        // Sent confirmation
        await context.sendActivity(`Got it, ${userProfile.userName}. I'll call you later.`)

        // Set next prompt state
        topicState.prompt = undefined; // End of conversation

        // Update states
        await this.topicState.set(context, topicState);
        await this.userProfile.set(context, userProfile);
    }
    
    // Save state changes to storage
    await this.conversationState.saveChanges(context);
    await this.userState.saveChanges(context);
    
}
else {
    await context.sendActivity(`[${context.activity.type} event detected]`);
}
```

---

## <a name="start-your-bot"></a><span data-ttu-id="28e35-182">Inicio del bot</span><span class="sxs-lookup"><span data-stu-id="28e35-182">Start your bot</span></span>
<span data-ttu-id="28e35-183">Ejecute el bot localmente.</span><span class="sxs-lookup"><span data-stu-id="28e35-183">Run your bot locally.</span></span>

### <a name="start-the-emulator-and-connect-your-bot"></a><span data-ttu-id="28e35-184">Inicio del emulador y conexión del bot</span><span class="sxs-lookup"><span data-stu-id="28e35-184">Start the emulator and connect your bot</span></span>
<span data-ttu-id="28e35-185">A continuación, inicie el emulador y, después, conéctese al bot en el emulador:</span><span class="sxs-lookup"><span data-stu-id="28e35-185">Next, start the emulator and then connect to your bot in the emulator:</span></span>

1. <span data-ttu-id="28e35-186">Haga clic en el vínculo **Open bot** (Abrir bot) de la pestaña de bienvenida del emulador.</span><span class="sxs-lookup"><span data-stu-id="28e35-186">Click the **Open Bot** link in the emulator "Welcome" tab.</span></span> 
2. <span data-ttu-id="28e35-187">Seleccione el archivo .bot ubicado en el directorio donde se creó la solución de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="28e35-187">Select the .bot file located in the directory where you created the Visual Studio solution.</span></span>

### <a name="interact-with-your-bot"></a><span data-ttu-id="28e35-188">Interacción con el bot</span><span class="sxs-lookup"><span data-stu-id="28e35-188">Interact with your bot</span></span>

<span data-ttu-id="28e35-189">Envíe un mensaje al bot y este responderá con un mensaje.</span><span class="sxs-lookup"><span data-stu-id="28e35-189">Send a message to your bot, and the bot will respond back with a message.</span></span>
<span data-ttu-id="28e35-190">![Emulador en ejecución](../media/emulator-v4/emulator-running.png)</span><span class="sxs-lookup"><span data-stu-id="28e35-190">![Emulator running](../media/emulator-v4/emulator-running.png)</span></span>

<span data-ttu-id="28e35-191">Si decide administrar usted mismo el estado, consulte [Petición de datos de entrada a los usuarios mediante sus propios mensajes](bot-builder-primitive-prompts.md).</span><span class="sxs-lookup"><span data-stu-id="28e35-191">If you decide to manage state yourself, see [manage conversation flow with your own prompts](bot-builder-primitive-prompts.md).</span></span> <span data-ttu-id="28e35-192">Una alternativa consiste en usar el cuadro de diálogo de cascada.</span><span class="sxs-lookup"><span data-stu-id="28e35-192">An alternative is to use the waterfall dialog.</span></span> <span data-ttu-id="28e35-193">El cuadro de diálogo realiza el seguimiento del estado de la conversación de forma automática, por lo que no es necesario crear marcas para realizar el seguimiento del estado.</span><span class="sxs-lookup"><span data-stu-id="28e35-193">The dialog keeps track of the conversation state for you so you do not need to create flags to track your state.</span></span> <span data-ttu-id="28e35-194">Para más información, consulte [Administración de conversaciones simples con diálogos](bot-builder-dialog-manage-conversation-flow.md).</span><span class="sxs-lookup"><span data-stu-id="28e35-194">For more information, see [manage a simple conversation with dialogs](bot-builder-dialog-manage-conversation-flow.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="28e35-195">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="28e35-195">Next steps</span></span>
<span data-ttu-id="28e35-196">Ahora que sabe cómo usar el estado para leer y escribir datos de bot en el almacenamiento, veremos cómo puede leer y escribir directamente en el almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="28e35-196">Now that you know how to use state to help you read and write bot data to storage, lets take a look at how you can read and write directly to storage.</span></span>

> [!div class="nextstepaction"]
> <span data-ttu-id="28e35-197">[Escritura directa en el almacenamiento](bot-builder-howto-v4-storage.md).</span><span class="sxs-lookup"><span data-stu-id="28e35-197">[How to write directly to storage](bot-builder-howto-v4-storage.md).</span></span>
