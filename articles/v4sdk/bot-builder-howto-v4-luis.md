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
# <a name="using-luis-for-language-understanding"></a>Uso de LUIS para la comprensión del lenguaje

[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]

La tarea de entender qué quiere decir el usuario conversacionalmente y contextualmente puede ser difícil, pero hace que la conversación del bot parezca más natural. Language Understanding, denominado LUIS, le permite hacer precisamente esto, de modo que el bot pueda reconocer la intención de los mensajes de usuario, permitir al usuario emplear un lenguaje más natural y dirigir mejor el flujo de conversación. Si necesita más información sobre cómo se integra LUIS con un bot, vea [Language understanding for bots](./bot-builder-concept-LUIS.md) (Language Understanding para bots). 

Este tema le guía por el proceso de configurar bots simples que usan LUIS para reconocer algunas intenciones diferentes.

## <a name="installing-packages"></a>Instalar paquetes

En primer lugar, asegúrese de que tiene los paquetes necesarios para LUIS.

# <a name="ctabcs"></a>[C#](#tab/cs)

[Agregue una referencia](https://docs.microsoft.com/en-us/nuget/tools/package-manager-ui) a la versión v4 de los siguientes paquetes NuGet:


* `Microsoft.Bot.Builder.AI.LUIS`

# <a name="javascripttabjs"></a>[JavaScript](#tab/js)

Instale los paquetes botbuilder y botbuilder-ai en el proyecto mediante npm:

* `npm install --save botbuilder`
* `npm install --save botbuilder-ai`

---

## <a name="set-up-your-luis-app"></a>Configuración de la aplicación de LUIS

En primer lugar, configure una _aplicación de LUIS_, que es un servicio que se crea en [luis.ai](https://www.luis.ai). Dicha aplicación de LUIS se puede entrenar para que pueda reconocer determinadas intenciones. Encontrará los detalles sobre cómo crear una aplicación de LUIS en el sitio de LUIS.

En este ejemplo, solo usará una aplicación de LUIS de demostración que puede reconocer las intenciones Help, Cancel y Weather; el identificador de aplicación ya está en el código de ejemplo. Debe tener una clave de Cognitive Services. Para obtenerla, inicie sesión en [luis.ai](https://www.luis.ai) y copie la clave de **Configuración de usuario** > **Clave de creación**.

> [!NOTE]
> Para crear su propia copia de la aplicación de LUIS pública usada en este ejemplo, copie el archivo de LUIS [JSON](https://github.com/Microsoft/LUIS-Samples/blob/master/examples/simple-bot-example/FirstSimpleBotExample.json). Después, [importe](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/create-new-app#import-new-app) la aplicación de LUIS, [entrénela](https://docs.microsoft.com/en-us/azure/cognitive-services/LUIS/luis-how-to-train) y [publíquela](https://docs.microsoft.com/en-us/azure/cognitive-services/LUIS/publishapp). Reemplace el identificador de aplicación pública del código de ejemplo por el identificador de aplicación de la nueva aplicación de LUIS.


### <a name="configure-your-bot-to-call-your-luis-app"></a>Configure el bot para llamar a la aplicación de LUIS.

# <a name="ctabcs"></a>[C#](#tab/cs)

Aunque es posible crear y llamar a la aplicación de LUIS en todo momento, la práctica de codificación recomendada es registrar el servicio LUIS como un singleton y después pasarlo como parámetro al constructor del bot. Explicaremos este método aquí ya que es un poco más complicado.

Empiece con la plantilla del bot de eco y abra **Startup.cs**. 

Agregue una instrucción `using` para `Microsoft.Bot.Builder.AI.LUIS`.

```csharp
// add this
using Microsoft.Bot.Builder.AI.LUIS;
```

Agregue el código siguiente al final de `ConfigureServices`, después de la inicialización de estado. Esto toma la información del archivo `appsettings.json`, pero esas cadenas se pueden tomar del archivo `.bot`, como el ejemplo vinculado al final de este artículo, o codificarse de forma rígida para las pruebas.

El singleton devuelve un nuevo `LuisRecognizer` al constructor.

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

Pegue la clave de suscripción desde [luis.ai](https://www.luis.ai) en lugar de `<subscriptionKey>`. Esto se encuentra más fácilmente si hace clic en el nombre de la cuenta en la parte superior derecha, y se va a **Configuración**, donde se llama a la **clave de creación**.

> [!NOTE]
> Si usa su propia aplicación de LUIS en lugar de la pública, puede obtener el identificador, la clave de suscripción y la dirección URL de la aplicación de LUIS en [luis.ai](https://www.luis.ai). Estos se pueden encontrar en las pestañas **Publicar** y **Configuración** de la página de la aplicación.
>
>Para encontrar la URL base a fin de usarla en `LuisModel`, inicie sesión en [luis.ai](https://www.luis.ai), vaya a la pestaña **Publicar** y consulte la columna **Punto de conexión**, situada en **Resources and Keys** (Recursos y claves). La URL base es la parte de la **dirección URL del punto de conexión** que aparece antes del identificador de suscripción y otros parámetros.

A continuación, tenemos que darle al bot esta instancia de LUIS. Abra **EchoBot.cs** y, en la parte superior del archivo, agregue el código siguiente. Para su referencia, también se incluyen el encabezado de clase y los elementos de estado, pero ahora no se explican aquí.

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

# <a name="javascripttabjs"></a>[JavaScript](#tab/js)

En primer lugar, siga los pasos descritos en la [guía de inicio rápido](../javascript/bot-builder-javascript-quickstart.md) de JavaScript para crear un bot. Aquí estamos codificando nuestra información de LUIS en el bot, pero se puede extraer del archivo `.bot`, como se hace en el ejemplo vinculado al final de este artículo.

En el nuevo bot, edite **app.js** para requerir la clase `LuisRecognizer` y cree una instancia para el modelo de LUIS:

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

Después, en el constructor del bot `LuisBot`, obtenga la aplicación para crear la instancia de LuisRecognizer.

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
> Si usa su propia aplicación de LUIS en lugar de la pública, puede obtener el identificador de la aplicación, la clave de suscripción y la región de la aplicación de LUIS en [https://www.luis.ai](https://www.luis.ai). Estos se pueden encontrar en las pestañas de publicación y configuración de la página de la aplicación.

---

El reconocimiento de lenguaje de LUIS ya está configurado para su bot. A continuación, echemos un vistazo a cómo obtener la intención de LUIS.

## <a name="get-the-intent-by-calling-luis"></a>Obtención de la intención mediante una llamada a LUIS

El bot obtiene los resultados de LUIS mediante una llamada el reconocedor de LUIS.

# <a name="ctabcs"></a>[C#](#tab/cs)

Para que el bot simplemente envíe una respuesta basada en la intención que la aplicación de LUIS ha detectado, llame a `LuisRecognizer` para obtener un `RecognizerResult`. Esto puede realizarse en el código cada vez que necesite obtener la intención de LUIS.

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

Todas las intenciones reconocidas en la expresión se devolverán como una asignación de nombres de intenciones a las puntuaciones, y se puede acceder a ellas desde `result.Intents`. Se proporciona un método `LuisRecognizer.topIntent()` estático para ayudar a simplificar la búsqueda de la intención con la puntuación más alta de un conjunto de resultados.

Todas las entidades reconocidas se devolverán como una asignación de nombres de entidades a valores, y se accederá a ellas mediante `results.entities`. Es posible devolver metadatos de entidad adicionales si se pasa un valor `verbose=true` al crear la clase LuisRecognizer. Después, se puede acceder a los metadatos agregados mediante `results.entities.$instance`.

# <a name="javascripttabjs"></a>[JavaScript](#tab/js)

Edite el código para escuchar la actividad de entrada, de modo que llame a `LuisRecognizer` para obtener un `RecognizerResult`.

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

Todas las intenciones reconocidas en la expresión se devolverán como una asignación de nombres de intenciones a las puntuaciones, y se puede acceder a ellas desde `results.intents`. Se proporciona un método `LuisRecognizer.topIntent()` estático para ayudar a simplificar la búsqueda de la intención con la puntuación más alta de un conjunto de resultados.


---

Pruebe a ejecutar el bot en Bot Framework Emulator y dígale cosas como "weather" (tiempo), "help" (ayuda) y "cancel" (cancelar).

![ejecutar el bot](./media/how-to-luis/run-luis-bot.png)

## <a name="extract-entities"></a>Extraer entidades

Además de reconocer la intención, una aplicación de LUIS puede extraer entidades (es decir, palabras importantes) para cumplir la solicitud de un usuario. Por ejemplo, para un bot del tiempo, la aplicación de LUIS podría extraer a partir mensaje del usuario la ubicación para proporcionar el informe meteorológico.

Una manera habitual de estructurar la conversación consiste en identificar todas las entidades del mensaje del usuario y solicitarle las entidades necesarias que no se encuentren. Después, los pasos siguientes controlan la respuesta a la solicitud.

# <a name="ctabcs"></a>[C#](#tab/cs)

Imagine que el mensaje del usuario era "What's the weather in Seattle"? (¿Qué tiempo hace en Seattle?). `LuisRecognizer` le ofrece un objeto `RecognizerResult` con una propiedad `Entities` que tiene esta estructura:

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

La siguiente función auxiliar se puede agregar al bot para obtener entidades del `RecognizerResult` de LUIS. Requerirá el uso de la biblioteca `Newtonsoft.Json.Linq`, que tendrá que agregar a sus instrucciones **using**.

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

Al recopilar información como entidades a partir de varios pasos de una conversación, puede ser útil guardar la información que necesita en su estado. Si se encuentra una entidad, se puede agregar al campo de estado adecuado. En la conversación, si el paso actual ya tiene el campo asociado completado, se puede omitir el paso para solicitar esa información.

# <a name="javascripttabjs"></a>[JavaScript](#tab/js)

Imagine que el mensaje del usuario era "What's the weather in Seattle"? (¿Qué tiempo hace en Seattle?). `LuisRecognizer` le ofrece un objeto `RecognizerResult` con una propiedad `entities` que tiene esta estructura:

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

Esta función `findEntities` busca las entidades reconocidas por la aplicación de LUIS que coinciden con la `entityName` de entrada.


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

Al recopilar información como entidades a partir de varios pasos de una conversación, puede ser útil guardar la información que necesita en su estado. Si se encuentra una entidad, se puede agregar al campo de estado adecuado. En la conversación, si el paso actual ya tiene el campo asociado completado, se puede omitir el paso para solicitar esa información.

## <a name="additional-resources"></a>Recursos adicionales

Para obtener un ejemplo de uso de LUIS, consulte los proyectos para [[C#](https://aka.ms/cs-luis-sample)] o [[JavaScript](https://aka.ms/js-luis-sample)].

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Combinación de LUIS y servicios QnA con la herramienta de distribución](./bot-builder-tutorial-dispatch.md)
