---
title: Verwenden der Angular-Projektvorlage mit ASP.NET Core
author: SteveSandersonMS
description: Erfahren Sie, wie Sie sich mit der Projektvorlage für die Einzelseitenanwendung (Single-Page Application, SPA) von ASP.NET Core für Angular und die Angular-CLI vertraut machen.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/angular
ms.openlocfilehash: 244fece83279ae4d9ead9b345fcdd66ad6ed4225
ms.sourcegitcommit: 466300d32f8c33e64ee1b419a2cbffe702863cdf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2018
ms.locfileid: "34555429"
---
# <a name="use-the-angular-project-template-with-aspnet-core"></a>Verwenden der Angular-Projektvorlage mit ASP.NET Core

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> Diese Dokumentation befasst sich nicht mit der in ASP.NET Core 2.0 enthaltenen Angular-Projektvorlage. Sie befasst sich mit der neueren Angular-Vorlage, die manuell aktualisiert werden kann. Die Vorlage ist standardmäßig in ASP.NET Core 2.1 enthalten.

::: moniker-end

Die aktualisierte Angular-Projektvorlage stellt einen geeigneten Anfangspunkt für ASP.NET Core-Apps dar, die Angular und die Angular-CLI für die Implementierung einer umfangreichen, clientseitigen Benutzerschnittstelle (User Interface, UI) verwenden.

Mit der Vorlage können Sie ein ASP.NET Core-Projekt, das als API-Back-End fungieren soll, sowie ein Angular-CLI-Projekt erstellen, das als Benutzerschnittstelle fungieren soll. Die Vorlage bietet den Vorteil, dass beide Projekte in einem App-Projekt gehostet werden können. Folglich kann das App-Projekt als eine einzelne Einheit erstellt und veröffentlicht werden.

## <a name="create-a-new-app"></a>Erstellen einer neuen App

Stellen Sie bei Verwendung von ASP.NET Core 2.0 sicher, dass Sie [die aktualisierte Angular-Projektvorlage installiert](xref:spa/index#installation) haben. Wenn Sie über ASP.NET Core 2.1 verfügen, ist keine Installation erforderlich.

Erstellen Sie über eine Eingabeaufforderung mit dem Befehl `dotnet new angular` in einem leeren Verzeichnis ein neues Projekt. Mit den folgenden Befehlen wird die App beispielsweise im Verzeichnis *my-new-app* erstellt und zu diesem Verzeichnis gewechselt:

```console
dotnet new angular -o my-new-app
cd my-new-app
```

Führen Sie die App über Visual Studio oder die .NET Core CLI aus:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

Öffnen Sie die generierte *CSPROJ*-Datei, und führen Sie die App von dort wie gewohnt aus.

Während der Erstellung werden bei der ersten Ausführung npm-Abhängigkeiten wiederhergestellt. Dies kann einige Minuten dauern. Nachfolgende Builds sind wesentlich schneller.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core-CLI](#tab/netcore-cli/)

Stellen Sie sicher, dass Sie über eine Umgebungsvariable mit dem Namen `ASPNETCORE_Environment` und dem Wert `Development` verfügen. Führen Sie unter Windows (bei Eingabeaufforderungen außerhalb von PowerShell) `SET ASPNETCORE_Environment=Development` aus. Führen Sie unter Linux oder macOS `export ASPNETCORE_Environment=Development` aus.

Führen Sie [dotnet build](/dotnet/core/tools/dotnet-build) aus, um zu überprüfen, ob die App ordnungsgemäß erstellt wurde. Während der Erstellung werden bei der ersten Ausführung npm-Abhängigkeiten wiederhergestellt. Dies kann einige Minuten dauern. Nachfolgende Builds sind wesentlich schneller.

Führen Sie [dotnet run](/dotnet/core/tools/dotnet-run) aus, um die App zu starten. Eine der folgenden ähnelnde Meldung wird angezeigt:

```console
Now listening on: http://localhost:<port>
```

Navigieren Sie in einem Browser zu dieser URL.

Die App startet im Hintergrund eine Instanz des Angular-CLI-Servers. Eine der folgenden ähnelnde Meldung wird protokolliert: *NG Live-Entwicklungsserver lauscht an localhost:&lt;otherport&gt;, öffnen Sie http://localhost:&lt;otherport&gt; /* in Ihrem Browser. Ignorieren Sie die Meldung&mdash;Es handelt sich **nicht** um die URL für die kombinierte ASP.NET Core und Angular-LI-App.

---

Mit der Projektvorlage werden eine ASP.NET Core-App und eine Angular-App erstellt. Die ASP.NET Core-App ist zur Verwendung für den Datenzugriff, die Autorisierung und weitere serverseitige Belange vorgesehen. Die Angular-App, die sich im Unterverzeichnis *ClientApp* befindet, ist für sämtliche Belange der Benutzerschnittstelle vorgesehen.

## <a name="add-pages-images-styles-modules-etc"></a>Hinzufügen von Seiten, Images, Formatvorlagen, Modulen usw.

Das Verzeichnis *ClientApp* enthält eine standardmäßige Angular-CLI-App. Weitere Informationen finden Sie in der offiziellen [Angular-Dokumentation](https://github.com/angular/angular-cli/wiki).

Die Angular-App, die mit dieser Vorlage erstellt wurde, und die App, die von der Angular-CLI selbst (über `ng new`) erstellt wurde, unterscheiden sich geringfügig. Die Funktionen der App sind jedoch unverändert. Die mit der Vorlage erstellte App enthält ein auf [Bootstrap](https://getbootstrap.com/) basiertes Layout und ein Beispiel für grundlegendes Routing.

## <a name="run-ng-commands"></a>Ausführen von ng-Befehlen

Wechseln Sie über eine Eingabeaufforderung zum Unterverzeichnis *ClientApp*:

```console
cd ClientApp
```

Wenn das `ng`-Tool global installiert ist, können Sie seine Befehle ausführen. Sie können z.B. `ng lint`, `ng test` oder einen beliebigen anderen [Angular-CLI-Befehl](https://github.com/angular/angular-cli/wiki#additional-commands) ausführen. Allerdings muss `ng serve` nicht ausgeführt werden, da Ihre ASP.NET Core-App den serverseitigen und den clientseitigen Teil Ihrer App bedient. Intern verwendet sie `ng serve` bei der Entwicklung.

Wenn Sie das `ng`-Tool nicht installiert haben, führen Sie stattdessen `npm run ng` aus. Sie können beispielsweise `npm run ng lint` `npm run ng test` ausführen.

## <a name="install-npm-packages"></a>NPM-Pakete installieren

Verwenden Sie für die Installation von npm-Paketen von Drittanbietern eine Eingabeaufforderung im Unterverzeichnis *ClientApp*. Zum Beispiel:

```console
cd ClientApp
npm install --save <package_name>
```

## <a name="publish-and-deploy"></a>Veröffentlichen und Bereitstellen

Bei der Entwicklung wird die App in einem für Entwickler optimierten Modus ausgeführt. JavaScript-Pakete enthalten beispielsweise Quellzuordnungen (damit Sie beim Debuggen Ihren ursprünglichen TypeScript-Code anzeigen können). Die App überwacht TypeScript-, HTML- und CSS-Dateiänderungen auf dem Datenträger und führt bei Feststellung dieser Dateiänderungen automatisch eine Neukompilierung und ein erneutes Laden durch.

Stellen Sie bei der Produktion eine für Leistung optimierte Version Ihrer App bereit. Dies erfolgt gemäß der Konfiguration automatisch. Beim Veröffentlichen gibt die Buildkonfiguration einen verkleinerten, Ahead-of-Time-kompilierten Build (AOT-Build) Ihres clientseitigen Codes aus. Im Gegensatz zum Entwicklungsbuild muss Node.js beim Produktionsbuild nicht auf dem Server installiert sein (sofern Sie das [serverseitige Vorabrendern](#server-side-rendering) nicht aktiviert haben).

Sie können [ASP.NET Core-Standardhosting- und -bereitstellungsmethoden](xref:host-and-deploy/index) verwenden.

## <a name="run-ng-serve-independently"></a>Unabhängiges Ausführen von „ng serve“

Das Projekt ist so konfiguriert, dass die eigene Instanz des Angular-CLI-Servers im Hintergrund gestartet wird, wenn die ASP.NET Core-App im Entwicklungsmodus gestartet wird. Dies ist nützlich, da es bedeutet, dass Sie keinen separaten Server manuell ausführen müssen.

Bei diesem Standardsetup gibt es einen Nachteil. Jedes Mal, wenn Sie Ihren C#-Code ändern und Ihre ASP.NET Core-App neu gestartet werden muss, wird auch der Angular-CLI-Server neu gestartet. Die Sicherung wird nach ca. 10 Sekunden gestartet. Wenn Sie Ihren C#-Code häufig bearbeiten und nicht warten möchten, bis der Angular-CLI-Server neu gestartet wurde, können Sie den Angular-CLI-Server unabhängig vom ASP.NET Core-Prozess extern ausführen. Gehen Sie hierzu wie folgt vor:

1. Wechseln Sie in einer Eingabeaufforderung zu dem Unterverzeichnis *ClientApp*, und starten Sie den Angular-CLI-Entwicklungsserver:

    ```console
    cd ClientApp
    npm start
    ```

    > [!IMPORTANT]
    > Starten Sie den Angular-CLI-Entwicklungsserver mit `npm start` und nicht mit `ng serve`, damit die Konfiguration in *package.json* gewahrt wird. Um zusätzliche Parameter an den Angular-CLI-Server zu übergeben, fügen Sie diese der entsprechenden `scripts`-Zeile in der Datei *package.json* hinzu.

2. Ändern Sie Ihre ASP.NET Core-App so, dass die externe Angular-CLI-Instanz verwendet wird, anstatt eine eigene Instanz zu starten. Ersetzen Sie den `spa.UseAngularCliServer`-Aufruf in Ihrer *Startklasse* durch Folgendes:

    ```csharp
    spa.UseProxyToSpaDevelopmentServer("http://localhost:4200");
    ```

Wenn Sie Ihre ASP.NET Core-App starten, wird kein Angular-CLI-Server gestartet. Stattdessen wird die Instanz verwendet, die Sie manuell gestartet haben. Dadurch kann sie schneller gestartet und neu gestartet werden. Sie müssen also nicht mehr jedes Mal warten, bis Angular-CLI Ihre Client-App neu erstellt hat.

## <a name="server-side-rendering"></a>Serverseitiges Rendering

Sie können ein Leistungsfeature verwenden, d.h. Sie können Ihre Angular-App vorab auf dem Server rendern und sie auf dem Client ausführen. Dies bedeutet, dass Browser HTML-Markup erhalten, das die ursprüngliche UI Ihrer App darstellt, und diese anzeigen, bevor Sie JavaScript-Pakete herunterladen und ausführen. Ein Großteil der Implementierung dieses Merkmals stammt vom Angular-Feature [Angular Universal](https://universal.angular.io/).

> [!TIP]
> Durch Aktivieren von serverseitigem Rendering (SSR) treten einige zusätzliche Komplikationen während der Entwicklung und der Bereitstellung auf. Weitere Informationen dazu, ob das SSR für Ihre Anforderungen geeignet ist, finden Sie unter [Nachteile von SSR](#drawbacks-of-ssr).

Zum Aktivieren von SSR müssen Sie einige Ergänzungen für das Projekt vornehmen.

Fügen Sie in der *Startup*-Klasse *nach* der Zeile, die `spa.Options.SourcePath` konfiguriert, und *vor* dem Aufruf von `UseAngularCliServer` oder `UseProxyToSpaDevelopmentServer` Folgendes hinzu:

[!code-csharp[](sample/AngularServerSideRendering/Startup.cs?name=snippet_Call_UseSpa&highlight=5-12)]

Im Entwicklungsmodus versucht dieser Code das SSR-Paket durch Ausführen des Skripts `build:ssr` zu erstellen, das in *ClientApp\package.json* definiert ist. Dies erstellt eine Angular-App namens `ssr`, die noch nicht definiert ist.

Am Ende des `apps`-Arrays *ClientApp/.angular-cli.json* definieren Sie eine zusätzliche App mit dem Namen `ssr`. Verwenden Sie folgende Optionen:

[!code-json[](sample/AngularServerSideRendering/ClientApp/.angular-cli.json?range=24-41)]

Die neue SSR-fähige App-Konfiguration erfordert zwei weitere Dateien: *tsconfig.server.json* und *main.server.ts*. Die Datei *tsconfig.server.json* gibt die Optionen für die TypeScript-Kompilierung an. Die Datei *main.server.ts* dient als Codeeinstiegspunkt während des SSR.

Fügen Sie eine neue Datei namens *tsconfig.server.json* in *ClientApp/Src* (neben der vorhandenen Datei *tsconfig.app.json*) hinzu, die Folgendes enthält:

[!code-json[](sample/AngularServerSideRendering/ClientApp/src/tsconfig.server.json)]

Diese Datei konfiguriert den AOT-Compiler von Angular so, dass er das Modul `app.server.module` sucht. Fügen Sie dies hinzu, indem Sie eine neue Datei in *ClientApp/src/app/app.server.module.ts* (neben der vorhandenen Datei *app.module.ts*) erstellen, die Folgendes enthält:

[!code-typescript[](sample/AngularServerSideRendering/ClientApp/src/app/app.server.module.ts)]

Dieses Modul erbt vom clientseitigen `app.module` und definiert, welche zusätzlichen Angular-Module während des SSR verfügbar sind.

Bedenken Sie, dass der neue Eintrag `ssr` in *.angular cli.json* auf die Einstiegspunktdatei *main.server.ts* verwiesen hat. Sie haben die Datei noch nicht hinzugefügt und sollten dies jetzt nachholen. Erstellen Sie eine neue Datei in *ClientApp/src/main.server.ts* (neben der vorhandenen Datei *main.ts*), die Folgendes enthält:

[!code-typescript[](sample/AngularServerSideRendering/ClientApp/src/main.server.ts)]

Der Code dieser Datei wird von ASP.NET Core für jede Anforderung bei der Ausführung der `UseSpaPrerendering`-Middleware ausgeführt, die Sie der *Startup*-Klasse hinzugefügt haben. Dabei werden `params` aus dem .NET-Code empfangen (z.B. die URL, die angefordert wird) und Aufrufe von Angular-SSR-APIs durchgeführt, um das resultierende HTML abzurufen.

Genau genommen reicht dies aus, um das SSR im Entwicklungsmodus zu aktivieren. Es ist von grundlegender Bedeutung, dass eine letzte Änderung vorgenommen wird, sodass Ihre App bei der Veröffentlichung ordnungsgemäß funktioniert. Setzen Sie in der *csproj*-Hauptdatei der App den Eigenschaftswert `BuildServerSideRenderer` auf `true`:

[!code-xml[](sample/AngularServerSideRendering/AngularServerSideRendering.csproj?name=snippet_EnableBuildServerSideRenderer)]

Dadurch wird der Buildprozess so konfiguriert, dass `build:ssr` während der Veröffentlichung ausgeführt wird und die SSR-Dateien auf dem Server bereitgestellt werden. Wenn Sie die Funktion nicht aktivieren, schlägt das SSR in der Produktion fehl.

Wenn Ihre App im Entwicklungs- oder Produktionsmodus ausgeführt wird, wird der Angular-Code vorab als HTML auf dem Server gerendert. Der clientseitige Code wird normal ausgeführt.

### <a name="pass-data-from-net-code-into-typescript-code"></a>Übergeben von Daten aus .NET Code in TypeScript-Code

Während des SSR empfiehlt es sich, per Anforderung Daten aus der ASP.NET Core-App an die Angular-App zu übergeben. Beispielsweise könnten Sie Cookieinformationen übergeben oder etwas aus einer Datenbank lesen. Bearbeiten Sie hierzu die *Startup*-Klasse. Im Rückruf für `UseSpaPrerendering` legen Sie einen Wert für `options.SupplyData` fest, z.B.:

```csharp
options.SupplyData = (context, data) =>
{
    // Creates a new value called isHttpsRequest that's passed to TypeScript code
    data["isHttpsRequest"] = context.Request.IsHttps;
};
```

Der `SupplyData`-Rückruf ermöglicht Ihnen, beliebige JSON-serialisierbare Daten per Anforderung zu übergeben (z.B. Zeichenfolgen, boolesche Werte oder Zahlen). Der *main.server.ts*-Code erhält diese als `params.data`. Im vorangehenden Codebeispiel wird z.B. ein boolescher Wert als `params.data.isHttpsRequest` an den `createServerRenderer`-Rückruf übergeben. Sie können dies auf eine beliebige Art und Weise, die von Angular unterstützt wird, an andere Teile Ihrer App übergeben. Sehen Sie beispielsweise, wie *main.server.ts* den `BASE_URL`-Wert an eine beliebige Komponente übergibt, deren Konstruktor für den Empfang deklariert wurde.

### <a name="drawbacks-of-ssr"></a>Nachteile von SSR

Nicht alle Apps profitieren von SSR. Der Hauptvorteil ist die wahrgenommene Leistung. Besucher, die Ihre App über eine langsame Netzwerkverbindung oder auf langsamen mobilen Geräten erreichen, finden die ursprüngliche UI schnell, auch wenn das Abrufen und Analysieren der JavaScript-Pakete einige Zeit dauert. Allerdings werden viele SPAs hauptsächlich über schnelle, interne Unternehmensnetzwerke auf schnellen Computern verwendet, auf denen die App nahezu umgehend angezeigt wird.

Gleichzeitig hat die Aktivierung von SSR zahlreiche Nachteile. Sie erhöht die Komplexität des Entwicklungsprozesses. Der Code muss in zwei verschiedenen Umgebungen ausgeführt werden: clientseitig und serverseitig (über ASP.NET Core in einer Node.js-Umgebung aufgerufen). Nachfolgend sind einige Dinge aufgeführt, die Sie berücksichtigen sollten:

* Das SSR erfordert eine Node.js-Installation auf Produktionsservern. Dies ist für einige Bereitstellungsszenarios, wie z.B. Azure App Services, automatisch der Fall, für andere, wie etwa Azure Service Fabric, dagegen nicht.
* Bei Aktivierung des `BuildServerSideRenderer`-Buildflags veröffentlicht das *node_modules*-Verzeichnis. Dieser Ordner enthält mehr als 20.000 Dateien, was die Bereitstellungszeit erhöht.
* Zum Ausführen von Code in einer Node.js-Umgebung kann nicht davon ausgegangen werden, dass Browser-spezifische JavaScript-APIs, wie z.B. `window` oder `localStorage`, vorhanden sind. Wenn Ihr Code (oder eine Bibliothek eines Drittanbieters, auf die Sie verweisen) versucht, diese APIs zu verwenden, erhalten Sie einen Fehler während des SSR. Verwenden Sie beispielsweise jQuery nicht, da es häufig auf Browser-spezifische APIs verweist. Um Fehler auszuschließen, müssen Sie das SSR vermeiden oder aber Browser-spezifische APIs oder Bibliotheken vermeiden. Sie können alle Aufrufe dieser APIs in Überprüfungen einschließen, um sicherzustellen, dass sie nicht während des SSR aufgerufen werden. Verwenden Sie z.B. eine Überprüfung wie die im folgenden JavaScript- oder TypeScript-Code enthaltene:

    ```javascript
    if (typeof window !== 'undefined') {
        // Call browser-specific APIs here
    }
    ```
