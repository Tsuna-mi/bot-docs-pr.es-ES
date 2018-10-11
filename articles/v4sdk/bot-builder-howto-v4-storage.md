---
title: Escritura directa en el almacenamiento | Microsoft Docs
description: Obtenga información sobre cómo escribir directamente en el almacenamiento con el SDK Bot Builder para .NET.
keywords: almacenamiento, lectura y escritura, almacenamiento en memoria, eTag
author: DeniseMak
ms.author: v-demak
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 09/14/18
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: 0bbb507484840ab1fb0dc7c209b5d7ca8e9c9cfb
ms.sourcegitcommit: 3bf3dbb1a440b3d83e58499c6a2ac116fe04b2f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/23/2018
ms.locfileid: "46708151"
---
# <a name="write-directly-to-storage"></a><span data-ttu-id="34b32-104">Escritura directa en el almacenamiento</span><span class="sxs-lookup"><span data-stu-id="34b32-104">Write directly to storage</span></span>

[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]

<span data-ttu-id="34b32-105">Puede leer y escribir directamente en el objeto de almacenamiento sin usar middleware ni un objeto de contexto.</span><span class="sxs-lookup"><span data-stu-id="34b32-105">You can read and write directly to your storage object without using middleware or context object.</span></span> <span data-ttu-id="34b32-106">Esto puede ser adecuado para los datos que usa el bot, que provienen de un origen externo al flujo de conversación del bot.</span><span class="sxs-lookup"><span data-stu-id="34b32-106">This can be appropriate to data that your bot uses, that comes from a source outside your bot's conversation flow.</span></span> <span data-ttu-id="34b32-107">Por ejemplo, suponga que el bot permite al usuario solicitar el informe meteorológico y el bot lo recupera para una fecha concreta, leyéndolo de una base de datos externa.</span><span class="sxs-lookup"><span data-stu-id="34b32-107">For example, let's say your bot allows the user to ask for the weather report, and your bot retrieves the weather report for a specified date, by reading it from an external database.</span></span> <span data-ttu-id="34b32-108">El contenido de la base de datos meteorológicos no depende de la información del usuario ni del contexto de la conversación, por lo que se podría leer directamente desde el almacenamiento en lugar de usar el administrador de estado.</span><span class="sxs-lookup"><span data-stu-id="34b32-108">The content of the weather database isn't dependent on user information or the conversation context, so you could just read it directly from storage instead of using the state manager.</span></span> <span data-ttu-id="34b32-109">En los ejemplos de código de este artículo, se muestra cómo leer y escribir datos en el almacenamiento con **almacenamiento en memoria**, **Cosmos DB**, **Blob Storage** y el **almacén de transcripciones de blobs de Azure**.</span><span class="sxs-lookup"><span data-stu-id="34b32-109">The code examples in this article show you how to read and write data to storage using **Memory Storage**, **Cosmos DB**, **Blob Storage**, and **Azure Blob Transcript Store**.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="34b32-110">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="34b32-110">Prerequisites</span></span>
- <span data-ttu-id="34b32-111">Si no tiene una suscripción a Azure, cree una cuenta [gratuita](https://azure.microsoft.com/en-us/free/) antes de empezar.</span><span class="sxs-lookup"><span data-stu-id="34b32-111">If you don't have an Azure subscription, create a [free](https://azure.microsoft.com/en-us/free/) account before you begin.</span></span>
- <span data-ttu-id="34b32-112">Instale Bot Framework [Emulator](https://github.com/Microsoft/BotFramework-Emulator/releases)</span><span class="sxs-lookup"><span data-stu-id="34b32-112">Install Bot Framework [Emulator](https://github.com/Microsoft/BotFramework-Emulator/releases)</span></span>

## <a name="memory-storage"></a><span data-ttu-id="34b32-113">Almacenamiento en memoria</span><span class="sxs-lookup"><span data-stu-id="34b32-113">Memory storage</span></span>

<span data-ttu-id="34b32-114">Primero crearemos un bot que lea y escriba datos en el almacenamiento en memoria.</span><span class="sxs-lookup"><span data-stu-id="34b32-114">We will first create a bot that will read and write data to Memory Storage.</span></span> <span data-ttu-id="34b32-115">El almacenamiento en memoria se usa solo con fines de prueba y no está pensado para su uso en producción.</span><span class="sxs-lookup"><span data-stu-id="34b32-115">Memory storage is used for testing purposes only and is not intended for production use.</span></span> <span data-ttu-id="34b32-116">Asegúrese de establecer el almacenamiento en Cosmos DB o Blob Storage antes de publicar el bot.</span><span class="sxs-lookup"><span data-stu-id="34b32-116">Be sure to set storage to Cosmos DB or Blob Storage before publishing your bot.</span></span>

#### <a name="build-a-basic-bot"></a><span data-ttu-id="34b32-117">Creación de un bot básico</span><span class="sxs-lookup"><span data-stu-id="34b32-117">Build a basic bot</span></span>

<span data-ttu-id="34b32-118">El resto de este tema se basa en un bot de eco.</span><span class="sxs-lookup"><span data-stu-id="34b32-118">The rest of this topic builds off of a Echo bot.</span></span> <span data-ttu-id="34b32-119">Puede crear uno en [C#](../dotnet/bot-builder-dotnet-sdk-quickstart.md) o [JS](../javascript/bot-builder-javascript-quickstart.md).</span><span class="sxs-lookup"><span data-stu-id="34b32-119">You can create one in either [C#](../dotnet/bot-builder-dotnet-sdk-quickstart.md) or [JS](../javascript/bot-builder-javascript-quickstart.md).</span></span> <span data-ttu-id="34b32-120">Puede usar Bot Framework Emulator para conectarse al bot, conversar con él y probarlo.</span><span class="sxs-lookup"><span data-stu-id="34b32-120">You can use the Bot Framework Emulator to connect to, converse with, and test your bot.</span></span> <span data-ttu-id="34b32-121">El ejemplo siguiente agrega todos los mensajes del usuario a una lista.</span><span class="sxs-lookup"><span data-stu-id="34b32-121">The following sample adds every message from the the user to a list.</span></span> <span data-ttu-id="34b32-122">La estructura de datos que contiene la lista se guarda en el almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="34b32-122">The data structure containing the list is saved to your storage.</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="34b32-123">C#</span><span class="sxs-lookup"><span data-stu-id="34b32-123">C#</span></span>](#tab/csharp)

```csharp
using System;
using System.Threading.Tasks;
using Microsoft.Bot.Builder;
using Microsoft.Bot.Builder.TraceExtensions;
using Microsoft.Bot.Builder.Azure;
using Microsoft.Bot.Schema;
using System.Collections.Generic;
using System.Linq;
using System.Threading;

// Create local Memory Storage.
private static readonly MemoryStorage _myStorage = new MemoryStorage();

// Create cancellation token (used by Async Write operation).
public CancellationToken cancellationToken { get; private set; }

// Class for storing a log of utterances (text of messages) as a list.
public class UtteranceLog : IStoreItem
{
     // A list of things that users have said to the bot
     public List<string> UtteranceList { get; } = new List<string>();

     // The number of conversational turns that have occurred        
     public int TurnNumber { get; set; } = 0;

     // Create concurrency control where this is used.
     public string ETag { get; set; } = "*";
}

// Every Conversation turn for our Bot calls this method.
public async Task OnTurnAsync(ITurnContext context)
{
     
     var activityType = context.Activity.Type;
     // See if activity type for this turn is a message from the user.
     if (activityType == ActivityTypes.Message)
     {
         var utterance = context.Activity.Text;
         UtteranceLog logItems = null;
          
         // see if there are previous messages saved in sstorage.
         string[] utteranceList = { "UtteranceLog" };
         logItems = _myStorage.ReadAsync<UtteranceLog>(utteranceList).Result?.FirstOrDefault().Value;

         // If no stored messages were found, create and store a new entry.
         if (logItems is null)
         {
             logItems = new UtteranceLog();
         }
         
         // add new message to list of messages to display.
         logItems.UtteranceList.Add(utterance);
         // increment turn counter.
         logItems.TurnNumber++;
         
         // show user new list of saved messages.
         await context.SendActivityAsync($"The list is now: {string.Join(", ", logItems.UtteranceList)}");
         
         // Create Dictionary object to hold new list of messages.
         {
             changes.Add("UtteranceLog", logItems);
          };
          
          // Save new list to your Storage.
          await _myStorage.WriteAsync(changes,cancellationToken);
     }
     return;
}

```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="34b32-124">JavaScript</span><span class="sxs-lookup"><span data-stu-id="34b32-124">JavaScript</span></span>](#tab/javascript)

```javascript
const { BotFrameworkAdapter, ConversationState, BotStateSet, MemoryStorage } = require('botbuilder');
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

// Add memory storage.
var storage = new MemoryStorage();

const conversationState = new ConversationState(storage);
adapter.use(conversationState);

// Listen for incoming activity .
server.post('/api/messages', (req, res) => {
    // Route received activity to adapter for processing.
    adapter.processActivity(req, res, async (context) => {
        if (context.activity.type === 'message') {
            const state = conversationState.get(context);
            const count = state.count === undefined ? state.count = 0 : ++state.count;

            await logMessageText(storage, context);

            await context.sendActivity(`${count}: You said "${context.activity.text}"`);
        } else {
            await context.sendActivity(`[${context.activity.type} event detected]`);
        }
    });
});

async function logMessageText(storage, context) {
    let utterance = context.activity.text;
    try {
        // Read from the storage.
        let storeItems = await storage.read(["UtteranceLog"])
        // Check the result.
        var utteranceLog = storeItems["UtteranceLog"];

        if (typeof (utteranceLog) != 'undefined') {
            // The log exists so we can write to it.
            storeItems["UtteranceLog"].UtteranceList.push(utterance);

            try {
                await storage.write(storeItems)
                context.sendActivity('Successful write to utterance log.');
            } catch (err) {
                context.sendActivity(`Write failed of UtteranceLog: ${err}`);
            }

         } else {
            context.sendActivity(`need to create new utterance log`);
            storeItems["UtteranceLog"] = { UtteranceList: [`${utterance}`], "eTag": "*" }

            try {
                await storage.write(storeItems)
                context.sendActivity('Successful write to log.');
            } catch (err) {
                context.sendActivity(`Write failed: ${err}`);
            }
        }
    } catch (err) {
        context.sendActivity(`Read rejected. ${err}`);
    };
}
```

---

### <a name="start-your-bot"></a><span data-ttu-id="34b32-125">Inicio del bot</span><span class="sxs-lookup"><span data-stu-id="34b32-125">Start your bot</span></span>
<span data-ttu-id="34b32-126">Ejecute el bot localmente.</span><span class="sxs-lookup"><span data-stu-id="34b32-126">Run your bot locally.</span></span>

### <a name="start-the-emulator-and-connect-your-bot"></a><span data-ttu-id="34b32-127">Inicio del emulador y conexión del bot</span><span class="sxs-lookup"><span data-stu-id="34b32-127">Start the emulator and connect your bot</span></span>
<span data-ttu-id="34b32-128">A continuación, inicie el emulador y, después, conéctese al bot en el emulador:</span><span class="sxs-lookup"><span data-stu-id="34b32-128">Next, start the emulator and then connect to your bot in the emulator:</span></span>

1. <span data-ttu-id="34b32-129">Haga clic en el vínculo **Open Bot** (Abrir bot) de la pestaña de bienvenida del emulador.</span><span class="sxs-lookup"><span data-stu-id="34b32-129">Click the **Open Bot** link in the emulator "Welcome" tab.</span></span> 
2. <span data-ttu-id="34b32-130">Seleccione el archivo .bot ubicado en el directorio donde se creó el proyecto.</span><span class="sxs-lookup"><span data-stu-id="34b32-130">Select the .bot file located in the directory where you created the project.</span></span>

### <a name="interact-with-your-bot"></a><span data-ttu-id="34b32-131">Interacción con el bot</span><span class="sxs-lookup"><span data-stu-id="34b32-131">Interact with your bot</span></span>
<span data-ttu-id="34b32-132">Envíe un mensaje al bot y este enumerará los mensajes que ha recibido.</span><span class="sxs-lookup"><span data-stu-id="34b32-132">Send a message to your bot, and the bot will list the messages it received.</span></span>
<span data-ttu-id="34b32-133">![Emulador en ejecución](../media/emulator-v4/emulator-running.png)</span><span class="sxs-lookup"><span data-stu-id="34b32-133">![Emulator running](../media/emulator-v4/emulator-running.png)</span></span>

 
## <a name="using-cosmos-db"></a><span data-ttu-id="34b32-134">Uso de Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="34b32-134">Using Cosmos DB</span></span>
<span data-ttu-id="34b32-135">Ahora que ha usado el almacenamiento en memoria, actualizaremos el código para usar Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="34b32-135">Now that you've used memory storage, we'll update the code to use Azure Cosmos DB.</span></span> <span data-ttu-id="34b32-136">Cosmos DB es la base de datos multimodelo de distribución global de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="34b32-136">Cosmos DB is Microsoft's globally distributed, multi-model database.</span></span> <span data-ttu-id="34b32-137">Azure Cosmos DB permite escalar de forma elástica e individual el rendimiento y el almacenamiento en cualquiera de las regiones geográficas de Azure.</span><span class="sxs-lookup"><span data-stu-id="34b32-137">Azure Cosmos DB enables you to elastically and independently scale throughput and storage across any number of Azure's geographic regions.</span></span> <span data-ttu-id="34b32-138">Ofrece garantía de rendimiento, latencia, disponibilidad y coherencia con Acuerdos de Nivel de Servicio (SLA) integrales.</span><span class="sxs-lookup"><span data-stu-id="34b32-138">It offers throughput, latency, availability, and consistency guarantees with comprehensive service level agreements (SLAs).</span></span> 

### <a name="set-up"></a><span data-ttu-id="34b32-139">Instalación</span><span class="sxs-lookup"><span data-stu-id="34b32-139">Set up</span></span>
<span data-ttu-id="34b32-140">Para utilizar Cosmos DB en el bot, es necesario configurar algunas cosas antes de meternos en el código.</span><span class="sxs-lookup"><span data-stu-id="34b32-140">To use Cosmos DB in your bot, you'll need to get a few things set up before getting into the code.</span></span>

#### <a name="create-your-database-account"></a><span data-ttu-id="34b32-141">Creación de la cuenta de base de datos</span><span class="sxs-lookup"><span data-stu-id="34b32-141">Create your database account</span></span>
1. <span data-ttu-id="34b32-142">En una nueva ventana del explorador, inicie sesión en [Azure Portal](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="34b32-142">In a new browser window, sign in to the [Azure portal](http://portal.azure.com).</span></span>
2. <span data-ttu-id="34b32-143">Haga clic en **Crear un recurso > Bases de datos > Azure Cosmos DB**</span><span class="sxs-lookup"><span data-stu-id="34b32-143">Click **Create a resource > Databases > Azure Cosmos DB**</span></span>
3. <span data-ttu-id="34b32-144">En la página **Nueva cuenta**, proporcione un nombre único en el campo **ID**.</span><span class="sxs-lookup"><span data-stu-id="34b32-144">In the **New account page**, provide a unique name in the **ID** field.</span></span> <span data-ttu-id="34b32-145">En **API**, seleccione **SQL** y proporcione la información de **Suscripción**, **Ubicación** y **Grupo de recursos**.</span><span class="sxs-lookup"><span data-stu-id="34b32-145">For **API**, select **SQL**, and provide **Subscription**, **Location**, and **Resource group** information.</span></span>
4. <span data-ttu-id="34b32-146">A continuación, haga clic en **Crear**.</span><span class="sxs-lookup"><span data-stu-id="34b32-146">Then click **Create**.</span></span>

<span data-ttu-id="34b32-147">La cuenta tarda unos minutos en crearse.</span><span class="sxs-lookup"><span data-stu-id="34b32-147">The account creation takes a few minutes.</span></span> <span data-ttu-id="34b32-148">Espere a que el portal muestre la página "Enhorabuena.</span><span class="sxs-lookup"><span data-stu-id="34b32-148">Wait for the portal to display the Congratulations!</span></span> <span data-ttu-id="34b32-149">Se ha creado su cuenta de Azure Cosmos DB".</span><span class="sxs-lookup"><span data-stu-id="34b32-149">Your Azure Cosmos DB account was created page.</span></span>

##### <a name="add-a-collection"></a><span data-ttu-id="34b32-150">Agregar una colección</span><span class="sxs-lookup"><span data-stu-id="34b32-150">Add a collection</span></span>
1. <span data-ttu-id="34b32-151">Haga clic en **Configuración > Nueva colección**.</span><span class="sxs-lookup"><span data-stu-id="34b32-151">Click **Settings > New Collection**.</span></span> <span data-ttu-id="34b32-152">El área **Agregar colección** se muestra en el extremo derecho, pero es posible que haya que desplazarse hacia la derecha para verlo.</span><span class="sxs-lookup"><span data-stu-id="34b32-152">The **Add Collection** area is displayed on the far right, you may need to scroll right to see it.</span></span>

![Adición de una colección de Cosmos DB](./media/add_database_collection.png)

2. <span data-ttu-id="34b32-154">La nueva colección de base de datos, "bot-cosmos-sql-db" con el identificador de colección "bot-storage".</span><span class="sxs-lookup"><span data-stu-id="34b32-154">Your new database collection, "bot-cosmos-sql-db" with a collection id of "bot-storage."</span></span> <span data-ttu-id="34b32-155">Usaremos estos valores en el ejemplo de código siguiente.</span><span class="sxs-lookup"><span data-stu-id="34b32-155">We will use these values in our coding example that follows below.</span></span>

![Cosmos DB](./media/cosmos-db-sql-database.png)

3. <span data-ttu-id="34b32-157">El identificador URI del punto de conexión y la clave están disponibles en la pestaña **Claves** de la configuración de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="34b32-157">The endpoint URI and key are available within the **Keys** tab of your database settings.</span></span> <span data-ttu-id="34b32-158">Necesitará estos valores para configurar el código más adelante en este artículo.</span><span class="sxs-lookup"><span data-stu-id="34b32-158">These values will be needed to configure your code further down in this article.</span></span> 

![Claves de Cosmos DB](./media/comos-db-keys.png)

#### <a name="add-configuration-information"></a><span data-ttu-id="34b32-160">Adición de la información de configuración</span><span class="sxs-lookup"><span data-stu-id="34b32-160">Add configuration information</span></span>
<span data-ttu-id="34b32-161">Los datos de configuración para agregar el almacenamiento de Cosmos DB son cortos y sencillos; puede agregar una configuración adicional con estos mismos métodos a medida que el bot se vuelva más complejo.</span><span class="sxs-lookup"><span data-stu-id="34b32-161">Our configuration data to add Cosmos DB storage is short and simple, you can add additional configuration settings using these same methods as your bot gets more complex.</span></span> <span data-ttu-id="34b32-162">En este ejemplo se usan los nombres de la base de datos y de la colección de Cosmos DB del ejemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="34b32-162">This example uses the Cosmos DB database and collection names from the example above.</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="34b32-163">C#</span><span class="sxs-lookup"><span data-stu-id="34b32-163">C#</span></span>](#tab/csharp)

```csharp
private const string CosmosServiceEndpoint = "<your-cosmos-db-URI>";
private const string CosmosDBKey = "<your-cosmos-db-account-key>";
private const string CosmosDBDatabaseName = "bot-cosmos-sql-db";
private const string CosmosDBCollectionName = "bot-storage";
```


# <a name="javascripttabjavascript"></a>[<span data-ttu-id="34b32-164">JavaScript</span><span class="sxs-lookup"><span data-stu-id="34b32-164">JavaScript</span></span>](#tab/javascript)

<span data-ttu-id="34b32-165">Agregue la siguiente información al archivo `.env`.</span><span class="sxs-lookup"><span data-stu-id="34b32-165">Add the following information to your `.env` file.</span></span> 
 
```javascript
ACTUAL_SERVICE_ENDPOINT=<your database URI>
ACTUAL_AUTH_KEY=<your database key>
DATABASE=Tasks
COLLECTION=Items
```
---

#### <a name="installing-packages"></a><span data-ttu-id="34b32-166">Instalación de paquetes</span><span class="sxs-lookup"><span data-stu-id="34b32-166">Installing packages</span></span>
<span data-ttu-id="34b32-167">Asegúrese de que tiene los paquetes necesarios para Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="34b32-167">Make sure you have the packages necessary for Cosmos DB</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="34b32-168">C#</span><span class="sxs-lookup"><span data-stu-id="34b32-168">C#</span></span>](#tab/csharp)

```powershell
Install-Package Microsoft.Bot.Builder.Azure
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="34b32-169">JavaScript</span><span class="sxs-lookup"><span data-stu-id="34b32-169">JavaScript</span></span>](#tab/javascript)

<span data-ttu-id="34b32-170">Puede agregar referencias a botbuilder-azure en el proyecto mediante npm: --></span><span class="sxs-lookup"><span data-stu-id="34b32-170">You can add references to botbuilder-azure in your project via npm: --></span></span>


```powershell
npm install --save botbuilder-azure 
```

<span data-ttu-id="34b32-171">El bot necesita incluir un paquete adicional para usar el archivo de configuración .env.</span><span class="sxs-lookup"><span data-stu-id="34b32-171">To use the .env configuration file, the bot needs an extra package included.</span></span> <span data-ttu-id="34b32-172">En primer lugar, obtenga el paquete de dotnet de npm:</span><span class="sxs-lookup"><span data-stu-id="34b32-172">First, get the dotnet package from npm:</span></span>

```powershell
npm install --save dotenv
```

---

### <a name="implementation"></a><span data-ttu-id="34b32-173">Implementación</span><span class="sxs-lookup"><span data-stu-id="34b32-173">Implementation</span></span> 

# <a name="ctabcsharp"></a>[<span data-ttu-id="34b32-174">C#</span><span class="sxs-lookup"><span data-stu-id="34b32-174">C#</span></span>](#tab/csharp)

<span data-ttu-id="34b32-175">El código de ejemplo siguiente se ejecuta con el mismo código de bot que el ejemplo de [almacenamiento en memoria](#memory-storage) anterior.</span><span class="sxs-lookup"><span data-stu-id="34b32-175">The following sample code runs using the same bot code as the [memory storage](#memory-storage) sample provided above.</span></span>
<span data-ttu-id="34b32-176">El siguiente fragmento de código muestra una implementación de almacenamiento de Cosmos DB para "_myStorage_", que sustituye al almacenamiento en memoria local.</span><span class="sxs-lookup"><span data-stu-id="34b32-176">The code snippet below shows an implementation of Cosmos DB storage for '_myStorage_' that replaces local Memory storage.</span></span> 

```csharp
using Microsoft.Bot.Builder.Azure;

// Create access to Cosmos DB storage.
// Replaces Memory Storage with reference to Cosmos DB.
private static readonly CosmosDbStorage _myStorage = new CosmosDbStorage(new CosmosDbStorageOptions
{
   AuthKey = CosmosDBKey,
   CollectionId = CosmosDBCollectionName,
   CosmosDBEndpoint = new Uri(CosmosServiceEndpoint),
   DatabaseId = CosmosDBDatabaseName,
});
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="34b32-177">JavaScript</span><span class="sxs-lookup"><span data-stu-id="34b32-177">JavaScript</span></span>](#tab/javascript)

<span data-ttu-id="34b32-178">El siguiente código de ejemplo es similar al de [almacenamiento en memoria](#memory-storage), pero con algunos pequeños cambios.</span><span class="sxs-lookup"><span data-stu-id="34b32-178">The following sample code is similar to [memory storage](#memory-storage) but with some slight changes.</span></span>

<span data-ttu-id="34b32-179">Se requiere `CosmosDbStorage` de botbuilder-azure y configurar dotenv para leer el archivo `.env`.</span><span class="sxs-lookup"><span data-stu-id="34b32-179">Require `CosmosDbStorage` from botbuilder-azure and configure dotenv to read the `.env` file.</span></span>

<span data-ttu-id="34b32-180">**app.js**</span><span class="sxs-lookup"><span data-stu-id="34b32-180">**app.js**</span></span>
```javascript
const { CosmosDbStorage } = require("botbuilder-azure");
require('dotenv').config()
```

<span data-ttu-id="34b32-181">Reemplace el almacenamiento en memoria por la referencia a Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="34b32-181">Replace Memory Storage with reference to Cosmos DB.</span></span>

```javascript
//Add CosmosDB 
const storage = new CosmosDbStorage({
    serviceEndpoint: process.env.ACTUAL_SERVICE_ENDPOINT, 
    authKey: process.env.ACTUAL_AUTH_KEY, 
    databaseId: process.env.DATABASE,
    collectionId: process.env.COLLECTION
})

const conversationState = new ConversationState(storage);
adapter.use(conversationState);
```

---

## <a name="start-your-bot"></a><span data-ttu-id="34b32-182">Inicio del bot</span><span class="sxs-lookup"><span data-stu-id="34b32-182">Start your bot</span></span>
<span data-ttu-id="34b32-183">Ejecute el bot localmente.</span><span class="sxs-lookup"><span data-stu-id="34b32-183">Run your bot locally.</span></span>

## <a name="start-the-emulator-and-connect-your-bot"></a><span data-ttu-id="34b32-184">Inicio del emulador y conexión del bot</span><span class="sxs-lookup"><span data-stu-id="34b32-184">Start the emulator and connect your bot</span></span>
<span data-ttu-id="34b32-185">A continuación, inicie el emulador y, después, conéctese al bot en el emulador:</span><span class="sxs-lookup"><span data-stu-id="34b32-185">Next, start the emulator and then connect to your bot in the emulator:</span></span>

1. <span data-ttu-id="34b32-186">Haga clic en el vínculo **Open Bot** (Abrir bot) de la pestaña de bienvenida del emulador.</span><span class="sxs-lookup"><span data-stu-id="34b32-186">Click the **Open Bot** link in the emulator "Welcome" tab.</span></span> 
2. <span data-ttu-id="34b32-187">Seleccione el archivo .bot ubicado en el directorio donde se creó el proyecto.</span><span class="sxs-lookup"><span data-stu-id="34b32-187">Select the .bot file located in the directory where you created the project.</span></span>

## <a name="interact-with-your-bot"></a><span data-ttu-id="34b32-188">Interacción con el bot</span><span class="sxs-lookup"><span data-stu-id="34b32-188">Interact with your bot</span></span>
<span data-ttu-id="34b32-189">Envíe un mensaje al bot y este enumerará los mensajes que ha recibido.</span><span class="sxs-lookup"><span data-stu-id="34b32-189">Send a message to your bot, and the bot will list the messages it received.</span></span>
<span data-ttu-id="34b32-190">![Emulador en ejecución](../media/emulator-v4/emulator-running.png)</span><span class="sxs-lookup"><span data-stu-id="34b32-190">![Emulator running](../media/emulator-v4/emulator-running.png)</span></span>


### <a name="view-your-data"></a><span data-ttu-id="34b32-191">Consulta de los datos</span><span class="sxs-lookup"><span data-stu-id="34b32-191">View your data</span></span>
<span data-ttu-id="34b32-192">Después de ejecutar el bot y guardar la información, lo podemos ver en Azure Portal en la pestaña **Explorador de datos**.</span><span class="sxs-lookup"><span data-stu-id="34b32-192">After you have run your bot and saved your information, we can view it in the Azure portal under the **Data Explorer** tab.</span></span> 

![Ejemplo del Explorador de datos](./media/data_explorer.PNG)

### <a name="manage-concurrency-using-etags"></a><span data-ttu-id="34b32-194">Administración de la simultaneidad mediante eTags</span><span class="sxs-lookup"><span data-stu-id="34b32-194">Manage concurrency using eTags</span></span>
<span data-ttu-id="34b32-195">En nuestro ejemplo de código de bot, establecemos la propiedad `eTag` de cada elemento `IStoreItem` en `*`.</span><span class="sxs-lookup"><span data-stu-id="34b32-195">In our bot code example we set the `eTag` property of each `IStoreItem` to `*`.</span></span> <span data-ttu-id="34b32-196">Cosmos DB utiliza el miembro `eTag` (etiqueta de entidad) del objeto de almacén para administrar la simultaneidad.</span><span class="sxs-lookup"><span data-stu-id="34b32-196">The `eTag` (entity tag) member of your store object is used within Cosmos DB to manage concurrency.</span></span> <span data-ttu-id="34b32-197">La etiqueta `eTag` indica a la base de datos qué hacer si otra instancia del bot ha cambiado el objeto en el mismo almacenamiento en el que está escribiendo su bot.</span><span class="sxs-lookup"><span data-stu-id="34b32-197">The `eTag` tells your database what to do if another instance of the bot has changed the object in the same storage that your bot is writing to.</span></span> 

<!-- define optimistic concurrency -->

#### <a name="last-write-wins---allow-overwrites"></a><span data-ttu-id="34b32-198">La última escritura tiene prioridad: permitir la sobrescritura</span><span class="sxs-lookup"><span data-stu-id="34b32-198">Last write wins - allow overwrites</span></span>
<span data-ttu-id="34b32-199">Un valor de propiedad `eTag` de asterisco (`*`) indica que la última escritura tiene prioridad.</span><span class="sxs-lookup"><span data-stu-id="34b32-199">An `eTag` property value of asterisk (`*`) indicates that the last writer wins.</span></span> <span data-ttu-id="34b32-200">Al crear un nuevo almacén de datos, puede establecer la etiqueta `eTag` de una propiedad en `*` para indicar que no ha guardado previamente los datos que está escribiendo, o que desea que el último escritor sobrescriba cualquier propiedad guardada previamente.</span><span class="sxs-lookup"><span data-stu-id="34b32-200">When creating a new data store, you can set `eTag` of a property to `*` to indicate that you have not previously saved the data that you are writing, or that you want the last writer to overwrite any previously saved property.</span></span> <span data-ttu-id="34b32-201">Si la simultaneidad no es un problema para el bot, establezca la propiedad `eTag` en `*` para habilitar la sobrescritura para los datos que escriba.</span><span class="sxs-lookup"><span data-stu-id="34b32-201">If concurrency is not an issue for your bot, setting the `eTag` property to `*` for any data that you are writing enables overwrites.</span></span>

#### <a name="maintain-concurrency-and-prevent-overwrites"></a><span data-ttu-id="34b32-202">Mantenimiento de la simultaneidad y evitación de la sobrescritura</span><span class="sxs-lookup"><span data-stu-id="34b32-202">Maintain concurrency and prevent overwrites</span></span>
<span data-ttu-id="34b32-203">Cuando almacene datos en Cosmos DB, use un valor distinto de `*` en la propiedad `eTag` si desea evitar el acceso simultáneo a una propiedad y sobrescribir los cambios de otra instancia del bot.</span><span class="sxs-lookup"><span data-stu-id="34b32-203">When storing your data into Cosmos DB, use a value other than `*` for the `eTag` if you want to prevent concurrent access to a property and avoid overwriting changes from another instance of the bot.</span></span> <span data-ttu-id="34b32-204">El bot recibe una respuesta de error con el mensaje `etag conflict key=` cuando intenta guardar los datos de estado, y la etiqueta `eTag` no tiene el mismo valor que `eTag` en el almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="34b32-204">The bot receives an error response with the message `etag conflict key=` when it attempts to save state data and the `eTag` is not the same value as the `eTag` in storage.</span></span> <!-- To control concurrency of data that is stored using `IStorage`, the BotBuilder SDK checks the entity tag (ETag) for `Storage.Write()` requests. -->

<span data-ttu-id="34b32-205">De forma predeterminada, el almacén de Cosmos DB comprueba la igualdad de la propiedad `eTag` de un objeto de almacenamiento cada vez que un bot escribe en ese elemento y, a continuación, lo actualiza a un nuevo valor único después de cada escritura.</span><span class="sxs-lookup"><span data-stu-id="34b32-205">By default, the Cosmos DB store checks the `eTag` property of a storage object for equality every time a bot writes to that item, and then updates it to a new unique value after each write.</span></span> <span data-ttu-id="34b32-206">Si la propiedad `eTag` en la escritura no coincide con la `eTag` en el almacenamiento, significa que otro subproceso o bot ha cambiado los datos.</span><span class="sxs-lookup"><span data-stu-id="34b32-206">If the `eTag` property on write doesn't match the `eTag` in storage, it means another bot or thread changed the data.</span></span> 

<span data-ttu-id="34b32-207">Por ejemplo, supongamos que desea que su bot edite una nota guardada, pero no quiere que sobrescriba los cambios que haya realizado otra instancia del bot.</span><span class="sxs-lookup"><span data-stu-id="34b32-207">For example, let's say you want your bot to edit a saved note, but you don't want your bot to overwrite changes that another instance of the bot has done.</span></span> <span data-ttu-id="34b32-208">Si otra instancia del bot ha realizado modificaciones, lo que quiere es que el usuario edite la versión con las actualizaciones más recientes.</span><span class="sxs-lookup"><span data-stu-id="34b32-208">If another instance of the bot has made edits, you want the user to edit the version with the latest updates.</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="34b32-209">C#</span><span class="sxs-lookup"><span data-stu-id="34b32-209">C#</span></span>](#tab/csharp)

<span data-ttu-id="34b32-210">Primero, cree una clase que implemente `IStoreItem`.</span><span class="sxs-lookup"><span data-stu-id="34b32-210">First, create a class that implements `IStoreItem`.</span></span>

```csharp
public class Note : IStoreItem
{
    public string Name { get; set; }
    public string Contents { get; set; }
    public string ETag { get; set; }
}
```

<span data-ttu-id="34b32-211">A continuación, cree una nota inicial mediante la creación de un objeto de almacenamiento y agregue el objeto a su almacén.</span><span class="sxs-lookup"><span data-stu-id="34b32-211">Next, create an initial note by creating a storage object, and add the object to your store.</span></span>

```csharp
// create a note for the first time, with a non-null, non-* ETag.
var note = new Note { Name = "Shopping List", Contents = "eggs", ETag = "x" };

var changes = Dictionary<string, object>();
{
    changes.Add("Note", note);
};
await NoteStore.WriteAsync(changes, cancellationToken);
```

<span data-ttu-id="34b32-212">A continuación, acceda y actualice la nota más adelante, manteniendo su `eTag` que usted lee desde el almacén.</span><span class="sxs-lookup"><span data-stu-id="34b32-212">Then, access and update the note later, keeping its `eTag` that you read from the store.</span></span>

```csharp
var note = NoteStore.ReadAsync<Note>("Note").Result?.FirstOrDefault().Value;

if (note != null)
{
    note.Contents += ", bread";
    var changes = new Dictionary<string, object>();
    {
         changes.Add("Note1", note);
    };
    await NoteStore.WriteAsync(changes, cancellationToken);
}
```

<span data-ttu-id="34b32-213">Si se ha actualizado la nota en el almacén antes de que haya escrito los cambios, la llamada a `Write` iniciará una excepción.</span><span class="sxs-lookup"><span data-stu-id="34b32-213">If the note was updated in the store before you write your changes, the call to `Write` will throw an exception.</span></span>


# <a name="javascripttabjavascript"></a>[<span data-ttu-id="34b32-214">JavaScript</span><span class="sxs-lookup"><span data-stu-id="34b32-214">JavaScript</span></span>](#tab/javascript)

<span data-ttu-id="34b32-215">Agregue una función auxiliar al final del bot que escriba una nota de ejemplo en un almacén de datos.</span><span class="sxs-lookup"><span data-stu-id="34b32-215">Add a helper function to the end of your bot that will write a sample note to a data store.</span></span>
<span data-ttu-id="34b32-216">Primero, cree un objeto `myNoteData`.</span><span class="sxs-lookup"><span data-stu-id="34b32-216">First create `myNoteData` object.</span></span>

```javascript
// Helper function for writing a sample note to a data store
async function createSampleNote(storage, context) {
    var myNoteData = {
        name: "Shopping List",
        contents: "eggs",
        // If any Note file is already stored, the eTag field
        // must be set to "*" in order to allow writing without first reading the stored eTag
        // otherwise you'll likely get an exception indicating an eTag conflict. 
        eTag: "*"
    }
}
```

<span data-ttu-id="34b32-217">Dentro de la función auxiliar `createSampleNote`, inicialice un objeto `changes`, agréguele las *notas* y, a continuación, escríbalo en el almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="34b32-217">Within the `createSampleNote` helper function initialize a `changes` object and add your *notes* to it, then write it to storage.</span></span>

```javascript
// Write the note data to the "Note" key
var changes = {};
changes["Note"] = myNoteData;
// Creates a file named Note, if it doesn't already exist.
// specifying eTag= "*" will overwrite any existing contents.
// The act of writing to the file automatically updates the eTag property
// The first time you write to Note, the eTag is changed from *, and file contents will become:
//    {"name":"Shopping List","contents":"eggs","eTag":"1"}
try {
    await storage.write(changes);
    await context.sendActivity('Successful created a note.');
} catch (err) {
    await context.sendActivity(`Could not create note: ${err}`);
}
```

<span data-ttu-id="34b32-218">A continuación, para acceder y actualizar la nota más adelante, creamos otra función auxiliar a la que se puede tener acceso cuando el usuario escribe "actualizar nota".</span><span class="sxs-lookup"><span data-stu-id="34b32-218">Then to access and update the note later we create another helper function that can be accessed when the user types in "update note".</span></span>

```javascript
async function updateSampleNote(storage, context) {
    try {
        // Read in a note
        var note = await storage.read(["Note"]);
        console.log(`note.eTag=${note["Note"].eTag}\n note=${JSON.stringify(note)}`);
        // update the note that we just read
        note["Note"].contents += ", bread";
        console.log(`Updated note=${JSON.stringify(note)}`);

        try {
            await storage.write(note); // Write the changes back to storage
            await context.sendActivity('Successfully updated to note.');
        } catch (err) {            console.log(`Write failed: ${err}`);
        }
    }
    catch (err) {
        await context.sendActivity(`Unable to read the Note: ${err}`);
    }
}
```

<span data-ttu-id="34b32-219">Si se ha actualizado la nota en el almacén antes de que haya escrito los cambios, la llamada a `write` iniciará una excepción.</span><span class="sxs-lookup"><span data-stu-id="34b32-219">If the note was updated in the store before you write your changes, the call to `write` will throw an exception.</span></span>

---

<span data-ttu-id="34b32-220">Para mantener la simultaneidad, siempre es necesario leer una propiedad desde el almacenamiento y luego modificar la propiedad que se ha leído, de esta manera `eTag` se mantiene.</span><span class="sxs-lookup"><span data-stu-id="34b32-220">To maintain concurrency, always read a property from storage, then modify the property you read, so that the `eTag` is maintained.</span></span> <span data-ttu-id="34b32-221">Si lee datos de usuario desde el almacén, la respuesta contendrá la propiedad de eTag.</span><span class="sxs-lookup"><span data-stu-id="34b32-221">If you read user data from the store, the response will contain the eTag property.</span></span> <span data-ttu-id="34b32-222">Si cambia los datos y escribe datos actualizados en el almacén, la solicitud debe incluir la propiedad de eTag que especifica el mismo valor que leyó anteriormente.</span><span class="sxs-lookup"><span data-stu-id="34b32-222">If you change the data and write updated data to the store, your request should include the eTag property that specifies the same value as you read earlier.</span></span> <span data-ttu-id="34b32-223">Sin embargo, escribir un objeto con su `eTag` establecida en `*` permitirá que la escritura sobrescriba cualquier otro cambio.</span><span class="sxs-lookup"><span data-stu-id="34b32-223">However, writing an object with its `eTag` set to `*` will allow the write to overwrite any other changes.</span></span>

## <a name="using-blob-storage"></a><span data-ttu-id="34b32-224">Uso de Blob Storage</span><span class="sxs-lookup"><span data-stu-id="34b32-224">Using Blob storage</span></span> 
<span data-ttu-id="34b32-225">Azure Blob Storage es la solución de almacenamiento de objetos de Microsoft para la nube.</span><span class="sxs-lookup"><span data-stu-id="34b32-225">Azure Blob storage is Microsoft's object storage solution for the cloud.</span></span> <span data-ttu-id="34b32-226">Blob Storage está optimizado para el almacenamiento de cantidades masivas de datos no estructurados, como texto o datos binarios.</span><span class="sxs-lookup"><span data-stu-id="34b32-226">Blob storage is optimized for storing massive amounts of unstructured data, such as text or binary data.</span></span>

### <a name="create-your-blob-storage-account"></a><span data-ttu-id="34b32-227">Creación de la cuenta de Blob Storage</span><span class="sxs-lookup"><span data-stu-id="34b32-227">Create your Blob storage account</span></span>
<span data-ttu-id="34b32-228">Para utilizar Blob Storage en el bot, es necesario configurar algunas cosas antes de meternos en el código.</span><span class="sxs-lookup"><span data-stu-id="34b32-228">To use Blob storage in your bot, you'll need to get a few things set up before getting into the code.</span></span>
1. <span data-ttu-id="34b32-229">En una nueva ventana del explorador, inicie sesión en [Azure Portal](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="34b32-229">In a new browser window, sign in to the [Azure portal](http://portal.azure.com).</span></span>
2. <span data-ttu-id="34b32-230">Haga clic en **Crear un recurso > Almacenamiento > Cuenta de almacenamiento: blob, archivo, tabla, cola**</span><span class="sxs-lookup"><span data-stu-id="34b32-230">Click **Create a resource > Storage > Storage account - blob, file, table, queue**</span></span>
3. <span data-ttu-id="34b32-231">En la página **Nueva cuenta**, escriba un **Nombre** para la cuenta de almacenamiento, seleccione **Blob Storage** en **Tipo de cuenta** y proporcione la información de **Ubicación**, **Grupo de recursos** y **Suscripción**.</span><span class="sxs-lookup"><span data-stu-id="34b32-231">In the **New account page**, enter **Name** for the storage account, select **Blob storage** for **Account kind**, provide **Location**, **Resource group** and **Subscription** infomration.</span></span>  
4. <span data-ttu-id="34b32-232">A continuación, haga clic en **Crear**.</span><span class="sxs-lookup"><span data-stu-id="34b32-232">Then click **Create**.</span></span>

![Creación de un almacenamiento de blobs](./media/create-blob-storage.png)

#### <a name="add-configuration-information"></a><span data-ttu-id="34b32-234">Adición de la información de configuración</span><span class="sxs-lookup"><span data-stu-id="34b32-234">Add configuration information</span></span>

<span data-ttu-id="34b32-235">Busque las claves de Blob Storage que necesita para configurar el almacenamiento de blobs del bot, tal y como se indicó anteriormente:</span><span class="sxs-lookup"><span data-stu-id="34b32-235">Find the Blob Storage keys you need to configure Blob Storage for your bot as shown above:</span></span>
1. <span data-ttu-id="34b32-236">En Azure Portal, abra la cuenta de Blob Storage y seleccione **Configuración > Claves de acceso**.</span><span class="sxs-lookup"><span data-stu-id="34b32-236">In the Azure portal, open your Blob Storage account and select **Settings > Access keys**.</span></span>

![Búsqueda de las claves de Blob Storage](./media/find-blob-storage-keys.png)

<span data-ttu-id="34b32-238">Ahora usaremos dos de estas claves para proporcionar acceso a nuestro código a la cuenta de Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="34b32-238">We will now use two of these keys to provide our code with access to our Blob Storage account.</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="34b32-239">C#</span><span class="sxs-lookup"><span data-stu-id="34b32-239">C#</span></span>](#tab/csharp)

```csharp
using Microsoft.Bot.Builder.Azure;
```

<span data-ttu-id="34b32-240">Actualice la línea de código que apunta "_myStorage_" a la cuenta de Blob Storage existente.</span><span class="sxs-lookup"><span data-stu-id="34b32-240">Update the line of code that points "_myStorage_" to your existing Blob Storage account.</span></span>

```csharp
private static readonly AzureBlobStorage _myStorage = new AzureBlobStorage("<your-blob-storage-account-string>", "<your-blob-storage-container-name>");
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="34b32-241">JavaScript</span><span class="sxs-lookup"><span data-stu-id="34b32-241">JavaScript</span></span>](#tab/javascript)
```javascript
const mystorage = new BlobStorage({
   <youy_containerName>,
   <your_storageAccountOrConnectionString>,
   <your_storageAccessKey>
})
```
---

<span data-ttu-id="34b32-242">Una vez que "_myStorage_" se establece para que apunte a la cuenta de Blob Storage, el código del bot almacena y recupera los datos desde Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="34b32-242">Once "_myStorage_" is set to point to your Blob Storage account, your bot code will now store and retrieve data from Blob Storage.</span></span>

## <a name="start-your-bot"></a><span data-ttu-id="34b32-243">Inicio del bot</span><span class="sxs-lookup"><span data-stu-id="34b32-243">Start your bot</span></span>
<span data-ttu-id="34b32-244">Ejecute el bot localmente.</span><span class="sxs-lookup"><span data-stu-id="34b32-244">Run your bot locally.</span></span>

## <a name="start-the-emulator-and-connect-your-bot"></a><span data-ttu-id="34b32-245">Inicio del emulador y conexión del bot</span><span class="sxs-lookup"><span data-stu-id="34b32-245">Start the emulator and connect your bot</span></span>
<span data-ttu-id="34b32-246">A continuación, inicie el emulador y, después, conéctese al bot en el emulador:</span><span class="sxs-lookup"><span data-stu-id="34b32-246">Next, start the emulator and then connect to your bot in the emulator:</span></span>

1. <span data-ttu-id="34b32-247">Haga clic en el vínculo **Open Bot** (Abrir bot) de la pestaña de bienvenida del emulador.</span><span class="sxs-lookup"><span data-stu-id="34b32-247">Click the **Open Bot** link in the emulator "Welcome" tab.</span></span> 
2. <span data-ttu-id="34b32-248">Seleccione el archivo .bot ubicado en el directorio donde se creó el proyecto.</span><span class="sxs-lookup"><span data-stu-id="34b32-248">Select the .bot file located in the directory where you created the project.</span></span>

## <a name="interact-with-your-bot"></a><span data-ttu-id="34b32-249">Interacción con el bot</span><span class="sxs-lookup"><span data-stu-id="34b32-249">Interact with your bot</span></span>
<span data-ttu-id="34b32-250">Envíe un mensaje al bot y este enumerará los mensajes que ha recibido.</span><span class="sxs-lookup"><span data-stu-id="34b32-250">Send a message to your bot, and the bot will list the messages it received.</span></span>
<span data-ttu-id="34b32-251">![Emulador en ejecución](../media/emulator-v4/emulator-running.png)</span><span class="sxs-lookup"><span data-stu-id="34b32-251">![Emulator running](../media/emulator-v4/emulator-running.png)</span></span>

### <a name="view-your-data"></a><span data-ttu-id="34b32-252">Consulta de los datos</span><span class="sxs-lookup"><span data-stu-id="34b32-252">View your data</span></span>
<span data-ttu-id="34b32-253">Después de ejecutar el bot y guardar la información, lo podemos ver en la pestaña **Explorador de Storage** de Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="34b32-253">After you have run your bot and saved your information, we can view it in under the **Storage Explorer** tab in the Azure portal.</span></span>

## <a name="blob-transcript-storage"></a><span data-ttu-id="34b32-254">Almacenamiento de transcripciones de blobs</span><span class="sxs-lookup"><span data-stu-id="34b32-254">Blob transcript storage</span></span>
<span data-ttu-id="34b32-255">El almacenamiento de transcripciones de blobs de Azure ofrece una opción de almacenamiento especializado que le permite guardar y recuperar con facilidad las conversaciones del usuario en forma de una transcripción grabada.</span><span class="sxs-lookup"><span data-stu-id="34b32-255">Azure blob transcript storage provides a specialized storage option that allows you to easily save and retrieve user conversations in the form of a recorded transcript.</span></span> <span data-ttu-id="34b32-256">El almacenamiento de transcripciones de blobs de Azure es especialmente útil para capturar automáticamente las entradas de usuario para examinarlas durante la depuración del rendimiento del bot.</span><span class="sxs-lookup"><span data-stu-id="34b32-256">Azure blob transcript storage is particularly useful for automatically capturing user inputs to examine when debugging of your bot's performance.</span></span>

### <a name="set-up"></a><span data-ttu-id="34b32-257">Instalación</span><span class="sxs-lookup"><span data-stu-id="34b32-257">Set up</span></span>
<span data-ttu-id="34b32-258">El almacenamiento de transcripciones de blobs de Azure puede usar la misma cuenta de Blob Storage que creó siguiendo los pasos detallados en las secciones "_Creación de la cuenta de Blob Storage_" y "_Adición de la información de configuración_" anteriores.</span><span class="sxs-lookup"><span data-stu-id="34b32-258">Azure blob transcript storage can use the same blob storage account created following the steps detailed in sections "_Create your blob storage account_" and "_Add configuration information_" above.</span></span> <span data-ttu-id="34b32-259">Para este análisis, hemos agregado un nuevo contenedor de blobs, "_mybottranscripts_".</span><span class="sxs-lookup"><span data-stu-id="34b32-259">For this discussion, we have added a new blob container, "_mybottranscripts_."</span></span> 

### <a name="implementation"></a><span data-ttu-id="34b32-260">Implementación</span><span class="sxs-lookup"><span data-stu-id="34b32-260">Implementation</span></span> 
<span data-ttu-id="34b32-261">El siguiente código conecta el puntero de almacenamiento de transcripciones "_myTranscripts_" a la nueva cuenta de almacenamiento de transcripciones de blobs de Azure.</span><span class="sxs-lookup"><span data-stu-id="34b32-261">The following code connects transcript storage pointer "_myTranscripts_" to your new Azure blob transcript storage account.</span></span> <span data-ttu-id="34b32-262">Línea que conecta el puntero de almacenamiento de transcripciones "_myTranscripts_."</span><span class="sxs-lookup"><span data-stu-id="34b32-262">Single line that connects transcript storage pointer "_myTranscripts_."</span></span>

```csharp
using Microsoft.Bot.Builder.Azure;
private readonly AzureBlobTranscriptStore _myTranscripts = new AzureBlobTranscriptStore("<your-blob-storage-account-string>", "<your-blob-storage-account-name>");
```

### <a name="store-user-conversations-in-azure-blob-transcripts"></a><span data-ttu-id="34b32-263">Almacenamiento de las conversaciones del usuario en transcripciones de blobs de Azure</span><span class="sxs-lookup"><span data-stu-id="34b32-263">Store user conversations in azure blob transcripts</span></span>
<span data-ttu-id="34b32-264">Después de tener disponible un contenedor de blobs para almacenar las transcripciones, puede comenzar a conservar las conversaciones de los usuarios con el bot.</span><span class="sxs-lookup"><span data-stu-id="34b32-264">After a blob container is available to store transcripts you can begin to preserve your users' conversations with your bot.</span></span> <span data-ttu-id="34b32-265">Estas conversaciones se pueden utilizar más adelante como una herramienta de depuración para ver cómo interactúan los usuarios con el bot.</span><span class="sxs-lookup"><span data-stu-id="34b32-265">These conversations can later be used as a debugging tool to see how users are interacting with your bot.</span></span> <span data-ttu-id="34b32-266">El siguiente código conserva las entradas de la conversación del usuario para su posterior revisión.</span><span class="sxs-lookup"><span data-stu-id="34b32-266">The following code preserves user conversation inputs for later review.</span></span>


```csharp
/// <summary>
/// Every Conversation turn for our EchoBot will call this method. 
/// </summary>
/// <param name="context">Turn scoped context containing all the data needed
/// for processing this conversation turn. </param>        
public async Task OnTurnAsync(ITurnContext context, CancellationToken cancellationToken = default(CancellationToken))
{
    var activityType = context.Activity.Type;

    await context.SendActivityAsync($"Activity type: {context.Activity.Type}.");

    // This bot is processing Messages
    if (activityType == ActivityTypes.Message)
    {
        // save user input into bot transcript storage for later debugging and review.
        await _myTranscripts.LogActivityAsync(context.Activity);
```


### <a name="find-all-stored-transcripts-for-your-channel"></a><span data-ttu-id="34b32-267">Búsqueda de todas las transcripciones almacenadas del canal</span><span class="sxs-lookup"><span data-stu-id="34b32-267">Find all stored transcripts for your channel</span></span>
<span data-ttu-id="34b32-268">Para ver qué datos se han almacenado, puede usar el código siguiente para buscar mediante programación los elementos "_ConversationID_" de todas las transcripciones que ha almacenado.</span><span class="sxs-lookup"><span data-stu-id="34b32-268">To see what data you have stored, you can use the following code to programmatically find the "_ConversationIDs_" for all transcripts that you have stored.</span></span> <span data-ttu-id="34b32-269">Al usar el emulador para probar el código, seleccione "_Start Over_" (Volver a empezar) para comenzar una transcripción nueva con un nuevo "_ConversationID_".</span><span class="sxs-lookup"><span data-stu-id="34b32-269">When using the bot emulator to test your code, selecting "_Start Over_" begins a new transcript with a new "_ConversationID_."</span></span>

```csharp
List<string> storedTranscripts = new List<string>();
PagedResult<Transcript> pagedResult = null;
var pageSize = 0;
do
{
    pagedResult = await _myTranscripts.ListTranscriptsAsync("emulator", pagedResult?.ContinuationToken);
    
    // transcript item contains ChannelId, Created, Id.
    // save the converasationIds (Id) found by "ListTranscriptsAsync" to a local list.
    foreach (var item in pagedResult.Items)
    {
         // Make sure we store an unescaped conversationId string.
         var strConversationId = item.Id;
         storedTranscripts.Add(Uri.UnescapeDataString(strConversationId));
    }
} while (pagedResult.ContinuationToken != null);
```


### <a name="retrieve-user-conversations-from-azure-blob-transcript-storage"></a><span data-ttu-id="34b32-270">Recuperación de las conversaciones del usuario desde el almacenamiento de transcripciones de blobs de Azure</span><span class="sxs-lookup"><span data-stu-id="34b32-270">Retrieve user conversations from azure blob transcript storage</span></span>
<span data-ttu-id="34b32-271">Una vez que las transcripciones de interacción del bot se han almacenado en el almacén de transcripciones de blobs de Azure, se pueden recuperar mediante programación para realizar pruebas o depuración mediante el método de AzureBlobTranscriptStorage, "_GetTranscriptActivities_".</span><span class="sxs-lookup"><span data-stu-id="34b32-271">Once bot interaction transcripts have been stored into your Azure blob transcript store, you can programmatically retrieve them for testing or debugging using the AzureBlobTranscriptStorage method, "_GetTranscriptActivities_".</span></span> <span data-ttu-id="34b32-272">El fragmento de código siguiente recupera todas las transcripciones de entradas de usuario que se han recibido y almacenado en las últimas 24 horas de cada transcripción almacenada.</span><span class="sxs-lookup"><span data-stu-id="34b32-272">The following code snippet retrieves all user inputs transcripts that were received and stored within the previous 24 hours from each stored transcript.</span></span>


```csharp
var numTranscripts = storedTranscripts.Count();
for (int i = 0; i < numTranscripts; i++)
{
    PagedResult<IActivity> pagedActivities = null;
    do
    {
        string thisConversationId = storedTranscripts[i];
        // Find all inputs in the last 24 hours.
        DateTime yesterday = DateTime.Now.AddDays(-1);
        // Retrieve iActivities for this transcript.
        pagedActivities = await _myTranscripts.GetTranscriptActivitiesAsync("emulator", thisConversationId, pagedActivities?.ContinuationToken, yesterday);
        foreach (var item in pagedActivities.Items)
        {
            // View as message and find value for key "text" :
            var thisMessage = item.AsMessageActivity();
            var userInput = thisMessage.Text;
         }
    } while (pagedActivities.ContinuationToken != null);
}
```

### <a name="remove-stored-transcripts-from-azure-blob-transcript-storage"></a><span data-ttu-id="34b32-273">Eliminación de las transcripciones almacenadas del almacenamiento de transcripciones de blobs de Azure</span><span class="sxs-lookup"><span data-stu-id="34b32-273">Remove stored transcripts from azure blob transcript storage</span></span>
<span data-ttu-id="34b32-274">Una vez que haya terminado de utilizar los datos de entrada de usuario para probar o depurar el bot, las transcripciones almacenadas se pueden eliminar mediante programación del almacén de transcripciones de blobs de Azure.</span><span class="sxs-lookup"><span data-stu-id="34b32-274">Once you have finished using user input data to test or debug you bot, stored transcripts can be programmatically removed from your Azure blob transcript store.</span></span> <span data-ttu-id="34b32-275">El siguiente fragmento de código elimina todas las transcripciones almacenadas del almacén de transcripciones del bot.</span><span class="sxs-lookup"><span data-stu-id="34b32-275">The following code snippet removes all stored transcripts from your bot transcript store.</span></span>


```csharp
for (int i = 0; i < numTranscripts; i++)
{
   // Remove all stored transcripts except the last one found.
   if (i > 0)
   {
       string thisConversationId = storedTranscripts[i];    
       await _myTranscripts.DeleteTranscriptAsync("emulator", thisConversationId);
    }
}
```


<span data-ttu-id="34b32-276">El vínculo siguiente proporciona más información sobre el [almacenamiento de transcripciones de blobs de Azure](https://docs.microsoft.com/en-us/dotnet/api/microsoft.bot.builder.azure.azureblobtranscriptstore)</span><span class="sxs-lookup"><span data-stu-id="34b32-276">This following link provides more information concerning [Azure Blob Transcript Storage](https://docs.microsoft.com/en-us/dotnet/api/microsoft.bot.builder.azure.azureblobtranscriptstore)</span></span>  

## <a name="next-steps"></a><span data-ttu-id="34b32-277">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="34b32-277">Next steps</span></span>
<span data-ttu-id="34b32-278">Ahora que sabe cómo leer lectura y escritura directamente desde el almacenamiento, eche un vistazo a cómo puede usar el Administrador de estado para que lo haga por usted.</span><span class="sxs-lookup"><span data-stu-id="34b32-278">Now that you know how to read read and write directly from storage, lets take a look at how you can use the state manager to do that for you.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="34b32-279">Guardar el estado mediante las propiedades de usuario y de conversación</span><span class="sxs-lookup"><span data-stu-id="34b32-279">Save state using conversation and user properties</span></span>](bot-builder-howto-v4-state.md)

