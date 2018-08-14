---
title: Bot Builder SDK para .NET | Microsoft Docs
description: Empieza a trabajar con el Bot Builder SDK para. NET, un marco eficaz y fácil de usar para la creación de bots.
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: bd89017714a46c541468ccc9ed9990dde14719b7
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305081"
---
# <a name="bot-builder-sdk-for-net"></a>Bot Builder SDK para .NET
> [!div class="op_single_selector"]
> - [.NET](../dotnet/bot-builder-dotnet-overview.md)
> - [Node.js](../nodejs/bot-builder-nodejs-overview.md)
> - [REST](../rest-api/bot-framework-rest-overview.md)

El Bot Builder SDK para .NET es un marco eficaz para crear bots que pueden controlar tanto interacciones de forma libre como conversaciones más guiadas, donde el usuario selecciona entre valores posibles. Es fácil de usar y aprovecha C# para proporcionar una manera conocida de escribir bots para los desarrolladores de .NET.

Mediante el SDK, puede crear bots que aprovechen las características siguientes: 

- Sistema eficaz de cuadros de diálogo con cuadros que están aislados y que admiten composición
- Avisos integrados para cosas sencillas, como Sí/No, cadenas, números y enumeraciones
- Cuadros de diálogo integrados que usan marcos de inteligencia artificial eficaces, como <a href="http://luis.ai" target="_blank">LUIS</a>
- FormFlow para generar un bot automáticamente (desde una clase de C#), que guía al usuario a través de la conversación, brindando ayuda, navegación, aclaraciones y confirmaciones según sea necesario

> [!IMPORTANT]
> El 31 de julio de 2017 se implementaron cambios importantes en el protocolo de seguridad de Bot Framework. Para evitar que estos cambios afecten negativamente a su bot, debe asegurarse de que la aplicación use el Bot Builder SDK v3.5 o posterior. Si ha creado un bot mediante un SDK que obtuvo antes del 5 de enero de 2017 (la fecha de lanzamiento del Bot Builder SDK 3.5), asegúrese de actualizar a Bot Builder SDK v3.5 o posterior.

## <a name="get-the-sdk"></a>Obtención del SDK

El SDK está disponible como paquete de NuGet y como código abierto en <a href="https://github.com/Microsoft/BotBuilder" target="_blank">GitHub</a>.

> [!IMPORTANT]
> El Bot Builder SDK para .NET requiere .NET Framework 4.6 o posterior. Si va a agregar el SDK a un proyecto existente que tiene como destino una versión anterior de .NET Framework, deberá actualizar primero el proyecto que tienen como destino .NET Framework 4.6.

Para instalar el SDK dentro de un proyecto de Visual Studio, complete los pasos siguientes:

1. En el **Explorador de soluciones**, haga clic con el botón derecho en el nombre del proyecto y seleccione **Administrar paquetes NuGet…**.
2. En la pestaña **Examinar**, escriba "Microsoft.Bot.Builder" en el cuadro de búsqueda.
3. Seleccione **Microsoft.Bot.Builder** en la lista de resultados, haga clic en **Instalar** y acepte los cambios.

## <a name="get-code-samples"></a>Obtención de ejemplos de código

Este SDK incluye [código fuente de ejemplo](bot-builder-dotnet-samples.md) que usa las características del Bot Builder SDK para. NET.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre la creación de bots con el Bot Builder SDK para .NET, revise los artículos a lo largo de esta sección, que comienzan con:

- [Guía de inicio rápido](bot-builder-dotnet-quickstart.md): compile y pruebe rápidamente un bot simple siguiendo las instrucciones de este tutorial paso a paso.
- [Conceptos clave](bot-builder-dotnet-concepts.md): aprenda sobre los conceptos clave del Bot Builder SDK para .NET.

Si tiene problemas o sugerencias sobre el Bot Builder SDK para .NET, consulte el [soporte técnico](../bot-service-resources-links-help.md) para obtener una lista de los recursos disponibles. 
