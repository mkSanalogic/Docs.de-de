---
title: Sichere Speicherung von geheime app-Schlüssel in der Entwicklung in ASP.NET Core
author: rick-anderson
description: Informationen Sie zum Speichern und Abrufen von vertraulichen Informationen als app geheime Schlüssel während der Entwicklung einer App ASP.NET Core.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 05/23/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/app-secrets
ms.openlocfilehash: fd5cf5cdffd7281d7f4e0d96e8230b60be64a7c3
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/06/2018
ms.locfileid: "34819135"
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a>Sichere Speicherung von geheime app-Schlüssel in der Entwicklung in ASP.NET Core

Durch [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), und [Scott Addie](https://github.com/scottaddie)

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

Dieses Dokument erläutert Techniken zum Speichern und Abrufen von sensiblen Daten während der Entwicklung einer App ASP.NET Core. Sie sollten niemals Kennwörter oder andere vertraulichen Daten im Quellcode speichern, und Sie darf keine Produktion geheime Schlüssel in der Entwicklung oder Modus zu testen. Sie können speichern und Schützen von Azure geheime Schlüssel Test- und produktionsumgebungen mit der [Azure Key Vault-Konfigurationsanbieter](xref:security/key-vault-configuration).

## <a name="environment-variables"></a>Umgebungsvariablen

Umgebungsvariablen werden zum Speichern von app-Kennwörter im Code oder in den lokalen Konfigurationsdateien zu vermeiden. Umgebungsvariablen außer Kraft setzen von Konfigurationswerten für alle zuvor angegebenen Konfigurationsquellen aus.

::: moniker range="<= aspnetcore-1.1"
Konfigurieren Sie das Lesen der Werte für Umgebungsvariablen, durch den Aufruf [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) in die `Startup` Konstruktor:

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]
::: moniker-end

Betrachten Sie eine ASP.NET Core-Web-app in der **einzelne Benutzerkonten** Sicherheit aktiviert ist. Eine Standard-Datenbank-Verbindungszeichenfolge wird in das Projekt aufgenommenes *appsettings.json* Datei mit dem Schlüssel `DefaultConnection`. Die Standard-Verbindungszeichenfolge ist für LocalDB, im Benutzermodus ausgeführt wird und ein Kennwort erfordert. Während der Bereitstellung von Apps die `DefaultConnection` Schlüssel-Wert kann überschrieben werden, mit dem Wert einer Umgebungsvariablen. Die Umgebungsvariable kann die vollständige Verbindungszeichenfolge mit vertraulichen Anmeldeinformationen zu speichern.

> [!WARNING]
> Umgebungsvariablen werden im Allgemeinen in einfachen, unverschlüsselte Text gespeichert. Wenn der Computer oder einen Prozess beschädigt ist, können der Umgebungsvariablen von nicht vertrauenswürdigen Parteien zugegriffen werden. Zusätzliche Maßnahmen, um die Offenlegung von vertrauliche Benutzerdaten zu verhindern, dass möglicherweise erforderlich.

## <a name="secret-manager"></a>Geheime Schlüssel-Manager

Das Schlüssel-Manager-Tool speichert vertrauliche Daten während der Entwicklung eines Projekts ASP.NET Core. In diesem Kontext ist ein Teil der sensiblen Daten ein app-Geheimnis. App-Kennwörter werden in einem anderen Speicherort aus der Projektstruktur gespeichert. Die app-Kennwörter werden mit einem bestimmten Projekt verknüpft ist oder mehreren Projekten gemeinsam. Die app-Kennwörter werden nicht in das Quellsteuerelement eingecheckt.

> [!WARNING]
> Der geheime Schlüssel-Manager-Tool nicht die gespeicherten geheimen Schlüssel zum Verschlüsseln und sollte nicht als vertrauenswürdigen Speicher behandelt werden. Es ist nur für Entwicklungszwecke. Die Schlüssel und Werte werden in einer JSON-Konfigurationsdatei im Verzeichnis Benutzers-Profil gespeichert.

## <a name="how-the-secret-manager-tool-works"></a>Wie funktioniert das Schlüssel-Manager-tool

Das Schlüssel-Manager-Tool abstrahiert die Implementierungsdetails, z. B., wo und wie die Werte gespeichert werden. Sie können das Tool verwenden, ohne diese Implementierungsdetails. Die Werte werden in einer JSON-Konfigurationsdatei in einem System geschütztem Benutzerprofilordner auf dem lokalen Computer gespeichert:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

-Pfad:

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="macostabmacos"></a>[macOS](#tab/macos)

-Pfad:

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

-Pfad:

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

Klicken Sie in den vorherigen Dateipfade, ersetzen Sie `<user_secrets_id>` mit der `UserSecretsId` Wert in der *csproj* Datei.

Schreiben Sie Code, der abhängig von der Speicherort oder das Format der Daten, die mit dem geheimen Schlüssel-Manager-Tool gespeichert nicht. Diese Implementierungsdetails können geändert werden. Z. B. die Werte für den geheimen werden nicht verschlüsselt, aber in der Zukunft werden konnte.

::: moniker range="<= aspnetcore-2.0"
## <a name="install-the-secret-manager-tool"></a>Installieren Sie den geheimen Schlüssel-Manager

Das Schlüssel-Manager-Tool wird mit der .NET Core CLI ab .NET Core SDK 2.1.300 gebündelt. Für .NET Core SDK-Versionen vor 2.1.300 ist ein Tool-Installationsordner erforderlich.

> [!TIP]
> Führen Sie `dotnet --version` aus eine Befehlsshell, die Anzahl der installierten .NET Core SDK-Version finden Sie unter.

Wenn die .NET Core SDK verwendet das Tool enthält, wird eine Warnung angezeigt:

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

Installieren der [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet-Paket im Projekt ASP.NET Core. Zum Beispiel:

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]

Führen Sie den folgenden Befehl in eine Befehlsshell beim Überprüfen der Installation der Tools:

```console
dotnet user-secrets -h
```

Das Schlüssel-Manager-Tool zeigt die Verwendung des Beispiels, der Optionen und der Befehl Hilfe:

```console
Usage: dotnet user-secrets [options] [command]

Options:
  -?|-h|--help                        Show help information
  --version                           Show version information
  -v|--verbose                        Show verbose output
  -p|--project <PROJECT>              Path to project. Defaults to searching the current directory.
  -c|--configuration <CONFIGURATION>  The project configuration to use. Defaults to 'Debug'.
  --id                                The user secret ID to use.

Commands:
  clear   Deletes all the application secrets
  list    Lists all the application secrets
  remove  Removes the specified user secret
  set     Sets the user secret to the specified value

Use "dotnet user-secrets [command] --help" for more information about a command.
```

> [!NOTE]
> Müssen Sie sich im selben Verzeichnis wie die *csproj* Datei im definierten Tools ausführen, die *csproj* Datei `DotNetCliToolReference` Elemente.
::: moniker-end

## <a name="set-a-secret"></a>Legen Sie einen geheimen Schlüssel

Das Schlüssel-Manager-Tool verarbeitet projektspezifische Konfigurationseinstellungen, die in Ihrem Profil gespeichert. Definieren, um Benutzer geheime Schlüssel zu verwenden, eine `UserSecretsId` Element innerhalb einer `PropertyGroup` von der *csproj* Datei. Der Wert des `UserSecretsId` ist willkürlich, jedoch ist charakteristisch für das Projekt. Entwickler in der Regel generiert eine GUID für die `UserSecretsId`.

::: moniker range="<= aspnetcore-1.1"
[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]
::: moniker-end

> [!TIP]
> Klicken Sie in Visual Studio mit der rechten Maustaste des Projekts im Projektmappen-Explorer, und wählen Sie **verwalten Benutzer geheime Schlüssel** aus dem Kontextmenü. Fügt dieser Bewegung eine `UserSecretsId` -Element, mit einer GUID, die aufgefüllt sind, zu der *csproj* Datei. Visual Studio öffnet eine *secrets.json* Datei im Text-Editor. Ersetzen Sie den Inhalt des *secrets.json* mit Schlüssel-Wert-Paare gespeichert werden soll. Beispiel: [!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)]

Definieren Sie ein app-Geheimnis bestehend aus einem Schlüssel und seinen Wert an. Der geheime Schlüssel ist mit des Projekts zugeordneten `UserSecretsId` Wert. Führen Sie beispielsweise den folgenden Befehl aus dem Verzeichnis, in dem die *csproj* Datei vorhanden ist:

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

Im vorherigen Beispiel der Doppelpunkt bedeutet, dass `Movies` ist ein Objekt mit Literalen eine `ServiceApiKey` Eigenschaft.

Das Schlüssel-Manager-Tool kann von anderen Verzeichnissen zu verwendet werden. Verwenden der `--project` Option aus, um den Dateisystempfad zu dem Angeben der *csproj* Datei vorhanden ist. Zum Beispiel:

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

## <a name="set-multiple-secrets"></a>Legen Sie mehrere geheime Schlüssel

Ein Batch von geheimen Schlüsseln kann festgelegt werden, indem piping JSON in der `set` Befehl. Im folgenden Beispiel die *input.json* Dateiinhalt sind über die Pipeline an das `set` Befehl.

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

Öffnen Sie eine Befehlsshell, und führen Sie den folgenden Befehl aus:

  ```console
  type .\input.json | dotnet user-secrets set
  ```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

Öffnen Sie eine Befehlsshell, und führen Sie den folgenden Befehl aus:

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

Öffnen Sie eine Befehlsshell, und führen Sie den folgenden Befehl aus:

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a>Zugriff auf einen geheimen Schlüssel

::: moniker range="<= aspnetcore-1.1"
Die [ASP.NET Core-Konfigurations-API](xref:fundamentals/configuration/index) ermöglicht den Zugriff auf den geheimen Schlüssel-Manager. Installieren der [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet-Paket.

Fügen Sie die Benutzer Konfigurationsquelle für geheime Schlüssel mit einem Aufruf von [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in die `Startup` Konstruktor:

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
Die [ASP.NET Core-Konfigurations-API](xref:fundamentals/configuration/index) ermöglicht den Zugriff auf den geheimen Schlüssel-Manager. Wenn Ihr Projekt auf .NET Framework abzielt, installieren Sie die [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet-Paket.

In ASP.NET Core 2.0 oder höher, die Benutzer-Konfigurationsquelle geheime Schlüssel automatisch für den Entwicklungsmodus hinzugefügt, wenn das Projekt aufgerufen [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) zum Initialisieren einer neuen Instanz des Hosts mit Standardwerten vorkonfiguriert. `CreateDefaultBuilder` Aufrufe [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) bei der [EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname) ist [Entwicklung](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development):

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]

Wenn `CreateDefaultBuilder` wird nicht aufgerufen wird, während der Erstellung des Hosts, die Benutzer Konfigurationsquelle für geheime Schlüssel mit einem Aufruf von hinzufügen [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in die `Startup` Konstruktor:

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]
::: moniker-end

Vertrauliche Benutzerdaten abgerufen werden können, über die `Configuration` API:

::: moniker range="<= aspnetcore-1.1"
[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]
::: moniker-end

## <a name="string-replacement-with-secrets"></a>Zeichenfolgenersetzungen mit geheimen Schlüsseln

Speichern von Kennwörtern als nur-Text ist unsicher. Z. B. eine Datenbankverbindungszeichenfolge, die in gespeicherten *appsettings.json* eventuell ein Kennwort für den angegebenen Benutzer:

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

Ein sicherer Ansatz besteht darin, das Kennwort als ein geheimer Schlüssel zu speichern. Zum Beispiel:

```console
dotnet user-secrets set "DbPassword" "pass123"
```

Entfernen Sie die `Password` Schlüssel-Wert-Paar aus der Verbindungszeichenfolge in *appsettings.json*. Zum Beispiel:

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

Der Schlüssel-Wert kann festgelegt werden, auf eine ["SqlConnectionStringBuilder"](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder) des Objekts [Kennwort](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.password) Eigenschaft die Verbindungszeichenfolge abgeschlossen:

::: moniker range="<= aspnetcore-1.1"
[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]
::: moniker-end

## <a name="list-the-secrets"></a>Liste der geheime Schlüssel

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Führen den folgenden Befehl aus dem Verzeichnis, in dem die *csproj* Datei vorhanden ist:

```console
dotnet user-secrets list
```

Die folgende Ausgabe wird angezeigt:

```console
Movies:ServiceApiKey = 12345
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
```

Im vorherigen Beispiel kennzeichnet ein Doppelpunkt in den Schlüsselnamen in die Objekthierarchie *secrets.json*.

## <a name="remove-a-single-secret"></a>Entfernen Sie einen einzigen geheimen Schlüssel

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Führen den folgenden Befehl aus dem Verzeichnis, in dem die *csproj* Datei vorhanden ist:

```console
dotnet user-secrets remove "Movies:ConnectionString"
```

Der app *secrets.json* Datei wurde geändert, um das Schlüssel-Wert-Paar zugeordnet Entfernen der `MoviesConnectionString` Schlüssel:

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

Ausführen `dotnet user-secrets list` wird die folgende Meldung angezeigt:

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a>Entfernen Sie aller geheimen Schlüssel

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Führen den folgenden Befehl aus dem Verzeichnis, in dem die *csproj* Datei vorhanden ist:

```console
dotnet user-secrets clear
```

Alle Benutzer geheime Schlüssel für die app aus gelöscht wurden die *secrets.json* Datei:

```json
{}
```

Ausführen `dotnet user-secrets list` wird die folgende Meldung angezeigt:

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>
