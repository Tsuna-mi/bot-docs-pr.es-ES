---
title: Conexión de un bot a Skype | Microsoft Docs
description: Obtenga información sobre cómo configurar un bot para acceder a través de la interfaz de Skpye.
keywords: skype, canales de bot, configurar skype, publicar, conectarse a canales
author: v-ducvo
ms.author: RobStand
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 2/1/2018
ms.openlocfilehash: 5dc4063125855113f813b8873b01df84c90e197e
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305089"
---
# <a name="connect-a-bot-to-skype"></a>Conexión de un bot a Skype

Skype le mantiene conectado con los usuarios a través de mensajería instantánea, teléfono y videollamadas. Para ampliar esta funcionalidad, cree bots que los usuarios pueden detectar y con los que puedan interactuar a través de la interfaz de Skype.

Para agregar el canal de Skype, abra el bot en [Azure Portal](https://portal.azure.com/), haga clic en la hoja **Canales** y luego en **Skype**. Esto le llevará a la página **Configurar Skype**. Rellene toda la información necesaria sobre el bot y haga clic en **Guardar** para conectarse al canal de Skype. Acepte los **Términos de servicio**, y el canal de Skype se agregará al bot.

![Agregar el canal de Skype](~/media/channels/skype-addchannel.png)

## <a name="web-control"></a>Control web

Para insertar el bot en su sitio web, puede obtener el código haciendo clic en el botón **Obtener código para insertar** desde la sección **Control web**.

## <a name="messaging"></a>Mensajería

En esta sección configura la manera en que el bot envía y recibe mensajes de Skype.

## <a name="calling"></a>Llamada

En esta sección se configura la característica de llamadas de Skype en el bot. Puede especificar si **Llamada** está habilitada para el bot, y si está habilitada, si se usará la funcionalidad IVR o de multimedia en tiempo real.

## <a name="groups"></a>Grupos

En esta sección se configura si el bot puede agregarse a un grupo y cómo se comporta en un grupo para la mensajería; también se utiliza para habilitar las videollamadas grupales para los bots de llamada.

## <a name="publish"></a>Publicar

En esta sección se establece la configuración de publicación del bot. Todos los campos con * son campos obligatorios.

Los bots en **versión preliminar** están limitados a 100 contactos. Si necesita más de 100 contactos, envíe el bot para revisión. Al hacer clic en **Enviar para revisión**, hará automáticamente que el bot se pueda buscar en Skype si se lo acepta. Si no se aprueba la solicitud, se le notificará qué debe cambiar para que pueda aprobarse.

## <a name="next-steps"></a>Pasos siguientes

* [Skype Empresarial](bot-service-channel-connect-skypeforbusiness.md)
