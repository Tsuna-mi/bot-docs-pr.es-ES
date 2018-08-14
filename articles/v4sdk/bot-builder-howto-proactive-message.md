---
title: Uso de la mensajería automática | Microsoft Docs
description: Obtenga información sobre cómo usar mensajes automáticos con el bot.
keywords: mensaje automático
author: jonathanfingold
ms.author: jonathanfingold
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 05/01/2018
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: fd53a897d9847432fd337402d40edfcd6f4ff061
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305725"
---
# <a name="how-to-use-proactive-messaging"></a>Uso de la mensajería automática

A menudo, los bots envían _mensajes reactivos_, pero hay veces que necesitamos enviar también un [mensaje automático](bot-builder-proactive-messages.md). 

Un caso común de mensajería automática es cuando el bot realiza una tarea puede llevar una cantidad indeterminada de tiempo. En este caso, puede almacenar información sobre la tarea, indicar al usuario que el bot regresará cuando la tarea termine y dejar que la conversación continúe. Cuando la tarea se completa, el bot puede reanudar la conversación con el envío automático del mensaje de confirmación.

# <a name="ctabcs"></a>[C#](#tab/cs)

## <a name="notes-about-this-sample"></a>Notas sobre este ejemplo

Se va a modificar el ejemplo básico de EchoBot.
- Se usará `Microsoft.Samples.Proactive` como el espacio de nombres.
- Se sustituirá el archivo de estado con un archivo `JobData.cs`.
- Se sustituirá el archivo del bot con un archivo `ProactiveBot.cs`.

> [!NOTE]
> La mensajería automática requiere actualmente que el bot tenga un identificador de aplicación y una contraseña válidos.


## <a name="define-task-data"></a>Definición de datos de tareas

En este escenario, se va a realizar un seguimiento de las tareas arbitrarias que diferentes usuarios pueden crear en distintas conversaciones. Por tanto, se va a usar el software intermedio de estado del bot general, en lugar del software intermedio de estado del usuario o de la conversación.

La siguiente clase define la estructura de datos que se usará para los trabajos individuales.


```csharp
using Microsoft.Bot.Schema;
using System.Collections.Generic;

namespace Microsoft.Samples.Proactive
{
    /// <summary>
    /// Class for storing job state. 
    /// </summary>
    public class JobData
    {
        /// <summary>
        /// The name to use to read and write this bot state object to storage.
        /// </summary>
        public readonly static string PropertyName = $"BotState:{typeof(Dictionary<int, JobData>).FullName}";

        public int JobNumber { get; set; } = 0;
        public bool Completed { get; set; } = false;

        /// <summary>
        /// The conversation reference to which to send status updates.
        /// </summary>
        public ConversationReference Conversation { get; set; }
    }
}
```


También es necesario agregar el software intermedio de estado al código de inicio.


En el archivo `StartUp.cs`, actualice el método `ConfigureServices` para agregar un diccionario de trabajos al estado del bot. En el código siguiente, es la última llamada a `options.Middleware.Add`.
```csharp
// This method gets called by the runtime. Use this method to add services to the container.
public void ConfigureServices(IServiceCollection services)
{
    services.AddBot<ProactiveBot>(options =>
    {
        options.CredentialProvider = new ConfigurationCredentialProvider(Configuration);

        // The CatchExceptionMiddleware provides a top-level exception handler for your bot. 
        // Any exceptions thrown by other Middleware, or by your OnTurn method, will be 
        // caught here. To facillitate debugging, the exception is sent out, via Trace, 
        // to the emulator. Trace activities are NOT displayed to users, so in addition
        // an "Ooops" message is sent. 
        options.Middleware.Add(new CatchExceptionMiddleware<Exception>(async (context, exception) =>
        {
            await context.TraceActivity($"{nameof(ProactiveBot)} Exception", exception);
            await context.SendActivity("Sorry, it looks like something went wrong!");
        }));

        // The Memory Storage used here is for local bot debugging only. When the bot
        // is restarted, anything stored in memory will be gone. 
        IStorage dataStore = new MemoryStorage();

        // Using the base BotState here, since the job log is not necessarily tied to a
        // specific user or conversation.
        options.Middleware.Add(
            new BotState<Dictionary<int, JobData>>(
                dataStore, JobData.PropertyName, (context) => $"jobs/{typeof(Dictionary<int, JobData>)}"));
    });
}
```


## <a name="update-your-bot-to-create-and-run-jobs"></a>Actualización del bot para crear y ejecutar trabajos

En cada turno, se permitirá al usuario crear un trabajo escribiendo `run` o `run job`.

Como respuesta, el bot realizará los siguientes pasos en dicho turno:
- Crear el trabajo.
- Registrar información sobre la conversación actual para poder enviarle el mensaje automático más adelante.
- Permitir que el usuario sepa que se va a iniciar el trabajo y cuándo se finalizará.
- Iniciar el trabajo asincrónico.
- Dejar que el turno se cierre.

El trabajo que se va a iniciar es un sencillo temporizador de cinco segundos que se completa con el envío del mensaje automático.
- La llamada al método de continuación de la conversación del adaptador crea un turno iniciado por el bot.
- Este turno tiene su propio contexto, del que se recupera la información de estado.
- Este contexto se usa para enviar el mensaje automático al usuario.



> [!NOTE]
> El método `GetAppId` es una solución alternativa para habilitar la mensajería automática en el SDK de .NET.

```csharp
using Microsoft.Bot;
using Microsoft.Bot.Builder;
using Microsoft.Bot.Connector.Authentication;
using Microsoft.Bot.Schema;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Security.Claims;
using System.Security.Principal;
using System.Threading;
using System.Threading.Tasks;

namespace Microsoft.Samples.Proactive
{
    public class ProactiveBot : IBot
    {
        /// <summary>
        /// Random number generator for job numbers.
        /// </summary>
        private static Random NumberGenerator = new Random();

        /// <summary>
        /// Gets the job log from the bot state.
        /// </summary>
        /// <param name="context">The current turn context.</param>
        /// <returns>The job log.</returns>
        private static Dictionary<int, JobData> GetJobLog(ITurnContext context)
        {
            return context.Services.Get<Dictionary<int, JobData>>(JobData.PropertyName);
        }

        /// <summary>
        /// Workaround to get the bot's app ID.
        /// </summary>
        /// <param name="context">The current turn context.</param>
        /// <returns>The application ID for the bot.</returns>
        private static string GetAppId(ITurnContext context)
        {
            // The BotFrameworkAdapter sets the identity provider on the context object.
            var claimsIdentity = context.Services.Get<IIdentity>("BotIdentity") as ClaimsIdentity;

            // For requests from a channel, the app ID is in the Audience claim of the JWT token.
            // For requests from the emulator, it is in the AppId claim.
            // For unauthenticated requests, we have anonymouse identity provided auth is disabled.
            // For Activities coming from Emulator AppId claim contains the Bot's AAD AppId.
            var botAppIdClaim =
                (claimsIdentity.Claims?.SingleOrDefault(claim => claim.Type == AuthenticationConstants.AudienceClaim)
                ?? claimsIdentity.Claims?.SingleOrDefault(claim => claim.Type == AuthenticationConstants.AppIdClaim));

            return botAppIdClaim?.Value;
        }

        /// <summary>
        /// Every Conversation turn calls this method.
        /// When the user types "run" or "run job", the bot starts a "job".
        /// When the job finishes, the bot proactively notifies the user.
        /// </summary>
        /// <param name="context">The turn context.</param>
        /// <remarks>When our virtual job finishes, it sends a proactive message
        /// to notify the user that the job completed.</remarks>
        public async Task OnTurn(ITurnContext context)
        {
            // This bot is only handling Messages
            if (context.Activity.Type is ActivityTypes.Message)
            {
                var text = context.Activity.AsMessageActivity()?.Text?.Trim().ToLower();
                switch (text)
                {
                    case "run":
                    case "run job":

                        var jobLog = GetJobLog(context);
                        var job = CreateJob(context, jobLog);
                        var appId = GetAppId(context);
                        var conversation = TurnContext.GetConversationReference(context.Activity);

                        await context.SendActivity($"We're starting job {job.JobNumber} for you. We'll notify you when it's complete.");

                        // Since the context is disposed at the end of the turn, extract and send the
                        // information we need to send the proactive message later.
                        var adapter = context.Adapter;
                        Task.Run(() =>
                        {
                            // Simulate a separate process to complete the user's job.
                            Thread.Sleep(5000);

                            // Perform bookkeeping and send the proactive message.
                            CompleteJob(adapter, appId, conversation, job.JobNumber);
                        });

                        break;

                    default:

                        await context.SendActivity("Type 'run' or 'run job' to start a new job.");

                        break;
                }
            }
        }

        /// <summary>
        /// Creates a simulated job and updates the job log.
        /// </summary>
        /// <param name="context">The current turn context.</param>
        /// <param name="jobLog">The job log.</param>
        /// <returns>A new job.</returns>
        private JobData CreateJob(ITurnContext context, Dictionary<int, JobData> jobLog)
        {
            // Generate a non-duplicate job number;
            int number;
            while (jobLog.ContainsKey(number = NumberGenerator.Next())) { }

            // Simulate creaing the job and logging it.
            var job = new JobData
            {
                JobNumber = number,
                Conversation = TurnContext.GetConversationReference(context.Activity)
            };
            jobLog.Add(job.JobNumber, job);

            // Return the created job.
            return job;
        }

        /// <summary>
        /// Performs bookkeeping and proactively notifies the user that their job completed.
        /// </summary>
        /// <param name="adapter">The bot adapter with which to send the message.</param>
        /// <param name="appId">The app ID of the bot to send the message from.</param>
        /// <param name="conversation">The conversation in which to put the message.</param>
        /// <param name="jobNumber">The number of the job that completed.</param>
        private async void CompleteJob(BotAdapter adapter, string appId, ConversationReference conversation, int jobNumber)
        {
            await adapter.ContinueConversation(appId, conversation, async context =>
            {
                // Get the job log from state, and retrieve the job.
                var jobLog = GetJobLog(context);
                var job = jobLog[jobNumber];

                // Perform bookkeeping.
                job.Completed = true;

                // Send the user a proactive confirmation message.
                await context.SendActivity($"Job {job.JobNumber} is complete.");
            });
        }
    }
}
```

# <a name="javascripttabjs"></a>[JavaScript](#tab/js)

Para poder enviar un mensaje automático a un usuario, el usuario tendrá que enviar al menos un mensaje de estilo reactivo al bot. 

Debe enviar un mensaje al bot, ya que necesita obtener una referencia al objeto de actividad y guardarlo en algún sitio para su futuro uso. El objeto de actividad se puede considerar como la dirección de los usuarios que contiene información sobre el canal al que pertenecen, su identificador de usuario, el identificador de la conversación e incluso el servidor que debe recibir los futuros mensajes. Se trata de un objeto JSON sencillo y debe guardarse completo sin manipularlo.

Se va a empezar con un fragmento de código corto que muestra cómo guardar la referencia de la conversación cada vez que el usuario diga "suscribir":
```javascript
const { MemoryStorage } = require('botbuilder');

const storage = new MemoryStorage();

// Listen for incoming activity 
server.post('/api/messages', (req, res) => {
    // Route received activity to adapter for processing
    adapter.processActivity(req, res, async (context) => {
        if (context.activity.type === 'message') {
            const utterances = (context.activity.text || '').trim().toLowerCase()
            if (utterances === 'subscribe') {
                var userId = await saveReference(TurnContext.getConversationReference(context.activity));
                await subscribeUser(userId)
                await context.sendActivity(`Thank You! We will message you shortly.`);
               
            } else{
                await context.sendActivity("Say 'subscribe' to start proactive message");
            }
    
        }
    });
});
```
El fragmento de código anterior llama a la función `saveReference()` que guardará la referencia del usuario con `MemoryStorage` y devuelve `userId`. Una vez que se haya guardado correctamente la referencia, se llama a `subscribeUser()`, que notificará al usuario que se ha suscrito. 

La función `subscribeUser()` es la que configura la suscripción real. Echemos un vistazo a una implementación sencilla que inicia un temporizador de dos segundos y que envía un mensaje automático al usuario una vez transcurrido dicho período:

```javascript
// Persist info to storage
async function saveReference(reference){
    const userId = reference.activityId
    const changes = {};
    changes['reference/' + userId] = reference;
    await storage.write(changes); // Write reference info to persisted storage
    return userId;
}

// Subscribe user to a proactive call. In this case, we are using a setTimeOut() to trigger the proactive call
async function subscribeUser(userId) {
    setTimeout(async () => {
        const reference = await findReference(userId);
        if (reference) {
            await adapter.continueConversation(reference, async (context) => {
                await context.sendActivity("You have been notified");
            });
            
        }
    }, 2000); // Trigger after 2 secs
}

// Read the stored reference info from storage
async function findReference(userId){
    const referenceKey = 'reference/' + userId;
    var rows = await storage.read([referenceKey])
    var reference = await rows[referenceKey]

    return reference;
}
```

La función `subscribeUser()` configura un temporizador que buscará el objeto de referencia leyéndolo en el almacenamiento. Si se encontró el objeto de referencia, se puede continuar la conversación con el usuario. El método `continueConversation` permite al bot enviar mensajes automáticos a una conversación o usuario con los que ya se ha comunicado.

---

## <a name="test-your-bot"></a>Prueba del bot

Para probar el bot, impleméntelo en Azure como un bot de solo registro y pruébelo en Chat en web o localmente con el emulador.
