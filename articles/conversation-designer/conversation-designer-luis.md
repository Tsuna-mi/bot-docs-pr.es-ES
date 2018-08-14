---
title: Definición de un reconocedor de LUIS como desencadenador de tareas | Microsoft Docs
description: Obtenga información sobre cómo configurar el reconocedor de comprensión de lenguaje como desencadenador de tareas mediante LUIS.ai
author: vkannan
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ROBOTS: NoIndex, NoFollow
ms.openlocfilehash: 39fe222fb1d54346b33617c425b1fdf2d56daa0d
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39306248"
---
# <a name="define-a-luis-recognizer-as-task-trigger"></a><span data-ttu-id="0cf73-103">Definición de un reconocedor de LUIS como desencadenador de tareas</span><span class="sxs-lookup"><span data-stu-id="0cf73-103">Define a LUIS recognizer as task trigger</span></span>
> [!IMPORTANT]
> <span data-ttu-id="0cf73-104">Conversation Designer no está disponible para todos los clientes todavía.</span><span class="sxs-lookup"><span data-stu-id="0cf73-104">Conversation Designer is not available to all customers yet.</span></span> <span data-ttu-id="0cf73-105">Habrá más información sobre la disponibilidad de Conversation Designer más adelante este año.</span><span class="sxs-lookup"><span data-stu-id="0cf73-105">More details on availability of Conversation Designer will come later this year.</span></span>

<span data-ttu-id="0cf73-106">A menudo, el usuario expresa su intención de realizar una tarea en lenguaje natural.</span><span class="sxs-lookup"><span data-stu-id="0cf73-106">Most often, user express their intent to perform a task in natural language.</span></span> <span data-ttu-id="0cf73-107">Con Conversation Designer, puede configurar fácilmente un modelo de comprensión de lenguaje natural para el bot con la tecnología de <a href="https://luis.ai" target="_blank">LUIS.ai</a>.</span><span class="sxs-lookup"><span data-stu-id="0cf73-107">With Conversation Designer, you can easily set up a natural language understanding model for your bot powered by <a href="https://luis.ai" target="_blank">LUIS.ai</a>.</span></span>

<span data-ttu-id="0cf73-108">Para ello, seleccione el tipo de desencadenador **User says or types something** (El usuario dice o escribe algo).</span><span class="sxs-lookup"><span data-stu-id="0cf73-108">To do this, select the trigger type **User says or types something**.</span></span> <span data-ttu-id="0cf73-109">Esto le dará la opción de especificar el nombre de la **intención**.</span><span class="sxs-lookup"><span data-stu-id="0cf73-109">This will provide you with the option to specify the **intent** name.</span></span> 

<span data-ttu-id="0cf73-110">Puede buscar las intenciones existentes o crear una en el campo **Which language intent?** (¿Qué intención de lenguaje?).</span><span class="sxs-lookup"><span data-stu-id="0cf73-110">You can search for existing intents or create a new one in the **Which language intent?** field.</span></span>

## <a name="create-a-new-intent"></a><span data-ttu-id="0cf73-111">Creación de una nueva intención</span><span class="sxs-lookup"><span data-stu-id="0cf73-111">Create a new intent</span></span>

<span data-ttu-id="0cf73-112">Para crear una intención, escriba el nombre de la intención y haga clic en **Create new intent** (Crear intención).</span><span class="sxs-lookup"><span data-stu-id="0cf73-112">To create a new intent, type in the name for the intent and click **Create new intent**.</span></span> <span data-ttu-id="0cf73-113">Escriba las expresiones de ejemplo para las posibles cosas que espera que los usuarios digan que deberían desencadenar esta tarea específica.</span><span class="sxs-lookup"><span data-stu-id="0cf73-113">Enter example utterances for the possible things you expect your users to say that should trigger this specific task.</span></span>

<span data-ttu-id="0cf73-114">Por ejemplo, un bot de café debe ser capaz de realizar la tarea de encontrar las cafeterías cerca del usuario.</span><span class="sxs-lookup"><span data-stu-id="0cf73-114">For example, a cafe bot should be able to perform the task of finding cafe locations near the user.</span></span> <span data-ttu-id="0cf73-115">Para controlar este escenario, seleccione **User says or types something** y establezca el **Intent name** (Nombre de intención) en "LocationNearMe".</span><span class="sxs-lookup"><span data-stu-id="0cf73-115">To handle this scenario, select **User says or types something** and set the **Intent name** to "LocationNearMe".</span></span> <span data-ttu-id="0cf73-116">A continuación, proporcione expresiones de ejemplo para intención.</span><span class="sxs-lookup"><span data-stu-id="0cf73-116">Then, provide example utterances for this intent.</span></span> <span data-ttu-id="0cf73-117">Por ejemplo: </span><span class="sxs-lookup"><span data-stu-id="0cf73-117">For example:</span></span> 
- <span data-ttu-id="0cf73-118">*buscar ubicaciones a mi alrededor*</span><span class="sxs-lookup"><span data-stu-id="0cf73-118">*find locations near me*</span></span>
- <span data-ttu-id="0cf73-119">*buscar cafeterías a mi alrededor*</span><span class="sxs-lookup"><span data-stu-id="0cf73-119">*find cafe locations near me*</span></span>
- <span data-ttu-id="0cf73-120">*¿está abierta la tienda Redmond ahora?*</span><span class="sxs-lookup"><span data-stu-id="0cf73-120">*is the Redmond store open now?*</span></span>
- <span data-ttu-id="0cf73-121">*¿hay una tienda en Seattle?*</span><span class="sxs-lookup"><span data-stu-id="0cf73-121">*is there a store in Seattle?*</span></span>
- <span data-ttu-id="0cf73-122">*¿qué cafeterías hay abiertas ahora?*</span><span class="sxs-lookup"><span data-stu-id="0cf73-122">*what cafe locations are open now?*</span></span>
- <span data-ttu-id="0cf73-123">*¿dónde puedo comer algo?*</span><span class="sxs-lookup"><span data-stu-id="0cf73-123">*where can I get something to eat?*</span></span>
- <span data-ttu-id="0cf73-124">*Me gustaría comer algo*</span><span class="sxs-lookup"><span data-stu-id="0cf73-124">*I would like to get something to eat*</span></span>
- <span data-ttu-id="0cf73-125">*Tengo hambre*</span><span class="sxs-lookup"><span data-stu-id="0cf73-125">*I'm hungry*</span></span>

<span data-ttu-id="0cf73-126">Escriba tantas expresiones como se le ocurran que ayuden a transmitir la intención del usuario para desencadenar esta tarea específica.</span><span class="sxs-lookup"><span data-stu-id="0cf73-126">Enter as many utterances as you can possibly think of that help express the user's intent to trigger this specific task.</span></span>

## <a name="default-intents-provisioned-for-your-bot"></a><span data-ttu-id="0cf73-127">Intenciones predeterminadas aprovisionadas para el bot</span><span class="sxs-lookup"><span data-stu-id="0cf73-127">Default intents provisioned for your bot</span></span>

<span data-ttu-id="0cf73-128">De manera predeterminada, el bot se aprovisiona con 4 intenciones.</span><span class="sxs-lookup"><span data-stu-id="0cf73-128">By default, your bot is provisioned with 4 intents.</span></span> 
- <span data-ttu-id="0cf73-129">**None** (Ninguna): se trata de la intención de reserva (predeterminada) para el bot.</span><span class="sxs-lookup"><span data-stu-id="0cf73-129">**None**: This is the fallback (default) intent for your bot.</span></span> <span data-ttu-id="0cf73-130">Utilice esta intención para ayudar a capturar cosas a las que el bot no sabe cómo responder todavía.</span><span class="sxs-lookup"><span data-stu-id="0cf73-130">Use this intent to help capture things that your bot does not yet know how to respond to.</span></span>
- <span data-ttu-id="0cf73-131">**Help** (Ayuda): configure expresiones de ejemplo que ayuden a decidir si el usuario necesita ayuda.</span><span class="sxs-lookup"><span data-stu-id="0cf73-131">**Help**: Set up with example utterances that help determine if user needs help.</span></span> <span data-ttu-id="0cf73-132">*Ayuda, necesito ayuda, ¿qué puedo decir?, estoy atascado* y así sucesivamente.</span><span class="sxs-lookup"><span data-stu-id="0cf73-132">*Help, I need help, what can I say?, I'm stuck* and so on.</span></span>
- <span data-ttu-id="0cf73-133">**Greeting** (Saludo): configure expresiones de ejemplo que ayuden a asocial la intención de saludo, como *Hola, Qué tal, Buenos días, ¿Cómo estás, bot?* y así sucesivamente.</span><span class="sxs-lookup"><span data-stu-id="0cf73-133">**Greeting**: Set up with example utterances that help match Greeting intent such as *Hi, Hello, Good morning, How are you bot* and so on.</span></span>
- <span data-ttu-id="0cf73-134">**Cancel** (Cancelar): configure expresiones de ejemplo para la intención de cancelar.</span><span class="sxs-lookup"><span data-stu-id="0cf73-134">**Cancel**: Set up with example utterances for cancel intent.</span></span> <span data-ttu-id="0cf73-135">*Detener, Cancelar esto, no lo hagas, revertir* y así sucesivamente.</span><span class="sxs-lookup"><span data-stu-id="0cf73-135">*Stop, Cancel that, do not do it, revert* and so on.</span></span>

## <a name="create-and-label-entities"></a><span data-ttu-id="0cf73-136">Creación y etiquetado de entidades</span><span class="sxs-lookup"><span data-stu-id="0cf73-136">Create and label entities</span></span>

<span data-ttu-id="0cf73-137">Además de ayudarle a determinar la intención del usuario, la comprensión de lenguaje puede ayudarle a determinar las entidades específicas de interés correspondientes a la tarea.</span><span class="sxs-lookup"><span data-stu-id="0cf73-137">Apart from helping determine user intent, language understanding can also help you determine specific entities of interest relevant to the task.</span></span> <span data-ttu-id="0cf73-138">Por ejemplo, cuando el usuario dice *buscar cafeterías cerca de Redmond*, puede que quiera extraer *Redmond* como valor posible para la entidad ***location***.</span><span class="sxs-lookup"><span data-stu-id="0cf73-138">As an example, when user says *find cafe locations near Redmond*, you might want to extract *Redmond* as a possible value for entity ***location***.</span></span> 

<span data-ttu-id="0cf73-139">Para configurar las entidades de la tarea, desde la cadena **Utterance**, seleccione la parte de la expresión que debe ser un ejemplo de un valor de la entidad.</span><span class="sxs-lookup"><span data-stu-id="0cf73-139">To set up entities for the task, from the **Utterance** string, select the part of the utterance that should be an example for an entity value.</span></span> <span data-ttu-id="0cf73-140">Asigne esto a una entidad existente o cree una.</span><span class="sxs-lookup"><span data-stu-id="0cf73-140">Assign this to an existing entity or create a new one.</span></span> <span data-ttu-id="0cf73-141">Para crear una entidad, escriba el nombre de la entidad en el campo **Search or create** (Buscar o crear) y haga clic en **Crear nueva entidad**.</span><span class="sxs-lookup"><span data-stu-id="0cf73-141">To create a new entity, type the name of the entity into the **Search or create** field and click **Create new entity**.</span></span> 

# <a name="supported-entity-types"></a><span data-ttu-id="0cf73-142">Tipos de entidad admitidos</span><span class="sxs-lookup"><span data-stu-id="0cf73-142">Supported entity types</span></span>

<span data-ttu-id="0cf73-143">La comprensión de lenguaje da la facultad de crear diferentes tipos de entidades.</span><span class="sxs-lookup"><span data-stu-id="0cf73-143">Language understanding provides you the power to create different types of entities.</span></span> <span data-ttu-id="0cf73-144">Al crear una entidad, es necesario especificar un `type`.</span><span class="sxs-lookup"><span data-stu-id="0cf73-144">When you create a new entity, you must specify a `type`.</span></span> 

<span data-ttu-id="0cf73-145">Los tipos disponibles son:</span><span class="sxs-lookup"><span data-stu-id="0cf73-145">Available types are :</span></span>

- <span data-ttu-id="0cf73-146">**Simple**: este es el tipo *predeterminado*.</span><span class="sxs-lookup"><span data-stu-id="0cf73-146">**Simple**: This is the *default* type.</span></span>
- <span data-ttu-id="0cf73-147">**List**: use este tipo si la entidad tiene un conjunto finito de valores posibles.</span><span class="sxs-lookup"><span data-stu-id="0cf73-147">**List**: Use this type if your entity has a finite set of possible values.</span></span> <span data-ttu-id="0cf73-148">Ejemplo: *Color*, *City*.</span><span class="sxs-lookup"><span data-stu-id="0cf73-148">Example: *Color*, *City*.</span></span>
- <span data-ttu-id="0cf73-149">**Hierarchical**: use este tipo para crear entidades con la relación de elementos primarios y secundarios.</span><span class="sxs-lookup"><span data-stu-id="0cf73-149">**Hierarchical**: Use this type to create entities with parent-child relationship.</span></span> <span data-ttu-id="0cf73-150">Ejemplo: *fromCity* y *toCity* tienen la entidad *City* como elemento primario.</span><span class="sxs-lookup"><span data-stu-id="0cf73-150">Example: *fromCity* and *toCity* both have *City* entity as parent</span></span>
- <span data-ttu-id="0cf73-151">**Composite**: use este tipo para crear grupos de valores que conformen una unidad significativa.</span><span class="sxs-lookup"><span data-stu-id="0cf73-151">**Composite**: Use this type to create groups of values that make up a meaningful unit.</span></span> <span data-ttu-id="0cf73-152">Ejemplo: *City* y *State* conforman la entidad *Location*.</span><span class="sxs-lookup"><span data-stu-id="0cf73-152">Example: *City* and *State* together make up the *Location* entity.</span></span>

<!-- # pre-built entity types TBD -->

# <a name="entities-in-use"></a><span data-ttu-id="0cf73-153">Entidades en uso</span><span class="sxs-lookup"><span data-stu-id="0cf73-153">Entities in use</span></span>

<span data-ttu-id="0cf73-154">A medida que crea y agrega entidades a la sección de comprensión de lenguaje, la tabla **Entities in use** (Entidades en uso) se actualiza con la lista de entidades que usa esta tarea específica.</span><span class="sxs-lookup"><span data-stu-id="0cf73-154">As you create and add entities to the language understanding section, the **Entities in use** table updates with the list of entities this specific task uses.</span></span> <span data-ttu-id="0cf73-155">Puede agregar manualmente a la lista otras entidades que se usan en esta tarea.</span><span class="sxs-lookup"><span data-stu-id="0cf73-155">You can manually add other entities used in this task to the list.</span></span> 

<span data-ttu-id="0cf73-156">Las opciones disponibles son la siguientes:</span><span class="sxs-lookup"><span data-stu-id="0cf73-156">Available options are:</span></span>

- <span data-ttu-id="0cf73-157">**Code**: se trata de una entidad que se crea en el script personalizado.</span><span class="sxs-lookup"><span data-stu-id="0cf73-157">**Code**: This is an entity that is created in your custom script.</span></span> <span data-ttu-id="0cf73-158">Puede especificarla aquí para ayudar con la creación de características como intellisense.</span><span class="sxs-lookup"><span data-stu-id="0cf73-158">You can specify it here to help with authoring features like intellisense.</span></span>

<!-- # Use as help tip TBD  -->

## <a name="next-step"></a><span data-ttu-id="0cf73-159">Paso siguiente</span><span class="sxs-lookup"><span data-stu-id="0cf73-159">Next step</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="0cf73-160">Reconocedor de código</span><span class="sxs-lookup"><span data-stu-id="0cf73-160">Code recognizer</span></span>](conversation-designer-code-recognizer.md)
