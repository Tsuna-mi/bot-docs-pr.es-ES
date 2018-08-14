---
title: Control de la interrupción del usuario | Microsoft Docs
description: Obtenga información sobre cómo controlar el flujo de conversación directa y la interrupción del usuario.
keywords: interrumpir, interrupciones, cambio de tema, interrupción
author: v-ducvo
ms.author: v-ducvo
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 04/17/2018
ms.reviewer: ''
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: 651a7410893f7a66f5941121edc7b34055807ba7
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39304869"
---
# <a name="handle-user-interrupt"></a><span data-ttu-id="48bb1-104">Control de la interrupción del usuario</span><span class="sxs-lookup"><span data-stu-id="48bb1-104">Handle user interrupt</span></span>

[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]

<span data-ttu-id="48bb1-105">El control de interrupciones del usuario es un aspecto importante de un bot sólido.</span><span class="sxs-lookup"><span data-stu-id="48bb1-105">Handling user interrupt is an important aspect of a robust bot.</span></span> <span data-ttu-id="48bb1-106">Aunque puede pensar que los usuarios siguen el flujo de conversación definido paso a paso, es muy probable que cambien de parecer o formulen una pregunta en mitad del proceso en lugar de responder la pregunta.</span><span class="sxs-lookup"><span data-stu-id="48bb1-106">While you may think that your users will follow your defined conversation flow step by step, chances are good that they will change their minds or ask a question in the middle of the process instead of answering the question.</span></span> <span data-ttu-id="48bb1-107">En estas situaciones, ¿cómo controlaría su bot la entrada del usuario?</span><span class="sxs-lookup"><span data-stu-id="48bb1-107">In these situations, how would your bot handle the user's input?</span></span> <span data-ttu-id="48bb1-108">¿Como sería la experiencia del usuario?</span><span class="sxs-lookup"><span data-stu-id="48bb1-108">What would the user experience be like?</span></span> <span data-ttu-id="48bb1-109">¿Cómo mantendría los datos de estado del usuario?</span><span class="sxs-lookup"><span data-stu-id="48bb1-109">How would you maintain user state data?</span></span>

<span data-ttu-id="48bb1-110">No hay ninguna respuesta correcta a estas preguntas, ya que cada situación es única según el escenario para el que se haya diseñado el bot.</span><span class="sxs-lookup"><span data-stu-id="48bb1-110">There is no right answer to these questions as each situation is unique to the scenario your bot is designed to handle.</span></span> <span data-ttu-id="48bb1-111">En este tema, se exploran algunas formas comunes de controlar las interrupciones del usuario y se sugieren algunas formas para implementarlas en su bot.</span><span class="sxs-lookup"><span data-stu-id="48bb1-111">In this topic, we will explore some common ways to handle user interruptions and suggest some ways to implement them in your bot.</span></span>

## <a name="handle-expected-interruptions"></a><span data-ttu-id="48bb1-112">Controlar interrupciones esperadas</span><span class="sxs-lookup"><span data-stu-id="48bb1-112">Handle expected interruptions</span></span>

<span data-ttu-id="48bb1-113">Un flujo de conversación de procedimientos cuenta con un conjunto básico de pasos que quiere que el usuario siga y cualquier acción del usuario que no corresponda con esos pasos son posibles interrupciones.</span><span class="sxs-lookup"><span data-stu-id="48bb1-113">A procedural conversation flow has a core set of steps that you want to lead the user through, and any user actions that vary from those steps are potential interruptions.</span></span> <span data-ttu-id="48bb1-114">En un flujo normal, existen interrupciones que puede anticipar.</span><span class="sxs-lookup"><span data-stu-id="48bb1-114">In a normal flow, there are interruptions that you can anticipate.</span></span>

<span data-ttu-id="48bb1-115">**Reserva de una mesa**: en un bot para reservar una mesa, los pasos principales pueden ser pedir al usuario una fecha y hora, el número de personas y el nombre de reserva.</span><span class="sxs-lookup"><span data-stu-id="48bb1-115">**Table reservation** In a table reservation bot, the core steps may be to ask the user for a date and time, the size of the party, and the reservation name.</span></span> <span data-ttu-id="48bb1-116">En ese proceso, se pueden anticipar algunas interrupciones esperadas, como por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="48bb1-116">In that process, some expected interruptions you could anticipate may include:</span></span> 
 * <span data-ttu-id="48bb1-117">`cancel`: para salir del proceso.</span><span class="sxs-lookup"><span data-stu-id="48bb1-117">`cancel`: To exit the process.</span></span>
 * <span data-ttu-id="48bb1-118">`help`: para proporcionar más instrucciones acerca de este proceso.</span><span class="sxs-lookup"><span data-stu-id="48bb1-118">`help`: To provide additional guidance about this process.</span></span>
 * <span data-ttu-id="48bb1-119">`more info`: para dar consejos y sugerencias o proporcionar maneras alternativas para reservar una mesa (p. ej.: un número de teléfono o una dirección de correo electrónico de contacto).</span><span class="sxs-lookup"><span data-stu-id="48bb1-119">`more info`: To give hints and suggestions or provide alternative ways of reserving a table (e.g.: an email address or phone number to contact).</span></span>
 * <span data-ttu-id="48bb1-120">`show list of available tables`: si es una opción; muestra una lista de mesas disponibles para la fecha y hora indicada por el usuario.</span><span class="sxs-lookup"><span data-stu-id="48bb1-120">`show list of available tables`: If that is an option; show a list of tables available for the date and time the user wanted.</span></span>

<span data-ttu-id="48bb1-121">**Pedir una cena**: en un bot para pedir una cena, los pasos principales serían proporcionar una lista de los elementos del menú y permitir que el usuario agregue elementos al carro de la compra.</span><span class="sxs-lookup"><span data-stu-id="48bb1-121">**Order dinner** In an order dinner bot, the core steps would be to provide a list of menu items and allow the user to add items to their cart.</span></span> <span data-ttu-id="48bb1-122">En este proceso, se pueden anticipar algunas interrupciones esperadas, como por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="48bb1-122">In this process, some expected interruptions you could anticipate may include:</span></span> 
 * <span data-ttu-id="48bb1-123">`cancel`: para salir del proceso de pedido.</span><span class="sxs-lookup"><span data-stu-id="48bb1-123">`cancel`: To exit the ordering process.</span></span>
 * <span data-ttu-id="48bb1-124">`more info`: para proporcionar detalles nutricionales sobre cada elemento del menú.</span><span class="sxs-lookup"><span data-stu-id="48bb1-124">`more info`: To provide dietary detail about each menu item.</span></span>
 * <span data-ttu-id="48bb1-125">`help`: para proporcionar ayuda sobre cómo usar el sistema.</span><span class="sxs-lookup"><span data-stu-id="48bb1-125">`help`: To provide help on how to use the system.</span></span>
 * <span data-ttu-id="48bb1-126">`process order`: para procesar el pedido.</span><span class="sxs-lookup"><span data-stu-id="48bb1-126">`process order`: To process the order.</span></span>

<span data-ttu-id="48bb1-127">Puede proporcionarlas al usuario como una lista de **acciones sugeridas** o como una sugerencia de modo que el usuario al menos tenga en cuenta qué comandos puede enviar que el bot pueda entender.</span><span class="sxs-lookup"><span data-stu-id="48bb1-127">You could provide these to the user as a list of **suggested actions** or as a hint so the user is at least aware of what commands they can send that the bot would understand.</span></span>

<span data-ttu-id="48bb1-128">Por ejemplo, en el flujo del pedido de cena, se pueden proporcionar interrupciones esperadas junto con los elementos del menú.</span><span class="sxs-lookup"><span data-stu-id="48bb1-128">For example, in the order dinner flow, you can provide expected interruptions along with the menu items.</span></span> <span data-ttu-id="48bb1-129">En este caso, los elementos del menú se envían como una matriz de `choices`.</span><span class="sxs-lookup"><span data-stu-id="48bb1-129">In this case, the menu items are sent as an array of `choices`.</span></span>

# <a name="ctabcsharptab"></a>[<span data-ttu-id="48bb1-130">C#</span><span class="sxs-lookup"><span data-stu-id="48bb1-130">C#</span></span>](#tab/csharptab)

```cs
public class dinnerItem
{
    public string Description;
    public double Price;
}

public class dinnerMenu
{
    static public Dictionary<string, dinnerItem> dinnerChoices = new Dictionary<string, dinnerItem>
    {
        { "potato salad", new dinnerItem { Description="Potato Salad", Price=5.99 } },
        { "tuna sandwich", new dinnerItem { Description="Tuna Sandwich", Price=6.89 } },
        { "clam chowder", new dinnerItem { Description="Clam Chowder", Price=4.50 } }
    };

    static public string[] choices = new string[] {"Potato Salad", "Tuna Sandwich", "Clam Chowder", "more info", "Process order", "help", "Cancel"};
}
```

# <a name="javascripttabjstab"></a>[<span data-ttu-id="48bb1-131">JavaScript</span><span class="sxs-lookup"><span data-stu-id="48bb1-131">JavaScript</span></span>](#tab/jstab)

```javascript
var dinnerMenu = {
    choices: ["Potato Salad - $5.99", "Tuna Sandwich - $6.89", "Clam Chowder - $4.50", 
            "more info", "Process order", "Cancel"],
    "Potato Salad - $5.99": {
        Description: "Potato Salad",
        Price: 5.99
    },
    "Tuna Sandwich - $6.89": {
        Description: "Tuna Sandwich",
        Price: 6.89
    },
    "Clam Chowder - $4.50": {
        Description: "Clam Chowder",
        Price: 4.50
    }
}
```

---

<span data-ttu-id="48bb1-132">En la lógica del pedido, puede comprobarlos mediante la correspondencia de cadenas o expresiones regulares.</span><span class="sxs-lookup"><span data-stu-id="48bb1-132">In your ordering logic, you can check for them using string matching or regular expressions.</span></span>

# <a name="ctabcsharptab"></a>[<span data-ttu-id="48bb1-133">C#</span><span class="sxs-lookup"><span data-stu-id="48bb1-133">C#</span></span>](#tab/csharptab)

<span data-ttu-id="48bb1-134">En primer lugar, debemos definir una aplicación auxiliar para realizar un seguimiento de nuestros pedidos.</span><span class="sxs-lookup"><span data-stu-id="48bb1-134">First, we need to define a helper to keep track of our orders</span></span>

```cs
// Helper class for storing the order in the dictionary
public class Orders
{
    public double total;
    public string order;
    public bool processOrder;

    // Initialize order values
    public Orders()
    {
        total = 0;
        order = "";
        processOrder = false;
    }
}
```

<span data-ttu-id="48bb1-135">A continuación, agregue el cuadro de diálogo al bot.</span><span class="sxs-lookup"><span data-stu-id="48bb1-135">Then, add the dialog to your bot.</span></span>

```cs
dialogs.Add("orderPrompt", new WaterfallStep[]
{
    async (dc, args, next) =>
    {
        // Prompt the user
        await dc.Prompt("choicePrompt",
            "What would you like for dinner?",
            new ChoicePromptOptions
            {
                Choices = dinnerMenu.choices.Select( s => new Choice { Value = s }).ToList(),
                RetryPromptString = "I'm sorry, I didn't understand that. What would you " +
                    "like for dinner?"
            });
    },
    async(dc, args, next) =>
    {
        var convo = ConversationState<Dictionary<string,object>>.Get(dc.Context);

        // Get the user's choice from the previous prompt
        var response = (args["Value"] as FoundChoice).Value.ToLower();

        if(response == "process order")
        {
            try 
            {
                var order = convo["order"];

                await dc.Context.SendActivity("Order is on it's way!");
                
                // In production, you may want to store something more helpful, 
                // such as send order off to be made
                (order as Orders).processOrder = true;

                // Once it's submitted, clear the current order
                convo.Remove("order");
                await dc.End();
            }
            catch
            {
                await dc.Context.SendActivity("Your order is empty, please add your order choice");
                // Ask again
                await dc.Replace("orderPrompt");
            }
        }
        else if(response == "cancel" )
        {
            // Get rid of current order
            convo.Remove("order");
            await dc.Context.SendActivity("Your order has been canceled");
            await dc.End();
        }
        else if(response == "more info")
        {
            // Send more information about the options
            var msg = "More info: <br/>" +
                "Potato Salad: contains 330 calaries per serving. Cost: 5.99 <br/>"
                + "Tuna Sandwich: contains 700 calaries per serving. Cost: 6.89 <br/>"
                + "Clam Chowder: contains 650 calaries per serving. Cost: 4.50";
            await dc.Context.SendActivity(msg);

            // Ask again
            await dc.Replace("orderPrompt");
        }
        else if(response == "help")
        {
            // Provide help information
            await dc.Context.SendActivity("To make an order, add as many items to your cart as " +
                "you like then choose the \"Process order\" option to check out.");

            // Ask again
            await dc.Replace("orderPrompt");
        }
        else
        {
            // Unlikely to get past the prompt verification, but this will catch 
            // anything that isn't a valid menu choice
            if(!dinnerMenu.dinnerChoices.ContainsKey(response))
            {
                await dc.Context.SendActivity("Sorry, that is not a valid item. " +
                    "Please pick one from the menu.");
    
                // Ask again
                await dc.Replace("orderPrompt");
            }
            else {
                // Add the item to cart
                Orders currentOrder;

                // If there is a current order going, add to it. If not, start a new one
                try
                {
                    currentOrder = convo["order"] as Orders;
                }
                catch
                {
                    convo["order"] = new Orders();
                    currentOrder = convo["order"] as Orders;
                }

                // Add to the current order
                currentOrder.order += (dinnerMenu.dinnerChoices[$"{response}"].Description) + ", ";
                currentOrder.total += (double)dinnerMenu.dinnerChoices[$"{response}"].Price;

                // Save back to the conversation state
                convo["order"] = currentOrder;

                await dc.Context.SendActivity($"Added to cart. Current order: " +
                    $"{currentOrder.order} " +
                    $"<br/>Current total: ${currentOrder.total}");

                // Ask again to allow user to add more items or process order
                await dc.Replace("orderPrompt");
            }
        }
    }
});
```

# <a name="javascripttabjstab"></a>[<span data-ttu-id="48bb1-136">JavaScript</span><span class="sxs-lookup"><span data-stu-id="48bb1-136">JavaScript</span></span>](#tab/jstab)

```javascript
// Helper dialog to repeatedly prompt user for orders
dialogs.add('orderPrompt', [
    async function(dc){
        await dc.prompt('choicePrompt', "What would you like?", dinnerMenu.choices);
    },
    async function(dc, choice){
        if(choice.value.match(/process order/ig)){
            if(orderCart.orders.length > 0) {
                // Process the order
                dc.context.activity.conversation.dinnerOrder = orderCart;

                await dc.end();
            }
            else {
                await dc.context.sendActivity("Your cart was empty. Please add at least one item to the cart.");
                // Ask again
                await dc.replace('orderPrompt');
            }
        }
        else if(choice.value.match(/cancel/ig)){
            orderCart.clear(context);
            await dc.context.sendActivity("Your order has been canceled.");
            await dc.end(choice.value);
        }
        else if(choice.value.match(/more info/ig)){
            var msg = "More info: <br/>Potato Salad: contains 330 calaries per serving. <br/>"
                + "Tuna Sandwich: contains 700 calaries per serving. <br/>" 
                + "Clam Chowder: contains 650 calaries per serving."
            await dc.context.sendActivity(msg);
            
            // Ask again
            await dc.replace('orderPrompt');
        }
        else if(choice.value.match(/help/ig)){
            var msg = `Help: <br/>To make an order, add as many items to your cart as you like then choose the "Process order" option to check out.`
            await dc.context.sendActivity(msg);
            
            // Ask again
            await dc.replace('orderPrompt');
        }
        else {
            var choice = dinnerMenu[choice.value];

            // Only proceed if user chooses an item from the menu
            if(!choice){
                await dc.context.sendActivity("Sorry, that is not a valid item. Please pick one from the menu.");
                
                // Ask again
                await dc.replace('orderPrompt');
            }
            else {
                // Add the item to cart
                orderCart.orders.push(choice);
                orderCart.total += dinnerMenu[choice.value].Price;

                await dc.context.sendActivity(`Added to cart: ${choice.value}. <br/>Current total: $${orderCart.total}`);

                // Ask again
                await dc.replace('orderPrompt');
            }
        }
    }
]);
```

---

## <a name="handle-unexpected-interruptions"></a><span data-ttu-id="48bb1-137">Controlar interrupciones inesperadas</span><span class="sxs-lookup"><span data-stu-id="48bb1-137">Handle unexpected interruptions</span></span>

<span data-ttu-id="48bb1-138">Hay interrupciones que se encuentran fuera del ámbito de lo que su bot está diseñado para hacer.</span><span class="sxs-lookup"><span data-stu-id="48bb1-138">There are interruptions that are out of scope of what your bot is designed to do.</span></span>
<span data-ttu-id="48bb1-139">Aunque no se pueden prever todas las interrupciones, hay patrones de interrupción que puede programar para que su bot controle.</span><span class="sxs-lookup"><span data-stu-id="48bb1-139">While you cannot anticipate all interruptions, there are patterns of interruptions that you can program your bot to handle.</span></span>

### <a name="switching-topic-of-conversations"></a><span data-ttu-id="48bb1-140">Cambiar el tema de las conversaciones</span><span class="sxs-lookup"><span data-stu-id="48bb1-140">Switching topic of conversations</span></span>
<span data-ttu-id="48bb1-141">¿Qué ocurre si el usuario está en medio de una conversación y quiere cambiar a otra conversación?</span><span class="sxs-lookup"><span data-stu-id="48bb1-141">What if the user is in the middle of one conversation and wants to switch to another conversation?</span></span> <span data-ttu-id="48bb1-142">Por ejemplo, el bot puede reservar una mesa y pedir una cena.</span><span class="sxs-lookup"><span data-stu-id="48bb1-142">For example, your bot can reserve a table and order dinner.</span></span>
<span data-ttu-id="48bb1-143">Mientras el usuario está en el flujo para _reservar una mesa_, en lugar de responder la pregunta "¿Cuántas personas están en la lista?", el usuario envía el mensaje "pedir cena".</span><span class="sxs-lookup"><span data-stu-id="48bb1-143">While the user is in the _reserve a table_ flow, instead of answering the question for "How many people are in your party?", the user sends the message "order dinner".</span></span> <span data-ttu-id="48bb1-144">En este caso, el usuario cambió de opinión y quiere tener una conversación para pedir una cena.</span><span class="sxs-lookup"><span data-stu-id="48bb1-144">In this case, the user changed their mind and wants to engage in a dinner ordering conversation instead.</span></span> <span data-ttu-id="48bb1-145">¿Cómo se puede controlar esta interrupción?</span><span class="sxs-lookup"><span data-stu-id="48bb1-145">How should you handle this interruption?</span></span> 

<span data-ttu-id="48bb1-146">Puede cambiar los temas en el flujo para pedir una cena o hacer que sea un problema temporal informando al usuario que se espera un número y solicitárselo de nuevo.</span><span class="sxs-lookup"><span data-stu-id="48bb1-146">You can switch topics to the order dinner flow or you can make it a sticky issue by telling the user that you are expecting a number and reprompt them.</span></span> <span data-ttu-id="48bb1-147">Si permite cambiar los temas, a continuación, debe decidir si se guardará el progreso para que el usuario pueda continuar desde donde lo dejó o podría eliminar toda la información que recopiló para que el usuario tenga que reiniciar el proceso desde el principio la próxima vez que quiera reservar una mesa.</span><span class="sxs-lookup"><span data-stu-id="48bb1-147">If you do allow them to switch topics, you then have to decide if you will save the progress so that the user can pick up from where they left off or you could delete all the information you have collected so that they will have to start that process all over next time they want to reserve a table.</span></span> <span data-ttu-id="48bb1-148">Para obtener más información acerca de cómo administrar los datos de estado de usuario, consulte el artículo [Save state using conversation and user properties](bot-builder-howto-v4-state.md) (Guardar el estado mediante las propiedades del usuario y la conversación).</span><span class="sxs-lookup"><span data-stu-id="48bb1-148">For more information about managing user state data, see [Save state using conversation and user properties](bot-builder-howto-v4-state.md).</span></span>

### <a name="apply-artificial-intelligence"></a><span data-ttu-id="48bb1-149">Aplicar inteligencia artificial</span><span class="sxs-lookup"><span data-stu-id="48bb1-149">Apply artificial intelligence</span></span>
<span data-ttu-id="48bb1-150">Para las interrupciones que no están en el ámbito, puede intentar adivinar la intención del usuario.</span><span class="sxs-lookup"><span data-stu-id="48bb1-150">For interruptions that are not in scope, you can try to guess what the user intent is.</span></span> <span data-ttu-id="48bb1-151">Puede hacerlo mediante servicios de IA, como QnAMaker, LUIS o su lógica personalizada, y, a continuación, ofrecer sugerencias para lo que el bot considera que el usuario desea.</span><span class="sxs-lookup"><span data-stu-id="48bb1-151">You can do this using AI services such as QnAMaker, LUIS, or your custom logic, then offer up suggestions for what the bot thinks the user wants.</span></span> 

<span data-ttu-id="48bb1-152">Por ejemplo, en medio del flujo para reservar una mesa, el usuario dice: "Quiero pedir una hamburguesa".</span><span class="sxs-lookup"><span data-stu-id="48bb1-152">For example, while in the middle of the reserve table flow, the user says, "I want to order a burger".</span></span> <span data-ttu-id="48bb1-153">En este flujo de conversación, el bot no sabe cómo lidiar con esa entrada.</span><span class="sxs-lookup"><span data-stu-id="48bb1-153">This is not something the bot knows how to handle from this conversation flow.</span></span> <span data-ttu-id="48bb1-154">Como el flujo actual no tiene nada que ver con el pedido y el otro comando de conversación del bot es "pedir una cena", el bot no sabe qué hacer con esta entrada.</span><span class="sxs-lookup"><span data-stu-id="48bb1-154">Since the current flow has nothing to do with ordering, and the bot's other conversation command is "order dinner", the bot does not know what to do with this input.</span></span> <span data-ttu-id="48bb1-155">Si aplica LUIS, por ejemplo, podría entrenar el modelo para que reconozca que quiere pedir comida (p. ej.: LUIS puede devolver una intención "orderFood").</span><span class="sxs-lookup"><span data-stu-id="48bb1-155">If you apply LUIS, for example, you could train the model to recognize that they want to order food (e.g.: LUIS can return an "orderFood" intent).</span></span> <span data-ttu-id="48bb1-156">Por lo tanto, el bot podría responder: "Parece que quiere pedir una comida.</span><span class="sxs-lookup"><span data-stu-id="48bb1-156">Thus, the bot could response with, "It seems you want to order food.</span></span> <span data-ttu-id="48bb1-157">¿Le gustaría cambiar a nuestro proceso de pedido de cena?"</span><span class="sxs-lookup"><span data-stu-id="48bb1-157">Would you like to switch to our order dinner process instead?"</span></span> <span data-ttu-id="48bb1-158">Para obtener más información sobre el aprendizaje de LUIS y cómo detectar las intenciones del usuario, consulte [User LUIS for language understanding](bot-builder-howto-v4-luis.md) (LUIS de usuario para comprensión lingüística).</span><span class="sxs-lookup"><span data-stu-id="48bb1-158">For more information on training LUIS and detecting user intents, see [User LUIS for language understanding](bot-builder-howto-v4-luis.md).</span></span>

### <a name="default-response"></a><span data-ttu-id="48bb1-159">Respuesta predeterminada</span><span class="sxs-lookup"><span data-stu-id="48bb1-159">Default response</span></span>
<span data-ttu-id="48bb1-160">Si se produce un error en todo lo demás, puede enviar una respuesta genérica predeterminada en lugar de no hacer nada y dejar al usuario preguntándose qué está ocurriendo.</span><span class="sxs-lookup"><span data-stu-id="48bb1-160">If all else fails, you can send a generic default response instead of doing nothing and leaving the user wondering what is going on.</span></span> <span data-ttu-id="48bb1-161">La respuesta predeterminada debe indicar al usuario cuáles son los comandos que entiende el bot, de modo que el usuario pueda retomar el proceso.</span><span class="sxs-lookup"><span data-stu-id="48bb1-161">The default response should tell the user what commands the bot understands so the user can get back on track.</span></span>

<span data-ttu-id="48bb1-162">Puede comprobar nuevamente la marca de contexto **responded** en la parte final de la lógica del bot para ver si el bot envió algo al usuario durante el turno.</span><span class="sxs-lookup"><span data-stu-id="48bb1-162">You can check against the context **responded** flag at the end of the bot logic to see if the bot sent anything back to the user during the turn.</span></span> <span data-ttu-id="48bb1-163">Si el bot procesa la entrada del usuario, pero no responde, lo más probable es que el bot no sepa qué hacer con la entrada.</span><span class="sxs-lookup"><span data-stu-id="48bb1-163">If the bot processes the user's input but does not respond, chances are that the bot does not know what to do with the input.</span></span> <span data-ttu-id="48bb1-164">En ese caso, puede detectarlo y enviar un mensaje predeterminado al usuario.</span><span class="sxs-lookup"><span data-stu-id="48bb1-164">In that case, you can catch it and send the user a default message.</span></span>

<span data-ttu-id="48bb1-165">El valor predeterminado para este bot es proporcionar al usuario el cuadro de diálogo `mainMenu`.</span><span class="sxs-lookup"><span data-stu-id="48bb1-165">The default for this bot is to give the user the `mainMenu` dialog.</span></span> <span data-ttu-id="48bb1-166">Depende de usted decidir qué experiencia tendrá el usuario en esta situación con el bot.</span><span class="sxs-lookup"><span data-stu-id="48bb1-166">It's up to you to decide what experience your user will have in this situation for your bot.</span></span>

# <a name="ctabcsharptab"></a>[<span data-ttu-id="48bb1-167">C#</span><span class="sxs-lookup"><span data-stu-id="48bb1-167">C#</span></span>](#tab/csharptab)

```cs
if(!context.Responded)
{
    await dc.EndAll().Begin("mainMenu");
}
```

# <a name="javascripttabjstab"></a>[<span data-ttu-id="48bb1-168">JavaScript</span><span class="sxs-lookup"><span data-stu-id="48bb1-168">JavaScript</span></span>](#tab/jstab)

```javascript
// Check to see if anyone replied. If not then clear all the stack and present the main menu
if (!context.responded) {
    await dc.endAll().begin('mainMenu');
}
```

---

