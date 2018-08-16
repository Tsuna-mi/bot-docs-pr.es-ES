---
title: Importación y exportación de un bot de Conversation Designer | Microsoft Docs
description: Aprenda a importar y a exportar bots de Conversation Designer.
author: v-ducvo
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ROBOTS: NoIndex, NoFollow
ms.openlocfilehash: ed055cb0f75d148e6a0dd3248366851901d9a675
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39306256"
---
# <a name="export-and-import-a-conversation-designer-bot"></a>Importación y exportación de un bot de Conversation Designer

Los bots de Conversation Designer se pueden exportar a su equipo como un archivo .zip. Esto le permite realizar copias de seguridad del bot. Más adelante, puede restaurar el bot a un estado anterior mediante cualquiera de los archivos .zip exportados. 

## <a name="export-a-conversation-designer-bot"></a>Exportación de un bot de Conversation Designer

La exportación le permite guardar el estado actual del bot de Conversation Designer en el equipo local. 

Para exportar un bot de Conversation Designer, haga lo siguiente:
1. En la esquina superior derecha del panel de navegación, haga clic en el botón de puntos suspensivos (**...**).
2. Haga clic en **Exportar**. El servidor recopilará la información necesaria y devolverá una opción para guardar el archivo zip.
3. Guarde el archivo .zip exportado en un equipo local.

En este momento, el bot ya está guardado en un archivo .zip en el equipo. Más adelante, puede restaurar el bot a este estado [importándolo](#import-a-conversation-designer-bot) de nuevo en el bot.

## <a name="import-a-conversation-designer-bot"></a>Importación de un bot de Conversation Designer

La importación le permite restaurar el bot de Conversation Designer a un estado anterior. La importación reemplazará el bot actual por el bot importado. Si no desea perder lo que tiene en el bot actual, asegúrese de [exportarlo](#export-a-conversation-designer-bot) antes de realizar una operación de importación.

Para importar un bot de Conversation Designer, haga lo siguiente:
1. En la esquina superior derecha del panel de navegación, haga clic en el botón de puntos suspensivos (**...**).
2. Haga clic en **Import**. 
3. Si quiere guardar el trabajo en el bot actual, elija la opción **Backup and import** (Hacer una copia de seguridad e importar). Esta opción guardará el bot actual en el equipo local y, a continuación, le solicitará la ubicación del archivo .zip para importar. En caso contrario, elija **Import without backing up** (Importar sin hacer una copia de seguridad).

En ese momento se importa el bot.

> [!NOTE]
> Solo puede importar los bots que haya exportado Conversation Designer.

## <a name="import-a-luis-app-into-a-conversation-designer-bot"></a>Importación de una aplicación de LUIS en un bot de Conversation Designer

Si tiene una aplicación de LUIS y desea usarla en su bot de Conversation Designer, puede importar esa aplicación en su bot. Desde un punto de vista conceptual, este proceso requiere que exporte la aplicación de LUIS y el bot de Conversation Designer y, posteriormente, intercambie el contenido del archivo **luis.model** por el del archivo **luis.json**. A continuación, importe los cambios en el bot de Conversation Designer. Básicamente, va a reemplazar las intenciones de LUIS del bot por las de la aplicación de LUIS. Por este motivo, se recomienda realizar esta operación de importación antes de empezar a personalizar las intenciones de LUIS del bot; en caso contrario, esta operación de importación sobrescribirá todo el trabajo.

> [!NOTE]
> Si va a realizar cambios en una aplicación de [LUIS](https://luis.ai) que está asociada a su bot (cada bot de Conversation Designer tiene una aplicación de LUIS correspondiente), no es necesario llevar a cabo estos pasos. En su lugar, todo lo que necesita hacer es ir al bot de Conversation Designer y hacer clic en [**Guardar**](conversation-designer-save-bot.md).

Para importar la aplicación de LUIS en el bot de Conversation Designer, haga lo siguiente:

1. Desde [LUIS.ai](https://luis.ai), abra la aplicación de LUIS y, a continuación, haga clic en **CONFIGURACIÓN**.
2. Elija las **versiones** de la aplicación que desea usar y haga clic en el icono de acción **{ }**. Esta acción descargará el archivo **luis.json** en el equipo local. 
3. En Conversation Designer, [cree un nuevo bot](conversation-designer-create-bot.md#create-a-conversation-designer-bot) o abra uno ya existente.
4. [Exporte](#export-a-conversation-designer-bot) el bot. Esta acción exportará el bot como un archivo .zip en el equipo.
5. Vaya al archivo .zip exportado y extráigalo.
6. Abra el archivo **luis.model** en un editor de texto y reemplace el contenido de este archivo por el contenido del archivo **luis.json**. Guarde el archivo.
7. Comprima la carpeta del bot de Conversation Designer.
8. [Importe](#import-a-conversation-designer-bot) el bot de nuevo en el bot de Conversation Designer.

El bot ahora puede usar las nuevas intenciones de LUIS que acaba de importar.

## <a name="next-step"></a>Paso siguiente
> [!div class="nextstepaction"]
> [Tareas](conversation-designer-tasks.md)
