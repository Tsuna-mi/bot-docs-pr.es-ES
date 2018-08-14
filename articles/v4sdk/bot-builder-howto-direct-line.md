---
title: Cómo crear un cliente y un bot de Direct Line | Microsoft Docs
description: Obtenga información sobre cómo crear un cliente y un bot de Direct Line con V4 del SDK de Bot Builder para .NET.
keywords: bot de direct line, cliente de direct line, canal personalizado, basado en consola, publicar
author: v-royhar
ms.author: v-royhar
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 4/16/18
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: ac96e35763d690c91e6584ff9a840b490e0a32cd
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39304793"
---
# <a name="how-to-create-a-direct-line-bot-and-client"></a><span data-ttu-id="35a05-104">Cómo crear un cliente y un bot de Direct Line</span><span class="sxs-lookup"><span data-stu-id="35a05-104">How to create a Direct Line bot and client</span></span>

<span data-ttu-id="35a05-105">Los bots de Direct Line de Microsoft Bot Framework son bots que pueden funcionar con un cliente personalizado de su propio diseño.</span><span class="sxs-lookup"><span data-stu-id="35a05-105">Microsoft Bot Framework Direct Line bots are bots that can function with a custom client of your own design.</span></span> <span data-ttu-id="35a05-106">Los bots de Direct Line son increíblemente parecidos a los bots normales.</span><span class="sxs-lookup"><span data-stu-id="35a05-106">Direct Line bots are remarkably similar to normal bots.</span></span> <span data-ttu-id="35a05-107">Simplemente no necesitan utilizar los canales proporcionados.</span><span class="sxs-lookup"><span data-stu-id="35a05-107">They just don't need to use the provided channels.</span></span>

<span data-ttu-id="35a05-108">Los clientes de Direct Line se pueden escribir para que sean lo que quiera.</span><span class="sxs-lookup"><span data-stu-id="35a05-108">Direct Line clients can be written to be whatever you want them to be.</span></span> <span data-ttu-id="35a05-109">Puede escribir un cliente de Android, un cliente de iOS o incluso una aplicación cliente basada en consola.</span><span class="sxs-lookup"><span data-stu-id="35a05-109">You can write an Android client, an iOS client, or even a console-based client application.</span></span>

<span data-ttu-id="35a05-110">En este tema obtendrá información sobre cómo crear e implementar un bot de Direct Line y cómo crear y ejecutar una aplicación cliente de Direct Line basada en consola.</span><span class="sxs-lookup"><span data-stu-id="35a05-110">In this topic you will learn how to create and deploy a Direct Line bot, and how to create and run a console-based Direct Line client app.</span></span>

## <a name="create-your-bot"></a><span data-ttu-id="35a05-111">Creación del bot</span><span class="sxs-lookup"><span data-stu-id="35a05-111">Create your bot</span></span>

<span data-ttu-id="35a05-112">Cada lenguaje sigue una ruta de acceso diferente para crear el bot.</span><span class="sxs-lookup"><span data-stu-id="35a05-112">Each language follows a different path to create the bot.</span></span> <span data-ttu-id="35a05-113">JavaScript crea el bot en Azure y luego modifica el código, mientras que C# crea el bot localmente y luego lo publica en Azure, pero ambos son métodos válidos y se pueden usar con cualquiera de estos lenguajes.</span><span class="sxs-lookup"><span data-stu-id="35a05-113">Javascript creates the bot in Azure, then modifies the code while C# creates the bot locally, then publishes it to Azure, but both are valid methods and can be used with either language.</span></span> <span data-ttu-id="35a05-114">Si quiere obtener más información sobre cómo publicar el bot en Azure, eche un vistazo a [deploying your bot in Azure](../bot-builder-howto-deploy-azure.md) (Implementación de un bot en Azure).</span><span class="sxs-lookup"><span data-stu-id="35a05-114">If you want details on publishing your bot to Azure, take a look at [deploying your bot in Azure](../bot-builder-howto-deploy-azure.md).</span></span>

# <a name="ctabcscreatebot"></a>[<span data-ttu-id="35a05-115">C#</span><span class="sxs-lookup"><span data-stu-id="35a05-115">C#</span></span>](#tab/cscreatebot)

### <a name="create-the-solution-in-visual-studio"></a><span data-ttu-id="35a05-116">Creación de la solución en Visual Studio</span><span class="sxs-lookup"><span data-stu-id="35a05-116">Create the solution in Visual Studio</span></span>

<span data-ttu-id="35a05-117">Para crear la solución para el bot de Direct Line, en Visual Studio 2015 o una versión posterior:</span><span class="sxs-lookup"><span data-stu-id="35a05-117">To create the solution for the Direct Line bot, in Visual Studio 2015 or later:</span></span>

1. <span data-ttu-id="35a05-118">Cree una nueva **aplicación web ASP.NET Core** en **Visual C#** > **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="35a05-118">Create a new **ASP.NET Core Web Application**, in **Visual C#** > **.NET Core**.</span></span>

1. <span data-ttu-id="35a05-119">Escriba **DirectLineBotSample** como nombre y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="35a05-119">For the name, enter **DirectLineBotSample**, and click **OK**.</span></span>

1. <span data-ttu-id="35a05-120">Asegúrese de que **.NET Core** y **ASP.NET Core 2.0** estén seleccionados, elija la plantilla de proyecto **Vacía** y, después, haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="35a05-120">Make sure **.NET Core** and **ASP.NET Core 2.0** are selected, choose the **Empty** project template, then click **OK**.</span></span>

#### <a name="add-dependencies"></a><span data-ttu-id="35a05-121">Adición de dependencias</span><span class="sxs-lookup"><span data-stu-id="35a05-121">Add dependencies</span></span>

1. <span data-ttu-id="35a05-122">En el **Explorador de soluciones**, haga clic con el botón derecho en **Dependencias** y, después, en **Administrar paquetes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="35a05-122">In **Solution Explorer**, right click **Dependencies**, then choose **Manage NuGet Packages**.</span></span>

1. <span data-ttu-id="35a05-123">Haga clic en **Examinar** y, a continuación, asegúrese de que la casilla **Incluir versión preliminar** esté activada.</span><span class="sxs-lookup"><span data-stu-id="35a05-123">Click **Browse**, then make sure the **Include prerelease** check box is checked.</span></span>

1. <span data-ttu-id="35a05-124">Busque e instale los siguientes paquetes NuGet:</span><span class="sxs-lookup"><span data-stu-id="35a05-124">Search for and install the following NuGet packages:</span></span>
    - <span data-ttu-id="35a05-125">Microsoft.Bot.Builder</span><span class="sxs-lookup"><span data-stu-id="35a05-125">Microsoft.Bot.Builder</span></span>
    - <span data-ttu-id="35a05-126">Microsoft.Bot.Builder.Core.Extensions</span><span class="sxs-lookup"><span data-stu-id="35a05-126">Microsoft.Bot.Builder.Core.Extensions</span></span>
    - <span data-ttu-id="35a05-127">Microsoft.Bot.Builder.Integration.AspNet.Core</span><span class="sxs-lookup"><span data-stu-id="35a05-127">Microsoft.Bot.Builder.Integration.AspNet.Core</span></span>
    - <span data-ttu-id="35a05-128">Newtonsoft.Json</span><span class="sxs-lookup"><span data-stu-id="35a05-128">Newtonsoft.Json</span></span>

### <a name="create-the-appsettingsjson-file"></a><span data-ttu-id="35a05-129">Creación del archivo appsettings.json</span><span class="sxs-lookup"><span data-stu-id="35a05-129">Create the appsettings.json file</span></span>

<span data-ttu-id="35a05-130">El archivo appsettings.json contendrá el id. de la aplicación de Microsoft, la contraseña de la aplicación y la cadena de conexión de datos.</span><span class="sxs-lookup"><span data-stu-id="35a05-130">The appsettings.json file will contain the Microsoft App ID, App Password, and Data Connection string.</span></span> <span data-ttu-id="35a05-131">Como este bot no almacenará la información de estado, la cadena de conexión de datos permanecerá vacía.</span><span class="sxs-lookup"><span data-stu-id="35a05-131">Since this bot will not store state information, the Data Connection string remain empty.</span></span> <span data-ttu-id="35a05-132">Y, si solo usa Bot Framework Emulator, todas ellas pueden estar vacías.</span><span class="sxs-lookup"><span data-stu-id="35a05-132">And if you're only using the Bot Framework Emulator, all of them can remain empty.</span></span>

<span data-ttu-id="35a05-133">Para crear un archivo **appsettings.json**:</span><span class="sxs-lookup"><span data-stu-id="35a05-133">To create an **appsettings.json** file:</span></span>

1. <span data-ttu-id="35a05-134">Haga clic con el botón derecho en el proyecto **DirectLineBotSample** y elija **Agregar** > **Nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="35a05-134">Right-click the **DirectLineBotSample** Project, choose **Add** > **New Item**.</span></span>

1. <span data-ttu-id="35a05-135">En **ASP.NET Core**, haga clic en **Archivo de configuración de ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="35a05-135">In **ASP.NET Core**, click **ASP.NET Configuration File**.</span></span>

1. <span data-ttu-id="35a05-136">Escriba **appsettings.json** en el nombre y haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="35a05-136">Type **appsettings.json** for the name, and click **Add**.</span></span>

<span data-ttu-id="35a05-137">Reemplace el contenido del archivo appsettings.json por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="35a05-137">Replace the contents of the appsettings.json file with the following:</span></span>

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

### <a name="edit-startupcs"></a><span data-ttu-id="35a05-138">Edición de Startup.cs</span><span class="sxs-lookup"><span data-stu-id="35a05-138">Edit Startup.cs</span></span>

<span data-ttu-id="35a05-139">Reemplace el contenido del archivo Startup.cs por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="35a05-139">Replace the contents of the Startup.cs file with the following:</span></span>

<span data-ttu-id="35a05-140">**Nota**: **DirectBot** se definirá en el paso siguiente, por lo que puede pasar por alto el subrayado rojo.</span><span class="sxs-lookup"><span data-stu-id="35a05-140">**Note**: **DirectBot** will be defined in the next step, so you can ignore the red underline on it.</span></span>

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

### <a name="create-the-directbot-class"></a><span data-ttu-id="35a05-141">Creación de la clase DirectBot</span><span class="sxs-lookup"><span data-stu-id="35a05-141">Create the DirectBot class</span></span>

<span data-ttu-id="35a05-142">La clase DirectBot contiene la mayor parte de la lógica para este bot.</span><span class="sxs-lookup"><span data-stu-id="35a05-142">The DirectBot class contains most of the logic for this bot.</span></span>

1. <span data-ttu-id="35a05-143">Haga clic en el **DirectLineBotSample** > **Agregar** > **Nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="35a05-143">Right-click the **DirectLineBotSample** > **Add** > **New Item**.</span></span>

1. <span data-ttu-id="35a05-144">En **ASP.NET Core**, haga clic en **Clase**.</span><span class="sxs-lookup"><span data-stu-id="35a05-144">In **ASP.NET Core**, click **Class**.</span></span>

1. <span data-ttu-id="35a05-145">Escriba **DirectBot.cs** en el nombre y haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="35a05-145">Type **DirectBot.cs** for the name, and click **Add**.</span></span>

<span data-ttu-id="35a05-146">Reemplace el contenido del archivo DirectBot.cs por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="35a05-146">Replace the contents of the DirectBot.cs file with the following:</span></span>

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

                case ActivityTypes.Ping:
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

<span data-ttu-id="35a05-147">**Nota:** No es necesario cambiar el archivo Program.cs.</span><span class="sxs-lookup"><span data-stu-id="35a05-147">**Note:** The exiting Program.cs file does not need to be changed.</span></span>

<span data-ttu-id="35a05-148">Para comprobar que todo está en orden, presione **F6** para compilar el proyecto.</span><span class="sxs-lookup"><span data-stu-id="35a05-148">To verify things are in order, press **F6** to build the project.</span></span> <span data-ttu-id="35a05-149">No debería haber advertencias ni errores.</span><span class="sxs-lookup"><span data-stu-id="35a05-149">There should be no warnings or errors.</span></span>

### <a name="publish-the-bot-from-visual-studio"></a><span data-ttu-id="35a05-150">Publicación del bot desde Visual Studio</span><span class="sxs-lookup"><span data-stu-id="35a05-150">Publish the bot from Visual Studio</span></span>

1. <span data-ttu-id="35a05-151">En el Explorador de soluciones de Visual Studio, haga clic con el botón derecho en el proyecto **DirectLineBotSample** y luego en **Establecer como proyecto de inicio**.</span><span class="sxs-lookup"><span data-stu-id="35a05-151">In Visual Studio, on the Solutions Explorer, right click on the **DirectLineBotSample** project and click **Set as StartUp Project**.</span></span>

    ![Página de publicación de Visual Studio](media/bot-builder-howto-direct-line/visual-studio-publish-page.png)

1. <span data-ttu-id="35a05-153">Vuelva a hacer clic con el botón derecho en **DirectLineBotSample** y luego en **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="35a05-153">Right click **DirectLineBotSample** again and click **Publish**.</span></span> <span data-ttu-id="35a05-154">Se muestra la página **Publicar** de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="35a05-154">The Visual Studio **Publish** page appears.</span></span>

1. <span data-ttu-id="35a05-155">Si ya tiene un perfil de publicación para este bot, selecciónelo y haga clic en el botón **Publicar** y, después, vaya a la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="35a05-155">If you already have a publishing profile for this bot, select it and click the **Publish** button, then go on to the next section.</span></span>

1. <span data-ttu-id="35a05-156">Para crear un nuevo perfil de publicación, seleccione el icono **Microsoft Azure App Service**.</span><span class="sxs-lookup"><span data-stu-id="35a05-156">To create a new publishing profile, select the **Microsoft Azure App Service** icon.</span></span>

1. <span data-ttu-id="35a05-157">Seleccione **Crear nuevo**.</span><span class="sxs-lookup"><span data-stu-id="35a05-157">Select **Create new**.</span></span>

1. <span data-ttu-id="35a05-158">Haga clic en el botón **Publicar** .</span><span class="sxs-lookup"><span data-stu-id="35a05-158">Click the **Publish** button.</span></span> <span data-ttu-id="35a05-159">Se muestra el cuadro de diálogo **Create App Service** (Crear una instancia de App Service).</span><span class="sxs-lookup"><span data-stu-id="35a05-159">The **Create App Service** dialog appears.</span></span>

    ![Cuadro de diálogo Create App Service (Crear una instancia de App Service)](media/bot-builder-howto-direct-line/create-app-service-dialog.png)

    - <span data-ttu-id="35a05-161">En Nombre de la aplicación, asígnele un nombre que pueda encontrar más adelante.</span><span class="sxs-lookup"><span data-stu-id="35a05-161">For App Name, give it a name you can find later.</span></span> <span data-ttu-id="35a05-162">Por ejemplo, puede agregar su nombre de correo electrónico al principio del nombre de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="35a05-162">For example, you can add your email name to the beginning of the App Name.</span></span>

    - <span data-ttu-id="35a05-163">Compruebe si está usando la suscripción correcta.</span><span class="sxs-lookup"><span data-stu-id="35a05-163">Verify you are using the correct subscription.</span></span>

    - <span data-ttu-id="35a05-164">En Grupo de recursos, compruebe que usa el grupo de recursos correcto.</span><span class="sxs-lookup"><span data-stu-id="35a05-164">For Resource Group, verify you are using the correct resource group.</span></span> <span data-ttu-id="35a05-165">El grupo de recursos puede encontrarse en la hoja **Introducción** del bot.</span><span class="sxs-lookup"><span data-stu-id="35a05-165">The resource group can be found on the **Overview** blade of your bot.</span></span> <span data-ttu-id="35a05-166">**Nota:** Un grupo de recursos incorrecto es difícil de corregir.</span><span class="sxs-lookup"><span data-stu-id="35a05-166">**Note:** An incorrect resource group is difficult to correct.</span></span>

    - <span data-ttu-id="35a05-167">Compruebe si está usando el plan correcto de App Service.</span><span class="sxs-lookup"><span data-stu-id="35a05-167">Verify you are using the correct App Service Plan.</span></span>
    
1. <span data-ttu-id="35a05-168">Haga clic en el botón **Crear**.</span><span class="sxs-lookup"><span data-stu-id="35a05-168">Click the **Create** button.</span></span> <span data-ttu-id="35a05-169">Visual Studio inicia la implementación del bot.</span><span class="sxs-lookup"><span data-stu-id="35a05-169">Visual Studio will begin deploying your bot.</span></span>

<span data-ttu-id="35a05-170">Una vez publicado el bot, aparecerá un explorador con el punto de conexión de dirección URL del bot.</span><span class="sxs-lookup"><span data-stu-id="35a05-170">After your bot is published, a browser will appear with the URL endpoint of your bot.</span></span>

### <a name="create-bot-channels-registration-bot-on-microsoft-azure"></a><span data-ttu-id="35a05-171">Creación de un bot de registro de canales de bot en Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="35a05-171">Create Bot Channels Registration bot on Microsoft Azure</span></span>

<span data-ttu-id="35a05-172">El bot de Direct Line puede hospedarse en cualquier plataforma.</span><span class="sxs-lookup"><span data-stu-id="35a05-172">The Direct Line bot can be hosted on any platform.</span></span> <span data-ttu-id="35a05-173">En este ejemplo, el bot se hospedará en Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="35a05-173">In this example, the bot will be hosted on Microsoft Azure.</span></span> 

<span data-ttu-id="35a05-174">Para crear el bot en Microsoft Azure:</span><span class="sxs-lookup"><span data-stu-id="35a05-174">To create the bot on Microsoft Azure:</span></span>

1. <span data-ttu-id="35a05-175">En Microsoft Azure Portal, haga clic en **Crear un recurso** y, a continuación, busque "Bot Channels Registration" (Registro de canales de bot).</span><span class="sxs-lookup"><span data-stu-id="35a05-175">On the Microsoft Azure Portal, click **Create a resource**, then search for "Bot Channels Registration".</span></span>

1. <span data-ttu-id="35a05-176">Haga clic en **Create**(Crear).</span><span class="sxs-lookup"><span data-stu-id="35a05-176">Click **Create**.</span></span> <span data-ttu-id="35a05-177">Aparece la hoja Bot Channels Registration (Registro de canales de bot).</span><span class="sxs-lookup"><span data-stu-id="35a05-177">The Bot Channels Registration blade appears.</span></span>

    ![Hoja Bot Channels Registration (Registro de canales de bot), con los campos de nombre del bot, suscripción, grupo de recursos, ubicación, plan de tarifa y punto de conexión de mensajería, entre otros.](media/bot-builder-howto-direct-line/bot-service-registration-blade.png)

1. <span data-ttu-id="35a05-179">En la hoja Bot Channels Registration (Registro de canales de bot), escriba el **nombre del bot**, la **suscripción**, el **grupo de recursos**, la **ubicación** y el **plan de tarifa**.</span><span class="sxs-lookup"><span data-stu-id="35a05-179">On the Bot Channels Registration blade, enter the **Bot name**, **Subscription**, **Resource group**, **Location**, and **Pricing tier**.</span></span>

1. <span data-ttu-id="35a05-180">Deje el **Punto de conexión de mensajería** en blanco.</span><span class="sxs-lookup"><span data-stu-id="35a05-180">Leave the **Messaging endpoint** blank.</span></span> <span data-ttu-id="35a05-181">Este valor se rellenará más adelante.</span><span class="sxs-lookup"><span data-stu-id="35a05-181">This value will be filled in later.</span></span>

1. <span data-ttu-id="35a05-182">Haga clic en **Id. y contraseña de aplicación de Microsoft** y, a continuación, haga clic en **Creación automática del id. y la contraseña de la aplicación**.</span><span class="sxs-lookup"><span data-stu-id="35a05-182">Click the **Microsoft App ID and password**, then click **Auto create App ID and password**.</span></span>

1. <span data-ttu-id="35a05-183">Seleccione la casilla **Anclar al panel**.</span><span class="sxs-lookup"><span data-stu-id="35a05-183">Check the **Pin to dashboard** check box.</span></span>

1. <span data-ttu-id="35a05-184">Haga clic en el botón **Crear**.</span><span class="sxs-lookup"><span data-stu-id="35a05-184">Click the **Create** button.</span></span>

<span data-ttu-id="35a05-185">La implementación tardará unos minutos, pero, una vez hecho esto, debería aparecer en el panel.</span><span class="sxs-lookup"><span data-stu-id="35a05-185">Deployment will take a few minutes, but once that's done it should appear in your dashboard.</span></span>

### <a name="update-the-appsettingsjson-file"></a><span data-ttu-id="35a05-186">Actualización del archivo appsettings.json</span><span class="sxs-lookup"><span data-stu-id="35a05-186">Update the appsettings.json file</span></span>

1. <span data-ttu-id="35a05-187">Haga clic en el bot en el panel o, para ir al nuevo recurso, haga clic en **Todos los recursos** y busque el nombre de su registro de canales de bot.</span><span class="sxs-lookup"><span data-stu-id="35a05-187">Click the bot on your dashboard, or go to your new resource by clicking **All resources** and searching for the name of your Bot Channels Registration.</span></span>

1. <span data-ttu-id="35a05-188">Haga clic en **Descripción general**.</span><span class="sxs-lookup"><span data-stu-id="35a05-188">Click on **Overview**.</span></span>

1. <span data-ttu-id="35a05-189">Copie el nombre del grupo de recursos en la cadena **botId** en **Program.cs** del proyecto **DirectLineClientSample**.</span><span class="sxs-lookup"><span data-stu-id="35a05-189">Copy the name of the Resource group into the **botId** string in the **Program.cs** of the **DirectLineClientSample** project.</span></span>

1. <span data-ttu-id="35a05-190">Haga clic en **Configuración** y, a continuación, haga clic en **Administrar** cerca del campo **Microsoft App ID** (Id. de la aplicación de Microsoft).</span><span class="sxs-lookup"><span data-stu-id="35a05-190">Click **Settings**, then click **Manage** near the **Microsoft App ID** field.</span></span>

1. <span data-ttu-id="35a05-191">Copie el id. de la aplicación y péguelo en el campo **"MicrosoftAppId"** del archivo **appsettings.json**.</span><span class="sxs-lookup"><span data-stu-id="35a05-191">Copy the Application ID and paste it into the **"MicrosoftAppId"** field of the **appsettings.json** file.</span></span>

1. <span data-ttu-id="35a05-192">Haga clic en el botón **Generar nueva contraseña**.</span><span class="sxs-lookup"><span data-stu-id="35a05-192">Click the **Generate New Password** button.</span></span>

1. <span data-ttu-id="35a05-193">Copie la nueva contraseña y péguela en el campo **"MicrosoftAppPassword"** del archivo **appsettings.json**.</span><span class="sxs-lookup"><span data-stu-id="35a05-193">Copy the new password and paste it into the **"MicrosoftAppPassword"** field of the **appsettings.json** file.</span></span>

### <a name="set-the-messaging-endpoint"></a><span data-ttu-id="35a05-194">Establecimiento del punto de conexión de mensajería</span><span class="sxs-lookup"><span data-stu-id="35a05-194">Set the Messaging endpoint</span></span>

<span data-ttu-id="35a05-195">Para agregar el punto de conexión al bot:</span><span class="sxs-lookup"><span data-stu-id="35a05-195">To add the endpoint to your bot:</span></span>

1. <span data-ttu-id="35a05-196">Copie la dirección URL del explorador que apareció después de publicar el bot.</span><span class="sxs-lookup"><span data-stu-id="35a05-196">Copy the URL address from the browser that popped up after publishing your bot.</span></span>

    ![Hoja de configuración del registro de canales de bot, con el punto de conexión de mensajería resaltado](media/bot-builder-howto-direct-line/bot-channels-registration-settings.png)

1. <span data-ttu-id="35a05-198">Aparece la hoja **Configuración** del bot.</span><span class="sxs-lookup"><span data-stu-id="35a05-198">Bring up the **Settings** blade of your bot.</span></span>

1. <span data-ttu-id="35a05-199">Pegue la dirección en el **punto de conexión de mensajería**.</span><span class="sxs-lookup"><span data-stu-id="35a05-199">Paste the address into the **Messaging endpoint**.</span></span>

1. <span data-ttu-id="35a05-200">Edite la dirección para que empiece por "https://" y termine por "/api/messages".</span><span class="sxs-lookup"><span data-stu-id="35a05-200">Edit the address to start with "https://" and end with "/api/messages".</span></span> <span data-ttu-id="35a05-201">Por ejemplo, si la dirección copiada del explorador es "http://v-royhar-dlbot-directlinebotsample20180329044602.azurewebsites.net", modifíquela a "https://v-royhar-dlbot-directlinebotsample20180329044602.azurewebsites.net/api/messages".</span><span class="sxs-lookup"><span data-stu-id="35a05-201">For example, if the address copied from the browser is "http://v-royhar-dlbot-directlinebotsample20180329044602.azurewebsites.net", edit it to "https://v-royhar-dlbot-directlinebotsample20180329044602.azurewebsites.net/api/messages".</span></span>

1. <span data-ttu-id="35a05-202">Haga clic en el botón **Guardar** en la hoja de configuración.</span><span class="sxs-lookup"><span data-stu-id="35a05-202">Click the **Save** button on the Settings blade.</span></span>

<span data-ttu-id="35a05-203">**NOTA:** Si se produce un error de publicación, es posible que deba detener el bot en Azure para poder publicar los cambios en el bot.</span><span class="sxs-lookup"><span data-stu-id="35a05-203">**NOTE:** If publishing fails, you may need to stop your bot on Azure before you can publish changes to your bot.</span></span>

1. <span data-ttu-id="35a05-204">Busque el nombre de App Service (se encuentra entre "https://" y ".azurewebsites.net").</span><span class="sxs-lookup"><span data-stu-id="35a05-204">Find your App Service name (it's between the "https://" and ".azurewebsites.net".</span></span>

1. <span data-ttu-id="35a05-205">En "Todos los recursos" de Azure Portal, busque ese recurso.</span><span class="sxs-lookup"><span data-stu-id="35a05-205">Search for that resource in the Azure Portal "All resources".</span></span>

1. <span data-ttu-id="35a05-206">Haga clic en el recurso de App Service.</span><span class="sxs-lookup"><span data-stu-id="35a05-206">Click on the App Service resource.</span></span> <span data-ttu-id="35a05-207">Aparece la hoja de App Service.</span><span class="sxs-lookup"><span data-stu-id="35a05-207">The App Serivce blade appears.</span></span>

1. <span data-ttu-id="35a05-208">Haga clic en el botón **Detener** en la hoja de App Service.</span><span class="sxs-lookup"><span data-stu-id="35a05-208">Click the **Stop** button on the App Service blade.</span></span>

1. <span data-ttu-id="35a05-209">En Visual Studio, vuelva a publicar el bot.</span><span class="sxs-lookup"><span data-stu-id="35a05-209">In Visual Studio, Publish your bot again.</span></span>

1. <span data-ttu-id="35a05-210">Haga clic en el botón **Inicio** en la hoja de App Service.</span><span class="sxs-lookup"><span data-stu-id="35a05-210">Click the **Start** button on the App Service blade.</span></span>

### <a name="test-your-bot-in-webchat"></a><span data-ttu-id="35a05-211">Prueba del bot en el chat en web</span><span class="sxs-lookup"><span data-stu-id="35a05-211">Test your bot in webchat</span></span>

<span data-ttu-id="35a05-212">Para comprobar que el bot funciona, compruébelo en el Chat en web:</span><span class="sxs-lookup"><span data-stu-id="35a05-212">To verify that your bot is working, check your bot in webchat:</span></span>

1. <span data-ttu-id="35a05-213">En la hoja Bot Channels Registration (Registro de canales del bot) del bot, haga clic en **Configuración** y, a continuación, en **Probar en Chat en web**.</span><span class="sxs-lookup"><span data-stu-id="35a05-213">In the Bot Channels Registration blade for your bot, click **Settings**, then **Test in Web Chat**.</span></span>

1. <span data-ttu-id="35a05-214">Escriba "Hola".</span><span class="sxs-lookup"><span data-stu-id="35a05-214">Type "Hi".</span></span> <span data-ttu-id="35a05-215">El bot debería responder con un mensaje de bienvenida.</span><span class="sxs-lookup"><span data-stu-id="35a05-215">The bot should respond with a welcome message.</span></span>

1. <span data-ttu-id="35a05-216">Escriba "Mostrarme una tarjeta de imagen prominente".</span><span class="sxs-lookup"><span data-stu-id="35a05-216">Type "show me a hero card".</span></span> <span data-ttu-id="35a05-217">El bot debería mostrar una tarjeta de imagen prominente.</span><span class="sxs-lookup"><span data-stu-id="35a05-217">The bot should display a hero card.</span></span>

1. <span data-ttu-id="35a05-218">Escriba "Enviarme una imagen de botframework".</span><span class="sxs-lookup"><span data-stu-id="35a05-218">Type "send me a botframework image".</span></span> <span data-ttu-id="35a05-219">El bot debería mostrar una imagen de la documentación de Bot Framework.</span><span class="sxs-lookup"><span data-stu-id="35a05-219">The bot should display an image from the Bot Framework documentation.</span></span>

1. <span data-ttu-id="35a05-220">Escriba cualquier otra cosa y el bot debería responder con "Ha dicho" y el mensaje entre comillas.</span><span class="sxs-lookup"><span data-stu-id="35a05-220">Type anything else and the bot should reply with, "You said" and your message in quotes.</span></span>

### <a name="troubleshooting"></a><span data-ttu-id="35a05-221">solución de problemas</span><span class="sxs-lookup"><span data-stu-id="35a05-221">Troubleshooting</span></span>

<span data-ttu-id="35a05-222">Si el bot no funciona, compruebe o vuelva a escribir el id. y la contraseña de aplicación de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="35a05-222">If your bot is not working, verify or reenter your Microsoft App ID and password.</span></span> <span data-ttu-id="35a05-223">Aunque lo haya copiado con anterioridad, compruebe el id. de la aplicación de Microsoft en la hoja de configuración de Bot Channels Registration (Registro de canales del bot) con el valor del campo **"MicrosoftAppId"** del archivo appsettings.json.</span><span class="sxs-lookup"><span data-stu-id="35a05-223">Even if you copied it before, check the Microsoft App ID on the settings blade of your Bot Channels Registration against the value in the **"MicrosoftAppId"** field in the appsettings.json file.</span></span>

<span data-ttu-id="35a05-224">Compruebe la contraseña o cree y use una nueva:</span><span class="sxs-lookup"><span data-stu-id="35a05-224">Verify your password or create and use a new password:</span></span> 

1. <span data-ttu-id="35a05-225">Haga clic en **Administrar** junto al campo **Microsoft App ID** (Id. de la aplicación de Microsoft) en la hoja Bot Channels Registration (Registro de canales del bot).</span><span class="sxs-lookup"><span data-stu-id="35a05-225">Click **Manage** next to the **Microsoft App ID** field on your Bot Channels Registration blade.</span></span>

1. <span data-ttu-id="35a05-226">Inicie sesión en el Portal de registro de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="35a05-226">Log into the Application Registration Portal.</span></span>

1. <span data-ttu-id="35a05-227">Compruebe si las tres primeras letras de la contraseña coinciden con el campo **"MicrosoftAppPassword"** del archivo **appsettings.json**.</span><span class="sxs-lookup"><span data-stu-id="35a05-227">Verify the first three letters of your password match the **"MicrosoftAppPassword"** field in the **appsettings.json** file.</span></span> 

1. <span data-ttu-id="35a05-228">Si los valores no coinciden, genere una nueva contraseña y almacene ese valor en el campo **"MicrosoftAppPassword"** del archivo **appsettings.json**.</span><span class="sxs-lookup"><span data-stu-id="35a05-228">If the values do not match, generate a new password and store that value in the **"MicrosoftAppPassword"** field in the **appsettings.json** file.</span></span>

# <a name="javascripttabjscreatebot"></a>[<span data-ttu-id="35a05-229">JavaScript</span><span class="sxs-lookup"><span data-stu-id="35a05-229">JavaScript</span></span>](#tab/jscreatebot)

### <a name="create-web-app-bot-on-microsoft-azure"></a><span data-ttu-id="35a05-230">Creación de una aplicación web en Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="35a05-230">Create Web App Bot on Microsoft Azure</span></span>

<span data-ttu-id="35a05-231">Para crear el bot en Microsoft Azure:</span><span class="sxs-lookup"><span data-stu-id="35a05-231">To create the bot on Microsoft Azure:</span></span> 

1. <span data-ttu-id="35a05-232">En Microsoft Azure Portal, haga clic en **Crear un recurso** y, a continuación, busque "Bot de aplicación web".</span><span class="sxs-lookup"><span data-stu-id="35a05-232">On the Microsoft Azure Portal, click **Create a resource**, then search for "Web App Bot".</span></span>

1. <span data-ttu-id="35a05-233">Haga clic en **Create**(Crear).</span><span class="sxs-lookup"><span data-stu-id="35a05-233">Click **Create**.</span></span> <span data-ttu-id="35a05-234">Aparece la hoja Bot de aplicación web.</span><span class="sxs-lookup"><span data-stu-id="35a05-234">The Web App Bot blade appears.</span></span>

![Registro del bot de aplicación web](media/bot-builder-howto-direct-line/web-app-bot-registration.png)

1. <span data-ttu-id="35a05-236">En la hoja Bot de aplicación web, escriba el **nombre del bot**, la **suscripción**, el **grupo de recursos**, la **ubicación**, el **plan de tarifa** y el **nombre de la aplicación**.</span><span class="sxs-lookup"><span data-stu-id="35a05-236">On the Web App Bot blade, enter the **Bot name**, **Subscription**, **Resource group**, **Location**, **Pricing tier**, **App name**.</span></span>

1. <span data-ttu-id="35a05-237">Seleccione la plantilla de bot que quiere utilizar.</span><span class="sxs-lookup"><span data-stu-id="35a05-237">Select the bot template you would like to use.</span></span> <span data-ttu-id="35a05-238">**Versión de SDK: SDK v4** y **lenguaje de SDK: Node.js**</span><span class="sxs-lookup"><span data-stu-id="35a05-238">**SDK version: SDK v4** and **SDK language: Node.js**</span></span> 

1. <span data-ttu-id="35a05-239">Seleccione la plantilla básica de versión preliminar de v4.</span><span class="sxs-lookup"><span data-stu-id="35a05-239">Select the basic v4 preview template.</span></span> 

1. <span data-ttu-id="35a05-240">Elija el **plan de App Service/ubicación**, **Azure Storage** y la **ubicación de Application Insights**.</span><span class="sxs-lookup"><span data-stu-id="35a05-240">Choose your **App service plan/Location**, **Azure Storage** and **Application Insights Location**.</span></span>

1. <span data-ttu-id="35a05-241">Seleccione la casilla Anclar al panel.</span><span class="sxs-lookup"><span data-stu-id="35a05-241">Check the Pin to dashboard check box.</span></span>

1. <span data-ttu-id="35a05-242">Haga clic en el botón Crear.</span><span class="sxs-lookup"><span data-stu-id="35a05-242">Click the Create button.</span></span>

1. <span data-ttu-id="35a05-243">Espere a que se implemente el bot.</span><span class="sxs-lookup"><span data-stu-id="35a05-243">Wait for your bot to be deployed.</span></span> <span data-ttu-id="35a05-244">Como seleccionó la casilla Anclar al panel, el bot aparecerá en el panel.</span><span class="sxs-lookup"><span data-stu-id="35a05-244">Because you checked the Pin to dashboard check box, your bot will appear on your dashboard.</span></span>

### <a name="edit-your-direct-line-bot"></a><span data-ttu-id="35a05-245">Edición del bot de Direct Line</span><span class="sxs-lookup"><span data-stu-id="35a05-245">Edit your Direct Line Bot</span></span>

<span data-ttu-id="35a05-246">Ahora vamos a agregar algunas características más al bot de eco.</span><span class="sxs-lookup"><span data-stu-id="35a05-246">Now lets add some more features to the echo bot.</span></span>

1. <span data-ttu-id="35a05-247">En la hoja Bot de aplicación web, haga clic en Crear.</span><span class="sxs-lookup"><span data-stu-id="35a05-247">On the Web App Bot blade, click Build.</span></span> 

1. <span data-ttu-id="35a05-248">Haga clic en Descargar el archivo ZIP.</span><span class="sxs-lookup"><span data-stu-id="35a05-248">Click Download zip file.</span></span> 

1. <span data-ttu-id="35a05-249">Extraiga el archivo ZIP en un directorio local.</span><span class="sxs-lookup"><span data-stu-id="35a05-249">Extract the .zip file to a local directory.</span></span>

1. <span data-ttu-id="35a05-250">Vaya a la carpeta extraída y abra los archivos de origen en su IDE favorito.</span><span class="sxs-lookup"><span data-stu-id="35a05-250">Navigate to the extracted folder and open the source files in your favorite IDE.</span></span>

1. <span data-ttu-id="35a05-251">En el archivo app.js, copie y pegue el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="35a05-251">In the app.js file copy and paste the code below.</span></span>

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

<span data-ttu-id="35a05-252">Cuando esté listo, puede volver a publicar los orígenes en Azure.</span><span class="sxs-lookup"><span data-stu-id="35a05-252">When you are ready, you can publish the sources back to Azure.</span></span> [<span data-ttu-id="35a05-253">Siga estos pasos para obtener información sobre cómo volver a publicar el bot en Azure</span><span class="sxs-lookup"><span data-stu-id="35a05-253">Follow these steps to learn how to publish your bot back to Azure</span></span>](../bot-service-build-download-source-code.md#publish-node-bot-source-code-to-azure)

---


## <a name="create-the-console-client-app"></a><span data-ttu-id="35a05-254">Creación de la aplicación cliente de consola</span><span class="sxs-lookup"><span data-stu-id="35a05-254">Create the console client app</span></span>

# <a name="ctabcsclientapp"></a>[<span data-ttu-id="35a05-255">C#</span><span class="sxs-lookup"><span data-stu-id="35a05-255">C#</span></span>](#tab/csclientapp)

<span data-ttu-id="35a05-256">La aplicación cliente de consola opera en dos subprocesos.</span><span class="sxs-lookup"><span data-stu-id="35a05-256">The console client application operates in two threads.</span></span> <span data-ttu-id="35a05-257">El subproceso principal acepta la entrada del usuario y envía mensajes al bot.</span><span class="sxs-lookup"><span data-stu-id="35a05-257">The primary thread accepts user input and sends messages to the bot.</span></span> <span data-ttu-id="35a05-258">El subproceso secundario sondea el bot una vez por segundo para recuperar los mensajes del bot y, a continuación, muestra los mensajes recibidos.</span><span class="sxs-lookup"><span data-stu-id="35a05-258">The secondary thread polls the bot once per second to retrieve any messages from the bot, then displays the messages received.</span></span>

<span data-ttu-id="35a05-259">Para crear un proyecto de consola:</span><span class="sxs-lookup"><span data-stu-id="35a05-259">To create the console project:</span></span>

1. <span data-ttu-id="35a05-260">En el Explorador de soluciones, haga clic con el botón derecho en **Solution 'DirectLineBotSample'** (solución "DirectLineBotSample").</span><span class="sxs-lookup"><span data-stu-id="35a05-260">Right-click the **Solution 'DirectLineBotSample'** in Solution Explorer.</span></span>

1. <span data-ttu-id="35a05-261">Elija **Agregar** > **Nuevo proyecto**.</span><span class="sxs-lookup"><span data-stu-id="35a05-261">Choose **Add** > **New Project**.</span></span>

1. <span data-ttu-id="35a05-262">En **Visual C#** > \**Escritorio clásico de Windows*, elija **Aplicación de consola (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="35a05-262">In **Visual C#** > \**Windows Classic Desktop*, choose **Console App (.NET Framework)**.</span></span>
 
    <span data-ttu-id="35a05-263">**Nota:** No elija **Aplicación de consola (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="35a05-263">**Note:** Do not choose **Console App (.NET Core)**.</span></span>

1. <span data-ttu-id="35a05-264">En el nombre, escriba **DirectLineClientSample**.</span><span class="sxs-lookup"><span data-stu-id="35a05-264">For the name, enter **DirectLineClientSample**.</span></span>

1. <span data-ttu-id="35a05-265">Haga clic en **OK**.</span><span class="sxs-lookup"><span data-stu-id="35a05-265">Click **OK**.</span></span>

### <a name="add-the-nuget-packages-to-the-console-app"></a><span data-ttu-id="35a05-266">Agregue los paquetes NuGet a la aplicación de consola.</span><span class="sxs-lookup"><span data-stu-id="35a05-266">Add the NuGet packages to the console app</span></span>

1. <span data-ttu-id="35a05-267">Haga clic con el botón secundario en **Referencias**.</span><span class="sxs-lookup"><span data-stu-id="35a05-267">Right-click **References**.</span></span>

1. <span data-ttu-id="35a05-268">Haga clic en **Administración de paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="35a05-268">Click **Manage NuGet Packages**.</span></span>

1. <span data-ttu-id="35a05-269">Haga clic en **Examinar**.</span><span class="sxs-lookup"><span data-stu-id="35a05-269">Click **Browse**.</span></span> <span data-ttu-id="35a05-270">Asegúrese de que la casilla **Incluir versión preliminar** esté activada.</span><span class="sxs-lookup"><span data-stu-id="35a05-270">Make sure the **Include prerelease** check box is checked.</span></span>

1. <span data-ttu-id="35a05-271">Busque e instale los siguientes paquetes NuGet:</span><span class="sxs-lookup"><span data-stu-id="35a05-271">Search for and install the following NuGet packages:</span></span>
    - <span data-ttu-id="35a05-272">Microsoft.Bot.Connector.DirectLine (v3.0.2)</span><span class="sxs-lookup"><span data-stu-id="35a05-272">Microsoft.Bot.Connector.DirectLine (v3.0.2)</span></span>
    - <span data-ttu-id="35a05-273">Newtonsoft.Json</span><span class="sxs-lookup"><span data-stu-id="35a05-273">Newtonsoft.Json</span></span>

### <a name="edit-the-programcs-file"></a><span data-ttu-id="35a05-274">Edición del archivo Program.cs</span><span class="sxs-lookup"><span data-stu-id="35a05-274">Edit the Program.cs file</span></span>

<span data-ttu-id="35a05-275">Reemplace el contenido del archivo **Program.cs** de DirectLineClientSample por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="35a05-275">Replace the contents of the DirectLineClientSample **Program.cs** file with the following:</span></span>

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

<span data-ttu-id="35a05-276">Para comprobar que todo está en orden, presione **F6** para compilar el proyecto.</span><span class="sxs-lookup"><span data-stu-id="35a05-276">To verify things are in order, press **F6** to build the project.</span></span> <span data-ttu-id="35a05-277">No debería haber advertencias ni errores.</span><span class="sxs-lookup"><span data-stu-id="35a05-277">There should be no warnings or errors.</span></span>

# <a name="javascripttabjsclientapp"></a>[<span data-ttu-id="35a05-278">JavaScript</span><span class="sxs-lookup"><span data-stu-id="35a05-278">JavaScript</span></span>](#tab/jsclientapp)

### <a name="create-a-direct-line-client"></a><span data-ttu-id="35a05-279">Creación de un cliente de Direct Line</span><span class="sxs-lookup"><span data-stu-id="35a05-279">Create a Direct Line Client</span></span> 

<span data-ttu-id="35a05-280">Ahora que ha implementado el bot de aplicación web, podemos crear un cliente de Direct Line.</span><span class="sxs-lookup"><span data-stu-id="35a05-280">Now that you have deployed your web app bot we can create a Direct Line Client.</span></span>

```
    md DirectLineClient
    cd DirectLineClient
    npm init
```

<span data-ttu-id="35a05-281">A continuación, instale estos paquetes de npm.</span><span class="sxs-lookup"><span data-stu-id="35a05-281">Next, install these packages from npm</span></span>

```
    npm install --save open
    npm install --save request
    npm install --save request-promise
    npm install --save swagger-client
```

<span data-ttu-id="35a05-282">Cree un archivo **app.js**.</span><span class="sxs-lookup"><span data-stu-id="35a05-282">Create an **app.js** file.</span></span> <span data-ttu-id="35a05-283">Copie y pegue el código siguiente en este archivo.</span><span class="sxs-lookup"><span data-stu-id="35a05-283">Copy and paste the code below into this file.</span></span>

<span data-ttu-id="35a05-284">Se obtendrá directLineSecret en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="35a05-284">We will get the directLineSecret in the next step.</span></span>
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

## <a name="configure-the-direct-line-channel"></a><span data-ttu-id="35a05-285">Configuración del canal de Direct Line</span><span class="sxs-lookup"><span data-stu-id="35a05-285">Configure the Direct Line channel</span></span>

<span data-ttu-id="35a05-286">La adición del canal de Direct Line transforma este bot en un bot de Direct Line.</span><span class="sxs-lookup"><span data-stu-id="35a05-286">Adding the Direct Line channel makes this bot into a Direct Line bot.</span></span>

<span data-ttu-id="35a05-287">Para configurar el canal de Direct Line:</span><span class="sxs-lookup"><span data-stu-id="35a05-287">To configure the Direct Line channel:</span></span>

![Hoja Conexión a los canales con el botón de canal de Direct Line resaltado.](media/bot-builder-howto-direct-line/direct-line-channel-button.png)

1. <span data-ttu-id="35a05-289">En la hoja **Bot Channels Registration** (Registro de canales de bot), haga clic en **Canales**.</span><span class="sxs-lookup"><span data-stu-id="35a05-289">On the **Bot Channels Registration** blade, click **Channels**.</span></span> <span data-ttu-id="35a05-290">Aparecerá la hoja **Conexión a los canales**.</span><span class="sxs-lookup"><span data-stu-id="35a05-290">The **Connect to Channels** blade will appear.</span></span>

    ![Hoja Configure Direct Line (Configuración de Direct Line) que muestra ambas claves secretas.](media/bot-builder-howto-direct-line/configure-direct-line.png)

1. <span data-ttu-id="35a05-292">En la hoja **Conexión a los canales**, haga clic en el botón **Configure Direct Line channel** (Configurar canal de Direct Line).</span><span class="sxs-lookup"><span data-stu-id="35a05-292">On the **Connect to Channels** blade, click the **Configure Direct Line channel** button.</span></span> 

1. <span data-ttu-id="35a05-293">Si la opción **3.0** no está activada, coloque una marca de verificación en la casilla de verificación.</span><span class="sxs-lookup"><span data-stu-id="35a05-293">If **3.0** is not checked, put a check mark in the check box.</span></span>

1. <span data-ttu-id="35a05-294">Haga clic en **Mostrar** para una clave secreta por lo menos.</span><span class="sxs-lookup"><span data-stu-id="35a05-294">Click **Show** for at least one secret key.</span></span>

1. <span data-ttu-id="35a05-295">Copie una de las claves secretas y péguela en la aplicación cliente, en la cadena **directLineSecret**.</span><span class="sxs-lookup"><span data-stu-id="35a05-295">Copy one of the secret keys and paste it into your client app, into the **directLineSecret** string.</span></span>

1. <span data-ttu-id="35a05-296">Haga clic en Done (Listo).</span><span class="sxs-lookup"><span data-stu-id="35a05-296">Click Done.</span></span>

## <a name="run-the-client-app"></a><span data-ttu-id="35a05-297">Ejecución de la aplicación cliente</span><span class="sxs-lookup"><span data-stu-id="35a05-297">Run the client app</span></span>

# <a name="ctabcsrunclient"></a>[<span data-ttu-id="35a05-298">C#</span><span class="sxs-lookup"><span data-stu-id="35a05-298">C#</span></span>](#tab/csrunclient)

<span data-ttu-id="35a05-299">El bot ya está listo para comunicarse con la aplicación cliente de consola de Direct Line.</span><span class="sxs-lookup"><span data-stu-id="35a05-299">Your bot is now ready to communicate with the Direct Line console client application.</span></span> <span data-ttu-id="35a05-300">Para ejecutar la aplicación de consola, haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="35a05-300">To run the console app, do the following:</span></span>

1. <span data-ttu-id="35a05-301">En Visual Studio, haga clic con el botón derecho en el proyecto **DirectLineClientSample** y seleccione **Set as StartUp Project**. (Establecer como proyecto de inicio).</span><span class="sxs-lookup"><span data-stu-id="35a05-301">In Visual Studio, right click **DirectLineClientSample** project and select **Set as StartUp Project**.</span></span>

1. <span data-ttu-id="35a05-302">Examine el archivo **Program.cs** en el proyecto **DirectLineClientSample**.</span><span class="sxs-lookup"><span data-stu-id="35a05-302">Examine the **Program.cs** file in the **DirectLineClientSample** project.</span></span>

    - <span data-ttu-id="35a05-303">Compruebe que el código secreto de Direct Line está en la cadena **directLineSecret**.</span><span class="sxs-lookup"><span data-stu-id="35a05-303">Verify that the one Direct Line secret code is in the **directLineSecret** string.</span></span>

1. <span data-ttu-id="35a05-304">Si **directLineSecret** no es correcto, vaya a Azure Portal, haga clic en el bot de Direct Line, en **Canales**, luego en **Configure Direct Line channel** (Configurar canal de Direct Line) (o **Editar**), mostrar una clave y luego cópiela en la cadena **directLineSecret**.</span><span class="sxs-lookup"><span data-stu-id="35a05-304">If the **directLineSecret** isn't correct, go to the Azure portal, click your Direct Line bot, click **Channels**, click **Configure Direct Line channel** (or **Edit**), show a key, then copy that key to the **directLineSecret** string.</span></span>

    - <span data-ttu-id="35a05-305">Compruebe que el grupo de recursos está en la cadena **botId**.</span><span class="sxs-lookup"><span data-stu-id="35a05-305">Verify that the resource group is in the **botId** string.</span></span>

1. <span data-ttu-id="35a05-306">Si no lo está, desde la hoja **Introducción**, copie y pegue el **grupo de recursos** en la cadena **botId**.</span><span class="sxs-lookup"><span data-stu-id="35a05-306">If it isn't, from the **Overview** blade, copy-and-paste the **Resource group** into the **botId** string.</span></span>

1. <span data-ttu-id="35a05-307">Pulse F5 para iniciar la depuración.</span><span class="sxs-lookup"><span data-stu-id="35a05-307">Press F5 or Start debugging.</span></span>

<span data-ttu-id="35a05-308">Se iniciará la aplicación cliente de consola.</span><span class="sxs-lookup"><span data-stu-id="35a05-308">The console client app will start.</span></span> <span data-ttu-id="35a05-309">Para probar la aplicación:</span><span class="sxs-lookup"><span data-stu-id="35a05-309">To test out the app:</span></span> 

1. <span data-ttu-id="35a05-310">Escriba "hola".</span><span class="sxs-lookup"><span data-stu-id="35a05-310">Enter "Hi".</span></span> <span data-ttu-id="35a05-311">El bot debería mostrar "Ha dicho 'Hola'".</span><span class="sxs-lookup"><span data-stu-id="35a05-311">The bot should display 'You said "Hi"'.</span></span>

1. <span data-ttu-id="35a05-312">Escriba "Mostrarme una tarjeta de imagen prominente".</span><span class="sxs-lookup"><span data-stu-id="35a05-312">Enter "show me a hero card".</span></span> <span data-ttu-id="35a05-313">El bot debería mostrar una tarjeta de imagen prominente.</span><span class="sxs-lookup"><span data-stu-id="35a05-313">The bot should display a hero card.</span></span>

1. <span data-ttu-id="35a05-314">Escriba "Enviarme una imagen de botframework".</span><span class="sxs-lookup"><span data-stu-id="35a05-314">Enter "send me a botframework image".</span></span> <span data-ttu-id="35a05-315">El bot iniciará un explorador para mostrar una imagen de la documentación de Bot Framework.</span><span class="sxs-lookup"><span data-stu-id="35a05-315">The bot will launch a browser to display an image from the Bot Framework documentation.</span></span>

1. <span data-ttu-id="35a05-316">Escriba cualquier otra cosa y el bot debería responder con "Ha dicho" y el mensaje entre comillas.</span><span class="sxs-lookup"><span data-stu-id="35a05-316">Enter anything else and the bot should reply with, "You said" and your message in quotes.</span></span>

# <a name="javascripttabjsrunclient"></a>[<span data-ttu-id="35a05-317">JavaScript</span><span class="sxs-lookup"><span data-stu-id="35a05-317">JavaScript</span></span>](#tab/jsrunclient)

<span data-ttu-id="35a05-318">Para ejecutar el ejemplo, deberá ejecutar la aplicación DirectLineClient.</span><span class="sxs-lookup"><span data-stu-id="35a05-318">To run the sample you will need to run your DirectLineClient app.</span></span>

1. <span data-ttu-id="35a05-319">Abra una consola CMD y CD en el directorio DirectLineClient.</span><span class="sxs-lookup"><span data-stu-id="35a05-319">Open a CMD console and CD to the DirectLineClient directory</span></span>

1. <span data-ttu-id="35a05-320">Ejecute `node app.js`</span><span class="sxs-lookup"><span data-stu-id="35a05-320">Run `node app.js`</span></span>

<span data-ttu-id="35a05-321">Para probar los tipos de mensajes personalizados en la consola del cliente, muéstreme una tarjeta de imagen prominente o envíeme una imagen de botframework y verá el resultado siguiente.</span><span class="sxs-lookup"><span data-stu-id="35a05-321">To test the custom messages type in the Client's console show me a hero card or send me a botframework image and you should see the following outcome.</span></span>

![resultado](media/bot-builder-howto-direct-line/outcome.png)

---


### <a name="next-steps"></a><span data-ttu-id="35a05-323">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="35a05-323">Next steps</span></span>
