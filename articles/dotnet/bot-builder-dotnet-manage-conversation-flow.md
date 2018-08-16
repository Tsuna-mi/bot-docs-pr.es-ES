---
title: Administración del flujo de conversación con diálogos | Microsoft Docs
description: Aprenda a modelar las conversaciones y a administrar el flujo de conversación mediante diálogos y Bot Builder SDK para Node.js.
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: 091da9c1db310491c7e2263d8e7eab51cfcc3787
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305945"
---
# <a name="manage-conversation-flow-with-dialogs"></a><span data-ttu-id="a149c-103">Administración del flujo de conversación con diálogos</span><span class="sxs-lookup"><span data-stu-id="a149c-103">Manage conversation flow with dialogs</span></span>
> [!div class="op_single_selector"]
> - [.NET](../dotnet/bot-builder-dotnet-manage-conversation-flow.md)
> - [Node.js](../nodejs/bot-builder-nodejs-dialog-manage-conversation-flow.md)

[!INCLUDE [Dialog flow example](../includes/snippet-dotnet-manage-conversation-flow-intro.md)]

<span data-ttu-id="a149c-106">En este artículo se describe cómo modelar este flujo de conversación con [diálogos](bot-builder-dotnet-dialogs.md) y Bot Builder SDK para. NET.</span><span class="sxs-lookup"><span data-stu-id="a149c-106">This article describes how to model this conversation flow by using [dialogs](bot-builder-dotnet-dialogs.md) and the Bot Builder SDK for .NET.</span></span> 

## <a name="invoke-the-root-dialog"></a><span data-ttu-id="a149c-107">Invocación del diálogo raíz</span><span class="sxs-lookup"><span data-stu-id="a149c-107">Invoke the root dialog</span></span>

<span data-ttu-id="a149c-108">En primer lugar, el controlador de bot invoca el "diálogo raíz".</span><span class="sxs-lookup"><span data-stu-id="a149c-108">First, the bot controller invokes the "root dialog".</span></span> <span data-ttu-id="a149c-109">En el ejemplo siguiente se muestra cómo conectar la llamada HTTP POST básica a un controlador y luego invocar el diálogo raíz.</span><span class="sxs-lookup"><span data-stu-id="a149c-109">The following example shows how to wire the basic HTTP POST call to a controller and then invoke the root dialog.</span></span> 

```cs
public class MessagesController : ApiController
{
    public async Task<HttpResponseMessage> Post([FromBody]Activity activity)
    {
            // Redirect to the root dialog.
            await Conversation.SendAsync(activity, () => new RootDialog()); 
            ...
    }
}
```

## <a name="invoke-the-new-order-dialog"></a><span data-ttu-id="a149c-110">Invocación del diálogo "Nuevo pedido"</span><span class="sxs-lookup"><span data-stu-id="a149c-110">Invoke the 'New Order' dialog</span></span>

<span data-ttu-id="a149c-111">A continuación, el diálogo raíz invoca el diálogo "Nuevo pedido".</span><span class="sxs-lookup"><span data-stu-id="a149c-111">Next, the root dialog invokes the 'New Order' dialog.</span></span> 

```cs
[Serializable]
public class RootDialog : IDialog<object>
{
    public async Task StartAsync(IDialogContext context)
    {
        // Root dialog initiates and waits for the next message from the user. 
        // When a message arrives, call MessageReceivedAsync.
        context.Wait(this.MessageReceivedAsync); 
    }

    public virtual async Task MessageReceivedAsync(IDialogContext context, IAwaitable<IMessageActivity> result)
    {
        var message = await result; // We've got a message!
        if (message.Text.ToLower().Contains("order"))
        {
            // User said 'order', so invoke the New Order Dialog and wait for it to finish.
            // Then, call ResumeAfterNewOrderDialog.
            await context.Forward(new NewOrderDialog(), this.ResumeAfterNewOrderDialog, message, CancellationToken.None);
        }
        // User typed something else; for simplicity, ignore this input and wait for the next message.
        context.Wait(this.MessageReceivedAsync);
    }

    private async Task ResumeAfterNewOrderDialog(IDialogContext context, IAwaitable<string> result)
    {
        // Store the value that NewOrderDialog returned. 
        // (At this point, new order dialog has finished and returned some value to use within the root dialog.)
        var resultFromNewOrder = await result;

        await context.PostAsync($"New order dialog just told me this: {resultFromNewOrder}");

        // Again, wait for the next message from the user.
        context.Wait(this.MessageReceivedAsync);
    }
}
```

## <a id="dialog-lifecycle"></a> <span data-ttu-id="a149c-112">Ciclo de vida de los diálogos</span><span class="sxs-lookup"><span data-stu-id="a149c-112">Dialog lifecycle</span></span>

<span data-ttu-id="a149c-113">Cuando se invoca un diálogo, toma el control del flujo de la conversación.</span><span class="sxs-lookup"><span data-stu-id="a149c-113">When a dialog is invoked, it takes control of the conversation flow.</span></span> <span data-ttu-id="a149c-114">Todos los mensajes nuevos estarán sujetos a procesamiento por parte de ese diálogo hasta que se cierra o redirija a otro diálogo.</span><span class="sxs-lookup"><span data-stu-id="a149c-114">Every new message will be subject to processing by that dialog until it either closes or redirects to another dialog.</span></span> 

<span data-ttu-id="a149c-115">En C#, puede usar `context.Wait()` para especificar la devolución de llamada para invocar la próxima vez que el usuario envíe un mensaje.</span><span class="sxs-lookup"><span data-stu-id="a149c-115">In C#, you can use `context.Wait()` to specify the callback to invoke the next time the user sends a message.</span></span> <span data-ttu-id="a149c-116">Para cerrar un diálogo y quitarlo de la pila (de forma que se envíe de nuevo al usuario al diálogo anterior de la pila), use `context.Done()`.</span><span class="sxs-lookup"><span data-stu-id="a149c-116">To close a dialog and remove it from the stack (thereby sending the user back to the prior dialog in the stack), use `context.Done()`.</span></span> <span data-ttu-id="a149c-117">Todos los métodos de diálogo se deben terminar con `context.Wait()`, `context.Fail()`, `context.Done()`, o alguna directiva de redireccionamiento como `context.Forward()` o `context.Call()`.</span><span class="sxs-lookup"><span data-stu-id="a149c-117">You must end every dialog method with `context.Wait()`, `context.Fail()`, `context.Done()`, or some redirection directive such as `context.Forward()` or `context.Call()`.</span></span> <span data-ttu-id="a149c-118">Un método de diálogo que no termina de alguna de estas maneras generará un error (porque el marco no sabe qué acción realizar la próxima vez que el usuario envíe un mensaje).</span><span class="sxs-lookup"><span data-stu-id="a149c-118">A dialog method that does not end with one of these will result in an error (because the framework does not know what action to take the next time the user sends a message).</span></span>

## <a name="passing-state-between-dialogs"></a><span data-ttu-id="a149c-119">Paso del estado entre diálogos</span><span class="sxs-lookup"><span data-stu-id="a149c-119">Passing state between dialogs</span></span>

<span data-ttu-id="a149c-120">Aunque puede almacenar el estado en Bot State, también puede pasar datos entre distintos diálogos mediante la sobrecarga del constructor de clases de diálogos.</span><span class="sxs-lookup"><span data-stu-id="a149c-120">While you can store state in bot state, you can also pass data between different dialogs by overloading your dialog class constructor.</span></span>

```cs
[Serializable]
public class AgeDialog : IDialog<int>
{
    private string name;

    public AgeDialog(string name)
    {
        this.name = name;
    }
}
 ```

<span data-ttu-id="a149c-121">Código de diálogo de llamada, paso del valor de nombre del usuario.</span><span class="sxs-lookup"><span data-stu-id="a149c-121">Calling dialog code, passing in name value from the user.</span></span>

```cs
private async Task NameDialogResumeAfter(IDialogContext context, IAwaitable<string> result)
{
    try
    {
        this.name = await result;
        context.Call(new AgeDialog(this.name), this.AgeDialogResumeAfter);
    }
    catch (TooManyAttemptsException)
    {
        await context.PostAsync("I'm sorry, I'm having issues understanding you. Let's try again.");
        await this.SendWelcomeMessageAsync(context);
    }
}
```

## <a name="sample-code"></a><span data-ttu-id="a149c-122">Código de ejemplo</span><span class="sxs-lookup"><span data-stu-id="a149c-122">Sample code</span></span> 

<span data-ttu-id="a149c-123">Para obtener un ejemplo completo que muestra cómo administrar una conversación mediante diálogos en Bot Builder SDK para. NET, consulte el <a href="https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/core-BasicMultiDialog" target="_blank">ejemplo básico de varios diálogos</a> en GitHub.</span><span class="sxs-lookup"><span data-stu-id="a149c-123">For a complete sample that shows how to manage a conversation by using dialogs in the Bot Builder SDK for .NET, see the <a href="https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/core-BasicMultiDialog" target="_blank">Basic Multi-Dialog sample</a> in GitHub.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a149c-124">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="a149c-124">Additional resources</span></span>

- [<span data-ttu-id="a149c-125">Diálogos</span><span class="sxs-lookup"><span data-stu-id="a149c-125">Dialogs</span></span>](bot-builder-dotnet-dialogs.md)
- [<span data-ttu-id="a149c-126">Diseño y control del flujo de conversación</span><span class="sxs-lookup"><span data-stu-id="a149c-126">Design and control conversation flow</span></span>](../bot-service-design-conversation-flow.md)
- <span data-ttu-id="a149c-127"><a href="https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/core-BasicMultiDialog" target="_blank">Ejemplo básico de varios diálogos (GitHub)</a></span><span class="sxs-lookup"><span data-stu-id="a149c-127"><a href="https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/core-BasicMultiDialog" target="_blank">Basic Multi-Dialog sample (GitHub)</a></span></span>
- <span data-ttu-id="a149c-128"><a href="/dotnet/api/?view=botbuilder-3.11.0" target="_blank">Referencia de Bot Builder SDK para .NET</a></span><span class="sxs-lookup"><span data-stu-id="a149c-128"><a href="/dotnet/api/?view=botbuilder-3.11.0" target="_blank">Bot Builder SDK for .NET Reference</a></span></span>
