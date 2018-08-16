---
title: Descarga y reimplementación del código fuente para Bot Service | Microsoft Docs
description: Aprenda a descargar y publicar un bot de Bot Service.
keywords: descargar código fuente, volver a implementar, implementar, archivo zip, publicar
author: v-ducvo
ms.author: v-ducvo
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 03/08/2018
ms.openlocfilehash: 6d76388712ffeff94c56ba89b4bf4f4831caf45c
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305821"
---
# <a name="download-and-redeploy-bot-service-source-code"></a>Descarga y reimplementación del código fuente de Bot Service

Bot Service le permite descargar el proyecto de código fuente completo para el bot. Así, puede trabajar en el bot localmente con el IDE que prefiera. Cuando haya terminado de realizar los cambios, puede publicarlos en Azure. 

Este tema se mostrará cómo descargar el código fuente de un bot y publicar los cambios en Azure. 

## <a name="download-bot-source-code"></a>Descarga del código fuente del bot

Para desarrollar el bot localmente, haga lo siguiente:

1. Inicie sesión en Azure Portal y abra la hoja correspondiente al bot.
2. En la sección **BOT MANAGEMENT** (Administración de bots), haga cic en **Build** (Compilación).
3. Haga clic en **Descargar el archivo ZIP**. 

   ![Descarga del código fuente](~/media/azure-bot-build/download-zip-file.png)

4. Extraiga el archivo ZIP en un directorio local.
5. Vaya a la carpeta extraída y abra los archivos de origen en el IDE que prefiera.
6. Realice los cambios en la aplicación. Modifique los archivos de código fuente existentes o agregue otros nuevos al proyecto.

Cuando esté listo, puede volver a publicar los orígenes en Azure.

## <a name="publish-node-bot-source-code-to-azure"></a>Publicación del código fuente del bot de Node en Azure

Para instalar estos paquetes, vaya al directorio del proyecto desde un símbolo del sistema y ejecute los siguientes comandos de NPM.

**Nota:** estos paquetes solo deben agregarse una vez.

```console
npm install --save fs
npm install --save path
npm install --save request
npm install --save zip-folder
```

Ahora está listo para publicar el proyecto en Microsoft Azure. Para publicar el proyecto en Microsoft Azure, ejecute el siguiente comando de NPM en el símbolo del sistema:

```console
npm run azure-publish
```

> [!NOTE]
> Si experimenta un error después de este comando de NPM, es posible que deba agregar `"scripts": {"azure-publish": "node publish.js"}` a su archivo `package.json` y volver a ejecutarlo.

## <a name="publish-c-bot-source-code-to-azure"></a>Publicación del código fuente del bot de C# en Azure

La publicación de código de C# en Azure con Visual Studio es un proceso de dos pasos: en primer lugar, deberá configurar la configuración de la publicación. Después, se publican los cambios.

Para configurar la publicación desde Visual Studio, haga lo siguiente:

1. En Visual Studio, haga clic en  **Explorador de soluciones**.
2. Haga clic con el botón derecho en el nombre del proyecto y haga clic en **Publicar**. Se abre la ventana **Publicar**.
3. Haga clic en **Crear nuevo perfil**, haga clic en **Importar perfil** y haga clic en **Aceptar**.
4. Vaya a la carpeta del proyecto y, a continuación, vaya a la carpeta **PostDeployScripts** y seleccione el archivo que termine en **.PublishSettings**. Haga clic en **Abrir**.

El proyecto ahora está configurado para publicar los cambios en Azure.

Una vez que el proyecto está configurado, puede publicar el código fuente de bot en Azure mediante los pasos siguientes:

1. En Visual Studio, haga clic en  **Explorador de soluciones**.
2. Haga clic con el botón derecho en el nombre del proyecto y haga clic en **Publicar**.
3. Haga clic en el botón **Publicar** para publicar los cambios en Azure.

## <a name="next-steps"></a>Pasos siguientes
Ahora que sabe cómo crear un bot localmente, puede configurar una implementación continua para el bot.

> [!div class="nextstepaction"]
> [Configuración de la implementación continua](bot-service-build-continuous-deployment.md)
