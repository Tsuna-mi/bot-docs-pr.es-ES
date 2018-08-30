---
title: Conceptos básicos de Bot Builder | Microsoft Docs
description: Estructura básica de Bot Builder SDK.
keywords: contexto de turno, estructura del bot, recepción de entrada
author: ivorb
ms.author: v-ivorb
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 04/18/2018
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: 34564b411f911ae82197d5a34cb954a103abe70b
ms.sourcegitcommit: 2dc75701b169d822c9499e393439161bc87639d2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42905419"
---
# <a name="basic-bot-structure"></a>Estructura básica del bot

[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]

Azure Bot Service y Bot Builder SDK proporcionan bibliotecas, ejemplos y herramientas que ayudan a compilar y depurar los bots. Pero antes de definirlos en detalle, es importante entender la estructura básica de un bot y cómo funciona todo en conjunto. Estos conceptos se aplican independientemente del lenguaje de programación que elija. En este artículo se proporcionan vínculos a contenido más detallado, así como una introducción al funcionamiento de un bot.

Examinemos desde el principio la estructura básica de un bot.

## <a name="creation-of-your-bot"></a>Creación de un bot

Un bot puede crearse de diversas maneras, por ejemplo, en [Azure Portal](~/bot-service-quickstart.md), en [Visual Studio](~/dotnet/bot-builder-dotnet-sdk-quickstart.md) o mediante herramientas de línea de comandos para [JavaScript](~/javascript/bot-builder-javascript-quickstart.md), [Java ](~/java/bot-builder-java-quickstart.md) o [Python](~/python/bot-builder-python-quickstart.md). Una vez creado, un bot se puede ejecutar en un equipo local, en Azure o en otro servicio en la nube. Todos los bots funcionan de manera muy parecida, independientemente de dónde se ejecuten o cómo se hayan creado.

## <a name="interaction-with-your-bot"></a>Interacción con el bot

Un bot no tiene intrínsecamente ninguna interfaz de usuario visible como la de un sitio web o una aplicación; en su lugar, un usuario interactúa con él a través de una [conversación](~/v4sdk/bot-concepts.md#activities-and-conversations). En función de la aplicación que se usa para conectar con el bot (que se llama [canal](~/v4sdk/bot-concepts.md), aunque aquí no hablaremos de esto), se envía y se recibe cierta información entre el usuario y el bot.

En el bot, cada unidad de información se denomina **actividad**, que puede tener varias formas diferentes. Las actividades incluyen tanto la comunicación originada por el usuario, llamada **mensaje**, como la información adicional incluida en otros [tipos de actividad](~/bot-service-activities-entities.md). Dicha información adicional puede incluir el hecho de que una entidad nueva se haya unido a la conversación o la haya abandonado, la finalización de una conversación, etc. El sistema subyacente se encarga de enviar estos tipos de comunicación procedentes de la conexión del usuario, sin necesidad de que este haga nada.

El bot recibe la comunicación y la encapsula en un objeto de actividad con el tipo correcto para entregárselo al código del bot. Los demás tipos de actividad proporcionan información útil, aunque la actividad más interesante y más común es la actividad de **mensaje** del usuario.

Cada actividad que recibe el bot comienza un turno, que en breve describiremos con más detalle.

## <a name="receiving-user-input"></a>Recibir la entrada de usuario

Cuando recibimos una actividad de mensaje del usuario, es necesario entender lo que nos está enviando. La manera más sencilla consiste en simplemente hacer coincidir el texto del mensaje entrante con una cadena. En función de la cadena que sea, podemos decidir hacer algo, según lo que el bot intente conseguir. Esto puede incluir responder al usuario, actualizar una variable o recurso, guardarlo en el [almacenamiento](~/v4sdk/bot-builder-storage-concept.md) o un procesamiento similar.

Existen formas más complejas de reconocer la entrada del usuario, por ejemplo, mediante el uso de [LUIS](~/v4sdk/bot-builder-concept-luis.md) o [QnA Maker](~/v4sdk/bot-builder-howto-qna.md), pero la coincidencia de cadenas es lo más sencillo.

## <a name="defining-a-turn"></a>Definir un turno

[!INCLUDE [Define a turn](~/includes/snippet-definition-turn.md)]

El procesamiento de la actividad se administra a través del **adaptador**, como se describe con más detalle en [Procesamiento de actividades](~/v4sdk/bot-builder-concept-activity-processing.md). Cuando el adaptador recibe una actividad, crea un **contexto de turno** para proporcionar información sobre la actividad y proporcionar contexto al turno actual que se está procesando. Dicho contexto de turno existe mientras el turno esté activo y, después, se elimina, lo que indica que el turno ha finalizado.

El [contexto de turno](~/v4sdk/bot-builder-concept-activity-processing.md#turn-context) contiene información, y se usa el mismo objeto en todas las capas del bot. Esto es útil porque este objeto de contexto de turno puede (y debe) usarse para almacenar información que se podría necesitar más adelante en el turno.

> [!IMPORTANT]
> Todos los **turnos** son independientes entre sí, ya que se ejecutan por sí mismos, y es posible que se superpongan. El bot puede procesar a la vez varios turnos de distintos usuarios en canales diferentes. Cada turno tendrá su propio contexto de turno, pero conviene tener en cuenta la complejidad que se presenta en algunas situaciones.

## <a name="where-to-go-from-here"></a>Cómo continuar a partir de aquí

En este artículo no se incluye mucha información, por ejemplo, cómo [se procesan las actividades](~/v4sdk/bot-builder-concept-activity-processing.md), los diferentes [tipos de conversación](~/v4sdk/bot-builder-conversations.md), la realización de un seguimiento del [estado de la conversación](~/v4sdk/bot-builder-storage-concept.md) para mantener conversaciones inteligentes, etc. Los demás temas se basan en los conceptos básicos y abarcan el resto de nociones necesarias para entender los bots y Azure Bot Service. Puede consultar los pasos de las secciones siguientes para aumentar sus conocimientos, o bien ir directamente a lo que más le llame la atención, probar el [inicio rápido](~/bot-service-quickstart.md) para crear su primer bot o profundizar en el [desarrollo](~/v4sdk/bot-builder-howto-send-messages.md) de bots.

## <a name="next-steps"></a>Pasos siguientes

Bot Connector Service permite al bot comunicarse con usuarios de plataformas diferentes.

> [!div class="nextstepaction"]
> [Canales y Bot Connector Service](~/v4sdk/bot-concepts.md)
