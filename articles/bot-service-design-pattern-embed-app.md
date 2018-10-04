---
title: Inserción de un bot en una aplicación | Microsoft Docs
description: Obtenga información sobre cómo diseñar un bot que se incrustará en otra aplicación.
author: matvelloso
ms.author: mateusv
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 08/15/2018
ms.openlocfilehash: 53148b178b937fdee5bde9a47ef016eedbb92da4
ms.sourcegitcommit: f0b22c6286e44578c11c9f15d22b542c199f0024
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2018
ms.locfileid: "47403901"
---
# <a name="embed-a-bot-in-an-app"></a><span data-ttu-id="f9b57-103">Inserción de un bot en una aplicación</span><span class="sxs-lookup"><span data-stu-id="f9b57-103">Embed a bot in an app</span></span>

<span data-ttu-id="f9b57-104">Aunque los bots normalmente existen fuera de las aplicaciones, también puede integrarse con las aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="f9b57-104">Although bots most commonly exist outside of apps, they can also be integrated with apps.</span></span> <span data-ttu-id="f9b57-105">Por ejemplo, puede insertar un [bot de conocimiento](~/bot-service-design-pattern-knowledge-base.md) dentro de una aplicación para ayudar a los usuarios a encontrar información que, de lo contrario, podría ser difícil de ubicar dentro de las estructuras complejas de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f9b57-105">For example, you could embed a [knowledge bot](~/bot-service-design-pattern-knowledge-base.md) within an app to help users find information that might otherwise be challenging to locate within complex app structures.</span></span> <span data-ttu-id="f9b57-106">Podría insertar un bot dentro de una aplicación de departamento de soporte técnico para que actúe como primer respondedor a solicitudes entrantes de los usuarios.</span><span class="sxs-lookup"><span data-stu-id="f9b57-106">You could embed a bot within a help desk app to act as the first responder to incoming user requests.</span></span> <span data-ttu-id="f9b57-107">El bot podría resolver problemas sencillos de manera independiente y [derivar](~/bot-service-design-pattern-handoff-human.md) problemas más complejos a un agente humano.</span><span class="sxs-lookup"><span data-stu-id="f9b57-107">The bot could independently resolve simple issues and [hand off](~/bot-service-design-pattern-handoff-human.md) more complex issues to a human agent.</span></span> 

## <a name="integrating-bot-with-app"></a><span data-ttu-id="f9b57-108">Integración del bot con la aplicación</span><span class="sxs-lookup"><span data-stu-id="f9b57-108">Integrating bot with app</span></span>

<span data-ttu-id="f9b57-109">La forma de integrar un bot con una aplicación varía según el tipo de aplicación.</span><span class="sxs-lookup"><span data-stu-id="f9b57-109">The way to integrate a bot with an app varies depending on the type of app.</span></span> 

### <a name="native-mobile-app"></a><span data-ttu-id="f9b57-110">Aplicación móvil nativa</span><span class="sxs-lookup"><span data-stu-id="f9b57-110">Native mobile app</span></span>

<span data-ttu-id="f9b57-111">Una aplicación que se crea en código nativo puede comunicarse con Bot Framework mediante [Direct Line API][directLineAPI], ya sea a través de REST o websockets.</span><span class="sxs-lookup"><span data-stu-id="f9b57-111">An app that is created in native code can communicate with the Bot Framework by using the [Direct Line API][directLineAPI], either via REST or websockets.</span></span>

### <a name="web-based-mobile-app"></a><span data-ttu-id="f9b57-112">Aplicación móvil basada en web</span><span class="sxs-lookup"><span data-stu-id="f9b57-112">Web-based mobile app</span></span>

<span data-ttu-id="f9b57-113">Una aplicación móvil que se compila con lenguaje y marcos web, como <a href="https://cordova.apache.org/" target="_blank">Cordova</a>, puede comunicarse con Bot Framework mediante los mismos componentes que usaría un [bot insertado en un sitio web](~/bot-service-design-pattern-embed-web-site.md), simplemente encapsulado dentro del shell de una aplicación nativa.</span><span class="sxs-lookup"><span data-stu-id="f9b57-113">A mobile app that is built by using web language and frameworks such as <a href="https://cordova.apache.org/" target="_blank">Cordova</a> may communicate with the Bot Framework by using the same components that a [bot embedded within a website](~/bot-service-design-pattern-embed-web-site.md) would use, just encapsulated within a native app's shell.</span></span>

### <a name="iot-app"></a><span data-ttu-id="f9b57-114">Aplicación de IoT</span><span class="sxs-lookup"><span data-stu-id="f9b57-114">IoT app</span></span>

<span data-ttu-id="f9b57-115">Una aplicación de IoT puede comunicarse con Bot Framework mediante [Direct Line API][directLineAPI].</span><span class="sxs-lookup"><span data-stu-id="f9b57-115">An IoT app can communicate with the Bot Framework by using the [Direct Line API][directLineAPI].</span></span> <span data-ttu-id="f9b57-116">En algunos escenarios, también puede usar <a href="https://www.microsoft.com/cognitive-services/" target="_blank">Microsoft Cognitive Services</a> para habilitar funcionalidades como reconocimiento de imágenes y voz.</span><span class="sxs-lookup"><span data-stu-id="f9b57-116">In some scenarios, it may also use <a href="https://www.microsoft.com/cognitive-services/" target="_blank">Microsoft Cognitive Services</a> to enable capabilities such as image recognition and speech.</span></span>

### <a name="other-types-of-apps-and-games"></a><span data-ttu-id="f9b57-117">Otros tipos de aplicaciones y juegos</span><span class="sxs-lookup"><span data-stu-id="f9b57-117">Other types of apps and games</span></span>

<span data-ttu-id="f9b57-118">Otros tipos de aplicaciones y juegos pueden comunicarse con Bot Framework mediante [Direct Line API][directLineAPI].</span><span class="sxs-lookup"><span data-stu-id="f9b57-118">Other types of apps and games can communicate with the Bot Framework by using the [Direct Line API][directLineAPI].</span></span> 

## <a name="creating-a-cross-platform-mobile-app-that-runs-a-bot"></a><span data-ttu-id="f9b57-119">Creación de una aplicación móvil multiplataforma que ejecuta un bot</span><span class="sxs-lookup"><span data-stu-id="f9b57-119">Creating a cross-platform mobile app that runs a bot</span></span>

<span data-ttu-id="f9b57-120">Este ejemplo de creación de una aplicación móvil que ejecuta un bot usa <a href="https://www.xamarin.com/" target="_blank">Xamarin</a>, una herramienta popular para crear aplicaciones móviles multiplataforma.</span><span class="sxs-lookup"><span data-stu-id="f9b57-120">This example of creating a mobile app that runs a bot uses <a href="https://www.xamarin.com/" target="_blank">Xamarin</a>, a popular tool for building cross-platform mobile applications.</span></span> 

<span data-ttu-id="f9b57-121">En primer lugar, cree un componente de vista web simple y úsela para hospedar un <a href="https://github.com/Microsoft/BotFramework-WebChat" target="_blank">control de chat en web</a>.</span><span class="sxs-lookup"><span data-stu-id="f9b57-121">First, create a simple web view component and use it to host a <a href="https://github.com/Microsoft/BotFramework-WebChat" target="_blank">web chat control</a>.</span></span> <span data-ttu-id="f9b57-122">A continuación, mediante Azure Portal, agregue el canal del chat en web.</span><span class="sxs-lookup"><span data-stu-id="f9b57-122">Then, using Azure portal, add the Web Chat channel.</span></span> 

<span data-ttu-id="f9b57-123">A continuación, especifique la dirección URL de chat en web registrada como origen para el control de vista web en la aplicación de Xamarin:</span><span class="sxs-lookup"><span data-stu-id="f9b57-123">Next, specify the registered web chat URL as the source for the web view control in the Xamarin app:</span></span>

```cs
public class WebPage : ContentPage
{
    public WebPage()
    {
        var browser = new WebView();
        browser.Source = "https://webchat.botframework.com/embed/<YOUR SECRET KEY HERE>";
        this.Content = browser;
    }
}
```

<span data-ttu-id="f9b57-124">Con este proceso, puede crear una aplicación móvil multiplataforma que represente la vista web insertada con el control de chat en web.</span><span class="sxs-lookup"><span data-stu-id="f9b57-124">Using this process, you can create a cross-platform mobile application that renders the embedded web view with the web chat control.</span></span>

![Canal secundario](~/media/bot-service-design-pattern-embed-app/xamarin-apps.png)

<!-- TODO: No sample bot available
## Sample code

For a complete sample that shows how to create a cross-platform mobile app that runs a bot (as described in this article), see the <a href="https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/capability-BotInApps" target="_blank">Bot in Apps sample</a> in GitHub.
-->

## <a name="additional-resources"></a><span data-ttu-id="f9b57-126">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="f9b57-126">Additional resources</span></span>

- <span data-ttu-id="f9b57-127">[Direct Line API][directLineAPI] (API de Direct Line)</span><span class="sxs-lookup"><span data-stu-id="f9b57-127">[Direct Line API][directLineAPI]</span></span>
- <span data-ttu-id="f9b57-128"><a href="https://www.microsoft.com/cognitive-services/" target="_blank">Microsoft Cognitive Services</a></span><span class="sxs-lookup"><span data-stu-id="f9b57-128"><a href="https://www.microsoft.com/cognitive-services/" target="_blank">Microsoft Cognitive Services</a></span></span>

[directLineAPI]: https://docs.botframework.com/en-us/restapi/directline3/#navtitle
