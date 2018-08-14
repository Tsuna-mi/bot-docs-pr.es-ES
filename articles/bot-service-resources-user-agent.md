---
title: Solicitudes de agente de usuario de Bot Framework | Microsoft Docs
description: Descripción de las solicitudes de Bot Framework al servidor web.
author: JohnD-ms
ms.author: v-jodemp
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ms.openlocfilehash: 900881eea35af5f8137b99ccd5e50739342bf5e8
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39306448"
---
# <a name="bot-framework-user-agent-requests"></a><span data-ttu-id="44ad6-103">Solicitudes de agente de usuario de Bot Framework</span><span class="sxs-lookup"><span data-stu-id="44ad6-103">Bot Framework User-Agent requests</span></span>

<span data-ttu-id="44ad6-104">Si está leyendo este mensaje, es probable que haya recibido una solicitud de un servicio de Microsoft Bot Framework.</span><span class="sxs-lookup"><span data-stu-id="44ad6-104">If you’re reading this message, you’ve probably received a request from a Microsoft Bot Framework service.</span></span> <span data-ttu-id="44ad6-105">Esta guía le ayudará a comprender la naturaleza de estas solicitudes y proporcionará los pasos para detenerlas, si es lo que quiere.</span><span class="sxs-lookup"><span data-stu-id="44ad6-105">This guide will help you understand the nature of these requests and provide steps to stop them, if so desired.</span></span>

<span data-ttu-id="44ad6-106">Si ha recibido una solicitud de nuestro servicio, es probable que tuviera un encabezado de agente de usuario con un formato similar al de la cadena siguiente:</span><span class="sxs-lookup"><span data-stu-id="44ad6-106">If you received a request from our service, it likely had a User-Agent header formatted similar to the string below:</span></span>

```User-Agent: BF-DirectLine/3.0 (Microsoft-BotFramework/3.0 +https://botframework.com/ua)```

<span data-ttu-id="44ad6-107">La parte más importante de esta cadena es el identificador **Microsoft-BotFramework**, que usa Microsoft Bot Framework, una colección de herramientas y servicios que permite a los desarrolladores de software independientes crear y trabajar con bots propios.</span><span class="sxs-lookup"><span data-stu-id="44ad6-107">The most important part of this string is the **Microsoft-BotFramework** identifier, which is used by the Microsoft Bot Framework, a collection of tools and services that allows independent software developers to create and operate their own bots.</span></span>

<span data-ttu-id="44ad6-108">Si es un desarrollador de bot, es posible que ya sepa por qué estas solicitudes se dirigen al servicio.</span><span class="sxs-lookup"><span data-stu-id="44ad6-108">If you’re a bot developer, you may already know why these requests are being directed to your service.</span></span> <span data-ttu-id="44ad6-109">Si no es así, siga leyendo para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="44ad6-109">If not, continue reading to learn more.</span></span>

## <a name="why-is-microsoft-contacting-my-service"></a><span data-ttu-id="44ad6-110">¿Por qué Microsoft se pone en contacto con mi servicio?</span><span class="sxs-lookup"><span data-stu-id="44ad6-110">Why is Microsoft contacting my service?</span></span>

<span data-ttu-id="44ad6-111">Bot Framework conecta a los usuarios de servicios de chat como Skype y Facebook Messenger a bots, que son servidores web con API REST que se ejecutan en puntos de conexión a los que se pueda acceder a través de Internet.</span><span class="sxs-lookup"><span data-stu-id="44ad6-111">The Bot Framework connects users on chat services like Skype and Facebook Messenger to bots, which are web servers with REST APIs running on internet-accessible endpoints.</span></span> <span data-ttu-id="44ad6-112">Las llamadas HTTP a los bots (también denominadas llamadas de webhook) solo se envían a las direcciones URL especificadas por un desarrollador de bot que se haya registrado en el portal para desarrolladores de Bot Framework.</span><span class="sxs-lookup"><span data-stu-id="44ad6-112">The HTTP calls to bots (also called webhook calls) are sent only to URLs specified by a bot developer who registered with the Bot Framework developer portal.</span></span>

<span data-ttu-id="44ad6-113">Si recibe solicitudes no solicitadas desde los servicios de Bot Framework a su servicio web, es probable que un desarrollador haya escrito su dirección URL, de forma accidental o deliberada, como devolución de llamada de webhook para su bot.</span><span class="sxs-lookup"><span data-stu-id="44ad6-113">If you’re receiving unsolicited requests from Bot Framework services to your web service, it is likely because a developer has either accidentally or knowingly entered your URL as the webhook callback for their bot.</span></span>

## <a name="to-stop-these-requests"></a><span data-ttu-id="44ad6-114">Para detener estas solicitudes</span><span class="sxs-lookup"><span data-stu-id="44ad6-114">To stop these requests</span></span>

<span data-ttu-id="44ad6-115">Para obtener asistencia con el fin de evitar que las solicitudes no deseadas alcancen el servicio web, póngase en contacto con [bf-reports@microsoft.com](mailto://bf-reports@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="44ad6-115">For assistance in stopping undesired requests from reaching your web service, please contact [bf-reports@microsoft.com](mailto://bf-reports@microsoft.com).</span></span>
