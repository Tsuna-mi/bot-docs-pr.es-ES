---
title: Autenticación de actividades con .NET Core | Microsoft Docs
description: Obtenga información sobre cómo autenticar las actividades de bots con .NET Core.
author: v-ducvo
ms.author: v-ducvo
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/17
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: 6df7923caa708ac2b10af37d860dfac317e113a0
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305097"
---
# <a name="authenticating-activities-using-net-core"></a><span data-ttu-id="f4eb2-103">Autenticación de actividades con .NET Core</span><span class="sxs-lookup"><span data-stu-id="f4eb2-103">Authenticating activities using .NET Core</span></span>

<span data-ttu-id="f4eb2-104">Si decide desarrollar el bot con [.NET Core](/dotnet/core/index), puede usar el [Bot Framework Connector](bot-builder-dotnet-connector.md) para enviar y recibir mensajes de [actividad](https://docs.botframework.com/en-us/csharp/builder/sdkreference/dc/d2f/class_microsoft_1_1_bot_1_1_connector_1_1_activity.html) desde el bot.</span><span class="sxs-lookup"><span data-stu-id="f4eb2-104">If you choose to develop your bot using [.NET Core](/dotnet/core/index), you can use the [Bot Framework Connector](bot-builder-dotnet-connector.md) to send and receive [activity](https://docs.botframework.com/en-us/csharp/builder/sdkreference/dc/d2f/class_microsoft_1_1_bot_1_1_connector_1_1_activity.html) messages from your bot.</span></span> <span data-ttu-id="f4eb2-105">Para usar el servicio Connector, debe configurar el modelo de autenticación adecuado para la versión del marco de destino.</span><span class="sxs-lookup"><span data-stu-id="f4eb2-105">To use the Connector service, you need to set up the proper authentication model for the framework version you are targeting.</span></span>

<span data-ttu-id="f4eb2-106">Bot Framework Connector.AspNetCore admite las siguientes versiones de ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="f4eb2-106">The Bot Framework Connector.AspNetCore supports the following versions of ASP.NET:</span></span>
* <span data-ttu-id="f4eb2-107">Para .NET Core v1.1/AspNetCore1.x, use **Microsoft.Bot.Connector.AspNetCore 1.x**</span><span class="sxs-lookup"><span data-stu-id="f4eb2-107">For .NET Core v1.1/AspNetCore1.x use **Microsoft.Bot.Connector.AspNetCore 1.x**</span></span>
* <span data-ttu-id="f4eb2-108">Para .NET Core v2.0/AspNetCore2.x, use **Microsoft.Bot.Connector.AspNetCore 2.x**</span><span class="sxs-lookup"><span data-stu-id="f4eb2-108">For .NET Core v2.0/AspNetCore2.x use **Microsoft.Bot.Connector.AspNetCore 2.x**</span></span>

<span data-ttu-id="f4eb2-109">En este artículo se muestra cómo configurar el modelo de autenticación para el marco específico que el bot tiene como destino.</span><span class="sxs-lookup"><span data-stu-id="f4eb2-109">This article will show you how to set up authentication model for the specific framework that your bot targets.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f4eb2-110">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="f4eb2-110">Prerequisites</span></span>

* [<span data-ttu-id="f4eb2-111">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="f4eb2-111">Visual Studio 2017</span></span>](https://www.visualstudio.com/downloads/)
* <span data-ttu-id="f4eb2-112">[.NET Core](https://www.microsoft.com/net/download/windows).</span><span class="sxs-lookup"><span data-stu-id="f4eb2-112">[.NET Core](https://www.microsoft.com/net/download/windows).</span></span> <span data-ttu-id="f4eb2-113">Instale la versión de .NET Core de destino (p. ej.: .NET Core v1.1 o .NET Core v2.0).</span><span class="sxs-lookup"><span data-stu-id="f4eb2-113">Install .NET Core version you target (e.g.: .NET Core v1.1 or .NET Core v 2.0).</span></span>
* <span data-ttu-id="f4eb2-114">[Registre un bot](~/bot-service-quickstart-registration.md).</span><span class="sxs-lookup"><span data-stu-id="f4eb2-114">[Register a bot](~/bot-service-quickstart-registration.md).</span></span> <span data-ttu-id="f4eb2-115">Registre el bot para obtener un AppID y una contraseña necesarios para el proceso de autenticación.</span><span class="sxs-lookup"><span data-stu-id="f4eb2-115">Register your bot to obtain an AppID and Password that is needed for the authentication process.</span></span>

## <a name="create-a-net-core-project"></a><span data-ttu-id="f4eb2-116">Creación de un proyecto de .NET Core</span><span class="sxs-lookup"><span data-stu-id="f4eb2-116">Create a .NET Core project</span></span>

<span data-ttu-id="f4eb2-117">Para crear el proyecto de .NET Core, realice lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="f4eb2-117">To create your .NET Core project, do the following:</span></span>

1. <span data-ttu-id="f4eb2-118">Abra Visual Studio 2017 y haga clic en **Archivo > Nuevo > Proyecto…**</span><span class="sxs-lookup"><span data-stu-id="f4eb2-118">Open Visual Studio 2017 and click **File > New > Project...**.</span></span>
2. <span data-ttu-id="f4eb2-119">Expanda el nodo **Visual C#** y haga clic en **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="f4eb2-119">Expand the **Visual C#** node and click **.NET Core**.</span></span>
3. <span data-ttu-id="f4eb2-120">Elija el tipo de proyecto **Aplicación web ASP.NET Core** y rellene la información del proyecto (p. ej.: los campos Nombre, Ubicación y Nombre de la solución).</span><span class="sxs-lookup"><span data-stu-id="f4eb2-120">Choose **ASP.NET Core Web Application** project type and fill in the project information (e.g.: Name, Location, and Solution name fields).</span></span>
4. <span data-ttu-id="f4eb2-121">Haga clic en **OK**.</span><span class="sxs-lookup"><span data-stu-id="f4eb2-121">Click **OK**.</span></span>
5. <span data-ttu-id="f4eb2-122">Asegúrese de que el proyecto tenga como destino *.NET Core* y la versión de *ASP.NET Core* que desee.</span><span class="sxs-lookup"><span data-stu-id="f4eb2-122">Ensure that the project is targeting *.NET Core* and the *ASP.NET Core* version you want.</span></span> <span data-ttu-id="f4eb2-123">Por ejemplo, la captura de pantalla siguiente muestra que el proyecto tiene como destino **.NET Core** y **ASP.NET Core 2.0**:</span><span class="sxs-lookup"><span data-stu-id="f4eb2-123">For example, the screenshot below shows the project is targeting **.NET Core** and **ASP.NET Core 2.0**:</span></span>

![Crear un proyecto de ASP.NET Core v2.0](~/media/dotnet-core-authentication/create-asp-net-core-2x-project.png)
 
  > [!NOTE]
  > <span data-ttu-id="f4eb2-125">Si el destino es **ASP.NET Core 2.0**, asegúrese de que la versión instalada en nuestra máquina sea al menos 2.x y posterior.</span><span class="sxs-lookup"><span data-stu-id="f4eb2-125">If you are targeting **ASP.NET Core 2.0**, make sure the installed version on our machine is at least 2.x and above.</span></span>

6. <span data-ttu-id="f4eb2-126">Elija el tipo de proyecto **API web**.</span><span class="sxs-lookup"><span data-stu-id="f4eb2-126">Choose **Web API** project type.</span></span>
7. <span data-ttu-id="f4eb2-127">Haga clic en **Aceptar** para crear el proyecto.</span><span class="sxs-lookup"><span data-stu-id="f4eb2-127">Click **OK** to create the project.</span></span>

## <a name="download-the-nuget-package"></a><span data-ttu-id="f4eb2-128">Descarga del paquete NuGet</span><span class="sxs-lookup"><span data-stu-id="f4eb2-128">Download the NuGet package</span></span>

<span data-ttu-id="f4eb2-129">Para usar el **Bot Framework Connector**, instale el paquete NuGet adecuado para su versión de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="f4eb2-129">To use the **Bot Framework Connector**, install the NuGet package appropriate for your .NET Core version.</span></span> <span data-ttu-id="f4eb2-130">Realice los pasos siguientes para instalar el paquete NuGet:</span><span class="sxs-lookup"><span data-stu-id="f4eb2-130">To install the NuGet package, do the following:</span></span>

1. <span data-ttu-id="f4eb2-131">En el **Explorador de soluciones**, haga clic con el botón derecho en el nombre del proyecto y **Administrar paquetes NuGet…**</span><span class="sxs-lookup"><span data-stu-id="f4eb2-131">From **Solution Explorer**, right-click the project name and **Manage NuGet Packages...**</span></span>
2. <span data-ttu-id="f4eb2-132">Haga clic en **Examinar** y busque **Connector.ASPNetCore**.</span><span class="sxs-lookup"><span data-stu-id="f4eb2-132">Click **Browse** and search for **Connector.ASPNetCore**.</span></span> 
3. <span data-ttu-id="f4eb2-133">Elija la versión que desee establecer como destino.</span><span class="sxs-lookup"><span data-stu-id="f4eb2-133">Choose the version you want to target.</span></span> <span data-ttu-id="f4eb2-134">Por ejemplo, la captura de pantalla siguiente muestra la versión **2.0.0.3** seleccionada.</span><span class="sxs-lookup"><span data-stu-id="f4eb2-134">For example, the screenshot below shows the version **2.0.0.3** selected.</span></span> <span data-ttu-id="f4eb2-135">Para instalar la versión 1.1.3.2, haga clic en la lista desplegable **Versión** y elija la versión **1.1.3.2** en su lugar.</span><span class="sxs-lookup"><span data-stu-id="f4eb2-135">To install version 1.1.3.2, click the **Version** drop-down and choose version **1.1.3.2** instead.</span></span>
<span data-ttu-id="f4eb2-136">![Paquete NuGet para ASP.NET Core versión 2.0.0.3](~/media/dotnet-core-authentication/nuget-package-net-core-version.png)</span><span class="sxs-lookup"><span data-stu-id="f4eb2-136">![NuGet Package for ASP Net Core version 2.0.0.3](~/media/dotnet-core-authentication/nuget-package-net-core-version.png)</span></span>
4. <span data-ttu-id="f4eb2-137">Haga clic en **Instalar**.</span><span class="sxs-lookup"><span data-stu-id="f4eb2-137">Click **Install**.</span></span>

## <a name="update-the-appsettingsjson"></a><span data-ttu-id="f4eb2-138">Actualización de appsettings.json</span><span class="sxs-lookup"><span data-stu-id="f4eb2-138">Update the appsettings.json</span></span>

<span data-ttu-id="f4eb2-139">El Bot Framework Connector requiere su AppID y contraseña para autenticar el bot.</span><span class="sxs-lookup"><span data-stu-id="f4eb2-139">The Bot Framework Connector requires your AppID and Password to authenticate the bot.</span></span> <span data-ttu-id="f4eb2-140">Puede establecer estos valores en el archivo **appsettings.json** de su proyecto de aplicación web.</span><span class="sxs-lookup"><span data-stu-id="f4eb2-140">You can set these values in the **appsettings.json** of your web app project.</span></span>

> [!NOTE]
> <span data-ttu-id="f4eb2-141">Para buscar **AppID** y **AppPassword** del bot, consulte [MicrosoftAppID y MicrosoftAppPassword](~/bot-service-manage-overview.md#microsoftappid-and-microsoftapppassword).</span><span class="sxs-lookup"><span data-stu-id="f4eb2-141">To find your bot's **AppID** and **AppPassword**, see [MicrosoftAppID and MicrosoftAppPassword](~/bot-service-manage-overview.md#microsoftappid-and-microsoftapppassword).</span></span>

### <a name="appsettingsjson-for-net-core-v11"></a><span data-ttu-id="f4eb2-142">appsettings.json para .NET Core v1.1:</span><span class="sxs-lookup"><span data-stu-id="f4eb2-142">appsettings.json for .NET Core v1.1:</span></span>

```json
{
  "Logging": {
    "IncludeScopes": false,
    "LogLevel": {
      "Default": "Warning"
    }
  },
  "MicrosoftAppId": "<MicrosoftAppId>",
  "MicrosoftAppPassword": "<MicrosoftAppPassword>"
}
```

### <a name="appsettingsjson-for-net-core-v20"></a><span data-ttu-id="f4eb2-143">appsettings.json para .NET Core v2.0:</span><span class="sxs-lookup"><span data-stu-id="f4eb2-143">appsettings.json for .NET Core v2.0:</span></span>

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
  "MicrosoftAppId": "<MicrosoftAppId>",
  "MicrosoftAppPassword": "<MicrosoftAppPassword>"
}
```

## <a name="update-the-startupcs-class"></a><span data-ttu-id="f4eb2-144">Actualización de la clase startup.cs</span><span class="sxs-lookup"><span data-stu-id="f4eb2-144">Update the startup.cs class</span></span>

<span data-ttu-id="f4eb2-145">Actualice la clase **startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="f4eb2-145">Update the **startup.cs** class.</span></span> <span data-ttu-id="f4eb2-146">Según la versión del proyecto, actualice el fragmento de código adecuado para permitir que la autenticación funcione.</span><span class="sxs-lookup"><span data-stu-id="f4eb2-146">Depending on your project version, update the appropriate code fragment to allow authentication to work.</span></span>

### <a name="startupcs-for-net-core-v11"></a><span data-ttu-id="f4eb2-147">startup.cs para .NET Core v1.1</span><span class="sxs-lookup"><span data-stu-id="f4eb2-147">startup.cs for .NET Core v1.1</span></span>

```cs
public class Startup
    {
        public Startup(IHostingEnvironment env)
        {
            var builder = new ConfigurationBuilder()
                .SetBasePath(env.ContentRootPath)
                .AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
                .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true)
                .AddEnvironmentVariables();
            Configuration = builder.Build();
        }

        public IConfigurationRoot Configuration { get; }

        // This method gets called by the runtime. Use this method to add services to the container.
        public void ConfigureServices(IServiceCollection services)
        {
            services.AddSingleton(_ => Configuration);

            services.AddSingleton(typeof(ICredentialProvider), new StaticCredentialProvider(
                Configuration.GetSection(MicrosoftAppCredentials.MicrosoftAppIdKey)?.Value,
                Configuration.GetSection(MicrosoftAppCredentials.MicrosoftAppPasswordKey)?.Value));

            // Add framework services.
            services.AddMvc(options =>
            {
                options.Filters.Add(typeof(TrustServiceUrlAttribute));
            });
        }

        // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
        public void Configure(IApplicationBuilder app, IServiceProvider serviceProvider, IHostingEnvironment env, ILoggerFactory loggerFactory)
        {
            loggerFactory.AddConsole(Configuration.GetSection("Logging"));
            loggerFactory.AddDebug();

            app.UseStaticFiles();

            ICredentialProvider credentialProvider = serviceProvider.GetService<ICredentialProvider>();

            app.UseBotAuthentication(credentialProvider);

            app.UseMvc();
        }
    }
```

### <a name="startupcs-for-net-core-v20"></a><span data-ttu-id="f4eb2-148">startup.cs para .NET Core v2.0:</span><span class="sxs-lookup"><span data-stu-id="f4eb2-148">startup.cs for .NET Core v2.0:</span></span>

```cs
public class Startup
    {
        public Startup(IHostingEnvironment env)
        {
            var builder = new ConfigurationBuilder()
                .SetBasePath(env.ContentRootPath)
                .AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
                .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true)
                .AddEnvironmentVariables();
            Configuration = builder.Build();
        }

        public IConfiguration Configuration { get; }
        // This method gets called by the runtime. Use this method to add services to the container.
        public void ConfigureServices(IServiceCollection services)
        {
            services.AddSingleton(_ => Configuration);

            var credentialProvider = new StaticCredentialProvider(Configuration.GetSection(MicrosoftAppCredentials.MicrosoftAppIdKey)?.Value,
                Configuration.GetSection(MicrosoftAppCredentials.MicrosoftAppPasswordKey)?.Value);

            services.AddAuthentication(
                    options =>
                    {
                        options.DefaultAuthenticateScheme = JwtBearerDefaults.AuthenticationScheme;
                        options.DefaultChallengeScheme = JwtBearerDefaults.AuthenticationScheme;
                    }
                )
                .AddBotAuthentication(credentialProvider);

            services.AddSingleton(typeof(ICredentialProvider), credentialProvider);

            services.AddMvc(options =>
            {
                options.Filters.Add(typeof(TrustServiceUrlAttribute));
            });
        }


        // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
        public void Configure(IApplicationBuilder app, IHostingEnvironment env)
        {
            app.UseStaticFiles();
            app.UseAuthentication();
            app.UseMvc();
        }
    }
```

## <a name="create-the-messagescontrollercs-class"></a><span data-ttu-id="f4eb2-149">Creación de la clase MessagesController.cs</span><span class="sxs-lookup"><span data-stu-id="f4eb2-149">Create the MessagesController.cs class</span></span>

<span data-ttu-id="f4eb2-150">Desde el **Explorador de soluciones** agregue una nueva clase vacía denominada **MessagesController.cs**.</span><span class="sxs-lookup"><span data-stu-id="f4eb2-150">From **Solution Explorer** add a new empty class called **MessagesController.cs**.</span></span> <span data-ttu-id="f4eb2-151">En la clase **MessagesController.cs**, actualice el método **Post** con el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="f4eb2-151">In the **MessagesController.cs** class, update the **Post** method with the code below.</span></span> <span data-ttu-id="f4eb2-152">Esto permitirá que el bot envíe y reciba mensajes para el usuario.</span><span class="sxs-lookup"><span data-stu-id="f4eb2-152">This will allow your bot to send and receive messages for the user.</span></span>

```cs
private IConfiguration configuration;

public MessagesController(IConfiguration configuration)
{
    this.configuration = configuration;
}


[Authorize(Roles = "Bot")]
[HttpPost]
public async Task<OkResult> Post([FromBody] Activity activity)
{
    if (activity.Type == ActivityTypes.Message)
    {
        //MicrosoftAppCredentials.TrustServiceUrl(activity.ServiceUrl);
        var appCredentials = new MicrosoftAppCredentials(configuration);
        var connector = new ConnectorClient(new Uri(activity.ServiceUrl), appCredentials);

        // return our reply to the user
        var reply = activity.CreateReply("HelloWorld");
        await connector.Conversations.ReplyToActivityAsync(reply);
    }
    else
    {
        //HandleSystemMessage(activity);
    }
    return Ok();
}
```
