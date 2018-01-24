---
title: "Solicitudes de administración con los controladores de MVC de ASP.NET Core"
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 07/03/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/actions
ms.openlocfilehash: cef493fc2010d1c82e5c1dfec85864539252b817
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
# <a name="handling-requests-with-controllers-in-aspnet-core-mvc"></a><span data-ttu-id="cd565-102">Solicitudes de administración con los controladores de MVC de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cd565-102">Handling requests with controllers in ASP.NET Core MVC</span></span>

<span data-ttu-id="cd565-103">Por [Steve Smith](https://ardalis.com/) y [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="cd565-103">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="cd565-104">Controladores, acciones y resultados de la acción son una parte fundamental de cómo los desarrolladores crear aplicaciones con MVC de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="cd565-104">Controllers, actions, and action results are a fundamental part of how developers build apps using ASP.NET Core MVC.</span></span>

## <a name="what-is-a-controller"></a><span data-ttu-id="cd565-105">¿Qué es un controlador?</span><span class="sxs-lookup"><span data-stu-id="cd565-105">What is a Controller?</span></span>

<span data-ttu-id="cd565-106">Un controlador se usa para definir y agrupar un conjunto de acciones.</span><span class="sxs-lookup"><span data-stu-id="cd565-106">A controller is used to define and group a set of actions.</span></span> <span data-ttu-id="cd565-107">Una acción (o *método de acción*) es un método en un controlador que administra las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="cd565-107">An action (or *action method*) is a method on a controller which handles requests.</span></span> <span data-ttu-id="cd565-108">Controladores de agrupan lógicamente acciones similares.</span><span class="sxs-lookup"><span data-stu-id="cd565-108">Controllers logically group similar actions together.</span></span> <span data-ttu-id="cd565-109">Esta agregación de acciones permite comunes conjuntos de reglas, como el enrutamiento, el almacenamiento en caché y autorización, que se aplicará de forma conjunta.</span><span class="sxs-lookup"><span data-stu-id="cd565-109">This aggregation of actions allows common sets of rules, such as routing, caching, and authorization, to be applied collectively.</span></span> <span data-ttu-id="cd565-110">Las solicitudes se asignan a las acciones a través de [enrutamiento](xref:mvc/controllers/routing).</span><span class="sxs-lookup"><span data-stu-id="cd565-110">Requests are mapped to actions through [routing](xref:mvc/controllers/routing).</span></span>

<span data-ttu-id="cd565-111">Por convención, las clases de controlador:</span><span class="sxs-lookup"><span data-stu-id="cd565-111">By convention, controller classes:</span></span>
* <span data-ttu-id="cd565-112">Residen en el nivel de raíz del proyecto *controladores* carpeta</span><span class="sxs-lookup"><span data-stu-id="cd565-112">Reside in the project's root-level *Controllers* folder</span></span>
* <span data-ttu-id="cd565-113">Heredar de`Microsoft.AspNetCore.Mvc.Controller`</span><span class="sxs-lookup"><span data-stu-id="cd565-113">Inherit from `Microsoft.AspNetCore.Mvc.Controller`</span></span>

<span data-ttu-id="cd565-114">Un controlador es una clase se pueden crear instancias en las que al menos una de las siguientes condiciones es true:</span><span class="sxs-lookup"><span data-stu-id="cd565-114">A controller is an instantiable class in which at least one of the following conditions is true:</span></span>
* <span data-ttu-id="cd565-115">El nombre de clase se utiliza como sufijo "Controller"</span><span class="sxs-lookup"><span data-stu-id="cd565-115">The class name is suffixed with "Controller"</span></span>
* <span data-ttu-id="cd565-116">La clase hereda de una clase cuyo nombre lleva el sufijo "Controller"</span><span class="sxs-lookup"><span data-stu-id="cd565-116">The class inherits from a class whose name is suffixed with "Controller"</span></span>
* <span data-ttu-id="cd565-117">La clase se decora con el `[Controller]` atributo</span><span class="sxs-lookup"><span data-stu-id="cd565-117">The class is decorated with the `[Controller]` attribute</span></span>

<span data-ttu-id="cd565-118">Una clase de controlador no debe tener asociado un `[NonController]` atributo.</span><span class="sxs-lookup"><span data-stu-id="cd565-118">A controller class must not have an associated `[NonController]` attribute.</span></span>

<span data-ttu-id="cd565-119">Los controladores deben seguir la [principio de dependencias explícitas](http://deviq.com/explicit-dependencies-principle/).</span><span class="sxs-lookup"><span data-stu-id="cd565-119">Controllers should follow the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/).</span></span> <span data-ttu-id="cd565-120">Existen dos enfoques para implementar este principio.</span><span class="sxs-lookup"><span data-stu-id="cd565-120">There are a couple approaches to implementing this principle.</span></span> <span data-ttu-id="cd565-121">Si varias acciones de controlador requieren el mismo servicio, considere la posibilidad de usar [inyección de constructor](xref:mvc/controllers/dependency-injection#constructor-injection) para solicitar esas dependencias.</span><span class="sxs-lookup"><span data-stu-id="cd565-121">If multiple controller actions require the same service, consider using [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) to request those dependencies.</span></span> <span data-ttu-id="cd565-122">Si el servicio es necesario sólo un método de acción única, considere la posibilidad de usar [acción inyección](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices) para solicitar la dependencia.</span><span class="sxs-lookup"><span data-stu-id="cd565-122">If the service is needed by only a single action method, consider using [Action Injection](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices) to request the dependency.</span></span>

<span data-ttu-id="cd565-123">En el **M**odelo -**V**er -**C**ontroller patrón, un controlador es responsable de la creación de instancias del modelo y el procesamiento inicial de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="cd565-123">Within the **M**odel-**V**iew-**C**ontroller pattern, a controller is responsible for the initial processing of the request and instantiation of the model.</span></span> <span data-ttu-id="cd565-124">Por lo general, las decisiones empresariales deben realizarse dentro del modelo.</span><span class="sxs-lookup"><span data-stu-id="cd565-124">Generally, business decisions should be performed within the model.</span></span>

<span data-ttu-id="cd565-125">El controlador toma el resultado de que el modelo de procesamiento (si existe) y devuelve la vista correcta y sus datos de vista asociada o el resultado de la llamada API.</span><span class="sxs-lookup"><span data-stu-id="cd565-125">The controller takes the result of the model's processing (if any) and returns either the proper view and its associated view data or the result of the API call.</span></span> <span data-ttu-id="cd565-126">Obtenga más información en [información general de ASP.NET MVC Core](xref:mvc/overview) y [Introducción a ASP.NET MVC de núcleo y Visual Studio](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="cd565-126">Learn more at [Overview of ASP.NET Core MVC](xref:mvc/overview) and [Getting started with ASP.NET Core MVC and Visual Studio](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="cd565-127">El controlador es un *nivel de interfaz de usuario* abstracción.</span><span class="sxs-lookup"><span data-stu-id="cd565-127">The controller is a *UI-level* abstraction.</span></span> <span data-ttu-id="cd565-128">Sus responsabilidades son para asegurarse de datos de la solicitud están válidas y elegir qué vista (o el resultado de una API) se debe devolver.</span><span class="sxs-lookup"><span data-stu-id="cd565-128">Its responsibilities are to ensure request data is valid and to choose which view (or result for an API) should be returned.</span></span> <span data-ttu-id="cd565-129">En las aplicaciones factorizadas correctamente, no incluyen directamente datos acceso o la lógica empresarial.</span><span class="sxs-lookup"><span data-stu-id="cd565-129">In well-factored apps, it does not directly include data access or business logic.</span></span> <span data-ttu-id="cd565-130">En su lugar, el controlador se delega a estas responsabilidades de control de servicios.</span><span class="sxs-lookup"><span data-stu-id="cd565-130">Instead, the controller delegates to services handling these responsibilities.</span></span>

## <a name="defining-actions"></a><span data-ttu-id="cd565-131">Definir acciones</span><span class="sxs-lookup"><span data-stu-id="cd565-131">Defining Actions</span></span>

<span data-ttu-id="cd565-132">Métodos públicos en un controlador, excepto las decorada con el `[NonAction]` de atributo, se muestran algunas acciones.</span><span class="sxs-lookup"><span data-stu-id="cd565-132">Public methods on a controller, except those decorated with the `[NonAction]` attribute, are actions.</span></span> <span data-ttu-id="cd565-133">Parámetros de acciones están enlazados a datos de la solicitud y se validan mediante [enlace de modelo](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="cd565-133">Parameters on actions are bound to request data and are validated using [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="cd565-134">Se produce la validación de modelo para todo lo que está enlazada a un modelo.</span><span class="sxs-lookup"><span data-stu-id="cd565-134">Model validation occurs for everything that's model-bound.</span></span> <span data-ttu-id="cd565-135">La `ModelState.IsValid` valor de la propiedad indica si el enlace de modelos y la validación se realizó correctamente.</span><span class="sxs-lookup"><span data-stu-id="cd565-135">The `ModelState.IsValid` property value indicates whether model binding and validation succeeded.</span></span>

<span data-ttu-id="cd565-136">Métodos de acción deben contener lógica para asignar una solicitud a una entidad de negocio.</span><span class="sxs-lookup"><span data-stu-id="cd565-136">Action methods should contain logic for mapping a request to a business concern.</span></span> <span data-ttu-id="cd565-137">Normalmente se deben representar problemas empresariales como los servicios que el controlador tiene acceso a través de [inyección de dependencia](xref:mvc/controllers/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="cd565-137">Business concerns should typically be represented as services that the controller accesses through [dependency injection](xref:mvc/controllers/dependency-injection).</span></span> <span data-ttu-id="cd565-138">Acciones, a continuación, asignan el resultado de la acción de negocios en un estado de aplicación.</span><span class="sxs-lookup"><span data-stu-id="cd565-138">Actions then map the result of the business action to an application state.</span></span>

<span data-ttu-id="cd565-139">Acciones pueden devolver nada, pero con frecuencia devolver una instancia de `IActionResult` (o `Task<IActionResult>` de los métodos asincrónicos) que genera una respuesta.</span><span class="sxs-lookup"><span data-stu-id="cd565-139">Actions can return anything, but frequently return an instance of `IActionResult` (or `Task<IActionResult>` for async methods) that produces a response.</span></span> <span data-ttu-id="cd565-140">El método de acción es responsable de elegir *qué tipo de respuesta*.</span><span class="sxs-lookup"><span data-stu-id="cd565-140">The action method is responsible for choosing *what kind of response*.</span></span> <span data-ttu-id="cd565-141">Resultado de la acción *el responde*.</span><span class="sxs-lookup"><span data-stu-id="cd565-141">The action result *does the responding*.</span></span>

### <a name="controller-helper-methods"></a><span data-ttu-id="cd565-142">Métodos de aplicación auxiliar de controlador</span><span class="sxs-lookup"><span data-stu-id="cd565-142">Controller Helper Methods</span></span>

<span data-ttu-id="cd565-143">Controladores normalmente se heredan de [controlador](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller), aunque esto no es necesario.</span><span class="sxs-lookup"><span data-stu-id="cd565-143">Controllers usually inherit from [Controller](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller), although this is not required.</span></span> <span data-ttu-id="cd565-144">Derivar de `Controller` proporciona acceso a tres categorías de métodos auxiliares:</span><span class="sxs-lookup"><span data-stu-id="cd565-144">Deriving from `Controller` provides access to three categories of helper methods:</span></span>

#### <a name="1-methods-resulting-in-an-empty-response-body"></a><span data-ttu-id="cd565-145">1. Métodos resultante en un cuerpo de respuesta vacío</span><span class="sxs-lookup"><span data-stu-id="cd565-145">1. Methods resulting in an empty response body</span></span>

<span data-ttu-id="cd565-146">Ya no `Content-Type` encabezado de respuesta HTTP se incluye, puesto que el cuerpo de respuesta no tiene contenido para describir.</span><span class="sxs-lookup"><span data-stu-id="cd565-146">No `Content-Type` HTTP response header is included, since the response body lacks content to describe.</span></span>

<span data-ttu-id="cd565-147">Hay dos tipos de resultado de esta categoría: redireccionamiento y código de estado HTTP.</span><span class="sxs-lookup"><span data-stu-id="cd565-147">There are two result types within this category: Redirect and HTTP Status Code.</span></span>

* <span data-ttu-id="cd565-148">**Código de estado HTTP**</span><span class="sxs-lookup"><span data-stu-id="cd565-148">**HTTP Status Code**</span></span>

    <span data-ttu-id="cd565-149">Este tipo devuelve un código de estado HTTP.</span><span class="sxs-lookup"><span data-stu-id="cd565-149">This type returns an HTTP status code.</span></span> <span data-ttu-id="cd565-150">Dos métodos auxiliares de este tipo son `BadRequest`, `NotFound`, y `Ok`.</span><span class="sxs-lookup"><span data-stu-id="cd565-150">A couple helper methods of this type are `BadRequest`, `NotFound`, and `Ok`.</span></span> <span data-ttu-id="cd565-151">Por ejemplo, `return BadRequest();` genera un código de 400 estado cuando se ejecuta.</span><span class="sxs-lookup"><span data-stu-id="cd565-151">For example, `return BadRequest();` produces a 400 status code when executed.</span></span> <span data-ttu-id="cd565-152">Cuando los métodos como `BadRequest`, `NotFound`, y `Ok` están sobrecargados, ya no se consideran los servicios de respuesta de código de estado HTTP, puesto que la negociación de contenido lleva a cabo.</span><span class="sxs-lookup"><span data-stu-id="cd565-152">When methods such as `BadRequest`, `NotFound`, and `Ok` are overloaded, they no longer qualify as HTTP Status Code responders, since content negotiation is taking place.</span></span>

* <span data-ttu-id="cd565-153">**Redirect**</span><span class="sxs-lookup"><span data-stu-id="cd565-153">**Redirect**</span></span>

    <span data-ttu-id="cd565-154">Este tipo, devuelve una redirección a una acción o destino (mediante `Redirect`, `LocalRedirect`, `RedirectToAction`, o `RedirectToRoute`).</span><span class="sxs-lookup"><span data-stu-id="cd565-154">This type returns a redirect to an action or destination (using `Redirect`, `LocalRedirect`, `RedirectToAction`, or `RedirectToRoute`).</span></span> <span data-ttu-id="cd565-155">Por ejemplo, `return RedirectToAction("Complete", new {id = 123});` redirige a `Complete`, pasando un objeto anónimo.</span><span class="sxs-lookup"><span data-stu-id="cd565-155">For example, `return RedirectToAction("Complete", new {id = 123});` redirects to `Complete`, passing an anonymous object.</span></span>

    <span data-ttu-id="cd565-156">El tipo de resultado de redirección difiere del tipo de código de estado HTTP principalmente en la adición de un `Location` encabezado de respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="cd565-156">The Redirect result type differs from the HTTP Status Code type primarily in the addition of a `Location` HTTP response header.</span></span>

#### <a name="2-methods-resulting-in-a-non-empty-response-body-with-a-predefined-content-type"></a><span data-ttu-id="cd565-157">2. Métodos resultante en el cuerpo de respuesta no vacía con un tipo de contenido predefinido</span><span class="sxs-lookup"><span data-stu-id="cd565-157">2. Methods resulting in a non-empty response body with a predefined content type</span></span>

<span data-ttu-id="cd565-158">Mayoría de los métodos auxiliares de esta categoría incluye una `ContentType` propiedad, lo que le permite establecer el `Content-Type` encabezado de respuesta para describir el cuerpo de respuesta.</span><span class="sxs-lookup"><span data-stu-id="cd565-158">Most helper methods in this category include a `ContentType` property, allowing you to set the `Content-Type` response header to describe the response body.</span></span>

<span data-ttu-id="cd565-159">Hay dos tipos de resultado de esta categoría: [vista](xref:mvc/views/overview) y [respuesta con formato](xref:mvc/models/formatting).</span><span class="sxs-lookup"><span data-stu-id="cd565-159">There are two result types within this category: [View](xref:mvc/views/overview) and [Formatted Response](xref:mvc/models/formatting).</span></span>

* <span data-ttu-id="cd565-160">**Vista**</span><span class="sxs-lookup"><span data-stu-id="cd565-160">**View**</span></span>

    <span data-ttu-id="cd565-161">Este tipo devuelve una vista que utiliza un modelo para representar HTML.</span><span class="sxs-lookup"><span data-stu-id="cd565-161">This type returns a view which uses a model to render HTML.</span></span> <span data-ttu-id="cd565-162">Por ejemplo, `return View(customer);` pasa de un modelo a la vista de enlace de datos.</span><span class="sxs-lookup"><span data-stu-id="cd565-162">For example, `return View(customer);` passes a model to the view for data-binding.</span></span>

* <span data-ttu-id="cd565-163">**Respuesta con formato**</span><span class="sxs-lookup"><span data-stu-id="cd565-163">**Formatted Response**</span></span>

    <span data-ttu-id="cd565-164">Este tipo devuelve JSON o un formato de intercambio de datos similares para representar un objeto de una forma específica.</span><span class="sxs-lookup"><span data-stu-id="cd565-164">This type returns JSON or a similar data exchange format to represent an object in a specific manner.</span></span> <span data-ttu-id="cd565-165">Por ejemplo, `return Json(customer);` serializa el objeto proporcionado en formato JSON.</span><span class="sxs-lookup"><span data-stu-id="cd565-165">For example, `return Json(customer);` serializes the provided object into JSON format.</span></span>
    
    <span data-ttu-id="cd565-166">Otros métodos comunes de este tipo son `File`, `PhysicalFile`, y `VirtualFile`.</span><span class="sxs-lookup"><span data-stu-id="cd565-166">Other common methods of this type include `File`, `PhysicalFile`, and `VirtualFile`.</span></span> <span data-ttu-id="cd565-167">Por ejemplo, `return PhysicalFile(customerFilePath, "text/xml");` devuelve un archivo XML descrito por un `Content-Type` valor de encabezado de respuesta de "text/xml".</span><span class="sxs-lookup"><span data-stu-id="cd565-167">For example, `return PhysicalFile(customerFilePath, "text/xml");` returns an XML file described by a `Content-Type` response header value of "text/xml".</span></span>

#### <a name="3-methods-resulting-in-a-non-empty-response-body-formatted-in-a-content-type-negotiated-with-the-client"></a><span data-ttu-id="cd565-168">3. Métodos resultante en un cuerpo de respuesta no vacía con formato en un tipo de contenido que se negocian con el cliente</span><span class="sxs-lookup"><span data-stu-id="cd565-168">3. Methods resulting in a non-empty response body formatted in a content type negotiated with the client</span></span>

<span data-ttu-id="cd565-169">Esta categoría es mejor conocido como **negociación de contenido**.</span><span class="sxs-lookup"><span data-stu-id="cd565-169">This category is better known as **Content Negotiation**.</span></span> <span data-ttu-id="cd565-170">[Negociación de contenido](xref:mvc/models/formatting#content-negotiation) se aplica siempre que una acción devuelve un [ObjectResult](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.objectresult) tipo o algo distinto de un [IActionResult](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.iactionresult) implementación.</span><span class="sxs-lookup"><span data-stu-id="cd565-170">[Content negotiation](xref:mvc/models/formatting#content-negotiation) applies whenever an action returns an [ObjectResult](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.objectresult) type or something other than an [IActionResult](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.iactionresult) implementation.</span></span> <span data-ttu-id="cd565-171">Una acción que devuelve no`IActionResult` implementación (por ejemplo, `object`) también devuelve una respuesta con formato.</span><span class="sxs-lookup"><span data-stu-id="cd565-171">An action that returns a non-`IActionResult` implementation (for example, `object`) also returns a Formatted Response.</span></span>

<span data-ttu-id="cd565-172">Algunos métodos de aplicación auxiliar de este tipo incluyen `BadRequest`, `CreatedAtRoute`, y `Ok`.</span><span class="sxs-lookup"><span data-stu-id="cd565-172">Some helper methods of this type include `BadRequest`, `CreatedAtRoute`, and `Ok`.</span></span> <span data-ttu-id="cd565-173">Algunos ejemplos de estos métodos son `return BadRequest(modelState);`, `return CreatedAtRoute("routename", values, newobject);`, y `return Ok(value);`, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="cd565-173">Examples of these methods include `return BadRequest(modelState);`, `return CreatedAtRoute("routename", values, newobject);`, and `return Ok(value);`, respectively.</span></span> <span data-ttu-id="cd565-174">Tenga en cuenta que `BadRequest` y `Ok` realizan la negociación de contenido solo cuando se pasa un valor; sin tener que pasar un valor, en su lugar, actúan como tipos de resultado de código de estado HTTP.</span><span class="sxs-lookup"><span data-stu-id="cd565-174">Note that `BadRequest` and `Ok` perform content negotiation only when passed a value; without being passed a value, they instead serve as HTTP Status Code result types.</span></span> <span data-ttu-id="cd565-175">El `CreatedAtRoute` método, por otro lado, siempre realiza una negociación de contenido desde las sobrecargas todos requieren que se pasa un valor.</span><span class="sxs-lookup"><span data-stu-id="cd565-175">The `CreatedAtRoute` method, on the other hand, always performs content negotiation since its overloads all require that a value be passed.</span></span>

### <a name="cross-cutting-concerns"></a><span data-ttu-id="cd565-176">Problemas de corte del cruce</span><span class="sxs-lookup"><span data-stu-id="cd565-176">Cross-Cutting Concerns</span></span>

<span data-ttu-id="cd565-177">Normalmente, las aplicaciones compartan partes de su flujo de trabajo.</span><span class="sxs-lookup"><span data-stu-id="cd565-177">Applications typically share parts of their workflow.</span></span> <span data-ttu-id="cd565-178">Por ejemplo, una aplicación que requiere autenticación para tener acceso a la cesta o una aplicación que almacena en caché datos en algunas páginas.</span><span class="sxs-lookup"><span data-stu-id="cd565-178">Examples include an app that requires authentication to access the shopping cart, or an app that caches data on some pages.</span></span> <span data-ttu-id="cd565-179">Para llevar a cabo lógica antes o después de un método de acción, use un *filtro*.</span><span class="sxs-lookup"><span data-stu-id="cd565-179">To perform logic before or after an action method, use a *filter*.</span></span> <span data-ttu-id="cd565-180">Usar [filtros](xref:mvc/controllers/filters) en cuestiones de corte del cruce puede reducir la duplicación, lo que les permite seguir el [principio no repita usted mismo (SECA)](http://deviq.com/don-t-repeat-yourself/).</span><span class="sxs-lookup"><span data-stu-id="cd565-180">Using [Filters](xref:mvc/controllers/filters) on cross-cutting concerns can reduce duplication, allowing them to follow the [Don't Repeat Yourself (DRY) principle](http://deviq.com/don-t-repeat-yourself/).</span></span>

<span data-ttu-id="cd565-181">Filtrar más los atributos, como `[Authorize]`, se puede aplicar en el nivel de controlador o acción según el nivel deseado de granularidad.</span><span class="sxs-lookup"><span data-stu-id="cd565-181">Most filter attributes, such as `[Authorize]`, can be applied at the controller or action level depending upon the desired level of granularity.</span></span>

<span data-ttu-id="cd565-182">Control de errores y las respuestas en caché son a menudo preocupaciones transversales:</span><span class="sxs-lookup"><span data-stu-id="cd565-182">Error handling and response caching are often cross-cutting concerns:</span></span>
   * [<span data-ttu-id="cd565-183">Control de errores</span><span class="sxs-lookup"><span data-stu-id="cd565-183">Error handling</span></span>](xref:mvc/controllers/filters#exception-filters)
   * [<span data-ttu-id="cd565-184">Almacenamiento en caché de respuestas</span><span class="sxs-lookup"><span data-stu-id="cd565-184">Response Caching</span></span>](xref:performance/caching/response)

<span data-ttu-id="cd565-185">Muchos problemas de corte del cruce pueden controlarse mediante filtros o personalizado [middleware](xref:fundamentals/middleware).</span><span class="sxs-lookup"><span data-stu-id="cd565-185">Many cross-cutting concerns can be handled using filters or custom [middleware](xref:fundamentals/middleware).</span></span>