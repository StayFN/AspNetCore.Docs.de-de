---
title: Befehl „dotnet aspnet-codegenerator“
author: rick-anderson
description: Der Befehl „dotnet aspnet-codegenerator“ erstellt das Gerüst für ASP.NET Core-Projekte.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 07/04/2019
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: fundamentals/tools/dotnet-aspnet-codegenerator
ms.openlocfilehash: a106654c8a37e84e9186a2f06d90605df753e8a7
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.contentlocale: de-DE
ms.lasthandoff: 06/26/2020
ms.locfileid: "85405600"
---
# <a name="dotnet-aspnet-codegenerator"></a><span data-ttu-id="1e5f6-103">dotnet aspnet-codegenerator</span><span class="sxs-lookup"><span data-stu-id="1e5f6-103">dotnet aspnet-codegenerator</span></span>

<span data-ttu-id="1e5f6-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1e5f6-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1e5f6-105">`dotnet aspnet-codegenerator`: Führt die ASP.NET Core-Gerüstbauengine aus.</span><span class="sxs-lookup"><span data-stu-id="1e5f6-105">`dotnet aspnet-codegenerator` - Runs the ASP.NET Core scaffolding engine.</span></span> <span data-ttu-id="1e5f6-106">`dotnet aspnet-codegenerator` wird nur für den Gerüstbau über die Befehlszeile benötigt, nicht zum Gerüstbau mit Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1e5f6-106">`dotnet aspnet-codegenerator` is only required to scaffold from the command line, it's not needed to use scaffolding with Visual Studio.</span></span>

<span data-ttu-id="1e5f6-107">Dieser Artikel gilt für [.NET Core 2.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/2.1) und höher.</span><span class="sxs-lookup"><span data-stu-id="1e5f6-107">This article applies to [.NET Core 2.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/2.1) and later.</span></span>

## <a name="installing-aspnet-codegenerator"></a><span data-ttu-id="1e5f6-108">Installieren von „aspnet-codegenerator“</span><span class="sxs-lookup"><span data-stu-id="1e5f6-108">Installing aspnet-codegenerator</span></span>

<span data-ttu-id="1e5f6-109">`dotnet-aspnet-codegenerator` ist ein [globales Tool](/dotnet/core/tools/global-tools), das installiert werden muss.</span><span class="sxs-lookup"><span data-stu-id="1e5f6-109">`dotnet-aspnet-codegenerator` is a [global tool](/dotnet/core/tools/global-tools) that must be installed.</span></span> <span data-ttu-id="1e5f6-110">Über den folgenden Befehl wird die neueste stabile Version des `dotnet-aspnet-codegenerator`-Tools installiert:</span><span class="sxs-lookup"><span data-stu-id="1e5f6-110">The following command installs the latest stable version of the `dotnet-aspnet-codegenerator` tool:</span></span>

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="1e5f6-111">Mit dem folgenden Befehl wird `dotnet-aspnet-codegenerator` auf die neueste stabile Version aktualisiert, die in den installierten .NET Core SDKs verfügbar ist:</span><span class="sxs-lookup"><span data-stu-id="1e5f6-111">The following command updates `dotnet-aspnet-codegenerator` to the latest stable version available from the installed .NET Core SDKs:</span></span>

```dotnetcli
dotnet tool update -g dotnet-aspnet-codegenerator
```

## <a name="synopsis"></a><span data-ttu-id="1e5f6-112">Übersicht</span><span class="sxs-lookup"><span data-stu-id="1e5f6-112">Synopsis</span></span>

```
dotnet aspnet-codegenerator [arguments] [-p|--project] [-n|--nuget-package-dir] [-c|--configuration] [-tfm|--target-framework] [-b|--build-base-path] [--no-build] 
dotnet aspnet-codegenerator [-h|--help]
```

## <a name="description"></a><span data-ttu-id="1e5f6-113">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="1e5f6-113">Description</span></span>

<span data-ttu-id="1e5f6-114">Der globale Befehl `dotnet aspnet-codegenerator` führt den ASP.NET Code-Codegenerator und die Gerüstbauengine aus.</span><span class="sxs-lookup"><span data-stu-id="1e5f6-114">The `dotnet aspnet-codegenerator` global command runs the ASP.NET Core code generator and scaffolding engine.</span></span>

## <a name="arguments"></a><span data-ttu-id="1e5f6-115">Argumente</span><span class="sxs-lookup"><span data-stu-id="1e5f6-115">Arguments</span></span>

`generator`

<span data-ttu-id="1e5f6-116">Der auszuführende Codegenerator.</span><span class="sxs-lookup"><span data-stu-id="1e5f6-116">The code generator to run.</span></span> <span data-ttu-id="1e5f6-117">Folgende Generatoren sind verfügbar:</span><span class="sxs-lookup"><span data-stu-id="1e5f6-117">The following generators are available:</span></span>

| <span data-ttu-id="1e5f6-118">Generator</span><span class="sxs-lookup"><span data-stu-id="1e5f6-118">Generator</span></span> | <span data-ttu-id="1e5f6-119">Vorgang</span><span class="sxs-lookup"><span data-stu-id="1e5f6-119">Operation</span></span> |
| ----------------- | ------------ | 
| <span data-ttu-id="1e5f6-120">area</span><span class="sxs-lookup"><span data-stu-id="1e5f6-120">area</span></span>      | [<span data-ttu-id="1e5f6-121">Gerüstbau für einen Bereich</span><span class="sxs-lookup"><span data-stu-id="1e5f6-121">Scaffolds an Area</span></span>](/aspnet/core/mvc/controllers/areas) |
  <span data-ttu-id="1e5f6-122">Controller</span><span class="sxs-lookup"><span data-stu-id="1e5f6-122">controller</span></span>| [<span data-ttu-id="1e5f6-123">Gerüstbau für einen Controller</span><span class="sxs-lookup"><span data-stu-id="1e5f6-123">Scaffolds a controller</span></span>](/aspnet/core/tutorials/first-mvc-app/adding-model) |
  <span data-ttu-id="1e5f6-124">Identität</span><span class="sxs-lookup"><span data-stu-id="1e5f6-124">identity</span></span>  | <span data-ttu-id="1e5f6-125">[Gerüstbau Identity](/aspnet/core/security/authentication/scaffold-identity)</span><span class="sxs-lookup"><span data-stu-id="1e5f6-125">[Scaffolds Identity](/aspnet/core/security/authentication/scaffold-identity)</span></span> |
  <span data-ttu-id="1e5f6-126">razorpage</span><span class="sxs-lookup"><span data-stu-id="1e5f6-126">razorpage</span></span> | <span data-ttu-id="1e5f6-127">[Gerüstbau für Razor Pages](/aspnet/core/tutorials/razor-pages/model)</span><span class="sxs-lookup"><span data-stu-id="1e5f6-127">[Scaffolds Razor Pages](/aspnet/core/tutorials/razor-pages/model)</span></span> |
  <span data-ttu-id="1e5f6-128">Sicht</span><span class="sxs-lookup"><span data-stu-id="1e5f6-128">view</span></span>      | [<span data-ttu-id="1e5f6-129">Gerüstbau für eine Ansicht</span><span class="sxs-lookup"><span data-stu-id="1e5f6-129">Scaffolds a view</span></span>](/aspnet/core/mvc/views/overview) |

## <a name="options"></a><span data-ttu-id="1e5f6-130">Optionen</span><span class="sxs-lookup"><span data-stu-id="1e5f6-130">Options</span></span>

`-n|--nuget-package-dir`

<span data-ttu-id="1e5f6-131">Gibt das NuGet-Paketverzeichnis an.</span><span class="sxs-lookup"><span data-stu-id="1e5f6-131">Specifies the NuGet package directory.</span></span>

`-c|--configuration {Debug|Release}`

<span data-ttu-id="1e5f6-132">Legt die Buildkonfiguration fest.</span><span class="sxs-lookup"><span data-stu-id="1e5f6-132">Defines the build configuration.</span></span> <span data-ttu-id="1e5f6-133">Der Standardwert ist `Debug`.</span><span class="sxs-lookup"><span data-stu-id="1e5f6-133">The default value is `Debug`.</span></span>

`-tfm|--target-framework`

<span data-ttu-id="1e5f6-134">Das zu verwendende [Zielframework](/dotnet/standard/frameworks).</span><span class="sxs-lookup"><span data-stu-id="1e5f6-134">Target [Framework](/dotnet/standard/frameworks) to use.</span></span> <span data-ttu-id="1e5f6-135">Beispielsweise `net46`.</span><span class="sxs-lookup"><span data-stu-id="1e5f6-135">For example, `net46`.</span></span>

`-b|--build-base-path`

<span data-ttu-id="1e5f6-136">Der Basispfad für den Build.</span><span class="sxs-lookup"><span data-stu-id="1e5f6-136">The build base path.</span></span>

`-h|--help`

<span data-ttu-id="1e5f6-137">Druckt eine kurze Hilfe für den Befehl.</span><span class="sxs-lookup"><span data-stu-id="1e5f6-137">Prints out a short help for the command.</span></span>

`--no-build`

<span data-ttu-id="1e5f6-138">Erstellt das Projekt nicht vor der Ausführung.</span><span class="sxs-lookup"><span data-stu-id="1e5f6-138">Doesn't build the project before running.</span></span> <span data-ttu-id="1e5f6-139">Zudem wird das Flag `--no-restore` implizit festgelegt.</span><span class="sxs-lookup"><span data-stu-id="1e5f6-139">It also implicitly sets the `--no-restore` flag.</span></span>

`-p|--project <PATH>`

<span data-ttu-id="1e5f6-140">Gibt den Pfad der auszuführenden Projektdatei an (Ordnername oder vollständiger Pfad).</span><span class="sxs-lookup"><span data-stu-id="1e5f6-140">Specifies the path of the project file to run (folder name or full path).</span></span> <span data-ttu-id="1e5f6-141">Wenn nicht angegeben, wird standardmäßig das aktuelle Verzeichnis gewählt.</span><span class="sxs-lookup"><span data-stu-id="1e5f6-141">If not specified, it defaults to the current directory.</span></span>

## <a name="generator-options"></a><span data-ttu-id="1e5f6-142">Generatoroptionen</span><span class="sxs-lookup"><span data-stu-id="1e5f6-142">Generator options</span></span>

<span data-ttu-id="1e5f6-143">In den folgenden Abschnitte werden die verfügbaren Optionen für die unterstützten Generatoren vorgestellt:</span><span class="sxs-lookup"><span data-stu-id="1e5f6-143">The following sections detail the options available for the supported generators:</span></span>

* <span data-ttu-id="1e5f6-144">Bereich</span><span class="sxs-lookup"><span data-stu-id="1e5f6-144">Area</span></span>
* <span data-ttu-id="1e5f6-145">Controller</span><span class="sxs-lookup"><span data-stu-id="1e5f6-145">Controller</span></span>
* Identity  
* <span data-ttu-id="1e5f6-146">Razor Pages</span><span class="sxs-lookup"><span data-stu-id="1e5f6-146">Razorpage</span></span>
* <span data-ttu-id="1e5f6-147">Ansicht</span><span class="sxs-lookup"><span data-stu-id="1e5f6-147">View</span></span>

<a name="area"></a>

### <a name="area-options"></a><span data-ttu-id="1e5f6-148">Bereichsoptionen</span><span class="sxs-lookup"><span data-stu-id="1e5f6-148">Area options</span></span>

<span data-ttu-id="1e5f6-149">Dieses Tool ist für ASP.NET Core-Webprojekte mit Controllern und Ansichten vorgesehen.</span><span class="sxs-lookup"><span data-stu-id="1e5f6-149">This tool is intended for ASP.NET Core web projects with controllers and views.</span></span> <span data-ttu-id="1e5f6-150">Es ist nicht für Razor Pages-Apps gedacht.</span><span class="sxs-lookup"><span data-stu-id="1e5f6-150">It's not intended for Razor Pages apps.</span></span>

<span data-ttu-id="1e5f6-151">Verwendung: `dotnet aspnet-codegenerator area AreaNameToGenerate`</span><span class="sxs-lookup"><span data-stu-id="1e5f6-151">Usage: `dotnet aspnet-codegenerator area AreaNameToGenerate`</span></span>

<span data-ttu-id="1e5f6-152">Der oben gezeigte Befehl generiert die folgenden Ordner:</span><span class="sxs-lookup"><span data-stu-id="1e5f6-152">The preceding command generates the following folders:</span></span>

* <span data-ttu-id="1e5f6-153">*Bereiche*</span><span class="sxs-lookup"><span data-stu-id="1e5f6-153">*Areas*</span></span>
  * <span data-ttu-id="1e5f6-154">*AreaNameToGenerate*</span><span class="sxs-lookup"><span data-stu-id="1e5f6-154">*AreaNameToGenerate*</span></span>
    * <span data-ttu-id="1e5f6-155">*Controller*</span><span class="sxs-lookup"><span data-stu-id="1e5f6-155">*Controllers*</span></span>
    * <span data-ttu-id="1e5f6-156">*Data*</span><span class="sxs-lookup"><span data-stu-id="1e5f6-156">*Data*</span></span>
    * <span data-ttu-id="1e5f6-157">*Models*</span><span class="sxs-lookup"><span data-stu-id="1e5f6-157">*Models*</span></span>
    * <span data-ttu-id="1e5f6-158">*Ansichten*</span><span class="sxs-lookup"><span data-stu-id="1e5f6-158">*Views*</span></span>

<a name="ctl"></a>

### <a name="controller-options"></a><span data-ttu-id="1e5f6-159">Controlleroptionen</span><span class="sxs-lookup"><span data-stu-id="1e5f6-159">Controller options</span></span>

<span data-ttu-id="1e5f6-160">Die folgende Tabelle zeigt die Optionen für `aspnet-codegenerator` `controller` und `razorpage`:</span><span class="sxs-lookup"><span data-stu-id="1e5f6-160">The following table lists options for  `aspnet-codegenerator` `controller` and `razorpage`:</span></span>

[!INCLUDE [aspnet-codegenerator-args-md.md](~/includes/aspnet-codegenerator-args-md.md)]

<span data-ttu-id="1e5f6-161">Die folgende Tabelle zeigt die Optionen für `aspnet-codegenerator controller`:</span><span class="sxs-lookup"><span data-stu-id="1e5f6-161">The following table lists options unique to  `aspnet-codegenerator controller`:</span></span>

| <span data-ttu-id="1e5f6-162">Option</span><span class="sxs-lookup"><span data-stu-id="1e5f6-162">Option</span></span>               | <span data-ttu-id="1e5f6-163">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="1e5f6-163">Description</span></span>|
| ----------------- | ------------ |
| <span data-ttu-id="1e5f6-164">--controllerName oder -name</span><span class="sxs-lookup"><span data-stu-id="1e5f6-164">--controllerName or -name</span></span> | <span data-ttu-id="1e5f6-165">Der Name des Controllers.</span><span class="sxs-lookup"><span data-stu-id="1e5f6-165">Name of the controller.</span></span> |
| <span data-ttu-id="1e5f6-166">--useAsyncActions oder -async</span><span class="sxs-lookup"><span data-stu-id="1e5f6-166">--useAsyncActions or -async</span></span> | <span data-ttu-id="1e5f6-167">Generiert asynchrone Controlleraktionen.</span><span class="sxs-lookup"><span data-stu-id="1e5f6-167">Generate async controller actions.</span></span> |
| <span data-ttu-id="1e5f6-168">--noViews oder -nv</span><span class="sxs-lookup"><span data-stu-id="1e5f6-168">--noViews or -nv</span></span> | <span data-ttu-id="1e5f6-169">Generiert **keine** Ansichten.</span><span class="sxs-lookup"><span data-stu-id="1e5f6-169">Generate **no** views.</span></span> |
| <span data-ttu-id="1e5f6-170">--restWithNoViews oder -api</span><span class="sxs-lookup"><span data-stu-id="1e5f6-170">--restWithNoViews or -api</span></span>  | <span data-ttu-id="1e5f6-171">Generiert einen Controller mit einer API im REST-Stil.</span><span class="sxs-lookup"><span data-stu-id="1e5f6-171">Generate a Controller with REST style API.</span></span> <span data-ttu-id="1e5f6-172">`noViews` wird vorausgesetzt, und ansichtsbezogene Optionen werden ignoriert.</span><span class="sxs-lookup"><span data-stu-id="1e5f6-172">`noViews` is assumed and any view related options are ignored.</span></span> |
| <span data-ttu-id="1e5f6-173">--readWriteActions oder -actions</span><span class="sxs-lookup"><span data-stu-id="1e5f6-173">--readWriteActions or -actions</span></span> | <span data-ttu-id="1e5f6-174">Generiert einen Controller mit Lese-/Schreibaktionen ohne Modell.</span><span class="sxs-lookup"><span data-stu-id="1e5f6-174">Generate controller with read/write actions without a model.</span></span> |

<span data-ttu-id="1e5f6-175">Verwenden Sie den Schalter `-h`, um Hilfe zum `aspnet-codegenerator controller`-Befehl zu erhalten:</span><span class="sxs-lookup"><span data-stu-id="1e5f6-175">Use the `-h` switch for help on the `aspnet-codegenerator controller` command:</span></span>

```dotnetcli
dotnet aspnet-codegenerator controller -h
```

<span data-ttu-id="1e5f6-176">Unter [Erstellen des Gerüsts für das Filmmodell](/aspnet/core/tutorials/razor-pages/model) finden Sie ein Beispiel für `dotnet aspnet-codegenerator controller`.</span><span class="sxs-lookup"><span data-stu-id="1e5f6-176">See [Scaffold the movie model](/aspnet/core/tutorials/razor-pages/model) for an example of `dotnet aspnet-codegenerator controller`.</span></span>

### <a name="razorpage"></a><span data-ttu-id="1e5f6-177">Razor Pages</span><span class="sxs-lookup"><span data-stu-id="1e5f6-177">Razorpage</span></span>

<a name="rp"></a>

<span data-ttu-id="1e5f6-178">Für Razor Pages können jeweils einzelne Gerüste erstellt werden, indem der Name der neuen Seite und der zu verwendenden Vorlage angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="1e5f6-178">Razor Pages can be individually scaffolded by specifying the name of the new page and the template to use.</span></span> <span data-ttu-id="1e5f6-179">Folgende Vorlagen werden unterstützt:</span><span class="sxs-lookup"><span data-stu-id="1e5f6-179">The supported templates are:</span></span>

* `Empty`
* `Create`
* `Edit`
* `Delete`
* `Details`
* `List`

<span data-ttu-id="1e5f6-180">Beispielsweise verwendet der folgende Befehl die Bearbeitungsvorlage, um *MyEdit.cshtml* und *MyEdit.cshtml.cs* zu generieren:</span><span class="sxs-lookup"><span data-stu-id="1e5f6-180">For example, the following command uses the Edit template to generate *MyEdit.cshtml* and *MyEdit.cshtml.cs*:</span></span>

```dotnetcli
dotnet aspnet-codegenerator razorpage MyEdit Edit -m Movie -dc RazorPagesMovieContext -outDir Pages/Movies
```

<span data-ttu-id="1e5f6-181">Typischerweise werden die Vorlage und der Name der generierten Datei nicht angegeben, und es werden die folgenden Vorlagen erstellt:</span><span class="sxs-lookup"><span data-stu-id="1e5f6-181">Typically, the template and generated file name is not specified, and the following templates are created:</span></span>

* `Create`
* `Edit`
* `Delete`
* `Details`
* `List`

<span data-ttu-id="1e5f6-182">Die folgende Tabelle zeigt die Optionen für `aspnet-codegenerator` `razorpage` und `controller`:</span><span class="sxs-lookup"><span data-stu-id="1e5f6-182">The following table lists options for  `aspnet-codegenerator` `razorpage` and `controller`:</span></span>

[!INCLUDE [aspnet-codegenerator-args-md.md](~/includes/aspnet-codegenerator-args-md.md)]

<span data-ttu-id="1e5f6-183">Die folgende Tabelle zeigt die Optionen für `aspnet-codegenerator razorpage`:</span><span class="sxs-lookup"><span data-stu-id="1e5f6-183">The following table lists options unique to  `aspnet-codegenerator razorpage`:</span></span>

| <span data-ttu-id="1e5f6-184">Option</span><span class="sxs-lookup"><span data-stu-id="1e5f6-184">Option</span></span>               | <span data-ttu-id="1e5f6-185">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="1e5f6-185">Description</span></span>|
| ----------------- | ------------ |
|   <span data-ttu-id="1e5f6-186">--namespaceName oder -namespace</span><span class="sxs-lookup"><span data-stu-id="1e5f6-186">--namespaceName or -namespace</span></span> | <span data-ttu-id="1e5f6-187">Der Name des Namespace, der für das generierte PageModel verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="1e5f6-187">The name of the namespace to use for the generated PageModel</span></span> |
| <span data-ttu-id="1e5f6-188">--partialView oder -partial</span><span class="sxs-lookup"><span data-stu-id="1e5f6-188">--partialView or -partial</span></span> | <span data-ttu-id="1e5f6-189">Generiert eine Teilansicht.</span><span class="sxs-lookup"><span data-stu-id="1e5f6-189">Generate a partial view.</span></span> <span data-ttu-id="1e5f6-190">Die Layoutoptionen -l und -udl werden ignoriert, wenn diese Option angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="1e5f6-190">Layout options -l and -udl are ignored if this is specified.</span></span> |
| <span data-ttu-id="1e5f6-191">--noPageModel oder -npm</span><span class="sxs-lookup"><span data-stu-id="1e5f6-191">--noPageModel or -npm</span></span> | <span data-ttu-id="1e5f6-192">Schalter, um keine PageModel-Klasse für eine leere Vorlage zu generieren.</span><span class="sxs-lookup"><span data-stu-id="1e5f6-192">Switch to not generate a PageModel class for Empty template</span></span> |

<span data-ttu-id="1e5f6-193">Verwenden Sie den Schalter `-h`, um Hilfe zum `aspnet-codegenerator razorpage`-Befehl zu erhalten:</span><span class="sxs-lookup"><span data-stu-id="1e5f6-193">Use the `-h` switch for help on the `aspnet-codegenerator razorpage` command:</span></span>

```dotnetcli
dotnet aspnet-codegenerator razorpage -h
```

<span data-ttu-id="1e5f6-194">Unter [Erstellen des Gerüsts für das Filmmodell](/aspnet/core/tutorials/razor-pages/model) finden Sie ein Beispiel für `dotnet aspnet-codegenerator razorpage`.</span><span class="sxs-lookup"><span data-stu-id="1e5f6-194">See [Scaffold the movie model](/aspnet/core/tutorials/razor-pages/model) for an example of `dotnet aspnet-codegenerator razorpage`.</span></span>

### Identity

<span data-ttu-id="1e5f6-195">Siehe [Gerüst Identity](/aspnet/core/security/authentication/scaffold-identity)</span><span class="sxs-lookup"><span data-stu-id="1e5f6-195">See [Scaffold Identity](/aspnet/core/security/authentication/scaffold-identity)</span></span>
