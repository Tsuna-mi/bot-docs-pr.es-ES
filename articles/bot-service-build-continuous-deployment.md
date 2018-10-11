---
title: Configuración de implementación continua para Bot Service | Microsoft Docs
description: Obtenga información sobre cómo configurar una implementación continua desde el control de código fuente para una instancia de Bot Service.
keywords: implementación continua, publicar, implementar, azure portal
author: ivorb
ms.author: v-ivorb
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 09/18/2018
ms.openlocfilehash: 45c89c911106d5b6a1e250f6e6ab3d472c90ab92
ms.sourcegitcommit: 3bf3dbb1a440b3d83e58499c6a2ac116fe04b2f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/23/2018
ms.locfileid: "46707991"
---
# <a name="set-up-continuous-deployment"></a><span data-ttu-id="df902-104">Configurar la implementación continua</span><span class="sxs-lookup"><span data-stu-id="df902-104">Set up continuous deployment</span></span>
<span data-ttu-id="df902-105">Si el código está insertado en **GitHub** o **Azure DevOps (anteriormente Visual Studio Team Services)**, use la implementación continua para implementar automáticamente los cambios de código desde el repositorio de origen en Azure.</span><span class="sxs-lookup"><span data-stu-id="df902-105">If your code is checked into **GitHub** or **Azure DevOps (formerly Visual Studio Team Services)**, use continous deployment to automatically deploy code changes from your source repository to Azure.</span></span> <span data-ttu-id="df902-106">En este tema, hablaremos sobre cómo configurar la implementación continua para **GitHub** y **Azure DevOps**.</span><span class="sxs-lookup"><span data-stu-id="df902-106">In this topic, we'll cover setting up continuous deployment for **GitHub** and **Azure DevOps**.</span></span>

> [!NOTE]
> <span data-ttu-id="df902-107">Una vez configurada la implementación continua, el editor de código en línea de Azure Portal se convierte en *SOLO LECTURA*.</span><span class="sxs-lookup"><span data-stu-id="df902-107">Once continuous deployment is set up, the online code editor in the Azure portal becomes *READ ONLY*.</span></span>

## <a name="continuous-deployment-using-github"></a><span data-ttu-id="df902-108">Implementación continua con GitHub</span><span class="sxs-lookup"><span data-stu-id="df902-108">Continuous deployment using GitHub</span></span>

<span data-ttu-id="df902-109">Para configurar una implementación continua con GitHub, haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="df902-109">To set up continuous deployment using GitHub, do the following:</span></span>

1. <span data-ttu-id="df902-110">Utilice el repositorio de GitHub que contiene el código fuente que desea implementar en Azure.</span><span class="sxs-lookup"><span data-stu-id="df902-110">Use the GitHub repository that contains the source code you want to deploy to Azure.</span></span> <span data-ttu-id="df902-111">Puede [bifurcar](https://help.github.com/articles/fork-a-repo/) un repositorio existente o crear el suyo propio y cargar el código fuente relacionado en el repositorio de GitHub.</span><span class="sxs-lookup"><span data-stu-id="df902-111">You can either [fork](https://help.github.com/articles/fork-a-repo/) an existing repository or create your own and upload related source code into your GitHub repository.</span></span>
2. <span data-ttu-id="df902-112">En [Azure Portal](https://portal.azure.com), vaya a la hoja **Compilar** del bot y haga clic en **Configurar la implementación continua**.</span><span class="sxs-lookup"><span data-stu-id="df902-112">In the [Azure portal](https://portal.azure.com), go to your bot's **Build** blade and click **Configure continuous deployment**.</span></span> 
3. <span data-ttu-id="df902-113">Haga clic en **Configuración**.</span><span class="sxs-lookup"><span data-stu-id="df902-113">Click **Setup**.</span></span>
   
   ![Configurar implementación continua](~/media/azure-bot-build/continuous-deployment-setup.png)

4. <span data-ttu-id="df902-115">Haga clic en **Elegir origen** y seleccione **GitHub**.</span><span class="sxs-lookup"><span data-stu-id="df902-115">Click **Choose Source** and select **GitHub**.</span></span>

   ![Elija GitHub](~/media/azure-bot-build/continuous-deployment-setup-github.png)

5. <span data-ttu-id="df902-117">Haga clic en **Autorización**, luego en **Autorizar** y siga los avisos para proporcionar autorización para que Azure acceda a su cuenta de GitHub.</span><span class="sxs-lookup"><span data-stu-id="df902-117">Click **Authorization** then click the **Authorize** button and follow the prompts to give Azure authorization to access your GitHub account.</span></span>

6. <span data-ttu-id="df902-118">Haga clic en **Elegir proyecto** y elija un proyecto.</span><span class="sxs-lookup"><span data-stu-id="df902-118">Click **Choose project** and select a project.</span></span>

7. <span data-ttu-id="df902-119">Haga clic en **Elegir rama** y elija una rama.</span><span class="sxs-lookup"><span data-stu-id="df902-119">Click **Choose branch** and select a branch.</span></span>

8. <span data-ttu-id="df902-120">Haga clic en **Aceptar** para completar el proceso de configuración.</span><span class="sxs-lookup"><span data-stu-id="df902-120">Click **OK** to complete the setup process.</span></span>

<span data-ttu-id="df902-121">Ahora la configuración de la implementación continua con GitHub está completa.</span><span class="sxs-lookup"><span data-stu-id="df902-121">Now your continuous deployment with GitHub setup is complete.</span></span> <span data-ttu-id="df902-122">Cada vez que confirme en el repositorio de código fuente, los cambios se implementarán automáticamente en Azure Bot Service.</span><span class="sxs-lookup"><span data-stu-id="df902-122">Whenever you commit to the source code repository, your changes will automatically be deployed to the Azure Bot Service.</span></span>

## <a name="continuous-deployment-using-azure-devops"></a><span data-ttu-id="df902-123">Implementación continua con Azure DevOps</span><span class="sxs-lookup"><span data-stu-id="df902-123">Continuous deployment using Azure DevOps</span></span>

1. <span data-ttu-id="df902-124">En [Azure Portal](https://portal.azure.com), en la hoja **Compilar** del bot, haga clic en **Configurar la implementación continua**.</span><span class="sxs-lookup"><span data-stu-id="df902-124">In the [Azure portal](https://portal.azure.com), from your bot's **Build** blade, click **Configure continuous deployment**.</span></span> 
2. <span data-ttu-id="df902-125">Haga clic en **Configuración**.</span><span class="sxs-lookup"><span data-stu-id="df902-125">Click **Setup**.</span></span>
   
   ![Configurar implementación continua](~/media/azure-bot-build/continuous-deployment-setup.png)

3. <span data-ttu-id="df902-127">Haga clic en **Elegir origen** y seleccione **Visual Studio Team Services**.</span><span class="sxs-lookup"><span data-stu-id="df902-127">Click **Choose Source** and select **Visual Studio Team Services**.</span></span> <span data-ttu-id="df902-128">Tenga en cuenta que Visual Studio Team Services es ahora Azure DevOps Services.</span><span class="sxs-lookup"><span data-stu-id="df902-128">Please keep in mind that Visual Studio Team Services is now Azure DevOps Services.</span></span>

   ![Elija Visual Studio Team Services:](~/media/azure-bot-build/continuous-deployment-setup-vs.png)

4. <span data-ttu-id="df902-130">Haga clic en **Elegir la cuenta** y seleccione una cuenta.</span><span class="sxs-lookup"><span data-stu-id="df902-130">Click **Choose your account** and select an account.</span></span>

> [!NOTE]
> <span data-ttu-id="df902-131">Si no ve su cuenta, asegúrese de que esté vinculada a su suscripción a Azure.</span><span class="sxs-lookup"><span data-stu-id="df902-131">If you do not see your account, make sure it is linked to your Azure subscription.</span></span> <span data-ttu-id="df902-132">Para vincular la cuenta a la suscripción de Azure, vaya a Azure Portal y abra **Organizaciones de Azure DevOps Services (anteriormente Team Services)**.</span><span class="sxs-lookup"><span data-stu-id="df902-132">To link your account to your Azure subscription, go to the Azure Portal, open **Azure DevOps Services organizations (formerly Team Services)**.</span></span> <span data-ttu-id="df902-133">Verá una lista de las organizaciones que tiene en Azure DevOps.</span><span class="sxs-lookup"><span data-stu-id="df902-133">You will see a list of organizations you have in your Azure DevOps.</span></span> <span data-ttu-id="df902-134">Haga clic en la que contiene el código fuente del bot que desea implementar y encontrará un botón**Conectar AAD**.</span><span class="sxs-lookup"><span data-stu-id="df902-134">Click into the one with the source code of the bot you want to deploy, you will find a **Connect AAD** button.</span></span> <span data-ttu-id="df902-135">Si la organización que ha elegido no está vinculada a la suscripción de Azure, este botón estará activo.</span><span class="sxs-lookup"><span data-stu-id="df902-135">If the organization you choose is not linked to your Azure subscription, this button will be active.</span></span> <span data-ttu-id="df902-136">Por lo tanto, haga clic en este botón para configurar la conexión.</span><span class="sxs-lookup"><span data-stu-id="df902-136">So click this button to set up the connection.</span></span> <span data-ttu-id="df902-137">Es posible que deba esperar un poco para que surta efecto.</span><span class="sxs-lookup"><span data-stu-id="df902-137">You may need to wait some time before it takes effect.</span></span>

5. <span data-ttu-id="df902-138">Haga clic en **Elegir proyecto** y elija un proyecto.</span><span class="sxs-lookup"><span data-stu-id="df902-138">Click **Choose project** and select a project.</span></span>

> [!NOTE]
> <span data-ttu-id="df902-139">Solo se admiten proyectos VSTS Git.</span><span class="sxs-lookup"><span data-stu-id="df902-139">Only VSTS Git projects are supported.</span></span>

6. <span data-ttu-id="df902-140">Haga clic en **Elegir rama** y elija una rama.</span><span class="sxs-lookup"><span data-stu-id="df902-140">Click **Choose branch** and select a branch.</span></span>
7. <span data-ttu-id="df902-141">Haga clic en **Aceptar** para completar el proceso de configuración.</span><span class="sxs-lookup"><span data-stu-id="df902-141">Click **OK** to complete the setup process.</span></span>

   ![Configuración de Visual Studio](~/media/azure-bot-build/continuous-deployment-setup-vs-configuration.png)

<span data-ttu-id="df902-143">Ahora la configuración de la implementación continua con Azure DevOps está completa.</span><span class="sxs-lookup"><span data-stu-id="df902-143">Now your continuous deployment with Azure DevOps setup is complete.</span></span> <span data-ttu-id="df902-144">Cada vez que confirme, los cambios se implementarán automáticamente en Azure.</span><span class="sxs-lookup"><span data-stu-id="df902-144">Whenever you commit, your changes will automatically be deployed to Azure.</span></span>

## <a name="disable-continuous-deployment"></a><span data-ttu-id="df902-145">Deshabilitación de la implementación continua</span><span class="sxs-lookup"><span data-stu-id="df902-145">Disable continuous deployment</span></span>

<span data-ttu-id="df902-146">Si bien el bot está configurado para la implementación continua, no puede usar el editor de código en línea para realizar cambios en el bot.</span><span class="sxs-lookup"><span data-stu-id="df902-146">While your bot is configured for continuous deployment, you may not use the online code editor to make changes to your bot.</span></span> <span data-ttu-id="df902-147">Si desea usar el editor de código en línea, puede deshabilitar temporalmente la implementación continua.</span><span class="sxs-lookup"><span data-stu-id="df902-147">If you want to use the online code editor, you can temporarily disable continuous deployment.</span></span>

<span data-ttu-id="df902-148">Para deshabilitar la implementación continua, haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="df902-148">To disable continuous deployment, do the following:</span></span>

1. <span data-ttu-id="df902-149">Desde la hoja **Compilar** del bot, haga clic en **Configure continuous deployment** (Configurar la implementación continua).</span><span class="sxs-lookup"><span data-stu-id="df902-149">From your bot's **Build** blade, click **Configure continuous deployment**.</span></span> 
2. <span data-ttu-id="df902-150">Haga clic en **Desconectar** para deshabilitar la implementación continua.</span><span class="sxs-lookup"><span data-stu-id="df902-150">Click **Disconnect** to disable continuous deployment.</span></span> <span data-ttu-id="df902-151">Para volver a habilitar la implementación continua, repita los pasos de las secciones anteriores correspondientes.</span><span class="sxs-lookup"><span data-stu-id="df902-151">To re-enable continuous deployment, repeat the steps from the appropriate sections above.</span></span>

## <a name="additional-information"></a><span data-ttu-id="df902-152">Información adicional</span><span class="sxs-lookup"><span data-stu-id="df902-152">Additional information</span></span>
- [<span data-ttu-id="df902-153">Azure DevOps</span><span class="sxs-lookup"><span data-stu-id="df902-153">Azure DevOps</span></span>](https://docs.microsoft.com/en-us/azure/devops/?view=vsts)
