---
title: Introducción a las tareas | Microsoft Docs
description: Obtenga información sobre las tareas del bot de Conversation Designer.
author: vkannan
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ROBOTS: NoIndex, NoFollow
ms.openlocfilehash: 685a0296f1bfa5452c28f4ec4d2e4e439f09baca
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305724"
---
# <a name="tasks-in-conversation-designer-bots"></a>Tareas de los bots de Conversation Designer
> [!IMPORTANT]
> Conversation Designer no está disponible para todos los clientes todavía. Habrá más información sobre la disponibilidad de Conversation Designer más adelante este año.

Los bots de Conversation Designer se componen de un grupo de tareas relacionadas. Una **tarea** es una acción que realiza el bot en respuesta a una actividad o solicitud de usuario específica. Por ejemplo, un bot de un restaurante podría incluir estas tareas: "Buscar ubicaciones", "Reservar una mesa", "Pedir comida" y "Administrar reservas". Cada tarea corresponde a un objetivo de usuario diferente. 

## <a name="triggers-and-actions"></a>Desencadenadores y acciones
Una tarea llevará a cabo una acción cuando se cumple una condición del desencadenador. Todas las tareas siguen este modelo: **desencadenador IF**, **acción DO**.

Un **desencadenador** puede ser de uno de estos dos tipos:
1. **El usuario se une a una conversación o la abandona** (iniciado por la actividad): una tarea desencadenada por "el usuario se une a una conversación o la abandona" realizará una acción cuando un usuario inicia o finaliza una conversación con el bot. Este desencadenador es útil para enviar un mensaje de introducción al usuario. 
2. **El usuario dice o escribe algo al bot** (iniciado por el mensaje): una tarea desencadenada por "el usuario dice o escribe algo al bot" realiza una acción en respuesta al mensaje del usuario. El mensaje del usuario se interpreta mediante un **reconocedor**.

Cuando se cumple una condición del desencadenador, una tarea realizará una de las acciones siguientes:
- **Dar una respuesta**: una respuesta puede ser cualquier combinación de texto mostrado, texto hablado o tarjeta enriquecida. Con esta acción, el bot envía una respuesta al usuario y completa la tarea. Una respuesta es una conversación de un solo turno, lo que significa que la tarea no espera mensajes de seguimiento del usuario.
- **Iniciar un diálogo**: un diálogo es una conversación de varios turnos entre el bot y el usuario. Los diálogos resultan particularmente útiles cuando el bot necesita mantener una conversación fluida con un usuario para recopilar la información necesaria para completar una tarea.
- **Ejecutar una función de script**: el bot puede ejecutar un script personalizado que se escribe para llamar a un servicio back-end para completar una tarea.

## <a name="tips-for-modeling-tasks"></a>Sugerencias para modelar tareas

1. Cada bot debe diseñarse para realizar varias tareas diferentes para el cliente. Debe considerar detenidamente la lista de esas funciones y crear una tarea para cada una de ellas.
2. Para configurar los desencadenadores, piense en cómo puede detectar la intención del cliente para realizar una tarea. ¿Cómo *sabrá* el bot lo que quiere hacer el cliente?
3. Si usa Language Understanding como reconocedor, asegúrese de incluir ejemplos suficientes para las distintas formas en que el usuario puede expresar la intención de realizar una tarea. "Reservar una mesa" debe desencadenar la misma acción que "realizar una reserva" o "realizar la reserva de una mesa".
4. Considere la posibilidad de agregar ejemplos a la intención de Language Understanding que sean gramáticamente correctos.
5. Si tiene intención de publicar su bot como una habilidad de Cortana, considere la posibilidad de agregar frases de ejemplo que funcionen bien con desencadenadores de Cortana. "Pida al nombre de bot que haga algo". 
6. Para los reconocedores de código, escriba patrones de expresión regular para determinar la intención del usuario. Los reconocedores de código también pueden devolver las entidades que puede usar en la tarea.
7. Si **dar una respuesta** es la *acción* de la tarea, puede incluir opcionalmente una tarjeta adaptable para representar en canales admitidos.

## <a name="next-step"></a>Paso siguiente
> [!div class="nextstepaction"]
> [Reconocedor de LUIS](conversation-designer-luis.md)
