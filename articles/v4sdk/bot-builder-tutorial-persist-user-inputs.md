---
title: Conservación de datos de usuario | Microsoft Docs
description: Obtenga información sobre cómo guardar los datos de estado de los usuario en el almacenamiento del Bot Builder SDK.
keywords: conversación de datos de usuario, almacenamiento, datos de conversación
author: v-ducvo
ms.author: v-ducvo
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 4/23/2018
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: 28377a1e611464012df28d3edf78d1cf01351345
ms.sourcegitcommit: 44f100a588ffda19c275b118f4f97029f12d1449
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/26/2018
ms.locfileid: "42928373"
---
# <a name="persist-user-data"></a><span data-ttu-id="fcc03-104">Conservación de datos de usuario</span><span class="sxs-lookup"><span data-stu-id="fcc03-104">Persist user data</span></span>

[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]

<span data-ttu-id="fcc03-105">Cuando el bot pide una entrada a los usuarios, lo más probable es que quiera conservar parte de la información en el almacenamiento de alguna forma.</span><span class="sxs-lookup"><span data-stu-id="fcc03-105">When the bot ask users for input, chances are that you would want to persist some of the information to storage of some form.</span></span> <span data-ttu-id="fcc03-106">El Bot Builder SDK le permite almacenar entradas de usuario mediante *almacenamiento en memoria*, *almacenamiento de archivos* o almacenamiento en bases de datos, como *CosmosDB* o *SQL*, donde los tipos de almacenamiento local se utilizan principalmente para pruebas o creación de prototipos, y los últimos tipos de almacenamiento son más adecuados para los bots de producción.</span><span class="sxs-lookup"><span data-stu-id="fcc03-106">The Bot Builder SDK allows you to store user inputs using *in-memory storage*, *file storage*, or database storage such as *CosmosDB* or *SQL*, where local storage types are mainly used for testing or prototyping and the later storage types are best for production bots.</span></span>

<span data-ttu-id="fcc03-107">En este tutorial se mostrará cómo definir el objeto de almacenamiento y guardar las entradas de usuario para el objeto de almacenamiento de modo que se pueda conservar.</span><span class="sxs-lookup"><span data-stu-id="fcc03-107">This tutorial will show you how to define your storage object and save user inputs to the storage object so that it can be persisted.</span></span> 

> [!NOTE]
> <span data-ttu-id="fcc03-108">Independientemente del tipo de almacenamiento que decida usar, el proceso de enlazar y conservar los datos es el mismo.</span><span class="sxs-lookup"><span data-stu-id="fcc03-108">Regardless of the storage type you choose to use, the process for hooking it up and persisting data is the same.</span></span> <span data-ttu-id="fcc03-109">En este tutorial se usa `FileStorage` como almacenamiento para conservar los datos.</span><span class="sxs-lookup"><span data-stu-id="fcc03-109">This tutorial uses the `FileStorage` as the storage to persist data.</span></span>
> <span data-ttu-id="fcc03-110">Para obtener más información sobre el estado y otros tipos de almacenamiento, consulte [Administración del estado de la conversación y del usuario](bot-builder-howto-v4-state.md).</span><span class="sxs-lookup"><span data-stu-id="fcc03-110">For more information about state and other storage types, see [Manage conversation and user state](bot-builder-howto-v4-state.md).</span></span>

## <a name="prequisite"></a><span data-ttu-id="fcc03-111">Requisito previo</span><span class="sxs-lookup"><span data-stu-id="fcc03-111">Prequisite</span></span> 

<span data-ttu-id="fcc03-112">Este tutorial se basa en el tutorial para [reservar una mesa](bot-builder-tutorial-waterfall.md).</span><span class="sxs-lookup"><span data-stu-id="fcc03-112">This tutorial builds on the [Reserve a table](bot-builder-tutorial-waterfall.md) tutorial.</span></span> <span data-ttu-id="fcc03-113">En este tutorial, va a crear un bot que pide al usuario tres tipos de información sobre la reserva de una mesa en su restaurante.</span><span class="sxs-lookup"><span data-stu-id="fcc03-113">In that tutorial, you build a bot that asks the user for three pieces of information about their table reservation at your restaurant.</span></span> <span data-ttu-id="fcc03-114">Sin embargo, no se conservó la entrada del usuario.</span><span class="sxs-lookup"><span data-stu-id="fcc03-114">However, the user's input wasn't persisted.</span></span> <span data-ttu-id="fcc03-115">En este tutorial se agregará la persistencia del almacenamiento de datos a ese bot.</span><span class="sxs-lookup"><span data-stu-id="fcc03-115">This tutorial will add data storage persistance to that bot.</span></span>

## <a name="add-storage-to-middleware-layer"></a><span data-ttu-id="fcc03-116">Incorporación de almacenamiento a la capa de software intermedio</span><span class="sxs-lookup"><span data-stu-id="fcc03-116">Add storage to middleware layer</span></span>

<span data-ttu-id="fcc03-117">El Bot Builder SDK V4 controla el estado y el almacenamiento a través de software intermedio de administrador de estado.</span><span class="sxs-lookup"><span data-stu-id="fcc03-117">The Bot Builder V4 SDK handles state and storage through a state manager middleware.</span></span> <span data-ttu-id="fcc03-118">El software intermedio proporciona una capa de abstracción que permite acceder a las propiedades mediante un almacén simple de valor de clave, independiente del tipo de almacenamiento subyacente.</span><span class="sxs-lookup"><span data-stu-id="fcc03-118">The middleware provides an abstraction layer that lets you access properties using a simple key-value store, independent of the type of underlying storage.</span></span> <span data-ttu-id="fcc03-119">El administrador de estado se encarga de escribir los datos en el almacenamiento y de administrar la simultaneidad, con independencia de que el tipo de almacenamiento subyacente sea en memoria, de archivos o almacenamiento de tablas de Azure.</span><span class="sxs-lookup"><span data-stu-id="fcc03-119">The state manager takes care of writing data to storage and managing concurrency, whether the underlying storage type is in-memory, file storage, or Azure table storage.</span></span> <span data-ttu-id="fcc03-120">En este tutorial, se usará `FileStorage` para conservar las entradas de usuario.</span><span class="sxs-lookup"><span data-stu-id="fcc03-120">In this tutorial, we will be using `FileStorage` to persist the user inputs.</span></span>

<span data-ttu-id="fcc03-121">El proveedor `FileStorage` viene con el paquete `bot-builder`.</span><span class="sxs-lookup"><span data-stu-id="fcc03-121">The `FileStorage` provider comes with the `bot-builder` package.</span></span> <span data-ttu-id="fcc03-122">El ejemplo con el que empezó usa el proveedor `MemoryStorage`.</span><span class="sxs-lookup"><span data-stu-id="fcc03-122">The sample you started with uses the `MemoryStorage` provider.</span></span> <span data-ttu-id="fcc03-123">Ese tipo de almacenamiento que usa almacenamiento *en memoria* volátil del bot, que se elimina cuando se reinicia el bot.</span><span class="sxs-lookup"><span data-stu-id="fcc03-123">That storage type is using the bot's volitile *in-memory* which gets disposed when the bot is restarted.</span></span> <span data-ttu-id="fcc03-124">Por otro lado, la biblioteca `FileStorage` se comporta de manera similar a una base de datos.</span><span class="sxs-lookup"><span data-stu-id="fcc03-124">The `FileStorage` library, on the other hand, behave similar to a database.</span></span> <span data-ttu-id="fcc03-125">Es decir, esta biblioteca escribe la información de almacenamiento en un archivo en el equipo local.</span><span class="sxs-lookup"><span data-stu-id="fcc03-125">That is, this library writes the storage information to a file on your local computer.</span></span> <span data-ttu-id="fcc03-126">Puede especificar dónde colocar este archivo de almacenamiento para que se puede inspeccionar más adelante.</span><span class="sxs-lookup"><span data-stu-id="fcc03-126">You can specify where to put this storage file so that you can inspect it later.</span></span>

<span data-ttu-id="fcc03-127">Para usar `FileStorage`, busque la instrucción en el bot donde se define el objeto `conversationState` y actualícelo para crear un `new botbuilder.FileStorage("c:/temp")`.</span><span class="sxs-lookup"><span data-stu-id="fcc03-127">To use the `FileStorage`, find the statement in your bot where the `conversationState` object is defined and update it to create a `new botbuilder.FileStorage("c:/temp")`.</span></span> <span data-ttu-id="fcc03-128">Además, puede definir la ubicación donde se debe escribir este archivo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="fcc03-128">Also, you can define the location where this storage file should be written out to.</span></span> <span data-ttu-id="fcc03-129">De este modo, puede encontrarlo fácilmente para inspeccionar el contenido de lo que se conservó.</span><span class="sxs-lookup"><span data-stu-id="fcc03-129">That way, you can easily find it to inspect the content of what got persisted.</span></span>

# <a name="ctabcstab"></a>[<span data-ttu-id="fcc03-130">C#</span><span class="sxs-lookup"><span data-stu-id="fcc03-130">C#</span></span>](#tab/cstab)
```cs
var storage = new FileStorage("c:/temp");

// These two classes are simply Dictionaries to store state
options.Middleware.Add(new ConversationState<MyBot.convoState>(storage));
options.Middleware.Add(new UserState<MyBot.userState>(storage));
```

<span data-ttu-id="fcc03-131">El Bot Builder SDK proporciona tres objetos de estado con ámbitos diferentes entre los que puede elegir.</span><span class="sxs-lookup"><span data-stu-id="fcc03-131">The Bot Builder SDK provide three state objects with different scopes that you can choose from.</span></span>

| <span data-ttu-id="fcc03-132">Estado</span><span class="sxs-lookup"><span data-stu-id="fcc03-132">State</span></span> | <span data-ttu-id="fcc03-133">Ámbito</span><span class="sxs-lookup"><span data-stu-id="fcc03-133">Scope</span></span> | <span data-ttu-id="fcc03-134">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="fcc03-134">Description</span></span> |
| ---- | ---- | ---- |
| `dc.ActiveDialog.State` | <span data-ttu-id="fcc03-135">cuadro de diálogo</span><span class="sxs-lookup"><span data-stu-id="fcc03-135">dialog</span></span> | <span data-ttu-id="fcc03-136">Estado disponible para los pasos del cuadro de diálogo de cascada.</span><span class="sxs-lookup"><span data-stu-id="fcc03-136">State available to the steps of the waterfall dialog.</span></span> |
| `ConversationState` | <span data-ttu-id="fcc03-137">conversación</span><span class="sxs-lookup"><span data-stu-id="fcc03-137">conversation</span></span> | <span data-ttu-id="fcc03-138">Estado disponible para la conversación actual.</span><span class="sxs-lookup"><span data-stu-id="fcc03-138">State available to current conversation.</span></span> |
| `UserState` | <span data-ttu-id="fcc03-139">user</span><span class="sxs-lookup"><span data-stu-id="fcc03-139">user</span></span> | <span data-ttu-id="fcc03-140">Estado disponible para varias conversaciones.</span><span class="sxs-lookup"><span data-stu-id="fcc03-140">State available accross multiple conversations.</span></span> |

# <a name="javascripttabjstab"></a>[<span data-ttu-id="fcc03-141">JavaScript</span><span class="sxs-lookup"><span data-stu-id="fcc03-141">JavaScript</span></span>](#tab/jstab)

<span data-ttu-id="fcc03-142">**app.js**</span><span class="sxs-lookup"><span data-stu-id="fcc03-142">**app.js**</span></span>
```javascript
// Storage
const storage = new FileStorage("c:/temp");
const convoState = new ConversationState(storage);
const userState  = new UserState(storage);
adapter.use(new BotStateSet(convoState, userState));
```

<span data-ttu-id="fcc03-143">El `BotStateSet` pueden administrar tanto `ConversationState` como `UserState` al mismo tiempo.</span><span class="sxs-lookup"><span data-stu-id="fcc03-143">The `BotStateSet` can manage both the `ConversationState` and `UserState` at the same time.</span></span> <span data-ttu-id="fcc03-144">Cuando llega el momento de guardar los datos de usuario, puede elegir.</span><span class="sxs-lookup"><span data-stu-id="fcc03-144">When it comes time to save user data, you can choose.</span></span> <span data-ttu-id="fcc03-145">El Bot Builder SDK proporciona tres objetos de estado con ámbitos diferentes entre los que puede elegir.</span><span class="sxs-lookup"><span data-stu-id="fcc03-145">The Bot Builder SDK provide three state objects with different scopes that you can choose from.</span></span>

| <span data-ttu-id="fcc03-146">Estado</span><span class="sxs-lookup"><span data-stu-id="fcc03-146">State</span></span> | <span data-ttu-id="fcc03-147">Ámbito</span><span class="sxs-lookup"><span data-stu-id="fcc03-147">Scope</span></span> | <span data-ttu-id="fcc03-148">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="fcc03-148">Description</span></span> |
| ---- | ---- | ---- |
| `dc.activeDialog.state` | <span data-ttu-id="fcc03-149">cuadro de diálogo</span><span class="sxs-lookup"><span data-stu-id="fcc03-149">dialog</span></span> | <span data-ttu-id="fcc03-150">Estado disponible para los pasos del cuadro de diálogo de cascada.</span><span class="sxs-lookup"><span data-stu-id="fcc03-150">State available to the steps of the waterfall dialog.</span></span> |
| `ConversationState` | <span data-ttu-id="fcc03-151">conversación</span><span class="sxs-lookup"><span data-stu-id="fcc03-151">conversation</span></span> | <span data-ttu-id="fcc03-152">Estado disponible para la conversación actual.</span><span class="sxs-lookup"><span data-stu-id="fcc03-152">State available to current conversation.</span></span> |
| `UserState` | <span data-ttu-id="fcc03-153">user</span><span class="sxs-lookup"><span data-stu-id="fcc03-153">user</span></span> | <span data-ttu-id="fcc03-154">Estado disponible para varias conversaciones.</span><span class="sxs-lookup"><span data-stu-id="fcc03-154">State available accross multiple conversations.</span></span> |

---
 

## <a name="persist-state"></a><span data-ttu-id="fcc03-155">Conservación del estado</span><span class="sxs-lookup"><span data-stu-id="fcc03-155">Persist state</span></span>

<span data-ttu-id="fcc03-156">Para el bot, puede escribir en cualquiera de estas tres ubicaciones de estado, según lo que guarde y cuánto tiempo debe conservarse.</span><span class="sxs-lookup"><span data-stu-id="fcc03-156">For your bot you can write to any of these three state locations, depending on what you're saving and how long it needs to persist.</span></span>  

# <a name="ctabcstab"></a>[<span data-ttu-id="fcc03-157">C#</span><span class="sxs-lookup"><span data-stu-id="fcc03-157">C#</span></span>](#tab/cstab)

<span data-ttu-id="fcc03-158">El *software intermedio del administrador de estado* controla el estado de escritura automáticamente al final de cada turno.</span><span class="sxs-lookup"><span data-stu-id="fcc03-158">The *state manager middleware* handles writing state for you at the end of each turn.</span></span> <span data-ttu-id="fcc03-159">Por lo tanto, todo lo que debe hacer en el bot es asignar los datos al objeto de estado de su elección.</span><span class="sxs-lookup"><span data-stu-id="fcc03-159">So, all you need to do in your bot is assign the data to the state object of your choice.</span></span> <span data-ttu-id="fcc03-160">En este ejemplo, se usa `dc.ActiveDialog.State` para realizar el seguimiento de la entrada del usuario para la reserva.</span><span class="sxs-lookup"><span data-stu-id="fcc03-160">In this example, we use the `dc.ActiveDialog.State` to track the user input for our reservation.</span></span> <span data-ttu-id="fcc03-161">De este modo, en lugar de guardar la entrada del usuario en una variable global, se puede almacenar en un ámbito de objeto de estado temporal en el cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="fcc03-161">This way, instead of saving the user input in a global variable, you can store it in a temporary state object scope to the dialog.</span></span> <span data-ttu-id="fcc03-162">Este objeto solo existe en la medida en que el cuadro de diálogo esté activo; si desea conservarlo más tiempo, debe transferirlo a uno de los otros objetos de estado.</span><span class="sxs-lookup"><span data-stu-id="fcc03-162">This object only exists as long as the dialog is active; if you want to persist it longer, you must transfer it to one of the other state objects.</span></span> <span data-ttu-id="fcc03-163">En este caso, asignamos la reserva `msg` al estado de la conversación en el último paso de la cascada.</span><span class="sxs-lookup"><span data-stu-id="fcc03-163">In this case, we assign the reservation `msg` to the conversation state in the last step of the waterfall.</span></span>

```cs
dialogs.Add("reserveTable", new WaterfallStep[]
{
    async (dc, args, next) =>
    {
        // Prompt for the guest's name.
        await dc.Context.SendActivity("Welcome to the reservation service.");

        dc.ActiveDialog.State = new Dictionary<string, object>();

        await dc.Prompt("dateTimePrompt", "Please provide a reservation date and time.");
    },
    async(dc, args, next) =>
    {
        var dateTimeResult = ((DateTimeResult)args).Resolution.First();

        dc.ActiveDialog.State["date"] = Convert.ToDateTime(dateTimeResult.Value);
        
        // Ask for next info
        await dc.Prompt("partySizePrompt", "How many people are in your party?");

    },
    async(dc, args, next) =>
    {
        dc.ActiveDialog.State["partySize"] = (int)args["Value"];

        // Ask for next info
        await dc.Prompt("textPrompt", "Who's name will this be under?");
    },
    async(dc, args, next) =>
    {
        dc.ActiveDialog.State["name"] = args["Text"];
        string msg = "Reservation confirmed. Reservation details - " +
        $"\nDate/Time: {dc.ActiveDialog.State["date"].ToString()} " +
        $"\nParty size: {dc.ActiveDialog.State["partySize"].ToString()} " +
        $"\nReservation name: {dc.ActiveDialog.State["name"]}";

        var convo = ConversationState<convoState>.Get(dc.Context);

        // In production, you may want to store something more helpful
        convo[$"{dc.ActiveDialog.State["name"]} reservation"] = msg;

        await dc.Context.SendActivity(msg);
        await dc.End();
    }
});
```

# <a name="javascripttabjstab"></a>[<span data-ttu-id="fcc03-164">JavaScript</span><span class="sxs-lookup"><span data-stu-id="fcc03-164">JavaScript</span></span>](#tab/jstab)

<span data-ttu-id="fcc03-165">El *software intermedio del administrador de estado* controla el estado de escritura en el archivo automáticamente al final de cada turno.</span><span class="sxs-lookup"><span data-stu-id="fcc03-165">The *state manager middleware* handles writing state to file for you at the end of each turn.</span></span> <span data-ttu-id="fcc03-166">Por lo tanto, todo lo que debe hacer en el bot es asignar los datos al objeto de estado de su elección.</span><span class="sxs-lookup"><span data-stu-id="fcc03-166">So, all you need to do in your bot is assign the data to the state object of your choice.</span></span> <span data-ttu-id="fcc03-167">En este ejemplo, se usa `dc.activeDialog.state` para realizar el seguimiento de la entrada del usuario en un objeto `reservervationInfo`.</span><span class="sxs-lookup"><span data-stu-id="fcc03-167">In this example, we use the `dc.activeDialog.state` to track the user input in a `reservervationInfo` object.</span></span> <span data-ttu-id="fcc03-168">De este modo, en lugar de guardar la entrada del usuario en una variable global, se puede almacenar en un ámbito de objeto de estado temporal en el cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="fcc03-168">This way, instead of saving the user input in a global variable, you can store it in a temporary state object scope to the dialog.</span></span> <span data-ttu-id="fcc03-169">Dado que este objeto solo existe en la medida en que el cuadro de diálogo esté activo; si desea conservarlo, debe transferirlo a uno de los otros objetos de estado.</span><span class="sxs-lookup"><span data-stu-id="fcc03-169">Because this object only exists as long as the dialog is active, if you want to persist it, you must transfer it to one of the other state objects.</span></span> <span data-ttu-id="fcc03-170">En este caso, se asigna `reservationInfo` al estado `convo` en el último paso de la cascada.</span><span class="sxs-lookup"><span data-stu-id="fcc03-170">In this case, we assign the `reservationInfo` to the `convo` state in the last step of the waterfall.</span></span>

```javascript
// Reserve a table:
// Help the user to reserve a table

dialogs.add('reserveTable', [
    async function(dc, args, next){
        await dc.context.sendActivity("Welcome to the reservation service.");

        dc.activeDialog.state.reservationInfo = {}; // Clears any previous data
        await dc.prompt('dateTimePrompt', "Please provide a reservation date and time.");
    },
    async function(dc, result){
        dc.activeDialog.state.reservationInfo.dateTime = result[0].value;

        // Ask for next info
        await dc.prompt('partySizePrompt', "How many people are in your party?");
    },
    async function(dc, result){
        dc.activeDialog.state.reservationInfo.partySize = result;

        // Ask for next info
        await dc.prompt('textPrompt', "Who's name will this be under?");
    },
    async function(dc, result){
        dc.activeDialog.state.reservationInfo.reserveName = result;
        
        // Persist data
        var convo = conversationState.get(dc.context);; // conversationState.get(dc.context);
        convo.reservationInfo = dc.activeDialog.state.reservationInfo;

        // Confirm reservation
        var msg = `Reservation confirmed. Reservation details: 
            <br/>Date/Time: ${dc.activeDialog.state.reservationInfo.dateTime} 
            <br/>Party size: ${dc.activeDialog.state.reservationInfo.partySize} 
            <br/>Reservation name: ${dc.activeDialog.state.reservationInfo.reserveName}`;
            
        await dc.context.sendActivity(msg);
        await dc.end();
    }
]);
```

---

<span data-ttu-id="fcc03-171">Ahora, está listo para enlazarlo a la lógica del bot.</span><span class="sxs-lookup"><span data-stu-id="fcc03-171">Now, you are ready to hook this into the bot logic.</span></span>

## <a name="start-the-dialog"></a><span data-ttu-id="fcc03-172">Inicio del cuadro de diálogo</span><span class="sxs-lookup"><span data-stu-id="fcc03-172">Start the dialog</span></span>

<span data-ttu-id="fcc03-173">No hay ningún código que deba cambiar aquí.</span><span class="sxs-lookup"><span data-stu-id="fcc03-173">There is no code changes you need to make here.</span></span> <span data-ttu-id="fcc03-174">Simplemente ejecute el bot y envíe el mensaje para iniciar la conversación `reserveTable`.</span><span class="sxs-lookup"><span data-stu-id="fcc03-174">Simply run the bot and send the message to start the `reserveTable` conversation.</span></span>

## <a name="check-file-storage-content"></a><span data-ttu-id="fcc03-175">Comprobación del contenido del almacenamiento de archivo</span><span class="sxs-lookup"><span data-stu-id="fcc03-175">Check file storage content</span></span>

<span data-ttu-id="fcc03-176">Después de ejecutar el bot y haber pasado por la conversación `reserveTable`, busque la información guardada en un archivo en la ubicación especificada (p. ej.: "C:/temp").</span><span class="sxs-lookup"><span data-stu-id="fcc03-176">After you run the bot and have gone through the `reserveTable` conversation, find the information saved to a file in the location you specified (e.g.: "C:/temp").</span></span> <span data-ttu-id="fcc03-177">Al nombre de archivo se antepone "conversation!"</span><span class="sxs-lookup"><span data-stu-id="fcc03-177">The file name is prepended with either "conversation!"</span></span> <span data-ttu-id="fcc03-178">o "user!".</span><span class="sxs-lookup"><span data-stu-id="fcc03-178">or "user!".</span></span> <span data-ttu-id="fcc03-179">Puede ser útil para ordenar los archivos por fecha, para poder encontrar el último de manera más fácil.</span><span class="sxs-lookup"><span data-stu-id="fcc03-179">It may help to sort the files by date so you can find the lastest one easier.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fcc03-180">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="fcc03-180">Next steps</span></span>

<span data-ttu-id="fcc03-181">Ahora que sabe cómo guardar entradas de usuario, eche un vistazo a qué tipo de entrada puede pedir al usuario mediante la biblioteca de avisos.</span><span class="sxs-lookup"><span data-stu-id="fcc03-181">Now that you know how to save user inputs, lets take a look at what type of input you can ask from the user using the prompts library.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="fcc03-182">Solicitud de entradas a los usuarios</span><span class="sxs-lookup"><span data-stu-id="fcc03-182">Prompt user for input</span></span>](~/v4sdk/bot-builder-prompts.md)
