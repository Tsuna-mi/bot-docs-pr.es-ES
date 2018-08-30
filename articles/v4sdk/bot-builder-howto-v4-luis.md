---
title: Uso de LUIS para la comprensión del lenguaje | Microsoft Docs
description: Obtenga información sobre cómo usar LUIS para la comprensión del lenguaje natural con Bot Builder SDK.
keywords: Language Understanding, LUIS, reconocimiento de intenciones, entidades, software intermedio
author: ivorb
ms.author: v-demak
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 4/30/17
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: 88651a2c86698d55e3a429d7ea62662976d2115f
ms.sourcegitcommit: 2dc75701b169d822c9499e393439161bc87639d2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42906169"
---
# <a name="using-luis-for-language-understanding"></a><span data-ttu-id="4a676-104">Uso de LUIS para la comprensión del lenguaje</span><span class="sxs-lookup"><span data-stu-id="4a676-104">Using LUIS for Language Understanding</span></span>

[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]

<span data-ttu-id="4a676-105">La tarea de entender qué quiere decir el usuario conversacionalmente y contextualmente puede ser difícil, pero hace que la conversación del bot parezca más natural.</span><span class="sxs-lookup"><span data-stu-id="4a676-105">The ability to understand what your user means conversationally and contextually can be a difficult task, but can give your bot a more natural conversation feel.</span></span> <span data-ttu-id="4a676-106">Language Understanding, denominado LUIS, le permite hacer precisamente esto, de modo que el bot pueda reconocer la intención de los mensajes de usuario, permitir al usuario emplear un lenguaje más natural y dirigir mejor el flujo de conversación.</span><span class="sxs-lookup"><span data-stu-id="4a676-106">Language Understanding, called LUIS, enables you to do just that so that your bot can recognize the intent of user messages, allow for more natural language from your user, and better direct the conversation flow.</span></span> <span data-ttu-id="4a676-107">Si necesita más información sobre cómo se integra LUIS con un bot, vea [Language understanding for bots](./bot-builder-concept-LUIS.md) (Language Understanding para bots).</span><span class="sxs-lookup"><span data-stu-id="4a676-107">If you need more background on how LUIS integrates with a bot, see [language understanding for bots](./bot-builder-concept-LUIS.md).</span></span> 

<span data-ttu-id="4a676-108">Este tema le guía por el proceso de configurar bots simples que usan LUIS para reconocer algunas intenciones diferentes.</span><span class="sxs-lookup"><span data-stu-id="4a676-108">This topic walks you through setting up simple bots that use LUIS to recognize a few different intents.</span></span>

## <a name="installing-packages"></a><span data-ttu-id="4a676-109">Instalar paquetes</span><span class="sxs-lookup"><span data-stu-id="4a676-109">Installing Packages</span></span>

<span data-ttu-id="4a676-110">En primer lugar, asegúrese de que tiene los paquetes necesarios para LUIS.</span><span class="sxs-lookup"><span data-stu-id="4a676-110">First, make sure you have the packages necessary for LUIS.</span></span>

# <a name="ctabcs"></a>[<span data-ttu-id="4a676-111">C#</span><span class="sxs-lookup"><span data-stu-id="4a676-111">C#</span></span>](#tab/cs)

<span data-ttu-id="4a676-112">[Agregue una referencia](https://docs.microsoft.com/en-us/nuget/tools/package-manager-ui) a la versión preliminar v4 de los siguientes paquetes NuGet:</span><span class="sxs-lookup"><span data-stu-id="4a676-112">[Add a reference](https://docs.microsoft.com/en-us/nuget/tools/package-manager-ui) to v4 prerelease version of the following NuGet packages:</span></span>


* `Microsoft.Bot.Builder.Ai.LUIS`

# <a name="javascripttabjs"></a>[<span data-ttu-id="4a676-113">JavaScript</span><span class="sxs-lookup"><span data-stu-id="4a676-113">JavaScript</span></span>](#tab/js)

<span data-ttu-id="4a676-114">Puede agregar una referencia al paquete botbuilder y botbuilder-ai en el proyecto a través de npm:</span><span class="sxs-lookup"><span data-stu-id="4a676-114">You can add reference to botbuilder and botbuilder-ai package in your project via npm:</span></span>

* `npm install --save botbuilder@preview`
* `npm install --save botbuilder-ai@preview`

---


## <a name="set-up-middleware-to-use-your-luis-app"></a><span data-ttu-id="4a676-115">Configurar el software intermedio para que use la aplicación de LUIS</span><span class="sxs-lookup"><span data-stu-id="4a676-115">Set up middleware to use your LUIS app</span></span>

<span data-ttu-id="4a676-116">En primer lugar, configure una _aplicación de LUIS_, que es un servicio que se crea en [www.luis.ai](https://www.luis.ai).</span><span class="sxs-lookup"><span data-stu-id="4a676-116">First, set up a _LUIS app_, which is a service you create at [www.luis.ai](https://www.luis.ai).</span></span> <span data-ttu-id="4a676-117">Dicha aplicación de LUIS se puede entrenar para que pueda reconocer determinadas intenciones.</span><span class="sxs-lookup"><span data-stu-id="4a676-117">That LUIS app can be trained for certain intents it should be able to recognize.</span></span> <span data-ttu-id="4a676-118">Encontrará los detalles sobre cómo crear una aplicación de LUIS en el [sitio de LUIS](https://www.luis.ai).</span><span class="sxs-lookup"><span data-stu-id="4a676-118">Details on how to create your LUIS app can be found on the [LUIS site](https://www.luis.ai).</span></span>

<span data-ttu-id="4a676-119">En este ejemplo, solo usará una aplicación de LUIS de demostración que puede reconocer las intenciones Help, Cancel y Weather; el identificador de aplicación ya está en el código de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="4a676-119">For this example, you'll just use a demo LUIS app that can recognize Help, Cancel, and Weather intents; the app ID is already in the sample code.</span></span> <span data-ttu-id="4a676-120">Debe tener una clave de Cognitive Services. Para obtenerla, inicie sesión en [www.luis.ai](https://www.luis.ai) y copie la clave de **Configuración de usuario** > **Authoring Key** (Clave de creación).</span><span class="sxs-lookup"><span data-stu-id="4a676-120">You will need to have a Cognitive Services key that you can get by logging in to [www.luis.ai](https://www.luis.ai) and copying the key from **User settings** > **Authoring Key**.</span></span>

> [!NOTE] 
> <span data-ttu-id="4a676-121">Para crear su propia copia de la aplicación de LUIS pública usada en este ejemplo, copie el archivo de LUIS [FirstSimpleBotExample.json](https://github.com/Microsoft/LUIS-Samples/blob/master/examples/simple-bot-example/FirstSimpleBotExample.json).</span><span class="sxs-lookup"><span data-stu-id="4a676-121">To create your own copy of the public LUIS app used in this example, copy the [FirstSimpleBotExample.json](https://github.com/Microsoft/LUIS-Samples/blob/master/examples/simple-bot-example/FirstSimpleBotExample.json) LUIS file.</span></span> <span data-ttu-id="4a676-122">Después, [importe la aplicación de LUIS](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/create-new-app#import-new-app), [entrénela](https://docs.microsoft.com/en-us/azure/cognitive-services/LUIS/luis-how-to-train) y [publíquela](https://docs.microsoft.com/en-us/azure/cognitive-services/LUIS/publishapp).</span><span class="sxs-lookup"><span data-stu-id="4a676-122">Then [import the LUIS app](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/create-new-app#import-new-app), [train](https://docs.microsoft.com/en-us/azure/cognitive-services/LUIS/luis-how-to-train), and [publish](https://docs.microsoft.com/en-us/azure/cognitive-services/LUIS/publishapp) it.</span></span> <span data-ttu-id="4a676-123">Reemplace el identificador de aplicación pública del código de ejemplo por el identificador de aplicación de la nueva aplicación de LUIS.</span><span class="sxs-lookup"><span data-stu-id="4a676-123">Replace the public app ID in the sample code with the app ID of your new LUIS app.</span></span>

<span data-ttu-id="4a676-124">Configure el bot de modo que llame a la aplicación de LUIS para cada mensaje que reciba de un usuario. Para ello, basta con agregarlo a la pila de software intermedio del bot.</span><span class="sxs-lookup"><span data-stu-id="4a676-124">Configure your bot to call your LUIS app for every message received from a user by simply adding it to your bot's middleware stack.</span></span> <span data-ttu-id="4a676-125">El software intermedio almacena los resultados del reconocimiento en el objeto de contexto, al que después puede acceder la lógica del bot.</span><span class="sxs-lookup"><span data-stu-id="4a676-125">The middleware stores the recognition results on the context object, and can then be accessed by your bot logic.</span></span>

# <a name="ctabcs"></a>[<span data-ttu-id="4a676-126">C#</span><span class="sxs-lookup"><span data-stu-id="4a676-126">C#</span></span>](#tab/cs)
<span data-ttu-id="4a676-127">Empiece con la plantilla del bot de eco y abra **Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="4a676-127">Start with the Echo bot template, and open **Startup.cs**.</span></span> 

<span data-ttu-id="4a676-128">Agregue una instrucción `using` para `Microsoft.Bot.Builder.Ai.LUIS`.</span><span class="sxs-lookup"><span data-stu-id="4a676-128">Add a `using` statement for `Microsoft.Bot.Builder.Ai.LUIS`</span></span>

```csharp
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Bot.Builder.BotFramework;
using Microsoft.Bot.Builder.Integration.AspNet.Core;
// add this
using Microsoft.Bot.Builder.Ai.LUIS;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Logging;
using System;
```

<span data-ttu-id="4a676-129">Actualice el método `ConfigureServices` en su archivo `Startup.cs` para agregar un objeto `LuisRecognizerMiddleware` que se conecta con la aplicación de LUIS.</span><span class="sxs-lookup"><span data-stu-id="4a676-129">Update the `ConfigureServices` method in your `Startup.cs` file to add a `LuisRecognizerMiddleware` object that connects to your LUIS app.</span></span> 

```csharp
// This method gets called by the runtime. Use this method to add services to the container.
public void ConfigureServices(IServiceCollection services)
{
    services.AddBot<EchoBot>(options =>
    {
        options.CredentialProvider = new ConfigurationCredentialProvider(Configuration);
. 
        options.Middleware.Add(new CatchExceptionMiddleware<Exception>(async (context, exception) =>
        {
            await context.TraceActivity("EchoBot Exception", exception);
            await context.SendActivity("Sorry, it looks like something went wrong!");
        }));

        // The Memory Storage used here is for local bot debugging only. When the bot
        // is restarted, anything stored in memory will be gone. 
        IStorage dataStore = new MemoryStorage();

        options.Middleware.Add(new ConversationState<EchoState>(dataStore));

        // Add LUIS recognizer as middleware
        options.Middleware.Add(
            new LuisRecognizerMiddleware(
                new LuisModel(
                    // This appID is for a public app that's made available for demo purposes
                    "eb0bf5e0-b468-421b-9375-fdfb644c512e",
                    // You can use it by replacing <subscriptionKey> with your Authoring Key
                    // which you can find at https://www.luis.ai under User settings > Authoring Key
                    "<subscriptionKey>",
                    // The location-based URL begins with "https://<region>.api.cognitive.microsoft.com", where region is the region associated with the key you are using. Some examples of regions are `westus`, `westcentralus`, `eastus2`, and `southeastasia`.
                    new Uri("https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/"))));
    });
}
```

<span data-ttu-id="4a676-130"><!--TODO: DOES PUBLIC APP WORK WITH KEYS IN DIFFERENT REGIIONS? --> Pegue la clave de suscripción desde [https://www.luis.ai](https://www.luis.ai) en lugar de `<subscriptionKey>`.</span><span class="sxs-lookup"><span data-stu-id="4a676-130"><!--TODO: DOES PUBLIC APP WORK WITH KEYS IN DIFFERENT REGIIONS? --> Paste in your subscription key from [https://www.luis.ai](https://www.luis.ai) in place of `<subscriptionKey>`.</span></span>

> [!NOTE] 
> <span data-ttu-id="4a676-131">Si usa su propia aplicación de LUIS en lugar de la pública, puede obtener el identificador, la clave de suscripción y la dirección URL de la aplicación de LUIS en [https://www.luis.ai](https://www.luis.ai).</span><span class="sxs-lookup"><span data-stu-id="4a676-131">If you're using your own LUIS app instead of the public one, you can get the ID, subscription key, and URL for your LUIS app from [https://www.luis.ai](https://www.luis.ai).</span></span> 
>
><span data-ttu-id="4a676-132">Para encontrar la URL base a fin de usarla en `LuisModel`, inicie sesión en el [sitio de LUIS](https://www.luis.ai), vaya a la pestaña **Publicar** y consulte la columna **Punto de conexión**, situada en **Resources and Keys** (Recursos y claves).</span><span class="sxs-lookup"><span data-stu-id="4a676-132">You can find the base URL to use in your `LuisModel` by logging into the [LUIS site](https://www.luis.ai), going to the **Publish** tab, and looking at the **Endpoint** column under **Resources and Keys**.</span></span> <span data-ttu-id="4a676-133">La URL base es la parte de la **dirección URL del punto de conexión** que aparece antes del identificador de suscripción y otros parámetros.</span><span class="sxs-lookup"><span data-stu-id="4a676-133">The base URL is the portion of the **Endpoint URL** before the subscription ID and other parameters.</span></span>

# <a name="javascripttabjs"></a>[<span data-ttu-id="4a676-134">JavaScript</span><span class="sxs-lookup"><span data-stu-id="4a676-134">JavaScript</span></span>](#tab/js)

<span data-ttu-id="4a676-135">En primer lugar, solicite o importe la clase [LuisRecognizer](https://github.com/Microsoft/botbuilder-js/tree/master/doc/botbuilder-ai/classes/botbuilder_ai.luisrecognizer.md) y cree una instancia para el modelo de LUIS:</span><span class="sxs-lookup"><span data-stu-id="4a676-135">First require/import in the [LuisRecognizer](https://github.com/Microsoft/botbuilder-js/tree/master/doc/botbuilder-ai/classes/botbuilder_ai.luisrecognizer.md) class and create an instance for your LUIS model:</span></span>

```javascript
const { LuisRecognizer } = require('botbuilder-ai');

const model = new LuisRecognizer({
    // This appID is for a public app that's made available for demo purposes
    // You can use it by providing your LUIS subscription key
     appId: 'eb0bf5e0-b468-421b-9375-fdfb644c512e',
    // replace subscriptionKey with your Authoring Key
    // your key is at https://www.luis.ai under User settings > Authoring Key 
    subscriptionKey: '<your subscription key>',
    // The serviceEndpoint URL begins with "https://<region>.api.cognitive.microsoft.com", where region is the region associated with the key you are using. Some examples of regions are `westus`, `westcentralus`, `eastus2`, and `southeastasia`.
    serviceEndpoint: 'https://westus.api.cognitive.microsoft.com'
});
```

<span data-ttu-id="4a676-136">Agregue el modelo a la pila de software intermedio:</span><span class="sxs-lookup"><span data-stu-id="4a676-136">Add the model to the middleware stack:</span></span>

```javascript
adapter.use(model);
```


> [!NOTE] 
> <span data-ttu-id="4a676-137">Si usa su propia aplicación de LUIS en lugar de la pública, puede obtener el identificador, el identificador de suscripción y la dirección URL de la aplicación de LUIS en [https://www.luis.ai](https://www.luis.ai).</span><span class="sxs-lookup"><span data-stu-id="4a676-137">If you're using your own LUIS app instead of the public one, you can get the ID, subscription ID, and URL for your LUIS app from [https://www.luis.ai](https://www.luis.ai).</span></span> 

---



<span data-ttu-id="4a676-138">Language Understanding (LUIS) ya está conectado a su bot.</span><span class="sxs-lookup"><span data-stu-id="4a676-138">LUIS language understanding is now plugged into your bot.</span></span> <span data-ttu-id="4a676-139">Ahora, veamos cómo obtener la intención del modelo de LUIS almacenado en el objeto de contexto.</span><span class="sxs-lookup"><span data-stu-id="4a676-139">Next, let's look at how to get the intent from the LUIS model stored on the context object.</span></span>


## <a name="get-the-intent-from-the-turn-context"></a><span data-ttu-id="4a676-140">Obtener la intención del contexto de turno</span><span class="sxs-lookup"><span data-stu-id="4a676-140">Get the intent from the turn context</span></span>

<span data-ttu-id="4a676-141">Se puede acceder a los resultados de LUIS desde el bot si se usa el contexto en cada turno de conversación.</span><span class="sxs-lookup"><span data-stu-id="4a676-141">The results from LUIS are then accessible from your bot using the context on every conversational turn.</span></span>

# <a name="ctabcs"></a>[<span data-ttu-id="4a676-142">C#</span><span class="sxs-lookup"><span data-stu-id="4a676-142">C#</span></span>](#tab/cs)

<span data-ttu-id="4a676-143">Para que el bot simplemente envíe una respuesta basada en la intención que la aplicación de LUIS ha detectado, reemplace el código de `OnTurn` por lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="4a676-143">To have your bot simply send a reply based on the intent that the LUIS app detected, replace the code in `OnTurn` with the following:</span></span>

```cs
using System.Threading.Tasks;
using Microsoft.Bot;
using Microsoft.Bot.Builder;
using Microsoft.Bot.Builder.Ai.LUIS;
using Microsoft.Bot.Builder.Core.Extensions;
using Microsoft.Bot.Schema;

namespace Bot_Builder_Echo_Bot1
{
    public class EchoBot : IBot
    {
        /// <summary>
        /// Every Conversation turn for our EchoBot will call this method. In here
        /// the bot checks the Activty type to verify it's a message, checks the /// intent from the LUIS recognizer, and sends a reply based on the recognized intent
        /// </summary>
        /// <param name="context">Turn-scoped context containing all the data needed
        /// for processing this conversation turn. </param>        
        public async Task OnTurn(ITurnContext context)
        {
            // This bot is only handling Messages
            if (context.Activity.Type == ActivityTypes.Message)
            {

                var result = context.Services.Get<RecognizerResult>
                    (LuisRecognizerMiddleware.LuisRecognizerResultKey);
                var topIntent = result?.GetTopScoringIntent();
                switch ((topIntent != null) ? topIntent.Value.intent : null)
                {
                    case null:
                        await context.SendActivity("Failed to get results from LUIS.");
                        break;
                    case "None":
                        await context.SendActivity("Sorry, I don't understand.");
                        break;
                    case "Help":
                        await context.SendActivity("<here's some help>");
                        break;
                    case "Cancel":
                        // Cancel the process.
                        await context.SendActivity("<cancelling the process>");
                        break;
                    case "Weather":
                        // Cancel the process.
                        await context.SendActivity("The weather today is sunny.");
                        break;
                    default:
                        // Received an intent we didn't expect, so send its name and score.
                        await context.SendActivity($"Intent: {topIntent.Value.intent} ({topIntent.Value.score}).");
                        break;
                }
            }
        }
    }    
}

```

# <a name="javascripttabjs"></a>[<span data-ttu-id="4a676-144">JavaScript</span><span class="sxs-lookup"><span data-stu-id="4a676-144">JavaScript</span></span>](#tab/js)

```javascript
// Listen for incoming activity 
server.post('/api/messages', (req, res) => {
    // Route received activity to adapter for processing
    adapter.processActivity(req, res, async (context) => {
        if (context.activity.type === 'message') {
            const results = model.get(context);
            const topIntent = LuisRecognizer.topIntent(results);
            switch (topIntent) {

                case 'Cancel':
                    await context.sendActivity("<cancelling the process>")
                    break;
                case 'Help':
                    await context.sendActivity("<here's some help>");
                    break;
                case 'Weather':
                    await context.sendActivity("The weather today is sunny.");
                    break;
                case 'None':                    
                    await context.sendActivity("Sorry, I don't understand.")
                    break;
                case 'null':                    
                    await context.sendActivity("Failed to get results from LUIS.")
                    break;
                default:
                    // Received an intent we didn't expect, so send its name and score.
                    await context.sendActivity(`The top intent was ${topIntent}`);
            }
        }
    });
});
```

<span data-ttu-id="4a676-145">Todas las intenciones reconocidas en la expresión se devolverán como una asignación de nombres de intenciones a las puntuaciones, y se puede acceder a ellas desde `results.intents`.</span><span class="sxs-lookup"><span data-stu-id="4a676-145">Any intents recognized in the utterance will be returned as a map of intent names to scores and can be accessed from `results.intents`.</span></span> <span data-ttu-id="4a676-146">Se proporciona un método `LuisRecognizer.topIntent()` estático para ayudar a simplificar la búsqueda de la intención con la puntuación más alta de un conjunto de resultados.</span><span class="sxs-lookup"><span data-stu-id="4a676-146">A static `LuisRecognizer.topIntent()` method is provided to help simplify finding the top scoring intent for a result set.</span></span>
<span data-ttu-id="4a676-147">Todas las entidades reconocidas se devolverán como una asignación de nombres de entidades a valores, y se accederá a ellas a través de `results.entities`.</span><span class="sxs-lookup"><span data-stu-id="4a676-147">Any entities recognized will be returned as a map of entity names to values and accessed via `results.entities`.</span></span> <span data-ttu-id="4a676-148">Es posible devolver metadatos de entidad adicionales si se pasa un valor `verbose=true` al crear la clase LuisRecognizer.</span><span class="sxs-lookup"><span data-stu-id="4a676-148">Additional entity metadata can be returned by passing a `verbose=true` setting when creating the LuisRecognizer.</span></span> <span data-ttu-id="4a676-149">Después, se puede acceder a los metadatos agregados mediante `results.entities.$instance`.</span><span class="sxs-lookup"><span data-stu-id="4a676-149">The added metadata can then be accessed via `results.entities.$instance`.</span></span>

---

<span data-ttu-id="4a676-150"><!-- TODO: SHOW RUNNING THE FIRST BOT --> Pruebe a ejecutar el bot en Bot Framework Emulator y dígale cosas como "weather" (tiempo), "help" (ayuda) y "cancel" (cancelar).</span><span class="sxs-lookup"><span data-stu-id="4a676-150"><!-- TODO: SHOW RUNNING THE FIRST BOT --> Try running the bot in the Bot Framework Emulator, and say things like "weather", "help", and "cancel" to it.</span></span>

![ejecutar el bot](./media/how-to-luis/run-luis-bot.png)


## <a name="using-a-luis-recognizer-with-conversation-state"></a><span data-ttu-id="4a676-152">Usar un reconocedor de LUIS con estado de la conversación</span><span class="sxs-lookup"><span data-stu-id="4a676-152">Using a LUIS recognizer with conversation state</span></span>
<span data-ttu-id="4a676-153"><!-- TBD, complete example --> Si la respuesta al usuario tiene más de un solo turno, tal vez le interese llevar un seguimiento de dónde se encuentra en la conversación. Para ello, guarde el estado de la conversación.</span><span class="sxs-lookup"><span data-stu-id="4a676-153"><!-- TBD, complete example --> If your reply to the user has more than a single turn, you might decide to track where you are in the conversation by saving conversation state.</span></span> <span data-ttu-id="4a676-154">Puede usar la intención de LUIS como ayuda para establecer los datos del estado de la conversación, por ejemplo, si un tema de la conversación se ha iniciado o completado.</span><span class="sxs-lookup"><span data-stu-id="4a676-154">You can use the intent from LUIS to help you set conversation state data, such as whether a conversation topic has been started or completed.</span></span>

<span data-ttu-id="4a676-155">El software intermedio del reconocedor de LUIS se ejecuta para cada turno de un bot, de modo que usted pueda obtener una intención para cada mensaje que recibe el usuario.</span><span class="sxs-lookup"><span data-stu-id="4a676-155">The LUIS recognizer middleware runs for every turn of your bot, so that you can get an intent for each message received by your user.</span></span> <span data-ttu-id="4a676-156">Si quiere iniciar un flujo de conversación de varios turnos basado en una intención, una manera de hacerlo consiste en omitir la lógica para el cambio de tema hasta que haya terminado con el tema actual.</span><span class="sxs-lookup"><span data-stu-id="4a676-156">If you want to start a multi-turn conversation flow based on an intent, one way to do it is to skip the logic for changing topics until you're done with the current topic.</span></span>

# <a name="ctabcs"></a>[<span data-ttu-id="4a676-157">C#</span><span class="sxs-lookup"><span data-stu-id="4a676-157">C#</span></span>](#tab/cs)

<span data-ttu-id="4a676-158">En EchoState.cs, cambie EchoState a lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="4a676-158">In EchoState.cs, change EchoState to the following:</span></span>

```csharp
public class ConversationStateInfo 
{
    public bool WeatherTopicStarted  { get; set; }
    public bool HelpTopicStarted  { get; set; }
    public bool CancelTopicStarted  { get; set; }
}
```

<span data-ttu-id="4a676-159">En Startup.cs, cambie la inicialización de ConversationState para usar `ConversationStateInfo`.</span><span class="sxs-lookup"><span data-stu-id="4a676-159">In Startup.cs, change the initialization of ConversationState to use `ConversationStateInfo`.</span></span>
```cs
    options.Middleware.Add(new ConversationState<ConversationStateInfo>(dataStore));
```

<span data-ttu-id="4a676-160">En EchoBot.cs, edite `OnTurn`:</span><span class="sxs-lookup"><span data-stu-id="4a676-160">In EchoBot.cs, edit `OnTurn`:</span></span>
```cs
public async Task OnTurn(ITurnContext context)
{
    // This bot is only handling Messages
    if (context.Activity.Type == ActivityTypes.Message)
    {
        var text = context.Activity.Text;
        var conversationState = context.GetConversationState<ConversationStateInfo>() ?? new ConversationStateInfo();

        // Here, you can add some other logic based on the topic flags in conversation state
        // For example, if you know that a particular topic was started in a previous turn,
        // you can send the reply for that topic and bypass getting the intent from LUIS 
        if (conversationState.WeatherTopicStarted)
        {
            // Set this flag to false since this reply concludes the topic.
            conversationState.WeatherTopicStarted = false;
            // Assume that they responded to the prompt with a location.
            await context.SendActivity($"The weather in {text} is sunny.");
        }
        else
        {
            var result = context.Services.Get<RecognizerResult>
            (LuisRecognizerMiddleware.LuisRecognizerResultKey);
            var topIntent = result?.GetTopScoringIntent();
            switch ((topIntent != null) ? topIntent.Value.intent : null)
            {
                case null:
                    await context.SendActivity("Failed to get results from LUIS.");
                    break;
                case "None":
                    await context.SendActivity("Sorry, I don't understand.");
                    break;
                case "Weather":
                    conversationState.WeatherTopicStarted = true;
                    await context.SendActivity($"Looks like you want a weather forecast. What city do you want the forecast for?");

                    break;
                case "Help":
                    conversationState.HelpTopicStarted = true;
                    await context.SendActivity("<here's some help>");
                    break;
                case "Cancel":
                    // Cancel the process.
                    conversationState.CancelTopicStarted = true;
                    await context.SendActivity("<cancelling the process>");
                    break;
                default:
                    // Received an intent we didn't expect, so send its name and score.
                    await context.SendActivity($"Intent: {topIntent.Value.intent} ({topIntent.Value.score}).");
                    break;
            }
        }
    }
}
```

# <a name="javascripttabjs"></a>[<span data-ttu-id="4a676-161">JavaScript</span><span class="sxs-lookup"><span data-stu-id="4a676-161">JavaScript</span></span>](#tab/js)

<span data-ttu-id="4a676-162">Agregue software intermedio de estado de la conversación después de agregar la clase `LuisRecognizer`.</span><span class="sxs-lookup"><span data-stu-id="4a676-162">Add conversation state middleware after you add the `LuisRecognizer`.</span></span>

```javascript
const model = new LuisRecognizer({
    // This appID is for a public app that's made available for demo purposes
    // You can use it by providing your LUIS subscription key
     appId: 'eb0bf5e0-b468-421b-9375-fdfb644c512e',
    // replace subscriptionKey with your Authoring Key
    // your key is at https://www.luis.ai under User settings > Authoring Key 
    subscriptionKey: '<your subscription>',
    // The serviceEndpoint URL begins with "https://<region>.api.cognitive.microsoft.com", where region is the region associated with the key you are using. Some examples of regions are `westus`, `westcentralus`, `eastus2`, and `southeastasia`.
    serviceEndpoint: 'https://westus.api.cognitive.microsoft.com'
});
adapter.use(model)

// Add conversation state middleware
const conversationState = new ConversationState(new MemoryStorage());
adapter.use(conversationState);
```

<span data-ttu-id="4a676-163">Después, puede guardar el estado que indica qué tema se ha iniciado.</span><span class="sxs-lookup"><span data-stu-id="4a676-163">Then you can save state indicating which topic has been started.</span></span>

```javascript
// Listen for incoming activities 
server.post('/api/messages', (req, res) => {
    // Route received activity to adapter for processing
    adapter.processActivity(req, res, async(context) => {
        if (context.activity.type === 'message') {
            var utterance = context.activity.text;
            
            // Check topic flags in conversation state 
            if (conversationState.weatherTopicStarted) 
            {
                // Assume the user's message is a reply to the bot's prompt for a location
                await context.sendActivity(`The weather in ${utterance} is sunny.`);
                // This conversation flow is now finished. Set flag to false,
                // so that on the next turn the user can ask for another weather forecast.
                conversationState.WeatherTopicStarted = false;
            }
            // To add more steps to the other topics
            // you could check the topic flags here
            else 
            {
                const results = model.get(context);
                const topIntent = LuisRecognizer.topIntent(results);
                switch (topIntent) {
                    case 'None':
                        //Add app logic when there is no result
                        await context.sendActivity("<null case>")
                        break;
                    case 'Cancel':
                        conversationState.cancelTopicStarted = true;
                        await context.sendActivity("<cancelling the process>")
                        break;
                    case 'Help':
                        conversationState.helpTopicStarted = true;
                        await context.sendActivity("<here's some help>");
                        break;
                    case 'Weather':
                        conversationState.weatherTopicStarted = true;
                        await context.sendActivity("Looks like you want a weather forecast. What city do you want the forecast for?");
                        break;
                    default:
                        // Add app logic for the recognition results.
                        await context.sendActivity(`Received this intent: ${topIntent}`);
                }
            }
        }
    });
});
```

---

<span data-ttu-id="4a676-164">Pruebe a ejecutar el bot en Bot Framework Emulator. Fíjese en que "get weather" (obtener el tiempo) es ahora un flujo de conversación de dos turnos.</span><span class="sxs-lookup"><span data-stu-id="4a676-164">Try running the bot in the Bot Framework Emulator, and notice that "get weather" is now a two-turn conversation flow.</span></span>

![ejecutar el bot](./media/how-to-luis/run-luis-bot-2step-weather.png)


## <a name="using-luis-with-dialogs"></a><span data-ttu-id="4a676-166">Uso de LUIS con cuadros de diálogo</span><span class="sxs-lookup"><span data-stu-id="4a676-166">Using LUIS with dialogs</span></span>

<span data-ttu-id="4a676-167">Si usa la intención de LUIS para desencadenar un flujo de conversación de varios turnos, puede resultar útil usar cuadros de diálogo para encapsular el flujo.</span><span class="sxs-lookup"><span data-stu-id="4a676-167">If you're using the intent from LUIS to trigger a multi-turn conversation flow, it can be helpful to use dialogs to encapsulate this flow.</span></span> <span data-ttu-id="4a676-168">Este bot de ejemplo funciona con una aplicación de LUIS que detecta las intenciones que se usan para desencadenar un cuadro de diálogo de automatización del hogar o bien un cuadro de diálogo del tiempo.</span><span class="sxs-lookup"><span data-stu-id="4a676-168">This example bot works with a LUIS app that detects intents used to trigger either a home automation dialog or a weather dialog.</span></span>

> [!NOTE] 
> <span data-ttu-id="4a676-169">Para crear su propia copia de la aplicación de LUIS pública usada en este ejemplo, copie el archivo de LUIS [WeatherOrHomeAutomation.json](https://github.com/Microsoft/LUIS-Samples/blob/master/examples/simple-bot-example/WeatherOrHomeAutomation.json).</span><span class="sxs-lookup"><span data-stu-id="4a676-169">To create your own copy of the public LUIS app used in this example, copy the [WeatherOrHomeAutomation.json](https://github.com/Microsoft/LUIS-Samples/blob/master/examples/simple-bot-example/WeatherOrHomeAutomation.json) LUIS file.</span></span> <span data-ttu-id="4a676-170">Después, [importe la aplicación de LUIS](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/create-new-app#import-new-app), [entrénela](https://docs.microsoft.com/en-us/azure/cognitive-services/LUIS/luis-how-to-train) y [publíquela](https://docs.microsoft.com/en-us/azure/cognitive-services/LUIS/publishapp).</span><span class="sxs-lookup"><span data-stu-id="4a676-170">Then [import the LUIS app](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/create-new-app#import-new-app), [train](https://docs.microsoft.com/en-us/azure/cognitive-services/LUIS/luis-how-to-train), and [publish](https://docs.microsoft.com/en-us/azure/cognitive-services/LUIS/publishapp) it.</span></span> <span data-ttu-id="4a676-171">Reemplace el identificador de aplicación pública del código de ejemplo por el identificador de aplicación de la nueva aplicación de LUIS.</span><span class="sxs-lookup"><span data-stu-id="4a676-171">Replace the public app ID in the sample code with the app ID of your new LUIS app.</span></span>

# <a name="ctabcs"></a>[<span data-ttu-id="4a676-172">C#</span><span class="sxs-lookup"><span data-stu-id="4a676-172">C#</span></span>](#tab/cs)

<span data-ttu-id="4a676-173">En primer lugar, modifique `ConfigureServices` en Startup.c, para agregar software intermedio para la aplicación de LUIS.</span><span class="sxs-lookup"><span data-stu-id="4a676-173">First, modify `ConfigureServices` in Startup.cs to add middleware for your LUIS app.</span></span> <span data-ttu-id="4a676-174">Cambie el nombre de `EchoBot` a `LuisDialogBot`.</span><span class="sxs-lookup"><span data-stu-id="4a676-174">Rename `EchoBot` to `LuisDialogBot`.</span></span>
<span data-ttu-id="4a676-175">En este ejemplo, la aplicación de LUIS agregada como `LuisRecognizerMiddleware` detecta las intenciones `homeautomation` o `weather` para desencadenar cuadros de diálogo con esos nombres.</span><span class="sxs-lookup"><span data-stu-id="4a676-175">In this example, the LUIS app added as `LuisRecognizerMiddleware` detects the `homeautomation` or `weather` intents, to trigger dialogs with those names.</span></span> 

```csharp
public void ConfigureServices(IServiceCollection services)
{  
    // Rename EchoBot to LuisDialogBot
    services.AddBot<LuisDialogBot>(options =>
    {
        options.CredentialProvider = new ConfigurationCredentialProvider(Configuration); 
        options.Middleware.Add(new CatchExceptionMiddleware<Exception>(async (context, exception) =>
        {
            await context.TraceActivity("EchoBot Exception", exception);
            await context.SendActivity("Sorry, it looks like something went wrong!");
        }));

        // The Memory Storage used here is for local bot debugging only. When the bot
        // is restarted, anything stored in memory will be gone. 
        IStorage dataStore = new MemoryStorage();

        // Use Dictionary<string, object> for the conversation state type
        options.Middleware.Add(new ConversationState<Dictionary<string, object>>(dataStore));

        // Add LUIS recognizer as middleware
        options.Middleware.Add(
            new LuisRecognizerMiddleware(
                new LuisModel(
                    // This appID is for a public app that's made available for demo purposes
                    "428affb6-7650-46ac-9184-68c00a4f1729",
                    // You can use it by replacing <subscriptionKey> with your Authoring Key
                    // which you can find at https://www.luis.ai under User settings > Authoring Key
                    "<subscriptionKey>",
                    // The location-based URL begins with "https://<region>.api.cognitive.microsoft.com", where region is the region associated with the key you are using. Some examples of regions are `westus`, `westcentralus`, `eastus2`, and `southeastasia`.
                    new Uri("https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/"))));
    });
}
```

<span data-ttu-id="4a676-176">Cambie el nombre de **EchoBot.cs** a **LuisDialogBot.cs** y cambie el nombre de la clase `EchoBot` a `LuisDialogBot`.</span><span class="sxs-lookup"><span data-stu-id="4a676-176">Rename **EchoBot.cs** to **LuisDialogBot.cs**, and rename the `EchoBot` class to `LuisDialogBot`.</span></span> <span data-ttu-id="4a676-177">Después, en **LuisDialogBot.cs**, agregue las siguientes instrucciones `using`.</span><span class="sxs-lookup"><span data-stu-id="4a676-177">Then, in **LuisDialogBot.cs** add the following `using` statements.</span></span>

```csharp
using System;
using System.Collections.Generic;
using System.Threading.Tasks;
using Microsoft.Bot.Builder;
using Microsoft.Bot.Builder.Ai.LUIS;
using Microsoft.Bot.Builder.Core.Extensions;
using Microsoft.Bot.Builder.Dialogs;
using Microsoft.Bot.Builder.Prompts;
using Microsoft.Bot.Schema;
```

<span data-ttu-id="4a676-178">En **LuisDialogBot.cs**, agregue el siguiente código a la clase `LuisDialogBot`.</span><span class="sxs-lookup"><span data-stu-id="4a676-178">In **LuisDialogBot.cs**, add the following code to the `LuisDialogBot` class.</span></span>

```csharp
    public class LuisDialogBot : IBot
    {
        private DialogSet _dialogs;

        public LuisDialogBot()
        {
            _dialogs = new DialogSet();

            _dialogs.Add("homeautomation", CreateHomeAutomationWaterfall());
            _dialogs.Add("weather", CreateWeatherWaterfall());
            _dialogs.Add("weather_city", new Builder.Dialogs.TextPrompt());
        }

        // App ID for a separate LUIS app used to tell if the user wants to turn the lights on or off
        // The `homeautomation` dialogs uses results from this app to determine the "on/off" argument. 
        private static LuisModel luisHomeAutomation =
            new LuisModel("76feb726-515b-44c4-acc9-adb216965a58", "SUBSCRIPTION-KEY", new System.Uri("https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/"));

        public async Task OnTurn(ITurnContext context)
        {
            // This bot is only handling Messages
            if (context.Activity.Type == ActivityTypes.Message)
            {

                // Create a dialog context
                var state = ConversationState<Dictionary<string, object>>.Get(context);
                var dc = _dialogs.CreateContext(context, state);

                // Run the next dialog step
                await dc.Continue();
                
                // Check if any dialog has responded on this turn
                if (!context.Responded)
                {

                    var luisResult = context.Services.Get<RecognizerResult>(LuisRecognizerMiddleware.LuisRecognizerResultKey);
                    var topIntent = luisResult?.GetTopScoringIntent();
                    var utterance = context.Activity.Text;
                    var dialogArgs = new Dictionary<string, object>();

                    switch ((topIntent != null) ? topIntent.Value.intent.ToLowerInvariant() : null)
                    {
                        case "homeautomation":
                            // The Homeautomation_TurnOn and Homeautomation_TurnOff dialogs 
                            // use results from a separate LUIS app.
                            // The results determine the "on/off" argument to pass to the dialog. 
                            var recognizerHomeAutomation = new LuisRecognizer(luisHomeAutomation);
                            RecognizerResult recognizerResult = await recognizerHomeAutomation.Recognize(utterance, System.Threading.CancellationToken.None);
                            var topHomeAutoIntent = recognizerResult.GetTopScoringIntent().intent;


                            dialogArgs.Add("IntentFromHomeAuto", "");
                            switch ((topHomeAutoIntent != null) ? topHomeAutoIntent.ToLowerInvariant() : null)
                            {
                                case "homeautomation_turnon":
                                    dialogArgs["Intent_HomeAutomation"] = "on";
                                    await dc.Begin("homeautomation", dialogArgs);
                                    break;
                                case "homeautomation_turnoff":
                                    dialogArgs["Intent_HomeAutomation"] = "off";
                                    await dc.Begin("homeautomation", dialogArgs);
                                    break;
                                case null:
                                    await dc.Begin("homeautomation", null);
                                    break;
                                default:
                                    dialogArgs["Intent_HomeAutomation"] = topHomeAutoIntent;
                                    await dc.Begin("homeautomation", dialogArgs);
                                    break;
                            }
                            break;
                        case "weather":
                            dialogArgs.Add("LuisResult", luisResult);
                            await dc.Begin("weather", dialogArgs);
                            break;
                        case null:
                            await context.SendActivity($"Couldn't get a result from LUIS. You said: {utterance}");
                            break;
                        default:
                            // The intent didn't match any case, so just display the recognition results.
                            await context.SendActivity($"you said: {utterance}");
                            await context.SendActivity($"Recognized intent: {topIntent.Value.intent}.");

                            break;

                    }

                }
            }
        }

    }


```

<span data-ttu-id="4a676-179">Después de `OnTurn`, agregue código en la clase `LuisDialogBot` para implementar los cuadros de diálogo.</span><span class="sxs-lookup"><span data-stu-id="4a676-179">After `OnTurn`, add code within the `LuisDialogBot` class for implementing the dialogs.</span></span>

```csharp
        // The home automation waterfall has one step
        private WaterfallStep[] CreateHomeAutomationWaterfall()
        {
            return new WaterfallStep[] {
                TurnLightsOnOrOff
            };
        }

        // The weather waterfall has two steps
        private WaterfallStep[] CreateWeatherWaterfall()
        {
            return new WaterfallStep[] {
                AskWeatherLocation,
                SendWeatherReport
            };
        }

        /// <summary>
        /// This is the first step and only of the home automation dialog.
        /// </summary>
        /// <param name="dc"></param>
        /// <param name="args">Can be "on", "off", another string for an intent name, or null.
        /// null indicates that no result was received from which to get an intent name.</param>
        /// <param name="next"></param>
        /// <returns></returns>
        private async Task TurnLightsOnOrOff(DialogContext dc, IDictionary<string, object> args, SkipStepFunction next)
        {
            var intentFromHomeAutomation = args["Intent_HomeAutomation"];
            if (args != null)
            {
                switch (intentFromHomeAutomation)
                {
                    case "on":
                    case "off":
                        await dc.Context.SendActivity($"Turning {intentFromHomeAutomation} your lights!");
                        break;
                    default:
                        await dc.Context.SendActivity($"Intent detected by homeautomation was: {intentFromHomeAutomation}, but the home automation system doesn't support that yet.");
                        break;
                }

            }
            else
            {
                await dc.Context.SendActivity($"You said {dc.Context.Activity.AsMessageActivity().Text}. Unable to get a result from which to determine on/off/other operation");
            }

            await dc.End();

        }

        // This is the first step of the weather dialog
        private async Task AskWeatherLocation(DialogContext dc, IDictionary<string, object> args, SkipStepFunction next)
        {
            var dialogState = dc.ActiveDialog.State as IDictionary<string, object>;
            
            if (args["LuisResult"] is RecognizerResult luisResult)
            {
                var location = GetEntity<string>(luisResult, "Weather_Location");
                if (!string.IsNullOrEmpty(location))
                {
                    dialogState.Add("Location", location);
                }
            }

            // Save info back to the dialog instance
            dc.ActiveDialog.State = dialogState;

            if (!dialogState.ContainsKey("Location"))
            {
                await dc.Prompt("weather_city", "What city do you want the weather for?");
            }
            else
            {
                // We've set the location parameter for the weather report,
                // so go to the next step in the waterfall
                await dc.Continue();
            }
        }

        // This the second step of the weather dialog
        private async Task SendWeatherReport(DialogContext dc, IDictionary<string, object> args, SkipStepFunction next)
        {
            var dialogState = dc.ActiveDialog.State as IDictionary<string, object>;
            if (args != null)
            {
                if (!dialogState.ContainsKey("Location"))
                {   // Interpret args as a reply to prompt only if they didn't give location
                    TextResult locationResult = (TextResult)args;
                    dialogState.Add("Location", locationResult.Text);
                }

            }

            // You can add some logic that uses the location 
            // to get the weather from a weather service instead of hard-coding it
            await dc.Context.SendActivity($"The weather forecast for '{dialogState["Location"]}' is sunny and 70 degrees F.");

            dc.ActiveDialog.State = new Dictionary<string, object>(); // clear the dialog state 
            await dc.End();
        }
```

<span data-ttu-id="4a676-180">Agregue esta función auxiliar.</span><span class="sxs-lookup"><span data-stu-id="4a676-180">Add this helper function.</span></span>

```cs
private T GetEntity<T>(RecognizerResult luisResult, string entityKey)
{
    var data = luisResult.Entities as IDictionary<string, JToken>;
    if (data.TryGetValue(entityKey, out JToken value))
    {
        return value.First.Value<T>();
    }
    return default(T);
}
```

# <a name="javascripttabjs"></a>[<span data-ttu-id="4a676-181">JavaScript</span><span class="sxs-lookup"><span data-stu-id="4a676-181">JavaScript</span></span>](#tab/js)

<span data-ttu-id="4a676-182">Si aún no lo ha hecho, es posible que deba instalar la biblioteca de cuadros de diálogo.</span><span class="sxs-lookup"><span data-stu-id="4a676-182">You may need to install the dialogs library if you have not already done so.</span></span> 

```cmd
npm install --save botbuilder-dialogs
```

<span data-ttu-id="4a676-183">En primer lugar, cree una aplicación de LUIS y agréguela al bot mediante `adapter.use`:</span><span class="sxs-lookup"><span data-stu-id="4a676-183">First, create a LUIS app and add it to your bot using `adapter.use`:</span></span>

```javascript
const { BotFrameworkAdapter, ConversationState, MemoryStorage, TurnContext } = require('botbuilder');
const { LuisRecognizer } = require('botbuilder-ai');
const { DialogSet } = require('botbuilder-dialogs');
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

// Create LuisRecognizer 
// The LUIS application is public, meaning you can use your own subscription key to test the applications.
const luisRecognizer = new LuisRecognizer({
    appId: '1fefd4a7-ae5b-4e63-99f7-0cf499a1423b',
    subscriptionKey: process.env.LUIS_SUBSCRIPTION_KEY,
    serviceEndpoint: 'https://westus.api.cognitive.microsoft.com/',
    verbose: true
});

// Add the recognizer to your bot
adapter.use(luisRecognizer);

// create conversation state
const conversationState = new ConversationState(new MemoryStorage());
adapter.use(conversationState);

// register some dialogs for usage with the intents detected by the LUIS app
const dialogs = new DialogSet();
```

<span data-ttu-id="4a676-184">Luego, agregue los cuadros de diálogo:</span><span class="sxs-lookup"><span data-stu-id="4a676-184">Next, add dialogs:</span></span>

```javascript

dialogs.add('HomeAutomation_TurnOn', [
    async (dialogContext) => {
        const state = conversationState.get(dialogContext.context);
        // state.homeAutomationTurnOn counts how many times this dialog was called 
        state.homeAutomationTurnOn = state.homeAutomationTurnOn ? state.homeAutomationTurnOn + 1 : 1;
        await dialogContext.context.sendActivity(`${state.homeAutomationTurnOn}: You reached the "HomeAutomation_TurnOn" dialog.`);

        await dialogContext.end();
    }
]);

dialogs.add('Weather_GetForecast', [
    async (dialogContext) => {
        const state = conversationState.get(dialogContext.context);
        // state.weatherGetForecast counts how many times this dialog was called  
        state.weatherGetForecast = state.weatherGetForecast ? state.weatherGetForecast + 1 : 1;
        await dialogContext.context.sendActivity(`${state.weatherGetForecast}: You reached the "Weather_GetForecast" dialog.`);

        await dialogContext.end();
    }
]);

dialogs.add('None', [
    async (dialogContext) => {
        const state = conversationState.get(dialogContext.context);
        // state.None counts how many times this dialog was called        
        state.None = state.None ? state.None + 1 : 1;
        await dialogContext.context.sendActivity(`${state.None}: You reached the "None" dialog.`);

        await dialogContext.end();
    }
]);
```

<span data-ttu-id="4a676-185">En el bot, invoque cada cuadro de diálogo en función de la intención reconocida:</span><span class="sxs-lookup"><span data-stu-id="4a676-185">In your bot, invoke each dialog based on the recognized intent:</span></span>
```javascript
server.post('/api/messages', (req, res) => {
    adapter.processActivity(req, res, async (context) => {
        if (context.activity.type === 'message') {
            const state = conversationState.get(context);
            const dc = dialogs.createContext(context, state);

            // Retrieve the LUIS results from our LUIS application
            const luisResults = luisRecognizer.get(context);

            // Extract the top intent from LUIS and use it to select which dialog to start
            // "NotFound" is the intent name for when no top intent can be found.
            const topIntent = LuisRecognizer.topIntent(luisResults, "NotFound");

            const isMessage = context.activity.type === 'message';
            if (isMessage) {
                switch (topIntent) {
                    case 'homeautomation':                    
                        await dc.begin("HomeAutomation_TurnOn", luisResults);
                        break;
                    case 'weather':                    
                        await dc.begin("Weather_GetForecast", luisResults);
                        break;
                    case 'None':
                        await dc.begin("None");
                        break;
                    case 'NotFound':
                        await context.sendActivity(`Sorry, I didn't get any results from LUIS.`);
                        break;
                    default:
                        // handle intents for which we have no dialog
                        await context.sendActivity(`You reached the ${topIntent} intent.`);
                        break;
                }
            }
            
            if (!context.responded) {
                await dc.continue();
                if (!context.responded && isMessage) {
                    await dc.context.sendActivity(`Hi! I'm the LUIS dialog bot. Say something and LUIS will decide how the message should be routed.`);
                }
            }
        }
    });
});
```

<span data-ttu-id="4a676-186">Pruebe a ejecutar el bot en Bot Framework Emulator y dígale cosas como "turn on the lights" (encender las luces) y "get weather in Seattle" (obtener el tiempo en Seattle).</span><span class="sxs-lookup"><span data-stu-id="4a676-186">Try running the bot in the Bot Framework Emulator, and say things like "turn on the lights", and "get weather in Seattle" to it.</span></span>

![ejecutar el bot](./media/how-to-luis/run-luis-bot-js-single-step-dialog.png)

---

## <a name="extract-entities"></a><span data-ttu-id="4a676-188">Extraer entidades</span><span class="sxs-lookup"><span data-stu-id="4a676-188">Extract entities</span></span>

<span data-ttu-id="4a676-189">Además de reconocer la intención, una aplicación de LUIS puede extraer entidades, que son palabras importantes para cumplir la solicitud de un usuario.</span><span class="sxs-lookup"><span data-stu-id="4a676-189">Besides recognizing intent, a LUIS app can also extract entities, which are important words for fulfilling a user's request.</span></span> <span data-ttu-id="4a676-190">Por ejemplo, en el caso del cuadro de diálogo del tiempo, la aplicación de LUIS podría extraer a partir mensaje del usuario la ubicación para proporcionar el informe meteorológico.</span><span class="sxs-lookup"><span data-stu-id="4a676-190">For example, in the example of the weather dialog, the LUIS app might be able to extract the location for the weather report from the user's message.</span></span> 

<span data-ttu-id="4a676-191">Una manera habitual de usar los cuadros de diálogo consiste en identificar todas las entidades del mensaje del usuario y solicitarle las entidades necesarias que no se encuentren.</span><span class="sxs-lookup"><span data-stu-id="4a676-191">A common way to use dialogs is to identify any entities in the user's message, and prompt for any of the required entities that are not found.</span></span> <span data-ttu-id="4a676-192">Después, el paso de cuadro de diálogo posterior controla la respuesta a la solicitud.</span><span class="sxs-lookup"><span data-stu-id="4a676-192">Then, the subsequent dialog step handles the response to the prompt.</span></span>

# <a name="ctabcs"></a>[<span data-ttu-id="4a676-193">C#</span><span class="sxs-lookup"><span data-stu-id="4a676-193">C#</span></span>](#tab/cs)

<span data-ttu-id="4a676-194">Imagine que el mensaje del usuario era "What's the weather in Seattle"? (¿Qué tiempo hace en Seattle?).</span><span class="sxs-lookup"><span data-stu-id="4a676-194">Let's say the message from the user was "What's the weather in Seattle"?</span></span> <span data-ttu-id="4a676-195">Para el bot de este ejemplo, la clase [LuisRecognizer](https://docs.microsoft.com/en-us/dotnet/api/microsoft.bot.builder.ai.luis.luisrecognizer) ofrece un [RecognizerResult](https://docs.microsoft.com/en-us/dotnet/api/microsoft.bot.builder.core.extensions.recognizerresult) con una propiedad [`Entities` ](https://docs.microsoft.com/en-us/dotnet/api/microsoft.bot.builder.core.extensions.recognizerresult#properties-) que tiene esta estructura:</span><span class="sxs-lookup"><span data-stu-id="4a676-195">For the bot in this example, the the [LuisRecognizer](https://docs.microsoft.com/en-us/dotnet/api/microsoft.bot.builder.ai.luis.luisrecognizer) gives you a [RecognizerResult](https://docs.microsoft.com/en-us/dotnet/api/microsoft.bot.builder.core.extensions.recognizerresult) with an [`Entities` property](https://docs.microsoft.com/en-us/dotnet/api/microsoft.bot.builder.core.extensions.recognizerresult#properties-) that has this structure:</span></span>

```json
{
"$instance": {
    "Weather_Location": [
        {
            "startIndex": 22,
            "endIndex": 29,
            "text": "seattle",
            "score": 0.8073087
        }
    ]
},
"Weather_Location": [
        "seattle"
    ]
}
```

<span data-ttu-id="4a676-196">La siguiente función auxiliar que ha agregado a la clase `LuisDialogBot` obtiene entidades del `RecognizerResult` de LUIS.</span><span class="sxs-lookup"><span data-stu-id="4a676-196">The following helper function that you added to your `LuisDialogBot` class gets entities out of the `RecognizerResult` from LUIS.</span></span>

```cs
// Get entities from LUIS result
private T GetEntity<T>(RecognizerResult luisResult, string entityKey)
{
    var data = luisResult.Entities as IDictionary<string, JToken>;
    if (data.TryGetValue(entityKey, out JToken value))
    {
        return value.First.Value<T>();
    }
    return default(T);
}
```

<span data-ttu-id="4a676-197">Al recopilar información como entidades a partir de varios pasos de un cuadro de diálogo, puede ser útil guardar la información que necesita en un estado específico del cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="4a676-197">When gathering information like entities from multiple steps in a dialog, it can be helpful to save the information you need in dialog-specific state.</span></span> <span data-ttu-id="4a676-198">Por ejemplo, en el cuadro de diálogo `AskWeatherLocation`, las entidades se extraen de los resultados de LUIS que se pasan al cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="4a676-198">For example, in the `AskWeatherLocation` dialog, the entities are extracted from LUIS results passed to the dialog.</span></span> <span data-ttu-id="4a676-199">Si se encuentra una entidad `Weather_Location`, se agrega al estado del cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="4a676-199">If a `Weather_Location` entity is found, it's added to the dialog state.</span></span>

```cs

        // This is the first step of the weather dialog
        private async Task AskWeatherLocation(DialogContext dc, IDictionary<string, object> args, SkipStepFunction next)
        {
            var dialogState = dc.ActiveDialog.State as IDictionary<string, object>;
            
            if (args["LuisResult"] is RecognizerResult luisResult)
            {
                var location = GetEntity<string>(luisResult, "Weather_Location");
                if (!string.IsNullOrEmpty(location))
                {
                    dialogState.Add("Location", location);
                }
            }

            // Save info back to the dialog instance
            dc.ActiveDialog.State = dialogState;

            if (!dialogState.ContainsKey("Location"))
            {
                await dc.Prompt("weather_city", "What city do you want the weather for?");
            }
            else
            {
                // We've set the location parameter for the weather report,
                // so go to the next step in the waterfall
                await dc.Continue();
            }
        }
```

<span data-ttu-id="4a676-200">En el segundo paso del cuadro de diálogo, puede obtener la información guardada en el paso anterior a partir del estado de la instancia del cuadro de diálogo, así como de la respuesta del usuario a la pregunta sobre la ubicación, y usarla para enviar un informe meteorológico al usuario.</span><span class="sxs-lookup"><span data-stu-id="4a676-200">In the second step of the dialog, you can get the information saved in the previous step from the dialog instance's state, as well as the user's reply to the prompt for location, and uses it to send a weather report to the user.</span></span>

```cs

// This the second step of the weather dialog
private async Task SendWeatherReport(DialogContext dc, IDictionary<string, object> args, SkipStepFunction next)
{
    var dialogState = dc.ActiveDialog.State as IDictionary<string, object>;
    if (args != null)
    {
        if (!dialogState.ContainsKey("Location"))
        {   // Interpret args as a reply to prompt only if they didn't give location
            TextResult locationResult = (TextResult)args;
            dialogState.Add("Location", locationResult.Text);
        }

    }

    // You can add some logic that uses the location and date
    // to get the weather from a weather service instead of hard-coding it
    await dc.Context.SendActivity($"The weather forecast for '{dialogState["Location"]}' is sunny and 70 degrees F.");
    
    // clear the dialog state 
    dc.ActiveDialog.State = new Dictionary<string, object>(); 
    await dc.End();
}
```

# <a name="javascripttabjs"></a>[<span data-ttu-id="4a676-201">JavaScript</span><span class="sxs-lookup"><span data-stu-id="4a676-201">JavaScript</span></span>](#tab/js)

<span data-ttu-id="4a676-202">Por ejemplo, imagine que el mensaje del usuario era "What's the weather in Seattle"? (¿Qué tiempo hace en Seattle?).</span><span class="sxs-lookup"><span data-stu-id="4a676-202">For example, let's say the message from the user was "What's the weather in Seattle"?</span></span> <span data-ttu-id="4a676-203">Para el bot de este ejemplo, la clase [LuisRecognizer](https://docs.microsoft.com/en-us/javascript/api/botbuilder-ai/luisrecognizer) ofrece un [RecognizerResult](https://docs.microsoft.com/en-us/javascript/api/botbuilder-core-extensions/recognizerresult) con una propiedad `entities` que tiene esta estructura:</span><span class="sxs-lookup"><span data-stu-id="4a676-203">For the bot in this example, the the [LuisRecognizer](https://docs.microsoft.com/en-us/javascript/api/botbuilder-ai/luisrecognizer) gives you a [RecognizerResult](https://docs.microsoft.com/en-us/javascript/api/botbuilder-core-extensions/recognizerresult) with an `entities` property that has this structure:</span></span>

```json
{
"$instance": {
    "Weather_Location": [
        {
            "startIndex": 22,
            "endIndex": 29,
            "text": "seattle",
            "score": 0.8073087
        }
    ]
},
"Weather_Location": [
        "seattle"
    ]
}
```

<span data-ttu-id="4a676-204">Esta función `findEntities` busca las entidades `Weather_Location` reconocidas por la aplicación de LUIS.</span><span class="sxs-lookup"><span data-stu-id="4a676-204">This `findEntities` function looks for any `Weather_Location` entities recognized by the LUIS app.</span></span>

<!-- TODO: Turn into a waterfall -->

```javascript
// Helper function for finding a specified entity
// entityResults are the results from LuisRecognizer.get(context)
function findEntities(entityName, entityResults) {
    let entities = []
    if (entityName in entityResults) {
        entityResults[entityName].forEach(entity => {
            entities.push(entity);
        });
    }
    return entities.length > 0 ? entities : undefined;
}
```

<span data-ttu-id="4a676-205">Llame a `findEntities` desde el cuadro de diálogo `Weather_GetForecast`.</span><span class="sxs-lookup"><span data-stu-id="4a676-205">Call `findEntities` from the `Weather_GetForecast` dialog.</span></span>

```javascript
// Pass the LUIS recognizer result to the args parameter
dialogs.add('Weather_GetForecast', [
    async (dialogContext, args) => {
        const locations = findEntities('Weather_Location', args.entities);

        const state = conversationState.get(dialogContext.context);
        state.weatherGetForecast = state.weatherGetForecast ? state.weatherGetForecast + 1 : 1;
        await dialogContext.context.sendActivity(`${state.weatherGetForecast}: You reached the "Weather.GetForecast" dialog.`);
        if (locations) {
            await dialogContext.context.sendActivity(`Found these "Weather_Location" entities:\n${locations.join(', ')}`);
        }
        await dialogContext.end();
    }
]);
```

<span data-ttu-id="4a676-206">Las entidades se extraen de los resultados de LUIS que se pasan a `dc.begin`.</span><span class="sxs-lookup"><span data-stu-id="4a676-206">The entities are extracted from the LUIS results passed to `dc.begin`.</span></span>

```javascript
await dc.begin("Weather_GetForecast", luisResults);
```
---

## <a name="additional-resources"></a><span data-ttu-id="4a676-207">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="4a676-207">Additional resources</span></span>

<span data-ttu-id="4a676-208">Para más información sobre LUIS, consulte [Language Understanding](./bot-builder-concept-luis.md).</span><span class="sxs-lookup"><span data-stu-id="4a676-208">For more background on LUIS, see [Language Understanding](./bot-builder-concept-luis.md).</span></span>

<span data-ttu-id="4a676-209">Para información sobre cómo compilar las aplicaciones de LUIS que se usan en estos ejemplos, vea [LUIS apps for Bot Builder](https://aka.ms/luis-bot-examples) (Aplicaciones de LUIS para Bot Builder).</span><span class="sxs-lookup"><span data-stu-id="4a676-209">For info on how to build the LUIS apps used in these examples, see [LUIS apps for Bot Builder](https://aka.ms/luis-bot-examples).</span></span>

<span data-ttu-id="4a676-210">LUIS puede combinarse con otros servicios de Cognitive Services para que el bot sea aún más eficaz.</span><span class="sxs-lookup"><span data-stu-id="4a676-210">LUIS can be combined with other Cognitive Services, to make your bot even more powerful.</span></span> <span data-ttu-id="4a676-211">Vea [Combine LUIS apps and QnA services using the Dispatch tool](./bot-builder-tutorial-dispatch.md) (Combinar aplicaciones de LUIS y servicios de QnA mediante la herramienta de distribución) para aprender a combinar QnA con Language Understanding (LUIS) en el bot.</span><span class="sxs-lookup"><span data-stu-id="4a676-211">See [Combine LUIS apps and QnA services using the Dispatch tool](./bot-builder-tutorial-dispatch.md) to learn how to combine QnA with Language Understanding (LUIS) in your bot.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4a676-212">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="4a676-212">Next steps</span></span>


> [!div class="nextstepaction"]
> [<span data-ttu-id="4a676-213">Generación de resultados de LUIS fuertemente tipados</span><span class="sxs-lookup"><span data-stu-id="4a676-213">Generate strongly-typed LUIS results</span></span>](./bot-builder-howto-v4-luisgen.md)


