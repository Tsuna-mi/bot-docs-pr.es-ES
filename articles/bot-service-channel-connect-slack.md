---
title: Conexión de un bot a Slack | Microsoft Docs
description: Obtenga información sobre cómo configurar la conexión de un bot a Slack.
keywords: conectar un bot, canal de bot, bot de Slack, aplicación de mensajería Slack
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ms.openlocfilehash: 56fe0af4d34e6e0aa4bc420112c541a410aa1301
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305108"
---
# <a name="connect-a-bot-to-slack"></a><span data-ttu-id="5a832-104">Conexión de un bot a Slack</span><span class="sxs-lookup"><span data-stu-id="5a832-104">Connect a bot to Slack</span></span>

<span data-ttu-id="5a832-105">Puede configurar el bot para que se comunique con las personas mediante la aplicación de mensajería Slack.</span><span class="sxs-lookup"><span data-stu-id="5a832-105">You can configure your bot to communicate with people using the Slack messaging app.</span></span>

## <a name="create-a-slack-application-for-your-bot"></a><span data-ttu-id="5a832-106">Creación de una aplicación de Slack para el bot</span><span class="sxs-lookup"><span data-stu-id="5a832-106">Create a Slack Application for your bot</span></span>

<span data-ttu-id="5a832-107">Inicie sesión en Slack y [cree una aplicación de Slack](https://api.slack.com/applications/new).</span><span class="sxs-lookup"><span data-stu-id="5a832-107">Log into Slack and [create a Slack application](https://api.slack.com/applications/new).</span></span>

![Configuración del bot](~/media/channels/slack-NewApp.png)

## <a name="create-an-app-and-assign-a-development-slack-team"></a><span data-ttu-id="5a832-109">Creación de una aplicación y asignación de un equipo de desarrollo de Slack</span><span class="sxs-lookup"><span data-stu-id="5a832-109">Create an app and assign a Development Slack team</span></span>

<span data-ttu-id="5a832-110">Escriba un nombre de aplicación y seleccione un equipo de desarrollo de Slack.</span><span class="sxs-lookup"><span data-stu-id="5a832-110">Enter an App Name and select a Development Slack Team.</span></span> <span data-ttu-id="5a832-111">Si aún no es miembro de un equipo de desarrollo de Slack, [cree uno o únase a uno](https://slack.com/).</span><span class="sxs-lookup"><span data-stu-id="5a832-111">If you are not already a member of a Development Slack Team, [create or join one](https://slack.com/).</span></span>

![Creación de aplicación](~/media/channels/slack-CreateApp.png)

<span data-ttu-id="5a832-113">Haga clic en **Create app** (Crear aplicación).</span><span class="sxs-lookup"><span data-stu-id="5a832-113">Click **Create App**.</span></span> <span data-ttu-id="5a832-114">Slack creará la aplicación y generará un id. de cliente y un secreto de cliente.</span><span class="sxs-lookup"><span data-stu-id="5a832-114">Slack will create your app and generate a Client ID and Client Secret.</span></span>

## <a name="add-a-new-redirect-url"></a><span data-ttu-id="5a832-115">Adición de una nueva dirección URL de redireccionamiento</span><span class="sxs-lookup"><span data-stu-id="5a832-115">Add a new Redirect URL</span></span>

<span data-ttu-id="5a832-116">A continuación, agregará una nueva dirección URL de redireccionamiento.</span><span class="sxs-lookup"><span data-stu-id="5a832-116">Next you will add a new Redirect URL.</span></span>

1. <span data-ttu-id="5a832-117">Seleccione la pestaña **OAuth & Permissions** (OAuth y permisos).</span><span class="sxs-lookup"><span data-stu-id="5a832-117">Select the **OAuth & Permissions** tab.</span></span>
2. <span data-ttu-id="5a832-118">Haga clic en **Add a new Redirect URL** (Adición de una nueva dirección URL de redireccionamiento).</span><span class="sxs-lookup"><span data-stu-id="5a832-118">Click **Add a new Redirect URL**.</span></span>
3. <span data-ttu-id="5a832-119">Escriba https://slack.botframework.com.</span><span class="sxs-lookup"><span data-stu-id="5a832-119">Enter https://slack.botframework.com.</span></span>
4. <span data-ttu-id="5a832-120">Haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="5a832-120">Click **Add**.</span></span>
5. <span data-ttu-id="5a832-121">Haga clic en **Save URLs** (Guardar direcciones URL).</span><span class="sxs-lookup"><span data-stu-id="5a832-121">Click **Save URLs**.</span></span>

![Agregar dirección URL de redireccionamiento](~/media/channels/slack-RedirectURL.png)

## <a name="create-a-slack-bot-user"></a><span data-ttu-id="5a832-123">Creación de un usuario de bot de Slack</span><span class="sxs-lookup"><span data-stu-id="5a832-123">Create a Slack Bot User</span></span>

<span data-ttu-id="5a832-124">Agregar un usuario de bot le permite asignar un nombre de usuario al bot y elegir si siempre se muestra como en línea.</span><span class="sxs-lookup"><span data-stu-id="5a832-124">Adding a Bot User allows you to assign a username for your bot and choose whether it is always shown as online.</span></span>

1. <span data-ttu-id="5a832-125">Seleccione la pestaña **Bot Users** (Usuarios de bot).</span><span class="sxs-lookup"><span data-stu-id="5a832-125">Select the **Bot Users** tab.</span></span>
2. <span data-ttu-id="5a832-126">Haga clic en **Add a Bot User** (Agregar un usuario de bot).</span><span class="sxs-lookup"><span data-stu-id="5a832-126">Click **Add a Bot User**.</span></span>

![Crear bot](~/media/channels/slack-CreateBot.png)

<span data-ttu-id="5a832-128">Haga clic en **Add Bot User** para validar la configuración, haga clic en **Always Show My Bot as Online** (Mostrar siempre mi bot como en línea) para **activar** y, a continuación, haga clic en **Guardar cambios**.</span><span class="sxs-lookup"><span data-stu-id="5a832-128">Click **Add Bot User** to validate your settings, click **Always Show My Bot as Online** to **On**, and then click **Save Changes**.</span></span>

![Crear bot](~/media/channels/slack-CreateApp-AddBotUser.png)

## <a name="subscribe-to-bot-events"></a><span data-ttu-id="5a832-130">Suscribirse a eventos de bot</span><span class="sxs-lookup"><span data-stu-id="5a832-130">Subscribe to Bot Events</span></span>

<span data-ttu-id="5a832-131">Siga estos pasos para suscribirse a seis eventos determinados de bot.</span><span class="sxs-lookup"><span data-stu-id="5a832-131">Follow these steps to subscribe to six particular bot events.</span></span> <span data-ttu-id="5a832-132">Si se suscribe a eventos de bot, se notificará a la aplicación acerca de las actividades de los usuarios en la dirección URL que especifique.</span><span class="sxs-lookup"><span data-stu-id="5a832-132">By subscribing to bot events, your app will be notified of user activities at the URL you specify.</span></span>

> [!TIP]
> <span data-ttu-id="5a832-133">El identificador de bot es una propiedad de su bot.</span><span class="sxs-lookup"><span data-stu-id="5a832-133">Your bot handle is a property of your bot.</span></span> <span data-ttu-id="5a832-134">Para obtener el identificador de un bot, visite [https://dev.botframework.com/bots](https://dev.botframework.com/bots), elija un bot y haga clic en **SETTINGS** (Configuración).</span><span class="sxs-lookup"><span data-stu-id="5a832-134">To find a bot's handle, visit [https://dev.botframework.com/bots](https://dev.botframework.com/bots), choose a bot, and click **SETTINGS**.</span></span>

1. <span data-ttu-id="5a832-135">Seleccione la pestaña **Suscripciones a eventos**.</span><span class="sxs-lookup"><span data-stu-id="5a832-135">Select the **Event Subscriptions** tab.</span></span>
2. <span data-ttu-id="5a832-136">Haga clic en **Habilitar eventos** para **activar**.</span><span class="sxs-lookup"><span data-stu-id="5a832-136">Click **Enable Events** to **On**.</span></span>
3. <span data-ttu-id="5a832-137">En **Dirección URL de la solicitud**, escriba esta dirección URL, pero reemplace `{YourBotHandle}` con el identificador del bot.</span><span class="sxs-lookup"><span data-stu-id="5a832-137">In **Request URL**, enter this URL, but replace `{YourBotHandle}` with your bot handle.</span></span>
        `https://slack.botframework.com/api/Events/{YourBotHandle}`
4. <span data-ttu-id="5a832-138">En **Subscribe to Bot Events** (suscribirse a eventos de Bot), haga clic en **Add Bot User Event** (Agregar evento de usuario de bot).</span><span class="sxs-lookup"><span data-stu-id="5a832-138">In **Subscribe to Bot Events**, click **Add Bot User Event**.</span></span>
5. <span data-ttu-id="5a832-139">En la lista de eventos, haga clic en **Add Bot User Event** y seleccione estos seis tipos de evento:</span><span class="sxs-lookup"><span data-stu-id="5a832-139">In the list of events, click Add **Bot User Event** and select these six event types:</span></span>
    * `member_joined_channel`
    * `member_left_channel`
    * `message.channels`
    * `message.groups`
    * `message.im`
    * `message.mpim`
6. <span data-ttu-id="5a832-140">Haga clic en **Guardar cambios**.</span><span class="sxs-lookup"><span data-stu-id="5a832-140">Click **Save Changes**.</span></span>

![Suscribirse a eventos](~/media/channels/slack-EnableEvents.png)

## <a name="add-and-configure-interactive-messages-optional"></a><span data-ttu-id="5a832-142">Adición y configuración de mensajes interactivos (opcional)</span><span class="sxs-lookup"><span data-stu-id="5a832-142">Add and Configure Interactive Messages (optional)</span></span>

<span data-ttu-id="5a832-143">Si el bot usará funcionalidades específicas de Slack, como botones, siga estos pasos:</span><span class="sxs-lookup"><span data-stu-id="5a832-143">If your bot will use Slack-specific functionality such as buttons, follow these steps:</span></span>

1. <span data-ttu-id="5a832-144">Seleccione la pestaña **Interactive Components** (Componentes interactivos) y haga clic en **Enable Interactive Components** (Habilitar componentes interactivos).</span><span class="sxs-lookup"><span data-stu-id="5a832-144">Select the **Interactive Components** tab and click **Enable Interactive Components**.</span></span>
2. <span data-ttu-id="5a832-145">Escriba `https://slack.botframework.com/api/Actions` como **Dirección URL de la solicitud**.</span><span class="sxs-lookup"><span data-stu-id="5a832-145">Enter `https://slack.botframework.com/api/Actions` as the **Request URL**.</span></span>
3. <span data-ttu-id="5a832-146">Haga clic en el botón **Enable Interactive Messages** (Habilitar mensajes interactivos) y, a continuación, haga clic en el botón **Guardar cambios**.</span><span class="sxs-lookup"><span data-stu-id="5a832-146">Click the **Enable Interactive Messages** button, and then click the **Save changes** button.</span></span>

![Habilitar mensajes](~/media/channels/slack-MessageURL.png)

## <a name="gather-credentials"></a><span data-ttu-id="5a832-148">Obtención de credenciales</span><span class="sxs-lookup"><span data-stu-id="5a832-148">Gather credentials</span></span>

<span data-ttu-id="5a832-149">Seleccione la pestaña **Información básica** y desplácese hasta la sección **Credenciales de la aplicación**.</span><span class="sxs-lookup"><span data-stu-id="5a832-149">Select the **Basic Information** tab and scroll to the **App Credentials** section.</span></span>
<span data-ttu-id="5a832-150">Se muestran el id. de cliente, el secreto de cliente y el token de comprobación necesario para la configuración de un bot de Slack.</span><span class="sxs-lookup"><span data-stu-id="5a832-150">The Client ID, Client Secret, and Verification Token required for configuration of your Slack bot are displayed.</span></span>

![Obtención de credenciales](~/media/channels/slack-AppCredentials.png)

## <a name="submit-credentials"></a><span data-ttu-id="5a832-152">Envío de credenciales</span><span class="sxs-lookup"><span data-stu-id="5a832-152">Submit credentials</span></span>

<span data-ttu-id="5a832-153">En otra ventana del explorador, vuelva al sitio de Bot Framework en `https://dev.botframework.com/`.</span><span class="sxs-lookup"><span data-stu-id="5a832-153">In a separate browser window, return to the Bot Framework site at `https://dev.botframework.com/`.</span></span>

1. <span data-ttu-id="5a832-154">Seleccione **Mis bots** y elija el bot que desea que se conecte con Slack.</span><span class="sxs-lookup"><span data-stu-id="5a832-154">Select **My bots** and choose the Bot that you want to connect to Slack.</span></span>
2. <span data-ttu-id="5a832-155">En la sección **Add a channel** (Agregar un canal), haga clic en el icono de Slack.</span><span class="sxs-lookup"><span data-stu-id="5a832-155">In the **Add a channel** section, click the Slack icon.</span></span>
3. <span data-ttu-id="5a832-156">En la sección **Enter your Slack credentials** (Escriba sus credenciales de Slack), pegue las credenciales de la aplicación desde el sitio web de Slack en los campos correspondientes.</span><span class="sxs-lookup"><span data-stu-id="5a832-156">In the **Enter your Slack credentials** section, paste the App Credentials from the Slack website into the appropriate fields.</span></span>
4. <span data-ttu-id="5a832-157">La **Landing Page URL** (Dirección URL de la página de aterrizaje) es opcional.</span><span class="sxs-lookup"><span data-stu-id="5a832-157">The **Landing Page URL** is optional.</span></span> <span data-ttu-id="5a832-158">Puede omitirla o cambiarla.</span><span class="sxs-lookup"><span data-stu-id="5a832-158">You may omit or change it.</span></span>
5. <span data-ttu-id="5a832-159">Haga clic en **Save**(Guardar).</span><span class="sxs-lookup"><span data-stu-id="5a832-159">Click **Save**.</span></span>

![Envío de credenciales](~/media/channels/slack-SubmitCredentials.png)

<span data-ttu-id="5a832-161">Siga las instrucciones para autorizar el acceso de la aplicación Slack a su equipo de desarrollo de Slack.</span><span class="sxs-lookup"><span data-stu-id="5a832-161">Follow the instructions to authorize your Slack app's access to your Development Slack Team.</span></span>

## <a name="enable-the-bot"></a><span data-ttu-id="5a832-162">Habilitación del bot</span><span class="sxs-lookup"><span data-stu-id="5a832-162">Enable the bot</span></span>

<span data-ttu-id="5a832-163">En la página Configure Slack (Configurar Slack), confirme que el control deslizante junto al botón Guardar esté establecido en **Habilitado**.</span><span class="sxs-lookup"><span data-stu-id="5a832-163">On the Configure Slack page, confirm the slider by the Save button is set to **Enabled**.</span></span>
<span data-ttu-id="5a832-164">El bot está configurado para comunicarse con los usuarios en Slack.</span><span class="sxs-lookup"><span data-stu-id="5a832-164">Your bot is configured to communicate with users in Slack.</span></span>

## <a name="create-an-add-to-slack-button"></a><span data-ttu-id="5a832-165">Creación de un botón Agregar a Slack</span><span class="sxs-lookup"><span data-stu-id="5a832-165">Create an Add to Slack button</span></span>

<span data-ttu-id="5a832-166">Slack proporciona el código HTML que se puede usar para ayudar a los usuarios de Slack a encontrar su bot en la sección *Add the Slack button* (Agregar el botón de Slack) de [esta página](https://api.slack.com/docs/slack-button).</span><span class="sxs-lookup"><span data-stu-id="5a832-166">Slack provides HTML you can use to help Slack users find your bot in the *Add the Slack button* section of [this page](https://api.slack.com/docs/slack-button).</span></span>
<span data-ttu-id="5a832-167">Para usar este código HTML con el bot, reemplace el valor de href (comienza con `https://`) por la dirección URL que se encuentra en configuración del canal de Slack del bot.</span><span class="sxs-lookup"><span data-stu-id="5a832-167">To use this HTML with your bot, replace the href value (begins with `https://`) with the URL found in your bot's Slack channel settings.</span></span>
<span data-ttu-id="5a832-168">Siga estos pasos para obtener la dirección URL de reemplazo.</span><span class="sxs-lookup"><span data-stu-id="5a832-168">Follow these steps to get the replacement URL.</span></span>

1. <span data-ttu-id="5a832-169">En [https://dev.botframework.com/bots](https://dev.botframework.com/bots), haga clic en el bot.</span><span class="sxs-lookup"><span data-stu-id="5a832-169">On [https://dev.botframework.com/bots](https://dev.botframework.com/bots), click your bot.</span></span>
2. <span data-ttu-id="5a832-170">Haga clic en **CHANNELS** (Canales), haga clic con el botón derecho en la entrada denominada **Slack** y haga clic en **Copy link** (Copiar vínculo).</span><span class="sxs-lookup"><span data-stu-id="5a832-170">Click **CHANNELS**, right-click the entry named **Slack**, and click **Copy link**.</span></span> <span data-ttu-id="5a832-171">Esta dirección URL ahora está en el Portapapeles.</span><span class="sxs-lookup"><span data-stu-id="5a832-171">This URL is now in your clipboard.</span></span>
3. <span data-ttu-id="5a832-172">Pegue esta dirección URL desde el Portapapeles en el código HTML proporcionado para el botón de Slack.</span><span class="sxs-lookup"><span data-stu-id="5a832-172">Paste this URL from your clipboard into the HTML provided for the Slack button.</span></span> <span data-ttu-id="5a832-173">Esta dirección URL reemplaza el valor de href proporcionado por Slack para este bot.</span><span class="sxs-lookup"><span data-stu-id="5a832-173">This URL replaces the href value provided by Slack for this bot.</span></span>

<span data-ttu-id="5a832-174">Los usuarios autorizados pueden hacer clic en el botón **Agregar a Slack** proporcionado por este código HTML modificado para comunicarse con el bot en Slack.</span><span class="sxs-lookup"><span data-stu-id="5a832-174">Authorized users can click the **Add to Slack** button provided by this modified HTML to reach your bot on Slack.</span></span>
