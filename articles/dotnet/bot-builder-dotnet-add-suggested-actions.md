---
title: Incorporación de acciones sugeridas a mensajes | Microsoft Docs
description: Obtenga información sobre cómo agregar acciones sugeridas a mensajes mediante el SDK de Bot Builder para .NET.
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 03/13/2018
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: 63e5f4b8ac86b6b0e29d09796dbe74295bf3e213
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39304914"
---
# <a name="add-suggested-actions-to-messages"></a>Incorporación de acciones sugeridas a mensajes
> [!div class="op_single_selector"]
> - [.NET](../dotnet/bot-builder-dotnet-add-suggested-actions.md)
> - [Node.js](../nodejs/bot-builder-nodejs-send-suggested-actions.md)
> - [REST](../rest-api/bot-framework-rest-connector-add-suggested-actions.md)

[!INCLUDE [Introduction to suggested actions](../includes/snippet-suggested-actions-intro.md)]

> [!TIP]
> Use el [Inspector de canales][channelInspector] para ver la apariencia de las acciones sugeridas y cómo funcionan en varios canales.

## <a name="send-suggested-actions"></a>Enviar acciones sugeridas

Para agregar acciones sugeridas a un mensaje, establezca la propiedad `SuggestedActions` de la actividad en una lista de objetos [CardAction][cardAction] que representan los botones que se presentarán al usuario. 

En este ejemplo de código se muestra cómo crear un mensaje que presenta tres acciones sugeridas al usuario:

[!code-csharp[Add suggested actions](../includes/code/dotnet-add-suggested-actions.cs#addSuggestedActions)]

Cuando el usuario pulse una de las acciones sugeridas, el bot recibirá un mensaje del usuario que contendrá el elemento `Value` de la acción correspondiente.

## <a name="additional-resources"></a>Recursos adicionales

- [Preview features with the Channel Inspector][inspector] (Vista previa de las características con el Inspector de canales)
- [Activities overview](bot-builder-dotnet-activities.md) (Introducción a las actividades)
- [Create messages](bot-builder-dotnet-create-messages.md) (Creación de mensajes)
- <a href="https://docs.botframework.com/en-us/csharp/builder/sdkreference/dc/d2f/class_microsoft_1_1_bot_1_1_connector_1_1_activity.html" target="_blank">Activity class</a> (Clase Activity)
- <a href="/dotnet/api/microsoft.bot.connector.imessageactivity" target="_blank">IMessageActivity interface</a> (Interfaz IMessageActivity)
- <a href="/dotnet/api/microsoft.bot.connector.cardaction" target="_blank">CardAction class</a> (Clase CardAction)
- <a href="/dotnet/api/microsoft.bot.connector.suggestedactions" target="_blank">SuggestedActions class</a> (Clase SuggestedActions)

[cardAction]: /dotnet/api/microsoft.bot.connector.cardaction

[inspector]: ../bot-service-channel-inspector.md

[channelInspector]: ../bot-service-channel-inspector.md


