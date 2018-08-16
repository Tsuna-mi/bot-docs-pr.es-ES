---
title: Guardar un bot de Conversation Designer | Microsoft Docs
description: Aprenda a guardar y entrenar el modelo de reconocimiento del lenguaje y a preparar el reconocimiento de voz en un bot de Conversation Designer.
author: vkannan
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ROBOTS: NoIndex, NoFollow
ms.openlocfilehash: 3a911158c379f879c0be604fb5e8ba30ab22ee44
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39306221"
---
# <a name="saving-your-conversation-designer-bot"></a>Guardar el bot de Conversation Designer
> [!IMPORTANT]
> Conversation Designer no está disponible para todos los clientes todavía. Habrá más información sobre la disponibilidad de Conversation Designer más adelante este año.

Mientras trabaja en Conversation Designer, todo el trabajo se almacena en caché en la memoria del explorador. Para confirmar los cambios, haga clic en el botón **Guardar** situado en la esquina superior izquierda del menú de navegación izquierdo. Para evitar perder el trabajo, se recomienda que lo guarde con frecuencia. Además de hacer clic en el botón **Guardar**, también puede guardar su trabajo mediante el método abreviado de teclado **CTRL+S**.

## <a name="trains-luis-and-primes-speech-recognition"></a>Entrena LUIS y prepara el reconocimiento de voz

Al hacer clic en el botón **Guardar** se guardarán los cambios en el bot y se llevarán a cabo algunas tareas adicionales. A diferencia del método abreviado del teclado, si hace clic en el botón **Guardar** también indicará a Conversation Designer que debe realizar las siguientes tareas:

- Entrena las nuevas intenciones y entidades de LUIS para el bot y publica el modelo de LUIS localmente en Bot Service (si es necesario). Estas intenciones se pueden agregar en Conversation Designer o en la aplicación de [LUIS](https://luis.ai) correspondiente del bot.
- Actualiza el entorno en tiempo de ejecución de la conversación para que use el nuevo modelo de LUIS.
- Prepara el reconocimiento de voz mediante la preparación y envío de expresiones de ejemplo a Microsoft Cognitive Services, lo cual contribuye a mejorar significativamente la precisión del reconocimiento de voz para este bot.

## <a name="next-step"></a>Paso siguiente
> [!div class="nextstepaction"]
> [Probar bot](conversation-designer-debug-bot.md)
