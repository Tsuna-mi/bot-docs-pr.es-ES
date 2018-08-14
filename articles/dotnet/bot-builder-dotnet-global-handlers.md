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
# <a name="implement-global-message-handlers"></a>Implementación de controladores de mensajes globales

[!INCLUDE [pre-release-label](../includes/pre-release-label-v3.md)]

[!INCLUDE [Introduction to global message handlers](../includes/snippet-global-handlers-intro.md)]

## <a name="listen-for-keywords-in-user-input"></a>Escucha de las palabras clave en la entrada de usuario

En el tutorial siguiente se muestra cómo implementar controladores de mensajes globales mediante el SDK de Bot Builder para .NET.

En primer lugar, `Global.asax.cs` registra `GlobalMessageHandlersBotModule`, que se implementa como se muestra aquí. En este ejemplo, el módulo registra dos diálogos puntuables: uno para administrar una solicitud para cambiar la configuración (`SettingsScorable`) y otro para administrar una solicitud para cancelar (`CancelScoreable`).

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

`CancelScorable` contiene un método `PrepareAsync` que define el desencadenador: si el texto del mensaje es "Cancelar", se desencadenará este diálogo puntuable.

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

Cuando se recibe una solicitud de "Cancelar", el método `PostAsync` dentro de `CancelScoreable` restablece la pila de diálogos. 

```cs
protected override async Task PostAsync(IActivity item, string state, CancellationToken token)
{
    this.task.Reset();
}
```

Cuando se recibe una solicitud de "Cambiar configuración", el método `PostAsync` dentro de `SettingsScorable` invoca a `SettingsDialog` (pasando la solicitud a ese diálogo), con lo que se agrega `SettingsDialog` a la parte superior de la pila de diálogos y se pone en control de la conversación.

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

## <a name="sample-code"></a>Código de ejemplo

Para ver un ejemplo completo que muestra cómo implementar controladores de mensajes globales mediante el SDK de Bot Builder para .NET, consulte el <a href="https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/core-GlobalMessageHandlers" target="_blank">ejemplo de controladores de mensajes globales</a> en GitHub.

## <a name="additional-resources"></a>Recursos adicionales

- [Diseño y control del flujo de conversación](../bot-service-design-conversation-flow.md)
- <a href="/dotnet/api/?view=botbuilder-3.12.2.4" target="_blank">Referencia del SDK de Bot Builder para .NET</a>
- <a href="https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/core-GlobalMessageHandlers" target="_blank">Ejemplo de controladores de mensajes globales (GitHub)</a>
