---
title: Estado y almacenamiento | Microsoft Docs
description: Describe el administrador de estado, el estado de la conversación y el estado del usuario dentro del SDK de Bot Builder.
keywords: LUIS, estado de la conversación, estado del usuario, almacenamiento, administración del estado
author: DeniseMak
ms.author: v-demak
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 02/15/2018
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: e25ecec3aec1cdebe3b9eae4bff0d3c434cb610b
ms.sourcegitcommit: 1abc32353c20acd103e0383121db21b705e5eec3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/21/2018
ms.locfileid: "42756411"
---
# <a name="state-and-storage"></a>Estado y almacenamiento
[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]

Una clave del diseño correcto de bots consiste en realizar el seguimiento del contexto de una conversación, para que el bot recuerde cosas como las respuestas a preguntas anteriores.
En función del uso previsto del bot, es posible que incluso sea necesario realizar el seguimiento de la información de estado o almacenamiento durante más tiempo que la duración de la conversación.
El *estado* de un bot es la información que recuerda con el fin de responder de forma adecuada a los mensajes entrantes. El SDK de Bot Builder proporciona clases para almacenar y recuperar datos de estado como un objeto asociado a un usuario o una conversación.

* Las **propiedades de la conversación** ayudan al bot a realizar el seguimiento de la conversación actual que tiene con el usuario. Si es necesario que el bot complete una secuencia de pasos o que cambie entre temas de conversación, se pueden usar las propiedades de la conversación para administrar los pasos en una secuencia o realizar el seguimiento del tema actual. Como las propiedades de la conversación reflejan el estado de la conversación actual, normalmente se borran al final de una conversación, cuando el bot recibe una actividad de _fin de la conversación_.
* Las **propiedades de usuario** se pueden usar para muchos propósitos, como determinar dónde dejó la conversación anterior el usuario o simplemente saludar a un usuario por su nombre cuando regrese. Si almacena las preferencias del usuario, puede usar esa información para personalizar la conversación la próxima vez que chatee. Por ejemplo, podría alertar al usuario sobre un artículo de noticias sobre un tema que le interesa, o bien cuando haya una cita disponible. Debe borrarlas si el bot recibe una actividad _eliminar datos de usuario_.

Puede usar [Almacenamiento](bot-builder-howto-v4-storage.md) para leer y escribir en el almacenamiento persistente. Esto permite que el bot haga cosas como actualizar recursos compartidos, registrar confirmaciones de asistencia o votos, o bien leer datos meteorológicos históricos. Al igual que una aplicación usa el almacenamiento para lograr sus objetivos, el bot puede hacerlo dentro de la conversación con el usuario.

<!-- 
*Conversation state* pertains to the current conversation that the user is having with your bot. When the conversation ends, your bot deletes this data.

You can also store *user state* that persists after a conversation ends. For example, if you store a user's preferences, you can use that information to customize the conversation the next time you chat. For example, you might alert the user to a news article about a topic that interests her, or alert a user when an appointment becomes available. 
-->

<!-- You should generally avoid saving state using a global variable or function closures.
Doing so will create issues when you want to scale out your bot. Instead, use the conversation state and user state middleware that the BotBuilder SDK provides --> 


## <a name="types-of-underlying-storage"></a>Tipos de almacenamiento subyacente

El SDK proporciona software intermedio de administrador de estado de bot para conservar el estado del usuario y la conversación. Se puede acceder al estado mediante el contexto del bot. Este administrador de estado puede usar Azure Table Storage, almacenamiento de archivos o almacenamiento en memoria como el almacenamiento de datos subyacente. También puede crear sus propios componentes de almacenamiento para el bot.

Los bots creados con Azure Table Storage se pueden diseñar para que sean sin estado y escalables en varios nodos de proceso.

> [!NOTE] 
> El almacenamiento de archivos y en memoria no se escalará en todos los nodos.

## <a name="writing-directly-to-storage"></a>Escritura directa en el almacenamiento

También puede usar el SDK de Bot Builder para leer y escribir datos directamente en el almacenamiento, sin usar software intermedio ni el contexto del bot. Esto puede ser adecuado para los datos que usa el bot, que provienen de un origen externo al flujo de conversación del bot.

Por ejemplo, suponga que el bot permite al usuario solicitar el informe meteorológico y el bot lo recupera para una fecha concreta, leyéndolo de una base de datos externa. El contenido de la base de datos meteorológicos no depende de la información del usuario ni del contexto de la conversación, por lo que se podría leer directamente desde el almacenamiento en lugar de usar el administrador de estado.  Vea [Escritura directa en el almacenamiento](bot-builder-howto-v4-storage.md) para obtener un ejemplo.

## <a name="next-steps"></a>Pasos siguientes

A continuación, se describirá la forma de procesar las actividades, en profundidad, y cómo responderlas.

> [!div class="nextstepaction"]
> [Procesamiento de actividades](bot-builder-concept-activity-processing.md)

## <a name="additional-resources"></a>Recursos adicionales

- [How to save state](bot-builder-howto-v4-state.md) (Cómo guardar el estado)
- [Escritura directa en el almacenamiento](bot-builder-howto-v4-storage.md)
