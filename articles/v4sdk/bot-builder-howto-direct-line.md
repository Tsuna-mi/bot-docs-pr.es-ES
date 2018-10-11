---
title: Creación de un bot y un cliente de Direct Line | Microsoft Docs
description: Obtenga información sobre cómo crear un cliente y un bot de Direct Line con V4 del SDK de Bot Builder para .NET.
keywords: bot de direct line, cliente de direct line, canal personalizado, basado en consola, publicar
author: v-royhar
ms.author: v-royhar
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 4/16/18
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: 81b6f1f9373c18bd3aedb393cfc4966587bf24cb
ms.sourcegitcommit: 6c2426c43cd2212bdea1ecbbf8ed245145b3c30d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2018
ms.locfileid: "48852210"
---
# <a name="create-a-direct-line-bot-and-client"></a><span data-ttu-id="502fb-104">Creación de un bot y un cliente de Direct Line</span><span class="sxs-lookup"><span data-stu-id="502fb-104">Create a Direct Line bot and client</span></span>

[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]

<span data-ttu-id="502fb-105">Los bots de Direct Line de Microsoft Bot Framework son bots que pueden funcionar con un cliente personalizado de su propio diseño.</span><span class="sxs-lookup"><span data-stu-id="502fb-105">Microsoft Bot Framework Direct Line bots are bots that can function with a custom client of your own design.</span></span> <span data-ttu-id="502fb-106">Los bots de Direct Line son increíblemente parecidos a los bots normales.</span><span class="sxs-lookup"><span data-stu-id="502fb-106">Direct Line bots are remarkably similar to normal bots.</span></span> <span data-ttu-id="502fb-107">Simplemente no necesitan utilizar los canales proporcionados.</span><span class="sxs-lookup"><span data-stu-id="502fb-107">They just don't need to use the provided channels.</span></span>

<span data-ttu-id="502fb-108">Los clientes de Direct Line se pueden escribir para que sean lo que quiera.</span><span class="sxs-lookup"><span data-stu-id="502fb-108">Direct Line clients can be written to be whatever you want them to be.</span></span> <span data-ttu-id="502fb-109">Puede escribir un cliente de Android, un cliente de iOS o incluso una aplicación cliente basada en consola.</span><span class="sxs-lookup"><span data-stu-id="502fb-109">You can write an Android client, an iOS client, or even a console-based client application.</span></span>

<span data-ttu-id="502fb-110">En este tema obtendrá información sobre cómo crear e implementar un bot de Direct Line y cómo crear y ejecutar una aplicación cliente de Direct Line basada en consola.</span><span class="sxs-lookup"><span data-stu-id="502fb-110">In this topic you will learn how to create and deploy a Direct Line bot, and how to create and run a console-based Direct Line client app.</span></span>

## <a name="create-your-bot"></a><span data-ttu-id="502fb-111">Creación del bot</span><span class="sxs-lookup"><span data-stu-id="502fb-111">Create your bot</span></span>

<span data-ttu-id="502fb-112">Cada lenguaje sigue una ruta de acceso diferente para crear el bot.</span><span class="sxs-lookup"><span data-stu-id="502fb-112">Each language follows a different path to create the bot.</span></span> <span data-ttu-id="502fb-113">JavaScript crea el bot en Azure y luego modifica el código, mientras que C# crea el bot localmente y luego lo publica en Azure, pero ambos son métodos válidos y se pueden usar con cualquiera de estos lenguajes.</span><span class="sxs-lookup"><span data-stu-id="502fb-113">Javascript creates the bot in Azure, then modifies the code while C# creates the bot locally, then publishes it to Azure, but both are valid methods and can be used with either language.</span></span> <span data-ttu-id="502fb-114">Si quiere obtener más información sobre cómo publicar el bot en Azure, eche un vistazo a [deploying your bot in Azure](../bot-builder-howto-deploy-azure.md) (Implementación de un bot en Azure).</span><span class="sxs-lookup"><span data-stu-id="502fb-114">If you want details on publishing your bot to Azure, take a look at [deploying your bot in Azure](../bot-builder-howto-deploy-azure.md).</span></span>

# <a name="ctabcscreatebot"></a>[<span data-ttu-id="502fb-115">C#</span><span class="sxs-lookup"><span data-stu-id="502fb-115">C#</span></span>](#tab/cscreatebot)

### <a name="create-the-solution-in-visual-studio"></a><span data-ttu-id="502fb-116">Creación de la solución en Visual Studio</span><span class="sxs-lookup"><span data-stu-id="502fb-116">Create the solution in Visual Studio</span></span>

<span data-ttu-id="502fb-117">Para crear la solución para el bot de Direct Line, en Visual Studio 2015 o una versión posterior:</span><span class="sxs-lookup"><span data-stu-id="502fb-117">To create the solution for the Direct Line bot, in Visual Studio 2015 or later:</span></span>

1. <span data-ttu-id="502fb-118">Cree una nueva **aplicación web ASP.NET Core** en **Visual C#** > **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="502fb-118">Create a new **ASP.NET Core Web Application**, in **Visual C#** > **.NET Core**.</span></span>

1. <span data-ttu-id="502fb-119">Escriba **DirectLineBotSample** como nombre y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="502fb-119">For the name, enter **DirectLineBotSample**, and click **OK**.</span></span>

1. <span data-ttu-id="502fb-120">Asegúrese de que **.NET Core** y **ASP.NET Core 2.0** estén seleccionados, elija la plantilla de proyecto **Vacía** y, después, haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="502fb-120">Make sure **.NET Core** and **ASP.NET Core 2.0** are selected, choose the **Empty** project template, then click **OK**.</span></span>

#### <a name="add-dependencies"></a><span data-ttu-id="502fb-121">Adición de dependencias</span><span class="sxs-lookup"><span data-stu-id="502fb-121">Add dependencies</span></span>

1. <span data-ttu-id="502fb-122">En el **Explorador de soluciones**, haga clic con el botón derecho en **Dependencias** y, después, en **Administrar paquetes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="502fb-122">In **Solution Explorer**, right click **Dependencies**, then choose **Manage NuGet Packages**.</span></span>

1. <span data-ttu-id="502fb-123">Haga clic en **Examinar** y, a continuación, asegúrese de que la casilla **Incluir versión preliminar** esté activada.</span><span class="sxs-lookup"><span data-stu-id="502fb-123">Click **Browse**, then make sure the **Include prerelease** check box is checked.</span></span>

1. <span data-ttu-id="502fb-124">Busque e instale los siguientes paquetes NuGet:</span><span class="sxs-lookup"><span data-stu-id="502fb-124">Search for and install the following NuGet packages:</span></span>
    - <span data-ttu-id="502fb-125">Microsoft.Bot.Builder</span><span class="sxs-lookup"><span data-stu-id="502fb-125">Microsoft.Bot.Builder</span></span>
    - <span data-ttu-id="502fb-126">Microsoft.Bot.Builder.Core.Extensions</span><span class="sxs-lookup"><span data-stu-id="502fb-126">Microsoft.Bot.Builder.Core.Extensions</span></span>
    - <span data-ttu-id="502fb-127">Microsoft.Bot.Builder.Integration.AspNet.Core</span><span class="sxs-lookup"><span data-stu-id="502fb-127">Microsoft.Bot.Builder.Integration.AspNet.Core</span></span>
    - <span data-ttu-id="502fb-128">Microsoft.Rest.ClientRuntime</span><span class="sxs-lookup"><span data-stu-id="502fb-128">Microsoft.Rest.ClientRuntime</span></span>
    - <span data-ttu-id="502fb-129">Newtonsoft.Json</span><span class="sxs-lookup"><span data-stu-id="502fb-129">Newtonsoft.Json</span></span>

### <a name="create-the-appsettingsjson-file"></a><span data-ttu-id="502fb-130">Creación del archivo appsettings.json</span><span class="sxs-lookup"><span data-stu-id="502fb-130">Create the appsettings.json file</span></span>

<span data-ttu-id="502fb-131">El archivo appsettings.json contendrá el id. de la aplicación de Microsoft, la contraseña de la aplicación y la cadena de conexión de datos.</span><span class="sxs-lookup"><span data-stu-id="502fb-131">The appsettings.json file will contain the Microsoft App ID, App Password, and Data Connection string.</span></span> <span data-ttu-id="502fb-132">Como este bot no almacenará la información de estado, la cadena de conexión de datos permanecerá vacía.</span><span class="sxs-lookup"><span data-stu-id="502fb-132">Since this bot will not store state information, the Data Connection string remain empty.</span></span> <span data-ttu-id="502fb-133">Y, si solo usa Bot Framework Emulator, todas ellas pueden estar vacías.</span><span class="sxs-lookup"><span data-stu-id="502fb-133">And if you're only using the Bot Framework Emulator, all of them can remain empty.</span></span>

<span data-ttu-id="502fb-134">Para crear un archivo **appsettings.json**:</span><span class="sxs-lookup"><span data-stu-id="502fb-134">To create an **appsettings.json** file:</span></span>

1. <span data-ttu-id="502fb-135">Haga clic con el botón derecho en el proyecto **DirectLineBotSample** y elija **Agregar** > **Nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="502fb-135">Right-click the **DirectLineBotSample** Project, choose **Add** > **New Item**.</span></span>

1. <span data-ttu-id="502fb-136">En **ASP.NET Core**, haga clic en **Archivo de configuración de ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="502fb-136">In **ASP.NET Core**, click **ASP.NET Configuration File**.</span></span>

1. <span data-ttu-id="502fb-137">Escriba **appsettings.json** en el nombre y haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="502fb-137">Type **appsettings.json** for the name, and click **Add**.</span></span>

<span data-ttu-id="502fb-138">Reemplace el contenido del archivo appsettings.json por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="502fb-138">Replace the contents of the appsettings.json file with the following:</span></span>

```json
{
    "Logging": {
        "IncludeScopes": false,
        "Debug": {
            "LogLevel": {
                "Default": "Warning"
            }
        },
        "Console": {
            "LogLevel": {
                "Default": "Warning"
            }
        }
    },

    "MicrosoftAppId": "",
    "MicrosoftAppPassword": "",
    "DataConnectionString": ""
}
```

### <a name="edit-startupcs"></a><span data-ttu-id="502fb-139">Edición de Startup.cs</span><span class="sxs-lookup"><span data-stu-id="502fb-139">Edit Startup.cs</span></span>

<span data-ttu-id="502fb-140">Reemplace el contenido del archivo Startup.cs por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="502fb-140">Replace the contents of the Startup.cs file with the following:</span></span>

<span data-ttu-id="502fb-141">**Nota**: **DirectBot** se definirá en el paso siguiente, por lo que puede pasar por alto el subrayado rojo.</span><span class="sxs-lookup"><span data-stu-id="502fb-141">**Note**: **DirectBot** will be defined in the next step, so you can ignore the red underline on it.</span></span>

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Hosting;
using Microsoft.AspNetCore.Http;
using Microsoft.Bot.Builder.BotFramework;
using Microsoft.Bot.Builder.Integration.AspNet.Core;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;

namespace DirectLineBotSample
{
    public class Startup
    {
        public IConfiguration Configuration { get; }

        public Startup(IHostingEnvironment env)
        {
            var builder = new ConfigurationBuilder()
                .SetBasePath(env.ContentRootPath)
                .AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
                .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true)
                .AddEnvironmentVariables();
            Configuration = builder.Build();
        }

        // This method gets called by the runtime. Use this method to add services to the container.
        public void ConfigureServices(IServiceCollection services)
        {
            services.AddSingleton(_ => Configuration);
            services.AddBot<DirectBot>(options =>
            {
                options.CredentialProvider = new ConfigurationCredentialProvider(Configuration);
            });
        }

        // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
        public void Configure(IApplicationBuilder app, IHostingEnvironment env)
        {
            if (env.IsDevelopment())
            {
                app.UseDeveloperExceptionPage();
            }

            app.UseDefaultFiles();
            app.UseStaticFiles();
            app.UseBotFramework();
        }
    }
}
```

### <a name="create-the-directbot-class"></a><span data-ttu-id="502fb-142">Creación de la clase DirectBot</span><span class="sxs-lookup"><span data-stu-id="502fb-142">Create the DirectBot class</span></span>

<span data-ttu-id="502fb-143">La clase DirectBot contiene la mayor parte de la lógica para este bot.</span><span class="sxs-lookup"><span data-stu-id="502fb-143">The DirectBot class contains most of the logic for this bot.</span></span>

1. <span data-ttu-id="502fb-144">Haga clic en el **DirectLineBotSample** > **Agregar** > **Nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="502fb-144">Right-click the **DirectLineBotSample** > **Add** > **New Item**.</span></span>

1. <span data-ttu-id="502fb-145">En **ASP.NET Core**, haga clic en **Clase**.</span><span class="sxs-lookup"><span data-stu-id="502fb-145">In **ASP.NET Core**, click **Class**.</span></span>

1. <span data-ttu-id="502fb-146">Escriba **DirectBot.cs** en el nombre y haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="502fb-146">Type **DirectBot.cs** for the name, and click **Add**.</span></span>

<span data-ttu-id="502fb-147">Reemplace el contenido del archivo DirectBot.cs por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="502fb-147">Replace the contents of the DirectBot.cs file with the following:</span></span>

```csharp
using System.Threading.Tasks;
using Microsoft.Bot;
using Microsoft.Bot.Builder;
using Microsoft.Bot.Builder.Core.Extensions;
using Microsoft.Bot.Schema;

namespace DirectLineBotSample
{
    public class DirectBot : IBot
    {
        public Task OnTurn(ITurnContext context)
        {
            // Respond to the various activity types.
            switch (context.Activity.Type)
            {
                case ActivityTypes.Message:
                    // Respond to the incoming text message.
                    RespondToMessage(context);
                    break;

                case ActivityTypes.ConversationUpdate:
                    break;

                case ActivityTypes.ContactRelationUpdate:
                    break;

                case ActivityTypes.Typing:
                    break;

                case ActivityTypes.DeleteUserData:
                    break;
            }

            return Task.CompletedTask;
        }

        /// <summary>
        /// Responds to the incoming message by either sending a hero card, an image, 
        /// or echoing the user's message.
        /// </summary>
        /// <param name="context">The context of this conversation.</param>
        private void RespondToMessage(ITurnContext context)
        {
            switch (context.Activity.Text.Trim().ToLower())
            {
                case "hi":
                case "hello":
                case "help":
                    // Send the user an instruction message.
                    context.SendActivity("Welcome to the Bot to showcase the DirectLine API. " +
                    "Send \"Show me a hero card\" or \"Send me a BotFramework image\" to see how the " +
                    "DirectLine client supports custom channel data. Any other message will be echoed.");
                    break;

                case "show me a hero card":
                    // Create the hero card.
                    HeroCard heroCard = new HeroCard()
                    {
                        Title = "Sample Hero Card",
                        Text = "Displayed in the DirectLine client"
                    };

                    // Attach the hero card to a new activity.
                    context.SendActivity(MessageFactory.Attachment(heroCard.ToAttachment()));
                    break;

                case "send me a botframework image":
                    // Create the image attachment.
                    Attachment imageAttachment = new Attachment()
                    {
                        ContentType = "image/png",
                        ContentUrl = "https://docs.microsoft.com/en-us/bot-framework/media/how-it-works/architecture-resize.png",
                    };

                    // Attach the image attachment to a new activity.
                    context.SendActivity(MessageFactory.Attachment(imageAttachment));
                    break;

                default:
                    // No command was encountered. Echo the user's message.
                    context.SendActivity($"You said \"{context.Activity.Text}\"");
                    break;
            }
        }
    }
}
```

<span data-ttu-id="502fb-148">**Nota:** No es necesario cambiar el archivo Program.cs.</span><span class="sxs-lookup"><span data-stu-id="502fb-148">**Note:** The exiting Program.cs file does not need to be changed.</span></span>

<span data-ttu-id="502fb-149">Para comprobar que todo está en orden, presione **F6** para compilar el proyecto.</span><span class="sxs-lookup"><span data-stu-id="502fb-149">To verify things are in order, press **F6** to build the project.</span></span> <span data-ttu-id="502fb-150">No debería haber advertencias ni errores.</span><span class="sxs-lookup"><span data-stu-id="502fb-150">There should be no warnings or errors.</span></span>

### <a name="publish-the-bot-from-visual-studio"></a><span data-ttu-id="502fb-151">Publicación del bot desde Visual Studio</span><span class="sxs-lookup"><span data-stu-id="502fb-151">Publish the bot from Visual Studio</span></span>

1. <span data-ttu-id="502fb-152">En el Explorador de soluciones de Visual Studio, haga clic con el botón derecho en el proyecto **DirectLineBotSample** y luego en **Establecer como proyecto de inicio**.</span><span class="sxs-lookup"><span data-stu-id="502fb-152">In Visual Studio, on the Solutions Explorer, right click on the **DirectLineBotSample** project and click **Set as StartUp Project**.</span></span>

    ![Página de publicación de Visual Studio](media/bot-builder-howto-direct-line/visual-studio-publish-page.png)

1. <span data-ttu-id="502fb-154">Vuelva a hacer clic con el botón derecho en **DirectLineBotSample** y luego en **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="502fb-154">Right click **DirectLineBotSample** again and click **Publish**.</span></span> <span data-ttu-id="502fb-155">Se muestra la página **Publicar** de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="502fb-155">The Visual Studio **Publish** page appears.</span></span>

1. <span data-ttu-id="502fb-156">Si ya tiene un perfil de publicación para este bot, selecciónelo y haga clic en el botón **Publicar** y, después, vaya a la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="502fb-156">If you already have a publishing profile for this bot, select it and click the **Publish** button, then go on to the next section.</span></span>

1. <span data-ttu-id="502fb-157">Para crear un nuevo perfil de publicación, seleccione el icono **Microsoft Azure App Service**.</span><span class="sxs-lookup"><span data-stu-id="502fb-157">To create a new publishing profile, select the **Microsoft Azure App Service** icon.</span></span>

1. <span data-ttu-id="502fb-158">Seleccione **Crear nuevo**.</span><span class="sxs-lookup"><span data-stu-id="502fb-158">Select **Create new**.</span></span>

1. <span data-ttu-id="502fb-159">Haga clic en el botón **Publicar** .</span><span class="sxs-lookup"><span data-stu-id="502fb-159">Click the **Publish** button.</span></span> <span data-ttu-id="502fb-160">Se muestra el cuadro de diálogo **Create App Service** (Crear una instancia de App Service).</span><span class="sxs-lookup"><span data-stu-id="502fb-160">The **Create App Service** dialog appears.</span></span>

    ![Cuadro de diálogo Create App Service (Crear una instancia de App Service)](media/bot-builder-howto-direct-line/create-app-service-dialog.png)

    - <span data-ttu-id="502fb-162">En Nombre de la aplicación, asígnele un nombre que pueda encontrar más adelante.</span><span class="sxs-lookup"><span data-stu-id="502fb-162">For App Name, give it a name you can find later.</span></span> <span data-ttu-id="502fb-163">Por ejemplo, puede agregar su nombre de correo electrónico al principio del nombre de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="502fb-163">For example, you can add your email name to the beginning of the App Name.</span></span>

    - <span data-ttu-id="502fb-164">Compruebe si está usando la suscripción correcta.</span><span class="sxs-lookup"><span data-stu-id="502fb-164">Verify you are using the correct subscription.</span></span>

    - <span data-ttu-id="502fb-165">En Grupo de recursos, compruebe que usa el grupo de recursos correcto.</span><span class="sxs-lookup"><span data-stu-id="502fb-165">For Resource Group, verify you are using the correct resource group.</span></span> <span data-ttu-id="502fb-166">El grupo de recursos puede encontrarse en la hoja **Introducción** del bot.</span><span class="sxs-lookup"><span data-stu-id="502fb-166">The resource group can be found on the **Overview** blade of your bot.</span></span> <span data-ttu-id="502fb-167">**Nota:** Un grupo de recursos incorrecto es difícil de corregir.</span><span class="sxs-lookup"><span data-stu-id="502fb-167">**Note:** An incorrect resource group is difficult to correct.</span></span>

    - <span data-ttu-id="502fb-168">Compruebe si está usando el plan correcto de App Service.</span><span class="sxs-lookup"><span data-stu-id="502fb-168">Verify you are using the correct App Service Plan.</span></span>
    
1. <span data-ttu-id="502fb-169">Haga clic en el botón **Crear**.</span><span class="sxs-lookup"><span data-stu-id="502fb-169">Click the **Create** button.</span></span> <span data-ttu-id="502fb-170">Visual Studio inicia la implementación del bot.</span><span class="sxs-lookup"><span data-stu-id="502fb-170">Visual Studio will begin deploying your bot.</span></span>

<span data-ttu-id="502fb-171">Una vez publicado el bot, aparecerá un explorador con el punto de conexión de dirección URL del bot.</span><span class="sxs-lookup"><span data-stu-id="502fb-171">After your bot is published, a browser will appear with the URL endpoint of your bot.</span></span>

### <a name="create-bot-channels-registration-bot-on-microsoft-azure"></a><span data-ttu-id="502fb-172">Creación de un bot de registro de canales de bot en Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="502fb-172">Create Bot Channels Registration bot on Microsoft Azure</span></span>

<span data-ttu-id="502fb-173">El bot de Direct Line puede hospedarse en cualquier plataforma.</span><span class="sxs-lookup"><span data-stu-id="502fb-173">The Direct Line bot can be hosted on any platform.</span></span> <span data-ttu-id="502fb-174">En este ejemplo, el bot se hospedará en Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="502fb-174">In this example, the bot will be hosted on Microsoft Azure.</span></span> 

<span data-ttu-id="502fb-175">Para crear el bot en Microsoft Azure:</span><span class="sxs-lookup"><span data-stu-id="502fb-175">To create the bot on Microsoft Azure:</span></span>

1. <span data-ttu-id="502fb-176">En Microsoft Azure Portal, haga clic en **Crear un recurso** y, a continuación, busque "Bot Channels Registration" (Registro de canales de bot).</span><span class="sxs-lookup"><span data-stu-id="502fb-176">On the Microsoft Azure Portal, click **Create a resource**, then search for "Bot Channels Registration".</span></span>

1. <span data-ttu-id="502fb-177">Haga clic en **Create**(Crear).</span><span class="sxs-lookup"><span data-stu-id="502fb-177">Click **Create**.</span></span> <span data-ttu-id="502fb-178">Aparece la hoja Bot Channels Registration (Registro de canales de bot).</span><span class="sxs-lookup"><span data-stu-id="502fb-178">The Bot Channels Registration blade appears.</span></span>

    ![Hoja Bot Channels Registration (Registro de canales de bot), con los campos de nombre del bot, suscripción, grupo de recursos, ubicación, plan de tarifa y punto de conexión de mensajería, entre otros.](media/bot-builder-howto-direct-line/bot-service-registration-blade.png)

1. <span data-ttu-id="502fb-180">En la hoja Bot Channels Registration (Registro de canales de bot), escriba el **nombre del bot**, la **suscripción**, el **grupo de recursos**, la **ubicación** y el **plan de tarifa**.</span><span class="sxs-lookup"><span data-stu-id="502fb-180">On the Bot Channels Registration blade, enter the **Bot name**, **Subscription**, **Resource group**, **Location**, and **Pricing tier**.</span></span>

1. <span data-ttu-id="502fb-181">Deje el **Punto de conexión de mensajería** en blanco.</span><span class="sxs-lookup"><span data-stu-id="502fb-181">Leave the **Messaging endpoint** blank.</span></span> <span data-ttu-id="502fb-182">Este valor se rellenará más adelante.</span><span class="sxs-lookup"><span data-stu-id="502fb-182">This value will be filled in later.</span></span>

1. <span data-ttu-id="502fb-183">Haga clic en **Id. y contraseña de aplicación de Microsoft** y, a continuación, haga clic en **Creación automática del id. y la contraseña de la aplicación**.</span><span class="sxs-lookup"><span data-stu-id="502fb-183">Click the **Microsoft App ID and password**, then click **Auto create App ID and password**.</span></span>

1. <span data-ttu-id="502fb-184">Seleccione la casilla **Anclar al panel**.</span><span class="sxs-lookup"><span data-stu-id="502fb-184">Check the **Pin to dashboard** check box.</span></span>

1. <span data-ttu-id="502fb-185">Haga clic en el botón **Crear**.</span><span class="sxs-lookup"><span data-stu-id="502fb-185">Click the **Create** button.</span></span>

<span data-ttu-id="502fb-186">La implementación tardará unos minutos, pero, una vez hecho esto, debería aparecer en el panel.</span><span class="sxs-lookup"><span data-stu-id="502fb-186">Deployment will take a few minutes, but once that's done it should appear in your dashboard.</span></span>

### <a name="update-the-appsettingsjson-file"></a><span data-ttu-id="502fb-187">Actualización del archivo appsettings.json</span><span class="sxs-lookup"><span data-stu-id="502fb-187">Update the appsettings.json file</span></span>

1. <span data-ttu-id="502fb-188">Haga clic en el bot en el panel o, para ir al nuevo recurso, haga clic en **Todos los recursos** y busque el nombre de su registro de canales de bot.</span><span class="sxs-lookup"><span data-stu-id="502fb-188">Click the bot on your dashboard, or go to your new resource by clicking **All resources** and searching for the name of your Bot Channels Registration.</span></span>

1. <span data-ttu-id="502fb-189">Haga clic en **Descripción general**.</span><span class="sxs-lookup"><span data-stu-id="502fb-189">Click on **Overview**.</span></span>

1. <span data-ttu-id="502fb-190">Copie el nombre del grupo de recursos en la cadena **botId** en **Program.cs** del proyecto **DirectLineClientSample**.</span><span class="sxs-lookup"><span data-stu-id="502fb-190">Copy the name of the Resource group into the **botId** string in the **Program.cs** of the **DirectLineClientSample** project.</span></span>

1. <span data-ttu-id="502fb-191">Haga clic en **Configuración** y, a continuación, haga clic en **Administrar** cerca del campo **Microsoft App ID** (Id. de la aplicación de Microsoft).</span><span class="sxs-lookup"><span data-stu-id="502fb-191">Click **Settings**, then click **Manage** near the **Microsoft App ID** field.</span></span>

1. <span data-ttu-id="502fb-192">Copie el id. de la aplicación y péguelo en el campo **"MicrosoftAppId"** del archivo **appsettings.json**.</span><span class="sxs-lookup"><span data-stu-id="502fb-192">Copy the Application ID and paste it into the **"MicrosoftAppId"** field of the **appsettings.json** file.</span></span>

1. <span data-ttu-id="502fb-193">Haga clic en el botón **Generar nueva contraseña**.</span><span class="sxs-lookup"><span data-stu-id="502fb-193">Click the **Generate New Password** button.</span></span>

1. <span data-ttu-id="502fb-194">Copie la nueva contraseña y péguela en el campo **"MicrosoftAppPassword"** del archivo **appsettings.json**.</span><span class="sxs-lookup"><span data-stu-id="502fb-194">Copy the new password and paste it into the **"MicrosoftAppPassword"** field of the **appsettings.json** file.</span></span>

### <a name="set-the-messaging-endpoint"></a><span data-ttu-id="502fb-195">Establecimiento del punto de conexión de mensajería</span><span class="sxs-lookup"><span data-stu-id="502fb-195">Set the Messaging endpoint</span></span>

<span data-ttu-id="502fb-196">Para agregar el punto de conexión al bot:</span><span class="sxs-lookup"><span data-stu-id="502fb-196">To add the endpoint to your bot:</span></span>

1. <span data-ttu-id="502fb-197">Copie la dirección URL del explorador que apareció después de publicar el bot.</span><span class="sxs-lookup"><span data-stu-id="502fb-197">Copy the URL address from the browser that popped up after publishing your bot.</span></span>

    ![Hoja de configuración del registro de canales de bot, con el punto de conexión de mensajería resaltado](media/bot-builder-howto-direct-line/bot-channels-registration-settings.png)

1. <span data-ttu-id="502fb-199">Aparece la hoja **Configuración** del bot.</span><span class="sxs-lookup"><span data-stu-id="502fb-199">Bring up the **Settings** blade of your bot.</span></span>

1. <span data-ttu-id="502fb-200">Pegue la dirección en el **punto de conexión de mensajería**.</span><span class="sxs-lookup"><span data-stu-id="502fb-200">Paste the address into the **Messaging endpoint**.</span></span>

1. <span data-ttu-id="502fb-201">Edite la dirección para que empiece por "https://" y termine por "/api/messages".</span><span class="sxs-lookup"><span data-stu-id="502fb-201">Edit the address to start with "https://" and end with "/api/messages".</span></span> <span data-ttu-id="502fb-202">Por ejemplo, si la dirección copiada del explorador es "http://v-royhar-dlbot-directlinebotsample20180329044602.azurewebsites.net", modifíquela a "https://v-royhar-dlbot-directlinebotsample20180329044602.azurewebsites.net/api/messages".</span><span class="sxs-lookup"><span data-stu-id="502fb-202">For example, if the address copied from the browser is "http://v-royhar-dlbot-directlinebotsample20180329044602.azurewebsites.net", edit it to "https://v-royhar-dlbot-directlinebotsample20180329044602.azurewebsites.net/api/messages".</span></span>

1. <span data-ttu-id="502fb-203">Haga clic en el botón **Guardar** en la hoja de configuración.</span><span class="sxs-lookup"><span data-stu-id="502fb-203">Click the **Save** button on the Settings blade.</span></span>

<span data-ttu-id="502fb-204">**NOTA:** Si se produce un error de publicación, es posible que deba detener el bot en Azure para poder publicar los cambios en el bot.</span><span class="sxs-lookup"><span data-stu-id="502fb-204">**NOTE:** If publishing fails, you may need to stop your bot on Azure before you can publish changes to your bot.</span></span>

1. <span data-ttu-id="502fb-205">Busque el nombre de App Service (se encuentra entre "https://" y ".azurewebsites.net").</span><span class="sxs-lookup"><span data-stu-id="502fb-205">Find your App Service name (it's between the "https://" and ".azurewebsites.net".</span></span>

1. <span data-ttu-id="502fb-206">En "Todos los recursos" de Azure Portal, busque ese recurso.</span><span class="sxs-lookup"><span data-stu-id="502fb-206">Search for that resource in the Azure Portal "All resources".</span></span>

1. <span data-ttu-id="502fb-207">Haga clic en el recurso de App Service.</span><span class="sxs-lookup"><span data-stu-id="502fb-207">Click on the App Service resource.</span></span> <span data-ttu-id="502fb-208">Aparece la hoja de App Service.</span><span class="sxs-lookup"><span data-stu-id="502fb-208">The App Serivce blade appears.</span></span>

1. <span data-ttu-id="502fb-209">Haga clic en el botón **Detener** en la hoja de App Service.</span><span class="sxs-lookup"><span data-stu-id="502fb-209">Click the **Stop** button on the App Service blade.</span></span>

1. <span data-ttu-id="502fb-210">En Visual Studio, vuelva a publicar el bot.</span><span class="sxs-lookup"><span data-stu-id="502fb-210">In Visual Studio, Publish your bot again.</span></span>

1. <span data-ttu-id="502fb-211">Haga clic en el botón **Inicio** en la hoja de App Service.</span><span class="sxs-lookup"><span data-stu-id="502fb-211">Click the **Start** button on the App Service blade.</span></span>

### <a name="test-your-bot-in-webchat"></a><span data-ttu-id="502fb-212">Prueba del bot en el chat en web</span><span class="sxs-lookup"><span data-stu-id="502fb-212">Test your bot in webchat</span></span>

<span data-ttu-id="502fb-213">Para comprobar que el bot funciona, compruébelo en el Chat en web:</span><span class="sxs-lookup"><span data-stu-id="502fb-213">To verify that your bot is working, check your bot in webchat:</span></span>

1. <span data-ttu-id="502fb-214">En la hoja Bot Channels Registration (Registro de canales del bot) del bot, haga clic en **Configuración** y, a continuación, en **Probar en Chat en web**.</span><span class="sxs-lookup"><span data-stu-id="502fb-214">In the Bot Channels Registration blade for your bot, click **Settings**, then **Test in Web Chat**.</span></span>

1. <span data-ttu-id="502fb-215">Escriba "Hola".</span><span class="sxs-lookup"><span data-stu-id="502fb-215">Type "Hi".</span></span> <span data-ttu-id="502fb-216">El bot debería responder con un mensaje de bienvenida.</span><span class="sxs-lookup"><span data-stu-id="502fb-216">The bot should respond with a welcome message.</span></span>

1. <span data-ttu-id="502fb-217">Escriba "Mostrarme una tarjeta de imagen prominente".</span><span class="sxs-lookup"><span data-stu-id="502fb-217">Type "show me a hero card".</span></span> <span data-ttu-id="502fb-218">El bot debería mostrar una tarjeta de imagen prominente.</span><span class="sxs-lookup"><span data-stu-id="502fb-218">The bot should display a hero card.</span></span>

1. <span data-ttu-id="502fb-219">Escriba "Enviarme una imagen de botframework".</span><span class="sxs-lookup"><span data-stu-id="502fb-219">Type "send me a botframework image".</span></span> <span data-ttu-id="502fb-220">El bot debería mostrar una imagen de la documentación de Bot Framework.</span><span class="sxs-lookup"><span data-stu-id="502fb-220">The bot should display an image from the Bot Framework documentation.</span></span>

1. <span data-ttu-id="502fb-221">Escriba cualquier otra cosa y el bot debería responder con "Ha dicho" y el mensaje entre comillas.</span><span class="sxs-lookup"><span data-stu-id="502fb-221">Type anything else and the bot should reply with, "You said" and your message in quotes.</span></span>

### <a name="troubleshooting"></a><span data-ttu-id="502fb-222">solución de problemas</span><span class="sxs-lookup"><span data-stu-id="502fb-222">Troubleshooting</span></span>

<span data-ttu-id="502fb-223">Si el bot no funciona, compruebe o vuelva a escribir el id. y la contraseña de aplicación de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="502fb-223">If your bot is not working, verify or reenter your Microsoft App ID and password.</span></span> <span data-ttu-id="502fb-224">Aunque lo haya copiado con anterioridad, compruebe el id. de la aplicación de Microsoft en la hoja de configuración de Bot Channels Registration (Registro de canales del bot) con el valor del campo **"MicrosoftAppId"** del archivo appsettings.json.</span><span class="sxs-lookup"><span data-stu-id="502fb-224">Even if you copied it before, check the Microsoft App ID on the settings blade of your Bot Channels Registration against the value in the **"MicrosoftAppId"** field in the appsettings.json file.</span></span>

<span data-ttu-id="502fb-225">Compruebe la contraseña o cree y use una nueva:</span><span class="sxs-lookup"><span data-stu-id="502fb-225">Verify your password or create and use a new password:</span></span> 

1. <span data-ttu-id="502fb-226">Haga clic en **Administrar** junto al campo **Microsoft App ID** (Id. de la aplicación de Microsoft) en la hoja Bot Channels Registration (Registro de canales del bot).</span><span class="sxs-lookup"><span data-stu-id="502fb-226">Click **Manage** next to the **Microsoft App ID** field on your Bot Channels Registration blade.</span></span>

1. <span data-ttu-id="502fb-227">Inicie sesión en el Portal de registro de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="502fb-227">Log into the Application Registration Portal.</span></span>

1. <span data-ttu-id="502fb-228">Compruebe si las tres primeras letras de la contraseña coinciden con el campo **"MicrosoftAppPassword"** del archivo **appsettings.json**.</span><span class="sxs-lookup"><span data-stu-id="502fb-228">Verify the first three letters of your password match the **"MicrosoftAppPassword"** field in the **appsettings.json** file.</span></span> 

1. <span data-ttu-id="502fb-229">Si los valores no coinciden, genere una nueva contraseña y almacene ese valor en el campo **"MicrosoftAppPassword"** del archivo **appsettings.json**.</span><span class="sxs-lookup"><span data-stu-id="502fb-229">If the values do not match, generate a new password and store that value in the **"MicrosoftAppPassword"** field in the **appsettings.json** file.</span></span>

# <a name="javascripttabjscreatebot"></a>[<span data-ttu-id="502fb-230">JavaScript</span><span class="sxs-lookup"><span data-stu-id="502fb-230">JavaScript</span></span>](#tab/jscreatebot)

### <a name="create-web-app-bot-on-microsoft-azure"></a><span data-ttu-id="502fb-231">Creación de una aplicación web en Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="502fb-231">Create Web App Bot on Microsoft Azure</span></span>

<span data-ttu-id="502fb-232">Para crear el bot en Microsoft Azure:</span><span class="sxs-lookup"><span data-stu-id="502fb-232">To create the bot on Microsoft Azure:</span></span> 

1. <span data-ttu-id="502fb-233">En Microsoft Azure Portal, haga clic en **Crear un recurso** y, a continuación, busque "Bot de aplicación web".</span><span class="sxs-lookup"><span data-stu-id="502fb-233">On the Microsoft Azure Portal, click **Create a resource**, then search for "Web App Bot".</span></span>

1. <span data-ttu-id="502fb-234">Haga clic en **Create**(Crear).</span><span class="sxs-lookup"><span data-stu-id="502fb-234">Click **Create**.</span></span> <span data-ttu-id="502fb-235">Aparece la hoja Bot de aplicación web.</span><span class="sxs-lookup"><span data-stu-id="502fb-235">The Web App Bot blade appears.</span></span>

![Registro del bot de aplicación web](media/bot-builder-howto-direct-line/web-app-bot-registration.png)

1. <span data-ttu-id="502fb-237">En la hoja Bot de aplicación web, escriba el **nombre del bot**, la **suscripción**, el **grupo de recursos**, la **ubicación**, el **plan de tarifa** y el **nombre de la aplicación**.</span><span class="sxs-lookup"><span data-stu-id="502fb-237">On the Web App Bot blade, enter the **Bot name**, **Subscription**, **Resource group**, **Location**, **Pricing tier**, **App name**.</span></span>

1. <span data-ttu-id="502fb-238">Seleccione la plantilla de bot que quiere utilizar.</span><span class="sxs-lookup"><span data-stu-id="502fb-238">Select the bot template you would like to use.</span></span> <span data-ttu-id="502fb-239">**Versión de SDK: SDK v4** y **lenguaje de SDK: Node.js**</span><span class="sxs-lookup"><span data-stu-id="502fb-239">**SDK version: SDK v4** and **SDK language: Node.js**</span></span> 

1. <span data-ttu-id="502fb-240">Seleccione la plantilla básica de versión preliminar de v4.</span><span class="sxs-lookup"><span data-stu-id="502fb-240">Select the basic v4 preview template.</span></span> 

1. <span data-ttu-id="502fb-241">Elija el **plan de App Service/ubicación**, **Azure Storage** y la **ubicación de Application Insights**.</span><span class="sxs-lookup"><span data-stu-id="502fb-241">Choose your **App service plan/Location**, **Azure Storage** and **Application Insights Location**.</span></span>

1. <span data-ttu-id="502fb-242">Seleccione la casilla Anclar al panel.</span><span class="sxs-lookup"><span data-stu-id="502fb-242">Check the Pin to dashboard check box.</span></span>

1. <span data-ttu-id="502fb-243">Haga clic en el botón Crear.</span><span class="sxs-lookup"><span data-stu-id="502fb-243">Click the Create button.</span></span>

1. <span data-ttu-id="502fb-244">Espere a que se implemente el bot.</span><span class="sxs-lookup"><span data-stu-id="502fb-244">Wait for your bot to be deployed.</span></span> <span data-ttu-id="502fb-245">Como seleccionó la casilla Anclar al panel, el bot aparecerá en el panel.</span><span class="sxs-lookup"><span data-stu-id="502fb-245">Because you checked the Pin to dashboard check box, your bot will appear on your dashboard.</span></span>

### <a name="edit-your-direct-line-bot"></a><span data-ttu-id="502fb-246">Edición del bot de Direct Line</span><span class="sxs-lookup"><span data-stu-id="502fb-246">Edit your Direct Line Bot</span></span>

<span data-ttu-id="502fb-247">Ahora vamos a agregar algunas características más al bot de eco.</span><span class="sxs-lookup"><span data-stu-id="502fb-247">Now lets add some more features to the echo bot.</span></span>

1. <span data-ttu-id="502fb-248">En la hoja Bot de aplicación web, haga clic en Crear.</span><span class="sxs-lookup"><span data-stu-id="502fb-248">On the Web App Bot blade, click Build.</span></span> 

1. <span data-ttu-id="502fb-249">Haga clic en Descargar el archivo ZIP.</span><span class="sxs-lookup"><span data-stu-id="502fb-249">Click Download zip file.</span></span> 

1. <span data-ttu-id="502fb-250">Extraiga el archivo ZIP en un directorio local.</span><span class="sxs-lookup"><span data-stu-id="502fb-250">Extract the .zip file to a local directory.</span></span>

1. <span data-ttu-id="502fb-251">Vaya a la carpeta extraída y abra los archivos de origen en su IDE favorito.</span><span class="sxs-lookup"><span data-stu-id="502fb-251">Navigate to the extracted folder and open the source files in your favorite IDE.</span></span>

1. <span data-ttu-id="502fb-252">En el archivo app.js, copie y pegue el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="502fb-252">In the app.js file copy and paste the code below.</span></span>

```javascript
// Copyright (c) Microsoft Corporation. All rights reserved.
// Licensed under the MIT License.

const { BotFrameworkAdapter, MessageFactory, CardFactory } = require('botbuilder');
const restify = require('restify');

// Create server
let server = restify.createServer();
server.listen(process.env.port || process.env.PORT || 3978, function () {
    console.log(`${server.name} listening to ${server.url}`);
});

// Create adapter
const adapter = new BotFrameworkAdapter({
    appId: process.env.MicrosoftAppId,
    appPassword: process.env.MicrosoftAppPassword
});

// Responds to the incoming message by either sending a hero card, an image, 
// or echoing the user's message.

// Listen for incoming requests 
server.post('/api/messages', (req, res) => {
    // Route received request to adapter for processing
    adapter.processActivity(req, res, (context) => {
        if (context.activity.type === 'message') {
            const text = (context.activity.text || '').trim().toLowerCase()

            switch(text){
                case 'hi':
                case "hello":
                case "help":
                    // Send the user an instruction message.
                    return context.sendActivity("Welcome to the Bot to showcase the DirectLine API. " +
                    "Send \"Show me a hero card\" or \"Send me a BotFramework image\" to see how the " +
                    "DirectLine client supports custom channel data. Any other message will be echoed.");
                    break;

                case 'show me a hero card':
                    // Create the hero card.
                    const message = MessageFactory.attachment(
                        CardFactory.heroCard(   
                        'Sample Hero Card', //cards title
                        'Displayed in the DirectLine client' //cards text
                        )
                    );
                    return context.sendActivity(message);
                    break;
                    
                case 'send me a botframework image':
                    // Create the image attachment.
                    const imageOrVideoMessage = MessageFactory.contentUrl('https://docs.microsoft.com/en-us/azure/bot-service/media/how-it-works/architecture-resize.png', 'image/png')
                    return context.sendActivity(imageOrVideoMessage);
                    break;
                
                default:
                    // No command was encountered. Echo the user's message.
                    return context.sendActivity(`You said ${context.activity.text}`);
                    break;
                    
            }
        }
    });
});

```

<span data-ttu-id="502fb-253">Cuando esté listo, puede volver a publicar los orígenes en Azure.</span><span class="sxs-lookup"><span data-stu-id="502fb-253">When you are ready, you can publish the sources back to Azure.</span></span> <span data-ttu-id="502fb-254">Siga estos pasos para obtener información sobre cómo volver a publicar el bot en [Azure](../bot-service-build-download-source-code.md)</span><span class="sxs-lookup"><span data-stu-id="502fb-254">Follow these steps to learn how to publish your bot back to [Azure](../bot-service-build-download-source-code.md)</span></span>

---


## <a name="create-the-console-client-app"></a><span data-ttu-id="502fb-255">Creación de la aplicación cliente de consola</span><span class="sxs-lookup"><span data-stu-id="502fb-255">Create the console client app</span></span>

# <a name="ctabcsclientapp"></a>[<span data-ttu-id="502fb-256">C#</span><span class="sxs-lookup"><span data-stu-id="502fb-256">C#</span></span>](#tab/csclientapp)

<span data-ttu-id="502fb-257">La aplicación cliente de consola opera en dos subprocesos.</span><span class="sxs-lookup"><span data-stu-id="502fb-257">The console client application operates in two threads.</span></span> <span data-ttu-id="502fb-258">El subproceso principal acepta la entrada del usuario y envía mensajes al bot.</span><span class="sxs-lookup"><span data-stu-id="502fb-258">The primary thread accepts user input and sends messages to the bot.</span></span> <span data-ttu-id="502fb-259">El subproceso secundario sondea el bot una vez por segundo para recuperar los mensajes del bot y, a continuación, muestra los mensajes recibidos.</span><span class="sxs-lookup"><span data-stu-id="502fb-259">The secondary thread polls the bot once per second to retrieve any messages from the bot, then displays the messages received.</span></span>

<span data-ttu-id="502fb-260">Para crear un proyecto de consola:</span><span class="sxs-lookup"><span data-stu-id="502fb-260">To create the console project:</span></span>

1. <span data-ttu-id="502fb-261">En el Explorador de soluciones, haga clic con el botón derecho en **Solution 'DirectLineBotSample'** (solución "DirectLineBotSample").</span><span class="sxs-lookup"><span data-stu-id="502fb-261">Right-click the **Solution 'DirectLineBotSample'** in Solution Explorer.</span></span>

1. <span data-ttu-id="502fb-262">Elija **Agregar** > **Nuevo proyecto**.</span><span class="sxs-lookup"><span data-stu-id="502fb-262">Choose **Add** > **New Project**.</span></span>

1. <span data-ttu-id="502fb-263">En **Visual C#** > \**Escritorio clásico de Windows*, elija **Aplicación de consola (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="502fb-263">In **Visual C#** > \**Windows Classic Desktop*, choose **Console App (.NET Framework)**.</span></span>
 
    <span data-ttu-id="502fb-264">**Nota:** No elija **Aplicación de consola (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="502fb-264">**Note:** Do not choose **Console App (.NET Core)**.</span></span>

1. <span data-ttu-id="502fb-265">En el nombre, escriba **DirectLineClientSample**.</span><span class="sxs-lookup"><span data-stu-id="502fb-265">For the name, enter **DirectLineClientSample**.</span></span>

1. <span data-ttu-id="502fb-266">Haga clic en **OK**.</span><span class="sxs-lookup"><span data-stu-id="502fb-266">Click **OK**.</span></span>

### <a name="add-the-nuget-packages-to-the-console-app"></a><span data-ttu-id="502fb-267">Agregue los paquetes NuGet a la aplicación de consola.</span><span class="sxs-lookup"><span data-stu-id="502fb-267">Add the NuGet packages to the console app</span></span>

1. <span data-ttu-id="502fb-268">Haga clic con el botón secundario en **Referencias**.</span><span class="sxs-lookup"><span data-stu-id="502fb-268">Right-click **References**.</span></span>

1. <span data-ttu-id="502fb-269">Haga clic en **Administración de paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="502fb-269">Click **Manage NuGet Packages**.</span></span>

1. <span data-ttu-id="502fb-270">Haga clic en **Examinar**.</span><span class="sxs-lookup"><span data-stu-id="502fb-270">Click **Browse**.</span></span> <span data-ttu-id="502fb-271">Asegúrese de que la casilla **Incluir versión preliminar** esté activada.</span><span class="sxs-lookup"><span data-stu-id="502fb-271">Make sure the **Include prerelease** check box is checked.</span></span>

1. <span data-ttu-id="502fb-272">Busque e instale los siguientes paquetes NuGet:</span><span class="sxs-lookup"><span data-stu-id="502fb-272">Search for and install the following NuGet packages:</span></span>
    - <span data-ttu-id="502fb-273">Microsoft.Bot.Connector.DirectLine (v3.0.2)</span><span class="sxs-lookup"><span data-stu-id="502fb-273">Microsoft.Bot.Connector.DirectLine (v3.0.2)</span></span>
    - <span data-ttu-id="502fb-274">Newtonsoft.Json</span><span class="sxs-lookup"><span data-stu-id="502fb-274">Newtonsoft.Json</span></span>

### <a name="edit-the-programcs-file"></a><span data-ttu-id="502fb-275">Edición del archivo Program.cs</span><span class="sxs-lookup"><span data-stu-id="502fb-275">Edit the Program.cs file</span></span>

<span data-ttu-id="502fb-276">Reemplace el contenido del archivo **Program.cs** de DirectLineClientSample por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="502fb-276">Replace the contents of the DirectLineClientSample **Program.cs** file with the following:</span></span>

```csharp
using System;
using System.Diagnostics;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.Bot.Connector.DirectLine;
using Newtonsoft.Json;

namespace DirectLineClientSample
{
    class Program
    {
        // ************
        // Replace the following values with your Direct Line secret and the name of your bot resource ID.
        //*************
        private static string directLineSecret = "*** Replace with Direct Line secret ***";
        private static string botId = "*** Replace with the resource name of your bot ***";

        // This gives a name to the bot user.
        private static string fromUser = "DirectLineClientSampleUser";

        static void Main(string[] args)
        {
            StartBotConversation().Wait();
        }


        /// <summary>
        /// Drives the user's conversation with the bot.
        /// </summary>
        /// <returns></returns>
        private static async Task StartBotConversation()
        {
            // Create a new Direct Line client.
            DirectLineClient client = new DirectLineClient(directLineSecret);

            // Start the conversation.
            var conversation = await client.Conversations.StartConversationAsync();

            // Start the bot message reader in a separate thread.
            new System.Threading.Thread(async () => await ReadBotMessagesAsync(client, conversation.ConversationId)).Start();

            // Prompt the user to start talking to the bot.
            Console.Write("Type your message (or \"exit\" to end): ");

            // Loop until the user chooses to exit this loop.
            while (true)
            {
                // Accept the input from the user.
                string input = Console.ReadLine().Trim();

                // Check to see if the user wants to exit.
                if (input.ToLower() == "exit")
                {
                    // Exit the app if the user requests it.
                    break;
                }
                else
                {
                    if (input.Length > 0)
                    {
                        // Create a message activity with the text the user entered.
                        Activity userMessage = new Activity
                        {
                            From = new ChannelAccount(fromUser),
                            Text = input,
                            Type = ActivityTypes.Message
                        };

                        // Send the message activity to the bot.
                        await client.Conversations.PostActivityAsync(conversation.ConversationId, userMessage);
                    }
                }
            }
        }


        /// <summary>
        /// Polls the bot continuously and retrieves messages sent by the bot to the client.
        /// </summary>
        /// <param name="client">The Direct Line client.</param>
        /// <param name="conversationId">The conversation ID.</param>
        /// <returns></returns>
        private static async Task ReadBotMessagesAsync(DirectLineClient client, string conversationId)
        {
            string watermark = null;

            // Poll the bot for replies once per second.
            while (true)
            {
                // Retrieve the activity set from the bot.
                var activitySet = await client.Conversations.GetActivitiesAsync(conversationId, watermark);
                watermark = activitySet?.Watermark;

                // Extract the activies sent from our bot.
                var activities = from x in activitySet.Activities
                                 where x.From.Id == botId
                                 select x;

                // Analyze each activity in the activity set.
                foreach (Activity activity in activities)
                {
                    // Display the text of the activity.
                    Console.WriteLine(activity.Text);

                    // Are there any attachments?
                    if (activity.Attachments != null)
                    {
                        // Extract each attachment from the activity.
                        foreach (Attachment attachment in activity.Attachments)
                        {
                            switch (attachment.ContentType)
                            {
                                // Display a hero card.
                                case "application/vnd.microsoft.card.hero":
                                    RenderHeroCard(attachment);
                                    break;

                                // Display the image in a browser.
                                case "image/png":
                                    Console.WriteLine($"Opening the requested image '{attachment.ContentUrl}'");
                                    Process.Start(attachment.ContentUrl);
                                    break;
                            }
                        }
                    }

                    // Redisplay the user prompt.
                    Console.Write("\nType your message (\"exit\" to end): ");
                }

                // Wait for one second before polling the bot again.
                await Task.Delay(TimeSpan.FromSeconds(1)).ConfigureAwait(false);
            }
        }


        /// <summary>
        /// Displays the hero card on the console.
        /// </summary>
        /// <param name="attachment">The attachment that contains the hero card.</param>
        private static void RenderHeroCard(Attachment attachment)
        {
            const int Width = 70;
            // Function to center a string between asterisks.
            Func<string, string> contentLine = (content) => string.Format($"{{0, -{Width}}}", string.Format("{0," + ((Width + content.Length) / 2).ToString() + "}", content));

            // Extract the hero card data.
            var heroCard = JsonConvert.DeserializeObject<HeroCard>(attachment.Content.ToString());

            // Display the hero card.
            if (heroCard != null)
            {
                Console.WriteLine("/{0}", new string('*', Width + 1));
                Console.WriteLine("*{0}*", contentLine(heroCard.Title));
                Console.WriteLine("*{0}*", new string(' ', Width));
                Console.WriteLine("*{0}*", contentLine(heroCard.Text));
                Console.WriteLine("{0}/", new string('*', Width + 1));
            }
        }
    }
}
```

<span data-ttu-id="502fb-277">Para comprobar que todo está en orden, presione **F6** para compilar el proyecto.</span><span class="sxs-lookup"><span data-stu-id="502fb-277">To verify things are in order, press **F6** to build the project.</span></span> <span data-ttu-id="502fb-278">No debería haber advertencias ni errores.</span><span class="sxs-lookup"><span data-stu-id="502fb-278">There should be no warnings or errors.</span></span>

# <a name="javascripttabjsclientapp"></a>[<span data-ttu-id="502fb-279">JavaScript</span><span class="sxs-lookup"><span data-stu-id="502fb-279">JavaScript</span></span>](#tab/jsclientapp)

### <a name="create-a-direct-line-client"></a><span data-ttu-id="502fb-280">Creación de un cliente de Direct Line</span><span class="sxs-lookup"><span data-stu-id="502fb-280">Create a Direct Line Client</span></span> 

<span data-ttu-id="502fb-281">Ahora que ha implementado el bot de aplicación web, podemos crear un cliente de Direct Line.</span><span class="sxs-lookup"><span data-stu-id="502fb-281">Now that you have deployed your web app bot we can create a Direct Line Client.</span></span>

```
    md DirectLineClient
    cd DirectLineClient
    npm init
```

<span data-ttu-id="502fb-282">A continuación, instale estos paquetes de npm.</span><span class="sxs-lookup"><span data-stu-id="502fb-282">Next, install these packages from npm</span></span>

```
    npm install --save open
    npm install --save request
    npm install --save request-promise
    npm install --save swagger-client
```

<span data-ttu-id="502fb-283">Cree un archivo **app.js**.</span><span class="sxs-lookup"><span data-stu-id="502fb-283">Create an **app.js** file.</span></span> <span data-ttu-id="502fb-284">Copie y pegue el código siguiente en este archivo.</span><span class="sxs-lookup"><span data-stu-id="502fb-284">Copy and paste the code below into this file.</span></span>

<span data-ttu-id="502fb-285">Se obtendrá directLineSecret en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="502fb-285">We will get the directLineSecret in the next step.</span></span>
```javascript
var Swagger = require('swagger-client');
var open = require('open');
var rp = require('request-promise');

// config items
var pollInterval = 1000;
// Change the Direct Line Secret to your own 
var directLineSecret = 'your secret here';
var directLineClientName = 'DirectLineClient';
var directLineSpecUrl = 'https://docs.botframework.com/en-us/restapi/directline3/swagger.json';

var directLineClient = rp(directLineSpecUrl)
    .then(function (spec) {
        // client
        return new Swagger({
            spec: JSON.parse(spec.trim()),
            usePromise: true
        });
    })
    .then(function (client) {
        // add authorization header to client
        client.clientAuthorizations.add('AuthorizationBotConnector', new Swagger.ApiKeyAuthorization('Authorization', 'Bearer ' + directLineSecret, 'header'));
        return client;
    })
    .catch(function (err) {
        console.error('Error initializing DirectLine client', err);
    });

// once the client is ready, create a new conversation
directLineClient.then(function (client) {
    client.Conversations.Conversations_StartConversation()                          // create conversation
        .then(function (response) {
            return response.obj.conversationId;
        })                            // obtain id
        .then(function (conversationId) {
            sendMessagesFromConsole(client, conversationId);                        // start watching console input for sending new messages to bot
            pollMessages(client, conversationId);                                   // start polling messages from bot
        })
        .catch(function (err) {
            console.error('Error starting conversation', err);
        });
});

// Read from console (stdin) and send input to conversation using DirectLine client
function sendMessagesFromConsole(client, conversationId) {
    var stdin = process.openStdin();
    process.stdout.write('Command> ');
    stdin.addListener('data', function (e) {
        var input = e.toString().trim();
        if (input) {
            // exit
            if (input.toLowerCase() === 'exit') {
                return process.exit();
            }

            // send message
            client.Conversations.Conversations_PostActivity(
                {
                    conversationId: conversationId,
                    activity: {
                        textFormat: 'plain',
                        text: input,
                        type: 'message',
                        from: {
                            id: directLineClientName,
                            name: directLineClientName
                        }
                    }
                }).catch(function (err) {
                    console.error('Error sending message:', err);
                });

            process.stdout.write('Command> ');
        }
    });
}

// Poll Messages from conversation using DirectLine client
function pollMessages(client, conversationId) {
    console.log('Starting polling message for conversationId: ' + conversationId);
    var watermark = null;
    setInterval(function () {
        client.Conversations.Conversations_GetActivities({ conversationId: conversationId, watermark: watermark })
            .then(function (response) {
                watermark = response.obj.watermark;                                 // use watermark so subsequent requests skip old messages
                return response.obj.activities;
            })
            .then(printMessages);
    }, pollInterval);
}

// Helpers methods
function printMessages(activities) {
    if (activities && activities.length) {
        // ignore own messages
        activities = activities.filter(function (m) { return m.from.id !== directLineClientName });

        if (activities.length) {
            process.stdout.clearLine();
            process.stdout.cursorTo(0);

            // print other messages
            activities.forEach(printMessage);

            process.stdout.write('Command> ');
        }
    }
}

function printMessage(activity) {
    if (activity.text) {
        console.log(activity.text);
    }

    if (activity.attachments) {
        activity.attachments.forEach(function (attachment) {
            switch (attachment.contentType) {
                case "application/vnd.microsoft.card.hero":
                    renderHeroCard(attachment);
                    break;

                case "image/png":
                    console.log('Opening the requested image ' + attachment.contentUrl);
                    open(attachment.contentUrl);
                    break;
            }
        });
    }
}

function renderHeroCard(attachment) {
    var width = 70;
    var contentLine = function (content) {
        return ' '.repeat((width - content.length) / 2) +
            content +
            ' '.repeat((width - content.length) / 2);
    }

    console.log('/' + '*'.repeat(width + 1));
    console.log('*' + contentLine(attachment.content.title) + '*');
    console.log('*' + ' '.repeat(width) + '*');
    console.log('*' + contentLine(attachment.content.text) + '*');
    console.log('*'.repeat(width + 1) + '/');
}
```

---

## <a name="configure-the-direct-line-channel"></a><span data-ttu-id="502fb-286">Configuración del canal de Direct Line</span><span class="sxs-lookup"><span data-stu-id="502fb-286">Configure the Direct Line channel</span></span>

<span data-ttu-id="502fb-287">La adición del canal de Direct Line transforma este bot en un bot de Direct Line.</span><span class="sxs-lookup"><span data-stu-id="502fb-287">Adding the Direct Line channel makes this bot into a Direct Line bot.</span></span>

<span data-ttu-id="502fb-288">Para configurar el canal de Direct Line:</span><span class="sxs-lookup"><span data-stu-id="502fb-288">To configure the Direct Line channel:</span></span>

![Hoja Conexión a los canales con el botón de canal de Direct Line resaltado.](media/bot-builder-howto-direct-line/direct-line-channel-button.png)

1. <span data-ttu-id="502fb-290">En la hoja **Bot Channels Registration** (Registro de canales de bot), haga clic en **Canales**.</span><span class="sxs-lookup"><span data-stu-id="502fb-290">On the **Bot Channels Registration** blade, click **Channels**.</span></span> <span data-ttu-id="502fb-291">Aparecerá la hoja **Conexión a los canales**.</span><span class="sxs-lookup"><span data-stu-id="502fb-291">The **Connect to Channels** blade will appear.</span></span>

    ![Hoja Configure Direct Line (Configuración de Direct Line) que muestra ambas claves secretas.](media/bot-builder-howto-direct-line/configure-direct-line.png)

1. <span data-ttu-id="502fb-293">En la hoja **Conexión a los canales**, haga clic en el botón **Configure Direct Line channel** (Configurar canal de Direct Line).</span><span class="sxs-lookup"><span data-stu-id="502fb-293">On the **Connect to Channels** blade, click the **Configure Direct Line channel** button.</span></span> 

1. <span data-ttu-id="502fb-294">Si la opción **3.0** no está activada, coloque una marca de verificación en la casilla de verificación.</span><span class="sxs-lookup"><span data-stu-id="502fb-294">If **3.0** is not checked, put a check mark in the check box.</span></span>

1. <span data-ttu-id="502fb-295">Haga clic en **Mostrar** para una clave secreta por lo menos.</span><span class="sxs-lookup"><span data-stu-id="502fb-295">Click **Show** for at least one secret key.</span></span>

1. <span data-ttu-id="502fb-296">Copie una de las claves secretas y péguela en la aplicación cliente, en la cadena **directLineSecret**.</span><span class="sxs-lookup"><span data-stu-id="502fb-296">Copy one of the secret keys and paste it into your client app, into the **directLineSecret** string.</span></span>

1. <span data-ttu-id="502fb-297">Haga clic en Done (Listo).</span><span class="sxs-lookup"><span data-stu-id="502fb-297">Click Done.</span></span>

## <a name="run-the-client-app"></a><span data-ttu-id="502fb-298">Ejecución de la aplicación cliente</span><span class="sxs-lookup"><span data-stu-id="502fb-298">Run the client app</span></span>

# <a name="ctabcsrunclient"></a>[<span data-ttu-id="502fb-299">C#</span><span class="sxs-lookup"><span data-stu-id="502fb-299">C#</span></span>](#tab/csrunclient)

<span data-ttu-id="502fb-300">El bot ya está listo para comunicarse con la aplicación cliente de consola de Direct Line.</span><span class="sxs-lookup"><span data-stu-id="502fb-300">Your bot is now ready to communicate with the Direct Line console client application.</span></span> <span data-ttu-id="502fb-301">Para ejecutar la aplicación de consola, haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="502fb-301">To run the console app, do the following:</span></span>

1. <span data-ttu-id="502fb-302">En Visual Studio, haga clic con el botón derecho en el proyecto **DirectLineClientSample** y seleccione **Set as StartUp Project**. (Establecer como proyecto de inicio).</span><span class="sxs-lookup"><span data-stu-id="502fb-302">In Visual Studio, right click **DirectLineClientSample** project and select **Set as StartUp Project**.</span></span>

1. <span data-ttu-id="502fb-303">Examine el archivo **Program.cs** en el proyecto **DirectLineClientSample**.</span><span class="sxs-lookup"><span data-stu-id="502fb-303">Examine the **Program.cs** file in the **DirectLineClientSample** project.</span></span>

    - <span data-ttu-id="502fb-304">Compruebe que el código secreto de Direct Line está en la cadena **directLineSecret**.</span><span class="sxs-lookup"><span data-stu-id="502fb-304">Verify that the one Direct Line secret code is in the **directLineSecret** string.</span></span>

1. <span data-ttu-id="502fb-305">Si **directLineSecret** no es correcto, vaya a Azure Portal, haga clic en el bot de Direct Line, en **Canales**, luego en **Configure Direct Line channel** (Configurar canal de Direct Line) (o **Editar**), mostrar una clave y luego cópiela en la cadena **directLineSecret**.</span><span class="sxs-lookup"><span data-stu-id="502fb-305">If the **directLineSecret** isn't correct, go to the Azure portal, click your Direct Line bot, click **Channels**, click **Configure Direct Line channel** (or **Edit**), show a key, then copy that key to the **directLineSecret** string.</span></span>

    - <span data-ttu-id="502fb-306">Compruebe que el grupo de recursos está en la cadena **botId**.</span><span class="sxs-lookup"><span data-stu-id="502fb-306">Verify that the resource group is in the **botId** string.</span></span>

1. <span data-ttu-id="502fb-307">Si no lo está, desde la hoja **Introducción**, copie y pegue el **grupo de recursos** en la cadena **botId**.</span><span class="sxs-lookup"><span data-stu-id="502fb-307">If it isn't, from the **Overview** blade, copy-and-paste the **Resource group** into the **botId** string.</span></span>

1. <span data-ttu-id="502fb-308">Pulse F5 para iniciar la depuración.</span><span class="sxs-lookup"><span data-stu-id="502fb-308">Press F5 or Start debugging.</span></span>

<span data-ttu-id="502fb-309">Se iniciará la aplicación cliente de consola.</span><span class="sxs-lookup"><span data-stu-id="502fb-309">The console client app will start.</span></span> <span data-ttu-id="502fb-310">Para probar la aplicación:</span><span class="sxs-lookup"><span data-stu-id="502fb-310">To test out the app:</span></span> 

1. <span data-ttu-id="502fb-311">Escriba "hola".</span><span class="sxs-lookup"><span data-stu-id="502fb-311">Enter "Hi".</span></span> <span data-ttu-id="502fb-312">El bot debería mostrar "Ha dicho 'Hola'".</span><span class="sxs-lookup"><span data-stu-id="502fb-312">The bot should display 'You said "Hi"'.</span></span>

1. <span data-ttu-id="502fb-313">Escriba "Mostrarme una tarjeta de imagen prominente".</span><span class="sxs-lookup"><span data-stu-id="502fb-313">Enter "show me a hero card".</span></span> <span data-ttu-id="502fb-314">El bot debería mostrar una tarjeta de imagen prominente.</span><span class="sxs-lookup"><span data-stu-id="502fb-314">The bot should display a hero card.</span></span>

1. <span data-ttu-id="502fb-315">Escriba "Enviarme una imagen de botframework".</span><span class="sxs-lookup"><span data-stu-id="502fb-315">Enter "send me a botframework image".</span></span> <span data-ttu-id="502fb-316">El bot iniciará un explorador para mostrar una imagen de la documentación de Bot Framework.</span><span class="sxs-lookup"><span data-stu-id="502fb-316">The bot will launch a browser to display an image from the Bot Framework documentation.</span></span>

1. <span data-ttu-id="502fb-317">Escriba cualquier otra cosa y el bot debería responder con "Ha dicho" y el mensaje entre comillas.</span><span class="sxs-lookup"><span data-stu-id="502fb-317">Enter anything else and the bot should reply with, "You said" and your message in quotes.</span></span>

# <a name="javascripttabjsrunclient"></a>[<span data-ttu-id="502fb-318">JavaScript</span><span class="sxs-lookup"><span data-stu-id="502fb-318">JavaScript</span></span>](#tab/jsrunclient)

<span data-ttu-id="502fb-319">Para ejecutar el ejemplo, deberá ejecutar la aplicación DirectLineClient.</span><span class="sxs-lookup"><span data-stu-id="502fb-319">To run the sample you will need to run your DirectLineClient app.</span></span>

1. <span data-ttu-id="502fb-320">Abra una consola CMD y CD en el directorio DirectLineClient.</span><span class="sxs-lookup"><span data-stu-id="502fb-320">Open a CMD console and CD to the DirectLineClient directory</span></span>

1. <span data-ttu-id="502fb-321">Ejecute `node app.js`</span><span class="sxs-lookup"><span data-stu-id="502fb-321">Run `node app.js`</span></span>

<span data-ttu-id="502fb-322">Para probar los tipos de mensajes personalizados en la consola del cliente, muéstreme una tarjeta de imagen prominente o envíeme una imagen de botframework y verá el resultado siguiente.</span><span class="sxs-lookup"><span data-stu-id="502fb-322">To test the custom messages type in the Client's console show me a hero card or send me a botframework image and you should see the following outcome.</span></span>

![resultado](media/bot-builder-howto-direct-line/outcome.png)

---


### <a name="next-steps"></a><span data-ttu-id="502fb-324">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="502fb-324">Next steps</span></span>
