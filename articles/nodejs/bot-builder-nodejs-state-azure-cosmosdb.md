---
title: Administración de datos de estado personalizado con Azure Cosmos DB | Microsoft Docs
description: Aprenda a guardar y a recuperar datos de estado mediante Azure Cosmos DB con Bot Builder SDK para Node.js.
author: DucVo
ms.author: v-ducvo
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: 9d3e1c315399ce3cadc6371ceb93055c836590a6
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39306216"
---
# <a name="manage-custom-state-data-with-azure-cosmos-db-for-nodejs"></a><span data-ttu-id="ed4b7-103">Administración de datos de estado personalizado con Azure Cosmos DB para Node.js</span><span class="sxs-lookup"><span data-stu-id="ed4b7-103">Manage custom state data with Azure Cosmos DB for Node.js</span></span>

<span data-ttu-id="ed4b7-104">En este artículo, implementará el almacenamiento de Cosmos DB para almacenar y administrar los datos de estado del bot.</span><span class="sxs-lookup"><span data-stu-id="ed4b7-104">In this article, you’ll implement Cosmos DB storage to store and manage your bot’s state data.</span></span> <span data-ttu-id="ed4b7-105">El servicio de estado de Connector predeterminado que usan los bots no es adecuado para el entorno de producción.</span><span class="sxs-lookup"><span data-stu-id="ed4b7-105">The default Connector State Service used by bots is not intended for the production environment.</span></span> <span data-ttu-id="ed4b7-106">Debería usar las [extensiones de Azure](https://www.npmjs.com/package/botbuilder-azure) disponibles en GitHub o implementar un cliente de estado personalizado mediante la plataforma de almacenamiento de datos de su elección.</span><span class="sxs-lookup"><span data-stu-id="ed4b7-106">You should either use [Azure Extensions](https://www.npmjs.com/package/botbuilder-azure) available on GitHub or implement a custom state client using data storage platform of your choice.</span></span> <span data-ttu-id="ed4b7-107">Estas son algunas de las razones para usar el almacenamiento de estado personalizado:</span><span class="sxs-lookup"><span data-stu-id="ed4b7-107">Here are some of the reasons to use custom state storage:</span></span>

- <span data-ttu-id="ed4b7-108">mayor rendimiento de la API de estado (más control sobre el rendimiento)</span><span class="sxs-lookup"><span data-stu-id="ed4b7-108">higher state API throughput (more control over performance)</span></span>
- <span data-ttu-id="ed4b7-109">latencia más baja para la distribución geográfica</span><span class="sxs-lookup"><span data-stu-id="ed4b7-109">lower-latency for geo-distribution</span></span>
- <span data-ttu-id="ed4b7-110">control sobre dónde se almacenan los datos (p. ej.: Oeste de EE. UU. frente a Este de EE. UU.)</span><span class="sxs-lookup"><span data-stu-id="ed4b7-110">control over where the data is stored (e.g.: West US vs East US)</span></span>
- <span data-ttu-id="ed4b7-111">acceso a los datos de estado real</span><span class="sxs-lookup"><span data-stu-id="ed4b7-111">access to the actual state data</span></span>
- <span data-ttu-id="ed4b7-112">base de datos de estado no compartida con otros bots</span><span class="sxs-lookup"><span data-stu-id="ed4b7-112">state data db not shared with other bots</span></span>
- <span data-ttu-id="ed4b7-113">almacenamiento de más de 32 kb</span><span class="sxs-lookup"><span data-stu-id="ed4b7-113">store more than 32kb</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ed4b7-114">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="ed4b7-114">Prerequisites</span></span>

- <span data-ttu-id="ed4b7-115">[Node.js](https://nodejs.org/en/).</span><span class="sxs-lookup"><span data-stu-id="ed4b7-115">[Node.js](https://nodejs.org/en/).</span></span>
- [<span data-ttu-id="ed4b7-116">Bot Framework Emulator</span><span class="sxs-lookup"><span data-stu-id="ed4b7-116">Bot Framework Emulator</span></span>](~/bot-service-debug-emulator.md)
- <span data-ttu-id="ed4b7-117">Debe tener un bot de Node.js.</span><span class="sxs-lookup"><span data-stu-id="ed4b7-117">Must have a Node.js bot.</span></span> <span data-ttu-id="ed4b7-118">Si no tiene uno, vaya a la sección sobre [cómo crear un bot](bot-builder-nodejs-quickstart.md).</span><span class="sxs-lookup"><span data-stu-id="ed4b7-118">If you do not have one, go [create a bot](bot-builder-nodejs-quickstart.md).</span></span> 

## <a name="create-azure-account"></a><span data-ttu-id="ed4b7-119">Creación de una cuenta de Azure</span><span class="sxs-lookup"><span data-stu-id="ed4b7-119">Create Azure account</span></span>
<span data-ttu-id="ed4b7-120">Si no tiene una cuenta de Azure, haga clic [aquí](https://azure.microsoft.com/en-us/free/) para registrarse y obtener una gratuita.</span><span class="sxs-lookup"><span data-stu-id="ed4b7-120">If you don't have an Azure account, click [here](https://azure.microsoft.com/en-us/free/) to sign up for a free account.</span></span>

## <a name="set-up-the-azure-cosmos-db-database"></a><span data-ttu-id="ed4b7-121">Configuración de la base de datos de Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="ed4b7-121">Set up the Azure Cosmos DB database</span></span>
1. <span data-ttu-id="ed4b7-122">Una vez que ha iniciado sesión en Azure Portal, haga clic en *Nuevo* para crear una base de datos de **Azure Cosmos DB**.</span><span class="sxs-lookup"><span data-stu-id="ed4b7-122">After you’ve logged into the Azure portal, create a new *Azure Cosmos DB* database by clicking **New**.</span></span> 
2. <span data-ttu-id="ed4b7-123">Haga clic en **Bases de datos**.</span><span class="sxs-lookup"><span data-stu-id="ed4b7-123">Click **Databases**.</span></span> 
3. <span data-ttu-id="ed4b7-124">Busque **Azure Cosmos DB** y haga clic en **Crear**.</span><span class="sxs-lookup"><span data-stu-id="ed4b7-124">Find **Azure Cosmos DB** and click **Create**.</span></span>
4. <span data-ttu-id="ed4b7-125">Rellene los campos.</span><span class="sxs-lookup"><span data-stu-id="ed4b7-125">Fill in the fields.</span></span> <span data-ttu-id="ed4b7-126">Para el campo **API**, seleccione **SQL (DocumentDB)**.</span><span class="sxs-lookup"><span data-stu-id="ed4b7-126">For the **API** field, select **SQL (DocumentDB)**.</span></span> <span data-ttu-id="ed4b7-127">Una vez rellenados todos los campos, haga clic en el botón **Crear** en la parte inferior de la pantalla para implementar la nueva base de datos.</span><span class="sxs-lookup"><span data-stu-id="ed4b7-127">When done filling in all the fields, click the **Create** button at the bottom of the screen to deploy the new database.</span></span> 
5. <span data-ttu-id="ed4b7-128">Tras la implementación de la nueva base de datos, vaya a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="ed4b7-128">After the new database is deployed, navigate to your new database.</span></span> <span data-ttu-id="ed4b7-129">Haga clic en **Claves de acceso** para encontrar las claves y las cadenas de conexión.</span><span class="sxs-lookup"><span data-stu-id="ed4b7-129">Click **Access keys** to find keys and connection strings.</span></span> <span data-ttu-id="ed4b7-130">El bot usará esta información para llamar al servicio de almacenamiento para guardar los datos de estado.</span><span class="sxs-lookup"><span data-stu-id="ed4b7-130">Your bot will use this information to call the storage service to save state data.</span></span>

## <a name="install-botbuilder-azure-module"></a><span data-ttu-id="ed4b7-131">Instalación del módulo botbuilder-azure</span><span class="sxs-lookup"><span data-stu-id="ed4b7-131">Install botbuilder-azure module</span></span>

<span data-ttu-id="ed4b7-132">Para instalar el módulo `botbuilder-azure` desde un símbolo del sistema, vaya al directorio del bot y ejecute el siguiente comando npm:</span><span class="sxs-lookup"><span data-stu-id="ed4b7-132">To install the `botbuilder-azure` module from a command prompt, navigate to the bot's directory and run the following npm command:</span></span>

```nodejs
npm install --save botbuilder-azure
```

## <a name="modify-your-bot-code"></a><span data-ttu-id="ed4b7-133">Modificación del código del bot</span><span class="sxs-lookup"><span data-stu-id="ed4b7-133">Modify your bot code</span></span>

<span data-ttu-id="ed4b7-134">Para usar la base de datos de **Azure Cosmos DB**, agregue las siguientes líneas de código al archivo **app.js** del bot.</span><span class="sxs-lookup"><span data-stu-id="ed4b7-134">To use your **Azure Cosmos DB** database, add the following lines of code to your bot's **app.js** file.</span></span>

1. <span data-ttu-id="ed4b7-135">Reclame el módulo recién instalado.</span><span class="sxs-lookup"><span data-stu-id="ed4b7-135">Require the newly installed module.</span></span>

   ```javascript
   var azure = require('botbuilder-azure'); 
   ```

2. <span data-ttu-id="ed4b7-136">Configure las opciones de conexión a Azure.</span><span class="sxs-lookup"><span data-stu-id="ed4b7-136">Configure the connection settings to connect to Azure.</span></span>
   ```javascript
   var documentDbOptions = {
       host: 'Your-Azure-DocumentDB-URI', 
       masterKey: 'Your-Azure-DocumentDB-Key', 
       database: 'botdocs',   
       collection: 'botdata'
   };
   ```
   <span data-ttu-id="ed4b7-137">Los valores `host` y `masterKey` se pueden encontrar en el menú **Claves** de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="ed4b7-137">The `host` and `masterKey` values can be found in the **Keys** menu of your database.</span></span> <span data-ttu-id="ed4b7-138">Si las entradas `database` y `collection` no existen en la base de datos de Azure, se crearán automáticamente.</span><span class="sxs-lookup"><span data-stu-id="ed4b7-138">If the `database` and `collection` entries do not exist in the Azure database, they will be created for you.</span></span>

3. <span data-ttu-id="ed4b7-139">Use el módulo `botbuilder-azure` para crear dos objetos para conectarse a la base de datos de Azure.</span><span class="sxs-lookup"><span data-stu-id="ed4b7-139">Using the `botbuilder-azure` module, create two new objects to connect to the Azure database.</span></span> <span data-ttu-id="ed4b7-140">En primer lugar, cree una instancia de `DocumentDBClient` pasando los valores de configuración de conexión (que se define como `documentDbOptions` a partir de lo anterior).</span><span class="sxs-lookup"><span data-stu-id="ed4b7-140">First, create an instance of `DocumentDBClient` passing in the connection configuration settings (defined as `documentDbOptions` from above).</span></span> <span data-ttu-id="ed4b7-141">A continuación, cree una instancia de `AzureBotStorage` pasando el objeto `DocumentDBClient`.</span><span class="sxs-lookup"><span data-stu-id="ed4b7-141">Next, create an instance of `AzureBotStorage` passing in the `DocumentDBClient` object.</span></span> <span data-ttu-id="ed4b7-142">Por ejemplo: </span><span class="sxs-lookup"><span data-stu-id="ed4b7-142">For example:</span></span>
   ```javascript
   var docDbClient = new azure.DocumentDbClient(documentDbOptions);

   var cosmosStorage = new azure.AzureBotStorage({ gzipData: false }, docDbClient);
   ```

4. <span data-ttu-id="ed4b7-143">Especifique que quiere usar su base de datos personalizada en lugar del almacenamiento en memoria.</span><span class="sxs-lookup"><span data-stu-id="ed4b7-143">Specify that you want to use your custom database instead of the in-memory storage.</span></span> <span data-ttu-id="ed4b7-144">Por ejemplo: </span><span class="sxs-lookup"><span data-stu-id="ed4b7-144">For example:</span></span>

   ```javascript
   var bot = new builder.UniversalBot(connector, function (session) {
        // ... Bot code ...
   })
   .set('storage', cosmosStorage);
   ```

<span data-ttu-id="ed4b7-145">Ahora está listo para probar el bot con el emulador.</span><span class="sxs-lookup"><span data-stu-id="ed4b7-145">Now you are ready to test the bot with the emulator.</span></span>

## <a name="run-your-bot-app"></a><span data-ttu-id="ed4b7-146">Ejecución de la aplicación bot</span><span class="sxs-lookup"><span data-stu-id="ed4b7-146">Run your bot app</span></span>

<span data-ttu-id="ed4b7-147">Desde un símbolo del sistema, vaya al directorio del bot y ejecute el bot con el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="ed4b7-147">From a command prompt, navigate to your bot's directory and run your bot with the following command:</span></span>

```nodejs
node app.js
```

## <a name="connect-your-bot-to-the-emulator"></a><span data-ttu-id="ed4b7-148">Conexión del bot al emulador</span><span class="sxs-lookup"><span data-stu-id="ed4b7-148">Connect your bot to the emulator</span></span>

<span data-ttu-id="ed4b7-149">En este momento, el bot se ejecuta de forma local.</span><span class="sxs-lookup"><span data-stu-id="ed4b7-149">At this point, your bot is running locally.</span></span> <span data-ttu-id="ed4b7-150">Inicie el emulador y, después, conéctese al bot desde él:</span><span class="sxs-lookup"><span data-stu-id="ed4b7-150">Start the emulator and then connect to your bot from the emulator:</span></span>

1. <span data-ttu-id="ed4b7-151">Escriba <strong>http://localhost:port-number/api/messages</strong> en la barra de direcciones del emulador, donde port-number coincide con el número de puerto mostrado en el explorador donde se ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ed4b7-151">Type <strong>http://localhost:port-number/api/messages</strong> into the emulator's address bar, where port-number matches the port number shown in the browser where your application is running.</span></span> <span data-ttu-id="ed4b7-152">Por ahora puede dejar en blanco los campos <strong>Microsoft App ID</strong> (Id. de aplicación de Microsoft) y <strong>Microsoft App Password</strong> (Contraseña de aplicación de Mictosoft).</span><span class="sxs-lookup"><span data-stu-id="ed4b7-152">You can leave <strong>Microsoft App ID</strong> and <strong>Microsoft App Password</strong> fields blank for now.</span></span> <span data-ttu-id="ed4b7-153">Más adelante obtendrá esta información al [registrar el bot](~/bot-service-quickstart-registration.md).</span><span class="sxs-lookup"><span data-stu-id="ed4b7-153">You'll get this information later when you [register your bot](~/bot-service-quickstart-registration.md).</span></span>
2. <span data-ttu-id="ed4b7-154">Haga clic en **Conectar**.</span><span class="sxs-lookup"><span data-stu-id="ed4b7-154">Click **Connect**.</span></span>
3. <span data-ttu-id="ed4b7-155">Para probar el bot, envíe a su bot un mensaje.</span><span class="sxs-lookup"><span data-stu-id="ed4b7-155">Test your bot by sending your bot a message.</span></span> <span data-ttu-id="ed4b7-156">Interactúe con el bot como haría normalmente.</span><span class="sxs-lookup"><span data-stu-id="ed4b7-156">Interact with your bot as you normally would.</span></span> <span data-ttu-id="ed4b7-157">Cuando haya terminado, vaya a **Explorador de Storage** y vea los datos de estado guardados.</span><span class="sxs-lookup"><span data-stu-id="ed4b7-157">When you are done, go to **Storage Explorer** and view your saved state data.</span></span>

## <a name="view-state-data-on-azure-portal"></a><span data-ttu-id="ed4b7-158">Vista de los datos de estado en Azure Portal</span><span class="sxs-lookup"><span data-stu-id="ed4b7-158">View state data on Azure Portal</span></span>

<span data-ttu-id="ed4b7-159">Para ver los datos de estado, inicie sesión en Azure Portal y vaya a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="ed4b7-159">To view the state data, sign into your Azure portal and navigate to your database.</span></span> <span data-ttu-id="ed4b7-160">Haga clic en **Explorador de datos (versión preliminar)** para verificar que se está guardando la información de estado del bot.</span><span class="sxs-lookup"><span data-stu-id="ed4b7-160">Click  **Data Explorer (preview)** to verify that the state information from your bot is being saved.</span></span>

## <a name="next-step"></a><span data-ttu-id="ed4b7-161">Paso siguiente</span><span class="sxs-lookup"><span data-stu-id="ed4b7-161">Next step</span></span>

<span data-ttu-id="ed4b7-162">Ahora que tiene control total sobre los datos de estado del bot, echemos un vistazo a cómo puede usarlos para administrar mejor el flujo de conversación.</span><span class="sxs-lookup"><span data-stu-id="ed4b7-162">Now that you have full control over your bot's state data, let's take a look at how you can use it to better manage conversation flow.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="ed4b7-163">Administración de flujo de conversación</span><span class="sxs-lookup"><span data-stu-id="ed4b7-163">Manage conversation flow</span></span>](bot-builder-nodejs-dialog-manage-conversation-flow.md)