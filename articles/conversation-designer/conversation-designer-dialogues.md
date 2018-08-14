---
title: Definir un cuadro de diálogo como una acción Do | Microsoft Docs
description: Obtenga información sobre cómo configurar el cuadro de diálogo como una acción Do.
author: vkannan
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ROBOTS: NoIndex, NoFollow
ms.openlocfilehash: d59df20821a7f63eb9ee5dea365597b1af839f75
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39304945"
---
# <a name="define-a-dialogue-as-a-do-action"></a>Definir un cuadro de diálogo como una acción Do
> [!IMPORTANT]
> Conversation Designer no está disponible para todos los clientes todavía. Habrá más información sobre la disponibilidad de Conversation Designer más adelante este año.

Un cuadro de diálogo trata sobre el modelo de conversación de una tarea específica. Por ejemplo, un bot de cafetería que ayuda a los usuarios a reservar una mesa, podría definir una tarea llamada "Reservar mesa". La acción **Do** está ligada a un diálogo que de forma al flujo de la conversación entre el bot y el usuario. 

Los diálogos son particularmente útiles cuando un bot inicia una conversación con el usuario para ayudarle a completar una tarea.  Los usuarios rara vez expresan todos los valores requeridos para completar una tarea en un solo enunciado. 

Por ejemplo, para reservar una mesa es necesaria la ubicación, el tamaño del grupo, la fecha y la preferencia horaria. Un bot que reserve mesas necesita entender y manejar las posibles frases que el usuario podría decir. 

- *reservar una mesa*: en este caso, ninguna de las entidades requeridas fue capturada. El bot debe entablar una conversación con el usuario.
- *reservar una mesa para las 7 de la tarde este sábado* : en este caso, se especifica la preferencia de fecha y hora, pero el bot aún debe recopilar la ubicación y cuántas personas irán.

Durante una conversación, algunas entidades pueden cambiar en función de la entrada del usuario. Por ejemplo, si el usuario dice "Reserva una mesa en Redmond para este domingo a las 7 de la tarde". pero el restaurante Redmond cierra a las seis los domingos, el diálogo del bot debe ser capaz de manejar correctamente las solicitudes que no sean válidas. 

Conversation Designer proporciona un diseñador de diálogos que permite arrastrar y soltar las opciones, para ayudarle a visualizar el flujo de conversación. El diseñador de diálogos ofrece siete *estados de diálogo* que puede usar para dar forma al flujo de la conversación.

## <a name="dialogue-states"></a>Estados de diálogo

Un diálogo se compone de estados conversacionales. El diálogo en sí se diseña como un flujo estructurado y directo que proporciona al tiempo de ejecución de la conversación una estructura sobre cómo ejecutar el flujo conversacional.

Los diálogos cuentan con una gran variedad estados integrados que puede usar. Los estados integrados compatibles incluyen lo siguiente:

- [**Iniciar**](#start-state): representa el estado inicial de un flujo conversacional. Todos los diálogos deben tener al menos un estado de inicio definido.
- [**Devolver**](#return-state): representa la finalización del flujo específico de la conversación. Dado que los flujos conversacionales admiten composición, la opción Devolver indica al tiempo de ejecución de la conversación que devuelva la ejecución a los posibles autores de la llamada de este diálogo.
- [**Decisión**](#decision-state): representa un punto de bifurcación en el flujo conversacional.
- [**Proceso** ](#process-state): representa un estado en el que el bot ejecuta la lógica de negocios.
- [**Solicitud**](#prompt-state): representa un estado en el que puede solicitar al usuario que escriba más información. 
- [**Comentarios**](#feedback-state): representa un estado en el que puede proporcionar comentarios o confirmaciones al usuario. Por ejemplo, un diálogo que confirme que la reserva se ha hecho.
- [**Módulo**](#module-state): representa una llamada a otro diálogo. Puesto que los flujos de diálogo se pueden componer de forma predeterminada, este estado puede llamar a un diálogo compartido o a algún otro diálogo tal como se define en esta tarea.

Cada estado de la conversación está conectado a otro mediante conectores de diálogo del diseñador de diálogos.

Cada estado de diálogo tiene un editor de estado asociado que se utiliza para especificar las propiedades de ese estado, incluidos los nombres de función de devolución de llamadas para scripts personalizados. El **Editor de estado** está ubicado como un panel de cambio de tamaño en la parte inferior del puerto de vista principal del diseñador de diálogos. Para abrir el editor, haga doble clic en un estado del diseñador de diálogos y un **Editor de estado** mostrará las propiedades correspondientes a ese estado.
<!-- TODO: insert screenshot of the wrench in horizontal menu -->

Las siguientes subsecciones proporcionan más detalles sobre cada uno de estos estados de diálogo integrados.

### <a name="start-state"></a>Estado de inicio

El estado de inicio indica el punto de inicio de un diálogo. El valor requerido es el **nombre** del estado. El nombre es "Inicio" de forma predeterminada, pero puede editarlo para cambiarle el nombre.

### <a name="return-state"></a>Estado de devolución

El estado de devolución representa la finalización de una rama del flujo de la conversación. Puesto que los diálogos admiten composición, el estado también indica al tiempo de ejecución de la conversación que debe devolver la ejecución al diálogo del autor de la llamada. El valor requerido es el **nombre** del estado. El nombre es "Devolución" de forma predeterminada, pero puede editarlo para cambiarle el nombre.

### <a name="decision-state"></a>Estado de toma de decisiones

Un estado de toma de decisiones representa una bifurcación en el flujo de la conversación. Puede escribir un script personalizado para evaluar la rama que debe seguir. Dependiendo de la entrada del usuario y la lógica de negocios, el script devolverá uno de los valores posibles de transición. Cada valor de transición solicita que el tiempo de ejecución de la conversación ejecute una rama diferente del diálogo.

Propiedades obligatorias para el estado de toma de decisiones:
- **Nombre**: nombre único para el estado de toma de decisiones.
- **Código para ejecutar en la ejecución**: nombre de la función de devolución de llamada que implementa su lógica de negocios para determinar qué rama de la conversación tomar. 

#### <a name="example-code-for-decision-state"></a>Código de ejemplo para el estado de toma de decisiones

En la siguiente función de devolución de llamada se muestra una decisión que indica al tiempo de ejecución de la conversación qué rama debe ejecutar.

```javascript
module.exports.fnDecisionState = function(context) {
    var a = context.taskEntities['a'];
    if (a[0].value === '0') {
        return 'yes';
    }
    else if (a[0].value === '1') {
        return 'no';
    }
}
```

### <a name="process-state"></a>Estado de proceso

El estado de proceso representa un punto en el diálogo donde el bot está avanzando en la conversación, o intentando realizar la acción correspondiente para terminar la tarea final. 

Propiedades obligatorias para el estado de proceso:
- **Nombre**: nombre único para el estado de proceso.
- **Código que se ejecutará en la ejecución**: nombre de la función de devolución de llamada que implementa la lógica de negocios.

#### <a name="example-code-for-process-state"></a>Código de ejemplo del estado de proceso

La siguiente función de devolución de llamada muestra el clima y devuelve la información meteorológica al usuario.

```javascript
module.exports.fnGetWeather = function(context) {
    var options =  {
        host: 'mock',
        path: '/get?a=b',
        method: 'get'
    };
    return http.request(options).then(function(response) {
        context.contextEntities['x'].value= response.statusCode;
        var jsonBody = JSON.parse(response.body);
          // understand response
        if (response.statusCode != "200") {
            // error
        }
    });
}
```

### <a name="prompt-state"></a>Estado de solicitud

El estado de solicitud le pide al usuario una información específica. Los estados de solicitud incorporan un sistema de subdiálogo y, debido a ello, se consideran estados complejos. 

Al usar un estado de solicitud, puede definir la respuesta real que le proporcionará al usuario e incluir opcionalmente una tarjeta adaptable. A continuación, puede especificar un desencadenador para analizar y comprender la respuesta del usuario. Este desencadenador puede ser LUIS o un reconocedor de código personalizado que utilice expresiones regulares.  

Si el usuario proporciona una entrada no válida, el bot puede volver a solicitar al usuario la misma información. Este comportamiento también se puede definir en el editor de estado de la solicitud. 

#### <a name="prompting-the-user"></a>Preguntar al usuario

La respuesta a las solicitudes le permitirá especificar el mensaje que se usará cuando **solicite al usuario** más información. Por ejemplo, para recopilar la fecha y la hora, las posibles preguntas que verá el usuario pueden ser "¿Cuándo le gustaría venir?" o "¿Cuándo le gustaría cenar con nosotros?"

#### <a name="prompt-listening-for-user-input"></a>Escucha de solicitudes de las entradas del usuario

Después de pedirle al usuario que responda, el tiempo de ejecución de la conversación escuchará automáticamente la información del usuario y tratará de comprender lo que dijo. Configure un desencadenador basado en LUIS o un reconocedor de código personalizado para intentar comprender la entrada e intención del usuario. Esto es similar al desencadenador de la tarea.

#### <a name="re-prompting-the-user"></a>Volver a preguntar al usuario

Use la sección que especifica cómo volver a preguntar al usuario, para especificar una respuesta para cada intento. Cada fila en la sección para volver a preguntar al usuario corresponde a la cadena de repetición de preguntas utilizada para ese turno específico. La primera respuesta se usará para la primera pregunta repetida, la segunda para la segunda, y así sucesivamente. Por ejemplo: 

*Lo siento, no le entendí. ¿Cuándo quiere venir? *
* Lo siento, pero me está costando entenderle. Probemos de nuevo. ¿Cuándo quiere venir?*

#### <a name="prompt-callback-functions"></a>Funciones de devolución de llamada de solicitudes

Puede especificar dos funciones de devolución de llamada diferentes en un estado de solicitud. 

1. **Antes de cada solicitud y repetición de solicitud**: ejecute esta función antes de cada solicitud o repetición de solicitud. Esta función de devolución de llamada espera un valor devuelto booleano donde "true" significa ejecutar esta solicitud o repetirla, y "false" significa que no se va a ejecutar esta solicitud o repetición de solicitud. Puede usar `getTurnIndex()` para obtener el índice de turno actual para ejecutar esa solicitud.
2. **Al responder**: ejecute esta función cada vez que se genere una solicitud, pero antes de que se haya enviado al usuario (incluyendo la repetición de la solicitud). Esto permite que el script modifique el mensaje que se envía al usuario.

#### <a name="sample-code"></a>Código de ejemplo

Este fragmento de código muestra un ejemplo para la devolución de llamada denominada **Antes de ejecutar**.

```javascript
module.exports.fnBeforeExecuting = function(context) {
    if(context.responses[0].text === "C") {
        return false;
    }
    return true;
}
```

Este fragmento de código muestra un ejemplo para la devolución de llamada denominada **Al solicitar**.

```javascript
module.exports.fnOnPrompting = function(context) {
    // include a hint card
    var activity = context.responses.slice(-1).pop();
    activity.attachments.push({
        "contentType": "application/vnd.microsoft.card.hero",
        "content": {
            "buttons": [
                {
                    "type": "imBack",
                    "title": "1",
                    "value": "1"
                },
                {
                    "type": "imBack",
                    "title": "2",
                    "value": "2"
                }
            ]
        }
    });
}
```

Este fragmento de código muestra un ejemplo para la devolución de llamada denominada **Antes de volver a solicitar**.

```javascript
module.exports.fnBeforeReprompting = function(context) {
    if(context.responses[0].text === "C") {
        return false;
    }
    return true;
}
```

### <a name="feedback-state"></a>Estado de comentarios

Use este estado para proporcionar una respuesta al usuario. Los casos de uso típicos de este estado incluyen el resultado final después de intentar finalizar la tarea, o para proporcionar una respuesta al usuario si se produce un error, etc. 

Cada estado de comentarios requiere un nombre único, algunos valores de respuesta posibles y, opcionalmente, puede incluir definiciones de tarjetas adaptables. Obtenga más información sobre la [definición de las tarjetas adaptables](conversation-designer-adaptive-cards.md).

Cada estado de comentarios también permite una función de devolución de llamada denominada **Al responder**, donde puede escribir su script personalizado para modificar la carga útil de la actividad antes de enviarla al usuario (si fuera necesario). 


```javascript
module.exports.fnOnResponding = function(context) {
    // include a hint card
    var activity = context.responses.slice(-1).pop();
    activity.attachments.push({
        "contentType": "application/vnd.microsoft.card.hero",
        "content": {
            "buttons": [
                {
                    "type": "imBack",
                    "title": "1",
                    "value": "1"
                },
                {
                    "type": "imBack",
                    "title": "2",
                    "value": "2"
                }
            ]
        }
    });

}
```

### <a name="module-state"></a>Estado de módulo

Un estado de módulo se usa para agregar una referencia para ejecutar un subdiálogo. Use este estado para encadenar diálogos. 

Cada estado de módulo debe tener un nombre único y un puntero que lleve al diálogo específico que se va a ejecutar. Las posibles opciones para que los diálogos se ejecuten deben estar en la opción **Diálogos compartidos** o en las **Tareas** específicas.

## <a name="multiple-dialogues-under-a-task"></a>Varios diálogos en una tarea

Una tarea puede tener varios diálogos. Para agregar un diálogo a una tarea, simplemente seleccione la tarea y haga clic en el botón **Agregar** en el panel del árbol que está a la izquierda; a continuación, haga clic en **Agregar diálogo**. Esto agregará un nuevo diálogo en la tarea seleccionada. 

Como los diálogos admiten composición, puede vincular el flujo del diálogo raíz a la tarea que se encarga de llamar a otros diálogos de la misma. Esto le permite encapsular subtareas y habilitar la opción de reutilización. Asimismo, esta opción también le permite encadenar estos diálogos en un flujo de conversación, mediante estados de *módulo*.

## <a name="next-step"></a>Paso siguiente
> [!div class="nextstepaction"]
> [Plantillas de respuesta](conversation-designer-response-templates.md)
