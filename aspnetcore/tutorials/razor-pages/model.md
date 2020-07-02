---
title: 'Teil 2: Hinzufügen eines Modells zu einer Razor Pages-App in ASP.NET Core'
author: rick-anderson
description: Dies ist Teil 2 der Tutorialreihe zu Razor Pages.
ms.author: riande
ms.date: 12/05/2019
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: tutorials/razor-pages/model
ms.openlocfilehash: 053e35e78b2bf58fded49c9a2860060d6a81c544
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/26/2020
ms.locfileid: "85408109"
---
# <a name="part-2-add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="48ada-103">Teil 2: Hinzufügen eines Modells zu einer Razor Pages-App in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="48ada-103">Part 2, add a model to a Razor Pages app in ASP.NET Core</span></span>

<span data-ttu-id="48ada-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="48ada-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<!-- In the next update on the CLI version, let the scaffolder do the same work the VS driven scaffolder does. That is, create the DB context, etc -->

<span data-ttu-id="48ada-105">In diesem Abschnitt werden Klassen für die Verwaltung von Filmen hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="48ada-105">In this section, classes are added for managing movies.</span></span> <span data-ttu-id="48ada-106">Die Modellklassen der App verwenden [Entity Framework Core](/ef/core) (EF Core), um mit der Datenbank zu arbeiten.</span><span class="sxs-lookup"><span data-stu-id="48ada-106">The app's model classes use [Entity Framework Core (EF Core)](/ef/core) to work with the database.</span></span> <span data-ttu-id="48ada-107">EF Core ist ein objektrelationaler Mapper (O/RM), der den Datenzugriff vereinfacht.</span><span class="sxs-lookup"><span data-stu-id="48ada-107">EF Core is an object-relational mapper (O/RM) that simplifies data access.</span></span>

<span data-ttu-id="48ada-108">Die Modellklassen werden als POCO-Klassen (von „Plain-Old CLR Objects“) bezeichnet, da sie keinerlei Abhängigkeit von EF Core aufweisen.</span><span class="sxs-lookup"><span data-stu-id="48ada-108">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="48ada-109">Sie definieren die Eigenschaften einer Datei, die in der Datenbank gespeichert ist.</span><span class="sxs-lookup"><span data-stu-id="48ada-109">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="48ada-110">Hinzufügen eines Datenmodells</span><span class="sxs-lookup"><span data-stu-id="48ada-110">Add a data model</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="48ada-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="48ada-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="48ada-112">Klicken Sie mit der rechten Maustaste auf das Projekt **RazorPagesMovie** > **Hinzufügen** > **Neuer Ordner**.</span><span class="sxs-lookup"><span data-stu-id="48ada-112">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="48ada-113">Geben Sie dem Ordner den Namen *Modelle*.</span><span class="sxs-lookup"><span data-stu-id="48ada-113">Name the folder *Models*.</span></span>

<span data-ttu-id="48ada-114">Klicken Sie mit der rechten Maustaste auf den Ordner *Modelle*.</span><span class="sxs-lookup"><span data-stu-id="48ada-114">Right click the *Models* folder.</span></span> <span data-ttu-id="48ada-115">Wählen Sie **Hinzufügen** > **Klasse** aus.</span><span class="sxs-lookup"><span data-stu-id="48ada-115">Select **Add** > **Class**.</span></span> <span data-ttu-id="48ada-116">Nennen Sie die Klasse **Movie**.</span><span class="sxs-lookup"><span data-stu-id="48ada-116">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="48ada-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="48ada-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="48ada-118">Fügen Sie einen Ordner namens *Modelle* hinzu.</span><span class="sxs-lookup"><span data-stu-id="48ada-118">Add a folder named *Models*.</span></span>
* <span data-ttu-id="48ada-119">Fügen Sie zum Ordner *Modelle* eine Klasse mit dem Namen *Movie.cs* hinzu.</span><span class="sxs-lookup"><span data-stu-id="48ada-119">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="48ada-120">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="48ada-120">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="48ada-121">Klicken Sie im Lösungspad mit der rechten Maustaste auf das Projekt **RazorPagesMovie**, und wählen Sie dann **Hinzufügen** > **Neuer Ordner...** aus. Geben Sie dem Ordner den Namen *Modelle*.</span><span class="sxs-lookup"><span data-stu-id="48ada-121">In Solution Pad, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder...**. Name the folder *Models*.</span></span>
* <span data-ttu-id="48ada-122">Klicken Sie mit der rechten Maustaste auf den Ordner *Modelle*, und klicken Sie auf **Hinzufügen** > **Neue Datei...** .</span><span class="sxs-lookup"><span data-stu-id="48ada-122">Right-click the *Models* folder, and then select **Add** > **New File...**.</span></span>
* <span data-ttu-id="48ada-123">Führen Sie im Dialogfeld **Neue Datei** folgende Aktionen aus:</span><span class="sxs-lookup"><span data-stu-id="48ada-123">In the **New File** dialog:</span></span>

  * <span data-ttu-id="48ada-124">Klicken Sie im linken Bereich auf **Allgemein**.</span><span class="sxs-lookup"><span data-stu-id="48ada-124">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="48ada-125">Klicken Sie im mittleren Bereich auf **Leere Klasse**.</span><span class="sxs-lookup"><span data-stu-id="48ada-125">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="48ada-126">Geben Sie der Klasse den Namen **Film**, und wählen sie **Neu** aus.</span><span class="sxs-lookup"><span data-stu-id="48ada-126">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

---

<span data-ttu-id="48ada-127">Erstellen Sie das Projekt, um sicherzustellen, dass keine Kompilierungsfehler vorliegen.</span><span class="sxs-lookup"><span data-stu-id="48ada-127">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="48ada-128">Erstellen des Gerüsts für das Filmmodell</span><span class="sxs-lookup"><span data-stu-id="48ada-128">Scaffold the movie model</span></span>

<span data-ttu-id="48ada-129">In diesem Abschnitt wird das Gerüst für das Filmmodell erstellt.</span><span class="sxs-lookup"><span data-stu-id="48ada-129">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="48ada-130">Mit dem Tool für den Gerüstbau werden Seiten für die Vorgänge „Create“ (Erstellen), „Read“ (Lesen), „Update“ (Aktualisieren) und „Delete“ (Löschen), kurz CRUD-Vorgänge, für das Filmmodell erstellt.</span><span class="sxs-lookup"><span data-stu-id="48ada-130">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="48ada-131">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="48ada-131">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="48ada-132">Erstellen Sie den Ordner *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="48ada-132">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="48ada-133">Klicken Sie mit der rechten Maustaste auf den Ordner *Pages* > **Hinzufügen** > **Neuer Ordner**.</span><span class="sxs-lookup"><span data-stu-id="48ada-133">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="48ada-134">Geben Sie dem Ordner den Namen *Movies*</span><span class="sxs-lookup"><span data-stu-id="48ada-134">Name the folder *Movies*</span></span>

<span data-ttu-id="48ada-135">Klicken Sie mit der rechten Maustaste auf den Ordner *Pages/Movies* > **Hinzufügen** > **Neues Gerüstelement**.</span><span class="sxs-lookup"><span data-stu-id="48ada-135">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Abbildung der vorherigen Anweisungen.](model/_static/sca.png)

<span data-ttu-id="48ada-137">Klicken Sie im Dialogfeld **Gerüst hinzufügen** auf **Razor-Seiten mithilfe des Entity Frameworks (CRUD)** > **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="48ada-137">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Abbildung der vorherigen Anweisungen.](model/_static/add_scaffold.png)

<span data-ttu-id="48ada-139">Vervollständigen Sie das Dialogfeld **Add Razor Pages using Entity Framework (CRUD)** (Razor-Seiten mithilfe des Entity Frameworks (CRUD) hinzufügen):</span><span class="sxs-lookup"><span data-stu-id="48ada-139">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="48ada-140">Wählen Sie in der Dropdownliste **Modellklasse** den Eintrag **Film (RazorPagesMovie.Models)** aus.</span><span class="sxs-lookup"><span data-stu-id="48ada-140">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="48ada-141">Wählen Sie in der Zeile **Datenkontextklasse** das Zeichen **+** (Plus) aus, und ändern Sie den generierten Namen RazorPagesMovie.**Models**.RazorPagesMovieContext in RazorPagesMovie.**Data**.RazorPagesMovieContext.</span><span class="sxs-lookup"><span data-stu-id="48ada-141">In the **Data context class** row, select the **+** (plus) sign and change the generated name from RazorPagesMovie.**Models**.RazorPagesMovieContext to RazorPagesMovie.**Data**.RazorPagesMovieContext.</span></span> <span data-ttu-id="48ada-142">[Dieses Änderung](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) ist nicht erforderlich.</span><span class="sxs-lookup"><span data-stu-id="48ada-142">[This change](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) is not required.</span></span> <span data-ttu-id="48ada-143">Sie erstellt die Datenbank-Kontextklasse mit dem korrekten Namespace.</span><span class="sxs-lookup"><span data-stu-id="48ada-143">It creates the database context class with the correct namespace.</span></span>
* <span data-ttu-id="48ada-144">Wählen Sie **Hinzufügen** aus.</span><span class="sxs-lookup"><span data-stu-id="48ada-144">Select **Add**.</span></span>

![Abbildung der vorherigen Anweisungen.](model/_static/3/arp.png)

<span data-ttu-id="48ada-146">Die Datei *appsettings.json* wird mit der Verbindungszeichenfolge aktualisiert, die zum Herstellen einer Verbindung mit einer lokalen Datenbank verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="48ada-146">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="48ada-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="48ada-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="48ada-148">Öffnen Sie ein Befehlsfenster im Projektverzeichnis (das Verzeichnis mit den Dateien *Program.cs*, *Startup.cs*, und *CSPROJ*).</span><span class="sxs-lookup"><span data-stu-id="48ada-148">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="48ada-149">Installieren Sie das Gerüstbautool:</span><span class="sxs-lookup"><span data-stu-id="48ada-149">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="48ada-150">**Für Windows**: Führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="48ada-150">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="48ada-151">**Für MacOS und Linux**: Führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="48ada-151">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  dotnet tool install --global dotnet-aspnet-codegenerator
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

[!INCLUDE [use SQL Server in production](~/includes/RP/sqlitedev.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="48ada-152">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="48ada-152">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="48ada-153">Erstellen Sie den Ordner *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="48ada-153">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="48ada-154">Klicken Sie mit der rechten Maustaste auf den Ordner *Pages* > **Hinzufügen** > **Neuer Ordner**.</span><span class="sxs-lookup"><span data-stu-id="48ada-154">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="48ada-155">Geben Sie dem Ordner den Namen *Movies*</span><span class="sxs-lookup"><span data-stu-id="48ada-155">Name the folder *Movies*</span></span>

<span data-ttu-id="48ada-156">Klicken Sie mit der rechten Maustaste auf den Ordner *Pages/Movies* > **Hinzufügen** > **Neuer Gerüstbau...** .</span><span class="sxs-lookup"><span data-stu-id="48ada-156">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolding...**.</span></span>

![Abbildung der vorherigen Anweisungen.](model/_static/scaMac.png)

<span data-ttu-id="48ada-158">Klicken Sie im Dialogfeld **Add New Scaffolding** (Neuen Gerüstbau hinzufügen) auf **Razor-Seiten mithilfe des Entity Frameworks (CRUD)** > **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="48ada-158">In the **New Scaffolding** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Next**.</span></span>

![Abbildung der vorherigen Anweisungen.](model/_static/add_scaffoldMac.png)

<span data-ttu-id="48ada-160">Vervollständigen Sie das Dialogfeld **Add Razor Pages using Entity Framework (CRUD)** (Razor-Seiten mithilfe des Entity Frameworks (CRUD) hinzufügen):</span><span class="sxs-lookup"><span data-stu-id="48ada-160">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="48ada-161">Wählen Sie in der Dropdownliste **Modellklasse** den Eintrag **Film (RazorPagesMovie.Models)** aus, oder geben Sie ihn ein.</span><span class="sxs-lookup"><span data-stu-id="48ada-161">In the **Model class** drop down, select, or type, **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="48ada-162">Geben Sie in der Zeile **Datenkontextklasse** den Namen der neuen Klasse „RazorPagesMovie.**Data**.RazorPagesMovieContext“ ein.</span><span class="sxs-lookup"><span data-stu-id="48ada-162">In the **Data context class** row, type the name for the new class, RazorPagesMovie.**Data**.RazorPagesMovieContext.</span></span> <span data-ttu-id="48ada-163">[Dieses Änderung](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) ist nicht erforderlich.</span><span class="sxs-lookup"><span data-stu-id="48ada-163">[This change](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) is not required.</span></span> <span data-ttu-id="48ada-164">Sie erstellt die Datenbank-Kontextklasse mit dem korrekten Namespace.</span><span class="sxs-lookup"><span data-stu-id="48ada-164">It creates the database context class with the correct namespace.</span></span>
* <span data-ttu-id="48ada-165">Wählen Sie **Hinzufügen** aus.</span><span class="sxs-lookup"><span data-stu-id="48ada-165">Select **Add**.</span></span>

![Abbildung der vorherigen Anweisungen.](model/_static/arpMac.png)

<span data-ttu-id="48ada-167">Die Datei *appsettings.json* wird mit der Verbindungszeichenfolge aktualisiert, die zum Herstellen einer Verbindung mit einer lokalen Datenbank verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="48ada-167">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

### <a name="add-ef-tools"></a><span data-ttu-id="48ada-168">Hinzufügen von EF-Tools</span><span class="sxs-lookup"><span data-stu-id="48ada-168">Add EF tools</span></span>

<span data-ttu-id="48ada-169">Führen Sie den folgenden .NET Core-CLI-Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="48ada-169">Run the following .NET Core CLI command:</span></span>

```dotnetcli
dotnet tool install --global dotnet-ef
```

<span data-ttu-id="48ada-170">Der vorangehende Befehl fügt die Entity Framework Core-Tools für die .NET Core-CLI hinzu.</span><span class="sxs-lookup"><span data-stu-id="48ada-170">The preceding command adds the Entity Framework Core Tools for the .NET Core CLI.</span></span>

---

### <a name="files-created"></a><span data-ttu-id="48ada-171">Erstellte Dateien</span><span class="sxs-lookup"><span data-stu-id="48ada-171">Files created</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="48ada-172">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="48ada-172">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="48ada-173">Der Gerüstprozess erstellt und ändert folgende Dateien:</span><span class="sxs-lookup"><span data-stu-id="48ada-173">The scaffold process creates and updates the following files:</span></span>

* <span data-ttu-id="48ada-174">*Pages/Movies*: „Create“, „Delete“, „Details“, „Edit“ und „Index“.</span><span class="sxs-lookup"><span data-stu-id="48ada-174">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="48ada-175">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="48ada-175">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="updated"></a><span data-ttu-id="48ada-176">Aktualisiert</span><span class="sxs-lookup"><span data-stu-id="48ada-176">Updated</span></span>

* <span data-ttu-id="48ada-177">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="48ada-177">*Startup.cs*</span></span>

<span data-ttu-id="48ada-178">Die erstellten und aktualisierten Daten werden im nächsten Abschnitt erläutert.</span><span class="sxs-lookup"><span data-stu-id="48ada-178">The created and updated files are explained in the next section.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="48ada-179">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="48ada-179">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="48ada-180">Der Gerüstprozess erstellt und ändert folgende Dateien:</span><span class="sxs-lookup"><span data-stu-id="48ada-180">The scaffold process creates and updates the following files:</span></span>

* <span data-ttu-id="48ada-181">*Pages/Movies*: „Create“, „Delete“, „Details“, „Edit“ und „Index“.</span><span class="sxs-lookup"><span data-stu-id="48ada-181">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="48ada-182">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="48ada-182">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="updated"></a><span data-ttu-id="48ada-183">Aktualisiert</span><span class="sxs-lookup"><span data-stu-id="48ada-183">Updated</span></span>

* <span data-ttu-id="48ada-184">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="48ada-184">*Startup.cs*</span></span>

<span data-ttu-id="48ada-185">Die erstellten und aktualisierten Daten werden im nächsten Abschnitt erläutert.</span><span class="sxs-lookup"><span data-stu-id="48ada-185">The created and updated files are explained in the next section.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="48ada-186">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="48ada-186">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="48ada-187">Der Gerüstbauprozess erstellt die folgenden Dateien:</span><span class="sxs-lookup"><span data-stu-id="48ada-187">The scaffold process creates the following files:</span></span>

* <span data-ttu-id="48ada-188">*Pages/Movies*: „Create“, „Delete“, „Details“, „Edit“ und „Index“.</span><span class="sxs-lookup"><span data-stu-id="48ada-188">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>

<span data-ttu-id="48ada-189">Die erstellten Daten werden im nächsten Abschnitt erläutert.</span><span class="sxs-lookup"><span data-stu-id="48ada-189">The created files are explained in the next section.</span></span>

---

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="48ada-190">Anfängliche Migration</span><span class="sxs-lookup"><span data-stu-id="48ada-190">Initial migration</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="48ada-191">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="48ada-191">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="48ada-192">In diesem Abschnitt wird die Paket-Manager-Konsole (Package Manager Console, PMC) für Folgendes verwendet:</span><span class="sxs-lookup"><span data-stu-id="48ada-192">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="48ada-193">Fügen Sie eine anfängliche Migration hinzu.</span><span class="sxs-lookup"><span data-stu-id="48ada-193">Add an initial migration.</span></span>
* <span data-ttu-id="48ada-194">Aktualisieren Sie die Datenbank mit der anfänglichen Migration.</span><span class="sxs-lookup"><span data-stu-id="48ada-194">Update the database with the initial migration.</span></span>

<span data-ttu-id="48ada-195">Wählen Sie im Menü **Tools** **NuGet-Paket-Manager** > **Paket-Manager-Konsole** aus.</span><span class="sxs-lookup"><span data-stu-id="48ada-195">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC-Menü](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="48ada-197">Geben Sie in der PMC die folgenden Befehle ein:</span><span class="sxs-lookup"><span data-stu-id="48ada-197">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration InitialCreate
Update-Database
```

# <a name="visual-studio-code"></a>[<span data-ttu-id="48ada-198">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="48ada-198">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="48ada-199">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="48ada-199">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="48ada-200">Mit den vorherigen Befehlen wird die folgende Warnung erstellt: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span><span class="sxs-lookup"><span data-stu-id="48ada-200">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="48ada-201">Dadurch werden Werte automatisch abgeschnitten, falls diese nicht der Standardgenauigkeit und -skalierung entsprechen.</span><span class="sxs-lookup"><span data-stu-id="48ada-201">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="48ada-202">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'." („Für die Spalte „Price“ mit Dezimalwerten beim Entitätstyp „Movie“ wurde kein Typ angegeben. Dadurch werden Werte automatisch abgeschnitten, falls diese nicht der Standardgenauigkeit und -skalierung entsprechen. Geben Sie den Spaltentyp von SQL-Server an, der durch „ForHasColumnType()“ sämtliche Werte unterstützen kann.")</span><span class="sxs-lookup"><span data-stu-id="48ada-202">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="48ada-203">Sie können diese Warnung ignorieren, sie wird in einem späteren Tutorial behoben.</span><span class="sxs-lookup"><span data-stu-id="48ada-203">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="48ada-204">Mit dem Migrationsbefehl wird Code generiert, um das anfängliche Datenbankschema zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="48ada-204">The migrations command generates code to create the initial database schema.</span></span> <span data-ttu-id="48ada-205">Das Schema basiert auf dem Modell, das in `DbContext` angegeben ist.</span><span class="sxs-lookup"><span data-stu-id="48ada-205">The schema is based on the model specified in `DbContext`.</span></span> <span data-ttu-id="48ada-206">Das Argument `InitialCreate` wird verwendet, um die Migrationen zu benennen.</span><span class="sxs-lookup"><span data-stu-id="48ada-206">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="48ada-207">Es kann jeder Name verwendet werden, aber per Konvention wird ein Name ausgewählt, der die Migration beschreibt.</span><span class="sxs-lookup"><span data-stu-id="48ada-207">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="48ada-208">Der `update`-Befehl führt die `Up`-Methode in Migrationen aus, die nicht angewendet wurden.</span><span class="sxs-lookup"><span data-stu-id="48ada-208">The `update` command runs the `Up` method in migrations that have not been applied.</span></span> <span data-ttu-id="48ada-209">In diesem Fall führt `update` die `Up`-Methode in der Datei *Migrations/\<time-stamp>_InitialCreate.cs* aus, mit der die Datenbank erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="48ada-209">In this case, `update` runs the `Up` method in  *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="48ada-210">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="48ada-210">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="48ada-211">Überprüfen des mit Dependency Injection registrierten Kontexts</span><span class="sxs-lookup"><span data-stu-id="48ada-211">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="48ada-212">ASP.NET Core wird mit [Dependency Injection](xref:fundamentals/dependency-injection) erstellt.</span><span class="sxs-lookup"><span data-stu-id="48ada-212">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="48ada-213">Dienste (z.B. der Datenbankkontext Entity Framework Core) werden über Dependency Injection beim Anwendungsstart registriert.</span><span class="sxs-lookup"><span data-stu-id="48ada-213">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="48ada-214">Komponenten, die diese Dienste erfordern (z. B. Razor Pages), werden von diesen Diensten über Konstruktorparameter bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="48ada-214">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="48ada-215">Der Konstruktorcode, der eine Datenbankkontext-Instanz abruft, wird später in diesem Tutorial erläutert.</span><span class="sxs-lookup"><span data-stu-id="48ada-215">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="48ada-216">Das Gerüstbautool hat automatisch einen Datenbankkontext erstellt und diesen mit dem Dependency Injection-Container registriert.</span><span class="sxs-lookup"><span data-stu-id="48ada-216">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="48ada-217">Untersuchen Sie die Methode `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="48ada-217">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="48ada-218">Die hervorgehobene Zeile wurde vom Gerüst hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="48ada-218">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

<span data-ttu-id="48ada-219">Der `RazorPagesMovieContext` koordiniert die EF Core-Funktionen (Create, Read, Update, Delete usw.) für das `Movie`-Modell.</span><span class="sxs-lookup"><span data-stu-id="48ada-219">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="48ada-220">Der Datenkontext (`RazorPagesMovieContext`) wird aus [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="48ada-220">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="48ada-221">Der Datenkontext gibt an, welche Entitäten im Datenmodell enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="48ada-221">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="48ada-222">Der vorangehende Code erstellt eine [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1)-Eigenschaft für die Entitätsmenge.</span><span class="sxs-lookup"><span data-stu-id="48ada-222">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="48ada-223">In der Terminologie von Entity Framework entspricht eine Entitätenmenge in der Regel einer Datenbanktabelle.</span><span class="sxs-lookup"><span data-stu-id="48ada-223">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="48ada-224">Entitäten entsprechen Zeilen in Tabellen.</span><span class="sxs-lookup"><span data-stu-id="48ada-224">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="48ada-225">Der Name der Verbindungszeichenfolge wird an den Kontext übergeben, indem Sie eine Methode auf einem [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions)-Objekt aufrufen.</span><span class="sxs-lookup"><span data-stu-id="48ada-225">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="48ada-226">Für die lokale Entwicklung liest das [ASP.NET Core-Konfigurationssystem](xref:fundamentals/configuration/index) die Verbindungszeichenfolge aus der *appsettings.json*-Datei.</span><span class="sxs-lookup"><span data-stu-id="48ada-226">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="48ada-227">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="48ada-227">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="48ada-228">Untersuchen Sie die Methode `Up`.</span><span class="sxs-lookup"><span data-stu-id="48ada-228">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="48ada-229">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="48ada-229">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="48ada-230">Untersuchen Sie die Methode `Up`.</span><span class="sxs-lookup"><span data-stu-id="48ada-230">Examine the `Up` method.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="48ada-231">Testen der App</span><span class="sxs-lookup"><span data-stu-id="48ada-231">Test the app</span></span>

* <span data-ttu-id="48ada-232">Führen Sie die App aus, und fügen Sie `/Movies` an die URL im Browser an (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="48ada-232">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="48ada-233">Wenn Sie eine Fehlermeldung erhalten, müssen Sie Folgendes tun:</span><span class="sxs-lookup"><span data-stu-id="48ada-233">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="48ada-234">Sie haben den [Migrationsschritt](#pmc) verpasst.</span><span class="sxs-lookup"><span data-stu-id="48ada-234">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="48ada-235">Testen Sie den Link **Create** (Erstellen).</span><span class="sxs-lookup"><span data-stu-id="48ada-235">Test the **Create** link.</span></span>

  ![Seite „Create“](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="48ada-237">Sie können unter Umständen in das Feld `Price` keine Kommas als Dezimaltrennzeichen eingeben.</span><span class="sxs-lookup"><span data-stu-id="48ada-237">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="48ada-238">Zur Unterstützung der [jQuery-Validierung](https://jqueryvalidation.org/) für Gebietsschemas mit einer anderen Sprache als Englisch, in denen ein Komma („,“) als Dezimaltrennzeichen verwendet wird, und für Datums- und Uhrzeitformate, die nicht dem US-englischen Format entsprechen, muss die App globalisiert werden.</span><span class="sxs-lookup"><span data-stu-id="48ada-238">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="48ada-239">Globalisierungsanweisungen finden Sie unter [GitHub-Problem](https://github.com/dotnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="48ada-239">For globalization instructions, see [this GitHub issue](https://github.com/dotnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="48ada-240">Testen Sie die Links **Edit** (Bearbeiten), **Details** und **Delete** (Löschen).</span><span class="sxs-lookup"><span data-stu-id="48ada-240">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="48ada-241">Im nächsten Tutorial finden Sie Erläuterungen zu den Dateien, die durch den Gerüstbau erstellt wurden.</span><span class="sxs-lookup"><span data-stu-id="48ada-241">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="48ada-242">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="48ada-242">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="48ada-243">[Zurück: Erste Schritte](xref:tutorials/razor-pages/razor-pages-start)
> [Weiter: Gerüstbau mit Razor Pages](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="48ada-243">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end

<!--  ::: moniker previous version   -->
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="48ada-244">In diesem Abschnitt werden Klassen für die Verwaltung von Filmen in einer plattformübergreifenden [SQLite-Datenbank](https://www.sqlite.org/index.html) hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="48ada-244">In this section, classes are added for managing movies in a cross-platform [SQLite database](https://www.sqlite.org/index.html).</span></span> <span data-ttu-id="48ada-245">Apps, die aus einer ASP.NET Core-Vorlage erstellt werden, verwenden eine SQLite-Datenbank.</span><span class="sxs-lookup"><span data-stu-id="48ada-245">Apps created from an ASP.NET Core template use a SQLite database.</span></span> <span data-ttu-id="48ada-246">Die Modellklassen der App werden mit [Entity Framework Core (EF Core)](/ef/core) ([SQLite-EF Core-Datenbankanbieter](/ef/core/providers/sqlite)) verwendet, um mit der Datenbank zu arbeiten.</span><span class="sxs-lookup"><span data-stu-id="48ada-246">The app's model classes are used with [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core Database Provider](/ef/core/providers/sqlite)) to work with the database.</span></span> <span data-ttu-id="48ada-247">EF Core ist ein ORM-Framework (Objektrelationales Mapping, ORM), das den Datenzugriff vereinfacht.</span><span class="sxs-lookup"><span data-stu-id="48ada-247">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="48ada-248">Die Modellklassen werden als POCO-Klassen (von „Plain-Old CLR Objects“) bezeichnet, da sie keinerlei Abhängigkeit von EF Core aufweisen.</span><span class="sxs-lookup"><span data-stu-id="48ada-248">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="48ada-249">Sie definieren die Eigenschaften einer Datei, die in der Datenbank gespeichert ist.</span><span class="sxs-lookup"><span data-stu-id="48ada-249">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="48ada-250">Hinzufügen eines Datenmodells</span><span class="sxs-lookup"><span data-stu-id="48ada-250">Add a data model</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="48ada-251">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="48ada-251">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="48ada-252">Klicken Sie mit der rechten Maustaste auf das Projekt **RazorPagesMovie** > **Hinzufügen** > **Neuer Ordner**.</span><span class="sxs-lookup"><span data-stu-id="48ada-252">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="48ada-253">Geben Sie dem Ordner den Namen *Modelle*.</span><span class="sxs-lookup"><span data-stu-id="48ada-253">Name the folder *Models*.</span></span>

<span data-ttu-id="48ada-254">Klicken Sie mit der rechten Maustaste auf den Ordner *Modelle*.</span><span class="sxs-lookup"><span data-stu-id="48ada-254">Right click the *Models* folder.</span></span> <span data-ttu-id="48ada-255">Wählen Sie **Hinzufügen** > **Klasse** aus.</span><span class="sxs-lookup"><span data-stu-id="48ada-255">Select **Add** > **Class**.</span></span> <span data-ttu-id="48ada-256">Nennen Sie die Klasse **Movie**.</span><span class="sxs-lookup"><span data-stu-id="48ada-256">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="48ada-257">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="48ada-257">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="48ada-258">Fügen Sie einen Ordner namens *Modelle* hinzu.</span><span class="sxs-lookup"><span data-stu-id="48ada-258">Add a folder named *Models*.</span></span>
* <span data-ttu-id="48ada-259">Fügen Sie zum Ordner *Modelle* eine Klasse mit dem Namen *Movie.cs* hinzu.</span><span class="sxs-lookup"><span data-stu-id="48ada-259">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="48ada-260">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="48ada-260">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="48ada-261">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt**RazorPagesMovie**, und klicken Sie dann auf **Hinzufügen** > **Neuer Ordner**.</span><span class="sxs-lookup"><span data-stu-id="48ada-261">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="48ada-262">Geben Sie dem Ordner den Namen *Modelle*.</span><span class="sxs-lookup"><span data-stu-id="48ada-262">Name the folder *Models*.</span></span>
* <span data-ttu-id="48ada-263">Klicken Sie mit der rechten Maustaste auf den Ordner *Modelle*, und klicken Sie auf **Hinzufügen** > **Neue Datei**.</span><span class="sxs-lookup"><span data-stu-id="48ada-263">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="48ada-264">Führen Sie im Dialogfeld **Neue Datei** folgende Aktionen aus:</span><span class="sxs-lookup"><span data-stu-id="48ada-264">In the **New File** dialog:</span></span>

  * <span data-ttu-id="48ada-265">Klicken Sie im linken Bereich auf **Allgemein**.</span><span class="sxs-lookup"><span data-stu-id="48ada-265">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="48ada-266">Klicken Sie im mittleren Bereich auf **Leere Klasse**.</span><span class="sxs-lookup"><span data-stu-id="48ada-266">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="48ada-267">Geben Sie der Klasse den Namen **Film**, und wählen sie **Neu** aus.</span><span class="sxs-lookup"><span data-stu-id="48ada-267">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

---

<span data-ttu-id="48ada-268">Erstellen Sie das Projekt, um sicherzustellen, dass keine Kompilierungsfehler vorliegen.</span><span class="sxs-lookup"><span data-stu-id="48ada-268">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="48ada-269">Erstellen des Gerüsts für das Filmmodell</span><span class="sxs-lookup"><span data-stu-id="48ada-269">Scaffold the movie model</span></span>

<span data-ttu-id="48ada-270">In diesem Abschnitt wird das Gerüst für das Filmmodell erstellt.</span><span class="sxs-lookup"><span data-stu-id="48ada-270">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="48ada-271">Mit dem Tool für den Gerüstbau werden Seiten für die Vorgänge „Create“ (Erstellen), „Read“ (Lesen), „Update“ (Aktualisieren) und „Delete“ (Löschen), kurz CRUD-Vorgänge, für das Filmmodell erstellt.</span><span class="sxs-lookup"><span data-stu-id="48ada-271">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="48ada-272">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="48ada-272">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="48ada-273">Erstellen Sie den Ordner *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="48ada-273">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="48ada-274">Klicken Sie mit der rechten Maustaste auf den Ordner *Pages* > **Hinzufügen** > **Neuer Ordner**.</span><span class="sxs-lookup"><span data-stu-id="48ada-274">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="48ada-275">Geben Sie dem Ordner den Namen *Movies*</span><span class="sxs-lookup"><span data-stu-id="48ada-275">Name the folder *Movies*</span></span>

<span data-ttu-id="48ada-276">Klicken Sie mit der rechten Maustaste auf den Ordner *Pages/Movies* > **Hinzufügen** > **Neues Gerüstelement**.</span><span class="sxs-lookup"><span data-stu-id="48ada-276">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Abbildung der vorherigen Anweisungen.](model/_static/sca.png)

<span data-ttu-id="48ada-278">Klicken Sie im Dialogfeld **Gerüst hinzufügen** auf **Razor-Seiten mithilfe des Entity Frameworks (CRUD)** > **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="48ada-278">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Abbildung der vorherigen Anweisungen.](model/_static/add_scaffold.png)

<span data-ttu-id="48ada-280">Vervollständigen Sie das Dialogfeld **Add Razor Pages using Entity Framework (CRUD)** (Razor-Seiten mithilfe des Entity Frameworks (CRUD) hinzufügen):</span><span class="sxs-lookup"><span data-stu-id="48ada-280">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>
<!-- In the next section, change 
(plus) sign and accept the generated name 
to use Data, it should not use models. That will make the namespace the same for the VS version and the CLI version
-->

* <span data-ttu-id="48ada-281">Wählen Sie in der Dropdownliste **Modellklasse** den Eintrag **Film (RazorPagesMovie.Models)** aus.</span><span class="sxs-lookup"><span data-stu-id="48ada-281">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="48ada-282">Wählen Sie in der Zeile **Datenkontextklasse** das Zeichen **+** (Plus) aus, und akzeptieren Sie den generierten Namen **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="48ada-282">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="48ada-283">Wählen Sie **Hinzufügen** aus.</span><span class="sxs-lookup"><span data-stu-id="48ada-283">Select **Add**.</span></span>

![Abbildung der vorherigen Anweisungen.](model/_static/arp.png)

<span data-ttu-id="48ada-285">Die Datei *appsettings.json* wird mit der Verbindungszeichenfolge aktualisiert, die zum Herstellen einer Verbindung mit einer lokalen Datenbank verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="48ada-285">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="48ada-286">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="48ada-286">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="48ada-287">Öffnen Sie ein Befehlsfenster im Projektverzeichnis (das Verzeichnis mit den Dateien *Program.cs*, *Startup.cs*, und *CSPROJ*).</span><span class="sxs-lookup"><span data-stu-id="48ada-287">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>

* <span data-ttu-id="48ada-288">**Für Windows**: Führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="48ada-288">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="48ada-289">**Für MacOS und Linux**: Führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="48ada-289">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="48ada-290">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="48ada-290">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="48ada-291">Erstellen Sie den Ordner *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="48ada-291">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="48ada-292">Klicken Sie mit der rechten Maustaste auf den Ordner *Pages* > **Hinzufügen** > **Neuer Ordner**.</span><span class="sxs-lookup"><span data-stu-id="48ada-292">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="48ada-293">Geben Sie dem Ordner den Namen *Movies*</span><span class="sxs-lookup"><span data-stu-id="48ada-293">Name the folder *Movies*</span></span>

<span data-ttu-id="48ada-294">Klicken Sie mit der rechten Maustaste auf den Ordner *Pages/Movies* > **Hinzufügen** > **Neues Gerüstelement**.</span><span class="sxs-lookup"><span data-stu-id="48ada-294">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Abbildung der vorherigen Anweisungen.](model/_static/scaMac.png)

<span data-ttu-id="48ada-296">Klicken Sie im Dialogfeld **Add New Scaffolding** (Neuen Gerüstbau hinzufügen) auf **Razor-Seiten mithilfe des Entity Frameworks (CRUD)** > **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="48ada-296">In the **Add New Scaffolding** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Abbildung der vorherigen Anweisungen.](model/_static/add_scaffoldMac.png)

<span data-ttu-id="48ada-298">Vervollständigen Sie das Dialogfeld **Add Razor Pages using Entity Framework (CRUD)** (Razor-Seiten mithilfe des Entity Frameworks (CRUD) hinzufügen):</span><span class="sxs-lookup"><span data-stu-id="48ada-298">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="48ada-299">Wählen Sie in der Dropdownliste **Modellklasse** den Eintrag **Film** aus, oder geben Sie ihn ein.</span><span class="sxs-lookup"><span data-stu-id="48ada-299">In the **Model class** drop down, select or type **Movie**.</span></span>
* <span data-ttu-id="48ada-300">Wählen Sie in der Zeile **Datenkontextklasse** **RazorPagesMovieContext** aus, um eine neue Datenbankkontext-Klasse mit dem richtigen Namespace zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="48ada-300">In the **Data context class** row, type select the **RazorPagesMovieContext** this will create a new db context class with the correct namespace.</span></span> <span data-ttu-id="48ada-301">In diesem Fall ist das **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="48ada-301">In this case it will be  **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="48ada-302">Wählen Sie **Hinzufügen** aus.</span><span class="sxs-lookup"><span data-stu-id="48ada-302">Select **Add**.</span></span>

![Abbildung der vorherigen Anweisungen.](model/_static/arpMac.png)

<span data-ttu-id="48ada-304">Die Datei *appsettings.json* wird mit der Verbindungszeichenfolge aktualisiert, die zum Herstellen einer Verbindung mit einer lokalen Datenbank verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="48ada-304">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

---

<span data-ttu-id="48ada-305">Der Gerüstprozess erstellt und ändert folgende Dateien:</span><span class="sxs-lookup"><span data-stu-id="48ada-305">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="48ada-306">Erstellte Dateien</span><span class="sxs-lookup"><span data-stu-id="48ada-306">Files created</span></span>

* <span data-ttu-id="48ada-307">*Pages/Movies*: „Create“, „Delete“, „Details“, „Edit“ und „Index“.</span><span class="sxs-lookup"><span data-stu-id="48ada-307">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="48ada-308">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="48ada-308">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="48ada-309">Datei aktualisiert</span><span class="sxs-lookup"><span data-stu-id="48ada-309">File updated</span></span>

* <span data-ttu-id="48ada-310">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="48ada-310">*Startup.cs*</span></span>

<span data-ttu-id="48ada-311">Die erstellten und aktualisierten Daten werden im nächsten Abschnitt erläutert.</span><span class="sxs-lookup"><span data-stu-id="48ada-311">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="48ada-312">Anfängliche Migration</span><span class="sxs-lookup"><span data-stu-id="48ada-312">Initial migration</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="48ada-313">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="48ada-313">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="48ada-314">In diesem Abschnitt wird die Paket-Manager-Konsole (Package Manager Console, PMC) für Folgendes verwendet:</span><span class="sxs-lookup"><span data-stu-id="48ada-314">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="48ada-315">Fügen Sie eine anfängliche Migration hinzu.</span><span class="sxs-lookup"><span data-stu-id="48ada-315">Add an initial migration.</span></span>
* <span data-ttu-id="48ada-316">Aktualisieren Sie die Datenbank mit der anfänglichen Migration.</span><span class="sxs-lookup"><span data-stu-id="48ada-316">Update the database with the initial migration.</span></span>

<span data-ttu-id="48ada-317">Wählen Sie im Menü **Tools** **NuGet-Paket-Manager** > **Paket-Manager-Konsole** aus.</span><span class="sxs-lookup"><span data-stu-id="48ada-317">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC-Menü](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="48ada-319">Geben Sie in der PMC die folgenden Befehle ein:</span><span class="sxs-lookup"><span data-stu-id="48ada-319">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Initial
Update-Database
```

<span data-ttu-id="48ada-320">Mit dem Befehl `Add-Migration` wird Code generiert, um das anfängliche Datenbankschema zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="48ada-320">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="48ada-321">Das Schema basiert auf dem Modell, das in `DbContext` angegeben ist (in der Datei *RazorPagesMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="48ada-321">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="48ada-322">Das Argument `InitialCreate` wird verwendet, um die Migration zu benennen.</span><span class="sxs-lookup"><span data-stu-id="48ada-322">The `InitialCreate` argument is used to name the migration.</span></span> <span data-ttu-id="48ada-323">Es kann jeder Name verwendet werden, aber per Konvention wird ein Name verwendet, der die Migration beschreibt.</span><span class="sxs-lookup"><span data-stu-id="48ada-323">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="48ada-324">Weitere Informationen finden Sie unter <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="48ada-324">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="48ada-325">Der Befehl `Update-Database` führt die Methode `Up` in der Datei *Migrations/\<time-stamp>_InitialCreate.cs* aus.</span><span class="sxs-lookup"><span data-stu-id="48ada-325">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="48ada-326">Die Methode `Up` erstellt die Datenbank.</span><span class="sxs-lookup"><span data-stu-id="48ada-326">The `Up` method creates the database.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="48ada-327">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="48ada-327">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="48ada-328">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="48ada-328">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---
> [!NOTE]
> <span data-ttu-id="48ada-329">Mit den vorherigen Befehlen wird die folgende Warnung erstellt: „*Für die Spalte „Preis“ mit Dezimalwerten beim Entitätstyp „Movie“ wurde kein Typ angegeben. Dadurch werden Werte automatisch abgeschnitten, falls diese nicht der Standardgenauigkeit und -skalierung entsprechen. Geben Sie den Spaltentyp von SQL-Server an, der durch „HasColumnType()“ sämtliche Werte unterstützen kann.* “ Sie können diese Warnung ignorieren, sie wird in einem zukünftigen Tutorial korrigiert.</span><span class="sxs-lookup"><span data-stu-id="48ada-329">The preceding commands generate the following warning: "*No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'.*" You can ignore that warning, it will be fixed in a later tutorial.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="48ada-330">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="48ada-330">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="48ada-331">Überprüfen des mit Dependency Injection registrierten Kontexts</span><span class="sxs-lookup"><span data-stu-id="48ada-331">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="48ada-332">ASP.NET Core wird mit [Dependency Injection](xref:fundamentals/dependency-injection) erstellt.</span><span class="sxs-lookup"><span data-stu-id="48ada-332">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="48ada-333">Dienste (z.B. der Datenbankkontext Entity Framework Core) werden über Dependency Injection beim Anwendungsstart registriert.</span><span class="sxs-lookup"><span data-stu-id="48ada-333">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="48ada-334">Komponenten, die diese Dienste erfordern (z. B. Razor Pages), werden von diesen Diensten über Konstruktorparameter bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="48ada-334">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="48ada-335">Der Konstruktorcode, der eine Datenbankkontext-Instanz abruft, wird später in diesem Tutorial erläutert.</span><span class="sxs-lookup"><span data-stu-id="48ada-335">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="48ada-336">Das Gerüstbautool hat automatisch einen Datenbankkontext erstellt und diesen mit dem Dependency Injection-Container registriert.</span><span class="sxs-lookup"><span data-stu-id="48ada-336">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="48ada-337">Untersuchen Sie die Methode `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="48ada-337">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="48ada-338">Die hervorgehobene Zeile wurde vom Gerüst hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="48ada-338">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="48ada-339">Der `RazorPagesMovieContext` koordiniert die EF Core-Funktionen (Create, Read, Update, Delete usw.) für das `Movie`-Modell.</span><span class="sxs-lookup"><span data-stu-id="48ada-339">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="48ada-340">Der Datenkontext (`RazorPagesMovieContext`) wird aus [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="48ada-340">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="48ada-341">Der Datenkontext gibt an, welche Entitäten im Datenmodell enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="48ada-341">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="48ada-342">Der vorangehende Code erstellt eine [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1)-Eigenschaft für die Entitätsmenge.</span><span class="sxs-lookup"><span data-stu-id="48ada-342">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="48ada-343">In der Terminologie von Entity Framework entspricht eine Entitätenmenge in der Regel einer Datenbanktabelle.</span><span class="sxs-lookup"><span data-stu-id="48ada-343">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="48ada-344">Entitäten entsprechen Zeilen in Tabellen.</span><span class="sxs-lookup"><span data-stu-id="48ada-344">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="48ada-345">Der Name der Verbindungszeichenfolge wird an den Kontext übergeben, indem Sie eine Methode auf einem [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions)-Objekt aufrufen.</span><span class="sxs-lookup"><span data-stu-id="48ada-345">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="48ada-346">Für die lokale Entwicklung liest das [ASP.NET Core-Konfigurationssystem](xref:fundamentals/configuration/index) die Verbindungszeichenfolge aus der *appsettings.json*-Datei.</span><span class="sxs-lookup"><span data-stu-id="48ada-346">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="48ada-347">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="48ada-347">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="48ada-348">Untersuchen Sie die Methode `Up`.</span><span class="sxs-lookup"><span data-stu-id="48ada-348">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="48ada-349">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="48ada-349">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="48ada-350">Untersuchen Sie die Methode `Up`.</span><span class="sxs-lookup"><span data-stu-id="48ada-350">Examine the `Up` method.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="48ada-351">Testen der App</span><span class="sxs-lookup"><span data-stu-id="48ada-351">Test the app</span></span>

* <span data-ttu-id="48ada-352">Führen Sie die App aus, und fügen Sie `/Movies` an die URL im Browser an (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="48ada-352">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="48ada-353">Wenn Sie eine Fehlermeldung erhalten, müssen Sie Folgendes tun:</span><span class="sxs-lookup"><span data-stu-id="48ada-353">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="48ada-354">Sie haben den [Migrationsschritt](#pmc) verpasst.</span><span class="sxs-lookup"><span data-stu-id="48ada-354">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="48ada-355">Testen Sie den Link **Create** (Erstellen).</span><span class="sxs-lookup"><span data-stu-id="48ada-355">Test the **Create** link.</span></span>

  ![Seite „Create“](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="48ada-357">Sie können unter Umständen in das Feld `Price` keine Kommas als Dezimaltrennzeichen eingeben.</span><span class="sxs-lookup"><span data-stu-id="48ada-357">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="48ada-358">Zur Unterstützung der [jQuery-Validierung](https://jqueryvalidation.org/) für Gebietsschemas mit einer anderen Sprache als Englisch, in denen ein Komma („,“) als Dezimaltrennzeichen verwendet wird, und für Datums- und Uhrzeitformate, die nicht dem US-englischen Format entsprechen, muss die App globalisiert werden.</span><span class="sxs-lookup"><span data-stu-id="48ada-358">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="48ada-359">Globalisierungsanweisungen finden Sie unter [GitHub-Problem](https://github.com/dotnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="48ada-359">For globalization instructions, see [this GitHub issue](https://github.com/dotnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="48ada-360">Testen Sie die Links **Edit** (Bearbeiten), **Details** und **Delete** (Löschen).</span><span class="sxs-lookup"><span data-stu-id="48ada-360">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="48ada-361">Im nächsten Tutorial finden Sie Erläuterungen zu den Dateien, die durch den Gerüstbau erstellt wurden.</span><span class="sxs-lookup"><span data-stu-id="48ada-361">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="48ada-362">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="48ada-362">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="48ada-363">[Zurück: Erste Schritte](xref:tutorials/razor-pages/razor-pages-start)
> [Weiter: Gerüstbau mit Razor Pages](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="48ada-363">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end
