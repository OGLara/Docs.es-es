---
title: ASP.NET Core en Nano Server
author: shirhatti
description: "Aprenda a implementar una aplicación existente de ASP.NET Core en una instancia de Nano Server que ejecuta IIS."
keywords: ASP.NET Core,nano server
ms.author: riande
manager: wpickett
ms.date: 11/04/2016
ms.topic: article
ms.assetid: 50922cf1-ca58-4006-9236-99b7ff2dd0cf
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/nano-server
ms.openlocfilehash: f30e911703d5c36d076872f91d4b2fafeefb91f5
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2018
---
# <a name="aspnet-core-with-iis-on-nano-server"></a><span data-ttu-id="46f5b-104">ASP.NET Core con IIS en Nano Server</span><span class="sxs-lookup"><span data-stu-id="46f5b-104">ASP.NET Core with IIS on Nano Server</span></span>

<span data-ttu-id="46f5b-105">Por [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="46f5b-105">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="46f5b-106">En este tutorial tomaremos una aplicación existente de ASP.NET Core y la implementaremos en una instancia de Nano Server que ejecuta IIS.</span><span class="sxs-lookup"><span data-stu-id="46f5b-106">In this tutorial, you'll take an existing ASP.NET Core app and deploy it to a Nano Server instance running IIS.</span></span>

## <a name="introduction"></a><span data-ttu-id="46f5b-107">Introducción</span><span class="sxs-lookup"><span data-stu-id="46f5b-107">Introduction</span></span>

<span data-ttu-id="46f5b-108">Nano Server es una opción de instalación de Windows Server 2016 que ofrece una ocupación mínima, mayor seguridad y un mejor servicio que Server Core o el servidor completo.</span><span class="sxs-lookup"><span data-stu-id="46f5b-108">Nano Server is an installation option in Windows Server 2016, offering a tiny footprint, better security, and better servicing than Server Core or full Server.</span></span> <span data-ttu-id="46f5b-109">Eche un vistazo a la [documentación oficial de Nano Server](https://docs.microsoft.com/windows-server/get-started/getting-started-with-nano-server) para obtener más detalles y vínculos de descarga de las versiones de evaluación de 180 días.</span><span class="sxs-lookup"><span data-stu-id="46f5b-109">Please consult the official [Nano Server documentation](https://docs.microsoft.com/windows-server/get-started/getting-started-with-nano-server) for more details and download links for 180 Days evaluation versions.</span></span> 

<span data-ttu-id="46f5b-110">Existen tres formas sencillas de probar Nano Server.</span><span class="sxs-lookup"><span data-stu-id="46f5b-110">There are three easy ways for you to try out Nano Server.</span></span> <span data-ttu-id="46f5b-111">Cuando inicie sesión con su cuenta de MS:</span><span class="sxs-lookup"><span data-stu-id="46f5b-111">When you sign in with your MS account:</span></span>

1. <span data-ttu-id="46f5b-112">Puede descargar el archivo ISO de Windows Server 2016 y crear una imagen de Nano Server.</span><span class="sxs-lookup"><span data-stu-id="46f5b-112">You can download the Windows Server 2016 ISO file and build a Nano Server image.</span></span>

2. <span data-ttu-id="46f5b-113">Descargue el disco duro virtual de Nano Server.</span><span class="sxs-lookup"><span data-stu-id="46f5b-113">Download the Nano Server VHD.</span></span>

3. <span data-ttu-id="46f5b-114">Cree una máquina virtual en Azure con la imagen de Nano Server de la Galería de Azure.</span><span class="sxs-lookup"><span data-stu-id="46f5b-114">Create a VM in Azure using the Nano Server image in the Azure Gallery.</span></span> <span data-ttu-id="46f5b-115">Si no tiene una cuenta de Azure, puede obtener una versión de prueba gratuita de 30 días.</span><span class="sxs-lookup"><span data-stu-id="46f5b-115">If you don’t have an Azure account, you can get a free 30-day trial.</span></span>

<span data-ttu-id="46f5b-116">En este tutorial, vamos a usar la segunda opción, el disco duro virtual de Nano Server compilado previamente desde Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="46f5b-116">In this tutorial, we will be using the 2nd option, the pre-built Nano Server VHD from Windows Server 2016.</span></span>

<span data-ttu-id="46f5b-117">Antes de continuar con este tutorial, necesitará la [salida publicada](xref:host-and-deploy/directory-structure) de una aplicación existente de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="46f5b-117">Before proceeding with this tutorial, you will need the [published output](xref:host-and-deploy/directory-structure) of an existing ASP.NET Core application.</span></span> <span data-ttu-id="46f5b-118">Asegúrese de que la aplicación está compilada para ejecutarse en un proceso de **64 bits**.</span><span class="sxs-lookup"><span data-stu-id="46f5b-118">Ensure your application is built to run in a **64-bit** process.</span></span>

## <a name="setting-up-the-nano-server-instance"></a><span data-ttu-id="46f5b-119">Configuración de la instancia de Nano Server</span><span class="sxs-lookup"><span data-stu-id="46f5b-119">Setting up the Nano Server instance</span></span>

<span data-ttu-id="46f5b-120">[Cree una nueva máquina virtual mediante Hyper-V](https://technet.microsoft.com/library/hh846766.aspx) en su máquina de desarrollo usando el disco duro virtual descargado anteriormente.</span><span class="sxs-lookup"><span data-stu-id="46f5b-120">[Create a new Virtual Machine using Hyper-V](https://technet.microsoft.com/library/hh846766.aspx) on your development machine using the previously downloaded VHD.</span></span> <span data-ttu-id="46f5b-121">La máquina le pedirá que establezca una contraseña de administrador antes de iniciar sesión.</span><span class="sxs-lookup"><span data-stu-id="46f5b-121">The machine will require you to set an administrator password before logging on.</span></span> <span data-ttu-id="46f5b-122">En la consola de VM, presione F11 para establecer la contraseña antes del primer inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="46f5b-122">At the VM console, press F11 to set the password before the first log in.</span></span> <span data-ttu-id="46f5b-123">También necesita comprobar la dirección IP de la nueva máquina virtual. Para ello compruebe la dirección IP fija del servidor DHCP proporcionada al aprovisionar la máquina virtual o compruebe la configuración de red de la consola de recuperación de Nano Server.</span><span class="sxs-lookup"><span data-stu-id="46f5b-123">You also need to check your new VM's IP address either my checking your DHCP server's fixed IP supplied while provisioning your VM or in Nano Server recovery console's networking settings.</span></span>

> [!NOTE]
> <span data-ttu-id="46f5b-124">Supongamos que la nueva máquina virtual se ejecuta con la dirección IP V4 local 192.168.1.10.</span><span class="sxs-lookup"><span data-stu-id="46f5b-124">Let's assume your new VM runs with the local V4 IP address 192.168.1.10.</span></span>

<span data-ttu-id="46f5b-125">Ahora puede administrarla mediante la comunicación remota de PowerShell, que es la única manera de administrar por completo Nano Server.</span><span class="sxs-lookup"><span data-stu-id="46f5b-125">Now you're able to manage it using PowerShell remoting, which is the only way to fully administer your Nano Server.</span></span>

## <a name="connecting-to-your-nano-server-instance-using-powershell-remoting"></a><span data-ttu-id="46f5b-126">Conectarse a la instancia de Nano Server usando la comunicación remota de PowerShell</span><span class="sxs-lookup"><span data-stu-id="46f5b-126">Connecting to your Nano Server instance using PowerShell Remoting</span></span>

<span data-ttu-id="46f5b-127">Abra una ventana de PowerShell con privilegios elevados para agregar la instancia remota de Nano Server a la lista `TrustedHosts`.</span><span class="sxs-lookup"><span data-stu-id="46f5b-127">Open an elevated PowerShell window to add your remote Nano Server instance to your `TrustedHosts` list.</span></span>

```PowerShell
$nanoServerIpAddress = "192.168.1.10"
Set-Item WSMan:\localhost\Client\TrustedHosts "$nanoServerIpAddress" -Concatenate -Force
```

> [!NOTE]
> <span data-ttu-id="46f5b-128">Reemplace la variable `$nanoServerIpAddress` con la dirección IP correcta.</span><span class="sxs-lookup"><span data-stu-id="46f5b-128">Replace the variable `$nanoServerIpAddress` with the correct IP address.</span></span>

<span data-ttu-id="46f5b-129">Una vez que haya agregado la instancia de Nano Server a `TrustedHosts`, puede conectarse a ella mediante la comunicación remota de PowerShell.</span><span class="sxs-lookup"><span data-stu-id="46f5b-129">Once you have added your Nano Server instance to your `TrustedHosts`, you can connect to it using PowerShell remoting.</span></span>

```PowerShell
$nanoServerSession = New-PSSession -ComputerName $nanoServerIpAddress -Credential ~\Administrator
Enter-PSSession $nanoServerSession
```

<span data-ttu-id="46f5b-130">Cuando se realiza una conexión correcta, se muestra un símbolo del sistema parecido a este:`[192.168.1.10]: PS C:\Users\Administrator\Documents>`</span><span class="sxs-lookup"><span data-stu-id="46f5b-130">A successful connection results in a prompt with a format looking like: `[192.168.1.10]: PS C:\Users\Administrator\Documents>`</span></span>

## <a name="creating-a-file-share"></a><span data-ttu-id="46f5b-131">Crear un recurso compartido de archivos</span><span class="sxs-lookup"><span data-stu-id="46f5b-131">Creating a file share</span></span>

<span data-ttu-id="46f5b-132">Cree un recurso compartido de archivos en Nano Server para que la aplicación publicada pueda copiarse en él.</span><span class="sxs-lookup"><span data-stu-id="46f5b-132">Create a file share on the Nano server so that the published application can be copied to it.</span></span> <span data-ttu-id="46f5b-133">Ejecute los siguientes comandos en la sesión remota:</span><span class="sxs-lookup"><span data-stu-id="46f5b-133">Run the following commands in the remote session:</span></span>

```PowerShell
New-Item C:\PublishedApps\AspNetCoreSampleForNano -type directory
netsh advfirewall firewall set rule group="File and Printer Sharing" new enable=yes
net share AspNetCoreSampleForNano=c:\PublishedApps\AspNetCoreSampleForNano /GRANT:EVERYONE`,FULL
```

<span data-ttu-id="46f5b-134">Después de ejecutar los comandos anteriores, debería tener acceso a este recurso compartido si visita `\\192.168.1.10\AspNetCoreSampleForNano` en el Explorador de Windows de la máquina host.</span><span class="sxs-lookup"><span data-stu-id="46f5b-134">After running the above commands, you should be able to access this share by visiting `\\192.168.1.10\AspNetCoreSampleForNano` in the host machine's Windows Explorer.</span></span>

## <a name="open-port-in-the-firewall"></a><span data-ttu-id="46f5b-135">Abrir el puerto en el firewall</span><span class="sxs-lookup"><span data-stu-id="46f5b-135">Open port in the firewall</span></span>

<span data-ttu-id="46f5b-136">Ejecute los siguientes comandos en la sesión remota para abrir un puerto en el firewall a fin de permitir que IIS escuche el tráfico TCP en el puerto TCP/8000.</span><span class="sxs-lookup"><span data-stu-id="46f5b-136">Run the following commands in the remote session to open up a port in the firewall to let IIS listen for TCP traffic on port TCP/8000.</span></span>

```PowerShell
New-NetFirewallRule -Name "AspNet5 IIS" -DisplayName "Allow HTTP on TCP/8000" -Protocol TCP -LocalPort 8000 -Action Allow -Enabled True
```

## <a name="installing-iis"></a><span data-ttu-id="46f5b-137">Instalar IIS</span><span class="sxs-lookup"><span data-stu-id="46f5b-137">Installing IIS</span></span>

<span data-ttu-id="46f5b-138">Agregue el proveedor `NanoServerPackage` desde la Galería de PowerShell.</span><span class="sxs-lookup"><span data-stu-id="46f5b-138">Add the `NanoServerPackage` provider from the PowerShell Gallery.</span></span> <span data-ttu-id="46f5b-139">Cuando se haya instalado e importado el proveedor, podrá instalar los paquetes de Windows.</span><span class="sxs-lookup"><span data-stu-id="46f5b-139">Once the provider is installed and imported, you can install Windows packages.</span></span>

<span data-ttu-id="46f5b-140">En la sesión de PowerShell que creó anteriormente, ejecute los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="46f5b-140">Run the following commands in the PowerShell session that was created earlier:</span></span>

```PowerShell
Install-PackageProvider NanoServerPackage
Import-PackageProvider NanoServerPackage
Install-NanoServerPackage -Name Microsoft-NanoServer-Storage-Package
Install-NanoServerPackage -Name Microsoft-NanoServer-IIS-Package
```

<span data-ttu-id="46f5b-141">Para comprobar rápidamente si IIS está configurado correctamente, puede visitar la dirección URL `http://192.168.1.10/`, en la que debería ver una página de bienvenida.</span><span class="sxs-lookup"><span data-stu-id="46f5b-141">To quickly verify if IIS is setup correctly, you can visit the URL `http://192.168.1.10/` and should see a welcome page.</span></span> <span data-ttu-id="46f5b-142">Cuando se instala IIS, se crea de manera predeterminada un sitio web denominado `Default Web Site` que escucha en el puerto 80.</span><span class="sxs-lookup"><span data-stu-id="46f5b-142">When IIS is installed, a website called `Default Web Site` listening on port 80 is created by default.</span></span>

## <a name="installing-the-aspnet-core-module-ancm"></a><span data-ttu-id="46f5b-143">Instalar el módulo de ASP.NET Core (ANCM)</span><span class="sxs-lookup"><span data-stu-id="46f5b-143">Installing the ASP.NET Core Module (ANCM)</span></span>

<span data-ttu-id="46f5b-144">El módulo de ASP.NET Core es un módulo de IIS 7.5+ que se encarga de la administración de procesos de las escuchas HTTP de ASP.NET Core y de enviar mediante proxy las solicitudes a los procesos que administra.</span><span class="sxs-lookup"><span data-stu-id="46f5b-144">The ASP.NET Core Module is an IIS 7.5+ module which is responsible for process management of ASP.NET Core HTTP listeners and to proxy requests to processes that it manages.</span></span> <span data-ttu-id="46f5b-145">En este momento, el proceso para instalar el módulo de ASP.NET Core para IIS es manual.</span><span class="sxs-lookup"><span data-stu-id="46f5b-145">At the moment, the process to install the ASP.NET Core Module for IIS is manual.</span></span> <span data-ttu-id="46f5b-146">Debe instalar el [conjunto de productos de hospedaje de Windows Server de .NET Core](https://download.microsoft.com/download/B/1/D/B1D7D5BF-3920-47AA-94BD-7A6E48822F18/DotNetCore.2.0.0-WindowsHosting.exe) en una máquina normal (que no sea Nano).</span><span class="sxs-lookup"><span data-stu-id="46f5b-146">You will need to install the [.NET Core Windows Server Hosting bundle](https://download.microsoft.com/download/B/1/D/B1D7D5BF-3920-47AA-94BD-7A6E48822F18/DotNetCore.2.0.0-WindowsHosting.exe) on a regular (not Nano) machine.</span></span> <span data-ttu-id="46f5b-147">Después de instalar el conjunto de productos en una máquina normal, debe copiar los siguientes archivos en el recurso compartido de archivos que hemos creado con anterioridad.</span><span class="sxs-lookup"><span data-stu-id="46f5b-147">After installing the bundle on a regular machine, you will need to copy the following files to the file share that we created earlier.</span></span>

<span data-ttu-id="46f5b-148">En un servidor normal (que no sea Nano) con IIS, ejecute los siguientes comandos de copia:</span><span class="sxs-lookup"><span data-stu-id="46f5b-148">On a regular (not Nano) server with IIS, run the following copy commands:</span></span>

```PowerShell
Copy-Item -Path  C:\windows\system32\inetsrv\aspnetcore.dll -Destination `\\<nanoserver-ip-address>\AspNetCoreSampleForNano`
Copy-Item -Path  C:\windows\system32\inetsrv\config\schema\aspnetcore_schema.xml -Destination `\\<nanoserver-ip-address>\AspNetCoreSampleForNano`
```

<span data-ttu-id="46f5b-149">Reemplace `C:\windows\system32\inetsrv` con `C:\Program Files\IIS Express` en una máquina con Windows 10.</span><span class="sxs-lookup"><span data-stu-id="46f5b-149">Replace `C:\windows\system32\inetsrv` with `C:\Program Files\IIS Express` on a Windows 10 machine.</span></span>

<span data-ttu-id="46f5b-150">En el lado Nano, debe copiar los siguientes archivos desde el recurso compartido de archivos que hemos creado anteriormente a las ubicaciones válidas.</span><span class="sxs-lookup"><span data-stu-id="46f5b-150">On the Nano side, you will need to copy the following files from the file share that we created earlier to the valid locations.</span></span> <span data-ttu-id="46f5b-151">Ejecute los siguientes comandos de copia:</span><span class="sxs-lookup"><span data-stu-id="46f5b-151">So, run the following copy commands:</span></span>

```PowerShell
Copy-Item -Path C:\PublishedApps\AspNetCoreSampleForNano\aspnetcore.dll -Destination C:\windows\system32\inetsrv\
Copy-Item -Path C:\PublishedApps\AspNetCoreSampleForNano\aspnetcore_schema.xml -Destination C:\windows\system32\inetsrv\config\schema\
```

<span data-ttu-id="46f5b-152">Ejecute el siguiente script en la sesión remota:</span><span class="sxs-lookup"><span data-stu-id="46f5b-152">Run the following script in the remote session:</span></span>

```PowerShell
# Backup existing applicationHost.config
Copy-Item -Path C:\Windows\System32\inetsrv\config\applicationHost.config -Destination  C:\Windows\System32\inetsrv\config\applicationHost_BeforeInstallingANCM.config

Import-Module IISAdministration

# Initialize variables
$aspNetCoreHandlerFilePath="C:\windows\system32\inetsrv\aspnetcore.dll"
Reset-IISServerManager -confirm:$false
$sm = Get-IISServerManager

# Add AppSettings section 
$sm.GetApplicationHostConfiguration().RootSectionGroup.Sections.Add("appSettings")

# Set Allow for handlers section
$appHostconfig = $sm.GetApplicationHostConfiguration()
$section = $appHostconfig.GetSection("system.webServer/handlers")
$section.OverrideMode="Allow"

# Add aspNetCore section to system.webServer
$sectionaspNetCore = $appHostConfig.RootSectionGroup.SectionGroups["system.webServer"].Sections.Add("aspNetCore")
$sectionaspNetCore.OverrideModeDefault = "Allow"
$sm.CommitChanges()

# Configure globalModule
Reset-IISServerManager -confirm:$false
$globalModules = Get-IISConfigSection "system.webServer/globalModules" | Get-IISConfigCollection
New-IISConfigCollectionElement $globalModules -ConfigAttribute @{"name"="AspNetCoreModule";"image"=$aspNetCoreHandlerFilePath}

# Configure module
$modules = Get-IISConfigSection "system.webServer/modules" | Get-IISConfigCollection
New-IISConfigCollectionElement $modules -ConfigAttribute @{"name"="AspNetCoreModule"}
```

> [!NOTE]
> <span data-ttu-id="46f5b-153">Elimine los archivos *aspnetcore.dll* y *aspnetcore_schema.xml* del recurso compartido después del paso anterior.</span><span class="sxs-lookup"><span data-stu-id="46f5b-153">Delete the files *aspnetcore.dll* and *aspnetcore_schema.xml* from the share after the above step.</span></span>

## <a name="installing-net-core-framework"></a><span data-ttu-id="46f5b-154">Instalar .NET Framework</span><span class="sxs-lookup"><span data-stu-id="46f5b-154">Installing .NET Core Framework</span></span>

<span data-ttu-id="46f5b-155">Si su aplicación se publica como una [implementación dependiente de Framework (FDD)](/dotnet/core/deploying/#framework-dependent-deployments-fdd), debe instalarse .NET Core en el servidor.</span><span class="sxs-lookup"><span data-stu-id="46f5b-155">If your app is published as a [framework-dependent deployment (FDD)](/dotnet/core/deploying/#framework-dependent-deployments-fdd), .NET Core must be installed on the server.</span></span> <span data-ttu-id="46f5b-156">Use el [script de PowerShell dotnet-install.ps1](https://dot.net/v1/dotnet-install.ps1) en una sesión remota de PowerShell para instalar .NET Core en su servidor de Nano Server.</span><span class="sxs-lookup"><span data-stu-id="46f5b-156">Use the [dotnet-install.ps1 PowerShell script](https://dot.net/v1/dotnet-install.ps1) in a remote PowerShell session to install .NET Core on your Nano Server.</span></span> <span data-ttu-id="46f5b-157">Pase la versión de la CLI con el modificador `-Version`:</span><span class="sxs-lookup"><span data-stu-id="46f5b-157">Pass the CLI version with the `-Version` switch:</span></span>

```console
dotnet-install.ps1 -Version 2.0.0
```

## <a name="publishing-the-application"></a><span data-ttu-id="46f5b-158">Publicar la aplicación</span><span class="sxs-lookup"><span data-stu-id="46f5b-158">Publishing the application</span></span>

<span data-ttu-id="46f5b-159">Copie el resultado publicado de la aplicación existente en la raíz del recurso compartido de archivos.</span><span class="sxs-lookup"><span data-stu-id="46f5b-159">Copy the published output of your existing application to the file share's root.</span></span>

<span data-ttu-id="46f5b-160">Puede que necesite realizar cambios en *web.config* para que apunte a la ubicación donde extrajo *dotnet.exe*.</span><span class="sxs-lookup"><span data-stu-id="46f5b-160">You may need to make changes to your *web.config* to point to where you extracted *dotnet.exe*.</span></span> <span data-ttu-id="46f5b-161">Como alternativa, puede agregar *dotnet.exe* a la ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="46f5b-161">Alternatively, you can add *dotnet.exe* to your PATH.</span></span>

<span data-ttu-id="46f5b-162">Ejemplo del aspecto que tendría un archivo *web.config* si *dotnet.exe* **no** estuviera en la ruta de acceso:</span><span class="sxs-lookup"><span data-stu-id="46f5b-162">Example of how a *web.config* might look if *dotnet.exe* is **not** on the PATH:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="C:\dotnet\dotnet.exe" arguments=".\AspNetCoreSampleForNano.dll" stdoutLogEnabled="false" stdoutLogFile=".\logs\aspnetcore-stdout" forwardWindowsAuthToken="true" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="46f5b-163">Ejecute los siguientes comandos en la sesión remota para crear un nuevo sitio en IIS para la aplicación publicada en un puerto diferente del sitio web predeterminado.</span><span class="sxs-lookup"><span data-stu-id="46f5b-163">Run the following commands in the remote session to create a new site in IIS for the published app on a different port than the default website.</span></span> <span data-ttu-id="46f5b-164">También debe abrir ese puerto para tener acceso a la web.</span><span class="sxs-lookup"><span data-stu-id="46f5b-164">You also need to open that port to access the web.</span></span> <span data-ttu-id="46f5b-165">Este script usa `DefaultAppPool` para simplificar el trabajo.</span><span class="sxs-lookup"><span data-stu-id="46f5b-165">This script uses the `DefaultAppPool` for simplicity.</span></span> <span data-ttu-id="46f5b-166">Para más información sobre la ejecución en un grupo de aplicaciones, vea [Grupos de aplicaciones](xref:host-and-deploy/iis/index#application-pools).</span><span class="sxs-lookup"><span data-stu-id="46f5b-166">For more considerations on running under an application pool, see [Application Pools](xref:host-and-deploy/iis/index#application-pools).</span></span>

```PowerShell
Import-module IISAdministration
New-IISSite -Name "AspNetCore" -PhysicalPath c:\PublishedApps\AspNetCoreSampleForNano -BindingInformation "*:8000:"
```

## <a name="known-issue-running-net-core-cli-on-nano-server-and-workaround"></a><span data-ttu-id="46f5b-167">Problema conocido al ejecutar la interfaz de la línea de comandos de .NET Core en Nano Server y solución</span><span class="sxs-lookup"><span data-stu-id="46f5b-167">Known issue running .NET Core CLI on Nano Server and workaround</span></span>
```PowerShell
New-NetFirewallRule -Name "AspNetCore Port 81 IIS" -DisplayName "Allow HTTP on TCP/81" -Protocol TCP -LocalPort 81 -Action Allow -Enabled True
```

## <a name="running-the-application"></a><span data-ttu-id="46f5b-168">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="46f5b-168">Running the application</span></span>

<span data-ttu-id="46f5b-169">Se puede acceder a la aplicación web publicada en un explorador en `http://192.168.1.10:8000`.</span><span class="sxs-lookup"><span data-stu-id="46f5b-169">The published web app is accessible in a browser at `http://192.168.1.10:8000`.</span></span> <span data-ttu-id="46f5b-170">Si ha configurado el registro como se describe en [Creación y redirección de registros](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection), puede ver los registros en *C:\PublishedApps\AspNetCoreSampleForNano\logs*.</span><span class="sxs-lookup"><span data-stu-id="46f5b-170">If you've set up logging as described in [Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection), you can view your logs at *C:\PublishedApps\AspNetCoreSampleForNano\logs*.</span></span>