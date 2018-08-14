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
# <a name="connect-a-bot-to-twilio"></a>Conexión de un bot a Twilio

Puede configurar su bot para que se comunique con las personas mediante la plataforma de comunicaciones en la nube de Twilio.

## <a name="log-in-to-or-create-a-twilio-account-for-sending-and-receiving-sms-messages"></a>Iniciar sesión en Twilio o crear una cuenta para enviar y recibir mensajes SMS

Si no tiene ninguna cuenta de Twilio, <a href="https://www.twilio.com/try-twilio" target="_blank">cree una nueva cuenta</a>.

## <a name="create-a-twiml-application"></a>Crear una aplicación de TwiML

<a href="https://www.twilio.com/user/account/messaging/dev-tools/twiml-apps/add" target="_blank">Crear una aplicación de TwiML</a>

![Crear la aplicación](~/media/channels/twi-StepTwiml.png)

 En Mensajería, la dirección URL de solicitud debe ser **https://sms.botframework.com/api/sms**.

## <a name="select-or-add-a-phone-number"></a>Seleccionar o agregar un número de teléfono

<a href="https://www.twilio.com/user/account/phone-numbers/incoming" target="_blank">Seleccionar o agregar un número de teléfono</a>. Haga clic en el número para agregarlo a la aplicación de TwiML que creó.

![Establecer el número de teléfono](~/media/channels/twi-StepPhone.png)

## <a name="specify-application-to-use-for-messaging"></a>Especificar la aplicación que se usará para mensajería
En la sección **Mensajería**, establezca **TwiML App** con el nombre de la aplicación TwiML que acaba de crear.
Copie el valor **Número de teléfono** para usarlo más adelante.

![Especificar la aplicación](~/media/channels/twi-StepPhone2.png)

## <a name="gather-credentials"></a>Obtener las credenciales

<a href="https://www.twilio.com/user/account/settings" target="_blank">Obtenga las credenciales</a> y luego haga clic en el icono en forma de "ojo" para ver el token de autenticación.

![Obtener las credenciales de la aplicación](~/media/channels/twi-StepAuth.png)

## <a name="submit-credentials"></a>Enviar credenciales

Escriba el número de teléfono, el valor de accountSID y el token de autenticación que copió anteriormente y haga clic en **Submit Twilio Credentials** (Enviar credenciales de Twilio).

## <a name="enable-the-bot"></a>Habilitar el bot
Seleccione **Enable this bot on SMS** (Habilitar este bot en SMS). A continuación, haga clic en **I'm done configuring SMS** (Finalicé la configuración de SMS).

Cuando haya completado estos pasos, el bot se configurará correctamente para comunicarse con los usuarios mediante Twilio.

