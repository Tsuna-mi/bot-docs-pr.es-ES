---
title: Pruebas del Bot Service en un chat en web | Microsoft Docs
description: Obtenga información sobre cómo probar Bot Service con el control de chat en web en Azure Portal.
keywords: probar en el chat en web, azure portal
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ms.openlocfilehash: 0c358f4e53f3fd64cce3635f644cc0f2f612e983
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39304735"
---
# <a name="test-in-web-chat"></a>Probar en Chat en web
Bot Service incluye el [control Chat en web](bot-service-channel-connect-webchat.md) que le ayudará a probar cómodamente el bot. 

## <a name="test-a-bot-in-the-azure-portal-with-web-chat"></a>Prueba de un bot en Azure Portal con el Chat en web
Inicie sesión en [Azure Portal](https://portal.azure.com) y abra la hoja para el bot. En la sección Bot Management (Administración de bot), haga clic en **Test in Web Chat** (Probar en Chat en web). Bot Service cargará el control Chat en web y se conectará a su bot.

![Interfaz de usuario de Test in Web Chat (Probar en Chat en web)](~/media/test-in-webchat/test-in-webchat.png)

Puede escribir texto para conversar con el bot. Si su bot es compatible con la voz, puede hacer clic en el botón de micrófono para hablar con él. Si su bot es compatible con los datos adjuntos, puede cargar un archivo adjunto, como una imagen. Obtenga información sobre cómo agregar características a su bot con el [SDK de Bot Builder](bot-builder-overview-getstarted.md).

> [!NOTE]
> Si el control Chat en web no se ha cargado por completo después de unos minutos, intente actualizar la página.

## <a name="next-steps"></a>Pasos siguientes
Ahora que sabe cómo probar el bot en Azure, obtenga información sobre cómo aprovechar al máximo Bot Framework Emulator para realizar pruebas y depuraciones de funcionalidades más detalladas.

> [!div class="nextstepaction"]
> [Bot Framework Emulator](bot-service-debug-emulator.md)