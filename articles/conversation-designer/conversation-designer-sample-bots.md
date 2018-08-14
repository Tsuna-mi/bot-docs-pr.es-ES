---
title: Creación de un bot de Conversation Designer desde un bot de ejemplo | Microsoft Docs
description: Inicie un bot nuevo con uno de estos bots de ejemplo.
author: v-ducvo
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ROBOTS: NoIndex, NoFollow
ms.openlocfilehash: 0ebf0d1b90b03789d8a77710631c83f0914099c5
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305424"
---
# <a name="create-a-conversation-designer-bot-from-a-sample-bots"></a>Creación de un bot de Conversation Designer desde un bot de ejemplo

Conversation Designer es una potente plataforma que proporciona un generador visual de bots, un modelo declarativo para definir estos y un runtime que facilita su creación. Una manera de simplificar la creación del bot es iniciar el bot desde un bot existente.

Puede crear bots de Conversation Designer desde uno de muchos bots de ejemplo o desde un bot de Conversation Designer previamente exportado. Cada bot de ejemplo es un bot completamente funcional. Esto ofrece un punto inicial sólido donde puede empezar a compilar su propio bot o personalizar el bot de ejemplo para ajustarlo a sus escenarios.

## <a name="the-sample-bots"></a>Los bots de ejemplo

Conversation Designer viene con un conjunto de **bots de ejemplo**. Puede empezar a compilar el bot basándose en uno de estos **bots de ejemplo**. La mayoría de estos **bots de ejemplo** abarcan un pequeño escenario de un bot completamente funcional. Si quiere ver todos estos bots de ejemplo funcionando en conjunto, cree el **bot completo de Contoso Cafe**. 

En la tabla siguiente se muestra el tipo de bots que puede crear con una descripción breve sobre cada bot y los escenarios que abarca. 

| Nombre del bot | Escenarios | Características incluidas |
| ---- | ---- | ---- |
| **Bot básico** | Un bot que tiene enlazadas algunas funcionalidades básicas. Por ejemplo, este bot muestra un mensaje de bienvenida cuando un usuario nuevo se agrega a una conversación, responde a los mensajes de saludo y ejecuta un comportamiento de reserva cuando el usuario pregunta algo que el bot no entiende. | - [Tareas](conversation-designer-tasks.md) <br/>- Acciones y desencadenadores de tareas <br/>- [Desencadenador de Language Understanding](conversation-designer-luis.md)<br/>- [Desencadenador de scripts](conversation-designer-code-recognizer.md)<br/>- [Acción de respuesta](conversation-designer-reply.md) |
| **Bot de eco** | Un **bot básico** que repite el mensaje de un usuario.  Todo lo que el usuario le diga al bot, este lo repetirá diciendo: "Esto es lo que dijo: ... mensaje...". | - [Desencadenador de scripts](conversation-designer-code-recognizer.md)<br/>- Interpretación del objeto de contexto en el código<br/>- Creación de entidades en el código<br>- Uso de entidades en la respuesta del bot |
| **Bot de QnA** | Un bot que puede llevar a cabo una experiencia de pregunta y respuesta de un solo turno mediante el uso de [QnAMaker.ai](http://qnamaker.ai). Para que este ejemplo funcione, debe tener configurada una cuenta de [QnA Maker](http://qnamaker.ai) y debe haberla entrenado con algunas preguntas y respuestas. Luego, abra este **bot de QnA**, vaya a la página **Scripts** y reemplace estos dos marcadores de posición por la información de QnA Maker: `<INSERT YOUR KB ID>` y `<INSERT YOUR SUBSCRIPTION KEY>`. Una vez que escriba ambos fragmentos de información, [guarde](conversation-designer-save-bot.md) el bot y podrá [probarlo](conversation-designer-debug-bot.md). | - [Desencadenador de scripts](conversation-designer-code-recognizer.md)<br/>- Realización de solicitudes HTTP desde el código<br/>- Conexión al servicio NLP/LU personalizado (QnAMaker.ai en el ejemplo) |
| **Bot de conversación** | Un bot que puede participar en una conversación simple y que recuerda el contexto, como el nombre del usuario. | - [Acción de diálogo](conversation-designer-dialogues.md)<br/>- [Estados de diálogo](conversation-designer-dialogues.md#dialogue-states) básicos y propiedades<br/>- Introducción al [diálogo a petición](conversation-designer-dialogues.md#prompt-state) (definición de un comportamiento de petición y nueva petición)<br/>- [Creación de intenciones](conversation-designer-luis.md#create-a-new-intent) con entidades etiquetadas en un diálogo a petición<br/>- Uso de entidades generadas por Language Understanding en la respuesta del bot<br/>- Uso de [tarjetas](conversation-designer-adaptive-cards.md) para mejorar la experiencia de usuario final<br/>- Enlace de entidades del bot a [formularios de entrada](conversation-designer-adaptive-cards.md#input-form)<br/>- Creación y uso del recurso de bot [Plantilla de respuesta](conversation-designer-response-templates.md) |
| **Bot de memoria** | Un bot que puede pedir las preferencias del usuario y que más tarde recupera la información de una conversación. Cuando el usuario diga algo como "Establecer preferencia de comunicación", este bot preguntará si quiere recibir la comunicación a través de "llamada" o "texto". También puede pedirle a este bot información sobre la ubicación de Contoso Cafe con una frase como "Buscar la ubicación del café" o "Obtener las indicaciones para llegar a Contoso Cafe en Redmond, WA". Cuando el bot encuentre la información, lo llamará o le enviará un mensaje de texto con la información que encontró. <br/><br/>Si ya se estableció una preferencia de comunicación, el bot simplemente usará esa preferencia; de lo contrario, se la pedirá al usuario. | - [Acción de diálogo](conversation-designer-dialogues.md)<br/>- Guardar datos en la memoria del bot mediante [`context.global`](conversation-designer-context-object.md#context-properties)<br/>- Recuperación de información contextual desde la memoria para usarla en un diálogo |
| **Bot para reservar una mesa** | Un bot que puede participar en una conversación de varios turnos con un usuario para ayudarlo a reservar una mesa en la tienda de Contoso Cafe. <br/><br/>Para reservar una mesa, este bot necesita tres fragmentos de información: (1) *tamaño de la fiesta*, (2) *fecha y hora* y (3) *ubicación*. <br/><br/>Puede dar estos tres fragmentos de información en un solo mensaje con una frase similar a esta: "Quisiera reservar una mesa para 4 personas en el local de Redmond el martes a las 2 de la tarde". Si olvida parte de esta información, el bot se la solicitará. | - [Acción de diálogo](conversation-designer-dialogues.md)<br/>- Acción de diálogo complejo con referencias a otro diálogo<br/>- Creación de diálogos en conjunto<br/>- Configuración del diálogo del bot con todo el potencial de LUIS para administrar aspectos como el rellenado de espacios por adelantado y la experiencia de correcciones<br/>- Uso de [tarjetas](conversation-designer-adaptive-cards.md) para mejorar la experiencia de usuario final<br/>- Enlace de entidades del bot a [formularios de entrada](conversation-designer-adaptive-cards.md#input-form)<br/>- Creación y uso del recurso de bot de [plantilla de respuesta condicional](conversation-designer-response-templates.md#conditional-response-templates) |
| **Bot de perfeccionamiento de la búsqueda** | Un bot que puede ayudar al usuario a restringir la búsqueda mediante el uso de información contextual proveniente de conversaciones anteriores. Puede usar frases como "¿La tienda de Seattle está abierta este fin de semana?" seguida de "¿Y la tienda de Renton?". El bot recordará que en la segunda pregunta sigue hablando de este fin de semana. También puede probar frases como "¿Qué tiendas están abiertas este viernes a las 8 de la noche?", seguida de "¿Y a las 10 de la noche?", etc. | - [Acción de diálogo](conversation-designer-dialogues.md)<br/>- Guardar datos en la memoria del bot mediante [`context.global`](conversation-designer-context-object.md#context-properties)<br/>- Uso de la memoria del bot para tener una conversación de tipo de contexto transmitido con el usuario<br/>- Recuperación de información contextual desde la memoria para usarla en un diálogo |
| **Bot para pedir un sándwich** | Un bot que muestra las distintas formas de pedirle información a un usuario cuando se pida un sándwich. Por ejemplo, este bot lo ayudará en el pedido de un sándwich. <br/><br/>Para pedir un sándwich, el bot requiere cuatro fragmentos de información: (1) *el tamaño del sándwich*, (2) *el tipo de pan*, (3) *la opción de proteína* y (4) *los ingredientes*. <br/><br/>De manera similar a lo que ocurre con el **bot para reservar una mesa**, puede brindar toda la información en un solo mensaje o bien brindar cada uno de los cuatro fragmentos de información necesaria a la vez con frases como "Quisiera pedir un sándwich" o "Quiero un sándwich grande" y el bot le preguntará el resto de la información necesaria para procesar el pedido. | - [Acción de diálogo](conversation-designer-dialogues.md)<br/>- Configuración de [solicitudes](conversation-designer-response-templates.md) para recopilar información de distintas maneras. |
| **Bot de Contoso Cafe** | Un bot completamente funcional creado para Contoso Cafe. Este bot puede hacer todo lo que hacen los demás bots de ejemplo. Brinda un mensaje de bienvenida con una tarjeta enriquecida con la cual puede interactuar. Permite mensajes de ayuda, saludos y reserva. Permite reservar una mesa, encontrar la ubicación de un café o pedir un sándwich. | - Todas las características de Conversation Designer<br/>- Guardar datos en la memoria del bot mediante [`context.global`](conversation-designer-context-object.md#context-properties)<br/>- Uso de [tarjetas](conversation-designer-adaptive-cards.md) para desencadenar tareas en un bot específico en función de la información del usuario (vea la tarjeta con botones en el mensaje de bienvenida)<br/>- Compilación de un bot complejo que puede realizar varias [tareas](conversation-designer-tasks.md) distintas. |

## <a name="explore-sample-bots"></a>Exploración de los bots de ejemplo

Si el bot de ejemplo actual que usa no es precisamente lo que quería, puede cambiar a otro bot de ejemplo en cualquier momento. Puede encontrar una lista de todos los bots de ejemplo en la página de **exploración de los bots de ejemplo**.

> [!WARNING] 
> Importar un bot de ejemplo reemplazará el bot actual por el bot de ejemplo. Si no quiere dejar la personalización en el bot actual, [expórtela](conversation-designer-export-import-bot.md#export-a-conversation-designer-bot) a un archivo ZIP.

Para cambiar a otro bot de ejemplo, haga lo siguiente:
1. En el bot actual, haga clic en **Explorar los bots de ejemplo**. Esta opción está en la parte inferior del panel de navegación de la izquierda. 
2. Elija un bot en la lista de **bots de ejemplo** y haga clic en **Importar**.
3. Si quiere guardar el trabajo en el bot actual, elija la opción **Backup and import** (Hacer una copia de seguridad e importar). Esta opción guardará el bot actual en el equipo local y luego importará el **bot de ejemplo** nuevo. En caso contrario, elija **Import without backing up** (Importar sin hacer una copia de seguridad).

El bot se reemplaza por el bot de ejemplo nuevo que acaba de seleccionar. 

## <a name="next-step"></a>Paso siguiente
> [!div class="nextstepaction"]
> [Guardar bot](conversation-designer-save-bot.md)
