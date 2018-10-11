---
title: Creación de un nuevo proyecto con el bot de empresa | Microsoft Docs
description: Aprenda a crear un bot mediante la plantilla del bot de empresa.
author: darrenj
ms.author: darrenj
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 09/18/2018
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: 2c6736cc8aa607da73c392b04ea894a19c86ff29
ms.sourcegitcommit: 87b5b0ca9b0d5e028ece9f7cc4948c5507062c2b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "47029783"
---
# <a name="enterprise-bot-template---creating-a-new-project"></a><span data-ttu-id="364e0-103">Plantilla del bot de empresa: Creación de un nuevo proyecto</span><span class="sxs-lookup"><span data-stu-id="364e0-103">Enterprise Bot Template - Creating a new project</span></span>

> [!NOTE]
> <span data-ttu-id="364e0-104">Este tema se aplica a la versión v4 del SDK.</span><span class="sxs-lookup"><span data-stu-id="364e0-104">This topics applies to v4 version of the SDK.</span></span> 

<span data-ttu-id="364e0-105">Esta plantilla reúne todos los procedimientos recomendados y los componentes auxiliares que hemos identificado mediante la creación de experiencias de conversación.</span><span class="sxs-lookup"><span data-stu-id="364e0-105">Enterprise Bot Template brings together all of the best practices and supporting components we've identified through building of conversational experiences.</span></span> <span data-ttu-id="364e0-106">La plantilla está disponible en las siguientes plataformas del SDK Bot Builder:</span><span class="sxs-lookup"><span data-stu-id="364e0-106">The template is available in the following Botbuilder SDK platforms:</span></span>

- <span data-ttu-id="364e0-107">.NET</span><span class="sxs-lookup"><span data-stu-id="364e0-107">.NET</span></span>
- <span data-ttu-id="364e0-108">Node.js (próximamente)</span><span class="sxs-lookup"><span data-stu-id="364e0-108">Node.js (coming soon)</span></span>

## <a name="net"></a><span data-ttu-id="364e0-109">.NET</span><span class="sxs-lookup"><span data-stu-id="364e0-109">.NET</span></span>

<span data-ttu-id="364e0-110">La plantilla del bot de empresa está disponible para. NET, especialmente para las versiones **V4** del SDK.</span><span class="sxs-lookup"><span data-stu-id="364e0-110">The Enterprise Bot Template is available for .NET, targeting **V4** versions of the SDK.</span></span> <span data-ttu-id="364e0-111">Está disponible como un paquete [VSIX](https://docs.microsoft.com/en-us/visualstudio/extensibility/anatomy-of-a-vsix-package).</span><span class="sxs-lookup"><span data-stu-id="364e0-111">It is available as a [VSIX](https://docs.microsoft.com/en-us/visualstudio/extensibility/anatomy-of-a-vsix-package) package.</span></span> <span data-ttu-id="364e0-112">Para descargar, haga clic en el siguiente vínculo:</span><span class="sxs-lookup"><span data-stu-id="364e0-112">To download, please click the following link:</span></span>

- [<span data-ttu-id="364e0-113">Plantilla del bot de empresa para el SDK Bot Builder v4</span><span class="sxs-lookup"><span data-stu-id="364e0-113">BotBuilder SDK V4 Enterprise Bot Template</span></span>](https://aka.ms/GetEnterpriseBotTemplate)

#### <a name="prerequisites"></a><span data-ttu-id="364e0-114">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="364e0-114">Prerequisites</span></span>

- [<span data-ttu-id="364e0-115">Visual Studio 2017 o superior</span><span class="sxs-lookup"><span data-stu-id="364e0-115">Visual Studio 2017 or greater</span></span>](https://www.visualstudio.com/downloads/)
- [<span data-ttu-id="364e0-116">Cuenta de Azure</span><span class="sxs-lookup"><span data-stu-id="364e0-116">Azure account</span></span>](https://azure.microsoft.com/en-us/free/)
- [<span data-ttu-id="364e0-117">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="364e0-117">Azure PowerShell</span></span>](https://docs.microsoft.com/en-us/powershell/azure/overview?view=azurermps-6.8.1)

### <a name="install-the-template"></a><span data-ttu-id="364e0-118">Instalación de la plantilla</span><span class="sxs-lookup"><span data-stu-id="364e0-118">Install the template</span></span>

<span data-ttu-id="364e0-119">Desde el directorio de guardado, simplemente abra el paquete VSIX y la plantilla del bot de empresa se instalará en Visual Studio y estará disponible la próxima vez que la abra.</span><span class="sxs-lookup"><span data-stu-id="364e0-119">From the saved directory, simply open the VSIX package and Enterprise Bot Template will be installed into Visual Studio and made available the next time you open it.</span></span>

<span data-ttu-id="364e0-120">Para crear un nuevo proyecto de bot con la plantilla, simplemente abra Visual Studio y seleccione **Archivo** > **Nuevo** > **Proyecto** y, desde Visual C#, seleccione **Bot Framework** > Plantilla de bot de empresa.</span><span class="sxs-lookup"><span data-stu-id="364e0-120">To create a new bot project using the template, simply open Visual Studio, and select **File** > **new** > **Project**, and from Visual C#, select **Bot Framework** > Enterprise Bot Template.</span></span> <span data-ttu-id="364e0-121">Esto creará localmente un nuevo proyecto de bot que puede modificar como quiera.</span><span class="sxs-lookup"><span data-stu-id="364e0-121">This will create a new bot project locally which you can edit as you wish.</span></span> 

![Archivar nueva plantilla de proyecto](media/enterprise-template/EnterpriseBot-NewProject.png)

## <a name="deploy-your-bot"></a><span data-ttu-id="364e0-123">Implementación del bot</span><span class="sxs-lookup"><span data-stu-id="364e0-123">Deploy your Bot</span></span>

<span data-ttu-id="364e0-124">Ahora que ha creado el proyecto, el siguiente paso consiste en crear la infraestructura de Azure auxiliar y realizar la configuración e implementación que permita al bot funcionar desde el principio.</span><span class="sxs-lookup"><span data-stu-id="364e0-124">Now that your have your project created the next step is to create the supporting Azure infrastructure and perform configuration/deployment enabling the Bot to work right out of the box.</span></span> <span data-ttu-id="364e0-125">Continúe en [Implementación del bot](bot-builder-enterprise-template-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="364e0-125">Continue with [Deploy the Bot](bot-builder-enterprise-template-deployment.md).</span></span>

> <span data-ttu-id="364e0-126">Debe ejecutar este paso ya que, de lo contrario, la inicialización del bot (AppInsights) y las dependencias de LUIS no estarán disponibles.</span><span class="sxs-lookup"><span data-stu-id="364e0-126">You must run this step otherwise Bot initalization (AppInsights) and LUIS dependencies will not be available.</span></span>
## <a name="customize-your-bot"></a><span data-ttu-id="364e0-127">Personalización del bot</span><span class="sxs-lookup"><span data-stu-id="364e0-127">Customize your Bot</span></span>

<span data-ttu-id="364e0-128">Después de comprobar que ha implementado correctamente el bot desde el principio, puede personalizarlo para adaptarlo a sus necesidades y al escenario.</span><span class="sxs-lookup"><span data-stu-id="364e0-128">After you verify that you have successfully deployed the Bot out of the box, you can customize the bot for your scenario and needs.</span></span> <span data-ttu-id="364e0-129">Continúe con [Personalización del bot](bot-builder-enterprise-template-customize.md).</span><span class="sxs-lookup"><span data-stu-id="364e0-129">Continue with [Customize the Bot](bot-builder-enterprise-template-customize.md).</span></span>
