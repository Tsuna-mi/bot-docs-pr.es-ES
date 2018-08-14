---
title: Administración de datos de estado personalizado con Azure Table Storage | Microsoft Docs
description: Aprenda a guardar y recuperar datos de estado mediante Azure Table Storage con Bot Builder SDK para .NET.
author: kaiqb
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: e2d8e6a5a390a27b61b11ad22f07ce0ab95f1686
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39306120"
---
# <a name="manage-custom-state-data-with-azure-table-storage-for-net"></a><span data-ttu-id="f4d52-103">Administración de datos de estado personalizado con Azure Table Storage para .NET</span><span class="sxs-lookup"><span data-stu-id="f4d52-103">Manage custom state data with Azure Table Storage for .NET</span></span>
<span data-ttu-id="f4d52-104">En este artículo, implementará Azure Table Storage para almacenar y administrar los datos de estado del bot.</span><span class="sxs-lookup"><span data-stu-id="f4d52-104">In this article, you’ll implement Azure Table storage to store and manage your bot’s state data.</span></span> <span data-ttu-id="f4d52-105">El servicio de estado de Connector predeterminado que usan los bots no es adecuado para el entorno de producción.</span><span class="sxs-lookup"><span data-stu-id="f4d52-105">The default Connector State Service used by bots is not intended for the production environment.</span></span> <span data-ttu-id="f4d52-106">Debería usar las [extensiones de Azure](https://github.com/Microsoft/BotBuilder-Azure) disponibles en GitHub o implementar un cliente de estado personalizado mediante la plataforma de almacenamiento de datos de su elección.</span><span class="sxs-lookup"><span data-stu-id="f4d52-106">You should either use [Azure Extensions](https://github.com/Microsoft/BotBuilder-Azure) available on GitHub or implement a custom state client using data storage platform of your choice.</span></span> <span data-ttu-id="f4d52-107">Estas son algunas de las razones para usar el almacenamiento de estado personalizado:</span><span class="sxs-lookup"><span data-stu-id="f4d52-107">Here are some of the reasons to use custom state storage:</span></span>
 - <span data-ttu-id="f4d52-108">Mayor rendimiento de la API de estado (más control sobre el rendimiento)</span><span class="sxs-lookup"><span data-stu-id="f4d52-108">Higher state API throughput (more control over performance)</span></span>
 - <span data-ttu-id="f4d52-109">Latencia más baja para la distribución geográfica</span><span class="sxs-lookup"><span data-stu-id="f4d52-109">Lower-latency for geo-distribution</span></span>
 - <span data-ttu-id="f4d52-110">Control sobre dónde se almacenan los datos</span><span class="sxs-lookup"><span data-stu-id="f4d52-110">Control over where the data is stored</span></span>
 - <span data-ttu-id="f4d52-111">Acceso a los datos de estado real</span><span class="sxs-lookup"><span data-stu-id="f4d52-111">Access to the actual state data</span></span>
 - <span data-ttu-id="f4d52-112">Almacenamiento de más de 32 kb de datos.</span><span class="sxs-lookup"><span data-stu-id="f4d52-112">Store more than 32kb of data</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f4d52-113">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="f4d52-113">Prerequisites</span></span>
<span data-ttu-id="f4d52-114">Necesitará:</span><span class="sxs-lookup"><span data-stu-id="f4d52-114">You'll need:</span></span>
 - [<span data-ttu-id="f4d52-115">Cuenta de Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="f4d52-115">Microsoft Azure Account</span></span>](https://azure.microsoft.com/en-us/free/)
 - [<span data-ttu-id="f4d52-116">Visual Studio 2015 o posterior</span><span class="sxs-lookup"><span data-stu-id="f4d52-116">Visual Studio 2015 or later</span></span>](https://www.visualstudio.com/)
 - [<span data-ttu-id="f4d52-117">Paquete de NuGet en Azure para Bot Builder</span><span class="sxs-lookup"><span data-stu-id="f4d52-117">Bot Builder Azure NuGet Package</span></span>](https://www.nuget.org/packages/Microsoft.Bot.Builder.Azure/)
 - [<span data-ttu-id="f4d52-118">Paquete de NuGet Autofac Web Api2</span><span class="sxs-lookup"><span data-stu-id="f4d52-118">Autofac Web Api2 NuGet Package</span></span>](https://www.nuget.org/packages/Autofac.WebApi2/)
 - [<span data-ttu-id="f4d52-119">Bot Framework Emulator</span><span class="sxs-lookup"><span data-stu-id="f4d52-119">Bot Framework Emulator</span></span>](https://emulator.botframework.com/)
 - [<span data-ttu-id="f4d52-120">Explorador de Azure Storage</span><span class="sxs-lookup"><span data-stu-id="f4d52-120">Azure Storage Explorer</span></span>](http://storageexplorer.com/)
 
## <a name="create-azure-account"></a><span data-ttu-id="f4d52-121">Creación de una cuenta de Azure</span><span class="sxs-lookup"><span data-stu-id="f4d52-121">Create Azure account</span></span>
<span data-ttu-id="f4d52-122">Si no tiene una cuenta de Azure, haga clic [aquí](https://azure.microsoft.com/en-us/free/) para registrarse y obtener una gratuita.</span><span class="sxs-lookup"><span data-stu-id="f4d52-122">If you don't have an Azure account, click [here](https://azure.microsoft.com/en-us/free/) to sign up for a free account.</span></span>

## <a name="set-up-the-azure-table-storage-service"></a><span data-ttu-id="f4d52-123">Configuración del servicio de Azure Table Storage</span><span class="sxs-lookup"><span data-stu-id="f4d52-123">Set up the Azure Table storage service</span></span>
1. <span data-ttu-id="f4d52-124">Una vez que ha iniciado sesión en Azure Portal, haga clic en **Nuevo** para crear un servicio de Azure Table Storage.</span><span class="sxs-lookup"><span data-stu-id="f4d52-124">After you’ve logged into the Azure portal, create a new Azure Table storage service by clicking on **New**.</span></span> 
2. <span data-ttu-id="f4d52-125">Busque la **cuenta de almacenamiento** que implementa la tabla de Azure.</span><span class="sxs-lookup"><span data-stu-id="f4d52-125">Search for **Storage account** that implements the Azure Table.</span></span> 
3. <span data-ttu-id="f4d52-126">Rellene los campos, haga clic en el botón **Crear** en la parte inferior de la pantalla para implementar el nuevo servicio de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="f4d52-126">Fill in the fields, click the **Create** button at the bottom of the screen to deploy the new storage service.</span></span> <span data-ttu-id="f4d52-127">Una vez implementado el nuevo servicio de almacenamiento, mostrará las características y opciones que tiene a su disposición.</span><span class="sxs-lookup"><span data-stu-id="f4d52-127">After the new storage service is deployed, it will display features and options available to you.</span></span>
4. <span data-ttu-id="f4d52-128">Seleccione la pestaña **Claves de acceso** a la izquierda y copie la cadena de conexión para su uso posterior.</span><span class="sxs-lookup"><span data-stu-id="f4d52-128">Select the **Access keys** tab on the left, and copy the connection string for later use.</span></span> <span data-ttu-id="f4d52-129">El bot usará esta cadena de conexión para llamar al servicio de almacenamiento para guardar los datos de estado.</span><span class="sxs-lookup"><span data-stu-id="f4d52-129">Your bot will use this connection string to call the storage service to save state data.</span></span>

## <a name="install-nuget-packages"></a><span data-ttu-id="f4d52-130">Instalación de paquetes NuGet</span><span class="sxs-lookup"><span data-stu-id="f4d52-130">Install NuGet packages</span></span>
1. <span data-ttu-id="f4d52-131">Abra un proyecto de bot de C# existente o cree uno con la plantilla Bot de C# en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f4d52-131">Open an existing C# bot project, or create a new one using the C# Bot template in Visual Studio.</span></span> 
2. <span data-ttu-id="f4d52-132">Instale los siguientes paquetes NuGet:</span><span class="sxs-lookup"><span data-stu-id="f4d52-132">Install the following NuGet packages:</span></span>
   - <span data-ttu-id="f4d52-133">Microsoft.Bot.Builder.Azure</span><span class="sxs-lookup"><span data-stu-id="f4d52-133">Microsoft.Bot.Builder.Azure</span></span>
   - <span data-ttu-id="f4d52-134">Autofac.WebApi2</span><span class="sxs-lookup"><span data-stu-id="f4d52-134">Autofac.WebApi2</span></span>

## <a name="add-connection-string"></a><span data-ttu-id="f4d52-135">Agregar cadena de conexión</span><span class="sxs-lookup"><span data-stu-id="f4d52-135">Add connection string</span></span> 
<span data-ttu-id="f4d52-136">Agregue la siguiente entrada en el archivo Web.config:</span><span class="sxs-lookup"><span data-stu-id="f4d52-136">Add the following entry in your Web.config file:</span></span> 
```XML
  <connectionStrings>
    <add name="StorageConnectionString"
    connectionString="YourConnectionString"/>
  </connectionStrings>
```
<span data-ttu-id="f4d52-137">Reemplace "YourConnectionString" con la cadena de conexión a Azure Table Storage que guardó anteriormente.</span><span class="sxs-lookup"><span data-stu-id="f4d52-137">Replace "YourConnectionString" with the connection string to the Azure Table storage you saved earlier.</span></span> <span data-ttu-id="f4d52-138">Guarde el archivo Web.config.</span><span class="sxs-lookup"><span data-stu-id="f4d52-138">Save the Web.config file.</span></span>

## <a name="modify-your-bot-code"></a><span data-ttu-id="f4d52-139">Modificación del código del bot</span><span class="sxs-lookup"><span data-stu-id="f4d52-139">Modify your bot code</span></span>
<span data-ttu-id="f4d52-140">En el archivo Global.asax.cs, agregue las siguientes instrucciones `using`:</span><span class="sxs-lookup"><span data-stu-id="f4d52-140">In the Global.asax.cs file, add the following `using` statements:</span></span>
```cs
using Autofac;
using System.Configuration;
using Microsoft.Bot.Connector;
using Microsoft.Bot.Builder.Azure;
using Microsoft.Bot.Builder.Dialogs;
using Microsoft.Bot.Builder.Dialogs.Internals;
```
<span data-ttu-id="f4d52-141">En el método `Application_Start()`, cree una instancia de la clase `TableBotDataStore`.</span><span class="sxs-lookup"><span data-stu-id="f4d52-141">In the `Application_Start()` method, create an instance of the `TableBotDataStore` class.</span></span> <span data-ttu-id="f4d52-142">La clase `TableBotDataStore` implementa la interfaz `IBotDataStore<BotData>`.</span><span class="sxs-lookup"><span data-stu-id="f4d52-142">The `TableBotDataStore` class implements the `IBotDataStore<BotData>` interface.</span></span> <span data-ttu-id="f4d52-143">La interfaz `IBotDataStore` le permite invalidar la conexión predeterminada al servicio de estado de Connector.</span><span class="sxs-lookup"><span data-stu-id="f4d52-143">The `IBotDataStore` interface allows you to override the default Connector State Service connection.</span></span>
 ```cs
 var store = new TableBotDataStore(ConfigurationManager.ConnectionStrings["StorageConnectionString"].ConnectionString);
 ```
<span data-ttu-id="f4d52-144">Registre el servicio como se muestra a continuación:</span><span class="sxs-lookup"><span data-stu-id="f4d52-144">Register the service as shown below:</span></span>
 ```cs
 Conversation.UpdateContainer(
            builder =>
            {
                builder.Register(c => store)
                          .Keyed<IBotDataStore<BotData>>(AzureModule.Key_DataStore)
                          .AsSelf()
                          .SingleInstance();

                builder.Register(c => new CachingBotDataStore(store,
                           CachingBotDataStoreConsistencyPolicy
                           .ETagBasedConsistency))
                           .As<IBotDataStore<BotData>>()
                           .AsSelf()
                           .InstancePerLifetimeScope();

                
            });
 ```
<span data-ttu-id="f4d52-145">Guarde el archivo Global.asax.cs.</span><span class="sxs-lookup"><span data-stu-id="f4d52-145">Save the Global.asax.cs file.</span></span>

## <a name="run-your-bot-app"></a><span data-ttu-id="f4d52-146">Ejecución de la aplicación bot</span><span class="sxs-lookup"><span data-stu-id="f4d52-146">Run your bot app</span></span>
<span data-ttu-id="f4d52-147">Ejecute el bot en Visual Studio, el código que agregó creará la tabla **botdata** personalizada en Azure.</span><span class="sxs-lookup"><span data-stu-id="f4d52-147">Run your bot in Visual Studio, the code you added will create the custom **botdata** table in Azure.</span></span>

## <a name="connect-your-bot-to-the-emulator"></a><span data-ttu-id="f4d52-148">Conexión del bot al emulador</span><span class="sxs-lookup"><span data-stu-id="f4d52-148">Connect your bot to the emulator</span></span>
<span data-ttu-id="f4d52-149">En este momento, el bot se ejecuta de forma local.</span><span class="sxs-lookup"><span data-stu-id="f4d52-149">At this point, your bot is running locally.</span></span> <span data-ttu-id="f4d52-150">A continuación, inicie el emulador y, después, conéctese al bot en el emulador:</span><span class="sxs-lookup"><span data-stu-id="f4d52-150">Next, start the emulator and then connect to your bot in the emulator:</span></span>
1. <span data-ttu-id="f4d52-151">Escriba http://localhost:port-number/api/messages en la barra de dirección, donde port-number coincide con el número de puerto mostrado en el explorador donde se ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f4d52-151">Type http://localhost:port-number/api/messages into the address bar, where port-number matches the port number shown in the browser where your application is running.</span></span> <span data-ttu-id="f4d52-152">Por ahora puede dejar en blanco los campos <strong>Microsoft App ID</strong> (Id. de aplicación de Microsoft) y <strong>Microsoft App Password</strong> (Contraseña de aplicación de Mictosoft).</span><span class="sxs-lookup"><span data-stu-id="f4d52-152">You can leave <strong>Microsoft App ID</strong> and <strong>Microsoft App Password</strong> fields blank for now.</span></span> <span data-ttu-id="f4d52-153">Más adelante obtendrá esta información al [registrar el bot](~/bot-service-quickstart-registration.md).</span><span class="sxs-lookup"><span data-stu-id="f4d52-153">You'll get this information later when you [register your bot](~/bot-service-quickstart-registration.md).</span></span>
2. <span data-ttu-id="f4d52-154">Haga clic en **Conectar**.</span><span class="sxs-lookup"><span data-stu-id="f4d52-154">Click **Connect**.</span></span> 
3. <span data-ttu-id="f4d52-155">Para probar su bot, escriba unos cuantos mensajes en el emulador.</span><span class="sxs-lookup"><span data-stu-id="f4d52-155">Test your bot by typing a few messages in the emulator.</span></span> 

## <a name="view-data-in-azure-table-storage"></a><span data-ttu-id="f4d52-156">Visualización de datos en Azure Table Storage</span><span class="sxs-lookup"><span data-stu-id="f4d52-156">View data in Azure Table storage</span></span>
<span data-ttu-id="f4d52-157">Para ver los datos de estado, abra el **Explorador de Storage** y conéctese a Azure con sus credenciales de Azure Portal o conéctese directamente a la tabla mediante el nombre y la clave de almacenamiento y después vaya al nombre de la tabla.</span><span class="sxs-lookup"><span data-stu-id="f4d52-157">To view the state data, open **Storage Explorer** and connect to Azure using your Azure Portal credential or connect directly to the table using the storage name and storage key then navigate to your table name.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="f4d52-158">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="f4d52-158">Next steps</span></span>
<span data-ttu-id="f4d52-159">En este artículo, implementó Azure Table Storage para guardar y administrar los datos del bot.</span><span class="sxs-lookup"><span data-stu-id="f4d52-159">In this article, you implemented Azure Table storage for saving and managing your bot's data.</span></span> <span data-ttu-id="f4d52-160">A continuación, obtenga información sobre cómo modelar el flujo de la conversación con diálogos.</span><span class="sxs-lookup"><span data-stu-id="f4d52-160">Next, learn how to model conversation flow by using dialogs.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="f4d52-161">Administración de flujo de conversación</span><span class="sxs-lookup"><span data-stu-id="f4d52-161">Manage conversation flow</span></span>](bot-builder-dotnet-manage-conversation-flow.md)


## <a name="additional-resources"></a><span data-ttu-id="f4d52-162">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="f4d52-162">Additional resources</span></span>

<span data-ttu-id="f4d52-163">Si no está familiarizado con los contenedores de inversión de control y el patrón de inserción de dependencias usados en el código anterior, vaya al sitio de [Autofac](http://autofac.readthedocs.io/en/latest/) para obtener información.</span><span class="sxs-lookup"><span data-stu-id="f4d52-163">If you are unfamiliar with Inversion of Control containers and Dependency Injection pattern used in the code above, go to [Autofac](http://autofac.readthedocs.io/en/latest/) site for information.</span></span> 

<span data-ttu-id="f4d52-164">También puede descargar un ejemplo completo de [Azure Table Storage](https://github.com/Microsoft/BotBuilder-Azure/tree/master/CSharp/Samples/AzureTable) de GitHub.</span><span class="sxs-lookup"><span data-stu-id="f4d52-164">You can also download a complete [Azure Table storage](https://github.com/Microsoft/BotBuilder-Azure/tree/master/CSharp/Samples/AzureTable) sample from GitHub.</span></span>
