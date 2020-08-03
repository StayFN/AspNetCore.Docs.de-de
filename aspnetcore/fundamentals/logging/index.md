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
# <a name="logging-in-net-core-and-aspnet-core"></a><span data-ttu-id="2f940-103">Protokollieren in .NET Core und ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2f940-103">Logging in .NET Core and ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="2f940-104">Von [Kirk Larkin](https://twitter.com/serpent5), [Jürgen Gutsch](https://github.com/JuergenGutsch) und [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2f940-104">By [Kirk Larkin](https://twitter.com/serpent5), [Juergen Gutsch](https://github.com/JuergenGutsch) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="2f940-105">.NET Core unterstützt eine Protokollierungs-API, die mit einer Vielzahl von integrierten Protokollierungsanbietern und Drittanbieter-Protokollierungslösungen zusammenarbeitet.</span><span class="sxs-lookup"><span data-stu-id="2f940-105">.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="2f940-106">Dieser Artikel zeigt, wie Sie die Protokollierungs-API mit integrierten Anbietern verwenden können.</span><span class="sxs-lookup"><span data-stu-id="2f940-106">This article shows how to use the logging API with built-in providers.</span></span>

<span data-ttu-id="2f940-107">Die meisten der Codebeispiele in diesem Artikel stammen aus ASP.NET Core-Apps.</span><span class="sxs-lookup"><span data-stu-id="2f940-107">Most of the code examples shown in this article are from ASP.NET Core apps.</span></span> <span data-ttu-id="2f940-108">Die auf die Protokollierung bezogenen Teile dieser Codeausschnitte gelten für jede .NET Core-App, die den [generischen Host](xref:fundamentals/host/generic-host) verwendet.</span><span class="sxs-lookup"><span data-stu-id="2f940-108">The logging-specific parts of these code snippets apply to any .NET Core app that uses the [Generic Host](xref:fundamentals/host/generic-host).</span></span> <span data-ttu-id="2f940-109">Die ASP.NET Core Web-App-Vorlagen verwenden den generischen Host.</span><span class="sxs-lookup"><span data-stu-id="2f940-109">The ASP.NET Core web app templates use the Generic Host.</span></span>

<span data-ttu-id="2f940-110">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/index/samples/3.x) ([Vorgehensweise zum Herunterladen](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2f940-110">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/index/samples/3.x) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<a name="lp"></a>

## <a name="logging-providers"></a><span data-ttu-id="2f940-111">Protokollierungsanbieter</span><span class="sxs-lookup"><span data-stu-id="2f940-111">Logging providers</span></span>

<span data-ttu-id="2f940-112">Protokollierungsanbieter speichern Protokolle, mit Ausnahme des `Console`-Anbieters, der Protokolle anzeigt.</span><span class="sxs-lookup"><span data-stu-id="2f940-112">Logging providers store logs, except for the `Console` provider which displays logs.</span></span> <span data-ttu-id="2f940-113">Beispielsweise speichert der Azure Application Insights-Anbieter Protokolle in Azure Application Insights.</span><span class="sxs-lookup"><span data-stu-id="2f940-113">For example, the Azure Application Insights provider stores logs in Azure Application Insights.</span></span> <span data-ttu-id="2f940-114">Es können mehrere Anbieter aktiviert werden.</span><span class="sxs-lookup"><span data-stu-id="2f940-114">Multiple providers can be enabled.</span></span>

<span data-ttu-id="2f940-115">Für die ASP.NET Core-Web-App-Standardvorlagen gilt Folgendes:</span><span class="sxs-lookup"><span data-stu-id="2f940-115">The default ASP.NET Core web app templates:</span></span>

* <span data-ttu-id="2f940-116">Sie verwenden den [generischen Host](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="2f940-116">Use the [Generic Host](xref:fundamentals/host/generic-host).</span></span>
* <span data-ttu-id="2f940-117">Sie rufen <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder%2A> auf, wodurch die folgenden Protokollierungsanbieter hinzugefügt werden:</span><span class="sxs-lookup"><span data-stu-id="2f940-117">Call <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder%2A>, which adds the following logging providers:</span></span>
  * [<span data-ttu-id="2f940-118">Konsole</span><span class="sxs-lookup"><span data-stu-id="2f940-118">Console</span></span>](#console)
  * [<span data-ttu-id="2f940-119">Debuggen</span><span class="sxs-lookup"><span data-stu-id="2f940-119">Debug</span></span>](#debug)
  * [<span data-ttu-id="2f940-120">EventSource</span><span class="sxs-lookup"><span data-stu-id="2f940-120">EventSource</span></span>](#event-source)
  * <span data-ttu-id="2f940-121">[EventLog](#welog): Nur Windows</span><span class="sxs-lookup"><span data-stu-id="2f940-121">[EventLog](#welog): Windows only</span></span>

[!code-csharp[](index/samples/3.x/TodoApiDTO/Program.cs?name=snippet_TemplateCode&highlight=9)]

<span data-ttu-id="2f940-122">Der vorstehende Code zeigt die `Program`-Klasse, die mit den ASP.NET Core-Web-App-Vorlagen erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="2f940-122">The preceding code shows the `Program` class created with the ASP.NET Core web app templates.</span></span> <span data-ttu-id="2f940-123">In den nächsten Abschnitten finden Sie Beispiele, die auf den ASP.NET Core-Web-App-Vorlagen basieren, die den generischen Host verwenden.</span><span class="sxs-lookup"><span data-stu-id="2f940-123">The next several sections provide samples based on the ASP.NET Core web app templates, which use the Generic Host.</span></span> <span data-ttu-id="2f940-124">[Nicht-Hostkonsolen-Apps](#nhca) werden weiter unten in diesem Dokument erläutert.</span><span class="sxs-lookup"><span data-stu-id="2f940-124">[Non-host console apps](#nhca) are discussed later in this document.</span></span>

<span data-ttu-id="2f940-125">Um den von `Host.CreateDefaultBuilder` hinzugefügten Standardsatz von Protokollierungsanbietern zu überschreiben, rufen Sie `ClearProviders` auf und fügen die erforderlichen Protokollierungsanbieter hinzu.</span><span class="sxs-lookup"><span data-stu-id="2f940-125">To override the default set of logging providers added by `Host.CreateDefaultBuilder`, call `ClearProviders` and add the required logging providers.</span></span> <span data-ttu-id="2f940-126">Beispielsweise folgender Code:</span><span class="sxs-lookup"><span data-stu-id="2f940-126">For example, the following code:</span></span>

* <span data-ttu-id="2f940-127">Ruft <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A> auf, um alle <xref:Microsoft.Extensions.Logging.ILoggerProvider>-Instanzen aus dem Generator zu entfernen.</span><span class="sxs-lookup"><span data-stu-id="2f940-127">Calls <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A> to remove all the <xref:Microsoft.Extensions.Logging.ILoggerProvider> instances from the builder.</span></span>
* <span data-ttu-id="2f940-128">Fügt den [Konsolen](#console)protokollierungsanbieter hinzu.</span><span class="sxs-lookup"><span data-stu-id="2f940-128">Adds the [Console](#console) logging provider.</span></span>

[!code-csharp[](index/samples/3.x/TodoApiDTO/Program.cs?name=snippet_AddProvider&highlight=5-6)]

<span data-ttu-id="2f940-129">Weitere Anbieter finden Sie unter:</span><span class="sxs-lookup"><span data-stu-id="2f940-129">For additional providers, see:</span></span>

* [<span data-ttu-id="2f940-130">Integrierte Protokollierungsanbieter</span><span class="sxs-lookup"><span data-stu-id="2f940-130">Built-in logging providers</span></span>](#bilp)
* <span data-ttu-id="2f940-131">[Protokollierungsanbieter von Drittanbietern](#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="2f940-131">[Third-party logging providers](#third-party-logging-providers).</span></span>

## <a name="create-logs"></a><span data-ttu-id="2f940-132">Erstellen von Protokollen</span><span class="sxs-lookup"><span data-stu-id="2f940-132">Create logs</span></span>

<span data-ttu-id="2f940-133">Um Protokolle zu erstellen, verwenden Sie ein <xref:Microsoft.Extensions.Logging.ILogger%601>-Objekt aus der [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection) (DI).</span><span class="sxs-lookup"><span data-stu-id="2f940-133">To create logs, use an <xref:Microsoft.Extensions.Logging.ILogger%601> object from [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span>

<span data-ttu-id="2f940-134">Im Beispiel unten geschieht Folgendes:</span><span class="sxs-lookup"><span data-stu-id="2f940-134">The following example:</span></span>

* <span data-ttu-id="2f940-135">Es wird eine Protokollierung (`ILogger<AboutModel>`) erstellt, die eine Protokoll*kategorie* des vollqualifizierten Namens vom Typ `AboutModel` verwendet.</span><span class="sxs-lookup"><span data-stu-id="2f940-135">Creates a logger, `ILogger<AboutModel>`, which uses a log *category* of the fully qualified name of the type `AboutModel`.</span></span> <span data-ttu-id="2f940-136">Die Protokollkategorie ist eine Zeichenfolge, die jedem Protokoll zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="2f940-136">The log category is a string that is associated with each log.</span></span>
* <span data-ttu-id="2f940-137">Es wird <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*> aufgerufen, um Protokollierung mit dem Protokolliergrad `Information` auszuführen.</span><span class="sxs-lookup"><span data-stu-id="2f940-137">Calls <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*> to log at the `Information` level.</span></span> <span data-ttu-id="2f940-138">Der Protokollierungs*grad* gibt den Schweregrad des protokollierten Ereignisses an.</span><span class="sxs-lookup"><span data-stu-id="2f940-138">The Log *level* indicates the severity of the logged event.</span></span>

[!code-csharp[](index/samples/3.x/TodoApiDTO/Pages/About.cshtml.cs?name=snippet_CallLogMethods&highlight=5,14)]

<span data-ttu-id="2f940-139">[Grade](#log-level) und [Kategorien](#log-category) werden weiter unten in diesem Dokument ausführlicher erläutert.</span><span class="sxs-lookup"><span data-stu-id="2f940-139">[Levels](#log-level) and [categories](#log-category) are explained in more detail later in this document.</span></span>

<span data-ttu-id="2f940-140">Weitere Informationen zu Blazor finden Sie unter [Erstellen von Protokollen in Blazor und Blazor WebAssembly](#clib) in diesem Dokument.</span><span class="sxs-lookup"><span data-stu-id="2f940-140">For information on Blazor, see [Create logs in Blazor and Blazor WebAssembly](#clib) in this document.</span></span>

<span data-ttu-id="2f940-141">[Erstellen von Protokollen in „Main“ und „Startup“](#clms) zeigt, wie Protokolle in `Main` und `Startup` erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="2f940-141">[Create logs in Main and Startup](#clms) shows how to create logs in `Main` and `Startup`.</span></span>

## <a name="configure-logging"></a><span data-ttu-id="2f940-142">Konfigurieren der Protokollierung</span><span class="sxs-lookup"><span data-stu-id="2f940-142">Configure logging</span></span>

<span data-ttu-id="2f940-143">Die Konfiguration der Protokollierung wird meistens im Abschnitt `Logging` von *appsettings*.`{Environment}` *.json*-Dateien angegeben.</span><span class="sxs-lookup"><span data-stu-id="2f940-143">Logging configuration is commonly provided by the `Logging` section of *appsettings*.`{Environment}`*.json* files.</span></span> <span data-ttu-id="2f940-144">Die folgende Datei *appsettings.Development.json* wird von den ASP.NET Core-Web-App-Vorlagen generiert:</span><span class="sxs-lookup"><span data-stu-id="2f940-144">The following *appsettings.Development.json* file is generated by the ASP.NET Core web app templates:</span></span>

[!code-json[](index/samples/3.x/TodoApiDTO/appsettings.Development.json)]

<span data-ttu-id="2f940-145">Für den oben stehenden JSON-Code gilt:</span><span class="sxs-lookup"><span data-stu-id="2f940-145">In the preceding JSON:</span></span>

* <span data-ttu-id="2f940-146">Die Kategorien `"Default"`, `"Microsoft"` und `"Microsoft.Hosting.Lifetime"` werden angegeben.</span><span class="sxs-lookup"><span data-stu-id="2f940-146">The `"Default"`, `"Microsoft"`, and `"Microsoft.Hosting.Lifetime"` categories are specified.</span></span>
* <span data-ttu-id="2f940-147">Die Kategorie `"Microsoft"` gilt für alle Kategorien, die mit `"Microsoft"` beginnen.</span><span class="sxs-lookup"><span data-stu-id="2f940-147">The `"Microsoft"` category applies to all categories that start with `"Microsoft"`.</span></span> <span data-ttu-id="2f940-148">Diese Einstellung gilt z. B. für die Kategorie `"Microsoft.AspNetCore.Routing.EndpointMiddleware"`.</span><span class="sxs-lookup"><span data-stu-id="2f940-148">For example, this setting applies to the `"Microsoft.AspNetCore.Routing.EndpointMiddleware"` category.</span></span>
* <span data-ttu-id="2f940-149">Die Kategorie `"Microsoft"` protokolliert mit Protokolliergrad `Warning` oder höher.</span><span class="sxs-lookup"><span data-stu-id="2f940-149">The `"Microsoft"` category logs at log level `Warning` and higher.</span></span>
* <span data-ttu-id="2f940-150">Die Kategorie `"Microsoft.Hosting.Lifetime"` ist genauer als die Kategorie `"Microsoft"`, sodass die Kategorie `"Microsoft.Hosting.Lifetime"` mit dem Protokolliergrad „Information“ oder höher protokolliert.</span><span class="sxs-lookup"><span data-stu-id="2f940-150">The `"Microsoft.Hosting.Lifetime"` category is more specific than the `"Microsoft"` category, so the `"Microsoft.Hosting.Lifetime"` category logs at log level "Information" and higher.</span></span>
* <span data-ttu-id="2f940-151">Ein bestimmter Protokollanbieter wird nicht angegeben, sodass `LogLevel` für alle aktivierten Protokollanbieter mit Ausnahme von [Windows EventLog](#welog) gilt.</span><span class="sxs-lookup"><span data-stu-id="2f940-151">A specific log provider is not specified, so `LogLevel` applies to all the enabled logging providers except for the [Windows EventLog](#welog).</span></span>

<span data-ttu-id="2f940-152">Die `Logging`-Eigenschaft kann <xref:Microsoft.Extensions.Logging.LogLevel> und Protokollanbietereigenschaften beinhalten.</span><span class="sxs-lookup"><span data-stu-id="2f940-152">The `Logging` property can have <xref:Microsoft.Extensions.Logging.LogLevel> and log provider properties.</span></span> <span data-ttu-id="2f940-153">`LogLevel` gibt den Mindest[grad](#log-level) an, der für ausgewählte Kategorien protokolliert werden soll.</span><span class="sxs-lookup"><span data-stu-id="2f940-153">The `LogLevel` specifies the minimum [level](#log-level) to log for selected categories.</span></span> <span data-ttu-id="2f940-154">Im JSON-Code oben werden die Protokolliergrade `Information` und `Warning` angegeben.</span><span class="sxs-lookup"><span data-stu-id="2f940-154">In the preceding JSON, `Information` and `Warning` log levels are specified.</span></span> <span data-ttu-id="2f940-155">`LogLevel` gibt den Schweregrad des Protokolls an und liegt zwischen 0 und 6:</span><span class="sxs-lookup"><span data-stu-id="2f940-155">`LogLevel` indicates the severity of the log and ranges from 0 to 6:</span></span>

<span data-ttu-id="2f940-156">`Trace` = 0, `Debug` = 1, `Information` = 2, `Warning` = 3, `Error` = 4, `Critical` = 5 und `None` = 6.</span><span class="sxs-lookup"><span data-stu-id="2f940-156">`Trace` = 0, `Debug` = 1, `Information` = 2, `Warning` = 3, `Error` = 4, `Critical` = 5, and `None` = 6.</span></span>

<span data-ttu-id="2f940-157">Wenn `LogLevel` angegeben wird, wird Protokollierung für Meldungen mit dem angegebenen Protokolliergrad oder höher aktiviert.</span><span class="sxs-lookup"><span data-stu-id="2f940-157">When a `LogLevel` is specified, logging is enabled for messages at the specified level and higher.</span></span> <span data-ttu-id="2f940-158">Im JSON-Code oben wird die Kategorie `Default` für `Information` oder höher protokolliert.</span><span class="sxs-lookup"><span data-stu-id="2f940-158">In the preceding JSON, the `Default` category is logged for `Information` and higher.</span></span> <span data-ttu-id="2f940-159">Beispielsweise werden `Information`-, `Warning`-, `Error`- und `Critical`-Meldungen protokolliert.</span><span class="sxs-lookup"><span data-stu-id="2f940-159">For example, `Information`, `Warning`, `Error`, and `Critical` messages are logged.</span></span> <span data-ttu-id="2f940-160">Wenn kein `LogLevel` angegeben wird, wird Protokollierung standardmäßig mit dem Protokolliergrad `Information` verwendet.</span><span class="sxs-lookup"><span data-stu-id="2f940-160">If no `LogLevel` is specified, logging defaults to the `Information` level.</span></span> <span data-ttu-id="2f940-161">Weitere Informationen finden Sie unter [Protokolliergrade](#llvl).</span><span class="sxs-lookup"><span data-stu-id="2f940-161">For more information, see [Log levels](#llvl).</span></span>

<span data-ttu-id="2f940-162">Eine Anbietereigenschaft kann eine `LogLevel`-Eigenschaft angeben.</span><span class="sxs-lookup"><span data-stu-id="2f940-162">A provider property can specify a `LogLevel` property.</span></span> <span data-ttu-id="2f940-163">`LogLevel` unter einem Anbieter gibt die Protokolliergrade an, die für diesen Anbieter protokolliert werden sollen, und überschreibt die Nicht-Anbieterprotokolleinstellungen.</span><span class="sxs-lookup"><span data-stu-id="2f940-163">`LogLevel` under a provider specifies levels to log for that provider, and overrides the non-provider log settings.</span></span> <span data-ttu-id="2f940-164">Sehen Sie sich die nachfolgende Datei *appsettings.json* an:</span><span class="sxs-lookup"><span data-stu-id="2f940-164">Consider the following *appsettings.json* file:</span></span>

[!code-json[](index/samples/3.x/TodoApiDTO/appsettings.Prod2.json)]

<span data-ttu-id="2f940-165">Die Einstellungen in `Logging.{providername}.LogLevel` überschreiben die Einstellungen in `Logging.LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="2f940-165">Settings in `Logging.{providername}.LogLevel` override settings in `Logging.LogLevel`.</span></span> <span data-ttu-id="2f940-166">Im JSON-Code oben wird der Standardprotokolliergrad `Debug` des Anbieters auf `Information` festgelegt:</span><span class="sxs-lookup"><span data-stu-id="2f940-166">In the preceding JSON, the `Debug` provider's default log level is set to `Information`:</span></span>

`Logging:Debug:LogLevel:Default:Information`

<span data-ttu-id="2f940-167">Die Einstellung oben gibt den Protokolliergrad `Information` für jede `Logging:Debug:`-Kategorie mit Ausnahme von `Microsoft.Hosting` an.</span><span class="sxs-lookup"><span data-stu-id="2f940-167">The preceding setting specifies the `Information` log level for every `Logging:Debug:` category except `Microsoft.Hosting`.</span></span> <span data-ttu-id="2f940-168">Wenn eine bestimmte Kategorie aufgelistet wird, überschreibt die jeweilige Kategorie die Standardkategorie.</span><span class="sxs-lookup"><span data-stu-id="2f940-168">When a specific category is listed, the specific category overrides the default category.</span></span> <span data-ttu-id="2f940-169">Im JSON-Code oben überschreiben die `Logging:Debug:LogLevel`-Kategorien `"Microsoft.Hosting"` und `"Default"` die Einstellungen in `Logging:LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="2f940-169">In the preceding JSON, the `Logging:Debug:LogLevel` categories `"Microsoft.Hosting"` and `"Default"` override the settings in `Logging:LogLevel`</span></span>

<span data-ttu-id="2f940-170">Der Mindestprotokolliergrad kann für Folgendes angegeben werden:</span><span class="sxs-lookup"><span data-stu-id="2f940-170">The minimum log level can be specified for any of:</span></span>

* <span data-ttu-id="2f940-171">Bestimmte Anbieter:  Beispiel: `Logging:EventSource:LogLevel:Default:Information`</span><span class="sxs-lookup"><span data-stu-id="2f940-171">Specific providers:  For example, `Logging:EventSource:LogLevel:Default:Information`</span></span>
* <span data-ttu-id="2f940-172">Bestimmte Kategorien: Beispiel: `Logging:LogLevel:Microsoft:Warning`</span><span class="sxs-lookup"><span data-stu-id="2f940-172">Specific categories: For example, `Logging:LogLevel:Microsoft:Warning`</span></span>
* <span data-ttu-id="2f940-173">Alle Anbieter und alle Kategorien: `Logging:LogLevel:Default:Warning`</span><span class="sxs-lookup"><span data-stu-id="2f940-173">All providers and all categories: `Logging:LogLevel:Default:Warning`</span></span>

<span data-ttu-id="2f940-174">Alle Protokolle unterhalb des Mindestprotokolliergrads werden ***nicht***:</span><span class="sxs-lookup"><span data-stu-id="2f940-174">Any logs below the minimum level are ***not***:</span></span>

* <span data-ttu-id="2f940-175">An den Anbieter übergeben.</span><span class="sxs-lookup"><span data-stu-id="2f940-175">Passed to the provider.</span></span>
* <span data-ttu-id="2f940-176">Protokolliert oder angezeigt.</span><span class="sxs-lookup"><span data-stu-id="2f940-176">Logged or displayed.</span></span>

<span data-ttu-id="2f940-177">Um alle Protokolle zu unterdrücken, geben Sie [LogLevel.None](xref:Microsoft.Extensions.Logging.LogLevel) an.</span><span class="sxs-lookup"><span data-stu-id="2f940-177">To suppress all logs, specify [LogLevel.None](xref:Microsoft.Extensions.Logging.LogLevel).</span></span> <span data-ttu-id="2f940-178">`LogLevel.None` hat den Wert 6, der höher als `LogLevel.Critical` (5) ist.</span><span class="sxs-lookup"><span data-stu-id="2f940-178">`LogLevel.None` has a value of 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="2f940-179">Wenn ein Anbieter [Protokollbereiche](#logscopes) unterstützt, gibt `IncludeScopes` an, ob sie aktiviert sind.</span><span class="sxs-lookup"><span data-stu-id="2f940-179">If a provider supports [log scopes](#logscopes), `IncludeScopes` indicates whether they're enabled.</span></span> <span data-ttu-id="2f940-180">Weitere Informationen finden Sie unter [Protokollierbereiche](#logscopes).</span><span class="sxs-lookup"><span data-stu-id="2f940-180">For more information, see [log scopes](#logscopes)</span></span>

<span data-ttu-id="2f940-181">Die folgende Datei *appsettings.json* enthält alle Anbieter, die standardmäßig aktiviert sind:</span><span class="sxs-lookup"><span data-stu-id="2f940-181">The following *appsettings.json* file contains all the providers enabled by default:</span></span>

[!code-json[](index/samples/3.x/TodoApiDTO/appsettings.Production.json)]

<span data-ttu-id="2f940-182">Im vorgehenden Beispiel:</span><span class="sxs-lookup"><span data-stu-id="2f940-182">In the preceding sample:</span></span>

* <span data-ttu-id="2f940-183">Die Kategorien und Protokolliergrade sind keine vorgeschlagenen Werte.</span><span class="sxs-lookup"><span data-stu-id="2f940-183">The categories and levels are not suggested values.</span></span> <span data-ttu-id="2f940-184">Das Beispiel wird bereitgestellt, um alle Standardanbieter zu zeigen.</span><span class="sxs-lookup"><span data-stu-id="2f940-184">The sample is provided to show all the default providers.</span></span>
* <span data-ttu-id="2f940-185">Die Einstellungen in `Logging.{providername}.LogLevel` überschreiben die Einstellungen in `Logging.LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="2f940-185">Settings in `Logging.{providername}.LogLevel` override settings in `Logging.LogLevel`.</span></span> <span data-ttu-id="2f940-186">Beispielsweise überschreibt der Protokolliergrad in `Debug.LogLevel.Default` den Protokolliergrad in `LogLevel.Default`.</span><span class="sxs-lookup"><span data-stu-id="2f940-186">For example, the level in `Debug.LogLevel.Default` overrides the level in `LogLevel.Default`.</span></span>
* <span data-ttu-id="2f940-187">Jeder Standardanbieter*alias* wird verwendet.</span><span class="sxs-lookup"><span data-stu-id="2f940-187">Each default provider *alias* is used.</span></span> <span data-ttu-id="2f940-188">Jeder Anbieter definiert einen *Alias*, der in der Konfiguration anstelle des vollqualifizierten Typnamens verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="2f940-188">Each provider defines an *alias* that can be used in configuration in place of the fully qualified type name.</span></span> <span data-ttu-id="2f940-189">Die folgenden Anbieteraliase sind integriert:</span><span class="sxs-lookup"><span data-stu-id="2f940-189">The built-in providers aliases are:</span></span>
  * <span data-ttu-id="2f940-190">Konsole</span><span class="sxs-lookup"><span data-stu-id="2f940-190">Console</span></span>
  * <span data-ttu-id="2f940-191">Debug</span><span class="sxs-lookup"><span data-stu-id="2f940-191">Debug</span></span>
  * <span data-ttu-id="2f940-192">EventSource</span><span class="sxs-lookup"><span data-stu-id="2f940-192">EventSource</span></span>
  * <span data-ttu-id="2f940-193">EventLog</span><span class="sxs-lookup"><span data-stu-id="2f940-193">EventLog</span></span>
  * <span data-ttu-id="2f940-194">AzureAppServicesFile</span><span class="sxs-lookup"><span data-stu-id="2f940-194">AzureAppServicesFile</span></span>
  * <span data-ttu-id="2f940-195">AzureAppServicesBlob</span><span class="sxs-lookup"><span data-stu-id="2f940-195">AzureAppServicesBlob</span></span>
  * <span data-ttu-id="2f940-196">ApplicationInsights</span><span class="sxs-lookup"><span data-stu-id="2f940-196">ApplicationInsights</span></span>

## <a name="set-log-level-by-command-line-environment-variables-and-other-configuration"></a><span data-ttu-id="2f940-197">Festlegen des Protokolliergrads über die Befehlszeile, Umgebungsvariablen und andere Konfigurationen</span><span class="sxs-lookup"><span data-stu-id="2f940-197">Set log level by command line, environment variables, and other configuration</span></span>

<span data-ttu-id="2f940-198">Der Protokolliergrad kann von einem beliebigen [Konfigurationsanbieter](xref:fundamentals/configuration/index) festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="2f940-198">Log level can be set by any of the [configuration providers](xref:fundamentals/configuration/index).</span></span> 

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="2f940-199">Die folgenden Befehle:</span><span class="sxs-lookup"><span data-stu-id="2f940-199">The following commands:</span></span>

* <span data-ttu-id="2f940-200">Legen den Umgebungsschlüssel `Logging:LogLevel:Microsoft` auf den Wert `Information` unter Windows fest.</span><span class="sxs-lookup"><span data-stu-id="2f940-200">Set the environment key `Logging:LogLevel:Microsoft` to a value of `Information` on Windows.</span></span>
* <span data-ttu-id="2f940-201">Testen die Einstellungen, wenn eine App verwendet wird, die mit den ASP.NET Core-Webanwendungsvorlagen erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="2f940-201">Test the settings when using an app created with the ASP.NET Core web application templates.</span></span> <span data-ttu-id="2f940-202">Der `dotnet run`-Befehl muss im Projektverzeichnis nach der Verwendung von `set` ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="2f940-202">The `dotnet run` command must be run in the project directory after using `set`.</span></span>

```cmd
set Logging__LogLevel__Microsoft=Information
dotnet run
```

<span data-ttu-id="2f940-203">Die oben gezeigte Umgebungseinstellungen:</span><span class="sxs-lookup"><span data-stu-id="2f940-203">The preceding environment setting:</span></span>

* <span data-ttu-id="2f940-204">Werden nur in Prozessen festgelegt, die über das Befehlsfenster gestartet werden, in dem sie festgelegt wurden.</span><span class="sxs-lookup"><span data-stu-id="2f940-204">Is only set in processes launched from the command window they were set in.</span></span>
* <span data-ttu-id="2f940-205">Werden nicht von Browsern gelesen, die mit Visual Studio gestartet wurden.</span><span class="sxs-lookup"><span data-stu-id="2f940-205">Isn't read by browsers launched with Visual Studio.</span></span>

<span data-ttu-id="2f940-206">Mit dem folgenden [setx](/windows-server/administration/windows-commands/setx)-Befehl werden auch der Umgebungsschlüssel und der Wert unter Windows festgelegt.</span><span class="sxs-lookup"><span data-stu-id="2f940-206">The following [setx](/windows-server/administration/windows-commands/setx) command also sets the environment key and value on Windows.</span></span> <span data-ttu-id="2f940-207">Anders als `set` werden `setx`-Einstellungen beibehalten.</span><span class="sxs-lookup"><span data-stu-id="2f940-207">Unlike `set`, `setx` settings are persisted.</span></span> <span data-ttu-id="2f940-208">Der Schalter `/M` legt die Variable in der Systemumgebung fest.</span><span class="sxs-lookup"><span data-stu-id="2f940-208">The `/M` switch sets the variable in the system environment.</span></span> <span data-ttu-id="2f940-209">Wenn `/M` nicht verwendet wird, wird eine Benutzerumgebungsvariable festgelegt.</span><span class="sxs-lookup"><span data-stu-id="2f940-209">If `/M` isn't used, a user environment variable is set.</span></span>

```cmd
setx Logging__LogLevel__Microsoft=Information /M
```

<span data-ttu-id="2f940-210">Wählen Sie in [Azure App Service](https://azure.microsoft.com/services/app-service/) auf der Seite **Einstellungen > Konfiguration** die Option **Neue Anwendungseinstellung** aus.</span><span class="sxs-lookup"><span data-stu-id="2f940-210">On [Azure App Service](https://azure.microsoft.com/services/app-service/), select **New application setting** on the **Settings > Configuration** page.</span></span> <span data-ttu-id="2f940-211">Anwendungseinstellungen von Azure App Service werden:</span><span class="sxs-lookup"><span data-stu-id="2f940-211">Azure App Service application settings are:</span></span>

* <span data-ttu-id="2f940-212">Im Ruhezustand verschlüsselt und über einen verschlüsselten Kanal übermittelt.</span><span class="sxs-lookup"><span data-stu-id="2f940-212">Encrypted at rest and transmitted over an encrypted channel.</span></span>
* <span data-ttu-id="2f940-213">Als Umgebungsvariablen verfügbar gemacht.</span><span class="sxs-lookup"><span data-stu-id="2f940-213">Exposed as environment variables.</span></span>

<span data-ttu-id="2f940-214">Weitere Informationen finden Sie unter [Azure-Apps: Überschreiben der App-Konfiguration im Azure-Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="2f940-214">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

<span data-ttu-id="2f940-215">Weitere Informationen zum Festlegen von ASP.NET Core-Konfigurationswerten mithilfe von Umgebungsvariablen finden Sie unter [Umgebungsvariablen](xref:fundamentals/configuration/index#environment-variables).</span><span class="sxs-lookup"><span data-stu-id="2f940-215">For more information on setting ASP.NET Core configuration values using environment variables, see [environment variables](xref:fundamentals/configuration/index#environment-variables).</span></span> <span data-ttu-id="2f940-216">Informationen zur Verwendung anderer Konfigurationsquellen, einschließlich der Befehlszeile, Azure Key Vault, Azure App Configuration, anderer Dateiformate und mehr finden Sie unter <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="2f940-216">For information on using other configuration sources, including the command line, Azure Key Vault, Azure App Configuration, other file formats, and more, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="how-filtering-rules-are-applied"></a><span data-ttu-id="2f940-217">Anwendung von Filterregeln</span><span class="sxs-lookup"><span data-stu-id="2f940-217">How filtering rules are applied</span></span>

<span data-ttu-id="2f940-218">Wenn ein <xref:Microsoft.Extensions.Logging.ILogger%601>-Objekt erstellt wird, wählt das <xref:Microsoft.Extensions.Logging.ILoggerFactory>-Objekt eine einzige Regel pro Anbieter aus, die auf diese Protokollierung angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="2f940-218">When an <xref:Microsoft.Extensions.Logging.ILogger%601> object is created, the <xref:Microsoft.Extensions.Logging.ILoggerFactory> object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="2f940-219">Alle von einer `ILogger`-Instanz geschriebenen Meldungen werden auf Grundlage der ausgewählten Regeln gefiltert.</span><span class="sxs-lookup"><span data-stu-id="2f940-219">All messages written by an `ILogger` instance are filtered based on the selected rules.</span></span> <span data-ttu-id="2f940-220">Aus den verfügbaren Regeln wird die für jeden Anbieter und jedes Kategoriepaar spezifischste Regel ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="2f940-220">The most specific rule for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="2f940-221">Beim Erstellen von `ILogger` für eine vorgegebene Kategorie wird für jeden Anbieter der folgende Algorithmus verwendet:</span><span class="sxs-lookup"><span data-stu-id="2f940-221">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="2f940-222">Es werden alle Regeln ausgewählt, die dem Anbieter oder dem zugehörigen Alias entsprechen.</span><span class="sxs-lookup"><span data-stu-id="2f940-222">Select all rules that match the provider or its alias.</span></span> <span data-ttu-id="2f940-223">Wenn keine Übereinstimmung gefunden wird, werden alle Regeln mit einem leeren Anbieter ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="2f940-223">If no match is found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="2f940-224">Aus dem Ergebnis des vorherigen Schritts werden die Regeln mit dem längsten übereinstimmenden Kategoriepräfix ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="2f940-224">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="2f940-225">Wenn keine Übereinstimmung gefunden wird, werden alle Regeln ohne Angabe einer Kategorie ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="2f940-225">If no match is found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="2f940-226">Wenn mehrere Regeln ausgewählt sind, wird die **letzte** Regel verwendet.</span><span class="sxs-lookup"><span data-stu-id="2f940-226">If multiple rules are selected, take the **last** one.</span></span>
* <span data-ttu-id="2f940-227">Wenn keine Regel ausgewählt ist, wird `MinimumLevel` verwendet.</span><span class="sxs-lookup"><span data-stu-id="2f940-227">If no rules are selected, use `MinimumLevel`.</span></span>

<a name="dnrvs"></a>

## <a name="logging-output-from-dotnet-run-and-visual-studio"></a><span data-ttu-id="2f940-228">Protokollieren der Ausgabe von dotnet run und Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2f940-228">Logging output from dotnet run and Visual Studio</span></span>

<span data-ttu-id="2f940-229">Protokolle, die mit den [Standardprotokollierungsanbietern](#lp) erstellt werden, werden angezeigt:</span><span class="sxs-lookup"><span data-stu-id="2f940-229">Logs created with the [default logging providers](#lp) are displayed:</span></span>

* <span data-ttu-id="2f940-230">In Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2f940-230">In Visual Studio</span></span>
  * <span data-ttu-id="2f940-231">Im Ausgabefenster „Debuggen“ beim Debuggen.</span><span class="sxs-lookup"><span data-stu-id="2f940-231">In the Debug output window when debugging.</span></span>
  * <span data-ttu-id="2f940-232">Im ASP.NET Core Web Server-Fenster.</span><span class="sxs-lookup"><span data-stu-id="2f940-232">In the ASP.NET Core Web Server window.</span></span>
* <span data-ttu-id="2f940-233">Im Konsolenfenster, wenn die App mit `dotnet run` ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="2f940-233">In the console window when the app is run with `dotnet run`.</span></span>

<span data-ttu-id="2f940-234">Protokolle, die mit „Microsoft“-Kategorien beginnen, stammen aus ASP.NET Core-Frameworkcode.</span><span class="sxs-lookup"><span data-stu-id="2f940-234">Logs that begin with "Microsoft" categories are from ASP.NET Core framework code.</span></span> <span data-ttu-id="2f940-235">ASP.NET Core und Anwendungscode verwenden dieselbe Protokollierungs-API und dieselben Protokollierungsanbieter.</span><span class="sxs-lookup"><span data-stu-id="2f940-235">ASP.NET Core and application code use the same logging API and providers.</span></span>

<a name="lcat"></a>

## <a name="log-category"></a><span data-ttu-id="2f940-236">Protokollkategorie</span><span class="sxs-lookup"><span data-stu-id="2f940-236">Log category</span></span>

<span data-ttu-id="2f940-237">Wenn ein `ILogger`-Objekt erstellt wird, wird eine *Kategorie* angegeben.</span><span class="sxs-lookup"><span data-stu-id="2f940-237">When an `ILogger` object is created, a *category* is specified.</span></span> <span data-ttu-id="2f940-238">Diese Kategorie ist in jeder Protokollmeldung enthalten, die von dieser Instanz von `ILogger` erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="2f940-238">That category is included with each log message created by that instance of `ILogger`.</span></span> <span data-ttu-id="2f940-239">Die Kategoriezeichenfolge ist willkürlich, aber die Konvention besteht darin, den Klassennamen zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="2f940-239">The category string is arbitrary, but the convention is to use the class name.</span></span> <span data-ttu-id="2f940-240">Beispielsweise kann in einem Controller der Name `"TodoApi.Controllers.TodoController"` lauten.</span><span class="sxs-lookup"><span data-stu-id="2f940-240">For example, in a controller the name might be `"TodoApi.Controllers.TodoController"`.</span></span> <span data-ttu-id="2f940-241">Die ASP.NET Core-Web-Apps verwenden `ILogger<T>` zum automatischen Abrufen einer `ILogger`-Instanz, die den vollqualifizierten Typnamen von `T` als Kategorie verwendet:</span><span class="sxs-lookup"><span data-stu-id="2f940-241">The ASP.NET Core web apps use `ILogger<T>` to automatically get an `ILogger` instance that uses the fully qualified type name of `T` as the category:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiDTO/Pages/Privacy.cshtml.cs?name=snippet)]

<span data-ttu-id="2f940-242">Um die Kategorie explizit anzugeben, rufen Sie `ILoggerFactory.CreateLogger` auf:</span><span class="sxs-lookup"><span data-stu-id="2f940-242">To explicitly specify the category, call `ILoggerFactory.CreateLogger`:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiDTO/Pages/Contact.cshtml.cs?name=snippet)]

<span data-ttu-id="2f940-243">`ILogger<T>` entspricht dem Aufruf von `CreateLogger` mit dem vollqualifizierten Typnamen `T`.</span><span class="sxs-lookup"><span data-stu-id="2f940-243">`ILogger<T>` is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

<a name="llvl"></a>

## <a name="log-level"></a><span data-ttu-id="2f940-244">Protokolliergrad</span><span class="sxs-lookup"><span data-stu-id="2f940-244">Log level</span></span>

<span data-ttu-id="2f940-245">In der folgenden Tabelle werden die <xref:Microsoft.Extensions.Logging.LogLevel>-Werte, die Erweiterungsmethode `Log{LogLevel}` und die empfohlene Syntax aufgeführt:</span><span class="sxs-lookup"><span data-stu-id="2f940-245">The following table lists the <xref:Microsoft.Extensions.Logging.LogLevel> values, the convenience `Log{LogLevel}` extension method, and the suggested usage:</span></span>

| <span data-ttu-id="2f940-246">LogLevel</span><span class="sxs-lookup"><span data-stu-id="2f940-246">LogLevel</span></span> | <span data-ttu-id="2f940-247">Wert</span><span class="sxs-lookup"><span data-stu-id="2f940-247">Value</span></span> | <span data-ttu-id="2f940-248">Methode</span><span class="sxs-lookup"><span data-stu-id="2f940-248">Method</span></span> | <span data-ttu-id="2f940-249">BESCHREIBUNG</span><span class="sxs-lookup"><span data-stu-id="2f940-249">Description</span></span> |
| -------- | ----- | ------ | ----------- |
| [<span data-ttu-id="2f940-250">Ablaufverfolgung</span><span class="sxs-lookup"><span data-stu-id="2f940-250">Trace</span></span>](xref:Microsoft.Extensions.Logging.LogLevel) | <span data-ttu-id="2f940-251">0</span><span class="sxs-lookup"><span data-stu-id="2f940-251">0</span></span> | <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogTrace%2A> | <span data-ttu-id="2f940-252">Enthält die ausführlichsten Meldungen.</span><span class="sxs-lookup"><span data-stu-id="2f940-252">Contain the most detailed messages.</span></span> <span data-ttu-id="2f940-253">Diese Meldungen enthalten möglicherweise sensible App-Daten.</span><span class="sxs-lookup"><span data-stu-id="2f940-253">These messages may contain sensitive app data.</span></span> <span data-ttu-id="2f940-254">Sie sind standardmäßig deaktiviert und sollten ***nicht*** in einer Produktionsumgebung aktiviert werden.</span><span class="sxs-lookup"><span data-stu-id="2f940-254">These messages are disabled by default and should ***not*** be enabled in production.</span></span> |
| [<span data-ttu-id="2f940-255">Debuggen</span><span class="sxs-lookup"><span data-stu-id="2f940-255">Debug</span></span>](xref:Microsoft.Extensions.Logging.LogLevel) | <span data-ttu-id="2f940-256">1</span><span class="sxs-lookup"><span data-stu-id="2f940-256">1</span></span> | <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogDebug%2A> | <span data-ttu-id="2f940-257">Zum Debuggen und für die Entwicklung.</span><span class="sxs-lookup"><span data-stu-id="2f940-257">For debugging and development.</span></span> <span data-ttu-id="2f940-258">Wegen des großen Volumens in der Produktion mit Vorsicht zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="2f940-258">Use with caution in production due to the high volume.</span></span> |
| [<span data-ttu-id="2f940-259">Information</span><span class="sxs-lookup"><span data-stu-id="2f940-259">Information</span></span>](xref:Microsoft.Extensions.Logging.LogLevel) | <span data-ttu-id="2f940-260">2</span><span class="sxs-lookup"><span data-stu-id="2f940-260">2</span></span> | <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation%2A> | <span data-ttu-id="2f940-261">Verfolgt den allgemeinen Ablauf der App nach.</span><span class="sxs-lookup"><span data-stu-id="2f940-261">Tracks the general flow of the app.</span></span> <span data-ttu-id="2f940-262">Kann über einen langfristigen Wert verfügen.</span><span class="sxs-lookup"><span data-stu-id="2f940-262">May have long-term value.</span></span> |
| [<span data-ttu-id="2f940-263">Warnung</span><span class="sxs-lookup"><span data-stu-id="2f940-263">Warning</span></span>](xref:Microsoft.Extensions.Logging.LogLevel) | <span data-ttu-id="2f940-264">3</span><span class="sxs-lookup"><span data-stu-id="2f940-264">3</span></span> | <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogWarning%2A> | <span data-ttu-id="2f940-265">Für ungewöhnliche oder unerwartete Ereignisse.</span><span class="sxs-lookup"><span data-stu-id="2f940-265">For abnormal or unexpected events.</span></span> <span data-ttu-id="2f940-266">Schließt in der Regel Fehler oder Bedingungen ein, die nicht bewirken, dass die App fehlschlägt.</span><span class="sxs-lookup"><span data-stu-id="2f940-266">Typically includes errors or conditions that don't cause the app to fail.</span></span> |
| [<span data-ttu-id="2f940-267">Fehler</span><span class="sxs-lookup"><span data-stu-id="2f940-267">Error</span></span>](xref:Microsoft.Extensions.Logging.LogLevel) | <span data-ttu-id="2f940-268">4</span><span class="sxs-lookup"><span data-stu-id="2f940-268">4</span></span> | <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogError%2A> | <span data-ttu-id="2f940-269">Für Fehler und Ausnahmen, die nicht behandelt werden können.</span><span class="sxs-lookup"><span data-stu-id="2f940-269">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="2f940-270">Diese Meldungen weisen auf einen Fehler im aktuellen Vorgang oder der aktuellen Anforderung und nicht auf einen anwendungsweiten Fehler hin.</span><span class="sxs-lookup"><span data-stu-id="2f940-270">These messages indicate a failure in the current operation or request, not an app-wide failure.</span></span> |
| <span data-ttu-id="2f940-271">[Critical](xref:Microsoft.Extensions.Logging.LogLevel) (Kritisch)</span><span class="sxs-lookup"><span data-stu-id="2f940-271">[Critical](xref:Microsoft.Extensions.Logging.LogLevel)</span></span> | <span data-ttu-id="2f940-272">5</span><span class="sxs-lookup"><span data-stu-id="2f940-272">5</span></span> | <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogCritical%2A> | <span data-ttu-id="2f940-273">Für Fehler, die sofortige Aufmerksamkeit erfordern.</span><span class="sxs-lookup"><span data-stu-id="2f940-273">For failures that require immediate attention.</span></span> <span data-ttu-id="2f940-274">Beispiel: Szenarios mit Datenverlust, Speichermangel.</span><span class="sxs-lookup"><span data-stu-id="2f940-274">Examples: data loss scenarios, out of disk space.</span></span> |
| [<span data-ttu-id="2f940-275">Keine</span><span class="sxs-lookup"><span data-stu-id="2f940-275">None</span></span>](xref:Microsoft.Extensions.Logging.LogLevel) | <span data-ttu-id="2f940-276">6</span><span class="sxs-lookup"><span data-stu-id="2f940-276">6</span></span> | | <span data-ttu-id="2f940-277">Gibt an, dass eine Protokollierungskategorie keine Meldungen schreiben soll.</span><span class="sxs-lookup"><span data-stu-id="2f940-277">Specifies that a logging category should not write any messages.</span></span> |

<span data-ttu-id="2f940-278">In der Tabelle oben wird der `LogLevel` vom niedrigsten bis zum höchsten Schweregrad aufgelistet.</span><span class="sxs-lookup"><span data-stu-id="2f940-278">In the previous table, the `LogLevel` is listed from lowest to highest severity.</span></span>

<span data-ttu-id="2f940-279">Der erste Parameter der [Log](xref:Microsoft.Extensions.Logging.LoggerExtensions)-Methode (<xref:Microsoft.Extensions.Logging.LogLevel>) gibt den Schweregrad des Protokolls an.</span><span class="sxs-lookup"><span data-stu-id="2f940-279">The [Log](xref:Microsoft.Extensions.Logging.LoggerExtensions) method's first parameter, <xref:Microsoft.Extensions.Logging.LogLevel>, indicates the severity of the log.</span></span> <span data-ttu-id="2f940-280">Anstatt `Log(LogLevel, ...)` aufzurufen, rufen die meisten Entwickler die [Log{LogLevel}](xref:Microsoft.Extensions.Logging.LoggerExtensions)-Erweiterungsmethoden auf.</span><span class="sxs-lookup"><span data-stu-id="2f940-280">Rather than calling `Log(LogLevel, ...)`, most developers call the [Log{LogLevel}](xref:Microsoft.Extensions.Logging.LoggerExtensions) extension methods.</span></span> <span data-ttu-id="2f940-281">Die `Log{LogLevel}`-Erweiterungsmethoden [rufen die Log-Methode auf und geben den LogLevel an](https://github.com/dotnet/extensions/blob/release/3.1/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="2f940-281">The `Log{LogLevel}` extension methods [call the Log method and specify the LogLevel](https://github.com/dotnet/extensions/blob/release/3.1/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs).</span></span> <span data-ttu-id="2f940-282">Beispielsweise sind die folgenden beiden Protokollierungsaufrufe funktionell gleichwertig und generieren das gleiche Protokoll:</span><span class="sxs-lookup"><span data-stu-id="2f940-282">For example, the following two logging calls are functionally equivalent and produce the same log:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiDTO/Controllers/TestController.cs?name=snippet0&highlight=6-7)]

<span data-ttu-id="2f940-283">`MyLogEvents.TestItem` ist die Ereignis-ID.</span><span class="sxs-lookup"><span data-stu-id="2f940-283">`MyLogEvents.TestItem` is the event ID.</span></span> <span data-ttu-id="2f940-284">`MyLogEvents` ist Teil der Beispiel-App und wird im Abschnitt [Log Event ID](#leid) (Protokollereignis-ID) angezeigt.</span><span class="sxs-lookup"><span data-stu-id="2f940-284">`MyLogEvents` is part of the sample app and is displayed in the [Log event ID](#leid) section.</span></span>

[!INCLUDE[](~/includes/MyDisplayRouteInfoBoth.md)]

<span data-ttu-id="2f940-285">Der folgende Code erstellt `Information`- und `Warning`-Protokolle:</span><span class="sxs-lookup"><span data-stu-id="2f940-285">The following code creates `Information` and `Warning` logs:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiDTO/Controllers/TodoItemsController.cs?name=snippet_CallLogMethods&highlight=4,10)]

<span data-ttu-id="2f940-286">Im Code oben ist der erste `Log{LogLevel}`-Parameter (`MyLogEvents.GetItem`) die [Protokollereignis-ID](#leid).</span><span class="sxs-lookup"><span data-stu-id="2f940-286">In the preceding code, the first `Log{LogLevel}` parameter,`MyLogEvents.GetItem`, is the [Log event ID](#leid).</span></span> <span data-ttu-id="2f940-287">Der zweite Parameter ist eine Meldungsvorlage mit Platzhaltern für Argumentwerte, die von den verbleibenden Methodenparametern bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="2f940-287">The second parameter is a message template with placeholders for argument values provided by the remaining method parameters.</span></span> <span data-ttu-id="2f940-288">Die Methodenparameter werden im [Abschnitt „Meldungsvorlage“](#lmt) weiter unten in diesem Dokument erläutert.</span><span class="sxs-lookup"><span data-stu-id="2f940-288">The method parameters are explained in the [message template](#lmt) section later in this document.</span></span>

<span data-ttu-id="2f940-289">Rufen Sie die entsprechende `Log{LogLevel}`-Methode auf, um die Menge an Protokollausgabedaten zu steuern, die in ein bestimmtes Speichermedium geschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="2f940-289">Call the appropriate `Log{LogLevel}` method to control how much log output is written to a particular storage medium.</span></span> <span data-ttu-id="2f940-290">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="2f940-290">For example:</span></span>

* <span data-ttu-id="2f940-291">In einer Produktionsumgebung</span><span class="sxs-lookup"><span data-stu-id="2f940-291">In production:</span></span>
  * <span data-ttu-id="2f940-292">Das Protokollieren mit den Protokolliergraden `Trace` oder `Information` verursacht viele detaillierte Protokollmeldungen.</span><span class="sxs-lookup"><span data-stu-id="2f940-292">Logging at the `Trace` or `Information` levels produces a high-volume of detailed log messages.</span></span> <span data-ttu-id="2f940-293">Um die Kosten zu kontrollieren und die Datenspeichergrenzen nicht zu überschreiten, protokollieren Sie Meldungen der Protokolliergrade `Trace` und `Information` in einem kostengünstigen Datenspeicher mit hohem Datenvolumen.</span><span class="sxs-lookup"><span data-stu-id="2f940-293">To control costs and not exceed data storage limits, log `Trace` and `Information` level messages to a high-volume, low-cost data store.</span></span> <span data-ttu-id="2f940-294">Erwägen Sie eine Beschränkung von `Trace` und `Information` auf bestimmte Kategorien.</span><span class="sxs-lookup"><span data-stu-id="2f940-294">Consider limiting `Trace` and `Information` to specific categories.</span></span>
  * <span data-ttu-id="2f940-295">Bei der Protokollierung mit den Protokolliergraden `Warning` bis `Critical` sollten nur wenige Protokollmeldungen generiert werden.</span><span class="sxs-lookup"><span data-stu-id="2f940-295">Logging at `Warning` through `Critical` levels should produce few log messages.</span></span>
    * <span data-ttu-id="2f940-296">Kosten und Speichergrenzwerte stellen in der Regel kein Problem dar.</span><span class="sxs-lookup"><span data-stu-id="2f940-296">Costs and storage limits usually aren't a concern.</span></span>
    * <span data-ttu-id="2f940-297">Wenige Protokolle ermöglichen eine größere Flexibilität bei der Auswahl von Datenspeichern.</span><span class="sxs-lookup"><span data-stu-id="2f940-297">Few logs allow more flexibility in data store choices.</span></span>
* <span data-ttu-id="2f940-298">Bei der Entwicklung:</span><span class="sxs-lookup"><span data-stu-id="2f940-298">In development:</span></span>
  * <span data-ttu-id="2f940-299">Legen Sie diese Option auf `Warning` fest.</span><span class="sxs-lookup"><span data-stu-id="2f940-299">Set to `Warning`.</span></span>
  * <span data-ttu-id="2f940-300">Fügen Sie bei der Problembehandlung `Trace`- oder `Information`-Meldungen hinzu.</span><span class="sxs-lookup"><span data-stu-id="2f940-300">Add `Trace` or `Information` messages when troubleshooting.</span></span> <span data-ttu-id="2f940-301">Um die Ausgabe einzuschränken, legen Sie `Trace` oder `Information` nur für die zu untersuchenden Kategorien fest.</span><span class="sxs-lookup"><span data-stu-id="2f940-301">To limit output, set `Trace` or `Information` only for the categories under investigation.</span></span>

<span data-ttu-id="2f940-302">ASP.NET Core schreibt Protokolle für Frameworkereignisse.</span><span class="sxs-lookup"><span data-stu-id="2f940-302">ASP.NET Core writes logs for framework events.</span></span> <span data-ttu-id="2f940-303">Berücksichtigen Sie z. B. die Protokollausgabe für Folgendes:</span><span class="sxs-lookup"><span data-stu-id="2f940-303">For example, consider the log output for:</span></span>

* <span data-ttu-id="2f940-304">Eine Razor Pages-App, die mit den ASP.NET Core-Vorlagen erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="2f940-304">A Razor Pages app created with the ASP.NET Core templates.</span></span>
* <span data-ttu-id="2f940-305">Protokollierung festgelegt auf `Logging:Console:LogLevel:Microsoft:Information`</span><span class="sxs-lookup"><span data-stu-id="2f940-305">Logging set to `Logging:Console:LogLevel:Microsoft:Information`</span></span>
* <span data-ttu-id="2f940-306">Navigation Sie zur Seite „Datenschutz“:</span><span class="sxs-lookup"><span data-stu-id="2f940-306">Navigation to the Privacy page:</span></span>

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

<span data-ttu-id="2f940-307">Der folgende JSON-Code legt `Logging:Console:LogLevel:Microsoft:Information` fest:</span><span class="sxs-lookup"><span data-stu-id="2f940-307">The following JSON sets `Logging:Console:LogLevel:Microsoft:Information`:</span></span>

[!code-json[](index/samples/3.x/TodoApiDTO/appsettings.MSFT.json)]

<a name="leid"></a>

## <a name="log-event-id"></a><span data-ttu-id="2f940-308">Protokollereignis-ID</span><span class="sxs-lookup"><span data-stu-id="2f940-308">Log event ID</span></span>

<span data-ttu-id="2f940-309">Jedes Protokoll kann eine *Ereignis-ID* angeben.</span><span class="sxs-lookup"><span data-stu-id="2f940-309">Each log can specify an *event ID*.</span></span> <span data-ttu-id="2f940-310">Die Beispiel-App verwendet die `MyLogEvents`-Klasse zum Definieren von Ereignis-IDs:</span><span class="sxs-lookup"><span data-stu-id="2f940-310">The sample app uses the `MyLogEvents` class to define event IDs:</span></span>

<!-- Review: to bad there is no way to use an enum for event ID's -->
[!code-csharp[](index/samples/3.x/TodoApiDTO/Models/MyLogEvents.cs?name=snippet_LoggingEvents)]

[!code-csharp[](index/samples/3.x/TodoApiDTO/Controllers/TodoItemsController.cs?name=snippet_CallLogMethods&highlight=4,10)]

<span data-ttu-id="2f940-311">Eine Ereignis-ID ordnet eine Gruppe von Ereignissen zu.</span><span class="sxs-lookup"><span data-stu-id="2f940-311">An event ID associates a set of events.</span></span> <span data-ttu-id="2f940-312">Beispielsweise können alle Protokolle, die sich auf die Anzeige einer Liste von Elementen auf einer Seite beziehen, 1001 sein.</span><span class="sxs-lookup"><span data-stu-id="2f940-312">For example, all logs related to displaying a list of items on a page might be 1001.</span></span>

<span data-ttu-id="2f940-313">Der Protokollierungsanbieter kann die Ereignis-ID in einem ID-Feld, in der Protokollierungsmeldung oder gar nicht speichern.</span><span class="sxs-lookup"><span data-stu-id="2f940-313">The logging provider may store the event ID in an ID field, in the logging message, or not at all.</span></span> <span data-ttu-id="2f940-314">Der Debuganbieter zeigt keine Ereignis-IDs an.</span><span class="sxs-lookup"><span data-stu-id="2f940-314">The Debug provider doesn't show event IDs.</span></span> <span data-ttu-id="2f940-315">Der Konsolenanbieter zeigt Ereignis-IDs in Klammern hinter der Kategorie an:</span><span class="sxs-lookup"><span data-stu-id="2f940-315">The console provider shows event IDs in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoItemsController[1002]
      Getting item 1
warn: TodoApi.Controllers.TodoItemsController[4000]
      Get(1) NOT FOUND
```

<span data-ttu-id="2f940-316">Einige Protokollierungsanbieter speichern die Ereignis-ID in einem Feld, das das Filtern nach der ID ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="2f940-316">Some logging providers store the event ID in a field, which allows for filtering on the ID.</span></span>

<a name="lmt"></a>

## <a name="log-message-template"></a><span data-ttu-id="2f940-317">Protokollmeldungsvorlage</span><span class="sxs-lookup"><span data-stu-id="2f940-317">Log message template</span></span>
<!-- Review, Each log API uses a message template. -->
<span data-ttu-id="2f940-318">Jede Protokoll-API verwendet eine Meldungsvorlage.</span><span class="sxs-lookup"><span data-stu-id="2f940-318">Each log API uses a message template.</span></span> <span data-ttu-id="2f940-319">Die Meldungsvorlage kann Platzhalter enthalten, für die Argumente bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="2f940-319">The message template can contain placeholders for which arguments are provided.</span></span> <span data-ttu-id="2f940-320">Verwenden Sie Namen für die Platzhalter, keine Zahlen.</span><span class="sxs-lookup"><span data-stu-id="2f940-320">Use names for the placeholders, not numbers.</span></span>

[!code-csharp[](index/samples/3.x/TodoApiDTO/Controllers/TodoItemsController.cs?name=snippet_CallLogMethods&highlight=4,10)]

<span data-ttu-id="2f940-321">Die Reihenfolge der Platzhalter – nicht ihre Namen – bestimmt, welche Parameter verwendet werden, um ihre Werte zur Verfügung zu stellen.</span><span class="sxs-lookup"><span data-stu-id="2f940-321">The order of placeholders, not their names, determines which parameters are used to provide their values.</span></span> <span data-ttu-id="2f940-322">Im folgenden Code befinden sich die Parameternamen in der Meldungsvorlage nicht in der richtigen Reihenfolge:</span><span class="sxs-lookup"><span data-stu-id="2f940-322">In the following code, the parameter names are out of sequence in the message template:</span></span>

```csharp
string p1 = "param1";
string p2 = "param2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="2f940-323">Dieser Code oben erstellt eine Protokollierungsmeldung mit den Parameterwerten in der richtigen Reihenfolge:</span><span class="sxs-lookup"><span data-stu-id="2f940-323">The preceding code creates a log message with the parameter values in sequence:</span></span>

```text
Parameter values: param1, param2
```

<span data-ttu-id="2f940-324">Dieser Ansatz ermöglicht es Protokollierungsanbietern, [semantische oder strukturierte Protokollierung](https://github.com/NLog/NLog/wiki/How-to-use-structured-logging) zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="2f940-324">This approach allows logging providers to implement [semantic or structured logging](https://github.com/NLog/NLog/wiki/How-to-use-structured-logging).</span></span> <span data-ttu-id="2f940-325">Die Argumente selbst (nicht nur die formatierte Meldungsvorlage) werden an das Protokollierungssystem übergeben.</span><span class="sxs-lookup"><span data-stu-id="2f940-325">The arguments themselves are passed to the logging system, not just the formatted message template.</span></span> <span data-ttu-id="2f940-326">Dies ermöglicht es Protokollierungsanbietern, die Parameterwerte als Felder zu speichern.</span><span class="sxs-lookup"><span data-stu-id="2f940-326">This enables logging providers to store the parameter values as fields.</span></span> <span data-ttu-id="2f940-327">Sehen Sie sich z. B. die folgende Protokollierungsmethode an:</span><span class="sxs-lookup"><span data-stu-id="2f940-327">For example, consider the following logger method:</span></span>

```csharp
_logger.LogInformation("Getting item {Id} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="2f940-328">Beispielsweise bei der Protokollierung in Azure Table Storage:</span><span class="sxs-lookup"><span data-stu-id="2f940-328">For example, when logging to Azure Table Storage:</span></span>

* <span data-ttu-id="2f940-329">Jede Azure Table-Entität kann über `ID`- und `RequestTime`-Eigenschaften verfügen.</span><span class="sxs-lookup"><span data-stu-id="2f940-329">Each Azure Table entity can have `ID` and `RequestTime` properties.</span></span>
* <span data-ttu-id="2f940-330">Tabellen mit Eigenschaften vereinfachen Abfragen für protokollierte Daten.</span><span class="sxs-lookup"><span data-stu-id="2f940-330">Tables with properties simplify queries on logged data.</span></span> <span data-ttu-id="2f940-331">Beispielsweise kann eine Abfrage alle Protokolle innerhalb eines bestimmten `RequestTime`-Bereichs ermitteln, ohne die Zeitangabe aus der Textnachricht analysieren zu müssen.</span><span class="sxs-lookup"><span data-stu-id="2f940-331">For example, a query can find all logs within a particular `RequestTime` range without having to parse the time out of the text message.</span></span>

## <a name="log-exceptions"></a><span data-ttu-id="2f940-332">Protokollieren von Ausnahmen</span><span class="sxs-lookup"><span data-stu-id="2f940-332">Log exceptions</span></span>

<span data-ttu-id="2f940-333">Die Protokollierungsmethoden verfügen über Überladungen, die einen Ausnahmeparameter annehmen:</span><span class="sxs-lookup"><span data-stu-id="2f940-333">The logger methods have overloads that take an exception parameter:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiDTO/Controllers/TestController.cs?name=snippet_Exp)]

[!INCLUDE[](~/includes/MyDisplayRouteInfoBoth.md)]

<span data-ttu-id="2f940-334">Ausnahmeprotokollierung ist anbieterspezifisch.</span><span class="sxs-lookup"><span data-stu-id="2f940-334">Exception logging is provider-specific.</span></span>

### <a name="default-log-level"></a><span data-ttu-id="2f940-335">Standardprotokolliergrad</span><span class="sxs-lookup"><span data-stu-id="2f940-335">Default log level</span></span>

<span data-ttu-id="2f940-336">Wenn der Standardprotokolliergrad nicht festgelegt ist, ist der Standardwert des Standardprotokolliergrads `Information`.</span><span class="sxs-lookup"><span data-stu-id="2f940-336">If the default log level is not set, the default log level value is `Information`.</span></span>

<span data-ttu-id="2f940-337">Betrachten Sie z. B. die folgende Web-App:</span><span class="sxs-lookup"><span data-stu-id="2f940-337">For example, consider the following web app:</span></span>

* <span data-ttu-id="2f940-338">Sie wurde mit den ASP.NET-Web-App-Vorlagen erstellt.</span><span class="sxs-lookup"><span data-stu-id="2f940-338">Created with the ASP.NET web app templates.</span></span>
* <span data-ttu-id="2f940-339">*appsettings.json* und *appsettings.Development.json* wurden gelöscht oder umbenannt.</span><span class="sxs-lookup"><span data-stu-id="2f940-339">*appsettings.json* and *appsettings.Development.json* deleted or renamed.</span></span>

<span data-ttu-id="2f940-340">Mit dem oben beschriebenen Setup generiert die Navigation zur Datenschutz- oder Homepage viele `Trace`-, `Debug`- und `Information`-Meldungen mit `Microsoft` im Kategorienamen.</span><span class="sxs-lookup"><span data-stu-id="2f940-340">With the preceding setup, navigating to the privacy or home page produces many `Trace`, `Debug`, and `Information`  messages with `Microsoft` in the category name.</span></span>

<span data-ttu-id="2f940-341">Mit dem folgenden Code wird der Standardprotokolliergrad festgelegt, wenn der Standardprotokolliergrad nicht in der Konfiguration festgelegt ist:</span><span class="sxs-lookup"><span data-stu-id="2f940-341">The following code sets the default log level when the default log level is not set in configuration:</span></span>

[!code-csharp[](index/samples/3.x/MyMain/Program.cs?name=snippet_MinLevel&highlight=10)]

<!-- review required: I say this a couple times -->
<span data-ttu-id="2f940-342">Im Allgemeinen sollten Standardprotokolliergrads in der Konfiguration und nicht im Code angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="2f940-342">Generally, log levels should be specified in configuration and not code.</span></span>

### <a name="filter-function"></a><span data-ttu-id="2f940-343">Filterfunktion</span><span class="sxs-lookup"><span data-stu-id="2f940-343">Filter function</span></span>

<span data-ttu-id="2f940-344">Eine Filterfunktion wird für alle Anbieter und Kategorien aufgerufen, denen keine Regeln durch Konfiguration oder Code zugewiesen sind:</span><span class="sxs-lookup"><span data-stu-id="2f940-344">A filter function is invoked for all providers and categories that don't have rules assigned to them by configuration or code:</span></span>

[!code-csharp[](index/samples/3.x/MyMain/Program.cs?name=snippet_FilterFunction)]

<span data-ttu-id="2f940-345">Der Code oben zeigt Konsolenprotokolle an, wenn die Kategorie `Controller` oder `Microsoft` enthält und der Protokolliergrad `Information` oder höher ist.</span><span class="sxs-lookup"><span data-stu-id="2f940-345">The preceding code displays console logs when the category contains `Controller` or `Microsoft` and the log level is `Information` or higher.</span></span>

<span data-ttu-id="2f940-346">Im Allgemeinen sollten Standardprotokolliergrads in der Konfiguration und nicht im Code angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="2f940-346">Generally, log levels should be specified in configuration and not code.</span></span>

## <a name="aspnet-core-and-ef-core-categories"></a><span data-ttu-id="2f940-347">ASP.NET Core- und EF Core-Kategorien</span><span class="sxs-lookup"><span data-stu-id="2f940-347">ASP.NET Core and EF Core categories</span></span>

<span data-ttu-id="2f940-348">Die folgende Tabelle enthält einige Kategorien, die von ASP.NET Core und Entity Framework Core verwendet werden, mit Hinweisen zu den Protokollen:</span><span class="sxs-lookup"><span data-stu-id="2f940-348">The following table contains some categories used by ASP.NET Core and Entity Framework Core, with notes about the logs:</span></span>

| <span data-ttu-id="2f940-349">Kategorie</span><span class="sxs-lookup"><span data-stu-id="2f940-349">Category</span></span>                            | <span data-ttu-id="2f940-350">Hinweise</span><span class="sxs-lookup"><span data-stu-id="2f940-350">Notes</span></span> |
| ----------------------------------- | ----- |
| <span data-ttu-id="2f940-351">Microsoft.AspNetCore</span><span class="sxs-lookup"><span data-stu-id="2f940-351">Microsoft.AspNetCore</span></span>                | <span data-ttu-id="2f940-352">Allgemeine ASP.NET Core-Diagnose.</span><span class="sxs-lookup"><span data-stu-id="2f940-352">General ASP.NET Core diagnostics.</span></span> |
| <span data-ttu-id="2f940-353">Microsoft.AspNetCore.DataProtection</span><span class="sxs-lookup"><span data-stu-id="2f940-353">Microsoft.AspNetCore.DataProtection</span></span> | <span data-ttu-id="2f940-354">Gibt an, welche Schlüssel in Betracht gezogen, gefunden und verwendet wurden.</span><span class="sxs-lookup"><span data-stu-id="2f940-354">Which keys were considered, found, and used.</span></span> |
| <span data-ttu-id="2f940-355">Microsoft.AspNetCore.HostFiltering</span><span class="sxs-lookup"><span data-stu-id="2f940-355">Microsoft.AspNetCore.HostFiltering</span></span>  | <span data-ttu-id="2f940-356">Hosts sind zulässig.</span><span class="sxs-lookup"><span data-stu-id="2f940-356">Hosts allowed.</span></span> |
| <span data-ttu-id="2f940-357">Microsoft.AspNetCore.Hosting</span><span class="sxs-lookup"><span data-stu-id="2f940-357">Microsoft.AspNetCore.Hosting</span></span>        | <span data-ttu-id="2f940-358">Gibt an, wie lange es bis zum Abschluss von HTTP-Anforderungen gedauert hat und zu welcher Zeit sie gestartet wurden.</span><span class="sxs-lookup"><span data-stu-id="2f940-358">How long HTTP requests took to complete and what time they started.</span></span> <span data-ttu-id="2f940-359">Gibt an, welche Hostingstartassemblys geladen wurden.</span><span class="sxs-lookup"><span data-stu-id="2f940-359">Which hosting startup assemblies were loaded.</span></span> |
| <span data-ttu-id="2f940-360">Microsoft.AspNetCore.Mvc</span><span class="sxs-lookup"><span data-stu-id="2f940-360">Microsoft.AspNetCore.Mvc</span></span>            | <span data-ttu-id="2f940-361">MVC und Razor-Diagnose</span><span class="sxs-lookup"><span data-stu-id="2f940-361">MVC and Razor diagnostics.</span></span> <span data-ttu-id="2f940-362">Modellbindung, Filterausführung, Ansichtskompilierung, Aktionsauswahl.</span><span class="sxs-lookup"><span data-stu-id="2f940-362">Model binding, filter execution, view compilation, action selection.</span></span> |
| <span data-ttu-id="2f940-363">Microsoft.AspNetCore.Routing</span><span class="sxs-lookup"><span data-stu-id="2f940-363">Microsoft.AspNetCore.Routing</span></span>        | <span data-ttu-id="2f940-364">Gibt Routenabgleichsinformationen an.</span><span class="sxs-lookup"><span data-stu-id="2f940-364">Route matching information.</span></span> |
| <span data-ttu-id="2f940-365">Microsoft.AspNetCore.Server</span><span class="sxs-lookup"><span data-stu-id="2f940-365">Microsoft.AspNetCore.Server</span></span>         | <span data-ttu-id="2f940-366">Start-, Beendigungs- und Keep-Alive-Antworten der Verbindung.</span><span class="sxs-lookup"><span data-stu-id="2f940-366">Connection start, stop, and keep alive responses.</span></span> <span data-ttu-id="2f940-367">HTTPS-Zertifikatinformationen.</span><span class="sxs-lookup"><span data-stu-id="2f940-367">HTTPS certificate information.</span></span> |
| <span data-ttu-id="2f940-368">Microsoft.AspNetCore.StaticFiles</span><span class="sxs-lookup"><span data-stu-id="2f940-368">Microsoft.AspNetCore.StaticFiles</span></span>    | <span data-ttu-id="2f940-369">Die bereitgestellten Dateien.</span><span class="sxs-lookup"><span data-stu-id="2f940-369">Files served.</span></span> |
| <span data-ttu-id="2f940-370">Microsoft.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="2f940-370">Microsoft.EntityFrameworkCore</span></span>       | <span data-ttu-id="2f940-371">Allgemeine Entity Framework Core-Diagnose.</span><span class="sxs-lookup"><span data-stu-id="2f940-371">General Entity Framework Core diagnostics.</span></span> <span data-ttu-id="2f940-372">Datenbankaktivität und -konfiguration, Änderungserkennung, Migrationen.</span><span class="sxs-lookup"><span data-stu-id="2f940-372">Database activity and configuration, change detection, migrations.</span></span> |

<span data-ttu-id="2f940-373">Wenn Sie weitere Kategorien im Konsolenfenster anzeigen möchten, legen Sie **appsettings.Development.json** auf Folgendes fest:</span><span class="sxs-lookup"><span data-stu-id="2f940-373">To view more categories in the console window, set **appsettings.Development.json** to the following:</span></span>

[!code-json[](index/samples/3.x/MyMain/appsettings.Trace.json)]

<!-- Review: What other providers support scopes? Console is not generally used in staging/production  -->

<a name="logscopes"></a>

## <a name="log-scopes"></a><span data-ttu-id="2f940-374">Protokollbereiche</span><span class="sxs-lookup"><span data-stu-id="2f940-374">Log scopes</span></span>

 <span data-ttu-id="2f940-375">Ein *Bereich* kann einen Satz logischer Vorgänge gruppieren.</span><span class="sxs-lookup"><span data-stu-id="2f940-375">A *scope* can group a set of logical operations.</span></span> <span data-ttu-id="2f940-376">Diese Gruppierung kann verwendet werden, um an jedes Protokoll, das als Teil einer Gruppe erstellt wird, die gleichen Daten anzufügen.</span><span class="sxs-lookup"><span data-stu-id="2f940-376">This grouping can be used to attach the same data to each log that's created as part of a set.</span></span> <span data-ttu-id="2f940-377">So kann beispielsweise jedes Protokoll, das im Rahmen der Verarbeitung einer Transaktion erstellt wird, die Transaktions-ID enthalten.</span><span class="sxs-lookup"><span data-stu-id="2f940-377">For example, every log created as part of processing a transaction can include the transaction ID.</span></span>

<span data-ttu-id="2f940-378">Ein Bereich:</span><span class="sxs-lookup"><span data-stu-id="2f940-378">A scope:</span></span>

* <span data-ttu-id="2f940-379">Ist ein <xref:System.IDisposable>-Typ, der von der <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*>-Methode zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="2f940-379">Is an <xref:System.IDisposable> type that's returned by the <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> method.</span></span>
* <span data-ttu-id="2f940-380">Er bleibt bestehen, bis er verworfen wird.</span><span class="sxs-lookup"><span data-stu-id="2f940-380">Lasts until it's disposed.</span></span>

<span data-ttu-id="2f940-381">Die folgenden Anbieter unterstützen Bereiche:</span><span class="sxs-lookup"><span data-stu-id="2f940-381">The following providers support scopes:</span></span>

* `Console`
* [<span data-ttu-id="2f940-382">AzureAppServicesFile und AzureAppServicesBlob</span><span class="sxs-lookup"><span data-stu-id="2f940-382">AzureAppServicesFile and AzureAppServicesBlob</span></span>](xref:Microsoft.Extensions.Logging.AzureAppServices.BatchingLoggerOptions.IncludeScopes)

<span data-ttu-id="2f940-383">Verwenden Sie einen Bereich, indem Sie Protokollierungsaufrufe mit einem `using`-Block umschließen, der als Wrapper verwendet wird:</span><span class="sxs-lookup"><span data-stu-id="2f940-383">Use a scope by wrapping logger calls in a `using` block:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiDTO/Controllers/TestController.cs?name=snippet_Scopes)]

<span data-ttu-id="2f940-384">Der folgende JSON-Code aktiviert Bereiche für den Konsolenanbieter:</span><span class="sxs-lookup"><span data-stu-id="2f940-384">The following JSON enables scopes for the console provider:</span></span>

[!code-json[](index/samples/3.x/TodoApiDTO/appsettings.Scopes.json)]

<span data-ttu-id="2f940-385">Der folgende Code aktiviert Bereiche für den Konsolenanbieter:</span><span class="sxs-lookup"><span data-stu-id="2f940-385">The following code enables scopes for the console provider:</span></span>

[!code-csharp[](index/samples/3.x/MyMain/Program.cs?name=snippet_Scopes)]

<span data-ttu-id="2f940-386">Im Allgemeinen sollte Protokollierung in der Konfiguration und nicht im Code angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="2f940-386">Generally, logging should be specified in configuration and not code.</span></span>

<a name="bilp"></a>

## <a name="built-in-logging-providers"></a><span data-ttu-id="2f940-387">Integrierte Protokollierungsanbieter</span><span class="sxs-lookup"><span data-stu-id="2f940-387">Built-in logging providers</span></span>

<span data-ttu-id="2f940-388">ASP.NET Core umfasst die folgenden Protokollierungsanbieter:</span><span class="sxs-lookup"><span data-stu-id="2f940-388">ASP.NET Core includes the following logging providers:</span></span>

* [<span data-ttu-id="2f940-389">Konsole</span><span class="sxs-lookup"><span data-stu-id="2f940-389">Console</span></span>](#console)
* [<span data-ttu-id="2f940-390">Debuggen</span><span class="sxs-lookup"><span data-stu-id="2f940-390">Debug</span></span>](#debug)
* [<span data-ttu-id="2f940-391">EventSource</span><span class="sxs-lookup"><span data-stu-id="2f940-391">EventSource</span></span>](#event-source)
* [<span data-ttu-id="2f940-392">EventLog</span><span class="sxs-lookup"><span data-stu-id="2f940-392">EventLog</span></span>](#welog)
* [<span data-ttu-id="2f940-393">AzureAppServicesFile und AzureAppServicesBlob</span><span class="sxs-lookup"><span data-stu-id="2f940-393">AzureAppServicesFile and AzureAppServicesBlob</span></span>](#azure-app-service)
* [<span data-ttu-id="2f940-394">ApplicationInsights</span><span class="sxs-lookup"><span data-stu-id="2f940-394">ApplicationInsights</span></span>](#azure-application-insights)

<span data-ttu-id="2f940-395">Informationen zu `stdout` und zur Debugprotokollierung mit dem ASP.NET Core-Modul finden Sie unter <xref:test/troubleshoot-azure-iis> und <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span><span class="sxs-lookup"><span data-stu-id="2f940-395">For information on `stdout` and debug logging with the ASP.NET Core Module, see <xref:test/troubleshoot-azure-iis> and <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

### <a name="console"></a><span data-ttu-id="2f940-396">Konsole</span><span class="sxs-lookup"><span data-stu-id="2f940-396">Console</span></span>

<span data-ttu-id="2f940-397">Der `Console`-Anbieter protokolliert die Ausgabe in der Konsole.</span><span class="sxs-lookup"><span data-stu-id="2f940-397">The `Console` provider logs output to the console.</span></span> <span data-ttu-id="2f940-398">Weitere Informationen zum Anzeigen von `Console`-Protokollen in der Entwicklung finden Sie unter [ Protokollieren der Ausgabe von dotnet run und Visual Studio](#dnrvs).</span><span class="sxs-lookup"><span data-stu-id="2f940-398">For more information on viewing `Console` logs in development, see [Logging output from dotnet run and Visual Studio](#dnrvs).</span></span>

### <a name="debug"></a><span data-ttu-id="2f940-399">Debug</span><span class="sxs-lookup"><span data-stu-id="2f940-399">Debug</span></span>

<span data-ttu-id="2f940-400">Der `Debug`-Anbieter schreibt die Protokollausgabe mithilfe der [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug)-Klasse.</span><span class="sxs-lookup"><span data-stu-id="2f940-400">The `Debug` provider writes log output by using the [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) class.</span></span> <span data-ttu-id="2f940-401">Aufrufe von `System.Diagnostics.Debug.WriteLine` schreiben in den `Debug`-Anbieter.</span><span class="sxs-lookup"><span data-stu-id="2f940-401">Calls to `System.Diagnostics.Debug.WriteLine` write to the `Debug` provider.</span></span>

<span data-ttu-id="2f940-402">Unter Linux ist der Speicherort des Protokolls des `Debug`-Anbieters abhängig von der Distribution und kann wie folgt lauten:</span><span class="sxs-lookup"><span data-stu-id="2f940-402">On Linux, the `Debug` provider log location is distribution-dependent and may be one of the following:</span></span>

* <span data-ttu-id="2f940-403">*/var/log/message*</span><span class="sxs-lookup"><span data-stu-id="2f940-403">*/var/log/message*</span></span>
* <span data-ttu-id="2f940-404">*/var/log/syslog*</span><span class="sxs-lookup"><span data-stu-id="2f940-404">*/var/log/syslog*</span></span>

### <a name="event-source"></a><span data-ttu-id="2f940-405">Ereignisquelle</span><span class="sxs-lookup"><span data-stu-id="2f940-405">Event Source</span></span>

<span data-ttu-id="2f940-406">Der `EventSource`-Anbieter schreibt in eine plattformübergreifende Ereignisquelle mit dem Namen `Microsoft-Extensions-Logging`.</span><span class="sxs-lookup"><span data-stu-id="2f940-406">The `EventSource` provider writes to a cross-platform event source with the name `Microsoft-Extensions-Logging`.</span></span> <span data-ttu-id="2f940-407">Unter Windows verwendet der Anbieter die [Ereignisablaufverfolgung für Windows (ETW)](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span><span class="sxs-lookup"><span data-stu-id="2f940-407">On Windows, the provider uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span>

#### <a name="dotnet-trace-tooling"></a><span data-ttu-id="2f940-408">dotnet-trace-Tool</span><span class="sxs-lookup"><span data-stu-id="2f940-408">dotnet trace tooling</span></span>

<span data-ttu-id="2f940-409">Das Tool [dotnet-trace](/dotnet/core/diagnostics/dotnet-trace) ist ein plattformübergreifendes globales Befehlszeilenschnittstellentool zum Sammeln von .NET Core-Ablaufverfolgungen eines ausgeführten Prozesses.</span><span class="sxs-lookup"><span data-stu-id="2f940-409">The [dotnet-trace](/dotnet/core/diagnostics/dotnet-trace) tool is a cross-platform CLI global tool that enables the collection of .NET Core traces of a running process.</span></span> <span data-ttu-id="2f940-410">Das Tool sammelt <xref:Microsoft.Extensions.Logging.EventSource>-Anbieterdaten mithilfe einer <xref:Microsoft.Extensions.Logging.EventSource.LoggingEventSource>.</span><span class="sxs-lookup"><span data-stu-id="2f940-410">The tool collects <xref:Microsoft.Extensions.Logging.EventSource> provider data using a <xref:Microsoft.Extensions.Logging.EventSource.LoggingEventSource>.</span></span>

<span data-ttu-id="2f940-411">Installationsanweisungen finden Sie unter [dotnet-trace](/dotnet/core/diagnostics/dotnet-trace).</span><span class="sxs-lookup"><span data-stu-id="2f940-411">See [dotnet-trace](/dotnet/core/diagnostics/dotnet-trace) for installation instructions.</span></span>

<span data-ttu-id="2f940-412">Verwenden Sie das dotnet-trace-Tool, um die Ablaufverfolgung aus einer App zu erfassen:</span><span class="sxs-lookup"><span data-stu-id="2f940-412">Use the dotnet trace tooling to collect a trace from an app:</span></span>

1. <span data-ttu-id="2f940-413">Führen Sie die App mit dem Befehl `dotnet run` aus.</span><span class="sxs-lookup"><span data-stu-id="2f940-413">Run the app with the `dotnet run` command.</span></span>
1. <span data-ttu-id="2f940-414">Bestimmen Sie den Prozessbezeichner der .NET Core-App:</span><span class="sxs-lookup"><span data-stu-id="2f940-414">Determine the process identifier (PID) of the .NET Core app:</span></span>
   * <span data-ttu-id="2f940-415">Unter Windows können Sie einen der folgenden Ansätze verwenden:</span><span class="sxs-lookup"><span data-stu-id="2f940-415">On Windows, use one of the following approaches:</span></span>
     * <span data-ttu-id="2f940-416">Task-Manager (STRG+ALT+ENTF)</span><span class="sxs-lookup"><span data-stu-id="2f940-416">Task Manager (Ctrl+Alt+Del)</span></span>
     * [<span data-ttu-id="2f940-417">tasklist-Befehl</span><span class="sxs-lookup"><span data-stu-id="2f940-417">tasklist command</span></span>](/windows-server/administration/windows-commands/tasklist)
     * [<span data-ttu-id="2f940-418">PowerShell-Befehl „Get-Process“</span><span class="sxs-lookup"><span data-stu-id="2f940-418">Get-Process Powershell command</span></span>](/powershell/module/microsoft.powershell.management/get-process)
   * <span data-ttu-id="2f940-419">Verwenden Sie unter Linux den [pidof-Befehl](https://refspecs.linuxfoundation.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/pidof.html).</span><span class="sxs-lookup"><span data-stu-id="2f940-419">On Linux, use the [pidof command](https://refspecs.linuxfoundation.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/pidof.html).</span></span>

   <span data-ttu-id="2f940-420">Suchen Sie den Prozessbezeichner für den Prozess, der den gleichen Namen wie die App-Assembly hat.</span><span class="sxs-lookup"><span data-stu-id="2f940-420">Find the PID for the process that has the same name as the app's assembly.</span></span>

1. <span data-ttu-id="2f940-421">Führen Sie den `dotnet trace`-Befehl aus.</span><span class="sxs-lookup"><span data-stu-id="2f940-421">Execute the `dotnet trace` command.</span></span>

   <span data-ttu-id="2f940-422">Allgemeine Befehlssyntax:</span><span class="sxs-lookup"><span data-stu-id="2f940-422">General command syntax:</span></span>

   ```dotnetcli
   dotnet trace collect -p {PID} 
       --providers Microsoft-Extensions-Logging:{Keyword}:{Event Level}
           :FilterSpecs=\"
               {Logger Category 1}:{Event Level 1};
               {Logger Category 2}:{Event Level 2};
               ...
               {Logger Category N}:{Event Level N}\"
   ```

   <span data-ttu-id="2f940-423">Wenn Sie eine PowerShell-Befehlsshell verwenden, schließen Sie den `--providers`-Wert in einfache Anführungszeichen (`'`) ein:</span><span class="sxs-lookup"><span data-stu-id="2f940-423">When using a PowerShell command shell, enclose the `--providers` value in single quotes (`'`):</span></span>

   ```dotnetcli
   dotnet trace collect -p {PID} 
       --providers 'Microsoft-Extensions-Logging:{Keyword}:{Event Level}
           :FilterSpecs=\"
               {Logger Category 1}:{Event Level 1};
               {Logger Category 2}:{Event Level 2};
               ...
               {Logger Category N}:{Event Level N}\"'
   ```

   <span data-ttu-id="2f940-424">Auf Plattformen, die nicht unter Windows ausgeführt werden, fügen Sie die `-f speedscope`-Option hinzu, um das Format der Ablaufverfolgungsdatei der Ausgabe in `speedscope` zu ändern.</span><span class="sxs-lookup"><span data-stu-id="2f940-424">On non-Windows platforms, add the `-f speedscope` option to change the format of the output trace file to `speedscope`.</span></span>

   | <span data-ttu-id="2f940-425">Stichwort</span><span class="sxs-lookup"><span data-stu-id="2f940-425">Keyword</span></span> | <span data-ttu-id="2f940-426">BESCHREIBUNG</span><span class="sxs-lookup"><span data-stu-id="2f940-426">Description</span></span> |
   | :-----: | ----------- |
   | <span data-ttu-id="2f940-427">1</span><span class="sxs-lookup"><span data-stu-id="2f940-427">1</span></span>       | <span data-ttu-id="2f940-428">Protokolliert Metaereignisse über die `LoggingEventSource`.</span><span class="sxs-lookup"><span data-stu-id="2f940-428">Log meta events about the `LoggingEventSource`.</span></span> <span data-ttu-id="2f940-429">Es werden keine Ereignisse von `ILogger`) protokolliert.</span><span class="sxs-lookup"><span data-stu-id="2f940-429">Doesn't log events from `ILogger`).</span></span> |
   | <span data-ttu-id="2f940-430">2</span><span class="sxs-lookup"><span data-stu-id="2f940-430">2</span></span>       | <span data-ttu-id="2f940-431">Aktiviert das `Message`-Ereignis, wenn `ILogger.Log()` aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="2f940-431">Turns on the `Message` event when `ILogger.Log()` is called.</span></span> <span data-ttu-id="2f940-432">Die Informationen werden in einer programmgesteuerten (nicht formatierten) Weise ausgegeben.</span><span class="sxs-lookup"><span data-stu-id="2f940-432">Provides information in a programmatic (not formatted) way.</span></span> |
   | <span data-ttu-id="2f940-433">4</span><span class="sxs-lookup"><span data-stu-id="2f940-433">4</span></span>       | <span data-ttu-id="2f940-434">Aktiviert das `FormatMessage`-Ereignis, wenn `ILogger.Log()` aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="2f940-434">Turns on the `FormatMessage` event when `ILogger.Log()` is called.</span></span> <span data-ttu-id="2f940-435">Gibt die formatierte Zeichenfolgeversion der Informationen an.</span><span class="sxs-lookup"><span data-stu-id="2f940-435">Provides the formatted string version of the information.</span></span> |
   | <span data-ttu-id="2f940-436">8</span><span class="sxs-lookup"><span data-stu-id="2f940-436">8</span></span>       | <span data-ttu-id="2f940-437">Aktiviert das `MessageJson`-Ereignis, wenn `ILogger.Log()` aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="2f940-437">Turns on the `MessageJson` event when `ILogger.Log()` is called.</span></span> <span data-ttu-id="2f940-438">Stellt eine JSON-Darstellung der Argumente bereit.</span><span class="sxs-lookup"><span data-stu-id="2f940-438">Provides a JSON representation of the arguments.</span></span> |

   | <span data-ttu-id="2f940-439">Ereignisgrad</span><span class="sxs-lookup"><span data-stu-id="2f940-439">Event Level</span></span> | <span data-ttu-id="2f940-440">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="2f940-440">Description</span></span>     |
   | :---------: | --------------- |
   | <span data-ttu-id="2f940-441">0</span><span class="sxs-lookup"><span data-stu-id="2f940-441">0</span></span>           | `LogAlways`     |
   | <span data-ttu-id="2f940-442">1</span><span class="sxs-lookup"><span data-stu-id="2f940-442">1</span></span>           | `Critical`      |
   | <span data-ttu-id="2f940-443">2</span><span class="sxs-lookup"><span data-stu-id="2f940-443">2</span></span>           | `Error`         |
   | <span data-ttu-id="2f940-444">3</span><span class="sxs-lookup"><span data-stu-id="2f940-444">3</span></span>           | `Warning`       |
   | <span data-ttu-id="2f940-445">4</span><span class="sxs-lookup"><span data-stu-id="2f940-445">4</span></span>           | `Informational` |
   | <span data-ttu-id="2f940-446">5</span><span class="sxs-lookup"><span data-stu-id="2f940-446">5</span></span>           | `Verbose`       |

   <span data-ttu-id="2f940-447">`FilterSpecs`-Einträge für `{Logger Category}` und `{Event Level}` stellen zusätzliche Protokollfilterbedingungen dar.</span><span class="sxs-lookup"><span data-stu-id="2f940-447">`FilterSpecs` entries for `{Logger Category}` and `{Event Level}` represent additional log filtering conditions.</span></span> <span data-ttu-id="2f940-448">Trennen Sie die `FilterSpecs`-Einträge durch ein Semikolon (`;`).</span><span class="sxs-lookup"><span data-stu-id="2f940-448">Separate `FilterSpecs` entries with a semicolon (`;`).</span></span>

   <span data-ttu-id="2f940-449">Beispiel mit einer Windows-Befehlsshell (**keine** einfachen Anführungszeichen um den `--providers`-Wert):</span><span class="sxs-lookup"><span data-stu-id="2f940-449">Example using a Windows command shell (**no** single quotes around the `--providers` value):</span></span>

   ```dotnetcli
   dotnet trace collect -p {PID} --providers Microsoft-Extensions-Logging:4:2:FilterSpecs=\"Microsoft.AspNetCore.Hosting*:4\"
   ```

   <span data-ttu-id="2f940-450">Mit dem vorangestellten Komma wird Folgendes aktiviert:</span><span class="sxs-lookup"><span data-stu-id="2f940-450">The preceding command activates:</span></span>

   * <span data-ttu-id="2f940-451">Die Ereignisquellenprotokollierung zur Erzeugung von formatierten Zeichenfolgen (`4`) für Fehler (`2`)</span><span class="sxs-lookup"><span data-stu-id="2f940-451">The Event Source logger to produce formatted strings (`4`) for errors (`2`).</span></span>
   * <span data-ttu-id="2f940-452">`Microsoft.AspNetCore.Hosting`-Protokollierung auf `Informational`-Protokollierungsebene (`4`)</span><span class="sxs-lookup"><span data-stu-id="2f940-452">`Microsoft.AspNetCore.Hosting` logging at the `Informational` logging level (`4`).</span></span>

1. <span data-ttu-id="2f940-453">Halten Sie das dotnet-trace-Tool an, indem Sie die EINGABETASTE oder STRG+C drücken.</span><span class="sxs-lookup"><span data-stu-id="2f940-453">Stop the dotnet trace tooling by pressing the Enter key or Ctrl+C.</span></span>

   <span data-ttu-id="2f940-454">Die Ablaufverfolgung wird mit dem Namen *trace.nettrace* in dem Ordner gespeichert, in dem der `dotnet trace`-Befehl ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="2f940-454">The trace is saved with the name *trace.nettrace* in the folder where the `dotnet trace` command is executed.</span></span>

1. <span data-ttu-id="2f940-455">Öffnen Sie die Ablaufverfolgung mit [Perfview](#perfview).</span><span class="sxs-lookup"><span data-stu-id="2f940-455">Open the trace with [Perfview](#perfview).</span></span> <span data-ttu-id="2f940-456">Öffnen Sie die Datei *trace.nettrace*, und untersuchen Sie die Ablaufverfolgungsereignisse.</span><span class="sxs-lookup"><span data-stu-id="2f940-456">Open the *trace.nettrace* file and explore the trace events.</span></span>

<span data-ttu-id="2f940-457">Wenn die App den Host nicht mit `CreateDefaultBuilder` erstellt, fügen Sie den [Ereignisquellenanbieter](#event-source-provider) zur Protokollierungskonfiguration der App hinzu.</span><span class="sxs-lookup"><span data-stu-id="2f940-457">If the app doesn't build the host with `CreateDefaultBuilder`, add the [Event Source provider](#event-source-provider) to the app's logging configuration.</span></span>

<span data-ttu-id="2f940-458">Weitere Informationen finden Sie unter:</span><span class="sxs-lookup"><span data-stu-id="2f940-458">For more information, see:</span></span>

* <span data-ttu-id="2f940-459">[Ablaufverfolgung für das Hilfsprogramm zur Leistungsanalyse (dotnet-trace)](/dotnet/core/diagnostics/dotnet-trace) (.NET Core-Dokumentation)</span><span class="sxs-lookup"><span data-stu-id="2f940-459">[Trace for performance analysis utility (dotnet-trace)](/dotnet/core/diagnostics/dotnet-trace) (.NET Core documentation)</span></span>
* <span data-ttu-id="2f940-460">[Ablaufverfolgung für das Hilfsprogramm zur Leistungsanalyse (dotnet-trace)](https://github.com/dotnet/diagnostics/blob/master/documentation/dotnet-trace-instructions.md) (GitHub-Repository-Dokumentation zu dotnet/diagnostics)</span><span class="sxs-lookup"><span data-stu-id="2f940-460">[Trace for performance analysis utility (dotnet-trace)](https://github.com/dotnet/diagnostics/blob/master/documentation/dotnet-trace-instructions.md) (dotnet/diagnostics GitHub repository documentation)</span></span>
* <span data-ttu-id="2f940-461">[LoggingEventSource-Klasse](xref:Microsoft.Extensions.Logging.EventSource.LoggingEventSource) (.NET API-Browser)</span><span class="sxs-lookup"><span data-stu-id="2f940-461">[LoggingEventSource Class](xref:Microsoft.Extensions.Logging.EventSource.LoggingEventSource) (.NET API Browser)</span></span>
* <xref:System.Diagnostics.Tracing.EventLevel>
* <span data-ttu-id="2f940-462">[LoggingEventSource-Verweisquelle (3.0)](https://github.com/dotnet/extensions/blob/release/3.0/src/Logging/Logging.EventSource/src/LoggingEventSource.cs): Ändern Sie für das Abrufen der Verweisquelle für eine andere Version den Branch in `release/{Version}`, wobei `{Version}` die Version der gewünschten ASP.NET Core-Version darstellt.</span><span class="sxs-lookup"><span data-stu-id="2f940-462">[LoggingEventSource reference source (3.0)](https://github.com/dotnet/extensions/blob/release/3.0/src/Logging/Logging.EventSource/src/LoggingEventSource.cs): To obtain reference source for a different version, change the branch to `release/{Version}`, where `{Version}` is the version of ASP.NET Core desired.</span></span>
* <span data-ttu-id="2f940-463">[Perfview](#perfview): Hilfreich zum Anzeigen der Ablaufverfolgung für Ereignisquellen.</span><span class="sxs-lookup"><span data-stu-id="2f940-463">[Perfview](#perfview): Useful for viewing Event Source traces.</span></span>

#### <a name="perfview"></a><span data-ttu-id="2f940-464">PerfView</span><span class="sxs-lookup"><span data-stu-id="2f940-464">Perfview</span></span>

<span data-ttu-id="2f940-465">Verwenden Sie das [PerfView-Hilfsprogramm](https://github.com/Microsoft/perfview) zum Sammeln und Anzeigen von Protokollen.</span><span class="sxs-lookup"><span data-stu-id="2f940-465">Use the [PerfView utility](https://github.com/Microsoft/perfview) to collect and view logs.</span></span> <span data-ttu-id="2f940-466">Es gibt andere Tools zur Anzeige von ETW-Protokollen, aber PerfView bietet die besten Ergebnisse bei der Arbeit mit ETW-Ereignissen, die von ASP.NET Core ausgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="2f940-466">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET Core.</span></span>

<span data-ttu-id="2f940-467">Um PerfView für das Erfassen von Ereignissen zu konfigurieren, die von diesem Anbieter protokolliert wurden, fügen Sie die Zeichenfolge `*Microsoft-Extensions-Logging` zur Liste **Zusätzliche Anbieter** hinzu.</span><span class="sxs-lookup"><span data-stu-id="2f940-467">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="2f940-468">Vergessen Sie nicht das `*` am Anfang der Zeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="2f940-468">Don't miss the `*` at the start of the string.</span></span>

<a name="welog"></a>

### <a name="windows-eventlog"></a><span data-ttu-id="2f940-469">Windows-EventLog</span><span class="sxs-lookup"><span data-stu-id="2f940-469">Windows EventLog</span></span>

<span data-ttu-id="2f940-470">Der `EventLog`-Anbieter sendet die Protokollausgabe an das Windows-Ereignisprotokoll.</span><span class="sxs-lookup"><span data-stu-id="2f940-470">The `EventLog` provider sends log output to the Windows Event Log.</span></span> <span data-ttu-id="2f940-471">Im Gegensatz zu den anderen Anbietern erbt der `EventLog`-Anbieter ***nicht*** die Standardeinstellungen für Nicht-Anbieter.</span><span class="sxs-lookup"><span data-stu-id="2f940-471">Unlike the other providers, the `EventLog` provider does ***not*** inherit the default non-provider settings.</span></span> <span data-ttu-id="2f940-472">Wenn keine `EventLog`-Protokolleinstellungen angegeben werden, wird [standardmäßig LogLevel.Warning](https://github.com/dotnet/extensions/blob/release/3.1/src/Hosting/Hosting/src/Host.cs#L99-L103) verwendet.</span><span class="sxs-lookup"><span data-stu-id="2f940-472">If `EventLog` log settings aren't specified, they [default to LogLevel.Warning](https://github.com/dotnet/extensions/blob/release/3.1/src/Hosting/Hosting/src/Host.cs#L99-L103).</span></span>

<span data-ttu-id="2f940-473">Legen Sie die Protokollierungsebene fest, um Ereignisse, die niedriger als <xref:Microsoft.Extensions.Logging.LogLevel.Warning?displayProperty=nameWithType> sind, zu protokollieren.</span><span class="sxs-lookup"><span data-stu-id="2f940-473">To log events lower than <xref:Microsoft.Extensions.Logging.LogLevel.Warning?displayProperty=nameWithType>, explicitly set the log level.</span></span> <span data-ttu-id="2f940-474">Im folgenden Beispiel wird der Standardprotokolliergrad auf <xref:Microsoft.Extensions.Logging.LogLevel.Information?displayProperty=nameWithType> festgelegt:</span><span class="sxs-lookup"><span data-stu-id="2f940-474">The following example sets the Event Log default log level to <xref:Microsoft.Extensions.Logging.LogLevel.Information?displayProperty=nameWithType>:</span></span>

```json
"Logging": {
  "EventLog": {
    "LogLevel": {
      "Default": "Information"
    }
  }
}
```

<span data-ttu-id="2f940-475">[AddEventLog-Überladungen](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions) können <xref:Microsoft.Extensions.Logging.EventLog.EventLogSettings> übergeben.</span><span class="sxs-lookup"><span data-stu-id="2f940-475">[AddEventLog overloads](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions) can pass in <xref:Microsoft.Extensions.Logging.EventLog.EventLogSettings>.</span></span> <span data-ttu-id="2f940-476">Wenn `null` oder nicht angegeben, werden die folgenden Standardeinstellungen verwendet:</span><span class="sxs-lookup"><span data-stu-id="2f940-476">If `null` or not specified, the following default settings are used:</span></span>

* <span data-ttu-id="2f940-477">`LogName`: „Anwendung“</span><span class="sxs-lookup"><span data-stu-id="2f940-477">`LogName`: "Application"</span></span>
* <span data-ttu-id="2f940-478">`SourceName`: „.NET Runtime“</span><span class="sxs-lookup"><span data-stu-id="2f940-478">`SourceName`: ".NET Runtime"</span></span>
* <span data-ttu-id="2f940-479">`MachineName`: Der Name des lokalen Computers wird verwendet.</span><span class="sxs-lookup"><span data-stu-id="2f940-479">`MachineName`: The local machine name is used.</span></span>

<span data-ttu-id="2f940-480">Der folgende Code ändert `SourceName` aus dem Standardwert `".NET Runtime"` in `MyLogs`:</span><span class="sxs-lookup"><span data-stu-id="2f940-480">The following code changes the `SourceName` from the default value of `".NET Runtime"` to `MyLogs`:</span></span>

[!code-csharp[](index/samples/3.x/MyMain/Program.cs?name=snippetEventLog)]

### <a name="azure-app-service"></a><span data-ttu-id="2f940-481">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="2f940-481">Azure App Service</span></span>

<span data-ttu-id="2f940-482">Das Anbieterpaket [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) schreibt Protokolle in Textdateien in das Dateisystem einer Azure App Service-App und in [Blob Storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in einem Azure Storage-Konto.</span><span class="sxs-lookup"><span data-stu-id="2f940-482">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span>

<span data-ttu-id="2f940-483">Das Anbieterpaket ist nicht im freigegebenen Framework enthalten.</span><span class="sxs-lookup"><span data-stu-id="2f940-483">The provider package isn't included in the shared framework.</span></span> <span data-ttu-id="2f940-484">Zum Verwenden des Anbieters müssen Sie das Anbieterpaket dem Projekt hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="2f940-484">To use the provider, add the provider package to the project.</span></span>

<span data-ttu-id="2f940-485">Verwenden Sie <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> und <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions> wie im folgenden Beispiel, um die Anbietereinstellungen zu konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="2f940-485">To configure provider settings, use <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> and <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>, as shown in the following example:</span></span>

[!code-csharp[](index/samples/3.x/MyMain/Program.cs?name=snippet_AzLogOptions)]

<span data-ttu-id="2f940-486">Wenn eine Bereitstellung in Azure App Service erfolgt, verwendet die App die Einstellungen im Abschnitt [App Service logs](/azure/app-service/web-sites-enable-diagnostic-log/#enable-application-logging-windows) (App Service-Protokolle) auf der Seite **App Service** im Azure-Portal.</span><span class="sxs-lookup"><span data-stu-id="2f940-486">When deployed to Azure App Service, the app uses the settings in the [App Service logs](/azure/app-service/web-sites-enable-diagnostic-log/#enable-application-logging-windows) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="2f940-487">Bei einem Update der folgenden Einstellungen werden die Änderungen sofort wirksam, ohne dass ein Neustart oder eine erneute Bereitstellung der App notwendig ist.</span><span class="sxs-lookup"><span data-stu-id="2f940-487">When the following settings are updated, the changes take effect immediately without requiring a restart or redeployment of the app.</span></span>

* <span data-ttu-id="2f940-488">**Anwendungsprotokoll (Dateisystem)**</span><span class="sxs-lookup"><span data-stu-id="2f940-488">**Application Logging (Filesystem)**</span></span>
* <span data-ttu-id="2f940-489">**Anwendungsprotokoll (Blob)**</span><span class="sxs-lookup"><span data-stu-id="2f940-489">**Application Logging (Blob)**</span></span>

<span data-ttu-id="2f940-490">Der Standardspeicherort für Protokolldateien ist der Ordner *D:\\home\\LogFiles\\Application*, und der standardmäßige Dateiname lautet *diagnostics-yyyymmdd.txt*.</span><span class="sxs-lookup"><span data-stu-id="2f940-490">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="2f940-491">Die Dateigröße ist standardmäßig auf 10 MB beschränkt, und die maximal zulässige Anzahl beibehaltener Dateien lautet 2.</span><span class="sxs-lookup"><span data-stu-id="2f940-491">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="2f940-492">Der Standardblobname lautet *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span><span class="sxs-lookup"><span data-stu-id="2f940-492">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span>

<span data-ttu-id="2f940-493">Der Anbieter führt nur Protokollierung aus, wenn das Projekt in der Azure-Umgebung ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="2f940-493">This provider only logs when the project runs in the Azure environment.</span></span>

#### <a name="azure-log-streaming"></a><span data-ttu-id="2f940-494">Azure-Protokollstreaming</span><span class="sxs-lookup"><span data-stu-id="2f940-494">Azure log streaming</span></span>

<span data-ttu-id="2f940-495">Azure-Protokollstreaming unterstützt das Anzeigen der Protokollaktivität in Echtzeit:</span><span class="sxs-lookup"><span data-stu-id="2f940-495">Azure log streaming supports viewing log activity in real time from:</span></span>

* <span data-ttu-id="2f940-496">App-Server</span><span class="sxs-lookup"><span data-stu-id="2f940-496">The app server</span></span>
* <span data-ttu-id="2f940-497">Webserver</span><span class="sxs-lookup"><span data-stu-id="2f940-497">The web server</span></span>
* <span data-ttu-id="2f940-498">Ablaufverfolgung für Anforderungsfehler</span><span class="sxs-lookup"><span data-stu-id="2f940-498">Failed request tracing</span></span>

<span data-ttu-id="2f940-499">So konfigurieren Sie das Azure-Protokollstreaming</span><span class="sxs-lookup"><span data-stu-id="2f940-499">To configure Azure log streaming:</span></span>

* <span data-ttu-id="2f940-500">Navigieren Sie von der Portalseite Ihrer App zur Seite **App Service-Protokolle**.</span><span class="sxs-lookup"><span data-stu-id="2f940-500">Navigate to the **App Service logs** page from the app's portal page.</span></span>
* <span data-ttu-id="2f940-501">Legen Sie **Anwendungsprotokollierung (Dateisystem)** auf **Ein** fest.</span><span class="sxs-lookup"><span data-stu-id="2f940-501">Set **Application Logging (Filesystem)** to **On**.</span></span>
* <span data-ttu-id="2f940-502">Wählen Sie die **Protokollierungsebene**.</span><span class="sxs-lookup"><span data-stu-id="2f940-502">Choose the log **Level**.</span></span> <span data-ttu-id="2f940-503">Diese Einstellung gilt nur für Azure-Protokollstreaming.</span><span class="sxs-lookup"><span data-stu-id="2f940-503">This setting only applies to Azure log streaming.</span></span>

<span data-ttu-id="2f940-504">Navigieren Sie zur Seite **Log Stream** (Protokollstream), um Protokolle anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="2f940-504">Navigate to the **Log Stream** page to view logs.</span></span> <span data-ttu-id="2f940-505">Die protokollierten Nachrichten werden mit der `ILogger`-Schnittstelle protokolliert.</span><span class="sxs-lookup"><span data-stu-id="2f940-505">The logged messages are logged with the `ILogger` interface.</span></span>

### <a name="azure-application-insights"></a><span data-ttu-id="2f940-506">Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="2f940-506">Azure Application Insights</span></span>

<span data-ttu-id="2f940-507">Das [Microsoft.Extensions.Logging.ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights)-Anbieterpaket schreibt Protokolle in [Azure Application Insights](/azure/azure-monitor/app/cloudservices).</span><span class="sxs-lookup"><span data-stu-id="2f940-507">The [Microsoft.Extensions.Logging.ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights) provider package writes logs to [Azure Application Insights](/azure/azure-monitor/app/cloudservices).</span></span> <span data-ttu-id="2f940-508">Application Insights ist ein Dienst, der eine Web-App überwacht und Tools für Abfragen und Analysen von Telemetriedaten zur Verfügung stellt.</span><span class="sxs-lookup"><span data-stu-id="2f940-508">Application Insights is a service that monitors a web app and provides tools for querying and analyzing the telemetry data.</span></span> <span data-ttu-id="2f940-509">Wenn Sie diesen Anbieter verwenden, können Sie Ihre Protokolle mithilfe der Application Insights-Tools abfragen und analysieren.</span><span class="sxs-lookup"><span data-stu-id="2f940-509">If you use this provider, you can query and analyze your logs by using the Application Insights tools.</span></span>

<span data-ttu-id="2f940-510">Der Protokollierungsanbieter ist als Abhängigkeit von [Microsoft.ApplicationInsights.AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) enthalten, d.h. als das Paket, das alle verfügbaren Telemetriedaten für ASP.NET Core bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="2f940-510">The logging provider is included as a dependency of [Microsoft.ApplicationInsights.AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore), which is the package that provides all available telemetry for ASP.NET Core.</span></span> <span data-ttu-id="2f940-511">Wenn Sie dieses Paket verwenden, ist nicht erforderlich, das Anbieterpaket zu installieren.</span><span class="sxs-lookup"><span data-stu-id="2f940-511">If you use this package, you don't have to install the provider package.</span></span>

<span data-ttu-id="2f940-512">Das [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web)-Paket bezieht sich auf ASP.NET 4.x, nicht auf ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2f940-512">The [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) package is for ASP.NET 4.x, not ASP.NET Core.</span></span>

<span data-ttu-id="2f940-513">Weitere Informationen finden Sie in den folgenden Ressourcen:</span><span class="sxs-lookup"><span data-stu-id="2f940-513">For more information, see the following resources:</span></span>

* [<span data-ttu-id="2f940-514">Application Insights-Übersicht</span><span class="sxs-lookup"><span data-stu-id="2f940-514">Application Insights overview</span></span>](/azure/application-insights/app-insights-overview)
* <span data-ttu-id="2f940-515">[Anwendungseinblicke für ASP.NET Core-Anwendungen](/azure/azure-monitor/app/asp-net-core): Beginnen Sie hier, wenn Sie die gesamte Bandbreite der Application Insights-Telemetrie zusammen mit der Protokollierung implementieren möchten.</span><span class="sxs-lookup"><span data-stu-id="2f940-515">[Application Insights for ASP.NET Core applications](/azure/azure-monitor/app/asp-net-core) - Start here if you want to implement the full range of Application Insights telemetry along with logging.</span></span>
* <span data-ttu-id="2f940-516">[ApplicationInsightsLoggerProvider für .NET Core ILogger-Protokolle](/azure/azure-monitor/app/ilogger): Beginnen Sie hier, wenn Sie den Protokollierungsanbieter ohne die übrige Application Insights-Telemetrie implementieren möchten.</span><span class="sxs-lookup"><span data-stu-id="2f940-516">[ApplicationInsightsLoggerProvider for .NET Core ILogger logs](/azure/azure-monitor/app/ilogger) - Start here if you want to implement the logging provider without the rest of Application Insights telemetry.</span></span>
* <span data-ttu-id="2f940-517">[Application Insights logging adapters (Adapter zur Protokollierung für Application Insights)](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-trace-logs).</span><span class="sxs-lookup"><span data-stu-id="2f940-517">[Application Insights logging adapters](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-trace-logs).</span></span>
* <span data-ttu-id="2f940-518">[Installieren, Konfigurieren und Initialisieren des Application Insights SDK](/learn/modules/instrument-web-app-code-with-application-insights): interaktive Tutorials auf der Microsoft Learn-Website.</span><span class="sxs-lookup"><span data-stu-id="2f940-518">[Install, configure, and initialize the Application Insights SDK](/learn/modules/instrument-web-app-code-with-application-insights) - Interactive tutorial on the Microsoft Learn site.</span></span>

## <a name="third-party-logging-providers"></a><span data-ttu-id="2f940-519">Protokollierungsanbieter von Drittanbietern</span><span class="sxs-lookup"><span data-stu-id="2f940-519">Third-party logging providers</span></span>

<span data-ttu-id="2f940-520">Protokollierungsframeworks von Drittanbietern aufgeführt, die mit ASP.NET Core funktionieren:</span><span class="sxs-lookup"><span data-stu-id="2f940-520">Third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="2f940-521">[elmah.io](https://elmah.io/) ([GitHub-Repository](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="2f940-521">[elmah.io](https://elmah.io/) ([GitHub repo](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span></span>
* <span data-ttu-id="2f940-522">[Gelf](https://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub-Repository](https://github.com/mattwcole/gelf-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="2f940-522">[Gelf](https://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub repo](https://github.com/mattwcole/gelf-extensions-logging))</span></span>
* <span data-ttu-id="2f940-523">[JSNLog](https://jsnlog.com/) ([GitHub-Repository](https://github.com/mperdeck/jsnlog))</span><span class="sxs-lookup"><span data-stu-id="2f940-523">[JSNLog](https://jsnlog.com/) ([GitHub repo](https://github.com/mperdeck/jsnlog))</span></span>
* <span data-ttu-id="2f940-524">[KissLog.net](https://kisslog.net/) ([GitHub repo](https://github.com/catalingavan/KissLog-net))</span><span class="sxs-lookup"><span data-stu-id="2f940-524">[KissLog.net](https://kisslog.net/) ([GitHub repo](https://github.com/catalingavan/KissLog-net))</span></span>
* <span data-ttu-id="2f940-525">[Log4Net](https://logging.apache.org/log4net/) ([GitHub-Repository](https://github.com/huorswords/Microsoft.Extensions.Logging.Log4Net.AspNetCore))</span><span class="sxs-lookup"><span data-stu-id="2f940-525">[Log4Net](https://logging.apache.org/log4net/) ([GitHub repo](https://github.com/huorswords/Microsoft.Extensions.Logging.Log4Net.AspNetCore))</span></span>
* <span data-ttu-id="2f940-526">[Loggr](https://loggr.net/) ([GitHub-Repository](https://github.com/imobile3/Loggr.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="2f940-526">[Loggr](https://loggr.net/) ([GitHub repo](https://github.com/imobile3/Loggr.Extensions.Logging))</span></span>
* <span data-ttu-id="2f940-527">[NLog](https://nlog-project.org/) ([GitHub-Repository](https://github.com/NLog/NLog.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="2f940-527">[NLog](https://nlog-project.org/) ([GitHub repo](https://github.com/NLog/NLog.Extensions.Logging))</span></span>
* <span data-ttu-id="2f940-528">[PLogger](https://www.nuget.org/packages/InvertedSoftware.PLogger.Core/) ([GitHub-Repository](https://github.com/invertedsoftware/InvertedSoftware.PLogger.Core))</span><span class="sxs-lookup"><span data-stu-id="2f940-528">[PLogger](https://www.nuget.org/packages/InvertedSoftware.PLogger.Core/) ([GitHub repo](https://github.com/invertedsoftware/InvertedSoftware.PLogger.Core))</span></span>
* <span data-ttu-id="2f940-529">[Sentry](https://sentry.io/welcome/) ([GitHub-Repository](https://github.com/getsentry/sentry-dotnet))</span><span class="sxs-lookup"><span data-stu-id="2f940-529">[Sentry](https://sentry.io/welcome/) ([GitHub repo](https://github.com/getsentry/sentry-dotnet))</span></span>
* <span data-ttu-id="2f940-530">[Serilog](https://serilog.net/) ([GitHub-Repository](https://github.com/serilog/serilog-aspnetcore))</span><span class="sxs-lookup"><span data-stu-id="2f940-530">[Serilog](https://serilog.net/) ([GitHub repo](https://github.com/serilog/serilog-aspnetcore))</span></span>
* <span data-ttu-id="2f940-531">[Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([GitHub-Repository](https://github.com/googleapis/google-cloud-dotnet))</span><span class="sxs-lookup"><span data-stu-id="2f940-531">[Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([Github repo](https://github.com/googleapis/google-cloud-dotnet))</span></span>

<span data-ttu-id="2f940-532">Einige Drittanbieterframeworks können eine [semantische Protokollierung (auch als strukturierte Protokollierung bezeichnet)](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging) ausführen.</span><span class="sxs-lookup"><span data-stu-id="2f940-532">Some third-party frameworks can perform [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="2f940-533">Die Verwendung eines Frameworks von Drittanbietern ist ähnlich wie die Verwendung eines integrierten Anbieters:</span><span class="sxs-lookup"><span data-stu-id="2f940-533">Using a third-party framework is similar to using one of the built-in providers:</span></span>

1. <span data-ttu-id="2f940-534">Fügen Sie Ihrem Paket ein NuGet-Paket hinzu.</span><span class="sxs-lookup"><span data-stu-id="2f940-534">Add a NuGet package to your project.</span></span>
1. <span data-ttu-id="2f940-535">Rufen Sie eine `ILoggerFactory`-Erweiterungsmethode auf, die vom Protokollierungsframework bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="2f940-535">Call an `ILoggerFactory` extension method provided by the logging framework.</span></span>

<span data-ttu-id="2f940-536">Weitere Informationen finden Sie in der Dokumentation zum jeweiligen Anbieter.</span><span class="sxs-lookup"><span data-stu-id="2f940-536">For more information, see each provider's documentation.</span></span> <span data-ttu-id="2f940-537">Protokollierungsanbieter von Drittanbietern werden von Microsoft nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="2f940-537">Third-party logging providers aren't supported by Microsoft.</span></span>

<a name="nhca"></a>

## <a name="non-host-console-app"></a><span data-ttu-id="2f940-538">Nicht-Host-Konsolen-App</span><span class="sxs-lookup"><span data-stu-id="2f940-538">Non-host console app</span></span>

<span data-ttu-id="2f940-539">Ein Beispiel zum Verwenden des generischen Hosts in einer nicht webbasierten Konsolen-App finden Sie in der *Program.cs*-Datei der [Beispiel-App für Hintergrundaufgaben](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples) (<xref:fundamentals/host/hosted-services>).</span><span class="sxs-lookup"><span data-stu-id="2f940-539">For an example of how to use the Generic Host in a non-web console app, see the *Program.cs* file of the [Background Tasks sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples) (<xref:fundamentals/host/hosted-services>).</span></span>

<span data-ttu-id="2f940-540">Das Protokollieren von Code für Apps ohne generischen Host weicht in Bezug auf das [Hinzufügen von Anbietern](#add-providers) und das [Erstellen von Protokollierungen](#create-logs) ab.</span><span class="sxs-lookup"><span data-stu-id="2f940-540">Logging code for apps without Generic Host differs in the way [providers are added](#add-providers) and [loggers are created](#create-logs).</span></span> 

### <a name="logging-providers"></a><span data-ttu-id="2f940-541">Protokollierungsanbieter</span><span class="sxs-lookup"><span data-stu-id="2f940-541">Logging providers</span></span>

<span data-ttu-id="2f940-542">Rufen Sie in einer Konsolen-App ohne Host die `Add{provider name}`-Erweiterungsmethode des Anbieters auf, während Sie eine `LoggerFactory` erstellen:</span><span class="sxs-lookup"><span data-stu-id="2f940-542">In a non-host console app, call the provider's `Add{provider name}` extension method while creating a `LoggerFactory`:</span></span>

[!code-csharp[](index/samples/3.x/LoggingConsoleApp/Program.cs?name=snippet_LoggerFactory&highlight=11-12)]

### <a name="create-logs"></a><span data-ttu-id="2f940-543">Erstellen von Protokollen</span><span class="sxs-lookup"><span data-stu-id="2f940-543">Create logs</span></span>

<span data-ttu-id="2f940-544">Verwenden Sie zum Erstellen von Protokollen ein <xref:Microsoft.Extensions.Logging.ILogger%601>-Objekt.</span><span class="sxs-lookup"><span data-stu-id="2f940-544">To create logs, use an <xref:Microsoft.Extensions.Logging.ILogger%601> object.</span></span> <span data-ttu-id="2f940-545">Verwenden Sie `LoggerFactory`, um ein `ILogger`-Element zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="2f940-545">Use the `LoggerFactory` to create an `ILogger`.</span></span>

<span data-ttu-id="2f940-546">Im folgenden Beispiel wird eine Protokollierung mit `LoggingConsoleApp.Program` als Kategorie erstellt.</span><span class="sxs-lookup"><span data-stu-id="2f940-546">The following example creates a logger with `LoggingConsoleApp.Program` as the category.</span></span>

[!code-csharp[](index/samples/3.x/LoggingConsoleApp/Program.cs?name=snippet_LoggerFactory&highlight=14)]

<span data-ttu-id="2f940-547">In den folgenden ASP.NET Core-Beispielen werden von der Protokollierung Protokolle mit dem Protokolliergrad `Information` erstellt.</span><span class="sxs-lookup"><span data-stu-id="2f940-547">In the following ASP.NET CORE examples, the logger is used to create logs with `Information` as the level.</span></span> <span data-ttu-id="2f940-548">Der Protokollierungs*grad* gibt den Schweregrad des protokollierten Ereignisses an.</span><span class="sxs-lookup"><span data-stu-id="2f940-548">The Log *level* indicates the severity of the logged event.</span></span>

[!code-csharp[](index/samples/3.x/LoggingConsoleApp/Program.cs?name=snippet_LoggerFactory&highlight=15)]

<span data-ttu-id="2f940-549">[Grade](#log-level) und [Kategorien](#log-category) werden in diesem Dokument ausführlicher erläutert.</span><span class="sxs-lookup"><span data-stu-id="2f940-549">[Levels](#log-level) and [categories](#log-category) are explained in more detail in this document.</span></span>

<a name="lhc"></a>

## <a name="log-during-host-construction"></a><span data-ttu-id="2f940-550">Protokollierung während der Hosterstellung</span><span class="sxs-lookup"><span data-stu-id="2f940-550">Log during host construction</span></span>

<span data-ttu-id="2f940-551">Die Protokollierung während der Hosterstellung wird nicht direkt unterstützt.</span><span class="sxs-lookup"><span data-stu-id="2f940-551">Logging during host construction isn't directly supported.</span></span> <span data-ttu-id="2f940-552">Es kann jedoch eine separate Protokollierung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="2f940-552">However, a separate logger can be used.</span></span> <span data-ttu-id="2f940-553">Im folgenden Beispiel wird eine [Serilog](https://serilog.net/)-Protokollierung zum Anmelden von `CreateHostBuilder` verwendet.</span><span class="sxs-lookup"><span data-stu-id="2f940-553">In the following example, a [Serilog](https://serilog.net/) logger is used to log in `CreateHostBuilder`.</span></span> <span data-ttu-id="2f940-554">`AddSerilog` verwendet die in `Log.Logger` angegebene statische Konfiguration:</span><span class="sxs-lookup"><span data-stu-id="2f940-554">`AddSerilog` uses the static configuration specified in `Log.Logger`:</span></span>

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

## <a name="configure-a-service-that-depends-on-ilogger"></a><span data-ttu-id="2f940-555">Konfigurieren eines Diensts, der von ILogger abhängig ist</span><span class="sxs-lookup"><span data-stu-id="2f940-555">Configure a service that depends on ILogger</span></span>

<span data-ttu-id="2f940-556">Die Konstruktorinjektion einer Protokollierung in `Startup` funktioniert in früheren Versionen von ASP.NET Core, da für den Webhost ein separater Abhängigkeitsinjektion-Container erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="2f940-556">Constructor injection of a logger into `Startup` works in earlier versions of ASP.NET Core because a separate DI container is created for the Web Host.</span></span> <span data-ttu-id="2f940-557">Weitere Informationen dazu, warum nur ein Container für den generischen Host erstellt wird, erhalten Sie in der [Breaking Change-Ankündigung](https://github.com/aspnet/Announcements/issues/353).</span><span class="sxs-lookup"><span data-stu-id="2f940-557">For information about why only one container is created for the Generic Host, see the [breaking change announcement](https://github.com/aspnet/Announcements/issues/353).</span></span>

<span data-ttu-id="2f940-558">Um einen Dienst zu konfigurieren, der von `ILogger<T>` abhängt, verwenden Sie die Konstruktorinjektion oder stellen eine Factorymethode bereit.</span><span class="sxs-lookup"><span data-stu-id="2f940-558">To configure a service that depends on `ILogger<T>`, use constructor injection or provide a factory method.</span></span> <span data-ttu-id="2f940-559">Die Nutzung einer Factorymethode wird nur empfohlen, wenn Sie keine andere Wahl haben.</span><span class="sxs-lookup"><span data-stu-id="2f940-559">The factory method approach is recommended only if there is no other option.</span></span> <span data-ttu-id="2f940-560">Angenommen, ein Dienst benötigt z. B. eine durch Abhängigkeitsinjektion bereitgestellte `ILogger<T>`-Instanz:</span><span class="sxs-lookup"><span data-stu-id="2f940-560">For example, consider a service that needs an `ILogger<T>` instance provided by DI:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Startup2.cs?name=snippet_ConfigureServices&highlight=6-10)]

<span data-ttu-id="2f940-561">Der hervorgehobene Code oben ist eine [Func](/dotnet/api/system.func-2), die ausgeführt wird, wenn der Abhängigkeitsinjektionscontainer erstmals eine Instanz von `MyService` erstellen muss.</span><span class="sxs-lookup"><span data-stu-id="2f940-561">The preceding highlighted code is a [Func](/dotnet/api/system.func-2) that runs the first time the DI container needs to construct an instance of `MyService`.</span></span> <span data-ttu-id="2f940-562">Auf diese Weise können Sie auf alle registrierten Dienste zugreifen.</span><span class="sxs-lookup"><span data-stu-id="2f940-562">You can access any of the registered services in this way.</span></span>

<a name="clms"></a>

## <a name="create-logs-in-main"></a><span data-ttu-id="2f940-563">Erstellen von Protokollen in Main</span><span class="sxs-lookup"><span data-stu-id="2f940-563">Create logs in Main</span></span>

<span data-ttu-id="2f940-564">Der folgende Code führt Protokollierung in `Main` aus, indem nach dem Erstellen des Hosts eine `ILogger`-Instanz von der Abhängigkeitsinjektion abgerufen wird:</span><span class="sxs-lookup"><span data-stu-id="2f940-564">The following code logs in `Main` by getting an `ILogger` instance from DI after building the host:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiDTO/Program.cs?name=snippet_LogProgram)]

### <a name="create-logs-in-startup"></a><span data-ttu-id="2f940-565">Erstellen von Protokollen in der Startklasse</span><span class="sxs-lookup"><span data-stu-id="2f940-565">Create logs in Startup</span></span>

<span data-ttu-id="2f940-566">Der folgende Code schreibt Protokolle in `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="2f940-566">The following code writes logs in `Startup.Configure`:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiDTO/Startup.cs?name=snippet_Configure)]

<span data-ttu-id="2f940-567">Das Schreiben von Protokollen vor Abschluss der Einrichtung des Abhängigkeitsinjektion-Containers in der `Startup.ConfigureServices`-Methode wird nicht unterstützt:</span><span class="sxs-lookup"><span data-stu-id="2f940-567">Writing logs before completion of the DI container setup in the `Startup.ConfigureServices` method is not supported:</span></span>

* <span data-ttu-id="2f940-568">Die Protokollierungsinjektion in den `Startup`-Konstruktor wird nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="2f940-568">Logger injection into the `Startup` constructor is not supported.</span></span>
* <span data-ttu-id="2f940-569">Die Protokollierungsinjektion in die `Startup.ConfigureServices`-Methodensignatur wird nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="2f940-569">Logger injection into the `Startup.ConfigureServices` method signature is not supported</span></span>

<span data-ttu-id="2f940-570">Dies Einschränkung ist darauf zurückzuführen, dass die Protokollierung von der Abhängigkeitsinjektion und der Konfiguration abhängt (die wiederum von der Abhängigkeitsinjektion abhängt).</span><span class="sxs-lookup"><span data-stu-id="2f940-570">The reason for this restriction is that logging depends on DI and on configuration, which in turns depends on DI.</span></span> <span data-ttu-id="2f940-571">Der Abhängigkeitsinjektion-Container wird erst nach Fertigstellung von `ConfigureServices` eingerichtet.</span><span class="sxs-lookup"><span data-stu-id="2f940-571">The DI container isn't set up until `ConfigureServices` finishes.</span></span>

<span data-ttu-id="2f940-572">Informationen zum Konfigurieren eines Diensts, der von `ILogger<T>` abhängt, oder dazu, warum die Konstruktorinjektion einer Protokollierung in `Startup` in früheren Versionen funktioniert hat, finden Sie unter [Konfigurieren eines Diensts, der von ILogger abhängt](#csdi).</span><span class="sxs-lookup"><span data-stu-id="2f940-572">For information on configuring a service that depends on `ILogger<T>` or why constructor injection of a logger into `Startup` worked in earlier versions, see [Configure a service that depends on ILogger](#csdi)</span></span>

### <a name="no-asynchronous-logger-methods"></a><span data-ttu-id="2f940-573">Keine asynchronen Protokollierungsmethoden</span><span class="sxs-lookup"><span data-stu-id="2f940-573">No asynchronous logger methods</span></span>

<span data-ttu-id="2f940-574">Die Protokollierung sollte so schnell erfolgen, dass sich die Leistungskosten von asynchronem Code nicht lohnen.</span><span class="sxs-lookup"><span data-stu-id="2f940-574">Logging should be so fast that it isn't worth the performance cost of asynchronous code.</span></span> <span data-ttu-id="2f940-575">Wenn ein Protokollierungsdatenspeicher langsam ist, schreiben Sie nicht direkt in diesen.</span><span class="sxs-lookup"><span data-stu-id="2f940-575">If a logging data store is slow, don't write to it directly.</span></span> <span data-ttu-id="2f940-576">Erwägen Sie, die Protokollnachrichten zunächst in einen schnellen Speicher zu schreiben und sie dann später in den langsamen Speicher zu verschieben.</span><span class="sxs-lookup"><span data-stu-id="2f940-576">Consider writing the log messages to a fast store initially, then moving them to the slow store later.</span></span> <span data-ttu-id="2f940-577">Wenn Sie beispielsweise in SQL Server protokollieren, verwenden Sie nicht direkt eine `Log`-Methode, da die `Log`-Methoden synchron sind.</span><span class="sxs-lookup"><span data-stu-id="2f940-577">For example, when logging to SQL Server, don't do so directly in a `Log` method, since the `Log` methods are synchronous.</span></span> <span data-ttu-id="2f940-578">Fügen Sie stattdessen die Protokollmeldungen synchron zu einer Warteschlange im Arbeitsspeicher hinzu, und rufen Sie mithilfe eines Hintergrundworkers die Nachrichten aus der Warteschlange ab, um sie dann asynchron per Pushvorgang an SQL Server zu übertragen.</span><span class="sxs-lookup"><span data-stu-id="2f940-578">Instead, synchronously add log messages to an in-memory queue and have a background worker pull the messages out of the queue to do the asynchronous work of pushing data to SQL Server.</span></span> <span data-ttu-id="2f940-579">Weitere Informationen finden Sie in [diesem GitHub-Issue](https://github.com/dotnet/AspNetCore.Docs/issues/11801).</span><span class="sxs-lookup"><span data-stu-id="2f940-579">For more information, see [this](https://github.com/dotnet/AspNetCore.Docs/issues/11801) GitHub issue.</span></span>

<a name="clib"></a>

## <a name="change-log-levels-in-a-running-app"></a><span data-ttu-id="2f940-580">Ändern von Protokolliergraden in einer aktuell ausgeführten App</span><span class="sxs-lookup"><span data-stu-id="2f940-580">Change log levels in a running app</span></span>

<span data-ttu-id="2f940-581">Die Protokollierungs-API umfasst kein Szenario zum Ändern der Protokollebene, während eine App ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="2f940-581">The Logging API doesn't include a scenario to change log levels while an app is running.</span></span> <span data-ttu-id="2f940-582">Einige Konfigurationsanbieter können jedoch die Konfiguration erneut laden, was sich unmittelbar auf die Protokollierungskonfiguration auswirkt.</span><span class="sxs-lookup"><span data-stu-id="2f940-582">However, some configuration providers are capable of reloading configuration, which takes immediate effect on logging configuration.</span></span> <span data-ttu-id="2f940-583">Beispielsweise lädt der [Dateikonfigurationsanbieter](xref:fundamentals/configuration/index#file-configuration-provider) die Protokollierungskonfiguration standardmäßig erneut.</span><span class="sxs-lookup"><span data-stu-id="2f940-583">For example, the [File Configuration Provider](xref:fundamentals/configuration/index#file-configuration-provider), reloads logging configuration by default.</span></span> <span data-ttu-id="2f940-584">Wenn die Konfiguration im Code geändert wird, während eine App ausgeführt wird, kann die App [IConfigurationRoot.Reload](xref:Microsoft.Extensions.Configuration.IConfigurationRoot.Reload*) aufrufen, um ihre Protokollierungskonfiguration zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="2f940-584">If configuration is changed in code while an app is running, the app can call [IConfigurationRoot.Reload](xref:Microsoft.Extensions.Configuration.IConfigurationRoot.Reload*) to update the app's logging configuration.</span></span>

## <a name="ilogger-and-iloggerfactory"></a><span data-ttu-id="2f940-585">ILogger und ILoggerFactory</span><span class="sxs-lookup"><span data-stu-id="2f940-585">ILogger and ILoggerFactory</span></span>

<span data-ttu-id="2f940-586">Die <xref:Microsoft.Extensions.Logging.ILogger%601>- und <xref:Microsoft.Extensions.Logging.ILoggerFactory>-Schnittstellen und -Implementierungen sind im .NET Core SDK enthalten.</span><span class="sxs-lookup"><span data-stu-id="2f940-586">The <xref:Microsoft.Extensions.Logging.ILogger%601> and <xref:Microsoft.Extensions.Logging.ILoggerFactory> interfaces and implementations are included in the .NET Core SDK.</span></span> <span data-ttu-id="2f940-587">Sie sind auch in den folgenden NuGet-Paketen verfügbar:</span><span class="sxs-lookup"><span data-stu-id="2f940-587">They are also available in the following NuGet packages:</span></span>  

* <span data-ttu-id="2f940-588">Die Schnittstellen befinden sich in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/).</span><span class="sxs-lookup"><span data-stu-id="2f940-588">The interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/).</span></span>
* <span data-ttu-id="2f940-589">Die Standardimplementierungen befinden sich in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span><span class="sxs-lookup"><span data-stu-id="2f940-589">The default implementations are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span></span>

<!-- review. Why would you want to hard code filtering rules in code? -->
<a name="fric"></a>

## <a name="apply-log-filter-rules-in-code"></a><span data-ttu-id="2f940-590">Anwenden von Protokollfilterregeln in Code</span><span class="sxs-lookup"><span data-stu-id="2f940-590">Apply log filter rules in code</span></span>

<span data-ttu-id="2f940-591">Die bevorzugte Vorgehensweise zum Festlegen von Protokollfilterregeln ist die Verwendung von [Konfiguration](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="2f940-591">The preferred approach for setting log filter rules is by using [Configuration](xref:fundamentals/configuration/index).</span></span>

<span data-ttu-id="2f940-592">Das folgende Beispiel zeigt, wie Filterregeln im Code registriert werden:</span><span class="sxs-lookup"><span data-stu-id="2f940-592">The following example shows how to register filter rules in code:</span></span>

[!code-csharp[](index/samples/3.x/MyMain/Program.cs?name=snippet_FilterInCode)]

<span data-ttu-id="2f940-593">`logging.AddFilter("System", LogLevel.Debug)` gibt die `System`-Kategorie und den Protokolliergrad `Debug` an.</span><span class="sxs-lookup"><span data-stu-id="2f940-593">`logging.AddFilter("System", LogLevel.Debug)` specifies the `System` category and log level `Debug`.</span></span> <span data-ttu-id="2f940-594">Der Filter wird auf alle Anbieter angewendet, da kein bestimmter Anbieter konfiguriert wurde.</span><span class="sxs-lookup"><span data-stu-id="2f940-594">The filter is applied to all providers because a specific provider was not configured.</span></span>

<span data-ttu-id="2f940-595">`AddFilter<DebugLoggerProvider>("Microsoft", LogLevel.Information)` gibt Folgendes an:</span><span class="sxs-lookup"><span data-stu-id="2f940-595">`AddFilter<DebugLoggerProvider>("Microsoft", LogLevel.Information)` specifies:</span></span>

* <span data-ttu-id="2f940-596">Den `Debug`-Protokollanbieter.</span><span class="sxs-lookup"><span data-stu-id="2f940-596">The `Debug` logging provider.</span></span>
* <span data-ttu-id="2f940-597">Der Protokolliergrad `Information` oder höher.</span><span class="sxs-lookup"><span data-stu-id="2f940-597">Log level `Information` and higher.</span></span>
* <span data-ttu-id="2f940-598">Alle Kategorien, die mit `"Microsoft"` beginnen.</span><span class="sxs-lookup"><span data-stu-id="2f940-598">All categories starting with `"Microsoft"`.</span></span>

## <a name="create-a-custom-logger"></a><span data-ttu-id="2f940-599">Erstellen einer benutzerdefinierten Protokollierung</span><span class="sxs-lookup"><span data-stu-id="2f940-599">Create a custom logger</span></span>

<span data-ttu-id="2f940-600">Fügen Sie einen <xref:Microsoft.Extensions.Logging.ILoggerProvider> mit einer <xref:Microsoft.Extensions.Logging.ILoggerFactory> hinzu, um eine benutzerdefinierte Protokollierung hinzuzufügen:</span><span class="sxs-lookup"><span data-stu-id="2f940-600">To add a custom logger, add an <xref:Microsoft.Extensions.Logging.ILoggerProvider> with <xref:Microsoft.Extensions.Logging.ILoggerFactory>:</span></span>

```csharp
public void Configure(
    IApplicationBuilder app,
    IWebHostEnvironment env,
    ILoggerFactory loggerFactory)
{
    loggerFactory.AddProvider(new CustomLoggerProvider(new CustomLoggerConfiguration()));
```

<span data-ttu-id="2f940-601">`ILoggerProvider` erstellt mindestens eine `ILogger`-Instanz.</span><span class="sxs-lookup"><span data-stu-id="2f940-601">The `ILoggerProvider` creates one or more `ILogger` instances.</span></span> <span data-ttu-id="2f940-602">Die `ILogger`-Instanzen werden vom Framework zum Protokollieren der Informationen verwendet.</span><span class="sxs-lookup"><span data-stu-id="2f940-602">The `ILogger` instances are used by the framework to log the information.</span></span>

### <a name="sample-custom-logger-configuration"></a><span data-ttu-id="2f940-603">Beispielkonfiguration für benutzerdefinierte Protokollierung</span><span class="sxs-lookup"><span data-stu-id="2f940-603">Sample custom logger configuration</span></span>

<span data-ttu-id="2f940-604">Im Beispiel geschieht Folgendes:</span><span class="sxs-lookup"><span data-stu-id="2f940-604">The sample:</span></span>

* <span data-ttu-id="2f940-605">Es ist ein sehr einfaches Beispiel, mit dem die Farbe der Protokollkonsole nach Ereignis-ID und Protokolliergrad festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="2f940-605">Is designed to be a very basic sample that sets the color of the log console by event ID and log level.</span></span> <span data-ttu-id="2f940-606">Protokollierungen ändern sich in der Regel nicht anhand der Ereignis-ID und sind nicht spezifisch für den Protokolliergrad.</span><span class="sxs-lookup"><span data-stu-id="2f940-606">Loggers generally don't change by event ID and are not specific to log level.</span></span>
* <span data-ttu-id="2f940-607">Es werden unterschiedliche Farbkonsoleneinträge pro Protokolliergrad und Ereignis-ID mithilfe des folgenden Konfigurationstyps erstellt:</span><span class="sxs-lookup"><span data-stu-id="2f940-607">Creates different color console entries per log level and event ID using the following configuration type:</span></span>

[!code-csharp[](index/samples/3.x/CustomLogger/ColorConsoleLogger/ColorConsoleLoggerConfiguration.cs?name=snippet)]

<span data-ttu-id="2f940-608">Der vorangehende Code legt den Standardprotokolliergrad auf `Warning` und die Farbe auf `Yellow` fest.</span><span class="sxs-lookup"><span data-stu-id="2f940-608">The preceding code sets the default level to `Warning` and the color to `Yellow`.</span></span> <span data-ttu-id="2f940-609">Wenn die `EventId` auf 0 festgelegt ist, werden alle Ereignisse protokolliert.</span><span class="sxs-lookup"><span data-stu-id="2f940-609">If the `EventId` is set to 0, we will log all events.</span></span>

### <a name="create-the-custom-logger"></a><span data-ttu-id="2f940-610">Erstellen der benutzerdefinierten Protokollierung</span><span class="sxs-lookup"><span data-stu-id="2f940-610">Create the custom logger</span></span>

<span data-ttu-id="2f940-611">Der Kategoriename der `ILogger`-Implementierung ist in der Regel die Protokollierungsquelle.</span><span class="sxs-lookup"><span data-stu-id="2f940-611">The `ILogger` implementation category name is typically the logging source.</span></span> <span data-ttu-id="2f940-612">Beispielsweise der Typ, in dem die Protokollierung erstellt wird:</span><span class="sxs-lookup"><span data-stu-id="2f940-612">For example, the type where the logger is created:</span></span>

[!code-csharp[](index/samples/3.x/CustomLogger/ColorConsoleLogger/ColorConsoleLogger.cs?name=snippet)]

<span data-ttu-id="2f940-613">Der vorangehende Code:</span><span class="sxs-lookup"><span data-stu-id="2f940-613">The preceding code:</span></span>

* <span data-ttu-id="2f940-614">Erstellt eine Protokollierungsinstanz pro Kategoriename.</span><span class="sxs-lookup"><span data-stu-id="2f940-614">Creates a logger instance per category name.</span></span>
* <span data-ttu-id="2f940-615">Überprüft `logLevel == _config.LogLevel` in `IsEnabled`, sodass jedes `logLevel`-Element über eine eindeutige Protokollierung verfügt.</span><span class="sxs-lookup"><span data-stu-id="2f940-615">Checks `logLevel == _config.LogLevel` in `IsEnabled`, so each `logLevel` has a unique logger.</span></span> <span data-ttu-id="2f940-616">Protokollierungen sollten im Allgemeinen auch für alle höheren Protokolliergrade aktiviert werden:</span><span class="sxs-lookup"><span data-stu-id="2f940-616">Generally, loggers should also be enabled for all higher log levels:</span></span>

```csharp
public bool IsEnabled(LogLevel logLevel)
{
    return logLevel >= _config.LogLevel;
}
```

### <a name="create-the-custom-loggerprovider"></a><span data-ttu-id="2f940-617">Erstellen des benutzerdefinierten LoggerProvider</span><span class="sxs-lookup"><span data-stu-id="2f940-617">Create the custom LoggerProvider</span></span>

<span data-ttu-id="2f940-618">`LoggerProvider` ist die Klasse, mit der die Protokollierungsinstanzen erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="2f940-618">The `LoggerProvider` is the class that creates the logger instances.</span></span> <span data-ttu-id="2f940-619">Möglicherweise ist sie nicht erforderlich, um eine Protokollierungsinstanz pro Kategorie zu erstellen. Dies ist jedoch für einige Protokollierungen sinnvoll, etwa für NLog oder log4net.</span><span class="sxs-lookup"><span data-stu-id="2f940-619">Maybe it is not needed to create a logger instance per category, but this makes sense for some Loggers, like NLog or log4net.</span></span> <span data-ttu-id="2f940-620">Auf diese Weise können Sie bei Bedarf auch verschiedene Protokollierungsausgabeziele pro Kategorie auswählen:</span><span class="sxs-lookup"><span data-stu-id="2f940-620">Doing this you are also able to choose different logging output targets per category if needed:</span></span>

[!code-csharp[](index/samples/3.x/CustomLogger/ColorConsoleLogger/ColorConsoleLoggerProvider.cs?name=snippet)]

<span data-ttu-id="2f940-621">Im Code oben erstellt <xref:Microsoft.Build.Logging.LoggerDescription.CreateLogger*> eine einzelne Instanz von `ColorConsoleLogger` pro Kategoriename und speichert sie in [`ConcurrentDictionary<TKey,TValue>`](/dotnet/api/system.collections.concurrent.concurrentdictionary-2);</span><span class="sxs-lookup"><span data-stu-id="2f940-621">In the preceding code, <xref:Microsoft.Build.Logging.LoggerDescription.CreateLogger*> creates a single instance of the `ColorConsoleLogger` per category name and stores it in the [`ConcurrentDictionary<TKey,TValue>`](/dotnet/api/system.collections.concurrent.concurrentdictionary-2);</span></span>

### <a name="usage-and-registration-of-the-custom-logger"></a><span data-ttu-id="2f940-622">Verwendung und Registrierung der benutzerdefinierten Protokollierung</span><span class="sxs-lookup"><span data-stu-id="2f940-622">Usage and registration of the custom logger</span></span>

<span data-ttu-id="2f940-623">Registrieren Sie die Protokollierung in `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="2f940-623">Register the logger in the `Startup.Configure`:</span></span>

[!code-csharp[](index/samples/3.x/CustomLogger/Startup.cs?name=snippet)]

<span data-ttu-id="2f940-624">Geben Sie für den Code oben mindestens eine Erweiterungsmethode für `ILoggerFactory` an:</span><span class="sxs-lookup"><span data-stu-id="2f940-624">For the preceding code, provide at least one extension method for the `ILoggerFactory`:</span></span>

[!code-csharp[](index/samples/3.x/CustomLogger/ColorConsoleLogger/ColorConsoleLoggerExtensions.cs?name=snippet)]

## <a name="additional-resources"></a><span data-ttu-id="2f940-625">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="2f940-625">Additional resources</span></span>

* <xref:fundamentals/logging/loggermessage>
* <span data-ttu-id="2f940-626">Fehler beim Protokollieren sollten im Repository [github.com/dotnet/runtime/](https://github.com/dotnet/runtime/issues) erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="2f940-626">Logging bugs should be created in the [github.com/dotnet/runtime/](https://github.com/dotnet/runtime/issues) repo.</span></span>
* <xref:blazor/fundamentals/logging>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="2f940-627">Von [Tom Dykstra](https://github.com/tdykstra) und [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="2f940-627">By [Tom Dykstra](https://github.com/tdykstra) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="2f940-628">.NET Core unterstützt eine Protokollierungs-API, die mit einer Vielzahl von integrierten Protokollierungsanbietern und Drittanbieter-Protokollierungslösungen zusammenarbeitet.</span><span class="sxs-lookup"><span data-stu-id="2f940-628">.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="2f940-629">Dieser Artikel zeigt, wie Sie die Protokollierungs-API mit integrierten Anbietern verwenden können.</span><span class="sxs-lookup"><span data-stu-id="2f940-629">This article shows how to use the logging API with built-in providers.</span></span>

<span data-ttu-id="2f940-630">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([Vorgehensweise zum Herunterladen](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2f940-630">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="add-providers"></a><span data-ttu-id="2f940-631">Hinzufügen von Anbietern</span><span class="sxs-lookup"><span data-stu-id="2f940-631">Add providers</span></span>

<span data-ttu-id="2f940-632">Ein Protokollierungsanbieter zeigt Protokolle an oder speichert diese.</span><span class="sxs-lookup"><span data-stu-id="2f940-632">A logging provider displays or stores logs.</span></span> <span data-ttu-id="2f940-633">Beispielsweise zeigt der Konsolenanbieter Protokolle in der Konsole an, und der Azure Application Insights-Anbieter speichert sie in Azure Application Insights.</span><span class="sxs-lookup"><span data-stu-id="2f940-633">For example, the Console provider displays logs on the console, and the Azure Application Insights provider stores them in Azure Application Insights.</span></span> <span data-ttu-id="2f940-634">Protokolle können durch das Hinzufügen mehrerer Anbieter an mehrere Ziele gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="2f940-634">Logs can be sent to multiple destinations by adding multiple providers.</span></span>

<span data-ttu-id="2f940-635">Um einen Anbieter hinzuzufügen, rufen Sie die `Add{provider name}`-Erweiterungsmethode des Anbieters in *Program.cs* auf:</span><span class="sxs-lookup"><span data-stu-id="2f940-635">To add a provider, call the provider's `Add{provider name}` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=18-20)]

<span data-ttu-id="2f940-636">Der vorstehende Code erfordert Verweise auf `Microsoft.Extensions.Logging` und `Microsoft.Extensions.Configuration`.</span><span class="sxs-lookup"><span data-stu-id="2f940-636">The preceding code requires references to `Microsoft.Extensions.Logging` and `Microsoft.Extensions.Configuration`.</span></span>

<span data-ttu-id="2f940-637">Die Standardprojektvorlage ruft die Methode <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A> auf, die die folgenden Protokollierungsanbieter hinzufügt:</span><span class="sxs-lookup"><span data-stu-id="2f940-637">The default project template calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>, which adds the following logging providers:</span></span>

* <span data-ttu-id="2f940-638">Konsole</span><span class="sxs-lookup"><span data-stu-id="2f940-638">Console</span></span>
* <span data-ttu-id="2f940-639">Debug</span><span class="sxs-lookup"><span data-stu-id="2f940-639">Debug</span></span>
* <span data-ttu-id="2f940-640">EventSource (beginnend mit ASP.NET Core 2.2)</span><span class="sxs-lookup"><span data-stu-id="2f940-640">EventSource (starting in ASP.NET Core 2.2)</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_TemplateCode&highlight=7)]

<span data-ttu-id="2f940-641">Bei Verwendung von `CreateDefaultBuilder` können Sie die Standardanbieter durch Ihre eigene Auswahl ersetzen.</span><span class="sxs-lookup"><span data-stu-id="2f940-641">If you use `CreateDefaultBuilder`, you can replace the default providers with your own choices.</span></span> <span data-ttu-id="2f940-642">Rufen Sie <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A> auf, und fügen Sie die gewünschten Anbieter hinzu.</span><span class="sxs-lookup"><span data-stu-id="2f940-642">Call <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A>, and add the providers you want.</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=18-22)]

<span data-ttu-id="2f940-643">Informationen zu den [integrierten Protokollierungsanbietern](#built-in-logging-providers) sowie zu [Protokollierungsanbietern von Drittanbietern](#third-party-logging-providers) finden Sie weiter unten in diesem Artikel.</span><span class="sxs-lookup"><span data-stu-id="2f940-643">Learn more about [built-in logging providers](#built-in-logging-providers) and [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="create-logs"></a><span data-ttu-id="2f940-644">Erstellen von Protokollen</span><span class="sxs-lookup"><span data-stu-id="2f940-644">Create logs</span></span>

<span data-ttu-id="2f940-645">Verwenden Sie zum Erstellen von Protokollen ein <xref:Microsoft.Extensions.Logging.ILogger%601>-Objekt.</span><span class="sxs-lookup"><span data-stu-id="2f940-645">To create logs, use an <xref:Microsoft.Extensions.Logging.ILogger%601> object.</span></span> <span data-ttu-id="2f940-646">Rufen Sie in einer Web-App oder einem gehosteten Dienst einen `ILogger` aus der Abhängigkeitsinjektion ab.</span><span class="sxs-lookup"><span data-stu-id="2f940-646">In a web app or hosted service, get an `ILogger` from dependency injection (DI).</span></span> <span data-ttu-id="2f940-647">Verwenden Sie in Konsolen-Apps ohne Host `LoggerFactory`, um einen `ILogger` zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="2f940-647">In non-host console apps, use the `LoggerFactory` to create an `ILogger`.</span></span>

<span data-ttu-id="2f940-648">Im folgenden ASP.NET Core-Beispiel wird eine Protokollierung mit `TodoApiSample.Pages.AboutModel` als Kategorie erstellt.</span><span class="sxs-lookup"><span data-stu-id="2f940-648">The following ASP.NET Core example creates a logger with `TodoApiSample.Pages.AboutModel` as the category.</span></span> <span data-ttu-id="2f940-649">Die Protokoll*kategorie* ist eine Zeichenfolge, die jedem Protokoll zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="2f940-649">The log *category* is a string that is associated with each log.</span></span> <span data-ttu-id="2f940-650">Die von der Abhängigkeitsinjektion bereitgestellte `ILogger<T>`-Instanz erstellt Protokolle, die den vollqualifizierten Namen vom Typ `T` als Kategorie aufweisen.</span><span class="sxs-lookup"><span data-stu-id="2f940-650">The `ILogger<T>` instance provided by DI creates logs that have the fully qualified name of type `T` as the category.</span></span> 

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_LoggerDI&highlight=3,5,7)]

<span data-ttu-id="2f940-651">In den folgenden Beispielen zu ASP.NET Core und einer Konsolen-App werden von der Protokollierung Protokolle mit dem Protokolliergrad `Information` erstellt.</span><span class="sxs-lookup"><span data-stu-id="2f940-651">In the following ASP.NET Core and console app examples, the logger is used to create logs with `Information` as the level.</span></span> <span data-ttu-id="2f940-652">Der Protokollierungs*grad* gibt den Schweregrad des protokollierten Ereignisses an.</span><span class="sxs-lookup"><span data-stu-id="2f940-652">The Log *level* indicates the severity of the logged event.</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_CallLogMethods&highlight=4)]

<span data-ttu-id="2f940-653">[Grade](#log-level) und [Kategorien](#log-category) werden weiter unten in diesem Artikel ausführlicher erläutert.</span><span class="sxs-lookup"><span data-stu-id="2f940-653">[Levels](#log-level) and [categories](#log-category) are explained in more detail later in this article.</span></span>

### <a name="create-logs-in-startup"></a><span data-ttu-id="2f940-654">Erstellen von Protokollen in der Startklasse</span><span class="sxs-lookup"><span data-stu-id="2f940-654">Create logs in Startup</span></span>

<span data-ttu-id="2f940-655">Zum Schreiben von Protokollen in der `Startup`-Klasse schließen Sie einen `ILogger`-Parameter in die Konstruktorsignatur ein:</span><span class="sxs-lookup"><span data-stu-id="2f940-655">To write logs in the `Startup` class, include an `ILogger` parameter in the constructor signature:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Startup.cs?name=snippet_Startup&highlight=3,5,8,20,27)]

### <a name="create-logs-in-the-program-class"></a><span data-ttu-id="2f940-656">Erstellen von Protokollen in der Program-Klasse</span><span class="sxs-lookup"><span data-stu-id="2f940-656">Create logs in the Program class</span></span>

<span data-ttu-id="2f940-657">Zum Schreiben von Protokollen in der `Program`-Klasse rufen Sie eine `ILogger`-Instanz aus DI ab:</span><span class="sxs-lookup"><span data-stu-id="2f940-657">To write logs in the `Program` class, get an `ILogger` instance from DI:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=9,10)]

<span data-ttu-id="2f940-658">Die Protokollierung während der Hosterstellung wird nicht direkt unterstützt.</span><span class="sxs-lookup"><span data-stu-id="2f940-658">Logging during host construction isn't directly supported.</span></span> <span data-ttu-id="2f940-659">Es kann jedoch eine separate Protokollierung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="2f940-659">However, a separate logger can be used.</span></span> <span data-ttu-id="2f940-660">Im folgenden Beispiel wird eine [Serilog](https://serilog.net/)-Protokollierung zum Anmelden von `CreateWebHostBuilder` verwendet.</span><span class="sxs-lookup"><span data-stu-id="2f940-660">In the following example, a [Serilog](https://serilog.net/) logger is used to log in `CreateWebHostBuilder`.</span></span> <span data-ttu-id="2f940-661">`AddSerilog` verwendet die in `Log.Logger` angegebene statische Konfiguration:</span><span class="sxs-lookup"><span data-stu-id="2f940-661">`AddSerilog` uses the static configuration specified in `Log.Logger`:</span></span>

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

### <a name="no-asynchronous-logger-methods"></a><span data-ttu-id="2f940-662">Keine asynchronen Protokollierungsmethoden</span><span class="sxs-lookup"><span data-stu-id="2f940-662">No asynchronous logger methods</span></span>

<span data-ttu-id="2f940-663">Die Protokollierung sollte so schnell erfolgen, dass sich die Leistungskosten von asynchronem Code nicht lohnen.</span><span class="sxs-lookup"><span data-stu-id="2f940-663">Logging should be so fast that it isn't worth the performance cost of asynchronous code.</span></span> <span data-ttu-id="2f940-664">Wenn Ihr Protokollierungsdatenspeicher langsam ist, schreiben Sie nicht direkt in ihn.</span><span class="sxs-lookup"><span data-stu-id="2f940-664">If your logging data store is slow, don't write to it directly.</span></span> <span data-ttu-id="2f940-665">Erwägen Sie, die Protokollnachrichten zunächst in einen schnellen Speicher zu schreiben und sie dann später in den langsamen Speicher zu verschieben.</span><span class="sxs-lookup"><span data-stu-id="2f940-665">Consider writing the log messages to a fast store initially, then move them to the slow store later.</span></span> <span data-ttu-id="2f940-666">Wenn Sie beispielsweise in SQL Server protokollieren, möchten Sie dies nicht direkt in einer `Log`-Methode tun, da die `Log`-Methoden synchron sind.</span><span class="sxs-lookup"><span data-stu-id="2f940-666">For example, if you're logging to SQL Server, you don't want to do that directly in a `Log` method, since the `Log` methods are synchronous.</span></span> <span data-ttu-id="2f940-667">Fügen Sie stattdessen die Protokollmeldungen synchron zu einer Warteschlange im Arbeitsspeicher hinzu, und rufen Sie mithilfe eines Hintergrundworkers die Nachrichten aus der Warteschlange ab, um sie dann asynchron per Pushvorgang an SQL Server zu übertragen.</span><span class="sxs-lookup"><span data-stu-id="2f940-667">Instead, synchronously add log messages to an in-memory queue and have a background worker pull the messages out of the queue to do the asynchronous work of pushing data to SQL Server.</span></span> <span data-ttu-id="2f940-668">Weitere Informationen finden Sie in [diesem GitHub-Issue](https://github.com/dotnet/AspNetCore.Docs/issues/11801).</span><span class="sxs-lookup"><span data-stu-id="2f940-668">For more information, see [this](https://github.com/dotnet/AspNetCore.Docs/issues/11801) GitHub issue.</span></span>

## <a name="configuration"></a><span data-ttu-id="2f940-669">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="2f940-669">Configuration</span></span>

<span data-ttu-id="2f940-670">Die Konfiguration von Protokollierungsanbietern erfolgt durch mindestens einen Konfigurationsanbieter:</span><span class="sxs-lookup"><span data-stu-id="2f940-670">Logging provider configuration is provided by one or more configuration providers:</span></span>

* <span data-ttu-id="2f940-671">Dateiformate (INI, JSON und XML)</span><span class="sxs-lookup"><span data-stu-id="2f940-671">File formats (INI, JSON, and XML).</span></span>
* <span data-ttu-id="2f940-672">Befehlszeilenargumenten</span><span class="sxs-lookup"><span data-stu-id="2f940-672">Command-line arguments.</span></span>
* <span data-ttu-id="2f940-673">Umgebungsvariablen.</span><span class="sxs-lookup"><span data-stu-id="2f940-673">Environment variables.</span></span>
* <span data-ttu-id="2f940-674">Speicherinterne .NET-Objekte</span><span class="sxs-lookup"><span data-stu-id="2f940-674">In-memory .NET objects.</span></span>
* <span data-ttu-id="2f940-675">Der unverschlüsselte Speicher von [Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="2f940-675">The unencrypted [Secret Manager](xref:security/app-secrets) storage.</span></span>
* <span data-ttu-id="2f940-676">Ein unverschlüsselter Benutzerspeicher, z.B. [Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="2f940-676">An encrypted user store, such as [Azure Key Vault](xref:security/key-vault-configuration).</span></span>
* <span data-ttu-id="2f940-677">Benutzerdefinierte Anbieter (installiert oder erstellt).</span><span class="sxs-lookup"><span data-stu-id="2f940-677">Custom providers (installed or created).</span></span>

<span data-ttu-id="2f940-678">Die Konfiguration der Protokollierung wird meist vom `Logging`-Abschnitt von Anwendungseinstellungsdateien angegeben.</span><span class="sxs-lookup"><span data-stu-id="2f940-678">For example, logging configuration is commonly provided by the `Logging` section of app settings files.</span></span> <span data-ttu-id="2f940-679">Im folgenden Beispiel werden die Inhalte einer herkömmlichen *appsettings.Development.json*-Datei veranschaulicht:</span><span class="sxs-lookup"><span data-stu-id="2f940-679">The following example shows the contents of a typical *appsettings.Development.json* file:</span></span>

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

<span data-ttu-id="2f940-680">Die `Logging`-Eigenschaft kann den `LogLevel` und Protokollanbietereigenschaften beinhalten (Konsole wird angezeigt).</span><span class="sxs-lookup"><span data-stu-id="2f940-680">The `Logging` property can have `LogLevel` and log provider properties (Console is shown).</span></span>

<span data-ttu-id="2f940-681">Die `LogLevel`-Eigenschaft unter `Logging` gibt den Mindest[grad](#log-level) an, der für ausgewählte Kategorien protokolliert werden soll.</span><span class="sxs-lookup"><span data-stu-id="2f940-681">The `LogLevel` property under `Logging` specifies the minimum [level](#log-level) to log for selected categories.</span></span> <span data-ttu-id="2f940-682">Im Beispiel protokollieren die Kategorien `System` und `Microsoft` mit dem Grad `Information` und alle anderen Kategorien mit dem Grad `Debug`.</span><span class="sxs-lookup"><span data-stu-id="2f940-682">In the example, `System` and `Microsoft` categories log at `Information` level, and all others log at `Debug` level.</span></span>

<span data-ttu-id="2f940-683">Weitere Eigenschaften unter `Logging` geben Protokollierungsanbieter an.</span><span class="sxs-lookup"><span data-stu-id="2f940-683">Other properties under `Logging` specify logging providers.</span></span> <span data-ttu-id="2f940-684">Das Beispiel bezieht sich auf den Konsolenanbieter.</span><span class="sxs-lookup"><span data-stu-id="2f940-684">The example is for the Console provider.</span></span> <span data-ttu-id="2f940-685">Wenn ein Anbieter [Protokollbereiche](#log-scopes) unterstützt, gibt `IncludeScopes` an, ob sie aktiviert sind.</span><span class="sxs-lookup"><span data-stu-id="2f940-685">If a provider supports [log scopes](#log-scopes), `IncludeScopes` indicates whether they're enabled.</span></span> <span data-ttu-id="2f940-686">Eine Anbietereigenschaft (z.B. `Console` im Beispiel) kann auch eine `LogLevel`-Eigenschaft angeben.</span><span class="sxs-lookup"><span data-stu-id="2f940-686">A provider property (such as `Console` in the example) may also specify a `LogLevel` property.</span></span> <span data-ttu-id="2f940-687">`LogLevel` unter einem Anbieter gibt die Grade an, die für diesen Anbieter protokolliert werden sollen.</span><span class="sxs-lookup"><span data-stu-id="2f940-687">`LogLevel` under a provider specifies levels to log for that provider.</span></span>

<span data-ttu-id="2f940-688">Wenn Grade in `Logging.{providername}.LogLevel` angegeben werden, überschreiben sie alle Angaben in `Logging.LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="2f940-688">If levels are specified in `Logging.{providername}.LogLevel`, they override anything set in `Logging.LogLevel`.</span></span> <span data-ttu-id="2f940-689">Betrachten Sie beispielsweise den folgenden JSON-Code:</span><span class="sxs-lookup"><span data-stu-id="2f940-689">For example, consider the following JSON:</span></span>

[!code-json[](index/samples/3.x/TodoApiDTO/appsettings.MSFT.json)]

<span data-ttu-id="2f940-690">Im JSON-Code oben wird der vorherige Protokolliergrad (Standardprotokolliergrad) durch die `Console`-Anbietereinstellungen überschrieben.</span><span class="sxs-lookup"><span data-stu-id="2f940-690">In the preceding JSON, the `Console` provider settings overrides the preceding (default) log level.</span></span>

<span data-ttu-id="2f940-691">Die Protokollierungs-API umfasst kein Szenario zum Ändern der Protokollebene, während eine App ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="2f940-691">The Logging API doesn't include a scenario to change log levels while an app is running.</span></span> <span data-ttu-id="2f940-692">Einige Konfigurationsanbieter können jedoch die Konfiguration erneut laden, was sich unmittelbar auf die Protokollierungskonfiguration auswirkt.</span><span class="sxs-lookup"><span data-stu-id="2f940-692">However, some configuration providers are capable of reloading configuration, which takes immediate effect on logging configuration.</span></span> <span data-ttu-id="2f940-693">Beispielsweise lädt der [Dateikonfigurationsanbieter](xref:fundamentals/configuration/index#file-configuration-provider), der durch `CreateDefaultBuilder` zum Lesen von Einstellungsdateien hinzugefügt wird, die Protokollierungskonfiguration standardmäßig erneut.</span><span class="sxs-lookup"><span data-stu-id="2f940-693">For example, the [File Configuration Provider](xref:fundamentals/configuration/index#file-configuration-provider), which is added by `CreateDefaultBuilder` to read settings files, reloads logging configuration by default.</span></span> <span data-ttu-id="2f940-694">Wenn die Konfiguration im Code geändert wird, während eine App ausgeführt wird, kann die App [IConfigurationRoot.Reload](xref:Microsoft.Extensions.Configuration.IConfigurationRoot.Reload*) aufrufen, um ihre Protokollierungskonfiguration zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="2f940-694">If configuration is changed in code while an app is running, the app can call [IConfigurationRoot.Reload](xref:Microsoft.Extensions.Configuration.IConfigurationRoot.Reload*) to update the app's logging configuration.</span></span>

<span data-ttu-id="2f940-695">Informationen zur Implementierung von Konfigurationsanbieter finden Sie hier: <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="2f940-695">For information on implementing configuration providers, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="sample-logging-output"></a><span data-ttu-id="2f940-696">Beispiel einer Protokollierungsausgabe</span><span class="sxs-lookup"><span data-stu-id="2f940-696">Sample logging output</span></span>

<span data-ttu-id="2f940-697">Mit dem im vorherigen Abschnitt gezeigten Beispielcode werden Protokolle in der Konsole angezeigt, wenn die Anwendung über die Befehlszeile ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="2f940-697">With the sample code shown in the preceding section, logs appear in the console when the app is run from the command line.</span></span> <span data-ttu-id="2f940-698">Hier ein Beispiel für eine Konsolenausgabe:</span><span class="sxs-lookup"><span data-stu-id="2f940-698">Here's an example of console output:</span></span>

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

<span data-ttu-id="2f940-699">Die vorherigen Protokolle wurden generiert, indem eine HTTP Get-Anforderung an die Beispiel-App unter `http://localhost:5000/api/todo/0` vorgenommen wurde.</span><span class="sxs-lookup"><span data-stu-id="2f940-699">The preceding logs were generated by making an HTTP Get request to the sample app at `http://localhost:5000/api/todo/0`.</span></span>

<span data-ttu-id="2f940-700">Nachfolgend sehen Sie, wie die gleichen Protokolle im Debugfenster angezeigt werden, wenn Sie die Beispiel-App in Visual Studio ausführen:</span><span class="sxs-lookup"><span data-stu-id="2f940-700">Here's an example of the same logs as they appear in the Debug window when you run the sample app in Visual Studio:</span></span>

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404
```

<span data-ttu-id="2f940-701">Die über die `ILogger`-Aufrufe aus dem vorherigen Abschnitt erstellten Protokolle beginnen mit „TodoApi“.</span><span class="sxs-lookup"><span data-stu-id="2f940-701">The logs that are created by the `ILogger` calls shown in the preceding section begin with "TodoApi".</span></span> <span data-ttu-id="2f940-702">Protokolle, die mit Microsoft-Kategorien beginnen, stammen aus ASP.NET Core-Frameworkcode.</span><span class="sxs-lookup"><span data-stu-id="2f940-702">The logs that begin with "Microsoft" categories are from ASP.NET Core framework code.</span></span> <span data-ttu-id="2f940-703">ASP.NET Core und Anwendungscode verwenden dieselbe Protokollierungs-API und dieselben Protokollierungsanbieter.</span><span class="sxs-lookup"><span data-stu-id="2f940-703">ASP.NET Core and application code are using the same logging API and providers.</span></span>

<span data-ttu-id="2f940-704">In den übrigen Abschnitten dieses Artikels werden einige Details und Optionen für die Protokollierung erläutert.</span><span class="sxs-lookup"><span data-stu-id="2f940-704">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="2f940-705">NuGet-Pakete</span><span class="sxs-lookup"><span data-stu-id="2f940-705">NuGet packages</span></span>

<span data-ttu-id="2f940-706">Die Schnittstellen `ILogger` und `ILoggerFactory` befinden sich in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), ihre Standardimplementierungen sind in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/) enthalten.</span><span class="sxs-lookup"><span data-stu-id="2f940-706">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="2f940-707">Protokollkategorie</span><span class="sxs-lookup"><span data-stu-id="2f940-707">Log category</span></span>

<span data-ttu-id="2f940-708">Wenn ein `ILogger`-Objekt erstellt wird, wird eine *Kategorie* für dieses Objekt angegeben.</span><span class="sxs-lookup"><span data-stu-id="2f940-708">When an `ILogger` object is created, a *category* is specified for it.</span></span> <span data-ttu-id="2f940-709">Diese Kategorie ist in jeder Protokollmeldung enthalten, die von dieser Instanz von `ILogger` erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="2f940-709">That category is included with each log message created by that instance of `ILogger`.</span></span> <span data-ttu-id="2f940-710">Die Kategorie kann eine beliebige Zeichenfolge sein, aber die Konvention besteht in der Verwendung des Klassennamens, z.B. „TodoApi.Controllers.TodoController“.</span><span class="sxs-lookup"><span data-stu-id="2f940-710">The category may be any string, but the convention is to use the class name, such as "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="2f940-711">Verwenden Sie `ILogger<T>` zum Abrufen einer `ILogger`-Instanz, die den vollqualifizierten Typnamen von `T` als Kategorie verwendet:</span><span class="sxs-lookup"><span data-stu-id="2f940-711">Use `ILogger<T>` to get an `ILogger` instance that uses the fully qualified type name of `T` as the category:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="2f940-712">Um die Kategorie explizit anzugeben, rufen Sie `ILoggerFactory.CreateLogger` auf:</span><span class="sxs-lookup"><span data-stu-id="2f940-712">To explicitly specify the category, call `ILoggerFactory.CreateLogger`:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

<span data-ttu-id="2f940-713">`ILogger<T>` entspricht dem Aufruf von `CreateLogger` mit dem vollqualifizierten Typnamen `T`.</span><span class="sxs-lookup"><span data-stu-id="2f940-713">`ILogger<T>` is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="2f940-714">Protokolliergrad</span><span class="sxs-lookup"><span data-stu-id="2f940-714">Log level</span></span>

<span data-ttu-id="2f940-715">Jedes Protokoll gibt einen <xref:Microsoft.Extensions.Logging.LogLevel>-Wert an.</span><span class="sxs-lookup"><span data-stu-id="2f940-715">Every log specifies a <xref:Microsoft.Extensions.Logging.LogLevel> value.</span></span> <span data-ttu-id="2f940-716">Der Protokolliergrad gibt den Schweregrad oder die Wichtigkeit an.</span><span class="sxs-lookup"><span data-stu-id="2f940-716">The log level indicates the severity or importance.</span></span> <span data-ttu-id="2f940-717">Sie können beispielsweise ein `Information`-Protokoll schreiben, wenn eine Methode normal endet, und ein `Warning`-Protokoll, wenn eine Methode den Statuscode *404 Not Found* zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="2f940-717">For example, you might write an `Information` log when a method ends normally and a `Warning` log when a method returns a *404 Not Found* status code.</span></span>

<span data-ttu-id="2f940-718">Der folgende Code erstellt `Information`- und `Warning`-Protokolle:</span><span class="sxs-lookup"><span data-stu-id="2f940-718">The following code creates `Information` and `Warning` logs:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="2f940-719">Im Code oben sind die Parameter `MyLogEvents.GetItem` und `MyLogEvents.GetItemNotFound` die [Protokollereignis-ID](#log-event-id).</span><span class="sxs-lookup"><span data-stu-id="2f940-719">In the preceding code, the `MyLogEvents.GetItem` and `MyLogEvents.GetItemNotFound` parameters are the [Log event ID](#log-event-id).</span></span> <span data-ttu-id="2f940-720">Der zweite Parameter ist eine Meldungsvorlage mit Platzhaltern für Argumentwerte, die von den verbleibenden Methodenparametern bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="2f940-720">The second parameter is a message template with placeholders for argument values provided by the remaining method parameters.</span></span> <span data-ttu-id="2f940-721">Die Methodenparameter werden im [Abschnitt „Protokollmeldungsvorlage“](#lmt) in diesem Artikel erläutert.</span><span class="sxs-lookup"><span data-stu-id="2f940-721">The method parameters are explained in the [Log message template section](#lmt) in this article.</span></span>

<span data-ttu-id="2f940-722">Protokollmethoden, die den Protokolliergrad im Methodennamen enthalten (z.B. `LogInformation` und `LogWarning`), sind [Erweiterungsmethoden für ILogger](xref:Microsoft.Extensions.Logging.LoggerExtensions).</span><span class="sxs-lookup"><span data-stu-id="2f940-722">Log methods that include the level in the method name (for example, `LogInformation` and `LogWarning`) are [extension methods for ILogger](xref:Microsoft.Extensions.Logging.LoggerExtensions).</span></span> <span data-ttu-id="2f940-723">Diese Methoden rufen eine `Log`-Methode auf, die einen `LogLevel`-Parameter annimmt.</span><span class="sxs-lookup"><span data-stu-id="2f940-723">These methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="2f940-724">Sie können anstelle eines Aufrufs dieser Erweiterungsmethoden die `Log`-Methode direkt aufrufen, aber die Syntax ist relativ kompliziert.</span><span class="sxs-lookup"><span data-stu-id="2f940-724">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="2f940-725">Weitere Informationen finden Sie unter <xref:Microsoft.Extensions.Logging.ILogger> und im [Quellcode für Protokollierungserweiterungen](https://github.com/dotnet/extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="2f940-725">For more information, see <xref:Microsoft.Extensions.Logging.ILogger> and the [logger extensions source code](https://github.com/dotnet/extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs).</span></span>

<span data-ttu-id="2f940-726">ASP.NET Core definiert die folgenden Protokolliergrade. Die Reihenfolge reicht vom geringsten bis zum höchsten Schweregrad.</span><span class="sxs-lookup"><span data-stu-id="2f940-726">ASP.NET Core defines the following log levels, ordered here from lowest to highest severity.</span></span>

* <span data-ttu-id="2f940-727">Trace = 0</span><span class="sxs-lookup"><span data-stu-id="2f940-727">Trace = 0</span></span>

  <span data-ttu-id="2f940-728">Protokolliert Informationen, die in der Regel nur für das Debuggen nützlich sind.</span><span class="sxs-lookup"><span data-stu-id="2f940-728">For information that's typically valuable only for debugging.</span></span> <span data-ttu-id="2f940-729">Diese Meldungen können sensible Anwendungsdaten enthalten und sollten daher nicht in einer Produktionsumgebung aktiviert werden.</span><span class="sxs-lookup"><span data-stu-id="2f940-729">These messages may contain sensitive application data and so shouldn't be enabled in a production environment.</span></span> <span data-ttu-id="2f940-730">*Standardmäßig deaktiviert.*</span><span class="sxs-lookup"><span data-stu-id="2f940-730">*Disabled by default.*</span></span>

* <span data-ttu-id="2f940-731">Debug = 1</span><span class="sxs-lookup"><span data-stu-id="2f940-731">Debug = 1</span></span>

  <span data-ttu-id="2f940-732">Protokolliert Informationen, die bei der Entwicklung und beim Debuggen hilfreich sein können.</span><span class="sxs-lookup"><span data-stu-id="2f940-732">For information that may be useful in development and debugging.</span></span> <span data-ttu-id="2f940-733">Beispiel: `Entering method Configure with flag set to true.` Aktivieren Sie Protokolle mit dem Protokolliergrad `Debug` aufgrund des hohen Protokollaufkommens nur zur Problembehandlung in der Produktion.</span><span class="sxs-lookup"><span data-stu-id="2f940-733">Example: `Entering method Configure with flag set to true.` Enable `Debug` level logs in production only when troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="2f940-734">Information = 2</span><span class="sxs-lookup"><span data-stu-id="2f940-734">Information = 2</span></span>

  <span data-ttu-id="2f940-735">Zur Nachverfolgung des allgemeinen Ablaufs der App.</span><span class="sxs-lookup"><span data-stu-id="2f940-735">For tracking the general flow of the app.</span></span> <span data-ttu-id="2f940-736">Diese Protokolle haben typischerweise einen langfristigen Nutzen.</span><span class="sxs-lookup"><span data-stu-id="2f940-736">These logs typically have some long-term value.</span></span> <span data-ttu-id="2f940-737">Ein Beispiel: `Request received for path /api/todo`</span><span class="sxs-lookup"><span data-stu-id="2f940-737">Example: `Request received for path /api/todo`</span></span>

* <span data-ttu-id="2f940-738">Warning = 3</span><span class="sxs-lookup"><span data-stu-id="2f940-738">Warning = 3</span></span>

  <span data-ttu-id="2f940-739">Für ungewöhnliche oder unerwartete Ereignisse im App-Verlauf.</span><span class="sxs-lookup"><span data-stu-id="2f940-739">For abnormal or unexpected events in the app flow.</span></span> <span data-ttu-id="2f940-740">Dazu können Fehler oder andere Bedingungen gehören, die zwar nicht zum Beenden der App führen, aber eventuell untersucht werden müssen.</span><span class="sxs-lookup"><span data-stu-id="2f940-740">These may include errors or other conditions that don't cause the app to stop but might need to be investigated.</span></span> <span data-ttu-id="2f940-741">Behandelte Ausnahmen sind eine typische Verwendung für den Protokolliergrad `Warning`.</span><span class="sxs-lookup"><span data-stu-id="2f940-741">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="2f940-742">Ein Beispiel: `FileNotFoundException for file quotes.txt.`</span><span class="sxs-lookup"><span data-stu-id="2f940-742">Example: `FileNotFoundException for file quotes.txt.`</span></span>

* <span data-ttu-id="2f940-743">Error = 4</span><span class="sxs-lookup"><span data-stu-id="2f940-743">Error = 4</span></span>

  <span data-ttu-id="2f940-744">Für Fehler und Ausnahmen, die nicht behandelt werden können.</span><span class="sxs-lookup"><span data-stu-id="2f940-744">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="2f940-745">Diese Meldungen weisen auf einen Fehler in der aktuellen Aktivität oder im aktuellen Vorgang (z.B. in der aktuellen HTTP-Anforderung) und nicht auf einen anwendungsweiten Fehler hin.</span><span class="sxs-lookup"><span data-stu-id="2f940-745">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an app-wide failure.</span></span> <span data-ttu-id="2f940-746">Beispielprotokollmeldung: `Cannot insert record due to duplicate key violation.`</span><span class="sxs-lookup"><span data-stu-id="2f940-746">Example log message: `Cannot insert record due to duplicate key violation.`</span></span>

* <span data-ttu-id="2f940-747">Critical = 5</span><span class="sxs-lookup"><span data-stu-id="2f940-747">Critical = 5</span></span>

  <span data-ttu-id="2f940-748">Für Fehler, die sofortige Aufmerksamkeit erfordern.</span><span class="sxs-lookup"><span data-stu-id="2f940-748">For failures that require immediate attention.</span></span> <span data-ttu-id="2f940-749">Beispiel: Szenarios mit Datenverlust, Speichermangel.</span><span class="sxs-lookup"><span data-stu-id="2f940-749">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="2f940-750">Verwenden Sie den Protokolliergrad, um die Menge an Protokollausgabedaten zu steuern, die in ein bestimmtes Speichermedium geschrieben oder an ein Anzeigefenster ausgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="2f940-750">Use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="2f940-751">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="2f940-751">For example:</span></span>

* <span data-ttu-id="2f940-752">In einer Produktionsumgebung</span><span class="sxs-lookup"><span data-stu-id="2f940-752">In production:</span></span>
  * <span data-ttu-id="2f940-753">Das Protokollieren von `Trace` über die `Information`-Ebenen verursacht viele detaillierte Protokollmeldungen.</span><span class="sxs-lookup"><span data-stu-id="2f940-753">Logging at the `Trace` through `Information` levels produces a high-volume of detailed log messages.</span></span> <span data-ttu-id="2f940-754">Protokollieren Sie `Trace` über Nachrichten auf `Information`-Ebene in einem kostengünstigen Speicher mit hohem Volumen, um die Kosten zu überwachen und die Grenzwerte des Datenspeichers nicht zu überschreiten.</span><span class="sxs-lookup"><span data-stu-id="2f940-754">To control costs and not exceed data storage limits, log `Trace` through `Information` level messages to a high-volume, low-cost data store.</span></span>
  * <span data-ttu-id="2f940-755">Das Protokollieren von `Warning` über die `Critical`-Ebenen verursacht weniger und kürzere Protokollmeldungen.</span><span class="sxs-lookup"><span data-stu-id="2f940-755">Logging at `Warning` through `Critical` levels typically produces fewer, smaller log messages.</span></span> <span data-ttu-id="2f940-756">Daher müssen Sie sich über Kosten- und Speichergrenzwerte keine Sorgen machen und sind bei der Auswahl des Datenspeichers flexibler.</span><span class="sxs-lookup"><span data-stu-id="2f940-756">Therefore, costs and storage limits usually aren't a concern, which results in greater flexibility of data store choice.</span></span>
* <span data-ttu-id="2f940-757">Entwicklungsphase:</span><span class="sxs-lookup"><span data-stu-id="2f940-757">During development:</span></span>
  * <span data-ttu-id="2f940-758">Protokollieren Sie `Warning` über `Critical`-Meldungen an der Konsole.</span><span class="sxs-lookup"><span data-stu-id="2f940-758">Log `Warning` through `Critical` messages to the console.</span></span>
  * <span data-ttu-id="2f940-759">Fügen Sie bei der Fehlerbehebung `Trace` über `Information`-Meldungen hinzu.</span><span class="sxs-lookup"><span data-stu-id="2f940-759">Add `Trace` through `Information` messages when troubleshooting.</span></span>

<span data-ttu-id="2f940-760">Im Abschnitt [Protokollfilterung](#log-filtering) weiter unten in diesem Artikel wird erläutert, wie Sie steuern, welche Protokolliergrade ein Anbieter verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="2f940-760">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="2f940-761">ASP.NET Core schreibt Protokolle für Frameworkereignisse.</span><span class="sxs-lookup"><span data-stu-id="2f940-761">ASP.NET Core writes logs for framework events.</span></span> <span data-ttu-id="2f940-762">Die zuvor in diesem Artikel gezeigten Protokollbeispiele enthielten Protokolle unterhalb des Protokolliergrads `Information`, deshalb wurden keine Protokolle des Protokolliergrads `Debug` oder `Trace` erstellt.</span><span class="sxs-lookup"><span data-stu-id="2f940-762">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` or `Trace` level logs were created.</span></span> <span data-ttu-id="2f940-763">Hier ist ein Beispiel für Konsolenprotokolle, die durch Ausführen der Beispiel-App generiert werden, die so konfiguriert ist, dass sie `Debug`-Protokolle anzeigt:</span><span class="sxs-lookup"><span data-stu-id="2f940-763">Here's an example of console logs produced by running the sample app configured to show `Debug` logs:</span></span>

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

## <a name="log-event-id"></a><span data-ttu-id="2f940-764">Protokollereignis-ID</span><span class="sxs-lookup"><span data-stu-id="2f940-764">Log event ID</span></span>

<span data-ttu-id="2f940-765">Jedes Protokoll kann eine *Ereignis-ID* angeben.</span><span class="sxs-lookup"><span data-stu-id="2f940-765">Each log can specify an *event ID*.</span></span> <span data-ttu-id="2f940-766">Die Beispiel-App verwendet hierzu eine lokal definierte `LoggingEvents`-Klasse:</span><span class="sxs-lookup"><span data-stu-id="2f940-766">The sample app does this by using a locally defined `LoggingEvents` class:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

<span data-ttu-id="2f940-767">Eine Ereignis-ID ordnet eine Gruppe von Ereignissen zu.</span><span class="sxs-lookup"><span data-stu-id="2f940-767">An event ID associates a set of events.</span></span> <span data-ttu-id="2f940-768">Beispielsweise können alle Protokolle, die sich auf die Anzeige einer Liste von Elementen auf einer Seite beziehen, 1001 sein.</span><span class="sxs-lookup"><span data-stu-id="2f940-768">For example, all logs related to displaying a list of items on a page might be 1001.</span></span>

<span data-ttu-id="2f940-769">Der Protokollierungsanbieter kann die Ereignis-ID in einem ID-Feld, in der Protokollierungsmeldung oder gar nicht speichern.</span><span class="sxs-lookup"><span data-stu-id="2f940-769">The logging provider may store the event ID in an ID field, in the logging message, or not at all.</span></span> <span data-ttu-id="2f940-770">Der Debuganbieter zeigt keine Ereignis-IDs an.</span><span class="sxs-lookup"><span data-stu-id="2f940-770">The Debug provider doesn't show event IDs.</span></span> <span data-ttu-id="2f940-771">Der Konsolenanbieter zeigt Ereignis-IDs in Klammern hinter der Kategorie an:</span><span class="sxs-lookup"><span data-stu-id="2f940-771">The console provider shows event IDs in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a><span data-ttu-id="2f940-772">Protokollmeldungsvorlage</span><span class="sxs-lookup"><span data-stu-id="2f940-772">Log message template</span></span>

<span data-ttu-id="2f940-773">Jedes Protokoll gibt eine Meldungsvorlage an.</span><span class="sxs-lookup"><span data-stu-id="2f940-773">Each log specifies a message template.</span></span> <span data-ttu-id="2f940-774">Die Meldungsvorlage kann Platzhalter enthalten, für die Argumente bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="2f940-774">The message template can contain placeholders for which arguments are provided.</span></span> <span data-ttu-id="2f940-775">Verwenden Sie Namen für die Platzhalter, keine Zahlen.</span><span class="sxs-lookup"><span data-stu-id="2f940-775">Use names for the placeholders, not numbers.</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="2f940-776">Die Reihenfolge der Platzhalter – nicht ihre Namen – bestimmt, welche Parameter verwendet werden, um ihre Werte zur Verfügung zu stellen.</span><span class="sxs-lookup"><span data-stu-id="2f940-776">The order of placeholders, not their names, determines which parameters are used to provide their values.</span></span> <span data-ttu-id="2f940-777">Beachten Sie im folgenden Code, dass die Parameternamen in der Meldungsvorlage nicht in der richtigen Reihenfolge vorhanden sind:</span><span class="sxs-lookup"><span data-stu-id="2f940-777">In the following code, notice that the parameter names are out of sequence in the message template:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="2f940-778">Dieser Code erstellt eine Protokollierungsmeldung mit den Parameterwerten in der richtigen Reihenfolge:</span><span class="sxs-lookup"><span data-stu-id="2f940-778">This code creates a log message with the parameter values in sequence:</span></span>

```text
Parameter values: parm1, parm2
```

<span data-ttu-id="2f940-779">Das Protokollierungsframework funktioniert auf diese Weise, damit Protokollierungsanbieter [semantische Protokollierung, die auch als strukturierte Protokollierung bezeichnet wird](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging), implementieren können.</span><span class="sxs-lookup"><span data-stu-id="2f940-779">The logging framework works this way so that logging providers can implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="2f940-780">Die Argumente selbst (nicht nur die formatierte Meldungsvorlage) werden an das Protokollierungssystem übergeben.</span><span class="sxs-lookup"><span data-stu-id="2f940-780">The arguments themselves are passed to the logging system, not just the formatted message template.</span></span> <span data-ttu-id="2f940-781">Diese Informationen ermöglichen es Protokollierungsanbietern, die Parameterwerte als Felder zu speichern.</span><span class="sxs-lookup"><span data-stu-id="2f940-781">This information enables logging providers to store the parameter values as fields.</span></span> <span data-ttu-id="2f940-782">Nehmen wir zum Beispiel an, dass Aufrufe von Protokollierungsmethoden folgendermaßen aussehen:</span><span class="sxs-lookup"><span data-stu-id="2f940-782">For example, suppose logger method calls look like this:</span></span>

```csharp
_logger.LogInformation("Getting item {Id} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="2f940-783">Wenn Sie die Protokolle an Azure Table Storage senden, kann jede Azure Table-Entität die Eigenschaften `ID` und `RequestTime` aufweisen. Dies vereinfacht die Abfrage von Protokolldaten.</span><span class="sxs-lookup"><span data-stu-id="2f940-783">If you're sending the logs to Azure Table Storage, each Azure Table entity can have `ID` and `RequestTime` properties, which simplifies queries on log data.</span></span> <span data-ttu-id="2f940-784">Eine Abfrage kann alle Protokolle in einem bestimmten `RequestTime`-Bereich finden, ohne die Uhrzeit aus den Textmeldungen zu analysieren.</span><span class="sxs-lookup"><span data-stu-id="2f940-784">A query can find all logs within a particular `RequestTime` range without parsing the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="2f940-785">Protokollieren von Ausnahmen</span><span class="sxs-lookup"><span data-stu-id="2f940-785">Logging exceptions</span></span>

<span data-ttu-id="2f940-786">Die Protokollierungsmethoden umfassen Überladungen, die das Übergeben von Ausnahmen ermöglichen, wie im folgenden Beispiel gezeigt:</span><span class="sxs-lookup"><span data-stu-id="2f940-786">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

<span data-ttu-id="2f940-787">Die verschiedenen Anbieter verarbeiten die Ausnahmeinformationen auf unterschiedliche Weise.</span><span class="sxs-lookup"><span data-stu-id="2f940-787">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="2f940-788">Hier sehen Sie ein Beispiel einer Debuganbieterausgabe aus dem obigen Codebeispiel.</span><span class="sxs-lookup"><span data-stu-id="2f940-788">Here's an example of Debug provider output from the code shown above.</span></span>

```text
TodoApiSample.Controllers.TodoController: Warning: GetById(55) NOT FOUND

System.Exception: Item not found exception.
   at TodoApiSample.Controllers.TodoController.GetById(String id) in C:\TodoApiSample\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="2f940-789">Protokollfilterung</span><span class="sxs-lookup"><span data-stu-id="2f940-789">Log filtering</span></span>

<span data-ttu-id="2f940-790">Sie können einen Mindestprotokolliergrad für einen bestimmten Anbieter und eine bestimmte Kategorie oder für alle Anbieter oder alle Kategorien festlegen.</span><span class="sxs-lookup"><span data-stu-id="2f940-790">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span> <span data-ttu-id="2f940-791">Alle Protokolle unter dem Mindestgrad werden nicht an diesen Anbieter weitergeleitet, sodass sie nicht angezeigt oder gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="2f940-791">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span>

<span data-ttu-id="2f940-792">Wenn Sie alle Protokolle unterdrücken möchten, geben Sie `LogLevel.None` als Mindestprotokolliergrad an.</span><span class="sxs-lookup"><span data-stu-id="2f940-792">To suppress all logs, specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="2f940-793">Der ganzzahlige Wert von `LogLevel.None` lautet 6 und liegt über `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="2f940-793">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

### <a name="create-filter-rules-in-configuration"></a><span data-ttu-id="2f940-794">Erstellen von Filterregeln in der Konfiguration</span><span class="sxs-lookup"><span data-stu-id="2f940-794">Create filter rules in configuration</span></span>

<span data-ttu-id="2f940-795">Der Projektvorlagencode ruft `CreateDefaultBuilder` auf, um die Protokollierung für die Konsolen-, Debug- und EventSource-Anbieter (ASP.NET Core 2.2 oder höher) einzurichten.</span><span class="sxs-lookup"><span data-stu-id="2f940-795">The project template code calls `CreateDefaultBuilder` to set up logging for the Console, Debug, and EventSource (ASP.NET Core 2.2 or later) providers.</span></span> <span data-ttu-id="2f940-796">Die `CreateDefaultBuilder`-Methode richtet die Protokollierung ein, um nach der Konfiguration in einem `Logging`-Abschnitt zu suchen; dies wird [weiter oben in diesem Artikel](#configuration) erläutert.</span><span class="sxs-lookup"><span data-stu-id="2f940-796">The `CreateDefaultBuilder` method sets up logging to look for configuration in a `Logging` section, as explained [earlier in this article](#configuration).</span></span>

<span data-ttu-id="2f940-797">Die Konfigurationsdaten geben die Mindestprotokolliergrade nach Anbieter und Kategorie an, wie im folgenden Beispiel gezeigt:</span><span class="sxs-lookup"><span data-stu-id="2f940-797">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

[!code-json[](index/samples/2.x/TodoApiSample/appsettings.json)]

<span data-ttu-id="2f940-798">Diese JSON-Konfiguration erstellt sechs Filterregeln: eine für den Debuganbieter, vier für den Konsolenanbieter und eine für alle Anbieter.</span><span class="sxs-lookup"><span data-stu-id="2f940-798">This JSON creates six filter rules: one for the Debug provider, four for the Console provider, and one for all providers.</span></span> <span data-ttu-id="2f940-799">Eine einzelne Regel wird für jeden Anbieter ausgewählt, wenn ein `ILogger`-Objekt erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="2f940-799">A single rule is chosen for each provider when an `ILogger` object is created.</span></span>

### <a name="filter-rules-in-code"></a><span data-ttu-id="2f940-800">Filterregeln im Code</span><span class="sxs-lookup"><span data-stu-id="2f940-800">Filter rules in code</span></span>

<span data-ttu-id="2f940-801">Das folgende Beispiel zeigt, wie Filterregeln im Code registriert werden:</span><span class="sxs-lookup"><span data-stu-id="2f940-801">The following example shows how to register filter rules in code:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

<span data-ttu-id="2f940-802">Das zweite `AddFilter` gibt den Debuganbieter durch Verwendung des zugehörigen Typnamens an.</span><span class="sxs-lookup"><span data-stu-id="2f940-802">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="2f940-803">Das erste `AddFilter` gilt für alle Anbieter, weil kein Anbietertyp angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="2f940-803">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

### <a name="how-filtering-rules-are-applied"></a><span data-ttu-id="2f940-804">Anwendung von Filterregeln</span><span class="sxs-lookup"><span data-stu-id="2f940-804">How filtering rules are applied</span></span>

<span data-ttu-id="2f940-805">Die Konfigurationsdaten und der in den vorangegangenen Beispielen gezeigte `AddFilter`-Code erzeugen die in der folgenden Tabelle gezeigten Regeln.</span><span class="sxs-lookup"><span data-stu-id="2f940-805">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="2f940-806">Die ersten sechs Regeln stammen aus dem Konfigurationsbeispiel, die letzten zwei Filter stammen aus dem Codebeispiel.</span><span class="sxs-lookup"><span data-stu-id="2f940-806">The first six come from the configuration example and the last two come from the code example.</span></span>

| <span data-ttu-id="2f940-807">Anzahl</span><span class="sxs-lookup"><span data-stu-id="2f940-807">Number</span></span> | <span data-ttu-id="2f940-808">Anbieter</span><span class="sxs-lookup"><span data-stu-id="2f940-808">Provider</span></span>      | <span data-ttu-id="2f940-809">Kategorien beginnend mit...</span><span class="sxs-lookup"><span data-stu-id="2f940-809">Categories that begin with ...</span></span>          | <span data-ttu-id="2f940-810">Mindestprotokolliergrad</span><span class="sxs-lookup"><span data-stu-id="2f940-810">Minimum log level</span></span> |
| :----: | ------------- | --------------------------------------- | ----------------- |
| <span data-ttu-id="2f940-811">1</span><span class="sxs-lookup"><span data-stu-id="2f940-811">1</span></span>      | <span data-ttu-id="2f940-812">Debug</span><span class="sxs-lookup"><span data-stu-id="2f940-812">Debug</span></span>         | <span data-ttu-id="2f940-813">Alle Kategorien</span><span class="sxs-lookup"><span data-stu-id="2f940-813">All categories</span></span>                          | <span data-ttu-id="2f940-814">Information</span><span class="sxs-lookup"><span data-stu-id="2f940-814">Information</span></span>       |
| <span data-ttu-id="2f940-815">2</span><span class="sxs-lookup"><span data-stu-id="2f940-815">2</span></span>      | <span data-ttu-id="2f940-816">Konsole</span><span class="sxs-lookup"><span data-stu-id="2f940-816">Console</span></span>       | <span data-ttu-id="2f940-817">Microsoft.AspNetCore.Mvc.Razor.Internal</span><span class="sxs-lookup"><span data-stu-id="2f940-817">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span> | <span data-ttu-id="2f940-818">Warnung</span><span class="sxs-lookup"><span data-stu-id="2f940-818">Warning</span></span>           |
| <span data-ttu-id="2f940-819">3</span><span class="sxs-lookup"><span data-stu-id="2f940-819">3</span></span>      | <span data-ttu-id="2f940-820">Konsole</span><span class="sxs-lookup"><span data-stu-id="2f940-820">Console</span></span>       | <span data-ttu-id="2f940-821">Microsoft.AspNetCore.Mvc.Razor.Razor</span><span class="sxs-lookup"><span data-stu-id="2f940-821">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>    | <span data-ttu-id="2f940-822">Debug</span><span class="sxs-lookup"><span data-stu-id="2f940-822">Debug</span></span>             |
| <span data-ttu-id="2f940-823">4</span><span class="sxs-lookup"><span data-stu-id="2f940-823">4</span></span>      | <span data-ttu-id="2f940-824">Konsole</span><span class="sxs-lookup"><span data-stu-id="2f940-824">Console</span></span>       | <span data-ttu-id="2f940-825">Microsoft.AspNetCore.Mvc.Razor</span><span class="sxs-lookup"><span data-stu-id="2f940-825">Microsoft.AspNetCore.Mvc.Razor</span></span>          | <span data-ttu-id="2f940-826">Fehler</span><span class="sxs-lookup"><span data-stu-id="2f940-826">Error</span></span>             |
| <span data-ttu-id="2f940-827">5</span><span class="sxs-lookup"><span data-stu-id="2f940-827">5</span></span>      | <span data-ttu-id="2f940-828">Konsole</span><span class="sxs-lookup"><span data-stu-id="2f940-828">Console</span></span>       | <span data-ttu-id="2f940-829">Alle Kategorien</span><span class="sxs-lookup"><span data-stu-id="2f940-829">All categories</span></span>                          | <span data-ttu-id="2f940-830">Information</span><span class="sxs-lookup"><span data-stu-id="2f940-830">Information</span></span>       |
| <span data-ttu-id="2f940-831">6</span><span class="sxs-lookup"><span data-stu-id="2f940-831">6</span></span>      | <span data-ttu-id="2f940-832">Alle Anbieter</span><span class="sxs-lookup"><span data-stu-id="2f940-832">All providers</span></span> | <span data-ttu-id="2f940-833">Alle Kategorien</span><span class="sxs-lookup"><span data-stu-id="2f940-833">All categories</span></span>                          | <span data-ttu-id="2f940-834">Debug</span><span class="sxs-lookup"><span data-stu-id="2f940-834">Debug</span></span>             |
| <span data-ttu-id="2f940-835">7</span><span class="sxs-lookup"><span data-stu-id="2f940-835">7</span></span>      | <span data-ttu-id="2f940-836">Alle Anbieter</span><span class="sxs-lookup"><span data-stu-id="2f940-836">All providers</span></span> | <span data-ttu-id="2f940-837">System</span><span class="sxs-lookup"><span data-stu-id="2f940-837">System</span></span>                                  | <span data-ttu-id="2f940-838">Debug</span><span class="sxs-lookup"><span data-stu-id="2f940-838">Debug</span></span>             |
| <span data-ttu-id="2f940-839">8</span><span class="sxs-lookup"><span data-stu-id="2f940-839">8</span></span>      | <span data-ttu-id="2f940-840">Debug</span><span class="sxs-lookup"><span data-stu-id="2f940-840">Debug</span></span>         | <span data-ttu-id="2f940-841">Microsoft</span><span class="sxs-lookup"><span data-stu-id="2f940-841">Microsoft</span></span>                               | <span data-ttu-id="2f940-842">Ablaufverfolgung</span><span class="sxs-lookup"><span data-stu-id="2f940-842">Trace</span></span>             |

<span data-ttu-id="2f940-843">Wenn ein `ILogger`-Objekt erstellt wird, wählt das `ILoggerFactory`-Objekt eine einzige Regel pro Anbieter aus, die auf diese Protokollierung angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="2f940-843">When an `ILogger` object is created, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="2f940-844">Alle von einer `ILogger`-Instanz geschriebenen Meldungen werden auf Grundlage der ausgewählten Regeln gefiltert.</span><span class="sxs-lookup"><span data-stu-id="2f940-844">All messages written by an `ILogger` instance are filtered based on the selected rules.</span></span> <span data-ttu-id="2f940-845">Aus den verfügbaren Regeln wird die für jeden Anbieter und jedes Kategoriepaar spezifischste Regel ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="2f940-845">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="2f940-846">Beim Erstellen von `ILogger` für eine vorgegebene Kategorie wird für jeden Anbieter der folgende Algorithmus verwendet:</span><span class="sxs-lookup"><span data-stu-id="2f940-846">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="2f940-847">Es werden alle Regeln ausgewählt, die dem Anbieter oder dem zugehörigen Alias entsprechen.</span><span class="sxs-lookup"><span data-stu-id="2f940-847">Select all rules that match the provider or its alias.</span></span> <span data-ttu-id="2f940-848">Wenn keine Übereinstimmung gefunden wird, werden alle Regeln mit einem leeren Anbieter ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="2f940-848">If no match is found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="2f940-849">Aus dem Ergebnis des vorherigen Schritts werden die Regeln mit dem längsten übereinstimmenden Kategoriepräfix ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="2f940-849">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="2f940-850">Wenn keine Übereinstimmung gefunden wird, werden alle Regeln ohne Angabe einer Kategorie ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="2f940-850">If no match is found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="2f940-851">Wenn mehrere Regeln ausgewählt sind, wird die **letzte** Regel verwendet.</span><span class="sxs-lookup"><span data-stu-id="2f940-851">If multiple rules are selected, take the **last** one.</span></span>
* <span data-ttu-id="2f940-852">Wenn keine Regel ausgewählt ist, wird `MinimumLevel` verwendet.</span><span class="sxs-lookup"><span data-stu-id="2f940-852">If no rules are selected, use `MinimumLevel`.</span></span>

<span data-ttu-id="2f940-853">Angenommen, Sie erstellen mit der oben aufgeführten Liste mit Regeln ein `ILogger`-Objekt für die Kategorie „Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine“:</span><span class="sxs-lookup"><span data-stu-id="2f940-853">With the preceding list of rules, suppose you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="2f940-854">Für den Debuganbieter gelten die Regeln 1, 6 und 8.</span><span class="sxs-lookup"><span data-stu-id="2f940-854">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="2f940-855">Regel 8 ist die spezifischste Regel, deshalb wird diese Regel ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="2f940-855">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="2f940-856">Für den Konsolenanbieter gelten die Regeln 3, 4, 5 und 6.</span><span class="sxs-lookup"><span data-stu-id="2f940-856">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="2f940-857">Regel 3 ist die spezifischste Regel.</span><span class="sxs-lookup"><span data-stu-id="2f940-857">Rule 3 is most specific.</span></span>

<span data-ttu-id="2f940-858">Die sich ergebende `ILogger`-Instanz sendet Protokolle mit dem Protokolliergrad `Trace` und höher an den Debuganbieter.</span><span class="sxs-lookup"><span data-stu-id="2f940-858">The resulting `ILogger` instance sends logs of `Trace` level and above to the Debug provider.</span></span> <span data-ttu-id="2f940-859">Protokolle mit dem Protokolliergrad `Debug` und höher werden an den Konsolenanbieter gesendet.</span><span class="sxs-lookup"><span data-stu-id="2f940-859">Logs of `Debug` level and above are sent to the Console provider.</span></span>

### <a name="provider-aliases"></a><span data-ttu-id="2f940-860">Anbieteraliase</span><span class="sxs-lookup"><span data-stu-id="2f940-860">Provider aliases</span></span>

<span data-ttu-id="2f940-861">Jeder Anbieter definiert einen *Alias*, der in der Konfiguration anstelle des vollqualifizierten Typnamens verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="2f940-861">Each provider defines an *alias* that can be used in configuration in place of the fully qualified type name.</span></span>  <span data-ttu-id="2f940-862">Verwenden Sie für die integrierten Anbieter die folgenden Aliase:</span><span class="sxs-lookup"><span data-stu-id="2f940-862">For the built-in providers, use the following aliases:</span></span>

* <span data-ttu-id="2f940-863">Konsole</span><span class="sxs-lookup"><span data-stu-id="2f940-863">Console</span></span>
* <span data-ttu-id="2f940-864">Debug</span><span class="sxs-lookup"><span data-stu-id="2f940-864">Debug</span></span>
* <span data-ttu-id="2f940-865">EventSource</span><span class="sxs-lookup"><span data-stu-id="2f940-865">EventSource</span></span>
* <span data-ttu-id="2f940-866">EventLog</span><span class="sxs-lookup"><span data-stu-id="2f940-866">EventLog</span></span>
* <span data-ttu-id="2f940-867">TraceSource</span><span class="sxs-lookup"><span data-stu-id="2f940-867">TraceSource</span></span>
* <span data-ttu-id="2f940-868">AzureAppServicesFile</span><span class="sxs-lookup"><span data-stu-id="2f940-868">AzureAppServicesFile</span></span>
* <span data-ttu-id="2f940-869">AzureAppServicesBlob</span><span class="sxs-lookup"><span data-stu-id="2f940-869">AzureAppServicesBlob</span></span>
* <span data-ttu-id="2f940-870">ApplicationInsights</span><span class="sxs-lookup"><span data-stu-id="2f940-870">ApplicationInsights</span></span>

### <a name="default-minimum-level"></a><span data-ttu-id="2f940-871">Standardmindestprotokolliergrad</span><span class="sxs-lookup"><span data-stu-id="2f940-871">Default minimum level</span></span>

<span data-ttu-id="2f940-872">Es gibt eine Einstellung für den Mindestprotokolliergrad, die nur dann wirksam wird, wenn für einen bestimmten Anbieter und eine bestimmte Kategorie keine Regeln aus Konfiguration oder Code gelten.</span><span class="sxs-lookup"><span data-stu-id="2f940-872">There's a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="2f940-873">Im folgenden Beispiel wird das Festlegen des Mindestprotokolliergrads veranschaulicht:</span><span class="sxs-lookup"><span data-stu-id="2f940-873">The following example shows how to set the minimum level:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_MinLevel&highlight=3)]

<span data-ttu-id="2f940-874">Wenn Sie den Mindestprotokolliergrad nicht explizit festlegen, lautet der Standardwert `Information`, d.h. Protokolle der Ebene `Trace` und `Debug` werden ignoriert.</span><span class="sxs-lookup"><span data-stu-id="2f940-874">If you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

### <a name="filter-functions"></a><span data-ttu-id="2f940-875">Filterfunktionen</span><span class="sxs-lookup"><span data-stu-id="2f940-875">Filter functions</span></span>

<span data-ttu-id="2f940-876">Eine Filterfunktion wird für alle Anbieter und Kategorien aufgerufen, denen keine Regeln durch Konfiguration oder Code zugewiesen sind.</span><span class="sxs-lookup"><span data-stu-id="2f940-876">A filter function is invoked for all providers and categories that don't have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="2f940-877">Code in der Funktion verfügt über Zugriff auf den Anbietertyp, die Kategorie und den Protokolliergrad.</span><span class="sxs-lookup"><span data-stu-id="2f940-877">Code in the function has access to the provider type, category, and log level.</span></span> <span data-ttu-id="2f940-878">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="2f940-878">For example:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

## <a name="system-categories-and-levels"></a><span data-ttu-id="2f940-879">Systemkategorien und -protokolliergrade</span><span class="sxs-lookup"><span data-stu-id="2f940-879">System categories and levels</span></span>

<span data-ttu-id="2f940-880">Dies sind einige Kategorien, die vom ASP.NET Core und Entity Framework Core verwendet werden, mit Hinweisen darauf, welche Protokolle von ihnen zu erwarten sind:</span><span class="sxs-lookup"><span data-stu-id="2f940-880">Here are some categories used by ASP.NET Core and Entity Framework Core, with notes about what logs to expect from them:</span></span>

| <span data-ttu-id="2f940-881">Kategorie</span><span class="sxs-lookup"><span data-stu-id="2f940-881">Category</span></span>                            | <span data-ttu-id="2f940-882">Hinweise</span><span class="sxs-lookup"><span data-stu-id="2f940-882">Notes</span></span> |
| ----------------------------------- | ----- |
| <span data-ttu-id="2f940-883">Microsoft.AspNetCore</span><span class="sxs-lookup"><span data-stu-id="2f940-883">Microsoft.AspNetCore</span></span>                | <span data-ttu-id="2f940-884">Allgemeine ASP.NET Core-Diagnose.</span><span class="sxs-lookup"><span data-stu-id="2f940-884">General ASP.NET Core diagnostics.</span></span> |
| <span data-ttu-id="2f940-885">Microsoft.AspNetCore.DataProtection</span><span class="sxs-lookup"><span data-stu-id="2f940-885">Microsoft.AspNetCore.DataProtection</span></span> | <span data-ttu-id="2f940-886">Gibt an, welche Schlüssel in Betracht gezogen, gefunden und verwendet wurden.</span><span class="sxs-lookup"><span data-stu-id="2f940-886">Which keys were considered, found, and used.</span></span> |
| <span data-ttu-id="2f940-887">Microsoft.AspNetCore.HostFiltering</span><span class="sxs-lookup"><span data-stu-id="2f940-887">Microsoft.AspNetCore.HostFiltering</span></span>  | <span data-ttu-id="2f940-888">Hosts sind zulässig.</span><span class="sxs-lookup"><span data-stu-id="2f940-888">Hosts allowed.</span></span> |
| <span data-ttu-id="2f940-889">Microsoft.AspNetCore.Hosting</span><span class="sxs-lookup"><span data-stu-id="2f940-889">Microsoft.AspNetCore.Hosting</span></span>        | <span data-ttu-id="2f940-890">Gibt an, wie lange es bis zum Abschluss von HTTP-Anforderungen gedauert hat und zu welcher Zeit sie gestartet wurden.</span><span class="sxs-lookup"><span data-stu-id="2f940-890">How long HTTP requests took to complete and what time they started.</span></span> <span data-ttu-id="2f940-891">Gibt an, welche Hostingstartassemblys geladen wurden.</span><span class="sxs-lookup"><span data-stu-id="2f940-891">Which hosting startup assemblies were loaded.</span></span> |
| <span data-ttu-id="2f940-892">Microsoft.AspNetCore.Mvc</span><span class="sxs-lookup"><span data-stu-id="2f940-892">Microsoft.AspNetCore.Mvc</span></span>            | <span data-ttu-id="2f940-893">MVC und Razor-Diagnose</span><span class="sxs-lookup"><span data-stu-id="2f940-893">MVC and Razor diagnostics.</span></span> <span data-ttu-id="2f940-894">Modellbindung, Filterausführung, Ansichtskompilierung, Aktionsauswahl.</span><span class="sxs-lookup"><span data-stu-id="2f940-894">Model binding, filter execution, view compilation, action selection.</span></span> |
| <span data-ttu-id="2f940-895">Microsoft.AspNetCore.Routing</span><span class="sxs-lookup"><span data-stu-id="2f940-895">Microsoft.AspNetCore.Routing</span></span>        | <span data-ttu-id="2f940-896">Gibt Routenabgleichsinformationen an.</span><span class="sxs-lookup"><span data-stu-id="2f940-896">Route matching information.</span></span> |
| <span data-ttu-id="2f940-897">Microsoft.AspNetCore.Server</span><span class="sxs-lookup"><span data-stu-id="2f940-897">Microsoft.AspNetCore.Server</span></span>         | <span data-ttu-id="2f940-898">Start-, Beendigungs- und Keep-Alive-Antworten der Verbindung.</span><span class="sxs-lookup"><span data-stu-id="2f940-898">Connection start, stop, and keep alive responses.</span></span> <span data-ttu-id="2f940-899">HTTPS-Zertifikatinformationen.</span><span class="sxs-lookup"><span data-stu-id="2f940-899">HTTPS certificate information.</span></span> |
| <span data-ttu-id="2f940-900">Microsoft.AspNetCore.StaticFiles</span><span class="sxs-lookup"><span data-stu-id="2f940-900">Microsoft.AspNetCore.StaticFiles</span></span>    | <span data-ttu-id="2f940-901">Die bereitgestellten Dateien.</span><span class="sxs-lookup"><span data-stu-id="2f940-901">Files served.</span></span> |
| <span data-ttu-id="2f940-902">Microsoft.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="2f940-902">Microsoft.EntityFrameworkCore</span></span>       | <span data-ttu-id="2f940-903">Allgemeine Entity Framework Core-Diagnose.</span><span class="sxs-lookup"><span data-stu-id="2f940-903">General Entity Framework Core diagnostics.</span></span> <span data-ttu-id="2f940-904">Datenbankaktivität und -konfiguration, Änderungserkennung, Migrationen.</span><span class="sxs-lookup"><span data-stu-id="2f940-904">Database activity and configuration, change detection, migrations.</span></span> |

## <a name="log-scopes"></a><span data-ttu-id="2f940-905">Protokollbereiche</span><span class="sxs-lookup"><span data-stu-id="2f940-905">Log scopes</span></span>

 <span data-ttu-id="2f940-906">Ein *Bereich* kann einen Satz logischer Vorgänge gruppieren.</span><span class="sxs-lookup"><span data-stu-id="2f940-906">A *scope* can group a set of logical operations.</span></span> <span data-ttu-id="2f940-907">Diese Gruppierung kann verwendet werden, um an jedes Protokoll, das als Teil einer Gruppe erstellt wird, die gleichen Daten anzufügen.</span><span class="sxs-lookup"><span data-stu-id="2f940-907">This grouping can be used to attach the same data to each log that's created as part of a set.</span></span> <span data-ttu-id="2f940-908">So kann beispielsweise jedes Protokoll, das im Rahmen der Verarbeitung einer Transaktion erstellt wird, die Transaktions-ID enthalten.</span><span class="sxs-lookup"><span data-stu-id="2f940-908">For example, every log created as part of processing a transaction can include the transaction ID.</span></span>

<span data-ttu-id="2f940-909">Ein Bereich ist ein `IDisposable`-Typ, der von der <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*>-Methode zurückgegeben und so lange beibehalten wird, bis er verworfen wird.</span><span class="sxs-lookup"><span data-stu-id="2f940-909">A scope is an `IDisposable` type that's returned by the <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> method and lasts until it's disposed.</span></span> <span data-ttu-id="2f940-910">Verwenden Sie einen Bereich, indem Sie Protokollierungsaufrufe mit einem `using`-Block umschließen, der als Wrapper verwendet wird:</span><span class="sxs-lookup"><span data-stu-id="2f940-910">Use a scope by wrapping logger calls in a `using` block:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

<span data-ttu-id="2f940-911">Der folgende Code aktiviert Bereiche für den Konsolenanbieter:</span><span class="sxs-lookup"><span data-stu-id="2f940-911">The following code enables scopes for the console provider:</span></span>

<span data-ttu-id="2f940-912">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="2f940-912">*Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="2f940-913">Um die bereichsbasierte Protokollierung zu aktivieren, muss die Konsolenprotokollierungsoption `IncludeScopes` konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="2f940-913">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>
>
> <span data-ttu-id="2f940-914">Weitere Informationen zur Konfiguration finden Sie im Abschnitt [Konfiguration](#configuration).</span><span class="sxs-lookup"><span data-stu-id="2f940-914">For information on configuration, see the [Configuration](#configuration) section.</span></span>

<span data-ttu-id="2f940-915">Jede Protokollmeldung enthält die bereichsbezogenen Informationen:</span><span class="sxs-lookup"><span data-stu-id="2f940-915">Each log message includes the scoped information:</span></span>

```
info: TodoApiSample.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApiSample.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApiSample.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApiSample.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="2f940-916">Integrierte Protokollierungsanbieter</span><span class="sxs-lookup"><span data-stu-id="2f940-916">Built-in logging providers</span></span>

<span data-ttu-id="2f940-917">ASP.NET Core wird mit den folgenden Anbietern bereitgestellt:</span><span class="sxs-lookup"><span data-stu-id="2f940-917">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="2f940-918">Konsole</span><span class="sxs-lookup"><span data-stu-id="2f940-918">Console</span></span>](#console-provider)
* [<span data-ttu-id="2f940-919">Debuggen</span><span class="sxs-lookup"><span data-stu-id="2f940-919">Debug</span></span>](#debug-provider)
* [<span data-ttu-id="2f940-920">EventSource</span><span class="sxs-lookup"><span data-stu-id="2f940-920">EventSource</span></span>](#event-source-provider)
* [<span data-ttu-id="2f940-921">EventLog</span><span class="sxs-lookup"><span data-stu-id="2f940-921">EventLog</span></span>](#windows-eventlog-provider)
* [<span data-ttu-id="2f940-922">TraceSource</span><span class="sxs-lookup"><span data-stu-id="2f940-922">TraceSource</span></span>](#tracesource-provider)
* [<span data-ttu-id="2f940-923">AzureAppServicesFile</span><span class="sxs-lookup"><span data-stu-id="2f940-923">AzureAppServicesFile</span></span>](#azure-app-service-provider)
* [<span data-ttu-id="2f940-924">AzureAppServicesBlob</span><span class="sxs-lookup"><span data-stu-id="2f940-924">AzureAppServicesBlob</span></span>](#azure-app-service-provider)
* [<span data-ttu-id="2f940-925">ApplicationInsights</span><span class="sxs-lookup"><span data-stu-id="2f940-925">ApplicationInsights</span></span>](#azure-application-insights-trace-logging)

<span data-ttu-id="2f940-926">Weitere Informationen zu „stdout“ und zur Debugprotokollierung mit dem ASP.NET Core-Modul finden Sie unter <xref:test/troubleshoot-azure-iis> und <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span><span class="sxs-lookup"><span data-stu-id="2f940-926">For information on stdout and debug logging with the ASP.NET Core Module, see <xref:test/troubleshoot-azure-iis> and <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

### <a name="console-provider"></a><span data-ttu-id="2f940-927">Der Konsolenanbieter</span><span class="sxs-lookup"><span data-stu-id="2f940-927">Console provider</span></span>

<span data-ttu-id="2f940-928">Das Anbieterpaket [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) sendet eine Protokollausgabe an die Konsole.</span><span class="sxs-lookup"><span data-stu-id="2f940-928">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span> 

```csharp
logging.AddConsole();
```

<span data-ttu-id="2f940-929">Um die Ausgabe der Konsolenprotokollierung anzuzeigen, öffnen Sie eine Eingabeaufforderung im Projektordner und führen den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="2f940-929">To see console logging output, open a command prompt in the project folder and run the following command:</span></span>

```dotnetcli
dotnet run
```

### <a name="debug-provider"></a><span data-ttu-id="2f940-930">Der Debuganbieter</span><span class="sxs-lookup"><span data-stu-id="2f940-930">Debug provider</span></span>

<span data-ttu-id="2f940-931">Beim Anbieterpaket [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) erfolgt die Protokollausgabe unter Verwendung der Klasse [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) (`Debug.WriteLine`-Methodenaufrufe).</span><span class="sxs-lookup"><span data-stu-id="2f940-931">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="2f940-932">Unter Linux werden Protokolle dieses Anbieters in */var/log/message* geschrieben.</span><span class="sxs-lookup"><span data-stu-id="2f940-932">On Linux, this provider writes logs to */var/log/message*.</span></span>

```csharp
logging.AddDebug();
```

### <a name="event-source-provider"></a><span data-ttu-id="2f940-933">Ereignisquellenanbieter</span><span class="sxs-lookup"><span data-stu-id="2f940-933">Event Source provider</span></span>

<span data-ttu-id="2f940-934">Das [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource)-Anbieterpaket schreibt in eine plattformübergreifende Ereignisquelle mit dem Namen `Microsoft-Extensions-Logging`.</span><span class="sxs-lookup"><span data-stu-id="2f940-934">The [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package writes to an Event Source cross-platform with the name `Microsoft-Extensions-Logging`.</span></span> <span data-ttu-id="2f940-935">Unter Windows verwendet der Anbieter die [Ereignisablaufverfolgung für Windows (ETW)](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span><span class="sxs-lookup"><span data-stu-id="2f940-935">On Windows, the provider uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span>

```csharp
logging.AddEventSourceLogger();
```

<span data-ttu-id="2f940-936">Der Ereignisquellenanbieter wird automatisch hinzugefügt, wenn `CreateDefaultBuilder` zum Erstellen des Hosts aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="2f940-936">The Event Source provider is added automatically when `CreateDefaultBuilder` is called to build the host.</span></span>

<span data-ttu-id="2f940-937">Verwenden Sie das [PerfView-Hilfsprogramm](https://github.com/Microsoft/perfview) zum Sammeln und Anzeigen von Protokollen.</span><span class="sxs-lookup"><span data-stu-id="2f940-937">Use the [PerfView utility](https://github.com/Microsoft/perfview) to collect and view logs.</span></span> <span data-ttu-id="2f940-938">Es gibt andere Tools zur Anzeige von ETW-Protokollen, aber PerfView bietet die besten Ergebnisse bei der Arbeit mit ETW-Ereignissen, die von ASP.NET Core ausgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="2f940-938">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET Core.</span></span>

<span data-ttu-id="2f940-939">Um PerfView für das Erfassen von Ereignissen zu konfigurieren, die von diesem Anbieter protokolliert wurden, fügen Sie die Zeichenfolge `*Microsoft-Extensions-Logging` zur Liste **Zusätzliche Anbieter** hinzu.</span><span class="sxs-lookup"><span data-stu-id="2f940-939">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="2f940-940">(Vergessen Sie nicht das Sternchen am Anfang der Zeichenfolge.)</span><span class="sxs-lookup"><span data-stu-id="2f940-940">(Don't miss the asterisk at the start of the string.)</span></span>

![Zusätzliche PerfView-Anbieter](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a><span data-ttu-id="2f940-942">Der Windows-EventLog-Anbieter</span><span class="sxs-lookup"><span data-stu-id="2f940-942">Windows EventLog provider</span></span>

<span data-ttu-id="2f940-943">Das Anbieterpaket [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) sendet eine Protokollausgabe in das Windows-Ereignisprotokoll.</span><span class="sxs-lookup"><span data-stu-id="2f940-943">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

```csharp
logging.AddEventLog();
```

<span data-ttu-id="2f940-944">Mithilfe von [AddEventLog-Überladungen](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions) können Sie <xref:Microsoft.Extensions.Logging.EventLog.EventLogSettings> übergeben.</span><span class="sxs-lookup"><span data-stu-id="2f940-944">[AddEventLog overloads](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions) let you pass in <xref:Microsoft.Extensions.Logging.EventLog.EventLogSettings>.</span></span> <span data-ttu-id="2f940-945">Wenn `null` oder nicht angegeben, werden die folgenden Standardeinstellungen verwendet:</span><span class="sxs-lookup"><span data-stu-id="2f940-945">If `null` or not specified, the following default settings are used:</span></span>

* <span data-ttu-id="2f940-946">`LogName`: „Anwendung“</span><span class="sxs-lookup"><span data-stu-id="2f940-946">`LogName`: "Application"</span></span>
* <span data-ttu-id="2f940-947">`SourceName`: „.NET Runtime“</span><span class="sxs-lookup"><span data-stu-id="2f940-947">`SourceName`: ".NET Runtime"</span></span>
* <span data-ttu-id="2f940-948">`MachineName`: Der Name des lokalen Computers wird verwendet.</span><span class="sxs-lookup"><span data-stu-id="2f940-948">`MachineName`: The local machine name is used.</span></span>

<span data-ttu-id="2f940-949">Ereignisse werden für die [Warnstufe und höher](#log-level) protokolliert.</span><span class="sxs-lookup"><span data-stu-id="2f940-949">Events are logged for [Warning level and higher](#log-level).</span></span> <span data-ttu-id="2f940-950">Im folgenden Beispiel wird der Standardprotokolliergrad auf <xref:Microsoft.Extensions.Logging.LogLevel.Information?displayProperty=nameWithType> festgelegt:</span><span class="sxs-lookup"><span data-stu-id="2f940-950">The following example sets the Event Log default log level to <xref:Microsoft.Extensions.Logging.LogLevel.Information?displayProperty=nameWithType>:</span></span>

```json
"Logging": {
  "EventLog": {
    "LogLevel": {
      "Default": "Information"
    }
  }
}
```

### <a name="tracesource-provider"></a><span data-ttu-id="2f940-951">Der TraceSource-Anbieter</span><span class="sxs-lookup"><span data-stu-id="2f940-951">TraceSource provider</span></span>

<span data-ttu-id="2f940-952">Das Anbieterpaket [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) verwendet die <xref:System.Diagnostics.TraceSource>-Bibliotheken und -Anbieter.</span><span class="sxs-lookup"><span data-stu-id="2f940-952">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the <xref:System.Diagnostics.TraceSource> libraries and providers.</span></span>

```csharp
logging.AddTraceSource(sourceSwitchName);
```

<span data-ttu-id="2f940-953">Mithilfe von [AddTraceSource-Überladungen](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions) können Sie eine Quelloption und einen Listener für die Ablaufverfolgung übergeben.</span><span class="sxs-lookup"><span data-stu-id="2f940-953">[AddTraceSource overloads](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="2f940-954">Um diesen Anbieter zu verwenden, muss eine App unter .NET Framework (anstelle von .NET Core) ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="2f940-954">To use this provider, an app has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="2f940-955">Der Anbieter kann Nachrichten an eine Vielzahl von [Listenern](/dotnet/framework/debug-trace-profile/trace-listeners) weiterleiten, z.B. an den in der Beispiel-App verwendeten <xref:System.Diagnostics.TextWriterTraceListener>.</span><span class="sxs-lookup"><span data-stu-id="2f940-955">The provider can route messages to a variety of [listeners](/dotnet/framework/debug-trace-profile/trace-listeners), such as the <xref:System.Diagnostics.TextWriterTraceListener> used in the sample app.</span></span>

### <a name="azure-app-service-provider"></a><span data-ttu-id="2f940-956">Der Azure App Service-Anbieter</span><span class="sxs-lookup"><span data-stu-id="2f940-956">Azure App Service provider</span></span>

<span data-ttu-id="2f940-957">Das Anbieterpaket [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) schreibt Protokolle in Textdateien in das Dateisystem einer Azure App Service-App und in [Blob Storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in einem Azure Storage-Konto.</span><span class="sxs-lookup"><span data-stu-id="2f940-957">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span>

```csharp
logging.AddAzureWebAppDiagnostics();
```

<span data-ttu-id="2f940-958">Dieses Anbieterpaket ist nicht im [Microsoft.AspNetCore.App-Metapaket](xref:fundamentals/metapackage-app) enthalten.</span><span class="sxs-lookup"><span data-stu-id="2f940-958">The provider package isn't included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="2f940-959">Wenn Sie Anwendungen für .NET Framework entwickeln oder auf das `Microsoft.AspNetCore.App`-Metapaket verweisen, fügen Sie das Anbieterpaket dem Projekt hinzu.</span><span class="sxs-lookup"><span data-stu-id="2f940-959">When targeting .NET Framework or referencing the `Microsoft.AspNetCore.App` metapackage, add the provider package to the project.</span></span> 

<span data-ttu-id="2f940-960">Eine <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*>-Überladung ermöglicht das Übergeben von <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings>.</span><span class="sxs-lookup"><span data-stu-id="2f940-960">An <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*> overload lets you pass in <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings>.</span></span> <span data-ttu-id="2f940-961">Das settings-Objekt kann Standardeinstellungen überschreiben, z.B. die Protokollierungsausgabevorlage, den Blobnamen und die maximale Dateigröße.</span><span class="sxs-lookup"><span data-stu-id="2f940-961">The settings object can override default settings, such as the logging output template, blob name, and file size limit.</span></span> <span data-ttu-id="2f940-962">(Die *Ausgabevorlage* ist eine Nachrichtenvorlage, die auf alle Protokolle zusätzlich zu dem angewendet wird, was mit einem `ILogger`-Methodenaufruf bereitgestellt wird.)</span><span class="sxs-lookup"><span data-stu-id="2f940-962">(*Output template* is a message template that's applied to all logs in addition to what's provided with an `ILogger` method call.)</span></span>

<span data-ttu-id="2f940-963">Wenn Sie eine Bereitstellung in einer App Service-App durchführen, berücksichtigt die Anwendung die Einstellungen im Abschnitt [App Service logs](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag) (App Service-Protokolle) auf der Seite **App Services** im Azure-Portal.</span><span class="sxs-lookup"><span data-stu-id="2f940-963">When you deploy to an App Service app, the application honors the settings in the [App Service logs](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="2f940-964">Bei einem Update der folgenden Einstellungen werden die Änderungen sofort wirksam, ohne dass ein Neustart oder eine erneute Bereitstellung der App notwendig ist.</span><span class="sxs-lookup"><span data-stu-id="2f940-964">When the following settings are updated, the changes take effect immediately without requiring a restart or redeployment of the app.</span></span>

* <span data-ttu-id="2f940-965">**Anwendungsprotokoll (Dateisystem)**</span><span class="sxs-lookup"><span data-stu-id="2f940-965">**Application Logging (Filesystem)**</span></span>
* <span data-ttu-id="2f940-966">**Anwendungsprotokoll (Blob)**</span><span class="sxs-lookup"><span data-stu-id="2f940-966">**Application Logging (Blob)**</span></span>

<span data-ttu-id="2f940-967">Der Standardspeicherort für Protokolldateien ist der Ordner *D:\\home\\LogFiles\\Application*, und der standardmäßige Dateiname lautet *diagnostics-yyyymmdd.txt*.</span><span class="sxs-lookup"><span data-stu-id="2f940-967">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="2f940-968">Die Dateigröße ist standardmäßig auf 10 MB beschränkt, und die maximal zulässige Anzahl beibehaltener Dateien lautet 2.</span><span class="sxs-lookup"><span data-stu-id="2f940-968">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="2f940-969">Der Standardblobname lautet *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span><span class="sxs-lookup"><span data-stu-id="2f940-969">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span>

<span data-ttu-id="2f940-970">Der Anbieter funktioniert nur, wenn das Projekt in der Azure-Umgebung ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="2f940-970">The provider only works when the project runs in the Azure environment.</span></span> <span data-ttu-id="2f940-971">Bei einer lokalen Ausführung zeigt er keine Auswirkungen. Der Anbieter schreibt keine Protokolle in lokale Dateien oder den lokalen Entwicklungsspeicher für BLOBs.</span><span class="sxs-lookup"><span data-stu-id="2f940-971">It has no effect when the project is run locally&mdash;it doesn't write to local files or local development storage for blobs.</span></span>

#### <a name="azure-log-streaming"></a><span data-ttu-id="2f940-972">Azure-Protokollstreaming</span><span class="sxs-lookup"><span data-stu-id="2f940-972">Azure log streaming</span></span>

<span data-ttu-id="2f940-973">Azure-Protokollstreaming ermöglicht Ihnen eine Echtzeitanzeige der Protokollaktivität für:</span><span class="sxs-lookup"><span data-stu-id="2f940-973">Azure log streaming lets you view log activity in real time from:</span></span>

* <span data-ttu-id="2f940-974">App-Server</span><span class="sxs-lookup"><span data-stu-id="2f940-974">The app server</span></span>
* <span data-ttu-id="2f940-975">Webserver</span><span class="sxs-lookup"><span data-stu-id="2f940-975">The web server</span></span>
* <span data-ttu-id="2f940-976">Ablaufverfolgung für Anforderungsfehler</span><span class="sxs-lookup"><span data-stu-id="2f940-976">Failed request tracing</span></span>

<span data-ttu-id="2f940-977">So konfigurieren Sie das Azure-Protokollstreaming</span><span class="sxs-lookup"><span data-stu-id="2f940-977">To configure Azure log streaming:</span></span>

* <span data-ttu-id="2f940-978">Navigieren Sie von der Portalseite Ihrer App zur Seite **App Service-Protokolle**.</span><span class="sxs-lookup"><span data-stu-id="2f940-978">Navigate to the **App Service logs** page from your app's portal page.</span></span>
* <span data-ttu-id="2f940-979">Legen Sie **Anwendungsprotokollierung (Dateisystem)** auf **Ein** fest.</span><span class="sxs-lookup"><span data-stu-id="2f940-979">Set **Application Logging (Filesystem)** to **On**.</span></span>
* <span data-ttu-id="2f940-980">Wählen Sie die **Protokollierungsebene**.</span><span class="sxs-lookup"><span data-stu-id="2f940-980">Choose the log **Level**.</span></span> <span data-ttu-id="2f940-981">Diese Einstellung gilt nur für das Azure-Protokollstreaming, nicht für andere Protokollierungsanbieter in der App.</span><span class="sxs-lookup"><span data-stu-id="2f940-981">This setting only applies to Azure log streaming, not other logging providers in the app.</span></span>

<span data-ttu-id="2f940-982">Navigieren Sie zur Seite **Log Stream** (Protokollstream), um App-Meldungen anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="2f940-982">Navigate to the **Log Stream** page to view app messages.</span></span> <span data-ttu-id="2f940-983">Diese werden von der App über die `ILogger`-Schnittstelle protokolliert.</span><span class="sxs-lookup"><span data-stu-id="2f940-983">They're logged by the app through the `ILogger` interface.</span></span>

### <a name="azure-application-insights-trace-logging"></a><span data-ttu-id="2f940-984">Ablaufverfolgungsprotokollierung für Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="2f940-984">Azure Application Insights trace logging</span></span>

<span data-ttu-id="2f940-985">Das [Microsoft.Extensions.Logging.ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights)-Anbieterpaket schreibt Protokolle in Azure Application Insights.</span><span class="sxs-lookup"><span data-stu-id="2f940-985">The [Microsoft.Extensions.Logging.ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights) provider package writes logs to Azure Application Insights.</span></span> <span data-ttu-id="2f940-986">Application Insights ist ein Dienst, der eine Web-App überwacht und Tools für Abfragen und Analysen von Telemetriedaten zur Verfügung stellt.</span><span class="sxs-lookup"><span data-stu-id="2f940-986">Application Insights is a service that monitors a web app and provides tools for querying and analyzing the telemetry data.</span></span> <span data-ttu-id="2f940-987">Wenn Sie diesen Anbieter verwenden, können Sie Ihre Protokolle mithilfe der Application Insights-Tools abfragen und analysieren.</span><span class="sxs-lookup"><span data-stu-id="2f940-987">If you use this provider, you can query and analyze your logs by using the Application Insights tools.</span></span>

<span data-ttu-id="2f940-988">Der Protokollierungsanbieter ist als Abhängigkeit von [Microsoft.ApplicationInsights.AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) enthalten, d.h. als das Paket, das alle verfügbaren Telemetriedaten für ASP.NET Core bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="2f940-988">The logging provider is included as a dependency of [Microsoft.ApplicationInsights.AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore), which is the package that provides all available telemetry for ASP.NET Core.</span></span> <span data-ttu-id="2f940-989">Wenn Sie dieses Paket verwenden, ist nicht erforderlich, das Anbieterpaket zu installieren.</span><span class="sxs-lookup"><span data-stu-id="2f940-989">If you use this package, you don't have to install the provider package.</span></span>

<span data-ttu-id="2f940-990">Verwenden Sie nicht das [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web)-Paket, das für ASP.NET 4.x bestimmt ist.</span><span class="sxs-lookup"><span data-stu-id="2f940-990">Don't use the [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) package&mdash;that's for ASP.NET 4.x.</span></span>

<span data-ttu-id="2f940-991">Weitere Informationen finden Sie in den folgenden Ressourcen:</span><span class="sxs-lookup"><span data-stu-id="2f940-991">For more information, see the following resources:</span></span>

* [<span data-ttu-id="2f940-992">Application Insights-Übersicht</span><span class="sxs-lookup"><span data-stu-id="2f940-992">Application Insights overview</span></span>](/azure/application-insights/app-insights-overview)
* <span data-ttu-id="2f940-993">[Anwendungseinblicke für ASP.NET Core-Anwendungen](/azure/azure-monitor/app/asp-net-core): Beginnen Sie hier, wenn Sie die gesamte Bandbreite der Application Insights-Telemetrie zusammen mit der Protokollierung implementieren möchten.</span><span class="sxs-lookup"><span data-stu-id="2f940-993">[Application Insights for ASP.NET Core applications](/azure/azure-monitor/app/asp-net-core) - Start here if you want to implement the full range of Application Insights telemetry along with logging.</span></span>
* <span data-ttu-id="2f940-994">[ApplicationInsightsLoggerProvider für .NET Core ILogger-Protokolle](/azure/azure-monitor/app/ilogger): Beginnen Sie hier, wenn Sie den Protokollierungsanbieter ohne die übrige Application Insights-Telemetrie implementieren möchten.</span><span class="sxs-lookup"><span data-stu-id="2f940-994">[ApplicationInsightsLoggerProvider for .NET Core ILogger logs](/azure/azure-monitor/app/ilogger) - Start here if you want to implement the logging provider without the rest of Application Insights telemetry.</span></span>
* <span data-ttu-id="2f940-995">[Application Insights logging adapters (Adapter zur Protokollierung für Application Insights)](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-trace-logs).</span><span class="sxs-lookup"><span data-stu-id="2f940-995">[Application Insights logging adapters](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-trace-logs).</span></span>
* <span data-ttu-id="2f940-996">[Installieren, Konfigurieren und Initialisieren des Application Insights SDK](/learn/modules/instrument-web-app-code-with-application-insights): interaktive Tutorials auf der Microsoft Learn-Website.</span><span class="sxs-lookup"><span data-stu-id="2f940-996">[Install, configure, and initialize the Application Insights SDK](/learn/modules/instrument-web-app-code-with-application-insights) - Interactive tutorial on the Microsoft Learn site.</span></span>

## <a name="third-party-logging-providers"></a><span data-ttu-id="2f940-997">Protokollierungsanbieter von Drittanbietern</span><span class="sxs-lookup"><span data-stu-id="2f940-997">Third-party logging providers</span></span>

<span data-ttu-id="2f940-998">Protokollierungsframeworks von Drittanbietern aufgeführt, die mit ASP.NET Core funktionieren:</span><span class="sxs-lookup"><span data-stu-id="2f940-998">Third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="2f940-999">[elmah.io](https://elmah.io/) ([GitHub-Repository](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="2f940-999">[elmah.io](https://elmah.io/) ([GitHub repo](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span></span>
* <span data-ttu-id="2f940-1000">[Gelf](https://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub-Repository](https://github.com/mattwcole/gelf-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="2f940-1000">[Gelf](https://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub repo](https://github.com/mattwcole/gelf-extensions-logging))</span></span>
* <span data-ttu-id="2f940-1001">[JSNLog](https://jsnlog.com/) ([GitHub-Repository](https://github.com/mperdeck/jsnlog))</span><span class="sxs-lookup"><span data-stu-id="2f940-1001">[JSNLog](https://jsnlog.com/) ([GitHub repo](https://github.com/mperdeck/jsnlog))</span></span>
* <span data-ttu-id="2f940-1002">[KissLog.net](https://kisslog.net/) ([GitHub repo](https://github.com/catalingavan/KissLog-net))</span><span class="sxs-lookup"><span data-stu-id="2f940-1002">[KissLog.net](https://kisslog.net/) ([GitHub repo](https://github.com/catalingavan/KissLog-net))</span></span>
* <span data-ttu-id="2f940-1003">[Log4Net](https://logging.apache.org/log4net/) ([GitHub-Repository](https://github.com/huorswords/Microsoft.Extensions.Logging.Log4Net.AspNetCore))</span><span class="sxs-lookup"><span data-stu-id="2f940-1003">[Log4Net](https://logging.apache.org/log4net/) ([GitHub repo](https://github.com/huorswords/Microsoft.Extensions.Logging.Log4Net.AspNetCore))</span></span>
* <span data-ttu-id="2f940-1004">[Loggr](https://loggr.net/) ([GitHub-Repository](https://github.com/imobile3/Loggr.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="2f940-1004">[Loggr](https://loggr.net/) ([GitHub repo](https://github.com/imobile3/Loggr.Extensions.Logging))</span></span>
* <span data-ttu-id="2f940-1005">[NLog](https://nlog-project.org/) ([GitHub-Repository](https://github.com/NLog/NLog.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="2f940-1005">[NLog](https://nlog-project.org/) ([GitHub repo](https://github.com/NLog/NLog.Extensions.Logging))</span></span>
* <span data-ttu-id="2f940-1006">[Sentry](https://sentry.io/welcome/) ([GitHub-Repository](https://github.com/getsentry/sentry-dotnet))</span><span class="sxs-lookup"><span data-stu-id="2f940-1006">[Sentry](https://sentry.io/welcome/) ([GitHub repo](https://github.com/getsentry/sentry-dotnet))</span></span>
* <span data-ttu-id="2f940-1007">[Serilog](https://serilog.net/) ([GitHub-Repository](https://github.com/serilog/serilog-aspnetcore))</span><span class="sxs-lookup"><span data-stu-id="2f940-1007">[Serilog](https://serilog.net/) ([GitHub repo](https://github.com/serilog/serilog-aspnetcore))</span></span>
* <span data-ttu-id="2f940-1008">[Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([GitHub-Repository](https://github.com/googleapis/google-cloud-dotnet))</span><span class="sxs-lookup"><span data-stu-id="2f940-1008">[Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([Github repo](https://github.com/googleapis/google-cloud-dotnet))</span></span>

<span data-ttu-id="2f940-1009">Einige Drittanbieterframeworks können eine [semantische Protokollierung (auch als strukturierte Protokollierung bezeichnet)](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging) ausführen.</span><span class="sxs-lookup"><span data-stu-id="2f940-1009">Some third-party frameworks can perform [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="2f940-1010">Die Verwendung eines Frameworks von Drittanbietern ist ähnlich wie die Verwendung eines integrierten Anbieters:</span><span class="sxs-lookup"><span data-stu-id="2f940-1010">Using a third-party framework is similar to using one of the built-in providers:</span></span>

1. <span data-ttu-id="2f940-1011">Fügen Sie Ihrem Paket ein NuGet-Paket hinzu.</span><span class="sxs-lookup"><span data-stu-id="2f940-1011">Add a NuGet package to your project.</span></span>
1. <span data-ttu-id="2f940-1012">Rufen Sie eine `ILoggerFactory`-Erweiterungsmethode auf, die vom Protokollierungsframework bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="2f940-1012">Call an `ILoggerFactory` extension method provided by the logging framework.</span></span>

<span data-ttu-id="2f940-1013">Weitere Informationen finden Sie in der Dokumentation zum jeweiligen Anbieter.</span><span class="sxs-lookup"><span data-stu-id="2f940-1013">For more information, see each provider's documentation.</span></span> <span data-ttu-id="2f940-1014">Protokollierungsanbieter von Drittanbietern werden von Microsoft nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="2f940-1014">Third-party logging providers aren't supported by Microsoft.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2f940-1015">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="2f940-1015">Additional resources</span></span>

* <xref:fundamentals/logging/loggermessage>

::: moniker-end
