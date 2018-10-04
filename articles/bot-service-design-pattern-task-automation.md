---
title: Creación de bots para automatizar tareas | Microsoft Docs
description: Obtenga información sobre cómo diseñar bots que realizan tareas sin intervención humana.
author: matvelloso
ms.author: mateusv
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 2/13/2018
ms.openlocfilehash: 3bf6bef805e4a86b6e070693660eb5cb20468ffd
ms.sourcegitcommit: f0b22c6286e44578c11c9f15d22b542c199f0024
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2018
ms.locfileid: "47404011"
---
# <a name="create-task-automation-bots"></a><span data-ttu-id="8bb3e-103">Creación de bots para automatizar tareas</span><span class="sxs-lookup"><span data-stu-id="8bb3e-103">Create task automation bots</span></span>

[!INCLUDE [pre-release-label](./includes/pre-release-label-v3.md)]

<span data-ttu-id="8bb3e-104">Un bot para automatizar tareas permite que el usuario complete una tarea específica o un conjunto de tareas sin la ayuda de ninguna persona.</span><span class="sxs-lookup"><span data-stu-id="8bb3e-104">A task automation bot enables the user to complete a specific task or set of tasks without any assistance from a human.</span></span> <span data-ttu-id="8bb3e-105">Este tipo de bot a menudo se asemeja a una aplicación o un sitio web típicos, ya que se comunica con el usuario principalmente a través de texto y controles de usuario avanzados.</span><span class="sxs-lookup"><span data-stu-id="8bb3e-105">This type of bot often closely resembles a typical app or website, communicating with the user primarily via rich user controls and text.</span></span> <span data-ttu-id="8bb3e-106">Puede incluir funcionalidades de comprensión del lenguaje natural para enriquecer las conversaciones con los usuarios.</span><span class="sxs-lookup"><span data-stu-id="8bb3e-106">It may have natural language understanding capabilities to enrich conversations with users.</span></span> 

## <a name="example-use-case-password-reset"></a><span data-ttu-id="8bb3e-107">Caso de uso de ejemplo: restablecimiento de contraseña</span><span class="sxs-lookup"><span data-stu-id="8bb3e-107">Example use case: password-reset</span></span>

<span data-ttu-id="8bb3e-108">Para comprender mejor la naturaleza de un bot de tareas, considere el siguiente caso de uso de ejemplo: restablecimiento de contraseña.</span><span class="sxs-lookup"><span data-stu-id="8bb3e-108">To better understand the nature of a task bot, consider an example use case: password-reset.</span></span> 

<span data-ttu-id="8bb3e-109">La empresa Contoso recibe varias llamadas al departamento de soporte técnico cada día de empleados que necesitan restablecer sus contraseñas.</span><span class="sxs-lookup"><span data-stu-id="8bb3e-109">The Contoso company receives several help desk calls each day from employees who need to reset their passwords.</span></span> <span data-ttu-id="8bb3e-110">Contoso quiere automatizar la tarea simple y repetitiva del restablecimiento de contraseña de un empleado para que los empleados del departamento de soporte técnico puedan dedicar su tiempo a abordar los problemas más complejos.</span><span class="sxs-lookup"><span data-stu-id="8bb3e-110">Contoso wants to automate the simple, repeatable task of resetting a employee's password so that help desk agents can devote their time to addressing more complex issues.</span></span> 

<span data-ttu-id="8bb3e-111">José, un desarrollador experimentado de Contoso, decide crear un bot para automatizar la tarea de restablecimiento de contraseña.</span><span class="sxs-lookup"><span data-stu-id="8bb3e-111">John, an experienced developer from Contoso, decides to create a bot to automate the password-reset task.</span></span> <span data-ttu-id="8bb3e-112">Comienza escribiendo una especificación de diseño para el bot, de la misma forma que lo haría si estuviera creando una nueva aplicación o sitio web.</span><span class="sxs-lookup"><span data-stu-id="8bb3e-112">He begins by writing a design specification for the bot, just as he would do if he were creating a new app or website.</span></span> 

::: moniker range="azure-bot-service-3.0"

### <a name="navigation-model"></a><span data-ttu-id="8bb3e-113">Modelo de navegación</span><span class="sxs-lookup"><span data-stu-id="8bb3e-113">Navigation model</span></span>

<span data-ttu-id="8bb3e-114">La especificación define el modelo de navegación:</span><span class="sxs-lookup"><span data-stu-id="8bb3e-114">The specification defines the navigation model:</span></span>

![Estructura del diálogo](~/media/bot-service-design-pattern-task-automation/simple-task1.png)

<span data-ttu-id="8bb3e-116">El usuario comienza en `RootDialog`.</span><span class="sxs-lookup"><span data-stu-id="8bb3e-116">The user begins at the `RootDialog`.</span></span> <span data-ttu-id="8bb3e-117">Cuando solicite un restablecimiento de contraseña,</span><span class="sxs-lookup"><span data-stu-id="8bb3e-117">When they request a password reset, they</span></span>  
<span data-ttu-id="8bb3e-118">se le llevará a `ResetPasswordDialog`.</span><span class="sxs-lookup"><span data-stu-id="8bb3e-118">will be directed to the `ResetPasswordDialog`.</span></span> <span data-ttu-id="8bb3e-119">En `ResetPasswordDialog`, el bot solicitará que el usuario escriba dos datos: el número de teléfono y la fecha de nacimiento.</span><span class="sxs-lookup"><span data-stu-id="8bb3e-119">With the `ResetPasswordDialog`, the bot will prompt the user for two pieces of information: phone number and birth date.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="8bb3e-120">El diseño del bot descrito en este artículo está pensado únicamente con fines de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="8bb3e-120">The bot design described in this article is intended for example purposes only.</span></span> <span data-ttu-id="8bb3e-121">En escenarios del mundo real, un bot de restablecimiento de contraseña probablemente implementará un proceso de verificación de identidad más sólido.</span><span class="sxs-lookup"><span data-stu-id="8bb3e-121">In real-world scenarios, a password-reset bot would likely implement a more robust identity verification process.</span></span>

### <a name="dialogs"></a><span data-ttu-id="8bb3e-122">Diálogos</span><span class="sxs-lookup"><span data-stu-id="8bb3e-122">Dialogs</span></span>

<span data-ttu-id="8bb3e-123">A continuación, la especificación describe la apariencia y funcionalidad de cada cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="8bb3e-123">Next, the specification describes the appearance and functionality of each dialog.</span></span> 

#### <a name="root-dialog"></a><span data-ttu-id="8bb3e-124">Cuadro de diálogo raíz</span><span class="sxs-lookup"><span data-stu-id="8bb3e-124">Root dialog</span></span>

<span data-ttu-id="8bb3e-125">El cuadro de diálogo raíz ofrece dos opciones al usuario:</span><span class="sxs-lookup"><span data-stu-id="8bb3e-125">The root dialog provides the user with two options:</span></span> 

1. <span data-ttu-id="8bb3e-126">**Cambiar contraseña**, que se utiliza para escenarios en los cuales el usuario conoce la contraseña actual y simplemente quiere cambiarla.</span><span class="sxs-lookup"><span data-stu-id="8bb3e-126">**Change Password** is for scenarios where the user knows their current password and simply wants to change it.</span></span>
2. <span data-ttu-id="8bb3e-127">**Restablecer contraseña**, que se utiliza para escenarios en los cuales el usuario ha olvidado o ha perdido su contraseña y debe generar una nueva.</span><span class="sxs-lookup"><span data-stu-id="8bb3e-127">**Reset Password** is for scenarios where the user has forgotten or misplaced their password and needs to generate a new one.</span></span>

> [!NOTE]
> <span data-ttu-id="8bb3e-128">Por motivos de simplicidad, este artículo solo describe el flujo para **restablecer la contraseña**.</span><span class="sxs-lookup"><span data-stu-id="8bb3e-128">For simplicity, this article describes only the **reset password** flow.</span></span>

<span data-ttu-id="8bb3e-129">La especificación describe el cuadro de diálogo raíz, como se muestra en la siguiente captura de pantalla.</span><span class="sxs-lookup"><span data-stu-id="8bb3e-129">The specification describes the root dialog as shown in the following screenshot.</span></span>

![Estructura del diálogo](~/media/bot-service-design-pattern-task-automation/simple-task2.png)

#### <a name="resetpassword-dialog"></a><span data-ttu-id="8bb3e-131">Cuadro de diálogo ResetPassword</span><span class="sxs-lookup"><span data-stu-id="8bb3e-131">ResetPassword dialog</span></span>

<span data-ttu-id="8bb3e-132">Cuando el usuario elige **Restablecer contraseña** desde el cuadro de diálogo raíz, se invoca el cuadro de diálogo `ResetPassword`.</span><span class="sxs-lookup"><span data-stu-id="8bb3e-132">When the user chooses **Reset Password** from the root dialog, the `ResetPassword` dialog is invoked.</span></span> 
<span data-ttu-id="8bb3e-133">A continuación, el cuadro de diálogo `ResetPassword` invoca otros dos cuadros de diálogo.</span><span class="sxs-lookup"><span data-stu-id="8bb3e-133">The `ResetPassword` dialog then invokes two other dialogs.</span></span> 
<span data-ttu-id="8bb3e-134">Primero, invoca el cuadro de diálogo `PromptStringRegex` para recopilar el número de teléfono del usuario.</span><span class="sxs-lookup"><span data-stu-id="8bb3e-134">First, it invokes the `PromptStringRegex` dialog to collect the user's phone number.</span></span> 
<span data-ttu-id="8bb3e-135">Luego, invoca el cuadro de diálogo `PromptDate` para recopilar la fecha de nacimiento del usuario.</span><span class="sxs-lookup"><span data-stu-id="8bb3e-135">Then it invokes the `PromptDate` dialog to collect the user's date of birth.</span></span> 

> [!NOTE]
> <span data-ttu-id="8bb3e-136">En este ejemplo, José decide implementar la lógica para recopilar el número de teléfono y la fecha de nacimiento del usuario mediante dos cuadros de diálogo independientes.</span><span class="sxs-lookup"><span data-stu-id="8bb3e-136">In this example, John chose to implement the logic for collecting the user's phone number and date of birth by using two separate dialogs.</span></span> <span data-ttu-id="8bb3e-137">El enfoque no solo simplifica el código necesario para cada cuadro de diálogo, sino que también aumenta las probabilidades de que estos cuadros de diálogo se puedan utilizar en otros escenarios en el futuro.</span><span class="sxs-lookup"><span data-stu-id="8bb3e-137">The approach not only simplifies the code required for each dialog, but also increases the odds of these dialogs being usable by other scenarios in the future.</span></span> 

<span data-ttu-id="8bb3e-138">La especificación describe el cuadro de diálogo `ResetPassword`.</span><span class="sxs-lookup"><span data-stu-id="8bb3e-138">The specification describes the `ResetPassword` dialog.</span></span>

![Estructura del diálogo](~/media/bot-service-design-pattern-task-automation/simple-task3.png)

#### <a name="promptstringregex-dialog"></a><span data-ttu-id="8bb3e-140">Cuadro de diálogo PromptStringRegex</span><span class="sxs-lookup"><span data-stu-id="8bb3e-140">PromptStringRegex dialog</span></span>

<span data-ttu-id="8bb3e-141">El cuadro de diálogo `PromptStringRegex` pide al usuario que escriba su número de teléfono y verifique que coincida con el formato esperado.</span><span class="sxs-lookup"><span data-stu-id="8bb3e-141">The `PromptStringRegex` dialog prompts the user to enter their phone number, and verifies that the phone number that the user provides matches the expected format.</span></span> 
<span data-ttu-id="8bb3e-142">También tiene en cuenta el escenario en el cual el usuario proporciona varias veces una entrada no válida.</span><span class="sxs-lookup"><span data-stu-id="8bb3e-142">It also accounts for the scenario where the user repeatedly provides invalid input.</span></span> 
<span data-ttu-id="8bb3e-143">La especificación describe el cuadro de diálogo `PromptStringRegex`.</span><span class="sxs-lookup"><span data-stu-id="8bb3e-143">The spec describes the `PromptStringRegex` dialog.</span></span>

![Estructura del diálogo](~/media/bot-service-design-pattern-task-automation/simple-task4.png)

### <a name="prototype"></a><span data-ttu-id="8bb3e-145">Prototipo</span><span class="sxs-lookup"><span data-stu-id="8bb3e-145">Prototype</span></span>

<span data-ttu-id="8bb3e-146">Por último, la especificación proporciona un ejemplo de un usuario que se comunica con el bot para completar correctamente la tarea de restablecimiento de contraseña.</span><span class="sxs-lookup"><span data-stu-id="8bb3e-146">Finally, the spec provides an example of a user communicating with the bot to successfully complete the password-reset task.</span></span>

![Estructura del diálogo](~/media/bot-service-design-pattern-task-automation/simple-task5.png)

::: moniker-end 

## <a name="bot-app-or-website"></a><span data-ttu-id="8bb3e-148">¿Bot, aplicación o sitio web?</span><span class="sxs-lookup"><span data-stu-id="8bb3e-148">Bot, app, or website?</span></span>

<span data-ttu-id="8bb3e-149">Quizás se pregunte, si un bot de automatización de tareas se parece a una aplicación o sitio web, ¿por qué no simplemente crear una aplicación o un sitio web en su lugar?</span><span class="sxs-lookup"><span data-stu-id="8bb3e-149">You may be wondering, if a task automation bot closely resembles an app or website, why not just build an app or website instead?</span></span> <span data-ttu-id="8bb3e-150">En función del escenario concreto, la creación de una aplicación o un sitio web en lugar de un bot puede ser una opción completamente razonable.</span><span class="sxs-lookup"><span data-stu-id="8bb3e-150">Depending on your particular scenario, building an app or website instead of a bot may be an entirely reasonable choice.</span></span> <span data-ttu-id="8bb3e-151">Incluso puede insertar el bot en una aplicación, mediante el uso de la [API Direct Line de Bot Framework][directLineAPI] o el <a href="https://aka.ms/BotFramework-WebChat" target="_blank">control Chat en web</a>.</span><span class="sxs-lookup"><span data-stu-id="8bb3e-151">You may even choose to embed your bot into an app, by using the [Bot Framework Direct Line API][directLineAPI] or <a href="https://aka.ms/BotFramework-WebChat" target="_blank">Web Chat control</a>.</span></span> <span data-ttu-id="8bb3e-152">La implementación del bot en el contexto de una aplicación ofrece lo mejor de ambos mundos: una experiencia de aplicación avanzada y una experiencia de conversación, todo en un solo lugar.</span><span class="sxs-lookup"><span data-stu-id="8bb3e-152">Implementing your bot within the context of an app provides the best of both worlds: a rich app experience and a conversational experience, all in one place.</span></span> 

<span data-ttu-id="8bb3e-153">Sin embargo, en muchos casos la creación de una aplicación o sitio web puede ser mucho más compleja y más cara que la creación de un bot.</span><span class="sxs-lookup"><span data-stu-id="8bb3e-153">In many cases, however, building an app or website can be significantly more complex and more expensive than building a bot.</span></span> <span data-ttu-id="8bb3e-154">Una aplicación o sitio web a menudo deben admitir varias plataformas y clientes, el empaquetado y la implementación pueden ser procesos tediosos y lentos y la experiencia del usuario de tener que descargar e instalar una aplicación no es necesariamente ideal.</span><span class="sxs-lookup"><span data-stu-id="8bb3e-154">An app or website often needs to support multiple clients and platforms, packaging and deploying can be tedious and time-consuming processes, and the user experience of having to download and install an app is not necessarily ideal.</span></span> <span data-ttu-id="8bb3e-155">Por estas razones, un bot suele ofrecer una forma mucho más sencilla de resolver el problema en cuestión.</span><span class="sxs-lookup"><span data-stu-id="8bb3e-155">For these reasons, a bot may often provide a much simpler way of solving the problem at hand.</span></span> 

<span data-ttu-id="8bb3e-156">Además, los bots ofrecen la libertad de ampliarse y extenderse fácilmente.</span><span class="sxs-lookup"><span data-stu-id="8bb3e-156">Additionally, bots provide the freedom to easily expand and extend.</span></span> <span data-ttu-id="8bb3e-157">Por ejemplo, un desarrollador puede optar por agregar funcionalidades de lenguaje natural y de voz al bot para el restablecimiento de contraseña, de modo que se pueda acceder a él a través de llamadas de audio, o bien puede agregar compatibilidad con mensajes de texto.</span><span class="sxs-lookup"><span data-stu-id="8bb3e-157">For example, a developer may choose to add natural language and speech capabilities to the password-reset bot so that it can be accessed via audio call, or she may add support for text messages.</span></span> <span data-ttu-id="8bb3e-158">La empresa puede configurar quioscos multimedia en toda la compilación y puede insertar el bot de restablecimiento de contraseña en esa experiencia.</span><span class="sxs-lookup"><span data-stu-id="8bb3e-158">The company may setup kiosks throughout the building and embed the password-reset bot into that experience.</span></span>

::: moniker range="azure-bot-service-3.0"
<!-- TODO: SimpleTaskAutomation no longer exists
## Sample code

For a complete sample that shows how to implement simple task automation using the Bot Builder SDK for .NET, see the <a href="https://aka.ms/capability-SimpleTaskAutomation" target="_blank">Simple Task Automation sample</a> in GitHub.

For a complete sample that shows how to implement simple task automation using the Bot Builder SDK for Node.js, see the <a href="https://aka.ms/capability-SimpleTaskAutomation" target="_blank">Simple Task Automation sample</a> in GitHub.
-->

## <a name="additional-resources"></a><span data-ttu-id="8bb3e-159">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="8bb3e-159">Additional resources</span></span>

- [<span data-ttu-id="8bb3e-160">Diálogos</span><span class="sxs-lookup"><span data-stu-id="8bb3e-160">Dialogs</span></span>](~/dotnet/bot-builder-dotnet-dialogs.md)
- [<span data-ttu-id="8bb3e-161">Administración del flujo de conversación con diálogos (.NET)</span><span class="sxs-lookup"><span data-stu-id="8bb3e-161">Manage conversation flow with dialogs (.NET)</span></span>](~/dotnet/bot-builder-dotnet-manage-conversation-flow.md)
- <span data-ttu-id="8bb3e-162">[Manage conversation flow with dialogs (Node.js)](~/nodejs/bot-builder-nodejs-manage-conversation-flow.md) (Administración del flujo de conversación con cuadros de diálogo [Node.js])</span><span class="sxs-lookup"><span data-stu-id="8bb3e-162">[Manage conversation flow with dialogs (Node.js)](~/nodejs/bot-builder-nodejs-manage-conversation-flow.md)</span></span>

::: moniker-end

[directLineAPI]: https://docs.botframework.com/en-us/restapi/directline3/#navtitle
