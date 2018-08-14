---
title: Conexión de un bot a GroupMe | Microsoft Docs
description: Aprenda a configurar la conexión de un bot a GroupMe.
keywords: canal de bot, GroupMe, crear GroupMe, credenciales
author: RobStand
ms.author: RobStand
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ms.openlocfilehash: 2cc3081f75c755d14f32f02fe9d0d3a3c48bcf14
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305292"
---
# <a name="connect-a-bot-to-groupme"></a><span data-ttu-id="f163d-104">Conexión de un bot a GroupMe</span><span class="sxs-lookup"><span data-stu-id="f163d-104">Connect a bot to GroupMe</span></span>

<span data-ttu-id="f163d-105">Puede configurar un bot para comunicarse con personas que usan la aplicación de mensajería GroupMe.</span><span class="sxs-lookup"><span data-stu-id="f163d-105">You can configure a bot to communicate with people using the GroupMe group messaging app.</span></span>

[!INCLUDE [Channel Inspector intro](~/includes/snippet-channel-inspector.md)]

## <a name="sign-up-for-a-groupme-account"></a><span data-ttu-id="f163d-106">Registrarse para obtener una cuenta de GroupMe</span><span class="sxs-lookup"><span data-stu-id="f163d-106">Sign up for a GroupMe account</span></span>

<span data-ttu-id="f163d-107">Si no tiene una cuenta de GroupMe, [regístrese para obtener una nueva cuenta](https://web.groupme.com/signup).</span><span class="sxs-lookup"><span data-stu-id="f163d-107">If you don't have a GroupMe account, [sign up for a new account](https://web.groupme.com/signup).</span></span>

## <a name="create-a-groupme-application"></a><span data-ttu-id="f163d-108">Creación de una aplicación GroupMe</span><span class="sxs-lookup"><span data-stu-id="f163d-108">Create a GroupMe application</span></span>

<span data-ttu-id="f163d-109">[Cree una aplicación GroupMe](https://dev.groupme.com/applications/new) para su bot.</span><span class="sxs-lookup"><span data-stu-id="f163d-109">[Create a GroupMe application](https://dev.groupme.com/applications/new) for your bot.</span></span>

<span data-ttu-id="f163d-110">Use esta dirección URL de devolución de llamada: `https://groupme.botframework.com/Home/Login`</span><span class="sxs-lookup"><span data-stu-id="f163d-110">Use this callback URL: `https://groupme.botframework.com/Home/Login`</span></span>

![Creación de una aplicación](~/media/channels/GM-StepApp.png)

## <a name="gather-credentials"></a><span data-ttu-id="f163d-112">Obtención de las credenciales</span><span class="sxs-lookup"><span data-stu-id="f163d-112">Gather credentials</span></span>

1. <span data-ttu-id="f163d-113">En el campo **Redirect URL** (URL de redireccionamiento), copie el valor que aparece después de **client_id=**.</span><span class="sxs-lookup"><span data-stu-id="f163d-113">In the **Redirect URL** field, copy the value after **client_id=**.</span></span>
2. <span data-ttu-id="f163d-114">Copie el valor de **Access Token** (Token de acceso).</span><span class="sxs-lookup"><span data-stu-id="f163d-114">Copy the **Access Token** value.</span></span>

![Copia del identificador de cliente y el token de acceso](~/media/channels/GM-StepClientId.png)


## <a name="submit-credentials"></a><span data-ttu-id="f163d-116">Envío de las credenciales</span><span class="sxs-lookup"><span data-stu-id="f163d-116">Submit credentials</span></span>

1. <span data-ttu-id="f163d-117">En dev.botframework.com, pegue el valor **client_id** que acaba de copiar en el campo **Client ID** (Id. de cliente).</span><span class="sxs-lookup"><span data-stu-id="f163d-117">On dev.botframework.com, paste the **client_id** value you just copied into the **Client ID** field.</span></span>
2. <span data-ttu-id="f163d-118">Pegue el valor de **Access Token** (Token de acceso) en el campo **Access Token** (Token de acceso).</span><span class="sxs-lookup"><span data-stu-id="f163d-118">Paste the **Access Token** value into the **Access Token** field.</span></span>
2. <span data-ttu-id="f163d-119">Haga clic en **Save**(Guardar).</span><span class="sxs-lookup"><span data-stu-id="f163d-119">Click **Save**.</span></span>

![Escribir credenciales](~/media/channels/GM-StepClientIDToken.png)
