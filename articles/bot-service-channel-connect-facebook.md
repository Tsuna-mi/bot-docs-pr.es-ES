---
title: Conexión de un bot a Facebook Messenger | Microsoft Docs
description: Obtenga información sobre cómo configurar la conexión de un bot a Facebook Messenger.
keywords: Facebook Messenger, canal de bot, aplicación de Facebook, id. de aplicación, secreto de aplicación, bot de Facebook, credenciales
author: RobStand
ms.author: RobStand
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 09/16/2018
ms.openlocfilehash: 60e39bb652ab5b7ffeeb5ba53bdf4c82f936553e
ms.sourcegitcommit: 3bf3dbb1a440b3d83e58499c6a2ac116fe04b2f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/23/2018
ms.locfileid: "46707871"
---
# <a name="connect-a-bot-to-facebook-messenger"></a>Conexión de un bot a Facebook Messenger

Para obtener más información acerca del desarrollo de Facebook Messenger, consulte la [documentación de la plataforma de Messenger](https://developers.facebook.com/docs/messenger-platform). Puede que quiera revisar la [lista de verificación previa al lanzamiento](https://developers.facebook.com/docs/messenger-platform/product-overview/launch#app_public), el [tutorial de inicio rápido](https://developers.facebook.com/docs/messenger-platform/guides/quick-start) y la [guía de configuración](https://developers.facebook.com/docs/messenger-platform/guides/setup) de Facebook.

Para configurar un bot para que se comunique con Facebook Messenger, habilite Facebook Messenger en una página de Facebook y, a continuación, conecte el bot a la aplicación.

> [!NOTE]
> La UI de Facebook puede aparecer de forma ligeramente diferente dependiendo de la versión que use.

## <a name="copy-the-page-id"></a>Copia del id. de página

Se accede al bot a través de una página de Facebook. [Cree una nueva página de Facebook](https://www.facebook.com/bookmarks/pages) o vaya a una página existente.

* Abra la página **Sobre** de la página de Facebook y, a continuación, copie y guarde el **id. de página**.

## <a name="create-a-facebook-app"></a>Creación de una aplicación de Facebook

[Cree una nueva aplicación de Facebook](https://developers.facebook.com/quickstarts/?platform=web) en la página y genere un id. y un secreto de aplicación.

![Creación de un id. de aplicación](~/media/channels/FB-CreateAppId.png)

* Copie y guarde el **id. de aplicación** y el **secreto de aplicación**.

![Guardar el id. y el secreto de aplicación](~/media/channels/FB-get-appid.png)

Establezca el control deslizante "Allow API Access to App Settings" (Permitir el acceso de la API a la configuración de la aplicación) en "Sí".

![Configuración de la aplicación](~/media/bot-service-channel-connect-facebook/api_settings.png)

## <a name="enable-messenger"></a>Habilitación de Messenger


Habilite Facebook Messenger en la nueva aplicación de Facebook.

* En la página **Configuración del producto** de la aplicación, haga clic en **Comenzar** y, a continuación, vuelva a hacer clic en **Comenzar**.


![Habilitación de Messenger](~/media/channels/FB-AddMessaging1.png)

## <a name="generate-a-page-access-token"></a>Generación de un token de acceso a la página

En el panel **Generación de tokens** de la sección de Messenger, seleccione la página de destino. Se generará un token de acceso a la página.

* Copie y guarde el **token de acceso a la página**.

![Generación del token](~/media/channels/FB-generateToken.png)

## <a name="enable-webhooks"></a>Habilitación de webhooks

Haga clic en **Configurar webhooks** para reenviar eventos de mensajería de Facebook Messenger al bot.

![Habilitación del webhook](~/media/channels/FB-webhook.png)

## <a name="provide-webhook-callback-url-and-verify-token"></a>Aprovisionamiento de la dirección URL de devolución de llamada de webhook y comprobación del token

En [Azure Portal](https://portal.azure.com/), abra el bot, haga clic en la pestaña **Canales** y, a continuación, haga clic en **Facebook Messenger**.

* Copie los valores **Dirección URL de devolución de llamada** y **Comprobar token** desde el portal.

![Copia de valores](~/media/channels/fb-callbackVerify.png)

1. Vuelva a Facebook Messenger y pegue los valores **Dirección URL de devolución de llamada** y **Comprobar token**.

2. En **Campos de suscripción**, seleccione *message\_deliveries*, *messages*, *messaging\_options* y *messaging\_postbacks*.

3. Haga clic en **Comprobar y guardar**.

![Configuración del webhook](~/media/channels/FB-webhookConfig.png)

4. Subscriba el webhook a la página de Facebook.

![Suscripción del webhook](~/media/bot-service-channel-connect-facebook/subscribe-webhook.png)


## <a name="provide-facebook-credentials"></a>Aprovisionamiento de credenciales de Facebook

En Azure Portal, pegue los valores de **Identificador de aplicación de Facebook**, **Secreto de aplicación de Facebook**, **Identificador de página** y **Token de acceso a la página** copiados de Facebook Messenger anteriormente. Puede usar el mismo bot en varias páginas de Facebook mediante la adición de identificadores de página y tokens de acceso adicionales.

![Escribir credenciales](~/media/channels/fb-credentials2.png)

## <a name="submit-for-review"></a>Envío para revisión

Facebook requiere una dirección URL de la directiva de privacidad y una dirección URL de las Condiciones del servicio en su página de configuración de aplicación básica. La página [Código de conducta](https://aka.ms/bf-conduct) contiene vínculos a recursos de terceros para ayudar a crear una directiva de privacidad. La página [Condiciones de uso](https://aka.ms/bf-terms) contiene los términos de ejemplo que le ayudarán a crear un documento de condiciones del servicio adecuado.

Al finalizar el bot, Facebook tiene su propio [proceso de revisión](https://developers.facebook.com/docs/messenger-platform/app-review) para las aplicaciones que se publican en Messenger. El bot se probará para asegurarse de que es compatible con las [políticas de la plataforma](https://developers.facebook.com/docs/messenger-platform/policy-overview) de Facebook.

## <a name="make-the-app-public-and-publish-the-page"></a>Publicación de la aplicación y la página

> [!NOTE]
> Una aplicación se encuentra en [modo de desarrollo](https://developers.facebook.com/docs/apps/managing-development-cycle) hasta que no se publica. Los complementos y las funcionalidades de API solo funcionarán para los administradores, los desarrolladores y los evaluadores.

Una vez que la revisión se considere correcta, en el panel de aplicaciones en Revisión de la aplicación, establezca la aplicación en Pública.
Asegúrese de que la página de Facebook asociada a este bot esté publicada. El estado aparece en la configuración de las páginas.

> [!NOTE]
> También puede usar Facebook Workplace. Para habilitarlo, cree una [integración personalizada](https://developers.facebook.com/docs/workplace/custom-integrations-new) para el área de trabajo y use su identificador de aplicación, secreto de aplicación y token de acceso. En lugar de un valor de pageID tradicional, use los números que siguen al nombre de la integración en la página Acerca de. Los webhooks se pueden conectar con las credenciales que se muestran en Azure.
