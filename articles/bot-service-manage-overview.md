---
title: Administración de un bot | Microsoft Docs
description: Aprenda a administrar un bot mediante el portal de Bot Service.
keywords: azure portal, administración de bots, probar en chat web, MicrosoftAppID, MicrosoftAppPassword, configuración de la aplicación
author: v-ducvo
ms.author: rstand
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ms.openlocfilehash: f64dddedc0215e1277a9570201b14e53fb031d9b
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305293"
---
# <a name="manage-a-bot"></a><span data-ttu-id="a5aa0-104">Administración de un bot</span><span class="sxs-lookup"><span data-stu-id="a5aa0-104">Manage a bot</span></span>

<span data-ttu-id="a5aa0-105">En este tema se explica cómo administrar un bot mediante Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="a5aa0-105">This topic explains how to manage a bot using the Azure portal.</span></span>

## <a name="bot-settings-overview"></a><span data-ttu-id="a5aa0-106">Introducción a la configuración de bots</span><span class="sxs-lookup"><span data-stu-id="a5aa0-106">Bot settings overview</span></span>

![Introducción a la configuración de bots](~/media/azure-manage-a-bot/overview.png)

<span data-ttu-id="a5aa0-108">En la hoja **Información general**, puede encontrar información general sobre el bot.</span><span class="sxs-lookup"><span data-stu-id="a5aa0-108">In the **Overview** blade, you can find high level information about your bot.</span></span> <span data-ttu-id="a5aa0-109">Por ejemplo, puede ver el **identificador de suscripción**, el **plan de tarifa** y el **punto de conexión de mensajería** del bot.</span><span class="sxs-lookup"><span data-stu-id="a5aa0-109">For example, you can see your bot's **Subscription ID**, **pricing tier**, and **Messaging endpoint**.</span></span>

## <a name="bot-management"></a><span data-ttu-id="a5aa0-110">Administración de bots</span><span class="sxs-lookup"><span data-stu-id="a5aa0-110">Bot management</span></span>

 <span data-ttu-id="a5aa0-111">La mayoría de las opciones de administración del bot se pueden encontrar en la sección **ADMINISTRACIÓN DE BOTS**.</span><span class="sxs-lookup"><span data-stu-id="a5aa0-111">You can find most of your bot's management options under the **BOT MANAGEMENT** section.</span></span> <span data-ttu-id="a5aa0-112">Esta es una lista de opciones para ayudarle a administrar su bot.</span><span class="sxs-lookup"><span data-stu-id="a5aa0-112">Below is a list of options to help you manage your bot.</span></span>

![Administración de bots](~/media/azure-manage-a-bot/bot-management.png)

| <span data-ttu-id="a5aa0-114">Opción</span><span class="sxs-lookup"><span data-stu-id="a5aa0-114">Option</span></span> |  <span data-ttu-id="a5aa0-115">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="a5aa0-115">Description</span></span> |
| ---- | ---- |
| <span data-ttu-id="a5aa0-116">**Compilar**</span><span class="sxs-lookup"><span data-stu-id="a5aa0-116">**Build**</span></span> | <span data-ttu-id="a5aa0-117">La pestaña Compilar proporciona opciones para realizar cambios en el bot.</span><span class="sxs-lookup"><span data-stu-id="a5aa0-117">The Build tab provides options for making changes to your bot.</span></span> <span data-ttu-id="a5aa0-118">Esta opción no está disponible para **Registration Only Bot** (Solo bot de registro).</span><span class="sxs-lookup"><span data-stu-id="a5aa0-118">This option is not available for **Registration Only Bot**.</span></span> |
| <span data-ttu-id="a5aa0-119">**Probar en Chat en web**</span><span class="sxs-lookup"><span data-stu-id="a5aa0-119">**Test in Web Chat**</span></span> | <span data-ttu-id="a5aa0-120">Use el control de chat web integrado para ayudarle a probar rápidamente su bot.</span><span class="sxs-lookup"><span data-stu-id="a5aa0-120">Use the integrated Web Chat control to help you quickly test your bot.</span></span> |
| <span data-ttu-id="a5aa0-121">**Analytics**</span><span class="sxs-lookup"><span data-stu-id="a5aa0-121">**Analytics**</span></span> | <span data-ttu-id="a5aa0-122">Si el análisis está activado para el bot, puede ver los datos de análisis que Application Insights ha recopilado para el bot.</span><span class="sxs-lookup"><span data-stu-id="a5aa0-122">If analytics is turned on for your bot, you can view the analytics data that Application Insights has collected for your bot.</span></span> |
| <span data-ttu-id="a5aa0-123">**Channels**</span><span class="sxs-lookup"><span data-stu-id="a5aa0-123">**Channels**</span></span> | <span data-ttu-id="a5aa0-124">Configure los canales que usa su bot para comunicarse con los usuarios.</span><span class="sxs-lookup"><span data-stu-id="a5aa0-124">Configure the channels your bot uses to communicate with users.</span></span> |
| <span data-ttu-id="a5aa0-125">**Configuración**</span><span class="sxs-lookup"><span data-stu-id="a5aa0-125">**Settings**</span></span> | <span data-ttu-id="a5aa0-126">Administre varias configuraciones de perfiles de bots, como nombre para mostrar, análisis y punto de conexión de mensajería.</span><span class="sxs-lookup"><span data-stu-id="a5aa0-126">Manage various bot profile settings such as display name, analytics, and messaging endpoint.</span></span> |
| <span data-ttu-id="a5aa0-127">**Preparación para la voz**</span><span class="sxs-lookup"><span data-stu-id="a5aa0-127">**Speech priming**</span></span> | <span data-ttu-id="a5aa0-128">Administre las conexiones entre la aplicación de LUIS y el servicio Bing Speech...</span><span class="sxs-lookup"><span data-stu-id="a5aa0-128">Manage the connections between your LUIS app and the Bing Speech service..</span></span> |
| <span data-ttu-id="a5aa0-129">**Precios de Bot Service**</span><span class="sxs-lookup"><span data-stu-id="a5aa0-129">**Bot Service pricing**</span></span> | <span data-ttu-id="a5aa0-130">Administre el plan de tarifa del servicio de bots.</span><span class="sxs-lookup"><span data-stu-id="a5aa0-130">Manage the pricing tier for the bot service.</span></span> |

## <a name="app-service-settings"></a><span data-ttu-id="a5aa0-131">Configuración de App Service</span><span class="sxs-lookup"><span data-stu-id="a5aa0-131">App service settings</span></span>

![Configuración de App Service](~/media/azure-manage-a-bot/app-service-settings.png)

<span data-ttu-id="a5aa0-133">La hoja **Configuración de la aplicación** contiene información detallada sobre su bot, como el entorno del bot, el identificador, la clave de Application Insights, el identificador de aplicación de Microsoft y la contraseña de aplicación de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="a5aa0-133">The **Application Settings** blade contains detailed information about your bot, such as the bot's environment, ID, Application Insights key, Microsoft App ID, and Microsoft App password.</span></span>

### <a name="microsoftappid-and-microsoftapppassword"></a><span data-ttu-id="a5aa0-134">MicrosoftAppID y MicrosoftAppPassword</span><span class="sxs-lookup"><span data-stu-id="a5aa0-134">MicrosoftAppID and MicrosoftAppPassword</span></span>

<span data-ttu-id="a5aa0-135">Puede encontrar los valores **MicrosoftAppID** y **MicrosoftAppPassword** de su bot en la hoja **Configuración de la aplicación**.</span><span class="sxs-lookup"><span data-stu-id="a5aa0-135">You can find the **MicrosoftAppID** and **MicrosoftAppPassword** for your bot in the **Application Settings** blade.</span></span>

![Microsoft AppID y contraseña](~/media/azure-manage-a-bot/app-settings.png)

> [!NOTE]
> <span data-ttu-id="a5aa0-137">El servicio de bots **Registro de canales de bots** incluye un valor *MicrosoftAppID*, pero como no hay un servicio de aplicaciones asociado con este tipo de servicio, no hay una hoja **Configuración de la aplicación** para que consulte el valor *MicrosoftAppPassword*.</span><span class="sxs-lookup"><span data-stu-id="a5aa0-137">The **Bot Channels Registration** bot service comes with a *MicrosoftAppID* but because there is no app service associated with this type of service, there is no **Application Settings** blade for you to look up the *MicrosoftAppPassword*.</span></span> <span data-ttu-id="a5aa0-138">Para obtener la contraseña, debe generar una.</span><span class="sxs-lookup"><span data-stu-id="a5aa0-138">To get the password, you must go generate one.</span></span> <span data-ttu-id="a5aa0-139">Para generar la contraseña para un **registro de canales de Bot**, consulte [Bot Channels Registration password](bot-service-quickstart-registration.md#bot-channels-registration-password) (Contraseña de registro de canales de bots).</span><span class="sxs-lookup"><span data-stu-id="a5aa0-139">To generate the password for a **Bot Channels Registration**, see [Bot Channels Registration password](bot-service-quickstart-registration.md#bot-channels-registration-password)</span></span>

## <a name="next-steps"></a><span data-ttu-id="a5aa0-140">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="a5aa0-140">Next steps</span></span>
<span data-ttu-id="a5aa0-141">Ahora que ha explorado la hoja de Bot Service en Azure Portal, aprenda a usar el Editor de código en línea para personalizar su bot.</span><span class="sxs-lookup"><span data-stu-id="a5aa0-141">Now that you have explored the Bot Service blade in the Azure portal, learn how to use the Online Code Editor to customize your bot.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="a5aa0-142">Uso del editor de código en línea</span><span class="sxs-lookup"><span data-stu-id="a5aa0-142">Use the Online Code Editor</span></span>](bot-service-build-online-code-editor.md)
