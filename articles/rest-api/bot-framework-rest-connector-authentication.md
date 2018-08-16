---
title: Autenticación de solicitudes | Microsoft Docs
description: Obtenga información sobre cómo autenticar las solicitudes de API en Bot Connector API y en Bot State API.
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ms.openlocfilehash: 53643f21e2cf1ebdfd84caed38f8f84c330ef71b
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305572"
---
# <a name="authentication"></a><span data-ttu-id="65f7d-103">Autenticación</span><span class="sxs-lookup"><span data-stu-id="65f7d-103">Authentication</span></span>

<span data-ttu-id="65f7d-104">El bot se comunica con el servicio Bot Connector con HTTP mediante un canal seguro (SSL/TLS).</span><span class="sxs-lookup"><span data-stu-id="65f7d-104">Your bot communicates with the Bot Connector service using HTTP over a secured channel (SSL/TLS).</span></span> <span data-ttu-id="65f7d-105">Cuando el bot envía una solicitud al servicio Connector, debe incluir información que el servicio Connector puede usar para comprobar su identidad.</span><span class="sxs-lookup"><span data-stu-id="65f7d-105">When your bot sends a request to the Connector service, it must include information that the Connector service can use to verify its identity.</span></span> <span data-ttu-id="65f7d-106">Del mismo modo, cuando el servicio Connector envía una solicitud al bot, debe incluir información que el bot puede usar para comprobar su identidad.</span><span class="sxs-lookup"><span data-stu-id="65f7d-106">Likewise, when the Connector service sends a request to your bot, it must include information that the bot can use to verify its identity.</span></span> <span data-ttu-id="65f7d-107">En este artículo se describen las tecnologías y los requisitos para la autenticación en el nivel de servicio que tiene lugar entre un bot y el servicio Bot Connector.</span><span class="sxs-lookup"><span data-stu-id="65f7d-107">This article describes the authentication technologies and requirements for the service-level authentication that takes place between a bot and the Bot Connector service.</span></span> <span data-ttu-id="65f7d-108">Si está escribiendo su propio código de autenticación, debe implementar los procedimientos de seguridad descritos en este artículo para habilitar al bot para el intercambio de mensajes con el servicio Bot Connector.</span><span class="sxs-lookup"><span data-stu-id="65f7d-108">If you are writing your own authentication code, you must implement the security procedures described in this article to enable your bot to exchange messages with the Bot Connector service.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="65f7d-109">Si está escribiendo su propio código de autenticación, es fundamental que aplique correctamente todos los procedimientos de seguridad.</span><span class="sxs-lookup"><span data-stu-id="65f7d-109">If you are writing your own authentication code, it is critical that you implement all security procedures correctly.</span></span> <span data-ttu-id="65f7d-110">Mediante la implementación de todos los pasos de este artículo, puede mitigar el riesgo de que un atacante pueda leer los mensajes que se envían al bot, enviar mensajes que suplanten al bot y robar las claves secretas.</span><span class="sxs-lookup"><span data-stu-id="65f7d-110">By implementing all steps in this article, you can mitigate the risk of an attacker being able to read messages that are sent to your bot, send messages that impersonate your bot, and steal secret keys.</span></span> 

<span data-ttu-id="65f7d-111">Si usa [Bot Builder SDK para .NET](../dotnet/bot-builder-dotnet-overview.md) o [Bot Builder SDK para Node.js](../nodejs/index.md), no es necesario implementar los procedimientos de seguridad descritos en este artículo, que el SDK lo hace automáticamente.</span><span class="sxs-lookup"><span data-stu-id="65f7d-111">If you are using the [Bot Builder SDK for .NET](../dotnet/bot-builder-dotnet-overview.md) or the [Bot Builder SDK for Node.js](../nodejs/index.md), you do not need to implement the security procedures described in this article, because the SDK automatically does it for you.</span></span> <span data-ttu-id="65f7d-112">Basta con configurar el proyecto con el identificador de aplicación y la contraseña que obtuvo para el bot durante el [registro](../bot-service-quickstart-registration.md) y el SDK controlará el resto.</span><span class="sxs-lookup"><span data-stu-id="65f7d-112">Simply configure your project with the App ID and password that you obtained for your bot during [registration](../bot-service-quickstart-registration.md) and the SDK will handle the rest.</span></span>

> [!WARNING]
> <span data-ttu-id="65f7d-113">En diciembre de 2016, la versión v3.1 del protocolo de seguridad de Bot Framework presentó cambios en varios valores que se usan durante la generación y validación de los tokens.</span><span class="sxs-lookup"><span data-stu-id="65f7d-113">In December 2016, v3.1 of the Bot Framework security protocol introduced changes to several values that are used during token generation and validation.</span></span> <span data-ttu-id="65f7d-114">A finales de otoño de 2017, la versión v3.2 del protocolo de seguridad de Bot Framework presenta, de nuevo, cambios en valores que se usan durante la generación y validación de los tokens.</span><span class="sxs-lookup"><span data-stu-id="65f7d-114">In late fall of 2017, v3.2 of the Bot Framework security protocol will be introduced which will again include changes to values that are used during token generation and validation.</span></span>
> <span data-ttu-id="65f7d-115">Para más información, consulte [Cambios en el protocolo de seguridad](#security-protocol-changes).</span><span class="sxs-lookup"><span data-stu-id="65f7d-115">For more information, see [Security protocol changes](#security-protocol-changes).</span></span>

## <a name="authentication-technologies"></a><span data-ttu-id="65f7d-116">Tecnologías de autenticación</span><span class="sxs-lookup"><span data-stu-id="65f7d-116">Authentication technologies</span></span>

<span data-ttu-id="65f7d-117">Se utilizan cuatro tecnologías de autenticación para establecer la confianza entre un bot y Bot Connector:</span><span class="sxs-lookup"><span data-stu-id="65f7d-117">Four authentication technologies are used to establish trust between a bot and the Bot Connector:</span></span>

| <span data-ttu-id="65f7d-118">Technology</span><span class="sxs-lookup"><span data-stu-id="65f7d-118">Technology</span></span> | <span data-ttu-id="65f7d-119">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="65f7d-119">Description</span></span> |
|----|----|
| <span data-ttu-id="65f7d-120">**SSL/TLS**</span><span class="sxs-lookup"><span data-stu-id="65f7d-120">**SSL/TLS**</span></span> | <span data-ttu-id="65f7d-121">Se usa SSL/TLS para todas las conexiones de servicio a servicio.</span><span class="sxs-lookup"><span data-stu-id="65f7d-121">SSL/TLS is used for all service-to-service connections.</span></span> <span data-ttu-id="65f7d-122">Se utilizan certificados`X.509v3` para establecer la identidad de todos los servicios HTTPS.</span><span class="sxs-lookup"><span data-stu-id="65f7d-122">`X.509v3` certificates are used to establish the identity of all HTTPS services.</span></span> <span data-ttu-id="65f7d-123">**Los clientes siempre deben inspeccionar los certificados del servicio para asegurarse de que son válidos y de confianza.**</span><span class="sxs-lookup"><span data-stu-id="65f7d-123">**Clients should always inspect service certificates to ensure they are trusted and valid.**</span></span> <span data-ttu-id="65f7d-124">(No se usan certificados de cliente como parte de este esquema).</span><span class="sxs-lookup"><span data-stu-id="65f7d-124">(Client certificates are NOT used as part of this scheme.)</span></span> |
| <span data-ttu-id="65f7d-125">**OAuth 2.0**</span><span class="sxs-lookup"><span data-stu-id="65f7d-125">**OAuth 2.0**</span></span> | <span data-ttu-id="65f7d-126">El inicio de sesión de OAuth 2.0 de la cuenta Microsoft (MSA) y el servicio de inicio de sesión de AAD v2 se usan para generar un token seguro que un bot puede usar para enviar mensajes.</span><span class="sxs-lookup"><span data-stu-id="65f7d-126">OAuth 2.0 login to the Microsoft Account (MSA)/AAD v2 login service is used to generate a secure token that a bot can use to send messages.</span></span> <span data-ttu-id="65f7d-127">Este token es un token de servicio a servicio, no es necesario ningún inicio de sesión de usuario.</span><span class="sxs-lookup"><span data-stu-id="65f7d-127">This token is a service-to-service token; no user login is required.</span></span> |
| <span data-ttu-id="65f7d-128">**JSON Web Token (JWT)**</span><span class="sxs-lookup"><span data-stu-id="65f7d-128">**JSON Web Token (JWT)**</span></span> | <span data-ttu-id="65f7d-129">Los JSON Web Token se usan para codificar los tokens que se envían hacia y desde el bot.</span><span class="sxs-lookup"><span data-stu-id="65f7d-129">JSON Web Tokens are used to encode tokens that are sent to and from the bot.</span></span> <span data-ttu-id="65f7d-130">**Los clientes deben comprobar completamente todos los tokens JWT que reciben**, según los requisitos descritos en este artículo.</span><span class="sxs-lookup"><span data-stu-id="65f7d-130">**Clients should fully verify all JWT tokens that they receive**, according to the requirements outlined in this article.</span></span> |
| <span data-ttu-id="65f7d-131">**Metadatos de OpenID**</span><span class="sxs-lookup"><span data-stu-id="65f7d-131">**OpenID metadata**</span></span> | <span data-ttu-id="65f7d-132">El servicio Bot Connector publica una lista de tokens válidos que usa para firmar sus propios tokens JWT en los metadatos de OpenID en un punto de conexión conocido y estático.</span><span class="sxs-lookup"><span data-stu-id="65f7d-132">The Bot Connector service publishes a list of valid tokens that it uses to sign its own JWT tokens to OpenID metadata at a well-known, static endpoint.</span></span> |

<span data-ttu-id="65f7d-133">En este artículo se describe cómo usar estas tecnologías mediante HTTPS y JSON estándar.</span><span class="sxs-lookup"><span data-stu-id="65f7d-133">This article describes how to use these technologies via standard HTTPS and JSON.</span></span> <span data-ttu-id="65f7d-134">No es necesario ningún SDK especial, aunque es posible que las aplicaciones auxiliares para OpenID y otras le sean útiles.</span><span class="sxs-lookup"><span data-stu-id="65f7d-134">No special SDKs are required, although you may find that helpers for OpenID etc. are useful.</span></span>

## <a id="bot-to-connector"></a> <span data-ttu-id="65f7d-135">Autenticación de las solicitudes del bot al servicio Bot Connector</span><span class="sxs-lookup"><span data-stu-id="65f7d-135">Authenticate requests from your bot to the Bot Connector service</span></span>

<span data-ttu-id="65f7d-136">Para comunicarse con el servicio Bot Connector, debe especificar un token de acceso en el encabezado `Authorization` de cada solicitud de API, con este formato:</span><span class="sxs-lookup"><span data-stu-id="65f7d-136">To communicate with the Bot Connector service, you must specify an access token in the `Authorization` header of each API request, using this format:</span></span> 

```http
Authorization: Bearer ACCESS_TOKEN
```

<span data-ttu-id="65f7d-137">Este diagrama muestra los pasos de la autenticación de bot a Connector:</span><span class="sxs-lookup"><span data-stu-id="65f7d-137">This diagram shows the steps for bot-to-connector authentication:</span></span>

![Autenticación con el servicio de inicio de sesión de MSA y, a continuación, con el bot](../media/connector/auth_bot_to_bot_connector.png)

> [!IMPORTANT]
> <span data-ttu-id="65f7d-139">Si aún no lo ha hecho, deberá [registrar el bot](../bot-service-quickstart-registration.md) con Bot Framework para obtener el identificador de aplicación y la contraseña.</span><span class="sxs-lookup"><span data-stu-id="65f7d-139">If you have not already done so, you must [register your bot](../bot-service-quickstart-registration.md) with the Bot Framework to obtain its AppID and password.</span></span> <span data-ttu-id="65f7d-140">Necesitará el identificador de la aplicación del bot y la contraseña para solicitar un token de acceso.</span><span class="sxs-lookup"><span data-stu-id="65f7d-140">You will need the bot's App ID and password to request an access token.</span></span>

### <a name="step-1-request-an-access-token-from-the-msaaad-v2-login-service"></a><span data-ttu-id="65f7d-141">Paso 1: Solicitud de un token de acceso al servicio de inicio de sesión de MSA o AAD v2</span><span class="sxs-lookup"><span data-stu-id="65f7d-141">Step 1: Request an access token from the MSA/AAD v2 login service</span></span>

<span data-ttu-id="65f7d-142">Para solicitar un token de acceso al servicio de inicio de sesión de MSA o AAD v2, emita la siguiente solicitud y reemplace **MICROSOFT-APP-ID** y **MICROSOFT-APP-PASSWORD** por el identificador de aplicación y la contraseña que obtuvo cuando [registró](../bot-service-quickstart-registration.md) el bot con Bot Framework.</span><span class="sxs-lookup"><span data-stu-id="65f7d-142">To request an access token from the MSA/AAD v2 login service, issue the following request, replacing **MICROSOFT-APP-ID** and **MICROSOFT-APP-PASSWORD** with the AppID and password that you obtained when you [registered](../bot-service-quickstart-registration.md) your bot with the Bot Framework.</span></span>

```http
POST https://login.microsoftonline.com/botframework.com/oauth2/v2.0/token
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&client_id=MICROSOFT-APP-ID&client_secret=MICROSOFT-APP-PASSWORD&scope=https%3A%2F%2Fapi.botframework.com%2F.default
```

### <a name="step-2-obtain-the-jwt-token-from-the-msaaad-v2-login-service-response"></a><span data-ttu-id="65f7d-143">Paso 2: Obtención del token JWT de la respuesta del servicio de inicio de sesión de MSA o AAD v2</span><span class="sxs-lookup"><span data-stu-id="65f7d-143">Step 2: Obtain the JWT token from the MSA/AAD v2 login service response</span></span>

<span data-ttu-id="65f7d-144">Si la aplicación es autorizada por el servicio de inicio de sesión de MSA o AAD v2, el cuerpo de la respuesta JSON especificará el token de acceso, su tipo y la expiración (en segundos).</span><span class="sxs-lookup"><span data-stu-id="65f7d-144">If your application is authorized by the MSA/AAD v2 login service, the JSON response body will specify your access token, its type, and its expiration (in seconds).</span></span> 

<span data-ttu-id="65f7d-145">Al agregar el token al encabezado `Authorization` de una solicitud, debe usar el valor exacto especificado en esta respuesta (es decir, sin escape ni codificación del valor del token).</span><span class="sxs-lookup"><span data-stu-id="65f7d-145">When adding the token to the `Authorization` header of a request, you must use the exact value that is specified in this response (i.e., do not escape or encode the token value).</span></span> <span data-ttu-id="65f7d-146">El token de acceso es válido hasta su expiración.</span><span class="sxs-lookup"><span data-stu-id="65f7d-146">The access token is valid until its expiration.</span></span> <span data-ttu-id="65f7d-147">Para evitar que expire el token y afecte al rendimiento del bot, puede almacenarlo en caché y actualizar el token de forma proactiva.</span><span class="sxs-lookup"><span data-stu-id="65f7d-147">To prevent token expiration from impacting your bot's performance, you may choose to cache and proactively refresh the token.</span></span>

<span data-ttu-id="65f7d-148">En este ejemplo se muestra una respuesta del servicio de inicio de sesión de MSA o AAD v2:</span><span class="sxs-lookup"><span data-stu-id="65f7d-148">This example shows a response from the MSA/AAD v2 login service:</span></span>

```http
HTTP/1.1 200 OK
... (other headers) 
```

```json
{
    "token_type":"Bearer",
    "expires_in":3600,
    "ext_expires_in":3600,
    "access_token":"eyJhbGciOiJIUzI1Ni..."
}
```

### <a name="step-3-specify-the-jwt-token-in-the-authorization-header-of-requests"></a><span data-ttu-id="65f7d-149">Paso 3: Especificación del token JWT en el encabezado Authorization de las solicitudes</span><span class="sxs-lookup"><span data-stu-id="65f7d-149">Step 3: Specify the JWT token in the Authorization header of requests</span></span>

<span data-ttu-id="65f7d-150">Cuando se envía una solicitud de API al servicio Bot Connector, debe especificar el token de acceso en el encabezado `Authorization` de la solicitud con este formato:</span><span class="sxs-lookup"><span data-stu-id="65f7d-150">When you send an API request to the Bot Connector service, specify the access token in the `Authorization` header of the request using this format:</span></span>

```http
Authorization: Bearer ACCESS_TOKEN
```

<span data-ttu-id="65f7d-151">Todas las solicitudes que envíe al servicio Bot Connector deben incluir el token de acceso en el encabezado `Authorization`.</span><span class="sxs-lookup"><span data-stu-id="65f7d-151">All requests that you send to the Bot Connector service must include the access token in the `Authorization` header.</span></span> <span data-ttu-id="65f7d-152">Si el token tiene un formato correcto, no ha expirado y fue generado por el servicio de inicio de sesión de MSA o AAD v2, el servicio Bot Connector autorizará la solicitud.</span><span class="sxs-lookup"><span data-stu-id="65f7d-152">If the token is correctly formed, is not expired, and was generated by the MSA/AAD v2 login service, the Bot Connector service will authorize the request.</span></span> <span data-ttu-id="65f7d-153">Se realizan comprobaciones adicionales para asegurarse de que el token pertenece al bot que envió la solicitud.</span><span class="sxs-lookup"><span data-stu-id="65f7d-153">Additional checks are performed to ensure that the token belongs to the bot that sent the request.</span></span>

<span data-ttu-id="65f7d-154">El ejemplo siguiente muestra cómo especificar el token de acceso en el encabezado `Authorization` de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="65f7d-154">The following example shows how to specify the access token in the `Authorization` header of the request.</span></span> 

```http
POST https://smba.trafficmanager.net/apis/v3/conversations/12345/activities 
Authorization: Bearer eyJhbGciOiJIUzI1Ni...
    
(JSON-serialized Activity message goes here)
```

> [!IMPORTANT]
> <span data-ttu-id="65f7d-155">Especifique el token JWT solo en el encabezado `Authorization` de las solicitudes que envíe al servicio Bot Connector.</span><span class="sxs-lookup"><span data-stu-id="65f7d-155">Only specify the JWT token in the `Authorization` header of requests you send to the Bot Connector service.</span></span> <span data-ttu-id="65f7d-156">No envíe el token por canales no seguros y no lo incluya en las solicitudes HTTP que se envían a otros servicios.</span><span class="sxs-lookup"><span data-stu-id="65f7d-156">Do NOT send the token over unsecured channels and do NOT include it in HTTP requests that you send to other services.</span></span> <span data-ttu-id="65f7d-157">El token JWT que obtiene del servicio de inicio de sesión de MSA o AAD v2 es como una contraseña y debe utilizarse con mucho cuidado.</span><span class="sxs-lookup"><span data-stu-id="65f7d-157">The JWT token that you obtain from the MSA/AAD v2 login service is like a password and should be handled with great care.</span></span> <span data-ttu-id="65f7d-158">Cualquiera que tenga el token puede usarlo para realizar operaciones en nombre del bot.</span><span class="sxs-lookup"><span data-stu-id="65f7d-158">Anyone that possesses the token may use it to perform operations on behalf of your bot.</span></span> 

#### <a name="bot-to-connector-example-jwt-components"></a><span data-ttu-id="65f7d-159">Bot a Connector: componentes JWT de ejemplo</span><span class="sxs-lookup"><span data-stu-id="65f7d-159">Bot to Connector: example JWT components</span></span>

```json
header:
{
  typ: "JWT",
  alg: "RS256",
  x5t: "<SIGNING KEY ID>",
  kid: "<SIGNING KEY ID>"
},
payload:
{
  aud: "https://api.botframework.com",
  iss: "https://sts.windows.net/d6d49420-f39b-4df7-a1dc-d59a935871db/",
  nbf: 1481049243,
  exp: 1481053143,
  appid: "<YOUR MICROSOFT APP ID>",
  ... other fields follow
}
```

> [!NOTE]
> <span data-ttu-id="65f7d-160">Los campos reales pueden variar en la práctica.</span><span class="sxs-lookup"><span data-stu-id="65f7d-160">Actual fields may vary in practice.</span></span> <span data-ttu-id="65f7d-161">Debe crear y validar todos los tokens JWT según lo especificado anteriormente.</span><span class="sxs-lookup"><span data-stu-id="65f7d-161">Create and validate all JWT tokens as specified above.</span></span>

## <a id="connector-to-bot"></a> <span data-ttu-id="65f7d-162">Autenticación de las solicitudes del servicio Bot Connector al bot</span><span class="sxs-lookup"><span data-stu-id="65f7d-162">Authenticate requests from the Bot Connector service to your bot</span></span>

<span data-ttu-id="65f7d-163">Cuando el servicio Bot Connector envía una solicitud al bot, especifica un token JWT firmado en el encabezado `Authorization` de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="65f7d-163">When the Bot Connector service sends a request to your bot, it specifies a signed JWT token in the `Authorization` header of the request.</span></span> <span data-ttu-id="65f7d-164">El bot puede autenticar las llamadas desde el servicio Bot Connector al verificar la autenticidad del token JWT firmado.</span><span class="sxs-lookup"><span data-stu-id="65f7d-164">Your bot can authenticate calls from the Bot Connector service by verifying the authenticity of the signed JWT token.</span></span> 

<span data-ttu-id="65f7d-165">Este diagrama muestra los pasos de la autenticación de Connector a bot:</span><span class="sxs-lookup"><span data-stu-id="65f7d-165">This diagram shows the steps for connector-to-bot authentication:</span></span>

![Autenticación de las llamadas desde Bot Connector al bot](../media/connector/auth_bot_connector_to_bot.png)

### <a id="openid-metadata-document"></a> <span data-ttu-id="65f7d-167">Paso 2: Obtención del documento de metadatos de OpenID</span><span class="sxs-lookup"><span data-stu-id="65f7d-167">Step 2: Get the OpenID metadata document</span></span>

<span data-ttu-id="65f7d-168">El documento de metadatos de OpenID especifica la ubicación de un segundo documento que enumera las claves de firma válidas del servicio Bot Connector.</span><span class="sxs-lookup"><span data-stu-id="65f7d-168">The OpenID metadata document specifies the location of a second document that lists the Bot Connector service's valid signing keys.</span></span> <span data-ttu-id="65f7d-169">Para obtener el documento de metadatos de OpenID, emita esta solicitud mediante HTTPS:</span><span class="sxs-lookup"><span data-stu-id="65f7d-169">To get the OpenID metadata document, issue this request via HTTPS:</span></span>

```http
GET https://login.botframework.com/v1/.well-known/openidconfiguration
```

> [!TIP]
> <span data-ttu-id="65f7d-170">Se trata de una dirección URL estática que se puede insertar directamente en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="65f7d-170">This is a static URL that you can hardcode into your application.</span></span> 

<span data-ttu-id="65f7d-171">El ejemplo siguiente muestra un documento de metadatos de OpenID devuelto en respuesta a la solicitud `GET`.</span><span class="sxs-lookup"><span data-stu-id="65f7d-171">The following example shows an OpenID metadata document that is returned in response to the `GET` request.</span></span> <span data-ttu-id="65f7d-172">La propiedad `jwks_uri` especifica la ubicación del documento que contiene las claves de firma válidas del servicio Bot Connector.</span><span class="sxs-lookup"><span data-stu-id="65f7d-172">The `jwks_uri` property specifies the location of the document that contains the Bot Connector service's valid signing keys.</span></span>

```json
{
    "issuer": "https://api.botframework.com",
    "authorization_endpoint": "https://invalid.botframework.com",
    "jwks_uri": "https://login.botframework.com/v1/.well-known/keys",
    "id_token_signing_alg_values_supported": [
      "RS256"
    ],
    "token_endpoint_auth_methods_supported": [
      "private_key_jwt"
    ]
}
```

### <a id="connector-to-bot-step-3"></a> <span data-ttu-id="65f7d-173">Paso 3: Obtención de la lista de claves de firma válidas</span><span class="sxs-lookup"><span data-stu-id="65f7d-173">Step 3: Get the list of valid signing keys</span></span>

<span data-ttu-id="65f7d-174">Para obtener la lista de claves de firma válidas, emita una solicitud `GET` mediante HTTPS a la dirección URL especificada por la propiedad `jwks_uri` en el documento de metadatos de OpenID.</span><span class="sxs-lookup"><span data-stu-id="65f7d-174">To get the list of valid signing keys, issue a `GET` request via HTTPS to the URL specified by the `jwks_uri` property in the OpenID metadata document.</span></span> <span data-ttu-id="65f7d-175">Por ejemplo: </span><span class="sxs-lookup"><span data-stu-id="65f7d-175">For example:</span></span>

```http
GET https://login.botframework.com/v1/.well-known/keys
```

<span data-ttu-id="65f7d-176">El cuerpo de la respuesta especifica el documento en [formato JWK](https://tools.ietf.org/html/rfc7517), pero también incluye una propiedad adicional para cada clave: `endorsements`.</span><span class="sxs-lookup"><span data-stu-id="65f7d-176">The response body specifies the document in the [JWK format](https://tools.ietf.org/html/rfc7517) but also includes an additional property for each key: `endorsements`.</span></span> <span data-ttu-id="65f7d-177">La lista de claves es relativamente estable y se puede almacenar en caché durante largos períodos de tiempo (de forma predeterminada, 5 días en Bot Builder SDK).</span><span class="sxs-lookup"><span data-stu-id="65f7d-177">The list of keys is relatively stable and may be cached for long periods of time (by default, 5 days within the Bot Builder SDK).</span></span>

<span data-ttu-id="65f7d-178">La propiedad `endorsements` dentro de cada clave contiene una o más cadenas de aprobación que puede usar para comprobar que el identificador de canal especificado en la propiedad `channelId` dentro del objeto [Activity][Activity] de la solicitud entrante es auténtico.</span><span class="sxs-lookup"><span data-stu-id="65f7d-178">The `endorsements` property within each key contains one or more endorsement strings which you can use to verify that the channel ID specified in the `channelId` property within the [Activity][Activity] object of the incoming request is authentic.</span></span> <span data-ttu-id="65f7d-179">La lista de identificadores de canal que requieren aprobaciones es configurable en cada bot.</span><span class="sxs-lookup"><span data-stu-id="65f7d-179">The list of channel IDs that require endorsements is configurable within each bot.</span></span> <span data-ttu-id="65f7d-180">De forma predeterminada, será la lista de todos los identificadores de canal publicados, aunque los desarrolladores de bots pueden invalidar los valores de identificador de canal seleccionados en cualquier caso.</span><span class="sxs-lookup"><span data-stu-id="65f7d-180">By default, it will be the list of all published channel IDs, although bot developers may override selected channel ID values either way.</span></span> <span data-ttu-id="65f7d-181">Si se requiere aprobación para un identificador de canal:</span><span class="sxs-lookup"><span data-stu-id="65f7d-181">If endorsement for a channel ID is required:</span></span>

- <span data-ttu-id="65f7d-182">Debe requerir que cualquier objeto [Activity][Activity] enviado al bot con ese identificador de canal venga acompañado de un token JWT firmado con una aprobación para ese canal.</span><span class="sxs-lookup"><span data-stu-id="65f7d-182">You should require that any [Activity][Activity] object sent to your bot with that channel ID is accompanied by a JWT token that is signed with an endorsement for that channel.</span></span> 
- <span data-ttu-id="65f7d-183">Si la aprobación no está presente, el bot debe rechazar la solicitud devolviendo un código de estado **HTTP 403 (prohibido)**.</span><span class="sxs-lookup"><span data-stu-id="65f7d-183">If the endorsement is not present, your bot should reject the request by returning an **HTTP 403 (Forbidden)** status code.</span></span>

### <a name="step-4-verify-the-jwt-token"></a><span data-ttu-id="65f7d-184">Paso 4: Verificación del token JWT</span><span class="sxs-lookup"><span data-stu-id="65f7d-184">Step 4: Verify the JWT token</span></span>

<span data-ttu-id="65f7d-185">Para verificar la autenticidad del token enviado por el servicio Bot Connector, debe extraer el token del encabezado `Authorization` de la solicitud, analizar el token, verificar su contenido y verificar su firma.</span><span class="sxs-lookup"><span data-stu-id="65f7d-185">To verify the authenticity of the token that was sent by the Bot Connector service, you must extract the token from the `Authorization` header of the request, parse the token, verify its contents, and verify its signature.</span></span> 

<span data-ttu-id="65f7d-186">Hay disponibles bibliotecas de análisis de JWT para muchas plataformas y la mayoría implementa un análisis de los tokens JWT seguro y confiable, aunque normalmente deberá configurar estas bibliotecas para requerir que determinadas características del token (su emisor, audiencia, etc.) contengan valores correctos.</span><span class="sxs-lookup"><span data-stu-id="65f7d-186">JWT parsing libraries are available for many platforms and most implement secure and reliable parsing for JWT tokens, although you must typically configure these libraries to require that certain characteristics of the token (its issuer, audience, etc.) contain correct values.</span></span> <span data-ttu-id="65f7d-187">Al analizar el token, debe configurar la biblioteca de análisis o escribir su propia validación para asegurarse de que el token cumple estos requisitos:</span><span class="sxs-lookup"><span data-stu-id="65f7d-187">When parsing the token, you must configure the parsing library or write your own validation to ensure the token meets these requirements:</span></span>

1. <span data-ttu-id="65f7d-188">El token se ha enviado en el encabezado HTTP `Authorization` con un esquema "Bearer" (Portador).</span><span class="sxs-lookup"><span data-stu-id="65f7d-188">The token was sent in the HTTP `Authorization` header with "Bearer" scheme.</span></span>
2. <span data-ttu-id="65f7d-189">El token tiene un formato JSON válido que se ajusta al [estándar JWT](http://openid.net/specs/draft-jones-json-web-token-07.html).</span><span class="sxs-lookup"><span data-stu-id="65f7d-189">The token is valid JSON that conforms to the [JWT standard](http://openid.net/specs/draft-jones-json-web-token-07.html).</span></span>
3. <span data-ttu-id="65f7d-190">El token contiene una notificación "issuer" (emisor) con el valor `https://api.botframework.com`.</span><span class="sxs-lookup"><span data-stu-id="65f7d-190">The token contains an "issuer" claim with value of `https://api.botframework.com`.</span></span>
4. <span data-ttu-id="65f7d-191">El token contiene una notificación "audience" (audiencia) con un valor igual al identificador de aplicación de Microsoft del bot.</span><span class="sxs-lookup"><span data-stu-id="65f7d-191">The token contains an "audience" claim with a value equal to the bot's Microsoft App ID.</span></span>
5. <span data-ttu-id="65f7d-192">El token está dentro de su período de validez.</span><span class="sxs-lookup"><span data-stu-id="65f7d-192">The token is within its validity period.</span></span> <span data-ttu-id="65f7d-193">El tiempo estándar del sector es de 5 minutos.</span><span class="sxs-lookup"><span data-stu-id="65f7d-193">Industry-standard clock-skew is 5 minutes.</span></span>
6. <span data-ttu-id="65f7d-194">El token tiene una firma criptográfica válida, con una clave enumerada en el documento de claves de OpenID que se recuperó en el [Paso 3](#connector-to-bot-step-3), utilizando el algoritmo de firma que se especifica en la propiedad `id_token_signing_alg_values_supported` del documento de metadatos de OpenID que se recuperó en [Paso 2](#openid-metadata-document).</span><span class="sxs-lookup"><span data-stu-id="65f7d-194">The token has a valid cryptographic signature, with a key listed in the OpenID keys document that was retrieved in [Step 3](#connector-to-bot-step-3), using the signing algorithm that is specified in the `id_token_signing_alg_values_supported` property of the Open ID Metadata document that was retrieved in [Step 2](#openid-metadata-document).</span></span>
7. <span data-ttu-id="65f7d-195">El token contiene una notificación "serviceUrl" (dirección URL de servicio) cuyo valor coincide con la propiedad `servieUrl` en la raíz del objeto [Activity][Activity] de la solicitud entrante.</span><span class="sxs-lookup"><span data-stu-id="65f7d-195">The token contains a "serviceUrl" claim with value that matches the `servieUrl` property at the root of the [Activity][Activity] object of the incoming request.</span></span> 

<span data-ttu-id="65f7d-196">Si el token no cumple todos estos requisitos, el bot debe rechazar la solicitud devolviendo un código de estado **HTTP 403 (prohibido)**.</span><span class="sxs-lookup"><span data-stu-id="65f7d-196">If the token does not meet all of these requirements, your bot should reject the request by returning an **HTTP 403 (Forbidden)** status code.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="65f7d-197">Todos estos requisitos son importantes, especialmente los requisitos 4 y 6.</span><span class="sxs-lookup"><span data-stu-id="65f7d-197">All of these requirements are important, particularly requirements 4 and 6.</span></span> <span data-ttu-id="65f7d-198">En caso de no implementarse todos estos requisitos de comprobación, se dejaría el bot abierto a ataques que podrían provocar que el bot divulgue su token JWT.</span><span class="sxs-lookup"><span data-stu-id="65f7d-198">Failure to implement ALL of these verification requirements will leave the bot open to attacks which could cause the bot to divulge its JWT token.</span></span>

<span data-ttu-id="65f7d-199">Los implementadores no deben exponer una manera de deshabilitar la validación del token JWT que se envía al bot.</span><span class="sxs-lookup"><span data-stu-id="65f7d-199">Implementers should not expose a way to disable validation of the JWT token that is sent to the bot.</span></span>

#### <a name="connector-to-bot-example-jwt-components"></a><span data-ttu-id="65f7d-200">Connector a bot: componentes JWT de ejemplo</span><span class="sxs-lookup"><span data-stu-id="65f7d-200">Connector to Bot: example JWT components</span></span>

```json
header:
{
  typ: "JWT",
  alg: "RS256",
  x5t: "<SIGNING KEY ID>",
  kid: "<SIGNING KEY ID>"
},
payload:
{
  aud: "<YOU MICROSOFT APP ID>",
  iss: "https://api.botframework.com",
  nbf: 1481049243,
  exp: 1481053143,
  ... other fields follow
}
```

> [!NOTE]
> <span data-ttu-id="65f7d-201">Los campos reales pueden variar en la práctica.</span><span class="sxs-lookup"><span data-stu-id="65f7d-201">Actual fields may vary in practice.</span></span> <span data-ttu-id="65f7d-202">Debe crear y validar todos los tokens JWT según lo especificado anteriormente.</span><span class="sxs-lookup"><span data-stu-id="65f7d-202">Create and validate all JWT tokens as specified above.</span></span>

## <a id="emulator-to-bot"></a> <span data-ttu-id="65f7d-203">Autenticación de las solicitudes del emulador de Bot Framework al bot</span><span class="sxs-lookup"><span data-stu-id="65f7d-203">Authenticate requests from the Bot Framework Emulator to your bot</span></span>

> [!WARNING]
> <span data-ttu-id="65f7d-204">A finales de otoño de 2017, se presenta la versión v3.2 del protocolo de seguridad de Bot Framework.</span><span class="sxs-lookup"><span data-stu-id="65f7d-204">In late fall of 2017, v3.2 of the Bot Framework security protocol will be introduced.</span></span> <span data-ttu-id="65f7d-205">Esta nueva versión incluye un nuevo valor "issuer" (emisor) dentro de los tokens que se intercambian entre el emulador de Bot Framework y el bot.</span><span class="sxs-lookup"><span data-stu-id="65f7d-205">This new version includes a new "issuer" value within tokens that are exchanged between the Bot Framework Eumaltor and your bot.</span></span> <span data-ttu-id="65f7d-206">Para prepararse para este cambio, los pasos siguientes describen cómo comprobar los valores de emisor de las versiones v3.1 y v3.2.</span><span class="sxs-lookup"><span data-stu-id="65f7d-206">To prepare for this change, the below steps outline how to check for both the v3.1 and v3.2 issuer values.</span></span> 

<span data-ttu-id="65f7d-207">El [Emulador de Bot Framework](../bot-service-debug-emulator.md) es una herramienta de escritorio que puede usar para probar la funcionalidad del bot.</span><span class="sxs-lookup"><span data-stu-id="65f7d-207">The [Bot Framework Emulator](../bot-service-debug-emulator.md) is a desktop tool that you can use to test the functionality of your bot.</span></span> <span data-ttu-id="65f7d-208">Aunque el emulador de Bot Framework usa las mismas [tecnologías de autenticación](#authentication-technologies) que se describieron anteriormente, no puede suplantar al servicio Bot Connector real.</span><span class="sxs-lookup"><span data-stu-id="65f7d-208">Although the Bot Framework Emulator uses the same [authentication technologies](#authentication-technologies) as described above, it is unable to impersonate the real Bot Connector service.</span></span> <span data-ttu-id="65f7d-209">En su lugar, usa el identificador de aplicación de Microsoft y la contraseña de aplicación de Microsoft que se especifican al conectar el emulador al bot para crear tokens que son idénticos a los que crea el bot.</span><span class="sxs-lookup"><span data-stu-id="65f7d-209">Instead, it uses the Microsoft App ID and Microsoft App Password that you specify when you connect the emulator to your bot to create tokens that are identical to those that the bot creates.</span></span> <span data-ttu-id="65f7d-210">Cuando el emulador envía una solicitud al bot, especifica el token JWT en el encabezado `Authorization` de la solicitud: en esencia, utiliza las propias credenciales del bot para autenticar la solicitud.</span><span class="sxs-lookup"><span data-stu-id="65f7d-210">When the emulator sends a request to your bot, it specifies the JWT token in the `Authorization` header of the request -- in essence, using the bot's own credentials to authenticate the request.</span></span> 

<span data-ttu-id="65f7d-211">Si está implementando una biblioteca de autenticación y desea aceptar solicitudes desde el emulador de Bot Framework, debe agregar esta ruta de acceso de comprobación adicional.</span><span class="sxs-lookup"><span data-stu-id="65f7d-211">If you are implementing an authentication library and want to accept requests from the Bot Framework Emulator, you must add this additional verification path.</span></span> <span data-ttu-id="65f7d-212">La ruta de acceso es estructuralmente similar a la ruta de acceso de comprobación [Connector -> Bot](#connector-to-bot), pero usa el documento de OpenID de MSA en lugar del documento de OpenID de Bot Connector.</span><span class="sxs-lookup"><span data-stu-id="65f7d-212">The path is structurally similar to the [Connector -> Bot](#connector-to-bot) verification path, but it uses MSA’s OpenID document instead of the Bot Connector’s OpenID document.</span></span>

<span data-ttu-id="65f7d-213">Este diagrama muestra los pasos de la autenticación de emulador a bot:</span><span class="sxs-lookup"><span data-stu-id="65f7d-213">This diagram shows the steps for emulator-to-bot authentication:</span></span>

![Autenticación de las llamadas del emulador de Bot Framework al bot](../media/connector/auth_bot_framework_emulator_to_bot.png)

---
### <a name="step-2-get-the-msa-openid-metadata-document"></a><span data-ttu-id="65f7d-215">Paso 2: Obtención del documento de metadatos de OpenID de MSA</span><span class="sxs-lookup"><span data-stu-id="65f7d-215">Step 2: Get the MSA OpenID metadata document</span></span>

<span data-ttu-id="65f7d-216">El documento de metadatos de OpenID especifica la ubicación de un segundo documento que enumera las claves de firma válidas.</span><span class="sxs-lookup"><span data-stu-id="65f7d-216">The OpenID metadata document specifies the location of a second document that lists the valid signing keys.</span></span> <span data-ttu-id="65f7d-217">Para obtener el documento de metadatos de OpenID de MSA, emita esta solicitud mediante HTTPS:</span><span class="sxs-lookup"><span data-stu-id="65f7d-217">To get the MSA OpenID metadata document, issue this request via HTTPS:</span></span>

```http
GET https://login.microsoftonline.com/botframework.com/v2.0/.well-known/openid-configuration
```

<span data-ttu-id="65f7d-218">El ejemplo siguiente muestra un documento de metadatos de OpenID devuelto en respuesta a la solicitud `GET`.</span><span class="sxs-lookup"><span data-stu-id="65f7d-218">The following example shows an OpenID metadata document that is returned in response to the `GET` request.</span></span> <span data-ttu-id="65f7d-219">La propiedad `jwks_uri` especifica la ubicación del documento que contiene las claves de firma válidas.</span><span class="sxs-lookup"><span data-stu-id="65f7d-219">The `jwks_uri` property specifies the location of the document that contains the valid signing keys.</span></span>

```json
{
    "authorization_endpoint":"https://login.microsoftonline.com/common/oauth2/v2.0/authorize",
    "token_endpoint":"https://login.microsoftonline.com/common/oauth2/v2.0/token",
    "token_endpoint_auth_methods_supported":["client_secret_post","private_key_jwt"],
    "jwks_uri":"https://login.microsoftonline.com/common/discovery/v2.0/keys",
    ...
}
```

### <a id="emulator-to-bot-step-3"></a> <span data-ttu-id="65f7d-220">Paso 3: Obtención de la lista de claves de firma válidas</span><span class="sxs-lookup"><span data-stu-id="65f7d-220">Step 3: Get the list of valid signing keys</span></span>

<span data-ttu-id="65f7d-221">Para obtener la lista de claves de firma válidas, emita una solicitud `GET` mediante HTTPS a la dirección URL especificada por la propiedad `jwks_uri` en el documento de metadatos de OpenID.</span><span class="sxs-lookup"><span data-stu-id="65f7d-221">To get the list of valid signing keys, issue a `GET` request via HTTPS to the URL specified by the `jwks_uri` property in the OpenID metadata document.</span></span> <span data-ttu-id="65f7d-222">Por ejemplo: </span><span class="sxs-lookup"><span data-stu-id="65f7d-222">For example:</span></span>

```http
GET https://login.microsoftonline.com/common/discovery/v2.0/keys 
Host: login.microsoftonline.com
```

<span data-ttu-id="65f7d-223">El cuerpo de la respuesta especifica el documento en [formato JWK](https://tools.ietf.org/html/rfc7517).</span><span class="sxs-lookup"><span data-stu-id="65f7d-223">The response body specifies the document in the [JWK format](https://tools.ietf.org/html/rfc7517).</span></span> 

### <a name="step-4-verify-the-jwt-token"></a><span data-ttu-id="65f7d-224">Paso 4: Verificación del token JWT</span><span class="sxs-lookup"><span data-stu-id="65f7d-224">Step 4: Verify the JWT token</span></span>

<span data-ttu-id="65f7d-225">Para verificar la autenticidad del token enviado por el emulador, debe extraer el token del encabezado `Authorization` de la solicitud, analizar el token, verificar su contenido y verificar su firma.</span><span class="sxs-lookup"><span data-stu-id="65f7d-225">To verify the authenticity of the token that was sent by the emulator, you must extract the token from the `Authorization` header of the request, parse the token, verify its contents, and verify its signature.</span></span> 

<span data-ttu-id="65f7d-226">Hay disponibles bibliotecas de análisis de JWT para muchas plataformas y la mayoría implementa un análisis de los tokens JWT seguro y confiable, aunque normalmente deberá configurar estas bibliotecas para requerir que determinadas características del token (su emisor, audiencia, etc.) contengan valores correctos.</span><span class="sxs-lookup"><span data-stu-id="65f7d-226">JWT parsing libraries are available for many platforms and most implement secure and reliable parsing for JWT tokens, although you must typically configure these libraries to require that certain characteristics of the token (its issuer, audience, etc.) contain correct values.</span></span> <span data-ttu-id="65f7d-227">Al analizar el token, debe configurar la biblioteca de análisis o escribir su propia validación para asegurarse de que el token cumple estos requisitos:</span><span class="sxs-lookup"><span data-stu-id="65f7d-227">When parsing the token, you must configure the parsing library or write your own validation to ensure the token meets these requirements:</span></span>

1. <span data-ttu-id="65f7d-228">El token se ha enviado en el encabezado HTTP `Authorization` con un esquema "Bearer" (Portador).</span><span class="sxs-lookup"><span data-stu-id="65f7d-228">The token was sent in the HTTP `Authorization` header with "Bearer" scheme.</span></span>
2. <span data-ttu-id="65f7d-229">El token tiene un formato JSON válido que se ajusta al [estándar JWT](http://openid.net/specs/draft-jones-json-web-token-07.html).</span><span class="sxs-lookup"><span data-stu-id="65f7d-229">The token is valid JSON that conforms to the [JWT standard](http://openid.net/specs/draft-jones-json-web-token-07.html).</span></span>
3. <span data-ttu-id="65f7d-230">El token contiene una notificación "issuer" (emisor) con el valor `https://sts.windows.net/d6d49420-f39b-4df7-a1dc-d59a935871db/` o `https://sts.windows.net/f8cdef31-a31e-4b4a-93e4-5f571e91255a/`.</span><span class="sxs-lookup"><span data-stu-id="65f7d-230">The token contains an "issuer" claim with value of `https://sts.windows.net/d6d49420-f39b-4df7-a1dc-d59a935871db/` or `https://sts.windows.net/f8cdef31-a31e-4b4a-93e4-5f571e91255a/`.</span></span> <span data-ttu-id="65f7d-231">(La comprobación de ambos valores de emisor garantizará que está comprobando tanto los valores de emisor del protocolo de seguridad v3.1 como los del v3.2)</span><span class="sxs-lookup"><span data-stu-id="65f7d-231">(Checking for both issuer values will ensure you are checking for both the security protocol v3.1 and v3.2 issuer values)</span></span>
4. <span data-ttu-id="65f7d-232">El token contiene una notificación "audience" (audiencia) con un valor igual al identificador de aplicación de Microsoft del bot.</span><span class="sxs-lookup"><span data-stu-id="65f7d-232">The token contains an "audience" claim with a value equal to the bot's Microsoft App ID.</span></span>
5. <span data-ttu-id="65f7d-233">El token contiene una notificación "appid" (identificador de aplicación) con un valor igual al identificador de aplicación de Microsoft del bot.</span><span class="sxs-lookup"><span data-stu-id="65f7d-233">The token contains an "appid" claim with the value equal to the bot's Microsoft App ID.</span></span>
6. <span data-ttu-id="65f7d-234">El token está dentro de su período de validez.</span><span class="sxs-lookup"><span data-stu-id="65f7d-234">The token is within its validity period.</span></span> <span data-ttu-id="65f7d-235">El tiempo estándar del sector es de 5 minutos.</span><span class="sxs-lookup"><span data-stu-id="65f7d-235">Industry-standard clock-skew is 5 minutes.</span></span>
7. <span data-ttu-id="65f7d-236">El token tiene una firma criptográfica válida con una clave enumerada en el documento de claves de OpenID que se recuperó en el [paso 3](#emulator-to-bot-step-3).</span><span class="sxs-lookup"><span data-stu-id="65f7d-236">The token has a valid cryptographic signature with a key listed in the OpenID keys document that was retrieved in [Step 3](#emulator-to-bot-step-3).</span></span>

> [!NOTE]
> <span data-ttu-id="65f7d-237">El requisito 5 es específico para la ruta de acceso de comprobación del emulador.</span><span class="sxs-lookup"><span data-stu-id="65f7d-237">Requirement 5 is a specific to the emulator verification path.</span></span> 

<span data-ttu-id="65f7d-238">Si el token no cumple todos estos requisitos, el bot debe finalizar la solicitud devolviendo un código de estado **HTTP 403 (prohibido)**.</span><span class="sxs-lookup"><span data-stu-id="65f7d-238">If the token does not meet all of these requirements, your bot should terminate the request by returning an **HTTP 403 (Forbidden)** status code.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="65f7d-239">Todos estos requisitos son importantes, especialmente los requisitos 4 y 7.</span><span class="sxs-lookup"><span data-stu-id="65f7d-239">All of these requirements are important, particularly requirements 4 and 7.</span></span> <span data-ttu-id="65f7d-240">En caso de no implementarse todos estos requisitos de comprobación, se dejaría el bot abierto a ataques que podrían provocar que el bot divulgue su token JWT.</span><span class="sxs-lookup"><span data-stu-id="65f7d-240">Failure to implement ALL of these verification requirements will leave the bot open to attacks which could cause the bot to divulge its JWT token.</span></span>

#### <a name="emulator-to-bot-example-jwt-components"></a><span data-ttu-id="65f7d-241">Emulador a bot: componentes JWT de ejemplo</span><span class="sxs-lookup"><span data-stu-id="65f7d-241">Emulator to Bot: example JWT components</span></span>

```json
header:
{
  typ: "JWT",
  alg: "RS256",
  x5t: "<SIGNING KEY ID>",
  kid: "<SIGNING KEY ID>"
},
payload:
{
  aud: "<YOUR MICROSOFT APP ID>",
  iss: "https://sts.windows.net/d6d49420-f39b-4df7-a1dc-d59a935871db/",
  nbf: 1481049243,
  exp: 1481053143,
  ... other fields follow
}
```

> [!NOTE]
> <span data-ttu-id="65f7d-242">Los campos reales pueden variar en la práctica.</span><span class="sxs-lookup"><span data-stu-id="65f7d-242">Actual fields may vary in practice.</span></span> <span data-ttu-id="65f7d-243">Debe crear y validar todos los tokens JWT según lo especificado anteriormente.</span><span class="sxs-lookup"><span data-stu-id="65f7d-243">Create and validate all JWT tokens as specified above.</span></span>

## <a name="security-protocol-changes"></a><span data-ttu-id="65f7d-244">Cambios del protocolo de seguridad</span><span class="sxs-lookup"><span data-stu-id="65f7d-244">Security protocol changes</span></span>

> [!WARNING]
> <span data-ttu-id="65f7d-245">La compatibilidad con la versión v3.0 del protocolo de seguridad se interrumpió el **31 de julio de 2017**.</span><span class="sxs-lookup"><span data-stu-id="65f7d-245">Support for v3.0 of the security protocol was discontinued on **July 31, 2017**.</span></span> <span data-ttu-id="65f7d-246">Si ha escrito su propio código de autenticación (es decir, no ha usado Bot Builder SDK para crear el bot), debe actualizar a la versión v3.1 del protocolo de seguridad mediante la actualización de la aplicación para que utilice los valores de la versión v3.1 que se enumeran a continuación.</span><span class="sxs-lookup"><span data-stu-id="65f7d-246">If you have written your own authentication code (i.e., did not use the Bot Builder SDK to create your bot), you must upgrade to v3.1 of the security protocol by updating your application to use the v3.1 values that are listed below.</span></span> 

### <a name="bot-to-connector-authenticationbot-to-connector"></a>[<span data-ttu-id="65f7d-247">Autenticación bot a Connector</span><span class="sxs-lookup"><span data-stu-id="65f7d-247">Bot to Connector authentication</span></span>](#bot-to-connector)

#### <a name="oauth-login-url"></a><span data-ttu-id="65f7d-248">Dirección URL de inicio de sesión de OAuth</span><span class="sxs-lookup"><span data-stu-id="65f7d-248">OAuth login URL</span></span>

| <span data-ttu-id="65f7d-249">Versión del protocolo</span><span class="sxs-lookup"><span data-stu-id="65f7d-249">Protocol version</span></span> | <span data-ttu-id="65f7d-250">Valor válido</span><span class="sxs-lookup"><span data-stu-id="65f7d-250">Valid value</span></span> |
|----|----|
| <span data-ttu-id="65f7d-251">v3.1 y v3.2</span><span class="sxs-lookup"><span data-stu-id="65f7d-251">v3.1 & v3.2</span></span> | `https://login.microsoftonline.com/botframework.com/oauth2/v2.0/token` |

#### <a name="oauth-scope"></a><span data-ttu-id="65f7d-252">Ámbito de OAuth</span><span class="sxs-lookup"><span data-stu-id="65f7d-252">OAuth scope</span></span>

| <span data-ttu-id="65f7d-253">Versión del protocolo</span><span class="sxs-lookup"><span data-stu-id="65f7d-253">Protocol version</span></span> | <span data-ttu-id="65f7d-254">Valor válido</span><span class="sxs-lookup"><span data-stu-id="65f7d-254">Valid value</span></span> |
|----|----|
| <span data-ttu-id="65f7d-255">v3.1 y v3.2</span><span class="sxs-lookup"><span data-stu-id="65f7d-255">v3.1 & v3.2</span></span> |  `https://api.botframework.com/.default` |

### <a name="connector-to-bot-authenticationconnector-to-bot"></a>[<span data-ttu-id="65f7d-256">Autenticación de Connector a bot</span><span class="sxs-lookup"><span data-stu-id="65f7d-256">Connector to Bot authentication</span></span>](#connector-to-bot)

#### <a name="openid-metadata-document"></a><span data-ttu-id="65f7d-257">Documento de metadatos de OpenID</span><span class="sxs-lookup"><span data-stu-id="65f7d-257">OpenID metadata document</span></span>

| <span data-ttu-id="65f7d-258">Versión del protocolo</span><span class="sxs-lookup"><span data-stu-id="65f7d-258">Protocol version</span></span> | <span data-ttu-id="65f7d-259">Valor válido</span><span class="sxs-lookup"><span data-stu-id="65f7d-259">Valid value</span></span> |
|----|----|
| <span data-ttu-id="65f7d-260">v3.1 y v3.2</span><span class="sxs-lookup"><span data-stu-id="65f7d-260">v3.1 & v3.2</span></span> | `https://login.botframework.com/v1/.well-known/openidconfiguration` |

#### <a name="jwt-issuer"></a><span data-ttu-id="65f7d-261">Emisor de JWT</span><span class="sxs-lookup"><span data-stu-id="65f7d-261">JWT Issuer</span></span>

| <span data-ttu-id="65f7d-262">Versión del protocolo</span><span class="sxs-lookup"><span data-stu-id="65f7d-262">Protocol version</span></span> | <span data-ttu-id="65f7d-263">Valor válido</span><span class="sxs-lookup"><span data-stu-id="65f7d-263">Valid value</span></span> |
|----|----|
| <span data-ttu-id="65f7d-264">v3.1 y v3.2</span><span class="sxs-lookup"><span data-stu-id="65f7d-264">v3.1 & v3.2</span></span> | `https://api.botframework.com` |

### <a name="emulator-to-bot-authenticationemulator-to-bot"></a>[<span data-ttu-id="65f7d-265">Autenticación de emulador a bot</span><span class="sxs-lookup"><span data-stu-id="65f7d-265">Emulator to Bot authentication</span></span>](#emulator-to-bot)

#### <a name="oauth-login-url"></a><span data-ttu-id="65f7d-266">Dirección URL de inicio de sesión de OAuth</span><span class="sxs-lookup"><span data-stu-id="65f7d-266">OAuth login URL</span></span>

| <span data-ttu-id="65f7d-267">Versión del protocolo</span><span class="sxs-lookup"><span data-stu-id="65f7d-267">Protocol version</span></span> | <span data-ttu-id="65f7d-268">Valor válido</span><span class="sxs-lookup"><span data-stu-id="65f7d-268">Valid value</span></span> |
|----|----|
| <span data-ttu-id="65f7d-269">v3.1 y v3.2</span><span class="sxs-lookup"><span data-stu-id="65f7d-269">v3.1 & v3.2</span></span> | `https://login.microsoftonline.com/botframework.com/oauth2/v2.0/token` |

#### <a name="oauth-scope"></a><span data-ttu-id="65f7d-270">Ámbito de OAuth</span><span class="sxs-lookup"><span data-stu-id="65f7d-270">OAuth scope</span></span>

| <span data-ttu-id="65f7d-271">Versión del protocolo</span><span class="sxs-lookup"><span data-stu-id="65f7d-271">Protocol version</span></span> | <span data-ttu-id="65f7d-272">Valor válido</span><span class="sxs-lookup"><span data-stu-id="65f7d-272">Valid value</span></span> |
|----|----|
| <span data-ttu-id="65f7d-273">v3.1 y v3.2</span><span class="sxs-lookup"><span data-stu-id="65f7d-273">v3.1 & v3.2</span></span> |  <span data-ttu-id="65f7d-274">Identificador de aplicación de Microsoft del bot + `/.default`</span><span class="sxs-lookup"><span data-stu-id="65f7d-274">Your bot’s Microsoft App ID + `/.default`</span></span> |

#### <a name="jwt-audience"></a><span data-ttu-id="65f7d-275">Audiencia de JWT</span><span class="sxs-lookup"><span data-stu-id="65f7d-275">JWT Audience</span></span>

| <span data-ttu-id="65f7d-276">Versión del protocolo</span><span class="sxs-lookup"><span data-stu-id="65f7d-276">Protocol version</span></span> | <span data-ttu-id="65f7d-277">Valor válido</span><span class="sxs-lookup"><span data-stu-id="65f7d-277">Valid value</span></span> |
|----|----|
| <span data-ttu-id="65f7d-278">v3.1 y v3.2</span><span class="sxs-lookup"><span data-stu-id="65f7d-278">v3.1 & v3.2</span></span> | <span data-ttu-id="65f7d-279">Identificador de aplicación de Microsoft del bot</span><span class="sxs-lookup"><span data-stu-id="65f7d-279">Your bot’s Microsoft App ID</span></span> |

#### <a name="jwt-issuer"></a><span data-ttu-id="65f7d-280">Emisor de JWT</span><span class="sxs-lookup"><span data-stu-id="65f7d-280">JWT Issuer</span></span>

| <span data-ttu-id="65f7d-281">Versión del protocolo</span><span class="sxs-lookup"><span data-stu-id="65f7d-281">Protocol version</span></span> | <span data-ttu-id="65f7d-282">Valor válido</span><span class="sxs-lookup"><span data-stu-id="65f7d-282">Valid value</span></span> |
|----|----|
| <span data-ttu-id="65f7d-283">v3.1</span><span class="sxs-lookup"><span data-stu-id="65f7d-283">v3.1</span></span> | `https://sts.windows.net/d6d49420-f39b-4df7-a1dc-d59a935871db/` |
| <span data-ttu-id="65f7d-284">v3.2</span><span class="sxs-lookup"><span data-stu-id="65f7d-284">v3.2</span></span> | `https://sts.windows.net/f8cdef31-a31e-4b4a-93e4-5f571e91255a/` |

#### <a name="openid-metadata-document"></a><span data-ttu-id="65f7d-285">Documento de metadatos de OpenID</span><span class="sxs-lookup"><span data-stu-id="65f7d-285">OpenID metadata document</span></span>

| <span data-ttu-id="65f7d-286">Versión del protocolo</span><span class="sxs-lookup"><span data-stu-id="65f7d-286">Protocol version</span></span> | <span data-ttu-id="65f7d-287">Valor válido</span><span class="sxs-lookup"><span data-stu-id="65f7d-287">Valid value</span></span> |
|----|----|
| <span data-ttu-id="65f7d-288">v3.1 y v3.2</span><span class="sxs-lookup"><span data-stu-id="65f7d-288">v3.1 & v3.2</span></span> | `https://login.microsoftonline.com/botframework.com/v2.0/.well-known/openid-configuration` |

## <a name="additional-resources"></a><span data-ttu-id="65f7d-289">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="65f7d-289">Additional resources</span></span>

- [<span data-ttu-id="65f7d-290">Solución de problemas de autenticación de Bot Framework</span><span class="sxs-lookup"><span data-stu-id="65f7d-290">Troubleshooting Bot Framework authentication</span></span>](../bot-service-troubleshoot-authentication-problems.md)
- [<span data-ttu-id="65f7d-291">JSON Web Token (JWT) draft-jones-json-web-token-07</span><span class="sxs-lookup"><span data-stu-id="65f7d-291">JSON Web Token (JWT) draft-jones-json-web-token-07</span></span>](http://openid.net/specs/draft-jones-json-web-token-07.html)
- [<span data-ttu-id="65f7d-292">Firma web JSON (JWS) draft-jones-json-web-signature-04</span><span class="sxs-lookup"><span data-stu-id="65f7d-292">JSON Web Signature (JWS) draft-jones-json-web-signature-04</span></span>](https://tools.ietf.org/html/draft-jones-json-web-signature-04)
- [<span data-ttu-id="65f7d-293">Clave web JSON (JWK) RFC 7517</span><span class="sxs-lookup"><span data-stu-id="65f7d-293">JSON Web Key (JWK) RFC 7517</span></span>](https://tools.ietf.org/html/rfc7517)

[Activity]: bot-framework-rest-connector-api-reference.md#activity-object
