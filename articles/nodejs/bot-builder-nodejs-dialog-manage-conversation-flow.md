---
title: Administración de un flujo de conversación con diálogos | Microsoft Docs
description: Aprenda a administrar una conversación con diálogos entre un bot y un usuario en Bot Builder SDK para Node.js.
author: v-ducvo
ms.author: v-ducvo
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: d6c8ad06b9fb198e684deae26e9cbad05a86a611
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39306176"
---
# <a name="manage-conversation-flow-with-dialogs"></a><span data-ttu-id="9355e-103">Administración del flujo de conversación con diálogos</span><span class="sxs-lookup"><span data-stu-id="9355e-103">Manage conversation flow with dialogs</span></span>
> [!div class="op_single_selector"]
> - [.NET](../dotnet/bot-builder-dotnet-manage-conversation-flow.md)
> - [Node.js](../nodejs/bot-builder-nodejs-dialog-manage-conversation-flow.md)

<span data-ttu-id="9355e-106">La administración del flujo de conversación es una tarea esencial en la compilación de bots.</span><span class="sxs-lookup"><span data-stu-id="9355e-106">Managing conversation flow is an essential task in building bots.</span></span> <span data-ttu-id="9355e-107">Un bot debe ser capaz de realizar tareas importantes con elegancia y de controlar las interrupciones correctamente.</span><span class="sxs-lookup"><span data-stu-id="9355e-107">A bot needs to be able to perform core tasks elegantly and handle interruptions gracefully.</span></span> <span data-ttu-id="9355e-108">Gracias a Bot Builder SDK para Node.js, puede administrar el flujo de conversación mediante diálogos.</span><span class="sxs-lookup"><span data-stu-id="9355e-108">With the Bot Builder SDK for Node.js, you can manage conversation flow using dialogs.</span></span>

<span data-ttu-id="9355e-109">Un diálogo es como una función de un programa.</span><span class="sxs-lookup"><span data-stu-id="9355e-109">A dialog is like a function in a program.</span></span> <span data-ttu-id="9355e-110">Por lo general, está diseñado para realizar una operación específica y se puede invocar con la frecuencia que sea necesaria.</span><span class="sxs-lookup"><span data-stu-id="9355e-110">It is generally designed to perform a specific operation and it can be invoked as often as it is needed.</span></span> <span data-ttu-id="9355e-111">Puede encadenar varios diálogos para controlar casi cualquier flujo de conversación que desee que su bot controle.</span><span class="sxs-lookup"><span data-stu-id="9355e-111">You can chain multiple dialogs together to handle just about any conversation flow that you want your bot to handle.</span></span> <span data-ttu-id="9355e-112">Bot Builder SDK para Node.js incluye características integradas como [avisos](bot-builder-nodejs-dialog-prompt.md) y [cascadas](bot-builder-nodejs-dialog-waterfall.md), que le ayudarán a administrar el flujo de conversación.</span><span class="sxs-lookup"><span data-stu-id="9355e-112">The Bot Builder SDK for Node.js includes built-in features such as [prompts](bot-builder-nodejs-dialog-prompt.md) and [waterfalls](bot-builder-nodejs-dialog-waterfall.md) to help you manage conversation flow.</span></span>

<span data-ttu-id="9355e-113">En este artículo se proporciona una serie de ejemplos para explicar cómo administrar flujos de conversación sencillos y flujos complejos en los que el bot puede controlar las interrupciones y reanudar el flujo correctamente mediante diálogos.</span><span class="sxs-lookup"><span data-stu-id="9355e-113">This article provides a series of examples to explain how to manage both simple conversation flows and complex conversation flows where your bot can handle interruptions and resume the flow gracefully using dialogs.</span></span> <span data-ttu-id="9355e-114">Los ejemplos se basan en los siguientes escenarios:</span><span class="sxs-lookup"><span data-stu-id="9355e-114">The examples are based on the following scenarios:</span></span> 

1. <span data-ttu-id="9355e-115">El bot realizará la reserva de una cena.</span><span class="sxs-lookup"><span data-stu-id="9355e-115">Your bot will take a dinner reservation.</span></span>
2. <span data-ttu-id="9355e-116">El bot puede procesar una solicitud de "Ayuda" en cualquier fase de la reserva.</span><span class="sxs-lookup"><span data-stu-id="9355e-116">Your bot can process "Help" request at any time during the reservation.</span></span>
3. <span data-ttu-id="9355e-117">El bot puede procesar ayuda contextual en el paso actual de la reserva.</span><span class="sxs-lookup"><span data-stu-id="9355e-117">Your bot can process context-sensitive "Help" for current step of the reservation.</span></span>
4. <span data-ttu-id="9355e-118">El bot puede controlar varios temas de conversación.</span><span class="sxs-lookup"><span data-stu-id="9355e-118">Your bot can handle multiple topics of conversation.</span></span>

## <a name="manage-conversation-flow-with-a-waterfall"></a><span data-ttu-id="9355e-119">Administración de un flujo de conversación mediante una cascada</span><span class="sxs-lookup"><span data-stu-id="9355e-119">Manage conversation flow with a waterfall</span></span>

<span data-ttu-id="9355e-120">Una [cascada](bot-builder-nodejs-dialog-waterfall.md) es un diálogo que permite al bot guiar fácilmente a un usuario por una serie de tareas.</span><span class="sxs-lookup"><span data-stu-id="9355e-120">A [waterfall](bot-builder-nodejs-dialog-waterfall.md) is a dialog that allows the bot to easily walk a user through a series of tasks.</span></span> <span data-ttu-id="9355e-121">En este ejemplo, el bot de reserva le hace al usuario una serie de preguntas para poder procesar la solicitud de reserva.</span><span class="sxs-lookup"><span data-stu-id="9355e-121">In this example, the reservation bot asks the user a series of questions so that the bot can process the reservation request.</span></span> <span data-ttu-id="9355e-122">El bot pedirá al usuario la información siguiente:</span><span class="sxs-lookup"><span data-stu-id="9355e-122">The bot will prompt the user for the following information:</span></span>

1. <span data-ttu-id="9355e-123">Fecha y hora de la reserva</span><span class="sxs-lookup"><span data-stu-id="9355e-123">Reservation date and time</span></span>
2. <span data-ttu-id="9355e-124">Número de asistentes</span><span class="sxs-lookup"><span data-stu-id="9355e-124">Number of people in the party</span></span>
3. <span data-ttu-id="9355e-125">Nombre de la persona que realiza la reserva</span><span class="sxs-lookup"><span data-stu-id="9355e-125">Name of the person making the reservation</span></span>

<span data-ttu-id="9355e-126">El siguiente ejemplo de código le muestra cómo usar una cascada para guiar al usuario por una serie de avisos.</span><span class="sxs-lookup"><span data-stu-id="9355e-126">The following code sample shows how to use a waterfall to guide the user through a series of prompts.</span></span>

```javascript
var inMemoryStorage = new builder.MemoryBotStorage();

// This is a dinner reservation bot that uses a waterfall technique to prompt users for input.
var bot = new builder.UniversalBot(connector, [
    function (session) {
        session.send("Welcome to the dinner reservation.");
        builder.Prompts.time(session, "Please provide a reservation date and time (e.g.: June 6th at 5pm)");
    },
    function (session, results) {
        session.dialogData.reservationDate = builder.EntityRecognizer.resolveTime([results.response]);
        builder.Prompts.number(session, "How many people are in your party?");
    },
    function (session, results) {
        session.dialogData.partySize = results.response;
        builder.Prompts.text(session, "Whose name will this reservation be under?");
    },
    function (session, results) {
        session.dialogData.reservationName = results.response;
        
        // Process request and display reservation details
        session.send(`Reservation confirmed. Reservation details: <br/>Date/Time: ${session.dialogData.reservationDate} <br/>Party size: ${session.dialogData.partySize} <br/>Reservation name: ${session.dialogData.reservationName}`);
        session.endDialog();
    }
]).set('storage', inMemoryStorage); // Register in-memory storage 
```

<span data-ttu-id="9355e-127">La funcionalidad principal de este bot se produce en el diálogo predeterminado.</span><span class="sxs-lookup"><span data-stu-id="9355e-127">The core functionality of this bot occurs in the default dialog.</span></span> <span data-ttu-id="9355e-128">El diálogo predeterminado se define cuando se crea el bot:</span><span class="sxs-lookup"><span data-stu-id="9355e-128">The default dialog is defined when the bot is created:</span></span> 

```javascript
var bot = new builder.UniversalBot(connector, [..waterfall steps..]); 
```

<span data-ttu-id="9355e-129">Además, durante este proceso de creación, puede establecer qué [almacenamiento de datos](bot-builder-nodejs-state.md) va a usar.</span><span class="sxs-lookup"><span data-stu-id="9355e-129">Also, during this creation process, you can set which [data storage](bot-builder-nodejs-state.md) you want to use.</span></span> <span data-ttu-id="9355e-130">Por ejemplo, para usar el almacenamiento en memoria, puede establecerlo de la siguiente forma:</span><span class="sxs-lookup"><span data-stu-id="9355e-130">For instance, to use the in-memory storage, you can set it as follows:</span></span>

```javascript
var inMemoryStorage = new builder.MemoryBotStorage();
var bot = new builder.UniversalBot(connector, [..waterfall steps..]).set('storage', inMemoryStorage); // Register in-memory storage 
```

<span data-ttu-id="9355e-131">El diálogo predeterminado se crea como una matriz de funciones que define los pasos de la cascada.</span><span class="sxs-lookup"><span data-stu-id="9355e-131">The default dialog is created as an array of functions that define the steps of the waterfall.</span></span> <span data-ttu-id="9355e-132">En el ejemplo hay cuatro funciones, por lo que la cascada consta de cuatro pasos.</span><span class="sxs-lookup"><span data-stu-id="9355e-132">In the example, there are four functions so the waterfall has four steps.</span></span> <span data-ttu-id="9355e-133">Cada paso realiza una sola tarea y los resultados se procesan en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="9355e-133">Each step performs a single task and the results are processed in the next step.</span></span> <span data-ttu-id="9355e-134">El proceso continúa hasta el último paso, en el que se confirma la reserva y finaliza el diálogo.</span><span class="sxs-lookup"><span data-stu-id="9355e-134">The process continues until the last step, where the reservation is confirmed and the dialog ends.</span></span>

<span data-ttu-id="9355e-135">En la siguiente captura de pantalla se muestran los resultados de este bot que se ejecuta en [Bot Framework Emulator](../bot-service-debug-emulator.md):</span><span class="sxs-lookup"><span data-stu-id="9355e-135">The following screen shot shows the results of this bot running in the [Bot Framework Emulator](../bot-service-debug-emulator.md):</span></span>

![Administración del flujo de conversación mediante una cascada](../media/bot-builder-nodejs-dialog-manage-conversation/waterfall-results.png)

### <a name="prompt-user-for-input"></a><span data-ttu-id="9355e-137">Petición de datos de entrada al usuario</span><span class="sxs-lookup"><span data-stu-id="9355e-137">Prompt user for input</span></span>

<span data-ttu-id="9355e-138">Cada paso de este ejemplo usa un aviso para solicitar datos de entrada al usuario.</span><span class="sxs-lookup"><span data-stu-id="9355e-138">Each step of this example uses a prompt to ask the user for input.</span></span> <span data-ttu-id="9355e-139">Un aviso es un tipo especial de diálogo que solicita datos de entrada al usuario, espera una respuesta y devuelve la respuesta al paso siguiente de la cascada.</span><span class="sxs-lookup"><span data-stu-id="9355e-139">A prompt is a special type of dialog that asks for user input, waits for a response, and returns the response to the next step in the waterfall.</span></span> <span data-ttu-id="9355e-140">Consulte [Petición de datos de entrada al usuario](bot-builder-nodejs-dialog-prompt.md) para más información acerca de los muchos tipos diferentes de avisos que puede usar en su bot.</span><span class="sxs-lookup"><span data-stu-id="9355e-140">See [Prompt users for input](bot-builder-nodejs-dialog-prompt.md) for information about the many different types of prompts you can use in your bot.</span></span>

<span data-ttu-id="9355e-141">En este ejemplo, el bot usa `Prompts.text()` para solicitar una respuesta libre al usuario en formato de texto.</span><span class="sxs-lookup"><span data-stu-id="9355e-141">In this example, the bot uses `Prompts.text()` to solicit a freeform response from the user in text format.</span></span> <span data-ttu-id="9355e-142">El usuario puede responder con cualquier texto y el bot debe decidir cómo controlar la respuesta.</span><span class="sxs-lookup"><span data-stu-id="9355e-142">The user can respond with any text and the bot must decide how to handle the response.</span></span> <span data-ttu-id="9355e-143">`Prompts.time()` usa la biblioteca [Chrono](https://github.com/wanasit/chrono) para analizar una cadena en busca de información sobre la fecha y hora.</span><span class="sxs-lookup"><span data-stu-id="9355e-143">`Prompts.time()` uses the [Chrono](https://github.com/wanasit/chrono) library to parse for date and time information from a string.</span></span> <span data-ttu-id="9355e-144">Esto permite que su bot comprenda más lenguaje natural para especificar la fecha y la hora.</span><span class="sxs-lookup"><span data-stu-id="9355e-144">This allows your bot to understand more natural language for specifying date and time.</span></span> <span data-ttu-id="9355e-145">Por ejemplo: "6 de junio de 2017 a las 9 pm", "hoy a las 7:30 pm", "el próximo lunes a las 6 pm", etc.</span><span class="sxs-lookup"><span data-stu-id="9355e-145">For example: "June 6th, 2017 at 9pm", "Today at 7:30pm", "next monday at 6pm", and so on.</span></span>

> [!TIP] 
> La hora que especifica el usuario se convierte a la hora UTC en función de la zona horaria del servidor que hospeda el bot. Como el servidor puede estar ubicado en una zona horaria diferente a la del usuario, asegúrese de tener en cuenta las zonas horarias. Para convertir la fecha y hora a la hora local del usuario, considere la posibilidad de pedir al usuario que indique en qué zona horaria se encuentra.

## <a name="manage-a-conversation-flow-with-multiple-dialogs"></a><span data-ttu-id="9355e-149">Administración de un flujo de conversación con varios diálogos</span><span class="sxs-lookup"><span data-stu-id="9355e-149">Manage a conversation flow with multiple dialogs</span></span>

<span data-ttu-id="9355e-150">Otra técnica para administrar un flujo de conversación es usar una combinación de cascada y varios diálogos.</span><span class="sxs-lookup"><span data-stu-id="9355e-150">Another technique for managing conversation flow is to use a combination of waterfall and multiple dialogs.</span></span> <span data-ttu-id="9355e-151">La cascada le permite encadenar funciones en un diálogo, mientras que estos le permiten dividir una conversación en fragmentos más pequeños de funcionalidad que se pueden reutilizar en cualquier momento.</span><span class="sxs-lookup"><span data-stu-id="9355e-151">The waterfall allows you to chain functions together in a dialog, while dialogs enable you to break a conversation into smaller pieces of functionality that can be reused at any time.</span></span>

<span data-ttu-id="9355e-152">Por ejemplo, piense en el bot para la reserva de una cena.</span><span class="sxs-lookup"><span data-stu-id="9355e-152">For example, consider the dinner reservation bot.</span></span> <span data-ttu-id="9355e-153">El siguiente ejemplo de código le muestra el ejemplo anterior reescrito para poder usar una cascada y varios diálogos.</span><span class="sxs-lookup"><span data-stu-id="9355e-153">The following code sample shows the previous example rewritten to use waterfall and multiple dialogs.</span></span>

```javascript
var inMemoryStorage = new builder.MemoryBotStorage();

// This is a dinner reservation bot that uses multiple dialogs to prompt users for input.
var bot = new builder.UniversalBot(connector, [
    function (session) {
        session.send("Welcome to the dinner reservation.");
        session.beginDialog('askForDateTime');
    },
    function (session, results) {
        session.dialogData.reservationDate = builder.EntityRecognizer.resolveTime([results.response]);
        session.beginDialog('askForPartySize');
    },
    function (session, results) {
        session.dialogData.partySize = results.response;
        session.beginDialog('askForReserverName');
    },
    function (session, results) {
        session.dialogData.reservationName = results.response;

        // Process request and display reservation details
        session.send(`Reservation confirmed. Reservation details: <br/>Date/Time: ${session.dialogData.reservationDate} <br/>Party size: ${session.dialogData.partySize} <br/>Reservation name: ${session.dialogData.reservationName}`);
        session.endDialog();
    }
]).set('storage', inMemoryStorage); // Register in-memory storage 

// Dialog to ask for a date and time
bot.dialog('askForDateTime', [
    function (session) {
        builder.Prompts.time(session, "Please provide a reservation date and time (e.g.: June 6th at 5pm)");
    },
    function (session, results) {
        session.endDialogWithResult(results);
    }
]);

// Dialog to ask for number of people in the party
bot.dialog('askForPartySize', [
    function (session) {
        builder.Prompts.text(session, "How many people are in your party?");
    },
    function (session, results) {
        session.endDialogWithResult(results);
    }
])

// Dialog to ask for the reservation name.
bot.dialog('askForReserverName', [
    function (session) {
        builder.Prompts.text(session, "Who's name will this reservation be under?");
    },
    function (session, results) {
        session.endDialogWithResult(results);
    }
]);
```

<span data-ttu-id="9355e-154">Los resultados de la ejecución de este bot son exactamente los mismos que los del bot anterior en el que solo se usaba una cascada.</span><span class="sxs-lookup"><span data-stu-id="9355e-154">The results from executing this bot are exactly the same as the previous bot where only waterfall is used.</span></span> <span data-ttu-id="9355e-155">No obstante, desde el punto de vista de la programación, hay dos diferencias principales:</span><span class="sxs-lookup"><span data-stu-id="9355e-155">However, programmatically, there are two primary differences:</span></span>

1. <span data-ttu-id="9355e-156">El diálogo predeterminado se dedica a administrar el flujo de la conversación.</span><span class="sxs-lookup"><span data-stu-id="9355e-156">The default dialog is dedicated to managing the flow of the conversation.</span></span>
2. <span data-ttu-id="9355e-157">Un diálogo independiente administra la tarea de cada paso de la conversación.</span><span class="sxs-lookup"><span data-stu-id="9355e-157">The task for each step of the conversation is being managed by a separate dialog.</span></span> <span data-ttu-id="9355e-158">En este caso, el bot necesitaba tres fragmentos de información, por lo que realiza tres avisos al usuario.</span><span class="sxs-lookup"><span data-stu-id="9355e-158">In this case, the bot needed three pieces of information so it prompts the user three times.</span></span> <span data-ttu-id="9355e-159">Ahora, cada aviso está contenido en su propio diálogo.</span><span class="sxs-lookup"><span data-stu-id="9355e-159">Each prompt is now contained within its own dialog.</span></span>

<span data-ttu-id="9355e-160">Con esta técnica, puede separar el flujo de conversación de la lógica de la tarea.</span><span class="sxs-lookup"><span data-stu-id="9355e-160">With this technique, you can separate the conversation flow from the task logic.</span></span> <span data-ttu-id="9355e-161">Esto permite que los diálogos se puedan reutilizar en flujos de conversación diferentes si fuera necesario.</span><span class="sxs-lookup"><span data-stu-id="9355e-161">This allows the dialogs to be reused by different conversation flows if necessary.</span></span> 

## <a name="respond-to-user-input"></a><span data-ttu-id="9355e-162">Respuesta a entradas del usuario</span><span class="sxs-lookup"><span data-stu-id="9355e-162">Respond to user input</span></span>

<span data-ttu-id="9355e-163">En el proceso de guiar al usuario por una serie de tareas, si el usuario tiene preguntas o si desea solicitar información adicional antes de responder, ¿cómo se podrían controlar esas solicitudes?</span><span class="sxs-lookup"><span data-stu-id="9355e-163">In the process of guiding the user through a series of tasks, if the user has questions or wants to request additional information before answering, how would you handle those requests?</span></span> <span data-ttu-id="9355e-164">Por ejemplo, independientemente de donde esté el usuario de la conversación, ¿cómo debería responder el bot si el usuario escribe "Ayuda", "Asistencia" o "Cancelar"?</span><span class="sxs-lookup"><span data-stu-id="9355e-164">For example, regardless of where the user is in the conversation, how would the bot respond if the user enters "Help", "Support" or "Cancel"?</span></span> <span data-ttu-id="9355e-165">¿Qué ocurre si el usuario desea información adicional sobre un paso?</span><span class="sxs-lookup"><span data-stu-id="9355e-165">What if the user wants additional information about a step?</span></span> <span data-ttu-id="9355e-166">¿Qué ocurre si el usuario cambia de opinión y desea abandonar la tarea actual para iniciar una tarea completamente diferente?</span><span class="sxs-lookup"><span data-stu-id="9355e-166">What happens if the user changes their mind and wants to abandon the current task to start a completely different task?</span></span>

<span data-ttu-id="9355e-167">Bot Builder SDK para Node.js permite que un bot escuche determinadas entradas en un contexto global o en uno local en el ámbito del diálogo actual.</span><span class="sxs-lookup"><span data-stu-id="9355e-167">The Bot Builder SDK for Node.js allows a bot to listen for certain input within a global context or within a local context in the scope of the current dialog.</span></span> <span data-ttu-id="9355e-168">Estas entradas se denominan [acciones](bot-builder-nodejs-dialog-actions.md), y permiten que el bot escuche las entradas del usuario según una cláusula `matches`.</span><span class="sxs-lookup"><span data-stu-id="9355e-168">These inputs are called [actions](bot-builder-nodejs-dialog-actions.md), which allow the bot to listen for user input based on a `matches` clause.</span></span> <span data-ttu-id="9355e-169">Depende del bot decidir cómo responder a entradas específicas del usuario.</span><span class="sxs-lookup"><span data-stu-id="9355e-169">It's up to the bot to decide how to react to specific user inputs.</span></span>

### <a name="handle-global-action"></a><span data-ttu-id="9355e-170">Control de acciones globales</span><span class="sxs-lookup"><span data-stu-id="9355e-170">Handle global action</span></span>

<span data-ttu-id="9355e-171">Si desea que el bot pueda controlar acciones en cualquier punto de una conversación, use `triggerAction`.</span><span class="sxs-lookup"><span data-stu-id="9355e-171">If you want your bot to be able to handle actions at any point in a conversation, use `triggerAction`.</span></span> <span data-ttu-id="9355e-172">Los desencadenadores permiten al bot invocar un diálogo específico si la entrada coincide con el término especificado.</span><span class="sxs-lookup"><span data-stu-id="9355e-172">Triggers allow the bot to invoke a specific dialog when the input matches the specified term.</span></span> <span data-ttu-id="9355e-173">Por ejemplo, si desea admitir una opción global de "Ayuda", puede crear un diálogo de ayuda y adjuntar una acción `triggerAction` que escuche en busca de entradas que coincidan con "Ayuda".</span><span class="sxs-lookup"><span data-stu-id="9355e-173">For example, if you want to support a global "Help" option, you can create a help dialog and attach a `triggerAction` that listens for input matching "Help".</span></span>

<span data-ttu-id="9355e-174">El ejemplo de código siguiente le muestra cómo adjuntar una acción `triggerAction` a un diálogo para especificar que se debe invocar al diálogo cuando el usuario escribe "ayuda".</span><span class="sxs-lookup"><span data-stu-id="9355e-174">The following code sample shows how to attach a `triggerAction` to a dialog to specify that the dialog should be invoked when the user enters "help".</span></span>

```javascript
// The dialog stack is cleared and this dialog is invoked when the user enters 'help'.
bot.dialog('help', function (session, args, next) {
    session.endDialog("This is a bot that can help you make a dinner reservation. <br/>Please say 'next' to continue");
})
.triggerAction({
    matches: /^help$/i,
});
```

<span data-ttu-id="9355e-175">De forma predeterminada, cuando se ejecuta una acción `triggerAction`, la pila del diálogo se borra y el diálogo desencadenado se convierte en el nuevo diálogo predeterminado.</span><span class="sxs-lookup"><span data-stu-id="9355e-175">By default, when a `triggerAction` executes, the dialog stack is cleared and the triggered dialog becomes the new default dialog.</span></span> <span data-ttu-id="9355e-176">En este ejemplo, cuando se ejecuta la acción `triggerAction`, se borra la pila de diálogo y el diálogo `help` se agrega a la pila como nuevo diálogo predeterminado.</span><span class="sxs-lookup"><span data-stu-id="9355e-176">In this example, when the `triggerAction` executes, the dialog stack is cleared and the `help` dialog is then added to the stack as the new default dialog.</span></span> <span data-ttu-id="9355e-177">Si este no es el comportamiento deseado, puede agregar la opción `onSelectAction` a la acción `triggerAction`.</span><span class="sxs-lookup"><span data-stu-id="9355e-177">If this is not the desired behavior, you can add the `onSelectAction` option to the `triggerAction`.</span></span> <span data-ttu-id="9355e-178">La opción `onSelectAction` permite al bot iniciar un nuevo diálogo sin borrar la pila de diálogo, lo que permite redirigir la conversación temporalmente y volver a reanudarla más adelante donde lo dejó.</span><span class="sxs-lookup"><span data-stu-id="9355e-178">The `onSelectAction` option allows the bot to start a new dialog without clearing the dialog stack, which enables the conversation to be temporarily redirected and later resume where it left off.</span></span>

<span data-ttu-id="9355e-179">En el ejemplo de código siguiente se muestra cómo usar la opción `onSelectAction` con la acción `triggerAction` para que el diálogo `help` se agregue a la pila de diálogos existente (y esta no se borra).</span><span class="sxs-lookup"><span data-stu-id="9355e-179">The following code sample shows how to use the `onSelectAction` option with `triggerAction` so that the `help` dialog is added to the existing dialog stack (and the dialog stack is not cleared).</span></span>

```javascript
bot.dialog('help', function (session, args, next) {
    session.endDialog("This is a bot that can help you make a dinner reservation. <br/>Please say 'next' to continue");
})
.triggerAction({
    matches: /^help$/i,
    onSelectAction: (session, args, next) => {
        // Add the help dialog to the dialog stack 
        // (override the default behavior of replacing the stack)
        session.beginDialog(args.action, args);
    }
});
```

<span data-ttu-id="9355e-180">En este ejemplo, el diálogo `help` toma el control de la conversación cuando el usuario escribe "ayuda".</span><span class="sxs-lookup"><span data-stu-id="9355e-180">In this example, the `help` dialog takes control of the conversation when the user enters "help".</span></span> <span data-ttu-id="9355e-181">Como la acción `triggerAction` contiene la opción `onSelectAction`, el diálogo `help` se inserta en la pila de diálogo existente y esta no se borra.</span><span class="sxs-lookup"><span data-stu-id="9355e-181">Because the `triggerAction` contains the `onSelectAction` option, the `help` dialog is pushed onto the existing dialog stack and the stack is not cleared.</span></span> <span data-ttu-id="9355e-182">Cuando finaliza el diálogo `help`, se elimina de la pila de diálogo y la conversación se reanuda en el punto en que se interrumpió con el comando `help`.</span><span class="sxs-lookup"><span data-stu-id="9355e-182">When the `help` dialog ends, it is removed from the dialog stack and the conversation resumes from the point at which it was interrupted with the `help` command.</span></span>

### <a name="handle-contextual-action"></a><span data-ttu-id="9355e-183">Control de acciones contextuales</span><span class="sxs-lookup"><span data-stu-id="9355e-183">Handle contextual action</span></span>

<span data-ttu-id="9355e-184">En el ejemplo anterior, se invoca el diálogo `help` si el usuario escribe "ayuda" en cualquier punto de la conversación.</span><span class="sxs-lookup"><span data-stu-id="9355e-184">In the previous example, the `help` dialog is invoked if the user enters "help" at any point in the conversation.</span></span> <span data-ttu-id="9355e-185">En este caso, el diálogo solo puede ofrecer orientaciones de ayuda generales ya que no hay ningún contexto específico para la solicitud de ayuda del usuario.</span><span class="sxs-lookup"><span data-stu-id="9355e-185">As such, the dialog can only offer general help guidance, as there is no specific context for the user's help request.</span></span> <span data-ttu-id="9355e-186">Pero, ¿qué ocurre si el usuario desea solicitar ayuda con respecto a un momento específico de la conversación?</span><span class="sxs-lookup"><span data-stu-id="9355e-186">But, what if the user wants to request help regarding a specific point in the conversation?</span></span> <span data-ttu-id="9355e-187">En ese caso, el diálogo `help` se debe desencadenar dentro del contexto del diálogo actual.</span><span class="sxs-lookup"><span data-stu-id="9355e-187">In that case, the `help` dialog must be triggered within the context of the current dialog.</span></span>

<span data-ttu-id="9355e-188">Por ejemplo, piense en el bot para la reserva de una cena.</span><span class="sxs-lookup"><span data-stu-id="9355e-188">For example, consider the dinner reservation bot.</span></span> <span data-ttu-id="9355e-189">¿Qué sucede si un usuario desea conocer el tamaño máximo del grupo cuando se le pregunta por el número de asistentes de su grupo?</span><span class="sxs-lookup"><span data-stu-id="9355e-189">What if a user wants to know the maximum party size when they are asked for the number of members in their party?</span></span> <span data-ttu-id="9355e-190">Para manejar este escenario, puede adjuntar una acción `beginDialogAction` al diálogo `askForPartySize`, que escuche la entrada de usuario "ayuda".</span><span class="sxs-lookup"><span data-stu-id="9355e-190">To handle this scenario, you can attach a `beginDialogAction` to the `askForPartySize` dialog, listening for the user input "help".</span></span>

<span data-ttu-id="9355e-191">El siguiente ejemplo de código muestra cómo adjuntar ayuda contextual a un diálogo mediante `beginDialogAction`.</span><span class="sxs-lookup"><span data-stu-id="9355e-191">The following code sample shows how to attach context-sensitive help to a dialog using `beginDialogAction`.</span></span>

```javascript
// Dialog to ask for number of people in the party
bot.dialog('askForPartySize', [
    function (session) {
        builder.Prompts.text(session, "How many people are in your party?");
    },
    function (session, results) {
       session.endDialogWithResult(results);
    }
])
.beginDialogAction('partySizeHelpAction', 'partySizeHelp', { matches: /^help$/i });

// Context Help dialog for party size
bot.dialog('partySizeHelp', function(session, args, next) {
    var msg = "Party size help: Our restaurant can support party sizes up to 150 members.";
    session.endDialog(msg);
})
```

<span data-ttu-id="9355e-192">En este ejemplo, cada vez que el usuario escriba "help", el bot insertará el diálogo `partySizeHelp` en la pila.</span><span class="sxs-lookup"><span data-stu-id="9355e-192">In this example, whenever the user enters "help", the bot will push the `partySizeHelp` dialog onto the stack.</span></span> <span data-ttu-id="9355e-193">Ese diálogo envía un mensaje de ayuda al usuario y, después, finaliza el diálogo y devuelve el control de nuevo al diálogo `askForPartySize`, que vuelve a preguntar el tamaño del grupo al usuario.</span><span class="sxs-lookup"><span data-stu-id="9355e-193">That dialog sends a help message to the user and then ends the dialog, returning control back to the `askForPartySize` dialog which reprompts the user for a party size.</span></span>

<span data-ttu-id="9355e-194">Es importante tener en cuenta que esta ayuda contextual solo se ejecuta cuando el usuario está en el diálogo `askForPartySize`.</span><span class="sxs-lookup"><span data-stu-id="9355e-194">It is important to note that this context-sensitive help is only executed when the user is in the `askForPartySize` dialog.</span></span> <span data-ttu-id="9355e-195">En caso contrario, se ejecutará el mensaje de ayuda de la acción `triggerAction` en su lugar.</span><span class="sxs-lookup"><span data-stu-id="9355e-195">Otherwise, the general help message from the `triggerAction` will execute instead.</span></span> <span data-ttu-id="9355e-196">En otras palabras, la cláusula local `matches` siempre tiene prioridad sobre la cláusula global `matches`.</span><span class="sxs-lookup"><span data-stu-id="9355e-196">In other words, the local `matches` clause always takes precedence over the global `matches` clause.</span></span> <span data-ttu-id="9355e-197">Por ejemplo, si una acción `beginDialogAction` coincide con **ayuda**, las coincidencias de **ayuda** de la acción `triggerAction` no se ejecutarán.</span><span class="sxs-lookup"><span data-stu-id="9355e-197">For example, if a `beginDialogAction` matches for **help**, then the matches for **help** in the `triggerAction` will not be executed.</span></span> <span data-ttu-id="9355e-198">Para más información, consulte [Prioridad de acciones](bot-builder-nodejs-dialog-actions.md#action-precedence).</span><span class="sxs-lookup"><span data-stu-id="9355e-198">For more information, see [Action precedence](bot-builder-nodejs-dialog-actions.md#action-precedence).</span></span>

### <a name="change-the-topic-of-conversation"></a><span data-ttu-id="9355e-199">Cambio del tema de conversación</span><span class="sxs-lookup"><span data-stu-id="9355e-199">Change the topic of conversation</span></span>

<span data-ttu-id="9355e-200">De forma predeterminada, la ejecución de una acción `triggerAction` borra la pila de diálogo y restablece la conversación, empezando por el diálogo especificado.</span><span class="sxs-lookup"><span data-stu-id="9355e-200">By default, executing a `triggerAction` clears the dialog stack and resets the conversation, starting with the specified dialog.</span></span> <span data-ttu-id="9355e-201">Este comportamiento es preferible cuando el bot tiene que cambiar de un tema de conversación a otro como, por ejemplo, cuando un usuario en medio del proceso normal de reserva de una cena decide pedir que se le sirva la cena en la habitación de su hotel.</span><span class="sxs-lookup"><span data-stu-id="9355e-201">This behavior is often preferable when your bot needs to switch from one topic of conversation to another, such as if a user in the midst of booking a dinner reservation instead decides to order dinner to be delivered to their hotel room.</span></span> 

<span data-ttu-id="9355e-202">El siguiente ejemplo se basa en el anterior de forma que el bot permita al usuario hacer una reserva para cenar en un restaurante o bien pedir que se le envíe la cena.</span><span class="sxs-lookup"><span data-stu-id="9355e-202">The following example builds upon the previous one such that the bot allows the user to either make a dinner reservation or order dinner to be delivered.</span></span> <span data-ttu-id="9355e-203">En este bot, el diálogo predeterminado es un diálogo de saludo que presenta dos opciones para el usuario: `Dinner Reservation` y `Order Dinner`.</span><span class="sxs-lookup"><span data-stu-id="9355e-203">In this bot, the default dialog is a greeting dialog that presents two options to the user: `Dinner Reservation` and `Order Dinner`.</span></span>

```javascript
var inMemoryStorage = new builder.MemoryBotStorage();

// This bot enables users to either make a dinner reservation or order dinner.
var bot = new builder.UniversalBot(connector, function(session){
    var msg = "Welcome to the reservation bot. Please say `Dinner Reservation` or `Order Dinner`";
    session.send(msg);
}).set('storage', inMemoryStorage); // Register in-memory storage 
```

<span data-ttu-id="9355e-204">La lógica de reserva de cena que estaba en el diálogo predeterminado del ejemplo anterior está ahora en su propio diálogo llamado `dinnerReservation`.</span><span class="sxs-lookup"><span data-stu-id="9355e-204">The dinner reservation logic that was in the default dialog of the previous example is now in its own dialog called `dinnerReservation`.</span></span> <span data-ttu-id="9355e-205">El flujo de `dinnerReservation` sigue siendo el mismo que en la versión de varios diálogos analizada anteriormente.</span><span class="sxs-lookup"><span data-stu-id="9355e-205">The flow of the `dinnerReservation` remains the same as the multiple dialog version discussed earlier.</span></span> <span data-ttu-id="9355e-206">La única diferencia es que el diálogo tiene una acción `triggerAction` adjunta.</span><span class="sxs-lookup"><span data-stu-id="9355e-206">The only difference is that the dialog has a `triggerAction` attached to it.</span></span> <span data-ttu-id="9355e-207">Observe que en esta versión, `confirmPrompt` pide al usuario que confirme que desea cambiar el tema de conversación antes de invocar el nuevo diálogo.</span><span class="sxs-lookup"><span data-stu-id="9355e-207">Notice that in this version, the `confirmPrompt` asks the user to confirm that they want to change the topic of conversation, before invoking the new dialog.</span></span> <span data-ttu-id="9355e-208">Es recomendable incluir un aviso `confirmPrompt` en escenarios como este porque una vez que se borra la pila de diálogo, se redirigirá al usuario al nuevo tema de conversación, abandonando por tanto el tema que estaba en curso.</span><span class="sxs-lookup"><span data-stu-id="9355e-208">It is good practice to include a `confirmPrompt` in scenarios like this because once the dialog stack is cleared, the user will be directed to the new topic of conversation, thereby abandoning the topic of conversation that was underway.</span></span>

```javascript
// This dialog helps the user make a dinner reservation.
bot.dialog('dinnerReservation', [
    function (session) {
        session.send("Welcome to the dinner reservation.");
        session.beginDialog('askForDateTime');
    },
    function (session, results) {
        session.dialogData.reservationDate = builder.EntityRecognizer.resolveTime([results.response]);
        session.beginDialog('askForPartySize');
    },
    function (session, results) {
        session.dialogData.partySize = results.response;
        session.beginDialog('askForReserverName');
    },
    function (session, results) {
        session.dialogData.reservationName = results.response;

        // Process request and display reservation details
        session.send(`Reservation confirmed. Reservation details: <br/>Date/Time: ${session.dialogData.reservationDate} <br/>Party size: ${session.dialogData.partySize} <br/>Reservation name: ${session.dialogData.reservationName}`);
        session.endDialog();
    }
])
.triggerAction({
    matches: /^dinner reservation$/i,
    confirmPrompt: "This will cancel your current request. Are you sure?"
});
```

<span data-ttu-id="9355e-209">El segundo tema de conversación se define en el diálogo `orderDinner` mediante una cascada.</span><span class="sxs-lookup"><span data-stu-id="9355e-209">The second topic of conversation is defined in the `orderDinner` dialog using a waterfall.</span></span> <span data-ttu-id="9355e-210">Este diálogo simplemente muestra el menú de la cena y pide al usuario el número de una habitación una vez especificado el pedido.</span><span class="sxs-lookup"><span data-stu-id="9355e-210">This dialog simply displays the dinner menu and prompts the user for a room number after the order is specified.</span></span> <span data-ttu-id="9355e-211">Se adjunta una acción `triggerAction` al diálogo para especificar que debe invocarse si el usuario escribe "encargar cena", y para asegurarse de que se pide al usuario que confirme su selección en caso de que muestre intención de cambiar el tema de conversación.</span><span class="sxs-lookup"><span data-stu-id="9355e-211">A `triggerAction` is attached to the dialog to specify that it should be invoked when the user enters "order dinner" and to ensure that the user is prompted to confirm their selection, should they indicate a desire to change the topic of conversation.</span></span>

```javascript
// This dialog help the user order dinner to be delivered to their hotel room.
var dinnerMenu = {
    "Potato Salad - $5.99": {
        Description: "Potato Salad",
        Price: 5.99
    },
    "Tuna Sandwich - $6.89": {
        Description: "Tuna Sandwich",
        Price: 6.89
    },
    "Clam Chowder - $4.50":{
        Description: "Clam Chowder",
        Price: 4.50
    }
};

bot.dialog('orderDinner', [
    function(session){
        session.send("Lets order some dinner!");
        builder.Prompts.choice(session, "Dinner menu:", dinnerMenu);
    },
    function (session, results) {
        if (results.response) {
            var order = dinnerMenu[results.response.entity];
            var msg = `You ordered: ${order.Description} for a total of $${order.Price}.`;
            session.dialogData.order = order;
            session.send(msg);
            builder.Prompts.text(session, "What is your room number?");
        } 
    },
    function(session, results){
        if(results.response){
            session.dialogData.room = results.response;
            var msg = `Thank you. Your order will be delivered to room #${session.dialogData.room}`;
            session.endDialog(msg);
        }
    }
])
.triggerAction({
    matches: /^order dinner$/i,
    confirmPrompt: "This will cancel your order. Are you sure?"
});
```

<span data-ttu-id="9355e-212">Una vez que el usuario inicia una conversación y selecciona `Dinner Reservation` o `Order Dinner`, puede cambiar de opinión en cualquier momento.</span><span class="sxs-lookup"><span data-stu-id="9355e-212">After the user starts a conversation and selects either `Dinner Reservation` or `Order Dinner`, they may change their mind at any time.</span></span> <span data-ttu-id="9355e-213">Por ejemplo, si el usuario está en medio del proceso para realizar una reserva de cena y escribe "encargar cena", el bot confirmará mediante un mensaje del tipo: "Esto cancelará la solicitud actual.</span><span class="sxs-lookup"><span data-stu-id="9355e-213">For example, if the user is in the middle of making a dinner reservation and enters "order dinner," the bot will confirm by saying, "This will cancel your current request.</span></span> <span data-ttu-id="9355e-214">¿Está seguro de que desea continuar?</span><span class="sxs-lookup"><span data-stu-id="9355e-214">Are you sure?".</span></span> <span data-ttu-id="9355e-215">Si el usuario escribe "no", se cancelará la solicitud y el usuario podrá continuar con el proceso de reserva de la cena.</span><span class="sxs-lookup"><span data-stu-id="9355e-215">If the user types "no," then the request is canceled and the user can continue with the dinner reservation process.</span></span> <span data-ttu-id="9355e-216">Si el usuario escribe "sí", el bot borrará la pila de diálogo y transferirá el control de la conversación al diálogo `orderDinner`.</span><span class="sxs-lookup"><span data-stu-id="9355e-216">If the user types "yes," the bot will clear the dialog stack and transfer control of the conversation to the `orderDinner` dialog.</span></span>

## <a name="end-conversation"></a><span data-ttu-id="9355e-217">Fin de la conversación</span><span class="sxs-lookup"><span data-stu-id="9355e-217">End conversation</span></span>

<span data-ttu-id="9355e-218">En los ejemplos anteriores, los diálogos se cierran con `session.endDialog` o `session.endDialogWithResult`. Ambas opciones cierran el diálogo, lo borran de la pila y devuelven el control al diálogo de llamada.</span><span class="sxs-lookup"><span data-stu-id="9355e-218">In the examples above, dialogs are closed by either using `session.endDialog` or `session.endDialogWithResult`, both of which end the dialog, remove it from the stack, and return control to the calling dialog.</span></span> <span data-ttu-id="9355e-219">En los casos en los que el usuario ha llegado al final de la conversación, se debería usar `session.endConversation` para indicar que la conversación ha finalizado.</span><span class="sxs-lookup"><span data-stu-id="9355e-219">In situations where the user has reached the end of the conversation, you should use `session.endConversation` to indicate that the conversation is finished.</span></span>

<span data-ttu-id="9355e-220">El método [`session.endConversation`](https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.session#endconversation) finaliza una conversación y, opcionalmente, envía un mensaje al usuario.</span><span class="sxs-lookup"><span data-stu-id="9355e-220">The [`session.endConversation`](https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.session#endconversation) method ends a conversation and optionally sends a message to the user.</span></span> <span data-ttu-id="9355e-221">Por ejemplo, el diálogo `orderDinner` del ejemplo anterior podría finalizar la conversación mediante `session.endConversation`, como se muestra en el siguiente ejemplo de código.</span><span class="sxs-lookup"><span data-stu-id="9355e-221">For example, the `orderDinner` dialog in the previous example could end the conversation by using `session.endConversation`, as shown in the following code sample.</span></span>

```javascript
bot.dialog('orderDinner', [
    //...waterfall steps...
    // Last step
    function(session, results){
        if(results.response){
            session.dialogData.room = results.response;
            var msg = `Thank you. Your order will be delivered to room #${session.dialogData.room}`;
            session.endConversation(msg);
        }
    }
]);
```

<span data-ttu-id="9355e-222">Una llamada a `session.endConversation` finalizará la conversación, con lo cual se borrará la pila de diálogo y se restablecerá el almacenamiento de [`session.conversationData`](https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.session#conversationdata).</span><span class="sxs-lookup"><span data-stu-id="9355e-222">Calling `session.endConversation` will end the conversation by clearing the dialog stack and resetting the [`session.conversationData`](https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.session#conversationdata) storage.</span></span> <span data-ttu-id="9355e-223">Para más información sobre el almacenamiento de datos, consulte [Administración de datos de estado](bot-builder-nodejs-state.md).</span><span class="sxs-lookup"><span data-stu-id="9355e-223">For more information on data storage, see [Manage state data](bot-builder-nodejs-state.md).</span></span>

<span data-ttu-id="9355e-224">Llamar a `session.endConversation` es algo lógico cuando el usuario completa el flujo de conversación para el que se ha diseñado el bot.</span><span class="sxs-lookup"><span data-stu-id="9355e-224">Calling `session.endConversation` is a logical thing to do when the user completes the conversation flow for which the bot is designed.</span></span> <span data-ttu-id="9355e-225">También puede usar `session.endConversation` para finalizar la conversación en los casos en los que el usuario escribe "cancelar" o "adiós" en medio de una conversación.</span><span class="sxs-lookup"><span data-stu-id="9355e-225">You may also use `session.endConversation` to end the conversation in situations where the user enters "cancel" or "goodbye" in the midst of a conversation.</span></span> <span data-ttu-id="9355e-226">Para ello, basta con adjuntar una acción `endConversationAction` al diálogo y hacer que este desencadenador escuche coincidencias de entrada de "cancelar" o "adiós".</span><span class="sxs-lookup"><span data-stu-id="9355e-226">To do so, simply attach an `endConversationAction` to the dialog and have this trigger listen for input matching "cancel" or "goodbye".</span></span>

<span data-ttu-id="9355e-227">El ejemplo de código siguiente muestra como adjuntar una acción `endConversationAction` a un diálogo para finalizar la conversación si el usuario escribe "cancelar" o "adiós".</span><span class="sxs-lookup"><span data-stu-id="9355e-227">The following code sample shows how to attach an `endConversationAction` to a dialog to end the conversation if the user enters "cancel" or "goodbye".</span></span>

```javascript
bot.dialog('dinnerOrder', [
    //...waterfall steps...
])
.endConversationAction(
    "endOrderDinner", "Ok. Goodbye.",
    {
        matches: /^cancel$|^goodbye$/i,
        confirmPrompt: "This will cancel your order. Are you sure?"
    }
);
```

<span data-ttu-id="9355e-228">Dado que finalizar una conversación con `session.endConversation` o `endConversationAction` borrará la pila de diálogo y forzará al usuario a empezar de nuevo, debería incluir un aviso `confirmPrompt` para asegurarse de que el usuario realmente desea hacer eso.</span><span class="sxs-lookup"><span data-stu-id="9355e-228">Since ending a conversation with `session.endConversation` or `endConversationAction` will clear the dialog stack and force the user to start over, you should include a `confirmPrompt` to ensure that the user really wants to do so.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9355e-229">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="9355e-229">Next steps</span></span>

<span data-ttu-id="9355e-230">En este artículo, ha explorado las maneras de administrar conversaciones que son secuenciales por naturaleza.</span><span class="sxs-lookup"><span data-stu-id="9355e-230">In this article, you explore ways to manage conversations that are sequential in nature.</span></span> <span data-ttu-id="9355e-231">¿Qué ocurre si desea repetir un diálogo o usar el patrón de bucles en la conversación?</span><span class="sxs-lookup"><span data-stu-id="9355e-231">What if you want to repeat a dialog or use looping pattern in your conversation?</span></span> <span data-ttu-id="9355e-232">Veamos cómo puede hacerlo sustituyendo los diálogos de la pila.</span><span class="sxs-lookup"><span data-stu-id="9355e-232">Let's see how you can do that by replacing dialogs on the stack.</span></span>

> [!div class="nextstepaction"]
> [Sustitución de diálogos](bot-builder-nodejs-dialog-replace.md)

