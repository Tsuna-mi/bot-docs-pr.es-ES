---
title: Administración de datos de estado personalizado con Azure Table Storage | Microsoft Docs
description: Aprenda a guardar y a recuperar datos de estado mediante Azure Table Storage con el SDK de Bot Builder para Node.js.
author: DucVo
ms.author: v-ducvo
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: 84fdbecfe59db49e2e88567a6c942c300ea226a2
ms.sourcegitcommit: 2dc75701b169d822c9499e393439161bc87639d2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42904365"
---
# <a name="manage-custom-state-data-with-azure-table-storage-for-nodejs"></a><span data-ttu-id="1a5fd-103">Administración de datos de estado personalizado con Azure Table Storage para Node.js</span><span class="sxs-lookup"><span data-stu-id="1a5fd-103">Manage custom state data with Azure Table storage for Node.js</span></span>

[!INCLUDE [pre-release-label](../includes/pre-release-label-v3.md)]

<span data-ttu-id="1a5fd-104">En este artículo, implementará Azure Table Storage para almacenar y administrar los datos de estado del bot.</span><span class="sxs-lookup"><span data-stu-id="1a5fd-104">In this article, you’ll implement Azure Table storage to store and manage your bot’s state data.</span></span> <span data-ttu-id="1a5fd-105">El servicio de estado de Connector predeterminado que usan los bots no es adecuado para el entorno de producción.</span><span class="sxs-lookup"><span data-stu-id="1a5fd-105">The default Connector State Service used by bots is not intended for the production environment.</span></span> <span data-ttu-id="1a5fd-106">Deberá usar las [extensiones de Azure](https://www.npmjs.com/package/botbuilder-azure) disponibles en GitHub o implementar un cliente de estado personalizado mediante la plataforma de almacenamiento de datos de su elección.</span><span class="sxs-lookup"><span data-stu-id="1a5fd-106">You should either use [Azure Extensions](https://www.npmjs.com/package/botbuilder-azure) available on GitHub or implement a custom state client using data storage platform of your choice.</span></span> <span data-ttu-id="1a5fd-107">Estas son algunas de las razones para usar el almacenamiento de estado personalizado:</span><span class="sxs-lookup"><span data-stu-id="1a5fd-107">Here are some of the reasons to use custom state storage:</span></span>

- <span data-ttu-id="1a5fd-108">mayor rendimiento de la API de estado (más control sobre el rendimiento)</span><span class="sxs-lookup"><span data-stu-id="1a5fd-108">higher state API throughput (more control over performance)</span></span>
- <span data-ttu-id="1a5fd-109">latencia más baja para la distribución geográfica</span><span class="sxs-lookup"><span data-stu-id="1a5fd-109">lower-latency for geo-distribution</span></span>
- <span data-ttu-id="1a5fd-110">control sobre dónde se almacenan los datos (p. ej.: oeste de Estados Unidos frente a este de Estados Unidos)</span><span class="sxs-lookup"><span data-stu-id="1a5fd-110">control over where the data is stored (e.g.: West US vs East US)</span></span>
- <span data-ttu-id="1a5fd-111">acceso a los datos de estado reales</span><span class="sxs-lookup"><span data-stu-id="1a5fd-111">access to the actual state data</span></span>
- <span data-ttu-id="1a5fd-112">base de datos de estado no compartida con otros bots</span><span class="sxs-lookup"><span data-stu-id="1a5fd-112">state data db not shared with other bots</span></span>
- <span data-ttu-id="1a5fd-113">almacenamiento de más de 32 kb</span><span class="sxs-lookup"><span data-stu-id="1a5fd-113">store more than 32kb</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1a5fd-114">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="1a5fd-114">Prerequisites</span></span>

- <span data-ttu-id="1a5fd-115">[Node.js](https://nodejs.org/en/).</span><span class="sxs-lookup"><span data-stu-id="1a5fd-115">[Node.js](https://nodejs.org/en/).</span></span>
- <span data-ttu-id="1a5fd-116">[Bot Framework Emulator](~/bot-service-debug-emulator.md).</span><span class="sxs-lookup"><span data-stu-id="1a5fd-116">[Bot Framework Emulator](~/bot-service-debug-emulator.md).</span></span>
- <span data-ttu-id="1a5fd-117">Debe tener un bot de Node.js.</span><span class="sxs-lookup"><span data-stu-id="1a5fd-117">Must have a Node.js bot.</span></span> <span data-ttu-id="1a5fd-118">Si no tiene uno, vaya a la sección sobre [cómo crear un bot](bot-builder-nodejs-quickstart.md).</span><span class="sxs-lookup"><span data-stu-id="1a5fd-118">If you do not have one, go [create a bot](bot-builder-nodejs-quickstart.md).</span></span> 
- <span data-ttu-id="1a5fd-119">[Explorador de Storage](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="1a5fd-119">[Storage Explorer](http://storageexplorer.com/).</span></span>

## <a name="create-azure-account"></a><span data-ttu-id="1a5fd-120">Creación de una cuenta de Azure</span><span class="sxs-lookup"><span data-stu-id="1a5fd-120">Create Azure account</span></span>
<span data-ttu-id="1a5fd-121">Si no tiene una cuenta de Azure, haga clic [aquí](https://azure.microsoft.com/en-us/free/) para registrarse y obtener una gratuita.</span><span class="sxs-lookup"><span data-stu-id="1a5fd-121">If you don't have an Azure account, click [here](https://azure.microsoft.com/en-us/free/) to sign up for a free account.</span></span>

## <a name="set-up-the-azure-table-storage-service"></a><span data-ttu-id="1a5fd-122">Configuración del servicio de Azure Table Storage</span><span class="sxs-lookup"><span data-stu-id="1a5fd-122">Set up the Azure Table storage service</span></span>
1. <span data-ttu-id="1a5fd-123">Una vez que ha iniciado sesión en Azure Portal, haga clic en **Nuevo** para crear un servicio de Azure Table Storage.</span><span class="sxs-lookup"><span data-stu-id="1a5fd-123">After you’ve logged into the Azure portal, create a new Azure Table storage service by clicking on **New**.</span></span> 
2. <span data-ttu-id="1a5fd-124">Busque la **cuenta de almacenamiento** que implementa la tabla de Azure.</span><span class="sxs-lookup"><span data-stu-id="1a5fd-124">Search for **Storage account** that implements the Azure Table.</span></span> <span data-ttu-id="1a5fd-125">Haga clic en **Crear** para comenzar a crear la cuenta de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="1a5fd-125">Click **Create** to start creating the storage account.</span></span> 
3. <span data-ttu-id="1a5fd-126">Rellene los campos, haga clic en el botón **Crear** en la parte inferior de la pantalla para implementar el nuevo servicio de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="1a5fd-126">Fill in the fields, click the **Create** button at the bottom of the screen to deploy the new storage service.</span></span> 
4. <span data-ttu-id="1a5fd-127">Después de implementar el nuevo servicio de almacenamiento, vaya a la cuenta de almacenamiento que acaba de crear.</span><span class="sxs-lookup"><span data-stu-id="1a5fd-127">After the new storage service is deployed, navigate to the storage account you just created.</span></span> <span data-ttu-id="1a5fd-128">Puede encontrarla en la hoja **Cuentas de almacenamiento**.</span><span class="sxs-lookup"><span data-stu-id="1a5fd-128">You can find it listed in the **Storage Accounts** blade.</span></span>
4. <span data-ttu-id="1a5fd-129">Seleccione **Claves de acceso** y copie la clave para usarla más adelante.</span><span class="sxs-lookup"><span data-stu-id="1a5fd-129">Select **Access keys**, and copy the key for later use.</span></span> <span data-ttu-id="1a5fd-130">El bot usará el **nombre de la cuenta de almacenamiento** y la **clave** para llamar al servicio de almacenamiento para guardar los datos de estado.</span><span class="sxs-lookup"><span data-stu-id="1a5fd-130">Your bot will use **Storage account name** and **Key** to call the storage service to save state data.</span></span>

## <a name="install-botbuilder-azure-module"></a><span data-ttu-id="1a5fd-131">Instalación del módulo botbuilder-azure</span><span class="sxs-lookup"><span data-stu-id="1a5fd-131">Install botbuilder-azure module</span></span>

<span data-ttu-id="1a5fd-132">Para instalar el módulo `botbuilder-azure` desde un símbolo del sistema, vaya al directorio del bot y ejecute el siguiente comando npm:</span><span class="sxs-lookup"><span data-stu-id="1a5fd-132">To install the `botbuilder-azure` module from a command prompt, navigate to the bot's directory and run the following npm command:</span></span>

```nodejs
npm install --save botbuilder-azure
```

## <a name="modify-your-bot-code"></a><span data-ttu-id="1a5fd-133">Modificación del código del bot</span><span class="sxs-lookup"><span data-stu-id="1a5fd-133">Modify your bot code</span></span>

<span data-ttu-id="1a5fd-134">Para usar el almacenamiento de **Azure Table**, agregue las siguientes líneas de código al archivo **app.js** del bot.</span><span class="sxs-lookup"><span data-stu-id="1a5fd-134">To use your **Azure Table** storage, add the following lines of code to your bot's **app.js** file.</span></span>

1. <span data-ttu-id="1a5fd-135">Reclame el módulo recién instalado.</span><span class="sxs-lookup"><span data-stu-id="1a5fd-135">Require the newly installed module.</span></span>

   ```javascript
   var azure = require('botbuilder-azure'); 
   ```

2. <span data-ttu-id="1a5fd-136">Configure las opciones de conexión a Azure.</span><span class="sxs-lookup"><span data-stu-id="1a5fd-136">Configure the connection settings to connect to Azure.</span></span>
   ```javascript
   // Table storage
   var tableName = "Table-Name"; // You define
   var storageName = "Table-Storage-Name"; // Obtain from Azure Portal
   var storageKey = "Azure-Table-Key"; // Obtain from Azure Portal
   ```
   <span data-ttu-id="1a5fd-137">Los valores `storageName` y `storageKay` se pueden encontrar en el menú **Claves de acceso** de Azure Table.</span><span class="sxs-lookup"><span data-stu-id="1a5fd-137">The `storageName` and `storageKay` values can be found in the **Access keys** menu of your Azure Table.</span></span> <span data-ttu-id="1a5fd-138">Si `tableName` no existe en Azure Table, se crea automáticamente.</span><span class="sxs-lookup"><span data-stu-id="1a5fd-138">If the `tableName` does not exist in the Azure Table, it will be created for you.</span></span>

3. <span data-ttu-id="1a5fd-139">Use el módulo `botbuilder-azure` para crear dos objetos para conectarse a la tabla de Azure.</span><span class="sxs-lookup"><span data-stu-id="1a5fd-139">Using the `botbuilder-azure` module, create two new objects to connect to the Azure Table.</span></span> <span data-ttu-id="1a5fd-140">En primer lugar, cree una instancia de `AzureTableClient` pasando los valores de configuración de conexión.</span><span class="sxs-lookup"><span data-stu-id="1a5fd-140">First, create an instance of `AzureTableClient` passing in the connection configuration settings.</span></span> <span data-ttu-id="1a5fd-141">A continuación, cree una instancia de `AzureBotStorage` pasando el objeto `AzureTableClient`.</span><span class="sxs-lookup"><span data-stu-id="1a5fd-141">Next, create an instance of `AzureBotStorage` passing in the `AzureTableClient` object.</span></span> <span data-ttu-id="1a5fd-142">Por ejemplo: </span><span class="sxs-lookup"><span data-stu-id="1a5fd-142">For example:</span></span>

   ```javascript
   var azureTableClient = new azure.AzureTableClient(tableName, storageName, storageKey);

   var tableStorage = new azure.AzureBotStorage({gzipData: false}, azureTableClient);
   ```

4. <span data-ttu-id="1a5fd-143">Especifique que quiere usar su base de datos personalizada en lugar del almacenamiento en memoria y agregue información de sesión a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="1a5fd-143">Specify that you want to use your custom database instead of the in-memory storage and add session information to database.</span></span> <span data-ttu-id="1a5fd-144">Por ejemplo: </span><span class="sxs-lookup"><span data-stu-id="1a5fd-144">For example:</span></span>

   ```javascript
   var bot = new builder.UniversalBot(connector, function (session) {
        // ... Bot code ...

        // capture session user information
        session.userData = {"userId": session.message.user.id, "jobTitle": "Senior Developer"};

        // capture conversation information  
        session.conversationData[timestamp.toISOString().replace(/:/g,"-")] = session.message.text;

        // save data
        session.save();
   })
   .set('storage', tableStorage);
   ```
<span data-ttu-id="1a5fd-145">Ahora está listo para probar el bot con el emulador.</span><span class="sxs-lookup"><span data-stu-id="1a5fd-145">Now you are ready to test the bot with the emulator.</span></span>

## <a name="run-your-bot-app"></a><span data-ttu-id="1a5fd-146">Ejecución de la aplicación bot</span><span class="sxs-lookup"><span data-stu-id="1a5fd-146">Run your bot app</span></span>

<span data-ttu-id="1a5fd-147">Desde un símbolo del sistema, vaya al directorio del bot y ejecute el bot con el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="1a5fd-147">From a command prompt, navigate to your bot's directory and run your bot with the following command:</span></span>

```nodejs
node app.js
```

## <a name="connect-your-bot-to-the-emulator"></a><span data-ttu-id="1a5fd-148">Conexión del bot al emulador</span><span class="sxs-lookup"><span data-stu-id="1a5fd-148">Connect your bot to the emulator</span></span>

<span data-ttu-id="1a5fd-149">En este momento, el bot se ejecuta de forma local.</span><span class="sxs-lookup"><span data-stu-id="1a5fd-149">At this point, your bot is running locally.</span></span> <span data-ttu-id="1a5fd-150">Inicie el emulador y, después, conéctese al bot desde él:</span><span class="sxs-lookup"><span data-stu-id="1a5fd-150">Start the emulator and then connect to your bot from the emulator:</span></span>

1. <span data-ttu-id="1a5fd-151">Escriba <strong>http://localhost:port-number/api/messages</strong> en la barra de direcciones del emulador, donde port-number coincide con el número de puerto mostrado en el explorador donde se ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1a5fd-151">Type <strong>http://localhost:port-number/api/messages</strong> into the emulator's address bar, where port-number matches the port number shown in the browser where your application is running.</span></span> <span data-ttu-id="1a5fd-152">Por ahora puede dejar en blanco los campos <strong>Microsoft App ID</strong> (Id. de aplicación de Microsoft) y <strong>Microsoft App Password</strong> (Contraseña de aplicación de Microsoft).</span><span class="sxs-lookup"><span data-stu-id="1a5fd-152">You can leave <strong>Microsoft App ID</strong> and <strong>Microsoft App Password</strong> fields blank for now.</span></span> <span data-ttu-id="1a5fd-153">Más adelante obtendrá esta información al [registrar el bot](~/bot-service-quickstart-registration.md).</span><span class="sxs-lookup"><span data-stu-id="1a5fd-153">You'll get this information later when you [register your bot](~/bot-service-quickstart-registration.md).</span></span>
2. <span data-ttu-id="1a5fd-154">Haga clic en **Conectar**.</span><span class="sxs-lookup"><span data-stu-id="1a5fd-154">Click **Connect**.</span></span>
3. <span data-ttu-id="1a5fd-155">Para probar el bot, envíe a su bot un mensaje.</span><span class="sxs-lookup"><span data-stu-id="1a5fd-155">Test your bot by sending your bot a message.</span></span> <span data-ttu-id="1a5fd-156">Interactúe con el bot como haría normalmente.</span><span class="sxs-lookup"><span data-stu-id="1a5fd-156">Interact with your bot as you normally would.</span></span> <span data-ttu-id="1a5fd-157">Cuando haya terminado, vaya al **Explorador de Storage** y vea los datos de estado guardados.</span><span class="sxs-lookup"><span data-stu-id="1a5fd-157">When you are done, go to **Storage Explorer** and view your saved state data.</span></span>

## <a name="view-data-in-storage-explorer"></a><span data-ttu-id="1a5fd-158">Visualización de los datos en el Explorador de Storage</span><span class="sxs-lookup"><span data-stu-id="1a5fd-158">View data in Storage Explorer</span></span>

<span data-ttu-id="1a5fd-159">Para ver los datos de estado, abra el **Explorador de Storage** y conéctese a Azure con sus credenciales de Azure Portal o conéctese directamente a la tabla mediante `storageName` y `storageKey` y navegue hasta su `tableName`.</span><span class="sxs-lookup"><span data-stu-id="1a5fd-159">To view the state data, open **Storage Explorer** and connect to Azure using your Azure Portal credential or connect directly to the Table using the `storageName` and `storageKey` and navigate to your `tableName`.</span></span> 

![Captura de pantalla del Explorador de Storage con filas de tabla de datos de bot](~/media/bot-builder-nodejs-state-azure-table-storage/bot-builder-nodejs-state-azure-table-storage-query.png)

<span data-ttu-id="1a5fd-161">Un registro de la conversación en la columna **datos** tiene esta apariencia:</span><span class="sxs-lookup"><span data-stu-id="1a5fd-161">One record of the conversation in the **data** column looks like:</span></span>

```JSON
{
    "2018-05-15T18-23-48.780Z": "I'm the second user",
    "2018-05-15T18-23-55.120Z": "Do you know what time it is?",
    "2018-05-15T18-24-12.214Z": "I'm looking for information about the new process."
}
```

## <a name="next-step"></a><span data-ttu-id="1a5fd-162">Paso siguiente</span><span class="sxs-lookup"><span data-stu-id="1a5fd-162">Next step</span></span>

<span data-ttu-id="1a5fd-163">Ahora que tiene control total sobre los datos de estado del bot, echemos un vistazo a cómo puede usarlos para administrar mejor el flujo de conversación.</span><span class="sxs-lookup"><span data-stu-id="1a5fd-163">Now that you have full control over your bot's state data, let's take a look at how you can use it to better manage conversation flow.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="1a5fd-164">Administración de flujo de conversación</span><span class="sxs-lookup"><span data-stu-id="1a5fd-164">Manage conversation flow</span></span>](bot-builder-nodejs-dialog-manage-conversation-flow.md)
