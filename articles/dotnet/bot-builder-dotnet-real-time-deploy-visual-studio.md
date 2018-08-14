---
title: Implementación de un bot multimedia en tiempo real de Skype en Azure | Microsoft Docs
description: Obtenga información sobre cómo implementar un bot de audio y vídeo en tiempo real de Skype en Azure mediante la característica de publicación integrada de Visual Studio.
author: MalarGit
ms.author: malarch
manager: ssulzer
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/17
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: 4ef96c82229d44a88e6063c64cd435cf7127a4b3
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305077"
---
# <a name="deploy-a-real-time-media-bot-from-visual-studio-to-azure"></a><span data-ttu-id="9d7ab-103">Implementación de un bot multimedia en tiempo real de Visual Studio en Azure</span><span class="sxs-lookup"><span data-stu-id="9d7ab-103">Deploy a real-time media bot from Visual Studio to Azure</span></span>
<span data-ttu-id="9d7ab-104">Los bots multimedia en tiempo real pueden hospedarse en una instancia de Azure Virtual Machine "IaaS" o en un servicio de nube de Azure "clásico".</span><span class="sxs-lookup"><span data-stu-id="9d7ab-104">Real-time media bots can be hosted in either an "IaaS" Azure Virtual Machine or a "classic" Azure Cloud Service.</span></span> <span data-ttu-id="9d7ab-105">En este artículo se muestra cómo implementar un bot, hospedado en un rol de trabajo del servicio de nube de Azure, desde Visual Studio con la funcionalidad de publicación integrada.</span><span class="sxs-lookup"><span data-stu-id="9d7ab-105">This article shows how to deploy a bot, hosted in an Azure Cloud Service Worker Role, from Visual Studio using its built-in publish capability.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9d7ab-106">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="9d7ab-106">Prerequisites</span></span>

<span data-ttu-id="9d7ab-107">Debe tener una suscripción a Microsoft Azure antes de poder implementar un bot en Azure.</span><span class="sxs-lookup"><span data-stu-id="9d7ab-107">You must have a Microsoft Azure subscription before you can deploy a bot to Azure.</span></span> <span data-ttu-id="9d7ab-108">Si aún no tiene una suscripción, puede registrarse para obtener una <a href="https://azure.microsoft.com/en-us/free/" target="_blank">cuenta gratuita</a>.</span><span class="sxs-lookup"><span data-stu-id="9d7ab-108">If you do not already have a subscription, you can register for a <a href="https://azure.microsoft.com/en-us/free/" target="_blank">free account</a>.</span></span> <span data-ttu-id="9d7ab-109">Además, el proceso descrito en este artículo requiere Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9d7ab-109">Additionally, the process described by this article requires Visual Studio.</span></span> <span data-ttu-id="9d7ab-110">Si aún no tiene Visual Studio, puede descargar gratis <a href="https://www.visualstudio.com/downloads/" target="_blank">Visual Studio 2017 Community</a>.</span><span class="sxs-lookup"><span data-stu-id="9d7ab-110">If you do not already have Visual Studio, you can download <a href="https://www.visualstudio.com/downloads/" target="_blank">Visual Studio 2017 Community</a> for free.</span></span>

### <a name="certificate-from-a-valid-certificate-authority"></a><span data-ttu-id="9d7ab-111">Certificado de una entidad de certificación válida</span><span class="sxs-lookup"><span data-stu-id="9d7ab-111">Certificate from a valid certificate authority</span></span>
<span data-ttu-id="9d7ab-112">El bot debe configurarse con un certificado válido de una autoridad de certificado de confianza (CA).</span><span class="sxs-lookup"><span data-stu-id="9d7ab-112">The bot needs to be configured with a valid certificate from a trusted Certificate Authority (CA).</span></span> <span data-ttu-id="9d7ab-113">El nombre del firmante (SN) o la última entrada del nombre alternativo del firmante (SAN) del certificado debe ser el nombre del servicio de nube.</span><span class="sxs-lookup"><span data-stu-id="9d7ab-113">The Subject Name (SN) or the last entry of the Subject Alternative Name (SAN) of the certificate should be the name of the cloud service.</span></span> <span data-ttu-id="9d7ab-114">Actualmente no se admiten certificados comodín.</span><span class="sxs-lookup"><span data-stu-id="9d7ab-114">Wild-card certificates are currently not supported.</span></span> <span data-ttu-id="9d7ab-115">Si se usa un registro CNAME para apuntar al servicio en la nube, el CNAME SN debe ser el SN o la última entrada del SAN del certificado.</span><span class="sxs-lookup"><span data-stu-id="9d7ab-115">If a CNAME is used to point to the cloud service, the CNAME should be the SN or the last SAN entry of the certificate.</span></span>

## <a name="configure-application-settings"></a><span data-ttu-id="9d7ab-116">Configuración de la aplicación</span><span class="sxs-lookup"><span data-stu-id="9d7ab-116">Configure application settings</span></span>
<span data-ttu-id="9d7ab-117">Para que el bot funcione correctamente en la nube, debe asegurarse de que la configuración de la aplicación sea correcta.</span><span class="sxs-lookup"><span data-stu-id="9d7ab-117">For your bot to function properly in the cloud, you must ensure that its application settings are correct.</span></span> <span data-ttu-id="9d7ab-118">Más concretamente, establezca los siguientes valores de clave en el archivo app.config de su rol de trabajo:</span><span class="sxs-lookup"><span data-stu-id="9d7ab-118">More specifically, set the following key values in the app.config file of your worker role:</span></span>
> <ul><li><span data-ttu-id="9d7ab-119">MicrosoftAppId</span><span class="sxs-lookup"><span data-stu-id="9d7ab-119">MicrosoftAppId</span></span></li><li><span data-ttu-id="9d7ab-120">MicrosoftAppPassword</span><span class="sxs-lookup"><span data-stu-id="9d7ab-120">MicrosoftAppPassword</span></span></li></ul>

> [!NOTE]
> <span data-ttu-id="9d7ab-121">Para buscar **AppID** y **AppPassword** del bot, consulte [MicrosoftAppID y MicrosoftAppPassword](~/bot-service-manage-overview.md#microsoftappid-and-microsoftapppassword).</span><span class="sxs-lookup"><span data-stu-id="9d7ab-121">To find your bot's **AppID** and **AppPassword**, see [MicrosoftAppID and MicrosoftAppPassword](~/bot-service-manage-overview.md#microsoftappid-and-microsoftapppassword).</span></span>

## <a name="create-worker-role-in-the-azure-portal"></a><span data-ttu-id="9d7ab-122">Creación del rol de trabajo en Azure Portal</span><span class="sxs-lookup"><span data-stu-id="9d7ab-122">Create worker role in the Azure Portal</span></span>
### <a name="step-1-create-cloud-serviceclassic"></a><span data-ttu-id="9d7ab-123">Paso 1: Crear el servicio en la nube (clásico)</span><span class="sxs-lookup"><span data-stu-id="9d7ab-123">Step 1: Create Cloud Service(classic)</span></span>
<span data-ttu-id="9d7ab-124">Inicie sesión en <a href="https://portal.azure.com">Azure Portal</a>.</span><span class="sxs-lookup"><span data-stu-id="9d7ab-124">Log on to <a href="https://portal.azure.com">Azure Portal</a>.</span></span> <span data-ttu-id="9d7ab-125">Seleccione **+** en el lado izquierdo de la pantalla y elija **Servicio en la nube (clásico)**.</span><span class="sxs-lookup"><span data-stu-id="9d7ab-125">Click **+** on the left side of the screen and choose **Cloud services (classic)**.</span></span> <span data-ttu-id="9d7ab-126">Proporcione la información necesaria en el formulario y haga clic en **Crear**.</span><span class="sxs-lookup"><span data-stu-id="9d7ab-126">Provide the required information in the form and click **Create**.</span></span>

![Creación del servicio en la nube](../media/real-time-media-bot-portal-service-creation.png)

> [!NOTE]
> <span data-ttu-id="9d7ab-128">Debe proporcionarse el nombre DNS del bot en la dirección URL para el registro del bot.</span><span class="sxs-lookup"><span data-stu-id="9d7ab-128">The DNS name of the bot should be supplied in the url for bot registration.</span></span>

### <a name="step-2-upload-the-certificate-for-the-bot"></a><span data-ttu-id="9d7ab-129">Paso 2: Cargar el certificado para el bot</span><span class="sxs-lookup"><span data-stu-id="9d7ab-129">Step 2: Upload the certificate for the bot</span></span>
<span data-ttu-id="9d7ab-130">Una vez creado el bot, cargue el certificado para el bot.</span><span class="sxs-lookup"><span data-stu-id="9d7ab-130">Once the bot is created, upload the certificate for the bot.</span></span>

![Carga del certificado](../media/real-time-media-bot-portal-certificates.png)

## <a name="modify-service-configuration-with-worker-role-details"></a><span data-ttu-id="9d7ab-132">Modificación de la configuración del servicio con los detalles del rol de trabajo</span><span class="sxs-lookup"><span data-stu-id="9d7ab-132">Modify service configuration with worker role details</span></span>
<span data-ttu-id="9d7ab-133">El nombre de dominio completo (FQDN) del bot no está disponible a través de las API de Azure RoleEnvironment.</span><span class="sxs-lookup"><span data-stu-id="9d7ab-133">The Fully-Qualified Domain Name (FQDN) of the bot is not available through the Azure RoleEnvironment APIs.</span></span> <span data-ttu-id="9d7ab-134">Por lo tanto, se debe proporcionar el FQDN al bot.</span><span class="sxs-lookup"><span data-stu-id="9d7ab-134">Therefore, the bot must be provided with its FQDN.</span></span> <span data-ttu-id="9d7ab-135">También es necesario que sepa sobre el certificado para HTTPS.</span><span class="sxs-lookup"><span data-stu-id="9d7ab-135">It also needs to know about the certificate for HTTPS.</span></span> <span data-ttu-id="9d7ab-136">Esto se puede configurar en el archivo de configuración del servicio (.cscfg) del rol de trabajo.</span><span class="sxs-lookup"><span data-stu-id="9d7ab-136">These can be configured in the service configuration (.cscfg) file of the worker role.</span></span>

> [!TIP]
> <span data-ttu-id="9d7ab-137">Si va a implementar un ejemplo del repositorio de Git BotBuilder RealTimeMediaCalling:</span><span class="sxs-lookup"><span data-stu-id="9d7ab-137">If you are deploying a sample from BotBuilder-RealTimeMediaCalling git repository,</span></span>
> - <span data-ttu-id="9d7ab-138">Sustituya el $DnsName$ por el nombre del servicio en la nube o el registro CNAME si se usa uno en la configuración del servicio.</span><span class="sxs-lookup"><span data-stu-id="9d7ab-138">Substitute the $DnsName$ with either the cloud service name or the CNAME if one is used in the service configuration.</span></span>
>   ```xml
>      <Setting name="ServiceDnsName" value="$DnsName$" />
>   ```
> 
> - <span data-ttu-id="9d7ab-139">Sustituya $CertThumbprint$ por la huella digital del certificado cargado en el bot en las siguientes líneas de la configuración.</span><span class="sxs-lookup"><span data-stu-id="9d7ab-139">Substitute $CertThumbprint$ with the thumbprint of the certificate uploaded to the bot in the following lines from the configuration.</span></span>
>   ```xml
>      <Setting name="DefaultCertificate" value="$CertThumbprint$" />
>      <Certificate name="Default" thumbprint="$CertThumbprint$" thumbprintAlgorithm="sha1" />
>   ```

## <a name="publish-the-bot-from-visual-studio"></a><span data-ttu-id="9d7ab-140">Publicación del bot desde Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9d7ab-140">Publish the bot from Visual Studio</span></span>
### <a name="step-1-launch-the-microsoft-azure-publishing-wizard-in-visual-studio"></a><span data-ttu-id="9d7ab-141">Paso 1: Iniciar el Asistente para la publicación de Microsoft Azure en Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9d7ab-141">Step 1: Launch the Microsoft Azure Publishing Wizard in Visual Studio</span></span>

<span data-ttu-id="9d7ab-142">Abra el proyecto en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9d7ab-142">Open your project in Visual Studio.</span></span> <span data-ttu-id="9d7ab-143">En el Explorador de soluciones, haga clic con el botón derecho en el proyecto del servicio en la nube y seleccione **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="9d7ab-143">In Solution Explorer, right-click the cloud service project and select **Publish**.</span></span> <span data-ttu-id="9d7ab-144">Esto inicia el Asistente para la publicación de Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="9d7ab-144">This starts the Microsoft Azure publishing wizard.</span></span> <span data-ttu-id="9d7ab-145">Use sus credenciales para iniciar sesión en la suscripción adecuada.</span><span class="sxs-lookup"><span data-stu-id="9d7ab-145">Use your credentials to sign in to the appropriate subscription.</span></span>

![Haga clic con el botón derecho en el proyecto y elija Publicar para iniciar al Asistente para la publicación de Microsoft Azure.](../media/real-time-media-bot-publish-signin.png)

### <a name="step-2-publish-the-bot"></a><span data-ttu-id="9d7ab-147">Paso 2: Publicar el bot</span><span class="sxs-lookup"><span data-stu-id="9d7ab-147">Step 2: Publish the bot</span></span>

<span data-ttu-id="9d7ab-148">Haga clic en **Next**.</span><span class="sxs-lookup"><span data-stu-id="9d7ab-148">Click **Next**.</span></span> <span data-ttu-id="9d7ab-149">Se abrirá la pestaña **Configuración**. Especifique Servicio en la nube, Entorno, Configuración de compilación y Configuración de servicio para implementar el bot.</span><span class="sxs-lookup"><span data-stu-id="9d7ab-149">This will open the **Settings** tab. Specify the Cloud Service, Environment, Build Configuration and the Service Configuration for deploying the bot.</span></span>

![Haga clic en Siguiente y vaya a la pestaña Configuración](../media/real-time-media-bot-publish-settings.png)

<span data-ttu-id="9d7ab-151">Como alternativa, puede elegir **Configuración avanzada** y especificar la cuenta de almacenamiento para los registros de implementación (que puede usar para depurar problemas).</span><span class="sxs-lookup"><span data-stu-id="9d7ab-151">You can optionally choose **Advanced Settings** and specify the Storage account for the deployment logs (which you can use to debug issues).</span></span>

![Haga clic en la pestaña Configuración avanzada](../media/real-time-media-bot-publish-advanced-settings.png)

<span data-ttu-id="9d7ab-153">Compruebe la configuración en la pestaña **Resumen** y haga clic en **Publicar** para implementar el bot en Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="9d7ab-153">Verify the configuration in the **Summary** tab and click **Publish** to deploy the bot to Microsoft Azure.</span></span>
