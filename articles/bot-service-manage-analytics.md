---
title: Análisis del bot | Microsoft Docs
description: Obtenga información sobre cómo usar la recopilación y el análisis de datos para mejorar su bot con análisis de Bot Framework.
keywords: análisis de bot, application insights, tráfico, latencia, integraciones, AppInsights
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ms.openlocfilehash: 3b0032db8e99c75ec8697f79a78cd6b0bd915db9
ms.sourcegitcommit: e8c513d3af5f0c514cadcbcd0a737a7393405afa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/20/2018
ms.locfileid: "42756579"
---
# <a name="bot-analytics"></a><span data-ttu-id="a2527-104">Análisis del bot</span><span class="sxs-lookup"><span data-stu-id="a2527-104">Bot analytics</span></span>
<span data-ttu-id="a2527-105">Analytics es una extensión de [Application Insights](/azure/application-insights/app-insights-analytics).</span><span class="sxs-lookup"><span data-stu-id="a2527-105">Analytics is an extension of [Application Insights](/azure/application-insights/app-insights-analytics).</span></span> <span data-ttu-id="a2527-106">Application Insights proporciona datos del **nivel de servicio** y de instrumentación, como tráfico, latencia e integraciones.</span><span class="sxs-lookup"><span data-stu-id="a2527-106">Application Insights provides **service-level** and instrumentation data like traffic, latency, and integrations.</span></span> <span data-ttu-id="a2527-107">Analytics proporciona informes de **nivel de conversación** relativos a los datos del usuario, los mensajes y los canales.</span><span class="sxs-lookup"><span data-stu-id="a2527-107">Analytics provides **conversation-level** reporting on user, message, and channel data.</span></span>

## <a name="view-analytics-for-a-bot"></a><span data-ttu-id="a2527-108">Visualización de análisis para un bot</span><span class="sxs-lookup"><span data-stu-id="a2527-108">View analytics for a bot</span></span>
<span data-ttu-id="a2527-109">Para obtener acceso a Analytics, abra el bot en el portal para desarrolladores y haga clic en **Analytics**.</span><span class="sxs-lookup"><span data-stu-id="a2527-109">To access Analytics, open the bot in the developer portal and click **Analytics**.</span></span>

<span data-ttu-id="a2527-110">¿Demasiados datos?</span><span class="sxs-lookup"><span data-stu-id="a2527-110">Too much data?</span></span> <span data-ttu-id="a2527-111">[Habilite y configure el muestreo](/azure/application-insights/app-insights-sampling) para reducir el tráfico y el almacenamiento de telemetría, a la vez que se mantiene un análisis estadísticamente correcto.</span><span class="sxs-lookup"><span data-stu-id="a2527-111">[Enable and configure sampling](/azure/application-insights/app-insights-sampling) to reduce telemetry traffic and storage while maintaining statistically correct analysis.</span></span> 

### <a name="specify-channel"></a><span data-ttu-id="a2527-112">Especificación del canal</span><span class="sxs-lookup"><span data-stu-id="a2527-112">Specify channel</span></span>
<span data-ttu-id="a2527-113">Elija los canales que aparecen en los gráficos a continuación.</span><span class="sxs-lookup"><span data-stu-id="a2527-113">Choose which channels appear in the graphs below.</span></span> <span data-ttu-id="a2527-114">Tenga en cuenta que si un bot no está habilitado en un canal, no habrá ningún dato de ese canal.</span><span class="sxs-lookup"><span data-stu-id="a2527-114">Note that if a bot is not enabled on a channel, there will be no data from that channel.</span></span>

![Selección del canal](~/media/analytics-channels.png)

* <span data-ttu-id="a2527-116">Active la casilla para incluir un canal en el gráfico.</span><span class="sxs-lookup"><span data-stu-id="a2527-116">Check the check box to include a channel in the chart.</span></span>
* <span data-ttu-id="a2527-117">Desmarque la casilla para quitar un canal del gráfico.</span><span class="sxs-lookup"><span data-stu-id="a2527-117">Clear the check box to remove a channel from the chart.</span></span>

### <a name="specify-time-period"></a><span data-ttu-id="a2527-118">Indicación del período de tiempo</span><span class="sxs-lookup"><span data-stu-id="a2527-118">Specify time period</span></span>
<span data-ttu-id="a2527-119">El análisis está disponible para los últimos 90 días únicamente.</span><span class="sxs-lookup"><span data-stu-id="a2527-119">Analysis is available for the past 90 days only.</span></span> <span data-ttu-id="a2527-120">La recopilación de datos comenzó cuando se habilitó Application Insights.</span><span class="sxs-lookup"><span data-stu-id="a2527-120">Data collection began when Application Insights was enabled.</span></span>

![Selección del período de tiempo](~/media/analytics-timepick.png)

<span data-ttu-id="a2527-122">Haga clic en el menú desplegable y, a continuación, haga clic en la cantidad de tiempo que se deben mostrar los gráficos.</span><span class="sxs-lookup"><span data-stu-id="a2527-122">Click the drop-down menu and then click the amount of time the graphs should display.</span></span>
<span data-ttu-id="a2527-123">Tenga en cuenta que, al cambiar el período de tiempo general, hará que el incremento de tiempo (eje X) en los gráficos cambié en consecuencia.</span><span class="sxs-lookup"><span data-stu-id="a2527-123">Note that changing the overall time frame will cause the time increment (X-axis) on the graphs to change accordingly.</span></span>

### <a name="grand-totals"></a><span data-ttu-id="a2527-124">Totales generales</span><span class="sxs-lookup"><span data-stu-id="a2527-124">Grand totals</span></span>
<span data-ttu-id="a2527-125">El número total de usuarios activos y de actividades enviadas y recibidas durante el período de tiempo especificado.</span><span class="sxs-lookup"><span data-stu-id="a2527-125">The total number of active users and activities sent and received during the specified time frame.</span></span>
<span data-ttu-id="a2527-126">Los guiones `--` indican que no hay ninguna actividad.</span><span class="sxs-lookup"><span data-stu-id="a2527-126">Dashes `--` indicate no activity.</span></span>

### <a name="retention"></a><span data-ttu-id="a2527-127">Retención</span><span class="sxs-lookup"><span data-stu-id="a2527-127">Retention</span></span>
<span data-ttu-id="a2527-128">Retención realiza un seguimiento de cuántos usuarios que enviaron un mensaje volvieron más adelante y enviaron otro.</span><span class="sxs-lookup"><span data-stu-id="a2527-128">Retention tracks how many users who sent one message came back later and sent another one.</span></span>
<span data-ttu-id="a2527-129">El gráfico abarca un plazo móvil de 10 días; los resultados no se ven afectados al cambiar el período de tiempo.</span><span class="sxs-lookup"><span data-stu-id="a2527-129">The chart is a rolling 10-day window; the results are not affected by changing the time frame.</span></span>

![Gráfico de retención](~/media/analytics-retention.png)

<span data-ttu-id="a2527-131">Tenga en cuenta que la fecha más reciente posible es dos días atrás: un usuario envió mensajes anteayer y luego *volvió* ayer.</span><span class="sxs-lookup"><span data-stu-id="a2527-131">Note that the most recent possible date is two days ago; a user sent messages the day before yesterday and then *returned* yesterday.</span></span>

### <a name="user"></a><span data-ttu-id="a2527-132">Usuario</span><span class="sxs-lookup"><span data-stu-id="a2527-132">User</span></span>
<span data-ttu-id="a2527-133">El gráfico Usuarios realiza un seguimiento de cuántos usuarios han accedido al bot con cada canal durante el período de tiempo especificado.</span><span class="sxs-lookup"><span data-stu-id="a2527-133">The Users graph tracks how many users accessed the bot using each channel during the specified time frame.</span></span>

![Gráfico de usuarios](~/media/analytics-users.png)

* <span data-ttu-id="a2527-135">El gráfico de porcentaje muestra qué porcentaje de los usuarios utiliza cada canal.</span><span class="sxs-lookup"><span data-stu-id="a2527-135">The percentage chart shows what percentage of users used each channel.</span></span>
* <span data-ttu-id="a2527-136">El gráfico de líneas indica cuántos usuarios accedieron al bot en un momento determinado.</span><span class="sxs-lookup"><span data-stu-id="a2527-136">The line graph indicates how many users were accessing the bot at a certain time.</span></span>
* <span data-ttu-id="a2527-137">La leyenda del gráfico de líneas indica qué color representa a qué canal e incluye el número total de usuarios durante el período de tiempo especificado.</span><span class="sxs-lookup"><span data-stu-id="a2527-137">The legend for the line graph indicates which color represents which channel and the includes the total number of users during the specified time period.</span></span>

### <a name="activities"></a><span data-ttu-id="a2527-138">Actividades</span><span class="sxs-lookup"><span data-stu-id="a2527-138">Activities</span></span>
<span data-ttu-id="a2527-139">El gráfico Actividades realiza el seguimiento de cuántas actividades se han enviado y recibido con cada canal durante el período de tiempo especificado.</span><span class="sxs-lookup"><span data-stu-id="a2527-139">The Activities graph tracks how many activities were sent and received using which channel during the specified time frame.</span></span>

![gráfico de actividades](~/media/analytics-activities.png)

* <span data-ttu-id="a2527-141">El gráfico de porcentaje muestra qué porcentaje de los actividades se comunicó a través de cada canal.</span><span class="sxs-lookup"><span data-stu-id="a2527-141">The percentage chart shows what percentage of activities were communicated over each channel.</span></span>
* <span data-ttu-id="a2527-142">El gráfico de líneas indica cuántas actividades se enviaron y recibieron durante el período de tiempo especificado.</span><span class="sxs-lookup"><span data-stu-id="a2527-142">The line graph indicates how many activities were sent and received over the specified time frame.</span></span>
* <span data-ttu-id="a2527-143">La leyenda del gráfico de líneas indica qué color de línea representa a cada canal y el número total de actividades enviadas y recibidas en ese canal durante el período de tiempo especificado.</span><span class="sxs-lookup"><span data-stu-id="a2527-143">The legend for the line graph indicates which line color represents each channel and the total number of activities sent and received on that channel during the specified time period.</span></span> 

## <a name="enable-analytics"></a><span data-ttu-id="a2527-144">Habilitar análisis</span><span class="sxs-lookup"><span data-stu-id="a2527-144">Enable analytics</span></span>
<span data-ttu-id="a2527-145">Analytics no está disponibles hasta que se haya habilitado y configurado Application Insights.</span><span class="sxs-lookup"><span data-stu-id="a2527-145">Analytics are not available until Application Insights has been enabled and configured.</span></span> <span data-ttu-id="a2527-146">Application Insights comenzará a recopilar los datos tan pronto como esté habilitado.</span><span class="sxs-lookup"><span data-stu-id="a2527-146">Application Insights will begin collecting data as soon as it is enabled.</span></span> <span data-ttu-id="a2527-147">Por ejemplo, si Application Insights se habilitó hace una semana para un bot de seis meses de antigüedad, solo habrá recopilado datos de una semana.</span><span class="sxs-lookup"><span data-stu-id="a2527-147">For example, if Application Insights was enabled a week ago for a six-month-old bot, it will have collected one week of data.</span></span>
> [!NOTE]
> <span data-ttu-id="a2527-148">Analytics requiere tanto una suscripción a Azure como un [recurso](/azure/application-insights/app-insights-create-new-resource) de Application Insights.</span><span class="sxs-lookup"><span data-stu-id="a2527-148">Analytics requires both an Azure subscription and Application Insights [resource](/azure/application-insights/app-insights-create-new-resource).</span></span>
<span data-ttu-id="a2527-149">Para obtener acceso a Application Insights, abra el bot en el [Portal de Bot Framework](https://dev.botframework.com/) y haga clic en **Configuración**.</span><span class="sxs-lookup"><span data-stu-id="a2527-149">To access Application Insights, open the bot in the [Bot Framework Portal](https://dev.botframework.com/) and click **Settings**.</span></span>

1. <span data-ttu-id="a2527-150">Cree un [recurso](/azure/application-insights/app-insights-create-new-resource) de Application Insights.</span><span class="sxs-lookup"><span data-stu-id="a2527-150">Create an Application Insights [resource](/azure/application-insights/app-insights-create-new-resource).</span></span>
2. <span data-ttu-id="a2527-151">Abra el bot en el panel.</span><span class="sxs-lookup"><span data-stu-id="a2527-151">Open the bot in the dashboard.</span></span> <span data-ttu-id="a2527-152">Haga clic en **Configuración** y desplácese hacia abajo hasta la sección **Analytics**.</span><span class="sxs-lookup"><span data-stu-id="a2527-152">Click **Settings** and scroll down to the **Analytics** section.</span></span>
3. <span data-ttu-id="a2527-153">Escriba la información para conectar el bot a Application Insights.</span><span class="sxs-lookup"><span data-stu-id="a2527-153">Enter the information to connect the bot to Application Insights.</span></span> <span data-ttu-id="a2527-154">Todos los campos son obligatorios.</span><span class="sxs-lookup"><span data-stu-id="a2527-154">All fields are required.</span></span>

![Conexión de Insights](~/media/analytics-enable.png)

### <a name="appinsights-instrumentation-key"></a><span data-ttu-id="a2527-156">Clave de instrumentación de AppInsights</span><span class="sxs-lookup"><span data-stu-id="a2527-156">AppInsights Instrumentation Key</span></span>
<span data-ttu-id="a2527-157">Para buscar este valor, abra Application Insights y vaya a **Configurar** > **Propiedades**.</span><span class="sxs-lookup"><span data-stu-id="a2527-157">To find this value, open Application Insights and navigate to **Configure** > **Properties**.</span></span>

### <a name="appinsights-api-key"></a><span data-ttu-id="a2527-158">Clave de API de AppInsights</span><span class="sxs-lookup"><span data-stu-id="a2527-158">AppInsights API key</span></span>
<span data-ttu-id="a2527-159">Proporcione una clave de API de Azure App Insights.</span><span class="sxs-lookup"><span data-stu-id="a2527-159">Provide an Azure App Insights API key.</span></span> <span data-ttu-id="a2527-160">Obtenga información sobre cómo [generar una nueva clave de API](https://dev.applicationinsights.io/documentation/Authorization/API-key-and-App-ID).</span><span class="sxs-lookup"><span data-stu-id="a2527-160">Learn how to [generate a new API key](https://dev.applicationinsights.io/documentation/Authorization/API-key-and-App-ID).</span></span> <span data-ttu-id="a2527-161">Solo se necesita permiso de **Lectura**.</span><span class="sxs-lookup"><span data-stu-id="a2527-161">Only **Read** permission is required.</span></span>

### <a name="appinsights-application-id"></a><span data-ttu-id="a2527-162">Id. de aplicación de AppInsights</span><span class="sxs-lookup"><span data-stu-id="a2527-162">AppInsights Application ID</span></span>
<span data-ttu-id="a2527-163">Para buscar este valor, abra Application Insights y vaya a **Configurar** > **Acceso de API**.</span><span class="sxs-lookup"><span data-stu-id="a2527-163">To find this value, open Application Insights and navigate to **Configure** > **API Access**.</span></span>

<span data-ttu-id="a2527-164">Para obtener más información sobre cómo ubicar estos valores, consulte [Claves de Application Insights](~/bot-service-resources-app-insights-keys.md).</span><span class="sxs-lookup"><span data-stu-id="a2527-164">For more information on how to locate these values, see [Application Insights keys](~/bot-service-resources-app-insights-keys.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a2527-165">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="a2527-165">Additional resources</span></span>
* [<span data-ttu-id="a2527-166">Claves de Application Insights</span><span class="sxs-lookup"><span data-stu-id="a2527-166">Application Insights keys</span></span>](~/bot-service-resources-app-insights-keys.md)