---
title: Cómo funciona Bot Service | Microsoft Docs
description: Conozca las características y funcionalidades de Bot Service.
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: 3b238fee9bf0c08f1bd4c8feb1cf6b379294ecfc
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305888"
---
# <a name="how-bot-service-works"></a><span data-ttu-id="3999e-103">Cómo funciona Bot Service</span><span class="sxs-lookup"><span data-stu-id="3999e-103">How Bot Service works</span></span>

<span data-ttu-id="3999e-104">Bot Service ofrece los componentes principales para la creación de bots, incluido Bot Builder SDK para desarrollar bots y Bot Framework para conectar bots a canales.</span><span class="sxs-lookup"><span data-stu-id="3999e-104">Bot Service provides the core components for creating bots, including the Bot Builder SDK for developing bots and the Bot Framework for connecting bots to channels.</span></span> <span data-ttu-id="3999e-105">Bot Service ofrece cinco plantillas entre las que puede elegir al crear sus bots con compatibilidad con .NET y Node.js.</span><span class="sxs-lookup"><span data-stu-id="3999e-105">Bot Service provides five templates you can choose from when creating your bots with support for .NET and Node.js.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3999e-106">Para usar Bot Service debe tener una suscripción a Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="3999e-106">You must have a Microsoft Azure subscription before you can use Bot Service.</span></span> <span data-ttu-id="3999e-107">Si aún no tiene una suscripción, puede registrarse para obtener una <a href="https://azure.microsoft.com/en-us/free/" target="_blank">cuenta gratuita</a>.</span><span class="sxs-lookup"><span data-stu-id="3999e-107">If you do not already have a subscription, you can register for a <a href="https://azure.microsoft.com/en-us/free/" target="_blank">free account</a>.</span></span>

## <a name="hosting-plans"></a><span data-ttu-id="3999e-108">Planes de hospedaje</span><span class="sxs-lookup"><span data-stu-id="3999e-108">Hosting plans</span></span>
<span data-ttu-id="3999e-109">Bot Service ofrece dos planes de hospedaje para los bots.</span><span class="sxs-lookup"><span data-stu-id="3999e-109">Bot Service provides two hosting plans for your bots.</span></span> <span data-ttu-id="3999e-110">Puede elegir el que se ajuste a sus necesidades.</span><span class="sxs-lookup"><span data-stu-id="3999e-110">You can choose a hosting plan that suits your needs.</span></span>

### <a name="app-service-plan"></a><span data-ttu-id="3999e-111">Plan de App Service</span><span class="sxs-lookup"><span data-stu-id="3999e-111">App Service plan</span></span>

<span data-ttu-id="3999e-112">Un bot que usa un plan de App Service es una aplicación web de Azure estándar que se puede configurar para asignar una capacidad predefinida con costos y escalado predecibles.</span><span class="sxs-lookup"><span data-stu-id="3999e-112">A bot that uses an App Service plan is a standard Azure web app you can set to allocate a predefined capacity with predictable costs and scaling.</span></span> <span data-ttu-id="3999e-113">Con un bot que usa este plan de hospedaje, puede:</span><span class="sxs-lookup"><span data-stu-id="3999e-113">With a bot that uses this hosting plan, you can:</span></span>

* <span data-ttu-id="3999e-114">Editar código fuente de bot en línea con un editor de código en el explorador avanzado.</span><span class="sxs-lookup"><span data-stu-id="3999e-114">Edit bot source code online using an advanced in-browser code editor.</span></span>
* <span data-ttu-id="3999e-115">Descargar, depurar y volver a publicar el bot de C# con Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3999e-115">Download, debug, and re-publish your C# bot using Visual Studio.</span></span>
* <span data-ttu-id="3999e-116">Configurar la implementación continua de forma fácil para Visual Studio Online y Github.</span><span class="sxs-lookup"><span data-stu-id="3999e-116">Set up continuous deployment easily for Visual Studio Online and Github.</span></span>
* <span data-ttu-id="3999e-117">Usar código de ejemplo preparado para Bot Builder SDK.</span><span class="sxs-lookup"><span data-stu-id="3999e-117">Use sample code prepared for the Bot Builder SDK.</span></span>

### <a name="consumption-plan"></a><span data-ttu-id="3999e-118">Plan de consumo</span><span class="sxs-lookup"><span data-stu-id="3999e-118">Consumption plan</span></span>
<span data-ttu-id="3999e-119">Un bot que usa un plan de consumo es un bot sin servidor que se ejecuta en <a href="http://go.microsoft.com/fwlink/?linkID=747839" target="_blank">Azure Functions</a>, y usa los precios de pago por ejecución de Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="3999e-119">A bot that uses a Consumption plan is a serverless bot that runs on <a href="http://go.microsoft.com/fwlink/?linkID=747839" target="_blank">Azure Functions</a>, and uses the pay-per-run Azure Functions pricing.</span></span> <span data-ttu-id="3999e-120">Un bot que usa este plan de hospedaje puede escalarse para administrar picos enormes de tráfico.</span><span class="sxs-lookup"><span data-stu-id="3999e-120">A bot that uses this hosting plan can scale to handle huge traffic spikes.</span></span> <span data-ttu-id="3999e-121">Puede editar el código fuente de bot en línea con un editor de código en el explorador básico.</span><span class="sxs-lookup"><span data-stu-id="3999e-121">You can edit bot source code online using a basic in-browser code editor.</span></span> <span data-ttu-id="3999e-122">Para más información sobre el entorno de ejecución de un bot de plan de consumo, consulte <a target='_blank' href='/azure/azure-functions/functions-scale'>Planes de consumo y de App Service de Azure Functions</a>.</span><span class="sxs-lookup"><span data-stu-id="3999e-122">For more information about the runtime environment of a Consumption plan bot, see <a target='_blank' href='/azure/azure-functions/functions-scale'>Azure Functions Consumption and App Service plans</a>.</span></span>

## <a name="templates"></a><span data-ttu-id="3999e-123">Plantillas</span><span class="sxs-lookup"><span data-stu-id="3999e-123">Templates</span></span>

<span data-ttu-id="3999e-124">Bot Service le permite crear rápida y fácilmente un bot en C# o Node.js mediante cinco plantillas.</span><span class="sxs-lookup"><span data-stu-id="3999e-124">Bot Service enables you to quickly and easily create a bot in either C# or Node.js by using one of five templates.</span></span>

[!INCLUDE [Bot Service templates](~/includes/snippet-abs-templates.md)]

<span data-ttu-id="3999e-125">[Más información](bot-service-concept-templates.md) sobre las diferentes plantillas y cómo pueden ayudarle a crear bots.</span><span class="sxs-lookup"><span data-stu-id="3999e-125">[Learn more](bot-service-concept-templates.md) about the different templates and how they can help you build bots.</span></span>

## <a name="develop-and-deploy"></a><span data-ttu-id="3999e-126">Desarrollo e implementación</span><span class="sxs-lookup"><span data-stu-id="3999e-126">Develop and deploy</span></span>

<span data-ttu-id="3999e-127">De forma predeterminada, Bot Service le permite desarrollar su bot directamente en el explorador con el editor de código en línea, sin necesidad de una cadena de herramientas.</span><span class="sxs-lookup"><span data-stu-id="3999e-127">By default, Bot Service enables you to develop your bot directly in the browser using the Online Code Editor, without any need for a tool chain.</span></span> 

<span data-ttu-id="3999e-128">Puede desarrollar y depurar el bot localmente con Bot Builder SDK y un IDE como Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="3999e-128">You can develop and debug your bot locally with the Bot Builder SDK and an IDE, such as Visual Studio 2017.</span></span> <span data-ttu-id="3999e-129">Puede publicar su bot directamente en Azure con Visual Studio 2017 o la CLI de Azure.</span><span class="sxs-lookup"><span data-stu-id="3999e-129">You can publish your bot directly to Azure using Visual Studio 2017 or the Azure CLI.</span></span> <span data-ttu-id="3999e-130">También puede [configurar la implementación continua](bot-service-continuous-deployment.md) con el sistema de control de código fuente de su elección, como VSTS o GitHub.</span><span class="sxs-lookup"><span data-stu-id="3999e-130">You can also [set up continuous deployment](bot-service-continuous-deployment.md) with the source control system of your choice, such as VSTS or GitHub.</span></span> <span data-ttu-id="3999e-131">Con la implementación continua configurada, puede desarrollar y depurar en un IDE en el equipo local, y todos los cambios en el código que se confirmen en el control de código fuente se implementarán automáticamente en Azure.</span><span class="sxs-lookup"><span data-stu-id="3999e-131">With continuous deployment configured, you can develop and debug in an IDE on your local computer, and any code changes that you commit to source control automatically deploy to Azure.</span></span>  

> [!TIP]
> <span data-ttu-id="3999e-132">Para evitar conflictos, después de habilitar la implementación continua asegúrese de modificar el código únicamente mediante esta implementación y no por medio de otros mecanismos.</span><span class="sxs-lookup"><span data-stu-id="3999e-132">After enabling continuous deployment, be sure to modify your code through continuous deployment only and not through other mechanisms to avoid conflict.</span></span>

## <a name="manage-your-bot"></a><span data-ttu-id="3999e-133">Administración de bots</span><span class="sxs-lookup"><span data-stu-id="3999e-133">Manage your bot</span></span> 

<span data-ttu-id="3999e-134">Durante el proceso de creación de un bot con Bot Service, especificará un nombre para el bot, configurará su plan de hospedaje, elegirá un plan de tarifa y configurará algunas otras opciones.</span><span class="sxs-lookup"><span data-stu-id="3999e-134">During the process of creating a bot using Bot Service, you specify a name for your bot, set up its hosting plan, choose a pricing tier, and configure some other settings.</span></span> <span data-ttu-id="3999e-135">Una vez creado el bot, puede cambiar su configuración, configurarlo para ejecutarse en uno o más canales y probarlo en Chat en web.</span><span class="sxs-lookup"><span data-stu-id="3999e-135">After your bot has been created, you can change its settings, configure it to run on one or more channels, and test it with in Web Chat.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="3999e-136">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="3999e-136">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="3999e-137">Creación de un bot con Bot Service</span><span class="sxs-lookup"><span data-stu-id="3999e-137">Create a bot with Bot Service</span></span>](bot-service-quickstart.md)