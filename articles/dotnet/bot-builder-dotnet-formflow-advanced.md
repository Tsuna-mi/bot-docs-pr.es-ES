---
title: Características avanzadas de FormFlow | Microsoft Docs
description: Obtenga información sobre cómo personalizar la experiencia del usuario mediante FormFlow y el SDK de Bot Builder para .NET.
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: 6637876b016b8680fe722602f530a0c6b0ddfc5a
ms.sourcegitcommit: 2dc75701b169d822c9499e393439161bc87639d2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42905409"
---
# <a name="advanced-features-of-formflow"></a><span data-ttu-id="ba492-103">Características avanzadas de FormFlow</span><span class="sxs-lookup"><span data-stu-id="ba492-103">Advanced features of FormFlow</span></span>

[!INCLUDE [pre-release-label](../includes/pre-release-label-v3.md)]

<span data-ttu-id="ba492-104">[Basic features of FormFlow](bot-builder-dotnet-formflow.md) (Características básicas de FromFlow) describe una implementación básica de FormFlow que ofrece una experiencia de usuario bastante genérica.</span><span class="sxs-lookup"><span data-stu-id="ba492-104">[Basic features of FormFlow](bot-builder-dotnet-formflow.md) describes a basic FormFlow implementation that delivers a fairly generic user experience.</span></span> <span data-ttu-id="ba492-105">Para ofrecer una experiencia de usuario más personalizada mediante FormFlow, puede especificar el estado inicial del formulario, agregar lógica de negocios para administrar las interdependencias entre los campos y procesar los datos del usuario, y usar atributos para personalizar mensajes, reemplazar plantillas, designar campos opcionales, establecer coincidencias con la entrada del usuario y validar dicha entrada.</span><span class="sxs-lookup"><span data-stu-id="ba492-105">To deliver a more customized user experience using FormFlow, you can specify initial form state, add business logic to manage interdependencies between fields and process user input, and use attributes to customize prompts, override templates, designate optional fields, match user input, and validate user input.</span></span> 

## <a name="specify-initial-form-state-and-entities"></a><span data-ttu-id="ba492-106">Especificación de las entidades y del estado inicial del formulario</span><span class="sxs-lookup"><span data-stu-id="ba492-106">Specify initial form state and entities</span></span>

<span data-ttu-id="ba492-107">Cuando inicie [FormDialog][formDialog], también puede pasar una instancia de su estado de forma opcional.</span><span class="sxs-lookup"><span data-stu-id="ba492-107">When you launch a [FormDialog][formDialog], you may optionally pass in an instance of your state.</span></span> <span data-ttu-id="ba492-108">Si pasa una instancia del estado, FormFlow omitirá de forma predeterminada los pasos para todos los campos que ya contienen valores. No se pedirá al usuario que rellene esos campos.</span><span class="sxs-lookup"><span data-stu-id="ba492-108">If you do pass in an instance of your state, then by default, FormFlow will skip steps for any fields that already contain values; the user will not be prompted for those fields.</span></span> <span data-ttu-id="ba492-109">Para forzar que el formulario solicite al usuario todos los campos (incluso los que ya contienen valores en el estado inicial), pase [FormOptions.PromptFieldsWithValues][promptFieldsWithValues] cuando inicie `FormDialog`.</span><span class="sxs-lookup"><span data-stu-id="ba492-109">To force the form to prompt the user for all fields (including those fields that already contain values in the initial state), pass in [FormOptions.PromptFieldsWithValues][promptFieldsWithValues] when you launch the `FormDialog`.</span></span> <span data-ttu-id="ba492-110">Si un campo contiene un valor inicial, el mensaje lo usará como valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="ba492-110">If a field contains an initial value, the prompt will use that value as the default value.</span></span>

<span data-ttu-id="ba492-111">También puede pasar entidades de [LUIS](https://luis.ai/) para enlazarlas con el estado.</span><span class="sxs-lookup"><span data-stu-id="ba492-111">You can also pass in [LUIS](https://luis.ai/) entities to bind to the state.</span></span> <span data-ttu-id="ba492-112">Si `EntityRecommendation.Type` es una ruta de acceso a un campo de la clase de C#, `EntityRecommendation.Entity` se pasará por el reconocedor para enlazarla con el campo.</span><span class="sxs-lookup"><span data-stu-id="ba492-112">If the `EntityRecommendation.Type` is a path to a field in your C# class, the `EntityRecommendation.Entity` will be passed through the recognizer to bind to your field.</span></span> <span data-ttu-id="ba492-113">FormFlow omitirá los pasos para todos los campos que estén enlazados a una entidad; no se pedirá al usuario que rellene esos campos.</span><span class="sxs-lookup"><span data-stu-id="ba492-113">FormFlow will skip steps for any fields that are bound to an entity; the user will not be prompted for those fields.</span></span> 

## <a name="add-business-logic"></a><span data-ttu-id="ba492-114">Adición de lógica de negocios</span><span class="sxs-lookup"><span data-stu-id="ba492-114">Add business logic</span></span> 

<span data-ttu-id="ba492-115">Puede especificar la lógica de negocios dentro de una función de validación para manipular las interdependencias entre los campos de formulario o aplicar una lógica específica durante el proceso de obtener o establecer el valor de un campo.</span><span class="sxs-lookup"><span data-stu-id="ba492-115">To handle interdependencies between form fields or apply specific logic during the process of getting or setting a field value, you can specify business logic within a validation function.</span></span> <span data-ttu-id="ba492-116">Una función de validación le permite manipular estados y devolver un objeto [ValidateResult][validateResult] que puede contener:</span><span class="sxs-lookup"><span data-stu-id="ba492-116">A validation function lets you manipulate the state and return a [ValidateResult][validateResult] object that can contain:</span></span> 

- <span data-ttu-id="ba492-117">una cadena de comentarios que describe el motivo por el que un valor no es válido</span><span class="sxs-lookup"><span data-stu-id="ba492-117">a feedback string that describes the reason that a value is invalid</span></span>
- <span data-ttu-id="ba492-118">un valor transformado</span><span class="sxs-lookup"><span data-stu-id="ba492-118">a transformed value</span></span>
- <span data-ttu-id="ba492-119">un conjunto de opciones para aclarar un valor</span><span class="sxs-lookup"><span data-stu-id="ba492-119">a set of choices for clarifying a value</span></span>

<span data-ttu-id="ba492-120">Este ejemplo de código muestra una función de validación para el campo `Toppings`.</span><span class="sxs-lookup"><span data-stu-id="ba492-120">This code example shows a validation function for the `Toppings` field.</span></span> <span data-ttu-id="ba492-121">Si la entrada para el campo contiene el valor de enumeración `ToppingOptions.Everything`, la función garantiza que el valor del campo `Toppings` contiene la lista completa de los ingredientes.</span><span class="sxs-lookup"><span data-stu-id="ba492-121">If input for the field contains the `ToppingOptions.Everything` enumeration value, the function ensures that the `Toppings` field value contains the full list of toppings.</span></span>

[!code-csharp[Validation function](../includes/code/dotnet-formflow-advanced.cs#validationFunction)]

<span data-ttu-id="ba492-122">Además de la función de validación, puede agregar el atributo [Term](#match-user-input-using-the-terms-attribute) para que coincida con las expresiones de usuario como “todo” o “no”.</span><span class="sxs-lookup"><span data-stu-id="ba492-122">In addition to the validation function, you can add the [Term](#match-user-input-using-the-terms-attribute) attribute to match user expressions such as "everything" or "not".</span></span>

[!code-csharp[Terms for Toppings](../includes/code/dotnet-formflow-advanced.cs#toppingsTerms)]

<span data-ttu-id="ba492-123">Con la función de validación mostrada anteriormente, este fragmento de código muestra la interacción entre el bot y el usuario cuando el usuario solicita “todo excepto Jalapeños”.</span><span class="sxs-lookup"><span data-stu-id="ba492-123">Using the validation function shown above, this snippet shows the interaction between bot and user when the user requests "everything but Jalapenos."</span></span> 

```console
Please select one or more toppings (current choice: No Preference)
 1. Everything
 2. Avocado
 3. Banana Peppers
 4. Cucumbers
 5. Green Bell Peppers
 6. Jalapenos
 7. Lettuce
 8. Olives
 9. Pickles
 10. Red Onion
 11. Spinach
 12. Tomatoes
> everything but jalapenos
For sandwich toppings you have selected Avocado, Banana Peppers, Cucumbers, Green Bell Peppers, Lettuce, Olives, Pickles, Red Onion, Spinach, and Tomatoes.
```

## <a name="formflow-attributes"></a><span data-ttu-id="ba492-124">Atributos de FormFlow</span><span class="sxs-lookup"><span data-stu-id="ba492-124">FormFlow attributes</span></span>

<span data-ttu-id="ba492-125">Puede agregar estos atributos de C# a la clase para personalizar el comportamiento de un diálogo de FormFlow.</span><span class="sxs-lookup"><span data-stu-id="ba492-125">You can add these C# attributes to your class to customize behavior of a FormFlow dialog.</span></span>

| <span data-ttu-id="ba492-126">Atributo</span><span class="sxs-lookup"><span data-stu-id="ba492-126">Attribute</span></span> | <span data-ttu-id="ba492-127">Propósito</span><span class="sxs-lookup"><span data-stu-id="ba492-127">Purpose</span></span> |
|----|----| 
| <span data-ttu-id="ba492-128">[Describe][describeAttribute]</span><span class="sxs-lookup"><span data-stu-id="ba492-128">[Describe][describeAttribute]</span></span> | <span data-ttu-id="ba492-129">Modifica cómo se muestra un campo o un valor en una plantilla o tarjeta.</span><span class="sxs-lookup"><span data-stu-id="ba492-129">Alter how a field or a value is shown in a template or card</span></span> |
| <span data-ttu-id="ba492-130">[Numeric][numericAttribute]</span><span class="sxs-lookup"><span data-stu-id="ba492-130">[Numeric][numericAttribute]</span></span> | <span data-ttu-id="ba492-131">Restringe los valores aceptados de un campo numérico.</span><span class="sxs-lookup"><span data-stu-id="ba492-131">Restrict the accepted values of a numeric field</span></span> |
| <span data-ttu-id="ba492-132">[Optional][optionalAttribute]</span><span class="sxs-lookup"><span data-stu-id="ba492-132">[Optional][optionalAttribute]</span></span> | <span data-ttu-id="ba492-133">Marca un campo como opcional.</span><span class="sxs-lookup"><span data-stu-id="ba492-133">Mark a field as optional</span></span> |
| <span data-ttu-id="ba492-134">[Pattern][patternAttribute]</span><span class="sxs-lookup"><span data-stu-id="ba492-134">[Pattern][patternAttribute]</span></span> | <span data-ttu-id="ba492-135">Define una expresión regular para validar un campo de cadena.</span><span class="sxs-lookup"><span data-stu-id="ba492-135">Define a regular expression to validate a string field</span></span> |
| <span data-ttu-id="ba492-136">[Prompt][promptAttribute]</span><span class="sxs-lookup"><span data-stu-id="ba492-136">[Prompt][promptAttribute]</span></span> | <span data-ttu-id="ba492-137">Define el mensaje para un campo.</span><span class="sxs-lookup"><span data-stu-id="ba492-137">Define the prompt for a field</span></span> |
| <span data-ttu-id="ba492-138">[Template][templateAttribute]</span><span class="sxs-lookup"><span data-stu-id="ba492-138">[Template][templateAttribute]</span></span> | <span data-ttu-id="ba492-139">Define la plantilla utilizada para generar mensajes o valores en los mensajes.</span><span class="sxs-lookup"><span data-stu-id="ba492-139">Define the template to use to generate prompts or values in prompts</span></span> |
| <span data-ttu-id="ba492-140">[Terms][termsAttribute]</span><span class="sxs-lookup"><span data-stu-id="ba492-140">[Terms][termsAttribute]</span></span> | <span data-ttu-id="ba492-141">Define los términos de entrada que coinciden con un campo o valor.</span><span class="sxs-lookup"><span data-stu-id="ba492-141">Define the input terms that match a field or value</span></span> |

## <a name="customize-prompts-using-the-prompt-attribute"></a><span data-ttu-id="ba492-142">Personalización de mensajes con el atributo Prompt</span><span class="sxs-lookup"><span data-stu-id="ba492-142">Customize prompts using the Prompt attribute</span></span>

<span data-ttu-id="ba492-143">Los mensajes predeterminados se generan automáticamente para cada campo del formulario, pero puede especificar un mensaje personalizado para cualquier campo con el atributo `Prompt`.</span><span class="sxs-lookup"><span data-stu-id="ba492-143">Default prompts are automatically generated for each field in your form, but you can specify a custom prompt for any field by using the `Prompt` attribute.</span></span> <span data-ttu-id="ba492-144">Por ejemplo, si el mensaje predeterminado para el campo `SandwichOrder.Sandwich` es “Seleccione un sándwich”, puede agregar el atributo `Prompt` para especificar un mensaje personalizado para el campo.</span><span class="sxs-lookup"><span data-stu-id="ba492-144">For example, if the default prompt for the `SandwichOrder.Sandwich` field is "Please select a sandwich", you can add the `Prompt` attribute to specify a custom prompt for that field.</span></span>

[!code-csharp[Prompt attribute](../includes/code/dotnet-formflow-advanced.cs#promptAttribute)]

<span data-ttu-id="ba492-145">Este ejemplo utiliza [lenguaje de patrones](bot-builder-dotnet-formflow-pattern-language.md) para rellenar dinámicamente el mensaje con los datos del formulario durante el tiempo de ejecución: `{&}` se reemplaza por la descripción del campo y `{||}` se reemplaza por la lista de opciones en la enumeración.</span><span class="sxs-lookup"><span data-stu-id="ba492-145">This example uses [pattern language](bot-builder-dotnet-formflow-pattern-language.md) to dynamically populate the prompt with form data at runtime: `{&}` is replaced with the description of the field and `{||}` is replaced with the list of choices in the enumeration.</span></span> 

> [!NOTE]
> <span data-ttu-id="ba492-146">De forma predeterminada, la descripción de un campo se genera a partir del nombre del campo.</span><span class="sxs-lookup"><span data-stu-id="ba492-146">By default, the description of a field is generated from the field's name.</span></span> <span data-ttu-id="ba492-147">Para especificar una descripción personalizada para un campo, agregue el atributo `Describe`.</span><span class="sxs-lookup"><span data-stu-id="ba492-147">To specify a custom description for a field, add the `Describe` attribute.</span></span>

<span data-ttu-id="ba492-148">En este fragmento de código se muestra el mensaje personalizado que se ha especificado en el ejemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="ba492-148">This snippet shows the customized prompt that is specified by the example above.</span></span>

```console
What kind of sandwich would you like?
1. BLT
2. Black Forest Ham
3. Buffalo Chicken
4. Chicken And Bacon Ranch Melt
5. Cold Cut Combo
6. Meatball Marinara
7. Oven Roasted Chicken
8. Roast Beef
9. Rotisserie Style Chicken
10. Spicy Italian
11. Steak And Cheese
12. Sweet Onion Teriyaki
13. Tuna
14. Turkey Breast
15. Veggie
>
```

<span data-ttu-id="ba492-149">Un atributo `Prompt` también puede especificar los parámetros que afectan a cómo el formulario muestra el mensaje.</span><span class="sxs-lookup"><span data-stu-id="ba492-149">A `Prompt` attribute may also specify parameters that affect how the form displays the prompt.</span></span> <span data-ttu-id="ba492-150">Por ejemplo, el parámetro `ChoiceFormat` determina cómo el formulario representa la lista de opciones.</span><span class="sxs-lookup"><span data-stu-id="ba492-150">For example, the `ChoiceFormat` parameter determines how the form renders the list of choices.</span></span>

[!code-csharp[Prompt attribute ChoiceFormat parameter](../includes/code/dotnet-formflow-advanced.cs#promptChoice)]

<span data-ttu-id="ba492-151">En este ejemplo, el valor del parámetro `ChoiceFormat` indica que se deben mostrar las opciones como una lista con viñetas (en lugar de una lista numerada).</span><span class="sxs-lookup"><span data-stu-id="ba492-151">In this example, the value of the `ChoiceFormat` parameter indicates that the choices should be displayed as a bulleted list (instead of a numbered list).</span></span>

```console
What kind of sandwich would you like?
- BLT
- Black Forest Ham
- Buffalo Chicken
- Chicken And Bacon Ranch Melt
- Cold Cut Combo
- Meatball Marinara
- Oven Roasted Chicken
- Roast Beef
- Rotisserie Style Chicken
- Spicy Italian
- Steak And Cheese
- Sweet Onion Teriyaki
- Tuna
- Turkey Breast
- Veggie
>
```

## <a name="customize-prompts-using-the-template-attribute"></a><span data-ttu-id="ba492-152">Personalización de mensajes con el atributo Template</span><span class="sxs-lookup"><span data-stu-id="ba492-152">Customize prompts using the Template attribute</span></span>

<span data-ttu-id="ba492-153">Mientras el atributo `Prompt` le permite personalizar el mensaje para un solo campo, el atributo `Template` le permite reemplazar las plantillas predeterminadas que usa FormFlow para generar automáticamente los mensajes.</span><span class="sxs-lookup"><span data-stu-id="ba492-153">While the `Prompt` attribute enables you to customize the prompt for a single field, the `Template` attribute enables you to replace the default templates that FormFlow uses to automatically generate prompts.</span></span> <span data-ttu-id="ba492-154">Este ejemplo de código utiliza el atributo `Template` para redefinir la forma en que el formulario manipula todos los campos de enumeración.</span><span class="sxs-lookup"><span data-stu-id="ba492-154">This code example uses the `Template` attribute to redefine how the form handles all enumeration fields.</span></span> <span data-ttu-id="ba492-155">El atributo indica que el usuario puede seleccionar solo un elemento, establece el texto del mensaje mediante el [lenguaje de patrones](bot-builder-dotnet-formflow-pattern-language.md) y especifica que el formulario debe mostrar solo un elemento por línea.</span><span class="sxs-lookup"><span data-stu-id="ba492-155">The attribute indicates that the user may select only one item, sets the prompt text by using [pattern language](bot-builder-dotnet-formflow-pattern-language.md), and specifies that the form should display only one item per line.</span></span> 

[!code-csharp[Template attribute](../includes/code/dotnet-formflow-advanced.cs#templateAttribute)]

<span data-ttu-id="ba492-156">En este fragmento de código se muestran los mensajes resultantes para los campos `Bread` y `Cheese`.</span><span class="sxs-lookup"><span data-stu-id="ba492-156">This snippet shows the resulting prompts for the `Bread` field and `Cheese` field.</span></span>

```console
What kind of bread would you like on your sandwich?
 1. Nine Grain Wheat
 2. Nine Grain Honey Oat
 3. Italian
 4. Italian Herbs And Cheese
 5. Flatbread
> 

What kind of cheese would you like on your sandwich? 
 1. American
 2. Monterey Cheddar
 3. Pepperjack
> 
```

<span data-ttu-id="ba492-157">Si usa el atributo `Template` para reemplazar las plantillas predeterminadas que usa FormFlow en la generación de mensajes, puede interponer alguna variación en los mensajes y los avisos generados por el formulario.</span><span class="sxs-lookup"><span data-stu-id="ba492-157">If you use the `Template` attribute to replace the default templates that FormFlow uses to generate prompts, you may want to interject some variation into the prompts and messages that the form generates.</span></span> <span data-ttu-id="ba492-158">Para ello, puede definir varias cadenas de texto mediante el [lenguaje de patrones](bot-builder-dotnet-formflow-pattern-language.md) y el formulario elegirá aleatoriamente entre las opciones disponibles cada vez que necesite mostrar un aviso o un mensaje.</span><span class="sxs-lookup"><span data-stu-id="ba492-158">To do so, you can define multiple text strings using [pattern language](bot-builder-dotnet-formflow-pattern-language.md), and the form will randomly choose from the available options each time it needs to display a prompt or message.</span></span>

<span data-ttu-id="ba492-159">Este ejemplo de código redefine la plantilla [TemplateUsage.NotUnderstood][notUnderstood] para especificar dos variaciones del mensaje.</span><span class="sxs-lookup"><span data-stu-id="ba492-159">This code example redefines the [TemplateUsage.NotUnderstood][notUnderstood] template to specify two different variations of message.</span></span> <span data-ttu-id="ba492-160">Cuando el bot necesite comunicar que no comprende la entrada del usuario, determinará el contenido del mensaje seleccionando aleatoriamente una de las dos cadenas de texto.</span><span class="sxs-lookup"><span data-stu-id="ba492-160">When the bot needs to communicate that it does not understand a user's input, it will determine message contents by randomly selecting one of the two text strings.</span></span> 

[!code-csharp[Template variations of message](../includes/code/dotnet-formflow-advanced.cs#templateMessages)]

<span data-ttu-id="ba492-161">En este fragmento de código se muestra un ejemplo de la interacción resultante entre el usuario y el bot.</span><span class="sxs-lookup"><span data-stu-id="ba492-161">This snippet shows an example of the resulting the interaction between bot and user.</span></span> 

```console
What size of sandwich do you want? (1. Six Inch, 2. Foot Long)
> two feet
I do not understand "two feet".
> two feet
Try again, I don't get "two feet"
> 
```

## <a name="designate-a-field-as-optional-using-the-optional-attribute"></a><span data-ttu-id="ba492-162">Designación de un campo como opcional con el atributo Optional</span><span class="sxs-lookup"><span data-stu-id="ba492-162">Designate a field as optional using the Optional attribute</span></span>

<span data-ttu-id="ba492-163">Para designar un campo como opcional, use el atributo `Optional`.</span><span class="sxs-lookup"><span data-stu-id="ba492-163">To designate a field as optional, use the `Optional` attribute.</span></span> <span data-ttu-id="ba492-164">Este ejemplo de código especifica que el campo `Cheese` es opcional.</span><span class="sxs-lookup"><span data-stu-id="ba492-164">This code example specifies that the `Cheese` field is optional.</span></span>

[!code-csharp[Optional attribute](../includes/code/dotnet-formflow-advanced.cs#optionalAttribute)]

<span data-ttu-id="ba492-165">Si un campo es opcional y no se ha especificado ningún valor, se mostrará la opción actual como “Sin preferencias”.</span><span class="sxs-lookup"><span data-stu-id="ba492-165">If a field is optional and no value has been specified, the current choice will be displayed as "No Preference".</span></span>

```console
What kind of cheese would you like on your sandwich? (current choice: No Preference)
  1. American
  2. Monterey Cheddar
  3. Pepperjack
 >
```

<span data-ttu-id="ba492-166">Si un campo es opcional y el usuario ha especificado un valor, se mostrará “Sin preferencias” como la última opción en la lista.</span><span class="sxs-lookup"><span data-stu-id="ba492-166">If a field is optional and the user has specified a value, "No Preference" will be displayed as the last choice in the list.</span></span>

```console
What kind of cheese would you like on your sandwich? (current choice: American)
 1. American
 2. Monterey Cheddar
 3. Pepperjack
 4. No Preference
>
```

## <a name="match-user-input-using-the-terms-attribute"></a><span data-ttu-id="ba492-167">Coincidencias con la entrada del usuario mediante el atributo Terms</span><span class="sxs-lookup"><span data-stu-id="ba492-167">Match user input using the Terms attribute</span></span>

<span data-ttu-id="ba492-168">Cuando un usuario envía un mensaje a un bot creado mediante FormFlow, el bot intenta identificar el significado de la entrada del usuario estableciendo una coincidencia de la entrada con una lista de términos.</span><span class="sxs-lookup"><span data-stu-id="ba492-168">When a user sends a message to a bot that is built using FormFlow, the bot attempts to identify the meaning of the user's input by matching the input to a list of terms.</span></span> <span data-ttu-id="ba492-169">De forma predeterminada, la lista de términos se genera al realizar los siguientes pasos para el campo o valor:</span><span class="sxs-lookup"><span data-stu-id="ba492-169">By default, the list of terms is generated by applying these steps to the field or value:</span></span> 

1. <span data-ttu-id="ba492-170">Insertar un salto en cambios de mayúsculas y guiones bajos (_).</span><span class="sxs-lookup"><span data-stu-id="ba492-170">Break on case changes and underscore (_).</span></span>
2. <span data-ttu-id="ba492-171">Generar cada <a href="https://en.wikipedia.org/wiki/N-gram" target="_blank">n-grama</a> hasta una longitud máxima.</span><span class="sxs-lookup"><span data-stu-id="ba492-171">Generate each <a href="https://en.wikipedia.org/wiki/N-gram" target="_blank">n-gram</a> up to a maximum length.</span></span>
3. <span data-ttu-id="ba492-172">Agregar “s?”</span><span class="sxs-lookup"><span data-stu-id="ba492-172">Add "s?"</span></span> <span data-ttu-id="ba492-173">al final de cada palabra (para admitir plurales).</span><span class="sxs-lookup"><span data-stu-id="ba492-173">to the end of each word (to support plurals).</span></span> 

<span data-ttu-id="ba492-174">Por ejemplo, el valor “PizzaDeCarneAngusConAjo” generaría estos términos:</span><span class="sxs-lookup"><span data-stu-id="ba492-174">For example, the value "AngusBeefAndGarlicPizza" would generate these terms:</span></span> 

- <span data-ttu-id="ba492-175">'pizzas?'</span><span class="sxs-lookup"><span data-stu-id="ba492-175">'angus?'</span></span>
- <span data-ttu-id="ba492-176">'carnes?'</span><span class="sxs-lookup"><span data-stu-id="ba492-176">'beefs?'</span></span>
- <span data-ttu-id="ba492-177">'angus?'</span><span class="sxs-lookup"><span data-stu-id="ba492-177">'garlics?'</span></span>
- <span data-ttu-id="ba492-178">'ajos?'</span><span class="sxs-lookup"><span data-stu-id="ba492-178">'pizzas?'</span></span>
- <span data-ttu-id="ba492-179">'carnes? angus?'</span><span class="sxs-lookup"><span data-stu-id="ba492-179">'angus? beefs?'</span></span>
- <span data-ttu-id="ba492-180">'pizzas? ajos?'</span><span class="sxs-lookup"><span data-stu-id="ba492-180">'garlics? pizzas?'</span></span>
- <span data-ttu-id="ba492-181">'pizza de carne angus con ajo'</span><span class="sxs-lookup"><span data-stu-id="ba492-181">'angus beef and garlic pizza'</span></span>

<span data-ttu-id="ba492-182">Para invalidar este comportamiento predeterminado y definir la lista de términos que se usan para hacer coincidencias con la entrada del usuario en un campo o en el valor de un campo, use el atributo `Terms`.</span><span class="sxs-lookup"><span data-stu-id="ba492-182">To override this default behavior and define the list of terms that are used to match user input to a field or a value in a field, use the `Terms` attribute.</span></span> <span data-ttu-id="ba492-183">Por ejemplo, puede usar el atributo `Terms` (con una expresión regular) para considerar que los usuarios suelen escribir mal la palabra “rotisserie” en inglés.</span><span class="sxs-lookup"><span data-stu-id="ba492-183">For example, you may use the `Terms` attribute (with a regular expression) to account for the fact that users are likely to misspell the word "rotisserie."</span></span> 

[!code-csharp[Terms attribute](../includes/code/dotnet-formflow-advanced.cs#termsAttribute)]

<span data-ttu-id="ba492-184">Mediante el uso del atributo `Terms`, aumentarán las posibilidades de que exista una coincidencia entre la entrada del usuario y una de las opciones válidas.</span><span class="sxs-lookup"><span data-stu-id="ba492-184">By using the `Terms` attribute, you increase the likelihood of being able to match user input with one of the valid choices.</span></span> <span data-ttu-id="ba492-185">El parámetro `Terms.MaxPhrase` en este ejemplo hace que `Language.GenerateTerms` genere más variaciones de términos.</span><span class="sxs-lookup"><span data-stu-id="ba492-185">The `Terms.MaxPhrase` parameter in this example causes the `Language.GenerateTerms` to generate additional variations of terms.</span></span> 

<span data-ttu-id="ba492-186">En este fragmento de código se muestra la interacción resultante entre el usuario y el bot cuando el usuario ha escrito incorrectamente “rotisserie”.</span><span class="sxs-lookup"><span data-stu-id="ba492-186">This snippet shows the resulting interaction between bot and user when the user misspells "Rotisserie."</span></span>

```console
What kind of sandwich would you like?
 1. BLT
 2. Black Forest Ham
 3. Buffalo Chicken
 4. Chicken And Bacon Ranch Melt
 5. Cold Cut Combo
 6. Meatball Marinara
 7. Oven Roasted Chicken
 8. Roast Beef
 9. Rotisserie Style Chicken
 10. Spicy Italian
 11. Steak And Cheese
 12. Sweet Onion Teriyaki
 13. Tuna
 14. Turkey Breast
 15. Veggie
> rotissary checkin
For sandwich I understood Rotisserie Style Chicken. "checkin" is not an option.
```

## <a name="validate-user-input-using-the-numeric-attribute-or-pattern-attribute"></a><span data-ttu-id="ba492-187">Validación de la entrada del usuario mediante los atributos Numeric o Pattern</span><span class="sxs-lookup"><span data-stu-id="ba492-187">Validate user input using the Numeric attribute or Pattern attribute</span></span>

<span data-ttu-id="ba492-188">Para restringir el intervalo de valores permitidos en un campo numérico, use el atributo `Numeric`.</span><span class="sxs-lookup"><span data-stu-id="ba492-188">To restrict the range of allowed values for a numeric field, use the `Numeric` attribute.</span></span> <span data-ttu-id="ba492-189">Este ejemplo de código utiliza el atributo `Numeric` para especificar que la entrada para el campo `Rating` debe ser un número entre 1 y 5.</span><span class="sxs-lookup"><span data-stu-id="ba492-189">This code example uses the `Numeric` attribute to specify that input for the `Rating` field must be a number between 1 and 5.</span></span> 

[!code-csharp[Numeric attribute](../includes/code/dotnet-formflow-advanced.cs#numericAttribute)]

<span data-ttu-id="ba492-190">Para especificar el formato que debe seguir el valor de un campo específico, use el atributo `Pattern`.</span><span class="sxs-lookup"><span data-stu-id="ba492-190">To specify the required format for the value of a particular field, use the `Pattern` attribute.</span></span> <span data-ttu-id="ba492-191">Este ejemplo de código usa el atributo `Pattern` para especificar el formato que debe seguir el valor del campo `PhoneNumber`.</span><span class="sxs-lookup"><span data-stu-id="ba492-191">This code example uses the `Pattern` attribute to specify the required format for the value of the `PhoneNumber` field.</span></span>

[!code-csharp[Pattern attribute](../includes/code/dotnet-formflow-advanced.cs#patternAttribute)]

## <a name="summary"></a><span data-ttu-id="ba492-192">Resumen</span><span class="sxs-lookup"><span data-stu-id="ba492-192">Summary</span></span>

<span data-ttu-id="ba492-193">En este artículo se describe cómo ofrecer una experiencia de usuario personalizada con FormFlow al especificar el estado inicial del formulario, agregar lógica de negocios para administrar las interdependencias entre los campos y procesar los datos del usuario, y usar atributos para personalizar mensajes, reemplazar plantillas, designar campos opcionales, establecer coincidencias con la entrada del usuario y validar dicha entrada.</span><span class="sxs-lookup"><span data-stu-id="ba492-193">This article has described how to deliver a customized user experience with FormFlow by specifying initial form state, adding business logic to manage interdependencies between fields and process user input, and using attributes to customize prompts, override templates, designate optional fields, match user input, and validate user input.</span></span> <span data-ttu-id="ba492-194">Para obtener más información sobre otras formas de personalizar la experiencia del usuario con FormFlow, consulte [Customize a form using FormBuilder](bot-builder-dotnet-formflow-formbuilder.md) (Personalización de un formulario mediante FormBuilder).</span><span class="sxs-lookup"><span data-stu-id="ba492-194">For information about additional ways to customize the user experience with FormFlow, see [Customize a form using FormBuilder](bot-builder-dotnet-formflow-formbuilder.md).</span></span>

## <a name="sample-code"></a><span data-ttu-id="ba492-195">Código de ejemplo</span><span class="sxs-lookup"><span data-stu-id="ba492-195">Sample code</span></span>

[!INCLUDE [Sample code](../includes/snippet-dotnet-formflow-samples.md)]

## <a name="additional-resources"></a><span data-ttu-id="ba492-196">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="ba492-196">Additional resources</span></span>

- [<span data-ttu-id="ba492-197">Características básicas de FormFlow</span><span class="sxs-lookup"><span data-stu-id="ba492-197">Basic features of FormFlow</span></span>](bot-builder-dotnet-formflow.md)
- <span data-ttu-id="ba492-198">[Customize a form using FormBuilder](bot-builder-dotnet-formflow-formbuilder.md) (Personalización de un formulario mediante FormBuilder)</span><span class="sxs-lookup"><span data-stu-id="ba492-198">[Customize a form using FormBuilder](bot-builder-dotnet-formflow-formbuilder.md)</span></span>
- [<span data-ttu-id="ba492-199">Localización del contenido del formulario</span><span class="sxs-lookup"><span data-stu-id="ba492-199">Localize form content</span></span>](bot-builder-dotnet-formflow-localize.md)
- [<span data-ttu-id="ba492-200">Definición de un formulario mediante esquemas JSON</span><span class="sxs-lookup"><span data-stu-id="ba492-200">Define a form using JSON schema</span></span>](bot-builder-dotnet-formflow-json-schema.md)
- [<span data-ttu-id="ba492-201">Personalización de la experiencia de usuario con lenguaje de patrones</span><span class="sxs-lookup"><span data-stu-id="ba492-201">Customize user experience with pattern language</span></span>](bot-builder-dotnet-formflow-pattern-language.md)
- <span data-ttu-id="ba492-202"><a href="/dotnet/api/?view=botbuilder-3.11.0" target="_blank">Referencia de Bot Builder SDK para .NET</a></span><span class="sxs-lookup"><span data-stu-id="ba492-202"><a href="/dotnet/api/?view=botbuilder-3.11.0" target="_blank">Bot Builder SDK for .NET Reference</a></span></span>

[formDialog]: /dotnet/api/microsoft.bot.builder.formflow.formdialog

[promptFieldsWithValues]: /dotnet/api/microsoft.bot.builder.formflow.formoptions.promptfieldswithvalues

[validateResult]: /dotnet/api/microsoft.bot.builder.formflow.validateresult

[describeAttribute]: /dotnet/api/microsoft.bot.builder.formflow.describeattribute

[numericAttribute]: /dotnet/api/microsoft.bot.builder.formflow.numericattribute

[optionalAttribute]: /dotnet/api/microsoft.bot.builder.formflow.optionalattribute

[patternAttribute]: /dotnet/api/microsoft.bot.builder.formflow.patternattribute

[promptAttribute]: /dotnet/api/microsoft.bot.builder.formflow.promptattribute

[templateAttribute]: /dotnet/api/microsoft.bot.builder.formflow.templateattribute

[termsAttribute]: /dotnet/api/microsoft.bot.builder.formflow.termsattribute

[notUnderstood]: /dotnet/api/microsoft.bot.builder.formflow.templateusage.notunderstood
