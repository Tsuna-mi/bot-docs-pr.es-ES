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
# <a name="define-a-script-function-as-a-do-action"></a><span data-ttu-id="57564-103">Definición de una función de script como una acción Do</span><span class="sxs-lookup"><span data-stu-id="57564-103">Define a script function as a Do action</span></span>

<span data-ttu-id="57564-104">Las [funciones de acción de script](conversation-designer-context-object.md#script-callback-functions) ejecutan un script personalizado que ha definido para ayudar a completar la tarea.</span><span class="sxs-lookup"><span data-stu-id="57564-104">[Script action functions](conversation-designer-context-object.md#script-callback-functions) execute custom script you define to help complete the task.</span></span> <span data-ttu-id="57564-105">Por ejemplo, llamar a un servicio para que establezca realmente el termostato en 23 grados cuando el usuario diga "Establecer mi termostato en 23 grados".</span><span class="sxs-lookup"><span data-stu-id="57564-105">For example, calling into a service to actually set the thermostat to 74 degrees when the user says "Set my thermostat to 74 degrees."</span></span> 

<span data-ttu-id="57564-106">Para usar el script personalizado como acción, seleccione **Acción de script** como el tipo de acción **DO** en el editor de tareas.</span><span class="sxs-lookup"><span data-stu-id="57564-106">To use custom script as the action, select **Script action** as the **DO** action type in the task editor.</span></span> <span data-ttu-id="57564-107">Escriba el nombre de la función que implementa la acción.</span><span class="sxs-lookup"><span data-stu-id="57564-107">Then enter the name of the function that implements the action.</span></span> <span data-ttu-id="57564-108">Haga clic en **Editar** para que aparezca el editor de **scripts** y escriba su implementación para esta función.</span><span class="sxs-lookup"><span data-stu-id="57564-108">Click **Edit** to bring up the **Scripts** editor and write your implementation for this function.</span></span> 

## <a name="script-action-function-parameter"></a><span data-ttu-id="57564-109">Parámetro de función de acción de script</span><span class="sxs-lookup"><span data-stu-id="57564-109">Script action function parameter</span></span>

<span data-ttu-id="57564-110">La función de devolución de llamada de la acción que se ha especificado se llamará siempre con el objeto [`context`](conversation-designer-context-object.md).</span><span class="sxs-lookup"><span data-stu-id="57564-110">The specified action callback function will always be called with the [`context`](conversation-designer-context-object.md) object.</span></span>

<span data-ttu-id="57564-111">El objeto `context` incluye `taskEntities` y `contextEntities`.</span><span class="sxs-lookup"><span data-stu-id="57564-111">The `context` object includes both `taskEntities` and `contextEntities`.</span></span> <span data-ttu-id="57564-112">Las entidades de la tarea son entidades definidas o generadas para esta tarea y las entidades de contexto son contenedores de propiedades donde se pueden conservar las entidades en las conversaciones con el usuario.</span><span class="sxs-lookup"><span data-stu-id="57564-112">Task entities are entities defined or generated for this task and context entities are a property bag where entities can be preserved across conversations with the user.</span></span>

## <a name="return-value"></a><span data-ttu-id="57564-113">Valor devuelto</span><span class="sxs-lookup"><span data-stu-id="57564-113">Return value</span></span>
<span data-ttu-id="57564-114">Se espera que las funciones de **acción de script** devuelvan un valor booleano.</span><span class="sxs-lookup"><span data-stu-id="57564-114">**Script action** functions are expected to return a boolean.</span></span>

## <a name="sample-script-action-function"></a><span data-ttu-id="57564-115">Ejemplo de función de acción de script</span><span class="sxs-lookup"><span data-stu-id="57564-115">Sample script action function</span></span>
<span data-ttu-id="57564-116">La función del ejemplo siguiente realiza una llamada HTTP para obtener una respuesta antes de devolver un desencadenador coincidente.</span><span class="sxs-lookup"><span data-stu-id="57564-116">The following sample function makes an HTTP call to get a response before returning a trigger matched.</span></span>

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

## <a name="next-step"></a><span data-ttu-id="57564-117">Paso siguiente</span><span class="sxs-lookup"><span data-stu-id="57564-117">Next step</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="57564-118">Acción de diálogo</span><span class="sxs-lookup"><span data-stu-id="57564-118">Dialogue action</span></span>](conversation-designer-dialogues.md)
