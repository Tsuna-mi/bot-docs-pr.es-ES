---
title: Vista previa de las características del bot con Channel Inspector | Microsoft Docs
description: Revise el aspecto y el funcionamiento de las distintas características de Bot Framework en diferentes canales con Channel Inspector.
keyword: bot, preview features, channel, display, Channel Inspector, Text formatting, markdown, XML
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ms.openlocfilehash: def3f811704af2e2a22612ba5755eb5dbd2d8044
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305068"
---
# <a name="preview-bot-features-with-the-channel-inspector"></a>Vista previa de las características del bot con Channel Inspector

Bot Framework le permite crear bots con una variedad de características como texto, botones, imágenes, tarjetas enriquecidas que se muestran en carrusel o en forma de lista y mucho más. Sin embargo, cada canal controla en última instancia la manera en que sus clientes de mensajería representan las funciones.

Incluso cuando múltiples canales admiten una función, cada canal puede representar la característica de una manera ligeramente diferente. En los casos en que un mensaje contenga características que un canal no admita de forma nativa, el canal puede intentar representar los contenidos del mensaje como texto o como una imagen estática, lo que puede afectar significativamente la apariencia del mensaje en el cliente. En algunos casos, un canal puede no ser compatible con una característica en particular. Por ejemplo, los clientes de GroupMe no pueden mostrar un indicador de escritura.

## <a name="the-channel-inspector"></a>Channel Inspector

[Channel Inspector][inspector] se ha creado para proporcionarle una vista previa del aspecto y el funcionamiento de las distintas características de Bot Framework en diferentes canales. Al comprender cómo se representan las características en varios canales, podrá diseñar su bot para ofrecer una experiencia de usuario excepcional en los canales donde se comunica. Channel Inspector también proporciona una excelente manera de conocer y explorar visualmente las características de Bot Framework.

> [!NOTE]
> Las tarjetas enriquecidas son un estándar en desarrollo para el intercambio de información del bot, y así poder garantizar una visualización uniforme en varios canales. Consulte la documentación de [.NET][netcard] o [Node.js][nodecard] para obtener más información acerca de las tarjetas enriquecidas.

## <a name="text-formatting"></a>Formato del texto

El formato del texto puede mejorar visualmente sus mensajes de texto. Además del texto **sin formato**, el bot puede enviar mensajes de texto usando el formato **markdown** o **xml** en los canales que los admitan. En las siguientes tablas se enumeran algunos de los formatos de texto más utilizados en **markdown** y **xml**. Cada canal puede admitir menos o más formatos de texto de los que se enumeran aquí. Puede consultar [Channel Inspector][inspector] para ver si la característica que quiere usar es compatible con el canal de destino.

### <a name="markdown-text-format"></a>Formato de texto markdown

Estos estilos pueden ser compatibles cuando `textFormat` se establece en **markdown**:  

| Estilo | Markdown | Ejemplo |
| ---- | ---- | ---- |
| negrita | \*\*texto\*\* | **text** |
| cursiva | \*text\* | *text* |
| encabezado (1-5) | # H1 | # H1 |
| tachado | \~\~texto\~\~ | ~~text~~ |
| regla horizontal | ---- | ---- |
| lista sin ordenar | texto \* |  <ul><li>text</li></ul> |
| lista ordenada | 1. texto | 1. texto |
| texto con formato previo | \`text\` | `text`  |
| blockquote | texto \> | <blockquote>text</blockquote> |
| hipervínculo | \[bing](http://www.bing.com) | [bing](http://www.bing.com) |
| vínculo de imagen| !\[duck](http://aka.ms/Fo983c) | ![duck](http://aka.ms/Fo983c) |

> [!NOTE]
> Las etiquetas HTML en **Markdown** no se admiten en los canales de chat en web de Microsoft Bot Framework. Si necesita usar etiquetas HTML en **Markdown**, puede representarlas en un canal de [línea directa](~/bot-service-channel-connect-directline.md) que las admita. Como alternativa, puede usar las etiquetas HTML que tiene a continuación; para ello, establezca `textFormat` en **xml** y conecte el bot al canal de Skype.

### <a name="xml-text-format"></a>Formato de texto XML

Estos estilos pueden ser compatibles cuando `textFormat` se establece en **xml**:

|      Estilo      |                       XML                       |                   Ejemplo                   |
|-----------------|-------------------------------------------------|---------------------------------------------|
|      negrita       |                 \<b\>text\</b\>                 |            <strong>text</strong>            |
|     cursiva      |                 \<i\>text\</i\>                 |                <em>text</em>                |
|    subrayado    |                 \<u\>text\</u\>                 |                 <u>text</u>                 |
|  tachado  |                 \<s\>text\</s\>                 |                 <s>text</s>                 |
|    hipervínculo    |   \<a href="http://www.bing.com"\>bing\</a\>    |   <a href="http://www.bing.com">bing</a>    |
|    párrafo    |                 \<p\>text\</p\>                 |                 <p>text</p>                 |
|   salto de línea    |                     \<br/\>                     |             línea 1 <br/>línea 2              |
| regla horizontal |                     \<hr/\>                     |                    <hr/>                    |
|  encabezado (1-4)   |                \<h1\>text\</h1\>                |             <h1>Título 1</h1>              |
|      código       |           \<code\>bloque de código\</code\>           |           <code>code block</code>           |
|      imagen      | \<img src="<http://aka.ms/Fo983c>" alt="Duck"\> | <img src="http://aka.ms/Fo983c" alt="Duck"> |

> [!NOTE]
> El elemento `textFormat` en **xml** solo es compatible con el canal de Skype.

## <a name="preview-features-across-various-channels"></a>Características de vista previa a través de diversos canales

Para ver cómo un canal representa una característica determinada, vaya a [Channel Inspector][inspector], seleccione el canal de la lista **Canal** y la característica de la lista **Característica**. Por ejemplo, para ver cómo Skype representa tarjeta de imagen prominente, establezca **Canal** en *Skype* y **Característica** en *HeroCard*.

![Channel Inspector mostrando el canal de Skype y la tarjeta de imagen prominente](~/media/bot-service-channel-inspector.png)

Channel Inspector muestra una vista previa de la característica seleccionada tal como se representará en el canal especificado. La sección **Notas** transmite información importante sobre las limitaciones del mensaje o los cambios de visualización. Por ejemplo, algunos tipos de tarjetas enriquecidas admiten solo una imagen, y algunas características pueden ser renderizadas en profundidad en ciertos canales.

> [!NOTE]
> Channel Inspector actualmente admite estos canales:
> * Cortana
> * Email
> * Facebook
> * GroupMe
> * Kik
> * Skype
> * Skype Empresarial
> * Slack
> * sms
> * Equipos de Microsoft
> * Telegram
> * WeChat
> * Chat en web
>
> Puede agregar en el futuro canales adicionales.

## <a name="features-that-can-be-previewed"></a>Características que se pueden ver previamente

Actualmente Channel Inspector le permite obtener una vista previa de las siguientes características.

|Característica | DESCRIPCIÓN|
| --- | ----|
| Tarjetas adaptables | Es una tarjeta que puede contener cualquier combinación de texto, voz, imágenes, botones y campos de entrada. |
| Botones| Botones en los que el usuario puede hacer clic. Los botones aparecen en el lienzo de la conversación con el mensaje al que pertenecen. |
| Carrusel| Es una lista horizontal, compacta y desplazable de tarjetas. Para obtener un diseño vertical, use la lista.|
| ChannelData| Es una forma de pasar los metadatos para obtener acceso a la funcionalidad específica del canal, más allá de tarjetas, texto y archivos adjuntos.|
| Codesample| Es un bloque de texto con formato previo, que consta de varias líneas y está diseñado para mostrar el código del equipo.|
| DirectMessage| Es un mensaje enviado a un único miembro de una conversación de grupo.
| Documento| Envío y recepción de archivos adjuntos que no son multimedia. |
| Emoji| Mostrar emojis admitidos.
| GroupChat| El bot envía mensajes para participar en conversaciones de grupo. |
| HeroCard| Es un archivo adjunto con formato que normalmente contiene una sola imagen grande, una acción de pulsado y botones (opcionales), junto con contenido de texto descriptivo. |
| Imágenes| Visualización de los datos adjuntos de la imagen. |
| Diseño de lista| Una lista vertical de las tarjetas. Para obtener un diseño horizontal y desplazable, utilice el carrusel.|
| Ubicación| Para compartir la ubicación física del usuario con el bot. |
| Markdown| Representa el texto con formato con Markdown.|
| Miembros| Comparte la lista de miembros de la conversación con el bot durante un chat de grupo. |
| Tarjeta de recepción| Se muestra una notificación al usuario. |
| Tarjeta de inicio de sesión| Solicita al usuario que escriba las credenciales de autenticación.|
| Acciones sugeridas | Son acciones que se presentan como botones que el usuario puede pulsar para escribir la entrada. |
| Tarjeta de miniatura| Es un archivo adjunto con formato que contiene una sola imagen pequeña (miniatura), una acción de pulsado y botones (opcionales), junto con contenido de texto descriptivo. |
| Escritura| Muestra un indicador de escritura. Esto resulta útil para informar al usuario de que el bot sigue funcionando, pero que está realizando alguna acción en segundo plano.|
| Vídeo| Muestra los datos adjuntos de vídeo y los controles de reproducción.|

## <a name="additional-resources"></a>Recursos adicionales

* [Channel Inspector][inspector]
* Tarjetas enriquecidas en [Node.js][nodecard] y [.NET][netcard]
* Archivos adjuntos multimedia en [Node.js][nodemedia] y [.NET][netmedia]
* Acciones sugeridas en [Node.js][nodebutton] y [.NET][netbutton]

[inspector]: https://docs.botframework.com/en-us/channel-inspector/channels/Skype/

[syntax]: https://daringfireball.net/projects/markdown/syntax

[netcard]: ~/dotnet/bot-builder-dotnet-add-rich-card-attachments.md
[nodecard]: ~/nodejs/bot-builder-nodejs-send-rich-cards.md

[netmedia]: ~/dotnet/bot-builder-dotnet-add-media-attachments.md
[nodemedia]: ~/nodejs/bot-builder-nodejs-send-receive-attachments.md

[netbutton]: ~/dotnet/bot-builder-dotnet-add-suggested-actions.md
[nodebutton]: ~/nodejs/bot-builder-nodejs-send-suggested-actions.md
