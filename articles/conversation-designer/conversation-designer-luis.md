---
title: Definición de un reconocedor de LUIS como desencadenador de tareas | Microsoft Docs
description: Obtenga información sobre cómo configurar el reconocedor de comprensión de lenguaje como desencadenador de tareas mediante LUIS.ai
author: vkannan
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ROBOTS: NoIndex, NoFollow
ms.openlocfilehash: 39fe222fb1d54346b33617c425b1fdf2d56daa0d
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39306248"
---
# <a name="define-a-luis-recognizer-as-task-trigger"></a>Definición de un reconocedor de LUIS como desencadenador de tareas
> [!IMPORTANT]
> Conversation Designer no está disponible para todos los clientes todavía. Habrá más información sobre la disponibilidad de Conversation Designer más adelante este año.

A menudo, el usuario expresa su intención de realizar una tarea en lenguaje natural. Con Conversation Designer, puede configurar fácilmente un modelo de comprensión de lenguaje natural para el bot con la tecnología de <a href="https://luis.ai" target="_blank">LUIS.ai</a>.

Para ello, seleccione el tipo de desencadenador **User says or types something** (El usuario dice o escribe algo). Esto le dará la opción de especificar el nombre de la **intención**. 

Puede buscar las intenciones existentes o crear una en el campo **Which language intent?** (¿Qué intención de lenguaje?).

## <a name="create-a-new-intent"></a>Creación de una nueva intención

Para crear una intención, escriba el nombre de la intención y haga clic en **Create new intent** (Crear intención). Escriba las expresiones de ejemplo para las posibles cosas que espera que los usuarios digan que deberían desencadenar esta tarea específica.

Por ejemplo, un bot de café debe ser capaz de realizar la tarea de encontrar las cafeterías cerca del usuario. Para controlar este escenario, seleccione **User says or types something** y establezca el **Intent name** (Nombre de intención) en "LocationNearMe". A continuación, proporcione expresiones de ejemplo para intención. Por ejemplo:  
- *buscar ubicaciones a mi alrededor*
- *buscar cafeterías a mi alrededor*
- *¿está abierta la tienda Redmond ahora?*
- *¿hay una tienda en Seattle?*
- *¿qué cafeterías hay abiertas ahora?*
- *¿dónde puedo comer algo?*
- *Me gustaría comer algo*
- *Tengo hambre*

Escriba tantas expresiones como se le ocurran que ayuden a transmitir la intención del usuario para desencadenar esta tarea específica.

## <a name="default-intents-provisioned-for-your-bot"></a>Intenciones predeterminadas aprovisionadas para el bot

De manera predeterminada, el bot se aprovisiona con 4 intenciones. 
- **None** (Ninguna): se trata de la intención de reserva (predeterminada) para el bot. Utilice esta intención para ayudar a capturar cosas a las que el bot no sabe cómo responder todavía.
- **Help** (Ayuda): configure expresiones de ejemplo que ayuden a decidir si el usuario necesita ayuda. *Ayuda, necesito ayuda, ¿qué puedo decir?, estoy atascado* y así sucesivamente.
- **Greeting** (Saludo): configure expresiones de ejemplo que ayuden a asocial la intención de saludo, como *Hola, Qué tal, Buenos días, ¿Cómo estás, bot?* y así sucesivamente.
- **Cancel** (Cancelar): configure expresiones de ejemplo para la intención de cancelar. *Detener, Cancelar esto, no lo hagas, revertir* y así sucesivamente.

## <a name="create-and-label-entities"></a>Creación y etiquetado de entidades

Además de ayudarle a determinar la intención del usuario, la comprensión de lenguaje puede ayudarle a determinar las entidades específicas de interés correspondientes a la tarea. Por ejemplo, cuando el usuario dice *buscar cafeterías cerca de Redmond*, puede que quiera extraer *Redmond* como valor posible para la entidad ***location***. 

Para configurar las entidades de la tarea, desde la cadena **Utterance**, seleccione la parte de la expresión que debe ser un ejemplo de un valor de la entidad. Asigne esto a una entidad existente o cree una. Para crear una entidad, escriba el nombre de la entidad en el campo **Search or create** (Buscar o crear) y haga clic en **Crear nueva entidad**. 

# <a name="supported-entity-types"></a>Tipos de entidad admitidos

La comprensión de lenguaje da la facultad de crear diferentes tipos de entidades. Al crear una entidad, es necesario especificar un `type`. 

Los tipos disponibles son:

- **Simple**: este es el tipo *predeterminado*.
- **List**: use este tipo si la entidad tiene un conjunto finito de valores posibles. Ejemplo: *Color*, *City*.
- **Hierarchical**: use este tipo para crear entidades con la relación de elementos primarios y secundarios. Ejemplo: *fromCity* y *toCity* tienen la entidad *City* como elemento primario.
- **Composite**: use este tipo para crear grupos de valores que conformen una unidad significativa. Ejemplo: *City* y *State* conforman la entidad *Location*.

<!-- # pre-built entity types TBD -->

# <a name="entities-in-use"></a>Entidades en uso

A medida que crea y agrega entidades a la sección de comprensión de lenguaje, la tabla **Entities in use** (Entidades en uso) se actualiza con la lista de entidades que usa esta tarea específica. Puede agregar manualmente a la lista otras entidades que se usan en esta tarea. 

Las opciones disponibles son la siguientes:

- **Code**: se trata de una entidad que se crea en el script personalizado. Puede especificarla aquí para ayudar con la creación de características como intellisense.

<!-- # Use as help tip TBD  -->

## <a name="next-step"></a>Paso siguiente
> [!div class="nextstepaction"]
> [Reconocedor de código](conversation-designer-code-recognizer.md)
