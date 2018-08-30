---
title: Personalización de un formulario mediante FormBuilder | Microsoft Docs
description: Obtenga información sobre cómo cambiar y personalizar de forma dinámica el flujo y el contenido de las conversaciones con FormBuilder para Bot Builder SDK para .NET.
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: 3ad843530b70b75a6db728a399a315534d2234ac
ms.sourcegitcommit: 2dc75701b169d822c9499e393439161bc87639d2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42904198"
---
# <a name="customize-a-form-using-formbuilder"></a><span data-ttu-id="4c604-103">Personalización de un formulario mediante FormBuilder</span><span class="sxs-lookup"><span data-stu-id="4c604-103">Customize a form using FormBuilder</span></span>

[!INCLUDE [pre-release-label](../includes/pre-release-label-v3.md)]

<span data-ttu-id="4c604-104">En [Características básicas de FormFlow](bot-builder-dotnet-formflow.md) se describe una implementación básica de FormFlow que ofrece una experiencia de usuario más o menos genérica y en [Características avanzadas de FormFlow](bot-builder-dotnet-formflow-advanced.md) se describe cómo puede personalizar la experiencia del usuario mediante el uso de atributos y lógica de negocios.</span><span class="sxs-lookup"><span data-stu-id="4c604-104">[Basic features of FormFlow](bot-builder-dotnet-formflow.md) describes a basic FormFlow implementation that delivers a fairly generic user experience, and [Advanced features of FormFlow](bot-builder-dotnet-formflow-advanced.md) describes how you can customize user experience by using business logic and attributes.</span></span> <span data-ttu-id="4c604-105">En este artículo se describe cómo puede usar [FormBuilder][formBuilder] para personalizar la experiencia del usuario aún más, especificando la secuencia en que el formulario ejecuta los pasos y definiendo de forma dinámica los valores de campos, las confirmaciones y los mensajes.</span><span class="sxs-lookup"><span data-stu-id="4c604-105">This article describes how you can use [FormBuilder][formBuilder] to customize user experience even further, by specifying the sequence in which the form executes steps and dynamically defining field values, confirmations, and messages.</span></span> 

## <a name="dynamically-define-field-values-confirmations-and-messages"></a><span data-ttu-id="4c604-106">Definición dinámica de valores de campo, confirmaciones y mensajes</span><span class="sxs-lookup"><span data-stu-id="4c604-106">Dynamically define field values, confirmations, and messages</span></span>

<span data-ttu-id="4c604-107">Con FormBuilder, puede definir de forma dinámica valores de campo, confirmaciones y mensajes.</span><span class="sxs-lookup"><span data-stu-id="4c604-107">Using FormBuilder, you can dynamically define field values, confirmations, and messages.</span></span>

### <a name="dynamically-define-field-values"></a><span data-ttu-id="4c604-108">Definición dinámica de valores de campo</span><span class="sxs-lookup"><span data-stu-id="4c604-108">Dynamically define field values</span></span> 

<span data-ttu-id="4c604-109">Un bot de sándwich diseñado para agregar una bebida o galleta gratuitas a cualquier pedido que especifique un sándwich grande usa el campo `Sandwich.Specials` para almacenar datos sobre artículos gratuitos.</span><span class="sxs-lookup"><span data-stu-id="4c604-109">A sandwich bot that is designed to add a free drink or cookie to any order that specifies a foot-long sandwich uses the `Sandwich.Specials` field to store data about free items.</span></span> <span data-ttu-id="4c604-110">En este caso, el valor del campo `Sandwich.Specials` debe establecerse de forma dinámica para cada pedido en función e si el pedido contiene o no un sándwich grande.</span><span class="sxs-lookup"><span data-stu-id="4c604-110">In this case, the value of the `Sandwich.Specials` field must be dynamically set for each order according to whether or not the order contains a foot-long sandwich.</span></span> 

<span data-ttu-id="4c604-111">El campo `Specials` se especifica como opcional y "Ninguno" se designa como texto para la opción que no indica ninguna preferencia.</span><span class="sxs-lookup"><span data-stu-id="4c604-111">The `Specials` field is specified as optional and "None" is designated as text for the choice that indicates no preference.</span></span>

[!code-csharp[Field definition](../includes/code/dotnet-formflow-formbuilder.cs#fieldDefinition)]

<span data-ttu-id="4c604-112">Este ejemplo de código muestra cómo establecer el valor del campo `Specials` de forma dinámica.</span><span class="sxs-lookup"><span data-stu-id="4c604-112">This code example shows how to dynamically set the value of the `Specials` field.</span></span> 

[!code-csharp[Define value](../includes/code/dotnet-formflow-formbuilder.cs#defineValue)]

<span data-ttu-id="4c604-113">En este ejemplo, el método [Advanced.Field.SetType][setType] especifica el tipo de campo (`null` representa un campo de enumeración).</span><span class="sxs-lookup"><span data-stu-id="4c604-113">In this example, the [Advanced.Field.SetType][setType] method specifies the field type (`null` represents an enumeration field).</span></span> <span data-ttu-id="4c604-114">El método [Advanced.Field.SetActive][setActive] especifica que el campo solo debe habilitarse si la longitud del sándwich es `Length.FootLong`.</span><span class="sxs-lookup"><span data-stu-id="4c604-114">The [Advanced.Field.SetActive][setActive] method specifies that the field should only be enabled if the length of the sandwich is `Length.FootLong`.</span></span> <span data-ttu-id="4c604-115">Por último, el método [Advanced.Field.SetDefine][setDefine] especifica un delegado asincrónico que define el campo.</span><span class="sxs-lookup"><span data-stu-id="4c604-115">Finally, the [Advanced.Field.SetDefine][setDefine] method specifies an async delegate that defines the field.</span></span> <span data-ttu-id="4c604-116">Al delegado se le pasa el objeto de estado actual y el valor [Advanced.Field][field] definido de forma dinámica.</span><span class="sxs-lookup"><span data-stu-id="4c604-116">The delegate is passed the current state object and the [Advanced.Field][field] that is being dynamically defined.</span></span> <span data-ttu-id="4c604-117">El delegado usa los métodos fluidos del campo para definir los valores de forma dinámica.</span><span class="sxs-lookup"><span data-stu-id="4c604-117">The delegate uses the field's fluent methods to dynamically define values.</span></span> <span data-ttu-id="4c604-118">En este ejemplo, los valores son cadenas y los métodos `AddDescription` y `AddTerms` especifican las descripciones y los términos de cada valor.</span><span class="sxs-lookup"><span data-stu-id="4c604-118">In this example, the values are strings and the `AddDescription` and `AddTerms` methods specify the descriptions and terms for each value.</span></span>

> [!NOTE]
> <span data-ttu-id="4c604-119">Para definir un valor de campo de forma dinámica, puede implementar [Advanced.IField][iField] por sí mismo o agilizar el proceso con el uso de la clase [Advanced.FieldReflector][FieldReflector] como se muestra en el ejemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="4c604-119">To dynamically define a field value, you can implement [Advanced.IField][iField] yourself, or streamline the process by using the [Advanced.FieldReflector][FieldReflector] class as shown in the example above.</span></span> 

### <a name="dynamically-define-messages-and-confirmations"></a><span data-ttu-id="4c604-120">Definición dinámica de mensajes y confirmaciones</span><span class="sxs-lookup"><span data-stu-id="4c604-120">Dynamically define messages and confirmations</span></span>

<span data-ttu-id="4c604-121">Con FormBuilder, también puede definir de forma dinámica confirmaciones y mensajes.</span><span class="sxs-lookup"><span data-stu-id="4c604-121">Using FormBuilder, you can also dynamically define messages and confirmations.</span></span> <span data-ttu-id="4c604-122">Cada mensaje y confirmación se ejecuta solo cuando los pasos anteriores del formulario están inactivos o completados.</span><span class="sxs-lookup"><span data-stu-id="4c604-122">Each message and confirmation runs only when prior steps in the form are inactive or completed.</span></span> 

<span data-ttu-id="4c604-123">Este ejemplo de código muestra una confirmación generada de forma dinámica que calcula el costo del sándwich.</span><span class="sxs-lookup"><span data-stu-id="4c604-123">This code example shows a dynamically generated confirmation that computes the cost of the sandwich.</span></span> 

[!code-csharp[Define confirmation](../includes/code/dotnet-formflow-formbuilder.cs#defineConfirmation)]

## <a name="customize-a-form-using-formbuilder"></a><span data-ttu-id="4c604-124">Personalización de un formulario mediante FormBuilder</span><span class="sxs-lookup"><span data-stu-id="4c604-124">Customize a form using FormBuilder</span></span>

<span data-ttu-id="4c604-125">Este ejemplo de código usa FormBuilder para definir los pasos del formulario, [validar las selecciones](bot-builder-dotnet-formflow-advanced.md#add-business-logic) y [definir de forma dinámica un valor de campo y una confirmación](#dynamically-define-field-values-confirmations-and-messages).</span><span class="sxs-lookup"><span data-stu-id="4c604-125">This code example uses FormBuilder to define the steps of the form, [validate selections](bot-builder-dotnet-formflow-advanced.md#add-business-logic), and [dynamically define a field value and confirmation](#dynamically-define-field-values-confirmations-and-messages).</span></span> <span data-ttu-id="4c604-126">De forma predeterminada, los pasos del formulario se ejecutarán en la secuencia en que están enumerados.</span><span class="sxs-lookup"><span data-stu-id="4c604-126">By default, steps in the form will be executed in the sequence in which they are listed.</span></span> <span data-ttu-id="4c604-127">Sin embargo, se pueden omitir los pasos para los campos que ya contienen valores o si se especifica la navegación explícita.</span><span class="sxs-lookup"><span data-stu-id="4c604-127">However, steps might be skipped for fields that already contain values or if explicit navigation is specified.</span></span> 

[!code-csharp[FormBuilder form](../includes/code/dotnet-formflow-formbuilder.cs#formBuilderForm)]

<span data-ttu-id="4c604-128">En este ejemplo, el formulario ejecuta estos pasos:</span><span class="sxs-lookup"><span data-stu-id="4c604-128">In this example, the form executes these steps:</span></span>

- <span data-ttu-id="4c604-129">Muestra un mensaje de bienvenida.</span><span class="sxs-lookup"><span data-stu-id="4c604-129">Shows a welcome message.</span></span> 
- <span data-ttu-id="4c604-130">Rellena `SandwichOrder.Sandwich`.</span><span class="sxs-lookup"><span data-stu-id="4c604-130">Fills in `SandwichOrder.Sandwich`.</span></span> 
- <span data-ttu-id="4c604-131">Rellena `SandwichOrder.Length`.</span><span class="sxs-lookup"><span data-stu-id="4c604-131">Fills in `SandwichOrder.Length`.</span></span> 
- <span data-ttu-id="4c604-132">Rellena `SandwichOrder.Bread`.</span><span class="sxs-lookup"><span data-stu-id="4c604-132">Fills in `SandwichOrder.Bread`.</span></span> 
- <span data-ttu-id="4c604-133">Rellena `SandwichOrder.Cheese`.</span><span class="sxs-lookup"><span data-stu-id="4c604-133">Fills in `SandwichOrder.Cheese`.</span></span> 
- <span data-ttu-id="4c604-134">Rellena `SandwichOrder.Toppings` y agrega los valores que faltan si el usuario seleccionó `ToppingOptions.Everything`.</span><span class="sxs-lookup"><span data-stu-id="4c604-134">Fills in `SandwichOrder.Toppings` and adds missing values if the user selected `ToppingOptions.Everything`.</span></span> <span data-ttu-id="4c604-135">-.</span><span class="sxs-lookup"><span data-stu-id="4c604-135">-.</span></span> <span data-ttu-id="4c604-136">Muestra un mensaje que confirma los ingredientes seleccionados.</span><span class="sxs-lookup"><span data-stu-id="4c604-136">Shows a message that confirms the selected toppings.</span></span> 
- <span data-ttu-id="4c604-137">Rellena `SandwichOrder.Sauces`.</span><span class="sxs-lookup"><span data-stu-id="4c604-137">Fills in `SandwichOrder.Sauces`.</span></span> 
- <span data-ttu-id="4c604-138">[Define de forma dinámica](#dynamically-define-field-values) el valor del campo para `SandwichOrder.Specials`.</span><span class="sxs-lookup"><span data-stu-id="4c604-138">[Dynamically defines](#dynamically-define-field-values) the field value for `SandwichOrder.Specials`.</span></span> 
- <span data-ttu-id="4c604-139">[Define de forma dinámica](#dynamically-define-messages-and-confirmations) la confirmación del costo del sándwich.</span><span class="sxs-lookup"><span data-stu-id="4c604-139">[Dynamically defines](#dynamically-define-messages-and-confirmations) the confirmation for cost of the sandwich.</span></span> 
- <span data-ttu-id="4c604-140">Rellena `SandwichOrder.DeliveryAddress` y [verifica](bot-builder-dotnet-formflow-advanced.md#add-business-logic) la cadena resultante.</span><span class="sxs-lookup"><span data-stu-id="4c604-140">Fills in `SandwichOrder.DeliveryAddress` and [verifies](bot-builder-dotnet-formflow-advanced.md#add-business-logic) the resulting string.</span></span> <span data-ttu-id="4c604-141">Si la dirección no empieza con un número, el formulario devuelve un mensaje.</span><span class="sxs-lookup"><span data-stu-id="4c604-141">If the address does not start with a number, the form returns a message.</span></span> 
- <span data-ttu-id="4c604-142">Rellena `SandwichOrder.DeliveryTime` con un mensaje personalizado.</span><span class="sxs-lookup"><span data-stu-id="4c604-142">Fills in `SandwichOrder.DeliveryTime` with a custom prompt.</span></span> 
- <span data-ttu-id="4c604-143">Confirma el pedido.</span><span class="sxs-lookup"><span data-stu-id="4c604-143">Confirms the order.</span></span> 
- <span data-ttu-id="4c604-144">Agrega los campos restantes definidos en la clase pero a los que `Field` no hace referencia de forma explícita.</span><span class="sxs-lookup"><span data-stu-id="4c604-144">Adds any remaining fields that were defined in the class but not explicitly referenced by `Field`.</span></span> <span data-ttu-id="4c604-145">(Si el ejemplo no llamó al método `AddRemainingFields`, el formulario no incluiría ningún campo al que no se hiciera referencia de forma explícita).</span><span class="sxs-lookup"><span data-stu-id="4c604-145">(If the example did not call the `AddRemainingFields` method, the form would not include any fields that were not explicity referenced.)</span></span> 
- <span data-ttu-id="4c604-146">Muestra un mensaje de agradecimiento.</span><span class="sxs-lookup"><span data-stu-id="4c604-146">Shows a thank you message.</span></span> 
- <span data-ttu-id="4c604-147">Define un controlador `OnCompletionAsync` para procesar el pedido.</span><span class="sxs-lookup"><span data-stu-id="4c604-147">Defines an `OnCompletionAsync` handler to process the order.</span></span> 

## <a name="sample-code"></a><span data-ttu-id="4c604-148">Código de ejemplo</span><span class="sxs-lookup"><span data-stu-id="4c604-148">Sample code</span></span>

[!INCLUDE [Sample code](../includes/snippet-dotnet-formflow-samples.md)]

## <a name="additional-resources"></a><span data-ttu-id="4c604-149">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="4c604-149">Additional resources</span></span>

- [<span data-ttu-id="4c604-150">Características básicas de FormFlow</span><span class="sxs-lookup"><span data-stu-id="4c604-150">Basic features of FormFlow</span></span>](bot-builder-dotnet-formflow.md)
- [<span data-ttu-id="4c604-151">Características avanzadas de FormFlow</span><span class="sxs-lookup"><span data-stu-id="4c604-151">Advanced features of FormFlow</span></span>](bot-builder-dotnet-formflow-advanced.md)
- [<span data-ttu-id="4c604-152">Localización del contenido del formulario</span><span class="sxs-lookup"><span data-stu-id="4c604-152">Localize form content</span></span>](bot-builder-dotnet-formflow-localize.md)
- [<span data-ttu-id="4c604-153">Definición de un formulario mediante esquemas JSON</span><span class="sxs-lookup"><span data-stu-id="4c604-153">Define a form using JSON schema</span></span>](bot-builder-dotnet-formflow-json-schema.md)
- [<span data-ttu-id="4c604-154">Personalización de la experiencia de usuario con lenguaje de patrones</span><span class="sxs-lookup"><span data-stu-id="4c604-154">Customize user experience with pattern language</span></span>](bot-builder-dotnet-formflow-pattern-language.md)
- <span data-ttu-id="4c604-155"><a href="/dotnet/api/?view=botbuilder-3.11.0" target="_blank">Referencia de Bot Builder SDK para .NET</a></span><span class="sxs-lookup"><span data-stu-id="4c604-155"><a href="/dotnet/api/?view=botbuilder-3.11.0" target="_blank">Bot Builder SDK for .NET Reference</a></span></span>

[formBuilder]: /dotnet/api/microsoft.bot.builder.formflow.formbuilder-1

[setType]: /dotnet/api/microsoft.bot.builder.formflow.advanced.field-1.settype

[setActive]: /dotnet/api/microsoft.bot.builder.formflow.advanced.field-1.setactive

[setDefine]: /dotnet/api/microsoft.bot.builder.formflow.advanced.field-1.setdefine

[field]: /dotnet/api/microsoft.bot.builder.formflow.advanced.field-1

[iField]: /dotnet/api/microsoft.bot.builder.formflow.advanced.ifield-1

[FieldReflector]: /dotnet/api/microsoft.bot.builder.formflow.advanced.fieldreflector-1
