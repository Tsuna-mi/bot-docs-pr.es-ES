---
title: Implementación de un bot en Azure | Microsoft Docs
description: Implemente su bot en la nube de Azure.
keywords: implementar bot, implementación de azure, registro de canal de bot, publicar visual studio
author: ivorb
ms.author: v-ivorb
manager: kamrani
ms.topic: get-started-article
ms.prod: bot-framework
ms.date: 05/14/2018
ms.openlocfilehash: f3e5ed5b14ffb81c597b10aeac45854464bad8b0
ms.sourcegitcommit: 2dc75701b169d822c9499e393439161bc87639d2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42905597"
---
# <a name="deploy-your-bot-to-azure"></a><span data-ttu-id="ad479-104">Implementación de un bot en Azure</span><span class="sxs-lookup"><span data-stu-id="ad479-104">Deploy your bot to Azure</span></span>

[!INCLUDE [pre-release-label](./includes/pre-release-label-v3.md)]

<span data-ttu-id="ad479-105">Una vez que haya creado su bot y lo haya comprobado localmente, puede insertarlo en Azure para que sea accesible desde cualquier lugar.</span><span class="sxs-lookup"><span data-stu-id="ad479-105">Once you have created your bot and verified it locally, you can push it to Azure to make it accessible from anywhere.</span></span> <span data-ttu-id="ad479-106">Para ello, primero implementará el bot en Azure en una instancia de App Service y luego configurará el bot con Azure Bot Service mediante el elemento Bot Channels Registration.</span><span class="sxs-lookup"><span data-stu-id="ad479-106">To do so, you will first deploy the bot to Azure in an App Service then you’ll configure your bot with the Azure Bot Service using the Bot Channels Registration item.</span></span>

## <a name="publish-from-visual-studio"></a><span data-ttu-id="ad479-107">Publicación desde Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ad479-107">Publish from Visual Studio</span></span>

<span data-ttu-id="ad479-108">Use Visual Studio para crear los recursos en Azure y publicar el código.</span><span class="sxs-lookup"><span data-stu-id="ad479-108">Use Visual Studio to create your resources in Azure and publish your code.</span></span>

<span data-ttu-id="ad479-109">En la ventana Explorador de soluciones, haga clic con el botón derecho en el nodo del proyecto y seleccione Publicar.</span><span class="sxs-lookup"><span data-stu-id="ad479-109">In the Solution Explorer window, right click on your project’s node and select Publish.</span></span>

![configuración de publicación](media/azure-bot-quickstarts/getting-started-publish-setting.png)

2. <span data-ttu-id="ad479-111">En el cuadro diálogo para la selección de un destino de publicación, asegúrese de que **App Service** esté seleccionado a la izquierda y que **Crear nuevo** esté seleccionado a la derecha.</span><span class="sxs-lookup"><span data-stu-id="ad479-111">In the Pick a publish target dialog, ensure **App Service** is selected on the left and **Create New** is selected on the right.</span></span>

3. <span data-ttu-id="ad479-112">Haga clic en el botón Publicar.</span><span class="sxs-lookup"><span data-stu-id="ad479-112">Click the Publish button.</span></span>

4. <span data-ttu-id="ad479-113">En la esquina superior derecha del cuadro de diálogo, asegúrese de que se muestra el identificador de usuario correcto para su suscripción de Azure.</span><span class="sxs-lookup"><span data-stu-id="ad479-113">In upper right of the dialog, ensure the dialog is showing the correct user ID for your Azure subscription.</span></span>

![publicación principal](media/azure-bot-quickstarts/getting-started-publish-main.png)

5. <span data-ttu-id="ad479-115">Escriba la información de nombre de la aplicación, suscripción, grupo de recursos y plan de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="ad479-115">Enter App Name, Subscription, Resource Group, and Hosting Plan information.</span></span>

6. <span data-ttu-id="ad479-116">Cuando esté listo, haga clic en Crear.</span><span class="sxs-lookup"><span data-stu-id="ad479-116">When ready, click Create.</span></span> <span data-ttu-id="ad479-117">Este proceso puede tardar unos minutos en completarse.</span><span class="sxs-lookup"><span data-stu-id="ad479-117">It can take a few minutes to complete the process.</span></span>

7. <span data-ttu-id="ad479-118">Una vez finalizado, se abrirá un explorador web que muestra la dirección URL pública del bot.</span><span class="sxs-lookup"><span data-stu-id="ad479-118">Once complete, a web browser will open showing your bot’s public URL.</span></span>

8. <span data-ttu-id="ad479-119">Realice una copia de esta dirección (será algo como https://<yourbotname>.azurewebsites.net/).</span><span class="sxs-lookup"><span data-stu-id="ad479-119">Make a copy of this URL (it will be something like https://<yourbotname>.azurewebsites.net/).</span></span>

> [!NOTE] 
> <span data-ttu-id="ad479-120">Deberá usar la versión HTTPS de la dirección URL al registrar el bot.</span><span class="sxs-lookup"><span data-stu-id="ad479-120">You’ll need to use the HTTPS version of the URL when registering your bot.</span></span> <span data-ttu-id="ad479-121">Azure proporciona compatibilidad con SSL en Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="ad479-121">Azure provides SSL support with Azure App Service.</span></span>

## <a name="create-your-bot-channels-registration"></a><span data-ttu-id="ad479-122">Creación del registro de canales de bot</span><span class="sxs-lookup"><span data-stu-id="ad479-122">Create your bot channels registration</span></span>
<span data-ttu-id="ad479-123">Con el bot implementado en Azure, deberá registrarlo en Azure Bot Service.</span><span class="sxs-lookup"><span data-stu-id="ad479-123">With your bot deployed in Azure you need to register it with the Azure Bot Service.</span></span>

1. <span data-ttu-id="ad479-124">Acceda a Azure Portal en https://portal.azure.com.</span><span class="sxs-lookup"><span data-stu-id="ad479-124">Access the Azure Portal at https://portal.azure.com.</span></span>

2. <span data-ttu-id="ad479-125">Inicie sesión con la misma identidad que usó anteriormente desde Visual Studio para publicar su bot.</span><span class="sxs-lookup"><span data-stu-id="ad479-125">Log in using the same identity you used earlier from Visual Studio to publish your bot.</span></span>

3. <span data-ttu-id="ad479-126">Haga clic en Crear un recurso.</span><span class="sxs-lookup"><span data-stu-id="ad479-126">Click Create a resource.</span></span>

4. <span data-ttu-id="ad479-127">En el campo Search the Marketplace (Buscar en Marketplace), escriba Bot Channels Registration y presione Entrar.</span><span class="sxs-lookup"><span data-stu-id="ad479-127">In the Search the Marketplace field type Bot Channels Registration and press Enter.</span></span>

5. <span data-ttu-id="ad479-128">En la lista devuelta, elija Bot Channels Registration:</span><span class="sxs-lookup"><span data-stu-id="ad479-128">In the returned list, choose Bot Channels Registration:</span></span>

![Publicar](media/azure-bot-quickstarts/getting-started-bot-registration.png)

6. <span data-ttu-id="ad479-130">Haga clic en Crear en la hoja que se abre.</span><span class="sxs-lookup"><span data-stu-id="ad479-130">Click create in the blade that opens.</span></span>

7. <span data-ttu-id="ad479-131">Proporcione un nombre para el bot.</span><span class="sxs-lookup"><span data-stu-id="ad479-131">Provide a Name for your bot.</span></span>

8. <span data-ttu-id="ad479-132">Elija la misma suscripción donde implementó el código del bot.</span><span class="sxs-lookup"><span data-stu-id="ad479-132">Choose the same Subscription where you deployed your bot’s code.</span></span>

9. <span data-ttu-id="ad479-133">Seleccione el grupo de recursos existente que establecerá la ubicación.</span><span class="sxs-lookup"><span data-stu-id="ad479-133">Pick your existing Resource group which will set the location.</span></span>

10. <span data-ttu-id="ad479-134">Puede elegir el plan de tarifa F0 para desarrollo y pruebas.</span><span class="sxs-lookup"><span data-stu-id="ad479-134">You can choose the F0 Pricing tier for development and testing.</span></span>

11. <span data-ttu-id="ad479-135">Escriba la dirección URL de su bot.</span><span class="sxs-lookup"><span data-stu-id="ad479-135">Enter your bot’s URL.</span></span> <span data-ttu-id="ad479-136">Asegúrese de que comienza por HTTPS y de que agrega /api/messages, por ejemplo https://yourbotname.azurewebsites.net/api/messages.</span><span class="sxs-lookup"><span data-stu-id="ad479-136">Make sure you start with HTTPS and that you add the /api/messages For example https://yourbotname.azurewebsites.net/api/messages</span></span>

12. <span data-ttu-id="ad479-137">Desactive por ahora Application Insights.</span><span class="sxs-lookup"><span data-stu-id="ad479-137">Turn off Application Insights for now.</span></span>

13. <span data-ttu-id="ad479-138">Haga clic en el identificador y la contraseña de aplicación de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="ad479-138">Click the Microsoft App ID and password</span></span>

14. <span data-ttu-id="ad479-139">En la hoja Nuevo, haga clic en Crear nuevo.</span><span class="sxs-lookup"><span data-stu-id="ad479-139">In the new blade click Create New.</span></span>

15. <span data-ttu-id="ad479-140">En la nueva hoja que aparece a la derecha, haga clic "Create App ID in the App Registration Portal" (Crear el identificador de aplicación en el portal de registro de aplicaciones), que se abre en una nueva pestaña del explorador.</span><span class="sxs-lookup"><span data-stu-id="ad479-140">In the new blade that opens to the right, click the "Create App ID in the App Registration Portal" which will open in a new browser tab.</span></span>

![msa de bot](media/azure-bot-quickstarts/getting-started-msa.png)

16. <span data-ttu-id="ad479-142">En la nueva pestaña, realice una copia del identificador de aplicación y guárdela en alguna parte.</span><span class="sxs-lookup"><span data-stu-id="ad479-142">In the new tab, make a copy of the App ID and save it somewhere.</span></span> 

17. <span data-ttu-id="ad479-143">Haga clic en el botón Generate an app password to continue (Generar una contraseña de aplicación para continuar).</span><span class="sxs-lookup"><span data-stu-id="ad479-143">Click the Generate an app password to continue button.</span></span>

18. <span data-ttu-id="ad479-144">Se abre un cuadro de diálogo de explorador que le proporciona la contraseña de la aplicación. Esta será la única vez que la reciba.</span><span class="sxs-lookup"><span data-stu-id="ad479-144">A browser dialog opens and provides you with your app’s password, which will be the only time you get it.</span></span> <span data-ttu-id="ad479-145">Copie y guarde esta contraseña en algún lugar donde pueda recuperarla más adelante.</span><span class="sxs-lookup"><span data-stu-id="ad479-145">Copy and save this password somewhere you can get to later.</span></span>

19. <span data-ttu-id="ad479-146">Cuando esté satisfecho con la contraseña guardada, haga clic en Aceptar.</span><span class="sxs-lookup"><span data-stu-id="ad479-146">Click OK once you’ve got the password saved.</span></span>

20. <span data-ttu-id="ad479-147">Simplemente cierre la pestaña del explorador y vuelva a la pestaña de Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="ad479-147">Just close the browser tab and return to the Azure Portal tab.</span></span>

21. <span data-ttu-id="ad479-148">Pegue el identificador y la contraseña de aplicación en los campos correctos y haga clic en Aceptar.</span><span class="sxs-lookup"><span data-stu-id="ad479-148">Paste in your App ID and Password in the correct fields and click OK.</span></span>

22. <span data-ttu-id="ad479-149">Ahora haga clic en Crear para configurar el registro de canales.</span><span class="sxs-lookup"><span data-stu-id="ad479-149">Now click Create to set up your channel registration.</span></span> <span data-ttu-id="ad479-150">Este proceso puede tardar algunos segundos o minutos en completarse.</span><span class="sxs-lookup"><span data-stu-id="ad479-150">This can take a few seconds to a few minutes.</span></span>

## <a name="update-your-bots-application-settings"></a><span data-ttu-id="ad479-151">Actualización de la configuración de la aplicación del bot</span><span class="sxs-lookup"><span data-stu-id="ad479-151">Update your bot’s Application Settings</span></span>
<span data-ttu-id="ad479-152">Para que el bot se autentique en Azure Bot Service, debe agregar dos valores de configuración a la configuración de la aplicación del bot en Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="ad479-152">In order for your bot to authenticate with the Azure Bot Service, you need to add two settings to your Bot’s Application Settings in Azure App Service.</span></span> 

1. <span data-ttu-id="ad479-153">Haga clic en App Services.</span><span class="sxs-lookup"><span data-stu-id="ad479-153">Click App Services.</span></span> <span data-ttu-id="ad479-154">Escriba el nombre del bot en el cuadro de texto de suscripciones.</span><span class="sxs-lookup"><span data-stu-id="ad479-154">Type your bot’s name in the Subscriptions text box.</span></span> <span data-ttu-id="ad479-155">A continuación, haga clic en el nombre del bot en la lista.</span><span class="sxs-lookup"><span data-stu-id="ad479-155">Then click on your bot's name in the list.</span></span>

![app service](media/azure-bot-quickstarts/getting-started-app-service.png)

2. <span data-ttu-id="ad479-157">En la lista de opciones a la izquierda de las opciones del bot, busque Application Settings (Configuración de la aplicación) en la sección Settings (Configuración) y haga clic en ella.</span><span class="sxs-lookup"><span data-stu-id="ad479-157">In the list of options on the left within your bot's options, locate Application Settings in the Settings section and click it.</span></span>

![identificador del bot](media/azure-bot-quickstarts/getting-started-app-settings-1.png)

3. <span data-ttu-id="ad479-159">Desplácese hasta que encuentre la sección Application settings (Configuración de la aplicación).</span><span class="sxs-lookup"><span data-stu-id="ad479-159">Scroll until you find the Application settings section.</span></span>

![msa de bot](media/azure-bot-quickstarts/getting-started-app-settings-2.png)

4. <span data-ttu-id="ad479-161">Haga clic en Add new setting (Agregar nueva configuración).</span><span class="sxs-lookup"><span data-stu-id="ad479-161">Click Add new setting.</span></span>

5. <span data-ttu-id="ad479-162">Escriba **MicrosoftAppId** como nombre y el identificador de la aplicación como valor.</span><span class="sxs-lookup"><span data-stu-id="ad479-162">Type **MicrosoftAppId** for the name and your App ID for the value.</span></span>

6. <span data-ttu-id="ad479-163">Haga clic en Add new setting (Agregar nueva configuración).</span><span class="sxs-lookup"><span data-stu-id="ad479-163">Click Add new setting</span></span>

7. <span data-ttu-id="ad479-164">Escriba **MicrosoftAppPassword** como nombre y su contraseña como valor.</span><span class="sxs-lookup"><span data-stu-id="ad479-164">Type **MicrosoftAppPassword** for the name and your password for the value.</span></span>

8. <span data-ttu-id="ad479-165">Haga clic en el botón Save (Guardar) de la parte superior.</span><span class="sxs-lookup"><span data-stu-id="ad479-165">Click the Save button up top.</span></span>

## <a name="test-your-bot-in-production"></a><span data-ttu-id="ad479-166">Prueba del bot en producción</span><span class="sxs-lookup"><span data-stu-id="ad479-166">Test Your Bot in Production</span></span>
<span data-ttu-id="ad479-167">En este momento, puede probar el bot desde Azure mediante el cliente de Chat en web integrado.</span><span class="sxs-lookup"><span data-stu-id="ad479-167">At this point, you can test your bot from Azure using the built-in Web Chat client.</span></span>

1. <span data-ttu-id="ad479-168">Vuelva al grupo de recursos en el portal.</span><span class="sxs-lookup"><span data-stu-id="ad479-168">Go back to your Resource group in the portal</span></span>

2. <span data-ttu-id="ad479-169">Abra el registro del bot.</span><span class="sxs-lookup"><span data-stu-id="ad479-169">Open your bot registration.</span></span>

3. <span data-ttu-id="ad479-170">En Bot Management (Administración de bots), seleccione Test in Web Chat (Probar en Chat en web).</span><span class="sxs-lookup"><span data-stu-id="ad479-170">Under Bot management, select Test in Web Chat.</span></span>

![probar en Chat en web](media/azure-bot-quickstarts/getting-started-test-webchat.png)

4. <span data-ttu-id="ad479-172">Escriba un mensaje como `Hi` y presione ENTRAR.</span><span class="sxs-lookup"><span data-stu-id="ad479-172">Type a message like `Hi` and press Enter.</span></span> <span data-ttu-id="ad479-173">El bot devolverá `Turn 1: You sent Hi`.</span><span class="sxs-lookup"><span data-stu-id="ad479-173">The bot will echo back `Turn 1: You sent Hi`.</span></span>

