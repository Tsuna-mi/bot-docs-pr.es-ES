---
title: Controlar las acciones del usuario | Microsoft Docs
description: Obtenga información sobre cómo controlar las acciones del usuario al permitir que el bot escuche y controle la entrada de usuario que contenga ciertas palabras clave mediante el Bot Builder SDK para Node.js.
author: DucVo
ms.author: v-ducvo
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: 365513c6370025fda807bfc8266ba68f329ceb88
ms.sourcegitcommit: 2dc75701b169d822c9499e393439161bc87639d2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42905706"
---
# <a name="handle-user-actions"></a><span data-ttu-id="034fe-103">Controlar acciones del usuario</span><span class="sxs-lookup"><span data-stu-id="034fe-103">Handle user actions</span></span>

[!INCLUDE [pre-release-label](../includes/pre-release-label-v3.md)]

> [!div class="op_single_selector"]
> - [.NET](../dotnet/bot-builder-dotnet-global-handlers.md)
> - [Node.js](../nodejs/bot-builder-nodejs-dialog-actions.md)

<span data-ttu-id="034fe-106">Normalmente los usuarios intentan tener acceso a cierta funcionalidad dentro de un bot con palabras clave, como "ayuda", "cancelar" o "empezar de nuevo".</span><span class="sxs-lookup"><span data-stu-id="034fe-106">Users commonly attempt to access certain functionality within a bot by using keywords like "help", "cancel", or "start over."</span></span> <span data-ttu-id="034fe-107">Los usuarios hacen esto en el medio de una conversación, cuando el bot está esperando una respuesta diferente.</span><span class="sxs-lookup"><span data-stu-id="034fe-107">Users do this in the middle of a conversation, when the bot is expecting a different response.</span></span> <span data-ttu-id="034fe-108">Al implementar **acciones**, puede diseñar el bot para que controle dichas solicitudes de manera más correcta.</span><span class="sxs-lookup"><span data-stu-id="034fe-108">By implementing **actions**, you can design your bot to handle such requests more gracefully.</span></span> <span data-ttu-id="034fe-109">Los controladores examinarán la entrada del usuario para las palabras clave que especifique, por ejemplo, "ayuda", "cancelar" o "empezar de nuevo" y responder según corresponda.</span><span class="sxs-lookup"><span data-stu-id="034fe-109">The handlers will examine user input for the keywords that you specify, such as "help", "cancel", or "start over," and respond appropriately.</span></span> 

![cómo hablan los usuarios](../media/designing-bots/capabilities/trigger-actions.png)

## <a name="action-types"></a><span data-ttu-id="034fe-111">Tipos de acción</span><span class="sxs-lookup"><span data-stu-id="034fe-111">Action types</span></span>

<span data-ttu-id="034fe-112">Los tipos de acciones que puede adjuntar a un cuadro de diálogo se muestran en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="034fe-112">The action types you can attach to a dialog are listed in the table below.</span></span> <span data-ttu-id="034fe-113">El vínculo para cada nombre de acción le llevará a una sección que proporcionan más detalles sobre esa acción.</span><span class="sxs-lookup"><span data-stu-id="034fe-113">The link for each action name will take you to a section that provide more details about that action.</span></span>

| <span data-ttu-id="034fe-114">.</span><span class="sxs-lookup"><span data-stu-id="034fe-114">Action</span></span> | <span data-ttu-id="034fe-115">Ámbito</span><span class="sxs-lookup"><span data-stu-id="034fe-115">Scope</span></span> | <span data-ttu-id="034fe-116">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="034fe-116">Description</span></span> |
|------|------| ---- |
| [<span data-ttu-id="034fe-117">triggerAction</span><span class="sxs-lookup"><span data-stu-id="034fe-117">triggerAction</span></span>](#bind-a-triggeraction) | <span data-ttu-id="034fe-118">Global</span><span class="sxs-lookup"><span data-stu-id="034fe-118">Global</span></span> | <span data-ttu-id="034fe-119">Enlaza una acción al cuadro de diálogo que borrará la pila del cuadro de diálogo y se insertará en la parte inferior de la pila.</span><span class="sxs-lookup"><span data-stu-id="034fe-119">Binds an action to the dialog that will clear the dialog stack and push itself onto the bottom of stack.</span></span> <span data-ttu-id="034fe-120">Use la opción `onSelectAction` para invalidar este comportamiento predeterminado.</span><span class="sxs-lookup"><span data-stu-id="034fe-120">Use `onSelectAction` option to override this default behavior.</span></span> |
| [<span data-ttu-id="034fe-121">customAction</span><span class="sxs-lookup"><span data-stu-id="034fe-121">customAction</span></span>](#bind-a-customaction) | <span data-ttu-id="034fe-122">Global</span><span class="sxs-lookup"><span data-stu-id="034fe-122">Global</span></span> | <span data-ttu-id="034fe-123">Enlaza una acción personalizada al bot que pueda procesar información o realizar una acción sin afectar a la pila del cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="034fe-123">Binds a custom action to the bot that can process information or take action without affecting the dialog stack.</span></span> <span data-ttu-id="034fe-124">Use la opción `onSelectAction` para personalizar la funcionalidad de esta acción.</span><span class="sxs-lookup"><span data-stu-id="034fe-124">Use `onSelectAction` option to customize the functionality of this action.</span></span> |
[<span data-ttu-id="034fe-125">beginDialogAction</span><span class="sxs-lookup"><span data-stu-id="034fe-125">beginDialogAction</span></span>](#bind-a-begindialogaction) | <span data-ttu-id="034fe-126">Contextual</span><span class="sxs-lookup"><span data-stu-id="034fe-126">Contextual</span></span> | <span data-ttu-id="034fe-127">Enlaza una acción al cuadro de diálogo que inicia otro cuadro de diálogo cuando se desencadena.</span><span class="sxs-lookup"><span data-stu-id="034fe-127">Binds an action to the dialog that starts another dialog when it is triggered.</span></span> <span data-ttu-id="034fe-128">El cuadro de diálogo inicial se insertará en la pila y se extraerá una vez que finalice.</span><span class="sxs-lookup"><span data-stu-id="034fe-128">The starting dialog will be pushed onto the stack and popped off once it ends.</span></span> |
[<span data-ttu-id="034fe-129">reloadAction</span><span class="sxs-lookup"><span data-stu-id="034fe-129">reloadAction</span></span>](#bind-a-reloadaction) | <span data-ttu-id="034fe-130">Contextual</span><span class="sxs-lookup"><span data-stu-id="034fe-130">Contextual</span></span> | <span data-ttu-id="034fe-131">Enlaza una acción al cuadro de diálogo que hace que otro cuadro de diálogo se recargue cuando se desencadena.</span><span class="sxs-lookup"><span data-stu-id="034fe-131">Binds an action to the dialog that causes the dialog to reload when it is triggered.</span></span> <span data-ttu-id="034fe-132">Puede usar `reloadAction` para controlar las expresiones del usuario, como "empezar de nuevo".</span><span class="sxs-lookup"><span data-stu-id="034fe-132">You can use `reloadAction` to handle user utterances like "start over."</span></span> |
[<span data-ttu-id="034fe-133">cancelAction</span><span class="sxs-lookup"><span data-stu-id="034fe-133">cancelAction</span></span>](#bind-a-cancelaction) | <span data-ttu-id="034fe-134">Contextual</span><span class="sxs-lookup"><span data-stu-id="034fe-134">Contextual</span></span> | <span data-ttu-id="034fe-135">Enlaza una acción al cuadro de diálogo que cancela otro cuadro de diálogo cuando se desencadena.</span><span class="sxs-lookup"><span data-stu-id="034fe-135">Binds an action to the dialog that cancels the dialog when it is triggered.</span></span> <span data-ttu-id="034fe-136">Puede usar `cancelAction` para controlar las expresiones del usuario, como "cancelar" o "no importa".</span><span class="sxs-lookup"><span data-stu-id="034fe-136">You can use `cancelAction` to handle user utterances like "cancel" or "nevermind."</span></span> |
[<span data-ttu-id="034fe-137">endConversationAction</span><span class="sxs-lookup"><span data-stu-id="034fe-137">endConversationAction</span></span>](#bind-an-endconversationaction) | <span data-ttu-id="034fe-138">Contextual</span><span class="sxs-lookup"><span data-stu-id="034fe-138">Contextual</span></span> | <span data-ttu-id="034fe-139">Enlaza una acción al cuadro de diálogo que finaliza la conversación con el usuario cuando se desencadena.</span><span class="sxs-lookup"><span data-stu-id="034fe-139">Binds an action to the dialog that ends the conversation with the user when triggered.</span></span> <span data-ttu-id="034fe-140">Puede usar `endConversationAction` para controlar las expresiones del usuario, como "adiós".</span><span class="sxs-lookup"><span data-stu-id="034fe-140">You can use `endConversationAction` to handle user utterances like "goodbye."</span></span> |

## <a name="action-precedence"></a><span data-ttu-id="034fe-141">Prioridad de acciones</span><span class="sxs-lookup"><span data-stu-id="034fe-141">Action precedence</span></span> 

<span data-ttu-id="034fe-142">Cuando un bot recibe una expresión de un usuario, comprueba la expresión frente a todas las acciones registradas en la pila del cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="034fe-142">When a bot receives an utterance from a user, it checks the utterance against all the registered actions that are on the dialog stack.</span></span> <span data-ttu-id="034fe-143">La coincidencia se inicia desde la parte superior de la pila hasta el final.</span><span class="sxs-lookup"><span data-stu-id="034fe-143">The matching starts from the top of the stack down to the bottom of the stack.</span></span> <span data-ttu-id="034fe-144">Si no hay ninguna coincidencia, luego comprueba la expresión frente a la opción `matches` de todas las acciones globales.</span><span class="sxs-lookup"><span data-stu-id="034fe-144">If nothing matches, then it check the utterance against the `matches` option of all global actions.</span></span>

<span data-ttu-id="034fe-145">La prioridad de acciones es importante en casos donde se usa el mismo comando en contextos diferentes.</span><span class="sxs-lookup"><span data-stu-id="034fe-145">The action precedence is important in cases where you use the same command in difference contexts.</span></span> <span data-ttu-id="034fe-146">Por ejemplo, puede usar el comando "Help" para el bot como ayuda general.</span><span class="sxs-lookup"><span data-stu-id="034fe-146">For example, you can use the "Help" command for the bot as a general help.</span></span> <span data-ttu-id="034fe-147">También puede usar "Help" para cada una de las tareas, pero estos comandos de ayuda son contextuales a cada tarea.</span><span class="sxs-lookup"><span data-stu-id="034fe-147">You can also use "Help" for each of the tasks but these help commands are contextual to each task.</span></span> <span data-ttu-id="034fe-148">Para obtener un ejemplo funcional que ahonda en este punto, consulte [Respond to user input](bot-builder-nodejs-dialog-manage-conversation-flow.md#respond-to-user-input) (Responder a la entrada del usuario).</span><span class="sxs-lookup"><span data-stu-id="034fe-148">For a working sample that elaborates on this point, see [Respond to user input](bot-builder-nodejs-dialog-manage-conversation-flow.md#respond-to-user-input).</span></span>

## <a name="bind-actions-to-dialog"></a><span data-ttu-id="034fe-149">Enlazar acciones al cuadro de diálogo</span><span class="sxs-lookup"><span data-stu-id="034fe-149">Bind actions to dialog</span></span>

<span data-ttu-id="034fe-150">Las expresiones del usuario o los clics en botones pueden *desencadenar* una acción, que está asociada con un [cuadro de diálogo](https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.dialog.html).</span><span class="sxs-lookup"><span data-stu-id="034fe-150">Either user utterances or button clicks can *trigger* an action, which is associated with a [dialog](https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.dialog.html).</span></span>
<span data-ttu-id="034fe-151">Si se especifica *matches*, la acción escuchará al usuario hasta que pronuncie una palabra o frase que desencadene la acción.</span><span class="sxs-lookup"><span data-stu-id="034fe-151">If *matches* is specified, the action will listen for the user to say a word or a phrase that triggers the action.</span></span>  <span data-ttu-id="034fe-152">La opción `matches` puede tomar una expresión regular o el nombre de un [reconocedor][RecognizeIntent].</span><span class="sxs-lookup"><span data-stu-id="034fe-152">The `matches` option can take a regular expression or the name of a [recognizer][RecognizeIntent].</span></span>
<span data-ttu-id="034fe-153">Para enlazar la acción al clic en un botón, use [CardAction.dialogAction()] [ CardAction] para desencadenar la acción.</span><span class="sxs-lookup"><span data-stu-id="034fe-153">To bind the action to a button click, use [CardAction.dialogAction()][CardAction] to trigger the action.</span></span>

<span data-ttu-id="034fe-154">Las acciones son *encadenables*, lo que le permite enlazar tantas acciones a un cuadro de diálogo como desee.</span><span class="sxs-lookup"><span data-stu-id="034fe-154">Actions are *chainable*, which allows you to bind as many actions to a dialog as you want.</span></span>

### <a name="bind-a-triggeraction"></a><span data-ttu-id="034fe-155">Enlazar triggerAction</span><span class="sxs-lookup"><span data-stu-id="034fe-155">Bind a triggerAction</span></span>

<span data-ttu-id="034fe-156">Para enlazar [triggerAction][triggerAction] a un cuadro de diálogo, haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="034fe-156">To bind a [triggerAction][triggerAction] to a dialog, do the following:</span></span>

```javascript
// Order dinner.
bot.dialog('orderDinner', [
    //...waterfall steps...
])
// Once triggered, will clear the dialog stack and pushes
// the 'orderDinner' dialog onto the bottom of stack.
.triggerAction({
    matches: /^order dinner$/i
});
```

<span data-ttu-id="034fe-157">Enlazar `triggerAction` a un cuadro de diálogo lo registra en el bot.</span><span class="sxs-lookup"><span data-stu-id="034fe-157">Binding a `triggerAction` to a dialog registers it to the bot.</span></span> <span data-ttu-id="034fe-158">Una vez que se desencadena, `triggerAction` borrará la pila del cuadro de diálogo e insertará el cuadro de diálogo desencadenado en la pila.</span><span class="sxs-lookup"><span data-stu-id="034fe-158">Once triggered, the `triggerAction` will clear the dialog stack and push the triggered dialog onto the stack.</span></span> <span data-ttu-id="034fe-159">Esta acción está concebida para cambiar entre [temas de conversación](bot-builder-nodejs-dialog-manage-conversation-flow.md#change-the-topic-of-conversation) o para permitir que los usuarios soliciten tareas independientes arbitrarias.</span><span class="sxs-lookup"><span data-stu-id="034fe-159">This action is best used to switch between [topic of conversation](bot-builder-nodejs-dialog-manage-conversation-flow.md#change-the-topic-of-conversation) or to allow users to request arbitrary stand-alone tasks.</span></span> <span data-ttu-id="034fe-160">Si desea invalidar el comportamiento por el que esta acción borra la pila del cuadro de diálogo, agregue una opción `onSelectAction` a `triggerAction`.</span><span class="sxs-lookup"><span data-stu-id="034fe-160">If you want to override the behavior where this action clears the dialog stack, add an `onSelectAction` option to the `triggerAction`.</span></span>

<span data-ttu-id="034fe-161">El siguiente fragmento de código muestra cómo proporcionar ayuda general de un contexto global sin borrar la pila del cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="034fe-161">The code snippet below shows how to provide general help from a global context without clearing the dialog stack.</span></span>

```javascript
bot.dialog('help', function (session, args, next) {
    //Send a help message
    session.endDialog("Global help menu.");
})
// Once triggered, will start a new dialog as specified by
// the 'onSelectAction' option.
.triggerAction({
    matches: /^help$/i,
    onSelectAction: (session, args, next) => {
        // Add the help dialog to the top of the dialog stack 
        // (override the default behavior of replacing the stack)
        session.beginDialog(args.action, args);
    }
});
```

<span data-ttu-id="034fe-162">En este caso, `triggerAction` está asociado al cuadro de diálogo `help` mismo (en contraposición al cuadro de diálogo `orderDinner`).</span><span class="sxs-lookup"><span data-stu-id="034fe-162">In this case, the `triggerAction` is attached to the `help` dialog itself (as opposed to the `orderDinner` dialog).</span></span> <span data-ttu-id="034fe-163">La opción `onSelectAction` le permite iniciar este cuadro de diálogo sin borrar la pila del cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="034fe-163">The `onSelectAction` option allows you to start this dialog without clearing the dialog stack.</span></span> <span data-ttu-id="034fe-164">Esto permite controlar las solicitudes globales, como "ayuda", "acerca de", "soporte", etc. Tenga en cuenta que la opción `onSelectAction` llama de manera explícita al método `session.beginDialog` para iniciar el cuadro de diálogo desencadenado.</span><span class="sxs-lookup"><span data-stu-id="034fe-164">This allows you to handle global requests such as "help", "about", "support", etc. Notice that the `onSelectAction` option explicitly calls the `session.beginDialog` method to start the triggered dialog.</span></span> <span data-ttu-id="034fe-165">El identificador del cuadro de diálogo desencadenado se proporciona a través de `args.action`.</span><span class="sxs-lookup"><span data-stu-id="034fe-165">The ID of the triggered dialog is provided via the `args.action`.</span></span> <span data-ttu-id="034fe-166">No codifique manualmente el identificador del cuadro de diálogo (p. ej.: 'help') en este método o, de lo contrario, es posible que obtenga errores en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="034fe-166">Do not hand code the dialog ID (e.g.: 'help') into this method otherwise you may get runtime errors.</span></span> <span data-ttu-id="034fe-167">Si desea desencadenar un mensaje de ayuda contextual para la tarea `orderDinner` misma, piense en la posibilidad de asociar `beginDialogAction` al cuadro de diálogo `orderDinner` en su lugar.</span><span class="sxs-lookup"><span data-stu-id="034fe-167">If you wish to trigger a contextual help message for the `orderDinner` task itself then consider attaching a `beginDialogAction` to the `orderDinner` dialog instead.</span></span>

### <a name="bind-a-customaction"></a><span data-ttu-id="034fe-168">Enlazar customAction</span><span class="sxs-lookup"><span data-stu-id="034fe-168">Bind a customAction</span></span>

<span data-ttu-id="034fe-169">A diferencia de otros tipos de acción, `customAction` no tiene definida ninguna acción predeterminada.</span><span class="sxs-lookup"><span data-stu-id="034fe-169">Unlike other action types, `customAction` does not have any default action defined.</span></span> <span data-ttu-id="034fe-170">Depende de usted definir lo que hace esta acción.</span><span class="sxs-lookup"><span data-stu-id="034fe-170">It's up to you to define what this action does.</span></span> <span data-ttu-id="034fe-171">La ventaja de usar `customAction` es que tendrá la opción de procesar las solicitudes del usuario sin manipular la pila del cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="034fe-171">The benefit of using `customAction` is that you have the option to process user requests without manipulating the dialog stack.</span></span> <span data-ttu-id="034fe-172">Cuando se desencadena `customAction`, la opción `onSelectAction` puede procesar la solicitud sin insertar nuevos cuadros de diálogo en la pila.</span><span class="sxs-lookup"><span data-stu-id="034fe-172">When a `customAction` is triggered, the `onSelectAction` option can process the request without pushing new dialogs onto the stack.</span></span> <span data-ttu-id="034fe-173">Una vez completada la acción, el control se devuelve al cuadro de diálogo que se encuentra en la parte superior de la pila, y el bot puede continuar.</span><span class="sxs-lookup"><span data-stu-id="034fe-173">Once the action is completed, control is passed back to the dialog that is at the top of the stack and the bot can continue.</span></span>

<span data-ttu-id="034fe-174">Puede usar `customAction` para proporcionar solicitudes generales y de acción rápida, como "¿Cuál es la temperatura exterior ahora?", "¿Qué hora es ahora en París?", "Recordarme comprar leche a las 5 p. m. hoy", etc. Estas son acciones generales que el bot puede realizar sin manipular la pila.</span><span class="sxs-lookup"><span data-stu-id="034fe-174">You can use a `customAction` to provide general and quick action requests such as "What is the temperature outside right now?", "What time is it right now in Paris?", "Remind me to buy milk at 5pm today.", etc. These are general actions that the bot can perform with out manipulating the stack.</span></span>

<span data-ttu-id="034fe-175">Otra diferencia clave con `customAction` es que se enlaza con el bot en lugar de con un cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="034fe-175">Another key difference with `customAction` is that it binds to the bot as opposed to a dialog.</span></span>

<span data-ttu-id="034fe-176">El ejemplo de código siguiente muestra cómo enlazar `customAction` al `bot` que escucha solicitudes para establecer un recordatorio.</span><span class="sxs-lookup"><span data-stu-id="034fe-176">The follow code sample shows how to bind a `customAction` to the `bot` listening for requests to set a reminder.</span></span>

```javascript
bot.customAction({
    matches: /remind|reminder/gi,
    onSelectAction: (session, args, next) => {
        // Set reminder...
        session.send("Reminder is set.");
    }
})
```

### <a name="bind-a-begindialogaction"></a><span data-ttu-id="034fe-177">Enlazar beginDialogAction</span><span class="sxs-lookup"><span data-stu-id="034fe-177">Bind a beginDialogAction</span></span>

<span data-ttu-id="034fe-178">Enlazar `beginDialogAction` a un cuadro de diálogo registrará la acción en el cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="034fe-178">Binding a `beginDialogAction` to a dialog will register the action to the dialog.</span></span> <span data-ttu-id="034fe-179">Este método iniciará otro cuadro de diálogo cuando se desencadene.</span><span class="sxs-lookup"><span data-stu-id="034fe-179">This method will start another dialog when it is triggered.</span></span> <span data-ttu-id="034fe-180">El comportamiento de esta acción es similar a llamar al método [beginDialog](https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.session.html#begindialog).</span><span class="sxs-lookup"><span data-stu-id="034fe-180">The behavior of this action is similar to calling the [beginDialog](https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.session.html#begindialog) method.</span></span> <span data-ttu-id="034fe-181">El nuevo cuadro de diálogo se inserta en la parte superior de la pila del cuadro de diálogo, por lo que no termina automáticamente la tarea actual.</span><span class="sxs-lookup"><span data-stu-id="034fe-181">The new dialog is pushed onto the top of the dialog stack so it does not automatically end the current task.</span></span> <span data-ttu-id="034fe-182">La tarea actual continúa una vez que finaliza el nuevo cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="034fe-182">The current task is continued once the new dialog ends.</span></span> 

<span data-ttu-id="034fe-183">El fragmento de código siguiente muestra cómo enlazar [beginDialogAction][beginDialogAction] a un cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="034fe-183">The following code snippet shows how to bind a [beginDialogAction][beginDialogAction] to a dialog.</span></span>

```javascript
// Order dinner.
bot.dialog('orderDinner', [
    //...waterfall steps...
])
// Once triggered, will start the 'showDinnerCart' dialog.
// Then, the waterfall will resumed from the step that was interrupted.
.beginDialogAction('showCartAction', 'showDinnerCart', {
    matches: /^show cart$/i
});

// Show dinner items in cart
bot.dialog('showDinnerCart', function(session){
    for(var i = 1; i < session.conversationData.orders.length; i++){
        session.send(`You ordered: ${session.conversationData.orders[i].Description} for a total of $${session.conversationData.orders[i].Price}.`);
    }

    // End this dialog
    session.endDialog(`Your total is: $${session.conversationData.orders[0].Price}`);
});
```

<span data-ttu-id="034fe-184">En casos donde deba pasar argumentos adicionales en el nuevo cuadro de diálogo, puede agregar una opción [`dialogArgs`](https://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.idialogactionoptions#dialogargs) a la acción.</span><span class="sxs-lookup"><span data-stu-id="034fe-184">In cases where you need to pass additional arguments into the new dialog, you can add a [`dialogArgs`](https://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.idialogactionoptions#dialogargs) option to the action.</span></span>

<span data-ttu-id="034fe-185">Mediante el ejemplo anterior, puede modificarlo para que acepte argumentos pasados a través de `dialogArgs`.</span><span class="sxs-lookup"><span data-stu-id="034fe-185">Using the sample above, you can modify it to accept arguments passed in via the `dialogArgs`.</span></span>

```javascript
// Order dinner.
bot.dialog('orderDinner', [
    //...waterfall steps...
])
// Once triggered, will start the 'showDinnerCart' dialog.
// Then, the waterfall will resumed from the step that was interrupted.
.beginDialogAction('showCartAction', 'showDinnerCart', {
    matches: /^show cart$/i,
    dialogArgs: {
        showTotal: true;
    }
});

// Show dinner items in cart with the option to show total or not.
bot.dialog('showDinnerCart', function(session, args){
    for(var i = 1; i < session.conversationData.orders.length; i++){
        session.send(`You ordered: ${session.conversationData.orders[i].Description} for a total of $${session.conversationData.orders[i].Price}.`);
    }

    if(args && args.showTotal){
        // End this dialog with total.
        session.endDialog(`Your total is: $${session.conversationData.orders[0].Price}`);
    }
    else{
        session.endDialog(); // Ends without a message.
    }
});
```

### <a name="bind-a-reloadaction"></a><span data-ttu-id="034fe-186">Enlazar reloadAction</span><span class="sxs-lookup"><span data-stu-id="034fe-186">Bind a reloadAction</span></span>

<span data-ttu-id="034fe-187">Enlazar `reloadAction` a un cuadro de diálogo lo registrará en el cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="034fe-187">Binding a `reloadAction` to a dialog will register it to the dialog.</span></span> <span data-ttu-id="034fe-188">Enlazar esta acción a un cuadro de diálogo hará que el cuadro de diálogo se reinicie cuando se desencadene la acción.</span><span class="sxs-lookup"><span data-stu-id="034fe-188">Binding this action to a dialog causes the dialog to restart when the action is triggered.</span></span> <span data-ttu-id="034fe-189">Desencadenar esta acción es similar a llamar al método [replaceDialog](https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.session.html#replacedialog).</span><span class="sxs-lookup"><span data-stu-id="034fe-189">Triggering this action is similar to calling the [replaceDialog](https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.session.html#replacedialog) method.</span></span> <span data-ttu-id="034fe-190">Esto es útil para implementar la lógica para controlar las expresiones del usuario, como "empezar de nuevo" o para crear [bucles](bot-builder-nodejs-dialog-replace.md#repeat-an-action).</span><span class="sxs-lookup"><span data-stu-id="034fe-190">This is useful for implementing logic to handle user utterances like "start over" or to create [loops](bot-builder-nodejs-dialog-replace.md#repeat-an-action).</span></span>

<span data-ttu-id="034fe-191">El fragmento de código siguiente muestra cómo enlazar [reloadAction][reloadAction] a un cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="034fe-191">The following code snippet shows how to bind a [reloadAction][reloadAction] to a dialog.</span></span>

```javascript
// Order dinner.
bot.dialog('orderDinner', [
    //...waterfall steps...
])
// Once triggered, will restart the dialog.
.reloadAction('startOver', 'Ok, starting over.', {
    matches: /^start over$/i
});
```

<span data-ttu-id="034fe-192">En casos donde deba pasar argumentos adicionales en el cuadro de diálogo recargado, puede agregar una opción [`dialogArgs`](https://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.idialogactionoptions#dialogargs) a la acción.</span><span class="sxs-lookup"><span data-stu-id="034fe-192">In cases where you need to pass additional arguments into the reloaded dialog, you can add a [`dialogArgs`](https://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.idialogactionoptions#dialogargs) option to the action.</span></span> <span data-ttu-id="034fe-193">Esta opción se pasa al parámetro `args`.</span><span class="sxs-lookup"><span data-stu-id="034fe-193">This option is passed into the `args` parameter.</span></span> <span data-ttu-id="034fe-194">Si se reescribe el código de ejemplo anterior para recibir un argumento en una acción de recargar, tendrá un aspecto similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="034fe-194">Rewriting the sample code above to receive an argument on a reload action will look something like this:</span></span>

```javascript
// Order dinner.
bot.dialog('orderDinner', [
    function(session, args, next){
        if(args && args.isReloaded){
            // Reload action was triggered.
        }

        session.send("Lets order some dinner!");
        builder.Prompts.choice(session, "Dinner menu:", dinnerMenu);
    }
    //...other waterfall steps...
])
// Once triggered, will restart the dialog.
.reloadAction('startOver', 'Ok, starting over.', {
    matches: /^start over$/i,
    dialogArgs: {
        isReloaded: true;
    }
});
```

### <a name="bind-a-cancelaction"></a><span data-ttu-id="034fe-195">Enlazar cancelAction</span><span class="sxs-lookup"><span data-stu-id="034fe-195">Bind a cancelAction</span></span>

<span data-ttu-id="034fe-196">Enlazar `cancelAction` lo registrará en el cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="034fe-196">Binding a `cancelAction` will register it to the dialog.</span></span> <span data-ttu-id="034fe-197">Una vez desencadenada, esta acción finalizará repentinamente el cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="034fe-197">Once triggered, this action will abruptly end the dialog.</span></span> <span data-ttu-id="034fe-198">Una vez que finaliza el diálogo, el cuadro de diálogo primario se reanuda con el código que indica `canceled`.</span><span class="sxs-lookup"><span data-stu-id="034fe-198">Once the dialog ends, the parent dialog will resume with a resumed code indicating that it was `canceled`.</span></span> <span data-ttu-id="034fe-199">Esta acción le permite controlar las expresiones "no importa" o "cancelar".</span><span class="sxs-lookup"><span data-stu-id="034fe-199">This action allows you to handle utterances such as "nevermind" or "cancel."</span></span> <span data-ttu-id="034fe-200">Si en su lugar necesita cancelar mediante programación un cuadro de diálogo, consulte [Cancel a dialog](bot-builder-nodejs-dialog-replace.md#cancel-a-dialog) (Cancelar un cuadro de diálogo).</span><span class="sxs-lookup"><span data-stu-id="034fe-200">If you need to programmatically cancel a dialog instead, see [Cancel a dialog](bot-builder-nodejs-dialog-replace.md#cancel-a-dialog).</span></span> <span data-ttu-id="034fe-201">Para obtener más información sobre *código reanudado*, consulte [Prompt results](bot-builder-nodejs-dialog-prompt.md#prompt-results) (Solicitar resultados).</span><span class="sxs-lookup"><span data-stu-id="034fe-201">For more information on *resumed code*, see [Prompt results](bot-builder-nodejs-dialog-prompt.md#prompt-results).</span></span> 

<span data-ttu-id="034fe-202">El fragmento de código siguiente muestra cómo enlazar [cancelAction][cancelAction] a un cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="034fe-202">The following code snippet shows how to bind a [cancelAction][cancelAction] to a dialog.</span></span>

```javascript
// Order dinner.
bot.dialog('orderDinner', [
    //...waterfall steps...
])
//Once triggered, will end the dialog.
.cancelAction('cancelAction', 'Ok, cancel order.', {
    matches: /^nevermind$|^cancel$|^cancel.*order/i
});
```

### <a name="bind-an-endconversationaction"></a><span data-ttu-id="034fe-203">Enlazar endConversationAction</span><span class="sxs-lookup"><span data-stu-id="034fe-203">Bind an endConversationAction</span></span>

<span data-ttu-id="034fe-204">Enlazar `endConversationAction` lo registrará en el cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="034fe-204">Binding an `endConversationAction` will register it to the dialog.</span></span> <span data-ttu-id="034fe-205">Una vez desencadenada, esta acción finaliza la conversación con el usuario.</span><span class="sxs-lookup"><span data-stu-id="034fe-205">Once triggered, this action ends the conversation with the user.</span></span> <span data-ttu-id="034fe-206">Desencadenar esta acción es similar a llamar al método [endConversation](https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.session.html#endconversation).</span><span class="sxs-lookup"><span data-stu-id="034fe-206">Triggering this action is similar to calling the [endConversation](https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.session.html#endconversation) method.</span></span> <span data-ttu-id="034fe-207">Una vez que finaliza una conversación, el Bot Builder SDK para Node.js borrará la pila del cuadro de diálogo y los datos de estado persistente.</span><span class="sxs-lookup"><span data-stu-id="034fe-207">Once a conversation ends, the Bot Builder SDK for Node.js will clear the dialog stack and persisted state data.</span></span> <span data-ttu-id="034fe-208">Para obtener más información sobre los datos de estado persistente, consulte [Manage state data](bot-builder-nodejs-state.md) (Administrar datos de estado).</span><span class="sxs-lookup"><span data-stu-id="034fe-208">For more information on persisted state data, see [Manage state data](bot-builder-nodejs-state.md).</span></span>

<span data-ttu-id="034fe-209">El fragmento de código siguiente muestra cómo enlazar [endConversationAction][endConversationAction] a un cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="034fe-209">The following code snippet shows how to bind an [endConversationAction][endConversationAction] to a dialog.</span></span>

```javascript
// Order dinner.
bot.dialog('orderDinner', [
    //...waterfall steps...
])
// Once triggered, will end the conversation.
.endConversationAction('endConversationAction', 'Ok, goodbye!', {
    matches: /^goodbye$/i
});
```

## <a name="confirm-interruptions"></a><span data-ttu-id="034fe-210">Confirmar las interrupciones</span><span class="sxs-lookup"><span data-stu-id="034fe-210">Confirm interruptions</span></span>

<span data-ttu-id="034fe-211">La mayoría, si no todas estas acciones interrumpen el flujo normal de una conversación.</span><span class="sxs-lookup"><span data-stu-id="034fe-211">Most, if not, all of these actions interrupt the normal flow of a conversation.</span></span> <span data-ttu-id="034fe-212">Muchas son perjudiciales y deben controlarse con cuidado.</span><span class="sxs-lookup"><span data-stu-id="034fe-212">Many are disruptive and should be handle with care.</span></span> <span data-ttu-id="034fe-213">Por ejemplo, `triggerAction`, `cancelAction` o `endConversationAction` borrarán la pila del cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="034fe-213">For example, the `triggerAction`, `cancelAction`, or the `endConversationAction` will clear the dialog stack.</span></span> <span data-ttu-id="034fe-214">Si el usuario cometió el error de desencadenar una de estas acciones, tendrá que volver a empezar la tarea.</span><span class="sxs-lookup"><span data-stu-id="034fe-214">If the user made the mistake of triggering either of these actions, they will have to start the task over again.</span></span> <span data-ttu-id="034fe-215">Para asegurarse de que el usuario en verdad tuvo la intención de desencadenar estas acciones, puede agregar una opción `confirmPrompt` a estas acciones.</span><span class="sxs-lookup"><span data-stu-id="034fe-215">To make sure the user really intended to trigger these actions, you can add a `confirmPrompt` option to these actions.</span></span> <span data-ttu-id="034fe-216">`confirmPrompt` preguntará si el usuario está seguro de cancelar o finalizar la tarea actual.</span><span class="sxs-lookup"><span data-stu-id="034fe-216">The `confirmPrompt` will ask if the user is sure about canceling or ending the current task.</span></span> <span data-ttu-id="034fe-217">Permite al usuario cambiar de opinión y continuar el proceso.</span><span class="sxs-lookup"><span data-stu-id="034fe-217">It allows the user to change their minds and continue the process.</span></span>

<span data-ttu-id="034fe-218">El fragmento de código siguiente se muestra [cancelAction][cancelAction] con [confirmPrompt](http://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.itriggeractionoptions#confirmprompt) para asegurarse de que el usuario realmente desea cancelar el proceso de pedido.</span><span class="sxs-lookup"><span data-stu-id="034fe-218">The code snippet below shows a [cancelAction][cancelAction] with a [confirmPrompt](http://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.itriggeractionoptions#confirmprompt) to make sure the user really wants to cancel the order process.</span></span>

```javascript
// Order dinner.
bot.dialog('orderDinner', [
    //...waterfall steps...
])
// Confirm before triggering the action.
// Once triggered, will end the dialog. 
.cancelAction('cancelAction', 'Ok, cancel order.', {
    matches: /^nevermind$|^cancel$|^cancel.*order/i,
    confirmPrompt: "Are you sure?"
});
```

<span data-ttu-id="034fe-219">Una vez que se desencadena esta acción, le preguntará al usuario "¿Estás seguro?".</span><span class="sxs-lookup"><span data-stu-id="034fe-219">Once this action is triggered, it will ask the user "Are you sure?"</span></span> <span data-ttu-id="034fe-220">El usuario tendrá que responder "Sí" para seguir con la acción o "No" para cancelar la acción y continuar donde estaba.</span><span class="sxs-lookup"><span data-stu-id="034fe-220">The user will have to answer "Yes" to go through with the action or "No" to cancel the action and continue where they were.</span></span>

## <a name="next-steps"></a><span data-ttu-id="034fe-221">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="034fe-221">Next steps</span></span>

<span data-ttu-id="034fe-222">Las **acciones** le permiten prever las solicitudes del usuario y permiten al bot controlar correctamente esas solicitudes.</span><span class="sxs-lookup"><span data-stu-id="034fe-222">**Actions** allow you to anticipate user requests and allow the bot to handle those requests gracefully.</span></span> <span data-ttu-id="034fe-223">Muchas de estas acciones son perjudiciales para el flujo de la conversación actual.</span><span class="sxs-lookup"><span data-stu-id="034fe-223">Many of these actions are disruptive to the current conversation flow.</span></span> <span data-ttu-id="034fe-224">Si desea permitir que los usuarios tengan la capacidad de salir y reanudar un flujo de conversación, debe guardar el estado del usuario antes de salir. Echemos un vistazo más de cerca a cómo guardar el estado del usuario y administrar datos de estado.</span><span class="sxs-lookup"><span data-stu-id="034fe-224">If you want to allow users the ability to switch out and resume a conversation flow, you need to save the user state before switching out. Let's take a closer look at how to save user state and manage state data.</span></span>

> [!div class="nextstepaction"]
> [Administración de datos de estado](bot-builder-nodejs-state.md)


[triggerAction]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.dialog.html#triggeraction

[cancelAction]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.dialog.html#cancelaction

[reloadAction]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.dialog.html#reloadaction

[beginDialogAction]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.dialog.html#begindialogaction

[endConversationAction]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.dialog.html#endconversationaction

[RecognizeIntent]: bot-builder-nodejs-recognize-intent-messages.md

[CardAction]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.cardaction#dialogaction
