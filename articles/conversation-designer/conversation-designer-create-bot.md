---
title: Crear un bot de Conversation Designer | Microsoft Docs
description: Obtenga información sobre cómo crear un bot con Conversation Designer.
author: v-ducvo
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ROBOTS: NoIndex, NoFollow
ms.openlocfilehash: 1b33d08e56bf8ae473b28cf1cf3a26d8138ce861
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39304917"
---
# <a name="create-a-new-conversation-designer-bot"></a>Cree un bot de Conversation Designer
> [!IMPORTANT]
> Conversation Designer no está disponible para todos los clientes todavía. Habrá más información sobre la disponibilidad de Conversation Designer más adelante este año.

Este tutorial le guiará paso a paso para crear un nuevo bot de Conversation Designer. 

## <a name="prerequisites"></a>Requisitos previos

- Conversation Designer requiere una suscripción de Azure. Empiece por <a href="https://azure.microsoft.com/en-us/" target="_blank">aquí</a>.
- Si aún no lo ha hecho, asegúrese de iniciar sesión en el [portal de LUIS](https://luis.ai) al menos una vez después de que haya creado una cuenta con ellos.
- Familiaridad con la programación de JavaScript. Las funciones de script personalizadas se escriben en JavaScript.
- Microsoft Edge o Google Chrome

## <a name="create-a-conversation-designer-bot"></a>Crear un bot de Conversation Designer

Para crear un bot de Conversation Designer, siga estos pasos:
1. Vaya a https://dev.botframework.com/ e inicie sesión. Use la dirección de correo electrónico que proporcionó a Microsoft Corporation para participar en esta versión preliminar privada.
2. Haga clic en **Crear un bot** en el panel de navegación de la parte superior derecha. 
3. Haga clic en **Crear** para *crear un bot con Conversation Designer*.
4. Seleccione una de los muchos [bots de ejemplo](conversation-designer-sample-bots.md) para empezar. Haga clic en **Next**. Si no está seguro de qué **bot de ejemplo** utilizar, elija el que más se parezca al bot que quiera crear. Más adelante podrá cambiar a otro **bot de ejemplo**.
5. Complete todos los campos y haga clic en **crear bots**; esta operación tarda aproximadamente 2 minutos en completar el aprovisionamiento del bot. 

## <a name="bot-provisioning"></a>Aprovisionamiento de bots

Las siguientes características de Azure se aprovisionan automáticamente al crear un bot de Conversation Designer: 

1. Grupo de recursos de Azure con el nombre del bot especificado
2. Azure App Service
3. Plan de Azure App Service 
4. Cuenta de Azure Storage
5. Application Insights 
6. Suscripción de Cognitive Services para [LUIS.ai](https://luis.ai). Se crea una aplicación de LUIS con la opción **Control de bots** (además de una cadena generada aleatoriamente) como el nombre de la aplicación.
7. Aplicación individual de la cuenta de Microsoft. [Más información](https://apps.dev.microsoft.com/#/appList)

## <a name="welcome-message"></a>Mensaje de bienvenida

Una vez aprovisionado el bot, Conversation Designer abrirá la página **Compilar** del bot. Aparecerá un mensaje de bienvenida con información que le ayudará a empezar a trabajar. Explore esas opciones o cierre el mensaje y empiece a trabajar en su bot. Puede volver al mensaje de bienvenida haciendo clic en los puntos suspensivos (**...**) del panel de navegación superior izquierdo, y seleccionando la opción **Bienvenida...**.

## <a name="next-step"></a>Paso siguiente
> [!div class="nextstepaction"]
> [Guardar bot](conversation-designer-save-bot.md)

## <a name="additional-resources"></a>Recursos adicionales
* Obtenga más información sobre las [tareas](conversation-designer-tasks.md)
* Obtenga más información sobre el [SDK de Bot Builder para Node](../nodejs/index.md) 
