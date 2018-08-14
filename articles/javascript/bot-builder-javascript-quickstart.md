---
title: Creación de un bot con el SDK de Bot Builder para JavaScript | Microsoft Docs
description: Cree un bot de forma rápida con el SDK de Bot Builder para JavaScript.
keywords: inicio rápido, sdk de bot builder, introducción
author: jonathanfingold
ms.author: jonathanfingold
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 07/12/2018
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: 337b6a0b8739b5e5de4d2d1b2b87dcad55f2c854
ms.sourcegitcommit: dcbc8ad992a3e242a11ebcdf0ee99714d919a877
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/30/2018
ms.locfileid: "39352934"
---
# <a name="create-a-bot-with-the-bot-builder-sdk-v4-preview-for-javascript"></a><span data-ttu-id="dc40b-104">Creación de un bot con el SDK v4 de Bot Builder (versión preliminar) para JavaScript</span><span class="sxs-lookup"><span data-stu-id="dc40b-104">Create a bot with the Bot Builder SDK v4 (preview) for JavaScript</span></span>
[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]

<span data-ttu-id="dc40b-105">Este inicio rápido le guía por la creación de un bot mediante el generador Bot Builder de Yeoman y el SDK de Bot Builder para JavaScript, y después se prueba con el emulador de Bot Framework.</span><span class="sxs-lookup"><span data-stu-id="dc40b-105">This quickstart walks you through building a bot by using the Yeoman Bot Builder generator and the Bot Builder SDK for JavaScript, and then testing it with the Bot Framework Emulator.</span></span> <span data-ttu-id="dc40b-106">Esto se basa en el [SDK v4 de Microsoft Bot Builder](https://github.com/Microsoft/botbuilder-js).</span><span class="sxs-lookup"><span data-stu-id="dc40b-106">This is based off the [Microsoft Bot Builder SDK v4](https://github.com/Microsoft/botbuilder-js).</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="dc40b-107">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="dc40b-107">Pre-requisites</span></span>
- [<span data-ttu-id="dc40b-108">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="dc40b-108">Visual Studio Code</span></span>](https://www.visualstudio.com/downloads)
- [<span data-ttu-id="dc40b-109">Node.js</span><span class="sxs-lookup"><span data-stu-id="dc40b-109">Node.js</span></span>](https://nodejs.org/en/)
- <span data-ttu-id="dc40b-110">[Yeoman](http://yeoman.io/), que puede usar un generador para crear un bot de forma automática.</span><span class="sxs-lookup"><span data-stu-id="dc40b-110">[Yeoman](http://yeoman.io/), which can use a generator to create a bot for you</span></span>
- [<span data-ttu-id="dc40b-111">Bot Emulator</span><span class="sxs-lookup"><span data-stu-id="dc40b-111">Bot Emulator</span></span>](https://github.com/Microsoft/BotFramework-Emulator)
- <span data-ttu-id="dc40b-112">Conocimientos sobre [restify](http://restify.com/) y la programación asincrónica en Java.</span><span class="sxs-lookup"><span data-stu-id="dc40b-112">Knowledge of [restify](http://restify.com/) and asynchronous programming in Java</span></span>

> [!NOTE]
> <span data-ttu-id="dc40b-113">En algunas instalaciones, el paso de instalación de restify genera un error relacionado con node-gyp.</span><span class="sxs-lookup"><span data-stu-id="dc40b-113">For some installations the install step for restify is giving an error related to node-gyp.</span></span>
> <span data-ttu-id="dc40b-114">Si este es el caso, intente ejecutar `npm install -g windows-build-tools`.</span><span class="sxs-lookup"><span data-stu-id="dc40b-114">If this is the case try running `npm install -g windows-build-tools`.</span></span>


<span data-ttu-id="dc40b-115">El SDK de Bot Builder para JavaScript consta de una serie de [paquetes](https://github.com/Microsoft/botbuilder-js/tree/master/libraries) que se pueden instalar desde NPM mediante una etiqueta `@preview` especial.</span><span class="sxs-lookup"><span data-stu-id="dc40b-115">The Bot Builder SDK for JavaScript consists of a series of [packages](https://github.com/Microsoft/botbuilder-js/tree/master/libraries) which can be installed from NPM using a special `@preview` tag.</span></span>

# <a name="create-a-bot"></a><span data-ttu-id="dc40b-116">Creación de un bot</span><span class="sxs-lookup"><span data-stu-id="dc40b-116">Create a bot</span></span>

<span data-ttu-id="dc40b-117">Abra un símbolo del sistema con privilegios elevados, cree un directorio e inicialice el paquete para el bot.</span><span class="sxs-lookup"><span data-stu-id="dc40b-117">Open an elevated command prompt, create a directory, and initialize the package for your bot.</span></span>

```bash
md myJsBots
cd myJsBots
```

<span data-ttu-id="dc40b-118">Después, instale Yeoman y el generador para JavaScript.</span><span class="sxs-lookup"><span data-stu-id="dc40b-118">Next, install Yeoman and the generator for JavaScript.</span></span>

```bash
npm install -g yo
npm install -g generator-botbuilder@preview
```

<span data-ttu-id="dc40b-119">Luego, use el generador para crear un bot de eco.</span><span class="sxs-lookup"><span data-stu-id="dc40b-119">Then, use the generator to create an echo bot.</span></span>

```bash
yo botbuilder
```

<span data-ttu-id="dc40b-120">Yeoman le solicitará alguna información con la que se va a crear el bot.</span><span class="sxs-lookup"><span data-stu-id="dc40b-120">Yeoman prompts you for some information with which to create your bot.</span></span>
-   <span data-ttu-id="dc40b-121">Escriba un nombre para el bot.</span><span class="sxs-lookup"><span data-stu-id="dc40b-121">Enter a name for your bot.</span></span>
-   <span data-ttu-id="dc40b-122">Escriba una descripción.</span><span class="sxs-lookup"><span data-stu-id="dc40b-122">Enter a description.</span></span>
-   <span data-ttu-id="dc40b-123">Elija el lenguaje para el bot, ya sea `JavaScript` o `TypeScript`.</span><span class="sxs-lookup"><span data-stu-id="dc40b-123">Choose the language for your bot, either `JavaScript` or `TypeScript`.</span></span>
-   <span data-ttu-id="dc40b-124">Elija la plantilla que se va a usar.</span><span class="sxs-lookup"><span data-stu-id="dc40b-124">Choose the template to use.</span></span> <span data-ttu-id="dc40b-125">En la actualidad, la única plantilla es `Echo`, pero se agregarán otras próximamente.</span><span class="sxs-lookup"><span data-stu-id="dc40b-125">Currently, `Echo` is the only template, but others will be added soon.</span></span>

<span data-ttu-id="dc40b-126">Yeoman crea el bot en una carpeta nueva.</span><span class="sxs-lookup"><span data-stu-id="dc40b-126">Yeoman creates your bot in a new folder.</span></span>

## <a name="explore-code"></a><span data-ttu-id="dc40b-127">Exploración del código</span><span class="sxs-lookup"><span data-stu-id="dc40b-127">Explore code</span></span>

<span data-ttu-id="dc40b-128">Al abrir la carpeta de bot recién creada, verá un archivo `app.js`.</span><span class="sxs-lookup"><span data-stu-id="dc40b-128">When you open your newly created bot folder, you will see an `app.js` file.</span></span> <span data-ttu-id="dc40b-129">Este archivo `app.js` contendrá todo el código necesario para ejecutar una aplicación de bot.</span><span class="sxs-lookup"><span data-stu-id="dc40b-129">This `app.js` file will contain all the code needed to run a bot app.</span></span> <span data-ttu-id="dc40b-130">Este archivo contiene un bot de eco que devolverá todo lo que escriba, y también incrementará un contador.</span><span class="sxs-lookup"><span data-stu-id="dc40b-130">This file contains an echo bot that will echo back whatever you input as well as increment a counter.</span></span> 

<span data-ttu-id="dc40b-131">En el código siguiente, el software intermedio de estado de la conversación usa el almacenamiento en memoria.</span><span class="sxs-lookup"><span data-stu-id="dc40b-131">In the following code, conversation state middleware uses in-memory storage.</span></span> <span data-ttu-id="dc40b-132">Lee y escribe el objeto de estado en el almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="dc40b-132">It reads and writes the state object to storage.</span></span> <span data-ttu-id="dc40b-133">La variable de contador realiza el seguimiento del número de mensajes que se envían al bot.</span><span class="sxs-lookup"><span data-stu-id="dc40b-133">The count variable keeps track of the number of messages sent to the bot.</span></span> <span data-ttu-id="dc40b-134">Puede usar una técnica similar para mantener el estado entre los turnos.</span><span class="sxs-lookup"><span data-stu-id="dc40b-134">You can use a similar technique to maintain state in between turns.</span></span> 

<span data-ttu-id="dc40b-135">**app.js**</span><span class="sxs-lookup"><span data-stu-id="dc40b-135">**app.js**</span></span>
```javascript
// Packages are installed for you
const { BotFrameworkAdapter, MemoryStorage, ConversationState } = require('botbuilder');
const restify = require('restify');

// Create server
let server = restify.createServer();
server.listen(process.env.port || process.env.PORT || 3978, function () {
    console.log(`${server.name} listening to ${server.url}`);
});

// Create adapter
const adapter = new BotFrameworkAdapter({ 
    appId: process.env.MICROSOFT_APP_ID, 
    appPassword: process.env.MICROSOFT_APP_PASSWORD 
});

// Add conversation state middleware
const conversationState = new ConversationState(new MemoryStorage());
adapter.use(conversationState);
```

<span data-ttu-id="dc40b-136">En el código siguiente se realizan escuchas de solicitudes entrantes y se comprueba el tipo de actividad entrante antes de enviar una respuesta al usuario.</span><span class="sxs-lookup"><span data-stu-id="dc40b-136">The following code listens for incoming request and checks the incoming activity type before sending a reply to the user.</span></span>

```javascript
// Listen for incoming requests 
server.post('/api/messages', (req, res) => {
    // Route received request to adapter for processing
    adapter.processActivity(req, res, (context) => {
        // This bot is only handling Messages
        if (context.activity.type === 'message') {
        
            // Get the conversation state
            const state = conversationState.get(context);
            
            // If state.count is undefined set it to 0, otherwise increment it by 1
            const count = state.count === undefined ? state.count = 0 : ++state.count;
            
            // Echo back to the user whatever they typed.
            return context.sendActivity(`${count}: You said "${context.activity.text}"`);
        } else {
            // Echo back the type of activity the bot detected if not of type message
            return context.sendActivity(`[${context.activity.type} event detected]`);
        }
    });
});
```

## <a name="start-your-bot"></a><span data-ttu-id="dc40b-137">Inicio del bot</span><span class="sxs-lookup"><span data-stu-id="dc40b-137">Start your bot</span></span>

<span data-ttu-id="dc40b-138">Cambie el directorio al que creó para el bot e inícielo.</span><span class="sxs-lookup"><span data-stu-id="dc40b-138">Change directories to the one created for your bot, and start it.</span></span>

```bash
cd <bot directory>
node app.js
```

## <a name="start-the-emulator-and-connect-your-bot"></a><span data-ttu-id="dc40b-139">Inicio del emulador y conexión del bot</span><span class="sxs-lookup"><span data-stu-id="dc40b-139">Start the emulator and connect your bot</span></span>
<span data-ttu-id="dc40b-140">En este momento, el bot se ejecuta de forma local.</span><span class="sxs-lookup"><span data-stu-id="dc40b-140">At this point, your bot is running locally.</span></span> <span data-ttu-id="dc40b-141">A continuación, inicie el emulador y, después, conéctese al bot en el emulador:</span><span class="sxs-lookup"><span data-stu-id="dc40b-141">Next, start the emulator and then connect to your bot in the emulator:</span></span>
1. <span data-ttu-id="dc40b-142">Haga clic en el vínculo **create a new bot configuration** (Crear configuración de bot) en la pestaña "Welcome" (Bienvenido) del emulador.</span><span class="sxs-lookup"><span data-stu-id="dc40b-142">Click **create a new bot configuration** link in the emulator "Welcome" tab.</span></span> 

2. <span data-ttu-id="dc40b-143">Escriba un **nombre de bot** y la ruta de acceso del directorio del código del bot.</span><span class="sxs-lookup"><span data-stu-id="dc40b-143">Enter a **Bot name** and enter the directory path to your bot code.</span></span> <span data-ttu-id="dc40b-144">El archivo de configuración del bot se guardará en esta ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="dc40b-144">The bot configuration file will be saved to this path.</span></span>

3. <span data-ttu-id="dc40b-145">Escriba `http://localhost:port-number/api/messages` en el campo **Dirección URL del punto de conexión**, donde *númeroDePuerto* coincide con el número de puerto que se muestra en el explorador en el que se ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="dc40b-145">Type `http://localhost:port-number/api/messages` into the **Endpoint URL** field, where *port-number* matches the port number shown in the browser where your application is running.</span></span>

4. <span data-ttu-id="dc40b-146">Haga clic en **Connect** (Conectar) para conectarse al bot.</span><span class="sxs-lookup"><span data-stu-id="dc40b-146">Click **Connect** to connect to your bot.</span></span> <span data-ttu-id="dc40b-147">No tendrá que especificar el **Id. de aplicación de Microsoft** ni la **contraseña de la aplicación de Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="dc40b-147">You won't need to specify **Microsoft App ID** and **Microsoft App Password**.</span></span> <span data-ttu-id="dc40b-148">Por ahora puede dejar estos campos en blanco.</span><span class="sxs-lookup"><span data-stu-id="dc40b-148">You can leave these fields blank for now.</span></span> <span data-ttu-id="dc40b-149">Obtendrá esta información más adelante al registrar el bot.</span><span class="sxs-lookup"><span data-stu-id="dc40b-149">You'll get this information later when you register your bot.</span></span>

<span data-ttu-id="dc40b-150">Envíe "Hi" (Hola) al bot y el bot responderá al mensaje con "0: You said "Hi"" (0: Ha dicho Hola).</span><span class="sxs-lookup"><span data-stu-id="dc40b-150">Send "Hi" to your bot, and the bot will respond with '0: You said "Hi"' to the message.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dc40b-151">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="dc40b-151">Next steps</span></span>

<span data-ttu-id="dc40b-152">A continuación, vaya a los conceptos que explican qué es un bot y cómo funciona.</span><span class="sxs-lookup"><span data-stu-id="dc40b-152">Next, jump into the concepts that explain a bot and how it works.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="dc40b-153">Conceptos básicos de bot</span><span class="sxs-lookup"><span data-stu-id="dc40b-153">Basic Bot concepts</span></span>](../v4sdk/bot-builder-basics.md)
