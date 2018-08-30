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
ms.openlocfilehash: 3f36b00a91644859445676676374f7d321087800
ms.sourcegitcommit: 2dc75701b169d822c9499e393439161bc87639d2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42906106"
---
# <a name="create-middleware-that-logs-in-cosmos-db"></a><span data-ttu-id="3c467-104">Creación de middleware que se registre en Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="3c467-104">Create Middleware that logs in Cosmos DB</span></span>

[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]

<span data-ttu-id="3c467-105">Aunque en el SDK se proporciona middleware útil, existen situaciones en las que deberá implementar su propio middleware para conseguir el objetivo deseado.</span><span class="sxs-lookup"><span data-stu-id="3c467-105">While useful middleware is provided with the SDK, there are situations where you will need to implement your own middleware to achieve the desired goal.</span></span>

<span data-ttu-id="3c467-106">En este ejemplo, crearemos una capa de middleware que conecta con Cosmos DB para registrar todos los mensajes recibidos y las respuestas que devolvimos.</span><span class="sxs-lookup"><span data-stu-id="3c467-106">In this sample, we'll create a middleware layer connecting to Cosmos DB to log all the received messages, and the replies we sent back.</span></span> <span data-ttu-id="3c467-107">El código que ve aquí está disponible como código fuente completo, proporcionado con nuestros [ejemplos](../dotnet/bot-builder-dotnet-samples.md).</span><span class="sxs-lookup"><span data-stu-id="3c467-107">The code you'll see here is available as full source code, provided with our [samples](../dotnet/bot-builder-dotnet-samples.md).</span></span>

## <a name="set-up"></a><span data-ttu-id="3c467-108">Instalación</span><span class="sxs-lookup"><span data-stu-id="3c467-108">Set up</span></span>

<span data-ttu-id="3c467-109">Es necesario configurar algunas cosas antes de meternos en el código.</span><span class="sxs-lookup"><span data-stu-id="3c467-109">We need to get a few things set up before getting into the code.</span></span>

### <a name="create-your-database"></a><span data-ttu-id="3c467-110">Creación de la base de datos</span><span class="sxs-lookup"><span data-stu-id="3c467-110">Create your database</span></span>

<span data-ttu-id="3c467-111">En primer lugar, necesitamos un lugar donde almacenar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="3c467-111">First, we need to have somewhere to store our database.</span></span>  <span data-ttu-id="3c467-112">Para ello, podemos usar una cuenta de Azure, o, con fines de prueba, puede usar el [emulador de Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/local-emulator).</span><span class="sxs-lookup"><span data-stu-id="3c467-112">This can be done through an Azure account, or for testing purposes you can use the [Cosmos DB emulator](https://docs.microsoft.com/en-us/azure/cosmos-db/local-emulator).</span></span> <span data-ttu-id="3c467-113">Sea cual sea el método elegido, se necesita un URI de punto de conexión y una clave para la base de datos.</span><span class="sxs-lookup"><span data-stu-id="3c467-113">Whichever method you choose, we need an endpoint URI and key for the database.</span></span>

<span data-ttu-id="3c467-114">En el ejemplo y el código proporcionados aquí se usa el emulador.</span><span class="sxs-lookup"><span data-stu-id="3c467-114">The sample, and the code provided here, is using the emulator.</span></span> <span data-ttu-id="3c467-115">El punto de conexión y la clave son los proporcionados para la cuenta de prueba de Cosmos DB, que son valores comunes que se incluyen con la documentación del emulador y no se pueden usar para producción.</span><span class="sxs-lookup"><span data-stu-id="3c467-115">The endpoint and key are those provided for the Cosmos DB test account, which are common values provided with the emulator documentation and cannot be used for production.</span></span>

<span data-ttu-id="3c467-116">Si quiere usar una instancia de Azure Cosmos DB real, siga el paso 1 del tema [Introducción a Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/sql-api-get-started).</span><span class="sxs-lookup"><span data-stu-id="3c467-116">If you would like to use a real Azure Cosmos DB, follow step 1 of the [Cosmos DB getting started](https://docs.microsoft.com/en-us/azure/cosmos-db/sql-api-get-started) topic.</span></span> <span data-ttu-id="3c467-117">Una vez hecho esto, el URI de punto de conexión y la clave están disponibles en la pestaña **Claves** de la configuración de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="3c467-117">Once that's done, both your endpoint URI and key are available in the **Keys** tab in your database settings.</span></span> <span data-ttu-id="3c467-118">Estos valores serán necesarios a continuación para nuestro archivo de configuración.</span><span class="sxs-lookup"><span data-stu-id="3c467-118">These values will be needed for our configuration file below.</span></span>

### <a name="build-a-basic-bot"></a><span data-ttu-id="3c467-119">Creación de un bot básico</span><span class="sxs-lookup"><span data-stu-id="3c467-119">Build a basic bot</span></span>

<span data-ttu-id="3c467-120">En el resto de este tema se trata la creación de un bot básico, como el HelloBot creado en la introducción.</span><span class="sxs-lookup"><span data-stu-id="3c467-120">The rest of this topic builds off of a basic bot, such as the HelloBot built in the overview.</span></span> <span data-ttu-id="3c467-121">Puede usar [Bot Framework Emulator](https://github.com/Microsoft/BotFramework-Emulator) para conectarse al bot, conversar con él y probarlo.</span><span class="sxs-lookup"><span data-stu-id="3c467-121">You can use the [Bot Framework Emulator](https://github.com/Microsoft/BotFramework-Emulator) to connect to, converse with, and test your bot.</span></span>

<span data-ttu-id="3c467-122">Además de las referencias necesarias para los bots estándar que usan Bot Framework, deberá agregar referencias a los siguientes paquetes NuGet:</span><span class="sxs-lookup"><span data-stu-id="3c467-122">In addition to the references needed for standard bots using the Bot Framework, you'll need to add references to the following NuGet packages:</span></span>

* <span data-ttu-id="3c467-123">Microsoft.Azure.Documentdb.Core</span><span class="sxs-lookup"><span data-stu-id="3c467-123">Microsoft.Azure.Documentdb.Core</span></span>
* <span data-ttu-id="3c467-124">System.Configuration.ConfigurationManager</span><span class="sxs-lookup"><span data-stu-id="3c467-124">System.Configuration.ConfigurationManager</span></span>

<span data-ttu-id="3c467-125">Estas bibliotecas se usan para la comunicación con la base de datos y permiten el uso de un archivo de configuración, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="3c467-125">These libraries are used for communication with the database, and enable use of a configuration file, respectively.</span></span>

### <a name="add-configuration-file"></a><span data-ttu-id="3c467-126">Adición de archivo de configuración</span><span class="sxs-lookup"><span data-stu-id="3c467-126">Add configuration file</span></span>

<span data-ttu-id="3c467-127">El uso de un archivo de configuración es recomendable por varias razones, de modo que aquí usaremos uno.</span><span class="sxs-lookup"><span data-stu-id="3c467-127">Use of a configuration file is good practice for several reasons, so we'll use one here.</span></span> <span data-ttu-id="3c467-128">Nuestros datos de configuración son cortos y sencillos, pero puede agregar otras opciones de configuración a este archivo a medida que el bot se vuelva más complejo.</span><span class="sxs-lookup"><span data-stu-id="3c467-128">Our configuration data is short and simple, however you can add other configuration settings to this as your bot gets more complex.</span></span>

# <a name="ctabcs"></a>[<span data-ttu-id="3c467-129">C#</span><span class="sxs-lookup"><span data-stu-id="3c467-129">C#</span></span>](#tab/cs)
1. <span data-ttu-id="3c467-130">Agregue un archivo al proyecto llamado `app.config`.</span><span class="sxs-lookup"><span data-stu-id="3c467-130">Add a file to your project named `app.config`.</span></span>
2. <span data-ttu-id="3c467-131">Guarde en él la siguiente información.</span><span class="sxs-lookup"><span data-stu-id="3c467-131">Save the following information to it.</span></span> <span data-ttu-id="3c467-132">El punto de conexión y la clave se definen para el emulador local, pero se pueden actualizar para la instancia de Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="3c467-132">The endpoint and key are defined for the local emulator, but can be updated for your Cosmos DB instance.</span></span>
    ```
    <?xml version="1.0" encoding="utf-8"?>
    <configuration>
        <appSettings>
            <add key="EmulatorDbUrl" value="https://localhost:8081"/>
            <add key="EmulatorDbKey" value="C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw=="/>
        </appSettings>
    </configuration>
    ```
# <a name="javascripttabjs"></a>[<span data-ttu-id="3c467-133">JavaScript</span><span class="sxs-lookup"><span data-stu-id="3c467-133">JavaScript</span></span>](#tab/js)
1. <span data-ttu-id="3c467-134">Agregue un archivo al proyecto llamado `config.js`.</span><span class="sxs-lookup"><span data-stu-id="3c467-134">Add a file to your project named `config.js`.</span></span>
2. <span data-ttu-id="3c467-135">Guarde en él la siguiente información.</span><span class="sxs-lookup"><span data-stu-id="3c467-135">Save the following information to it.</span></span> <span data-ttu-id="3c467-136">El punto de conexión y la clave se definen para el emulador local, pero se pueden actualizar para la instancia de Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="3c467-136">The endpoint and key are defined for the local emulator, but can be updated for your Cosmos DB instance.</span></span>
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

<span data-ttu-id="3c467-137">Si usa una instancia real de Cosmos DB, puede reemplazar los valores de las dos claves anteriores por los valores que tenga en la configuración de Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="3c467-137">If you are using an actual Cosmos DB, you can replace the values of the two keys above with the values you have in your Cosmos DB settings.</span></span> <span data-ttu-id="3c467-138">Como alternativa, puede agregar dos claves más a la configuración, lo que le permite cambiar entre el emulador para pruebas y la instancia real de Cosmos DB, como por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="3c467-138">Alternatively, you can add two more keys to your configuration, allowing you to switch between the emulator for testing and your actual Cosmos DB, such as:</span></span>

# <a name="ctabcs"></a>[<span data-ttu-id="3c467-139">C#</span><span class="sxs-lookup"><span data-stu-id="3c467-139">C#</span></span>](#tab/cs)
```
    <add key="ActualDbUrl" value="<your database URI>"/>
    <add key="ActualDbKey" value="<your database key>"/>
```
 <span data-ttu-id="3c467-140">Si agrega dos claves adicionales y quiere usar la base de datos real, no olvide especificar a continuación la clave correcta en el constructor en lugar de `EmulatorDbUrl` y `EmulatorDbKey`.</span><span class="sxs-lookup"><span data-stu-id="3c467-140">If you do add two additional keys and want to use the actual database, be sure to specify the correct key in the constructor below instead of `EmulatorDbUrl` and `EmulatorDbKey`.</span></span>

# <a name="javascripttabjs"></a>[<span data-ttu-id="3c467-141">JavaScript</span><span class="sxs-lookup"><span data-stu-id="3c467-141">JavaScript</span></span>](#tab/js)
```
    config.ActualServiceEndpoint = "your database URI;
    config.ActualAuthKey = "your database key";
```

<span data-ttu-id="3c467-142">Si agrega dos claves adicionales y quiere usar la base de datos real, no olvide especificar a continuación la clave correcta en el constructor en lugar de `EmulatorServiceEndpoint` y `EmulatorAuthKey`.</span><span class="sxs-lookup"><span data-stu-id="3c467-142">If you do add two additional keys and want to use the actual database, be sure to specify the correct key in the constructor below instead of `EmulatorServiceEndpoint` and `EmulatorAuthKey`.</span></span>

---



## <a name="creating-your-middleware"></a><span data-ttu-id="3c467-143">Creación del middleware</span><span class="sxs-lookup"><span data-stu-id="3c467-143">Creating your middleware</span></span>

# <a name="ctabcs"></a>[<span data-ttu-id="3c467-144">C#</span><span class="sxs-lookup"><span data-stu-id="3c467-144">C#</span></span>](#tab/cs)
<span data-ttu-id="3c467-145">Antes de empezar a escribir la lógica real del middleware, cree para él una clase en el proyecto de bot.</span><span class="sxs-lookup"><span data-stu-id="3c467-145">Before we start writing the actual middleware logic, create a new class in your bot project for the middleware.</span></span> <span data-ttu-id="3c467-146">Cuando tenga esa clase, cambie el espacio de nombres por lo que usaremos en el resto de nuestro bot, `Microsoft.Bot.Samples`.</span><span class="sxs-lookup"><span data-stu-id="3c467-146">Once you have that class, change the namespace to what we use for the rest of our bot, `Microsoft.Bot.Samples`.</span></span> <span data-ttu-id="3c467-147">Aquí verá que agregamos referencias a unas cuantas bibliotecas nuevas:</span><span class="sxs-lookup"><span data-stu-id="3c467-147">Here you'll see we added references to a few new libraries:</span></span>

* `Newtonsoft.Json;`
* `Microsoft.Azure.Documents;`
* `Microsoft.Azure.Documents.Client;`
* `Microsoft.Azure.Documents.Linq;`

<span data-ttu-id="3c467-148">Los distintos `Microsoft.Azure.Documents.*` son para las distintas partes de la comunicación con nuestra base de datos y `Newtonsoft.Json` nos ayuda a analizar el código JSON que va y viene a nuestra base de datos.</span><span class="sxs-lookup"><span data-stu-id="3c467-148">The various `Microsoft.Azure.Documents.*` are for different parts of communicating with our database, and `Newtonsoft.Json` helps us parse the JSON going back and forth to our database.</span></span>

<span data-ttu-id="3c467-149">Por último, cada fragmento de middleware implementa `IMiddleware`, que nos exige que definamos los controladores adecuados para que el middleware funcione correctamente.</span><span class="sxs-lookup"><span data-stu-id="3c467-149">Lastly, every piece of middleware implements `IMiddleware` requiring us to define the appropriate handlers for middleware to work properly.</span></span>

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

# <a name="javascripttabjs"></a>[<span data-ttu-id="3c467-150">JavaScript</span><span class="sxs-lookup"><span data-stu-id="3c467-150">JavaScript</span></span>](#tab/js)
<span data-ttu-id="3c467-151">Antes de empezar a escribir la lógica real del middleware, debemos crear nuestro middleware personalizado que se iniciará con `onTurn()`.</span><span class="sxs-lookup"><span data-stu-id="3c467-151">Before we start writing the actual middleware logic, we need to create our custom Middleware that will start with `onTurn()`.</span></span> 

```javascript
adapter.use({onTurn: async (context, next) =>{
    // Middleware logic here...
}})
    
```

---

#### <a name="defining-local-variables"></a><span data-ttu-id="3c467-152">Definición de variables locales</span><span class="sxs-lookup"><span data-stu-id="3c467-152">Defining local variables</span></span>

# <a name="ctabcs"></a>[<span data-ttu-id="3c467-153">C#</span><span class="sxs-lookup"><span data-stu-id="3c467-153">C#</span></span>](#tab/cs)
<span data-ttu-id="3c467-154">A continuación, necesitamos variables locales para manipular nuestra base de datos y una clase para almacenar la información que queremos registrar.</span><span class="sxs-lookup"><span data-stu-id="3c467-154">Next, we need local variables for manipulating our database and a class to store the information we want to log.</span></span> <span data-ttu-id="3c467-155">Esa clase de información, lo que llamamos `Log`, define el nombre que recibirán las propiedades JSON asociadas con cada miembro.</span><span class="sxs-lookup"><span data-stu-id="3c467-155">That information class, which we call `Log`, defines what the JSON properties associated with each member will be named.</span></span> <span data-ttu-id="3c467-156">Volveremos a ello más adelante.</span><span class="sxs-lookup"><span data-stu-id="3c467-156">We'll get back to that further on.</span></span>

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

# <a name="javascripttabjs"></a>[<span data-ttu-id="3c467-157">JavaScript</span><span class="sxs-lookup"><span data-stu-id="3c467-157">JavaScript</span></span>](#tab/js)
<span data-ttu-id="3c467-158">A continuación, necesitamos una variable local para almacenar la información que queremos registrar.</span><span class="sxs-lookup"><span data-stu-id="3c467-158">Next, we need a local variable to store our information that we want to log.</span></span> <span data-ttu-id="3c467-159">En el middleware, debemos acceder a conversationState con Cosmos DB como proveedor de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="3c467-159">In the middleware we must access conversationState which has Cosmos DB as the storage provider.</span></span> <span data-ttu-id="3c467-160">La variable `info` será el objeto del estado que se leerá y en el que se escribirá.</span><span class="sxs-lookup"><span data-stu-id="3c467-160">The variable `info` will be the state's object that we will read and write to.</span></span> 

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


#### <a name="initialize-database-connection"></a><span data-ttu-id="3c467-161">Inicialización de la conexión de base de datos</span><span class="sxs-lookup"><span data-stu-id="3c467-161">Initialize database connection</span></span>

# <a name="ctabcs"></a>[<span data-ttu-id="3c467-162">C#</span><span class="sxs-lookup"><span data-stu-id="3c467-162">C#</span></span>](#tab/cs)
<span data-ttu-id="3c467-163">El constructor de este middleware se encarga de extraer la información necesaria de nuestro archivo de configuración definido anteriormente y de crear el elemento `DocumentClient`, que nos permite leer y escribir en nuestra base de datos.</span><span class="sxs-lookup"><span data-stu-id="3c467-163">Our constructor of this middleware takes care of pulling the necessary information out of our configuration file defined above and creating the `DocumentClient`, which allows us to read and write from our database.</span></span> <span data-ttu-id="3c467-164">También nos aseguramos de que nuestra base de datos y colección estén configurados.</span><span class="sxs-lookup"><span data-stu-id="3c467-164">We also make sure our database and collection are set up.</span></span>

<span data-ttu-id="3c467-165">Para usar un nuevo elemento `DocumentClient` de manera adecuada es necesario deshacerse primero del anterior, y nuestro destructor hace justo eso.</span><span class="sxs-lookup"><span data-stu-id="3c467-165">Proper use of a new `DocumentClient` requires it to be disposed of, so our destructor does just that.</span></span>

<span data-ttu-id="3c467-166">La creación de nuestra base de datos depende de intentar realizar una lectura básica de la primera base de datos y, después, de la colección que contiene.</span><span class="sxs-lookup"><span data-stu-id="3c467-166">Creation of our database depends on attempting to do a basic read of first our database, then the collection within that database.</span></span> <span data-ttu-id="3c467-167">Si recibimos una excepción `NotFound`, la aceptamos y creamos la base de datos o colección en lugar de generar la pila.</span><span class="sxs-lookup"><span data-stu-id="3c467-167">If we hit a `NotFound` exception, we catch that and create the database or collection instead of throwing it up the stack.</span></span> <span data-ttu-id="3c467-168">En la mayoría de los casos ya se habrán creado, pero para los fines de este ejemplo esto nos permite olvidarnos de la creación de esa base de datos antes de la primera ejecución.</span><span class="sxs-lookup"><span data-stu-id="3c467-168">In most cases these will already be created, but for the sake of this sample this allows us to not worry about the creation of that database before first run.</span></span> <span data-ttu-id="3c467-169">En el código de ejemplo, la base de datos y la colección se dividen en dos funciones por motivos de simplicidad, pero se han combinado en el siguiente fragmento de código.</span><span class="sxs-lookup"><span data-stu-id="3c467-169">In the sample code, the database and collection are split into two functions for simplicity, but they have been combined in the snippet below.</span></span>

<span data-ttu-id="3c467-170">Para más información sobre COSMOS DB, eche un vistazo a su [sitio web](https://docs.microsoft.com/en-us/azure/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="3c467-170">For more on CosmosDB, take a look at their [website](https://docs.microsoft.com/en-us/azure/cosmos-db/).</span></span> <span data-ttu-id="3c467-171">En el resto de este tema, analizaremos los detalles de Cosmos DB propiamente dichos y nos centraremos en lo que necesita el bot para usarlo.</span><span class="sxs-lookup"><span data-stu-id="3c467-171">For the remainder of this topic, we'll breeze over details on Cosmos DB itself, and focus on what's required for your bot to use it.</span></span>

<span data-ttu-id="3c467-172">La definición de `Key`, `Endpoint` y `docClient` se incluyó en un fragmento de código anterior y solo se ha copiado aquí para una mayor claridad.</span><span class="sxs-lookup"><span data-stu-id="3c467-172">The definition of `Key`, `Endpoint`, and `docClient` were included in a snippet above, and have only been copied here for clarity.</span></span>

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

# <a name="javascripttabjs"></a>[<span data-ttu-id="3c467-173">JavaScript</span><span class="sxs-lookup"><span data-stu-id="3c467-173">JavaScript</span></span>](#tab/js)
<span data-ttu-id="3c467-174">La creación de nuestra base de datos depende de intentar realizar una lectura básica de la primera base de datos y, después, de la colección que contiene.</span><span class="sxs-lookup"><span data-stu-id="3c467-174">Creation of our database depends on attempting to do a basic read of first our database, then the collection within that database.</span></span> <span data-ttu-id="3c467-175">Si recibimos `undefined`, lo aceptamos y creamos un objeto llamado `log` que se establecerá en una matriz vacía.</span><span class="sxs-lookup"><span data-stu-id="3c467-175">If we hit `undefined`, we catch that and create an object called `log` that will be set to an empty array.</span></span> <span data-ttu-id="3c467-176">En la mayoría de los casos `log` ya se habrá creado, pero para los fines de este ejemplo esto nos permite olvidarnos de la creación de esa base de datos antes de la primera ejecución.</span><span class="sxs-lookup"><span data-stu-id="3c467-176">In most cases `log` will already be created, but for the sake of this sample this allows us to not worry about the creation of that database before first run.</span></span> <span data-ttu-id="3c467-177">En el ejemplo de código, comprobamos si `info.log` es indefinido.</span><span class="sxs-lookup"><span data-stu-id="3c467-177">In the sample code, we check to see if `info.log` is undefined.</span></span> <span data-ttu-id="3c467-178">Si es así, establézcalo en una matriz vacía.</span><span class="sxs-lookup"><span data-stu-id="3c467-178">If so set it to an empty array.</span></span>

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

#### <a name="read-from-database-logic"></a><span data-ttu-id="3c467-179">Lectura de la lógica de la base de datos</span><span class="sxs-lookup"><span data-stu-id="3c467-179">Read from database logic</span></span>

<span data-ttu-id="3c467-180">La última función auxiliar lee nuestra base de datos y devuelve el número de registros especificado más reciente.</span><span class="sxs-lookup"><span data-stu-id="3c467-180">The last helper function reads from our database and returns the most recent specified number of records.</span></span> <span data-ttu-id="3c467-181">Merece la pena mencionar que existen procedimientos de base de datos para recuperar los datos mejores que los que usamos aquí, especialmente cuando el almacén de datos es bastante más grande.</span><span class="sxs-lookup"><span data-stu-id="3c467-181">It's worth noting that there are better database practices for retrieving data than we use here, particularly when your data store is significantly larger.</span></span>

# <a name="ctabcs"></a>[<span data-ttu-id="3c467-182">C#</span><span class="sxs-lookup"><span data-stu-id="3c467-182">C#</span></span>](#tab/cs)
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

# <a name="javascripttabjs"></a>[<span data-ttu-id="3c467-183">JavaScript</span><span class="sxs-lookup"><span data-stu-id="3c467-183">JavaScript</span></span>](#tab/js)

<span data-ttu-id="3c467-184">El código siguiente está todavía dentro de la función `onTurn()` y se invocará si el usuario inserta la cadena "historial".</span><span class="sxs-lookup"><span data-stu-id="3c467-184">The code below is still within the `onTurn()` function and will be called if the user inputs the string "history".</span></span>

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

#### <a name="define-onturn"></a><span data-ttu-id="3c467-185">Definición de OnTurn()</span><span class="sxs-lookup"><span data-stu-id="3c467-185">Define OnTurn()</span></span>

<span data-ttu-id="3c467-186">El método `OnTurn()` de middleware estándar controla el resto del trabajo.</span><span class="sxs-lookup"><span data-stu-id="3c467-186">The standard middleware method `OnTurn()` then handles the rest of the work.</span></span> <span data-ttu-id="3c467-187">Solo queremos realizar el registro cuando la actividad actual es un mensaje, lo que comprobamos antes y después de llamar a `next()`.</span><span class="sxs-lookup"><span data-stu-id="3c467-187">We only want to log when the current activity is a message, which we check for both before and after calling `next()`.</span></span>

<span data-ttu-id="3c467-188">Lo primero que comprobamos es si este es nuestro mensaje de caso especial, donde el usuario pregunta por el historial más reciente.</span><span class="sxs-lookup"><span data-stu-id="3c467-188">The first thing we check is if this is our special case message, where the user is asking for the most recent history.</span></span> <span data-ttu-id="3c467-189">Si es así, invocamos la lectura de nuestra base de datos para obtener los registros más recientes y los enviamos a la conversación.</span><span class="sxs-lookup"><span data-stu-id="3c467-189">If so, we call read from our database to get the most recent records, and send that to the conversation.</span></span> <span data-ttu-id="3c467-190">Como la actividad actual se controla completamente, bloqueamos la canalización en lugar de pasar la ejecución.</span><span class="sxs-lookup"><span data-stu-id="3c467-190">Since that completely handles the current activity, we short circuit the pipeline instead of passing on execution.</span></span>

<span data-ttu-id="3c467-191">Para obtener la respuesta que envía el bot, creamos un controlador cada vez que recibimos un mensaje para captar esas respuestas, que se puede ver en la expresión lambda que proporcionamos a `OnSendActivity()`.</span><span class="sxs-lookup"><span data-stu-id="3c467-191">To get the response that bot sends, we create a handler every time we recieve a message to grab those responses, which can be seen in the lambda we give to `OnSendActivity()`.</span></span> <span data-ttu-id="3c467-192">Se genera una cadena para recopilar todos los mensajes enviados a través de `SendActivity()` para este objeto de contexto.</span><span class="sxs-lookup"><span data-stu-id="3c467-192">It builds a string to collect all the messages sent through `SendActivity()` for this context object.</span></span>

<span data-ttu-id="3c467-193">Una vez que la ejecución devuelve la canalización de `next()`, montamos nuestros datos de registro y los escribimos en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="3c467-193">Once execution returns up the pipeline from `next()`, we assemble our log data and write it out to the database.</span></span> 

<span data-ttu-id="3c467-194">Examine el explorador de datos de Cosmos DB tras unos cuantos mensajes enviados a este bot y verá sus datos en registros individuales.</span><span class="sxs-lookup"><span data-stu-id="3c467-194">Look in the Cosmos DB data explorer after a few messages are sent to this bot, and you should see your data in individual records.</span></span> <span data-ttu-id="3c467-195">Al examinar uno de estos registros, verá que los tres primeros elementos son los tres valores de nuestros datos de registro, que se llaman como la cadena especificada para sus respectivas `JsonProperty`.</span><span class="sxs-lookup"><span data-stu-id="3c467-195">When examining one of these records, you should see that the first three items are the three values of our log data, named as the string we specified for their respective `JsonProperty`.</span></span>

# <a name="ctabcs"></a>[<span data-ttu-id="3c467-196">C#</span><span class="sxs-lookup"><span data-stu-id="3c467-196">C#</span></span>](#tab/cs)
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

# <a name="javascripttabjs"></a>[<span data-ttu-id="3c467-197">JavaScript</span><span class="sxs-lookup"><span data-stu-id="3c467-197">JavaScript</span></span>](#tab/js)

<span data-ttu-id="3c467-198">Creación desde nuestra función `onTurn` descrita anteriormente.</span><span class="sxs-lookup"><span data-stu-id="3c467-198">Building off from our `onTurn` function described above.</span></span>

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

## <a name="sample-output"></a><span data-ttu-id="3c467-199">Salida de ejemplo</span><span class="sxs-lookup"><span data-stu-id="3c467-199">Sample output</span></span>

<span data-ttu-id="3c467-200">En el ejemplo completo, el bot usa la biblioteca de mensajes para pedir el nombre y el número favorito del usuario.</span><span class="sxs-lookup"><span data-stu-id="3c467-200">In the full sample, the bot uses the prompt library to ask for the user's name and favorite number.</span></span> <span data-ttu-id="3c467-201">Se puede encontrar más información sobre la biblioteca de mensajes en el tema sobre [mensajes al usuario](~/v4sdk/bot-builder-prompts.md).</span><span class="sxs-lookup"><span data-stu-id="3c467-201">Details on the prompt library can be found on the [prompting users](~/v4sdk/bot-builder-prompts.md) topic.</span></span>

<span data-ttu-id="3c467-202">Esta es una conversación de ejemplo con nuestro bot de ejemplo que ejecuta el código descrito anteriormente.</span><span class="sxs-lookup"><span data-stu-id="3c467-202">Here is an example conversation with our sample bot that is running the code outlined above.</span></span>

![Conversación de ejemplo de middleware de Cosmos](./media/cosmos-middleware-conversation.png)

## <a name="additional-resources"></a><span data-ttu-id="3c467-204">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="3c467-204">Additional resources</span></span>

* [<span data-ttu-id="3c467-205">Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="3c467-205">Cosmos DB</span></span>](https://docs.microsoft.com/en-us/azure/cosmos-db/)
* [<span data-ttu-id="3c467-206">Emulador de Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="3c467-206">Cosmos DB emulator</span></span>](https://docs.microsoft.com/en-us/azure/cosmos-db/local-emulator)

