---
title: Probar un bot | Microsoft Docs
description: Probar y depurar el bot de Conversation Designer.
author: vkannan
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ROBOTS: NoIndex, NoFollow
ms.openlocfilehash: b08bd96bc7c413ff7cb6db4899c8c0d3ad613ab8
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39306269"
---
# <a name="test-your-conversation-designer-bot"></a><span data-ttu-id="7592d-103">Probar el bot de Conversation Designer</span><span class="sxs-lookup"><span data-stu-id="7592d-103">Test your Conversation Designer bot</span></span>
> [!IMPORTANT]
> <span data-ttu-id="7592d-104">Conversation Designer no está disponible para todos los clientes todavía.</span><span class="sxs-lookup"><span data-stu-id="7592d-104">Conversation Designer is not available to all customers yet.</span></span> <span data-ttu-id="7592d-105">Habrá más información sobre la disponibilidad de Conversation Designer más adelante este año.</span><span class="sxs-lookup"><span data-stu-id="7592d-105">More details on availability of Conversation Designer will come later this year.</span></span>

<span data-ttu-id="7592d-106">Conversation Designer ofrece herramientas útiles de depuración a través de varias ventanas de salida diferentes (que se encuentra en la parte inferior de la pantalla).</span><span class="sxs-lookup"><span data-stu-id="7592d-106">The Conversation Designer offer useful debugging tools through several different Output window (located at the bottom of the screen).</span></span> <span data-ttu-id="7592d-107">Para comenzar las pruebas y la depuración, puede hacer clic en el botón **Probar** que se encuentra en la esquina superior derecha de la ventana.</span><span class="sxs-lookup"><span data-stu-id="7592d-107">You can start testing and debugging by clicking on the **Test** button found on the upper right corner of the window.</span></span> 

## <a name="validation-errors"></a><span data-ttu-id="7592d-108">Errores de validación</span><span class="sxs-lookup"><span data-stu-id="7592d-108">Validation Errors</span></span>
<span data-ttu-id="7592d-109">Hay varias condiciones en las que se producen errores de validación.</span><span class="sxs-lookup"><span data-stu-id="7592d-109">There are several conditions under which validation errors occur.</span></span> <span data-ttu-id="7592d-110">Por ejemplo: </span><span class="sxs-lookup"><span data-stu-id="7592d-110">For example:</span></span> 
- <span data-ttu-id="7592d-111">Falta una respuesta al desencadenador</span><span class="sxs-lookup"><span data-stu-id="7592d-111">you are missing a response to the trigger</span></span> 
- <span data-ttu-id="7592d-112">Hay información parcialmente completa en el flujo</span><span class="sxs-lookup"><span data-stu-id="7592d-112">partially complete information on flow</span></span>
- <span data-ttu-id="7592d-113">Faltan desencadenadores y código subyacente para los estados de plantilla de respuesta condicional, de decisión y de proceso</span><span class="sxs-lookup"><span data-stu-id="7592d-113">missing triggers and code behind for conditional response template, decision and process states</span></span>
- <span data-ttu-id="7592d-114">Se detecta un bucle infinito o recursivo en las referencias de plantilla</span><span class="sxs-lookup"><span data-stu-id="7592d-114">a recursive or infinite loop is detected in the template references</span></span> 
- <span data-ttu-id="7592d-115">El cuadro de diálogo tiene un estado huérfano que no está conectado al flujo.</span><span class="sxs-lookup"><span data-stu-id="7592d-115">your dialogue has an orphan state that is not connected to the flow.</span></span>
- <span data-ttu-id="7592d-116">Se agregó una tarea, pero faltan el desencadenador o la acción</span><span class="sxs-lookup"><span data-stu-id="7592d-116">you added a task, but the trigger or action is missing</span></span> 


## <a name="javascript-output"></a><span data-ttu-id="7592d-117">Salida de JavaScript</span><span class="sxs-lookup"><span data-stu-id="7592d-117">JavaScript Output</span></span>
<span data-ttu-id="7592d-118">Puede adjuntar un script a la ejecución de una respuesta para proporcionar funcionalidad adicional.</span><span class="sxs-lookup"><span data-stu-id="7592d-118">You can attach a script to the execution of a response to provide additional functionality.</span></span> <span data-ttu-id="7592d-119">Si se produce un error en el script, se mostrará aquí.</span><span class="sxs-lookup"><span data-stu-id="7592d-119">If there is an error in the script, it will be displayed here.</span></span> <span data-ttu-id="7592d-120">Puede agregar `console.log()` o `error.log()` o `debug.log()` a la lógica de negocios de su bot, que mostrará los mensajes en la ventana de salida.</span><span class="sxs-lookup"><span data-stu-id="7592d-120">You can add `console.log()` or `error.log()` or `debug.log()` to your bot's business logic, which will list messages in the output window.</span></span> <span data-ttu-id="7592d-121">Por ejemplo: </span><span class="sxs-lookup"><span data-stu-id="7592d-121">For example:</span></span>

``` javascript
module.exports.Respond_beforeResponse = function(context) {
    console.log(JSON.stringify(context));
}
```

## <a name="runtime-output"></a><span data-ttu-id="7592d-122">Salida en tiempo de ejecución</span><span class="sxs-lookup"><span data-stu-id="7592d-122">Runtime Output</span></span>
<span data-ttu-id="7592d-123">Aquí se muestran los errores o excepciones generados por el tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="7592d-123">Errors or exceptions generated by the runtime are displayed here.</span></span> <span data-ttu-id="7592d-124">Por ejemplo, si el bot no responde, examine la salida para investigar la causa de la excepción o error.</span><span class="sxs-lookup"><span data-stu-id="7592d-124">For example, if your bot is failing to respond, look at the output to investigate what caused the exception or error.</span></span> <span data-ttu-id="7592d-125">Si hay demasiados mensajes en la ventana de salida, haga clic en **Borrar todo** y, a continuación, pruebe de nuevo el bot enviando un mensaje al bot para ver los detalles del error.</span><span class="sxs-lookup"><span data-stu-id="7592d-125">If there are too many messages in the output window, you can click **Clear all**, and then test your bot again by sending the bot a message to see the error details.</span></span> 

## <a name="next-step"></a><span data-ttu-id="7592d-126">Paso siguiente</span><span class="sxs-lookup"><span data-stu-id="7592d-126">Next step</span></span>
> [!div class="nextstepaction"]
> <span data-ttu-id="7592d-127">[Import and export bot](conversation-designer-export-import-bot.md) (Importación y exportación de bots)</span><span class="sxs-lookup"><span data-stu-id="7592d-127">[Import and export bot](conversation-designer-export-import-bot.md)</span></span>
