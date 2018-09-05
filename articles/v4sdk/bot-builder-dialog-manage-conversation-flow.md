---
title: Administración de un flujo de conversación simple con diálogos | Microsoft Docs
description: Aprenda a administrar un flujo de conversación simple con diálogos en Bot Builder SDK para Node.js.
keywords: flujo de conversación simple, diálogos, avisos, cascadas, conjunto de diálogos
author: v-ducvo
ms.author: v-ducvo
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 8/2/2018
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: 77162601f542e6faa8908bc71abc971eb99fcc93
ms.sourcegitcommit: 1abc32353c20acd103e0383121db21b705e5eec3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/21/2018
ms.locfileid: "42756769"
---
# <a name="manage-simple-conversation-flow-with-dialogs"></a>Administración de un flujo de conversación simple con diálogos

[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]

Puede administrar flujos de conversación simples y complejos mediante la biblioteca Dialogs. En un flujo de conversación simple, el usuario comienza desde el primer paso de una *cascada*, continúa hasta el último paso y el intercambio de conversación finaliza. Los diálogos también pueden controlar [flujos de conversación complejos](~/v4sdk/bot-builder-dialog-manage-complex-conversation-flow.md) en los que partes del diálogo se pueden bifurcar y repetir en bucle.

<!-- TODO: We need a dialogs conceptual topic to link to, so we can reference that here, in place of describing what they are and what their features are in a how-to topic. -->

<!-- TODO: This paragraph belongs in a conceptual topic. --> Un diálogo es similar a una función de un programa. Por lo general está diseñado para realizar una operación específica en un orden específico y se puede invocar con la frecuencia que sea necesaria. El uso de diálogos permite al desarrollador de bots guiar el flujo de la conversación. Puede encadenar varios diálogos juntos para así controlar casi cualquier flujo de conversación que quiera que administre el bot. La biblioteca **Dialogs** de Bot Builder SDK incluye características integradas como _avisos_ y _diálogos en cascada_ para ayudarle a administrar el flujo de la conversación. Puede utilizar avisos para pedir a los usuarios distintos tipos de información. Puede usar una cascada para combinar varios pasos en una secuencia.

En este artículo, usamos _conjuntos de diálogos_ para crear un flujo de conversación que contiene tanto pasos de aviso como de cascada. Tenemos dos diálogos de ejemplo. El primero es un diálogo de un solo paso que realiza una operación que no requiere ninguna entrada del usuario. El segundo es un diálogo de varios pasos que solicita al usuario cierta información.

## <a name="install-the-dialogs-library"></a>Instalación de la biblioteca de diálogos

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
npm install --save botbuilder-dialogs@preview
```

Para usar **diálogos** en el bot debe incluir esto en el archivo **app.js**.

```javascript
const botbuilder_dialogs = require('botbuilder-dialogs');
```

---

## <a name="create-a-dialog-stack"></a>Creación de una pila de diálogos

En este primer ejemplo, vamos a crear un diálogo de un paso que puede sumar dos números y mostrar el resultado.

Para usar diálogos, debe crear primero un *conjunto de diálogos*.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

La biblioteca `Microsoft.Bot.Builder.Dialogs` proporciona una clase `DialogSet`.
Cree una clase **AdditionDialog** y agregue las instrucciones using que necesitaremos.
Puede agregar diálogos con nombre y conjuntos de diálogos a un conjunto de diálogos y luego acceder a ellos más tarde por su nombre.

```csharp
using Microsoft.Bot.Builder.Dialogs;
```

Derive la clase de **DialogSet** y defina los identificadores y las claves que vamos a usar para identificar los diálogos y la información de entrada para este conjunto de diálogos.

```csharp
/// <summary>Defines a simple dialog for adding two numbers together.</summary>
public class AdditionDialog : DialogSet
{
    /// <summary>The ID of the main dialog in the set.</summary>
    public const string Main = "additionDialog";

    /// <summary>Defines the IDs of the input arguments.</summary>
    public struct Inputs
    {
        public const string First = "first";
        public const string Second = "second";
    }
}
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

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

El nombre del diálogo (por ejemplo, `addTwoNumbers`) debe ser único dentro de cada conjunto de diálogos. Puede definir tantos diálogos como sea necesario dentro de cada conjunto. Si desea crear varios conjuntos de diálogos y hacer que funcionen juntos sin problemas, consulte [Creación de lógica de bot modular](bot-builder-compositcontrol.md).

La biblioteca de diálogos define los siguientes diálogos:

* Un diálogo de **mensaje** donde el diálogo usa al menos dos funciones, una para pedir al usuario que intervenga y la otra para procesar la entrada. Los diálogos se pueden encadenar juntos con el modelo de **cascada**.
* Un diálogo de **cascada** define una secuencia de _pasos de cascada_, que se ejecutan en orden. Un diálogo de cascada puede tener un solo paso, en cuyo caso se puede considerar como un diálogo sencillo de un único paso.

## <a name="create-a-single-step-dialog"></a>Creación de un diálogo de un único paso

Los diálogos de un único paso pueden ser útiles para capturar flujos conversacionales de un solo turno. En este ejemplo se crea un bot que puede detectar si el usuario dice algo parecido a "1 + 2" y se inicia un diálogo `addTwoNumbers` para responder con "1 + 2 = 3".

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

Se pasan valores a los diálogos y se devuelven de estos como contenedores de propiedades `IDictionary<string,object>`.

Para crear un diálogo sencillo dentro de un conjunto de diálogos, use el método `Add`. El siguiente código agrega una cascada de un solo paso llamada `addTwoNumbers`.

En este paso se da por hecho que los argumentos de diálogo que se pasan contienen las propiedades `first` y `second` que representan los números que se van a agregar.

Agregue el siguiente constructor a la clase **AdditionDialog**.

```csharp
/// <summary>Defines the steps of the dialog.</summary>
public AdditionDialog()
{
    Add(Main, new WaterfallStep[]
    {
        async (dc, args, next) =>
        {
            // Get the input from the arguments to the dialog and add them.
            var x =(double)args[Inputs.First];
            var y =(double)args[Inputs.Second];
            var sum = x + y;

            // Display the result to the user.
            await dc.Context.SendActivity($"{x} + {y} = {sum}");

            // End the dialog.
            await dc.End();
        }
    });
}
```

### <a name="pass-arguments-to-the-dialog"></a>Paso de argumentos al diálogo

En el código del bot, actualice las instrucciones using.

```cs
using Microsoft.Bot;
using Microsoft.Bot.Builder;
using Microsoft.Bot.Builder.Core.Extensions;
using Microsoft.Bot.Schema;
using System.Collections.Generic;
using System.Text.RegularExpressions;
using System.Threading.Tasks;
```

Agregue una propiedad estática a la clase para el diálogo de adición.

```cs
private static AdditionDialog AddTwoNumbers { get; } = new AdditionDialog();
```

Para llamar al diálogo desde dentro del método `OnTurn` del bot, modifique `OnTurn` para que contenga lo siguiente:

```cs
public async Task OnTurn(ITurnContext context)
{
    // Handle any message activity from the user.
    if (context.Activity.Type is ActivityTypes.Message)
    {
        // Get the conversation state from the turn context.
        var conversationState = context.GetConversationState<ConversationData>();

        // Generate a dialog context for the addition dialog.
        var dc = AddTwoNumbers.CreateContext(context, conversationState.DialogState);

        // Call a helper function that identifies if the user says something
        // like "2 + 3" or "1.25 + 3.28" and extract the numbers to add.
        if (TryParseAddingTwoNumbers(context.Activity.Text, out double first, out double second))
        {
            // Start the dialog, passing in the numbers to add.
            var args = new Dictionary<string, object>
            {
                [AdditionDialog.Inputs.First] = first,
                [AdditionDialog.Inputs.Second] = second
            };
            await dc.Begin(AdditionDialog.Main, args);
        }
        else
        {
            // Echo back to the user whatever they typed.
            await context.SendActivity($"You said '{context.Activity.Text}'");
        }
    }
}
```

Agregue una función auxiliar **TryParseAddingTwoNumbers** a la clase del bot. La función auxiliar simplemente usa una expresión regular sencilla para detectar si el mensaje del usuario es una solicitud para sumar 2 números.

```cs
// Recognizes if the message is a request to add 2 numbers, in the form: number + number,
// where number may have optionally have a decimal point.: 1 + 1, 123.99 + 45, 0.4+7.
// For the sake of simplicity it doesn't handle negative numbers or numbers like 1,000 that contain a comma.
// If you need more robust number recognition, try System.Recognizers.Text
public static bool TryParseAddingTwoNumbers(string message, out double first, out double second)
{
    // captures a number with optional -/+ and optional decimal portion
    const string NUMBER_REGEXP = "([-+]?(?:[0-9]+(?:\\.[0-9]+)?|\\.[0-9]+))";

    // matches the plus sign with optional spaces before and after it
    const string PLUSSIGN_REGEXP = "(?:\\s*)\\+(?:\\s*)";

    const string ADD_TWO_NUMBERS_REGEXP = NUMBER_REGEXP + PLUSSIGN_REGEXP + NUMBER_REGEXP;

    var regex = new Regex(ADD_TWO_NUMBERS_REGEXP);
    var matches = regex.Matches(message);

    first = 0;
    second = 0;
    if (matches.Count > 0)
    {
        var matched = matches[0];
        if (double.TryParse(matched.Groups[1].Value, out first)
            && double.TryParse(matched.Groups[2].Value, out second))
        {
            return true;
        }
    }
    return false;
}
```

Si usa la plantilla EchoBot, cambie el nombre de la clase **EchoState** a **ConversationData** y modifíquelo para que contenga lo siguiente.

```cs
using System.Collections.Generic;

/// <summary>
/// Class for storing conversation state.
/// </summary>
public class ConversationData
{
    /// <summary>Property for storing dialog state.</summary>
    public Dictionary<string, object> DialogState { get; set; } = new Dictionary<string, object>();
}
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

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
        // State will store all of your information
        const convoState = conversationState.get(context);
        const dc = dialogs.createContext(context, convoState);

        if (isMessage) {
            // TryParseAddingTwoNumbers checks if the message matches a regular expression
            // and if it does, returns an array of the numbers to add
            var numbers = await TryParseAddingTwoNumbers(context.activity.text); 
            if (numbers != null && numbers.length >=2 )
            {
                await dc.begin('addTwoNumbers', numbers);
            }
            else {
                // Just echo back the user's message if they're not adding numbers
                const count = (convoState.count === undefined ? convoState.count = 0 : ++convoState.count);
                return context.sendActivity(`Turn ${count}: You said "${context.activity.text}"`);
            }
        }
        else {
            return context.sendActivity(`[${context.activity.type} event detected]`);
        }

        if (!context.responded) {
            await dc.continue();
            // if the dialog didn't send a response
            if (!context.responded && isMessage) {
                await dc.context.sendActivity(`Hi! I'm the add 2 numbers bot. Say something like "What's 2+3?"`);
            }
        }
    });
});

```

Agregue la función auxiliar a **app.js**. La función auxiliar simplemente usa una expresión regular sencilla para detectar si el mensaje del usuario es una solicitud para sumar 2 números. Si la expresión regular coincide, devuelve una matriz que contiene los números que se van a sumar.

```javascript
async function TryParseAddingTwoNumbers(message) {
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

En el ejemplo siguiente, creamos un diálogo de varios pasos para solicitar información al usuario.

### <a name="create-a-dialog-with-waterfall-steps"></a>Creación de un diálogo con pasos de cascada

Una **cascada** es una implementación específica de un diálogo que se usa normalmente para recopilar información del usuario o guiarlo en una serie de tareas. Las tareas se implementan como una matriz de funciones, donde el resultado de la primera función se pasa como argumentos a la función siguiente y así sucesivamente. Cada función representa normalmente un paso del proceso general. En cada paso, el bot [pide al usuario que intervenga](bot-builder-prompts.md), espera una respuesta y, a continuación, pasa el resultado al paso siguiente.

Por ejemplo, el ejemplo de código siguiente define tres funciones en una matriz que representan los tres pasos de una **cascada**. Después de cada aviso, el bot confirma la entrada del usuario, pero no guarda la entrada. Si desea conservar las entradas del usuario, consulte [Conservación de datos de usuario](bot-builder-tutorial-persist-user-inputs.md) para más detalles.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

Esto muestra un constructor para un diálogo de saludo, en el que **GreetingDialog** deriva de **DialogSet**, **Inputs.Text** contiene el identificador que vamos a usar para el objeto **TextPrompt** y **Main** contiene el identificador del propio diálogo de saludo.

```csharp
public GreetingDialog()
{
    // Include a text prompt.
    Add(Inputs.Text, new TextPrompt());

    // Define the dialog logic for greeting the user.
    Add(Main, new WaterfallStep[]
    {
        async (dc, args, next) =>
        {
            // Ask for their name.
            await dc.Prompt(Inputs.Text, "What is your name?");
        },
        async (dc, args, next) =>
        {
            // Get the prompt result.
            var name = args["Text"] as string;

            // Acknowledge their input.
            await dc.Context.SendActivity($"Hi, {name}!");

            // Ask where they work.
            await dc.Prompt(Inputs.Text, "Where do you work?");
        },
        async (dc, args, next) =>
        {
            // Get the prompt result.
            var work = args["Text"] as string;

            // Acknowledge their input.
            await dc.Context.SendActivity($"{work} is a fun place.");

            // End the dialog.
            await dc.End();
        }
    });
}
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

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

---

La signatura del paso de una **cascada** es de la manera siguiente:

| Parámetro | DESCRIPCIÓN |
| :---- | :----- |
| `dc` | El contexto del diálogo. |
| `args` | Opcional, contiene los argumentos pasados en el paso. |
| `next` | Opcional, un método que le permite continuar con el paso siguiente de la cascada sin pedir confirmación. Puede proporcionar un argumento *args* cuando llame a este método, lo que le permite pasar los argumentos al siguiente paso de la cascada. |

Cada paso debe llamar a uno de los métodos siguientes antes de volver: el delegado *next()* o uno de los métodos del contexto de diálogo, *begin*, *end*, *prompt* o *replace*; de lo contrario, el bot se detendrá en ese paso. Es decir, si una función no finaliza con uno de estos métodos, todas las intervenciones de los usuarios harán que se vuelva a ejecutar este paso cada vez que el usuario envía al bot un mensaje.

Cuando se alcanza el final de la cascada, es recomendable volver con el método _end_ para que el diálogo se pueda sacar de la pila. Consulte la sección [Finalización de un diálogo](#end-a-dialog) para más información. Igualmente, para continuar desde un paso al siguiente, el paso de cascada debe finalizar con un aviso o llamar explícitamente al delegado _next_ para avanzar por la cascada.

## <a name="start-a-dialog"></a>Inicio de un diálogo

Para iniciar un diálogo, pase el valor de *dialogId* que desea iniciar en el método _begin_, _prompt_ o _replace_ del contexto de diálogo. El método _begin_ insertará el diálogo al principio de la pila, mientras que el método _replace_ sacará el diálogo actual de la pila e insertará el diálogo sustituto en ella.

Para iniciar un diálogo sin argumentos:

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

```csharp
// Start the greetings dialog.
await dc.Begin("greetings");
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

```javascript
// Start the 'greetings' dialog.
await dc.begin('greetings');
```

---

Para iniciar un diálogo con argumentos:

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

```csharp
// Start the greetings dialog, passing in a property bag.
await dc.Begin("greetings", args);
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

```javascript
// Start the 'greetings' dialog with the 'userName' passed in.
await dc.begin('greetings', userName);
```

---

Para iniciar un diálogo de **mensajes**:

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

En este caso, **Inputs.Text** contiene el identificador de un **TextPrompt**, que está en el mismo conjunto de diálogos.

```csharp
// Ask a user for their name.
await dc.Prompt(Inputs.Text, "What is your name?");
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

```javascript
// Ask a user for their name.
await dc.prompt('textPrompt', "What is your name?");
```

---

Según el tipo de símbolo de mensaje que inicie, la signatura del argumento del mensaje puede ser diferente. El método **DialogSet.prompt** es un método auxiliar. Este método toma argumentos y construye las opciones adecuadas para el mensaje; a continuación, llama al método **begin** para iniciar el diálogo de mensajes. Para más información sobre los avisos, consulte [Petición de datos de entrada al usuario](bot-builder-prompts.md).

## <a name="end-a-dialog"></a>Finalización de un diálogo

Para finalizar un diálogo, el método _end_ lo retira de la pila y devuelve un resultado opcional al diálogo primario.

Es recomendable llamar explícitamente al método _end_ al final del diálogo; sin embargo, no es obligatorio porque el diálogo se saca automáticamente de la pila cuando se llega al final de la cascada.

Para finalizar un diálogo:

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

```csharp
// End the current dialog by popping it off the stack.
await dc.End();
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

```javascript
// End the current dialog by popping it off the stack
await dc.end();
```

---

Para finalizar un diálogo y devolver información al diálogo primario, incluya un argumento de bolsa de propiedades.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

```csharp
// End the current dialog and return information to the parent dialog.
await dc.end(new Dictionary<string, object>
    {
        ["property1"] = value1,
        ["property2"] = value2
    });
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

```javascript
// End the current dialog and pass a result to the parent dialog
await dc.end({
    "property1": value1,
    "property2": value2
});
```

---

## <a name="clear-the-dialog-stack"></a>Borrado de la pila de diálogos

Si quiere sacar todos los diálogos de la pila, llame al método _end all_ para borrar la pila de diálogos.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

```csharp
// Pop all dialogs from the current stack.
await dc.EndAll();
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

```javascript
// Pop all dialogs from the current stack.
await dc.endAll();
```

---

## <a name="repeat-a-dialog"></a>Repetición de un diálogo

Para repetir un diálogo, use el método _replace_. El método *replace* del contexto de diálogo retira el diálogo actual de la pila, inserta el diálogo sustituto en la parte superior de la pila y comienza dicho diálogo. Es una excelente manera de controlar [flujos de conversación complejos](~/v4sdk/bot-builder-dialog-manage-complex-conversation-flow.md) y una buena técnica para administrar menús.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

```csharp
// End the current dialog and start the main menu dialog.
await dc.Replace("mainMenu");
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

```javascript
// End the current dialog and start the 'mainMenu' dialog.
await dc.replace('mainMenu');
```

---

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a administrar flujos de conversación simples, consulte cómo se puede aprovechar el método _replace_ para controlar flujos de conversación complejos.

> [!div class="nextstepaction"]
> [Administración de flujos de conversación complejos](bot-builder-dialog-manage-complex-conversation-flow.md)
