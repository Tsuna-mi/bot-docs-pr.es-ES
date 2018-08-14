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
# <a name="create-a-bot-with-the-bot-builder-sdk-v4-preview-for-javascript"></a>Creación de un bot con el SDK v4 de Bot Builder (versión preliminar) para JavaScript
[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]

Este inicio rápido le guía por la creación de un bot mediante el generador Bot Builder de Yeoman y el SDK de Bot Builder para JavaScript, y después se prueba con el emulador de Bot Framework. Esto se basa en el [SDK v4 de Microsoft Bot Builder](https://github.com/Microsoft/botbuilder-js).

## <a name="pre-requisites"></a>Requisitos previos
- [Visual Studio Code](https://www.visualstudio.com/downloads)
- [Node.js](https://nodejs.org/en/)
- [Yeoman](http://yeoman.io/), que puede usar un generador para crear un bot de forma automática.
- [Bot Emulator](https://github.com/Microsoft/BotFramework-Emulator)
- Conocimientos sobre [restify](http://restify.com/) y la programación asincrónica en Java.

> [!NOTE]
> En algunas instalaciones, el paso de instalación de restify genera un error relacionado con node-gyp.
> Si este es el caso, intente ejecutar `npm install -g windows-build-tools`.


El SDK de Bot Builder para JavaScript consta de una serie de [paquetes](https://github.com/Microsoft/botbuilder-js/tree/master/libraries) que se pueden instalar desde NPM mediante una etiqueta `@preview` especial.

# <a name="create-a-bot"></a>Creación de un bot

Abra un símbolo del sistema con privilegios elevados, cree un directorio e inicialice el paquete para el bot.

```bash
md myJsBots
cd myJsBots
```

Después, instale Yeoman y el generador para JavaScript.

```bash
npm install -g yo
npm install -g generator-botbuilder@preview
```

Luego, use el generador para crear un bot de eco.

```bash
yo botbuilder
```

Yeoman le solicitará alguna información con la que se va a crear el bot.
-   Escriba un nombre para el bot.
-   Escriba una descripción.
-   Elija el lenguaje para el bot, ya sea `JavaScript` o `TypeScript`.
-   Elija la plantilla que se va a usar. En la actualidad, la única plantilla es `Echo`, pero se agregarán otras próximamente.

Yeoman crea el bot en una carpeta nueva.

## <a name="explore-code"></a>Exploración del código

Al abrir la carpeta de bot recién creada, verá un archivo `app.js`. Este archivo `app.js` contendrá todo el código necesario para ejecutar una aplicación de bot. Este archivo contiene un bot de eco que devolverá todo lo que escriba, y también incrementará un contador. 

En el código siguiente, el software intermedio de estado de la conversación usa el almacenamiento en memoria. Lee y escribe el objeto de estado en el almacenamiento. La variable de contador realiza el seguimiento del número de mensajes que se envían al bot. Puede usar una técnica similar para mantener el estado entre los turnos. 

**app.js**
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

En el código siguiente se realizan escuchas de solicitudes entrantes y se comprueba el tipo de actividad entrante antes de enviar una respuesta al usuario.

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

## <a name="start-your-bot"></a>Inicio del bot

Cambie el directorio al que creó para el bot e inícielo.

```bash
cd <bot directory>
node app.js
```

## <a name="start-the-emulator-and-connect-your-bot"></a>Inicio del emulador y conexión del bot
En este momento, el bot se ejecuta de forma local. A continuación, inicie el emulador y, después, conéctese al bot en el emulador:
1. Haga clic en el vínculo **create a new bot configuration** (Crear configuración de bot) en la pestaña "Welcome" (Bienvenido) del emulador. 

2. Escriba un **nombre de bot** y la ruta de acceso del directorio del código del bot. El archivo de configuración del bot se guardará en esta ruta de acceso.

3. Escriba `http://localhost:port-number/api/messages` en el campo **Dirección URL del punto de conexión**, donde *númeroDePuerto* coincide con el número de puerto que se muestra en el explorador en el que se ejecuta la aplicación.

4. Haga clic en **Connect** (Conectar) para conectarse al bot. No tendrá que especificar el **Id. de aplicación de Microsoft** ni la **contraseña de la aplicación de Microsoft**. Por ahora puede dejar estos campos en blanco. Obtendrá esta información más adelante al registrar el bot.

Envíe "Hi" (Hola) al bot y el bot responderá al mensaje con "0: You said "Hi"" (0: Ha dicho Hola).

## <a name="next-steps"></a>Pasos siguientes

A continuación, vaya a los conceptos que explican qué es un bot y cómo funciona.

> [!div class="nextstepaction"]
> [Conceptos básicos de bot](../v4sdk/bot-builder-basics.md)
