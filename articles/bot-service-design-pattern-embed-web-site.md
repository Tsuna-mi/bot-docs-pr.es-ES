---
title: Insertar un bot en un sitio web | Microsoft Docs
description: Obtenga información sobre cómo diseñar un bot que se insertará en un sitio web.
author: matvelloso
ms.author: mateusv
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ms.openlocfilehash: a19145c446c74468cef3ae5d9abf6e90e91196ff
ms.sourcegitcommit: f0b22c6286e44578c11c9f15d22b542c199f0024
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2018
ms.locfileid: "47403982"
---
# <a name="embed-a-bot-in-a-website"></a><span data-ttu-id="659b9-103">Insertar un bot en un sitio web</span><span class="sxs-lookup"><span data-stu-id="659b9-103">Embed a bot in a website</span></span>

<span data-ttu-id="659b9-104">Aunque los bots comúnmente existen fuera de los sitios web, también pueden estar integrados en estos.</span><span class="sxs-lookup"><span data-stu-id="659b9-104">Although bots commonly exist outside of websites, they can also be embedded within a website.</span></span> <span data-ttu-id="659b9-105">Por ejemplo, puede insertar un [bot de conocimiento](~/bot-service-design-pattern-knowledge-base.md) dentro de un sitio web para que los usuarios puedan encontrar información que, de lo contrario, podría ser difícil de ubicar dentro de las estructuras complejas del sitio web.</span><span class="sxs-lookup"><span data-stu-id="659b9-105">For example, you may embed a [knowledge bot](~/bot-service-design-pattern-knowledge-base.md) within a website to enable users to quickly find information that might otherwise be challenging to locate within complex website structures.</span></span> <span data-ttu-id="659b9-106">Asimismo, también podría insertar un bot dentro de un sitio web del departamento de soporte técnico para que actúe como primer respondedor a solicitudes entrantes de los usuarios.</span><span class="sxs-lookup"><span data-stu-id="659b9-106">Or you might embed a bot within a help desk website to act as the first responder to incoming user requests.</span></span> <span data-ttu-id="659b9-107">El bot puede resolver problemas sencillos de manera independiente y [derivar](~/bot-service-design-pattern-handoff-human.md) problemas más complejos a un agente humano.</span><span class="sxs-lookup"><span data-stu-id="659b9-107">The bot could independently resolve simple issues and [handoff](~/bot-service-design-pattern-handoff-human.md) more complex issues to a human agent.</span></span> 

<span data-ttu-id="659b9-108">En este artículo se explora la integración de bots con sitios web y el proceso de utilizar el mecanismo *backchannel* para facilitar la comunicación privada entre una página web y un bot.</span><span class="sxs-lookup"><span data-stu-id="659b9-108">This article explores integrating bots with websites and the process of using the *backchannel* mechanism to facilitate private communication between a web page and a bot.</span></span> 

<span data-ttu-id="659b9-109">Microsoft proporciona dos formas diferentes de integrar un bot en un sitio web: el control web de Skype y un control web de código abierto.</span><span class="sxs-lookup"><span data-stu-id="659b9-109">Microsoft provides two different ways to integrate a bot in a website: the Skype web control and an open source web control.</span></span>

## <a name="skype-web-control"></a><span data-ttu-id="659b9-110">Control web de Skype</span><span class="sxs-lookup"><span data-stu-id="659b9-110">Skype web control</span></span>

<span data-ttu-id="659b9-111">El control web de Skype es esencialmente un cliente de Skype en un control habilitado para la web.</span><span class="sxs-lookup"><span data-stu-id="659b9-111">The Skype web control is essentially a Skype client in a web-enabled control.</span></span> <span data-ttu-id="659b9-112">La autenticación integrada de Skype permite que el robot autentique y reconozca a los usuarios, sin que el desarrollador tenga que escribir ningún código personalizado.</span><span class="sxs-lookup"><span data-stu-id="659b9-112">Built-in Skype authentication enables the bot to authenticate and recognize users, without requiring the developer to write any custom code.</span></span> <span data-ttu-id="659b9-113">Skype reconocerá automáticamente las cuentas de Microsoft utilizadas en su cliente web.</span><span class="sxs-lookup"><span data-stu-id="659b9-113">Skype will automatically recognize Microsoft Accounts used in its web client.</span></span> 

<span data-ttu-id="659b9-114">Debido a que el control web de Skype simplemente actúa como interfaz para Skype, el cliente de Skype del usuario tiene acceso  automático al contexto completo de cualquier conversación que facilite el control web.</span><span class="sxs-lookup"><span data-stu-id="659b9-114">Because the Skype web control simply acts as a front-end for Skype, the user's Skype client automatically has access to the full context of any conversation that the web control facilitates.</span></span> <span data-ttu-id="659b9-115">Incluso después de que se cierre el navegador web, el usuario puede continuar interactuando con el bot mediante el cliente de Skype.</span><span class="sxs-lookup"><span data-stu-id="659b9-115">Even after the web browser is closed, the user may continue to interact with the bot using the Skype client.</span></span> 

## <a name="open-source-web-control"></a><span data-ttu-id="659b9-116">Control web de código abierto</span><span class="sxs-lookup"><span data-stu-id="659b9-116">Open source web control</span></span>

<span data-ttu-id="659b9-117">El <a href="https://aka.ms/BotFramework-WebChat" target="_blank">control de chat en web de código abierto</a> se basa en ReactJS y usa [Direct Line API][directLineAPI] para comunicarse con Bot Framework.</span><span class="sxs-lookup"><span data-stu-id="659b9-117">The <a href="https://aka.ms/BotFramework-WebChat" target="_blank">open source web chat control</a> is based upon ReactJS and uses the [Direct Line API][directLineAPI] to communicate with the Bot Framework.</span></span> <span data-ttu-id="659b9-118">El control de chat en web proporciona un lienzo en blanco para implementar el chat en web, lo que le proporciona un control total sobre el comportamiento que tendrá y la experiencia del usuario que ofrecerá.</span><span class="sxs-lookup"><span data-stu-id="659b9-118">The web chat control provides a blank canvas for implementing the web chat, giving you full control over its behaviors and the user experience that it delivers.</span></span> 

<span data-ttu-id="659b9-119">El mecanismo *backchannel* permite que la página web que aloja el control se comunique directamente con el bot de una manera totalmente invisible para el usuario.</span><span class="sxs-lookup"><span data-stu-id="659b9-119">The *backchannel* mechanism enables the web page that is hosting the control to communicate directly with the bot in a manner that is entirely invisible to the user.</span></span> <span data-ttu-id="659b9-120">Esta capacidad proporciona cierta cantidad de escenarios útiles:</span><span class="sxs-lookup"><span data-stu-id="659b9-120">This capability enables a number of useful scenarios:</span></span> 

- <span data-ttu-id="659b9-121">La página web puede enviar datos relevantes al bot (por ejemplo, la ubicación GPS).</span><span class="sxs-lookup"><span data-stu-id="659b9-121">The web page can send relevant data to the bot (e.g., GPS location).</span></span>
- <span data-ttu-id="659b9-122">La página web puede asesorar al bot sobre las acciones del usuario (por ejemplo, "el usuario acaba de seleccionar la opción A del menú desplegable").</span><span class="sxs-lookup"><span data-stu-id="659b9-122">The web page can advise the bot about user actions (e.g., "user just selected Option A from the dropdown").</span></span>
- <span data-ttu-id="659b9-123">La página web puede enviar el bot al token de autenticación de un usuario conectado.</span><span class="sxs-lookup"><span data-stu-id="659b9-123">The web page can send the bot the auth token for a logged-in user.</span></span>
- <span data-ttu-id="659b9-124">El bot puede enviar datos relevantes a la página web (por ejemplo, el valor actual de la cartera del usuario).</span><span class="sxs-lookup"><span data-stu-id="659b9-124">The bot can send relevant data to the web page (e.g., current value of user's portfolio).</span></span>
- <span data-ttu-id="659b9-125">El bot puede enviar "comandos" a la página web (por ejemplo, cambiar el color de fondo).</span><span class="sxs-lookup"><span data-stu-id="659b9-125">The bot can send "commands" to the web page (e.g., change background color).</span></span>

## <a name="using-the-backchannel-mechanism"></a><span data-ttu-id="659b9-126">Usar el mecanismo backchannel</span><span class="sxs-lookup"><span data-stu-id="659b9-126">Using the backchannel mechanism</span></span>

[!INCLUDE [Introduction to backchannel mechanism](~/includes/snippet-backchannel.md)]

## <a name="sample-code"></a><span data-ttu-id="659b9-127">Código de ejemplo</span><span class="sxs-lookup"><span data-stu-id="659b9-127">Sample code</span></span>

<span data-ttu-id="659b9-128">El <a href="https://aka.ms/BotFramework-WebChat" target="_blank">control de chat en web de código abierto </a> está disponible en GitHub.</span><span class="sxs-lookup"><span data-stu-id="659b9-128">The <a href="https://aka.ms/BotFramework-WebChat" target="_blank">open source web chat control</a> is available via GitHub.</span></span> <span data-ttu-id="659b9-129">Para obtener más detalles sobre cómo puede implementar el mecanismo backchannel mediante el control de chat en web de código abierto y el SDK de Bot Builder para Node.js, consulte [Use the backchannel mechanism](~/nodejs/bot-builder-nodejs-backchannel.md) (Usar el mecanismo backchannel).</span><span class="sxs-lookup"><span data-stu-id="659b9-129">For details about how you can implement the backchannel mechanism using the open source web chat control and the Bot Builder SDK for Node.js, see [Use the backchannel mechanism](~/nodejs/bot-builder-nodejs-backchannel.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="659b9-130">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="659b9-130">Additional resources</span></span>

- <span data-ttu-id="659b9-131">[Direct Line API][directLineAPI] (API de Direct Line)</span><span class="sxs-lookup"><span data-stu-id="659b9-131">[Direct Line API][directLineAPI]</span></span>
- [<span data-ttu-id="659b9-132">Control de chat en web de código abierto</span><span class="sxs-lookup"><span data-stu-id="659b9-132">Open source web chat control</span></span>](https://github.com/Microsoft/BotFramework-WebChat)
- <span data-ttu-id="659b9-133">[Use the backchannel mechanism](~/nodejs/bot-builder-nodejs-backchannel.md) (Usar el mecanismo backchannel)</span><span class="sxs-lookup"><span data-stu-id="659b9-133">[Use the backchannel mechanism](~/nodejs/bot-builder-nodejs-backchannel.md)</span></span>

[directLineAPI]: https://docs.botframework.com/en-us/restapi/directline3/#navtitle
