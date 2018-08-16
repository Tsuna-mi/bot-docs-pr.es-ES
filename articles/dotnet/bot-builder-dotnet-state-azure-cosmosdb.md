---
title: Administración de datos de estado personalizado con Azure Cosmos DB | Microsoft Docs
description: Aprenda a guardar y a recuperar datos de estado mediante Azure Cosmos DB con Bot Builder SDK para Node.js.
author: kaiqb
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: b99dc4cd9011871d52479ade92968ebb29c8c73f
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305977"
---
# <a name="manage-custom-state-data-with-azure-cosmos-db-for-net"></a><span data-ttu-id="ec1b9-103">Administración de datos de estado personalizado con Azure Cosmos DB para Node.js</span><span class="sxs-lookup"><span data-stu-id="ec1b9-103">Manage custom state data with Azure Cosmos DB for .NET</span></span>
<span data-ttu-id="ec1b9-104">En este artículo, implementará Azure Cosmos DB para almacenar y administrar los datos de estado del bot.</span><span class="sxs-lookup"><span data-stu-id="ec1b9-104">In this article, you’ll implement Azure Cosmos DB to store and manage your bot’s state data.</span></span> <span data-ttu-id="ec1b9-105">El servicio de estado de Connector predeterminado que usan los bots no es adecuado para el entorno de producción.</span><span class="sxs-lookup"><span data-stu-id="ec1b9-105">The default Connector State Service used by bots is not intended for the production environment.</span></span> <span data-ttu-id="ec1b9-106">Usará las [extensiones de Azure](https://github.com/Microsoft/BotBuilder-Azure) disponibles en GitHub o implementará un cliente de estado personalizado mediante la plataforma de almacenamiento de datos de su elección.</span><span class="sxs-lookup"><span data-stu-id="ec1b9-106">You should either use [Azure Extensions](https://github.com/Microsoft/BotBuilder-Azure) available on GitHub or implement a custom state client using data storage platform of your choice.</span></span> <span data-ttu-id="ec1b9-107">Estas son algunas de las razones para usar el almacenamiento de estado personalizado:</span><span class="sxs-lookup"><span data-stu-id="ec1b9-107">Here are some of the reasons to use custom state storage:</span></span>
 - <span data-ttu-id="ec1b9-108">Mayor rendimiento de la API de estado (más control sobre el rendimiento)</span><span class="sxs-lookup"><span data-stu-id="ec1b9-108">Higher state API throughput (more control over performance)</span></span>
 - <span data-ttu-id="ec1b9-109">Latencia más baja para la distribución geográfica</span><span class="sxs-lookup"><span data-stu-id="ec1b9-109">Lower-latency for geo-distribution</span></span>
 - <span data-ttu-id="ec1b9-110">Control sobre dónde se almacenan los datos</span><span class="sxs-lookup"><span data-stu-id="ec1b9-110">Control over where the data is stored</span></span>
 - <span data-ttu-id="ec1b9-111">Acceso a los datos de estado reales</span><span class="sxs-lookup"><span data-stu-id="ec1b9-111">Access to the actual state data</span></span>
 - <span data-ttu-id="ec1b9-112">Almacenamiento de más de 32 kb de datos.</span><span class="sxs-lookup"><span data-stu-id="ec1b9-112">Store more than 32kb of data</span></span>
 
## <a name="prerequisites"></a><span data-ttu-id="ec1b9-113">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="ec1b9-113">Prerequisites</span></span>
<span data-ttu-id="ec1b9-114">Necesitará:</span><span class="sxs-lookup"><span data-stu-id="ec1b9-114">You'll need:</span></span>
 - [<span data-ttu-id="ec1b9-115">Cuenta de Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="ec1b9-115">Microsoft Azure Account</span></span>](https://azure.microsoft.com/en-us/free/)
 - [<span data-ttu-id="ec1b9-116">Visual Studio 2015 o posterior</span><span class="sxs-lookup"><span data-stu-id="ec1b9-116">Visual Studio 2015 or later</span></span>](https://www.visualstudio.com/)
 - [<span data-ttu-id="ec1b9-117">Paquete NuGet de Azure para Bot Builder</span><span class="sxs-lookup"><span data-stu-id="ec1b9-117">Bot Builder Azure NuGet Package</span></span>](https://www.nuget.org/packages/Microsoft.Bot.Builder.Azure/)
 - [<span data-ttu-id="ec1b9-118">Paquete NuGet Autofac Web Api2</span><span class="sxs-lookup"><span data-stu-id="ec1b9-118">Autofac Web Api2 NuGet Package</span></span>](https://www.nuget.org/packages/Autofac.WebApi2/)
 - [<span data-ttu-id="ec1b9-119">Bot Framework Emulator</span><span class="sxs-lookup"><span data-stu-id="ec1b9-119">Bot Framework Emulator</span></span>](~/bot-service-debug-emulator.md)
 
## <a name="create-azure-account"></a><span data-ttu-id="ec1b9-120">Creación de una cuenta de Azure</span><span class="sxs-lookup"><span data-stu-id="ec1b9-120">Create Azure account</span></span>
<span data-ttu-id="ec1b9-121">Si no tiene una cuenta de Azure, haga clic [aquí](https://azure.microsoft.com/en-us/free/) para registrarse y obtener una gratuita.</span><span class="sxs-lookup"><span data-stu-id="ec1b9-121">If you don't have an Azure account, click [here](https://azure.microsoft.com/en-us/free/) to sign up for a free account.</span></span>

## <a name="set-up-the-azure-cosmos-db-database"></a><span data-ttu-id="ec1b9-122">Configuración de la base de datos de Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="ec1b9-122">Set up the Azure Cosmos DB database</span></span>
1. <span data-ttu-id="ec1b9-123">Una vez que ha iniciado sesión en Azure Portal, haga clic en *Nuevo* para crear una base de datos de **Azure Cosmos DB**.</span><span class="sxs-lookup"><span data-stu-id="ec1b9-123">After you’ve logged into the Azure portal, create a new *Azure Cosmos DB* database by clicking **New**.</span></span> 
2. <span data-ttu-id="ec1b9-124">Haga clic en **Bases de datos**.</span><span class="sxs-lookup"><span data-stu-id="ec1b9-124">Click **Databases**.</span></span> 
3. <span data-ttu-id="ec1b9-125">Busque **Azure Cosmos DB** y haga clic en **Crear**.</span><span class="sxs-lookup"><span data-stu-id="ec1b9-125">Find **Azure Cosmos DB** and click **Create**.</span></span>
4. <span data-ttu-id="ec1b9-126">Rellene los campos.</span><span class="sxs-lookup"><span data-stu-id="ec1b9-126">Fill in the fields.</span></span> <span data-ttu-id="ec1b9-127">Para el campo **API**, seleccione **SQL (DocumentDB)**.</span><span class="sxs-lookup"><span data-stu-id="ec1b9-127">For the **API** field, select **SQL (DocumentDB)**.</span></span> <span data-ttu-id="ec1b9-128">Una vez rellenados todos los campos, haga clic en el botón **Crear** en la parte inferior de la pantalla para implementar la nueva base de datos.</span><span class="sxs-lookup"><span data-stu-id="ec1b9-128">When done filling in all the fields, click the **Create** button at the bottom of the screen to deploy the new database.</span></span> 
5. <span data-ttu-id="ec1b9-129">Tras la implementación de la nueva base de datos, vaya a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="ec1b9-129">After the new database is deployed, navigate to your new database.</span></span> <span data-ttu-id="ec1b9-130">Haga clic en **Claves de acceso** para encontrar las claves y las cadenas de conexión.</span><span class="sxs-lookup"><span data-stu-id="ec1b9-130">Click **Access keys** to find keys and connection strings.</span></span> <span data-ttu-id="ec1b9-131">El bot usará esta información para llamar al servicio de almacenamiento para guardar los datos de estado.</span><span class="sxs-lookup"><span data-stu-id="ec1b9-131">Your bot will use this information to call the storage service to save state data.</span></span>

## <a name="install-nuget-packages"></a><span data-ttu-id="ec1b9-132">Instalación de paquetes NuGet</span><span class="sxs-lookup"><span data-stu-id="ec1b9-132">Install NuGet packages</span></span>
1. <span data-ttu-id="ec1b9-133">Abra un proyecto de bot de C# existente o cree uno con la plantilla Bot en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ec1b9-133">Open an existing C# bot project, or create a new one using the Bot template in Visual Studio.</span></span> 
2. <span data-ttu-id="ec1b9-134">Instale los siguientes paquetes NuGet:</span><span class="sxs-lookup"><span data-stu-id="ec1b9-134">Install the following NuGet packages:</span></span>
   - <span data-ttu-id="ec1b9-135">Microsoft.Bot.Builder.Azure</span><span class="sxs-lookup"><span data-stu-id="ec1b9-135">Microsoft.Bot.Builder.Azure</span></span>
   - <span data-ttu-id="ec1b9-136">Autofac.WebApi2</span><span class="sxs-lookup"><span data-stu-id="ec1b9-136">Autofac.WebApi2</span></span>

## <a name="add-connection-string"></a><span data-ttu-id="ec1b9-137">Agregar cadena de conexión</span><span class="sxs-lookup"><span data-stu-id="ec1b9-137">Add connection string</span></span> 
<span data-ttu-id="ec1b9-138">Agregue las siguientes entradas al archivo Web.config:</span><span class="sxs-lookup"><span data-stu-id="ec1b9-138">Add the following entries into the Web.config file:</span></span>
```XML
<add key="DocumentDbUrl" value="Your DocumentDB URI"/>
<add key="DocumentDbKey" value="Your DocumentDB Key"/>
```
<span data-ttu-id="ec1b9-139">Reemplazará el valor por el URI y la clave principal encontrados en su instancia de Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="ec1b9-139">You'll replace the value with your URI and Primary Key found in your Azure Cosmos DB.</span></span> <span data-ttu-id="ec1b9-140">Guarde el archivo Web.config.</span><span class="sxs-lookup"><span data-stu-id="ec1b9-140">Save the Web.config file.</span></span>

## <a name="modify-your-bot-code"></a><span data-ttu-id="ec1b9-141">Modificación del código de bot</span><span class="sxs-lookup"><span data-stu-id="ec1b9-141">Modify your bot code</span></span>
<span data-ttu-id="ec1b9-142">Para usar el almacenamiento de **Azure Cosmos DB**, agregue las siguientes líneas de código al archivo **Global.asax.cs** de su bot dentro del método **Application_Start ()**.</span><span class="sxs-lookup"><span data-stu-id="ec1b9-142">To use **Azure Cosmos DB** storage, add the following lines of code to your bot's **Global.asax.cs** file inside the **Application_Start()** method.</span></span>

```cs
using System;
using Autofac;
using System.Web.Http;
using System.Configuration;
using Microsoft.Bot.Connector;
using Microsoft.Bot.Builder.Azure;
using Microsoft.Bot.Builder.Dialogs;
using Microsoft.Bot.Builder.Dialogs.Internals;

namespace SampleApp
{
    public class WebApiApplication : System.Web.HttpApplication
    {
        protected void Application_Start()
        {
            var uri = new Uri(ConfigurationManager.AppSettings["DocumentDbUrl"]);
            var key = ConfigurationManager.AppSettings["DocumentDbKey"];
            var store = new DocumentDbBotDataStore(uri, key);

            Conversation.UpdateContainer(
                        builder =>
                        {
                            builder.Register(c => store)
                                .Keyed<IBotDataStore<BotData>>(AzureModule.Key_DataStore)
                                .AsSelf()
                                .SingleInstance();

                            builder.Register(c => new CachingBotDataStore(store, CachingBotDataStoreConsistencyPolicy.ETagBasedConsistency))
                                .As<IBotDataStore<BotData>>()
                                .AsSelf()
                                .InstancePerLifetimeScope();

                        });

        }
    }
}
```

<span data-ttu-id="ec1b9-143">Guarde el archivo Global.asax.cs.</span><span class="sxs-lookup"><span data-stu-id="ec1b9-143">Save the global.asax.cs file.</span></span> <span data-ttu-id="ec1b9-144">Ahora está listo para probar el bot con el emulador.</span><span class="sxs-lookup"><span data-stu-id="ec1b9-144">Now you are ready to test the bot with the emulator.</span></span>

## <a name="run-your-bot-app"></a><span data-ttu-id="ec1b9-145">Ejecución de la aplicación bot</span><span class="sxs-lookup"><span data-stu-id="ec1b9-145">Run your bot app</span></span>
<span data-ttu-id="ec1b9-146">Ejecute el bot en Visual Studio; el código que agregó creará la tabla **botdata** personalizada en Azure.</span><span class="sxs-lookup"><span data-stu-id="ec1b9-146">Run your bot in Visual Studio, the code you added will create the custom **botdata** table in Azure.</span></span>

## <a name="connect-your-bot-to-the-emulator"></a><span data-ttu-id="ec1b9-147">Conexión del bot al emulador</span><span class="sxs-lookup"><span data-stu-id="ec1b9-147">Connect your bot to the emulator</span></span>
<span data-ttu-id="ec1b9-148">En este momento, el bot se ejecuta de forma local.</span><span class="sxs-lookup"><span data-stu-id="ec1b9-148">At this point, your bot is running locally.</span></span> <span data-ttu-id="ec1b9-149">A continuación, inicie el emulador y, después, conéctese al bot en el emulador:</span><span class="sxs-lookup"><span data-stu-id="ec1b9-149">Next, start the emulator and then connect to your bot in the emulator:</span></span>
1. <span data-ttu-id="ec1b9-150">Escriba http://localhost:port-number/api/messages en la barra de dirección, donde port-number coincide con el número de puerto mostrado en el explorador donde se ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ec1b9-150">Type http://localhost:port-number/api/messages into the address bar, where port-number matches the port number shown in the browser where your application is running.</span></span> <span data-ttu-id="ec1b9-151">Por ahora puede dejar en blanco los campos <strong>Microsoft App ID</strong> (Id. de aplicación de Microsoft) y <strong>Microsoft App Password</strong> (Contraseña de aplicación de Microsoft).</span><span class="sxs-lookup"><span data-stu-id="ec1b9-151">You can leave <strong>Microsoft App ID</strong> and <strong>Microsoft App Password</strong> fields blank for now.</span></span> <span data-ttu-id="ec1b9-152">Más adelante obtendrá esta información al [registrar el bot](~/bot-service-quickstart-registration.md).</span><span class="sxs-lookup"><span data-stu-id="ec1b9-152">You'll get this information later when you [register your bot](~/bot-service-quickstart-registration.md).</span></span>
2. <span data-ttu-id="ec1b9-153">Haga clic en **Conectar**.</span><span class="sxs-lookup"><span data-stu-id="ec1b9-153">Click **Connect**.</span></span> 
3. <span data-ttu-id="ec1b9-154">Para probar su bot, escriba unos cuantos mensajes en el emulador.</span><span class="sxs-lookup"><span data-stu-id="ec1b9-154">Test your bot by typing a few messages in the emulator.</span></span> 

## <a name="view-state-data-on-azure-portal"></a><span data-ttu-id="ec1b9-155">Vista de los datos de estado en Azure Portal</span><span class="sxs-lookup"><span data-stu-id="ec1b9-155">View state data on Azure Portal</span></span>
<span data-ttu-id="ec1b9-156">Para ver los datos de estado, inicie sesión en Azure Portal y vaya a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="ec1b9-156">To view the state data, sign into your Azure portal and navigate to your database.</span></span> <span data-ttu-id="ec1b9-157">Haga clic en **Explorador de datos (versión preliminar)** para verificar que se está guardando la información de estado del bot.</span><span class="sxs-lookup"><span data-stu-id="ec1b9-157">Click **Data Explorer (preview)** to verify that the state information from your bot is being saved.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="ec1b9-158">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="ec1b9-158">Next steps</span></span>
<span data-ttu-id="ec1b9-159">En este artículo, ha usado Cosmos DB para guardar y administrar datos del bot.</span><span class="sxs-lookup"><span data-stu-id="ec1b9-159">In this article, you used Cosmos DB for saving and managing your bot's data.</span></span> <span data-ttu-id="ec1b9-160">A continuación, aprenda cómo modelar el flujo de la conversación con diálogos.</span><span class="sxs-lookup"><span data-stu-id="ec1b9-160">Next, learn how to model conversation flow by using dialogs.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="ec1b9-161">Administración de flujo de conversación</span><span class="sxs-lookup"><span data-stu-id="ec1b9-161">Manage conversation flow</span></span>](bot-builder-dotnet-manage-conversation-flow.md)

## <a name="additional-resources"></a><span data-ttu-id="ec1b9-162">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="ec1b9-162">Additional resources</span></span>
<span data-ttu-id="ec1b9-163">Si no está familiarizado con los contenedores de inversión de control y el patrón de inserción de dependencias usados en el código anterior, visite el sitio de [Autofac](http://autofac.readthedocs.io/en/latest/) para obtener información.</span><span class="sxs-lookup"><span data-stu-id="ec1b9-163">If you are unfamiliar with Inversion of Control containers and Dependency Injection pattern used in the code above, visit [Autofac](http://autofac.readthedocs.io/en/latest/) site for information.</span></span> 

<span data-ttu-id="ec1b9-164">También puede descargar un [ejemplo](https://github.com/Microsoft/BotBuilder-Azure/tree/master/CSharp/Samples/DocumentDb) de GitHub para saber más sobre el uso de Cosmos DB para administrar el estado.</span><span class="sxs-lookup"><span data-stu-id="ec1b9-164">You can also download a [sample](https://github.com/Microsoft/BotBuilder-Azure/tree/master/CSharp/Samples/DocumentDb) from GitHub to learn more about using Cosmos DB for managing state.</span></span> 
