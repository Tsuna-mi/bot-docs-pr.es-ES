---
title: Administración de un flujo de conversación complejo con diálogos | Microsoft Docs
description: Aprenda a administrar un flujo de conversación complejo con diálogos en el SDK de Bot Builder para Node.js.
keywords: flujo de conversación complejo, repetir, bucle, menú, diálogos, avisos, cascadas, conjunto de diálogos
author: v-ducvo
ms.author: v-ducvo
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 7/27/2018
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: 304de6783a268140bf74f95d96cd9c24e12c4c05
ms.sourcegitcommit: 1abc32353c20acd103e0383121db21b705e5eec3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/21/2018
ms.locfileid: "40236369"
---
# <a name="manage-complex-conversation-flows-with-dialogs"></a>Administración de flujos de conversación complejos con diálogos

[!INCLUDE [pre-release-label](~/includes/pre-release-label.md)]

Puede administrar flujos de conversación simples y complejos mediante la biblioteca Dialogs. En [flujos de conversación simples](bot-builder-dialog-manage-conversation-flow.md), el usuario comienza desde el primer paso de una *cascada*, continúa hasta el último paso y el intercambio de conversación finaliza. En este artículo, usaremos diálogos para administrar conversaciones más complejas con partes que se pueden bifurcar y repetir en bucle. Para ello, vamos a usar el método _replace_ del contexto del diálogo, además de pasar argumentos entre las distintas partes del diálogo.

<!-- TODO: We need a dialogs conceptual topic to link to, so we can reference that here, in place of describing what they are and what their features are in a how-to topic. -->

Para proporcionarle más control sobre la *pila de diálogos*, la biblioteca **Dialogs** proporciona un método _replace_. Este método permite retirar el diálogo actual de la pila, insertar un diálogo nuevo en la parte superior de la pila e iniciar el nuevo diálogo. Esta característica permite proporcionar una conversación más compleja. Estas técnicas se pueden usar para crear conversaciones arbitrariamente complejas. Si aumenta la complejidad de la conversación hasta que los diálogos resultan difíciles de administrar, es posible crear su propio flujo de lógica de control para realizar un seguimiento de la conversación del usuario.

<!-- TODO: This is probably a good place to add a link to the modular/composite dialogs topic. -->

En este artículo vamos a crear los diálogos para el bot de un hotel que un cliente podría utilizar para reservar una mesa o pedir el envío de alguna comida a su habitación. El diálogo de nivel superior proporciona al invitado estas dos opciones. Si desean reservar una mesa, el diálogo de nivel superior inicia un diálogo de reservas de mesa. Si el cliente desea pedir cena para la habitación, el diálogo de nivel superior inicia un diálogo de pedidos de cena. El diálogo de pedidos de cena primero pide al cliente que seleccione elementos de comida de un menú y, a continuación, pide el número de habitación. La selección de elementos de comida también es un diálogo, por lo que podemos permitir que el cliente seleccione varios elementos antes de procesar su pedido para la cena.

Este diagrama muestra los diálogos que se van a crear en este artículo y su relación entre sí.

![Ilustración de los diálogos que se usan en este artículo](~/media/complex-conversation-flows.png)

## <a name="how-to-branch"></a>Cómo bifurcar

El contexto del diálogo mantiene una _pila de diálogos_ y, para cada diálogo de la pila, lleva el seguimiento de cuál es el siguiente paso. Su método _begin_ inserta un diálogo en la parte superior de la pila y su método _end_ retira el diálogo superior de la pila.

Un diálogo puede iniciar un nuevo diálogo (también conocido como bifurcación) en el mismo conjunto de diálogos mediante una llamada al método _begin_ del contexto del diálogo, al que se proporciona el identificador del diálogo nuevo. El diálogo original está todavía en la pila, pero las llamadas al método _continue_ del contexto del diálogo solo se envían al diálogo superior de la pila, el _diálogo activo_. Cuando un diálogo se retira de la pila, el contexto del diálogo se reanudará con el siguiente paso de la cascada de la pila donde se quedó.

Por lo tanto, puede crear una rama dentro del flujo de conversación al incluir un paso en un diálogo que puede elegir condicionalmente un diálogo para iniciar un conjunto de diálogos posibles.

## <a name="how-to-loop"></a>Cómo repetir en bucle

El método _replace_ del contexto del diálogo le permite reemplazar el diálogo superior de la pila. El estado del diálogo antiguo se descarta y el diálogo nuevo se inicia desde el principio. Puede usar este método para crear un bucle al reemplazar un diálogo por sí mismo. Sin embargo, para [conservar el estado](bot-builder-howto-v4-state.md), necesita pasar información a la nueva instancia del diálogo en la llamada al método _replace_. En el ejemplo siguiente, verá que el carro de pedidos de cena se almacena en el estado del diálogo y, cuando se repite el diálogo `orderPrompt`, se pasa el estado actual del diálogo para que el estado del diálogo nuevo pueda agregar más elementos en él. Si desea almacenar esta información en otros estados, consulte [Conservación de los datos del usuario](bot-builder-tutorial-persist-user-inputs.md).

## <a name="create-the-dialogs-for-the-hotel-bot"></a>Creación de los diálogos para el bot del hotel

En esta sección vamos a crear los diálogos para administrar una conversación para el bot del hotel descrito.

### <a name="install-the-dialogs-library"></a>Instalación de la biblioteca de diálogos

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

Comenzaremos a partir de una plantilla EchoBot básica. Para obtener instrucciones, consulte la [guía de inicio rápido para .NET](~/dotnet/bot-builder-dotnet-quickstart.md).

Para usar diálogos, instale el paquete NuGet `Microsoft.Bot.Builder.Dialogs` para su proyecto o solución.
A continuación, haga referencia a la biblioteca Dialogs en las instrucciones using de los archivos de código según sea necesario.

```csharp
using Microsoft.Bot.Builder.Dialogs;
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

La biblioteca `botbuilder-dialogs` se puede descargar desde NPM. Para instalar la biblioteca `botbuilder-dialogs`, ejecute el siguiente comando de NPM:

```cmd
npm install --save botbuilder-dialogs
```

Para usar **dialogs** en el bot, inclúyalo en el código del bot. Por ejemplo, agregue esto a su archivo **app.js**:

```javascript
const botbuilder_dialogs = require('botbuilder-dialogs');
```

---

### <a name="create-a-dialog-set"></a>Creación de un conjunto de diálogos

Cree un _conjunto de diálogos_ al que vamos a agregar todos los diálogos de este ejemplo.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

Cree una clase **HotelDialog** y agregue las instrucciones using que necesitaremos.

```csharp
using Microsoft.Bot.Builder.Core.Extensions;
using Microsoft.Bot.Builder.Dialogs;
using Microsoft.Bot.Builder.Prompts.Choices;
using Microsoft.Bot.Schema;
using Microsoft.Recognizers.Text;
using System;
using System.Collections.Generic;
using System.Linq;
```

Derive la clase de **DialogSet** y defina los identificadores y las claves que vamos a usar para identificar los diálogos, avisos y la información de estado para este conjunto de diálogos.

```csharp
/// <summary>Contains the set of dialogs and prompts for the hotel bot.</summary>
public class HotelDialog : DialogSet
{
    /// <summary>The ID of the top-level dialog.</summary>
    public const string MainMenu = "mainMenu";

    /// <summary>Contains the IDs for the other dialogs in the set.</summary>
    private static class Dialogs
    {
        public const string OrderDinner = "orderDinner";
        public const string OrderPrompt = "orderPrompt";
        public const string ReserveTable = "reserveTable";
    }

    /// <summary>Contains the IDs for the prompts used by the dialogs.</summary>
    private static class Inputs
    {
        public const string Choice = "choicePrompt";
        public const string Number = "numberPrompt";
    }

    /// <summary>Contains the keys used to manage dialog state.</summary>
    private static class Outputs
    {
        public const string OrderCart = "orderCart";
        public const string OrderTotal = "orderTotal";
        public const string RoomNumber = "roomNumber";
    }
}
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

Para usar **dialogs** en el bot, inclúyalo en el código del bot.

Haga referencia a la biblioteca desde el archivo **app.js**.

```javascript
const botbuilder_dialogs = require('botbuilder-dialogs');
```

Y, a continuación, cree el conjunto de diálogos.

```javascript
const dialogs = new botbuilder_dialogs.DialogSet();
```

---

### <a name="add-the-prompts-to-the-set"></a>Adición de los avisos al conjunto

Vamos a usar un elemento **ChoicePrompt** para preguntar a los clientes si desean pedir cena o reservar una mesa y también qué opción seleccionar del menú de cenas. Y vamos a usar un elemento **NumberPrompt** para preguntar al cliente el número de habitación cuando pida la cena.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

Agregue los dos avisos en el constructor **HotelDialog**.

```csharp
// Add the prompts.
this.Add(Inputs.Choice, new ChoicePrompt(Culture.English));
this.Add(Inputs.Number, new NumberPrompt<int>(Culture.English));
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

Agregue estos dos avisos al conjunto de diálogos.

```javascript
dialogs.add('choicePrompt', new botbuilder_dialogs.ChoicePrompt());
dialogs.add('numberPrompt', new botbuilder_dialogs.NumberPrompt());
```

---

### <a name="define-some-of-the-supporting-information"></a>Definición de la información auxiliar

Puesto que necesitamos información sobre cada opción en el menú de cenas, vamos a configurarlo ahora.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

Cree una clase estática privada llamada **Lists** que va a contener esta información. También crearemos las clases privadas **WelcomeChoice** y **MenuChoice** para contener la información sobre cada opción.

También vamos a agregar información en la lista de opciones en el diálogo de bienvenida de nivel superior y vamos a crear las listas de apoyo que usaremos más adelante para pedir esta información a los clientes. Es un poco de trabajo por adelantado, pero hará que el código del diálogo sea más sencillo.

```csharp
/// <summary>Describes an option for the top-level dialog.</summary>
private class WelcomeChoice
{
    /// <summary>The text to show the guest for this option.</summary>
    public string Description { get; set; }

    /// <summary>The ID of the associated dialog for this option.</summary>
    public string DialogName { get; set; }
}

/// <summary>Describes an option for the food-selection dialog.</summary>
/// <remarks>We have two types of options. One represents meal items that the guest
/// can add to their order. The other represents a request to process or cancel the
/// order.</remarks>
private class MenuChoice
{
    /// <summary>The request text for cancelling the meal order.</summary>
    public const string Cancel = "Cancel order";

    /// <summary>The request text for processing the meal order.</summary>
    public const string Process = "Process order";

    /// <summary>The name of the meal item or the request.</summary>
    public string Name { get; set; }

    /// <summary>The price of the meal item; or NaN for a request.</summary>
    public double Price { get; set; }

    /// <summary>The text to show the guest for this option.</summary>
    public string Description => (double.IsNaN(Price)) ? Name : $"{Name} - ${Price:0.00}";
}

/// <summary>Contains the lists used to present options to the guest.</summary>
private static class Lists
{
    /// <summary>The options for the top-level dialog.</summary>
    public static List<WelcomeChoice> WelcomeOptions { get; } = new List<WelcomeChoice>
    {
        new WelcomeChoice { Description = "Order dinner", DialogName = Dialogs.OrderDinner },
        new WelcomeChoice { Description = "Reserve a table", DialogName = Dialogs.ReserveTable },
    };

    private static List<string> WelcomeList { get; } = WelcomeOptions.Select(x => x.Description).ToList();

    /// <summary>The choices to present in the choice prompt for the top-level dialog.</summary>
    public static List<Choice> WelcomeChoices { get; } = ChoiceFactory.ToChoices(WelcomeList);

    /// <summary>The reprompt action for the top-level dialog.</summary>
    public static Activity WelcomeReprompt
    {
        get
        {
            var reprompt = MessageFactory.SuggestedActions(WelcomeList, "Please choose an option");
            reprompt.AttachmentLayout = AttachmentLayoutTypes.List;
            return reprompt as Activity;
        }
    }

    /// <summary>The options for the food-selection dialog.</summary>
    public static List<MenuChoice> MenuOptions { get; } = new List<MenuChoice>
    {
        new MenuChoice { Name = "Potato Salad", Price = 5.99 },
        new MenuChoice { Name = "Tuna Sandwich", Price = 6.89 },
        new MenuChoice { Name = "Clam Chowder", Price = 4.50 },
        new MenuChoice { Name = MenuChoice.Process, Price = double.NaN },
        new MenuChoice { Name = MenuChoice.Cancel, Price = double.NaN },
    };

    private static List<string> MenuList { get; } = MenuOptions.Select(x => x.Description).ToList();

    /// <summary>The choices to present in the choice prompt for the food-selection dialog.</summary>
    public static List<Choice> MenuChoices { get; } = ChoiceFactory.ToChoices(MenuList);

    /// <summary>The reprompt action for the food-selection dialog.</summary>
    public static Activity MenuReprompt
    {
        get
        {
            var reprompt = MessageFactory.SuggestedActions(MenuList, "Please choose an option");
            reprompt.AttachmentLayout = AttachmentLayoutTypes.List;
            return reprompt as Activity;
        }
    }
}
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

Cree una constante **dinnerMenu** que va a contener esta información.

```javascript
const dinnerMenu = {
    choices: ["Potato Salad - $5.99", "Tuna Sandwich - $6.89", "Clam Chowder - $4.50", 
        "Process order", "Cancel"],
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

### <a name="create-the-welcome-dialog"></a>Creación del diálogo de bienvenida

Este diálogo usa un elemento `ChoicePrompt` para mostrar el menú y espera a que el usuario elija una opción. Cuando el usuario elige `Order dinner` o `Reserve a table`, se inicia el diálogo de la opción adecuada y, cuando ese diálogo finaliza, en lugar de simplemente terminar el diálogo en el último paso y dejar que el usuario se pregunte qué está pasando, este diálogo `mainMenu` se repite a sí mismo iniciando el diálogo `mainMenu` de nuevo. Con esta simple estructura, el bot siempre mostrará el menú y el usuario siempre sabrá cuáles son sus opciones.

El diálogo **MainMenu** contiene estos pasos de cascada.

* En el primer paso de la cascada, se inicializa o se borra el estado del diálogo, se saluda al cliente y se le pide que elija entre las opciones disponibles: `Order dinner` o `Reserve a table`.
* En el segundo paso, se recupera su selección y, a continuación, se inicia el diálogo secundario asociado a esa selección. Una vez que el diálogo secundario finaliza, este diálogo se reanudará en el paso siguiente.
* En el último paso, usamos el método **DialogContext.Replace** para reemplazar este diálogo por una nueva instancia de sí mismo, lo que convierte el diálogo de bienvenida en un menú en bucle.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

```csharp
// Add the main welcome dialog.
this.Add(MainMenu, new WaterfallStep[]
{
    async (dc, args, next) =>
    {
        // Greet the guest and ask them to choose an option.
        await dc.Context.SendActivity("Welcome to Contoso Hotel and Resort.");
        await dc.Prompt(Inputs.Choice, "How may we serve you today?", new ChoicePromptOptions
        {
            Choices = Lists.WelcomeChoices,
            RetryPromptActivity = Lists.WelcomeReprompt,
        });
    },
    async (dc, args, next) =>
    {
        // Begin a child dialog associated with the chosen option.
        var choice = (FoundChoice)args["Value"];
        var dialogId = Lists.WelcomeOptions[choice.Index].DialogName;

        await dc.Begin(dialogId, dc.ActiveDialog.State);
    },
    async (dc, args, next) =>
    {
        // Start this dialog over again.
        await dc.Replace(MainMenu);
    },
});
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

```javascript
// Display a menu and ask user to choose a menu item. Direct user to the item selected otherwise, show
// the menu again.
dialogs.add('mainMenu', [
    async function(dc){
        await dc.context.sendActivity("Welcome to Contoso Hotel and Resort.");
        await dc.prompt('choicePrompt', "How may we serve you today?", ['Order Dinner', 'Reserve a table']);
    },
    async function(dc, result){
        if(result.value.match(/order dinner/ig)){
            await dc.begin('orderDinner');
        }
        else if(result.value.match(/reserve a table/ig)){
            await dc.begin('reserveTable');
        }
    },
    async function(dc, result){
        // Start over
        await dc.endAll().begin('mainMenu');
    }
]);
```

---

### <a name="create-the-order-dinner-dialog"></a>Creación del diálogo de pedidos de cena

En el diálogo de pedidos de cena, se da la bienvenida al cliente al "servicio de pedidos de cena" y se inicia inmediatamente el diálogo de selección de comidas, que abordaremos en la sección siguiente. Lo importante es que, si el cliente pide al servicio que procese el pedido, el diálogo de selección de comidas devuelve la lista de elementos del pedido. Para finalizar el proceso completo, este diálogo solicita un número de habitación para entregar la comida y, a continuación, envía un mensaje de confirmación antes de finalizar. Si el cliente cancela su pedido, este diálogo no pide un número de habitación y finaliza inmediatamente.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

Agregue una estructura de datos a la clase **HotelDialog** que podremos usar para hacer un seguimiento del pedido de cena del cliente.

```csharp
/// <summary>Contains the guest's dinner order.</summary>
private class OrderCart : List<MenuChoice> { }
```

En el constructor **HotelDialog**, agregue el diálogo de pedidos de cena.

* En el primer paso, se envía un mensaje de bienvenida y se inicia el diálogo de selección de comidas.
* En el segundo paso, comprobamos si el diálogo de selección de comidas devuelve un carro del pedido.
  * Si es así, se le pedirá su número de habitación y se guarda la información del carro del pedido.
  * Si no, se supone que canceló su pedido y se llama a **DialogContext.End** para finalizar este diálogo.
* En el último paso, se registra el número de habitación y se envía un mensaje de confirmación antes de finalizar. Este es el paso en que el bot reenviaría el pedido a un servicio de procesamiento.

```csharp
// Add the order-dinner dialog.
this.Add(Dialogs.OrderDinner, new WaterfallStep[]
{
    async (dc, args, next) =>
    {
        await dc.Context.SendActivity("Welcome to our Dinner order service.");

        // Start the food selection dialog.
        await dc.Begin(Dialogs.OrderPrompt);
    },
    async (dc, args, next) =>
    {
        if (args.TryGetValue(Outputs.OrderCart, out object arg) && arg is OrderCart cart)
        {
            // If there are items in the order, record the order and ask for a room number.
            dc.ActiveDialog.State[Outputs.OrderCart] = cart;
            await dc.Prompt(Inputs.Number, "What is your room number?", new PromptOptions
            {
                RetryPromptString = "Please enter your room number."
            });
        }
        else
        {
            // Otherwise, assume the order was cancelled by the guest and exit.
            await dc.End();
        }
    },
    async (dc, args, next) =>
    {
        // Get and save the guest's answer.
        var roomNumber = args["Text"] as string;
        dc.ActiveDialog.State[Outputs.RoomNumber] = roomNumber;

        // Process the dinner order using the collected order cart and room number.

        await dc.Context.SendActivity($"Thank you. Your order will be delivered to room {roomNumber} within 45 minutes.");
        await dc.End();
    },
});
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

```javascript
// Order dinner:
// Help user order dinner from a menu
dialogs.add('orderDinner', [
    async function (dc){
        await dc.context.sendActivity("Welcome to our Dinner order service.");
        
        await dc.begin('orderPrompt', dc.activeDialog.state.orderCart); // Prompt for orders
    },
    async function (dc, result) {
        if(result == "Cancel"){
            await dc.end();
        }
        else { 
            await dc.prompt('numberPrompt', "What is your room number?");
        }
    },
    async function(dc, result){
        await dc.context.sendActivity(`Thank you. Your order will be delivered to room ${result} within 45 minutes.`);
        await dc.end();
    }
]);
```

---

### <a name="create-the-order-prompt-dialog"></a>Creación del diálogo de pedidos

En el diálogo de selección de comidas, presentaremos al cliente una lista de opciones que incluye los elementos de la cena que pueden pedir y las dos solicitudes de procesamiento de pedidos. Este diálogo se repite hasta que el cliente decide que el bot procese o cancele su pedido.

* Cuando el cliente selecciona un elemento de la cena, lo agregamos a su _carro_.
* Si el cliente elige procesar su pedido, primero comprobamos si el carro está vacío.
  * Si está vacío, se envía un mensaje de error y continúa el bucle.
  * En caso contrario, se finaliza este diálogo y se devuelve la información del carro al diálogo primario.
* Si el cliente elige cancelar su pedido, se finaliza el diálogo y no se devuelve información del carro.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

En el constructor **HotelDialog**, agregue el diálogo de selección de comidas.

* En el primer paso se inicializa el estado del diálogo. Si los argumentos de entrada del diálogo contienen información del carro, la guardamos en el estado del diálogo; en caso contrario, se crea y se agrega un carro vacío. A continuación, se le pedirá al cliente que seleccione una opción del menú de cenas.
* En el paso siguiente, nos centramos en la opción que el cliente ha seleccionado:
  * Si es una solicitud para procesar el pedido, se comprueba si el carro contiene algún elemento.
    * Si es así, se finaliza el diálogo y se devuelve el contenido del carro.
    * En caso contrario, se envía un mensaje de error al cliente y se empieza de nuevo desde el principio del diálogo.
  * Si es una solicitud para cancelar el pedido, se finaliza el diálogo y se devuelve un diccionario vacío.
  * Si es un elemento de la cena, se agrega al carro, se envía un mensaje de estado y el diálogo vuelve a comenzar, pasando el estado actual del pedido.

```csharp
// Add the food-selection dialog.
this.Add(Dialogs.OrderPrompt, new WaterfallStep[]
    {
        async (dc, args, next) =>
        {
            if (args is null || !args.ContainsKey(Outputs.OrderCart))
            {
                // First time through, initialize the order state.
                dc.ActiveDialog.State[Outputs.OrderCart] = new OrderCart();
                dc.ActiveDialog.State[Outputs.OrderTotal] = 0.0;
            }
            else
            {
                // Otherwise, set the order state to that of the arguments.
                dc.ActiveDialog.State = new Dictionary<string, object>(args);
            }

            await dc.Prompt(Inputs.Choice, "What would you like?", new ChoicePromptOptions
            {
                Choices = Lists.MenuChoices,
                RetryPromptActivity = Lists.MenuReprompt,
            });
        },
        async (dc, args, next) =>
        {
            // Get the guest's choice.
            var choice = (FoundChoice)args["Value"];
            var option = Lists.MenuOptions[choice.Index];

            // Get the current order from dialog state.
            var cart = (OrderCart)dc.ActiveDialog.State[Outputs.OrderCart];

            if (option.Name is MenuChoice.Process)
            {
                if (cart.Count > 0)
                {
                    // If there are any items in the order, then exit this dialog,
                    // and return the list of selected food items.
                    await dc.End(new Dictionary<string, object>
                    {
                        [Outputs.OrderCart] = cart
                    });
                }
                else
                {
                    // Otherwise, send an error message and restart from
                    // the beginning of this dialog.
                    await dc.Context.SendActivity(
                        "Your cart is empty. Please add at least one item to the cart.");
                    await dc.Replace(Dialogs.OrderPrompt);
                }
            }
            else if (option.Name is MenuChoice.Cancel)
            {
                await dc.Context.SendActivity("Your order has been cancelled.");

                // Exit this dialog, returning an empty property bag.
                dc.ActiveDialog.State.Clear();
                await dc.End(new Dictionary<string, object>());
            }
            else
            {
                // Add the selected food item to the order and update the order total.
                cart.Add(option);
                var total = (double)dc.ActiveDialog.State[Outputs.OrderTotal] + option.Price;
                dc.ActiveDialog.State[Outputs.OrderTotal] = total;

                await dc.Context.SendActivity($"Added {option.Name} (${option.Price:0.00}) to your order." +
                    Environment.NewLine + Environment.NewLine +
                    $"Your current total is ${total:0.00}.");

                // Present the order options again, passing in the current order state.
                await dc.Replace(Dialogs.OrderPrompt, dc.ActiveDialog.State);
            }
        },
    });
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

```javascript
// Helper dialog to repeatedly prompt user for orders
dialogs.add('orderPrompt', [
    async function(dc, orderCart){
        // Define a new cart of one does not exists
        if(!orderCart){
            // Initialize a new cart
            // convoState = conversationState.get(dc.context);
            dc.activeDialog.state.orderCart = {
                orders: [],
                total: 0
            };
        }
        else {
            dc.activeDialog.state.orderCart = orderCart;
        }
        await dc.prompt('choicePrompt', "What would you like?", dinnerMenu.choices);
    },
    async function(dc, choice){
        // Get state object
        // convoState = conversationState.get(dc.context);

        if(choice.value.match(/process order/ig)){
            if(dc.activeDialog.state.orderCart.orders.length > 0) {
                // Process the order
                // ...
                dc.activeDialog.state.orderCart = undefined; // Reset cart
                await dc.context.sendActivity("Processing your order.");
                await dc.end();
            }
            else {
                await dc.context.sendActivity("Your cart was empty. Please add at least one item to the cart.");
                // Ask again
                await dc.replace('orderPrompt');
            }
        }
        else if(choice.value.match(/cancel/ig)){
            //dc.activeDialog.state.orderCart = undefined; // Reset cart
            await dc.context.sendActivity("Your order has been canceled.");
            await dc.end(choice.value);
        }
        else {
            var item = dinnerMenu[choice.value];

            // Only proceed if user chooses an item from the menu
            if(!item){
                await dc.context.sendActivity("Sorry, that is not a valid item. Please pick one from the menu.");
                
                // Ask again
                await dc.replace('orderPrompt');
            }
            else {
                // Add the item to cart
                dc.activeDialog.state.orderCart.orders.push(item);
                dc.activeDialog.state.orderCart.total += item.Price;

                await dc.context.sendActivity(`Added to cart: ${choice.value}. <br/>Current total: $${dc.activeDialog.state.orderCart.total}`);

                // Ask again
                await dc.replace('orderPrompt', dc.activeDialog.state.orderCart);
            }
        }
    }
]);
```

El código de ejemplo anterior muestra que el diálogo principal `orderDinner` usa un diálogo auxiliar llamado `orderPrompt` para controlar las opciones del usuario. El diálogo `orderPrompt` muestra el menú de elementos de comida, pide al usuario que elija un elemento, agrega el elemento al carro y le pregunta de nuevo en un bucle. Esto permite al usuario agregar varios artículos a su pedido. El diálogo se repite hasta que el usuario elige `Process order` o `Cancel`. En ese momento, la ejecución se transfiere al diálogo principal (el diálogo `orderDinner`). El diálogo `orderDinner` realiza algunas tareas de última hora si el usuario quiere procesar el pedido; en caso contrario, finaliza y devuelve la ejecución de nuevo a su diálogo principal (p. ej.: `mainMenu`). El diálogo `mainMenu` a su vez continúa ejecutando el último paso que es simplemente volver a mostrar las opciones del menú principal.

---
### <a name="create-the-reserve-table-dialog"></a>Creación del diálogo de reserva de mesa

<!-- TODO: Update the "Manage simple conversation flows" topic to use a reserveTable dialog, and then reference that from here. -->

Para no extender este ejemplo, se muestra solo una implementación mínima del diálogo `reserveTable`.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

```csharp
// Add the table-reservation dialog.
this.Add(Dialogs.ReserveTable, new WaterfallStep[]
    {
        // Replace this waterfall with your reservation steps.
        async (dc, args, next) =>
        {
            await dc.Context.SendActivity("Your table has been reserved.");
            await dc.End();
        }
    });
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

```javascript
// Reserve a table:
// Help the user to reserve a table

dialogs.add('reserveTable', [
    // Replace this waterfall with your reservation steps.
    async function(dc, args, next){
        await dc.context.sendActivity("Your table has been reserved");
        await dc.end();
    }
]);
```

---

## <a name="next-steps"></a>Pasos siguientes

Puede ofrecer otras opciones en el menú, como "más información" o "ayuda" para mejorar este bot. Para más información sobre la implementación de estos tipos de interrupciones, consulte [Control de las interrupciones de usuario](bot-builder-howto-handle-user-interrupt.md).

Ahora que ha aprendido a usar diálogos, avisos y cascadas para administrar flujos de conversación complejos, echemos un vistazo a cómo podemos dividir nuestros diálogos (como los diálogos `orderDinner` y `reserveTable`) en objetos independientes, en lugar de agruparlos en un conjunto de diálogos de gran tamaño.

> [!div class="nextstepaction"]
> [Creación de lógica de bot modular con control compuesto](bot-builder-compositcontrol.md)
