---
title: Acerca de Bot Service | Microsoft Docs
description: Obtenga información acerca de Bot Service, un servicio para compilar, conectar, probar, implementar, supervisar y administrar bots.
keywords: información general, introducción, SDK, esquema
author: Kaiqb
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 05/03/2018
ms.openlocfilehash: 142d4cbe0c252e88bab800bb3823b70434a65bd6
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39306213"
---
::: moniker range="azure-bot-service-3.0"

# <a name="azure-bot-service"></a>Azure Bot Service

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

# <a name="azure-bot-service"></a>Azure Bot Service

[!INCLUDE [pre-release-label](includes/pre-release-label.md)]

Azure Bot Service proporciona herramientas para crear, probar, implementar y administrar bots inteligentes desde un único lugar. Mediante la plataforma modular y extensible que proporciona el SDK, los desarrolladores pueden aprovechar las plantillas para crear bots que proporcionen voz, reconocimiento del lenguaje, preguntas y respuestas, etc.  

## <a name="what-is-a-bot"></a>¿Qué es un bot?
Un bot es una aplicación con la que los usuarios interactúan de forma conversacional mediante texto, gráficos (tarjetas) o voz. Puede ser un diálogo sencillo de pregunta y respuesta, o un bot sofisticado que permita a los usuarios interactuar con los servicios de una forma inteligente mediante la coincidencia de patrones, el seguimiento de estados y técnicas de inteligencia artificial bien integradas con los servicios empresariales existentes. Compruebe los [casos prácticos](https://azure.microsoft.com/services/bot-service/) de los bots.  

## <a name="building-a-bot"></a>Compilación de un bot 
Puede decidir usar su entorno de desarrollo o sus herramientas de línea de comandos favoritas para crear el bot en C#, JavaScript, Java y Python. Se proporcionan herramientas para las distintas etapas de desarrollo de los bots que puede usar para compilar el bot y empezar a trabajar.    

![Información general sobre el bot](media/bot-service-overview.png) 

## <a name="plan"></a>Plan 
Antes de escribir código, revise las [directrices de diseño](bot-service-design-principles.md)  del bot para seguir los procedimientos recomendados e identificar las necesidades del bot. Puede crear un bot simple o incluir funcionalidades más sofisticadas como voz, reconocimiento del lenguaje, QnA o la posibilidad de extraer conocimientos a partir de distintos orígenes y proporcionar respuestas inteligentes. Para empezar a trabajar, puede usar las [herramientas de la CLI](~/bot-builder-create-templates.md), [Azure Portal](bot-service-quickstart.md) o las plantillas siguientes.

**Elija la plantilla que necesite**

| .NET | JavaScript | 
| --- | --- | 
| [Plantilla de VSIX](https://marketplace.visualstudio.com/items?itemName=BotBuilder.botbuilderv4) | [Plantilla de Yeoman](https://www.npmjs.com/package/generator-botbuilder) Use @preview para obtener la plantilla v4. |

**Explore o instale las herramientas si es necesario**

- Descargue las [herramientas de la CLI](https://github.com/Microsoft/botbuilder-tools) para crear y administrar los recursos de bot. Estas herramientas le permiten administrar el archivo de configuración del bot, la aplicación LUIS, la base de conocimiento de QnA, etc, desde la línea de comandos. Puede encontrar más información en el archivo [Léame](https://github.com/Microsoft/botbuilder-tools/blob/master/README.md).
- El [emulador](https://github.com/Microsoft/BotFramework-Emulator/releases) para probar el bot
- [LUIS](https://www.luis.ai/) para agregar lenguaje natural a los bots
- [QnA Maker](https://qnamaker.ai/) para responder a las preguntas del usuario de una manera más natural y conversacional


## <a name="build-your-bot"></a>Compilación del bot 
El bot es un servicio web que implementa una interfaz de conversación y que se comunica con Bot Service. Puede crear esta solución en una amplia variedad de entornos y lenguajes, y le ofrecemos herramientas sencillas para empezar en Visual Studio o Yeoman, o directamente en Azure Portal. Mire a continuación algunas de las herramientas y servicios que puede usar.

Algunos de los muchos componentes disponibles son:

| Componentes | DESCRIPCIÓN |
| --- | --- |
| [LUIS](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/home) | Agregue reconocimiento del lenguaje |
| [QnA Maker](https://docs.microsoft.com/en-us/azure/cognitive-services/qnamaker/home)  | Agregue la base de conocimiento para responder a las preguntas del usuario |
| [Herramienta de distribución](~/v4sdk/bot-builder-tutorial-dispatch.md) | Si usa más de un modelo, determine de forma inteligente cuál usar |
| [Medios enriquecidos](v4sdk/bot-builder-howto-add-media-attachments.md) | Mejore la experiencia del usuario con tarjetas de medios o voz |
| [Voz](https://azure.microsoft.com/services/cognitive-services/speech/) | Convierta audio en texto, comprenda la intención y convierta de nuevo el texto en voz |
| [Spelling](https://azure.microsoft.com/services/cognitive-services/spell-check/) | Corrija errores ortográficos, reconozca la diferencia entre nombres, nombres de marca y jerga |

> [!NOTE]
> La tabla anterior no es una lista completa. Explore los artículos de la izquierda, comenzando por el [envío de mensajes](v4sdk/bot-builder-howto-send-messages.md), para obtener más funcionalidades de bot.

## <a name="test-your-bot"></a>Prueba del bot 
Los bots son aplicaciones complejas con una gran cantidad de elementos diferentes que funcionan conjuntamente. Como cualquier otra aplicación compleja, esto puede provocar algunos errores interesantes o que el bot se comporte de forma diferente a la esperada. Antes de publicarlo, pruebe el bot. 

[Pruebe el bot con el emulador](bot-service-debug-emulator.md) o [Pruebe el bot en Chat en web](bot-service-manage-test-webchat.md).

## <a name="publish"></a>Publicar 
Cuando esté listo, publique su bot en Azure o en su propio servicio web o centro de datos. Puede configurar la implementación continua que le permite desarrollar el bot de forma local y es útil si el bot se inserta en un control de código fuente como GitHub o Visual Studio Team Services. A medida que inserta los cambios en el repositorio de origen, los cambios se implementarán automáticamente en Azure.

[Implemente en Azure](bot-service-build-continuous-deployment.md) mediante la implementación continua.

## <a name="connect"></a>Conectar          
Conecte el bot a canales como Facebook, Messenger, Kik, Skype, Slack, Microsoft Teams, Telegram, mensajes de texto o SMS, Twilio, Cortana y Skype para aumentar las interacciones y llegar a más clientes.

[Elija los canales que se van a agregar](bot-service-manage-channels.md).


## <a name="evaluate"></a>Evaluate 
Use los datos recopilados en Azure Portal para identificar oportunidades para mejorar las funcionalidades y el rendimiento de su bot. Puede obtener nivel de servicio y datos de instrumentación como tráfico, latencia e integraciones. Analytics proporciona también informes de nivel de conversación relativos a los datos del usuario, los mensajes y los canales. 

Explore cómo [recopilar análisis](bot-service-manage-analytics.md).

::: moniker-end
