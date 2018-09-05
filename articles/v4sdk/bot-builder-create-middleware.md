---
title: Creación de middleware propio | Microsoft Docs
description: Aprenda a escribir su propio software intermedio.
keywords: software intermedio, software intermedio personalizado, cortocircuito, reserva, controladores de eventos
author: ivorb
ms.author: v-ivorb
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 03/21/2018
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: b774f2de5856e6001d1b75c47b92aff6399d8fe3
ms.sourcegitcommit: 2dc75701b169d822c9499e393439161bc87639d2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42904355"
---
# <a name="create-your-own-middleware"></a><span data-ttu-id="f4c39-104">Creación de middleware propio</span><span class="sxs-lookup"><span data-stu-id="f4c39-104">Create your own middleware</span></span>

[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]

<span data-ttu-id="f4c39-105">El software intermedio le permite escribir complementos enriquecidos para los bots, que otros también pueden utilizar.</span><span class="sxs-lookup"><span data-stu-id="f4c39-105">Middleware allows you to write rich plugins for your bots, that can then be used by others as well.</span></span> <span data-ttu-id="f4c39-106">Aquí se muestra cómo agregar e implementar software intermedio básico y, además, se explica cómo funciona.</span><span class="sxs-lookup"><span data-stu-id="f4c39-106">Here we'll show how to add and implement basic middleware, and show how it works.</span></span> <span data-ttu-id="f4c39-107">El SDK v4 le proporciona algún software intermedio, para tareas como la administración de estados, LUIS, QnAMaker y traducción.</span><span class="sxs-lookup"><span data-stu-id="f4c39-107">The v4 SDK provides some middleware for you, for things such as state management, LUIS, QnAMaker, and translation.</span></span> <span data-ttu-id="f4c39-108">Eche un vistazo a Bot Builder SDK para [.NET](https://github.com/Microsoft/botbuilder-dotnet) o [JavaScript](https://github.com/Microsoft/botbuilder-js) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="f4c39-108">Take a look at the bot builder SDK for [.NET](https://github.com/Microsoft/botbuilder-dotnet) or [JavaScript](https://github.com/Microsoft/botbuilder-js) for more information.</span></span>

## <a name="adding-middleware"></a><span data-ttu-id="f4c39-109">Adición de software intermedio</span><span class="sxs-lookup"><span data-stu-id="f4c39-109">Adding middleware</span></span>

<span data-ttu-id="f4c39-110">En el ejemplo siguiente, basado en el ejemplo de bot básico creado en la experiencia [Creación de un bot con Bot Service](~/bot-service-quickstart.md), se agregan dos fragmentos distintos de middleware a nuestros servicios con una instancia nueva de cada una de estas clases.</span><span class="sxs-lookup"><span data-stu-id="f4c39-110">In the example below, based on our basic bot sample created through the [Get Started](~/bot-service-quickstart.md) experience, two different pieces of middleware are added to our services with a new instance of each of those classes.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f4c39-111">Recuerde que el orden en que se agregan a las opciones determina el orden en que se ejecutan.</span><span class="sxs-lookup"><span data-stu-id="f4c39-111">Remember, the order in which they are added to the options determines the order in which they are executed.</span></span> <span data-ttu-id="f4c39-112">Debe tener en cuenta cómo funcionará si usa más de un fragmento del software intermedio.</span><span class="sxs-lookup"><span data-stu-id="f4c39-112">Be sure to consider how that will work if using more than one piece of middleware.</span></span>

<span data-ttu-id="f4c39-113">**Startup.cs**</span><span class="sxs-lookup"><span data-stu-id="f4c39-113">**Startup.cs**</span></span>

# <a name="ctabcsaddmiddleware"></a>[<span data-ttu-id="f4c39-114">C#</span><span class="sxs-lookup"><span data-stu-id="f4c39-114">C#</span></span>](#tab/csaddmiddleware)

<span data-ttu-id="f4c39-115">Agregue llamadas del método `options.Middleware.Add(new MyMiddleware());` a las opciones del servicio de bot para cada fragmento del software intermedio que desee agregar.</span><span class="sxs-lookup"><span data-stu-id="f4c39-115">Add `options.Middleware.Add(new MyMiddleware());` method calls to your bot service options for each piece of middleware you want to add.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton(_ => Configuration);
    services.AddBot<HelloBot>(options =>
    {
        options.CredentialProvider = new ConfigurationCredentialProvider(Configuration);
        options.Middleware.Add(new MyMiddleware());
        options.Middleware.Add(new MyOtherMiddleware());
    });
}
```
# <a name="javascripttabjsaddmiddleware"></a>[<span data-ttu-id="f4c39-116">JavaScript</span><span class="sxs-lookup"><span data-stu-id="f4c39-116">JavaScript</span></span>](#tab/jsaddmiddleware)

<span data-ttu-id="f4c39-117">Agregue `adapter.use(MyMiddleware());` a su adaptador para cada pieza de software intermedio que desee agregar.</span><span class="sxs-lookup"><span data-stu-id="f4c39-117">Add `adapter.use(MyMiddleware());` to your adapter for each piece of middleware you want to add.</span></span>

```javascript
// Create adapter
const adapter = new botbuilder.BotFrameworkAdapter({
    appId: process.env.MICROSOFT_APP_ID, 
    appPassword: process.env.MICROSOFT_APP_PASSWORD 
});

adapter.use(MyMiddleware());
adapter.use(MyOtherMiddleware());
```

---


## <a name="implementing-your-middleware"></a><span data-ttu-id="f4c39-118">Implementación del software intermedio</span><span class="sxs-lookup"><span data-stu-id="f4c39-118">Implementing your Middleware</span></span>

<span data-ttu-id="f4c39-119">Cada fragmento del software intermedio se hereda de una interfaz de software intermedio y siempre implementa su controlador de procesamiento, que se ejecuta en cada actividad que se envía al bot.</span><span class="sxs-lookup"><span data-stu-id="f4c39-119">Each piece of middleware inherits from a middlware interface, and always implements it's processing handler, which is run on every activity that gets sent to your bot.</span></span> <span data-ttu-id="f4c39-120">Para cada fragmento de software intermedio agregado, el controlador de procesamiento tiene la oportunidad de modificar el objeto de contexto o de realizar una tarea, como los registros, antes de permitir que otro software intermedio o la lógica del bot interactúen con el objeto de contexto a medida que continúa en la canalización.</span><span class="sxs-lookup"><span data-stu-id="f4c39-120">For each piece of middleware added, the processing handler gets a chance to modify the context object or perform a task, such as logging, before allowing other middleware or bot logic to interact with the context object as it continues down the pipeline.</span></span>

# <a name="ctabcsetagoverwrite"></a>[<span data-ttu-id="f4c39-121">C#</span><span class="sxs-lookup"><span data-stu-id="f4c39-121">C#</span></span>](#tab/csetagoverwrite)

<span data-ttu-id="f4c39-122">Cada fragmento de software intermedio se hereda de `IMiddleware` y siempre implementa `OnTurn()`.</span><span class="sxs-lookup"><span data-stu-id="f4c39-122">Each piece of middleware inherits from `IMiddleware` and always implements `OnTurn()`.</span></span>

<span data-ttu-id="f4c39-123">**ExampleMiddleware.cs**</span><span class="sxs-lookup"><span data-stu-id="f4c39-123">**ExampleMiddleware.cs**</span></span>
```csharp
public class MyMiddleware : IMiddleware
{
    public async Task OnTurn(ITurnContext context, MiddlewareSet.NextDelegate next)
    {            
        // This simple middleware reports the request type and if we responded
        await context.SendActivity($"Request type: {context.Activity.Type}");
        
        await next();            

        // Report if any responses were recorded
        string response = context.Responded ? "yes" : "no";
        await context.SendActivity($"Responded?  {response}");
    }
}

public class MyOtherMiddleware : IMiddleware
{
    public async Task OnTurn(ITurnContext context, MiddlewareSet.NextDelegate next)
    {
        // simple middleware to add an additional send activity
        await context.SendActivity($"My other middleware just saying Hi before the bot logic");

        await next();
    }
}

```

# <a name="javascripttabjsimplementmiddleware"></a>[<span data-ttu-id="f4c39-124">JavaScript</span><span class="sxs-lookup"><span data-stu-id="f4c39-124">JavaScript</span></span>](#tab/jsimplementmiddleware)

<span data-ttu-id="f4c39-125">Cada fragmento de software intermedio se hereda de `MiddlewareSet` y siempre implementa `onTurn()`.</span><span class="sxs-lookup"><span data-stu-id="f4c39-125">Each piece of middleware inherits from `MiddlewareSet` and always implements `onTurn()`.</span></span>

<span data-ttu-id="f4c39-126">**ExampleMiddleware.js**</span><span class="sxs-lookup"><span data-stu-id="f4c39-126">**ExampleMiddleware.js**</span></span>
```js
adapter.use({onTurn: async (context, next) =>{

    // This simple middleware reports the activity type and if we responded
    await context.sendActivity(`Activity type: ${context.activity.type}`); 
    await next();            

    // Report if any responses were recorded
    const response = context.activity.text ? "yes" : "no";
    await context.sendActivity(`Responded?  ${response}`);

}}, {onTurn: async (context, next) => {

     // simple middleware to add an additional send activity
     await context.sendActivity("My other middleware just saying Hi before the bot logic");

     await next();
}})
```

---

<span data-ttu-id="f4c39-127">Una llamada a `next()` hace que la ejecución continúe hasta el siguiente fragmento de software intermedio.</span><span class="sxs-lookup"><span data-stu-id="f4c39-127">Calling `next()` causes execution to continue to the next piece of middleware.</span></span> <span data-ttu-id="f4c39-128">La posibilidad de elegir cuándo transferir la ejecución permite escribir código que se ejecuta **después** de que se haya ejecutado el resto de la pila del software intermedio.</span><span class="sxs-lookup"><span data-stu-id="f4c39-128">The ability to choose when execution is passed on allows you to write code that runs **after** the rest of the middleware stack has run.</span></span> <span data-ttu-id="f4c39-129">Se puede realizar una acción en el "borde final" del controlador de procesamiento, después de que se hayan ejecutado la lógica del bot y otro software intermedio.</span><span class="sxs-lookup"><span data-stu-id="f4c39-129">We can take action on the 'trailing edge' of the processing handler, after the bot logic and other middleware has run.</span></span>  <span data-ttu-id="f4c39-130">En el ejemplo anterior, el primer software intermedio implementado hizo eso exactamente, elaborar informes si se respondía a este objeto de contexto, antes de volver a pasar la ejecución a la canalización.</span><span class="sxs-lookup"><span data-stu-id="f4c39-130">In the example above, the first middleware implemented did just that, reporting if we responded for this context object, before passing execution back up the pipeline.</span></span>

## <a name="short-circuit-routing"></a><span data-ttu-id="f4c39-131">Enrutamiento de cortocircuito</span><span class="sxs-lookup"><span data-stu-id="f4c39-131">Short circuit routing</span></span>

<span data-ttu-id="f4c39-132">En algunos casos, puede que desee detener cualquier procesamiento adicional de la actividad recibida, lo que podemos denominar como cortocircuito.</span><span class="sxs-lookup"><span data-stu-id="f4c39-132">In some cases, you may want to stop any further processing of the received activity, which we call short-circuiting.</span></span> <span data-ttu-id="f4c39-133">Esto resulta útil para los casos en que el software intermedio se hace cargo totalmente de una solicitud, ofrece una respuesta sencilla a comandos específicos o puede controlar de otro modo una solicitud entrante sin que la lógica del bot necesite verla.</span><span class="sxs-lookup"><span data-stu-id="f4c39-133">This is useful for cases where the middleware completely takes care of a request, provides an easy response for specific commands, or otherwise can handle an incoming request without the bot logic needing to see it.</span></span>

<span data-ttu-id="f4c39-134">Se va a crear un fragmento de código del software intermedio que enviará una respuesta y evitará cualquier enrutamiento adicional de la solicitud cada vez que el usuario diga "ping":</span><span class="sxs-lookup"><span data-stu-id="f4c39-134">Let's create a piece of middleware that will send a reply and prevent any further routing of the request anytime the user says "ping":</span></span>

# <a name="ctabcsmiddlewareshortcircuit"></a>[<span data-ttu-id="f4c39-135">C#</span><span class="sxs-lookup"><span data-stu-id="f4c39-135">C#</span></span>](#tab/csmiddlewareshortcircuit)
```cs
public class ExampleMiddleware : IMiddleware
{
    public async Task OnTurn(ITurnContext context, MiddlewareSet.NextDelegate next)
    {
        var utterance = context.Activity?.AsMessageActivity()?.Text.Trim().ToLower();

        if (utterance == "ping") 
        {
            context.SendActivity("pong");
            return;
        } 
        else 
        {
            await next();
        }
    }
}
```
# <a name="javascripttabjsmiddlewareshortcircuit"></a>[<span data-ttu-id="f4c39-136">JavaScript</span><span class="sxs-lookup"><span data-stu-id="f4c39-136">JavaScript</span></span>](#tab/jsmiddlewareshortcircuit)
```JavaScript
adapter.use({onTurn: async (context, next) =>{
    const utterance = (context.activity.text || '').trim().toLowerCase();
        if (utterance == "ping") 
        {
            await context.sendActivity("pong");
            return;
        } 
        else 
        {
            await next();
        }

}})

```

---

## <a name="fallback-processing"></a><span data-ttu-id="f4c39-137">Procesamiento de reserva</span><span class="sxs-lookup"><span data-stu-id="f4c39-137">Fallback processing</span></span>

<span data-ttu-id="f4c39-138">Otra cosa que puede que tenga que hacer es responder a una solicitud a la que aún no se haya respondido.</span><span class="sxs-lookup"><span data-stu-id="f4c39-138">Another thing you might need to do is respond to a request that has not been responded to yet.</span></span> <span data-ttu-id="f4c39-139">Esto se realiza fácilmente con el borde final del controlador de procesamiento comprobando la propiedad `context.Responded`.</span><span class="sxs-lookup"><span data-stu-id="f4c39-139">This is easily accomplished using the trailing edge of the processing handler by checking the `context.Responded` property.</span></span> <span data-ttu-id="f4c39-140">Se va a crear un fragmento sencillo de software intermedio que dice automáticamente "No lo he entendido", en caso de que el bot no pueda controlar la solicitud:</span><span class="sxs-lookup"><span data-stu-id="f4c39-140">Let's create a simple piece of middleware that automatically says "I didn't understand," should the bot fail to handle the request:</span></span>

# <a name="ctabcsfallback"></a>[<span data-ttu-id="f4c39-141">C#</span><span class="sxs-lookup"><span data-stu-id="f4c39-141">C#</span></span>](#tab/csfallback)
```cs
public async Task OnTurn(ITurnContext context, MiddlewareSet.NextDelegate next)
{
    await next();

    if (!context.Responded) 
    {
        context.SendActivity("I didn't understand.");
    }
}
```
# <a name="javascripttabjsfallback"></a>[<span data-ttu-id="f4c39-142">JavaScript</span><span class="sxs-lookup"><span data-stu-id="f4c39-142">JavaScript</span></span>](#tab/jsfallback)
```JavaScript
adapter.use({onTurn: async (context, next) =>{
    await next();

    if (!context.responded) 
    {
       await context.sendActivity("I didn't understand.");
    }

}})
```

---

> [!NOTE] 
> <span data-ttu-id="f4c39-143">Esto podría no funcionar en todos los casos, como cundo otro software intermedio puede responder al usuario o cuando el bot recibe un mensaje correctamente pero no responde.</span><span class="sxs-lookup"><span data-stu-id="f4c39-143">This might not work in all cases, such as when other middleware might be able to respond to the user or when the bot receives a message correctly but does not reply.</span></span> <span data-ttu-id="f4c39-144">Responder con "No lo entiendo" resultaría confuso para el usuario.</span><span class="sxs-lookup"><span data-stu-id="f4c39-144">Responding with, "I don't understand" would be misleading to our user.</span></span>


