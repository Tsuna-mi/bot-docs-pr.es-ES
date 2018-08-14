---
title: Definición de los pasos de conversación con cascadas | Microsoft Docs
description: Aprenda a usar cascadas para definir los pasos de una conversación con el SDK de Bot Builder para Node.js.
author: v-ducvo
ms.author: v-ducvo
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: ac8e01a81e3f8acca3aa6976eec47017876c3042
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305296"
---
# <a name="define-conversation-steps-with-waterfalls"></a><span data-ttu-id="1de56-103">Definición de los pasos de la conversación con cascadas</span><span class="sxs-lookup"><span data-stu-id="1de56-103">Define conversation steps with waterfalls</span></span>

<span data-ttu-id="1de56-104">Una conversación es una serie de mensajes que se intercambian un usuario y un bot.</span><span class="sxs-lookup"><span data-stu-id="1de56-104">A conversation is a series of messages exchanged between user and bot.</span></span> <span data-ttu-id="1de56-105">Cuando el objetivo del bot es llevar al usuario por una serie de pasos, puede usar una cascada para definir los pasos de la conversación.</span><span class="sxs-lookup"><span data-stu-id="1de56-105">When the bot's objective is to lead the user through a series of steps, you can use a waterfall to define the steps of the conversation.</span></span>

<span data-ttu-id="1de56-106">Una cascada es una implementación específica de un [diálogo](bot-builder-nodejs-dialog-overview.md) que se usa normalmente para recopilar información del usuario o guiarlo en una serie de tareas.</span><span class="sxs-lookup"><span data-stu-id="1de56-106">A waterfall is a specific implementation of a [dialog](bot-builder-nodejs-dialog-overview.md) that is most commonly used to collect information from the user or guide the user through a series of tasks.</span></span> <span data-ttu-id="1de56-107">Las tareas se implementan como una matriz de funciones donde los resultados de la primera función se pasan como entrada a la función siguiente y así sucesivamente.</span><span class="sxs-lookup"><span data-stu-id="1de56-107">The tasks are implemented as an array of functions where the results of the first function are passed as input into the next function, and so on.</span></span> <span data-ttu-id="1de56-108">Cada función representa normalmente un paso del proceso general.</span><span class="sxs-lookup"><span data-stu-id="1de56-108">Each function typically represents one step in the overall process.</span></span> <span data-ttu-id="1de56-109">En cada paso, el bot pide al usuario que intervenga, espera una respuesta y, a continuación, pasa el resultado al paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="1de56-109">At each step, the bot prompts the user for input, waits for a response, and then passes the result to the next step.</span></span>

<span data-ttu-id="1de56-110">Este artículo le ayudará a entender cómo funciona una cascada y cómo puede usarla para [administrar el flujo de conversación](bot-builder-nodejs-dialog-manage-conversation.md).</span><span class="sxs-lookup"><span data-stu-id="1de56-110">This article will help you understand how a waterfall works and how you can use it to [manage conversation flow](bot-builder-nodejs-dialog-manage-conversation.md).</span></span>

## <a name="conversation-steps"></a><span data-ttu-id="1de56-111">Pasos de la conversación</span><span class="sxs-lookup"><span data-stu-id="1de56-111">Conversation steps</span></span>

<span data-ttu-id="1de56-112">Una conversación suele implica varios intercambios de solicitud/respuesta entre el usuario y el bot.</span><span class="sxs-lookup"><span data-stu-id="1de56-112">A conversation typically involves several prompt/response exchanges between the user and the bot.</span></span> <span data-ttu-id="1de56-113">Cada intercambio de solicitud/respuesta es un paso adelante en la conversación.</span><span class="sxs-lookup"><span data-stu-id="1de56-113">Each prompt/response exchange is a step forward in the conversation.</span></span> <span data-ttu-id="1de56-114">Puede crear una conversación mediante una cascada con tan solo dos pasos.</span><span class="sxs-lookup"><span data-stu-id="1de56-114">You can create a conversation using a waterfall with as few as two steps.</span></span>

<span data-ttu-id="1de56-115">Por ejemplo, considere el siguiente diálogo `greetings`.</span><span class="sxs-lookup"><span data-stu-id="1de56-115">For example, consider the following `greetings` dialog.</span></span> <span data-ttu-id="1de56-116">El primer paso de la cascada pide al usuario el nombre y el segundo paso usa la respuesta para saludar al usuario por su nombre.</span><span class="sxs-lookup"><span data-stu-id="1de56-116">The first step of the waterfall prompts for the user's name and the second step uses the response to greet the user by name.</span></span>

```javascript
bot.dialog('greetings', [
    // Step 1
    function (session) {
        builder.Prompts.text(session, 'Hi! What is your name?');
    },
    // Step 2
    function (session, results) {
        session.endDialog(`Hello ${results.response}!`);
    }
]);
```

<span data-ttu-id="1de56-117">Esto es posible gracias al uso de mensajes.</span><span class="sxs-lookup"><span data-stu-id="1de56-117">What makes this possible is the use of prompts.</span></span> <span data-ttu-id="1de56-118">El SDK de Bot Builder para Node.js proporciona varios tipos diferentes de [mensajes](bot-builder-nodejs-dialog-prompt.md) integrados que puede usar para pedir al usuario diversos tipos de información.</span><span class="sxs-lookup"><span data-stu-id="1de56-118">The Bot Builder SDK for Node.js provides several different types of built-in [prompts](bot-builder-nodejs-dialog-prompt.md) that you can use to ask the user for various types of information.</span></span>

<span data-ttu-id="1de56-119">El ejemplo de código siguiente muestra un diálogo que usa mensajes para recopilar diversos fragmentos de información del usuario mediante una cascada de 4 pasos.</span><span class="sxs-lookup"><span data-stu-id="1de56-119">The following sample code shows a dialog that uses prompts to collect various pieces of information from the user throughout a 4-step waterfall.</span></span>

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
        builder.Prompts.text(session, "How many people are in your party?");
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

<span data-ttu-id="1de56-120">En este ejemplo, el diálogo predeterminado tiene cuatro funciones, cada una de las cuales representa un paso de la cascada.</span><span class="sxs-lookup"><span data-stu-id="1de56-120">In this example, the default dialog has four functions, each one representing a step in the waterfall.</span></span> <span data-ttu-id="1de56-121">Cada paso pide al usuario que intervenga y envía los resultados al paso siguiente para su procesamiento.</span><span class="sxs-lookup"><span data-stu-id="1de56-121">Each step prompts the user for input and sends the results to the next step to be processed.</span></span> <span data-ttu-id="1de56-122">Este proceso continúa hasta que se ejecuta el último paso, que confirma la reserva y finaliza el diálogo.</span><span class="sxs-lookup"><span data-stu-id="1de56-122">This process continues until the last step executes, thereby confirming the reservation and ending the dialog.</span></span>

<span data-ttu-id="1de56-123">En la siguiente captura de pantalla se muestran los resultados de este bot que se ejecuta en [Bot Framework Emulator](~/bot-service-debug-emulator.md).</span><span class="sxs-lookup"><span data-stu-id="1de56-123">The following screenshot shows the results of this bot running in the [Bot Framework Emulator](~/bot-service-debug-emulator.md).</span></span>

![Administración del flujo de conversación mediante una cascada](~/media/bot-builder-nodejs-dialog-manage-conversation/waterfall-results.png)

## <a name="create-a-conversation-with-multiple-waterfalls"></a><span data-ttu-id="1de56-125">Creación de una conversación con varias cascadas</span><span class="sxs-lookup"><span data-stu-id="1de56-125">Create a conversation with multiple waterfalls</span></span>

<span data-ttu-id="1de56-126">Puede usar varias cascadas dentro de una conversación para definir cualquier estructura de conversación que requiera el bot.</span><span class="sxs-lookup"><span data-stu-id="1de56-126">You can use multiple waterfalls within a conversation to define whatever conversation structure your bot requires.</span></span> <span data-ttu-id="1de56-127">Por ejemplo, podría usar una cascada para administrar el flujo de conversación y usar cascadas adicionales para recopilar información del usuario.</span><span class="sxs-lookup"><span data-stu-id="1de56-127">For example, you might use one waterfall to manage the conversation flow and use additional waterfalls to collect information from the user.</span></span> <span data-ttu-id="1de56-128">Cada cascada se encapsula en un diálogo y se puede invocar mediante una llamada a la función `session.beginDialog`.</span><span class="sxs-lookup"><span data-stu-id="1de56-128">Each waterfall is encapsulated in a dialog and can be invoked by calling the `session.beginDialog` function.</span></span>

<span data-ttu-id="1de56-129">En el ejemplo de código siguiente se muestra cómo usar varias cascadas en una conversación.</span><span class="sxs-lookup"><span data-stu-id="1de56-129">The following code sample shows how to use multiple waterfalls in a conversation.</span></span> <span data-ttu-id="1de56-130">La cascada dentro del diálogo `greetings` administra el flujo de la conversación, mientras que la cascada dentro del diálogo `askName` recopila información del usuario.</span><span class="sxs-lookup"><span data-stu-id="1de56-130">The waterfall within the `greetings` dialog manages the flow of the conversation, while the waterfall within the `askName` dialog collects information from the user.</span></span>

```javascript
bot.dialog('greetings', [
    function (session) {
        session.beginDialog('askName');
    },
    function (session, results) {
        session.endDialog('Hello %s!', results.response);
    }
]);
bot.dialog('askName', [
    function (session) {
        builder.Prompts.text(session, 'Hi! What is your name?');
    },
    function (session, results) {
        session.endDialogWithResult(results);
    }
]);
```

## <a name="advance-the-waterfall"></a><span data-ttu-id="1de56-131">Avance en la cascada</span><span class="sxs-lookup"><span data-stu-id="1de56-131">Advance the waterfall</span></span>

<span data-ttu-id="1de56-132">Una cascada avanza paso a paso en la secuencia con que las funciones están definidas en la matriz.</span><span class="sxs-lookup"><span data-stu-id="1de56-132">A waterfall progresses from step to step in the sequence that the functions are defined in the array.</span></span> <span data-ttu-id="1de56-133">La primera función dentro de una cascada puede recibir los argumentos que se pasan al diálogo.</span><span class="sxs-lookup"><span data-stu-id="1de56-133">The first function within a waterfall can receive the arguments that are passed to the dialog.</span></span> <span data-ttu-id="1de56-134">Cualquier función dentro de una cascada puede usar la función `next` para avanzar al paso siguiente sin pedirle al usuario que intervenga.</span><span class="sxs-lookup"><span data-stu-id="1de56-134">Any function within a waterfall can use the `next` function to proceed to the next step without prompting user for input.</span></span>

<span data-ttu-id="1de56-135">En el ejemplo de código siguiente se muestra cómo usar la función `next` dentro de un diálogo que lleva al usuario por el proceso de proporcionar información para su perfil de usuario.</span><span class="sxs-lookup"><span data-stu-id="1de56-135">The following code sample shows how to use the `next` function within a dialog that walks the user through the process of providing information for their user profile.</span></span> <span data-ttu-id="1de56-136">En cada paso de la cascada, el bot pide al usuario un fragmento de información (si es necesario) y el siguiente paso de la cascada procesa la respuesta del usuario (si la hay).</span><span class="sxs-lookup"><span data-stu-id="1de56-136">In each step of the waterfall, the bot prompts the user for a piece of information (if necessary), and the user's response (if any) is processed by the subsequent step of the waterfall.</span></span> <span data-ttu-id="1de56-137">El paso final de la cascada en el diálogo `ensureProfile` finaliza ese diálogo y devuelve la información de perfil completada al diálogo de llamada, que envía un saludo personalizado al usuario.</span><span class="sxs-lookup"><span data-stu-id="1de56-137">The final step of the waterfall in the `ensureProfile` dialog ends that dialog and returns the completed profile information to the calling dialog, which then sends a personalized greeting to the user.</span></span>

```javascript
var inMemoryStorage = new builder.MemoryBotStorage();

// This bot ensures user's profile is up to date.
var bot = new builder.UniversalBot(connector, [
    function (session) {
        session.beginDialog('ensureProfile', session.userData.profile);
    },
    function (session, results) {
        session.userData.profile = results.response; // Save user profile.
        session.send(`Hello ${session.userData.profile.name}! I love ${session.userData.profile.company}!`);
    }
]).set('storage', inMemoryStorage); // Register in-memory storage 

bot.dialog('ensureProfile', [
    function (session, args, next) {
        session.dialogData.profile = args || {}; // Set the profile or create the object.
        if (!session.dialogData.profile.name) {
            builder.Prompts.text(session, "What's your name?");
        } else {
            next(); // Skip if we already have this info.
        }
    },
    function (session, results, next) {
        if (results.response) {
            // Save user's name if we asked for it.
            session.dialogData.profile.name = results.response;
        }
        if (!session.dialogData.profile.company) {
            builder.Prompts.text(session, "What company do you work for?");
        } else {
            next(); // Skip if we already have this info.
        }
    },
    function (session, results) {
        if (results.response) {
            // Save company name if we asked for it.
            session.dialogData.profile.company = results.response;
        }
        session.endDialogWithResult({ response: session.dialogData.profile });
    }
]);
```

> [!NOTE]
> <span data-ttu-id="1de56-138">En este ejemplo se usan dos contenedores de datos diferentes para almacenar los datos: `dialogData` y `userData`.</span><span class="sxs-lookup"><span data-stu-id="1de56-138">This example uses two different data bags to store data: `dialogData` and `userData`.</span></span> <span data-ttu-id="1de56-139">Si el bot se distribuye entre varios nodos de proceso, cada paso de la cascada podría procesarse con un nodo diferente.</span><span class="sxs-lookup"><span data-stu-id="1de56-139">If the bot is distributed across multiple compute nodes, each step of the waterfall could be processed by a different node.</span></span> <span data-ttu-id="1de56-140">Para más información sobre cómo almacenar datos de bot, consulte [Administración de datos de estado](bot-builder-nodejs-state.md).</span><span class="sxs-lookup"><span data-stu-id="1de56-140">For more information about storing bot data, see [Manage state data](bot-builder-nodejs-state.md).</span></span>

## <a name="end-a-waterfall"></a><span data-ttu-id="1de56-141">Finalización de una cascada</span><span class="sxs-lookup"><span data-stu-id="1de56-141">End a waterfall</span></span>

<span data-ttu-id="1de56-142">Un diálogo que se crea con una cascada se debe terminar explícitamente, de lo contrario, el bot repetirá la cascada indefinidamente.</span><span class="sxs-lookup"><span data-stu-id="1de56-142">A dialog that is created using a waterfall must be explicitly ended, otherwise the bot will repeat the waterfall indefinitely.</span></span> <span data-ttu-id="1de56-143">Puede finalizar una cascada mediante uno de los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="1de56-143">You can end a waterfall by using one of the following methods:</span></span>

* <span data-ttu-id="1de56-144">`session.endDialog`: use este método para finalizar la cascada si no hay ningún dato para devolver al diálogo de llamada.</span><span class="sxs-lookup"><span data-stu-id="1de56-144">`session.endDialog`: Use this method to end the waterfall if there is no data to return to the calling dialog.</span></span>

* <span data-ttu-id="1de56-145">`session.endDialogWithResult`: use este método para finalizar la cascada si hay datos para devolver al diálogo de llamada.</span><span class="sxs-lookup"><span data-stu-id="1de56-145">`session.endDialogWithResult`: Use this method to end the waterfall if there is data to return to the calling dialog.</span></span> <span data-ttu-id="1de56-146">El argumento `response` que se devuelve puede ser un objeto JSON o cualquier tipo de datos primitivo de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="1de56-146">The `response` argument that is returned can be a JSON object or any JavaScript primitive data type.</span></span> <span data-ttu-id="1de56-147">Por ejemplo: </span><span class="sxs-lookup"><span data-stu-id="1de56-147">For example:</span></span>
  ```javascript
  session.endDialogWithResult({
    response: { name: session.dialogData.name, company: session.dialogData.company }
  });
  ```

* <span data-ttu-id="1de56-148">`session.endConversation`: use este método para finalizar la cascada si el final de esta representa el final de la conversación.</span><span class="sxs-lookup"><span data-stu-id="1de56-148">`session.endConversation`: Use this method to end the waterfall if the end of the waterfall represents the end of the conversation.</span></span>

<span data-ttu-id="1de56-149">Como alternativa al uso de uno de estos tres métodos para finalizar una cascada, puede asociar el desencadenador `endConversationAction` al diálogo.</span><span class="sxs-lookup"><span data-stu-id="1de56-149">As an alternative to using one of these three methods to end a waterfall, you can attach the `endConversationAction` trigger to the dialog.</span></span> <span data-ttu-id="1de56-150">Por ejemplo: </span><span class="sxs-lookup"><span data-stu-id="1de56-150">For example:</span></span>

```javascript
bot.dialog('dinnerOrder', [
    //...waterfall steps...,
    // Last step
    function(session, results){
        if(results.response){
            session.dialogData.room = results.response;
            var msg = `Thank you. Your order will be delivered to room #${session.dialogData.room}`;
            session.endConversation(msg);
        }
    }
])
.endConversationAction(
    "endOrderDinner", "Ok. Goodbye.",
    {
        matches: /^cancel$|^goodbye$/i,
        confirmPrompt: "This will cancel your order. Are you sure?"
    }
);
```

## <a name="next-steps"></a><span data-ttu-id="1de56-151">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="1de56-151">Next steps</span></span>

<span data-ttu-id="1de56-152">Con una cascada puede recopilar información del usuario con *mensajes*.</span><span class="sxs-lookup"><span data-stu-id="1de56-152">Using waterfall, you can collect information from the user with *prompts*.</span></span> <span data-ttu-id="1de56-153">Vamos a profundizar en cómo puede solicitar la intervención del usuario.</span><span class="sxs-lookup"><span data-stu-id="1de56-153">Let's dive into how you can prompt user for input.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="1de56-154">Solicitud de entrada de usuario</span><span class="sxs-lookup"><span data-stu-id="1de56-154">Prompt user for input</span></span>](bot-builder-nodejs-dialog-prompt.md)
