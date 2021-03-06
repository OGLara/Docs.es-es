---
title: Hospedaje e implementación de ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo configurar entornos de hospedaje e implementar aplicaciones de ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 08/07/2017
uid: host-and-deploy/index
ms.openlocfilehash: e62b68c4cfad29bb8bea3b9fbb2c231a4afeccea
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095673"
---
# <a name="host-and-deploy-aspnet-core"></a>Hospedaje e implementación de ASP.NET Core

En general, para implementar una aplicación de ASP.NET Core en un entorno de hospedaje:

* Publique la aplicación en una carpeta del servidor de hospedaje.
* Configure un administrador de procesos que inicie la aplicación cuando lleguen las solicitudes y la reinicie si se bloquea o si se reinicia el servidor.
* Si desea la configuración de un proxy inverso, configure un proxy inverso que reenvíe las solicitudes a la aplicación.

## <a name="publish-to-a-folder"></a>Publicar la aplicación en una carpeta

El comando de la CLI [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) compila el código de la aplicación y copia los archivos necesarios para ejecutar la aplicación en una carpeta *publish*. Al efectuar una implementación desde Visual Studio, el paso del comando [dotnet publish](/dotnet/core/tools/dotnet-publish) se produce automáticamente antes de que los archivos se copien en el destino de implementación.

### <a name="folder-contents"></a>Contenido de la carpeta

La carpeta *publish* contiene archivos *.exe* y *.dll* de la aplicación, sus dependencias y, de forma opcional, el tiempo de ejecución .NET.

Se puede publicar una aplicación .NET Core como *independiente* o *dependiente del marco*. Si la aplicación es independiente, los archivos *.dll* que contienen el tiempo de ejecución .NET se incluyen en la carpeta *publish*. Si la aplicación es dependiente del marco, los archivos del tiempo de ejecución .NET no se incluyen porque la aplicación tiene una referencia a una versión de .NET que está instalada en el servidor. El modelo de implementación predeterminado es dependiente del marco. Para más información, vea [Implementación de aplicaciones .NET Core](/dotnet/articles/core/deploying/index).

Además de los archivos *.exe* y *.dll*, la carpeta *publish* de una aplicación ASP.NET Core suele contener archivos de configuración, recursos estáticos y vistas de MVC. Para más información, vea [Directory structure](xref:host-and-deploy/directory-structure) (Estructura de directorios).

## <a name="set-up-a-process-manager"></a>Configurar un administrador de procesos

Una aplicación de ASP.NET Core es una aplicación de consola que se debe iniciar cuando se inicia un servidor y se debe reiniciar si este se bloquea. Para automatizar los inicios y los reinicios, se necesita un administrador de procesos. Los administradores de procesos más comunes para ASP.NET Core son:

* Linux
  * [Nginx](xref:host-and-deploy/linux-nginx)
  * [Apache](xref:host-and-deploy/linux-apache)
* Windows
  * [IIS](xref:host-and-deploy/iis/index)
  * [Servicio de Windows](xref:host-and-deploy/windows-service)

## <a name="set-up-a-reverse-proxy"></a>Configurar un proxy inverso

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Si la aplicación usa el servidor web [Kestrel](xref:fundamentals/servers/kestrel), puede usar [Nginx](xref:host-and-deploy/linux-nginx), [Apache](xref:host-and-deploy/linux-apache) o [IIS](xref:host-and-deploy/iis/index) como servidor proxy inverso. Un servidor proxy inverso recibe las solicitudes HTTP de Internet y las reenvía a Kestrel después de un control preliminar.

Cualquiera de las configuraciones&mdash;con o sin un servidor proxy inverso&mdash;es una configuración de hospedaje válida y admitida para ASP.NET 2.0 o aplicaciones posteriores. Para más información, vea [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy) (Cuándo se debe usar Kestrel con un proxy inverso).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Si la aplicación usa el servidor web [Kestrel](xref:fundamentals/servers/kestrel) y se verá expuesto a Internet, debe usar [Nginx](xref:host-and-deploy/linux-nginx), [Apache](xref:host-and-deploy/linux-apache) o [IIS](xref:host-and-deploy/iis/index) como servidor proxy inverso. Un servidor proxy inverso recibe las solicitudes HTTP de Internet y las reenvía a Kestrel después de un control preliminar. La razón principal por la que debe usar un proxy inverso es la seguridad. Para más información, vea [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel?tabs=aspnetcore1x#when-to-use-kestrel-with-a-reverse-proxy) (Cuándo se debe usar Kestrel con un proxy inverso).

---

## <a name="proxy-server-and-load-balancer-scenarios"></a>Escenarios de servidor proxy y equilibrador de carga

Podría ser necesario realizar una configuración adicional para las aplicaciones hospedadas detrás de servidores proxy y equilibradores de carga. Sin una configuración adicional, una aplicación podría no tener acceso al esquema (HTTP/HTTPS) y la dirección IP remota donde se originó una solicitud. Para más información, vea [Configurar ASP.NET Core para trabajar con servidores proxy y equilibradores de carga](xref:host-and-deploy/proxy-load-balancer).

## <a name="using-visual-studio-and-msbuild-to-automate-deployment"></a>Usar Visual Studio y MSBuild para automatizar la implementación

La implementación a menudo requiere tareas adicionales además de copiar el resultado del comando [dotnet publish](/dotnet/core/tools/dotnet-publish) en un servidor. Por ejemplo, podrían necesitarse o eliminarse archivos adicionales de la carpeta *publish*. Para la implementación web, Visual Studio usa MSBuild, que puede personalizar de modo que lleve a cabo muchas otras tareas durante la implementación. Para más información, vea [Publish profiles in Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) (Publicar perfiles en Visual Studio) y el libro [Using MSBuild and Team Foundation Build](http://msbuildbook.com/) (Usar MSBuild y Team Foundation Build).

Mediante la [característica de publicación web](xref:tutorials/publish-to-azure-webapp-using-vs) o la [compatibilidad integrada con Git](xref:host-and-deploy/azure-apps/azure-continuous-deployment), puede implementar las aplicaciones directamente desde Visual Studio en Azure App Service. Visual Studio Team Services es compatible con la [implementación continua en Azure App Service](/vsts/build-release/apps/cd/azure/aspnet-core-to-azure-webapp?tabs=vsts).

## <a name="publishing-to-azure"></a>Publicación en Azure

Vea [Publicar una aplicación web de ASP.NET Core en Azure App Service con Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) para obtener instrucciones sobre cómo publicar una aplicación en Azure con Visual Studio. También se puede publicar la aplicación en Azure desde la [línea de comandos](xref:tutorials/publish-to-azure-webapp-using-cli).

## <a name="host-in-a-web-farm"></a>Hospedaje en una granja de servidores web

Para obtener más información sobre la configuración del hospedaje de aplicaciones de ASP.NET Core en un entorno de granja de servidores web (por ejemplo, para implementar varias instancias de la aplicación para escalabilidad), consulte <xref:host-and-deploy/web-farm>.

## <a name="additional-resources"></a>Recursos adicionales

Para información sobre cómo usar Docker como entorno de hospedaje, vea [Host ASP.NET Core apps in Docker](xref:host-and-deploy/docker/index) (Hospedar aplicaciones de ASP.NET Core en Docker).
