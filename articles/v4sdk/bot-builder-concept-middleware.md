---
title: Software intermedio | Microsoft Docs
description: Descripción del software intermedio y de sus usos en el SDK del bot.
keywords: software intermedio, canalización de software intermedio, cortocircuito, usos del software intermedio
author: ivorb
ms.author: v-ivorb
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 05/24/2018
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: d8201da0fb406f30888dfaa4ff6017f125990104
ms.sourcegitcommit: 2dc75701b169d822c9499e393439161bc87639d2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42905379"
---
# <a name="middleware"></a>Software intermedio

[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]

El software intermedio es simplemente una clase que se encuentra entre el adaptador y la lógica del bot, que se agrega a la colección de software intermedio del adaptador durante la inicialización. El SDK le permite escribir su propio software intermedio o agregar componentes reutilizables de software intermedio creado por otros usuarios. ¿Qué se puede hacer en el software intermedio? Prácticamente de todo. Todas las actividades que entran y salen de su bot fluyen a través del software intermedio.

El adaptador procesa y dirige las actividades entrantes a través de la canalización de software intermedio del bot a la lógica del bot y, luego, otra vez de vuelta. Cuando las actividades entran y salen de los bots, cada fragmento de software intermedio puede inspeccionar o actuar sobre la actividad, tanto antes como después de que se ejecute la lógica del bot.

Antes de centrarse en el software intermedio, es importante entender los [bots en general](~/v4sdk/bot-builder-basics.md) y [cómo procesan las actividades](~/v4sdk/bot-builder-concept-activity-processing.md).

## <a name="uses-for-middleware"></a>Usos del software intermedio

A menudo, los usuarios se preguntan qué debe ir en el software intermedio en lugar de en la lógica del bot. El software intermedio es bastante flexible, por lo que en última instancia, esto depende de usted. Veamos un par de usos en los que el software intermedio resulta ideal.

### <a name="looking-at-or-acting-on-every-activity"></a>Registrar o actuar sobre todas las actividades

Existen muchas situaciones que requieren que nuestro bot haga algo en todas las actividades, o bien en todas las actividades de un tipo determinado. Por ejemplo, tal vez nos interese registrar todas las actividades de mensajes que recibe el bot, o proporcionar una respuesta de reserva si el bot no ha generado ninguna respuesta en este turno. El software intermedio es idóneo para hacerlo, ya que puede actuar tanto antes como después de que se haya ejecutado el resto de la lógica del bot.

### <a name="modifying-or-enhancing-the-turn-context"></a>Modificar o mejorar el contexto de turno

Algunas conversaciones pueden ser mucho más provechosas si el bot tiene más información de la que se proporciona en la actividad. En este caso, el middleware podría comprobar la información de estado de la conversación que se tiene hasta el momento, consultar un origen de datos externo y anexarlo al objeto del [contexto de turno](bot-builder-concept-activity-processing.md#turn-context) antes de pasar la ejecución a la lógica del bot.
Por ejemplo, el software intermedio podría identificar los detalles de la conversación, como el identificador de conversación y el estado, y consultar un servicio de directorio para obtener información. También podría agregar el objeto de usuario que se recibe de esa consulta externa al objeto de contexto y pasarlo, con lo que proporciona más datos sobre el usuario y permite que el bot controle mejor la solicitud.

El software intermedio puede usarse para las dos finalidades anteriores, o bien para otra completamente diferente; todo depende de cómo quiera que se estructure el bot y de lo que el bot intente conseguir.
El software intermedio puede establecer o conservar un estado, reaccionar a las solicitudes entrantes o producir un cortocircuito en la canalización.
Entre los candidatos para el software intermedio, se incluyen los siguientes:

- **almacenamiento o persistencia**: use software intermedio para almacenar o conservar valores después de un turno, o bien para actualizar un valor según lo que haya ocurrido en ese turno.
- **control de errores y de errores leves**: si se produce una excepción, el software intermedio puede enviar un mensaje descriptivo para esa excepción.
- **traducción**: este software intermedio podría detectar y traducir los mensajes entrantes, así como configurar los mensajes salientes de modo que se traduzcan al idioma de entrada detectado.

Puede definir su propio software intermedio o usar el software intermedio del SDK, que representa componentes independientes de la aplicación, como:

- Software intermedio de estado, que guarda y restaura la información de estado del objeto de contexto. El SDK proporciona software intermedio de estado del usuario y la conversación.
- Software intermedio de traducción, que puede reconocer el idioma de entrada y traducirlo a otro lenguaje, por ejemplo, uno que entienda la aplicación.
- Software intermedio de registro, que puede registrar las actividades entrantes y salientes.

## <a name="the-bot-middleware-pipeline"></a>La canalización de software intermedio del bot

Para cada actividad, el adaptador llama al software intermedio en el **orden en que se ha agregado**. El adaptador pasa el objeto de contexto para el turno y un delegado _next_, y el software intermedio llama al delegado para pasar el control al siguiente software intermedio de la canalización. El software intermedio también puede llevar a cabo acciones después de que se devuelva el delegado _next_ antes de completar el método. Es como si cada objeto de software intermedio tuviese la oportunidad de actuar directamente sobre los objetos de software intermedio que lo siguen en la canalización.

Por ejemplo: 

- El primer controlador de turno del objeto de software intermedio ejecuta código antes de llamar a _next_.
  - El segundo controlador de turno del objeto de software intermedio ejecuta código antes de llamar a _next_.
    - El controlador de turno del bot se ejecuta y se devuelve.
  - El segundo controlador de turno del objeto de software intermedio ejecuta el código restante antes de devolver.
- El primer controlador de turno del objeto de software intermedio ejecuta el código restante antes de devolver.

Si el software intermedio no llama al delegado next, el adaptador no llama a ninguno de los controladores de turno posteriores de software intermedio o bot y se produce un cortocircuito en la canalización.

Una vez que se ha completado la canalización de software intermedio del bot, el turno acaba y el contexto de turno sale del ámbito.

El software intermedio o el bot pueden generar respuestas y registrar los controladores de eventos de respuesta, pero debe tener en cuenta que las respuestas se controlan en procesos independientes.

## <a name="order-of-middleware"></a>Orden del software intermedio

Puesto que el orden en el que se agrega el software intermedio determina el orden en el que este procesa una actividad, es importante decidir la secuencia que se seguirá para agregar dicho software intermedio.

> [!NOTE]
> Esto está pensado para proporcionarle un patrón común que funcione para la mayoría de los bots, pero debe tener en cuenta cómo interactuará cada fragmento de software intermedio con los demás según su situación.

Los primeros elementos de la canalización de software intermedio probablemente serán los que se encarguen de las tareas de nivel más bajo que se usan de cada vez, como el registro, el control de excepciones, la administración de estados y la traducción, entre otras cosas. El orden de estos elementos puede variar según sus necesidades, por ejemplo, si quiere que el mensaje entrante se traduzca primero, antes de que se controlen las excepciones, o si debe producirse primero el control de excepciones, lo que puede conllevar que los mensajes de excepción no se traduzcan.

Los últimos elementos de la canalización de software intermedio deberían ser el software intermedio específico del bot, que es el que se implementa para realizar el procesamiento de cada mensaje que se envía al bot. Si el software intermedio usa información de estado u otra información definida en el contexto del bot, agréguela a la canalización de software intermedio después del software intermedio que modifica el estado o el contexto.

## <a name="short-circuiting"></a>Cortocircuitos

Un concepto importante relacionado con el software intermedio (y los [controladores de eventos](~/v4sdk/bot-builder-concept-activity-processing.md#response-event-handlers)) es el _cortocircuito_. Si la ejecución va a continuar por las capas siguientes, el software intermedio (o un controlador) debe pasar la ejecución mediante una llamada al delegado _next_.  Si no se llama al delegado next dentro de ese software intermedio (o controlador), se producirá un cortocircuito en la canalización actual y las capas posteriores no se ejecutarán. Esto significa que se omite toda la lógica del bot, así como el software intermedio que se encuentre más adelante en la canalización.

En el caso de los controladores de eventos, si no se llama a _next_ el evento se cancela, lo que supone un resultado muy diferente del que se obtiene cuando el software intermedio omite la lógica. Al no procesar el resto del evento, el adaptador no lo envía nunca.

> [!TIP]
> Si aun así produce un cortocircuito en un evento, como `SendActivities`, asegúrese de que es el comportamiento que tenía previsto. En caso contrario, pueden producirse errores muy confusos.

## <a name="next-steps"></a>Pasos siguientes

Ahora que está familiarizado con algunos conceptos clave de un bot, vamos a profundizar en la manera en que un bot puede enviar mensajes proactivos.

> [!div class="nextstepaction"]
> [Mensajes proactivos](~/v4sdk/bot-builder-proactive-messages.md)
