---
title: ASP.NET Core-Blazor-Vorlagen
author: guardrex
description: Hier erfahren Sie mehr über ASP.NET Core-Vorlagen für Blazor-Apps und die Blazor-Projektstruktur.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/19/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/templates
ms.openlocfilehash: f1b131947a242323295a763ba2f2473af0ccfb4f
ms.sourcegitcommit: 66fca14611eba141d455fe0bd2c37803062e439c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2020
ms.locfileid: "85944535"
---
# <a name="aspnet-core-blazor-templates"></a><span data-ttu-id="53e09-103">ASP.NET Core-Blazor-Vorlagen</span><span class="sxs-lookup"><span data-stu-id="53e09-103">ASP.NET Core Blazor templates</span></span>

<span data-ttu-id="53e09-104">Von [Daniel Roth](https://github.com/danroth27) und [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="53e09-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="53e09-105">Das Blazor-Framework bietet Vorlagen zum Entwickeln von Apps für die einzelnen Blazor-Hostingmodelle:</span><span class="sxs-lookup"><span data-stu-id="53e09-105">The Blazor framework provides templates to develop apps for each of the Blazor hosting models:</span></span>

* Blazor WebAssembly<span data-ttu-id="53e09-106"> (`blazorwasm`)</span><span class="sxs-lookup"><span data-stu-id="53e09-106"> (`blazorwasm`)</span></span>
* Blazor Server<span data-ttu-id="53e09-107"> (`blazorserver`)</span><span class="sxs-lookup"><span data-stu-id="53e09-107"> (`blazorserver`)</span></span>

<span data-ttu-id="53e09-108">Weitere Informationen zur Blazor-Hostingmodellen finden Sie unter <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="53e09-108">For more information on Blazor's hosting models, see <xref:blazor/hosting-models>.</span></span>

<span data-ttu-id="53e09-109">Vorlagenoptionen sind verfügbar, indem Sie die `--help`-Option an den CLI-Befehl [`dotnet new`](/dotnet/core/tools/dotnet-new) übergeben:</span><span class="sxs-lookup"><span data-stu-id="53e09-109">Template options are available by passing the `--help` option to the [`dotnet new`](/dotnet/core/tools/dotnet-new) CLI command:</span></span>

```dotnetcli
dotnet new blazorwasm --help
dotnet new blazorserver --help
```

## <a name="blazor-project-structure"></a>Blazor<span data-ttu-id="53e09-110">-Projektstruktur</span><span class="sxs-lookup"><span data-stu-id="53e09-110"> project structure</span></span>

<span data-ttu-id="53e09-111">Die folgenden Dateien und Ordner bilden eine Blazor-App, die aus einer Blazor-Vorlage generiert wird:</span><span class="sxs-lookup"><span data-stu-id="53e09-111">The following files and folders make up a Blazor app generated from a Blazor template:</span></span>

* <span data-ttu-id="53e09-112">`Program.cs`: Der Einstiegspunkt der App, von dem aus Folgendes eingerichtet wird:</span><span class="sxs-lookup"><span data-stu-id="53e09-112">`Program.cs`: The app's entry point that sets up the:</span></span>

  * <span data-ttu-id="53e09-113">ASP.NET Core-[Host](xref:fundamentals/host/generic-host) (Blazor Server)</span><span class="sxs-lookup"><span data-stu-id="53e09-113">ASP.NET Core [host](xref:fundamentals/host/generic-host) (Blazor Server)</span></span>
  * <span data-ttu-id="53e09-114">WebAssembly-Host (Blazor WebAssembly): Der Code in dieser Datei ist für Apps eindeutig, die aus der Blazor WebAssembly-Vorlage (`blazorwasm`) erstellt wurden.</span><span class="sxs-lookup"><span data-stu-id="53e09-114">WebAssembly host (Blazor WebAssembly): The code in this file is unique to apps created from the Blazor WebAssembly template (`blazorwasm`).</span></span>
    * <span data-ttu-id="53e09-115">Die `App`-Komponente, die die Stammkomponente der App ist, wird als `app`-DOM-Element für die `Add`-Methode angegeben.</span><span class="sxs-lookup"><span data-stu-id="53e09-115">The `App` component, which is the root component of the app, is specified as the `app` DOM element to the `Add` method.</span></span>
    * <span data-ttu-id="53e09-116">Dienste können mit der `ConfigureServices`-Methode auf dem Host-Generator konfiguriert werden (z. B. `builder.Services.AddSingleton<IMyDependency, MyDependency>();`).</span><span class="sxs-lookup"><span data-stu-id="53e09-116">Services can be configured with the `ConfigureServices` method on the host builder (for example, `builder.Services.AddSingleton<IMyDependency, MyDependency>();`).</span></span>
    * <span data-ttu-id="53e09-117">Die Konfiguration kann über den Host-Generator (`builder.Configuration`) bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="53e09-117">Configuration can be supplied via the host builder (`builder.Configuration`).</span></span>

* <span data-ttu-id="53e09-118">`Startup.cs` (Blazor Server): Diese Datei enthält die Startlogik der App.</span><span class="sxs-lookup"><span data-stu-id="53e09-118">`Startup.cs` (Blazor Server): Contains the app's startup logic.</span></span> <span data-ttu-id="53e09-119">Die `Startup`-Klasse definiert zwei Methoden:</span><span class="sxs-lookup"><span data-stu-id="53e09-119">The `Startup` class defines two methods:</span></span>

  * <span data-ttu-id="53e09-120">`ConfigureServices`: Diese Methode konfiguriert die [DI-Dienste (Dependency Injection)](xref:fundamentals/dependency-injection) der App.</span><span class="sxs-lookup"><span data-stu-id="53e09-120">`ConfigureServices`: Configures the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) services.</span></span> <span data-ttu-id="53e09-121">In Blazor Server-Apps werden Dienste durch Aufrufe von <xref:Microsoft.Extensions.DependencyInjection.ComponentServiceCollectionExtensions.AddServerSideBlazor%2A> hinzugefügt, und `WeatherForecastService` wird dem Dienstcontainer hinzugefügt, damit die `FetchData`-Beispielkomponente darauf zugreifen kann.</span><span class="sxs-lookup"><span data-stu-id="53e09-121">In Blazor Server apps, services are added by calling <xref:Microsoft.Extensions.DependencyInjection.ComponentServiceCollectionExtensions.AddServerSideBlazor%2A>, and the `WeatherForecastService` is added to the service container for use by the example `FetchData` component.</span></span>
  * <span data-ttu-id="53e09-122">`Configure`: Konfiguriert die Pipeline für die Anforderungsverarbeitung der App:</span><span class="sxs-lookup"><span data-stu-id="53e09-122">`Configure`: Configures the app's request handling pipeline:</span></span>
    * <span data-ttu-id="53e09-123"><xref:Microsoft.AspNetCore.Builder.ComponentEndpointRouteBuilderExtensions.MapBlazorHub%2A> wird aufgerufen, um einen Endpunkt für die Echtzeitverbindung mit dem Browser einzurichten.</span><span class="sxs-lookup"><span data-stu-id="53e09-123"><xref:Microsoft.AspNetCore.Builder.ComponentEndpointRouteBuilderExtensions.MapBlazorHub%2A> is called to set up an endpoint for the real-time connection with the browser.</span></span> <span data-ttu-id="53e09-124">Die Verbindung wird mit [SignalR](xref:signalr/introduction) hergestellt. Dabei handelt es sich um ein Framework zum Hinzufügen von Echtzeitwebfunktionen zu Apps.</span><span class="sxs-lookup"><span data-stu-id="53e09-124">The connection is created with [SignalR](xref:signalr/introduction), which is a framework for adding real-time web functionality to apps.</span></span>
    * <span data-ttu-id="53e09-125">[`MapFallbackToPage("/_Host")`](xref:Microsoft.AspNetCore.Builder.RazorPagesEndpointRouteBuilderExtensions.MapFallbackToPage*) wird aufgerufen, um die Stammseite der App (`Pages/_Host.cshtml`) einzurichten und die Navigation zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="53e09-125">[`MapFallbackToPage("/_Host")`](xref:Microsoft.AspNetCore.Builder.RazorPagesEndpointRouteBuilderExtensions.MapFallbackToPage*) is called to set up the root page of the app (`Pages/_Host.cshtml`) and enable navigation.</span></span>

* <span data-ttu-id="53e09-126">`wwwroot/index.html` (Blazor WebAssembly): Die Stammseite der App, die als HTML-Seite implementiert ist:</span><span class="sxs-lookup"><span data-stu-id="53e09-126">`wwwroot/index.html` (Blazor WebAssembly): The root page of the app implemented as an HTML page:</span></span>
  * <span data-ttu-id="53e09-127">Wenn eine Seite der App zum ersten Mal angefordert wird, wird diese Seite gerendert und in der Antwort zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="53e09-127">When any page of the app is initially requested, this page is rendered and returned in the response.</span></span>
  * <span data-ttu-id="53e09-128">Die Seite gibt an, wo die `App`-Stammkomponente gerendert wird.</span><span class="sxs-lookup"><span data-stu-id="53e09-128">The page specifies where the root `App` component is rendered.</span></span> <span data-ttu-id="53e09-129">Die `App`-Komponente (`App.razor`) wird in der `AddComponent` -Methode in `Startup.Configure` als `app`-DOM-Element angegeben.</span><span class="sxs-lookup"><span data-stu-id="53e09-129">The `App` component (`App.razor`) is specified as the `app` DOM element to the `AddComponent` method in `Startup.Configure`.</span></span>
  * <span data-ttu-id="53e09-130">Die `_framework/blazor.webassembly.js`-JavaScript-Datei wird geladen:</span><span class="sxs-lookup"><span data-stu-id="53e09-130">The `_framework/blazor.webassembly.js` JavaScript file is loaded, which:</span></span>
    * <span data-ttu-id="53e09-131">Diese lädt die .NET-Runtime, die App und der App-Abhängigkeiten herunter.</span><span class="sxs-lookup"><span data-stu-id="53e09-131">Downloads the .NET runtime, the app, and the app's dependencies.</span></span>
    * <span data-ttu-id="53e09-132">Sie initialisiert die Runtime, damit die App ausgeführt werden kann.</span><span class="sxs-lookup"><span data-stu-id="53e09-132">Initializes the runtime to run the app.</span></span>

* <span data-ttu-id="53e09-133">`App.razor`: Hierbei handelt es sich um die Stammkomponente der App, die das clientseitige Routing mithilfe der <xref:Microsoft.AspNetCore.Components.Routing.Router>-Komponente einrichtet.</span><span class="sxs-lookup"><span data-stu-id="53e09-133">`App.razor`: The root component of the app that sets up client-side routing using the <xref:Microsoft.AspNetCore.Components.Routing.Router> component.</span></span> <span data-ttu-id="53e09-134">Die <xref:Microsoft.AspNetCore.Components.Routing.Router>-Komponente fängt die Browsernavigation ab und rendert die Seite, die der angeforderten Adresse entspricht.</span><span class="sxs-lookup"><span data-stu-id="53e09-134">The <xref:Microsoft.AspNetCore.Components.Routing.Router> component intercepts browser navigation and renders the page that matches the requested address.</span></span>

* <span data-ttu-id="53e09-135">`Pages`-Ordner: Dieser Ordner enthält die routingfähigen Komponenten/Seiten (`.razor`), aus denen die Blazor-App besteht, sowie die Razor-Stammseite einer Blazor Server-App.</span><span class="sxs-lookup"><span data-stu-id="53e09-135">`Pages` folder: Contains the routable components/pages (`.razor`) that make up the Blazor app and the root Razor page of a Blazor Server app.</span></span> <span data-ttu-id="53e09-136">Die Route für jede Seite wird mithilfe der [`@page`](xref:mvc/views/razor#page)-Anweisung angegeben.</span><span class="sxs-lookup"><span data-stu-id="53e09-136">The route for each page is specified using the [`@page`](xref:mvc/views/razor#page) directive.</span></span> <span data-ttu-id="53e09-137">Die Vorlage besteht aus den folgenden Teilen:</span><span class="sxs-lookup"><span data-stu-id="53e09-137">The template includes the following:</span></span>
  * <span data-ttu-id="53e09-138">`_Host.cshtml` (Blazor Server): Die Stammseite der App, die als Razor-Seite implementiert ist:</span><span class="sxs-lookup"><span data-stu-id="53e09-138">`_Host.cshtml` (Blazor Server): The root page of the app implemented as a Razor Page:</span></span>
    * <span data-ttu-id="53e09-139">Wenn eine Seite der App zum ersten Mal angefordert wird, wird diese Seite gerendert und in der Antwort zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="53e09-139">When any page of the app is initially requested, this page is rendered and returned in the response.</span></span>
    * <span data-ttu-id="53e09-140">Die `_framework/blazor.server.js`-JavaScript-Datei wird geladen, die die SignalR-Echtzeitverbindung zwischen dem Browser und dem Server einrichtet.</span><span class="sxs-lookup"><span data-stu-id="53e09-140">The `_framework/blazor.server.js` JavaScript file is loaded, which sets up the real-time SignalR connection between the browser and the server.</span></span>
    * <span data-ttu-id="53e09-141">Die Hostseite gibt an, wo die `App`-Stammkomponente (`App.razor`) gerendert wird.</span><span class="sxs-lookup"><span data-stu-id="53e09-141">The Host page specifies where the root `App` component (`App.razor`) is rendered.</span></span>
  * <span data-ttu-id="53e09-142">`Counter` (`Pages/Counter.razor`): Implementiert die Zählerseite.</span><span class="sxs-lookup"><span data-stu-id="53e09-142">`Counter` (`Pages/Counter.razor`): Implements the Counter page.</span></span>
  * <span data-ttu-id="53e09-143">`Error` (`Error.razor`, nur Blazor Server-App): Wird gerendert, wenn in der App eine nicht behandelte Ausnahme auftritt.</span><span class="sxs-lookup"><span data-stu-id="53e09-143">`Error` (`Error.razor`, Blazor Server app only): Rendered when an unhandled exception occurs in the app.</span></span>
  * <span data-ttu-id="53e09-144">`FetchData` (`Pages/FetchData.razor`): Implementiert die Seite zum Abrufen von Daten.</span><span class="sxs-lookup"><span data-stu-id="53e09-144">`FetchData` (`Pages/FetchData.razor`): Implements the Fetch data page.</span></span>
  * <span data-ttu-id="53e09-145">`Index` (`Pages/Index.razor`): Implementiert die Homepage.</span><span class="sxs-lookup"><span data-stu-id="53e09-145">`Index` (`Pages/Index.razor`): Implements the Home page.</span></span>

* <span data-ttu-id="53e09-146">`Shared`-Ordner: Dieser Ordner enthält weitere Benutzeroberflächenkomponenten (`.razor`), die von der App verwendet werden:</span><span class="sxs-lookup"><span data-stu-id="53e09-146">`Shared` folder: Contains other UI components (`.razor`) used by the app:</span></span>
  * <span data-ttu-id="53e09-147">`MainLayout` (`MainLayout.razor`): Die Layoutkomponente der App.</span><span class="sxs-lookup"><span data-stu-id="53e09-147">`MainLayout` (`MainLayout.razor`): The app's layout component.</span></span>
  * <span data-ttu-id="53e09-148">`NavMenu` (`NavMenu.razor`): Diese Komponente implementiert die Navigation in der Seitenleiste.</span><span class="sxs-lookup"><span data-stu-id="53e09-148">`NavMenu` (`NavMenu.razor`): Implements sidebar navigation.</span></span> <span data-ttu-id="53e09-149">Sie schließt die [`NavLink`-Komponente](xref:blazor/fundamentals/routing#navlink-component) (<xref:Microsoft.AspNetCore.Components.Routing.NavLink>) ein, die Navigationslinks zu anderen Razor-Komponenten rendert.</span><span class="sxs-lookup"><span data-stu-id="53e09-149">Includes the [`NavLink` component](xref:blazor/fundamentals/routing#navlink-component) (<xref:Microsoft.AspNetCore.Components.Routing.NavLink>), which renders navigation links to other Razor components.</span></span> <span data-ttu-id="53e09-150">Die <xref:Microsoft.AspNetCore.Components.Routing.NavLink>-Komponente gibt automatisch einen ausgewählten Zustand an, wenn die Komponente geladen wird, sodass der Benutzer nachvollziehen kann, welche Komponente derzeit angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="53e09-150">The <xref:Microsoft.AspNetCore.Components.Routing.NavLink> component automatically indicates a selected state when its component is loaded, which helps the user understand which component is currently displayed.</span></span>

* <span data-ttu-id="53e09-151">`_Imports.razor`: Enthält gängige Razor-Anweisungen, die in die Komponenten der App (`.razor`) eingefügt werden sollen, z. B. [`@using`](xref:mvc/views/razor#using)-Anweisungen für Namespaces.</span><span class="sxs-lookup"><span data-stu-id="53e09-151">`_Imports.razor`: Includes common Razor directives to include in the app's components (`.razor`), such as [`@using`](xref:mvc/views/razor#using) directives for namespaces.</span></span>

* <span data-ttu-id="53e09-152">`Data`-Ordner (Blazor Server): Dieser Ordner enthält die `WeatherForecast`-Klasse und Implementierung von `WeatherForecastService`, die Beispielwetterdaten für die `FetchData`-Komponente der App bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="53e09-152">`Data` folder (Blazor Server): Contains the `WeatherForecast` class and implementation of the `WeatherForecastService` that provide example weather data to the app's `FetchData` component.</span></span>

* <span data-ttu-id="53e09-153">`wwwroot`: Der Ordner [Web Root](xref:fundamentals/index#web-root) für die App, der die öffentlichen statischen Ressourcen der App enthält.</span><span class="sxs-lookup"><span data-stu-id="53e09-153">`wwwroot`: The [Web Root](xref:fundamentals/index#web-root) folder for the app containing the app's public static assets.</span></span>

* <span data-ttu-id="53e09-154">`appsettings.json` (Blazor Server): Konfigurationseinstellungen für die App.</span><span class="sxs-lookup"><span data-stu-id="53e09-154">`appsettings.json` (Blazor Server): Configuration settings for the app.</span></span>
