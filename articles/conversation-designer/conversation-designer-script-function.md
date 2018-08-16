---
title: Definición de una función de script como una acción Do | Microsoft Docs
description: Aprenda a configurar una función de acción de script como una acción Do.
author: vkannan
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ROBOTS: NoIndex, NoFollow
ms.openlocfilehash: 674781c2cb5a8700941feb59b2ce7ab902979d4f
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39306205"
---
# <a name="define-a-script-function-as-a-do-action"></a>Definición de una función de script como una acción Do

Las [funciones de acción de script](conversation-designer-context-object.md#script-callback-functions) ejecutan un script personalizado que ha definido para ayudar a completar la tarea. Por ejemplo, llamar a un servicio para que establezca realmente el termostato en 23 grados cuando el usuario diga "Establecer mi termostato en 23 grados". 

Para usar el script personalizado como acción, seleccione **Acción de script** como el tipo de acción **DO** en el editor de tareas. Escriba el nombre de la función que implementa la acción. Haga clic en **Editar** para que aparezca el editor de **scripts** y escriba su implementación para esta función. 

## <a name="script-action-function-parameter"></a>Parámetro de función de acción de script

La función de devolución de llamada de la acción que se ha especificado se llamará siempre con el objeto [`context`](conversation-designer-context-object.md).

El objeto `context` incluye `taskEntities` y `contextEntities`. Las entidades de la tarea son entidades definidas o generadas para esta tarea y las entidades de contexto son contenedores de propiedades donde se pueden conservar las entidades en las conversaciones con el usuario.

## <a name="return-value"></a>Valor devuelto
Se espera que las funciones de **acción de script** devuelvan un valor booleano.

## <a name="sample-script-action-function"></a>Ejemplo de función de acción de script
La función del ejemplo siguiente realiza una llamada HTTP para obtener una respuesta antes de devolver un desencadenador coincidente.

```javascript
module.exports.NewTask_do_onRun = function(context) {
    var options =  {
        host: 'HOST',
        path: 'PATH',
        method: 'post',
        headers: {
            "HEADER1" : "VALUE"
        }, 
        body: {
            "BODY": VALUE
        }
    };

    return http.request(options).then(function(response) {
      // parse response payload
      // Send a message to user
      context.responses.push({text: "Done", type: "message"});
    });
} 
```

## <a name="next-step"></a>Paso siguiente
> [!div class="nextstepaction"]
> [Acción de diálogo](conversation-designer-dialogues.md)
