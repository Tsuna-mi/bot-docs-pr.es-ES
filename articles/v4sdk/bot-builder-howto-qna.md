---
title: Uso de QnA Maker para responder preguntas | Microsoft Docs
description: Obtenga información sobre cómo usar QnA Maker en el bot.
keywords: pregunta y respuesta, QnA, preguntas más frecuentes, software intermedio
author: ivorb
ms.author: v-ivorb
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 03/13/2018
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: 7dd973e2b5a151e754925e6f19c6e4f82507f745
ms.sourcegitcommit: 2dc75701b169d822c9499e393439161bc87639d2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42906179"
---
# <a name="use-qna-maker-to-answer-questions"></a><span data-ttu-id="7ce2e-104">Uso de QnA Maker para responder preguntas</span><span class="sxs-lookup"><span data-stu-id="7ce2e-104">Use QnA Maker to answer questions</span></span>


<span data-ttu-id="7ce2e-105">Para agregar compatibilidad simple de preguntas y respuestas al bot, puede usar el servicio [QnA Maker](https://docs.microsoft.com/en-us/azure/cognitive-services/qnamaker/home).</span><span class="sxs-lookup"><span data-stu-id="7ce2e-105">To add simple question and answer support to your bot, you can use the [QnA Maker](https://docs.microsoft.com/en-us/azure/cognitive-services/qnamaker/home) service.</span></span>


<span data-ttu-id="7ce2e-106">Uno de los requisitos básicos para escribir un servicio de QnA Maker propio es inicializarlo con preguntas y respuestas.</span><span class="sxs-lookup"><span data-stu-id="7ce2e-106">One of the basic requirements in writing your own QnA Maker service is to seed it with questions and answers.</span></span> <span data-ttu-id="7ce2e-107">En muchos casos, las preguntas y respuestas ya existen en el contenido como preguntas más frecuentes u otra documentación.</span><span class="sxs-lookup"><span data-stu-id="7ce2e-107">In many cases, the questions and answers already exist in content like FAQs or other documentation.</span></span> <span data-ttu-id="7ce2e-108">En otras ocasiones, le interesará personalizar las respuestas a las preguntas de forma más natural y conversacional.</span><span class="sxs-lookup"><span data-stu-id="7ce2e-108">Other times you would like to customize your answers to questions in a more natural, conversational way.</span></span> 

## <a name="create-a-qna-maker-service"></a><span data-ttu-id="7ce2e-109">Creación de un servicio QnA Maker</span><span class="sxs-lookup"><span data-stu-id="7ce2e-109">Create a QnA Maker service</span></span>
<span data-ttu-id="7ce2e-110">En primer lugar, cree una cuenta e inicie sesión en [QnA Maker](https://qnamaker.ai/).</span><span class="sxs-lookup"><span data-stu-id="7ce2e-110">First create an account and sign in at [QnA Maker](https://qnamaker.ai/).</span></span> <span data-ttu-id="7ce2e-111">Después, vaya a **Create a knowledge base** (Crear una base de conocimiento).</span><span class="sxs-lookup"><span data-stu-id="7ce2e-111">Then navigate to **Create a knowledge base**.</span></span> <span data-ttu-id="7ce2e-112">Haga clic en **Create a QnA service** (Crear un servicio QnA) y siga las instrucciones para crear un servicio de Azure QnA.</span><span class="sxs-lookup"><span data-stu-id="7ce2e-112">Click **Create a QnA service** and follow the instructions for creating an Azure QnA service.</span></span>

![Imagen 1 de QnA](media/QnA_1.png)

<span data-ttu-id="7ce2e-114">Se le redirigirá a [Create QnA Maker](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker) (Crear QnA Maker).</span><span class="sxs-lookup"><span data-stu-id="7ce2e-114">You will be redirected to [Create QnA Maker](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker).</span></span> <span data-ttu-id="7ce2e-115">Complete el formulario y haga clic en **Crear**.</span><span class="sxs-lookup"><span data-stu-id="7ce2e-115">Complete the form and click **Create**.</span></span>

![Imagen 2 de QnA](media/QnA_2.png)
 
<span data-ttu-id="7ce2e-117">Después de crear el servicio QnA en Azure Portal se le proporcionarán claves bajo el encabezado Administración de recursos que puede pasar por alto.</span><span class="sxs-lookup"><span data-stu-id="7ce2e-117">After you create your QnA service in the Azure Portal you will be given keys under the Resource Management heading that you may disregard.</span></span> <span data-ttu-id="7ce2e-118">Vaya al paso 2 para conectar el servicio Azure QnA.</span><span class="sxs-lookup"><span data-stu-id="7ce2e-118">Proceed to step 2 to connect your Azure QnA service.</span></span> <span data-ttu-id="7ce2e-119">Actualice la página para seleccionar el servicio de Azure que acaba de crear y escriba el nombre de la base de conocimiento.</span><span class="sxs-lookup"><span data-stu-id="7ce2e-119">Refresh the page to select the Azure service you just created and enter the name of your knowledge base.</span></span>

![Imagen 3 de QnA](media/QnA_3.png)

<span data-ttu-id="7ce2e-121">Haga clic en **Create your KB** (Crear su KB).</span><span class="sxs-lookup"><span data-stu-id="7ce2e-121">Click **Create your KB**.</span></span> <span data-ttu-id="7ce2e-122">Escriba sus propias preguntas y respuestas, o bien copie los ejemplos siguientes.</span><span class="sxs-lookup"><span data-stu-id="7ce2e-122">Enter your own question and answers or copy the following examples.</span></span> 

![Imagen 4 de QnA](media/QnA_4.png)

<span data-ttu-id="7ce2e-124">Como alternativa, puede seleccionar **Populate your KB** (Rellenar su KB) y proporcionar un archivo o una dirección URL.</span><span class="sxs-lookup"><span data-stu-id="7ce2e-124">Alternatively, you can choose **Populate your KB** and provide a file or URL.</span></span> <span data-ttu-id="7ce2e-125">[Aquí](https://aka.ms/qna-tsv) encontrará un archivo de código fuente de ejemplo para generar un servicio QnA Maker simple.</span><span class="sxs-lookup"><span data-stu-id="7ce2e-125">A sample source file for generating a simple QnA Maker service is [here](https://aka.ms/qna-tsv).</span></span>

<span data-ttu-id="7ce2e-126">Después de agregar nuevos pares de QnA o rellenar la KB, haga clic en **Guardar y entrenar**.</span><span class="sxs-lookup"><span data-stu-id="7ce2e-126">After adding new QnA pairs or populating your KB, click **Save and train**.</span></span> <span data-ttu-id="7ce2e-127">Cuando haya terminado, haga clic en **Publicar** en la pestaña **PUBLICAR**.</span><span class="sxs-lookup"><span data-stu-id="7ce2e-127">Once you are completed, in the **PUBLISH** tab, click **Publish**.</span></span>

<span data-ttu-id="7ce2e-128">Para conectar el servicio QnA al bot, necesitará la cadena de solicitud HTTP que contiene el identificador de la base de conocimiento y la clave de suscripción de QnA Maker.</span><span class="sxs-lookup"><span data-stu-id="7ce2e-128">To connect your QnA service to your bot, you will need the HTTP request string containing the knowledge base ID and QnA Maker subscription key.</span></span> <span data-ttu-id="7ce2e-129">Copie la solicitud HTTP de ejemplo del resultado de la publicación.</span><span class="sxs-lookup"><span data-stu-id="7ce2e-129">Copy the example HTTP request from the publishing result.</span></span> 

![Imagen 5 de QnA](media/QnA_5.png)

## <a name="installing-packages"></a><span data-ttu-id="7ce2e-131">Instalación de paquetes</span><span class="sxs-lookup"><span data-stu-id="7ce2e-131">Installing Packages</span></span>

<span data-ttu-id="7ce2e-132">Antes de pasar al código, asegúrese de que tiene los paquetes necesarios para QnA Maker.</span><span class="sxs-lookup"><span data-stu-id="7ce2e-132">Before we get coding, make sure you have the packages necessary for QnA Maker.</span></span>

# <a name="ctabcs"></a>[<span data-ttu-id="7ce2e-133">C#</span><span class="sxs-lookup"><span data-stu-id="7ce2e-133">C#</span></span>](#tab/cs)

<span data-ttu-id="7ce2e-134">[Agregue una referencia](https://docs.microsoft.com/en-us/nuget/tools/package-manager-ui) a la versión preliminar v4 de los siguientes paquetes NuGet:</span><span class="sxs-lookup"><span data-stu-id="7ce2e-134">[Add a reference](https://docs.microsoft.com/en-us/nuget/tools/package-manager-ui) to v4 prerelease version of the following NuGet packages:</span></span>

* `Microsoft.Bot.Builder.Ai.QnA`

# <a name="javascripttabjs"></a>[<span data-ttu-id="7ce2e-135">JavaScript</span><span class="sxs-lookup"><span data-stu-id="7ce2e-135">JavaScript</span></span>](#tab/js)

<span data-ttu-id="7ce2e-136">Puede agregar cualquiera de estos servicios al bot mediante el paquete botbuilder-ai.</span><span class="sxs-lookup"><span data-stu-id="7ce2e-136">Either of these services can be added to your bot using the botbuilder-ai package.</span></span> <span data-ttu-id="7ce2e-137">Puede agregar este paquete al proyecto a través de npm:</span><span class="sxs-lookup"><span data-stu-id="7ce2e-137">You can add this package to your project via npm:</span></span>

* `npm install --save botbuilder@preview`
* `npm install --save botbuilder-ai@preview`

---


## <a name="using-qna-maker"></a><span data-ttu-id="7ce2e-138">Uso de QnA Maker</span><span class="sxs-lookup"><span data-stu-id="7ce2e-138">Using QnA Maker</span></span>

<span data-ttu-id="7ce2e-139">QnA Maker se agrega primero como software intermedio.</span><span class="sxs-lookup"><span data-stu-id="7ce2e-139">QnA Maker is first added as middleware.</span></span> <span data-ttu-id="7ce2e-140">Después, los resultados se pueden usar dentro de la lógica del bot.</span><span class="sxs-lookup"><span data-stu-id="7ce2e-140">Then we can use the results within our bot logic.</span></span>

# <a name="ctabcs"></a>[<span data-ttu-id="7ce2e-141">C#</span><span class="sxs-lookup"><span data-stu-id="7ce2e-141">C#</span></span>](#tab/cs)

<span data-ttu-id="7ce2e-142">Actualice el método `ConfigureServices` del archivo `Startup.cs` para agregar un objeto `QnAMakerMiddleware`.</span><span class="sxs-lookup"><span data-stu-id="7ce2e-142">Update the `ConfigureServices` method in your `Startup.cs` file to add a `QnAMakerMiddleware` object.</span></span> <span data-ttu-id="7ce2e-143">Puede configurar el bot de modo que compruebe en la base de conocimiento todos los mensajes que reciba de un usuario. Para ello, basta con agregarlo a la pila de software intermedio del bot.</span><span class="sxs-lookup"><span data-stu-id="7ce2e-143">You can configure your bot to check your knowledge base for every message received from a user, by simply adding it to your bot's middleware stack.</span></span>


```csharp
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Bot.Builder.Ai.Qna;
using Microsoft.Bot.Builder.BotFramework;
using Microsoft.Bot.Builder.Core.Extensions;
using Microsoft.Bot.Builder.Integration.AspNet.Core;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Logging;
using System;

public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton(_ => Configuration);
    services.AddBot<AiBot>(options =>
    {
        options.CredentialProvider = new ConfigurationCredentialProvider(Configuration);

        var endpoint = new QnAMakerEndpoint
        {
           KnowledgeBaseId = "YOUR-KB-ID",
           // Get the Host from the HTTP request example at https://www.qnamaker.ai
           // For GA services: https://<Service-Name>.azurewebsites.net/qnamaker
           // For Preview services: https://westus.api.cognitive.microsoft.com/qnamaker/v2.0           
           Host = "YOUR-HTTP-REQUEST-HOST",
           EndpointKey = "YOUR-QNA-MAKER-SUBSCRIPTION-KEY"
        };
        options.Middleware.Add(new QnAMakerMiddleware(endpoint));
    });
}
```



<span data-ttu-id="7ce2e-144">Modifique el código del archivo EchoBot.cs para que `OnTurn` envíe un mensaje de reserva en caso de que el software intermedio de QnA Maker no haya enviado una respuesta a la pregunta del usuario:</span><span class="sxs-lookup"><span data-stu-id="7ce2e-144">Edit code in the EchoBot.cs file, so that `OnTurn` sends a fallback message in the case that the QnA Maker middleware didn't send a response to the user's question:</span></span>

```csharp
using System.Threading.Tasks;
using Microsoft.Bot;
using Microsoft.Bot.Builder;
using Microsoft.Bot.Schema;

namespace Bot_Builder_Echo_Bot_QnA
{
    public class EchoBot : IBot
    {    
        public async Task OnTurn(ITurnContext context)
        {
            // This bot is only handling Messages
            if (context.Activity.Type == ActivityTypes.Message)
            {             
                if (!context.Responded)
                {
                    // QnA didn't send the user an answer
                    await context.SendActivity("Sorry, I couldn't find a good match in the KB.");

                }
            }
        }
    }
}
```


<span data-ttu-id="7ce2e-145">Vea el [ejemplo QnA Maker](https://aka.ms/qna-cs-bot-sample) para obtener un bot de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="7ce2e-145">See the [QnA Maker sample](https://aka.ms/qna-cs-bot-sample) for a sample bot.</span></span>

# <a name="javascripttabjs"></a>[<span data-ttu-id="7ce2e-146">JavaScript</span><span class="sxs-lookup"><span data-stu-id="7ce2e-146">JavaScript</span></span>](#tab/js)

<span data-ttu-id="7ce2e-147">En primer lugar, use require o import en la clase [QnAMaker](https://github.com/Microsoft/botbuilder-js/tree/master/doc/botbuilder-ai/classes/botbuilder_ai.qnamaker.md):</span><span class="sxs-lookup"><span data-stu-id="7ce2e-147">First require/import in the [QnAMaker](https://github.com/Microsoft/botbuilder-js/tree/master/doc/botbuilder-ai/classes/botbuilder_ai.qnamaker.md) class:</span></span>

```js
const { QnAMaker } = require('botbuilder-ai');
```

<span data-ttu-id="7ce2e-148">Cree un `QnAMaker` inicializándolo con una cadena basada en la solicitud HTTP para el servicio QnA.</span><span class="sxs-lookup"><span data-stu-id="7ce2e-148">Create a `QnAMaker` by initializing it with a string based on the HTTP request for your QnA service.</span></span> <span data-ttu-id="7ce2e-149">Puede copiar una solicitud de ejemplo para el servicio desde el [portal de QnA Maker](https://qnamaker.ai) en Configuración > Detalles de la implementación.</span><span class="sxs-lookup"><span data-stu-id="7ce2e-149">You can copy an example request for your service from the [QnA Maker portal](https://qnamaker.ai) under Settings > Deployment details.</span></span>


<span data-ttu-id="7ce2e-150">El formato de la cadena varía en función de si el servicio de QnA Maker usa la versión de disponibilidad general o la versión preliminar de QnA Maker.</span><span class="sxs-lookup"><span data-stu-id="7ce2e-150">The format of the string varies depending on whether your QnA Maker service is using the GA or the Preview version of QnA Maker.</span></span> <span data-ttu-id="7ce2e-151">Copie la solicitud HTTP de ejemplo y obtenga el Id. de la base de conocimiento, la clave de suscripción y el host que se van a usar al inicializar `QnAMaker`.</span><span class="sxs-lookup"><span data-stu-id="7ce2e-151">Copy the example HTTP request, and get the knowledge base ID, subscription key, and host to use when you initialize the `QnAMaker`.</span></span>

<!--
**Preview**
```js
const qnaEndpointString = 
    // Replace xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx with your knowledge base ID
    "POST /knowledgebases/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/generateAnswer\r\n" + 
    "Host: https://westus.api.cognitive.microsoft.com/qnamaker/v2.0\r\n" +
    // Replace xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx with your QnAMaker subscription key
    "Ocp-Apim-Subscription-Key: xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx\r\n"
const qna = new QnAMaker(qnaEndpointString);
```

**GA**
```js
const qnaEndpointString = 
    // Replace xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx with your knowledge base ID
    "POST /knowledgebases/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/generateAnswer\r\n" + 
    // Replace <Service-Name> to match the Azure URL where your service is hosted
    "Host: https://<Service-Name>.azurewebsites.net/qnamaker\r\n" +
    // Replace xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx with your QnAMaker subscription key
    "Authorization: EndpointKey xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx\r\n"
const qna = new QnAMaker(qnaEndpointString);
```
-->
<span data-ttu-id="7ce2e-152">**Vista previa**</span><span class="sxs-lookup"><span data-stu-id="7ce2e-152">**Preview**</span></span>
```js
const qna = new QnAMaker(
    {
        knowledgeBaseId: '<KNOWLEDGE-BASE-ID>',
        endpointKey: '<QNA-SUBSCRIPTION-KEY>',
        host: 'https://westus.api.cognitive.microsoft.com/qnamaker/v2.0'
    },
    {
        // set this to true to send answers from QnA Maker
        answerBeforeNext: true
    }
);
```
<span data-ttu-id="7ce2e-153">**Disponibilidad general**</span><span class="sxs-lookup"><span data-stu-id="7ce2e-153">**GA**</span></span>
```js
const qna = new QnAMaker(
    {
        knowledgeBaseId: '<KNOWLEDGE-BASE-ID>',
        endpointKey: '<QNA-SUBSCRIPTION-KEY>',
        host: 'https://<Service-Name>.azurewebsites.net/qnamaker'
    },
    {
        answerBeforeNext: true
    }
);
```
<span data-ttu-id="7ce2e-154">Puede configurar el bot para que llame automáticamente a QnA Maker si simplemente lo agrega a la pila de software intermedio del bot:</span><span class="sxs-lookup"><span data-stu-id="7ce2e-154">You can configure your bot to automatically call QNA maker by simply adding it to your bot's middleware stack:</span></span>

```js
// Add QnA Maker as middleware
adapter.use(qna);

// Listen for incoming requests 
server.post('/api/messages', (req, res) => {
    // Route received request to adapter for processing
    adapter.processActivity(req, res, async (context) => {
        // If `!context.responded`, that means an answer wasn't found for the user's utterance.
        // In this case, we send the user a fallback message.
        if (context.activity.type === 'message' && !context.responded) {
            await context.sendActivity('No QnA Maker answers were found.');
        } else if (context.activity.type !== 'message') {
            await context.sendActivity(`[${context.activity.type} event detected]`);
        }
    });
});
```

<span data-ttu-id="7ce2e-155">La inicialización de `answerBeforeNext` en `true` al inicializar `QnAMaker` significa que el software intermedio de QnA Maker responde automáticamente si encuentra una respuesta, antes de llegar a la lógica principal del bot en `processActivity`.</span><span class="sxs-lookup"><span data-stu-id="7ce2e-155">Initializing  `answerBeforeNext` to `true` when initializing `QnAMaker` means that the QnA Maker middleware automatically responds if it finds an answer, before getting to your bot's main logic in `processActivity`.</span></span> <span data-ttu-id="7ce2e-156">Si en su lugar se establece `answerBeforeNext` en `false`, el bot solo llamará a QnA Maker después de la lógica principal del bot en ejecuciones de `processActivity`, y solo si el bot no ha respondido al usuario.</span><span class="sxs-lookup"><span data-stu-id="7ce2e-156">If instead, you set `answerBeforeNext` to `false`, the bot will only call QnA Maker after all of your bot's main logic in `processActivity` runs, and only if your bot hasn't replied to the user.</span></span> 

## <a name="calling-qna-maker-without-using-middleware"></a><span data-ttu-id="7ce2e-157">Una llamada a QnA Maker sin usar software intermedio</span><span class="sxs-lookup"><span data-stu-id="7ce2e-157">Calling QnA Maker without using middleware</span></span>

<span data-ttu-id="7ce2e-158">En el ejemplo anterior, la instrucción `adapter.use(qna);` significa que QnA se está ejecutando como software intermedio y, por tanto, responde a todos los mensajes que recibe el bot.</span><span class="sxs-lookup"><span data-stu-id="7ce2e-158">In the previous example, the `adapter.use(qna);` statement means that QnA is running as middleware, and therefore responds to every message the bot receives.</span></span> <span data-ttu-id="7ce2e-159">Para obtener más control sobre cómo y cuándo se llama a QnA Maker, puede llamar a `qna.answer()` directamente desde dentro de la lógica del bot en vez de instalarlo como un elemento de software intermedio.</span><span class="sxs-lookup"><span data-stu-id="7ce2e-159">For more control over how and when QnA Maker is called, you can call `qna.answer()` directly from within your bot's logic instead of installing it as a piece of middleware.</span></span>

<span data-ttu-id="7ce2e-160">Quite la instrucción `adapter.use(qna);` y use el código siguiente para llamar directamente a QnA Maker.</span><span class="sxs-lookup"><span data-stu-id="7ce2e-160">Remove the `adapter.use(qna);` statement and use the following code to call QnA Maker directly.</span></span>

```js
// Listen for incoming activity 
server.post('/api/messages', (req, res) => {
    // Route received activity to adapter for processing
    adapter.processActivity(req, res, async (context) => {
        if (context.activity.type === 'message') {
            var handled = await qna.answer(context)
                if (!handled) {
                    await context.sendActivity(`I'm sorry. I didn't understand.`);
                }
        }
    });
});
```

<span data-ttu-id="7ce2e-161">Otra forma de personalizar QnA Maker es mediante `qna.generateAnswer()`.</span><span class="sxs-lookup"><span data-stu-id="7ce2e-161">Another way to customize the QnA Maker is with `qna.generateAnswer()`.</span></span> <span data-ttu-id="7ce2e-162">Este método permite obtener más detalles sobre las respuestas de QnA Maker.</span><span class="sxs-lookup"><span data-stu-id="7ce2e-162">This method allows you to get more detail on the answers from QnA Maker.</span></span>


```js
// Listen for incoming activity 
server.post('/api/messages', (req, res) => {
    // Route received activity to adapter for processing
    adapter.processActivity(req, res, async (context) => {
        if (context.activity.type === 'message') {
            // Get all the answers QnA Maker finds
            var results = await qna.generateAnswer(context.activity.text);
                if (results && results.length > 0) {
                    await context.sendActivity(results[0].answer);
                } else {
                    await context.sendActivity(`I don't know.`);
                }
    
        }
    });
});
```
---

<span data-ttu-id="7ce2e-163">Haga preguntas al bot para ver las respuestas del servicio QnA Maker.</span><span class="sxs-lookup"><span data-stu-id="7ce2e-163">Ask your bot questions to see the replies from your QnA Maker service.</span></span>

![Imagen 6 de QnA](media/QnA_6.png)



## <a name="next-steps"></a><span data-ttu-id="7ce2e-165">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="7ce2e-165">Next steps</span></span>

<span data-ttu-id="7ce2e-166">QnA Maker se puede combinar con otros servicios de Cognitive Services para que el bot sea aún más eficaz.</span><span class="sxs-lookup"><span data-stu-id="7ce2e-166">QnA Maker can be combined with other Cognitive Services, to make your bot even more powerful.</span></span> <span data-ttu-id="7ce2e-167">La herramienta de distribución proporciona una forma de combinar QnA con Language Understanding (LUIS) en el bot.</span><span class="sxs-lookup"><span data-stu-id="7ce2e-167">The Dispatch tool provides a way to combine QnA with Language Understanding (LUIS) in your bot.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="7ce2e-168">Combinación de aplicaciones de LUIS y servicios QnA con la herramienta de distribución</span><span class="sxs-lookup"><span data-stu-id="7ce2e-168">Combine LUIS apps and QnA services using the Dispatch tool</span></span>](./bot-builder-tutorial-dispatch.md)
