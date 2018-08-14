---
title: Creación de middleware mediante Cosmos DB | Microsoft Docs
description: Aprenda a crear middleware personalizado que se registre en Cosmos DB.
keywords: middleware, cosmos db, base de datos, azure, onTurn
author: ivorb
ms.author: v-ivorb
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 03/21/2018
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: 29edff2db0e513a37b8ca8ac0f97e0647282567d
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305248"
---
# <a name="create-middleware-that-logs-in-cosmos-db"></a>Creación de middleware que se registre en Cosmos DB

Aunque en el SDK se proporciona middleware útil, existen situaciones en las que deberá implementar su propio middleware para conseguir el objetivo deseado.

En este ejemplo, crearemos una capa de middleware que conecta con Cosmos DB para registrar todos los mensajes recibidos y las respuestas que devolvimos. El código que ve aquí está disponible como código fuente completo, proporcionado con nuestros [ejemplos](../dotnet/bot-builder-dotnet-samples.md).

## <a name="set-up"></a>Instalación

Es necesario configurar algunas cosas antes de meternos en el código.

### <a name="create-your-database"></a>Creación de la base de datos

En primer lugar, necesitamos un lugar donde almacenar la base de datos.  Para ello, podemos usar una cuenta de Azure, o, con fines de prueba, puede usar el [emulador de Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/local-emulator). Sea cual sea el método elegido, se necesita un URI de punto de conexión y una clave para la base de datos.

En el ejemplo y el código proporcionados aquí se usa el emulador. El punto de conexión y la clave son los proporcionados para la cuenta de prueba de Cosmos DB, que son valores comunes que se incluyen con la documentación del emulador y no se pueden usar para producción.

Si quiere usar una instancia de Azure Cosmos DB real, siga el paso 1 del tema [Introducción a Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/sql-api-get-started). Una vez hecho esto, el URI de punto de conexión y la clave están disponibles en la pestaña **Claves** de la configuración de la base de datos. Estos valores serán necesarios a continuación para nuestro archivo de configuración.

### <a name="build-a-basic-bot"></a>Creación de un bot básico

En el resto de este tema se trata la creación de un bot básico, como el HelloBot creado en la introducción. Puede usar [Bot Framework Emulator](https://github.com/Microsoft/BotFramework-Emulator) para conectarse al bot, conversar con él y probarlo.

Además de las referencias necesarias para los bots estándar que usan Bot Framework, deberá agregar referencias a los siguientes paquetes NuGet:

* Microsoft.Azure.Documentdb.Core
* System.Configuration.ConfigurationManager

Estas bibliotecas se usan para la comunicación con la base de datos y permiten el uso de un archivo de configuración, respectivamente.

### <a name="add-configuration-file"></a>Adición de archivo de configuración

El uso de un archivo de configuración es recomendable por varias razones, de modo que aquí usaremos uno. Nuestros datos de configuración son cortos y sencillos, pero puede agregar otras opciones de configuración a este archivo a medida que el bot se vuelva más complejo.

# <a name="ctabcs"></a>[C#](#tab/cs)
1. Agregue un archivo al proyecto llamado `app.config`.
2. Guarde en él la siguiente información. El punto de conexión y la clave se definen para el emulador local, pero se pueden actualizar para la instancia de Cosmos DB.
    ```
    <?xml version="1.0" encoding="utf-8"?>
    <configuration>
        <appSettings>
            <add key="EmulatorDbUrl" value="https://localhost:8081"/>
            <add key="EmulatorDbKey" value="C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw=="/>
        </appSettings>
    </configuration>
    ```
# <a name="javascripttabjs"></a>[JavaScript](#tab/js)
1. Agregue un archivo al proyecto llamado `config.js`.
2. Guarde en él la siguiente información. El punto de conexión y la clave se definen para el emulador local, pero se pueden actualizar para la instancia de Cosmos DB.
    ```javascript
        var config = {}

        config.EmulatorServiceEndpoint = "https://localhost:8081";
        config.EmulatorAuthKey = "C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWE+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==";
        config.database = {
            "id": "Tasks"
        };
        config.collection = {
            "id": "Items"
        };

        module.exports = config;
    ```

---

Si usa una instancia real de Cosmos DB, puede reemplazar los valores de las dos claves anteriores por los valores que tenga en la configuración de Cosmos DB. Como alternativa, puede agregar dos claves más a la configuración, lo que le permite cambiar entre el emulador para pruebas y la instancia real de Cosmos DB, como por ejemplo:

# <a name="ctabcs"></a>[C#](#tab/cs)
```
    <add key="ActualDbUrl" value="<your database URI>"/>
    <add key="ActualDbKey" value="<your database key>"/>
```
 Si agrega dos claves adicionales y quiere usar la base de datos real, no olvide especificar a continuación la clave correcta en el constructor en lugar de `EmulatorDbUrl` y `EmulatorDbKey`.

# <a name="javascripttabjs"></a>[JavaScript](#tab/js)
```
    config.ActualServiceEndpoint = "your database URI;
    config.ActualAuthKey = "your database key";
```

Si agrega dos claves adicionales y quiere usar la base de datos real, no olvide especificar a continuación la clave correcta en el constructor en lugar de `EmulatorServiceEndpoint` y `EmulatorAuthKey`.

---



## <a name="creating-your-middleware"></a>Creación del middleware

# <a name="ctabcs"></a>[C#](#tab/cs)
Antes de empezar a escribir la lógica real del middleware, cree para él una clase en el proyecto de bot. Cuando tenga esa clase, cambie el espacio de nombres por lo que usaremos en el resto de nuestro bot, `Microsoft.Bot.Samples`. Aquí verá que agregamos referencias a unas cuantas bibliotecas nuevas:

* `Newtonsoft.Json;`
* `Microsoft.Azure.Documents;`
* `Microsoft.Azure.Documents.Client;`
* `Microsoft.Azure.Documents.Linq;`

Los distintos `Microsoft.Azure.Documents.*` son para las distintas partes de la comunicación con nuestra base de datos y `Newtonsoft.Json` nos ayuda a analizar el código JSON que va y viene a nuestra base de datos.

Por último, cada fragmento de middleware implementa `IMiddleware`, que nos exige que definamos los controladores adecuados para que el middleware funcione correctamente.

```cs
using System;
using System.Collections.Generic;
using System.Threading.Tasks;
using System.Configuration;
using Newtonsoft.Json;
using Microsoft.Bot.Builder;
using Microsoft.Bot.Schema;
using Microsoft.Azure.Documents;
using Microsoft.Azure.Documents.Client;
using Microsoft.Azure.Documents.Linq;

namespace Microsoft.Bot.Samples
{
    public class CosmosMiddleware : IMiddleware
    {
        ...
```

# <a name="javascripttabjs"></a>[JavaScript](#tab/js)
Antes de empezar a escribir la lógica real del middleware, debemos crear nuestro middleware personalizado que se iniciará con `onTurn()`. 

```javascript
adapter.use({onTurn: async (context, next) =>{
    // Middleware logic here...
}})
    
```

---

#### <a name="defining-local-variables"></a>Definición de variables locales

# <a name="ctabcs"></a>[C#](#tab/cs)
A continuación, necesitamos variables locales para manipular nuestra base de datos y una clase para almacenar la información que queremos registrar. Esa clase de información, lo que llamamos `Log`, define el nombre que recibirán las propiedades JSON asociadas con cada miembro. Volveremos a ello más adelante.

```cs
    private string Endpoint;
    private string Key;
    private static readonly string Database = "BotData";
    private static readonly string Collection = "BotCollection";
    public DocumentClient docClient;

    public class Log
    {
        [JsonProperty("Time")]
        public string Time;

        [JsonProperty("MessageReceived")]
        public string Message;

        [JsonProperty("ReplySent")]
        public string Reply;
    }
```

# <a name="javascripttabjs"></a>[JavaScript](#tab/js)
A continuación, necesitamos una variable local para almacenar la información que queremos registrar. En el middleware, debemos acceder a conversationState con Cosmos DB como proveedor de almacenamiento. La variable `info` será el objeto del estado que se leerá y en el que se escribirá. 

```javascript
// Add conversation state middleware
const conversationState = new ConversationState(storage);
adapter.use(conversationState);

adapter.use({onTurn: async (context, next) =>{

    const info = conversationState.get(context);
    // More middleware logic 
}})
```

--- 


#### <a name="initialize-database-connection"></a>Inicialización de la conexión de base de datos

# <a name="ctabcs"></a>[C#](#tab/cs)
El constructor de este middleware se encarga de extraer la información necesaria de nuestro archivo de configuración definido anteriormente y de crear el elemento `DocumentClient`, que nos permite leer y escribir en nuestra base de datos. También nos aseguramos de que nuestra base de datos y colección estén configurados.

Para usar un nuevo elemento `DocumentClient` de manera adecuada es necesario deshacerse primero del anterior, y nuestro destructor hace justo eso.

La creación de nuestra base de datos depende de intentar realizar una lectura básica de la primera base de datos y, después, de la colección que contiene. Si recibimos una excepción `NotFound`, la aceptamos y creamos la base de datos o colección en lugar de generar la pila. En la mayoría de los casos ya se habrán creado, pero para los fines de este ejemplo esto nos permite olvidarnos de la creación de esa base de datos antes de la primera ejecución. En el código de ejemplo, la base de datos y la colección se dividen en dos funciones por motivos de simplicidad, pero se han combinado en el siguiente fragmento de código.

Para más información sobre COSMOS DB, eche un vistazo a su [sitio web](https://docs.microsoft.com/en-us/azure/cosmos-db/). En el resto de este tema, analizaremos los detalles de Cosmos DB propiamente dichos y nos centraremos en lo que necesita el bot para usarlo.

La definición de `Key`, `Endpoint` y `docClient` se incluyó en un fragmento de código anterior y solo se ha copiado aquí para una mayor claridad.

```cs
    private string Endpoint;
    private string Key;
    // ...
    public DocumentClient docClient;
    // ...

    public CosmosMiddleware()
    {
        Endpoint = ConfigurationManager.AppSettings["EmulatorDbUrl"];
        Key = ConfigurationManager.AppSettings["EmulatorDbKey"];
        docClient = new DocumentClient(new Uri(Endpoint), Key);
        CreateDatabaseAndCollection().ConfigureAwait(false);
    }

    ~CosmosMiddleware()
    {
        docClient.Dispose();
    }

    private async Task CreateDatabaseAndCollection()
    {
        try
        {
            await docClient.ReadDatabaseAsync(UriFactory.CreateDatabaseUri(Database));
        }
        catch (DocumentClientException e)
        {
            if (e.StatusCode == System.Net.HttpStatusCode.NotFound)
            {
                await docClient.CreateDatabaseAsync(new Database { Id = Database });
            }
            else
            {
                throw;
            }
        }

        try
        {
            await docClient.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri
                (Database, Collection));
        }
        catch (DocumentClientException e)
        {
            if (e.StatusCode == System.Net.HttpStatusCode.NotFound)
            {
                await docClient.CreateDocumentCollectionAsync(
                    UriFactory.CreateDatabaseUri(Database),
                    new DocumentCollection { Id = Collection });
            }
            else
            {
                throw;
            }
        }
    }
```

# <a name="javascripttabjs"></a>[JavaScript](#tab/js)
La creación de nuestra base de datos depende de intentar realizar una lectura básica de la primera base de datos y, después, de la colección que contiene. Si recibimos `undefined`, lo aceptamos y creamos un objeto llamado `log` que se establecerá en una matriz vacía. En la mayoría de los casos `log` ya se habrá creado, pero para los fines de este ejemplo esto nos permite olvidarnos de la creación de esa base de datos antes de la primera ejecución. En el ejemplo de código, comprobamos si `info.log` es indefinido. Si es así, establézcalo en una matriz vacía.

```javascript
adapter.use({onTurn: async (context, next) =>{

    const info = conversationState.get(context);

    if(info.log == undefined){
        info.log = [];
    }

    // More bot logic below
}})
```
---

#### <a name="read-from-database-logic"></a>Lectura de la lógica de la base de datos

La última función auxiliar lee nuestra base de datos y devuelve el número de registros especificado más reciente. Merece la pena mencionar que existen procedimientos de base de datos para recuperar los datos mejores que los que usamos aquí, especialmente cuando el almacén de datos es bastante más grande.

# <a name="ctabcs"></a>[C#](#tab/cs)
```cs
    public async Task<string> ReadFromDatabase(int numberOfRecords)
    {
        var documents = docClient.CreateDocumentQuery<Log>(
                UriFactory.CreateDocumentCollectionUri(Database, Collection))
                .AsDocumentQuery();
        List<Log> messages = new List<Log>();
        while (documents.HasMoreResults)
        {
            messages.AddRange(await documents.ExecuteNextAsync<Log>());
        }

        // Create a sublist of messages containing the number of requested records.
        List<Log> messageSublist = messages.GetRange(messages.Count - numberOfRecords, numberOfRecords);

        string history = "";

        // Send the last 3 messages.
        foreach (Log logEntry in messageSublist)
        {
            history += ("Message was: " + logEntry.Message + " Reply was: " + logEntry.Reply + "  ");
        }

        return history;
    }
```

# <a name="javascripttabjs"></a>[JavaScript](#tab/js)

El código siguiente está todavía dentro de la función `onTurn()` y se invocará si el usuario inserta la cadena "historial".

```javascript
adapter.use({onTurn: async (context, next) =>{

    const utterance = (context.activity.text || '').trim().toLowerCase();
    const isMessage = context.activity.type === 'message';
    const info = conversationState.get(context);

    if(info.log == undefined){
        info.log = [];
    }

    if(isMessage && utterance == 'history'){
        // Loop through the info.log array
        var logHistory = "";
        for(var i = 0; i < info.log.length; i++){
            logHistory += info.log[i] + " ";
        }
        await context.sendActivity(logHistory);
        // Short circuit the middleware
        return
    }
    //...
}})

```

---

#### <a name="define-onturn"></a>Definición de OnTurn()

El método `OnTurn()` de middleware estándar controla el resto del trabajo. Solo queremos realizar el registro cuando la actividad actual es un mensaje, lo que comprobamos antes y después de llamar a `next()`.

Lo primero que comprobamos es si este es nuestro mensaje de caso especial, donde el usuario pregunta por el historial más reciente. Si es así, invocamos la lectura de nuestra base de datos para obtener los registros más recientes y los enviamos a la conversación. Como la actividad actual se controla completamente, bloqueamos la canalización en lugar de pasar la ejecución.

Para obtener la respuesta que envía el bot, creamos un controlador cada vez que recibimos un mensaje para captar esas respuestas, que se puede ver en la expresión lambda que proporcionamos a `OnSendActivity()`. Se genera una cadena para recopilar todos los mensajes enviados a través de `SendActivity()` para este objeto de contexto.

Una vez que la ejecución devuelve la canalización de `next()`, montamos nuestros datos de registro y los escribimos en la base de datos. 

Examine el explorador de datos de Cosmos DB tras unos cuantos mensajes enviados a este bot y verá sus datos en registros individuales. Al examinar uno de estos registros, verá que los tres primeros elementos son los tres valores de nuestros datos de registro, que se llaman como la cadena especificada para sus respectivas `JsonProperty`.

# <a name="ctabcs"></a>[C#](#tab/cs)
```cs
    public async Task OnTurn
        (ITurnContext context, MiddlewareSet.NextDelegate next)
    {
        string botReply = "";

        if (context.Activity.Type == ActivityTypes.Message)
        {
            if (context.Activity.Text == "history")
            {
                // Read last 3 responses from the database, and short circuit future execution.
                await context.SendActivity(await ReadFromDatabase(3));
                return;
            }

            // Create a send activity handler to grab all response activities 
            // from the activity list.
            context.OnSendActivity(async (activityContext, activityList, activityNext) =>
            {
                foreach (Activity activity in activityList)
                {
                    botReply += (activity.Text + " ");
                }
                await activityNext();
            });
        }

        // Pass execution on to the next layer in the pipeline.
        await next();

        // Save logs for each conversational exchange only.
        if (context.Activity.Type == ActivityTypes.Message)
        {
            // Build a log object to write to the database.
            var logData = new Log
            {
                Time = DateTime.Now.ToString(),
                Message = context.Activity.Text,
                Reply = botReply
            };

            // Write our log to the database.
            try
            {
                var document = await docClient.CreateDocumentAsync(UriFactory.
                    CreateDocumentCollectionUri(Database, Collection), logData);
            }
            catch (Exception ex)
            {
                // More logic for what to do on a failed write can be added here
                throw ex;
            }
        }
    }
```

# <a name="javascripttabjs"></a>[JavaScript](#tab/js)

Creación desde nuestra función `onTurn` descrita anteriormente.

```javascript
adapter.use({onTurn: async (context, next) =>{

    const utterance = (context.activity.text || '').trim().toLowerCase();
    const isMessage = context.activity.type === 'message';
    const info = conversationState.get(context);

    if(info.log == undefined){
        info.log = [];
    }

    if(isMessage && utterance == 'history'){
        // Loop through info.log 
        var logHistory = "";
        for(var i = 0; i < info.log.length; i++){
            logHistory += info.log[i] + " ";
        }
        await context.sendActivity(logHistory);
        // Short circuit the middleware
        return
    } else if(isMessage){
        // Store the users response
        info.log.push(`Message was: ${context.activity.text}`); 

        await context.onSendActivities(async (handlerContext, activities, handlerNext) => 
        {
            // Store the bot's reply
            info.log.push(`Reply was: ${activities[0].text}`);
            
            await handlerNext(); 
        });
        await next();
    }
}})

```

---

## <a name="sample-output"></a>Salida de ejemplo

En el ejemplo completo, el bot usa la biblioteca de mensajes para pedir el nombre y el número favorito del usuario. Se puede encontrar más información sobre la biblioteca de mensajes en el tema sobre [mensajes al usuario](~/v4sdk/bot-builder-prompts.md).

Esta es una conversación de ejemplo con nuestro bot de ejemplo que ejecuta el código descrito anteriormente.

![Conversación de ejemplo de middleware de Cosmos](./media/cosmos-middleware-conversation.png)

## <a name="additional-resources"></a>Recursos adicionales

* [Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/)
* [Emulador de Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/local-emulator)

