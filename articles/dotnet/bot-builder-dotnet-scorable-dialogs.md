---
title: Controladores de mensajes globales mediante diálogos puntuables
description: Cree diálogos más flexibles mediante puntuables en el SDK de Bot Builder para .NET.
author: matthewshim-ms
ms.author: v-shimma
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: 8ba63ad99c772c7cf5884180a62244e0dfe11db2
ms.sourcegitcommit: 67445b42796d90661afc643c6bb6533e9a662cbc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2018
ms.locfileid: "39574911"
---
# <a name="global-message-handlers-using-scorables"></a><span data-ttu-id="1454a-103">Controladores de mensajes globales mediante diálogos puntuables</span><span class="sxs-lookup"><span data-stu-id="1454a-103">Global message handlers using scorables</span></span>

[!INCLUDE [pre-release-label](../includes/pre-release-label-v3.md)]

<span data-ttu-id="1454a-104">Los usuarios intentan acceder a ciertas funcionalidades dentro del bot con palabras como "ayuda", "cancelar" o "empezar de nuevo" en medio de una conversación cuando el bot está esperando una respuesta distinta.</span><span class="sxs-lookup"><span data-stu-id="1454a-104">Users attempt to access certain functionality within a bot by using words like "help," "cancel," or "start over" in the middle of a conversation when the bot is expecting a different response.</span></span> <span data-ttu-id="1454a-105">Puede diseñar su bot para controlar correctamente estas solicitudes mediante los diálogos puntuables.</span><span class="sxs-lookup"><span data-stu-id="1454a-105">You can design your bot to gracefully handle such requests using scorable dialogs.</span></span>

<span data-ttu-id="1454a-106">Los diálogos puntuables supervisan todos los mensajes entrantes y determinan si un mensaje es accionable de alguna manera.</span><span class="sxs-lookup"><span data-stu-id="1454a-106">Scorable dialogs monitor all incoming messages and determine whether a message is actionable in some way.</span></span> <span data-ttu-id="1454a-107">A los mensajes puntuables se les asigna una puntuación entre [0 y 1] en cada diálogo puntuable.</span><span class="sxs-lookup"><span data-stu-id="1454a-107">Messages that are scorable are assigned a score between [0 – 1] by each scorable dialog.</span></span> <span data-ttu-id="1454a-108">El diálogo puntuable que determina la puntuación más alta se agrega a la parte superior de la pila del diálogo y, a continuación, entrega la respuesta al usuario.</span><span class="sxs-lookup"><span data-stu-id="1454a-108">The scorable dialog that determines the highest score is added to the top of the dialog stack and then hands the response to the user.</span></span> <span data-ttu-id="1454a-109">Después de que el diálogo puntuable finalice su ejecución, la conversación continúa desde donde se dejó.</span><span class="sxs-lookup"><span data-stu-id="1454a-109">After the scorable dialog completes execution, the conversation continues from where it left off.</span></span>

<span data-ttu-id="1454a-110">Los diálogos puntuables le permiten crear conversaciones más flexibles, ya que así los usuarios pueden "interrumpir" el flujo normal de conversación que se ve en los diálogos normales.</span><span class="sxs-lookup"><span data-stu-id="1454a-110">Scorables enable you to create more flexible conversations by allowing your users to 'interrupt' the normal conversation flow you find in regular dialogs.</span></span>

## <a name="create-a-scorable-dialog"></a><span data-ttu-id="1454a-111">Creación de un diálogo puntuable</span><span class="sxs-lookup"><span data-stu-id="1454a-111">Create a scorable dialog</span></span>

<span data-ttu-id="1454a-112">En primer lugar, defina un nuevo [diálogo](bot-builder-dotnet-dialogs.md).</span><span class="sxs-lookup"><span data-stu-id="1454a-112">First, define a new [dialog](bot-builder-dotnet-dialogs.md).</span></span> <span data-ttu-id="1454a-113">El código siguiente usa un diálogo que se deriva de la interfaz `IDialog`.</span><span class="sxs-lookup"><span data-stu-id="1454a-113">The following code uses a dialog that is derived from the `IDialog` interface.</span></span>

```cs
public class SampleDialog : IDialog<object>
{
    public async Task StartAsync(IDialogContext context)
    {
        await context.PostAsync("This is a Sample Dialog which is Scorable. Reply with anything to return to the prior prior dialog.");

        context.Wait(this.MessageReceived);
    }

    private async Task MessageReceived(IDialogContext context, IAwaitable<IMessageActivity> result)
    {
        var message = await result;

        if ((message.Text != null) && (message.Text.Trim().Length > 0))
        {
            context.Done<object>(null);
        }
        else
        {
            context.Fail(new Exception("Message was not a string or was an empty string."));
        }
    }
}
```
<span data-ttu-id="1454a-114">Para hacer un diálogo puntuable, cree una clase que herede de la clase abstracta `ScorableBase`.</span><span class="sxs-lookup"><span data-stu-id="1454a-114">To make a scorable dialog, create a class that inherits from the `ScorableBase` abstract class.</span></span> <span data-ttu-id="1454a-115">En el siguiente código se muestra una clase `SampleScorable`.</span><span class="sxs-lookup"><span data-stu-id="1454a-115">The following code shows a `SampleScorable` class.</span></span>

```cs
using Microsoft.Bot.Builder.Dialogs;
using Microsoft.Bot.Builder.Dialogs.Internals;
using Microsoft.Bot.Builder.Internals.Fibers;
using Microsoft.Bot.Builder.Scorables.Internals;

public class SampleScorable : ScorableBase<IActivity, string, double>
{
    private readonly IDialogTask task;

    public SampleScorable(IDialogTask task)
    {
        SetField.NotNull(out this.task, nameof(task), task);
    }
}
```
<span data-ttu-id="1454a-116">La clase abstracta `ScorableBase` hereda desde la interfaz `IScorable`.</span><span class="sxs-lookup"><span data-stu-id="1454a-116">The `ScorableBase` abstract class inherits from the `IScorable` interface.</span></span> <span data-ttu-id="1454a-117">Deberá implementar los siguientes métodos `IScorable` en la clase:</span><span class="sxs-lookup"><span data-stu-id="1454a-117">You will need to implement the following `IScorable` methods in your class:</span></span>

- <span data-ttu-id="1454a-118">`PrepareAsync` es el primer método que se llama en la instancia puntuable.</span><span class="sxs-lookup"><span data-stu-id="1454a-118">`PrepareAsync` is the first method that is called in the scorable instance.</span></span> <span data-ttu-id="1454a-119">Acepta la actividad de mensaje entrante y analiza y establece el estado del diálogo, el cual se pasa a todos los demás métodos de la interfaz `IScorable`.</span><span class="sxs-lookup"><span data-stu-id="1454a-119">It accepts incoming message activity, analyzes and sets the dialog's state, which is passed to all the other methods of the `IScorable` interface.</span></span>

```cs
protected override async Task<string> PrepareAsync(IActivity item, CancellationToken token)
{
        // TODO: insert your code here
}
```

- <span data-ttu-id="1454a-120">El método `HasScore` comprueba la propiedad de estado para determinar si el diálogo puntuable debe darle una puntuación al mensaje.</span><span class="sxs-lookup"><span data-stu-id="1454a-120">The `HasScore` method checks the state property to determine if the scorable dialog should provide a score for the message.</span></span> <span data-ttu-id="1454a-121">Si devuelve false, se omitirá el mensaje en el diálogo puntuable.</span><span class="sxs-lookup"><span data-stu-id="1454a-121">If it returns false, the message will be ignored by the scorable dialog.</span></span>

```cs
protected override bool HasScore(IActivity item, string state)
{
        // TODO: insert your code here
}
```

- <span data-ttu-id="1454a-122">`GetScore` solo se desencadenará si `HasScore` devuelve true.</span><span class="sxs-lookup"><span data-stu-id="1454a-122">`GetScore` will only trigger if `HasScore` returns true.</span></span> <span data-ttu-id="1454a-123">Deberá aprovisionar la lógica en este método para determinar la puntuación para un mensaje entre 0 y 1.</span><span class="sxs-lookup"><span data-stu-id="1454a-123">You’ll provision the logic in this method to determine the score for a message between 0 - 1.</span></span>

```cs
protected override double GetScore(IActivity item, string state)
{
        // TODO: insert your code here
}
```
- <span data-ttu-id="1454a-124">En el método `PostAsync`, defina acciones básicas para que la clase puntuable las realice.</span><span class="sxs-lookup"><span data-stu-id="1454a-124">In the `PostAsync` method, define core actions to be performed for the scorable class.</span></span> <span data-ttu-id="1454a-125">Todos los diálogos puntuables supervisarán a los mensajes entrantes y asignarán puntuaciones a los mensajes válidos en función del método GetScore de puntuables.</span><span class="sxs-lookup"><span data-stu-id="1454a-125">All scorable dialogs will monitor incoming messages, and assign scores to valid messages based on the scorables' GetScore method.</span></span> <span data-ttu-id="1454a-126">La clase puntuable que determina la puntuación más alta (entre 0 y 1,0) desencadenará el método `PostAsync` del puntuable.</span><span class="sxs-lookup"><span data-stu-id="1454a-126">The scorable class which determines the highest score (between 0 - 1.0) will then trigger that scorable's `PostAsync` method.</span></span>

```cs
protected override Task PostAsync(IActivity item, string state, CancellationToken token)
{
        //TODO: insert your code here
}
```

- <span data-ttu-id="1454a-127">Se realiza una llamada a `DoneAsync` una vez completado el proceso de puntuación.</span><span class="sxs-lookup"><span data-stu-id="1454a-127">`DoneAsync` is called after the scoring process is complete.</span></span> <span data-ttu-id="1454a-128">Use este método para eliminar los recursos con ámbito.</span><span class="sxs-lookup"><span data-stu-id="1454a-128">Use this method to dispose of any scoped resources.</span></span>

```cs
protected override Task DoneAsync(IActivity item, string state, CancellationToken token)
{
        //TODO: insert your code here
}
```

## <a name="create-a-module-to-register-the-iscorable-service"></a><span data-ttu-id="1454a-129">Creación de un módulo para registrar el servicio IScorable</span><span class="sxs-lookup"><span data-stu-id="1454a-129">Create a module to register the IScorable service</span></span>

<span data-ttu-id="1454a-130">A continuación, defina `Module` que registrará a la clase `SampleScorable` como un componente.</span><span class="sxs-lookup"><span data-stu-id="1454a-130">Next, define a `Module` that will register the `SampleScorable` class as a component.</span></span> <span data-ttu-id="1454a-131">Esto aprovisionará al servicio `IScorable`.</span><span class="sxs-lookup"><span data-stu-id="1454a-131">This will provision the `IScorable` service.</span></span>

```cs
public class GlobalMessageHandlersBotModule : Module
{
    protected override void Load(ContainerBuilder builder)
    {
        base.Load(builder);

        builder
            .Register(c => new SampleScorable(c.Resolve<IDialogTask>()))
            .As<IScorable<IActivity, double>>()
            .InstancePerLifetimeScope();
    }
}
```
## <a name="register-the-module"></a><span data-ttu-id="1454a-132">Registro del módulo</span><span class="sxs-lookup"><span data-stu-id="1454a-132">Register the module</span></span>  

<span data-ttu-id="1454a-133">El último paso del proceso es aplicar `SampleScorable` al contenedor de conversación del bot.</span><span class="sxs-lookup"><span data-stu-id="1454a-133">The last step in the process is to apply the `SampleScorable` to the bot's Conversation Container.</span></span> <span data-ttu-id="1454a-134">Esto registrará el servicio puntuable dentro de la canalización de control de mensajes de Bot Framework.</span><span class="sxs-lookup"><span data-stu-id="1454a-134">This will register the scorable service within the Bot Framework's message handling pipeline.</span></span> <span data-ttu-id="1454a-135">En el siguiente código se muestra cómo actualizar a `Conversation.Container` dentro de la inicialización de la aplicación del bot en **Global.asax.cs**:</span><span class="sxs-lookup"><span data-stu-id="1454a-135">The following code shows to update the `Conversation.Container` within the bot app's initialization in **Global.asax.cs**:</span></span>

```cs
public class WebApiApplication : System.Web.HttpApplication
{
    protected void Application_Start()
    {
        this.RegisterBotModules();
        GlobalConfiguration.Configure(WebApiConfig.Register);
    }

    private void RegisterBotModules()
    {
        var builder = new ContainerBuilder();
        builder.RegisterModule(new ReflectionSurrogateModule());

        //Register the module within the Conversation container
        builder.RegisterModule<GlobalMessageHandlersBotModule>();

        builder.Update(Conversation.Container);
    }
}
```

## <a name="additional-resources"></a><span data-ttu-id="1454a-136">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="1454a-136">Additional resources</span></span>
* [<span data-ttu-id="1454a-137">Ejemplo de controladores de mensajes globales</span><span class="sxs-lookup"><span data-stu-id="1454a-137">Global Message Handlers sample</span></span>](https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/core-GlobalMessageHandlers)
* [<span data-ttu-id="1454a-138">Ejemplo de Bot puntuable simple</span><span class="sxs-lookup"><span data-stu-id="1454a-138">Simple Scorable Bot sample</span></span>](https://github.com/Microsoft/BotFramework-Samples/tree/master/blog-samples/CSharp/ScorableBotSample)
* [<span data-ttu-id="1454a-139">Introducción a los diálogos</span><span class="sxs-lookup"><span data-stu-id="1454a-139">Dialogs overview</span></span>](bot-builder-dotnet-dialogs.md)
* [<span data-ttu-id="1454a-140">AutoFac</span><span class="sxs-lookup"><span data-stu-id="1454a-140">AutoFac</span></span>](https://autofac.org/)
