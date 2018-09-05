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
# <a name="anatomy-of-a-bot"></a>Anatomía de un bot

[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]

Un bot, en el [sentido más básico](bot-builder-basics.md), es una aplicación de conversación con la que los usuarios se comunican mediante mensajes. Sigue la estructura básica de una aplicación web en su lenguaje correspondiente y se crea sobre ella con el SDK de Bot Framework.

Un bot se compone de varios componentes que le permiten [enviar mensajes](./bot-builder-howto-send-messages.md) y realizar un seguimiento del [estado](./bot-builder-storage-concept.md). Se puede comunicar con el bot a través del [emulador](../bot-service-debug-emulator.md), un [chat en web](../bot-service-manage-test-webchat.md) o, en producción, mediante uno de los [canales](bot-concepts.md). 

En este caso, trataremos un bot de eco básico paso a paso y examinaremos cada pieza relacionada con el funcionamiento.

## <a name="files-created-for-echo-bot"></a>Archivos creados para el bot de eco

Cada lenguaje de programación crea archivos diferentes para el bot de eco, que en este ejemplo hemos llamado **BasicEcho** y estos archivos se dividen en tres secciones diferentes: la sección del sistema, los elementos auxiliares y el núcleo del bot. En la tabla siguiente se enumeran los archivos principales para C# y JavaScript.

| C# | JavaScript |
| --- | --- |
| `Properties > launchSettings.json` <br> `wwwroot > default.htm` <br> `appsettings.json` <br> `BasicEcho.bot` <br> `EchoBot.cs` <br> `EchoState.cs` <br> `Program.cs` <br> `readme.md` <br> `Startup.cs` | `.env` <br> `app.js` <br> `package.json` <br> `README.md` <br> `basicEcho.bot` |

A continuación veremos algunas características principales de estos archivos por secciones. Algunos de estos archivos tienen su propio conjunto de inclusiones, que son tanto específicos del bot como bibliotecas de programación generales. Estos archivos de inclusión proporcionan un conjunto de funciones necesarias, así como algunas que es posible que desee incluir en la aplicación. Aunque no analizaremos los detalles de estos archivos de inclusión, si tiene curiosidad, no dude en usar Visual Studio para ver las definiciones y de qué espacios de nombres forman parte.

## <a name="system-section"></a>Sección del sistema

La sección del sistema tiene lo que necesita para hacer que el bot funcione como una aplicación estándar. Incluye los archivos de configuración, el adaptador y algunos archivos JSON. Estos elementos se pueden (y, a menudo, se deben) mantener independientes, salvo algunos archivos de configuración que pueden necesitar su información específica.

# <a name="ctabcs"></a>[C#](#tab/cs)

Un bot es un tipo de marco de trabajo de aplicación [web de ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/?view=aspnetcore-2.1). Si consulta los [Conceptos básicos de ASP.NET](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/index?view=aspnetcore-2.1&tabs=aspnetcore2x), verá código similar en archivos como los appsettings.json, Program.cs y Startup.cs que se describen a continuación. Estos archivos son necesarios para todas las aplicaciones web y no son específicos del bot. El código de algunos de estos archivos no se copiará aquí, pero lo verá al ejecutar el bot.

### <a name="appsettingsjson"></a>appsettings.json

Este archivo contiene simplemente la información de configuración del bot, principalmente el identificador de aplicación y la contraseña, en formato JSON básico.  En las pruebas con el [emulador](../bot-service-debug-emulator.md), esta información no es necesaria y se puede dejar en blanco, pero es necesaria en producción.

### <a name="programcs"></a>Program.cs

Este archivo es necesario para que la aplicación web ASP.NET funcione y especifica la clase `Startup` para su ejecución.

### <a name="startupcs"></a>Startup.cs

**Startup.cs** tiene otras partes interesantes que se tratarán más adelante, pero aquí veremos las secciones del sistema.

El constructor de `Startup` especifica la configuración de la aplicación y las variables de entorno y crea la configuración de la aplicación web.

El método `Configure` finaliza la configuración de la aplicación, ya que especifica que la aplicación usa Bot Framework y algunos otros archivos. Todos los bots que utilizan Bot Framework necesitarán esa llamada de configuración.

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

### <a name="launchsettingsjson-and-readmemd"></a>launchSettings.json y readme.md

**launchSettings.json** contiene simplemente alguna configuración para aplicaciones web y **readme.md** es una explicación simple de una sola línea sobre el bot de eco. El contenido de estos archivos no es importante para comprender mejor este bot. 

# <a name="javascripttabjs"></a>[JavaScript](#tab/js)

La sección del sistema contiene principalmente **package.json**, el archivo **.env**, **app.js** y **README.md**. El código de algunos de estos archivos no se copiará aquí, pero lo verá al ejecutar el bot.

### <a name="packagejson"></a>package.json

**package.json** especifica las dependencias del bot y sus versiones asociadas. Esto viene configurado por la plantilla y el sistema.

### <a name="env-file"></a>Archivo .env

El archivo **.env** especifica la información de configuración del bot, como el número de puerto, el identificador de aplicación y la contraseña, entre otras cosas. Si utiliza determinadas tecnologías o si usa este bot en producción, deberá agregar las claves o dirección URL específicas a esta configuración. Para este bot de eco, no obstante, no necesita hacer nada aquí ahora mismo; el identificador de aplicación y la contraseña se pueden dejar sin definir en este momento. 

Para usar el archivo de configuración **.env** la plantilla necesita incluir un paquete adicional.  En primer lugar, obtenga el paquete `dotenv` de npm:

`npm install dotenv`

A continuación, agregue la siguiente línea al bot con las otras bibliotecas necesarias:

```javascript
const dotenv = require('dotenv');
```

### <a name="appjs"></a>app.js

La parte superior del archivo `app.js` contiene el servidor y el adaptador que permiten al bot comunicarse con el usuario y enviar respuestas. El servidor escuchará en el puerto especificado en el archivo de configuración **.env** o volverá al 3978 para la conexión con el emulador. El adaptador actúa como el director del bot, dirige la comunicación entrante y saliente y la autenticación, entre otras. 

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

### <a name="readmemd"></a>README.md

**README.md** proporciona información útil sobre algunos aspectos de la creación de un bot, como la estructura básica del bot o los *diálogos*, pero no es importante para comprender el bot.

---


## <a name="helper-items"></a>Elementos auxiliares

Como en cualquier otro programa, los métodos auxiliares y otras definiciones pueden simplificar nuestro código. Hay algunas partes del bot que pertenecen a esa categoría, como el archivo `.bot`, que son importantes para el bot pero no forman parte necesariamente de la lógica básica del bot.

### <a name="bot-file"></a>archivo .bot

El archivo `.bot` contiene información, como el punto de conexión, el identificador de aplicación y la contraseña, que permite a los [canales](bot-concepts.md) conectarse al bot. Este archivo se crea automáticamente al iniciar la creación de un bot desde una plantilla, pero puede crear el suyo propio con el emulador u otras herramientas.

El contenido del archivo debe coincidir con lo que ve aquí a menos que asigne al bot un nombre distinto a *BasicEcho*. Es posible que deba cambiar y actualizar esta información en función del uso del bot, pero para ejecutar este bot de eco no es necesario ningún cambio ahora.

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

# <a name="ctabcs"></a>[C#](#tab/cs)

### <a name="echostatecs"></a>EchoState.cs

**EchoState.cs** contiene una clase simple que el bot utiliza para mantener el estado actual. Contiene solo un valor `int` que se usa para incrementar el contador de turnos. Aunque trataremos al estado en la sección siguiente, por ahora solo necesita comprender que `EchoState` es la clase que contiene el contador de turnos.

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

### <a name="defaulthtm"></a>default.htm

**default.htm** es la página web que se muestra al ejecutar el bot, escrita en código `html`. Aunque muestra información útil para conectarse al bot y cómo interactuar con él, el contenido de la página no afecta al comportamiento del bot. Verá el código emergente al ejecutar el bot.

# <a name="javascripttabjs"></a>[JavaScript](#tab/js)

### <a name="required-libraries"></a>Bibliotecas necesarias

En la parte superior del archivo `app.js` encontrará una serie de módulos o bibliotecas que van a ser necesarios. Estos módulos le darán acceso a un conjunto de funciones que tal vez desee incluir en la aplicación. 

```javascript
const { BotFrameworkAdapter, MemoryStorage, ConversationState } = require('botbuilder');
const restify = require('restify');
```

---

## <a name="core-of-the-bot"></a>Núcleo del bot

Por último, lo que interesa realmente! El núcleo del bot define cómo interactúa el bot con el usuario, incluidos el middleware y la lógica del bot.

# <a name="ctabcs"></a>[C#](#tab/cs)

### <a name="middleware"></a>Software intermedio

En **Startup.cs** hay un método llamado `ConfigureServices`. Este método configura el proveedor de credenciales y agrega el middleware.

El [middleware](bot-builder-concept-middleware.md) que se agrega aquí hace dos cosas. La primera, el control de excepciones, que permite al bot controlar el estado de error. Se define insertado como una expresión lambda, simplemente imprime la excepción en el terminal e indica al usuario que algo ha ido mal.

El segundo middleware mantiene el estado mediante el uso de `MemoryStorage`, que se definió inmediatamente antes. Este estado se define como `ConversationState`, lo que significa simplemente que mantiene el estado de la conversación. Utiliza `EchoState`, la clase que hemos definido con el contador de turnos, para almacenar la información que le interesa.

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

### <a name="bot-logic"></a>Lógica del bot

La lógica principal del bot se define en el controlador `OnTurn` del bot. Se proporciona una variable de [contexto](./bot-builder-concept-activity-processing.md#turn-context) a `OnTurn` como argumento, con información sobre la [actividad](bot-builder-concept-activity-processing.md) entrante y la conversación, entre otros.

Las actividades entrantes pueden ser de [diversos tipos](../bot-service-activities-entities.md#activity-types), por lo que primero comprobamos si el bot ha recibido un mensaje. Si es un mensaje, creamos una variable de estado que contiene la información actual de la conversación del bot. A continuación, incrementamos el valor de `int`, que lleva el contador de turnos, antes de devolver al usuario el recuento junto con el mensaje enviado.

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

# <a name="javascripttabjs"></a>[JavaScript](#tab/js)

### <a name="middleware"></a>Software intermedio

En **app.js**, usamos el [middleware](bot-builder-concept-middleware.md) del bot para mantener el estado, mediante el uso de `MemoryStorage`, que se definió como uno de los módulos necesarios en la parte superior de `botbuilder`. Este estado se define como `ConversationState`, lo que significa simplemente que mantiene el estado de la conversación. `ConversationState` almacenará en memoria la información que le interesa, que en este caso es simplemente un contador de turnos.

```javascript
// Add conversation state middleware
const conversationState = new ConversationState(new MemoryStorage());
adapter.use(conversationState);
```

### <a name="bot-logic"></a>Lógica del bot

El tercer parámetro en *processActivity* es un controlador de función al que se llamará para realizar la lógica del bot después de que la [actividad](bot-builder-concept-activity-processing.md) recibida haya sido previamente procesada por el adaptador y enrutada a algún middleware. La variable de [contexto](./bot-builder-concept-activity-processing.md#turn-context), que se pasa como un argumento al controlador de función, se puede usar para proporcionar información sobre la actividad entrante, el remitente y el receptor, el canal o la conversación, entre otros.

En primer lugar, se comprueba si el bot ha recibido un mensaje. Si el bot no ha recibido un mensaje, se devuelve el tipo de actividad recibida. A continuación, creamos una variable de estado que contiene la información de la conversación del bot. A continuación, establecemos la variable del contador en 0 si no está definida (lo que se producirá cuando se inicia el bot) o la aumentamos con cada nuevo mensaje. Por último, devolvemos al usuario el contador junto con el mensaje que envió.

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
