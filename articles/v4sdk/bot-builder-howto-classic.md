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
ms.openlocfilehash: 3f7e88192d989e79f2124e03571ddba3b5291955
ms.sourcegitcommit: 2dc75701b169d822c9499e393439161bc87639d2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42904639"
---
# <a name="how-to-run-net-sdk-v3-bots-in-sdk-40"></a><span data-ttu-id="55ece-104">Cómo ejecutar los bots de SDK V3 para .NET en SDK 4.0</span><span class="sxs-lookup"><span data-stu-id="55ece-104">How to run .NET SDK V3 bots in SDK 4.0</span></span>

[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]

<span data-ttu-id="55ece-105">El paquete NuGet **Microsoft.Bot.Builder.Classic** facilita la migración de los bots de la versión 3.x a la versión 4.0 de Microsoft Bot Framework.</span><span class="sxs-lookup"><span data-stu-id="55ece-105">The **Microsoft.Bot.Builder.Classic** NuGet package eases migration of your bots from version 3.x to version 4.0 of the Microsoft Bot Framework.</span></span>

<span data-ttu-id="55ece-106">**NOTA:** Este proceso solo funciona para bots de **aplicación web ASP.NET (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="55ece-106">**NOTE:** This process only works for **ASP.NET Web Application (.NET Framework)** bots.</span></span> <span data-ttu-id="55ece-107">No funciona en bots de **aplicación web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="55ece-107">This does not work on **ASP.NET Core Web Application** bots.</span></span>

## <a name="the-process"></a><span data-ttu-id="55ece-108">El proceso</span><span class="sxs-lookup"><span data-stu-id="55ece-108">The process</span></span>

<span data-ttu-id="55ece-109">El proceso es relativamente simple:</span><span class="sxs-lookup"><span data-stu-id="55ece-109">The process is relatively simple:</span></span>

- <span data-ttu-id="55ece-110">Agregue el paquete NuGet **Microsoft.Bot.Builder.Classic** al proyecto.</span><span class="sxs-lookup"><span data-stu-id="55ece-110">Add the **Microsoft.Bot.Builder.Classic** NuGet package to the project.</span></span>
    - <span data-ttu-id="55ece-111">También es posible que deba actualizar el paquete NuGet **Autofac**.</span><span class="sxs-lookup"><span data-stu-id="55ece-111">You may also need to update the **Autofac** NuGet package.</span></span>
- <span data-ttu-id="55ece-112">Actualice los espacios de nombres **Microsoft.Bot.Builder.Classic**.</span><span class="sxs-lookup"><span data-stu-id="55ece-112">Update the **Microsoft.Bot.Builder.Classic** namespaces.</span></span>
- <span data-ttu-id="55ece-113">Invoque cuadros de diálogo con **Conversation.SendAsync()** basado en la versión 4.0 de **ITurnContext**.</span><span class="sxs-lookup"><span data-stu-id="55ece-113">Invoke dialogs using the 4.0 **ITurnContext** based **Conversation.SendAsync()**.</span></span>

### <a name="add-the-microsoftbotbuilderclassic-nuget-package"></a><span data-ttu-id="55ece-114">Agregue el paquete NuGet Microsoft.Bot.Builder.Classic</span><span class="sxs-lookup"><span data-stu-id="55ece-114">Add the Microsoft.Bot.Builder.Classic NuGet package</span></span>

<span data-ttu-id="55ece-115">Para agregar el paquete NuGet **Microsoft.Bot.Builder.Classic**, use **Manage NuGet Packages** (Administrar paquetes NuGet).</span><span class="sxs-lookup"><span data-stu-id="55ece-115">To add the **Microsoft.Bot.Builder.Classic** NuGet package, you use **Manage NuGet Packages** to add the package.</span></span>

### <a name="update-the-namespaces"></a><span data-ttu-id="55ece-116">Actualizar los espacios de nombres</span><span class="sxs-lookup"><span data-stu-id="55ece-116">Update the namespaces</span></span>

<span data-ttu-id="55ece-117">Para actualizar los espacios de nombres, elimine cualquiera de las siguientes instrucciones `using` que encuentre:</span><span class="sxs-lookup"><span data-stu-id="55ece-117">To update the namespaces, delete any of the following `using` statements you find:</span></span>

```csharp
using Microsoft.Bot.Builder.Dialogs;
using Microsoft.Bot.Builder.Dialogs.Internals;
using Microsoft.Bot.Builder.FormFlow;
using Microsoft.Bot.Builder.Scorables;
```

<span data-ttu-id="55ece-118">Luego, en su lugar agregue las siguientes instrucciones `using`:</span><span class="sxs-lookup"><span data-stu-id="55ece-118">Then add the following `using` statements in their place:</span></span>

```csharp
using Microsoft.Bot.Builder.Adapters;
using Microsoft.Bot.Builder.Classic.Dialogs;
using Microsoft.Bot.Builder.Classic.Dialogs.Internals;
using Microsoft.Bot.Builder.Classic.FormFlow;
using Microsoft.Bot.Builder.Classic.Scorables;
using Microsoft.Bot.Connector;
using Microsoft.Bot.Schema;
```

<span data-ttu-id="55ece-119">Si tiene una línea `[BotAuthentication]`, elimínela o elimine el comentario.</span><span class="sxs-lookup"><span data-stu-id="55ece-119">If you have a `[BotAuthentication]` line, delete the line or comment it out.</span></span>

### <a name="invoke-your-3x-dialog"></a><span data-ttu-id="55ece-120">Invocar el diálogo de la versión 3.x</span><span class="sxs-lookup"><span data-stu-id="55ece-120">Invoke your 3.x dialog</span></span>

<span data-ttu-id="55ece-121">Para invocar el diálogo de la versión 3.x aún debe usar `Conversation.SendAsync`, solo que ahora se toma el objeto **ITurnContext** de la versión 4.0 en lugar de **Activity**.</span><span class="sxs-lookup"><span data-stu-id="55ece-121">To invoke your 3.x dialog you still use `Conversation.SendAsync`, only now it takes a 4.0 **ITurnContext** instead of an **Activity**.</span></span>

```csharp
// invoke a Classic V3 IDialog 
await Conversation.SendAsync(turnContext, () => new EchoDialog());
```

<span data-ttu-id="55ece-122">Si no tiene el objeto **ITurnContext**, pero tiene el objeto **Activity**, puede obtener el objeto **ITurnContext** de esta manera:</span><span class="sxs-lookup"><span data-stu-id="55ece-122">If you do not have the **ITurnContext** object, but you have an **Activity** object, you can obtain the **ITurnContext** object this way:</span></span>

```csharp
BotFrameworkAdapter adapter = new BotFrameworkAdapter("", "");

await adapter.ProcessActivity(this.Request.Headers.Authorization?.Parameter,
        activity,
        async (context) =>
        {
            // Do something with context here. For example, the body of your Post() method may go here.
        });
```

## <a name="fix-assembly-conflicts"></a><span data-ttu-id="55ece-123">Solucionar los conflictos de ensamblado</span><span class="sxs-lookup"><span data-stu-id="55ece-123">Fix assembly conflicts</span></span>

<span data-ttu-id="55ece-124">Los bots fabricados con el paquete NuGet clásico entrarán en conflicto con sus ensamblados.</span><span class="sxs-lookup"><span data-stu-id="55ece-124">Bots made with the classic NuGet Package will have conflicts with their assemblies.</span></span> <span data-ttu-id="55ece-125">Esto aparecerá en la ventana Lista de errores en Visual Studio después de una compilación.</span><span class="sxs-lookup"><span data-stu-id="55ece-125">This will appear in the Error List window in Visual Studio after a build.</span></span>

### <a name="if-you-see-warning-found-conflicts-between-different-versions-of-the-same-dependent-assembly"></a><span data-ttu-id="55ece-126">Si aparece el mensaje "Warning: Found conflicts between different versions of the same dependent assembly" ("Advertencia: se encontraron conflictos entre diferentes versiones del mismo ensamblado dependiente")</span><span class="sxs-lookup"><span data-stu-id="55ece-126">If you see "Warning: Found conflicts between different versions of the same dependent assembly"</span></span>

<span data-ttu-id="55ece-127">Si encuentra una advertencia que comienza con el siguiente texto: **Found conflicts between different versions of the same dependent assembly** (Se encontraron conflictos entre versiones diferentes del mismo ensamblado dependiente):</span><span class="sxs-lookup"><span data-stu-id="55ece-127">If you find a warning that begins with the following text: **Found conflicts between different versions of the same dependent assembly**:</span></span>

- <span data-ttu-id="55ece-128">Haga doble clic en el mensaje de advertencia.</span><span class="sxs-lookup"><span data-stu-id="55ece-128">Double-click the warning message.</span></span> <span data-ttu-id="55ece-129">Aparecerá un cuadro de diálogo con la pregunta: "Do you want to fix these conflicts by adding binding redirect records in the application configuration file? (¿Quiere resolver estos conflictos agregando registros de redireccionamiento de enlace al archivo de configuración de la aplicación?").</span><span class="sxs-lookup"><span data-stu-id="55ece-129">A dialog box appears which asks, "Do you want to fix these conflicts by adding binding redirect records in the application configuration file?"</span></span>
- <span data-ttu-id="55ece-130">Haga clic en "Sí".</span><span class="sxs-lookup"><span data-stu-id="55ece-130">Click "Yes".</span></span>
- <span data-ttu-id="55ece-131">Vuelva a compilar el proyecto.</span><span class="sxs-lookup"><span data-stu-id="55ece-131">Build the project again.</span></span>

### <a name="if-you-see-error-missing-method-exception-on-startup"></a><span data-ttu-id="55ece-132">Si aparece "Error: Missing Method Exception on startup" ("Error: falta la excepción del método en el inicio")</span><span class="sxs-lookup"><span data-stu-id="55ece-132">If you see "Error: Missing Method Exception on startup"</span></span>

<span data-ttu-id="55ece-133">Hay un error con .NET Standard que parece ocurrir cuando se actualiza un proyecto de .NET 4.6 anterior a la versión 4.6.1 y, a continuación, se intenta usar una biblioteca .NET standard en él.</span><span class="sxs-lookup"><span data-stu-id="55ece-133">There is a bug with .NET Standard that seems to happen when you take an older .NET 4.6 project and upgrade to 4.6.1 and then attempt to use a .NET standard library with it.</span></span> <span data-ttu-id="55ece-134">Básicamente, hay dos ensamblados System.Net.Http distintos que intentan intercambiarse de forma dinámica. La solución alternativa es agregar una redirección de enlace al archivo Web.config para System.Net.Http.</span><span class="sxs-lookup"><span data-stu-id="55ece-134">Essentially, there are 2 different System.Net.Http assemblies that try to dynamic swap out. The workaround is to add a binding redirect to your Web.config for System.Net.Http.</span></span> 

<span data-ttu-id="55ece-135">Si recibe este error, agregue lo siguiente al archivo Web.config:</span><span class="sxs-lookup"><span data-stu-id="55ece-135">If you receive this error, add the follwing to your Web.config file:</span></span>

```xml
<dependentAssembly>
    <assemblyIdentity name="System.Net.Http" publicKeyToken="B03F5F7F11D50A3A" culture="neutral" />
    <bindingRedirect oldVersion="0.0.0.0-4.2.0.0" newVersion="4.2.0.0" />
</dependentAssembly>
```

<span data-ttu-id="55ece-136">Para obtener más información sobre este problema, consulte [System.Net.Http v4.2.0.0 being copied/loaded from MSBuild tooling #25773](https://github.com/dotnet/corefx/issues/25773) (Copia o carga de System.Net.Http v 4.2.0.0 a partir de las herramientas de MSBuild #25773).</span><span class="sxs-lookup"><span data-stu-id="55ece-136">For further details on this issue, see [System.Net.Http v4.2.0.0 being copied/loaded from MSBuild tooling #25773](https://github.com/dotnet/corefx/issues/25773).</span></span>

## <a name="sample-of-a-converted-bot"></a><span data-ttu-id="55ece-137">Ejemplo de un bot convertido</span><span class="sxs-lookup"><span data-stu-id="55ece-137">Sample of a converted bot</span></span>

<span data-ttu-id="55ece-138">Para un bot que ya se ha convertido, puede mirar el ejemplo [EchoBot-Classic](https://github.com/Microsoft/botbuilder-dotnet/tree/master/samples/Microsoft.Bot.Samples.EchoBot-Classic) que muestra un bot de la versión 3.x convertido para que funcione con la versión 4.0.</span><span class="sxs-lookup"><span data-stu-id="55ece-138">For a bot that has already been converted, you can check out the [EchoBot-Classic](https://github.com/Microsoft/botbuilder-dotnet/tree/master/samples/Microsoft.Bot.Samples.EchoBot-Classic) sample which shows a 3.x bot converted to work with 4.0.</span></span>

## <a name="limitations"></a><span data-ttu-id="55ece-139">Limitaciones</span><span class="sxs-lookup"><span data-stu-id="55ece-139">Limitations</span></span>
<span data-ttu-id="55ece-140">Microsoft.Bot.Builder.Classic es solo una biblioteca de .NET 4.61 y no funciona en .NET Core.</span><span class="sxs-lookup"><span data-stu-id="55ece-140">The Microsoft.Bot.Builder.Classic is a .NET 4.61 library only, it does not work on .NET core.</span></span>
