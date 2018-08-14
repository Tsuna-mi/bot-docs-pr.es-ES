---
title: Conversaciones en Bot Builder SDK | Microsoft Docs
description: Describe qué es una conversación en Bot Builder SDK.
keywords: flujo de conversación, reconocimiento de intenciones, un solo turno, varios turnos
author: jonathanfingold
ms.author: jonathanfingold
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 04/11/2018
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: 679109cad2f7b0c0c5826a47884b98e1149cb380
ms.sourcegitcommit: f95702d27abbd242c902eeb218d55a72df56ce56
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/19/2018
ms.locfileid: "39306480"
---
# <a name="conversation-flow"></a>Flujo de conversación
[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]

Dado que un bot puede considerarse como una interfaz de usuario conversacional, el flujo de conversación es la manera en que se interactúa con el usuario, y puede adoptar diferentes formas. Un flujo de conversación correcto ayuda a mejorar la interacción del usuario y el rendimiento del bot.

Para diseñar del flujo de conversación de un bot es necesario decidir cómo responde el bot cuando el usuario le dice algo. Un bot reconoce en primer lugar la tarea o el tema de conversación basándose en un mensaje del usuario. Para determinar la tarea o el tema (lo que se denomina *intención*) asociado al mensaje de un usuario, el bot puede buscar palabras o patrones en el texto del mensaje, bien puede aprovechar servicios como [Language Understanding (LUIS)](bot-builder-concept-luis.md) y QnA Maker. 

Una vez que ha reconocido la intención del usuario, en función del escenario, el bot podría satisfacer la solicitud del usuario con una sola respuesta, con lo que completaría la conversación en un solo turno, o podría requerir una serie de turnos. Para los flujos de conversación de varios turnos, Bot Builder SDK proporciona [administración de estados](./bot-builder-howto-v4-state.md) para llevar un seguimiento de una conversación, [avisos](bot-builder-prompts.md) para pedir información y [cuadros de diálogo](bot-builder-dialog-manage-conversation-flow.md) para encapsular los flujos de conversación. 

En un bot complejo con varios subsistemas, usted podría usar varios servicios para reconocer la intención, uno para cada subcomponente del bot. La [herramienta de distribución](bot-builder-tutorial-dispatch.md) coloca en un mismo lugar los resultados de varios servicios cuando se combinan subsistemas de conversación en un bot. 
<!-- 
A conversation identifies a series of activities sent between a bot and a user on a specific channel and represents an interaction between one or more bots and either a _direct_ conversation with a specific user or a _group_ conversation with multiple users.
A bot communicates with a user on a channel by receiving activities from, and sending activities to the user.

- Each user has an ID that is unique per channel.
- Each conversation has an ID that is unique per channel.
- The channel sets the conversation ID when it starts the conversation.
- The bot cannot start a conversation; however, once it has a conversation ID, it can resume that conversation.
- Not all channels support group conversations.
-->

## <a name="single-turn-conversation"></a>Conversación de un solo turno

El flujo de conversación más sencillo es el de un solo turno. En un flujo de un solo turno, el bot finaliza su tarea en un turno, que consta de un mensaje del usuario y una respuesta del bot. 



<!-- 
The EchoBot sample in the BotBuilder SDK is a single-turn bot. Here are other examples of single turn conversation flow:
* A bot for getting the weather report, that just tells the user what the weather is, when they say "What's the weather?".
* An IoT bot that responds to "turn on the lights" by calling an IoT service. -->

<!-- The following isn't always true, it's a generalization --> El tipo más sencillo de bot de un solo turno no necesita llevar un seguimiento del estado de la conversación. Cada vez que recibe un mensaje, responde únicamente en función del contexto del mensaje entrante actual, sin tener conocimiento de turnos de conversación anteriores.

![Bot del tiempo de un solo turno](./media/concept-conversation/weather-single-turn.png)

Un bot del tiempo tiene un flujo de un solo turno. Simplemente le proporciona al usuario un informe meteorológico, sin necesidad de preguntar la ciudad o la fecha. Toda la lógica para mostrar el informe meteorológico se basa en el mensaje que el bot acaba de recibir. En cada turno de una conversación, el bot recibe un contexto de turno que puede usar para determinar qué hará a continuación y cómo fluye la conversación. 

## <a name="multiple-turns"></a>Varios turnos

La mayoría de los tipos de conversación no se puede completar en un solo turno, por lo que un bot también puede tener un flujo de conversación de varios turnos. Algunos escenarios que requieren varios turnos de conversación incluyen lo siguiente:

 * Un bot que le pide al usuario la información adicional que necesita para completar una tarea. El bot debe comprobar si cuenta con todos los parámetros para cumplir la tarea.
 * Un bot que guía al usuario a través de los pasos en un proceso, como la realización de un pedido. El bot debe llevar un seguimiento de dónde se encuentra el usuario en la secuencia de pasos.

Por ejemplo, un bot del tiempo tiene un flujo de varios turnos si responde a la pregunta "¿Qué tiempo hace?" preguntando por la ciudad.

![Bot del tiempo de varios turnos](./media/concept-conversation/weather-multi-turn.png)

Cuando el usuario responde al mensaje del bot en el que le pregunta por la ciudad y el bot recibe la respuesta "Seattle", el bot necesita tener contexto guardado para entender que el mensaje actual es la respuesta a una pregunta anterior y que forma parte de una solicitud para obtener el tiempo. Los bots de varios turnos llevan un seguimiento del estado para responder de forma adecuada a los mensajes nuevos.

<!--
```
// TBD: snippet showing receiving message and using ConversationProperties
```
-->

Vea [Managing state](bot-builder-storage-concept.md) (Administración del estado) para obtener información general sobre cómo administrar el estado y [How to use user and conversation properties](bot-builder-howto-v4-state.md) (Cómo usar las propiedades de usuario y conversación) para obtener un ejemplo.

> [!NOTE]
> Las conversaciones de varios turnos con clientes de la API de REST deben llevar un seguimiento de su propio estado, por ejemplo, en una base de datos o Table Storage. 

## <a name="conversation-topics"></a>Temas de conversación

Tal vez diseñe su bot para que controle más de un tipo de tareas. Por ejemplo, podría tener un bot que proporcione flujos de conversación diferentes para saludar al usuario, realizar un pedido, cancelar y obtener ayuda. Una manera de controlar este cambio en la conversación entre tareas o temas diferentes consiste en reconocer la intención (lo que el usuario quiere hacer) del mensaje actual. 

### <a name="recognize-intent"></a>Reconocimiento de intenciones

Bot Builder SDK proporciona _reconocedores_ que procesan cada mensaje entrante para determinar la intención, de modo que el bot pueda iniciar el flujo de conversación correspondiente. Antes de la _devolución de llamada de recepción_, los reconocedores examinan el contenido del mensaje del usuario para determinar la intención y, después, devuelven la intención al bot mediante el uso del objeto de contexto de turno dentro de la devolución de llamada de recepción, almacenada como la **intención principal** en el objeto de contexto de turno. 

El reconocedor que determina la **intención principal** puede usar simplemente expresiones regulares, Language Understanding (LUIS) u otra lógica que se haya desarrollado como software intermedio. Aquí se muestran ejemplos de posibles reconocedores:
   
* Configura un reconocedor mediante expresiones regulares para detectar cada vez que un usuario emplea la palabra "help" (ayuda).
* Usa Language Understanding (LUIS) para entrenar un servicio con ejemplos de las maneras en las que un usuario podría pedir ayuda y lo asigna a la intención "Help".
* Crea su propio software intermedio de reconocedor, que inspecciona las actividades entrantes y devuelve la intención "translate" cada vez que detecta un mensaje en otro idioma.

Para más información, vea [Language Understanding with LUIS](bot-builder-concept-luis.md) (Language Understanding con LUIS). <!-- TODO: ADD THIS TOPIC OR SNIPPET-->

### <a name="consider-how-to-interrupt-conversation-flow-or-change-topics"></a>Cómo interrumpir el flujo de conversación o cambiar de tema

Una manera de llevar un seguimiento del punto de la conversación en el que se encuentra consiste en usar el [estado de la conversación](bot-builder-howto-v4-state.md). De este modo, se guarda información sobre el tema activo o sobre los pasos de una secuencia que se han completado.

Cuando un bot se vuelve más complejo, también se puede concebir una secuencia de flujos de conversación que se producen en una pila. Por ejemplo, el bot invoca el flujo del nuevo pedido y, después, invoca el de la búsqueda de productos. Después, el usuario selecciona un producto y lo confirma, con lo que se completa el flujo de búsqueda de productos y, luego, el pedido.

El problema es que una conversación rara vez sigue una ruta tan lineal y lógica. Los usuarios no se comunican en "pilas", sino que tienden a cambiar de opinión con frecuencia. Considere el siguiente ejemplo:

![El usuario dice algo inesperado](./media/concept-conversation/interruption.png)

Mientras que el bot podría haber construido lógicamente una pila de flujos, el usuario podría decidir hacer algo completamente diferente o formular una pregunta que no tenga ninguna relación con el tema actual. En el ejemplo, el usuario hace una pregunta en lugar de facilitar la respuesta afirmativa o negativa que el flujo espera. ¿Cómo debe responder el flujo?

* Insiste en que el usuario responda primero a la pregunta.
* Pasa por alto todo lo que el usuario ha hecho hasta el momento, restablece la pila del flujo al completo y empieza desde cero intentando responder a la pregunta del usuario.
* Intenta responder a la pregunta del usuario y, después, regresa a la pregunta con respuesta afirmativa o negativa para intentar reanudar la conversación desde ahí.

No hay una respuesta correcta a esta pregunta, ya que la mejor solución dependerá de los aspectos concretos del escenario y de cómo espera el usuario que responda el bot. Para más información, vea [Handle user interrupt](bot-builder-howto-handle-user-interrupt.md) (Controlar las interrupciones del usuario).

> [!TIP]
> Si usa Bot Builder SDK para Node.Js, puede usar [cuadros de diálogo](bot-builder-dialog-manage-conversation-flow.md) para administrar el flujo de conversación.

## <a name="conversation-lifetime"></a>Duración de la conversación

<!-- Note: these activities are dependent on whether the channel actually sends them. Also, we should add links --> Un bot recibe una actividad de _actualización de la conversación_ cada vez que se ha agregado a una conversación, se han agregado otros miembros a la conversación (o se han eliminado de ella) o han cambiado los metadatos de la conversación.
Puede que le interese que el bot reaccione a las actividades de actualización de la conversación saludando a los usuarios o presentándose.

Un bot recibe una actividad de _finalización de la conversación_ para indicar que el usuario ha puesto fin a la conversación. Un bot podría enviar una actividad de _finalización de la conversación_ para indicarle al usuario que la conversación está llegando a su fin. Si va a almacenar información sobre la conversación, puede que le interese borrar esa información cuando la conversación haya finalizado.

<!--  Types of conversations

Your bot can support multi-turn interactions where it prompts users for multiple peices of information. It can be focused on a very specific task or support multiple types of tasks. 
The Bot Builder SDK has some built-in support for Language Understatnding (LUIS) and QnA Maker for adding natural language "question and answer" features to your bot.

<!--TODO: Add with links when these topics are available:
[Conversation flow] and other design articles.
[Using recognizers] [Using state and storage] and other how tos.
-->
## <a name="conversations-channels-and-users"></a>Conversaciones, canales y usuarios

Las conversaciones pueden ser de dos tipos: una conversación _directa_ con un usuario específico o una conversación de _grupo_ con varios usuarios.
Para comunicarse con un usuario en un canal, un bot recibe actividades del usuario y a su vez le envía actividades.

- Cada usuario tiene un identificador único para cada canal.
- Cada conversación tiene un identificador único para cada canal.
- El canal establece el identificador de conversación cuando inicia la conversación.
- El bot no puede iniciar una conversación, pero en cuanto dispone de un identificador de conversación, puede reanudarla.
- No todos los canales admiten conversaciones de grupo.

## <a name="next-steps"></a>Pasos siguientes

En el caso de conversaciones complejas, como algunas de las indicadas anteriormente, es necesario poder conservar la información durante más de un turno. Echemos ahora un vistazo al estado y al almacenamiento.

> [!div class="nextstepaction"]
> [Estado y almacenamiento](bot-builder-storage-concept.md)

<!-- In addition, your bot can send activities back to the user, either _proactively_, in response to internal logic, or _reactively_, in response to an activity from the user or channel.-->
<!--TODO: Link to messaging how tos.-->

<!--  TODO: Change to next steps, one for each of LUIS and State
## See also

- Activities
- Adapter
- Context
- Proactive messaging
- State
-->

[QnAMaker]:(bot-builder-luis-and-qna.md#using-qna-maker)

<!-- TODO: Update when the Dispatch concept is pushed -->
[Dispatch]:(bot-builder-concept-luis.md)
