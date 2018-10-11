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
# <a name="create-an-integrated-set-of-dialogs"></a><span data-ttu-id="93aba-104">Creación de un conjunto de diálogos integrado</span><span class="sxs-lookup"><span data-stu-id="93aba-104">Create an integrated set of dialogs</span></span>

[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]

<span data-ttu-id="93aba-105">Imagine que va a crear un bot de hotel que controla varias tareas como, por ejemplo, saludar al usuario, reservar una mesa para cenar, encargar comida, configurar una alarma, mostrar el tiempo actual, etc.</span><span class="sxs-lookup"><span data-stu-id="93aba-105">Imagine that you are creating a hotel bot that handles multiple tasks such as greeting the user, reserving a dinner table, ordering food, setting an alarm, displaying the current weather and many others.</span></span> <span data-ttu-id="93aba-106">Puede controlar cada una de estas tareas del bot con un objeto de diálogo, pero esto puede hacer que el código del diálogo sea demasiado grande y desordenado en el archivo principal del bot.</span><span class="sxs-lookup"><span data-stu-id="93aba-106">You can handle each of these tasks within your bot using one dialog object but this can make your dialog code too large and cluttered in your bot's main file.</span></span>

<span data-ttu-id="93aba-107">Puede resolver este problema mediante la modularización.</span><span class="sxs-lookup"><span data-stu-id="93aba-107">You can tackle this issue through modularization.</span></span> <span data-ttu-id="93aba-108">La modularización simplificará el código y hará que sea más fácil su depuración.</span><span class="sxs-lookup"><span data-stu-id="93aba-108">Modularization will streamline your code and make it easier to debug.</span></span> <span data-ttu-id="93aba-109">Además, puede dividirlo en secciones con lo cual puede hacer que varios equipos trabajen a la vez en el mismo bot.</span><span class="sxs-lookup"><span data-stu-id="93aba-109">Additionally, you can break it up into sections allowing multiple teams to simultaneously work on the bot.</span></span> <span data-ttu-id="93aba-110">Podemos crear un bot que administre varios flujos de conversación dividiéndolos en componentes mediante un contenedor de diálogos.</span><span class="sxs-lookup"><span data-stu-id="93aba-110">We can create a bot that manages multiple conversation flows by breaking them up into components using a dialog container.</span></span> <span data-ttu-id="93aba-111">Vamos a crear varios flujos básicos de conversación y a mostrar cómo se pueden combinar mediante un contenedor de diálogos.</span><span class="sxs-lookup"><span data-stu-id="93aba-111">We will create a few basic conversation flows and show how they can be combined together using a dialog container.</span></span>

<span data-ttu-id="93aba-112">En este ejemplo vamos a crear un bot de hotel que combina los módulos de registro, despertador y reserva de mesa.</span><span class="sxs-lookup"><span data-stu-id="93aba-112">In this example we will be creating a hotel bot that combines check-in, wake-up, and reserve-table modules.</span></span>

## <a name="managing-state"></a><span data-ttu-id="93aba-113">Administración de estados</span><span class="sxs-lookup"><span data-stu-id="93aba-113">Managing state</span></span>

<span data-ttu-id="93aba-114">Hay muchas maneras de configurar la administración de estados de un bot que usa diálogos compuestos.</span><span class="sxs-lookup"><span data-stu-id="93aba-114">There are many ways to set up state management for a bot that uses composite dialogs.</span></span> <span data-ttu-id="93aba-115">En este bot, se muestra una manera de hacerlo.</span><span class="sxs-lookup"><span data-stu-id="93aba-115">In this bot, we demonstrate one way to do so.</span></span>

<span data-ttu-id="93aba-116">Para más información acerca de la administración de estados, consulte el artículo [Guardar el estado mediante las propiedades del usuario y la conversación](bot-builder-howto-v4-state.md).</span><span class="sxs-lookup"><span data-stu-id="93aba-116">For more information about state management, see [Save state using conversation and user properties](bot-builder-howto-v4-state.md).</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="93aba-117">C#</span><span class="sxs-lookup"><span data-stu-id="93aba-117">C#</span></span>](#tab/csharp)

<span data-ttu-id="93aba-118">Cada diálogo recopilará algo de información y esta se guardará en el estado del usuario.</span><span class="sxs-lookup"><span data-stu-id="93aba-118">Each dialog will collect some information, and we will save this information to the user state.</span></span> <span data-ttu-id="93aba-119">Vamos a definir una clase para cada diálogo y a usar esas clases como propiedades en nuestra clase de estado de usuario.</span><span class="sxs-lookup"><span data-stu-id="93aba-119">We'll define a class for each dialog, and we'll use those classes as properties in our user state class.</span></span>

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

<span data-ttu-id="93aba-120">En un turno de bot, el método `CreateContext` del conjunto de diálogos establece el estado del diálogo.</span><span class="sxs-lookup"><span data-stu-id="93aba-120">Within a bot turn, the dialog set's `CreateContext` method establishes dialog state.</span></span>
<span data-ttu-id="93aba-121">El método toma el [contexto de turno](bot-builder-concept-activity-processing.md#turn-context) y un objeto de estado como parámetros.</span><span class="sxs-lookup"><span data-stu-id="93aba-121">The method takes the [turn context](bot-builder-concept-activity-processing.md#turn-context) and a state object as parameters.</span></span>

<span data-ttu-id="93aba-122">En el caso de los diálogos, este objeto de estado debe implementar la interfaz `IDictionary<string, object>`.</span><span class="sxs-lookup"><span data-stu-id="93aba-122">For dialogs, this state object must implement the `IDictionary<string, object>` interface.</span></span> <span data-ttu-id="93aba-123">Puesto que este bot solo usa el estado de conversación para alojar el estado del diálogo, nuestra clase de estado de conversación puede ser un simple diccionario.</span><span class="sxs-lookup"><span data-stu-id="93aba-123">Since this bot is only using conversation state to house the dialog state, our conversation state class can be a simple dictionary.</span></span>

```csharp
using System.Collections.Generic;

/// <summary>
/// Conversation state information.
/// We are also using this directly for dialog state, which needs an <see cref="IDictionary{string, object}"/> object.
/// </summary>
public class ConversationInfo : Dictionary<string, object> { }
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="93aba-124">JavaScript</span><span class="sxs-lookup"><span data-stu-id="93aba-124">JavaScript</span></span>](#tab/javascript)

<span data-ttu-id="93aba-125">Para realizar un seguimiento de la entrada del usuario, se va a pasar un objeto `userInfo` desde el diálogo principal como parámetro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="93aba-125">To keep track of the user's input, we will pass in from the parent dialog a `userInfo` object as a dialog parameter.</span></span> <span data-ttu-id="93aba-126">En cada clase de diálogo, se va a integrar el diálogo en el constructor, lo cual le permite guardar información en `userInfo`.</span><span class="sxs-lookup"><span data-stu-id="93aba-126">In each dialog class the dialog will be built into the constructor, which allows you to save information to `userInfo`.</span></span> <span data-ttu-id="93aba-127">A lo largo de este diálogo, podemos escribir en un objeto de estado local definido como propiedad en el objeto `step.values` a medida que el usuario introduce la información.</span><span class="sxs-lookup"><span data-stu-id="93aba-127">Throughout this dialog, we can write to a local state object defined as a property on the `step.values` object as the user inputs information.</span></span> <span data-ttu-id="93aba-128">Una vez completado el diálogo, se eliminará el objeto de estado local.</span><span class="sxs-lookup"><span data-stu-id="93aba-128">After the dialog is complete, the local state object will be disposed of.</span></span> <span data-ttu-id="93aba-129">Por lo tanto, se guardará el objeto de estado local en el `userInfo` principal, lo cual ayudará a conservar la información acerca del usuario en todas las conversaciones que tenga con este.</span><span class="sxs-lookup"><span data-stu-id="93aba-129">Therefore, we will save the local state object to the parent's `userInfo`, which will persist information about the user across all the conversations you have with the user.</span></span> 

---

## <a name="define-a-modular-check-in-dialog"></a><span data-ttu-id="93aba-130">Definición de un diálogo de registro modular</span><span class="sxs-lookup"><span data-stu-id="93aba-130">Define a modular check-in dialog</span></span>

<span data-ttu-id="93aba-131">En primer lugar, vamos a empezar con un sencillo diálogo de registro que pedirá al usuario su nombre y el número de habitación en la que se alojará.</span><span class="sxs-lookup"><span data-stu-id="93aba-131">First, we start with a simple check-in dialog that will ask the user for their name and what room they will be staying in.</span></span> <span data-ttu-id="93aba-132">Para modularizar esta tarea, vamos a crear una clase `CheckIn` que extienda `ComponentDialog`.</span><span class="sxs-lookup"><span data-stu-id="93aba-132">To modularize this task, we create a `CheckIn` class that extends `ComponentDialog`.</span></span> <span data-ttu-id="93aba-133">Esta clase tiene un constructor que define el nombre del diálogo de raíz que, a su vez, se define como una *cascada* con tres pasos.</span><span class="sxs-lookup"><span data-stu-id="93aba-133">This class has a constructor that defines the name of the root dialog, which is defined as a *waterfall* with three steps.</span></span> <span data-ttu-id="93aba-134">La firma y construcción del objeto de diálogo es exactamente la misma que la de una cascada estándar.</span><span class="sxs-lookup"><span data-stu-id="93aba-134">The signature and construction of the dialog object is exactly the same as a standard waterfall.</span></span>

<span data-ttu-id="93aba-135">**Pasos del diálogo de registro**</span><span class="sxs-lookup"><span data-stu-id="93aba-135">**Check-in dialog steps**</span></span>

1. <span data-ttu-id="93aba-136">Pida el nombre del huésped.</span><span class="sxs-lookup"><span data-stu-id="93aba-136">Ask for the guest's name.</span></span>
1. <span data-ttu-id="93aba-137">Pregunte por la habitación en la que le gustaría permanecer.</span><span class="sxs-lookup"><span data-stu-id="93aba-137">Ask for the room they'd like to stay in.</span></span>
1. <span data-ttu-id="93aba-138">Envíe un mensaje de confirmación y complete el diálogo.</span><span class="sxs-lookup"><span data-stu-id="93aba-138">Send a confirmation message and complete the dialog.</span></span>

<span data-ttu-id="93aba-139">Para más información sobre diálogos y cascadas, consulte [Uso de diálogos para administrar un flujo de conversación simple](bot-builder-dialog-manage-conversation-flow.md).</span><span class="sxs-lookup"><span data-stu-id="93aba-139">For more information about dialogs and waterfalls, see [Use dialogs to manage simple conversation flow](bot-builder-dialog-manage-conversation-flow.md).</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="93aba-140">C#</span><span class="sxs-lookup"><span data-stu-id="93aba-140">C#</span></span>](#tab/csharp)

<span data-ttu-id="93aba-141">La clase `CheckIn` tiene un constructor privado que define los pasos de nuestro diálogo de registro, crea una única instancia y la expone en una propiedad estática `Instance`.</span><span class="sxs-lookup"><span data-stu-id="93aba-141">The `CheckIn` class has a private constructor that defines the steps for our check-in dialog, creates a single instance, and exposes that in a static `Instance` property.</span></span>

<span data-ttu-id="93aba-142">A lo largo de este diálogo podemos escribir en un objeto de estado local, al que se puede acceder mediante una propiedad del contexto de paso, `step.values`.</span><span class="sxs-lookup"><span data-stu-id="93aba-142">Throughout this dialog we can write to a local state object, accessible through a property of the step context, `step.values`.</span></span> <span data-ttu-id="93aba-143">Una vez completado el diálogo, se elimina el objeto de estado local.</span><span class="sxs-lookup"><span data-stu-id="93aba-143">When the dialog completes, the local state object is be disposed of.</span></span> <span data-ttu-id="93aba-144">Por tanto, vamos a guardar el objeto de estado local en el `userState` del bot, lo cual ayudará a conservar la información acerca del usuario en todas las conversaciones que tenga con este.</span><span class="sxs-lookup"><span data-stu-id="93aba-144">Therefore, we save the local state object to the bot's `userState` that will persist information about the user across all the conversations you have with the user.</span></span>

<span data-ttu-id="93aba-145">Para más información acerca de la administración de estados, consulte el artículo [Guardar el estado mediante las propiedades del usuario y la conversación](bot-builder-howto-v4-state.md).</span><span class="sxs-lookup"><span data-stu-id="93aba-145">For more information about state management, see [Save state using conversation and user properties](bot-builder-howto-v4-state.md).</span></span> 

<span data-ttu-id="93aba-146">**CheckIn.cs**</span><span class="sxs-lookup"><span data-stu-id="93aba-146">**CheckIn.cs**</span></span>
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

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="93aba-147">JavaScript</span><span class="sxs-lookup"><span data-stu-id="93aba-147">JavaScript</span></span>](#tab/javascript)

<span data-ttu-id="93aba-148">**checkIn.js**</span><span class="sxs-lookup"><span data-stu-id="93aba-148">**checkIn.js**</span></span>
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

## <a name="define-modular-reserve-table-and-wake-up-dialogs"></a><span data-ttu-id="93aba-149">Definición de los diálogos modulares de reserva de mesa y de despertador</span><span class="sxs-lookup"><span data-stu-id="93aba-149">Define modular reserve-table and wake-up dialogs</span></span>

<span data-ttu-id="93aba-150">Una ventaja de usar un diálogo de componentes es la posibilidad de unir diálogos.</span><span class="sxs-lookup"><span data-stu-id="93aba-150">One benefit of using a component dialog s the ability to compose dialogs together.</span></span> <span data-ttu-id="93aba-151">Como cada `DialogSet` mantiene un conjunto exclusivo de `dialogs`, el uso compartido o la referencia cruzada de `dialogs` no se puede realizar fácilmente.</span><span class="sxs-lookup"><span data-stu-id="93aba-151">Since each `DialogSet` maintains an exclusive set of `dialogs`, sharing or cross referencing `dialogs` cannot be done easily.</span></span> <span data-ttu-id="93aba-152">Y es aquí donde entra en juego el diálogo de componentes.</span><span class="sxs-lookup"><span data-stu-id="93aba-152">This is where the component dialog comes in.</span></span> <span data-ttu-id="93aba-153">Puede usar diálogos de componentes para crear un diálogo compuesto que facilite la administración del flujo de conversación entre diálogos.</span><span class="sxs-lookup"><span data-stu-id="93aba-153">You can use component dialogs to create a composite dialog that makes managing the dialog flow across dialogs easier.</span></span> <span data-ttu-id="93aba-154">A modo de ejemplo, vamos a crear dos diálogos más: uno para preguntar al usuario qué mesa le gustaría reservar para cenar y otro para crear una llamada de despertador.</span><span class="sxs-lookup"><span data-stu-id="93aba-154">To illustrate that, let's create two more dialogs: one to ask the user which table they would like to reserve for dinner, and one to create a wake up call.</span></span> <span data-ttu-id="93aba-155">Una vez más, vamos a usar una clase aparte para cada diálogo y cada diálogo ampliará `ComponentDialog`.</span><span class="sxs-lookup"><span data-stu-id="93aba-155">Again, we'll use a seaparate class for each dialog, and each dialog will extend `ComponentDialog`.</span></span>

<span data-ttu-id="93aba-156">**Pasos del diálogo de reserva de mesa**</span><span class="sxs-lookup"><span data-stu-id="93aba-156">**Reserve-table dialog steps**</span></span>

1. <span data-ttu-id="93aba-157">Pregunte por la mesa que se va a reservar.</span><span class="sxs-lookup"><span data-stu-id="93aba-157">Ask for the table to reserve.</span></span>
1. <span data-ttu-id="93aba-158">Envíe un mensaje de confirmación y complete el diálogo.</span><span class="sxs-lookup"><span data-stu-id="93aba-158">Send a confirmation message and complete the dialog.</span></span>

<span data-ttu-id="93aba-159">**Pasos del diálogo de despertador**</span><span class="sxs-lookup"><span data-stu-id="93aba-159">**Wake-up dialog steps**</span></span>

1. <span data-ttu-id="93aba-160">Pregunte la hora a la que desea que se le despierte.</span><span class="sxs-lookup"><span data-stu-id="93aba-160">Ask for the wake-up time to set for them.</span></span>
1. <span data-ttu-id="93aba-161">Envíe un mensaje de confirmación y complete el diálogo.</span><span class="sxs-lookup"><span data-stu-id="93aba-161">Send a confirmation message and complete the dialog.</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="93aba-162">C#</span><span class="sxs-lookup"><span data-stu-id="93aba-162">C#</span></span>](#tab/csharp)

<span data-ttu-id="93aba-163">**ReserveTable.cs**</span><span class="sxs-lookup"><span data-stu-id="93aba-163">**ReserveTable.cs**</span></span>
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

<span data-ttu-id="93aba-164">**WakeUp.cs**</span><span class="sxs-lookup"><span data-stu-id="93aba-164">**WakeUp.cs**</span></span>
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

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="93aba-165">JavaScript</span><span class="sxs-lookup"><span data-stu-id="93aba-165">JavaScript</span></span>](#tab/javascript)

<span data-ttu-id="93aba-166">**reserveTable.js**</span><span class="sxs-lookup"><span data-stu-id="93aba-166">**reserveTable.js**</span></span>
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

<span data-ttu-id="93aba-167">**wakeUp.js**</span><span class="sxs-lookup"><span data-stu-id="93aba-167">**wakeUp.js**</span></span>
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

## <a name="add-modular-dialogs-to-a-bot"></a><span data-ttu-id="93aba-168">Incorporación de diálogos modulares a un bot</span><span class="sxs-lookup"><span data-stu-id="93aba-168">Add modular dialogs to a bot</span></span>

<span data-ttu-id="93aba-169">El archivo principal del bot une estos tres diálogos modularizados en un bot.</span><span class="sxs-lookup"><span data-stu-id="93aba-169">The main bot file ties these three modularized dialogs into one bot.</span></span>

- <span data-ttu-id="93aba-170">Los tres diálogos se agregan al conjunto de diálogos principal de nuestro bot.</span><span class="sxs-lookup"><span data-stu-id="93aba-170">All three dialogs are added to our bot's main dialog set.</span></span>
- <span data-ttu-id="93aba-171">Los diálogos de reserva de mesa y de despertador se integran en el flujo de conversación del diálogo principal.</span><span class="sxs-lookup"><span data-stu-id="93aba-171">The reserve-table and wake-up dialogs are built into the conversation flow of the main dialog.</span></span>
- <span data-ttu-id="93aba-172">Cuando se inicia una nueva conversación, no hay ningún diálogo activo y la lógica del turno de encendido del bot asume el control.</span><span class="sxs-lookup"><span data-stu-id="93aba-172">When a new conversation starts, we don't have an active dialog and the bot's on turn logic takes over.</span></span>

<span data-ttu-id="93aba-173">**Controlador del turno de encendido del bot**</span><span class="sxs-lookup"><span data-stu-id="93aba-173">**Bot's on-turn handler**</span></span>

<span data-ttu-id="93aba-174">Siempre que el turno del bot está fuera de un diálogo activo, este comprueba el estado del usuario.</span><span class="sxs-lookup"><span data-stu-id="93aba-174">Whenever the bot turn is outside an active dialog it checks user state.</span></span>
1. <span data-ttu-id="93aba-175">Si ya tiene la información del huésped, inicia su diálogo principal.</span><span class="sxs-lookup"><span data-stu-id="93aba-175">If it already has the guest's information, it starts it's main dialog.</span></span>
1. <span data-ttu-id="93aba-176">En caso contrario, iniciará el diálogo secundario del principal de registro.</span><span class="sxs-lookup"><span data-stu-id="93aba-176">Otherwise, it starts the main dialog's check-in child dialog.</span></span>

<span data-ttu-id="93aba-177">**Pasos del diálogo principal**</span><span class="sxs-lookup"><span data-stu-id="93aba-177">**Main dialog setps**</span></span>

1. <span data-ttu-id="93aba-178">Preguntar al huésped qué desea hacer: reservar una mesa o configurar una llamada de despertador.</span><span class="sxs-lookup"><span data-stu-id="93aba-178">Ask the guest what they'd like to do: reserve a table or set a wake up call.</span></span>
1. <span data-ttu-id="93aba-179">Iniciar el diálogo secundario correspondiente o enviar un mensaje de _entrada no comprendida_ y pasar al siguiente paso.</span><span class="sxs-lookup"><span data-stu-id="93aba-179">Start the corresponding child dialog, or send an _input not understood_ message and skip to the next step.</span></span>
1. <span data-ttu-id="93aba-180">Volver al principio de este diálogo.</span><span class="sxs-lookup"><span data-stu-id="93aba-180">Skip back to the beginning of this dialog.</span></span>


# <a name="ctabcsharp"></a>[<span data-ttu-id="93aba-181">C#</span><span class="sxs-lookup"><span data-stu-id="93aba-181">C#</span></span>](#tab/csharp)

<span data-ttu-id="93aba-182">El flujo de diálogo se actualiza con el método `Continue` del contexto del diálogo.</span><span class="sxs-lookup"><span data-stu-id="93aba-182">Dialog flow is updated using dialog context's `Continue` method.</span></span> <span data-ttu-id="93aba-183">Este método ejecuta el siguiente paso de la cascada en la pila de diálogo.</span><span class="sxs-lookup"><span data-stu-id="93aba-183">This method runs the next step of the waterfall on the dialog stack.</span></span>

<span data-ttu-id="93aba-184">**HotelBot.cs**</span><span class="sxs-lookup"><span data-stu-id="93aba-184">**HotelBot.cs**</span></span>
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

<span data-ttu-id="93aba-185">Por último, necesitamos actualizar el método `ConfigureServices` de la clase `StartUp` para conectar nuestro bot y agregar el middleware de estado.</span><span class="sxs-lookup"><span data-stu-id="93aba-185">Finally, we need to update the `ConfigureServices` method of the `StartUp` class to connect our bot and add state middleware.</span></span>

<span data-ttu-id="93aba-186">**Startup.cs**</span><span class="sxs-lookup"><span data-stu-id="93aba-186">**Startup.cs**</span></span>
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

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="93aba-187">JavaScript</span><span class="sxs-lookup"><span data-stu-id="93aba-187">JavaScript</span></span>](#tab/javascript)

<span data-ttu-id="93aba-188">El flujo de diálogo se actualiza con el método `continue` del contexto del diálogo.</span><span class="sxs-lookup"><span data-stu-id="93aba-188">Dialog flow is updated using dialog context's `continue` method.</span></span> <span data-ttu-id="93aba-189">Este método ejecuta el siguiente paso de la cascada en la pila de diálogo.</span><span class="sxs-lookup"><span data-stu-id="93aba-189">This method runs the next step of the waterfall on the dialog stack.</span></span>

<span data-ttu-id="93aba-190">**app.js**</span><span class="sxs-lookup"><span data-stu-id="93aba-190">**app.js**</span></span>
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

<span data-ttu-id="93aba-191">Como puede ver, los diálogos modulares se agregan al diálogo principal del bot de manera parecida a como se agregan los [avisos](bot-builder-prompts.md) a un diálogo.</span><span class="sxs-lookup"><span data-stu-id="93aba-191">As you can see, the modular dialogs are added to the bot's main dialog in a fashion similiar to how you add [prompts](bot-builder-prompts.md) to a dialog.</span></span> <span data-ttu-id="93aba-192">Puede agregar tantos diálogos secundarios al diálogo principal como desee.</span><span class="sxs-lookup"><span data-stu-id="93aba-192">You can add as many child dialogs to your main dialog as you want.</span></span> <span data-ttu-id="93aba-193">Cada módulo permitiría agregar funcionalidades y servicios adicionales que el bot puede ofrecer a sus usuarios.</span><span class="sxs-lookup"><span data-stu-id="93aba-193">Each module would add additional capabilities and services that the bot can offer to your users.</span></span>

## <a name="next-steps"></a><span data-ttu-id="93aba-194">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="93aba-194">Next steps</span></span>

<span data-ttu-id="93aba-195">Ahora que ya sabe cómo modularizar la lógica del bot mediante diálogos, echemos un vistazo al uso de Language Understanding (LUIS) para ayudar al bot a decidir cuándo empezar los diálogos.</span><span class="sxs-lookup"><span data-stu-id="93aba-195">Now that you know how to modularize your bot logic by using dialogs, let's take a look at how to use Language Understanding (LUIS) to help your bot decide when to begin the dialogs.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="93aba-196">Uso de LUIS para el reconocimiento de lenguaje</span><span class="sxs-lookup"><span data-stu-id="93aba-196">Use LUIS for Language Understanding</span></span>](./bot-builder-howto-v4-luis.md)
