---
title: Petición de datos de entrada a los usuarios con la biblioteca Dialogs | Microsoft Docs
description: Obtenga información sobre cómo solicitar datos de entrada a los usuarios en Bot Builder SDK para Node.js.
keywords: mensajes, cuadros de diálogo, AttachmentPrompt, ChoicePrompt, ConfirmPrompt, DatetimePrompt, NumberPrompt, TextPrompt, volver a solicitar, validación
author: v-ducvo
ms.author: v-ducvo
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 4/10/2018
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: 0b238ed510fd1d6fda82734af373f344b0dc28e3
ms.sourcegitcommit: 2dc75701b169d822c9499e393439161bc87639d2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42905369"
---
# <a name="prompt-users-for-input-using-the-dialogs-library"></a>Petición de datos de entrada a los usuarios con la biblioteca Dialogs

[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]

A menudo los bots recopilan su información a través de preguntas que se realizan a los usuarios. Puede enviar un mensaje estándar al usuario mediante el método _send activity_ del objeto del [contexto de turno](bot-builder-concept-activity-processing.md#turn-context) para pedir una cadena de entrada; sin embargo, Bot Builder SDK ofrece la biblioteca **Dialogs** que se puede usar para solicitar otros tipos de información. En este tema se detalla cómo usar **preguntas** para pedir entradas al usuario.

En este artículo se describe cómo usar las preguntas dentro de un cuadro de diálogo. Para obtener información sobre el uso de los diálogos en general, consulte [Uso de diálogos para administrar un flujo de conversación simple](bot-builder-dialog-manage-conversation-flow.md).

## <a name="prompt-types"></a>Tipos de avisos

La biblioteca de cuadros de diálogo ofrece una serie de tipos de preguntas, cada una para solicitar un tipo de respuesta diferente.

| Prompt | DESCRIPCIÓN |
|:----|:----|
| **AttachmentPrompt** | Solicita al usuario un archivo adjunto como un documento o una imagen. |
| **ChoicePrompt** | Solicita al usuario que elija entre un conjunto de opciones. |
| **ConfirmPrompt** | Solicita al usuario que confirme su acción. |
| **DatetimePrompt** | Solicita al usuario una fecha y una hora. Los usuarios puedan responder con lenguaje natural, como "Mañana a las 8 p.m." o "Viernes a las 10 a.m.". El SDK de Bot Framework usa la entidad precompilada `builtin.datetimeV2` de LUIS. Para obtener más información, vea [builtin.datetimev2](https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-prebuilt-entities#builtindatetimev2). |
| **NumberPrompt** | Solicita al usuario un número. El usuario puede responder con "10" o "diez". Si la respuesta es "diez", por ejemplo, la pregunta convertirá la respuesta en un número y devolverá `10` como resultado. |
| **TextPrompt** | Solicita al usuario una cadena de texto. |

## <a name="add-references-to-prompt-library"></a>Agregar referencias a la biblioteca de preguntas

Puede obtener la biblioteca **dialogs** si agrega el paquete **dialogs** al bot. Trataremos los diálogos en [Uso de diálogos para administrar un flujo de conversación simple](bot-builder-dialog-manage-conversation-flow.md), pero vamos a usar diálogos en nuestros avisos.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

Instale el paquete **Microsoft.Bot.Builder.Dialogs** de NuGet.

Después, incluya la referencia a la biblioteca desde el código del bot.

```cs
using Microsoft.Bot.Builder.Dialogs;
```

Puede definir un cuadro de diálogo como una clase o una propiedad insertada en el archivo de código del bot.

El código de este artículo está escrito para un cuadro de diálogo que se define como una clase.
En los ejemplos siguientes se supone que va a agregar código al constructor del cuadro de diálogo.

El flujo principal del cuadro de diálogo es su colección de pasos y se le debe proporcionar un identificador. El bot usa este identificador para recuperar el cuadro de diálogo, por lo que es una buena práctica exponerlo como una constante.

```cs
public class MyDialog : DialogSet
{
    public const string Name = "mainDialog";

    public MyDialog()
    {
        // Define your dialog's prompts and steps here.
    }
}
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

Instale el paquete dialogs desde NPM:

```cmd
npm install --save botbuilder-dialogs@preview
```

Para usar **dialogs** en el bot, inclúyalo en el código del bot.

En el archivo app.js, agregue lo siguiente.

```javascript
const {DialogSet} = require("botbuilder-dialogs");
const dialogs = new DialogSet();
```

---

## <a name="prompt-the-user"></a>Preguntar al usuario

Para solicitar entradas al usuario, puede agregar una pregunta al cuadro de diálogo. Por ejemplo, puede definir una pregunta de tipo **TextPrompt** y asignarle un identificador de cuadro de diálogo de **textPrompt**:

Después de agregar un cuadro de diálogo de pregunta, se puede usar en un cuadro de diálogo de cascada de dos pasos simple o usar varias preguntas juntas en una cascada de varios pasos. Un cuadro de diálogo de *cascada* es simplemente una manera de definir una secuencia de pasos. Para más información, consulte la sección [Uso de diálogos](bot-builder-dialog-manage-conversation-flow.md#using-dialogs-to-guide-the-user-through-steps) en [Uso de diálogos para administrar un flujo de conversación simple](bot-builder-dialog-manage-conversation-flow.md).

En el primer turno, el cuadro de diálogo solicita el nombre al usuario y, en el segundo, procesa la entrada del usuario como respuesta a la solicitud.

Por ejemplo, en el cuadro de diálogo siguiente se le pide el nombre al usuario y, después, se le saluda por su nombre:

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

Cada pregunta que se usa en el cuadro de diálogo también tiene un nombre, que el cuadro de diálogo usa en el bot para acceder a la pregunta. En todos estos ejemplos, los identificadores de pregunta se exponen como constantes.

Una llamada al método **Prompt** o **End** del contexto del cuadro de diálogo señala el final del paso al cuadro de diálogo. Sin estas instrucciones, el cuadro de diálogo no se ejecutará correctamente.

```csharp
/// <summary>Defines a simple greeting dialog that asks for the user's name.</summary>
public class MyDialog : DialogSet
{
    /// <summary>The ID of the main dialog in the set.</summary>
    public const string Name = "mainDialog";

    /// <summary>Defines the IDs of the prompts in the set.</summary>
    public struct Inputs
    {
        /// <summary>The ID of the text prompt.</summary>
        public const string Text = "textPrompt";
    }

    /// <summary>Defines the prompts and steps of the dialog.</summary>
    public MyDialog()
    {
        Add(Inputs.Text, new TextPrompt());
        Add(Name, new WaterfallStep[]
        {
            // Each step takes in a dialog context, arguments, and the next delegate.
            async (dc, args, next) =>
            {
                // Prompt for the user's name.
                await dc.Prompt(Inputs.Text, "What is your name?");
            },
            async(dc, args, next) =>
            {
                var user = (string)args["Text"];
                await dc.Context.SendActivity($"Hi {user}!");
                await dc.End();
            }
        });
    }
}
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

```javascript
const {TextPrompt} = require("botbuilder-dialogs");
```

```javascript
// Greet user:
// Ask for the user name and then greet them by name.
dialogs.add('textPrompt', new TextPrompt());
dialogs.add('greetings', [
    async function (dc){
        await dc.prompt('textPrompt', 'What is your name?');
    },
    async function(dc, userName){
        await dc.context.sendActivity(`Hi ${userName}!`);
        await dc.end();
    }
]);
```

---

> [!NOTE]
> Para iniciar un cuadro de diálogo, obtenga un contexto de cuadro de diálogo y use su método _begin_. Para más información, consulte [Uso de diálogos para administrar un flujo de conversación simple](./bot-builder-dialog-manage-conversation-flow.md).

## <a name="reusable-prompts"></a>Preguntas reutilizables

Una pregunta se puede reutilizar para solicitar otra información con el mismo tipo de pregunta. Por ejemplo, en el código de ejemplo anterior se definía una pregunta de texto y se usaba para preguntarle el nombre al usuario. Si quisiera, por ejemplo, también podría usar esa misma pregunta para pedir al usuario otra cadena de texto, como "Where do you work?" (¿Dónde trabajas?).

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

```cs
/// <summary>Defines a simple greeting dialog that asks for the user's name and place of work.</summary>
public class MyDialog : DialogSet
{
    /// <summary>The ID of the main dialog in the set.</summary>
    public const string Name = "mainDialog";

    /// <summary>Defines the IDs of the prompts in the set.</summary>
    public struct Inputs
    {
        /// <summary>The ID of the text prompt.</summary>
        public const string Text = "textPrompt";
    }

    /// <summary>Defines the prompts and steps of the dialog.</summary>
    public MyDialog()
    {
        Add(Inputs.Text, new TextPrompt());
        Add(Name, new WaterfallStep[]
        {
            async (dc, args, next) =>
            {
                // Prompt for the user's name.
                await dc.Prompt(Inputs.Text, "What is your name?");
            },
            async(dc, args, next) =>
            {
                var user = (string)args["Text"];

                // Ask them where they work.
                await dc.Prompt(Inputs.Text, $"Hi {user}! Where do you work?");
            },
            async(dc, args, next) =>
            {
                var workplace = (string)args["Text"];

                await dc.Context.SendActivity($"{workplace} is a cool place!");
                await dc.End();
            }
        });
    }
}
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

```javascript
// Greet user:
// Ask for the user name and then greet them by name.
// Ask them where they work.
dialogs.add('textPrompt', new botbuilder_dialogs.TextPrompt());
dialogs.add('greetings',[
    async function (dc){
        await dc.prompt('textPrompt', 'What is your name?');
    },
    async function(dc, userName){
        await dc.context.sendActivity(`Hi ${userName}!`);

        // Ask them where they work.
        await dc.prompt('textPrompt', 'Where do you work?');
    },
    async function(dc, workPlace){
        await dc.context.sendActivity(`${workPlace} is a cool place!`);

        await dc.end();
    }
]);
```

---

Pero si quiere emparejar la pregunta con el valor esperado que se solicita, puede asignar a cada pregunta un valor *dialogId* único. Un cuadro de diálogo se agrega con un identificador único. Al usar otros identificadores, también se pueden crear varios cuadros de diálogo **prompt** del mismo tipo. Por ejemplo, se podrían crear dos cuadros de diálogo **TextPrompt** para el ejemplo anterior:

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

```cs
/// <summary>The ID of the main dialog in the set.</summary>
public const string Name = "mainDialog";

/// <summary>Defines the IDs of the prompts in the set.</summary>
public struct Inputs
{
    /// <summary>The ID of the name prompt.</summary>
    public const string Name = "namePrompt";

    /// <summary>The ID of the work prompt.</summary>
    public const string Work = "workPrompt";
}

/// <summary>Defines the prompts and steps of the dialog.</summary>
public MyDialog()
{
    Add(Inputs.Name, new TextPrompt());
    Add(Inputs.Work, new TextPrompt());
    Add(Name, new WaterfallStep[]
    {
        async (dc, args, next) =>
        {
            // Prompt for the user's name.
            await dc.Prompt(Inputs.Name, "What is your name?");
        },
        async(dc, args, next) =>
        {
            var user = (string)args["Text"];

            // Ask them where they work.
            await dc.Prompt(Inputs.Work, $"Hi {user}! Where do you work?");
        },
        async(dc, args, next) =>
        {
            var workplace = (string)args["Text"];

            await dc.Context.SendActivity($"{workplace} is a cool place!");
            await dc.End();
        }
    });
}
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

```javascript
dialogs.add('namePrompt', new TextPrompt());
dialogs.add('workPlacePrompt', new TextPrompt());
```

---

Para la reutilización del código, la definición de un solo `textPrompt` funcionaría para todas estas preguntas porque preguntan por una cadena de texto como respuesta. Pero la capacidad de asignar nombres a los cuadros de diálogo resulta útil cuando es necesario validar la entrada de la pregunta. En este caso, es posible que las preguntas usen **TextPrompt** pero que cada una busque un conjunto de valores diferente. Veamos cómo se pueden validar las respuestas de las preguntas mediante un `NumberPrompt`.

## <a name="specify-prompt-options"></a>Especificar las opciones de las preguntas

Cuando se usa una pregunta en un paso de cuadro de diálogo, también se pueden proporcionar opciones de pregunta, como una cadena de nueva pregunta.

Especificar una cadena de nueva pregunta es útil cuando la entrada del usuario no puede satisfacer una pregunta, ya sea porque está en un formato que no se puede analizar, como "mañana" para una pregunta de símbolo, o bien porque la entrada no cumpla un criterio de validación.

> [!TIP]
> Cuando se crea una pregunta de número, es necesario especificar la referencia cultural de entrada que va a usar. La pregunta de número puede interpretar una amplia variedad de entradas, "doce" o "un cuarto", así como "12" y "0,25". La referencia cultural de la entrada ayuda a que la pregunta interprete la entrada del usuario más correctamente.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

Las referencias culturales de las entradas se definen en una biblioteca adicional.

```csharp
using Microsoft.Bot.Builder.Dialogs;
using Microsoft.Recognizers.Text;
```

En el código siguiente se agregaría una pregunta de número a un conjunto de cuadros de diálogo existente, **dialogs**.

```csharp
dialogs.Add("numberPrompt", new NumberPrompt<int>(Culture.English));
```

En un paso de cuadro de diálogo, el código siguiente podría solicitar entradas al usuario y proporcionar una cadena de nueva pregunta para usarla en caso de que la entrada no se pudiera interpretar como un número.

```csharp
await dc.Prompt("numberPrompt", "How many people are in your party?", new PromptOptions()
{
    RetryPromptString = "Sorry, please specify the number of people in your party."
});
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

```javascript
const {NumberPrompt} = require("botbuilder-dialogs");
```

```javascript
await dc.prompt('numberPrompt', 'How many people in your party?', { retryPrompt: `Sorry, please specify the number of people in your party.` })
```

```javascript
dialogs.add('numberPrompt', new NumberPrompt());
```

---

En concreto, la pregunta de opciones requiere información adicional, la lista de opciones disponibles para el usuario.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

En este ejemplo se usan tipos de los espacios de nombres siguientes.

```csharp
using Microsoft.Bot.Builder.Core.Extensions;
using Microsoft.Bot.Builder.Dialogs;
using Microsoft.Bot.Builder.Prompts.Choices;
using Microsoft.Bot.Schema;
using Microsoft.Recognizers.Text;
using System.Collections.Generic;
```


Cuando se usa **ChoicePrompt** para pedir al usuario que elija entre un conjunto de opciones, se debe proporcionar a la pregunta ese conjunto de opciones, dentro de un objeto **ChoicePromptOptions**. En este caso, se usa **ChoiceFactory** para convertir una lista de opciones en un formato adecuado.

También se usa una actividad **SuggestedActions** como nueva pregunta, como una manera de volver a proporcionar las opciones de entrada al usuario.


```csharp
/// <summary>Defines a dialog that asks for a choice of color.</summary>
public class MyDialog : DialogSet
{
    /// <summary>The ID of the main dialog in the set.</summary>
    public const string Name = "mainDialog";

    /// <summary>Defines the IDs of the prompts in the set.</summary>
    public struct Inputs
    {
        /// <summary>The ID of the color prompt.</summary>
        public const string Color = "colorPrompt";
    }

    /// <summary>The available colors to choose from.</summary>
    public List<string> Colors = new List<string> { "Green", "Blue" };

    /// <summary>Defines the prompts and steps of the dialog.</summary>
    public MyDialog()
    {
        Add(Inputs.Color, new ChoicePrompt(Culture.English));
        Add(Name, new WaterfallStep[]
        {
            async (dc, args, next) =>
            {
                // Prompt for a color. A choice prompt requires that you specify choice options.
                await dc.Prompt(Inputs.Color, "Please make a choice.", new ChoicePromptOptions()
                {
                    Choices = ChoiceFactory.ToChoices(Colors),
                    RetryPromptActivity =
                        MessageFactory.SuggestedActions(Colors, "Please choose a color.") as Activity
                });
            },
            async(dc, args, next) =>
            {
                var color = (FoundChoice)args["Value"];

                await dc.Context.SendActivity($"You chose {color.Value}.");
                await dc.End();
            }
        });
    }
}
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

```javascript
const {ChoicePrompt} = require("botbuilder-dialogs");
```

```javascript
dialogs.add('choicePrompt', new ChoicePrompt());
```

```javascript
// A choice prompt requires that you specify choice options.
const list = ['green', 'blue'];
await dc.prompt('choicePrompt', 'Please make a choice', list, {retryPrompt: 'Please choose a color.'});
```

---

## <a name="validate-a-prompt-response"></a>Validar la respuesta de una pregunta

Puede validar la respuesta de una pregunta antes de devolver el valor válido al paso siguiente de la **cascada**. Por ejemplo, para validar un **NumberPrompt** dentro de un intervalo de números entre **6** y **20**, puede tener lógica de validación similar a la siguiente:

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

```cs
using Microsoft.Bot.Builder.Dialogs;
using Microsoft.Recognizers.Text;
using PromptStatus = Microsoft.Bot.Builder.Prompts.PromptStatus;
```

```cs
/// <summary>Defines a dialog that asks for the number of people in a party.</summary>
public class MyDialog : DialogSet
{
    /// <summary>The ID of the main dialog in the set.</summary>
    public const string Name = "mainDialog";

    /// <summary>Defines the IDs of the prompts in the set.</summary>
    public struct Inputs
    {
        /// <summary>The ID of the party size prompt.</summary>
        public const string Size = "parytySize";
    }

    /// <summary>Defines the prompts and steps of the dialog.</summary>
    public MyDialog()
    {
        // Include a validation function for the party size prompt.
        Add(Inputs.Size, new NumberPrompt<int>(Culture.English, async (context, result) =>
        {
            if (result.Value < 6 || result.Value > 20)
            {
                result.Status = PromptStatus.OutOfRange;
            }
        }));
        Add(Name, new WaterfallStep[]
        {
            async (dc, args, next) =>
            {
                // Prompt for the party size.
                await dc.Prompt(Inputs.Size, "How many people are in your party?", new PromptOptions()
                {
                    RetryPromptString = "Please specify party size between 6 and 20."
                });
            },
            async(dc, args, next) =>
            {
                var size = (int)args["Value"];

                await dc.Context.SendActivity($"Okay, {size} people!");
                await dc.End();
            }
        });
    }
}
```

La validación también se puede encapsular dentro de su propio método privado, y agregarse de esa forma.

```cs
/// <summary>Validates input for the partySize prompt.</summary>
/// <param name="context">The context object for the current turn of the bot.</param>
/// <param name="result">The recognition result from the prompt.</param>
/// <returns>An updated recognition result.</returns>
private static async Task PartySizeValidator(ITurnContext context, Int32Result result)
{
    if (result.Value < 6 || result.Value > 20)
    {
        result.Status = PromptStatus.OutOfRange;
    }
}
```

En el cuadro de diálogo, especifique el método que se va a usar para validar la entrada.

```cs
// Include a validation function for the party size prompt.
Add(Inputs.Size, new NumberPrompt<int>(Culture.English, PartySizeValidator));
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

```javascript
// Customized prompts with validations
// A number prompt with validation for valid party size within a range.
dialogs.add('partySizePrompt', new botbuilder_dialogs.NumberPrompt( async (context, value) => {
    try {
        if(value < 6 ){
            throw new Error('Party size too small.');
        }
        else if(value > 20){
            throw new Error('Party size too big.')
        }
        else {
            return value; // Return the valid value
        }
    } catch (err) {
        await context.sendActivity(`${err.message} <br/>Please provide a valid number between 6 and 20.`);
        return undefined;
    }
}));
```

---

Del mismo modo, si quiere validar una respuesta **DatetimePrompt** para una fecha y hora en el futuro, puede tener lógica de validación similar a la siguiente:

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

```cs
using Microsoft.Bot.Builder;
using Microsoft.Bot.Builder.Dialogs;
using Microsoft.Recognizers.Text;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using DateTimeResult = Microsoft.Bot.Builder.Prompts.DateTimeResult;
using PromptStatus = Microsoft.Bot.Builder.Prompts.PromptStatus;
```

```cs
/// <summary>Validates input for the reservationTime prompt.</summary>
/// <param name="context">The context object for the current turn of the bot.</param>
/// <param name="result">The recognition result from the prompt.</param>
/// <returns>An updated recognition result.</returns>
private static async Task TimeValidator(ITurnContext context, DateTimeResult result)
{
    if (result.Resolution.Count == 0)
    {
        await context.SendActivity("Sorry, I did not recognize the time that you entered.");
        result.Status = PromptStatus.NotRecognized;
    }

    // Find any recognized time that is not in the past.
    var now = DateTime.Now;
    DateTime time = default(DateTime);
    var resolution = result.Resolution.FirstOrDefault(
        res => DateTime.TryParse(res.Value, out time) && time > now);

    if (resolution != null)
    {
        // If found, keep only that result.
        result.Resolution.Clear();
        result.Resolution.Add(resolution);
    }
    else
    {
        // Otherwise, flag the input as out of range.
        await context.SendActivity("Please enter a time in the future, such as \"tomorrow at 9am\"");
        result.Status = PromptStatus.OutOfRange;
    }
}
```

```csharp
Add(Inputs.Time, new DateTimePrompt(Culture.English, TimeValidator));
```

Encontrará más ejemplos en el [repositorio de ejemplos](https://github.com/Microsoft/botbuilder-dotnet).

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

```JavaScript
// A date and time prompt with validation for date/time in the future.
dialogs.add('dateTimePrompt', new botbuilder_dialogs.DatetimePrompt( async (context, values) => {
    try {
        if (values.length < 0) { throw new Error('missing time') }
        if (values[0].type !== 'datetime') { throw new Error('unsupported type') }
        const value = new Date(values[0].value);
        if (value.getTime() < new Date().getTime()) { throw new Error('in the past') }
        return value;
    } catch (err) {
        await context.sendActivity(`Please enter a valid time in the future like "tomorrow at 9am".`);
        return undefined;
    }
}));
```

Encontrará más ejemplos en el [repositorio de ejemplos](https://github.com/Microsoft/botbuilder-js).

---

> [!TIP]
> Las preguntas de fecha y hora se pueden resolver en varias fechas diferentes si el usuario proporciona una respuesta ambigua. En función del uso previsto, puede que le interese comprobar todas las resoluciones proporcionadas por el resultado de la pregunta, en lugar de simplemente la primera.

Se pueden usar técnicas similares para validar las respuestas para cualquiera de los tipos de pregunta.

## <a name="save-user-data"></a>Guardar los datos del usuario

Al solicitar entradas al usuario, tiene varias opciones sobre cómo controlarlas. Por ejemplo, puede consumir y descartar la entrada, guardarla en una variable global, guardarla en un contenedor de almacenamiento volátil o en memoria, guardarla en un archivo, o bien en una base de datos externa. Para obtener más información sobre cómo guardar los datos del usuario, vea [Administración de los datos del usuario](bot-builder-howto-v4-state.md).

## <a name="next-steps"></a>Pasos siguientes

Ahora que sabe cómo solicitar entradas al usuario, vamos a mejorar la experiencia de usuario y el código de bot mediante la administración de varios flujos de conversación a través de cuadros de diálogo.


