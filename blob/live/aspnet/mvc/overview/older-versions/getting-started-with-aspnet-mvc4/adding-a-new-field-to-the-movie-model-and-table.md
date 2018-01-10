---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
title: "Agregar un nuevo campo al modelo de película y tabla | Documentos de Microsoft"
author: Rick-Anderson
description: "Nota: Una versión actualizada de este tutorial está disponible aquí que usa ASP.NET MVC 5 y Visual Studio 2013. Es más seguro y mucho más fácil de seguir y demostraciones..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 9ef2c4f1-a305-4e0a-9fb8-bfbd9ef331d9
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
msc.type: authoredcontent
ms.openlocfilehash: 0c094c4d4c99702a5b513717126872a254ca3e5a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-new-field-to-the-movie-model-and-table"></a><span data-ttu-id="4afdb-104">Agregar un nuevo campo en el modelo de película como una tabla</span><span class="sxs-lookup"><span data-stu-id="4afdb-104">Adding a New Field to the Movie Model and Table</span></span>
====================
<span data-ttu-id="4afdb-105">Por [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="4afdb-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="4afdb-106">Hay disponible una versión actualizada de este tutorial [aquí](../../getting-started/introduction/getting-started.md) que usa ASP.NET MVC 5 y Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="4afdb-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="4afdb-107">Es más seguro y mucho más fácil de seguir y se muestra más características.</span><span class="sxs-lookup"><span data-stu-id="4afdb-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="4afdb-108">En esta sección usará Entity Framework Code First Migrations migrar algunos cambios a las clases del modelo, por lo que el cambio se aplica a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="4afdb-108">In this section you'll use Entity Framework Code First Migrations to migrate some changes to the model classes so the change is applied to the database.</span></span>

<span data-ttu-id="4afdb-109">De forma predeterminada, cuando se usa Entity Framework Code First para crear automáticamente una base de datos, como lo hizo anteriormente en este tutorial, Code First agrega una tabla para ayudar a realizar el seguimiento de si el esquema de la base de datos está sincronizado con las clases del modelo se generó a partir de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="4afdb-109">By default, when you use Entity Framework Code First to automatically create a database, as you did earlier in this tutorial, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="4afdb-110">Si no están sincronizados, Entity Framework produce un error.</span><span class="sxs-lookup"><span data-stu-id="4afdb-110">If they aren't in sync, the Entity Framework throws an error.</span></span> <span data-ttu-id="4afdb-111">Esto hace más fácil realizar un seguimiento de los problemas en tiempo de desarrollo que en caso contrario, podría encontrar solamente (por errores poco claros) en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="4afdb-111">This makes it easier to track down issues at development time that you might otherwise only find (by obscure errors) at run time.</span></span>

## <a name="setting-up-code-first-migrations-for-model-changes"></a><span data-ttu-id="4afdb-112">Configuración de migraciones de Code First para cambios de modelo</span><span class="sxs-lookup"><span data-stu-id="4afdb-112">Setting up Code First Migrations for Model Changes</span></span>

<span data-ttu-id="4afdb-113">Si usas Visual Studio 2012, haga doble clic en el *Movies.mdf* archivo desde el Explorador de soluciones para abrir la herramienta de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="4afdb-113">If you are using Visual Studio 2012, double click the *Movies.mdf* file from Solution Explorer to open the database tool.</span></span> <span data-ttu-id="4afdb-114">Visual Studio Express para Web mostrará el Explorador de base de datos, que Visual Studio 2012 se mostrará el Explorador de servidores.</span><span class="sxs-lookup"><span data-stu-id="4afdb-114">Visual Studio Express for Web will show Database Explorer, Visual Studio 2012 will show Server Explorer.</span></span> <span data-ttu-id="4afdb-115">Si usa Visual Studio 2010, use el Explorador de objetos de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="4afdb-115">If you are using Visual Studio 2010, use SQL Server Object Explorer.</span></span>

<span data-ttu-id="4afdb-116">En la herramienta de la base de datos (Explorador de base de datos, Explorador de servidores o explorador de objetos de SQL Server), haga clic en `MovieDBContext` y seleccione **eliminar** para quitar la base de datos de películas.</span><span class="sxs-lookup"><span data-stu-id="4afdb-116">In the database tool (Database Explorer, Server Explorer or SQL Server Object Explorer), right click on `MovieDBContext` and select **Delete** to drop the movies database.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image1.png)

<span data-ttu-id="4afdb-117">Vaya al explorador de soluciones.</span><span class="sxs-lookup"><span data-stu-id="4afdb-117">Navigate back to Solution Explorer.</span></span> <span data-ttu-id="4afdb-118">Haga clic con el botón secundario en el *Movies.mdf* de archivos y seleccione **eliminar** para quitar la base de datos de películas.</span><span class="sxs-lookup"><span data-stu-id="4afdb-118">Right click on the *Movies.mdf* file and select **Delete** to remove the movies database.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image2.png)

<span data-ttu-id="4afdb-119">Compile la aplicación para asegurarse de que no hay ningún error.</span><span class="sxs-lookup"><span data-stu-id="4afdb-119">Build the application to make sure there are no errors.</span></span>

<span data-ttu-id="4afdb-120">Desde el **herramientas** menú, haga clic en **Administrador de paquetes de biblioteca** y, a continuación, **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="4afdb-120">From the **Tools** menu, click **Library Package Manager** and then **Package Manager Console**.</span></span>

![Agregar módulo Man](adding-a-new-field-to-the-movie-model-and-table/_static/image3.png)

<span data-ttu-id="4afdb-122">En el **Package Manager Console** ventana en el `PM>` símbolo del sistema escriba "Enable-Migrations - ContextTypeName MvcMovie.Models.MovieDBContext".</span><span class="sxs-lookup"><span data-stu-id="4afdb-122">In the **Package Manager Console** window at the `PM>` prompt enter "Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext".</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image4.png)

<span data-ttu-id="4afdb-123">El **Enable-Migrations** comando (se muestra arriba) crea un *archivo Configuration.cs que* archivo en una nueva *migraciones* carpeta.</span><span class="sxs-lookup"><span data-stu-id="4afdb-123">The **Enable-Migrations** command (shown above) creates a *Configuration.cs* file in a new *Migrations* folder.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image5.png)

<span data-ttu-id="4afdb-124">Visual Studio abre el *archivo Configuration.cs que* archivos.</span><span class="sxs-lookup"><span data-stu-id="4afdb-124">Visual Studio opens the *Configuration.cs* file.</span></span> <span data-ttu-id="4afdb-125">Reemplace el `Seed` método en el *archivo Configuration.cs que* archivos con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="4afdb-125">Replace the `Seed` method in the *Configuration.cs* file with the following code:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample1.cs)]

<span data-ttu-id="4afdb-126">Haga clic con el botón secundario en la línea ondulada roja bajo `Movie` y seleccione **resolver** , a continuación, **con** **MvcMovie.Models;**</span><span class="sxs-lookup"><span data-stu-id="4afdb-126">Right click on the red squiggly line under `Movie` and select **Resolve** then **using** **MvcMovie.Models;**</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image6.png)

<span data-ttu-id="4afdb-127">Si lo hace, agrega la siguiente instrucción using:</span><span class="sxs-lookup"><span data-stu-id="4afdb-127">Doing so adds the following using statement:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample2.cs)]

> [!NOTE] 
> 
> <span data-ttu-id="4afdb-128">Code First Migrations llamadas el `Seed` método después de cada migración (es decir, una llamada a **Actualizar base de datos** en la consola de administrador de paquetes), y este método actualiza las filas que ya se han insertado o insertan si únicamente aún no existen.</span><span class="sxs-lookup"><span data-stu-id="4afdb-128">Code First Migrations calls the `Seed` method after every migration (that is, calling **update-database** in the Package Manager Console), and this method updates rows that have already been inserted, or inserts them if they don't exist yet.</span></span>


<span data-ttu-id="4afdb-129">**Presione CTRL-MAYÚS + B para compilar el proyecto.** (Los pasos siguientes se producirá un error si el no se compilan en este momento.)</span><span class="sxs-lookup"><span data-stu-id="4afdb-129">**Press CTRL-SHIFT-B to build the project.**(The following steps will fail if your don't build at this point.)</span></span>

<span data-ttu-id="4afdb-130">El paso siguiente consiste en crear un `DbMigration` clase para la migración inicial.</span><span class="sxs-lookup"><span data-stu-id="4afdb-130">The next step is to create a `DbMigration` class for the initial migration.</span></span> <span data-ttu-id="4afdb-131">Esta migración crea una nueva base de datos, que es por lo que se elimina la *movie.mdf* archivo en un paso anterior.</span><span class="sxs-lookup"><span data-stu-id="4afdb-131">This migration to creates a new database, that's why you deleted the *movie.mdf* file in a previous step.</span></span>

<span data-ttu-id="4afdb-132">En el **Package Manager Console** ventana, escriba el comando "Agregar-migración inicial" para crear la migración inicial.</span><span class="sxs-lookup"><span data-stu-id="4afdb-132">In the **Package Manager Console** window, enter the command "add-migration Initial" to create the initial migration.</span></span> <span data-ttu-id="4afdb-133">El nombre "Inicial" es arbitrario y se utiliza para asignar nombre al archivo de migración que creó.</span><span class="sxs-lookup"><span data-stu-id="4afdb-133">The name "Initial" is arbitrary and is used to name the migration file created.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image7.png)

<span data-ttu-id="4afdb-134">Migraciones de Code First crea otro archivo de clase en el *migraciones* carpeta (con el nombre *{DateStamp}\_Initial.cs* ), y esta clase contiene código que crea el esquema de base de datos.</span><span class="sxs-lookup"><span data-stu-id="4afdb-134">Code First Migrations creates another class file in the *Migrations* folder (with the name *{DateStamp}\_Initial.cs* ), and this class contains code that creates the database schema.</span></span> <span data-ttu-id="4afdb-135">El nombre de archivo de migración previamente se fija con una marca de tiempo para ayudar a con la ordenación.</span><span class="sxs-lookup"><span data-stu-id="4afdb-135">The migration filename is pre-fixed with a timestamp to help with ordering.</span></span> <span data-ttu-id="4afdb-136">Examine la *{DateStamp}\_Initial.cs* archivo, contiene las instrucciones para crear la tabla de películas para la base de datos de la película.</span><span class="sxs-lookup"><span data-stu-id="4afdb-136">Examine the *{DateStamp}\_Initial.cs* file, it contains the instructions to create the Movies table for the Movie DB.</span></span> <span data-ttu-id="4afdb-137">Al actualizar la base de datos en las instrucciones a continuación, esto *{DateStamp}\_Initial.cs* archivo ejecutará y crear la el esquema de base de datos.</span><span class="sxs-lookup"><span data-stu-id="4afdb-137">When you update the database in the instructions below, this *{DateStamp}\_Initial.cs* file will run and create the the DB schema.</span></span> <span data-ttu-id="4afdb-138">La **inicialización** método se ejecutará para rellenar la base de datos con datos de prueba.</span><span class="sxs-lookup"><span data-stu-id="4afdb-138">Then the **Seed** method will run to populate the DB with test data.</span></span>

<span data-ttu-id="4afdb-139">En el **Package Manager Console**, escriba el comando "update-database" para crear la base de datos y ejecutar el **inicialización** método.</span><span class="sxs-lookup"><span data-stu-id="4afdb-139">In the **Package Manager Console**, enter the command "update-database" to create the database and run the **Seed** method.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image8.png)

<span data-ttu-id="4afdb-140">Si se produce un error que indica una tabla ya existe y no se puede crear, probablemente es porque ejecutó la aplicación después de eliminar la base de datos y antes de ejecutar `update-database`.</span><span class="sxs-lookup"><span data-stu-id="4afdb-140">If you get an error that indicates a table already exists and can't be created, it is probably because you ran the application after you deleted the database and before you executed `update-database`.</span></span> <span data-ttu-id="4afdb-141">En ese caso, elimine la *Movies.mdf* archivo nuevo y vuelva a intentar la `update-database` comando.</span><span class="sxs-lookup"><span data-stu-id="4afdb-141">In that case, delete the *Movies.mdf* file again and retry the `update-database` command.</span></span> <span data-ttu-id="4afdb-142">Si sigue recibiendo un error, elimine la carpeta migrations y contenido e inicie con las instrucciones que aparecen en la parte superior de esta página (que es eliminar la *Movies.mdf* de archivos, a continuación, continúe con Enable-Migrations).</span><span class="sxs-lookup"><span data-stu-id="4afdb-142">If you still get an error, delete the migrations folder and contents then start with the instructions at the top of this page (that is delete the *Movies.mdf* file then proceed to Enable-Migrations).</span></span>

<span data-ttu-id="4afdb-143">Ejecute la aplicación y navegue hasta la */Movies* dirección URL.</span><span class="sxs-lookup"><span data-stu-id="4afdb-143">Run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="4afdb-144">Se muestran los datos de inicialización.</span><span class="sxs-lookup"><span data-stu-id="4afdb-144">The seed data is displayed.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image9.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="4afdb-145">Adición de una propiedad de clasificación al modelo Movie</span><span class="sxs-lookup"><span data-stu-id="4afdb-145">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="4afdb-146">Empiece agregando un nuevo `Rating` propiedad existente `Movie` clase.</span><span class="sxs-lookup"><span data-stu-id="4afdb-146">Start by adding a new `Rating` property to the existing `Movie` class.</span></span> <span data-ttu-id="4afdb-147">Abra la *Models\Movie.cs* de archivos y agregar el `Rating` propiedad como esta:</span><span class="sxs-lookup"><span data-stu-id="4afdb-147">Open the *Models\Movie.cs* file and add the `Rating` property like this one:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample3.cs)]

<span data-ttu-id="4afdb-148">La sección completa `Movie` clase ahora parece similar al código siguiente:</span><span class="sxs-lookup"><span data-stu-id="4afdb-148">The complete `Movie` class now looks like the following code:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample4.cs?highlight=8)]

<span data-ttu-id="4afdb-149">Genere la aplicación mediante el **generar** &gt; **crear películas** menú comando o presionando CTRL-MAYÚS-B.</span><span class="sxs-lookup"><span data-stu-id="4afdb-149">Build the application using the **Build** &gt;**Build Movie** menu command or by pressing CTRL-SHIFT-B.</span></span>

<span data-ttu-id="4afdb-150">Ahora que ha actualizado el `Model` (clase), también debe actualizar el *\Views\Movies\Index.cshtml* y *\Views\Movies\Create.cshtml* ver plantillas para mostrar el nuevo `Rating`propiedad en la vista del explorador.</span><span class="sxs-lookup"><span data-stu-id="4afdb-150">Now that you've updated the `Model` class, you also need to update the *\Views\Movies\Index.cshtml* and *\Views\Movies\Create.cshtml* view templates in order to display the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="4afdb-151">Abra la*\Views\Movies\Index.cshtml* de archivos y agregar un `<th>Rating</th>` justo después de encabezado de columna la **precio** columna.</span><span class="sxs-lookup"><span data-stu-id="4afdb-151">Open the*\Views\Movies\Index.cshtml* file and add a `<th>Rating</th>` column heading just after the **Price** column.</span></span> <span data-ttu-id="4afdb-152">A continuación, agregue un `<td>` columna cerca del final de la plantilla para representar la `@item.Rating` valor.</span><span class="sxs-lookup"><span data-stu-id="4afdb-152">Then add a `<td>` column near the end of the template to render the `@item.Rating` value.</span></span> <span data-ttu-id="4afdb-153">A continuación se muestra qué actualizado *Index.cshtml* plantilla de vista es similar:</span><span class="sxs-lookup"><span data-stu-id="4afdb-153">Below is what the updated *Index.cshtml* view template looks like:</span></span>

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample5.cshtml?highlight=26-28,46-48)]

<span data-ttu-id="4afdb-154">A continuación, abra el *\Views\Movies\Create.cshtml* de archivos y agregue el siguiente marcado cerca del final del formulario.</span><span class="sxs-lookup"><span data-stu-id="4afdb-154">Next, open the *\Views\Movies\Create.cshtml* file and add the following markup near the end of the form.</span></span> <span data-ttu-id="4afdb-155">Esto representa un cuadro de texto para que pueda especificar una clasificación cuando se crea una película nuevo.</span><span class="sxs-lookup"><span data-stu-id="4afdb-155">This renders a text box so that you can specify a rating when a new movie is created.</span></span>

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample6.cshtml)]

<span data-ttu-id="4afdb-156">Ahora ha actualizado el código de aplicación para admitir los nuevos `Rating` propiedad.</span><span class="sxs-lookup"><span data-stu-id="4afdb-156">You've now updated the application code to support the new `Rating` property.</span></span>

<span data-ttu-id="4afdb-157">Ahora ejecute la aplicación y navegue hasta la */Movies* dirección URL.</span><span class="sxs-lookup"><span data-stu-id="4afdb-157">Now run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="4afdb-158">Al hacer esto, sin embargo, verá uno de los siguientes errores:</span><span class="sxs-lookup"><span data-stu-id="4afdb-158">When you do this, though, you'll see one of the following errors:</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image10.png)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image11.png)

<span data-ttu-id="4afdb-159">Está viendo este error porque la actualización `Movie` clase de modelo en la aplicación ahora es diferente que el esquema de la `Movie` tabla de la base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="4afdb-159">You're seeing this error because the updated `Movie` model class in the application is now different than the schema of the `Movie` table of the existing database.</span></span> <span data-ttu-id="4afdb-160">(No hay ninguna columna `Rating` en la tabla de la base de datos).</span><span class="sxs-lookup"><span data-stu-id="4afdb-160">(There's no `Rating` column in the database table.)</span></span>


<span data-ttu-id="4afdb-161">Este error se puede resolver de varias maneras:</span><span class="sxs-lookup"><span data-stu-id="4afdb-161">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="4afdb-162">Haga que Entity Framework quite de forma automática la base de datos y la vuelva a crear basándose en el nuevo esquema de la clase del modelo.</span><span class="sxs-lookup"><span data-stu-id="4afdb-162">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="4afdb-163">Este enfoque resulta muy cómoda para realizar el desarrollo activo en una base de datos de prueba; permite evolucione rápidamente el esquema de modelo y la base de datos entre sí.</span><span class="sxs-lookup"><span data-stu-id="4afdb-163">This approach is very convenient when doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="4afdb-164">La desventaja, sin embargo, es que se pierden los datos existentes en la base de datos, por lo que *no* va a utilizar este enfoque en una base de datos de producción.</span><span class="sxs-lookup"><span data-stu-id="4afdb-164">The downside, though, is that you lose existing data in the database — so you *don't* want to use this approach on a production database!</span></span> <span data-ttu-id="4afdb-165">Usando a un inicializador para propagar automáticamente una base de datos con datos de prueba a menudo es una manera productiva al desarrollar una aplicación.</span><span class="sxs-lookup"><span data-stu-id="4afdb-165">Using an initializer to automatically seed a database with test data is often a productive way to develope an application.</span></span> <span data-ttu-id="4afdb-166">Para obtener más información sobre los inicializadores de base de datos de Entity Framework, vea de Tom Dykstra [tutorial de ASP.NET MVC/Entity Framework](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="4afdb-166">For more information on Entity Framework database initializers, see Tom Dykstra's [ASP.NET MVC/Entity Framework tutorial](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>
2. <span data-ttu-id="4afdb-167">Modifique explícitamente el esquema de la base de datos existente para que coincida con las clases del modelo.</span><span class="sxs-lookup"><span data-stu-id="4afdb-167">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="4afdb-168">La ventaja de este enfoque es que se conservan los datos.</span><span class="sxs-lookup"><span data-stu-id="4afdb-168">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="4afdb-169">Puede realizar este cambio de forma manual o mediante la creación de un script de cambio de base de datos.</span><span class="sxs-lookup"><span data-stu-id="4afdb-169">You can make this change either manually or by creating a database change script.</span></span>
3. <span data-ttu-id="4afdb-170">Use Migraciones de Code First para actualizar el esquema de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="4afdb-170">Use Code First Migrations to update the database schema.</span></span>


<span data-ttu-id="4afdb-171">Para este tutorial se usa Migraciones de Code First.</span><span class="sxs-lookup"><span data-stu-id="4afdb-171">For this tutorial, we'll use Code First Migrations.</span></span>

<span data-ttu-id="4afdb-172">Actualizar el método de inicialización para que proporcione un valor para la nueva columna.</span><span class="sxs-lookup"><span data-stu-id="4afdb-172">Update the Seed method so that it provides a value for the new column.</span></span> <span data-ttu-id="4afdb-173">Abra el archivo Migrations\Configuration.cs y agregar un campo de clasificación para cada objeto de la película.</span><span class="sxs-lookup"><span data-stu-id="4afdb-173">Open Migrations\Configuration.cs file and add a Rating field to each Movie object.</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample7.cs?highlight=6)]

<span data-ttu-id="4afdb-174">Compile la solución y, a continuación, abra el **Package Manager Console** ventana y escriba el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="4afdb-174">Build the solution, and then open the **Package Manager Console** window and enter the following command:</span></span>

`add-migration AddRatingMig`

<span data-ttu-id="4afdb-175">El `add-migration` comando indica el marco de trabajo de migración para examinar el modelo de película actual con el esquema actual de la base de datos de la película y crear el código necesario para migrar la base de datos al nuevo modelo.</span><span class="sxs-lookup"><span data-stu-id="4afdb-175">The `add-migration` command tells the migration framework to examine the current movie model with the current movie DB schema and create the necessary code to migrate the DB to the new model.</span></span> <span data-ttu-id="4afdb-176">El AddRatingMig es arbitrario y se utiliza para asignar nombre al archivo de migración.</span><span class="sxs-lookup"><span data-stu-id="4afdb-176">The AddRatingMig is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="4afdb-177">Es útil emplear un nombre descriptivo para el paso de migración.</span><span class="sxs-lookup"><span data-stu-id="4afdb-177">It's helpful to use a meaningful name for the migration step.</span></span>

<span data-ttu-id="4afdb-178">Cuando finaliza este comando, Visual Studio abre el archivo de clase que define la nueva `DbMIgration` clase derivada y en el `Up` método puede ver el código que crea la nueva columna.</span><span class="sxs-lookup"><span data-stu-id="4afdb-178">When this command finishes, Visual Studio opens the class file that defines the new `DbMIgration` derived class, and in the `Up` method you can see the code that creates the new column.</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample8.cs)]

<span data-ttu-id="4afdb-179">Compile la solución y, a continuación, escriba el comando "update-database" en la **Package Manager Console** ventana.</span><span class="sxs-lookup"><span data-stu-id="4afdb-179">Build the solution, and then enter the "update-database" command in the **Package Manager Console** window.</span></span>

<span data-ttu-id="4afdb-180">La siguiente imagen muestra la salida en el **Package Manager Console** ventana (será diferente la marca de fecha anteponiendo AddRatingMig.)</span><span class="sxs-lookup"><span data-stu-id="4afdb-180">The following image shows the output in the **Package Manager Console** window (The date stamp prepending AddRatingMig will be different.)</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image12.png)

<span data-ttu-id="4afdb-181">Vuelva a ejecutar la aplicación y navegue a la dirección URL /Movies.</span><span class="sxs-lookup"><span data-stu-id="4afdb-181">Re-run the application and navigate to the /Movies URL.</span></span> <span data-ttu-id="4afdb-182">Puede ver el nuevo campo de clasificación.</span><span class="sxs-lookup"><span data-stu-id="4afdb-182">You can see the new Rating field.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image13.png)

<span data-ttu-id="4afdb-183">Haga clic en el **crear nuevo** el vínculo para agregar una película nuevo.</span><span class="sxs-lookup"><span data-stu-id="4afdb-183">Click the **Create New** link to add a new movie.</span></span> <span data-ttu-id="4afdb-184">Tenga en cuenta que puede agregar una clasificación.</span><span class="sxs-lookup"><span data-stu-id="4afdb-184">Note that you can add a rating.</span></span>

![7_CreateRioII](adding-a-new-field-to-the-movie-model-and-table/_static/image14.png)

<span data-ttu-id="4afdb-186">Haga clic en **Crear**.</span><span class="sxs-lookup"><span data-stu-id="4afdb-186">Click **Create**.</span></span> <span data-ttu-id="4afdb-187">La película nuevo, incluida la clasificación, se muestra ahora en la lista de películas:</span><span class="sxs-lookup"><span data-stu-id="4afdb-187">The new movie, including the rating, now shows up in the movies listing:</span></span>

![7_ourNewMovie_SM](adding-a-new-field-to-the-movie-model-and-table/_static/image15.png)

<span data-ttu-id="4afdb-189">También debe agregar el `Rating` campo para la edición, detalles e SearchIndex plantillas de vista.</span><span class="sxs-lookup"><span data-stu-id="4afdb-189">You should also add the `Rating` field to the Edit, Details and SearchIndex view templates.</span></span>

<span data-ttu-id="4afdb-190">Puede escribir el comando "update-database" en la **Package Manager Console** volver a la ventana y ningún cambio que se aplicarían, porque el esquema coincide con el modelo.</span><span class="sxs-lookup"><span data-stu-id="4afdb-190">You could enter the "update-database" command in the **Package Manager Console** window again and no changes would be made, because the schema matches the model.</span></span>

<span data-ttu-id="4afdb-191">En esta sección se ha visto cómo puede modificar objetos de modelo y mantenerlos sincronizados con los cambios de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="4afdb-191">In this section you saw how you can modify model objects and keep the database in sync with the changes.</span></span> <span data-ttu-id="4afdb-192">También aprendió una forma de rellenar una base de datos recién creada con datos de ejemplo para probar escenarios.</span><span class="sxs-lookup"><span data-stu-id="4afdb-192">You also learned a way to populate a newly created database with sample data so you can try out scenarios.</span></span> <span data-ttu-id="4afdb-193">A continuación, echemos un vistazo a cómo puede agregar una lógica de validación a las clases del modelo y habilitar algunas reglas de negocios que se aplicará.</span><span class="sxs-lookup"><span data-stu-id="4afdb-193">Next, let's look at how you can add richer validation logic to the model classes and enable some business rules to be enforced.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="4afdb-194">[Anterior](examining-the-edit-methods-and-edit-view.md)
[Siguiente](adding-validation-to-the-model.md)</span><span class="sxs-lookup"><span data-stu-id="4afdb-194">[Previous](examining-the-edit-methods-and-edit-view.md)
[Next](adding-validation-to-the-model.md)</span></span>