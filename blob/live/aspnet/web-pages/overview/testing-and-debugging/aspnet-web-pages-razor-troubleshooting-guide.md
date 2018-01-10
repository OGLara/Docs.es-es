---
uid: web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
title: "ASP.NET Web Pages (Razor) Guía de solución de problemas | Documentos de Microsoft"
author: tfitzmac
description: "Este artículo describen los problemas que podría tener al trabajar con ASP.NET Web Pages (Razor) y algunas soluciones sugeridas. Versiones de software de página Web de ASP.NET..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: 2a2c1833-0bfe-4e2e-9cc0-341b52c7b121
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
msc.type: authoredcontent
ms.openlocfilehash: e507d97903583c7233456e9139e1a869f8bf75bd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-pages-razor-troubleshooting-guide"></a><span data-ttu-id="f4e9e-104">Guía de solución de problemas (Razor) de ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="f4e9e-104">ASP.NET Web Pages (Razor) Troubleshooting Guide</span></span>
====================
<span data-ttu-id="f4e9e-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="f4e9e-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="f4e9e-106">Este artículo describen los problemas que podría tener al trabajar con ASP.NET Web Pages (Razor) y algunas soluciones sugeridas.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-106">This article describes issues that you might have when working with ASP.NET Web Pages (Razor) and some suggested solutions.</span></span>
> 
> ## <a name="software-versions"></a><span data-ttu-id="f4e9e-107">Versiones de software</span><span class="sxs-lookup"><span data-stu-id="f4e9e-107">Software versions</span></span>
> 
> 
> - <span data-ttu-id="f4e9e-108">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="f4e9e-108">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="f4e9e-109">Este tutorial también funciona con ASP.NET Web Pages 2 y páginas Web de ASP.NET 1.0.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-109">This tutorial also works with ASP.NET Web Pages 2 and ASP.NET Web Pages 1.0.</span></span>


<span data-ttu-id="f4e9e-110">Este tema contiene las siguientes secciones:</span><span class="sxs-lookup"><span data-stu-id="f4e9e-110">This topic contains the following sections:</span></span>

- [<span data-ttu-id="f4e9e-111">Problemas con la ejecución de páginas</span><span class="sxs-lookup"><span data-stu-id="f4e9e-111">Issues with Running Pages</span></span>](#Issues_Running_.cshtml_Pages)
- [<span data-ttu-id="f4e9e-112">Problemas con el código Razor</span><span class="sxs-lookup"><span data-stu-id="f4e9e-112">Issues with Razor Code</span></span>](#IssuesWithRazorCode)
- [<span data-ttu-id="f4e9e-113">Problemas con la seguridad y la pertenencia a</span><span class="sxs-lookup"><span data-stu-id="f4e9e-113">Issues with Security and Membership</span></span>](#membership)
- [<span data-ttu-id="f4e9e-114">Problemas con el envío de correo electrónico</span><span class="sxs-lookup"><span data-stu-id="f4e9e-114">Issues with Sending Email</span></span>](#email)
- [<span data-ttu-id="f4e9e-115">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="f4e9e-115">Additional Resources</span></span>](#AdditionalResources)

<span data-ttu-id="f4e9e-116">Para preguntas generales, vea [ASP.NET Web Pages (Razor) preguntas más frecuentes sobre](https://go.microsoft.com/fwlink/?LinkId=253000).</span><span class="sxs-lookup"><span data-stu-id="f4e9e-116">For general questions, see [ASP.NET Web Pages (Razor) FAQ](https://go.microsoft.com/fwlink/?LinkId=253000).</span></span>

<a id="Issues_Running_.cshtml_Pages"></a>
## <a name="issues-with-running-pages"></a><span data-ttu-id="f4e9e-117">Problemas con la ejecución de páginas</span><span class="sxs-lookup"><span data-stu-id="f4e9e-117">Issues with Running Pages</span></span>

<span data-ttu-id="f4e9e-118">Puede evitar que una variedad de problemas *.cshtml* y *.vbhtml* páginas ejecuten correctamente.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-118">A variety of issues can prevent *.cshtml* and *.vbhtml* pages from running properly.</span></span> <span data-ttu-id="f4e9e-119">En esta sección se enumera mensajes de error comunes y causas probables.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-119">This section lists common error messages and likely causes.</span></span>

### <a name="http-error-403---forbidden-access-is-denied"></a><span data-ttu-id="f4e9e-120">HTTP Error 403 - Prohibido: Acceso denegado</span><span class="sxs-lookup"><span data-stu-id="f4e9e-120">HTTP Error 403 - Forbidden: Access is denied</span></span>

<span data-ttu-id="f4e9e-121">*No tiene permiso para ver este directorio o esta página con las credenciales que ha proporcionado.*</span><span class="sxs-lookup"><span data-stu-id="f4e9e-121">*You do not have permission to view this directory or page using the credentials that you supplied.*</span></span>

<span data-ttu-id="f4e9e-122">Este error puede producirse si el servidor no está ejecutando la versión correcta de .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-122">This error can occur if the server is not running the correct version of the .NET Framework.</span></span> <span data-ttu-id="f4e9e-123">Asegúrese de que el equipo que ejecuta el servidor (local o remotamente) tiene al menos .NET Framework 4 instalado.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-123">Make sure that the computer that's running the server (locally or remotely) has at least the .NET Framework 4 installed.</span></span> <span data-ttu-id="f4e9e-124">Además, asegúrese de que la propia aplicación se configura para ejecutar la versión correcta.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-124">Also make sure that the application itself is configured to run the right version.</span></span>

<span data-ttu-id="f4e9e-125">Si ve este problema localmente mientras trabaja en WebMatrix, haga clic en el **sitio** área de trabajo y en la vista de árbol, después, haga clic en **configuración**.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-125">If you see this problem locally while working in WebMatrix, click the **Site** workspace, and then in the treeview click **Settings**.</span></span> <span data-ttu-id="f4e9e-126">En el **seleccione la versión de .NET Framework** seleccione **.NET 4 (integrado)**.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-126">In the **Select .NET Framework Version** list, select **.NET 4 (Integrated)**.</span></span> <span data-ttu-id="f4e9e-127">Si ya se ha establecido esta versión, intente ejecutar WebMatrix como administrador.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-127">If this version is already set, try running WebMatrix as an administrator.</span></span>

<span data-ttu-id="f4e9e-128">Asegúrese de que la raíz del sitio Web tiene al menos un *.cshtml* archivos en ella.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-128">Make sure that the root of your website has at least one *.cshtml* file in it.</span></span>

<span data-ttu-id="f4e9e-129">Si ve este error cuando el servidor web está en un servidor remoto, póngase en contacto con el administrador del servidor.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-129">If you see this error when the web server is on a remote server, contact the server administrator.</span></span> <span data-ttu-id="f4e9e-130">Asegúrese de que el servidor tenga .NET Framework 4 o posterior instalado.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-130">Make sure that the server has the .NET Framework 4 or later installed.</span></span> <span data-ttu-id="f4e9e-131">Además, asegúrese de que la aplicación se ejecuta en un grupo de aplicaciones está configurado para utilizar esa versión de.NET Framework.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-131">Also make sure that the application is running in an application pool that's configured to use that version of the.NET Framework.</span></span>

<span data-ttu-id="f4e9e-132">Si tiene control sobre el servidor, asegúrese de que se está ejecutando la versión correcta de .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-132">If you have control over the server, make sure it's running the correct version of the .NET Framework.</span></span> <span data-ttu-id="f4e9e-133">También puede intentar reparar la instalación mediante la ejecución de la `aspnet_regiis -iru` comando.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-133">You might also try repairing the installation by running the `aspnet_regiis -iru` command.</span></span> <span data-ttu-id="f4e9e-134">(Por ejemplo, si instaló IIS después de instalar .NET Framework, IIS no se configurarse correctamente para ejecutar las páginas ASP.NET.) Para obtener más información, consulte [herramienta de registro de IIS de ASP.NET (Aspnet\_regiis.exe)](https://msdn.microsoft.com/en-US/library/k6h9cz8h(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="f4e9e-134">(For example, if you install IIS after you install the .NET Framework, IIS will not be correctly configured to run ASP.NET pages.) For more information, see [ASP.NET IIS Registration Tool (Aspnet\_regiis.exe)](https://msdn.microsoft.com/en-US/library/k6h9cz8h(v=vs.100).aspx).</span></span>

### <a name="http-error-40314---forbidden"></a><span data-ttu-id="f4e9e-135">Error de HTTP 403.14 - prohibido</span><span class="sxs-lookup"><span data-stu-id="f4e9e-135">HTTP Error 403.14 - Forbidden</span></span>

<span data-ttu-id="f4e9e-136">*El servidor Web está configurado para no mostrar el contenido de este directorio.*</span><span class="sxs-lookup"><span data-stu-id="f4e9e-136">*The Web server is configured to not list the contents of this directory.*</span></span>

<span data-ttu-id="f4e9e-137">Este error puede producirse si se solicita un recurso protegido (como el *Web.config* archivo) o que se encuentra en una carpeta protegida (como *aplicación\_datos* o *aplicación\_Código*).</span><span class="sxs-lookup"><span data-stu-id="f4e9e-137">This error can occur if you request a resource that's protected (like the *Web.config* file) or that's in a folder that's protected (like *App\_Data* or *App\_Code*).</span></span>

### <a name="http-error-40417---not-found"></a><span data-ttu-id="f4e9e-138">No se encontró un Error HTTP 404.17-</span><span class="sxs-lookup"><span data-stu-id="f4e9e-138">HTTP Error 404.17 - Not Found</span></span>

<span data-ttu-id="f4e9e-139">*El contenido solicitado parece ser un script y el controlador de archivos estáticos no lo servirá.*</span><span class="sxs-lookup"><span data-stu-id="f4e9e-139">*The requested content appears to be script and will not be served by the static file handler.*</span></span>

<span data-ttu-id="f4e9e-140">Este error puede producirse si el servidor no está configurado correctamente para que utilice .NET Framework 4 o posterior y, por tanto, no reconoce el código en `@{ }` bloques.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-140">This error can occur if the server is not configured correctly to use the .NET Framework 4 or later, and therefore does not recognize the code in `@{ }` blocks.</span></span> <span data-ttu-id="f4e9e-141">Vea la descripción anterior de *HTTP Error 403 - Prohibido: acceso denegado*.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-141">See the description earlier for *HTTP Error 403 - Forbidden: Access is denied*.</span></span>

### <a name="http-error-4047---not-found"></a><span data-ttu-id="f4e9e-142">No se encontró un Error HTTP 404.7:</span><span class="sxs-lookup"><span data-stu-id="f4e9e-142">HTTP Error 404.7 - Not Found</span></span>

<span data-ttu-id="f4e9e-143">*El módulo de filtrado de solicitudes está configurada para denegar la extensión de archivo*</span><span class="sxs-lookup"><span data-stu-id="f4e9e-143">*The request filtering module is configured to deny the file extension*</span></span>

<span data-ttu-id="f4e9e-144">Este error puede producirse si *.cshtml* o *.vbhtml* extensiones se ha bloqueado explícitamente en el servidor.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-144">This error can occur if *.cshtml* or *.vbhtml* extensions have been explicitly blocked on the server.</span></span> <span data-ttu-id="f4e9e-145">Un síntoma de este problema es que funcionan las direcciones URL cuando no incluya la extensión, pero las direcciones URL que incluyen *.cshtml* o *.vbhtml* no funcionan.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-145">A symptom of this problem is that URLs work when they do not include the extension, but URLs that include *.cshtml* or *.vbhtml* do not work.</span></span> <span data-ttu-id="f4e9e-146">Una posible solución consiste en volver a habilitar las extensiones en el sitio *Web.config* archivo.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-146">A possible solution is to re-enable the extensions in the site's *Web.config* file.</span></span> <span data-ttu-id="f4e9e-147">En el ejemplo siguiente se muestra cómo habilitar la *.cshtml* extensión.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-147">The following example shows how to enable the *.cshtml* extension.</span></span>

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample1.xml?highlight=5-6)]

### <a name="http-error-4048---not-found"></a><span data-ttu-id="f4e9e-148">No se encontró un Error HTTP 404.8:</span><span class="sxs-lookup"><span data-stu-id="f4e9e-148">HTTP Error 404.8 - Not Found</span></span>

<span data-ttu-id="f4e9e-149">*El módulo de filtrado de solicitudes está configurada para denegar una ruta de acceso en la dirección URL que contiene una sección hiddenSegment.*</span><span class="sxs-lookup"><span data-stu-id="f4e9e-149">*The request filtering module is configured to deny a path in the URL that contains a hiddenSegment section.*</span></span>

<span data-ttu-id="f4e9e-150">Este error puede producirse si se solicita un recurso protegido (como el *Web.config* archivo) o que se encuentra en una carpeta protegida (como *aplicación\_datos* o *aplicación\_Código*).</span><span class="sxs-lookup"><span data-stu-id="f4e9e-150">This error can occur if you request a resource that's protected (like the *Web.config* file) or that's in a folder that's protected (like *App\_Data* or *App\_Code*).</span></span>

### <a name="this-type-of-page-is-not-served-server-error-in--application"></a><span data-ttu-id="f4e9e-151">Este tipo de página no disponible (Error de servidor en la aplicación '/')</span><span class="sxs-lookup"><span data-stu-id="f4e9e-151">This type of page is not served (Server Error in '/' Application)</span></span>

<span data-ttu-id="f4e9e-152">Vea la descripción anterior de 404.17 de Error de HTTP.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-152">See the description earlier for HTTP Error 404.17.</span></span>

<a id="IssuesWithRazorCode"></a>
## <a name="issues-with-razor-code"></a><span data-ttu-id="f4e9e-153">Problemas con el código Razor</span><span class="sxs-lookup"><span data-stu-id="f4e9e-153">Issues with Razor code</span></span>

### <a name="the-name-class-does-not-exist-in-the-current-context"></a><span data-ttu-id="f4e9e-154">El nombre '*clase*' no existe en el contexto actual</span><span class="sxs-lookup"><span data-stu-id="f4e9e-154">The name '*class*' does not exist in the current context</span></span>

<span data-ttu-id="f4e9e-155">A menudo, una razón verá este error es que `class` referencias no se instala una aplicación auxiliar, pero la aplicación auxiliar.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-155">Often, a reason you see this error is that `class` references a helper, but the helper is not installed.</span></span> <span data-ttu-id="f4e9e-156">Por ejemplo, si intenta utilizar una aplicación auxiliar, pero si no ha instalado el paquete de NuGet, verá este error.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-156">For example, if you try to use a helper, but if you haven't installed the package from NuGet, you'll see this error.</span></span> <span data-ttu-id="f4e9e-157">Uso de la galería en WebMatrix para buscar e instalar la aplicación auxiliar.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-157">Use the Gallery in WebMatrix to find and install the helper.</span></span>

<span data-ttu-id="f4e9e-158">Si se instala la aplicación auxiliar, pero la página aún no la reconoce, intente agregar agregar un `using` instrucción en el código.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-158">If the helper is installed, but the page still doesn't recognize it, try adding add a `using` statement to the code.</span></span> <span data-ttu-id="f4e9e-159">En el `using` instrucción, referencia de espacio de nombres que incluye la aplicación auxiliar.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-159">In the `using` statement, reference the namespace that includes the helper.</span></span> <span data-ttu-id="f4e9e-160">Por ejemplo, las aplicaciones auxiliares básicas que se encuentran en el paquete de aplicación auxiliar de ASP.NET Web están en el `System.Web.Helpers` espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-160">For example, the basic helpers that are in the ASP.NET Web Helpers package are in the `System.Web.Helpers` namespace.</span></span> <span data-ttu-id="f4e9e-161">En la parte superior de la página donde desea utilizar la aplicación auxiliar, agregue esta línea:</span><span class="sxs-lookup"><span data-stu-id="f4e9e-161">At the top of the page where you want to use the helper, add this line:</span></span>

`@using Microsoft.Web.Helpers;`

<a id="membership"></a>
## <a name="issues-with-security-and-membership"></a><span data-ttu-id="f4e9e-162">Problemas con la seguridad y la pertenencia a</span><span class="sxs-lookup"><span data-stu-id="f4e9e-162">Issues with Security and Membership</span></span>

<span data-ttu-id="f4e9e-163">Si se usa el sistema de seguridad integrados (pertenencia) en ASP.NET Web Pages (Razor), pueden surgir los siguientes problemas.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-163">If you are using the built-in security (membership) system in ASP.NET Web Pages (Razor), you might encounter the following issues.</span></span>

### <a name="to-call-this-method-the-membershipprovider-property-must-be-an-instance-of-extendedmembershipprovider"></a><span data-ttu-id="f4e9e-164">Para llamar a este método, la propiedad "Membership.Provider" debe ser una instancia de "ExtendedMembershipProvider"</span><span class="sxs-lookup"><span data-stu-id="f4e9e-164">To call this method, the "Membership.Provider" property must be an instance of "ExtendedMembershipProvider"</span></span>

<span data-ttu-id="f4e9e-165">Este error puede indicar que no hay `AspNetSqlMembershipProvider` clase se configura.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-165">This error can indicate that no `AspNetSqlMembershipProvider` class is configured.</span></span> <span data-ttu-id="f4e9e-166">(Un síntoma es que el sitio funciona bien localmente pero este error produce cuando se publica en el servidor de un proveedor de hospedaje). Una solución para este problema consiste en habilitar explícitamente la pertenencia sencillo debe agregar lo siguiente en el sitio *Web.config* archivo:</span><span class="sxs-lookup"><span data-stu-id="f4e9e-166">(A symptom is that the site works fine locally but throws this error when you publish it to a hosting provider's server.) One fix for this problem is to explicitly enable simple membership by adding the following to the site's *Web.config* file:</span></span>

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample2.xml?highlight=6)]

<a id="email"></a>
## <a name="issues-with-sending-email"></a><span data-ttu-id="f4e9e-167">Problemas con el envío de correo electrónico</span><span class="sxs-lookup"><span data-stu-id="f4e9e-167">Issues with Sending Email</span></span>

<span data-ttu-id="f4e9e-168">Problemas con el envío de correo electrónico pueden resultar complicado para depurar.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-168">Problems with sending email can be challenging to debug.</span></span> <span data-ttu-id="f4e9e-169">Un problema inicial puede ser que no se puede conectar al servidor SMTP.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-169">An initial problem can be that you can't connect to the SMTP server.</span></span> <span data-ttu-id="f4e9e-170">Si la conexión es correcta, ASP.NET entrega el mensaje al servidor SMTP.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-170">If the connection is successful, ASP.NET hands the message off to the SMTP server.</span></span> <span data-ttu-id="f4e9e-171">Sin embargo, puede haber problemas con el propio mensaje que impide que el servidor SMTP de enviarlo.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-171">However, there can be problems with the message itself that prevents the SMTP server from sending it.</span></span>

<span data-ttu-id="f4e9e-172">Si la aplicación no envía correo electrónico correctamente, intente lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="f4e9e-172">If your application does not successfully send email, try the following:</span></span>

- <span data-ttu-id="f4e9e-173">El nombre del servidor SMTP suele ser algo parecido a `smtp.provider.com` o `smtp.provider.net`.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-173">The SMTP server name is often something like `smtp.provider.com` or `smtp.provider.net`.</span></span> <span data-ttu-id="f4e9e-174">Sin embargo, si se publica un sitio en un proveedor de hospedaje, el nombre del servidor SMTP en ese momento podría ser `localhost`.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-174">However, if you publish your site to a hosting provider, the SMTP server name at that point might be `localhost`.</span></span> <span data-ttu-id="f4e9e-175">Esta situación se produce porque una vez que ha publicado y el sitio se ejecuta en el servidor del proveedor, el servidor SMTP puede ser local desde la perspectiva de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-175">This situation occurs because after you've published and your site is running on the provider's server, the SMTP server might be local from the perspective of your application.</span></span> <span data-ttu-id="f4e9e-176">Este cambio en los nombres de servidor, podría significar que tiene que cambiar el nombre del servidor SMTP como parte del proceso de publicación.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-176">This change in server names might mean that you have to change the SMTP server name as part of your publishing process.</span></span>
- <span data-ttu-id="f4e9e-177">Normalmente, el número de puerto es 25.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-177">The port number is usually 25.</span></span> <span data-ttu-id="f4e9e-178">Sin embargo, algunos proveedores requieren que se va a utilizar el puerto 587 o algún otro puerto.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-178">However, some providers require you to use port 587 or some other port.</span></span> <span data-ttu-id="f4e9e-179">Consulte con el propietario del servidor SMTP qué número de puerto que esperan utilizar.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-179">Check with the owner of the SMTP server what port number they expect you to use.</span></span>
- <span data-ttu-id="f4e9e-180">Asegúrese de que utiliza las credenciales correctas.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-180">Make sure that you use the right credentials.</span></span> <span data-ttu-id="f4e9e-181">Si ha publicado su sitio en un proveedor de hospedaje, utilice las credenciales que el proveedor ha indicado específicamente son para correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-181">If you've published your site to a hosting provider, use the credentials that the provider has specifically indicated are for email.</span></span> <span data-ttu-id="f4e9e-182">Estas credenciales podrían ser diferentes de las credenciales que se usa para publicar.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-182">These credentials might be different from the credentials you use to publish.</span></span>
- <span data-ttu-id="f4e9e-183">En ocasiones, no necesita credenciales en absoluto.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-183">Sometimes you don't need credentials at all.</span></span> <span data-ttu-id="f4e9e-184">Si va a enviar correo electrónico mediante el uso de su ISP personal, el proveedor de correo electrónico ya quizá sepa que sus credenciales.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-184">If you're sending email by using your personal ISP, your email provider might already know your credentials.</span></span> <span data-ttu-id="f4e9e-185">Después de publicar, debe utilizar credenciales diferentes a cuando se prueba en el equipo local.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-185">After you publish, you might need to use different credentials than when you test on your local computer.</span></span>
- <span data-ttu-id="f4e9e-186">Si su proveedor de correo electrónico usa cifrado, establezca `WebMail.EnableSsl` a `true`.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-186">If your email provider uses encryption, set `WebMail.EnableSsl` to `true`.</span></span>

<span data-ttu-id="f4e9e-187">Si hay un error al enviar correo electrónico, verá un mensaje de error ASP.NET estándar, que es similar a esto:</span><span class="sxs-lookup"><span data-stu-id="f4e9e-187">If there is an error sending email, you might see a standard ASP.NET error message, which looks like this:</span></span>

![Mensaje de error ASP.NET cuando hay un problema con el correo electrónico](aspnet-web-pages-razor-troubleshooting-guide/_static/image1.png)

<span data-ttu-id="f4e9e-189">También es posible depurar problemas con el envío de correo electrónico mediante el uso de un `try-catch` bloque, como en el ejemplo siguiente.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-189">You can also debug problems with sending email by using a `try-catch` block, as in the following example.</span></span> <span data-ttu-id="f4e9e-190">Cuando se usa un `try-catch` bloque, ASP.NET no muestra los mensajes de error estándar.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-190">When you use a `try-catch` block, ASP.NET does not display its standard error messages.</span></span> <span data-ttu-id="f4e9e-191">En su lugar, puede capturar el error en la `catch` parte del bloque.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-191">Instead, you can capture the error in the `catch` portion of the block.</span></span>

[!code-cshtml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample3.cshtml)]

<span data-ttu-id="f4e9e-192">Sustituya los valores adecuados para `your-SMTP-server-name`, y así sucesivamente.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-192">Substitute the appropriate values for `your-SMTP-server-name`, and so on.</span></span> <span data-ttu-id="f4e9e-193">Algunos de los mensajes de error, es posible que vea este modo incluyen lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="f4e9e-193">Some of the error messages you might see this way include the following:</span></span>

- <span data-ttu-id="f4e9e-194">*Error al enviar correo.*</span><span class="sxs-lookup"><span data-stu-id="f4e9e-194">*Failure sending mail.*</span></span>

    <span data-ttu-id="f4e9e-195">O bien</span><span class="sxs-lookup"><span data-stu-id="f4e9e-195">-or-</span></span>

    <span data-ttu-id="f4e9e-196">*Un intento de conexión no se pudo porque la parte conectada no respondió adecuadamente tras un período de tiempo o conexión establecida no se pudo porque el host conectado no respondió*</span><span class="sxs-lookup"><span data-stu-id="f4e9e-196">*A connection attempt failed because the connected party did not properly respond after a period of time, or established connection failed because connected host has failed to respond*</span></span>

    <span data-ttu-id="f4e9e-197">Este error normalmente significa que la aplicación no se pudo conectar al servidor SMTP.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-197">This error usually means that the application could not connect to the SMTP server.</span></span> <span data-ttu-id="f4e9e-198">Compruebe el nombre del servidor y número de puerto.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-198">Check the server name and port number.</span></span>
- <span data-ttu-id="f4e9e-199">*Buzón no disponible. La respuesta del servidor fue: 5.1.0 &lt; someuser@invaliddomain &gt; remitente rechazado: dominio del remitente no válido*</span><span class="sxs-lookup"><span data-stu-id="f4e9e-199">*Mailbox unavailable. The server response was: 5.1.0 &lt;someuser@invaliddomain&gt; sender rejected : invalid sender domain*</span></span>

    <span data-ttu-id="f4e9e-200">Este mensaje puede indicar que el `From` dirección no es correcta o falta.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-200">This message can indicate that the `From` address is not correct or is missing.</span></span>
- <span data-ttu-id="f4e9e-201">*La cadena especificada no está en el formato que requiere una dirección de correo electrónico.*</span><span class="sxs-lookup"><span data-stu-id="f4e9e-201">*The specified string is not in the form required for an e-mail address.*</span></span>

    <span data-ttu-id="f4e9e-202">Este error puede indicar que el valor de la `To` o `From` propiedades no se reconocen como direcciones de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-202">This error might indicate that the value of the `To` or `From` properties are not recognized as email addresses.</span></span> <span data-ttu-id="f4e9e-203">(ASP.NET no puede comprobar que la dirección de correo electrónico es válida, solo 's en el formato correcto, como  *name@domain.com* .)</span><span class="sxs-lookup"><span data-stu-id="f4e9e-203">(ASP.NET cannot check that the email address is valid, only that it's in the correct format, like *name@domain.com*.)</span></span>

> [!NOTE]
> <span data-ttu-id="f4e9e-204">Quite el código que muestra el error (`@errorMessage`) antes de publicar la página en un sitio en vivo.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-204">Remove the markup that displays the error (`@errorMessage`) before you publish the page to a live site.</span></span> <span data-ttu-id="f4e9e-205">No es una buena idea de que los usuarios puedan ver los mensajes de error que se obtengan de un servidor.</span><span class="sxs-lookup"><span data-stu-id="f4e9e-205">It's not a good idea to let users see error messages that you get from a server.</span></span>


<a id="AdditionalResources"></a>
## <a name="additional-resources"></a><span data-ttu-id="f4e9e-206">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="f4e9e-206">Additional Resources</span></span>

[<span data-ttu-id="f4e9e-207">ASP.NET Web Pages (Razor) preguntas más frecuentes</span><span class="sxs-lookup"><span data-stu-id="f4e9e-207">ASP.NET Web Pages (Razor) FAQ</span></span>](https://go.microsoft.com/fwlink/?LinkId=253000)

<span data-ttu-id="f4e9e-208">[WebMatrix y ASP.NET Web Pages](https://forums.asp.net/1224.aspx/1?WebMatrix) foro en el sitio Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f4e9e-208">[WebMatrix and ASP.NET Web Pages](https://forums.asp.net/1224.aspx/1?WebMatrix) forum on the ASP.NET website</span></span>