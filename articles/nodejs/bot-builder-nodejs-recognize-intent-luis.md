---
title: Reconocimiento de intenciones y entidades con LUIS | Microsoft Docs
description: Integre un bot con LUIS para detectar la intención del usuario y responder según corresponda mediante el desencadenamiento de diálogos con Bot Builder SDK para Node.js.
author: DeniseMak
ms.author: v-demak
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 03/28/2018
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: 82a4d0843a9aaab25779d833f2b1b1d2ab2516c2
ms.sourcegitcommit: 2dc75701b169d822c9499e393439161bc87639d2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42905107"
---
# <a name="recognize-intents-and-entities-with-luis"></a><span data-ttu-id="50f9c-103">Reconocimiento de intenciones y entidades con LUIS</span><span class="sxs-lookup"><span data-stu-id="50f9c-103">Recognize intents and entities with LUIS</span></span> 

[!INCLUDE [pre-release-label](../includes/pre-release-label-v3.md)]

<span data-ttu-id="50f9c-104">Este artículo usa el ejemplo de un bot para tomar notas, a fin de demostrar cómo Language Understanding ([LUIS][LUIS]) ayuda a que el bot responda correctamente a la entrada de lenguaje natural.</span><span class="sxs-lookup"><span data-stu-id="50f9c-104">This article uses the example of a bot for taking notes, to demonstrate how Language Understanding ([LUIS][LUIS]) helps your bot respond appropriately to natural language input.</span></span> <span data-ttu-id="50f9c-105">Un bot detecta lo que un usuario desea hacer mediante la identificación de su **intención**.</span><span class="sxs-lookup"><span data-stu-id="50f9c-105">A bot detects what a user wants to do by identifying their **intent**.</span></span> <span data-ttu-id="50f9c-106">Esta intención se determina a partir de la entrada de texto, voz o **grabaciones de voz**.</span><span class="sxs-lookup"><span data-stu-id="50f9c-106">This intent is determined from spoken or textual input, or **utterances**.</span></span> <span data-ttu-id="50f9c-107">La intención asigna grabaciones de voz a acciones que realiza el bot, como invocar un diálogo.</span><span class="sxs-lookup"><span data-stu-id="50f9c-107">The intent maps utterances to actions that the bot takes, such as invoking a dialog.</span></span> <span data-ttu-id="50f9c-108">Puede que un bot también necesite extraer **entidades**, que son palabras importantes de las grabaciones de voz.</span><span class="sxs-lookup"><span data-stu-id="50f9c-108">A bot may also need to extract **entities**, which are important words in utterances.</span></span> <span data-ttu-id="50f9c-109">A veces, las entidades deben cumplir una intención.</span><span class="sxs-lookup"><span data-stu-id="50f9c-109">Sometimes entities are required to fulfill an intent.</span></span> <span data-ttu-id="50f9c-110">En el ejemplo de un bot de toma de notas, la entidad `Notes.Title` identifica el título de cada nota.</span><span class="sxs-lookup"><span data-stu-id="50f9c-110">In the example of a note-taking bot, the `Notes.Title` entity identifies the title of each note.</span></span>

## <a name="create-a-language-understanding-bot-with-bot-service"></a><span data-ttu-id="50f9c-111">Crear un bot de Language Understanding con Bot Service</span><span class="sxs-lookup"><span data-stu-id="50f9c-111">Create a Language Understanding bot with Bot Service</span></span>

1. <span data-ttu-id="50f9c-112">En [Azure Portal](https://portal.azure.com), haga clic en **Crear nuevo recurso** en la hoja de menú y, después, en **Ver todo**.</span><span class="sxs-lookup"><span data-stu-id="50f9c-112">In the [Azure portal](https://portal.azure.com), select **Create new resource** in the menu blade and click **See all**.</span></span>

    ![Crear recurso](../media/bot-builder-nodejs-use-luis/bot-service-creation.png)

2. <span data-ttu-id="50f9c-114">En el cuadro de búsqueda, busque **Bot de aplicación web**.</span><span class="sxs-lookup"><span data-stu-id="50f9c-114">In the search box, search for **Web App Bot**.</span></span> 

    ![Crear recurso](../media/bot-builder-nodejs-use-luis/bot-service-selection.png)

3. <span data-ttu-id="50f9c-116">En la hoja **Servicio de bots**, proporcione la información necesaria y haga clic en **Crear**.</span><span class="sxs-lookup"><span data-stu-id="50f9c-116">In the **Bot Service** blade, provide the required information, and click **Create**.</span></span> <span data-ttu-id="50f9c-117">Esto crea e implementa el servicio de bots y la aplicación de LUIS en Azure.</span><span class="sxs-lookup"><span data-stu-id="50f9c-117">This creates and deploys the bot service and LUIS app to Azure.</span></span> 
   * <span data-ttu-id="50f9c-118">Establezca **Nombre de la aplicación** en el nombre del bot.</span><span class="sxs-lookup"><span data-stu-id="50f9c-118">Set **App name** to your bot’s name.</span></span> <span data-ttu-id="50f9c-119">El nombre se usa como el subdominio cuando el bot se implementa en la nube (por ejemplo, mynotesbot.azurewebsites.net).</span><span class="sxs-lookup"><span data-stu-id="50f9c-119">The name is used as the subdomain when your bot is deployed to the cloud (for example, mynotesbot.azurewebsites.net).</span></span> <span data-ttu-id="50f9c-120">Este nombre también se utiliza como el nombre de la aplicación de LUIS asociada con el bot.</span><span class="sxs-lookup"><span data-stu-id="50f9c-120">This name is also used as the name of the LUIS app associated with your bot.</span></span> <span data-ttu-id="50f9c-121">Cópielo para usarlo más adelante, para encontrar la aplicación de LUIS asociada con el bot.</span><span class="sxs-lookup"><span data-stu-id="50f9c-121">Copy it to use later, to find the LUIS app associated with the bot.</span></span>
   * <span data-ttu-id="50f9c-122">Seleccione la suscripción, el [grupo de recursos](/azure/azure-resource-manager/resource-group-overview), el plan de App Service y la [ubicación](https://azure.microsoft.com/en-us/regions/).</span><span class="sxs-lookup"><span data-stu-id="50f9c-122">Select the subscription, [resource group](/azure/azure-resource-manager/resource-group-overview), App service plan, and [location](https://azure.microsoft.com/en-us/regions/).</span></span>
   * <span data-ttu-id="50f9c-123">Seleccione la plantilla **Language Understanding (Node.js)** en el campo **Bot template** (Plantilla de bot).</span><span class="sxs-lookup"><span data-stu-id="50f9c-123">Select the **Language understanding (Node.js)** template for the **Bot template** field.</span></span>

     ![Hoja Servicio de bots](../media/bot-builder-nodejs-use-luis/bot-service-setting-callout-template.png)

   * <span data-ttu-id="50f9c-125">Active la casilla para confirmar los términos del servicio.</span><span class="sxs-lookup"><span data-stu-id="50f9c-125">Check the box to confirm to the terms of service.</span></span>

4. <span data-ttu-id="50f9c-126">Confirme que se ha implementado el servicio de bots.</span><span class="sxs-lookup"><span data-stu-id="50f9c-126">Confirm that the bot service has been deployed.</span></span>
    * <span data-ttu-id="50f9c-127">Haga clic en Notificaciones (el icono de la campana que se encuentra en el borde superior de Azure Portal).</span><span class="sxs-lookup"><span data-stu-id="50f9c-127">Click Notifications (the bell icon that is located along the top edge of the Azure portal).</span></span> <span data-ttu-id="50f9c-128">La notificación cambiará de **Implementación iniciada** a **Implementación correcta**.</span><span class="sxs-lookup"><span data-stu-id="50f9c-128">The notification will change from **Deployment started** to **Deployment succeeded**.</span></span>
    * <span data-ttu-id="50f9c-129">Después de que la notificación cambie a **Implementación correcta**, haga clic en **Ir al recurso** en esa notificación.</span><span class="sxs-lookup"><span data-stu-id="50f9c-129">After the notification changes to **Deployment succeeded**, click **Go to resource** on that notification.</span></span>

## <a name="try-the-bot"></a><span data-ttu-id="50f9c-130">Probar el bot</span><span class="sxs-lookup"><span data-stu-id="50f9c-130">Try the bot</span></span>

<span data-ttu-id="50f9c-131">Para confirmar que el bot se ha implementado, active las **Notificaciones**.</span><span class="sxs-lookup"><span data-stu-id="50f9c-131">Confirm that the bot has been deployed by checking the **Notifications**.</span></span> <span data-ttu-id="50f9c-132">Las notificaciones cambiarán de **Implementación en curso...** a **Implementación correcta**.</span><span class="sxs-lookup"><span data-stu-id="50f9c-132">The notifications will change from **Deployment in progress...** to **Deployment succeeded**.</span></span> <span data-ttu-id="50f9c-133">Haga clic en el botón **Ir al recurso** para abrir la hoja de recursos del bot.</span><span class="sxs-lookup"><span data-stu-id="50f9c-133">Click **Go to resource** button to open the bot's resources blade.</span></span>

<span data-ttu-id="50f9c-134">Una vez registrado el bot, haga clic en **Test in Web Chat** (Probar en Chat en web) para abrir el panel Chat en web.</span><span class="sxs-lookup"><span data-stu-id="50f9c-134">Once the bot is registered, click **Test in Web Chat** to open the Web Chat pane.</span></span> <span data-ttu-id="50f9c-135">Escriba "hello" ("hola") en el Chat en web.</span><span class="sxs-lookup"><span data-stu-id="50f9c-135">Type "hello" in Web Chat.</span></span>

  ![Probar el bot en Chat en web](../media/bot-builder-nodejs-use-luis/bot-service-web-chat.png)

<span data-ttu-id="50f9c-137">El bot responde "You have reached Greeting.</span><span class="sxs-lookup"><span data-stu-id="50f9c-137">The bot responds by saying "You have reached Greeting.</span></span> <span data-ttu-id="50f9c-138">You said: hello" ("Se ha puesto en contacto con Saludo. Ha dicho: hola").</span><span class="sxs-lookup"><span data-stu-id="50f9c-138">You said: hello".</span></span> <span data-ttu-id="50f9c-139">Esto confirma que el bot ha recibido el mensaje y lo ha pasado a una aplicación de LUIS predeterminada que ha creado.</span><span class="sxs-lookup"><span data-stu-id="50f9c-139">This confirms that the bot has received your message and passed it to a default LUIS app that it created.</span></span> <span data-ttu-id="50f9c-140">Esta aplicación de LUIS predeterminada ha detectado una intención Saludo.</span><span class="sxs-lookup"><span data-stu-id="50f9c-140">This default LUIS app detected a Greeting intent.</span></span>

## <a name="modify-the-luis-app"></a><span data-ttu-id="50f9c-141">Modificación de la aplicación de LUIS</span><span class="sxs-lookup"><span data-stu-id="50f9c-141">Modify the LUIS app</span></span>

<span data-ttu-id="50f9c-142">Inicie sesión en [https://www.luis.ai](https://www.luis.ai) con la misma cuenta que usa para iniciar sesión en Azure.</span><span class="sxs-lookup"><span data-stu-id="50f9c-142">Log in to [https://www.luis.ai](https://www.luis.ai) using the same account you use to log in to Azure.</span></span> <span data-ttu-id="50f9c-143">Haga clic en **Mis aplicaciones**.</span><span class="sxs-lookup"><span data-stu-id="50f9c-143">Click on **My apps**.</span></span> <span data-ttu-id="50f9c-144">En la lista de aplicaciones, busque la aplicación que comienza con el nombre especificado en **Nombre de la aplicación** en la hoja **Servicio de bots** cuando creó el servicio de bots.</span><span class="sxs-lookup"><span data-stu-id="50f9c-144">In the list of apps, find the app that begins with the name specified in **App name** in the **Bot Service** blade when you created the Bot Service.</span></span> 

<span data-ttu-id="50f9c-145">Se inicia la aplicación de LUIS con cuatro intenciones: Cancelar, Saludo, Ayuda y Ninguno.</span><span class="sxs-lookup"><span data-stu-id="50f9c-145">The LUIS app starts with 4 intents: Cancel: Greeting, Help, and None.</span></span> <!-- picture -->

<span data-ttu-id="50f9c-146">Los siguientes pasos agregan las intenciones Note.Create, Note.ReadAloud y Note.Delete:</span><span class="sxs-lookup"><span data-stu-id="50f9c-146">The following steps add the Note.Create, Note.ReadAloud, and Note.Delete intents:</span></span> 

1. <span data-ttu-id="50f9c-147">Haga clic en **Dominios creados previamente** en la parte inferior izquierda de la página.</span><span class="sxs-lookup"><span data-stu-id="50f9c-147">Click on **Prebuit Domains** in the lower left of the page.</span></span> <span data-ttu-id="50f9c-148">Busque el dominio **Nota** y haga clic en **Agregar dominio**.</span><span class="sxs-lookup"><span data-stu-id="50f9c-148">Find the **Note** domain and click **Add domain**.</span></span>

2. <span data-ttu-id="50f9c-149">En este tutorial no se usan todas las intenciones incluidas en el dominio **Nota** creado previamente.</span><span class="sxs-lookup"><span data-stu-id="50f9c-149">This tutorial doesn't use all of the intents included in the **Note** prebuilt domain.</span></span> <span data-ttu-id="50f9c-150">En la página **Intenciones**, haga clic en cada uno de los siguientes nombres de intenciones y luego haga clic en el botón **Eliminar intención**.</span><span class="sxs-lookup"><span data-stu-id="50f9c-150">In the **Intents** page, click on each of the following intent names and then click the **Delete Intent** button.</span></span>
   * <span data-ttu-id="50f9c-151">Note.ShowNext</span><span class="sxs-lookup"><span data-stu-id="50f9c-151">Note.ShowNext</span></span>
   * <span data-ttu-id="50f9c-152">Note.DeleteNoteItem</span><span class="sxs-lookup"><span data-stu-id="50f9c-152">Note.DeleteNoteItem</span></span>
   * <span data-ttu-id="50f9c-153">Note.Confirm</span><span class="sxs-lookup"><span data-stu-id="50f9c-153">Note.Confirm</span></span>
   * <span data-ttu-id="50f9c-154">Note.Clear</span><span class="sxs-lookup"><span data-stu-id="50f9c-154">Note.Clear</span></span>
   * <span data-ttu-id="50f9c-155">Note.CheckOffItem</span><span class="sxs-lookup"><span data-stu-id="50f9c-155">Note.CheckOffItem</span></span>
   * <span data-ttu-id="50f9c-156">Note.AddToNote</span><span class="sxs-lookup"><span data-stu-id="50f9c-156">Note.AddToNote</span></span>

   <span data-ttu-id="50f9c-157">Las únicas intenciones que deben permanecer en la aplicación de LUIS son las siguientes:</span><span class="sxs-lookup"><span data-stu-id="50f9c-157">The only intents that should remain in the LUIS app are the following:</span></span> 
   * <span data-ttu-id="50f9c-158">Note.ReadAloud</span><span class="sxs-lookup"><span data-stu-id="50f9c-158">Note.ReadAloud</span></span>
   * <span data-ttu-id="50f9c-159">Note.Create</span><span class="sxs-lookup"><span data-stu-id="50f9c-159">Note.Create</span></span>
   * <span data-ttu-id="50f9c-160">Note.Delete</span><span class="sxs-lookup"><span data-stu-id="50f9c-160">Note.Delete</span></span>
   * <span data-ttu-id="50f9c-161">None</span><span class="sxs-lookup"><span data-stu-id="50f9c-161">None</span></span>
   * <span data-ttu-id="50f9c-162">Ayuda</span><span class="sxs-lookup"><span data-stu-id="50f9c-162">Help</span></span>
   * <span data-ttu-id="50f9c-163">Greeting</span><span class="sxs-lookup"><span data-stu-id="50f9c-163">Greeting</span></span>
   * <span data-ttu-id="50f9c-164">Cancelar</span><span class="sxs-lookup"><span data-stu-id="50f9c-164">Cancel</span></span>

     ![intenciones mostradas en la aplicación de LUIS](../media/bot-builder-nodejs-use-luis/luis-intent-list.png)


3.  <span data-ttu-id="50f9c-166">Haga clic en el botón **Entrenar** en la esquina superior derecha para entrenar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="50f9c-166">Click the **Train** button in the upper right to train your app.</span></span>
4.  <span data-ttu-id="50f9c-167">Haga clic en **PUBLICAR** en la barra de navegación superior para abrir la página **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="50f9c-167">Click **PUBLISH** in the top navigation bar to open the **Publish** page.</span></span> <span data-ttu-id="50f9c-168">Haga clic en el botón **Publish to production slot** (Publicar en el espacio de producción).</span><span class="sxs-lookup"><span data-stu-id="50f9c-168">Click the **Publish to production slot** button.</span></span> <span data-ttu-id="50f9c-169">Tras una publicación correcta, una aplicación de LUIS se implementa en la dirección URL mostrada en la columna **Punto de conexión** de la página **Publicar aplicación**, en la fila que empieza con el nombre de recurso Starter_Key.</span><span class="sxs-lookup"><span data-stu-id="50f9c-169">After successful publish, a LUIS app is deployed to the URL displayed in the **Endpoint** column in the **Publish App** page, in the row that starts with the Resource Name Starter_Key.</span></span> <span data-ttu-id="50f9c-170">La dirección URL tiene un formato similar a este ejemplo: `https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx?subscription-key=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx&timezoneOffset=0&verbose=true&q=`.</span><span class="sxs-lookup"><span data-stu-id="50f9c-170">The URL has a format similar to this example: `https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx?subscription-key=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx&timezoneOffset=0&verbose=true&q=`.</span></span> <span data-ttu-id="50f9c-171">El identificador de la aplicación y la clave de suscripción de esta dirección URL son las mismas que LuisAppId y LuisAPIKey en ** Configuración de App Service > ApplicationSettings > Configuración de la aplicación **</span><span class="sxs-lookup"><span data-stu-id="50f9c-171">The app ID and subscription key in this URL are the same as LuisAppId and LuisAPIKey in ** App Service Settings > ApplicationSettings > App settings **</span></span>


## <a name="modify-the-bot-code"></a><span data-ttu-id="50f9c-172">Modificar el código del bot</span><span class="sxs-lookup"><span data-stu-id="50f9c-172">Modify the bot code</span></span>

<span data-ttu-id="50f9c-173">Haga clic en **Compilar** y después en **Open online code editor** (Abrir el editor de código en línea).</span><span class="sxs-lookup"><span data-stu-id="50f9c-173">Click **Build** and then click **Open online code editor**.</span></span>

   ![Abrir el editor de código en línea](../media/bot-builder-nodejs-use-luis/bot-service-build.png)

<span data-ttu-id="50f9c-175">En el editor de código, abra `app.js`.</span><span class="sxs-lookup"><span data-stu-id="50f9c-175">In the code editor, open `app.js`.</span></span> <span data-ttu-id="50f9c-176">Contiene el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="50f9c-176">It contains the following code:</span></span>

```javascript
var restify = require('restify');
var builder = require('botbuilder');
var botbuilder_azure = require("botbuilder-azure");

// Setup Restify Server
var server = restify.createServer();
server.listen(process.env.port || process.env.PORT || 3978, function () {
   console.log('%s listening to %s', server.name, server.url); 
});
  
// Create chat connector for communicating with the Bot Framework Service
var connector = new builder.ChatConnector({
    appId: process.env.MicrosoftAppId,
    appPassword: process.env.MicrosoftAppPassword,
    openIdMetadata: process.env.BotOpenIdMetadata 
});

// Listen for messages from users 
server.post('/api/messages', connector.listen());

/*----------------------------------------------------------------------------------------
* Bot Storage: This is a great spot to register the private state storage for your bot. 
* We provide adapters for Azure Table, CosmosDb, SQL Azure, or you can implement your own!
* For samples and documentation, see: https://github.com/Microsoft/BotBuilder-Azure
* ---------------------------------------------------------------------------------------- */

var tableName = 'botdata';
var azureTableClient = new botbuilder_azure.AzureTableClient(tableName, process.env['AzureWebJobsStorage']);
var tableStorage = new botbuilder_azure.AzureBotStorage({ gzipData: false }, azureTableClient);

// Create your bot with a function to receive messages from the user
// This default message handler is invoked if the user's utterance doesn't
// match any intents handled by other dialogs.
var bot = new builder.UniversalBot(connector, function (session, args) {
    session.send('You reached the default message handler. You said \'%s\'.', session.message.text);
});

bot.set('storage', tableStorage);

// Make sure you add code to validate these fields
var luisAppId = process.env.LuisAppId;
var luisAPIKey = process.env.LuisAPIKey;
var luisAPIHostName = process.env.LuisAPIHostName || 'westus.api.cognitive.microsoft.com';

const LuisModelUrl = 'https://' + luisAPIHostName + '/luis/v2.0/apps/' + luisAppId + '?subscription-key=' + luisAPIKey;

// Create a recognizer that gets intents from LUIS, and add it to the bot
var recognizer = new builder.LuisRecognizer(LuisModelUrl);
bot.recognizer(recognizer);

// Add a dialog for each intent that the LUIS app recognizes.
// See https://docs.microsoft.com/en-us/bot-framework/nodejs/bot-builder-nodejs-recognize-intent-luis 
bot.dialog('GreetingDialog',
    (session) => {
        session.send('You reached the Greeting intent. You said \'%s\'.', session.message.text);
        session.endDialog();
    }
).triggerAction({
    matches: 'Greeting'
})

bot.dialog('HelpDialog',
    (session) => {
        session.send('You reached the Help intent. You said \'%s\'.', session.message.text);
        session.endDialog();
    }
).triggerAction({
    matches: 'Help'
})

bot.dialog('CancelDialog',
    (session) => {
        session.send('You reached the Cancel intent. You said \'%s\'.', session.message.text);
        session.endDialog();
    }
).triggerAction({
    matches: 'Cancel'
})

```


> [!TIP] 
> <span data-ttu-id="50f9c-177">También puede encontrar el código de ejemplo descrito en este artículo en el [ejemplo del bot de notas][NotesSample].</span><span class="sxs-lookup"><span data-stu-id="50f9c-177">You can also find the sample code described in this article in the [Notes bot sample][NotesSample].</span></span>



## <a name="edit-the-default-message-handler"></a><span data-ttu-id="50f9c-178">Edición del controlador de mensajes predeterminado</span><span class="sxs-lookup"><span data-stu-id="50f9c-178">Edit the default message handler</span></span>
<span data-ttu-id="50f9c-179">El bot tiene un controlador de mensajes predeterminado.</span><span class="sxs-lookup"><span data-stu-id="50f9c-179">The bot has a default message handler.</span></span> <span data-ttu-id="50f9c-180">Edítelo para que coincida con lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="50f9c-180">Edit it to match the following:</span></span> 
```javascript
// Create your bot with a function to receive messages from the user.
// This default message handler is invoked if the user's utterance doesn't
// match any intents handled by other dialogs.
var bot = new builder.UniversalBot(connector, function (session, args) {
    session.send("Hi... I'm the note bot sample. I can create new notes, read saved notes to you and delete notes.");

   // If the object for storing notes in session.userData doesn't exist yet, initialize it
   if (!session.userData.notes) {
       session.userData.notes = {};
       console.log("initializing userData.notes in default message handler");
   }
});
```

## <a name="handle-the-notecreate-intent"></a><span data-ttu-id="50f9c-181">Control de la intención Note.Create</span><span class="sxs-lookup"><span data-stu-id="50f9c-181">Handle the Note.Create intent</span></span>

<span data-ttu-id="50f9c-182">Copie el código siguiente y péguelo en el extremo de app.js:</span><span class="sxs-lookup"><span data-stu-id="50f9c-182">Copy the following code and paste it at the end of app.js:</span></span>

```javascript
// CreateNote dialog
bot.dialog('CreateNote', [
    function (session, args, next) {
        // Resolve and store any Note.Title entity passed from LUIS.
        var intent = args.intent;
        var title = builder.EntityRecognizer.findEntity(intent.entities, 'Note.Title');

        var note = session.dialogData.note = {
          title: title ? title.entity : null,
        };
        
        // Prompt for title
        if (!note.title) {
            builder.Prompts.text(session, 'What would you like to call your note?');
        } else {
            next();
        }
    },
    function (session, results, next) {
        var note = session.dialogData.note;
        if (results.response) {
            note.title = results.response;
        }

        // Prompt for the text of the note
        if (!note.text) {
            builder.Prompts.text(session, 'What would you like to say in your note?');
        } else {
            next();
        }
    },
    function (session, results) {
        var note = session.dialogData.note;
        if (results.response) {
            note.text = results.response;
        }
        
        // If the object for storing notes in session.userData doesn't exist yet, initialize it
        if (!session.userData.notes) {
            session.userData.notes = {};
            console.log("initializing session.userData.notes in CreateNote dialog");
        }
        // Save notes in the notes object
        session.userData.notes[note.title] = note;

        // Send confirmation to user
        session.endDialog('Creating note named "%s" with text "%s"',
            note.title, note.text);
    }
]).triggerAction({ 
    matches: 'Note.Create',
    confirmPrompt: "This will cancel the creation of the note you started. Are you sure?" 
}).cancelAction('cancelCreateNote', "Note canceled.", {
    matches: /^(cancel|nevermind)/i,
    confirmPrompt: "Are you sure?"
});
```

<span data-ttu-id="50f9c-183">Todas las entidades de la grabación de voz se pasan al diálogo mediante el parámetro `args`.</span><span class="sxs-lookup"><span data-stu-id="50f9c-183">Any entities in the utterance are passed to the dialog using the `args` parameter.</span></span> <span data-ttu-id="50f9c-184">El primer paso de la [cascada][waterfall] llama a [EntityRecognizer.findEntity][EntityRecognizer_findEntity] para obtener el título de la nota de cualquier entidad `Note.Title` de la respuesta de LUIS.</span><span class="sxs-lookup"><span data-stu-id="50f9c-184">The first step of the [waterfall][waterfall] calls [EntityRecognizer.findEntity][EntityRecognizer_findEntity] to get the title of the note from any `Note.Title` entities in the LUIS response.</span></span> <span data-ttu-id="50f9c-185">Si la aplicación de LUIS no detectó una entidad `Note.Title`, el bot pide al usuario el nombre de la nota.</span><span class="sxs-lookup"><span data-stu-id="50f9c-185">If the LUIS app didn't detect a `Note.Title` entity, the bot prompts the user for the name of the note.</span></span> <span data-ttu-id="50f9c-186">El segundo paso de la cascada pide el texto que se va a incluir en la nota.</span><span class="sxs-lookup"><span data-stu-id="50f9c-186">The second step of the waterfall prompts for the text to include in the note.</span></span> <span data-ttu-id="50f9c-187">Una vez que el bot tiene el texto de la nota, el tercer paso usa [session.userData][session_userData] para guardar la nota en un objeto `notes`, para lo que usa el título como clave.</span><span class="sxs-lookup"><span data-stu-id="50f9c-187">Once the bot has the text of the note, the third step uses [session.userData][session_userData] to save the note in a `notes` object, using the title as the key.</span></span> <span data-ttu-id="50f9c-188">Para más información sobre `session.UserData`, vea [Administración de datos de estado](./bot-builder-nodejs-state.md).</span><span class="sxs-lookup"><span data-stu-id="50f9c-188">For more information on `session.UserData` see [Manage state data](./bot-builder-nodejs-state.md).</span></span> 



## <a name="handle-the-notedelete-intent"></a><span data-ttu-id="50f9c-189">Control de la intención Note.Delete</span><span class="sxs-lookup"><span data-stu-id="50f9c-189">Handle the Note.Delete intent</span></span>
<span data-ttu-id="50f9c-190">Igual que para la intención `Note.Create`, el bot examina el parámetro `args` para un título.</span><span class="sxs-lookup"><span data-stu-id="50f9c-190">Just as for the `Note.Create` intent, the bot examines the `args` parameter for a title.</span></span> <span data-ttu-id="50f9c-191">Si no se detecta ningún título, el bot pregunta al usuario.</span><span class="sxs-lookup"><span data-stu-id="50f9c-191">If no title is detected, the bot prompts the user.</span></span> <span data-ttu-id="50f9c-192">El título se utiliza para buscar la nota para eliminarla de `session.userData.notes`.</span><span class="sxs-lookup"><span data-stu-id="50f9c-192">The title is used to look up the note to delete from `session.userData.notes`.</span></span> 



<span data-ttu-id="50f9c-193">Copie el código siguiente y péguelo en el extremo de app.js:</span><span class="sxs-lookup"><span data-stu-id="50f9c-193">Copy the following code and paste it at the end of app.js:</span></span>
```javascript
// Delete note dialog
bot.dialog('DeleteNote', [
    function (session, args, next) {
        if (noteCount(session.userData.notes) > 0) {
            // Resolve and store any Note.Title entity passed from LUIS.
            var title;
            var intent = args.intent;
            var entity = builder.EntityRecognizer.findEntity(intent.entities, 'Note.Title');
            if (entity) {
                // Verify that the title is in our set of notes.
                title = builder.EntityRecognizer.findBestMatch(session.userData.notes, entity.entity);
            }
            
            // Prompt for note name
            if (!title) {
                builder.Prompts.choice(session, 'Which note would you like to delete?', session.userData.notes);
            } else {
                next({ response: title });
            }
        } else {
            session.endDialog("No notes to delete.");
        }
    },
    function (session, results) {
        delete session.userData.notes[results.response.entity];        
        session.endDialog("Deleted the '%s' note.", results.response.entity);
    }
]).triggerAction({
    matches: 'Note.Delete'
}).cancelAction('cancelDeleteNote', "Ok - canceled note deletion.", {
    matches: /^(cancel|nevermind)/i
});
```

<span data-ttu-id="50f9c-194">El código que controla `Note.Delete` usa la función `noteCount` para determinar si el objeto `notes` contiene las notas.</span><span class="sxs-lookup"><span data-stu-id="50f9c-194">The code that handles `Note.Delete` uses the `noteCount` function to determine whether the `notes` object contains notes.</span></span> 

<span data-ttu-id="50f9c-195">Pegue la función de la aplicación auxiliar `noteCount` al final de `app.js`.</span><span class="sxs-lookup"><span data-stu-id="50f9c-195">Paste the `noteCount` helper function at the end of `app.js`.</span></span>

[!code-js[Add a helper function that returns the number of notes (JavaScript)](../includes/code/node-basicNote.js#CountNotesHelper)]

## <a name="handle-the-notereadaloud-intent"></a><span data-ttu-id="50f9c-196">Control de la intención Note.ReadAloud</span><span class="sxs-lookup"><span data-stu-id="50f9c-196">Handle the Note.ReadAloud intent</span></span>

<span data-ttu-id="50f9c-197">Copie el código siguiente y péguelo en `app.js` después del controlador para `Note.Delete`:</span><span class="sxs-lookup"><span data-stu-id="50f9c-197">Copy the following code and paste it in `app.js` after the handler for `Note.Delete`:</span></span>

```javascript
// Read note dialog
bot.dialog('ReadNote', [
    function (session, args, next) {
        if (noteCount(session.userData.notes) > 0) {
           
            // Resolve and store any Note.Title entity passed from LUIS.
            var title;
            var intent = args.intent;
            var entity = builder.EntityRecognizer.findEntity(intent.entities, 'Note.Title');
            if (entity) {
                // Verify it's in our set of notes.
                title = builder.EntityRecognizer.findBestMatch(session.userData.notes, entity.entity);
            }
            
            // Prompt for note name
            if (!title) {
                builder.Prompts.choice(session, 'Which note would you like to read?', session.userData.notes);
            } else {
                next({ response: title });
            }
        } else {
            session.endDialog("No notes to read.");
        }
    },
    function (session, results) {        
        session.endDialog("Here's the '%s' note: '%s'.", results.response.entity, session.userData.notes[results.response.entity].text);
    }
]).triggerAction({
    matches: 'Note.ReadAloud'
}).cancelAction('cancelReadNote', "Ok.", {
    matches: /^(cancel|nevermind)/i
});

```

<span data-ttu-id="50f9c-198">El objeto `session.userData.notes` se pasa como el tercer argumento a `builder.Prompts.choice`, para que el mensaje muestre una lista de notas al usuario.</span><span class="sxs-lookup"><span data-stu-id="50f9c-198">The `session.userData.notes` object is passed as the third argument to `builder.Prompts.choice`, so that the prompt displays a list of notes to the user.</span></span>

<span data-ttu-id="50f9c-199">Ahora que ha agregado controladores para las nuevas intenciones, el código completo para `app.js` contiene lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="50f9c-199">Now that you've added handlers for the new intents, the full code for `app.js` contains the following:</span></span>

```javascript
var restify = require('restify');
var builder = require('botbuilder');
var botbuilder_azure = require("botbuilder-azure");

// Setup Restify Server
var server = restify.createServer();
server.listen(process.env.port || process.env.PORT || 3978, function () {
   console.log('%s listening to %s', server.name, server.url); 
});
  
// Create chat connector for communicating with the Bot Framework Service
var connector = new builder.ChatConnector({
    appId: process.env.MicrosoftAppId,
    appPassword: process.env.MicrosoftAppPassword,
    openIdMetadata: process.env.BotOpenIdMetadata 
});

// Listen for messages from users 
server.post('/api/messages', connector.listen());

/*----------------------------------------------------------------------------------------
* Bot Storage: This is a great spot to register the private state storage for your bot. 
* We provide adapters for Azure Table, CosmosDb, SQL Azure, or you can implement your own!
* For samples and documentation, see: https://github.com/Microsoft/BotBuilder-Azure
* ---------------------------------------------------------------------------------------- */

var tableName = 'botdata';
var azureTableClient = new botbuilder_azure.AzureTableClient(tableName, process.env['AzureWebJobsStorage']);
var tableStorage = new botbuilder_azure.AzureBotStorage({ gzipData: false }, azureTableClient);

// Create your bot with a function to receive messages from the user.
// This default message handler is invoked if the user's utterance doesn't
// match any intents handled by other dialogs.
var bot = new builder.UniversalBot(connector, function (session, args) {
    session.send("Hi... I'm the note bot sample. I can create new notes, read saved notes to you and delete notes.");

   // If the object for storing notes in session.userData doesn't exist yet, initialize it
   if (!session.userData.notes) {
       session.userData.notes = {};
       console.log("initializing userData.notes in default message handler");
   }
});

bot.set('storage', tableStorage);

// Make sure you add code to validate these fields
var luisAppId = process.env.LuisAppId;
var luisAPIKey = process.env.LuisAPIKey;
var luisAPIHostName = process.env.LuisAPIHostName || 'westus.api.cognitive.microsoft.com';

const LuisModelUrl = 'https://' + luisAPIHostName + '/luis/v2.0/apps/' + luisAppId + '?subscription-key=' + luisAPIKey;

// Create a recognizer that gets intents from LUIS, and add it to the bot
var recognizer = new builder.LuisRecognizer(LuisModelUrl);
bot.recognizer(recognizer);

// CreateNote dialog
bot.dialog('CreateNote', [
    function (session, args, next) {
        // Resolve and store any Note.Title entity passed from LUIS.
        var intent = args.intent;
        var title = builder.EntityRecognizer.findEntity(intent.entities, 'Note.Title');

        var note = session.dialogData.note = {
          title: title ? title.entity : null,
        };
        
        // Prompt for title
        if (!note.title) {
            builder.Prompts.text(session, 'What would you like to call your note?');
        } else {
            next();
        }
    },
    function (session, results, next) {
        var note = session.dialogData.note;
        if (results.response) {
            note.title = results.response;
        }

        // Prompt for the text of the note
        if (!note.text) {
            builder.Prompts.text(session, 'What would you like to say in your note?');
        } else {
            next();
        }
    },
    function (session, results) {
        var note = session.dialogData.note;
        if (results.response) {
            note.text = results.response;
        }
        
        // If the object for storing notes in session.userData doesn't exist yet, initialize it
        if (!session.userData.notes) {
            session.userData.notes = {};
            console.log("initializing session.userData.notes in CreateNote dialog");
        }
        // Save notes in the notes object
        session.userData.notes[note.title] = note;

        // Send confirmation to user
        session.endDialog('Creating note named "%s" with text "%s"',
            note.title, note.text);
    }
]).triggerAction({ 
    matches: 'Note.Create',
    confirmPrompt: "This will cancel the creation of the note you started. Are you sure?" 
}).cancelAction('cancelCreateNote', "Note canceled.", {
    matches: /^(cancel|nevermind)/i,
    confirmPrompt: "Are you sure?"
});

// Delete note dialog
bot.dialog('DeleteNote', [
    function (session, args, next) {
        if (noteCount(session.userData.notes) > 0) {
            // Resolve and store any Note.Title entity passed from LUIS.
            var title;
            var intent = args.intent;
            var entity = builder.EntityRecognizer.findEntity(intent.entities, 'Note.Title');
            if (entity) {
                // Verify that the title is in our set of notes.
                title = builder.EntityRecognizer.findBestMatch(session.userData.notes, entity.entity);
            }
            
            // Prompt for note name
            if (!title) {
                builder.Prompts.choice(session, 'Which note would you like to delete?', session.userData.notes);
            } else {
                next({ response: title });
            }
        } else {
            session.endDialog("No notes to delete.");
        }
    },
    function (session, results) {
        delete session.userData.notes[results.response.entity];        
        session.endDialog("Deleted the '%s' note.", results.response.entity);
    }
]).triggerAction({
    matches: 'Note.Delete'
}).cancelAction('cancelDeleteNote', "Ok - canceled note deletion.", {
    matches: /^(cancel|nevermind)/i
});


// Read note dialog
bot.dialog('ReadNote', [
    function (session, args, next) {
        if (noteCount(session.userData.notes) > 0) {
           
            // Resolve and store any Note.Title entity passed from LUIS.
            var title;
            var intent = args.intent;
            var entity = builder.EntityRecognizer.findEntity(intent.entities, 'Note.Title');
            if (entity) {
                // Verify it's in our set of notes.
                title = builder.EntityRecognizer.findBestMatch(session.userData.notes, entity.entity);
            }
            
            // Prompt for note name
            if (!title) {
                builder.Prompts.choice(session, 'Which note would you like to read?', session.userData.notes);
            } else {
                next({ response: title });
            }
        } else {
            session.endDialog("No notes to read.");
        }
    },
    function (session, results) {        
        session.endDialog("Here's the '%s' note: '%s'.", results.response.entity, session.userData.notes[results.response.entity].text);
    }
]).triggerAction({
    matches: 'Note.ReadAloud'
}).cancelAction('cancelReadNote', "Ok.", {
    matches: /^(cancel|nevermind)/i
});


// Helper function to count the number of notes stored in session.userData.notes
function noteCount(notes) {

    var i = 0;
    for (var name in notes) {
        i++;
    }
    return i;
}
```

## <a name="test-the-bot"></a><span data-ttu-id="50f9c-200">Probar el bot</span><span class="sxs-lookup"><span data-stu-id="50f9c-200">Test the bot</span></span>

<span data-ttu-id="50f9c-201">En Azure Portal, haga clic en **Test in Web Chat** (Probar en Chat en web) para probar el bot.</span><span class="sxs-lookup"><span data-stu-id="50f9c-201">In the Azure Portal, click on **Test in Web Chat** to test the bot.</span></span> <span data-ttu-id="50f9c-202">Intente escribir mensajes como "Crear una nota", "Leer mis notas" y "Eliminar notas" para invocar las intenciones agregadas.</span><span class="sxs-lookup"><span data-stu-id="50f9c-202">Try type messages like "Create a note", "read my notes", and "delete notes" to invoke the intents that you added to it.</span></span>
   <span data-ttu-id="50f9c-203">![Probar el bot de notas en Chat en web](../media/bot-builder-nodejs-use-luis/bot-service-test-notebot.png)</span><span class="sxs-lookup"><span data-stu-id="50f9c-203">![Test notes bot in Web Chat](../media/bot-builder-nodejs-use-luis/bot-service-test-notebot.png)</span></span>

> [!TIP]
> <span data-ttu-id="50f9c-204">Si ve que el bot no reconoce siempre la intención o las entidades correctas, proporcione a la aplicación de LUIS más ejemplos de expresiones para entrenarla y mejorar su rendimiento.</span><span class="sxs-lookup"><span data-stu-id="50f9c-204">If you find that your bot doesn't always recognize the correct intent or entities, improve your LUIS app's performance by giving it more example utterances to train it.</span></span> <span data-ttu-id="50f9c-205">Puede volver a entrenar la aplicación de LUIS sin modificar el código del bot.</span><span class="sxs-lookup"><span data-stu-id="50f9c-205">You can retrain your LUIS app without any modification to your bot's code.</span></span> <span data-ttu-id="50f9c-206">Vea [Add example utterances](/azure/cognitive-services/LUIS/add-example-utterances) (Agregar expresiones de ejemplo) y [Train and test your LUIS app](/azure/cognitive-services/LUIS/train-test) (Entrenar y probar la aplicación de LUIS).</span><span class="sxs-lookup"><span data-stu-id="50f9c-206">See [Add example utterances](/azure/cognitive-services/LUIS/add-example-utterances) and [train and test your LUIS app](/azure/cognitive-services/LUIS/train-test).</span></span>


## <a name="next-steps"></a><span data-ttu-id="50f9c-207">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="50f9c-207">Next steps</span></span>

<span data-ttu-id="50f9c-208">Al probar el bot, puede ver que el reconocedor puede desencadenar la interrupción del diálogo activo actualmente.</span><span class="sxs-lookup"><span data-stu-id="50f9c-208">From trying the bot, you can see that the recognizer can trigger interruption of the currently active dialog.</span></span> <span data-ttu-id="50f9c-209">Permitir y controlar interrupciones es un diseño flexible que representa lo que el usuario hace realmente.</span><span class="sxs-lookup"><span data-stu-id="50f9c-209">Allowing and handling interruptions is a flexible design that accounts for what users really do.</span></span> <span data-ttu-id="50f9c-210">Más información sobre las diversas acciones que puede asociar con una intención reconocida.</span><span class="sxs-lookup"><span data-stu-id="50f9c-210">Learn more about the various actions you can associate with a recognized intent.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="50f9c-211">Control de acciones del usuario</span><span class="sxs-lookup"><span data-stu-id="50f9c-211">Handle user actions</span></span>](bot-builder-nodejs-dialog-actions.md)


[LUIS]: https://www.luis.ai/

[intentDialog]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.intentdialog.html

[intentDialog_matches]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.intentdialog.html#matches 

[NotesSample]: https://github.com/Microsoft/BotFramework-Samples/tree/master/docs-samples/Node/basics-naturalLanguage

[triggerAction]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.dialog.html#triggeraction

[confirmPrompt]: https://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.itriggeractionoptions.html#confirmprompt

[waterfall]: bot-builder-nodejs-dialog-manage-conversation-flow.md#manage-conversation-flow-with-a-waterfall

[session_userData]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.session.html#userdata

[EntityRecognizer_findEntity]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.entityrecognizer.html#findentity

[matches]: https://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.itriggeractionoptions.html#matches

[LUISAzureDocs]: /azure/cognitive-services/LUIS/Home

[Dialog]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.dialog.html

[IntentRecognizerSetOptions]: https://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.iintentrecognizersetoptions.html

[LuisRecognizer]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.luisrecognizer

[LUISConcepts]: https://docs.botframework.com/en-us/node/builder/guides/understanding-natural-language/

[DisambiguationSample]: https://github.com/Microsoft/BotBuilder/tree/master/Node/examples/feature-onDisambiguateRoute

[IDisambiguateRouteHandler]: https://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.idisambiguateroutehandler.html

[RegExpRecognizer]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.regexprecognizer.html

[AlarmBot]: https://github.com/Microsoft/BotBuilder/blob/master/Node/examples/basics-naturalLanguage/app.js

[LUISBotSample]: https://github.com/Microsoft/BotBuilder-Samples/tree/master/Node/intelligence-LUIS

[UniversalBot]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.universalbot.html
