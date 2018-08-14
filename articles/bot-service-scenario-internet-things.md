---
title: Escenario de bot de Internet de las cosas | Microsoft Docs
description: Explore el escenario de bot de Internet de las cosas con Bot Framework.
author: BrianRandell
ms.author: v-brra
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: 3b65f323427760fa43586f471aefb6811ef3e675
ms.sourcegitcommit: 67445b42796d90661afc643c6bb6533e9a662cbc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2018
ms.locfileid: "39574531"
---
# <a name="internet-of-things-iot-bot-scenario"></a><span data-ttu-id="c48f6-103">Escenarios de bot de Internet de las cosas (IoT)</span><span class="sxs-lookup"><span data-stu-id="c48f6-103">Internet of Things (IoT) Bot Scenario</span></span>

[!INCLUDE [pre-release-label](includes/pre-release-label-v3.md)]

<span data-ttu-id="c48f6-104">Este bot de Internet de las cosas (IoT) le hace más fácil la tarea de controlar los dispositivos de su casa, por ejemplo, una luz Philips Hue mediante comandos de voz o de chat interactivo.</span><span class="sxs-lookup"><span data-stu-id="c48f6-104">This Internet of Things (IoT) Bot makes it easy for you to control devices around your home, such as a Philips Hue light using voice or interactive chat commands.</span></span>

<span data-ttu-id="c48f6-105">A la gente le encanta hablar con sus cosas.</span><span class="sxs-lookup"><span data-stu-id="c48f6-105">People love to talk to their things.</span></span> <span data-ttu-id="c48f6-106">Desde los días del primer mando para TV, a las personas les ha encantado no tener que moverse para interactuar con su entorno.</span><span class="sxs-lookup"><span data-stu-id="c48f6-106">Since the days of the first TV remote, people have loved not having to move to affect their environment.</span></span> <span data-ttu-id="c48f6-107">Este bot de IoT permite a una persona administrar una luz Philips Hue mediante comandos simples de chat o voz.</span><span class="sxs-lookup"><span data-stu-id="c48f6-107">This IoT bot allows a person to manage a Philips Hue by simple chat commands or voice.</span></span> <span data-ttu-id="c48f6-108">Además, al utilizar el chat, una persona puede recibir opciones visuales relacionadas con los colores para elegir.</span><span class="sxs-lookup"><span data-stu-id="c48f6-108">In addition, when using chat, a person can be given visual choices related to colors to pick.</span></span>

![Diagrama del bot de Internet de las cosas](~/media/scenarios/bot-service-scenario-iot-bot.png)

<span data-ttu-id="c48f6-110">Este es el flujo lógico de un bot de IoT:</span><span class="sxs-lookup"><span data-stu-id="c48f6-110">Here is the logic flow of an IoT bot:</span></span>

1. <span data-ttu-id="c48f6-111">El usuario se registra en Skype y accede al bot de IoT.</span><span class="sxs-lookup"><span data-stu-id="c48f6-111">The user logs into Skype and accesses the IoT bot.</span></span>
2. <span data-ttu-id="c48f6-112">El usuario pide por voz al bot que encienda las luces a través del dispositivo IoT.</span><span class="sxs-lookup"><span data-stu-id="c48f6-112">Using voice, the user asks the bot to turn on the lights via the IoT device.</span></span>
3. <span data-ttu-id="c48f6-113">La solicitud se retransmite a un servicio de terceros que tiene acceso a la red del dispositivo IoT.</span><span class="sxs-lookup"><span data-stu-id="c48f6-113">The request is relayed to a 3rd party service that has access to the IoT device network.</span></span>
4. <span data-ttu-id="c48f6-114">Los resultados del comando se devuelven al usuario.</span><span class="sxs-lookup"><span data-stu-id="c48f6-114">The results of the command are returned to the user.</span></span>
5. <span data-ttu-id="c48f6-115">Application Insights recopila telemetría de tiempo de ejecución para facilitar el desarrollo con el uso y el rendimiento del bot.</span><span class="sxs-lookup"><span data-stu-id="c48f6-115">Application insights gathers runtime telemetery to help development with bot performance and usage.</span></span>

## <a name="sample-bot"></a><span data-ttu-id="c48f6-116">Bot de ejemplo</span><span class="sxs-lookup"><span data-stu-id="c48f6-116">Sample bot</span></span>
<span data-ttu-id="c48f6-117">El bot de IoT le permitirá usar rápidamente comandos de chat de canales como Skype o Slack para controlar la luz Hue.</span><span class="sxs-lookup"><span data-stu-id="c48f6-117">The IoT bot will allow you to quickly use chat commands from channels like Skype or Slack to control your Hue.</span></span> <span data-ttu-id="c48f6-118">Para facilitar el acceso remoto, llamará a applets IFTTT predefinidos para que funcionen con Hue.</span><span class="sxs-lookup"><span data-stu-id="c48f6-118">To facilitate remote access, you'll call IFTTT applets predefined to work with Hue.</span></span>

<span data-ttu-id="c48f6-119">Puede descargar o clonar el código fuente de este bot de ejemplo de [Samples for Common Bot Framework Scenarios](https://aka.ms/bot/scenarios).</span><span class="sxs-lookup"><span data-stu-id="c48f6-119">You can download or clone the source code for this sample bot from [Samples for Common Bot Framework Scenarios](https://aka.ms/bot/scenarios).</span></span>

## <a name="components-youll-use"></a><span data-ttu-id="c48f6-120">Componentes que va a utilizar</span><span class="sxs-lookup"><span data-stu-id="c48f6-120">Components you'll use</span></span>
<span data-ttu-id="c48f6-121">El bot de Internet de las cosas (IoT) usa los siguientes componentes:</span><span class="sxs-lookup"><span data-stu-id="c48f6-121">The Internet of Things (IoT) Bot uses the following components:</span></span>
-   <span data-ttu-id="c48f6-122">Philips Hue</span><span class="sxs-lookup"><span data-stu-id="c48f6-122">Philips Hue</span></span>
-   <span data-ttu-id="c48f6-123">IFTTT (si esto, entonces aquello)</span><span class="sxs-lookup"><span data-stu-id="c48f6-123">If This Then That (IFTTT)</span></span>
-   <span data-ttu-id="c48f6-124">Application Insights</span><span class="sxs-lookup"><span data-stu-id="c48f6-124">Application Insights</span></span>

### <a name="philips-hue"></a><span data-ttu-id="c48f6-125">Philips Hue</span><span class="sxs-lookup"><span data-stu-id="c48f6-125">Philips Hue</span></span>
<span data-ttu-id="c48f6-126">Las bombillas y el puente conectados Philips Hue le permite asumir el control completo de la iluminación.</span><span class="sxs-lookup"><span data-stu-id="c48f6-126">Philips Hue connected bulbs and bridge let you to take full control of your lighting.</span></span> <span data-ttu-id="c48f6-127">No importa lo que quiera hacer con la iluminación, Hue puede encargarse.</span><span class="sxs-lookup"><span data-stu-id="c48f6-127">Whatever you want to do with your lighting, Hue can.</span></span> <span data-ttu-id="c48f6-128">Hue tiene una API que usted puede usar desde la red local.</span><span class="sxs-lookup"><span data-stu-id="c48f6-128">Hue has an API you can use from your local network.</span></span> <span data-ttu-id="c48f6-129">Sin embargo, desea poder acceder a los dispositivos y luces Hue controlados desde cualquier lugar mediante una interfaz sencilla de bot.</span><span class="sxs-lookup"><span data-stu-id="c48f6-129">However, you want to be able to access your Hue controlled devices and lights from anywhere using a friendly Bot interface.</span></span> <span data-ttu-id="c48f6-130">Por lo tanto, accederá a Hue mediante IFTTT.</span><span class="sxs-lookup"><span data-stu-id="c48f6-130">Thus you'll access Hue via IFTTT.</span></span>

### <a name="ifttt"></a><span data-ttu-id="c48f6-131">IFTTT</span><span class="sxs-lookup"><span data-stu-id="c48f6-131">IFTTT</span></span>
<span data-ttu-id="c48f6-132">IFTTT es un servicio gratuito basado en web que las personas usan para crear cadenas de instrucciones condicionales sencillas, llamadas applets.</span><span class="sxs-lookup"><span data-stu-id="c48f6-132">IFTTT is a free web-based service that people use to create chains of simple conditional statements, called applets.</span></span> <span data-ttu-id="c48f6-133">Puede desencadenar un applet desde el bot para que haga algo en su nombre.</span><span class="sxs-lookup"><span data-stu-id="c48f6-133">You can trigger an applet from your Bot to have it do something on your behalf.</span></span> <span data-ttu-id="c48f6-134">Hay una serie de applets de Hue predefinidos disponibles para encender y apagar las luces, cambiar la escena y mucho más.</span><span class="sxs-lookup"><span data-stu-id="c48f6-134">There are a number of predefined Hue applets available to turn lights on and off, change the scene, and more.</span></span>

### <a name="application-insights"></a><span data-ttu-id="c48f6-135">Application Insights</span><span class="sxs-lookup"><span data-stu-id="c48f6-135">Application Insights</span></span>
<span data-ttu-id="c48f6-136">Application Insights le ayuda a obtener un conocimiento práctico y detallado mediante la administración del rendimiento de las aplicaciones (APM) y los análisis instantáneos.</span><span class="sxs-lookup"><span data-stu-id="c48f6-136">Application Insights helps you get actionable insights through application performance management (APM) and instant analytics.</span></span> <span data-ttu-id="c48f6-137">Obtiene de manera inmediata supervisión del rendimiento, alertas muy eficaces y paneles muy fáciles de usar que le ayudarán a garantizar que el bot esté disponible y con el rendimiento que espera de él.</span><span class="sxs-lookup"><span data-stu-id="c48f6-137">Out of the box you get rich performance monitoring, powerful alerting, and easy-to-consume dashboards to help ensure your Bot is available and performing as you expect.</span></span> <span data-ttu-id="c48f6-138">Puede ver rápidamente si hay algún problema y luego llevar a cabo un análisis de la causa principal para detectar el error y corregirlo.</span><span class="sxs-lookup"><span data-stu-id="c48f6-138">You can quickly see if you have a problem, then perform a root cause analysis to find and fix the issue.</span></span>
