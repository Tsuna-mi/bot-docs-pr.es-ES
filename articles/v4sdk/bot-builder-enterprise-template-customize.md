---
title: Personalización de bots de empresa | Microsoft Docs
description: Aprenda a personalizar el blob creado mediante la plantilla de empresa.
author: darrenj
ms.author: darrenj
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 09/18/2018
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: 6fc7f73d406c1bbbc2b2671c9336df6bda10ade6
ms.sourcegitcommit: 87b5b0ca9b0d5e028ece9f7cc4948c5507062c2b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "47029763"
---
# <a name="enterprise-bot-template---customize-your-bot"></a><span data-ttu-id="f9d10-103">Plantilla de bot de empresa: personalización del bot</span><span class="sxs-lookup"><span data-stu-id="f9d10-103">Enterprise Bot Template - Customize your Bot</span></span>

> [!NOTE]
> <span data-ttu-id="f9d10-104">Este tema se aplica a las versiones v4 del SDK.</span><span class="sxs-lookup"><span data-stu-id="f9d10-104">This topics applies to v4 version of the SDK.</span></span> 

## <a name="net"></a><span data-ttu-id="f9d10-105">.NET</span><span class="sxs-lookup"><span data-stu-id="f9d10-105">.NET</span></span>
<span data-ttu-id="f9d10-106">Después de haber implementado y comprobado que la plantilla de bot de empresa funciona de un extremo a otro como se muestra [aquí](bot-builder-enterprise-template-deployment.md) en las instrucciones, puede personalizar fácilmente el bot según su escenario y sus necesidades.</span><span class="sxs-lookup"><span data-stu-id="f9d10-106">After you have deployed and tested that the Enterprise Bot Template works end-to-end as listed in the instructions [here](bot-builder-enterprise-template-deployment.md), you can easily customize your bot based on your scenario and needs.</span></span> <span data-ttu-id="f9d10-107">El objetivo de la plantilla es proporcionar una base sólida sobre la que crear su experiencia conversacional.</span><span class="sxs-lookup"><span data-stu-id="f9d10-107">The goal of the template is to provide a solid foundation upon which to build your conversational experience.</span></span>

## <a name="project-structure"></a><span data-ttu-id="f9d10-108">Estructura del proyecto</span><span class="sxs-lookup"><span data-stu-id="f9d10-108">Project Structure</span></span>

<span data-ttu-id="f9d10-109">La estructura de carpeta del bot se muestra a continuación y representa nuestro procedimiento recomendado para estructurar el proyecto del bot y procesar los mensajes entrantes.</span><span class="sxs-lookup"><span data-stu-id="f9d10-109">The folder structure of your Bot is shown below and represents our recommended best practice for structuring your Bot project and processing incoming messages.</span></span>

    | - YourBot.bot         // The .bot file containing all of your Bot configuration including dependencies
    | - README.md           // README file containing links to documentation
    | - Program.cs          // Default Program.cs file
    | - Startup.cs          // Core Bot Initialisation including Bot Configuration LUIS, Dispatcher, etc. 
    | - <BOTNAME>State.cs   // The Root State class for your Bot
    | - appsettings.json    // References above .bot file for Configuration information. App Insights key
    | - CognitiveModels     
        | - LUIS            // .LU file containing base conversational intents (Greeting, Help, Cancel)
        | - QnA             // .LU file containing example QnA items
    | - DeploymentScripts   // msbot clone recipe for deployment
    | - Dialogs             // All Bot dialogs sit under this folder
        | - Main            // Root Dialog for all messages
            | - MainDialog.cs       // Dialog Logic
            | - MainResponses.cs    // Dialog responses
            | - Resources           // Adaptive Card JSON, Resource File
        | - Onboarding
            | - OnboardingDialog.cs       // Onboarding dialog Logic
            | - OnboardingResponses.cs    // Onboarding dialog responses
            | - OnboardingState.cs        // Localised dialog state
            | - Resources                 Resource File
        | - Cancel
        | - Escalate
        | - Signin
    | - Middleware          // Telemetry, Content Moderator
    | - ServiceClients      // SDK libraries, example GraphClient provided for Auth example
   
## <a name="update-introduction-message"></a><span data-ttu-id="f9d10-110">Actualización del mensaje de introducción</span><span class="sxs-lookup"><span data-stu-id="f9d10-110">Update Introduction Message</span></span>

<span data-ttu-id="f9d10-111">El mensaje de introducción hace uso de una [tarjeta adaptable](https://www.adaptivecards.io).</span><span class="sxs-lookup"><span data-stu-id="f9d10-111">The Introduction message makes use of an [Adaptive Card](https://www.adaptivecards.io).</span></span> <span data-ttu-id="f9d10-112">Para personalizar esta introducción del bot, puede encontrar el archivo JSON llamado ```Intro.json``` dentro de la carpeta Dialogs/Main/Resources.</span><span class="sxs-lookup"><span data-stu-id="f9d10-112">To customize this introduction for your bot you can find the JSON file within the Dialogs/Main/Resources folder called ```Intro.json```.</span></span> <span data-ttu-id="f9d10-113">Use el [visualizador de tarjetas adaptables](http://adaptivecards.io/visualizer) para modificar la tarjeta adaptable de acuerdo con las necesidades del bot.</span><span class="sxs-lookup"><span data-stu-id="f9d10-113">Use the [Adaptive Card visualizer](http://adaptivecards.io/visualizer) to modify the Adaptive Card to suit your Bot's requirements.</span></span>

## <a name="update-bot-responses"></a><span data-ttu-id="f9d10-114">Actualización de las respuestas del bot</span><span class="sxs-lookup"><span data-stu-id="f9d10-114">Update Bot Responses</span></span>

<span data-ttu-id="f9d10-115">Cada diálogo del proyecto tiene un conjunto de respuestas que se almacenan en archivos de recursos complementarios (.resx).</span><span class="sxs-lookup"><span data-stu-id="f9d10-115">Each Dialog within the project has a set of responses that are stored within supporting resource (.resx) files.</span></span> <span data-ttu-id="f9d10-116">Estos archivos se pueden encontrar en la carpeta de recursos, debajo de cada diálogo.</span><span class="sxs-lookup"><span data-stu-id="f9d10-116">These can be found within the Resources folder under each Dialog.</span></span>

<span data-ttu-id="f9d10-117">Puede cambiar las respuestas en el editor de recursos de Visual Studio como se muestra a continuación para ajustar cómo responde el bot.</span><span class="sxs-lookup"><span data-stu-id="f9d10-117">You can change the responses in the Visual Studio resource editor as shown below to adjust how your bot responds.</span></span>

![Personalización de las respuestas del bot](media/enterprise-template/EnterpriseBot-CustomisingResponses.png)

<span data-ttu-id="f9d10-119">Este enfoque es compatible con respuestas multilingües que usan el enfoque de localización de archivos de recursos estándar.</span><span class="sxs-lookup"><span data-stu-id="f9d10-119">This approach supports multi-lingual responses using the standard resource file localization approach.</span></span> <span data-ttu-id="f9d10-120">Se puede encontrar más información [aquí.](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/localization?view=aspnetcore-2.1)</span><span class="sxs-lookup"><span data-stu-id="f9d10-120">Further information can be found [here.](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/localization?view=aspnetcore-2.1)</span></span>

## <a name="updating-your-cognitive-models"></a><span data-ttu-id="f9d10-121">Actualización de los modelos cognitivos</span><span class="sxs-lookup"><span data-stu-id="f9d10-121">Updating your Cognitive Models</span></span>

<span data-ttu-id="f9d10-122">De forma predeterminada, la plantilla de empresa incluye dos modelos cognitivos: una base de conocimientos de QnAMaker con P+F de ejemplo y un modelo de LUIS para intenciones generales (saludo, ayuda, cancelar, etc.).</span><span class="sxs-lookup"><span data-stu-id="f9d10-122">There are two Cognitive Models included with the Enterprise Template by default, a sample FAQ QnAMaker Knowledge Base and a LUIS Model for General intents (Greeting, Help, Cancel, etc.).</span></span> <span data-ttu-id="f9d10-123">Estos modelos se pueden personalizar de acuerdo a sus necesidades.</span><span class="sxs-lookup"><span data-stu-id="f9d10-123">These models can be customized to suit your needs.</span></span> <span data-ttu-id="f9d10-124">También puede agregar nuevos modelos de LUIS y bases de conocimientos de QnAMaker para expandir las funcionalidades del bot.</span><span class="sxs-lookup"><span data-stu-id="f9d10-124">You can also add new LUIS models and QnAMaker knowledge bases to expand your bot's capabilities.</span></span>

### <a name="updating-an-existing-luis-model"></a><span data-ttu-id="f9d10-125">Actualización de un modelo de LUIS existente</span><span class="sxs-lookup"><span data-stu-id="f9d10-125">Updating an existing LUIS Model</span></span>
<span data-ttu-id="f9d10-126">Para actualizar un modelo de LUIS existente para la plantilla de empresa, siga estos pasos:</span><span class="sxs-lookup"><span data-stu-id="f9d10-126">To update an existing LUIS model for the Enterprise Template, perform these steps:</span></span>
1. <span data-ttu-id="f9d10-127">Realice los cambios en el modelo de LUIS en el [portal de LUIS](http://luis.ai) o mediante las herramientas de la CLI [LuDown](https://github.com/Microsoft/botbuilder-tools/tree/master/packages/Ludown) y [Luis](https://github.com/Microsoft/botbuilder-tools/tree/master/packages/LUIS).</span><span class="sxs-lookup"><span data-stu-id="f9d10-127">Make your changes to the LUIS Model in the [LUIS Portal](http://luis.ai) or using the [LuDown](https://github.com/Microsoft/botbuilder-tools/tree/master/packages/Ludown) and [Luis](https://github.com/Microsoft/botbuilder-tools/tree/master/packages/LUIS) CLI tools.</span></span> 
2. <span data-ttu-id="f9d10-128">Ejecute el siguiente comando para actualizar el modelo de distribución de modo que refleje los cambios (garantiza el enrutamiento adecuado de los mensajes):</span><span class="sxs-lookup"><span data-stu-id="f9d10-128">Run the following command to update your Dispatch model to reflect your changes (ensures proper message routing):</span></span>
```shell
    dispatch refresh --bot "YOURBOT.bot" --secret YOURSECRET
```
3. <span data-ttu-id="f9d10-129">Ejecute el siguiente comando desde la raíz del proyecto con cada modelo actualizado a fin de actualizar sus clases LuisGen asociadas:</span><span class="sxs-lookup"><span data-stu-id="f9d10-129">Run the following command from your project root for each updated model to update their associated LuisGen classes:</span></span> 
```shell
    luis export version --appId [LUIS_APP_ID] --versionId [LUIS_APP_VERSION] --authoringKey [YOUR_LUIS_AUTHORING_KEY] | luisgen - -cs [CS_FILE_NAME] -o "\Dialogs\Shared\Resources"
```

### <a name="updating-an-existing-qnamaker-knowledge-base"></a><span data-ttu-id="f9d10-130">Actualización de una base de conocimiento de QnAMaker existente</span><span class="sxs-lookup"><span data-stu-id="f9d10-130">Updating an existing QnAMaker Knowledge Base</span></span>
<span data-ttu-id="f9d10-131">Para actualizar una base de conocimientos de QnAMaker existente, realice los pasos siguientes:</span><span class="sxs-lookup"><span data-stu-id="f9d10-131">To update an existing QnAMaker Knowledge Base, perform the following steps:</span></span>
1. <span data-ttu-id="f9d10-132">Realice cambios en la base de conocimientos de QnAMaker mediante las herramientas de la CLI [LuDown](https://github.com/Microsoft/botbuilder-tools/tree/master/packages/Ludown) y [QnAMaker](https://github.com/Microsoft/botbuilder-tools/tree/master/packages/QnAMaker) o el [portal de QnAMaker](https://qnamaker.ai).</span><span class="sxs-lookup"><span data-stu-id="f9d10-132">Make changes to your QnAMaker Knowledge Base via the [LuDown](https://github.com/Microsoft/botbuilder-tools/tree/master/packages/Ludown) and [QnAMaker](https://github.com/Microsoft/botbuilder-tools/tree/master/packages/QnAMaker) CLI tools or the [QnAMaker Portal](https://qnamaker.ai).</span></span>
2. <span data-ttu-id="f9d10-133">Ejecute el siguiente comando para actualizar el modelo de distribución de modo que refleje los cambios (garantiza el enrutamiento adecuado de los mensajes):</span><span class="sxs-lookup"><span data-stu-id="f9d10-133">Run the following command to update your Dispatch model to reflect your changes (ensures proper message routing):</span></span>
```shell
    dispatch refresh --bot "YOURBOT.bot" --secret YOURSECRET
```

### <a name="adding-a-new-luis-model"></a><span data-ttu-id="f9d10-134">Incorporación de un nuevo modelo de LUIS</span><span class="sxs-lookup"><span data-stu-id="f9d10-134">Adding a new LUIS model</span></span>

<span data-ttu-id="f9d10-135">En escenarios donde quiera agregar un nuevo modelo de LUIS al proyecto, deberá actualizar la configuración del bot y el Dispatcher para tener la seguridad de que se tiene en cuenta el modelo adicional.</span><span class="sxs-lookup"><span data-stu-id="f9d10-135">In scenarios where you wish to add a new LUIS model to your project you need to update the Bot configuration and Dispatcher to ensure it is aware of the additional model.</span></span> 
1. <span data-ttu-id="f9d10-136">Cree el modelo de LUIS mediante las herramientas de la CLI LuDown/LUIS o el portal de LUIS.</span><span class="sxs-lookup"><span data-stu-id="f9d10-136">Create your LUIS model through LuDown/LUIS CLI tools or through the LUIS portal</span></span>
2. <span data-ttu-id="f9d10-137">Ejecute el comando siguiente para conectar la nueva aplicación de LUIS al archivo .bot:</span><span class="sxs-lookup"><span data-stu-id="f9d10-137">Run the following command to connect your new LUIS app to your .bot file:</span></span>
```shell
    msbot connect luis --appId [LUIS_APP_ID] --authoringKey [LUIS_AUTHORING_KEY] --subscriptionKey [LUIS_SUBSCRIPTION_KEY] 
```
3. <span data-ttu-id="f9d10-138">Agregue este nuevo modelo de LUIS al Dispatcher mediante el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="f9d10-138">Add this new LUIS model to your Dispatcher through the following command</span></span>
```shell
    dispatch add -t luis -id YOUR_LUIS_APPID -bot "YOURBOT.bot" -secret YOURSECRET
```
4. <span data-ttu-id="f9d10-139">Actualice el modelo de distribución para que refleje los cambios en el modelo de LUIS mediante el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="f9d10-139">Refresh the dispatch model to reflect the LUIS model changes through the following command</span></span>
```shell
    dispatch refresh -bot "YOURBOT.bot" -secret YOURSECRET
```

## <a name="adding-a-new-dialog"></a><span data-ttu-id="f9d10-140">Incorporación de un nuevo diálogo</span><span class="sxs-lookup"><span data-stu-id="f9d10-140">Adding a new dialog</span></span> 

<span data-ttu-id="f9d10-141">Para agregar un nuevo diálogo al bot, primero debe crear una nueva carpeta debajo de los diálogos y asegurarse de que esta clase se obtenga de `EnterpriseDialog`.</span><span class="sxs-lookup"><span data-stu-id="f9d10-141">To add a new Dialog to your Bot you need to first create a new Folder under Dialogs and ensure this class derives from `EnterpriseDialog`.</span></span> <span data-ttu-id="f9d10-142">A continuación, debe conectar la infraestructura del diálogo.</span><span class="sxs-lookup"><span data-stu-id="f9d10-142">You then need to wire up the Dialog infrastructure.</span></span> <span data-ttu-id="f9d10-143">El diálogo de incorporación muestra un ejemplo sencillo al que puede hacer referencia y, luego, se muestra un extracto junto con información general de los pasos.</span><span class="sxs-lookup"><span data-stu-id="f9d10-143">The Onboarding dialog shows a simple example which you can refer to and an excerpt is shown below alongside an overview of the steps.</span></span>

- <span data-ttu-id="f9d10-144">Agregar un diálogo de cascada al constructor</span><span class="sxs-lookup"><span data-stu-id="f9d10-144">Add a waterfall dialog to your constructor</span></span>
- <span data-ttu-id="f9d10-145">Definir los pasos de la cascada</span><span class="sxs-lookup"><span data-stu-id="f9d10-145">Define the steps for your waterfall</span></span>
- <span data-ttu-id="f9d10-146">Crear los pasos de la cascada</span><span class="sxs-lookup"><span data-stu-id="f9d10-146">Create your waterfall steps</span></span>
- <span data-ttu-id="f9d10-147">Llamar a AddDialog pasando la cascada</span><span class="sxs-lookup"><span data-stu-id="f9d10-147">Call AddDialog passing your waterfall</span></span>
- <span data-ttu-id="f9d10-148">Llamar a AddDialog pasando los avisos que se usan en la cascada</span><span class="sxs-lookup"><span data-stu-id="f9d10-148">Call AddDialog passing any prompts you use in your waterfall</span></span>
- <span data-ttu-id="f9d10-149">Establecer el InitialDialogId en el primer diálogo donde quiere que se ejecute el componente</span><span class="sxs-lookup"><span data-stu-id="f9d10-149">Set your InitialDialogId to the first dialog you want the component to run</span></span>

```
InitialDialogId = nameof(OnboardingDialog);

var onboarding = new WaterfallStep[]
{
    AskForName,
    AskForEmail,
    AskForLocation,
    FinishOnboardingDialog,
};

AddDialog(new WaterfallDialog(InitialDialogId, onboarding));
AddDialog(new TextPrompt(NamePrompt));
AddDialog(new TextPrompt(EmailPrompt));
AddDialog(new TextPrompt(LocationPrompt));
```

<span data-ttu-id="f9d10-150">A continuación, debe crear el Administrador de plantillas para gestionar las respuestas.</span><span class="sxs-lookup"><span data-stu-id="f9d10-150">Then you need to create the Template Manager to handle responses.</span></span> <span data-ttu-id="f9d10-151">Cree una clase y derívela de TemplateManager; se proporciona un ejemplo en el archivo OnboardingResponses.cs y, a continuación, se muestra un extracto.</span><span class="sxs-lookup"><span data-stu-id="f9d10-151">Create a new class and derive from TemplateManager, an example is provided in the OnboardingResponses.cs file and an excerpt is shown below.</span></span>

```
public const string _namePrompt = "namePrompt";
public const string _haveName = "haveName";
public const string _emailPrompt = "emailPrompt";
      
private static LanguageTemplateDictionary _responseTemplates = new LanguageTemplateDictionary
{
    ["default"] = new TemplateIdMap
    {
        {
            _namePrompt,
            (context, data) => OnboardingStrings.NAME_PROMPT
        },
        {
            _haveName,
            (context, data) => string.Format(OnboardingStrings.HAVE_NAME, data.name)
        },
        {
            _emailPrompt,
            (context, data) => OnboardingStrings.EMAIL_PROMPT
        },
```

<span data-ttu-id="f9d10-152">Para representar las respuestas, puede usar una instancia del Administrador de plantillas para acceder a estas respuestas mediante `ReplyWith` o `RenderTemplate` en los avisos.</span><span class="sxs-lookup"><span data-stu-id="f9d10-152">To render responses you can use a Template Manager instance to access these responses through `ReplyWith` or `RenderTemplate` for Prompts.</span></span> <span data-ttu-id="f9d10-153">A continuación se muestran ejemplos.</span><span class="sxs-lookup"><span data-stu-id="f9d10-153">Examples are shown below.</span></span>

```
Prompt = await _responder.RenderTemplate(sc.Context, "en", OnboardingResponses._namePrompt),
await _responder.ReplyWith(sc.Context, OnboardingResponses._haveName, new { name });
```

<span data-ttu-id="f9d10-154">La última pieza de la infraestructura del diálogo es la creación de una clase de estado con ámbito solo para su diálogo.</span><span class="sxs-lookup"><span data-stu-id="f9d10-154">The final piece of Dialog infrastruture is the creation of a State class scoped to your Dialog only.</span></span> <span data-ttu-id="f9d10-155">Cree una clase y asegúrese de que se deriva de `DialogState`.</span><span class="sxs-lookup"><span data-stu-id="f9d10-155">Create a new class and ensure it derives from `DialogState`</span></span>

<span data-ttu-id="f9d10-156">Una vez finalizado el diálogo, deberá agregarlo al componente `MainDialog` mediante `AddDialog`.</span><span class="sxs-lookup"><span data-stu-id="f9d10-156">Once your dialog is complete, your need to add the dialog to your `MainDialog` component using `AddDialog`.</span></span> <span data-ttu-id="f9d10-157">Para usar el nuevo diálogo, llame a `dc.BeginDialogAsync()` en el método `RouteAsync` y desencadénelo con la intención de LUIS adecuada si lo desea.</span><span class="sxs-lookup"><span data-stu-id="f9d10-157">To use your new Dialog, call `dc.BeginDialogAsync()` from within your `RouteAsync` method, triggering with the appropriate LUIS intent if desired.</span></span>

## <a name="conversational-insights-using-powerbi-dashboard-and-application-insights"></a><span data-ttu-id="f9d10-158">Conclusiones conversacionales mediante el panel de Power BI y Application Insights</span><span class="sxs-lookup"><span data-stu-id="f9d10-158">Conversational insights using PowerBI dashboard and Application Insights</span></span>
- <span data-ttu-id="f9d10-159">Para empezar a trabajar con conclusiones conversacionales, siga con la [configuración de análisis conversacionales con el panel de PowerBI](bot-builder-enterprise-template-powerbi.md).</span><span class="sxs-lookup"><span data-stu-id="f9d10-159">To get started with getting Conversational insights, continue with  [Configure conversational analytics with PowerBI dashboard](bot-builder-enterprise-template-powerbi.md).</span></span>

