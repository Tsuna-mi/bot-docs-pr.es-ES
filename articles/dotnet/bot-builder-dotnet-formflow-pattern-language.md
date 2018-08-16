---
title: Personalización de la experiencia de usuario con lenguaje de patrones | Microsoft Docs
description: Aprenda a personalizar los avisos FormFlow y a invalidar las plantillas FormFlow mediante un lenguaje de patrones con Bot Builder SDK para .NET.
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: 109353d6dc26b957d23ae29e77b1c36811d2c2a2
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39306072"
---
# <a name="customize-user-experience-with-pattern-language"></a><span data-ttu-id="b2cd5-103">Personalización de la experiencia de usuario con lenguaje de patrones</span><span class="sxs-lookup"><span data-stu-id="b2cd5-103">Customize user experience with pattern language</span></span>

<span data-ttu-id="b2cd5-104">Para personalizar un aviso o invalidar una plantilla predeterminada, puede usar un lenguaje de patrones para especificar el contenido o el formato del aviso.</span><span class="sxs-lookup"><span data-stu-id="b2cd5-104">When you customize a prompt or override a default template, you can use pattern language to specify the contents and/or format of the prompt.</span></span> 

## <a name="prompts-and-templates"></a><span data-ttu-id="b2cd5-105">Avisos y plantillas</span><span class="sxs-lookup"><span data-stu-id="b2cd5-105">Prompts and templates</span></span>

<span data-ttu-id="b2cd5-106">Un [aviso][promptAttribute] define el mensaje que se envía al usuario para solicitar información o para pedir confirmación.</span><span class="sxs-lookup"><span data-stu-id="b2cd5-106">A [prompt][promptAttribute] defines the message that is sent to the user to request a piece of information or ask for confirmation.</span></span> <span data-ttu-id="b2cd5-107">Puede personalizar un aviso con el [atributo Prompt](bot-builder-dotnet-formflow-advanced.md#customize-prompts-using-the-prompt-attribute) o de manera implícita por medio de [IFormBuilder<T>.Field][field].</span><span class="sxs-lookup"><span data-stu-id="b2cd5-107">You can customize a prompt by using the [Prompt attribute](bot-builder-dotnet-formflow-advanced.md#customize-prompts-using-the-prompt-attribute) or implicitly through [IFormBuilder<T>.Field][field].</span></span> 

<span data-ttu-id="b2cd5-108">Los formularios usan plantillas para construir automáticamente avisos y otras cosas como la ayuda.</span><span class="sxs-lookup"><span data-stu-id="b2cd5-108">Forms use templates to automatically construct prompts and other things such as help.</span></span> <span data-ttu-id="b2cd5-109">Puede invalidar la plantilla predeterminada de una clase o un campo mediante el [atributo Template](bot-builder-dotnet-formflow-advanced.md#customize-prompts-using-the-template-attribute).</span><span class="sxs-lookup"><span data-stu-id="b2cd5-109">You can override the default template of a class or field by using the [Template attribute](bot-builder-dotnet-formflow-advanced.md#customize-prompts-using-the-template-attribute).</span></span> 

> [!TIP]
> <span data-ttu-id="b2cd5-110">La clase [FormConfiguration.Templates][formConfiguration] define un conjunto de plantillas integradas que ofrecen buenos ejemplos de cómo usar lenguaje de patrones.</span><span class="sxs-lookup"><span data-stu-id="b2cd5-110">The [FormConfiguration.Templates][formConfiguration] class defines a set of built-in templates that provide good examples of how to use pattern language.</span></span>

## <a name="elements-of-pattern-language"></a><span data-ttu-id="b2cd5-111">Elementos de lenguaje de patrones</span><span class="sxs-lookup"><span data-stu-id="b2cd5-111">Elements of pattern language</span></span>

<span data-ttu-id="b2cd5-112">El lenguaje de patrones usa llaves (`{}`) para identificar los elementos que se reemplazarán en tiempo de ejecución por valores reales.</span><span class="sxs-lookup"><span data-stu-id="b2cd5-112">Pattern language uses curly braces (`{}`) to identify elements that will be replaced at runtime with actual values.</span></span> <span data-ttu-id="b2cd5-113">En esta tabla se enumeran los elementos del lenguaje de patrones.</span><span class="sxs-lookup"><span data-stu-id="b2cd5-113">This table lists the elements of pattern language.</span></span>

| <span data-ttu-id="b2cd5-114">Elemento</span><span class="sxs-lookup"><span data-stu-id="b2cd5-114">Element</span></span> | <span data-ttu-id="b2cd5-115">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="b2cd5-115">Description</span></span> |
|----|----|
| `{<format>}` | <span data-ttu-id="b2cd5-116">Muestra el valor del campo actual (el campo al que se aplica el atributo).</span><span class="sxs-lookup"><span data-stu-id="b2cd5-116">Shows the value of the current field (the field that the attribute applies to).</span></span> |
| `{&}` | <span data-ttu-id="b2cd5-117">Muestra la descripción del campo actual (a menos que se especifique lo contrario, este es el nombre del campo).</span><span class="sxs-lookup"><span data-stu-id="b2cd5-117">Shows the description of the current field (unless otherwise specified, this is the name of the field).</span></span> |
| `{<field><format>}` | <span data-ttu-id="b2cd5-118">Muestra el valor del campo designado.</span><span class="sxs-lookup"><span data-stu-id="b2cd5-118">Shows the value of the named field.</span></span> | 
| `{&<field>}` | <span data-ttu-id="b2cd5-119">Muestra la descripción del campo designado.</span><span class="sxs-lookup"><span data-stu-id="b2cd5-119">Shows the description of the named field.</span></span> |
| <code>{&#124;&#124;}</code> | <span data-ttu-id="b2cd5-120">Muestra las opciones actuales, que podrían ser el valor actual de un campo, "sin preferencias" o los valores de una enumeración.</span><span class="sxs-lookup"><span data-stu-id="b2cd5-120">Shows the current choice(s), which could be the current value of a field, "no preference" or the values of an enumeration.</span></span> |
| `{[{<field><format>} ...]}` | <span data-ttu-id="b2cd5-121">Muestra una lista de valores de los campos designados mediante [Separator][separator] y [LastSeparator][lastSeparator] para separar los valores individuales de la lista.</span><span class="sxs-lookup"><span data-stu-id="b2cd5-121">Shows a list of values from the named fields using [Separator][separator] and [LastSeparator][lastSeparator] to separate the individual values in the list.</span></span> |
| `{*}` | <span data-ttu-id="b2cd5-122">Muestra una línea para cada campo activo; cada línea contiene la descripción del campo y el valor actual.</span><span class="sxs-lookup"><span data-stu-id="b2cd5-122">Shows one line for each active field; each line contains the field description and current value.</span></span> | 
| `{*filled}` | <span data-ttu-id="b2cd5-123">Muestra una línea para cada campo activo que contiene un valor actual; cada línea contiene la descripción del campo y el valor actual.</span><span class="sxs-lookup"><span data-stu-id="b2cd5-123">Shows one line for each active field that contains an actual value; each line contains the field description and current value.</span></span> |
| `{<nth><format>}` | <span data-ttu-id="b2cd5-124">Un especificador normal de formato en C# que se aplica al enésimo argumento de una plantilla.</span><span class="sxs-lookup"><span data-stu-id="b2cd5-124">A regular C# format specifier that applies to the nth argument of a template.</span></span> <span data-ttu-id="b2cd5-125">Para obtener la lista de argumentos disponibles, consulte [TemplateUsage][templateUsage].</span><span class="sxs-lookup"><span data-stu-id="b2cd5-125">For the list of available arguments, see [TemplateUsage][templateUsage].</span></span> |
| `{?<textOrPatternElement>...}` | <span data-ttu-id="b2cd5-126">Sustitución condicional.</span><span class="sxs-lookup"><span data-stu-id="b2cd5-126">Conditional substitution.</span></span> <span data-ttu-id="b2cd5-127">Si todo lo referente a elementos de patrón tiene valores, estos se sustituyen y se usa la expresión entera.</span><span class="sxs-lookup"><span data-stu-id="b2cd5-127">If all referred to pattern elements have values, the values are substituted and the whole expression is used.</span></span> |

<span data-ttu-id="b2cd5-128">Para los elementos mencionados anteriormente:</span><span class="sxs-lookup"><span data-stu-id="b2cd5-128">For the elements listed above:</span></span>

- <span data-ttu-id="b2cd5-129">El marcador de posición `<field>` es el nombre de un campo en la clase de formulario.</span><span class="sxs-lookup"><span data-stu-id="b2cd5-129">The `<field>` placeholder is the name of a field in your form class.</span></span> <span data-ttu-id="b2cd5-130">Por ejemplo, si la clase contiene un campo con el nombre `Size`, puede especificar `{Size}` como el elemento de patrón.</span><span class="sxs-lookup"><span data-stu-id="b2cd5-130">For example, if your class contains a field with the name `Size`, you could specify `{Size}` as the pattern element.</span></span> 

- <span data-ttu-id="b2cd5-131">Un botón de puntos suspensivos (`"..."`) dentro de un elemento de patrón indica que el elemento puede contener varios valores.</span><span class="sxs-lookup"><span data-stu-id="b2cd5-131">An ellipses (`"..."`) within a pattern element indicates that the element may contain multiple values.</span></span>

- <span data-ttu-id="b2cd5-132">El marcador de posición `<format>` es un especificador de formato en C#.</span><span class="sxs-lookup"><span data-stu-id="b2cd5-132">The `<format>` placeholder is a C# format specifier.</span></span> <span data-ttu-id="b2cd5-133">Por ejemplo, si la clase contiene un campo con el nombre `Rating` y de tipo `double`, puede especificar `{Rating:F2}` como elemento de patrón para mostrar dos dígitos de precisión.</span><span class="sxs-lookup"><span data-stu-id="b2cd5-133">For example, if the class contains a field with the name `Rating` and of type `double`, you could specify `{Rating:F2}` as the pattern element to show two digits of precision.</span></span>

- <span data-ttu-id="b2cd5-134">El marcador de posición `<nth>` hace referencia al enésimo argumento de una plantilla.</span><span class="sxs-lookup"><span data-stu-id="b2cd5-134">The `<nth>` placeholder references the nth argument of a template.</span></span>

### <a name="pattern-language-within-a-prompt-attribute"></a><span data-ttu-id="b2cd5-135">Lenguaje de patrones dentro de un atributo Prompt</span><span class="sxs-lookup"><span data-stu-id="b2cd5-135">Pattern language within a Prompt attribute</span></span>

<span data-ttu-id="b2cd5-136">En este ejemplo se usa el elemento `{&}` para mostrar la descripción del campo `Sandwich` y el elemento `{||}` para mostrar la lista de opciones para ese campo.</span><span class="sxs-lookup"><span data-stu-id="b2cd5-136">This example uses the `{&}` element to show the description of the `Sandwich` field and the `{||}` element to show the list of choices for that field.</span></span>

[!code-csharp[Patterns example](../includes/code/dotnet-formflow-pattern-language.cs#patterns1)]

<span data-ttu-id="b2cd5-137">Este es el aviso resultante:</span><span class="sxs-lookup"><span data-stu-id="b2cd5-137">This is the resulting prompt:</span></span>

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

## <a name="formatting-parameters"></a><span data-ttu-id="b2cd5-138">Parámetros de formato</span><span class="sxs-lookup"><span data-stu-id="b2cd5-138">Formatting parameters</span></span>

<span data-ttu-id="b2cd5-139">Los avisos y las plantillas admiten estos parámetros de formato.</span><span class="sxs-lookup"><span data-stu-id="b2cd5-139">Prompts and templates support these formatting parameters.</span></span>

| <span data-ttu-id="b2cd5-140">Uso</span><span class="sxs-lookup"><span data-stu-id="b2cd5-140">Usage</span></span> | <span data-ttu-id="b2cd5-141">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="b2cd5-141">Description</span></span> |
|----|----|
| `AllowDefault` | <span data-ttu-id="b2cd5-142">Se aplica a los elementos de patrón <code>{&#124;&#124;}</code>.</span><span class="sxs-lookup"><span data-stu-id="b2cd5-142">Applies to <code>{&#124;&#124;}</code> pattern elements.</span></span> <span data-ttu-id="b2cd5-143">Determina si el formulario debe mostrar el valor actual del campo como una posible opción.</span><span class="sxs-lookup"><span data-stu-id="b2cd5-143">Determines whether the form should show the current value of the field as a possible choice.</span></span> <span data-ttu-id="b2cd5-144">Si es `true`, se muestra el valor actual como posible valor.</span><span class="sxs-lookup"><span data-stu-id="b2cd5-144">If `true`, the current value is shown as a possible value.</span></span> <span data-ttu-id="b2cd5-145">El valor predeterminado es `true`.</span><span class="sxs-lookup"><span data-stu-id="b2cd5-145">The default is `true`.</span></span> |
| `ChoiceCase` | <span data-ttu-id="b2cd5-146">Se aplica a los elementos de patrón <code>{&#124;&#124;}</code>.</span><span class="sxs-lookup"><span data-stu-id="b2cd5-146">Applies to <code>{&#124;&#124;}</code> pattern elements.</span></span> <span data-ttu-id="b2cd5-147">Determina si el texto de cada opción se normaliza (por ejemplo, si la primera letra de cada palabra se pone en mayúscula).</span><span class="sxs-lookup"><span data-stu-id="b2cd5-147">Determines whether the text of each choice is normalized (e.g., whether the first letter of each word is capitalized).</span></span> <span data-ttu-id="b2cd5-148">El valor predeterminado es `CaseNormalization.None`.</span><span class="sxs-lookup"><span data-stu-id="b2cd5-148">The default is `CaseNormalization.None`.</span></span> <span data-ttu-id="b2cd5-149">Para conocer los valores posibles, consulte [CaseNormalization][caseNormalization].</span><span class="sxs-lookup"><span data-stu-id="b2cd5-149">For possible values, see [CaseNormalization][caseNormalization].</span></span> |
| `ChoiceFormat` | <span data-ttu-id="b2cd5-150">Se aplica a los elementos de patrón <code>{&#124;&#124;}</code>.</span><span class="sxs-lookup"><span data-stu-id="b2cd5-150">Applies to <code>{&#124;&#124;}</code> pattern elements.</span></span> <span data-ttu-id="b2cd5-151">Determina si se debe mostrar una lista de opciones como una lista numerada o una lista con viñetas.</span><span class="sxs-lookup"><span data-stu-id="b2cd5-151">Determines whether to show a list of choices as a numbered list or a bulleted list.</span></span> <span data-ttu-id="b2cd5-152">Para obtener una lista numerada, establezca `ChoiceFormat` en `{0}` (valor predeterminado).</span><span class="sxs-lookup"><span data-stu-id="b2cd5-152">For a numbered list, set `ChoiceFormat` to `{0}` (default).</span></span> <span data-ttu-id="b2cd5-153">Para obtener una lista con viñetas, establezca `ChoiceFormat` en `{1}`.</span><span class="sxs-lookup"><span data-stu-id="b2cd5-153">For a bulleted list list, set `ChoiceFormat` to `{1}`.</span></span> |
| `ChoiceLastSeparator` | <span data-ttu-id="b2cd5-154">Se aplica a los elementos de patrón <code>{&#124;&#124;}</code>.</span><span class="sxs-lookup"><span data-stu-id="b2cd5-154">Applies to <code>{&#124;&#124;}</code> pattern elements.</span></span> <span data-ttu-id="b2cd5-155">Determina si una lista alineada de opciones incluye un separador delante de la última opción.</span><span class="sxs-lookup"><span data-stu-id="b2cd5-155">Determines whether an inline list of choices includes a separator before the last choice.</span></span> |
| `ChoiceParens` | <span data-ttu-id="b2cd5-156">Se aplica a los elementos de patrón <code>{&#124;&#124;}</code>.</span><span class="sxs-lookup"><span data-stu-id="b2cd5-156">Applies to <code>{&#124;&#124;}</code> pattern elements.</span></span> <span data-ttu-id="b2cd5-157">Determina si una lista alineada de opciones se muestra entre paréntesis.</span><span class="sxs-lookup"><span data-stu-id="b2cd5-157">Determines whether an inline list of choices is shown within parentheses.</span></span> <span data-ttu-id="b2cd5-158">Si es `true`, la lista de opciones se muestra entre paréntesis.</span><span class="sxs-lookup"><span data-stu-id="b2cd5-158">If `true`, the list of choices is shown within parentheses.</span></span> <span data-ttu-id="b2cd5-159">El valor predeterminado es `true`.</span><span class="sxs-lookup"><span data-stu-id="b2cd5-159">The default is `true`.</span></span> |
| `ChoiceSeparator` | <span data-ttu-id="b2cd5-160">Se aplica a los elementos de patrón <code>{&#124;&#124;}</code>.</span><span class="sxs-lookup"><span data-stu-id="b2cd5-160">Applies to <code>{&#124;&#124;}</code> pattern elements.</span></span> <span data-ttu-id="b2cd5-161">Determina si una lista alineada de opciones incluye un separador delate de cada opción excepto la última.</span><span class="sxs-lookup"><span data-stu-id="b2cd5-161">Determines whether an inline list of choices includes a separator before every choice except the last choice.</span></span> | 
| `ChoiceStyle` | <span data-ttu-id="b2cd5-162">Se aplica a los elementos de patrón <code>{&#124;&#124;}</code>.</span><span class="sxs-lookup"><span data-stu-id="b2cd5-162">Applies to <code>{&#124;&#124;}</code> pattern elements.</span></span> <span data-ttu-id="b2cd5-163">Determina si la lista de opciones se muestra alineada o por línea.</span><span class="sxs-lookup"><span data-stu-id="b2cd5-163">Determines whether the list of choices is shown inline or per line.</span></span> <span data-ttu-id="b2cd5-164">El valor predeterminado es `ChoiceStyleOptions.Auto` que determina en tiempo de ejecución si la opción se debe mostrar alineada o en una lista.</span><span class="sxs-lookup"><span data-stu-id="b2cd5-164">The default is `ChoiceStyleOptions.Auto` which determines at runtime whether to show the choice inline or in a list.</span></span> <span data-ttu-id="b2cd5-165">Para conocer los valores posibles, consulte [ChoiceStyleOptions][choiceStyleOptions].</span><span class="sxs-lookup"><span data-stu-id="b2cd5-165">For possible values, see [ChoiceStyleOptions][choiceStyleOptions].</span></span> |
| `Feedback` | <span data-ttu-id="b2cd5-166">Se aplica solo a los avisos.</span><span class="sxs-lookup"><span data-stu-id="b2cd5-166">Applies to prompts only.</span></span> <span data-ttu-id="b2cd5-167">Determina si el formulario repite la elección del usuario para indicar que comprende la selección.</span><span class="sxs-lookup"><span data-stu-id="b2cd5-167">Determines whether the form echoes the user's choice to indicate that the form understood the selection.</span></span> <span data-ttu-id="b2cd5-168">El valor predeterminado es `FeedbackOptions.Auto` que repite la entrada del usuario solo si alguna de sus partes no se entiende.</span><span class="sxs-lookup"><span data-stu-id="b2cd5-168">The default is `FeedbackOptions.Auto` which echoes the user's input only if part of it is not understood.</span></span> <span data-ttu-id="b2cd5-169">Para conocer los valores posibles, consulte [FeedbackOptions][feedbackOptions].</span><span class="sxs-lookup"><span data-stu-id="b2cd5-169">For possible values, see [FeedbackOptions][feedbackOptions].</span></span> |
| `FieldCase` | <span data-ttu-id="b2cd5-170">Determina si el texto de la descripción del campo se normaliza (por ejemplo, si la primera letra de cada palabra se escribe en mayúsculas).</span><span class="sxs-lookup"><span data-stu-id="b2cd5-170">Determines whether the text of the field's description is normalized (e.g., whether the first letter of each word is capitalized).</span></span> <span data-ttu-id="b2cd5-171">El valor predeterminado es `CaseNormalization.Lower` que pasa la descripción a minúsculas.</span><span class="sxs-lookup"><span data-stu-id="b2cd5-171">The default is `CaseNormalization.Lower` which converts the description to lowercase.</span></span> <span data-ttu-id="b2cd5-172">Para conocer los valores posibles, consulte [CaseNormalization][caseNormalization].</span><span class="sxs-lookup"><span data-stu-id="b2cd5-172">For possible values, see [CaseNormalization][caseNormalization].</span></span> |
| `LastSeparator` | <span data-ttu-id="b2cd5-173">Se aplica a los elementos de patrón `{[]}`.</span><span class="sxs-lookup"><span data-stu-id="b2cd5-173">Applies to `{[]}` pattern elements.</span></span> <span data-ttu-id="b2cd5-174">Determina si una matriz de elementos incluye un separador delante de la última opción.</span><span class="sxs-lookup"><span data-stu-id="b2cd5-174">Determines whether an array of items includes a separator before the last item.</span></span> |
| `Separator` | <span data-ttu-id="b2cd5-175">Se aplica a los elementos de patrón `{[]}`.</span><span class="sxs-lookup"><span data-stu-id="b2cd5-175">Applies to `{[]}` pattern elements.</span></span> <span data-ttu-id="b2cd5-176">Determina si una matriz de elementos incluye un separador delante de cada elemento de la matriz excepto el último.</span><span class="sxs-lookup"><span data-stu-id="b2cd5-176">Determines whether an array of items includes a separator before every item in the array except the last item.</span></span> |
| `ValueCase` | <span data-ttu-id="b2cd5-177">Determina si el texto del valor del campo se normaliza (por ejemplo, si la primera letra de cada palabra se escribe en mayúsculas).</span><span class="sxs-lookup"><span data-stu-id="b2cd5-177">Determines whether the text of the field's value is normalized (e.g., whether the first letter of each word is capitalized)_.</span></span> <span data-ttu-id="b2cd5-178">El valor predeterminado es `CaseNormalization.InitialUpper` que convierte la primera letra de cada palabra a mayúsculas.</span><span class="sxs-lookup"><span data-stu-id="b2cd5-178">The default is `CaseNormalization.InitialUpper` which converts the first letter of each word to uppercase.</span></span> <span data-ttu-id="b2cd5-179">Para conocer los valores posibles, consulte [CaseNormalization][caseNormalization].</span><span class="sxs-lookup"><span data-stu-id="b2cd5-179">For possible values, see [CaseNormalization][caseNormalization].</span></span> |

### <a name="prompt-attribute-with-formatting-parameter"></a><span data-ttu-id="b2cd5-180">Atributo Prompt con parámetro de formato</span><span class="sxs-lookup"><span data-stu-id="b2cd5-180">Prompt attribute with formatting parameter</span></span> 

<span data-ttu-id="b2cd5-181">En este ejemplo se usa el parámetro `ChoiceFormat` para especificar que la lista de opciones debe mostrarse como una lista con viñetas.</span><span class="sxs-lookup"><span data-stu-id="b2cd5-181">This example uses the `ChoiceFormat` parameter to specify that the list of choices should be displayed as a bulleted list.</span></span>

[!code-csharp[Patterns example](../includes/code/dotnet-formflow-pattern-language.cs#patterns2)]

<span data-ttu-id="b2cd5-182">Este es el aviso resultante:</span><span class="sxs-lookup"><span data-stu-id="b2cd5-182">This is the resulting prompt:</span></span>

```console
What kind of sandwich would you like?
* BLT
* Black Forest Ham
* Buffalo Chicken
* Chicken And Bacon Ranch Melt
* Cold Cut Combo
* Meatball Marinara
* Oven Roasted Chicken
* Roast Beef
* Rotisserie Style Chicken
* Spicy Italian
* Steak And Cheese
* Sweet Onion Teriyaki
* Tuna
* Turkey Breast
* Veggie
>
```

## <a name="sample-code"></a><span data-ttu-id="b2cd5-183">Código de ejemplo</span><span class="sxs-lookup"><span data-stu-id="b2cd5-183">Sample code</span></span>

[!INCLUDE [Sample code](../includes/snippet-dotnet-formflow-samples.md)]

## <a name="additional-resources"></a><span data-ttu-id="b2cd5-184">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="b2cd5-184">Additional resources</span></span>

- [<span data-ttu-id="b2cd5-185">Características básicas de FormFlow</span><span class="sxs-lookup"><span data-stu-id="b2cd5-185">Basic features of FormFlow</span></span>](bot-builder-dotnet-formflow.md)
- [<span data-ttu-id="b2cd5-186">Características avanzadas de FormFlow</span><span class="sxs-lookup"><span data-stu-id="b2cd5-186">Advanced features of FormFlow</span></span>](bot-builder-dotnet-formflow-advanced.md)
- [<span data-ttu-id="b2cd5-187">Personalización de un formulario mediante FormBuilder</span><span class="sxs-lookup"><span data-stu-id="b2cd5-187">Customize a form using FormBuilder</span></span>](bot-builder-dotnet-formflow-formbuilder.md)
- [<span data-ttu-id="b2cd5-188">Localización del contenido del formulario</span><span class="sxs-lookup"><span data-stu-id="b2cd5-188">Localize form content</span></span>](bot-builder-dotnet-formflow-localize.md)
- [<span data-ttu-id="b2cd5-189">Definición de un formulario mediante esquemas JSON</span><span class="sxs-lookup"><span data-stu-id="b2cd5-189">Define a form using JSON schema</span></span>](bot-builder-dotnet-formflow-json-schema.md)
- <span data-ttu-id="b2cd5-190"><a href="/dotnet/api/?view=botbuilder-3.11.0" target="_blank">Referencia de Bot Builder SDK para .NET</a></span><span class="sxs-lookup"><span data-stu-id="b2cd5-190"><a href="/dotnet/api/?view=botbuilder-3.11.0" target="_blank">Bot Builder SDK for .NET Reference</a></span></span>

[promptAttribute]: /dotnet/api/microsoft.bot.builder.formflow.promptattribute

[field]: /dotnet/api/microsoft.bot.builder.formflow.iformbuilder-1.field

[formConfiguration]: /dotnet/api/microsoft.bot.builder.formflow.formconfiguration

[separator]: /dotnet/api/microsoft.bot.builder.formflow.advanced.templatebaseattribute.separator

[lastSeparator]: /dotnet/api/microsoft.bot.builder.formflow.advanced.templatebaseattribute.lastseparator

[templateUsage]: /dotnet/api/microsoft.bot.builder.formflow.templateusage

[caseNormalization]: /dotnet/api/microsoft.bot.builder.formflow.casenormalization

[choiceStyleOptions]: /dotnet/api/microsoft.bot.builder.formflow.choicestyleoptions

[feedbackOptions]: /dotnet/api/microsoft.bot.builder.formflow.feedbackoptions
