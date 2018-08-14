---
title: Cómo ejecutar el bot .NET SDK V3 en SDK V4 | Microsoft Docs
description: Obtenga información sobre cómo convertir un bot de la versión 3.x a 4.0 mediante el paquete NuGet clásico.
keywords: migración, bot clásico, convertir v3, v3 a v4
author: v-royhar
ms.author: v-royhar
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 4/25/18
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: a808076e4865a181802b85cfc24ce342dbf23cba
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39304915"
---
# <a name="how-to-run-net-sdk-v3-bots-in-sdk-40"></a>Cómo ejecutar los bots de SDK V3 para .NET en SDK 4.0

El paquete NuGet **Microsoft.Bot.Builder.Classic** facilita la migración de los bots de la versión 3.x a la versión 4.0 de Microsoft Bot Framework.

**NOTA:** Este proceso solo funciona para bots de **aplicación web ASP.NET (.NET Framework)**. No funciona en bots de **aplicación web ASP.NET Core**.

## <a name="the-process"></a>El proceso

El proceso es relativamente simple:

- Agregue el paquete NuGet **Microsoft.Bot.Builder.Classic** al proyecto.
    - También es posible que deba actualizar el paquete NuGet **Autofac**.
- Actualice los espacios de nombres **Microsoft.Bot.Builder.Classic**.
- Invoque cuadros de diálogo con **Conversation.SendAsync()** basado en la versión 4.0 de **ITurnContext**.

### <a name="add-the-microsoftbotbuilderclassic-nuget-package"></a>Agregue el paquete NuGet Microsoft.Bot.Builder.Classic

Para agregar el paquete NuGet **Microsoft.Bot.Builder.Classic**, use **Manage NuGet Packages** (Administrar paquetes NuGet).

### <a name="update-the-namespaces"></a>Actualizar los espacios de nombres

Para actualizar los espacios de nombres, elimine cualquiera de las siguientes instrucciones `using` que encuentre:

```csharp
using Microsoft.Bot.Builder.Dialogs;
using Microsoft.Bot.Builder.Dialogs.Internals;
using Microsoft.Bot.Builder.FormFlow;
using Microsoft.Bot.Builder.Scorables;
```

Luego, en su lugar agregue las siguientes instrucciones `using`:

```csharp
using Microsoft.Bot.Builder.Adapters;
using Microsoft.Bot.Builder.Classic.Dialogs;
using Microsoft.Bot.Builder.Classic.Dialogs.Internals;
using Microsoft.Bot.Builder.Classic.FormFlow;
using Microsoft.Bot.Builder.Classic.Scorables;
using Microsoft.Bot.Connector;
using Microsoft.Bot.Schema;
```

Si tiene una línea `[BotAuthentication]`, elimínela o elimine el comentario.

### <a name="invoke-your-3x-dialog"></a>Invocar el diálogo de la versión 3.x

Para invocar el diálogo de la versión 3.x aún debe usar `Conversation.SendAsync`, solo que ahora se toma el objeto **ITurnContext** de la versión 4.0 en lugar de **Activity**.

```csharp
// invoke a Classic V3 IDialog 
await Conversation.SendAsync(turnContext, () => new EchoDialog());
```

Si no tiene el objeto **ITurnContext**, pero tiene el objeto **Activity**, puede obtener el objeto **ITurnContext** de esta manera:

```csharp
BotFrameworkAdapter adapter = new BotFrameworkAdapter("", "");

await adapter.ProcessActivity(this.Request.Headers.Authorization?.Parameter,
        activity,
        async (context) =>
        {
            // Do something with context here. For example, the body of your Post() method may go here.
        });
```

## <a name="fix-assembly-conflicts"></a>Solucionar los conflictos de ensamblado

Los bots fabricados con el paquete NuGet clásico entrarán en conflicto con sus ensamblados. Esto aparecerá en la ventana Lista de errores en Visual Studio después de una compilación.

### <a name="if-you-see-warning-found-conflicts-between-different-versions-of-the-same-dependent-assembly"></a>Si aparece el mensaje "Warning: Found conflicts between different versions of the same dependent assembly" ("Advertencia: se encontraron conflictos entre diferentes versiones del mismo ensamblado dependiente")

Si encuentra una advertencia que comienza con el siguiente texto: **Found conflicts between different versions of the same dependent assembly** (Se encontraron conflictos entre versiones diferentes del mismo ensamblado dependiente):

- Haga doble clic en el mensaje de advertencia. Aparecerá un cuadro de diálogo con la pregunta: "Do you want to fix these conflicts by adding binding redirect records in the application configuration file? (¿Quiere resolver estos conflictos agregando registros de redireccionamiento de enlace al archivo de configuración de la aplicación?").
- Haga clic en "Sí".
- Vuelva a compilar el proyecto.

### <a name="if-you-see-error-missing-method-exception-on-startup"></a>Si aparece "Error: Missing Method Exception on startup" ("Error: falta la excepción del método en el inicio")

Hay un error con .NET Standard que parece ocurrir cuando se actualiza un proyecto de .NET 4.6 anterior a la versión 4.6.1 y, a continuación, se intenta usar una biblioteca .NET standard en él. Básicamente, hay dos ensamblados System.Net.Http distintos que intentan intercambiarse de forma dinámica. La solución alternativa es agregar una redirección de enlace al archivo Web.config para System.Net.Http. 

Si recibe este error, agregue lo siguiente al archivo Web.config:

```xml
<dependentAssembly>
    <assemblyIdentity name="System.Net.Http" publicKeyToken="B03F5F7F11D50A3A" culture="neutral" />
    <bindingRedirect oldVersion="0.0.0.0-4.2.0.0" newVersion="4.2.0.0" />
</dependentAssembly>
```

Para obtener más información sobre este problema, consulte [System.Net.Http v4.2.0.0 being copied/loaded from MSBuild tooling #25773](https://github.com/dotnet/corefx/issues/25773) (Copia o carga de System.Net.Http v 4.2.0.0 a partir de las herramientas de MSBuild #25773).

## <a name="sample-of-a-converted-bot"></a>Ejemplo de un bot convertido

Para un bot que ya se ha convertido, puede mirar el ejemplo [EchoBot-Classic](https://github.com/Microsoft/botbuilder-dotnet/tree/master/samples/Microsoft.Bot.Samples.EchoBot-Classic) que muestra un bot de la versión 3.x convertido para que funcione con la versión 4.0.

## <a name="limitations"></a>Limitaciones
Microsoft.Bot.Builder.Classic es solo una biblioteca de .NET 4.61 y no funciona en .NET Core.
