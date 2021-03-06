---
uid: webhooks/diagnostics/debugging
title: Debuggen von ASP.NET WebHooks | Microsoft Docs
author: rick-anderson
description: Das Debuggen von ASP.NET WebHooks.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 467da78b-3c35-4c51-8b08-77a32379e4a8
ms.technology: ''
ms.prod: .net-framework
ms.openlocfilehash: 524cdf0246eda9ef213414923cd23a92a01f211e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
ms.locfileid: "28044863"
---
# <a name="aspnet-webhooks-debugging"></a>Debuggen von ASP.NET WebHooks  

## <a name="debugging-in-azure"></a>Debuggen in Azure

Um die Web-Anwendung zu debuggen, während der Ausführung in Azure finden Sie im Lernprogramm [Problembehandlung bei einer Web-app in Azure App Service mithilfe von Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).

## <a name="debugging-with-source-and-symbols"></a>Debuggen mit Quellen und Symbole

Zusätzlich zu Ihren eigenen Code-Debuggen, ist es möglich, direkt in Microsoft ASP.NET WebHooks und in der Tat Debuggen aller .NET. Dies funktioniert unabhängig davon, ob Sie lokal oder Remote Debuggen. Konfigurieren Sie zuerst Visual Studio, um die Quellen und Symbole zu suchen, navigieren Sie zu **Debuggen** und dann **Optionen und Einstellungen**. Legen Sie die Optionen wie folgt:

![Optionen und Einstellungen](_static/SourceSymbols.png)

Fügen Sie dann auf einen Link zur [symbolsource.org](http://symbolsource.org) zum Herunterladen der Quellen und Symbole. Wechseln Sie zu der **Symbole** Menü oben auf der Registerkarte, und fügen Sie eine Symbolspeicherort folgende:

```
http://srv.symbolsource.org/pdb/Public
```

Stellen Sie außerdem sicher, dass das Cacheverzeichnis für einen kurzen Namen verfügt; Andernfalls können die Dateinamen zu lang werden die bewirkt, der die Symbole nicht geladen werden. Ein Beispielpfad ist:

```
C:\SymCache
```

Die Einstellungen sollte etwa wie folgt aussehen:

![Beispiel für Optionen Symbol Speicherort](_static/SymSource.png)
