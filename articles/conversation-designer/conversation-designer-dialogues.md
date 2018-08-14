---
title: Definir un cuadro de diálogo como una acción Do | Microsoft Docs
description: Obtenga información sobre cómo configurar el cuadro de diálogo como una acción Do.
author: vkannan
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ROBOTS: NoIndex, NoFollow
ms.openlocfilehash: d59df20821a7f63eb9ee5dea365597b1af839f75
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39304945"
---
# <a name="define-a-dialogue-as-a-do-action"></a><span data-ttu-id="0b651-103">Definir un cuadro de diálogo como una acción Do</span><span class="sxs-lookup"><span data-stu-id="0b651-103">Define a dialogue as a Do action</span></span>
> [!IMPORTANT]
> <span data-ttu-id="0b651-104">Conversation Designer no está disponible para todos los clientes todavía.</span><span class="sxs-lookup"><span data-stu-id="0b651-104">Conversation Designer is not available to all customers yet.</span></span> <span data-ttu-id="0b651-105">Habrá más información sobre la disponibilidad de Conversation Designer más adelante este año.</span><span class="sxs-lookup"><span data-stu-id="0b651-105">More details on availability of Conversation Designer will come later this year.</span></span>

<span data-ttu-id="0b651-106">Un cuadro de diálogo trata sobre el modelo de conversación de una tarea específica.</span><span class="sxs-lookup"><span data-stu-id="0b651-106">A dialogue covers the conversational model for a specific task.</span></span> <span data-ttu-id="0b651-107">Por ejemplo, un bot de cafetería que ayuda a los usuarios a reservar una mesa, podría definir una tarea llamada "Reservar mesa".</span><span class="sxs-lookup"><span data-stu-id="0b651-107">For example, a cafe bot that helps users book a table could define a task called "Book Table".</span></span> <span data-ttu-id="0b651-108">La acción **Do** está ligada a un diálogo que de forma al flujo de la conversación entre el bot y el usuario.</span><span class="sxs-lookup"><span data-stu-id="0b651-108">The **Do** action would be bound to a dialogue that models the conversational flow between the bot and user.</span></span> 

<span data-ttu-id="0b651-109">Los diálogos son particularmente útiles cuando un bot inicia una conversación con el usuario para ayudarle a completar una tarea.</span><span class="sxs-lookup"><span data-stu-id="0b651-109">Dialogues are particularly useful when a bot engages in a back and forth conversation with the user to help complete a task.</span></span>  <span data-ttu-id="0b651-110">Los usuarios rara vez expresan todos los valores requeridos para completar una tarea en un solo enunciado.</span><span class="sxs-lookup"><span data-stu-id="0b651-110">Users rarely express all required values to complete a task in a single utterance.</span></span> 

<span data-ttu-id="0b651-111">Por ejemplo, para reservar una mesa es necesaria la ubicación, el tamaño del grupo, la fecha y la preferencia horaria.</span><span class="sxs-lookup"><span data-stu-id="0b651-111">Booking a table requires the location, party size, date, and time preference.</span></span> <span data-ttu-id="0b651-112">Un bot que reserve mesas necesita entender y manejar las posibles frases que el usuario podría decir.</span><span class="sxs-lookup"><span data-stu-id="0b651-112">A bot that books tables needs to understand and handle various possible phrases the user could say.</span></span> 

- <span data-ttu-id="0b651-113">*reservar una mesa*: en este caso, ninguna de las entidades requeridas fue capturada.</span><span class="sxs-lookup"><span data-stu-id="0b651-113">*book a table*: In this case, none of the required entities were captured.</span></span> <span data-ttu-id="0b651-114">El bot debe entablar una conversación con el usuario.</span><span class="sxs-lookup"><span data-stu-id="0b651-114">The bot must now engage in a conversation with the user.</span></span>
- <span data-ttu-id="0b651-115">*reservar una mesa para las 7 de la tarde este sábado* : en este caso, se especifica la preferencia de fecha y hora, pero el bot aún debe recopilar la ubicación y cuántas personas irán.</span><span class="sxs-lookup"><span data-stu-id="0b651-115">*book a table for 7PM this Saturday*: In this case, date and time preference is specified but the bot must still gather the intended location and party size.</span></span>

<span data-ttu-id="0b651-116">Durante una conversación, algunas entidades pueden cambiar en función de la entrada del usuario.</span><span class="sxs-lookup"><span data-stu-id="0b651-116">During a conversation, some entities may change based on user input.</span></span> <span data-ttu-id="0b651-117">Por ejemplo, si el usuario dice "Reserva una mesa en Redmond para este domingo a las 7 de la tarde".</span><span class="sxs-lookup"><span data-stu-id="0b651-117">For example, if the user says "Book a table in Redmond for this Sunday at 7 pm."</span></span> <span data-ttu-id="0b651-118">pero el restaurante Redmond cierra a las seis los domingos, el diálogo del bot debe ser capaz de manejar correctamente las solicitudes que no sean válidas.</span><span class="sxs-lookup"><span data-stu-id="0b651-118">but the Redmond location closes at six on Sundays, the bot's dialogue should handle such invalid requests.</span></span> 

<span data-ttu-id="0b651-119">Conversation Designer proporciona un diseñador de diálogos que permite arrastrar y soltar las opciones, para ayudarle a visualizar el flujo de conversación.</span><span class="sxs-lookup"><span data-stu-id="0b651-119">Conversation Designer provides a drag-and-drop dialogue designer to help you visualize your conversation flow.</span></span> <span data-ttu-id="0b651-120">El diseñador de diálogos ofrece siete *estados de diálogo* que puede usar para dar forma al flujo de la conversación.</span><span class="sxs-lookup"><span data-stu-id="0b651-120">The dialogue designer offers seven *dialogue states* you can use to model your conversation flow.</span></span>

## <a name="dialogue-states"></a><span data-ttu-id="0b651-121">Estados de diálogo</span><span class="sxs-lookup"><span data-stu-id="0b651-121">Dialogue states</span></span>

<span data-ttu-id="0b651-122">Un diálogo se compone de estados conversacionales.</span><span class="sxs-lookup"><span data-stu-id="0b651-122">A dialogue is made up of conversational states.</span></span> <span data-ttu-id="0b651-123">El diálogo en sí se diseña como un flujo estructurado y directo que proporciona al tiempo de ejecución de la conversación una estructura sobre cómo ejecutar el flujo conversacional.</span><span class="sxs-lookup"><span data-stu-id="0b651-123">The dialogue itself is modeled as a structured, directed flow that provide the conversation runtime a structure for how to execute the conversational flow.</span></span>

<span data-ttu-id="0b651-124">Los diálogos cuentan con una gran variedad estados integrados que puede usar.</span><span class="sxs-lookup"><span data-stu-id="0b651-124">Dialogues come with many builtin states you can use.</span></span> <span data-ttu-id="0b651-125">Los estados integrados compatibles incluyen lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="0b651-125">Supported builtin states include the following:</span></span>

- <span data-ttu-id="0b651-126">[**Iniciar**](#start-state): representa el estado inicial de un flujo conversacional.</span><span class="sxs-lookup"><span data-stu-id="0b651-126">[**Start**](#start-state): Represents the starting state for a conversational flow.</span></span> <span data-ttu-id="0b651-127">Todos los diálogos deben tener al menos un estado de inicio definido.</span><span class="sxs-lookup"><span data-stu-id="0b651-127">All dialogues must have at least one start state defined.</span></span>
- <span data-ttu-id="0b651-128">[**Devolver**](#return-state): representa la finalización del flujo específico de la conversación.</span><span class="sxs-lookup"><span data-stu-id="0b651-128">[**Return**](#return-state): Represents completion of the specific conversational flow.</span></span> <span data-ttu-id="0b651-129">Dado que los flujos conversacionales admiten composición, la opción Devolver indica al tiempo de ejecución de la conversación que devuelva la ejecución a los posibles autores de la llamada de este diálogo.</span><span class="sxs-lookup"><span data-stu-id="0b651-129">Given that conversational flows are composable, return instructs the conversation runtime to return execution to any possible callers of this dialogue.</span></span>
- <span data-ttu-id="0b651-130">[**Decisión**](#decision-state): representa un punto de bifurcación en el flujo conversacional.</span><span class="sxs-lookup"><span data-stu-id="0b651-130">[**Decision**](#decision-state): Represents a point of branching in the conversational flow.</span></span>
- <span data-ttu-id="0b651-131">[**Proceso** ](#process-state): representa un estado en el que el bot ejecuta la lógica de negocios.</span><span class="sxs-lookup"><span data-stu-id="0b651-131">[**Process**](#process-state): Represents a state where your bot is executing business logic.</span></span>
- <span data-ttu-id="0b651-132">[**Solicitud**](#prompt-state): representa un estado en el que puede solicitar al usuario que escriba más información.</span><span class="sxs-lookup"><span data-stu-id="0b651-132">[**Prompt**](#prompt-state): Represents a state you can prompt the user for input.</span></span> 
- <span data-ttu-id="0b651-133">[**Comentarios**](#feedback-state): representa un estado en el que puede proporcionar comentarios o confirmaciones al usuario.</span><span class="sxs-lookup"><span data-stu-id="0b651-133">[**Feedback**](#feedback-state): Represent a state you can provide feedback or confirmation to the user.</span></span> <span data-ttu-id="0b651-134">Por ejemplo, un diálogo que confirme que la reserva se ha hecho.</span><span class="sxs-lookup"><span data-stu-id="0b651-134">For example, a dialogue confirming that the reservation has been made.</span></span>
- <span data-ttu-id="0b651-135">[**Módulo**](#module-state): representa una llamada a otro diálogo.</span><span class="sxs-lookup"><span data-stu-id="0b651-135">[**Module**](#module-state): Represents a call to another dialogue.</span></span> <span data-ttu-id="0b651-136">Puesto que los flujos de diálogo se pueden componer de forma predeterminada, este estado puede llamar a un diálogo compartido o a algún otro diálogo tal como se define en esta tarea.</span><span class="sxs-lookup"><span data-stu-id="0b651-136">Since dialogue flows are composable by default,  this state can call either a shared dialogue or some other dialogue as defined under this task.</span></span>

<span data-ttu-id="0b651-137">Cada estado de la conversación está conectado a otro mediante conectores de diálogo del diseñador de diálogos.</span><span class="sxs-lookup"><span data-stu-id="0b651-137">Each conversational state is connected to another using dialogue connectors in the dialogue designer.</span></span>

<span data-ttu-id="0b651-138">Cada estado de diálogo tiene un editor de estado asociado que se utiliza para especificar las propiedades de ese estado, incluidos los nombres de función de devolución de llamadas para scripts personalizados.</span><span class="sxs-lookup"><span data-stu-id="0b651-138">Every dialogue state has an associated state editor that is used to specify the properties for that state, including callback function names for custom scripts.</span></span> <span data-ttu-id="0b651-139">El **Editor de estado** está ubicado como un panel de cambio de tamaño en la parte inferior del puerto de vista principal del diseñador de diálogos.</span><span class="sxs-lookup"><span data-stu-id="0b651-139">The **State Editor** located as a resize pane at the bottom of the dialogue designer main view port.</span></span> <span data-ttu-id="0b651-140">Para abrir el editor, haga doble clic en un estado del diseñador de diálogos y un **Editor de estado** mostrará las propiedades correspondientes a ese estado.</span><span class="sxs-lookup"><span data-stu-id="0b651-140">To bring up the editor, double-click a state from the dialogue designer and a **State Editor** will display the properties for that state.</span></span>
<!-- TODO: insert screenshot of the wrench in horizontal menu -->

<span data-ttu-id="0b651-141">Las siguientes subsecciones proporcionan más detalles sobre cada uno de estos estados de diálogo integrados.</span><span class="sxs-lookup"><span data-stu-id="0b651-141">The following sub-sections provide more details about each of these builtin dialogue states.</span></span>

### <a name="start-state"></a><span data-ttu-id="0b651-142">Estado de inicio</span><span class="sxs-lookup"><span data-stu-id="0b651-142">Start state</span></span>

<span data-ttu-id="0b651-143">El estado de inicio indica el punto de inicio de un diálogo.</span><span class="sxs-lookup"><span data-stu-id="0b651-143">Start state denotes the starting point of a dialogue.</span></span> <span data-ttu-id="0b651-144">El valor requerido es el **nombre** del estado.</span><span class="sxs-lookup"><span data-stu-id="0b651-144">Required value is state **name**.</span></span> <span data-ttu-id="0b651-145">El nombre es "Inicio" de forma predeterminada, pero puede editarlo para cambiarle el nombre.</span><span class="sxs-lookup"><span data-stu-id="0b651-145">The name is defaulted to "Start" and you can edit it to rename this state.</span></span>

### <a name="return-state"></a><span data-ttu-id="0b651-146">Estado de devolución</span><span class="sxs-lookup"><span data-stu-id="0b651-146">Return state</span></span>

<span data-ttu-id="0b651-147">El estado de devolución representa la finalización de una rama del flujo de la conversación.</span><span class="sxs-lookup"><span data-stu-id="0b651-147">Return state represents completion of the specific branch of the conversational flow.</span></span> <span data-ttu-id="0b651-148">Puesto que los diálogos admiten composición, el estado también indica al tiempo de ejecución de la conversación que debe devolver la ejecución al diálogo del autor de la llamada.</span><span class="sxs-lookup"><span data-stu-id="0b651-148">Since dialogues are composable, the state also instructs the conversation runtime to return execution to the caller dialogue.</span></span> <span data-ttu-id="0b651-149">El valor requerido es el **nombre** del estado.</span><span class="sxs-lookup"><span data-stu-id="0b651-149">Required value is state **name**.</span></span> <span data-ttu-id="0b651-150">El nombre es "Devolución" de forma predeterminada, pero puede editarlo para cambiarle el nombre.</span><span class="sxs-lookup"><span data-stu-id="0b651-150">The name is defaulted to "Return" and you can edit it to rename this state.</span></span>

### <a name="decision-state"></a><span data-ttu-id="0b651-151">Estado de toma de decisiones</span><span class="sxs-lookup"><span data-stu-id="0b651-151">Decision state</span></span>

<span data-ttu-id="0b651-152">Un estado de toma de decisiones representa una bifurcación en el flujo de la conversación.</span><span class="sxs-lookup"><span data-stu-id="0b651-152">A decision state represents branching in the conversational flow.</span></span> <span data-ttu-id="0b651-153">Puede escribir un script personalizado para evaluar la rama que debe seguir.</span><span class="sxs-lookup"><span data-stu-id="0b651-153">You can write custom script to evaluate which branch to follow.</span></span> <span data-ttu-id="0b651-154">Dependiendo de la entrada del usuario y la lógica de negocios, el script devolverá uno de los valores posibles de transición.</span><span class="sxs-lookup"><span data-stu-id="0b651-154">Depending on user input and business logic, the script will return one of the possible transition values.</span></span> <span data-ttu-id="0b651-155">Cada valor de transición solicita que el tiempo de ejecución de la conversación ejecute una rama diferente del diálogo.</span><span class="sxs-lookup"><span data-stu-id="0b651-155">Each transition value prompts the conversation runtime to run a different branch of the dialogue.</span></span>

<span data-ttu-id="0b651-156">Propiedades obligatorias para el estado de toma de decisiones:</span><span class="sxs-lookup"><span data-stu-id="0b651-156">Required properties for decision state:</span></span>
- <span data-ttu-id="0b651-157">**Nombre**: nombre único para el estado de toma de decisiones.</span><span class="sxs-lookup"><span data-stu-id="0b651-157">**Name**: Unique name for the decision state.</span></span>
- <span data-ttu-id="0b651-158">**Código para ejecutar en la ejecución**: nombre de la función de devolución de llamada que implementa su lógica de negocios para determinar qué rama de la conversación tomar.</span><span class="sxs-lookup"><span data-stu-id="0b651-158">**Code to execute on run**: Name of the callback function that implements your business logic to determine which branch of the conversation to take.</span></span> 

#### <a name="example-code-for-decision-state"></a><span data-ttu-id="0b651-159">Código de ejemplo para el estado de toma de decisiones</span><span class="sxs-lookup"><span data-stu-id="0b651-159">Example code for decision state</span></span>

<span data-ttu-id="0b651-160">En la siguiente función de devolución de llamada se muestra una decisión que indica al tiempo de ejecución de la conversación qué rama debe ejecutar.</span><span class="sxs-lookup"><span data-stu-id="0b651-160">The following sample callback function returns a decision that instruct the conversation runtime which branch to execute.</span></span>

```javascript
module.exports.fnDecisionState = function(context) {
    var a = context.taskEntities['a'];
    if (a[0].value === '0') {
        return 'yes';
    }
    else if (a[0].value === '1') {
        return 'no';
    }
}
```

### <a name="process-state"></a><span data-ttu-id="0b651-161">Estado de proceso</span><span class="sxs-lookup"><span data-stu-id="0b651-161">Process state</span></span>

<span data-ttu-id="0b651-162">El estado de proceso representa un punto en el diálogo donde el bot está avanzando en la conversación, o intentando realizar la acción correspondiente para terminar la tarea final.</span><span class="sxs-lookup"><span data-stu-id="0b651-162">Process state represents a point in the dialogue where the bot is either driving the conversation forward or attempting to perform the final task completion action.</span></span> 

<span data-ttu-id="0b651-163">Propiedades obligatorias para el estado de proceso:</span><span class="sxs-lookup"><span data-stu-id="0b651-163">Required properties for process state:</span></span>
- <span data-ttu-id="0b651-164">**Nombre**: nombre único para el estado de proceso.</span><span class="sxs-lookup"><span data-stu-id="0b651-164">**Name**: Unique name for the process state</span></span>
- <span data-ttu-id="0b651-165">**Código que se ejecutará en la ejecución**: nombre de la función de devolución de llamada que implementa la lógica de negocios.</span><span class="sxs-lookup"><span data-stu-id="0b651-165">**Code to execute on run**: Name of the callback function that implements your business logic.</span></span>

#### <a name="example-code-for-process-state"></a><span data-ttu-id="0b651-166">Código de ejemplo del estado de proceso</span><span class="sxs-lookup"><span data-stu-id="0b651-166">Example code for process state</span></span>

<span data-ttu-id="0b651-167">La siguiente función de devolución de llamada muestra el clima y devuelve la información meteorológica al usuario.</span><span class="sxs-lookup"><span data-stu-id="0b651-167">The following sample callback function gets the weather and returns the weather information to the user.</span></span>

```javascript
module.exports.fnGetWeather = function(context) {
    var options =  {
        host: 'mock',
        path: '/get?a=b',
        method: 'get'
    };
    return http.request(options).then(function(response) {
        context.contextEntities['x'].value= response.statusCode;
        var jsonBody = JSON.parse(response.body);
          // understand response
        if (response.statusCode != "200") {
            // error
        }
    });
}
```

### <a name="prompt-state"></a><span data-ttu-id="0b651-168">Estado de solicitud</span><span class="sxs-lookup"><span data-stu-id="0b651-168">Prompt state</span></span>

<span data-ttu-id="0b651-169">El estado de solicitud le pide al usuario una información específica.</span><span class="sxs-lookup"><span data-stu-id="0b651-169">Prompt state asks the user for a specific piece of information.</span></span> <span data-ttu-id="0b651-170">Los estados de solicitud incorporan un sistema de subdiálogo y, debido a ello, se consideran estados complejos.</span><span class="sxs-lookup"><span data-stu-id="0b651-170">Prompt states embody a sub-dialogue system within them and so by definition are complex states.</span></span> 

<span data-ttu-id="0b651-171">Al usar un estado de solicitud, puede definir la respuesta real que le proporcionará al usuario e incluir opcionalmente una tarjeta adaptable.</span><span class="sxs-lookup"><span data-stu-id="0b651-171">Within a prompt state, you can define the actual response to provide to the user and optionally include an adaptive card.</span></span> <span data-ttu-id="0b651-172">A continuación, puede especificar un desencadenador para analizar y comprender la respuesta del usuario.</span><span class="sxs-lookup"><span data-stu-id="0b651-172">You can then specify a trigger to parse and understand the user's response.</span></span> <span data-ttu-id="0b651-173">Este desencadenador puede ser LUIS o un reconocedor de código personalizado que utilice expresiones regulares.</span><span class="sxs-lookup"><span data-stu-id="0b651-173">This trigger can be either LUIS or a custom code recognizer using regex.</span></span>  

<span data-ttu-id="0b651-174">Si el usuario proporciona una entrada no válida, el bot puede volver a solicitar al usuario la misma información.</span><span class="sxs-lookup"><span data-stu-id="0b651-174">If the user provide an invalid input, the bot can re-prompt the user for the same information.</span></span> <span data-ttu-id="0b651-175">Este comportamiento también se puede definir en el editor de estado de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="0b651-175">This behavior can also be defined in the prompt state editor.</span></span> 

#### <a name="prompting-the-user"></a><span data-ttu-id="0b651-176">Preguntar al usuario</span><span class="sxs-lookup"><span data-stu-id="0b651-176">Prompting the user</span></span>

<span data-ttu-id="0b651-177">La respuesta a las solicitudes le permitirá especificar el mensaje que se usará cuando **solicite al usuario** más información.</span><span class="sxs-lookup"><span data-stu-id="0b651-177">Prompt response allows you to specify the message to use when **Prompting the user** for input.</span></span> <span data-ttu-id="0b651-178">Por ejemplo, para recopilar la fecha y la hora, las posibles preguntas que verá el usuario pueden ser "¿Cuándo le gustaría venir?"</span><span class="sxs-lookup"><span data-stu-id="0b651-178">For example, to gather the date and time, possible responses might be "When would you like to come in?"</span></span> <span data-ttu-id="0b651-179">o "¿Cuándo le gustaría cenar con nosotros?"</span><span class="sxs-lookup"><span data-stu-id="0b651-179">or "When would you like to dine with us?"</span></span>

#### <a name="prompt-listening-for-user-input"></a><span data-ttu-id="0b651-180">Escucha de solicitudes de las entradas del usuario</span><span class="sxs-lookup"><span data-stu-id="0b651-180">Prompt listening for user input</span></span>

<span data-ttu-id="0b651-181">Después de pedirle al usuario que responda, el tiempo de ejecución de la conversación escuchará automáticamente la información del usuario y tratará de comprender lo que dijo.</span><span class="sxs-lookup"><span data-stu-id="0b651-181">After the user is prompted to respond, the conversation runtime will automatically listen for user input and try to understand what was said.</span></span> <span data-ttu-id="0b651-182">Configure un desencadenador basado en LUIS o un reconocedor de código personalizado para intentar comprender la entrada e intención del usuario.</span><span class="sxs-lookup"><span data-stu-id="0b651-182">Configure a trigger based on LUIS or custom code recognizer to try and understand user input and intent.</span></span> <span data-ttu-id="0b651-183">Esto es similar al desencadenador de la tarea.</span><span class="sxs-lookup"><span data-stu-id="0b651-183">This is similar to the task trigger.</span></span>

#### <a name="re-prompting-the-user"></a><span data-ttu-id="0b651-184">Volver a preguntar al usuario</span><span class="sxs-lookup"><span data-stu-id="0b651-184">Re-prompting the user</span></span>

<span data-ttu-id="0b651-185">Use la sección que especifica cómo volver a preguntar al usuario, para especificar una respuesta para cada intento.</span><span class="sxs-lookup"><span data-stu-id="0b651-185">Use the re-prompt section to specify a response for each attempt.</span></span> <span data-ttu-id="0b651-186">Cada fila en la sección para volver a preguntar al usuario corresponde a la cadena de repetición de preguntas utilizada para ese turno específico.</span><span class="sxs-lookup"><span data-stu-id="0b651-186">Each row in the re-prompt section corresponds to the re-prompt string used for that specific turn.</span></span> <span data-ttu-id="0b651-187">La primera respuesta se usará para la primera pregunta repetida, la segunda para la segunda, y así sucesivamente.</span><span class="sxs-lookup"><span data-stu-id="0b651-187">The first response will be used for the first re-prompt, the second for the second, and so on.</span></span> <span data-ttu-id="0b651-188">Por ejemplo: </span><span class="sxs-lookup"><span data-stu-id="0b651-188">For example:</span></span>

<span data-ttu-id="0b651-189">*Lo siento, no le entendí. ¿Cuándo quiere venir? *
* Lo siento, pero me está costando entenderle. Probemos de nuevo. ¿Cuándo quiere venir?*</span><span class="sxs-lookup"><span data-stu-id="0b651-189">*I'm sorry, I did not understand, when did you want to come in?*
*My bad, I'm having a hard time understanding you. Let's try this again - when did you want to come in?*</span></span>

#### <a name="prompt-callback-functions"></a><span data-ttu-id="0b651-190">Funciones de devolución de llamada de solicitudes</span><span class="sxs-lookup"><span data-stu-id="0b651-190">Prompt callback functions</span></span>

<span data-ttu-id="0b651-191">Puede especificar dos funciones de devolución de llamada diferentes en un estado de solicitud.</span><span class="sxs-lookup"><span data-stu-id="0b651-191">You can specify two different callback functions on a prompt state.</span></span> 

1. <span data-ttu-id="0b651-192">**Antes de cada solicitud y repetición de solicitud**: ejecute esta función antes de cada solicitud o repetición de solicitud.</span><span class="sxs-lookup"><span data-stu-id="0b651-192">**Before every prompt and reprompt**: Execute this function before every prompt or rerompt.</span></span> <span data-ttu-id="0b651-193">Esta función de devolución de llamada espera un valor devuelto booleano donde "true" significa ejecutar esta solicitud o repetirla, y "false" significa que no se va a ejecutar esta solicitud o repetición de solicitud.</span><span class="sxs-lookup"><span data-stu-id="0b651-193">This callback function expects a boolean return value where true means execute this prompt or reprompt and false means do not execute this prompt or reprompt.</span></span> <span data-ttu-id="0b651-194">Puede usar `getTurnIndex()` para obtener el índice de turno actual para ejecutar esa solicitud.</span><span class="sxs-lookup"><span data-stu-id="0b651-194">You can use `getTurnIndex()` to get the current turn index for that prompt execution.</span></span>
2. <span data-ttu-id="0b651-195">**Al responder**: ejecute esta función cada vez que se genere una solicitud, pero antes de que se haya enviado al usuario (incluyendo la repetición de la solicitud).</span><span class="sxs-lookup"><span data-stu-id="0b651-195">**On responding**: Execute this function every time a prompt has been generated but before it has been sent to the user (including reprompt response).</span></span> <span data-ttu-id="0b651-196">Esto permite que el script modifique el mensaje que se envía al usuario.</span><span class="sxs-lookup"><span data-stu-id="0b651-196">This enables the script to modify the message sent to the user.</span></span>

#### <a name="sample-code"></a><span data-ttu-id="0b651-197">Código de ejemplo</span><span class="sxs-lookup"><span data-stu-id="0b651-197">Sample code</span></span>

<span data-ttu-id="0b651-198">Este fragmento de código muestra un ejemplo para la devolución de llamada denominada **Antes de ejecutar**.</span><span class="sxs-lookup"><span data-stu-id="0b651-198">This code snippet shows an example for **before executing** callback.</span></span>

```javascript
module.exports.fnBeforeExecuting = function(context) {
    if(context.responses[0].text === "C") {
        return false;
    }
    return true;
}
```

<span data-ttu-id="0b651-199">Este fragmento de código muestra un ejemplo para la devolución de llamada denominada **Al solicitar**.</span><span class="sxs-lookup"><span data-stu-id="0b651-199">This code snippet shows an example for **On prompting** callback.</span></span>

```javascript
module.exports.fnOnPrompting = function(context) {
    // include a hint card
    var activity = context.responses.slice(-1).pop();
    activity.attachments.push({
        "contentType": "application/vnd.microsoft.card.hero",
        "content": {
            "buttons": [
                {
                    "type": "imBack",
                    "title": "1",
                    "value": "1"
                },
                {
                    "type": "imBack",
                    "title": "2",
                    "value": "2"
                }
            ]
        }
    });
}
```

<span data-ttu-id="0b651-200">Este fragmento de código muestra un ejemplo para la devolución de llamada denominada **Antes de volver a solicitar**.</span><span class="sxs-lookup"><span data-stu-id="0b651-200">This code snippet shows an example for **Before reprompting** callback.</span></span>

```javascript
module.exports.fnBeforeReprompting = function(context) {
    if(context.responses[0].text === "C") {
        return false;
    }
    return true;
}
```

### <a name="feedback-state"></a><span data-ttu-id="0b651-201">Estado de comentarios</span><span class="sxs-lookup"><span data-stu-id="0b651-201">Feedback state</span></span>

<span data-ttu-id="0b651-202">Use este estado para proporcionar una respuesta al usuario.</span><span class="sxs-lookup"><span data-stu-id="0b651-202">Use this state to provide a response back to user.</span></span> <span data-ttu-id="0b651-203">Los casos de uso típicos de este estado incluyen el resultado final después de intentar finalizar la tarea, o para proporcionar una respuesta al usuario si se produce un error, etc.</span><span class="sxs-lookup"><span data-stu-id="0b651-203">Typical use cases for this includes the final outcome after the task completion attempt or to provide a response back to user in failure conditions, etc.</span></span> 

<span data-ttu-id="0b651-204">Cada estado de comentarios requiere un nombre único, algunos valores de respuesta posibles y, opcionalmente, puede incluir definiciones de tarjetas adaptables.</span><span class="sxs-lookup"><span data-stu-id="0b651-204">Each feedback state requires a unique name, some possible response values and can optionally include adaptive card definitions.</span></span> <span data-ttu-id="0b651-205">Obtenga más información sobre la [definición de las tarjetas adaptables](conversation-designer-adaptive-cards.md).</span><span class="sxs-lookup"><span data-stu-id="0b651-205">Learn more about [adaptive cards definition](conversation-designer-adaptive-cards.md).</span></span>

<span data-ttu-id="0b651-206">Cada estado de comentarios también permite una función de devolución de llamada denominada **Al responder**, donde puede escribir su script personalizado para modificar la carga útil de la actividad antes de enviarla al usuario (si fuera necesario).</span><span class="sxs-lookup"><span data-stu-id="0b651-206">Each feedback state also allows for an **On responding** callback function where you can write your custom script to modify the activity payload if required before it is sent to the user.</span></span> 


```javascript
module.exports.fnOnResponding = function(context) {
    // include a hint card
    var activity = context.responses.slice(-1).pop();
    activity.attachments.push({
        "contentType": "application/vnd.microsoft.card.hero",
        "content": {
            "buttons": [
                {
                    "type": "imBack",
                    "title": "1",
                    "value": "1"
                },
                {
                    "type": "imBack",
                    "title": "2",
                    "value": "2"
                }
            ]
        }
    });

}
```

### <a name="module-state"></a><span data-ttu-id="0b651-207">Estado de módulo</span><span class="sxs-lookup"><span data-stu-id="0b651-207">Module state</span></span>

<span data-ttu-id="0b651-208">Un estado de módulo se usa para agregar una referencia para ejecutar un subdiálogo.</span><span class="sxs-lookup"><span data-stu-id="0b651-208">A module state is used to add a reference to execute a sub-dialogue.</span></span> <span data-ttu-id="0b651-209">Use este estado para encadenar diálogos.</span><span class="sxs-lookup"><span data-stu-id="0b651-209">Use this state to string dialogues together.</span></span> 

<span data-ttu-id="0b651-210">Cada estado de módulo debe tener un nombre único y un puntero que lleve al diálogo específico que se va a ejecutar.</span><span class="sxs-lookup"><span data-stu-id="0b651-210">Each module state requires a unique name and a pointer to a specific dialogue to execute.</span></span> <span data-ttu-id="0b651-211">Las posibles opciones para que los diálogos se ejecuten deben estar en la opción **Diálogos compartidos** o en las **Tareas** específicas.</span><span class="sxs-lookup"><span data-stu-id="0b651-211">Possible options for dialogues to execute must live under **Shared dialogues** or under the specific **Tasks**.</span></span>

## <a name="multiple-dialogues-under-a-task"></a><span data-ttu-id="0b651-212">Varios diálogos en una tarea</span><span class="sxs-lookup"><span data-stu-id="0b651-212">Multiple dialogues under a task</span></span>

<span data-ttu-id="0b651-213">Una tarea puede tener varios diálogos.</span><span class="sxs-lookup"><span data-stu-id="0b651-213">A task can have multiple dialogues.</span></span> <span data-ttu-id="0b651-214">Para agregar un diálogo a una tarea, simplemente seleccione la tarea y haga clic en el botón **Agregar** en el panel del árbol que está a la izquierda; a continuación, haga clic en **Agregar diálogo**.</span><span class="sxs-lookup"><span data-stu-id="0b651-214">To add a dialogue to a task, simply select the task and click on the **Add** button in the left tree panel then click **Add dialogue**.</span></span> <span data-ttu-id="0b651-215">Esto agregará un nuevo diálogo en la tarea seleccionada.</span><span class="sxs-lookup"><span data-stu-id="0b651-215">This will add a new dialogue under the selected task.</span></span> 

<span data-ttu-id="0b651-216">Como los diálogos admiten composición, puede vincular el flujo del diálogo raíz a la tarea que se encarga de llamar a otros diálogos de la misma.</span><span class="sxs-lookup"><span data-stu-id="0b651-216">Since dialogues are composable, you can bound the root dialogue flow to the task that calls other dialogues under the task.</span></span> <span data-ttu-id="0b651-217">Esto le permite encapsular subtareas y habilitar la opción de reutilización.</span><span class="sxs-lookup"><span data-stu-id="0b651-217">This allows you to encapsulate sub-tasks and enable reuse.</span></span> <span data-ttu-id="0b651-218">Asimismo, esta opción también le permite encadenar estos diálogos en un flujo de conversación, mediante estados de *módulo*.</span><span class="sxs-lookup"><span data-stu-id="0b651-218">This also enables you to chain these dialogues in a conversational flow using *module* states.</span></span>

## <a name="next-step"></a><span data-ttu-id="0b651-219">Paso siguiente</span><span class="sxs-lookup"><span data-stu-id="0b651-219">Next step</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="0b651-220">Plantillas de respuesta</span><span class="sxs-lookup"><span data-stu-id="0b651-220">Response templates</span></span>](conversation-designer-response-templates.md)
