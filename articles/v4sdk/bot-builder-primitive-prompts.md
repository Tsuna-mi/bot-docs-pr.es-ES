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
# <a name="prompt-users-for-input-using-your-own-prompts"></a>Petición de datos de entrada a los usuarios mediante sus propios mensajes
[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]

A menudo, una conversación entre un usuario y un bot implica mostrar preguntas (avisos) al usuario para obtener información, analizar la respuesta del usuario y, a continuación, actuar en dicha información. En el tema sobre cómo [preguntar a los usuarios mediante la biblioteca Dialogs](bot-builder-prompts.md), se detalla cómo pedir datos de entrada a los usuarios mediante la biblioteca **Dialogs**. Entre otras cosas, la biblioteca **Dialogs** se encarga del seguimiento de la conversación actual y de la pregunta que se está planteando en cada momento. También proporciona métodos para solicitar diferentes tipos de información como texto, números, fecha y hora, etc. 

En determinadas situaciones, puede que la biblioteca integrada **Dialogs** no sea ser la solución adecuada para su bot; **Dialogs** podría agregar demasiada sobrecarga para bots simples, ser demasiado rígida o, por el contrario, no conseguir lo que su bot necesita hacer. En esos casos, puede omitir la biblioteca e implementar su propia lógica de preguntas. En este tema se mostrará cómo configurar un *bot Echo* básico para que pueda administrar la conversación con sus propios avisos.

## <a name="track-prompt-states"></a>Seguimiento del estado de los avisos

En una conversación controlada por avisos, necesita controlar dónde está en la conversación y qué pregunta se está haciendo en cada momento. En el código, esto se traduce en administrar un par de marcas. 

Por ejemplo, puede crear un par de propiedades que desea controlar. 

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

Aquí tenemos una clase de perfil de usuario anidada en la información de nuestro aviso, lo que permite almacenar todo esto en la propiedad [state](bot-builder-howto-v4-state.md) de nuestro bot.

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

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

Estos estados simplemente indican qué tema y aviso están actualmente activados. Para asegurarse de que estas marcas funcionarán según lo esperado cuando se implementen en la nube, vamos a almacenarlas en el [estado de la conversación](bot-builder-howto-v4-state.md). Coloque este código dentro de la lógica principal del bot.

**app.js**
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

## <a name="manage-a-topic-of-conversation"></a>Administración de un tema de conversación

En una conversación secuencial, como aquellas en las que se reúne información del usuario, necesita saber cuándo ha escrito el usuario la conversación y en qué parte de ella está. Puede realizar un seguimiento de esto en el controlador de turnos del bot principal. Para ello, puede establecer y comprobar las marcas del aviso definidas anteriormente y, a continuación, actuar en consecuencia. El ejemplo siguiente muestra cómo puede usar estas marcas para recopilar información del perfil del usuario a través de la conversación.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

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

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

**app.js**
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

El código de ejemplo anterior comprueba si un perfil de usuario está definido en memoria. Si no lo está y se trata de un nuevo usuario, establezca la marca para iniciar ese tema automáticamente. A continuación, se muestra cómo puede avanzar en la conversación al establecer la marca `prompt` en el valor de la pregunta actual. Con esta marca establecida en el valor apropiado, la cláusula condicional detectará la respuesta del usuario en cada turno y la procesará en consecuencia. 

Por último, debe restablecer estas marcas cuando termine de pedir información para que el bot no entre en un bucle. Puede extender esta configuración básica para administrar flujos de conversación más complejos, ya que el bot requiere simplemente definir otras marcas o bifurcar la conversación en función de la entrada del usuario.

## <a name="manage-multiple-topics-of-conversations"></a>Administración de varios temas de conversación

Una vez que es capaz de controlar un tema de conversación, puede ampliar la lógica del bot para controlar varios temas de conversación. Al igual que un solo tema de conversación, para controlar varios temas basta con comprobar las condiciones que desencadenan un tema determinado y, a continuación, tomar la ruta adecuada.

Puede refactorizar nuestro ejemplo anterior para permitir otras funciones y temas, como la reserva de una mesa o pedir una cena. Se puede agregar información adicional al perfil de usuario o marcas de estado del tema según sea necesario para realizar un seguimiento de la conversación.

Para ayudar a administrar mejor varios temas de conversación, un método podría ser proporcionar un menú principal. Mediante el uso de [acciones sugeridas](bot-builder-howto-add-suggested-actions.md) puede dar al usuario la opción de qué tema elegir y, a continuación, profundizar en ese tema. También resultará útil dividir el código en funciones independientes, lo que facilita seguir el flujo de la conversación.

## <a name="validate-user-input"></a>Validación de entradas de usuario

La biblioteca **Dialogs** proporciona formas integradas para validar la entrada del usuario, pero también podemos hacerlo con nuestros propios avisos. Por ejemplo, si se solicita la edad del usuario, deseamos asegurarnos de que se obtiene un número y no algo como "Bob" como respuesta.

El análisis de un número o de una fecha y hora es una tarea compleja que queda fuera del ámbito de este tema. Afortunadamente, hay una biblioteca que podemos aprovechar. Para analizar correctamente esta información, usamos la biblioteca [Text Recognizer de Microsoft](https://github.com/Microsoft/Recognizers-Text). Este paquete está disponible mediante NuGet o también puede descargarlo desde el repositorio. Además, se incluye en la biblioteca **Dialogs**, que vale la pena mencionar, aunque no la vamos a usar aquí.

Esta biblioteca es especialmente útil para el análisis de lenguaje común o respuestas complejas acerca de cuestiones como la fecha, la hora o números de teléfono. En este ejemplo, solo se valida un número para el tamaño de la reserva de una cena, pero la misma idea se puede extender para validaciones mucho más en profundidad. Si no está familiarizado con esta biblioteca, revise el contenido que se encuentra en el sitio para ver lo que tiene que ofrecer.

En este ejemplo solo se muestra el uso de `RecognizeNumber`. Encontrará detalles sobre cómo usar otros métodos de Recognizer de la biblioteca en la [documentación del repositorio](https://github.com/Microsoft/Recognizers-Text/blob/master/README.md).

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

Para usar la biblioteca Recognizers, agréguela a las instrucciones using.

```csharp
using Microsoft.Recognizers.Text.Number;
using Microsoft.Recognizers.Text;
using System.Linq; // Used to get the first result from the recognizer
```

A continuación, defina una función que realmente realiza la validación.

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

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

Para usar la biblioteca Recognizers, debe incluirla en una instrucción require en **app.js**:

```javascript
// Required packages for this bot
var Recognizers = require('@microsoft/recognizers-text-suite');
```

A continuación, defina una función que realmente realiza la validación.

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

En el paso del aviso que se va a validar, llame a la función de validación antes de continuar con el siguiente aviso. Si se produce un error en la validación, formule la pregunta de nuevo.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

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

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

**app.js**

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

## <a name="next-step"></a>Paso siguiente

Ahora que conoce cómo administrar los estados del aviso, echemos un vistazo a cómo aprovechar la biblioteca **Dialogs** para pedir datos de entrada a los usuarios.

> [!div class="nextstepaction"]
> [Petición de datos de entrada a los usuarios mediante el uso de Dialogs](bot-builder-prompts.md)
