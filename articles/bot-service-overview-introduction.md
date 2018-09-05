---
title: Introducción a Azure Bot Service | Microsoft Docs
description: Obtenga información acerca de Bot Service, un servicio para compilar, conectar, probar, implementar, supervisar y administrar bots.
keywords: información general, introducción, SDK, esquema
author: Kaiqb
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 08/27/2018
ms.openlocfilehash: 47dadde9a294855e3c39c1cbd17635b2839b856d
ms.sourcegitcommit: d2e0a1c7da19afc1254bc689bb345dc1804484e2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2018
ms.locfileid: "43117046"
---
::: moniker range="azure-bot-service-3.0"

# <a name="about-azure-bot-service"></a>Acerca de Azure Bot Service

[!INCLUDE [pre-release-label](includes/pre-release-label-v3.md)]

Azure Bot Service proporciona herramientas para crear, probar, implementar y administrar bots inteligentes desde un único lugar. Mediante la plataforma modular y extensible que proporciona el SDK, los desarrolladores pueden aprovechar las plantillas para crear bots que proporcionen voz, reconocimiento del lenguaje, preguntas y respuestas, etc.  

## <a name="what-is-a-bot"></a>¿Qué es un bot?
Un bot es una aplicación con la que los usuarios interactúan de forma conversacional mediante texto, gráficos (tarjetas) o voz. Puede ser un diálogo sencillo de pregunta y respuesta, o un bot sofisticado que permita a los usuarios interactuar con los servicios de una forma inteligente mediante la coincidencia de patrones, el seguimiento de estados y técnicas de inteligencia artificial bien integradas con los servicios empresariales existentes. Compruebe los [casos prácticos](https://azure.microsoft.com/services/bot-service/) de los bots.  

## <a name="building-a-bot"></a>Compilación de un bot 
Puede decidir usar su entorno de desarrollo o sus herramientas de línea de comandos favoritas para crear el bot en C# o Node.js. Se proporcionan herramientas para las distintas etapas de desarrollo de los bots que puede usar para compilar el bot y empezar a trabajar.    

![Información general sobre el bot](media/bot-service-overview.png) 

## <a name="plan"></a>Plan 
Antes de escribir código, revise las [directrices de diseño](bot-service-design-principles.md)  del bot para seguir los procedimientos recomendados e identificar las necesidades del bot. Puede crear un bot simple o incluir funcionalidades más sofisticadas como voz, reconocimiento del lenguaje, QnA o la posibilidad de extraer conocimientos a partir de distintos orígenes y proporcionar respuestas inteligentes.  

> [!TIP] 
>
> Instale la plantilla que necesite:
>  - [Plantilla VSIX](https://marketplace.visualstudio.com/items?itemName=BotBuilder.BotBuilderV3) de Bot Builder SDK v3 para .NET 
>  - [Plantilla Yeoman](https://www.npmjs.com/package/generator-botbuilder) de Bot Builder SDK v3 para Node.js 
>
> Instalación de herramientas:
> - Descargue las [herramientas de la CLI](https://github.com/Microsoft/botbuilder-tools) para crear y administrar los recursos de bot. Estas herramientas le permiten administrar el archivo de configuración del bot, la aplicación LUIS, la base de conocimiento de QnA, etc, desde la línea de comandos. Puede encontrar más información en el archivo [Léame](https://github.com/Microsoft/botbuilder-tools/blob/master/README.md).
> - El [emulador](https://github.com/Microsoft/BotFramework-Emulator/releases) para probar el bot
>
> Si es necesario, use componentes de bot como:  
> - [LUIS](https://www.luis.ai/) para agregar el reconocimiento del lenguaje a los bots
> - [QnA Maker](https://qnamaker.ai/) para responder a las preguntas del usuario de una manera más natural y conversacional
> - [Speech](https://azure.microsoft.com/services/cognitive-services/speech/) para convertir audio en texto, comprender la intención y convertir de nuevo el texto en voz  
> - [Spelling](https://azure.microsoft.com/services/cognitive-services/spell-check/) para corregir errores ortográficos, reconocer la diferencia entre nombres, nombres de marca y jerga 
> - [Cognitive Services](bot-service-concept-intelligence.md) para otros varios componentes inteligentes 


## <a name="build-your-bot"></a>Compilación del bot 
El bot es un servicio web que implementa una interfaz de conversación y que se comunica con Bot Service. Puede crear esta solución en una amplia variedad de entornos y lenguajes, y le ofrecemos herramientas sencillas para empezar en Visual Studio o Yeoman, o directamente en Azure Portal. Mire a continuación algunas de las herramientas y servicios que puede usar.

> [!TIP]
>
> Cree un bot mediante un [SDK](~/dotnet/bot-builder-dotnet-quickstart.md), [Azure Portal](bot-service-quickstart.md) o con las [herramientas de la CLI](~/bot-builder-create-templates.md).
>
> Agregue los componentes: 
> - Agregue el modelo de reconocimiento del lenguaje denominado [LUIS](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/home). 
> - Agregue la base de conocimiento de [QnA Maker](https://docs.microsoft.com/en-us/azure/cognitive-services/qnamaker/home) para responder a las preguntas del usuario.  
> - Mejore la experiencia del usuario con tarjetas, voz o traducción. 
> - Agregue la lógica al bot mediante Bot Builder SDK.   

## <a name="test-your-bot"></a>Prueba del bot 
Los bots son aplicaciones complejas con una gran cantidad de elementos diferentes que funcionan conjuntamente. Como cualquier otra aplicación compleja, esto puede provocar algunos errores interesantes o que el bot se comporte de forma diferente a la esperada. Antes de publicarlo, pruebe el bot.

> [!TIP] 
>
> - [Pruebe el bot con el emulador](bot-service-debug-emulator.md)
> - [Pruebe el bot en Chat en web](bot-service-manage-test-webchat.md)

## <a name="publish"></a>Publicar 
Cuando esté listo, publique su bot en Azure o en su propio servicio web o centro de datos. Puede configurar la implementación continua que le permite desarrollar el bot de forma local y es útil si el bot se inserta en un control de código fuente como GitHub o Visual Studio Team Services. A medida que inserta los cambios en el repositorio de origen, los cambios se implementarán automáticamente en Azure.

> [!Tip]
>
> - [Implementación en Azure](bot-service-build-continuous-deployment.md)

## <a name="connect"></a>Conectar          
Conecte el bot a canales como Facebook, Messenger, Kik, Skype, Slack, Microsoft Teams, Telegram, mensajes de texto o SMS, Twilio, Cortana y Skype para aumentar las interacciones y llegar a más clientes.  
  
> [!TIP]
>
> - [Elija los canales que se van a agregar](bot-service-manage-channels.md)


## <a name="evaluate"></a>Evaluate 
Use los datos recopilados en Azure Portal para identificar oportunidades para mejorar las funcionalidades y el rendimiento de su bot. Puede obtener nivel de servicio y datos de instrumentación como tráfico, latencia e integraciones. Analytics proporciona también informes de nivel de conversación relativos a los datos del usuario, los mensajes y los canales.

> [!Tip]
>
> - [Recopilación de análisis](bot-service-manage-analytics.md) 


::: moniker-end

::: moniker range="azure-bot-service-4.0"

# <a name="about-azure-bot-service"></a>Acerca de Azure Bot Service

[!INCLUDE [pre-release-label](includes/pre-release-label.md)]

Azure Bot Service proporciona herramientas para crear, probar, implementar y administrar bots inteligentes desde un único lugar. Mediante la plataforma modular y extensible que proporciona el SDK, los desarrolladores pueden aprovechar las plantillas para crear bots que proporcionen voz, reconocimiento de lenguaje natural, control de preguntas y respuestas, etc.  

## <a name="what-is-a-bot"></a>¿Qué es un bot?

Un bot es una aplicación que puede comunicarse mediante una conversación con usuarios humanos. El software puede interactuar mediante texto, voz, gráficos o menús y realizar tareas relacionadas con la conversación. Puede ser un simple intercambio de preguntas y respuestas o un bot sofisticado que permite a los usuarios interactuar con los servicios de forma natural, mientras que en segundo plano, el bot utiliza técnicas de inteligencia perfectamente integradas con los servicios existentes.

Los bots permiten a los sistemas recopilar información o proporcionar una experiencia a los usuarios menos similar a la de un equipo informático y más parecida a una interacción. También realizan tareas simples, como tomar una reserva de una cena o recopilar información del perfil del usuario, en los sistemas (o la integración con otros sistemas) cuando no es necesaria la interacción directa del usuario. 

## <a name="building-a-bot"></a>Compilación de un bot 

Puede decidir usar su entorno de desarrollo o sus herramientas de línea de comandos favoritas para crear el bot en C#, JavaScript, Java y Python. Se proporcionan herramientas para las distintas etapas de desarrollo de los bots que puede usar para compilar el bot y empezar a trabajar.    

### <a name="design"></a>Diseño

Antes de escribir código, revise las [directrices de diseño](bot-service-design-principles.md)  del bot para seguir los procedimientos recomendados e identificar las necesidades del bot. Puede crear un bot simple o incluir funcionalidades más sofisticadas, como voz, reconocimiento de lenguaje natural o responder las preguntas que formulan los usuarios. 

### <a name="build"></a>Compilación

El bot es un servicio web que implementa una interfaz de conversación y que se comunica con Bot Service. Puede crear esta solución en una amplia variedad de entornos y lenguajes y le ofrecemos plantillas sencillas para empezar a trabajar. Puede iniciar el desarrollo de bots en [Azure Portal](bot-service-quickstart.md) o usar las plantillas que se enumeran a continuación para el desarrollo local en el lenguaje que prefiera.

| Plantilla de .NET | Plantilla de JavaScript | 
| --- | --- | 
| [Plantilla de VSIX](https://marketplace.visualstudio.com/items?itemName=BotBuilder.botbuilderv4) | [Plantilla de Yeoman](https://www.npmjs.com/package/generator-botbuilder) Use @preview para obtener la plantilla v4. |


Estos son los componentes adicionales que puede usar para mejorar la funcionalidad del bot. 

| Característica | DESCRIPCIÓN | Vínculo |
| --- | --- | --- |
| Agregar procesamiento de lenguaje natural | Habilite el bot para reconocer el lenguaje natural, los errores de ortografía, usar la voz y reconocer la intención del usuario | [Uso de LUIS](~/v4sdk/bot-builder-howto-v4-luis.md) 
| Responder preguntas | Agregue una base de conocimiento para responder preguntas que los usuarios formulan de forma más natural y conversacional | [Uso de QnA Maker](~/v4sdk/bot-builder-howto-qna.md) 
| Administrar varios modelos | Si usa más de un modelo, por ejemplo, para LUIS y para QnA Maker, determine de manera inteligente cuándo usar cada uno de ellos durante la conversación del bot | [Herramienta de distribución](~/v4sdk/bot-builder-tutorial-dispatch.md) |
| Agregar tarjetas y botones | Mejore la experiencia del usuario con medios distintos al texto, como gráficos, menús y tarjetas | [Adición de tarjetas](v4sdk/bot-builder-howto-add-media-attachments.md) |

> [!NOTE]
> La tabla anterior no es una lista completa. Explore los artículos de la izquierda, comenzando por el [envío de mensajes](~/v4sdk/bot-builder-howto-send-messages.md), para obtener más funcionalidades de bot.

Además, se ofrecen herramientas de la CLI para ayudarle a crear, administrar y probar los recursos de bots. Estas herramientas permiten administrar un archivo de configuración del bot, una aplicación de LUIS, una base de conocimiento de QnA, etc, desde la línea de comandos. Puede encontrar más información en el archivo [Léame](https://github.com/Microsoft/botbuilder-tools/blob/master/README.md).

### <a name="test"></a>Prueba 
Los bots son aplicaciones complejas con una gran cantidad de elementos diferentes que funcionan conjuntamente. Como cualquier otra aplicación compleja, esto puede provocar algunos errores interesantes o que el bot se comporte de forma diferente a la esperada. Antes de publicarlo, pruebe el bot. 

[Pruebe el bot con el emulador](bot-service-debug-emulator.md) o [Pruebe el bot en Chat en web](bot-service-manage-test-webchat.md).

### <a name="publish"></a>Publicar 
Cuando esté listo, [publique el bot en Azure](bot-builder-howto-deploy-azure.md) o en su propio servicio web o centro de datos.

### <a name="connect"></a>Conectar          
Conecte el bot a canales como Facebook, Messenger, Kik, Skype, Slack, Microsoft Teams, Telegram, mensajes de texto o SMS, Twilio, Cortana y Skype para aumentar las interacciones y llegar a más clientes.

[Elija los canales que se van a agregar](bot-service-manage-channels.md).


### <a name="evaluate"></a>Evaluate 
Use los datos recopilados en Azure Portal para identificar oportunidades para mejorar las funcionalidades y el rendimiento de su bot. Puede obtener nivel de servicio y datos de instrumentación como tráfico, latencia e integraciones. Analytics proporciona también informes de nivel de conversación relativos a los datos del usuario, los mensajes y los canales. 

Explore cómo [recopilar datos de análisis](bot-service-manage-analytics.md).


## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Creación de un bot](bot-service-quickstart.md)

::: moniker-end
