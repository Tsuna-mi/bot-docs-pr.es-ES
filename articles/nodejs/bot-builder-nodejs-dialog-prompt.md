---
title: Petición de datos de entrada al usuario | Microsoft Docs
description: Aprenda a usar avisos para recopilar información del usuario con Bot Builder SDK para Node.js.
author: v-ducvo
ms.author: v-ducvo
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: aa20dc396b68ede3271d12a8deab2e673a79d1d1
ms.sourcegitcommit: 2dc75701b169d822c9499e393439161bc87639d2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42904487"
---
# <a name="prompt-for-user-input"></a><span data-ttu-id="8a90f-103">Petición de datos de entrada al usuario</span><span class="sxs-lookup"><span data-stu-id="8a90f-103">Prompt for user input</span></span>

[!INCLUDE [pre-release-label](../includes/pre-release-label-v3.md)]

<span data-ttu-id="8a90f-104">Bot Builder SDK para Node.js proporciona un conjunto de avisos integrados para simplificar la recopilación de información de un usuario.</span><span class="sxs-lookup"><span data-stu-id="8a90f-104">The Bot Builder SDK for Node.js provides a set of built-in prompts to simplify collecting inputs from a user.</span></span> 

<span data-ttu-id="8a90f-105">Cada vez que un bot necesita que intervenga el usuario, se usa un *aviso*.</span><span class="sxs-lookup"><span data-stu-id="8a90f-105">A *prompt* is used whenever a bot needs input from the user.</span></span> <span data-ttu-id="8a90f-106">Para pedir al usuario una serie de datos de entrada, se pueden encadenar avisos en una cascada.</span><span class="sxs-lookup"><span data-stu-id="8a90f-106">You can use prompts to ask a user for a series of inputs by chaining the prompts in a waterfall.</span></span> <span data-ttu-id="8a90f-107">Puede usar avisos junto con la [cascada](bot-builder-nodejs-dialog-waterfall.md) como ayuda para [administrar el flujo de conversación](bot-builder-nodejs-manage-conversation-flow.md) del bot.</span><span class="sxs-lookup"><span data-stu-id="8a90f-107">You can use prompts in conjunction with [waterfall](bot-builder-nodejs-dialog-waterfall.md) to help you [manage conversation flow](bot-builder-nodejs-manage-conversation-flow.md) in your bot.</span></span> 

<span data-ttu-id="8a90f-108">Este artículo le ayudará a comprender cómo funcionan los avisos y cómo usarlos para recopilar información de los usuarios.</span><span class="sxs-lookup"><span data-stu-id="8a90f-108">This article will help you understand how prompts work and how you can use them to collect information from users.</span></span>

## <a name="prompts-and-responses"></a><span data-ttu-id="8a90f-109">Avisos y respuestas</span><span class="sxs-lookup"><span data-stu-id="8a90f-109">Prompts and responses</span></span>

<span data-ttu-id="8a90f-110">Siempre que necesite la intervención de un usuario, puede enviar un aviso, esperar a que el usuario proporcione los datos de entrada y, luego, procesar la información y enviar una respuesta al usuario.</span><span class="sxs-lookup"><span data-stu-id="8a90f-110">Whenever you need input from a user, you can send a prompt, wait for the user to respond with input, and then process the input and send a response to the user.</span></span>

<span data-ttu-id="8a90f-111">El siguiente código de ejemplo pide al usuario su nombre y responde con un mensaje de saludo.</span><span class="sxs-lookup"><span data-stu-id="8a90f-111">The following code sample prompts the user for their name and responds with a greeting message.</span></span>

```javascript
bot.dialog('greetings', [
    // Step 1
    function (session) {
        builder.Prompts.text(session, 'Hi! What is your name?');
    },
    // Step 2
    function (session, results) {
        session.endDialog(`Hello ${results.response}!`);
    }
]);
```

<span data-ttu-id="8a90f-112">Con esta construcción básica, puede modelar el flujo de conversación y agregar tantos avisos y respuestas como el bot necesite.</span><span class="sxs-lookup"><span data-stu-id="8a90f-112">Using this basic construct, you can model your conversation flow by adding as many prompts and responses as your bot requires.</span></span>

## <a name="prompt-results"></a><span data-ttu-id="8a90f-113">Resultados de los avisos</span><span class="sxs-lookup"><span data-stu-id="8a90f-113">Prompt results</span></span> 

<span data-ttu-id="8a90f-114">Los avisos integrados se implementan como [diálogos](bot-builder-nodejs-dialog-overview.md) que devuelven la respuesta del usuario en el campo `results.response`.</span><span class="sxs-lookup"><span data-stu-id="8a90f-114">Built-in prompts are implemented as [dialogs](bot-builder-nodejs-dialog-overview.md) that return the user's response in the `results.response` field.</span></span> <span data-ttu-id="8a90f-115">En el caso de objetos JSON, las respuestas se devuelven en el campo `results.response.entity`.</span><span class="sxs-lookup"><span data-stu-id="8a90f-115">For JSON objects, responses are returned in the `results.response.entity` field.</span></span> <span data-ttu-id="8a90f-116">Cualquier tipo de [controlador de diálogo](bot-builder-nodejs-dialog-overview.md#dialog-handlers) puede recibir el resultado de un aviso.</span><span class="sxs-lookup"><span data-stu-id="8a90f-116">Any type of [dialog handler](bot-builder-nodejs-dialog-overview.md#dialog-handlers) can receive the result of a prompt.</span></span> <span data-ttu-id="8a90f-117">Una vez que el bot recibe una respuesta, puede usarla o pasarla de nuevo al diálogo de llamada mediante la invocación del método [`session.endDialogWithResult`][EndDialogWithResult].</span><span class="sxs-lookup"><span data-stu-id="8a90f-117">Once the bot receives a response, it can consume it or pass it back to the calling dialog by calling the [`session.endDialogWithResult`][EndDialogWithResult] method.</span></span>

<span data-ttu-id="8a90f-118">En el código de ejemplo siguiente se muestra cómo devolver el resultado de un aviso al diálogo de llamada con el método `session.endDialogWithResult`.</span><span class="sxs-lookup"><span data-stu-id="8a90f-118">The following code sample shows how to return a prompt result to the calling dialog by using the `session.endDialogWithResult` method.</span></span> <span data-ttu-id="8a90f-119">En este ejemplo, el diálogo `greetings` usa el resultado del aviso que devuelve el diálogo `askName` para saludar al usuario por su nombre.</span><span class="sxs-lookup"><span data-stu-id="8a90f-119">In this example, the `greetings` dialog uses the prompt result that the `askName` dialog returns to greet the user by name.</span></span>

```javascript
// Ask the user for their name and greet them by name.
bot.dialog('greetings', [
    function (session) {
        session.beginDialog('askName');
    },
    function (session, results) {
        session.endDialog(`Hello ${results.response}!`);
    }
]);
bot.dialog('askName', [
    function (session) {
        builder.Prompts.text(session, 'Hi! What is your name?');
    },
    function (session, results) {
        session.endDialogWithResult(results);
    }
]);
```

## <a name="prompt-types"></a><span data-ttu-id="8a90f-120">Tipos de avisos</span><span class="sxs-lookup"><span data-stu-id="8a90f-120">Prompt types</span></span>
<span data-ttu-id="8a90f-121">Bot Builder SDK para Node.js incluye varios tipos diferentes de avisos integrados.</span><span class="sxs-lookup"><span data-stu-id="8a90f-121">The Bot Builder SDK for Node.js includes several different types of built-in prompts.</span></span> 

|<span data-ttu-id="8a90f-122">**Tipo de aviso**</span><span class="sxs-lookup"><span data-stu-id="8a90f-122">**Prompt type**</span></span>     | <span data-ttu-id="8a90f-123">**Descripción**</span><span class="sxs-lookup"><span data-stu-id="8a90f-123">**Description**</span></span> |     
| ------------------ | --------------- |
|[<span data-ttu-id="8a90f-124">Prompts.text</span><span class="sxs-lookup"><span data-stu-id="8a90f-124">Prompts.text</span></span>](#promptstext) | <span data-ttu-id="8a90f-125">Pide al usuario que escriba una cadena de texto.</span><span class="sxs-lookup"><span data-stu-id="8a90f-125">Asks the user to enter a string of text.</span></span> |     
|[<span data-ttu-id="8a90f-126">Prompts.confirm</span><span class="sxs-lookup"><span data-stu-id="8a90f-126">Prompts.confirm</span></span>](#promptsconfirm) | <span data-ttu-id="8a90f-127">Pide al usuario que confirme una acción.</span><span class="sxs-lookup"><span data-stu-id="8a90f-127">Asks the user to confirm an action.</span></span>| 
|[<span data-ttu-id="8a90f-128">Prompts.number</span><span class="sxs-lookup"><span data-stu-id="8a90f-128">Prompts.number</span></span>](#promptsnumber) | <span data-ttu-id="8a90f-129">Pide al usuario que escriba un número.</span><span class="sxs-lookup"><span data-stu-id="8a90f-129">Asks the user to enter a number.</span></span>     |
|[<span data-ttu-id="8a90f-130">Prompts.time</span><span class="sxs-lookup"><span data-stu-id="8a90f-130">Prompts.time</span></span>](#promptstime) | <span data-ttu-id="8a90f-131">Pide al usuario una hora o la fecha y la hora.</span><span class="sxs-lookup"><span data-stu-id="8a90f-131">Asks the user for a time or date/time.</span></span>      |
|[<span data-ttu-id="8a90f-132">Prompts.choice</span><span class="sxs-lookup"><span data-stu-id="8a90f-132">Prompts.choice</span></span>](#promptschoice) | <span data-ttu-id="8a90f-133">Pide al usuario que elija de una lista de opciones.</span><span class="sxs-lookup"><span data-stu-id="8a90f-133">Asks the user to choose from a list of options.</span></span>    |
|[<span data-ttu-id="8a90f-134">Prompts.attachment</span><span class="sxs-lookup"><span data-stu-id="8a90f-134">Prompts.attachment</span></span>](#promptsattachment) | <span data-ttu-id="8a90f-135">Pide al usuario que cargue una imagen o un vídeo.</span><span class="sxs-lookup"><span data-stu-id="8a90f-135">Asks the user to upload a picture or video.</span></span>|       

<span data-ttu-id="8a90f-136">En las secciones siguientes se proporcionan detalles adicionales sobre cada tipo de aviso.</span><span class="sxs-lookup"><span data-stu-id="8a90f-136">The following sections provide additional details about each type of prompt.</span></span>

### <a name="promptstext"></a><span data-ttu-id="8a90f-137">Prompts.text</span><span class="sxs-lookup"><span data-stu-id="8a90f-137">Prompts.text</span></span>

<span data-ttu-id="8a90f-138">Use el método [Prompts.text()][PromptsText] para pedir al usuario una **cadena de texto**.</span><span class="sxs-lookup"><span data-stu-id="8a90f-138">Use the [Prompts.text()][PromptsText] method to ask the user for a **string of text**.</span></span> <span data-ttu-id="8a90f-139">El aviso devuelve la respuesta del usuario como [IPromptTextResult][IPromptTextResult].</span><span class="sxs-lookup"><span data-stu-id="8a90f-139">The prompt returns the user's response as an [IPromptTextResult][IPromptTextResult].</span></span>

```javascript
builder.Prompts.text(session, "What is your name?");
```

### <a name="promptsconfirm"></a><span data-ttu-id="8a90f-140">Prompts.confirm</span><span class="sxs-lookup"><span data-stu-id="8a90f-140">Prompts.confirm</span></span>

<span data-ttu-id="8a90f-141">Use el método [Prompts.confirm()][PromptsConfirm] para pedir al usuario que confirme una acción con una respuesta **sí o no**.</span><span class="sxs-lookup"><span data-stu-id="8a90f-141">Use the [Prompts.confirm()][PromptsConfirm] method to ask the user to confirm an action with a **yes/no** response.</span></span> <span data-ttu-id="8a90f-142">El aviso devuelve la respuesta del usuario como [IPromptConfirmResult](http://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.ipromptconfirmresult.html).</span><span class="sxs-lookup"><span data-stu-id="8a90f-142">The prompt returns the user's response as an [IPromptConfirmResult](http://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.ipromptconfirmresult.html).</span></span>

```javascript
builder.Prompts.confirm(session, "Are you sure you wish to cancel your order?");
```

### <a name="promptsnumber"></a><span data-ttu-id="8a90f-143">Prompts.number</span><span class="sxs-lookup"><span data-stu-id="8a90f-143">Prompts.number</span></span>

<span data-ttu-id="8a90f-144">Use el método [Prompts.number()][PromptsNumber] para pedir al usuario un **número**.</span><span class="sxs-lookup"><span data-stu-id="8a90f-144">Use the [Prompts.number()][PromptsNumber] method to ask the user for a **number**.</span></span> <span data-ttu-id="8a90f-145">El aviso devuelve la respuesta del usuario como [IPromptNumberResult](http://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.ipromptnumberresult.html).</span><span class="sxs-lookup"><span data-stu-id="8a90f-145">The prompt returns the user's response as an [IPromptNumberResult](http://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.ipromptnumberresult.html).</span></span>

```javascript
builder.Prompts.number(session, "How many would you like to order?");
```

### <a name="promptstime"></a><span data-ttu-id="8a90f-146">Prompts.time</span><span class="sxs-lookup"><span data-stu-id="8a90f-146">Prompts.time</span></span>

<span data-ttu-id="8a90f-147">Use el método [Prompts.time()][PromptsTime] para pedir al usuario una **hora** o una **fecha y hora**.</span><span class="sxs-lookup"><span data-stu-id="8a90f-147">Use the [Prompts.time()][PromptsTime] method to ask the user for a **time** or **date/time**.</span></span> <span data-ttu-id="8a90f-148">El aviso devuelve la respuesta del usuario como [IPromptTimeResult](http://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.iprompttimeresult.html).</span><span class="sxs-lookup"><span data-stu-id="8a90f-148">The prompt returns the user's response as an [IPromptTimeResult](http://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.iprompttimeresult.html).</span></span> <span data-ttu-id="8a90f-149">El marco usa la biblioteca [Chrono](https://github.com/wanasit/chrono) para analizar la respuesta del usuario y admite tanto respuestas relativas (por ejemplo, "en 5 minutos)" como no relativas (p. ej., "6 de junio a las 2 de la tarde").</span><span class="sxs-lookup"><span data-stu-id="8a90f-149">The framework uses the [Chrono](https://github.com/wanasit/chrono) library to parse the user's response and supports both relative responses (e.g., "in 5 minutes") and non-relative responses (e.g., "June 6th at 2pm").</span></span>

<span data-ttu-id="8a90f-150">El campo [results.response](http://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.iprompttimeresult.html#response), que representa la respuesta del usuario, contiene un objeto [entity](http://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.ientity.html) que especifica la fecha y la hora.</span><span class="sxs-lookup"><span data-stu-id="8a90f-150">The [results.response](http://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.iprompttimeresult.html#response) field, which represents the user's response, contains an [entity](http://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.ientity.html) object that specifies the date and time.</span></span> <span data-ttu-id="8a90f-151">Para resolver la fecha y hora en un objeto `Date` de JavaScript, use el método [EntityRecognizer.resolveTime()](http://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.entityrecognizer.html#resolvetime).</span><span class="sxs-lookup"><span data-stu-id="8a90f-151">To resolve the date and time into a JavaScript `Date` object, use the [EntityRecognizer.resolveTime()](http://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.entityrecognizer.html#resolvetime) method.</span></span>

> [!TIP] 
> <span data-ttu-id="8a90f-152">La hora que especifica el usuario se convierte a la hora UTC en función de la zona horaria del servidor que hospeda el bot.</span><span class="sxs-lookup"><span data-stu-id="8a90f-152">The time that the user enters is converted to UTC time based upon the time zone of the server that hosts the bot.</span></span> <span data-ttu-id="8a90f-153">Como el servidor puede estar ubicado en una zona horaria diferente a la del usuario, asegúrese de tener en cuenta las zonas horarias.</span><span class="sxs-lookup"><span data-stu-id="8a90f-153">Since the server may be located in a different time zone than the user, be sure to take time zones into consideration.</span></span> <span data-ttu-id="8a90f-154">Para convertir la fecha y hora a la hora local del usuario, considere la posibilidad de pedir al usuario que indique en qué zona horaria se encuentra.</span><span class="sxs-lookup"><span data-stu-id="8a90f-154">To convert date and time to the user's local time, consider asking the user what time zone they are in.</span></span>

```javascript
bot.dialog('createAlarm', [
    function (session) {
        session.dialogData.alarm = {};
        builder.Prompts.text(session, "What would you like to name this alarm?");
    },
    function (session, results, next) {
        if (results.response) {
            session.dialogData.name = results.response;
            builder.Prompts.time(session, "What time would you like to set an alarm for?");
        } else {
            next();
        }
    },
    function (session, results) {
        if (results.response) {
            session.dialogData.time = builder.EntityRecognizer.resolveTime([results.response]);
        }

        // Return alarm to caller  
        if (session.dialogData.name && session.dialogData.time) {
            session.endDialogWithResult({ 
                response: { name: session.dialogData.name, time: session.dialogData.time } 
            }); 
        } else {
            session.endDialogWithResult({
                resumed: builder.ResumeReason.notCompleted
            });
        }
    }
]);
```

### <a name="promptschoice"></a><span data-ttu-id="8a90f-155">Prompts.choice</span><span class="sxs-lookup"><span data-stu-id="8a90f-155">Prompts.choice</span></span>

<span data-ttu-id="8a90f-156">Use el método [Prompts.choice()][PromptsChoice] para pedir al usuario que **elija de una lista de opciones**.</span><span class="sxs-lookup"><span data-stu-id="8a90f-156">Use the [Prompts.choice()][PromptsChoice] method to ask the user to **choose from a list of options**.</span></span> <span data-ttu-id="8a90f-157">Para transmitir su selección, el usuario puede escribir el número asociado con la opción que haya elegido o escribir el nombre de la opción que elija.</span><span class="sxs-lookup"><span data-stu-id="8a90f-157">The user can convey their selection either by entering the number associated with the option that they choose or by entering the name of the option that they choose.</span></span> <span data-ttu-id="8a90f-158">Se admiten coincidencias totales y parciales del nombre de la opción.</span><span class="sxs-lookup"><span data-stu-id="8a90f-158">Both full and partial matches of the option's name are supported.</span></span> <span data-ttu-id="8a90f-159">El aviso devuelve la respuesta del usuario como [IPromptChoiceResult](http://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.ipromptchoiceresult.html).</span><span class="sxs-lookup"><span data-stu-id="8a90f-159">The prompt returns the user's response as an [IPromptChoiceResult](http://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.ipromptchoiceresult.html).</span></span> 

<span data-ttu-id="8a90f-160">Para especificar el estilo de la lista que se presenta al usuario, establezca la propiedad [IPromptOptions.listStyle](http://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.ipromptoptions.html#liststyle).</span><span class="sxs-lookup"><span data-stu-id="8a90f-160">To specify the style of the list that is presented to the user, set the [IPromptOptions.listStyle](http://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.ipromptoptions.html#liststyle) property.</span></span> <span data-ttu-id="8a90f-161">En la tabla siguiente se muestran los valores de enumeración `ListStyle` de esta propiedad.</span><span class="sxs-lookup"><span data-stu-id="8a90f-161">The following table shows the `ListStyle` enumeration values for this property.</span></span>


<span data-ttu-id="8a90f-162">Los valores de enumeración `ListStyle` son los siguientes:</span><span class="sxs-lookup"><span data-stu-id="8a90f-162">The `ListStyle` enum values are as follows:</span></span>

| <span data-ttu-id="8a90f-163">Índice</span><span class="sxs-lookup"><span data-stu-id="8a90f-163">Index</span></span> | <span data-ttu-id="8a90f-164">NOMBRE</span><span class="sxs-lookup"><span data-stu-id="8a90f-164">Name</span></span> | <span data-ttu-id="8a90f-165">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="8a90f-165">Description</span></span> |
| ---- | ---- | ---- |
| <span data-ttu-id="8a90f-166">0</span><span class="sxs-lookup"><span data-stu-id="8a90f-166">0</span></span> | <span data-ttu-id="8a90f-167">None</span><span class="sxs-lookup"><span data-stu-id="8a90f-167">none</span></span> | <span data-ttu-id="8a90f-168">No se representa ninguna lista.</span><span class="sxs-lookup"><span data-stu-id="8a90f-168">No list is rendered.</span></span> <span data-ttu-id="8a90f-169">Se usa cuando la lista se incluye como parte del aviso.</span><span class="sxs-lookup"><span data-stu-id="8a90f-169">This is used when the list is included as part of the prompt.</span></span> |
| <span data-ttu-id="8a90f-170">1</span><span class="sxs-lookup"><span data-stu-id="8a90f-170">1</span></span> | <span data-ttu-id="8a90f-171">inline</span><span class="sxs-lookup"><span data-stu-id="8a90f-171">inline</span></span> | <span data-ttu-id="8a90f-172">Las opciones se representan como una lista alineada del formulario "1.</span><span class="sxs-lookup"><span data-stu-id="8a90f-172">Choices are rendered as an inline list of the form "1.</span></span> <span data-ttu-id="8a90f-173">rojo, 2.</span><span class="sxs-lookup"><span data-stu-id="8a90f-173">red, 2.</span></span> <span data-ttu-id="8a90f-174">verde, o 3.</span><span class="sxs-lookup"><span data-stu-id="8a90f-174">green, or 3.</span></span> <span data-ttu-id="8a90f-175">azul".</span><span class="sxs-lookup"><span data-stu-id="8a90f-175">blue".</span></span> |
| <span data-ttu-id="8a90f-176">2</span><span class="sxs-lookup"><span data-stu-id="8a90f-176">2</span></span> | <span data-ttu-id="8a90f-177">list</span><span class="sxs-lookup"><span data-stu-id="8a90f-177">list</span></span> | <span data-ttu-id="8a90f-178">Las opciones se representan como una lista numerada.</span><span class="sxs-lookup"><span data-stu-id="8a90f-178">Choices are rendered as a numbered list.</span></span> |
| <span data-ttu-id="8a90f-179">3</span><span class="sxs-lookup"><span data-stu-id="8a90f-179">3</span></span> | <span data-ttu-id="8a90f-180">button</span><span class="sxs-lookup"><span data-stu-id="8a90f-180">button</span></span> | <span data-ttu-id="8a90f-181">Las opciones se representan como botones para canales que admiten botones.</span><span class="sxs-lookup"><span data-stu-id="8a90f-181">Choices are rendered as buttons for channels that support buttons.</span></span> <span data-ttu-id="8a90f-182">Para otros canales se representan como texto.</span><span class="sxs-lookup"><span data-stu-id="8a90f-182">For other channels they will be rendered as text.</span></span> |
| <span data-ttu-id="8a90f-183">4</span><span class="sxs-lookup"><span data-stu-id="8a90f-183">4</span></span> | <span data-ttu-id="8a90f-184">auto</span><span class="sxs-lookup"><span data-stu-id="8a90f-184">auto</span></span> | <span data-ttu-id="8a90f-185">El estilo se selecciona automáticamente según el canal y el número de opciones.</span><span class="sxs-lookup"><span data-stu-id="8a90f-185">The style is selected automatically based on the channel and number of options.</span></span> | 

<span data-ttu-id="8a90f-186">Puede acceder a esta enumeración desde el objeto `builder`, o bien puede proporcionar un índice para elegir un elemento `ListStyle`.</span><span class="sxs-lookup"><span data-stu-id="8a90f-186">You can access this enum from the `builder` object or you can provide an index to choose a `ListStyle`.</span></span> <span data-ttu-id="8a90f-187">Por ejemplo, ambas instrucciones del siguiente fragmento de código realizan la misma cosa.</span><span class="sxs-lookup"><span data-stu-id="8a90f-187">For example, both statements in the following code snippet accomplish the same thing.</span></span>

```javascript
// ListStyle passed in as Enum
builder.Prompts.choice(session, "Which color?", "red|green|blue", { listStyle: builder.ListStyle.button });

// ListStyle passed in as index
builder.Prompts.choice(session, "Which color?", "red|green|blue", { listStyle: 3 });
```

<span data-ttu-id="8a90f-188">Para especificar la lista de opciones, puede usar una cadena delimitada por barras verticales (`|`), una matriz de cadenas o un mapa de objetos.</span><span class="sxs-lookup"><span data-stu-id="8a90f-188">To specify the list of options, you can use a pipe-delimited (`|`) string, an array of strings, or an object map.</span></span>

<span data-ttu-id="8a90f-189">Una cadena delimitada por barras verticales:</span><span class="sxs-lookup"><span data-stu-id="8a90f-189">A pipe-delimited string:</span></span> 

```javascript
builder.Prompts.choice(session, "Which color?", "red|green|blue");
```

<span data-ttu-id="8a90f-190">Una matriz de cadenas:</span><span class="sxs-lookup"><span data-stu-id="8a90f-190">An array of strings:</span></span>

```javascript
builder.Prompts.choice(session, "Which color?", ["red","green","blue"]);
```

<span data-ttu-id="8a90f-191">Un mapa de objetos:</span><span class="sxs-lookup"><span data-stu-id="8a90f-191">An object map:</span></span> 

```javascript
var salesData = {
    "west": {
        units: 200,
        total: "$6,000"
    },
    "central": {
        units: 100,
        total: "$3,000"
    },
    "east": {
        units: 300,
        total: "$9,000"
    }
};

bot.dialog('getSalesData', [
    function (session) {
        builder.Prompts.choice(session, "Which region would you like sales for?", salesData); 
    },
    function (session, results) {
        if (results.response) {
            var region = salesData[results.response.entity];
            session.send(`We sold ${region.units} units for a total of ${region.total}.`); 
        } else {
            session.send("OK");
        }
    }
]);
```

### <a name="promptsattachment"></a><span data-ttu-id="8a90f-192">Prompts.attachment</span><span class="sxs-lookup"><span data-stu-id="8a90f-192">Prompts.attachment</span></span>

<span data-ttu-id="8a90f-193">Use el método [Prompts.attachment()][PromptsAttachment] para pedir al usuario que cargue un archivo, como una imagen o un vídeo.</span><span class="sxs-lookup"><span data-stu-id="8a90f-193">Use the [Prompts.attachment()][PromptsAttachment] method to ask the user to upload a file such an image or video.</span></span> <span data-ttu-id="8a90f-194">El aviso devuelve la respuesta del usuario como [IPromptAttachmentResult](http://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.ipromptattachmentresult.html).</span><span class="sxs-lookup"><span data-stu-id="8a90f-194">The prompt returns the user's response as an [IPromptAttachmentResult](http://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.ipromptattachmentresult.html).</span></span>

```javascript
builder.Prompts.attachment(session, "Upload a picture for me to transform.");
```

## <a name="next-steps"></a><span data-ttu-id="8a90f-195">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="8a90f-195">Next steps</span></span>

<span data-ttu-id="8a90f-196">Ahora que sabe cómo dirigir a los usuarios por los pasos de una cascada y pedirles información, vamos a ver algunas formas de ayudarle a administrar mejor el flujo de conversación.</span><span class="sxs-lookup"><span data-stu-id="8a90f-196">Now that you know how to step users through a waterfall and prompt them for information, lets take a look at ways to help you better manage the conversation flow.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="8a90f-197">Administración de flujo de conversación</span><span class="sxs-lookup"><span data-stu-id="8a90f-197">Manage conversation flow</span></span>](bot-builder-nodejs-dialog-manage-conversation-flow.md)


[SendAttachments]: bot-builder-nodejs-send-receive-attachments.md
[SendCardWithButtons]: bot-builder-nodejs-send-rich-cards.md
[RecognizeUserIntent]: bot-builder-nodejs-recognize-intent-messages.md
[SaveUserData]: bot-builder-nodejs-save-user-data.md

[UniversalBot]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.universalbot.html
[ChatConnector]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.chatconnector.html
[Session]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.session


[SendTyping]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.session#sendtyping

[EndDialogWithResult]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.session.html#enddialogwithresult

[IPromptResult]: https://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.ipromptresult.html

[Result_Response]: https://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.ipromptresult.html#reponse

[ResumeReason]: https://docs.botframework.com/en-us/node/builder/chat-reference/enums/_botbuilder_d_.resumereason.html

[Result_Resumed]: https://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.ipromptresult.html#resumed

[entity]: https://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.ientity.html

[ResolveTime]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.entityrecognizer.html#resolvetime

[PromptsRef]: https://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.__global.iprompts.html

[PromptsText]: https://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.__global.iprompts.html#text

[IPromptTextResult]: https://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.iprompttextresult.html

[PromptsConfirm]: https://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.__global.iprompts.html#confirm

[IPromptConfirmResult]: https://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.ipromptconfirmresult.html

[PromptsNumber]: https://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.__global.iprompts.html#number

[IPromptNumberResult]: https://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.ipromptnumberresult.html

[PromptsTime]: https://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.__global.iprompts.html#time

[IPromptTimeResult]: https://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.iprompttimeresult.html

[PromptsChoice]: https://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.__global.iprompts.html#choice

[IPromptChoiceResult]: https://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.ipromptchoiceresult.html

[PromptsAttachment]: https://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.__global.iprompts.html#attachment

[IPromptAttachmentResult]: https://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.ipromptattachmentresult.html
