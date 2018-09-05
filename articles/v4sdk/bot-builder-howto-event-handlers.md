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
# <a name="using-event-handlers"></a>Uso de controladores de eventos

[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]

Los controladores de eventos son funciones que podemos agregar a eventos de actividad futura dentro de un [turno](bot-builder-basics.md#defining-a-turn). Dichas actividades son `SendActivity`, `UpdateActivity` y `DeleteActivity`, y cada una tiene su propio controlador. Estos controladores son útiles cuando necesite realizar algo en cada actividad futura de ese tipo para el objeto de contexto actual.

Puede agregar varios controladores de eventos a cada evento de actividad, y se procesarán para cada evento **después** una vez agregados, así que es importante dónde los agregue en el código.

> [!NOTE]
> Los *controladores de eventos* de este artículo no cumplen las definiciones específicas del lenguaje tradicional de *eventos* y *controladores*. En su lugar, hacen referencia a la idea de programación más conceptual de *eventos* y *controladores*.

## <a name="using-the-next-delegate"></a>Uso del delegado *next*

Al llamar al delegado *next*, cada controlador pasa la ejecución al siguiente controlador en el orden en que se agregaron los controladores. El último delegado *next* es una llamada al adaptador para enviar, actualizar o eliminar la actividad realmente.

A menos que se indique lo contrario, es importante llamar a *next()* dentro de su controlador, si no se [bloqueará](bot-builder-create-middleware.md#short-circuit-routing) la actividad. La diferencia entre bloquear una actividad frente al middleware es que lo primero cancela completamente la actividad, mientras que el middleware cambia el código que se puede ejecutar, pero la ejecución de la actividad continúa.

Para las actividades de envío, si se realizan correctamente, el delegado *next* devuelve los identificadores asignados a los mensajes enviados.

## <a name="expected-return-value"></a>Valor devuelto esperado

Para el valor devuelto, el controlador de eventos espera que sea el resultado del siguiente delegado. Si es necesario, ese resultado se puede ver y almacenar antes de devolverlo, o se puede devolver directamente tal como se muestra a continuación.

El valor devuelto del controlador `SendActivity` es el valor devuelto que se pasa en la cadena como el valor devuelto del delegado siguiente correspondiente.

# <a name="ctabcseventhandler"></a>[C#](#tab/cseventhandler)

Los tres tipos de controladores de eventos proporcionados son `OnSendActivities()`, `OnUpdateActivity()` y `OnDeleteActivity()`. En este caso, estamos agregando un controlador `OnSendActivities()` dentro de nuestro código de bot, pero también se pueden agregar controladores en el código de middleware.

A menudo, los controladores se agregan como expresiones lambda, que verá en este ejemplo. Aquí, escucharemos la actividad de entrada del usuario cuando se escribe **ayuda**.

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

# <a name="javascripttabjseventhandler"></a>[JavaScript](#tab/jseventhandler)

Los tres tipos de controladores de eventos proporcionados son `onSendActivities()`, `onUpdateActivity()` y `onDeleteActivity()`. En este caso, estamos agregando un controlador `onSendActivities()` dentro de nuestro código de bot, pero también se pueden agregar controladores en el código de middleware.

En este ejemplo, escucharemos la actividad de entrada del usuario cuando se escribe **ayuda**.

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

Es importante diferenciar entre los eventos *actividad de envío* y *actividad de actualización o eliminación*; en el primero se crea un evento de actividad completamente nuevo y en el segundo se actúa sobre una actividad pasada. También: No todos los canales admiten actividades de *actualización* o *eliminación*. Para tener en cuenta esa posibilidad, es recomendable agregar el control de excepciones adecuado en torno a las llamadas a estas actividades y sus controladores.

