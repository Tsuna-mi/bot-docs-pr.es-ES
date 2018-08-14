---
title: Conectar un bot a Cortana | Microsoft Docs
description: Obtenga información sobre cómo configurar un bot para acceder a través de la interfaz de Cortana.
keywords: cortana, canal de bot, configurar cortana
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 04/30/2018
ms.openlocfilehash: 6e694ce8b54ebd2405d7496d333c2bb27eb344f1
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305069"
---
# <a name="connect-a-bot-to-cortana"></a><span data-ttu-id="0eda5-104">Conectar un bot a Cortana</span><span class="sxs-lookup"><span data-stu-id="0eda5-104">Connect a bot to Cortana</span></span>

<span data-ttu-id="0eda5-105">Cortana es un canal habilitado para voz que puede enviar y recibir mensajes de voz, además de tener conversaciones por escrito.</span><span class="sxs-lookup"><span data-stu-id="0eda5-105">Cortana is a speech-enabled channel that can send and receive voice messages in addition to textual conversation.</span></span> <span data-ttu-id="0eda5-106">Un bot que se vaya a conectar a Cortana debe diseñarse para que admita tanto la voz y el texto.</span><span class="sxs-lookup"><span data-stu-id="0eda5-106">A bot intended to connect to Cortana should be designed for speech as well as text.</span></span> <span data-ttu-id="0eda5-107">Una *habilidad* de Cortana es un bot que se puede invocar con un cliente de Cortana.</span><span class="sxs-lookup"><span data-stu-id="0eda5-107">A Cortana *skill* is a bot that can be invoked using a Cortana client.</span></span> <span data-ttu-id="0eda5-108">Si publica un bot, se agregará a la lista de habilidades disponibles.</span><span class="sxs-lookup"><span data-stu-id="0eda5-108">Publishing a bot adds the bot to the list of available skills.</span></span>

<span data-ttu-id="0eda5-109">Para agregar el canal de Cortana, abra el bot en [Azure Portal](https://portal.azure.com/), haga clic en la hoja **Canales** y luego en **Cortana**.</span><span class="sxs-lookup"><span data-stu-id="0eda5-109">To add the Cortana channel, open the bot in the [Azure Portal](https://portal.azure.com/), click the **Channels** blade, and then click **Cortana**.</span></span>

![Agregar el canal de Cortana](~/media/channels/cortana-addchannel.png)

## <a name="configure-cortana"></a><span data-ttu-id="0eda5-111">Configurar Cortana</span><span class="sxs-lookup"><span data-stu-id="0eda5-111">Configure Cortana</span></span>

<span data-ttu-id="0eda5-112">Al conectar su bot con el canal de Cortana, parte de la información básica sobre el mismo se completará previamente en el formulario de registro.</span><span class="sxs-lookup"><span data-stu-id="0eda5-112">When connecting your bot with the Cortana channel, some basic information about your bot will be pre-filled into the registration form.</span></span> <span data-ttu-id="0eda5-113">Revise esta información detenidamente.</span><span class="sxs-lookup"><span data-stu-id="0eda5-113">Review this information carefully.</span></span> <span data-ttu-id="0eda5-114">Este formulario consta de los siguientes campos:</span><span class="sxs-lookup"><span data-stu-id="0eda5-114">This form consists of the following fields.</span></span>

| <span data-ttu-id="0eda5-115">Campo</span><span class="sxs-lookup"><span data-stu-id="0eda5-115">Field</span></span> | <span data-ttu-id="0eda5-116">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="0eda5-116">Description</span></span> |
|------|------|
| <span data-ttu-id="0eda5-117">**Icono de habilidades**</span><span class="sxs-lookup"><span data-stu-id="0eda5-117">**Skill icon**</span></span> | <span data-ttu-id="0eda5-118">Un icono que se muestra en el lienzo de Cortana cuando se invoca la habilidad.</span><span class="sxs-lookup"><span data-stu-id="0eda5-118">An icon that is displayed in the Cortana canvas when your skill is invoked.</span></span> <span data-ttu-id="0eda5-119">También se usa cuando las habilidades son detectables (como Microsoft Store).</span><span class="sxs-lookup"><span data-stu-id="0eda5-119">This is also used where skills are discoverable (like the Microsoft store).</span></span> <span data-ttu-id="0eda5-120">(32 KB máximo; solo PNG).</span><span class="sxs-lookup"><span data-stu-id="0eda5-120">(32KB max, PNG only).</span></span>|
| <span data-ttu-id="0eda5-121">**Nombre para mostrar**</span><span class="sxs-lookup"><span data-stu-id="0eda5-121">**Display name**</span></span> | <span data-ttu-id="0eda5-122">El nombre de la habilidad de Cortana se muestra al usuario en la parte superior de la interfaz de usuario visual.</span><span class="sxs-lookup"><span data-stu-id="0eda5-122">The name of your Cortana skill is displayed to the user at the top of the visual UI.</span></span> <span data-ttu-id="0eda5-123">(Límite de 30 caracteres).</span><span class="sxs-lookup"><span data-stu-id="0eda5-123">(30 character limit)</span></span> |
| <span data-ttu-id="0eda5-124">**Nombre de invocación**</span><span class="sxs-lookup"><span data-stu-id="0eda5-124">**Invocation name**</span></span> | <span data-ttu-id="0eda5-125">Este es el nombre que los usuarios deben decir al invocar una habilidad.</span><span class="sxs-lookup"><span data-stu-id="0eda5-125">This is the name users say when invoking a skill.</span></span> <span data-ttu-id="0eda5-126">No debe tener más de tres palabras y debe ser fácil pronunciar.</span><span class="sxs-lookup"><span data-stu-id="0eda5-126">It should be no more than three words and easy to pronounce.</span></span> <span data-ttu-id="0eda5-127">Consulte la [Invocation Name Guidelines][invocation] (Información del nombre de invocación) para obtener más información sobre cómo elegir este nombre.</span><span class="sxs-lookup"><span data-stu-id="0eda5-127">See the [Invocation Name Guidelines][invocation] for more information on how to choose this name.</span></span>|
| <span data-ttu-id="0eda5-128">**Descripción**</span><span class="sxs-lookup"><span data-stu-id="0eda5-128">**Description**</span></span> | <span data-ttu-id="0eda5-129">Una descripción de la habilidad de Cortana.</span><span class="sxs-lookup"><span data-stu-id="0eda5-129">A description of your Cortana skill.</span></span> <span data-ttu-id="0eda5-130">Se usa cuando las habilidades son detectables (como Microsoft Store).</span><span class="sxs-lookup"><span data-stu-id="0eda5-130">This is used where skills are discoverable (like the Microsoft store).</span></span> |
| <span data-ttu-id="0eda5-131">**Descripción breve**</span><span class="sxs-lookup"><span data-stu-id="0eda5-131">**Short description**</span></span> | <span data-ttu-id="0eda5-132">Breve descripción de la funcionalidad de la habilidad; se usa para describir la habilidad en el cuaderno de Cortana.</span><span class="sxs-lookup"><span data-stu-id="0eda5-132">A short description of your skill’s functionality, used to describe the skill in Cortana’s notebook.</span></span> |

## <a name="general-bot-information"></a><span data-ttu-id="0eda5-133">Información general sobre el bot</span><span class="sxs-lookup"><span data-stu-id="0eda5-133">General bot information</span></span>

<span data-ttu-id="0eda5-134">En la sección **Manage user identity through connected services** (Administrar la identidad del usuario a través de los servicios conectados), seleccione esta opción para habilitarla.</span><span class="sxs-lookup"><span data-stu-id="0eda5-134">Under the **Manage user identity through connected services section** press the option to enable it.</span></span> <span data-ttu-id="0eda5-135">Rellene el formulario.</span><span class="sxs-lookup"><span data-stu-id="0eda5-135">Fill in the form.</span></span>

<span data-ttu-id="0eda5-136">Todos los campos marcados con un asterisco (\*) son obligatorios.</span><span class="sxs-lookup"><span data-stu-id="0eda5-136">All fields marked with an asterisk (\*) are required.</span></span> <span data-ttu-id="0eda5-137">Los bots deben publicarse en Bot Framework antes de conectarlos Cortana.</span><span class="sxs-lookup"><span data-stu-id="0eda5-137">Bots must be published on the Bot Framework before they can be connected to Cortana.</span></span>

![Proporcionar información general](~/media/channels/cortana-details.png)

### <a name="sign-in-at-invocation"></a><span data-ttu-id="0eda5-139">Iniciar sesión en la invocación</span><span class="sxs-lookup"><span data-stu-id="0eda5-139">Sign in at invocation</span></span>

<span data-ttu-id="0eda5-140">Seleccione esta opción si quiere que Cortana inicie la sesión del usuario en el momento en que este último invoque la habilidad.</span><span class="sxs-lookup"><span data-stu-id="0eda5-140">Select this option if you want Cortana to sign in the user at the time they invoke your skill.</span></span>

### <a name="sign-in-when-required"></a><span data-ttu-id="0eda5-141">Iniciar sesión cuando se solicite</span><span class="sxs-lookup"><span data-stu-id="0eda5-141">Sign in when required</span></span>

<span data-ttu-id="0eda5-142">Seleccione esta opción si usa una tarjeta SignIn de Bot Framework para iniciar la sesión del usuario.</span><span class="sxs-lookup"><span data-stu-id="0eda5-142">Select this option if you use a Bot Framework's SignIn card to sign in the user.</span></span> <span data-ttu-id="0eda5-143">Puede usar esta opción si quiere iniciar la sesión del usuario solo este si utiliza una función que requiera autenticación.</span><span class="sxs-lookup"><span data-stu-id="0eda5-143">Typically, you use this option if you want to sign in the user only if they use a feature that requires authentication.</span></span> <span data-ttu-id="0eda5-144">Cuando su habilidad envía un mensaje que incluye la tarjeta SignIn como un archivo adjunto, Cortana ignora la tarjeta SignIn y realiza el flujo de autorización usando la configuración para conectar la cuenta.</span><span class="sxs-lookup"><span data-stu-id="0eda5-144">When your skill sends a message that includes the SignIn card as an attachment, Cortana ignores the SignIn card and performs the authorization flow using the Connect Account settings.</span></span>

### <a name="connected-service-icon"></a><span data-ttu-id="0eda5-145">Icono de servicio conectado</span><span class="sxs-lookup"><span data-stu-id="0eda5-145">Connected service icon</span></span>

<span data-ttu-id="0eda5-146">El icono que quiere mostrar cuando el usuario inicie sesión en la habilidad.</span><span class="sxs-lookup"><span data-stu-id="0eda5-146">The icon that you want displayed when the user signs in to your skill.</span></span>

### <a name="account-name"></a><span data-ttu-id="0eda5-147">Nombre de cuenta</span><span class="sxs-lookup"><span data-stu-id="0eda5-147">Account name</span></span>

<span data-ttu-id="0eda5-148">El nombre de habilidad que quiere mostrar cuando el usuario inicie sesión en la misma.</span><span class="sxs-lookup"><span data-stu-id="0eda5-148">The name of your skill that you want displayed when the user signs in to your skill.</span></span>

### <a name="client-id-for-third-party-services"></a><span data-ttu-id="0eda5-149">Id. de cliente de servicios de terceros</span><span class="sxs-lookup"><span data-stu-id="0eda5-149">Client ID for third-party services</span></span>

<span data-ttu-id="0eda5-150">Identificador de aplicación del bot.</span><span class="sxs-lookup"><span data-stu-id="0eda5-150">Your bot's application ID.</span></span> <span data-ttu-id="0eda5-151">Recibió el identificador al registrar el bot.</span><span class="sxs-lookup"><span data-stu-id="0eda5-151">You received the ID when you registered your bot.</span></span>

### <a name="space-separated-list-of-scopes"></a><span data-ttu-id="0eda5-152">Lista de ámbitos separada por espacios</span><span class="sxs-lookup"><span data-stu-id="0eda5-152">Space-separated list of scopes</span></span>

<span data-ttu-id="0eda5-153">Especifique los ámbitos que necesita el servicio (consulte la documentación del servicio).</span><span class="sxs-lookup"><span data-stu-id="0eda5-153">Specify the scopes that the service requires (see the service's documentation).</span></span>

### <a name="authorization-url"></a><span data-ttu-id="0eda5-154">Dirección URL de autorización</span><span class="sxs-lookup"><span data-stu-id="0eda5-154">Authorization URL</span></span>

<span data-ttu-id="0eda5-155">Establézcalo en `https://login.microsoftonline.com/common/oauth2/v2.0/authorize`.</span><span class="sxs-lookup"><span data-stu-id="0eda5-155">Set to `https://login.microsoftonline.com/common/oauth2/v2.0/authorize`.</span></span>

### <a name="grant-type"></a><span data-ttu-id="0eda5-156">Tipo de concesión</span><span class="sxs-lookup"><span data-stu-id="0eda5-156">Grant type</span></span>

<span data-ttu-id="0eda5-157">Seleccione el código de autorización para usar el flujo de concesión de código.</span><span class="sxs-lookup"><span data-stu-id="0eda5-157">Select Authorization code to use code grant flow.</span></span> <span data-ttu-id="0eda5-158">Seleccione Implícito para usar el flujo implícito.</span><span class="sxs-lookup"><span data-stu-id="0eda5-158">Select Implicit to use implicit flow.</span></span>

### <a name="token-url"></a><span data-ttu-id="0eda5-159">Dirección URL de token</span><span class="sxs-lookup"><span data-stu-id="0eda5-159">Token URL</span></span>

<span data-ttu-id="0eda5-160">Si selecciona el código de autorización, establézcalo en `https://login.microsoftonline.com/common/oauth2/v2.0/token`.</span><span class="sxs-lookup"><span data-stu-id="0eda5-160">If you select Authorization code, set to `https://login.microsoftonline.com/common/oauth2/v2.0/token`.</span></span>

### <a name="client-secretpassword-for-third-party-services"></a><span data-ttu-id="0eda5-161">Contraseña o secreto de cliente para servicios de terceros</span><span class="sxs-lookup"><span data-stu-id="0eda5-161">Client secret/password for third party services</span></span>

<span data-ttu-id="0eda5-162">Contraseña del bot.</span><span class="sxs-lookup"><span data-stu-id="0eda5-162">The bot's password.</span></span> <span data-ttu-id="0eda5-163">Recibió la contraseña al registrar el bot.</span><span class="sxs-lookup"><span data-stu-id="0eda5-163">You received the password when you registered your bot.</span></span>

### <a name="client-authentication-scheme"></a><span data-ttu-id="0eda5-164">Esquema de autenticación de clientes</span><span class="sxs-lookup"><span data-stu-id="0eda5-164">Client authentication scheme</span></span>

<span data-ttu-id="0eda5-165">Seleccione HTTP Basic.</span><span class="sxs-lookup"><span data-stu-id="0eda5-165">Select HTTP Basic.</span></span>

### <a name="token-options"></a><span data-ttu-id="0eda5-166">Opciones de token</span><span class="sxs-lookup"><span data-stu-id="0eda5-166">Token options</span></span>

<span data-ttu-id="0eda5-167">Establézcalo en POST.</span><span class="sxs-lookup"><span data-stu-id="0eda5-167">Set to POST.</span></span>

### <a name="request-user-profile-data-optional"></a><span data-ttu-id="0eda5-168">Solicitar datos de perfil de usuario (opcional)</span><span class="sxs-lookup"><span data-stu-id="0eda5-168">Request user profile data (optional)</span></span>

<span data-ttu-id="0eda5-169">Cortana proporciona acceso a diferentes tipos de información de perfil de usuario, que puede usar para personalizar el bot para el usuario.</span><span class="sxs-lookup"><span data-stu-id="0eda5-169">Cortana provides access to several different types of user profile information, that you can use to customize the bot for the user.</span></span> <span data-ttu-id="0eda5-170">Por ejemplo, si una habilidad tiene acceso al nombre y la ubicación del usuario, entonces la habilidad puede proporcionar una respuesta personalizada como "Hola Kamran, espero que estés teniendo un día agradable en Bellevue, Washington".</span><span class="sxs-lookup"><span data-stu-id="0eda5-170">For example, if a skill has access to the user’s name and location then the skill can have custom response such as “Hello Kamran, I hope you are having a pleasant day in Bellevue, Washington.”</span></span>

<span data-ttu-id="0eda5-171">Haga clic en el enlace **Agregar una solicitud de perfil de usuario** y seleccione en la lista desplegable la información del perfil de usuario que quiera usar.</span><span class="sxs-lookup"><span data-stu-id="0eda5-171">Click the **Add a user profile request** link, then select the user profile information you want from the drop-down list.</span></span> <span data-ttu-id="0eda5-172">Agregue un nombre descriptivo que se pueda usar para obtener acceso a esta información desde el código del bot.</span><span class="sxs-lookup"><span data-stu-id="0eda5-172">Add a friendly name to use to access this information from your bot's code.</span></span>

### <a name="save-skill"></a><span data-ttu-id="0eda5-173">Guardar la habilidad</span><span class="sxs-lookup"><span data-stu-id="0eda5-173">Save skill</span></span>

<span data-ttu-id="0eda5-174">Cuando haya completado el formulario de registro de su habilidad de Cortana, haga clic en Guardar para completar la conexión.</span><span class="sxs-lookup"><span data-stu-id="0eda5-174">When you are done filling out the registration form for your Cortana skill, click Save to complete the connection.</span></span> <span data-ttu-id="0eda5-175">Esto le llevará de vuelta a la hoja Canales del bot, donde verá que ya está conectado a Cortana.</span><span class="sxs-lookup"><span data-stu-id="0eda5-175">This brings you back to your bot's Channels blade and you should see that it is now connected to Cortana.</span></span>

<span data-ttu-id="0eda5-176">En este momento, su bot ya se ha implementado automáticamente como una habilidad de Cortana en su cuenta.</span><span class="sxs-lookup"><span data-stu-id="0eda5-176">At this point your bot has already been automatically deployed as a Cortana skill to your account.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0eda5-177">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="0eda5-177">Next steps</span></span>

* <span data-ttu-id="0eda5-178">[Cortana Skills Kit](https://aka.ms/CortanaSkillsDocs) (Kit de habilidades de Cortana)</span><span class="sxs-lookup"><span data-stu-id="0eda5-178">[Cortana Skills Kit](https://aka.ms/CortanaSkillsDocs)</span></span>
* [<span data-ttu-id="0eda5-179">Habilitar depuración</span><span class="sxs-lookup"><span data-stu-id="0eda5-179">Enable debugging</span></span>](bot-service-debug-cortana-skill.md)
* <span data-ttu-id="0eda5-180">[Publicar una habilidad de Cortana][publish]</span><span class="sxs-lookup"><span data-stu-id="0eda5-180">[Publish a Cortana skill][publish]</span></span>

[invocation]: https://docs.microsoft.com/en-us/cortana/skills/cortana-invocation-guidelines
[publish]: https://docs.microsoft.com/en-us/cortana/skills/publish-skill
[connected]: https://aka.ms/CortanaSkillsBotConnectedAccount
[CortanaEntity]: https://aka.ms/lgvcto
