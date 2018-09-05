---
title: Envío de mensajes automáticos | Microsoft Docs
description: Obtenga información sobre cómo usar mensajes automáticos con el bot.
keywords: mensaje automático
author: jonathanfingold
ms.author: jonathanfingold
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 05/01/2018
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: c22ce6a35d4d49506360a78a439f15137c429d9d
ms.sourcegitcommit: 2dc75701b169d822c9499e393439161bc87639d2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42905139"
---
# <a name="send-proactive-messages"></a><span data-ttu-id="9b420-104">Envío de mensajes automáticos</span><span class="sxs-lookup"><span data-stu-id="9b420-104">Send proactive messages</span></span> 

[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]


<span data-ttu-id="9b420-105">A menudo, los bots envían _mensajes reactivos_, pero hay veces que necesitamos enviar también un [mensaje automático](bot-builder-proactive-messages.md).</span><span class="sxs-lookup"><span data-stu-id="9b420-105">Often bots send _reactive messages_, but there are times when we need to be able to send a [proactive message](bot-builder-proactive-messages.md) as well.</span></span> 

<span data-ttu-id="9b420-106">Un caso común de mensajería automática es cuando el bot realiza una tarea puede llevar una cantidad indeterminada de tiempo.</span><span class="sxs-lookup"><span data-stu-id="9b420-106">A common case of proactive messaging comes when our bot is performing a task that can take an indeterminate amount of time.</span></span> <span data-ttu-id="9b420-107">En este caso, puede almacenar información sobre la tarea, indicar al usuario que el bot regresará cuando la tarea termine y dejar que la conversación continúe.</span><span class="sxs-lookup"><span data-stu-id="9b420-107">In this case, you can store information about the task, tell the user that the bot will get back to them when the task finishes, and let the conversation proceed.</span></span> <span data-ttu-id="9b420-108">Cuando la tarea se completa, el bot puede reanudar la conversación con el envío automático del mensaje de confirmación.</span><span class="sxs-lookup"><span data-stu-id="9b420-108">When the task completes, the bot can resume the conversation by sending the confirmation message proactively.</span></span>

# <a name="ctabcs"></a>[<span data-ttu-id="9b420-109">C#</span><span class="sxs-lookup"><span data-stu-id="9b420-109">C#</span></span>](#tab/cs)

## <a name="notes-about-this-sample"></a><span data-ttu-id="9b420-110">Notas sobre este ejemplo</span><span class="sxs-lookup"><span data-stu-id="9b420-110">Notes about this sample</span></span>

<span data-ttu-id="9b420-111">Se va a modificar el ejemplo básico de EchoBot.</span><span class="sxs-lookup"><span data-stu-id="9b420-111">We're modifying the basic EchoBot sample.</span></span>
- <span data-ttu-id="9b420-112">Se usará `Microsoft.Samples.Proactive` como el espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="9b420-112">We're using `Microsoft.Samples.Proactive` as the namespace.</span></span>
- <span data-ttu-id="9b420-113">Se sustituirá el archivo de estado con un archivo `JobData.cs`.</span><span class="sxs-lookup"><span data-stu-id="9b420-113">We're replacing the state file with a `JobData.cs` file.</span></span>
- <span data-ttu-id="9b420-114">Se sustituirá el archivo del bot con un archivo `ProactiveBot.cs`.</span><span class="sxs-lookup"><span data-stu-id="9b420-114">We're replacing the bot file with a `ProactiveBot.cs` file.</span></span>

> [!NOTE]
> <span data-ttu-id="9b420-115">La mensajería automática requiere actualmente que el bot tenga un identificador de aplicación y una contraseña válidos.</span><span class="sxs-lookup"><span data-stu-id="9b420-115">Proactive messaging currently requires your bot to have a valid ApplicationID and password.</span></span>


## <a name="define-task-data"></a><span data-ttu-id="9b420-116">Definición de datos de tareas</span><span class="sxs-lookup"><span data-stu-id="9b420-116">Define task data</span></span>

<span data-ttu-id="9b420-117">En este escenario, se va a realizar un seguimiento de las tareas arbitrarias que diferentes usuarios pueden crear en distintas conversaciones.</span><span class="sxs-lookup"><span data-stu-id="9b420-117">In this scenario, we're tracking arbitrary tasks that can be created by various users in different conversations.</span></span> <span data-ttu-id="9b420-118">Por tanto, se va a usar el software intermedio de estado del bot general, en lugar del software intermedio de estado del usuario o de la conversación.</span><span class="sxs-lookup"><span data-stu-id="9b420-118">So, we're using general bot state middleware, instead of user or conversation state middleware.</span></span>

<span data-ttu-id="9b420-119">La siguiente clase define la estructura de datos que se usará para los trabajos individuales.</span><span class="sxs-lookup"><span data-stu-id="9b420-119">The following class defines the data structure we'll use for individual jobs.</span></span>


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


<span data-ttu-id="9b420-120">También es necesario agregar el software intermedio de estado al código de inicio.</span><span class="sxs-lookup"><span data-stu-id="9b420-120">We also need to add the state middleware to our startup code.</span></span>


<span data-ttu-id="9b420-121">En el archivo `StartUp.cs`, actualice el método `ConfigureServices` para agregar un diccionario de trabajos al estado del bot.</span><span class="sxs-lookup"><span data-stu-id="9b420-121">In the `StartUp.cs` file, update the `ConfigureServices` method to add a dictionary of jobs to our bot state.</span></span> <span data-ttu-id="9b420-122">En el código siguiente, es la última llamada a `options.Middleware.Add`.</span><span class="sxs-lookup"><span data-stu-id="9b420-122">In the following code, it is the last call to `options.Middleware.Add`.</span></span>
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


## <a name="update-your-bot-to-create-and-run-jobs"></a><span data-ttu-id="9b420-123">Actualización del bot para crear y ejecutar trabajos</span><span class="sxs-lookup"><span data-stu-id="9b420-123">Update your bot to create and run jobs</span></span>

<span data-ttu-id="9b420-124">En cada turno, se permitirá al usuario crear un trabajo escribiendo `run` o `run job`.</span><span class="sxs-lookup"><span data-stu-id="9b420-124">Each turn, we'll let a user create a job by typing `run` or `run job`.</span></span>

<span data-ttu-id="9b420-125">Como respuesta, el bot realizará los siguientes pasos en dicho turno:</span><span class="sxs-lookup"><span data-stu-id="9b420-125">In response, our bot will take the following steps within that turn:</span></span>
- <span data-ttu-id="9b420-126">Crear el trabajo.</span><span class="sxs-lookup"><span data-stu-id="9b420-126">Create the job.</span></span>
- <span data-ttu-id="9b420-127">Registrar información sobre la conversación actual para poder enviarle el mensaje automático más adelante.</span><span class="sxs-lookup"><span data-stu-id="9b420-127">Record information about the current conversation so we can send the proactive message later.</span></span>
- <span data-ttu-id="9b420-128">Permitir que el usuario sepa que se va a iniciar el trabajo y cuándo se finalizará.</span><span class="sxs-lookup"><span data-stu-id="9b420-128">Let the user know that we are starting their job and will let them know later when it is complete.</span></span>
- <span data-ttu-id="9b420-129">Iniciar el trabajo asincrónico.</span><span class="sxs-lookup"><span data-stu-id="9b420-129">Start the asynchronous job.</span></span>
- <span data-ttu-id="9b420-130">Dejar que el turno se cierre.</span><span class="sxs-lookup"><span data-stu-id="9b420-130">Let the turn exit.</span></span>

<span data-ttu-id="9b420-131">El trabajo que se va a iniciar es un sencillo temporizador de cinco segundos que se completa con el envío del mensaje automático.</span><span class="sxs-lookup"><span data-stu-id="9b420-131">The job we're starting is a simple 5 second timer that then completes by sending the proactive message.</span></span>
- <span data-ttu-id="9b420-132">La llamada al método de continuación de la conversación del adaptador crea un turno iniciado por el bot.</span><span class="sxs-lookup"><span data-stu-id="9b420-132">The call to the adapter's continue conversation method creates a new turn initiated by the bot.</span></span>
- <span data-ttu-id="9b420-133">Este turno tiene su propio [contexto de turno](bot-builder-concept-activity-processing.md#turn-context), del que se recupera la información de estado.</span><span class="sxs-lookup"><span data-stu-id="9b420-133">This turn has its own [turn context](bot-builder-concept-activity-processing.md#turn-context) from which we retrieve the state information.</span></span>
- <span data-ttu-id="9b420-134">Este contexto se usa para enviar el mensaje automático al usuario.</span><span class="sxs-lookup"><span data-stu-id="9b420-134">We use this context to send the proactive message to the user.</span></span>



> [!NOTE]
> <span data-ttu-id="9b420-135">El método `GetAppId` es una solución alternativa para habilitar la mensajería automática en el SDK de .NET.</span><span class="sxs-lookup"><span data-stu-id="9b420-135">The `GetAppId` method is a work around to enable proactive messaging in the .NET SDK.</span></span>

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

# <a name="javascripttabjs"></a>[<span data-ttu-id="9b420-136">JavaScript</span><span class="sxs-lookup"><span data-stu-id="9b420-136">JavaScript</span></span>](#tab/js)

<span data-ttu-id="9b420-137">Para poder enviar un mensaje automático a un usuario, el usuario tendrá que enviar al menos un mensaje de estilo reactivo al bot.</span><span class="sxs-lookup"><span data-stu-id="9b420-137">Before you can send a proactive message to a user, the user will have to send at least one reactive style message to your bot.</span></span> 

<span data-ttu-id="9b420-138">Debe enviar un mensaje al bot, ya que necesita obtener una referencia al objeto de actividad y guardarlo en algún sitio para su futuro uso.</span><span class="sxs-lookup"><span data-stu-id="9b420-138">You need to send one message to the bot since it needs to get a reference to the activity object and save it somewhere for future use.</span></span> <span data-ttu-id="9b420-139">El objeto de actividad se puede considerar como la dirección de los usuarios que contiene información sobre el canal al que pertenecen, su identificador de usuario, el identificador de la conversación e incluso el servidor que debe recibir los futuros mensajes.</span><span class="sxs-lookup"><span data-stu-id="9b420-139">You can think of the activity object as the users address as it contains information about the channel they came in on, their user ID, the conversation ID, and even the server that should receive any future messages.</span></span> <span data-ttu-id="9b420-140">Se trata de un objeto JSON sencillo y debe guardarse completo sin manipularlo.</span><span class="sxs-lookup"><span data-stu-id="9b420-140">This object is simple JSON and should be saved whole without tampering.</span></span>

<span data-ttu-id="9b420-141">Se va a empezar con un fragmento de código corto que muestra cómo guardar la referencia de la conversación cada vez que el usuario diga "suscribir":</span><span class="sxs-lookup"><span data-stu-id="9b420-141">Let's start with a short code snippet that shows how to save the conversation reference anytime the user says subscribe:</span></span>
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
<span data-ttu-id="9b420-142">El fragmento de código anterior llama a la función `saveReference()` que guardará la referencia del usuario con `MemoryStorage` y devuelve `userId`.</span><span class="sxs-lookup"><span data-stu-id="9b420-142">The snippets above calls the `saveReference()` function that will save the user's reference with `MemoryStorage` and returns `userId`.</span></span> <span data-ttu-id="9b420-143">Una vez que se haya guardado correctamente la referencia, se llama a `subscribeUser()`, que notificará al usuario que se ha suscrito.</span><span class="sxs-lookup"><span data-stu-id="9b420-143">Once the reference is successfully saved we then call `subscribeUser()` which will notify the user that they have been subscribed.</span></span> 

<span data-ttu-id="9b420-144">La función `subscribeUser()` es la que configura la suscripción real.</span><span class="sxs-lookup"><span data-stu-id="9b420-144">The `subscribeUser()` function is what sets up the actual subscription.</span></span> <span data-ttu-id="9b420-145">Echemos un vistazo a una implementación sencilla que inicia un temporizador de dos segundos y que envía un mensaje automático al usuario una vez transcurrido dicho período:</span><span class="sxs-lookup"><span data-stu-id="9b420-145">Let's take a look at a simple implementation that starts a 2 second timer and proactively messages the user once the timer elapses:</span></span>

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

<span data-ttu-id="9b420-146">La función `subscribeUser()` configura un temporizador que buscará el objeto de referencia leyéndolo en el almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="9b420-146">The `subscribeUser()` function sets up a timer that will locate the reference object by reading it from storage.</span></span> <span data-ttu-id="9b420-147">Si se encontró el objeto de referencia, se puede continuar la conversación con el usuario.</span><span class="sxs-lookup"><span data-stu-id="9b420-147">If the reference object was found we can continue the conversation with the user.</span></span> <span data-ttu-id="9b420-148">El método `continueConversation` permite al bot enviar mensajes automáticos a una conversación o usuario con los que ya se ha comunicado.</span><span class="sxs-lookup"><span data-stu-id="9b420-148">The `continueConversation` method lets the bot proactively send messages to a conversation or user that it has already communicated with.</span></span>

---

## <a name="test-your-bot"></a><span data-ttu-id="9b420-149">Prueba del bot</span><span class="sxs-lookup"><span data-stu-id="9b420-149">Test your bot</span></span>

<span data-ttu-id="9b420-150">Para probar el bot, impleméntelo en Azure como un bot de solo registro y pruébelo en Chat en web o localmente con el emulador.</span><span class="sxs-lookup"><span data-stu-id="9b420-150">To test your bot, deploy it to Azure as a registration only bot, and test it in Web Chat, or test it locally using the Emulator.</span></span>
