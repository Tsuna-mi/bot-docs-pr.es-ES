---
title: Traducir la entrada de usuario para que el bot sea multilingüe | Microsoft Docs
description: Cómo traducir la entrada de usuario automáticamente al lenguaje nativo del bot y volver a traducirla al idioma del usuario.
author: DeniseMak
ms.author: v-demak
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 04/06/2018
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: e316ff90b68f860274579f06e7196deec364e082
ms.sourcegitcommit: 2dc75701b169d822c9499e393439161bc87639d2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42905638"
---
# <a name="translate-from-the-users-language-to-make-your-bot-multilingual"></a><span data-ttu-id="cc944-103">Traducir del idioma del usuario para que el bot sea multilingüe</span><span class="sxs-lookup"><span data-stu-id="cc944-103">Translate from the user's language to make your bot multilingual</span></span>

[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]

<span data-ttu-id="cc944-104">El bot puede usar [Microsoft Translator](https://www.microsoft.com/en-us/translator/) para traducir automáticamente los mensajes a un lenguaje que el bot entienda y, opcionalmente, traducir las respuestas del bot al idioma del usuario.</span><span class="sxs-lookup"><span data-stu-id="cc944-104">Your bot can use [Microsoft Translator](https://www.microsoft.com/en-us/translator/) to automatically translate messages to the language your bot understands, and optionally translate the bot's replies back to the user's language.</span></span>
<!-- 
- [Get a Text Services key](#get-a-text-services-key)
- [Installing Packages](#installing-packages)
- [Combine translation with LUIS or QnA Maker](#using-luis)
- [Combine translation with QnA Maker](#using-qna-maker)
-->

## <a name="get-a-text-services-key"></a><span data-ttu-id="cc944-105">Obtener una clave de Text Services</span><span class="sxs-lookup"><span data-stu-id="cc944-105">Get a Text Services key</span></span>

<span data-ttu-id="cc944-106">En primer lugar, necesitará una clave para usar el servicio Microsoft Translator.</span><span class="sxs-lookup"><span data-stu-id="cc944-106">First, you'll need a key for using the Microsoft Translator service.</span></span> <span data-ttu-id="cc944-107">Puede obtener una [clave de evaluación gratuita](https://www.microsoft.com/en-us/translator/trial.aspx#get-started) en Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="cc944-107">You can get a [free trial key](https://www.microsoft.com/en-us/translator/trial.aspx#get-started) in the Azure portal.</span></span>

## <a name="installing-packages"></a><span data-ttu-id="cc944-108">Instalar paquetes</span><span class="sxs-lookup"><span data-stu-id="cc944-108">Installing Packages</span></span>

<span data-ttu-id="cc944-109">Asegúrese de que tiene los paquetes necesarios para agregar la traducción al bot.</span><span class="sxs-lookup"><span data-stu-id="cc944-109">Make sure you have the packages necessary to add translation to your bot.</span></span>

# <a name="ctabcsrefs"></a>[<span data-ttu-id="cc944-110">C#</span><span class="sxs-lookup"><span data-stu-id="cc944-110">C#</span></span>](#tab/csrefs)

<span data-ttu-id="cc944-111">[Agregue una referencia](https://docs.microsoft.com/en-us/nuget/tools/package-manager-ui) a la versión preliminar de los siguientes paquetes NuGet:</span><span class="sxs-lookup"><span data-stu-id="cc944-111">[Add a reference](https://docs.microsoft.com/en-us/nuget/tools/package-manager-ui) to the prerelease version of the following NuGet packages:</span></span>

* `Microsoft.Bot.Builder.Integration.AspNet.Core`
* <span data-ttu-id="cc944-112">`Microsoft.Bot.Builder.Ai` (necesario para la traducción)</span><span class="sxs-lookup"><span data-stu-id="cc944-112">`Microsoft.Bot.Builder.Ai` (required for translation)</span></span>

<span data-ttu-id="cc944-113">Si va a combinar la traducción con Language Understanding (LUIS), agregue también una referencia a:</span><span class="sxs-lookup"><span data-stu-id="cc944-113">If you're going to combine translation with Language Understanding (LUIS), also add a reference to:</span></span>

* <span data-ttu-id="cc944-114">`Microsoft.Bot.Builder.Luis` (necesario para LUIS)</span><span class="sxs-lookup"><span data-stu-id="cc944-114">`Microsoft.Bot.Builder.Luis` (required for LUIS)</span></span>

# <a name="javascripttabjsrefs"></a>[<span data-ttu-id="cc944-115">JavaScript</span><span class="sxs-lookup"><span data-stu-id="cc944-115">JavaScript</span></span>](#tab/jsrefs)

<span data-ttu-id="cc944-116">Puede agregar cualquiera de estos servicios al bot mediante el paquete botbuilder-ai.</span><span class="sxs-lookup"><span data-stu-id="cc944-116">Either of these services can be added to your bot using the botbuilder-ai package.</span></span> <span data-ttu-id="cc944-117">Puede agregar este paquete al proyecto a través de npm:</span><span class="sxs-lookup"><span data-stu-id="cc944-117">You can add this package to your project via npm:</span></span>
* `npm install --save botbuilder@preview`
* `npm install --save botbuilder-ai@preview`

---

## <a name="configure-translation"></a><span data-ttu-id="cc944-118">Configurar la traducción</span><span class="sxs-lookup"><span data-stu-id="cc944-118">Configure translation</span></span>

<span data-ttu-id="cc944-119">Puede configurar el bot de modo que llame al traductor para cada mensaje que reciba de un usuario. Para ello, basta con agregarlo a la pila de software intermedio del bot.</span><span class="sxs-lookup"><span data-stu-id="cc944-119">You can configure your bot to call the translator for every message received from a user, by simply adding it to your bot's middleware stack.</span></span> <span data-ttu-id="cc944-120">El software intermedio usa el resultado de la traducción para modificar el mensaje del usuario mediante el objeto de contexto.</span><span class="sxs-lookup"><span data-stu-id="cc944-120">The middleware uses the translation result to modify the user's message using the context object.</span></span>


# <a name="ctabcssetuptranslate"></a>[<span data-ttu-id="cc944-121">C#</span><span class="sxs-lookup"><span data-stu-id="cc944-121">C#</span></span>](#tab/cssetuptranslate)

<span data-ttu-id="cc944-122">Comience con el ejemplo EchoBot del SDK y actualice el método `ConfigureServices` en el archivo `Startup.cs` para agregar `TranslationMiddleware` al bot.</span><span class="sxs-lookup"><span data-stu-id="cc944-122">Start with the EchoBot sample in the SDK, and update the `ConfigureServices` method in your `Startup.cs` file to add `TranslationMiddleware` to the bot.</span></span> <span data-ttu-id="cc944-123">Esto configura el bot de modo que traduzca todos los mensajes recibidos de un usuario.</span><span class="sxs-lookup"><span data-stu-id="cc944-123">This configures your bot to translate every message received from a user.</span></span> <!--, by simply adding it to your bot's middleware set. The middleware stores the translation results on the context object. -->

<span data-ttu-id="cc944-124">**Startup.cs**</span><span class="sxs-lookup"><span data-stu-id="cc944-124">**Startup.cs**</span></span>
```csharp
using System;
using System.Collections.Generic;
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Bot.Builder.Ai;
using Microsoft.Bot.Builder.BotFramework;
using Microsoft.Bot.Builder.Core.Extensions;
using Microsoft.Bot.Builder.Integration.AspNet.Core;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Logging;

// This method gets called by the runtime. Use this method to add services to the container.
public void ConfigureServices(IServiceCollection services)
{
    services.AddBot<TranslatorBot>(options =>
    {
        options.CredentialProvider = new ConfigurationCredentialProvider(Configuration);
        var middleware = options.Middleware;
        // Add translation middleware
        // The first parameter is a list of languages the bot recognizes
        middleware.Add(new TranslationMiddleware(new string[] { "en" }, "<YOUR MICROSOFT TRANSLATOR API KEY>", false));

    });
}


```

> [!TIP] 
> <span data-ttu-id="cc944-125">El SDK de Bot Builder detecta automáticamente el idioma del usuario en función del mensaje que acaba de enviar.</span><span class="sxs-lookup"><span data-stu-id="cc944-125">The BotBuilder SDK automatically detects the user's language based on the message they just submitted.</span></span> <span data-ttu-id="cc944-126">Para invalidar esta funcionalidad, puede proporcionar parámetros adicionales de devolución de llamada para agregar su propia lógica a fin de detectar y cambiar el idioma del usuario.</span><span class="sxs-lookup"><span data-stu-id="cc944-126">To override this functionality, you can supply additional callback parameters to add your own logic for detecting and changing the user's language.</span></span>  



<span data-ttu-id="cc944-127">Eche un vistazo al código de `EchoBot.cs`, en el que se envía "You sent" seguido de lo que dice el usuario:</span><span class="sxs-lookup"><span data-stu-id="cc944-127">Take a look at the code in `EchoBot.cs`, where it sends "You sent" followed by what the user says:</span></span>

```cs
using Microsoft.Bot.Builder;
using Microsoft.Bot.Schema;
using System.Threading.Tasks;

namespace Microsoft.Bot.Samples
{
    public class EchoBot : IBot
    {
        public EchoBot() { }

        public async Task OnTurn(ITurnContext context)
        {
            switch (context.Activity.Type)
            {
                case ActivityTypes.Message:
                // echo back the user's input.
                    await context.SendActivity($"You sent '{context.Activity.Text}'");


                case ActivityTypes.ConversationUpdate:
                    foreach (var newMember in context.Activity.MembersAdded)
                    {
                        if (newMember.Id != context.Activity.Recipient.Id)
                        {
                            await context.SendActivity("Hello and welcome to the echo bot.");
                        }
                    }
                    break;
            }
        }
        
    }
}
```

<span data-ttu-id="cc944-128">Cuando se agrega software intermedio de traducción, un parámetro opcional especifica si se deben volver a traducir las respuestas al idioma del usuario.</span><span class="sxs-lookup"><span data-stu-id="cc944-128">When you add translation middleware, an optional parameter specifies whether to translate replies back to the user's language.</span></span> <span data-ttu-id="cc944-129">En `Startup.cs`, hemos especificado `false` para simplemente traducir los mensajes de usuario al lenguaje del bot.</span><span class="sxs-lookup"><span data-stu-id="cc944-129">In `Startup.cs`, we specified `false` to just translate the user messages to the bot's language.</span></span>

```cs
        // The first parameter is a list of languages the bot recognizes
        // The second parameter is your API key
        // The third parameter specifies whether to translate the bot's replies back to the user's language
        middleware.Add(new TranslationMiddleware(new string[] { "en" }, "<YOUR MICROSOFT TRANSLATOR API KEY>", false));
```

# <a name="javascripttabjssetuptranslate"></a>[<span data-ttu-id="cc944-130">JavaScript</span><span class="sxs-lookup"><span data-stu-id="cc944-130">JavaScript</span></span>](#tab/jssetuptranslate)

<span data-ttu-id="cc944-131">Para configurar el software intermedio de traducción con un bot de eco, pegue lo siguiente en app.js.</span><span class="sxs-lookup"><span data-stu-id="cc944-131">To set up translation middleware with an echo bot, paste the following into app.js.</span></span>

```javascript
const { BotFrameworkAdapter, MemoryStorage, ConversationState } = require('botbuilder');
const { LanguageTranslator, LocaleConverter } = require('botbuilder-ai');
const restify = require('restify');

// Create server
let server = restify.createServer();
server.listen(process.env.port || process.env.PORT || 3978, function () {
    console.log(`${server.name} listening to ${server.url}`);
});

// Create adapter
const adapter = new BotFrameworkAdapter({ 
    appId: process.env.MICROSOFT_APP_ID, 
    appPassword: process.env.MICROSOFT_APP_PASSWORD 
});

// Add conversation state middleware
const conversationState = new ConversationState(new MemoryStorage());
adapter.use(conversationState);

// Add language translator middleware
const languageTranslator = new LanguageTranslator({
    translatorKey: "xxxxxx",
    noTranslatePatterns: new Set(),
    nativeLanguages: ['fr', 'de'] 
});
adapter.use(languageTranslator);


// Listen for incoming requests 
server.post('/api/messages', (req, res) => {
    // Route received request to adapter for processing
    adapter.processActivity(req, res, (context) => {
        if (context.activity.type === 'message') {
            const state = conversationState.get(context);
            const count = state.count === undefined ? state.count = 0 : ++state.count;
            return context.sendActivity(`${count}: You said "${context.activity.text}"`);
        } else {
            return context.sendActivity(`[${context.activity.type} event detected]`);
        }
    });
});
```

---

## <a name="run-the-bot-and-see-translated-input"></a><span data-ttu-id="cc944-132">Ejecutar el bot y ver la entrada traducida</span><span class="sxs-lookup"><span data-stu-id="cc944-132">Run the bot and see translated input</span></span>

<span data-ttu-id="cc944-133">Ejecute el bot y escriba algunos mensajes en otros idiomas.</span><span class="sxs-lookup"><span data-stu-id="cc944-133">Run the bot, and type in a few messages in other languages.</span></span> <span data-ttu-id="cc944-134">Verá que el bot ha traducido el mensaje de usuario e indica la traducción en su respuesta.</span><span class="sxs-lookup"><span data-stu-id="cc944-134">You'll see that the bot translated the user message and indicates the translation in its response.</span></span>

![el bot detecta el idioma y traduce la entrada](./media/how-to-bot-translate/bot-detects-language-translates-input.png)




## <a name="invoke-logic-in-the-bots-native-language"></a><span data-ttu-id="cc944-136">Invocar lógica en el lenguaje nativo del bot</span><span class="sxs-lookup"><span data-stu-id="cc944-136">Invoke logic in the bot's native language</span></span>

<span data-ttu-id="cc944-137">Ahora, agregue la lógica que busque palabras en inglés.</span><span class="sxs-lookup"><span data-stu-id="cc944-137">Now, add logic that checks for English words.</span></span> <span data-ttu-id="cc944-138">Si el usuario dice "ayuda" o "cancelar" en un idioma que no sea inglés, el bot lo traduce al inglés y se invoca la lógica que busca las palabras en inglés "help" o "cancel".</span><span class="sxs-lookup"><span data-stu-id="cc944-138">If the user says "help" or "cancel" in another language, the bot translates it into English and the logic that checks for the english words "help" or "cancel" is invoked.</span></span>

# <a name="ctabcshelp"></a>[<span data-ttu-id="cc944-139">C#</span><span class="sxs-lookup"><span data-stu-id="cc944-139">C#</span></span>](#tab/cshelp)
```cs
        case ActivityTypes.Message:
            // check the message text before calling context.SendActivity
            var messagetext = context.Activity.Text.Trim().ToLower();
            if (string.Equals("help", messagetext))
            {
                await context.SendActivity("You asked for help.");
            }
```

# <a name="javascripttabjshelp"></a>[<span data-ttu-id="cc944-140">JavaScript</span><span class="sxs-lookup"><span data-stu-id="cc944-140">JavaScript</span></span>](#tab/jshelp)
```javascript
if (context.activity.type === 'message') {
    // check the message text before calling context.sendActivity
    var messagetext = context.activity.text.trim().toLowerCase();
    if(messagetext === "help"){
        await context.sendActivity("you said help");
    }
}
```

---

![el bot detecta la palabra "ayuda" en francés](./media/how-to-bot-translate/bot-detects-help-french.png)



## <a name="translate-replies-back-to-the-users-language"></a><span data-ttu-id="cc944-142">Traducir las respuestas de nuevo al idioma del usuario</span><span class="sxs-lookup"><span data-stu-id="cc944-142">Translate replies back to the user's language</span></span>

<span data-ttu-id="cc944-143">También puede volver a traducir las respuestas al idioma del usuario. Para ello, establezca el último parámetro de constructor en `true`.</span><span class="sxs-lookup"><span data-stu-id="cc944-143">You can also translate replies back to the user's language, by setting the last constructor parameter to `true`.</span></span>

# <a name="ctabcstranslateback"></a>[<span data-ttu-id="cc944-144">C#</span><span class="sxs-lookup"><span data-stu-id="cc944-144">C#</span></span>](#tab/cstranslateback)
```cs
// Use language recognition to detect the user's language from their message, instead of providing helper callbacks.
// Last parameter indicates that we'll translate replies back to the user's language
middleware.Add(new TranslationMiddleware(new string[] { "en" }, "TRANSLATION-SUBSCRIPTION-KEY", true));
```

# <a name="javascripttabjstranslateback"></a>[<span data-ttu-id="cc944-145">JavaScript</span><span class="sxs-lookup"><span data-stu-id="cc944-145">JavaScript</span></span>](#tab/jstranslateback)
```javascript
// Use language recognition to detect the user's language from their message, instead of providing helper callbacks.
// Last parameter indicates that we'll translate replies back to the user's language
const languageTranslator = new LanguageTranslator({
    translatorKey: "xxxxxx",
    noTranslatePatterns: new Set(),
    nativeLanguages: ['en'],
    translateBackToUserLanguage: true
});
adapter.use(languageTranslator);
```

---

## <a name="run-the-bot-to-see-replies-in-the-users-language"></a><span data-ttu-id="cc944-146">Ejecutar el bot para ver las respuestas en el idioma del usuario</span><span class="sxs-lookup"><span data-stu-id="cc944-146">Run the bot to see replies in the user's language</span></span>

<span data-ttu-id="cc944-147">Ejecute el bot y escriba algunos mensajes en otros idiomas.</span><span class="sxs-lookup"><span data-stu-id="cc944-147">Run the bot, and type in a few messages in other languages.</span></span> <span data-ttu-id="cc944-148">Verá que detecta el idioma del usuario y traduce la respuesta.</span><span class="sxs-lookup"><span data-stu-id="cc944-148">You'll see that it detects the user's language and translates the response.</span></span>

![el bot detecta el idioma y traduce la respuesta](./media/how-to-bot-translate/bot-detects-language-translates-response.png)


## <a name="adding-logic-for-detecting-or-changing-the-user-language"></a><span data-ttu-id="cc944-150">Agregar lógica para detectar o cambiar el idioma del usuario</span><span class="sxs-lookup"><span data-stu-id="cc944-150">Adding logic for detecting or changing the user language</span></span>

<span data-ttu-id="cc944-151">En lugar de dejar que Botbuilder SDK detecte automáticamente el idioma del usuario, puede proporcionar devoluciones de llamada para agregar su propia lógica a fin de determinar el idioma del usuario, o bien determinar cuándo ha cambiado el idioma del usuario.</span><span class="sxs-lookup"><span data-stu-id="cc944-151">Instead of letting the Botbuilder SDK automatically detect the user's language, you can provide callbacks to add your own logic for determining the user's language, or determining when the user's language has changed.</span></span>

> [!TIP] 
> <span data-ttu-id="cc944-152">Vea el ejemplo de traducción del SDK para obtener un bot de ejemplo que implementa devoluciones de llamada para detectar y cambiar el idioma del usuario.</span><span class="sxs-lookup"><span data-stu-id="cc944-152">See the Translation sample in the SDK for a sample bot that implements callbacks for detecting and changing the user language.</span></span> 

<span data-ttu-id="cc944-153">En el ejemplo siguiente, la devolución de llamada `CheckUserChangedLanguage` comprueba si hay un mensaje de usuario específico para cambiar el idioma.</span><span class="sxs-lookup"><span data-stu-id="cc944-153">In the following example, the `CheckUserChangedLanguage` callback checks for a specific user message to change the language.</span></span> 

# <a name="ctabcschangelanguage"></a>[<span data-ttu-id="cc944-154">C#</span><span class="sxs-lookup"><span data-stu-id="cc944-154">C#</span></span>](#tab/cschangelanguage)
```cs
// In Startup.cs
middleware.Add(new TranslationMiddleware(new string[] { "en" }, "<YOUR MICROSOFT TRANSLATOR API KEY>", null, TranslatorLocaleHelper.GetActiveLanguage, TranslatorLocaleHelper.CheckUserChangedLanguage));

// Add a file TranslatorLocalHelper.cs with the following:

    class CurrentUserState
    {
        public string Language { get; set; }
    }

    public static void SetLanguage(ITurnContext context, string language) => context.GetConversationState<CurrentUserState>().Language = language;

    public static bool IsSupportedLanguage(string language) => _supportedLanguages.Contains(language);

    public static async Task<bool> CheckUserChangedLanguage(ITurnContext context)
    {
        bool changeLang = false;
        
        // use a specific message from user to change language
        if (context.Activity.Type == ActivityTypes.Message)
        {
            var messageActivity = context.Activity.AsMessageActivity();
            if (messageActivity.Text.ToLower().StartsWith("set my language to"))
            {
                changeLang = true;
            }
            if (changeLang)
            {
                var newLang = messageActivity.Text.ToLower().Replace("set my language to", "").Trim();
                if (!string.IsNullOrWhiteSpace(newLang)
                        && IsSupportedLanguage(newLang))
                {
                    SetLanguage(context, newLang);
                    await context.SendActivity($@"Changing your language to {newLang}");
                }
                else
                {
                    await context.SendActivity($@"{newLang} is not a supported language.");
                }
                // return true to intercept message from further processesing
                return true;
            }
        }

        return false;
    }

    public static string GetActiveLanguage(ITurnContext context)
    {

        if (context.Activity.Type == ActivityTypes.Message
            && context.GetConversationState<CurrentUserState>() != null && context.GetConversationState<CurrentUserState>().Language != null)
        {
            return context.GetConversationState<CurrentUserState>().Language;
        }

        return "en";
    }

```

# <a name="javascripttabjschangelanguage"></a>[<span data-ttu-id="cc944-155">JavaScript</span><span class="sxs-lookup"><span data-stu-id="cc944-155">JavaScript</span></span>](#tab/jschangelanguage)
```javascript
// When the user inputs 'set my language to fr'
// The bot will automatically change all text to French
async function setUserLanguage(context) {
    let state = conversationState.get(context)
    if (context.activity.text.toLowerCase().startsWith('set my language to')) {
        state.language = context.activity.text.toLowerCase().replace('set my language to', '').trim();
        await context.sendActivity(`Setting your language to ${state.language}`);
        return Promise.resolve(true);
    } else {
        return Promise.resolve(false);
    }
}

// Add language translator middleware
const languageTranslator = new LanguageTranslator({
    translatorKey: "<YOUR MICROSOFT TRANSLATOR API KEY>",
    noTranslatePatterns: new Set(),
    nativeLanguages: ['en'],
    setUserLanguage: setUserLanguage,
    translateBackToUserLanguage: true
});
adapter.use(languageTranslator);

// Listen for incoming activity 
server.post('/api/messages', (req, res) => {
    // Route received activity to adapter for processing
    adapter.processActivity(req, res, async (context) => {
        if (context.activity.type != 'message') {
            await context.sendActivity(`[${context.activity.type} event detected]`);
        } else {
            await context.sendActivity(`You said: ${context.activity.text}`)
        }
    });
});

```

---

## <a name="combining-luis-or-qna-with-translation"></a><span data-ttu-id="cc944-156">Combinar LUIS o QnA con la traducción</span><span class="sxs-lookup"><span data-stu-id="cc944-156">Combining LUIS or QnA with translation</span></span>

<span data-ttu-id="cc944-157">Si combina la traducción con otros servicios en el bot, como LUIS o QnA Maker, agregue primero el software intermedio de traducción para que los mensajes se traduzcan antes de pasarlos a otro software intermedio que espera el lenguaje nativo del bot.</span><span class="sxs-lookup"><span data-stu-id="cc944-157">If you're combining translation with other services in your bot, like LUIS or QnA maker, add the translation middleware first, so that the messages are translated before passing them to other middleware that expects the bot's native language.</span></span>

# <a name="ctabcslanguageluis"></a>[<span data-ttu-id="cc944-158">C#</span><span class="sxs-lookup"><span data-stu-id="cc944-158">C#</span></span>](#tab/cslanguageluis)
```cs
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton(_ => Configuration);
    services.AddBot<AiBot>(options =>
    {
        options.CredentialProvider = new ConfigurationCredentialProvider(Configuration);
        var middleware = options.Middleware;

        // Add translation middleware
        // The first parameter is a list of languages the bot recognizes. Input in these language won't be translated.
        middleware.Add(new TranslationMiddleware(new string[] { "en" }, "<YOUR MICROSOFT TRANSLATOR API KEY>", false));

        // Add LUIS middleware after translation middleware to pass translated messages to LUIS
        middleware.Add(
            new LuisRecognizerMiddleware(
                new LuisModel("luisAppId", "subscriptionId", new Uri("luisModelBaseUrl"))));
    });
}
```

# <a name="javascripttabjslanguageluis"></a>[<span data-ttu-id="cc944-159">JavaScript</span><span class="sxs-lookup"><span data-stu-id="cc944-159">JavaScript</span></span>](#tab/jslanguageluis)
```javascript
// Add language translator middleware
const languageTranslator = new LanguageTranslator({
    translatorKey: "xxxxxx",
    noTranslatePatterns: new Set(),
    nativeLanguages: ['en'],
    translateBackToUserLanguage: false
});
adapter.use(languageTranslator);

// Add Luis recognizer middleware
const luisRecognizer = new LuisRecognizer({
    appId: "xxxxxx",
    subscriptionKey: "xxxxxxx"
});
adapter.use(luisRecognizer);


// Listen for incoming requests 
server.post('/api/messages', (req, res) => {
    // Route received request to adapter for processing
    adapter.processActivity(req, res, async (context) => {
        if (context.activity.type != 'message') {
            await context.sendActivity(`[${context.activity.type} event detected]`);
        } else {
            let results = luisRecognizer.get(context);
            for (const intent in results.intents) {
                await context.sendActivity(`intent: ${intent.toString()} : ${results.intents[intent.toString()]}`)
            }
        }
    });
});

```

---

<span data-ttu-id="cc944-160">En el código del bot, los resultados de LUIS se basan en la entrada que ya se ha traducido al lenguaje nativo del bot.</span><span class="sxs-lookup"><span data-stu-id="cc944-160">In your bot code, the LUIS results are based on input that has already been translated to the bot's native language.</span></span> <span data-ttu-id="cc944-161">Pruebe a modificar el código del bot para comprobar los resultados de una aplicación de LUIS:</span><span class="sxs-lookup"><span data-stu-id="cc944-161">Try modifying the bot code to check the results of a LUIS app:</span></span>

```cs
public async Task OnTurn(ITurnContext context)
{
    switch (context.Activity.Type)
    {
        case ActivityTypes.Message:
            // check LUIS results
            var luisResult = context.Services.Get<RecognizerResult>(LuisRecognizerMiddleware.LuisRecognizerResultKey);
            (string key, double score) topItem = luisResult.GetTopScoringIntent();
            await context.SendActivity($"The top intent was: '{topItem.key}', with score {topItem.score}");

            await context.SendActivity($"You sent '{context.Activity.Text}'");

        case ActivityTypes.ConversationUpdate:
            foreach (var newMember in context.Activity.MembersAdded)
            {
                if (newMember.Id != context.Activity.Recipient.Id)
                {
                    await context.SendActivity("Hello and welcome to the echo bot.");
                }
            }
            break;
        }
    }  
}          
```

## <a name="bypass-translation-for-specified-patterns"></a><span data-ttu-id="cc944-162">Omitir la traducción de patrones especificados</span><span class="sxs-lookup"><span data-stu-id="cc944-162">Bypass translation for specified patterns</span></span>
<span data-ttu-id="cc944-163">Es posible que no le interese que el bot traduzca ciertas palabras, como los nombres propios.</span><span class="sxs-lookup"><span data-stu-id="cc944-163">There may be certain words you don't want your bot to translate, such as proper names.</span></span> <span data-ttu-id="cc944-164">Puede proporcionar expresiones regulares para indicar los patrones que no hay que traducir.</span><span class="sxs-lookup"><span data-stu-id="cc944-164">You can provide regular expressions to indicate patterns that shouldn't be translated.</span></span> <span data-ttu-id="cc944-165">Por ejemplo, si el usuario dice "Me llamo…" en un lenguaje no nativo para el bot y quiere impedir que su nombre se traduzca, puede usar un patrón para especificarlo.</span><span class="sxs-lookup"><span data-stu-id="cc944-165">For example, if the user says "My name is ..." in a non-native language for your bot, and you want to avoid translating their name, you can use a pattern to specify that.</span></span>

# <a name="ctabcsbypass"></a>[<span data-ttu-id="cc944-166">C#</span><span class="sxs-lookup"><span data-stu-id="cc944-166">C#</span></span>](#tab/csbypass)
```cs
    // Startup.cs

    // Pattern representing input to not translate
    Dictionary<string, List<string>> patterns = new Dictionary<string, List<string>>();
    // single pattern for fr language, to avoid translating what follows "my name is"
    patterns.Add("fr", new List<string> { "mon nom est (.+)" });
    
    middleware.Add(new TranslationMiddleware(new string[] { "en" }, "<YOUR API KEY>", patterns, TranslatorLocaleHelper.GetActiveLanguage, TranslatorLocaleHelper.CheckUserChangedLanguage));

```
<!-- TODO: ADD more explanation (both of these callbacks are run every time), fix image by debugging regex for l'etat -->

# <a name="javascripttabjsbypass"></a>[<span data-ttu-id="cc944-167">JavaScript</span><span class="sxs-lookup"><span data-stu-id="cc944-167">JavaScript</span></span>](#tab/jsbypass)
```javascript
// Add language translator middleware
const languageTranslator = new LanguageTranslator({
    translatorKey: "<YOUR API KEY>",
    // single pattern for fr language, to avoid translating what follows "my name is"
    noTranslatePatterns: new Set("fr", "/mon nom est (.+)/"),
    nativeLanguages: ['en'],
    translateBackToUserLanguage: false,
    
});
adapter.use(languageTranslator);

```

---

![el bot omite la traducción de un patrón](./media/how-to-bot-translate/bot-no-translate-name-fr.png)

## <a name="localize-dates"></a><span data-ttu-id="cc944-169">Localizar fechas</span><span class="sxs-lookup"><span data-stu-id="cc944-169">Localize dates</span></span>

<span data-ttu-id="cc944-170">Si necesita localizar fechas, puede agregar `LocaleConverterMiddleware`.</span><span class="sxs-lookup"><span data-stu-id="cc944-170">If need to localize dates, you can add `LocaleConverterMiddleware`.</span></span> <span data-ttu-id="cc944-171">Por ejemplo, si sabe que el bot espera las fechas en el formato `MM/DD/YYYY` y es posible que los usuarios de otras configuraciones regionales escriban las fechas en el formato `DD/MM/YYYY`, el software intermedio del convertidor de configuración regional puede convertir automáticamente las fechas al formato que espera el bot.</span><span class="sxs-lookup"><span data-stu-id="cc944-171">For example, if you know that your bot expects dates in the format `MM/DD/YYYY`, and users in other locales might enter dates in the format `DD/MM/YYYY`, the locale converter middleware can automatically convert dates to the format your bot expects.</span></span>

> [!NOTE]
> <span data-ttu-id="cc944-172">El software intermedio del convertidor de configuración regional está pensado para convertir solo fechas.</span><span class="sxs-lookup"><span data-stu-id="cc944-172">The locale converter middleware is intended to convert only dates.</span></span> <span data-ttu-id="cc944-173">No tiene ningún conocimiento sobre los resultados del software intermedio de traducción.</span><span class="sxs-lookup"><span data-stu-id="cc944-173">It has no knowledge of the results of translation middleware.</span></span> <span data-ttu-id="cc944-174">Si usa software intermedio de traducción, tenga cuidado al combinarlo con el convertidor de configuración regional.</span><span class="sxs-lookup"><span data-stu-id="cc944-174">If you're using translation middleware, be careful how you combine it with locale converter.</span></span> <span data-ttu-id="cc944-175">El software intermedio de traducción traduce las fechas que están en formato de texto junto con el resto de la entrada de texto, pero no traduce otras fechas.</span><span class="sxs-lookup"><span data-stu-id="cc944-175">Translation middleware will translate some dates that are in textual format along with other text input, but it doesn't translate dates</span></span>

<span data-ttu-id="cc944-176">Por ejemplo, en la imagen siguiente se muestra un bot que devuelve la entrada de usuario después de traducirla de inglés a francés.</span><span class="sxs-lookup"><span data-stu-id="cc944-176">For example, the following image shows a bot that echos back the user input, after translating from English to French.</span></span> <span data-ttu-id="cc944-177">Usa `TranslationMiddleware` sin usar `LocaleConverterMiddleware`.</span><span class="sxs-lookup"><span data-stu-id="cc944-177">It uses `TranslationMiddleware` without using `LocaleConverterMiddleware`.</span></span>

![bot que traduce las fechas sin conversión de fechas](./media/how-to-bot-translate/locale-date-before.png)

<span data-ttu-id="cc944-179">A continuación se muestra el mismo bot con `LocaleConverterMiddleware` agregado.</span><span class="sxs-lookup"><span data-stu-id="cc944-179">The following shows the same bot if the `LocaleConverterMiddleware` is added.</span></span>

![bot que traduce las fechas sin conversión de fechas](./media/how-to-bot-translate/locale-date-after.png)

<span data-ttu-id="cc944-181">Los convertidores de configuración regional son compatibles con las configuraciones regionales de inglés, francés, alemán y chino.</span><span class="sxs-lookup"><span data-stu-id="cc944-181">Locale converters can support English, French, German, and Chinese locales.</span></span> <!-- TODO: ADD DETAIL ABOUT SUPPORTED LOCALES -->

# <a name="ctabcstranslation"></a>[<span data-ttu-id="cc944-182">C#</span><span class="sxs-lookup"><span data-stu-id="cc944-182">C#</span></span>](#tab/cstranslation)
```cs
// Startup.cs
// Add locale converter middleware
middleware.Add(new LocaleConverterMiddleware(TranslatorLocaleHelper.GetActiveLocale, TranslatorLocaleHelper.CheckUserChangedLocale, "en-us", LocaleConverter.Converter));

// TranslatorLocaleHelper.cs
        public static async Task<bool> CheckUserChangedLocale(ITurnContext context)
        {
            bool changeLocale = false;//logic implemented by developper to make a signal for language changing 
            //use a specific message from user to change language
            if (context.Activity.Type == ActivityTypes.Message)
            {
                var messageActivity = context.Activity.AsMessageActivity();
                if (messageActivity.Text.ToLower().StartsWith("set my locale to"))
                {
                    changeLocale = true;
                }
                if (changeLocale)
                {
                    var newLocale = messageActivity.Text.ToLower().Replace("set my locale to", "").Trim(); //extracted by the user using user state 
                    if (!string.IsNullOrWhiteSpace(newLocale)
                            && IsSupportedLanguage(newLocale))
                    {
                        SetLocale(context, newLocale);
                        await context.SendActivity($@"Changing your language to {newLocale}");
                    }
                    else
                    {
                        await context.SendActivity($@"{newLocale} is not a supported locale.");
                    }
                    //intercepts message
                    return true;
                }
            }

            return false;
        }
        public static string GetActiveLocale(ITurnContext context)
        {
            if (currentLocale != null)
            {
                //the user has specified a different locale so update the bot state
                if (context.GetConversationState<CurrentUserState>() != null
                    && currentLocale != context.GetConversationState<CurrentUserState>().Locale)
                {
                    SetLocale(context, currentLocale);
                }
            }
            if (context.Activity.Type == ActivityTypes.Message
                && context.GetConversationState<CurrentUserState>() != null && context.GetConversationState<CurrentUserState>().Locale != null)
            {
                return context.GetConversationState<CurrentUserState>().Locale;
            }

            return "en-us";
        }
```

# <a name="javascripttabjstranslation"></a>[<span data-ttu-id="cc944-183">JavaScript</span><span class="sxs-lookup"><span data-stu-id="cc944-183">JavaScript</span></span>](#tab/jstranslation)

<!-- this snippet only works if the user doesn't actually try to change their locale.  Emailed Mostafa about the issue 
It should change the locale after you type in 'set my locale to....' -->

```javascript
// Delegates for getting and setting user locale
function getUserLocale(context) {
    const state = conversationState.get(context)
    if (state.locale == undefined) {
        return 'en-us';
    } else {
        return state.locale;
    }
}

// Function to change the user's locale.
async function setUserLocale(context) {
    let state = conversationState.get(context)
    if (context.activity.text.toLowerCase().startsWith('set my locale to')) {        
        state.locale = context.activity.text.toLowerCase().replace('set my locale to', '').trim();
        await context.sendActivity(`Setting your locale to ${state.locale}`);
        return Promise.resolve(true);
    } else {
        return Promise.resolve(false);
    }
}

// Add locale converter middleware
// Will convert input to fr-fr
const localeConverter = new LocaleConverter({
    toLocale: 'fr-fr',
    setUserLocale: setUserLocale,
    getUserLocale: getUserLocale
});
adapter.use(localeConverter);


// Listen for incoming requests 
server.post('/api/messages', (req, res) => {
    // Route received request to adapter for processing
    adapter.processActivity(req, res, async (context) => {
        if (context.activity.type != 'message') {
            await context.sendActivity(`[${context.activity.type} event detected]`);
        } else {
            await context.sendActivity(`The message that you sent was:  ${context.activity.text}`)
        }
    });
});

```

---
