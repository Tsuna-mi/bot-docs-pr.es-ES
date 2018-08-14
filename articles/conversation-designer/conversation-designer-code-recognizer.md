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
# <a name="define-code-recognizer-as-task-trigger"></a><span data-ttu-id="3c959-103">Definición de un reconocedor de código como desencadenador de tareas</span><span class="sxs-lookup"><span data-stu-id="3c959-103">Define code recognizer as task trigger</span></span>
> [!IMPORTANT]
> <span data-ttu-id="3c959-104">Conversation Designer aún no está disponible para todos los clientes.</span><span class="sxs-lookup"><span data-stu-id="3c959-104">Conversation Designer is not available to all customers yet.</span></span> <span data-ttu-id="3c959-105">Podrá obtener más información sobre la disponibilidad de Conversation Designer a lo largo de este año.</span><span class="sxs-lookup"><span data-stu-id="3c959-105">More details on availability of Conversation Designer will come later this year.</span></span>

<span data-ttu-id="3c959-106">Los reconocedores de código le permiten escribir scripts personalizados que le ayuden a realizar una tarea.</span><span class="sxs-lookup"><span data-stu-id="3c959-106">Code recognizers enable you to write custom scripts to help perform a task.</span></span> <span data-ttu-id="3c959-107">La evaluación de expresiones basadas en regex o una llamada a otros servicios se pueden usar a fin de determinar la intención del usuario.</span><span class="sxs-lookup"><span data-stu-id="3c959-107">Regex-based expression evaluation or calling into other services can be used to help determine the user's intent.</span></span> <span data-ttu-id="3c959-108">El script personalizado indicará el tiempo de ejecución de la conversación para desencadenar una tarea.</span><span class="sxs-lookup"><span data-stu-id="3c959-108">The custom script will instruct the conversation runtime to trigger a task.</span></span> 

## <a name="create-a-script-function"></a><span data-ttu-id="3c959-109">Creación de una función de script</span><span class="sxs-lookup"><span data-stu-id="3c959-109">Create a script function</span></span>
<span data-ttu-id="3c959-110">Para utilizar un script como reconocedor, en el editor de tareas, elija "Función de script" como reconocedor, especifique el nombre de la función y haga clic en **Create/view function** (Crear o ver función) para empezar a editar un script.</span><span class="sxs-lookup"><span data-stu-id="3c959-110">To use a script as a recognizer, in the task editor, choose "Script function" as the recognizer, specify the name of the function, and Click **Create/view function** to start editing a script.</span></span> <span data-ttu-id="3c959-111">También puede hacer clic en **Create/view function** (Crear o ver función) sin especificar un nombre de script y se creará una función vacía con el nombre predeterminado.</span><span class="sxs-lookup"><span data-stu-id="3c959-111">You can also click **Create/view function** without specifying a script name and an empty function will be created for you with the default name.</span></span> 

## <a name="script-trigger-function-parameter"></a><span data-ttu-id="3c959-112">Parámetro de la función de desencadenador de script</span><span class="sxs-lookup"><span data-stu-id="3c959-112">Script trigger function parameter</span></span>

<span data-ttu-id="3c959-113">La función de devolución de llamada del script que se ha especificado se llamará siempre con el objeto [`context`](conversation-designer-context-object.md).</span><span class="sxs-lookup"><span data-stu-id="3c959-113">The specified script callback function will always be called with the [`context`](conversation-designer-context-object.md) object.</span></span>

<span data-ttu-id="3c959-114">El objeto `context` incluye `taskEntities` y `contextEntities`.</span><span class="sxs-lookup"><span data-stu-id="3c959-114">The `context` object includes both `taskEntities` and `contextEntities`.</span></span> <span data-ttu-id="3c959-115">Las entidades de la tarea son entidades definidas o generadas para esta tarea y las entidades de contexto son contenedores de propiedades donde se pueden conservar las entidades a través de las conversaciones con el usuario.</span><span class="sxs-lookup"><span data-stu-id="3c959-115">Task entities are entities defined or generated for this task and context entities are property bags where entities can be preserved across conversations with the user.</span></span>

## <a name="return-value"></a><span data-ttu-id="3c959-116">Valor devuelto</span><span class="sxs-lookup"><span data-stu-id="3c959-116">Return value</span></span>

<span data-ttu-id="3c959-117">Se espera que las funciones de **desencadenador de script** devuelvan un valor booleano.</span><span class="sxs-lookup"><span data-stu-id="3c959-117">**Script trigger** functions are expected to return a boolean.</span></span>

## <a name="sample-regex-based-recognizer"></a><span data-ttu-id="3c959-118">Ejemplo de reconocedor basado en regex</span><span class="sxs-lookup"><span data-stu-id="3c959-118">Sample regex-based recognizer</span></span>
<span data-ttu-id="3c959-119">El ejemplo de código siguiente usa regex para procesar la solicitud y devuelve un valor booleano, por lo que el tiempo de ejecución de la conversación puede determinar el desencadenador de script que se debe ejecutar.</span><span class="sxs-lookup"><span data-stu-id="3c959-119">The following code sample uses regex to process the request and returns a boolean so the conversation runtime can determine which script trigger to execute.</span></span>

```javascript
module.exports.fnFindPhoneTrigger = function(context) {
    if (context.request.text.includes("call") || context.request.text.includes("ring")) {
        return true;
    } else {
        return false;
    }
} 
```

## <a name="next-step"></a><span data-ttu-id="3c959-120">Paso siguiente</span><span class="sxs-lookup"><span data-stu-id="3c959-120">Next step</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="3c959-121">Acción de respuesta</span><span class="sxs-lookup"><span data-stu-id="3c959-121">Reply action</span></span>](conversation-designer-reply.md)
