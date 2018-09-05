---
title: Incorporación de autenticación al bot mediante Azure Bot Service | Microsoft Docs
description: Obtenga información sobre cómo usar las características de autenticación de Azure Bot Service para agregar el inicio de sesión único al bot.
author: JonathanFingold
ms.author: JonathanFingold
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 7/2/2018
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: ca9ccb0f0425bd0cabf1ce7ef236acb7a008aefe
ms.sourcegitcommit: 2dc75701b169d822c9499e393439161bc87639d2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42905916"
---
<span data-ttu-id="fd707-103">En este tutorial se usan las nuevas funciones de autenticación de bot de Azure Bot Service, que proporciona características para facilitar el desarrollo de un bot que autentica a los usuarios en varios proveedores de identidades como Azure AD (Azure Active Directory), GitHub, Uber y así sucesivamente.</span><span class="sxs-lookup"><span data-stu-id="fd707-103">This tutorial uses new bot authentication capabilities in Azure Bot Service, providing features to make it easier to develop a bot that authenticates users to various identity providers such as Azure AD (Azure Active Directory), GitHub, Uber, and so on.</span></span> <span data-ttu-id="fd707-104">Estas actualizaciones también adoptan pasos hacia una mejor experiencia del usuario mediante la eliminación de la _comprobación del código mágico_ para algunos clientes.</span><span class="sxs-lookup"><span data-stu-id="fd707-104">These updates also take steps towards an improved user experience by eliminating the _magic code verification_ for some clients.</span></span>

<span data-ttu-id="fd707-105">Antes de esto, era necesario que el bot incluyera controladores de OAuth y vínculos de inicio de sesión, que almacenara los identificadores de cliente de destino y los secretos, y que realizara la administración de los tokens de usuario.</span><span class="sxs-lookup"><span data-stu-id="fd707-105">Prior to this, your bot needed to include OAuth controllers and login links, store the target client IDs and secrets, and perform user token management.</span></span>
<!--
These capabilities were bundled in the BotAuth and AuthBot samples that are on GitHub.
-->

<span data-ttu-id="fd707-106">Ahora, ya no es necesario que los desarrolladores de bots hospeden controladores de OAuth o administren el ciclo de vida de los tokens, ya que todo esto se puede realizar mediante Azure Bot Service.</span><span class="sxs-lookup"><span data-stu-id="fd707-106">Now, bot developers no longer need to host OAuth controllers or manage the token life-cycle, as all of this can now be done by the Azure Bot Service.</span></span>

<span data-ttu-id="fd707-107">Las características incluyen:</span><span class="sxs-lookup"><span data-stu-id="fd707-107">The features include:</span></span>

- <span data-ttu-id="fd707-108">Mejoras en los canales para admitir características de autenticación nuevas, como las bibliotecas WebChat y DirectLineJS nuevas para eliminar la necesidad de la comprobación de código mágico de seis dígitos.</span><span class="sxs-lookup"><span data-stu-id="fd707-108">Improvements to the channels to support new authentication features, such as new WebChat and DirectLineJS libraries to eliminate the need for the 6-digit magic code verification.</span></span>
- <span data-ttu-id="fd707-109">Mejoras en Azure Portal para agregar, eliminar y configurar opciones de conexión a distintos proveedores de identidades de OAuth.</span><span class="sxs-lookup"><span data-stu-id="fd707-109">Improvements to the Azure Portal to  add, delete, and configure connection settings to various OAuth identity providers.</span></span>
- <span data-ttu-id="fd707-110">Compatibilidad con una variedad de proveedores de identidades estándar como Azure AD (los puntos de conexión v1 y v2), GitHub y otros.</span><span class="sxs-lookup"><span data-stu-id="fd707-110">Support for a variety of out-of-the-box identity providers including Azure AD (both v1 and v2 endpoints), GitHub, and others.</span></span>
- <span data-ttu-id="fd707-111">Actualizaciones en los SDK de Bot Builder para C# y Node.js con el fin de poder recuperar los tokens, crear OAuthCards y controlar eventos de TokenResponse.</span><span class="sxs-lookup"><span data-stu-id="fd707-111">Updates to the C# and Node.js Bot Builder SDKs to be able to retrieve tokens, create OAuthCards and handle TokenResponse events.</span></span>
- <span data-ttu-id="fd707-112">Ejemplos de cómo crear un bot que se autentique en Azure AD (puntos de conexión v1 y v2) y GitHub.</span><span class="sxs-lookup"><span data-stu-id="fd707-112">Samples for how to make a bot that authenticates to Azure AD (v1 and v2 endpoints) and to GitHub.</span></span>

<span data-ttu-id="fd707-113">Puede extrapolar a partir de los pasos descritos en este artículo para agregar estas características a un bot existente.</span><span class="sxs-lookup"><span data-stu-id="fd707-113">You can extrapolate from the steps in this article to add such features to an existing bot.</span></span> <span data-ttu-id="fd707-114">Estos son los bots de ejemplo que muestran las nuevas características de autenticación</span><span class="sxs-lookup"><span data-stu-id="fd707-114">The following are sample bots that demonstrate the new authentication features</span></span>

| <span data-ttu-id="fd707-115">Muestra</span><span class="sxs-lookup"><span data-stu-id="fd707-115">Sample</span></span> | <span data-ttu-id="fd707-116">Versión de BotBuilder</span><span class="sxs-lookup"><span data-stu-id="fd707-116">BotBuilder version</span></span> | <span data-ttu-id="fd707-117">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="fd707-117">Description</span></span> |
|:---|:---:|:---|
| [<span data-ttu-id="fd707-118">AadV1Bot</span><span class="sxs-lookup"><span data-stu-id="fd707-118">AadV1Bot</span></span>](https://github.com/Microsoft/BotBuilder/tree/master/CSharp/Samples/AadV1Bot) | <span data-ttu-id="fd707-119">v3</span><span class="sxs-lookup"><span data-stu-id="fd707-119">v3</span></span> | <span data-ttu-id="fd707-120">Muestra la compatibilidad de OAuthCard en el SDK v3 de C#, mediante el punto de conexión v1 de Azure AD</span><span class="sxs-lookup"><span data-stu-id="fd707-120">Demonstrates OAuthCard support in the v3 C# SDK, using the Azure AD v1 endpoint</span></span> |
| [<span data-ttu-id="fd707-121">AadV2Bot</span><span class="sxs-lookup"><span data-stu-id="fd707-121">AadV2Bot</span></span>](https://github.com/Microsoft/BotBuilder/tree/master/CSharp/Samples/AadV2Bot) | <span data-ttu-id="fd707-122">v3</span><span class="sxs-lookup"><span data-stu-id="fd707-122">v3</span></span> |  <span data-ttu-id="fd707-123">Muestra la compatibilidad de OAuthCard en el SDK v3 de C#, mediante el punto de conexión v2 de Azure AD</span><span class="sxs-lookup"><span data-stu-id="fd707-123">Demonstrates OAuthCard support in the v3 C# SDK, using the Azure AD v2 endpoint</span></span> |
| [<span data-ttu-id="fd707-124">GitHubBot</span><span class="sxs-lookup"><span data-stu-id="fd707-124">GitHubBot</span></span>](https://github.com/Microsoft/BotBuilder/tree/master/CSharp/Samples/GitHubBot) | <span data-ttu-id="fd707-125">v3</span><span class="sxs-lookup"><span data-stu-id="fd707-125">v3</span></span> |  <span data-ttu-id="fd707-126">Muestra la compatibilidad de OAuthCard en el SDK v3 de C#, mediante GitHub</span><span class="sxs-lookup"><span data-stu-id="fd707-126">Demonstrates OAuthCard support in the v3 C# SDK, using GitHub</span></span> |
| [<span data-ttu-id="fd707-127">BasicOAuth</span><span class="sxs-lookup"><span data-stu-id="fd707-127">BasicOAuth</span></span>](https://github.com/Microsoft/BotBuilder/tree/master/CSharp/Samples/Microsoft.Bot.Sample.BasicOAuth) | <span data-ttu-id="fd707-128">v3</span><span class="sxs-lookup"><span data-stu-id="fd707-128">v3</span></span> |  <span data-ttu-id="fd707-129">Muestra la compatibilidad de OAuth 2.0 en el SDK v3 de C#</span><span class="sxs-lookup"><span data-stu-id="fd707-129">Demonstrates OAuth 2.0 support in the v3 C# SDK</span></span> |

> [!NOTE]
> <span data-ttu-id="fd707-130">Las características de autenticación también funcionan con Node.js con BotBuilder v3.</span><span class="sxs-lookup"><span data-stu-id="fd707-130">The authentication features also work with Node.js with BotBuilder v3.</span></span> <span data-ttu-id="fd707-131">Pero en este artículo solo se describe código de C# de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="fd707-131">However, this article covers just sample C# code.</span></span>

<span data-ttu-id="fd707-132">Para obtener más información y soporte técnico, vea [Bot Framework additional resources](https://docs.microsoft.com/azure/bot-service/bot-service-resources-links-help) (Recursos adicionales de Bot Framework).</span><span class="sxs-lookup"><span data-stu-id="fd707-132">For additional information and support, refer to [Bot Framework additional resources](https://docs.microsoft.com/azure/bot-service/bot-service-resources-links-help).</span></span>

## <a name="overview"></a><span data-ttu-id="fd707-133">Información general</span><span class="sxs-lookup"><span data-stu-id="fd707-133">Overview</span></span>

<span data-ttu-id="fd707-134">En este tutorial se crea un bot de ejemplo que se conecta a Microsoft Graph mediante un token v1 o v2 de Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fd707-134">This tutorial creates a sample bot that connects to the Microsoft Graph using an Azure AD v1 or v2 token.</span></span> <span data-ttu-id="fd707-135"><!--verify this info and fix wording--> Como parte de este proceso, se usará código de un repositorio de GitHub y en este tutorial se describe cómo configurarlo, incluida la aplicación de bot.</span><span class="sxs-lookup"><span data-stu-id="fd707-135"><!--verify this info and fix wording--> As part of this process, you'll use code from a GitHub repo, and this tutorial describes how to set that up, including the bot application.</span></span>

- [<span data-ttu-id="fd707-136">Crear el bot y una aplicación de autenticación</span><span class="sxs-lookup"><span data-stu-id="fd707-136">Create your bot and an authentication application</span></span>](#create-your-bot-and-an-authentication-application)
- [<span data-ttu-id="fd707-137">Preparar el código de ejemplo del bot</span><span class="sxs-lookup"><span data-stu-id="fd707-137">Prepare the bot sample code</span></span>](#prepare-the-bot-sample-code)
- [<span data-ttu-id="fd707-138">Usar el emulador para probar el bot</span><span class="sxs-lookup"><span data-stu-id="fd707-138">Use the Emulator to test your bot</span></span>](#use-the-emulator-to-test-your-bot)

<span data-ttu-id="fd707-139">Para completar estos pasos, necesitará instalar Visual Studio 2017, npm, node y gitt.</span><span class="sxs-lookup"><span data-stu-id="fd707-139">To complete these steps, you will need Visual Studio 2017, npm, node, and git installed.</span></span> <span data-ttu-id="fd707-140">También debe tener cierta familiaridad con Azure, OAuth 2.0 y el desarrollo de bots.</span><span class="sxs-lookup"><span data-stu-id="fd707-140">You should also have some familiarity with Azure, OAuth 2.0, and bot development.</span></span>

<span data-ttu-id="fd707-141">Cuando haya terminado, tendrá un bot que puede responder a una serie de tareas simples en una aplicación de Azure AD, como las de comprobar y enviar un correo electrónico, o bien mostrar quién es usted y quién es el administrador.</span><span class="sxs-lookup"><span data-stu-id="fd707-141">Once you finish, you will have a bot that can respond to a few simple tasks against an Azure AD application, such as checking and sending an email, or displaying who you are and who your manager is.</span></span> <span data-ttu-id="fd707-142">Para ello, el bot usará un token de una aplicación de Azure AD con la biblioteca Microsoft.Graph.</span><span class="sxs-lookup"><span data-stu-id="fd707-142">To do this, your bot will use a token from an Azure AD application against the Microsoft.Graph library.</span></span>

<span data-ttu-id="fd707-143">En la sección final se desglosa parte del código del bot.</span><span class="sxs-lookup"><span data-stu-id="fd707-143">The final section breaks down some of the bot code</span></span>

- [<span data-ttu-id="fd707-144">Notas sobre el flujo de recuperación de tokens</span><span class="sxs-lookup"><span data-stu-id="fd707-144">Notes on the token retrieval flow</span></span>](#notes-on-the-token-retrieval-flow)

## <a name="create-your-bot-and-an-authentication-application"></a><span data-ttu-id="fd707-145">Crear el bot y una aplicación de autenticación</span><span class="sxs-lookup"><span data-stu-id="fd707-145">Create your bot and an authentication application</span></span>

<span data-ttu-id="fd707-146">Deberá crear un bot de registro, en el que se publicará el código del bot, y una aplicación de Azure AD (v1 o v2) para permitir que el bot acceda a Office 365.</span><span class="sxs-lookup"><span data-stu-id="fd707-146">You need to create a registration bot to which you'll publish your bot code, and you need to create an Azure AD (either v1 or v2) application to allow your bot to access Office 365.</span></span>

> [!NOTE]
> <span data-ttu-id="fd707-147">Estas características de autenticación funcionan con otros tipos de bots.</span><span class="sxs-lookup"><span data-stu-id="fd707-147">These authentication features work with other types of bots.</span></span> <span data-ttu-id="fd707-148">Pero en este tutorial solo se usa un bot de registro.</span><span class="sxs-lookup"><span data-stu-id="fd707-148">However this tutorial uses a registration only bot.</span></span>

### <a name="register-an-application-in-azure-ad"></a><span data-ttu-id="fd707-149">Registrar una aplicación en Azure AD</span><span class="sxs-lookup"><span data-stu-id="fd707-149">Register an application in Azure AD</span></span>

<span data-ttu-id="fd707-150">Necesita una aplicación de Azure AD que el bot pueda usar para conectarse a Microsoft Graph API, sus propios recursos protegidos por AD Azure y así sucesivamente.</span><span class="sxs-lookup"><span data-stu-id="fd707-150">You need an Azure AD application that your bot can use to connect to the Microsoft Graph API, your own Azure AD-protected resources, and so on.</span></span>

<span data-ttu-id="fd707-151">Para este bot se pueden usar puntos de conexión v1 o v2 de Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fd707-151">For this bot you can use Azure AD v1 or v2 endpoints.</span></span>
<span data-ttu-id="fd707-152">Para obtener información sobre las diferencias entre los puntos de conexión v1 y v2, vea la [comparación entre v1 y v2](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-compare) y la [introducción al punto de conexión v2.0 de Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-appmodel-v2-overview).</span><span class="sxs-lookup"><span data-stu-id="fd707-152">For information about the differences between the v1 and v2 endpoints, see the [v1-v2 comparison](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-compare) and the [Azure AD v2.0 endpoint overview](https://docs.microsoft.com/azure/active-directory/develop/active-directory-appmodel-v2-overview).</span></span>

#### <a name="to-create-an-azure-ad-v1-application"></a><span data-ttu-id="fd707-153">Para crear una aplicación v1 de Azure AD</span><span class="sxs-lookup"><span data-stu-id="fd707-153">To create an Azure AD v1 application</span></span>

1. <span data-ttu-id="fd707-154">Vaya a [Azure AD en Azure Portal](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview).</span><span class="sxs-lookup"><span data-stu-id="fd707-154">Go to [Azure AD in the Azure portal](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview).</span></span>
1. <span data-ttu-id="fd707-155">Haga clic en **Registros de aplicaciones**.</span><span class="sxs-lookup"><span data-stu-id="fd707-155">Click **App registrations**.</span></span>
1. <span data-ttu-id="fd707-156">En el panel **Registros de aplicaciones**, haga clic en **Nuevo registro de aplicaciones**.</span><span class="sxs-lookup"><span data-stu-id="fd707-156">In the **App registrations** panel, click **New application registration**.</span></span>
1. <span data-ttu-id="fd707-157">Rellene los campos obligatorios y cree el registro de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="fd707-157">Fill in the required fields and create the app registration.</span></span>
   1. <span data-ttu-id="fd707-158">Asigne un nombre a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="fd707-158">Name your application.</span></span>
   1. <span data-ttu-id="fd707-159">Establezca **Tipo de aplicación** en **Aplicación web o API**.</span><span class="sxs-lookup"><span data-stu-id="fd707-159">Set the **Application type** to **Web app / API**.</span></span>
   1. <span data-ttu-id="fd707-160">Establezca **URL de inicio de sesión** en `https://token.botframework.com/.auth/web/redirect`.</span><span class="sxs-lookup"><span data-stu-id="fd707-160">Set the **Sign-on URL** to `https://token.botframework.com/.auth/web/redirect`.</span></span>
   1. <span data-ttu-id="fd707-161">Haga clic en **Create**(Crear).</span><span class="sxs-lookup"><span data-stu-id="fd707-161">Click **Create**.</span></span>
      - <span data-ttu-id="fd707-162">Una vez creada, se muestra en un panel de **Aplicación registrada**.</span><span class="sxs-lookup"><span data-stu-id="fd707-162">Once it is created, it is displayed in a **Registered app** pane.</span></span>
      - <span data-ttu-id="fd707-163">Anote el **Id. de aplicación**.</span><span class="sxs-lookup"><span data-stu-id="fd707-163">Record the **Application ID** value.</span></span> <span data-ttu-id="fd707-164">Más adelante lo proporcionará como el _Id. de cliente_.</span><span class="sxs-lookup"><span data-stu-id="fd707-164">You will provide this later as the _Client ID_.</span></span>
1. <span data-ttu-id="fd707-165">Haga clic en **Configuración** para configurar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="fd707-165">Click **Settings** to configure your application.</span></span>
1. <span data-ttu-id="fd707-166">Haga clic en **Claves** para abrir el panel **Claves**.</span><span class="sxs-lookup"><span data-stu-id="fd707-166">Click **Keys** to open the **Keys** panel.</span></span>
   1. <span data-ttu-id="fd707-167">En **Contraseñas**, cree una clave `BotLogin`.</span><span class="sxs-lookup"><span data-stu-id="fd707-167">Under **Passwords**, create a `BotLogin` key.</span></span>
   1. <span data-ttu-id="fd707-168">Establezca su **Duración** en **Nunca expira**.</span><span class="sxs-lookup"><span data-stu-id="fd707-168">Set its **Duration** to **Never expires**.</span></span>
   1. <span data-ttu-id="fd707-169">Haga clic en **Guardar** y registre el valor de clave.</span><span class="sxs-lookup"><span data-stu-id="fd707-169">Click **Save** and record the key value.</span></span> <span data-ttu-id="fd707-170">Más adelante lo proporcionará para el _secreto de aplicación_.</span><span class="sxs-lookup"><span data-stu-id="fd707-170">You provide this later for the _application secret_.</span></span>
   1. <span data-ttu-id="fd707-171">Cierre el panel **Claves**.</span><span class="sxs-lookup"><span data-stu-id="fd707-171">Close the **Keys** panel.</span></span>
1. <span data-ttu-id="fd707-172">Haga clic en **Permisos necesarios** para abrir el panel **Permisos necesarios**.</span><span class="sxs-lookup"><span data-stu-id="fd707-172">Click **Required permissions** to open the **Required permissions** panel.</span></span>
   1. <span data-ttu-id="fd707-173">Haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="fd707-173">Click **Add**.</span></span>
   1. <span data-ttu-id="fd707-174">Haga clic en **Seleccionar una API**, seleccione **Microsoft Graph** y haga clic en **Seleccionar**.</span><span class="sxs-lookup"><span data-stu-id="fd707-174">Click **Select an API**, then select **Microsoft Graph** and click **Select**.</span></span>
   1. <span data-ttu-id="fd707-175">Haga clic en **Seleccionar permisos**.</span><span class="sxs-lookup"><span data-stu-id="fd707-175">Click **Select permissions**.</span></span> <span data-ttu-id="fd707-176">Elija los permisos de aplicación que va a usar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="fd707-176">Choose the application permissions your application will use.</span></span>

      > [!NOTE]
      > <span data-ttu-id="fd707-177">Los permisos marcados como **Requiere un administrador** requerirán un usuario y un administrador de inquilinos para iniciar sesión, por lo que evite usarlos para el bot.</span><span class="sxs-lookup"><span data-stu-id="fd707-177">Any permission marked as **Requires Admin** will require both a user and a tenant admin to login, so for your bot tend to stay away from these.</span></span>

      <span data-ttu-id="fd707-178">Seleccione los permisos delegados siguientes de Microsoft Graph:</span><span class="sxs-lookup"><span data-stu-id="fd707-178">Select the following Microsoft Graph delegated permissions:</span></span>
      - <span data-ttu-id="fd707-179">Leer los perfiles básicos de todos los usuarios</span><span class="sxs-lookup"><span data-stu-id="fd707-179">Read all users' basic profiles</span></span>
      - <span data-ttu-id="fd707-180">Permite leer el correo del usuario.</span><span class="sxs-lookup"><span data-stu-id="fd707-180">Read user mail</span></span>
      - <span data-ttu-id="fd707-181">Iniciar sesión y leer el perfil de usuario</span><span class="sxs-lookup"><span data-stu-id="fd707-181">Sign in and read user profile</span></span>
      - <span data-ttu-id="fd707-182">Enviar correo como un usuario</span><span class="sxs-lookup"><span data-stu-id="fd707-182">Send mail as a user</span></span>
      - <span data-ttu-id="fd707-183">Ver el perfil básico de los usuarios</span><span class="sxs-lookup"><span data-stu-id="fd707-183">View users' basic profile</span></span>
      - <span data-ttu-id="fd707-184">Ver la dirección de correo electrónico de los usuarios</span><span class="sxs-lookup"><span data-stu-id="fd707-184">View users' email address</span></span>

   1. <span data-ttu-id="fd707-185">Haga clic en **Seleccionar** y después en **Listo**.</span><span class="sxs-lookup"><span data-stu-id="fd707-185">Click **Select**, then click **Done**.</span></span>
   1. <span data-ttu-id="fd707-186">Cierre el panel **Permisos necesarios**.</span><span class="sxs-lookup"><span data-stu-id="fd707-186">Close the **Required permissions** panel.</span></span>

<span data-ttu-id="fd707-187">Ahora tiene una aplicación v1 de Azure AD configurada.</span><span class="sxs-lookup"><span data-stu-id="fd707-187">You now have an Azure AD v1 application configured.</span></span>

#### <a name="to-create-an-azure-ad-v2-application"></a><span data-ttu-id="fd707-188">Para crear una aplicación v2 de Azure AD</span><span class="sxs-lookup"><span data-stu-id="fd707-188">To create an Azure AD v2 application</span></span>

1. <span data-ttu-id="fd707-189">Vaya al [Portal de registro de aplicaciones de Microsoft](https://apps.dev.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="fd707-189">Go to the [Microsoft Application Registration Portal](https://apps.dev.microsoft.com).</span></span>
1. <span data-ttu-id="fd707-190">Haga clic en **Agregar una aplicación**.</span><span class="sxs-lookup"><span data-stu-id="fd707-190">Click **Add an app**</span></span>
1. <span data-ttu-id="fd707-191">Asigne un nombre a la aplicación de Azure AD y haga clic en **Crear**.</span><span class="sxs-lookup"><span data-stu-id="fd707-191">Give your Azure AD app a name, and click **Create**.</span></span>

    <span data-ttu-id="fd707-192">Anote el GUID de **Id. de aplicación**.</span><span class="sxs-lookup"><span data-stu-id="fd707-192">Record the **Application Id** GUID.</span></span> <span data-ttu-id="fd707-193">Más adelante lo proporcionará como el identificador de cliente para la configuración de conexión.</span><span class="sxs-lookup"><span data-stu-id="fd707-193">You will provide this later as your client ID for your connection setting.</span></span>

1. <span data-ttu-id="fd707-194">En **Secretos de aplicación**, haga clic en **Generar nueva contraseña**.</span><span class="sxs-lookup"><span data-stu-id="fd707-194">Under **Application Secrets**, click **Generate New Password**.</span></span>

    <span data-ttu-id="fd707-195">Anote la contraseña de la ventana emergente.</span><span class="sxs-lookup"><span data-stu-id="fd707-195">Record the password from the pop-up.</span></span> <span data-ttu-id="fd707-196">Más adelante la proporcionará como el secreto de cliente para la configuración de conexión.</span><span class="sxs-lookup"><span data-stu-id="fd707-196">You will provide this later as your client Secret for your connection setting.</span></span>

1. <span data-ttu-id="fd707-197">En **Plataformas**, haga clic en **Agregar plataforma**.</span><span class="sxs-lookup"><span data-stu-id="fd707-197">Under **Platforms**, click **Add Platform**.</span></span>
1. <span data-ttu-id="fd707-198">En el menú emergente **Agregar plataforma**, haga clic en **Web**.</span><span class="sxs-lookup"><span data-stu-id="fd707-198">In the **Add Platform** pop-up, click **Web**.</span></span>
    1. <span data-ttu-id="fd707-199">Deje la opción **Permitir flujo implícito** activada.</span><span class="sxs-lookup"><span data-stu-id="fd707-199">Leave **Allow Implicit Flow** checked.</span></span>
    1. <span data-ttu-id="fd707-200">En **Direcciones URL de redireccionamiento**, escriba `https://token.botframework.com/.auth/web/redirect`.</span><span class="sxs-lookup"><span data-stu-id="fd707-200">For **Redirect URL**, enter `https://token.botframework.com/.auth/web/redirect`.</span></span>
    1. <span data-ttu-id="fd707-201">Deje **URL de cierre de sesión** en blanco.</span><span class="sxs-lookup"><span data-stu-id="fd707-201">Leave **Logout URL** blank.</span></span>
1. <span data-ttu-id="fd707-202">En **Permisos para Microsoft Graph**, puede agregar permisos delegados adicionales.</span><span class="sxs-lookup"><span data-stu-id="fd707-202">Under **Microsoft Graph Permissions**, you can add additional delegated permissions.</span></span>
    - <span data-ttu-id="fd707-203">Para este tutorial, agregue los permisos **Mail.Read**, **Mail.Send**, **openid**, **profile**, **User.Read** y **User.ReadBasic.All**.</span><span class="sxs-lookup"><span data-stu-id="fd707-203">For this tutorial, add the **Mail.Read**, **Mail.Send**, **openid**, **profile**, **User.Read**, and **User.ReadBasic.All** permissions.</span></span>
      <span data-ttu-id="fd707-204">El ámbito de la configuración de conexión debe tener **openid** y un recurso en el gráfico de Azure AD, como **Mail.Read**.</span><span class="sxs-lookup"><span data-stu-id="fd707-204">The scope of the connection setting needs to have both **openid** and a resource in the Azure AD graph, such as **Mail.Read**.</span></span>
    - <span data-ttu-id="fd707-205">Anote los permisos que elija.</span><span class="sxs-lookup"><span data-stu-id="fd707-205">Record the permissions you choose.</span></span> <span data-ttu-id="fd707-206">Más adelante los proporcionará como los ámbitos para la configuración de conexión.</span><span class="sxs-lookup"><span data-stu-id="fd707-206">You will provide this later as the scopes for your connection setting.</span></span>

1. <span data-ttu-id="fd707-207">Haga clic en **Guardar** en la parte inferior de la página.</span><span class="sxs-lookup"><span data-stu-id="fd707-207">Click **Save** at the bottom of the page.</span></span>

### <a name="create-your-bot-on-azure"></a><span data-ttu-id="fd707-208">Creación de un bot en Azure</span><span class="sxs-lookup"><span data-stu-id="fd707-208">Create your bot on Azure</span></span>

<span data-ttu-id="fd707-209">Cree un **Registro de canales de bot** mediante [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="fd707-209">Create a **Bot Channels Registration** using the [Azure Portal](https://portal.azure.com/).</span></span>

### <a name="register-your-azure-ad-application-with-your-bot"></a><span data-ttu-id="fd707-210">Registro de la aplicación de Azure AD con el bot</span><span class="sxs-lookup"><span data-stu-id="fd707-210">Register your Azure AD application with your bot</span></span>

<span data-ttu-id="fd707-211">El paso siguiente consiste en registrar la aplicación de Azure AD que acaba de crear con el bot.</span><span class="sxs-lookup"><span data-stu-id="fd707-211">The next step is to register with your bot the Azure AD application that you just created.</span></span>

#### <a name="to-register-an-azure-ad-v1-application"></a><span data-ttu-id="fd707-212">Para registrar una aplicación v1 de Azure AD</span><span class="sxs-lookup"><span data-stu-id="fd707-212">To register an Azure AD v1 application</span></span>

1. <span data-ttu-id="fd707-213">Vaya hasta la página de recursos del bot en [Azure Portal](http://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="fd707-213">Navigate to your bot's resource page on the [Azure Portal](http://portal.azure.com/).</span></span>
1. <span data-ttu-id="fd707-214">Haga clic en **Configuración**.</span><span class="sxs-lookup"><span data-stu-id="fd707-214">Click **Settings**.</span></span>
1. <span data-ttu-id="fd707-215">En **Configuración de conexión de OAuth**, cerca de la parte inferior de la página, haga clic en **Agregar configuración**.</span><span class="sxs-lookup"><span data-stu-id="fd707-215">Under **OAuth Connection Settings** near the bottom of the page, click **Add Setting**.</span></span>
1. <span data-ttu-id="fd707-216">Rellene el formulario de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="fd707-216">Fill in the form as follows:</span></span>
    1. <span data-ttu-id="fd707-217">Para **Nombre**, escriba un nombre para la conexión.</span><span class="sxs-lookup"><span data-stu-id="fd707-217">For **Name**, enter a name for your connection.</span></span> <span data-ttu-id="fd707-218">Se usará en el código del bot.</span><span class="sxs-lookup"><span data-stu-id="fd707-218">You'll use in your bot code.</span></span>
    1. <span data-ttu-id="fd707-219">En **Proveedor de servicios**, seleccione **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="fd707-219">For **Service Provider**, select **Azure Active Directory**.</span></span> <span data-ttu-id="fd707-220">Después de seleccionar esta opción, se mostrarán los campos específicos de Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fd707-220">Once you select this, the Azure AD-specific fields will be displayed.</span></span>
    1. <span data-ttu-id="fd707-221">Para **Id. de cliente**, escriba el identificador de aplicación que registró para la aplicación v1 de Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fd707-221">For **Client id**, enter the application ID that you recorded for your Azure AD v1 application.</span></span>
    1. <span data-ttu-id="fd707-222">Para **Secreto de cliente**, escriba la clave que registró para la clave `BotLogin` de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="fd707-222">For **Client secret**, enter the key that your recorded for your application's `BotLogin` key.</span></span>
    1. <span data-ttu-id="fd707-223">En **Tipo de concesión**, escriba `authorization_code`.</span><span class="sxs-lookup"><span data-stu-id="fd707-223">For **Grant Type**, enter `authorization_code`.</span></span>
    1. <span data-ttu-id="fd707-224">Para **Dirección URL de inicio de sesión**, escriba `https://login.microsoftonline.com`.</span><span class="sxs-lookup"><span data-stu-id="fd707-224">For **Login URL**, enter `https://login.microsoftonline.com`.</span></span>
    1. <span data-ttu-id="fd707-225">Para **Id. de inquilino**, escriba el identificador de inquilino de Azure Active Directory, por ejemplo `microsoft.com` o `common`.</span><span class="sxs-lookup"><span data-stu-id="fd707-225">For **Tenant ID**, enter the tenant ID for your Azure Active Directory, for example `microsoft.com` or `common`.</span></span>

       <span data-ttu-id="fd707-226">Este será el inquilino asociado a los usuarios que se pueden autenticar.</span><span class="sxs-lookup"><span data-stu-id="fd707-226">This will be the tenant associated with the users who can be authenticated.</span></span> <span data-ttu-id="fd707-227">Para permitir que cualquier usuario se autentique a sí mismo mediante el bot, use el inquilino `common`.</span><span class="sxs-lookup"><span data-stu-id="fd707-227">To allow anyone to authenticate themselves via the bot, use the `common` tenant.</span></span>

    1. <span data-ttu-id="fd707-228">En **URL de recursos**, escriba `https://graph.microsoft.com/`.</span><span class="sxs-lookup"><span data-stu-id="fd707-228">For **Resource URL**, enter `https://graph.microsoft.com/`.</span></span>
    1. <span data-ttu-id="fd707-229">Deje **Ámbitos** en blanco.</span><span class="sxs-lookup"><span data-stu-id="fd707-229">Leave **Scopes** blank.</span></span>
1. <span data-ttu-id="fd707-230">Haga clic en **Save**(Guardar).</span><span class="sxs-lookup"><span data-stu-id="fd707-230">Click **Save**.</span></span>

> [!NOTE]
> <span data-ttu-id="fd707-231">Estos valores permiten que la aplicación acceda a datos de Office 365 a través de Microsoft Graph API.</span><span class="sxs-lookup"><span data-stu-id="fd707-231">These values enable your application to access Office 365 data via the Microsoft Graph API.</span></span>

<span data-ttu-id="fd707-232">Ahora puede usar este nombre de conexión en el código del bot para recuperar los tokens de usuario.</span><span class="sxs-lookup"><span data-stu-id="fd707-232">You can now use this connection name in your bot code to retrieve user tokens.</span></span>

#### <a name="to-register-an-azure-ad-v2-application"></a><span data-ttu-id="fd707-233">Para registrar una aplicación v2 de Azure AD</span><span class="sxs-lookup"><span data-stu-id="fd707-233">To register an Azure AD v2 application</span></span>

1. <span data-ttu-id="fd707-234">Vaya a la página de registro de canales del bot en [Azure Portal](http://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="fd707-234">Navigate to your bot's Bot Channels Registration page on the [Azure Portal](http://portal.azure.com/).</span></span>
1. <span data-ttu-id="fd707-235">Haga clic en **Configuración**.</span><span class="sxs-lookup"><span data-stu-id="fd707-235">Click **Settings**.</span></span>
1. <span data-ttu-id="fd707-236">En **Configuración de conexión de OAuth**, cerca de la parte inferior de la página, haga clic en **Agregar configuración**.</span><span class="sxs-lookup"><span data-stu-id="fd707-236">Under **OAuth Connection Settings** near the bottom of the page, click **Add Setting**.</span></span>
1. <span data-ttu-id="fd707-237">Rellene el formulario de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="fd707-237">Fill in the form as follows:</span></span>
    1. <span data-ttu-id="fd707-238">Para **Nombre**, escriba un nombre para la conexión.</span><span class="sxs-lookup"><span data-stu-id="fd707-238">For **Name**, enter a name for your connection.</span></span> <span data-ttu-id="fd707-239">Lo usará en el código del bot.</span><span class="sxs-lookup"><span data-stu-id="fd707-239">You'll use it in your bot code.</span></span>
    1. <span data-ttu-id="fd707-240">En **Proveedor de servicios**, seleccione **Azure Active Directory v2**.</span><span class="sxs-lookup"><span data-stu-id="fd707-240">For **Service Provider**, select **Azure Active Directory v2**.</span></span> <span data-ttu-id="fd707-241">Después de seleccionar esta opción, se mostrarán los campos específicos de Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fd707-241">Once you select this, the Azure AD-specific fields will be displayed.</span></span>
    1. <span data-ttu-id="fd707-242">Para **Id. de cliente**, escriba el identificador de aplicación v2 de Azure AD del registro de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="fd707-242">For **Client id**, enter your Azure AD v2 application ID from application registration.</span></span>
    1. <span data-ttu-id="fd707-243">Para **Secreto de cliente**, escriba la contraseña de aplicación v2 de Azure AD del registro de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="fd707-243">For **Client secret**, enter your Azure AD v2 application password from application registration.</span></span>
    1. <span data-ttu-id="fd707-244">Para **Id. de inquilino**, escriba el identificador de inquilino de Azure Active Directory, por ejemplo `microsoft.com` o `common`.</span><span class="sxs-lookup"><span data-stu-id="fd707-244">For **Tenant ID**, enter the tenant ID for your Azure Active Directory, for example `microsoft.com` or `common`.</span></span>

        <span data-ttu-id="fd707-245">Este será el inquilino asociado a los usuarios que se pueden autenticar.</span><span class="sxs-lookup"><span data-stu-id="fd707-245">This will be the tenant associated with the users who can be authenticated.</span></span> <span data-ttu-id="fd707-246">Para permitir que cualquier usuario se autentique a sí mismo mediante el bot, use el inquilino `common`.</span><span class="sxs-lookup"><span data-stu-id="fd707-246">To allow anyone to authenticate themselves via the bot, use the `common` tenant.</span></span>

    1. <span data-ttu-id="fd707-247">En **Ámbitos**, escriba los nombres de los permisos que eligió en el registro de la aplicación: `Mail.Read Mail.Send openid profile User.Read User.ReadBasic.All`.</span><span class="sxs-lookup"><span data-stu-id="fd707-247">For **Scopes**, enter the names of the permission you chose from application registration: `Mail.Read Mail.Send openid profile User.Read User.ReadBasic.All`.</span></span>

        > [!NOTE]
        > <span data-ttu-id="fd707-248">Para Azure AD v2, **Ámbitos** toma una lista de valores que distingue mayúsculas de minúsculas, separados por espacios.</span><span class="sxs-lookup"><span data-stu-id="fd707-248">For Azure AD v2, **Scopes** takes a case-sensitive, space-separated list of values.</span></span>

1. <span data-ttu-id="fd707-249">Haga clic en **Save**(Guardar).</span><span class="sxs-lookup"><span data-stu-id="fd707-249">Click **Save**.</span></span>

> [!NOTE]
> <span data-ttu-id="fd707-250">Estos valores permiten que la aplicación acceda a datos de Office 365 a través de Microsoft Graph API.</span><span class="sxs-lookup"><span data-stu-id="fd707-250">These values enable your application to access Office 365 data via the Microsoft Graph API.</span></span>

<span data-ttu-id="fd707-251">Ahora puede usar este nombre de conexión en el código del bot para recuperar los tokens de usuario.</span><span class="sxs-lookup"><span data-stu-id="fd707-251">You can now use this connection name in your bot code to retrieve user tokens.</span></span>

#### <a name="to-test-your-connection"></a><span data-ttu-id="fd707-252">Para probar la conexión</span><span class="sxs-lookup"><span data-stu-id="fd707-252">To test your connection</span></span>

1. <span data-ttu-id="fd707-253">Abra la conexión que acaba de crear.</span><span class="sxs-lookup"><span data-stu-id="fd707-253">Open the connection you just created.</span></span>
1. <span data-ttu-id="fd707-254">Haga clic en **Probar conexión** en la parte superior del panel **Configuración de conexión del proveedor de servicios**.</span><span class="sxs-lookup"><span data-stu-id="fd707-254">Click **Test Connection** at the top of the **Service Provider Connection Setting** pane.</span></span>
1. <span data-ttu-id="fd707-255">La primera vez, se debería abrir una pestaña del explorador nueva con los permisos que solicita la aplicación y en la que se le pide que acepte.</span><span class="sxs-lookup"><span data-stu-id="fd707-255">The first time, this should open a new browser tab listing the permissions your app is requesting and prompt you to accept.</span></span>
1. <span data-ttu-id="fd707-256">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="fd707-256">Click **Accept**.</span></span>
1. <span data-ttu-id="fd707-257">Después, esto debería redirigirle a una página **Prueba de conexión a "<nombreDeLaConexión>" correcta**.</span><span class="sxs-lookup"><span data-stu-id="fd707-257">This should then redirect you to a **Test Connection to \`<your-connection-name>' Succeeded** page.</span></span>

## <a name="prepare-the-bot-sample-code"></a><span data-ttu-id="fd707-258">Preparar el código de ejemplo del bot</span><span class="sxs-lookup"><span data-stu-id="fd707-258">Prepare the bot sample code</span></span>

1. <span data-ttu-id="fd707-259">Clone el repositorio de GitHub en https://github.com/Microsoft/BotBuilder.</span><span class="sxs-lookup"><span data-stu-id="fd707-259">Clone the github repository at https://github.com/Microsoft/BotBuilder.</span></span>
1. <span data-ttu-id="fd707-260">Abra y compile la solución, `BotBuilder\CSharp\Microsoft.Bot.Builder.sln`.</span><span class="sxs-lookup"><span data-stu-id="fd707-260">Open and build the solution, `BotBuilder\CSharp\Microsoft.Bot.Builder.sln`.</span></span>
1. <span data-ttu-id="fd707-261">Cierre esa solución y abra `BotBuilder\CSharp\Samples\Microsoft.Bot.Builder.Samples.sln`.</span><span class="sxs-lookup"><span data-stu-id="fd707-261">Close that solution and open, `BotBuilder\CSharp\Samples\Microsoft.Bot.Builder.Samples.sln`.</span></span>
1. <span data-ttu-id="fd707-262">Establezca el proyecto de inicio.</span><span class="sxs-lookup"><span data-stu-id="fd707-262">Set the start up project.</span></span>
    - <span data-ttu-id="fd707-263">Para un bot en el que se use la aplicación v1 de Azure AD, use el proyecto `Microsoft.Bot.Sample.AadV1Bot`.</span><span class="sxs-lookup"><span data-stu-id="fd707-263">For a bot that uses the v1 Azure AD application, use the `Microsoft.Bot.Sample.AadV1Bot` project.</span></span>
    - <span data-ttu-id="fd707-264">Para un bot en el que se use la aplicación v2 de Azure AD, use el proyecto `Microsoft.Bot.Sample.AadV2Bot`.</span><span class="sxs-lookup"><span data-stu-id="fd707-264">For a bot that uses the v2 Azure AD application, use the `Microsoft.Bot.Sample.AadV2Bot` project.</span></span>
1. <span data-ttu-id="fd707-265">Abra el archivo `Web.config` y modifique la configuración de la aplicación como sigue:</span><span class="sxs-lookup"><span data-stu-id="fd707-265">Open the `Web.config` file, and modify the app settings as follows:</span></span>
    1. <span data-ttu-id="fd707-266">Establezca `ConnectionName` en el valor que usó al configurar la configuración de conexión de OAuth 2.0 del bot.</span><span class="sxs-lookup"><span data-stu-id="fd707-266">Set the `ConnectionName` to the value you used when you configured your bot's OAuth 2.0 connection setting.</span></span>
    1. <span data-ttu-id="fd707-267">Establezca el valor `MicrosoftAppId` en el identificador de aplicación del bot.</span><span class="sxs-lookup"><span data-stu-id="fd707-267">Set the `MicrosoftAppId` value to your bot's app ID.</span></span>
    1. <span data-ttu-id="fd707-268">Establezca el valor `MicosoftAppPassword` en el secreto del bot.</span><span class="sxs-lookup"><span data-stu-id="fd707-268">Set the `MicosoftAppPassword` value to your bot's secret.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="fd707-269">En función de los caracteres del secreto, es posible que sea necesario aplicar secuencias de escape XML a la contraseña.</span><span class="sxs-lookup"><span data-stu-id="fd707-269">Depending on the characters in your secret, you may need to XML escape the password.</span></span> <span data-ttu-id="fd707-270">Por ejemplo, cualquier símbolo de Y comercial (&) tendrá que codificarse como `&amp;`.</span><span class="sxs-lookup"><span data-stu-id="fd707-270">For example, any ampersands (&) will need to be encoded as `&amp;`.</span></span>

    ```xml
    <appSettings>
        <add key="ConnectionName" value="<your-AAD-connection-name>"/>
        <add key="MicrosoftAppId" value="<your-bot-appId>" />
        <add key="MicrosoftAppPassword" value="<your-bot-password>" />
    </appSettings>
    ```

    <span data-ttu-id="fd707-271">Si no sabe cómo obtener los valores **Id. de aplicación de Microsoft** y **Contraseña de aplicación de Microsoft**, busque en **Configuración de la aplicación** del Azure App Service que se haya aprovisionado para el bot en Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="fd707-271">If you do not know how to get your **Microsoft app ID** and **Microsoft app password** values, look in the **ApplicationSettings** of the Azure app service that was provisioned for your bot on the Azure Portal.</span></span>

    > [!NOTE]
    > <span data-ttu-id="fd707-272">Ahora podría publicar el código de este bot en la suscripción de Azure (si hace clic con el botón derecho en el proyecto y selecciona **Publicar**), pero para este tutorial no es necesario.</span><span class="sxs-lookup"><span data-stu-id="fd707-272">You could now publish this bot code back to your Azure subscription (right-click on the project and choose **Publish**), but it is not necessary for this tutorial.</span></span> <span data-ttu-id="fd707-273">Tendría que establecer una configuración de publicación que usara la aplicación y el plan de hospedaje que usó durante la configuración del bot en Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="fd707-273">You would need to set up a publishing configuration that uses the application and hosting plan that you used when configuration the bot in the Azure Portal.</span></span>

## <a name="use-the-emulator-to-test-your-bot"></a><span data-ttu-id="fd707-274">Usar el emulador para probar el bot</span><span class="sxs-lookup"><span data-stu-id="fd707-274">Use the Emulator to test your bot</span></span>

<span data-ttu-id="fd707-275">Deberá instalar el [Emulador de bot](https://github.com/Microsoft/BotFramework-Emulator) para probar el bot de forma local.</span><span class="sxs-lookup"><span data-stu-id="fd707-275">You will need to install the [Bot Emulator](https://github.com/Microsoft/BotFramework-Emulator) to test your bot locally.</span></span> <span data-ttu-id="fd707-276">Puede usar el emulador v3 o v4.</span><span class="sxs-lookup"><span data-stu-id="fd707-276">You can use the v3 or v4 Emulator.</span></span>

1. <span data-ttu-id="fd707-277">Inicie el bot (con o sin depuración).</span><span class="sxs-lookup"><span data-stu-id="fd707-277">Start your bot (with or without debugging).</span></span>
1. <span data-ttu-id="fd707-278">Anote el número de puerto de host local de la página.</span><span class="sxs-lookup"><span data-stu-id="fd707-278">Note the localhost port number for the page.</span></span> <span data-ttu-id="fd707-279">Necesitará esta información para interactuar con el bot.</span><span class="sxs-lookup"><span data-stu-id="fd707-279">You will need this information to interact with your bot.</span></span>
1. <span data-ttu-id="fd707-280">Inicie el emulador.</span><span class="sxs-lookup"><span data-stu-id="fd707-280">Start the Emulator.</span></span>
1. <span data-ttu-id="fd707-281">Conéctese al bot.</span><span class="sxs-lookup"><span data-stu-id="fd707-281">Connect to your bot.</span></span>

   <span data-ttu-id="fd707-282">Si todavía no ha configurado la conexión, proporcione la dirección y el id. de aplicación y la contraseña de Microsoft del bot.</span><span class="sxs-lookup"><span data-stu-id="fd707-282">If you haven't configured the connection already, provide the address and your bot's Microsoft app ID and password.</span></span> <span data-ttu-id="fd707-283">Agregue `/api/messages` a la dirección URL del bot.</span><span class="sxs-lookup"><span data-stu-id="fd707-283">Add `/api/messages` to the bot's URL.</span></span> <span data-ttu-id="fd707-284">La dirección URL se parecerá a esta `http://localhost:portNumber/api/messages`.</span><span class="sxs-lookup"><span data-stu-id="fd707-284">Your URL will look something like `http://localhost:portNumber/api/messages`.</span></span>

1. <span data-ttu-id="fd707-285">Escriba `help` para ver una lista de los comandos disponibles para el bot y probar las características de autenticación.</span><span class="sxs-lookup"><span data-stu-id="fd707-285">Type `help` to see a list of available commands for the bot, and test the authentication features.</span></span>
1. <span data-ttu-id="fd707-286">Una vez que haya iniciado sesión, no es necesario que vuelva a proporcionar las credenciales hasta que cierre la sesión.</span><span class="sxs-lookup"><span data-stu-id="fd707-286">Once you've signed in, you don't need to provide your credentials again until you sign out.</span></span>
1. <span data-ttu-id="fd707-287">Para cerrar la sesión y cancelar la autenticación, escriba `signout`.</span><span class="sxs-lookup"><span data-stu-id="fd707-287">To sign out, and cancel your authentication, type `signout`.</span></span>

<!--To restart completely from scratch you also need to:
1. Navigate to the **AppData** folder for your account.
1. Go to the **Roaming/botframework-emulator** subfolder.
1. Delete the **Cookies** and **Coolies-journal** files.
-->

> [!NOTE]
> <span data-ttu-id="fd707-288">La autenticación del bot requiere el uso de Bot Connector Service.</span><span class="sxs-lookup"><span data-stu-id="fd707-288">Bot authentication requires use of the Bot Connector Service.</span></span> <span data-ttu-id="fd707-289">El servicio accede a la información de registro de los canales del bot, motivo por el que es necesario establecer el punto de conexión de mensajería del bot en el portal.</span><span class="sxs-lookup"><span data-stu-id="fd707-289">The service accesses the bot channels registration information for your bot, which is why you need to set your bot's messaging endpoint on the portal.</span></span> <span data-ttu-id="fd707-290">La autenticación también requiere el uso de HTTPS, por lo que fue necesario crear una dirección de reenvío HTTPS para el bot que se ejecuta localmente.</span><span class="sxs-lookup"><span data-stu-id="fd707-290">Authentication also requires the use of HTTPS, which is why you needed to create an HTTPS forwarding address for your bot running locally.</span></span>

<!--The following is necessary for WebChat:
1. Use the **ngrok** command-line tool to get a forwarding HTTPS address for your bot.
   - For information on how to do this, see [Debug any Channel locally using ngrok](https://blog.botframework.com/2017/10/19/debug-channel-locally-using-ngrok/).
   - Any time you exit **ngrok**, you will need to redo this and the following step before starting the Emulator.
1. On the Azure Portal, go to the **Settings** blade for your bot.
   1. In the **Configuration** section, change the **Messaging endpoint** to the HTTPS forwarding address generated by **ngrok**.
   1. Click **Save** to save your change.
-->

## <a name="notes-on-the-token-retrieval-flow"></a><span data-ttu-id="fd707-291">Notas sobre el flujo de recuperación de tokens</span><span class="sxs-lookup"><span data-stu-id="fd707-291">Notes on the token retrieval flow</span></span>

<span data-ttu-id="fd707-292">Cuando un usuario le pide al bot que haga algo para lo que es necesario que el usuario haya iniciado sesión, el bot puede usar `Microsoft.Bot.Builder.Dialogs.GetTokenDialog` para iniciar la recuperación de un token para una conexión determinada.</span><span class="sxs-lookup"><span data-stu-id="fd707-292">When a user asks the bot to do something that requires the bot to have the user logged in, the bot can use the `Microsoft.Bot.Builder.Dialogs.GetTokenDialog` to initiate retrieving a token for a given connection.</span></span> <span data-ttu-id="fd707-293">Los dos fragmentos de código siguientes se toman de la clase `GetTokenDialog`.</span><span class="sxs-lookup"><span data-stu-id="fd707-293">The next couple of snippets are taken from the `GetTokenDialog` class.</span></span>

### <a name="check-for-a-cached-token"></a><span data-ttu-id="fd707-294">Buscar un token en caché</span><span class="sxs-lookup"><span data-stu-id="fd707-294">Check for a cached token</span></span>

<span data-ttu-id="fd707-295">En este código, en primer lugar el bot realiza una comprobación rápida para determinar si Azure Bot Service ya tiene un token para el usuario (que se identifica mediante el remitente de la actividad actual) y el nombre de la conexión dado (que es el nombre de conexión que se usa en la configuración).</span><span class="sxs-lookup"><span data-stu-id="fd707-295">In this code, first the bot does a quick check to determine if the Azure Bot Service already has a token for the user (which is identified by the current Activity sender) and the given ConnectionName (which is the connection name used in configuration).</span></span> <span data-ttu-id="fd707-296">Azure Bot Service tendrá ya un token en caché o no.</span><span class="sxs-lookup"><span data-stu-id="fd707-296">Azure Bot Service will either already have a token cached or it will not.</span></span> <span data-ttu-id="fd707-297">La llamada a GetUserTokenAsync realiza esta "comprobación rápida".</span><span class="sxs-lookup"><span data-stu-id="fd707-297">The call to GetUserTokenAsync performs this ‘quick check'.</span></span> <span data-ttu-id="fd707-298">Si Azure Bot Service tiene un token y lo devuelve, el token se puede usar inmediatamente.</span><span class="sxs-lookup"><span data-stu-id="fd707-298">If Azure Bot Service has a token and returns it, the token can immediately be used.</span></span> <span data-ttu-id="fd707-299">Si Azure Bot Service no tiene un token, este método devolverá NULL.</span><span class="sxs-lookup"><span data-stu-id="fd707-299">If Azure Bot Service does not have a token, this method will return null.</span></span> <span data-ttu-id="fd707-300">En este caso, el bot puede enviar una OAuthCard personalizada para que el usuario inicie sesión.</span><span class="sxs-lookup"><span data-stu-id="fd707-300">In this case, the bot can send a customized OAuthCard for the user to login.</span></span>

```csharp
// First ask Bot Service if it already has a token for this user
var token = await context.GetUserTokenAsync(ConnectionName).ConfigureAwait(false);
if (token != null)
{
    // use the token to do exciting things!
}
else
{
    // If Bot Service does not have a token, send an OAuth card to sign in
    await SendOAuthCardAsync(context, (Activity)context.Activity);
}
```

### <a name="send-an-oauthcard-to-the-user"></a><span data-ttu-id="fd707-301">Envío de OAuthCard al usuario</span><span class="sxs-lookup"><span data-stu-id="fd707-301">Send an OAuthCard to the user</span></span>

<span data-ttu-id="fd707-302">Puede personalizar la OAuthCard con el texto y el texto de botón que quiera.</span><span class="sxs-lookup"><span data-stu-id="fd707-302">You can customize the OAuthCard with whatever text and button text you want.</span></span> <span data-ttu-id="fd707-303">Las partes importantes son las siguientes:</span><span class="sxs-lookup"><span data-stu-id="fd707-303">The important pieces are:</span></span>

- <span data-ttu-id="fd707-304">Establecer `ContentType` en `OAuthCard.ContentType`.</span><span class="sxs-lookup"><span data-stu-id="fd707-304">Set the `ContentType` to `OAuthCard.ContentType`.</span></span>
- <span data-ttu-id="fd707-305">Establecer la propiedad `ConnectionName` en el nombre de la conexión que se quiere usar.</span><span class="sxs-lookup"><span data-stu-id="fd707-305">Set the `ConnectionName` property to the name of the connection you want to use.</span></span>
- <span data-ttu-id="fd707-306">Incluir un botón con un elemento `CardAction` de `Type` `ActionTypes.Signin`; tenga en cuenta que no es necesario especificar ningún valor para el vínculo de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="fd707-306">Include one button with a `CardAction` of `Type` `ActionTypes.Signin`; note that you do not need to specify any value for the sign in link.</span></span>

<span data-ttu-id="fd707-307">Al final de esta llamada, el bot debe "esperar a que el token" regrese.</span><span class="sxs-lookup"><span data-stu-id="fd707-307">At the end of this call, the bot needs to "wait for the token" to come back.</span></span> <span data-ttu-id="fd707-308">Esta espera tiene lugar en la secuencia de actividad principal porque es posible que el usuario tenga que hacer muchas cosas para iniciar sesión.</span><span class="sxs-lookup"><span data-stu-id="fd707-308">This waiting takes place on the main Activity stream because there could be a lot the user needs to do to sign-in.</span></span>

```csharp
private async Task SendOAuthCardAsync(IDialogContext context, Activity activity)
{
    await context.PostAsync($"To do this, you'll first need to sign in.");

    var reply = await context.Activity.CreateOAuthReplyAsync(_connectionName, _signInMessage, _buttonLabel).ConfigureAwait(false);
    await context.PostAsync(reply);

    context.Wait(WaitForToken);
}
```

### <a name="wait-for-a-tokenresponseevent"></a><span data-ttu-id="fd707-309">Esperar a TokenResponseEvent</span><span class="sxs-lookup"><span data-stu-id="fd707-309">Wait for a TokenResponseEvent</span></span>

<span data-ttu-id="fd707-310">En este código, la clase de cuadro de diálogo del bot está esperando un `TokenResponseEvent` (a continuación se muestra más información sobre cómo se enruta esto a la pila del cuadro de diálogo).</span><span class="sxs-lookup"><span data-stu-id="fd707-310">In this code the Bot's dialog class is waiting for a `TokenResponseEvent` (more about how this is routed to the Dialog stack is below).</span></span> <span data-ttu-id="fd707-311">Primero, el método `WaitForToken` determina si este evento se ha enviado.</span><span class="sxs-lookup"><span data-stu-id="fd707-311">The `WaitForToken` method first determines if this event was sent.</span></span> <span data-ttu-id="fd707-312">Si se ha enviado, el bot lo puede usar.</span><span class="sxs-lookup"><span data-stu-id="fd707-312">If it was sent, it can be used by the bot.</span></span> <span data-ttu-id="fd707-313">En caso contrario, el método `WaitForToken` toma el texto que se haya enviado al bot y lo pasa a `GetUserTokenAsync`.</span><span class="sxs-lookup"><span data-stu-id="fd707-313">If it was not, the `WaitForToken` method takes whatever text was sent to the bot and passes it to `GetUserTokenAsync`.</span></span> <span data-ttu-id="fd707-314">El motivo es que algunos clientes (como WebChat) no necesitan el código de comprobación y pueden enviar el token directamente en `TokenResponseEvent`.</span><span class="sxs-lookup"><span data-stu-id="fd707-314">The reason for this is that some clients (like WebChat) do not need the Magic Code verification code and can directly send the Token in the `TokenResponseEvent`.</span></span> <span data-ttu-id="fd707-315">Otros clientes requerirán el código mágico (como Facebook o Slack).</span><span class="sxs-lookup"><span data-stu-id="fd707-315">Other clients still require the magic code (like Facebook or Slack).</span></span> <span data-ttu-id="fd707-316">Azure Bot Service presentará a estos clientes un código mágico de seis dígitos y pedirá al usuario que lo escriba en la ventana de chat.</span><span class="sxs-lookup"><span data-stu-id="fd707-316">The Azure Bot Service will present these clients with a six digit magic code and ask the user to type this into the chat window.</span></span> <span data-ttu-id="fd707-317">Aunque no es lo idóneo, es el comportamiento "de reserva", de modo que si `WaitForToke`n recibe un código, el bot lo puede enviar a Azure Bot Service y obtener un token.</span><span class="sxs-lookup"><span data-stu-id="fd707-317">While not ideal, this is the 'fall back' behavior and so if `WaitForToke`n receives a code, the bot can send this code to the Azure Bot Service and get a token back.</span></span> <span data-ttu-id="fd707-318">Si también se produce un error en esta llamada, puede decidir si notificar un error, o bien hacer otra cosa.</span><span class="sxs-lookup"><span data-stu-id="fd707-318">If this call also fails, then you can decide to report an error, or do something else.</span></span> <span data-ttu-id="fd707-319">Pero en la mayoría de los casos, el bot tendrá un token de usuario.</span><span class="sxs-lookup"><span data-stu-id="fd707-319">In most cases though, the bot will now have a user token.</span></span>

<span data-ttu-id="fd707-320">Si examina el archivo **MessageController.cs**, verá que las actividades `Event` de este tipo también se enrutan a la pila del cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="fd707-320">If you look in the **MessageController.cs** file, you'll see that `Event` activities of this type are also routed to the dialog stack.</span></span>

```csharp
private async Task WaitForToken(IDialogContext context, IAwaitable<object> result)
{
    var activity = await result as Activity;

    var tokenResponse = activity.ReadTokenResponseContent();
    if (tokenResponse != null)
    {
        // Use the token to do exciting things!
    }
    else
    {
        if (!string.IsNullOrEmpty(activity.Text))
        {
            tokenResponse = await context.GetUserTokenAsync(ConnectionName,
                                                               activity.Text);
            if (tokenResponse != null)
            {
                // Use the token to do exciting things!
                return;
            }
        }
        await context.PostAsync($"Hmm. Something went wrong. Let's try again.");
        await SendOAuthCardAsync(context, activity);
    }
}
```

### <a name="message-controller"></a><span data-ttu-id="fd707-321">Controlador de mensajes</span><span class="sxs-lookup"><span data-stu-id="fd707-321">Message controller</span></span>

<span data-ttu-id="fd707-322">En las llamadas posteriores al bot, tenga en cuenta que este bot de ejemplo nunca almacena el token en caché.</span><span class="sxs-lookup"><span data-stu-id="fd707-322">On subsequent calls to the bot, notice that the token is never cached by this sample bot.</span></span> <span data-ttu-id="fd707-323">Se debe a que el bot siempre puede pedir el token a Azure Bot Service.</span><span class="sxs-lookup"><span data-stu-id="fd707-323">This is because the bot can always ask the Azure Bot Service for the token.</span></span> <span data-ttu-id="fd707-324">Esto evita que el bot tenga que administrar el ciclo de vida del token, actualizar el token, etc., ya Azure Bot Service se encarga de todo esto de forma automática.</span><span class="sxs-lookup"><span data-stu-id="fd707-324">This avoids the bot needing to manage the token life-cycle, refresh the token, etc, as Azure Bot Service does all of this for you.</span></span>

```csharp
else if(message.Type == ActivityTypes.Event)
{
    if(message.IsTokenResponseEvent())
    {
        await Conversation.SendAsync(message, () => new Dialogs.RootDialog());
    }
}
```
