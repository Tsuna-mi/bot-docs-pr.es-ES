---
title: Procesamiento de actividades | Microsoft Docs
description: Descripción del procesamiento de actividades en el SDK del bot.
keywords: adaptador de bot, software intermedio personalizado, cortocircuito, reserva, controladores de eventos
author: jonathanfingold
ms.author: jonathanfingold
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 03/22/2018
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: 8e08ac4721ee78bbd5d13dac09d9e505d3a1b134
ms.sourcegitcommit: f95702d27abbd242c902eeb218d55a72df56ce56
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/19/2018
ms.locfileid: "39306492"
---
# <a name="activity-processing"></a>Procesamiento de actividades

[!INCLUDE [pre-release-label](~/includes/pre-release-label.md)]

El bot y el usuario intercambian información a través de actividades. Cada una de las actividades que recibe la aplicación de bot se pasa a un adaptador de bot, que a su vez pasa la información de la actividad a la lógica del bot y, por último, envía las respuestas al usuario.

[!INCLUDE [Define a turn](~/includes/snippet-definition-turn.md)]

> [!IMPORTANT]
> Las actividades, especialmente las que [generamos](#generating-responses) durante un turno de bot, se administran de forma asincrónica. Se trata de una parte necesaria de la creación de un bot; si necesita repasar cómo funciona todo esto, vea [Programación asincrónica para .NET](https://docs.microsoft.com/en-us/dotnet/csharp/async) o [async para JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function), según el lenguaje que haya elegido.

## <a name="the-bot-adapter"></a>El adaptador de bot

Un bot está dirigido por su **adaptador**, que podría considerarse como el director del bot. El adaptador se encarga de la autenticación, el enrutamiento de la comunicación entrante y saliente, etc. El adaptador difiere en función de su entorno (es decir, internamente funciona de forma diferente en un entorno local y en Azure), pero en cada instancia se ocupa del mismo objetivo. En la mayoría de los casos no trabajamos con el adaptador directamente (por ejemplo, al crear un bot a partir de una plantilla), pero no está de más saber dónde se encuentra y qué hace.

El adaptador de bot encapsula los procesos de autenticación, envía actividades a Bot Connector Service y recibe actividades de este. Cuando el bot recibe una actividad, el adaptador encapsula todo lo relativo a esa actividad, crea un [objeto de contexto](#turn-context) para el turno, lo pasa a la lógica de aplicación del bot y envía las respuestas que genera el bot al canal del usuario.

## <a name="authentication"></a>Autenticación

El adaptador autentica todas las actividades entrantes que recibe la aplicación mediante la información de la actividad y el encabezado `Authentication` de la solicitud de REST.

El adaptador usa un objeto de conector y las credenciales de la aplicación para autenticar las actividades de salida con destino al usuario.

La autenticación de Bot Connector Service usa tokens `Bearer` de JWT (JSON Web Token), así como el **identificador de aplicación de Microsoft** y la **contraseña de aplicación de Microsoft** que Azure genera automáticamente cuando se crea un servicio de bot o se registra un bot. La aplicación necesitará estas credenciales en el tiempo de inicialización para permitir que el adaptador autentique el tráfico.

> [!NOTE]
> Si está ejecutando o probando el bot localmente (por ejemplo, mediante Bot Framework Emulator), puede hacerlo sin configurar el adaptador para autenticar el tráfico hacia y desde el bot.

## <a name="turn-context"></a>Contexto de turno

Cuando un adaptador recibe una actividad, genera un objeto de **contexto de turno**, que proporciona información sobre la actividad entrante, el remitente, el receptor, el canal, la conversación y otros datos necesarios para procesar la actividad. Después, el adaptador pasa este objeto de contexto al bot. El objeto de contexto se conserva mientras dure el turno y proporciona información sobre lo siguiente:

* Conversación: identifica la conversación e incluye información sobre el bot y el usuario que participan en la conversación.
* Actividad: las solicitudes y las respuestas de una conversación son los tipos de actividades. Este contexto proporciona información sobre la actividad entrante, incluida la información de enrutamiento, la información sobre el canal, la conversación, el remitente y el receptor.
* Estado: propiedades que se usan para llevar un seguimiento de su [estado](~/v4sdk/bot-builder-storage-concept.md), por ejemplo, dónde se encuentra en una conversación con un usuario, información sobre el usuario u otra información de la lógica de negocios.
* Información personalizada: si extiende el bot mediante la implementación de software intermedio o dentro de la lógica del bot, puede hacer que haya información adicional disponible en cada turno.

El objeto de contexto también puede usarse para enviar una respuesta al usuario y obtener una referencia al adaptador para crear una conversación o continuar con una ya existente.

> [!NOTE]
> La aplicación y el adaptador controlarán las solicitudes de forma asincrónica, pero no es necesario que la lógica de negocios esté controlada por la solicitud-respuesta.

## <a name="middleware"></a>Software intermedio

Puede agregar software intermedio al adaptador. El software intermedio consiste en capas de complementos que se agregan al bot y a la lógica básica del bot. Para información más detallada sobre el software intermedio, vea el [artículo independiente sobre el software intermedio](~/v4sdk/bot-builder-concept-middleware.md).

El software intermedio y la lógica del bot usan el objeto de contexto para recuperar información sobre la actividad y actuar en consecuencia. El software intermedio y el bot también pueden actualizar o agregar información al objeto de contexto, por ejemplo, para llevar un seguimiento del estado de un turno, una conversación u otro ámbito. El SDK proporciona _software intermedio de estado_ que se puede usar para agregar persistencia de estado al bot.

## <a name="generating-responses"></a>Generación de respuestas

El objeto de contexto proporciona métodos de respuesta de actividad para permitir que el código responda a una actividad:

* Los métodos _send activity_ y _send activities_ envían una o más actividades a la conversación.
* Si el canal lo admite, el método _update activity_ actualiza una actividad de la conversación.
* Si el canal lo admite, el método _delete activity_ elimina una actividad de la conversación.

Cada método de respuesta se ejecuta en un proceso asincrónico. Cuando se llama al método de respuesta de actividad, este clona la lista de controladores de eventos asociados antes de empezar a llamar a los controladores, lo que significa que contendrá todos los controladores agregados hasta ese momento, pero no contendrá nada de lo que se agregue una vez iniciado el proceso.

Esto también significa que no se garantiza el orden de las respuestas, sobre todo cuando una tarea es más compleja que otra. Si el bot puede generar varias respuestas para una actividad entrante, asegúrese de que tienen sentido en cualquier orden en el que las reciba el usuario.

> [!IMPORTANT]
> El subproceso que administra el turno de bot principal se ocupa de desechar el objeto de contexto cuando termina. Si una respuesta (incluidos sus controladores) tarda mucho e intenta actuar sobre el objeto de contexto, es posible que reciba un error `Context was disposed`. **Asegúrese de usar `await` para las llamadas de actividad**, a fin de que el subproceso principal espere la actividad generada antes de finalizar su procesamiento y desechar el contexto de turno.

## <a name="response-event-handlers"></a>Controladores de eventos de respuesta

Además de la lógica del bot y del software intermedio, se pueden agregar controladores de respuesta (a veces denominados controladores de eventos o controladores de eventos de actividad) al objeto de contexto. Se llama a estos controladores cuando la respuesta asociada se produce en el objeto de contexto actual, antes de ejecutar la respuesta real. Estos controladores son útiles si sabe que querrá hacer algo, ya sea antes o después del evento real, para todas las actividades de ese tipo durante el resto de la respuesta actual.

> [!WARNING]
> Tenga cuidado de no llamar a un método de respuesta de actividad desde su controlador de eventos de respuesta correspondiente, por ejemplo, mediante una llamada al método de actividad de envío desde un controlador _on send activity_. Si lo hace, puede generar un bucle infinito.

Cada actividad nueva obtiene un nuevo subproceso en el que se ejecuta. Cuando se crea el subproceso para procesar la actividad, la lista de controladores de esa actividad se copia en ese subproceso nuevo. No se ejecutará para ese evento de actividad específico ningún controlador agregado después de ese punto.

El adaptador administra los controladores registrados en un objeto de contexto de forma muy similar al modo en que administra la [canalización de software intermedio](~/v4sdk/bot-builder-concept-middleware.md#the-bot-middleware-pipeline).

Es decir, se llama a los controladores en el orden en que se agregan, y al llamar al delegado _next_ se pasa el control al siguiente controlador de eventos registrado. Si un controlador no llama al delegado next, el adaptador no llama a ninguno de los controladores de eventos posteriores; se produce un [cortocircuito](~/v4sdk/bot-builder-concept-middleware.md#short-circuiting) en el evento y el adaptador no envía la respuesta al canal.

## <a name="next-steps"></a>Pasos siguientes

Ahora que está familiarizado con algunos conceptos clave de un bot, vamos a profundizar en la manera en que un bot puede enviar mensajes proactivos.

> [!div class="nextstepaction"]
> [Software intermedio](~/v4sdk/bot-builder-concept-middleware.md)
