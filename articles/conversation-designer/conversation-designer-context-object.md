---
title: Referencia de API para el objeto de contexto | Microsoft Docs
description: Obtenga información sobre cómo hacer referencia al objeto de contexto en el bot de Conversation Designer.
author: vkannan
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ROBOTS: NoIndex, NoFollow
ms.openlocfilehash: 5ba70189c16815539524d3c9046da03ed0344b9b
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39304810"
---
# <a name="api-reference"></a>Referencia de API
> [!IMPORTANT]
> Conversation Designer aún no está disponible para todos los clientes. Podrá obtener más información sobre la disponibilidad de Conversation Designer a lo largo de este año.

Conversation Designer le ofrece la posibilidad de agregar lógica de negocios personalizada al bot. Estas funciones de script se implementan en el editor **Scripts**. Las funciones se enlazan en varios lugares, como las plantillas de respuesta condicional, los *desencadenadores* o las *acciones* de la tarea, o los estados de diálogo, y todos reciben un objeto `context` de tipo **[IConversationContext]** en sus parámetros de función.

En el ejemplo de código siguiente se muestra la firma para una función de respuesta condicional con el objeto **contexto** pasado.

```javascript
/**
* @param {IConversationContext} context
*/
module.exports.NewConditionalResponse_onRun = function(context) {
    // Business logic here
    return true; // Returns a boolean
}
```

A través del objeto `context`, puede obtener acceso a información acerca de la conversación entre el usuario y el bot.

## <a name="script-callback-functions"></a>Funciones de devolución de llamada de script

Las funciones de devolución de llamada de script personalizadas que cree pueden adoptar muchas formas. Mientras que puede darles nombres diferentes, funcionalmente, toman una de las formas siguientes.

| Forma | Parámetro | Tipo de valor devuelto | DESCRIPCIÓN |
| ---- | ---- | ---- | ---- |
| Antes de la función de respuesta | contexto | **void** o **promise** | Función que se ejecuta antes de dar una respuesta. |
| Función de proceso | contexto | **void** o **promise** | Función que realiza la lógica de negocios. |
| Función de decisión | contexto | **string** o **promise** | Función que toma decisiones según la lógica de negocios. La cadena devuelta debe coincidir con una condición de un bloque de [decisión](conversation-designer-dialogues.md#decision-state). |
| Función de reconocimiento de código | contexto | **boolean** o **promise** | Lógica de negocios personalizada que se ejecuta cuando se produce un **desencadenador de script**. Devuelve `true` para indicar una coincidencia. De lo contrario, devuelve `false` para cancelar la coincidencia. |
| función onRecognize | contexto | **boolean** o **promise** | Función que se ejecuta solo si hay una coincidencia de un reconocedor de LUIS. Utilice esta función de devolución de llamada para procesar las entidades de LUIS y devolver un valor **booleano** adecuado. Devuelve `true` para indicar una coincidencia. De lo contrario, devuelve `false` para cancelar la coincidencia. |

## <a name="iconversationcontext-interface"></a>Interfaz IConversationContext

La interfaz `IConversationContext` realiza un seguimiento de información de la conversación entre el usuario y el bot. Todas las funciones personalizadas que se enlazan a través de Conversation Designer recibirán un objeto `context` como argumento del parámetro.

## <a name="context-properties"></a>Propiedades de contexto
El objeto `context` expone las siguientes propiedades.

| NOMBRE |  Código | DESCRIPCIÓN |
| ---- | ---- | ---- |
| `request` | `context.request` | Obtiene el objeto de la solicitud que contiene la actividad del bot.  |
| | `context.request.attachment` | Actividad de datos adjuntos que puede contener una tarjeta adaptable. |
| | `context.request.text` | Actividad de texto que contiene el mensaje de texto entrante del cliente. |
| | `context.request.speak` | Actividad de texto que contiene el texto oral (si está disponible) del cliente. |
| | `context.request.type` | Especifica el tipo de actividad (valor predeterminado: "message"). |
| `responses` | `context.responses` | Mantiene una matriz de actividades que se enviará al cliente al final del estado actual o del código subyacente a la ejecución. |
| | `context.responses.push` | Agrega una actividad a la respuesta. |
| `global` | `context.global` | Objeto de JavaScript que contiene los datos conversacionales definidos. Este objeto persiste a lo largo de la conversación. |
| `local` | `context.local` | Objeto de JavaScript que contiene los datos de la tarea definidos. Este objeto persiste mientras dura una tarea específica. Las intenciones de LUIS siempre se devuelven al contexto local. Si quiere continuar con los resultados de LUIS, considere copiarlos en el contexto `context.global`. |
| | `context.local['@description']` | Devuelve las entidades sin procesar recibidas de LUIS. |
| `sticky` | `context.sticky` | Indica el nombre de la tarea actual. |
| `currentTemplate` | `context.currentTemplate` | [Plantilla de respuesta condicional](conversation-designer-response-templates.md#conditional-response-templates) que se llama para las evaluaciones de visualización y habla. Este objeto contiene tres propiedades: <br/>1. **name**: nombre de la plantilla actual. <br/>2. **modalityDisplay**: valor booleano que indica que la modalidad está asociada a una evaluación de visualización. <br/>3. **modalitySpeak**: valor booleano que indica que la modalidad está asociada a una evaluación de habla. |

## <a name="context-methods"></a>Métodos de contexto
El objeto `context` expone los siguientes métodos.

| NOMBRE | Tipo de valor devuelto | Código | DESCRIPCIÓN |
| ---- | ---- | ---- | ---- |
| `getCurrentTurn` | **número** | `context.getCurrentTurn();` | Obtiene el giro del fotograma de la parte superior de la pila si está ejecutando una nueva solicitud de confirmación. |

## <a name="next-step"></a>Paso siguiente
> [!div class="nextstepaction"]
> [Creación de un bot](conversation-designer-create-bot.md)
