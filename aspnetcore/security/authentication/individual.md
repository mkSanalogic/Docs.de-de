---
title: Artikel basierend auf ASP.NET Core-Projekte mit Konten einzelner Benutzer erstellt
author: rick-anderson
description: Ermitteln Sie Artikel basierend auf ASP.NET Core-Projekte mit Konten einzelner Benutzer erstellt.
manager: wpickett
ms.author: riande
ms.date: 11/30/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/individual
ms.openlocfilehash: 6d05cd8c0ee6c9029eb9bbafc15d9b19ee7633de
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252021"
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a>Artikel basierend auf ASP.NET Core-Projekte mit Konten einzelner Benutzer erstellt

ASP.NET Core Identität ist in-Projektvorlagen in Visual Studio mit der Option "Einzelne Benutzerkonten" enthalten.

Die Authentifizierung Vorlagen stehen in .NET Core CLI mit `-au Individual`:

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new webapp -au Individual
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new razor -au Individual
```

::: moniker-end

In den folgenden Artikeln veranschaulichen, wie den Code in ASP.NET Core Vorlagen, mit denen einzelne Benutzerkonten generiert:

* [Zweistufige Authentifizierung mit SMS](xref:security/authentication/2fa)
* [Account confirmation and password recovery in ASP.NET Core (Kontobestätigung und Kennwortwiederherstellung in ASP.NET Core)](xref:security/authentication/accconfirm)
* [Erstellen einer ASP.NET Core-app mit Benutzerdaten durch Autorisierung geschützt](xref:security/authorization/secure-data)
