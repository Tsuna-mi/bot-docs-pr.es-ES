---
title: Implementación de controladores de mensajes globales | Microsoft Docs
description: Obtenga información sobre permitir que el bot escuche y controle la entrada de usuario que contiene ciertas palabras clave mediante el SDK de Bot Builder para .NET.
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: 796e36fdfd5ef2ee20ba1f5e000f92ef3e4ec1ae
ms.sourcegitcommit: 67445b42796d90661afc643c6bb6533e9a662cbc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2018
ms.locfileid: "39574661"
---
# <a name="implement-global-message-handlers"></a><span data-ttu-id="2b83c-103">Implementación de controladores de mensajes globales</span><span class="sxs-lookup"><span data-stu-id="2b83c-103">Implement global message handlers</span></span>

[!INCLUDE [pre-release-label](../includes/pre-release-label-v3.md)]

[!INCLUDE [Introduction to global message handlers](../includes/snippet-global-handlers-intro.md)]

## <a name="listen-for-keywords-in-user-input"></a><span data-ttu-id="2b83c-104">Escucha de las palabras clave en la entrada de usuario</span><span class="sxs-lookup"><span data-stu-id="2b83c-104">Listen for keywords in user input</span></span>

<span data-ttu-id="2b83c-105">En el tutorial siguiente se muestra cómo implementar controladores de mensajes globales mediante el SDK de Bot Builder para .NET.</span><span class="sxs-lookup"><span data-stu-id="2b83c-105">The following walk through shows how to implement global message handlers by using the Bot Builder SDK for .NET.</span></span>

<span data-ttu-id="2b83c-106">En primer lugar, `Global.asax.cs` registra `GlobalMessageHandlersBotModule`, que se implementa como se muestra aquí.</span><span class="sxs-lookup"><span data-stu-id="2b83c-106">First, `Global.asax.cs` registers `GlobalMessageHandlersBotModule`, which is implemented as shown here.</span></span> <span data-ttu-id="2b83c-107">En este ejemplo, el módulo registra dos diálogos puntuables: uno para administrar una solicitud para cambiar la configuración (`SettingsScorable`) y otro para administrar una solicitud para cancelar (`CancelScoreable`).</span><span class="sxs-lookup"><span data-stu-id="2b83c-107">In this example, the module registers two scorables: one for managing a request to change settings (`SettingsScorable`) and another for managing a request to cancel (`CancelScoreable`).</span></span>

```cs
public class GlobalMessageHandlersBotModule : Module
{
    protected override void Load(ContainerBuilder builder)
    {
        base.Load(builder);

        builder
            .Register(c => new SettingsScorable(c.Resolve<IDialogTask>()))
            .As<IScorable<IActivity, double>>()
            .InstancePerLifetimeScope();

        builder
            .Register(c => new CancelScorable(c.Resolve<IDialogTask>()))
            .As<IScorable<IActivity, double>>()
            .InstancePerLifetimeScope();
    }
}
```

<span data-ttu-id="2b83c-108">`CancelScorable` contiene un método `PrepareAsync` que define el desencadenador: si el texto del mensaje es "Cancelar", se desencadenará este diálogo puntuable.</span><span class="sxs-lookup"><span data-stu-id="2b83c-108">The `CancelScorable` contains a `PrepareAsync` method that defines the trigger: if the message text is "cancel", this scorable will be triggered.</span></span>

```cs
protected override async Task<string> PrepareAsync(IActivity activity, CancellationToken token)
{
    var message = activity as IMessageActivity;
    if (message != null && !string.IsNullOrWhiteSpace(message.Text))
    {
        if (message.Text.Equals("cancel", StringComparison.InvariantCultureIgnoreCase))
        {
            return message.Text;
        }
    }
    return null;
}
```

<span data-ttu-id="2b83c-109">Cuando se recibe una solicitud de "Cancelar", el método `PostAsync` dentro de `CancelScoreable` restablece la pila de diálogos.</span><span class="sxs-lookup"><span data-stu-id="2b83c-109">When a "cancel" request is received, the `PostAsync` method within `CancelScoreable` resets the dialog stack.</span></span> 

```cs
protected override async Task PostAsync(IActivity item, string state, CancellationToken token)
{
    this.task.Reset();
}
```

<span data-ttu-id="2b83c-110">Cuando se recibe una solicitud de "Cambiar configuración", el método `PostAsync` dentro de `SettingsScorable` invoca a `SettingsDialog` (pasando la solicitud a ese diálogo), con lo que se agrega `SettingsDialog` a la parte superior de la pila de diálogos y se pone en control de la conversación.</span><span class="sxs-lookup"><span data-stu-id="2b83c-110">When a "change settings" request is received, the `PostAsync` method within `SettingsScorable` invokes the `SettingsDialog` (passing the request to that dialog), thereby adding the `SettingsDialog` to the top of the dialog stack and putting it in control of the conversation.</span></span>

```cs
protected override async Task PostAsync(IActivity item, string state, CancellationToken token)
{
    var message = item as IMessageActivity;
    if (message != null)
    {
        var settingsDialog = new SettingsDialog();
        var interruption = settingsDialog.Void<object, IMessageActivity>();
        this.task.Call(interruption, null);
        await this.task.PollAsync(token);
    }
}
```

## <a name="sample-code"></a><span data-ttu-id="2b83c-111">Código de ejemplo</span><span class="sxs-lookup"><span data-stu-id="2b83c-111">Sample code</span></span>

<span data-ttu-id="2b83c-112">Para ver un ejemplo completo que muestra cómo implementar controladores de mensajes globales mediante el SDK de Bot Builder para .NET, consulte el <a href="https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/core-GlobalMessageHandlers" target="_blank">ejemplo de controladores de mensajes globales</a> en GitHub.</span><span class="sxs-lookup"><span data-stu-id="2b83c-112">For a complete sample that shows how to implement global message handlers using the Bot Builder SDK for .NET, see the <a href="https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/core-GlobalMessageHandlers" target="_blank">Global Message Handlers sample</a> in GitHub.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2b83c-113">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="2b83c-113">Additional resources</span></span>

- [<span data-ttu-id="2b83c-114">Diseño y control del flujo de conversación</span><span class="sxs-lookup"><span data-stu-id="2b83c-114">Design and control conversation flow</span></span>](../bot-service-design-conversation-flow.md)
- <span data-ttu-id="2b83c-115"><a href="/dotnet/api/?view=botbuilder-3.12.2.4" target="_blank">Referencia del SDK de Bot Builder para .NET</a></span><span class="sxs-lookup"><span data-stu-id="2b83c-115"><a href="/dotnet/api/?view=botbuilder-3.12.2.4" target="_blank">Bot Builder SDK for .NET Reference</a></span></span>
- <span data-ttu-id="2b83c-116"><a href="https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/core-GlobalMessageHandlers" target="_blank">Ejemplo de controladores de mensajes globales (GitHub)</a></span><span class="sxs-lookup"><span data-stu-id="2b83c-116"><a href="https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/core-GlobalMessageHandlers" target="_blank">Global Message Handlers sample (GitHub)</a></span></span>
