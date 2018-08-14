---
title: Configuración de implementación continua para Bot Service | Microsoft Docs
description: Obtenga información sobre cómo configurar una implementación continua desde el control de código fuente para una instancia de Bot Service.
keywords: implementación continua, publicar, implementar, azure portal
author: v-ducvo
ms.author: v-ducvo
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 03/08/2018
ms.openlocfilehash: 596d264c4df72959c71ab353e5038175fc2bed31
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305096"
---
# <a name="set-up-continuous-deployment"></a><span data-ttu-id="e7dcd-104">Configurar la implementación continua</span><span class="sxs-lookup"><span data-stu-id="e7dcd-104">Set up continuous deployment</span></span>

<span data-ttu-id="e7dcd-105">La implementación continua le permite desarrollar el bot localmente.</span><span class="sxs-lookup"><span data-stu-id="e7dcd-105">Continuous deployment allows you to develop your bot locally.</span></span> <span data-ttu-id="e7dcd-106">La implementación continua es útil si su bot se inserta en un control de código fuente, como **GitHub** o **Visual Studio Team Services**.</span><span class="sxs-lookup"><span data-stu-id="e7dcd-106">Continuous deployment is useful if your bot is checked into a source control like **GitHub** or **Visual Studio Team Services**.</span></span> <span data-ttu-id="e7dcd-107">A medida que inserta los cambios en el repositorio de origen, los cambios se implementarán automáticamente en Azure.</span><span class="sxs-lookup"><span data-stu-id="e7dcd-107">As you check your changes back into your source repository, your changes will automatically be deployed to Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="e7dcd-108">Una vez configurada la implementación continua, el [editor de código en línea](bot-service-build-online-code-editor.md) se convierte en *SOLO LECTURA*.</span><span class="sxs-lookup"><span data-stu-id="e7dcd-108">Once continuous deployment is set up, the [online code editor](bot-service-build-online-code-editor.md) becomes *READ ONLY*.</span></span>

<span data-ttu-id="e7dcd-109">En este tema se muestra cómo configurar la implementación continua para **GitHub** y **Visual Studio Team Services**.</span><span class="sxs-lookup"><span data-stu-id="e7dcd-109">This topic will show you how to set up continuous deployment for **GitHub** and **Visual Studio Team Services**.</span></span>

## <a name="continuous-deployment-using-github"></a><span data-ttu-id="e7dcd-110">Implementación continua con GitHub</span><span class="sxs-lookup"><span data-stu-id="e7dcd-110">Continuous deployment using GitHub</span></span>

<span data-ttu-id="e7dcd-111">Para configurar una implementación continua con GitHub, haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="e7dcd-111">To set up continuous deployment using GitHub, do the following:</span></span>

1. <span data-ttu-id="e7dcd-112">[Bifurque](https://help.github.com/articles/fork-a-repo/) el repositorio de GitHub que contiene el código que desea implementar en Azure.</span><span class="sxs-lookup"><span data-stu-id="e7dcd-112">[Fork](https://help.github.com/articles/fork-a-repo/) the GitHub repository that contains the code you want to deploy to Azure.</span></span>
2. <span data-ttu-id="e7dcd-113">En Azure Portal, vaya a la hoja **Compilar** del bot y haga clic en **Configure continuous deployment** (Configurar la implementación continua).</span><span class="sxs-lookup"><span data-stu-id="e7dcd-113">From the Azure portal, go to your bot's **Build** blade and click **Configure continuous deployment**.</span></span> 
3. <span data-ttu-id="e7dcd-114">Haga clic en **Configuración**.</span><span class="sxs-lookup"><span data-stu-id="e7dcd-114">Click **Setup**.</span></span>
   
   ![Configurar implementación continua](~/media/azure-bot-build/continuous-deployment-setup.png)

4. <span data-ttu-id="e7dcd-116">Haga clic en **Elegir origen** y elija **GitHub**.</span><span class="sxs-lookup"><span data-stu-id="e7dcd-116">Click **Choose source** and choose **GitHub**.</span></span>

   ![Elija GitHub](~/media/azure-bot-build/continuous-deployment-setup-github.png)

5. <span data-ttu-id="e7dcd-118">Haga clic en **Autorización**, luego en **Autorizar** y siga los avisos para proporcionar autorización para que Azure acceda a su cuenta de GitHub.</span><span class="sxs-lookup"><span data-stu-id="e7dcd-118">Click **Authorization** then click the **Authorize** button and follow the prompts to give Azure authorization to access your GitHub account.</span></span>

<span data-ttu-id="e7dcd-119">La configuración de la implementación continua con GitHub está completa.</span><span class="sxs-lookup"><span data-stu-id="e7dcd-119">Your continuous deployment with GitHub setup is complete.</span></span> <span data-ttu-id="e7dcd-120">Cada vez que confirme, los cambios se implementarán automáticamente en Azure.</span><span class="sxs-lookup"><span data-stu-id="e7dcd-120">Whenever you commit, your changes will automatically be deployed to Azure.</span></span>

## <a name="continuous-deployment-using-visual-studio"></a><span data-ttu-id="e7dcd-121">Implementación continua con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e7dcd-121">Continuous deployment using Visual Studio</span></span>

1. <span data-ttu-id="e7dcd-122">Desde la hoja **Compilar** del bot, haga clic en **Configure continuous deployment** (Configurar la implementación continua).</span><span class="sxs-lookup"><span data-stu-id="e7dcd-122">From your bot's **Build** blade, click **Configure continuous deployment**.</span></span> 
2. <span data-ttu-id="e7dcd-123">Haga clic en **Configuración**.</span><span class="sxs-lookup"><span data-stu-id="e7dcd-123">Click **Setup**.</span></span>
   
   ![Configurar implementación continua](~/media/azure-bot-build/continuous-deployment-setup.png)

3. <span data-ttu-id="e7dcd-125">Haga clic en **Elegir origen** y elija **Visual Studio Team Services**.</span><span class="sxs-lookup"><span data-stu-id="e7dcd-125">Click **Choose source** and choose **Visual Studio Team Services**.</span></span>

   ![Elija Visual Studio Team Services:](~/media/azure-bot-build/continuous-deployment-setup-vs.png)

4. <span data-ttu-id="e7dcd-127">Haga clic en **elija su cuenta** y elija una cuenta.</span><span class="sxs-lookup"><span data-stu-id="e7dcd-127">Click **Choose your account** and choose an account.</span></span>

> [!NOTE]
> <span data-ttu-id="e7dcd-128">Si no ve su cuenta, asegúrese de que esté vinculada a su suscripción a Azure.</span><span class="sxs-lookup"><span data-stu-id="e7dcd-128">If you do not see your account, make sure it is linked to your Azure subscription.</span></span>
> <span data-ttu-id="e7dcd-129">Para obtener más información, consulte [Linking your VSTS account to your Azure subscription](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App#linking-your-vsts-account-to-your-azure-subscription) (Cincular su cuenta de VSTS con su suscripción a Azure).</span><span class="sxs-lookup"><span data-stu-id="e7dcd-129">For more information, see [Linking your VSTS account to your Azure subscription](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App#linking-your-vsts-account-to-your-azure-subscription).</span></span>

5. <span data-ttu-id="e7dcd-130">Haga clic en **Elegir un proyecto** y elija un proyecto.</span><span class="sxs-lookup"><span data-stu-id="e7dcd-130">Click **Choose a project** and choose a project.</span></span>

> [!NOTE]
> <span data-ttu-id="e7dcd-131">Solo se admiten proyectos VSTS Git.</span><span class="sxs-lookup"><span data-stu-id="e7dcd-131">Only VSTS Git projects are supported.</span></span>

6. <span data-ttu-id="e7dcd-132">Haga clic en **Elegir rama** y elija una rama para crear.</span><span class="sxs-lookup"><span data-stu-id="e7dcd-132">click **Choose Branch** and choose a branch to branch.</span></span>
7. <span data-ttu-id="e7dcd-133">Haga clic en **Aceptar** para completar el proceso de configuración.</span><span class="sxs-lookup"><span data-stu-id="e7dcd-133">Click **OK** to complete the setup process.</span></span>

   ![Configuración de Visual Studio](~/media/azure-bot-build/continuous-deployment-setup-vs-configuration.png)

<span data-ttu-id="e7dcd-135">La configuración de la implementación continua con Visual Studio Team Services está completa.</span><span class="sxs-lookup"><span data-stu-id="e7dcd-135">Your continuous deployment with Visual Studio Team Services setup is complete.</span></span> <span data-ttu-id="e7dcd-136">Cada vez que confirme, los cambios se implementarán automáticamente en Azure.</span><span class="sxs-lookup"><span data-stu-id="e7dcd-136">Whenever you commit, your changes will automatically be deployed to Azure.</span></span>

## <a name="disable-continuous-deployment"></a><span data-ttu-id="e7dcd-137">Deshabilitación de la implementación continua</span><span class="sxs-lookup"><span data-stu-id="e7dcd-137">Disable continuous deployment</span></span>

<span data-ttu-id="e7dcd-138">Si bien el bot está configurado para la implementación continua, no puede usar el editor de código en línea para realizar cambios en el bot.</span><span class="sxs-lookup"><span data-stu-id="e7dcd-138">While your bot is configured for continuous deployment, you may not use the online code editor to make changes to your bot.</span></span> <span data-ttu-id="e7dcd-139">Si desea usar el editor de código en línea, puede deshabilitar temporalmente la implementación continua.</span><span class="sxs-lookup"><span data-stu-id="e7dcd-139">If you want to use the online code editor, you can temporarily disable continuous deployment.</span></span>

<span data-ttu-id="e7dcd-140">Para deshabilitar la implementación continua, haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="e7dcd-140">To disable continuous deployment, do the following:</span></span>

1. <span data-ttu-id="e7dcd-141">Desde la hoja **Compilar** del bot, haga clic en **Configure continuous deployment** (Configurar la implementación continua).</span><span class="sxs-lookup"><span data-stu-id="e7dcd-141">From your bot's **Build** blade, click **Configure continuous deployment**.</span></span> 
2. <span data-ttu-id="e7dcd-142">Haga clic en **Desconectar** para deshabilitar la implementación continua.</span><span class="sxs-lookup"><span data-stu-id="e7dcd-142">Click **Disconnect** to disable continuous deployment.</span></span> <span data-ttu-id="e7dcd-143">Para volver a habilitar la implementación continua, repita los pasos de las secciones anteriores correspondientes.</span><span class="sxs-lookup"><span data-stu-id="e7dcd-143">To re-enable continuous deployment, repeat the steps from the appropriate sections above.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e7dcd-144">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="e7dcd-144">Next steps</span></span>
<span data-ttu-id="e7dcd-145">Ahora que el bot se ha configurado para la implementación continua, pruebe el código con el Chat en web en línea.</span><span class="sxs-lookup"><span data-stu-id="e7dcd-145">Now that your bot is set up for continuous deployment, test your code using the online Web Chat.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="e7dcd-146">Probar en Chat en web</span><span class="sxs-lookup"><span data-stu-id="e7dcd-146">Test in Web Chat</span></span>](bot-service-manage-test-webchat.md)
