---
title: Solución de problemas de autenticación de Bot Framework | Microsoft Docs
description: Aprenda a solucionar errores de autenticación con un bot.
author: DeniseMak
ms.author: v-demak
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/17
ms.openlocfilehash: a64edda73832f4d3fff49b08b5eaf6792c021ece
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305829"
---
# <a name="troubleshooting-bot-framework-authentication"></a><span data-ttu-id="bc5ef-103">Solución de problemas de autenticación de Bot Framework</span><span class="sxs-lookup"><span data-stu-id="bc5ef-103">Troubleshooting Bot Framework authentication</span></span>

<span data-ttu-id="bc5ef-104">Esta guía puede ayudarle a solucionar problemas de autenticación con el bot evaluando una serie de escenarios para determinar dónde se produce el problema.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-104">This guide can help you to troubleshoot authentication issues with your bot by evaluating a series of scenarios to determine where the problem exists.</span></span> 

> [!NOTE]
> <span data-ttu-id="bc5ef-105">Para completar todos los pasos descritos en esta guía, debe descargar y usar [Bot Framework Emulator][ Emulator] y debe tener acceso a la configuración de registro del bot en el <a href="https://dev.botframework.com" target="_blank">portal de Bot Framework</a>.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-105">To complete all steps in this guide, you will need to download and use the [Bot Framework Emulator][Emulator] and must have access to the bot's registration settings in the <a href="https://dev.botframework.com" target="_blank">Bot Framework Portal</a>.</span></span>

## <a id="PW"></a><span data-ttu-id="bc5ef-106">Identificador de aplicación y contraseña</span><span class="sxs-lookup"><span data-stu-id="bc5ef-106">App ID and password</span></span>

<span data-ttu-id="bc5ef-107">La seguridad del bot se configura con el **identificador de aplicación de Microsoft** y la **contraseña de aplicación de Microsoft** que se obtiene al registrar el bot con Bot Framework.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-107">Bot security is configured by the **Microsoft App ID** and **Microsoft App Password** that you obtain when you register your bot with the Bot Framework.</span></span> <span data-ttu-id="bc5ef-108">Normalmente, estos valores son especificados en el archivo de configuración del bot y se usan para recuperar los tokens de acceso del servicio Cuenta Microsoft.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-108">These values are typically specified within the bot's configuration file and used to retrieve access tokens from the Microsoft Account service.</span></span> 

<span data-ttu-id="bc5ef-109">Si aún no lo ha hecho, [registre el bot](~/bot-service-quickstart-registration.md) para obtener un **identificador de aplicación de Microsoft** y una **contraseña de aplicación de Microsoft** que pueda usar para la autenticación.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-109">If you have not yet done so, [register your bot](~/bot-service-quickstart-registration.md) to obtain a **Microsoft App ID** and **Microsoft App Password** that it can use for authentication.</span></span> 

> [!NOTE]
> <span data-ttu-id="bc5ef-110">Para buscar los valores **AppID** y **AppPassword** del bot, consulte [MicrosoftAppID y MicrosoftAppPassword](bot-service-manage-overview.md#microsoftappid-and-microsoftapppassword).</span><span class="sxs-lookup"><span data-stu-id="bc5ef-110">To find your bot's **AppID** and **AppPassword**, see [MicrosoftAppID and MicrosoftAppPassword](bot-service-manage-overview.md#microsoftappid-and-microsoftapppassword).</span></span>

## <a name="step-1-disable-security-and-test-on-localhost"></a><span data-ttu-id="bc5ef-111">Paso 1: Deshabilitar la seguridad y probar en el host local</span><span class="sxs-lookup"><span data-stu-id="bc5ef-111">Step 1: Disable security and test on localhost</span></span>

<span data-ttu-id="bc5ef-112">En este paso, comprobará que su bot es accesible y funcional en el host local cuando la seguridad está deshabilitada.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-112">In this step, you will verify that your bot is accessible and functional on localhost when security is disabled.</span></span> 

> [!WARNING]
> <span data-ttu-id="bc5ef-113">Deshabilitar la seguridad para el bot puede permitir que atacantes desconocidos suplanten a los usuarios.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-113">Disabling security for your bot may allow unknown attackers to impersonate users.</span></span> <span data-ttu-id="bc5ef-114">Solo debe implementar el procedimiento siguiente si está trabajando en un entorno de depuración protegido.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-114">Only implement the following procedure if you are operating in a protected debugging environment.</span></span>

### <a id="disable-security-localhost"></a> <span data-ttu-id="bc5ef-115">Deshabilitar la seguridad</span><span class="sxs-lookup"><span data-stu-id="bc5ef-115">Disable security</span></span>

<span data-ttu-id="bc5ef-116">Para deshabilitar la seguridad para el bot, edite sus valores de configuración para quitar los valores del identificador de aplicación y la contraseña.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-116">To disable security for your bot, edit its configuration settings to remove the values for app ID and password.</span></span> 

<span data-ttu-id="bc5ef-117">Si usa Bot Builder SDK para. NET, edite estas opciones en el archivo web.config:</span><span class="sxs-lookup"><span data-stu-id="bc5ef-117">If you're using the Bot Builder SDK for .NET, edit these settings in the Web.config file:</span></span>

```xml
<appSettings>
  <add key="MicrosoftAppId" value="" />
  <add key="MicrosoftAppPassword" value="" />
</appSettings>
```

<span data-ttu-id="bc5ef-118">Si usa Bot Builder SDK para Node.js, edite estos valores (o actualice las variables de entorno correspondientes):</span><span class="sxs-lookup"><span data-stu-id="bc5ef-118">If you're using the Bot Builder SDK for Node.js, edit these values (or update the corresponding environment variables):</span></span>

```javascript
var connector = new builder.ChatConnector({
  appId: null,
  appPassword: null
});
```

### <a name="test-your-bot-on-localhost"></a><span data-ttu-id="bc5ef-119">Prueba del bot en el host local</span><span class="sxs-lookup"><span data-stu-id="bc5ef-119">Test your bot on localhost</span></span> 

<span data-ttu-id="bc5ef-120">A continuación, pruebe el bot en el host local mediante Bot Framework Emulator.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-120">Next, test your bot on localhost by using the Bot Framework Emulator.</span></span>

1. <span data-ttu-id="bc5ef-121">Inicie el bot en el host local.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-121">Start your bot on localhost.</span></span>
2. <span data-ttu-id="bc5ef-122">Inicie Bot Framework Emulator.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-122">Start the Bot Framework Emulator.</span></span>
3. <span data-ttu-id="bc5ef-123">Conecte el bot usando el emulador.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-123">Connect to your bot using the emulator.</span></span>
    - <span data-ttu-id="bc5ef-124">Escriba `http://localhost:port-number/api/messages` en la barra de direcciones del emulador, donde **port-number** coincide con el número de puerto mostrado en el explorador en el que se ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-124">Type `http://localhost:port-number/api/messages` into the emulator's address bar, where **port-number** matches the port number shown in the browser where your application is running.</span></span> 
    - <span data-ttu-id="bc5ef-125">Asegúrese de dejar vacíos los campos **Microsoft App ID** (Id. de aplicación de Microsoft) y **Microsoft App Password** (Contraseña de aplicación de Microsoft).</span><span class="sxs-lookup"><span data-stu-id="bc5ef-125">Ensure that the **Microsoft App ID** and **Microsoft App Password** fields are both empty.</span></span>
    - <span data-ttu-id="bc5ef-126">Haga clic en **Conectar**.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-126">Click **Connect**.</span></span>
4. <span data-ttu-id="bc5ef-127">Para probar la conectividad al bot, escriba algún texto en el emulador y presione Entrar.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-127">To test connectivity to your bot, type some text into the emulator and press Enter.</span></span>

<span data-ttu-id="bc5ef-128">Si el bot responde a la entrada y no hay ningún error en la ventana de chat, ha comprobado que su bot es accesible y funcional en el host local cuando la seguridad está deshabilitada.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-128">If the bot responds to the input and there are no errors in the chat window, you have verified that your bot is accessible and functional on localhost when security is disabled.</span></span> <span data-ttu-id="bc5ef-129">Siga con el [Paso 2](#step-2).</span><span class="sxs-lookup"><span data-stu-id="bc5ef-129">Proceed to [Step 2](#step-2).</span></span>

<span data-ttu-id="bc5ef-130">Si se indica algún error en la ventana de chat, haga clic en él para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-130">If one or more error(s) are indicated in the chat window, click the error(s) for details.</span></span> <span data-ttu-id="bc5ef-131">Algunos problemas comunes son:</span><span class="sxs-lookup"><span data-stu-id="bc5ef-131">Common issues include:</span></span>

* <span data-ttu-id="bc5ef-132">La configuración del emulador especifica un punto de conexión incorrecto para el bot.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-132">The emulator settings specify an incorrect endpoint for the bot.</span></span> <span data-ttu-id="bc5ef-133">Asegúrese de que ha incluido el número de puerto adecuado en la dirección URL y la ruta de acceso correcta al final de la dirección URL (p. ej., `/api/messages`).</span><span class="sxs-lookup"><span data-stu-id="bc5ef-133">Make sure you have included the proper port number in the URL and the proper path at the end of the URL (e.g., `/api/messages`).</span></span>
* <span data-ttu-id="bc5ef-134">La configuración del emulador especifica un punto de conexión de bot que comienza con `https`.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-134">The emulator settings specify a bot endpoint that begins with `https`.</span></span> <span data-ttu-id="bc5ef-135">En el host local, el punto de conexión debe comenzar por `http`.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-135">On localhost, the endpoint should begin with `http`.</span></span>
* <span data-ttu-id="bc5ef-136">La configuración del emulador especifica un valor para los campos **Microsoft App ID** (Id. de aplicación de Microsoft) o **Microsoft App Password** (Contraseña de la aplicación de Microsoft).</span><span class="sxs-lookup"><span data-stu-id="bc5ef-136">The emulator settings specify a value for the **Microsoft App ID** field and/or the **Microsoft App Password** field.</span></span> <span data-ttu-id="bc5ef-137">Ambos campos deben estar vacíos.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-137">Both fields should be empty.</span></span>
* <span data-ttu-id="bc5ef-138">La seguridad no se ha deshabilitado para el bot.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-138">Security has not been disabled of for the bot.</span></span> <span data-ttu-id="bc5ef-139">[Compruebe](#disable-security-localhost) que el bot no especifica un valor para el identificador de aplicación o la contraseña.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-139">[Verify](#disable-security-localhost) that the bot does not specify a value for either app ID or password.</span></span>

## <a id="step-2"></a> <span data-ttu-id="bc5ef-140">Paso 2: Comprobar el identificador de aplicación y la contraseña del bot</span><span class="sxs-lookup"><span data-stu-id="bc5ef-140">Step 2: Verify your bot's app ID and password</span></span>

<span data-ttu-id="bc5ef-141">En este paso, comprobará que el identificador de aplicación y la contraseña que va a usar el bot para la autenticación son válidos.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-141">In this step, you will verify that the app ID and password that your bot will use for authentication are valid.</span></span> <span data-ttu-id="bc5ef-142">(Si no conoce estos valores, [obténgalos](#PW) ahora).</span><span class="sxs-lookup"><span data-stu-id="bc5ef-142">(If you do not know these values, [obtain them](#PW) now.)</span></span> 

> [!WARNING]
> <span data-ttu-id="bc5ef-143">Las siguientes instrucciones deshabilitan la comprobación de SSL para `login.microsoftonline.com`.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-143">The following instructions disable SSL verification for `login.microsoftonline.com`.</span></span> <span data-ttu-id="bc5ef-144">Realice este procedimiento solo en una red segura y considere la posibilidad de cambiar la contraseña de su aplicación posteriormente.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-144">Only perform this procedure on a secure network and consider changing your application's password afterward.</span></span>

### <a name="issue-an-http-request-to-the-microsoft-login-service"></a><span data-ttu-id="bc5ef-145">Emisión de una solicitud HTTP para el servicio de inicio de sesión de Microsoft</span><span class="sxs-lookup"><span data-stu-id="bc5ef-145">Issue an HTTP request to the Microsoft login service</span></span>

<span data-ttu-id="bc5ef-146">Estas instrucciones describen cómo usar [cURL](https://curl.haxx.se/download.html) para emitir una solicitud HTTP para el servicio de inicio de sesión de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-146">These instructions describe how to use [cURL](https://curl.haxx.se/download.html) to issue an HTTP request to the Microsoft login service.</span></span> <span data-ttu-id="bc5ef-147">Puede usar una herramienta alternativa como Postman, solo tiene que comprobar que la solicitud se ajusta al [protocolo de autenticación](~/rest-api/bot-framework-rest-connector-authentication.md) de Bot Framework.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-147">You may use an alternative tool such as Postman, just ensure that the request conforms to the Bot Framework [authentication protocol](~/rest-api/bot-framework-rest-connector-authentication.md).</span></span>

<span data-ttu-id="bc5ef-148">Para comprobar que el identificador de aplicación y la contraseña del bot son válidos, emita la siguiente solicitud con **cURL**, reemplazando `APP_ID` y `APP_PASSWORD` con el identificador de aplicación y la contraseña de su bot.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-148">To verify that your bot's app ID and password are valid, issue the following request using **cURL**, replacing `APP_ID` and `APP_PASSWORD` with your bot's app ID and password.</span></span>

```cmd
curl -k -X POST https://login.microsoftonline.com/botframework.com/oauth2/v2.0/token -d "grant_type=client_credentials&client_id=APP_ID&client_secret=APP_PASSWORD&scope=https%3A%2F%2Fapi.botframework.com%2F.default"
```

<span data-ttu-id="bc5ef-149">Esta solicitud intenta intercambiar el identificador de aplicación y la contraseña del su bot por un token de acceso.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-149">This request attempts to exchange your bot's app ID and password for an access token.</span></span> <span data-ttu-id="bc5ef-150">Si la solicitud se realiza correctamente, recibirá una carga JSON que contendrá una propiedad `access_token`, entre otros elementos.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-150">If the request is successful, you will receive a JSON payload that contains an `access_token` property, amongst others.</span></span> 

```json
{"token_type":"Bearer","expires_in":3599,"ext_expires_in":0,"access_token":"eyJ0eXAJKV1Q..."}
```

<span data-ttu-id="bc5ef-151">Si la solicitud se realiza correctamente, ha comprobado que el identificador de aplicación y la contraseña que especificó en la solicitud son válidos.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-151">If the request is successful, you have verified that the app ID and password that you specified in the request are valid.</span></span> <span data-ttu-id="bc5ef-152">Siga con el [Paso 3](#step-3).</span><span class="sxs-lookup"><span data-stu-id="bc5ef-152">Proceed to [Step 3](#step-3).</span></span>

<span data-ttu-id="bc5ef-153">Si recibe un error en respuesta a la solicitud, examine la respuesta para identificar la causa.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-153">If you receive an error in response to the request, examine the response to identify the cause of the error.</span></span> <span data-ttu-id="bc5ef-154">Si la respuesta indica que el identificador de aplicación o la contraseña no son válidos, [obtenga los valores correctos](#PW) desde el portal de Bot Framework y vuelva a emitir la solicitud con los nuevos valores para confirmar que son válidos.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-154">If the response indicates that the app ID or the password is invalid, [obtain the correct values](#PW) from the Bot Framework Portal and re-issue the request with the new values to confirm that they are valid.</span></span> 

## <span data-ttu-id="bc5ef-155">Paso 3: Deshabilitar la seguridad y probar en el host local<a id="step-3"></a></span><span class="sxs-lookup"><span data-stu-id="bc5ef-155">Step 3: Enable security and test on localhost <a id="step-3"></a></span></span>

<span data-ttu-id="bc5ef-156">En este momento, ha comprobado que su bot es accesible y funcional en el host local cuando la seguridad está deshabilitada y ha confirmado que el identificador de aplicación y la contraseña que utilizará el bot para la autenticación son válidos.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-156">At this point, you have verified that your bot is accessible and functional on localhost when security is disabled and confirmed that the app ID and password that the bot will use for authentication are valid.</span></span> <span data-ttu-id="bc5ef-157">En este paso, comprobará que su bot es accesible y funcional en el host local cuando la seguridad está habilitada.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-157">In this step, you will verify that your bot is accessible and functional on localhost when security is enabled.</span></span>

### <a id="enable-security-localhost"></a> <span data-ttu-id="bc5ef-158">Habilitar la seguridad</span><span class="sxs-lookup"><span data-stu-id="bc5ef-158">Enable security</span></span>

<span data-ttu-id="bc5ef-159">La seguridad del bot se basa en los servicios de Microsoft, incluso cuando se ejecuta en el host local.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-159">Your bot's security relies on Microsoft services, even when your bot is running only on localhost.</span></span> <span data-ttu-id="bc5ef-160">Para habilitar la seguridad para el bot, edite sus valores de configuración para rellenar el identificador de aplicación y la contraseña con los valores que comprobó en el [Paso 2](#step-2).</span><span class="sxs-lookup"><span data-stu-id="bc5ef-160">To enable security for your bot, edit its configuration settings to populate app ID and password with the values that you verified in [Step 2](#step-2).</span></span>

<span data-ttu-id="bc5ef-161">Si usa Bot Builder SDK para. NET, rellene estas opciones en el archivo Web.config:</span><span class="sxs-lookup"><span data-stu-id="bc5ef-161">If you're using the Bot Builder SDK for .NET, populate these settings in the Web.config file:</span></span>

```xml
<appSettings>
  <add key="MicrosoftAppId" value="APP_ID" />
  <add key="MicrosoftAppPassword" value="PASSWORD" />
</appSettings>
```

<span data-ttu-id="bc5ef-162">Si usa Bot Builder SDK para Node.js, rellene estos valores (o actualice las variables de entorno correspondientes):</span><span class="sxs-lookup"><span data-stu-id="bc5ef-162">If you're using the Bot Builder SDK for Node.js, populate these settings (or update the corresponding environment variables):</span></span>

```javascript
var connector = new builder.ChatConnector({
  appId: 'APP_ID',
  appPassword: 'PASSWORD'
});
```

> [!NOTE]
> <span data-ttu-id="bc5ef-163">Para buscar los valores **AppID** y **AppPassword** del bot, consulte [MicrosoftAppID y MicrosoftAppPassword](bot-service-manage-overview.md#microsoftappid-and-microsoftapppassword).</span><span class="sxs-lookup"><span data-stu-id="bc5ef-163">To find your bot's **AppID** and **AppPassword**, see [MicrosoftAppID and MicrosoftAppPassword](bot-service-manage-overview.md#microsoftappid-and-microsoftapppassword).</span></span>

### <a name="test-your-bot-on-localhost"></a><span data-ttu-id="bc5ef-164">Prueba del bot en el host local</span><span class="sxs-lookup"><span data-stu-id="bc5ef-164">Test your bot on localhost</span></span> 

<span data-ttu-id="bc5ef-165">A continuación, pruebe el bot en el host local mediante Bot Framework Emulator.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-165">Next, test your bot on localhost by using the Bot Framework Emulator.</span></span>

1. <span data-ttu-id="bc5ef-166">Inicie el bot en el host local.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-166">Start your bot on localhost.</span></span>
2. <span data-ttu-id="bc5ef-167">Inicie Bot Framework Emulator.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-167">Start the Bot Framework Emulator.</span></span>
3. <span data-ttu-id="bc5ef-168">Conecte el bot usando el emulador.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-168">Connect to your bot using the emulator.</span></span>
    - <span data-ttu-id="bc5ef-169">Escriba `http://localhost:port-number/api/messages` en la barra de direcciones del emulador, donde **port-number** coincide con el número de puerto mostrado en el explorador en el que se ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-169">Type `http://localhost:port-number/api/messages` into the emulator's address bar, where **port-number** matches the port number shown in the browser where your application is running.</span></span> 
    - <span data-ttu-id="bc5ef-170">Escriba el identificador de la aplicación del bot en el campo **Microsoft App ID** (Id. de aplicación de Microsoft).</span><span class="sxs-lookup"><span data-stu-id="bc5ef-170">Enter your bot's app ID into the **Microsoft App ID** field.</span></span>
    - <span data-ttu-id="bc5ef-171">Escriba la contraseña del bot en el campo **Microsoft App Password** (Contraseña de aplicación de Microsoft).</span><span class="sxs-lookup"><span data-stu-id="bc5ef-171">Enter your bot's password into the **Microsoft App Password** field.</span></span>
    - <span data-ttu-id="bc5ef-172">Haga clic en **Conectar**.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-172">Click **Connect**.</span></span>
4. <span data-ttu-id="bc5ef-173">Para probar la conectividad al bot, escriba algún texto en el emulador y presione Entrar.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-173">To test connectivity to your bot, type some text into the emulator and press Enter.</span></span>

<span data-ttu-id="bc5ef-174">Si el bot responde a la entrada y no hay ningún error en la ventana de chat, ha comprobado que su bot es accesible y funcional en el host local cuando la seguridad está habilitada.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-174">If the bot responds to the input and there are no errors in the chat window, you have verified that your bot is accessible and functional on localhost when security is enabled.</span></span>  <span data-ttu-id="bc5ef-175">Siga con el [Paso 4](#step-4).</span><span class="sxs-lookup"><span data-stu-id="bc5ef-175">Proceed to [Step 4](#step-4).</span></span>

<span data-ttu-id="bc5ef-176">Si se indica algún error en la ventana de chat, haga clic en él para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-176">If one or more error(s) are indicated in the chat window, click the error(s) for details.</span></span> <span data-ttu-id="bc5ef-177">Algunos problemas comunes son:</span><span class="sxs-lookup"><span data-stu-id="bc5ef-177">Common issues include:</span></span>

* <span data-ttu-id="bc5ef-178">La configuración del emulador especifica un punto de conexión incorrecto para el bot.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-178">The emulator settings specify an incorrect endpoint for the bot.</span></span> <span data-ttu-id="bc5ef-179">Asegúrese de que ha incluido el número de puerto adecuado en la dirección URL y la ruta de acceso correcta al final de la dirección URL (p. ej., `/api/messages`).</span><span class="sxs-lookup"><span data-stu-id="bc5ef-179">Make sure you have included the proper port number in the URL and the proper path at the end of the URL (e.g., `/api/messages`).</span></span>
* <span data-ttu-id="bc5ef-180">La configuración del emulador especifica un punto de conexión de bot que comienza con `https`.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-180">The emulator settings specify a bot endpoint that begins with `https`.</span></span> <span data-ttu-id="bc5ef-181">En el host local, el punto de conexión debe comenzar por `http`.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-181">On localhost, the endpoint should begin with `http`.</span></span>
* <span data-ttu-id="bc5ef-182">En la configuración del emulador, los campos **Microsoft App ID** (Id. de aplicación de Microsoft) y **Microsoft App Password** (Contraseña de la aplicación de Microsoft) no contienen valores válidos.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-182">In the emulator settings, the **Microsoft App ID** field and/or the **Microsoft App Password** do not contain valid values.</span></span> <span data-ttu-id="bc5ef-183">Se deben rellenar ambos campos y cada uno debe contener el valor correspondiente que comprobó en el [Paso 2](#step-2).</span><span class="sxs-lookup"><span data-stu-id="bc5ef-183">Both fields should be populated and each field should contain the corresponding value that you verified in [Step 2](#step-2).</span></span>
* <span data-ttu-id="bc5ef-184">La seguridad no se ha habilitado para el bot.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-184">Security has not been enabled for the bot.</span></span> <span data-ttu-id="bc5ef-185">[Compruebe](#enable-security-localhost) que la configuración del bot especifica valores para el identificador de aplicación y la contraseña.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-185">[Verify](#enable-security-localhost) that the bot configuration settings specify values for both app ID and password.</span></span>

## <span data-ttu-id="bc5ef-186">Paso 4: Probar el bot en la nube <a id="step-4"></a></span><span class="sxs-lookup"><span data-stu-id="bc5ef-186">Step 4: Test your bot in the cloud <a id="step-4"></a></span></span>

<span data-ttu-id="bc5ef-187">En este momento, ha comprobado que su bot es accesible y funcional en el host local cuando la seguridad está deshabilitada, ha confirmado que el identificador de aplicación y la contraseña del bot son válidos, y ha comprobado que es accesible y funcional en el host local cuando la seguridad está habilitada.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-187">At this point, you have verified that your bot is accessible and functional on localhost when security is disabled, confirmed that your bot's app ID and password are valid, and verified that your bot is accessible and functional on localhost when security is enabled.</span></span> <span data-ttu-id="bc5ef-188">En este paso, implementará el bot en la nube y comprobará que es accesible y funcional con la seguridad habilitada.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-188">In this step, you will deploy your bot to the cloud and verify that it is accessible and functional there with security enabled.</span></span> 

### <a name="deploy-your-bot-to-the-cloud"></a><span data-ttu-id="bc5ef-189">Implementación de un bot en la nube</span><span class="sxs-lookup"><span data-stu-id="bc5ef-189">Deploy your bot to the cloud</span></span>

<span data-ttu-id="bc5ef-190">Bot Framework requiere que los bots sean accesibles desde Internet, por lo que debe implementar el bot en una plataforma con hospedaje en la nube como Azure.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-190">The Bot Framework requires that bots be accessible from the internet, so you must deploy your bot to a cloud hosting platform such as Azure.</span></span> <span data-ttu-id="bc5ef-191">Asegúrese de habilitar la seguridad para el bot antes de la implementación, como se describe en el [Paso 3](#step-3).</span><span class="sxs-lookup"><span data-stu-id="bc5ef-191">Be sure to enable security for your bot prior to deployment, as described in [Step 3](#step-3).</span></span>

> [!NOTE]
> <span data-ttu-id="bc5ef-192">Si aún no tiene un proveedor de hospedaje en la nube, puede registrarse para obtener una <a href="https://azure.microsoft.com/en-us/free/" target="_blank">cuenta gratuita</a>.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-192">If you do not already have a cloud hosting provider, you can register for a <a href="https://azure.microsoft.com/en-us/free/" target="_blank">free account</a>..</span></span> 

<span data-ttu-id="bc5ef-193">Al implementar el bot en Azure, SSL se configurará automáticamente para la aplicación, lo que permite habilitar el punto de conexión de **HTTPS** que Bot Framework requiere.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-193">If you deploy your bot to Azure, SSL will automatically be configured for your application, thereby enabling the **HTTPS** endpoint that the Bot Framework requires.</span></span> <span data-ttu-id="bc5ef-194">Si realiza la implementación en otro proveedor de hospedaje en la nube, asegúrese de comprobar que la aplicación está configurada para SSL, a fin de que el bot disponga de un punto de conexión **HTTPS**.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-194">If you deploy to another cloud hosting provider, be sure to verify that your application is configured for SSL so that the bot will have an **HTTPS** endpoint.</span></span>

### <a name="test-your-bot"></a><span data-ttu-id="bc5ef-195">Prueba del bot</span><span class="sxs-lookup"><span data-stu-id="bc5ef-195">Test your bot</span></span> 

<span data-ttu-id="bc5ef-196">Para probar el bot en la nube con la seguridad habilitada, complete los pasos siguientes.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-196">To test your bot in the cloud with security enabled, complete the following steps.</span></span>

1. <span data-ttu-id="bc5ef-197">Asegúrese de que el bot se ha implementado correctamente y se está ejecutando.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-197">Ensure that your bot has been successfully deployed and is running.</span></span> 
2. <span data-ttu-id="bc5ef-198">Inicie sesión en el <a href="https://dev.botframework.com" target="_blank">portal de Bot Framework</a>.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-198">Sign in to the <a href="https://dev.botframework.com" target="_blank">Bot Framework Portal</a>.</span></span>
3. <span data-ttu-id="bc5ef-199">Haga clic en **My Bots** (Mis bots).</span><span class="sxs-lookup"><span data-stu-id="bc5ef-199">Click **My Bots**.</span></span>
4. <span data-ttu-id="bc5ef-200">Seleccione el bot que quiera probar.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-200">Select the bot that you want to test.</span></span>
5. <span data-ttu-id="bc5ef-201">Haga clic en **Test** (Prueba) para abrir el bot en un control de charla web incrustado.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-201">Click **Test** to open the bot in an embedded web chat control.</span></span>
6. <span data-ttu-id="bc5ef-202">Para probar la conectividad al bot, escriba en el control de chat web y presione Entrar.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-202">To test connectivity to your bot, type some text into the web chat control and press Enter.</span></span>

<span data-ttu-id="bc5ef-203">Si se indica un error en la ventana de chat, utilice el mensaje de error para determinar la causa del error.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-203">If an error is indicated in the chat window, use the error message to determine the cause of the error.</span></span> <span data-ttu-id="bc5ef-204">Algunos problemas comunes son:</span><span class="sxs-lookup"><span data-stu-id="bc5ef-204">Common issues include:</span></span> 

* <span data-ttu-id="bc5ef-205">La información de **Messaging endpoint** (Punto de conexión de mensajería) especificada en la página **Settings** (Configuración) para el bot en el portal de Bot Framework es incorrecta.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-205">The **Messaging endpoint** specified on the **Settings** page for your bot in the Bot Framework Portal is incorrect.</span></span> <span data-ttu-id="bc5ef-206">Asegúrese de que ha incluido la ruta de acceso correcta al final de la dirección URL (p. ej., `/api/messages`).</span><span class="sxs-lookup"><span data-stu-id="bc5ef-206">Make sure you have included the proper path at the end of the URL (e.g., `/api/messages`).</span></span>
* <span data-ttu-id="bc5ef-207">La información de **Messaging endpoint** (Punto de conexión de mensajería) especificada en la página **Settings** (Configuración) para el bot en el portal de Bot Framework no comienza con `https` o no es de confianza para Bot Framework.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-207">The **Messaging endpoint** specified on the **Settings** page for your bot in the Bot Framework Portal does not begin with `https` or is not trusted by the Bot Framework.</span></span> <span data-ttu-id="bc5ef-208">El bot debe tener un certificado de confianza en la cadena válido.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-208">Your bot must have a valid, chain-trusted certificate.</span></span>
* <span data-ttu-id="bc5ef-209">Faltan valores de configuración para el bot o son incorrectos para el identificador de aplicación o la contraseña.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-209">The bot is configured with missing or incorrect values for app ID or password.</span></span> <span data-ttu-id="bc5ef-210">[Compruebe](#enable-security-localhost) que los valores de configuración del bot son válidos para el identificador de aplicación y la contraseña.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-210">[Verify](#enable-security-localhost) that the bot configuration settings specify valid values for app ID and password.</span></span>

<span data-ttu-id="bc5ef-211">Si el bot responde correctamente a la entrada, ha comprobado que su bot es accesible y funcional en la nube con la seguridad habilitada.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-211">If the bot responds appropriately to the input, you have verified that your bot is accessible and functional in the cloud with security enabled.</span></span> <span data-ttu-id="bc5ef-212">En este momento, el bot está listo para [conectarse a un canal](~/bot-service-manage-channels.md) como Skype, Facebook Messenger, Direct Line y otros de forma segura.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-212">At this point, your bot is ready to securely [connect to a channel](~/bot-service-manage-channels.md) such as Skype, Facebook Messenger, Direct Line, and others.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bc5ef-213">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="bc5ef-213">Additional resources</span></span>

<span data-ttu-id="bc5ef-214">Si sigue experimentando problemas después de completar los pasos anteriores, puede:</span><span class="sxs-lookup"><span data-stu-id="bc5ef-214">If you are still experiencing issues after completing the steps above, you can:</span></span>

* <span data-ttu-id="bc5ef-215">[Depurar el bot en la nube](~/bot-service-debug-emulator.md) mediante Bot Framework Emulator y <a href="https://ngrok.com/" target="_blank">ngrok</a>.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-215">[Debug your bot in the cloud](~/bot-service-debug-emulator.md) using the Bot Framework Emulator and <a href="https://ngrok.com/" target="_blank">ngrok</a>.</span></span>
* <span data-ttu-id="bc5ef-216">Usar una herramienta de creación de conexiones proxy como [Fiddler](https://www.telerik.com/fiddler) para inspeccionar el tráfico HTTPS a y desde su bot.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-216">Use a proxying tool like [Fiddler](https://www.telerik.com/fiddler) to inspect HTTPS traffic to and from your bot.</span></span> <span data-ttu-id="bc5ef-217">*Fiddler no es un producto de Microsoft.*</span><span class="sxs-lookup"><span data-stu-id="bc5ef-217">*Fiddler is not a Microsoft product.*</span></span>
* <span data-ttu-id="bc5ef-218">Revisar la [Guía de autenticación de Bot Connector][ BotConnectorAuthGuide] para obtener información acerca de las tecnologías de autenticación que usa Bot Framework.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-218">Review the [Bot Connector authentication guide][BotConnectorAuthGuide] to learn about the authentication technologies that the Bot Framework uses.</span></span>
* <span data-ttu-id="bc5ef-219">Solicitar ayuda a otros usuarios con los recursos de [soporte técnico][Support] de Bot Framework.</span><span class="sxs-lookup"><span data-stu-id="bc5ef-219">Solicit help from others by using the Bot Framework [support][Support] resources.</span></span> 

[BotConnectorAuthGuide]: ~/rest-api/bot-framework-rest-connector-authentication.md
[Support]: bot-service-resources-links-help.md
[Emulator]: bot-service-debug-emulator.md
