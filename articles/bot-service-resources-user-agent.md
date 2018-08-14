---
title: Solicitudes de agente de usuario de Bot Framework | Microsoft Docs
description: Descripción de las solicitudes de Bot Framework al servidor web.
author: JohnD-ms
ms.author: v-jodemp
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ms.openlocfilehash: 900881eea35af5f8137b99ccd5e50739342bf5e8
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39306448"
---
# <a name="bot-framework-user-agent-requests"></a>Solicitudes de agente de usuario de Bot Framework

Si está leyendo este mensaje, es probable que haya recibido una solicitud de un servicio de Microsoft Bot Framework. Esta guía le ayudará a comprender la naturaleza de estas solicitudes y proporcionará los pasos para detenerlas, si es lo que quiere.

Si ha recibido una solicitud de nuestro servicio, es probable que tuviera un encabezado de agente de usuario con un formato similar al de la cadena siguiente:

```User-Agent: BF-DirectLine/3.0 (Microsoft-BotFramework/3.0 +https://botframework.com/ua)```

La parte más importante de esta cadena es el identificador **Microsoft-BotFramework**, que usa Microsoft Bot Framework, una colección de herramientas y servicios que permite a los desarrolladores de software independientes crear y trabajar con bots propios.

Si es un desarrollador de bot, es posible que ya sepa por qué estas solicitudes se dirigen al servicio. Si no es así, siga leyendo para obtener más información.

## <a name="why-is-microsoft-contacting-my-service"></a>¿Por qué Microsoft se pone en contacto con mi servicio?

Bot Framework conecta a los usuarios de servicios de chat como Skype y Facebook Messenger a bots, que son servidores web con API REST que se ejecutan en puntos de conexión a los que se pueda acceder a través de Internet. Las llamadas HTTP a los bots (también denominadas llamadas de webhook) solo se envían a las direcciones URL especificadas por un desarrollador de bot que se haya registrado en el portal para desarrolladores de Bot Framework.

Si recibe solicitudes no solicitadas desde los servicios de Bot Framework a su servicio web, es probable que un desarrollador haya escrito su dirección URL, de forma accidental o deliberada, como devolución de llamada de webhook para su bot.

## <a name="to-stop-these-requests"></a>Para detener estas solicitudes

Para obtener asistencia con el fin de evitar que las solicitudes no deseadas alcancen el servicio web, póngase en contacto con [bf-reports@microsoft.com](mailto://bf-reports@microsoft.com).
