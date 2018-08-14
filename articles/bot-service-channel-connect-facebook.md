---
title: Conexión de un bot a Facebook Messenger | Microsoft Docs
description: Obtenga información sobre cómo configurar la conexión de un bot a Facebook Messenger.
keywords: Facebook Messenger, canal de bot, aplicación de Facebook, id. de aplicación, secreto de aplicación, bot de Facebook, credenciales
author: RobStand
ms.author: RobStand
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ms.openlocfilehash: 0a9ad7d51234b417d5d0f27dbcffe4ce839ba94a
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39304858"
---
# <a name="connect-a-bot-to-facebook-messenger"></a><span data-ttu-id="05b89-104">Conexión de un bot a Facebook Messenger</span><span class="sxs-lookup"><span data-stu-id="05b89-104">Connect a bot to Facebook Messenger</span></span>

<span data-ttu-id="05b89-105">Para obtener más información acerca del desarrollo de Facebook Messenger, consulte la [documentación de la plataforma de Messenger](https://developers.facebook.com/docs/messenger-platform).</span><span class="sxs-lookup"><span data-stu-id="05b89-105">To learn more about developing for Facebook Messenger, see the [Messenger platform documentation](https://developers.facebook.com/docs/messenger-platform).</span></span> <span data-ttu-id="05b89-106">Puede que quiera revisar la [lista de verificación previa al lanzamiento](https://developers.facebook.com/docs/messenger-platform/product-overview/launch#app_public), el [tutorial de inicio rápido](https://developers.facebook.com/docs/messenger-platform/guides/quick-start) y la [guía de configuración](https://developers.facebook.com/docs/messenger-platform/guides/setup) de Facebook.</span><span class="sxs-lookup"><span data-stu-id="05b89-106">You may wish to review Facebook's [pre-launch guidelines](https://developers.facebook.com/docs/messenger-platform/product-overview/launch#app_public), [quick start](https://developers.facebook.com/docs/messenger-platform/guides/quick-start), and [setup guide](https://developers.facebook.com/docs/messenger-platform/guides/setup).</span></span>

<span data-ttu-id="05b89-107">Para configurar un bot para que se comunique con Facebook Messenger, habilite Facebook Messenger en una página de Facebook y, a continuación, conecte el bot a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="05b89-107">To configure a bot to communicate using Facebook Messenger, enable Facebook Messenger on a Facebook page and then connect the bot to the app.</span></span>

[!INCLUDE [Channel Inspector intro](~/includes/snippet-channel-inspector.md)]

> [!NOTE]
> <span data-ttu-id="05b89-108">La UI de Facebook puede aparecer de forma ligeramente diferente dependiendo de la versión que use.</span><span class="sxs-lookup"><span data-stu-id="05b89-108">The Facebook UI may appear slightly different depending on which version you are using.</span></span>

## <a name="copy-the-page-id"></a><span data-ttu-id="05b89-109">Copia del id. de página</span><span class="sxs-lookup"><span data-stu-id="05b89-109">Copy the Page ID</span></span>

<span data-ttu-id="05b89-110">Se accede al bot a través de una página de Facebook.</span><span class="sxs-lookup"><span data-stu-id="05b89-110">The bot is accessed through a Facebook Page.</span></span> <span data-ttu-id="05b89-111">[Cree una nueva página de Facebook](https://www.facebook.com/bookmarks/pages) o vaya a una página existente.</span><span class="sxs-lookup"><span data-stu-id="05b89-111">[Create a new Facebook Page](https://www.facebook.com/bookmarks/pages) or go to an existing Page.</span></span>

* <span data-ttu-id="05b89-112">Abra la página **Sobre** de la página de Facebook y, a continuación, copie y guarde el **id. de página**.</span><span class="sxs-lookup"><span data-stu-id="05b89-112">Open the Facebook Page's **About** page and then copy and save the **Page ID**.</span></span>

## <a name="create-a-facebook-app"></a><span data-ttu-id="05b89-113">Creación de una aplicación de Facebook</span><span class="sxs-lookup"><span data-stu-id="05b89-113">Create a Facebook app</span></span>

<span data-ttu-id="05b89-114">[Cree una nueva aplicación de Facebook](https://developers.facebook.com/quickstarts/?platform=web) en la página y genere un id. y un secreto de aplicación.</span><span class="sxs-lookup"><span data-stu-id="05b89-114">[Create a new Facebook App](https://developers.facebook.com/quickstarts/?platform=web) on the Page and generate an App ID and App Secret for it.</span></span>

![Creación de un id. de aplicación](~/media/channels/FB-CreateAppId.png)

* <span data-ttu-id="05b89-116">Copie y guarde el **id. de aplicación** y el **secreto de aplicación**.</span><span class="sxs-lookup"><span data-stu-id="05b89-116">Copy and save the **App ID** and the **App Secret**.</span></span>

![Guardar el id. y el secreto de aplicación](~/media/channels/FB-get-appid.png)

<span data-ttu-id="05b89-118">Establezca el control deslizante "Allow API Access to App Settings" (Permitir el acceso de la API a la configuración de la aplicación) en "Sí".</span><span class="sxs-lookup"><span data-stu-id="05b89-118">Set the "Allow API Access to App Settings" slider to "Yes".</span></span>

![Configuración de la aplicación](~/media/bot-service-channel-connect-facebook/api_settings.png)

## <a name="enable-messenger"></a><span data-ttu-id="05b89-120">Habilitación de Messenger</span><span class="sxs-lookup"><span data-stu-id="05b89-120">Enable messenger</span></span>


<span data-ttu-id="05b89-121">Habilite Facebook Messenger en la nueva aplicación de Facebook.</span><span class="sxs-lookup"><span data-stu-id="05b89-121">Enable Facebook Messenger in the new Facebook App.</span></span>

* <span data-ttu-id="05b89-122">En la página **Configuración del producto** de la aplicación, haga clic en **Comenzar** y, a continuación, vuelva a hacer clic en **Comenzar**.</span><span class="sxs-lookup"><span data-stu-id="05b89-122">On the **Product Setup** page of the app, click **Get Started** and then click **Get Started** again.</span></span>


![Habilitación de Messenger](~/media/channels/FB-AddMessaging1.png)

## <a name="generate-a-page-access-token"></a><span data-ttu-id="05b89-124">Generación de un token de acceso a la página</span><span class="sxs-lookup"><span data-stu-id="05b89-124">Generate a Page Access Token</span></span>

<span data-ttu-id="05b89-125">En el panel **Generación de tokens** de la sección de Messenger, seleccione la página de destino.</span><span class="sxs-lookup"><span data-stu-id="05b89-125">In the **Token Generation** panel of the Messenger section, select the target Page.</span></span> <span data-ttu-id="05b89-126">Se generará un token de acceso a la página.</span><span class="sxs-lookup"><span data-stu-id="05b89-126">A Page Access Token will be generated.</span></span>

* <span data-ttu-id="05b89-127">Copie y guarde el **token de acceso a la página**.</span><span class="sxs-lookup"><span data-stu-id="05b89-127">Copy and save the **Page Access Token**.</span></span>

![Generación del token](~/media/channels/FB-generateToken.png)

## <a name="enable-webhooks"></a><span data-ttu-id="05b89-129">Habilitación de webhooks</span><span class="sxs-lookup"><span data-stu-id="05b89-129">Enable webhooks</span></span>

<span data-ttu-id="05b89-130">Haga clic en **Configurar webhooks** para reenviar eventos de mensajería de Facebook Messenger al bot.</span><span class="sxs-lookup"><span data-stu-id="05b89-130">Click **Set up Webhooks** to forward messaging events from Facebook Messenger to the bot.</span></span>

![Habilitación del webhook](~/media/channels/FB-webhook.png)

## <a name="provide-webhook-callback-url-and-verify-token"></a><span data-ttu-id="05b89-132">Aprovisionamiento de la dirección URL de devolución de llamada de webhook y comprobación del token</span><span class="sxs-lookup"><span data-stu-id="05b89-132">Provide webhook callback URL and verify token</span></span>

<span data-ttu-id="05b89-133">Vuelva al [portal de Bot Framework](https://dev.botframework.com/).</span><span class="sxs-lookup"><span data-stu-id="05b89-133">Return to the [Bot Framework Portal](https://dev.botframework.com/).</span></span> <span data-ttu-id="05b89-134">Haga clic en el bot, abra la pestaña **Canales** y, a continuación, haga clic en **Facebook Messenger**.</span><span class="sxs-lookup"><span data-stu-id="05b89-134">Open the bot, click the **Channels** tab, and then click **Facebook Messenger**.</span></span>

* <span data-ttu-id="05b89-135">Copie los valores **Dirección URL de devolución de llamada** y **Comprobar token** desde el portal.</span><span class="sxs-lookup"><span data-stu-id="05b89-135">Copy the **Callback URL** and **Verify Token** values from the portal.</span></span>

![Copia de valores](~/media/channels/fb-callbackVerify.png)

1. <span data-ttu-id="05b89-137">Vuelva a Facebook Messenger y pegue los valores **Dirección URL de devolución de llamada** y **Comprobar token**.</span><span class="sxs-lookup"><span data-stu-id="05b89-137">Return to Facebook Messenger and paste the **Callback URL** and **Verify Token** values.</span></span>

2. <span data-ttu-id="05b89-138">En **Campos de suscripción**, seleccione *message\_deliveries*, *messages*, *messaging\_options* y *messaging\_postbacks*.</span><span class="sxs-lookup"><span data-stu-id="05b89-138">Under **Subscription Fields**, select *message\_deliveries*, *messages*, *messaging\_options*, and *messaging\_postbacks*.</span></span>

3. <span data-ttu-id="05b89-139">Haga clic en **Comprobar y guardar**.</span><span class="sxs-lookup"><span data-stu-id="05b89-139">Click **Verify and Save**.</span></span>

![Configuración del webhook](~/media/channels/FB-webhookConfig.png)

4. <span data-ttu-id="05b89-141">Subscriba el webhook a la página de Facebook.</span><span class="sxs-lookup"><span data-stu-id="05b89-141">Subscribe the webhook to the Facebook page.</span></span>

![Suscripción del webhook](~/media/bot-service-channel-connect-facebook/subscribe-webhook.png)


## <a name="provide-facebook-credentials"></a><span data-ttu-id="05b89-143">Aprovisionamiento de credenciales de Facebook</span><span class="sxs-lookup"><span data-stu-id="05b89-143">Provide Facebook credentials</span></span>

<span data-ttu-id="05b89-144">En el portal de Bot Framework, pegue los valores **Id. de página**, **Id. de aplicación**, **Secreto de la aplicación** y **Token de acceso a la página** copiados de Facebook Messenger anteriormente.</span><span class="sxs-lookup"><span data-stu-id="05b89-144">On the Bot Framework Portal, paste the **Page ID**, **App ID**, **App Secret**, and **Page Access Token** values copied from Facebook Messenger previously.</span></span>

![Escritura de las credenciales](~/media/channels/fb-credentials2.png)

## <a name="submit-for-review"></a><span data-ttu-id="05b89-146">Envío para revisión</span><span class="sxs-lookup"><span data-stu-id="05b89-146">Submit for review</span></span>

<span data-ttu-id="05b89-147">Facebook requiere una dirección URL de la directiva de privacidad y una dirección URL de las Condiciones del servicio en su página de configuración de aplicación básica.</span><span class="sxs-lookup"><span data-stu-id="05b89-147">Facebook requires a Privacy Policy URL and Terms of Service URL on its basic app settings page.</span></span> <span data-ttu-id="05b89-148">La página [Código de conducta](https://aka.ms/bf-conduct) contiene vínculos a recursos de terceros para ayudar a crear una directiva de privacidad.</span><span class="sxs-lookup"><span data-stu-id="05b89-148">The [Code of Conduct](https://aka.ms/bf-conduct) page contains third party resource links to help create a privacy policy.</span></span> <span data-ttu-id="05b89-149">La página [Condiciones de uso](https://aka.ms/bf-terms) contiene los términos de ejemplo que le ayudarán a crear un documento de condiciones del servicio adecuado.</span><span class="sxs-lookup"><span data-stu-id="05b89-149">The [Terms of Use](https://aka.ms/bf-terms) page contains sample terms to help create an appropriate Terms of Service document.</span></span>

<span data-ttu-id="05b89-150">Al finalizar el bot, Facebook tiene su propio [proceso de revisión](https://developers.facebook.com/docs/messenger-platform/app-review) para las aplicaciones que se publican en Messenger.</span><span class="sxs-lookup"><span data-stu-id="05b89-150">After the bot is finished, Facebook has its own [review process](https://developers.facebook.com/docs/messenger-platform/app-review) for apps that are published to Messenger.</span></span> <span data-ttu-id="05b89-151">El bot se probará para asegurarse de que es compatible con las [políticas de la plataforma](https://developers.facebook.com/docs/messenger-platform/policy-overview) de Facebook.</span><span class="sxs-lookup"><span data-stu-id="05b89-151">The bot will be tested to ensure it is compliant with Facebook's [Platform Policies](https://developers.facebook.com/docs/messenger-platform/policy-overview).</span></span>

## <a name="make-the-app-public-and-publish-the-page"></a><span data-ttu-id="05b89-152">Publicación de la aplicación y la página</span><span class="sxs-lookup"><span data-stu-id="05b89-152">Make the App public and publish the Page</span></span>

> [!NOTE]
> <span data-ttu-id="05b89-153">Una aplicación se encuentra en [modo de desarrollo](https://developers.facebook.com/docs/apps/managing-development-cycle) hasta que no se publica.</span><span class="sxs-lookup"><span data-stu-id="05b89-153">Until an app is published, it is in [Development Mode](https://developers.facebook.com/docs/apps/managing-development-cycle).</span></span> <span data-ttu-id="05b89-154">Los complementos y las funcionalidades de API solo funcionarán para los administradores, los desarrolladores y los evaluadores.</span><span class="sxs-lookup"><span data-stu-id="05b89-154">Plugin and API functionality will only work for admins, developers, and testers.</span></span>

<span data-ttu-id="05b89-155">Una vez que la revisión se considere correcta, en el panel de aplicaciones en Revisión de la aplicación, establezca la aplicación en Pública.</span><span class="sxs-lookup"><span data-stu-id="05b89-155">After the review is successful, in the App Dashboard under App Review, set the app to Public.</span></span>
<span data-ttu-id="05b89-156">Asegúrese de que la página de Facebook asociada a este bot esté publicada.</span><span class="sxs-lookup"><span data-stu-id="05b89-156">Ensure that the Facebook Page associated with this bot is published.</span></span> <span data-ttu-id="05b89-157">El estado aparece en la configuración de las páginas.</span><span class="sxs-lookup"><span data-stu-id="05b89-157">Status appears in Pages settings.</span></span>
