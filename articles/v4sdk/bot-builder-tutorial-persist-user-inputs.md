---
title: Conservación de datos de usuario | Microsoft Docs
description: Obtenga información sobre cómo guardar los datos de estado de los usuario en el almacenamiento del Bot Builder SDK.
keywords: conversación de datos de usuario, almacenamiento, datos de conversación
author: v-ducvo
ms.author: v-ducvo
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 09/19/2018
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: b70f0bfbc76ad06be30fc7f590118b69ab1baf92
ms.sourcegitcommit: 3cb288cf2f09eaede317e1bc8d6255becf1aec61
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2018
ms.locfileid: "47389744"
---
# <a name="persist-user-data"></a><span data-ttu-id="94096-104">Conservación de datos de usuario</span><span class="sxs-lookup"><span data-stu-id="94096-104">Persist user data</span></span>

[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]

<span data-ttu-id="94096-105">Cuando el bot pide una entrada a los usuarios, lo más probable es que quiera conservar parte de la información en el almacenamiento de alguna forma.</span><span class="sxs-lookup"><span data-stu-id="94096-105">When the bot ask users for input, chances are that you would want to persist some of the information to storage of some form.</span></span> <span data-ttu-id="94096-106">El SDK Bot Builder permite almacenar las entradas de los usuarios mediante *almacenamiento en memoria* o almacenamiento en una base de datos como *CosmosDB*.</span><span class="sxs-lookup"><span data-stu-id="94096-106">The Bot Builder SDK allows you to store user inputs using *in-memory storage* or database storage such as *CosmosDB*.</span></span> <span data-ttu-id="94096-107">Los tipos de almacenamiento local se usan principalmente durante la realización de pruebas o la creación de prototipos del bot.</span><span class="sxs-lookup"><span data-stu-id="94096-107">Local storage types are mainly used during testing or prototyping of your bot.</span></span> <span data-ttu-id="94096-108">Sin embargo, los tipos de almacenamiento persistente, como el almacenamiento de base de datos, son los mejores para los bots de producción.</span><span class="sxs-lookup"><span data-stu-id="94096-108">However, persistent storage types, such as database storage, are best for production bots.</span></span> 

<span data-ttu-id="94096-109">En este tema se muestra cómo definir un objeto de almacenamiento y guardar las entradas de usuario en dicho objeto para que se pueda conservar.</span><span class="sxs-lookup"><span data-stu-id="94096-109">This topic shows you how to define your storage object and save user inputs to the storage object so that it can be persisted.</span></span> <span data-ttu-id="94096-110">Usaremos un diálogo para pedir al usuario su nombre, en caso de que no lo tengamos aún.</span><span class="sxs-lookup"><span data-stu-id="94096-110">We'll use a dialog to ask the user for their name, if we don't already have it.</span></span> <span data-ttu-id="94096-111">Independientemente del tipo de almacenamiento que decida usar, el proceso de enlazar y conservar los datos es el mismo.</span><span class="sxs-lookup"><span data-stu-id="94096-111">Regardless of the storage type you choose to use, the process for hooking it up and persisting data is the same.</span></span> <span data-ttu-id="94096-112">El código de este tema usa `CosmosDB` como espacio de almacenamiento para conservar los datos.</span><span class="sxs-lookup"><span data-stu-id="94096-112">The code in this topic uses `CosmosDB` as the storage to persist data.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="94096-113">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="94096-113">Prerequisites</span></span>

<span data-ttu-id="94096-114">Se requieren ciertos recursos, en función del entorno de desarrollo que se desee usar.</span><span class="sxs-lookup"><span data-stu-id="94096-114">Certain resources are required, based on the development environment you want to use.</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="94096-115">C#</span><span class="sxs-lookup"><span data-stu-id="94096-115">C#</span></span>](#tab/csharp)

* <span data-ttu-id="94096-116">[Instalación de Visual Studio 2015 o superior](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="94096-116">[Install Visual Studio 2015 or greater](https://www.visualstudio.com/downloads/).</span></span>
* <span data-ttu-id="94096-117">[Instalación de la plantilla de Bot Builder V4](https://marketplace.visualstudio.com/items?itemName=BotBuilder.botbuilderv4).</span><span class="sxs-lookup"><span data-stu-id="94096-117">[Install the BotBuilder V4 Template](https://marketplace.visualstudio.com/items?itemName=BotBuilder.botbuilderv4).</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="94096-118">JavaScript</span><span class="sxs-lookup"><span data-stu-id="94096-118">JavaScript</span></span>](#tab/javascript)

* <span data-ttu-id="94096-119">[Instalación de Visual Studio Code](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="94096-119">[Install Visual Studio Code](https://www.visualstudio.com/downloads/).</span></span>
* <span data-ttu-id="94096-120">[Instalación de Node.js v8.5 o superior](https://nodejs.org/en/).</span><span class="sxs-lookup"><span data-stu-id="94096-120">[Install Node.js v8.5 or greater](https://nodejs.org/en/).</span></span>
* <span data-ttu-id="94096-121">[Instalación de Yeoman](http://yeoman.io/).</span><span class="sxs-lookup"><span data-stu-id="94096-121">[Install Yeoman](http://yeoman.io/).</span></span>
* <span data-ttu-id="94096-122">Instale el generador de plantillas para Bot Builder v4 de Node.js.</span><span class="sxs-lookup"><span data-stu-id="94096-122">Install the Node.js v4 Botbuilder template generator.</span></span>

    ```shell
    npm install generator-botbuilder
    ```

---

<span data-ttu-id="94096-123">En este tutorial se usan los siguientes paquetes.</span><span class="sxs-lookup"><span data-stu-id="94096-123">This tutorial makes use of the following packages.</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="94096-124">C#</span><span class="sxs-lookup"><span data-stu-id="94096-124">C#</span></span>](#tab/csharp)

<span data-ttu-id="94096-125">Instale estos paquetes desde el administrador de paquetes de NuGet.</span><span class="sxs-lookup"><span data-stu-id="94096-125">Install these packages from the NuGet packet manager.</span></span>

* <span data-ttu-id="94096-126">**Microsoft.Bot.Builder.Azure**</span><span class="sxs-lookup"><span data-stu-id="94096-126">**Microsoft.Bot.Builder.Azure**</span></span>
* <span data-ttu-id="94096-127">**Microsoft.Bot.Builder.Dialogs**</span><span class="sxs-lookup"><span data-stu-id="94096-127">**Microsoft.Bot.Builder.Dialogs**</span></span>
* <span data-ttu-id="94096-128">**Microsoft.Bot.Builder.Integration.AspNet.Core**</span><span class="sxs-lookup"><span data-stu-id="94096-128">**Microsoft.Bot.Builder.Integration.AspNet.Core**</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="94096-129">JavaScript</span><span class="sxs-lookup"><span data-stu-id="94096-129">JavaScript</span></span>](#tab/javascript)

<span data-ttu-id="94096-130">Vaya a la carpeta de proyecto del bot e instale el paquete `botbuilder-dialogs` de NPM:</span><span class="sxs-lookup"><span data-stu-id="94096-130">Navigate to your bot's project folder and install the `botbuilder-dialogs` package from NPM:</span></span>

```cmd
npm install --save botbuilder-dialogs
npm install --save botbuilder-azure
```

---

<span data-ttu-id="94096-131">Para probar el bot que cree en este tutorial, tendrá que instalar [BotFramework Emulator](https://github.com/Microsoft/BotFramework-Emulator).</span><span class="sxs-lookup"><span data-stu-id="94096-131">To test the bot you create in this tutorial, you will need to install the [BotFramework Emulator](https://github.com/Microsoft/BotFramework-Emulator).</span></span>

## <a name="create-a-cosmosdb-service-and-update-your-application-settings"></a><span data-ttu-id="94096-132">Creación de un servicio CosmosDB y actualización de la configuración de la aplicación</span><span class="sxs-lookup"><span data-stu-id="94096-132">Create a CosmosDB service and update your application settings</span></span>
<span data-ttu-id="94096-133">Para configurar un servicio CosmosDB y una base de datos, siga las instrucciones para [usar CosmosDB](bot-builder-howto-v4-storage.md#using-cosmos-db).</span><span class="sxs-lookup"><span data-stu-id="94096-133">To set up a CosmosDB service and a database, follow the instructions for [using CosmosDB](bot-builder-howto-v4-storage.md#using-cosmos-db).</span></span> <span data-ttu-id="94096-134">Aquí se resumen los pasos:</span><span class="sxs-lookup"><span data-stu-id="94096-134">The steps are summarized here:</span></span>
   1. <span data-ttu-id="94096-135">En una nueva ventana del explorador, inicie sesión en <a href="http://portal.azure.com/" target="_blank">Azure Portal</a>.</span><span class="sxs-lookup"><span data-stu-id="94096-135">In a new browser window, sign in to the <a href="http://portal.azure.com/" target="_blank">Azure portal</a>.</span></span>
   1. <span data-ttu-id="94096-136">Haga clic en **Crear un recurso > Bases de datos > Azure Cosmos DB**.</span><span class="sxs-lookup"><span data-stu-id="94096-136">Click **Create a resource > Databases > Azure Cosmos DB**.</span></span>
   1. <span data-ttu-id="94096-137">En la página **Nueva cuenta**, proporcione un nombre único en el campo **ID**.</span><span class="sxs-lookup"><span data-stu-id="94096-137">In the **New account** page, provide a unique name in the **ID** field.</span></span> <span data-ttu-id="94096-138">En **API**, seleccione **SQL** y proporcione la información de **Suscripción**, **Ubicación** y **Grupo de recursos**.</span><span class="sxs-lookup"><span data-stu-id="94096-138">For **API**, select **SQL**, and provide **Subscription**, **Location**, and **Resource group** information.</span></span>
   1. <span data-ttu-id="94096-139">Haga clic en **Create**(Crear).</span><span class="sxs-lookup"><span data-stu-id="94096-139">Click **Create**.</span></span>

<span data-ttu-id="94096-140">A continuación, agregue una colección a ese servicio para su uso con este bot.</span><span class="sxs-lookup"><span data-stu-id="94096-140">Then, Add a collection to that service for use with this bot.</span></span>

<span data-ttu-id="94096-141">Registre el identificador de la base de datos y el identificador de la colección que usó para agregar la colección, así como el identificador URI y la clave principal de la configuración de las claves de la colección, ya que serán necesarios para conectar el bot al servicio.</span><span class="sxs-lookup"><span data-stu-id="94096-141">Record the database ID and collection ID you used to add the collection, and also the URI and primary key from the collection's keys settings, as we will need these to connect our bot to the service.</span></span>

### <a name="update-your-application-settings"></a><span data-ttu-id="94096-142">Actualización de la configuración de la aplicación</span><span class="sxs-lookup"><span data-stu-id="94096-142">Update your application settings</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="94096-143">C#</span><span class="sxs-lookup"><span data-stu-id="94096-143">C#</span></span>](#tab/csharp)

<span data-ttu-id="94096-144">Actualice el archivo **appsettings.json** para incluir la información de conexión de CosmosDB.</span><span class="sxs-lookup"><span data-stu-id="94096-144">Update your **appsettings.json** file to include the connection information for CosmosDB .</span></span>

```csharp
{
  // Settings for CosmosDB.
  "CosmosDB": {
    "DatabaseID": "<your-database-identifier>",
    "CollectionID": "<your-collection-identifier>",
    "EndpointUri": "<your-CosmosDB-endpoint>",
    "AuthenticationKey": "<your-primary-key>"
  }
}
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="94096-145">JavaScript</span><span class="sxs-lookup"><span data-stu-id="94096-145">JavaScript</span></span>](#tab/javascript)

<span data-ttu-id="94096-146">En la carpeta del proyecto, busque el archivo **.env** y agréguele estas entradas con los datos específicos de Cosmos.</span><span class="sxs-lookup"><span data-stu-id="94096-146">In your project folder, locate the **.env** file and add these entries with your Cosmos specific data to it.</span></span>

<span data-ttu-id="94096-147">**.env**</span><span class="sxs-lookup"><span data-stu-id="94096-147">**.env**</span></span>

```cmd
DB_SERVICE_ENDPOINT=<database service endpoint>
AUTH_KEY=<authentication key>
DATABASE=<database name>
COLLECTION=<collection name>
```

<span data-ttu-id="94096-148">Luego, en el archivo **index.js** principal del bot, reemplace `storage` para usar `CosmosDbStorage` en lugar de `MemoryStorage`.</span><span class="sxs-lookup"><span data-stu-id="94096-148">Then, in your bot's main **index.js** file, replace `storage` to use `CosmosDbStorage` instead of `MemoryStorage`.</span></span> <span data-ttu-id="94096-149">Durante el tiempo de ejecución, se extraerán las variables de entorno y rellenarán estos campos.</span><span class="sxs-lookup"><span data-stu-id="94096-149">During run time, the environment variables will be pulled in and populate these fields.</span></span>

```javascript
const storage = new CosmosDbStorage({
    serviceEndpoint: process.env.DB_SERVICE_ENDPOINT, 
    authKey: process.env.AUTH_KEY, 
    databaseId: process.env.DATABASE,
    collectionId: process.env.COLLECTION
});
```

---

## <a name="create-storage-state-manager-and-state-property-accessor-objects"></a><span data-ttu-id="94096-150">Creación de los objetos del descriptor de acceso de almacenamiento, administrador de estado y de las propiedades del estado</span><span class="sxs-lookup"><span data-stu-id="94096-150">Create storage, state manager, and state property accessor objects</span></span>

<span data-ttu-id="94096-151">Los bots usan objetos de almacenamiento y administración de estado para administrar y conservar el estado.</span><span class="sxs-lookup"><span data-stu-id="94096-151">Bots use state management and storage objects to manage and persist state.</span></span> <span data-ttu-id="94096-152">El administrador proporciona una capa de abstracción que permite acceder a las propiedades de estado mediante descriptores de acceso de las propiedades del estado, independientemente del tipo de almacenamiento subyacente.</span><span class="sxs-lookup"><span data-stu-id="94096-152">The manager provides an abstraction layer that lets you access state properties using state property accessors, independent of the type of underlying storage.</span></span> <span data-ttu-id="94096-153">Use el administrador de estado para escribir datos en el espacio de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="94096-153">Use the state manager to write data to storage.</span></span> 


# <a name="ctabcsharp"></a>[<span data-ttu-id="94096-154">C#</span><span class="sxs-lookup"><span data-stu-id="94096-154">C#</span></span>](#tab/csharp)

### <a name="define-a-class-for-your-user-data"></a><span data-ttu-id="94096-155">Definición de una clase para los datos de usuario</span><span class="sxs-lookup"><span data-stu-id="94096-155">Define a class for your user data</span></span>

<span data-ttu-id="94096-156">Cambie el nombre del archivo **CounterState.cs** a **UserData.cs**y el de la clase **CounterState** a **UserData**.</span><span class="sxs-lookup"><span data-stu-id="94096-156">Rename the file **CounterState.cs** to **UserData.cs**, and rename the **CounterState** class to **UserData**.</span></span>

<span data-ttu-id="94096-157">Actualice esta clase para que contenga los datos que va a recopilar.</span><span class="sxs-lookup"><span data-stu-id="94096-157">Update this class to hold the data you will collect.</span></span>

```csharp
/// <summary>
/// Class for storing persistent user data.
/// </summary>
public class UserData
{
    public string Name { get; set; }
}
```

### <a name="define-a-class-for-your-state-and-state-property-accessor-objects"></a><span data-ttu-id="94096-158">Definición de una clase para el estado y los objetos del descriptor de acceso de las propiedades del estado</span><span class="sxs-lookup"><span data-stu-id="94096-158">Define a class for your state and state property accessor objects</span></span>

<span data-ttu-id="94096-159">Cambie el nombre del archivo **EchoBotAccessors.cs** a **BotAccessors.cs** y cambiar el nombre de la clase **EchoBotAccessors** a **BotAccessors**.</span><span class="sxs-lookup"><span data-stu-id="94096-159">Rename the file **EchoBotAccessors.cs** to **BotAccessors.cs**, and rename the **EchoBotAccessors** class to **BotAccessors**.</span></span>

<span data-ttu-id="94096-160">Actualice esta clase para almacenar los objetos del estado y los descriptores de acceso de las propiedades del estado que va a necesitar el bot.</span><span class="sxs-lookup"><span data-stu-id="94096-160">Update this class to store the state objects and state property accessors your bot will need.</span></span>

```csharp
using Microsoft.Bot.Builder;
using Microsoft.Bot.Builder.Dialogs;
using System;

public class BotAccessors
{
    public UserState UserState { get; }

    public ConversationState ConversationState { get; }

    public IStatePropertyAccessor<DialogState> DialogStateAccessor { get; set; }

    public IStatePropertyAccessor<UserData> UserDataAccessor { get; set; }

    public BotAccessors(UserState userState, ConversationState conversationState)
    {
        this.UserState = userState
            ?? throw new ArgumentNullException(nameof(userState));

        this.ConversationState = conversationState
            ?? throw new ArgumentNullException(nameof(conversationState));
    }
}
```

### <a name="update-the-startup-code-for-your-bot"></a><span data-ttu-id="94096-161">Actualización del código de inicio para el bot</span><span class="sxs-lookup"><span data-stu-id="94096-161">Update the startup code for your bot</span></span>

<span data-ttu-id="94096-162">En el archivo **Startup.cs**, actualice las instrucciones using.</span><span class="sxs-lookup"><span data-stu-id="94096-162">In your **Startup.cs** file, update your using statements.</span></span>

```csharp
using System;
using System.Linq;
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Bot.Builder;
using Microsoft.Bot.Builder.Azure;
using Microsoft.Bot.Builder.BotFramework;
using Microsoft.Bot.Builder.Dialogs;
using Microsoft.Bot.Builder.Integration;
using Microsoft.Bot.Builder.Integration.AspNet.Core;
using Microsoft.Bot.Builder.TraceExtensions;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Options;
```

<span data-ttu-id="94096-163">En el método `ConfigureServices`, actualice la llamada para agregar el bot a partir del lugar en que se crea el objeto de almacenamiento de respaldo y, después, registre el objeto de descriptores de acceso del bot.</span><span class="sxs-lookup"><span data-stu-id="94096-163">In your `ConfigureServices` method, update the add bot call, starting from where you create the backing storage object, and then register your bot accessors object.</span></span>

<span data-ttu-id="94096-164">Necesitamos el estado de la conversación del objeto `DialogState` para realizar un seguimiento del estado del diálogo.</span><span class="sxs-lookup"><span data-stu-id="94096-164">We need conversation state for the `DialogState` object to track the dialog state.</span></span> <span data-ttu-id="94096-165">Vamos a registrar elementos singleton para el descriptor de acceso de las propiedades del estado del diálogo y el conjunto de diálogos que va a usar nuestro bot.</span><span class="sxs-lookup"><span data-stu-id="94096-165">We're registering singletons for the dialog state property accessor and the dialog set that our bot will use.</span></span> <span data-ttu-id="94096-166">El bot creará su propio descriptor de acceso de las propiedades del estado para el estado de usuario.</span><span class="sxs-lookup"><span data-stu-id="94096-166">The bot will create its own state property accessor for the user state.</span></span>

<span data-ttu-id="94096-167">El descriptor de acceso `BotAccessors` es una manera eficaz de administrar el almacenamiento de varios objetos de estado de un bot.</span><span class="sxs-lookup"><span data-stu-id="94096-167">The `BotAccessors` accessor is an efficient way to manage storage for multiple state objects of your bot.</span></span>

```cs
public void ConfigureServices(IServiceCollection services)
{
    // Register your bot.
    services.AddBot<UserDataBot>(options =>
    {
        // ...

        // Use persistent storage and create state management objects.
        var CosmosSettings = Configuration.GetSection("CosmosDB");
        IStorage storage = new CosmosDbStorage(
            new CosmosDbStorageOptions
            {
                DatabaseId = CosmosSettings["DatabaseID"],
                CollectionId = CosmosSettings["CollectionID"],
                CosmosDBEndpoint = new Uri(CosmosSettings["EndpointUri"]),
                AuthKey = CosmosSettings["AuthenticationKey"],
            });
        options.State.Add(new ConversationState(storage));
        options.State.Add(new UserState(storage));
    });

    // Register the bot's state and state property accessor objects.
    services.AddSingleton<BotAccessors>(sp =>
    {
        var options = sp.GetRequiredService<IOptions<BotFrameworkOptions>>().Value;
        var userState = options.State.OfType<UserState>().FirstOrDefault();
        var conversationState = options.State.OfType<ConversationState>().FirstOrDefault();

        return new BotAccessors(userState, conversationState)
        {
            UserDataAccessor = userState.CreateProperty<UserData>("UserDataBot.UserData"),
            DialogStateAccessor = conversationState.CreateProperty<DialogState>("UserDataBot.DialogState"),
        };
    });
}
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="94096-168">JavaScript</span><span class="sxs-lookup"><span data-stu-id="94096-168">JavaScript</span></span>](#tab/javascript)

### <a name="indexjs"></a><span data-ttu-id="94096-169">index.js</span><span class="sxs-lookup"><span data-stu-id="94096-169">index.js</span></span>

<span data-ttu-id="94096-170">En el archivo **index.js** de su bot principal, actualice las siguientes instrucciones require.</span><span class="sxs-lookup"><span data-stu-id="94096-170">In your main bot's **index.js** file, update the following require statements.</span></span>

```javascript
const { BotFrameworkAdapter, ConversationState, UserState } = require('botbuilder');
const { CosmosDbStorage } = require('botbuilder-azure');
```

<span data-ttu-id="94096-171">Vamos a usar `UserState` para almacenar los datos de este tutorial.</span><span class="sxs-lookup"><span data-stu-id="94096-171">We will be using the `UserState` to store data for this tutorial.</span></span> <span data-ttu-id="94096-172">Necesitamos crear un objeto `userState` y actualizar esta línea de código para pasar un segundo parámetro a la clase `MainDialog`.</span><span class="sxs-lookup"><span data-stu-id="94096-172">We need to create a new `userState` object and update this line of code to pass in a second parameter to the `MainDialog` class.</span></span>

```javascript
// Create conversation state with in-memory storage provider. 
const conversationState = new ConversationState(storage);
const userState = new UserState(storage);

// Create the main dialog.
const mainDlg = new MainDialog(conversationState, userState);
```

### <a name="dialogsmaindialogindexjs"></a><span data-ttu-id="94096-173">dialogs/mainDialog/index.js</span><span class="sxs-lookup"><span data-stu-id="94096-173">dialogs/mainDialog/index.js</span></span>

<span data-ttu-id="94096-174">En la clase `MainDialog`, requiera las bibliotecas necesarias para que el bot funcione.</span><span class="sxs-lookup"><span data-stu-id="94096-174">In the `MainDialog` class, require the necessary libraries for your bot to operate.</span></span> <span data-ttu-id="94096-175">En este tutorial, vamos a usar la biblioteca **Dialogs**.</span><span class="sxs-lookup"><span data-stu-id="94096-175">In this tutorial, we will be using the **Dialogs** library.</span></span>

```javascript
// Required packages for this bot
const { ActivityTypes } = require('botbuilder');
const { DialogSet, WaterfallDialog, TextPrompt, NumberPrompt } = require('botbuilder-dialogs');

```

<span data-ttu-id="94096-176">Actualice el constructor de la clase `MainDialog` para aceptar un segundo parámetro como `userState`.</span><span class="sxs-lookup"><span data-stu-id="94096-176">Update the `MainDialog` class's constructor to accept a second parameter as `userState`.</span></span> <span data-ttu-id="94096-177">Además, actualice el constructor para definir los estados, diálogos y mensajes necesarios para este tutorial.</span><span class="sxs-lookup"><span data-stu-id="94096-177">Also, update the constructor to define the states, dialogs, and prompts we need for this tutorial.</span></span> <span data-ttu-id="94096-178">En este caso, vamos a define una cascada de dos pasos, en la que en el _paso 1_ se pide el nombre al usuario y el _paso 2_ devuelve la entrada del usuario.</span><span class="sxs-lookup"><span data-stu-id="94096-178">In this case, we are defining a two step waterfall where _step 1_ ask the user for their name and _step 2_ returns the user input.</span></span> <span data-ttu-id="94096-179">La lógica principal del bot es la que dicta si se conserva esa información.</span><span class="sxs-lookup"><span data-stu-id="94096-179">It's up to the bot's main logic to persist that information.</span></span>

```javascript
constructor (conversationState, userState) {

    // creates a new state accessor property. see https://aka.ms/about-bot-state-accessors to learn more about the bot state and state accessors 
    this.conversationState = conversationState;
    this.userState = userState;

    this.dialogState = this.conversationState.createProperty('dialogState');

    this.userDataAccessor = this.userState.createProperty('userData');

    this.dialogs = new DialogSet(this.dialogState);
    
    // Add prompts
    this.dialogs.add(new TextPrompt('textPrompt'));
    
    // Check in user:
    this.dialogs.add(new WaterfallDialog('greetings', [
        async function (step) {
            return await step.prompt('textPrompt', "What is your name?");
        },
        async function (step){
            return await step.endDialog(step.result);
        }
    ]));
}
```

---

<span data-ttu-id="94096-180">Cuando llega el momento de guardar los datos del usuario, hay varias opciones entre las que se puede elegir.</span><span class="sxs-lookup"><span data-stu-id="94096-180">When it comes time to save user data, you have some choices.</span></span> <span data-ttu-id="94096-181">El SDK proporciona varios objetos de estado con ámbitos diferentes entre los que puede elegir.</span><span class="sxs-lookup"><span data-stu-id="94096-181">The SDK provides a few state objects with different scopes that you can choose from.</span></span> <span data-ttu-id="94096-182">Aquí vamos a usar el estado de la conversación para administrar el objeto de estado del diálogo y el estado del usuario para administrar los datos del usuario.</span><span class="sxs-lookup"><span data-stu-id="94096-182">Here, we're using conversation state to manage the dialog state object and user state to manage user data.</span></span>

<span data-ttu-id="94096-183">Para más información acerca del almacenamiento y del estado en general, consulte [almacenamiento](bot-builder-howto-v4-storage.md) y [administración del estado del usuario y de la conversación](bot-builder-howto-v4-state.md).</span><span class="sxs-lookup"><span data-stu-id="94096-183">For more information about storage and state in general, see [storage](bot-builder-howto-v4-storage.md) and [how to manage conversation and user state](bot-builder-howto-v4-state.md).</span></span>

## <a name="create-a-greeting-dialog"></a><span data-ttu-id="94096-184">Creación de un diálogo de saludo</span><span class="sxs-lookup"><span data-stu-id="94096-184">Create a greeting dialog</span></span>

<span data-ttu-id="94096-185">Vamos a usar un diálogo para recopilar el nombre del usuario.</span><span class="sxs-lookup"><span data-stu-id="94096-185">We'll use a dialog to collect the user's name.</span></span> <span data-ttu-id="94096-186">Para simplificar este escenario, el diálogo devolverá el nombre del usuario y el bot administrará el objeto de datos de usuario y el estado.</span><span class="sxs-lookup"><span data-stu-id="94096-186">To keep this scenario simple, the dialog will return the user's name, and the bot will manage the user data object and state.</span></span>

<span data-ttu-id="94096-187">Cree una clase **GreetingsDialog** e incluya el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="94096-187">Create a **GreetingsDialog** class and include the following code.</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="94096-188">C#</span><span class="sxs-lookup"><span data-stu-id="94096-188">C#</span></span>](#tab/csharp)

```cs
using Microsoft.Bot.Builder;
using Microsoft.Bot.Builder.Dialogs;

/// <summary>Defines a dialog for collecting a user's name.</summary>
public class GreetingsDialog : DialogSet
{
    /// <summary>The ID of the main dialog.</summary>
    public const string MainDialog = "main";

    /// <summary>The ID of the the text prompt to use in the dialog.</summary>
    private const string TextPrompt = "textPrompt";

    /// <summary>Creates a new instance of this dialog set.</summary>
    /// <param name="dialogState">The dialog state property accessor to use for dialog state.</param>
    public GreetingsDialog(IStatePropertyAccessor<DialogState> dialogState)
        : base(dialogState)
    {
        // Add the text prompt to the dialog set.
        Add(new TextPrompt(TextPrompt));

        // Define the main dialog and add it to the set.
        Add(new WaterfallDialog(MainDialog, new WaterfallStep[]
        {
            async (stepContext, cancellationToken) =>
            {
                // Ask the user for their name.
                return await stepContext.PromptAsync(TextPrompt, new PromptOptions
                {
                    Prompt = MessageFactory.Text("What is your name?"),
                }, cancellationToken);
            },
            async (stepContext, cancellationToken) =>
            {
                // Assume that they entered their name, and return the value.
                return await stepContext.EndDialogAsync(stepContext.Result, cancellationToken);
            },
        }));
    }
}
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="94096-189">JavaScript</span><span class="sxs-lookup"><span data-stu-id="94096-189">JavaScript</span></span>](#tab/javascript)

<span data-ttu-id="94096-190">Vea la sección anterior en la que se crea el diálogo se crea dentro del constructor `MainDialog`.</span><span class="sxs-lookup"><span data-stu-id="94096-190">See the section above where the dialog is created within the `MainDialog`'s constructor.</span></span>

---

<span data-ttu-id="94096-191">Para más información acerca de los diálogos, consulte [solicitud de entrada](bot-builder-prompts.md) y [administración de un flujo de conversación simple](bot-builder-dialog-manage-conversation-flow.md).</span><span class="sxs-lookup"><span data-stu-id="94096-191">For more information about dialogs, see [how to prompt for input](bot-builder-prompts.md) and [how to manage simple conversation flow](bot-builder-dialog-manage-conversation-flow.md).</span></span>

## <a name="update-your-bot-to-use-the-dialog-and-user-state"></a><span data-ttu-id="94096-192">Actualización del bot para que utilice el estado del diálogo y del usuario</span><span class="sxs-lookup"><span data-stu-id="94096-192">Update your bot to use the dialog and user state</span></span>

<span data-ttu-id="94096-193">La construcción de bot y la administración de los datos proporcionados por el usuario se trataran por separado.</span><span class="sxs-lookup"><span data-stu-id="94096-193">We'll discuss bot construction and managing user input separately.</span></span>

### <a name="add-the-dialog-and-a-user-state-accessor"></a><span data-ttu-id="94096-194">Adición del descriptor de acceso del estado del diálogo y del usuario</span><span class="sxs-lookup"><span data-stu-id="94096-194">Add the dialog and a user state accessor</span></span>

<span data-ttu-id="94096-195">Es necesario realizar un seguimiento de la instancia del diálogo y del descriptor de acceso de las propiedades del estado en los datos del usuario.</span><span class="sxs-lookup"><span data-stu-id="94096-195">We need to track the dialog instance and the state property accessor for user data.</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="94096-196">C#</span><span class="sxs-lookup"><span data-stu-id="94096-196">C#</span></span>](#tab/csharp)

<span data-ttu-id="94096-197">Agregue el código para inicializar nuestro bot.</span><span class="sxs-lookup"><span data-stu-id="94096-197">Add code to initialize our bot.</span></span>

```cs
using System.Linq;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.Bot.Builder;
using Microsoft.Bot.Builder.Dialogs;
using Microsoft.Bot.Schema;

/// <summary>Defines the bot for the persisting user data tutorial.</summary>
public class UserDataBot : IBot
{
    /// <summary>The bot's state and state property accessor objects.</summary>
    private BotAccessors Accessors { get; }

    /// <summary>The dialog set that has the dialog to use.</summary>
    private GreetingsDialog GreetingsDialog { get; }

    /// <summary>Create a new instance of the bot.</summary>
    /// <param name="options">The options to use for our app.</param>
    /// <param name="greetingsDialog">An instance of the dialog set.</param>
    public UserDataBot(BotAccessors botAccessors)
    {
        // Retrieve the bot's state and accessors.
        Accessors = botAccessors;

        // Create the greetings dialog.
        GreetingsDialog = new GreetingsDialog(Accessors.DialogStateAccessor);
    }
}
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="94096-198">JavaScript</span><span class="sxs-lookup"><span data-stu-id="94096-198">JavaScript</span></span>](#tab/javascript)

<span data-ttu-id="94096-199">Vea la sección anterior en la que se definen estos descriptores de acceso dentro del constructor de `MainDialog`.</span><span class="sxs-lookup"><span data-stu-id="94096-199">See the section above where these state accessors are defined within the `MainDialog`'s constructor.</span></span>

---

### <a name="update-the-turn-handler"></a><span data-ttu-id="94096-200">Actualización del controlador de turno</span><span class="sxs-lookup"><span data-stu-id="94096-200">Update the turn handler</span></span>

<span data-ttu-id="94096-201">El controlador de turno saludará a los usuarios cuando se incorporen a la conversación y les responderá cuando envíen un mensaje al bot.</span><span class="sxs-lookup"><span data-stu-id="94096-201">The turn handler will greet the user when they first join the conversation, and respond to them whenever they send the bot a message.</span></span> <span data-ttu-id="94096-202">Si en algún momento el botón desconoce el nombre del usuario, iniciará el diálogo de salido para preguntarle su nombre.</span><span class="sxs-lookup"><span data-stu-id="94096-202">If at any point the bot doesn't know the user's name already, it will start the greeting dialog to ask for their name.</span></span> <span data-ttu-id="94096-203">Cuando el diálogo finaliza, se guarda su nombre en el estado del usuario mediante un objeto de estado generado por nuestro descriptor de acceso de propiedades de los estados.</span><span class="sxs-lookup"><span data-stu-id="94096-203">When the dialog completes, we'll save their name to user state using a state object generated by our state property accessor.</span></span> <span data-ttu-id="94096-204">Cuando el turno finaliza, el descriptor de acceso y el administrador de estado escribirán los cambios en el objeto fuera del almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="94096-204">When the turn ends, the accessor and state manager will write changes to the object out to storage.</span></span>

<span data-ttu-id="94096-205">También vamos a agregar compatibilidad con la actividad _eliminar datos de usuario_.</span><span class="sxs-lookup"><span data-stu-id="94096-205">We'll also add support for the _delete user data_ activity.</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="94096-206">C#</span><span class="sxs-lookup"><span data-stu-id="94096-206">C#</span></span>](#tab/csharp)

<span data-ttu-id="94096-207">Actualice el método `OnTurnAsync` del bot.</span><span class="sxs-lookup"><span data-stu-id="94096-207">Update the bot's `OnTurnAsync` method.</span></span>

```cs
/// <summary>Handles incoming activities to the bot.</summary>
/// <param name="turnContext">The context object for the current turn.</param>
/// <param name="cancellationToken">A cancellation token that can be used by other objects
/// or threads to receive notice of cancellation.</param>
/// <returns>A task that represents the work queued to execute.</returns>
public async Task OnTurnAsync(ITurnContext turnContext, CancellationToken cancellationToken = default(CancellationToken))
{
    // Retrieve user data from state.
    UserData userData = await Accessors.UserDataAccessor.GetAsync(turnContext, () => new UserData());

    // Establish context for our dialog from the turn context.
    DialogContext dc = await GreetingsDialog.CreateContextAsync(turnContext);

    // Handle conversation update, message, and delete user data activities.
    switch (turnContext.Activity.Type)
    {
        case ActivityTypes.ConversationUpdate:

            // Greet any user that is added to the conversation.
            IConversationUpdateActivity activity = turnContext.Activity.AsConversationUpdateActivity();
            if (activity.MembersAdded.Any(member => member.Id != activity.Recipient.Id))
            {
                if (userData.Name is null)
                {
                    // If we don't already have their name, start a dialog to collect it.
                    await turnContext.SendActivityAsync("Welcome to the User Data bot.");
                    await dc.BeginDialogAsync(GreetingsDialog.MainDialog);
                }
                else
                {
                    // Otherwise, greet them by name.
                    await turnContext.SendActivityAsync($"Hi {userData.Name}! Welcome back to the User Data bot.");
                }
            }

            break;

        case ActivityTypes.Message:

            // If there's a dialog running, continue it.
            if (dc.ActiveDialog != null)
            {
                var dialogTurnResult = await dc.ContinueDialogAsync();
                if (dialogTurnResult.Status == DialogTurnStatus.Complete
                    && dialogTurnResult.Result is string name
                    && !string.IsNullOrWhiteSpace(name))
                {
                    // If it completes successfully and returns a valid name, save the name and greet the user.
                    userData.Name = name;
                    await turnContext.SendActivityAsync($"Pleased to meet you {userData.Name}.");
                }
            }
            // Else, if we don't have the user's name yet, ask for it.
            else if (userData.Name is null)
            {
                await dc.BeginDialogAsync(GreetingsDialog.MainDialog);
            }
            // Else, echo the user's message text.
            else
            {
                await turnContext.SendActivityAsync($"{userData.Name} said, '{turnContext.Activity.Text}'.");
            }

            break;

        case ActivityTypes.DeleteUserData:

            // Delete the user's data.
            userData.Name = null;
            await turnContext.SendActivityAsync("I have deleted your user data.");

            break;
    }

    // Update the user data in the turn's state cache.
    await Accessors.UserDataAccessor.SetAsync(turnContext, userData, cancellationToken);

    // Persist any changes to storage.
    await Accessors.UserState.SaveChangesAsync(turnContext, false, cancellationToken);
    await Accessors.ConversationState.SaveChangesAsync(turnContext, false, cancellationToken);
}
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="94096-208">JavaScript</span><span class="sxs-lookup"><span data-stu-id="94096-208">JavaScript</span></span>](#tab/javascript)

<span data-ttu-id="94096-209">Actualice el controlador de `MainDialog` `onTurn`.</span><span class="sxs-lookup"><span data-stu-id="94096-209">Update the `MainDialog`'s `onTurn` handler.</span></span>

<span data-ttu-id="94096-210">**dialogs/mainDialog/index.js**</span><span class="sxs-lookup"><span data-stu-id="94096-210">**dialogs/mainDialog/index.js**</span></span>

```javascript
async onTurn(turnContext) {
        
    const dc = await this.dialogs.createContext(turnContext); // Create dialog context
    const userData = await this.userDataAccessor.get(turnContext, {});

    switch(turnContext.activity.type){
        case ActivityTypes.ConversationUpdate:
            if (turnContext.activity.membersAdded[0].name !== 'Bot') {
                if(userData.name){
                    await turnContext.sendActivity(`Hi ${userData.name}! Welcome back to the User Data bot.`);
                }
                else {
                    // send a "this is what the bot does" message
                    await turnContext.sendActivity('Welcome to the User Data bot.');
                    await dc.beginDialog('greetings');
                }
            }
        break;
        case ActivityTypes.Message:
            // If there is an active dialog running, continue it
            if(dc.activeDialog){
                var turnResult = await dc.continueDialog();
                if(turnResult.status == "complete" && turnResult.result){
                    // If it completes successfully and returns a value, save the name and greet the user.
                    userData.name = turnResult.result;
                    await this.userDataAccessor.set(turnContext, userData);
                    await turnContext.sendActivity(`Pleased to meet you ${userData.name}.`);
                }
            }
            // Else, if we don't have the user's name yet, ask for it.
            else if(!userData.name){
                await dc.beginDialog('greetings');
            }
            // Else, echo the user's message text.
            else {
                await turnContext.sendActivity(`${userData.name} said, ${turnContext.activity.text}.`);
            }
        break;
        case "deleteUserData":
            // Delete the user's data.
            // Note: You can use the emuluator to send this activity.
            userData.name = null;
            await this.userDataAccessor.set(turnContext, userData);
            await turnContext.sendActivity("I have deleted your user data.");
        break;
    }

    // Save changes to the user name.
    await this.userState.saveChanges(turnContext);

    // End this turn by saving changes to the conversation state.
    await this.conversationState.saveChanges(turnContext);

}

```

---

## <a name="start-your-bot-in-visual-studio"></a><span data-ttu-id="94096-211">Inicio del bot en Visual Studio</span><span class="sxs-lookup"><span data-stu-id="94096-211">Start your bot in Visual Studio</span></span>
<span data-ttu-id="94096-212">Compile y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="94096-212">Build and run your application.</span></span>

## <a name="start-the-emulator-and-connect-your-bot"></a><span data-ttu-id="94096-213">Inicio del emulador y conexión del bot</span><span class="sxs-lookup"><span data-stu-id="94096-213">Start the emulator and connect your bot</span></span>

<span data-ttu-id="94096-214">A continuación, inicie el emulador y, después, conéctese al bot en el emulador:</span><span class="sxs-lookup"><span data-stu-id="94096-214">Next, start the emulator and then connect to your bot in the emulator:</span></span>

1. <span data-ttu-id="94096-215">Haga clic en el vínculo **Open bot** (Abrir bot) de la pestaña de bienvenida del emulador.</span><span class="sxs-lookup"><span data-stu-id="94096-215">Click the **Open Bot** link in the emulator "Welcome" tab.</span></span> 
2. <span data-ttu-id="94096-216">Seleccione el archivo .bot ubicado en el directorio donde se creó la solución de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="94096-216">Select the .bot file located in the directory where you created the Visual Studio solution.</span></span>

## <a name="interact-with-your-bot"></a><span data-ttu-id="94096-217">Interacción con el bot</span><span class="sxs-lookup"><span data-stu-id="94096-217">Interact with your bot</span></span>
<span data-ttu-id="94096-218">Envíe un mensaje al bot y este responderá con un mensaje.</span><span class="sxs-lookup"><span data-stu-id="94096-218">Send a message to your bot, and the bot will respond back with a message.</span></span>
<span data-ttu-id="94096-219">![Emulador en ejecución](../media/emulator-v4/emulator-running.png)</span><span class="sxs-lookup"><span data-stu-id="94096-219">![Emulator running](../media/emulator-v4/emulator-running.png)</span></span>


## <a name="next-steps"></a><span data-ttu-id="94096-220">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="94096-220">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="94096-221">Administración del estado de la conversación y del usuario</span><span class="sxs-lookup"><span data-stu-id="94096-221">Manage conversation and user state</span></span>](bot-builder-howto-v4-state.md)
