---
title: Conectar un bot a Cortana | Microsoft Docs
description: Obtenga información sobre cómo configurar un bot para acceder a través de la interfaz de Cortana.
keywords: cortana, canal de bot, configurar cortana
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 04/30/2018
ms.openlocfilehash: 6e694ce8b54ebd2405d7496d333c2bb27eb344f1
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305069"
---
# <a name="connect-a-bot-to-cortana"></a>Conectar un bot a Cortana

Cortana es un canal habilitado para voz que puede enviar y recibir mensajes de voz, además de tener conversaciones por escrito. Un bot que se vaya a conectar a Cortana debe diseñarse para que admita tanto la voz y el texto. Una *habilidad* de Cortana es un bot que se puede invocar con un cliente de Cortana. Si publica un bot, se agregará a la lista de habilidades disponibles.

Para agregar el canal de Cortana, abra el bot en [Azure Portal](https://portal.azure.com/), haga clic en la hoja **Canales** y luego en **Cortana**.

![Agregar el canal de Cortana](~/media/channels/cortana-addchannel.png)

## <a name="configure-cortana"></a>Configurar Cortana

Al conectar su bot con el canal de Cortana, parte de la información básica sobre el mismo se completará previamente en el formulario de registro. Revise esta información detenidamente. Este formulario consta de los siguientes campos:

| Campo | DESCRIPCIÓN |
|------|------|
| **Icono de habilidades** | Un icono que se muestra en el lienzo de Cortana cuando se invoca la habilidad. También se usa cuando las habilidades son detectables (como Microsoft Store). (32 KB máximo; solo PNG).|
| **Nombre para mostrar** | El nombre de la habilidad de Cortana se muestra al usuario en la parte superior de la interfaz de usuario visual. (Límite de 30 caracteres). |
| **Nombre de invocación** | Este es el nombre que los usuarios deben decir al invocar una habilidad. No debe tener más de tres palabras y debe ser fácil pronunciar. Consulte la [Invocation Name Guidelines][invocation] (Información del nombre de invocación) para obtener más información sobre cómo elegir este nombre.|
| **Descripción** | Una descripción de la habilidad de Cortana. Se usa cuando las habilidades son detectables (como Microsoft Store). |
| **Descripción breve** | Breve descripción de la funcionalidad de la habilidad; se usa para describir la habilidad en el cuaderno de Cortana. |

## <a name="general-bot-information"></a>Información general sobre el bot

En la sección **Manage user identity through connected services** (Administrar la identidad del usuario a través de los servicios conectados), seleccione esta opción para habilitarla. Rellene el formulario.

Todos los campos marcados con un asterisco (*) son obligatorios. Los bots deben publicarse en Bot Framework antes de conectarlos Cortana.

![Proporcionar información general](~/media/channels/cortana-details.png)

### <a name="sign-in-at-invocation"></a>Iniciar sesión en la invocación

Seleccione esta opción si quiere que Cortana inicie la sesión del usuario en el momento en que este último invoque la habilidad.

### <a name="sign-in-when-required"></a>Iniciar sesión cuando se solicite

Seleccione esta opción si usa una tarjeta SignIn de Bot Framework para iniciar la sesión del usuario. Puede usar esta opción si quiere iniciar la sesión del usuario solo este si utiliza una función que requiera autenticación. Cuando su habilidad envía un mensaje que incluye la tarjeta SignIn como un archivo adjunto, Cortana ignora la tarjeta SignIn y realiza el flujo de autorización usando la configuración para conectar la cuenta.

### <a name="connected-service-icon"></a>Icono de servicio conectado

El icono que quiere mostrar cuando el usuario inicie sesión en la habilidad.

### <a name="account-name"></a>Nombre de cuenta

El nombre de habilidad que quiere mostrar cuando el usuario inicie sesión en la misma.

### <a name="client-id-for-third-party-services"></a>Id. de cliente de servicios de terceros

Identificador de aplicación del bot. Recibió el identificador al registrar el bot.

### <a name="space-separated-list-of-scopes"></a>Lista de ámbitos separada por espacios

Especifique los ámbitos que necesita el servicio (consulte la documentación del servicio).

### <a name="authorization-url"></a>Dirección URL de autorización

Establézcalo en `https://login.microsoftonline.com/common/oauth2/v2.0/authorize`.

### <a name="grant-type"></a>Tipo de concesión

Seleccione el código de autorización para usar el flujo de concesión de código. Seleccione Implícito para usar el flujo implícito.

### <a name="token-url"></a>Dirección URL de token

Si selecciona el código de autorización, establézcalo en `https://login.microsoftonline.com/common/oauth2/v2.0/token`.

### <a name="client-secretpassword-for-third-party-services"></a>Contraseña o secreto de cliente para servicios de terceros

Contraseña del bot. Recibió la contraseña al registrar el bot.

### <a name="client-authentication-scheme"></a>Esquema de autenticación de clientes

Seleccione HTTP Basic.

### <a name="token-options"></a>Opciones de token

Establézcalo en POST.

### <a name="request-user-profile-data-optional"></a>Solicitar datos de perfil de usuario (opcional)

Cortana proporciona acceso a diferentes tipos de información de perfil de usuario, que puede usar para personalizar el bot para el usuario. Por ejemplo, si una habilidad tiene acceso al nombre y la ubicación del usuario, entonces la habilidad puede proporcionar una respuesta personalizada como "Hola Kamran, espero que estés teniendo un día agradable en Bellevue, Washington".

Haga clic en el enlace **Agregar una solicitud de perfil de usuario** y seleccione en la lista desplegable la información del perfil de usuario que quiera usar. Agregue un nombre descriptivo que se pueda usar para obtener acceso a esta información desde el código del bot.

### <a name="save-skill"></a>Guardar la habilidad

Cuando haya completado el formulario de registro de su habilidad de Cortana, haga clic en Guardar para completar la conexión. Esto le llevará de vuelta a la hoja Canales del bot, donde verá que ya está conectado a Cortana.

En este momento, su bot ya se ha implementado automáticamente como una habilidad de Cortana en su cuenta.

## <a name="next-steps"></a>Pasos siguientes

* [Cortana Skills Kit](https://aka.ms/CortanaSkillsDocs) (Kit de habilidades de Cortana)
* [Habilitar depuración](bot-service-debug-cortana-skill.md)
* [Publicar una habilidad de Cortana][publish]

[invocation]: https://docs.microsoft.com/en-us/cortana/skills/cortana-invocation-guidelines
[publish]: https://docs.microsoft.com/en-us/cortana/skills/publish-skill
[connected]: https://aka.ms/CortanaSkillsBotConnectedAccount
[CortanaEntity]: https://aka.ms/lgvcto
