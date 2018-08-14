---
title: Creación de un bot con el SDK de Bot Builder para Node.js | Microsoft Docs
description: Cree un bot con el SDK de Bot Builder para Node.js, un eficaz marco de construcción de bots.
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: get-started-article
ms.prod: bot-framework
ms.date: 12/13/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: 7259e1336965733b860a6129d51b3c2eb10362c7
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305233"
---
# <a name="create-a-bot-with-the-bot-builder-sdk-for-nodejs"></a><span data-ttu-id="1f95f-103">Creación de un bot con el SDK de Bot Builder para Node.js</span><span class="sxs-lookup"><span data-stu-id="1f95f-103">Create a bot with the Bot Builder SDK for Node.js</span></span>
> [!div class="op_single_selector"]
> - [.NET](../dotnet/bot-builder-dotnet-quickstart.md)
> - [Node.js](../nodejs/bot-builder-nodejs-quickstart.md)
> - [Bot Service](../bot-service-quickstart.md)
> - [REST](../rest-api/bot-framework-rest-connector-quickstart.md)

<span data-ttu-id="1f95f-108">El SDK de Bot Builder para Node.js es un marco para el desarrollo de bots.</span><span class="sxs-lookup"><span data-stu-id="1f95f-108">The Bot Builder SDK for Node.js is a framework for developing bots.</span></span> <span data-ttu-id="1f95f-109">Es fácil de usar y modela marcos como Express y Restify para proporcionar a los desarrolladores de JavaScript una manera familiar de escribir bots.</span><span class="sxs-lookup"><span data-stu-id="1f95f-109">It is easy to use and models frameworks like Express & restify to provide a familiar way for JavaScript developers to write bots.</span></span>

<span data-ttu-id="1f95f-110">Este tutorial le guía en la creación de un bot con el SDK de Bot Builder para Node.js.</span><span class="sxs-lookup"><span data-stu-id="1f95f-110">This tutorial walks you through building a bot by using the Bot Builder SDK for Node.js.</span></span> <span data-ttu-id="1f95f-111">Puede probar el bot en una ventana de consola y con Bot Framework Emulator.</span><span class="sxs-lookup"><span data-stu-id="1f95f-111">You can test the bot in a console window and with the Bot Framework Emulator.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1f95f-112">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="1f95f-112">Prerequisites</span></span>
<span data-ttu-id="1f95f-113">Para comenzar, complete las tareas que son requisito previo:</span><span class="sxs-lookup"><span data-stu-id="1f95f-113">Get started by completing the following prerequisite tasks:</span></span>

1. <span data-ttu-id="1f95f-114">[Instale Node.js](https://nodejs.org).</span><span class="sxs-lookup"><span data-stu-id="1f95f-114">Install [Node.js](https://nodejs.org).</span></span>
2. <span data-ttu-id="1f95f-115">Cree una carpeta para el bot.</span><span class="sxs-lookup"><span data-stu-id="1f95f-115">Create a folder for your bot.</span></span>
3. <span data-ttu-id="1f95f-116">Desde un terminal o símbolo del sistema, vaya a la carpeta que acaba de crear.</span><span class="sxs-lookup"><span data-stu-id="1f95f-116">From a command prompt or terminal, navigate to the folder you just created.</span></span>
4. <span data-ttu-id="1f95f-117">Ejecute el siguiente comando **npm**:</span><span class="sxs-lookup"><span data-stu-id="1f95f-117">Run the following **npm** command:</span></span>

```nodejs
npm init
```

<span data-ttu-id="1f95f-118">Siga las indicaciones en pantalla para especificar información sobre su bot, y npm creará un archivo **package.json** que contiene la información proporcionada.</span><span class="sxs-lookup"><span data-stu-id="1f95f-118">Follow the prompt on the screen to enter information about your bot and npm will create a **package.json** file that contains the information you provided.</span></span> 

## <a name="install-the-sdk"></a><span data-ttu-id="1f95f-119">Instalación del SDK</span><span class="sxs-lookup"><span data-stu-id="1f95f-119">Install the SDK</span></span>
<span data-ttu-id="1f95f-120">A continuación, instale el SDK de Bot Builder para Node.js mediante el siguiente comando **npm**:</span><span class="sxs-lookup"><span data-stu-id="1f95f-120">Next, install the Bot Builder SDK for Node.js by running the following **npm** command:</span></span>

```nodejs
npm install --save botbuilder
```

<span data-ttu-id="1f95f-121">Una vez que tenga instalado el SDK, está listo para escribir su primer bot.</span><span class="sxs-lookup"><span data-stu-id="1f95f-121">Once you have the SDK installed, you are ready to write your first bot.</span></span>

<span data-ttu-id="1f95f-122">Como primer bot, creará uno que simplemente devuelva cualquier entrada del usuario.</span><span class="sxs-lookup"><span data-stu-id="1f95f-122">For your first bot, you will create a bot that simply echoes back any user input.</span></span> <span data-ttu-id="1f95f-123">Para crear el bot, siga estos pasos:</span><span class="sxs-lookup"><span data-stu-id="1f95f-123">To create your bot, follow these steps:</span></span>

1. <span data-ttu-id="1f95f-124">En la carpeta que creó anteriormente para el bot, cree un archivo denominado **app.js**.</span><span class="sxs-lookup"><span data-stu-id="1f95f-124">In the folder that you created earlier for your bot, create a new file named **app.js**.</span></span>
2. <span data-ttu-id="1f95f-125">Abra **app.js** en un editor de texto o en un IDE de su elección.</span><span class="sxs-lookup"><span data-stu-id="1f95f-125">Open **app.js** in a text editor or an IDE of your choice.</span></span> <span data-ttu-id="1f95f-126">Agregue el siguiente código al archivo:</span><span class="sxs-lookup"><span data-stu-id="1f95f-126">Add the following code to the file:</span></span> 

   [!code-javascript[consolebot code sample Node.js](../includes/code/node-getstarted.js#consolebot)]

3. <span data-ttu-id="1f95f-127">Guarde el archivo.</span><span class="sxs-lookup"><span data-stu-id="1f95f-127">Save the file.</span></span> <span data-ttu-id="1f95f-128">Ahora está listo para ejecutar y probar el bot.</span><span class="sxs-lookup"><span data-stu-id="1f95f-128">Now you are ready to run and test out your bot.</span></span>

### <a name="start-your-bot"></a><span data-ttu-id="1f95f-129">Inicio del bot</span><span class="sxs-lookup"><span data-stu-id="1f95f-129">Start your bot</span></span>

<span data-ttu-id="1f95f-130">Vaya al directorio de su bot en una ventana de consola e inicie el bot:</span><span class="sxs-lookup"><span data-stu-id="1f95f-130">Navigate to your bot's directory in a console window and start your bot:</span></span>

```nodejs
node app.js
```

<span data-ttu-id="1f95f-131">El bot se ejecuta ahora localmente.</span><span class="sxs-lookup"><span data-stu-id="1f95f-131">Your bot is now running locally.</span></span> <span data-ttu-id="1f95f-132">Para probar su bot, escriba unos cuantos mensajes en la ventana de consola.</span><span class="sxs-lookup"><span data-stu-id="1f95f-132">Try out your bot by typing a few messages in the console window.</span></span>
<span data-ttu-id="1f95f-133">Verá que el bot responde a cada mensaje que envía repitiendo el mensaje con el texto delante *"Dijo"*.</span><span class="sxs-lookup"><span data-stu-id="1f95f-133">You should see that the bot responds to each message you send by echoing back your message prefixed with the text *"You said:"*.</span></span>

## <a name="install-restify"></a><span data-ttu-id="1f95f-134">Instalar Restify</span><span class="sxs-lookup"><span data-stu-id="1f95f-134">Install Restify</span></span>

<span data-ttu-id="1f95f-135">Los bots de consola son buenos clientes basados en texto, pero para usar cualquiera de los canales de Bot Framework (o ejecutar el bot en el emulador), el bot deberá ejecutarse en un punto de conexión de API.</span><span class="sxs-lookup"><span data-stu-id="1f95f-135">Console bots are good text-based clients, but in order to use any of the Bot Framework channels (or run your bot in the emulator), your bot will need to run on an API endpoint.</span></span> <span data-ttu-id="1f95f-136">Instale <a href="http://restify.com/" target="_blank">restify</a> con el siguiente comando **npm**:</span><span class="sxs-lookup"><span data-stu-id="1f95f-136">Install <a href="http://restify.com/" target="_blank">restify</a> by running the following **npm** command:</span></span>

```nodejs
npm install --save restify
```

<span data-ttu-id="1f95f-137">Una vez instalado, está listo para realizar algunos cambios en el bot.</span><span class="sxs-lookup"><span data-stu-id="1f95f-137">Once you have Restify in place, you're ready to make some changes to your bot.</span></span>

## <a name="edit-your-bot"></a><span data-ttu-id="1f95f-138">Edición del bot</span><span class="sxs-lookup"><span data-stu-id="1f95f-138">Edit your bot</span></span>

<span data-ttu-id="1f95f-139">Deberá realizar algunos cambios en el archivo **app.js**.</span><span class="sxs-lookup"><span data-stu-id="1f95f-139">You will need to make some changes to your **app.js** file.</span></span> 

1. <span data-ttu-id="1f95f-140">Agregue una línea para exigir el módulo `restify`.</span><span class="sxs-lookup"><span data-stu-id="1f95f-140">Add a line to require the `restify` module.</span></span>
2. <span data-ttu-id="1f95f-141">Cambie `ConsoleConnector` por `ChatConnector`.</span><span class="sxs-lookup"><span data-stu-id="1f95f-141">Change the `ConsoleConnector` to a `ChatConnector`.</span></span>
3. <span data-ttu-id="1f95f-142">Incluya el identificador y la contraseña de aplicación de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="1f95f-142">Include your Microsoft App ID and App Password.</span></span>
4. <span data-ttu-id="1f95f-143">Haga que el conector escuche en un punto de conexión de API.</span><span class="sxs-lookup"><span data-stu-id="1f95f-143">Have the connector listen on an API endpoint.</span></span>

   [!code-javascript[echobot code sample Node.js](../includes/code/node-getstarted.js#echobot)]

5. <span data-ttu-id="1f95f-144">Guarde el archivo.</span><span class="sxs-lookup"><span data-stu-id="1f95f-144">Save the file.</span></span> <span data-ttu-id="1f95f-145">Ahora está listo para ejecutar y probar el bot en el emulador.</span><span class="sxs-lookup"><span data-stu-id="1f95f-145">Now you are ready to run and test out your bot in the emulator.</span></span>

> [!NOTE] 
> No se necesita un **identificador de aplicación de Microsoft** ni una **contraseña de aplicación de Microsoft** para ejecutar el bot en Bot Framework Emulator.

## <a name="test-your-bot"></a><span data-ttu-id="1f95f-147">Prueba del bot</span><span class="sxs-lookup"><span data-stu-id="1f95f-147">Test your bot</span></span>
<span data-ttu-id="1f95f-148">A continuación, pruebe el bot mediante [Bot Framework Emulator](../bot-service-debug-emulator.md) para verlo en acción.</span><span class="sxs-lookup"><span data-stu-id="1f95f-148">Next, test your bot by using the [Bot Framework Emulator](../bot-service-debug-emulator.md) to see it in action.</span></span> <span data-ttu-id="1f95f-149">El emulador es una aplicación de escritorio que le permite probar y depurar el bot en localhost o ejecutarlo de forma remota a través de un túnel.</span><span class="sxs-lookup"><span data-stu-id="1f95f-149">The emulator is a desktop application that lets you test and debug your bot on localhost or running remotely through a tunnel.</span></span>

<span data-ttu-id="1f95f-150">Primero, [descargue](https://emulator.botframework.com) e instale el emulador.</span><span class="sxs-lookup"><span data-stu-id="1f95f-150">First, you'll need to [download](https://emulator.botframework.com) and install the emulator.</span></span> <span data-ttu-id="1f95f-151">Una vez finalizada la descarga, inicie el archivo ejecutable y complete el proceso de instalación.</span><span class="sxs-lookup"><span data-stu-id="1f95f-151">After the download completes, launch the executable and complete the installation process.</span></span>

### <a name="start-your-bot"></a><span data-ttu-id="1f95f-152">Inicio del bot</span><span class="sxs-lookup"><span data-stu-id="1f95f-152">Start your bot</span></span>

<span data-ttu-id="1f95f-153">Después de instalar el emulador, navegue al directorio de su bot en una ventana de consola e inicie su bot:</span><span class="sxs-lookup"><span data-stu-id="1f95f-153">After installing the emulator, navigate to your bot's directory in a console window and start your bot:</span></span>

```nodejs
node app.js
```
   
<span data-ttu-id="1f95f-154">El bot se ejecuta ahora localmente.</span><span class="sxs-lookup"><span data-stu-id="1f95f-154">Your bot is now running locally.</span></span>

### <a name="start-the-emulator-and-connect-your-bot"></a><span data-ttu-id="1f95f-155">Inicio del emulador y conexión del bot</span><span class="sxs-lookup"><span data-stu-id="1f95f-155">Start the emulator and connect your bot</span></span>
<span data-ttu-id="1f95f-156">Después de iniciar su bot, inicie el emulador y, a continuación, conecte el bot:</span><span class="sxs-lookup"><span data-stu-id="1f95f-156">After you start your bot, start the emulator and then connect your bot:</span></span>

1. <span data-ttu-id="1f95f-157">Haga clic en el vínculo **create a new bot configuration** (Crear configuración de bot) en la ventana del emulador.</span><span class="sxs-lookup"><span data-stu-id="1f95f-157">Click **create a new bot configuration** link in the emulator window.</span></span> 

2. <span data-ttu-id="1f95f-158">Escriba `http://localhost:port-number/api/messages` en la barra de dirección, donde *port-number* coincide con el número de puerto mostrado en el explorador donde se ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1f95f-158">Type `http://localhost:port-number/api/messages` into the address bar, where *port-number* matches the port number shown in the browser where your application is running.</span></span>

3. <span data-ttu-id="1f95f-159">Haga clic en Save (Guardar) y Connect (Conectar).</span><span class="sxs-lookup"><span data-stu-id="1f95f-159">Click Save and connect.</span></span> <span data-ttu-id="1f95f-160">No tendrá que especificar el identificador ni la contraseña de aplicación de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="1f95f-160">You won't need to specify Microsoft App ID and Microsoft App Password.</span></span> <span data-ttu-id="1f95f-161">Por ahora puede dejar estos campos en blanco.</span><span class="sxs-lookup"><span data-stu-id="1f95f-161">You can leave these fields blank for now.</span></span> <span data-ttu-id="1f95f-162">Más adelante obtendrá esta información al registrar el bot.</span><span class="sxs-lookup"><span data-stu-id="1f95f-162">You'll get this information later when you register your bot.</span></span>

### <a name="try-out-your-bot"></a><span data-ttu-id="1f95f-163">Prueba del bot</span><span class="sxs-lookup"><span data-stu-id="1f95f-163">Try out your bot</span></span>

<span data-ttu-id="1f95f-164">Ahora que el bot se ejecuta localmente y está conectado al emulador, escriba algunos mensajes en el emulador para probar el bot.</span><span class="sxs-lookup"><span data-stu-id="1f95f-164">Now that your bot is running locally and is connected to the emulator, try out your bot by typing a few messages in the emulator.</span></span>
<span data-ttu-id="1f95f-165">Verá que el bot responde a cada mensaje que envía repitiendo el mensaje con el texto delante *"Dijo"*.</span><span class="sxs-lookup"><span data-stu-id="1f95f-165">You should see that the bot responds to each message you send by echoing back your message prefixed with the text *"You said:"*.</span></span>

<span data-ttu-id="1f95f-166">Ha creado correctamente su primer bot mediante el SDK de Bot Builder para Node.js.</span><span class="sxs-lookup"><span data-stu-id="1f95f-166">You've successfully created your first bot using the Bot Builder SDK for Node.js!</span></span>

## <a name="next-steps"></a><span data-ttu-id="1f95f-167">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="1f95f-167">Next steps</span></span>

> [!div class="nextstepaction"]
> [SDK de Bot Builder para Node.js](bot-builder-nodejs-overview.md)
