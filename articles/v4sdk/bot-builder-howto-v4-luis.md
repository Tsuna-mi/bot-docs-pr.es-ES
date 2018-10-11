---
title: Uso de LUIS para la comprensión del lenguaje | Microsoft Docs
description: Obtenga información sobre cómo usar LUIS para la comprensión del lenguaje natural con Bot Builder SDK.
keywords: Language Understanding, LUIS, reconocimiento de intenciones, entidades, software intermedio
author: ivorb
ms.author: v-ivorb
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 09/19/18
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: c4a7cfba6f588c95dbebf4886ffd7e432d99c3ae
ms.sourcegitcommit: 3cb288cf2f09eaede317e1bc8d6255becf1aec61
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2018
ms.locfileid: "47389806"
---
# <a name="using-luis-for-language-understanding"></a><span data-ttu-id="7f779-104">Uso de LUIS para la comprensión del lenguaje</span><span class="sxs-lookup"><span data-stu-id="7f779-104">Using LUIS for Language Understanding</span></span>

[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]

<span data-ttu-id="7f779-105">La tarea de entender qué quiere decir el usuario conversacionalmente y contextualmente puede ser difícil, pero hace que la conversación del bot parezca más natural.</span><span class="sxs-lookup"><span data-stu-id="7f779-105">The ability to understand what your user means conversationally and contextually can be a difficult task, but can give your bot a more natural conversation feel.</span></span> <span data-ttu-id="7f779-106">Language Understanding, denominado LUIS, le permite hacer precisamente esto, de modo que el bot pueda reconocer la intención de los mensajes de usuario, permitir al usuario emplear un lenguaje más natural y dirigir mejor el flujo de conversación.</span><span class="sxs-lookup"><span data-stu-id="7f779-106">Language Understanding, called LUIS, enables you to do just that so that your bot can recognize the intent of user messages, allow for more natural language from your user, and better direct the conversation flow.</span></span> <span data-ttu-id="7f779-107">Si necesita más información sobre cómo se integra LUIS con un bot, vea [Language understanding for bots](./bot-builder-concept-LUIS.md) (Language Understanding para bots).</span><span class="sxs-lookup"><span data-stu-id="7f779-107">If you need more background on how LUIS integrates with a bot, see [language understanding for bots](./bot-builder-concept-LUIS.md).</span></span> 

<span data-ttu-id="7f779-108">Este tema le guía por el proceso de configurar bots simples que usan LUIS para reconocer algunas intenciones diferentes.</span><span class="sxs-lookup"><span data-stu-id="7f779-108">This topic walks you through setting up simple bots that use LUIS to recognize a few different intents.</span></span>

## <a name="installing-packages"></a><span data-ttu-id="7f779-109">Instalar paquetes</span><span class="sxs-lookup"><span data-stu-id="7f779-109">Installing Packages</span></span>

<span data-ttu-id="7f779-110">En primer lugar, asegúrese de que tiene los paquetes necesarios para LUIS.</span><span class="sxs-lookup"><span data-stu-id="7f779-110">First, make sure you have the packages necessary for LUIS.</span></span>

# <a name="ctabcs"></a>[<span data-ttu-id="7f779-111">C#</span><span class="sxs-lookup"><span data-stu-id="7f779-111">C#</span></span>](#tab/cs)

<span data-ttu-id="7f779-112">[Agregue una referencia](https://docs.microsoft.com/en-us/nuget/tools/package-manager-ui) a la versión v4 de los siguientes paquetes NuGet:</span><span class="sxs-lookup"><span data-stu-id="7f779-112">[Add a reference](https://docs.microsoft.com/en-us/nuget/tools/package-manager-ui) to v4 version of the following NuGet packages:</span></span>


* `Microsoft.Bot.Builder.AI.LUIS`

# <a name="javascripttabjs"></a>[<span data-ttu-id="7f779-113">JavaScript</span><span class="sxs-lookup"><span data-stu-id="7f779-113">JavaScript</span></span>](#tab/js)

<span data-ttu-id="7f779-114">Instale los paquetes botbuilder y botbuilder-ai en el proyecto mediante npm:</span><span class="sxs-lookup"><span data-stu-id="7f779-114">Install the botbuilder and botbuilder-ai packages in your project using npm:</span></span>

* `npm install --save botbuilder`
* `npm install --save botbuilder-ai`

---

## <a name="set-up-your-luis-app"></a><span data-ttu-id="7f779-115">Configuración de la aplicación de LUIS</span><span class="sxs-lookup"><span data-stu-id="7f779-115">Set up your LUIS app</span></span>

<span data-ttu-id="7f779-116">En primer lugar, configure una _aplicación de LUIS_, que es un servicio que se crea en [luis.ai](https://www.luis.ai).</span><span class="sxs-lookup"><span data-stu-id="7f779-116">First, set up a _LUIS app_, which is a service you create at [luis.ai](https://www.luis.ai).</span></span> <span data-ttu-id="7f779-117">Dicha aplicación de LUIS se puede entrenar para que pueda reconocer determinadas intenciones.</span><span class="sxs-lookup"><span data-stu-id="7f779-117">That LUIS app can be trained for certain intents it should be able to recognize.</span></span> <span data-ttu-id="7f779-118">Encontrará los detalles sobre cómo crear una aplicación de LUIS en el sitio de LUIS.</span><span class="sxs-lookup"><span data-stu-id="7f779-118">Details on how to create your LUIS app can be found on the LUIS site.</span></span>

<span data-ttu-id="7f779-119">En este ejemplo, solo usará una aplicación de LUIS de demostración que puede reconocer las intenciones Help, Cancel y Weather; el identificador de aplicación ya está en el código de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="7f779-119">For this example, you'll just use a demo LUIS app that can recognize Help, Cancel, and Weather intents; the app ID is already in the sample code.</span></span> <span data-ttu-id="7f779-120">Debe tener una clave de Cognitive Services. Para obtenerla, inicie sesión en [luis.ai](https://www.luis.ai) y copie la clave de **Configuración de usuario** > **Clave de creación**.</span><span class="sxs-lookup"><span data-stu-id="7f779-120">You will need to have a Cognitive Services key that you can get by logging in to [luis.ai](https://www.luis.ai) and copying the key from **User settings** > **Authoring Key**.</span></span>

> [!NOTE]
> <span data-ttu-id="7f779-121">Para crear su propia copia de la aplicación de LUIS pública usada en este ejemplo, copie el archivo de LUIS [JSON](https://github.com/Microsoft/LUIS-Samples/blob/master/examples/simple-bot-example/FirstSimpleBotExample.json).</span><span class="sxs-lookup"><span data-stu-id="7f779-121">To create your own copy of the public LUIS app used in this example, copy the [JSON](https://github.com/Microsoft/LUIS-Samples/blob/master/examples/simple-bot-example/FirstSimpleBotExample.json) LUIS file.</span></span> <span data-ttu-id="7f779-122">Después, [importe](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/create-new-app#import-new-app) la aplicación de LUIS, [entrénela](https://docs.microsoft.com/en-us/azure/cognitive-services/LUIS/luis-how-to-train) y [publíquela](https://docs.microsoft.com/en-us/azure/cognitive-services/LUIS/publishapp).</span><span class="sxs-lookup"><span data-stu-id="7f779-122">Then [import](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/create-new-app#import-new-app) the LUIS app, [train](https://docs.microsoft.com/en-us/azure/cognitive-services/LUIS/luis-how-to-train), and [publish](https://docs.microsoft.com/en-us/azure/cognitive-services/LUIS/publishapp) it.</span></span> <span data-ttu-id="7f779-123">Reemplace el identificador de aplicación pública del código de ejemplo por el identificador de aplicación de la nueva aplicación de LUIS.</span><span class="sxs-lookup"><span data-stu-id="7f779-123">Replace the public app ID in the sample code with the app ID of your new LUIS app.</span></span>


### <a name="configure-your-bot-to-call-your-luis-app"></a><span data-ttu-id="7f779-124">Configure el bot para llamar a la aplicación de LUIS.</span><span class="sxs-lookup"><span data-stu-id="7f779-124">Configure your bot to call your LUIS app.</span></span>

# <a name="ctabcs"></a>[<span data-ttu-id="7f779-125">C#</span><span class="sxs-lookup"><span data-stu-id="7f779-125">C#</span></span>](#tab/cs)

<span data-ttu-id="7f779-126">Aunque es posible crear y llamar a la aplicación de LUIS en todo momento, la práctica de codificación recomendada es registrar el servicio LUIS como un singleton y después pasarlo como parámetro al constructor del bot.</span><span class="sxs-lookup"><span data-stu-id="7f779-126">While it's possible to both create and call your LUIS app on every turn, it's better coding practice to register your LUIS service as a singleton and then pass them as parameters to your bot's constructor.</span></span> <span data-ttu-id="7f779-127">Explicaremos este método aquí ya que es un poco más complicado.</span><span class="sxs-lookup"><span data-stu-id="7f779-127">We'll show that method here as it's slightly more complicated.</span></span>

<span data-ttu-id="7f779-128">Empiece con la plantilla del bot de eco y abra **Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="7f779-128">Start with the Echo bot template, and open **Startup.cs**.</span></span> 

<span data-ttu-id="7f779-129">Agregue una instrucción `using` para `Microsoft.Bot.Builder.AI.LUIS`.</span><span class="sxs-lookup"><span data-stu-id="7f779-129">Add a `using` statement for `Microsoft.Bot.Builder.AI.LUIS`</span></span>

```csharp
// add this
using Microsoft.Bot.Builder.AI.LUIS;
```

<span data-ttu-id="7f779-130">Agregue el código siguiente al final de `ConfigureServices`, después de la inicialización de estado.</span><span class="sxs-lookup"><span data-stu-id="7f779-130">Add the following code at the end of `ConfigureServices`, after the state initialization.</span></span> <span data-ttu-id="7f779-131">Esto toma la información del archivo `appsettings.json`, pero esas cadenas se pueden tomar del archivo `.bot`, como el ejemplo vinculado al final de este artículo, o codificarse de forma rígida para las pruebas.</span><span class="sxs-lookup"><span data-stu-id="7f779-131">This grabs the information from the `appsettings.json` file, but those strings can be grabbed from your `.bot` file, like the sample linked at the end of this article, or hard coded for testing.</span></span>

<span data-ttu-id="7f779-132">El singleton devuelve un nuevo `LuisRecognizer` al constructor.</span><span class="sxs-lookup"><span data-stu-id="7f779-132">The singleton returns a new `LuisRecognizer` to the constructor.</span></span>

```csharp
    // Create and register a LUIS recognizer.
    services.AddSingleton(sp =>
    {
        // Get LUIS information from appsettings.json.
        var section = this.Configuration.GetSection("Luis");
        var luisApp = new LuisApplication(
            applicationId: section["AppId"],
            endpointKey: section["SubscriptionKey"],
            azureRegion: section["Region"]);

        // Specify LUIS options. These may vary for your bot.
        var luisPredictionOptions = new LuisPredictionOptions
        {
            IncludeAllIntents = true,
        };

        return new LuisRecognizer(
            application: luisApp,
            predictionOptions: luisPredictionOptions,
            includeApiResults: true);
    });
```

<span data-ttu-id="7f779-133">Pegue la clave de suscripción desde [luis.ai](https://www.luis.ai) en lugar de `<subscriptionKey>`.</span><span class="sxs-lookup"><span data-stu-id="7f779-133">Paste in your subscription key from [luis.ai](https://www.luis.ai) in place of `<subscriptionKey>`.</span></span> <span data-ttu-id="7f779-134">Esto se encuentra más fácilmente si hace clic en el nombre de la cuenta en la parte superior derecha, y se va a **Configuración**, donde se llama a la **clave de creación**.</span><span class="sxs-lookup"><span data-stu-id="7f779-134">This is most readily found if you click on your account name on the top right, and go to **Settings**, where it is called your **Authoring Key**.</span></span>

> [!NOTE]
> <span data-ttu-id="7f779-135">Si usa su propia aplicación de LUIS en lugar de la pública, puede obtener el identificador, la clave de suscripción y la dirección URL de la aplicación de LUIS en [luis.ai](https://www.luis.ai).</span><span class="sxs-lookup"><span data-stu-id="7f779-135">If you're using your own LUIS app instead of the public one, you can get the ID, subscription key, and URL for your LUIS app from [luis.ai](https://www.luis.ai).</span></span> <span data-ttu-id="7f779-136">Estos se pueden encontrar en las pestañas **Publicar** y **Configuración** de la página de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7f779-136">These can be found under **Publish** and **Settings** tabs on your app's page.</span></span>
>
><span data-ttu-id="7f779-137">Para encontrar la URL base a fin de usarla en `LuisModel`, inicie sesión en [luis.ai](https://www.luis.ai), vaya a la pestaña **Publicar** y consulte la columna **Punto de conexión**, situada en **Resources and Keys** (Recursos y claves).</span><span class="sxs-lookup"><span data-stu-id="7f779-137">You can find the base URL to use in your `LuisModel` by logging into the [luis.ai](https://www.luis.ai), going to the **Publish** tab, and looking at the **Endpoint** column under **Resources and Keys**.</span></span> <span data-ttu-id="7f779-138">La URL base es la parte de la **dirección URL del punto de conexión** que aparece antes del identificador de suscripción y otros parámetros.</span><span class="sxs-lookup"><span data-stu-id="7f779-138">The base URL is the portion of the **Endpoint URL** before the subscription ID and other parameters.</span></span>

<span data-ttu-id="7f779-139">A continuación, tenemos que darle al bot esta instancia de LUIS.</span><span class="sxs-lookup"><span data-stu-id="7f779-139">Next, we need to give your bot this LUIS instance.</span></span> <span data-ttu-id="7f779-140">Abra **EchoBot.cs** y, en la parte superior del archivo, agregue el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="7f779-140">Open up **EchoBot.cs**, and at the top of the file add the following code.</span></span> <span data-ttu-id="7f779-141">Para su referencia, también se incluyen el encabezado de clase y los elementos de estado, pero ahora no se explican aquí.</span><span class="sxs-lookup"><span data-stu-id="7f779-141">For your reference the class heading and state items are also included, but we won't explain them here.</span></span>

```csharp
public class EchoBot : IBot
{
    /// <summary>
    /// Gets the Echo Bot state.
    /// </summary>
    private IStatePropertyAccessor<EchoState> EchoStateAccessor { get; }

    /// <summary>
    /// Gets the LUIS recognizer.
    /// </summary>
    private LuisRecognizer Recognizer { get; } = null;

    public EchoBot(ConversationState state, LuisRecognizer luis)
    {
        EchoStateAccessor = state.CreateProperty<EchoState>("EchoBot.EchoState");

        // The incoming luis variable is the LUIS Recognizer we added above.
        this.Recognizer = luis ?? throw new ArgumentNullException(nameof(luis));
    }

    /// ...
```

# <a name="javascripttabjs"></a>[<span data-ttu-id="7f779-142">JavaScript</span><span class="sxs-lookup"><span data-stu-id="7f779-142">JavaScript</span></span>](#tab/js)

<span data-ttu-id="7f779-143">En primer lugar, siga los pasos descritos en la [guía de inicio rápido](../javascript/bot-builder-javascript-quickstart.md) de JavaScript para crear un bot.</span><span class="sxs-lookup"><span data-stu-id="7f779-143">First follow the steps in JavaScript [quickstart](../javascript/bot-builder-javascript-quickstart.md) to create a bot.</span></span> <span data-ttu-id="7f779-144">Aquí estamos codificando nuestra información de LUIS en el bot, pero se puede extraer del archivo `.bot`, como se hace en el ejemplo vinculado al final de este artículo.</span><span class="sxs-lookup"><span data-stu-id="7f779-144">Here we are hardcoding our LUIS information into our bot, but it can be pulled from the `.bot` file as is done in the sample linked at the end of this article.</span></span>

<span data-ttu-id="7f779-145">En el nuevo bot, edite **app.js** para requerir la clase `LuisRecognizer` y cree una instancia para el modelo de LUIS:</span><span class="sxs-lookup"><span data-stu-id="7f779-145">In the new bot, edit **app.js** to require the `LuisRecognizer` class and create an instance for your LUIS model:</span></span>

```javascript
const { ActivityTypes } = require('botbuilder');
const { LuisRecognizer } = require('botbuilder-ai');

const luisApplication = {
    // This appID is for a public app that's made available for demo purposes
    // You can use it or use your own LUIS "Application ID" at https://www.luis.ai under "App Settings".
     applicationId: 'eb0bf5e0-b468-421b-9375-fdfb644c512e',
    // Replace endpointKey with your "Subscription Key"
    // your key is at https://www.luis.ai under Publish > Resources and Keys, look in the Endpoint column
    // The "subscription-key" is embeded in the Endpoint link. 
    endpointKey: '<your subscription key>',
    // You can find your app's region info embeded in the Endpoint link as well.
    // Some examples of regions are `westus`, `westcentralus`, `eastus2`, and `southeastasia`.
    azureRegion: 'westus'
}

// Create configuration for LuisRecognizer's runtime behavior.
const luisPredictionOptions = {
    includeAllIntents: true,
    log: true,
    staging: false
}

// Create the bot that handles incoming Activities.
const luisBot = new LuisBot(luisApplication, luisPredictionOptions);
```

<span data-ttu-id="7f779-146">Después, en el constructor del bot `LuisBot`, obtenga la aplicación para crear la instancia de LuisRecognizer.</span><span class="sxs-lookup"><span data-stu-id="7f779-146">Then, in the constructor of your bot `LuisBot`, get the application to create the LuisRecognizer instance.</span></span>

```javascript
    /**
     * The LuisBot constructor requires one argument (`application`) which is used to create an instance of `LuisRecognizer`.
     * @param {object} luisApplication The basic configuration needed to call LUIS. In this sample the configuration is retrieved from the .bot file.
     * @param {object} luisPredictionOptions (Optional) Contains additional settings for configuring calls to LUIS.
     */
    constructor(application, luisPredictionOptions) {
        this.luisRecognizer = new LuisRecognizer(application, luisPredictionOptions, true);
    }
```

> [!NOTE] 
> <span data-ttu-id="7f779-147">Si usa su propia aplicación de LUIS en lugar de la pública, puede obtener el identificador de la aplicación, la clave de suscripción y la región de la aplicación de LUIS en [https://www.luis.ai](https://www.luis.ai).</span><span class="sxs-lookup"><span data-stu-id="7f779-147">If you're using your own LUIS app instead of the public one, you can get the application ID, subscription key, and region for your LUIS app from [https://www.luis.ai](https://www.luis.ai).</span></span> <span data-ttu-id="7f779-148">Estos se pueden encontrar en las pestañas de publicación y configuración de la página de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7f779-148">These can be found under Publish and Settings tabs on your app's page.</span></span>

---

<span data-ttu-id="7f779-149">El reconocimiento de lenguaje de LUIS ya está configurado para su bot.</span><span class="sxs-lookup"><span data-stu-id="7f779-149">LUIS language understanding is now configured for your bot.</span></span> <span data-ttu-id="7f779-150">A continuación, echemos un vistazo a cómo obtener la intención de LUIS.</span><span class="sxs-lookup"><span data-stu-id="7f779-150">Next, let's look at how to get the intent from LUIS.</span></span>

## <a name="get-the-intent-by-calling-luis"></a><span data-ttu-id="7f779-151">Obtención de la intención mediante una llamada a LUIS</span><span class="sxs-lookup"><span data-stu-id="7f779-151">Get the intent by calling LUIS</span></span>

<span data-ttu-id="7f779-152">El bot obtiene los resultados de LUIS mediante una llamada el reconocedor de LUIS.</span><span class="sxs-lookup"><span data-stu-id="7f779-152">Your bot gets results from LUIS by calling the LUIS recognizer.</span></span>

# <a name="ctabcs"></a>[<span data-ttu-id="7f779-153">C#</span><span class="sxs-lookup"><span data-stu-id="7f779-153">C#</span></span>](#tab/cs)

<span data-ttu-id="7f779-154">Para que el bot simplemente envíe una respuesta basada en la intención que la aplicación de LUIS ha detectado, llame a `LuisRecognizer` para obtener un `RecognizerResult`.</span><span class="sxs-lookup"><span data-stu-id="7f779-154">To have your bot simply send a reply based on the intent that the LUIS app detected, call the the `LuisRecognizer`, to get a `RecognizerResult`.</span></span> <span data-ttu-id="7f779-155">Esto puede realizarse en el código cada vez que necesite obtener la intención de LUIS.</span><span class="sxs-lookup"><span data-stu-id="7f779-155">This can be done within your code whenever you need to get the LUIS intent.</span></span>

```cs
using System.Threading.Tasks;
using Microsoft.Bot;
using Microsoft.Bot.Builder;
using Microsoft.Bot.Builder.Core.Extensions;
using Microsoft.Bot.Schema;
// add this reference
using Microsoft.Bot.Builder.AI.LUIS;

namespace EchoBot
{
    public class EchoBot : IBot
    {
        /// <summary>
        /// Echo bot turn handler 
        /// </summary>
        /// <param name="context">Turn scoped context containing all the data needed
        /// for processing this conversation turn. </param>        
        public async Task OnTurnAsync(ITurnContext context, System.Threading.CancellationToken token)
        {            
            // This bot is only handling Messages
            if (context.Activity.Type == ActivityTypes.Message)
            {
                // Call LUIS recognizer
                var result = this.Recognizer.RecognizeAsync(context, System.Threading.CancellationToken.None);
                
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
                        // Report the weather.
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

<span data-ttu-id="7f779-156">Todas las intenciones reconocidas en la expresión se devolverán como una asignación de nombres de intenciones a las puntuaciones, y se puede acceder a ellas desde `result.Intents`.</span><span class="sxs-lookup"><span data-stu-id="7f779-156">Any intents recognized in the utterance will be returned as a map of intent names to scores and can be accessed from `result.Intents`.</span></span> <span data-ttu-id="7f779-157">Se proporciona un método `LuisRecognizer.topIntent()` estático para ayudar a simplificar la búsqueda de la intención con la puntuación más alta de un conjunto de resultados.</span><span class="sxs-lookup"><span data-stu-id="7f779-157">A static `LuisRecognizer.topIntent()` method is provided to help simplify finding the top scoring intent for a result set.</span></span>

<span data-ttu-id="7f779-158">Todas las entidades reconocidas se devolverán como una asignación de nombres de entidades a valores, y se accederá a ellas mediante `results.entities`.</span><span class="sxs-lookup"><span data-stu-id="7f779-158">Any entities recognized will be returned as a map of entity names to values and accessed using `results.entities`.</span></span> <span data-ttu-id="7f779-159">Es posible devolver metadatos de entidad adicionales si se pasa un valor `verbose=true` al crear la clase LuisRecognizer.</span><span class="sxs-lookup"><span data-stu-id="7f779-159">Additional entity metadata can be returned by passing a `verbose=true` setting when creating the LuisRecognizer.</span></span> <span data-ttu-id="7f779-160">Después, se puede acceder a los metadatos agregados mediante `results.entities.$instance`.</span><span class="sxs-lookup"><span data-stu-id="7f779-160">The added metadata can then be accessed using `results.entities.$instance`.</span></span>

# <a name="javascripttabjs"></a>[<span data-ttu-id="7f779-161">JavaScript</span><span class="sxs-lookup"><span data-stu-id="7f779-161">JavaScript</span></span>](#tab/js)

<span data-ttu-id="7f779-162">Edite el código para escuchar la actividad de entrada, de modo que llame a `LuisRecognizer` para obtener un `RecognizerResult`.</span><span class="sxs-lookup"><span data-stu-id="7f779-162">Edit the code for listening to incoming activity, so that it calls `LuisRecognizer` to get a `RecognizerResult`.</span></span>

```javascript
const { ActivityTypes } = require('botbuilder');
const { LuisRecognizer } = require('botbuilder-ai');

// Listen for incoming requests 
server.post('/api/messages', (req, res) => {
    // Route received request to adapter for processing
    adapter.processActivity(req, res, async (context) => {
        if (context.activity.type === 'message') {
            // Perform a call to LUIS to retrieve results for the user's message.
            const results = await this.luisRecognizer.recognize(turnContext);

            // Since the LuisRecognizer was configured to include the raw results, get the `topScoringIntent` as specified by LUIS.
            const topIntent = results.luisResult.topScoringIntent;
            
            switch (topIntent) {
                case 'None':
                    await context.sendActivity("Sorry, I don't understand.")
                    break;
                case 'Cancel':
                    await context.sendActivity("<cancelling the process>")
                    break;
                case 'Help':
                    await context.sendActivity("<here's some help>");
                    break;
                case 'Weather':
                    await context.sendActivity("The weather today is sunny.");
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

<span data-ttu-id="7f779-163">Todas las intenciones reconocidas en la expresión se devolverán como una asignación de nombres de intenciones a las puntuaciones, y se puede acceder a ellas desde `results.intents`.</span><span class="sxs-lookup"><span data-stu-id="7f779-163">Any intents recognized in the utterance will be returned as a map of intent names to scores and can be accessed from `results.intents`.</span></span> <span data-ttu-id="7f779-164">Se proporciona un método `LuisRecognizer.topIntent()` estático para ayudar a simplificar la búsqueda de la intención con la puntuación más alta de un conjunto de resultados.</span><span class="sxs-lookup"><span data-stu-id="7f779-164">A static `LuisRecognizer.topIntent()` method is provided to help simplify finding the top scoring intent for a result set.</span></span>


---

<span data-ttu-id="7f779-165">Pruebe a ejecutar el bot en Bot Framework Emulator y dígale cosas como "weather" (tiempo), "help" (ayuda) y "cancel" (cancelar).</span><span class="sxs-lookup"><span data-stu-id="7f779-165">Try running the bot in the Bot Framework Emulator, and say things like "weather", "help", and "cancel" to it.</span></span>

![ejecutar el bot](./media/how-to-luis/run-luis-bot.png)

## <a name="extract-entities"></a><span data-ttu-id="7f779-167">Extraer entidades</span><span class="sxs-lookup"><span data-stu-id="7f779-167">Extract entities</span></span>

<span data-ttu-id="7f779-168">Además de reconocer la intención, una aplicación de LUIS puede extraer entidades (es decir, palabras importantes) para cumplir la solicitud de un usuario.</span><span class="sxs-lookup"><span data-stu-id="7f779-168">Besides recognizing intent, a LUIS app can also extract entities, which are important words for fulfilling a user's request.</span></span> <span data-ttu-id="7f779-169">Por ejemplo, para un bot del tiempo, la aplicación de LUIS podría extraer a partir mensaje del usuario la ubicación para proporcionar el informe meteorológico.</span><span class="sxs-lookup"><span data-stu-id="7f779-169">For example, for a weather bot, the LUIS app might be able to extract the location for the weather report from the user's message.</span></span>

<span data-ttu-id="7f779-170">Una manera habitual de estructurar la conversación consiste en identificar todas las entidades del mensaje del usuario y solicitarle las entidades necesarias que no se encuentren.</span><span class="sxs-lookup"><span data-stu-id="7f779-170">A common way to structure your conversation is to identify any entities in the user's message, and prompt for any of the required entities that are not found.</span></span> <span data-ttu-id="7f779-171">Después, los pasos siguientes controlan la respuesta a la solicitud.</span><span class="sxs-lookup"><span data-stu-id="7f779-171">Then, the subsequent steps handle the response to the prompt.</span></span>

# <a name="ctabcs"></a>[<span data-ttu-id="7f779-172">C#</span><span class="sxs-lookup"><span data-stu-id="7f779-172">C#</span></span>](#tab/cs)

<span data-ttu-id="7f779-173">Imagine que el mensaje del usuario era "What's the weather in Seattle"? (¿Qué tiempo hace en Seattle?).</span><span class="sxs-lookup"><span data-stu-id="7f779-173">Let's say the message from the user was "What's the weather in Seattle"?</span></span> <span data-ttu-id="7f779-174">`LuisRecognizer` le ofrece un objeto `RecognizerResult` con una propiedad `Entities` que tiene esta estructura:</span><span class="sxs-lookup"><span data-stu-id="7f779-174">The `LuisRecognizer` gives you a `RecognizerResult` with an `Entities` property that has this structure:</span></span>

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

<span data-ttu-id="7f779-175">La siguiente función auxiliar se puede agregar al bot para obtener entidades del `RecognizerResult` de LUIS.</span><span class="sxs-lookup"><span data-stu-id="7f779-175">The following helper function can be added to your bot to get entities out of the `RecognizerResult` from LUIS.</span></span> <span data-ttu-id="7f779-176">Requerirá el uso de la biblioteca `Newtonsoft.Json.Linq`, que tendrá que agregar a sus instrucciones **using**.</span><span class="sxs-lookup"><span data-stu-id="7f779-176">It will require the use of the `Newtonsoft.Json.Linq` library, which you'll have to add to your **using** statements.</span></span>

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

<span data-ttu-id="7f779-177">Al recopilar información como entidades a partir de varios pasos de una conversación, puede ser útil guardar la información que necesita en su estado.</span><span class="sxs-lookup"><span data-stu-id="7f779-177">When gathering information like entities from multiple steps in a conversation, it can be helpful to save the information you need in your state.</span></span> <span data-ttu-id="7f779-178">Si se encuentra una entidad, se puede agregar al campo de estado adecuado.</span><span class="sxs-lookup"><span data-stu-id="7f779-178">If an entity is found, it can be added to the appropriate state field.</span></span> <span data-ttu-id="7f779-179">En la conversación, si el paso actual ya tiene el campo asociado completado, se puede omitir el paso para solicitar esa información.</span><span class="sxs-lookup"><span data-stu-id="7f779-179">In your conversation if the current step already has the associated field completed, the step to prompt for that information can be skipped.</span></span>

# <a name="javascripttabjs"></a>[<span data-ttu-id="7f779-180">JavaScript</span><span class="sxs-lookup"><span data-stu-id="7f779-180">JavaScript</span></span>](#tab/js)

<span data-ttu-id="7f779-181">Imagine que el mensaje del usuario era "What's the weather in Seattle"? (¿Qué tiempo hace en Seattle?).</span><span class="sxs-lookup"><span data-stu-id="7f779-181">Let's say the message from the user was "What's the weather in Seattle"?</span></span> <span data-ttu-id="7f779-182">`LuisRecognizer` le ofrece un objeto `RecognizerResult` con una propiedad `entities` que tiene esta estructura:</span><span class="sxs-lookup"><span data-stu-id="7f779-182">The `LuisRecognizer` gives you a `RecognizerResult` with an `entities` property that has this structure:</span></span>

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

<span data-ttu-id="7f779-183">Esta función `findEntities` busca las entidades reconocidas por la aplicación de LUIS que coinciden con la `entityName` de entrada.</span><span class="sxs-lookup"><span data-stu-id="7f779-183">This `findEntities` function looks for any entities recognized by the LUIS app that match the incoming `entityName`.</span></span>


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

<span data-ttu-id="7f779-184">Al recopilar información como entidades a partir de varios pasos de una conversación, puede ser útil guardar la información que necesita en su estado.</span><span class="sxs-lookup"><span data-stu-id="7f779-184">When gathering information like entities from multiple steps in a conversation, it can be helpful to save the information you need in your state.</span></span> <span data-ttu-id="7f779-185">Si se encuentra una entidad, se puede agregar al campo de estado adecuado.</span><span class="sxs-lookup"><span data-stu-id="7f779-185">If an entity is found, it can be added to the appropriate state field.</span></span> <span data-ttu-id="7f779-186">En la conversación, si el paso actual ya tiene el campo asociado completado, se puede omitir el paso para solicitar esa información.</span><span class="sxs-lookup"><span data-stu-id="7f779-186">In your conversation if the current step already has the associated field completed, the step to prompt for that information can be skipped.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7f779-187">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="7f779-187">Additional resources</span></span>

<span data-ttu-id="7f779-188">Para obtener un ejemplo de uso de LUIS, consulte los proyectos para [[C#](https://aka.ms/cs-luis-sample)] o [[JavaScript](https://aka.ms/js-luis-sample)].</span><span class="sxs-lookup"><span data-stu-id="7f779-188">For a sample using LUIS, check out the projects for [[C#](https://aka.ms/cs-luis-sample)] or [[JavaScript](https://aka.ms/js-luis-sample)].</span></span>

## <a name="next-steps"></a><span data-ttu-id="7f779-189">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="7f779-189">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="7f779-190">Combinación de LUIS y servicios QnA con la herramienta de distribución</span><span class="sxs-lookup"><span data-stu-id="7f779-190">Combine LUIS and QnA services using the Dispatch tool</span></span>](./bot-builder-tutorial-dispatch.md)
