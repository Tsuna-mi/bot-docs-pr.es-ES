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
ms.openlocfilehash: fc260f34f28e406dc88dd5b688d84cd79c7e9449
ms.sourcegitcommit: 2dc75701b169d822c9499e393439161bc87639d2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42905957"
---
# <a name="recognize-intents-and-entities-with-luis"></a><span data-ttu-id="bba4e-103">Reconocimiento de intenciones y entidades con LUIS</span><span class="sxs-lookup"><span data-stu-id="bba4e-103">Recognize intents and entities with LUIS</span></span> 

[!INCLUDE [pre-release-label](../includes/pre-release-label-v3.md)]

<span data-ttu-id="bba4e-104">Este artículo usa el ejemplo de un bot para tomar notas, a fin de demostrar cómo Language Understanding ([LUIS][LUIS]) ayuda a que el bot responda correctamente a la entrada de lenguaje natural.</span><span class="sxs-lookup"><span data-stu-id="bba4e-104">This article uses the example of a bot for taking notes, to demonstrate how Language Understanding ([LUIS][LUIS]) helps your bot respond appropriately to natural language input.</span></span> <span data-ttu-id="bba4e-105">Un bot detecta lo que un usuario desea hacer mediante la identificación de su **intención**.</span><span class="sxs-lookup"><span data-stu-id="bba4e-105">A bot detects what a user wants to do by identifying their **intent**.</span></span> <span data-ttu-id="bba4e-106">Esta intención se determina a partir de la entrada de texto, voz o **grabaciones de voz**.</span><span class="sxs-lookup"><span data-stu-id="bba4e-106">This intent is determined from spoken or textual input, or **utterances**.</span></span> <span data-ttu-id="bba4e-107">La intención asigna grabaciones de voz a acciones que realiza el bot.</span><span class="sxs-lookup"><span data-stu-id="bba4e-107">The intent maps utterances to actions that the bot takes.</span></span> <span data-ttu-id="bba4e-108">Por ejemplo, un bot de toma de notas reconoce una intención `Notes.Create` para invocar la funcionalidad para crear una nota.</span><span class="sxs-lookup"><span data-stu-id="bba4e-108">For example, a note-taking bot recognizes a `Notes.Create` intent to invoke the functionality for creating a note.</span></span> <span data-ttu-id="bba4e-109">Puede que un bot también necesite extraer **entidades**, que son palabras importantes de las grabaciones de voz.</span><span class="sxs-lookup"><span data-stu-id="bba4e-109">A bot may also need to extract **entities**, which are important words in utterances.</span></span> <span data-ttu-id="bba4e-110">En el ejemplo de un bot de toma de notas, la entidad `Notes.Title` identifica el título de cada nota.</span><span class="sxs-lookup"><span data-stu-id="bba4e-110">In the example of a note-taking bot, the `Notes.Title` entity identifies the title of each note.</span></span>

## <a name="create-a-language-understanding-bot-with-bot-service"></a><span data-ttu-id="bba4e-111">Crear un bot de Language Understanding con Bot Service</span><span class="sxs-lookup"><span data-stu-id="bba4e-111">Create a Language Understanding bot with Bot Service</span></span>

1. <span data-ttu-id="bba4e-112">En [Azure Portal](https://portal.azure.com), haga clic en **Crear nuevo recurso** en la hoja de menú y, después, en **Ver todo**.</span><span class="sxs-lookup"><span data-stu-id="bba4e-112">In the [Azure portal](https://portal.azure.com), select **Create new resource** in the menu blade and click **See all**.</span></span>

<!-- Start with the steps in [Create a bot with Bot Service](../bot-service-quickstart.md) to start creating a new bot service.  -->

    ![Create new resource](../media/bot-builder-dotnet-use-luis/bot-service-creation.png)

2. <span data-ttu-id="bba4e-113">En el cuadro de búsqueda, busque **Bot de aplicación web**.</span><span class="sxs-lookup"><span data-stu-id="bba4e-113">In the search box, search for **Web App Bot**.</span></span> 

    ![Crear recurso](../media/bot-builder-dotnet-use-luis/bot-service-selection.png)

3. <span data-ttu-id="bba4e-115">En la hoja **Servicio de bots**, proporcione la información necesaria y haga clic en **Crear**.</span><span class="sxs-lookup"><span data-stu-id="bba4e-115">In the **Bot Service** blade, provide the required information, and click **Create**.</span></span> <span data-ttu-id="bba4e-116">Esto crea e implementa el servicio de bots y la aplicación de LUIS en Azure.</span><span class="sxs-lookup"><span data-stu-id="bba4e-116">This creates and deploys the bot service and LUIS app to Azure.</span></span> 
   * <span data-ttu-id="bba4e-117">Establezca **Nombre de la aplicación** en el nombre del bot.</span><span class="sxs-lookup"><span data-stu-id="bba4e-117">Set **App name** to your bot’s name.</span></span> <span data-ttu-id="bba4e-118">El nombre se usa como el subdominio cuando el bot se implementa en la nube (por ejemplo, mynotesbot.azurewebsites.net).</span><span class="sxs-lookup"><span data-stu-id="bba4e-118">The name is used as the subdomain when your bot is deployed to the cloud (for example, mynotesbot.azurewebsites.net).</span></span> <span data-ttu-id="bba4e-119">Este nombre también se utiliza como el nombre de la aplicación de LUIS asociada con el bot.</span><span class="sxs-lookup"><span data-stu-id="bba4e-119">This name is also used as the name of the LUIS app associated with your bot.</span></span> <span data-ttu-id="bba4e-120">Cópielo para usarlo más adelante, para encontrar la aplicación de LUIS asociada con el bot.</span><span class="sxs-lookup"><span data-stu-id="bba4e-120">Copy it to use later, to find the LUIS app associated with the bot.</span></span>
   * <span data-ttu-id="bba4e-121">Seleccione la suscripción, el [grupo de recursos](/azure/azure-resource-manager/resource-group-overview), el plan de App Service y la [ubicación](https://azure.microsoft.com/en-us/regions/).</span><span class="sxs-lookup"><span data-stu-id="bba4e-121">Select the subscription, [resource group](/azure/azure-resource-manager/resource-group-overview), App service plan, and [location](https://azure.microsoft.com/en-us/regions/).</span></span>
   * <span data-ttu-id="bba4e-122">Seleccione la plantilla **Language Understanding (C#)** en el campo **Bot template** (Plantilla de bot).</span><span class="sxs-lookup"><span data-stu-id="bba4e-122">Select the **Language understanding (C#)** template for the **Bot template** field.</span></span>

     ![Hoja Servicio de bots](../media/bot-builder-dotnet-use-luis/bot-service-setting-callout-template.png)

   * <span data-ttu-id="bba4e-124">Active la casilla para confirmar los términos del servicio.</span><span class="sxs-lookup"><span data-stu-id="bba4e-124">Check the box to confirm to the terms of service.</span></span>

4. <span data-ttu-id="bba4e-125">Confirme que se ha implementado el servicio de bots.</span><span class="sxs-lookup"><span data-stu-id="bba4e-125">Confirm that the bot service has been deployed.</span></span>
    * <span data-ttu-id="bba4e-126">Haga clic en Notificaciones (el icono de la campana que se encuentra en el borde superior de Azure Portal).</span><span class="sxs-lookup"><span data-stu-id="bba4e-126">Click Notifications (the bell icon that is located along the top edge of the Azure portal).</span></span> <span data-ttu-id="bba4e-127">La notificación cambiará de **Implementación iniciada** a **Implementación correcta**.</span><span class="sxs-lookup"><span data-stu-id="bba4e-127">The notification will change from **Deployment started** to **Deployment succeeded**.</span></span>
    * <span data-ttu-id="bba4e-128">Después de que la notificación cambie a **Implementación correcta**, haga clic en **Ir al recurso** en esa notificación.</span><span class="sxs-lookup"><span data-stu-id="bba4e-128">After the notification changes to **Deployment succeeded**, click **Go to resource** on that notification.</span></span>

## <a name="try-the-bot"></a><span data-ttu-id="bba4e-129">Probar el bot</span><span class="sxs-lookup"><span data-stu-id="bba4e-129">Try the bot</span></span>

<span data-ttu-id="bba4e-130">Para confirmar que el bot se ha implementado, active las **Notificaciones**.</span><span class="sxs-lookup"><span data-stu-id="bba4e-130">Confirm that the bot has been deployed by checking the **Notifications**.</span></span> <span data-ttu-id="bba4e-131">Las notificaciones cambiarán de **Implementación en curso...** a **Implementación correcta**.</span><span class="sxs-lookup"><span data-stu-id="bba4e-131">The notifications will change from **Deployment in progress...** to **Deployment succeeded**.</span></span> <span data-ttu-id="bba4e-132">Haga clic en el botón **Ir al recurso** para abrir la hoja de recursos del bot.</span><span class="sxs-lookup"><span data-stu-id="bba4e-132">Click **Go to resource** button to open the bot's resources blade.</span></span>

<span data-ttu-id="bba4e-133">Una vez registrado el bot, haga clic en **Test in Web Chat** (Probar en Chat en web) para abrir el panel Chat en web.</span><span class="sxs-lookup"><span data-stu-id="bba4e-133">Once the bot is registered, click **Test in Web Chat** to open the Web Chat pane.</span></span> <span data-ttu-id="bba4e-134">Escriba "hello" ("hola") en el Chat en web.</span><span class="sxs-lookup"><span data-stu-id="bba4e-134">Type "hello" in Web Chat.</span></span>

  ![Probar el bot en Chat en web](../media/bot-builder-dotnet-use-luis/bot-service-web-chat.png)

<span data-ttu-id="bba4e-136">El bot responde "You have reached Greeting.</span><span class="sxs-lookup"><span data-stu-id="bba4e-136">The bot responds by saying "You have reached Greeting.</span></span> <span data-ttu-id="bba4e-137">You said: hello" ("Se ha puesto en contacto con Saludo. Ha dicho: hola").</span><span class="sxs-lookup"><span data-stu-id="bba4e-137">You said: hello".</span></span> <span data-ttu-id="bba4e-138">Esto confirma que el bot ha recibido el mensaje y lo ha pasado a una aplicación de LUIS predeterminada que ha creado.</span><span class="sxs-lookup"><span data-stu-id="bba4e-138">This confirms that the bot has received your message and passed it to a default LUIS app that it created.</span></span> <span data-ttu-id="bba4e-139">Esta aplicación de LUIS predeterminada ha detectado una intención Saludo.</span><span class="sxs-lookup"><span data-stu-id="bba4e-139">This default LUIS app detected a Greeting intent.</span></span>

## <a name="modify-the-luis-app"></a><span data-ttu-id="bba4e-140">Modificación de la aplicación de LUIS</span><span class="sxs-lookup"><span data-stu-id="bba4e-140">Modify the LUIS app</span></span>

<span data-ttu-id="bba4e-141">Inicie sesión en [https://www.luis.ai](https://www.luis.ai) con la misma cuenta que usa para iniciar sesión en Azure.</span><span class="sxs-lookup"><span data-stu-id="bba4e-141">Log in to [https://www.luis.ai](https://www.luis.ai) using the same account you use to log in to Azure.</span></span> <span data-ttu-id="bba4e-142">Haga clic en **Mis aplicaciones**.</span><span class="sxs-lookup"><span data-stu-id="bba4e-142">Click on **My apps**.</span></span> <span data-ttu-id="bba4e-143">En la lista de aplicaciones, busque la aplicación que comienza con el nombre especificado en **Nombre de la aplicación** en la hoja **Servicio de bots** cuando creó el servicio de bots.</span><span class="sxs-lookup"><span data-stu-id="bba4e-143">In the list of apps, find the app that begins with the name specified in **App name** in the **Bot Service** blade when you created the Bot Service.</span></span> 

<span data-ttu-id="bba4e-144">Se inicia la aplicación de LUIS con cuatro intenciones: Cancelar, Saludo, Ayuda y Ninguno.</span><span class="sxs-lookup"><span data-stu-id="bba4e-144">The LUIS app starts with 4 intents: Cancel: Greeting, Help, and None.</span></span> <!-- picture -->

<span data-ttu-id="bba4e-145">Los siguientes pasos agregan las intenciones Note.Create, Note.ReadAloud y Note.Delete:</span><span class="sxs-lookup"><span data-stu-id="bba4e-145">The following steps add the Note.Create, Note.ReadAloud, and Note.Delete intents:</span></span> 

1. <span data-ttu-id="bba4e-146">Haga clic en **Dominios creados previamente** en la parte inferior izquierda de la página.</span><span class="sxs-lookup"><span data-stu-id="bba4e-146">Click on **Prebuit Domains** in the lower left of the page.</span></span> <span data-ttu-id="bba4e-147">Busque el dominio **Nota** y haga clic en **Agregar dominio**.</span><span class="sxs-lookup"><span data-stu-id="bba4e-147">Find the **Note** domain and click **Add domain**.</span></span>

2. <span data-ttu-id="bba4e-148">En este tutorial no se usan todas las intenciones incluidas en el dominio **Nota** creado previamente.</span><span class="sxs-lookup"><span data-stu-id="bba4e-148">This tutorial doesn't use all of the intents included in the **Note** prebuilt domain.</span></span> <span data-ttu-id="bba4e-149">En la página **Intenciones**, haga clic en cada uno de los siguientes nombres de intenciones y luego haga clic en el botón **Eliminar intención**.</span><span class="sxs-lookup"><span data-stu-id="bba4e-149">In the **Intents** page, click on each of the following intent names and then click the **Delete Intent** button.</span></span>
   * <span data-ttu-id="bba4e-150">Note.ShowNext</span><span class="sxs-lookup"><span data-stu-id="bba4e-150">Note.ShowNext</span></span>
   * <span data-ttu-id="bba4e-151">Note.DeleteNoteItem</span><span class="sxs-lookup"><span data-stu-id="bba4e-151">Note.DeleteNoteItem</span></span>
   * <span data-ttu-id="bba4e-152">Note.Confirm</span><span class="sxs-lookup"><span data-stu-id="bba4e-152">Note.Confirm</span></span>
   * <span data-ttu-id="bba4e-153">Note.Clear</span><span class="sxs-lookup"><span data-stu-id="bba4e-153">Note.Clear</span></span>
   * <span data-ttu-id="bba4e-154">Note.CheckOffItem</span><span class="sxs-lookup"><span data-stu-id="bba4e-154">Note.CheckOffItem</span></span>
   * <span data-ttu-id="bba4e-155">Note.AddToNote</span><span class="sxs-lookup"><span data-stu-id="bba4e-155">Note.AddToNote</span></span>

   <span data-ttu-id="bba4e-156">Las únicas intenciones que deben permanecer en la aplicación de LUIS son las siguientes:</span><span class="sxs-lookup"><span data-stu-id="bba4e-156">The only intents that should remain in the LUIS app are the following:</span></span> 
   * <span data-ttu-id="bba4e-157">Note.ReadAloud</span><span class="sxs-lookup"><span data-stu-id="bba4e-157">Note.ReadAloud</span></span>
   * <span data-ttu-id="bba4e-158">Note.Create</span><span class="sxs-lookup"><span data-stu-id="bba4e-158">Note.Create</span></span>
   * <span data-ttu-id="bba4e-159">Note.Delete</span><span class="sxs-lookup"><span data-stu-id="bba4e-159">Note.Delete</span></span>
   * <span data-ttu-id="bba4e-160">None</span><span class="sxs-lookup"><span data-stu-id="bba4e-160">None</span></span>
   * <span data-ttu-id="bba4e-161">Ayuda</span><span class="sxs-lookup"><span data-stu-id="bba4e-161">Help</span></span>
   * <span data-ttu-id="bba4e-162">Greeting</span><span class="sxs-lookup"><span data-stu-id="bba4e-162">Greeting</span></span>
   * <span data-ttu-id="bba4e-163">Cancelar</span><span class="sxs-lookup"><span data-stu-id="bba4e-163">Cancel</span></span> 

     ![intenciones mostradas en la aplicación de LUIS](../media/bot-builder-dotnet-use-luis/luis-intent-list.png)

3. <span data-ttu-id="bba4e-165">Haga clic en el botón **Entrenar** en la esquina superior derecha para entrenar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bba4e-165">Click the **Train** button in the upper right to train your app.</span></span>
4. <span data-ttu-id="bba4e-166">Haga clic en **PUBLICAR** en la barra de navegación superior para abrir la página **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="bba4e-166">Click **PUBLISH** in the top navigation bar to open the **Publish** page.</span></span> <span data-ttu-id="bba4e-167">Haga clic en el botón **Publish to production slot** (Publicar en el espacio de producción).</span><span class="sxs-lookup"><span data-stu-id="bba4e-167">Click the **Publish to production slot** button.</span></span> <span data-ttu-id="bba4e-168">Tras una publicación correcta, copie la dirección URL mostrada en la columna **Punto de conexión** de la página **Publicar aplicación**, en la fila que empieza con el nombre de recurso Starter_Key.</span><span class="sxs-lookup"><span data-stu-id="bba4e-168">After successful publish, copy the URL displayed in the **Endpoint** column the **Publish App** page, in the row that starts with the Resource Name Starter_Key.</span></span> <span data-ttu-id="bba4e-169">Guarde esta dirección URL para usarla más adelante en el código del bot.</span><span class="sxs-lookup"><span data-stu-id="bba4e-169">Save this URL to use later in your bot’s code.</span></span> <span data-ttu-id="bba4e-170">La dirección URL tiene un formato similar a este ejemplo: `https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx?subscription-key=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx&timezoneOffset=0&verbose=true&q=`.</span><span class="sxs-lookup"><span data-stu-id="bba4e-170">The URL has a format similar to this example: `https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx?subscription-key=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx&timezoneOffset=0&verbose=true&q=`</span></span>

## <a name="modify-the-bot-code"></a><span data-ttu-id="bba4e-171">Modificar el código del bot</span><span class="sxs-lookup"><span data-stu-id="bba4e-171">Modify the bot code</span></span>

<span data-ttu-id="bba4e-172">Haga clic en **Compilar** y después en **Open online code editor** (Abrir el editor de código en línea).</span><span class="sxs-lookup"><span data-stu-id="bba4e-172">Click **Build** and then click **Open online code editor**.</span></span>
    <span data-ttu-id="bba4e-173">![Abrir el editor de código en línea](../media/bot-builder-dotnet-use-luis/bot-service-build.png)</span><span class="sxs-lookup"><span data-stu-id="bba4e-173">![Open online code editor](../media/bot-builder-dotnet-use-luis/bot-service-build.png)</span></span>

<span data-ttu-id="bba4e-174">En el editor de código, abra `BasicLuisDialog.cs`.</span><span class="sxs-lookup"><span data-stu-id="bba4e-174">In the code editor, open `BasicLuisDialog.cs`.</span></span> <span data-ttu-id="bba4e-175">Contiene el código siguiente para controlar las intenciones de la aplicación de LUIS.</span><span class="sxs-lookup"><span data-stu-id="bba4e-175">It contains the following code for handling intents from the LUIS app.</span></span>
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
### <a name="create-a-class-for-storing-notes"></a><span data-ttu-id="bba4e-176">Creación de una clase para almacenar notas</span><span class="sxs-lookup"><span data-stu-id="bba4e-176">Create a class for storing notes</span></span>

<span data-ttu-id="bba4e-177">Agregue la siguiente instrucción `using` en BasicLuisDialog.cs.</span><span class="sxs-lookup"><span data-stu-id="bba4e-177">Add the following `using` statement in BasicLuisDialog.cs.</span></span>

```cs
using System.Collections.Generic;
```

<span data-ttu-id="bba4e-178">Agregue el siguiente código en la clase `BasicLuisDialog`, después de la definición del constructor.</span><span class="sxs-lookup"><span data-stu-id="bba4e-178">Add the following code within the `BasicLuisDialog` class, after the constructor definition.</span></span>

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

### <a name="handle-the-notecreate-intent"></a><span data-ttu-id="bba4e-179">Control de la intención Note.Create</span><span class="sxs-lookup"><span data-stu-id="bba4e-179">Handle the Note.Create intent</span></span>
<span data-ttu-id="bba4e-180">Para controlar la intención Note.Create, agregue el código siguiente a la clase `BasicLuisDialog`.</span><span class="sxs-lookup"><span data-stu-id="bba4e-180">To handle the Note.Create intent, add the following code to the `BasicLuisDialog` class.</span></span>

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

### <a name="handle-the-notereadaloud-intent"></a><span data-ttu-id="bba4e-181">Control de la intención Note.ReadAloud</span><span class="sxs-lookup"><span data-stu-id="bba4e-181">Handle the Note.ReadAloud Intent</span></span>
<span data-ttu-id="bba4e-182">El bot puede usar la intención `Note.ReadAloud` para mostrar el contenido de una nota o de todas las notas, si no se encuentra el título de la nota.</span><span class="sxs-lookup"><span data-stu-id="bba4e-182">The bot can use the `Note.ReadAloud` intent to show the contents of a note, or of all the notes if the note title isn't detected.</span></span>

<span data-ttu-id="bba4e-183">Pegue el código siguiente en la clase `BasicLuisDialog`.</span><span class="sxs-lookup"><span data-stu-id="bba4e-183">Paste the following code into the `BasicLuisDialog` class.</span></span>
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

### <a name="handle-the-notedelete-intent"></a><span data-ttu-id="bba4e-184">Control de la intención Note.Delete</span><span class="sxs-lookup"><span data-stu-id="bba4e-184">Handle the Note.Delete intent</span></span>
<span data-ttu-id="bba4e-185">Pegue el código siguiente en la clase `BasicLuisDialog`.</span><span class="sxs-lookup"><span data-stu-id="bba4e-185">Paste the following code into the `BasicLuisDialog` class.</span></span>

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

## <a name="build-the-bot"></a><span data-ttu-id="bba4e-186">Compilar el bot</span><span class="sxs-lookup"><span data-stu-id="bba4e-186">Build the bot</span></span>
<span data-ttu-id="bba4e-187">Haga clic con el botón derecho en **build.cmd** en el editor de código y elija **Ejecutar desde la consola**.</span><span class="sxs-lookup"><span data-stu-id="bba4e-187">Right-click on **build.cmd** in the code editor and choose **Run from Console**.</span></span>

   ![Ejecutar build.cmd](../media/bot-builder-dotnet-use-luis/bot-service-run-console.png)

## <a name="test-the-bot"></a><span data-ttu-id="bba4e-189">Probar el bot</span><span class="sxs-lookup"><span data-stu-id="bba4e-189">Test the bot</span></span>

<span data-ttu-id="bba4e-190">En Azure Portal, haga clic en **Test in Web Chat** (Probar en Chat en web) para probar el bot.</span><span class="sxs-lookup"><span data-stu-id="bba4e-190">In the Azure Portal, click on **Test in Web Chat** to test the bot.</span></span> <span data-ttu-id="bba4e-191">Intente escribir mensajes como "Crear una nota", "Leer mis notas" y "Eliminar notas".</span><span class="sxs-lookup"><span data-stu-id="bba4e-191">Try type messages like "Create a note", "read my notes", and "delete notes".</span></span>
   <span data-ttu-id="bba4e-192">![Probar el bot de notas en Chat en web](../media/bot-builder-dotnet-use-luis/bot-service-test-notebot.png)</span><span class="sxs-lookup"><span data-stu-id="bba4e-192">![Test notes bot in Web Chat](../media/bot-builder-dotnet-use-luis/bot-service-test-notebot.png)</span></span>

> [!TIP]
> <span data-ttu-id="bba4e-193">Si ve que el bot no reconoce siempre la intención o las entidades correctas, proporcione a la aplicación de LUIS más ejemplos de expresiones para entrenarla y mejorar su rendimiento.</span><span class="sxs-lookup"><span data-stu-id="bba4e-193">If you find that your bot doesn't always recognize the correct intent or entities, improve your LUIS app's performance by giving it more example utterances to train it.</span></span> <span data-ttu-id="bba4e-194">Puede volver a entrenar la aplicación de LUIS sin modificar el código del bot.</span><span class="sxs-lookup"><span data-stu-id="bba4e-194">You can retrain your LUIS app without any modification to your bot's code.</span></span> <span data-ttu-id="bba4e-195">Vea [Add example utterances](/azure/cognitive-services/LUIS/add-example-utterances) (Agregar expresiones de ejemplo) y [Train and test your LUIS app](/azure/cognitive-services/LUIS/train-test) (Entrenar y probar la aplicación de LUIS).</span><span class="sxs-lookup"><span data-stu-id="bba4e-195">See [Add example utterances](/azure/cognitive-services/LUIS/add-example-utterances) and [train and test your LUIS app](/azure/cognitive-services/LUIS/train-test).</span></span>

> [!TIP]
> <span data-ttu-id="bba4e-196">Si se ejecuta el código del bot en un problema, compruebe lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="bba4e-196">If your bot code runs into an issue, check the following:</span></span>
> * <span data-ttu-id="bba4e-197">Ha [compilado el bot](./bot-builder-dotnet-luis-dialogs.md#build-the-bot).</span><span class="sxs-lookup"><span data-stu-id="bba4e-197">You have [built the bot](./bot-builder-dotnet-luis-dialogs.md#build-the-bot).</span></span>
> * <span data-ttu-id="bba4e-198">El código del bot define un controlador para cada intención en la aplicación de LUIS.</span><span class="sxs-lookup"><span data-stu-id="bba4e-198">Your bot code defines a handler for every intent in your LUIS app.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bba4e-199">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="bba4e-199">Next steps</span></span>

<span data-ttu-id="bba4e-200">Al probar el bot, puede observar como una intención de LUIS invoca las tareas.</span><span class="sxs-lookup"><span data-stu-id="bba4e-200">From trying the bot, you can see how tasks are invoked by a LUIS intent.</span></span> <span data-ttu-id="bba4e-201">Sin embargo, este sencillo ejemplo no permite interrumpir el diálogo activo actualmente.</span><span class="sxs-lookup"><span data-stu-id="bba4e-201">However, this simple example doesn't allow for interruption of the currently active dialog.</span></span> <span data-ttu-id="bba4e-202">Permitir y controlar interrupciones como "ayuda" o "cancelar" es un diseño flexible que representa lo que el usuario hace realmente.</span><span class="sxs-lookup"><span data-stu-id="bba4e-202">Allowing and handling interruptions like "help" or "cancel" is a flexible design that accounts for what users really do.</span></span> <span data-ttu-id="bba4e-203">Obtenga más información sobre el uso de diálogos puntuables, para que los diálogos puedan controlar las interrupciones.</span><span class="sxs-lookup"><span data-stu-id="bba4e-203">Learn more about using scorable dialogs so that your dialogs can handle interruptions.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="bba4e-204">Controladores de mensajes globales mediante diálogos puntuables</span><span class="sxs-lookup"><span data-stu-id="bba4e-204">Global message handlers using scorables</span></span>](bot-builder-dotnet-scorable-dialogs.md)

## <a name="additional-resources"></a><span data-ttu-id="bba4e-205">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="bba4e-205">Additional resources</span></span>

- [<span data-ttu-id="bba4e-206">Diálogos</span><span class="sxs-lookup"><span data-stu-id="bba4e-206">Dialogs</span></span>](bot-builder-dotnet-dialogs.md)
- [<span data-ttu-id="bba4e-207">Administración del flujo de conversación con diálogos</span><span class="sxs-lookup"><span data-stu-id="bba4e-207">Manage conversation flow with dialogs</span></span>](bot-builder-dotnet-manage-conversation-flow.md)
- <span data-ttu-id="bba4e-208"><a href="https://www.luis.ai" target="_blank">LUIS</a></span><span class="sxs-lookup"><span data-stu-id="bba4e-208"><a href="https://www.luis.ai" target="_blank">LUIS</a></span></span>
- <span data-ttu-id="bba4e-209"><a href="/dotnet/api/?view=botbuilder-3.11.0" target="_blank">Referencia de Bot Builder SDK para .NET</a></span><span class="sxs-lookup"><span data-stu-id="bba4e-209"><a href="/dotnet/api/?view=botbuilder-3.11.0" target="_blank">Bot Builder SDK for .NET Reference</a></span></span>

[LUIS]: https://www.luis.ai/
[NotesSample]: https://github.com/Microsoft/BotFramework-Samples/tree/master/docs-samples/CSharp/Simple-LUIS-Notes-Sample
[NotesSampleJSON]: https://github.com/Microsoft/BotFramework-Samples/blob/master/docs-samples/CSharp/Simple-LUIS-Notes-Sample/Notes.json
