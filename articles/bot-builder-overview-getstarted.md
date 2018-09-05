---
title: Comenzar a crear bots con Bot Builder | Microsoft Docs
description: Comience a crear bots eficaces con Bot Builder.
author: robstand
ms.author: kamrani
manager: kamrani
ms.topic: get-started-article
ms.prod: bot-framework
ms.date: 12/13/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: 3ee7843e64dfa95427ebcb132740eab3db281ffc
ms.sourcegitcommit: 2dc75701b169d822c9499e393439161bc87639d2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42904018"
---
# <a name="develop-bots-with-bot-builder"></a>Desarrollo de bots con Bot Builder



Bot Builder proporciona un SDK, bibliotecas, ejemplos y herramientas que ayudan a compilar y depurar los bots. Cuando se compila un bot con Bot Service, el bot está respaldado por el SDK de Bot Builder. También puede usar el SDK de Bot Builder para crear un bot desde cero mediante C# o Node.js. Bot Builder incluye Bot Framework Emulator para probar los bots, y el inspector de canales para previsualizar la experiencia del usuario del bot en distintos canales.

Si quiere crear un bot con cualquier lenguaje de programación, puede usar la API REST.

## <a name="bot-builder-sdk-for-net"></a>SDK de Bot Builder para .NET
El SDK de Bot Builder para .NET aprovecha C# para proporcionar una manera conocida de escribir bots para los desarrolladores de .NET. Es un marco eficaz para crear bots que pueden controlar tanto interacciones de forma libre como conversaciones más guiadas, donde el usuario selecciona entre valores posibles. 

El [inicio rápido de .NET](~/dotnet/bot-builder-dotnet-quickstart.md) le guiará a través de la creación de un bot con el SDK de Bot Builder para .NET.

EL SDK de Bot Builder para .NET está disponible como un [paquete NuGet](https://www.nuget.org/packages/Microsoft.Bot.Builder/).

> [!IMPORTANT]
> El SDK de Bot Builder para .NET requiere .NET Framework 4.6 o posterior. Si va a agregar el SDK a un proyecto existente que tiene como destino una versión anterior de .NET Framework, deberá actualizar primero el proyecto para que se dirija a .NET Framework 4.6.

Para instalar el SDK en un proyecto existente de Visual Studio, complete los pasos siguientes:

1. En el **Explorador de soluciones**, haga clic con el botón derecho en el nombre del proyecto y seleccione **Administrar paquetes NuGet…**.
2. En la pestaña **Examinar**, escriba "Microsoft.Bot.Builder" en el cuadro de búsqueda.
3. Seleccione **Microsoft.Bot.Builder** en la lista de resultados, haga clic en **Instalar** y acepte los cambios.

También puede descargar el [código fuente](https://github.com/Microsoft/BotBuilder/tree/master/CSharp) del SDK de Bot Builder para .NET desde GitHub.

### <a name="visual-studio-project-templates"></a>Plantillas de proyecto de Visual Studio
Descargue las plantillas de proyecto de Visual Studio para acelerar el desarrollo de bots.

* [Plantilla de bot de Visual Studio][bot-template] para el desarrollo de bots con C#
* [Plantilla de habilidades de Cortana de Visual Studio][cortana-template] para el desarrollo de habilidades de Cortana con C#

> [!TIP]
> <a href="/visualstudio/ide/how-to-locate-and-organize-project-and-item-templates" target="_blank">Obtenga más información</a> sobre cómo instalar plantillas de proyecto de Visual Studio 2017.

## <a name="bot-builder-sdk-for-nodejs"></a>SDK de Bot Builder para Node.js
El SDK de Bot Builder para Node.js proporciona una manera conocida de escribir bots para los desarrolladores de Node.js. Puede usarlo para crear una amplia variedad de interfaces de usuario conversacionales, desde indicaciones sencillas hasta conversaciones de forma libre. El SDK de Bot Builder para Node.js usa restify, un marco conocido para la creación de servicios web, para crear el servidor web del bot. El SDK también es compatible con Express. 

El [inicio rápido de Node.js](~/nodejs/bot-builder-nodejs-quickstart.md) le guiará a través de la creación de un bot con el SDK de Bot Builder para Node.js. 

El SDK de Bot Builder para Node.js está disponible como un paquete npm. Para instalar Bot Builder SDK para Node.js y sus dependencias, primero cree una carpeta para el bot, vaya a ella y ejecute el comando **npm** siguiente:

```nodejs
npm init
```

A continuación, instale el SDK de Bot Builder para Node.js y los módulos de <a href="http://restify.com/" target="_blank">Restify</a> mediante la ejecución de los comandos **npm** siguientes:

```nodejs
npm install --save botbuilder
npm install --save restify
```

También puede descargar el [código fuente](https://github.com/Microsoft/BotBuilder/tree/master/Node) del SDK de Bot Builder para Node.js desde GitHub.

## <a name="rest-api"></a>API DE REST

Puede crear un bot con cualquier lenguaje de programación mediante la API REST de Bot Framework. El [inicio rápido de REST](rest-api/bot-framework-rest-connector-quickstart.md) le guiará a través de la creación de un bot con REST.

Tres de las API REST están en Bot Framework:

 - La [API REST de Bot Connector][connectorAPI] permite que el bot envíe y reciba mensajes en los canales configurados en el [portal de Bot Framework](https://dev.botframework.com/). 

- La [API REST de Bot State][stateAPI] permite que el bot almacene y recupere el estado asociado a las conversaciones que se llevan a cabo a través de la API REST de Bot Connector.

- La [API REST de Direct Line][directLineAPI] le permite conectar su propia aplicación, por ejemplo, una aplicación cliente, un control Chat en web o una aplicación móvil, directamente a un bot único.

## <a name="bot-framework-emulator"></a>Bot Framework Emulator
El Bot Framework Emulator es una aplicación de escritorio que le permite probar y depurar sus bots de forma local o remota. Con el emulador, puede chatear con el bot e inspeccionar los mensajes que este envía y recibe. [Obtenga más información sobre el emulador de Bot Framework](~/bot-service-debug-emulator.md) y [descargue el emulador](http://emulator.botframework.com) para su plataforma.

## <a name="bot-framework-channel-inspector"></a>Bot Framework Channel Inspector
Vea rápidamente lasqué aspecto tendrán las características del bot en los distintos canales con Bot Framework [Channel Inspector](bot-service-channel-inspector.md).

[bot-template]: http://aka.ms/bf-bc-vstemplate
[cortana-template]: https://aka.ms/bf-cortanaskill-template


[connectorAPI]: https://docs.botframework.com/en-us/restapi/connector/#navtitle
 
[stateAPI]: https://docs.botframework.com/en-us/restapi/state/#navtitle

[directLineAPI]: https://docs.botframework.com/en-us/restapi/directline3/#navtitle
