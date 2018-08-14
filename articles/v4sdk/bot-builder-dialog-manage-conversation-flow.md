---
title: Administración de un flujo de conversación con diálogos | Microsoft Docs
description: Aprenda a administrar un flujo de conversación con diálogos en el SDK de Bot Builder para Node.js.
keywords: flujo de conversación, diálogos, mensajes, cascadas, conjunto de diálogos
author: v-ducvo
ms.author: v-ducvo
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 5/8/2018
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: 99184ba71072c159c598c7f68289c42a51926795
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305264"
---
# <a name="manage-conversation-flow-with-dialogs"></a>Administración del flujo de conversación con diálogos
[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]


Administrar el flujo de conversación es una tarea esencial en la creación de bots. Con el SDK de Bot Builder, puede administrar el flujo de conversación mediante **diálogos**.

Un diálogo es similar a una función de un programa. Por lo general está diseñado para realizar una operación específica y se puede invocar con la frecuencia que sea necesaria. Puede encadenar varios diálogos juntos para así controlar casi cualquier flujo de conversación que quiera que administre el bot. La biblioteca de **diálogos** del SDK de Bot Builder incluye características integradas como **mensajes** y **cascadas** para ayudarle a administrar el flujo de conversación mediante diálogos. La biblioteca de mensajes proporciona varios mensajes, que puede usar para pedir a los usuarios distintos tipos de información. Las cascadas proporcionan una manera de combinar varios pasos en una secuencia.

En este artículo se muestra cómo crear un objeto de diálogos e insertar mensajes y pasos de cascada en un conjunto de diálogos para administrar flujos de conversación sencillos y complejos. 

## <a name="install-the-dialogs-library"></a>Instalación de la biblioteca de diálogos

# <a name="ctabcsharp"></a>[C#](#tab/csharp)
Para usar diálogos, instale el paquete NuGet `Microsoft.Bot.Builder.Dialogs` para su proyecto o solución.
A continuación, haga referencia a la biblioteca de diálogos mediante instrucciones en los archivos de código. Por ejemplo: 

```csharp
using Microsoft.Bot.Builder.Dialogs;
```

# <a name="javascripttabjs"></a>[JavaScript](#tab/js)
La biblioteca `botbuilder-dialogs` se puede descargar desde NPM. Para instalar la biblioteca `botbuilder-dialogs`, ejecute el siguiente comando de NPM:

```cmd
npm install --save botbuilder-dialogs
```

Para usar **dialogs** en el bot, inclúyalo en el código del bot. Por ejemplo: 

**app.js**

```javascript
const botbuilder_dialogs = require('botbuilder-dialogs');
```
---

## <a name="create-a-dialog-stack"></a>Creación de una pila de diálogos

Para usar diálogos, debe crear primero un *conjunto de diálogos*.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

La biblioteca `Microsoft.Bot.Builder.Dialogs` proporciona una clase `DialogSet`.
Para un conjunto de diálogos, puede agregar diálogos y conjuntos de diálogos con nombre y luego acceder a ellos más tarde por su nombre.

```csharp
IDialog dialog = null;
// Initialize dialog.

DialogSet dialogs = new DialogSet();
dialogs.Add("dialog name", dialog);
```

# <a name="javascripttabjs"></a>[JavaScript](#tab/js)

La biblioteca `botbuilder-dialogs` proporciona una clase `DialogSet`.
La clase **DialogSet** define una **pila de diálogos** y le ofrece una sencilla interfaz para administrar la pila.
El SDK de Bot Builder implementa la pila como una matriz.

Para crear un elemento **DialogSet**, realice lo siguiente:

```javascript
const dialogs = new botbuilder_dialogs.DialogSet();
```

La llamada anterior creará un elemento **DialogSet** con una **pila de diálogos** predeterminada llamada `dialogStack`.
Si quiere asignar un nombre a la pila, puede pasarla como un parámetro a **DialogSet()**. Por ejemplo: 

```javascript
const dialogs = new botbuilder_dialogs.DialogSet("myStack");
```
---

Al crear un diálogo solo se agrega la definición del diálogo al conjunto. El diálogo no se ejecuta hasta que se inserta en la pila de diálogos mediante la llamada a un método _begin_ o _replace_. 

El nombre del diálogo (por ejemplo, `addTwoNumbers`) debe ser único dentro de cada conjunto de diálogos. Puede definir tantos diálogos como sea necesario dentro de cada conjunto.

La biblioteca de diálogos define los siguientes diálogos:
-   Un diálogo de **mensaje** donde el diálogo usa al menos dos funciones, una para pedir al usuario que intervenga y la otra para procesar la entrada.
    Los diálogos se pueden encadenar juntos con el modelo de **cascada**.
-   Un diálogo de **cascada** define una secuencia de _pasos de cascada_, que se ejecutan en orden.
    Un diálogo de cascada puede tener un solo paso, en cuyo caso se puede considerar como un diálogo sencillo de un único paso.

## <a name="create-a-single-step-dialog"></a>Creación de un diálogo de un único paso

Los diálogos de un único paso pueden ser útiles para capturar flujos conversacionales de un solo turno. En este ejemplo se crea un bot que puede detectar si el usuario dice algo parecido a "1 + 2" y se inicia un diálogo `addTwoNumbers` para responder con "1 + 2 = 3". 

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

Se pasan valores a los diálogos y se devuelven de estos como contenedores de propiedades `IDictionary<string,object>`.

Para crear un diálogo sencillo dentro de un conjunto de diálogos, use el método `Add`. El siguiente código agrega una cascada de un solo paso llamada `addTwoNumbers`.

En este paso se da por hecho que los argumentos de diálogo que se pasan contienen las propiedades `first` y `second` que representan los números que se van a agregar.

Comience con la plantilla EchoBot. A continuación, agregue código en la clase de bot para agregar el diálogo en el constructor.
```csharp
public class EchoBot : IBot
{
    private DialogSet _dialogs;

    public EchoBot()
    {
        _dialogs = new DialogSet();
        _dialogs.Add("addTwoNumbers", new WaterfallStep[]
        {              
            async (dc, args, next) =>
            {
                double sum = (double)args["first"] + (double)args["second"];
                await dc.Context.SendActivity($"{args["first"]} + {args["second"]} = {sum}");
                await dc.End();
            }
        });
    }

    // The rest of the class definition is omitted here but would include OnTurn()
}

```

### <a name="pass-arguments-to-the-dialog"></a>Paso de argumentos al diálogo

Para llamar al diálogo desde dentro del método `OnTurn` del bot, modifique `OnTurn` para que contenga lo siguiente:
```cs
public async Task OnTurn(ITurnContext context)
{
    // This bot is only handling Messages
    if (context.Activity.Type == ActivityTypes.Message)
    {
        // Get the conversation state from the turn context
        var state = context.GetConversationState<EchoState>();

        // create a dialog context
        var dialogCtx = _dialogs.CreateContext(context, state);

        // Bump the turn count. 
        state.TurnCount++;

        await dialogCtx.Continue();
        if (!context.Responded)
        {
            // Call a helper function that identifies if the user says something 
            // like "2 + 3" or "1.25 + 3.28" and extract the numbers to add            
            if (TryParseAddingTwoNumbers(context.Activity.Text, out double first, out double second))
            { 
                var dialogArgs = new Dictionary<string, object>
                {
                    ["first"] = first,
                    ["second"] = second
                };                        
                await dialogCtx.Begin("addTwoNumbers", dialogArgs);
            }
            else
            {
                // Echo back to the user whatever they typed.
                await context.SendActivity($"Turn: {state.TurnCount}. You said '{context.Activity.Text}'");
            }
        }
    }
}
```

Agregue la función auxiliar a la clase de bot. La función auxiliar simplemente usa una expresión regular sencilla para detectar si el mensaje del usuario es una solicitud para sumar 2 números.

```cs
// Recognizes if the message is a request to add 2 numbers, in the form: number + number, 
// where number may have optionally have a decimal point.: 1 + 1, 123.99 + 45, 0.4+7. 
// For the sake of simplicity it doesn't handle negative numbers or numbers like 1,000 that contain a comma.
// If you need more robust number recognition, try System.Recognizers.Text
public bool TryParseAddingTwoNumbers(string message, out double first, out double second)
{
    // captures a number with optional -/+ and optional decimal portion
    const string NUMBER_REGEXP = "([-+]?(?:[0-9]+(?:\\.[0-9]+)?|\\.[0-9]+))";
    // matches the plus sign with optional spaces before and after it
    const string PLUSSIGN_REGEXP = "(?:\\s*)\\+(?:\\s*)";
    const string ADD_TWO_NUMBERS_REGEXP = NUMBER_REGEXP + PLUSSIGN_REGEXP + NUMBER_REGEXP;
    var regex = new Regex(ADD_TWO_NUMBERS_REGEXP);
    var matches = regex.Matches(message);
    var succeeded = false;
    first = 0;
    second = 0;
    if (matches.Count == 0)
    {
        succeeded = false;
    }
    else
    {
        var matched = matches[0];
        if ( System.Double.TryParse(matched.Groups[1].Value, out first) 
            && System.Double.TryParse(matched.Groups[2].Value, out second))
        {
            succeeded = true;
        } 
    }
    return succeeded;
}
```

Si está usando la plantilla EchoBot, modifique la clase `EchoState` en **EchoState.cs** de la manera siguiente:

```cs
/// <summary>
/// Class for storing conversation state.
/// This bot only stores the turn count in order to echo it to the user
/// </summary>
public class EchoState: Dictionary<string, object>
{
    private const string TurnCountKey = "TurnCount";
    public EchoState()
    {
        this[TurnCountKey] = 0;            
    }

    public int TurnCount
    {
        get { return (int)this[TurnCountKey]; }
        set { this[TurnCountKey] = value; }
    }
}
```

# <a name="javascripttabjs"></a>[JavaScript](#tab/js)

Comience con la plantilla JS descrita en [Create a bot with the Bot Builder SDK v4](../javascript/bot-builder-javascript-quickstart.md) (Creación de un bot con el SDK de Bot Builder v4). En **app.js**, agregue una instrucción para exigir `botbuilder-dialogs`.
```js
const {DialogSet} = require('botbuilder-dialogs');
```

En **app.js**, agregue el código siguiente que define un diálogo sencillo llamado `addTwoNumbers` que pertenece al conjunto `dialogs`:

```javascript
const dialogs = new DialogSet("myDialogStack");

// Show the sum of two numbers.
dialogs.add('addTwoNumbers', [async function (dc, numbers){
        var sum = Number.parseFloat(numbers[0]) + Number.parseFloat(numbers[1]);
        await dc.context.sendActivity(`${numbers[0]} + ${numbers[1]} = ${sum}`);
        await dc.end();
    }]
);
```

Reemplace el código de **app.js** para procesar la actividad entrante por el siguiente código. El bot llama a una función auxiliar para comprobar si el mensaje entrante se parece a una solicitud para sumar dos números. En caso afirmativo, pasa los números del argumento a `DialogContext.begin`.

```js
// Listen for incoming activity 
server.post('/api/messages', (req, res) => {
    // Route received activity to adapter for processing
    adapter.processActivity(req, res, async (context) => {
        const isMessage = context.activity.type === 'message';
        if (isMessage) {
            const state = conversationState.get(context);
            const count = state.count === undefined ? state.count = 0 : ++state.count;

            // create a dialog context
            const dc = dialogs.createContext(context, state);

            // MatchesAdd2Numbers checks if the message matches a regular expression
            // and if it does, returns an array of the numbers to add
            var numbers = await MatchesAdd2Numbers(context.activity.text); 
            if (numbers != null && numbers.length >=2 )
            {    
                await dc.begin('addTwoNumbers', numbers);
            }
            else {
                // Just echo back the user's message if they're not adding numbers
                return context.sendActivity(`Turn ${count}: You said "${context.activity.text}"`); 
            }           
        } else {
            return context.sendActivity(`[${context.activity.type} event detected]`);
        }
        if (!context.responded) {
            await dc.continue();
            // if the dialog didn't send a response
            if (!context.responded && isMessage) {
                await dc.context.sendActivity(`Hi! I'm the add 2 numbers bot. Say something like "what's 1+2?"`);
            }
        }
    });
});
```

Agregue la función auxiliar a **app.js**. La función auxiliar simplemente usa una expresión regular sencilla para detectar si el mensaje del usuario es una solicitud para sumar 2 números. Si la expresión regular coincide, devuelve una matriz que contiene los números que se van a sumar.

```javascript
async function MatchesAdd2Numbers(message) {
    const ADD_NUMBERS_REGEXP = /([-+]?(?:[0-9]+(?:\.[0-9]+)?|\.[0-9]+))(?:\s*)\+(?:\s*)([-+]?(?:[0-9]+(?:\.[0-9]+)?|\.[0-9]+))/i;
    let matched = ADD_NUMBERS_REGEXP.exec(message);
    if (!matched) {
        // message wasn't a request to add 2 numbers
        return null;
    }
    else {
        var numbers = [matched[1], matched[2]];
        return numbers;
    }
}
```

---

### <a name="run-the-bot"></a>Ejecución del bot

Intente ejecutar el bot en Bot Framework Emulator y dígale algo como  "¿Cuánto es 1 + 1?"

![ejecutar el bot](./media/how-to-dialogs/bot-output-add-numbers.png)



## <a name="using-dialogs-to-guide-the-user-through-steps"></a>Uso de diálogos para guiar al usuario paso a paso

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

### <a name="create-a-composite-dialog"></a>Creación de un diálogo compuesto

Los siguientes fragmentos de código se toman del ejemplo de código [Microsoft.Bot.Samples.Dialog.Prompts](https://github.com/Microsoft/botbuilder-dotnet/tree/master/samples/MIcrosoft.Bot.Samples.Dialog.Prompts) del repositorio botbuilder-dotnet.

En Startup.cs:
1.  Cambie el nombre del bot por `DialogContainerBot`.
1.  Use un diccionario sencillo como contenedor de propiedades para el estado de conversación del bot.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddBot<DialogContainerBot>(options =>
    {
        options.CredentialProvider = new ConfigurationCredentialProvider(Configuration);
        options.Middleware.Add(new ConversationState<Dictionary<string, object>>(new MemoryStorage()));
    });
}
```

Cambie el nombre de `EchoBot` por `DialogContainerBot`.

En `DialogContainerBot.cs`, defina una clase para un diálogo de perfil.

```csharp
public class ProfileControl : DialogContainer
{
    public ProfileControl()
        : base("fillProfile")
    {
        Dialogs.Add("fillProfile", 
            new WaterfallStep[]
            {
                async (dc, args, next) =>
                {
                    dc.ActiveDialog.State = new Dictionary<string, object>();
                    await dc.Prompt("textPrompt", "What's your name?");
                },
                async (dc, args, next) =>
                {
                    dc.ActiveDialog.State["name"] = args["Value"];
                    await dc.Prompt("textPrompt", "What's your phone number?");
                },
                async (dc, args, next) =>
                {
                    dc.ActiveDialog.State["phone"] = args["Value"];
                    await dc.End(dc.ActiveDialog.State);
                }
            }
        );
        Dialogs.Add("textPrompt", new Builder.Dialogs.TextPrompt());
    }
}
```

A continuación, en la definición de bot, declare un campo como diálogo principal del bot e inicialícelo en el constructor del bot.
El diálogo principal del bot incluye el diálogo de perfil.

```csharp
private DialogSet _dialogs;

public DialogContainerBot()
{
    _dialogs = new DialogSet();

    _dialogs.Add("getProfile", new ProfileControl());
    _dialogs.Add("firstRun",
        new WaterfallStep[]
        {
            async (dc, args, next) =>
            {
                    await dc.Context.SendActivity("Welcome! We need to ask a few questions to get started.");
                    await dc.Begin("getProfile");
            },
            async (dc, args, next) =>
            {
                await dc.Context.SendActivity($"Thanks {args["name"]} I have your phone number as {args["phone"]}!");
                await dc.End();
            }
        }
    );
}
```

En el método `OnTurn` del bot:
-   Salude al usuario cuando se inicie la conversación.
-   Inicialice y _continúe_ el diálogo principal cada vez que se reciba un mensaje del usuario.

    Si el diálogo no ha generado una respuesta, debe asumir que lo completó anteriormente o que aún no se ha iniciado y _comenzarlo_, mediante la especificación del nombre de diálogo del conjunto con el que empezar.

```csharp
public async Task OnTurn(ITurnContext turnContext)
{
    try
    {
        switch (turnContext.Activity.Type)
        {
            case ActivityTypes.ConversationUpdate:
                foreach (var newMember in turnContext.Activity.MembersAdded)
                {
                    if (newMember.Id != turnContext.Activity.Recipient.Id)
                    {
                        await turnContext.SendActivity("Hello and welcome to the Composite Control bot.");
                    }
                }
                break;

            case ActivityTypes.Message:
                var state = ConversationState<Dictionary<string, object>>.Get(turnContext);
                var dc = _dialogs.CreateContext(turnContext, state);

                await dc.Continue();

                if (!turnContext.Responded)
                {
                    await dc.Begin("firstRun");
                }

                break;
        }
    }
    catch (Exception e)
    {
        await turnContext.SendActivity($"Exception: {e.Message}");
    }
}

```

# <a name="javascripttabjs"></a>[JavaScript](#tab/js)

### <a name="create-a-dialog-with-waterfall-steps"></a>Creación de un diálogo con pasos de cascada

Una conversación se compone de una serie de mensajes intercambiados entre un usuario y un bot. Cuando el objetivo del bot es llevar al usuario a través de una serie de pasos, puede usar un modelo de **cascada** para definir los pasos de la conversación.

Una **cascada** es una implementación específica de un diálogo que se usa normalmente para recopilar información del usuario o guiarlo en una serie de tareas. Las tareas se implementan como una matriz de funciones, donde el resultado de la primera función se pasa como argumentos a la función siguiente y así sucesivamente. Cada función representa normalmente un paso del proceso general. En cada paso, el bot [pide al usuario que intervenga](bot-builder-prompts.md), espera una respuesta y, a continuación, pasa el resultado al paso siguiente.

Por ejemplo, el ejemplo de código siguiente define tres funciones en una matriz que representa los tres pasos de una **cascada**:

```javascript
// Greet user:
// Ask for the user name and then greet them by name.
// Ask them where they work.
dialogs.add('greetings',[
    async function (dc){
        await dc.prompt('textPrompt', 'What is your name?');
    },
    async function(dc, results){
        var userName = results;
        await dc.context.sendActivity(`Hi ${userName}!`);
        await dc.prompt('textPrompt', 'Where do you work?');
    },
    async function(dc, results){
        var workPlace = results;
        await dc.context.sendActivity(`${workPlace} is a fun place.`);
        await dc.end(); // Ends the dialog
    }
]);

dialogs.add('textPrompt', new botbuilder_dialogs.TextPrompt());
```

La signatura del paso de una **cascada** es de la manera siguiente:

| Parámetro | DESCRIPCIÓN |
| ---- | ----- |
| `context` | El contexto del diálogo. |
| `args` | Opcional, contiene los argumentos pasados en el paso. |
| `next` | Opcional, un método que le permite continuar con el paso siguiente de la cascada. Puede proporcionar un argumento *args* cuando llame a este método, lo que le permite pasar los argumentos al siguiente paso de la cascada. |

Cada paso debe llamar a uno de los métodos siguientes antes de devolver: *next()*, *dialogs.prompt()*, *dialogs.end()*, *dialogs.begin()* o *Promise.resolve()*; de lo contrario, el bot se detendrá en ese paso. Es decir, si una función no devuelve uno de estos métodos, todas las intervenciones de los usuarios harán que se vuelva a ejecutar este paso cada vez que el usuario envía al bot un mensaje.

Cuando se alcanza el final de la cascada, es recomendable volver con el método `end()` para que el diálogo se pueda sacar de la pila. Consulte la sección [Finalización de un diálogo](#end-a-dialog) para más información. Igualmente, para continuar desde un paso al siguiente, el paso de cascada debe finalizar con un mensaje o llamar explícitamente a la función `next()` para avanzar por la cascada. 

### <a name="start-a-dialog"></a>Inicio de un diálogo

Para iniciar un diálogo, pase el elemento *dialogId* que quiere iniciar a los métodos `begin()`, `prompt()` o `replace()`. El método **begin** insertará el diálogo al principio de la pila, mientras que el método **replace** sacará el diálogo actual de la pila e insertará el diálogo sustituto en ella.

Para iniciar un diálogo sin argumentos:

```javascript
// Start the 'greetings' dialog.
await dc.begin('greetings');
```

Para iniciar un diálogo con argumentos:

```javascript
// Start the 'greetings' dialog with the 'userName' passed in. 
await dc.begin('greetings', userName);
```

Para iniciar un diálogo de **mensajes**:

```javascript
// Start a 'choicePrompt' dialog with choices passed in as an array of colors to choose from.
await dc.prompt('choicePrompt', `choice: select a color`, ['red', 'green', 'blue']);
```

Según el tipo de símbolo de mensaje que inicie, la signatura del argumento del mensaje puede ser diferente. El método **DialogSet.prompt** es un método auxiliar. Este método toma argumentos y construye las opciones adecuadas para el mensaje; a continuación, llama al método **begin** para iniciar el diálogo de mensajes.

Para reemplazar un diálogo de la pila:

```javascript
// End the current dialog and start the 'mainMenu' dialog.
await dc.replace('mainMenu'); // Can optionally passed in an 'args' as the second argument.
```

Puede encontrar más información sobre cómo usar el método **replace()** en las secciones [Repetición de un diálogo](#repeat-a-dialog) y [Bucles de diálogos](#dialog-loops) a continuación.

## <a name="end-a-dialog"></a>Finalización de un diálogo

Para finaliza un diálogo, sáquelo de la pila y devuelva un resultado opcional al diálogo principal. El método **Dialog.resume()** del diálogo principal se invoca con cualquier resultado devuelto.

Es recomendable llamar explícitamente al método `end()` al final del diálogo; sin embargo, no es obligatorio porque el diálogo se saca automáticamente de la pila cuando se llega al final de la cascada.

Para finalizar un diálogo:

```javascript
// End the current dialog by popping it off the stack
await dc.end();
```

Para finalizar un diálogo con argumentos opcionales pasados al diálogo principal:

```javascript
// End the current dialog and pass a result to the parent dialog
await dc.end(result);
```

Como alternativa, también puede devolver una promesa resuelta para finalizar el diálogo:

```javascript
await Promise.resolve();
```

La llamada a `Promise.resolve()` dará lugar a que finalice el dialogo y se saque de la pila. Sin embargo, este método no llama al diálogo principal para reanudar la ejecución. Después de llamar a `Promise.resolve()`, la ejecución se detiene y el bot reanuda donde se deja el diálogo principal cuando el usuario envía al bot un mensaje. Puede que esta no sea la experiencia idónea para finalizar un diálogo. Considere la posibilidad de finalizar un diálogo con `end()` o `replace()` para que el bot pueda continuar interactuando con el usuario.

### <a name="clear-the-dialog-stack"></a>Borrado de la pila de diálogos

Si quiere sacar todos los diálogos de la pila, puede borrar la pila de diálogos mediante la llamada al método `dc.endAll()`.

```javascript
// Pop all dialogs from the current stack.
await dc.endAll();
```

### <a name="repeat-a-dialog"></a>Repetición de un diálogo

Para repetir un diálogo, use el método `dialogs.replace()`.

```javascript
// End the current dialog and start the 'mainMenu' dialog.
await dc.replace('mainMenu'); 
```

Si quiere mostrar el menú principal de forma predeterminada, puede crear un diálogo `mainMenu` con los pasos siguientes:

```javascript
// Display a menu and ask user to choose a menu item. Direct user to the item selected.
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
        else {
            // Repeat the menu
            await dc.replace('mainMenu');
        }
    },
    async function(dc, result){
        // Start over
        await dc.endAll().begin('mainMenu');
    }
]);

dialogs.add('choicePrompt', new botbuilder_dialogs.ChoicePrompt());
```

Este diálogo usa un elemento `ChoicePrompt` para mostrar el menú y espera a que el usuario elija una opción. Cuando el usuario elige `Order Dinner` o `Reserve a table`, se inicia el diálogo de la opción adecuada, y cuando se realiza esa tarea, este diálogo se repite, en lugar de simplemente finalizarlo en el último paso.

### <a name="dialog-loops"></a>Bucles de diálogos

Otra forma de usar el método `replace()` es mediante la emulación de bucles. Tomemos como ejemplo este escenario. Si quiere permitir que el usuario agregue varios elementos de menú a un carrito, puede crea un bucle con las opciones de menú hasta que el usuario finalice el pedido.

```javascript
// Order dinner:
// Help user order dinner from a menu

var dinnerMenu = {
    choices: ["Potato Salad - $5.99", "Tuna Sandwich - $6.89", "Clam Chowder - $4.50", 
        "More info", "Process order", "Cancel", "Help"],
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

// The order cart
var orderCart = {
    orders: [],
    total: 0,
    clear: function(dc) {
        this.orders = [];
        this.total = 0;
        dc.context.activity.conversation.orderCart = null;
    }
};

dialogs.add('orderDinner', [
    async function (dc){
        await dc.context.sendActivity("Welcome to our Dinner order service.");
        orderCart.clear(dc); // Clears the cart.

        await dc.begin('orderPrompt'); // Prompt for orders
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

// Helper dialog to repeatedly prompt user for orders
dialogs.add('orderPrompt', [
    async function(dc){
        await dc.prompt('choicePrompt', "What would you like?", dinnerMenu.choices);
    },
    async function(dc, choice){
        if(choice.value.match(/process order/ig)){
            if(orderCart.orders.length > 0) {
                // Process the order
                // ...
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

// Define prompts
// Generic prompts
dialogs.add('textPrompt', new botbuilder_dialogs.TextPrompt());
dialogs.add('numberPrompt', new botbuilder_dialogs.NumberPrompt());
dialogs.add('dateTimePrompt', new botbuilder_dialogs.DatetimePrompt());
dialogs.add('choicePrompt', new botbuilder_dialogs.ChoicePrompt());

```

El código de ejemplo anterior muestra que el diálogo principal `orderDinner` usa un diálogo auxiliar llamado `orderPrompt` para controlar las opciones del usuario. El diálogo `orderPrompt` muestra el menú, pide al usuario que elija un artículo, agrega el artículo al carro y le pregunta de nuevo. Esto permite al usuario agregar varios artículos a su pedido. El diálogo se repite hasta que el usuario elige `Process order` o `Cancel`. En ese momento, la ejecución se transfiere al diálogo principal (p. ej.: `orderDinner`). El diálogo `orderDinner` realiza algunas tareas de última hora si el usuario quiere procesar el pedido; en caso contrario, finaliza y devuelve la ejecución de nuevo a su diálogo principal (p. ej.: `mainMenu`). El diálogo `mainMenu` a su vez continúa ejecutando el último paso que es simplemente volver a mostrar las opciones del menú principal.

---

## <a name="next-steps"></a>Pasos siguientes

Ahora que sabe cómo usar **diálogos**, **mensajes** y **cascadas** para administrar el flujo de conversación, echemos un vistazo a cómo podemos dividir nuestros diálogos en tareas modulares en lugar de amontonarlos en el objeto `dialogs` de la lógica del bot.

> [!div class="nextstepaction"]
> [Creación de lógica de bot modular con control compuesto](bot-builder-compositcontrol.md)