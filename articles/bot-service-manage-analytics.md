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
ms.openlocfilehash: 503e9b2231b198346f5a7cd767a1e6a866e9e5b3
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305101"
---
# <a name="bot-analytics"></a>Análisis del bot
Analytics es una extensión de [Application Insights](/azure/application-insights/app-insights-analytics). Application Insights proporciona datos del **nivel de servicio** y de instrumentación, como tráfico, latencia e integraciones. Analytics proporciona informes de **nivel de conversación** relativos a los datos del usuario, los mensajes y los canales.

## <a name="view-analytics-for-a-bot"></a>Visualización de análisis para un bot
Para obtener acceso a Analytics, abra el bot en el portal para desarrolladores y haga clic en **Analytics**.

¿Demasiados datos? [Habilite y configure el muestreo](/azure/application-insights/app-insights-sampling) para reducir el tráfico y el almacenamiento de telemetría, a la vez que se mantiene un análisis estadísticamente correcto. 

### <a name="specify-channel"></a>Especificación del canal
Elija los canales que aparecen en los gráficos a continuación. Tenga en cuenta que si un bot no está habilitado en un canal, no habrá ningún dato de ese canal.

![Selección del canal](~/media/analytics-channels.png)

* Active la casilla para incluir un canal en el gráfico.
* Desmarque la casilla para quitar un canal del gráfico.

### <a name="specify-time-period"></a>Indicación del período de tiempo
El análisis está disponible para los últimos 90 días únicamente. La recopilación de datos comenzó cuando se habilitó Application Insights.

![Selección del período de tiempo](~/media/analytics-timepick.png)

Haga clic en el menú desplegable y, a continuación, haga clic en la cantidad de tiempo que se deben mostrar los gráficos.
Tenga en cuenta que, al cambiar el período de tiempo general, hará que el incremento de tiempo (eje X) en los gráficos cambié en consecuencia.

### <a name="grand-totals"></a>Totales generales
El número total de usuarios activos y de mensajes enviados y recibidos durante el período de tiempo especificado.
Los guiones `--` indican que no hay ninguna actividad.

### <a name="retention"></a>Retención
Retención realiza un seguimiento de cuántos usuarios que enviaron un mensaje volvieron más adelante y enviaron otro.
El gráfico abarca un plazo móvil de 10 días; los resultados no se ven afectados al cambiar el período de tiempo.

![Gráfico de retención](~/media/analytics-retention.png)

Tenga en cuenta que la fecha más reciente posible es dos días atrás: un usuario envió mensajes anteayer y luego *volvió* ayer.

### <a name="user"></a>Usuario
El gráfico Usuarios realiza un seguimiento de cuántos usuarios han accedido al bot con cada canal durante el período de tiempo especificado.

![Gráfico de usuarios](~/media/analytics-users.png)

* El gráfico de porcentaje muestra qué porcentaje de los usuarios utiliza cada canal.
* El gráfico de líneas indica cuántos usuarios accedieron al bot en un momento determinado.
* La leyenda del gráfico de líneas indica qué color representa a qué canal e incluye el número total de usuarios durante el período de tiempo especificado.

### <a name="messages"></a>error de Hadoop
El gráfico Mensajes realiza el seguimiento de cuántos mensajes se han enviado y recibido con cada canal durante el período de tiempo especificado.

![Gráfico de mensajes](~/media/analytics-messages.png)

* El gráfico de porcentaje muestra qué porcentaje de los mensajes se comunicó a través de cada canal.
* El gráfico de líneas indica cuántos mensajes se enviaron y recibieron durante el período de tiempo especificado.
* La leyenda del gráfico de líneas indica qué color de línea representa a cada canal y el número total de mensajes enviados y recibidos en ese canal durante el período de tiempo especificado. 

## <a name="enable-analytics"></a>Habilitar análisis
Analytics no está disponibles hasta que se haya habilitado y configurado Application Insights. Application Insights comenzará a recopilar los datos tan pronto como esté habilitado. Por ejemplo, si Application Insights se habilitó hace una semana para un bot de seis meses de antigüedad, solo habrá recopilado datos de una semana.
> [!NOTE]
> Analytics requiere tanto una suscripción a Azure como un [recurso](/azure/application-insights/app-insights-create-new-resource) de Application Insights.
Para obtener acceso a Application Insights, abra el bot en el [Portal de Bot Framework](https://dev.botframework.com/) y haga clic en **Configuración**.

1. Cree un [recurso](/azure/application-insights/app-insights-create-new-resource) de Application Insights.
2. Abra el bot en el panel. Haga clic en **Configuración** y desplácese hacia abajo hasta la sección **Analytics**.
3. Escriba la información para conectar el bot a Application Insights. Todos los campos son obligatorios.

![Conexión de Insights](~/media/analytics-enable.png)

### <a name="appinsights-instrumentation-key"></a>Clave de instrumentación de AppInsights
Para buscar este valor, abra Application Insights y vaya a **Configurar** > **Propiedades**.

### <a name="appinsights-api-key"></a>Clave de API de AppInsights
Proporcione una clave de API de Azure App Insights. Obtenga información sobre cómo [generar una nueva clave de API](https://dev.applicationinsights.io/documentation/Authorization/API-key-and-App-ID). Solo se necesita permiso de **Lectura**.

### <a name="appinsights-application-id"></a>Id. de aplicación de AppInsights
Para buscar este valor, abra Application Insights y vaya a **Configurar** > **Acceso de API**.

Para obtener más información sobre cómo ubicar estos valores, consulte [Claves de Application Insights](~/bot-service-resources-app-insights-keys.md).

## <a name="additional-resources"></a>Recursos adicionales
* [Claves de Application Insights](~/bot-service-resources-app-insights-keys.md)