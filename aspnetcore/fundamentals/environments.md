---
title: Verwenden von mehreren Umgebungen in ASP.NET Core
author: rick-anderson
description: Erfahren Sie, wie Sie in ASP.NET Core-Apps das App-Verhalten umgebungsübergreifend steuern können.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 7/1/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: fundamentals/environments
ms.openlocfilehash: 977d222ed61fa914bd4ffb70e192ef19d4da5c33
ms.sourcegitcommit: 6fb27ea41a92f6d0e91dfd0eba905d2ac1a707f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/15/2020
ms.locfileid: "87330628"
---
# <a name="use-multiple-environments-in-aspnet-core"></a><span data-ttu-id="4e8f3-103">Verwenden von mehreren Umgebungen in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4e8f3-103">Use multiple environments in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="4e8f3-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT) und [Kirk Larkin](https://twitter.com/serpent5)</span><span class="sxs-lookup"><span data-stu-id="4e8f3-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Kirk Larkin](https://twitter.com/serpent5)</span></span>

<span data-ttu-id="4e8f3-105">ASP.NET Core konfiguriert das App-Verhalten basierend auf der Laufzeitumgebung mit einer Umgebungsvariablen.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-105">ASP.NET Core configures app behavior based on the runtime environment using an environment variable.</span></span>

<span data-ttu-id="4e8f3-106">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/3.1sample) ([Vorgehensweise zum Herunterladen](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4e8f3-106">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/3.1sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="4e8f3-107">Umgebungen</span><span class="sxs-lookup"><span data-stu-id="4e8f3-107">Environments</span></span>

<span data-ttu-id="4e8f3-108">ASP.NET Core liest zur Bestimmung der Laufzeitumgebung aus den folgenden Umgebungsvariablen:</span><span class="sxs-lookup"><span data-stu-id="4e8f3-108">To determine the runtime environment, ASP.NET Core reads from the following environment variables:</span></span>

1. [<span data-ttu-id="4e8f3-109">DOTNET_ENVIRONMENT</span><span class="sxs-lookup"><span data-stu-id="4e8f3-109">DOTNET_ENVIRONMENT</span></span>](xref:fundamentals/configuration/index#default-host-configuration)
1. <span data-ttu-id="4e8f3-110">`ASPNETCORE_ENVIRONMENT`, bei Aufruf von <xref:Microsoft.Extensions.Hosting.GenericHostBuilderExtensions.ConfigureWebHostDefaults*>.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-110">`ASPNETCORE_ENVIRONMENT` when <xref:Microsoft.Extensions.Hosting.GenericHostBuilderExtensions.ConfigureWebHostDefaults*> is called.</span></span> <span data-ttu-id="4e8f3-111">`ConfigureWebHostDefaults` wird von den Standardvorlagen der ASP.NET Core-Web-App aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-111">The default ASP.NET Core web app templates call `ConfigureWebHostDefaults`.</span></span> <span data-ttu-id="4e8f3-112">`DOTNET_ENVIRONMENT` wird vom `ASPNETCORE_ENVIRONMENT`-Wert überschrieben.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-112">The `ASPNETCORE_ENVIRONMENT` value overrides `DOTNET_ENVIRONMENT`.</span></span>

<span data-ttu-id="4e8f3-113">`IHostEnvironment.EnvironmentName` kann auf einen beliebigen Wert festgelegt werden, das Framework stellt allerdings die folgenden Werte bereit:</span><span class="sxs-lookup"><span data-stu-id="4e8f3-113">`IHostEnvironment.EnvironmentName` can be set to any value, but the following values are provided by the framework:</span></span>

* <span data-ttu-id="4e8f3-114"><xref:Microsoft.Extensions.Hosting.Environments.Development> : Mit der Datei [launchSettings.json](#lsj) wird `ASPNETCORE_ENVIRONMENT` auf dem lokalen Computer auf `Development` festgelegt.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-114"><xref:Microsoft.Extensions.Hosting.Environments.Development> : The [launchSettings.json](#lsj) file sets `ASPNETCORE_ENVIRONMENT` to `Development` on the local machine.</span></span>
* <xref:Microsoft.Extensions.Hosting.Environments.Staging>
* <span data-ttu-id="4e8f3-115"><xref:Microsoft.Extensions.Hosting.Environments.Production> : Der Standardwert, wenn `DOTNET_ENVIRONMENT` und `ASPNETCORE_ENVIRONMENT` nicht festgelegt wurden.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-115"><xref:Microsoft.Extensions.Hosting.Environments.Production> : The default if `DOTNET_ENVIRONMENT` and `ASPNETCORE_ENVIRONMENT` have not been set.</span></span>

<span data-ttu-id="4e8f3-116">Der folgende Code</span><span class="sxs-lookup"><span data-stu-id="4e8f3-116">The following code:</span></span>

* <span data-ttu-id="4e8f3-117">Ruft [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) auf, wenn `ASPNETCORE_ENVIRONMENT` auf `Development` festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-117">Calls [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="4e8f3-118">ruft [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) auf, wenn der Wert von `ASPNETCORE_ENVIRONMENT` auf `Staging`, `Production` oder `Staging_2` festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-118">Calls [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) when the value of `ASPNETCORE_ENVIRONMENT` is set to `Staging`, `Production`, or  `Staging_2`.</span></span>
* <span data-ttu-id="4e8f3-119">fügt <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> in `Startup.Configure` ein.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-119">Injects <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> into `Startup.Configure`.</span></span> <span data-ttu-id="4e8f3-120">Diese Vorgehensweise ist nützlich, wenn die App `Startup.Configure` nur für einige Umgebungen mit minimalen Codeunterschieden pro Umgebung anpassen muss.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-120">This approach is useful when the app only requires adjusting `Startup.Configure` for a few environments with minimal code differences per environment.</span></span>
* <span data-ttu-id="4e8f3-121">ähnelt dem von den ASP.NET Core-Vorlagen generierten Code.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-121">Is similar to the code generated by the ASP.NET Core templates.</span></span>

[!code-csharp[](environments/3.1sample/EnvironmentsSample/Startup.cs?name=snippet)]

<span data-ttu-id="4e8f3-122">Das [Hilfsprogramm für Umgebungstags](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) verwendet den Wert von [IHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName) zum Einschließen oder Ausschließen von Markup im Element:</span><span class="sxs-lookup"><span data-stu-id="4e8f3-122">The [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of [IHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName) to include or exclude markup in the element:</span></span>

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

<span data-ttu-id="4e8f3-123">Die [Infoseite](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/3.1sample/EnvironmentsSample/Pages/About.cshtml) des [Beispielcodes](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/3.1sample) enthält das vorangehende Markup und zeigt den Wert von `IWebHostEnvironment.EnvironmentName` an.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-123">The [About page](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/3.1sample/EnvironmentsSample/Pages/About.cshtml) from the [sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/3.1sample) includes the preceding markup and displays the value of `IWebHostEnvironment.EnvironmentName`.</span></span>

<span data-ttu-id="4e8f3-124">Unter Windows und macOS wird bei Umgebungsvariablen und Werten die Groß-/Kleinschreibung nicht beachtet.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-124">On Windows and macOS, environment variables and values aren't case-sensitive.</span></span> <span data-ttu-id="4e8f3-125">Bei Linux-Umgebungsvariablen und -Werten wird die **Groß-/Kleinschreibung** standardmäßig beachtet.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-125">Linux environment variables and values are **case-sensitive** by default.</span></span>

### <a name="create-environmentssample"></a><span data-ttu-id="4e8f3-126">Erstellen von „EnvironmentsSample“</span><span class="sxs-lookup"><span data-stu-id="4e8f3-126">Create EnvironmentsSample</span></span>

<span data-ttu-id="4e8f3-127">Der in diesem Artikel verwendete [Beispielcode](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/3.1sample) basiert auf einem Razor Pages-Projekt namens *EnvironmentsSample*.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-127">The [sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/3.1sample) used in this document is based on a Razor Pages project named *EnvironmentsSample*.</span></span>

<span data-ttu-id="4e8f3-128">Mit dem folgenden Code wird eine Web-App mit dem Namen *EnvironmentsSample* erstellt und ausgeführt:</span><span class="sxs-lookup"><span data-stu-id="4e8f3-128">The following code creates and runs a web app named *EnvironmentsSample*:</span></span>

```bash
dotnet new webapp -o EnvironmentsSample
cd EnvironmentsSample
dotnet run --verbosity normal
```

<span data-ttu-id="4e8f3-129">Wenn die App ausgeführt wird, wird eine Ausgabe ähnlich der folgenden angezeigt:</span><span class="sxs-lookup"><span data-stu-id="4e8f3-129">When the app runs, it displays some of the following output:</span></span>

```bash
Using launch settings from c:\tmp\EnvironmentsSample\Properties\launchSettings.json
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: c:\tmp\EnvironmentsSample
```

<a name="lsj"></a>

### <a name="development-and-launchsettingsjson"></a><span data-ttu-id="4e8f3-130">Entwicklung und die Datei „launchSettings.json“</span><span class="sxs-lookup"><span data-stu-id="4e8f3-130">Development and launchSettings.json</span></span>

<span data-ttu-id="4e8f3-131">In der Entwicklungsumgebung können Features aktiviert werden, die in der Produktion nicht verfügbar gemacht werden sollten.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-131">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="4e8f3-132">Mit den ASP.NET Core-Vorlagen wird beispielsweise die [Seite mit Ausnahmen für Entwickler](xref:fundamentals/error-handling#developer-exception-page) in der Entwicklungsumgebung aktiviert.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-132">For example, the ASP.NET Core templates enable the [Developer Exception Page](xref:fundamentals/error-handling#developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="4e8f3-133">Die Umgebung für die Entwicklung lokaler Computer kann in der Datei *Properties\launchSettings.json* des Projekts festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-133">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="4e8f3-134">Mit in der Datei *launchSettings.json* festgelegten Umgebungsvariablen werden in der Systemumgebung festgelegte Werte überschrieben.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-134">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="4e8f3-135">Die Datei *launchSettings.json*</span><span class="sxs-lookup"><span data-stu-id="4e8f3-135">The *launchSettings.json* file:</span></span>

* <span data-ttu-id="4e8f3-136">wird nur auf dem lokalen Entwicklungscomputer verwendet.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-136">Is only used on the local development machine.</span></span>
* <span data-ttu-id="4e8f3-137">wird nicht bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-137">Is not deployed.</span></span>
* <span data-ttu-id="4e8f3-138">enthält Profileinstellungen.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-138">contains profile settings.</span></span>

<span data-ttu-id="4e8f3-139">Der folgende JSON-Code zeigt die Datei *launchSettings.json* für ein ASP.NET Core-Webprojekt mit dem Namen *EnvironmentsSample*, das mit Visual Studio oder `dotnet new` erstellt wurde:</span><span class="sxs-lookup"><span data-stu-id="4e8f3-139">The following JSON shows the *launchSettings.json* file for an ASP.NET Core web projected named *EnvironmentsSample* created with Visual Studio or `dotnet new`:</span></span>

[!code-json[](environments/3.1sample/EnvironmentsSample/Properties/launchSettingsCopy.json)]

<span data-ttu-id="4e8f3-140">Das vorangehende Markup enthält zwei Profile:</span><span class="sxs-lookup"><span data-stu-id="4e8f3-140">The preceding markup contains two profiles:</span></span>

* <span data-ttu-id="4e8f3-141">`IIS Express`: Das Standardprofil, das beim Starten der App in Visual Studio verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-141">`IIS Express`: The default profile used when launching the app from Visual Studio.</span></span> <span data-ttu-id="4e8f3-142">Da der Schlüssel `"commandName"` den Wert `"IISExpress"` besitzt, wird der Webserver [IISExpress](/iis/extensions/introduction-to-iis-express/iis-express-overview) verwendet.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-142">The `"commandName"` key has the value `"IISExpress"`, therefore, [IISExpress](/iis/extensions/introduction-to-iis-express/iis-express-overview) is the web server.</span></span> <span data-ttu-id="4e8f3-143">Sie können das Startprofil auf das Projekt oder ein beliebiges anderes Profil festlegen.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-143">You can set the launch profile to the project or any other profile included.</span></span> <span data-ttu-id="4e8f3-144">In der folgenden Abbildung wird etwa durch Auswählen des Projektnamens der [Kestrel-Webserver](xref:fundamentals/servers/kestrel) gestartet.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-144">For example, in the image below, selecting the project name launches the [Kestrel web server](xref:fundamentals/servers/kestrel).</span></span>

  ![Starten von IIS Express im Menü](environments/_static/iisx2.png)
* <span data-ttu-id="4e8f3-146">`EnvironmentsSample`: Der Profilname entspricht dem Projektnamen.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-146">`EnvironmentsSample`: The profile name is the project name.</span></span> <span data-ttu-id="4e8f3-147">Dies ist das Standardprofil beim Starten der App mit `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-147">This profile is used by default when launching the app with `dotnet run`.</span></span>  <span data-ttu-id="4e8f3-148">Da der Schlüssel `"commandName"` den Wert `"Project"` besitzt, wird der [Kestrel-Webserver](xref:fundamentals/servers/kestrel) gestartet.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-148">The `"commandName"` key has the value `"Project"`, therefore, the [Kestrel web server](xref:fundamentals/servers/kestrel) is launched.</span></span>

<span data-ttu-id="4e8f3-149">Mit dem Wert von `commandName` wird der zu startende Webserver angegeben.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-149">The value of `commandName` can specify the web server to launch.</span></span> <span data-ttu-id="4e8f3-150">`commandName` kann einer der folgenden sein:</span><span class="sxs-lookup"><span data-stu-id="4e8f3-150">`commandName` can be any one of the following:</span></span>

* <span data-ttu-id="4e8f3-151">`IISExpress` : Es wird IIS Express gestartet.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-151">`IISExpress` : Launches IIS Express.</span></span>
* <span data-ttu-id="4e8f3-152">`IIS` : Es wird kein Webserver gestartet.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-152">`IIS` : No web server launched.</span></span> <span data-ttu-id="4e8f3-153">Die Verfügbarkeit von IIS wird erwartet.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-153">IIS is expected to be available.</span></span>
* <span data-ttu-id="4e8f3-154">`Project` : Es wird Kestrel gestartet.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-154">`Project` : Launches Kestrel.</span></span>

<span data-ttu-id="4e8f3-155">Auf der Registerkarte **Debuggen** der Visual Studio-Projekteigenschaften wird eine grafische Benutzeroberfläche für die Bearbeitung der Datei *launchSettings.json* bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-155">The Visual Studio project properties **Debug** tab provides a GUI to edit the *launchSettings.json* file.</span></span> <span data-ttu-id="4e8f3-156">An Projektprofilen vorgenommene Änderungen werden möglicherweise erst nach einem Neustart des Webservers wirksam.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-156">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="4e8f3-157">Kestrel muss neu gestartet werden, bevor es an der Umgebung vorgenommene Änderungen erkennen kann.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-157">Kestrel must be restarted before it can detect changes made to its environment.</span></span>

![Projekteigenschaften zum Festlegen von Umgebungsvariablen](environments/_static/project-properties-debug.png)

<span data-ttu-id="4e8f3-159">Die folgende *launchSettings.json*-Datei enthält mehrere Profile:</span><span class="sxs-lookup"><span data-stu-id="4e8f3-159">The following *launchSettings.json* file contains multiple profiles:</span></span>

[!code-json[](environments/3.1sample/EnvironmentsSample/Properties/launchSettings.json)]

<span data-ttu-id="4e8f3-160">Profile können auf folgende Weise ausgewählt werden:</span><span class="sxs-lookup"><span data-stu-id="4e8f3-160">Profiles can be selected:</span></span>

* <span data-ttu-id="4e8f3-161">Über die Visual Studio-Benutzeroberfläche.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-161">From the Visual Studio UI.</span></span>
* <span data-ttu-id="4e8f3-162">Durch Verwenden des Befehls [`dotnet run`](/dotnet/core/tools/dotnet-run) in einer Befehlsshell, bei dem die Option `--launch-profile` auf den Profilnamen festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-162">Using the [`dotnet run`](/dotnet/core/tools/dotnet-run) command in a command shell with the `--launch-profile` option set to the profile's name.</span></span> <span data-ttu-id="4e8f3-163">*Bei dieser Vorgehensweise werden nur Kestrel-Profile unterstützt.*</span><span class="sxs-lookup"><span data-stu-id="4e8f3-163">*This approach only supports Kestrel profiles.*</span></span>

  ```dotnetcli
  dotnet run --launch-profile "SampleApp"
  ```

> [!WARNING]
> <span data-ttu-id="4e8f3-164">In der Datei *launchSettings.json* sollten keine geheimen Schlüssel gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-164">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="4e8f3-165">Mit dem [Secret Manager-Tool](xref:security/app-secrets) können geheime Schlüssel für die lokale Umgebung gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-165">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

<span data-ttu-id="4e8f3-166">Wenn Sie [Visual Studio Code](https://code.visualstudio.com/) verwenden, können Umgebungsvariablen in der Datei *.vscode/launch.json* festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-166">When using [Visual Studio Code](https://code.visualstudio.com/), environment variables can be set in the *.vscode/launch.json* file.</span></span> <span data-ttu-id="4e8f3-167">Im folgenden Beispiel werden mehrere Umgebungsvariablen für [Hostkonfigurationswerte](xref:fundamentals/host/web-host#host-configuration-values) festgelegt:</span><span class="sxs-lookup"><span data-stu-id="4e8f3-167">The following example sets several [Host configuration values environment variables](xref:fundamentals/host/web-host#host-configuration-values):</span></span>

[!code-json[](environments/3.1sample/EnvironmentsSample/.vscode/launch.json?range=4-10,32-38)]

<span data-ttu-id="4e8f3-168">Die Datei *.vscode/launch.json* wird nur von Visual Studio Code verwendet.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-168">The *.vscode/launch.json* file is only used by Visual Studio Code.</span></span>

### <a name="production"></a><span data-ttu-id="4e8f3-169">Produktion</span><span class="sxs-lookup"><span data-stu-id="4e8f3-169">Production</span></span>

<span data-ttu-id="4e8f3-170">Die Produktionsumgebung sollte so konfiguriert werden, dass eine maximale Sicherheit, [Leistung](xref:performance/performance-best-practices) und Stabilität der App erreicht werden.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-170">The production environment should be configured to maximize security, [performance](xref:performance/performance-best-practices), and application robustness.</span></span> <span data-ttu-id="4e8f3-171">Allgemeine Einstellungen, die sich von der Entwicklung unterscheiden, sind zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="4e8f3-171">Some common settings that differ from development include:</span></span>

* <span data-ttu-id="4e8f3-172">[Zwischenspeichern](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="4e8f3-172">[Caching](xref:performance/caching/memory).</span></span>
* <span data-ttu-id="4e8f3-173">Clientseitige Ressourcen werden gebündelt, verkleinert und potenziell von einem CDN bedient.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-173">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="4e8f3-174">Seiten zur Fehlerdiagnose sind deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-174">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="4e8f3-175">Angezeigte Fehlerseiten sind aktiviert.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-175">Friendly error pages enabled.</span></span>
* <span data-ttu-id="4e8f3-176">Die [Produktionsprotokollierung](xref:fundamentals/logging/index) und -überwachung sind aktiviert.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-176">Production [logging](xref:fundamentals/logging/index) and monitoring enabled.</span></span> <span data-ttu-id="4e8f3-177">Beispiel: [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="4e8f3-177">For example, using [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="4e8f3-178">Festlegen der Umgebung</span><span class="sxs-lookup"><span data-stu-id="4e8f3-178">Set the environment</span></span>

<span data-ttu-id="4e8f3-179">Häufig ist es sinnvoll, mit einer Umgebungsvariablen oder Plattformeinstellung eine bestimmte Umgebung für Tests festzulegen.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-179">It's often useful to set a specific environment for testing with an environment variable or platform setting.</span></span> <span data-ttu-id="4e8f3-180">Wenn die Umgebung nicht festgelegt ist, wird `Production` als Standardwert verwendet, wodurch die meisten Debugfeatures deaktiviert werden.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-180">If the environment isn't set, it defaults to `Production`, which disables most debugging features.</span></span> <span data-ttu-id="4e8f3-181">Welche Methode zum Festlegen der Umgebung verwendet wird, hängt vom Betriebssystem ab.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-181">The method for setting the environment depends on the operating system.</span></span>

<span data-ttu-id="4e8f3-182">Wenn der Host erstellt wird, bestimmt die letzte von der App gelesene Umgebungseinstellung die Umgebung der App.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-182">When the host is built, the last environment setting read by the app determines the app's environment.</span></span> <span data-ttu-id="4e8f3-183">Die Umgebung der App kann nicht geändert werden, während die App ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-183">The app's environment can't be changed while the app is running.</span></span>

<span data-ttu-id="4e8f3-184">Auf der [Infoseite](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/3.1sample/EnvironmentsSample/Pages/About.cshtml) des [Beispielcodes](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/3.1sample) wird der Wert von `IWebHostEnvironment.EnvironmentName` angezeigt.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-184">The [About page](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/3.1sample/EnvironmentsSample/Pages/About.cshtml) from the [sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/3.1sample) displays the value of `IWebHostEnvironment.EnvironmentName`.</span></span>

### <a name="azure-app-service"></a><span data-ttu-id="4e8f3-185">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="4e8f3-185">Azure App Service</span></span>

<span data-ttu-id="4e8f3-186">Führen Sie die folgenden Schritte durch, um die Umgebung in [Azure App Service](https://azure.microsoft.com/services/app-service/) festzulegen:</span><span class="sxs-lookup"><span data-stu-id="4e8f3-186">To set the environment in [Azure App Service](https://azure.microsoft.com/services/app-service/), perform the following steps:</span></span>

1. <span data-ttu-id="4e8f3-187">Wählen Sie die App auf dem Blatt **App Services** aus.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-187">Select the app from the **App Services** blade.</span></span>
1. <span data-ttu-id="4e8f3-188">Wählen Sie in der Gruppe **Einstellungen** das Blatt **Konfiguration** aus.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-188">In the **Settings** group, select the **Configuration** blade.</span></span>
1. <span data-ttu-id="4e8f3-189">Wählen Sie auf der Registerkarte **Anwendungseinstellungen** **Neue Anwendungseinstellung** aus.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-189">In the **Application settings** tab, select **New application setting**.</span></span>
1. <span data-ttu-id="4e8f3-190">Geben Sie im Fenster **Anwendungseinstellung hinzufügen/bearbeiten** `ASPNETCORE_ENVIRONMENT` als **Namen** an.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-190">In the **Add/Edit application setting** window, provide `ASPNETCORE_ENVIRONMENT` for the **Name**.</span></span> <span data-ttu-id="4e8f3-191">Geben Sie als **Wert** die Umgebung ein (beispielsweise `Staging`).</span><span class="sxs-lookup"><span data-stu-id="4e8f3-191">For **Value**, provide the environment (for example, `Staging`).</span></span>
1. <span data-ttu-id="4e8f3-192">Aktivieren Sie das Kontrollkästchen **Bereitstellungssloteinstellung**, wenn Sie möchten, dass die Umgebungseinstellung im aktuellen Slot bleibt, wenn Bereitstellungsslots getauscht werden.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-192">Select the **Deployment slot setting** check box if you wish the environment setting to remain with the current slot when deployment slots are swapped.</span></span> <span data-ttu-id="4e8f3-193">Weitere Informationen finden Sie unter [Einrichten von Stagingumgebungen in Azure App Service](/azure/app-service/web-sites-staged-publishing) in der Azure-Dokumentation.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-193">For more information, see [Set up staging environments in Azure App Service](/azure/app-service/web-sites-staged-publishing) in the Azure documentation.</span></span>
1. <span data-ttu-id="4e8f3-194">Wählen Sie **OK** aus, um das Fenster **Anwendungseinstellung hinzufügen/bearbeiten** zu schließen.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-194">Select **OK** to close the **Add/Edit application setting** window.</span></span>
1. <span data-ttu-id="4e8f3-195">Klicken Sie oben auf dem Blatt **Konfiguration** auf **Speichern**.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-195">Select **Save** at the top of the **Configuration** blade.</span></span>

<span data-ttu-id="4e8f3-196">Azure App Service startet die App automatisch neu, nachdem eine App-Einstellung im Azure-Portal hinzugefügt, geändert oder gelöscht wurde.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-196">Azure App Service automatically restarts the app after an app setting is added, changed, or deleted in the Azure portal.</span></span>

### <a name="windows"></a><span data-ttu-id="4e8f3-197">Windows</span><span class="sxs-lookup"><span data-stu-id="4e8f3-197">Windows</span></span>

<span data-ttu-id="4e8f3-198">Umgebungsvariablen in der Datei *launchSettings.json* überschreiben in der Systemumgebung festgelegte Werte.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-198">Environment values in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="4e8f3-199">Zum Festlegen der `ASPNETCORE_ENVIRONMENT` für die aktuelle Sitzung werden folgende Befehle verwendet, wenn die App mit [dotnet run](/dotnet/core/tools/dotnet-run) gestartet wird:</span><span class="sxs-lookup"><span data-stu-id="4e8f3-199">To set the `ASPNETCORE_ENVIRONMENT` for the current session when the app is started using [dotnet run](/dotnet/core/tools/dotnet-run), the following commands are used:</span></span>

<span data-ttu-id="4e8f3-200">**Eingabeaufforderung**</span><span class="sxs-lookup"><span data-stu-id="4e8f3-200">**Command prompt**</span></span>

```console
set ASPNETCORE_ENVIRONMENT=Staging
dotnet run --no-launch-profile
```

<span data-ttu-id="4e8f3-201">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="4e8f3-201">**PowerShell**</span></span>

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Staging"
dotnet run --no-launch-profile
```

<span data-ttu-id="4e8f3-202">Der vorangehende Befehl legt `ASPNETCORE_ENVIRONMENT` nur für Prozesse fest, die von diesem Befehlsfenster aus gestartet werden.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-202">The preceding command sets `ASPNETCORE_ENVIRONMENT` only for processes launched from that command window.</span></span>

<span data-ttu-id="4e8f3-203">Wenn Sie den Wert in Windows global festlegen möchten, nutzen Sie eine der folgenden Möglichkeiten:</span><span class="sxs-lookup"><span data-stu-id="4e8f3-203">To set the value globally in Windows, use either of the following approaches:</span></span>

* <span data-ttu-id="4e8f3-204">Öffnen Sie **Systemsteuerung** > **System** > **Erweiterte Systemeinstellungen**, und fügen Sie den Wert `ASPNETCORE_ENVIRONMENT` hinzu, oder bearbeiten Sie ihn:</span><span class="sxs-lookup"><span data-stu-id="4e8f3-204">Open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value:</span></span>

  ![Erweiterte Systemeigenschaften](environments/_static/systemsetting_environment.png)

  ![ASP.NET Core-Umgebungsvariable](environments/_static/windows_aspnetcore_environment.png)

* <span data-ttu-id="4e8f3-207">Führen Sie die Eingabeaufforderung als Administrator aus, und verwenden Sie den Befehl `setx`. Alternativ können Sie auch die PowerShell-Eingabeaufforderung als Administrator ausführen und `[Environment]::SetEnvironmentVariable` nutzen:</span><span class="sxs-lookup"><span data-stu-id="4e8f3-207">Open an administrative command prompt and use the `setx` command or open an administrative PowerShell command prompt and use `[Environment]::SetEnvironmentVariable`:</span></span>

  <span data-ttu-id="4e8f3-208">**Eingabeaufforderung**</span><span class="sxs-lookup"><span data-stu-id="4e8f3-208">**Command prompt**</span></span>

  ```console
  setx ASPNETCORE_ENVIRONMENT Staging /M
  ```

  <span data-ttu-id="4e8f3-209">Mit dem Parameter `/M` wird angegeben, dass die Umgebungsvariable auf Systemebene festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-209">The `/M` switch indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="4e8f3-210">Ohne den Parameter `/M` wird die Umgebungsvariable für das Nutzerkonto festgelegt.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-210">If the `/M` switch isn't used, the environment variable is set for the user account.</span></span>

  <span data-ttu-id="4e8f3-211">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="4e8f3-211">**PowerShell**</span></span>

  ```powershell
  [Environment]::SetEnvironmentVariable("ASPNETCORE_ENVIRONMENT", "Staging", "Machine")
  ```

  <span data-ttu-id="4e8f3-212">Mit dem Optionswert `Machine` wird angegeben, dass die Umgebungsvariable auf Systemebene festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-212">The `Machine` option value indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="4e8f3-213">Verwenden Sie stattdessen den Optionswert `User`, wird die Umgebungsvariable für das Nutzerkonto festgelegt.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-213">If the option value is changed to `User`, the environment variable is set for the user account.</span></span>

<span data-ttu-id="4e8f3-214">Sobald die Umgebungsvariable `ASPNETCORE_ENVIRONMENT` global festgelegt wird, gilt sie in jedem geöffneten Befehlsfenster für `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-214">When the `ASPNETCORE_ENVIRONMENT` environment variable is set globally, it takes effect for `dotnet run` in any command window opened after the value is set.</span></span> <span data-ttu-id="4e8f3-215">Umgebungsvariablen in der Datei *launchSettings.json* überschreiben in der Systemumgebung festgelegte Werte.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-215">Environment values in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="4e8f3-216">**web.config**</span><span class="sxs-lookup"><span data-stu-id="4e8f3-216">**web.config**</span></span>

<span data-ttu-id="4e8f3-217">Eine Anleitung zum Festlegen der Umgebungsvariable `ASPNETCORE_ENVIRONMENT` mit *web.config* finden Sie im Abschnitt *Festlegen von Umgebungsvariablen* der im Artikel zu <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-217">To set the `ASPNETCORE_ENVIRONMENT` environment variable with *web.config*, see the *Setting environment variables* section of <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.</span></span>

<span data-ttu-id="4e8f3-218">**Projektdatei oder Veröffentlichungsprofil**</span><span class="sxs-lookup"><span data-stu-id="4e8f3-218">**Project file or publish profile**</span></span>

<span data-ttu-id="4e8f3-219">**Für Windows IIS-Bereitstellungen**: Beziehen Sie die `<EnvironmentName>`-Eigenschaft in das [Veröffentlichungsprofil (PUBXML)](xref:host-and-deploy/visual-studio-publish-profiles) oder die Projektdatei ein.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-219">**For Windows IIS deployments:** Include the `<EnvironmentName>` property in the [publish profile (.pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) or project file.</span></span> <span data-ttu-id="4e8f3-220">Dieser Ansatz legt die Umgebung in *web.config* fest, wenn das Projekt veröffentlicht wird:</span><span class="sxs-lookup"><span data-stu-id="4e8f3-220">This approach sets the environment in *web.config* when the project is published:</span></span>

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

<span data-ttu-id="4e8f3-221">**Pro IIS-Anwendungspool**</span><span class="sxs-lookup"><span data-stu-id="4e8f3-221">**Per IIS Application Pool**</span></span>

<span data-ttu-id="4e8f3-222">Wenn Sie für eine App, die in einem isolierten Anwendungspool ausgeführt wird, die Umgebungsvariable `ASPNETCORE_ENVIRONMENT` festlegen möchten (unterstützt in IIS 10.0 oder höher), lesen Sie den Abschnitt *AppCmd.exe command (Befehl „AppCmd.exe“)* im Artikel zu [Umgebungsvariablen&lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe).</span><span class="sxs-lookup"><span data-stu-id="4e8f3-222">To set the `ASPNETCORE_ENVIRONMENT` environment variable for an app running in an isolated Application Pool (supported on IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span> <span data-ttu-id="4e8f3-223">Wurde die Umgebungsvariable `ASPNETCORE_ENVIRONMENT` für einen Anwendungspool festgelegt, überschreibt der Wert die Einstellung auf Systemebene.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-223">When the `ASPNETCORE_ENVIRONMENT` environment variable is set for an app pool, its value overrides a setting at the system level.</span></span>

<span data-ttu-id="4e8f3-224">Wenn Sie eine APP in den IIS hosten und die Umgebungsvariable `ASPNETCORE_ENVIRONMENT` hinzufügen oder ändern, sorgen Sie auf eine der folgenden Arten und Weisen dafür, dass der neue Wert in den Apps hinterlegt ist:</span><span class="sxs-lookup"><span data-stu-id="4e8f3-224">When hosting an app in IIS and adding or changing the `ASPNETCORE_ENVIRONMENT` environment variable, use any one of the following approaches to have the new value picked up by apps:</span></span>

* <span data-ttu-id="4e8f3-225">Führen Sie in einer Eingabeaufforderung `net stop was /y` gefolgt von `net start w3svc` aus.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-225">Execute `net stop was /y` followed by `net start w3svc` from a command prompt.</span></span>
* <span data-ttu-id="4e8f3-226">Starten Sie den Server neu.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-226">Restart the server.</span></span>

#### <a name="macos"></a><span data-ttu-id="4e8f3-227">macOS</span><span class="sxs-lookup"><span data-stu-id="4e8f3-227">macOS</span></span>

<span data-ttu-id="4e8f3-228">Das Festlegen der aktuellen Umgebung für macOS kann inline während der Ausführung der App erfolgen:</span><span class="sxs-lookup"><span data-stu-id="4e8f3-228">Setting the current environment for macOS can be performed in-line when running the app:</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Staging dotnet run
```

<span data-ttu-id="4e8f3-229">Legen Sie die Umgebung alternativ mit `export` vor der Ausführung der App fest:</span><span class="sxs-lookup"><span data-stu-id="4e8f3-229">Alternatively, set the environment with `export` prior to running the app:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Staging
```

<span data-ttu-id="4e8f3-230">Umgebungsvariablen auf Computerebene werden in der *BASHRC*- oder *BASH_PROFILE*-Datei festgelegt.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-230">Machine-level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="4e8f3-231">Bearbeiten Sie die Datei mit einem beliebigen Text-Editor.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-231">Edit the file using any text editor.</span></span> <span data-ttu-id="4e8f3-232">Fügen Sie die folgende Anweisung hinzu:</span><span class="sxs-lookup"><span data-stu-id="4e8f3-232">Add the following statement:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Staging
```

#### <a name="linux"></a><span data-ttu-id="4e8f3-233">Linux</span><span class="sxs-lookup"><span data-stu-id="4e8f3-233">Linux</span></span>

<span data-ttu-id="4e8f3-234">Verwenden Sie bei Linux-Distributionen für sitzungsbasierte Variableneinstellungen den Befehl `export` in der Eingabeaufforderung und die Datei *bash_profile* für Umgebungseinstellungen auf Computerebene.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-234">For Linux distributions, use the `export` command at a command prompt for session-based variable settings and *bash_profile* file for machine-level environment settings.</span></span>

### <a name="set-the-environment-in-code"></a><span data-ttu-id="4e8f3-235">Festlegen der Umgebung im Code</span><span class="sxs-lookup"><span data-stu-id="4e8f3-235">Set the environment in code</span></span>

<span data-ttu-id="4e8f3-236">Rufen Sie beim Erstellen des Hosts <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseEnvironment*> auf.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-236">Call <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseEnvironment*> when building the host.</span></span> <span data-ttu-id="4e8f3-237">Siehe <xref:fundamentals/host/generic-host#environmentname>.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-237">See <xref:fundamentals/host/generic-host#environmentname>.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="4e8f3-238">Konfiguration nach Umgebung</span><span class="sxs-lookup"><span data-stu-id="4e8f3-238">Configuration by environment</span></span>

<span data-ttu-id="4e8f3-239">Informationen zum Laden der Konfiguration nach Umgebung finden Sie unter <xref:fundamentals/configuration/index#json-configuration-provider>.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-239">To load configuration by environment, see <xref:fundamentals/configuration/index#json-configuration-provider>.</span></span>

## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="4e8f3-240">Umgebungsbasierte Startklasse und Methoden</span><span class="sxs-lookup"><span data-stu-id="4e8f3-240">Environment-based Startup class and methods</span></span>

### <a name="inject-iwebhostenvironment-into-the-startup-class"></a><span data-ttu-id="4e8f3-241">Einfügen von IWebHostEnvironment in die Startklasse</span><span class="sxs-lookup"><span data-stu-id="4e8f3-241">Inject IWebHostEnvironment into the Startup class</span></span>

<span data-ttu-id="4e8f3-242">Fügen Sie <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> in den `Startup`-Konstruktor ein.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-242">Inject <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> into the `Startup` constructor.</span></span> <span data-ttu-id="4e8f3-243">Diese Vorgehensweise ist nützlich, wenn die App `Startup` nur für einige Umgebungen mit minimalen Codeunterschieden pro Umgebung konfigurieren muss.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-243">This approach is useful when the app requires configuring `Startup` for only a few environments with minimal code differences per environment.</span></span>

<span data-ttu-id="4e8f3-244">Im folgenden Beispiel:</span><span class="sxs-lookup"><span data-stu-id="4e8f3-244">In the following example:</span></span>

* <span data-ttu-id="4e8f3-245">Die Umgebung wird im Feld `_env` gespeichert.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-245">The environment is held in the `_env` field.</span></span>
* <span data-ttu-id="4e8f3-246">`_env` wird in `ConfigureServices` und `Configure` verwendet, um die Startkonfiguration basierend auf der Umgebung der App anzuwenden.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-246">`_env` is used in `ConfigureServices` and `Configure` to apply startup configuration based on the app's environment.</span></span>

[!code-csharp[](environments/3.1sample/EnvironmentsSample/StartupInject.cs?name=snippet&highlight=3-11)]

### <a name="startup-class-conventions"></a><span data-ttu-id="4e8f3-247">Konventionen der Startup-Klasse</span><span class="sxs-lookup"><span data-stu-id="4e8f3-247">Startup class conventions</span></span>

<span data-ttu-id="4e8f3-248">Nach dem Start einer ASP.NET Core-App lädt die [Startklasse](xref:fundamentals/startup) die App.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-248">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="4e8f3-249">Die App kann je nach Umgebung mehrere `Startup`-Klassen definieren.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-249">The app can define multiple `Startup` classes for different environments.</span></span> <span data-ttu-id="4e8f3-250">Die entsprechende `Startup`-Klasse wird zur Laufzeit ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-250">The appropriate `Startup` class is selected at runtime.</span></span> <span data-ttu-id="4e8f3-251">Die Klasse, deren Namenssuffix mit der aktuellen Umgebung übereinstimmt, wird priorisiert.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-251">The class whose name suffix matches the current environment is prioritized.</span></span> <span data-ttu-id="4e8f3-252">Ist keine übereinstimmende `Startup{EnvironmentName}`-Klasse vorhanden, wird die Klasse `Startup` verwendet.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-252">If a matching `Startup{EnvironmentName}` class isn't found, the `Startup` class is used.</span></span> <span data-ttu-id="4e8f3-253">Diese Vorgehensweise ist nützlich, wenn die App „Startup“ für mehrere Umgebungen mit vielen Codeunterschieden pro Umgebung konfigurieren muss.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-253">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span> <span data-ttu-id="4e8f3-254">Für typische Apps ist diese Vorgehensweise nicht erforderlich.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-254">Typical apps will not need this approach.</span></span>

<span data-ttu-id="4e8f3-255">Wenn Sie umgebungsbasierte `Startup`-Klassen implementieren möchten, erstellen Sie eine `Startup{EnvironmentName}`-Klasse und eine Fallback-Klasse vom Typ `Startup`:</span><span class="sxs-lookup"><span data-stu-id="4e8f3-255">To implement environment-based `Startup` classes, create a `Startup{EnvironmentName}` classes and a fallback `Startup` class:</span></span>

[!code-csharp[](environments/3.1sample/EnvironmentsSample/StartupClassConventions.cs?name=snippet)]

<span data-ttu-id="4e8f3-256">Verwenden Sie die Überladung [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup), die einen Assemblynamen akzeptiert:</span><span class="sxs-lookup"><span data-stu-id="4e8f3-256">Use the [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) overload that accepts an assembly name:</span></span>

[!code-csharp[](environments/3.1sample/EnvironmentsSample/Program.cs?name=snippet)]

### <a name="startup-method-conventions"></a><span data-ttu-id="4e8f3-257">Konventionen der Methode „Start“</span><span class="sxs-lookup"><span data-stu-id="4e8f3-257">Startup method conventions</span></span>

<span data-ttu-id="4e8f3-258">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) und [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) unterstützen umgebungsspezifische Versionen der Form `Configure<EnvironmentName>` und `Configure<EnvironmentName>Services`.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-258">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) support environment-specific versions of the form `Configure<EnvironmentName>` and `Configure<EnvironmentName>Services`.</span></span> <span data-ttu-id="4e8f3-259">Diese Vorgehensweise eignet sich für Apps, bei denen Startoptionen für mehrere Umgebungen mit vielen Codeunterschieden pro Umgebung konfiguriert werden müssen:</span><span class="sxs-lookup"><span data-stu-id="4e8f3-259">This approach is useful when the app requires configuring startup for several environments with many code differences per environment:</span></span>

[!code-csharp[](environments/3.1sample/EnvironmentsSample/StartupMethodConventions.cs?name=snippet)]

## <a name="additional-resources"></a><span data-ttu-id="4e8f3-260">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="4e8f3-260">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>
* <xref:blazor/fundamentals/environments>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="4e8f3-261">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="4e8f3-261">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="4e8f3-262">ASP.NET Core konfiguriert das App-Verhalten basierend auf der Laufzeitumgebung mit einer Umgebungsvariablen.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-262">ASP.NET Core configures app behavior based on the runtime environment using an environment variable.</span></span>

<span data-ttu-id="4e8f3-263">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([Vorgehensweise zum Herunterladen](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4e8f3-263">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="4e8f3-264">Umgebungen</span><span class="sxs-lookup"><span data-stu-id="4e8f3-264">Environments</span></span>

<span data-ttu-id="4e8f3-265">ASP.NET Core liest die Umgebungsvariable `ASPNETCORE_ENVIRONMENT` beim Start der App und speichert diesen Wert unter [IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName).</span><span class="sxs-lookup"><span data-stu-id="4e8f3-265">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at app startup and stores the value in [IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName).</span></span> <span data-ttu-id="4e8f3-266">`ASPNETCORE_ENVIRONMENT` kann auf einen beliebigen Wert festgelegt werden, das Framework stellt allerdings nur drei Werte bereit:</span><span class="sxs-lookup"><span data-stu-id="4e8f3-266">`ASPNETCORE_ENVIRONMENT` can be set to any value, but three values are provided by the framework:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>
* <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Staging>
* <span data-ttu-id="4e8f3-267"><xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Production> (Standardwert)</span><span class="sxs-lookup"><span data-stu-id="4e8f3-267"><xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Production> (default)</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet)]

<span data-ttu-id="4e8f3-268">Der vorangehende Code:</span><span class="sxs-lookup"><span data-stu-id="4e8f3-268">The preceding code:</span></span>

* <span data-ttu-id="4e8f3-269">Ruft [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) auf, wenn `ASPNETCORE_ENVIRONMENT` auf `Development` festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-269">Calls [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="4e8f3-270">Ruft [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) auf, wenn der Wert von `ASPNETCORE_ENVIRONMENT` auf einen der folgenden Werte festgelegt ist:</span><span class="sxs-lookup"><span data-stu-id="4e8f3-270">Calls [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

  * `Staging`
  * `Production`
  * `Staging_2`

<span data-ttu-id="4e8f3-271">Das [Umgebungstaghilfsprogramm](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) verwendet den Wert von `IHostingEnvironment.EnvironmentName` zum Einschließen oder Ausschließen von Markup im Element:</span><span class="sxs-lookup"><span data-stu-id="4e8f3-271">The [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

<span data-ttu-id="4e8f3-272">Unter Windows und macOS wird bei Umgebungsvariablen und Werten die Groß-/Kleinschreibung nicht beachtet.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-272">On Windows and macOS, environment variables and values aren't case-sensitive.</span></span> <span data-ttu-id="4e8f3-273">Bei Linux-Umgebungsvariablen und -Werten wird die Groß-/Kleinschreibung standardmäßig beachtet.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-273">Linux environment variables and values are case-sensitive by default.</span></span>

### <a name="development"></a><span data-ttu-id="4e8f3-274">Entwicklung</span><span class="sxs-lookup"><span data-stu-id="4e8f3-274">Development</span></span>

<span data-ttu-id="4e8f3-275">In der Entwicklungsumgebung können Features aktiviert werden, die in der Produktion nicht verfügbar gemacht werden sollten.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-275">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="4e8f3-276">Mit den ASP.NET Core-Vorlagen wird beispielsweise die [Seite mit Ausnahmen für Entwickler](xref:fundamentals/error-handling#developer-exception-page) in der Entwicklungsumgebung aktiviert.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-276">For example, the ASP.NET Core templates enable the [Developer Exception Page](xref:fundamentals/error-handling#developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="4e8f3-277">Die Umgebung für die Entwicklung lokaler Computer kann in der Datei *Properties\launchSettings.json* des Projekts festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-277">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="4e8f3-278">Mit in der Datei *launchSettings.json* festgelegten Umgebungsvariablen werden in der Systemumgebung festgelegte Werte überschrieben.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-278">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="4e8f3-279">Die folgende JSON zeigt drei Profile aus der Datei *launchSettings.json* an:</span><span class="sxs-lookup"><span data-stu-id="4e8f3-279">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

```json
{
  "iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iisExpress": {
      "applicationUrl": "http://localhost:54339/",
      "sslPort": 0
    }
  },
  "profiles": {
    "IIS Express": {
      "commandName": "IISExpress",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_My_Environment": "1",
        "ASPNETCORE_DETAILEDERRORS": "1",
        "ASPNETCORE_ENVIRONMENT": "Staging"
      }
    },
    "EnvironmentsSample": {
      "commandName": "Project",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Staging"
      },
      "applicationUrl": "http://localhost:54340/"
    },
    "Kestrel Staging": {
      "commandName": "Project",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_My_Environment": "1",
        "ASPNETCORE_DETAILEDERRORS": "1",
        "ASPNETCORE_ENVIRONMENT": "Staging"
      },
      "applicationUrl": "http://localhost:51997/"
    }
  }
}
```

> [!NOTE]
> <span data-ttu-id="4e8f3-280">Die Eigenschaft `applicationUrl` in *launchSettings.json* kann eine Liste von Server-URLs angeben.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-280">The `applicationUrl` property in *launchSettings.json* can specify a list of server URLs.</span></span> <span data-ttu-id="4e8f3-281">Verwenden Sie ein Semikolon zwischen den URLs in der Liste:</span><span class="sxs-lookup"><span data-stu-id="4e8f3-281">Use a semicolon between the URLs in the list:</span></span>
>
> ```json
> "EnvironmentsSample": {
>    "commandName": "Project",
>    "launchBrowser": true,
>    "applicationUrl": "https://localhost:5001;http://localhost:5000",
>    "environmentVariables": {
>      "ASPNETCORE_ENVIRONMENT": "Development"
>    }
> }
> ```

<span data-ttu-id="4e8f3-282">Wenn die Anwendung mit [dotnet run](/dotnet/core/tools/dotnet-run) gestartet wird, wird das erste Profil mit `"commandName": "Project"` verwendet.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-282">When the app is launched with [dotnet run](/dotnet/core/tools/dotnet-run), the first profile with `"commandName": "Project"` is used.</span></span> <span data-ttu-id="4e8f3-283">Der Wert von `commandName` gibt den zu startenden Webserver an.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-283">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="4e8f3-284">`commandName` kann einer der folgenden sein:</span><span class="sxs-lookup"><span data-stu-id="4e8f3-284">`commandName` can be any one of the following:</span></span>

* `IISExpress`
* `IIS`
* <span data-ttu-id="4e8f3-285">`Project` (über das Kestrel gestartet wird)</span><span class="sxs-lookup"><span data-stu-id="4e8f3-285">`Project` (which launches Kestrel)</span></span>

<span data-ttu-id="4e8f3-286">Beim Start einer App mit [dotnet run](/dotnet/core/tools/dotnet-run) tritt Folgendes ein:</span><span class="sxs-lookup"><span data-stu-id="4e8f3-286">When an app is launched with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

* <span data-ttu-id="4e8f3-287">Die Datei *launchSettings.json* wird, sofern verfügbar, gelesen.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-287">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="4e8f3-288">Durch `environmentVariables`-Einstellungen in der Datei *launchSettings.json* werden Umgebungsvariablen überschrieben.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-288">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="4e8f3-289">Die Hostingumgebung wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-289">The hosting environment is displayed.</span></span>

<span data-ttu-id="4e8f3-290">Die folgende Ausgabe zeigt eine App, die mit [dotnet run](/dotnet/core/tools/dotnet-run) gestartet wurde:</span><span class="sxs-lookup"><span data-stu-id="4e8f3-290">The following output shows an app started with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

```bash
PS C:\Websites\EnvironmentsSample> dotnet run
Using launch settings from C:\Websites\EnvironmentsSample\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Websites\EnvironmentsSample
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="4e8f3-291">Auf der Registerkarte **Debuggen** der Visual Studio-Projekteigenschaften wird eine grafische Benutzeroberfläche für die Bearbeitung der Datei *launchSettings.json* bereitgestellt:</span><span class="sxs-lookup"><span data-stu-id="4e8f3-291">The Visual Studio project properties **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![Projekteigenschaften zum Festlegen von Umgebungsvariablen](environments/_static/project-properties-debug.png)

<span data-ttu-id="4e8f3-293">An Projektprofilen vorgenommene Änderungen werden möglicherweise erst nach einem Neustart des Webservers wirksam.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-293">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="4e8f3-294">Kestrel muss neu gestartet werden, bevor es an der Umgebung vorgenommene Änderungen erkennen kann.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-294">Kestrel must be restarted before it can detect changes made to its environment.</span></span>

> [!WARNING]
> <span data-ttu-id="4e8f3-295">In der Datei *launchSettings.json* sollten keine geheimen Schlüssel gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-295">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="4e8f3-296">Mit dem [Secret Manager-Tool](xref:security/app-secrets) können geheime Schlüssel für die lokale Umgebung gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-296">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

<span data-ttu-id="4e8f3-297">Wenn Sie [Visual Studio Code](https://code.visualstudio.com/) verwenden, können Umgebungsvariablen in der Datei *.vscode/launch.json* festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-297">When using [Visual Studio Code](https://code.visualstudio.com/), environment variables can be set in the *.vscode/launch.json* file.</span></span> <span data-ttu-id="4e8f3-298">Im folgenden Beispiel wird die Umgebung auf `Development` festgelegt:</span><span class="sxs-lookup"><span data-stu-id="4e8f3-298">The following example sets the environment to `Development`:</span></span>

```json
{
   "version": "0.2.0",
   "configurations": [
        {
            "name": ".NET Core Launch (web)",

            ... additional VS Code configuration settings ...

            "env": {
                "ASPNETCORE_ENVIRONMENT": "Development"
            }
        }
    ]
}
```

<span data-ttu-id="4e8f3-299">Die Datei *.vscode/launch.json* im Projekt wird nicht gelesen, wenn die App mit `dotnet run` genauso wie *Properties/launchSettings.json* gestartet wird.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-299">A *.vscode/launch.json* file in the project isn't read when starting the app with `dotnet run` in the same way as *Properties/launchSettings.json*.</span></span> <span data-ttu-id="4e8f3-300">Wenn Sie eine App in der Entwicklung starten, die keine *launchSettings.json*-Datei enthält, legen Sie die Umgebung mit einer Umgebungsvariablen oder einem Befehlszeilenargument auf den `dotnet run`-Befehl fest.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-300">When launching an app in development that doesn't have a *launchSettings.json* file, either set the environment with an environment variable or a command-line argument to the `dotnet run` command.</span></span>

### <a name="production"></a><span data-ttu-id="4e8f3-301">Produktion</span><span class="sxs-lookup"><span data-stu-id="4e8f3-301">Production</span></span>

<span data-ttu-id="4e8f3-302">Die Produktionsumgebung sollte so konfiguriert werden, dass Sicherheit, Leistung und Stabilität der App maximiert werden.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-302">The production environment should be configured to maximize security, performance, and app robustness.</span></span> <span data-ttu-id="4e8f3-303">Allgemeine Einstellungen, die sich von der Entwicklung unterscheiden, sind zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="4e8f3-303">Some common settings that differ from development include:</span></span>

* <span data-ttu-id="4e8f3-304">Zwischenspeicherung.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-304">Caching.</span></span>
* <span data-ttu-id="4e8f3-305">Clientseitige Ressourcen werden gebündelt, verkleinert und potenziell von einem CDN bedient.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-305">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="4e8f3-306">Seiten zur Fehlerdiagnose sind deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-306">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="4e8f3-307">Angezeigte Fehlerseiten sind aktiviert.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-307">Friendly error pages enabled.</span></span>
* <span data-ttu-id="4e8f3-308">Die Produktionsprotokollierung und -überwachung ist aktiviert.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-308">Production logging and monitoring enabled.</span></span> <span data-ttu-id="4e8f3-309">Beispiel: [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="4e8f3-309">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="4e8f3-310">Festlegen der Umgebung</span><span class="sxs-lookup"><span data-stu-id="4e8f3-310">Set the environment</span></span>

<span data-ttu-id="4e8f3-311">Häufig ist es sinnvoll, mit einer Umgebungsvariablen oder Plattformeinstellung eine bestimmte Umgebung für Tests festzulegen.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-311">It's often useful to set a specific environment for testing with an environment variable or platform setting.</span></span> <span data-ttu-id="4e8f3-312">Wenn die Umgebung nicht festgelegt ist, wird `Production` als Standardwert verwendet, wodurch die meisten Debugfeatures deaktiviert werden.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-312">If the environment isn't set, it defaults to `Production`, which disables most debugging features.</span></span> <span data-ttu-id="4e8f3-313">Welche Methode zum Festlegen der Umgebung verwendet wird, hängt vom Betriebssystem ab.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-313">The method for setting the environment depends on the operating system.</span></span>

<span data-ttu-id="4e8f3-314">Wenn der Host erstellt wird, bestimmt die letzte von der App gelesene Umgebungseinstellung die Umgebung der App.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-314">When the host is built, the last environment setting read by the app determines the app's environment.</span></span> <span data-ttu-id="4e8f3-315">Die Umgebung der App kann nicht geändert werden, während die App ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-315">The app's environment can't be changed while the app is running.</span></span>

### <a name="environment-variable-or-platform-setting"></a><span data-ttu-id="4e8f3-316">Umgebungsvariable oder Plattformeinstellung</span><span class="sxs-lookup"><span data-stu-id="4e8f3-316">Environment variable or platform setting</span></span>

#### <a name="azure-app-service"></a><span data-ttu-id="4e8f3-317">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="4e8f3-317">Azure App Service</span></span>

<span data-ttu-id="4e8f3-318">Führen Sie die folgenden Schritte durch, um die Umgebung in [Azure App Service](https://azure.microsoft.com/services/app-service/) festzulegen:</span><span class="sxs-lookup"><span data-stu-id="4e8f3-318">To set the environment in [Azure App Service](https://azure.microsoft.com/services/app-service/), perform the following steps:</span></span>

1. <span data-ttu-id="4e8f3-319">Wählen Sie die App auf dem Blatt **App Services** aus.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-319">Select the app from the **App Services** blade.</span></span>
1. <span data-ttu-id="4e8f3-320">Wählen Sie in der Gruppe **Einstellungen** das Blatt **Konfiguration** aus.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-320">In the **Settings** group, select the **Configuration** blade.</span></span>
1. <span data-ttu-id="4e8f3-321">Wählen Sie auf der Registerkarte **Anwendungseinstellungen** **Neue Anwendungseinstellung** aus.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-321">In the **Application settings** tab, select **New application setting**.</span></span>
1. <span data-ttu-id="4e8f3-322">Geben Sie im Fenster **Anwendungseinstellung hinzufügen/bearbeiten** `ASPNETCORE_ENVIRONMENT` als **Namen** an.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-322">In the **Add/Edit application setting** window, provide `ASPNETCORE_ENVIRONMENT` for the **Name**.</span></span> <span data-ttu-id="4e8f3-323">Geben Sie als **Wert** die Umgebung ein (beispielsweise `Staging`).</span><span class="sxs-lookup"><span data-stu-id="4e8f3-323">For **Value**, provide the environment (for example, `Staging`).</span></span>
1. <span data-ttu-id="4e8f3-324">Aktivieren Sie das Kontrollkästchen **Bereitstellungssloteinstellung**, wenn Sie möchten, dass die Umgebungseinstellung im aktuellen Slot bleibt, wenn Bereitstellungsslots getauscht werden.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-324">Select the **Deployment slot setting** check box if you wish the environment setting to remain with the current slot when deployment slots are swapped.</span></span> <span data-ttu-id="4e8f3-325">Weitere Informationen finden Sie unter [Einrichten von Stagingumgebungen in Azure App Service](/azure/app-service/web-sites-staged-publishing) in der Azure-Dokumentation.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-325">For more information, see [Set up staging environments in Azure App Service](/azure/app-service/web-sites-staged-publishing) in the Azure documentation.</span></span>
1. <span data-ttu-id="4e8f3-326">Wählen Sie **OK** aus, um das Fenster **Anwendungseinstellung hinzufügen/bearbeiten** zu schließen.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-326">Select **OK** to close the **Add/Edit application setting** window.</span></span>
1. <span data-ttu-id="4e8f3-327">Klicken Sie oben auf dem Blatt **Konfiguration** auf **Speichern**.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-327">Select **Save** at the top of the **Configuration** blade.</span></span>

<span data-ttu-id="4e8f3-328">Azure App Service startet die App automatisch neu, nachdem eine App-Einstellung (Umgebungsvariable) im Azure-Portal hinzugefügt, geändert oder gelöscht wurde.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-328">Azure App Service automatically restarts the app after an app setting (environment variable) is added, changed, or deleted in the Azure portal.</span></span>

#### <a name="windows"></a><span data-ttu-id="4e8f3-329">Windows</span><span class="sxs-lookup"><span data-stu-id="4e8f3-329">Windows</span></span>

<span data-ttu-id="4e8f3-330">Zum Festlegen der `ASPNETCORE_ENVIRONMENT` für die aktuelle Sitzung werden folgende Befehle verwendet, wenn die App mit [dotnet run](/dotnet/core/tools/dotnet-run) gestartet wird:</span><span class="sxs-lookup"><span data-stu-id="4e8f3-330">To set the `ASPNETCORE_ENVIRONMENT` for the current session when the app is started using [dotnet run](/dotnet/core/tools/dotnet-run), the following commands are used:</span></span>

<span data-ttu-id="4e8f3-331">**Eingabeaufforderung**</span><span class="sxs-lookup"><span data-stu-id="4e8f3-331">**Command prompt**</span></span>

```console
set ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="4e8f3-332">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="4e8f3-332">**PowerShell**</span></span>

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="4e8f3-333">Diese Befehle sind nur für das aktuelle Fenster wirksam.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-333">These commands only take effect for the current window.</span></span> <span data-ttu-id="4e8f3-334">Wenn das Fenster geschlossen wird, wird die Einstellung `ASPNETCORE_ENVIRONMENT` auf die Standardeinstellung oder den Computerwert zurückgesetzt.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-334">When the window is closed, the `ASPNETCORE_ENVIRONMENT` setting reverts to the default setting or machine value.</span></span>

<span data-ttu-id="4e8f3-335">Wenn Sie den Wert in Windows global festlegen möchten, nutzen Sie eine der folgenden Möglichkeiten:</span><span class="sxs-lookup"><span data-stu-id="4e8f3-335">To set the value globally in Windows, use either of the following approaches:</span></span>

* <span data-ttu-id="4e8f3-336">Öffnen Sie **Systemsteuerung** > **System** > **Erweiterte Systemeinstellungen**, und fügen Sie den Wert `ASPNETCORE_ENVIRONMENT` hinzu, oder bearbeiten Sie ihn:</span><span class="sxs-lookup"><span data-stu-id="4e8f3-336">Open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value:</span></span>

  ![Erweiterte Systemeigenschaften](environments/_static/systemsetting_environment.png)

  ![ASP.NET Core-Umgebungsvariable](environments/_static/windows_aspnetcore_environment.png)

* <span data-ttu-id="4e8f3-339">Führen Sie die Eingabeaufforderung als Administrator aus, und verwenden Sie den Befehl `setx`. Alternativ können Sie auch die PowerShell-Eingabeaufforderung als Administrator ausführen und `[Environment]::SetEnvironmentVariable` nutzen:</span><span class="sxs-lookup"><span data-stu-id="4e8f3-339">Open an administrative command prompt and use the `setx` command or open an administrative PowerShell command prompt and use `[Environment]::SetEnvironmentVariable`:</span></span>

  <span data-ttu-id="4e8f3-340">**Eingabeaufforderung**</span><span class="sxs-lookup"><span data-stu-id="4e8f3-340">**Command prompt**</span></span>

  ```console
  setx ASPNETCORE_ENVIRONMENT Development /M
  ```

  <span data-ttu-id="4e8f3-341">Mit dem Parameter `/M` wird angegeben, dass die Umgebungsvariable auf Systemebene festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-341">The `/M` switch indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="4e8f3-342">Ohne den Parameter `/M` wird die Umgebungsvariable für das Nutzerkonto festgelegt.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-342">If the `/M` switch isn't used, the environment variable is set for the user account.</span></span>

  <span data-ttu-id="4e8f3-343">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="4e8f3-343">**PowerShell**</span></span>

  ```powershell
  [Environment]::SetEnvironmentVariable("ASPNETCORE_ENVIRONMENT", "Development", "Machine")
  ```

  <span data-ttu-id="4e8f3-344">Mit dem Optionswert `Machine` wird angegeben, dass die Umgebungsvariable auf Systemebene festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-344">The `Machine` option value indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="4e8f3-345">Verwenden Sie stattdessen den Optionswert `User`, wird die Umgebungsvariable für das Nutzerkonto festgelegt.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-345">If the option value is changed to `User`, the environment variable is set for the user account.</span></span>

<span data-ttu-id="4e8f3-346">Sobald die Umgebungsvariable `ASPNETCORE_ENVIRONMENT` global festgelegt wird, gilt sie in jedem geöffneten Befehlsfenster für `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-346">When the `ASPNETCORE_ENVIRONMENT` environment variable is set globally, it takes effect for `dotnet run` in any command window opened after the value is set.</span></span>

<span data-ttu-id="4e8f3-347">**web.config**</span><span class="sxs-lookup"><span data-stu-id="4e8f3-347">**web.config**</span></span>

<span data-ttu-id="4e8f3-348">Eine Anleitung zum Festlegen der Umgebungsvariable `ASPNETCORE_ENVIRONMENT` mit *web.config* finden Sie im Abschnitt *Festlegen von Umgebungsvariablen* der im Artikel zu <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-348">To set the `ASPNETCORE_ENVIRONMENT` environment variable with *web.config*, see the *Setting environment variables* section of <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.</span></span>

<span data-ttu-id="4e8f3-349">**Projektdatei oder Veröffentlichungsprofil**</span><span class="sxs-lookup"><span data-stu-id="4e8f3-349">**Project file or publish profile**</span></span>

<span data-ttu-id="4e8f3-350">**Für Windows IIS-Bereitstellungen**: Beziehen Sie die `<EnvironmentName>`-Eigenschaft in das [Veröffentlichungsprofil (PUBXML)](xref:host-and-deploy/visual-studio-publish-profiles) oder die Projektdatei ein.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-350">**For Windows IIS deployments:** Include the `<EnvironmentName>` property in the [publish profile (.pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) or project file.</span></span> <span data-ttu-id="4e8f3-351">Dieser Ansatz legt die Umgebung in *web.config* fest, wenn das Projekt veröffentlicht wird:</span><span class="sxs-lookup"><span data-stu-id="4e8f3-351">This approach sets the environment in *web.config* when the project is published:</span></span>

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

<span data-ttu-id="4e8f3-352">**Pro IIS-Anwendungspool**</span><span class="sxs-lookup"><span data-stu-id="4e8f3-352">**Per IIS Application Pool**</span></span>

<span data-ttu-id="4e8f3-353">Wenn Sie für eine App, die in einem isolierten Anwendungspool ausgeführt wird, die Umgebungsvariable `ASPNETCORE_ENVIRONMENT` festlegen möchten (unterstützt in IIS 10.0 oder höher), lesen Sie den Abschnitt *AppCmd.exe command (Befehl „AppCmd.exe“)* im Artikel zu [Umgebungsvariablen&lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe).</span><span class="sxs-lookup"><span data-stu-id="4e8f3-353">To set the `ASPNETCORE_ENVIRONMENT` environment variable for an app running in an isolated Application Pool (supported on IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span> <span data-ttu-id="4e8f3-354">Wurde die Umgebungsvariable `ASPNETCORE_ENVIRONMENT` für einen Anwendungspool festgelegt, überschreibt der Wert die Einstellung auf Systemebene.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-354">When the `ASPNETCORE_ENVIRONMENT` environment variable is set for an app pool, its value overrides a setting at the system level.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4e8f3-355">Wenn Sie eine APP in den IIS hosten und die Umgebungsvariable `ASPNETCORE_ENVIRONMENT` hinzufügen oder ändern, sorgen Sie auf eine der folgenden Arten und Weisen dafür, dass der neue Wert in den Apps hinterlegt ist:</span><span class="sxs-lookup"><span data-stu-id="4e8f3-355">When hosting an app in IIS and adding or changing the `ASPNETCORE_ENVIRONMENT` environment variable, use any one of the following approaches to have the new value picked up by apps:</span></span>
>
> * <span data-ttu-id="4e8f3-356">Führen Sie in einer Eingabeaufforderung `net stop was /y` gefolgt von `net start w3svc` aus.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-356">Execute `net stop was /y` followed by `net start w3svc` from a command prompt.</span></span>
> * <span data-ttu-id="4e8f3-357">Starten Sie den Server neu.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-357">Restart the server.</span></span>

#### <a name="macos"></a><span data-ttu-id="4e8f3-358">macOS</span><span class="sxs-lookup"><span data-stu-id="4e8f3-358">macOS</span></span>

<span data-ttu-id="4e8f3-359">Das Festlegen der aktuellen Umgebung für macOS kann inline während der Ausführung der App erfolgen:</span><span class="sxs-lookup"><span data-stu-id="4e8f3-359">Setting the current environment for macOS can be performed in-line when running the app:</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

<span data-ttu-id="4e8f3-360">Legen Sie die Umgebung alternativ mit `export` vor der Ausführung der App fest:</span><span class="sxs-lookup"><span data-stu-id="4e8f3-360">Alternatively, set the environment with `export` prior to running the app:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="4e8f3-361">Umgebungsvariablen auf Computerebene werden in der *BASHRC*- oder *BASH_PROFILE*-Datei festgelegt.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-361">Machine-level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="4e8f3-362">Bearbeiten Sie die Datei mit einem beliebigen Text-Editor.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-362">Edit the file using any text editor.</span></span> <span data-ttu-id="4e8f3-363">Fügen Sie die folgende Anweisung hinzu:</span><span class="sxs-lookup"><span data-stu-id="4e8f3-363">Add the following statement:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

#### <a name="linux"></a><span data-ttu-id="4e8f3-364">Linux</span><span class="sxs-lookup"><span data-stu-id="4e8f3-364">Linux</span></span>

<span data-ttu-id="4e8f3-365">Verwenden Sie bei Linux-Distributionen für sitzungsbasierte Variableneinstellungen den Befehl `export` in der Eingabeaufforderung und die Datei *bash_profile* für Umgebungseinstellungen auf Computerebene.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-365">For Linux distributions, use the `export` command at a command prompt for session-based variable settings and *bash_profile* file for machine-level environment settings.</span></span>

### <a name="set-the-environment-in-code"></a><span data-ttu-id="4e8f3-366">Festlegen der Umgebung im Code</span><span class="sxs-lookup"><span data-stu-id="4e8f3-366">Set the environment in code</span></span>

<span data-ttu-id="4e8f3-367">Rufen Sie beim Erstellen des Hosts <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseEnvironment*> auf.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-367">Call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseEnvironment*> when building the host.</span></span> <span data-ttu-id="4e8f3-368">Siehe <xref:fundamentals/host/web-host#environment>.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-368">See <xref:fundamentals/host/web-host#environment>.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="4e8f3-369">Konfiguration nach Umgebung</span><span class="sxs-lookup"><span data-stu-id="4e8f3-369">Configuration by environment</span></span>

<span data-ttu-id="4e8f3-370">Zum Laden von „Konfiguration nach Umgebung“ empfehlen wir:</span><span class="sxs-lookup"><span data-stu-id="4e8f3-370">To load configuration by environment, we recommend:</span></span>

* <span data-ttu-id="4e8f3-371">*appsettings*-Dateien (*appsettings.{Umgebung}.json*).</span><span class="sxs-lookup"><span data-stu-id="4e8f3-371">*appsettings* files (*appsettings.{Environment}.json*).</span></span> <span data-ttu-id="4e8f3-372">Siehe <xref:fundamentals/configuration/index#json-configuration-provider>.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-372">See <xref:fundamentals/configuration/index#json-configuration-provider>.</span></span>
* <span data-ttu-id="4e8f3-373">Umgebungsvariablen (in jedem System festgelegt, in dem die App gehostet wird).</span><span class="sxs-lookup"><span data-stu-id="4e8f3-373">Environment variables (set on each system where the app is hosted).</span></span> <span data-ttu-id="4e8f3-374">Weitere Informationen finden Sie unter <xref:fundamentals/host/web-host#environment> und <xref:security/app-secrets#environment-variables>.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-374">See <xref:fundamentals/host/web-host#environment> and <xref:security/app-secrets#environment-variables>.</span></span>
* <span data-ttu-id="4e8f3-375">Secret Manager (nur in der Entwicklungsumgebung)</span><span class="sxs-lookup"><span data-stu-id="4e8f3-375">Secret Manager (in the Development environment only).</span></span> <span data-ttu-id="4e8f3-376">Siehe <xref:security/app-secrets>.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-376">See <xref:security/app-secrets>.</span></span>

## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="4e8f3-377">Umgebungsbasierte Startklasse und Methoden</span><span class="sxs-lookup"><span data-stu-id="4e8f3-377">Environment-based Startup class and methods</span></span>

### <a name="inject-ihostingenvironment-into-startupconfigure"></a><span data-ttu-id="4e8f3-378">Einfügen von IHostingEnvironment in Startup.Configure</span><span class="sxs-lookup"><span data-stu-id="4e8f3-378">Inject IHostingEnvironment into Startup.Configure</span></span>

<span data-ttu-id="4e8f3-379">Fügen Sie <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> in `Startup.Configure` ein.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-379">Inject <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> into `Startup.Configure`.</span></span> <span data-ttu-id="4e8f3-380">Diese Vorgehensweise ist nützlich, wenn die App `Startup.Configure` nur für einige Umgebungen mit minimalen Codeunterschieden pro Umgebung konfigurieren muss.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-380">This approach is useful when the app only requires configuring `Startup.Configure` for only a few environments with minimal code differences per environment.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // Development environment code
    }
    else
    {
        // Code for all other environments
    }
}
```

### <a name="inject-ihostingenvironment-into-the-startup-class"></a><span data-ttu-id="4e8f3-381">Einfügen von IHostingEnvironment in die Startklasse</span><span class="sxs-lookup"><span data-stu-id="4e8f3-381">Inject IHostingEnvironment into the Startup class</span></span>

<span data-ttu-id="4e8f3-382">Fügen Sie <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> in den `Startup`-Konstruktor ein, und weisen Sie den Dienst einem Feld zu, das in der gesamten `Startup`-Klasse verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-382">Inject <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> into the `Startup` constructor and assign the service to a field for use throughout the `Startup` class.</span></span> <span data-ttu-id="4e8f3-383">Diese Vorgehensweise ist nützlich, wenn die App „Startup“ nur für einige Umgebungen mit minimalen Codeunterschieden pro Umgebung konfigurieren muss.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-383">This approach is useful when the app requires configuring startup for only a few environments with minimal code differences per environment.</span></span>

<span data-ttu-id="4e8f3-384">Im folgenden Beispiel:</span><span class="sxs-lookup"><span data-stu-id="4e8f3-384">In the following example:</span></span>

* <span data-ttu-id="4e8f3-385">Die Umgebung wird im Feld `_env` gespeichert.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-385">The environment is held in the `_env` field.</span></span>
* <span data-ttu-id="4e8f3-386">`_env` wird in `ConfigureServices` und `Configure` verwendet, um die Startkonfiguration basierend auf der Umgebung der App anzuwenden.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-386">`_env` is used in `ConfigureServices` and `Configure` to apply startup configuration based on the app's environment.</span></span>

```csharp
public class Startup
{
    private readonly IHostingEnvironment _env;

    public Startup(IHostingEnvironment env)
    {
        _env = env;
    }

    public void ConfigureServices(IServiceCollection services)
    {
        if (_env.IsDevelopment())
        {
            // Development environment code
        }
        else if (_env.IsStaging())
        {
            // Staging environment code
        }
        else
        {
            // Code for all other environments
        }
    }

    public void Configure(IApplicationBuilder app)
    {
        if (_env.IsDevelopment())
        {
            // Development environment code
        }
        else
        {
            // Code for all other environments
        }
    }
}
```

### <a name="startup-class-conventions"></a><span data-ttu-id="4e8f3-387">Konventionen der Startup-Klasse</span><span class="sxs-lookup"><span data-stu-id="4e8f3-387">Startup class conventions</span></span>

<span data-ttu-id="4e8f3-388">Nach dem Start einer ASP.NET Core-App lädt die [Startklasse](xref:fundamentals/startup) die App.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-388">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="4e8f3-389">Die App kann je nach Umgebung unterschiedliche `Startup`-Klassen definieren (z. B. `StartupDevelopment`).</span><span class="sxs-lookup"><span data-stu-id="4e8f3-389">The app can define separate `Startup` classes for different environments (for example, `StartupDevelopment`).</span></span> <span data-ttu-id="4e8f3-390">Die entsprechende `Startup`-Klasse wird zur Laufzeit ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-390">The appropriate `Startup` class is selected at runtime.</span></span> <span data-ttu-id="4e8f3-391">Die Klasse, deren Namenssuffix mit der aktuellen Umgebung übereinstimmt, wird priorisiert.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-391">The class whose name suffix matches the current environment is prioritized.</span></span> <span data-ttu-id="4e8f3-392">Ist keine übereinstimmende `Startup{EnvironmentName}`-Klasse vorhanden, wird die Klasse `Startup` verwendet.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-392">If a matching `Startup{EnvironmentName}` class isn't found, the `Startup` class is used.</span></span> <span data-ttu-id="4e8f3-393">Diese Vorgehensweise ist nützlich, wenn die App „Startup“ für mehrere Umgebungen mit vielen Codeunterschieden pro Umgebung konfigurieren muss.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-393">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span>

<span data-ttu-id="4e8f3-394">Wenn Sie umgebungsbasierte `Startup`-Klassen implementieren möchten, erstellen Sie für jedes verwendete Element eine `Startup{EnvironmentName}`-Klasse und eine Fallback-Klasse des Typs `Startup`:</span><span class="sxs-lookup"><span data-stu-id="4e8f3-394">To implement environment-based `Startup` classes, create a `Startup{EnvironmentName}` class for each environment in use and a fallback `Startup` class:</span></span>

```csharp
// Startup class to use in the Development environment
public class StartupDevelopment
{
    public void ConfigureServices(IServiceCollection services)
    {
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
    }
}

// Startup class to use in the Production environment
public class StartupProduction
{
    public void ConfigureServices(IServiceCollection services)
    {
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
    }
}

// Fallback Startup class
// Selected if the environment doesn't match a Startup{EnvironmentName} class
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
    }
}
```

<span data-ttu-id="4e8f3-395">Verwenden Sie die Überladung [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup), die einen Assemblynamen akzeptiert:</span><span class="sxs-lookup"><span data-stu-id="4e8f3-395">Use the [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) overload that accepts an assembly name:</span></span>

```csharp
public static void Main(string[] args)
{
    CreateWebHostBuilder(args).Build().Run();
}

public static IWebHostBuilder CreateWebHostBuilder(string[] args)
{
    var assemblyName = typeof(Startup).GetTypeInfo().Assembly.FullName;

    return WebHost.CreateDefaultBuilder(args)
        .UseStartup(assemblyName);
}
```

### <a name="startup-method-conventions"></a><span data-ttu-id="4e8f3-396">Konventionen der Methode „Start“</span><span class="sxs-lookup"><span data-stu-id="4e8f3-396">Startup method conventions</span></span>

<span data-ttu-id="4e8f3-397">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) und [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) unterstützen umgebungsspezifische Versionen der Form `Configure<EnvironmentName>` und `Configure<EnvironmentName>Services`.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-397">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) support environment-specific versions of the form `Configure<EnvironmentName>` and `Configure<EnvironmentName>Services`.</span></span> <span data-ttu-id="4e8f3-398">Diese Vorgehensweise ist nützlich, wenn die App „Startup“ für mehrere Umgebungen mit vielen Codeunterschieden pro Umgebung konfigurieren muss.</span><span class="sxs-lookup"><span data-stu-id="4e8f3-398">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet_all&highlight=15,42)]

## <a name="additional-resources"></a><span data-ttu-id="4e8f3-399">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="4e8f3-399">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>

::: moniker-end
