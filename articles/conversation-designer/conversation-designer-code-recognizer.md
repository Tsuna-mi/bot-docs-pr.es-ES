---
title: Definición de un reconocedor de código como desencadenador de tareas | Microsoft Docs
description: Obtenga información sobre cómo usar un reconocedor de código personalizado como desencadenador de tareas.
author: vkannan
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ROBOTS: NoIndex, NoFollow
ms.openlocfilehash: 6d44cd299e46971b2218bb78d16e454d5df3c1d9
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39304811"
---
# <a name="define-code-recognizer-as-task-trigger"></a>Definición de un reconocedor de código como desencadenador de tareas
> [!IMPORTANT]
> Conversation Designer aún no está disponible para todos los clientes. Podrá obtener más información sobre la disponibilidad de Conversation Designer a lo largo de este año.

Los reconocedores de código le permiten escribir scripts personalizados que le ayuden a realizar una tarea. La evaluación de expresiones basadas en regex o una llamada a otros servicios se pueden usar a fin de determinar la intención del usuario. El script personalizado indicará el tiempo de ejecución de la conversación para desencadenar una tarea. 

## <a name="create-a-script-function"></a>Creación de una función de script
Para utilizar un script como reconocedor, en el editor de tareas, elija "Función de script" como reconocedor, especifique el nombre de la función y haga clic en **Create/view function** (Crear o ver función) para empezar a editar un script. También puede hacer clic en **Create/view function** (Crear o ver función) sin especificar un nombre de script y se creará una función vacía con el nombre predeterminado. 

## <a name="script-trigger-function-parameter"></a>Parámetro de la función de desencadenador de script

La función de devolución de llamada del script que se ha especificado se llamará siempre con el objeto [`context`](conversation-designer-context-object.md).

El objeto `context` incluye `taskEntities` y `contextEntities`. Las entidades de la tarea son entidades definidas o generadas para esta tarea y las entidades de contexto son contenedores de propiedades donde se pueden conservar las entidades a través de las conversaciones con el usuario.

## <a name="return-value"></a>Valor devuelto

Se espera que las funciones de **desencadenador de script** devuelvan un valor booleano.

## <a name="sample-regex-based-recognizer"></a>Ejemplo de reconocedor basado en regex
El ejemplo de código siguiente usa regex para procesar la solicitud y devuelve un valor booleano, por lo que el tiempo de ejecución de la conversación puede determinar el desencadenador de script que se debe ejecutar.

```javascript
module.exports.fnFindPhoneTrigger = function(context) {
    if (context.request.text.includes("call") || context.request.text.includes("ring")) {
        return true;
    } else {
        return false;
    }
} 
```

## <a name="next-step"></a>Paso siguiente
> [!div class="nextstepaction"]
> [Acción de respuesta](conversation-designer-reply.md)
