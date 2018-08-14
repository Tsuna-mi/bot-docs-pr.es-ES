---
title: Conexión de un bot a Twilio | Microsoft Docs
description: Obtenga información sobre cómo configurar la conexión de un bot a Twilio.
keywords: Twilio, canales de bot, SMS, aplicación, teléfono, configurar Twilio, comunicación en la nube, texto
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ms.openlocfilehash: 7e09126d50cfbebfc0aad0ee7fcb71b4e7551a7d
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39304860"
---
# <a name="connect-a-bot-to-twilio"></a><span data-ttu-id="777f0-104">Conexión de un bot a Twilio</span><span class="sxs-lookup"><span data-stu-id="777f0-104">Connect a bot to Twilio</span></span>

<span data-ttu-id="777f0-105">Puede configurar su bot para que se comunique con las personas mediante la plataforma de comunicaciones en la nube de Twilio.</span><span class="sxs-lookup"><span data-stu-id="777f0-105">You can configure your bot to communicate with people using the Twilio cloud communication platform.</span></span>

## <a name="log-in-to-or-create-a-twilio-account-for-sending-and-receiving-sms-messages"></a><span data-ttu-id="777f0-106">Iniciar sesión en Twilio o crear una cuenta para enviar y recibir mensajes SMS</span><span class="sxs-lookup"><span data-stu-id="777f0-106">Log in to or create a Twilio account for sending and receiving SMS messages</span></span>

<span data-ttu-id="777f0-107">Si no tiene ninguna cuenta de Twilio, <a href="https://www.twilio.com/try-twilio" target="_blank">cree una nueva cuenta</a>.</span><span class="sxs-lookup"><span data-stu-id="777f0-107">If you don't have a Twilio account, <a href="https://www.twilio.com/try-twilio" target="_blank">create a new account</a></span></span>

## <a name="create-a-twiml-application"></a><span data-ttu-id="777f0-108">Crear una aplicación de TwiML</span><span class="sxs-lookup"><span data-stu-id="777f0-108">Create a TwiML Application</span></span>

<span data-ttu-id="777f0-109"><a href="https://www.twilio.com/user/account/messaging/dev-tools/twiml-apps/add" target="_blank">Crear una aplicación de TwiML</a></span><span class="sxs-lookup"><span data-stu-id="777f0-109"><a href="https://www.twilio.com/user/account/messaging/dev-tools/twiml-apps/add" target="_blank">Create a TwiML application</a></span></span>

![Crear la aplicación](~/media/channels/twi-StepTwiml.png)

 <span data-ttu-id="777f0-111">En Mensajería, la dirección URL de solicitud debe ser **https://sms.botframework.com/api/sms**.</span><span class="sxs-lookup"><span data-stu-id="777f0-111">Under Messaging, the Request URL should be **https://sms.botframework.com/api/sms**.</span></span>

## <a name="select-or-add-a-phone-number"></a><span data-ttu-id="777f0-112">Seleccionar o agregar un número de teléfono</span><span class="sxs-lookup"><span data-stu-id="777f0-112">Select or add a phone number</span></span>

<span data-ttu-id="777f0-113"><a href="https://www.twilio.com/user/account/phone-numbers/incoming" target="_blank">Seleccionar o agregar un número de teléfono</a>.</span><span class="sxs-lookup"><span data-stu-id="777f0-113"><a href="https://www.twilio.com/user/account/phone-numbers/incoming" target="_blank">Select or add a phone number</a>.</span></span> <span data-ttu-id="777f0-114">Haga clic en el número para agregarlo a la aplicación de TwiML que creó.</span><span class="sxs-lookup"><span data-stu-id="777f0-114">Click  the number to add it to the TwiML application you created.</span></span>

![Establecer el número de teléfono](~/media/channels/twi-StepPhone.png)

## <a name="specify-application-to-use-for-messaging"></a><span data-ttu-id="777f0-116">Especificar la aplicación que se usará para mensajería</span><span class="sxs-lookup"><span data-stu-id="777f0-116">Specify application to use for Messaging</span></span>
<span data-ttu-id="777f0-117">En la sección **Mensajería**, establezca **TwiML App** con el nombre de la aplicación TwiML que acaba de crear.</span><span class="sxs-lookup"><span data-stu-id="777f0-117">In the **Messaging** section, set the **TwiML App** to the name of the TwiML App you just created.</span></span>
<span data-ttu-id="777f0-118">Copie el valor **Número de teléfono** para usarlo más adelante.</span><span class="sxs-lookup"><span data-stu-id="777f0-118">Copy the **Phone Number** value for later use.</span></span>

![Especificar la aplicación](~/media/channels/twi-StepPhone2.png)

## <a name="gather-credentials"></a><span data-ttu-id="777f0-120">Obtener las credenciales</span><span class="sxs-lookup"><span data-stu-id="777f0-120">Gather credentials</span></span>

<span data-ttu-id="777f0-121"><a href="https://www.twilio.com/user/account/settings" target="_blank">Obtenga las credenciales</a> y luego haga clic en el icono en forma de "ojo" para ver el token de autenticación.</span><span class="sxs-lookup"><span data-stu-id="777f0-121"><a href="https://www.twilio.com/user/account/settings" target="_blank">Gather credentials</a> and then click the "eye" icon to see the Auth Token.</span></span>

![Obtener las credenciales de la aplicación](~/media/channels/twi-StepAuth.png)

## <a name="submit-credentials"></a><span data-ttu-id="777f0-123">Enviar credenciales</span><span class="sxs-lookup"><span data-stu-id="777f0-123">Submit credentials</span></span>

<span data-ttu-id="777f0-124">Escriba el número de teléfono, el valor de accountSID y el token de autenticación que copió anteriormente y haga clic en **Submit Twilio Credentials** (Enviar credenciales de Twilio).</span><span class="sxs-lookup"><span data-stu-id="777f0-124">Enter the phone number, accountSID, and Auth Token you copied earlier and click **Submit Twilio Credentials**.</span></span>

## <a name="enable-the-bot"></a><span data-ttu-id="777f0-125">Habilitar el bot</span><span class="sxs-lookup"><span data-stu-id="777f0-125">Enable the bot</span></span>
<span data-ttu-id="777f0-126">Seleccione **Enable this bot on SMS** (Habilitar este bot en SMS).</span><span class="sxs-lookup"><span data-stu-id="777f0-126">Check **Enable this bot on SMS**.</span></span> <span data-ttu-id="777f0-127">A continuación, haga clic en **I'm done configuring SMS** (Finalicé la configuración de SMS).</span><span class="sxs-lookup"><span data-stu-id="777f0-127">Then click **I'm done configuring SMS**.</span></span>

<span data-ttu-id="777f0-128">Cuando haya completado estos pasos, el bot se configurará correctamente para comunicarse con los usuarios mediante Twilio.</span><span class="sxs-lookup"><span data-stu-id="777f0-128">When you have completed these steps, your bot will be successfully configured to communicate with users using Twilio.</span></span>

