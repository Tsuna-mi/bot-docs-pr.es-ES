---
title: Configuración de un bot para ejecutarlo en uno o más canales | Microsoft Docs
description: Obtenga información sobre cómo configurar un bot para ejecutarlo en uno o más canales mediante el Portal de Framework Bot.
keywords: canales de bot, configurar, cortana, facebook messenger, kik, slack, skype, azure portal
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ms.openlocfilehash: cb682bf77f801c98d00deffa0fc63249962248cd
ms.sourcegitcommit: dcbc8ad992a3e242a11ebcdf0ee99714d919a877
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/30/2018
ms.locfileid: "39352924"
---
# <a name="connect-a-bot-to-channels"></a>Conexión de un bot a canales

Un canal es una conexión entre Bot Framework y aplicaciones de comunicación. Un bot se configura para conectarse a los canales en los que quiere que esté disponible. Por ejemplo, un bot conectado al canal de Skype se puede agregar a una lista de contactos y los usuarios pueden interactuar con él en Skype. 

Los canales incluyen muchos servicios populares, como [Cortana](bot-service-channel-connect-cortana.md), [Facebook Messenger](bot-service-channel-connect-facebook.md), [Kik](bot-service-channel-connect-kik.md) y [Slack](bot-service-channel-connect-slack.md), además de otros. [Skype](https://dev.skype.com/bots) y Chat en web están preconfigurados de forma automática. 

La conexión a los canales es rápida y sencilla en [Azure Portal](https://portal.azure.com).

## <a name="get-started"></a>Introducción

Para la mayoría de los canales, se debe proporcionar información de configuración de canal para ejecutar el bot en el canal. La mayoría de los canales requieren que el bot tenga una cuenta en el canal y otros, como Facebook Messenger, requieren que también tenga una aplicación registrada con el canal.

Para configurar el bot para que se conecte a un canal, siga los pasos siguientes:

1. Inicie sesión en <a href="https://portal.azure.com" target="_blank">Azure Portal</a>.
1. Seleccione el bot que quiera configurar.
3. En la hoja Servicio de bots, haga clic en **Canales** en **Administración de bots**.
4. Haga clic en el icono del canal al que quiera agregar el bot.

![Conexión a los canales](~/media/channels/connect-to-channels.png)

Después de configurar el canal, los usuarios de ese canal pueden empezar a usar el bot.

## <a name="publish-a-bot"></a>Publicación de un bot

El proceso de publicación es diferente para cada canal.

[!INCLUDE [publishing](~/includes/snippet-publish-to-channel.md)]

