---
title: Administración del estado de la conversación y del usuario | Microsoft Docs
description: Obtenga información sobre cómo guardar y recuperar datos con el SDK V4 de Bot Builder para .NET.
keywords: estado de la conversación, estado del usuario, software intermedio de estado, flujo de la conversación, almacenamiento de archivos, almacenamiento de tablas de azure
author: ivorb
ms.author: v-demak
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 05/03/18
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: a74c52af0ca56b62491ca3aa39d09885c2540c18
ms.sourcegitcommit: ee63d9dc1944a6843368bdabf5878950229f61d0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/23/2018
ms.locfileid: "42795214"
---
# <a name="manage-conversation-and-user-state"></a><span data-ttu-id="4ba49-104">Administración del estado de la conversación y del usuario</span><span class="sxs-lookup"><span data-stu-id="4ba49-104">Manage conversation and user state</span></span>

[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]

<span data-ttu-id="4ba49-105">Para que el bot guarde el estado del usuario y la conversación, inicialice primero el software intermedio de administrador de estado y, después, use las propiedades de estado de usuario y de conversación.</span><span class="sxs-lookup"><span data-stu-id="4ba49-105">For your bot to save conversation and user state, first initialize state manager middleware, and then use the conversation and user state properties.</span></span>
<span data-ttu-id="4ba49-106">Para obtener más información sobre cómo se usa ese estado, vea [Estado y almacenamiento](./bot-builder-storage-concept.md).</span><span class="sxs-lookup"><span data-stu-id="4ba49-106">For more information on how that state is used, see [State and storage](./bot-builder-storage-concept.md).</span></span>

## <a name="initialize-state-manager-middleware"></a><span data-ttu-id="4ba49-107">Inicializar el software intermedio de administrador de estado</span><span class="sxs-lookup"><span data-stu-id="4ba49-107">Initialize state manager middleware</span></span>

<span data-ttu-id="4ba49-108">En el SDK, debe inicializar el adaptador de bot para usar el software intermedio de administrador de estado con el fin de poder usar los almacenes de propiedades de conversación o usuario.</span><span class="sxs-lookup"><span data-stu-id="4ba49-108">In the SDK, you need to initialize the bot adapter to use state manager middleware before you can use the conversation or user property stores.</span></span> <span data-ttu-id="4ba49-109">El _estado de la conversación_ se usa para las propiedades de conversación, y el _estado de usuario_ para las de usuario.</span><span class="sxs-lookup"><span data-stu-id="4ba49-109">_Conversation state_ is used for conversation properties, and _user state_ is used for user properties.</span></span> <span data-ttu-id="4ba49-110">(Se puede acceder a las propiedades de estado del usuario entre varias conversaciones). El software intermedio de administrador de estado proporciona una abstracción que permite acceder a las propiedades mediante un simple valor de clave o almacén de objeto, independiente del tipo de almacenamiento subyacente.</span><span class="sxs-lookup"><span data-stu-id="4ba49-110">(User state properties can be accessed across multiple conversations.) The state manager middleware provides an abstraction that lets you access properties using a simple key-value or object store, independent of the type of underlying storage.</span></span> <span data-ttu-id="4ba49-111">El administrador de estado se encarga de escribir los datos en el almacenamiento y de administrar la simultaneidad, con independencia de que el tipo de almacenamiento subyacente sea en memoria, de archivos o Azure Table Storage.</span><span class="sxs-lookup"><span data-stu-id="4ba49-111">The state manager takes care of writing data to storage and managing concurrency, whether the underlying storage type is in-memory, file storage, or Azure Table Storage.</span></span>


# <a name="ctabcsharp"></a>[<span data-ttu-id="4ba49-112">C#</span><span class="sxs-lookup"><span data-stu-id="4ba49-112">C#</span></span>](#tab/csharp)
<span data-ttu-id="4ba49-113">Para ver cómo se inicializa `ConversationState`, vea `Startup.cs` en el ejemplo Microsoft.Bot.Samples.EchoBot AspNetCore.</span><span class="sxs-lookup"><span data-stu-id="4ba49-113">To see how `ConversationState` is initialized, see `Startup.cs` in the Microsoft.Bot.Samples.EchoBot-AspNetCore sample.</span></span>

<span data-ttu-id="4ba49-114">Bibliotecas necesarias para este código:</span><span class="sxs-lookup"><span data-stu-id="4ba49-114">The libraries required for this code:</span></span>

```csharp
using Microsoft.Bot.Builder.BotFramework;
using Microsoft.Bot.Builder.Core.Extensions;
using Microsoft.Bot.Builder.Integration.AspNet.Core;
```

<span data-ttu-id="4ba49-115">Inicializando `ConversationState`:</span><span class="sxs-lookup"><span data-stu-id="4ba49-115">Initializing `ConversationState`:</span></span>

```csharp
services.AddBot<EchoBot>(options =>
{
    options.CredentialProvider = new ConfigurationCredentialProvider(Configuration);

    IStorage dataStore = new MemoryStorage();
    options.Middleware.Add(new ConversationState<EchoState>(dataStore));
});
```

<span data-ttu-id="4ba49-116">En la línea `options.Middleware.Add(new ConversationState<EchoState>(dataStore));`, `ConversationState` es el objeto de administrador de estado de la conversación, que se agrega al bot como software intermedio.</span><span class="sxs-lookup"><span data-stu-id="4ba49-116">In the line `options.Middleware.Add(new ConversationState<EchoState>(dataStore));`, `ConversationState` is the conversation state manager object, which is added to the bot as middleware.</span></span> <span data-ttu-id="4ba49-117">El parámetro de tipo `EchoState` es el tipo que representa cómo se quiere almacenar la información de estado de la conversación.</span><span class="sxs-lookup"><span data-stu-id="4ba49-117">The `EchoState` type parameter is the type representing how you want conversation state info to be stored.</span></span> <span data-ttu-id="4ba49-118">Un bot puede usar cualquier tipo de clase para los datos de estado de usuario o de conversación.</span><span class="sxs-lookup"><span data-stu-id="4ba49-118">A bot can use any class type for conversation or user state data.</span></span>

<span data-ttu-id="4ba49-119">La implementación de `EchoState` está en `EchoBot.cs`:</span><span class="sxs-lookup"><span data-stu-id="4ba49-119">The implementation of `EchoState` is in `EchoBot.cs`:</span></span>
```csharp
public class EchoState
{
    public int TurnNumber { get; set; }
}
``` 

# <a name="javascripttabjs"></a>[<span data-ttu-id="4ba49-120">JavaScript</span><span class="sxs-lookup"><span data-stu-id="4ba49-120">JavaScript</span></span>](#tab/js)

<span data-ttu-id="4ba49-121">Defina el proveedor de almacenamiento y asígnelo al administrador de estado que quiera usar.</span><span class="sxs-lookup"><span data-stu-id="4ba49-121">Define the storage provider and assign it to the state manager you want to use.</span></span>
<span data-ttu-id="4ba49-122">Puede usar el mismo proveedor de almacenamiento para el software intermedio de administración de `ConversationState` y `UserState`.</span><span class="sxs-lookup"><span data-stu-id="4ba49-122">You could use the same storage provider for `ConversationState` and `UserState` management middleware.</span></span>
<span data-ttu-id="4ba49-123">Después, se usaría la biblioteca `BotStateSet` para conectarlo a la capa de software intermedio que va a administrar la persistencia de los datos de forma automática.</span><span class="sxs-lookup"><span data-stu-id="4ba49-123">You would then use the `BotStateSet` library to connect it to the middleware layer that will manage the persisting of data for you.</span></span>

```javascript
const { BotFrameworkAdapter, MemoryStorage, ConversationState, UserState, BotStateSet } = require('botbuilder');
const restify = require('restify');

// Create adapter
const adapter = new BotFrameworkAdapter({ 
    appId: process.env.MICROSOFT_APP_ID, 
    appPassword: process.env.MICROSOFT_APP_PASSWORD 
});

// Add conversation state middleware.
const conversationState = new ConversationState(new MemoryStorage());
adapter.use(conversationState);

// Alternatively, use both conversation and user state middleware.
// const storage = new MemoryStorage;
// const conversationState = new ConverstationState(storage);
// const userState = new UserState(storage);
// adapter.use(new BotStateSet(conversationState, userState));
```    
---

> [!NOTE] 
> <span data-ttu-id="4ba49-124">El almacenamiento de datos en memoria solo está pensado para pruebas.</span><span class="sxs-lookup"><span data-stu-id="4ba49-124">In-memory data storage is intended for testing only.</span></span> <span data-ttu-id="4ba49-125">Este tipo de almacenamiento es volátil y temporal.</span><span class="sxs-lookup"><span data-stu-id="4ba49-125">This storage is volatile and temporary.</span></span> <span data-ttu-id="4ba49-126">Los datos se borran cada vez que se reinicia el bot.</span><span class="sxs-lookup"><span data-stu-id="4ba49-126">The data is cleared each time the bot is restarted.</span></span> <span data-ttu-id="4ba49-127">Vea [Almacenamiento de archivos](#file-storage) y [Almacenamiento de tablas de Azure](#azure-table-storage) más adelante en este artículo para configurar otros medios de almacenamiento subyacentes para el estado de la conversación y el usuario.</span><span class="sxs-lookup"><span data-stu-id="4ba49-127">See [File storage](#file-storage) and [Azure table storage](#azure-table-storage) later in this article to set up other underlying storage mediums for conversation state and user state.</span></span> 

### <a name="configuring-state-manager-middleware"></a><span data-ttu-id="4ba49-128">Configuración del software intermedio de administrador de estado</span><span class="sxs-lookup"><span data-stu-id="4ba49-128">Configuring state manager middleware</span></span>

<span data-ttu-id="4ba49-129">Al inicializar el software intermedio de estado, un parámetro de _configuración de estado_ opcional permite cambiar el comportamiento predeterminado de cómo se guardan las propiedades.</span><span class="sxs-lookup"><span data-stu-id="4ba49-129">When initializing the state middleware, an optional _state settings_ parameter allows you to change the default behavior of how properties are saved.</span></span> <span data-ttu-id="4ba49-130">La configuración predeterminada es la siguiente:</span><span class="sxs-lookup"><span data-stu-id="4ba49-130">The default settings are:</span></span>

* <span data-ttu-id="4ba49-131">Conservar las propiedades más allá de la duración del contexto de turno.</span><span class="sxs-lookup"><span data-stu-id="4ba49-131">Persist properties beyond the lifetime of the turn context.</span></span>
* <span data-ttu-id="4ba49-132">Si más de una instancia del bot escribe en una propiedad, se permite que la última instancia del bot sobrescriba la anterior.</span><span class="sxs-lookup"><span data-stu-id="4ba49-132">If more than one instance of the bot writes to a property, allow the last instance of the bot to overwrite the previous one.</span></span>

## <a name="use-conversation-and-user-state-properties"></a><span data-ttu-id="4ba49-133">Uso de propiedades de estado de conversación y usuario</span><span class="sxs-lookup"><span data-stu-id="4ba49-133">Use conversation and user state properties</span></span> 
<!-- middleware and message context properties -->

<span data-ttu-id="4ba49-134">Una vez que se ha configurado el software intermedio de administrador de estado, puede obtener las propiedades de estado de usuario y conversación del objeto de contexto.</span><span class="sxs-lookup"><span data-stu-id="4ba49-134">Once the state manager middleware has been configured, you can get the conversation state and user state properties from the context object.</span></span>
<!-- Changes are written to storage before the `SendActivity()` pipeline completes. -->

# <a name="ctabcsharp"></a>[<span data-ttu-id="4ba49-135">C#</span><span class="sxs-lookup"><span data-stu-id="4ba49-135">C#</span></span>](#tab/csharp)

<span data-ttu-id="4ba49-136">Puede ver cómo funciona mediante el ejemplo `Microsoft.Bot.Samples.EchoBot` del SDK de Bot Builder.</span><span class="sxs-lookup"><span data-stu-id="4ba49-136">You can see how this works using the `Microsoft.Bot.Samples.EchoBot` sample in the Bot Builder SDK.</span></span> 

<span data-ttu-id="4ba49-137">En el controlador `OnTurn`, `context.GetConversationState` hace que el estado de la conversación acceda a los datos que se han definido y el estado se puede modificar mediante las propiedades (en este caso, incrementando `TurnNumber`).</span><span class="sxs-lookup"><span data-stu-id="4ba49-137">In the `OnTurn` handler, `context.GetConversationState` gets the conversation state to access the data you defined, and you can modify the state by using the properties (in this case, incrementing `TurnNumber`).</span></span>

```csharp
public async Task OnTurn(ITurnContext context)
{
    // This bot is only handling Messages
    if (context.Activity.Type == ActivityTypes.Message)
    {
        // Get the conversation state from the turn context
        var state = context.GetConversationState<EchoState>();

        // Bump the turn count. 
        state.TurnCount++;

        // Echo back to the user whatever they typed.
        await context.SendActivity($"Turn {state.TurnCount}: You sent '{context.Activity.Text}'");
    }
}
```   

# <a name="javascripttabjs"></a>[<span data-ttu-id="4ba49-138">JavaScript</span><span class="sxs-lookup"><span data-stu-id="4ba49-138">JavaScript</span></span>](#tab/js)

<span data-ttu-id="4ba49-139">Puede ver cómo funciona esto mediante el EchoBot generado desde el ejemplo de generador de Yeoman.</span><span class="sxs-lookup"><span data-stu-id="4ba49-139">You can see how this works using the generated EchoBot from the Yeoman generator sample.</span></span>

<span data-ttu-id="4ba49-140">En este ejemplo de código se muestra cómo se puede almacenar un contador de turnos en el estado de la conversación.</span><span class="sxs-lookup"><span data-stu-id="4ba49-140">This code sample shows how you can store a turn counter to the conversation state.</span></span>

```javascript
const { BotFrameworkAdapter, MemoryStorage, ConversationState } = require('botbuilder');
const restify = require('restify');

// Create server
let server = restify.createServer();
server.listen(process.env.port || process.env.PORT || 3978, function () {
    console.log(`${server.name} listening to ${server.url}`);
});

// Create adapter
const adapter = new BotFrameworkAdapter({ 
    appId: process.env.MICROSOFT_APP_ID, 
    appPassword: process.env.MICROSOFT_APP_PASSWORD 
});

// Add conversation state middleware
const conversationState = new ConversationState(new MemoryStorage());
adapter.use(conversationState);

// Listen for incoming requests 
server.post('/api/messages', (req, res) => {
    // Route received request to adapter for processing
    adapter.processActivity(req, res, async (context) => {
        if (context.activity.type === 'message') {
            const state = conversationState.get(context);
            const count = state.count === undefined ? state.count = 0 : ++state.count;
            await context.sendActivity(`${count}: You said "${context.activity.text}"`);
        } else {
            await context.sendActivity(`[${context.activity.type} event detected]`);
        }
    });
});
```
---

## <a name="using-conversation-state-to-direct-conversation-flow"></a><span data-ttu-id="4ba49-141">Uso del estado de la conversación para dirigir el flujo de la conversación</span><span class="sxs-lookup"><span data-stu-id="4ba49-141">Using conversation state to direct conversation flow</span></span>

<span data-ttu-id="4ba49-142">Al diseñar un flujo de conversación, es útil definir un indicador de estado para dirigir el flujo de la conversación.</span><span class="sxs-lookup"><span data-stu-id="4ba49-142">In designing a conversation flow, it is useful to define a state flag to direct the conversation flow.</span></span> <span data-ttu-id="4ba49-143">La marca puede ser un simple tipo **booleano** o un tipo que incluya el nombre del tema actual.</span><span class="sxs-lookup"><span data-stu-id="4ba49-143">The flag can be a simple **Boolean** type or a type that includes the name of the current topic.</span></span> <span data-ttu-id="4ba49-144">La marca puede ayudar a realizar el seguimiento de dónde se encuentra en una conversación.</span><span class="sxs-lookup"><span data-stu-id="4ba49-144">The flag can help you track where in a conversation you are.</span></span> <span data-ttu-id="4ba49-145">Por ejemplo, una marca de tipo **booleano** puede indicar si se encuentra en una conversación o no.</span><span class="sxs-lookup"><span data-stu-id="4ba49-145">For example, a **Boolean** type flag can tell you whether you are in a conversation or not.</span></span> <span data-ttu-id="4ba49-146">Mientras que una propiedad de nombre de tema puede indicar en qué conversación se encuentra en la actualidad.</span><span class="sxs-lookup"><span data-stu-id="4ba49-146">While a topic name property can tell you which conversation you are currently in.</span></span>

<span data-ttu-id="4ba49-147">En el ejemplo siguiente se usa una propiedad _have asked name_ booleana para marcar cuándo el bot ha solicitado el nombre del usuario.</span><span class="sxs-lookup"><span data-stu-id="4ba49-147">The following example uses a Boolean _have asked name_ property to flag when the bot has asked the user for their name.</span></span> <span data-ttu-id="4ba49-148">Cuando se recibe el mensaje siguiente, el bot comprueba la propiedad.</span><span class="sxs-lookup"><span data-stu-id="4ba49-148">When the next message is received, the bot checks the property.</span></span> <span data-ttu-id="4ba49-149">Si la propiedad está establecida en `true`, el bot sabe que al usuario se le acaba de solicitar el nombre e interpreta el mensaje entrante como un nombre que se va a guardar como una propiedad de usuario.</span><span class="sxs-lookup"><span data-stu-id="4ba49-149">If the property is set to `true`, the bot knows the user was just asked for their name, and interprets the incoming message as a name to save as a user property.</span></span>


# <a name="ctabcsharp"></a>[<span data-ttu-id="4ba49-150">C#</span><span class="sxs-lookup"><span data-stu-id="4ba49-150">C#</span></span>](#tab/csharp)
```csharp
public class ConversationInfo
{
    public bool haveAskedNameFlag { get; set; }
    public bool haveAskedNumberFlag { get; set; }
}

public class UserInfo
{
    public string name { get; set; }
    public string telephoneNumber { get; set; }
    public bool done { get; set; }
}

public async Task OnTurn(ITurnContext context)
{
    // Get state objects. Default objects are created if they don't already exist.
    var convo = ConversationState<ConversationInfo>.Get(context);
    var user = UserState<UserInfo>.Get(context);

    if (context.Activity.Type is ActivityTypes.Message)
    {
        if (string.IsNullOrEmpty(user.name) && !convo.haveAskedNameFlag)
        {
            // Ask for the name.
            await context.SendActivity("Hello. What's your name?");

            // Set flag to show we've asked for the name. We save this out so the
            // context object for the next turn of the conversation can check haveAskedName
            convo.haveAskedNameFlag = true;
        }
        else if (!convo.haveAskedNumberFlag)
        {
            // Save the name.
            var name = context.Activity.AsMessageActivity().Text;
            user.name = name;
            convo.haveAskedNameFlag = false; // Reset flag

            // Ask for the phone number. You might want a flag to track this, too.
            await context.SendActivity($"Hello, {name}. What's your telephone number?");
            convo.haveAskedNumberFlag = true;
        }
        else if (convo.haveAskedNumberFlag)
        {
            // save the telephone number
            var telephonenumber = context.Activity.AsMessageActivity().Text;

            user.telephoneNumber = telephonenumber;
            convo.haveAskedNumberFlag = false; // Reset flag
            await context.SendActivity($"Got it. I'll call you later.");
        }
    }
}
```

<span data-ttu-id="4ba49-151">Para configurar el estado de usuario para que `UserState<UserInfo>.Get(context)` pueda devolverlo, se agrega software intermedio de estado de usuario.</span><span class="sxs-lookup"><span data-stu-id="4ba49-151">To set up user state so that it can be returned by `UserState<UserInfo>.Get(context)`, you add user state middleware.</span></span> <span data-ttu-id="4ba49-152">Por ejemplo, en `Startup.cs` de EchoBot de ASP.NET Core, cambie el código de ConfigureServices.cs:</span><span class="sxs-lookup"><span data-stu-id="4ba49-152">For example, in `Startup.cs` of the ASP .NET Core EchoBot, changing the code in ConfigureServices.cs:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddBot<EchoBot>(options =>
    {
        options.CredentialProvider = new ConfigurationCredentialProvider(Configuration);
        
        IStorage dataStore = new MemoryStorage();
        options.Middleware.Add(new ConversationState<ConversationInfo>(dataStore));
        options.Middleware.Add(new UserState<UserInfo>(dataStore));
    });
}
```

# <a name="javascripttabjs"></a>[<span data-ttu-id="4ba49-153">JavaScript</span><span class="sxs-lookup"><span data-stu-id="4ba49-153">JavaScript</span></span>](#tab/js)

<span data-ttu-id="4ba49-154">**app.js**</span><span class="sxs-lookup"><span data-stu-id="4ba49-154">**app.js**</span></span>

```js
const { BotFrameworkAdapter, MemoryStorage, ConversationState, UserState, BotStateSet } = require('botbuilder');
const restify = require('restify');

// Create server
let server = restify.createServer();
server.listen(process.env.port || process.env.PORT || 3978, function () {
    console.log(`${server.name} listening to ${server.url}`);
});

// Create adapter (it's ok for MICROSOFT_APP_ID and MICROSOFT_APP_PASSWORD to be blank for now)  
const adapter = new BotFrameworkAdapter({ 
    appId: process.env.MICROSOFT_APP_ID, 
    appPassword: process.env.MICROSOFT_APP_PASSWORD 
});

// Storage
const storage = new MemoryStorage();
const conversationState = new ConversationState(storage);
const userState  = new UserState(storage);
adapter.use(new BotStateSet(conversationState, userState));

// Listen for incoming requests 
server.post('/api/messages', (req, res) => {
    // Route received request to adapter for processing
    adapter.processActivity(req, res, async (context) => {
        const isMessage = (context.activity.type === 'message');
        const convo = conversationState.get(context);
        const user = userState.get(context);

        if (isMessage) {
            if(!user.name && !convo.haveAskedNameFlag){
                // Ask for the name.
                await context.sendActivity("What is your name?")
                // Set flag to show we've asked for the name. We save this out so the
                // context object for the next turn of the conversation can check haveAskedNameFlag
                convo.haveAskedNameFlag = true;
            } else if(convo.haveAskedNameFlag){
                // Save the name.
                user.name = context.activity.text;
                convo.haveAskedNameFlag = false; // Reset flag

                await context.sendActivity(`Hello, ${user.name}. What's your telephone number?`);
                convo.haveAskedNumberFlag = true; // Set flag
            } else if(convo.haveAskedNumberFlag){
                // save the phone number
                user.telephonenumber = context.activity.text;
                convo.haveAskedNumberFlag = false; // Reset flag
                await context.sendActivity(`Got it. I'll call you later.`);
            }
        }

        // ...
    });
});

```

---

<span data-ttu-id="4ba49-155">Una alternativa consiste en usar el modelo de _cascada_ de un cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="4ba49-155">An alternative is to use the _waterfall_ model of a dialog.</span></span> <span data-ttu-id="4ba49-156">El cuadro de diálogo realiza el seguimiento del estado de la conversación de forma automática, por lo que no es necesario crear marcas para realizar el seguimiento del estado.</span><span class="sxs-lookup"><span data-stu-id="4ba49-156">The dialog keeps track of the conversation state for you so you do not need to create flags to track your state.</span></span> <span data-ttu-id="4ba49-157">Para más información, consulte [Administración de conversaciones simples con diálogos](bot-builder-dialog-manage-conversation-flow.md).</span><span class="sxs-lookup"><span data-stu-id="4ba49-157">For more information, see [Manage simple conversation with Dialogs](bot-builder-dialog-manage-conversation-flow.md).</span></span>

## <a name="file-storage"></a><span data-ttu-id="4ba49-158">File Storage</span><span class="sxs-lookup"><span data-stu-id="4ba49-158">File storage</span></span>

<span data-ttu-id="4ba49-159">El proveedor de almacenamiento de memoria usa el almacenamiento en memoria que se elimina cuando se reinicia el bot.</span><span class="sxs-lookup"><span data-stu-id="4ba49-159">The memory storage provider uses in-memory storage that gets disposed when the bot is restarted.</span></span> <span data-ttu-id="4ba49-160">Solo es adecuado con fines de prueba.</span><span class="sxs-lookup"><span data-stu-id="4ba49-160">It is good for testing purposes only.</span></span> <span data-ttu-id="4ba49-161">Si quiere conservar los datos, pero no quiere enlazar el bot a una base de datos, puede usar el proveedor de almacenamiento de archivos.</span><span class="sxs-lookup"><span data-stu-id="4ba49-161">If you want to persist data but do not want to hook your bot up to a database, you can use the file storage provider.</span></span> <span data-ttu-id="4ba49-162">Aunque este proveedor también está diseñado con fines de prueba, conserva los datos de estado en un archivo que se puede inspeccionar.</span><span class="sxs-lookup"><span data-stu-id="4ba49-162">While this provider is also intented for testing purposes, it persists state data to a file so that you can inspect it.</span></span> <span data-ttu-id="4ba49-163">Los datos se escriben en el archivo con formato JSON.</span><span class="sxs-lookup"><span data-stu-id="4ba49-163">The data is written out to file using JSON format.</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="4ba49-164">C#</span><span class="sxs-lookup"><span data-stu-id="4ba49-164">C#</span></span>](#tab/csharp)

<span data-ttu-id="4ba49-165">Vaya a `Startup.cs` en el ejemplo Microsoft.Bot.Samples.EchoBot-AspNetCore y modifique el código del método `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="4ba49-165">Go to `Startup.cs` in the Microsoft.Bot.Samples.EchoBot-AspNetCore sample, and edit the code in the `ConfigureServices` method.</span></span>
```csharp
// This method gets called by the runtime. Use this method to add services to the container.
public void ConfigureServices(IServiceCollection services)
{
    services.AddBot<EchoBot>(options =>
    {
        options.CredentialProvider = new ConfigurationCredentialProvider(Configuration);

        // Using file storage instead of in-memory storage.
        IStorage dataStore = new FileStorage(System.IO.Path.GetTempPath());
        options.Middleware.Add(new ConversationState<EchoState>(dataStore));
    });
}
``` 

<span data-ttu-id="4ba49-166">Ejecute el código y deje que EchoBot devuelva la entrada varias veces.</span><span class="sxs-lookup"><span data-stu-id="4ba49-166">Run the code, and let the echobot echo back your input a few times.</span></span>

<span data-ttu-id="4ba49-167">Después, vaya al directorio especificado por `System.IO.Path.GetTempPath()`.</span><span class="sxs-lookup"><span data-stu-id="4ba49-167">Then go to the directory specified by `System.IO.Path.GetTempPath()`.</span></span> <span data-ttu-id="4ba49-168">Debería ver un archivo con un nombre que empieza por "conversation".</span><span class="sxs-lookup"><span data-stu-id="4ba49-168">You should see a file with a name starting with "conversation".</span></span> <span data-ttu-id="4ba49-169">Ábralo y examine el código JSON.</span><span class="sxs-lookup"><span data-stu-id="4ba49-169">Open it and look at the JSON.</span></span> <span data-ttu-id="4ba49-170">El archivo contiene algo similar a lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="4ba49-170">The file contains something like the following:</span></span>
```json
{
  "$type": "Microsoft.Bot.Samples.Echo.EchoState, Microsoft.Bot.Samples.EchoBot",
  "TurnNumber": "3",
  "eTag": "ecfe2a23566b4b52b2fe697cffc59385"
}
```

<span data-ttu-id="4ba49-171">`$type` especifica el tipo de estructura de datos que se usa en el bot para almacenar el estado de la conversación.</span><span class="sxs-lookup"><span data-stu-id="4ba49-171">The `$type` specifies the type of the data structure you're using in your bot to store conversation state.</span></span> <span data-ttu-id="4ba49-172">El campo `TurnNumber` se corresponde con la propiedad `TurnNumber` de la clase `EchoState`.</span><span class="sxs-lookup"><span data-stu-id="4ba49-172">The `TurnNumber` field corresponds to the `TurnNumber` property in the `EchoState` class.</span></span> <span data-ttu-id="4ba49-173">El campo `eTag` se hereda de `IStoreItem` y es un valor único que se actualiza de forma automática cada vez que el bot actualiza el estado de la conversación.</span><span class="sxs-lookup"><span data-stu-id="4ba49-173">The `eTag` field is inherited from `IStoreItem` and is a unique value that automatically gets updated each time your bot updates conversation state.</span></span>  <span data-ttu-id="4ba49-174">El campo eTag permite al bot habilitar la simultaneidad optimista.</span><span class="sxs-lookup"><span data-stu-id="4ba49-174">The eTag field enables your bot to enable optimistic concurrency.</span></span>

# <a name="javascripttabjs"></a>[<span data-ttu-id="4ba49-175">JavaScript</span><span class="sxs-lookup"><span data-stu-id="4ba49-175">JavaScript</span></span>](#tab/js)

<span data-ttu-id="4ba49-176">Para usar `FileStorage`, actualice el ejemplo EchoBot que se describe en la sección [Uso de propiedades de estado de usuario y conversación](#use-conversation-and-user-state-properties) anterior.</span><span class="sxs-lookup"><span data-stu-id="4ba49-176">To use `FileStorage`, update your echo bot sample described in section: [Use conversation and user state properties](#use-conversation-and-user-state-properties) earlier.</span></span> <span data-ttu-id="4ba49-177">Asegúrese de que `storage` está establecido en `FileStorage` en lugar de `MemoryStorage` y que requiere `FileStorage` de botbuilder.</span><span class="sxs-lookup"><span data-stu-id="4ba49-177">Make sure `storage` is set to `FileStorage` instead of `MemoryStorage` and require `FileStorage` from botbuilder.</span></span> <span data-ttu-id="4ba49-178">Estos son los únicos cambios necesarios.</span><span class="sxs-lookup"><span data-stu-id="4ba49-178">These are the only changes needed.</span></span> 

```javascript
// Storage
const storage = new FileStorage("c:/temp");
const conversationState = new ConversationState(storage);
const userState  = new UserState(storage);
adapter.use(new BotStateSet(conversationState, userState));
```

<span data-ttu-id="4ba49-179">El proveedor `FileStorage` toma una "ruta de acceso" como parámetro.</span><span class="sxs-lookup"><span data-stu-id="4ba49-179">The `FileStorage` provider takes a "path" as a parameter.</span></span> <span data-ttu-id="4ba49-180">Especificar una ruta de acceso permite encontrar fácilmente el archivo con la información guardada desde el bot.</span><span class="sxs-lookup"><span data-stu-id="4ba49-180">Specifying a path allows you to easily find the file with the persisted information from your bot.</span></span> <span data-ttu-id="4ba49-181">Se creará un archivo para cada *conversación*.</span><span class="sxs-lookup"><span data-stu-id="4ba49-181">Each *conversation* will have a new file created for it.</span></span> <span data-ttu-id="4ba49-182">Por tanto, en la *ruta de acceso*, encontrará varios nombres de archivo que empiezan por `conversation!`.</span><span class="sxs-lookup"><span data-stu-id="4ba49-182">So, in the *path*, you may find multiple file names starting with `conversation!`.</span></span> <span data-ttu-id="4ba49-183">Puede ordenar por fecha para encontrar la conversación más reciente más fácilmente.</span><span class="sxs-lookup"><span data-stu-id="4ba49-183">You can sort by date to find the lastest conversation easier.</span></span> <span data-ttu-id="4ba49-184">Por otro lado, solo encontrará un archivo para el estado del *usuario*.</span><span class="sxs-lookup"><span data-stu-id="4ba49-184">On the other hand, you will only find one file for the *user* state.</span></span> <span data-ttu-id="4ba49-185">El nombre de archivo comenzará por `user!`.</span><span class="sxs-lookup"><span data-stu-id="4ba49-185">The filename will start with `user!`.</span></span> <span data-ttu-id="4ba49-186">Siempre que cambie el estado de cualquiera de estos objetos, el administrador de estado actualizará el archivo para reflejar lo que ha cambiado.</span><span class="sxs-lookup"><span data-stu-id="4ba49-186">Anytime the state of either of these object changes, the state manager will update the file to reflect what's changed.</span></span>

<span data-ttu-id="4ba49-187">Ejecute el bot y envíele varios mensajes.</span><span class="sxs-lookup"><span data-stu-id="4ba49-187">Run the bot and send it a few messages.</span></span> <span data-ttu-id="4ba49-188">Después, busque el archivo de almacenamiento y ábralo.</span><span class="sxs-lookup"><span data-stu-id="4ba49-188">Then, find the storage file and open it.</span></span> <span data-ttu-id="4ba49-189">Este es el aspecto que es posible que tenga el contenido de JSON para el bot de eco que realiza el seguimiento de los contadores de turnos.</span><span class="sxs-lookup"><span data-stu-id="4ba49-189">Here is what the JSON content might look like for the echo bot that keeps track of the turn counter.</span></span>

```json
{
  "turnNumber": "3",
  "eTag": "322"
}
```
---

## <a name="azure-table-storage"></a><span data-ttu-id="4ba49-190">Almacenamiento de tablas de Azure</span><span class="sxs-lookup"><span data-stu-id="4ba49-190">Azure table storage</span></span>

<span data-ttu-id="4ba49-191">También puede usar Azure Table Storage como el medio de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="4ba49-191">You can also use Azure Table storage as your storage medium.</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="4ba49-192">C#</span><span class="sxs-lookup"><span data-stu-id="4ba49-192">C#</span></span>](#tab/csharp)

<span data-ttu-id="4ba49-193">En el ejemplo Microsoft.Bot.Samples.EchoBot-AspNetCore, agregue una referencia al paquete NuGet `Microsoft.Bot.Builder.Azure`.</span><span class="sxs-lookup"><span data-stu-id="4ba49-193">In the Microsoft.Bot.Samples.EchoBot-AspNetCore sample, add a reference to the `Microsoft.Bot.Builder.Azure` NuGet package.</span></span>

<span data-ttu-id="4ba49-194">Después, vaya a `Startup.cs`, agregue una instrucción `using Microsoft.Bot.Builder.Azure;` y modifique el código del método `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="4ba49-194">Then, go to `Startup.cs`, add a `using Microsoft.Bot.Builder.Azure;` statement, and edit the code in the `ConfigureServices` method.</span></span>
```csharp
services.AddBot<EchoBot>(options =>
{
    options.CredentialProvider = new ConfigurationCredentialProvider(Configuration);
    // The parameters are the connection string and table name.
    // "UseDevelopmentStorage=true" is the connection string to use if you are using the Azure Storage Emulator.
    // Replace it with your own connection string if you're not using the emulator
    options.Middleware.Add(new ConversationState<EchoState>(new AzureTableStorage("UseDevelopmentStorage=true","conversationstatetable")));
    // you could also specify the cloud storage account instead of the connection string
    /* options.Middleware.Add(new ConversationState<EchoState>(
        new AzureTableStorage(WindowsAzure.Storage.CloudStorageAccount.DevelopmentStorageAccount, "conversationstatetable"))); */
    options.EnableProactiveMessages = true;
});
```
<span data-ttu-id="4ba49-195">`UseDevelopmentStorage=true` es la cadena de conexión que se puede usar con el [Emulador de Azure Storage][AzureStorageEmulator].</span><span class="sxs-lookup"><span data-stu-id="4ba49-195">`UseDevelopmentStorage=true` is the connection string you can use with the [Azure Storage Emulator][AzureStorageEmulator].</span></span> <span data-ttu-id="4ba49-196">Reemplácela por una cadena de conexión propia si no usa el emulador.</span><span class="sxs-lookup"><span data-stu-id="4ba49-196">Replace it with your own connection string if you're not using the emulator.</span></span>

<span data-ttu-id="4ba49-197">Si no existe la tabla con el nombre especificado en el constructor para `AzureTableStorage`, se crea.</span><span class="sxs-lookup"><span data-stu-id="4ba49-197">If the table with the name you specify in the constructor to `AzureTableStorage` doesn't exist, it is created.</span></span>

<!-- 
TODO: step-by-step inspection of the stored table
-->

# <a name="javascripttabjs"></a>[<span data-ttu-id="4ba49-198">JavaScript</span><span class="sxs-lookup"><span data-stu-id="4ba49-198">JavaScript</span></span>](#tab/js)

<span data-ttu-id="4ba49-199">En `app.js` del ejemplo EchoBot, puede crear ConversationState mediante `AzureTableStorage`.</span><span class="sxs-lookup"><span data-stu-id="4ba49-199">In `app.js` of the echobot sample, you can create ConversationState using `AzureTableStorage`.</span></span> 

```bash
npm install --save botbuilder-azure@preview
```

```javascript
const { BotFrameworkAdapter, FileStorage, MemoryStorage, ConversationState } = require('botbuilder');
const { TableStorage } = require('botbuilder-azure');

// ...

// Create adapter
const adapter = new BotFrameworkAdapter({
    appId: process.env.MICROSOFT_APP_ID,
    appPassword: process.env.MICROSOFT_APP_PASSWORD
});

// Add conversation state middleware
// The parameters are the connection string and table name.
// "UseDevelopmentStorage=true" is the connection string to use if you are using the Azure Storage Emulator.
// Replace it with your own connection string if you're not using the emulator
var azureStorage = new TableStorage({ tableName: "TestAzureTable1", storageAccountOrConnectionString: "UseDevelopmentStorage=true"})

// You can alternatively use your account name and table name
// var azureStorage = new TableStorage({tableName: "TestAzureTable2", storageAccessKey: "V3ah0go4DLkMtQKUPC6EbgFeXnE6GeA+veCwDNFNcdE6rqSVE/EQO/kjfemJaitPwtAkmR9lMKLtcvgPhzuxZg==", storageAccountOrConnectionString: "storageaccount"});

const conversationState = new ConversationState(azureStorage);
adapter.use(conversationState);

```

<span data-ttu-id="4ba49-200">`UseDevelopmentStorage=true` es la cadena de conexión que se puede usar con el [Emulador de Azure Storage][AzureStorageEmulator].</span><span class="sxs-lookup"><span data-stu-id="4ba49-200">`UseDevelopmentStorage=true` is the connection string you can use with the [Azure Storage Emulator][AzureStorageEmulator].</span></span>
<span data-ttu-id="4ba49-201">Si no existe la tabla con el nombre especificado en el constructor para `AzureTableStorage`, se crea.</span><span class="sxs-lookup"><span data-stu-id="4ba49-201">If the table with the name you specify in the constructor to `AzureTableStorage` doesn't exist, it is created.</span></span>

---

<span data-ttu-id="4ba49-202">Para ver los datos de estado de la conversación que se guardan, ejecute el ejemplo y, después, abra la tabla mediante el [Explorador de Azure Storage][AzureStorageExplorer].</span><span class="sxs-lookup"><span data-stu-id="4ba49-202">To look at the conversation state data that is saved, run the sample and then open the table using [Azure Storage explorer][AzureStorageExplorer].</span></span>

![Datos de estado de la conversación de echobot en el Explorador de Azure Storage](media/how-to-state/echostate-azure-storage-explorer.png)


<span data-ttu-id="4ba49-204">La **clave de partición** es una clave generada de forma exclusiva específica de la conversación actual.</span><span class="sxs-lookup"><span data-stu-id="4ba49-204">The **Partition Key** is a uniquely-generated key specific to the current conversation.</span></span> <span data-ttu-id="4ba49-205">Si reinicia el bot o inicia una conversación nueva, la nueva conversación obtendrá su propia fila con su propia clave de partición.</span><span class="sxs-lookup"><span data-stu-id="4ba49-205">If you restart the bot or start a new conversation, the new conversation will get its own row with its own partition key.</span></span> <span data-ttu-id="4ba49-206">La estructura de datos `EchoState` se serializa en JSON y se guarda en la columna **Json** de la tabla de Azure.</span><span class="sxs-lookup"><span data-stu-id="4ba49-206">The `EchoState` data structure is serialized to JSON and saved in the **Json** column of the Azure Table.</span></span> 

```json
{
    "$type":"Microsoft.Bot.Samples.Echo.AspNetCore.EchoState, Microsoft.Bot.Samples.EchoBot-AspNetCore",
    "TurnNumber":2,
    "LastMessage":"second message",
    "eTag":"*"
}
```

<span data-ttu-id="4ba49-207">Los campos `$type` y `eTag` se agregan mediante el SDK de BotBuilder.</span><span class="sxs-lookup"><span data-stu-id="4ba49-207">The `$type` and `eTag` fields are added by the BotBuilder SDK.</span></span> <span data-ttu-id="4ba49-208">Para obtener más información sobre las eTag, vea [Managing optimistic concurrency](bot-builder-howto-v4-storage.md#manage-concurrency-using-etags) (Administración de la simultaneidad optimista).</span><span class="sxs-lookup"><span data-stu-id="4ba49-208">For more about eTags, see [Managing optimistic concurrency](bot-builder-howto-v4-storage.md#manage-concurrency-using-etags)</span></span>


## <a name="next-steps"></a><span data-ttu-id="4ba49-209">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="4ba49-209">Next steps</span></span>

<span data-ttu-id="4ba49-210">Ahora que sabe cómo usar el estado para leer y escribir datos de bot en el almacenamiento, veremos cómo puede leer y escribir directamente en el almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="4ba49-210">Now that you know how to use state to help you read and write bot data to storage, lets take a look at how you can read and write directly to storage.</span></span>

> [!div class="nextstepaction"]
> <span data-ttu-id="4ba49-211">[Escritura directa en el almacenamiento](bot-builder-howto-v4-storage.md).</span><span class="sxs-lookup"><span data-stu-id="4ba49-211">[How to write directly to storage](bot-builder-howto-v4-storage.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4ba49-212">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="4ba49-212">Additional resources</span></span>
<span data-ttu-id="4ba49-213">Para obtener más información sobre el almacenamiento, vea [Almacenamiento en el SDK de Bot Builder](bot-builder-storage-concept.md)</span><span class="sxs-lookup"><span data-stu-id="4ba49-213">For more background on storage, see [Storage in the Bot Builder SDK](bot-builder-storage-concept.md)</span></span>

<!-- Links -->
[AzureStorageEmulator]: https://docs.microsoft.com/azure/storage/common/storage-use-emulator
[AzureStorageExplorer]: https://azure.microsoft.com/features/storage-explorer/
