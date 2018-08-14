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
# <a name="handle-user-interrupt"></a>Control de la interrupción del usuario

[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]

El control de interrupciones del usuario es un aspecto importante de un bot sólido. Aunque puede pensar que los usuarios siguen el flujo de conversación definido paso a paso, es muy probable que cambien de parecer o formulen una pregunta en mitad del proceso en lugar de responder la pregunta. En estas situaciones, ¿cómo controlaría su bot la entrada del usuario? ¿Como sería la experiencia del usuario? ¿Cómo mantendría los datos de estado del usuario?

No hay ninguna respuesta correcta a estas preguntas, ya que cada situación es única según el escenario para el que se haya diseñado el bot. En este tema, se exploran algunas formas comunes de controlar las interrupciones del usuario y se sugieren algunas formas para implementarlas en su bot.

## <a name="handle-expected-interruptions"></a>Controlar interrupciones esperadas

Un flujo de conversación de procedimientos cuenta con un conjunto básico de pasos que quiere que el usuario siga y cualquier acción del usuario que no corresponda con esos pasos son posibles interrupciones. En un flujo normal, existen interrupciones que puede anticipar.

**Reserva de una mesa**: en un bot para reservar una mesa, los pasos principales pueden ser pedir al usuario una fecha y hora, el número de personas y el nombre de reserva. En ese proceso, se pueden anticipar algunas interrupciones esperadas, como por ejemplo: 
 * `cancel`: para salir del proceso.
 * `help`: para proporcionar más instrucciones acerca de este proceso.
 * `more info`: para dar consejos y sugerencias o proporcionar maneras alternativas para reservar una mesa (p. ej.: un número de teléfono o una dirección de correo electrónico de contacto).
 * `show list of available tables`: si es una opción; muestra una lista de mesas disponibles para la fecha y hora indicada por el usuario.

**Pedir una cena**: en un bot para pedir una cena, los pasos principales serían proporcionar una lista de los elementos del menú y permitir que el usuario agregue elementos al carro de la compra. En este proceso, se pueden anticipar algunas interrupciones esperadas, como por ejemplo: 
 * `cancel`: para salir del proceso de pedido.
 * `more info`: para proporcionar detalles nutricionales sobre cada elemento del menú.
 * `help`: para proporcionar ayuda sobre cómo usar el sistema.
 * `process order`: para procesar el pedido.

Puede proporcionarlas al usuario como una lista de **acciones sugeridas** o como una sugerencia de modo que el usuario al menos tenga en cuenta qué comandos puede enviar que el bot pueda entender.

Por ejemplo, en el flujo del pedido de cena, se pueden proporcionar interrupciones esperadas junto con los elementos del menú. En este caso, los elementos del menú se envían como una matriz de `choices`.

# <a name="ctabcsharptab"></a>[C#](#tab/csharptab)

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

# <a name="javascripttabjstab"></a>[JavaScript](#tab/jstab)

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

En la lógica del pedido, puede comprobarlos mediante la correspondencia de cadenas o expresiones regulares.

# <a name="ctabcsharptab"></a>[C#](#tab/csharptab)

En primer lugar, debemos definir una aplicación auxiliar para realizar un seguimiento de nuestros pedidos.

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

A continuación, agregue el cuadro de diálogo al bot.

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

# <a name="javascripttabjstab"></a>[JavaScript](#tab/jstab)

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

## <a name="handle-unexpected-interruptions"></a>Controlar interrupciones inesperadas

Hay interrupciones que se encuentran fuera del ámbito de lo que su bot está diseñado para hacer.
Aunque no se pueden prever todas las interrupciones, hay patrones de interrupción que puede programar para que su bot controle.

### <a name="switching-topic-of-conversations"></a>Cambiar el tema de las conversaciones
¿Qué ocurre si el usuario está en medio de una conversación y quiere cambiar a otra conversación? Por ejemplo, el bot puede reservar una mesa y pedir una cena.
Mientras el usuario está en el flujo para _reservar una mesa_, en lugar de responder la pregunta "¿Cuántas personas están en la lista?", el usuario envía el mensaje "pedir cena". En este caso, el usuario cambió de opinión y quiere tener una conversación para pedir una cena. ¿Cómo se puede controlar esta interrupción? 

Puede cambiar los temas en el flujo para pedir una cena o hacer que sea un problema temporal informando al usuario que se espera un número y solicitárselo de nuevo. Si permite cambiar los temas, a continuación, debe decidir si se guardará el progreso para que el usuario pueda continuar desde donde lo dejó o podría eliminar toda la información que recopiló para que el usuario tenga que reiniciar el proceso desde el principio la próxima vez que quiera reservar una mesa. Para obtener más información acerca de cómo administrar los datos de estado de usuario, consulte el artículo [Save state using conversation and user properties](bot-builder-howto-v4-state.md) (Guardar el estado mediante las propiedades del usuario y la conversación).

### <a name="apply-artificial-intelligence"></a>Aplicar inteligencia artificial
Para las interrupciones que no están en el ámbito, puede intentar adivinar la intención del usuario. Puede hacerlo mediante servicios de IA, como QnAMaker, LUIS o su lógica personalizada, y, a continuación, ofrecer sugerencias para lo que el bot considera que el usuario desea. 

Por ejemplo, en medio del flujo para reservar una mesa, el usuario dice: "Quiero pedir una hamburguesa". En este flujo de conversación, el bot no sabe cómo lidiar con esa entrada. Como el flujo actual no tiene nada que ver con el pedido y el otro comando de conversación del bot es "pedir una cena", el bot no sabe qué hacer con esta entrada. Si aplica LUIS, por ejemplo, podría entrenar el modelo para que reconozca que quiere pedir comida (p. ej.: LUIS puede devolver una intención "orderFood"). Por lo tanto, el bot podría responder: "Parece que quiere pedir una comida. ¿Le gustaría cambiar a nuestro proceso de pedido de cena?" Para obtener más información sobre el aprendizaje de LUIS y cómo detectar las intenciones del usuario, consulte [User LUIS for language understanding](bot-builder-howto-v4-luis.md) (LUIS de usuario para comprensión lingüística).

### <a name="default-response"></a>Respuesta predeterminada
Si se produce un error en todo lo demás, puede enviar una respuesta genérica predeterminada en lugar de no hacer nada y dejar al usuario preguntándose qué está ocurriendo. La respuesta predeterminada debe indicar al usuario cuáles son los comandos que entiende el bot, de modo que el usuario pueda retomar el proceso.

Puede comprobar nuevamente la marca de contexto **responded** en la parte final de la lógica del bot para ver si el bot envió algo al usuario durante el turno. Si el bot procesa la entrada del usuario, pero no responde, lo más probable es que el bot no sepa qué hacer con la entrada. En ese caso, puede detectarlo y enviar un mensaje predeterminado al usuario.

El valor predeterminado para este bot es proporcionar al usuario el cuadro de diálogo `mainMenu`. Depende de usted decidir qué experiencia tendrá el usuario en esta situación con el bot.

# <a name="ctabcsharptab"></a>[C#](#tab/csharptab)

```cs
if(!context.Responded)
{
    await dc.EndAll().Begin("mainMenu");
}
```

# <a name="javascripttabjstab"></a>[JavaScript](#tab/jstab)

```javascript
// Check to see if anyone replied. If not then clear all the stack and present the main menu
if (!context.responded) {
    await dc.endAll().begin('mainMenu');
}
```

---

