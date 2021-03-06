---
title: Cifrado de claves en reposo en ASP.NET Core
author: rick-anderson
description: Obtenga información acerca de los detalles de implementación del cifrado de clave de protección de datos de ASP.NET Core en reposo.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-encryption-at-rest
ms.openlocfilehash: c733540bbee2d48ab45cf2b230b7be1ee07fb146
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274705"
---
# <a name="key-encryption-at-rest-in-aspnet-core"></a>Cifrado de claves en reposo en ASP.NET Core

<a name="data-protection-implementation-key-encryption-at-rest"></a>

De forma predeterminada, el sistema de protección de datos [emplea un método heurístico](xref:security/data-protection/configuration/default-settings) para determinar cómo criptográfico material de clave debe cifrarse en reposo. El desarrollador puede invalidar la heurística y especificar manualmente cómo se deben cifrar las claves en reposo.

> [!NOTE]
> Si especifica un cifrado de clave explícita en el mecanismo de rest, el sistema de protección de datos se anular el registro el mecanismo de almacenamiento de claves predeterminado que proporciona la heurística. Debe [especifican un mecanismo de almacenamiento de claves explícitas](xref:security/data-protection/implementation/key-storage-providers#data-protection-implementation-key-storage-providers), en caso contrario, no podrá iniciar el sistema de protección de datos.

<a name="data-protection-implementation-key-encryption-at-rest-providers"></a>

El sistema de protección de datos que se suministra con tres mecanismos de cifrado de claves de forma predeterminada.

## <a name="windows-dpapi"></a>DPAPI de Windows

*Este mecanismo solo está disponible en Windows.*

Cuando se utiliza DPAPI de Windows, material de clave se cifrará a través de [CryptProtectData](https://msdn.microsoft.com/library/windows/desktop/aa380261(v=vs.85).aspx) antes de que se conservan en el almacenamiento. DPAPI es un mecanismo de cifrado adecuado para los datos que nunca se leerán fuera de la máquina actual (aunque es posible hacer copia de estas claves en Active Directory; vea [DPAPI y perfiles móviles](https://support.microsoft.com/kb/309408/#6)). Por ejemplo configurar el cifrado de clave en el resto DPAPI.

```csharp
sc.AddDataProtection()
    // only the local user account can decrypt the keys
    .ProtectKeysWithDpapi();
```

Si `ProtectKeysWithDpapi` se llaman sin ningún parámetro, sólo la cuenta de usuario de Windows actual puede descifrar el material de clave persistente. Opcionalmente, puede especificar que cualquier cuenta de usuario en el equipo (no solo la cuenta de usuario actual) debe ser capaz de descifrar el material de clave, como se muestra en el ejemplo siguiente.

```csharp
sc.AddDataProtection()
    // all user accounts on the machine can decrypt the keys
    .ProtectKeysWithDpapi(protectToLocalMachine: true);
```

## <a name="x509-certificate"></a>Certificado X.509

*Este mecanismo no está disponible en `.NET Core 1.0` o `1.1`.*

Si la aplicación se reparte entre varias máquinas, puede ser conveniente para distribuir un certificado X.509 compartido entre las máquinas y configurar aplicaciones para usar este certificado para el cifrado de claves en reposo. Vea a continuación para obtener un ejemplo.

```csharp
sc.AddDataProtection()
    // searches the cert store for the cert with this thumbprint
    .ProtectKeysWithCertificate("3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0");
```

Debido a limitaciones de .NET Framework se admiten sólo los certificados con claves privadas de CAPI. Vea [basada en certificados de cifrado con Windows DPAPI-NG](#data-protection-implementation-key-encryption-at-rest-dpapi-ng) a continuación para buscar posibles soluciones para estas limitaciones.

<a name="data-protection-implementation-key-encryption-at-rest-dpapi-ng"></a>

## <a name="windows-dpapi-ng"></a>Windows DPAPI-NG

*Este mecanismo solo está disponible en Windows 8 / Windows Server 2012 y versiones posterior.*

A partir de Windows 8, el sistema operativo admite DPAPI-NG (también denominado CNG DPAPI). Microsoft distribuye su escenario de uso como se indica a continuación.

   Informática en nube, sin embargo, a menudo requiere que ese contenido cifrado en un equipo pueden descifrar en otro. Por lo tanto, a partir de Windows 8, Microsoft ampliado la idea de usar una API relativamente sencilla para abarcar escenarios de nube. Esta nueva API, denominada DPAPI-NG, permite compartir de forma segura secretos (claves, contraseñas, material de clave) y mensajes protegiéndolos a un conjunto de entidades de seguridad que puede utilizarse para desproteger en equipos diferentes después de la autorización y la autenticación correcta.

   Desde [sobre DPAPI CNG](https://msdn.microsoft.com/library/windows/desktop/hh706794(v=vs.85).aspx)

La entidad de seguridad se codifica como una regla de descriptor de protección. Considere el ejemplo siguiente, que cifra el material de clave de modo que solo el usuario unido al dominio con el SID especificado puede descifrar el material de clave.

```csharp
sc.AddDataProtection()
    // uses the descriptor rule "SID=S-1-5-21-..."
    .ProtectKeysWithDpapiNG("SID=S-1-5-21-...",
    flags: DpapiNGProtectionDescriptorFlags.None);
```

También hay una sobrecarga sin parámetros de `ProtectKeysWithDpapiNG`. Se trata de un método útil para especificar la regla "SID = mío", donde la extraiga es el SID de la cuenta de usuario de Windows actual.

```csharp
sc.AddDataProtection()
    // uses the descriptor rule "SID={current account SID}"
    .ProtectKeysWithDpapiNG();
```

En este escenario, el controlador de dominio de AD es responsable de distribuir las claves de cifrado que se utiliza en las operaciones de NG DPAPI. El usuario de destino podrá descifrar la carga cifrada desde cualquier equipo unido al dominio (siempre que el proceso se ejecuta bajo su identidad).

## <a name="certificate-based-encryption-with-windows-dpapi-ng"></a>Cifrado basada en certificados con Windows DPAPI-NG

Si se está ejecutando en Windows 8.1 / Windows Server 2012 R2 o versiones posteriores, puede usar Windows DPAPI-NG para realizar el cifrado basada en certificados, incluso si la aplicación se ejecuta en .NET Core. Para aprovechar estas características, utilice la cadena de descriptor de la regla "certificado = HashId:thumbprint", donde la huella digital es la huella digital con codificación hexadecimal SHA1 del certificado que se va a usar. Vea a continuación para obtener un ejemplo.

```csharp
sc.AddDataProtection()
    // searches the cert store for the cert with this thumbprint
    .ProtectKeysWithDpapiNG("CERTIFICATE=HashId:3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0",
        flags: DpapiNGProtectionDescriptorFlags.None);
```

Cualquier aplicación que se señala en este repositorio debe ejecutarse en Windows 8.1 / Windows Server 2012 R2 o posterior para que pueda descifrar esta clave.

## <a name="custom-key-encryption"></a>Cifrado de clave personalizado

Si no resultan adecuados los mecanismos de forma predeterminada, el programador puede especificar su propio mecanismo de cifrado de claves proporcionando un personalizado `IXmlEncryptor`.
