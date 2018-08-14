---
title: Migración de un bot de C# de un plan de consumo a un plan de App Service | Microsoft Docs
description: Migre un bot de Bot Service de C# de un plan de hospedaje de consumo a un plan de hospedaje de App Service.
author: v-ducvo
ms.author: v-ducvo
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ms.openlocfilehash: 1ba574de619e482b6248d33bcb805080c0ea72d0
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305104"
---
# <a name="change-the-hosting-plan-for-your-bot-service"></a><span data-ttu-id="ce32a-103">Cambio del plan de hospedaje para el servicio de bots</span><span class="sxs-lookup"><span data-stu-id="ce32a-103">Change the hosting plan for your bot service</span></span>

<span data-ttu-id="ce32a-104">En este tema se explica cómo puede migrar un bot de script de C# con un plan de consumo a un bot de C# con un plan de App Service.</span><span class="sxs-lookup"><span data-stu-id="ce32a-104">This topic explains how you can migrate a C# script bot with a Consumption plan into a C# bot with an App Service plan.</span></span> 

## <a name="advantages-of-a-bot-on-an-app-service-plan"></a><span data-ttu-id="ce32a-105">Ventajas de un bot en un plan de App Service</span><span class="sxs-lookup"><span data-stu-id="ce32a-105">Advantages of a bot on an App Service plan</span></span>

<span data-ttu-id="ce32a-106">Los bots en un plan de App Service se ejecutan como aplicaciones web de Azure.</span><span class="sxs-lookup"><span data-stu-id="ce32a-106">Bots on an App Service plan run as Azure web apps.</span></span> <span data-ttu-id="ce32a-107">Los bots de aplicación web pueden hacer cosas que los bots del plan de consumo no pueden:</span><span class="sxs-lookup"><span data-stu-id="ce32a-107">Web app bots can do things that Consumption plan bots cannot:</span></span>

- <span data-ttu-id="ce32a-108">Un bot de aplicación web puede agregar definiciones de ruta personalizadas.</span><span class="sxs-lookup"><span data-stu-id="ce32a-108">A web app bot can add custom route definitions.</span></span>
- <span data-ttu-id="ce32a-109">Un bot de aplicación web puede habilitar un servidor Websocket.</span><span class="sxs-lookup"><span data-stu-id="ce32a-109">A web app bot can enable a Websocket server.</span></span> 
- <span data-ttu-id="ce32a-110">Un bot que usa un plan de hospedaje de consumo tiene las mismas limitaciones que todo el código que se ejecuta en Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="ce32a-110">A bot that uses a Consumption hosting plan has the same limitations as all code running on Azure Functions.</span></span> <span data-ttu-id="ce32a-111">Para obtener más información, consulte <a target='_blank' href='/azure/azure-functions/functions-scale'>Plan de consumo y plan de App Service de Azure Functions</a>.</span><span class="sxs-lookup"><span data-stu-id="ce32a-111">For more information, see <a target='_blank' href='/azure/azure-functions/functions-scale'>Azure Functions Consumption and App Service plans</a>.</span></span>

## <a name="download-your-existing-bot-source"></a><span data-ttu-id="ce32a-112">Descarga del origen del bot existente</span><span class="sxs-lookup"><span data-stu-id="ce32a-112">Download your existing bot source</span></span>

<span data-ttu-id="ce32a-113">Siga estos pasos para descargar el código fuente de su bot existente:</span><span class="sxs-lookup"><span data-stu-id="ce32a-113">Follow these steps to download the source code of your existing bot:</span></span>

1. <span data-ttu-id="ce32a-114">Dentro del bot de Azure, haga clic en la pestaña **Configuración** y expanda la sección **Implementación continua**.</span><span class="sxs-lookup"><span data-stu-id="ce32a-114">Within your Azure bot, click the **Settings** tab and expand the **Continuous deployment** section.</span></span>  
2. <span data-ttu-id="ce32a-115">Haga clic en el botón azul para descargar el archivo ZIP que contiene el código fuente del bot.</span><span class="sxs-lookup"><span data-stu-id="ce32a-115">Click the blue button to download the zip file that contains the source code for your bot.</span></span>  
    ![Descarga del archivo ZIP del bot](~/media/continuous-deployment-consumption-download.png)
3. <span data-ttu-id="ce32a-117">Extraiga en una carpeta local el contenido del archivo ZIP descargado.</span><span class="sxs-lookup"><span data-stu-id="ce32a-117">Extract the contents of the downloaded zip file to a local folder.</span></span> 


## <a name="create-a-bot-template"></a><span data-ttu-id="ce32a-118">Creación de una plantilla de bot</span><span class="sxs-lookup"><span data-stu-id="ce32a-118">Create a bot template</span></span>

<span data-ttu-id="ce32a-119">Bot Service tiene las mismas plantillas para los bots del plan de consumo y el plan de App Service.</span><span class="sxs-lookup"><span data-stu-id="ce32a-119">Bot Service has the same templates for Consumption plan and App Service plan bots.</span></span> <span data-ttu-id="ce32a-120">Para migrar de un bot de plan de consumo, cree un nuevo bot de plan de App Service en Bot Service, basado en la misma plantilla.</span><span class="sxs-lookup"><span data-stu-id="ce32a-120">To migrate from a Consumption plan bot, create a new App Service plan bot in Bot Service, based on the same template.</span></span> <span data-ttu-id="ce32a-121">El código subyacente puede diferir entre los dos tipos de hospedaje para la misma plantilla, pero la nueva aplicación web tiene una estructura y características de configuración similares que las que usa el bot existente.</span><span class="sxs-lookup"><span data-stu-id="ce32a-121">Underlying code can differ between the two hosting types for the same template, but the new web app has similar structure and configuration features that your existing bot uses.</span></span>

## <a name="download-the-new-bot-source"></a><span data-ttu-id="ce32a-122">Descarga del origen del nuevo bot</span><span class="sxs-lookup"><span data-stu-id="ce32a-122">Download the new bot source</span></span>

<span data-ttu-id="ce32a-123">Siga estos pasos para descargar el código fuente del nuevo bot:</span><span class="sxs-lookup"><span data-stu-id="ce32a-123">Follow these steps to download the source code of the new bot:</span></span>

1. <span data-ttu-id="ce32a-124">Dentro de un bot de Azure, haga clic en la pestaña **BUILD** (Compilar), busque la sección **Download source code** (Descargar código fuente) y haga clic en **Descargar archivo ZIP**.</span><span class="sxs-lookup"><span data-stu-id="ce32a-124">Within your Azure bot, click the **BUILD** tab, find the **Download source code** section, and click **Download zip file**.</span></span> 
2. <span data-ttu-id="ce32a-125">Extraiga en la carpeta local el contenido del archivo ZIP descargado.</span><span class="sxs-lookup"><span data-stu-id="ce32a-125">Extract the contents of the downloaded zip file to the local folder.</span></span>

## <a name="add-source-files-to-new-solution"></a><span data-ttu-id="ce32a-126">Adición de archivos de origen a la nueva solución</span><span class="sxs-lookup"><span data-stu-id="ce32a-126">Add source files to new solution</span></span>

<span data-ttu-id="ce32a-127">Algunos archivos .csx podrían compilarse y ejecutarse como archivos .cs en la nueva solución.</span><span class="sxs-lookup"><span data-stu-id="ce32a-127">Some .csx files might compile and run as .cs files in the new solution.</span></span> <span data-ttu-id="ce32a-128">Crear un archivo .cs para cada archivo .csx de la solución, excepto `run.csx`.</span><span class="sxs-lookup"><span data-stu-id="ce32a-128">Create a .cs file for each .csx file in your solution, except `run.csx`.</span></span> <span data-ttu-id="ce32a-129">Migrará la lógica `run.csx` a mano.</span><span class="sxs-lookup"><span data-stu-id="ce32a-129">You will migrate `run.csx` logic by hand.</span></span> <span data-ttu-id="ce32a-130">En los archivos. cs, debe agregar una declaración de clase y la una declaración de espacio de nombres opcional.</span><span class="sxs-lookup"><span data-stu-id="ce32a-130">In your .cs files, you may need to add a class declaration and optional namespace declaration.</span></span>

## <a name="migrate-runcsx-logic-into-your-project"></a><span data-ttu-id="ce32a-131">Migración de la lógica de run.csx al proyecto</span><span class="sxs-lookup"><span data-stu-id="ce32a-131">Migrate run.csx logic into your project</span></span>

<span data-ttu-id="ce32a-132">Los proyectos de script de C# tienen un método `Run`donde se controlan diferentes valores de `ActivityTypes`.</span><span class="sxs-lookup"><span data-stu-id="ce32a-132">C# script projects have a `Run` method where different `ActivityTypes` values are handled.</span></span> <span data-ttu-id="ce32a-133">Importe la lógica de control de actividades al método `MessageController.Post`, en `MessageController.cs`.</span><span class="sxs-lookup"><span data-stu-id="ce32a-133">Import your activity handling logic into the `MessageController.Post` method, in `MessageController.cs`.</span></span>

## <a name="remove-compiler-keywords"></a><span data-ttu-id="ce32a-134">Eliminación de palabras clave del compilador</span><span class="sxs-lookup"><span data-stu-id="ce32a-134">Remove compiler keywords</span></span>

<span data-ttu-id="ce32a-135">Los archivos de script de C# pueden incluir un módulo de referencia con la palabra clave `#r`.</span><span class="sxs-lookup"><span data-stu-id="ce32a-135">C# script files can include a reference module using the `#r` keyword.</span></span> <span data-ttu-id="ce32a-136">Quite estas líneas y, además, agréguelas como referencias al proyecto de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ce32a-136">Remove these lines, and also add these as references to your Visual Studio project.</span></span> <span data-ttu-id="ce32a-137">Además, quitar las palabra clave `#load`, que insertan otros archivos de código fuente en la compilación de archivos.</span><span class="sxs-lookup"><span data-stu-id="ce32a-137">Also remove `#load` keywords, which insert other source code files in a file compilation.</span></span> <span data-ttu-id="ce32a-138">En su lugar, agregue todos los archivos `.csx` al proyecto como código fuente `.cs`.</span><span class="sxs-lookup"><span data-stu-id="ce32a-138">Instead, add all `.csx` files as to your project as `.cs` source code.</span></span>

## <a name="add-references-from-projectjson"></a><span data-ttu-id="ce32a-139">Adición de referencias de project.json</span><span class="sxs-lookup"><span data-stu-id="ce32a-139">Add references from project.json</span></span>

<span data-ttu-id="ce32a-140">Si su bot de plan de consumo agrega referencias de NuGet a su archivo `project.json`, agregue estas referencias a la nueva solución de Visual Studio haciendo clic con el botón derecho en el proyecto en el panel Explorador de soluciones y haciendo clic en **Agregar referencia**.</span><span class="sxs-lookup"><span data-stu-id="ce32a-140">If your Consumption plan bot adds NuGet references in its `project.json` file, add these references to your new Visual Studio solution by right-clicking the project in the Solution Explorer pane, and clicking **Add Reference**.</span></span>

### <a name="add-references-that-were-implicit"></a><span data-ttu-id="ce32a-141">Adición de referencias que eran implícitas</span><span class="sxs-lookup"><span data-stu-id="ce32a-141">Add references that were implicit</span></span>

<span data-ttu-id="ce32a-142">Un bot de Bot Service en un plan de consumo incluye implícitamente estas referencias en todos los archivos de origen .csx.</span><span class="sxs-lookup"><span data-stu-id="ce32a-142">A Bot Service bot on a Consumption plan implicitly includes these references in all .csx source files.</span></span> <span data-ttu-id="ce32a-143">Es posible que el origen migrado a los archivos de origen .cs archivos tengan que agregar referencias explícitas para estas clases:</span><span class="sxs-lookup"><span data-stu-id="ce32a-143">Source migrated into .cs source files may need to add explicit references for these classes:</span></span>

- <span data-ttu-id="ce32a-144">`TraceWriter` en el paquete NuGet `Microsoft.Azure.WebJobs` que proporciona los tipos de espacio de nombres `Microsoft.Azure.WebJobs.Host`.</span><span class="sxs-lookup"><span data-stu-id="ce32a-144">`TraceWriter` in the NuGet package `Microsoft.Azure.WebJobs` that provides the `Microsoft.Azure.WebJobs.Host` namespace types.</span></span> 
- <span data-ttu-id="ce32a-145">Desencadenadores de temporizador en el paquete de NuGet `Microsoft.Azure.WebJobs.Extensions`</span><span class="sxs-lookup"><span data-stu-id="ce32a-145">timer triggers in the NuGet package `Microsoft.Azure.WebJobs.Extensions`</span></span>
- <span data-ttu-id="ce32a-146">`Newtonsoft.Json`, `Microsoft.ServiceBus` y otros ensamblados a los que se hace referencia automáticamente</span><span class="sxs-lookup"><span data-stu-id="ce32a-146">`Newtonsoft.Json`, `Microsoft.ServiceBus`, and other automatically referenced assemblies</span></span>
- <span data-ttu-id="ce32a-147">`System.Threading.Tasks` y otros espacios de nombres importados automáticamente</span><span class="sxs-lookup"><span data-stu-id="ce32a-147">`System.Threading.Tasks` and other automatically imported namespaces</span></span>

<span data-ttu-id="ce32a-148">Para obtener más información, consulte *Converting to class files* (Conversión a archivos de clase) en <a target='_blank' href='https://blogs.msdn.microsoft.com/appserviceteam/2017/03/16/publishing-a-net-class-library-as-a-function-app/'>Publishing a .NET class library as a Function App</a> (Publicación de una biblioteca de clases .NET como Function App).</span><span class="sxs-lookup"><span data-stu-id="ce32a-148">For additional guidance, see *Converting to class files* in <a target='_blank' href='https://blogs.msdn.microsoft.com/appserviceteam/2017/03/16/publishing-a-net-class-library-as-a-function-app/'>Publishing a .NET class library as a Function App</a>.</span></span>

## <a name="debug-your-new-bot"></a><span data-ttu-id="ce32a-149">Depuración del nuevo bot</span><span class="sxs-lookup"><span data-stu-id="ce32a-149">Debug your new bot</span></span>

<span data-ttu-id="ce32a-150">Es mucho más sencillo depurar localmente un bot en un plan de App Service que un bot en un plan de consumo.</span><span class="sxs-lookup"><span data-stu-id="ce32a-150">A bot on an App Service plan is much easier than a bot on a Consumption plan to debug locally.</span></span> <span data-ttu-id="ce32a-151">Puede usar el [emulador](bot-service-debug-emulator.md) para depurar el código migrado localmente.</span><span class="sxs-lookup"><span data-stu-id="ce32a-151">You can use [the emulator](bot-service-debug-emulator.md) to debug your migrated code locally.</span></span>

## <a name="publish-from-visual-studio-or-set-up-continuous-deployment"></a><span data-ttu-id="ce32a-152">Publicación desde Visual Studio, o configuración de una implementación continua</span><span class="sxs-lookup"><span data-stu-id="ce32a-152">Publish from Visual Studio, or set up continuous deployment</span></span>

<span data-ttu-id="ce32a-153">Por último, publique el código fuente migrado a Bot Service ya sea importando el archivo `.PublishSettings` y haciendo clic en **Publicar**, o [configurando la implementación continua](bot-service-debug-bot.md).</span><span class="sxs-lookup"><span data-stu-id="ce32a-153">Finally, publish your migrated source code to Bot Service either by importing its `.PublishSettings` file and clicking **Publish**, or by [setting up continuous deployment](bot-service-debug-bot.md).</span></span>
