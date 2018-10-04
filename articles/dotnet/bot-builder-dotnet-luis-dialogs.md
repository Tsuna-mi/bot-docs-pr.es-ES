---
title: Reconocimiento de intenciones y entidades con LUIS | Microsoft Docs
description: Aprenda a habilitar el bot para entender el lenguaje natural con el uso de diálogos LUIS en Bot Builder SDK para .NET.
author: DeniseMak
ms.author: v-demak
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: f95335149fa2c896d905834832089ffbfa960bf2
ms.sourcegitcommit: d4afc924b0e1907c4d6f7a6fc5ac1fe521aeef7e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2018
ms.locfileid: "47447397"
---
# <a name="recognize-intents-and-entities-with-luis"></a><span data-ttu-id="3bce6-103">Reconocimiento de intenciones y entidades con LUIS</span><span class="sxs-lookup"><span data-stu-id="3bce6-103">Recognize intents and entities with LUIS</span></span> 

[!INCLUDE [pre-release-label](../includes/pre-release-label-v3.md)]

<span data-ttu-id="3bce6-104">Este artículo usa el ejemplo de un bot para tomar notas, a fin de demostrar cómo Language Understanding ([LUIS][LUIS]) ayuda a que el bot responda correctamente a la entrada de lenguaje natural.</span><span class="sxs-lookup"><span data-stu-id="3bce6-104">This article uses the example of a bot for taking notes, to demonstrate how Language Understanding ([LUIS][LUIS]) helps your bot respond appropriately to natural language input.</span></span> <span data-ttu-id="3bce6-105">Un bot detecta lo que un usuario desea hacer mediante la identificación de su **intención**.</span><span class="sxs-lookup"><span data-stu-id="3bce6-105">A bot detects what a user wants to do by identifying their **intent**.</span></span> <span data-ttu-id="3bce6-106">Esta intención se determina a partir de la entrada de texto, voz o **grabaciones de voz**.</span><span class="sxs-lookup"><span data-stu-id="3bce6-106">This intent is determined from spoken or textual input, or **utterances**.</span></span> <span data-ttu-id="3bce6-107">La intención asigna grabaciones de voz a acciones que realiza el bot.</span><span class="sxs-lookup"><span data-stu-id="3bce6-107">The intent maps utterances to actions that the bot takes.</span></span> <span data-ttu-id="3bce6-108">Por ejemplo, un bot de toma de notas reconoce una intención `Notes.Create` para invocar la funcionalidad para crear una nota.</span><span class="sxs-lookup"><span data-stu-id="3bce6-108">For example, a note-taking bot recognizes a `Notes.Create` intent to invoke the functionality for creating a note.</span></span> <span data-ttu-id="3bce6-109">Puede que un bot también necesite extraer **entidades**, que son palabras importantes de las grabaciones de voz.</span><span class="sxs-lookup"><span data-stu-id="3bce6-109">A bot may also need to extract **entities**, which are important words in utterances.</span></span> <span data-ttu-id="3bce6-110">En el ejemplo de un bot de toma de notas, la entidad `Notes.Title` identifica el título de cada nota.</span><span class="sxs-lookup"><span data-stu-id="3bce6-110">In the example of a note-taking bot, the `Notes.Title` entity identifies the title of each note.</span></span>

## <a name="create-a-language-understanding-bot-with-bot-service"></a><span data-ttu-id="3bce6-111">Crear un bot de Language Understanding con Bot Service</span><span class="sxs-lookup"><span data-stu-id="3bce6-111">Create a Language Understanding bot with Bot Service</span></span>

1. <span data-ttu-id="3bce6-112">En [Azure Portal](https://portal.azure.com), haga clic en **Crear nuevo recurso** en la hoja de menú y, después, en **Ver todo**.</span><span class="sxs-lookup"><span data-stu-id="3bce6-112">In the [Azure portal](https://portal.azure.com), select **Create new resource** in the menu blade and click **See all**.</span></span>

    ![Crear recurso](../media/bot-builder-dotnet-use-luis/bot-service-creation.png)

2. <span data-ttu-id="3bce6-114">En el cuadro de búsqueda, busque **Bot de aplicación web**.</span><span class="sxs-lookup"><span data-stu-id="3bce6-114">In the search box, search for **Web App Bot**.</span></span> 

    ![Crear recurso](../media/bot-builder-dotnet-use-luis/bot-service-selection.png)

3. <span data-ttu-id="3bce6-116">En la hoja **Servicio de bots**, proporcione la información necesaria y haga clic en **Crear**.</span><span class="sxs-lookup"><span data-stu-id="3bce6-116">In the **Bot Service** blade, provide the required information, and click **Create**.</span></span> <span data-ttu-id="3bce6-117">Esto crea e implementa el servicio de bots y la aplicación de LUIS en Azure.</span><span class="sxs-lookup"><span data-stu-id="3bce6-117">This creates and deploys the bot service and LUIS app to Azure.</span></span> 
   * <span data-ttu-id="3bce6-118">Establezca **Nombre de la aplicación** en el nombre del bot.</span><span class="sxs-lookup"><span data-stu-id="3bce6-118">Set **App name** to your bot’s name.</span></span> <span data-ttu-id="3bce6-119">El nombre se usa como el subdominio cuando el bot se implementa en la nube (por ejemplo, mynotesbot.azurewebsites.net).</span><span class="sxs-lookup"><span data-stu-id="3bce6-119">The name is used as the subdomain when your bot is deployed to the cloud (for example, mynotesbot.azurewebsites.net).</span></span> <span data-ttu-id="3bce6-120">Este nombre también se utiliza como el nombre de la aplicación de LUIS asociada con el bot.</span><span class="sxs-lookup"><span data-stu-id="3bce6-120">This name is also used as the name of the LUIS app associated with your bot.</span></span> <span data-ttu-id="3bce6-121">Cópielo para usarlo más adelante, para encontrar la aplicación de LUIS asociada con el bot.</span><span class="sxs-lookup"><span data-stu-id="3bce6-121">Copy it to use later, to find the LUIS app associated with the bot.</span></span>
   * <span data-ttu-id="3bce6-122">Seleccione la suscripción, el [grupo de recursos](/azure/azure-resource-manager/resource-group-overview), el plan de App Service y la [ubicación](https://azure.microsoft.com/en-us/regions/).</span><span class="sxs-lookup"><span data-stu-id="3bce6-122">Select the subscription, [resource group](/azure/azure-resource-manager/resource-group-overview), App service plan, and [location](https://azure.microsoft.com/en-us/regions/).</span></span>
   * <span data-ttu-id="3bce6-123">Seleccione la plantilla **Language Understanding (C#)** en el campo **Bot template** (Plantilla de bot).</span><span class="sxs-lookup"><span data-stu-id="3bce6-123">Select the **Language understanding (C#)** template for the **Bot template** field.</span></span>

     ![Hoja Servicio de bots](../media/bot-builder-dotnet-use-luis/bot-service-setting-callout-template.png)

   * <span data-ttu-id="3bce6-125">Active la casilla para confirmar los términos del servicio.</span><span class="sxs-lookup"><span data-stu-id="3bce6-125">Check the box to confirm to the terms of service.</span></span>

4. <span data-ttu-id="3bce6-126">Confirme que se ha implementado el servicio de bots.</span><span class="sxs-lookup"><span data-stu-id="3bce6-126">Confirm that the bot service has been deployed.</span></span>
    * <span data-ttu-id="3bce6-127">Haga clic en Notificaciones (el icono de la campana que se encuentra en el borde superior de Azure Portal).</span><span class="sxs-lookup"><span data-stu-id="3bce6-127">Click Notifications (the bell icon that is located along the top edge of the Azure portal).</span></span> <span data-ttu-id="3bce6-128">La notificación cambiará de **Implementación iniciada** a **Implementación correcta**.</span><span class="sxs-lookup"><span data-stu-id="3bce6-128">The notification will change from **Deployment started** to **Deployment succeeded**.</span></span>
    * <span data-ttu-id="3bce6-129">Después de que la notificación cambie a **Implementación correcta**, haga clic en **Ir al recurso** en esa notificación.</span><span class="sxs-lookup"><span data-stu-id="3bce6-129">After the notification changes to **Deployment succeeded**, click **Go to resource** on that notification.</span></span>

## <a name="try-the-bot"></a><span data-ttu-id="3bce6-130">Probar el bot</span><span class="sxs-lookup"><span data-stu-id="3bce6-130">Try the bot</span></span>

<span data-ttu-id="3bce6-131">Para confirmar que el bot se ha implementado, active las **Notificaciones**.</span><span class="sxs-lookup"><span data-stu-id="3bce6-131">Confirm that the bot has been deployed by checking the **Notifications**.</span></span> <span data-ttu-id="3bce6-132">Las notificaciones cambiarán de **Implementación en curso...** a **Implementación correcta**.</span><span class="sxs-lookup"><span data-stu-id="3bce6-132">The notifications will change from **Deployment in progress...** to **Deployment succeeded**.</span></span> <span data-ttu-id="3bce6-133">Haga clic en el botón **Ir al recurso** para abrir la hoja de recursos del bot.</span><span class="sxs-lookup"><span data-stu-id="3bce6-133">Click **Go to resource** button to open the bot's resources blade.</span></span>

<span data-ttu-id="3bce6-134">Una vez registrado el bot, haga clic en **Test in Web Chat** (Probar en Chat en web) para abrir el panel Chat en web.</span><span class="sxs-lookup"><span data-stu-id="3bce6-134">Once the bot is registered, click **Test in Web Chat** to open the Web Chat pane.</span></span> <span data-ttu-id="3bce6-135">Escriba "hello" ("hola") en el Chat en web.</span><span class="sxs-lookup"><span data-stu-id="3bce6-135">Type "hello" in Web Chat.</span></span>

  ![Probar el bot en Chat en web](../media/bot-builder-dotnet-use-luis/bot-service-web-chat.png)

<span data-ttu-id="3bce6-137">El bot responde "You have reached Greeting.</span><span class="sxs-lookup"><span data-stu-id="3bce6-137">The bot responds by saying "You have reached Greeting.</span></span> <span data-ttu-id="3bce6-138">You said: hello" ("Se ha puesto en contacto con Saludo. Ha dicho: hola").</span><span class="sxs-lookup"><span data-stu-id="3bce6-138">You said: hello".</span></span> <span data-ttu-id="3bce6-139">Esto confirma que el bot ha recibido el mensaje y lo ha pasado a una aplicación de LUIS predeterminada que ha creado.</span><span class="sxs-lookup"><span data-stu-id="3bce6-139">This confirms that the bot has received your message and passed it to a default LUIS app that it created.</span></span> <span data-ttu-id="3bce6-140">Esta aplicación de LUIS predeterminada ha detectado una intención Saludo.</span><span class="sxs-lookup"><span data-stu-id="3bce6-140">This default LUIS app detected a Greeting intent.</span></span>

## <a name="modify-the-luis-app"></a><span data-ttu-id="3bce6-141">Modificación de la aplicación de LUIS</span><span class="sxs-lookup"><span data-stu-id="3bce6-141">Modify the LUIS app</span></span>

<span data-ttu-id="3bce6-142">Inicie sesión en [https://www.luis.ai](https://www.luis.ai) con la misma cuenta que usa para iniciar sesión en Azure.</span><span class="sxs-lookup"><span data-stu-id="3bce6-142">Log in to [https://www.luis.ai](https://www.luis.ai) using the same account you use to log in to Azure.</span></span> <span data-ttu-id="3bce6-143">Haga clic en **Mis aplicaciones**.</span><span class="sxs-lookup"><span data-stu-id="3bce6-143">Click on **My apps**.</span></span> <span data-ttu-id="3bce6-144">En la lista de aplicaciones, busque la aplicación que comienza con el nombre especificado en **Nombre de la aplicación** en la hoja **Servicio de bots** cuando creó el servicio de bots.</span><span class="sxs-lookup"><span data-stu-id="3bce6-144">In the list of apps, find the app that begins with the name specified in **App name** in the **Bot Service** blade when you created the Bot Service.</span></span> 

<span data-ttu-id="3bce6-145">Se inicia la aplicación de LUIS con cuatro intenciones: Cancelar, Saludo, Ayuda y Ninguno.</span><span class="sxs-lookup"><span data-stu-id="3bce6-145">The LUIS app starts with 4 intents: Cancel: Greeting, Help, and None.</span></span> <!-- picture -->

<span data-ttu-id="3bce6-146">Los siguientes pasos agregan las intenciones Note.Create, Note.ReadAloud y Note.Delete:</span><span class="sxs-lookup"><span data-stu-id="3bce6-146">The following steps add the Note.Create, Note.ReadAloud, and Note.Delete intents:</span></span> 

1. <span data-ttu-id="3bce6-147">Haga clic en **Dominios creados previamente** en la parte inferior izquierda de la página.</span><span class="sxs-lookup"><span data-stu-id="3bce6-147">Click on **Prebuit Domains** in the lower left of the page.</span></span> <span data-ttu-id="3bce6-148">Busque el dominio **Nota** y haga clic en **Agregar dominio**.</span><span class="sxs-lookup"><span data-stu-id="3bce6-148">Find the **Note** domain and click **Add domain**.</span></span>

2. <span data-ttu-id="3bce6-149">En este tutorial no se usan todas las intenciones incluidas en el dominio **Nota** creado previamente.</span><span class="sxs-lookup"><span data-stu-id="3bce6-149">This tutorial doesn't use all of the intents included in the **Note** prebuilt domain.</span></span> <span data-ttu-id="3bce6-150">En la página **Intenciones**, haga clic en cada uno de los siguientes nombres de intenciones y luego haga clic en el botón **Eliminar intención**.</span><span class="sxs-lookup"><span data-stu-id="3bce6-150">In the **Intents** page, click on each of the following intent names and then click the **Delete Intent** button.</span></span>
   * <span data-ttu-id="3bce6-151">Note.ShowNext</span><span class="sxs-lookup"><span data-stu-id="3bce6-151">Note.ShowNext</span></span>
   * <span data-ttu-id="3bce6-152">Note.DeleteNoteItem</span><span class="sxs-lookup"><span data-stu-id="3bce6-152">Note.DeleteNoteItem</span></span>
   * <span data-ttu-id="3bce6-153">Note.Confirm</span><span class="sxs-lookup"><span data-stu-id="3bce6-153">Note.Confirm</span></span>
   * <span data-ttu-id="3bce6-154">Note.Clear</span><span class="sxs-lookup"><span data-stu-id="3bce6-154">Note.Clear</span></span>
   * <span data-ttu-id="3bce6-155">Note.CheckOffItem</span><span class="sxs-lookup"><span data-stu-id="3bce6-155">Note.CheckOffItem</span></span>
   * <span data-ttu-id="3bce6-156">Note.AddToNote</span><span class="sxs-lookup"><span data-stu-id="3bce6-156">Note.AddToNote</span></span>

   <span data-ttu-id="3bce6-157">Las únicas intenciones que deben permanecer en la aplicación de LUIS son las siguientes:</span><span class="sxs-lookup"><span data-stu-id="3bce6-157">The only intents that should remain in the LUIS app are the following:</span></span> 
   * <span data-ttu-id="3bce6-158">Note.ReadAloud</span><span class="sxs-lookup"><span data-stu-id="3bce6-158">Note.ReadAloud</span></span>
   * <span data-ttu-id="3bce6-159">Note.Create</span><span class="sxs-lookup"><span data-stu-id="3bce6-159">Note.Create</span></span>
   * <span data-ttu-id="3bce6-160">Note.Delete</span><span class="sxs-lookup"><span data-stu-id="3bce6-160">Note.Delete</span></span>
   * <span data-ttu-id="3bce6-161">None</span><span class="sxs-lookup"><span data-stu-id="3bce6-161">None</span></span>
   * <span data-ttu-id="3bce6-162">Ayuda</span><span class="sxs-lookup"><span data-stu-id="3bce6-162">Help</span></span>
   * <span data-ttu-id="3bce6-163">Greeting</span><span class="sxs-lookup"><span data-stu-id="3bce6-163">Greeting</span></span>
   * <span data-ttu-id="3bce6-164">Cancelar</span><span class="sxs-lookup"><span data-stu-id="3bce6-164">Cancel</span></span> 

     ![intenciones mostradas en la aplicación de LUIS](../media/bot-builder-dotnet-use-luis/luis-intent-list.png)

3. <span data-ttu-id="3bce6-166">Haga clic en el botón **Entrenar** en la esquina superior derecha para entrenar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3bce6-166">Click the **Train** button in the upper right to train your app.</span></span>
4. <span data-ttu-id="3bce6-167">Haga clic en **PUBLICAR** en la barra de navegación superior para abrir la página **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="3bce6-167">Click **PUBLISH** in the top navigation bar to open the **Publish** page.</span></span> <span data-ttu-id="3bce6-168">Haga clic en el botón **Publish to production slot** (Publicar en el espacio de producción).</span><span class="sxs-lookup"><span data-stu-id="3bce6-168">Click the **Publish to production slot** button.</span></span> <span data-ttu-id="3bce6-169">Tras una publicación correcta, copie la dirección URL mostrada en la columna **Punto de conexión** de la página **Publicar aplicación**, en la fila que empieza con el nombre de recurso Starter_Key.</span><span class="sxs-lookup"><span data-stu-id="3bce6-169">After successful publish, copy the URL displayed in the **Endpoint** column the **Publish App** page, in the row that starts with the Resource Name Starter_Key.</span></span> <span data-ttu-id="3bce6-170">Guarde esta dirección URL para usarla más adelante en el código del bot.</span><span class="sxs-lookup"><span data-stu-id="3bce6-170">Save this URL to use later in your bot’s code.</span></span> <span data-ttu-id="3bce6-171">La dirección URL tiene un formato similar a este ejemplo: `https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx?subscription-key=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx&timezoneOffset=0&verbose=true&q=`.</span><span class="sxs-lookup"><span data-stu-id="3bce6-171">The URL has a format similar to this example: `https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx?subscription-key=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx&timezoneOffset=0&verbose=true&q=`</span></span>

## <a name="modify-the-bot-code"></a><span data-ttu-id="3bce6-172">Modificar el código del bot</span><span class="sxs-lookup"><span data-stu-id="3bce6-172">Modify the bot code</span></span>

<span data-ttu-id="3bce6-173">Haga clic en **Compilar** y después en **Open online code editor** (Abrir el editor de código en línea).</span><span class="sxs-lookup"><span data-stu-id="3bce6-173">Click **Build** and then click **Open online code editor**.</span></span>
    <span data-ttu-id="3bce6-174">![Abrir el editor de código en línea](../media/bot-builder-dotnet-use-luis/bot-service-build.png)</span><span class="sxs-lookup"><span data-stu-id="3bce6-174">![Open online code editor](../media/bot-builder-dotnet-use-luis/bot-service-build.png)</span></span>

<span data-ttu-id="3bce6-175">En el editor de código, abra `BasicLuisDialog.cs`.</span><span class="sxs-lookup"><span data-stu-id="3bce6-175">In the code editor, open `BasicLuisDialog.cs`.</span></span> <span data-ttu-id="3bce6-176">Contiene el código siguiente para controlar las intenciones de la aplicación de LUIS.</span><span class="sxs-lookup"><span data-stu-id="3bce6-176">It contains the following code for handling intents from the LUIS app.</span></span>
```cs
using System;
using System.Configuration;
using System.Threading.Tasks;

using Microsoft.Bot.Builder.Dialogs;
using Microsoft.Bot.Builder.Luis;
using Microsoft.Bot.Builder.Luis.Models;

namespace Microsoft.Bot.Sample.LuisBot
{
    // For more information about this template visit http://aka.ms/azurebots-csharp-luis
    [Serializable]
    public class BasicLuisDialog : LuisDialog<object>
    {
        public BasicLuisDialog() : base(new LuisService(new LuisModelAttribute(
            ConfigurationManager.AppSettings["LuisAppId"], 
            ConfigurationManager.AppSettings["LuisAPIKey"], 
            domain: ConfigurationManager.AppSettings["LuisAPIHostName"])))
        {
        }

        [LuisIntent("None")]
        public async Task NoneIntent(IDialogContext context, LuisResult result)
        {
            await this.ShowLuisResult(context, result);
        }

        // Go to https://luis.ai and create a new intent, then train/publish your luis app.
        // Finally replace "Greeting" with the name of your newly created intent in the following handler
        [LuisIntent("Greeting")]
        public async Task GreetingIntent(IDialogContext context, LuisResult result)
        {
            await this.ShowLuisResult(context, result);
        }

        [LuisIntent("Cancel")]
        public async Task CancelIntent(IDialogContext context, LuisResult result)
        {
            await this.ShowLuisResult(context, result);
        }

        [LuisIntent("Help")]
        public async Task HelpIntent(IDialogContext context, LuisResult result)
        {
            await this.ShowLuisResult(context, result);
        }

        private async Task ShowLuisResult(IDialogContext context, LuisResult result) 
        {
            await context.PostAsync($"You have reached {result.Intents[0].Intent}. You said: {result.Query}");
            context.Wait(MessageReceived);
        }
    }
}
```
### <a name="create-a-class-for-storing-notes"></a><span data-ttu-id="3bce6-177">Creación de una clase para almacenar notas</span><span class="sxs-lookup"><span data-stu-id="3bce6-177">Create a class for storing notes</span></span>

<span data-ttu-id="3bce6-178">Agregue la siguiente instrucción `using` en BasicLuisDialog.cs.</span><span class="sxs-lookup"><span data-stu-id="3bce6-178">Add the following `using` statement in BasicLuisDialog.cs.</span></span>

```cs
using System.Collections.Generic;
```

<span data-ttu-id="3bce6-179">Agregue el siguiente código en la clase `BasicLuisDialog`, después de la definición del constructor.</span><span class="sxs-lookup"><span data-stu-id="3bce6-179">Add the following code within the `BasicLuisDialog` class, after the constructor definition.</span></span>

```cs
        // Store notes in a dictionary that uses the title as a key
        private readonly Dictionary<string, Note> noteByTitle = new Dictionary<string, Note>();
        
        [Serializable]
        public sealed class Note : IEquatable<Note>
        {

            public string Title { get; set; }
            public string Text { get; set; }

            public override string ToString()
            {
                return $"[{this.Title} : {this.Text}]";
            }

            public bool Equals(Note other)
            {
                return other != null
                    && this.Text == other.Text
                    && this.Title == other.Title;
            }

            public override bool Equals(object other)
            {
                return Equals(other as Note);
            }

            public override int GetHashCode()
            {
                return this.Title.GetHashCode();
            }
        }

        // CONSTANTS        
        // Name of note title entity
        public const string Entity_Note_Title = "Note.Title";
        // Default note title
        public const string DefaultNoteTitle = "default";
```

### <a name="handle-the-notecreate-intent"></a><span data-ttu-id="3bce6-180">Control de la intención Note.Create</span><span class="sxs-lookup"><span data-stu-id="3bce6-180">Handle the Note.Create intent</span></span>
<span data-ttu-id="3bce6-181">Para controlar la intención Note.Create, agregue el código siguiente a la clase `BasicLuisDialog`.</span><span class="sxs-lookup"><span data-stu-id="3bce6-181">To handle the Note.Create intent, add the following code to the `BasicLuisDialog` class.</span></span>

```cs
        private Note noteToCreate;
        private string currentTitle;
        [LuisIntent("Note.Create")]
        public Task NoteCreateIntent(IDialogContext context, LuisResult result)
        {
            EntityRecommendation title;
            if (!result.TryFindEntity(Entity_Note_Title, out title))
            {
                // Prompt the user for a note title
                PromptDialog.Text(context, After_TitlePrompt, "What is the title of the note you want to create?");
            }
            else
            {
                var note = new Note() { Title = title.Entity };
                noteToCreate = this.noteByTitle[note.Title] = note;

                // Prompt the user for what they want to say in the note           
                PromptDialog.Text(context, After_TextPrompt, "What do you want to say in your note?");
            }
            
            return Task.CompletedTask;
        }
        
        
        private async Task After_TitlePrompt(IDialogContext context, IAwaitable<string> result)
        {
            EntityRecommendation title;
            // Set the title (used for creation, deletion, and reading)
            currentTitle = await result;
            if (currentTitle != null)
            {
                title = new EntityRecommendation(type: Entity_Note_Title) { Entity = currentTitle };
            }
            else
            {
                // Use the default note title
                title = new EntityRecommendation(type: Entity_Note_Title) { Entity = DefaultNoteTitle };
            }

            // Create a new note object 
            var note = new Note() { Title = title.Entity };
            // Add the new note to the list of notes and also save it in order to add text to it later
            noteToCreate = this.noteByTitle[note.Title] = note;

            // Prompt the user for what they want to say in the note           
            PromptDialog.Text(context, After_TextPrompt, "What do you want to say in your note?");

        }

        private async Task After_TextPrompt(IDialogContext context, IAwaitable<string> result)
        {
            // Set the text of the note
            noteToCreate.Text = await result;
            
            await context.PostAsync($"Created note **{this.noteToCreate.Title}** that says \"{this.noteToCreate.Text}\".");
            
            context.Wait(MessageReceived);
        }
```

### <a name="handle-the-notereadaloud-intent"></a><span data-ttu-id="3bce6-182">Control de la intención Note.ReadAloud</span><span class="sxs-lookup"><span data-stu-id="3bce6-182">Handle the Note.ReadAloud Intent</span></span>
<span data-ttu-id="3bce6-183">El bot puede usar la intención `Note.ReadAloud` para mostrar el contenido de una nota o de todas las notas, si no se encuentra el título de la nota.</span><span class="sxs-lookup"><span data-stu-id="3bce6-183">The bot can use the `Note.ReadAloud` intent to show the contents of a note, or of all the notes if the note title isn't detected.</span></span>

<span data-ttu-id="3bce6-184">Pegue el código siguiente en la clase `BasicLuisDialog`.</span><span class="sxs-lookup"><span data-stu-id="3bce6-184">Paste the following code into the `BasicLuisDialog` class.</span></span>
```cs
        [LuisIntent("Note.ReadAloud")]
        public async Task NoteReadAloudIntent(IDialogContext context, LuisResult result)
        {
            Note note;
            if (TryFindNote(result, out note))
            {
                await context.PostAsync($"**{note.Title}**: {note.Text}.");
            }
            else
            {
                // Print out all the notes if no specific note name was detected
                string NoteList = "Here's the list of all notes: \n\n";
                foreach (KeyValuePair<string, Note> entry in noteByTitle)
                {
                    Note noteInList = entry.Value;
                    NoteList += $"**{noteInList.Title}**: {noteInList.Text}.\n\n";
                }
                await context.PostAsync(NoteList);
            }

            context.Wait(MessageReceived);
        }
        
        public bool TryFindNote(string noteTitle, out Note note)
        {
            bool foundNote = this.noteByTitle.TryGetValue(noteTitle, out note); // TryGetValue returns false if no match is found.
            return foundNote;
        }
        
        public bool TryFindNote(LuisResult result, out Note note)
        {
            note = null;

            string titleToFind;

            EntityRecommendation title;
            if (result.TryFindEntity(Entity_Note_Title, out title))
            {
                titleToFind = title.Entity;
            }
            else
            {
                titleToFind = DefaultNoteTitle;
            }

            return this.noteByTitle.TryGetValue(titleToFind, out note); // TryGetValue returns false if no match is found.
        }
```

### <a name="handle-the-notedelete-intent"></a><span data-ttu-id="3bce6-185">Control de la intención Note.Delete</span><span class="sxs-lookup"><span data-stu-id="3bce6-185">Handle the Note.Delete intent</span></span>
<span data-ttu-id="3bce6-186">Pegue el código siguiente en la clase `BasicLuisDialog`.</span><span class="sxs-lookup"><span data-stu-id="3bce6-186">Paste the following code into the `BasicLuisDialog` class.</span></span>

```cs
        [LuisIntent("Note.Delete")]
        public async Task NoteDeleteIntent(IDialogContext context, LuisResult result)
        {
            Note note;
            if (TryFindNote(result, out note))
            {
                this.noteByTitle.Remove(note.Title);
                await context.PostAsync($"Note {note.Title} deleted");
            }
            else
            {                             
                // Prompt the user for a note title
                PromptDialog.Text(context, After_DeleteTitlePrompt, "What is the title of the note you want to delete?");                         
            }           
        }

        private async Task After_DeleteTitlePrompt(IDialogContext context, IAwaitable<string> result)
        {
            Note note;
            string titleToDelete = await result;
            bool foundNote = this.noteByTitle.TryGetValue(titleToDelete, out note);

            if (foundNote)
            {
                this.noteByTitle.Remove(note.Title);
                await context.PostAsync($"Note {note.Title} deleted");
            }
            else
            {
                await context.PostAsync($"Did not find note named {titleToDelete}.");
            }

            context.Wait(MessageReceived);
        }
```

## <a name="build-the-bot"></a><span data-ttu-id="3bce6-187">Compilar el bot</span><span class="sxs-lookup"><span data-stu-id="3bce6-187">Build the bot</span></span>
<span data-ttu-id="3bce6-188">Haga clic con el botón derecho en **build.cmd** en el editor de código y elija **Ejecutar desde la consola**.</span><span class="sxs-lookup"><span data-stu-id="3bce6-188">Right-click on **build.cmd** in the code editor and choose **Run from Console**.</span></span>

   ![Ejecutar build.cmd](../media/bot-builder-dotnet-use-luis/bot-service-run-console.png)

## <a name="test-the-bot"></a><span data-ttu-id="3bce6-190">Probar el bot</span><span class="sxs-lookup"><span data-stu-id="3bce6-190">Test the bot</span></span>

<span data-ttu-id="3bce6-191">En Azure Portal, haga clic en **Test in Web Chat** (Probar en Chat en web) para probar el bot.</span><span class="sxs-lookup"><span data-stu-id="3bce6-191">In the Azure Portal, click on **Test in Web Chat** to test the bot.</span></span> <span data-ttu-id="3bce6-192">Intente escribir mensajes como "Crear una nota", "Leer mis notas" y "Eliminar notas".</span><span class="sxs-lookup"><span data-stu-id="3bce6-192">Try type messages like "Create a note", "read my notes", and "delete notes".</span></span>
   <span data-ttu-id="3bce6-193">![Probar el bot de notas en Chat en web](../media/bot-builder-dotnet-use-luis/bot-service-test-notebot.png)</span><span class="sxs-lookup"><span data-stu-id="3bce6-193">![Test notes bot in Web Chat](../media/bot-builder-dotnet-use-luis/bot-service-test-notebot.png)</span></span>

> [!TIP]
> <span data-ttu-id="3bce6-194">Si ve que el bot no reconoce siempre la intención o las entidades correctas, proporcione a la aplicación de LUIS más ejemplos de expresiones para entrenarla y mejorar su rendimiento.</span><span class="sxs-lookup"><span data-stu-id="3bce6-194">If you find that your bot doesn't always recognize the correct intent or entities, improve your LUIS app's performance by giving it more example utterances to train it.</span></span> <span data-ttu-id="3bce6-195">Puede volver a entrenar la aplicación de LUIS sin modificar el código del bot.</span><span class="sxs-lookup"><span data-stu-id="3bce6-195">You can retrain your LUIS app without any modification to your bot's code.</span></span> <span data-ttu-id="3bce6-196">Vea [Add example utterances](/azure/cognitive-services/LUIS/add-example-utterances) (Agregar expresiones de ejemplo) y [Train and test your LUIS app](/azure/cognitive-services/LUIS/train-test) (Entrenar y probar la aplicación de LUIS).</span><span class="sxs-lookup"><span data-stu-id="3bce6-196">See [Add example utterances](/azure/cognitive-services/LUIS/add-example-utterances) and [train and test your LUIS app](/azure/cognitive-services/LUIS/train-test).</span></span>

> [!TIP]
> <span data-ttu-id="3bce6-197">Si se ejecuta el código del bot en un problema, compruebe lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="3bce6-197">If your bot code runs into an issue, check the following:</span></span>
> * <span data-ttu-id="3bce6-198">Ha [compilado el bot](./bot-builder-dotnet-luis-dialogs.md#build-the-bot).</span><span class="sxs-lookup"><span data-stu-id="3bce6-198">You have [built the bot](./bot-builder-dotnet-luis-dialogs.md#build-the-bot).</span></span>
> * <span data-ttu-id="3bce6-199">El código del bot define un controlador para cada intención en la aplicación de LUIS.</span><span class="sxs-lookup"><span data-stu-id="3bce6-199">Your bot code defines a handler for every intent in your LUIS app.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3bce6-200">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="3bce6-200">Next steps</span></span>

<span data-ttu-id="3bce6-201">Al probar el bot, puede observar como una intención de LUIS invoca las tareas.</span><span class="sxs-lookup"><span data-stu-id="3bce6-201">From trying the bot, you can see how tasks are invoked by a LUIS intent.</span></span> <span data-ttu-id="3bce6-202">Sin embargo, este sencillo ejemplo no permite interrumpir el diálogo activo actualmente.</span><span class="sxs-lookup"><span data-stu-id="3bce6-202">However, this simple example doesn't allow for interruption of the currently active dialog.</span></span> <span data-ttu-id="3bce6-203">Permitir y controlar interrupciones como "ayuda" o "cancelar" es un diseño flexible que representa lo que el usuario hace realmente.</span><span class="sxs-lookup"><span data-stu-id="3bce6-203">Allowing and handling interruptions like "help" or "cancel" is a flexible design that accounts for what users really do.</span></span> <span data-ttu-id="3bce6-204">Obtenga más información sobre el uso de diálogos puntuables, para que los diálogos puedan controlar las interrupciones.</span><span class="sxs-lookup"><span data-stu-id="3bce6-204">Learn more about using scorable dialogs so that your dialogs can handle interruptions.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="3bce6-205">Controladores de mensajes globales mediante diálogos puntuables</span><span class="sxs-lookup"><span data-stu-id="3bce6-205">Global message handlers using scorables</span></span>](bot-builder-dotnet-scorable-dialogs.md)

## <a name="additional-resources"></a><span data-ttu-id="3bce6-206">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="3bce6-206">Additional resources</span></span>

- [<span data-ttu-id="3bce6-207">Diálogos</span><span class="sxs-lookup"><span data-stu-id="3bce6-207">Dialogs</span></span>](bot-builder-dotnet-dialogs.md)
- [<span data-ttu-id="3bce6-208">Administración del flujo de conversación con diálogos</span><span class="sxs-lookup"><span data-stu-id="3bce6-208">Manage conversation flow with dialogs</span></span>](bot-builder-dotnet-manage-conversation-flow.md)
- <span data-ttu-id="3bce6-209"><a href="https://www.luis.ai" target="_blank">LUIS</a></span><span class="sxs-lookup"><span data-stu-id="3bce6-209"><a href="https://www.luis.ai" target="_blank">LUIS</a></span></span>
- <span data-ttu-id="3bce6-210"><a href="/dotnet/api/?view=botbuilder-3.11.0" target="_blank">Referencia de Bot Builder SDK para .NET</a></span><span class="sxs-lookup"><span data-stu-id="3bce6-210"><a href="/dotnet/api/?view=botbuilder-3.11.0" target="_blank">Bot Builder SDK for .NET Reference</a></span></span>

[LUIS]: https://www.luis.ai/
[NotesSample]: https://github.com/Microsoft/BotFramework-Samples/tree/master/docs-samples/CSharp/Simple-LUIS-Notes-Sample
[NotesSampleJSON]: https://github.com/Microsoft/BotFramework-Samples/blob/master/docs-samples/CSharp/Simple-LUIS-Notes-Sample/Notes.json
