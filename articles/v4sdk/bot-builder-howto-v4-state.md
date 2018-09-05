---
title: Administración del estado de la conversación y del usuario | Microsoft Docs
description: Obtenga información sobre cómo guardar y recuperar datos con el SDK V4 de Bot Builder para .NET.
keywords: estado de la conversación, estado del usuario, software intermedio de estado, flujo de la conversación, almacenamiento de archivos, almacenamiento de tablas de azure
author: ivorb
ms.author: v-demak
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 05/03/18
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: a74c52af0ca56b62491ca3aa39d09885c2540c18
ms.sourcegitcommit: ee63d9dc1944a6843368bdabf5878950229f61d0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/23/2018
ms.locfileid: "42795214"
---
# <a name="manage-conversation-and-user-state"></a>Administración del estado de la conversación y del usuario

[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]

Para que el bot guarde el estado del usuario y la conversación, inicialice primero el software intermedio de administrador de estado y, después, use las propiedades de estado de usuario y de conversación.
Para obtener más información sobre cómo se usa ese estado, vea [Estado y almacenamiento](./bot-builder-storage-concept.md).

## <a name="initialize-state-manager-middleware"></a>Inicializar el software intermedio de administrador de estado

En el SDK, debe inicializar el adaptador de bot para usar el software intermedio de administrador de estado con el fin de poder usar los almacenes de propiedades de conversación o usuario. El _estado de la conversación_ se usa para las propiedades de conversación, y el _estado de usuario_ para las de usuario. (Se puede acceder a las propiedades de estado del usuario entre varias conversaciones). El software intermedio de administrador de estado proporciona una abstracción que permite acceder a las propiedades mediante un simple valor de clave o almacén de objeto, independiente del tipo de almacenamiento subyacente. El administrador de estado se encarga de escribir los datos en el almacenamiento y de administrar la simultaneidad, con independencia de que el tipo de almacenamiento subyacente sea en memoria, de archivos o Azure Table Storage.


# <a name="ctabcsharp"></a>[C#](#tab/csharp)
Para ver cómo se inicializa `ConversationState`, vea `Startup.cs` en el ejemplo Microsoft.Bot.Samples.EchoBot AspNetCore.

Bibliotecas necesarias para este código:

```csharp
using Microsoft.Bot.Builder.BotFramework;
using Microsoft.Bot.Builder.Core.Extensions;
using Microsoft.Bot.Builder.Integration.AspNet.Core;
```

Inicializando `ConversationState`:

```csharp
services.AddBot<EchoBot>(options =>
{
    options.CredentialProvider = new ConfigurationCredentialProvider(Configuration);

    IStorage dataStore = new MemoryStorage();
    options.Middleware.Add(new ConversationState<EchoState>(dataStore));
});
```

En la línea `options.Middleware.Add(new ConversationState<EchoState>(dataStore));`, `ConversationState` es el objeto de administrador de estado de la conversación, que se agrega al bot como software intermedio. El parámetro de tipo `EchoState` es el tipo que representa cómo se quiere almacenar la información de estado de la conversación. Un bot puede usar cualquier tipo de clase para los datos de estado de usuario o de conversación.

La implementación de `EchoState` está en `EchoBot.cs`:
```csharp
public class EchoState
{
    public int TurnNumber { get; set; }
}
``` 

# <a name="javascripttabjs"></a>[JavaScript](#tab/js)

Defina el proveedor de almacenamiento y asígnelo al administrador de estado que quiera usar.
Puede usar el mismo proveedor de almacenamiento para el software intermedio de administración de `ConversationState` y `UserState`.
Después, se usaría la biblioteca `BotStateSet` para conectarlo a la capa de software intermedio que va a administrar la persistencia de los datos de forma automática.

```javascript
const { BotFrameworkAdapter, MemoryStorage, ConversationState, UserState, BotStateSet } = require('botbuilder');
const restify = require('restify');

// Create adapter
const adapter = new BotFrameworkAdapter({ 
    appId: process.env.MICROSOFT_APP_ID, 
    appPassword: process.env.MICROSOFT_APP_PASSWORD 
});

// Add conversation state middleware.
const conversationState = new ConversationState(new MemoryStorage());
adapter.use(conversationState);

// Alternatively, use both conversation and user state middleware.
// const storage = new MemoryStorage;
// const conversationState = new ConverstationState(storage);
// const userState = new UserState(storage);
// adapter.use(new BotStateSet(conversationState, userState));
```    
---

> [!NOTE] 
> El almacenamiento de datos en memoria solo está pensado para pruebas. Este tipo de almacenamiento es volátil y temporal. Los datos se borran cada vez que se reinicia el bot. Vea [Almacenamiento de archivos](#file-storage) y [Almacenamiento de tablas de Azure](#azure-table-storage) más adelante en este artículo para configurar otros medios de almacenamiento subyacentes para el estado de la conversación y el usuario. 

### <a name="configuring-state-manager-middleware"></a>Configuración del software intermedio de administrador de estado

Al inicializar el software intermedio de estado, un parámetro de _configuración de estado_ opcional permite cambiar el comportamiento predeterminado de cómo se guardan las propiedades. La configuración predeterminada es la siguiente:

* Conservar las propiedades más allá de la duración del contexto de turno.
* Si más de una instancia del bot escribe en una propiedad, se permite que la última instancia del bot sobrescriba la anterior.

## <a name="use-conversation-and-user-state-properties"></a>Uso de propiedades de estado de conversación y usuario 
<!-- middleware and message context properties -->

Una vez que se ha configurado el software intermedio de administrador de estado, puede obtener las propiedades de estado de usuario y conversación del objeto de contexto.
<!-- Changes are written to storage before the `SendActivity()` pipeline completes. -->

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

Puede ver cómo funciona mediante el ejemplo `Microsoft.Bot.Samples.EchoBot` del SDK de Bot Builder. 

En el controlador `OnTurn`, `context.GetConversationState` hace que el estado de la conversación acceda a los datos que se han definido y el estado se puede modificar mediante las propiedades (en este caso, incrementando `TurnNumber`).

```csharp
public async Task OnTurn(ITurnContext context)
{
    // This bot is only handling Messages
    if (context.Activity.Type == ActivityTypes.Message)
    {
        // Get the conversation state from the turn context
        var state = context.GetConversationState<EchoState>();

        // Bump the turn count. 
        state.TurnCount++;

        // Echo back to the user whatever they typed.
        await context.SendActivity($"Turn {state.TurnCount}: You sent '{context.Activity.Text}'");
    }
}
```   

# <a name="javascripttabjs"></a>[JavaScript](#tab/js)

Puede ver cómo funciona esto mediante el EchoBot generado desde el ejemplo de generador de Yeoman.

En este ejemplo de código se muestra cómo se puede almacenar un contador de turnos en el estado de la conversación.

```javascript
const { BotFrameworkAdapter, MemoryStorage, ConversationState } = require('botbuilder');
const restify = require('restify');

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

// Add conversation state middleware
const conversationState = new ConversationState(new MemoryStorage());
adapter.use(conversationState);

// Listen for incoming requests 
server.post('/api/messages', (req, res) => {
    // Route received request to adapter for processing
    adapter.processActivity(req, res, async (context) => {
        if (context.activity.type === 'message') {
            const state = conversationState.get(context);
            const count = state.count === undefined ? state.count = 0 : ++state.count;
            await context.sendActivity(`${count}: You said "${context.activity.text}"`);
        } else {
            await context.sendActivity(`[${context.activity.type} event detected]`);
        }
    });
});
```
---

## <a name="using-conversation-state-to-direct-conversation-flow"></a>Uso del estado de la conversación para dirigir el flujo de la conversación

Al diseñar un flujo de conversación, es útil definir un indicador de estado para dirigir el flujo de la conversación. La marca puede ser un simple tipo **booleano** o un tipo que incluya el nombre del tema actual. La marca puede ayudar a realizar el seguimiento de dónde se encuentra en una conversación. Por ejemplo, una marca de tipo **booleano** puede indicar si se encuentra en una conversación o no. Mientras que una propiedad de nombre de tema puede indicar en qué conversación se encuentra en la actualidad.

En el ejemplo siguiente se usa una propiedad _have asked name_ booleana para marcar cuándo el bot ha solicitado el nombre del usuario. Cuando se recibe el mensaje siguiente, el bot comprueba la propiedad. Si la propiedad está establecida en `true`, el bot sabe que al usuario se le acaba de solicitar el nombre e interpreta el mensaje entrante como un nombre que se va a guardar como una propiedad de usuario.


# <a name="ctabcsharp"></a>[C#](#tab/csharp)
```csharp
public class ConversationInfo
{
    public bool haveAskedNameFlag { get; set; }
    public bool haveAskedNumberFlag { get; set; }
}

public class UserInfo
{
    public string name { get; set; }
    public string telephoneNumber { get; set; }
    public bool done { get; set; }
}

public async Task OnTurn(ITurnContext context)
{
    // Get state objects. Default objects are created if they don't already exist.
    var convo = ConversationState<ConversationInfo>.Get(context);
    var user = UserState<UserInfo>.Get(context);

    if (context.Activity.Type is ActivityTypes.Message)
    {
        if (string.IsNullOrEmpty(user.name) && !convo.haveAskedNameFlag)
        {
            // Ask for the name.
            await context.SendActivity("Hello. What's your name?");

            // Set flag to show we've asked for the name. We save this out so the
            // context object for the next turn of the conversation can check haveAskedName
            convo.haveAskedNameFlag = true;
        }
        else if (!convo.haveAskedNumberFlag)
        {
            // Save the name.
            var name = context.Activity.AsMessageActivity().Text;
            user.name = name;
            convo.haveAskedNameFlag = false; // Reset flag

            // Ask for the phone number. You might want a flag to track this, too.
            await context.SendActivity($"Hello, {name}. What's your telephone number?");
            convo.haveAskedNumberFlag = true;
        }
        else if (convo.haveAskedNumberFlag)
        {
            // save the telephone number
            var telephonenumber = context.Activity.AsMessageActivity().Text;

            user.telephoneNumber = telephonenumber;
            convo.haveAskedNumberFlag = false; // Reset flag
            await context.SendActivity($"Got it. I'll call you later.");
        }
    }
}
```

Para configurar el estado de usuario para que `UserState<UserInfo>.Get(context)` pueda devolverlo, se agrega software intermedio de estado de usuario. Por ejemplo, en `Startup.cs` de EchoBot de ASP.NET Core, cambie el código de ConfigureServices.cs:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddBot<EchoBot>(options =>
    {
        options.CredentialProvider = new ConfigurationCredentialProvider(Configuration);
        
        IStorage dataStore = new MemoryStorage();
        options.Middleware.Add(new ConversationState<ConversationInfo>(dataStore));
        options.Middleware.Add(new UserState<UserInfo>(dataStore));
    });
}
```

# <a name="javascripttabjs"></a>[JavaScript](#tab/js)

**app.js**

```js
const { BotFrameworkAdapter, MemoryStorage, ConversationState, UserState, BotStateSet } = require('botbuilder');
const restify = require('restify');

// Create server
let server = restify.createServer();
server.listen(process.env.port || process.env.PORT || 3978, function () {
    console.log(`${server.name} listening to ${server.url}`);
});

// Create adapter (it's ok for MICROSOFT_APP_ID and MICROSOFT_APP_PASSWORD to be blank for now)  
const adapter = new BotFrameworkAdapter({ 
    appId: process.env.MICROSOFT_APP_ID, 
    appPassword: process.env.MICROSOFT_APP_PASSWORD 
});

// Storage
const storage = new MemoryStorage();
const conversationState = new ConversationState(storage);
const userState  = new UserState(storage);
adapter.use(new BotStateSet(conversationState, userState));

// Listen for incoming requests 
server.post('/api/messages', (req, res) => {
    // Route received request to adapter for processing
    adapter.processActivity(req, res, async (context) => {
        const isMessage = (context.activity.type === 'message');
        const convo = conversationState.get(context);
        const user = userState.get(context);

        if (isMessage) {
            if(!user.name && !convo.haveAskedNameFlag){
                // Ask for the name.
                await context.sendActivity("What is your name?")
                // Set flag to show we've asked for the name. We save this out so the
                // context object for the next turn of the conversation can check haveAskedNameFlag
                convo.haveAskedNameFlag = true;
            } else if(convo.haveAskedNameFlag){
                // Save the name.
                user.name = context.activity.text;
                convo.haveAskedNameFlag = false; // Reset flag

                await context.sendActivity(`Hello, ${user.name}. What's your telephone number?`);
                convo.haveAskedNumberFlag = true; // Set flag
            } else if(convo.haveAskedNumberFlag){
                // save the phone number
                user.telephonenumber = context.activity.text;
                convo.haveAskedNumberFlag = false; // Reset flag
                await context.sendActivity(`Got it. I'll call you later.`);
            }
        }

        // ...
    });
});

```

---

Una alternativa consiste en usar el modelo de _cascada_ de un cuadro de diálogo. El cuadro de diálogo realiza el seguimiento del estado de la conversación de forma automática, por lo que no es necesario crear marcas para realizar el seguimiento del estado. Para más información, consulte [Administración de conversaciones simples con diálogos](bot-builder-dialog-manage-conversation-flow.md).

## <a name="file-storage"></a>File Storage

El proveedor de almacenamiento de memoria usa el almacenamiento en memoria que se elimina cuando se reinicia el bot. Solo es adecuado con fines de prueba. Si quiere conservar los datos, pero no quiere enlazar el bot a una base de datos, puede usar el proveedor de almacenamiento de archivos. Aunque este proveedor también está diseñado con fines de prueba, conserva los datos de estado en un archivo que se puede inspeccionar. Los datos se escriben en el archivo con formato JSON.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

Vaya a `Startup.cs` en el ejemplo Microsoft.Bot.Samples.EchoBot-AspNetCore y modifique el código del método `ConfigureServices`.
```csharp
// This method gets called by the runtime. Use this method to add services to the container.
public void ConfigureServices(IServiceCollection services)
{
    services.AddBot<EchoBot>(options =>
    {
        options.CredentialProvider = new ConfigurationCredentialProvider(Configuration);

        // Using file storage instead of in-memory storage.
        IStorage dataStore = new FileStorage(System.IO.Path.GetTempPath());
        options.Middleware.Add(new ConversationState<EchoState>(dataStore));
    });
}
``` 

Ejecute el código y deje que EchoBot devuelva la entrada varias veces.

Después, vaya al directorio especificado por `System.IO.Path.GetTempPath()`. Debería ver un archivo con un nombre que empieza por "conversation". Ábralo y examine el código JSON. El archivo contiene algo similar a lo siguiente:
```json
{
  "$type": "Microsoft.Bot.Samples.Echo.EchoState, Microsoft.Bot.Samples.EchoBot",
  "TurnNumber": "3",
  "eTag": "ecfe2a23566b4b52b2fe697cffc59385"
}
```

`$type` especifica el tipo de estructura de datos que se usa en el bot para almacenar el estado de la conversación. El campo `TurnNumber` se corresponde con la propiedad `TurnNumber` de la clase `EchoState`. El campo `eTag` se hereda de `IStoreItem` y es un valor único que se actualiza de forma automática cada vez que el bot actualiza el estado de la conversación.  El campo eTag permite al bot habilitar la simultaneidad optimista.

# <a name="javascripttabjs"></a>[JavaScript](#tab/js)

Para usar `FileStorage`, actualice el ejemplo EchoBot que se describe en la sección [Uso de propiedades de estado de usuario y conversación](#use-conversation-and-user-state-properties) anterior. Asegúrese de que `storage` está establecido en `FileStorage` en lugar de `MemoryStorage` y que requiere `FileStorage` de botbuilder. Estos son los únicos cambios necesarios. 

```javascript
// Storage
const storage = new FileStorage("c:/temp");
const conversationState = new ConversationState(storage);
const userState  = new UserState(storage);
adapter.use(new BotStateSet(conversationState, userState));
```

El proveedor `FileStorage` toma una "ruta de acceso" como parámetro. Especificar una ruta de acceso permite encontrar fácilmente el archivo con la información guardada desde el bot. Se creará un archivo para cada *conversación*. Por tanto, en la *ruta de acceso*, encontrará varios nombres de archivo que empiezan por `conversation!`. Puede ordenar por fecha para encontrar la conversación más reciente más fácilmente. Por otro lado, solo encontrará un archivo para el estado del *usuario*. El nombre de archivo comenzará por `user!`. Siempre que cambie el estado de cualquiera de estos objetos, el administrador de estado actualizará el archivo para reflejar lo que ha cambiado.

Ejecute el bot y envíele varios mensajes. Después, busque el archivo de almacenamiento y ábralo. Este es el aspecto que es posible que tenga el contenido de JSON para el bot de eco que realiza el seguimiento de los contadores de turnos.

```json
{
  "turnNumber": "3",
  "eTag": "322"
}
```
---

## <a name="azure-table-storage"></a>Almacenamiento de tablas de Azure

También puede usar Azure Table Storage como el medio de almacenamiento.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

En el ejemplo Microsoft.Bot.Samples.EchoBot-AspNetCore, agregue una referencia al paquete NuGet `Microsoft.Bot.Builder.Azure`.

Después, vaya a `Startup.cs`, agregue una instrucción `using Microsoft.Bot.Builder.Azure;` y modifique el código del método `ConfigureServices`.
```csharp
services.AddBot<EchoBot>(options =>
{
    options.CredentialProvider = new ConfigurationCredentialProvider(Configuration);
    // The parameters are the connection string and table name.
    // "UseDevelopmentStorage=true" is the connection string to use if you are using the Azure Storage Emulator.
    // Replace it with your own connection string if you're not using the emulator
    options.Middleware.Add(new ConversationState<EchoState>(new AzureTableStorage("UseDevelopmentStorage=true","conversationstatetable")));
    // you could also specify the cloud storage account instead of the connection string
    /* options.Middleware.Add(new ConversationState<EchoState>(
        new AzureTableStorage(WindowsAzure.Storage.CloudStorageAccount.DevelopmentStorageAccount, "conversationstatetable"))); */
    options.EnableProactiveMessages = true;
});
```
`UseDevelopmentStorage=true` es la cadena de conexión que se puede usar con el [Emulador de Azure Storage][AzureStorageEmulator]. Reemplácela por una cadena de conexión propia si no usa el emulador.

Si no existe la tabla con el nombre especificado en el constructor para `AzureTableStorage`, se crea.

<!-- 
TODO: step-by-step inspection of the stored table
-->

# <a name="javascripttabjs"></a>[JavaScript](#tab/js)

En `app.js` del ejemplo EchoBot, puede crear ConversationState mediante `AzureTableStorage`. 

```bash
npm install --save botbuilder-azure@preview
```

```javascript
const { BotFrameworkAdapter, FileStorage, MemoryStorage, ConversationState } = require('botbuilder');
const { TableStorage } = require('botbuilder-azure');

// ...

// Create adapter
const adapter = new BotFrameworkAdapter({
    appId: process.env.MICROSOFT_APP_ID,
    appPassword: process.env.MICROSOFT_APP_PASSWORD
});

// Add conversation state middleware
// The parameters are the connection string and table name.
// "UseDevelopmentStorage=true" is the connection string to use if you are using the Azure Storage Emulator.
// Replace it with your own connection string if you're not using the emulator
var azureStorage = new TableStorage({ tableName: "TestAzureTable1", storageAccountOrConnectionString: "UseDevelopmentStorage=true"})

// You can alternatively use your account name and table name
// var azureStorage = new TableStorage({tableName: "TestAzureTable2", storageAccessKey: "V3ah0go4DLkMtQKUPC6EbgFeXnE6GeA+veCwDNFNcdE6rqSVE/EQO/kjfemJaitPwtAkmR9lMKLtcvgPhzuxZg==", storageAccountOrConnectionString: "storageaccount"});

const conversationState = new ConversationState(azureStorage);
adapter.use(conversationState);

```

`UseDevelopmentStorage=true` es la cadena de conexión que se puede usar con el [Emulador de Azure Storage][AzureStorageEmulator].
Si no existe la tabla con el nombre especificado en el constructor para `AzureTableStorage`, se crea.

---

Para ver los datos de estado de la conversación que se guardan, ejecute el ejemplo y, después, abra la tabla mediante el [Explorador de Azure Storage][AzureStorageExplorer].

![Datos de estado de la conversación de echobot en el Explorador de Azure Storage](media/how-to-state/echostate-azure-storage-explorer.png)


La **clave de partición** es una clave generada de forma exclusiva específica de la conversación actual. Si reinicia el bot o inicia una conversación nueva, la nueva conversación obtendrá su propia fila con su propia clave de partición. La estructura de datos `EchoState` se serializa en JSON y se guarda en la columna **Json** de la tabla de Azure. 

```json
{
    "$type":"Microsoft.Bot.Samples.Echo.AspNetCore.EchoState, Microsoft.Bot.Samples.EchoBot-AspNetCore",
    "TurnNumber":2,
    "LastMessage":"second message",
    "eTag":"*"
}
```

Los campos `$type` y `eTag` se agregan mediante el SDK de BotBuilder. Para obtener más información sobre las eTag, vea [Managing optimistic concurrency](bot-builder-howto-v4-storage.md#manage-concurrency-using-etags) (Administración de la simultaneidad optimista).


## <a name="next-steps"></a>Pasos siguientes

Ahora que sabe cómo usar el estado para leer y escribir datos de bot en el almacenamiento, veremos cómo puede leer y escribir directamente en el almacenamiento.

> [!div class="nextstepaction"]
> [Escritura directa en el almacenamiento](bot-builder-howto-v4-storage.md).

## <a name="additional-resources"></a>Recursos adicionales
Para obtener más información sobre el almacenamiento, vea [Almacenamiento en el SDK de Bot Builder](bot-builder-storage-concept.md)

<!-- Links -->
[AzureStorageEmulator]: https://docs.microsoft.com/azure/storage/common/storage-use-emulator
[AzureStorageExplorer]: https://azure.microsoft.com/features/storage-explorer/
