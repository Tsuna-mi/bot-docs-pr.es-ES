---
title: Introducción a los diálogos | Microsoft Docs
description: Obtenga información sobre cómo usar los diálogos en el SDK de Bot Builder para Node.js para modelar las conversaciones y administrar el flujo de conversación.
author: DucVo
ms.author: v-ducvo
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: c92d68c429dc48722640f29032338528ac96e24c
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39304792"
---
# <a name="dialogs-in-the-bot-builder-sdk-for-nodejs"></a><span data-ttu-id="ab550-103">Diálogos en Bot Builder SDK para .Node.js</span><span class="sxs-lookup"><span data-stu-id="ab550-103">Dialogs in the Bot Builder SDK for Node.js</span></span>
> [!div class="op_single_selector"]
> - [.NET](../dotnet/bot-builder-dotnet-dialogs.md)
> - [Node.js](../nodejs/bot-builder-nodejs-dialog-overview.md)

<span data-ttu-id="ab550-106">Los diálogos del SDK de Bot Builder para Node.js le permiten modelar las conversaciones y administrar el flujo de conversación.</span><span class="sxs-lookup"><span data-stu-id="ab550-106">Dialogs in the Bot Builder SDK for Node.js allow you to model conversations and manage conversation flow.</span></span> <span data-ttu-id="ab550-107">Un bot se comunica con un usuario a través de conversaciones.</span><span class="sxs-lookup"><span data-stu-id="ab550-107">A bot communicates with a user via conversations.</span></span> <span data-ttu-id="ab550-108">Las conversaciones se organizan en diálogos.</span><span class="sxs-lookup"><span data-stu-id="ab550-108">Conversations are organized into dialogs.</span></span> <span data-ttu-id="ab550-109">Los diálogos pueden contener preguntas y pasos en cascada.</span><span class="sxs-lookup"><span data-stu-id="ab550-109">Dialogs can contain waterfall steps, and prompts.</span></span> <span data-ttu-id="ab550-110">A medida que el usuario interactúa con el bot, el bot inicia, detiene y cambia entre diferentes diálogos en respuesta a los mensajes del usuario.</span><span class="sxs-lookup"><span data-stu-id="ab550-110">As the user interacts with the bot, the bot will start, stop, and switch between various dialogs in response to user messages.</span></span> <span data-ttu-id="ab550-111">La comprensión del funcionamiento de los diálogos es clave para diseñar y crear correctamente bots excelentes.</span><span class="sxs-lookup"><span data-stu-id="ab550-111">Understanding how dialogs work is key to successfully designing and creating great bots.</span></span> 

<span data-ttu-id="ab550-112">En este artículo se presentan los conceptos de diálogo.</span><span class="sxs-lookup"><span data-stu-id="ab550-112">This article introduces dialog concepts.</span></span> <span data-ttu-id="ab550-113">Después de leer este artículo, siga los vínculos de la sección [Pasos siguientes](#next-steps) para profundizar más en estos conceptos.</span><span class="sxs-lookup"><span data-stu-id="ab550-113">After you read this article, then follow the links in the [Next steps](#next-steps) section to dive deeper into these concepts.</span></span>

## <a name="conversations-through-dialogs"></a><span data-ttu-id="ab550-114">Conversaciones a través de diálogos</span><span class="sxs-lookup"><span data-stu-id="ab550-114">Conversations through dialogs</span></span>

<span data-ttu-id="ab550-115">El SDK de Bot Builder para Node.js define una conversación como la comunicación entre un bot y un usuario a través de uno o varios diálogos.</span><span class="sxs-lookup"><span data-stu-id="ab550-115">Bot Builder SDK for Node.js defines a conversation as the communication between a bot and a user through one or more dialogs.</span></span> <span data-ttu-id="ab550-116">Un diálogo, en su nivel más básico, es un módulo reutilizable que realiza una operación o recopila información de un usuario.</span><span class="sxs-lookup"><span data-stu-id="ab550-116">A dialog, at its most basic level, is a reusable module that performs an operation or collects information from a user.</span></span> <span data-ttu-id="ab550-117">Puede englobar la lógica compleja del bot en un código de diálogo reutilizable.</span><span class="sxs-lookup"><span data-stu-id="ab550-117">You can encapsulate the complex logic of your bot in reusable dialog code.</span></span>

<span data-ttu-id="ab550-118">Una conversación se puede estructurar y cambiar de muchas maneras:</span><span class="sxs-lookup"><span data-stu-id="ab550-118">A conversation can be structured and changed in many ways:</span></span>

- <span data-ttu-id="ab550-119">Puede originarse desde el [diálogo predeterminado](#default-dialog).</span><span class="sxs-lookup"><span data-stu-id="ab550-119">It can originate from your [default dialog](#default-dialog).</span></span>
- <span data-ttu-id="ab550-120">Se puede redirigir desde un diálogo a otro.</span><span class="sxs-lookup"><span data-stu-id="ab550-120">It can be redirected from one dialog to another.</span></span>
- <span data-ttu-id="ab550-121">Se puede reanudar.</span><span class="sxs-lookup"><span data-stu-id="ab550-121">It can be resumed.</span></span>
- <span data-ttu-id="ab550-122">Puede seguir un patrón de [cascada](bot-builder-nodejs-dialog-waterfall.md), que guía al usuario a través de un conjunto de pasos o [pregunta](bot-builder-nodejs-dialog-prompt.md) al usuario una serie de cuestiones.</span><span class="sxs-lookup"><span data-stu-id="ab550-122">It can follow a [waterfall](bot-builder-nodejs-dialog-waterfall.md) pattern, which guides the user through a series of steps or [prompts](bot-builder-nodejs-dialog-prompt.md) the user with a series of questions.</span></span>
- <span data-ttu-id="ab550-123">Puede usar [acciones](bot-builder-nodejs-dialog-actions.md) que escuchan palabras o frases que desencadenan un diálogo distinto.</span><span class="sxs-lookup"><span data-stu-id="ab550-123">It can use [actions](bot-builder-nodejs-dialog-actions.md) that listen for words or phrases that trigger a different dialog.</span></span> 

<span data-ttu-id="ab550-124">Puede pensar en una conversación como un padre en diálogos.</span><span class="sxs-lookup"><span data-stu-id="ab550-124">You can think of a conversation like a parent to dialogs.</span></span> <span data-ttu-id="ab550-125">Por lo tanto, una conversación contiene una *pila de diálogos* y mantiene su propio conjunto de datos de estado; concretamente, `conversationData` y `privateConversationData`.</span><span class="sxs-lookup"><span data-stu-id="ab550-125">As such, a conversation contains a *dialog stack* and maintain its own set of state data; namely, the `conversationData` and the `privateConversationData`.</span></span> <span data-ttu-id="ab550-126">Un diálogo, por otro parte, mantiene `dialogData`.</span><span class="sxs-lookup"><span data-stu-id="ab550-126">A dialog, on the other hand, maintains the `dialogData`.</span></span> <span data-ttu-id="ab550-127">Para obtener más información sobre los datos de estado, consulte [Administración de datos de estado](bot-builder-nodejs-state.md).</span><span class="sxs-lookup"><span data-stu-id="ab550-127">For more information on state data, see [Manage state data](bot-builder-nodejs-state.md).</span></span>

## <a name="dialog-stack"></a><span data-ttu-id="ab550-128">Pila de diálogos</span><span class="sxs-lookup"><span data-stu-id="ab550-128">Dialog stack</span></span>

<span data-ttu-id="ab550-129">Un bot interactúa con un usuario a través de una serie de diálogos que se mantienen en una pila de diálogos.</span><span class="sxs-lookup"><span data-stu-id="ab550-129">A bot interacts with a user through a series of dialogs that are maintained on a dialog stack.</span></span> <span data-ttu-id="ab550-130">Los diálogos se insertan y se extraen de la pila en el transcurso de una conversación.</span><span class="sxs-lookup"><span data-stu-id="ab550-130">Dialogs are pushed on and popped off the stack in the course of a conversation.</span></span> <span data-ttu-id="ab550-131">La pila funciona como una pila LIFO normal; es decir, el último diálogo agregado será el primero en completarse.</span><span class="sxs-lookup"><span data-stu-id="ab550-131">The stack works like a normal LIFO stack; meaning, the last dialog added will be the first one to complete.</span></span> <span data-ttu-id="ab550-132">Una vez completado un cuadro de diálogo, el control se devuelve al diálogo anterior en la pila.</span><span class="sxs-lookup"><span data-stu-id="ab550-132">Once a dialog completes then control is returned to the previous dialog on the stack.</span></span>

<span data-ttu-id="ab550-133">Cuando se inicia una conversación de bot por primera vez o cuando finaliza una conversación, la pila de diálogos está vacía.</span><span class="sxs-lookup"><span data-stu-id="ab550-133">When a bot conversation first starts or when a conversation ends, the dialog stack is empty.</span></span> <span data-ttu-id="ab550-134">En este momento, cuando el usuario envía un mensaje al bot, el bot responde con el *diálogo predeterminado*.</span><span class="sxs-lookup"><span data-stu-id="ab550-134">At this point, when the a user sends a message to the bot, the bot will respond with the *default dialog*.</span></span>

## <a name="default-dialog"></a><span data-ttu-id="ab550-135">Diálogo predeterminado</span><span class="sxs-lookup"><span data-stu-id="ab550-135">Default dialog</span></span>

<span data-ttu-id="ab550-136">Antes de la versión 3.5 de Bot Framework, un diálogo *raíz* se definía mediante la adición de un diálogo denominado `/`, que conduce a convenciones de nomenclatura similares a las de las direcciones URL.</span><span class="sxs-lookup"><span data-stu-id="ab550-136">Prior to Bot Framework version 3.5, a *root* dialog is defined by adding a dialog named `/`, which lead to naming conventions similar to that of URLs.</span></span> <span data-ttu-id="ab550-137">Esta convención de nomenclatura no era adecuada para la nomenclatura de los diálogos.</span><span class="sxs-lookup"><span data-stu-id="ab550-137">This naming convention wasn't appropriate to naming dialogs.</span></span> 

> [!NOTE]
> A partir de la versión 3.5 de Bot Framework, el *diálogo predeterminado* se registra como el segundo parámetro del constructor de [`UniversalBot`](https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.universalbot.html#constructor).  

<span data-ttu-id="ab550-139">En el fragmento de código siguiente se muestra cómo definir el diálogo predeterminado al crear el objeto `UniversalBot`.</span><span class="sxs-lookup"><span data-stu-id="ab550-139">The following code snippet shows how to define the default dialog when creating the `UniversalBot` object.</span></span>

```javascript
var bot = new builder.UniversalBot(connector, [
    //...Default dialog waterfall steps...
    ]);
```

<span data-ttu-id="ab550-140">El diálogo predeterminado se ejecuta siempre que la pila de diálogos está vacía y ningún otro cuadro de diálogo se [desencadena](bot-builder-nodejs-dialog-actions.md) a través de LUIS u otro [reconocedor](bot-builder-nodejs-recognize-intent-messages.md).</span><span class="sxs-lookup"><span data-stu-id="ab550-140">The default dialog runs whenever the dialog stack is empty and no other dialog is [triggered](bot-builder-nodejs-dialog-actions.md) via LUIS or another [recognizer](bot-builder-nodejs-recognize-intent-messages.md).</span></span> <span data-ttu-id="ab550-141">Como el diálogo predeterminado es la primera respuesta del bot al usuario, dicho diálogo debe proporcionar información contextual al usuario, como una lista de los comandos disponibles o una introducción a lo que puede hacer el bot.</span><span class="sxs-lookup"><span data-stu-id="ab550-141">As the default dialog is the bot's first response to the user, the default dialog should provide some contextual information to the user, such as a list of available commands or an overview of what the bot can do.</span></span>

## <a name="dialog-handlers"></a><span data-ttu-id="ab550-142">Controladores de diálogos</span><span class="sxs-lookup"><span data-stu-id="ab550-142">Dialog handlers</span></span>

<span data-ttu-id="ab550-143">El controlador de diálogos administra el flujo de una conversación.</span><span class="sxs-lookup"><span data-stu-id="ab550-143">The dialog handler manages the flow of a conversation.</span></span> <span data-ttu-id="ab550-144">Para avanzar a través de una conversación, el controlador de diálogos dirige el flujo iniciando y finalizando diálogos.</span><span class="sxs-lookup"><span data-stu-id="ab550-144">To progress through a conversation, the dialog handler directs the flow by starting and ending dialogs.</span></span> 

## <a name="starting-and-ending-dialogs"></a><span data-ttu-id="ab550-145">Inicio y finalización de diálogos</span><span class="sxs-lookup"><span data-stu-id="ab550-145">Starting and ending dialogs</span></span>

<span data-ttu-id="ab550-146">Para iniciar un diálogo nuevo (e insertarlo en la pila), utilice [`session.beginDialog()`](http://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.session#begindialog).</span><span class="sxs-lookup"><span data-stu-id="ab550-146">To start a new dialog (and push it onto the stack), use [`session.beginDialog()`](http://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.session#begindialog).</span></span> <span data-ttu-id="ab550-147">Para finalizar un diálogo (y quitarlo de la pila, devolviendo el control al diálogo que realiza la llamada), use [`session.endDialog()`](http://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.session#enddialog) o [`session.endDialogWithResult()`](http://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.session#enddialogwithresult).</span><span class="sxs-lookup"><span data-stu-id="ab550-147">To end a dialog (and remove it from the stack, returning control to the calling dialog), use either [`session.endDialog()`](http://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.session#enddialog) or [`session.endDialogWithResult()`](http://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.session#enddialogwithresult).</span></span> 

## <a name="using-waterfalls-and-prompts"></a><span data-ttu-id="ab550-148">Uso de cascadas y preguntas</span><span class="sxs-lookup"><span data-stu-id="ab550-148">Using waterfalls and prompts</span></span>

<span data-ttu-id="ab550-149">La [cascada](bot-builder-nodejs-dialog-waterfall.md) es una manera sencilla de modelar y administrar el flujo de conversación.</span><span class="sxs-lookup"><span data-stu-id="ab550-149">[Waterfall](bot-builder-nodejs-dialog-waterfall.md) is a simple way to model and manage conversation flow.</span></span> <span data-ttu-id="ab550-150">Una cascada contiene una secuencia de pasos.</span><span class="sxs-lookup"><span data-stu-id="ab550-150">A waterfall contains a sequence of steps.</span></span> <span data-ttu-id="ab550-151">En cada paso, puede completar una acción en nombre del usuario o [pedir](bot-builder-nodejs-dialog-prompt.md) información al usuario.</span><span class="sxs-lookup"><span data-stu-id="ab550-151">In each step, you can either complete an action on behalf of the user or [prompt](bot-builder-nodejs-dialog-prompt.md) the user for information.</span></span>

<span data-ttu-id="ab550-152">Una cascada se implementa mediante un diálogo que se compone de una colección de funciones.</span><span class="sxs-lookup"><span data-stu-id="ab550-152">A waterfall is implemented using a dialog that's made up of a collection of functions.</span></span> <span data-ttu-id="ab550-153">Cada función define un paso de la cascada.</span><span class="sxs-lookup"><span data-stu-id="ab550-153">Each function defines a step in the waterfall.</span></span> <span data-ttu-id="ab550-154">En el ejemplo de código siguiente se muestra una conversación simple que usa una cascada de dos pasos para preguntar al usuario cómo se llama y saludarlo por su nombre.</span><span class="sxs-lookup"><span data-stu-id="ab550-154">The following code sample shows a simple conversation that uses a two step waterfall to prompt the user for their name and greet them by name.</span></span>

```javascript
// Ask the user for their name and greet them by name.
bot.dialog('greetings', [
    function (session) {
        builder.Prompts.text(session, 'Hi! What is your name?');
    },
    function (session, results) {
        session.endDialog(`Hello ${results.response}!`);
    }
]);
```

<span data-ttu-id="ab550-155">Cuando un bot llega al final de la cascada sin finalizar el diálogo, el siguiente mensaje del usuario reiniciará ese diálogo en el primer paso de la cascada.</span><span class="sxs-lookup"><span data-stu-id="ab550-155">When a bot reaches the end of the waterfall without ending the dialog, the next message from the user will restart that dialog at step one of the waterfall.</span></span> <span data-ttu-id="ab550-156">Esto puede provocar frustración, ya que el usuario puede sentir que está atrapado en un bucle.</span><span class="sxs-lookup"><span data-stu-id="ab550-156">This may lead to frustrations as the user may feel like they are trapped in a loop.</span></span> <span data-ttu-id="ab550-157">Para evitar esta situación, cuando una conversación o un diálogo llega a su fin, es recomendable llamar explícitamente a `endDialog`, `endDialogWithResult` o `endConversation`.</span><span class="sxs-lookup"><span data-stu-id="ab550-157">To avoid this situation, when a conversation or dialog has come to an end, it is best practice to explicitly call `endDialog`, `endDialogWithResult`, or `endConversation`.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ab550-158">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="ab550-158">Next steps</span></span>

<span data-ttu-id="ab550-159">Para profundizar más en los diálogos, es importante entender cómo funciona el patrón de cascada y cómo usarlo para guiar a los usuarios a través de un proceso.</span><span class="sxs-lookup"><span data-stu-id="ab550-159">To dive deeper into dialogs, it is important to understand how waterfall pattern works and how to use it to guide users through a process.</span></span>

> [!div class="nextstepaction"]
> [Definición de los pasos de la conversación con cascadas](bot-builder-nodejs-dialog-waterfall.md)
