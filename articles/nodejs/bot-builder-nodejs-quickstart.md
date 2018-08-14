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
ms.openlocfilehash: 6159997ec5ea3dbd3188ba2ea4b6207b5d9db08f
ms.sourcegitcommit: 97bb24f15041caccef4ca5736aa14f144881e0c6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2018
ms.locfileid: "39567554"
---
# <a name="create-a-bot-with-the-bot-builder-sdk-for-nodejs"></a>Creación de un bot con el SDK de Bot Builder para Node.js

[!INCLUDE [pre-release-label](../includes/pre-release-label-v3.md)]

> [!div class="op_single_selector"]
> - [.NET](../dotnet/bot-builder-dotnet-quickstart.md)
> - [Node.js](../nodejs/bot-builder-nodejs-quickstart.md)
> - [Bot Service](../bot-service-quickstart.md)
> - [REST](../rest-api/bot-framework-rest-connector-quickstart.md)

El SDK de Bot Builder para Node.js es un marco para el desarrollo de bots. Es fácil de usar y modela marcos como Express y Restify para proporcionar a los desarrolladores de JavaScript una manera familiar de escribir bots.

Este tutorial le guía en la creación de un bot con el SDK de Bot Builder para Node.js. Puede probar el bot en una ventana de consola y con Bot Framework Emulator.

## <a name="prerequisites"></a>Requisitos previos
Para comenzar, complete las tareas que son requisito previo:

1. [Instale Node.js](https://nodejs.org).
2. Cree una carpeta para el bot.
3. Desde un terminal o símbolo del sistema, vaya a la carpeta que acaba de crear.
4. Ejecute el siguiente comando **npm**:

```nodejs
npm init
```

Siga las indicaciones en pantalla para especificar información sobre su bot, y npm creará un archivo **package.json** que contiene la información proporcionada. 

## <a name="install-the-sdk"></a>Instalación del SDK
A continuación, instale el SDK de Bot Builder para Node.js mediante el siguiente comando **npm**:

```nodejs
npm install --save botbuilder
```

Una vez que tenga instalado el SDK, está listo para escribir su primer bot.

Como primer bot, creará uno que simplemente devuelva cualquier entrada del usuario. Para crear el bot, siga estos pasos:

1. En la carpeta que creó anteriormente para el bot, cree un archivo denominado **app.js**.
2. Abra **app.js** en un editor de texto o en un IDE de su elección. Agregue el siguiente código al archivo: 

   [!code-javascript[consolebot code sample Node.js](../includes/code/node-getstarted.js#consolebot)]

3. Guarde el archivo. Ahora está listo para ejecutar y probar el bot.

### <a name="start-your-bot"></a>Inicio del bot

Vaya al directorio de su bot en una ventana de consola e inicie el bot:

```nodejs
node app.js
```

El bot se ejecuta ahora localmente. Para probar su bot, escriba unos cuantos mensajes en la ventana de consola.
Verá que el bot responde a cada mensaje que envía repitiendo el mensaje con el texto delante *"Dijo"*.

## <a name="install-restify"></a>Instalar Restify

Los bots de consola son buenos clientes basados en texto, pero para usar cualquiera de los canales de Bot Framework (o ejecutar el bot en el emulador), el bot deberá ejecutarse en un punto de conexión de API. Instale <a href="http://restify.com/" target="_blank">restify</a> con el siguiente comando **npm**:

```nodejs
npm install --save restify
```

Una vez instalado, está listo para realizar algunos cambios en el bot.

## <a name="edit-your-bot"></a>Edición del bot

Deberá realizar algunos cambios en el archivo **app.js**. 

1. Agregue una línea para exigir el módulo `restify`.
2. Cambie `ConsoleConnector` por `ChatConnector`.
3. Incluya el identificador y la contraseña de aplicación de Microsoft.
4. Haga que el conector escuche en un punto de conexión de API.

   [!code-javascript[echobot code sample Node.js](../includes/code/node-getstarted.js#echobot)]

5. Guarde el archivo. Ahora está listo para ejecutar y probar el bot en el emulador.

> [!NOTE] 
> No se necesita un **identificador de aplicación de Microsoft** ni una **contraseña de aplicación de Microsoft** para ejecutar el bot en Bot Framework Emulator.

## <a name="test-your-bot"></a>Prueba del bot
A continuación, pruebe el bot mediante [Bot Framework Emulator](../bot-service-debug-emulator.md) para verlo en acción. El emulador es una aplicación de escritorio que le permite probar y depurar el bot en localhost o ejecutarlo de forma remota a través de un túnel.

Primero, [descargue](https://emulator.botframework.com) e instale el emulador. Una vez finalizada la descarga, inicie el archivo ejecutable y complete el proceso de instalación.

### <a name="start-your-bot"></a>Inicio del bot

Después de instalar el emulador, navegue al directorio de su bot en una ventana de consola e inicie su bot:

```nodejs
node app.js
```
   
El bot se ejecuta ahora localmente.

### <a name="start-the-emulator-and-connect-your-bot"></a>Inicio del emulador y conexión del bot
Después de iniciar su bot, inicie el emulador y, a continuación, conecte el bot:

1. Haga clic en el vínculo **create a new bot configuration** (Crear configuración de bot) en la ventana del emulador. 

2. Escriba `http://localhost:port-number/api/messages` en la barra de dirección, donde *port-number* coincide con el número de puerto mostrado en el explorador donde se ejecuta la aplicación.

3. Haga clic en Save (Guardar) y Connect (Conectar). No tendrá que especificar el identificador ni la contraseña de aplicación de Microsoft. Por ahora puede dejar estos campos en blanco. Más adelante obtendrá esta información al registrar el bot.

### <a name="try-out-your-bot"></a>Prueba del bot

Ahora que el bot se ejecuta localmente y está conectado al emulador, escriba algunos mensajes en el emulador para probar el bot.
Verá que el bot responde a cada mensaje que envía repitiendo el mensaje con el texto delante *"Dijo"*.

Ha creado correctamente su primer bot mediante el SDK de Bot Builder para Node.js.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [SDK de Bot Builder para Node.js](bot-builder-nodejs-overview.md)
