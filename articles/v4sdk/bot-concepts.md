---
title: Canales y Bot Connector Service | Microsoft Docs
description: Conceptos clave de Bot Builder SDK.
keywords: actividades, conversación
author: jonathanfingold
ms.author: jonathanfingold
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 03/28/2018
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: eaf1a8f714ea8711985b732f797951d241abbca2
ms.sourcegitcommit: 44f100a588ffda19c275b118f4f97029f12d1449
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/26/2018
ms.locfileid: "42928303"
---
# <a name="channels-and-the-bot-connector-service"></a>Canales y Bot Connector Service

[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]

Los canales son el punto de conexión desde el que un usuario se conecta a nuestro bot, como Facebook, Skype, correo electrónico, Slack, etc. Bot Connector Service, que se configura a través de [Azure Portal](https://portal.azure.com), conecta su bot a estos canales y facilita la comunicación entre el bot y el usuario. 

Además de los canales estándar proporcionados con Bot Connector Service, también puede conectar el bot a su propia aplicación cliente con [línea directa](bot-builder-howto-direct-line.md) como canal.

Bot Connector Service le permite desarrollar su bot de forma independiente del canal mediante la normalización de los mensajes que el bot envía a un canal. Esto implica convertirlo del esquema del generador de bots al esquema del canal. Sin embargo, si el canal no es compatible con todos los aspectos del esquema del generador de bots, el servicio intentará convertir el mensaje a un formato que sea compatible con el canal. Por ejemplo, si el bot envía al canal SMS un mensaje que contiene una tarjeta con los botones de acción, el conector puede enviar la tarjeta como imagen e incluir las acciones como vínculos en el texto del mensaje.

## <a name="activities-and-conversations"></a>Actividades y conversaciones


Bot Connector Service usa JSON para intercambiar información entre el bot y el usuario, y el Bot Builder SDK encapsula esta información en un objeto de actividad específico del idioma. Los [tipos de actividad](../bot-service-activities-entities.md) se mencionaron al analizar la [interacción con el bot](bot-builder-basics.md#interaction-with-your-bot), donde el tipo más común de actividad es un mensaje, pero también son importantes los otros tipos de actividad. Se incluyen una actualización de conversación, actualización de la relación de contacto, eliminar datos de usuario, fin de la conversación, escribir, reacción del mensaje y algunas otras actividades específicas del bot que probablemente el usuario nunca verá. La información detallada sobre cada uno de ellos y cuándo se producen puede encontrarse en nuestro contenido de referencia de las actividades.

Cada turno y su actividad asociada pertenecen a una **conversación** lógica, que representa una interacción entre uno o varios de los bots y un usuario específico o un grupo de usuarios. Una conversación es específica de un canal y tiene un identificador exclusivo para ese canal.

## <a name="next-steps"></a>Pasos siguientes

Ahora que está familiarizado con algunos conceptos clave de un bot, vamos a profundizar en las diferentes formas de conversación que puede usar nuestro bot.

> [!div class="nextstepaction"]
> [Formularios de conversación](bot-builder-conversations.md)
