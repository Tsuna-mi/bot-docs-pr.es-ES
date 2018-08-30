---
title: Protección del bot | Microsoft Docs
description: Obtenga información sobre cómo proteger el bot mediante el uso de la autenticación de Bot Framework y HTTPS.
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/17
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: 376982396ac11cbcf54f26255235b3779e0e1c26
ms.sourcegitcommit: 2dc75701b169d822c9499e393439161bc87639d2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42905717"
---
# <a name="secure-your-bot"></a><span data-ttu-id="9ca4d-103">Protección del bot</span><span class="sxs-lookup"><span data-stu-id="9ca4d-103">Secure your bot</span></span>

[!INCLUDE [pre-release-label](../includes/pre-release-label-v3.md)]

<span data-ttu-id="9ca4d-104">Su bot puede conectarse a muchos canales de comunicación diferentes (Skype, SMS, correo electrónico, etc.) a través del servicio de Bot Framework Connector.</span><span class="sxs-lookup"><span data-stu-id="9ca4d-104">Your bot can be connected to many different communication channels (Skype, SMS, email, and others) through the Bot Framework Connector service.</span></span> <span data-ttu-id="9ca4d-105">En este artículo se describe cómo proteger su bot con la autenticación Bot Framework y HTTPS.</span><span class="sxs-lookup"><span data-stu-id="9ca4d-105">This article describes how to secure your bot by using HTTPS and Bot Framework authentication.</span></span>

## <a name="use-https-and-bot-framework-authentication"></a><span data-ttu-id="9ca4d-106">Usar la autenticación de Bot Framework y HTTPS</span><span class="sxs-lookup"><span data-stu-id="9ca4d-106">Use HTTPS and Bot Framework authentication</span></span>

<span data-ttu-id="9ca4d-107">Para asegurarse de que solo el [conector](bot-builder-dotnet-concepts.md#connector) de Bot Framework Connector pueda acceder al punto de conexión del bot, configure el punto de conexión del bot para usar únicamente HTTPS y habilite la autenticación de Bot Framework [registrando](~/bot-service-quickstart-registration.md) el bot para que adquiera su appID y contraseña.</span><span class="sxs-lookup"><span data-stu-id="9ca4d-107">To ensure that your bot's endpoint can only be accessed by the Bot Framework [Connector](bot-builder-dotnet-concepts.md#connector), configure your bot's endpoint to use only HTTPS and enable Bot Framework authentication by [registering](~/bot-service-quickstart-registration.md) your bot to acquire its appID and password.</span></span> 

## <a name="configure-authentication-for-your-bot"></a><span data-ttu-id="9ca4d-108">Configurar la autenticación para el bot</span><span class="sxs-lookup"><span data-stu-id="9ca4d-108">Configure authentication for your bot</span></span>

<span data-ttu-id="9ca4d-109">Especifique el appID y la contraseña del bot en el archivo web.config del bot.</span><span class="sxs-lookup"><span data-stu-id="9ca4d-109">Specify the bot's appID and password in your bot's web.config file.</span></span> 

> [!NOTE]
> <span data-ttu-id="9ca4d-110">Para buscar el **AppID** y **AppPassword** del bot, consulte la sección [MicrosoftAppID and MicrosoftAppPassword](~/bot-service-manage-overview.md#microsoftappid-and-microsoftapppassword) (MicrosoftAppID y MicrosoftAppPassword).</span><span class="sxs-lookup"><span data-stu-id="9ca4d-110">To find your bot's **AppID** and **AppPassword**, see [MicrosoftAppID and MicrosoftAppPassword](~/bot-service-manage-overview.md#microsoftappid-and-microsoftapppassword).</span></span>

```xml
<appSettings>
    <add key="MicrosoftAppId" value="_appIdValue_" />
    <add key="MicrosoftAppPassword" value="_passwordValue_" />
</appSettings>
```

<span data-ttu-id="9ca4d-111">A continuación, utilice el atributo `[BotAuthentication]` para especificar las credenciales de autenticación al usar el SDK de Bot Builder para .NET para crear su bot.</span><span class="sxs-lookup"><span data-stu-id="9ca4d-111">Then, use the `[BotAuthentication]` attribute to specify authentication credentials when using the Bot Builder SDK for .NET to create your bot.</span></span> 

<span data-ttu-id="9ca4d-112">Para usar las credenciales de autenticación que se almacenan en el archivo web.config, especifique el atributo `[BotAuthentication]` sin parámetros.</span><span class="sxs-lookup"><span data-stu-id="9ca4d-112">To use the authentication credentials that are stored in the web.config file, specify the `[BotAuthentication]` with no parameters.</span></span>

[!code-csharp[Use botAuthentication attribute](../includes/code/dotnet-security.cs#attribute1)]

<span data-ttu-id="9ca4d-113">Para usar otros valores para las credenciales de autenticación, especifique el atributo `[BotAuthentication]` e inserte esos valores.</span><span class="sxs-lookup"><span data-stu-id="9ca4d-113">To use other values for authentication credentials, specify the `[BotAuthentication]` attribute and pass in those values.</span></span>

[!code-csharp[Use botAuthentication attribute with parameters](../includes/code/dotnet-security.cs#attribute2)]

## <a name="additional-resources"></a><span data-ttu-id="9ca4d-114">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="9ca4d-114">Additional resources</span></span>

- [<span data-ttu-id="9ca4d-115">Bot Builder SDK para .NET</span><span class="sxs-lookup"><span data-stu-id="9ca4d-115">Bot Builder SDK for .NET</span></span>](bot-builder-dotnet-overview.md)
- <span data-ttu-id="9ca4d-116">[Key concepts in the bot Builder SDK for .NET](bot-builder-dotnet-concepts.md) (Conceptos clave del SDK de Bot Builder para .NET)</span><span class="sxs-lookup"><span data-stu-id="9ca4d-116">[Key concepts in the bot Builder SDK for .NET](bot-builder-dotnet-concepts.md)</span></span>
- <span data-ttu-id="9ca4d-117">[Register a bot with the Bot Framework](~/bot-service-quickstart-registration.md) (Registrar un bot con Bot Framework)</span><span class="sxs-lookup"><span data-stu-id="9ca4d-117">[Register a bot with the Bot Framework](~/bot-service-quickstart-registration.md)</span></span>
