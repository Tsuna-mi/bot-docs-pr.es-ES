---
title: Plantilla de respuesta | Microsoft Docs
description: Aprenda a configurar la plantilla de respuesta para los bots de Conversation Designer.
author: vkannan
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ROBOTS: NoIndex, NoFollow
ms.openlocfilehash: b09b7fa5f5672c121711deb2edcdc9718a79983f
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39306253"
---
# <a name="response-template-for-conversation-designer-bots"></a>Plantilla de respuesta para los bots de Conversation Designer
> [!IMPORTANT]
> Conversation Designer no está disponible para todos los clientes todavía. Habrá más información sobre la disponibilidad de Conversation Designer más adelante este año.

La generación de lenguaje permite al bot comunicarse con el usuario de forma fluida y natural mediante mensajes de respuesta variable. Estos mensajes se administran mediante plantillas de respuesta de Conversation Designer.

Las plantillas de respuesta permiten la reutilización de respuestas y ayudan a mantener un tono y un lenguaje coherentes en las respuestas del bot. 

## <a name="create-response-templates"></a>Creación de plantillas de respuesta

En Conversation Designer, puede crear plantillas de respuesta que puede volver a usar en cualquier situación en la que tenga que enviar una respuesta a un usuario. 

Las plantillas de respuesta simples definen una colección única de expresiones que se pueden verbalizar o mostrar en pantalla. Después, puede usar estas plantillas en las respuestas del bot, los estados del aviso o en la tarjetas adaptables que se usan para reconstruir una cadena totalmente resuelta.

Para agregar una plantilla de respuesta simple, haga lo siguiente:
1. En el panel izquierdo, haga clic en **Agregar**. Aparecerá un menú contextual.
2. Haga clic en **Respuesta simple**. Aparece una ventana de edición en el panel de contenido principal.
3. En el campo **Simple response name** (Nombre de la respuesta simple), escriba un nombre para esta respuesta simple.
4. En el campo **Bot's response to user** (Respuesta del bot al usuario), escriba la respuesta, una frase cada vez.
5. En la esquina superior derecha, haga clic en **Guardar** cuando haya terminado de configurar la respuesta simple. 

Por ejemplo, puede crear una plantilla de frase de confirmación (llámela, "AckPhrase") con los siguientes elementos:

- Aceptar
- Por supuesto
- Seguro
- Será un placer ayudarle
- Me alegra poder ayudarle

Con esta plantilla, el entorno en tiempo de ejecución resuelve con una de estas cadenas aleatoriamente para que las respuestas del bot sean más naturales y menos aburridas y monótonas.

## <a name="conditional-response-templates"></a>Plantillas de respuesta condicional

Las plantillas de respuesta condicional permiten especificar varias respuestas con condiciones. La función de devolución de llamada que escribe ayuda al motor de generación de lenguaje a decidir qué cadena de respuesta debe usar según la condición que haya especificado. 

Por ejemplo, una plantilla de hora del día resolvería con "buenos días" o "buenas tardes" según la hora real del día. 

Para agregar una plantilla de respuesta condicional, haga lo siguiente:
1. En el panel izquierdo, haga clic en **Agregar**. Aparecerá un menú contextual.
2. Haga clic en **Respuesta condicional**. Aparece una ventana de edición en el panel de contenido principal.
3. En el campo **Conditional response name** (Nombre de la respuesta condicional), escriba un nombre para esta plantilla.
4. En el campo **Respuesta condicional**, escriba las frases, una cada vez. Por ejemplo, para "timeOfDayTemplate", escriba "Buenos días" y "Buenas tardes" como dos respuestas posibles en la plantilla.
5. Para la respuesta "Buenos días", especifique su **nombre de condición** como "mañana". Para la respuesta "Buenas tardes", especifique su **nombre de condición** como "tarde". El script personalizado que escriba le devolverá una de estas condiciones.
6. En el campo **Code to execute on run** (Código que se ejecutará en ejecución), escriba un nombre para la función de devolución de llamada (p. ej.: `fnResolveTimeOfDayTemplate`). A continuación, haga clic en **Ver código** para cargar el editor de *scripts**. Una vez allí, puede definir la implementación de esta función de devolución de llamada.
7. En la esquina superior derecha, haga clic en **Guardar** para guardar los cambios y crear esta plantilla.

Ejemplo de código de la función de devolución de llamada *fnResolveTimeOfDayTemplate*. Esta función devolverá la cadena que coincida con el **nombre de condición** "mañana" o "tarde" especificado por la respuesta.

```javascript
module.exports.fnTimeOfDayTemplate = function(context) {
    var currentTime = new Date().getHours();
    if(currentTime >= 12) {
        return "evening!";
    } else {
        return "morning!";
    }
}
```

## <a name="send-a-response-to-user"></a>Envío de una respuesta al usuario

Para enviar una respuesta al usuario mediante plantillas de respuesta, agregue los nombres de plantilla entre corchetes. Por ejemplo, para usar la plantilla de respuesta simple "AckPhrase" en un estado de comentarios, en el campo **Bot's response to user** (Respuesta del bot al usuario) escriba la frase `[AckPhrase], I will get that done for you right away`

Si desea utilizar corchetes en las respuestas, use "\" como carácter de escape para indicar al entorno en tiempo de ejecución de la conversación que omita la resolución de los corchetes.

Si tiene entidades definidas, puede usarlas también en su respuesta al usuario. Puede hacer referencia a las entidades en la generación del lenguaje poniendo el nombre de la entidad entre llaves. Por ejemplo, si el bot tiene una entidad `location`, puede hacer referencia a ella en las respuestas de la siguiente manera: `[AckPhrase], I can help you find a table at {location}.`

## <a name="nesting-response-templates"></a>Anidamiento de plantillas de respuesta

Las plantillas de respuesta se pueden "anidar". Una plantilla de respuesta puede contener referencias a otra plantilla de respuesta. Por ejemplo, podría usar la plantilla de respuesta simple `AckPhrase` y la plantilla de respuesta condicional `timeOfDayTemplate` para construir una nueva plantilla de respuesta `timeBasedGreeting` que use ambas plantillas para resolver la respuesta final. 

> [!TIP]
> Aunque el anidamiento de plantillas es una característica eficaz, asegúrese de que las plantillas anidadas no causan un bucle infinito. Es decir, evite situaciones en las que hace que `AckPhrase` llame a `timeOfDayTemplate` y `timeOfDayTemplate` devuelva la llamada a `AckPhrase`.

Por ejemplo, cree un nuevo nombre de **respuesta simple** `timeBasedGreeting` y escriba el texto siguiente como posible **respuesta del bot a un usuario** para esta plantilla: `[timeOfDayTemplate] [AckPhrase], ... `

## <a name="define-user-utterance-as-help-tips"></a>Definición de expresiones del usuario como sugerencias de ayuda

Al definir una **expresión** del usuario, puede establecer también una expresión como **sugerencia de ayuda**. Para establecer una expresión como **sugerencia de ayuda**, haga clic en `...` a la derecha de la expresión y seleccione **Use as help tip** (Usar como sugerencia de ayuda). 

La sugerencia de ayuda se usa como la plantilla integrada de generación de lenguaje `[builtin-tasktips]` en la que puede definir un campo **Bot's response to user** (Respuesta del bot al usuario). Por ejemplo, podría crear una respuesta que diga: `Sorry I did not understand that. Here are some things you can try - [builtin.tasktips]`

Si una tarea no tiene ninguna **sugerencia de ayuda** definida, `[builtin-tasktips]` se resolverá con una cadena vacía. Si una tarea tiene varias **sugerencias de ayuda** definidas, `[builtin-tasktips]` seleccionará una de forma aleatoria cada vez que se proporcione una respuesta.

## <a name="next-step"></a>Paso siguiente
> [!div class="nextstepaction"]
> [Tarjetas adaptables](conversation-designer-adaptive-cards.md)
