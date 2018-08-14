---
title: Incorporación de datos adjuntos con elementos multimedia a mensajes | Microsoft Docs
description: Obtenga información sobre cómo agregar datos adjuntos con elementos multimedia a los mensajes mediante el SDK de Bot Builder para. NET.
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: 68546a9c46bdeb96d31195800cee8a8253fd207b
ms.sourcegitcommit: 67445b42796d90661afc643c6bb6533e9a662cbc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2018
ms.locfileid: "39574755"
---
# <a name="add-media-attachments-to-messages"></a><span data-ttu-id="4909a-103">Incorporación de datos adjuntos con elementos multimedia a mensajes</span><span class="sxs-lookup"><span data-stu-id="4909a-103">Add media attachments to messages</span></span>

[!INCLUDE [pre-release-label](../includes/pre-release-label-v3.md)]

> [!div class="op_single_selector"]
> - [.NET](../dotnet/bot-builder-dotnet-add-media-attachments.md)
> - [Node.js](../nodejs/bot-builder-nodejs-send-receive-attachments.md)
> - [REST](../rest-api/bot-framework-rest-connector-add-media-attachments.md)

<span data-ttu-id="4909a-107">Un intercambio de mensajes entre el usuario y el bot puede contener datos adjuntos con elementos multimedia (por ejemplo, imágenes, vídeo, audio, archivos).</span><span class="sxs-lookup"><span data-stu-id="4909a-107">A message exchange between user and bot can contain media attachments (e.g., image, video, audio, file).</span></span> <span data-ttu-id="4909a-108">La propiedad `Attachments` del objeto <a href="https://docs.botframework.com/en-us/csharp/builder/sdkreference/dc/d2f/class_microsoft_1_1_bot_1_1_connector_1_1_activity.html" target="_blank">Activity</a> contiene una matriz de objetos <a href="https://docs.microsoft.com/en-us/dotnet/api/microsoft.bot.connector.attachments?view=botconnector-3.12.2.4" target="_blank">Attachment</a> que presentan los datos adjuntos con elementos multimedia y las tarjetas enriquecidas dentro del mensaje.</span><span class="sxs-lookup"><span data-stu-id="4909a-108">The `Attachments` property of the <a href="https://docs.botframework.com/en-us/csharp/builder/sdkreference/dc/d2f/class_microsoft_1_1_bot_1_1_connector_1_1_activity.html" target="_blank">Activity</a> object contains an array of <a href="https://docs.microsoft.com/en-us/dotnet/api/microsoft.bot.connector.attachments?view=botconnector-3.12.2.4" target="_blank">Attachment</a> objects that represent the media attachments and rich cards within to the message.</span></span> 

> [!NOTE]
> [Incorporación de tarjetas enriquecidas a mensajes](bot-builder-dotnet-add-rich-card-attachments.md)

## <a name="add-a-media-attachment"></a><span data-ttu-id="4909a-110">Incorporación de datos adjuntos con elementos multimedia</span><span class="sxs-lookup"><span data-stu-id="4909a-110">Add a media attachment</span></span>  

<span data-ttu-id="4909a-111">Para agregar datos adjuntos con elementos multimedia a un mensaje, cree un objeto `Attachment` para la actividad `message` y establezca las propiedades `ContentType`, `ContentUrl` y `Name`.</span><span class="sxs-lookup"><span data-stu-id="4909a-111">To add a media attachment to a message, create an `Attachment` object for the `message` activity and set the `ContentType`, `ContentUrl`, and `Name` properties.</span></span> 

[!code-csharp[Add media attachment](../includes/code/dotnet-add-attachments.cs#addMediaAttachment)]

<span data-ttu-id="4909a-112">Si los datos adjuntos son una imagen, un audio o un vídeo, el servicio del conector comunicará los datos adjuntos al canal de una manera que permita que el [canal](bot-builder-dotnet-channeldata.md) presente esos datos adjuntos dentro de la conversación.</span><span class="sxs-lookup"><span data-stu-id="4909a-112">If an attachment is an image, audio, or video, the Connector service will communicate attachment data to the channel in a way that enables the [channel](bot-builder-dotnet-channeldata.md) to render that attachment within the conversation.</span></span> <span data-ttu-id="4909a-113">Si los datos adjuntos son un archivo, la dirección URL del archivo se presentará como un hipervínculo dentro de la conversación.</span><span class="sxs-lookup"><span data-stu-id="4909a-113">If the attachment is a file, the file URL will be rendered as a hyperlink within the conversation.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4909a-114">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="4909a-114">Additional resources</span></span>

- <span data-ttu-id="4909a-115">[Preview features with the Channel Inspector][inspector] (Vista previa de las características con el Inspector de canales)</span><span class="sxs-lookup"><span data-stu-id="4909a-115">[Preview features with the Channel Inspector][inspector]</span></span>
- <span data-ttu-id="4909a-116">[Activities overview](bot-builder-dotnet-activities.md) (Introducción a las actividades)</span><span class="sxs-lookup"><span data-stu-id="4909a-116">[Activities overview](bot-builder-dotnet-activities.md)</span></span>
- <span data-ttu-id="4909a-117">[Create messages](bot-builder-dotnet-create-messages.md) (Creación de mensajes)</span><span class="sxs-lookup"><span data-stu-id="4909a-117">[Create messages](bot-builder-dotnet-create-messages.md)</span></span>
- [<span data-ttu-id="4909a-118">Incorporación de tarjetas enriquecidas a mensajes</span><span class="sxs-lookup"><span data-stu-id="4909a-118">Add rich cards to messages</span></span>](bot-builder-dotnet-add-rich-card-attachments.md)
- <span data-ttu-id="4909a-119"><a href="https://docs.botframework.com/en-us/csharp/builder/sdkreference/dc/d2f/class_microsoft_1_1_bot_1_1_connector_1_1_activity.html" target="_blank">Clase Activity</a></span><span class="sxs-lookup"><span data-stu-id="4909a-119"><a href="https://docs.botframework.com/en-us/csharp/builder/sdkreference/dc/d2f/class_microsoft_1_1_bot_1_1_connector_1_1_activity.html" target="_blank">Activity class</a></span></span>
- <span data-ttu-id="4909a-120"><a href="https://docs.microsoft.com/en-us/dotnet/api/microsoft.bot.connector.attachments?view=botconnector-3.12.2.4" target="_blank">Clase Attachment</a></span><span class="sxs-lookup"><span data-stu-id="4909a-120"><a href="https://docs.microsoft.com/en-us/dotnet/api/microsoft.bot.connector.attachments?view=botconnector-3.12.2.4" target="_blank">Attachment class</a></span></span>

[inspector]: ../bot-service-channel-inspector.md


