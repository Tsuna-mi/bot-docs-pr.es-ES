---
title: Definición de un formulario mediante esquemas JSON y FormFlow | Microsoft Docs
description: Aprenda a definir un formulario mediante esquemas JSON y FormFlow con Bot Builder SDK para. NET.
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: a7f6e3f186e0c4b9f6096cad72a91ef6f3fdffd4
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39306096"
---
# <a name="define-a-form-using-json-schema"></a><span data-ttu-id="58e69-103">Definición de un formulario mediante esquemas JSON</span><span class="sxs-lookup"><span data-stu-id="58e69-103">Define a form using JSON schema</span></span>

<span data-ttu-id="58e69-104">Si usa una [clase de C#](bot-builder-dotnet-formflow.md#create-class) para definir el formulario cuando crea un bot con FormFlow, el formulario se deriva de la definición estática de su tipo en C#.</span><span class="sxs-lookup"><span data-stu-id="58e69-104">If you use a [C# class](bot-builder-dotnet-formflow.md#create-class) to define the form when you create a bot with FormFlow, the form derives from the static definition of your type in C#.</span></span> <span data-ttu-id="58e69-105">En su lugar, puede definir el formulario mediante <a href="http://json-schema.org/documentation.html" target="_blank">esquemas JSON</a>.</span><span class="sxs-lookup"><span data-stu-id="58e69-105">As an alternative, you may instead define the form by using <a href="http://json-schema.org/documentation.html" target="_blank">JSON schema</a>.</span></span> <span data-ttu-id="58e69-106">Un formulario que se define mediante esquemas JSON se basa por completo en datos. Puede cambiar el formulario (y por tanto, el comportamiento del bot) solo con actualizar el esquema.</span><span class="sxs-lookup"><span data-stu-id="58e69-106">A form that is defined using JSON schema is purely data-driven; you can change the form (and therefore, the behavior of the bot) simply by updating the schema.</span></span> 

<span data-ttu-id="58e69-107">El esquema JSON describe los campos dentro de <a href="http://www.newtonsoft.com/json/help/html/T_Newtonsoft_Json_Linq_JObject.htm" target="_blank">JObject</a> e incluye anotaciones que controlan los avisos, las plantillas y los términos.</span><span class="sxs-lookup"><span data-stu-id="58e69-107">The JSON schema describes the fields within your <a href="http://www.newtonsoft.com/json/help/html/T_Newtonsoft_Json_Linq_JObject.htm" target="_blank">JObject</a> and includes annotations that control prompts, templates, and terms.</span></span> <span data-ttu-id="58e69-108">Para usar el esquema JSON con FormFlow, primero debe agregar el paquete NuGet `Microsoft.Bot.Builder.FormFlow.Json` al proyecto e importar el espacio de nombres `Microsoft.Bot.Builder.FormFlow.Json`.</span><span class="sxs-lookup"><span data-stu-id="58e69-108">To use JSON schema with FormFlow, you must add the `Microsoft.Bot.Builder.FormFlow.Json` NuGet package to your project and import the `Microsoft.Bot.Builder.FormFlow.Json` namespace.</span></span>

## <a name="standard-keywords"></a><span data-ttu-id="58e69-109">Palabras clave estándar</span><span class="sxs-lookup"><span data-stu-id="58e69-109">Standard keywords</span></span> 

<span data-ttu-id="58e69-110">FormFlow admite estas palabras clave estándar de <a href="http://json-schema.org/documentation.html" target="_blank">esquema JSON</a>:</span><span class="sxs-lookup"><span data-stu-id="58e69-110">FormFlow supports these standard <a href="http://json-schema.org/documentation.html" target="_blank">JSON Schema</a> keywords:</span></span>

| <span data-ttu-id="58e69-111">Palabra clave</span><span class="sxs-lookup"><span data-stu-id="58e69-111">Keyword</span></span> | <span data-ttu-id="58e69-112">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="58e69-112">Description</span></span> | 
|----|----|
| <span data-ttu-id="58e69-113">Tipo</span><span class="sxs-lookup"><span data-stu-id="58e69-113">type</span></span> | <span data-ttu-id="58e69-114">Define el tipo de datos que contiene el campo.</span><span class="sxs-lookup"><span data-stu-id="58e69-114">Defines the type of data that the field contains.</span></span> |
| <span data-ttu-id="58e69-115">enum</span><span class="sxs-lookup"><span data-stu-id="58e69-115">enum</span></span> | <span data-ttu-id="58e69-116">Define los valores válidos para el campo.</span><span class="sxs-lookup"><span data-stu-id="58e69-116">Defines the valid values for the field.</span></span> |
| <span data-ttu-id="58e69-117">minimum</span><span class="sxs-lookup"><span data-stu-id="58e69-117">minimum</span></span> | <span data-ttu-id="58e69-118">Define el valor numérico mínimo permitido para el campo (como se describe en [NumericAttribute][numericAttribute]).</span><span class="sxs-lookup"><span data-stu-id="58e69-118">Defines the minimum numeric value allowed for the field (as described in [NumericAttribute][numericAttribute]).</span></span> |
| <span data-ttu-id="58e69-119">maximum</span><span class="sxs-lookup"><span data-stu-id="58e69-119">maximum</span></span> | <span data-ttu-id="58e69-120">Define el valor numérico máximo permitido para el campo (como se describe en [NumericAttribute][numericAttribute]).</span><span class="sxs-lookup"><span data-stu-id="58e69-120">Defines the maximum numeric value allowed for the field (as described in [NumericAttribute][numericAttribute]).</span></span> |
| <span data-ttu-id="58e69-121">requerido</span><span class="sxs-lookup"><span data-stu-id="58e69-121">required</span></span> | <span data-ttu-id="58e69-122">Define qué campos son obligatorios.</span><span class="sxs-lookup"><span data-stu-id="58e69-122">Defines which fields are required.</span></span> |
| <span data-ttu-id="58e69-123">Patrón</span><span class="sxs-lookup"><span data-stu-id="58e69-123">pattern</span></span> | <span data-ttu-id="58e69-124">Valida los valores de cadena (como se describe en [PatternAttribute][patternAttribute]).</span><span class="sxs-lookup"><span data-stu-id="58e69-124">Validates string values (as described in [PatternAttribute][patternAttribute]).</span></span> |

## <a name="extensions-to-json-schema"></a><span data-ttu-id="58e69-125">Extensiones de esquemas JSON</span><span class="sxs-lookup"><span data-stu-id="58e69-125">Extensions to JSON Schema</span></span>

<span data-ttu-id="58e69-126">FormFlow extiende el <a href="http://json-schema.org/documentation.html" target="_blank">esquema JSON</a> estándar para admitir varias propiedades adicionales.</span><span class="sxs-lookup"><span data-stu-id="58e69-126">FormFlow extends the standard <a href="http://json-schema.org/documentation.html" target="_blank">JSON Schema</a> to support several additional properties.</span></span>

### <a name="additional-properties-at-the-root-of-the-schema"></a><span data-ttu-id="58e69-127">Propiedades adicionales en la raíz del esquema</span><span class="sxs-lookup"><span data-stu-id="58e69-127">Additional properties at the root of the schema</span></span>

| <span data-ttu-id="58e69-128">Propiedad</span><span class="sxs-lookup"><span data-stu-id="58e69-128">Property</span></span> | <span data-ttu-id="58e69-129">Valor</span><span class="sxs-lookup"><span data-stu-id="58e69-129">Value</span></span> |
|----|----|
| <span data-ttu-id="58e69-130">OnCompletion</span><span class="sxs-lookup"><span data-stu-id="58e69-130">OnCompletion</span></span> | <span data-ttu-id="58e69-131">Script de C# con argumentos `(IDialogContext context, JObject state)` para rellenar el formulario.</span><span class="sxs-lookup"><span data-stu-id="58e69-131">C# script with arguments `(IDialogContext context, JObject state)` for completing the form.</span></span> |
| <span data-ttu-id="58e69-132">Referencias</span><span class="sxs-lookup"><span data-stu-id="58e69-132">References</span></span> | <span data-ttu-id="58e69-133">Referencias que se van a incluir en los scripts.</span><span class="sxs-lookup"><span data-stu-id="58e69-133">References to include in scripts.</span></span> <span data-ttu-id="58e69-134">Por ejemplo, `[assemblyReference, ...]`.</span><span class="sxs-lookup"><span data-stu-id="58e69-134">For example, `[assemblyReference, ...]`.</span></span> <span data-ttu-id="58e69-135">Las rutas de acceso deben ser absolutas o estar relacionadas con el directorio actual.</span><span class="sxs-lookup"><span data-stu-id="58e69-135">Paths should be absolute or relative to the current directory.</span></span> <span data-ttu-id="58e69-136">De forma predeterminada, el script incluye `Microsoft.Bot.Builder.dll`.</span><span class="sxs-lookup"><span data-stu-id="58e69-136">By default, the script includes `Microsoft.Bot.Builder.dll`.</span></span> |
| <span data-ttu-id="58e69-137">Importaciones</span><span class="sxs-lookup"><span data-stu-id="58e69-137">Imports</span></span> | <span data-ttu-id="58e69-138">Importaciones que se van a incluir en los scripts.</span><span class="sxs-lookup"><span data-stu-id="58e69-138">Imports to include in scripts.</span></span> <span data-ttu-id="58e69-139">Por ejemplo, `[import, ...]`.</span><span class="sxs-lookup"><span data-stu-id="58e69-139">For example, `[import, ...]`.</span></span> <span data-ttu-id="58e69-140">De forma predeterminada, el script incluye los espacios de nombres `Microsoft.Bot.Builder`, `Microsoft.Bot.Builder.Dialogs`, `Microsoft.Bot.Builder.FormFlow`, `Microsoft.Bot.Builder.FormFlow.Advanced`, `System.Collections.Generic` y `System.Linq`.</span><span class="sxs-lookup"><span data-stu-id="58e69-140">By default, the script includes the `Microsoft.Bot.Builder`, `Microsoft.Bot.Builder.Dialogs`, `Microsoft.Bot.Builder.FormFlow`, `Microsoft.Bot.Builder.FormFlow.Advanced`, `System.Collections.Generic`, and `System.Linq` namespaces.</span></span> |

### <a name="additional-properties-at-the-root-of-the-schema-or-as-peers-of-the-type-property"></a><span data-ttu-id="58e69-141">Propiedades adicionales en la raíz del esquema o como elementos del mismo nivel de la propiedad type</span><span class="sxs-lookup"><span data-stu-id="58e69-141">Additional properties at the root of the schema or as peers of the type property</span></span>

| <span data-ttu-id="58e69-142">Propiedad</span><span class="sxs-lookup"><span data-stu-id="58e69-142">Property</span></span> | <span data-ttu-id="58e69-143">Valor</span><span class="sxs-lookup"><span data-stu-id="58e69-143">Value</span></span> |
|----|----|
| <span data-ttu-id="58e69-144">Plantillas</span><span class="sxs-lookup"><span data-stu-id="58e69-144">Templates</span></span> | `{ TemplateUsage: { Patterns: [string, ...], <args> }, ...}` |
| <span data-ttu-id="58e69-145">Prompt</span><span class="sxs-lookup"><span data-stu-id="58e69-145">Prompt</span></span> | `{ Patterns:[string, ...] <args>}` |

<span data-ttu-id="58e69-146">Para especificar plantillas y avisos en el esquema JSON, use el mismo vocabulario que se definió en [TemplateAttribute][templateAttribute] y [PromptAttribute][promptAttribute].</span><span class="sxs-lookup"><span data-stu-id="58e69-146">To specify templates and prompts in JSON schema, use the same vocabulary as defined by [TemplateAttribute][templateAttribute] and [PromptAttribute][promptAttribute].</span></span> <span data-ttu-id="58e69-147">Los nombres y valores de propiedad del esquema deben coincidir con los nombres y valores de propiedad de la enumeración de C# subyacente.</span><span class="sxs-lookup"><span data-stu-id="58e69-147">Property names and values in the schema should match the property names and values in the underlying C# enumeration.</span></span> <span data-ttu-id="58e69-148">Por ejemplo, este fragmento de esquema define una plantilla que reemplaza la plantilla `TemplateUsage.NotUnderstood` y especifica un `TemplateBaseAttribute.ChoiceStyle`:</span><span class="sxs-lookup"><span data-stu-id="58e69-148">For example, this schema snippet defines a template that overrides the `TemplateUsage.NotUnderstood` template and specifies a `TemplateBaseAttribute.ChoiceStyle`:</span></span> 

```json
"Templates":{ "NotUnderstood": { "Patterns": ["I don't get it"], "ChoiceStyle":"Auto"}}
```

### <a name="additional-properties-as-peers-of-the-type-property"></a><span data-ttu-id="58e69-149">Propiedades adicionales como elementos del mismo nivel de la propiedad type</span><span class="sxs-lookup"><span data-stu-id="58e69-149">Additional properties as peers of the type property</span></span>

|   <span data-ttu-id="58e69-150">Propiedad</span><span class="sxs-lookup"><span data-stu-id="58e69-150">Property</span></span>   |          <span data-ttu-id="58e69-151">Contenido</span><span class="sxs-lookup"><span data-stu-id="58e69-151">Contents</span></span>           |                                                   <span data-ttu-id="58e69-152">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="58e69-152">Description</span></span>                                                    |
|--------------|-----------------------------|------------------------------------------------------------------------------------------------------------------|
|   <span data-ttu-id="58e69-153">Datetime</span><span class="sxs-lookup"><span data-stu-id="58e69-153">DateTime</span></span>   |            <span data-ttu-id="58e69-154">booleano</span><span class="sxs-lookup"><span data-stu-id="58e69-154">bool</span></span>             |                                  <span data-ttu-id="58e69-155">Indica si el campo es un campo `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="58e69-155">Indicates whether field is a `DateTime` field.</span></span>                                  |
|   <span data-ttu-id="58e69-156">Describe</span><span class="sxs-lookup"><span data-stu-id="58e69-156">Describe</span></span>   |      <span data-ttu-id="58e69-157">cadena u objeto</span><span class="sxs-lookup"><span data-stu-id="58e69-157">string or object</span></span>       |                  <span data-ttu-id="58e69-158">Descripción de un campo como se describe en [DescribeAttribute][describeAttribute].</span><span class="sxs-lookup"><span data-stu-id="58e69-158">Description of a field as described in [DescribeAttribute][describeAttribute].</span></span>                  |
|    <span data-ttu-id="58e69-159">Términos</span><span class="sxs-lookup"><span data-stu-id="58e69-159">Terms</span></span>     |       `[string,...]`        |                  <span data-ttu-id="58e69-160">Expresiones regulares para la coincidencia con un valor de campo como se describe en TermsAttribute.</span><span class="sxs-lookup"><span data-stu-id="58e69-160">Regular expressions for matching a field value as described in TermsAttribute.</span></span>                  |
|  <span data-ttu-id="58e69-161">MaxPhrase</span><span class="sxs-lookup"><span data-stu-id="58e69-161">MaxPhrase</span></span>   |             <span data-ttu-id="58e69-162">int</span><span class="sxs-lookup"><span data-stu-id="58e69-162">int</span></span>             |                  <span data-ttu-id="58e69-163">Ejecuta los términos a través de `Language.GenerateTerms(string, int)` para expandirlos.</span><span class="sxs-lookup"><span data-stu-id="58e69-163">Runs your terms through `Language.GenerateTerms(string, int)` to expand them.</span></span>                   |
|    <span data-ttu-id="58e69-164">Valores</span><span class="sxs-lookup"><span data-stu-id="58e69-164">Values</span></span>    | <span data-ttu-id="58e69-165">\`{ string: {Describe:string</span><span class="sxs-lookup"><span data-stu-id="58e69-165">\`{ string: {Describe:string</span></span> |                                  <span data-ttu-id="58e69-166">object, Terms:[string, ...], MaxPhrase}, ...}\`</span><span class="sxs-lookup"><span data-stu-id="58e69-166">object, Terms:[string, ...], MaxPhrase}, ...}\`</span></span>                                  |
|    <span data-ttu-id="58e69-167">Active</span><span class="sxs-lookup"><span data-stu-id="58e69-167">Active</span></span>    |           <span data-ttu-id="58e69-168">script</span><span class="sxs-lookup"><span data-stu-id="58e69-168">script</span></span>            | <span data-ttu-id="58e69-169">Script de C# con argumentos `(JObject state)->bool` para comprobar si el campo, mensaje o confirmación están activos.</span><span class="sxs-lookup"><span data-stu-id="58e69-169">C# script with arguments `(JObject state)->bool` to test whether the field, message, or confirmation is active.</span></span>  |
|   <span data-ttu-id="58e69-170">Validación</span><span class="sxs-lookup"><span data-stu-id="58e69-170">Validate</span></span>   |           <span data-ttu-id="58e69-171">script</span><span class="sxs-lookup"><span data-stu-id="58e69-171">script</span></span>            |      <span data-ttu-id="58e69-172">Script de C# con argumentos `(JObject state, object value)->ValidateResult` para validar un valor de campo.</span><span class="sxs-lookup"><span data-stu-id="58e69-172">C# script with arguments `(JObject state, object value)->ValidateResult` for validating a field value.</span></span>      |
|    <span data-ttu-id="58e69-173">Define</span><span class="sxs-lookup"><span data-stu-id="58e69-173">Define</span></span>    |           <span data-ttu-id="58e69-174">script</span><span class="sxs-lookup"><span data-stu-id="58e69-174">script</span></span>            |        <span data-ttu-id="58e69-175">Script de C# con argumentos `(JObject state, Field<JObject> field)` para definir un campo de forma dinámica.</span><span class="sxs-lookup"><span data-stu-id="58e69-175">C# script with arguments `(JObject state, Field<JObject> field)` for dynamically defining a field.</span></span>        |
|     <span data-ttu-id="58e69-176">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="58e69-176">Next</span></span>     |           <span data-ttu-id="58e69-177">script</span><span class="sxs-lookup"><span data-stu-id="58e69-177">script</span></span>            | <span data-ttu-id="58e69-178">Script de C# con argumentos `(object value, JObject state)` para determinar el siguiente paso después de rellenar un campo.</span><span class="sxs-lookup"><span data-stu-id="58e69-178">C# script with arguments `(object value, JObject state)` for determining the next step after filling in a field.</span></span> |
|    <span data-ttu-id="58e69-179">Antes</span><span class="sxs-lookup"><span data-stu-id="58e69-179">Before</span></span>    |          <span data-ttu-id="58e69-180">\`[confirm</span><span class="sxs-lookup"><span data-stu-id="58e69-180">\`[confirm</span></span>          |                                                  <span data-ttu-id="58e69-181">message, ...]\`</span><span class="sxs-lookup"><span data-stu-id="58e69-181">message, ...]\`</span></span>                                                  |
|    <span data-ttu-id="58e69-182">Después</span><span class="sxs-lookup"><span data-stu-id="58e69-182">After</span></span>     |          <span data-ttu-id="58e69-183">\`[confirm</span><span class="sxs-lookup"><span data-stu-id="58e69-183">\`[confirm</span></span>          |                                                  <span data-ttu-id="58e69-184">message, ...]\`</span><span class="sxs-lookup"><span data-stu-id="58e69-184">message, ...]\`</span></span>                                                  |
| <span data-ttu-id="58e69-185">Dependencias</span><span class="sxs-lookup"><span data-stu-id="58e69-185">Dependencies</span></span> |        <span data-ttu-id="58e69-186">[string, ...]</span><span class="sxs-lookup"><span data-stu-id="58e69-186">[string, ...]</span></span>        |                           <span data-ttu-id="58e69-187">Campos de los que depende este campo, mensaje o confirmación.</span><span class="sxs-lookup"><span data-stu-id="58e69-187">Fields that this field, message, or confirmation depends on.</span></span>                           |

<span data-ttu-id="58e69-188">Use `{Confirm:script|[string, ...], ...templateArgs}` en el valor de la propiedad **Before** o de la propiedad **After** para definir una confirmación mediante un script de C# con un argumento `(JObject state)` o mediante un conjunto de patrones que se seleccionará aleatoriamente con argumentos de plantilla opcionales.</span><span class="sxs-lookup"><span data-stu-id="58e69-188">Use `{Confirm:script|[string, ...], ...templateArgs}` within the value of the **Before** property or the **After** property to define a confirmation by using either a C# script with argument `(JObject state)` or a set of patterns that will be randomly selected with optional template arguments.</span></span>

<span data-ttu-id="58e69-189">Use `{Message:script|[string, ...] ...templateArgs}` en el valor de la propiedad **Before** o de la propiedad **After** para definir un mensaje mediante un script de C# con un argumento `(JObject state)` o mediante un conjunto de patrones que se seleccionará aleatoriamente con argumentos de plantilla opcionales.</span><span class="sxs-lookup"><span data-stu-id="58e69-189">Use `{Message:script|[string, ...] ...templateArgs}` within the value of the **Before** property or the **After** property to define a message by using either a C# script with argument `(JObject state)` or a set of patterns that will be randomly selected with optional template arguments.</span></span>

## <a name="scripts"></a><span data-ttu-id="58e69-190">Scripts</span><span class="sxs-lookup"><span data-stu-id="58e69-190">Scripts</span></span>

<span data-ttu-id="58e69-191">Algunas de las propiedades que se han descrito anteriormente contienen un script como valor de propiedad.</span><span class="sxs-lookup"><span data-stu-id="58e69-191">Several of the properties that are described above contain a script as the property value.</span></span> <span data-ttu-id="58e69-192">Un script puede ser cualquier fragmento de código de C# que puede encontrar normalmente en el cuerpo de un método.</span><span class="sxs-lookup"><span data-stu-id="58e69-192">A script can be any snippet of C# code that you might normally find in the body of a method.</span></span> <span data-ttu-id="58e69-193">Puede agregar referencias mediante la propiedad **References** y la propiedad **Imports**.</span><span class="sxs-lookup"><span data-stu-id="58e69-193">You can add references by using the **References** property and/or the **Imports** property.</span></span> <span data-ttu-id="58e69-194">Entre las variables globales especiales se incluyen:</span><span class="sxs-lookup"><span data-stu-id="58e69-194">Special global variables include:</span></span>

| <span data-ttu-id="58e69-195">Variable</span><span class="sxs-lookup"><span data-stu-id="58e69-195">Variable</span></span> | <span data-ttu-id="58e69-196">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="58e69-196">Description</span></span> |
|----|----|
| <span data-ttu-id="58e69-197">choice</span><span class="sxs-lookup"><span data-stu-id="58e69-197">choice</span></span> | <span data-ttu-id="58e69-198">Distribución interna que ejecuta el script.</span><span class="sxs-lookup"><span data-stu-id="58e69-198">Internal dispatch for the script to execute.</span></span> |
| <span data-ttu-id="58e69-199">state</span><span class="sxs-lookup"><span data-stu-id="58e69-199">state</span></span> | <span data-ttu-id="58e69-200">Límite de estado de formulario `JObject` para todos los scripts.</span><span class="sxs-lookup"><span data-stu-id="58e69-200">`JObject` form state bound for all scripts.</span></span> |
| <span data-ttu-id="58e69-201">ifield</span><span class="sxs-lookup"><span data-stu-id="58e69-201">ifield</span></span> | <span data-ttu-id="58e69-202">`IField<JObject>` para permitir el razonamiento sobre el campo actual para todos los scripts, excepto los generadores de mensajes o avisos de confirmación.</span><span class="sxs-lookup"><span data-stu-id="58e69-202">`IField<JObject>` to allow reasoning over the current field for all scripts except Message/Confirm prompt builders.</span></span> |
| <span data-ttu-id="58e69-203">value</span><span class="sxs-lookup"><span data-stu-id="58e69-203">value</span></span> | <span data-ttu-id="58e69-204">Valor de objeto que se va a validar con **Validate**.</span><span class="sxs-lookup"><span data-stu-id="58e69-204">Object value to be validated for **Validate**.</span></span> |
| <span data-ttu-id="58e69-205">campo</span><span class="sxs-lookup"><span data-stu-id="58e69-205">field</span></span> | <span data-ttu-id="58e69-206">`Field<JObject>` para permitir la actualización dinámica de un campo en **Define**.</span><span class="sxs-lookup"><span data-stu-id="58e69-206">`Field<JObject>` to allow dynamically updating a field in **Define**.</span></span> |
| <span data-ttu-id="58e69-207">contexto</span><span class="sxs-lookup"><span data-stu-id="58e69-207">context</span></span> | <span data-ttu-id="58e69-208">`IDialogContext` contexto para permitir la publicación de resultados en **OnCompletion**.</span><span class="sxs-lookup"><span data-stu-id="58e69-208">`IDialogContext` context to allow posting results in **OnCompletion**.</span></span> |

<span data-ttu-id="58e69-209">Los campos que se definen mediante el esquema JSON tienen la misma capacidad para ampliar o reemplazar las definiciones mediante programación que tiene cualquier otro campo.</span><span class="sxs-lookup"><span data-stu-id="58e69-209">Fields that are defined via JSON schema have the same ability to extend or override the definitions programatically as any other field.</span></span> <span data-ttu-id="58e69-210">También se pueden localizar de la misma manera.</span><span class="sxs-lookup"><span data-stu-id="58e69-210">They can also be localized in the same way.</span></span>

## <a name="json-schema-example"></a><span data-ttu-id="58e69-211">Ejemplo de esquema JSON</span><span class="sxs-lookup"><span data-stu-id="58e69-211">JSON schema example</span></span>

<span data-ttu-id="58e69-212">La manera más sencilla de definir un formulario es definir todo, incluido el código de C#, directamente en el esquema JSON.</span><span class="sxs-lookup"><span data-stu-id="58e69-212">The simplest way to define a form is to define everything, including any C# code, directly in the JSON schema.</span></span> <span data-ttu-id="58e69-213">En este ejemplo se muestra el esquema JSON de un bot anotado que se usa para encargar un sándwich que se ha descrito en [Personalización de un formulario mediante FormBuilder](bot-builder-dotnet-formflow-formbuilder.md).</span><span class="sxs-lookup"><span data-stu-id="58e69-213">This example shows the JSON schema for the annotated sandwich bot that is described in [Customize a form using FormBuilder](bot-builder-dotnet-formflow-formbuilder.md).</span></span>

```json
{
  "References": [ "Microsoft.Bot.Sample.AnnotatedSandwichBot.dll" ],
  "Imports": [ "Microsoft.Bot.Sample.AnnotatedSandwichBot.Resource" ],
  "type": "object",
  "required": [
    "Sandwich",
    "Length",
    "Ingredients",
    "DeliveryAddress"
  ],
  "Templates": {
    "NotUnderstood": {
      "Patterns": [ "I do not understand \"{0}\".", "Try again, I don't get \"{0}\"." ]
    },
    "EnumSelectOne": {
      "Patterns": [ "What kind of {&} would you like on your sandwich? {||}" ],
      "ChoiceStyle": "Auto"
    }
  },
  "properties": {
    "Sandwich": {
      "Prompt": { "Patterns": [ "What kind of {&} would you like? {||}" ] },
      "Before": [ { "Message": [ "Welcome to the sandwich order bot!" ] } ],
      "Describe": { "Image": "https://placeholdit.imgix.net/~text?txtsize=16&txt=Sandwich&w=125&h=40&txttrack=0&txtclr=000&txtfont=bold" },
      "type": [
        "string",
        "null"
      ],
      "enum": [
        "BLT",
        "BlackForestHam",
        "BuffaloChicken",
        "ChickenAndBaconRanchMelt",
        "ColdCutCombo",
        "MeatballMarinara",
        "OvenRoastedChicken",
        "RoastBeef",
        "RotisserieStyleChicken",
        "SpicyItalian",
        "SteakAndCheese",
        "SweetOnionTeriyaki",
        "Tuna",
        "TurkeyBreast",
        "Veggie"
      ],
      "Values": {
        "RotisserieStyleChicken": {
          "Terms": [ "rotis\\w* style chicken" ],
          "MaxPhrase": 3
        }
      }
    },
    "Length": {
      "Prompt": {
        "Patterns": [ "What size of sandwich do you want? {||}" ]
      },
      "type": [
        "string",
        "null"
      ],
      "enum": [
        "SixInch",
        "FootLong"
      ]
    },
    "Ingredients": {
      "type": "object",
      "required": [ "Bread" ],
      "properties": {
        "Bread": {
          "type": [
            "string",
            "null"
          ],
          "Describe": {
            "Title": "Sandwich Bot",
            "SubTitle": "Bread Picker"
          },
          "enum": [
            "NineGrainWheat",
            "NineGrainHoneyOat",
            "Italian",
            "ItalianHerbsAndCheese",
            "Flatbread"
          ]
        },
        "Cheese": {
          "type": [
            "string",
            "null"
          ],
          "enum": [
            "American",
            "MontereyCheddar",
            "Pepperjack"
          ]
        },
        "Toppings": {
          "type": "array",
          "items": {
            "type": "integer",
            "enum": [
              "Everything",
              "Avocado",
              "BananaPeppers",
              "Cucumbers",
              "GreenBellPeppers",
              "Jalapenos",
              "Lettuce",
              "Olives",
              "Pickles",
              "RedOnion",
              "Spinach",
              "Tomatoes"
            ],
            "Values": {
              "Everything": { "Terms": [ "except", "but", "not", "no", "all", "everything" ] }
            }
          },
          "Validate": "var values = ((List<object>) value).OfType<string>(); var result = new ValidateResult {IsValid = true, Value = values} ; if (values != null && values.Contains(\"Everything\")) { result.Value = (from topping in new string[] {  \"Avocado\", \"BananaPeppers\", \"Cucumbers\", \"GreenBellPeppers\", \"Jalapenos\", \"Lettuce\", \"Olives\", \"Pickles\", \"RedOnion\", \"Spinach\", \"Tomatoes\"} where !values.Contains(topping) select topping).ToList();} return result;",
          "After": [ { "Message": [ "For sandwich toppings you have selected {Ingredients.Toppings}." ] } ]
        },
        "Sauces": {
          "type": [
            "array",
            "null"
          ],
          "items": {
            "type": "string",
            "enum": [
              "ChipotleSouthwest",
              "HoneyMustard",
              "LightMayonnaise",
              "RegularMayonnaise",
              "Mustard",
              "Oil",
              "Pepper",
              "Ranch",
              "SweetOnion",
              "Vinegar"
            ]
          }
        }
      }
    },
    "Specials": {
      "Templates": {
        "NoPreference": { "Patterns": [ "None" ] }
      },
      "type": [
        "string",
        "null"
      ],
      "Active": "return (string) state[\"Length\"] == \"FootLong\";",
      "Define": "field.SetType(null).AddDescription(\"cookie\", DynamicSandwich.FreeCookie).AddTerms(\"cookie\", Language.GenerateTerms(DynamicSandwich.FreeCookie, 2)).AddDescription(\"drink\", DynamicSandwich.FreeDrink).AddTerms(\"drink\", Language.GenerateTerms(DynamicSandwich.FreeDrink, 2)); return true;",
      "After": [ { "Confirm": "var cost = 0.0; switch ((string) state[\"Length\"]) { case \"SixInch\": cost = 5.0; break; case \"FootLong\": cost=6.50; break;} return new PromptAttribute($\"Total for your sandwich is {cost:C2} is that ok?\");" } ]
    },
    "DeliveryAddress": {
      "type": [
        "string",
        "null"
      ],
      "Validate": "var result = new ValidateResult{ IsValid = true, Value = value}; var address = (value as string).Trim(); if (address.Length > 0 && (address[0] < '0' || address[0] > '9')) {result.Feedback = DynamicSandwich.BadAddress; result.IsValid = false; } return result;"
    },
    "PhoneNumber": {
      "type": [ "string", "null" ],
      "pattern": "(\\(\\d{3}\\))?\\s*\\d{3}(-|\\s*)\\d{4}"
    },
    "DeliveryTime": {
      "Templates": {
        "StatusFormat": {
          "Patterns": [ "{&}: {:t}" ],
          "FieldCase": "None"
        }
      },
      "DateTime": true,
      "type": [
        "string",
        "null"
      ],
      "After": [ { "Confirm": [ "Do you want to order your {Length} {Sandwich} on {Ingredients.Bread} {&Ingredients.Bread} with {[{Ingredients.Cheese} {Ingredients.Toppings} {Ingredients.Sauces} to be sent to {DeliveryAddress} {?at {DeliveryTime}}?" ] } ]
    },
    "Rating": {
      "Describe": "your experience today",
      "type": [
        "number",
        "null"
      ],
      "minimum": 1,
      "maximum": 5,
      "After": [ { "Message": [ "Thanks for ordering your sandwich!" ] } ]
    }
  },
  "OnCompletion": "await context.PostAsync(\"We are currently processing your sandwich. We will message you the status.\");"
}
```

## <a name="implement-formflow-with-json-schema"></a><span data-ttu-id="58e69-214">Implementación de FormFlow con esquema JSON</span><span class="sxs-lookup"><span data-stu-id="58e69-214">Implement FormFlow with JSON schema</span></span>

<span data-ttu-id="58e69-215">Para implementar FormFlow con un esquema JSON, use `FormBuilderJson`, que es compatible con la misma interfaz fluida que `FormBuilder`.</span><span class="sxs-lookup"><span data-stu-id="58e69-215">To implement FormFlow with a JSON schema, use `FormBuilderJson`, which supports the same fluent interface as `FormBuilder`.</span></span> <span data-ttu-id="58e69-216">En este ejemplo de código se muestra cómo implementar el esquema JSON de un bot anotado que se usa para encargar un sándwich que se ha descrito en [Personalización de un formulario mediante FormBuilder](bot-builder-dotnet-formflow-formbuilder.md).</span><span class="sxs-lookup"><span data-stu-id="58e69-216">This code example shows how to implement the JSON schema for the annotated sandwich bot that is described in [Customize a form using FormBuilder](bot-builder-dotnet-formflow-formbuilder.md).</span></span>

[!code-csharp[Use JSON schema](../includes/code/dotnet-formflow-json-schema.cs#useSchema)]

## <a name="sample-code"></a><span data-ttu-id="58e69-217">Código de ejemplo</span><span class="sxs-lookup"><span data-stu-id="58e69-217">Sample code</span></span>

[!INCLUDE [Sample code](../includes/snippet-dotnet-formflow-samples.md)]

## <a name="additional-resources"></a><span data-ttu-id="58e69-218">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="58e69-218">Additional resources</span></span>

- [<span data-ttu-id="58e69-219">Características básicas de FormFlow</span><span class="sxs-lookup"><span data-stu-id="58e69-219">Basic features of FormFlow</span></span>](bot-builder-dotnet-formflow.md)
- [<span data-ttu-id="58e69-220">Características avanzadas de FormFlow</span><span class="sxs-lookup"><span data-stu-id="58e69-220">Advanced features of FormFlow</span></span>](bot-builder-dotnet-formflow-advanced.md)
- [<span data-ttu-id="58e69-221">Personalización de un formulario mediante FormBuilder</span><span class="sxs-lookup"><span data-stu-id="58e69-221">Customize a form using FormBuilder</span></span>](bot-builder-dotnet-formflow-formbuilder.md)
- [<span data-ttu-id="58e69-222">Localización del contenido del formulario</span><span class="sxs-lookup"><span data-stu-id="58e69-222">Localize form content</span></span>](bot-builder-dotnet-formflow-localize.md)
- [<span data-ttu-id="58e69-223">Personalización de la experiencia de usuario con lenguaje de patrones</span><span class="sxs-lookup"><span data-stu-id="58e69-223">Customize user experience with pattern language</span></span>](bot-builder-dotnet-formflow-pattern-language.md)
- <span data-ttu-id="58e69-224"><a href="/dotnet/api/?view=botbuilder-3.11.0" target="_blank">Referencia de Bot Builder SDK para .NET</a></span><span class="sxs-lookup"><span data-stu-id="58e69-224"><a href="/dotnet/api/?view=botbuilder-3.11.0" target="_blank">Bot Builder SDK for .NET Reference</a></span></span>

[numericAttribute]: /dotnet/api/microsoft.bot.builder.formflow.numericattribute

[patternAttribute]: /dotnet/api/microsoft.bot.builder.formflow.patternattribute

[templateAttribute]: /dotnet/api/microsoft.bot.builder.formflow.templateattribute

[promptAttribute]: /dotnet/api/microsoft.bot.builder.formflow.promptattribute

[describeAttribute]: /dotnet/api/microsoft.bot.builder.formflow.describeattribute
