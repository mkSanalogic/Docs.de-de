---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/accessing-your-models-data-from-a-controller
title: Zugriff auf das Modell Daten aus einem Controller (c#) | Microsoft Docs
author: Rick-Anderson
description: In diesem Lernprogramm erfahren Sie die Grundlagen der Erstellung einer ASP.NET MVC-Webanwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1, also...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 002ada5c-f114-47ab-a441-57dbdb728ea0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 4218116eec6f177730087955b8fb69e5ecc0022f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872802"
---
<a name="accessing-your-models-data-from-a-controller-c"></a>Zugriff auf das Modell Daten aus einem Controller (c#)
====================
durch [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Eine aktualisierte Version dieses Lernprogramms steht [hier](../../../getting-started/introduction/getting-started.md) , ASP.NET MVC 5 und Visual Studio 2013 verwendet. Es ist sicherer, viel einfacher, führen und weitere Funktionen veranschaulicht.
> 
> 
> In diesem Lernprogramm erfahren Sie die Grundlagen der Erstellung einer ASP.NET MVC-Webanwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1, also eine kostenlose Version von Microsoft Visual Studio. Bevor Sie beginnen, stellen Sie sicher, dass Sie die unten aufgeführten erforderlichen Komponenten installiert haben. Sie können alle installieren, indem Sie auf den folgenden Link: [Webplattform-Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativ können Sie die erforderlichen Komponenten, die über die folgenden Links einzeln installieren:
> 
> - [Visual Studio Web Developer Express SP1-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Common Language Runtime + Tools unterstützen)
> 
> Wenn Sie Visual Studio 2010 anstelle von Visual Web Developer 2010 verwenden, die Komponenten installieren, indem Sie auf den folgenden Link: [Visual Studio 2010-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Ein Visual Web Developer-Projekt mit C#-Quellcode ist zu diesem Thema steht zur Verfügung. [Die C#-Version herunterladen](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Wenn Sie Visual Basic bevorzugen, wechseln Sie zu der [Visual Basic-Version](../vb/intro-to-aspnet-mvc-3.md) dieses Lernprogramm.

In diesem Abschnitt erstellen Sie ein neues `MoviesController` Klasse, und Schreiben von Code, ruft die Filmdaten ab und zeigt ihn im Browser mit einer Vorlage anzeigen. Achten Sie darauf, dass zum Erstellen der Anwendung, bevor Sie fortfahren.

Mit der rechten Maustaste die *Controller* Ordner und erstellen Sie ein neues `MoviesController` Controller. Wählen Sie die folgenden Optionen:

- Controllername: **MoviesController**. (Dies ist die Standardeinstellung. )
- Vorlage: **Controller mit Lese-/schreibaktionen und Ansichten, die Verwendung von Entity Framework**.
- Modellschemas: **Film (MvcMovie.Models)**.
- Datenkontextklasse: **MovieDBContext (MvcMovie.Models)**.
- Ansichten: **Razor (CSHTML)**. (Die Standardeinstellung.)

[![AddScaffoldedMovieController](accessing-your-models-data-from-a-controller/_static/image2.png "AddScaffoldedMovieController")](accessing-your-models-data-from-a-controller/_static/image1.png)

Klicken Sie auf **Hinzufügen**. Visual Web Developer erstellt die folgenden Dateien und Ordner:

- *Eine MoviesController.cs* -Datei in des Projekts *Controller* Ordner.
- Ein *Filme* Ordner des Projekts *Ansichten* Ordner.
- *Create.cshtml Delete.cshtml, Details.cshtml, Edit.cshtml*, und *Index.cshtml* in der neuen *Views\Movies* Ordner.

[![NewMovieControllerScreenShot](accessing-your-models-data-from-a-controller/_static/image4.png "NewMovieControllerScreenShot")](accessing-your-models-data-from-a-controller/_static/image3.png)

Der ASP.NET MVC 3-Gerüstbau automatisch erstellt, die CRUD-Vorgänge (erstellen, lesen, aktualisieren und löschen) Aktionsmethoden und Ansichten für Sie. Sie verfügen jetzt über eine voll funktionsfähige Webanwendung, mit dem Sie erstellen, auflisten, bearbeiten und löschen Film-Einträge.

Führen Sie die Anwendung, und navigieren Sie zu der `Movies` Controller durch Anfügen */Movies* an die URL in der Adressleiste des Browsers. Da die Anwendung auf das Standardrouting der vertrauenden Seite ist (definiert der *"Global.asax"* Datei), die Browseranforderung `http://localhost:xxxxx/Movies` an Standardeinstellung weitergeleitet `Index` Aktionsmethode von der `Movies` Controller. Das heißt, die Browseranforderung `http://localhost:xxxxx/Movies` entspricht effektiv der Browseranforderung `http://localhost:xxxxx/Movies/Index`. Das Ergebnis ist eine leere Liste von Filmen, da Sie noch keine hinzugefügt haben.

![](accessing-your-models-data-from-a-controller/_static/image5.png)

### <a name="creating-a-movie"></a>Erstellen einen Film

Klicken Sie auf den Link **Neu erstellen**. Geben Sie einige Details über einen Film, und klicken Sie dann auf die **erstellen** Schaltfläche.

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Klicken auf die **erstellen** Schaltfläche bewirkt, dass das Formular an den Server zurückgesendet werden, in dem die Informationen in der Datenbank gespeichert. Sie sind dann umgeleitet, um die */Movies* URL, wo Sie in die Auflistung den neu erstellten Film finden können.

[![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image8.png "IndexWhenHarryMet")](accessing-your-models-data-from-a-controller/_static/image7.png)

Erstellen Sie ein paar weitere Filmeinträge. Testen Sie die Links **Edit** (Bearbeiten), **Details** und **Delete** (Löschen), die alle funktionsbereit sind.

## <a name="examining-the-generated-code"></a>Überprüfen des generierten Codes

Öffnen der *Controllers\MoviesController.cs* Datei, und überprüfen Sie die generierte `Index` Methode. Ein Teil der Film-Controller mit dem `Index` Methode wird unten gezeigt.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

Die folgende Zeile aus der `MoviesController` Klasse instanziiert einen Film-Datenbankkontext aus, wie zuvor beschrieben. Den Datenbankkontext Film können Sie Abfragen, bearbeiten und Löschen von Filmen.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

Eine Anforderung an die `Movies` Controller gibt alle Einträge in der `Movies` Tabelle der Datenbank Film und übergibt dann die Ergebnisse in der `Index` anzeigen.

## <a name="strongly-typed-models-and-the-model-keyword"></a>Stark typisierte Modelle und die @model Schlüsselwort

Weiter oben in diesem Lernprogramm Sie gesehen haben wie ein Controller Daten oder Objekte in eine Ansicht Vorlage übergeben kann die `ViewBag` Objekt. Die `ViewBag` ist ein dynamisches Objekt, das eine spät gebundene auf bequeme Weise Informationen an eine Ansicht übergeben werden.

ASP.NET MVC bietet außerdem die Möglichkeit, stark übergeben Daten oder Objekte zu einer Sicht Vorlage eingegeben. Stark typisierte dieser Ansatz ermöglicht eine bessere Kompilierung von Code und umfangreichere IntelliSense im Visual Web Developer-Editor. Wir verwenden diese Ansatz mit der `MoviesController` Klasse und *Index.cshtml* Vorlage anzeigen.

Beachten Sie, wie der Code erstellt ein [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) Objekt beim Aufrufen der `View` Hilfsmethode in der `Index` Aktionsmethode. Der Code übergibt dann diese `Movies` Liste auf dem Controller aus, um die Ansicht:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs)]

Durch Einschließen einer `@model` -Anweisung am Anfang der Vorlagendatei anzeigen, können Sie den Typ des Objekts, das die Sicht erwartet angeben. Beim Erstellen des Controllers Film automatisch enthalten die folgenden Visual Web Developer `@model` Anweisung am Anfang der *Index.cshtml* Datei:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.cshtml)]

Dies `@model` Richtlinie können Sie die Liste von Filmen zuzugreifen, die mithilfe der Controller auf die Ansicht durch Übergeben einer `Model` -Objekt, das stark typisiert ist. Beispielsweise ist in der *Index.cshtml* Vorlage, der Code durchläuft die Filme durch praktische eine `foreach` Anweisung über die stark typisierte `Model` Objekt:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.cshtml)]

Da die `Model` Objekt stark typisiert ist (als ein `IEnumerable<Movie>` Objekt), die jeweils `item` als Objekt in der Schleife typisiert ist `Movie`. U. a. bedeutet dies, dass Sie erhalten die kompilierzeitüberprüfung des Codes und vollständige IntelliSense-Unterstützung im Code-Editor:

[![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image10.png "ModelIntellisene")](accessing-your-models-data-from-a-controller/_static/image9.png)

## <a name="working-with-sql-server-compact"></a>Arbeiten mit SQL Server Compact

Entity Framework Code First wurde festgestellt, dass die Datenbank-Verbindungszeichenfolge, die bereitgestellt wurden, zeigt eine `Movies` Datenbank, die noch nicht vorhanden ist, sodass Code First die Datenbank automatisch erstellt. Sie können überprüfen, dass es durch die Überprüfung erstellt wird die *App\_Daten* Ordner. Wenn Sie sehen die *Movies.sdf* Datei, klicken Sie auf die **alle Dateien anzeigen** Schaltfläche der **Projektmappen-Explorer** -Symbolleiste klicken Sie auf die **aktualisieren** Schaltfläche, und erweitern Sie dann die *App\_Daten* Ordner.

[![SDF_in_SolnExp](accessing-your-models-data-from-a-controller/_static/image12.png "SDF_in_SolnExp")](accessing-your-models-data-from-a-controller/_static/image11.png)

Doppelklicken Sie auf *Movies.sdf* öffnen **Server-Explorer**. Erweitern Sie dann die **Tabellen** Ordner, um die Tabellen anzuzeigen, die in der Datenbank erstellt wurden.

> [!NOTE]
> Wenn Sie eine Fehlermeldung erhalten, bei einem Doppelklick *Movies.sdf*, stellen Sie sicher, dass Sie installiert haben [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Common Language Runtime + Tools unterstützen). (Links für die Software, finden Sie in der Liste der erforderlichen Komponenten in Teil 1 dieses Lernprogramms Serie). Wenn Sie die Version jetzt installieren, müssen Sie zu schließen und erneut öffnen Visual Web Developer.


[![DB_explorer](accessing-your-models-data-from-a-controller/_static/image14.png "DB_explorer")](accessing-your-models-data-from-a-controller/_static/image13.png)

Es gibt zwei Tabellen, eine für die `Movie` Entitätenmenge und dann die `EdmMetadata` Tabelle. Die `EdmMetadata` Tabelle wird vom Entity Framework verwendet, um zu bestimmen, wenn das Modell und die Datenbank nicht mehr synchron sind.

Mit der rechten Maustaste die `Movies` Tabelle, und wählen Sie **Tabellendaten anzeigen** die Daten sehen Sie erstellt haben.

[![MoviesTable](accessing-your-models-data-from-a-controller/_static/image16.png "MoviesTable")](accessing-your-models-data-from-a-controller/_static/image15.png)

Mit der rechten Maustaste die `Movies` Tabelle, und wählen Sie **Tabellenschema bearbeiten**.

[![EditTableSchema](accessing-your-models-data-from-a-controller/_static/image18.png "EditTableSchema")](accessing-your-models-data-from-a-controller/_static/image17.png)

[![TableSchemaSM](accessing-your-models-data-from-a-controller/_static/image20.png "TableSchemaSM")](accessing-your-models-data-from-a-controller/_static/image19.png)

Beachten Sie, dass wie das Schema der `Movies` Tabelle zugeordnet, die `Movie` Klasse, die Sie zuvor erstellt haben. Entity Framework Code First automatisch erstellt, dieses Schema für Sie auf der Grundlage Ihrer `Movie` Klasse.

Wenn Sie fertig sind, schließen Sie die Verbindung. (Wenn Sie die Verbindung nicht schließen, erhalten Sie möglicherweise einen Fehler das nächste Mal des Projekts ausführen).

[![CloseConnection](accessing-your-models-data-from-a-controller/_static/image22.png "CloseConnection")](accessing-your-models-data-from-a-controller/_static/image21.png)

Sie haben jetzt die Datenbank und eine einfache Liste Seite daraus angezeigt werden sollen. In den nächsten Lernprogrammen wir untersuchen Sie den Rest des Codes scaffolded und Hinzufügen einer `SearchIndex` Methode und eine `SearchIndex` anzeigen, die für Filme in dieser Datenbank durchsuchen kann.

> [!div class="step-by-step"]
> [Zurück](adding-a-model.md)
> [Weiter](examining-the-edit-methods-and-edit-view.md)
