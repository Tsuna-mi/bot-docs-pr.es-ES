---
title: Escritura directa en el almacenamiento | Microsoft Docs
description: Aprenda a escribir directamente en almacenamiento con la versión 4 de Bot Builder SDK para .NET.
keywords: almacenamiento, lectura y escritura, almacenamiento en memoria, eTag
author: DeniseMak
ms.author: v-demak
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 05/2/18
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: 76f8976aefe3d4fefcffc46e691dbd0b35e41ec7
ms.sourcegitcommit: 1abc32353c20acd103e0383121db21b705e5eec3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/21/2018
ms.locfileid: "42756617"
---
# <a name="write-directly-to-storage"></a>Escritura directa en el almacenamiento

<!--
 Note for V4: You can write directly to storage without using the state manager. Therefore, this topic isn't called "managing state". State is in a separate topic.
 ## Manage state data by writing directly to storage
-->
[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]

Puede escribir directamente en el almacenamiento sin usar el objeto de contexto o el middleware, leyendo y escribiendo directamente en el objeto de almacenamiento.

Este ejemplo de código muestra cómo leer y escribir datos en el almacenamiento. En este ejemplo, el almacenamiento es un archivo, pero puede cambiar fácilmente el código para inicializar el objeto de almacenamiento utilizando almacenamiento en memoria de Azure Table Storage en su lugar. 

# <a name="ctabcsharpechorproperty"></a>[C#](#tab/csharpechorproperty)
Se deberá definir un objeto y usar `IStorage.Write` y `IStorage.Read` para guardar y recuperar el estado. 

El ejemplo siguiente agrega todos los mensajes del usuario a una lista. La estructura de datos que contiene la lista se guarda en un archivo dentro del directorio que proporcione al constructor de `FileStorage`.

Comience con la plantilla EchoBot de Visual Studio en Bot Builder SDK V4.
Edite el archivo `EchoBot.cs` . Agregue las siguientes instrucciones using y reemplace la definición de clase `EchoBot`.

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
# <a name="javascripttabjsechoproperty"></a>[JavaScript](#tab/jsechoproperty)

El ejemplo siguiente agrega todos los mensajes del usuario a una lista. La estructura de datos que contiene la lista se guarda en un archivo dentro del directorio que proporcione al constructor de `FileStorage`.

Pegue lo siguiente código en `app.js`.
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

## <a name="manage-concurrency-using-etags"></a>Administración de la simultaneidad mediante eTags

En el ejemplo anterior, ha establecido la propiedad `eTag` en `*`. El miembro `eTag` (etiqueta de entidad) de su objeto de almacén sirve para administrar la simultaneidad. La etiqueta `eTag` indica qué hacer si otra instancia del bot ha cambiado el objeto en almacenamiento en el que está escribiendo su bot. 

<!-- define optimistic concurrency -->

### <a name="last-write-wins---allow-overwrites"></a>La última escritura tiene prioridad: permitir la sobrescritura

Establezca el valor de eTag en `*` para permitir que otras instancias del bot sobrescriban los datos escritos previamente. Un valor de propiedad `eTag` de asterisco (`*`) indica que la última escritura tiene prioridad. Al crear un nuevo almacén de datos, puede establecer la etiqueta `eTag` de una propiedad en `*` para indicar que no ha guardado previamente los datos que está escribiendo, o que desea que el último escritor sobrescriba cualquier propiedad guardada previamente. Si la simultaneidad no es un problema para el bot, establecer la propiedad `eTag` en `*` para los datos que se están escribiendo permitirá la sobrescritura.

Esto se muestra en el ejemplo de código siguiente.

# <a name="ctabcsetagoverwrite"></a>[C#](#tab/csetagoverwrite)
Utilice la clase `UtteranceLog` que hemos definido anteriormente.
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
# <a name="javascripttabjstagoverwrite"></a>[JavaScript](#tab/jstagoverwrite)
Incorporación de una expresión nueva al registro y autorización de la sobrescritura.

```javascript
storeItems["UtteranceLog"] = { UtteranceList: [`${utterance}`], "eTag": "*" }
await storage.write(storeItems)
```
---

### <a name="maintain-concurrency-and-prevent-overwrites"></a>Mantenimiento de la simultaneidad y evitación de la sobrescritura
Use un valor distinto de `*` para la `eTag` si desea evitar el acceso simultáneo a una propiedad, para evitar los cambios por sobrescritura de otra instancia del bot. El bot recibe una respuesta de error con el mensaje `etag conflict key=` cuando intenta guardar los datos de estado y la etiqueta `eTag` no es igual que en el almacenamiento. <!-- To control concurrency of data that is stored using `IStorage`, the BotBuilder SDK checks the entity tag (ETag) for `Storage.Write()` requests. -->

De forma predeterminada, el almacén comprueba la igualdad de la propiedad `eTag` de un objeto de almacenamiento cada vez que un bot escribe en ese elemento y, a continuación, lo actualiza a un nuevo valor único después de cada escritura. Si la propiedad `eTag` en la escritura no coincide con la `eTag` en el almacenamiento, significa que otro subproceso o bot ha cambiado los datos. 

Por ejemplo, supongamos que desea que su bot edite una nota guardada, pero no quiere que sobrescriba los cambios que haya realizado otra instancia del bot. Si otra instancia del bot ha realizado modificaciones, lo que quiere es que el usuario edite la versión con las actualizaciones más recientes.

# <a name="ctabcsetag"></a>[C#](#tab/csetag)
Primero, cree una clase que implemente `IStoreItem`.
```csharp
public class Note : IStoreItem
{
    public string Name { get; set; }
    public string Contents { get; set; }
    public string eTag { get; set; }
}
```
A continuación, cree una nota inicial mediante la creación de un objeto de almacenamiento y agregue el objeto a su almacén.
```csharp
// create a note for the first time, with a non-null, non-* eTag.
var note = new Note { Name = "Shopping List", Contents = "eggs", eTag = "x" };

var changes = new KeyValuePair<string, object>[]
{
    new KeyValuePair<string, object>("Note", note)
};
await NoteStore.Write(changes);
```
A continuación, acceda y actualice la nota más adelante, manteniendo su `eTag` que usted lee desde el almacén.
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
Si se ha actualizado la nota en el almacén antes de que haya escrito los cambios, la llamada a `Write` iniciará una excepción.

# <a name="javascripttabjsetag"></a>[JavaScript](#tab/jsetag)

Primero, cree un objeto`myNote`.
```javascript
var myNote = {
    name: "Shopping List",
    contents: "eggs",
    eTag: "*"
}
```
A continuación, inicialice un objeto `changes` y agregue a él sus *notas*, a continuación, escríbalo en el almacenamiento.

```javascript
// Write a note
var changes = {};
changes["Note"] = myNote;
await storage.write(changes);
```
A continuación, acceda y actualice la nota más adelante, manteniendo su `eTag` que usted lee desde el almacén.
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
Si se ha actualizado la nota en el almacén antes de que haya escrito los cambios, la llamada a `Write` iniciará una excepción.

---

Para mantener la simultaneidad, siempre es necesario leer una propiedad desde el almacenamiento y luego modificar la propiedad que se ha leído, de esta manera `eTag` se mantiene. Si lee datos de usuario desde el almacén, la respuesta contendrá la propiedad de eTag. Si cambia los datos y escribe datos actualizados en el almacén, la solicitud debe incluir la propiedad de eTag que especifica el mismo valor que leyó anteriormente. Sin embargo, escribir un objeto con su `eTag` establecida en `*` permitirá que la escritura sobrescriba cualquier otro cambio.

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

## <a name="next-steps"></a>Pasos siguientes

Ahora que sabe cómo leer lectura y escritura directamente desde el almacenamiento, eche un vistazo a cómo puede usar el Administrador de estado para que lo haga por usted.

> [!div class="nextstepaction"]
> [Guardar el estado mediante las propiedades de usuario y de conversación](bot-builder-howto-v4-state.md)

## <a name="additional-resources"></a>Recursos adicionales

- [Concepto: Almacenamiento en Bot Builder SDK](bot-builder-storage-concept.md)


