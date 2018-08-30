---
title: Introducción a las actividades | Microsoft Docs
description: Conozca sobre los diferentes tipos de actividades disponibles en Bot Connector Service.
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ms.openlocfilehash: 565f71dee4269ecd67dabe18af57e36defcfb3fc
ms.sourcegitcommit: e8c513d3af5f0c514cadcbcd0a737a7393405afa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/20/2018
ms.locfileid: "42756560"
---
# <a name="activities-overview"></a><span data-ttu-id="7a528-103">Introducción a las actividades</span><span class="sxs-lookup"><span data-stu-id="7a528-103">Activities overview</span></span>

<span data-ttu-id="7a528-104">Bot Connector Service intercambia información entre el bot y el canal (usuario) pasando un objeto [Activity][Activity].</span><span class="sxs-lookup"><span data-stu-id="7a528-104">The Bot Connector service exchanges information between between bot and channel (user) by passing an [Activity][Activity] object.</span></span> <span data-ttu-id="7a528-105">El tipo de actividad más común es **message**, pero hay otros tipos de actividades que se pueden usar para comunicar distintos tipos de información a un bot o canal.</span><span class="sxs-lookup"><span data-stu-id="7a528-105">The most common type of activity is **message**, but there are other activity types that can be used to communicate various types of information to a bot or channel.</span></span> 

## <a name="activity-types-in-the-bot-connector-service"></a><span data-ttu-id="7a528-106">Tipos de actividad en Bot Connector Service</span><span class="sxs-lookup"><span data-stu-id="7a528-106">Activity types in the Bot Connector service</span></span>

<span data-ttu-id="7a528-107">Bot Connector Service admite los siguientes tipos de actividades.</span><span class="sxs-lookup"><span data-stu-id="7a528-107">The following activity types are supported by the Bot Connector service.</span></span>

| <span data-ttu-id="7a528-108">Tipo de actividad</span><span class="sxs-lookup"><span data-stu-id="7a528-108">Activity type</span></span> | <span data-ttu-id="7a528-109">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="7a528-109">Description</span></span> |
|------|------|------|
| <span data-ttu-id="7a528-110">Mensaje</span><span class="sxs-lookup"><span data-stu-id="7a528-110">message</span></span> | <span data-ttu-id="7a528-111">Representa una comunicación entre el bot y el usuario.</span><span class="sxs-lookup"><span data-stu-id="7a528-111">Represents a communication between bot and user.</span></span> |
| <span data-ttu-id="7a528-112">conversationUpdate</span><span class="sxs-lookup"><span data-stu-id="7a528-112">conversationUpdate</span></span> | <span data-ttu-id="7a528-113">Indica que el bot se agregó a una conversación, que otros miembros se agregaron o se quitaron de la conversación, o bien que los metadatos de la conversación han cambiado.</span><span class="sxs-lookup"><span data-stu-id="7a528-113">Indicates that the bot was added to a conversation, other members were added to or removed from the conversation, or conversation metadata has changed.</span></span> |
| <span data-ttu-id="7a528-114">contactRelationUpdate</span><span class="sxs-lookup"><span data-stu-id="7a528-114">contactRelationUpdate</span></span> | <span data-ttu-id="7a528-115">Indica que el bot se agregó o quitó de la lista de contactos de un usuario.</span><span class="sxs-lookup"><span data-stu-id="7a528-115">Indicates that the bot was added or removed from a user's contact list.</span></span> |
| <span data-ttu-id="7a528-116">typing</span><span class="sxs-lookup"><span data-stu-id="7a528-116">typing</span></span> | <span data-ttu-id="7a528-117">Indica que el usuario o el bot en el otro extremo de la conversación está redactando una respuesta.</span><span class="sxs-lookup"><span data-stu-id="7a528-117">Indicates that the user or bot on the other end of the conversation is compiling a response.</span></span> | 
| <span data-ttu-id="7a528-118">deleteUserData</span><span class="sxs-lookup"><span data-stu-id="7a528-118">deleteUserData</span></span> | <span data-ttu-id="7a528-119">Indica a un bot que un usuario ha solicitado que el bot elimine todos los datos de usuario que haya podido almacenar.</span><span class="sxs-lookup"><span data-stu-id="7a528-119">Indicates to a bot that a user has requested that the bot delete any user data it may have stored.</span></span> |
| <span data-ttu-id="7a528-120">endOfConversation</span><span class="sxs-lookup"><span data-stu-id="7a528-120">endOfConversation</span></span> | <span data-ttu-id="7a528-121">Indica el final de una conversación.</span><span class="sxs-lookup"><span data-stu-id="7a528-121">Indicates the end of a conversation.</span></span> |

## <a name="message"></a><span data-ttu-id="7a528-122">Mensaje</span><span class="sxs-lookup"><span data-stu-id="7a528-122">message</span></span>

<span data-ttu-id="7a528-123">El bot enviará actividades de **mensaje** para comunicar información a los usuarios y recibir actividades de **mensaje** de ellos.</span><span class="sxs-lookup"><span data-stu-id="7a528-123">Your bot will send **message** activities to communicate information to and receive **message** activities from users.</span></span> <span data-ttu-id="7a528-124">Algunos mensajes pueden ser de texto sin formato, mientras que otros pueden incluir contenido más enriquecido como [elementos multimedia adjuntos](bot-framework-rest-connector-add-media-attachments.md), [botones y tarjetas](bot-framework-rest-connector-add-rich-cards.md), o [datos específicos del canal](bot-framework-rest-connector-channeldata.md).</span><span class="sxs-lookup"><span data-stu-id="7a528-124">Some messages may be plain text, while others may contain richer content such as [media attachments](bot-framework-rest-connector-add-media-attachments.md), [buttons, and cards](bot-framework-rest-connector-add-rich-cards.md) or [channel-specific data](bot-framework-rest-connector-channeldata.md).</span></span> <span data-ttu-id="7a528-125">Para información sobre las propiedades de mensaje más usadas, consulte [Creación de mensajes](bot-framework-rest-connector-create-messages.md).</span><span class="sxs-lookup"><span data-stu-id="7a528-125">For information about commonly-used message properties, see [Create messages](bot-framework-rest-connector-create-messages.md).</span></span> <span data-ttu-id="7a528-126">Para más información acerca de cómo enviar y recibir mensajes, consulte [Envío y recepción de mensajes](bot-framework-rest-connector-send-and-receive-messages.md).</span><span class="sxs-lookup"><span data-stu-id="7a528-126">For information about how to send and receive messages, see [Send and receive messages](bot-framework-rest-connector-send-and-receive-messages.md).</span></span> 

## <a name="conversationupdate"></a><span data-ttu-id="7a528-127">conversationUpdate</span><span class="sxs-lookup"><span data-stu-id="7a528-127">conversationUpdate</span></span>

<span data-ttu-id="7a528-128">Un bot recibe una actividad **conversationUpdate** cada vez que se ha agregado a una conversación, se han agregado otros miembros a la conversación (o se han eliminado de ella), o bien han cambiado los metadatos de la conversación.</span><span class="sxs-lookup"><span data-stu-id="7a528-128">A bot receives a **conversationUpdate** activity whenever it has been added to a conversation, other members have been added to or removed from a conversation, or conversation metadata has changed.</span></span> 

<span data-ttu-id="7a528-129">Si se han agregado miembros a la conversación, la propiedad `addedMembers` de la actividad identificará a los miembros nuevos.</span><span class="sxs-lookup"><span data-stu-id="7a528-129">If members have been added to the conversation, the activity's `addedMembers` property will identify the new members.</span></span> <span data-ttu-id="7a528-130">Si se han eliminado miembros de la conversación, la propiedad `removedMembers` identificará a los miembros eliminados.</span><span class="sxs-lookup"><span data-stu-id="7a528-130">If members have been removed from the conversation, the `removedMembers` property will identify the removed members.</span></span> 

> [!TIP]
> <span data-ttu-id="7a528-131">Si el bot recibe una actividad **conversationUpdate** en la que se indica que un usuario se ha unido a la conversación, puede elegir que le responda enviando un mensaje de bienvenida a ese usuario.</span><span class="sxs-lookup"><span data-stu-id="7a528-131">If your bot receives a **conversationUpdate** activity indicating that a user has joined the conversation, you may choose to respond by sending a welcome message to that user.</span></span> 

## <a name="contactrelationupdate"></a><span data-ttu-id="7a528-132">contactRelationUpdate</span><span class="sxs-lookup"><span data-stu-id="7a528-132">contactRelationUpdate</span></span>

<span data-ttu-id="7a528-133">Un bot recibe una actividad **contactRelationUpdate** siempre que se agrega o se quita de la lista de contactos de un usuario.</span><span class="sxs-lookup"><span data-stu-id="7a528-133">A bot receives a **contactRelationUpdate** activity whenever it is added to or removed from a user's contact list.</span></span> <span data-ttu-id="7a528-134">El valor de la propiedad `action` de la actividad (add | remove) indica si el bot se ha agregado o quitado de la lista de contactos del usuario.</span><span class="sxs-lookup"><span data-stu-id="7a528-134">The value of the activity's `action` property (add | remove) indicates whether the bot has been added or removed from the user's contact list.</span></span>

## <a name="typing"></a><span data-ttu-id="7a528-135">typing</span><span class="sxs-lookup"><span data-stu-id="7a528-135">typing</span></span>

<span data-ttu-id="7a528-136">Un bot recibe una actividad **typing** para indicar que el usuario está escribiendo una respuesta.</span><span class="sxs-lookup"><span data-stu-id="7a528-136">A bot receives a **typing** activity to indicate that the user is typing a response.</span></span> <span data-ttu-id="7a528-137">Un bot puede enviar una actividad **tyiping** para indicar al usuario que está trabajando para satisfacer una solicitud o compilar una respuesta.</span><span class="sxs-lookup"><span data-stu-id="7a528-137">A bot may send a **typing** activity to indicate to the user that it is working to fulfill a request or compile a response.</span></span> 

## <a name="deleteuserdata"></a><span data-ttu-id="7a528-138">deleteUserData</span><span class="sxs-lookup"><span data-stu-id="7a528-138">deleteUserData</span></span>

<span data-ttu-id="7a528-139">Un bot recibe una actividad **deleteUserData** cuando un usuario solicita la eliminación de cualquier dato que el bot haya conservado previamente para el usuario.</span><span class="sxs-lookup"><span data-stu-id="7a528-139">A bot receives a **deleteUserData** activity when a user requests deletion of any data that the bot has previously persisted for him or her.</span></span> <span data-ttu-id="7a528-140">Si el bot recibe este tipo de actividad, debe eliminar cualquier información de identificación personal (PII) que haya almacenado previamente para el usuario que ha realizado la solicitud.</span><span class="sxs-lookup"><span data-stu-id="7a528-140">If your bot receives this type of activity, it should delete any personally identifiable information (PII) that it has previously stored for the user that made the request.</span></span>

## <a name="endofconversation"></a><span data-ttu-id="7a528-141">endOfConversation</span><span class="sxs-lookup"><span data-stu-id="7a528-141">endOfConversation</span></span> 

<span data-ttu-id="7a528-142">Un bot recibe una actividad **endOfConversation** para indicar que el usuario ha puesto fin a la conversación.</span><span class="sxs-lookup"><span data-stu-id="7a528-142">A bot receives an **endOfConversation** activity to indicate that the user has ended the conversation.</span></span> <span data-ttu-id="7a528-143">Un bot podría enviar una actividad **endOfConversation** para indicar al usuario que la conversación está llegando a su fin.</span><span class="sxs-lookup"><span data-stu-id="7a528-143">A bot may send an **endOfConversation** activity to indicate to the user that the conversation is ending.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="7a528-144">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="7a528-144">Additional resources</span></span>

- [<span data-ttu-id="7a528-145">Creación de mensajes</span><span class="sxs-lookup"><span data-stu-id="7a528-145">Create messages</span></span>](bot-framework-rest-connector-create-messages.md)
- [<span data-ttu-id="7a528-146">Envío y recepción de mensajes</span><span class="sxs-lookup"><span data-stu-id="7a528-146">Send and receive messages</span></span>](bot-framework-rest-connector-send-and-receive-messages.md)

[Activity]: bot-framework-rest-connector-api-reference.md#activity-object