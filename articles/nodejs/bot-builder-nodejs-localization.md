---
title: Compatibilidad con la localización | Microsoft Docs
description: Aprenda a determinar dónde se encuentra el usuario y a habilitar la funcionalidad de localización mediante Bot Builder SDK para Node.js.
author: DeniseMak
ms.author: v-demak
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: 8f58c8d0b884e929c575e01aa5e0fee5c21ea4e6
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305953"
---
# <a name="support-localization"></a><span data-ttu-id="6dc9d-103">Compatibilidad con la localización</span><span class="sxs-lookup"><span data-stu-id="6dc9d-103">Support localization</span></span>

<span data-ttu-id="6dc9d-104">Bot Builder incluye un completo sistema de localización para la creación de bots que pueden comunicarse con el usuario en varios idiomas.</span><span class="sxs-lookup"><span data-stu-id="6dc9d-104">Bot Builder includes a rich localization system for building bots that can communicate with the user in multiple languages.</span></span> <span data-ttu-id="6dc9d-105">Todos los avisos del bot se pueden localizar mediante archivos JSON almacenados en la estructura de directorios del bot.</span><span class="sxs-lookup"><span data-stu-id="6dc9d-105">All of your bot's prompts can be localized using JSON files stored in your bot's directory structure.</span></span> <span data-ttu-id="6dc9d-106">Si usa un sistema como LUIS para realizar el procesamiento de lenguaje natural, puede configurar [LuisRecognizer][LUISRecognizer] con un modelo independiente para cada idioma que admita su bot, y el SDK seleccionará automáticamente el modelo que coincida con la configuración regional preferida del usuario.</span><span class="sxs-lookup"><span data-stu-id="6dc9d-106">If you’re using a system like LUIS to perform natural language processing, you can configure your [LuisRecognizer][LUISRecognizer] with a separate model for each language your bot supports and the SDK will automatically select the model that matches the user's preferred locale.</span></span>

## <a name="determine-the-locale-by-prompting-the-user"></a><span data-ttu-id="6dc9d-107">Preguntar al usuario para determinar la configuración regional</span><span class="sxs-lookup"><span data-stu-id="6dc9d-107">Determine the locale by prompting the user</span></span>
<span data-ttu-id="6dc9d-108">El primer paso para localizar su bot es agregar la posibilidad de identificar el idioma preferido del usuario.</span><span class="sxs-lookup"><span data-stu-id="6dc9d-108">The first step to localizing your bot is adding the ability to identify the user's preferred language.</span></span> <span data-ttu-id="6dc9d-109">El SDK proporciona un método [session.preferredLocale()][preferredLocal] para guardar y recuperar esta preferencia de forma individualizada.</span><span class="sxs-lookup"><span data-stu-id="6dc9d-109">The SDK provides a [session.preferredLocale()][preferredLocal] method to both save and retrieve this preference on a per-user basis.</span></span> <span data-ttu-id="6dc9d-110">El ejemplo siguiente es un diálogo que pregunta al usuario su idioma preferido y, a continuación, guarda su elección.</span><span class="sxs-lookup"><span data-stu-id="6dc9d-110">The following example is a dialog to prompt the user for their preferred language and then save their choice.</span></span>

``` javascript
bot.dialog('/localePicker', [
    function (session) {
        // Prompt the user to select their preferred locale
        builder.Prompts.choice(session, "What's your preferred language?", 'English|Español|Italiano');
    },
    function (session, results) {
        // Update preferred locale
        var locale;
        switch (results.response.entity) {
            case 'English':
                locale = 'en';
                break;
            case 'Español':
                locale = 'es';
                break;
            case 'Italiano':
                locale = 'it';
                break;
        }
        session.preferredLocale(locale, function (err) {
            if (!err) {
                // Locale files loaded
                session.endDialog(`Your preferred language is now ${results.response.entity}`);
            } else {
                // Problem loading the selected locale
                session.error(err);
            }
        });
    }
]);
```

## <a name="determine-the-locale-by-using-analytics"></a><span data-ttu-id="6dc9d-111">Determinación de la configuración regional mediante análisis</span><span class="sxs-lookup"><span data-stu-id="6dc9d-111">Determine the locale by using analytics</span></span>
<span data-ttu-id="6dc9d-112">Otra manera de determinar la configuración regional del usuario es usar un servicio como [Text Analytics API](/azure/cognitive-services/cognitive-services-text-analytics-quick-start) para detectar automáticamente el lenguaje del usuario según el texto del mensaje que envía.</span><span class="sxs-lookup"><span data-stu-id="6dc9d-112">Another way to determine the user's locale is to use a service like the [Text Analytics API](/azure/cognitive-services/cognitive-services-text-analytics-quick-start) to automatically detect the user's language based upon the text of the message they sent.</span></span>

<span data-ttu-id="6dc9d-113">El siguiente fragmento de código muestra cómo se puede incorporar este servicio a su propio bot.</span><span class="sxs-lookup"><span data-stu-id="6dc9d-113">The code snippet below illustrates how you can incorporate this service into your own bot.</span></span>
``` javascript
var request = require('request');

bot.use({
    receive: function (event, next) {
        if (event.text && !event.textLocale) {
            var options = {
                method: 'POST',
                url: 'https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/languages?numberOfLanguagesToDetect=1',
                body: { documents: [{ id: 'message', text: event.text }]},
                json: true,
                headers: {
                    'Ocp-Apim-Subscription-Key': '<YOUR API KEY>'
                }
            };
            request(options, function (error, response, body) {
                if (!error && body) {
                    if (body.documents && body.documents.length > 0) {
                        var languages = body.documents[0].detectedLanguages;
                        if (languages && languages.length > 0) {
                            event.textLocale = languages[0].iso6391Name;
                        }
                    }
                }
                next();
            });
        } else {
            next();
        }
    }
});
```

<span data-ttu-id="6dc9d-114">Una vez que agregue el fragmento de código anterior al bot, la llamada a [session.preferredLocale()][preferredLocal] devolverá automáticamente el idioma detectado.</span><span class="sxs-lookup"><span data-stu-id="6dc9d-114">Once you add the above code snippet to your bot, calling [session.preferredLocale()][preferredLocal] will automatically return the detected language.</span></span> <span data-ttu-id="6dc9d-115">El orden de búsqueda de `preferredLocale()` es así:</span><span class="sxs-lookup"><span data-stu-id="6dc9d-115">The search order for `preferredLocale()` is as follows:</span></span>
1. <span data-ttu-id="6dc9d-116">Configuración regional guardada mediante la llamada a `session.preferredLocale()`.</span><span class="sxs-lookup"><span data-stu-id="6dc9d-116">Locale saved by calling `session.preferredLocale()`.</span></span> <span data-ttu-id="6dc9d-117">Este valor se almacena en `session.userData['BotBuilder.Data.PreferredLocale']`.</span><span class="sxs-lookup"><span data-stu-id="6dc9d-117">This value is stored in `session.userData['BotBuilder.Data.PreferredLocale']`.</span></span>
2. <span data-ttu-id="6dc9d-118">Configuración regional detectada asignada a `session.message.textLocale`.</span><span class="sxs-lookup"><span data-stu-id="6dc9d-118">Detected locale assigned to `session.message.textLocale`.</span></span>
3. <span data-ttu-id="6dc9d-119">La configuración regional predeterminada configurada para el bot (p. ej.: inglés ["en"]).</span><span class="sxs-lookup"><span data-stu-id="6dc9d-119">The configured default locale for the bot (e.g.: English (‘en’)).</span></span>

<span data-ttu-id="6dc9d-120">Puede configurar la configuración regional predeterminada del bot mediante su constructor:</span><span class="sxs-lookup"><span data-stu-id="6dc9d-120">You can configure the bot's default locale using its constructor:</span></span>

```javascript
var bot = new builder.UniversalBot(connector, {
    localizerSettings: { 
        defaultLocale: "es" 
    }
});
```

## <a name="localize-prompts"></a><span data-ttu-id="6dc9d-121">Localización de los avisos</span><span class="sxs-lookup"><span data-stu-id="6dc9d-121">Localize prompts</span></span>
<span data-ttu-id="6dc9d-122">El sistema de localización predeterminado de Bot Builder SDK está basado en archivos y permite que un bot admita varios idiomas por medio de archivos JSON almacenados en discos.</span><span class="sxs-lookup"><span data-stu-id="6dc9d-122">The default localization system for the Bot Builder SDK is file-based and allows a bot to support multiple languages using JSON files stored on disk.</span></span> <span data-ttu-id="6dc9d-123">De forma predeterminada, el sistema de localización buscará los avisos del bot en el archivo **./locale/<IETF TAG>/index.json** donde <IETF TAG> es una [etiqueta de idioma IETF][IEFT] válida que representa la configuración regional preferida para la que se buscarán los avisos.</span><span class="sxs-lookup"><span data-stu-id="6dc9d-123">By default, the localization system will search for the bot's prompts in the **./locale/<IETF TAG>/index.json** file where <IETF TAG> is a valid [IETF language tag][IEFT] representing the preferred locale for which to find prompts.</span></span> 

<span data-ttu-id="6dc9d-124">En la captura de pantalla siguiente se muestra la estructura de directorio de un bot que admite tres idiomas: inglés, italiano y español.</span><span class="sxs-lookup"><span data-stu-id="6dc9d-124">The following screenshot shows the directory structure for a bot that supports three languages: English, Italian, and Spanish.</span></span>

![Estructura de directorio de tres configuraciones regionales](../media/locale-dir.png)

<span data-ttu-id="6dc9d-126">La estructura del archivo es una sencilla asignación JSON de identificadores de mensaje a cadenas de texto localizadas.</span><span class="sxs-lookup"><span data-stu-id="6dc9d-126">The structure of the file is a simple JSON map of message IDs to localized text strings.</span></span> <span data-ttu-id="6dc9d-127">Si el valor es una matriz en lugar de una cadena, se elige un aviso de la matriz de forma aleatoria cuando el valor se recupera mediante [session.localizer.gettext()][GetText].</span><span class="sxs-lookup"><span data-stu-id="6dc9d-127">If the value is an array instead of a string, one prompt from the array is chosen at random when that value is retrieved using [session.localizer.gettext()][GetText].</span></span> 

<span data-ttu-id="6dc9d-128">El bot recupera automáticamente la versión localizada de un mensaje si se pasa el identificador del mensaje en una llamada a [session.send()](http://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.session#send) en lugar de texto específico del idioma:</span><span class="sxs-lookup"><span data-stu-id="6dc9d-128">The bot automatically retrieves the localized version of a message if you pass the message ID in a call to [session.send()](http://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.session#send) instead of language-specific text:</span></span>

```javascript
var bot = new builder.UniversalBot(connector, [
    function (session) {
        session.send("greeting");
        session.send("instructions");
        session.beginDialog('/localePicker');
    },
    function (session) {
        builder.Prompts.text(session, "text_prompt");
    }
]);
```

<span data-ttu-id="6dc9d-129">Internamente, el SDK llama a [`session.preferredLocale()`][preferredLocale] para obtener la configuración regional preferida del usuario y luego la usa en una llamada a [`session.localizer.gettext()`][GetText] para asignar el identificador de mensaje a su cadena de texto localizada.</span><span class="sxs-lookup"><span data-stu-id="6dc9d-129">Internally, the SDK calls [`session.preferredLocale()`][preferredLocale] to get the user's preferred locale and then uses that in a call to [`session.localizer.gettext()`][GetText] to map the message ID to its localized text string.</span></span>  <span data-ttu-id="6dc9d-130">Hay ocasiones en que es posible que deba llamar manualmente al localizador.</span><span class="sxs-lookup"><span data-stu-id="6dc9d-130">There are times where you may need to manually call the localizer.</span></span> <span data-ttu-id="6dc9d-131">Por ejemplo, los valores de enumeración pasados a [`Prompts.choice()`][promptsChoice] nunca se localizan automáticamente, por lo que es posible que tenga que recuperar manualmente una lista localizada antes de llamar al aviso:</span><span class="sxs-lookup"><span data-stu-id="6dc9d-131">For instance, the enum values passed to [`Prompts.choice()`][promptsChoice] are never automatically localized so you may need to manually retrieve a localized list prior to calling the prompt:</span></span>

```javascript
var options = session.localizer.gettext(session.preferredLocale(), "choice_options");
builder.Prompts.choice(session, "choice_prompt", options);
```

<span data-ttu-id="6dc9d-132">El localizador predeterminado busca un identificador de mensaje en varios archivos y, si no encuentra uno (o no se han proporcionado archivos de localización), simplemente devuelve el texto del identificador, lo que hace que el uso de archivos de localización sea transparente y opcional.</span><span class="sxs-lookup"><span data-stu-id="6dc9d-132">The default localizer searches for a message ID across multiple files and if it can’t find an ID (or if no localization files were provided) it will simply return the text of ID, making the use of localization files transparent and optional.</span></span>  <span data-ttu-id="6dc9d-133">Se busca en los archivos en el orden siguiente:</span><span class="sxs-lookup"><span data-stu-id="6dc9d-133">Files are searched in the following order:</span></span>

1. <span data-ttu-id="6dc9d-134">Se busca en el archivo **index.json** con la configuración regional devuelta por [`session.preferredLocale()`][preferredLocale].</span><span class="sxs-lookup"><span data-stu-id="6dc9d-134">The **index.json** file under the locale returned by [`session.preferredLocale()`][preferredLocale] is searched.</span></span>
2. <span data-ttu-id="6dc9d-135">Si la configuración regional incluía una subetiqueta opcional como **en-US**, se busca en la etiqueta raíz de **en**.</span><span class="sxs-lookup"><span data-stu-id="6dc9d-135">If the locale included an optional subtag like **en-US** then the root tag of **en** is searched.</span></span>
3. <span data-ttu-id="6dc9d-136">Se busca en la configuración regional predeterminada configurada del bot.</span><span class="sxs-lookup"><span data-stu-id="6dc9d-136">The bot's configured default locale is searched.</span></span>

## <a name="use-namespaces-to-customize-and-localize-prompts"></a><span data-ttu-id="6dc9d-137">Uso de espacios de nombres para personalizar y localizar avisos</span><span class="sxs-lookup"><span data-stu-id="6dc9d-137">Use namespaces to customize and localize prompts</span></span>
<span data-ttu-id="6dc9d-138">El localizador predeterminado admite el uso de espacios de nombres de avisos para evitar conflictos entre identificadores de mensaje.</span><span class="sxs-lookup"><span data-stu-id="6dc9d-138">The default localizer supports the namespacing of prompts to avoid collisions between message IDs.</span></span>  <span data-ttu-id="6dc9d-139">El bot puede invalidar los avisos de un espacio de nombres para personalizar o reformular los avisos de otro espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="6dc9d-139">Your bot can override namespaced prompts to customize or reword the prompts from another namespace.</span></span>  <span data-ttu-id="6dc9d-140">Puede aprovechar esta funcionalidad para personalizar los mensajes integrados del SDK, y así agregar compatibilidad con idiomas adicionales o simplemente reformular los mensajes actuales del SDK.</span><span class="sxs-lookup"><span data-stu-id="6dc9d-140">You can leverage this capability to customize the SDK’s built-in messages, letting you either add support for additional languages or to simply reword the SDK's current messages.</span></span>  <span data-ttu-id="6dc9d-141">Por ejemplo, puede cambiar el mensaje de error predeterminado del SDK con solo agregar un archivo llamado **BotBuilder.json** al directorio de configuración regional del bot y luego agregar una entrada para el identificador de mensaje `default_error`:</span><span class="sxs-lookup"><span data-stu-id="6dc9d-141">For instance, you can change the SDK’s default error message by simply adding a file called **BotBuilder.json** to your bot's locale directory and then adding an entry for the `default_error` message ID:</span></span>

![BotBuilder.json para el uso de espacios de nombres de configuración regional](../media/locale-namespacing.png)


## <a name="additional-resources"></a><span data-ttu-id="6dc9d-143">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="6dc9d-143">Additional resources</span></span>

<span data-ttu-id="6dc9d-144">Para información sobre cómo localizar un reconocedor, consulte [Reconocimiento de intenciones](bot-builder-nodejs-recognize-intent-messages.md).</span><span class="sxs-lookup"><span data-stu-id="6dc9d-144">To learn about how to localize a recognizer, see [Recognizing intent](bot-builder-nodejs-recognize-intent-messages.md).</span></span>


[LUIS]: https://www.luis.ai/
[IMessage]: http://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.imessage
[IntentRecognizerSetOptions]: https://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.iintentrecognizersetoptions.html
[LUISRecognizer]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.luisrecognizer
[LUISSample]: https://github.com/Microsoft/BotBuilder/blob/master/Node/examples/basics-naturalLanguage/app.js
[DisambiguationSample]: https://github.com/Microsoft/BotBuilder/tree/master/Node/examples/feature-onDisambiguateRoute
[preferredLocal]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.session#preferredlocale
[preferredLocale]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.session#preferredlocale
[promptsChoice]: https://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.__global.iprompts.html#choice
[GetText]: https://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.ilocalizer.html#gettext
[IEFT]: https://en.wikipedia.org/wiki/IETF_language_tag

