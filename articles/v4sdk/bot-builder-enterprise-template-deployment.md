---
title: Implementación de plantillas de bot de empresa | Microsoft Docs
description: Aprenda a implementar todos los recursos de Azure auxiliares para el bot de empresa.
author: darrenj
ms.author: darrenj
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 09/18/2018
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: a4ac88872e11cd32a9de96d52dcf9e917bb3488f
ms.sourcegitcommit: 87b5b0ca9b0d5e028ece9f7cc4948c5507062c2b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "47029803"
---
# <a name="enterprise-bot-template---deploying-your-bot"></a><span data-ttu-id="c866b-103">Plantilla de bot de empresa: Implementación del bot</span><span class="sxs-lookup"><span data-stu-id="c866b-103">Enterprise Bot Template - Deploying your Bot</span></span>

> [!NOTE]
> <span data-ttu-id="c866b-104">Este tema se aplica a la versión v4 del SDK.</span><span class="sxs-lookup"><span data-stu-id="c866b-104">This topics applies to v4 version of the SDK.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="c866b-105">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="c866b-105">Prerequisites</span></span>

- <span data-ttu-id="c866b-106">Asegúrese de que el [administrador de paquetes de Node](https://nodejs.org/en/) está instalado.</span><span class="sxs-lookup"><span data-stu-id="c866b-106">Ensure the [Node Package manager](https://nodejs.org/en/) is installed.</span></span>

- <span data-ttu-id="c866b-107">Instale las herramientas de la línea de comandos (CLI) de Azure Bot Service.</span><span class="sxs-lookup"><span data-stu-id="c866b-107">Install the Azure Bot Service command line (CLI) tools.</span></span> <span data-ttu-id="c866b-108">Es importante que lo haga incluso si ya ha utilizado las herramientas anteriormente para asegurarse de que tiene las versiones más recientes.</span><span class="sxs-lookup"><span data-stu-id="c866b-108">It's important to do this even if you've used the tools before to ensure you have the latest versions.</span></span>

```shell
npm install -g ludown luis-apis qnamaker botdispatch msbot luisgen chatdown
```

- <span data-ttu-id="c866b-109">Instale las herramientas de la línea de comandos (CLI) de Azure desde [aquí](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli-windows?view=azure-cli-latest).</span><span class="sxs-lookup"><span data-stu-id="c866b-109">Install the Azure Command Line Tools (CLI) from [here](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli-windows?view=azure-cli-latest)</span></span>

- <span data-ttu-id="c866b-110">Instale la extensión de AZ para Bot Service.</span><span class="sxs-lookup"><span data-stu-id="c866b-110">Install the AZ Extension for Bot Service</span></span>
```shell
az extension add -n botservice
```

## <a name="configuration"></a><span data-ttu-id="c866b-111">Configuración</span><span class="sxs-lookup"><span data-stu-id="c866b-111">Configuration</span></span>

- <span data-ttu-id="c866b-112">Recupere su clave de creación de LUIS.</span><span class="sxs-lookup"><span data-stu-id="c866b-112">Retrieve your LUIS Authoring Key</span></span>
   - <span data-ttu-id="c866b-113">Repase [esta](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/luis-reference-regions) página de documentación para más información sobre el portal de LUIS correcto para la región en la que tiene planeado realizar la implementación.</span><span class="sxs-lookup"><span data-stu-id="c866b-113">Review [this](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/luis-reference-regions) documentation page for the correct LUIS portal for the region you plan to deploy to.</span></span> 
   - <span data-ttu-id="c866b-114">Una vez que haya iniciado sesión, haga clic en el nombre que aparece en la esquina superior derecha.</span><span class="sxs-lookup"><span data-stu-id="c866b-114">Once signed in click on your name in the top right hand corner.</span></span>
   - <span data-ttu-id="c866b-115">Seleccione Configuración y anote la clave de creación para el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="c866b-115">Choose Settings and make a note of the Authoring Key for the next step.</span></span>

## <a name="deployment"></a><span data-ttu-id="c866b-116">Implementación</span><span class="sxs-lookup"><span data-stu-id="c866b-116">Deployment</span></span>

><span data-ttu-id="c866b-117">Si tiene varias suscripciones de Azure y desea asegurarse de que la implementación selecciona la correcta, ejecute los comandos siguientes antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="c866b-117">If you have multiple Azure subscriptions and want to ensure the deployment selects the correct one, run the following commands before continuing.</span></span>

 <span data-ttu-id="c866b-118">Siga el proceso de inicio de sesión del explorador en su cuenta de Azure.</span><span class="sxs-lookup"><span data-stu-id="c866b-118">Follow the browser login process into your Azure Account</span></span>
```shell
az login
az account list
az account set --subscription "YOUR_SUBSCRIPTION_NAME"
```

<span data-ttu-id="c866b-119">Los bots de plantilla de empresa requieren las siguientes dependencias para una operación de extremo a extremo.</span><span class="sxs-lookup"><span data-stu-id="c866b-119">Enterprise Template Bots require the following dependencies for end to end operation.</span></span>
- <span data-ttu-id="c866b-120">Aplicación web de Azure</span><span class="sxs-lookup"><span data-stu-id="c866b-120">Azure Web App</span></span>
- <span data-ttu-id="c866b-121">Cuenta de Azure Storage (transcripciones)</span><span class="sxs-lookup"><span data-stu-id="c866b-121">Azure Storage Account (Transcripts)</span></span>
- <span data-ttu-id="c866b-122">Azure Application Insights (datos de telemetría)</span><span class="sxs-lookup"><span data-stu-id="c866b-122">Azure Application Insights (Telemetry)</span></span>
- <span data-ttu-id="c866b-123">Azure Cosmos Database (estado)</span><span class="sxs-lookup"><span data-stu-id="c866b-123">Azure CosmosDb (State)</span></span>
- <span data-ttu-id="c866b-124">Azure Cognitive Services: Language Understanding</span><span class="sxs-lookup"><span data-stu-id="c866b-124">Azure Cognitive Services - Language Understanding</span></span>
- <span data-ttu-id="c866b-125">Azure Cognitive Services: QnAMaker (incluido Azure Search, Azure Web App)</span><span class="sxs-lookup"><span data-stu-id="c866b-125">Azure Cognitive Services - QnAMaker (including Azure Search, Azure Web App)</span></span>
- <span data-ttu-id="c866b-126">Azure Cognitive Services: Content Moderator (paso opcional manual)</span><span class="sxs-lookup"><span data-stu-id="c866b-126">Azure Cognitive Services - Content Moderator (optional manual step)</span></span>

<span data-ttu-id="c866b-127">El nuevo proyecto de bot tiene un método de implementación que permite que el comando `msbot clone services` automatice la implementación de todos los servicios mencionados anteriormente en la suscripción de Azure y garantiza que el archivo .bot del proyecto se actualiza con todos los servicios, incluyendo las claves que permiten el funcionamiento ininterrumpido del bot.</span><span class="sxs-lookup"><span data-stu-id="c866b-127">Your new Bot project has a deployment recipe enabling the `msbot clone services` command to automate deployment of all the above services into your Azure subscription and ensure the .bot file in your project is updated with all of the services including keys enabling seamless operation of your Bot.</span></span>

> <span data-ttu-id="c866b-128">Una vez implementado, revise los planes de tarifas de los servicios creados y adáptelos a su escenario.</span><span class="sxs-lookup"><span data-stu-id="c866b-128">Once deployed, review the Pricing Tiers for the created services and adjust to suit your scenario.</span></span>

<span data-ttu-id="c866b-129">El archivo README.md del proyecto creado contiene una línea de comandos de servicios de clonación de msbot de ejemplo que se ha actualizado con el nombre del bot creado y una versión genérica como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="c866b-129">The README.md within your created project contains an example msbot clone services command line updated with your created Bot name and a generic version is shown below.</span></span> <span data-ttu-id="c866b-130">Asegúrese de que actualiza la clave de creación del paso anterior y elija la ubicación del centro de datos de Azure que desee usar (por ejemplo, westus o westeurope).</span><span class="sxs-lookup"><span data-stu-id="c866b-130">Ensure you update the authoring key from the previous step and choose the Azure datacenter location you wish to use (e.g. westus or westeurope).</span></span>

> <span data-ttu-id="c866b-131">Asegúrese de que la clave de creación de LUIS que se recuperó en el paso anterior es para la región que va a especificar a continuación.</span><span class="sxs-lookup"><span data-stu-id="c866b-131">Ensure the LUIS authoring key retrieved on the previous step is for the region you specify below.</span></span>

```shell
msbot clone services --name "YOUR_BOT_NAME" --luisAuthoringKey "YOUR_AUTHORING_KEY" --folder "DeploymentScripts\msbotClone" --location "westus"
```

<span data-ttu-id="c866b-132">La herramienta msbot resumirá el plan de implementación incluyendo la ubicación y la SKU.</span><span class="sxs-lookup"><span data-stu-id="c866b-132">The msbot tool will outline the deployment plan including location and SKU.</span></span> <span data-ttu-id="c866b-133">Asegúrese de revisarlo antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="c866b-133">Ensure you review before proceeding.</span></span>

![Confirmación de implementación](./media/enterprise-template/EnterpriseBot-ConfirmDeployment.png)

><span data-ttu-id="c866b-135">Una vez completada la implementación, es **obligatorio** tomar nota del secreto del archivo .bot proporcionado ya que lo necesitará en pasos posteriores.</span><span class="sxs-lookup"><span data-stu-id="c866b-135">After deployment is complete, it's **imperative** that you make a note of the .bot file secret provided as this will be required for later steps.</span></span>

- <span data-ttu-id="c866b-136">Actualice el archivo `appsettings.json` con el nombre y el secreto del archivo .bot recién creado.</span><span class="sxs-lookup"><span data-stu-id="c866b-136">Update your `appsettings.json` file with the newly created .bot file name and .bot file secret.</span></span>
- <span data-ttu-id="c866b-137">Ejecute el siguiente comando y recupere la clave InstrumentationKey de la instancia de Application Insights y actualícela en el archivo `appsettings.json`.</span><span class="sxs-lookup"><span data-stu-id="c866b-137">Run the following command and retrieve the InstrumentationKey for your Application Insights instance and update InstrumentationKey in your `appsettings.json` file.</span></span>

`msbot list --bot YOURBOTFILE.bot --secret YOUR_BOT_SECRET`

        {
          "botFilePath": ".\\YOURBOTFILE.bot",
          "botFileSecret": "YOUR_BOT_SECRET",
          "ApplicationInsights": {
            "InstrumentationKey": "YOUR_INSTRUMENTATION_KEY"
          }
        }

## <a name="testing"></a><span data-ttu-id="c866b-138">Prueba</span><span class="sxs-lookup"><span data-stu-id="c866b-138">Testing</span></span>

<span data-ttu-id="c866b-139">Una vez completada, ejecute el proyecto de bot dentro de su entorno de desarrollo y abra el emulador de Bot Framework.</span><span class="sxs-lookup"><span data-stu-id="c866b-139">Once complete, run your bot project within your development envrionment and open the Bot Framework Emulator.</span></span> <span data-ttu-id="c866b-140">En el emulador, elija Abrir bot en el menú Archivo y vaya al archivo .bot en el directorio.</span><span class="sxs-lookup"><span data-stu-id="c866b-140">Within the Emulator, choose Open Bot from teh File menu and navigate to the .bot file in your directory.</span></span>

<span data-ttu-id="c866b-141">A continuación, escriba ```hi``` para comprobar que todo funciona.</span><span class="sxs-lookup"><span data-stu-id="c866b-141">Then type ```hi``` to verify everything is working.</span></span>

## <a name="deploy-to-azure"></a><span data-ttu-id="c866b-142">Implementar en Azure</span><span class="sxs-lookup"><span data-stu-id="c866b-142">Deploy to Azure</span></span>

<span data-ttu-id="c866b-143">Las pruebas pueden realizarse localmente por completo.</span><span class="sxs-lookup"><span data-stu-id="c866b-143">Testing can be performed end to end locally.</span></span> <span data-ttu-id="c866b-144">Cuando esté listo para implementar el bot en Azure para realizar pruebas adicionales puede usar el comando siguiente para publicar el código fuente, esto se puede ejecutar cada vez que desee insertar actualizaciones de código fuente.</span><span class="sxs-lookup"><span data-stu-id="c866b-144">When your ready to deploy your Bot to Azure for additional testing you can use the following command to publish the source code, this can be run whenever you wish to push source code updates.</span></span>

```shell
az bot publish -g YOUR_BOT_NAME -n YOUR_BOT_NAME --proj-file YOUR_BOT_NAME.csproj --sdk-version v4
```

## <a name="enabling-more-scenarios"></a><span data-ttu-id="c866b-145">Habilitación de escenarios adicionales</span><span class="sxs-lookup"><span data-stu-id="c866b-145">Enabling more scenarios</span></span>

<span data-ttu-id="c866b-146">El proyecto de bot proporciona funcionalidad adicional que se puede habilitar mediante los pasos siguientes.</span><span class="sxs-lookup"><span data-stu-id="c866b-146">Your Bot project provides additional functionality which you can enable through the following steps.</span></span>

### <a name="authentication"></a><span data-ttu-id="c866b-147">Autenticación</span><span class="sxs-lookup"><span data-stu-id="c866b-147">Authentication</span></span>

<span data-ttu-id="c866b-148">Para habilitar la autenticación siga estos pasos después de configurar un nombre de conexión de autenticación dentro de la configuración del bot en Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="c866b-148">To enable authentication follow these steps after configuring an Authentication Connection Name within the Settings of your Bot in the Azure Portal.</span></span> <span data-ttu-id="c866b-149">Se puede encontrar más información en la [documentación](https://docs.microsoft.com/en-us/azure/bot-service/bot-builder-tutorial-authentication?view=azure-bot-service-3.0).</span><span class="sxs-lookup"><span data-stu-id="c866b-149">Further information can be found in the [documentation](https://docs.microsoft.com/en-us/azure/bot-service/bot-builder-tutorial-authentication?view=azure-bot-service-3.0).</span></span>

<span data-ttu-id="c866b-150">Registre `SignInDialog` en el constructor MainDialog:</span><span class="sxs-lookup"><span data-stu-id="c866b-150">Register the `SignInDialog` in the MainDialog constructor:</span></span>
    
`AddDialog(new SignInDialog(_services.AuthConnectionName));`

<span data-ttu-id="c866b-151">Agregue lo siguiente al código en la ubicación que desee para probar un flujo sencillo de inicio de sesión:</span><span class="sxs-lookup"><span data-stu-id="c866b-151">Add the following in your code at your desired location to test a simple login flow:</span></span>
    
`var signInResult = await dc.BeginAsync(SignInDialog.Name);`

### <a name="content-moderation"></a><span data-ttu-id="c866b-152">Moderación de contenido</span><span class="sxs-lookup"><span data-stu-id="c866b-152">Content Moderation</span></span>

<span data-ttu-id="c866b-153">La moderación de contenido puede usarse para identificar información de identificación personal (PII) y contenido para adultos en los mensajes enviados al bot.</span><span class="sxs-lookup"><span data-stu-id="c866b-153">Content moderation can be used to identify personally identifiable information (PII) and adult content in the messages sent to the bot.</span></span> <span data-ttu-id="c866b-154">Para habilitar esta funcionalidad, vaya a Azure Portal y cree un nuevo servicio Content Moderator.</span><span class="sxs-lookup"><span data-stu-id="c866b-154">To enable this functionality, go to the azure portal and create a new content moderator service.</span></span> <span data-ttu-id="c866b-155">Recopile su clave de suscripción y región para configurar el archivo .bot.</span><span class="sxs-lookup"><span data-stu-id="c866b-155">Collect your subscription key and region to configure your .bot file.</span></span> 

> <span data-ttu-id="c866b-156">Este paso se automatizará en el futuro.</span><span class="sxs-lookup"><span data-stu-id="c866b-156">This step will be automated in the future.</span></span>

<span data-ttu-id="c866b-157">Agregue el siguiente código en la parte inferior del método service.AddBot<>() en el inicio para habilitar la moderación de contenido en cada turno.</span><span class="sxs-lookup"><span data-stu-id="c866b-157">Add the following code to the bottom of your service.AddBot<>() method in startup to enable content moderation on every turn.</span></span> <span data-ttu-id="c866b-158">Se puede acceder al resultado de la moderación de contenido a través del estado del bot.</span><span class="sxs-lookup"><span data-stu-id="c866b-158">The result of content moderation can be accessed via your bot state</span></span> 
    
```
    // Content Moderation Middleware (analyzes incoming messages for inappropriate content including PII, profanity, etc.)
    var moderatorService = botConfig.Services.Where(s => s.Name == ContentModeratorMiddleware.ServiceName).FirstOrDefault();
    if (moderatorService != null)
    {
        var moderator = moderatorService as GenericService;
        var moderatorKey = moderator.Configuration["subscriptionKey"];
        var moderatorRegion = moderator.Configuration["region"];
        var moderatorMiddleware = new ContentModeratorMiddleware(moderatorKey, moderatorRegion);
        options.Middleware.Add(moderatorMiddleware);
    }
```
<span data-ttu-id="c866b-159">Acceda al resultado del middleware mediante una llamada a este desde la pila de diálogos</span><span class="sxs-lookup"><span data-stu-id="c866b-159">Access the middleware result by calling this from within your dialog stack</span></span>
```     
    var cm = dc.Context.TurnState.Get<Microsoft.CognitiveServices.ContentModerator.Models.Screen>(ContentModeratorMiddleware.TextModeratorResultKey);
```

## <a name="customize-your-bot"></a><span data-ttu-id="c866b-160">Personalización del bot</span><span class="sxs-lookup"><span data-stu-id="c866b-160">Customize your Bot</span></span>

<span data-ttu-id="c866b-161">Después de comprobar que ha implementado correctamente el bot desde el principio, puede personalizarlo para adaptarlo a sus necesidades y al escenario.</span><span class="sxs-lookup"><span data-stu-id="c866b-161">After you verify that you have successfully deployed the Bot out of the box, you can customize the bot for your scenario and needs.</span></span> <span data-ttu-id="c866b-162">Continúe con [Personalización del bot](bot-builder-enterprise-template-customize.md).</span><span class="sxs-lookup"><span data-stu-id="c866b-162">Continue with [Customize the Bot](bot-builder-enterprise-template-customize.md).</span></span>
