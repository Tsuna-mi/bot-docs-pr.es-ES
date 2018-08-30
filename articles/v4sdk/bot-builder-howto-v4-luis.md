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
# <a name="using-luis-for-language-understanding"></a>Uso de LUIS para la comprensión del lenguaje

[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]

La tarea de entender qué quiere decir el usuario conversacionalmente y contextualmente puede ser difícil, pero hace que la conversación del bot parezca más natural. Language Understanding, denominado LUIS, le permite hacer precisamente esto, de modo que el bot pueda reconocer la intención de los mensajes de usuario, permitir al usuario emplear un lenguaje más natural y dirigir mejor el flujo de conversación. Si necesita más información sobre cómo se integra LUIS con un bot, vea [Language understanding for bots](./bot-builder-concept-LUIS.md) (Language Understanding para bots). 

Este tema le guía por el proceso de configurar bots simples que usan LUIS para reconocer algunas intenciones diferentes.

## <a name="installing-packages"></a>Instalar paquetes

En primer lugar, asegúrese de que tiene los paquetes necesarios para LUIS.

# <a name="ctabcs"></a>[C#](#tab/cs)

[Agregue una referencia](https://docs.microsoft.com/en-us/nuget/tools/package-manager-ui) a la versión preliminar v4 de los siguientes paquetes NuGet:


* `Microsoft.Bot.Builder.Ai.LUIS`

# <a name="javascripttabjs"></a>[JavaScript](#tab/js)

Puede agregar una referencia al paquete botbuilder y botbuilder-ai en el proyecto a través de npm:

* `npm install --save botbuilder@preview`
* `npm install --save botbuilder-ai@preview`

---


## <a name="set-up-middleware-to-use-your-luis-app"></a>Configurar el software intermedio para que use la aplicación de LUIS

En primer lugar, configure una _aplicación de LUIS_, que es un servicio que se crea en [www.luis.ai](https://www.luis.ai). Dicha aplicación de LUIS se puede entrenar para que pueda reconocer determinadas intenciones. Encontrará los detalles sobre cómo crear una aplicación de LUIS en el [sitio de LUIS](https://www.luis.ai).

En este ejemplo, solo usará una aplicación de LUIS de demostración que puede reconocer las intenciones Help, Cancel y Weather; el identificador de aplicación ya está en el código de ejemplo. Debe tener una clave de Cognitive Services. Para obtenerla, inicie sesión en [www.luis.ai](https://www.luis.ai) y copie la clave de **Configuración de usuario** > **Authoring Key** (Clave de creación).

> [!NOTE] 
> Para crear su propia copia de la aplicación de LUIS pública usada en este ejemplo, copie el archivo de LUIS [FirstSimpleBotExample.json](https://github.com/Microsoft/LUIS-Samples/blob/master/examples/simple-bot-example/FirstSimpleBotExample.json). Después, [importe la aplicación de LUIS](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/create-new-app#import-new-app), [entrénela](https://docs.microsoft.com/en-us/azure/cognitive-services/LUIS/luis-how-to-train) y [publíquela](https://docs.microsoft.com/en-us/azure/cognitive-services/LUIS/publishapp). Reemplace el identificador de aplicación pública del código de ejemplo por el identificador de aplicación de la nueva aplicación de LUIS.

Configure el bot de modo que llame a la aplicación de LUIS para cada mensaje que reciba de un usuario. Para ello, basta con agregarlo a la pila de software intermedio del bot. El software intermedio almacena los resultados del reconocimiento en el objeto de contexto, al que después puede acceder la lógica del bot.

# <a name="ctabcs"></a>[C#](#tab/cs)
Empiece con la plantilla del bot de eco y abra **Startup.cs**. 

Agregue una instrucción `using` para `Microsoft.Bot.Builder.Ai.LUIS`.

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

Actualice el método `ConfigureServices` en su archivo `Startup.cs` para agregar un objeto `LuisRecognizerMiddleware` que se conecta con la aplicación de LUIS. 

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

<!--TODO: DOES PUBLIC APP WORK WITH KEYS IN DIFFERENT REGIIONS? --> Pegue la clave de suscripción desde [https://www.luis.ai](https://www.luis.ai) en lugar de `<subscriptionKey>`.

> [!NOTE] 
> Si usa su propia aplicación de LUIS en lugar de la pública, puede obtener el identificador, la clave de suscripción y la dirección URL de la aplicación de LUIS en [https://www.luis.ai](https://www.luis.ai). 
>
>Para encontrar la URL base a fin de usarla en `LuisModel`, inicie sesión en el [sitio de LUIS](https://www.luis.ai), vaya a la pestaña **Publicar** y consulte la columna **Punto de conexión**, situada en **Resources and Keys** (Recursos y claves). La URL base es la parte de la **dirección URL del punto de conexión** que aparece antes del identificador de suscripción y otros parámetros.

# <a name="javascripttabjs"></a>[JavaScript](#tab/js)

En primer lugar, solicite o importe la clase [LuisRecognizer](https://github.com/Microsoft/botbuilder-js/tree/master/doc/botbuilder-ai/classes/botbuilder_ai.luisrecognizer.md) y cree una instancia para el modelo de LUIS:

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

Agregue el modelo a la pila de software intermedio:

```javascript
adapter.use(model);
```


> [!NOTE] 
> Si usa su propia aplicación de LUIS en lugar de la pública, puede obtener el identificador, el identificador de suscripción y la dirección URL de la aplicación de LUIS en [https://www.luis.ai](https://www.luis.ai). 

---



Language Understanding (LUIS) ya está conectado a su bot. Ahora, veamos cómo obtener la intención del modelo de LUIS almacenado en el objeto de contexto.


## <a name="get-the-intent-from-the-turn-context"></a>Obtener la intención del contexto de turno

Se puede acceder a los resultados de LUIS desde el bot si se usa el contexto en cada turno de conversación.

# <a name="ctabcs"></a>[C#](#tab/cs)

Para que el bot simplemente envíe una respuesta basada en la intención que la aplicación de LUIS ha detectado, reemplace el código de `OnTurn` por lo siguiente:

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

# <a name="javascripttabjs"></a>[JavaScript](#tab/js)

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

Todas las intenciones reconocidas en la expresión se devolverán como una asignación de nombres de intenciones a las puntuaciones, y se puede acceder a ellas desde `results.intents`. Se proporciona un método `LuisRecognizer.topIntent()` estático para ayudar a simplificar la búsqueda de la intención con la puntuación más alta de un conjunto de resultados.
Todas las entidades reconocidas se devolverán como una asignación de nombres de entidades a valores, y se accederá a ellas a través de `results.entities`. Es posible devolver metadatos de entidad adicionales si se pasa un valor `verbose=true` al crear la clase LuisRecognizer. Después, se puede acceder a los metadatos agregados mediante `results.entities.$instance`.

---

<!-- TODO: SHOW RUNNING THE FIRST BOT --> Pruebe a ejecutar el bot en Bot Framework Emulator y dígale cosas como "weather" (tiempo), "help" (ayuda) y "cancel" (cancelar).

![ejecutar el bot](./media/how-to-luis/run-luis-bot.png)


## <a name="using-a-luis-recognizer-with-conversation-state"></a>Usar un reconocedor de LUIS con estado de la conversación
<!-- TBD, complete example --> Si la respuesta al usuario tiene más de un solo turno, tal vez le interese llevar un seguimiento de dónde se encuentra en la conversación. Para ello, guarde el estado de la conversación. Puede usar la intención de LUIS como ayuda para establecer los datos del estado de la conversación, por ejemplo, si un tema de la conversación se ha iniciado o completado.

El software intermedio del reconocedor de LUIS se ejecuta para cada turno de un bot, de modo que usted pueda obtener una intención para cada mensaje que recibe el usuario. Si quiere iniciar un flujo de conversación de varios turnos basado en una intención, una manera de hacerlo consiste en omitir la lógica para el cambio de tema hasta que haya terminado con el tema actual.

# <a name="ctabcs"></a>[C#](#tab/cs)

En EchoState.cs, cambie EchoState a lo siguiente:

```csharp
public class ConversationStateInfo 
{
    public bool WeatherTopicStarted  { get; set; }
    public bool HelpTopicStarted  { get; set; }
    public bool CancelTopicStarted  { get; set; }
}
```

En Startup.cs, cambie la inicialización de ConversationState para usar `ConversationStateInfo`.
```cs
    options.Middleware.Add(new ConversationState<ConversationStateInfo>(dataStore));
```

En EchoBot.cs, edite `OnTurn`:
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

# <a name="javascripttabjs"></a>[JavaScript](#tab/js)

Agregue software intermedio de estado de la conversación después de agregar la clase `LuisRecognizer`.

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

Después, puede guardar el estado que indica qué tema se ha iniciado.

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

Pruebe a ejecutar el bot en Bot Framework Emulator. Fíjese en que "get weather" (obtener el tiempo) es ahora un flujo de conversación de dos turnos.

![ejecutar el bot](./media/how-to-luis/run-luis-bot-2step-weather.png)


## <a name="using-luis-with-dialogs"></a>Uso de LUIS con cuadros de diálogo

Si usa la intención de LUIS para desencadenar un flujo de conversación de varios turnos, puede resultar útil usar cuadros de diálogo para encapsular el flujo. Este bot de ejemplo funciona con una aplicación de LUIS que detecta las intenciones que se usan para desencadenar un cuadro de diálogo de automatización del hogar o bien un cuadro de diálogo del tiempo.

> [!NOTE] 
> Para crear su propia copia de la aplicación de LUIS pública usada en este ejemplo, copie el archivo de LUIS [WeatherOrHomeAutomation.json](https://github.com/Microsoft/LUIS-Samples/blob/master/examples/simple-bot-example/WeatherOrHomeAutomation.json). Después, [importe la aplicación de LUIS](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/create-new-app#import-new-app), [entrénela](https://docs.microsoft.com/en-us/azure/cognitive-services/LUIS/luis-how-to-train) y [publíquela](https://docs.microsoft.com/en-us/azure/cognitive-services/LUIS/publishapp). Reemplace el identificador de aplicación pública del código de ejemplo por el identificador de aplicación de la nueva aplicación de LUIS.

# <a name="ctabcs"></a>[C#](#tab/cs)

En primer lugar, modifique `ConfigureServices` en Startup.c, para agregar software intermedio para la aplicación de LUIS. Cambie el nombre de `EchoBot` a `LuisDialogBot`.
En este ejemplo, la aplicación de LUIS agregada como `LuisRecognizerMiddleware` detecta las intenciones `homeautomation` o `weather` para desencadenar cuadros de diálogo con esos nombres. 

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

Cambie el nombre de **EchoBot.cs** a **LuisDialogBot.cs** y cambie el nombre de la clase `EchoBot` a `LuisDialogBot`. Después, en **LuisDialogBot.cs**, agregue las siguientes instrucciones `using`.

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

En **LuisDialogBot.cs**, agregue el siguiente código a la clase `LuisDialogBot`.

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

Después de `OnTurn`, agregue código en la clase `LuisDialogBot` para implementar los cuadros de diálogo.

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

Agregue esta función auxiliar.

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

# <a name="javascripttabjs"></a>[JavaScript](#tab/js)

Si aún no lo ha hecho, es posible que deba instalar la biblioteca de cuadros de diálogo. 

```cmd
npm install --save botbuilder-dialogs
```

En primer lugar, cree una aplicación de LUIS y agréguela al bot mediante `adapter.use`:

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

Luego, agregue los cuadros de diálogo:

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

En el bot, invoque cada cuadro de diálogo en función de la intención reconocida:
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

Pruebe a ejecutar el bot en Bot Framework Emulator y dígale cosas como "turn on the lights" (encender las luces) y "get weather in Seattle" (obtener el tiempo en Seattle).

![ejecutar el bot](./media/how-to-luis/run-luis-bot-js-single-step-dialog.png)

---

## <a name="extract-entities"></a>Extraer entidades

Además de reconocer la intención, una aplicación de LUIS puede extraer entidades, que son palabras importantes para cumplir la solicitud de un usuario. Por ejemplo, en el caso del cuadro de diálogo del tiempo, la aplicación de LUIS podría extraer a partir mensaje del usuario la ubicación para proporcionar el informe meteorológico. 

Una manera habitual de usar los cuadros de diálogo consiste en identificar todas las entidades del mensaje del usuario y solicitarle las entidades necesarias que no se encuentren. Después, el paso de cuadro de diálogo posterior controla la respuesta a la solicitud.

# <a name="ctabcs"></a>[C#](#tab/cs)

Imagine que el mensaje del usuario era "What's the weather in Seattle"? (¿Qué tiempo hace en Seattle?). Para el bot de este ejemplo, la clase [LuisRecognizer](https://docs.microsoft.com/en-us/dotnet/api/microsoft.bot.builder.ai.luis.luisrecognizer) ofrece un [RecognizerResult](https://docs.microsoft.com/en-us/dotnet/api/microsoft.bot.builder.core.extensions.recognizerresult) con una propiedad [`Entities` ](https://docs.microsoft.com/en-us/dotnet/api/microsoft.bot.builder.core.extensions.recognizerresult#properties-) que tiene esta estructura:

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

La siguiente función auxiliar que ha agregado a la clase `LuisDialogBot` obtiene entidades del `RecognizerResult` de LUIS.

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

Al recopilar información como entidades a partir de varios pasos de un cuadro de diálogo, puede ser útil guardar la información que necesita en un estado específico del cuadro de diálogo. Por ejemplo, en el cuadro de diálogo `AskWeatherLocation`, las entidades se extraen de los resultados de LUIS que se pasan al cuadro de diálogo. Si se encuentra una entidad `Weather_Location`, se agrega al estado del cuadro de diálogo.

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

En el segundo paso del cuadro de diálogo, puede obtener la información guardada en el paso anterior a partir del estado de la instancia del cuadro de diálogo, así como de la respuesta del usuario a la pregunta sobre la ubicación, y usarla para enviar un informe meteorológico al usuario.

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

# <a name="javascripttabjs"></a>[JavaScript](#tab/js)

Por ejemplo, imagine que el mensaje del usuario era "What's the weather in Seattle"? (¿Qué tiempo hace en Seattle?). Para el bot de este ejemplo, la clase [LuisRecognizer](https://docs.microsoft.com/en-us/javascript/api/botbuilder-ai/luisrecognizer) ofrece un [RecognizerResult](https://docs.microsoft.com/en-us/javascript/api/botbuilder-core-extensions/recognizerresult) con una propiedad `entities` que tiene esta estructura:

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

Esta función `findEntities` busca las entidades `Weather_Location` reconocidas por la aplicación de LUIS.

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

Llame a `findEntities` desde el cuadro de diálogo `Weather_GetForecast`.

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

Las entidades se extraen de los resultados de LUIS que se pasan a `dc.begin`.

```javascript
await dc.begin("Weather_GetForecast", luisResults);
```
---

## <a name="additional-resources"></a>Recursos adicionales

Para más información sobre LUIS, consulte [Language Understanding](./bot-builder-concept-luis.md).

Para información sobre cómo compilar las aplicaciones de LUIS que se usan en estos ejemplos, vea [LUIS apps for Bot Builder](https://aka.ms/luis-bot-examples) (Aplicaciones de LUIS para Bot Builder).

LUIS puede combinarse con otros servicios de Cognitive Services para que el bot sea aún más eficaz. Vea [Combine LUIS apps and QnA services using the Dispatch tool](./bot-builder-tutorial-dispatch.md) (Combinar aplicaciones de LUIS y servicios de QnA mediante la herramienta de distribución) para aprender a combinar QnA con Language Understanding (LUIS) en el bot.

## <a name="next-steps"></a>Pasos siguientes


> [!div class="nextstepaction"]
> [Generación de resultados de LUIS fuertemente tipados](./bot-builder-howto-v4-luisgen.md)


