---
title: Protokollieren in .NET Core und ASP.NET Core
author: rick-anderson
description: In diesem Artikel wird das Verwenden des Protokollierungsframeworks erläutert, das vom Microsoft.Extensions.Logging-NuGet-Paket bereitgestellt wird.
ms.author: riande
ms.custom: mvc
ms.date: 6/29/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: fundamentals/logging/index
ms.openlocfilehash: a4962c3132c60b68cb2d4b5a97e9b082aba15d15
ms.sourcegitcommit: 50e7c970f327dbe92d45eaf4c21caa001c9106d0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2020
ms.locfileid: "87330741"
---
# <a name="logging-in-net-core-and-aspnet-core"></a>Protokollieren in .NET Core und ASP.NET Core

::: moniker range=">= aspnetcore-3.0"

Von [Kirk Larkin](https://twitter.com/serpent5), [Jürgen Gutsch](https://github.com/JuergenGutsch) und [Rick Anderson](https://twitter.com/RickAndMSFT)

.NET Core unterstützt eine Protokollierungs-API, die mit einer Vielzahl von integrierten Protokollierungsanbietern und Drittanbieter-Protokollierungslösungen zusammenarbeitet. Dieser Artikel zeigt, wie Sie die Protokollierungs-API mit integrierten Anbietern verwenden können.

Die meisten der Codebeispiele in diesem Artikel stammen aus ASP.NET Core-Apps. Die auf die Protokollierung bezogenen Teile dieser Codeausschnitte gelten für jede .NET Core-App, die den [generischen Host](xref:fundamentals/host/generic-host) verwendet. Die ASP.NET Core Web-App-Vorlagen verwenden den generischen Host.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/index/samples/3.x) ([Vorgehensweise zum Herunterladen](xref:index#how-to-download-a-sample))

<a name="lp"></a>

## <a name="logging-providers"></a>Protokollierungsanbieter

Protokollierungsanbieter speichern Protokolle, mit Ausnahme des `Console`-Anbieters, der Protokolle anzeigt. Beispielsweise speichert der Azure Application Insights-Anbieter Protokolle in Azure Application Insights. Es können mehrere Anbieter aktiviert werden.

Für die ASP.NET Core-Web-App-Standardvorlagen gilt Folgendes:

* Sie verwenden den [generischen Host](xref:fundamentals/host/generic-host).
* Sie rufen <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder%2A> auf, wodurch die folgenden Protokollierungsanbieter hinzugefügt werden:
  * [Konsole](#console)
  * [Debuggen](#debug)
  * [EventSource](#event-source)
  * [EventLog](#welog): Nur Windows

[!code-csharp[](index/samples/3.x/TodoApiDTO/Program.cs?name=snippet_TemplateCode&highlight=9)]

Der vorstehende Code zeigt die `Program`-Klasse, die mit den ASP.NET Core-Web-App-Vorlagen erstellt wurde. In den nächsten Abschnitten finden Sie Beispiele, die auf den ASP.NET Core-Web-App-Vorlagen basieren, die den generischen Host verwenden. [Nicht-Hostkonsolen-Apps](#nhca) werden weiter unten in diesem Dokument erläutert.

Um den von `Host.CreateDefaultBuilder` hinzugefügten Standardsatz von Protokollierungsanbietern zu überschreiben, rufen Sie `ClearProviders` auf und fügen die erforderlichen Protokollierungsanbieter hinzu. Beispielsweise folgender Code:

* Ruft <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A> auf, um alle <xref:Microsoft.Extensions.Logging.ILoggerProvider>-Instanzen aus dem Generator zu entfernen.
* Fügt den [Konsolen](#console)protokollierungsanbieter hinzu.

[!code-csharp[](index/samples/3.x/TodoApiDTO/Program.cs?name=snippet_AddProvider&highlight=5-6)]

Weitere Anbieter finden Sie unter:

* [Integrierte Protokollierungsanbieter](#bilp)
* [Protokollierungsanbieter von Drittanbietern](#third-party-logging-providers).

## <a name="create-logs"></a>Erstellen von Protokollen

Um Protokolle zu erstellen, verwenden Sie ein <xref:Microsoft.Extensions.Logging.ILogger%601>-Objekt aus der [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection) (DI).

Im Beispiel unten geschieht Folgendes:

* Es wird eine Protokollierung (`ILogger<AboutModel>`) erstellt, die eine Protokoll*kategorie* des vollqualifizierten Namens vom Typ `AboutModel` verwendet. Die Protokollkategorie ist eine Zeichenfolge, die jedem Protokoll zugeordnet ist.
* Es wird <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*> aufgerufen, um Protokollierung mit dem Protokolliergrad `Information` auszuführen. Der Protokollierungs*grad* gibt den Schweregrad des protokollierten Ereignisses an.

[!code-csharp[](index/samples/3.x/TodoApiDTO/Pages/About.cshtml.cs?name=snippet_CallLogMethods&highlight=5,14)]

[Grade](#log-level) und [Kategorien](#log-category) werden weiter unten in diesem Dokument ausführlicher erläutert.

Weitere Informationen zu Blazor finden Sie unter [Erstellen von Protokollen in Blazor und Blazor WebAssembly](#clib) in diesem Dokument.

[Erstellen von Protokollen in „Main“ und „Startup“](#clms) zeigt, wie Protokolle in `Main` und `Startup` erstellt werden.

## <a name="configure-logging"></a>Konfigurieren der Protokollierung

Die Konfiguration der Protokollierung wird meistens im Abschnitt `Logging` von *appsettings*.`{Environment}` *.json*-Dateien angegeben. Die folgende Datei *appsettings.Development.json* wird von den ASP.NET Core-Web-App-Vorlagen generiert:

[!code-json[](index/samples/3.x/TodoApiDTO/appsettings.Development.json)]

Für den oben stehenden JSON-Code gilt:

* Die Kategorien `"Default"`, `"Microsoft"` und `"Microsoft.Hosting.Lifetime"` werden angegeben.
* Die Kategorie `"Microsoft"` gilt für alle Kategorien, die mit `"Microsoft"` beginnen. Diese Einstellung gilt z. B. für die Kategorie `"Microsoft.AspNetCore.Routing.EndpointMiddleware"`.
* Die Kategorie `"Microsoft"` protokolliert mit Protokolliergrad `Warning` oder höher.
* Die Kategorie `"Microsoft.Hosting.Lifetime"` ist genauer als die Kategorie `"Microsoft"`, sodass die Kategorie `"Microsoft.Hosting.Lifetime"` mit dem Protokolliergrad „Information“ oder höher protokolliert.
* Ein bestimmter Protokollanbieter wird nicht angegeben, sodass `LogLevel` für alle aktivierten Protokollanbieter mit Ausnahme von [Windows EventLog](#welog) gilt.

Die `Logging`-Eigenschaft kann <xref:Microsoft.Extensions.Logging.LogLevel> und Protokollanbietereigenschaften beinhalten. `LogLevel` gibt den Mindest[grad](#log-level) an, der für ausgewählte Kategorien protokolliert werden soll. Im JSON-Code oben werden die Protokolliergrade `Information` und `Warning` angegeben. `LogLevel` gibt den Schweregrad des Protokolls an und liegt zwischen 0 und 6:

`Trace` = 0, `Debug` = 1, `Information` = 2, `Warning` = 3, `Error` = 4, `Critical` = 5 und `None` = 6.

Wenn `LogLevel` angegeben wird, wird Protokollierung für Meldungen mit dem angegebenen Protokolliergrad oder höher aktiviert. Im JSON-Code oben wird die Kategorie `Default` für `Information` oder höher protokolliert. Beispielsweise werden `Information`-, `Warning`-, `Error`- und `Critical`-Meldungen protokolliert. Wenn kein `LogLevel` angegeben wird, wird Protokollierung standardmäßig mit dem Protokolliergrad `Information` verwendet. Weitere Informationen finden Sie unter [Protokolliergrade](#llvl).

Eine Anbietereigenschaft kann eine `LogLevel`-Eigenschaft angeben. `LogLevel` unter einem Anbieter gibt die Protokolliergrade an, die für diesen Anbieter protokolliert werden sollen, und überschreibt die Nicht-Anbieterprotokolleinstellungen. Sehen Sie sich die nachfolgende Datei *appsettings.json* an:

[!code-json[](index/samples/3.x/TodoApiDTO/appsettings.Prod2.json)]

Die Einstellungen in `Logging.{providername}.LogLevel` überschreiben die Einstellungen in `Logging.LogLevel`. Im JSON-Code oben wird der Standardprotokolliergrad `Debug` des Anbieters auf `Information` festgelegt:

`Logging:Debug:LogLevel:Default:Information`

Die Einstellung oben gibt den Protokolliergrad `Information` für jede `Logging:Debug:`-Kategorie mit Ausnahme von `Microsoft.Hosting` an. Wenn eine bestimmte Kategorie aufgelistet wird, überschreibt die jeweilige Kategorie die Standardkategorie. Im JSON-Code oben überschreiben die `Logging:Debug:LogLevel`-Kategorien `"Microsoft.Hosting"` und `"Default"` die Einstellungen in `Logging:LogLevel`.

Der Mindestprotokolliergrad kann für Folgendes angegeben werden:

* Bestimmte Anbieter:  Beispiel: `Logging:EventSource:LogLevel:Default:Information`
* Bestimmte Kategorien: Beispiel: `Logging:LogLevel:Microsoft:Warning`
* Alle Anbieter und alle Kategorien: `Logging:LogLevel:Default:Warning`

Alle Protokolle unterhalb des Mindestprotokolliergrads werden ***nicht***:

* An den Anbieter übergeben.
* Protokolliert oder angezeigt.

Um alle Protokolle zu unterdrücken, geben Sie [LogLevel.None](xref:Microsoft.Extensions.Logging.LogLevel) an. `LogLevel.None` hat den Wert 6, der höher als `LogLevel.Critical` (5) ist.

Wenn ein Anbieter [Protokollbereiche](#logscopes) unterstützt, gibt `IncludeScopes` an, ob sie aktiviert sind. Weitere Informationen finden Sie unter [Protokollierbereiche](#logscopes).

Die folgende Datei *appsettings.json* enthält alle Anbieter, die standardmäßig aktiviert sind:

[!code-json[](index/samples/3.x/TodoApiDTO/appsettings.Production.json)]

Im vorgehenden Beispiel:

* Die Kategorien und Protokolliergrade sind keine vorgeschlagenen Werte. Das Beispiel wird bereitgestellt, um alle Standardanbieter zu zeigen.
* Die Einstellungen in `Logging.{providername}.LogLevel` überschreiben die Einstellungen in `Logging.LogLevel`. Beispielsweise überschreibt der Protokolliergrad in `Debug.LogLevel.Default` den Protokolliergrad in `LogLevel.Default`.
* Jeder Standardanbieter*alias* wird verwendet. Jeder Anbieter definiert einen *Alias*, der in der Konfiguration anstelle des vollqualifizierten Typnamens verwendet werden kann. Die folgenden Anbieteraliase sind integriert:
  * Konsole
  * Debug
  * EventSource
  * EventLog
  * AzureAppServicesFile
  * AzureAppServicesBlob
  * ApplicationInsights

## <a name="set-log-level-by-command-line-environment-variables-and-other-configuration"></a>Festlegen des Protokolliergrads über die Befehlszeile, Umgebungsvariablen und andere Konfigurationen

Der Protokolliergrad kann von einem beliebigen [Konfigurationsanbieter](xref:fundamentals/configuration/index) festgelegt werden. 

[!INCLUDE[](~/includes/environmentVarableColon.md)]

Die folgenden Befehle:

* Legen den Umgebungsschlüssel `Logging:LogLevel:Microsoft` auf den Wert `Information` unter Windows fest.
* Testen die Einstellungen, wenn eine App verwendet wird, die mit den ASP.NET Core-Webanwendungsvorlagen erstellt wurde. Der `dotnet run`-Befehl muss im Projektverzeichnis nach der Verwendung von `set` ausgeführt werden.

```cmd
set Logging__LogLevel__Microsoft=Information
dotnet run
```

Die oben gezeigte Umgebungseinstellungen:

* Werden nur in Prozessen festgelegt, die über das Befehlsfenster gestartet werden, in dem sie festgelegt wurden.
* Werden nicht von Browsern gelesen, die mit Visual Studio gestartet wurden.

Mit dem folgenden [setx](/windows-server/administration/windows-commands/setx)-Befehl werden auch der Umgebungsschlüssel und der Wert unter Windows festgelegt. Anders als `set` werden `setx`-Einstellungen beibehalten. Der Schalter `/M` legt die Variable in der Systemumgebung fest. Wenn `/M` nicht verwendet wird, wird eine Benutzerumgebungsvariable festgelegt.

```cmd
setx Logging__LogLevel__Microsoft=Information /M
```

Wählen Sie in [Azure App Service](https://azure.microsoft.com/services/app-service/) auf der Seite **Einstellungen > Konfiguration** die Option **Neue Anwendungseinstellung** aus. Anwendungseinstellungen von Azure App Service werden:

* Im Ruhezustand verschlüsselt und über einen verschlüsselten Kanal übermittelt.
* Als Umgebungsvariablen verfügbar gemacht.

Weitere Informationen finden Sie unter [Azure-Apps: Überschreiben der App-Konfiguration im Azure-Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).

Weitere Informationen zum Festlegen von ASP.NET Core-Konfigurationswerten mithilfe von Umgebungsvariablen finden Sie unter [Umgebungsvariablen](xref:fundamentals/configuration/index#environment-variables). Informationen zur Verwendung anderer Konfigurationsquellen, einschließlich der Befehlszeile, Azure Key Vault, Azure App Configuration, anderer Dateiformate und mehr finden Sie unter <xref:fundamentals/configuration/index>.

## <a name="how-filtering-rules-are-applied"></a>Anwendung von Filterregeln

Wenn ein <xref:Microsoft.Extensions.Logging.ILogger%601>-Objekt erstellt wird, wählt das <xref:Microsoft.Extensions.Logging.ILoggerFactory>-Objekt eine einzige Regel pro Anbieter aus, die auf diese Protokollierung angewendet wird. Alle von einer `ILogger`-Instanz geschriebenen Meldungen werden auf Grundlage der ausgewählten Regeln gefiltert. Aus den verfügbaren Regeln wird die für jeden Anbieter und jedes Kategoriepaar spezifischste Regel ausgewählt.

Beim Erstellen von `ILogger` für eine vorgegebene Kategorie wird für jeden Anbieter der folgende Algorithmus verwendet:

* Es werden alle Regeln ausgewählt, die dem Anbieter oder dem zugehörigen Alias entsprechen. Wenn keine Übereinstimmung gefunden wird, werden alle Regeln mit einem leeren Anbieter ausgewählt.
* Aus dem Ergebnis des vorherigen Schritts werden die Regeln mit dem längsten übereinstimmenden Kategoriepräfix ausgewählt. Wenn keine Übereinstimmung gefunden wird, werden alle Regeln ohne Angabe einer Kategorie ausgewählt.
* Wenn mehrere Regeln ausgewählt sind, wird die **letzte** Regel verwendet.
* Wenn keine Regel ausgewählt ist, wird `MinimumLevel` verwendet.

<a name="dnrvs"></a>

## <a name="logging-output-from-dotnet-run-and-visual-studio"></a>Protokollieren der Ausgabe von dotnet run und Visual Studio

Protokolle, die mit den [Standardprotokollierungsanbietern](#lp) erstellt werden, werden angezeigt:

* In Visual Studio
  * Im Ausgabefenster „Debuggen“ beim Debuggen.
  * Im ASP.NET Core Web Server-Fenster.
* Im Konsolenfenster, wenn die App mit `dotnet run` ausgeführt wird.

Protokolle, die mit „Microsoft“-Kategorien beginnen, stammen aus ASP.NET Core-Frameworkcode. ASP.NET Core und Anwendungscode verwenden dieselbe Protokollierungs-API und dieselben Protokollierungsanbieter.

<a name="lcat"></a>

## <a name="log-category"></a>Protokollkategorie

Wenn ein `ILogger`-Objekt erstellt wird, wird eine *Kategorie* angegeben. Diese Kategorie ist in jeder Protokollmeldung enthalten, die von dieser Instanz von `ILogger` erstellt wird. Die Kategoriezeichenfolge ist willkürlich, aber die Konvention besteht darin, den Klassennamen zu verwenden. Beispielsweise kann in einem Controller der Name `"TodoApi.Controllers.TodoController"` lauten. Die ASP.NET Core-Web-Apps verwenden `ILogger<T>` zum automatischen Abrufen einer `ILogger`-Instanz, die den vollqualifizierten Typnamen von `T` als Kategorie verwendet:

[!code-csharp[](index/samples/3.x/TodoApiDTO/Pages/Privacy.cshtml.cs?name=snippet)]

Um die Kategorie explizit anzugeben, rufen Sie `ILoggerFactory.CreateLogger` auf:

[!code-csharp[](index/samples/3.x/TodoApiDTO/Pages/Contact.cshtml.cs?name=snippet)]

`ILogger<T>` entspricht dem Aufruf von `CreateLogger` mit dem vollqualifizierten Typnamen `T`.

<a name="llvl"></a>

## <a name="log-level"></a>Protokolliergrad

In der folgenden Tabelle werden die <xref:Microsoft.Extensions.Logging.LogLevel>-Werte, die Erweiterungsmethode `Log{LogLevel}` und die empfohlene Syntax aufgeführt:

| LogLevel | Wert | Methode | BESCHREIBUNG |
| -------- | ----- | ------ | ----------- |
| [Ablaufverfolgung](xref:Microsoft.Extensions.Logging.LogLevel) | 0 | <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogTrace%2A> | Enthält die ausführlichsten Meldungen. Diese Meldungen enthalten möglicherweise sensible App-Daten. Sie sind standardmäßig deaktiviert und sollten ***nicht*** in einer Produktionsumgebung aktiviert werden. |
| [Debuggen](xref:Microsoft.Extensions.Logging.LogLevel) | 1 | <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogDebug%2A> | Zum Debuggen und für die Entwicklung. Wegen des großen Volumens in der Produktion mit Vorsicht zu verwenden. |
| [Information](xref:Microsoft.Extensions.Logging.LogLevel) | 2 | <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation%2A> | Verfolgt den allgemeinen Ablauf der App nach. Kann über einen langfristigen Wert verfügen. |
| [Warnung](xref:Microsoft.Extensions.Logging.LogLevel) | 3 | <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogWarning%2A> | Für ungewöhnliche oder unerwartete Ereignisse. Schließt in der Regel Fehler oder Bedingungen ein, die nicht bewirken, dass die App fehlschlägt. |
| [Fehler](xref:Microsoft.Extensions.Logging.LogLevel) | 4 | <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogError%2A> | Für Fehler und Ausnahmen, die nicht behandelt werden können. Diese Meldungen weisen auf einen Fehler im aktuellen Vorgang oder der aktuellen Anforderung und nicht auf einen anwendungsweiten Fehler hin. |
| [Critical](xref:Microsoft.Extensions.Logging.LogLevel) (Kritisch) | 5 | <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogCritical%2A> | Für Fehler, die sofortige Aufmerksamkeit erfordern. Beispiel: Szenarios mit Datenverlust, Speichermangel. |
| [Keine](xref:Microsoft.Extensions.Logging.LogLevel) | 6 | | Gibt an, dass eine Protokollierungskategorie keine Meldungen schreiben soll. |

In der Tabelle oben wird der `LogLevel` vom niedrigsten bis zum höchsten Schweregrad aufgelistet.

Der erste Parameter der [Log](xref:Microsoft.Extensions.Logging.LoggerExtensions)-Methode (<xref:Microsoft.Extensions.Logging.LogLevel>) gibt den Schweregrad des Protokolls an. Anstatt `Log(LogLevel, ...)` aufzurufen, rufen die meisten Entwickler die [Log{LogLevel}](xref:Microsoft.Extensions.Logging.LoggerExtensions)-Erweiterungsmethoden auf. Die `Log{LogLevel}`-Erweiterungsmethoden [rufen die Log-Methode auf und geben den LogLevel an](https://github.com/dotnet/extensions/blob/release/3.1/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs). Beispielsweise sind die folgenden beiden Protokollierungsaufrufe funktionell gleichwertig und generieren das gleiche Protokoll:

[!code-csharp[](index/samples/3.x/TodoApiDTO/Controllers/TestController.cs?name=snippet0&highlight=6-7)]

`MyLogEvents.TestItem` ist die Ereignis-ID. `MyLogEvents` ist Teil der Beispiel-App und wird im Abschnitt [Log Event ID](#leid) (Protokollereignis-ID) angezeigt.

[!INCLUDE[](~/includes/MyDisplayRouteInfoBoth.md)]

Der folgende Code erstellt `Information`- und `Warning`-Protokolle:

[!code-csharp[](index/samples/3.x/TodoApiDTO/Controllers/TodoItemsController.cs?name=snippet_CallLogMethods&highlight=4,10)]

Im Code oben ist der erste `Log{LogLevel}`-Parameter (`MyLogEvents.GetItem`) die [Protokollereignis-ID](#leid). Der zweite Parameter ist eine Meldungsvorlage mit Platzhaltern für Argumentwerte, die von den verbleibenden Methodenparametern bereitgestellt werden. Die Methodenparameter werden im [Abschnitt „Meldungsvorlage“](#lmt) weiter unten in diesem Dokument erläutert.

Rufen Sie die entsprechende `Log{LogLevel}`-Methode auf, um die Menge an Protokollausgabedaten zu steuern, die in ein bestimmtes Speichermedium geschrieben werden. Zum Beispiel:

* In einer Produktionsumgebung
  * Das Protokollieren mit den Protokolliergraden `Trace` oder `Information` verursacht viele detaillierte Protokollmeldungen. Um die Kosten zu kontrollieren und die Datenspeichergrenzen nicht zu überschreiten, protokollieren Sie Meldungen der Protokolliergrade `Trace` und `Information` in einem kostengünstigen Datenspeicher mit hohem Datenvolumen. Erwägen Sie eine Beschränkung von `Trace` und `Information` auf bestimmte Kategorien.
  * Bei der Protokollierung mit den Protokolliergraden `Warning` bis `Critical` sollten nur wenige Protokollmeldungen generiert werden.
    * Kosten und Speichergrenzwerte stellen in der Regel kein Problem dar.
    * Wenige Protokolle ermöglichen eine größere Flexibilität bei der Auswahl von Datenspeichern.
* Bei der Entwicklung:
  * Legen Sie diese Option auf `Warning` fest.
  * Fügen Sie bei der Problembehandlung `Trace`- oder `Information`-Meldungen hinzu. Um die Ausgabe einzuschränken, legen Sie `Trace` oder `Information` nur für die zu untersuchenden Kategorien fest.

ASP.NET Core schreibt Protokolle für Frameworkereignisse. Berücksichtigen Sie z. B. die Protokollausgabe für Folgendes:

* Eine Razor Pages-App, die mit den ASP.NET Core-Vorlagen erstellt wurde.
* Protokollierung festgelegt auf `Logging:Console:LogLevel:Microsoft:Information`
* Navigation Sie zur Seite „Datenschutz“:

```console
info: Microsoft.AspNetCore.Hosting.Diagnostics[1]
      Request starting HTTP/2 GET https://localhost:5001/Privacy
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[0]
      Executing endpoint '/Privacy'
info: Microsoft.AspNetCore.Mvc.RazorPages.Infrastructure.PageActionInvoker[3]
      Route matched with {page = "/Privacy"}. Executing page /Privacy
info: Microsoft.AspNetCore.Mvc.RazorPages.Infrastructure.PageActionInvoker[101]
      Executing handler method DefaultRP.Pages.PrivacyModel.OnGet - ModelState is Valid
info: Microsoft.AspNetCore.Mvc.RazorPages.Infrastructure.PageActionInvoker[102]
      Executed handler method OnGet, returned result .
info: Microsoft.AspNetCore.Mvc.RazorPages.Infrastructure.PageActionInvoker[103]
      Executing an implicit handler method - ModelState is Valid
info: Microsoft.AspNetCore.Mvc.RazorPages.Infrastructure.PageActionInvoker[104]
      Executed an implicit handler method, returned result Microsoft.AspNetCore.Mvc.RazorPages.PageResult.
info: Microsoft.AspNetCore.Mvc.RazorPages.Infrastructure.PageActionInvoker[4]
      Executed page /Privacy in 74.5188ms
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[1]
      Executed endpoint '/Privacy'
info: Microsoft.AspNetCore.Hosting.Diagnostics[2]
      Request finished in 149.3023ms 200 text/html; charset=utf-8
```

Der folgende JSON-Code legt `Logging:Console:LogLevel:Microsoft:Information` fest:

[!code-json[](index/samples/3.x/TodoApiDTO/appsettings.MSFT.json)]

<a name="leid"></a>

## <a name="log-event-id"></a>Protokollereignis-ID

Jedes Protokoll kann eine *Ereignis-ID* angeben. Die Beispiel-App verwendet die `MyLogEvents`-Klasse zum Definieren von Ereignis-IDs:

<!-- Review: to bad there is no way to use an enum for event ID's -->
[!code-csharp[](index/samples/3.x/TodoApiDTO/Models/MyLogEvents.cs?name=snippet_LoggingEvents)]

[!code-csharp[](index/samples/3.x/TodoApiDTO/Controllers/TodoItemsController.cs?name=snippet_CallLogMethods&highlight=4,10)]

Eine Ereignis-ID ordnet eine Gruppe von Ereignissen zu. Beispielsweise können alle Protokolle, die sich auf die Anzeige einer Liste von Elementen auf einer Seite beziehen, 1001 sein.

Der Protokollierungsanbieter kann die Ereignis-ID in einem ID-Feld, in der Protokollierungsmeldung oder gar nicht speichern. Der Debuganbieter zeigt keine Ereignis-IDs an. Der Konsolenanbieter zeigt Ereignis-IDs in Klammern hinter der Kategorie an:

```console
info: TodoApi.Controllers.TodoItemsController[1002]
      Getting item 1
warn: TodoApi.Controllers.TodoItemsController[4000]
      Get(1) NOT FOUND
```

Einige Protokollierungsanbieter speichern die Ereignis-ID in einem Feld, das das Filtern nach der ID ermöglicht.

<a name="lmt"></a>

## <a name="log-message-template"></a>Protokollmeldungsvorlage
<!-- Review, Each log API uses a message template. -->
Jede Protokoll-API verwendet eine Meldungsvorlage. Die Meldungsvorlage kann Platzhalter enthalten, für die Argumente bereitgestellt werden. Verwenden Sie Namen für die Platzhalter, keine Zahlen.

[!code-csharp[](index/samples/3.x/TodoApiDTO/Controllers/TodoItemsController.cs?name=snippet_CallLogMethods&highlight=4,10)]

Die Reihenfolge der Platzhalter – nicht ihre Namen – bestimmt, welche Parameter verwendet werden, um ihre Werte zur Verfügung zu stellen. Im folgenden Code befinden sich die Parameternamen in der Meldungsvorlage nicht in der richtigen Reihenfolge:

```csharp
string p1 = "param1";
string p2 = "param2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

Dieser Code oben erstellt eine Protokollierungsmeldung mit den Parameterwerten in der richtigen Reihenfolge:

```text
Parameter values: param1, param2
```

Dieser Ansatz ermöglicht es Protokollierungsanbietern, [semantische oder strukturierte Protokollierung](https://github.com/NLog/NLog/wiki/How-to-use-structured-logging) zu implementieren. Die Argumente selbst (nicht nur die formatierte Meldungsvorlage) werden an das Protokollierungssystem übergeben. Dies ermöglicht es Protokollierungsanbietern, die Parameterwerte als Felder zu speichern. Sehen Sie sich z. B. die folgende Protokollierungsmethode an:

```csharp
_logger.LogInformation("Getting item {Id} at {RequestTime}", id, DateTime.Now);
```

Beispielsweise bei der Protokollierung in Azure Table Storage:

* Jede Azure Table-Entität kann über `ID`- und `RequestTime`-Eigenschaften verfügen.
* Tabellen mit Eigenschaften vereinfachen Abfragen für protokollierte Daten. Beispielsweise kann eine Abfrage alle Protokolle innerhalb eines bestimmten `RequestTime`-Bereichs ermitteln, ohne die Zeitangabe aus der Textnachricht analysieren zu müssen.

## <a name="log-exceptions"></a>Protokollieren von Ausnahmen

Die Protokollierungsmethoden verfügen über Überladungen, die einen Ausnahmeparameter annehmen:

[!code-csharp[](index/samples/3.x/TodoApiDTO/Controllers/TestController.cs?name=snippet_Exp)]

[!INCLUDE[](~/includes/MyDisplayRouteInfoBoth.md)]

Ausnahmeprotokollierung ist anbieterspezifisch.

### <a name="default-log-level"></a>Standardprotokolliergrad

Wenn der Standardprotokolliergrad nicht festgelegt ist, ist der Standardwert des Standardprotokolliergrads `Information`.

Betrachten Sie z. B. die folgende Web-App:

* Sie wurde mit den ASP.NET-Web-App-Vorlagen erstellt.
* *appsettings.json* und *appsettings.Development.json* wurden gelöscht oder umbenannt.

Mit dem oben beschriebenen Setup generiert die Navigation zur Datenschutz- oder Homepage viele `Trace`-, `Debug`- und `Information`-Meldungen mit `Microsoft` im Kategorienamen.

Mit dem folgenden Code wird der Standardprotokolliergrad festgelegt, wenn der Standardprotokolliergrad nicht in der Konfiguration festgelegt ist:

[!code-csharp[](index/samples/3.x/MyMain/Program.cs?name=snippet_MinLevel&highlight=10)]

<!-- review required: I say this a couple times -->
Im Allgemeinen sollten Standardprotokolliergrads in der Konfiguration und nicht im Code angegeben werden.

### <a name="filter-function"></a>Filterfunktion

Eine Filterfunktion wird für alle Anbieter und Kategorien aufgerufen, denen keine Regeln durch Konfiguration oder Code zugewiesen sind:

[!code-csharp[](index/samples/3.x/MyMain/Program.cs?name=snippet_FilterFunction)]

Der Code oben zeigt Konsolenprotokolle an, wenn die Kategorie `Controller` oder `Microsoft` enthält und der Protokolliergrad `Information` oder höher ist.

Im Allgemeinen sollten Standardprotokolliergrads in der Konfiguration und nicht im Code angegeben werden.

## <a name="aspnet-core-and-ef-core-categories"></a>ASP.NET Core- und EF Core-Kategorien

Die folgende Tabelle enthält einige Kategorien, die von ASP.NET Core und Entity Framework Core verwendet werden, mit Hinweisen zu den Protokollen:

| Kategorie                            | Hinweise |
| ----------------------------------- | ----- |
| Microsoft.AspNetCore                | Allgemeine ASP.NET Core-Diagnose. |
| Microsoft.AspNetCore.DataProtection | Gibt an, welche Schlüssel in Betracht gezogen, gefunden und verwendet wurden. |
| Microsoft.AspNetCore.HostFiltering  | Hosts sind zulässig. |
| Microsoft.AspNetCore.Hosting        | Gibt an, wie lange es bis zum Abschluss von HTTP-Anforderungen gedauert hat und zu welcher Zeit sie gestartet wurden. Gibt an, welche Hostingstartassemblys geladen wurden. |
| Microsoft.AspNetCore.Mvc            | MVC und Razor-Diagnose Modellbindung, Filterausführung, Ansichtskompilierung, Aktionsauswahl. |
| Microsoft.AspNetCore.Routing        | Gibt Routenabgleichsinformationen an. |
| Microsoft.AspNetCore.Server         | Start-, Beendigungs- und Keep-Alive-Antworten der Verbindung. HTTPS-Zertifikatinformationen. |
| Microsoft.AspNetCore.StaticFiles    | Die bereitgestellten Dateien. |
| Microsoft.EntityFrameworkCore       | Allgemeine Entity Framework Core-Diagnose. Datenbankaktivität und -konfiguration, Änderungserkennung, Migrationen. |

Wenn Sie weitere Kategorien im Konsolenfenster anzeigen möchten, legen Sie **appsettings.Development.json** auf Folgendes fest:

[!code-json[](index/samples/3.x/MyMain/appsettings.Trace.json)]

<!-- Review: What other providers support scopes? Console is not generally used in staging/production  -->

<a name="logscopes"></a>

## <a name="log-scopes"></a>Protokollbereiche

 Ein *Bereich* kann einen Satz logischer Vorgänge gruppieren. Diese Gruppierung kann verwendet werden, um an jedes Protokoll, das als Teil einer Gruppe erstellt wird, die gleichen Daten anzufügen. So kann beispielsweise jedes Protokoll, das im Rahmen der Verarbeitung einer Transaktion erstellt wird, die Transaktions-ID enthalten.

Ein Bereich:

* Ist ein <xref:System.IDisposable>-Typ, der von der <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*>-Methode zurückgegeben wird.
* Er bleibt bestehen, bis er verworfen wird.

Die folgenden Anbieter unterstützen Bereiche:

* `Console`
* [AzureAppServicesFile und AzureAppServicesBlob](xref:Microsoft.Extensions.Logging.AzureAppServices.BatchingLoggerOptions.IncludeScopes)

Verwenden Sie einen Bereich, indem Sie Protokollierungsaufrufe mit einem `using`-Block umschließen, der als Wrapper verwendet wird:

[!code-csharp[](index/samples/3.x/TodoApiDTO/Controllers/TestController.cs?name=snippet_Scopes)]

Der folgende JSON-Code aktiviert Bereiche für den Konsolenanbieter:

[!code-json[](index/samples/3.x/TodoApiDTO/appsettings.Scopes.json)]

Der folgende Code aktiviert Bereiche für den Konsolenanbieter:

[!code-csharp[](index/samples/3.x/MyMain/Program.cs?name=snippet_Scopes)]

Im Allgemeinen sollte Protokollierung in der Konfiguration und nicht im Code angegeben werden.

<a name="bilp"></a>

## <a name="built-in-logging-providers"></a>Integrierte Protokollierungsanbieter

ASP.NET Core umfasst die folgenden Protokollierungsanbieter:

* [Konsole](#console)
* [Debuggen](#debug)
* [EventSource](#event-source)
* [EventLog](#welog)
* [AzureAppServicesFile und AzureAppServicesBlob](#azure-app-service)
* [ApplicationInsights](#azure-application-insights)

Informationen zu `stdout` und zur Debugprotokollierung mit dem ASP.NET Core-Modul finden Sie unter <xref:test/troubleshoot-azure-iis> und <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.

### <a name="console"></a>Konsole

Der `Console`-Anbieter protokolliert die Ausgabe in der Konsole. Weitere Informationen zum Anzeigen von `Console`-Protokollen in der Entwicklung finden Sie unter [ Protokollieren der Ausgabe von dotnet run und Visual Studio](#dnrvs).

### <a name="debug"></a>Debug

Der `Debug`-Anbieter schreibt die Protokollausgabe mithilfe der [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug)-Klasse. Aufrufe von `System.Diagnostics.Debug.WriteLine` schreiben in den `Debug`-Anbieter.

Unter Linux ist der Speicherort des Protokolls des `Debug`-Anbieters abhängig von der Distribution und kann wie folgt lauten:

* */var/log/message*
* */var/log/syslog*

### <a name="event-source"></a>Ereignisquelle

Der `EventSource`-Anbieter schreibt in eine plattformübergreifende Ereignisquelle mit dem Namen `Microsoft-Extensions-Logging`. Unter Windows verwendet der Anbieter die [Ereignisablaufverfolgung für Windows (ETW)](https://msdn.microsoft.com/library/windows/desktop/bb968803).

#### <a name="dotnet-trace-tooling"></a>dotnet-trace-Tool

Das Tool [dotnet-trace](/dotnet/core/diagnostics/dotnet-trace) ist ein plattformübergreifendes globales Befehlszeilenschnittstellentool zum Sammeln von .NET Core-Ablaufverfolgungen eines ausgeführten Prozesses. Das Tool sammelt <xref:Microsoft.Extensions.Logging.EventSource>-Anbieterdaten mithilfe einer <xref:Microsoft.Extensions.Logging.EventSource.LoggingEventSource>.

Installationsanweisungen finden Sie unter [dotnet-trace](/dotnet/core/diagnostics/dotnet-trace).

Verwenden Sie das dotnet-trace-Tool, um die Ablaufverfolgung aus einer App zu erfassen:

1. Führen Sie die App mit dem Befehl `dotnet run` aus.
1. Bestimmen Sie den Prozessbezeichner der .NET Core-App:
   * Unter Windows können Sie einen der folgenden Ansätze verwenden:
     * Task-Manager (STRG+ALT+ENTF)
     * [tasklist-Befehl](/windows-server/administration/windows-commands/tasklist)
     * [PowerShell-Befehl „Get-Process“](/powershell/module/microsoft.powershell.management/get-process)
   * Verwenden Sie unter Linux den [pidof-Befehl](https://refspecs.linuxfoundation.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/pidof.html).

   Suchen Sie den Prozessbezeichner für den Prozess, der den gleichen Namen wie die App-Assembly hat.

1. Führen Sie den `dotnet trace`-Befehl aus.

   Allgemeine Befehlssyntax:

   ```dotnetcli
   dotnet trace collect -p {PID} 
       --providers Microsoft-Extensions-Logging:{Keyword}:{Event Level}
           :FilterSpecs=\"
               {Logger Category 1}:{Event Level 1};
               {Logger Category 2}:{Event Level 2};
               ...
               {Logger Category N}:{Event Level N}\"
   ```

   Wenn Sie eine PowerShell-Befehlsshell verwenden, schließen Sie den `--providers`-Wert in einfache Anführungszeichen (`'`) ein:

   ```dotnetcli
   dotnet trace collect -p {PID} 
       --providers 'Microsoft-Extensions-Logging:{Keyword}:{Event Level}
           :FilterSpecs=\"
               {Logger Category 1}:{Event Level 1};
               {Logger Category 2}:{Event Level 2};
               ...
               {Logger Category N}:{Event Level N}\"'
   ```

   Auf Plattformen, die nicht unter Windows ausgeführt werden, fügen Sie die `-f speedscope`-Option hinzu, um das Format der Ablaufverfolgungsdatei der Ausgabe in `speedscope` zu ändern.

   | Stichwort | BESCHREIBUNG |
   | :-----: | ----------- |
   | 1       | Protokolliert Metaereignisse über die `LoggingEventSource`. Es werden keine Ereignisse von `ILogger`) protokolliert. |
   | 2       | Aktiviert das `Message`-Ereignis, wenn `ILogger.Log()` aufgerufen wird. Die Informationen werden in einer programmgesteuerten (nicht formatierten) Weise ausgegeben. |
   | 4       | Aktiviert das `FormatMessage`-Ereignis, wenn `ILogger.Log()` aufgerufen wird. Gibt die formatierte Zeichenfolgeversion der Informationen an. |
   | 8       | Aktiviert das `MessageJson`-Ereignis, wenn `ILogger.Log()` aufgerufen wird. Stellt eine JSON-Darstellung der Argumente bereit. |

   | Ereignisgrad | Beschreibung     |
   | :---------: | --------------- |
   | 0           | `LogAlways`     |
   | 1           | `Critical`      |
   | 2           | `Error`         |
   | 3           | `Warning`       |
   | 4           | `Informational` |
   | 5           | `Verbose`       |

   `FilterSpecs`-Einträge für `{Logger Category}` und `{Event Level}` stellen zusätzliche Protokollfilterbedingungen dar. Trennen Sie die `FilterSpecs`-Einträge durch ein Semikolon (`;`).

   Beispiel mit einer Windows-Befehlsshell (**keine** einfachen Anführungszeichen um den `--providers`-Wert):

   ```dotnetcli
   dotnet trace collect -p {PID} --providers Microsoft-Extensions-Logging:4:2:FilterSpecs=\"Microsoft.AspNetCore.Hosting*:4\"
   ```

   Mit dem vorangestellten Komma wird Folgendes aktiviert:

   * Die Ereignisquellenprotokollierung zur Erzeugung von formatierten Zeichenfolgen (`4`) für Fehler (`2`)
   * `Microsoft.AspNetCore.Hosting`-Protokollierung auf `Informational`-Protokollierungsebene (`4`)

1. Halten Sie das dotnet-trace-Tool an, indem Sie die EINGABETASTE oder STRG+C drücken.

   Die Ablaufverfolgung wird mit dem Namen *trace.nettrace* in dem Ordner gespeichert, in dem der `dotnet trace`-Befehl ausgeführt wird.

1. Öffnen Sie die Ablaufverfolgung mit [Perfview](#perfview). Öffnen Sie die Datei *trace.nettrace*, und untersuchen Sie die Ablaufverfolgungsereignisse.

Wenn die App den Host nicht mit `CreateDefaultBuilder` erstellt, fügen Sie den [Ereignisquellenanbieter](#event-source-provider) zur Protokollierungskonfiguration der App hinzu.

Weitere Informationen finden Sie unter:

* [Ablaufverfolgung für das Hilfsprogramm zur Leistungsanalyse (dotnet-trace)](/dotnet/core/diagnostics/dotnet-trace) (.NET Core-Dokumentation)
* [Ablaufverfolgung für das Hilfsprogramm zur Leistungsanalyse (dotnet-trace)](https://github.com/dotnet/diagnostics/blob/master/documentation/dotnet-trace-instructions.md) (GitHub-Repository-Dokumentation zu dotnet/diagnostics)
* [LoggingEventSource-Klasse](xref:Microsoft.Extensions.Logging.EventSource.LoggingEventSource) (.NET API-Browser)
* <xref:System.Diagnostics.Tracing.EventLevel>
* [LoggingEventSource-Verweisquelle (3.0)](https://github.com/dotnet/extensions/blob/release/3.0/src/Logging/Logging.EventSource/src/LoggingEventSource.cs): Ändern Sie für das Abrufen der Verweisquelle für eine andere Version den Branch in `release/{Version}`, wobei `{Version}` die Version der gewünschten ASP.NET Core-Version darstellt.
* [Perfview](#perfview): Hilfreich zum Anzeigen der Ablaufverfolgung für Ereignisquellen.

#### <a name="perfview"></a>PerfView

Verwenden Sie das [PerfView-Hilfsprogramm](https://github.com/Microsoft/perfview) zum Sammeln und Anzeigen von Protokollen. Es gibt andere Tools zur Anzeige von ETW-Protokollen, aber PerfView bietet die besten Ergebnisse bei der Arbeit mit ETW-Ereignissen, die von ASP.NET Core ausgegeben werden.

Um PerfView für das Erfassen von Ereignissen zu konfigurieren, die von diesem Anbieter protokolliert wurden, fügen Sie die Zeichenfolge `*Microsoft-Extensions-Logging` zur Liste **Zusätzliche Anbieter** hinzu. Vergessen Sie nicht das `*` am Anfang der Zeichenfolge.

<a name="welog"></a>

### <a name="windows-eventlog"></a>Windows-EventLog

Der `EventLog`-Anbieter sendet die Protokollausgabe an das Windows-Ereignisprotokoll. Im Gegensatz zu den anderen Anbietern erbt der `EventLog`-Anbieter ***nicht*** die Standardeinstellungen für Nicht-Anbieter. Wenn keine `EventLog`-Protokolleinstellungen angegeben werden, wird [standardmäßig LogLevel.Warning](https://github.com/dotnet/extensions/blob/release/3.1/src/Hosting/Hosting/src/Host.cs#L99-L103) verwendet.

Legen Sie die Protokollierungsebene fest, um Ereignisse, die niedriger als <xref:Microsoft.Extensions.Logging.LogLevel.Warning?displayProperty=nameWithType> sind, zu protokollieren. Im folgenden Beispiel wird der Standardprotokolliergrad auf <xref:Microsoft.Extensions.Logging.LogLevel.Information?displayProperty=nameWithType> festgelegt:

```json
"Logging": {
  "EventLog": {
    "LogLevel": {
      "Default": "Information"
    }
  }
}
```

[AddEventLog-Überladungen](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions) können <xref:Microsoft.Extensions.Logging.EventLog.EventLogSettings> übergeben. Wenn `null` oder nicht angegeben, werden die folgenden Standardeinstellungen verwendet:

* `LogName`: „Anwendung“
* `SourceName`: „.NET Runtime“
* `MachineName`: Der Name des lokalen Computers wird verwendet.

Der folgende Code ändert `SourceName` aus dem Standardwert `".NET Runtime"` in `MyLogs`:

[!code-csharp[](index/samples/3.x/MyMain/Program.cs?name=snippetEventLog)]

### <a name="azure-app-service"></a>Azure App Service

Das Anbieterpaket [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) schreibt Protokolle in Textdateien in das Dateisystem einer Azure App Service-App und in [Blob Storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in einem Azure Storage-Konto.

Das Anbieterpaket ist nicht im freigegebenen Framework enthalten. Zum Verwenden des Anbieters müssen Sie das Anbieterpaket dem Projekt hinzufügen.

Verwenden Sie <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> und <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions> wie im folgenden Beispiel, um die Anbietereinstellungen zu konfigurieren:

[!code-csharp[](index/samples/3.x/MyMain/Program.cs?name=snippet_AzLogOptions)]

Wenn eine Bereitstellung in Azure App Service erfolgt, verwendet die App die Einstellungen im Abschnitt [App Service logs](/azure/app-service/web-sites-enable-diagnostic-log/#enable-application-logging-windows) (App Service-Protokolle) auf der Seite **App Service** im Azure-Portal. Bei einem Update der folgenden Einstellungen werden die Änderungen sofort wirksam, ohne dass ein Neustart oder eine erneute Bereitstellung der App notwendig ist.

* **Anwendungsprotokoll (Dateisystem)**
* **Anwendungsprotokoll (Blob)**

Der Standardspeicherort für Protokolldateien ist der Ordner *D:\\home\\LogFiles\\Application*, und der standardmäßige Dateiname lautet *diagnostics-yyyymmdd.txt*. Die Dateigröße ist standardmäßig auf 10 MB beschränkt, und die maximal zulässige Anzahl beibehaltener Dateien lautet 2. Der Standardblobname lautet *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.

Der Anbieter führt nur Protokollierung aus, wenn das Projekt in der Azure-Umgebung ausgeführt wird.

#### <a name="azure-log-streaming"></a>Azure-Protokollstreaming

Azure-Protokollstreaming unterstützt das Anzeigen der Protokollaktivität in Echtzeit:

* App-Server
* Webserver
* Ablaufverfolgung für Anforderungsfehler

So konfigurieren Sie das Azure-Protokollstreaming

* Navigieren Sie von der Portalseite Ihrer App zur Seite **App Service-Protokolle**.
* Legen Sie **Anwendungsprotokollierung (Dateisystem)** auf **Ein** fest.
* Wählen Sie die **Protokollierungsebene**. Diese Einstellung gilt nur für Azure-Protokollstreaming.

Navigieren Sie zur Seite **Log Stream** (Protokollstream), um Protokolle anzuzeigen. Die protokollierten Nachrichten werden mit der `ILogger`-Schnittstelle protokolliert.

### <a name="azure-application-insights"></a>Azure Application Insights

Das [Microsoft.Extensions.Logging.ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights)-Anbieterpaket schreibt Protokolle in [Azure Application Insights](/azure/azure-monitor/app/cloudservices). Application Insights ist ein Dienst, der eine Web-App überwacht und Tools für Abfragen und Analysen von Telemetriedaten zur Verfügung stellt. Wenn Sie diesen Anbieter verwenden, können Sie Ihre Protokolle mithilfe der Application Insights-Tools abfragen und analysieren.

Der Protokollierungsanbieter ist als Abhängigkeit von [Microsoft.ApplicationInsights.AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) enthalten, d.h. als das Paket, das alle verfügbaren Telemetriedaten für ASP.NET Core bereitstellt. Wenn Sie dieses Paket verwenden, ist nicht erforderlich, das Anbieterpaket zu installieren.

Das [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web)-Paket bezieht sich auf ASP.NET 4.x, nicht auf ASP.NET Core.

Weitere Informationen finden Sie in den folgenden Ressourcen:

* [Application Insights-Übersicht](/azure/application-insights/app-insights-overview)
* [Anwendungseinblicke für ASP.NET Core-Anwendungen](/azure/azure-monitor/app/asp-net-core): Beginnen Sie hier, wenn Sie die gesamte Bandbreite der Application Insights-Telemetrie zusammen mit der Protokollierung implementieren möchten.
* [ApplicationInsightsLoggerProvider für .NET Core ILogger-Protokolle](/azure/azure-monitor/app/ilogger): Beginnen Sie hier, wenn Sie den Protokollierungsanbieter ohne die übrige Application Insights-Telemetrie implementieren möchten.
* [Application Insights logging adapters (Adapter zur Protokollierung für Application Insights)](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-trace-logs).
* [Installieren, Konfigurieren und Initialisieren des Application Insights SDK](/learn/modules/instrument-web-app-code-with-application-insights): interaktive Tutorials auf der Microsoft Learn-Website.

## <a name="third-party-logging-providers"></a>Protokollierungsanbieter von Drittanbietern

Protokollierungsframeworks von Drittanbietern aufgeführt, die mit ASP.NET Core funktionieren:

* [elmah.io](https://elmah.io/) ([GitHub-Repository](https://github.com/elmahio/Elmah.Io.Extensions.Logging))
* [Gelf](https://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub-Repository](https://github.com/mattwcole/gelf-extensions-logging))
* [JSNLog](https://jsnlog.com/) ([GitHub-Repository](https://github.com/mperdeck/jsnlog))
* [KissLog.net](https://kisslog.net/) ([GitHub repo](https://github.com/catalingavan/KissLog-net))
* [Log4Net](https://logging.apache.org/log4net/) ([GitHub-Repository](https://github.com/huorswords/Microsoft.Extensions.Logging.Log4Net.AspNetCore))
* [Loggr](https://loggr.net/) ([GitHub-Repository](https://github.com/imobile3/Loggr.Extensions.Logging))
* [NLog](https://nlog-project.org/) ([GitHub-Repository](https://github.com/NLog/NLog.Extensions.Logging))
* [PLogger](https://www.nuget.org/packages/InvertedSoftware.PLogger.Core/) ([GitHub-Repository](https://github.com/invertedsoftware/InvertedSoftware.PLogger.Core))
* [Sentry](https://sentry.io/welcome/) ([GitHub-Repository](https://github.com/getsentry/sentry-dotnet))
* [Serilog](https://serilog.net/) ([GitHub-Repository](https://github.com/serilog/serilog-aspnetcore))
* [Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([GitHub-Repository](https://github.com/googleapis/google-cloud-dotnet))

Einige Drittanbieterframeworks können eine [semantische Protokollierung (auch als strukturierte Protokollierung bezeichnet)](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging) ausführen.

Die Verwendung eines Frameworks von Drittanbietern ist ähnlich wie die Verwendung eines integrierten Anbieters:

1. Fügen Sie Ihrem Paket ein NuGet-Paket hinzu.
1. Rufen Sie eine `ILoggerFactory`-Erweiterungsmethode auf, die vom Protokollierungsframework bereitgestellt wird.

Weitere Informationen finden Sie in der Dokumentation zum jeweiligen Anbieter. Protokollierungsanbieter von Drittanbietern werden von Microsoft nicht unterstützt.

<a name="nhca"></a>

## <a name="non-host-console-app"></a>Nicht-Host-Konsolen-App

Ein Beispiel zum Verwenden des generischen Hosts in einer nicht webbasierten Konsolen-App finden Sie in der *Program.cs*-Datei der [Beispiel-App für Hintergrundaufgaben](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples) (<xref:fundamentals/host/hosted-services>).

Das Protokollieren von Code für Apps ohne generischen Host weicht in Bezug auf das [Hinzufügen von Anbietern](#add-providers) und das [Erstellen von Protokollierungen](#create-logs) ab. 

### <a name="logging-providers"></a>Protokollierungsanbieter

Rufen Sie in einer Konsolen-App ohne Host die `Add{provider name}`-Erweiterungsmethode des Anbieters auf, während Sie eine `LoggerFactory` erstellen:

[!code-csharp[](index/samples/3.x/LoggingConsoleApp/Program.cs?name=snippet_LoggerFactory&highlight=11-12)]

### <a name="create-logs"></a>Erstellen von Protokollen

Verwenden Sie zum Erstellen von Protokollen ein <xref:Microsoft.Extensions.Logging.ILogger%601>-Objekt. Verwenden Sie `LoggerFactory`, um ein `ILogger`-Element zu erstellen.

Im folgenden Beispiel wird eine Protokollierung mit `LoggingConsoleApp.Program` als Kategorie erstellt.

[!code-csharp[](index/samples/3.x/LoggingConsoleApp/Program.cs?name=snippet_LoggerFactory&highlight=14)]

In den folgenden ASP.NET Core-Beispielen werden von der Protokollierung Protokolle mit dem Protokolliergrad `Information` erstellt. Der Protokollierungs*grad* gibt den Schweregrad des protokollierten Ereignisses an.

[!code-csharp[](index/samples/3.x/LoggingConsoleApp/Program.cs?name=snippet_LoggerFactory&highlight=15)]

[Grade](#log-level) und [Kategorien](#log-category) werden in diesem Dokument ausführlicher erläutert.

<a name="lhc"></a>

## <a name="log-during-host-construction"></a>Protokollierung während der Hosterstellung

Die Protokollierung während der Hosterstellung wird nicht direkt unterstützt. Es kann jedoch eine separate Protokollierung verwendet werden. Im folgenden Beispiel wird eine [Serilog](https://serilog.net/)-Protokollierung zum Anmelden von `CreateHostBuilder` verwendet. `AddSerilog` verwendet die in `Log.Logger` angegebene statische Konfiguration:

```csharp
using System;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.Hosting;
using Microsoft.Extensions.Logging;

public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args)
    {
        var builtConfig = new ConfigurationBuilder()
            .AddJsonFile("appsettings.json")
            .AddCommandLine(args)
            .Build();

        Log.Logger = new LoggerConfiguration()
            .WriteTo.Console()
            .WriteTo.File(builtConfig["Logging:FilePath"])
            .CreateLogger();

        try
        {
            return Host.CreateDefaultBuilder(args)
                .ConfigureServices((context, services) =>
                {
                    services.AddRazorPages();
                })
                .ConfigureAppConfiguration((hostingContext, config) =>
                {
                    config.AddConfiguration(builtConfig);
                })
                .ConfigureLogging(logging =>
                {   
                    logging.AddSerilog();
                })
                .ConfigureWebHostDefaults(webBuilder =>
                {
                    webBuilder.UseStartup<Startup>();
                });
        }
        catch (Exception ex)
        {
            Log.Fatal(ex, "Host builder error");

            throw;
        }
        finally
        {
            Log.CloseAndFlush();
        }
    }
}
```

<a name="csdi"></a>

## <a name="configure-a-service-that-depends-on-ilogger"></a>Konfigurieren eines Diensts, der von ILogger abhängig ist

Die Konstruktorinjektion einer Protokollierung in `Startup` funktioniert in früheren Versionen von ASP.NET Core, da für den Webhost ein separater Abhängigkeitsinjektion-Container erstellt wird. Weitere Informationen dazu, warum nur ein Container für den generischen Host erstellt wird, erhalten Sie in der [Breaking Change-Ankündigung](https://github.com/aspnet/Announcements/issues/353).

Um einen Dienst zu konfigurieren, der von `ILogger<T>` abhängt, verwenden Sie die Konstruktorinjektion oder stellen eine Factorymethode bereit. Die Nutzung einer Factorymethode wird nur empfohlen, wenn Sie keine andere Wahl haben. Angenommen, ein Dienst benötigt z. B. eine durch Abhängigkeitsinjektion bereitgestellte `ILogger<T>`-Instanz:

[!code-csharp[](index/samples/3.x/TodoApiSample/Startup2.cs?name=snippet_ConfigureServices&highlight=6-10)]

Der hervorgehobene Code oben ist eine [Func](/dotnet/api/system.func-2), die ausgeführt wird, wenn der Abhängigkeitsinjektionscontainer erstmals eine Instanz von `MyService` erstellen muss. Auf diese Weise können Sie auf alle registrierten Dienste zugreifen.

<a name="clms"></a>

## <a name="create-logs-in-main"></a>Erstellen von Protokollen in Main

Der folgende Code führt Protokollierung in `Main` aus, indem nach dem Erstellen des Hosts eine `ILogger`-Instanz von der Abhängigkeitsinjektion abgerufen wird:

[!code-csharp[](index/samples/3.x/TodoApiDTO/Program.cs?name=snippet_LogProgram)]

### <a name="create-logs-in-startup"></a>Erstellen von Protokollen in der Startklasse

Der folgende Code schreibt Protokolle in `Startup.Configure`:

[!code-csharp[](index/samples/3.x/TodoApiDTO/Startup.cs?name=snippet_Configure)]

Das Schreiben von Protokollen vor Abschluss der Einrichtung des Abhängigkeitsinjektion-Containers in der `Startup.ConfigureServices`-Methode wird nicht unterstützt:

* Die Protokollierungsinjektion in den `Startup`-Konstruktor wird nicht unterstützt.
* Die Protokollierungsinjektion in die `Startup.ConfigureServices`-Methodensignatur wird nicht unterstützt.

Dies Einschränkung ist darauf zurückzuführen, dass die Protokollierung von der Abhängigkeitsinjektion und der Konfiguration abhängt (die wiederum von der Abhängigkeitsinjektion abhängt). Der Abhängigkeitsinjektion-Container wird erst nach Fertigstellung von `ConfigureServices` eingerichtet.

Informationen zum Konfigurieren eines Diensts, der von `ILogger<T>` abhängt, oder dazu, warum die Konstruktorinjektion einer Protokollierung in `Startup` in früheren Versionen funktioniert hat, finden Sie unter [Konfigurieren eines Diensts, der von ILogger abhängt](#csdi).

### <a name="no-asynchronous-logger-methods"></a>Keine asynchronen Protokollierungsmethoden

Die Protokollierung sollte so schnell erfolgen, dass sich die Leistungskosten von asynchronem Code nicht lohnen. Wenn ein Protokollierungsdatenspeicher langsam ist, schreiben Sie nicht direkt in diesen. Erwägen Sie, die Protokollnachrichten zunächst in einen schnellen Speicher zu schreiben und sie dann später in den langsamen Speicher zu verschieben. Wenn Sie beispielsweise in SQL Server protokollieren, verwenden Sie nicht direkt eine `Log`-Methode, da die `Log`-Methoden synchron sind. Fügen Sie stattdessen die Protokollmeldungen synchron zu einer Warteschlange im Arbeitsspeicher hinzu, und rufen Sie mithilfe eines Hintergrundworkers die Nachrichten aus der Warteschlange ab, um sie dann asynchron per Pushvorgang an SQL Server zu übertragen. Weitere Informationen finden Sie in [diesem GitHub-Issue](https://github.com/dotnet/AspNetCore.Docs/issues/11801).

<a name="clib"></a>

## <a name="change-log-levels-in-a-running-app"></a>Ändern von Protokolliergraden in einer aktuell ausgeführten App

Die Protokollierungs-API umfasst kein Szenario zum Ändern der Protokollebene, während eine App ausgeführt wird. Einige Konfigurationsanbieter können jedoch die Konfiguration erneut laden, was sich unmittelbar auf die Protokollierungskonfiguration auswirkt. Beispielsweise lädt der [Dateikonfigurationsanbieter](xref:fundamentals/configuration/index#file-configuration-provider) die Protokollierungskonfiguration standardmäßig erneut. Wenn die Konfiguration im Code geändert wird, während eine App ausgeführt wird, kann die App [IConfigurationRoot.Reload](xref:Microsoft.Extensions.Configuration.IConfigurationRoot.Reload*) aufrufen, um ihre Protokollierungskonfiguration zu aktualisieren.

## <a name="ilogger-and-iloggerfactory"></a>ILogger und ILoggerFactory

Die <xref:Microsoft.Extensions.Logging.ILogger%601>- und <xref:Microsoft.Extensions.Logging.ILoggerFactory>-Schnittstellen und -Implementierungen sind im .NET Core SDK enthalten. Sie sind auch in den folgenden NuGet-Paketen verfügbar:  

* Die Schnittstellen befinden sich in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/).
* Die Standardimplementierungen befinden sich in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).

<!-- review. Why would you want to hard code filtering rules in code? -->
<a name="fric"></a>

## <a name="apply-log-filter-rules-in-code"></a>Anwenden von Protokollfilterregeln in Code

Die bevorzugte Vorgehensweise zum Festlegen von Protokollfilterregeln ist die Verwendung von [Konfiguration](xref:fundamentals/configuration/index).

Das folgende Beispiel zeigt, wie Filterregeln im Code registriert werden:

[!code-csharp[](index/samples/3.x/MyMain/Program.cs?name=snippet_FilterInCode)]

`logging.AddFilter("System", LogLevel.Debug)` gibt die `System`-Kategorie und den Protokolliergrad `Debug` an. Der Filter wird auf alle Anbieter angewendet, da kein bestimmter Anbieter konfiguriert wurde.

`AddFilter<DebugLoggerProvider>("Microsoft", LogLevel.Information)` gibt Folgendes an:

* Den `Debug`-Protokollanbieter.
* Der Protokolliergrad `Information` oder höher.
* Alle Kategorien, die mit `"Microsoft"` beginnen.

## <a name="create-a-custom-logger"></a>Erstellen einer benutzerdefinierten Protokollierung

Fügen Sie einen <xref:Microsoft.Extensions.Logging.ILoggerProvider> mit einer <xref:Microsoft.Extensions.Logging.ILoggerFactory> hinzu, um eine benutzerdefinierte Protokollierung hinzuzufügen:

```csharp
public void Configure(
    IApplicationBuilder app,
    IWebHostEnvironment env,
    ILoggerFactory loggerFactory)
{
    loggerFactory.AddProvider(new CustomLoggerProvider(new CustomLoggerConfiguration()));
```

`ILoggerProvider` erstellt mindestens eine `ILogger`-Instanz. Die `ILogger`-Instanzen werden vom Framework zum Protokollieren der Informationen verwendet.

### <a name="sample-custom-logger-configuration"></a>Beispielkonfiguration für benutzerdefinierte Protokollierung

Im Beispiel geschieht Folgendes:

* Es ist ein sehr einfaches Beispiel, mit dem die Farbe der Protokollkonsole nach Ereignis-ID und Protokolliergrad festgelegt wird. Protokollierungen ändern sich in der Regel nicht anhand der Ereignis-ID und sind nicht spezifisch für den Protokolliergrad.
* Es werden unterschiedliche Farbkonsoleneinträge pro Protokolliergrad und Ereignis-ID mithilfe des folgenden Konfigurationstyps erstellt:

[!code-csharp[](index/samples/3.x/CustomLogger/ColorConsoleLogger/ColorConsoleLoggerConfiguration.cs?name=snippet)]

Der vorangehende Code legt den Standardprotokolliergrad auf `Warning` und die Farbe auf `Yellow` fest. Wenn die `EventId` auf 0 festgelegt ist, werden alle Ereignisse protokolliert.

### <a name="create-the-custom-logger"></a>Erstellen der benutzerdefinierten Protokollierung

Der Kategoriename der `ILogger`-Implementierung ist in der Regel die Protokollierungsquelle. Beispielsweise der Typ, in dem die Protokollierung erstellt wird:

[!code-csharp[](index/samples/3.x/CustomLogger/ColorConsoleLogger/ColorConsoleLogger.cs?name=snippet)]

Der vorangehende Code:

* Erstellt eine Protokollierungsinstanz pro Kategoriename.
* Überprüft `logLevel == _config.LogLevel` in `IsEnabled`, sodass jedes `logLevel`-Element über eine eindeutige Protokollierung verfügt. Protokollierungen sollten im Allgemeinen auch für alle höheren Protokolliergrade aktiviert werden:

```csharp
public bool IsEnabled(LogLevel logLevel)
{
    return logLevel >= _config.LogLevel;
}
```

### <a name="create-the-custom-loggerprovider"></a>Erstellen des benutzerdefinierten LoggerProvider

`LoggerProvider` ist die Klasse, mit der die Protokollierungsinstanzen erstellt werden. Möglicherweise ist sie nicht erforderlich, um eine Protokollierungsinstanz pro Kategorie zu erstellen. Dies ist jedoch für einige Protokollierungen sinnvoll, etwa für NLog oder log4net. Auf diese Weise können Sie bei Bedarf auch verschiedene Protokollierungsausgabeziele pro Kategorie auswählen:

[!code-csharp[](index/samples/3.x/CustomLogger/ColorConsoleLogger/ColorConsoleLoggerProvider.cs?name=snippet)]

Im Code oben erstellt <xref:Microsoft.Build.Logging.LoggerDescription.CreateLogger*> eine einzelne Instanz von `ColorConsoleLogger` pro Kategoriename und speichert sie in [`ConcurrentDictionary<TKey,TValue>`](/dotnet/api/system.collections.concurrent.concurrentdictionary-2);

### <a name="usage-and-registration-of-the-custom-logger"></a>Verwendung und Registrierung der benutzerdefinierten Protokollierung

Registrieren Sie die Protokollierung in `Startup.Configure`:

[!code-csharp[](index/samples/3.x/CustomLogger/Startup.cs?name=snippet)]

Geben Sie für den Code oben mindestens eine Erweiterungsmethode für `ILoggerFactory` an:

[!code-csharp[](index/samples/3.x/CustomLogger/ColorConsoleLogger/ColorConsoleLoggerExtensions.cs?name=snippet)]

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* <xref:fundamentals/logging/loggermessage>
* Fehler beim Protokollieren sollten im Repository [github.com/dotnet/runtime/](https://github.com/dotnet/runtime/issues) erstellt werden.
* <xref:blazor/fundamentals/logging>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Von [Tom Dykstra](https://github.com/tdykstra) und [Steve Smith](https://ardalis.com/)

.NET Core unterstützt eine Protokollierungs-API, die mit einer Vielzahl von integrierten Protokollierungsanbietern und Drittanbieter-Protokollierungslösungen zusammenarbeitet. Dieser Artikel zeigt, wie Sie die Protokollierungs-API mit integrierten Anbietern verwenden können.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([Vorgehensweise zum Herunterladen](xref:index#how-to-download-a-sample))

## <a name="add-providers"></a>Hinzufügen von Anbietern

Ein Protokollierungsanbieter zeigt Protokolle an oder speichert diese. Beispielsweise zeigt der Konsolenanbieter Protokolle in der Konsole an, und der Azure Application Insights-Anbieter speichert sie in Azure Application Insights. Protokolle können durch das Hinzufügen mehrerer Anbieter an mehrere Ziele gesendet werden.

Um einen Anbieter hinzuzufügen, rufen Sie die `Add{provider name}`-Erweiterungsmethode des Anbieters in *Program.cs* auf:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=18-20)]

Der vorstehende Code erfordert Verweise auf `Microsoft.Extensions.Logging` und `Microsoft.Extensions.Configuration`.

Die Standardprojektvorlage ruft die Methode <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A> auf, die die folgenden Protokollierungsanbieter hinzufügt:

* Konsole
* Debug
* EventSource (beginnend mit ASP.NET Core 2.2)

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_TemplateCode&highlight=7)]

Bei Verwendung von `CreateDefaultBuilder` können Sie die Standardanbieter durch Ihre eigene Auswahl ersetzen. Rufen Sie <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A> auf, und fügen Sie die gewünschten Anbieter hinzu.

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=18-22)]

Informationen zu den [integrierten Protokollierungsanbietern](#built-in-logging-providers) sowie zu [Protokollierungsanbietern von Drittanbietern](#third-party-logging-providers) finden Sie weiter unten in diesem Artikel.

## <a name="create-logs"></a>Erstellen von Protokollen

Verwenden Sie zum Erstellen von Protokollen ein <xref:Microsoft.Extensions.Logging.ILogger%601>-Objekt. Rufen Sie in einer Web-App oder einem gehosteten Dienst einen `ILogger` aus der Abhängigkeitsinjektion ab. Verwenden Sie in Konsolen-Apps ohne Host `LoggerFactory`, um einen `ILogger` zu erstellen.

Im folgenden ASP.NET Core-Beispiel wird eine Protokollierung mit `TodoApiSample.Pages.AboutModel` als Kategorie erstellt. Die Protokoll*kategorie* ist eine Zeichenfolge, die jedem Protokoll zugeordnet ist. Die von der Abhängigkeitsinjektion bereitgestellte `ILogger<T>`-Instanz erstellt Protokolle, die den vollqualifizierten Namen vom Typ `T` als Kategorie aufweisen. 

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_LoggerDI&highlight=3,5,7)]

In den folgenden Beispielen zu ASP.NET Core und einer Konsolen-App werden von der Protokollierung Protokolle mit dem Protokolliergrad `Information` erstellt. Der Protokollierungs*grad* gibt den Schweregrad des protokollierten Ereignisses an.

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_CallLogMethods&highlight=4)]

[Grade](#log-level) und [Kategorien](#log-category) werden weiter unten in diesem Artikel ausführlicher erläutert.

### <a name="create-logs-in-startup"></a>Erstellen von Protokollen in der Startklasse

Zum Schreiben von Protokollen in der `Startup`-Klasse schließen Sie einen `ILogger`-Parameter in die Konstruktorsignatur ein:

[!code-csharp[](index/samples/2.x/TodoApiSample/Startup.cs?name=snippet_Startup&highlight=3,5,8,20,27)]

### <a name="create-logs-in-the-program-class"></a>Erstellen von Protokollen in der Program-Klasse

Zum Schreiben von Protokollen in der `Program`-Klasse rufen Sie eine `ILogger`-Instanz aus DI ab:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=9,10)]

Die Protokollierung während der Hosterstellung wird nicht direkt unterstützt. Es kann jedoch eine separate Protokollierung verwendet werden. Im folgenden Beispiel wird eine [Serilog](https://serilog.net/)-Protokollierung zum Anmelden von `CreateWebHostBuilder` verwendet. `AddSerilog` verwendet die in `Log.Logger` angegebene statische Konfiguration:

```csharp
using System;
using Microsoft.AspNetCore;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.Logging;

public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var builtConfig = new ConfigurationBuilder()
            .AddJsonFile("appsettings.json")
            .AddCommandLine(args)
            .Build();

        Log.Logger = new LoggerConfiguration()
            .WriteTo.Console()
            .WriteTo.File(builtConfig["Logging:FilePath"])
            .CreateLogger();

        try
        {
            return WebHost.CreateDefaultBuilder(args)
                .ConfigureServices((context, services) =>
                {
                    services.AddMvc();
                })
                .ConfigureAppConfiguration((hostingContext, config) =>
                {
                    config.AddConfiguration(builtConfig);
                })
                .ConfigureLogging(logging =>
                {
                    logging.AddSerilog();
                })
                .UseStartup<Startup>();
        }
        catch (Exception ex)
        {
            Log.Fatal(ex, "Host builder error");

            throw;
        }
        finally
        {
            Log.CloseAndFlush();
        }
    }
}
```

### <a name="no-asynchronous-logger-methods"></a>Keine asynchronen Protokollierungsmethoden

Die Protokollierung sollte so schnell erfolgen, dass sich die Leistungskosten von asynchronem Code nicht lohnen. Wenn Ihr Protokollierungsdatenspeicher langsam ist, schreiben Sie nicht direkt in ihn. Erwägen Sie, die Protokollnachrichten zunächst in einen schnellen Speicher zu schreiben und sie dann später in den langsamen Speicher zu verschieben. Wenn Sie beispielsweise in SQL Server protokollieren, möchten Sie dies nicht direkt in einer `Log`-Methode tun, da die `Log`-Methoden synchron sind. Fügen Sie stattdessen die Protokollmeldungen synchron zu einer Warteschlange im Arbeitsspeicher hinzu, und rufen Sie mithilfe eines Hintergrundworkers die Nachrichten aus der Warteschlange ab, um sie dann asynchron per Pushvorgang an SQL Server zu übertragen. Weitere Informationen finden Sie in [diesem GitHub-Issue](https://github.com/dotnet/AspNetCore.Docs/issues/11801).

## <a name="configuration"></a>Konfiguration

Die Konfiguration von Protokollierungsanbietern erfolgt durch mindestens einen Konfigurationsanbieter:

* Dateiformate (INI, JSON und XML)
* Befehlszeilenargumenten
* Umgebungsvariablen.
* Speicherinterne .NET-Objekte
* Der unverschlüsselte Speicher von [Secret Manager](xref:security/app-secrets).
* Ein unverschlüsselter Benutzerspeicher, z.B. [Azure Key Vault](xref:security/key-vault-configuration).
* Benutzerdefinierte Anbieter (installiert oder erstellt).

Die Konfiguration der Protokollierung wird meist vom `Logging`-Abschnitt von Anwendungseinstellungsdateien angegeben. Im folgenden Beispiel werden die Inhalte einer herkömmlichen *appsettings.Development.json*-Datei veranschaulicht:

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Debug",
      "System": "Information",
      "Microsoft": "Information"
    },
    "Console":
    {
      "IncludeScopes": true
    }
  }
}
```

Die `Logging`-Eigenschaft kann den `LogLevel` und Protokollanbietereigenschaften beinhalten (Konsole wird angezeigt).

Die `LogLevel`-Eigenschaft unter `Logging` gibt den Mindest[grad](#log-level) an, der für ausgewählte Kategorien protokolliert werden soll. Im Beispiel protokollieren die Kategorien `System` und `Microsoft` mit dem Grad `Information` und alle anderen Kategorien mit dem Grad `Debug`.

Weitere Eigenschaften unter `Logging` geben Protokollierungsanbieter an. Das Beispiel bezieht sich auf den Konsolenanbieter. Wenn ein Anbieter [Protokollbereiche](#log-scopes) unterstützt, gibt `IncludeScopes` an, ob sie aktiviert sind. Eine Anbietereigenschaft (z.B. `Console` im Beispiel) kann auch eine `LogLevel`-Eigenschaft angeben. `LogLevel` unter einem Anbieter gibt die Grade an, die für diesen Anbieter protokolliert werden sollen.

Wenn Grade in `Logging.{providername}.LogLevel` angegeben werden, überschreiben sie alle Angaben in `Logging.LogLevel`. Betrachten Sie beispielsweise den folgenden JSON-Code:

[!code-json[](index/samples/3.x/TodoApiDTO/appsettings.MSFT.json)]

Im JSON-Code oben wird der vorherige Protokolliergrad (Standardprotokolliergrad) durch die `Console`-Anbietereinstellungen überschrieben.

Die Protokollierungs-API umfasst kein Szenario zum Ändern der Protokollebene, während eine App ausgeführt wird. Einige Konfigurationsanbieter können jedoch die Konfiguration erneut laden, was sich unmittelbar auf die Protokollierungskonfiguration auswirkt. Beispielsweise lädt der [Dateikonfigurationsanbieter](xref:fundamentals/configuration/index#file-configuration-provider), der durch `CreateDefaultBuilder` zum Lesen von Einstellungsdateien hinzugefügt wird, die Protokollierungskonfiguration standardmäßig erneut. Wenn die Konfiguration im Code geändert wird, während eine App ausgeführt wird, kann die App [IConfigurationRoot.Reload](xref:Microsoft.Extensions.Configuration.IConfigurationRoot.Reload*) aufrufen, um ihre Protokollierungskonfiguration zu aktualisieren.

Informationen zur Implementierung von Konfigurationsanbieter finden Sie hier: <xref:fundamentals/configuration/index>.

## <a name="sample-logging-output"></a>Beispiel einer Protokollierungsausgabe

Mit dem im vorherigen Abschnitt gezeigten Beispielcode werden Protokolle in der Konsole angezeigt, wenn die Anwendung über die Befehlszeile ausgeführt wird. Hier ein Beispiel für eine Konsolenausgabe:

```console
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:5000/api/todo/0
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
info: TodoApi.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 42.9286ms
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[2]
      Request finished in 148.889ms 404
```

Die vorherigen Protokolle wurden generiert, indem eine HTTP Get-Anforderung an die Beispiel-App unter `http://localhost:5000/api/todo/0` vorgenommen wurde.

Nachfolgend sehen Sie, wie die gleichen Protokolle im Debugfenster angezeigt werden, wenn Sie die Beispiel-App in Visual Studio ausführen:

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404
```

Die über die `ILogger`-Aufrufe aus dem vorherigen Abschnitt erstellten Protokolle beginnen mit „TodoApi“. Protokolle, die mit Microsoft-Kategorien beginnen, stammen aus ASP.NET Core-Frameworkcode. ASP.NET Core und Anwendungscode verwenden dieselbe Protokollierungs-API und dieselben Protokollierungsanbieter.

In den übrigen Abschnitten dieses Artikels werden einige Details und Optionen für die Protokollierung erläutert.

## <a name="nuget-packages"></a>NuGet-Pakete

Die Schnittstellen `ILogger` und `ILoggerFactory` befinden sich in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), ihre Standardimplementierungen sind in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/) enthalten.

## <a name="log-category"></a>Protokollkategorie

Wenn ein `ILogger`-Objekt erstellt wird, wird eine *Kategorie* für dieses Objekt angegeben. Diese Kategorie ist in jeder Protokollmeldung enthalten, die von dieser Instanz von `ILogger` erstellt wird. Die Kategorie kann eine beliebige Zeichenfolge sein, aber die Konvention besteht in der Verwendung des Klassennamens, z.B. „TodoApi.Controllers.TodoController“.

Verwenden Sie `ILogger<T>` zum Abrufen einer `ILogger`-Instanz, die den vollqualifizierten Typnamen von `T` als Kategorie verwendet:

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

Um die Kategorie explizit anzugeben, rufen Sie `ILoggerFactory.CreateLogger` auf:

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

`ILogger<T>` entspricht dem Aufruf von `CreateLogger` mit dem vollqualifizierten Typnamen `T`.

## <a name="log-level"></a>Protokolliergrad

Jedes Protokoll gibt einen <xref:Microsoft.Extensions.Logging.LogLevel>-Wert an. Der Protokolliergrad gibt den Schweregrad oder die Wichtigkeit an. Sie können beispielsweise ein `Information`-Protokoll schreiben, wenn eine Methode normal endet, und ein `Warning`-Protokoll, wenn eine Methode den Statuscode *404 Not Found* zurückgibt.

Der folgende Code erstellt `Information`- und `Warning`-Protokolle:

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

Im Code oben sind die Parameter `MyLogEvents.GetItem` und `MyLogEvents.GetItemNotFound` die [Protokollereignis-ID](#log-event-id). Der zweite Parameter ist eine Meldungsvorlage mit Platzhaltern für Argumentwerte, die von den verbleibenden Methodenparametern bereitgestellt werden. Die Methodenparameter werden im [Abschnitt „Protokollmeldungsvorlage“](#lmt) in diesem Artikel erläutert.

Protokollmethoden, die den Protokolliergrad im Methodennamen enthalten (z.B. `LogInformation` und `LogWarning`), sind [Erweiterungsmethoden für ILogger](xref:Microsoft.Extensions.Logging.LoggerExtensions). Diese Methoden rufen eine `Log`-Methode auf, die einen `LogLevel`-Parameter annimmt. Sie können anstelle eines Aufrufs dieser Erweiterungsmethoden die `Log`-Methode direkt aufrufen, aber die Syntax ist relativ kompliziert. Weitere Informationen finden Sie unter <xref:Microsoft.Extensions.Logging.ILogger> und im [Quellcode für Protokollierungserweiterungen](https://github.com/dotnet/extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs).

ASP.NET Core definiert die folgenden Protokolliergrade. Die Reihenfolge reicht vom geringsten bis zum höchsten Schweregrad.

* Trace = 0

  Protokolliert Informationen, die in der Regel nur für das Debuggen nützlich sind. Diese Meldungen können sensible Anwendungsdaten enthalten und sollten daher nicht in einer Produktionsumgebung aktiviert werden. *Standardmäßig deaktiviert.*

* Debug = 1

  Protokolliert Informationen, die bei der Entwicklung und beim Debuggen hilfreich sein können. Beispiel: `Entering method Configure with flag set to true.` Aktivieren Sie Protokolle mit dem Protokolliergrad `Debug` aufgrund des hohen Protokollaufkommens nur zur Problembehandlung in der Produktion.

* Information = 2

  Zur Nachverfolgung des allgemeinen Ablaufs der App. Diese Protokolle haben typischerweise einen langfristigen Nutzen. Ein Beispiel: `Request received for path /api/todo`

* Warning = 3

  Für ungewöhnliche oder unerwartete Ereignisse im App-Verlauf. Dazu können Fehler oder andere Bedingungen gehören, die zwar nicht zum Beenden der App führen, aber eventuell untersucht werden müssen. Behandelte Ausnahmen sind eine typische Verwendung für den Protokolliergrad `Warning`. Ein Beispiel: `FileNotFoundException for file quotes.txt.`

* Error = 4

  Für Fehler und Ausnahmen, die nicht behandelt werden können. Diese Meldungen weisen auf einen Fehler in der aktuellen Aktivität oder im aktuellen Vorgang (z.B. in der aktuellen HTTP-Anforderung) und nicht auf einen anwendungsweiten Fehler hin. Beispielprotokollmeldung: `Cannot insert record due to duplicate key violation.`

* Critical = 5

  Für Fehler, die sofortige Aufmerksamkeit erfordern. Beispiel: Szenarios mit Datenverlust, Speichermangel.

Verwenden Sie den Protokolliergrad, um die Menge an Protokollausgabedaten zu steuern, die in ein bestimmtes Speichermedium geschrieben oder an ein Anzeigefenster ausgegeben werden. Zum Beispiel:

* In einer Produktionsumgebung
  * Das Protokollieren von `Trace` über die `Information`-Ebenen verursacht viele detaillierte Protokollmeldungen. Protokollieren Sie `Trace` über Nachrichten auf `Information`-Ebene in einem kostengünstigen Speicher mit hohem Volumen, um die Kosten zu überwachen und die Grenzwerte des Datenspeichers nicht zu überschreiten.
  * Das Protokollieren von `Warning` über die `Critical`-Ebenen verursacht weniger und kürzere Protokollmeldungen. Daher müssen Sie sich über Kosten- und Speichergrenzwerte keine Sorgen machen und sind bei der Auswahl des Datenspeichers flexibler.
* Entwicklungsphase:
  * Protokollieren Sie `Warning` über `Critical`-Meldungen an der Konsole.
  * Fügen Sie bei der Fehlerbehebung `Trace` über `Information`-Meldungen hinzu.

Im Abschnitt [Protokollfilterung](#log-filtering) weiter unten in diesem Artikel wird erläutert, wie Sie steuern, welche Protokolliergrade ein Anbieter verarbeitet.

ASP.NET Core schreibt Protokolle für Frameworkereignisse. Die zuvor in diesem Artikel gezeigten Protokollbeispiele enthielten Protokolle unterhalb des Protokolliergrads `Information`, deshalb wurden keine Protokolle des Protokolliergrads `Debug` oder `Trace` erstellt. Hier ist ein Beispiel für Konsolenprotokolle, die durch Ausführen der Beispiel-App generiert werden, die so konfiguriert ist, dass sie `Debug`-Protokolle anzeigt:

```console
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:62555/api/todo/0
dbug: Microsoft.AspNetCore.Routing.Tree.TreeRouter[1]
      Request successfully matched the route with name 'GetTodo' and template 'api/Todo/{id}'.
dbug: Microsoft.AspNetCore.Mvc.Internal.ActionSelector[2]
      Action 'TodoApi.Controllers.TodoController.Update (TodoApi)' with id '089d59b6-92ec-472d-b552-cc613dfd625d' did not match the constraint 'Microsoft.AspNetCore.Mvc.Internal.HttpMethodActionConstraint'
dbug: Microsoft.AspNetCore.Mvc.Internal.ActionSelector[2]
      Action 'TodoApi.Controllers.TodoController.Delete (TodoApi)' with id 'f3476abe-4bd9-4ad3-9261-3ead09607366' did not match the constraint 'Microsoft.AspNetCore.Mvc.Internal.HttpMethodActionConstraint'
dbug: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action TodoApi.Controllers.TodoController.GetById (TodoApi)
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
info: TodoApi.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
dbug: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action method TodoApi.Controllers.TodoController.GetById (TodoApi), returned result Microsoft.AspNetCore.Mvc.NotFoundResult.
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 0.8788ms
dbug: Microsoft.AspNetCore.Server.Kestrel[9]
      Connection id "0HL6L7NEFF2QD" completed keep alive response.
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[2]
      Request finished in 2.7286ms 404
```

## <a name="log-event-id"></a>Protokollereignis-ID

Jedes Protokoll kann eine *Ereignis-ID* angeben. Die Beispiel-App verwendet hierzu eine lokal definierte `LoggingEvents`-Klasse:

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

Eine Ereignis-ID ordnet eine Gruppe von Ereignissen zu. Beispielsweise können alle Protokolle, die sich auf die Anzeige einer Liste von Elementen auf einer Seite beziehen, 1001 sein.

Der Protokollierungsanbieter kann die Ereignis-ID in einem ID-Feld, in der Protokollierungsmeldung oder gar nicht speichern. Der Debuganbieter zeigt keine Ereignis-IDs an. Der Konsolenanbieter zeigt Ereignis-IDs in Klammern hinter der Kategorie an:

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a>Protokollmeldungsvorlage

Jedes Protokoll gibt eine Meldungsvorlage an. Die Meldungsvorlage kann Platzhalter enthalten, für die Argumente bereitgestellt werden. Verwenden Sie Namen für die Platzhalter, keine Zahlen.

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

Die Reihenfolge der Platzhalter – nicht ihre Namen – bestimmt, welche Parameter verwendet werden, um ihre Werte zur Verfügung zu stellen. Beachten Sie im folgenden Code, dass die Parameternamen in der Meldungsvorlage nicht in der richtigen Reihenfolge vorhanden sind:

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

Dieser Code erstellt eine Protokollierungsmeldung mit den Parameterwerten in der richtigen Reihenfolge:

```text
Parameter values: parm1, parm2
```

Das Protokollierungsframework funktioniert auf diese Weise, damit Protokollierungsanbieter [semantische Protokollierung, die auch als strukturierte Protokollierung bezeichnet wird](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging), implementieren können. Die Argumente selbst (nicht nur die formatierte Meldungsvorlage) werden an das Protokollierungssystem übergeben. Diese Informationen ermöglichen es Protokollierungsanbietern, die Parameterwerte als Felder zu speichern. Nehmen wir zum Beispiel an, dass Aufrufe von Protokollierungsmethoden folgendermaßen aussehen:

```csharp
_logger.LogInformation("Getting item {Id} at {RequestTime}", id, DateTime.Now);
```

Wenn Sie die Protokolle an Azure Table Storage senden, kann jede Azure Table-Entität die Eigenschaften `ID` und `RequestTime` aufweisen. Dies vereinfacht die Abfrage von Protokolldaten. Eine Abfrage kann alle Protokolle in einem bestimmten `RequestTime`-Bereich finden, ohne die Uhrzeit aus den Textmeldungen zu analysieren.

## <a name="logging-exceptions"></a>Protokollieren von Ausnahmen

Die Protokollierungsmethoden umfassen Überladungen, die das Übergeben von Ausnahmen ermöglichen, wie im folgenden Beispiel gezeigt:

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

Die verschiedenen Anbieter verarbeiten die Ausnahmeinformationen auf unterschiedliche Weise. Hier sehen Sie ein Beispiel einer Debuganbieterausgabe aus dem obigen Codebeispiel.

```text
TodoApiSample.Controllers.TodoController: Warning: GetById(55) NOT FOUND

System.Exception: Item not found exception.
   at TodoApiSample.Controllers.TodoController.GetById(String id) in C:\TodoApiSample\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a>Protokollfilterung

Sie können einen Mindestprotokolliergrad für einen bestimmten Anbieter und eine bestimmte Kategorie oder für alle Anbieter oder alle Kategorien festlegen. Alle Protokolle unter dem Mindestgrad werden nicht an diesen Anbieter weitergeleitet, sodass sie nicht angezeigt oder gespeichert werden.

Wenn Sie alle Protokolle unterdrücken möchten, geben Sie `LogLevel.None` als Mindestprotokolliergrad an. Der ganzzahlige Wert von `LogLevel.None` lautet 6 und liegt über `LogLevel.Critical` (5).

### <a name="create-filter-rules-in-configuration"></a>Erstellen von Filterregeln in der Konfiguration

Der Projektvorlagencode ruft `CreateDefaultBuilder` auf, um die Protokollierung für die Konsolen-, Debug- und EventSource-Anbieter (ASP.NET Core 2.2 oder höher) einzurichten. Die `CreateDefaultBuilder`-Methode richtet die Protokollierung ein, um nach der Konfiguration in einem `Logging`-Abschnitt zu suchen; dies wird [weiter oben in diesem Artikel](#configuration) erläutert.

Die Konfigurationsdaten geben die Mindestprotokolliergrade nach Anbieter und Kategorie an, wie im folgenden Beispiel gezeigt:

[!code-json[](index/samples/2.x/TodoApiSample/appsettings.json)]

Diese JSON-Konfiguration erstellt sechs Filterregeln: eine für den Debuganbieter, vier für den Konsolenanbieter und eine für alle Anbieter. Eine einzelne Regel wird für jeden Anbieter ausgewählt, wenn ein `ILogger`-Objekt erstellt wird.

### <a name="filter-rules-in-code"></a>Filterregeln im Code

Das folgende Beispiel zeigt, wie Filterregeln im Code registriert werden:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

Das zweite `AddFilter` gibt den Debuganbieter durch Verwendung des zugehörigen Typnamens an. Das erste `AddFilter` gilt für alle Anbieter, weil kein Anbietertyp angegeben wird.

### <a name="how-filtering-rules-are-applied"></a>Anwendung von Filterregeln

Die Konfigurationsdaten und der in den vorangegangenen Beispielen gezeigte `AddFilter`-Code erzeugen die in der folgenden Tabelle gezeigten Regeln. Die ersten sechs Regeln stammen aus dem Konfigurationsbeispiel, die letzten zwei Filter stammen aus dem Codebeispiel.

| Anzahl | Anbieter      | Kategorien beginnend mit...          | Mindestprotokolliergrad |
| :----: | ------------- | --------------------------------------- | ----------------- |
| 1      | Debug         | Alle Kategorien                          | Information       |
| 2      | Konsole       | Microsoft.AspNetCore.Mvc.Razor.Internal | Warnung           |
| 3      | Konsole       | Microsoft.AspNetCore.Mvc.Razor.Razor    | Debug             |
| 4      | Konsole       | Microsoft.AspNetCore.Mvc.Razor          | Fehler             |
| 5      | Konsole       | Alle Kategorien                          | Information       |
| 6      | Alle Anbieter | Alle Kategorien                          | Debug             |
| 7      | Alle Anbieter | System                                  | Debug             |
| 8      | Debug         | Microsoft                               | Ablaufverfolgung             |

Wenn ein `ILogger`-Objekt erstellt wird, wählt das `ILoggerFactory`-Objekt eine einzige Regel pro Anbieter aus, die auf diese Protokollierung angewendet wird. Alle von einer `ILogger`-Instanz geschriebenen Meldungen werden auf Grundlage der ausgewählten Regeln gefiltert. Aus den verfügbaren Regeln wird die für jeden Anbieter und jedes Kategoriepaar spezifischste Regel ausgewählt.

Beim Erstellen von `ILogger` für eine vorgegebene Kategorie wird für jeden Anbieter der folgende Algorithmus verwendet:

* Es werden alle Regeln ausgewählt, die dem Anbieter oder dem zugehörigen Alias entsprechen. Wenn keine Übereinstimmung gefunden wird, werden alle Regeln mit einem leeren Anbieter ausgewählt.
* Aus dem Ergebnis des vorherigen Schritts werden die Regeln mit dem längsten übereinstimmenden Kategoriepräfix ausgewählt. Wenn keine Übereinstimmung gefunden wird, werden alle Regeln ohne Angabe einer Kategorie ausgewählt.
* Wenn mehrere Regeln ausgewählt sind, wird die **letzte** Regel verwendet.
* Wenn keine Regel ausgewählt ist, wird `MinimumLevel` verwendet.

Angenommen, Sie erstellen mit der oben aufgeführten Liste mit Regeln ein `ILogger`-Objekt für die Kategorie „Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine“:

* Für den Debuganbieter gelten die Regeln 1, 6 und 8. Regel 8 ist die spezifischste Regel, deshalb wird diese Regel ausgewählt.
* Für den Konsolenanbieter gelten die Regeln 3, 4, 5 und 6. Regel 3 ist die spezifischste Regel.

Die sich ergebende `ILogger`-Instanz sendet Protokolle mit dem Protokolliergrad `Trace` und höher an den Debuganbieter. Protokolle mit dem Protokolliergrad `Debug` und höher werden an den Konsolenanbieter gesendet.

### <a name="provider-aliases"></a>Anbieteraliase

Jeder Anbieter definiert einen *Alias*, der in der Konfiguration anstelle des vollqualifizierten Typnamens verwendet werden kann.  Verwenden Sie für die integrierten Anbieter die folgenden Aliase:

* Konsole
* Debug
* EventSource
* EventLog
* TraceSource
* AzureAppServicesFile
* AzureAppServicesBlob
* ApplicationInsights

### <a name="default-minimum-level"></a>Standardmindestprotokolliergrad

Es gibt eine Einstellung für den Mindestprotokolliergrad, die nur dann wirksam wird, wenn für einen bestimmten Anbieter und eine bestimmte Kategorie keine Regeln aus Konfiguration oder Code gelten. Im folgenden Beispiel wird das Festlegen des Mindestprotokolliergrads veranschaulicht:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_MinLevel&highlight=3)]

Wenn Sie den Mindestprotokolliergrad nicht explizit festlegen, lautet der Standardwert `Information`, d.h. Protokolle der Ebene `Trace` und `Debug` werden ignoriert.

### <a name="filter-functions"></a>Filterfunktionen

Eine Filterfunktion wird für alle Anbieter und Kategorien aufgerufen, denen keine Regeln durch Konfiguration oder Code zugewiesen sind. Code in der Funktion verfügt über Zugriff auf den Anbietertyp, die Kategorie und den Protokolliergrad. Zum Beispiel:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

## <a name="system-categories-and-levels"></a>Systemkategorien und -protokolliergrade

Dies sind einige Kategorien, die vom ASP.NET Core und Entity Framework Core verwendet werden, mit Hinweisen darauf, welche Protokolle von ihnen zu erwarten sind:

| Kategorie                            | Hinweise |
| ----------------------------------- | ----- |
| Microsoft.AspNetCore                | Allgemeine ASP.NET Core-Diagnose. |
| Microsoft.AspNetCore.DataProtection | Gibt an, welche Schlüssel in Betracht gezogen, gefunden und verwendet wurden. |
| Microsoft.AspNetCore.HostFiltering  | Hosts sind zulässig. |
| Microsoft.AspNetCore.Hosting        | Gibt an, wie lange es bis zum Abschluss von HTTP-Anforderungen gedauert hat und zu welcher Zeit sie gestartet wurden. Gibt an, welche Hostingstartassemblys geladen wurden. |
| Microsoft.AspNetCore.Mvc            | MVC und Razor-Diagnose Modellbindung, Filterausführung, Ansichtskompilierung, Aktionsauswahl. |
| Microsoft.AspNetCore.Routing        | Gibt Routenabgleichsinformationen an. |
| Microsoft.AspNetCore.Server         | Start-, Beendigungs- und Keep-Alive-Antworten der Verbindung. HTTPS-Zertifikatinformationen. |
| Microsoft.AspNetCore.StaticFiles    | Die bereitgestellten Dateien. |
| Microsoft.EntityFrameworkCore       | Allgemeine Entity Framework Core-Diagnose. Datenbankaktivität und -konfiguration, Änderungserkennung, Migrationen. |

## <a name="log-scopes"></a>Protokollbereiche

 Ein *Bereich* kann einen Satz logischer Vorgänge gruppieren. Diese Gruppierung kann verwendet werden, um an jedes Protokoll, das als Teil einer Gruppe erstellt wird, die gleichen Daten anzufügen. So kann beispielsweise jedes Protokoll, das im Rahmen der Verarbeitung einer Transaktion erstellt wird, die Transaktions-ID enthalten.

Ein Bereich ist ein `IDisposable`-Typ, der von der <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*>-Methode zurückgegeben und so lange beibehalten wird, bis er verworfen wird. Verwenden Sie einen Bereich, indem Sie Protokollierungsaufrufe mit einem `using`-Block umschließen, der als Wrapper verwendet wird:

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

Der folgende Code aktiviert Bereiche für den Konsolenanbieter:

*Program.cs*:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> Um die bereichsbasierte Protokollierung zu aktivieren, muss die Konsolenprotokollierungsoption `IncludeScopes` konfiguriert werden.
>
> Weitere Informationen zur Konfiguration finden Sie im Abschnitt [Konfiguration](#configuration).

Jede Protokollmeldung enthält die bereichsbezogenen Informationen:

```
info: TodoApiSample.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApiSample.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApiSample.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApiSample.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a>Integrierte Protokollierungsanbieter

ASP.NET Core wird mit den folgenden Anbietern bereitgestellt:

* [Konsole](#console-provider)
* [Debuggen](#debug-provider)
* [EventSource](#event-source-provider)
* [EventLog](#windows-eventlog-provider)
* [TraceSource](#tracesource-provider)
* [AzureAppServicesFile](#azure-app-service-provider)
* [AzureAppServicesBlob](#azure-app-service-provider)
* [ApplicationInsights](#azure-application-insights-trace-logging)

Weitere Informationen zu „stdout“ und zur Debugprotokollierung mit dem ASP.NET Core-Modul finden Sie unter <xref:test/troubleshoot-azure-iis> und <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.

### <a name="console-provider"></a>Der Konsolenanbieter

Das Anbieterpaket [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) sendet eine Protokollausgabe an die Konsole. 

```csharp
logging.AddConsole();
```

Um die Ausgabe der Konsolenprotokollierung anzuzeigen, öffnen Sie eine Eingabeaufforderung im Projektordner und führen den folgenden Befehl aus:

```dotnetcli
dotnet run
```

### <a name="debug-provider"></a>Der Debuganbieter

Beim Anbieterpaket [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) erfolgt die Protokollausgabe unter Verwendung der Klasse [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) (`Debug.WriteLine`-Methodenaufrufe).

Unter Linux werden Protokolle dieses Anbieters in */var/log/message* geschrieben.

```csharp
logging.AddDebug();
```

### <a name="event-source-provider"></a>Ereignisquellenanbieter

Das [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource)-Anbieterpaket schreibt in eine plattformübergreifende Ereignisquelle mit dem Namen `Microsoft-Extensions-Logging`. Unter Windows verwendet der Anbieter die [Ereignisablaufverfolgung für Windows (ETW)](https://msdn.microsoft.com/library/windows/desktop/bb968803).

```csharp
logging.AddEventSourceLogger();
```

Der Ereignisquellenanbieter wird automatisch hinzugefügt, wenn `CreateDefaultBuilder` zum Erstellen des Hosts aufgerufen wird.

Verwenden Sie das [PerfView-Hilfsprogramm](https://github.com/Microsoft/perfview) zum Sammeln und Anzeigen von Protokollen. Es gibt andere Tools zur Anzeige von ETW-Protokollen, aber PerfView bietet die besten Ergebnisse bei der Arbeit mit ETW-Ereignissen, die von ASP.NET Core ausgegeben werden.

Um PerfView für das Erfassen von Ereignissen zu konfigurieren, die von diesem Anbieter protokolliert wurden, fügen Sie die Zeichenfolge `*Microsoft-Extensions-Logging` zur Liste **Zusätzliche Anbieter** hinzu. (Vergessen Sie nicht das Sternchen am Anfang der Zeichenfolge.)

![Zusätzliche PerfView-Anbieter](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a>Der Windows-EventLog-Anbieter

Das Anbieterpaket [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) sendet eine Protokollausgabe in das Windows-Ereignisprotokoll.

```csharp
logging.AddEventLog();
```

Mithilfe von [AddEventLog-Überladungen](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions) können Sie <xref:Microsoft.Extensions.Logging.EventLog.EventLogSettings> übergeben. Wenn `null` oder nicht angegeben, werden die folgenden Standardeinstellungen verwendet:

* `LogName`: „Anwendung“
* `SourceName`: „.NET Runtime“
* `MachineName`: Der Name des lokalen Computers wird verwendet.

Ereignisse werden für die [Warnstufe und höher](#log-level) protokolliert. Im folgenden Beispiel wird der Standardprotokolliergrad auf <xref:Microsoft.Extensions.Logging.LogLevel.Information?displayProperty=nameWithType> festgelegt:

```json
"Logging": {
  "EventLog": {
    "LogLevel": {
      "Default": "Information"
    }
  }
}
```

### <a name="tracesource-provider"></a>Der TraceSource-Anbieter

Das Anbieterpaket [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) verwendet die <xref:System.Diagnostics.TraceSource>-Bibliotheken und -Anbieter.

```csharp
logging.AddTraceSource(sourceSwitchName);
```

Mithilfe von [AddTraceSource-Überladungen](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions) können Sie eine Quelloption und einen Listener für die Ablaufverfolgung übergeben.

Um diesen Anbieter zu verwenden, muss eine App unter .NET Framework (anstelle von .NET Core) ausgeführt werden. Der Anbieter kann Nachrichten an eine Vielzahl von [Listenern](/dotnet/framework/debug-trace-profile/trace-listeners) weiterleiten, z.B. an den in der Beispiel-App verwendeten <xref:System.Diagnostics.TextWriterTraceListener>.

### <a name="azure-app-service-provider"></a>Der Azure App Service-Anbieter

Das Anbieterpaket [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) schreibt Protokolle in Textdateien in das Dateisystem einer Azure App Service-App und in [Blob Storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in einem Azure Storage-Konto.

```csharp
logging.AddAzureWebAppDiagnostics();
```

Dieses Anbieterpaket ist nicht im [Microsoft.AspNetCore.App-Metapaket](xref:fundamentals/metapackage-app) enthalten. Wenn Sie Anwendungen für .NET Framework entwickeln oder auf das `Microsoft.AspNetCore.App`-Metapaket verweisen, fügen Sie das Anbieterpaket dem Projekt hinzu. 

Eine <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*>-Überladung ermöglicht das Übergeben von <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings>. Das settings-Objekt kann Standardeinstellungen überschreiben, z.B. die Protokollierungsausgabevorlage, den Blobnamen und die maximale Dateigröße. (Die *Ausgabevorlage* ist eine Nachrichtenvorlage, die auf alle Protokolle zusätzlich zu dem angewendet wird, was mit einem `ILogger`-Methodenaufruf bereitgestellt wird.)

Wenn Sie eine Bereitstellung in einer App Service-App durchführen, berücksichtigt die Anwendung die Einstellungen im Abschnitt [App Service logs](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag) (App Service-Protokolle) auf der Seite **App Services** im Azure-Portal. Bei einem Update der folgenden Einstellungen werden die Änderungen sofort wirksam, ohne dass ein Neustart oder eine erneute Bereitstellung der App notwendig ist.

* **Anwendungsprotokoll (Dateisystem)**
* **Anwendungsprotokoll (Blob)**

Der Standardspeicherort für Protokolldateien ist der Ordner *D:\\home\\LogFiles\\Application*, und der standardmäßige Dateiname lautet *diagnostics-yyyymmdd.txt*. Die Dateigröße ist standardmäßig auf 10 MB beschränkt, und die maximal zulässige Anzahl beibehaltener Dateien lautet 2. Der Standardblobname lautet *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.

Der Anbieter funktioniert nur, wenn das Projekt in der Azure-Umgebung ausgeführt wird. Bei einer lokalen Ausführung zeigt er keine Auswirkungen. Der Anbieter schreibt keine Protokolle in lokale Dateien oder den lokalen Entwicklungsspeicher für BLOBs.

#### <a name="azure-log-streaming"></a>Azure-Protokollstreaming

Azure-Protokollstreaming ermöglicht Ihnen eine Echtzeitanzeige der Protokollaktivität für:

* App-Server
* Webserver
* Ablaufverfolgung für Anforderungsfehler

So konfigurieren Sie das Azure-Protokollstreaming

* Navigieren Sie von der Portalseite Ihrer App zur Seite **App Service-Protokolle**.
* Legen Sie **Anwendungsprotokollierung (Dateisystem)** auf **Ein** fest.
* Wählen Sie die **Protokollierungsebene**. Diese Einstellung gilt nur für das Azure-Protokollstreaming, nicht für andere Protokollierungsanbieter in der App.

Navigieren Sie zur Seite **Log Stream** (Protokollstream), um App-Meldungen anzuzeigen. Diese werden von der App über die `ILogger`-Schnittstelle protokolliert.

### <a name="azure-application-insights-trace-logging"></a>Ablaufverfolgungsprotokollierung für Azure Application Insights

Das [Microsoft.Extensions.Logging.ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights)-Anbieterpaket schreibt Protokolle in Azure Application Insights. Application Insights ist ein Dienst, der eine Web-App überwacht und Tools für Abfragen und Analysen von Telemetriedaten zur Verfügung stellt. Wenn Sie diesen Anbieter verwenden, können Sie Ihre Protokolle mithilfe der Application Insights-Tools abfragen und analysieren.

Der Protokollierungsanbieter ist als Abhängigkeit von [Microsoft.ApplicationInsights.AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) enthalten, d.h. als das Paket, das alle verfügbaren Telemetriedaten für ASP.NET Core bereitstellt. Wenn Sie dieses Paket verwenden, ist nicht erforderlich, das Anbieterpaket zu installieren.

Verwenden Sie nicht das [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web)-Paket, das für ASP.NET 4.x bestimmt ist.

Weitere Informationen finden Sie in den folgenden Ressourcen:

* [Application Insights-Übersicht](/azure/application-insights/app-insights-overview)
* [Anwendungseinblicke für ASP.NET Core-Anwendungen](/azure/azure-monitor/app/asp-net-core): Beginnen Sie hier, wenn Sie die gesamte Bandbreite der Application Insights-Telemetrie zusammen mit der Protokollierung implementieren möchten.
* [ApplicationInsightsLoggerProvider für .NET Core ILogger-Protokolle](/azure/azure-monitor/app/ilogger): Beginnen Sie hier, wenn Sie den Protokollierungsanbieter ohne die übrige Application Insights-Telemetrie implementieren möchten.
* [Application Insights logging adapters (Adapter zur Protokollierung für Application Insights)](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-trace-logs).
* [Installieren, Konfigurieren und Initialisieren des Application Insights SDK](/learn/modules/instrument-web-app-code-with-application-insights): interaktive Tutorials auf der Microsoft Learn-Website.

## <a name="third-party-logging-providers"></a>Protokollierungsanbieter von Drittanbietern

Protokollierungsframeworks von Drittanbietern aufgeführt, die mit ASP.NET Core funktionieren:

* [elmah.io](https://elmah.io/) ([GitHub-Repository](https://github.com/elmahio/Elmah.Io.Extensions.Logging))
* [Gelf](https://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub-Repository](https://github.com/mattwcole/gelf-extensions-logging))
* [JSNLog](https://jsnlog.com/) ([GitHub-Repository](https://github.com/mperdeck/jsnlog))
* [KissLog.net](https://kisslog.net/) ([GitHub repo](https://github.com/catalingavan/KissLog-net))
* [Log4Net](https://logging.apache.org/log4net/) ([GitHub-Repository](https://github.com/huorswords/Microsoft.Extensions.Logging.Log4Net.AspNetCore))
* [Loggr](https://loggr.net/) ([GitHub-Repository](https://github.com/imobile3/Loggr.Extensions.Logging))
* [NLog](https://nlog-project.org/) ([GitHub-Repository](https://github.com/NLog/NLog.Extensions.Logging))
* [Sentry](https://sentry.io/welcome/) ([GitHub-Repository](https://github.com/getsentry/sentry-dotnet))
* [Serilog](https://serilog.net/) ([GitHub-Repository](https://github.com/serilog/serilog-aspnetcore))
* [Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([GitHub-Repository](https://github.com/googleapis/google-cloud-dotnet))

Einige Drittanbieterframeworks können eine [semantische Protokollierung (auch als strukturierte Protokollierung bezeichnet)](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging) ausführen.

Die Verwendung eines Frameworks von Drittanbietern ist ähnlich wie die Verwendung eines integrierten Anbieters:

1. Fügen Sie Ihrem Paket ein NuGet-Paket hinzu.
1. Rufen Sie eine `ILoggerFactory`-Erweiterungsmethode auf, die vom Protokollierungsframework bereitgestellt wird.

Weitere Informationen finden Sie in der Dokumentation zum jeweiligen Anbieter. Protokollierungsanbieter von Drittanbietern werden von Microsoft nicht unterstützt.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* <xref:fundamentals/logging/loggermessage>

::: moniker-end
