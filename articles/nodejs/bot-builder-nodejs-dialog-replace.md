---
title: Reemplazar los cuadros de diálogo | Microsoft Docs
description: Aprenda a reemplazar los cuadros de diálogo para volver a solicitar la entrada y administrar el flujo de la conversación con el SDK de Bot Builder para Node.js.
author: v-ducvo
ms.author: v-ducvo
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: d1a2eca2ebdfccf6dacdf773c48c8b41444559ca
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305028"
---
# <a name="replace-dialogs"></a><span data-ttu-id="68845-103">Reemplazar cuadros de diálogo</span><span class="sxs-lookup"><span data-stu-id="68845-103">Replace dialogs</span></span>

<span data-ttu-id="68845-104">La capacidad de reemplazar un cuadro de diálogo puede ser útil cuando necesita validar la entrada del usuario o repetir una acción durante el transcurso de una conversación.</span><span class="sxs-lookup"><span data-stu-id="68845-104">The ability to replace a dialog can be useful when you need to validate user input or repeat an action during the course of a conversation.</span></span> <span data-ttu-id="68845-105">Con el SDK de Bot Builder para Node.js, puede reemplazar un diálogo usando el método [`session.replaceDialog`](http://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.session#replacedialog).</span><span class="sxs-lookup"><span data-stu-id="68845-105">With the Bot Builder SDK for Node.js, you can replace a dialog by using the [`session.replaceDialog`](http://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.session#replacedialog) method.</span></span> <span data-ttu-id="68845-106">Este método le permite finalizar el diálogo actual y reemplazarlo con un nuevo cuadro de diálogo sin tener que volver a la persona que llama.</span><span class="sxs-lookup"><span data-stu-id="68845-106">This method enables you to end the current dialog and replace it with a new dialog without returning to the caller.</span></span> 

## <a name="create-custom-prompts-to-validate-input"></a><span data-ttu-id="68845-107">Crear mensajes personalizados para validar la entrada</span><span class="sxs-lookup"><span data-stu-id="68845-107">Create custom prompts to validate input</span></span>

<span data-ttu-id="68845-108">El SDK de Bot Builder para Node.js incluye la validación de entradas para algunos tipos de [solicitudes](bot-builder-nodejs-dialog-prompt.md) como `Prompts.time` y `Prompts.choice`.</span><span class="sxs-lookup"><span data-stu-id="68845-108">The Bot Builder SDK for Node.js includes input validation for some types of [prompts](bot-builder-nodejs-dialog-prompt.md) such as `Prompts.time` and `Prompts.choice`.</span></span> <span data-ttu-id="68845-109">Para validar la entrada de texto que recibe en respuesta a `Prompts.text`, debe crear su propia lógica de validación y solicitudes personalizadas.</span><span class="sxs-lookup"><span data-stu-id="68845-109">To validate text input that you receive in response to `Prompts.text`, you must create your own validation logic and custom prompts.</span></span> 

<span data-ttu-id="68845-110">Es posible que quiera validar una entrada si esta debe cumplir con un determinado valor, patrón, rango o criterio que usted mismo haya definido.</span><span class="sxs-lookup"><span data-stu-id="68845-110">You may want to validate an input if the input must comply with a certain value, pattern, range, or criteria that you define.</span></span> <span data-ttu-id="68845-111">Si una entrada no puede hacer la validación, el bot puede solicitar al usuario esa información nuevamente mediante el método `session.replaceDialog`.</span><span class="sxs-lookup"><span data-stu-id="68845-111">If an input fails validation, the bot can prompt the user for that information again by using the `session.replaceDialog` method.</span></span>

<span data-ttu-id="68845-112">En el siguiente ejemplo de código, se muestra cómo crear un aviso personalizado para validar la entrada del usuario para un número de teléfono.</span><span class="sxs-lookup"><span data-stu-id="68845-112">The following code sample shows how to create a custom prompt to validate user input for a phone number.</span></span>

```javascript
// This dialog prompts the user for a phone number. 
// It will re-prompt the user if the input does not match a pattern for phone number.
bot.dialog('phonePrompt', [
    function (session, args) {
        if (args && args.reprompt) {
            builder.Prompts.text(session, "Enter the number using a format of either: '(555) 123-4567' or '555-123-4567' or '5551234567'")
        } else {
            builder.Prompts.text(session, "What's your phone number?");
        }
    },
    function (session, results) {
        var matched = results.response.match(/\d+/g);
        var number = matched ? matched.join('') : '';
        if (number.length == 10 || number.length == 11) {
            session.userData.phoneNumber = number; // Save the number.
            session.endDialogWithResult({ response: number });
        } else {
            // Repeat the dialog
            session.replaceDialog('phonePrompt', { reprompt: true });
        }
    }
]);
```

<span data-ttu-id="68845-113">En este ejemplo, inicialmente se solicita al usuario que proporcione su número de teléfono.</span><span class="sxs-lookup"><span data-stu-id="68845-113">In this example, the user is initially prompted to provide their phone number.</span></span> <span data-ttu-id="68845-114">La lógica de validación utiliza una expresión regular que coincide con un rango de dígitos de la entrada del usuario.</span><span class="sxs-lookup"><span data-stu-id="68845-114">The validation logic uses a regular expression that matches a range of digits from the user input.</span></span> <span data-ttu-id="68845-115">Si la entrada contiene 10 u 11 dígitos, ese número se devuelve en la respuesta.</span><span class="sxs-lookup"><span data-stu-id="68845-115">If the input contains 10 or 11 digits, then that number is returned in the response.</span></span> <span data-ttu-id="68845-116">De lo contrario, el método `session.replaceDialog` se ejecuta para repetir el cuadro de diálogo `phonePrompt`, que se encargará de solicitar al usuario que realice nuevamente la entrada, esta vez proporcionando información más específica con respecto al formato que debe tener la entrada que se espera.</span><span class="sxs-lookup"><span data-stu-id="68845-116">Otherwise, the `session.replaceDialog` method is executed to repeat the `phonePrompt` dialog, which prompts the user for input again, this time providing more specific guidance regarding the format of input that is expected.</span></span>

<span data-ttu-id="68845-117">Cuando llame al método `session.replaceDialog`, especifique el nombre del cuadro de diálogo a repetir y una lista de argumentos.</span><span class="sxs-lookup"><span data-stu-id="68845-117">When you call the `session.replaceDialog` method, you specify the name of the dialog to repeat and an arguments list.</span></span> <span data-ttu-id="68845-118">En este ejemplo, la lista de argumentos contiene `{ reprompt: true }`, lo que permite que el bot proporcione mensajes de solicitud diferentes dependiendo de si se trata de un aviso inicial o una repetición de la solicitud, pero puede especificar los argumentos que el bot necesite.</span><span class="sxs-lookup"><span data-stu-id="68845-118">In this example, the arguments list contains `{ reprompt: true }`, which enables the bot to provide different prompt messages depending on whether it is an initial prompt or a reprompt, but you can specify whatever arguments your bot may require.</span></span> 

## <a name="repeat-an-action"></a><span data-ttu-id="68845-119">Repetir una acción</span><span class="sxs-lookup"><span data-stu-id="68845-119">Repeat an action</span></span>

<span data-ttu-id="68845-120">Puede haber ocasiones en el curso de una conversación en las que desee repetir un diálogo para permitir al usuario completar una determinada acción varias veces.</span><span class="sxs-lookup"><span data-stu-id="68845-120">There may be times in the course of a conversation where you want to repeat a dialog to enable the user to complete a certain action multiple times.</span></span> <span data-ttu-id="68845-121">Por ejemplo, si el bot ofrece una variedad de servicios, inicialmente puede mostrar el menú de servicios, guiar al usuario a través del proceso de solicitud de un servicio y, a continuación, volver a mostrar el menú de servicios, lo que permite al usuario solicitar otro servicio.</span><span class="sxs-lookup"><span data-stu-id="68845-121">For example, if your bot offers a variety of services, you might initially display the menu of services, walk the user through the process of requesting a service, and then display the menu of services again, thereby enabling the user to request another service.</span></span> <span data-ttu-id="68845-122">Para lograr esto, puede usar el método [`session.replaceDialog`](https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.session#replacedialog) para volver a mostrar el menú de servicios, en lugar de finalizar la conversación con el método ['session.endConversation\`](https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.session#endconversation).</span><span class="sxs-lookup"><span data-stu-id="68845-122">To achieve this, you can use the [`session.replaceDialog`](https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.session#replacedialog) method to display the menu of services again, rather than ending the conversation with the ['session.endConversation\`](https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.session#endconversation) method.</span></span> 

<span data-ttu-id="68845-123">En el siguiente ejemplo se muestra cómo usar el método `session.replaceDialog` para implementar este tipo de escenario.</span><span class="sxs-lookup"><span data-stu-id="68845-123">The following example shows how to use the `session.replaceDialog` method to implement this type of scenario.</span></span> <span data-ttu-id="68845-124">En primer lugar, se define el menú de servicios:</span><span class="sxs-lookup"><span data-stu-id="68845-124">First, the menu of services is defined:</span></span>

```javascript
// Main menu
var menuItems = { 
    "Order dinner": {
        item: "orderDinner"
    },
    "Dinner reservation": {
        item: "dinnerReservation"
    },
    "Schedule shuttle": {
        item: "scheduleShuttle"
    },
    "Request wake-up call": {
        item: "wakeupCall"
    },
}
```

<span data-ttu-id="68845-125">El diálogo predeterminado invoca al diálogo `mainMenu`, por lo que el menú se presentará al usuario al comienzo de la conversación.</span><span class="sxs-lookup"><span data-stu-id="68845-125">The `mainMenu` dialog is invoked by the default dialog, so the menu will be presented to the user at the beginning of the conversation.</span></span> <span data-ttu-id="68845-126">Además, se adjunta un [`triggerAction`](https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.dialog.html#triggeraction) al diálogo `mainMenu` para que el menú también se muestre siempre que la entrada del usuario sea "menú principal".</span><span class="sxs-lookup"><span data-stu-id="68845-126">Additionally, a [`triggerAction`](https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.dialog.html#triggeraction) is attached to the `mainMenu` dialog so that the menu will also be presented any time the user input is "main menu".</span></span> <span data-ttu-id="68845-127">Cuando se muestra el menú al usuario y este selecciona una opción, se invoca el diálogo que corresponde a la selección del usuario mediante el método `session.beginDialog`.</span><span class="sxs-lookup"><span data-stu-id="68845-127">When the user is presented with the menu and selects an option, the dialog that corresponds to the user's selection is invoked by using the `session.beginDialog` method.</span></span>

```javascript
var inMemoryStorage = new builder.MemoryBotStorage();

// This is a reservation bot that has a menu of offerings.
var bot = new builder.UniversalBot(connector, [
    function(session){
        session.send("Welcome to Contoso Hotel and Resort.");
        session.beginDialog("mainMenu");
    }
]).set('storage', inMemoryStorage); // Register in-memory storage 

// Display the main menu and start a new request depending on user input.
bot.dialog("mainMenu", [
    function(session){
        builder.Prompts.choice(session, "Main Menu:", menuItems);
    },
    function(session, results){
        if(results.response){
            session.beginDialog(menuItems[results.response.entity].item);
        }
    }
])
.triggerAction({
    // The user can request this at any time.
    // Once triggered, it clears the stack and prompts the main menu again.
    matches: /^main menu$/i,
    confirmPrompt: "This will cancel your request. Are you sure?"
});
```

<span data-ttu-id="68845-128">En este ejemplo, si el usuario elige la opción 1 para especificar que la cena se entregue en su habitación, se invocará el cuadro de diálogo `orderDinner` y se guiará al usuario a través del proceso para pedir la cena.</span><span class="sxs-lookup"><span data-stu-id="68845-128">In this example, if the user chooses option 1 to order dinner to be delivered to their room, the `orderDinner` dialog will be invoked and will walk the user through the process of ordering dinner.</span></span> <span data-ttu-id="68845-129">Al final del proceso, el bot confirmará el orden y mostrará el menú principal nuevamente mediante el método `session.replaceDialog`.</span><span class="sxs-lookup"><span data-stu-id="68845-129">At the end of the process, the bot will confirm the order and display the main menu again by using the `session.replaceDialog` method.</span></span>

```javascript
// Menu: "Order dinner"
// This dialog allows user to order dinner to be delivered to their hotel room.
bot.dialog('orderDinner', [
    function(session){
        session.send("Lets order some dinner!");
        builder.Prompts.choice(session, "Dinner menu:", dinnerMenu);
    },
    function (session, results) {
        if (results.response) {
            var order = dinnerMenu[results.response.entity];
            var msg = `You ordered: %(Description)s for a total of $${order.Price}.`;
            session.dialogData.order = order;
            session.send(msg);
            builder.Prompts.text(session, "What is your room number?");
        } 
    },
    function(session, results){
        if(results.response){
            session.dialogData.room = results.response;
            var msg = `Thank you. Your order will be delivered to room #${results.response}.`;
            session.send(msg);
            session.replaceDialog("mainMenu"); // Display the menu again.
        }
    }
])
.reloadAction(
    "restartOrderDinner", "Ok. Let's start over.",
    {
        matches: /^start over$/i
    }
)
.cancelAction(
    "cancelOrder", "Type 'Main Menu' to continue.", 
    {
        matches: /^cancel$/i,
        confirmPrompt: "This will cancel your order. Are you sure?"
    }
);
```

<span data-ttu-id="68845-130">Dos desencadenadores se adjuntan al cuadro de diálogo `orderDinner` para permitir al usuario "comenzar de nuevo" o "cancelar" el pedido en cualquier momento durante el proceso.</span><span class="sxs-lookup"><span data-stu-id="68845-130">Two triggers are attached to the `orderDinner` dialog to enable the user to "start over" or "cancel" the order at any time during the ordering process.</span></span> 

<span data-ttu-id="68845-131">El primer desencadenador es [`reloadAction`](https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.dialog.html#reloadaction), el cual permite al usuario comenzar nuevamente el proceso de pedido enviando la entrada "Comenzar de nuevo".</span><span class="sxs-lookup"><span data-stu-id="68845-131">The first trigger is [`reloadAction`](https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.dialog.html#reloadaction), which allows the user to start the order process over again by sending the input "start over".</span></span> <span data-ttu-id="68845-132">Cuando el desencadenador coincide con el enunciado "empezar de nuevo", `reloadAction` reinicia el diálogo desde el principio.</span><span class="sxs-lookup"><span data-stu-id="68845-132">When the trigger matches the utterance "start over", the `reloadAction` restarts the dialog from the beginning.</span></span> 

<span data-ttu-id="68845-133">El segundo desencadenador es [`cancelAction`](https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.dialog.html#cancelaction), el cual permite al usuario anular el proceso de pedido por completo al enviar la entrada "cancelar".</span><span class="sxs-lookup"><span data-stu-id="68845-133">The second trigger is [`cancelAction`](https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.dialog.html#cancelaction), which allows the user to abort the order process completely by sending the input "cancel".</span></span> <span data-ttu-id="68845-134">Este desencadenador no muestra de nuevo el menú principal automáticamente, sino que envía un mensaje que le indica al usuario qué hacer; por ejemplo, mostrando el mensaje "Escriba" 'Menú principal' para continuar".</span><span class="sxs-lookup"><span data-stu-id="68845-134">This trigger does not automatically display the main menu again, but instead sends a message that tells the user what to do next by saying "Type 'Main Menu' to continue."</span></span>

### <a name="dialog-loops"></a><span data-ttu-id="68845-135">Bucles del cuadro de diálogo </span><span class="sxs-lookup"><span data-stu-id="68845-135">Dialog loops</span></span>

<span data-ttu-id="68845-136">En el ejemplo anterior, el usuario solo puede seleccionar un elemento en función del pedido.</span><span class="sxs-lookup"><span data-stu-id="68845-136">In the example above, the user can only select one item per order.</span></span> <span data-ttu-id="68845-137">Es decir, si el usuario desea pedir dos elementos del menú, deberá completar todo el proceso de pedido para el primer artículo y, a continuación, repetir todo el proceso nuevamente para pedir el segundo artículo.</span><span class="sxs-lookup"><span data-stu-id="68845-137">That is, if the user wanted to order two items from the menu, they would have to complete the entire ordering process for the first item and then repeat the entire ordering process again for the second item.</span></span> 

<span data-ttu-id="68845-138">En el siguiente ejemplo se muestra cómo mejorar el bot anterior refactorizando el menú de la cena en un diálogo separado.</span><span class="sxs-lookup"><span data-stu-id="68845-138">The following example shows how to improve upon the previous bot by refactoring the dinner menu into a separate dialog.</span></span> <span data-ttu-id="68845-139">Al hacerlo, el bot puede mostrar el menú de la cena en un bucle y, por lo tanto, permite al usuario seleccionar varios elementos en un solo pedido.</span><span class="sxs-lookup"><span data-stu-id="68845-139">Doing so enables the bot to repeat the dinner menu in a loop and therefore allows the user to select multiple items within a single order.</span></span>

<span data-ttu-id="68845-140">En primer lugar, agregue la opción "Comprar" al menú.</span><span class="sxs-lookup"><span data-stu-id="68845-140">First, add a "Check out" option to the menu.</span></span> <span data-ttu-id="68845-141">Esta opción permitirá al usuario salir del proceso de selección de artículos y continuar con el proceso de compra.</span><span class="sxs-lookup"><span data-stu-id="68845-141">This option will allow the user to exit the item selection process and continue with the check out process.</span></span>

```javascript
// The dinner menu
var dinnerMenu = { 
    //...other menu items...,
    "Check out": {
        Description: "Check out",
        Price: 0 // Order total. Updated as items are added to order.
    }
};
```

<span data-ttu-id="68845-142">A continuación, refactorice la solicitud de pedido en su propio cuadro de diálogo para que el bot pueda repetir el menú y permitir al usuario agregar varios elementos a su pedido.</span><span class="sxs-lookup"><span data-stu-id="68845-142">Next, refactor the order prompt into its own dialog so that the bot can repeat the menu and allow user to add multiple items to their order.</span></span>

```javascript
// Add dinner items to the list by repeating this dialog until the user says `check out`. 
bot.dialog("addDinnerItem", [
    function(session, args){
        if(args && args.reprompt){
            session.send("What else would you like to have for dinner tonight?");
        }
        else{
            // New order
            // Using the conversationData to store the orders
            session.conversationData.orders = new Array();
            session.conversationData.orders.push({ 
                Description: "Check out",
                Price: 0
            })
        }
        builder.Prompts.choice(session, "Dinner menu:", dinnerMenu);
    },
    function(session, results){
        if(results.response){
            if(results.response.entity.match(/^check out$/i)){
                session.endDialog("Checking out...");
            }
            else {
                var order = dinnerMenu[results.response.entity];
                session.conversationData.orders[0].Price += order.Price; // Add to total.
                var msg = `You ordered: ${order.Description} for a total of $${order.Price}.`;
                session.send(msg);
                session.conversationData.orders.push(order);
                session.replaceDialog("addDinnerItem", { reprompt: true }); // Repeat dinner menu
            }
        }
    }
])
.reloadAction(
    "restartOrderDinner", "Ok. Let's start over.",
    {
        matches: /^start over$/i
    }
);
```

<span data-ttu-id="68845-143">En este ejemplo, los pedidos se almacenan en un almacén de datos del bot cuyo ámbito se basa en la conversación actual, `session.conversationData.orders`.</span><span class="sxs-lookup"><span data-stu-id="68845-143">In this example, orders are stored in a bot data store that is scoped to the current conversation,  `session.conversationData.orders`.</span></span> <span data-ttu-id="68845-144">Cada vez que se hace un pedido, la variable se vuelve a iniciar con una nueva matriz, y cada vez que el usuario elige un artículo, el bot lo agrega a la matriz `orders` y añade el precio al total a pagar, que se almacena en la variable `Price` del proceso de compra.</span><span class="sxs-lookup"><span data-stu-id="68845-144">For every new order, the variable is re-initialized with a new array and for every item the user chooses, the bot adds that item to the `orders` array and adds the price to the total, which is stored in the checkout's `Price` variable.</span></span> <span data-ttu-id="68845-145">Cuando el usuario termina de seleccionar artículos en su pedido, puede decir "comprar" y continuar con el resto del proceso de pedido.</span><span class="sxs-lookup"><span data-stu-id="68845-145">When the user finishes selecting items for their order, they can say "check out" and then continue with the remainder of the order process.</span></span>

> [!NOTE]
> <span data-ttu-id="68845-146">Para obtener más información sobre el almacenamiento de datos del bot, consulte [Administrar datos de estado](bot-builder-nodejs-state.md).</span><span class="sxs-lookup"><span data-stu-id="68845-146">For more information about bot data storage, see [Manage state data](bot-builder-nodejs-state.md).</span></span> 

<span data-ttu-id="68845-147">Por último, actualice el segundo paso de cascada que se encuentra dentro del cuadro de diálogo `orderDinner`, para procesar el pedido con la confirmación.</span><span class="sxs-lookup"><span data-stu-id="68845-147">Finally, update the second step of the waterfall within the `orderDinner` dialog to process the order with confirmation.</span></span> 

```javascript
// Menu: "Order dinner"
// This dialog allows user to order dinner and have it delivered to their room.
bot.dialog('orderDinner', [
    function(session){
        session.send("Lets order some dinner!");
        session.beginDialog("addDinnerItem");
    },
    function (session, results) {
        if (results.response) {
            // Display itemize order with price total.
            for(var i = 1; i < session.conversationData.orders.length; i++){
                session.send(`You ordered: ${session.conversationData.orders[i].Description} for a total of $${session.conversationData.orders[i].Price}.`);
            }
            session.send(`Your total is: $${session.conversationData.orders[0].Price}`);

            // Continue with the check out process.
            builder.Prompts.text(session, "What is your room number?");
        } 
    },
    function(session, results){
        if(results.response){
            session.dialogData.room = results.response;
            var msg = `Thank you. Your order will be delivered to room #${results.response}`;
            session.send(msg);
            session.replaceDialog("mainMenu");
        }
    }
])
//...attached triggers...
;
```

## <a name="cancel-a-dialog"></a><span data-ttu-id="68845-148">Cancelar un cuadro de diálogo</span><span class="sxs-lookup"><span data-stu-id="68845-148">Cancel a dialog</span></span>

<span data-ttu-id="68845-149">Si bien el método `session.replaceDialog` se puede usar para reemplazar el cuadro de diálogo *actual* con uno nuevo, no se puede usar para reemplazar un cuadro de diálogo ubicado más abajo en la pila de diálogos.</span><span class="sxs-lookup"><span data-stu-id="68845-149">While the `session.replaceDialog` method can be used to replace the *current* dialog with a new one, it cannot be used to replace a dialog that is located further down the dialog stack.</span></span> <span data-ttu-id="68845-150">Para reemplazar un diálogo dentro de la pila de diálogos que no sea el diálogo *actual*, use el método [`session.cancelDialog`](http://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.session#canceldialog) en su lugar.</span><span class="sxs-lookup"><span data-stu-id="68845-150">To replace a dialog within the dialog stack that is not the *current* dialog, use the [`session.cancelDialog`](http://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.session#canceldialog) method instead.</span></span> 

<span data-ttu-id="68845-151">El método `session.cancelDialog` se puede usar para finalizar un diálogo independientemente de dónde esté en la pila de diálogos y, opcionalmente, se puede invocar un nuevo diálogo en su lugar.</span><span class="sxs-lookup"><span data-stu-id="68845-151">The `session.cancelDialog` method can be used to end a dialog regardless of where it exists in the dialog stack and optionally invoke a new dialog in its place.</span></span> <span data-ttu-id="68845-152">Para llamar al método `session.cancelDialog`, especifique el id. del diálogo para cancelarlo y, opcionalmente, especifique el id. del diálogo que se invocará en su lugar.</span><span class="sxs-lookup"><span data-stu-id="68845-152">To call the `session.cancelDialog` method, specify the ID of the dialog to cancel and optionally, specify the ID of the dialog to invoke in its place.</span></span> <span data-ttu-id="68845-153">Por ejemplo, este fragmento de código cancela el diálogo `orderDinner` y lo reemplaza con el cuadro de diálogo `mainMenu`:</span><span class="sxs-lookup"><span data-stu-id="68845-153">For example, this code snippet cancels the `orderDinner` dialog and replaces it with the `mainMenu` dialog:</span></span>

```javascript
session.cancelDialog('orderDinner', 'mainMenu'); 
```

<span data-ttu-id="68845-154">Cuando se llama al método `session.cancelDialog`, la pila de diálogos se examinará hacia atrás y se cancelará el primer resultado que coincida con ese diálogo; gracias a esto, ese diálogo y sus diálogos secundarios (si los hay) se eliminarán de la pila de diálogos.</span><span class="sxs-lookup"><span data-stu-id="68845-154">When the `session.cancelDialog` method is called, the dialog stack will be searched backwards and the first occurrence of that dialog will be canceled, causing that dialog and its child dialogs (if any) to be removed from the dialog stack.</span></span> <span data-ttu-id="68845-155">El control será devuelto al diálogo de llamadas, el cual puede comprobar si hay un código [`results.resumed`](http://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.ipromptresult.html#resumed) igual a [`ResumeReason.notCompleted`](http://docs.botframework.com/en-us/node/builder/chat-reference/enums/_botbuilder_d_.resumereason.html#notcompleted) para detectar la cancelación.</span><span class="sxs-lookup"><span data-stu-id="68845-155">Control will then be returned to the calling dialog, which can check for a [`results.resumed`](http://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.ipromptresult.html#resumed) code equal to [`ResumeReason.notCompleted`](http://docs.botframework.com/en-us/node/builder/chat-reference/enums/_botbuilder_d_.resumereason.html#notcompleted) to detect the cancellation.</span></span>

<span data-ttu-id="68845-156">Como alternativa a la opción de especificar el id. del diálogo para cancelarlo cuando se llame al método `session.cancelDialog`, puede especificar en su lugar el índice basado en cero del diálogo que se va a cancelar, donde el índice representa la posición del diálogo en la pila de diálogos.</span><span class="sxs-lookup"><span data-stu-id="68845-156">As an alternative to specifying the ID of the dialog to cancel when you call the `session.cancelDialog` method, you can instead specify the zero-based index of the dialog to cancel, where the index represents the dialog's position in the dialog stack.</span></span> <span data-ttu-id="68845-157">Por ejemplo, el siguiente fragmento de código finaliza el diálogo activo (índice = 0) y comienza el diálogo `mainMenu` en su lugar.</span><span class="sxs-lookup"><span data-stu-id="68845-157">For example, the following code snippet terminates the currently active dialog (index = 0) and starts the `mainMenu` dialog in its place.</span></span> <span data-ttu-id="68845-158">El diálogo `mainMenu` se invoca en la posición 0 de la pila de diálogos, convirtiéndose así en el nuevo cuadro de diálogo predeterminado.</span><span class="sxs-lookup"><span data-stu-id="68845-158">The `mainMenu` dialog is invoked at position 0 of the dialog stack, thereby becoming the new default dialog.</span></span>

```javascript
session.cancelDialog(0, 'mainMenu');
```

<span data-ttu-id="68845-159">Le recomendamos que mire la muestra que se discutió anteriormente en los [bucles de diálogo ](#dialog-loops).</span><span class="sxs-lookup"><span data-stu-id="68845-159">Consider the sample that is discussed in [dialog loops](#dialog-loops) above.</span></span> <span data-ttu-id="68845-160">Cuando el usuario llega al menú de selección de elementos, ese diálogo (`addDinnerItem`) es el cuarto diálogo de la pila de diálogos: `[default dialog, mainMenu, orderDinner, addDinnerItem]`.</span><span class="sxs-lookup"><span data-stu-id="68845-160">When the user reaches the item selection menu, that dialog (`addDinnerItem`) is the fourth dialog in the dialog stack: `[default dialog, mainMenu, orderDinner, addDinnerItem]`.</span></span> <span data-ttu-id="68845-161">¿Cómo se puede permitir que el usuario cancele su pedido desde el cuadro de diálogo `addDinnerItem`?</span><span class="sxs-lookup"><span data-stu-id="68845-161">How can you enable the user to cancel their order from within the `addDinnerItem` dialog?</span></span> <span data-ttu-id="68845-162">Si adjunta un desencadenador `cancelAction` al cuadro de diálogo `addDinnerItem`, solo llevará al usuario al cuadro de diálogo anterior (`orderDinner`), así que el usuario accederá nuevamente al cuadro de diálogo `addDinnerItem`.</span><span class="sxs-lookup"><span data-stu-id="68845-162">If you attach a `cancelAction` trigger to the `addDinnerItem` dialog, it will only return the user back to the previous dialog (`orderDinner`), which will send the user right back into the `addDinnerItem` dialog.</span></span>

<span data-ttu-id="68845-163">Aquí es donde el `session.cancelDialog` método resulta útil.</span><span class="sxs-lookup"><span data-stu-id="68845-163">This is where the `session.cancelDialog` method is useful.</span></span> <span data-ttu-id="68845-164">Comenzando con el [ejemplo de bucles de diálogo](#dialog-loops), agregue "Cancelar pedido" como una opción explícita dentro del menú de la cena.</span><span class="sxs-lookup"><span data-stu-id="68845-164">Starting with the [dialog loops example](#dialog-loops), add "Cancel order" as an explicit option within the dinner menu.</span></span>

```javascript
// The dinner menu
var dinnerMenu = { 
    //...other menu items...,
    "Check out": {
        Description: "Check out",
        Price: 0      // Order total. Updated as items are added to order.
    },
    "Cancel order": { // Cancel the order and back to Main Menu
        Description: "Cancel order",
        Price: 0
    }
};
```

<span data-ttu-id="68845-165">A continuación, actualice el cuadro de diálogo `addDinnerItem` para comprobar que la solicitud "Cancelar pedido" aparece.</span><span class="sxs-lookup"><span data-stu-id="68845-165">Then, update the `addDinnerItem` dialog to check for a "cancel order" request.</span></span> <span data-ttu-id="68845-166">Si se detecta la opción "Cancelar", utilice el método `session.cancelDialog` para cancelar el cuadro de diálogo predeterminado (es decir, el cuadro de diálogo en el índice 0 de la pila) e invoque el cuadro de diálogo `mainMenu` en su lugar.</span><span class="sxs-lookup"><span data-stu-id="68845-166">If "cancel" is detected, use the `session.cancelDialog` method to cancel the default dialog (i.e., the dialog at index 0 of the stack) and invoke the `mainMenu` dialog in its place.</span></span> 

```javascript
// Add dinner items to the list by repeating this dialog until the user says `check out`. 
bot.dialog("addDinnerItem", [
    //...waterfall steps...,
    // Last step
    function(session, results){
        if(results.response){
            if(results.response.entity.match(/^check out$/i)){
                session.endDialog("Checking out...");
            }
            else if(results.response.entity.match(/^cancel/i)){
                // Cancel the order and start "mainMenu" dialog.
                session.cancelDialog(0, "mainMenu");
            }
            else {
                //...add item to list and prompt again...
                session.replaceDialog("addDinnerItem", { reprompt: true }); // Repeat dinner menu.
            }
        }
    }
])
//...attached triggers...
;
```

<span data-ttu-id="68845-167">Al usar el método `session.cancelDialog` de esta manera, puede implementar cualquier flujo de conversación que requiera su bot.</span><span class="sxs-lookup"><span data-stu-id="68845-167">By using the `session.cancelDialog` method in this way, you can implement whatever conversation flow your bot requires.</span></span>

## <a name="next-steps"></a><span data-ttu-id="68845-168">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="68845-168">Next steps</span></span>

<span data-ttu-id="68845-169">Como puede ver, para reemplazar los diálogos en la pila, debe usar varios tipos de **acciones** para realizar esa tarea.</span><span class="sxs-lookup"><span data-stu-id="68845-169">As you can see, to replace dialogs on the stack, you use various types of **actions** to accomplish that task.</span></span> <span data-ttu-id="68845-170">Estas acciones le ofrecen una gran flexibilidad a la hora de gestionar el flujo de la conversación.</span><span class="sxs-lookup"><span data-stu-id="68845-170">Actions gives you great flexibilities in managing conversation flow.</span></span> <span data-ttu-id="68845-171">Echemos un vistazo más de cerca a las **acciones** y a cómo controlar mejor las acciones de los usuarios.</span><span class="sxs-lookup"><span data-stu-id="68845-171">Let's take a closer look at **actions** and how to better handle user actions.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="68845-172">Control de acciones del usuario</span><span class="sxs-lookup"><span data-stu-id="68845-172">Handle user actions</span></span>](bot-builder-nodejs-dialog-actions.md)
