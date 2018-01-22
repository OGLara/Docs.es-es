<span data-ttu-id="410d5-101">Reemplace el contenido de *Controllers/HelloWorldController.cs* con lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="410d5-101">Replace the contents of *Controllers/HelloWorldController.cs* with the following:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_1)]

<span data-ttu-id="410d5-102">Cada método `public` en un controlador puede ser invocado como un punto de conexión HTTP.</span><span class="sxs-lookup"><span data-stu-id="410d5-102">Every `public` method in a controller is callable as an HTTP endpoint.</span></span> <span data-ttu-id="410d5-103">En el ejemplo anterior, ambos métodos devuelven una cadena.</span><span class="sxs-lookup"><span data-stu-id="410d5-103">In the sample above, both methods return a string.</span></span>  <span data-ttu-id="410d5-104">Observe los comentarios delante de cada método.</span><span class="sxs-lookup"><span data-stu-id="410d5-104">Note the comments preceding each method.</span></span>

<span data-ttu-id="410d5-105">Un extremo HTTP es una dirección URL que se puede usar como destino en la aplicación web, como por ejemplo `http://localhost:1234/HelloWorld`. Combina el protocolo usado `HTTP`, la ubicación de red del servidor web (incluido el puerto TCP) `localhost:1234` y el URI de destino `HelloWorld`.</span><span class="sxs-lookup"><span data-stu-id="410d5-105">An HTTP endpoint is a targetable URL in the web application, such as `http://localhost:1234/HelloWorld`, and combines the protocol used: `HTTP`, the network location of the web server (including the TCP port): `localhost:1234` and the target URI `HelloWorld`.</span></span>

<span data-ttu-id="410d5-106">El primer comentario dice que se trata de un método [HTTP GET](https://www.w3schools.com/tags/ref_httpmethods.asp) que se invoca mediante la anexión de "/HelloWorld/" a la dirección URL base.</span><span class="sxs-lookup"><span data-stu-id="410d5-106">The first comment states this is an [HTTP GET](https://www.w3schools.com/tags/ref_httpmethods.asp) method that is invoked by appending "/HelloWorld/" to the base URL.</span></span> <span data-ttu-id="410d5-107">El segundo comentario dice que se trata de un método [HTTP GET](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) que se invoca mediante la anexión de "/HelloWorld/Welcome/" a la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="410d5-107">The second comment specifies an [HTTP GET](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) method that is invoked by appending "/HelloWorld/Welcome/" to the URL.</span></span> <span data-ttu-id="410d5-108">Más adelante en el tutorial usaremos el motor de scaffolding para generar métodos `HTTP POST`.</span><span class="sxs-lookup"><span data-stu-id="410d5-108">Later on in the tutorial you'll use the scaffolding engine to generate `HTTP POST` methods.</span></span>

<span data-ttu-id="410d5-109">Ejecute la aplicación en modo de no depuración y anexione "HelloWorld" a la ruta de acceso en la barra de direcciones.</span><span class="sxs-lookup"><span data-stu-id="410d5-109">Run the app in non-debug mode and append "HelloWorld" to the path in the address bar.</span></span> <span data-ttu-id="410d5-110">El método `Index` devuelve una cadena.</span><span class="sxs-lookup"><span data-stu-id="410d5-110">The `Index` method returns a string.</span></span>

![Ventana del explorador que muestra una respuesta de la aplicación a "This is my default action" (Esta es mi acción predeterminada)](../../tutorials/first-mvc-app/adding-controller/_static/hell1.png)

<span data-ttu-id="410d5-112">MVC invoca las clases del controlador (y los métodos de acción que contienen) en función de la URL entrante.</span><span class="sxs-lookup"><span data-stu-id="410d5-112">MVC invokes controller classes (and the action methods within them) depending on the incoming URL.</span></span> <span data-ttu-id="410d5-113">La [lógica de enrutamiento de URL](../../mvc/controllers/routing.md) predeterminada que usa MVC emplea un formato similar al siguiente para determinar qué código se debe invocar:</span><span class="sxs-lookup"><span data-stu-id="410d5-113">The default [URL routing logic](../../mvc/controllers/routing.md) used by MVC uses a format like this to determine what code to invoke:</span></span>

`/[Controller]/[ActionName]/[Parameters]`

<span data-ttu-id="410d5-114">El formato para el enrutamiento se establece en el archivo *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="410d5-114">You set the format for routing in the *Startup.cs* file.</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=snippet_1&highlight=5)]

<span data-ttu-id="410d5-115">Cuando se ejecuta la aplicación y no se suministra ningún segmento de dirección URL, de manera predeterminada se usa el controlador "Home" y el método "Index" especificados en la línea de plantilla resaltada arriba.</span><span class="sxs-lookup"><span data-stu-id="410d5-115">When you run the app and don't supply any URL segments, it defaults to the "Home" controller and the "Index" method specified in the template line highlighted above.</span></span>

<span data-ttu-id="410d5-116">El primer segmento de dirección URL determina la clase de controlador que se va a ejecutar.</span><span class="sxs-lookup"><span data-stu-id="410d5-116">The first URL segment determines the controller class to run.</span></span> <span data-ttu-id="410d5-117">De modo que `localhost:xxxx/HelloWorld` se asigna a la clase `HelloWorldController`.</span><span class="sxs-lookup"><span data-stu-id="410d5-117">So `localhost:xxxx/HelloWorld` maps to the `HelloWorldController` class.</span></span> <span data-ttu-id="410d5-118">La segunda parte del segmento de dirección URL determina el método de acción en la clase.</span><span class="sxs-lookup"><span data-stu-id="410d5-118">The second part of the URL segment determines the action method on the class.</span></span> <span data-ttu-id="410d5-119">De modo que `localhost:xxxx/HelloWorld/Index` podría provocar que se ejecute el método `Index` de la clase `HelloWorldController`.</span><span class="sxs-lookup"><span data-stu-id="410d5-119">So `localhost:xxxx/HelloWorld/Index` would cause the `Index` method of the `HelloWorldController` class to run.</span></span> <span data-ttu-id="410d5-120">Tenga en cuenta que solo es necesario navegar a `localhost:xxxx/HelloWorld` para que se llame al método `Index` de manera predeterminada.</span><span class="sxs-lookup"><span data-stu-id="410d5-120">Notice that you only had to browse to `localhost:xxxx/HelloWorld` and the `Index` method was called by default.</span></span> <span data-ttu-id="410d5-121">Esto es porque `Index` es el método predeterminado al que se llamará en un controlador si no se especifica explícitamente un nombre de método.</span><span class="sxs-lookup"><span data-stu-id="410d5-121">This is because `Index` is the default method that will be called on a controller if a method name is not explicitly specified.</span></span> <span data-ttu-id="410d5-122">La tercera parte del segmento de dirección URL (`id`) es para los datos de ruta.</span><span class="sxs-lookup"><span data-stu-id="410d5-122">The third part of the URL segment ( `id`) is for route data.</span></span> <span data-ttu-id="410d5-123">Veremos los datos de ruta más adelante en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="410d5-123">You'll see route data later on in this tutorial.</span></span>

<span data-ttu-id="410d5-124">Vaya a `http://localhost:xxxx/HelloWorld/Welcome`.</span><span class="sxs-lookup"><span data-stu-id="410d5-124">Browse to `http://localhost:xxxx/HelloWorld/Welcome`.</span></span> <span data-ttu-id="410d5-125">El método `Welcome` se ejecuta y devuelve la cadena "This is the Welcome action method..." (Este es el método de acción de bienvenida).</span><span class="sxs-lookup"><span data-stu-id="410d5-125">The `Welcome` method runs and returns the string "This is the Welcome action method...".</span></span> <span data-ttu-id="410d5-126">Para esta dirección URL, el controlador es `HelloWorld` y `Welcome` es el método de acción.</span><span class="sxs-lookup"><span data-stu-id="410d5-126">For this URL, the controller is `HelloWorld` and `Welcome` is the action method.</span></span> <span data-ttu-id="410d5-127">Todavía no ha usado el elemento `[Parameters]` de la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="410d5-127">You haven't used the `[Parameters]` part of the URL yet.</span></span>

![Ventana del explorador que muestra la respuesta de la aplicación "This is the Welcome action method" (Este es el método de acción predeterminado)](../../tutorials/first-mvc-app/adding-controller/_static/welcome.png)

<span data-ttu-id="410d5-129">Modifique el código para pasar cierta información del parámetro desde la dirección URL al controlador.</span><span class="sxs-lookup"><span data-stu-id="410d5-129">Modify the code to pass some parameter information from the URL to the controller.</span></span> <span data-ttu-id="410d5-130">Por ejemplo: `/HelloWorld/Welcome?name=Rick&numtimes=4`.</span><span class="sxs-lookup"><span data-stu-id="410d5-130">For example, `/HelloWorld/Welcome?name=Rick&numtimes=4`.</span></span> <span data-ttu-id="410d5-131">Cambie el método `Welcome` para que incluya dos parámetros, como se muestra en el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="410d5-131">Change the `Welcome` method to include two parameters as shown in the following code.</span></span> 

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_2)]

<span data-ttu-id="410d5-132">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="410d5-132">The preceding code:</span></span>

* <span data-ttu-id="410d5-133">Usa la característica de parámetro opcional de C# para indicar que el parámetro `numTimes` tiene el valor predeterminado 1 si no se pasa ningún valor para ese parámetro.</span><span class="sxs-lookup"><span data-stu-id="410d5-133">Uses the C# optional-parameter feature to indicate that the `numTimes` parameter defaults to 1 if no value is passed for that parameter.</span></span>
* <span data-ttu-id="410d5-134">Usa `HtmlEncoder.Default.Encode` para proteger la aplicación de entradas malintencionadas (es decir, JavaScript).</span><span class="sxs-lookup"><span data-stu-id="410d5-134">Uses`HtmlEncoder.Default.Encode` to protect the app from malicious input (namely JavaScript).</span></span> 
* <span data-ttu-id="410d5-135">Usa [cadenas interpoladas](https://docs.microsoft.com/dotnet/articles/csharp/language-reference/keywords/interpolated-strings).</span><span class="sxs-lookup"><span data-stu-id="410d5-135">Uses [Interpolated Strings](https://docs.microsoft.com/dotnet/articles/csharp/language-reference/keywords/interpolated-strings).</span></span>

<span data-ttu-id="410d5-136">Ejecute la aplicación y navegue a:</span><span class="sxs-lookup"><span data-stu-id="410d5-136">Run your app and browse to:</span></span>

   `http://localhost:xxxx/HelloWorld/Welcome?name=Rick&numtimes=4`

<span data-ttu-id="410d5-137">(Reemplace xxxx con el número de puerto). Puede probar valores diferentes para `name` y `numtimes` en la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="410d5-137">(Replace xxxx with your port number.) You can try different values for `name` and `numtimes` in  the URL.</span></span> <span data-ttu-id="410d5-138">El sistema de [enlace de modelos](../../mvc/models/model-binding.md) de MVC asigna automáticamente los parámetros con nombre de la cadena de consulta en la barra de dirección a los parámetros del método.</span><span class="sxs-lookup"><span data-stu-id="410d5-138">The MVC [model binding](../../mvc/models/model-binding.md) system automatically maps the named parameters from  the query string in the address bar to parameters in your method.</span></span> <span data-ttu-id="410d5-139">Vea [Model Binding](../../mvc/models/model-binding.md) (Enlace de modelos) para más información.</span><span class="sxs-lookup"><span data-stu-id="410d5-139">See [Model Binding](../../mvc/models/model-binding.md) for more information.</span></span>

![Ventana del explorador que muestra una respuesta de la aplicación a "Hello Rick, NumTimes is: 4" (Hola Rick, NumTimes es: 4)](../../tutorials/first-mvc-app/adding-controller/_static/rick4.png)

<span data-ttu-id="410d5-141">En la ilustración anterior, el segmento de dirección URL (`Parameters`) no se usa, y los parámetros `name` y `numTimes` se pasan como [cadenas de consulta](https://wikipedia.org/wiki/Query_string).</span><span class="sxs-lookup"><span data-stu-id="410d5-141">In the image above, the URL segment (`Parameters`) is not used, the `name` and `numTimes` parameters are passed as [query strings](https://wikipedia.org/wiki/Query_string).</span></span> <span data-ttu-id="410d5-142">El `?` (signo de interrogación) en la dirección URL anterior es un separador y le siguen las cadenas de consulta.</span><span class="sxs-lookup"><span data-stu-id="410d5-142">The `?` (question mark) in the above URL is a separator, and the query strings follow.</span></span> <span data-ttu-id="410d5-143">El carácter `&` separa las cadenas de consulta.</span><span class="sxs-lookup"><span data-stu-id="410d5-143">The `&` character separates query strings.</span></span>

<span data-ttu-id="410d5-144">Reemplace el método `Welcome` con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="410d5-144">Replace the `Welcome` method with the following code:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_3)]

<span data-ttu-id="410d5-145">Ejecute la aplicación y escriba la dirección URL siguiente: `http://localhost:xxx/HelloWorld/Welcome/3?name=Rick`</span><span class="sxs-lookup"><span data-stu-id="410d5-145">Run the app and enter the following URL:  `http://localhost:xxx/HelloWorld/Welcome/3?name=Rick`</span></span>

![Ventana del explorador que muestra una respuesta de la aplicación a "Hello Rick, ID: 3" (Hola Rick, Id.: 3)](../../tutorials/first-mvc-app/adding-controller/_static/rick_routedata.png)

<span data-ttu-id="410d5-147">Esta vez el tercer segmento de dirección URL coincide con el parámetro de ruta `id`.</span><span class="sxs-lookup"><span data-stu-id="410d5-147">This time the third URL segment  matched the route parameter `id`.</span></span> <span data-ttu-id="410d5-148">El método `Welcome` contiene un parámetro `id` que coincide con la plantilla de dirección URL en el método `MapRoute`.</span><span class="sxs-lookup"><span data-stu-id="410d5-148">The `Welcome`  method contains a parameter  `id` that matched the URL template in the `MapRoute` method.</span></span> <span data-ttu-id="410d5-149">El `?` del final (en `id?`) indica que el parámetro `id` es opcional.</span><span class="sxs-lookup"><span data-stu-id="410d5-149">The trailing `?`  (in `id?`) indicates the `id` parameter is optional.</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=snippet_1&highlight=5)]

<span data-ttu-id="410d5-150">En estos ejemplos, el controlador ha realizado la parte "VC" de MVC, es decir, el trabajo de vista y de controlador.</span><span class="sxs-lookup"><span data-stu-id="410d5-150">In these examples the controller has been doing the "VC" portion  of MVC - that is, the view and controller work.</span></span> <span data-ttu-id="410d5-151">El controlador devuelve HTML directamente.</span><span class="sxs-lookup"><span data-stu-id="410d5-151">The controller is returning HTML  directly.</span></span> <span data-ttu-id="410d5-152">Por lo general, no es aconsejable que los controles devuelvan HTML directamente, porque resulta muy complicado de programar y mantener.</span><span class="sxs-lookup"><span data-stu-id="410d5-152">Generally you don't want controllers returning HTML directly, since  that becomes very cumbersome to code and maintain.</span></span> <span data-ttu-id="410d5-153">En su lugar, se suele usar un archivo de plantilla de vista de Razor independiente para ayudar a generar la respuesta HTML.</span><span class="sxs-lookup"><span data-stu-id="410d5-153">Instead you typically use a separate Razor view template file to help generate the HTML response.</span></span> <span data-ttu-id="410d5-154">Haremos esto en el siguiente tutorial.</span><span class="sxs-lookup"><span data-stu-id="410d5-154">You do that in the next tutorial.</span></span>