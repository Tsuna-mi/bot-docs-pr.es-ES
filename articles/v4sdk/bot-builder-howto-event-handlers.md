---
title: Uso de controladores de eventos | Microsoft Docs
description: Comprenda cómo usar controladores de eventos.
author: ivorb
ms.author: v-ivorb
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 06/14/2018
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: 36bd2638c599de1662a37dd85790b6126184d51b
ms.sourcegitcommit: 2dc75701b169d822c9499e393439161bc87639d2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42905694"
---
# <a name="using-event-handlers"></a><span data-ttu-id="b17d8-103">Uso de controladores de eventos</span><span class="sxs-lookup"><span data-stu-id="b17d8-103">Using event handlers</span></span>

[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]

<span data-ttu-id="b17d8-104">Los controladores de eventos son funciones que podemos agregar a eventos de actividad futura dentro de un [turno](bot-builder-basics.md#defining-a-turn).</span><span class="sxs-lookup"><span data-stu-id="b17d8-104">Event handlers are functions we can add to future activity events within a [turn](bot-builder-basics.md#defining-a-turn).</span></span> <span data-ttu-id="b17d8-105">Dichas actividades son `SendActivity`, `UpdateActivity` y `DeleteActivity`, y cada una tiene su propio controlador.</span><span class="sxs-lookup"><span data-stu-id="b17d8-105">Those activities are `SendActivity`, `UpdateActivity`, and `DeleteActivity`, and each has it's own handler.</span></span> <span data-ttu-id="b17d8-106">Estos controladores son útiles cuando necesite realizar algo en cada actividad futura de ese tipo para el objeto de contexto actual.</span><span class="sxs-lookup"><span data-stu-id="b17d8-106">These handlers are useful when you need to do something on every future activity of that type for the current context object.</span></span>

<span data-ttu-id="b17d8-107">Puede agregar varios controladores de eventos a cada evento de actividad, y se procesarán para cada evento **después** una vez agregados, así que es importante dónde los agregue en el código.</span><span class="sxs-lookup"><span data-stu-id="b17d8-107">You can add multiple event handlers to each activity event, and they will process for every event **after** they are added, so where in the code you add them matters.</span></span>

> [!NOTE]
> <span data-ttu-id="b17d8-108">Los *controladores de eventos* de este artículo no cumplen las definiciones específicas del lenguaje tradicional de *eventos* y *controladores*.</span><span class="sxs-lookup"><span data-stu-id="b17d8-108">*Event handlers* in this article are not adhering to traditional language specific definitions of *events* and *handlers*.</span></span> <span data-ttu-id="b17d8-109">En su lugar, hacen referencia a la idea de programación más conceptual de *eventos* y *controladores*.</span><span class="sxs-lookup"><span data-stu-id="b17d8-109">Instead, they refer to the more conceptual programming idea of *events* and *handlers*.</span></span>

## <a name="using-the-next-delegate"></a><span data-ttu-id="b17d8-110">Uso del delegado *next*</span><span class="sxs-lookup"><span data-stu-id="b17d8-110">Using the *next* delegate</span></span>

<span data-ttu-id="b17d8-111">Al llamar al delegado *next*, cada controlador pasa la ejecución al siguiente controlador en el orden en que se agregaron los controladores.</span><span class="sxs-lookup"><span data-stu-id="b17d8-111">By calling the *next* delegate, each handler passes execution to the following handler in the order in which the handlers were added.</span></span> <span data-ttu-id="b17d8-112">El último delegado *next* es una llamada al adaptador para enviar, actualizar o eliminar la actividad realmente.</span><span class="sxs-lookup"><span data-stu-id="b17d8-112">The last *next* delegate is a call to the adapter to actually send, update, or delete the activity.</span></span>

<span data-ttu-id="b17d8-113">A menos que se indique lo contrario, es importante llamar a *next()* dentro de su controlador, si no se [bloqueará](bot-builder-create-middleware.md#short-circuit-routing) la actividad.</span><span class="sxs-lookup"><span data-stu-id="b17d8-113">Unless otherwise intended, it is important to call *next()* within your handler, otherwise you will [short circuit](bot-builder-create-middleware.md#short-circuit-routing) the activity.</span></span> <span data-ttu-id="b17d8-114">La diferencia entre bloquear una actividad frente al middleware es que lo primero cancela completamente la actividad, mientras que el middleware cambia el código que se puede ejecutar, pero la ejecución de la actividad continúa.</span><span class="sxs-lookup"><span data-stu-id="b17d8-114">The difference between short circuiting an activity versus middleware is that short circuiting completely cancels the activity, whereas middleware changes what code is allowed to run, but the activity’s execution continues.</span></span>

<span data-ttu-id="b17d8-115">Para las actividades de envío, si se realizan correctamente, el delegado *next* devuelve los identificadores asignados a los mensajes enviados.</span><span class="sxs-lookup"><span data-stu-id="b17d8-115">For send activities, if successful, the *next* delegate returns the ID(s) assigned to the sent message(s).</span></span>

## <a name="expected-return-value"></a><span data-ttu-id="b17d8-116">Valor devuelto esperado</span><span class="sxs-lookup"><span data-stu-id="b17d8-116">Expected return value</span></span>

<span data-ttu-id="b17d8-117">Para el valor devuelto, el controlador de eventos espera que sea el resultado del siguiente delegado.</span><span class="sxs-lookup"><span data-stu-id="b17d8-117">For the return value, the event handler expects it to be the result of the next delegate.</span></span> <span data-ttu-id="b17d8-118">Si es necesario, ese resultado se puede ver y almacenar antes de devolverlo, o se puede devolver directamente tal como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="b17d8-118">If needed, that result can be viewed and stored before returning it, or it can be returned directly as is seen below.</span></span>

<span data-ttu-id="b17d8-119">El valor devuelto del controlador `SendActivity` es el valor devuelto que se pasa en la cadena como el valor devuelto del delegado siguiente correspondiente.</span><span class="sxs-lookup"><span data-stu-id="b17d8-119">The return value of the `SendActivity` handler is the return value that gets passed back up through the chain as the return value of the corresponding next delegate.</span></span>

# <a name="ctabcseventhandler"></a>[<span data-ttu-id="b17d8-120">C#</span><span class="sxs-lookup"><span data-stu-id="b17d8-120">C#</span></span>](#tab/cseventhandler)

<span data-ttu-id="b17d8-121">Los tres tipos de controladores de eventos proporcionados son `OnSendActivities()`, `OnUpdateActivity()` y `OnDeleteActivity()`.</span><span class="sxs-lookup"><span data-stu-id="b17d8-121">The three types of event handlers provided are `OnSendActivities()`, `OnUpdateActivity()`, and `OnDeleteActivity()`.</span></span> <span data-ttu-id="b17d8-122">En este caso, estamos agregando un controlador `OnSendActivities()` dentro de nuestro código de bot, pero también se pueden agregar controladores en el código de middleware.</span><span class="sxs-lookup"><span data-stu-id="b17d8-122">Here, we're adding an `OnSendActivities()` handler within our bot code, however handlers can be added in middleware code as well.</span></span>

<span data-ttu-id="b17d8-123">A menudo, los controladores se agregan como expresiones lambda, que verá en este ejemplo.</span><span class="sxs-lookup"><span data-stu-id="b17d8-123">Handlers are often added as lambda expressions, which you'll see in this example.</span></span> <span data-ttu-id="b17d8-124">Aquí, escucharemos la actividad de entrada del usuario cuando se escribe **ayuda**.</span><span class="sxs-lookup"><span data-stu-id="b17d8-124">Here, we will listen to the user's input activity when **help** is written.</span></span>

```cs
public Task OnTurn(ITurnContext context)
{
    if (context.Activity.Type == ActivityTypes.Message)
    {
        // Get the conversation state from the turn context
        
        context.OnSendActivities(async (handlerContext, activities, handlerNext) =>
        {
            if (handlerContext.Activity.Text == "help")
            {
                Console.WriteLine("help!");
                // Do whatever logging you want to do for this help message
            }

            return await handlerNext();
        });
        // ...
    }
}
```

# <a name="javascripttabjseventhandler"></a>[<span data-ttu-id="b17d8-125">JavaScript</span><span class="sxs-lookup"><span data-stu-id="b17d8-125">JavaScript</span></span>](#tab/jseventhandler)

<span data-ttu-id="b17d8-126">Los tres tipos de controladores de eventos proporcionados son `onSendActivities()`, `onUpdateActivity()` y `onDeleteActivity()`.</span><span class="sxs-lookup"><span data-stu-id="b17d8-126">The three types of event handlers provided are `onSendActivities()`, `onUpdateActivity()`, and `onDeleteActivity()`.</span></span> <span data-ttu-id="b17d8-127">En este caso, estamos agregando un controlador `onSendActivities()` dentro de nuestro código de bot, pero también se pueden agregar controladores en el código de middleware.</span><span class="sxs-lookup"><span data-stu-id="b17d8-127">Here, we're adding an `onSendActivities()` handler within our bot code, however handlers can be added in middleware code as well.</span></span>

<span data-ttu-id="b17d8-128">En este ejemplo, escucharemos la actividad de entrada del usuario cuando se escribe **ayuda**.</span><span class="sxs-lookup"><span data-stu-id="b17d8-128">This example will listen to the user's input activity when **help** is written.</span></span>

```js
adapter.processActivity(req, res, async (context) => {

    if (context.activity.type === 'message') {

        context.onSendActivities(async (handlerContext, activities, handlerNext) => { 
            
            if(handlerContext.activity.text === 'help'){
                console.log('help!')
                // Do whatever logging you want to do for this help message
            }
            // Add handler logic here
        
            await handlerNext(); 
        });
        await context.sendActivity(`you said ${context.activity.text}`);
    }
});
```

---

<span data-ttu-id="b17d8-129">Es importante diferenciar entre los eventos *actividad de envío* y *actividad de actualización o eliminación*; en el primero se crea un evento de actividad completamente nuevo y en el segundo se actúa sobre una actividad pasada.</span><span class="sxs-lookup"><span data-stu-id="b17d8-129">It's important to differentiate between *send activity* and *update or delete activity* events, where the first creates an entirely new activity event and the later acts on a past activity.</span></span> <span data-ttu-id="b17d8-130">También:</span><span class="sxs-lookup"><span data-stu-id="b17d8-130">Also.</span></span> <span data-ttu-id="b17d8-131">No todos los canales admiten actividades de *actualización* o *eliminación*.</span><span class="sxs-lookup"><span data-stu-id="b17d8-131">not all channels support *update* or *delete* activity.</span></span> <span data-ttu-id="b17d8-132">Para tener en cuenta esa posibilidad, es recomendable agregar el control de excepciones adecuado en torno a las llamadas a estas actividades y sus controladores.</span><span class="sxs-lookup"><span data-stu-id="b17d8-132">It's suggested to add the appropriate exception handling around calls to these and their handlers to account for that possibility.</span></span>

