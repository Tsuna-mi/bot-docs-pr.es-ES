---
title: Descarga y reimplementación del código fuente de un bot | Microsoft Docs
description: Aprenda a descargar y publicar un bot de Bot Service.
keywords: descargar código fuente, volver a implementar, implementar, archivo zip, publicar
author: v-ducvo
ms.author: v-ducvo
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 03/08/2018
ms.openlocfilehash: b77e096d28f51f605db9c49d36e796553f9293ef
ms.sourcegitcommit: 1abc32353c20acd103e0383121db21b705e5eec3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/21/2018
ms.locfileid: "42756672"
---
# <a name="download-and-redeploy-bot-source-code"></a><span data-ttu-id="87897-104">Descarga y reimplementación del código fuente de un bot</span><span class="sxs-lookup"><span data-stu-id="87897-104">Download and redeploy bot source code</span></span>

<span data-ttu-id="87897-105">Bot Service le permite descargar el proyecto de código fuente completo para el bot.</span><span class="sxs-lookup"><span data-stu-id="87897-105">Bot Service allows you to download the entire source project for your bot.</span></span> <span data-ttu-id="87897-106">Así, puede trabajar en el bot localmente con el IDE que prefiera.</span><span class="sxs-lookup"><span data-stu-id="87897-106">This allows you to work on your bot locally using an IDE of your choice.</span></span> <span data-ttu-id="87897-107">Cuando haya terminado de realizar los cambios, puede publicarlos en Azure.</span><span class="sxs-lookup"><span data-stu-id="87897-107">Once you are done making changes, you can publish your changes back to Azure.</span></span> 

<span data-ttu-id="87897-108">Este tema se mostrará cómo descargar el código fuente de un bot y publicar los cambios en Azure.</span><span class="sxs-lookup"><span data-stu-id="87897-108">This topic will show you how to download your bot's source code and publishing the changes back to Azure.</span></span> 

## <a name="download-bot-source-code"></a><span data-ttu-id="87897-109">Descarga del código fuente del bot</span><span class="sxs-lookup"><span data-stu-id="87897-109">Download bot source code</span></span>

<span data-ttu-id="87897-110">Para desarrollar el bot localmente, haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="87897-110">To develop your bot locally, do the following:</span></span>

1. <span data-ttu-id="87897-111">Inicie sesión en Azure Portal y abra la hoja correspondiente al bot.</span><span class="sxs-lookup"><span data-stu-id="87897-111">In the Azure portal and open the blade for the bot.</span></span>
2. <span data-ttu-id="87897-112">En la sección **BOT MANAGEMENT** (Administración de bots), haga cic en **Build** (Compilación).</span><span class="sxs-lookup"><span data-stu-id="87897-112">Under the **BOT MANAGEMENT** section, Click **Build**.</span></span>
3. <span data-ttu-id="87897-113">Haga clic en **Descargar el archivo ZIP**.</span><span class="sxs-lookup"><span data-stu-id="87897-113">Click **Download zip file**.</span></span> 

   ![Descarga del código fuente](~/media/azure-bot-build/download-zip-file.png)

4. <span data-ttu-id="87897-115">Extraiga el archivo ZIP en un directorio local.</span><span class="sxs-lookup"><span data-stu-id="87897-115">Extract the .zip file to a local directory.</span></span>
5. <span data-ttu-id="87897-116">Vaya a la carpeta extraída y abra los archivos de origen en el IDE que prefiera.</span><span class="sxs-lookup"><span data-stu-id="87897-116">Navigate to the extracted folder and open the source files in your favorite IDE.</span></span>
6. <span data-ttu-id="87897-117">Realice los cambios en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="87897-117">Make changes to your sources.</span></span> <span data-ttu-id="87897-118">Modifique los archivos de código fuente existentes o agregue otros nuevos al proyecto.</span><span class="sxs-lookup"><span data-stu-id="87897-118">Either edit existing source files or add new ones to your project.</span></span>

<span data-ttu-id="87897-119">Cuando esté listo, puede volver a publicar los orígenes en Azure.</span><span class="sxs-lookup"><span data-stu-id="87897-119">When you are ready, you can publish the sources back to Azure.</span></span>

## <a name="publish-node-bot-source-code-to-azure"></a><span data-ttu-id="87897-120">Publicación del código fuente del bot de Node en Azure</span><span class="sxs-lookup"><span data-stu-id="87897-120">Publish Node bot source code to Azure</span></span>

<span data-ttu-id="87897-121">Para instalar estos paquetes, vaya al directorio del proyecto desde un símbolo del sistema y ejecute los siguientes comandos de NPM.</span><span class="sxs-lookup"><span data-stu-id="87897-121">To install these packages, browse to your project directory from a command prompt and run the following NPM commands.</span></span>

<span data-ttu-id="87897-122">**Nota:** estos paquetes solo deben agregarse una vez.</span><span class="sxs-lookup"><span data-stu-id="87897-122">**Note:** These packages only need to be added once.</span></span>

```console
npm install --save fs
npm install --save path
npm install --save request
npm install --save zip-folder
```

<span data-ttu-id="87897-123">Ahora está listo para publicar el proyecto en Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="87897-123">Now you are ready to publish your project to Microsoft Azure.</span></span> <span data-ttu-id="87897-124">Para publicar el proyecto en Microsoft Azure, ejecute el siguiente comando de NPM en el símbolo del sistema:</span><span class="sxs-lookup"><span data-stu-id="87897-124">To publish your project to Microsoft Azure, run the following NPM command in the cmd prompt:</span></span>

```console
npm run azure-publish
```

> [!NOTE]
> <span data-ttu-id="87897-125">Si experimenta un error después de este comando de NPM, es posible que deba agregar `"scripts": {"azure-publish": "node publish.js"}` a su archivo `package.json` y volver a ejecutarlo.</span><span class="sxs-lookup"><span data-stu-id="87897-125">If you run into an error after this NPM command you may need to add `"scripts": {"azure-publish": "node publish.js"}` to your `package.json` file and run it again.</span></span>

## <a name="publish-c-bot-source-code-to-azure"></a><span data-ttu-id="87897-126">Publicación del código fuente del bot de C# en Azure</span><span class="sxs-lookup"><span data-stu-id="87897-126">Publish C# bot source code to Azure</span></span>

<span data-ttu-id="87897-127">La publicación de código de C# en Azure con Visual Studio es un proceso de dos pasos: en primer lugar, deberá configurar la configuración de la publicación.</span><span class="sxs-lookup"><span data-stu-id="87897-127">Publishing C# code to Azure using Visual Studio is a two step process: First, you will need to configure publishing settings.</span></span> <span data-ttu-id="87897-128">Después, se publican los cambios.</span><span class="sxs-lookup"><span data-stu-id="87897-128">Then, you can publish your changes.</span></span>

<span data-ttu-id="87897-129">Para configurar la publicación desde Visual Studio, haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="87897-129">To configure publishing from Visual Studio, do the following:</span></span>

1. <span data-ttu-id="87897-130">En Visual Studio, haga clic en  **Explorador de soluciones**.</span><span class="sxs-lookup"><span data-stu-id="87897-130">In Visual Studio, click **Solution Explorer**.</span></span>
2. <span data-ttu-id="87897-131">Haga clic con el botón derecho en el nombre del proyecto y haga clic en **Publicar**. Se abre la ventana **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="87897-131">Right-click your project name and click **Publish...**. The **Publish** window opens.</span></span>
3. <span data-ttu-id="87897-132">Haga clic en **Crear nuevo perfil**, haga clic en **Importar perfil** y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="87897-132">Click **Create new profile**, click **Import profile**, and click **OK**.</span></span>
4. <span data-ttu-id="87897-133">Vaya a la carpeta del proyecto y, a continuación, vaya a la carpeta **PostDeployScripts** y seleccione el archivo que termine en **.PublishSettings**.</span><span class="sxs-lookup"><span data-stu-id="87897-133">Navigate to your project folder then navigate to the **PostDeployScripts** folder, and select the file that ends in **.PublishSettings**.</span></span> <span data-ttu-id="87897-134">Haga clic en **Abrir**.</span><span class="sxs-lookup"><span data-stu-id="87897-134">Click **Open**.</span></span>

<span data-ttu-id="87897-135">El proyecto ahora está configurado para publicar los cambios en Azure.</span><span class="sxs-lookup"><span data-stu-id="87897-135">Your project is now configured to publish changes to Azure.</span></span>

<span data-ttu-id="87897-136">Una vez que el proyecto está configurado, puede publicar el código fuente de bot en Azure mediante los pasos siguientes:</span><span class="sxs-lookup"><span data-stu-id="87897-136">Once your project is configured, you can publish your bot source code back to Azure by doing the following:</span></span>

1. <span data-ttu-id="87897-137">En Visual Studio, haga clic en  **Explorador de soluciones**.</span><span class="sxs-lookup"><span data-stu-id="87897-137">In Visual Studio, click **Solution Explorer**.</span></span>
2. <span data-ttu-id="87897-138">Haga clic con el botón derecho en el nombre del proyecto y haga clic en **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="87897-138">Right-click your project name and click **Publish...**.</span></span>
3. <span data-ttu-id="87897-139">Haga clic en el botón **Publicar** para publicar los cambios en Azure.</span><span class="sxs-lookup"><span data-stu-id="87897-139">Click the **Publish** button to publish your changes to Azure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="87897-140">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="87897-140">Next steps</span></span>
<span data-ttu-id="87897-141">Ahora que sabe cómo crear un bot localmente, puede configurar una implementación continua para el bot.</span><span class="sxs-lookup"><span data-stu-id="87897-141">Now that you know how to build your bot locally, you can setup continuous deployment for your bot.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="87897-142">Configuración de la implementación continua</span><span class="sxs-lookup"><span data-stu-id="87897-142">Set up continuous deployment</span></span>](bot-service-build-continuous-deployment.md)
