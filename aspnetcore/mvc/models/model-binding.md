---
title: Enlace de modelos
author: rick-anderson
description: 
keywords: "Núcleo de ASP.NET,"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: b355a48e-a15c-4d58-b69c-899763613a97
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/model-binding
ms.openlocfilehash: 930ea062ffb914cbd4f1500308b813167c1f601b
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2017
---
# <a name="model-binding"></a><span data-ttu-id="08067-103">Enlace de modelos</span><span class="sxs-lookup"><span data-stu-id="08067-103">Model Binding</span></span>

<span data-ttu-id="08067-104">Por [Rachel Appel](http://github.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="08067-104">By [Rachel Appel](http://github.com/rachelappel)</span></span>

## <a name="introduction-to-model-binding"></a><span data-ttu-id="08067-105">Introducción al modelo de enlace</span><span class="sxs-lookup"><span data-stu-id="08067-105">Introduction to model binding</span></span>

<span data-ttu-id="08067-106">Enlace de modelo en MVC de ASP.NET Core asigna datos de las solicitudes HTTP a los parámetros de método de acción.</span><span class="sxs-lookup"><span data-stu-id="08067-106">Model binding in ASP.NET Core MVC maps data from HTTP requests to action method parameters.</span></span> <span data-ttu-id="08067-107">Los parámetros pueden ser tipos simples como cadenas, enteros o flotantes, o pueden ser tipos complejos.</span><span class="sxs-lookup"><span data-stu-id="08067-107">The parameters may be simple types such as strings, integers, or floats, or they may be complex types.</span></span> <span data-ttu-id="08067-108">Se trata de una característica excelente de MVC porque la asignación de los datos entrantes a un equivalente es un escenario a menudo repetido, independientemente del tamaño o complejidad de los datos.</span><span class="sxs-lookup"><span data-stu-id="08067-108">This is a great feature of MVC because mapping incoming data to a counterpart is an often repeated scenario, regardless of size or complexity of the data.</span></span> <span data-ttu-id="08067-109">MVC soluciona este problema abstrayendo enlace para que los desarrolladores no tienen que mantener volver a escribir una versión ligeramente diferente de ese mismo código en cada aplicación.</span><span class="sxs-lookup"><span data-stu-id="08067-109">MVC solves this problem by abstracting binding away so developers don't have to keep rewriting a slightly different version of that same code in every app.</span></span> <span data-ttu-id="08067-110">Escribir su propio texto para escribir el código del convertidor es tedioso y es proclive a errores.</span><span class="sxs-lookup"><span data-stu-id="08067-110">Writing your own text to type converter code is tedious, and error prone.</span></span>

## <a name="how-model-binding-works"></a><span data-ttu-id="08067-111">Cómo funciona el enlace de modelo</span><span class="sxs-lookup"><span data-stu-id="08067-111">How model binding works</span></span>

<span data-ttu-id="08067-112">Cuando MVC recibe una solicitud HTTP, enruta a un método de acción específica de un controlador.</span><span class="sxs-lookup"><span data-stu-id="08067-112">When MVC receives an HTTP request, it routes it to a specific action method of a controller.</span></span> <span data-ttu-id="08067-113">Determina qué método de acción para ejecutar en función de lo que está en los datos de ruta, a continuación, enlaza los valores de la solicitud HTTP a parámetros de ese método de acción.</span><span class="sxs-lookup"><span data-stu-id="08067-113">It determines which action method to run based on what is in the route data, then it binds values from the HTTP request to that action method's parameters.</span></span> <span data-ttu-id="08067-114">Por ejemplo, considere la siguiente dirección URL:</span><span class="sxs-lookup"><span data-stu-id="08067-114">For example, consider the following URL:</span></span>

`http://contoso.com/movies/edit/2`

<span data-ttu-id="08067-115">Puesto que la plantilla de ruta tiene un aspecto parecido a éste, `{controller=Home}/{action=Index}/{id?}`, `movies/edit/2` enruta a la `Movies` controlador y su `Edit` método de acción.</span><span class="sxs-lookup"><span data-stu-id="08067-115">Since the route template looks like this, `{controller=Home}/{action=Index}/{id?}`, `movies/edit/2` routes to the `Movies` controller, and its `Edit` action method.</span></span> <span data-ttu-id="08067-116">También acepta un parámetro opcional denominado `id`.</span><span class="sxs-lookup"><span data-stu-id="08067-116">It also accepts an optional parameter called `id`.</span></span> <span data-ttu-id="08067-117">El código para el método de acción debe tener un aspecto similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="08067-117">The code for the action method should look something like this:</span></span>

<!-- literal_block {"ids": [], "linenos": true, "xml:space": "preserve", "language": "csharp"} -->

```csharp
public IActionResult Edit(int? id)
   ```

<span data-ttu-id="08067-118">Nota: Las cadenas de la ruta de dirección URL no distinguen mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="08067-118">Note: The strings in the URL route are not case sensitive.</span></span>

<span data-ttu-id="08067-119">MVC intentará enlazar los datos de la solicitud a los parámetros de acción por su nombre.</span><span class="sxs-lookup"><span data-stu-id="08067-119">MVC will try to bind request data to the action parameters by name.</span></span> <span data-ttu-id="08067-120">MVC buscará valores para cada parámetro con el nombre del parámetro y los nombres de sus propiedades públicas configurables.</span><span class="sxs-lookup"><span data-stu-id="08067-120">MVC will look for values for each parameter using the parameter name and the names of its public settable properties.</span></span> <span data-ttu-id="08067-121">En el ejemplo anterior, el parámetro de acción solo se denomina `id`, que MVC enlaza con el valor con el mismo nombre en los valores de ruta.</span><span class="sxs-lookup"><span data-stu-id="08067-121">In the above example, the only action parameter is named `id`, which MVC binds to the value with the same name in the route values.</span></span> <span data-ttu-id="08067-122">Además de los valores de ruta MVC enlazará los datos de varias partes de la solicitud y lo hace en un orden establecido.</span><span class="sxs-lookup"><span data-stu-id="08067-122">In addition to route values MVC will bind data from various parts of the request and it does so in a set order.</span></span> <span data-ttu-id="08067-123">A continuación se muestra una lista de los orígenes de datos en el orden en que se busca el enlace de modelos a través de ellos:</span><span class="sxs-lookup"><span data-stu-id="08067-123">Below is a list of the data sources in the order that model binding looks through them:</span></span>

1. <span data-ttu-id="08067-124">`Form values`: Estos son los valores de formulario que van en la solicitud HTTP que utilizan el método POST.</span><span class="sxs-lookup"><span data-stu-id="08067-124">`Form values`: These are form values that go in the HTTP request using the POST method.</span></span> <span data-ttu-id="08067-125">(incluidas las solicitudes POST de jQuery).</span><span class="sxs-lookup"><span data-stu-id="08067-125">(including jQuery POST requests).</span></span>

2. <span data-ttu-id="08067-126">`Route values`: El conjunto de valores de la ruta proporcionada por [enrutamiento](../../fundamentals/routing.md)</span><span class="sxs-lookup"><span data-stu-id="08067-126">`Route values`: The set of route values provided by [Routing](../../fundamentals/routing.md)</span></span>

3. <span data-ttu-id="08067-127">`Query strings`: La parte de la cadena de consulta del URI.</span><span class="sxs-lookup"><span data-stu-id="08067-127">`Query strings`: The query string part of the URI.</span></span>

<!-- DocFX BUG
The link works but generates an error when building with DocFX
@fundamentals/routing
[Routing](xref:fundamentals/routing)
-->

<span data-ttu-id="08067-128">Nota: Formulario valores, enrutar los datos y cadenas se almacenan como pares de nombre y valor de consulta.</span><span class="sxs-lookup"><span data-stu-id="08067-128">Note: Form values, route data, and query strings are all stored as name-value pairs.</span></span>

<span data-ttu-id="08067-129">Puesto que el enlace de modelos pedirá una clave denominada `id` y no hay nada denominado `id` en los valores del formulario, mueven en para los valores de ruta busca esa clave.</span><span class="sxs-lookup"><span data-stu-id="08067-129">Since model binding asked for a key named `id` and there is nothing named `id` in the form values, it moved on to the route values looking for that key.</span></span> <span data-ttu-id="08067-130">En nuestro ejemplo, es una coincidencia.</span><span class="sxs-lookup"><span data-stu-id="08067-130">In our example, it's a match.</span></span> <span data-ttu-id="08067-131">Se produce el enlace y el valor se convierte en un entero de 2.</span><span class="sxs-lookup"><span data-stu-id="08067-131">Binding happens, and the value is converted to the integer 2.</span></span> <span data-ttu-id="08067-132">La misma solicitud mediante la edición (Id. de la cadena) se convertiría en la cadena "2".</span><span class="sxs-lookup"><span data-stu-id="08067-132">The same request using Edit(string id) would convert to the string "2".</span></span>

<span data-ttu-id="08067-133">Hasta ahora en el ejemplo se utiliza tipos simples.</span><span class="sxs-lookup"><span data-stu-id="08067-133">So far the example uses simple types.</span></span> <span data-ttu-id="08067-134">En MVC tipos simples son cualquier tipo primitivo de .NET o con un convertidor de tipos de cadena.</span><span class="sxs-lookup"><span data-stu-id="08067-134">In MVC simple types are any .NET primitive type or type with a string type converter.</span></span> <span data-ttu-id="08067-135">Si el parámetro del método de acción fuera una clase como el `Movie` tipo, que contiene tipos simples y complejos, como propiedades, modelo enlace seguirán del MVC controlen correctamente.</span><span class="sxs-lookup"><span data-stu-id="08067-135">If the action method's parameter were a class such as the `Movie` type, which contains both simple and complex types as properties, MVC's model binding will still handle it nicely.</span></span> <span data-ttu-id="08067-136">Usa reflexión y recursividad para recorrer las propiedades de tipos complejos buscando coincidencias.</span><span class="sxs-lookup"><span data-stu-id="08067-136">It uses reflection and recursion to traverse the properties of complex types looking for matches.</span></span> <span data-ttu-id="08067-137">Enlace de modelos busca el patrón *parameter_name.property_name* para enlazar los valores a las propiedades.</span><span class="sxs-lookup"><span data-stu-id="08067-137">Model binding looks for the pattern *parameter_name.property_name* to bind values to properties.</span></span> <span data-ttu-id="08067-138">Si no encuentra los valores correspondientes de esta forma, intentará enlazar con el nombre de propiedad.</span><span class="sxs-lookup"><span data-stu-id="08067-138">If it doesn't find matching values of this form, it will attempt to bind using just the property name.</span></span> <span data-ttu-id="08067-139">Para esos tipos como `Collection` tipos, el enlace de modelos busca las coincidencias a *parameter_name [índice]* o simplemente *[índice]*.</span><span class="sxs-lookup"><span data-stu-id="08067-139">For those types such as `Collection` types, model binding looks for matches to *parameter_name[index]* or just *[index]*.</span></span> <span data-ttu-id="08067-140">Trata de enlace de modelo `Dictionary` del mismo modo, los tipos piden *parameter_name [key]* o simplemente *[key]*, siempre que las claves son tipos simples.</span><span class="sxs-lookup"><span data-stu-id="08067-140">Model binding treats  `Dictionary` types similarly, asking for *parameter_name[key]* or just *[key]*, as long as the keys are simple types.</span></span> <span data-ttu-id="08067-141">Las claves que se admiten coincide con los nombres de campo HTML y aplicaciones auxiliares de etiquetas generadas para el mismo tipo de modelo.</span><span class="sxs-lookup"><span data-stu-id="08067-141">Keys that are supported match the field names HTML and tag helpers generated for the same model type.</span></span> <span data-ttu-id="08067-142">Esto permite que los valores de ida y vuelta para que los campos de formulario se mantienen repleto de la entrada del usuario para su comodidad, por ejemplo, cuando los datos enlazados de una creación o edición no superan la validación.</span><span class="sxs-lookup"><span data-stu-id="08067-142">This enables round-tripping values so that the form fields remain filled with the user's input for their convenience, for example, when bound data from a create or edit did not pass validation.</span></span>

<span data-ttu-id="08067-143">En el orden de enlace que se produzca la clase debe tener un constructor público predeterminado y miembro esté enlazado debe ser propiedades públicas de escritura.</span><span class="sxs-lookup"><span data-stu-id="08067-143">In order for binding to happen the class must have a public default constructor and member to be bound must be public writable properties.</span></span> <span data-ttu-id="08067-144">Cuando se produzca el enlazado de modelo que solo se crearán instancias de la clase utilizando el constructor predeterminado público, pueden establecer las propiedades.</span><span class="sxs-lookup"><span data-stu-id="08067-144">When model binding happens the class will only be instantiated using the public default constructor, then the properties can be set.</span></span>

<span data-ttu-id="08067-145">Cuando se enlaza un parámetro, el enlace de modelos detiene buscando valores con ese nombre y lo pasa a enlazar el parámetro siguiente.</span><span class="sxs-lookup"><span data-stu-id="08067-145">When a parameter is bound, model binding stops looking for values with that name and it moves on to bind the next parameter.</span></span> <span data-ttu-id="08067-146">Si se produce un error de enlace, MVC no produce un error.</span><span class="sxs-lookup"><span data-stu-id="08067-146">If binding fails, MVC does not throw an error.</span></span> <span data-ttu-id="08067-147">Puede consultar los errores de estado del modelo mediante la comprobación de la `ModelState.IsValid` propiedad.</span><span class="sxs-lookup"><span data-stu-id="08067-147">You can query for model state errors by checking the `ModelState.IsValid` property.</span></span>

<span data-ttu-id="08067-148">Nota: Cada entrada en el controlador `ModelState` propiedad es una `ModelStateEntry` que contiene un `Errors property`.</span><span class="sxs-lookup"><span data-stu-id="08067-148">Note: Each entry in the controller's `ModelState` property is a `ModelStateEntry` containing an `Errors property`.</span></span> <span data-ttu-id="08067-149">Es no suele ser necesario consultar esta colección usted mismo.</span><span class="sxs-lookup"><span data-stu-id="08067-149">It's rarely necessary to query this collection yourself.</span></span> <span data-ttu-id="08067-150">Utilice `ModelState.IsValid` en su lugar.</span><span class="sxs-lookup"><span data-stu-id="08067-150">Use `ModelState.IsValid` instead.</span></span>

<span data-ttu-id="08067-151">Además, hay algunos tipos de datos especiales que MVC debe tener en cuenta al realizar el enlace de modelos:</span><span class="sxs-lookup"><span data-stu-id="08067-151">Additionally, there are some special data types that MVC must consider when performing model binding:</span></span>

* <span data-ttu-id="08067-152">`IFormFile`, `IEnumerable<IFormFile>`: Uno o más archivos cargados que forman parte de la solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="08067-152">`IFormFile`, `IEnumerable<IFormFile>`: One or more uploaded files that are part of the HTTP request.</span></span>

* <span data-ttu-id="08067-153">`CancelationToken`: Se utiliza para cancelar la actividad en controladores asincrónicos.</span><span class="sxs-lookup"><span data-stu-id="08067-153">`CancelationToken`: Used to cancel activity in asynchronous controllers.</span></span>

<span data-ttu-id="08067-154">Estos tipos se pueden enlazar a los parámetros de acción o a las propiedades en un tipo de clase.</span><span class="sxs-lookup"><span data-stu-id="08067-154">These types can be bound to action parameters or to properties on a class type.</span></span>

<span data-ttu-id="08067-155">Cuando se complete, el enlace de modelos [validación](validation.md) se produce.</span><span class="sxs-lookup"><span data-stu-id="08067-155">Once model binding is complete, [Validation](validation.md) occurs.</span></span> <span data-ttu-id="08067-156">Enlace de modelo predeterminado funciona muy bien para la mayoría de escenarios de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="08067-156">Default model binding works great for the vast majority of development scenarios.</span></span> <span data-ttu-id="08067-157">También es extensible para que si tiene necesidades únicas que pueda personalizar el comportamiento integrado.</span><span class="sxs-lookup"><span data-stu-id="08067-157">It is also extensible so if you have unique needs you can customize the built-in behavior.</span></span>

## <a name="customize-model-binding-behavior-with-attributes"></a><span data-ttu-id="08067-158">Personalizar el comportamiento de enlace de modelo con atributos</span><span class="sxs-lookup"><span data-stu-id="08067-158">Customize model binding behavior with attributes</span></span>

<span data-ttu-id="08067-159">MVC contiene varios atributos que puede usar para dirigir su comportamiento de enlace de modelo predeterminado para un origen diferente.</span><span class="sxs-lookup"><span data-stu-id="08067-159">MVC contains several attributes that you can use to direct its default model binding behavior to a different source.</span></span> <span data-ttu-id="08067-160">Por ejemplo, puede especificar si se requiere para una propiedad de enlace, o si nunca debe ocurrir en absoluto mediante el uso de la `[BindRequired]` o `[BindNever]` atributos.</span><span class="sxs-lookup"><span data-stu-id="08067-160">For example, you can specify whether binding is required for a property, or if it should never happen at all by using the `[BindRequired]` or `[BindNever]` attributes.</span></span> <span data-ttu-id="08067-161">Como alternativa, puede reemplazar el origen de datos predeterminado y especificar el origen de datos del enlazador de modelos.</span><span class="sxs-lookup"><span data-stu-id="08067-161">Alternatively, you can override the default data source, and specify the model binder's data source.</span></span> <span data-ttu-id="08067-162">A continuación se muestra una lista de atributos de enlace de modelo:</span><span class="sxs-lookup"><span data-stu-id="08067-162">Below is a list of model binding attributes:</span></span>

* <span data-ttu-id="08067-163">`[BindRequired]`: Este atributo agrega un error de estado de modelo si no se puede producir el enlace.</span><span class="sxs-lookup"><span data-stu-id="08067-163">`[BindRequired]`: This attribute adds a model state error if binding cannot occur.</span></span>

* <span data-ttu-id="08067-164">`[BindNever]`: Indica el enlazador de modelos para nunca se enlazan con este parámetro.</span><span class="sxs-lookup"><span data-stu-id="08067-164">`[BindNever]`: Tells the model binder to never bind to this parameter.</span></span>

* <span data-ttu-id="08067-165">`[FromHeader]`, `[FromQuery]`, `[FromRoute]`, `[FromForm]`: Utilícelos para especificar el origen de enlace exacto que desea aplicar.</span><span class="sxs-lookup"><span data-stu-id="08067-165">`[FromHeader]`, `[FromQuery]`, `[FromRoute]`, `[FromForm]`: Use these to specify the exact binding source you want to apply.</span></span>

* <span data-ttu-id="08067-166">`[FromServices]`: Este atributo usa [inyección de dependencia](../../fundamentals/dependency-injection.md) para enlazar parámetros de servicios.</span><span class="sxs-lookup"><span data-stu-id="08067-166">`[FromServices]`: This attribute uses [dependency injection](../../fundamentals/dependency-injection.md) to bind parameters from services.</span></span>

* <span data-ttu-id="08067-167">`[FromBody]`: Use los formateadores configurados para enlazar datos desde el cuerpo de solicitud.</span><span class="sxs-lookup"><span data-stu-id="08067-167">`[FromBody]`: Use the configured formatters to bind data from the request body.</span></span> <span data-ttu-id="08067-168">El formateador se selecciona basándose en el tipo de contenido de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="08067-168">The formatter is selected based on content type of the request.</span></span>

* <span data-ttu-id="08067-169">`[ModelBinder]`: Se utiliza para reemplazar el enlazador de modelos predeterminado, el origen de enlace y el nombre.</span><span class="sxs-lookup"><span data-stu-id="08067-169">`[ModelBinder]`: Used to override the default model binder, binding source and name.</span></span>

<span data-ttu-id="08067-170">Atributos son herramientas muy útiles cuando es necesario invalidar el comportamiento predeterminado del enlace del modelo.</span><span class="sxs-lookup"><span data-stu-id="08067-170">Attributes are very helpful tools when you need to override the default behavior of model binding.</span></span>

## <a name="binding-formatted-data-from-the-request-body"></a><span data-ttu-id="08067-171">Enlace de datos con formato del cuerpo de la solicitud</span><span class="sxs-lookup"><span data-stu-id="08067-171">Binding formatted data from the request body</span></span>

<span data-ttu-id="08067-172">Datos de la solicitud pueden proceder de una variedad de formatos como JSON, XML y muchos otros.</span><span class="sxs-lookup"><span data-stu-id="08067-172">Request data can come in a variety of formats including JSON, XML and many others.</span></span> <span data-ttu-id="08067-173">Cuando se utiliza el atributo [FromBody] para indicar que desea enlazar un parámetro a los datos en el cuerpo de solicitud, MVC usa un conjunto de formateadores configurado para controlar los datos de solicitud en función de su tipo de contenido.</span><span class="sxs-lookup"><span data-stu-id="08067-173">When you use the [FromBody] attribute to indicate that you want to bind a parameter to data in the request body, MVC uses a configured set of formatters to handle the request data based on its content type.</span></span> <span data-ttu-id="08067-174">De forma predeterminada, MVC incluye un `JsonInputFormatter` la clase para controlar datos JSON, pero puede agregar formateadores adicionales para el tratamiento de XML y otros formatos personalizados.</span><span class="sxs-lookup"><span data-stu-id="08067-174">By default MVC includes a `JsonInputFormatter` class for handling JSON data, but you can add additional formatters for handling XML and other custom formats.</span></span>

> [!NOTE]
> <span data-ttu-id="08067-175">Puede haber a lo sumo un parámetro por cada acción decorada con `[FromBody]`.</span><span class="sxs-lookup"><span data-stu-id="08067-175">There can be at most one parameter per action decorated with `[FromBody]`.</span></span> <span data-ttu-id="08067-176">El tiempo de ejecución de MVC de ASP.NET Core delega la responsabilidad de leer la secuencia de solicitud en el formateador.</span><span class="sxs-lookup"><span data-stu-id="08067-176">The ASP.NET Core MVC run-time delegates the responsibility of reading the request stream to the formatter.</span></span> <span data-ttu-id="08067-177">Una vez que se lee la secuencia de solicitud para un parámetro, por lo general no es posible leer la secuencia de solicitud nuevo para otro enlace `[FromBody]` parámetros.</span><span class="sxs-lookup"><span data-stu-id="08067-177">Once the request stream is read for a parameter, it's generally not possible to read the request stream again for binding other `[FromBody]` parameters.</span></span>

> [!NOTE]
> <span data-ttu-id="08067-178">El `JsonInputFormatter` es el formateador predeterminado y se basa en [Json.NET](http://www.newtonsoft.com/json).</span><span class="sxs-lookup"><span data-stu-id="08067-178">The `JsonInputFormatter` is the default formatter and is based on [Json.NET](http://www.newtonsoft.com/json).</span></span>

<span data-ttu-id="08067-179">ASP.NET selecciona entradas formateadores tomando como base la [Content-Type](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html) encabezado y el tipo del parámetro, a menos que haya un atributo aplicado a especificando en caso contrario.</span><span class="sxs-lookup"><span data-stu-id="08067-179">ASP.NET selects input formatters based on the [Content-Type](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html) header and the type of the parameter, unless there is an attribute applied to it specifying otherwise.</span></span> <span data-ttu-id="08067-180">Si le gustaría usar XML u otro formato, debe configurarla en el *Startup.cs* archivo, pero primero tiene que obtener una referencia a `Microsoft.AspNetCore.Mvc.Formatters.Xml` mediante NuGet.</span><span class="sxs-lookup"><span data-stu-id="08067-180">If you'd like to use XML or another format you must configure it in the *Startup.cs* file, but you may first have to obtain a reference to `Microsoft.AspNetCore.Mvc.Formatters.Xml` using NuGet.</span></span> <span data-ttu-id="08067-181">El código de inicio debe tener un aspecto similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="08067-181">Your startup code should look something like this:</span></span>

<!-- literal_block {"ids": [], "linenos": true, "xml:space": "preserve", "language": "csharp"} -->

```csharp
public void ConfigureServices(IServiceCollection services)
   {
       services.AddMvc()
          .AddXmlSerializerFormatters();
   }
   ```

<span data-ttu-id="08067-182">El código de la *Startup.cs* archivo contiene un `ConfigureServices` método con un `services` argumento puede utilizar para generar los servicios para la aplicación ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="08067-182">Code in the *Startup.cs* file contains a `ConfigureServices` method with a `services` argument you can use to build up services for your ASP.NET app.</span></span> <span data-ttu-id="08067-183">En el ejemplo, vamos a agregar a un formateador XML como un servicio que MVC proporcione para esta aplicación.</span><span class="sxs-lookup"><span data-stu-id="08067-183">In the sample, we are adding an XML formatter as a service that MVC will provide for this app.</span></span> <span data-ttu-id="08067-184">El `options` argumento pasado a la `AddMvc` método le permite agregar y administrar filtros, los formateadores y otras opciones de sistema de MVC al inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="08067-184">The `options` argument passed into the `AddMvc` method allows you to add and manage filters, formatters, and other system options from MVC upon app startup.</span></span> <span data-ttu-id="08067-185">A continuación, aplique la `Consumes` atributo a las clases de controlador o los métodos de acción para que funcione con el formato que desee.</span><span class="sxs-lookup"><span data-stu-id="08067-185">Then apply the `Consumes` attribute to controller classes or action methods to work with the format you want.</span></span>

### <a name="custom-model-binding"></a><span data-ttu-id="08067-186">Enlace de modelos personalizados</span><span class="sxs-lookup"><span data-stu-id="08067-186">Custom Model Binding</span></span>

<span data-ttu-id="08067-187">Puede ampliar el enlace de modelos escribiendo sus propio enlazadores de modelos personalizados.</span><span class="sxs-lookup"><span data-stu-id="08067-187">You can extend model binding by writing your own custom model binders.</span></span> <span data-ttu-id="08067-188">Obtenga más información sobre [enlace de modelos personalizados](../advanced/custom-model-binding.md).</span><span class="sxs-lookup"><span data-stu-id="08067-188">Learn more about [custom model binding](../advanced/custom-model-binding.md).</span></span>