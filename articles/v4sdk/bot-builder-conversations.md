---
title: Conversaciones en Bot Builder SDK | Microsoft Docs
description: Describe qué es una conversación en Bot Builder SDK.
keywords: flujo de conversación, reconocimiento de intenciones, un solo turno, varios turnos
author: jonathanfingold
ms.author: jonathanfingold
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 04/11/2018
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: 679109cad2f7b0c0c5826a47884b98e1149cb380
ms.sourcegitcommit: f95702d27abbd242c902eeb218d55a72df56ce56
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/19/2018
ms.locfileid: "39306480"
---
# <a name="conversation-flow"></a><span data-ttu-id="f3477-104">Flujo de conversación</span><span class="sxs-lookup"><span data-stu-id="f3477-104">Conversation flow</span></span>
[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]

<span data-ttu-id="f3477-105">Dado que un bot puede considerarse como una interfaz de usuario conversacional, el flujo de conversación es la manera en que se interactúa con el usuario, y puede adoptar diferentes formas.</span><span class="sxs-lookup"><span data-stu-id="f3477-105">Since a bot can be thought of as a conversational user interface, the conversation flow is how we interact with the user and can take different forms.</span></span> <span data-ttu-id="f3477-106">Un flujo de conversación correcto ayuda a mejorar la interacción del usuario y el rendimiento del bot.</span><span class="sxs-lookup"><span data-stu-id="f3477-106">Having the right conversation flow helps improve the user's interaction and the performance of your bot.</span></span>

<span data-ttu-id="f3477-107">Para diseñar del flujo de conversación de un bot es necesario decidir cómo responde el bot cuando el usuario le dice algo.</span><span class="sxs-lookup"><span data-stu-id="f3477-107">Designing a bot's conversation flow involves deciding how a bot responds when the user says something to it.</span></span> <span data-ttu-id="f3477-108">Un bot reconoce en primer lugar la tarea o el tema de conversación basándose en un mensaje del usuario.</span><span class="sxs-lookup"><span data-stu-id="f3477-108">A bot first recognizes the task or conversation topic based on a message from the user.</span></span> <span data-ttu-id="f3477-109">Para determinar la tarea o el tema (lo que se denomina *intención*) asociado al mensaje de un usuario, el bot puede buscar palabras o patrones en el texto del mensaje, bien puede aprovechar servicios como [Language Understanding (LUIS)](bot-builder-concept-luis.md) y QnA Maker.</span><span class="sxs-lookup"><span data-stu-id="f3477-109">To determine the task or topic (known as the *intent*) associated with a user's message, the bot can look for words or patterns in the text of the user's message, or it can take advantage of services like [Language Understanding (LUIS)](bot-builder-concept-luis.md) and QnA Maker.</span></span> 

<span data-ttu-id="f3477-110">Una vez que ha reconocido la intención del usuario, en función del escenario, el bot podría satisfacer la solicitud del usuario con una sola respuesta, con lo que completaría la conversación en un solo turno, o podría requerir una serie de turnos.</span><span class="sxs-lookup"><span data-stu-id="f3477-110">Once the bot has recognized the user's intent, depending on the scenario, the bot could fulfill the user's request with a single reply, completing the conversation in one turn, or it might require a series of turns.</span></span> <span data-ttu-id="f3477-111">Para los flujos de conversación de varios turnos, Bot Builder SDK proporciona [administración de estados](./bot-builder-howto-v4-state.md) para llevar un seguimiento de una conversación, [avisos](bot-builder-prompts.md) para pedir información y [cuadros de diálogo](bot-builder-dialog-manage-conversation-flow.md) para encapsular los flujos de conversación.</span><span class="sxs-lookup"><span data-stu-id="f3477-111">For multi-turn conversation flows, the Bot Builder SDK provides [state management](./bot-builder-howto-v4-state.md) for keeping track of a conversation, [prompts](bot-builder-prompts.md) for asking for information, and [dialogs](bot-builder-dialog-manage-conversation-flow.md) for encapsulating conversation flows.</span></span> 

<span data-ttu-id="f3477-112">En un bot complejo con varios subsistemas, usted podría usar varios servicios para reconocer la intención, uno para cada subcomponente del bot.</span><span class="sxs-lookup"><span data-stu-id="f3477-112">In a complex bot with multiple subsystems, it can be the case that you use multiple services to recognize intent, one for each subcomponent of the bot.</span></span> <span data-ttu-id="f3477-113">La [herramienta de distribución](bot-builder-tutorial-dispatch.md) coloca en un mismo lugar los resultados de varios servicios cuando se combinan subsistemas de conversación en un bot.</span><span class="sxs-lookup"><span data-stu-id="f3477-113">The [Dispatch tool](bot-builder-tutorial-dispatch.md) gets the results of multiple services in one place when you combine conversational subsystems into one bot.</span></span> 
<!-- 
A conversation identifies a series of activities sent between a bot and a user on a specific channel and represents an interaction between one or more bots and either a _direct_ conversation with a specific user or a _group_ conversation with multiple users.
A bot communicates with a user on a channel by receiving activities from, and sending activities to the user.

- Each user has an ID that is unique per channel.
- Each conversation has an ID that is unique per channel.
- The channel sets the conversation ID when it starts the conversation.
- The bot cannot start a conversation; however, once it has a conversation ID, it can resume that conversation.
- Not all channels support group conversations.
-->

## <a name="single-turn-conversation"></a><span data-ttu-id="f3477-114">Conversación de un solo turno</span><span class="sxs-lookup"><span data-stu-id="f3477-114">Single turn conversation</span></span>

<span data-ttu-id="f3477-115">El flujo de conversación más sencillo es el de un solo turno.</span><span class="sxs-lookup"><span data-stu-id="f3477-115">The simplest conversational flow is single-turn.</span></span> <span data-ttu-id="f3477-116">En un flujo de un solo turno, el bot finaliza su tarea en un turno, que consta de un mensaje del usuario y una respuesta del bot.</span><span class="sxs-lookup"><span data-stu-id="f3477-116">In a single-turn flow, the bot finishes its task in one turn, consisting of one message from the user and one reply from the bot.</span></span> 



<!-- 
The EchoBot sample in the BotBuilder SDK is a single-turn bot. Here are other examples of single turn conversation flow:
* A bot for getting the weather report, that just tells the user what the weather is, when they say "What's the weather?".
* An IoT bot that responds to "turn on the lights" by calling an IoT service. -->

<span data-ttu-id="f3477-117"><!-- The following isn't always true, it's a generalization --> El tipo más sencillo de bot de un solo turno no necesita llevar un seguimiento del estado de la conversación.</span><span class="sxs-lookup"><span data-stu-id="f3477-117"><!-- The following isn't always true, it's a generalization --> The simplest kind of single-turn bot doesn't need to keep track of conversation state.</span></span> <span data-ttu-id="f3477-118">Cada vez que recibe un mensaje, responde únicamente en función del contexto del mensaje entrante actual, sin tener conocimiento de turnos de conversación anteriores.</span><span class="sxs-lookup"><span data-stu-id="f3477-118">Each time it receives a message, it responds based only on the context of the current incoming message, without knowledge of past conversational turns.</span></span>

![Bot del tiempo de un solo turno](./media/concept-conversation/weather-single-turn.png)

<span data-ttu-id="f3477-120">Un bot del tiempo tiene un flujo de un solo turno. Simplemente le proporciona al usuario un informe meteorológico, sin necesidad de preguntar la ciudad o la fecha.</span><span class="sxs-lookup"><span data-stu-id="f3477-120">A weather bot has a single-turn flow, it just gives the user a weather report, without going back and forth asking for the city or the date.</span></span> <span data-ttu-id="f3477-121">Toda la lógica para mostrar el informe meteorológico se basa en el mensaje que el bot acaba de recibir.</span><span class="sxs-lookup"><span data-stu-id="f3477-121">All the logic for displaying the weather report is based on the message the bot just received.</span></span> <span data-ttu-id="f3477-122">En cada turno de una conversación, el bot recibe un contexto de turno que puede usar para determinar qué hará a continuación y cómo fluye la conversación.</span><span class="sxs-lookup"><span data-stu-id="f3477-122">In each turn of a conversation, the bot receives a turn context, which your bot can use to determine what to do next and how the conversation flows.</span></span> 

## <a name="multiple-turns"></a><span data-ttu-id="f3477-123">Varios turnos</span><span class="sxs-lookup"><span data-stu-id="f3477-123">Multiple turns</span></span>

<span data-ttu-id="f3477-124">La mayoría de los tipos de conversación no se puede completar en un solo turno, por lo que un bot también puede tener un flujo de conversación de varios turnos.</span><span class="sxs-lookup"><span data-stu-id="f3477-124">Most types of conversation can't be completed in a single turn, so a bot can also have a multi-turn conversation flow.</span></span> <span data-ttu-id="f3477-125">Algunos escenarios que requieren varios turnos de conversación incluyen lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="f3477-125">Some scenarios that require multiple conversational turns include:</span></span>

 * <span data-ttu-id="f3477-126">Un bot que le pide al usuario la información adicional que necesita para completar una tarea.</span><span class="sxs-lookup"><span data-stu-id="f3477-126">A bot that prompts the user for additional information that it needs to complete a task.</span></span> <span data-ttu-id="f3477-127">El bot debe comprobar si cuenta con todos los parámetros para cumplir la tarea.</span><span class="sxs-lookup"><span data-stu-id="f3477-127">The bot needs to track whether it has all the parameters for fulfilling the task.</span></span>
 * <span data-ttu-id="f3477-128">Un bot que guía al usuario a través de los pasos en un proceso, como la realización de un pedido.</span><span class="sxs-lookup"><span data-stu-id="f3477-128">A bot that guides the user through steps in a process, such as placing an order.</span></span> <span data-ttu-id="f3477-129">El bot debe llevar un seguimiento de dónde se encuentra el usuario en la secuencia de pasos.</span><span class="sxs-lookup"><span data-stu-id="f3477-129">The bot needs to track where the user is in the sequence of steps.</span></span>

<span data-ttu-id="f3477-130">Por ejemplo, un bot del tiempo tiene un flujo de varios turnos si responde a la pregunta "¿Qué tiempo hace?"</span><span class="sxs-lookup"><span data-stu-id="f3477-130">For example, a weather bot has a multi-turn flow, if the bot responds to "what's the weather?"</span></span> <span data-ttu-id="f3477-131">preguntando por la ciudad.</span><span class="sxs-lookup"><span data-stu-id="f3477-131">by asking for the city.</span></span>

![Bot del tiempo de varios turnos](./media/concept-conversation/weather-multi-turn.png)

<span data-ttu-id="f3477-133">Cuando el usuario responde al mensaje del bot en el que le pregunta por la ciudad y el bot recibe la respuesta "Seattle", el bot necesita tener contexto guardado para entender que el mensaje actual es la respuesta a una pregunta anterior y que forma parte de una solicitud para obtener el tiempo.</span><span class="sxs-lookup"><span data-stu-id="f3477-133">When the user replies to the bot's prompt for the city and the bot receives "Seattle", the bot needs to have some context saved to understand that the current message is the response to a previous prompt and part of a request to get weather.</span></span> <span data-ttu-id="f3477-134">Los bots de varios turnos llevan un seguimiento del estado para responder de forma adecuada a los mensajes nuevos.</span><span class="sxs-lookup"><span data-stu-id="f3477-134">Multi-turn bots keep track of state to respond appropriately to new messages.</span></span>

<!--
```
// TBD: snippet showing receiving message and using ConversationProperties
```
-->

<span data-ttu-id="f3477-135">Vea [Managing state](bot-builder-storage-concept.md) (Administración del estado) para obtener información general sobre cómo administrar el estado y [How to use user and conversation properties](bot-builder-howto-v4-state.md) (Cómo usar las propiedades de usuario y conversación) para obtener un ejemplo.</span><span class="sxs-lookup"><span data-stu-id="f3477-135">See [Managing state](bot-builder-storage-concept.md) for an overview of managing state, and see [How to use user and conversation properties](bot-builder-howto-v4-state.md) for an example.</span></span>

> [!NOTE]
> <span data-ttu-id="f3477-136">Las conversaciones de varios turnos con clientes de la API de REST deben llevar un seguimiento de su propio estado, por ejemplo, en una base de datos o Table Storage.</span><span class="sxs-lookup"><span data-stu-id="f3477-136">Multi-turn conversations with REST API clients will need to keep track of their own state, for example in a database or table storage.</span></span> 

## <a name="conversation-topics"></a><span data-ttu-id="f3477-137">Temas de conversación</span><span class="sxs-lookup"><span data-stu-id="f3477-137">Conversation topics</span></span>

<span data-ttu-id="f3477-138">Tal vez diseñe su bot para que controle más de un tipo de tareas.</span><span class="sxs-lookup"><span data-stu-id="f3477-138">You might design your bot to handle more than one type of task.</span></span> <span data-ttu-id="f3477-139">Por ejemplo, podría tener un bot que proporcione flujos de conversación diferentes para saludar al usuario, realizar un pedido, cancelar y obtener ayuda.</span><span class="sxs-lookup"><span data-stu-id="f3477-139">For example, you might have a bot that provides different conversation flows for greeting the user, placing an order, canceling, and getting help.</span></span> <span data-ttu-id="f3477-140">Una manera de controlar este cambio en la conversación entre tareas o temas diferentes consiste en reconocer la intención (lo que el usuario quiere hacer) del mensaje actual.</span><span class="sxs-lookup"><span data-stu-id="f3477-140">One way to handle this switch between conversation for different tasks or conversation topics is to recognize the intent (what the user wants to do) from the current message.</span></span> 

### <a name="recognize-intent"></a><span data-ttu-id="f3477-141">Reconocimiento de intenciones</span><span class="sxs-lookup"><span data-stu-id="f3477-141">Recognize intent</span></span>

<span data-ttu-id="f3477-142">Bot Builder SDK proporciona _reconocedores_ que procesan cada mensaje entrante para determinar la intención, de modo que el bot pueda iniciar el flujo de conversación correspondiente.</span><span class="sxs-lookup"><span data-stu-id="f3477-142">The Bot Builder SDK supplies _recognizers_ that process each incoming message to determine intent, so your bot can initiate the appropriate conversational flow.</span></span> <span data-ttu-id="f3477-143">Antes de la _devolución de llamada de recepción_, los reconocedores examinan el contenido del mensaje del usuario para determinar la intención y, después, devuelven la intención al bot mediante el uso del objeto de contexto de turno dentro de la devolución de llamada de recepción, almacenada como la **intención principal** en el objeto de contexto de turno.</span><span class="sxs-lookup"><span data-stu-id="f3477-143">Before the _receive callback_, recognizers look at the message content from the user to determine intent, and then return the intent to the bot using the turn context object within the receive callback, stored as the **Top Intent** on the turn context object.</span></span> 

<span data-ttu-id="f3477-144">El reconocedor que determina la **intención principal** puede usar simplemente expresiones regulares, Language Understanding (LUIS) u otra lógica que se haya desarrollado como software intermedio.</span><span class="sxs-lookup"><span data-stu-id="f3477-144">The recognizer that determines **Top Intent** can simply use regular expressions, Language Understanding (LUIS), or other logic that you develop as middleware.</span></span> <span data-ttu-id="f3477-145">Aquí se muestran ejemplos de posibles reconocedores:</span><span class="sxs-lookup"><span data-stu-id="f3477-145">The following could be examples of recognizers:</span></span>
   
* <span data-ttu-id="f3477-146">Configura un reconocedor mediante expresiones regulares para detectar cada vez que un usuario emplea la palabra "help" (ayuda).</span><span class="sxs-lookup"><span data-stu-id="f3477-146">You set up a recognizer using regular expressions to detect every time a user says the word help.</span></span>
* <span data-ttu-id="f3477-147">Usa Language Understanding (LUIS) para entrenar un servicio con ejemplos de las maneras en las que un usuario podría pedir ayuda y lo asigna a la intención "Help".</span><span class="sxs-lookup"><span data-stu-id="f3477-147">You use Language Understanding (LUIS) to train a service with examples of ways user might ask for help, and map that to the "Help" intent.</span></span>
* <span data-ttu-id="f3477-148">Crea su propio software intermedio de reconocedor, que inspecciona las actividades entrantes y devuelve la intención "translate" cada vez que detecta un mensaje en otro idioma.</span><span class="sxs-lookup"><span data-stu-id="f3477-148">You create your own recognizer middleware that inspects incoming activities and returns the "translate" intent every time it detects a message in another language.</span></span>

<span data-ttu-id="f3477-149">Para más información, vea [Language Understanding with LUIS](bot-builder-concept-luis.md) (Language Understanding con LUIS).</span><span class="sxs-lookup"><span data-stu-id="f3477-149">For more info [Language Understanding with LUIS](bot-builder-concept-luis.md).</span></span> <!-- TODO: ADD THIS TOPIC OR SNIPPET-->

### <a name="consider-how-to-interrupt-conversation-flow-or-change-topics"></a><span data-ttu-id="f3477-150">Cómo interrumpir el flujo de conversación o cambiar de tema</span><span class="sxs-lookup"><span data-stu-id="f3477-150">Consider how to interrupt conversation flow or change topics</span></span>

<span data-ttu-id="f3477-151">Una manera de llevar un seguimiento del punto de la conversación en el que se encuentra consiste en usar el [estado de la conversación](bot-builder-howto-v4-state.md). De este modo, se guarda información sobre el tema activo o sobre los pasos de una secuencia que se han completado.</span><span class="sxs-lookup"><span data-stu-id="f3477-151">One way to keep track of where you are in a conversation is to use [conversation state](bot-builder-howto-v4-state.md) to save information about the currently active topic or what steps in a sequence have been completed.</span></span>

<span data-ttu-id="f3477-152">Cuando un bot se vuelve más complejo, también se puede concebir una secuencia de flujos de conversación que se producen en una pila. Por ejemplo, el bot invoca el flujo del nuevo pedido y, después, invoca el de la búsqueda de productos.</span><span class="sxs-lookup"><span data-stu-id="f3477-152">When a bot becomes more complex, you can also imagine a sequence of conversation flows occurring in a stack; for instance, the bot will invoke the new order flow, and then invoke the product search flow.</span></span> <span data-ttu-id="f3477-153">Después, el usuario selecciona un producto y lo confirma, con lo que se completa el flujo de búsqueda de productos y, luego, el pedido.</span><span class="sxs-lookup"><span data-stu-id="f3477-153">Then the user will select a product and confirm, completing the product search flow, and then complete the order.</span></span>

<span data-ttu-id="f3477-154">El problema es que una conversación rara vez sigue una ruta tan lineal y lógica.</span><span class="sxs-lookup"><span data-stu-id="f3477-154">However, conversation rarely follows such a linear, logical path.</span></span> <span data-ttu-id="f3477-155">Los usuarios no se comunican en "pilas", sino que tienden a cambiar de opinión con frecuencia.</span><span class="sxs-lookup"><span data-stu-id="f3477-155">Users do not communicate in "stacks", instead they tend to frequently change their minds.</span></span> <span data-ttu-id="f3477-156">Considere el siguiente ejemplo:</span><span class="sxs-lookup"><span data-stu-id="f3477-156">Consider the following example:</span></span>

![El usuario dice algo inesperado](./media/concept-conversation/interruption.png)

<span data-ttu-id="f3477-158">Mientras que el bot podría haber construido lógicamente una pila de flujos, el usuario podría decidir hacer algo completamente diferente o formular una pregunta que no tenga ninguna relación con el tema actual.</span><span class="sxs-lookup"><span data-stu-id="f3477-158">While your bot may have logically constructed a stack of flows, the user may decide to do something entirely different or ask a question that may be unrelated to the current topic.</span></span> <span data-ttu-id="f3477-159">En el ejemplo, el usuario hace una pregunta en lugar de facilitar la respuesta afirmativa o negativa que el flujo espera.</span><span class="sxs-lookup"><span data-stu-id="f3477-159">In the example, the user asks a question rather than providing the yes/no response that the flow expects.</span></span> <span data-ttu-id="f3477-160">¿Cómo debe responder el flujo?</span><span class="sxs-lookup"><span data-stu-id="f3477-160">How should your flow respond?</span></span>

* <span data-ttu-id="f3477-161">Insiste en que el usuario responda primero a la pregunta.</span><span class="sxs-lookup"><span data-stu-id="f3477-161">Insist that the user answer the question first.</span></span>
* <span data-ttu-id="f3477-162">Pasa por alto todo lo que el usuario ha hecho hasta el momento, restablece la pila del flujo al completo y empieza desde cero intentando responder a la pregunta del usuario.</span><span class="sxs-lookup"><span data-stu-id="f3477-162">Disregard everything that the user had done previously, reset the whole flow stack, and start from the beginning by attempting to answer the user's question.</span></span>
* <span data-ttu-id="f3477-163">Intenta responder a la pregunta del usuario y, después, regresa a la pregunta con respuesta afirmativa o negativa para intentar reanudar la conversación desde ahí.</span><span class="sxs-lookup"><span data-stu-id="f3477-163">Attempt to answer the user's question and then return to that yes/no question and try to resume from there.</span></span>

<span data-ttu-id="f3477-164">No hay una respuesta correcta a esta pregunta, ya que la mejor solución dependerá de los aspectos concretos del escenario y de cómo espera el usuario que responda el bot.</span><span class="sxs-lookup"><span data-stu-id="f3477-164">There is no right answer to this question, as the best solution will depend upon the specifics of your scenario and how the user would reasonably expect the bot to respond.</span></span> <span data-ttu-id="f3477-165">Para más información, vea [Handle user interrupt](bot-builder-howto-handle-user-interrupt.md) (Controlar las interrupciones del usuario).</span><span class="sxs-lookup"><span data-stu-id="f3477-165">For more information, see [Handle user interrupt](bot-builder-howto-handle-user-interrupt.md).</span></span>

> [!TIP]
> <span data-ttu-id="f3477-166">Si usa Bot Builder SDK para Node.Js, puede usar [cuadros de diálogo](bot-builder-dialog-manage-conversation-flow.md) para administrar el flujo de conversación.</span><span class="sxs-lookup"><span data-stu-id="f3477-166">If you're using the Bot Builder SDK for Node.Js, you can use [Dialogs](bot-builder-dialog-manage-conversation-flow.md) to manage conversation flow.</span></span>

## <a name="conversation-lifetime"></a><span data-ttu-id="f3477-167">Duración de la conversación</span><span class="sxs-lookup"><span data-stu-id="f3477-167">Conversation lifetime</span></span>

<span data-ttu-id="f3477-168"><!-- Note: these activities are dependent on whether the channel actually sends them. Also, we should add links --> Un bot recibe una actividad de _actualización de la conversación_ cada vez que se ha agregado a una conversación, se han agregado otros miembros a la conversación (o se han eliminado de ella) o han cambiado los metadatos de la conversación.</span><span class="sxs-lookup"><span data-stu-id="f3477-168"><!-- Note: these activities are dependent on whether the channel actually sends them. Also, we should add links --> A bot receives a _conversation update_ activity whenever it has been added to a conversation, other members have been added to or removed from a conversation, or conversation metadata has changed.</span></span>
<span data-ttu-id="f3477-169">Puede que le interese que el bot reaccione a las actividades de actualización de la conversación saludando a los usuarios o presentándose.</span><span class="sxs-lookup"><span data-stu-id="f3477-169">You may want to have your bot react to conversation update activities by greeting users or introducing itself.</span></span>

<span data-ttu-id="f3477-170">Un bot recibe una actividad de _finalización de la conversación_ para indicar que el usuario ha puesto fin a la conversación.</span><span class="sxs-lookup"><span data-stu-id="f3477-170">A bot receives an _end of conversation_ activity to indicate that the user has ended the conversation.</span></span> <span data-ttu-id="f3477-171">Un bot podría enviar una actividad de _finalización de la conversación_ para indicarle al usuario que la conversación está llegando a su fin.</span><span class="sxs-lookup"><span data-stu-id="f3477-171">A bot may send an _end of conversation_ activity to indicate to the user that the conversation is ending.</span></span> <span data-ttu-id="f3477-172">Si va a almacenar información sobre la conversación, puede que le interese borrar esa información cuando la conversación haya finalizado.</span><span class="sxs-lookup"><span data-stu-id="f3477-172">If you are storing information about the conversation, you may want to clear that information when the conversation ends.</span></span>

<!--  Types of conversations

Your bot can support multi-turn interactions where it prompts users for multiple peices of information. It can be focused on a very specific task or support multiple types of tasks. 
The Bot Builder SDK has some built-in support for Language Understatnding (LUIS) and QnA Maker for adding natural language "question and answer" features to your bot.

<!--TODO: Add with links when these topics are available:
[Conversation flow] and other design articles.
[Using recognizers] [Using state and storage] and other how tos.
-->
## <a name="conversations-channels-and-users"></a><span data-ttu-id="f3477-173">Conversaciones, canales y usuarios</span><span class="sxs-lookup"><span data-stu-id="f3477-173">Conversations, channels, and users</span></span>

<span data-ttu-id="f3477-174">Las conversaciones pueden ser de dos tipos: una conversación _directa_ con un usuario específico o una conversación de _grupo_ con varios usuarios.</span><span class="sxs-lookup"><span data-stu-id="f3477-174">Conversations can be either a _direct_ conversation with a specific user or a _group_ conversation with multiple users.</span></span>
<span data-ttu-id="f3477-175">Para comunicarse con un usuario en un canal, un bot recibe actividades del usuario y a su vez le envía actividades.</span><span class="sxs-lookup"><span data-stu-id="f3477-175">A bot communicates with a user on a channel by receiving activities from, and sending activities to the user.</span></span>

- <span data-ttu-id="f3477-176">Cada usuario tiene un identificador único para cada canal.</span><span class="sxs-lookup"><span data-stu-id="f3477-176">Each user has an ID that is unique per channel.</span></span>
- <span data-ttu-id="f3477-177">Cada conversación tiene un identificador único para cada canal.</span><span class="sxs-lookup"><span data-stu-id="f3477-177">Each conversation has an ID that is unique per channel.</span></span>
- <span data-ttu-id="f3477-178">El canal establece el identificador de conversación cuando inicia la conversación.</span><span class="sxs-lookup"><span data-stu-id="f3477-178">The channel sets the conversation ID when it starts the conversation.</span></span>
- <span data-ttu-id="f3477-179">El bot no puede iniciar una conversación, pero en cuanto dispone de un identificador de conversación, puede reanudarla.</span><span class="sxs-lookup"><span data-stu-id="f3477-179">The bot cannot start a conversation; however, once it has a conversation ID, it can resume that conversation.</span></span>
- <span data-ttu-id="f3477-180">No todos los canales admiten conversaciones de grupo.</span><span class="sxs-lookup"><span data-stu-id="f3477-180">Not all channels support group conversations.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f3477-181">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="f3477-181">Next steps</span></span>

<span data-ttu-id="f3477-182">En el caso de conversaciones complejas, como algunas de las indicadas anteriormente, es necesario poder conservar la información durante más de un turno.</span><span class="sxs-lookup"><span data-stu-id="f3477-182">For complex conversations, such as some of those highlighted above, we need to be able to persist information for longer than a turn.</span></span> <span data-ttu-id="f3477-183">Echemos ahora un vistazo al estado y al almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="f3477-183">Lets look at state and storage next.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="f3477-184">Estado y almacenamiento</span><span class="sxs-lookup"><span data-stu-id="f3477-184">State and Storage</span></span>](bot-builder-storage-concept.md)

<!-- In addition, your bot can send activities back to the user, either _proactively_, in response to internal logic, or _reactively_, in response to an activity from the user or channel.-->
<!--TODO: Link to messaging how tos.-->

<!--  TODO: Change to next steps, one for each of LUIS and State
## See also

- Activities
- Adapter
- Context
- Proactive messaging
- State
-->

[QnAMaker]:(bot-builder-luis-and-qna.md#using-qna-maker)

<!-- TODO: Update when the Dispatch concept is pushed -->
[Dispatch]:(bot-builder-concept-luis.md)
