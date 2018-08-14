---
title: Configuración de implementación continua para Bot Service | Microsoft Docs
description: Obtenga información sobre cómo configurar una implementación continua desde el control de código fuente para una instancia de Bot Service.
keywords: implementación continua, publicar, implementar, azure portal
author: v-ducvo
ms.author: v-ducvo
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 03/08/2018
ms.openlocfilehash: 596d264c4df72959c71ab353e5038175fc2bed31
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305096"
---
# <a name="set-up-continuous-deployment"></a>Configurar la implementación continua

La implementación continua le permite desarrollar el bot localmente. La implementación continua es útil si su bot se inserta en un control de código fuente, como **GitHub** o **Visual Studio Team Services**. A medida que inserta los cambios en el repositorio de origen, los cambios se implementarán automáticamente en Azure.

> [!NOTE]
> Una vez configurada la implementación continua, el [editor de código en línea](bot-service-build-online-code-editor.md) se convierte en *SOLO LECTURA*.

En este tema se muestra cómo configurar la implementación continua para **GitHub** y **Visual Studio Team Services**.

## <a name="continuous-deployment-using-github"></a>Implementación continua con GitHub

Para configurar una implementación continua con GitHub, haga lo siguiente:

1. [Bifurque](https://help.github.com/articles/fork-a-repo/) el repositorio de GitHub que contiene el código que desea implementar en Azure.
2. En Azure Portal, vaya a la hoja **Compilar** del bot y haga clic en **Configure continuous deployment** (Configurar la implementación continua). 
3. Haga clic en **Configuración**.
   
   ![Configurar implementación continua](~/media/azure-bot-build/continuous-deployment-setup.png)

4. Haga clic en **Elegir origen** y elija **GitHub**.

   ![Elija GitHub](~/media/azure-bot-build/continuous-deployment-setup-github.png)

5. Haga clic en **Autorización**, luego en **Autorizar** y siga los avisos para proporcionar autorización para que Azure acceda a su cuenta de GitHub.

La configuración de la implementación continua con GitHub está completa. Cada vez que confirme, los cambios se implementarán automáticamente en Azure.

## <a name="continuous-deployment-using-visual-studio"></a>Implementación continua con Visual Studio

1. Desde la hoja **Compilar** del bot, haga clic en **Configure continuous deployment** (Configurar la implementación continua). 
2. Haga clic en **Configuración**.
   
   ![Configurar implementación continua](~/media/azure-bot-build/continuous-deployment-setup.png)

3. Haga clic en **Elegir origen** y elija **Visual Studio Team Services**.

   ![Elija Visual Studio Team Services:](~/media/azure-bot-build/continuous-deployment-setup-vs.png)

4. Haga clic en **elija su cuenta** y elija una cuenta.

> [!NOTE]
> Si no ve su cuenta, asegúrese de que esté vinculada a su suscripción a Azure.
> Para obtener más información, consulte [Linking your VSTS account to your Azure subscription](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App#linking-your-vsts-account-to-your-azure-subscription) (Cincular su cuenta de VSTS con su suscripción a Azure).

5. Haga clic en **Elegir un proyecto** y elija un proyecto.

> [!NOTE]
> Solo se admiten proyectos VSTS Git.

6. Haga clic en **Elegir rama** y elija una rama para crear.
7. Haga clic en **Aceptar** para completar el proceso de configuración.

   ![Configuración de Visual Studio](~/media/azure-bot-build/continuous-deployment-setup-vs-configuration.png)

La configuración de la implementación continua con Visual Studio Team Services está completa. Cada vez que confirme, los cambios se implementarán automáticamente en Azure.

## <a name="disable-continuous-deployment"></a>Deshabilitación de la implementación continua

Si bien el bot está configurado para la implementación continua, no puede usar el editor de código en línea para realizar cambios en el bot. Si desea usar el editor de código en línea, puede deshabilitar temporalmente la implementación continua.

Para deshabilitar la implementación continua, haga lo siguiente:

1. Desde la hoja **Compilar** del bot, haga clic en **Configure continuous deployment** (Configurar la implementación continua). 
2. Haga clic en **Desconectar** para deshabilitar la implementación continua. Para volver a habilitar la implementación continua, repita los pasos de las secciones anteriores correspondientes.

## <a name="next-steps"></a>Pasos siguientes
Ahora que el bot se ha configurado para la implementación continua, pruebe el código con el Chat en web en línea.

> [!div class="nextstepaction"]
> [Probar en Chat en web](bot-service-manage-test-webchat.md)
