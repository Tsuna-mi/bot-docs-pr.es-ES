---
title: Administración de un flujo de conversación con avisos primitivos | Microsoft Docs
description: Aprenda a administrar un flujo de conversación con avisos primitivos en el SDK de Bot Builder.
keywords: flujo de conversación, avisos
author: v-ducvo
ms.author: v-ducvo
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 7/20/2018
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: 1514e1bcafc87be9e8449382bca7aa156e512ed9
ms.sourcegitcommit: e8c513d3af5f0c514cadcbcd0a737a7393405afa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/20/2018
ms.locfileid: "40228354"
---
# <a name="prompt-users-for-input-using-your-own-prompts"></a><span data-ttu-id="cd37c-104">Petición de datos de entrada a los usuarios mediante sus propios mensajes</span><span class="sxs-lookup"><span data-stu-id="cd37c-104">Prompt users for input using your own prompts</span></span>
[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]

<span data-ttu-id="cd37c-105">A menudo, una conversación entre un usuario y un bot implica mostrar preguntas (avisos) al usuario para obtener información, analizar la respuesta del usuario y, a continuación, actuar en dicha información.</span><span class="sxs-lookup"><span data-stu-id="cd37c-105">A conversation between a bot and a user often involves asking (prompting) the user for information, parsing the user's response, and then acting on that information.</span></span> <span data-ttu-id="cd37c-106">En el tema sobre cómo [preguntar a los usuarios mediante la biblioteca Dialogs](bot-builder-prompts.md), se detalla cómo pedir datos de entrada a los usuarios mediante la biblioteca **Dialogs**.</span><span class="sxs-lookup"><span data-stu-id="cd37c-106">In the topic about [prompting users using the Dialogs library](bot-builder-prompts.md), it details how to prompt users for input using the **Dialogs** library.</span></span> <span data-ttu-id="cd37c-107">Entre otras cosas, la biblioteca **Dialogs** se encarga del seguimiento de la conversación actual y de la pregunta que se está planteando en cada momento.</span><span class="sxs-lookup"><span data-stu-id="cd37c-107">Among other things, the **Dialogs** library takes care of tracking the current conversation and the current question being asked.</span></span> <span data-ttu-id="cd37c-108">También proporciona métodos para solicitar diferentes tipos de información como texto, números, fecha y hora, etc.</span><span class="sxs-lookup"><span data-stu-id="cd37c-108">It also provides methods for requesting different types of information such as text, number, date and time, and so on.</span></span> 

<span data-ttu-id="cd37c-109">En determinadas situaciones, puede que la biblioteca integrada **Dialogs** no sea ser la solución adecuada para su bot; **Dialogs** podría agregar demasiada sobrecarga para bots simples, ser demasiado rígida o, por el contrario, no conseguir lo que su bot necesita hacer.</span><span class="sxs-lookup"><span data-stu-id="cd37c-109">In certain situations, the built in **Dialogs** may not be the right solution for your bot; **Dialogs** might add too much overhead for simple bots, be too rigid, or otherwise not achieve what you need your bot to do.</span></span> <span data-ttu-id="cd37c-110">En esos casos, puede omitir la biblioteca e implementar su propia lógica de preguntas.</span><span class="sxs-lookup"><span data-stu-id="cd37c-110">In those cases, you can skip the library and implement your own prompting logic.</span></span> <span data-ttu-id="cd37c-111">En este tema se mostrará cómo configurar un *bot Echo* básico para que pueda administrar la conversación con sus propios avisos.</span><span class="sxs-lookup"><span data-stu-id="cd37c-111">This topic will show you how to setup your basic *Echo bot* so that you can manage conversation using your own prompts.</span></span>

## <a name="track-prompt-states"></a><span data-ttu-id="cd37c-112">Seguimiento del estado de los avisos</span><span class="sxs-lookup"><span data-stu-id="cd37c-112">Track prompt states</span></span>

<span data-ttu-id="cd37c-113">En una conversación controlada por avisos, necesita controlar dónde está en la conversación y qué pregunta se está haciendo en cada momento.</span><span class="sxs-lookup"><span data-stu-id="cd37c-113">In a prompt-driven conversation, you need to track where in the conversation you currently are, and what question is currently being asked.</span></span> <span data-ttu-id="cd37c-114">En el código, esto se traduce en administrar un par de marcas.</span><span class="sxs-lookup"><span data-stu-id="cd37c-114">In code, this translates to managing a couple of flags.</span></span> 

<span data-ttu-id="cd37c-115">Por ejemplo, puede crear un par de propiedades que desea controlar.</span><span class="sxs-lookup"><span data-stu-id="cd37c-115">For example, you can create a couple of properties you want to track.</span></span> 

# <a name="ctabcsharp"></a>[<span data-ttu-id="cd37c-116">C#</span><span class="sxs-lookup"><span data-stu-id="cd37c-116">C#</span></span>](#tab/csharp)

<span data-ttu-id="cd37c-117">Aquí tenemos una clase de perfil de usuario anidada en la información de nuestro aviso, lo que permite almacenar todo esto en la propiedad [state](bot-builder-howto-v4-state.md) de nuestro bot.</span><span class="sxs-lookup"><span data-stu-id="cd37c-117">Here we have a user profile class nested in our prompt information, allowing all of these to be stored in our bot [state](bot-builder-howto-v4-state.md) together.</span></span>

```csharp
// Where user information will be stored
public class UserProfile
{
    public string userName = null;
    public string workPlace = null;
    public int age = 0;
}

// Flags to keep track of our prompts, and the 
// user profile object for this conversation
public class PromptInformation
{
    public string topicTitle = null;
    public string prompt = null;
    public UserProfile userProfile = null;
}
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="cd37c-118">JavaScript</span><span class="sxs-lookup"><span data-stu-id="cd37c-118">JavaScript</span></span>](#tab/javascript)

<span data-ttu-id="cd37c-119">Estos estados simplemente indican qué tema y aviso están actualmente activados.</span><span class="sxs-lookup"><span data-stu-id="cd37c-119">These states just keep track of which topic and which prompt we are currently on.</span></span> <span data-ttu-id="cd37c-120">Para asegurarse de que estas marcas funcionarán según lo esperado cuando se implementen en la nube, vamos a almacenarlas en el [estado de la conversación](bot-builder-howto-v4-state.md).</span><span class="sxs-lookup"><span data-stu-id="cd37c-120">To ensure these flags function as expected when deployed to the cloud, we store them in the [conversation state](bot-builder-howto-v4-state.md).</span></span> <span data-ttu-id="cd37c-121">Coloque este código dentro de la lógica principal del bot.</span><span class="sxs-lookup"><span data-stu-id="cd37c-121">Place this code within your main bot logic.</span></span>

<span data-ttu-id="cd37c-122">**app.js**</span><span class="sxs-lookup"><span data-stu-id="cd37c-122">**app.js**</span></span>
```javascript
// Define a topicStates object if it doesn't exist in the convoState.
if(!convo.topicStates){
    convo.topicStates = {
        "topicTitle": undefined, // Current conversation topic in progress
        "prompt": undefined      // Current prompt state in progress - indicate what question is being asked.
    };
}
```

---

## <a name="manage-a-topic-of-conversation"></a><span data-ttu-id="cd37c-123">Administración de un tema de conversación</span><span class="sxs-lookup"><span data-stu-id="cd37c-123">Manage a topic of conversation</span></span>

<span data-ttu-id="cd37c-124">En una conversación secuencial, como aquellas en las que se reúne información del usuario, necesita saber cuándo ha escrito el usuario la conversación y en qué parte de ella está.</span><span class="sxs-lookup"><span data-stu-id="cd37c-124">In a sequential conversation, such as one where you gather information from the user, you need to know when the user has entered the conversation and where in the conversation the user is.</span></span> <span data-ttu-id="cd37c-125">Puede realizar un seguimiento de esto en el controlador de turnos del bot principal. Para ello, puede establecer y comprobar las marcas del aviso definidas anteriormente y, a continuación, actuar en consecuencia.</span><span class="sxs-lookup"><span data-stu-id="cd37c-125">You can track this in the main bot turn handler by setting and checking the prompt flags defined above, then acting accordingly.</span></span> <span data-ttu-id="cd37c-126">El ejemplo siguiente muestra cómo puede usar estas marcas para recopilar información del perfil del usuario a través de la conversación.</span><span class="sxs-lookup"><span data-stu-id="cd37c-126">The sample below shows how you can use these flags to gather the user's profile information through the conversation.</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="cd37c-127">C#</span><span class="sxs-lookup"><span data-stu-id="cd37c-127">C#</span></span>](#tab/csharp)

```csharp
// Get our current state, as defined above
var convoState = context.GetConversationState<PromptInformation>();

if (convoState.userProfile == null)
{
    await context.SendActivity("Welcome new user, please fill out your profile information.");
    convoState.topicTitle = "profileTopic"; // Start the userProfile topic
    convoState.userProfile = new UserProfile();
}

// Start or continue a conversation if we are in one
if (convoState.topicTitle == "profileTopic")
{
    if (convoState.userProfile.userName == null && convoState.prompt == null)
    {
        convoState.prompt = "askName";
        await context.SendActivity("What is your name?");
    }
    else if (convoState.prompt == "askName")
    {
        // Save the user's response
        convoState.userProfile.userName = context.Activity.Text;

        // Ask next question
        convoState.prompt = "askAge";
        await context.SendActivity("How old are you?");
    }
    else if (convoState.prompt == "askAge")
    {
        // Save user's response
        if (!Int32.TryParse(context.Activity.Text, out convoState.userProfile.age))
        {
            // Failed to convert to int
            await context.SendActivity("Failed to convert string to int");
        }
        else
        {
            // Ask next question
            convoState.prompt = "workPlace";
            await context.SendActivity("Where do you work?");
        }

    }
    else if (convoState.prompt == "workPlace")
    {
        // Save user's response
        convoState.userProfile.workPlace = context.Activity.Text;

        // Done
        convoState.topicTitle = null; // Reset conversation flag
        convoState.prompt = null;     // Reset the prompt flag
        
        await context.SendActivity("Thank you. Your profile is complete.");
    }
    else // Somehow our flags got inappropriately set
    {
        await context.SendActivity("Looks like something went wrong, let's start over");
        convoState.userProfile = null;
        convoState.prompt = null;
        convoState.topicTitle = null;
    }
}
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="cd37c-128">JavaScript</span><span class="sxs-lookup"><span data-stu-id="cd37c-128">JavaScript</span></span>](#tab/javascript)

<span data-ttu-id="cd37c-129">**app.js**</span><span class="sxs-lookup"><span data-stu-id="cd37c-129">**app.js**</span></span>
```javascript
server.post('/api/messages', (req, res) => {
    adapter.processActivity(req, res, async (context) => {
        const isMessage = (context.activity.type === 'message');
        // State will store all of your information 
        const convo = conversationState.get(context);

        // Defined flags to manage the conversation flow and prompt states
        // convo.topicStates.topicTitle - Current conversation topic in progress
        // convo.topicStates.prompt - Current prompt state in progress - indicate what question is being asked.
        
        if (isMessage) {
            // Define a topicStates object if it doesn't exist in the convoState.
            if(!convo.topicStates){
                convo.topicStates = {
                    "topicTitle": undefined, // Current conversation topic in progress
                    "prompt": undefined      // Current prompt state in progress - indicate what question is being asked.
                };
            }
            
            // If user profile is not defined then define it.
            if(!convo.userProfile){
                
                await context.sendActivity(`Welcome new user, please fill out your profile information.`);
                convo.topicStates.topicTitle = "profileTopic"; // Start the userProfile topic
                convo.userProfile = { // Define the user's profile object
                    "userName": undefined,
                    "age": undefined,
                    "workPlace": undefined
                }; 
            }

            // Start or continue a conversation if we are in one
            if(convo.topicStates.topicTitle == "profileTopic"){
                if(!convo.userProfile.userName && !convo.topicStates.prompt){
                    convo.topicStates.prompt = "askName";
                    await context.sendActivity("What is your name?");
                }
                else if(convo.topicStates.prompt == "askName"){
                    // Save the user's response
                    convo.userProfile.userName = context.activity.text; 

                    // Ask next question
                    convo.topicStates.prompt = "askAge";
                    await context.sendActivity("How old are you?");
                }
                else if(convo.topicStates.prompt == "askAge"){
                    // Save user's response
                    convo.userProfile.age = context.activity.text;

                    // Ask next question
                    convo.topicStates.prompt = "workPlace";
                    await context.sendActivity("Where do you work?");
                }
                else if(convo.topicStates.prompt == "workPlace"){
                    // Save user's response
                    convo.userProfile.workPlace = context.activity.text;

                    // Done
                    convo.topicStates.topicTitle = undefined; // Reset conversation flag
                    convo.topicStates.prompt = undefined;     // Reset the prompt flag
                    await context.sendActivity("Thank you. Your profile is complete.");
                }
            }

            // Check for valid intents
            else if(context.activity.text && context.activity.text.match(/hi/ig)){
                await context.sendActivity(`Hi ${convo.userProfile.userName}.`);
            }
            else {
                // Default message
                await context.sendActivity("Hi. I'm the Contoso bot.");
            }
        }

    });
});

```

---

<span data-ttu-id="cd37c-130">El código de ejemplo anterior comprueba si un perfil de usuario está definido en memoria.</span><span class="sxs-lookup"><span data-stu-id="cd37c-130">The sample code above checks if a user's profile is defined in memory.</span></span> <span data-ttu-id="cd37c-131">Si no lo está y se trata de un nuevo usuario, establezca la marca para iniciar ese tema automáticamente.</span><span class="sxs-lookup"><span data-stu-id="cd37c-131">If not and this is a new user, then set the flag to start that topic automatically.</span></span> <span data-ttu-id="cd37c-132">A continuación, se muestra cómo puede avanzar en la conversación al establecer la marca `prompt` en el valor de la pregunta actual.</span><span class="sxs-lookup"><span data-stu-id="cd37c-132">Then, it shows how you can move forward through your conversation by setting the `prompt` flag to the value of the current question being asked.</span></span> <span data-ttu-id="cd37c-133">Con esta marca establecida en el valor apropiado, la cláusula condicional detectará la respuesta del usuario en cada turno y la procesará en consecuencia.</span><span class="sxs-lookup"><span data-stu-id="cd37c-133">With this flag set to the proper value, the conditional clause will catch the user's response on each turn and process it accordingly.</span></span> 

<span data-ttu-id="cd37c-134">Por último, debe restablecer estas marcas cuando termine de pedir información para que el bot no entre en un bucle.</span><span class="sxs-lookup"><span data-stu-id="cd37c-134">Lastly, you must reset these flags when you are done asking for information so your bot is not trapped in a loop.</span></span> <span data-ttu-id="cd37c-135">Puede extender esta configuración básica para administrar flujos de conversación más complejos, ya que el bot requiere simplemente definir otras marcas o bifurcar la conversación en función de la entrada del usuario.</span><span class="sxs-lookup"><span data-stu-id="cd37c-135">You can extend this basic setup to manage more complex conversation flows as your bot requires simply by defining other flags or branching the conversation based on user input.</span></span>

## <a name="manage-multiple-topics-of-conversations"></a><span data-ttu-id="cd37c-136">Administración de varios temas de conversación</span><span class="sxs-lookup"><span data-stu-id="cd37c-136">Manage multiple topics of conversations</span></span>

<span data-ttu-id="cd37c-137">Una vez que es capaz de controlar un tema de conversación, puede ampliar la lógica del bot para controlar varios temas de conversación.</span><span class="sxs-lookup"><span data-stu-id="cd37c-137">Once you are able to handle one topic of conversation, you can extend the bot logic to handle multiple topics of conversation.</span></span> <span data-ttu-id="cd37c-138">Al igual que un solo tema de conversación, para controlar varios temas basta con comprobar las condiciones que desencadenan un tema determinado y, a continuación, tomar la ruta adecuada.</span><span class="sxs-lookup"><span data-stu-id="cd37c-138">Just like a single topic of conversation, multiple topics can be handled simply by checking for conditions that trigger a particular topic, and then taking the appropriate path.</span></span>

<span data-ttu-id="cd37c-139">Puede refactorizar nuestro ejemplo anterior para permitir otras funciones y temas, como la reserva de una mesa o pedir una cena.</span><span class="sxs-lookup"><span data-stu-id="cd37c-139">In our example above, you can refactor it to allow for other functions and topics, such as reserving a table or ordering dinner.</span></span> <span data-ttu-id="cd37c-140">Se puede agregar información adicional al perfil de usuario o marcas de estado del tema según sea necesario para realizar un seguimiento de la conversación.</span><span class="sxs-lookup"><span data-stu-id="cd37c-140">Additional information can be added to the user profile or topic state flags as needed to keep track of the conversation.</span></span>

<span data-ttu-id="cd37c-141">Para ayudar a administrar mejor varios temas de conversación, un método podría ser proporcionar un menú principal.</span><span class="sxs-lookup"><span data-stu-id="cd37c-141">To help better manage multiple topics of conversation, one method could be to provide a main menu.</span></span> <span data-ttu-id="cd37c-142">Mediante el uso de [acciones sugeridas](bot-builder-howto-add-suggested-actions.md) puede dar al usuario la opción de qué tema elegir y, a continuación, profundizar en ese tema.</span><span class="sxs-lookup"><span data-stu-id="cd37c-142">Using [suggested actions](bot-builder-howto-add-suggested-actions.md), you can give your user a choice of which topic they could to engage in, and then diving into that topic.</span></span> <span data-ttu-id="cd37c-143">También resultará útil dividir el código en funciones independientes, lo que facilita seguir el flujo de la conversación.</span><span class="sxs-lookup"><span data-stu-id="cd37c-143">You may also find it helpful to split code into independent functions, making it easier to follow the flow of conversation.</span></span>

## <a name="validate-user-input"></a><span data-ttu-id="cd37c-144">Validación de entradas de usuario</span><span class="sxs-lookup"><span data-stu-id="cd37c-144">Validate user input</span></span>

<span data-ttu-id="cd37c-145">La biblioteca **Dialogs** proporciona formas integradas para validar la entrada del usuario, pero también podemos hacerlo con nuestros propios avisos.</span><span class="sxs-lookup"><span data-stu-id="cd37c-145">The **Dialogs** library provides built in ways to validate the user's input, but we can also do so with our own prompts.</span></span> <span data-ttu-id="cd37c-146">Por ejemplo, si se solicita la edad del usuario, deseamos asegurarnos de que se obtiene un número y no algo como "Bob" como respuesta.</span><span class="sxs-lookup"><span data-stu-id="cd37c-146">For example, if we ask for the user's age we want to make sure we get a number, not something like "Bob" as a response.</span></span>

<span data-ttu-id="cd37c-147">El análisis de un número o de una fecha y hora es una tarea compleja que queda fuera del ámbito de este tema.</span><span class="sxs-lookup"><span data-stu-id="cd37c-147">Parsing a number or a date and time is a complex task that's beyond the scope of this topic.</span></span> <span data-ttu-id="cd37c-148">Afortunadamente, hay una biblioteca que podemos aprovechar.</span><span class="sxs-lookup"><span data-stu-id="cd37c-148">Fortunately, there is a library we can leverage.</span></span> <span data-ttu-id="cd37c-149">Para analizar correctamente esta información, usamos la biblioteca [Text Recognizer de Microsoft](https://github.com/Microsoft/Recognizers-Text).</span><span class="sxs-lookup"><span data-stu-id="cd37c-149">To properly parse this information, we use the [Microsoft's Text Recognizer](https://github.com/Microsoft/Recognizers-Text) library.</span></span> <span data-ttu-id="cd37c-150">Este paquete está disponible mediante NuGet o también puede descargarlo desde el repositorio.</span><span class="sxs-lookup"><span data-stu-id="cd37c-150">This package is available through NuGet, or downloading it from the repository.</span></span> <span data-ttu-id="cd37c-151">Además, se incluye en la biblioteca **Dialogs**, que vale la pena mencionar, aunque no la vamos a usar aquí.</span><span class="sxs-lookup"><span data-stu-id="cd37c-151">Additionally, it's included in the **Dialogs** library, which is worth noting but we are not using that library here.</span></span>

<span data-ttu-id="cd37c-152">Esta biblioteca es especialmente útil para el análisis de lenguaje común o respuestas complejas acerca de cuestiones como la fecha, la hora o números de teléfono.</span><span class="sxs-lookup"><span data-stu-id="cd37c-152">This library is particularly useful for parsing common language or complex responses about things like date, time, or phone numbers.</span></span> <span data-ttu-id="cd37c-153">En este ejemplo, solo se valida un número para el tamaño de la reserva de una cena, pero la misma idea se puede extender para validaciones mucho más en profundidad.</span><span class="sxs-lookup"><span data-stu-id="cd37c-153">In this sample, we're only validating a number for a dinner reservation party size, but the same idea can be extended for much more in depth validations.</span></span> <span data-ttu-id="cd37c-154">Si no está familiarizado con esta biblioteca, revise el contenido que se encuentra en el sitio para ver lo que tiene que ofrecer.</span><span class="sxs-lookup"><span data-stu-id="cd37c-154">If you are not familiar with this library, review the content found on that site to see what it has to offer.</span></span>

<span data-ttu-id="cd37c-155">En este ejemplo solo se muestra el uso de `RecognizeNumber`.</span><span class="sxs-lookup"><span data-stu-id="cd37c-155">In this sample we only show the use of `RecognizeNumber`.</span></span> <span data-ttu-id="cd37c-156">Encontrará detalles sobre cómo usar otros métodos de Recognizer de la biblioteca en la [documentación del repositorio](https://github.com/Microsoft/Recognizers-Text/blob/master/README.md).</span><span class="sxs-lookup"><span data-stu-id="cd37c-156">Details on how to use other Recognizer methods from the library can be found in that [repository's documentation](https://github.com/Microsoft/Recognizers-Text/blob/master/README.md).</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="cd37c-157">C#</span><span class="sxs-lookup"><span data-stu-id="cd37c-157">C#</span></span>](#tab/csharp)

<span data-ttu-id="cd37c-158">Para usar la biblioteca Recognizers, agréguela a las instrucciones using.</span><span class="sxs-lookup"><span data-stu-id="cd37c-158">To use the recognizers library, add it to your using statements.</span></span>

```csharp
using Microsoft.Recognizers.Text.Number;
using Microsoft.Recognizers.Text;
using System.Linq; // Used to get the first result from the recognizer
```

<span data-ttu-id="cd37c-159">A continuación, defina una función que realmente realiza la validación.</span><span class="sxs-lookup"><span data-stu-id="cd37c-159">Then, define a function that actually does the validation.</span></span>

```csharp
private async Task<bool> ValidatePartySize(ITurnContext context, string value)
{
    try
    {
        // Recognize the input as a number. This works for responses such as
        // "twelve" as well as "12"
        var result = NumberRecognizer.RecognizeNumber(input, Culture.English);

        // Attempt to convert the Recognizer result to an integer
        Int32.TryParse(result.First().Text, out int partySize);
        
        if (partySize < 6)
        {
            throw new Exception("Party size too small.");
        }
        else if (partySize > 20)
        {
            throw new Exception("Party size too big.");
        }

        // If we got through this, the number is valid
        return true;
    }
    catch (Exception)
    {
        await context.SendActivity("Error with your party size. < br /> Please specify a number between 6 - 20.");
        return false;
    }
}
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="cd37c-160">JavaScript</span><span class="sxs-lookup"><span data-stu-id="cd37c-160">JavaScript</span></span>](#tab/javascript)

<span data-ttu-id="cd37c-161">Para usar la biblioteca Recognizers, debe incluirla en una instrucción require en **app.js**:</span><span class="sxs-lookup"><span data-stu-id="cd37c-161">To use the recognizers library, require it in **app.js**:</span></span>

```javascript
// Required packages for this bot
var Recognizers = require('@microsoft/recognizers-text-suite');
```

<span data-ttu-id="cd37c-162">A continuación, defina una función que realmente realiza la validación.</span><span class="sxs-lookup"><span data-stu-id="cd37c-162">Then, define a function that actually does the validation.</span></span>

```javascript
// Support party size between 6 and 20 only
async function validatePartySize(context, input){
    try {
        // Recognize the input as a number. This works for responses such as
        // "twelve" as well as "12"
        var result = Recognizers.recognizeNumber(input, Recognizers.Culture.English);
        var value = parseInt(results[0].resolution.value);

        if(value < 6) {
            throw new Error(`Party size too small.`);
        }
        else if(value > 20){
            throw new Error(`Party size too big.`);
        }
        return true; // Return the valid value
    }
    catch (err){
        await context.sendActivity(`${err.message} <br/>Please specify a number between 6 - 20.`);
        return false;
    }
}
```

---

<span data-ttu-id="cd37c-163">En el paso del aviso que se va a validar, llame a la función de validación antes de continuar con el siguiente aviso.</span><span class="sxs-lookup"><span data-stu-id="cd37c-163">Within the prompt step that we want to validate, call the validation function before moving on to the next prompt.</span></span> <span data-ttu-id="cd37c-164">Si se produce un error en la validación, formule la pregunta de nuevo.</span><span class="sxs-lookup"><span data-stu-id="cd37c-164">If the validation fails, ask the question again.</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="cd37c-165">C#</span><span class="sxs-lookup"><span data-stu-id="cd37c-165">C#</span></span>](#tab/csharp)

```csharp
if (convoState.prompt == "partySize")
{
    if (await ValidatePartySize(context, context.Activity.Text))
    {
        // Save user's response in our state, ReservationInfo, which 
        // is a new class we've added to our state
        convoState.ReservationInfo.partySize = context.Activity.Text;

        // Ask next question
        convoState.prompt = "reserveName";
        await context.SendActivity("Who's name will this be under?");
    }
    else
    {
        // Ask again
        await context.SendActivity("How many people are in your party?");
    }
}
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="cd37c-166">JavaScript</span><span class="sxs-lookup"><span data-stu-id="cd37c-166">JavaScript</span></span>](#tab/javascript)

<span data-ttu-id="cd37c-167">**app.js**</span><span class="sxs-lookup"><span data-stu-id="cd37c-167">**app.js**</span></span>

```javascript
// ...
if(convo.topicStates.prompt == "partySize"){
    if(await validatePartySize(context, context.activity.text)){
        // Save user's response
        convo.reservationInfo.partySize = context.activity.text;
        
        // Ask next question
        topicStates.prompt = "reserveName";
        await context.sendActivity("Who's name will this be under?");
    }
    else {
        // Ask again
        await context.sendActivity("How many people are in your party?");
    }
}
// ...
```

---

## <a name="next-step"></a><span data-ttu-id="cd37c-168">Paso siguiente</span><span class="sxs-lookup"><span data-stu-id="cd37c-168">Next step</span></span>

<span data-ttu-id="cd37c-169">Ahora que conoce cómo administrar los estados del aviso, echemos un vistazo a cómo aprovechar la biblioteca **Dialogs** para pedir datos de entrada a los usuarios.</span><span class="sxs-lookup"><span data-stu-id="cd37c-169">Now that you have a handle on how to manage the prompt states yourself, let's take a look at how you leverage the **Dialogs** library to prompt users for input.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="cd37c-170">Petición de datos de entrada a los usuarios mediante el uso de Dialogs</span><span class="sxs-lookup"><span data-stu-id="cd37c-170">Prompt users for input using Dialogs</span></span>](bot-builder-prompts.md)
