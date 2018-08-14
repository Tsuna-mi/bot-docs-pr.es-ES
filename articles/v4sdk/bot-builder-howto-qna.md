---
title: Uso de QnA Maker | Microsoft Docs
description: Obtenga información sobre cómo usar QnA Maker en el bot.
keywords: pregunta y respuesta, QnA, preguntas más frecuentes, software intermedio
author: ivorb
ms.author: v-ivorb
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 03/13/2018
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: 78bc2c849a2c1900da33c7419693a7ff84c43cb0
ms.sourcegitcommit: dcbc8ad992a3e242a11ebcdf0ee99714d919a877
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/30/2018
ms.locfileid: "39352954"
---
# <a name="how-to-use-qna-maker"></a>Cómo usar QnA Maker

Para agregar compatibilidad simple de preguntas y respuestas al bot, puede usar el servicio [QnA Maker](https://qnamaker.ai/).

Uno de los requisitos básicos para escribir un servicio de QnA Maker propio es inicializarlo con preguntas y respuestas. En muchos casos, las preguntas y respuestas ya existen en el contenido como preguntas más frecuentes u otra documentación. En otras ocasiones, le interesará personalizar las respuestas a las preguntas de forma más natural y conversacional. 

## <a name="create-a-qna-maker-service"></a>Creación de un servicio QnA Maker
En primer lugar, cree una cuenta e inicie sesión en [QnA Maker](https://qnamaker.ai/). Después, vaya a **Create a knowledge base** (Crear una base de conocimiento). Haga clic en **Create a QnA service** (Crear un servicio QnA) y siga las instrucciones para crear un servicio de Azure QnA.

![Imagen 1 de QnA](media/QnA_1.png)

Se le redirigirá a [Create QnA Maker](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker) (Crear QnA Maker). Complete el formulario y haga clic en **Crear**.

![Imagen 2 de QnA](media/QnA_2.png)
 
Después de crear el servicio QnA en Azure Portal se le proporcionarán claves bajo el encabezado Administración de recursos que puede pasar por alto. Vaya al paso 2 para conectar el servicio Azure QnA. Actualice la página para seleccionar el servicio de Azure que acaba de crear y escriba el nombre de la base de conocimiento.

![Imagen 3 de QnA](media/QnA_3.png)

Haga clic en **Create your KB** (Crear su KB). Escriba sus propias preguntas y respuestas, o bien copie los ejemplos siguientes. 

![Imagen 4 de QnA](media/QnA_4.png)

Como alternativa, puede seleccionar **Populate your KB** (Rellenar su KB) y proporcionar un archivo o una dirección URL. [Aquí](https://aka.ms/qna-tsv) encontrará un archivo de código fuente de ejemplo para generar un servicio QnA Maker simple.

Después de agregar nuevos pares de QnA o rellenar la KB, haga clic en **Guardar y entrenar**. Cuando haya terminado, haga clic en **Publicar** en la pestaña **PUBLICAR**.

Para conectar el servicio QnA al bot, necesitará la cadena de solicitud HTTP que contiene el identificador de la base de conocimiento y la clave de suscripción de QnA Maker. Copie la solicitud HTTP de ejemplo del resultado de la publicación. 

![Imagen 5 de QnA](media/QnA_5.png)

## <a name="installing-packages"></a>Instalación de paquetes

Antes de pasar al código, asegúrese de que tiene los paquetes necesarios para QnA Maker.

# <a name="ctabcs"></a>[C#](#tab/cs)

[Agregue una referencia](https://docs.microsoft.com/en-us/nuget/tools/package-manager-ui) a la versión preliminar v4 de los siguientes paquetes NuGet:

* `Microsoft.Bot.Builder.Ai.QnA`

# <a name="javascripttabjs"></a>[JavaScript](#tab/js)

Cualquiera de estos servicios se puede agregar al bot mediante el paquete botbuilder-ai. Puede agregar este paquete al proyecto a través de npm:

* `npm install --save botbuilder@preview`
* `npm install --save botbuilder-ai@preview`

---


## <a name="using-qna-maker"></a>Uso de QnA Maker

QnA Maker se agrega primero como software intermedio. Después, los resultados se pueden usar dentro de la lógica del bot.

# <a name="ctabcs"></a>[C#](#tab/cs)

Actualice el método `ConfigureServices` del archivo `Startup.cs` para agregar un objeto `QnAMakerMiddleware`. Puede configurar el bot de modo que compruebe en la base de conocimiento todos los mensajes que reciba de un usuario. Para ello, basta con agregarlo a la pila de software intermedio del bot.


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



Modifique el código del archivo EchoBot.cs para que `OnTurn` envíe un mensaje de reserva en caso de que el software intermedio de QnA Maker no haya enviado una respuesta a la pregunta del usuario:

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


Vea el [ejemplo QnA Maker](https://aka.ms/qna-cs-bot-sample) para obtener un bot de ejemplo.

# <a name="javascripttabjs"></a>[JavaScript](#tab/js)

En primer lugar, use require o import en la clase [QnAMaker](https://github.com/Microsoft/botbuilder-js/tree/master/doc/botbuilder-ai/classes/botbuilder_ai.qnamaker.md):

```js
const { QnAMaker } = require('botbuilder-ai');
```

Cree un `QnAMaker` inicializándolo con una cadena basada en la solicitud HTTP para el servicio QnA. Puede copiar una solicitud de ejemplo para el servicio desde el [portal de QnA Maker](https://qnamaker.ai) en Configuración > Detalles de la implementación.


El formato de la cadena varía en función de si el servicio de QnA Maker usa la versión de disponibilidad general o la versión preliminar de QnA Maker. Copie la solicitud HTTP de ejemplo y obtenga el Id. de la base de conocimiento, la clave de suscripción y el host que se van a usar al inicializar `QnAMaker`.

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
**Vista previa**
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
**Disponibilidad general**
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
Puede configurar el bot para que llame automáticamente a QnA Maker si simplemente lo agrega a la pila de software intermedio del bot:

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

La inicialización de `answerBeforeNext` en `true` al inicializar `QnAMaker` significa que el software intermedio de QnA Maker responde automáticamente si encuentra una respuesta, antes de llegar a la lógica principal del bot en `processActivity`. Si en su lugar se establece `answerBeforeNext` en `false`, el bot solo llamará a QnA Maker después de la lógica principal del bot en ejecuciones de `processActivity`, y solo si el bot no ha respondido al usuario. 

## <a name="calling-qna-maker-without-using-middleware"></a>Una llamada a QnA Maker sin usar software intermedio

En el ejemplo anterior, la instrucción `adapter.use(qna);` significa que QnA se está ejecutando como software intermedio y, por tanto, responde a todos los mensajes que recibe el bot. Para obtener más control sobre cómo y cuándo se llama a QnA Maker, puede llamar a `qna.answer()` directamente desde dentro de la lógica del bot en vez de instalarlo como un elemento de software intermedio.

Quite la instrucción `adapter.use(qna);` y use el código siguiente para llamar directamente a QnA Maker.

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

Otra forma de personalizar QnA Maker es mediante `qna.generateAnswer()`. Este método permite obtener más detalles sobre las respuestas de QnA Maker.


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

Haga preguntas al bot para ver las respuestas del servicio QnA Maker.

![Imagen 6 de QnA](media/QnA_6.png)



## <a name="next-steps"></a>Pasos siguientes

QnA Maker se puede combinar con otros servicios de Cognitive Services para que el bot sea aún más eficaz. La herramienta de distribución proporciona una forma de combinar QnA con Language Understanding (LUIS) en el bot.

> [!div class="nextstepaction"]
> [Combinación de aplicaciones de LUIS y servicios QnA con la herramienta de distribución](./bot-builder-tutorial-dispatch.md)
