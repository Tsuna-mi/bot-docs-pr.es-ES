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
# <a name="response-template-for-conversation-designer-bots"></a><span data-ttu-id="ead9d-103">Plantilla de respuesta para los bots de Conversation Designer</span><span class="sxs-lookup"><span data-stu-id="ead9d-103">Response template for Conversation Designer bots</span></span>
> [!IMPORTANT]
> <span data-ttu-id="ead9d-104">Conversation Designer no está disponible para todos los clientes todavía.</span><span class="sxs-lookup"><span data-stu-id="ead9d-104">Conversation Designer is not available to all customers yet.</span></span> <span data-ttu-id="ead9d-105">Habrá más información sobre la disponibilidad de Conversation Designer más adelante este año.</span><span class="sxs-lookup"><span data-stu-id="ead9d-105">More details on availability of Conversation Designer will come later this year.</span></span>

<span data-ttu-id="ead9d-106">La generación de lenguaje permite al bot comunicarse con el usuario de forma fluida y natural mediante mensajes de respuesta variable.</span><span class="sxs-lookup"><span data-stu-id="ead9d-106">Language generation enables your bot to communicate with the user in a rich and natural manner using variable response messages.</span></span> <span data-ttu-id="ead9d-107">Estos mensajes se administran mediante plantillas de respuesta de Conversation Designer.</span><span class="sxs-lookup"><span data-stu-id="ead9d-107">These messages are managed through response templates in Conversation Designer.</span></span>

<span data-ttu-id="ead9d-108">Las plantillas de respuesta permiten la reutilización de respuestas y ayudan a mantener un tono y un lenguaje coherentes en las respuestas del bot.</span><span class="sxs-lookup"><span data-stu-id="ead9d-108">Response templates enable response reuse and helps maintain a consistent tone and language in bot responses.</span></span> 

## <a name="create-response-templates"></a><span data-ttu-id="ead9d-109">Creación de plantillas de respuesta</span><span class="sxs-lookup"><span data-stu-id="ead9d-109">Create response templates</span></span>

<span data-ttu-id="ead9d-110">En Conversation Designer, puede crear plantillas de respuesta que puede volver a usar en cualquier situación en la que tenga que enviar una respuesta a un usuario.</span><span class="sxs-lookup"><span data-stu-id="ead9d-110">In Conversation Designer, you can create response templates that you can reuse in any situation where you need to send a response to a user.</span></span> 

<span data-ttu-id="ead9d-111">Las plantillas de respuesta simples definen una colección única de expresiones que se pueden verbalizar o mostrar en pantalla.</span><span class="sxs-lookup"><span data-stu-id="ead9d-111">Simple response templates define a one-off collection of possible speak and display utterances.</span></span> <span data-ttu-id="ead9d-112">Después, puede usar estas plantillas en las respuestas del bot, los estados del aviso o en la tarjetas adaptables que se usan para reconstruir una cadena totalmente resuelta.</span><span class="sxs-lookup"><span data-stu-id="ead9d-112">You can then use these templates in your bot's responses, prompt states, or in adaptive cards to re-construct a fully resolved string.</span></span>

<span data-ttu-id="ead9d-113">Para agregar una plantilla de respuesta simple, haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="ead9d-113">To add a simple response template, do the following:</span></span>
1. <span data-ttu-id="ead9d-114">En el panel izquierdo, haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="ead9d-114">From the left panel, click **Add**.</span></span> <span data-ttu-id="ead9d-115">Aparecerá un menú contextual.</span><span class="sxs-lookup"><span data-stu-id="ead9d-115">A context menu will show up.</span></span>
2. <span data-ttu-id="ead9d-116">Haga clic en **Respuesta simple**.</span><span class="sxs-lookup"><span data-stu-id="ead9d-116">Click **Simple response**.</span></span> <span data-ttu-id="ead9d-117">Aparece una ventana de edición en el panel de contenido principal.</span><span class="sxs-lookup"><span data-stu-id="ead9d-117">An edit window appears in the main content panel.</span></span>
3. <span data-ttu-id="ead9d-118">En el campo **Simple response name** (Nombre de la respuesta simple), escriba un nombre para esta respuesta simple.</span><span class="sxs-lookup"><span data-stu-id="ead9d-118">In the **Simple response name** field, enter a name for this simple response.</span></span>
4. <span data-ttu-id="ead9d-119">En el campo **Bot's response to user** (Respuesta del bot al usuario), escriba la respuesta, una frase cada vez.</span><span class="sxs-lookup"><span data-stu-id="ead9d-119">In the **Bot's response to user** field, enter the response one phrase at a time.</span></span>
5. <span data-ttu-id="ead9d-120">En la esquina superior derecha, haga clic en **Guardar** cuando haya terminado de configurar la respuesta simple.</span><span class="sxs-lookup"><span data-stu-id="ead9d-120">From the upper right corner, click **Save** when you are finished setting up the simple response.</span></span> 

<span data-ttu-id="ead9d-121">Por ejemplo, puede crear una plantilla de frase de confirmación (llámela, "AckPhrase") con los siguientes elementos:</span><span class="sxs-lookup"><span data-stu-id="ead9d-121">For example, you can create an acknowledgement phrase template (call it, "AckPhrase") with the following items:</span></span>

- <span data-ttu-id="ead9d-122">Aceptar</span><span class="sxs-lookup"><span data-stu-id="ead9d-122">Ok</span></span>
- <span data-ttu-id="ead9d-123">Por supuesto</span><span class="sxs-lookup"><span data-stu-id="ead9d-123">Sure</span></span>
- <span data-ttu-id="ead9d-124">Seguro</span><span class="sxs-lookup"><span data-stu-id="ead9d-124">You bet</span></span>
- <span data-ttu-id="ead9d-125">Será un placer ayudarle</span><span class="sxs-lookup"><span data-stu-id="ead9d-125">I'd be happy to help</span></span>
- <span data-ttu-id="ead9d-126">Me alegra poder ayudarle</span><span class="sxs-lookup"><span data-stu-id="ead9d-126">Glad to help</span></span>

<span data-ttu-id="ead9d-127">Con esta plantilla, el entorno en tiempo de ejecución resuelve con una de estas cadenas aleatoriamente para que las respuestas del bot sean más naturales y menos aburridas y monótonas.</span><span class="sxs-lookup"><span data-stu-id="ead9d-127">Using this template, the conversation runtime resolves to one of these strings randomly so that the bot's responses sound more natural instead of dull and monotonous.</span></span>

## <a name="conditional-response-templates"></a><span data-ttu-id="ead9d-128">Plantillas de respuesta condicional</span><span class="sxs-lookup"><span data-stu-id="ead9d-128">Conditional response templates</span></span>

<span data-ttu-id="ead9d-129">Las plantillas de respuesta condicional permiten especificar varias respuestas con condiciones.</span><span class="sxs-lookup"><span data-stu-id="ead9d-129">Conditional response templates specify multiple responses with conditions.</span></span> <span data-ttu-id="ead9d-130">La función de devolución de llamada que escribe ayuda al motor de generación de lenguaje a decidir qué cadena de respuesta debe usar según la condición que haya especificado.</span><span class="sxs-lookup"><span data-stu-id="ead9d-130">The callback function you write helps the language generation engine decide which response string to use based on the condition you specified.</span></span> 

<span data-ttu-id="ead9d-131">Por ejemplo, una plantilla de hora del día resolvería con "buenos días" o "buenas tardes" según la hora real del día.</span><span class="sxs-lookup"><span data-stu-id="ead9d-131">For example, a time of day template should resolve to "good morning" or "good evening" based on the actual time of day.</span></span> 

<span data-ttu-id="ead9d-132">Para agregar una plantilla de respuesta condicional, haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="ead9d-132">To add a conditional response template, do the following:</span></span>
1. <span data-ttu-id="ead9d-133">En el panel izquierdo, haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="ead9d-133">From left panel, click **Add**.</span></span> <span data-ttu-id="ead9d-134">Aparecerá un menú contextual.</span><span class="sxs-lookup"><span data-stu-id="ead9d-134">A context menu will appear.</span></span>
2. <span data-ttu-id="ead9d-135">Haga clic en **Respuesta condicional**.</span><span class="sxs-lookup"><span data-stu-id="ead9d-135">Click **Conditional response**.</span></span> <span data-ttu-id="ead9d-136">Aparece una ventana de edición en el panel de contenido principal.</span><span class="sxs-lookup"><span data-stu-id="ead9d-136">An edit window appears in the main content panel.</span></span>
3. <span data-ttu-id="ead9d-137">En el campo **Conditional response name** (Nombre de la respuesta condicional), escriba un nombre para esta plantilla.</span><span class="sxs-lookup"><span data-stu-id="ead9d-137">In the **Conditional response name** field, enter a name for this template.</span></span>
4. <span data-ttu-id="ead9d-138">En el campo **Respuesta condicional**, escriba las frases, una cada vez.</span><span class="sxs-lookup"><span data-stu-id="ead9d-138">In the **Conditional response** field, enter your phrases one at a time.</span></span> <span data-ttu-id="ead9d-139">Por ejemplo, para "timeOfDayTemplate", escriba "Buenos días" y "Buenas tardes" como dos respuestas posibles en la plantilla.</span><span class="sxs-lookup"><span data-stu-id="ead9d-139">For example, for the "timeOfDayTemplate", enter "Good morning" and "Good evening" as two possible responses in the template.</span></span>
5. <span data-ttu-id="ead9d-140">Para la respuesta "Buenos días", especifique su **nombre de condición** como "mañana".</span><span class="sxs-lookup"><span data-stu-id="ead9d-140">For the "Good morning" response, specify it's **Condition name** as "morning".</span></span> <span data-ttu-id="ead9d-141">Para la respuesta "Buenas tardes", especifique su **nombre de condición** como "tarde".</span><span class="sxs-lookup"><span data-stu-id="ead9d-141">Likewise, for the "Good evening" response, specify its **Condition name** as "evening".</span></span> <span data-ttu-id="ead9d-142">El script personalizado que escriba le devolverá una de estas condiciones.</span><span class="sxs-lookup"><span data-stu-id="ead9d-142">The custom script you will write will return either one of these condition.</span></span>
6. <span data-ttu-id="ead9d-143">En el campo **Code to execute on run** (Código que se ejecutará en ejecución), escriba un nombre para la función de devolución de llamada (p. ej.: `fnResolveTimeOfDayTemplate`).</span><span class="sxs-lookup"><span data-stu-id="ead9d-143">In the **Code to execute on run** field, enter a callback function name (e.g.: `fnResolveTimeOfDayTemplate`).</span></span> <span data-ttu-id="ead9d-144">A continuación, haga clic en **Ver código** para cargar el editor de \*scripts\*\*.</span><span class="sxs-lookup"><span data-stu-id="ead9d-144">Then, click **View code** to load the *Scripts*\* editor.</span></span> <span data-ttu-id="ead9d-145">Una vez allí, puede definir la implementación de esta función de devolución de llamada.</span><span class="sxs-lookup"><span data-stu-id="ead9d-145">From there, you can define the implementation for this callback function.</span></span>
7. <span data-ttu-id="ead9d-146">En la esquina superior derecha, haga clic en **Guardar** para guardar los cambios y crear esta plantilla.</span><span class="sxs-lookup"><span data-stu-id="ead9d-146">In the upper right corner, click **Save** to save your changes and create this template.</span></span>

<span data-ttu-id="ead9d-147">Ejemplo de código de la función de devolución de llamada *fnResolveTimeOfDayTemplate*.</span><span class="sxs-lookup"><span data-stu-id="ead9d-147">Sample code for the *fnResolveTimeOfDayTemplate* callback function.</span></span> <span data-ttu-id="ead9d-148">Esta función devolverá la cadena que coincida con el **nombre de condición** "mañana" o "tarde" especificado por la respuesta.</span><span class="sxs-lookup"><span data-stu-id="ead9d-148">This function will return the string that will match either the "morning" or "evening" **Condition name** specified by the response.</span></span>

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

## <a name="send-a-response-to-user"></a><span data-ttu-id="ead9d-149">Envío de una respuesta al usuario</span><span class="sxs-lookup"><span data-stu-id="ead9d-149">Send a response to user</span></span>

<span data-ttu-id="ead9d-150">Para enviar una respuesta al usuario mediante plantillas de respuesta, agregue los nombres de plantilla entre corchetes.</span><span class="sxs-lookup"><span data-stu-id="ead9d-150">To send a user a response using response templates, enclose either the template names in square brackets.</span></span> <span data-ttu-id="ead9d-151">Por ejemplo, para usar la plantilla de respuesta simple "AckPhrase" en un estado de comentarios, en el campo **Bot's response to user** (Respuesta del bot al usuario) escriba la frase `[AckPhrase], I will get that done for you right away`</span><span class="sxs-lookup"><span data-stu-id="ead9d-151">For example, to use the "AckPhrase" simple response template in a feedback state, enter the **Bot's response to user** phrase as `[AckPhrase], I will get that done for you right away`</span></span>

<span data-ttu-id="ead9d-152">Si desea utilizar corchetes en las respuestas, use "\" como carácter de escape para indicar al entorno en tiempo de ejecución de la conversación que omita la resolución de los corchetes.</span><span class="sxs-lookup"><span data-stu-id="ead9d-152">If you want to use square brackets in your responses, use "\" as the escape character to instruct the conversation runtime to skip resolution for square brackets.</span></span>

<span data-ttu-id="ead9d-153">Si tiene entidades definidas, puede usarlas también en su respuesta al usuario.</span><span class="sxs-lookup"><span data-stu-id="ead9d-153">If you have entities defined, you can use them in your response to user as well.</span></span> <span data-ttu-id="ead9d-154">Puede hacer referencia a las entidades en la generación del lenguaje poniendo el nombre de la entidad entre llaves.</span><span class="sxs-lookup"><span data-stu-id="ead9d-154">You can refer to your entities in language generation by enclosing the entity name in curly brackets.</span></span> <span data-ttu-id="ead9d-155">Por ejemplo, si el bot tiene una entidad `location`, puede hacer referencia a ella en las respuestas de la siguiente manera: `[AckPhrase], I can help you find a table at {location}.`</span><span class="sxs-lookup"><span data-stu-id="ead9d-155">For example, if your bot has a `location` entity, you can refer to it in your responses as follows: `[AckPhrase], I can help you find a table at {location}.`</span></span>

## <a name="nesting-response-templates"></a><span data-ttu-id="ead9d-156">Anidamiento de plantillas de respuesta</span><span class="sxs-lookup"><span data-stu-id="ead9d-156">Nesting response templates</span></span>

<span data-ttu-id="ead9d-157">Las plantillas de respuesta se pueden "anidar". Una plantilla de respuesta puede contener referencias a otra plantilla de respuesta.</span><span class="sxs-lookup"><span data-stu-id="ead9d-157">Response templates can be "nested"; one response template can have references to another response template.</span></span> <span data-ttu-id="ead9d-158">Por ejemplo, podría usar la plantilla de respuesta simple `AckPhrase` y la plantilla de respuesta condicional `timeOfDayTemplate` para construir una nueva plantilla de respuesta `timeBasedGreeting` que use ambas plantillas para resolver la respuesta final.</span><span class="sxs-lookup"><span data-stu-id="ead9d-158">For example, you could use the `AckPhrase` simple response template and the `timeOfDayTemplate` conditional response template to construct a new `timeBasedGreeting` response template that uses both templates to resolve the final response.</span></span> 

> [!TIP]
> <span data-ttu-id="ead9d-159">Aunque el anidamiento de plantillas es una característica eficaz, asegúrese de que las plantillas anidadas no causan un bucle infinito.</span><span class="sxs-lookup"><span data-stu-id="ead9d-159">While template nesting is a powerful feature, be sure to check that your nested templates do not cause an infinite loop.</span></span> <span data-ttu-id="ead9d-160">Es decir, evite situaciones en las que hace que `AckPhrase` llame a `timeOfDayTemplate` y `timeOfDayTemplate` devuelva la llamada a `AckPhrase`.</span><span class="sxs-lookup"><span data-stu-id="ead9d-160">That is, avoid situations where you have `AckPhrase` calling `timeOfDayTemplate` and `timeOfDayTemplate` calling back to `AckPhrase`.</span></span>

<span data-ttu-id="ead9d-161">Por ejemplo, cree un nuevo nombre de **respuesta simple** `timeBasedGreeting` y escriba el texto siguiente como posible **respuesta del bot a un usuario** para esta plantilla: `[timeOfDayTemplate] [AckPhrase], ... `</span><span class="sxs-lookup"><span data-stu-id="ead9d-161">For example, create a new **Simple response** name `timeBasedGreeting` and enter the following text as a possible **Bot's response to user** for this template: `[timeOfDayTemplate] [AckPhrase], ... `</span></span>

## <a name="define-user-utterance-as-help-tips"></a><span data-ttu-id="ead9d-162">Definición de expresiones del usuario como sugerencias de ayuda</span><span class="sxs-lookup"><span data-stu-id="ead9d-162">Define user utterance as help tips</span></span>

<span data-ttu-id="ead9d-163">Al definir una **expresión** del usuario, puede establecer también una expresión como **sugerencia de ayuda**.</span><span class="sxs-lookup"><span data-stu-id="ead9d-163">While defining user **Utterance**, you an also set an utterance as a **Help Tip**.</span></span> <span data-ttu-id="ead9d-164">Para establecer una expresión como **sugerencia de ayuda**, haga clic en `...` a la derecha de la expresión y seleccione **Use as help tip** (Usar como sugerencia de ayuda).</span><span class="sxs-lookup"><span data-stu-id="ead9d-164">To set an utterance as a **Help Tip**, click the `...` to the right of the utterance and select **Use as help tip**.</span></span> 

<span data-ttu-id="ead9d-165">La sugerencia de ayuda se usa como la plantilla integrada de generación de lenguaje `[builtin-tasktips]` en la que puede definir un campo **Bot's response to user** (Respuesta del bot al usuario).</span><span class="sxs-lookup"><span data-stu-id="ead9d-165">The help tip is used in the builtin language generation template `[builtin-tasktips]` anywhere you define a **Bot's response to user** field.</span></span> <span data-ttu-id="ead9d-166">Por ejemplo, podría crear una respuesta que diga: `Sorry I did not understand that. Here are some things you can try - [builtin.tasktips]`</span><span class="sxs-lookup"><span data-stu-id="ead9d-166">For example, you could compose a response that says: `Sorry I did not understand that. Here are some things you can try - [builtin.tasktips]`</span></span>

<span data-ttu-id="ead9d-167">Si una tarea no tiene ninguna **sugerencia de ayuda** definida, `[builtin-tasktips]` se resolverá con una cadena vacía.</span><span class="sxs-lookup"><span data-stu-id="ead9d-167">If a task has no **Help Tip** defined, then the `[builtin-tasktips]` will resolve to an empty string.</span></span> <span data-ttu-id="ead9d-168">Si una tarea tiene varias **sugerencias de ayuda** definidas, `[builtin-tasktips]` seleccionará una de forma aleatoria cada vez que se proporcione una respuesta.</span><span class="sxs-lookup"><span data-stu-id="ead9d-168">If a task has multiple **Help Tip** defined, then the `[builtin-tasktips]` will select one at random each time a response is given.</span></span>

## <a name="next-step"></a><span data-ttu-id="ead9d-169">Paso siguiente</span><span class="sxs-lookup"><span data-stu-id="ead9d-169">Next step</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="ead9d-170">Tarjetas adaptables</span><span class="sxs-lookup"><span data-stu-id="ead9d-170">Adaptive cards</span></span>](conversation-designer-adaptive-cards.md)