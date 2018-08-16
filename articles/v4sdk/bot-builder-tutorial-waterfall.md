---
title: Formulación de preguntas al usuario | Microsoft Docs
description: Aprenda a usar el modelo en cascada para preguntar al usuario varias entradas en Bot Builder SDK.
keywords: cascadas, diálogos, formular una pregunta, avisos
author: v-ducvo
ms.author: v-ducvo
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 5/10/2018
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: a5c55d4437033968f9c08ed49c07b9586cb9b7d8
ms.sourcegitcommit: 9a38d76afb0e82fdccc1f36f9b1a65042671e538
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/04/2018
ms.locfileid: "39514975"
---
# <a name="ask-the-user-questions"></a><span data-ttu-id="1e9a3-104">Formulación de preguntas al usuario</span><span class="sxs-lookup"><span data-stu-id="1e9a3-104">Ask the user questions</span></span>

[!INCLUDE [pre-release-label](~/includes/pre-release-label.md)]

<span data-ttu-id="1e9a3-105">En esencia, un bot se basa en la conversación con un usuario.</span><span class="sxs-lookup"><span data-stu-id="1e9a3-105">At its core, a bot is built around the conversation with a user.</span></span> <span data-ttu-id="1e9a3-106">La conversación puede adoptar [muchas formas](bot-builder-conversations.md); puede que sea corta o puede ser más compleja, puede ser para formular preguntas o para responder a estas.</span><span class="sxs-lookup"><span data-stu-id="1e9a3-106">Conversation can take [many forms](bot-builder-conversations.md); they may be short or may be more complex, may be asking questions or may be answering questions.</span></span> <span data-ttu-id="1e9a3-107">La forma que adopte la conversación depende de varios factores, pero todos ellos implican una conversación.</span><span class="sxs-lookup"><span data-stu-id="1e9a3-107">What shape the conversation takes depends on several factors, but they all involve a conversation.</span></span>

<span data-ttu-id="1e9a3-108">Este tutorial le guía por el proceso de creación de una conversación, desde formular una pregunta sencilla hasta la creación de un bot con varios turnos.</span><span class="sxs-lookup"><span data-stu-id="1e9a3-108">This tutorial guides you through building up a conversation, from asking a simple question through a multi-turn bot.</span></span> <span data-ttu-id="1e9a3-109">Nuestro ejemplo trata de la reserva de una mesa para cenar, pero puede imaginarse un bot que haga varias cosas mediante una conversación con varios turnos como, por ejemplo, realizar un pedido, responder preguntas frecuentes, realizar reservas, etc.</span><span class="sxs-lookup"><span data-stu-id="1e9a3-109">Our example will be around reserving a table, but you can imagine a bot that does a variety of things through a multi-turn conversation, such as placing an order, answering FAQs, making reservations, and so on.</span></span>

<span data-ttu-id="1e9a3-110">Un bot de chat interactivo puede responder a una entrada del usuario o preguntarle a este una entrada específica.</span><span class="sxs-lookup"><span data-stu-id="1e9a3-110">An interactive chat bot can respond to user input or ask user for specific input.</span></span> <span data-ttu-id="1e9a3-111">Este tutorial le mostrará cómo formular al usuario una pregunta mediante la biblioteca `Prompts` que forma parte de `Dialogs`.</span><span class="sxs-lookup"><span data-stu-id="1e9a3-111">This tutorial will show you how to ask a user a question using the `Prompts` library, which is part of `Dialogs`.</span></span> <span data-ttu-id="1e9a3-112">Se puede considerar a los [diálogos](../bot-service-design-conversation-flow.md) como el contenedor que define la estructura de una conversación. Los avisos que van incluidos en los diálogos se tratan con mayor detalle en su propio [artículo de procedimientos](bot-builder-prompts.md).</span><span class="sxs-lookup"><span data-stu-id="1e9a3-112">[Dialogs](../bot-service-design-conversation-flow.md) can be thought of as the container that defines a conversation structure, and prompts within dialogs is covered more in depth in its own [how-to article](bot-builder-prompts.md).</span></span>

## <a name="prerequisite"></a><span data-ttu-id="1e9a3-113">Requisito previo</span><span class="sxs-lookup"><span data-stu-id="1e9a3-113">Prerequisite</span></span>

<span data-ttu-id="1e9a3-114">El código de este tutorial se compilará sobre el **bot básico** que creó en la experiencia de [introducción](~/bot-service-quickstart.md).</span><span class="sxs-lookup"><span data-stu-id="1e9a3-114">Code in this tutorial will build on the **basic bot** you created through the [Get Started](~/bot-service-quickstart.md) experience.</span></span>

## <a name="get-the-package"></a><span data-ttu-id="1e9a3-115">Obtención del paquete</span><span class="sxs-lookup"><span data-stu-id="1e9a3-115">Get the package</span></span>

# <a name="ctabcstab"></a>[<span data-ttu-id="1e9a3-116">C#</span><span class="sxs-lookup"><span data-stu-id="1e9a3-116">C#</span></span>](#tab/cstab)

<span data-ttu-id="1e9a3-117">Instale el paquete **Microsoft.Bot.Builder.Dialogs** del administrador de paquetes NuGet.</span><span class="sxs-lookup"><span data-stu-id="1e9a3-117">Install the **Microsoft.Bot.Builder.Dialogs** package from Nuget packet manager.</span></span>

# <a name="javascripttabjstab"></a>[<span data-ttu-id="1e9a3-118">JavaScript</span><span class="sxs-lookup"><span data-stu-id="1e9a3-118">JavaScript</span></span>](#tab/jstab)
<span data-ttu-id="1e9a3-119">Vaya a la carpeta de proyecto del bot e instale el paquete `botbuilder-dialogs` de NPM:</span><span class="sxs-lookup"><span data-stu-id="1e9a3-119">Navigate to your bot's project folder and install the `botbuilder-dialogs` package from NPM:</span></span>

```cmd
npm install --save botbuilder-dialogs@preview
```

---

## <a name="import-package-to-bot"></a><span data-ttu-id="1e9a3-120">Importación del paquete al bot</span><span class="sxs-lookup"><span data-stu-id="1e9a3-120">Import package to bot</span></span>

# <a name="ctabcstab"></a>[<span data-ttu-id="1e9a3-121">C#</span><span class="sxs-lookup"><span data-stu-id="1e9a3-121">C#</span></span>](#tab/cstab)

<span data-ttu-id="1e9a3-122">Agregue referencias a diálogos y avisos en el código del bot.</span><span class="sxs-lookup"><span data-stu-id="1e9a3-122">Add reference to both dialogs and prompts in your bot code.</span></span>

```cs
// ...
using Microsoft.Bot.Builder.Dialogs;
using Microsoft.Bot.Builder.Prompts;
// ...
```


# <a name="javascripttabjstab"></a>[<span data-ttu-id="1e9a3-123">JavaScript</span><span class="sxs-lookup"><span data-stu-id="1e9a3-123">JavaScript</span></span>](#tab/jstab)

<span data-ttu-id="1e9a3-124">Abra el archivo **app.js** e incluya la biblioteca `botbuilder-dialogs` en el código de bot.</span><span class="sxs-lookup"><span data-stu-id="1e9a3-124">Open the **app.js** file and include the `botbuilder-dialogs` library in the bot code.</span></span>

```javascript
const botbuilder_dialogs = require('botbuilder-dialogs');
```

---

<span data-ttu-id="1e9a3-125">Esto le dará acceso a la biblioteca `DialogSet` y `Prompts` que va a utilizar para formular las preguntas al usuario.</span><span class="sxs-lookup"><span data-stu-id="1e9a3-125">This will give you access to the `DialogSet` and `Prompts` library that you will use to ask the user questions.</span></span> <span data-ttu-id="1e9a3-126">`DialogSet` es solo una colección de diálogos que se estructura en un patrón de **cascada**.</span><span class="sxs-lookup"><span data-stu-id="1e9a3-126">`DialogSet` is just a collection of dialogs, which we structure in a **waterfall** pattern.</span></span> <span data-ttu-id="1e9a3-127">Esto significa simplemente que un diálogo sigue a otro.</span><span class="sxs-lookup"><span data-stu-id="1e9a3-127">This simply means that one dialog follows another.</span></span>

## <a name="instantiate-a-dialogs-object"></a><span data-ttu-id="1e9a3-128">Creación de una instancia de un objeto de diálogos</span><span class="sxs-lookup"><span data-stu-id="1e9a3-128">Instantiate a dialogs object</span></span>

<span data-ttu-id="1e9a3-129">Cree una instancia de un objeto `dialogs`.</span><span class="sxs-lookup"><span data-stu-id="1e9a3-129">Instantiate a `dialogs` object.</span></span> <span data-ttu-id="1e9a3-130">Usará este objeto de diálogo para administrar el proceso de pregunta y respuesta.</span><span class="sxs-lookup"><span data-stu-id="1e9a3-130">You will use this dialog object to manage the question and answer process.</span></span>

# <a name="ctabcstab"></a>[<span data-ttu-id="1e9a3-131">C#</span><span class="sxs-lookup"><span data-stu-id="1e9a3-131">C#</span></span>](#tab/cstab)
<span data-ttu-id="1e9a3-132">Declare una variable de miembro en la clase de bot e inicialícela en el constructor del bot.</span><span class="sxs-lookup"><span data-stu-id="1e9a3-132">Declare a member variable in your bot class and initialize it in the constructor for your bot.</span></span> 
```cs
public class MyBot : IBot
{
    private readonly DialogSet dialogs;
    public MyBot()
    {
        dialogs = new DialogSet();
    }
    // The rest of the class definition is omitted here
}
```

# <a name="javascripttabjstab"></a>[<span data-ttu-id="1e9a3-133">JavaScript</span><span class="sxs-lookup"><span data-stu-id="1e9a3-133">JavaScript</span></span>](#tab/jstab)

```javascript
const dialogs = new botbuilder_dialogs.DialogSet();
```
---

## <a name="define-a-waterfall-dialog"></a><span data-ttu-id="1e9a3-134">Definición de un diálogo en cascada</span><span class="sxs-lookup"><span data-stu-id="1e9a3-134">Define a waterfall dialog</span></span>

<span data-ttu-id="1e9a3-135">Para formular una pregunta, necesitará al menos un diálogo en **cascada** con dos pasos.</span><span class="sxs-lookup"><span data-stu-id="1e9a3-135">To ask a question, you will need at least a two step **waterfall** dialog.</span></span> <span data-ttu-id="1e9a3-136">En este ejemplo, va a construir un diálogo en **cascada** de dos pasos en el que el primer paso pide al usuario su nombre y el segundo paso saluda al usuario por su nombre.</span><span class="sxs-lookup"><span data-stu-id="1e9a3-136">For this example, you will construct a two step **waterfall** dialog where the first step asks the user for their name and the second step greets the user by name.</span></span> 

# <a name="ctabcstab"></a>[<span data-ttu-id="1e9a3-137">C#</span><span class="sxs-lookup"><span data-stu-id="1e9a3-137">C#</span></span>](#tab/cstab)

<span data-ttu-id="1e9a3-138">Modifique el constructor del bot para agregar el diálogo:</span><span class="sxs-lookup"><span data-stu-id="1e9a3-138">Modify your bot's constructor to add the dialog:</span></span>
```csharp
public MyBot()
{
    dialogs = new DialogSet();
    dialogs.Add("greetings", new WaterfallStep[]
    {
        async (dc, args, next) =>
        {
            // Prompt for the guest's name.
            await dc.Prompt("textPrompt","What is your name?");
        },
        async(dc, args, next) =>
        {
            await dc.Context.SendActivity($"Hi {args["Text"]}!");
            await dc.End();
        }
    });
}
```

# <a name="javascripttabjstab"></a>[<span data-ttu-id="1e9a3-139">JavaScript</span><span class="sxs-lookup"><span data-stu-id="1e9a3-139">JavaScript</span></span>](#tab/jstab)

```javascript
// Greet user:
// Ask for the user name and then greet them by name.
dialogs.add('greetings',[
    async function (dc){
        await dc.prompt('textPrompt', 'What is your name?');
    },
    async function(dc, results){
        var userName = results;
        await dc.context.sendActivity(`Hello ${userName}!`);
        await dc.end(); // Ends the dialog
    }
]);
```

---

<span data-ttu-id="1e9a3-140">La pregunta se formula mediante un método `textPrompt` que venía con la biblioteca `Prompts`.</span><span class="sxs-lookup"><span data-stu-id="1e9a3-140">The question is asked using a `textPrompt` method that came with the `Prompts` library.</span></span> <span data-ttu-id="1e9a3-141">La biblioteca `Prompts` ofrece un conjunto de avisos que permiten pedir a los usuarios diversos tipos de información.</span><span class="sxs-lookup"><span data-stu-id="1e9a3-141">The `Prompts` library offers a set of prompts that allows you to ask users for various types of information.</span></span> <span data-ttu-id="1e9a3-142">Para más información sobre otros tipos de avisos, consulte [Petición de datos de entrada al usuario](~/v4sdk/bot-builder-prompts.md).</span><span class="sxs-lookup"><span data-stu-id="1e9a3-142">For more information about other prompt types, see [Prompt user for input](~/v4sdk/bot-builder-prompts.md).</span></span>

<span data-ttu-id="1e9a3-143">Para que los avisos funcionen, deberá agregar un aviso al objeto `dialogs` con el identificador de diálogo `textPrompt` y crearlo con el constructor `TextPrompt()`.</span><span class="sxs-lookup"><span data-stu-id="1e9a3-143">For the prompting to work, you will need to add a prompt to the `dialogs` object with the dialogId `textPrompt` and create it with the `TextPrompt()` constructor.</span></span>

# <a name="ctabcstab"></a>[<span data-ttu-id="1e9a3-144">C#</span><span class="sxs-lookup"><span data-stu-id="1e9a3-144">C#</span></span>](#tab/cstab)

```cs
public MyBot()
{
    dialogs = new DialogSet();
    dialogs.Add("greetings", new WaterfallStep[]
    {
        async (dc, args, next) =>
        {
            // Prompt for the guest's name.
            await dc.Prompt("textPrompt","What is your name?");
        },
        async(dc, args, next) =>
        {
            await dc.Context.SendActivity($"Hi {args["Text"]}!");
            await dc.End();
        }
    });
    // add the prompt, of type TextPrompt
    dialogs.Add("textPrompt", new Builder.Dialogs.TextPrompt());
}

```
<span data-ttu-id="1e9a3-145">Una vez que el usuario responda la pregunta, la respuesta se puede encontrar en el parámetro `args` del paso 2.</span><span class="sxs-lookup"><span data-stu-id="1e9a3-145">Once the user answers the question, the response can be found in the `args` parameter of step 2.</span></span>

# <a name="javascripttabjstab"></a>[<span data-ttu-id="1e9a3-146">JavaScript</span><span class="sxs-lookup"><span data-stu-id="1e9a3-146">JavaScript</span></span>](#tab/jstab)

```javascript
dialogs.add('textPrompt', new botbuilder_dialogs.TextPrompt());
```
<span data-ttu-id="1e9a3-147">Una vez que el usuario responda la pregunta, la respuesta se puede encontrar en el parámetro `results` del paso 2.</span><span class="sxs-lookup"><span data-stu-id="1e9a3-147">Once the user answers the question, the response can be found in the `results` parameter of step 2.</span></span> <span data-ttu-id="1e9a3-148">En este caso, `results` se asigna a una variable local `userName`.</span><span class="sxs-lookup"><span data-stu-id="1e9a3-148">In this case, the `results` is assigned to a local variable `userName`.</span></span> <span data-ttu-id="1e9a3-149">En otro tutorial posterior le mostraremos cómo conservar las entradas de usuario en un almacenamiento de su elección.</span><span class="sxs-lookup"><span data-stu-id="1e9a3-149">In a later tutorial, we'll show you how to persist user inputs to a storage of your choice.</span></span>

---


<span data-ttu-id="1e9a3-150">Ahora que ha definido `dialogs` para formular una pregunta, debe llamar al diálogo para iniciar el proceso de aviso.</span><span class="sxs-lookup"><span data-stu-id="1e9a3-150">Now that you have defined your `dialogs` to ask a question, you need to call on the dialog to start the prompting process.</span></span>

## <a name="start-the-dialog"></a><span data-ttu-id="1e9a3-151">Inicio del diálogo</span><span class="sxs-lookup"><span data-stu-id="1e9a3-151">Start the dialog</span></span>

# <a name="ctabcstab"></a>[<span data-ttu-id="1e9a3-152">C#</span><span class="sxs-lookup"><span data-stu-id="1e9a3-152">C#</span></span>](#tab/cstab)

<span data-ttu-id="1e9a3-153">Modifique la lógica del bot a algo parecido a esto:</span><span class="sxs-lookup"><span data-stu-id="1e9a3-153">Modify your bot logic to something like this:</span></span>

```cs

public async Task OnTurn(ITurnContext context)
{
    // We'll cover state later, in the next tutorial
    var state = ConversationState<Dictionary<string, object>>.Get(context);
    var dc = dialogs.CreateContext(context, state);
    if (context.Activity.Type == ActivityTypes.Message)
    {
        await dc.Continue();
        
        if(!context.Responded)
        {
            await dc.Begin("greetings");
        }
    }
}
```

<span data-ttu-id="1e9a3-154">La lógica del bot está en el método `OnTurn()`.</span><span class="sxs-lookup"><span data-stu-id="1e9a3-154">Bot logic goes in the `OnTurn()` method.</span></span> <span data-ttu-id="1e9a3-155">Una vez que el usuario dice "Hola", el bot iniciará el diálogo `greetings`.</span><span class="sxs-lookup"><span data-stu-id="1e9a3-155">Once the user says "Hi" then the bot will start the `greetings` dialog.</span></span> <span data-ttu-id="1e9a3-156">El primer paso del diálogo `greetings` pide al usuario su nombre.</span><span class="sxs-lookup"><span data-stu-id="1e9a3-156">The first step of the `greetings` dialog prompts the user for their name.</span></span> <span data-ttu-id="1e9a3-157">El usuario enviará una respuesta con su nombre como una actividad de mensaje y el resultado se enviará al paso dos de la cascada mediante el método `dc.Continue()`.</span><span class="sxs-lookup"><span data-stu-id="1e9a3-157">The user will send a reply with their name as a message activity, and the result is send to step two of the waterfall through the `dc.Continue()` method.</span></span> <span data-ttu-id="1e9a3-158">El segundo paso de la cascada, tal y como lo ha definido, saludará a los usuarios por su nombre y finalizará el diálogo.</span><span class="sxs-lookup"><span data-stu-id="1e9a3-158">The second step of the waterfall, as you have defined it, will greet the user by their name and ends the dialog.</span></span> 

# <a name="javascripttabjstab"></a>[<span data-ttu-id="1e9a3-159">JavaScript</span><span class="sxs-lookup"><span data-stu-id="1e9a3-159">JavaScript</span></span>](#tab/jstab)

<span data-ttu-id="1e9a3-160">Modifique el método `processActivity()` del bot **básico** para que tenga este aspecto:</span><span class="sxs-lookup"><span data-stu-id="1e9a3-160">Modify your **Basic** bot's `processActivity()` method to look like this:</span></span>

```javascript
// Listen for incoming activity 
server.post('/api/messages', (req, res) => {
    adapter.processActivity(req, res, async (context) => {
        const isMessage = (context.activity.type === 'message');
        // State will store all of your information 
        const convo = conversationState.get(context);
        const dc = dialogs.createContext(context, convo);

        if (isMessage) {
            // Check for valid intents
            if(context.activity.text.match(/hi/ig)){
                await dc.begin('greetings');
            }
        }

        if(!context.responded){
            // Continue executing the "current" dialog, if any.
            await dc.continue();

            if(!context.responded && isMessage){
                // Default message
                await context.sendActivity("Hi! I'm a simple bot. Please say 'Hi'.");
            }
        }
    });
});
```

<span data-ttu-id="1e9a3-161">La lógica del bot está en el método `processActivity()`.</span><span class="sxs-lookup"><span data-stu-id="1e9a3-161">Bot logic goes within the `processActivity()` method.</span></span> <span data-ttu-id="1e9a3-162">Una vez que el usuario dice "Hola", el bot iniciará el diálogo `greetings`.</span><span class="sxs-lookup"><span data-stu-id="1e9a3-162">Once the user says "Hi" then the bot will start the `greetings` dialog.</span></span> <span data-ttu-id="1e9a3-163">El primer paso del diálogo `greetings` pide al usuario su nombre.</span><span class="sxs-lookup"><span data-stu-id="1e9a3-163">The first step of the `greetings` dialog prompts the user for their name.</span></span> <span data-ttu-id="1e9a3-164">El usuario le enviará una respuesta con su nombre como mensaje `text` de la actividad.</span><span class="sxs-lookup"><span data-stu-id="1e9a3-164">The user will send a reply with their name as the activity's `text` message.</span></span> <span data-ttu-id="1e9a3-165">Como el mensaje no coincidía con las intenciones esperadas y el bot no ha enviado ninguna respuesta al usuario, el resultado se envía al paso dos de la cascada mediante el método `dc.continue()`.</span><span class="sxs-lookup"><span data-stu-id="1e9a3-165">Since the message didn't match any expected intents and the bot has not sent any reply to the user, the result is sent to step two of the waterfall through the `dc.continue()` method.</span></span> <span data-ttu-id="1e9a3-166">El segundo paso de la cascada, tal y como lo ha definido, saludará a los usuarios por su nombre y finalizará el diálogo.</span><span class="sxs-lookup"><span data-stu-id="1e9a3-166">The second step of the waterfall, as you have defined it, will greet the user by their name and ends the dialog.</span></span> <span data-ttu-id="1e9a3-167">Si, por ejemplo, el paso dos no envió al usuario el mensaje de saludo, el método `processActivity` finalizará con el envío del *mensaje predeterminado* al usuario.</span><span class="sxs-lookup"><span data-stu-id="1e9a3-167">If, for example, step two did not send the user the greeting message, then `processActivity` method will end with the *default message* sent to the user.</span></span>

---



## <a name="define-a-more-complex-waterfall-dialog"></a><span data-ttu-id="1e9a3-168">Definición de un cuadro de diálogo en cascada más complejo</span><span class="sxs-lookup"><span data-stu-id="1e9a3-168">Define a more complex waterfall dialog</span></span>

<span data-ttu-id="1e9a3-169">Ahora que hemos analizado cómo funciona un diálogo en cascada y cómo crear uno, vamos a probar un diálogo más complejo para reservar una mesa.</span><span class="sxs-lookup"><span data-stu-id="1e9a3-169">Now that we've covered how a waterfall dialog works and how to build one, let's try a more complex dialog aimed at reserving a table.</span></span>

<span data-ttu-id="1e9a3-170">Para administrar la solicitud de reserva de mesa, deberá definir un diálogo en **cascada** con cuatro pasos.</span><span class="sxs-lookup"><span data-stu-id="1e9a3-170">To manage the table reservation request, you will need to define a **waterfall** dialog with four steps.</span></span> <span data-ttu-id="1e9a3-171">En esta conversación, también se usarán `DatetimePrompt` y `NumberPrompt` además de `TextPrompt`.</span><span class="sxs-lookup"><span data-stu-id="1e9a3-171">In this conversation, you will also be using a `DatetimePrompt` and `NumberPrompt` in additional to the `TextPrompt`.</span></span>



# <a name="ctabcstab"></a>[<span data-ttu-id="1e9a3-172">C#</span><span class="sxs-lookup"><span data-stu-id="1e9a3-172">C#</span></span>](#tab/cstab)

<span data-ttu-id="1e9a3-173">Empiece con la plantilla Echo Bot y cambie el nombre del bot a CafeBot.</span><span class="sxs-lookup"><span data-stu-id="1e9a3-173">Start with the Echo Bot template, and rename your bot to CafeBot.</span></span> <span data-ttu-id="1e9a3-174">Agregue un `DialogSet` y algunas variables estáticas de miembros.</span><span class="sxs-lookup"><span data-stu-id="1e9a3-174">Add a `DialogSet` and some static member variables.</span></span>

```cs

namespace CafeBot
{
    public class CafeBot : IBot
    {
        private readonly DialogSet dialogs;

        // Usually, we would save the dialog answers to our state object, which will be covered in a later tutorial.
        // For purpose of this example, let's use the three static variables to store our reservation information.
        static DateTime reservationDate;
        static int partySize;
        static string reservationName;

        // the rest of the class definition is omitted here
        // but is discussed in the rest of this article
    }
}
```

<span data-ttu-id="1e9a3-175">Defina el diálogo `reserveTable`.</span><span class="sxs-lookup"><span data-stu-id="1e9a3-175">Then define your `reserveTable` dialog.</span></span> <span data-ttu-id="1e9a3-176">Puede agregar el diálogo dentro del constructor de clases del bot.</span><span class="sxs-lookup"><span data-stu-id="1e9a3-176">You can add the dialog within the bot class constructor.</span></span>
```cs
public CafeBot()
{
    dialogs = new DialogSet();

    // Define our dialog
    dialogs.Add("reserveTable", new WaterfallStep[]
    {
        async (dc, args, next) =>
        {
            // Prompt for the guest's name.
            await dc.Context.SendActivity("Welcome to the reservation service.");

            await dc.Prompt("dateTimePrompt", "Please provide a reservation date and time.");
        },
        async(dc, args, next) =>
        {
            var dateTimeResult = ((DateTimeResult)args).Resolution.First();

            reservationDate = Convert.ToDateTime(dateTimeResult.Value);
            
            // Ask for next info
            await dc.Prompt("partySizePrompt", "How many people are in your party?");

        },
        async(dc, args, next) =>
        {
            partySize = (int)args["Value"];

            // Ask for next info
            await dc.Prompt("textPrompt", "Whose name will this be under?");
        },
        async(dc, args, next) =>
        {
            reservationName = args["Text"];
            string msg = "Reservation confirmed. Reservation details - " +
            $"\nDate/Time: {reservationDate.ToString()} " +
            $"\nParty size: {partySize.ToString()} " +
            $"\nReservation name: {reservationName}";
            await dc.Context.SendActivity(msg);
            await dc.End();
        }
    });

     // Add a prompt for the reservation date
     dialogs.Add("dateTimePrompt", new Microsoft.Bot.Builder.Dialogs.DateTimePrompt(Culture.English));
     // Add a prompt for the party size
     dialogs.Add("partySizePrompt", new Microsoft.Bot.Builder.Dialogs.NumberPrompt<int>(Culture.English));
     // Add a prompt for the user's name
     dialogs.Add("textPrompt", new Microsoft.Bot.Builder.Dialogs.TextPrompt());
}
```


# <a name="javascripttabjstab"></a>[<span data-ttu-id="1e9a3-177">JavaScript</span><span class="sxs-lookup"><span data-stu-id="1e9a3-177">JavaScript</span></span>](#tab/jstab)

<span data-ttu-id="1e9a3-178">El diálogo `reserveTable` tendrá este aspecto:</span><span class="sxs-lookup"><span data-stu-id="1e9a3-178">The `reserveTable` dialog will look like this:</span></span>

```javascript
// Reserve a table:
// Help the user to reserve a table
var reservationInfo = {};

dialogs.add('reserveTable', [
    async function(dc, args, next){
        await dc.context.sendActivity("Welcome to the reservation service.");

        reservationInfo = {}; // Clears any previous data
        await dc.prompt('dateTimePrompt', "Please provide a reservation date and time.");
    },
    async function(dc, result){
        reservationInfo.dateTime = result[0].value;

        // Ask for next info
        await dc.prompt('partySizePrompt', "How many people are in your party?");
    },
    async function(dc, result){
        reservationInfo.partySize = result;

        // Ask for next info
        await dc.prompt('textPrompt', "Whose name will this be under?");
    },
    async function(dc, result){
        reservationInfo.reserveName = result;
        
        // Reservation confirmation
        var msg = `Reservation confirmed. Reservation details: 
            <br/>Date/Time: ${reservationInfo.dateTime} 
            <br/>Party size: ${reservationInfo.partySize} 
            <br/>Reservation name: ${reservationInfo.reserveName}`;
        await dc.context.sendActivity(msg);
        await dc.end();
    }
]);
```

---

<span data-ttu-id="1e9a3-179">El flujo de conversación del diálogo `reserveTable` formulará al usuario 3 preguntas en los tres primeros pasos de la cascada.</span><span class="sxs-lookup"><span data-stu-id="1e9a3-179">The conversation flow of the `reserveTable` dialog will ask the user 3 questions through the first three steps of the waterfall.</span></span> <span data-ttu-id="1e9a3-180">El paso cuatro procesa la respuesta a la última pregunta y envía al usuario la confirmación de la reserva.</span><span class="sxs-lookup"><span data-stu-id="1e9a3-180">Step four processes the answer to the last question and sends the user the reservation confirmation.</span></span>



# <a name="ctabcstab"></a>[<span data-ttu-id="1e9a3-181">C#</span><span class="sxs-lookup"><span data-stu-id="1e9a3-181">C#</span></span>](#tab/cstab)
<span data-ttu-id="1e9a3-182">Cada paso de la cascada del diálogo `reserveTable` usa un aviso para pedir información al usuario.</span><span class="sxs-lookup"><span data-stu-id="1e9a3-182">Each waterfall step of the `reserveTable` dialog uses a prompt to ask the user for information.</span></span> <span data-ttu-id="1e9a3-183">El código siguiente se usó para agregar los avisos al conjunto de diálogos.</span><span class="sxs-lookup"><span data-stu-id="1e9a3-183">The following code was used to add the prompts to the dialog set.</span></span>

```cs
dialogs.Add("dateTimePrompt", new Microsoft.Bot.Builder.Dialogs.DateTimePrompt(Culture.English));
dialogs.Add("partySizePrompt", new Microsoft.Bot.Builder.Dialogs.NumberPrompt<int>(Culture.English));
dialogs.Add("textPrompt", new Microsoft.Bot.Builder.Dialogs.TextPrompt());
```

# <a name="javascripttabjstab"></a>[<span data-ttu-id="1e9a3-184">JavaScript</span><span class="sxs-lookup"><span data-stu-id="1e9a3-184">JavaScript</span></span>](#tab/jstab)

<span data-ttu-id="1e9a3-185">Para que esta cascada funcione, también necesitará agregar los avisos al objeto `dialogs`:</span><span class="sxs-lookup"><span data-stu-id="1e9a3-185">For this waterfall to work, you will also need to add these prompts to the `dialogs` object:</span></span>

```javascript
// Define prompts
// Generic prompts
dialogs.add('textPrompt', new botbuilder_dialogs.TextPrompt());
dialogs.add('dateTimePrompt', new botbuilder_dialogs.DatetimePrompt());
dialogs.add('partySizePrompt', new botbuilder_dialogs.NumberPrompt());
```

---

<span data-ttu-id="1e9a3-186">Ahora, está listo para enlazarlo a la lógica del bot.</span><span class="sxs-lookup"><span data-stu-id="1e9a3-186">Now, you are ready to hook this into the bot logic.</span></span>

## <a name="start-the-dialog"></a><span data-ttu-id="1e9a3-187">Inicio del diálogo</span><span class="sxs-lookup"><span data-stu-id="1e9a3-187">Start the dialog</span></span>

# <a name="ctabcstab"></a>[<span data-ttu-id="1e9a3-188">C#</span><span class="sxs-lookup"><span data-stu-id="1e9a3-188">C#</span></span>](#tab/cstab)
<span data-ttu-id="1e9a3-189">Modifique la opción `OnTurn` del bot para que contenga el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="1e9a3-189">Modify your bot's `OnTurn` to contain the following code:</span></span>
```cs
public async Task OnTurn(ITurnContext context)
{
    if (context.Activity.Type == ActivityTypes.Message)
    {
        // The type parameter PropertyBag inherits from 
        // Dictionary<string,object>
        var state = ConversationState<Dictionary<string, object>>.Get(context);
        var dc = dialogs.CreateContext(context, state);
        await dc.Continue();

        // Additional logic can be added to enter each dialog depending on the message received
        
        if(!context.Responded)
        {
            if (context.Activity.Text.ToLowerInvariant().Contains("reserve table"))
            {
                await dc.Begin("reserveTable");
            }
            else
            {
                await context.SendActivity($"You said '{context.Activity.Text}'");
            }
        }
    }
}
```


<span data-ttu-id="1e9a3-190">En **Startup.cs**, cambie la inicialización del middleware ConversationState para que use una clase que deriva de `Dictionary<string,object>` en lugar de derivar de `EchoState`.</span><span class="sxs-lookup"><span data-stu-id="1e9a3-190">In **Startup.cs**, change the initialization of the ConversationState middleware to use a class deriving from `Dictionary<string,object>` instead of `EchoState`.</span></span>

<span data-ttu-id="1e9a3-191">Por ejemplo, en `Configure()`:</span><span class="sxs-lookup"><span data-stu-id="1e9a3-191">For example, in `Configure()`:</span></span>
```cs
options.Middleware.Add(new ConversationState<Dictionary<string, object>>(dataStore));
```


# <a name="javascripttabjstab"></a>[<span data-ttu-id="1e9a3-192">JavaScript</span><span class="sxs-lookup"><span data-stu-id="1e9a3-192">JavaScript</span></span>](#tab/jstab)

<span data-ttu-id="1e9a3-193">Para capturar la intención del usuario para esta solicitud, agregue unas líneas de código al método `processActivity()`.</span><span class="sxs-lookup"><span data-stu-id="1e9a3-193">To capture the user intent for this request, add a few lines of code to the `processActivity()` method.</span></span> <span data-ttu-id="1e9a3-194">Modifique el método `processActivity()` del bot para que tenga este aspecto:</span><span class="sxs-lookup"><span data-stu-id="1e9a3-194">Modify your bot's `processActivity()` method to look like this:</span></span>

```javascript
// Listen for incoming activity 
server.post('/api/messages', (req, res) => {
    adapter.processActivity(req, res, async (context) => {
        const isMessage = (context.activity.type === 'message');
        // State will store all of your information 
        const convo = conversationState.get(context);
        const dc = dialogs.createContext(context, convo);

        if (isMessage) {
            // Check for valid intents
            if(context.activity.text.match(/hi/ig)){
                await dc.begin('greetings');
            }
            else if(context.activity.text.match(/reserve table/ig)){
                await dc.begin('reserveTable');
            }
        }

        if(!context.responded){
            // Continue executing the "current" dialog, if any.
            await dc.continue();

            if(!context.responded && isMessage){
                // Default message
                await context.sendActivity("Hi! I'm a simple bot. Please say 'Hi' or 'Reserve table'.");
            }
        }
    });
});
```

<span data-ttu-id="1e9a3-195">En tiempo de ejecución, cada vez que el usuario envía el mensaje que contiene la cadena `reserve table`, el bot iniciará la conversación `reserveTable`.</span><span class="sxs-lookup"><span data-stu-id="1e9a3-195">At execution time, whenever the user sends the message containing the string `reserve table`, the bot will start the `reserveTable` conversation.</span></span>

---



## <a name="next-steps"></a><span data-ttu-id="1e9a3-196">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="1e9a3-196">Next steps</span></span>

<span data-ttu-id="1e9a3-197">En este tutorial, el bot guarda las entradas del usuario en variables dentro de nuestro bot.</span><span class="sxs-lookup"><span data-stu-id="1e9a3-197">In this tutorial, the bot is saving the user's input to variables within our bot.</span></span> <span data-ttu-id="1e9a3-198">Si desea almacenar o conservar esta información, deberá agregar el estado a la capa de middleware.</span><span class="sxs-lookup"><span data-stu-id="1e9a3-198">If you want to store or persist this information, you need to add state to the middleware layer.</span></span> <span data-ttu-id="1e9a3-199">Vamos a analizar más detalladamente cómo conservar los datos de estado del usuario en el siguiente tutorial.</span><span class="sxs-lookup"><span data-stu-id="1e9a3-199">Let's take a closer look at how to persist user state data in the next tutorial.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="1e9a3-200">Conservación de datos de usuario</span><span class="sxs-lookup"><span data-stu-id="1e9a3-200">Persist user data</span></span>](bot-builder-tutorial-persist-user-inputs.md)
