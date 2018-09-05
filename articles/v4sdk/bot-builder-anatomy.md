---
title: Anatomía de un bot | Microsoft Docs
description: Desglose de las distintas partes de un bot y su funcionamiento
keywords: ''
author: ivorb
ms.author: ivorb
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 06/25/2018
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: 238be22eff746edff30a5446d20991dfaedc945a
ms.sourcegitcommit: 44f100a588ffda19c275b118f4f97029f12d1449
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/26/2018
ms.locfileid: "42928293"
---
# <a name="anatomy-of-a-bot"></a><span data-ttu-id="a07fd-103">Anatomía de un bot</span><span class="sxs-lookup"><span data-stu-id="a07fd-103">Anatomy of a bot</span></span>

[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]

<span data-ttu-id="a07fd-104">Un bot, en el [sentido más básico](bot-builder-basics.md), es una aplicación de conversación con la que los usuarios se comunican mediante mensajes.</span><span class="sxs-lookup"><span data-stu-id="a07fd-104">A bot, in the most [basic sense](bot-builder-basics.md), is a conversational app that users communicate with using messages.</span></span> <span data-ttu-id="a07fd-105">Sigue la estructura básica de una aplicación web en su lenguaje correspondiente y se crea sobre ella con el SDK de Bot Framework.</span><span class="sxs-lookup"><span data-stu-id="a07fd-105">It follows the basic structure of a web app in it's respective language, and builds on top of that with the Bot Framework SDK.</span></span>

<span data-ttu-id="a07fd-106">Un bot se compone de varios componentes que le permiten [enviar mensajes](./bot-builder-howto-send-messages.md) y realizar un seguimiento del [estado](./bot-builder-storage-concept.md).</span><span class="sxs-lookup"><span data-stu-id="a07fd-106">A bot is composed of several different components, allowing you to [send messages](./bot-builder-howto-send-messages.md) and keep track of [state](./bot-builder-storage-concept.md).</span></span> <span data-ttu-id="a07fd-107">Se puede comunicar con el bot a través del [emulador](../bot-service-debug-emulator.md), un [chat en web](../bot-service-manage-test-webchat.md) o, en producción, mediante uno de los [canales](bot-concepts.md).</span><span class="sxs-lookup"><span data-stu-id="a07fd-107">You can communicate with your bot through the [emulator](../bot-service-debug-emulator.md), [WebChat](../bot-service-manage-test-webchat.md), or in production through one of the [channels](bot-concepts.md).</span></span> 

<span data-ttu-id="a07fd-108">En este caso, trataremos un bot de eco básico paso a paso y examinaremos cada pieza relacionada con el funcionamiento.</span><span class="sxs-lookup"><span data-stu-id="a07fd-108">Here, we'll walk through a basic Echo Bot, step by step, and examine each piece that comes together to make it work.</span></span>

## <a name="files-created-for-echo-bot"></a><span data-ttu-id="a07fd-109">Archivos creados para el bot de eco</span><span class="sxs-lookup"><span data-stu-id="a07fd-109">Files created for Echo Bot</span></span>

<span data-ttu-id="a07fd-110">Cada lenguaje de programación crea archivos diferentes para el bot de eco, que en este ejemplo hemos llamado **BasicEcho** y estos archivos se dividen en tres secciones diferentes: la sección del sistema, los elementos auxiliares y el núcleo del bot.</span><span class="sxs-lookup"><span data-stu-id="a07fd-110">Each programming language creates different files for the Echo bot, which in this example we named **BasicEcho**, and these files fall into three different sections: the system section, the helper items and the core of the bot.</span></span> <span data-ttu-id="a07fd-111">En la tabla siguiente se enumeran los archivos principales para C# y JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a07fd-111">The table below lists the main files for both C# and JavaScript.</span></span>

| <span data-ttu-id="a07fd-112">C#</span><span class="sxs-lookup"><span data-stu-id="a07fd-112">C#</span></span> | <span data-ttu-id="a07fd-113">JavaScript</span><span class="sxs-lookup"><span data-stu-id="a07fd-113">JavaScript</span></span> |
| --- | --- |
| `Properties > launchSettings.json` <br> `wwwroot > default.htm` <br> `appsettings.json` <br> `BasicEcho.bot` <br> `EchoBot.cs` <br> `EchoState.cs` <br> `Program.cs` <br> `readme.md` <br> `Startup.cs` | `.env` <br> `app.js` <br> `package.json` <br> `README.md` <br> `basicEcho.bot` |

<span data-ttu-id="a07fd-114">A continuación veremos algunas características principales de estos archivos por secciones.</span><span class="sxs-lookup"><span data-stu-id="a07fd-114">Below we will walk you through some key features in these files by section.</span></span> <span data-ttu-id="a07fd-115">Algunos de estos archivos tienen su propio conjunto de inclusiones, que son tanto específicos del bot como bibliotecas de programación generales.</span><span class="sxs-lookup"><span data-stu-id="a07fd-115">Some of these files have its own set of includes, which are both bot specific and general programming libraries.</span></span> <span data-ttu-id="a07fd-116">Estos archivos de inclusión proporcionan un conjunto de funciones necesarias, así como algunas que es posible que desee incluir en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a07fd-116">These includes provide a set of functions you need, as well as some that you may want in your application.</span></span> <span data-ttu-id="a07fd-117">Aunque no analizaremos los detalles de estos archivos de inclusión, si tiene curiosidad, no dude en usar Visual Studio para ver las definiciones y de qué espacios de nombres forman parte.</span><span class="sxs-lookup"><span data-stu-id="a07fd-117">While we will not go over the details of these includes, feel free to use Visual Studio to peek the definitions and see which namespaces they are a part of if you are curious.</span></span>

## <a name="system-section"></a><span data-ttu-id="a07fd-118">Sección del sistema</span><span class="sxs-lookup"><span data-stu-id="a07fd-118">System section</span></span>

<span data-ttu-id="a07fd-119">La sección del sistema tiene lo que necesita para hacer que el bot funcione como una aplicación estándar.</span><span class="sxs-lookup"><span data-stu-id="a07fd-119">The system section has what you need to make your bot function as a standard application.</span></span> <span data-ttu-id="a07fd-120">Incluye los archivos de configuración, el adaptador y algunos archivos JSON.</span><span class="sxs-lookup"><span data-stu-id="a07fd-120">It includes the configuration files, the adapter, and some JSON files.</span></span> <span data-ttu-id="a07fd-121">Estos elementos se pueden (y, a menudo, se deben) mantener independientes, salvo algunos archivos de configuración que pueden necesitar su información específica.</span><span class="sxs-lookup"><span data-stu-id="a07fd-121">These items can (and often should) be left alone, except some configuration files that may need your specific information.</span></span>

# <a name="ctabcs"></a>[<span data-ttu-id="a07fd-122">C#</span><span class="sxs-lookup"><span data-stu-id="a07fd-122">C#</span></span>](#tab/cs)

<span data-ttu-id="a07fd-123">Un bot es un tipo de marco de trabajo de aplicación [web de ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/?view=aspnetcore-2.1).</span><span class="sxs-lookup"><span data-stu-id="a07fd-123">A bot is a type of [ASP.NET Core web](https://docs.microsoft.com/en-us/aspnet/core/?view=aspnetcore-2.1) application framework.</span></span> <span data-ttu-id="a07fd-124">Si consulta los [Conceptos básicos de ASP.NET](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/index?view=aspnetcore-2.1&tabs=aspnetcore2x), verá código similar en archivos como los appsettings.json, Program.cs y Startup.cs que se describen a continuación.</span><span class="sxs-lookup"><span data-stu-id="a07fd-124">If you look at the [ASP.NET fundamentals](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/index?view=aspnetcore-2.1&tabs=aspnetcore2x), you'll see similar code in files such as appsettings.json, Program.cs and Startup.cs discussed below.</span></span> <span data-ttu-id="a07fd-125">Estos archivos son necesarios para todas las aplicaciones web y no son específicos del bot.</span><span class="sxs-lookup"><span data-stu-id="a07fd-125">These files are required for all web apps and are not bot specific.</span></span> <span data-ttu-id="a07fd-126">El código de algunos de estos archivos no se copiará aquí, pero lo verá al ejecutar el bot.</span><span class="sxs-lookup"><span data-stu-id="a07fd-126">Code in some of these files won't be copied here, but you will see it when you run the bot.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="a07fd-127">appsettings.json</span><span class="sxs-lookup"><span data-stu-id="a07fd-127">appsettings.json</span></span>

<span data-ttu-id="a07fd-128">Este archivo contiene simplemente la información de configuración del bot, principalmente el identificador de aplicación y la contraseña, en formato JSON básico.</span><span class="sxs-lookup"><span data-stu-id="a07fd-128">This file simply contains the settings information for your bot, mainly the app ID and password, in a basic JSON format.</span></span>  <span data-ttu-id="a07fd-129">En las pruebas con el [emulador](../bot-service-debug-emulator.md), esta información no es necesaria y se puede dejar en blanco, pero es necesaria en producción.</span><span class="sxs-lookup"><span data-stu-id="a07fd-129">When testing with the [emulator](../bot-service-debug-emulator.md) these information aren't needed and can be left blank, but they are necessary in production.</span></span>

### <a name="programcs"></a><span data-ttu-id="a07fd-130">Program.cs</span><span class="sxs-lookup"><span data-stu-id="a07fd-130">Program.cs</span></span>

<span data-ttu-id="a07fd-131">Este archivo es necesario para que la aplicación web ASP.NET funcione y especifica la clase `Startup` para su ejecución.</span><span class="sxs-lookup"><span data-stu-id="a07fd-131">This file is necessary for your ASP.NET web app to work, and specifies the `Startup` class to run.</span></span>

### <a name="startupcs"></a><span data-ttu-id="a07fd-132">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="a07fd-132">Startup.cs</span></span>

<span data-ttu-id="a07fd-133">**Startup.cs** tiene otras partes interesantes que se tratarán más adelante, pero aquí veremos las secciones del sistema.</span><span class="sxs-lookup"><span data-stu-id="a07fd-133">**Startup.cs** has other interesting parts that will be covered later, but here we'll get the system sections of it out of the way.</span></span>

<span data-ttu-id="a07fd-134">El constructor de `Startup` especifica la configuración de la aplicación y las variables de entorno y crea la configuración de la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="a07fd-134">The constructor for `Startup` specifies the app settings and environment variables, and creates the configuration for your web app.</span></span>

<span data-ttu-id="a07fd-135">El método `Configure` finaliza la configuración de la aplicación, ya que especifica que la aplicación usa Bot Framework y algunos otros archivos.</span><span class="sxs-lookup"><span data-stu-id="a07fd-135">The `Configure` method finishes the configuration of your app by specifying that the app use the Bot Framework and a few other files.</span></span> <span data-ttu-id="a07fd-136">Todos los bots que utilizan Bot Framework necesitarán esa llamada de configuración.</span><span class="sxs-lookup"><span data-stu-id="a07fd-136">All bots using the Bot Framework will need that configuration call.</span></span>

```cs
using System;
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Bot.Builder.BotFramework;
using Microsoft.Bot.Builder.Core.Extensions;
using Microsoft.Bot.Builder.Integration.AspNet.Core;
using Microsoft.Bot.Builder.TraceExtensions;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;

namespace BasicEcho
{
    public class Startup
    {
        // This method gets called by the runtime. Use this method to add services to the container.
        // For more information on how to configure your application, visit https://go.microsoft.com/fwlink/?LinkID=398940
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

        // ...
        // Definition of ConfigureServices covered in the next section
        // ...

        // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
        public void Configure(IApplicationBuilder app, IHostingEnvironment env)
        {
            if (env.IsDevelopment())
            {
                app.UseDeveloperExceptionPage();
            }

            app.UseDefaultFiles()
                .UseStaticFiles()
                .UseBotFramework();
        }
    }
}

```

### <a name="launchsettingsjson-and-readmemd"></a><span data-ttu-id="a07fd-137">launchSettings.json y readme.md</span><span class="sxs-lookup"><span data-stu-id="a07fd-137">launchSettings.json and readme.md</span></span>

<span data-ttu-id="a07fd-138">**launchSettings.json** contiene simplemente alguna configuración para aplicaciones web y **readme.md** es una explicación simple de una sola línea sobre el bot de eco.</span><span class="sxs-lookup"><span data-stu-id="a07fd-138">**launchSettings.json** simply contains some set up configuration for web apps, and **readme.md** is a simple one line explanation about the echo bot.</span></span> <span data-ttu-id="a07fd-139">El contenido de estos archivos no es importante para comprender mejor este bot.</span><span class="sxs-lookup"><span data-stu-id="a07fd-139">The contents of these files aren't important for your understanding of this bot.</span></span> 

# <a name="javascripttabjs"></a>[<span data-ttu-id="a07fd-140">JavaScript</span><span class="sxs-lookup"><span data-stu-id="a07fd-140">JavaScript</span></span>](#tab/js)

<span data-ttu-id="a07fd-141">La sección del sistema contiene principalmente **package.json**, el archivo **.env**, **app.js** y **README.md**.</span><span class="sxs-lookup"><span data-stu-id="a07fd-141">The system section mainly contains **package.json**, the **.env** file, **app.js** and **README.md**.</span></span> <span data-ttu-id="a07fd-142">El código de algunos de estos archivos no se copiará aquí, pero lo verá al ejecutar el bot.</span><span class="sxs-lookup"><span data-stu-id="a07fd-142">Code in some files won't be copied here, but you will see it when you run the bot.</span></span>

### <a name="packagejson"></a><span data-ttu-id="a07fd-143">package.json</span><span class="sxs-lookup"><span data-stu-id="a07fd-143">package.json</span></span>

<span data-ttu-id="a07fd-144">**package.json** especifica las dependencias del bot y sus versiones asociadas.</span><span class="sxs-lookup"><span data-stu-id="a07fd-144">**package.json** specifies dependencies and their associated versions for your bot.</span></span> <span data-ttu-id="a07fd-145">Esto viene configurado por la plantilla y el sistema.</span><span class="sxs-lookup"><span data-stu-id="a07fd-145">This is all set up by the template and your system.</span></span>

### <a name="env-file"></a><span data-ttu-id="a07fd-146">Archivo .env</span><span class="sxs-lookup"><span data-stu-id="a07fd-146">.env file</span></span>

<span data-ttu-id="a07fd-147">El archivo **.env** especifica la información de configuración del bot, como el número de puerto, el identificador de aplicación y la contraseña, entre otras cosas.</span><span class="sxs-lookup"><span data-stu-id="a07fd-147">The **.env** file specifies the configuration information for your bot, such as the port number, app ID, and password among other things.</span></span> <span data-ttu-id="a07fd-148">Si utiliza determinadas tecnologías o si usa este bot en producción, deberá agregar las claves o dirección URL específicas a esta configuración.</span><span class="sxs-lookup"><span data-stu-id="a07fd-148">If using certain technologies or using this bot in production, you will need to add your specific keys or URL to this configuration.</span></span> <span data-ttu-id="a07fd-149">Para este bot de eco, no obstante, no necesita hacer nada aquí ahora mismo; el identificador de aplicación y la contraseña se pueden dejar sin definir en este momento.</span><span class="sxs-lookup"><span data-stu-id="a07fd-149">For this Echo bot, however, you don't need to do anything here right now; the app ID and password may be left undefined at this time.</span></span> 

<span data-ttu-id="a07fd-150">Para usar el archivo de configuración **.env** la plantilla necesita incluir un paquete adicional.</span><span class="sxs-lookup"><span data-stu-id="a07fd-150">To use the **.env** configuration file, the template needs an extra package included.</span></span>  <span data-ttu-id="a07fd-151">En primer lugar, obtenga el paquete `dotenv` de npm:</span><span class="sxs-lookup"><span data-stu-id="a07fd-151">First, get the `dotenv` package from npm:</span></span>

`npm install dotenv`

<span data-ttu-id="a07fd-152">A continuación, agregue la siguiente línea al bot con las otras bibliotecas necesarias:</span><span class="sxs-lookup"><span data-stu-id="a07fd-152">Then add the following line to your bot with the other required libraries:</span></span>

```javascript
const dotenv = require('dotenv');
```

### <a name="appjs"></a><span data-ttu-id="a07fd-153">app.js</span><span class="sxs-lookup"><span data-stu-id="a07fd-153">app.js</span></span>

<span data-ttu-id="a07fd-154">La parte superior del archivo `app.js` contiene el servidor y el adaptador que permiten al bot comunicarse con el usuario y enviar respuestas.</span><span class="sxs-lookup"><span data-stu-id="a07fd-154">The top of your `app.js` file has the server and adapter that allow your bot to communicate with the user and send responses.</span></span> <span data-ttu-id="a07fd-155">El servidor escuchará en el puerto especificado en el archivo de configuración **.env** o volverá al 3978 para la conexión con el emulador.</span><span class="sxs-lookup"><span data-stu-id="a07fd-155">The server will listen on the specified port from the **.env** configuration, or fall back to 3978 for connection with your emulator.</span></span> <span data-ttu-id="a07fd-156">El adaptador actúa como el director del bot, dirige la comunicación entrante y saliente y la autenticación, entre otras.</span><span class="sxs-lookup"><span data-stu-id="a07fd-156">The adapter will act as the conductor for your bot, directing incoming and outgoing communication, authentication, and so on.</span></span> 

```javascript
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
``` 

### <a name="readmemd"></a><span data-ttu-id="a07fd-157">README.md</span><span class="sxs-lookup"><span data-stu-id="a07fd-157">README.md</span></span>

<span data-ttu-id="a07fd-158">**README.md** proporciona información útil sobre algunos aspectos de la creación de un bot, como la estructura básica del bot o los *diálogos*, pero no es importante para comprender el bot.</span><span class="sxs-lookup"><span data-stu-id="a07fd-158">**README.md** provides some helpful information on some aspects of building a bot, such as basic bot structure or what *Dialogs* are, but isn't important for understanding your bot.</span></span>

---


## <a name="helper-items"></a><span data-ttu-id="a07fd-159">Elementos auxiliares</span><span class="sxs-lookup"><span data-stu-id="a07fd-159">Helper items</span></span>

<span data-ttu-id="a07fd-160">Como en cualquier otro programa, los métodos auxiliares y otras definiciones pueden simplificar nuestro código.</span><span class="sxs-lookup"><span data-stu-id="a07fd-160">Like any program, having helper methods and other definitions can simplify our code.</span></span> <span data-ttu-id="a07fd-161">Hay algunas partes del bot que pertenecen a esa categoría, como el archivo `.bot`, que son importantes para el bot pero no forman parte necesariamente de la lógica básica del bot.</span><span class="sxs-lookup"><span data-stu-id="a07fd-161">There are a few parts of our bot that fall into that category, such as the `.bot` file, that are important for our bot but not necessarily part of the core bot logic.</span></span>

### <a name="bot-file"></a><span data-ttu-id="a07fd-162">archivo .bot</span><span class="sxs-lookup"><span data-stu-id="a07fd-162">.bot file</span></span>

<span data-ttu-id="a07fd-163">El archivo `.bot` contiene información, como el punto de conexión, el identificador de aplicación y la contraseña, que permite a los [canales](bot-concepts.md) conectarse al bot.</span><span class="sxs-lookup"><span data-stu-id="a07fd-163">The `.bot` file contains information, including the endpoint, app ID, and password, that allows [channels](bot-concepts.md) to connect to your bot.</span></span> <span data-ttu-id="a07fd-164">Este archivo se crea automáticamente al iniciar la creación de un bot desde una plantilla, pero puede crear el suyo propio con el emulador u otras herramientas.</span><span class="sxs-lookup"><span data-stu-id="a07fd-164">This file gets created for you when you start building a bot from a template, but you can create your own through the emulator or other tools.</span></span>

<span data-ttu-id="a07fd-165">El contenido del archivo debe coincidir con lo que ve aquí a menos que asigne al bot un nombre distinto a *BasicEcho*.</span><span class="sxs-lookup"><span data-stu-id="a07fd-165">Your file content should match what you see here unless you name your bot something other than *BasicEcho*.</span></span> <span data-ttu-id="a07fd-166">Es posible que deba cambiar y actualizar esta información en función del uso del bot, pero para ejecutar este bot de eco no es necesario ningún cambio ahora.</span><span class="sxs-lookup"><span data-stu-id="a07fd-166">You may need to change and update this information depending on how your bot is used, but for running this Echo Bot no changes are needed right now.</span></span>

```json
{
  "name": "BasicEcho",
  "secretKey": "",
  "services": [
    {
      "appId": "",
      "id": "http://localhost:3978/api/messages",
      "type": "endpoint",
      "appPassword": "",
      "endpoint": "http://localhost:3978/api/messages",
      "name": "BasicEcho"
    }
  ]
}
```

# <a name="ctabcs"></a>[<span data-ttu-id="a07fd-167">C#</span><span class="sxs-lookup"><span data-stu-id="a07fd-167">C#</span></span>](#tab/cs)

### <a name="echostatecs"></a><span data-ttu-id="a07fd-168">EchoState.cs</span><span class="sxs-lookup"><span data-stu-id="a07fd-168">EchoState.cs</span></span>

<span data-ttu-id="a07fd-169">**EchoState.cs** contiene una clase simple que el bot utiliza para mantener el estado actual.</span><span class="sxs-lookup"><span data-stu-id="a07fd-169">**EchoState.cs** contains a simple class that our bot uses to maintain our current state.</span></span> <span data-ttu-id="a07fd-170">Contiene solo un valor `int` que se usa para incrementar el contador de turnos.</span><span class="sxs-lookup"><span data-stu-id="a07fd-170">It contains only an `int` that we use to increment your turn counter.</span></span> <span data-ttu-id="a07fd-171">Aunque trataremos al estado en la sección siguiente, por ahora solo necesita comprender que `EchoState` es la clase que contiene el contador de turnos.</span><span class="sxs-lookup"><span data-stu-id="a07fd-171">We'll get into state more in the next section, but for now you only need to understand that `EchoState` is our class containing the turn count.</span></span>

```cs
namespace BasicEcho
{
    /// <summary>
    /// Class for storing conversation state.
    /// </summary>
    public class EchoState
    {
        public int TurnCount { get; set; } = 0;
    }
}
```

### <a name="defaulthtm"></a><span data-ttu-id="a07fd-172">default.htm</span><span class="sxs-lookup"><span data-stu-id="a07fd-172">default.htm</span></span>

<span data-ttu-id="a07fd-173">**default.htm** es la página web que se muestra al ejecutar el bot, escrita en código `html`.</span><span class="sxs-lookup"><span data-stu-id="a07fd-173">**default.htm** is the webpage that is displayed when you run your bot, written in `html`.</span></span> <span data-ttu-id="a07fd-174">Aunque muestra información útil para conectarse al bot y cómo interactuar con él, el contenido de la página no afecta al comportamiento del bot.</span><span class="sxs-lookup"><span data-stu-id="a07fd-174">It displays helpful information for connecting to your bot and how to interact with it, however, the content of the page doesn't impact the behavior of your bot.</span></span> <span data-ttu-id="a07fd-175">Verá el código emergente al ejecutar el bot.</span><span class="sxs-lookup"><span data-stu-id="a07fd-175">You will see the code pop up when you run the bot.</span></span>

# <a name="javascripttabjs"></a>[<span data-ttu-id="a07fd-176">JavaScript</span><span class="sxs-lookup"><span data-stu-id="a07fd-176">JavaScript</span></span>](#tab/js)

### <a name="required-libraries"></a><span data-ttu-id="a07fd-177">Bibliotecas necesarias</span><span class="sxs-lookup"><span data-stu-id="a07fd-177">Required libraries</span></span>

<span data-ttu-id="a07fd-178">En la parte superior del archivo `app.js` encontrará una serie de módulos o bibliotecas que van a ser necesarios.</span><span class="sxs-lookup"><span data-stu-id="a07fd-178">At the very top of your `app.js` file you will find a series of modules or libraries that are being required.</span></span> <span data-ttu-id="a07fd-179">Estos módulos le darán acceso a un conjunto de funciones que tal vez desee incluir en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a07fd-179">These modules will give you access to a set of functions that you may want to include in your application.</span></span> 

```javascript
const { BotFrameworkAdapter, MemoryStorage, ConversationState } = require('botbuilder');
const restify = require('restify');
```

---

## <a name="core-of-the-bot"></a><span data-ttu-id="a07fd-180">Núcleo del bot</span><span class="sxs-lookup"><span data-stu-id="a07fd-180">Core of the bot</span></span>

<span data-ttu-id="a07fd-181">Por último, lo que interesa realmente!</span><span class="sxs-lookup"><span data-stu-id="a07fd-181">Finally, what you're really interested in!</span></span> <span data-ttu-id="a07fd-182">El núcleo del bot define cómo interactúa el bot con el usuario, incluidos el middleware y la lógica del bot.</span><span class="sxs-lookup"><span data-stu-id="a07fd-182">The core of the bot defines how the bot interacts with the user, which includes middleware and the bot logic.</span></span>

# <a name="ctabcs"></a>[<span data-ttu-id="a07fd-183">C#</span><span class="sxs-lookup"><span data-stu-id="a07fd-183">C#</span></span>](#tab/cs)

### <a name="middleware"></a><span data-ttu-id="a07fd-184">Software intermedio</span><span class="sxs-lookup"><span data-stu-id="a07fd-184">Middleware</span></span>

<span data-ttu-id="a07fd-185">En **Startup.cs** hay un método llamado `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a07fd-185">Within **Startup.cs**, there is one method called `ConfigureServices`.</span></span> <span data-ttu-id="a07fd-186">Este método configura el proveedor de credenciales y agrega el middleware.</span><span class="sxs-lookup"><span data-stu-id="a07fd-186">This method sets up your credential provider and adds  middleware.</span></span>

<span data-ttu-id="a07fd-187">El [middleware](bot-builder-concept-middleware.md) que se agrega aquí hace dos cosas.</span><span class="sxs-lookup"><span data-stu-id="a07fd-187">[Middleware](bot-builder-concept-middleware.md) added here does two things.</span></span> <span data-ttu-id="a07fd-188">La primera, el control de excepciones, que permite al bot controlar el estado de error.</span><span class="sxs-lookup"><span data-stu-id="a07fd-188">The first being exception handling which allows your bot to fail gracefully.</span></span> <span data-ttu-id="a07fd-189">Se define insertado como una expresión lambda, simplemente imprime la excepción en el terminal e indica al usuario que algo ha ido mal.</span><span class="sxs-lookup"><span data-stu-id="a07fd-189">It's defined inline as a lambda expression, simply printing out the exception to the terminal and telling the user something went wrong.</span></span>

<span data-ttu-id="a07fd-190">El segundo middleware mantiene el estado mediante el uso de `MemoryStorage`, que se definió inmediatamente antes.</span><span class="sxs-lookup"><span data-stu-id="a07fd-190">The second middleware maintains your state, using `MemoryStorage` that was defined right before it.</span></span> <span data-ttu-id="a07fd-191">Este estado se define como `ConversationState`, lo que significa simplemente que mantiene el estado de la conversación.</span><span class="sxs-lookup"><span data-stu-id="a07fd-191">This state is defined as `ConversationState`, which just means it's keeping the state of your conversation.</span></span> <span data-ttu-id="a07fd-192">Utiliza `EchoState`, la clase que hemos definido con el contador de turnos, para almacenar la información que le interesa.</span><span class="sxs-lookup"><span data-stu-id="a07fd-192">It uses `EchoState`, the class we've defined with your turn counter, to store the information you're interested in.</span></span>

``` cs
// This method gets called by the runtime. Use this method to add services to the container.
public void ConfigureServices(IServiceCollection services)
{
    services.AddBot<EchoBot>(options =>
    {
        options.CredentialProvider = new ConfigurationCredentialProvider(Configuration);

        // The CatchExceptionMiddleware provides a top-level exception handler for your bot.
        // Any exceptions thrown by other Middleware, or by your OnTurn method, will be 
        // caught here. To facilitate debugging, the exception is sent out, via Trace, 
        // to the emulator. Trace activities are NOT displayed to users, so in addition
        // an "Ooops" message is sent.
        options.Middleware.Add(new CatchExceptionMiddleware<Exception>(async (context, exception) =>
        {
            await context.TraceActivity("EchoBot Exception", exception);
            await context.SendActivity("Sorry, it looks like something went wrong!");
        }));

        // The Memory Storage used here is for local bot debugging only. When the bot
        // is restarted, anything stored in memory will be gone. 
        IStorage dataStore = new MemoryStorage();

        // The File data store, shown here, is suitable for bots that run on 
        // a single machine and need durable state across application restarts.

        // IStorage dataStore = new FileStorage(System.IO.Path.GetTempPath());

        // For production bots use the Azure Table Store, Azure Blob, or 
        // Azure CosmosDB storage provides, as seen below. To include any of 
        // the Azure based storage providers, add the Microsoft.Bot.Builder.Azure 
        // Nuget package to your solution. That package is found at:
        //      https://www.nuget.org/packages/Microsoft.Bot.Builder.Azure/

        // IStorage dataStore = new Microsoft.Bot.Builder.Azure.AzureTableStorage("AzureTablesConnectionString", "TableName");
        // IStorage dataStore = new Microsoft.Bot.Builder.Azure.AzureBlobStorage("AzureBlobConnectionString", "containerName");

        options.Middleware.Add(new ConversationState<EchoState>(dataStore));
    });
}
```

### <a name="bot-logic"></a><span data-ttu-id="a07fd-193">Lógica del bot</span><span class="sxs-lookup"><span data-stu-id="a07fd-193">Bot logic</span></span>

<span data-ttu-id="a07fd-194">La lógica principal del bot se define en el controlador `OnTurn` del bot.</span><span class="sxs-lookup"><span data-stu-id="a07fd-194">The main bot logic is defined within the bot's `OnTurn` handler.</span></span> <span data-ttu-id="a07fd-195">Se proporciona una variable de [contexto](./bot-builder-concept-activity-processing.md#turn-context) a `OnTurn` como argumento, con información sobre la [actividad](bot-builder-concept-activity-processing.md) entrante y la conversación, entre otros.</span><span class="sxs-lookup"><span data-stu-id="a07fd-195">`OnTurn` is provided a [context](./bot-builder-concept-activity-processing.md#turn-context) variable as an argument, which provides information about the incoming [activity](bot-builder-concept-activity-processing.md), the conversation, etc.</span></span>

<span data-ttu-id="a07fd-196">Las actividades entrantes pueden ser de [diversos tipos](../bot-service-activities-entities.md#activity-types), por lo que primero comprobamos si el bot ha recibido un mensaje.</span><span class="sxs-lookup"><span data-stu-id="a07fd-196">Incoming activities can be of [various types](../bot-service-activities-entities.md#activity-types), so we first check to see if your bot has received a message.</span></span> <span data-ttu-id="a07fd-197">Si es un mensaje, creamos una variable de estado que contiene la información actual de la conversación del bot.</span><span class="sxs-lookup"><span data-stu-id="a07fd-197">If it is a message, we create a state variable that holds the current information of your bot's conversation.</span></span> <span data-ttu-id="a07fd-198">A continuación, incrementamos el valor de `int`, que lleva el contador de turnos, antes de devolver al usuario el recuento junto con el mensaje enviado.</span><span class="sxs-lookup"><span data-stu-id="a07fd-198">We then increment the `int` that keeps track of your turn count before echoing back to the user the count along with the message they sent.</span></span>

```cs
using System.Threading.Tasks;
using Microsoft.Bot;
using Microsoft.Bot.Builder;
using Microsoft.Bot.Builder.Core.Extensions;
using Microsoft.Bot.Schema;

namespace BasicEcho
{
    public class EchoBot : IBot
    {
        /// <summary>
        /// Every Conversation turn for our EchoBot will call this method. In here
        /// the bot checks the Activty type to verify it's a message, bumps the 
        /// turn conversation 'Turn' count, and then echoes the users typing
        /// back to them. 
        /// </summary>
        /// <param name="context">Turn scoped context containing all the data needed
        /// for processing this conversation turn. </param>        
        public async Task OnTurn(ITurnContext context)
        {
            // This bot is only handling Messages
            if (context.Activity.Type == ActivityTypes.Message)
            {
                // Get the conversation state from the turn context
                var state = context.GetConversationState<EchoState>();

                // Bump the turn count. 
                state.TurnCount++;

                // Echo back to the user whatever they typed.
                await context.SendActivity($"Turn {state.TurnCount}: You sent '{context.Activity.Text}'");
            }
        }
    }    
}
```

# <a name="javascripttabjs"></a>[<span data-ttu-id="a07fd-199">JavaScript</span><span class="sxs-lookup"><span data-stu-id="a07fd-199">JavaScript</span></span>](#tab/js)

### <a name="middleware"></a><span data-ttu-id="a07fd-200">Software intermedio</span><span class="sxs-lookup"><span data-stu-id="a07fd-200">Middleware</span></span>

<span data-ttu-id="a07fd-201">En **app.js**, usamos el [middleware](bot-builder-concept-middleware.md) del bot para mantener el estado, mediante el uso de `MemoryStorage`, que se definió como uno de los módulos necesarios en la parte superior de `botbuilder`.</span><span class="sxs-lookup"><span data-stu-id="a07fd-201">Within **app.js**, we use [middleware](bot-builder-concept-middleware.md) in this bot to maintain your state, using `MemoryStorage` that was defined as one of our required modules at the top from `botbuilder`.</span></span> <span data-ttu-id="a07fd-202">Este estado se define como `ConversationState`, lo que significa simplemente que mantiene el estado de la conversación.</span><span class="sxs-lookup"><span data-stu-id="a07fd-202">This state is defined as `ConversationState`, which just means it's keeping the state of your conversation.</span></span> <span data-ttu-id="a07fd-203">`ConversationState` almacenará en memoria la información que le interesa, que en este caso es simplemente un contador de turnos.</span><span class="sxs-lookup"><span data-stu-id="a07fd-203">`ConversationState` will store the information you're interested in, which in this case is simply a turn counter, in memory.</span></span>

```javascript
// Add conversation state middleware
const conversationState = new ConversationState(new MemoryStorage());
adapter.use(conversationState);
```

### <a name="bot-logic"></a><span data-ttu-id="a07fd-204">Lógica del bot</span><span class="sxs-lookup"><span data-stu-id="a07fd-204">Bot Logic</span></span>

<span data-ttu-id="a07fd-205">El tercer parámetro en *processActivity* es un controlador de función al que se llamará para realizar la lógica del bot después de que la [actividad](bot-builder-concept-activity-processing.md) recibida haya sido previamente procesada por el adaptador y enrutada a algún middleware.</span><span class="sxs-lookup"><span data-stu-id="a07fd-205">The third parameter within *processActivity* is a function handler that will be called to perform the bot’s logic after the received [activity](bot-builder-concept-activity-processing.md) has been pre-processed by the adapter and routed through any middleware.</span></span> <span data-ttu-id="a07fd-206">La variable de [contexto](./bot-builder-concept-activity-processing.md#turn-context), que se pasa como un argumento al controlador de función, se puede usar para proporcionar información sobre la actividad entrante, el remitente y el receptor, el canal o la conversación, entre otros.</span><span class="sxs-lookup"><span data-stu-id="a07fd-206">The [context](./bot-builder-concept-activity-processing.md#turn-context) variable, passed as an argument to the function handler, can be used to provide information about the incoming activity, the sender and receiver, the channel, the conversation, etc.</span></span>

<span data-ttu-id="a07fd-207">En primer lugar, se comprueba si el bot ha recibido un mensaje.</span><span class="sxs-lookup"><span data-stu-id="a07fd-207">We first check to see if the bot has received a message.</span></span> <span data-ttu-id="a07fd-208">Si el bot no ha recibido un mensaje, se devuelve el tipo de actividad recibida.</span><span class="sxs-lookup"><span data-stu-id="a07fd-208">If the bot did not receive a message, we will echo back the activity type received.</span></span> <span data-ttu-id="a07fd-209">A continuación, creamos una variable de estado que contiene la información de la conversación del bot.</span><span class="sxs-lookup"><span data-stu-id="a07fd-209">Next, we create a state variable that holds the information of your bot's conversation.</span></span> <span data-ttu-id="a07fd-210">A continuación, establecemos la variable del contador en 0 si no está definida (lo que se producirá cuando se inicia el bot) o la aumentamos con cada nuevo mensaje.</span><span class="sxs-lookup"><span data-stu-id="a07fd-210">We then set your count variable to 0 if undefined (which will occur when you start the bot) or increment it with every new message.</span></span> <span data-ttu-id="a07fd-211">Por último, devolvemos al usuario el contador junto con el mensaje que envió.</span><span class="sxs-lookup"><span data-stu-id="a07fd-211">Finally, we echo back to the user the count along with the message they sent.</span></span>

```javascript
// Listen for incoming requests 
server.post('/api/messages', (req, res) => {
    // Route received request to adapter for processing
    adapter.processActivity(req, res, (context) => {
        // This bot is only handling Messages
        if (context.activity.type === 'message') {

            // Get the conversation state
            const state = conversationState.get(context);

            // If state.count is undefined set it to 0, otherwise increment it by 1
            const count = state.count === undefined ? state.count = 0 : ++state.count;

            // Echo back to the user whatever they typed.
            return context.sendActivity(`${count}: You said "${context.activity.text}"`);
        } else {
            // Echo back the type of activity the bot detected if not of type message
            return context.sendActivity(`[${context.activity.type} event detected]`);
        }
    });
});
```

---
