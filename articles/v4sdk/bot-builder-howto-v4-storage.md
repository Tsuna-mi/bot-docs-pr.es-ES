---
title: Almacenamiento de datos | Microsoft Docs
description: Aprenda a escribir directamente en almacenamiento con la versión 4 de Bot Builder SDK para .NET.
keywords: almacenamiento, lectura y escritura, almacenamiento en memoria, eTag
author: DeniseMak
ms.author: v-demak
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 05/2/18
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: 653ec6a1983dd59c485a91b2c08ea07d9f2a34c8
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305576"
---
# <a name="save-data-directly-to-storage"></a><span data-ttu-id="a5096-104">Guardado de datos directamente en almacenamiento</span><span class="sxs-lookup"><span data-stu-id="a5096-104">Save data directly to storage</span></span>

<!--
 Note for V4: You can write directly to storage without using the state manager. Therefore, this topic isn't called "managing state". State is in a separate topic.
 ## Manage state data by writing directly to storage
-->
[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]

<span data-ttu-id="a5096-105">Puede escribir directamente en el almacenamiento sin usar el objeto de contexto o el middleware, leyendo y escribiendo directamente en el objeto de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="a5096-105">You can write directly to storage without using the context object or middleware, by reading and writing directly to your storage object.</span></span>

<span data-ttu-id="a5096-106">Este ejemplo de código muestra cómo leer y escribir datos en el almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="a5096-106">This code example shows you how to read and write data to storage.</span></span> <span data-ttu-id="a5096-107">En este ejemplo, el almacenamiento es un archivo, pero puede cambiar fácilmente el código para inicializar el objeto de almacenamiento utilizando almacenamiento en memoria de Azure Table Storage en su lugar.</span><span class="sxs-lookup"><span data-stu-id="a5096-107">In this example the storage is a file, but you can easily change the code to initialize the storage object to use memory storage of Azure Table Storage instead.</span></span> 

# <a name="ctabcsharpechorproperty"></a>[<span data-ttu-id="a5096-108">C#</span><span class="sxs-lookup"><span data-stu-id="a5096-108">C#</span></span>](#tab/csharpechorproperty)
<span data-ttu-id="a5096-109">Se deberá definir un objeto y usar `IStorage.Write` y `IStorage.Read` para guardar y recuperar el estado.</span><span class="sxs-lookup"><span data-stu-id="a5096-109">We'll define an object and use `IStorage.Write` and `IStorage.Read` to save and retrieve state.</span></span> 

<span data-ttu-id="a5096-110">El ejemplo siguiente agrega todos los mensajes del usuario a una lista.</span><span class="sxs-lookup"><span data-stu-id="a5096-110">The following sample adds every message from the the user to a list.</span></span> <span data-ttu-id="a5096-111">La estructura de datos que contiene la lista se guarda en un archivo dentro del directorio que proporcione al constructor de `FileStorage`.</span><span class="sxs-lookup"><span data-stu-id="a5096-111">The data structure containing the list is saved to a file within the directory you provide to the `FileStorage` constructor.</span></span>

<span data-ttu-id="a5096-112">Comience con la plantilla EchoBot de Visual Studio en Bot Builder SDK V4.</span><span class="sxs-lookup"><span data-stu-id="a5096-112">Start with the EchoBot Visual Studio template in the BotBuilder V4 SDK.</span></span>
<span data-ttu-id="a5096-113">Edite el archivo `EchoBot.cs` .</span><span class="sxs-lookup"><span data-stu-id="a5096-113">Edit the `EchoBot.cs` file.</span></span> <span data-ttu-id="a5096-114">Agregue las siguientes instrucciones using y reemplace la definición de clase `EchoBot`.</span><span class="sxs-lookup"><span data-stu-id="a5096-114">Add the following using statements, and replace the `EchoBot` class definition.</span></span>

```csharp
using System.Collections.Generic;
using System.Linq;
```

```csharp
// In the constructor initialize file storage
public class EchoBot : IBot
{
    private readonly FileStorage _myStorage;

    public EchoBot()
    {
        _myStorage = new FileStorage(System.IO.Path.GetTempPath());
    }

    // Add a class for storing a log of utterances (text of messages) as a list
    public class UtteranceLog : IStoreItem
    {
        // A list of things that users have said to the bot
        public List<string> UtteranceList { get; private set; } = new List<string>();

        // The number of conversational turns that have occurred        
        public int TurnNumber { get; set; } = 0;

        public string eTag { get; set; } = "*";
    }

    // Replace the OnTurn in EchoBot.cs with the following:
    public async Task OnTurn(ITurnContext context)
    {
        var activityType = context.Activity.Type;

        await context.SendActivity($"Activity type: {context.Activity.Type}.");

        if (activityType == ActivityTypes.Message)
        {
            // *********** begin (create or add to log of messages)
            var utterance = context.Activity.Text;
            bool restartList = false;

            if (utterance.Equals("restart"))
            {
                restartList = true;
            }

            // Attempt to read the existing property bag
            UtteranceLog logItems = null;
            try
            {
                logItems = _myStorage.Read<UtteranceLog>("UtteranceLog").Result?.FirstOrDefault().Value;
            }
            catch (System.Exception ex)
            {
                await context.SendActivity(ex.Message);
            }

            // If the property bag wasn't found, create a new one
            if (logItems is null)
            {
                try
                {
                    // add the current utterance to a new object.
                    logItems = new UtteranceLog();
                    logItems.UtteranceList.Add(utterance);

                    await context.SendActivity($"The list is now: {string.Join(", ",logItems.UtteranceList)}");

                    var changes = new KeyValuePair<string, object>[]
                    {
                        new KeyValuePair<string, object>("UtteranceLog", logItems)
                    };
                    await _myStorage.Write(changes);
                }
                catch (System.Exception ex)
                {
                    await context.SendActivity(ex.Message);
                }
            }
            // logItems.ContainsKey("UtteranceLog") == true, we were able to read a log from storage
            else
            {
                // Modify its property
                if (restartList)
                {
                    logItems.UtteranceList.Clear();
                }
                else
                {
                    logItems.UtteranceList.Add(utterance);
                    logItems.TurnNumber++;
                }

                await context.SendActivity($"The list is now: {string.Join(", ", logItems.UtteranceList)}");

                var changes = new KeyValuePair<string, object>[]
                {
                        new KeyValuePair<string, object>("UtteranceLog", logItems)
                };
                await _myStorage.Write(changes);
            }
        }

        return;
    }
}
```
# <a name="javascripttabjsechoproperty"></a>[<span data-ttu-id="a5096-115">JavaScript</span><span class="sxs-lookup"><span data-stu-id="a5096-115">JavaScript</span></span>](#tab/jsechoproperty)

<span data-ttu-id="a5096-116">El ejemplo siguiente agrega todos los mensajes del usuario a una lista.</span><span class="sxs-lookup"><span data-stu-id="a5096-116">The following sample adds every message from the the user to a list.</span></span> <span data-ttu-id="a5096-117">La estructura de datos que contiene la lista se guarda en un archivo dentro del directorio que proporcione al constructor de `FileStorage`.</span><span class="sxs-lookup"><span data-stu-id="a5096-117">The data structure containing the list is saved to a file within the directory you provide to the `FileStorage` constructor.</span></span>

<span data-ttu-id="a5096-118">Pegue lo siguiente código en `app.js`.</span><span class="sxs-lookup"><span data-stu-id="a5096-118">Paste the following into `app.js`.</span></span>
``` javascript
const { BotFrameworkAdapter, FileStorage, MemoryStorage, ConversationState, BotStateSet } = require('botbuilder');
const restify = require('restify');

// Create server.
let server = restify.createServer();
server.listen(process.env.port || process.env.PORT || 3978, function () {
    console.log(`${server.name} listening to ${server.url}`);
});

// Create adapter.
const adapter = new BotFrameworkAdapter({
    appId: process.env.MICROSOFT_APP_ID,
    appPassword: process.env.MICROSOFT_APP_PASSWORD
});

// Add storage.
var storage = new FileStorage("C:/temp");
const conversationState = new ConversationState(storage);
adapter.use(new BotStateSet(conversationState));

// Listen for incoming activities.
server.post('/api/messages', (req, res) => {
    // Route received activity to adapter for processing.
    adapter.processActivity(req, res, async (context) => {
        if (context.activity.type === 'message') {
            const state = conversationState.get(context);
            const count = state.count === undefined ? state.count = 0 : ++state.count;

            let utterance = context.activity.text;
            let storeItems = await storage.read(["UtteranceLog"])
            try {
                // check result
                var utteranceLog = storeItems["UtteranceLog"];
                
                if (typeof (utteranceLog) != 'undefined') {
                    // log exists so we can write to it
                    storeItems["UtteranceLog"].UtteranceList.push(utterance);
                    
                    await storage.write(storeItems)
                    try {
                        context.sendActivity('Successful write.');
                    } catch (err) {
                        context.sendActivity(`Srite failed of UtteranceLog: ${err}`);
                    }

                } else {
                    context.sendActivity(`need to create new utterance log`);
                    storeItems["UtteranceLog"] = { UtteranceList: [`${utterance}`], "eTag": "*" }
                    await storage.write(storeItems)
                    try {
                        context.sendActivity('Successful write.');
                    } catch (err) {
                        context.sendActivity(`Write failed: ${err}`);
                    }
                }
            } catch (err) {
                context.sendActivity(`Read rejected. ${err}`);
            };

            await context.sendActivity(`${count}: You said "${context.activity.text}"`);
        } else {
            await context.sendActivity(`[${context.activity.type} event detected]`);
        }
    });
});
```
---

## <a name="manage-concurrency-using-etags"></a><span data-ttu-id="a5096-119">Administración de la simultaneidad mediante eTags</span><span class="sxs-lookup"><span data-stu-id="a5096-119">Manage concurrency using eTags</span></span>

<span data-ttu-id="a5096-120">En el ejemplo anterior, ha establecido la propiedad `eTag` en `*`.</span><span class="sxs-lookup"><span data-stu-id="a5096-120">In the previous example, you set the `eTag` property to `*`.</span></span> <span data-ttu-id="a5096-121">El miembro `eTag` (etiqueta de entidad) de su objeto de almacén sirve para administrar la simultaneidad.</span><span class="sxs-lookup"><span data-stu-id="a5096-121">The `eTag` (entity tag) member of your store object is for managing concurrency.</span></span> <span data-ttu-id="a5096-122">La etiqueta `eTag` indica qué hacer si otra instancia del bot ha cambiado el objeto en almacenamiento en el que está escribiendo su bot.</span><span class="sxs-lookup"><span data-stu-id="a5096-122">The `eTag` indicates what to do if another instance of the bot has changed the object in storage that your bot is writing to.</span></span> 

<!-- define optimistic concurrency -->

### <a name="last-write-wins---allow-overwrites"></a><span data-ttu-id="a5096-123">La última escritura tiene prioridad: permitir la sobrescritura</span><span class="sxs-lookup"><span data-stu-id="a5096-123">Last write wins - allow overwrites</span></span>

<span data-ttu-id="a5096-124">Establezca el valor de eTag en `*` para permitir que otras instancias del bot sobrescriban los datos escritos previamente.</span><span class="sxs-lookup"><span data-stu-id="a5096-124">Set the eTag to `*` to allow other instances of the bot to overwrite previously written data.</span></span> <span data-ttu-id="a5096-125">Un valor de propiedad `eTag` de asterisco (`*`) indica que la última escritura tiene prioridad.</span><span class="sxs-lookup"><span data-stu-id="a5096-125">An `eTag` property value of asterisk (`*`) indicates that the last writer wins.</span></span> <span data-ttu-id="a5096-126">Al crear un nuevo almacén de datos, puede establecer la etiqueta `eTag` de una propiedad en `*` para indicar que no ha guardado previamente los datos que está escribiendo, o que desea que el último escritor sobrescriba cualquier propiedad guardada previamente.</span><span class="sxs-lookup"><span data-stu-id="a5096-126">When creating a new data store, you can set `eTag` of a property to `*` to indicate that you have not previously saved the data that you are writing, or that you want the last writer to overwrite any previously saved property.</span></span> <span data-ttu-id="a5096-127">Si la simultaneidad no es un problema para el bot, establecer la propiedad `eTag` en `*` para los datos que se están escribiendo permitirá la sobrescritura.</span><span class="sxs-lookup"><span data-stu-id="a5096-127">If concurrency is not an issue for your bot, setting the `eTag` property to `*` for any data that you are writing will allow overwrites.</span></span>

<span data-ttu-id="a5096-128">Esto se muestra en el ejemplo de código siguiente.</span><span class="sxs-lookup"><span data-stu-id="a5096-128">This is shown in the following code example.</span></span>

# <a name="ctabcsetagoverwrite"></a>[<span data-ttu-id="a5096-129">C#</span><span class="sxs-lookup"><span data-stu-id="a5096-129">C#</span></span>](#tab/csetagoverwrite)
<span data-ttu-id="a5096-130">Utilice la clase `UtteranceLog` que hemos definido anteriormente.</span><span class="sxs-lookup"><span data-stu-id="a5096-130">Use the `UtteranceLog` class that we defined earlier.</span></span>
```csharp
// Add the current utterance to a new object and save it to storage.
logItems = new UtteranceLog();
logItems.UtteranceList.Add(utterance);
logItems.eTag = "*";

var changes = new KeyValuePair<string, object>[]
{
    new KeyValuePair<string, object>("UtteranceLog", logItems)
};
await _myStorage.Write(changes);

```
# <a name="javascripttabjstagoverwrite"></a>[<span data-ttu-id="a5096-131">JavaScript</span><span class="sxs-lookup"><span data-stu-id="a5096-131">JavaScript</span></span>](#tab/jstagoverwrite)
<span data-ttu-id="a5096-132">Incorporación de una expresión nueva al registro y autorización de la sobrescritura.</span><span class="sxs-lookup"><span data-stu-id="a5096-132">Add a new utterance to the log and allow overwrite.</span></span>

```javascript
storeItems["UtteranceLog"] = { UtteranceList: [`${utterance}`], "eTag": "*" }
await storage.write(storeItems)
```
---

### <a name="maintain-concurrency-and-prevent-overwrites"></a><span data-ttu-id="a5096-133">Mantenimiento de la simultaneidad y evitación de la sobrescritura</span><span class="sxs-lookup"><span data-stu-id="a5096-133">Maintain concurrency and prevent overwrites</span></span>
<span data-ttu-id="a5096-134">Use un valor distinto de `*` para la `eTag` si desea evitar el acceso simultáneo a una propiedad, para evitar los cambios por sobrescritura de otra instancia del bot.</span><span class="sxs-lookup"><span data-stu-id="a5096-134">Use a value other than `*` for the `eTag` if you want to prevent concurrent access to a property, to avoid overwriting changes from another instance of the bot.</span></span> <span data-ttu-id="a5096-135">El bot recibe una respuesta de error con el mensaje `etag conflict key=` cuando intenta guardar los datos de estado y la etiqueta `eTag` no es igual que en el almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="a5096-135">The bot receives an error response with the message `etag conflict key=` when it attempts to save state data and the `eTag` is not the same as what is in storage.</span></span> <!-- To control concurrency of data that is stored using `IStorage`, the BotBuilder SDK checks the entity tag (ETag) for `Storage.Write()` requests. -->

<span data-ttu-id="a5096-136">De forma predeterminada, el almacén comprueba la igualdad de la propiedad `eTag` de un objeto de almacenamiento cada vez que un bot escribe en ese elemento y, a continuación, lo actualiza a un nuevo valor único después de cada escritura.</span><span class="sxs-lookup"><span data-stu-id="a5096-136">By default, the store checks the `eTag` property of a storage object for equality every time a bot writes to that item, and then updates it to a new unique value after each write.</span></span> <span data-ttu-id="a5096-137">Si la propiedad `eTag` en la escritura no coincide con la `eTag` en el almacenamiento, significa que otro subproceso o bot ha cambiado los datos.</span><span class="sxs-lookup"><span data-stu-id="a5096-137">If the `eTag` property on write doesn't match the `eTag` in storage, it means another bot or thread changed the data.</span></span> 

<span data-ttu-id="a5096-138">Por ejemplo, supongamos que desea que su bot edite una nota guardada, pero no quiere que sobrescriba los cambios que haya realizado otra instancia del bot.</span><span class="sxs-lookup"><span data-stu-id="a5096-138">For example, let's say you want your bot to edit a saved note, but you don't want your bot to overwrite changes that another instance of the bot has done.</span></span> <span data-ttu-id="a5096-139">Si otra instancia del bot ha realizado modificaciones, lo que quiere es que el usuario edite la versión con las actualizaciones más recientes.</span><span class="sxs-lookup"><span data-stu-id="a5096-139">If another instance of the bot has made edits, you want the user to edit the version with the latest updates.</span></span>

# <a name="ctabcsetag"></a>[<span data-ttu-id="a5096-140">C#</span><span class="sxs-lookup"><span data-stu-id="a5096-140">C#</span></span>](#tab/csetag)
<span data-ttu-id="a5096-141">Primero, cree una clase que implemente `IStoreItem`.</span><span class="sxs-lookup"><span data-stu-id="a5096-141">First, create a class that implements `IStoreItem`.</span></span>
```csharp
public class Note : IStoreItem
{
    public string Name { get; set; }
    public string Contents { get; set; }
    public string eTag { get; set; }
}
```
<span data-ttu-id="a5096-142">A continuación, cree una nota inicial mediante la creación de un objeto de almacenamiento y agregue el objeto a su almacén.</span><span class="sxs-lookup"><span data-stu-id="a5096-142">Next, create an initial note by creating a storage object, and add the object to your store.</span></span>
```csharp
// create a note for the first time, with a non-null, non-* eTag.
var note = new Note { Name = "Shopping List", Contents = "eggs", eTag = "x" };

var changes = new KeyValuePair<string, object>[]
{
    new KeyValuePair<string, object>("Note", note)
};
await NoteStore.Write(changes);
```
<span data-ttu-id="a5096-143">A continuación, acceda y actualice la nota más adelante, manteniendo su `eTag` que usted lee desde el almacén.</span><span class="sxs-lookup"><span data-stu-id="a5096-143">Then, access and update the note later, keeping its `eTag` that you read from the store.</span></span>
```csharp
var note = NoteStore.Read<Note>("Note").Result.FirstOrDefault().Value;

if (note != null)
{
    note.Contents += ", bread";
    var changes = new KeyValuePair<string, object>[]
    {
        new KeyValuePair<string, object>("Note1", note)
    };
    await NoteStore.Write(changes);
}
```
<span data-ttu-id="a5096-144">Si se ha actualizado la nota en el almacén antes de que haya escrito los cambios, la llamada a `Write` iniciará una excepción.</span><span class="sxs-lookup"><span data-stu-id="a5096-144">If the note was updated in the store before you write your changes, the call to `Write` will throw an exception.</span></span>

# <a name="javascripttabjsetag"></a>[<span data-ttu-id="a5096-145">JavaScript</span><span class="sxs-lookup"><span data-stu-id="a5096-145">JavaScript</span></span>](#tab/jsetag)

<span data-ttu-id="a5096-146">Primero, cree un objeto`myNote`.</span><span class="sxs-lookup"><span data-stu-id="a5096-146">First, create `myNote` object.</span></span>
```javascript
var myNote = {
    name: "Shopping List",
    contents: "eggs",
    eTag: "*"
}
```
<span data-ttu-id="a5096-147">A continuación, inicialice un objeto `changes` y agregue a él sus *notas*, a continuación, escríbalo en el almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="a5096-147">Next, initialize a `changes` object and add your *notes* to it, then write it to storage.</span></span>

```javascript
// Write a note
var changes = {};
changes["Note"] = myNote;
await storage.write(changes);
```
<span data-ttu-id="a5096-148">A continuación, acceda y actualice la nota más adelante, manteniendo su `eTag` que usted lee desde el almacén.</span><span class="sxs-lookup"><span data-stu-id="a5096-148">Then, access and update the note later, keeping its `eTag` that you read from the store.</span></span>
```javascript
// Read in a note
var note = await storage.read(["Note"]);
try {
    // Someone has updated the note. Need to update our local instance to match
    if(myNote.eTag != note.eTag){
        myNote = note;
    }

    // Add any updates to the note and write back out
    myNote.contents += ", bread";   // Add to the note
    changes["Note"] = note;
    await storage.write(changes); // Write the changes back to storage
    try {
        context.sendActivity('Successful write.');
    } catch (err) {
        context.sendActivity(`Write failed: ${err}`);
    }
}
catch (err) {
    context.sendActivity(`Unable to read the Note: ${err}`);
}
```
<span data-ttu-id="a5096-149">Si se ha actualizado la nota en el almacén antes de que haya escrito los cambios, la llamada a `Write` iniciará una excepción.</span><span class="sxs-lookup"><span data-stu-id="a5096-149">If the note was updated in the store before you write your changes, the call to `Write` will throw an exception.</span></span>

---

<span data-ttu-id="a5096-150">Para mantener la simultaneidad, siempre es necesario leer una propiedad desde el almacenamiento y luego modificar la propiedad que se ha leído, de esta manera `eTag` se mantiene.</span><span class="sxs-lookup"><span data-stu-id="a5096-150">To maintain concurrency, always read a property from storage, then modify the property you read, so that the `eTag` is maintained.</span></span> <span data-ttu-id="a5096-151">Si lee datos de usuario desde el almacén, la respuesta contendrá la propiedad de eTag.</span><span class="sxs-lookup"><span data-stu-id="a5096-151">If you read user data from the store, the response will contain the eTag property.</span></span> <span data-ttu-id="a5096-152">Si cambia los datos y escribe datos actualizados en el almacén, la solicitud debe incluir la propiedad de eTag que especifica el mismo valor que leyó anteriormente.</span><span class="sxs-lookup"><span data-stu-id="a5096-152">If you change the data and write updated data to the store, your request should include the eTag property that specifies the same value as you read earlier.</span></span> <span data-ttu-id="a5096-153">Sin embargo, escribir un objeto con su `eTag` establecida en `*` permitirá que la escritura sobrescriba cualquier otro cambio.</span><span class="sxs-lookup"><span data-stu-id="a5096-153">However, writing an object with its `eTag` set to `*` will allow the write to overwrite any other changes.</span></span>

<!-- If the ETag specified in your `Storage.Write()` request matches the current value in the store, the server will save the data and specify a new eTag value in the body of the response, that indicates that the data has been updated. If the ETag specified in your Storage.Write() request does not match the current value in the store, the bot responds with an error indicating an eTag conflict, to indicate that the user's data in the store has changed since you last saved or retrieved it. -->

<!-- TODO: new snippet -->

<!-- jf: I think this section can be cut entirely.

## Save a conversation reference using storage

You can use IStorage to save BotBuilder SDK objects as well as user-defined data. This code snippet uses `Storage.Write()` to save a ConversationReference object for use in sending a proactive message later.

# [C#](#tab/csharpwriteconvref)
```csharp
            var reference = context.ConversationReference;
            var userId = reference.User.Id;

            // save the ConversationReference to a global variable of this class
            conversationReference = reference;

            StoreItems storeItems = new StoreItems();
            StoreItem conversationReferenceToStore = new StoreItem();
            // set the eTag to "*" to indicate you're overwriting previous data
            conversationReferenceToStore.eTag = "*";
            conversationReferenceToStore.Add("ref", reference);
            storeItems[$"ConversationReference/{userId}"] = conversationReferenceToStore;

            await storage.Write(storeItems).ConfigureAwait(false);

            SubscribeUser(userId);

            await context.SendActivity("Thank You! We will message you shortly.");
        }

public void SubscribeUser(string userId)
{
    CreateContextForUserAsync(userId, async (ITurnContext context) =>
    {
        await context.SendActivity("You've been notified.");
        await Task.Delay(2000);
    });
                
}
```
# [JavaScript](#tab/jswriteconvref)
```javascript
const { MemoryStorage } = require('botbuilder');

const storage = new MemoryStorage();

// Listen for incoming activity 
server.post('/api/messages', (req, res) => {
    // Route received activity to adapter for processing
    adapter.processActivity(req, res, async (context) => {
        if (context.activity.type === 'message') {
            const utterances = (context.activity.text || '').trim().toLowerCase()
            if (utterances === 'subscribe') {
                const reference = context.activity;
                const userId = reference.id;
                const changes = {};
                changes['reference/' + userId] = reference;
                await storage.write(changes)
                await subscribeUser(userId)
                await context.sendActivity(`Thank You! We will message you shortly.`);
               
            } else{
                await context.sendActivity("Say 'subscribe'");
            }
    
        }
    });
});
```
---

To read the saved object from storage, call `Storage.Read()`.

# [C#](#tab/csharpread)
```csharp


        private async void CreateContextForUserAsync(String userId,Func<ITurnContext, Task> onReady)
        {
            var referenceKey = $"ConversationReference/{userId}";
            
            ConversationReference localRef = null;
            StoreItems conversationReferenceFromStorage = await storage.Read(referenceKey);
            foreach (var item in conversationReferenceFromStorage)
            {
                StoreItem value = item.Value as StoreItem;
                localRef = value["ref"];
            }
            
            await bot.CreateContext(this.conversationReference, onReady);

        }
```
# [JavaScript](#tab/jsread)
```javascript
async function subscribeUser(userId) {
    setTimeout(() => {
        createContextForUser(userId, (context) => {
            context.sendActivity("You have been notified");
        })
    }, 2000);
}

async function createContextForUser(userId, callback) {
    const referenceKey = 'reference/' + userId;
    var rows = await storage.read([referenceKey])
    var reference = await rows[referenceKey]
    await callback(adapter.createContext(reference))
          
}
```
---

-->

## <a name="next-steps"></a><span data-ttu-id="a5096-154">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="a5096-154">Next steps</span></span>

<span data-ttu-id="a5096-155">Ahora que sabe cómo leer lectura y escritura directamente desde el almacenamiento, eche un vistazo a cómo puede usar el Administrador de estado para que lo haga por usted.</span><span class="sxs-lookup"><span data-stu-id="a5096-155">Now that you know how to read read and write directly from storage, lets take a look at how you can use the state manager to do that for you.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="a5096-156">Guardar el estado mediante las propiedades de usuario y de conversación</span><span class="sxs-lookup"><span data-stu-id="a5096-156">Save state using conversation and user properties</span></span>](bot-builder-howto-v4-state.md)

## <a name="additional-resources"></a><span data-ttu-id="a5096-157">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="a5096-157">Additional resources</span></span>

- [<span data-ttu-id="a5096-158">Concepto: Almacenamiento en Bot Builder SDK</span><span class="sxs-lookup"><span data-stu-id="a5096-158">Concept: Storage in the Bot Builder SDK</span></span>](bot-builder-storage-concept.md)


