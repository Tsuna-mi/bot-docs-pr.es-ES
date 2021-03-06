---
title: Creación de un conjunto de diálogos integrado | Microsoft Docs
description: Aprenda a modularizar su lógica de bot mediante el contenedor de diálogos de Bot Builder SDK para Node.js y C#.
keywords: control compuesto, lógica de bot modular
author: v-ducvo
ms.author: v-ducvo
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 4/27/2018
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: b0ce3e6d11d20985e23c1bb74bd5b9dce8190d0c
ms.sourcegitcommit: 3bf3dbb1a440b3d83e58499c6a2ac116fe04b2f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/23/2018
ms.locfileid: "46707161"
---
# <a name="create-an-integrated-set-of-dialogs"></a>Creación de un conjunto de diálogos integrado

[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]

Imagine que va a crear un bot de hotel que controla varias tareas como, por ejemplo, saludar al usuario, reservar una mesa para cenar, encargar comida, configurar una alarma, mostrar el tiempo actual, etc. Puede controlar cada una de estas tareas del bot con un objeto de diálogo, pero esto puede hacer que el código del diálogo sea demasiado grande y desordenado en el archivo principal del bot.

Puede resolver este problema mediante la modularización. La modularización simplificará el código y hará que sea más fácil su depuración. Además, puede dividirlo en secciones con lo cual puede hacer que varios equipos trabajen a la vez en el mismo bot. Podemos crear un bot que administre varios flujos de conversación dividiéndolos en componentes mediante un contenedor de diálogos. Vamos a crear varios flujos básicos de conversación y a mostrar cómo se pueden combinar mediante un contenedor de diálogos.

En este ejemplo vamos a crear un bot de hotel que combina los módulos de registro, despertador y reserva de mesa.

## <a name="managing-state"></a>Administración de estados

Hay muchas maneras de configurar la administración de estados de un bot que usa diálogos compuestos. En este bot, se muestra una manera de hacerlo.

Para más información acerca de la administración de estados, consulte el artículo [Guardar el estado mediante las propiedades del usuario y la conversación](bot-builder-howto-v4-state.md).

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

Cada diálogo recopilará algo de información y esta se guardará en el estado del usuario. Vamos a definir una clase para cada diálogo y a usar esas clases como propiedades en nuestra clase de estado de usuario.

```csharp
/// <summary>
/// State information associated with the check-in dialog.
/// </summary>
public class GuestInfo
{
    public string Name { get; set; }
    public string Room { get; set; }
}

/// <summary>
/// State information associated with the reserve-table dialog.
/// </summary>
public class TableInfo
{
    public string Number { get; set; }
}

/// <summary>
/// State information associated with the wake-up call dialog.
/// </summary>
public class WakeUpInfo
{
    public string Time { get; set; }
}

/// <summary>
/// User state information.
/// </summary>
public class UserInfo
{
    public GuestInfo Guest { get; set; }
    public TableInfo Table { get; set; }
    public WakeUpInfo WakeUp { get; set; }
}
```

En un turno de bot, el método `CreateContext` del conjunto de diálogos establece el estado del diálogo.
El método toma el [contexto de turno](bot-builder-concept-activity-processing.md#turn-context) y un objeto de estado como parámetros.

En el caso de los diálogos, este objeto de estado debe implementar la interfaz `IDictionary<string, object>`. Puesto que este bot solo usa el estado de conversación para alojar el estado del diálogo, nuestra clase de estado de conversación puede ser un simple diccionario.

```csharp
using System.Collections.Generic;

/// <summary>
/// Conversation state information.
/// We are also using this directly for dialog state, which needs an <see cref="IDictionary{string, object}"/> object.
/// </summary>
public class ConversationInfo : Dictionary<string, object> { }
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

Para realizar un seguimiento de la entrada del usuario, se va a pasar un objeto `userInfo` desde el diálogo principal como parámetro de diálogo. En cada clase de diálogo, se va a integrar el diálogo en el constructor, lo cual le permite guardar información en `userInfo`. A lo largo de este diálogo, podemos escribir en un objeto de estado local definido como propiedad en el objeto `step.values` a medida que el usuario introduce la información. Una vez completado el diálogo, se eliminará el objeto de estado local. Por lo tanto, se guardará el objeto de estado local en el `userInfo` principal, lo cual ayudará a conservar la información acerca del usuario en todas las conversaciones que tenga con este. 

---

## <a name="define-a-modular-check-in-dialog"></a>Definición de un diálogo de registro modular

En primer lugar, vamos a empezar con un sencillo diálogo de registro que pedirá al usuario su nombre y el número de habitación en la que se alojará. Para modularizar esta tarea, vamos a crear una clase `CheckIn` que extienda `ComponentDialog`. Esta clase tiene un constructor que define el nombre del diálogo de raíz que, a su vez, se define como una *cascada* con tres pasos. La firma y construcción del objeto de diálogo es exactamente la misma que la de una cascada estándar.

**Pasos del diálogo de registro**

1. Pida el nombre del huésped.
1. Pregunte por la habitación en la que le gustaría permanecer.
1. Envíe un mensaje de confirmación y complete el diálogo.

Para más información sobre diálogos y cascadas, consulte [Uso de diálogos para administrar un flujo de conversación simple](bot-builder-dialog-manage-conversation-flow.md).

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

La clase `CheckIn` tiene un constructor privado que define los pasos de nuestro diálogo de registro, crea una única instancia y la expone en una propiedad estática `Instance`.

A lo largo de este diálogo podemos escribir en un objeto de estado local, al que se puede acceder mediante una propiedad del contexto de paso, `step.values`. Una vez completado el diálogo, se elimina el objeto de estado local. Por tanto, vamos a guardar el objeto de estado local en el `userState` del bot, lo cual ayudará a conservar la información acerca del usuario en todas las conversaciones que tenga con este.

Para más información acerca de la administración de estados, consulte el artículo [Guardar el estado mediante las propiedades del usuario y la conversación](bot-builder-howto-v4-state.md). 

**CheckIn.cs**
```csharp
using Microsoft.Bot.Builder.Core.Extensions;
using Microsoft.Bot.Builder.Dialogs;
using Microsoft.Recognizers.Text;

namespace HotelBot
{
    public class CheckIn : DialogContainer
    {
        public const string Id = "checkIn";

        private const string GuestKey = nameof(CheckIn);

        public static CheckIn Instance { get; } = new CheckIn();

        // You can start this from the parent using the dialog's ID.
        private CheckIn() : base(Id)
        {
            // Define the conversation flow using a waterfall model.
            this.Dialogs.Add(Id, new WaterfallStep[]
            {
                async (dc, args, next) =>
                {
                    // Clear the guest information and prompt for the guest's name.
                    dc.ActiveDialog.State[GuestKey] = new GuestInfo();
                    await dc.Prompt("textPrompt", "What is your name?");
                },
                async (dc, args, next) =>
                {
                    // Save the name and prompt for the room number.
                    var name = args["Value"] as string;

                    var guestInfo = dc.ActiveDialog.State[GuestKey];
                    ((GuestInfo)guestInfo).Name = name;

                    await dc.Prompt("numberPrompt",
                        $"Hi {name}. What room will you be staying in?");
                },
                async (dc, args, next) =>
                {
                    // Save the room number and "sign off".
                    var room = (string)args["Text"];

                    var guestInfo = dc.ActiveDialog.State[GuestKey];
                    ((GuestInfo)guestInfo).Room = room;

                    await dc.Context.SendActivity("Great, enjoy your stay!");

                    // Save dialog state to user state and end the dialog.
                    var userState = UserState<UserInfo>.Get(dc.Context);
                    userState.Guest = (GuestInfo)guestInfo;

                    await dc.End();
                }
            });

            // Define the prompts used in this conversation flow.
            this.Dialogs.Add("textPrompt", new TextPrompt());
            this.Dialogs.Add("numberPrompt", new NumberPrompt<int>(Culture.English));
        }
    }
}
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

**checkIn.js**
```JavaScript
const { ComponentDialog, DialogSet, TextPrompt, NumberPrompt, WaterfallDialog } = require('botbuilder-dialogs');

class CheckIn extends ComponentDialog {
    constructor(dialogId, userInfo) {
        // Dialog ID of 'checkIn' will start when class is called in the parent
        super(dialogId);

        // Defining the conversation flow using a waterfall model
        this.dialogs.add(new WaterfallDialog('checkIn', [
            async function (step) {
                // Create a new local guestInfo step.values object
                step.values.guestInfo = {};
                return await step.prompt('textPrompt', "What is your name?");
            },
            async function (step) {
                // Save the name 
                step.values.guestInfo.userName = step.result;
                return await step.prompt('numberPrompt', `Hi ${name}. What room will you be staying in?`);
            },
            async function (step) { 
                // Save the room number
                step.values.guestInfo.room = step.result;
                await step.context.sendActivity(`Great! Enjoy your stay!`);

                // Save dialog's state object to the parent's state object
                const user = await userInfo.get(step.context);
                user.guestInfo = step.values.guestInfo;
                // Save changes
                await userInfo.set(step.context, user);
                return await step.endDialog();
            }
        ]));
        
        // Defining the prompt used in this conversation flow
        this.dialogs.add(new TextPrompt('textPrompt'));
        this.dialogs.add(new NumberPrompt('numberPrompt'));
    }
}
exports.CheckIn = CheckIn;
```

---

## <a name="define-modular-reserve-table-and-wake-up-dialogs"></a>Definición de los diálogos modulares de reserva de mesa y de despertador

Una ventaja de usar un diálogo de componentes es la posibilidad de unir diálogos. Como cada `DialogSet` mantiene un conjunto exclusivo de `dialogs`, el uso compartido o la referencia cruzada de `dialogs` no se puede realizar fácilmente. Y es aquí donde entra en juego el diálogo de componentes. Puede usar diálogos de componentes para crear un diálogo compuesto que facilite la administración del flujo de conversación entre diálogos. A modo de ejemplo, vamos a crear dos diálogos más: uno para preguntar al usuario qué mesa le gustaría reservar para cenar y otro para crear una llamada de despertador. Una vez más, vamos a usar una clase aparte para cada diálogo y cada diálogo ampliará `ComponentDialog`.

**Pasos del diálogo de reserva de mesa**

1. Pregunte por la mesa que se va a reservar.
1. Envíe un mensaje de confirmación y complete el diálogo.

**Pasos del diálogo de despertador**

1. Pregunte la hora a la que desea que se le despierte.
1. Envíe un mensaje de confirmación y complete el diálogo.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

**ReserveTable.cs**
```csharp
using Microsoft.Bot.Builder.Core.Extensions;
using Microsoft.Bot.Builder.Dialogs;
using Microsoft.Recognizers.Text;
using System.Linq;
using Choice = Microsoft.Bot.Builder.Prompts.Choices.Choice;
using FoundChoice = Microsoft.Bot.Builder.Prompts.Choices.FoundChoice;

namespace HotelBot
{
    public class ReserveTable : DialogContainer
    {
        public const string Id = "reserveTable";

        private const string TableKey = "Table";

        public static ReserveTable Instance { get; } = new ReserveTable();

        private ReserveTable() : base(Id)
        {
            this.Dialogs.Add(Id, new WaterfallStep[]
            {
                async (dc, args, next) =>
                {
                    // Clear the table information and prompt for the table number.
                    dc.ActiveDialog.State[TableKey] = new TableInfo();

                    var guestInfo = UserState<UserInfo>.Get(dc.Context).Guest;

                    var prompt = $"Welcome {guestInfo.Name}, " +
                        $"which table would you like to reserve?";
                    var choices = new string[] { "1", "2", "3", "4", "5", "6" };
                    await dc.Prompt("choicePrompt", prompt,
                        new ChoicePromptOptions
                        {
                            Choices = choices.Select(s => new Choice { Value = s }).ToList()
                        });
                },
                async (dc, args, next) =>
                {
                    // Save the table number and "sign off".
                    var table = (args["Value"] as FoundChoice).Value;

                    var tableInfo = dc.ActiveDialog.State[TableKey];
                    ((TableInfo)tableInfo).Number = table;

                    await dc.Context.SendActivity(
                        $"Sounds great; we will reserve table number {table} for you.");

                    // Save dialog state to user state and end the dialog.
                    var userState = UserState<UserInfo>.Get(dc.Context);
                    userState.Table = (TableInfo)tableInfo;

                    await dc.End();
                }
            });

            // Define the prompts used in this conversation flow.
            this.Dialogs.Add("choicePrompt", new ChoicePrompt(Culture.English));
        }
    }
}
```

**WakeUp.cs**
```csharp
using Microsoft.Bot.Builder.Core.Extensions;
using Microsoft.Bot.Builder.Dialogs;
using Microsoft.Recognizers.Text;
using System.Collections.Generic;
using System.Linq;
using DateTimeResolution = Microsoft.Bot.Builder.Prompts.DateTimeResult.DateTimeResolution;

namespace HotelBot
{
    public class WakeUp : DialogContainer
    {
        public const string Id = "wakeUp";

        private const string WakeUpKey = "WakeUp";

        public static WakeUp Instance { get; } = new WakeUp();

        private WakeUp() : base(Id)
        {
            this.Dialogs.Add(Id, new WaterfallStep[]
            {
                async (dc, args, next) =>
                {
                    // Clear the wake up call information and prompt for the alarm time.
                   dc.ActiveDialog.State[WakeUpKey] = new WakeUpInfo();

                    var guestInfo = UserState<UserInfo>.Get(dc.Context).Guest;

                    await dc.Prompt("datePrompt", $"Hi {guestInfo.Name}, " +
                        $"what time would you like your alarm set for?");
                },
                async (dc, args, next) =>
                {
                    // Save the alarm time and "sign off".
                    var resolution = (List<DateTimeResolution>)args["Resolution"];

                    var wakeUp = dc.ActiveDialog.State[WakeUpKey];
                    ((WakeUpInfo)wakeUp).Time = resolution?.FirstOrDefault()?.Value;

                    var userState = UserState<UserInfo>.Get(dc.Context);
                    await dc.Context.SendActivity(
                        $"Your alarm is set to {((WakeUpInfo)wakeUp).Time}" +
                        $" for room {userState.Guest.Room}.");

                    // Save dialog state to user state and end the dialog.
                    userState.WakeUp = (WakeUpInfo)wakeUp;

                    await dc.End();
                }
            });

            // Define the prompts used in this conversation flow.
            this.Dialogs.Add("datePrompt", new DateTimePrompt(Culture.English));
        }
    }
}
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

**reserveTable.js**
```JavaScript
const { ComponentDialog, DialogSet, ChoicePrompt, WaterfallDialog } = require('botbuilder-dialogs');

class ReserveTable extends ComponentDialog {
    constructor(dialogId, userInfo) {
        super(dialogId); 

        // Defining the conversation flow using a waterfall model
        this.dialogs.add(new WaterfallDialog('reserve_table', [
            async function (step) {
                // Get the user state from context
                const user = await userInfo.get(step.context);

                // Create a new local reserveTable state object
                step.values.reserveTable = {};

                const prompt = `Welcome ${ user.guestInfo.userName }, which table would you like to reserve?`;
                const choices = ['1', '2', '3', '4', '5', '6'];
                return await step.prompt('choicePrompt', prompt, choices);
            },
            async function(step) {
                const choice = step.result;
                
                // Save the table number
                step.values.reserveTable.tableNumber = choice.value;
                await step.context.sendActivity(`Sounds great, we will reserve table number ${ choice.value } for you.`);
                
                // Get the user state from context
                const user = await userInfo.get(dc.context);
                // Persist dialog's state object to the parent's state object
                user.reserveTable = step.values.reserveTable;
               
                // Save changes
                await userInfo.set(step.context, user);

                // End the dialog
                return await step.endDialog();
            }
        ]));

        // Defining the prompt used in this conversation flow
        this.dialogs.add(new ChoicePrompt('choicePrompt'));
    }
}
exports.ReserveTable = ReserveTable;
```

**wakeUp.js**
```JavaScript
const { ComponentDialog, DialogSet, DatetimePrompt, WaterfallDialog } = require('botbuilder-dialogs');

class WakeUp extends ComponentDialog {
    constructor(userInfo) {
        // Dialog ID of 'wakeup' will start when class is called in the parent
        super('wakeUp');

        this.dialogs.add(new WaterfallDialog('wakeUp', [
            async function (step) {
                // Get the user state from context
                const user = await userInfo.get(step.context); 

                // Create a new local reserveTable state object
                step.values.wakeUp = {};  
                             
                return await step.prompt('datePrompt', `Hello, ${ user.guestInfo.userName }. What time would you like your alarm to be set?`);
            },
            async function (step) {
                const time = step.result;
            
                // Get the user state from context
                const user = await userInfo.get(step.context);

                // Save the time
                step.values.wakeUp.time = time[0].value

                await step.context.sendActivity(`Your alarm is set to ${ time[0].value } for room ${ user.guestInfo.room }`);
                
                // Save dialog's state object to the parent's state object
                user.wakeUp = step.values.wakeUp;

                // Save changes
                await userInfo.step(step.context, user);

                // End the dialog
                return await step.endDialog();
            }]));

        // Defining the prompt used in this conversation flow
        this.dialogs.add(new DatetimePrompt('datePrompt'));
    }
}
exports.WakeUp = WakeUp;
```

---

## <a name="add-modular-dialogs-to-a-bot"></a>Incorporación de diálogos modulares a un bot

El archivo principal del bot une estos tres diálogos modularizados en un bot.

- Los tres diálogos se agregan al conjunto de diálogos principal de nuestro bot.
- Los diálogos de reserva de mesa y de despertador se integran en el flujo de conversación del diálogo principal.
- Cuando se inicia una nueva conversación, no hay ningún diálogo activo y la lógica del turno de encendido del bot asume el control.

**Controlador del turno de encendido del bot**

Siempre que el turno del bot está fuera de un diálogo activo, este comprueba el estado del usuario.
1. Si ya tiene la información del huésped, inicia su diálogo principal.
1. En caso contrario, iniciará el diálogo secundario del principal de registro.

**Pasos del diálogo principal**

1. Preguntar al huésped qué desea hacer: reservar una mesa o configurar una llamada de despertador.
1. Iniciar el diálogo secundario correspondiente o enviar un mensaje de _entrada no comprendida_ y pasar al siguiente paso.
1. Volver al principio de este diálogo.


# <a name="ctabcsharp"></a>[C#](#tab/csharp)

El flujo de diálogo se actualiza con el método `Continue` del contexto del diálogo. Este método ejecuta el siguiente paso de la cascada en la pila de diálogo.

**HotelBot.cs**
```csharp
using System.Collections.Generic;
using System.Threading.Tasks;
using Microsoft.Bot;
using Microsoft.Bot.Builder;
using Microsoft.Bot.Builder.Core.Extensions;
using Microsoft.Bot.Builder.Dialogs;
using Microsoft.Bot.Schema;

namespace HotelBot
{
    public class HotelBot : IBot
    {
        private const string MainMenuId = "mainMenu";

        private DialogSet _dialogs { get; } = ComposeMainDialog();

        /// <summary>
        /// Every Conversation turn for our bot calls this method. 
        /// </summary>
        /// <param name="context">The current turn context.</param>        
        public async Task OnTurn(ITurnContext context)
        {
            if (context.Activity.Type is ActivityTypes.Message)
            {
                // Get the user and conversation state from the turn context.
                var userInfo = UserState<UserInfo>.Get(context);
                var conversationInfo = ConversationState<ConversationInfo>.Get(context);

                // Establish dialog state from the conversation state.
                var dc = _dialogs.CreateContext(context, conversationInfo);

                // Continue any current dialog.
                await dc.Continue();

                // Every turn sends a response, so if no response was sent,
                // then there no dialog is currently active.
                if (!context.Responded)
                {
                    // If we don't yet have the guest's info, start the check-in dialog.
                    if (string.IsNullOrEmpty(userInfo?.Guest?.Name))
                    {
                        await dc.Begin(CheckIn.Id);
                    }
                    // Otherwise, start our bot's main dialog.
                    else
                    {
                        await dc.Begin(MainMenuId);
                    }
                }
            }
        }

        /// <summary>
        /// Composes a main dialog for our bot.
        /// </summary>
        /// <returns>A new main dialog.</returns>
        private static DialogSet ComposeMainDialog()
        {
            var dialogs = new DialogSet();

            dialogs.Add(MainMenuId, new WaterfallStep[]
            {
                async (dc, args, next) =>
                {
                    var menu = new List<string> { "Reserve Table", "Wake Up" };
                    await dc.Context.SendActivity(MessageFactory.SuggestedActions(menu, "How can I help you?"));
                },
                async (dc, args, next) =>
                {
                    // Decide which dialog to start.
                    var result = (args["Activity"] as Activity)?.Text?.Trim().ToLowerInvariant();
                    switch (result)
                    {
                        case "reserve table":
                            await dc.Begin(ReserveTable.Id);
                            break;
                        case "wake up":
                            await dc.Begin(WakeUp.Id);
                            break;
                        default:
                            await dc.Context.SendActivity("Sorry, I don't understand that command. Please choose an option from the list below.");
                            await next();
                            break;
                    }
                },
                async (dc, args, next) =>
                {
                    // Show the main menu again.
                    await dc.Replace(MainMenuId);
                }
            });

            // Add our child dialogs.
            dialogs.Add(CheckIn.Id, CheckIn.Instance);
            dialogs.Add(ReserveTable.Id, ReserveTable.Instance);
            dialogs.Add(WakeUp.Id, WakeUp.Instance);

            return dialogs;
        }
    }
}
```

Por último, necesitamos actualizar el método `ConfigureServices` de la clase `StartUp` para conectar nuestro bot y agregar el middleware de estado.

**Startup.cs**
```csharp
// This method gets called by the runtime. Use this method to add services to the container.
public void ConfigureServices(IServiceCollection services)
{
    services.AddBot<HotelBot>(options =>
    {
        options.CredentialProvider = new ConfigurationCredentialProvider(Configuration);

        options.Middleware.Add(new CatchExceptionMiddleware<Exception>(async (context, exception) =>
        {
            await context.TraceActivity($"{nameof(HotelBot)} Exception", exception);
            await context.SendActivity("Sorry, it looks like something went wrong!");
        }));

        // Add state management middleware for conversation and user state.
        var path = Path.Combine(Path.GetTempPath(), nameof(HotelBot));
        if (!Directory.Exists(path))
        {
            Directory.CreateDirectory(path);
        }
        IStorage storage = new FileStorage(path);

        options.Middleware.Add(new ConversationState<ConversationInfo>(storage));
        options.Middleware.Add(new UserState<UserInfo>(storage));
    });
}
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

El flujo de diálogo se actualiza con el método `continue` del contexto del diálogo. Este método ejecuta el siguiente paso de la cascada en la pila de diálogo.

**app.js**
```JavaScript
const { BotFrameworkAdapter, ConversationState, UserState, MemoryStorage, MessageFactory } = require("botbuilder");
const { DialogSet } = require("botbuilder-dialogs");
const restify = require("restify");
var azure = require('botbuilder-azure'); 

// Create server
let server = restify.createServer();
server.listen(process.env.port || process.env.PORT || 3978, function () {
    console.log(`${server.name} listening to ${server.url}`);
});

// Create adapter
const adapter = new BotFrameworkAdapter({
    appId: process.env.MICROSOFT_APP_ID,
    appPassword: process.env.MICROSOFT_APP_PASSWORD
});

// Memory Storage
const storage = new MemoryStorage();
// ConversationState lasts for the entirety of a conversation then gets disposed of
const conversationState = new ConversationState(storage);

// UserState persists information about the user across all of the conversations you have with that user
const userState  = new UserState(storage);

// Create a place in the conversation state to store dialog state.
const dialogState = conversationState.createProperty('dialogState');

// Create a place in the user storage to store a user info.
const userInfo = userState.createProperty('userInfo');

// Create a dialog set and pass in our dialogState property.
const dialogs = new DialogSet(dialogState);

// Listen for incoming requests 
server.post('/api/messages', (req, res) => {
    adapter.processActivity(req, res, async (context) => {
        const isMessage = context.activity.type === 'message';
        const dc = dialogs.createContext(context);
 
        // Continue the current dialog if one is currently active
        await dc.continueDialog();

        // Default action
        if (!context.responded && isMessage) {

            // Getting the user info from the state
            const user = await userInfo.get(dc.context); 
            // If guest info is undefined prompt the user to check in
            if (!user.guestInfo) {
                await dc.beginDialog('checkInPrompt');
            } else {
                await dc.beginDialog('mainMenu'); 
            }           
        }
        
        // End by saving any changes to the state that have occured during this turn.
        await conversationState.saveChanges(dc.context);
        await userState.saveChanges(dc.context);
    });
});

dialogs.add(new WaterfallDialog('mainMenu', [
    async function (step) {
        const menu = ["Reserve Table", "Wake Up"];
        await step.context.sendActivity(MessageFactory.suggestedActions(menu));
        return await step.next();
    },
    async function (step) {
        // Decide which module to start
        switch (step.result) {
            case "Reserve Table":
                return await step.beginDialog('reservePrompt');
                break;
            case "Wake Up":
                return await step.beginDialog('wakeUpPrompt');
                break;
            default:
                await step.context.sendActivity("Sorry, i don't understand that command. Please choose an option from the list below.");
                return await step.next();
                break;            
        }
    },
    async function (step){
        return await step.replaceDialog('mainMenu'); // Show the menu again
    }

]));

// Importing the dialogs 
const checkIn = require("./checkIn");
dialogs.add(new checkIn.CheckIn('checkInPrompt', userState));

const reserve_table = require("./reserveTable");
dialogs.add(new reserve_table.ReserveTable('reservePrompt', userState));

const wake_up = require("./wake_up");
dialogs.add(new wake_up.WakeUp('wakeUpPrompt', userState));
```

---
<!-- TODO: These dialogs are not fully modularized, as there are cross dependencies:
    - Importantly, the dialogs need to know details about the bot's user state.
-->

Como puede ver, los diálogos modulares se agregan al diálogo principal del bot de manera parecida a como se agregan los [avisos](bot-builder-prompts.md) a un diálogo. Puede agregar tantos diálogos secundarios al diálogo principal como desee. Cada módulo permitiría agregar funcionalidades y servicios adicionales que el bot puede ofrecer a sus usuarios.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ya sabe cómo modularizar la lógica del bot mediante diálogos, echemos un vistazo al uso de Language Understanding (LUIS) para ayudar al bot a decidir cuándo empezar los diálogos.

> [!div class="nextstepaction"]
> [Uso de LUIS para el reconocimiento de lenguaje](./bot-builder-howto-v4-luis.md)
