---
title: Compilación de un bot con el editor de código en línea de Azure | Microsoft Docs
description: Obtenga información sobre cómo compilar el bot mediante el editor de código en línea en Bot Service.
keywords: editor de código en línea, azure portal, bot de functions
author: v-ducvo
ms.author: v-ducvo
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 09/21/2018
ms.openlocfilehash: 00342b624c43b8eea5bbc2bdd828703eafa5ef63
ms.sourcegitcommit: 3bf3dbb1a440b3d83e58499c6a2ac116fe04b2f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/23/2018
ms.locfileid: "46707861"
---
# <a name="edit-a-bot-with-online-code-editor"></a><span data-ttu-id="de63f-104">Edición de un bot con el editor de código en línea</span><span class="sxs-lookup"><span data-stu-id="de63f-104">Edit a bot with online code editor</span></span>

<span data-ttu-id="de63f-105">Puede usar el editor de código en línea para compilar el bot sin necesidad de un IDE.</span><span class="sxs-lookup"><span data-stu-id="de63f-105">You can use the online code editor to build your bot without needing an IDE.</span></span> <span data-ttu-id="de63f-106">En este tema se mostrará cómo abrir el código del bot en el editor de código en línea.</span><span class="sxs-lookup"><span data-stu-id="de63f-106">This topic will show you how to open your bot code in the online code editor.</span></span> 

## <a name="edit-bot-source-code-in-online-code-editor"></a><span data-ttu-id="de63f-107">Edición del código fuente del bot en el editor de código en línea</span><span class="sxs-lookup"><span data-stu-id="de63f-107">Edit bot source code in online code editor</span></span>

<span data-ttu-id="de63f-108">Para editar el código fuente de un bot en el editor de código en línea, haga lo siguiente para el tipo específico de bot que tiene.</span><span class="sxs-lookup"><span data-stu-id="de63f-108">To edit a bot's source code in the online code editor, do the following for the specific type of bot you have.</span></span>

### <a name="web-app-bot"></a><span data-ttu-id="de63f-109">Bot de aplicación web</span><span class="sxs-lookup"><span data-stu-id="de63f-109">Web App Bot</span></span>
1. <span data-ttu-id="de63f-110">Inicie sesión en [Azure Portal](http://portal.azure.com) y abra la hoja para el bot.</span><span class="sxs-lookup"><span data-stu-id="de63f-110">Sign into the [Azure portal](http://portal.azure.com) and open the blade for the bot.</span></span>
2. <span data-ttu-id="de63f-111">En la sección **Administración de bots**, haga clic en **Crear**.</span><span class="sxs-lookup"><span data-stu-id="de63f-111">Under the **Bot Management** section, Click **Build**.</span></span>
3. <span data-ttu-id="de63f-112">Haga clic en **Open online code editor** (Abrir el editor de código en línea).</span><span class="sxs-lookup"><span data-stu-id="de63f-112">Click **Open online code editor**.</span></span> <span data-ttu-id="de63f-113">El código del bot se abrirá en una nueva ventana del explorador.</span><span class="sxs-lookup"><span data-stu-id="de63f-113">This will open the bot's code in a new browser window.</span></span> 

   ![Abrir el editor de código en línea](~/media/azure-bot-build/open-online-code-editor.png)

   <span data-ttu-id="de63f-115">La estructura del archivo en el directorio **WWWRoot** será distinta en función del lenguaje del bot.</span><span class="sxs-lookup"><span data-stu-id="de63f-115">Depending on the language of the bot, the file structure under the **WWWRoot** directory will be different.</span></span> <span data-ttu-id="de63f-116">Por ejemplo, si tiene un bot de C#, es posible que el directorio **WWWRoot** tenga este aspecto:</span><span class="sxs-lookup"><span data-stu-id="de63f-116">For example, if you have a C# bot, your **WWWRoot** may look something like this:</span></span>

   ![Estructura de archivos de C#](~/media/azure-bot-build/cs-wwwroot-structure.png)

   <span data-ttu-id="de63f-118">Si tiene un bot de Node.js, es posible que el directorio **WWWRoot** tenga este aspecto:</span><span class="sxs-lookup"><span data-stu-id="de63f-118">If you have a Node.js bot, your **WWWRoot** may look something like this:</span></span>

   ![Estructura de archivos de Node.js](~/media/azure-bot-build/node-wwwroot-structure.png)

4. <span data-ttu-id="de63f-120">Modifique el código.</span><span class="sxs-lookup"><span data-stu-id="de63f-120">Make code changes.</span></span> <span data-ttu-id="de63f-121">Por ejemplo, para bots de C#, tiene un archivo .cs.</span><span class="sxs-lookup"><span data-stu-id="de63f-121">For example, for C# bots, you can a .cs file.</span></span> <span data-ttu-id="de63f-122">En el caso de los bots de Node.js, puede empezar con el archivo App.js.</span><span class="sxs-lookup"><span data-stu-id="de63f-122">For Node.js bots, you can start with the App.js file.</span></span>

   > [!NOTE]
   > <span data-ttu-id="de63f-123">Si bien puede modificar el código de los archivos de origen actuales del proyecto, no es posible crear archivos de origen nuevos con el editor de código en línea.</span><span class="sxs-lookup"><span data-stu-id="de63f-123">While you can make code changes to current source files in the project, it is not possible to create new source files using the online code editor.</span></span> <span data-ttu-id="de63f-124">Para agregar nuevos archivos de origen al bot, deberá [descargar el proyecto de origen](bot-service-build-download-source-code.md), agregar los archivos y volver a publicar los cambios en Azure.</span><span class="sxs-lookup"><span data-stu-id="de63f-124">To add new source files to the bot, you need to [download the source](bot-service-build-download-source-code.md) project, add your files and publish the changes back to Azure.</span></span>

5. <span data-ttu-id="de63f-125">Para los bots de C# que se encuentran en un **plan de consumo** y todos los bots de Node.js, el bot se guarda automáticamente.</span><span class="sxs-lookup"><span data-stu-id="de63f-125">For C# bots that are on a **Consumption plan** and all Node.js bots, the bot is automatically saved.</span></span> 

6. <span data-ttu-id="de63f-126">En el caso de los bots de C# que se encuentren en un plan de **App Service**, abra la hoja **Consola** y envíe el comando **build.cmd**.</span><span class="sxs-lookup"><span data-stu-id="de63f-126">For C# bots on an **App service** plan, open the **Console** blade and send the **build.cmd** command.</span></span> 

   ![Compilación del proyecto en la hoja Consola](~/media/azure-bot-build/cs-console-build-cmd.png)
 
   > [!NOTE]
   > <span data-ttu-id="de63f-128">Si este comando no se compila, intente reiniciar el servicio de aplicación del bot e intente volver a realizar la compilación.</span><span class="sxs-lookup"><span data-stu-id="de63f-128">If this command fails to build, try restarting your bot's app service and try building again.</span></span> <span data-ttu-id="de63f-129">Para reiniciar el servicio de aplicación, en la hoja del bot, haga clic en **All App service settings** (Toda la configuración de App Service) y haga clic en el botón **Reiniciar**.</span><span class="sxs-lookup"><span data-stu-id="de63f-129">To restart your app service, from your bot's blade, click **All App service settings** then click the **Restart** button.</span></span>
   > <span data-ttu-id="de63f-130">![Reinicio de una aplicación web](~/media/azure-bot-build/open-online-code-editor-restart-appservice.png)</span><span class="sxs-lookup"><span data-stu-id="de63f-130">![Restart a web app](~/media/azure-bot-build/open-online-code-editor-restart-appservice.png)</span></span>

7. <span data-ttu-id="de63f-131">Vuelva a Azure Portal y haga clic en **Test in Web Chat** (Probar en Chat en web) para probar los cambios.</span><span class="sxs-lookup"><span data-stu-id="de63f-131">Switch back to Azure portal and click **Test in Web Chat** to test out your changes.</span></span> <span data-ttu-id="de63f-132">Si ya tiene abierto el Chat web para este bot, haga clic en **Start over** (Volver a empezar) para ver los cambios nuevos.</span><span class="sxs-lookup"><span data-stu-id="de63f-132">If you already have the Web Chat open for this bot, click **Start over** to see the new changes.</span></span>

### <a name="functions-bot"></a><span data-ttu-id="de63f-133">Bot de Functions</span><span class="sxs-lookup"><span data-stu-id="de63f-133">Functions Bot</span></span>

1. <span data-ttu-id="de63f-134">Inicie sesión en [Azure Portal](http://portal.azure.com) y abra la hoja para el bot.</span><span class="sxs-lookup"><span data-stu-id="de63f-134">Sign into the [Azure portal](http://portal.azure.com) and open the blade for the bot.</span></span>
2. <span data-ttu-id="de63f-135">En la sección **Administración de bots**, haga clic en **Crear**.</span><span class="sxs-lookup"><span data-stu-id="de63f-135">Under the **Bot Management** section, Click **Build**.</span></span>
3. <span data-ttu-id="de63f-136">Haga clic en **Open this bot in Azure Functions** (Abrir este bot en Azure Functions).</span><span class="sxs-lookup"><span data-stu-id="de63f-136">Click **Open this bot in Azure Functions**.</span></span> <span data-ttu-id="de63f-137">Esta acción abrirá el bot con la interfaz de usuario de <a href="http://go.microsoft.com/fwlink/?linkID=747839" target="_blank">Azure Functions</a>.</span><span class="sxs-lookup"><span data-stu-id="de63f-137">This will open the bot with the <a href="http://go.microsoft.com/fwlink/?linkID=747839" target="_blank">Azure Functions</a> UI.</span></span> 
4. <span data-ttu-id="de63f-138">Modifique el código.</span><span class="sxs-lookup"><span data-stu-id="de63f-138">Make code changes.</span></span> <span data-ttu-id="de63f-139">Por ejemplo, actualice el código de los mensajes de la función.</span><span class="sxs-lookup"><span data-stu-id="de63f-139">For example, update the function's messages code.</span></span> <span data-ttu-id="de63f-140">La captura de pantalla siguiente muestra el código de los mensajes de un bot de Functions para Node.js.</span><span class="sxs-lookup"><span data-stu-id="de63f-140">The screen shot below shows the Messages code for a Node.js Functions Bot.</span></span>

   ![Editor de código de mensajes del bot de Functions](~/media/azure-bot-build/functions-messages-code.png)

5. <span data-ttu-id="de63f-142">Guarde los cambios del código.</span><span class="sxs-lookup"><span data-stu-id="de63f-142">Save your code changes.</span></span>
6. <span data-ttu-id="de63f-143">Vuelva a Azure Portal y haga clic en **Test in Web Chat** (Probar en Chat en web) para probar los cambios.</span><span class="sxs-lookup"><span data-stu-id="de63f-143">Switch back to Azure portal and click **Test in Web Chat** to test out your changes.</span></span> <span data-ttu-id="de63f-144">Si ya tiene abierto el Chat web para este bot, haga clic en **Start over** (Volver a empezar) para ver los cambios nuevos.</span><span class="sxs-lookup"><span data-stu-id="de63f-144">If you already have the Web Chat open for this bot, click **Start over** to see the new changes.</span></span>

## <a name="next-steps"></a><span data-ttu-id="de63f-145">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="de63f-145">Next steps</span></span>
<span data-ttu-id="de63f-146">Ahora que sabe cómo editar el código del bot con el editor de código en línea, también puede compilar localmente el bot mediante el IDE de su preferencia.</span><span class="sxs-lookup"><span data-stu-id="de63f-146">Now that you know how to edit your bot code using the Online Code Editor, you can also build your bot locally using your favorite IDE.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="de63f-147">Descarga del código fuente del bot</span><span class="sxs-lookup"><span data-stu-id="de63f-147">Download the bot source code</span></span>](bot-service-build-download-source-code.md)
