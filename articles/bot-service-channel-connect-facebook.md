---
title: Conexión de un bot a Facebook Messenger | Microsoft Docs
description: Obtenga información sobre cómo configurar la conexión de un bot a Facebook Messenger.
keywords: Facebook Messenger, canal de bot, aplicación de Facebook, id. de aplicación, secreto de aplicación, bot de Facebook, credenciales
author: RobStand
ms.author: RobStand
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 09/16/2018
ms.openlocfilehash: 60e39bb652ab5b7ffeeb5ba53bdf4c82f936553e
ms.sourcegitcommit: 3bf3dbb1a440b3d83e58499c6a2ac116fe04b2f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/23/2018
ms.locfileid: "46707871"
---
# <a name="connect-a-bot-to-facebook-messenger"></a><span data-ttu-id="02a91-104">Conexión de un bot a Facebook Messenger</span><span class="sxs-lookup"><span data-stu-id="02a91-104">Connect a bot to Facebook Messenger</span></span>

<span data-ttu-id="02a91-105">Para obtener más información acerca del desarrollo de Facebook Messenger, consulte la [documentación de la plataforma de Messenger](https://developers.facebook.com/docs/messenger-platform).</span><span class="sxs-lookup"><span data-stu-id="02a91-105">To learn more about developing for Facebook Messenger, see the [Messenger platform documentation](https://developers.facebook.com/docs/messenger-platform).</span></span> <span data-ttu-id="02a91-106">Puede que quiera revisar la [lista de verificación previa al lanzamiento](https://developers.facebook.com/docs/messenger-platform/product-overview/launch#app_public), el [tutorial de inicio rápido](https://developers.facebook.com/docs/messenger-platform/guides/quick-start) y la [guía de configuración](https://developers.facebook.com/docs/messenger-platform/guides/setup) de Facebook.</span><span class="sxs-lookup"><span data-stu-id="02a91-106">You may wish to review Facebook's [pre-launch guidelines](https://developers.facebook.com/docs/messenger-platform/product-overview/launch#app_public), [quick start](https://developers.facebook.com/docs/messenger-platform/guides/quick-start), and [setup guide](https://developers.facebook.com/docs/messenger-platform/guides/setup).</span></span>

<span data-ttu-id="02a91-107">Para configurar un bot para que se comunique con Facebook Messenger, habilite Facebook Messenger en una página de Facebook y, a continuación, conecte el bot a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="02a91-107">To configure a bot to communicate using Facebook Messenger, enable Facebook Messenger on a Facebook page and then connect the bot to the app.</span></span>

> [!NOTE]
> <span data-ttu-id="02a91-108">La UI de Facebook puede aparecer de forma ligeramente diferente dependiendo de la versión que use.</span><span class="sxs-lookup"><span data-stu-id="02a91-108">The Facebook UI may appear slightly different depending on which version you are using.</span></span>

## <a name="copy-the-page-id"></a><span data-ttu-id="02a91-109">Copia del id. de página</span><span class="sxs-lookup"><span data-stu-id="02a91-109">Copy the Page ID</span></span>

<span data-ttu-id="02a91-110">Se accede al bot a través de una página de Facebook.</span><span class="sxs-lookup"><span data-stu-id="02a91-110">The bot is accessed through a Facebook Page.</span></span> <span data-ttu-id="02a91-111">[Cree una nueva página de Facebook](https://www.facebook.com/bookmarks/pages) o vaya a una página existente.</span><span class="sxs-lookup"><span data-stu-id="02a91-111">[Create a new Facebook Page](https://www.facebook.com/bookmarks/pages) or go to an existing Page.</span></span>

* <span data-ttu-id="02a91-112">Abra la página **Sobre** de la página de Facebook y, a continuación, copie y guarde el **id. de página**.</span><span class="sxs-lookup"><span data-stu-id="02a91-112">Open the Facebook Page's **About** page and then copy and save the **Page ID**.</span></span>

## <a name="create-a-facebook-app"></a><span data-ttu-id="02a91-113">Creación de una aplicación de Facebook</span><span class="sxs-lookup"><span data-stu-id="02a91-113">Create a Facebook app</span></span>

<span data-ttu-id="02a91-114">[Cree una nueva aplicación de Facebook](https://developers.facebook.com/quickstarts/?platform=web) en la página y genere un id. y un secreto de aplicación.</span><span class="sxs-lookup"><span data-stu-id="02a91-114">[Create a new Facebook App](https://developers.facebook.com/quickstarts/?platform=web) on the Page and generate an App ID and App Secret for it.</span></span>

![Creación de un id. de aplicación](~/media/channels/FB-CreateAppId.png)

* <span data-ttu-id="02a91-116">Copie y guarde el **id. de aplicación** y el **secreto de aplicación**.</span><span class="sxs-lookup"><span data-stu-id="02a91-116">Copy and save the **App ID** and the **App Secret**.</span></span>

![Guardar el id. y el secreto de aplicación](~/media/channels/FB-get-appid.png)

<span data-ttu-id="02a91-118">Establezca el control deslizante "Allow API Access to App Settings" (Permitir el acceso de la API a la configuración de la aplicación) en "Sí".</span><span class="sxs-lookup"><span data-stu-id="02a91-118">Set the "Allow API Access to App Settings" slider to "Yes".</span></span>

![Configuración de la aplicación](~/media/bot-service-channel-connect-facebook/api_settings.png)

## <a name="enable-messenger"></a><span data-ttu-id="02a91-120">Habilitación de Messenger</span><span class="sxs-lookup"><span data-stu-id="02a91-120">Enable messenger</span></span>


<span data-ttu-id="02a91-121">Habilite Facebook Messenger en la nueva aplicación de Facebook.</span><span class="sxs-lookup"><span data-stu-id="02a91-121">Enable Facebook Messenger in the new Facebook App.</span></span>

* <span data-ttu-id="02a91-122">En la página **Configuración del producto** de la aplicación, haga clic en **Comenzar** y, a continuación, vuelva a hacer clic en **Comenzar**.</span><span class="sxs-lookup"><span data-stu-id="02a91-122">On the **Product Setup** page of the app, click **Get Started** and then click **Get Started** again.</span></span>


![Habilitación de Messenger](~/media/channels/FB-AddMessaging1.png)

## <a name="generate-a-page-access-token"></a><span data-ttu-id="02a91-124">Generación de un token de acceso a la página</span><span class="sxs-lookup"><span data-stu-id="02a91-124">Generate a Page Access Token</span></span>

<span data-ttu-id="02a91-125">En el panel **Generación de tokens** de la sección de Messenger, seleccione la página de destino.</span><span class="sxs-lookup"><span data-stu-id="02a91-125">In the **Token Generation** panel of the Messenger section, select the target Page.</span></span> <span data-ttu-id="02a91-126">Se generará un token de acceso a la página.</span><span class="sxs-lookup"><span data-stu-id="02a91-126">A Page Access Token will be generated.</span></span>

* <span data-ttu-id="02a91-127">Copie y guarde el **token de acceso a la página**.</span><span class="sxs-lookup"><span data-stu-id="02a91-127">Copy and save the **Page Access Token**.</span></span>

![Generación del token](~/media/channels/FB-generateToken.png)

## <a name="enable-webhooks"></a><span data-ttu-id="02a91-129">Habilitación de webhooks</span><span class="sxs-lookup"><span data-stu-id="02a91-129">Enable webhooks</span></span>

<span data-ttu-id="02a91-130">Haga clic en **Configurar webhooks** para reenviar eventos de mensajería de Facebook Messenger al bot.</span><span class="sxs-lookup"><span data-stu-id="02a91-130">Click **Set up Webhooks** to forward messaging events from Facebook Messenger to the bot.</span></span>

![Habilitación del webhook](~/media/channels/FB-webhook.png)

## <a name="provide-webhook-callback-url-and-verify-token"></a><span data-ttu-id="02a91-132">Aprovisionamiento de la dirección URL de devolución de llamada de webhook y comprobación del token</span><span class="sxs-lookup"><span data-stu-id="02a91-132">Provide webhook callback URL and verify token</span></span>

<span data-ttu-id="02a91-133">En [Azure Portal](https://portal.azure.com/), abra el bot, haga clic en la pestaña **Canales** y, a continuación, haga clic en **Facebook Messenger**.</span><span class="sxs-lookup"><span data-stu-id="02a91-133">In the [Azure portal](https://portal.azure.com/), open the bot, click the **Channels** tab, and then click **Facebook Messenger**.</span></span>

* <span data-ttu-id="02a91-134">Copie los valores **Dirección URL de devolución de llamada** y **Comprobar token** desde el portal.</span><span class="sxs-lookup"><span data-stu-id="02a91-134">Copy the **Callback URL** and **Verify Token** values from the portal.</span></span>

![Copia de valores](~/media/channels/fb-callbackVerify.png)

1. <span data-ttu-id="02a91-136">Vuelva a Facebook Messenger y pegue los valores **Dirección URL de devolución de llamada** y **Comprobar token**.</span><span class="sxs-lookup"><span data-stu-id="02a91-136">Return to Facebook Messenger and paste the **Callback URL** and **Verify Token** values.</span></span>

2. <span data-ttu-id="02a91-137">En **Campos de suscripción**, seleccione *message\_deliveries*, *messages*, *messaging\_options* y *messaging\_postbacks*.</span><span class="sxs-lookup"><span data-stu-id="02a91-137">Under **Subscription Fields**, select *message\_deliveries*, *messages*, *messaging\_optins*, and *messaging\_postbacks*.</span></span>

3. <span data-ttu-id="02a91-138">Haga clic en **Comprobar y guardar**.</span><span class="sxs-lookup"><span data-stu-id="02a91-138">Click **Verify and Save**.</span></span>

![Configuración del webhook](~/media/channels/FB-webhookConfig.png)

4. <span data-ttu-id="02a91-140">Subscriba el webhook a la página de Facebook.</span><span class="sxs-lookup"><span data-stu-id="02a91-140">Subscribe the webhook to the Facebook page.</span></span>

![Suscripción del webhook](~/media/bot-service-channel-connect-facebook/subscribe-webhook.png)


## <a name="provide-facebook-credentials"></a><span data-ttu-id="02a91-142">Aprovisionamiento de credenciales de Facebook</span><span class="sxs-lookup"><span data-stu-id="02a91-142">Provide Facebook credentials</span></span>

<span data-ttu-id="02a91-143">En Azure Portal, pegue los valores de **Identificador de aplicación de Facebook**, **Secreto de aplicación de Facebook**, **Identificador de página** y **Token de acceso a la página** copiados de Facebook Messenger anteriormente.</span><span class="sxs-lookup"><span data-stu-id="02a91-143">In Azure portal, paste the **Facebook App ID**, **Facebook App Secret**, **Page ID**,and **Page Access Token** values copied from Facebook Messenger previously.</span></span> <span data-ttu-id="02a91-144">Puede usar el mismo bot en varias páginas de Facebook mediante la adición de identificadores de página y tokens de acceso adicionales.</span><span class="sxs-lookup"><span data-stu-id="02a91-144">You can use the same bot on multiple facebook pages by adding additional page ids and access tokens.</span></span>

![Escribir credenciales](~/media/channels/fb-credentials2.png)

## <a name="submit-for-review"></a><span data-ttu-id="02a91-146">Envío para revisión</span><span class="sxs-lookup"><span data-stu-id="02a91-146">Submit for review</span></span>

<span data-ttu-id="02a91-147">Facebook requiere una dirección URL de la directiva de privacidad y una dirección URL de las Condiciones del servicio en su página de configuración de aplicación básica.</span><span class="sxs-lookup"><span data-stu-id="02a91-147">Facebook requires a Privacy Policy URL and Terms of Service URL on its basic app settings page.</span></span> <span data-ttu-id="02a91-148">La página [Código de conducta](https://aka.ms/bf-conduct) contiene vínculos a recursos de terceros para ayudar a crear una directiva de privacidad.</span><span class="sxs-lookup"><span data-stu-id="02a91-148">The [Code of Conduct](https://aka.ms/bf-conduct) page contains third party resource links to help create a privacy policy.</span></span> <span data-ttu-id="02a91-149">La página [Condiciones de uso](https://aka.ms/bf-terms) contiene los términos de ejemplo que le ayudarán a crear un documento de condiciones del servicio adecuado.</span><span class="sxs-lookup"><span data-stu-id="02a91-149">The [Terms of Use](https://aka.ms/bf-terms) page contains sample terms to help create an appropriate Terms of Service document.</span></span>

<span data-ttu-id="02a91-150">Al finalizar el bot, Facebook tiene su propio [proceso de revisión](https://developers.facebook.com/docs/messenger-platform/app-review) para las aplicaciones que se publican en Messenger.</span><span class="sxs-lookup"><span data-stu-id="02a91-150">After the bot is finished, Facebook has its own [review process](https://developers.facebook.com/docs/messenger-platform/app-review) for apps that are published to Messenger.</span></span> <span data-ttu-id="02a91-151">El bot se probará para asegurarse de que es compatible con las [políticas de la plataforma](https://developers.facebook.com/docs/messenger-platform/policy-overview) de Facebook.</span><span class="sxs-lookup"><span data-stu-id="02a91-151">The bot will be tested to ensure it is compliant with Facebook's [Platform Policies](https://developers.facebook.com/docs/messenger-platform/policy-overview).</span></span>

## <a name="make-the-app-public-and-publish-the-page"></a><span data-ttu-id="02a91-152">Publicación de la aplicación y la página</span><span class="sxs-lookup"><span data-stu-id="02a91-152">Make the App public and publish the Page</span></span>

> [!NOTE]
> <span data-ttu-id="02a91-153">Una aplicación se encuentra en [modo de desarrollo](https://developers.facebook.com/docs/apps/managing-development-cycle) hasta que no se publica.</span><span class="sxs-lookup"><span data-stu-id="02a91-153">Until an app is published, it is in [Development Mode](https://developers.facebook.com/docs/apps/managing-development-cycle).</span></span> <span data-ttu-id="02a91-154">Los complementos y las funcionalidades de API solo funcionarán para los administradores, los desarrolladores y los evaluadores.</span><span class="sxs-lookup"><span data-stu-id="02a91-154">Plugin and API functionality will only work for admins, developers, and testers.</span></span>

<span data-ttu-id="02a91-155">Una vez que la revisión se considere correcta, en el panel de aplicaciones en Revisión de la aplicación, establezca la aplicación en Pública.</span><span class="sxs-lookup"><span data-stu-id="02a91-155">After the review is successful, in the App Dashboard under App Review, set the app to Public.</span></span>
<span data-ttu-id="02a91-156">Asegúrese de que la página de Facebook asociada a este bot esté publicada.</span><span class="sxs-lookup"><span data-stu-id="02a91-156">Ensure that the Facebook Page associated with this bot is published.</span></span> <span data-ttu-id="02a91-157">El estado aparece en la configuración de las páginas.</span><span class="sxs-lookup"><span data-stu-id="02a91-157">Status appears in Pages settings.</span></span>

> [!NOTE]
> <span data-ttu-id="02a91-158">También puede usar Facebook Workplace.</span><span class="sxs-lookup"><span data-stu-id="02a91-158">You can also use Facebook Workplace.</span></span> <span data-ttu-id="02a91-159">Para habilitarlo, cree una [integración personalizada](https://developers.facebook.com/docs/workplace/custom-integrations-new) para el área de trabajo y use su identificador de aplicación, secreto de aplicación y token de acceso.</span><span class="sxs-lookup"><span data-stu-id="02a91-159">To enable it, create a [custom integration](https://developers.facebook.com/docs/workplace/custom-integrations-new) for your Workplace and use its App ID, App Secret, and Access Token.</span></span> <span data-ttu-id="02a91-160">En lugar de un valor de pageID tradicional, use los números que siguen al nombre de la integración en la página Acerca de.</span><span class="sxs-lookup"><span data-stu-id="02a91-160">Instead of a traditional pageID, use the numbers following the integrations name on its About page.</span></span> <span data-ttu-id="02a91-161">Los webhooks se pueden conectar con las credenciales que se muestran en Azure.</span><span class="sxs-lookup"><span data-stu-id="02a91-161">The webhooks can be connected with the credentials shown in Azure.</span></span>
