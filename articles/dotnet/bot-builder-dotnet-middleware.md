---
title: Interceptación de mensajes | Microsoft Docs
description: Obtenga información sobre cómo interceptar mensajes entre el usuario y un bot con Bot Builder SDK para .NET.
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: fbcb72fcd8f3903efc437d32dae4427538662c7f
ms.sourcegitcommit: 67445b42796d90661afc643c6bb6533e9a662cbc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2018
ms.locfileid: "39574881"
---
# <a name="intercept-messages"></a><span data-ttu-id="ce5cc-103">Interceptación de mensajes</span><span class="sxs-lookup"><span data-stu-id="ce5cc-103">Intercept messages</span></span>

[!INCLUDE [pre-release-label](../includes/pre-release-label-v3.md)]

> [!div class="op_single_selector"]
> - [.NET](../dotnet/bot-builder-dotnet-middleware.md)
> - [Node.js](../nodejs/bot-builder-nodejs-intercept-messages.md)

[!INCLUDE [Introducton to message logging](../includes/snippet-message-logging-intro.md)]

## <a name="intercept-and-log-messages"></a><span data-ttu-id="ce5cc-106">Interceptación y registro de mensajes</span><span class="sxs-lookup"><span data-stu-id="ce5cc-106">Intercept and log messages</span></span>

<span data-ttu-id="ce5cc-107">En el ejemplo de código siguiente se muestra cómo interceptar los mensajes que se intercambian entre el usuario y el bot utilizando el concepto de **software intermedio** de Bot Builder SDK para .NET.</span><span class="sxs-lookup"><span data-stu-id="ce5cc-107">The following code sample shows how to intercept messages that are exchanged between user and bot using the concept of **middleware** in the Bot Builder SDK for .NET.</span></span> 

<span data-ttu-id="ce5cc-108">Primero, cree una clase `DebugActivityLogger` y defina un método `LogAsync` para especificar qué operación se realiza para cada mensaje interceptado.</span><span class="sxs-lookup"><span data-stu-id="ce5cc-108">First, create a `DebugActivityLogger` class and define a `LogAsync` method to specify what action is taken for each intercepted message.</span></span> <span data-ttu-id="ce5cc-109">Este ejemplo imprime solo alguna información sobre cada mensaje.</span><span class="sxs-lookup"><span data-stu-id="ce5cc-109">This example just prints some information about each message.</span></span>

```cs
public class DebugActivityLogger : IActivityLogger
{
    public async Task LogAsync(IActivity activity)
    {
        Debug.WriteLine($"From:{activity.From.Id} - To:{activity.Recipient.Id} - Message:{activity.AsMessageActivity()?.Text}");
    }
}
```

<span data-ttu-id="ce5cc-110">Después agregue el siguiente código a `Global.asax.cs`.</span><span class="sxs-lookup"><span data-stu-id="ce5cc-110">Then, add the following code to `Global.asax.cs`.</span></span>  <span data-ttu-id="ce5cc-111">Todos los mensajes intercambiados entre el usuario y el bot (en cualquier dirección) ahora desencadenarán el método `LogAsync` en la clase `DebugActivityLogger`.</span><span class="sxs-lookup"><span data-stu-id="ce5cc-111">Every message that is exchanged between user and bot (in either direction) will now trigger the `LogAsync` method in the `DebugActivityLogger` class.</span></span> 

```cs
    public class WebApiApplication : System.Web.HttpApplication
    {
        protected void Application_Start()
        {
            var builder = new ContainerBuilder();
            builder.RegisterType<DebugActivityLogger>().AsImplementedInterfaces().InstancePerDependency();
            builder.Update(Conversation.Container);

            GlobalConfiguration.Configure(WebApiConfig.Register);
        }
    }
```

<span data-ttu-id="ce5cc-112">Aunque este ejemplo solo imprime alguna información sobre cada mensaje, puede actualizar el método `LogAsync` para especificar las acciones que desea realizar para cada mensaje.</span><span class="sxs-lookup"><span data-stu-id="ce5cc-112">Although this example simply prints some information about each message, you can update the `LogAsync` method to specify the actions that you want to take for each message.</span></span> 

## <a name="sample-code"></a><span data-ttu-id="ce5cc-113">Código de ejemplo</span><span class="sxs-lookup"><span data-stu-id="ce5cc-113">Sample code</span></span> 

<span data-ttu-id="ce5cc-114">Para obtener un ejemplo completo en el que se muestre cómo interceptar y registrar mensajes con Bot Builder SDK para .NET, consulte el <a href="https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/core-Middleware" target="_blank">ejemplo de software intermedio</a> en GitHub.</span><span class="sxs-lookup"><span data-stu-id="ce5cc-114">For a complete sample that shows how to intercept and log messages using the Bot Builder SDK for .NET, see the <a href="https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/core-Middleware" target="_blank">Middleware sample</a> in GitHub.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="ce5cc-115">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="ce5cc-115">Additional resources</span></span>

- <span data-ttu-id="ce5cc-116"><a href="/dotnet/api/?view=botbuilder-3.11.0" target="_blank">Referencia del SDK de Bot Builder para .NET</a></span><span class="sxs-lookup"><span data-stu-id="ce5cc-116"><a href="/dotnet/api/?view=botbuilder-3.11.0" target="_blank">Bot Builder SDK for .NET Reference</a></span></span>
- <span data-ttu-id="ce5cc-117"><a href="https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/core-Middleware" target="_blank">Ejemplo de software intermedio (GitHub)</a></span><span class="sxs-lookup"><span data-stu-id="ce5cc-117"><a href="https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/core-Middleware" target="_blank">Middleware sample (GitHub)</a></span></span>
