---
title: Claves de Application Insights | Microsoft Docs
description: Obtenga información sobre cómo obtener claves de Application Insights para agregar datos de telemetría a un bot.
author: v-ducvo
ms.author: v-ducvo
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ms.openlocfilehash: 07fb6e9630996a61932da99b0575d43f4604141e
ms.sourcegitcommit: 3cb288cf2f09eaede317e1bc8d6255becf1aec61
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2018
ms.locfileid: "47389434"
---
# <a name="application-insights-keys"></a><span data-ttu-id="831df-103">Claves de Application Insights</span><span class="sxs-lookup"><span data-stu-id="831df-103">Application Insights keys</span></span>

<span data-ttu-id="831df-104">Azure **Application Insights** muestra los datos de la aplicación en un recurso de Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="831df-104">Azure **Application Insights** displays data about your application in a Microsoft Azure resource.</span></span> <span data-ttu-id="831df-105">Para agregar datos de telemetría al bot, necesita una suscripción de Azure y un recurso de Application Insights creado para el bot.</span><span class="sxs-lookup"><span data-stu-id="831df-105">To add telemetry to your bot you need an Azure subscription and an Application Insights resource created for your bot.</span></span> <span data-ttu-id="831df-106">A partir de este recurso, puede obtener las tres claves para configurar el bot:</span><span class="sxs-lookup"><span data-stu-id="831df-106">From this resource, you can obtain the three keys to configure your bot:</span></span>

1. <span data-ttu-id="831df-107">Clave de instrumentación</span><span class="sxs-lookup"><span data-stu-id="831df-107">Instrumentation key</span></span>
2. <span data-ttu-id="831df-108">Identificador de aplicación</span><span class="sxs-lookup"><span data-stu-id="831df-108">Application ID</span></span>
3. <span data-ttu-id="831df-109">Clave de API</span><span class="sxs-lookup"><span data-stu-id="831df-109">API key</span></span>

<span data-ttu-id="831df-110">En este tema le mostraremos cómo crear estas claves de Application Insights.</span><span class="sxs-lookup"><span data-stu-id="831df-110">This topic will show you how to create these Application Insights keys.</span></span>

> [!NOTE]
> <span data-ttu-id="831df-111">Durante la creación del bot o el proceso de registro, tuvo la opción de *activar* o *desactivar* **Application Insights**.</span><span class="sxs-lookup"><span data-stu-id="831df-111">During the bot creation or registration process, you were given the option to turn **Application Insights** *On* or *Off*.</span></span> <span data-ttu-id="831df-112">Si lo había *activado*, el bot ya tiene todas las claves necesarias de Application Insights.</span><span class="sxs-lookup"><span data-stu-id="831df-112">If you had turned it *On* then your bot already has all the necessary Application Insights keys it needs.</span></span> <span data-ttu-id="831df-113">Sin embargo, si lo había *desactivado*, puede seguir las instrucciones de este tema para crear manualmente estas claves.</span><span class="sxs-lookup"><span data-stu-id="831df-113">However, if you had turned it *Off* then you can follow the instructions in this topic to help you manually create these keys.</span></span>

## <a name="instrumentation-key"></a><span data-ttu-id="831df-114">Clave de instrumentación</span><span class="sxs-lookup"><span data-stu-id="831df-114">Instrumentation key</span></span>

<span data-ttu-id="831df-115">Para obtener la clave de instrumentación, haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="831df-115">To get the Instrumentation key, do the following:</span></span>
1. <span data-ttu-id="831df-116">Desde [Azure Portal](http://portal.azure.com), en la sección Monitor, cree un nuevo recurso de **Application Insights** (o use uno existente).</span><span class="sxs-lookup"><span data-stu-id="831df-116">From the [Azure portal](http://portal.azure.com), under the Monitor section, create a new **Application Insights** resource (or use an existing one).</span></span>
<span data-ttu-id="831df-117">![Captura de pantalla del portal de la lista de Application Insights](~/media/portal-app-insights-add-new.png)</span><span class="sxs-lookup"><span data-stu-id="831df-117">![Portal screen capture of Application Insights listing](~/media/portal-app-insights-add-new.png)</span></span>

2. <span data-ttu-id="831df-118">En la lista de recursos de Application Insights, haga clic en el recurso de Application Insights que acaba de crear.</span><span class="sxs-lookup"><span data-stu-id="831df-118">From the list of Application Insights resources, click the Application Insight resource you just created.</span></span>

3. <span data-ttu-id="831df-119">Haga clic en **Descripción general**.</span><span class="sxs-lookup"><span data-stu-id="831df-119">Click **Overview**.</span></span>

4. <span data-ttu-id="831df-120">Expanda el bloque **Información esencial** y busque la **clave de instrumentación**.</span><span class="sxs-lookup"><span data-stu-id="831df-120">Expand the **Essentials** block and find the **Instrumentation Key**.</span></span> 
<span data-ttu-id="831df-121">![Captura de pantalla del portal de la clave de instrumentación](~/media/portal-app-insights-instrumentation-key.png)</span><span class="sxs-lookup"><span data-stu-id="831df-121">![Portal screen capture of the Instrumentation key](~/media/portal-app-insights-instrumentation-key.png)</span></span>

5. <span data-ttu-id="831df-122">Copie la **clave de instrumentación** y péguela en el campo **Clave de instrumentación de Application Insights** de la configuración del bot.</span><span class="sxs-lookup"><span data-stu-id="831df-122">Copy the **Instrumentation Key** and paste it to the **Application Insights Instrumentation Key** field of your bot's settings.</span></span>

## <a name="application-id"></a><span data-ttu-id="831df-123">Identificador de aplicación</span><span class="sxs-lookup"><span data-stu-id="831df-123">Application ID</span></span>

<span data-ttu-id="831df-124">Para obtener el id. de la aplicación, haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="831df-124">To get the Application ID, do the following:</span></span>
1. <span data-ttu-id="831df-125">Desde el recurso de Application Insights, haga clic en **Acceso de API**.</span><span class="sxs-lookup"><span data-stu-id="831df-125">From your Application Insights resource, click **API Access**.</span></span>

2. <span data-ttu-id="831df-126">Copie el **id. de la aplicación** y péguelo en el campo **Id. de la aplicación de Application Insights** de la configuración del bot.</span><span class="sxs-lookup"><span data-stu-id="831df-126">Copy the **Application ID** and paste it to the **Application Insights Application ID** field of your bot's settings.</span></span> 
<span data-ttu-id="831df-127">![Captura de pantalla del portal del id. de la aplicación](~/media/portal-app-insights-appid.png)</span><span class="sxs-lookup"><span data-stu-id="831df-127">![Portal screen capture of the Application ID](~/media/portal-app-insights-appid.png)</span></span>

## <a name="api-key"></a><span data-ttu-id="831df-128">Clave de API</span><span class="sxs-lookup"><span data-stu-id="831df-128">API key</span></span>

<span data-ttu-id="831df-129">Para obtener la clave de API, haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="831df-129">To get the API key, do the following:</span></span>
1. <span data-ttu-id="831df-130">Desde el recurso de Application Insights, haga clic en **Acceso de API**.</span><span class="sxs-lookup"><span data-stu-id="831df-130">From the Application Insights resource, click **API Access**.</span></span>

2. <span data-ttu-id="831df-131">Haga clic en **Crear clave de API**.</span><span class="sxs-lookup"><span data-stu-id="831df-131">Click **Create API Key**.</span></span>

3. <span data-ttu-id="831df-132">Escriba una descripción breve, compruebe la opción **Read telementry** (Leer datos de telemetría) y haga clic en el botón **Generar clave**.</span><span class="sxs-lookup"><span data-stu-id="831df-132">Enter a short description, check the **Read telementry** option, and click the **Generate key** button.</span></span>
<span data-ttu-id="831df-133">![Captura de pantalla del portal del id. de la aplicación y de la clave de API](~/media/portal-app-insights-appid-apikey.png)</span><span class="sxs-lookup"><span data-stu-id="831df-133">![Portal screen capture of the Application ID and API Key](~/media/portal-app-insights-appid-apikey.png)</span></span>

   > [!WARNING]
   > <span data-ttu-id="831df-134">Copie esta **clave de API** y guárdela porque nunca se le volverá a mostrar.</span><span class="sxs-lookup"><span data-stu-id="831df-134">Copy this **API key** and save it because this key will never be shown to you again.</span></span> <span data-ttu-id="831df-135">Si pierda esta clave, deberá crear una nueva.</span><span class="sxs-lookup"><span data-stu-id="831df-135">If you lose this key, you have to create a new one.</span></span>

4. <span data-ttu-id="831df-136">Copie la clave de API en el campo **Clave de API de Application Insights** de la configuración del bot.</span><span class="sxs-lookup"><span data-stu-id="831df-136">Copy the API key to the **Application Insights API key** field of your bot's settings.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="831df-137">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="831df-137">Additional resources</span></span>
<span data-ttu-id="831df-138">Para obtener más información sobre cómo conectar estos campos en la configuración del bot, consulte [Enable analytics](~/bot-service-manage-analytics.md#enable-analytics) (Habilitación de los análisis).</span><span class="sxs-lookup"><span data-stu-id="831df-138">For more information on how to connect these fields into your bot's settings, see [Enable analytics](~/bot-service-manage-analytics.md#enable-analytics).</span></span>