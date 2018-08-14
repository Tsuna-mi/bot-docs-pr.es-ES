---
title: Crear un bot de Conversation Designer | Microsoft Docs
description: Obtenga información sobre cómo crear un bot con Conversation Designer.
author: v-ducvo
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ROBOTS: NoIndex, NoFollow
ms.openlocfilehash: 1b33d08e56bf8ae473b28cf1cf3a26d8138ce861
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39304917"
---
# <a name="create-a-new-conversation-designer-bot"></a><span data-ttu-id="1bcb3-103">Cree un bot de Conversation Designer</span><span class="sxs-lookup"><span data-stu-id="1bcb3-103">Create a new Conversation Designer bot</span></span>
> [!IMPORTANT]
> <span data-ttu-id="1bcb3-104">Conversation Designer no está disponible para todos los clientes todavía.</span><span class="sxs-lookup"><span data-stu-id="1bcb3-104">Conversation Designer is not available to all customers yet.</span></span> <span data-ttu-id="1bcb3-105">Habrá más información sobre la disponibilidad de Conversation Designer más adelante este año.</span><span class="sxs-lookup"><span data-stu-id="1bcb3-105">More details on availability of Conversation Designer will come later this year.</span></span>

<span data-ttu-id="1bcb3-106">Este tutorial le guiará paso a paso para crear un nuevo bot de Conversation Designer.</span><span class="sxs-lookup"><span data-stu-id="1bcb3-106">This tutorial walks you through step-by-step instructions for creating a new Conversation Designer bot.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="1bcb3-107">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="1bcb3-107">Prerequisites</span></span>

- <span data-ttu-id="1bcb3-108">Conversation Designer requiere una suscripción de Azure.</span><span class="sxs-lookup"><span data-stu-id="1bcb3-108">Conversation Designer requires an Azure subscription.</span></span> <span data-ttu-id="1bcb3-109">Empiece por <a href="https://azure.microsoft.com/en-us/" target="_blank">aquí</a>.</span><span class="sxs-lookup"><span data-stu-id="1bcb3-109">You can get started <a href="https://azure.microsoft.com/en-us/" target="_blank">here</a></span></span>
- <span data-ttu-id="1bcb3-110">Si aún no lo ha hecho, asegúrese de iniciar sesión en el [portal de LUIS](https://luis.ai) al menos una vez después de que haya creado una cuenta con ellos.</span><span class="sxs-lookup"><span data-stu-id="1bcb3-110">If you haven't done so already, make sure you sign in to the [LUIS portal](https://luis.ai) at least once after you created an account with them.</span></span>
- <span data-ttu-id="1bcb3-111">Familiaridad con la programación de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="1bcb3-111">Familiarity with JavaScript programming.</span></span> <span data-ttu-id="1bcb3-112">Las funciones de script personalizadas se escriben en JavaScript.</span><span class="sxs-lookup"><span data-stu-id="1bcb3-112">Custom script functions are written in JavaScript.</span></span>
- <span data-ttu-id="1bcb3-113">Microsoft Edge o Google Chrome</span><span class="sxs-lookup"><span data-stu-id="1bcb3-113">Microsoft Edge or Google Chrome</span></span>

## <a name="create-a-conversation-designer-bot"></a><span data-ttu-id="1bcb3-114">Crear un bot de Conversation Designer</span><span class="sxs-lookup"><span data-stu-id="1bcb3-114">Create a Conversation Designer bot</span></span>

<span data-ttu-id="1bcb3-115">Para crear un bot de Conversation Designer, siga estos pasos:</span><span class="sxs-lookup"><span data-stu-id="1bcb3-115">To create a Conversation Designer bot, follow these steps:</span></span>
1. <span data-ttu-id="1bcb3-116">Vaya a https://dev.botframework.com/ e inicie sesión.</span><span class="sxs-lookup"><span data-stu-id="1bcb3-116">Go to https://dev.botframework.com/, and sign in.</span></span> <span data-ttu-id="1bcb3-117">Use la dirección de correo electrónico que proporcionó a Microsoft Corporation para participar en esta versión preliminar privada.</span><span class="sxs-lookup"><span data-stu-id="1bcb3-117">Use the email address you provided to Microsoft Corporation to participate in this private preview release.</span></span>
2. <span data-ttu-id="1bcb3-118">Haga clic en **Crear un bot** en el panel de navegación de la parte superior derecha.</span><span class="sxs-lookup"><span data-stu-id="1bcb3-118">Click **Create a bot** in the top-right navigation panel.</span></span> 
3. <span data-ttu-id="1bcb3-119">Haga clic en **Crear** para *crear un bot con Conversation Designer*.</span><span class="sxs-lookup"><span data-stu-id="1bcb3-119">Click **Create** to *Create a bot with the Conversation Designer*.</span></span>
4. <span data-ttu-id="1bcb3-120">Seleccione una de los muchos [bots de ejemplo](conversation-designer-sample-bots.md) para empezar.</span><span class="sxs-lookup"><span data-stu-id="1bcb3-120">Select from one of the many [sample bots](conversation-designer-sample-bots.md) to start with.</span></span> <span data-ttu-id="1bcb3-121">Haga clic en **Next**.</span><span class="sxs-lookup"><span data-stu-id="1bcb3-121">Click **Next**.</span></span> <span data-ttu-id="1bcb3-122">Si no está seguro de qué **bot de ejemplo** utilizar, elija el que más se parezca al bot que quiera crear.</span><span class="sxs-lookup"><span data-stu-id="1bcb3-122">If you are not sure which **sample bot** to use, just choose the one you think is the closest to a bot you want to build.</span></span> <span data-ttu-id="1bcb3-123">Más adelante podrá cambiar a otro **bot de ejemplo**.</span><span class="sxs-lookup"><span data-stu-id="1bcb3-123">You can later switch to a different **sample bot**.</span></span>
5. <span data-ttu-id="1bcb3-124">Complete todos los campos y haga clic en **crear bots**; esta operación tarda aproximadamente 2 minutos en completar el aprovisionamiento del bot.</span><span class="sxs-lookup"><span data-stu-id="1bcb3-124">Complete all fields and click **Create bot** – it takes about 2 minutes for bot provisioning to complete.</span></span> 

## <a name="bot-provisioning"></a><span data-ttu-id="1bcb3-125">Aprovisionamiento de bots</span><span class="sxs-lookup"><span data-stu-id="1bcb3-125">Bot Provisioning</span></span>

<span data-ttu-id="1bcb3-126">Las siguientes características de Azure se aprovisionan automáticamente al crear un bot de Conversation Designer:</span><span class="sxs-lookup"><span data-stu-id="1bcb3-126">The following Azure features are automatically provisioned when you create a Conversation Designer bot:</span></span> 

1. <span data-ttu-id="1bcb3-127">Grupo de recursos de Azure con el nombre del bot especificado</span><span class="sxs-lookup"><span data-stu-id="1bcb3-127">Azure resource group with the bot name you specified</span></span>
2. <span data-ttu-id="1bcb3-128">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="1bcb3-128">Azure App service</span></span>
3. <span data-ttu-id="1bcb3-129">Plan de Azure App Service</span><span class="sxs-lookup"><span data-stu-id="1bcb3-129">Azure App Service plan</span></span> 
4. <span data-ttu-id="1bcb3-130">Cuenta de Azure Storage</span><span class="sxs-lookup"><span data-stu-id="1bcb3-130">Azure Storage account</span></span>
5. <span data-ttu-id="1bcb3-131">Application Insights</span><span class="sxs-lookup"><span data-stu-id="1bcb3-131">Application Insights</span></span> 
6. <span data-ttu-id="1bcb3-132">Suscripción de Cognitive Services para [LUIS.ai](https://luis.ai).</span><span class="sxs-lookup"><span data-stu-id="1bcb3-132">Cognitive Services subscription for [LUIS.ai](https://luis.ai).</span></span> <span data-ttu-id="1bcb3-133">Se crea una aplicación de LUIS con la opción **Control de bots** (además de una cadena generada aleatoriamente) como el nombre de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1bcb3-133">A LUIS app is created with the **Bot handle** (plus a randomly generated string) as the app name.</span></span>
7. <span data-ttu-id="1bcb3-134">Aplicación individual de la cuenta de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="1bcb3-134">Microsoft Account single app.</span></span> [<span data-ttu-id="1bcb3-135">Más información</span><span class="sxs-lookup"><span data-stu-id="1bcb3-135">Learn more</span></span>](https://apps.dev.microsoft.com/#/appList)

## <a name="welcome-message"></a><span data-ttu-id="1bcb3-136">Mensaje de bienvenida</span><span class="sxs-lookup"><span data-stu-id="1bcb3-136">Welcome message</span></span>

<span data-ttu-id="1bcb3-137">Una vez aprovisionado el bot, Conversation Designer abrirá la página **Compilar** del bot.</span><span class="sxs-lookup"><span data-stu-id="1bcb3-137">Once the bot is provisioned, Conversation Designer will open the bot's **Build** page.</span></span> <span data-ttu-id="1bcb3-138">Aparecerá un mensaje de bienvenida con información que le ayudará a empezar a trabajar.</span><span class="sxs-lookup"><span data-stu-id="1bcb3-138">A welcome message appears with information to help you get started.</span></span> <span data-ttu-id="1bcb3-139">Explore esas opciones o cierre el mensaje y empiece a trabajar en su bot.</span><span class="sxs-lookup"><span data-stu-id="1bcb3-139">Explore those options or close the message and start working on your bot.</span></span> <span data-ttu-id="1bcb3-140">Puede volver al mensaje de bienvenida haciendo clic en los puntos suspensivos (**...**) del panel de navegación superior izquierdo, y seleccionando la opción **Bienvenida...**.</span><span class="sxs-lookup"><span data-stu-id="1bcb3-140">You can get back to the welcome message by clicking the ellipses (**...**) from the top-left navigation panel, and then choosing the **Welcome...** option.</span></span>

## <a name="next-step"></a><span data-ttu-id="1bcb3-141">Paso siguiente</span><span class="sxs-lookup"><span data-stu-id="1bcb3-141">Next step</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="1bcb3-142">Guardar bot</span><span class="sxs-lookup"><span data-stu-id="1bcb3-142">Save bot</span></span>](conversation-designer-save-bot.md)

## <a name="additional-resources"></a><span data-ttu-id="1bcb3-143">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="1bcb3-143">Additional resources</span></span>
* <span data-ttu-id="1bcb3-144">Obtenga más información sobre las [tareas](conversation-designer-tasks.md)</span><span class="sxs-lookup"><span data-stu-id="1bcb3-144">Learn about [tasks](conversation-designer-tasks.md)</span></span>
* <span data-ttu-id="1bcb3-145">Obtenga más información sobre el [SDK de Bot Builder para Node](../nodejs/index.md)</span><span class="sxs-lookup"><span data-stu-id="1bcb3-145">Learn more about the [Bot Builder SDK for Node](../nodejs/index.md)</span></span> 
