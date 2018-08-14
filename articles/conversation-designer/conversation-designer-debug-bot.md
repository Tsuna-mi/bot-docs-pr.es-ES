---
title: Probar un bot | Microsoft Docs
description: Probar y depurar el bot de Conversation Designer.
author: vkannan
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ROBOTS: NoIndex, NoFollow
ms.openlocfilehash: b08bd96bc7c413ff7cb6db4899c8c0d3ad613ab8
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39306269"
---
# <a name="test-your-conversation-designer-bot"></a>Probar el bot de Conversation Designer
> [!IMPORTANT]
> Conversation Designer no está disponible para todos los clientes todavía. Habrá más información sobre la disponibilidad de Conversation Designer más adelante este año.

Conversation Designer ofrece herramientas útiles de depuración a través de varias ventanas de salida diferentes (que se encuentra en la parte inferior de la pantalla). Para comenzar las pruebas y la depuración, puede hacer clic en el botón **Probar** que se encuentra en la esquina superior derecha de la ventana. 

## <a name="validation-errors"></a>Errores de validación
Hay varias condiciones en las que se producen errores de validación. Por ejemplo:  
- Falta una respuesta al desencadenador 
- Hay información parcialmente completa en el flujo
- Faltan desencadenadores y código subyacente para los estados de plantilla de respuesta condicional, de decisión y de proceso
- Se detecta un bucle infinito o recursivo en las referencias de plantilla 
- El cuadro de diálogo tiene un estado huérfano que no está conectado al flujo.
- Se agregó una tarea, pero faltan el desencadenador o la acción 


## <a name="javascript-output"></a>Salida de JavaScript
Puede adjuntar un script a la ejecución de una respuesta para proporcionar funcionalidad adicional. Si se produce un error en el script, se mostrará aquí. Puede agregar `console.log()` o `error.log()` o `debug.log()` a la lógica de negocios de su bot, que mostrará los mensajes en la ventana de salida. Por ejemplo: 

``` javascript
module.exports.Respond_beforeResponse = function(context) {
    console.log(JSON.stringify(context));
}
```

## <a name="runtime-output"></a>Salida en tiempo de ejecución
Aquí se muestran los errores o excepciones generados por el tiempo de ejecución. Por ejemplo, si el bot no responde, examine la salida para investigar la causa de la excepción o error. Si hay demasiados mensajes en la ventana de salida, haga clic en **Borrar todo** y, a continuación, pruebe de nuevo el bot enviando un mensaje al bot para ver los detalles del error. 

## <a name="next-step"></a>Paso siguiente
> [!div class="nextstepaction"]
> [Import and export bot](conversation-designer-export-import-bot.md) (Importación y exportación de bots)
