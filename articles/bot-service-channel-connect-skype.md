---
title: Conexión de un bot a Skype | Microsoft Docs
description: Obtenga información sobre cómo configurar un bot para acceder a través de la interfaz de Skpye.
keywords: skype, canales de bot, configurar skype, publicar, conectarse a canales
author: v-ducvo
ms.author: RobStand
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 2/1/2018
ms.openlocfilehash: 5dc4063125855113f813b8873b01df84c90e197e
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305089"
---
# <a name="connect-a-bot-to-skype"></a><span data-ttu-id="093e1-104">Conexión de un bot a Skype</span><span class="sxs-lookup"><span data-stu-id="093e1-104">Connect a bot to Skype</span></span>

<span data-ttu-id="093e1-105">Skype le mantiene conectado con los usuarios a través de mensajería instantánea, teléfono y videollamadas.</span><span class="sxs-lookup"><span data-stu-id="093e1-105">Skype keeps you connected with users through instant messaging, phone, and video calls.</span></span> <span data-ttu-id="093e1-106">Para ampliar esta funcionalidad, cree bots que los usuarios pueden detectar y con los que puedan interactuar a través de la interfaz de Skype.</span><span class="sxs-lookup"><span data-stu-id="093e1-106">Extend this functionality by building bots that users can discover and interact with through the Skype interface.</span></span>

<span data-ttu-id="093e1-107">Para agregar el canal de Skype, abra el bot en [Azure Portal](https://portal.azure.com/), haga clic en la hoja **Canales** y luego en **Skype**.</span><span class="sxs-lookup"><span data-stu-id="093e1-107">To add the Skype channel, open the bot in the [Azure Portal](https://portal.azure.com/), click the **Channels** blade, and then click **Skype**.</span></span> <span data-ttu-id="093e1-108">Esto le llevará a la página **Configurar Skype**.</span><span class="sxs-lookup"><span data-stu-id="093e1-108">This will take you to the **Configure Skype** settings page.</span></span> <span data-ttu-id="093e1-109">Rellene toda la información necesaria sobre el bot y haga clic en **Guardar** para conectarse al canal de Skype.</span><span class="sxs-lookup"><span data-stu-id="093e1-109">Fill out all the necessary information about your bot and click **Save** to connect the Skype channel.</span></span> <span data-ttu-id="093e1-110">Acepte los **Términos de servicio**, y el canal de Skype se agregará al bot.</span><span class="sxs-lookup"><span data-stu-id="093e1-110">Accept the **Terms of Service** and the Skype channel will be added to your bot.</span></span>

![Agregar el canal de Skype](~/media/channels/skype-addchannel.png)

## <a name="web-control"></a><span data-ttu-id="093e1-112">Control web</span><span class="sxs-lookup"><span data-stu-id="093e1-112">Web control</span></span>

<span data-ttu-id="093e1-113">Para insertar el bot en su sitio web, puede obtener el código haciendo clic en el botón **Obtener código para insertar** desde la sección **Control web**.</span><span class="sxs-lookup"><span data-stu-id="093e1-113">To embed the bot into your website, you can get the code by click the **Get embed code** button from the **Web control** section.</span></span>

## <a name="messaging"></a><span data-ttu-id="093e1-114">Mensajería</span><span class="sxs-lookup"><span data-stu-id="093e1-114">Messaging</span></span>

<span data-ttu-id="093e1-115">En esta sección configura la manera en que el bot envía y recibe mensajes de Skype.</span><span class="sxs-lookup"><span data-stu-id="093e1-115">This section configures how your bot sends and receives messages in Skype.</span></span>

## <a name="calling"></a><span data-ttu-id="093e1-116">Llamada</span><span class="sxs-lookup"><span data-stu-id="093e1-116">Calling</span></span>

<span data-ttu-id="093e1-117">En esta sección se configura la característica de llamadas de Skype en el bot.</span><span class="sxs-lookup"><span data-stu-id="093e1-117">This section configures the calling feature of Skype in your bot.</span></span> <span data-ttu-id="093e1-118">Puede especificar si **Llamada** está habilitada para el bot, y si está habilitada, si se usará la funcionalidad IVR o de multimedia en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="093e1-118">You can specify whether **Calling** is enabled for your bot and if enabled, whether IVR functionality or Real Time Media functionality is to be used.</span></span>

## <a name="groups"></a><span data-ttu-id="093e1-119">Grupos</span><span class="sxs-lookup"><span data-stu-id="093e1-119">Groups</span></span>

<span data-ttu-id="093e1-120">En esta sección se configura si el bot puede agregarse a un grupo y cómo se comporta en un grupo para la mensajería; también se utiliza para habilitar las videollamadas grupales para los bots de llamada.</span><span class="sxs-lookup"><span data-stu-id="093e1-120">This section configures whether your bot can be added to a group and how it behaves in a group for messaging and is also used to enable Group Video Calls for Calling bots.</span></span>

## <a name="publish"></a><span data-ttu-id="093e1-121">Publicar</span><span class="sxs-lookup"><span data-stu-id="093e1-121">Publish</span></span>

<span data-ttu-id="093e1-122">En esta sección se establece la configuración de publicación del bot.</span><span class="sxs-lookup"><span data-stu-id="093e1-122">This section configures the publish settings of your bot.</span></span> <span data-ttu-id="093e1-123">Todos los campos con \* son campos obligatorios.</span><span class="sxs-lookup"><span data-stu-id="093e1-123">All fields labeled with a \* are required fields.</span></span>

<span data-ttu-id="093e1-124">Los bots en **versión preliminar** están limitados a 100 contactos.</span><span class="sxs-lookup"><span data-stu-id="093e1-124">Bots in **Preview** are limited to 100 contacts.</span></span> <span data-ttu-id="093e1-125">Si necesita más de 100 contactos, envíe el bot para revisión.</span><span class="sxs-lookup"><span data-stu-id="093e1-125">If you need more than 100 contacts, submit your bot for review.</span></span> <span data-ttu-id="093e1-126">Al hacer clic en **Enviar para revisión**, hará automáticamente que el bot se pueda buscar en Skype si se lo acepta.</span><span class="sxs-lookup"><span data-stu-id="093e1-126">Clicking **Submit for Review** will automatically make your bot searchable in Skype if accepted.</span></span> <span data-ttu-id="093e1-127">Si no se aprueba la solicitud, se le notificará qué debe cambiar para que pueda aprobarse.</span><span class="sxs-lookup"><span data-stu-id="093e1-127">If your request cannot be approved, you will be notified as to what you need to change before it can be approved.</span></span>

## <a name="next-steps"></a><span data-ttu-id="093e1-128">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="093e1-128">Next steps</span></span>

* [<span data-ttu-id="093e1-129">Skype Empresarial</span><span class="sxs-lookup"><span data-stu-id="093e1-129">Skype for Business</span></span>](bot-service-channel-connect-skypeforbusiness.md)
