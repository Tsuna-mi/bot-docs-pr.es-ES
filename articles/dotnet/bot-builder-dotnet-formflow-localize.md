---
title: Localización del contenido de un formulario | Microsoft Docs
description: Obtenga información sobre cómo localizar el contenido de un formulario con FormFlow y el Bot Builder SDK para. NET.
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: bb0ac4b8e3fa34ec8863bb323ae968db37972a6f
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305100"
---
# <a name="localize-form-content"></a><span data-ttu-id="304b8-103">Localización del contenido de un formulario</span><span class="sxs-lookup"><span data-stu-id="304b8-103">Localize form content</span></span>

<span data-ttu-id="304b8-104">El idioma de localización de un formulario viene determinado por las propiedades [CurrentUICulture](https://msdn.microsoft.com/en-us/library/system.threading.thread.currentuiculture(v=vs.110).aspx) y [CurrentCulture](https://msdn.microsoft.com/en-us/library/system.threading.thread.currentculture(v=vs.110).aspx) del subproceso actual.</span><span class="sxs-lookup"><span data-stu-id="304b8-104">A form's localization language is determined by the current thread's [CurrentUICulture](https://msdn.microsoft.com/en-us/library/system.threading.thread.currentuiculture(v=vs.110).aspx) and [CurrentCulture](https://msdn.microsoft.com/en-us/library/system.threading.thread.currentculture(v=vs.110).aspx).</span></span> <span data-ttu-id="304b8-105">De manera predeterminada, la referencia cultural se obtiene del campo **Configuración regional** del mensaje actual, pero es posible invalidar este comportamiento predeterminado.</span><span class="sxs-lookup"><span data-stu-id="304b8-105">By default, the culture derives from the **Locale** field of the current message, but you can override that default behavior.</span></span> <span data-ttu-id="304b8-106">Según cómo se construye el bot, la información localizada puede proceder de hasta tres orígenes diferentes:</span><span class="sxs-lookup"><span data-stu-id="304b8-106">Depending on how your bot is constructed, localized information may come from up to three different sources:</span></span>

- <span data-ttu-id="304b8-107">la localización integrada para **PromptDialog** y **FormFlow**</span><span class="sxs-lookup"><span data-stu-id="304b8-107">the built-in localization for **PromptDialog** and **FormFlow**</span></span>
- <span data-ttu-id="304b8-108">un archivo de recursos que genera para las cadenas estáticas del formulario</span><span class="sxs-lookup"><span data-stu-id="304b8-108">a resource file that you generate for the static strings in your form</span></span>
- <span data-ttu-id="304b8-109">un archivo de recursos que crea con cadenas para los mensajes, confirmaciones o campos que se calculan dinámicamente</span><span class="sxs-lookup"><span data-stu-id="304b8-109">a resource file that you create with strings for dynamically-computed fields, messages or confirmations</span></span>

## <a name="generate-a-resource-file-for-the-static-strings-in-your-form"></a><span data-ttu-id="304b8-110">Generación de un archivo de recursos para las cadenas estáticas del formulario</span><span class="sxs-lookup"><span data-stu-id="304b8-110">Generate a resource file for the static strings in your form</span></span>

<span data-ttu-id="304b8-111">Las cadenas estáticas en un formulario incluyen las cadenas que genera el formulario a partir de la información de la clase de C# y las cadenas que especifica como avisos, plantillas, mensajes o confirmaciones.</span><span class="sxs-lookup"><span data-stu-id="304b8-111">Static strings in a form include the strings that the form generates from the information in your C# class and the strings that you specify as prompts, templates, messages or confirmations.</span></span> <span data-ttu-id="304b8-112">Las cadenas que se generan a partir de plantillas integradas no se consideran cadenas estáticas, puesto que esas cadenas ya están localizadas.</span><span class="sxs-lookup"><span data-stu-id="304b8-112">Strings that are generated from built-in templates are not considered static strings, since those strings are already localized.</span></span> <span data-ttu-id="304b8-113">Dado que muchas de las cadenas en un formulario se generan automáticamente, no es posible usar directamente las cadenas de recursos de C# normales.</span><span class="sxs-lookup"><span data-stu-id="304b8-113">Since many of the strings in a form are automatically generated, it is not feasible to use normal C# resource strings directly.</span></span> <span data-ttu-id="304b8-114">En su lugar, puede generar un archivo de recursos para las cadenas estáticas en el formulario mediante una llamada a `IFormBuilder.SaveResources` o mediante la herramienta **RView** que se incluye con el Bot Builder SDK para. NET.</span><span class="sxs-lookup"><span data-stu-id="304b8-114">Instead, you can generate a resource file for the static strings in your form either by calling `IFormBuilder.SaveResources` or by using the **RView** tool that is included with the BotBuilder SDK for .NET.</span></span>

### <a name="use-iformbuildersaveresources"></a><span data-ttu-id="304b8-115">Uso de IFormBuilder.SaveResources</span><span class="sxs-lookup"><span data-stu-id="304b8-115">Use IFormBuilder.SaveResources</span></span>

<span data-ttu-id="304b8-116">Puede generar un archivo de recursos mediante una llamada a [IFormBuilder.SaveResources][saveResources] en el formulario para guardar las cadenas en un archivo .resx.</span><span class="sxs-lookup"><span data-stu-id="304b8-116">You can generate a resource file by calling [IFormBuilder.SaveResources][saveResources] on your form to save the strings to a .resx file.</span></span>

### <a name="use-rview"></a><span data-ttu-id="304b8-117">Uso de RView</span><span class="sxs-lookup"><span data-stu-id="304b8-117">Use RView</span></span>

<span data-ttu-id="304b8-118">Como alternativa, puede generar un archivo de recursos que se base en el archivo .dll o .exe usando la herramienta <a href="https://github.com/Microsoft/BotBuilder/tree/master/CSharp/Tools/RView" target="_blank">RView</a> que se incluye en el Bot Builder SDK para. NET.</span><span class="sxs-lookup"><span data-stu-id="304b8-118">Alternatively, you can generate a resource file that is based upon your .dll or .exe by using the <a href="https://github.com/Microsoft/BotBuilder/tree/master/CSharp/Tools/RView" target="_blank">RView</a> tool that is included in the BotBuilder SDK for .NET.</span></span> <span data-ttu-id="304b8-119">Para generar el archivo .resx, ejecute **rview** y especifique el ensamblado que contiene el método de creación de formularios estáticos y la ruta de acceso a ese método.</span><span class="sxs-lookup"><span data-stu-id="304b8-119">To generate the .resx file, execute **rview** and specify the assembly that contains your static form-building method and the path to that method.</span></span> <span data-ttu-id="304b8-120">En este fragmento de código se muestra cómo generar el archivo de recursos `Microsoft.Bot.Sample.AnnotatedSandwichBot.SandwichOrder.resx` con **RView**.</span><span class="sxs-lookup"><span data-stu-id="304b8-120">This snippet shows how to generate the `Microsoft.Bot.Sample.AnnotatedSandwichBot.SandwichOrder.resx` resource file using **RView**.</span></span> 

```csharp
rview -g Microsoft.Bot.Sample.AnnotatedSandwichBot.dll Microsoft.Bot.Sample.AnnotatedSandwichBot.SandwichOrder.BuildForm
```

<span data-ttu-id="304b8-121">En este extracto se muestra parte del archivo .resx que se genera al ejecutar este comando **rview**.</span><span class="sxs-lookup"><span data-stu-id="304b8-121">This excerpt shows part of the .resx file that is generated by executing this **rview** command.</span></span>

```xml
<data name="Specials_description;VALUE" xml:space="preserve">
<value>Specials</value>
</data>
<data name="DeliveryAddress_description;VALUE" xml:space="preserve">
<value>Delivery Address</value>
</data>
<data name="DeliveryTime_description;VALUE" xml:space="preserve">
<value>Delivery Time</value>
</data>
<data name="PhoneNumber_description;VALUE" xml:space="preserve">
<value>Phone Number</value>
</data>
<data name="Rating_description;VALUE" xml:space="preserve">
<value>your experience today</value>
</data>
<data name="message0;LIST" xml:space="preserve">
<value>Welcome to the sandwich order bot!</value>
</data>
<data name="Sandwich_terms;LIST" xml:space="preserve">
<value>sandwichs?</value>
</data>
```

## <a name="configure-your-project"></a><span data-ttu-id="304b8-122">Configuración del proyecto</span><span class="sxs-lookup"><span data-stu-id="304b8-122">Configure your project</span></span>

<span data-ttu-id="304b8-123">Después de haber generado un archivo de recursos, agréguelo al proyecto y, a continuación, siga estos pasos para establecer el idioma neutro:</span><span class="sxs-lookup"><span data-stu-id="304b8-123">After you have generated a resource file, add it to your project and then set the neutral language by completing these steps:</span></span> 

1. <span data-ttu-id="304b8-124">Haga clic con el botón derecho en el proyecto y seleccione **Aplicación**.</span><span class="sxs-lookup"><span data-stu-id="304b8-124">Right-click on your project and select **Application**.</span></span>
2. <span data-ttu-id="304b8-125">Haga clic en **Información de ensamblado**.</span><span class="sxs-lookup"><span data-stu-id="304b8-125">Click **Assembly Information**.</span></span>
3. <span data-ttu-id="304b8-126">Seleccione el valor **Idioma neutro** que corresponde al idioma en que ha desarrollado el bot.</span><span class="sxs-lookup"><span data-stu-id="304b8-126">Select the **Neutral Language** value that corresponds to the language in which you developed your bot.</span></span>

<span data-ttu-id="304b8-127">Cuando se crea el formulario, el método [IFormBuilder.Build][build] buscará automáticamente recursos que contienen el nombre de tipo de formulario y los usará para localizar las cadenas estáticas en el formulario.</span><span class="sxs-lookup"><span data-stu-id="304b8-127">When your form is created, the [IFormBuilder.Build][build] method will automatically look for resources that contain your form type name and use them to localize the static strings in your form.</span></span> 

> [!NOTE]
> <span data-ttu-id="304b8-128">No es posible localizar los campos calculados dinámicamente que se definen mediante [Advanced.Field.SetDefine][setDefine] (como se describe en [Using Dynamic Fields](bot-builder-dotnet-formflow-formbuilder.md#dynamically-define-field-values-confirmations-and-messages) [Uso de campos dinámicos]) de la misma manera que los campos estáticos, puesto que las cadenas para los campos que se calculan dinámicamente se construyen en el momento en que se rellena el formulario.</span><span class="sxs-lookup"><span data-stu-id="304b8-128">Dynamically-computed fields that are defined using [Advanced.Field.SetDefine][setDefine] (as described in [Using Dynamic Fields](bot-builder-dotnet-formflow-formbuilder.md#dynamically-define-field-values-confirmations-and-messages)) cannot be localized in the same manner as static fields, since strings for dynamically-computed fields are constructed at the time the form is populated.</span></span> <span data-ttu-id="304b8-129">Sin embargo, puede localizar los campos calculados dinámicamente usando mecanismos de localización de C# normales.</span><span class="sxs-lookup"><span data-stu-id="304b8-129">However, you can localize dynamically-computed fields by using normal C# localization mechanisms.</span></span>

### <a name="localize-resource-files"></a><span data-ttu-id="304b8-130">Localización de archivos de recursos</span><span class="sxs-lookup"><span data-stu-id="304b8-130">Localize resource files</span></span> 

<span data-ttu-id="304b8-131">Después de haber agregado archivos de recursos al proyecto, puede localizarlos con el <a href="https://developer.microsoft.com/en-us/windows/develop/multilingual-app-toolkit" target="_blank">Kit de herramientas para aplicaciones multilingües (MAT)</a>.</span><span class="sxs-lookup"><span data-stu-id="304b8-131">After you have added resource files to your project, you can localize them by using the <a href="https://developer.microsoft.com/en-us/windows/develop/multilingual-app-toolkit" target="_blank">Multilingual App Toolkit (MAT)</a>.</span></span> <span data-ttu-id="304b8-132">Instale **MAT** y siga los pasos a continuación para habilitarlo para su proyecto:</span><span class="sxs-lookup"><span data-stu-id="304b8-132">Install **MAT**, then enable it for your project by completing these steps:</span></span>

1. <span data-ttu-id="304b8-133">Seleccione el proyecto en el Explorador de soluciones de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="304b8-133">Select your project in the Visual Studio Solution Explorer.</span></span>
2. <span data-ttu-id="304b8-134">Haga clic en **Herramientas**, **Kit de herramientas de aplicaciones multilingües** y **Habilitar**.</span><span class="sxs-lookup"><span data-stu-id="304b8-134">Click **Tools**, **Multilingual App Toolkit**, and **Enable**.</span></span>
3. <span data-ttu-id="304b8-135">Haga clic con el botón derecho en el proyecto y seleccione **Kit de herramientas de aplicaciones multilingües**, **Add Translations** (Agregar traducciones) para seleccionar las traducciones.</span><span class="sxs-lookup"><span data-stu-id="304b8-135">Right-click the project and select **Multilingual App Toolkit**, **Add Translations** to select the translations.</span></span> <span data-ttu-id="304b8-136">Esto creará archivos <a href="https://en.wikipedia.org/wiki/XLIFF" target="_blank">XLF</a> estándares del sector, que puede traducir de automática o manualmente.</span><span class="sxs-lookup"><span data-stu-id="304b8-136">This will create industry-standard <a href="https://en.wikipedia.org/wiki/XLIFF" target="_blank">XLF</a> files that you can automatically or manually translate.</span></span>

> [!NOTE]
> <span data-ttu-id="304b8-137">Aunque este artículo describe cómo usar el Kit de herramientas de aplicaciones multilingües para traducir el contenido, puede implementar la localización a través de una variedad de otros medios.</span><span class="sxs-lookup"><span data-stu-id="304b8-137">Although this article describes how to use the Multilingual App Toolkit to localize content, you may implement localization via a variety of other means.</span></span>

## <a name="see-it-in-action"></a><span data-ttu-id="304b8-138">Véalo en acción</span><span class="sxs-lookup"><span data-stu-id="304b8-138">See it in action</span></span>

<span data-ttu-id="304b8-139">Este ejemplo de código se basa en el de [Customize a form using FormBuilder](bot-builder-dotnet-formflow-formbuilder.md) (Personalización de un formulario mediante FormBuilder) para implementar la localización como se describió anteriormente.</span><span class="sxs-lookup"><span data-stu-id="304b8-139">This code example builds upon the one in [Customize a form using FormBuilder](bot-builder-dotnet-formflow-formbuilder.md) to implement localization as described above.</span></span> <span data-ttu-id="304b8-140">En este ejemplo, la clase `DynamicSandwich` (no se muestra aquí) contiene información de localización para los campos que se calculan dinámicamente, mensajes y confirmaciones.</span><span class="sxs-lookup"><span data-stu-id="304b8-140">In this example, the `DynamicSandwich` class (not shown here) contains localization information for dynamically-computed fields, messages and confirmations.</span></span>

[!code-csharp[Build localized form](../includes/code/dotnet-formflow-localize.cs#buildLocalizedForm)]

<span data-ttu-id="304b8-141">Este fragmento de código muestra la interacción resultante entre el bot y el usuario cuando `CurrentUICulture` es **Francés**.</span><span class="sxs-lookup"><span data-stu-id="304b8-141">This snippet shows the resulting interaction between bot and user when `CurrentUICulture` is **French**.</span></span>

```console
Bienvenue sur le bot d'ordre "sandwich" !
Quel genre de "sandwich" vous souhaitez sur votre "sandwich"?
 1. BLT
 2. Jambon Forêt Noire
 3. Poulet Buffalo
 4. Faire fondre le poulet et Bacon Ranch
 5. Combo de coupe à froid
 6. Boulette de viande Marinara
 7. Poulet rôti au four
 8. Rôti de boeuf
 9. Rotisserie poulet
 10. Italienne piquante
 11. Bifteck et fromage
 12. Oignon doux Teriyaki
 13. Thon
 14. Poitrine de dinde
 15. Veggie
> 2

Quel genre de longueur vous souhaitez sur votre "sandwich"?
 1. Six pouces
 2. Pied Long
> ?
* Vous renseignez le champ longueur.Réponses possibles:
* Vous pouvez saisir un numéro 1-2 ou des mots de la description. (Six pouces, ou Pied Long)
* Retourner à la question précédente.
* Assistance: Montrez les réponses possibles.
* Abandonner: Abandonner sans finir
* Recommencer remplir le formulaire. (Vos réponses précédentes sont enregistrées.)
* Statut: Montrer le progrès en remplissant le formulaire jusqu'à présent.
* Vous pouvez passer à un autre champ en entrant son nom. ("Sandwich", Longueur, Pain, Fromage, Nappages, Sauces, Adresse de remise, Délai de livraison, ou votre expérience aujourd'hui).
Quel genre de longueur vous souhaitez sur votre "sandwich"?
 1. Six pouces
 2. Pied Long
> 1

Quel genre de pain vous souhaitez sur votre "sandwich"?
 1. Neuf grains de blé
 2. Neuf grains miel avoine
 3. Italien
 4. Fromage et herbes italiennes
 5. Pain plat
> neuf
Par pain "neuf" vouliez-vous dire (1. Neuf grains miel avoine, ou 2. Neuf grains de blé)
```

## <a name="sample-code"></a><span data-ttu-id="304b8-142">Código de ejemplo</span><span class="sxs-lookup"><span data-stu-id="304b8-142">Sample code</span></span>

[!INCLUDE [Sample code](../includes/snippet-dotnet-formflow-samples.md)]

## <a name="additional-resources"></a><span data-ttu-id="304b8-143">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="304b8-143">Additional resources</span></span>

- [<span data-ttu-id="304b8-144">Características básicas de FormFlow</span><span class="sxs-lookup"><span data-stu-id="304b8-144">Basic features of FormFlow</span></span>](bot-builder-dotnet-formflow.md)
- [<span data-ttu-id="304b8-145">Características avanzadas de FormFlow</span><span class="sxs-lookup"><span data-stu-id="304b8-145">Advanced features of FormFlow</span></span>](bot-builder-dotnet-formflow-advanced.md)
- <span data-ttu-id="304b8-146">[Customize a form using FormBuilder](bot-builder-dotnet-formflow-formbuilder.md) (Personalización de un formulario mediante FormBuilder)</span><span class="sxs-lookup"><span data-stu-id="304b8-146">[Customize a form using FormBuilder](bot-builder-dotnet-formflow-formbuilder.md)</span></span>
- <span data-ttu-id="304b8-147">[Define a form using JSON schema](bot-builder-dotnet-formflow-json-schema.md) (Definición de un formulario mediante esquemas JSON)</span><span class="sxs-lookup"><span data-stu-id="304b8-147">[Define a form using JSON schema](bot-builder-dotnet-formflow-json-schema.md)</span></span>
- <span data-ttu-id="304b8-148">[Customize user experience with pattern language](bot-builder-dotnet-formflow-pattern-language.md) (Personalización de la experiencia de usuario con lenguaje de patrones)</span><span class="sxs-lookup"><span data-stu-id="304b8-148">[Customize user experience with pattern language](bot-builder-dotnet-formflow-pattern-language.md)</span></span>
- <span data-ttu-id="304b8-149"><a href="/dotnet/api/?view=botbuilder-3.11.0" target="_blank">Referencias de Bot Builder SDK para .NET</a></span><span class="sxs-lookup"><span data-stu-id="304b8-149"><a href="/dotnet/api/?view=botbuilder-3.11.0" target="_blank">Bot Builder SDK for .NET Reference</a></span></span>

[build]: /dotnet/api/microsoft.bot.builder.formflow.formbuilder-1.build 

[setDefine]: /dotnet/api/microsoft.bot.builder.formflow.advanced.field-1.setdefine

[saveResources]: /dotnet/api/microsoft.bot.builder.formflow.iform-1.saveresources
