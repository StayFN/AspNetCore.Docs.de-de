---
title: 'Tutorial: Erstellen einer Web-API mit ASP.NET Core'
author: rick-anderson
description: Informationen zum Erstellen einer Web-API mit ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 2/25/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: tutorials/first-web-api
ms.openlocfilehash: 63f91086a7e9d71add7f7a5d58d96f46fa76353c
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/26/2020
ms.locfileid: "85407784"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core"></a><span data-ttu-id="bbbb0-103">Tutorial: Erstellen einer Web-API mit ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bbbb0-103">Tutorial: Create a web API with ASP.NET Core</span></span>

<span data-ttu-id="bbbb0-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT), [Kirk Larkin](https://twitter.com/serpent5) und [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="bbbb0-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Kirk Larkin](https://twitter.com/serpent5), and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="bbbb0-105">In diesem Tutorial lernen Sie die Grundlagen der Erstellung einer Web-API mit ASP.NET Core kennen.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-105">This tutorial teaches the basics of building a web API with ASP.NET Core.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="bbbb0-106">In diesem Tutorial lernen Sie, wie die folgenden Aufgaben ausgeführt werden:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="bbbb0-107">Erstellen eines Web-API-Projekts</span><span class="sxs-lookup"><span data-stu-id="bbbb0-107">Create a web API project.</span></span>
> * <span data-ttu-id="bbbb0-108">Hinzufügen einer Modellklasse und eines Datenbankkontexts</span><span class="sxs-lookup"><span data-stu-id="bbbb0-108">Add a model class and a database context.</span></span>
> * <span data-ttu-id="bbbb0-109">Erstellen eines Controllergerüsts mit CRUD-Methoden</span><span class="sxs-lookup"><span data-stu-id="bbbb0-109">Scaffold a controller with CRUD methods.</span></span>
> * <span data-ttu-id="bbbb0-110">Konfigurieren des Routings, der URL-Pfade und der Rückgabewerte</span><span class="sxs-lookup"><span data-stu-id="bbbb0-110">Configure routing, URL paths, and return values.</span></span>
> * <span data-ttu-id="bbbb0-111">Aufrufen der Web-API mit Postman</span><span class="sxs-lookup"><span data-stu-id="bbbb0-111">Call the web API with Postman.</span></span>

<span data-ttu-id="bbbb0-112">Abschließend steht Ihnen eine Web-API zur Verfügung, die in einer relationalen Datenbank gespeicherte To-do-Elemente verwalten kann.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-112">At the end, you have a web API that can manage "to-do" items stored in a database.</span></span>

## <a name="overview"></a><span data-ttu-id="bbbb0-113">Übersicht</span><span class="sxs-lookup"><span data-stu-id="bbbb0-113">Overview</span></span>

<span data-ttu-id="bbbb0-114">In diesem Tutorial wird die folgende API erstellt:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-114">This tutorial creates the following API:</span></span>

|<span data-ttu-id="bbbb0-115">API</span><span class="sxs-lookup"><span data-stu-id="bbbb0-115">API</span></span> | <span data-ttu-id="bbbb0-116">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="bbbb0-116">Description</span></span> | <span data-ttu-id="bbbb0-117">Anforderungstext</span><span class="sxs-lookup"><span data-stu-id="bbbb0-117">Request body</span></span> | <span data-ttu-id="bbbb0-118">Antworttext</span><span class="sxs-lookup"><span data-stu-id="bbbb0-118">Response body</span></span> |
|--- | ---- | ---- | ---- |
|`GET /api/TodoItems` | <span data-ttu-id="bbbb0-119">Alle To-do-Elemente abrufen</span><span class="sxs-lookup"><span data-stu-id="bbbb0-119">Get all to-do items</span></span> | <span data-ttu-id="bbbb0-120">Keine</span><span class="sxs-lookup"><span data-stu-id="bbbb0-120">None</span></span> | <span data-ttu-id="bbbb0-121">Array von To-do-Elementen</span><span class="sxs-lookup"><span data-stu-id="bbbb0-121">Array of to-do items</span></span>|
|`GET /api/TodoItems/{id}` | <span data-ttu-id="bbbb0-122">Ein Element nach ID abrufen</span><span class="sxs-lookup"><span data-stu-id="bbbb0-122">Get an item by ID</span></span> | <span data-ttu-id="bbbb0-123">Keine</span><span class="sxs-lookup"><span data-stu-id="bbbb0-123">None</span></span> | <span data-ttu-id="bbbb0-124">To-do-Element</span><span class="sxs-lookup"><span data-stu-id="bbbb0-124">To-do item</span></span>|
|`POST /api/TodoItems` | <span data-ttu-id="bbbb0-125">Neues Element hinzufügen</span><span class="sxs-lookup"><span data-stu-id="bbbb0-125">Add a new item</span></span> | <span data-ttu-id="bbbb0-126">To-do-Element</span><span class="sxs-lookup"><span data-stu-id="bbbb0-126">To-do item</span></span> | <span data-ttu-id="bbbb0-127">To-do-Element</span><span class="sxs-lookup"><span data-stu-id="bbbb0-127">To-do item</span></span> |
|`PUT /api/TodoItems/{id}` | <span data-ttu-id="bbbb0-128">Vorhandenes Element aktualisieren &nbsp;</span><span class="sxs-lookup"><span data-stu-id="bbbb0-128">Update an existing item &nbsp;</span></span> | <span data-ttu-id="bbbb0-129">To-do-Element</span><span class="sxs-lookup"><span data-stu-id="bbbb0-129">To-do item</span></span> | <span data-ttu-id="bbbb0-130">Keine</span><span class="sxs-lookup"><span data-stu-id="bbbb0-130">None</span></span> |
|<span data-ttu-id="bbbb0-131">`DELETE /api/TodoItems/{id}` &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="bbbb0-131">`DELETE /api/TodoItems/{id}` &nbsp; &nbsp;</span></span> | <span data-ttu-id="bbbb0-132">Löschen eines Elements &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="bbbb0-132">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="bbbb0-133">Keine</span><span class="sxs-lookup"><span data-stu-id="bbbb0-133">None</span></span> | <span data-ttu-id="bbbb0-134">Keine</span><span class="sxs-lookup"><span data-stu-id="bbbb0-134">None</span></span>|

<span data-ttu-id="bbbb0-135">Das folgende Diagramm zeigt den Entwurf der App.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-135">The following diagram shows the design of the app.</span></span>

![Der Client ist das Feld ganz links.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="bbbb0-141">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="bbbb0-141">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="bbbb0-142">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bbbb0-142">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="bbbb0-143">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bbbb0-143">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="bbbb0-144">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="bbbb0-144">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="bbbb0-145">Erstellen eines Webprojekts</span><span class="sxs-lookup"><span data-stu-id="bbbb0-145">Create a web project</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="bbbb0-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bbbb0-146">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="bbbb0-147">Klicken Sie im Menü **Datei** auf **Neu** > **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-147">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="bbbb0-148">Wählen Sie die Vorlage **ASP.NET Core-Webanwendung** aus, und klicken Sie auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-148">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="bbbb0-149">Geben Sie dem Projekt den Namen *TodoApi*, und klicken Sie auf **Erstellen**.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-149">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="bbbb0-150">Vergewissern Sie sich, dass im Dialogfeld **Neue ASP.NET Core-Webanwendung erstellen** die Optionen **.NET Core** und **ASP.NET Core 3.1** ausgewählt sind.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-150">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.1** are selected.</span></span> <span data-ttu-id="bbbb0-151">Wählen Sie die Vorlage **API** aus, und klicken Sie auf **Erstellen**.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-151">Select the **API** template and click **Create**.</span></span>

![VS-Dialogfeld „Neues Projekt“](first-web-api/_static/vs3.png)

# <a name="visual-studio-code"></a>[<span data-ttu-id="bbbb0-153">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bbbb0-153">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="bbbb0-154">Öffnen Sie das [integrierte Terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="bbbb0-154">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="bbbb0-155">Wechseln Sie mit dem Befehl `cd`' zu dem Ordner, der den Projektordner enthalten soll.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-155">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="bbbb0-156">Führen Sie die folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-156">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapi -o TodoApi
   cd TodoApi
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   code -r ../TodoApi
   ```

* <span data-ttu-id="bbbb0-157">Wenn Sie in einem Dialogfeld angeben müssen, ob Sie dem Projekt die erforderlichen Elemente hinzufügen möchten, wählen Sie **Ja** aus.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-157">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

  <span data-ttu-id="bbbb0-158">Die obenstehenden Befehle haben folgende Konsequenzen:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-158">The preceding commands:</span></span>

  * <span data-ttu-id="bbbb0-159">Ein neues Web-API-Projekt wird erstellt und in Visual Studio Code geöffnet.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-159">Creates a new web API project and opens it in Visual Studio Code.</span></span>
  * <span data-ttu-id="bbbb0-160">Die NuGet-Pakete, die im nächsten Abschnitt erforderlich sind, werden hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-160">Adds the NuGet packages which are required in the next section.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="bbbb0-161">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="bbbb0-161">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="bbbb0-162">Klicken Sie auf **Datei** > **Neue Projektmappe**.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-162">Select **File** > **New Solution**.</span></span>

  ![Neue Projektmappe in macOS](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="bbbb0-164">Navigieren Sie in Visual Studio für Mac-Versionen vor Version 8.6 zu **.NET Core** > **App** > **API** > **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-164">In Visual Studio for Mac earlier than version 8.6, select **.NET Core** > **App** > **API** > **Next**.</span></span> <span data-ttu-id="bbbb0-165">Bei Version 8.6 oder höher klicken Sie auf **Web and Console** > **App** > **API** > **Weiter** (Web und Konsole).</span><span class="sxs-lookup"><span data-stu-id="bbbb0-165">In version 8.6 or later, select **Web and Console** > **App** > **API** > **Next** .</span></span>

  ![Auswählen von macOS-API-Vorlagen](first-web-api-mac/_static/api_template.png)

* <span data-ttu-id="bbbb0-167">Vergewissern Sie sich, dass das **Zielframework** auf **.NET Core 3.1** festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-167">Confirm the **Target Framework** is set to **.NET Core 3.1**.</span></span> <span data-ttu-id="bbbb0-168">Klicken Sie auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-168">Select **Next**.</span></span>

  ![Auswahl für .NET Core 3.1 in macOS](first-web-api-mac/_static/api_31_config.png)

* <span data-ttu-id="bbbb0-170">Geben Sie für **Projektname** *TodoApi* ein, und wählen Sie dann **Erstellen** aus.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-170">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![Dialogfeld „Konfiguration“](first-web-api-mac/_static/2.png)

[!INCLUDE[](~/includes/mac-terminal-access.md)]

<span data-ttu-id="bbbb0-172">Öffnen Sie im Projektordner ein Befehlsterminal, und führen Sie die folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-172">Open a command terminal in the project folder and run the following commands:</span></span>

   ```dotnetcli
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   ```

---

### <a name="test-the-api"></a><span data-ttu-id="bbbb0-173">Testen der API</span><span class="sxs-lookup"><span data-stu-id="bbbb0-173">Test the API</span></span>

<span data-ttu-id="bbbb0-174">Die Projektvorlage erstellt eine `WeatherForecast`-API.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-174">The project template creates a `WeatherForecast` API.</span></span> <span data-ttu-id="bbbb0-175">Rufen Sie in einem Browser die `Get`-Methode zum Testen der App auf.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-175">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="bbbb0-176">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bbbb0-176">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="bbbb0-177">Drücken Sie STRG+F5, um die App auszuführen.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-177">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="bbbb0-178">Visual Studio startet einen Browser und navigiert zu `https://localhost:<port>/WeatherForecast`, wobei es sich bei `<port>` um eine zufällig ausgewählte Portnummer handelt.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-178">Visual Studio launches a browser and navigates to `https://localhost:<port>/WeatherForecast`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="bbbb0-179">Wenn Sie in einem Dialogfeld angeben müssen, ob Sie dem IIS Express-Zertifikat vertrauen, wählen Sie **Ja** aus.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-179">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="bbbb0-180">Wählen Sie im folgenden Dialogfeld **Sicherheitswarnung** die Option **Ja** aus.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-180">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="bbbb0-181">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bbbb0-181">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="bbbb0-182">Drücken Sie STRG+F5, um die App auszuführen.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-182">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="bbbb0-183">Navigieren Sie in einem Browser zur folgenden URL: `https://localhost:5001/WeatherForecast`.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-183">In a browser, go to following URL: `https://localhost:5001/WeatherForecast`.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="bbbb0-184">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="bbbb0-184">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="bbbb0-185">Wählen Sie **Ausführen** > **Debugging starten** aus, um die App zu starten.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-185">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="bbbb0-186">Visual Studio für Mac startet einen Browser und navigiert zu `https://localhost:<port>`, wobei es sich bei `<port>` um eine zufällig ausgewählte Portnummer handelt.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-186">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="bbbb0-187">Der Fehler „HTTP 404: Nicht gefunden“ wird zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-187">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="bbbb0-188">Fügen Sie `/WeatherForecast` an die URL an, d. h., ändern Sie sie in `https://localhost:<port>/WeatherForecast`.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-188">Append `/WeatherForecast` to the URL (change the URL to `https://localhost:<port>/WeatherForecast`).</span></span>

---

<span data-ttu-id="bbbb0-189">Der zurückgegebene JSON-Code sieht in etwa wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-189">JSON similar to the following is returned:</span></span>

```json
[
    {
        "date": "2019-07-16T19:04:05.7257911-06:00",
        "temperatureC": 52,
        "temperatureF": 125,
        "summary": "Mild"
    },
    {
        "date": "2019-07-17T19:04:05.7258461-06:00",
        "temperatureC": 36,
        "temperatureF": 96,
        "summary": "Warm"
    },
    {
        "date": "2019-07-18T19:04:05.7258467-06:00",
        "temperatureC": 39,
        "temperatureF": 102,
        "summary": "Cool"
    },
    {
        "date": "2019-07-19T19:04:05.7258471-06:00",
        "temperatureC": 10,
        "temperatureF": 49,
        "summary": "Bracing"
    },
    {
        "date": "2019-07-20T19:04:05.7258474-06:00",
        "temperatureC": -1,
        "temperatureF": 31,
        "summary": "Chilly"
    }
]
```

## <a name="add-a-model-class"></a><span data-ttu-id="bbbb0-190">Hinzufügen einer Modellklasse</span><span class="sxs-lookup"><span data-stu-id="bbbb0-190">Add a model class</span></span>

<span data-ttu-id="bbbb0-191">Ein *Modell* ist eine Gruppe von Klassen, die die Daten darstellen, die die App verwaltet.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-191">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="bbbb0-192">Das Modell für diese App ist eine einzelne `TodoItem`-Klasse.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-192">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="bbbb0-193">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bbbb0-193">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="bbbb0-194">Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-194">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="bbbb0-195">Klicken Sie auf **Hinzufügen** > **Neuer Ordner**.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-195">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="bbbb0-196">Geben Sie dem Ordner den Namen *Modelle*.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-196">Name the folder *Models*.</span></span>

* <span data-ttu-id="bbbb0-197">Klicken Sie mit der rechten Maustaste auf den Ordner *Modelle*, und wählen Sie **Hinzufügen** > **Klasse** aus.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-197">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="bbbb0-198">Nennen Sie die Klasse *TodoItem*, und wählen Sie **Hinzufügen** aus.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-198">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="bbbb0-199">Ersetzen Sie den Vorlagencode durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-199">Replace the template code with the following code:</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="bbbb0-200">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bbbb0-200">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="bbbb0-201">Fügen Sie einen Ordner namens *Modelle* hinzu.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-201">Add a folder named *Models*.</span></span>

* <span data-ttu-id="bbbb0-202">Fügen Sie dem Ordner *Modelle* mit dem folgenden Code eine `TodoItem`-Klasse hinzu:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-202">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="bbbb0-203">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="bbbb0-203">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="bbbb0-204">Klicken Sie mit der rechten Maustaste auf das Projekt.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-204">Right-click the project.</span></span> <span data-ttu-id="bbbb0-205">Klicken Sie auf **Hinzufügen** > **Neuer Ordner**.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-205">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="bbbb0-206">Geben Sie dem Ordner den Namen *Modelle*.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-206">Name the folder *Models*.</span></span>

  ![Neuer Ordner](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="bbbb0-208">Klicken Sie mit der rechten Maustaste auf den Ordner *Modelle*, und wählen Sie **Hinzufügen** > **Neue Datei** > **Allgemein** > **Leere Klasse** aus.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-208">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="bbbb0-209">Nennen Sie die Klasse *TodoItem*, und klicken Sie dann auf **Neu**.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-209">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="bbbb0-210">Ersetzen Sie den Vorlagencode durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-210">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoItem.cs?name=snippet)]

<span data-ttu-id="bbbb0-211">Die `Id`-Eigenschaft fungiert als eindeutiger Schlüssel in einer relationalen Datenbank.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-211">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="bbbb0-212">Modellklassen können überall im Projekt platziert werden, doch gemäß der Konvention wird der Ordner *Modelle* verwendet.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-212">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="bbbb0-213">Hinzufügen eines Datenbankkontexts</span><span class="sxs-lookup"><span data-stu-id="bbbb0-213">Add a database context</span></span>

<span data-ttu-id="bbbb0-214">Der *Datenbankkontext* ist die Hauptklasse, die die Entity Framework-Funktionen für ein Datenmodell koordiniert.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-214">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="bbbb0-215">Diese Klasse wird durch Ableiten von der `Microsoft.EntityFrameworkCore.DbContext`-Klasse erstellt.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-215">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="bbbb0-216">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bbbb0-216">Visual Studio</span></span>](#tab/visual-studio)

### <a name="add-microsoftentityframeworkcoresqlserver"></a><span data-ttu-id="bbbb0-217">Hinzufügen von Microsoft.EntityFrameworkCore.SqlServer</span><span class="sxs-lookup"><span data-stu-id="bbbb0-217">Add Microsoft.EntityFrameworkCore.SqlServer</span></span>

* <span data-ttu-id="bbbb0-218">Klicken Sie im Menü **Extras** auf **NuGet-Paket-Manager > NuGet-Pakete für Projektmappe verwalten...** .</span><span class="sxs-lookup"><span data-stu-id="bbbb0-218">From the **Tools** menu, select **NuGet Package Manager > Manage NuGet Packages for Solution**.</span></span>
* <span data-ttu-id="bbbb0-219">Klicken Sie auf die Registerkarte **Durchsuchen**, und geben Sie dann **Microsoft.EntityFrameworkCore.SqlServer** in das Suchfeld ein.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-219">Select the **Browse** tab, and then enter **Microsoft.EntityFrameworkCore.SqlServer** in the search box.</span></span>
* <span data-ttu-id="bbbb0-220">Wählen Sie im linken Bereich **Microsoft.EntityFrameworkCore.SqlServer** aus.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-220">Select **Microsoft.EntityFrameworkCore.SqlServer** in the left pane.</span></span>
* <span data-ttu-id="bbbb0-221">Aktivieren Sie das Kontrollkästchen **Projekt** im rechten Bereich, und klicken Sie dann auf **Installieren**.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-221">Select the **Project** check box in the right pane and then select **Install**.</span></span>
* <span data-ttu-id="bbbb0-222">Fügen Sie mithilfe der obigen Schritte das `Microsoft.EntityFrameworkCore.InMemory`-NuGet-Paket hinzu.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-222">Use the preceding instructions to add the `Microsoft.EntityFrameworkCore.InMemory` NuGet package.</span></span>

![NuGet-Paket-Manager](first-web-api/_static/vs3NuGet.png)

## <a name="add-the-todocontext-database-context"></a><span data-ttu-id="bbbb0-224">Hinzufügen des TodoContext-Datenbankkontexts</span><span class="sxs-lookup"><span data-stu-id="bbbb0-224">Add the TodoContext database context</span></span>

* <span data-ttu-id="bbbb0-225">Klicken Sie mit der rechten Maustaste auf den Ordner *Modelle*, und wählen Sie **Hinzufügen** > **Klasse** aus.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-225">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="bbbb0-226">Nennen Sie die Klasse *TodoContext*, und klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-226">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="bbbb0-227">Visual Studio Code / Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="bbbb0-227">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="bbbb0-228">Fügen Sie eine `TodoContext`-Klasse zum Ordner *Modelle* hinzu.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-228">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="bbbb0-229">Geben Sie den folgenden Code ein:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-229">Enter the following code:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="bbbb0-230">Registrieren des Datenbankkontexts</span><span class="sxs-lookup"><span data-stu-id="bbbb0-230">Register the database context</span></span>

<span data-ttu-id="bbbb0-231">In ASP.NET Core müssen Dienste wie der Datenbankkontext mit dem Container [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection) registriert werden.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-231">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="bbbb0-232">Der Container stellt den Dienst für Controller bereit.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-232">The container provides the service to controllers.</span></span>

<span data-ttu-id="bbbb0-233">Aktualisieren Sie *Startup.cs* mit dem folgenden hervorgehobenen Code:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-233">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Startup.cs?highlight=7-8,23-24&name=snippet_all)]

<span data-ttu-id="bbbb0-234">Der vorangehende Code:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-234">The preceding code:</span></span>

* <span data-ttu-id="bbbb0-235">Entfernt nicht benötigte `using`-Deklarationen</span><span class="sxs-lookup"><span data-stu-id="bbbb0-235">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="bbbb0-236">Fügt dem Abhängigkeitscontainer den Datenbankkontext hinzu</span><span class="sxs-lookup"><span data-stu-id="bbbb0-236">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="bbbb0-237">Gibt an, dass der Datenbankkontext eine In-Memory Database verwendet</span><span class="sxs-lookup"><span data-stu-id="bbbb0-237">Specifies that the database context will use an in-memory database.</span></span>

## <a name="scaffold-a-controller"></a><span data-ttu-id="bbbb0-238">Erstellen eines Controllergerüsts</span><span class="sxs-lookup"><span data-stu-id="bbbb0-238">Scaffold a controller</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="bbbb0-239">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bbbb0-239">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="bbbb0-240">Klicken Sie mit der rechten Maustaste auf den Ordner *Controller*.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-240">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="bbbb0-241">Wählen Sie **Hinzufügen** > **Neues Gerüstelement** aus.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-241">Select **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="bbbb0-242">Klicken Sie auf **API-Controller mit Aktionen unter Verwendung von Entity Framework** und dann auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-242">Select **API Controller with actions, using Entity Framework**, and then select **Add**.</span></span>
* <span data-ttu-id="bbbb0-243">Führen Sie im Dialogfeld **API-Controller mit Aktionen unter Verwendung von Entity Framework** folgende Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-243">In the **Add API Controller with actions, using Entity Framework** dialog:</span></span>

  * <span data-ttu-id="bbbb0-244">Wählen Sie in der **Modellklasse** das Element **TodoItem (TodoApi.Models)** aus.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-244">Select **TodoItem (TodoApi.Models)** in the **Model class**.</span></span>
  * <span data-ttu-id="bbbb0-245">Wählen Sie in der **Datenkontextklasse** das Element **TodoContext (TodoApi.Models)** aus.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-245">Select **TodoContext (TodoApi.Models)** in the **Data context class**.</span></span>
  * <span data-ttu-id="bbbb0-246">Wählen Sie **Hinzufügen** aus.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-246">Select **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="bbbb0-247">Visual Studio Code / Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="bbbb0-247">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="bbbb0-248">Führen Sie die folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-248">Run the following commands:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator controller -name TodoItemsController -async -api -m TodoItem -dc TodoContext -outDir Controllers
```

<span data-ttu-id="bbbb0-249">Die obenstehenden Befehle haben folgende Konsequenzen:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-249">The preceding commands:</span></span>

* <span data-ttu-id="bbbb0-250">Die NuGet-Pakete für den Gerüstbau werden hinzufügt.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-250">Add NuGet packages required for scaffolding.</span></span>
* <span data-ttu-id="bbbb0-251">Die Gerüstbau-Engine (`dotnet-aspnet-codegenerator`) wird installiert.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-251">Installs the scaffolding engine (`dotnet-aspnet-codegenerator`).</span></span>
* <span data-ttu-id="bbbb0-252">Das Gerüst für `TodoItemsController` wird erstellt.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-252">Scaffolds the `TodoItemsController`.</span></span>

---

<span data-ttu-id="bbbb0-253">Der generierte Code hat folgende Auswirkungen:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-253">The generated code:</span></span>

* <span data-ttu-id="bbbb0-254">Markiert die Klasse mit dem [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute)-Attribut.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-254">Marks the class with the [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="bbbb0-255">Dieses Attribut gibt an, dass der Controller auf Web-API-Anforderungen reagiert.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-255">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="bbbb0-256">Weitere Informationen zu bestimmten Verhaltensweisen, die das Attribut ermöglicht, finden Sie unter <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-256">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="bbbb0-257">Verwendet die Abhängigkeitsinjektion, um den Datenbankkontext (`TodoContext`) dem Controller hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-257">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="bbbb0-258">Der Datenbankkontext wird in den einzelnen [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete)-Methoden im Controller verwendet.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-258">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

<span data-ttu-id="bbbb0-259">Die ASP.NET Core-Vorlagen für:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-259">The ASP.NET Core templates for:</span></span>

* <span data-ttu-id="bbbb0-260">Controller mit Ansichten enthalten `[action]` in der Routenvorlage.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-260">Controllers with views include `[action]` in the route template.</span></span>
* <span data-ttu-id="bbbb0-261">API-Controller enthalten keine `[action]` in der Routenvorlage.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-261">API controllers don't include `[action]` in the route template.</span></span>

<span data-ttu-id="bbbb0-262">Wenn sich das `[action]`-Token nicht in der Routenvorlage befindet, wird der [action](xref:mvc/controllers/routing#action)-Name von der Route ausgeschlossen.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-262">When the `[action]` token isn't in the route template, the [action](xref:mvc/controllers/routing#action) name is excluded from the route.</span></span> <span data-ttu-id="bbbb0-263">Dies bedeutet, dass der zugehörige Methodenname der Aktion nicht in der übereinstimmenden Route verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-263">That is, the action's associated method name isn't used in the matching route.</span></span>

## <a name="examine-the-posttodoitem-create-method"></a><span data-ttu-id="bbbb0-264">Überblick über die PostTodoItem-Erstellungsmethode</span><span class="sxs-lookup"><span data-stu-id="bbbb0-264">Examine the PostTodoItem create method</span></span>

<span data-ttu-id="bbbb0-265">Ersetzen Sie die Rückgabeanweisung in `PostTodoItem` durch eine Anweisung mit dem [nameof](/dotnet/csharp/language-reference/operators/nameof)-Operator:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-265">Replace the return statement in the `PostTodoItem` to use the [nameof](/dotnet/csharp/language-reference/operators/nameof) operator:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Create)]

<span data-ttu-id="bbbb0-266">Der oben stehende Code ist eine HTTP POST-Methode, wie durch das [`[HttpPost]`](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute)-Attribut angegeben.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-266">The preceding code is an HTTP POST method, as indicated by the [`[HttpPost]`](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="bbbb0-267">Die Methode ruft den Wert der Aufgabe aus dem Text der HTTP-Anforderung ab.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-267">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="bbbb0-268">Die <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*>-Methode:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-268">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> method:</span></span>

* <span data-ttu-id="bbbb0-269">Gibt den HTTP-Statuscode 201 zurück, wenn der Vorgang erfolgreich ist.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-269">Returns an HTTP 201 status code if successful.</span></span> <span data-ttu-id="bbbb0-270">HTTP 201 ist die Standardantwort für eine HTTP POST-Methode, die eine neue Ressource auf dem Server erstellt.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-270">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="bbbb0-271">Fügt der Antwort einen [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location)-Header hinzu.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-271">Adds a [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) header to the response.</span></span> <span data-ttu-id="bbbb0-272">Der `Location`-Header legt den [URI](https://developer.mozilla.org/docs/Glossary/URI) des neu erstellten To-do-Elements fest.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-272">The `Location` header specifies the [URI](https://developer.mozilla.org/docs/Glossary/URI) of the newly created to-do item.</span></span> <span data-ttu-id="bbbb0-273">Weitere Informationen finden Sie unter [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="bbbb0-273">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="bbbb0-274">Verweist auf die `GetTodoItem`-Aktion zum Erstellen des URIs des `Location`-Headers.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-274">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="bbbb0-275">Das `nameof`-Schlüsselwort von C# wird verwendet, um eine Hartcodierung des Aktionsnamens im `CreatedAtAction`-Aufruf zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-275">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

### <a name="install-postman"></a><span data-ttu-id="bbbb0-276">Installieren von Postman</span><span class="sxs-lookup"><span data-stu-id="bbbb0-276">Install Postman</span></span>

<span data-ttu-id="bbbb0-277">Dieses Tutorial verwendet Postman zum Testen der Web-API.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-277">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="bbbb0-278">Installieren Sie [Postman](https://www.getpostman.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="bbbb0-278">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
* <span data-ttu-id="bbbb0-279">Starten Sie die Web-App.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-279">Start the web app.</span></span>
* <span data-ttu-id="bbbb0-280">Starten Sie Postman.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-280">Start Postman.</span></span>
* <span data-ttu-id="bbbb0-281">Deaktivieren Sie **SSL certificate verification** (Verifizierung des SSL-Zertifikats).</span><span class="sxs-lookup"><span data-stu-id="bbbb0-281">Disable **SSL certificate verification**</span></span>
  * <span data-ttu-id="bbbb0-282">Deaktivieren Sie auf der Registerkarte **General** (Allgemein) unter File > Settings (**Datei** > **Einstellungen**) **SSL certificate verification** (Verifizierung des SSL-Zertifikats).</span><span class="sxs-lookup"><span data-stu-id="bbbb0-282">From **File** > **Settings** (**General** tab), disable **SSL certificate verification**.</span></span>
    > [!WARNING]
    > <span data-ttu-id="bbbb0-283">Aktivieren Sie die Verifizierung des SSL-Zertifikats wieder, nachdem Sie den Controller getestet haben.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-283">Re-enable SSL certificate verification after testing the controller.</span></span>

<a name="post"></a>

### <a name="test-posttodoitem-with-postman"></a><span data-ttu-id="bbbb0-284">Testen von PostTodoItem mit Postman</span><span class="sxs-lookup"><span data-stu-id="bbbb0-284">Test PostTodoItem with Postman</span></span>

* <span data-ttu-id="bbbb0-285">Erstellen Sie eine neue Anforderung.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-285">Create a new request.</span></span>
* <span data-ttu-id="bbbb0-286">Legen Sie als HTTP-Methode `POST` fest.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-286">Set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="bbbb0-287">Wählen Sie die Registerkarte **Body** (Text) aus.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-287">Select the **Body** tab.</span></span>
* <span data-ttu-id="bbbb0-288">Wählen Sie das Optionsfeld **raw** (Unformatiert) aus.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-288">Select the **raw** radio button.</span></span>
* <span data-ttu-id="bbbb0-289">Legen Sie den Typ auf **JSON (application/json)** fest.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-289">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="bbbb0-290">Geben Sie für die Aufgabe den Anforderungstext im JSON-Format ein:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-290">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="bbbb0-291">Wählen Sie **Send** (Senden) aus.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-291">Select **Send**.</span></span>

  ![Postman mit erstellter Anforderung](first-web-api/_static/3/create.png)

### <a name="test-the-location-header-uri"></a><span data-ttu-id="bbbb0-293">Testen des Adressheader-URIs</span><span class="sxs-lookup"><span data-stu-id="bbbb0-293">Test the location header URI</span></span>

* <span data-ttu-id="bbbb0-294">Wählen Sie auf der Registerkarte **Header** (Header) den Bereich **Response** (Antwort) aus.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-294">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="bbbb0-295">Kopieren Sie den den Headerwert von **Location** (Speicherort):</span><span class="sxs-lookup"><span data-stu-id="bbbb0-295">Copy the **Location** header value:</span></span>

  ![Registerkarte „Header“ in der Postman-Konsole](first-web-api/_static/3/create.png)

* <span data-ttu-id="bbbb0-297">Legen Sie die Methode auf „GET“ fest.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-297">Set the method to GET.</span></span>
* <span data-ttu-id="bbbb0-298">Fügen Sie den URI (z. B. `https://localhost:5001/api/TodoItems/1`) ein.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-298">Paste the URI (for example, `https://localhost:5001/api/TodoItems/1`).</span></span>
* <span data-ttu-id="bbbb0-299">Wählen Sie **Send** (Senden) aus.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-299">Select **Send**.</span></span>

## <a name="examine-the-get-methods"></a><span data-ttu-id="bbbb0-300">Überblick über die GET-Methoden</span><span class="sxs-lookup"><span data-stu-id="bbbb0-300">Examine the GET methods</span></span>

<span data-ttu-id="bbbb0-301">Diese Methoden implementieren zwei GET-Endpunkte:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-301">These methods implement two GET endpoints:</span></span>

* `GET /api/TodoItems`
* `GET /api/TodoItems/{id}`

<span data-ttu-id="bbbb0-302">Testen Sie die App, indem Sie die beiden Endpunkte in einem Browser oder über Postman aufrufen.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-302">Test the app by calling the two endpoints from a browser or Postman.</span></span> <span data-ttu-id="bbbb0-303">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-303">For example:</span></span>

* `https://localhost:5001/api/TodoItems`
* `https://localhost:5001/api/TodoItems/1`

<span data-ttu-id="bbbb0-304">Durch einen Aufruf von `GetTodoItems` wird eine Antwort gesendet, die der folgenden ähnelt:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-304">A response similar to the following is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

### <a name="test-get-with-postman"></a><span data-ttu-id="bbbb0-305">Testen von GET mit Postman</span><span class="sxs-lookup"><span data-stu-id="bbbb0-305">Test Get with Postman</span></span>

* <span data-ttu-id="bbbb0-306">Erstellen Sie eine neue Anforderung.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-306">Create a new request.</span></span>
* <span data-ttu-id="bbbb0-307">Legen Sie die HTTP-Methode auf **GET** fest.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-307">Set the HTTP method to **GET**.</span></span>
* <span data-ttu-id="bbbb0-308">Legen Sie die Anforderungs-URL auf `https://localhost:<port>/api/TodoItems` fest.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-308">Set the request URL to `https://localhost:<port>/api/TodoItems`.</span></span> <span data-ttu-id="bbbb0-309">Beispielsweise `https://localhost:5001/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-309">For example, `https://localhost:5001/api/TodoItems`.</span></span>
* <span data-ttu-id="bbbb0-310">Wählen Sie in Postman **Two pane view** (Ansicht mit zwei Bereichen) aus.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-310">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="bbbb0-311">Wählen Sie **Send** (Senden) aus.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-311">Select **Send**.</span></span>

<span data-ttu-id="bbbb0-312">Diese App verwendet eine In-Memory-Datenbank.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-312">This app uses an in-memory database.</span></span> <span data-ttu-id="bbbb0-313">Wenn die App angehalten und dann wieder gestartet wird, werden durch die vorherige GET-Anforderung keine Daten zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-313">If the app is stopped and started, the preceding GET request will not return any data.</span></span> <span data-ttu-id="bbbb0-314">Wenn keine Daten zurückgegeben werden, senden Sie mit [POST](#post) Daten an die App.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-314">If no data is returned, [POST](#post) data to the app.</span></span>

## <a name="routing-and-url-paths"></a><span data-ttu-id="bbbb0-315">Routing und URL-Pfade</span><span class="sxs-lookup"><span data-stu-id="bbbb0-315">Routing and URL paths</span></span>

<span data-ttu-id="bbbb0-316">Das Attribut [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) gibt eine Methode an, die auf eine HTTP GET-Anforderung antwortet.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-316">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="bbbb0-317">Der URL-Pfad für jede Methode wird wie folgt erstellt:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-317">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="bbbb0-318">Beginnen Sie mit der Vorlagenzeichenfolge im `Route`-Attribut des Controllers:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-318">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=TodoController&highlight=1)]

* <span data-ttu-id="bbbb0-319">Ersetzen Sie `[controller]` durch den Namen des Controllers, bei dem es sich standardmäßig um den Namen der Controller-Klasse ohne das Suffix „Controller“ handelt.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-319">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="bbbb0-320">In diesem Beispiel ist der Klassenname des Controllers „**TodoItems**Controller“. Der Controllername lautet also „TodoItems“.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-320">For this sample, the controller class name is **TodoItems**Controller, so the controller name is "TodoItems".</span></span> <span data-ttu-id="bbbb0-321">Beim ASP.NET Core-[Routing](xref:mvc/controllers/routing) wird die Groß-/Kleinschreibung nicht beachtet.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-321">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="bbbb0-322">Wenn das `[HttpGet]`-Attribut eine Routenvorlage (z. B. `[HttpGet("products")]`) hat, fügen Sie diese an den Pfad an.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-322">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="bbbb0-323">In diesem Beispiel wird keine Vorlage verwendet.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-323">This sample doesn't use a template.</span></span> <span data-ttu-id="bbbb0-324">Weitere Informationen finden Sie unter [Attributrouting mit Http[Verb]-Attributen](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="bbbb0-324">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="bbbb0-325">In der folgenden `GetTodoItem`-Methode ist `"{id}"` eine Platzhaltervariable für den eindeutigen Bezeichner des To-do-Elements.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-325">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="bbbb0-326">Wenn `GetTodoItem` aufgerufen wird, wird der Wert von `"{id}"` in der URL der Methode in ihrem Parameter `id` bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-326">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its `id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="bbbb0-327">Rückgabewerte</span><span class="sxs-lookup"><span data-stu-id="bbbb0-327">Return values</span></span>

<span data-ttu-id="bbbb0-328">Der Rückgabetyp der Methoden `GetTodoItems` und `GetTodoItem` ist der [ActionResult\<T>-Typ](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="bbbb0-328">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="bbbb0-329">ASP.NET Core serialisiert automatisch das Objekt in [JSON](https://www.json.org/) und schreibt den JSON-Code in den Text der Antwortnachricht.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-329">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="bbbb0-330">Der Antwortcode für diesen Rückgabetyp ist 200, vorausgesetzt, es gibt keine Ausnahmefehler.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-330">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="bbbb0-331">Nicht behandelte Ausnahmen werden in 5xx-Fehler übersetzt.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-331">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="bbbb0-332">`ActionResult`-Rückgabetypen können eine Vielzahl von HTTP-Statuscodes darstellen.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-332">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="bbbb0-333">Beispielsweise kann `GetTodoItem` zwei verschiedene Statuswerte zurückgeben:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-333">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="bbbb0-334">Wenn kein Element mit der angeforderten ID übereinstimmt, gibt die Methode einen 404-Fehlercode [Nicht gefunden](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) zurück.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-334">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="bbbb0-335">Andernfalls gibt die Methode 200 mit einem JSON-Antworttext zurück.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-335">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="bbbb0-336">Die Rückgabe von `item` löst eine HTTP 200-Antwort aus.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-336">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="the-puttodoitem-method"></a><span data-ttu-id="bbbb0-337">PutTodoItem-Methode</span><span class="sxs-lookup"><span data-stu-id="bbbb0-337">The PutTodoItem method</span></span>

<span data-ttu-id="bbbb0-338">Untersuchen Sie die `PutTodoItem`-Methode.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-338">Examine the `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Update)]

<span data-ttu-id="bbbb0-339">`PutTodoItem` ähnelt `PostTodoItem`, verwendet allerdings HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-339">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="bbbb0-340">Die Antwort ist [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="bbbb0-340">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="bbbb0-341">Gemäß der HTTP-Spezifikation erfordert eine PUT-Anforderung, dass der Client die gesamte aktualisierte Entität (nicht nur die Änderungen) sendet.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-341">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="bbbb0-342">Verwenden Sie [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute), um Teilupdates zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-342">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="bbbb0-343">Wenn beim Aufrufen von `PutTodoItem` ein Fehler zurückgegeben wird, rufen Sie `GET` auf, um sicherzustellen, dass die Datenbank ein Element enthält.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-343">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="bbbb0-344">Testen der PutTodoItem-Methode</span><span class="sxs-lookup"><span data-stu-id="bbbb0-344">Test the PutTodoItem method</span></span>

<span data-ttu-id="bbbb0-345">In diesem Beispiel wird eine In-Memory-Datenbank verwendet, die jedes Mal initialisiert werden muss, wenn die App gestartet wird.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-345">This sample uses an in-memory database that must be initialized each time the app is started.</span></span> <span data-ttu-id="bbbb0-346">Es muss ein Element in der Datenbank vorhanden sein, bevor Sie einen PUT-Aufruf durchführen.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-346">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="bbbb0-347">Rufen Sie vor einem PUT-Aufruf GET auf, um sicherzustellen, dass ein Element in der Datenbank vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-347">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="bbbb0-348">Aktualisieren Sie das To-do-Element, das über den ID-Wert 1 verfügt, und legen Sie als Namen „feed fish“ fest:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-348">Update the to-do item that has ID = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="bbbb0-349">Die folgende Abbildung zeigt das Postman-Update:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-349">The following image shows the Postman update:</span></span>

![Postman-Konsole mit der Antwort „204 (Kein Inhalt)“](first-web-api/_static/3/pmcput.png)

## <a name="the-deletetodoitem-method"></a><span data-ttu-id="bbbb0-351">DeleteTodoItem-Methode</span><span class="sxs-lookup"><span data-stu-id="bbbb0-351">The DeleteTodoItem method</span></span>

<span data-ttu-id="bbbb0-352">Untersuchen Sie die `DeleteTodoItem`-Methode.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-352">Examine the `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Delete)]

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="bbbb0-353">Testen der DeleteTodoItem-Methode</span><span class="sxs-lookup"><span data-stu-id="bbbb0-353">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="bbbb0-354">So löschen Sie mit Postman eine Aufgabe</span><span class="sxs-lookup"><span data-stu-id="bbbb0-354">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="bbbb0-355">Legen Sie die Methode auf `DELETE` fest.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-355">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="bbbb0-356">Legen Sie den URI des zu löschenden Objekts fest, z. B. `https://localhost:5001/api/TodoItems/1`.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-356">Set the URI of the object to delete (for example `https://localhost:5001/api/TodoItems/1`).</span></span>
* <span data-ttu-id="bbbb0-357">Wählen Sie **Send** (Senden) aus.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-357">Select **Send**.</span></span>

<a name="over-post"></a>

## <a name="prevent-over-posting"></a><span data-ttu-id="bbbb0-358">Vermeiden von Overposting</span><span class="sxs-lookup"><span data-stu-id="bbbb0-358">Prevent over-posting</span></span>

<span data-ttu-id="bbbb0-359">Derzeit macht die Beispiel-App das gesamte `TodoItem`-Objekt verfügbar.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-359">Currently the sample app exposes the entire `TodoItem` object.</span></span> <span data-ttu-id="bbbb0-360">In den Produktions-Apps sind die Daten, die eingegeben und mithilfe einer Teilmenge des Modells zurückgegeben werden, in der Regel eingeschränkt.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-360">Production apps typically limit the data that's input and returned using a subset of the model.</span></span> <span data-ttu-id="bbbb0-361">Hierfür gibt es mehrere Gründe, wobei die Sicherheit einer der Hauptgründe ist.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-361">There are multiple reasons behind this and security is a major one.</span></span> <span data-ttu-id="bbbb0-362">Die Teilmenge eines Modells wird üblicherweise als Datenübertragungsobjekt (DTO, Data Transfer Object), Eingabemodell oder Anzeigemodell bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-362">The subset of a model is usually referred to as a Data Transfer Object (DTO), input model, or view model.</span></span> <span data-ttu-id="bbbb0-363">In diesem Artikel wird das **DTO** verwendet.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-363">**DTO** is used in this article.</span></span>

<span data-ttu-id="bbbb0-364">Ein DTO kann für Folgendes verwendet werden:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-364">A DTO may be used to:</span></span>

* <span data-ttu-id="bbbb0-365">Vermeiden Sie Overposting.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-365">Prevent over-posting.</span></span>
* <span data-ttu-id="bbbb0-366">Ausblenden von Eigenschaften, die Clients nicht anzeigen sollen</span><span class="sxs-lookup"><span data-stu-id="bbbb0-366">Hide properties that clients are not supposed to view.</span></span>
* <span data-ttu-id="bbbb0-367">Auslassen einiger Eigenschaften, um die Nutzlast zu verringern</span><span class="sxs-lookup"><span data-stu-id="bbbb0-367">Omit some properties in order to reduce payload size.</span></span>
* <span data-ttu-id="bbbb0-368">Vereinfachen von Objektgraphen, die geschachtelte Objekte enthalten</span><span class="sxs-lookup"><span data-stu-id="bbbb0-368">Flatten object graphs that contain nested objects.</span></span> <span data-ttu-id="bbbb0-369">Vereinfachte Objektgraphen können für Clients zweckmäßiger sein.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-369">Flattened object graphs can be more convenient for clients.</span></span>

<span data-ttu-id="bbbb0-370">Um den DTO-Ansatz zu veranschaulichen, aktualisieren Sie die `TodoItem`-Klasse, sodass sie ein geheimes Feld einschließt:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-370">To demonstrate the DTO approach, update the `TodoItem` class to include a secret field:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApiDTO/Models/TodoItem.cs?name=snippet&highlight=6)]

<span data-ttu-id="bbbb0-371">Das geheime Feld muss in dieser App ausgeblendet werden, eine administrative App kann es jedoch verfügbar machen.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-371">The secret field needs to be hidden from this app, but an administrative app could choose to expose it.</span></span>

<span data-ttu-id="bbbb0-372">Vergewissern Sie sich, dass Sie das geheime Feld veröffentlichen und abrufen können.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-372">Verify you can post and get the secret field.</span></span>

<span data-ttu-id="bbbb0-373">Erstellen Sie ein DTO-Modell:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-373">Create a DTO model:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApiDTO/Models/TodoItemDTO.cs?name=snippet)]

<span data-ttu-id="bbbb0-374">Aktualisieren Sie `TodoItemsController`, sodass `TodoItemDTO` verwendet wird:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-374">Update the `TodoItemsController` to use `TodoItemDTO`:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApiDTO/Controllers/TodoItemsController.cs?name=snippet)]

<span data-ttu-id="bbbb0-375">Vergewissern Sie sich, dass Sie das geheime Feld weder veröffentlichen noch abrufen können.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-375">Verify you can't post or get the secret field.</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="bbbb0-376">Aufrufen der Web-API mit JavaScript</span><span class="sxs-lookup"><span data-stu-id="bbbb0-376">Call the web API with JavaScript</span></span>

<span data-ttu-id="bbbb0-377">Mehr dazu finden Sie im [-Tutorial: Aufrufen einer ASP.NET Core-Web-API mit JavaScript](xref:tutorials/web-api-javascript).</span><span class="sxs-lookup"><span data-stu-id="bbbb0-377">See [Tutorial: Call an ASP.NET Core web API with JavaScript](xref:tutorials/web-api-javascript).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="bbbb0-378">In diesem Tutorial lernen Sie, wie die folgenden Aufgaben ausgeführt werden:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-378">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="bbbb0-379">Erstellen eines Web-API-Projekts</span><span class="sxs-lookup"><span data-stu-id="bbbb0-379">Create a web API project.</span></span>
> * <span data-ttu-id="bbbb0-380">Hinzufügen einer Modellklasse und eines Datenbankkontexts</span><span class="sxs-lookup"><span data-stu-id="bbbb0-380">Add a model class and a database context.</span></span>
> * <span data-ttu-id="bbbb0-381">Hinzufügen eines Controllers</span><span class="sxs-lookup"><span data-stu-id="bbbb0-381">Add a controller.</span></span>
> * <span data-ttu-id="bbbb0-382">Hinzufügen von CRUD-Methoden</span><span class="sxs-lookup"><span data-stu-id="bbbb0-382">Add CRUD methods.</span></span>
> * <span data-ttu-id="bbbb0-383">Konfigurieren von Routing und URL-Pfaden</span><span class="sxs-lookup"><span data-stu-id="bbbb0-383">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="bbbb0-384">Angeben von Rückgabewerten</span><span class="sxs-lookup"><span data-stu-id="bbbb0-384">Specify return values.</span></span>
> * <span data-ttu-id="bbbb0-385">Aufrufen der Web-API mit Postman</span><span class="sxs-lookup"><span data-stu-id="bbbb0-385">Call the web API with Postman.</span></span>
> * <span data-ttu-id="bbbb0-386">Aufrufen der Web-API mit JavaScript</span><span class="sxs-lookup"><span data-stu-id="bbbb0-386">Call the web API with JavaScript.</span></span>

<span data-ttu-id="bbbb0-387">Am Ende haben Sie eine Web-API, die in einer relationalen Datenbank gespeicherte „Aufgaben“ verwalten kann.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-387">At the end, you have a web API that can manage "to-do" items stored in a relational database.</span></span>

## <a name="overview"></a><span data-ttu-id="bbbb0-388">Übersicht</span><span class="sxs-lookup"><span data-stu-id="bbbb0-388">Overview</span></span>

<span data-ttu-id="bbbb0-389">In diesem Tutorial wird die folgende API erstellt:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-389">This tutorial creates the following API:</span></span>

|<span data-ttu-id="bbbb0-390">API</span><span class="sxs-lookup"><span data-stu-id="bbbb0-390">API</span></span> | <span data-ttu-id="bbbb0-391">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="bbbb0-391">Description</span></span> | <span data-ttu-id="bbbb0-392">Anforderungstext</span><span class="sxs-lookup"><span data-stu-id="bbbb0-392">Request body</span></span> | <span data-ttu-id="bbbb0-393">Antworttext</span><span class="sxs-lookup"><span data-stu-id="bbbb0-393">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="bbbb0-394">GET /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="bbbb0-394">GET /api/TodoItems</span></span> | <span data-ttu-id="bbbb0-395">Alle To-do-Elemente abrufen</span><span class="sxs-lookup"><span data-stu-id="bbbb0-395">Get all to-do items</span></span> | <span data-ttu-id="bbbb0-396">Keine</span><span class="sxs-lookup"><span data-stu-id="bbbb0-396">None</span></span> | <span data-ttu-id="bbbb0-397">Array von To-do-Elementen</span><span class="sxs-lookup"><span data-stu-id="bbbb0-397">Array of to-do items</span></span>|
|<span data-ttu-id="bbbb0-398">GET /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="bbbb0-398">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="bbbb0-399">Ein Element nach ID abrufen</span><span class="sxs-lookup"><span data-stu-id="bbbb0-399">Get an item by ID</span></span> | <span data-ttu-id="bbbb0-400">Keine</span><span class="sxs-lookup"><span data-stu-id="bbbb0-400">None</span></span> | <span data-ttu-id="bbbb0-401">To-do-Element</span><span class="sxs-lookup"><span data-stu-id="bbbb0-401">To-do item</span></span>|
|<span data-ttu-id="bbbb0-402">POST /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="bbbb0-402">POST /api/TodoItems</span></span> | <span data-ttu-id="bbbb0-403">Neues Element hinzufügen</span><span class="sxs-lookup"><span data-stu-id="bbbb0-403">Add a new item</span></span> | <span data-ttu-id="bbbb0-404">To-do-Element</span><span class="sxs-lookup"><span data-stu-id="bbbb0-404">To-do item</span></span> | <span data-ttu-id="bbbb0-405">To-do-Element</span><span class="sxs-lookup"><span data-stu-id="bbbb0-405">To-do item</span></span> |
|<span data-ttu-id="bbbb0-406">PUT /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="bbbb0-406">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="bbbb0-407">Vorhandenes Element aktualisieren &nbsp;</span><span class="sxs-lookup"><span data-stu-id="bbbb0-407">Update an existing item &nbsp;</span></span> | <span data-ttu-id="bbbb0-408">To-do-Element</span><span class="sxs-lookup"><span data-stu-id="bbbb0-408">To-do item</span></span> | <span data-ttu-id="bbbb0-409">Keine</span><span class="sxs-lookup"><span data-stu-id="bbbb0-409">None</span></span> |
|<span data-ttu-id="bbbb0-410">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="bbbb0-410">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="bbbb0-411">Löschen eines Elements &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="bbbb0-411">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="bbbb0-412">Keine</span><span class="sxs-lookup"><span data-stu-id="bbbb0-412">None</span></span> | <span data-ttu-id="bbbb0-413">Keine</span><span class="sxs-lookup"><span data-stu-id="bbbb0-413">None</span></span>|

<span data-ttu-id="bbbb0-414">Das folgende Diagramm zeigt den Entwurf der App.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-414">The following diagram shows the design of the app.</span></span>

![Der Client ist das Feld ganz links.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="bbbb0-420">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="bbbb0-420">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="bbbb0-421">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bbbb0-421">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="bbbb0-422">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bbbb0-422">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="bbbb0-423">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="bbbb0-423">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="bbbb0-424">Erstellen eines Webprojekts</span><span class="sxs-lookup"><span data-stu-id="bbbb0-424">Create a web project</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="bbbb0-425">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bbbb0-425">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="bbbb0-426">Klicken Sie im Menü **Datei** auf **Neu** > **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-426">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="bbbb0-427">Wählen Sie die Vorlage **ASP.NET Core-Webanwendung** aus, und klicken Sie auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-427">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="bbbb0-428">Geben Sie dem Projekt den Namen *TodoApi*, und klicken Sie auf **Erstellen**.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-428">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="bbbb0-429">Vergewissern Sie sich, dass im Dialogfeld **Neue ASP.NET Core-Webanwendung erstellen** die Optionen **.NET Core** und **ASP.NET Core 2.2** ausgewählt sind.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-429">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 2.2** are selected.</span></span> <span data-ttu-id="bbbb0-430">Wählen Sie die Vorlage **API** aus, und klicken Sie auf **Erstellen**.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-430">Select the **API** template and click **Create**.</span></span> <span data-ttu-id="bbbb0-431">Wählen Sie **nicht** **Enable Docker Support** (Docker-Unterstützung aktivieren) aus.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-431">**Don't** select **Enable Docker Support**.</span></span>

![VS-Dialogfeld „Neues Projekt“](first-web-api/_static/vs.png)

# <a name="visual-studio-code"></a>[<span data-ttu-id="bbbb0-433">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bbbb0-433">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="bbbb0-434">Öffnen Sie das [integrierte Terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="bbbb0-434">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="bbbb0-435">Wechseln Sie mit dem Befehl `cd`' zu dem Ordner, der den Projektordner enthalten soll.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-435">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="bbbb0-436">Führen Sie die folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-436">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapi -o TodoApi
   code -r TodoApi
   ```

  <span data-ttu-id="bbbb0-437">Diese Befehle erstellen ein neues Web-API-Projekt und öffnen eine neue Visual Studio Code-Instanz im neuen Projektordner.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-437">These commands create a new web API project and open a new instance of Visual Studio Code in the new project folder.</span></span>

* <span data-ttu-id="bbbb0-438">Wenn Sie in einem Dialogfeld angeben müssen, ob Sie dem Projekt die erforderlichen Elemente hinzufügen möchten, wählen Sie **Ja** aus.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-438">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="bbbb0-439">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="bbbb0-439">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="bbbb0-440">Klicken Sie auf **Datei** > **Neue Projektmappe**.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-440">Select **File** > **New Solution**.</span></span>

  ![Neue Projektmappe in macOS](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="bbbb0-442">Navigieren Sie in Visual Studio für Mac-Versionen vor Version 8.6 zu **.NET Core** > **App** > **API** > **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-442">In Visual Studio for Mac earlier than version 8.6, select **.NET Core** > **App** > **API** > **Next**.</span></span> <span data-ttu-id="bbbb0-443">Bei Version 8.6 oder höher klicken Sie auf **Web and Console** > **App** > **API** > **Weiter** (Web und Konsole).</span><span class="sxs-lookup"><span data-stu-id="bbbb0-443">In version 8.6 or later, select **Web and Console** > **App** > **API** > **Next**.</span></span>
  
* <span data-ttu-id="bbbb0-444">Übernehmen Sie im Dialogfeld **Neue ASP.NET Core-Web-API konfigurieren** die Standardeinstellung **Zielframework** von \* *.NET Core 2.2*.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-444">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of \**.NET Core 2.2*.</span></span>

* <span data-ttu-id="bbbb0-445">Geben Sie für **Projektname** *TodoApi* ein, und wählen Sie dann **Erstellen** aus.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-445">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![Dialogfeld „Konfiguration“](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a><span data-ttu-id="bbbb0-447">Testen der API</span><span class="sxs-lookup"><span data-stu-id="bbbb0-447">Test the API</span></span>

<span data-ttu-id="bbbb0-448">Die Projektvorlage erstellt eine `values`-API.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-448">The project template creates a `values` API.</span></span> <span data-ttu-id="bbbb0-449">Rufen Sie in einem Browser die `Get`-Methode zum Testen der App auf.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-449">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="bbbb0-450">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bbbb0-450">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="bbbb0-451">Drücken Sie STRG+F5, um die App auszuführen.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-451">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="bbbb0-452">Visual Studio startet einen Browser und navigiert zu `https://localhost:<port>/api/values`, wobei es sich bei `<port>` um eine zufällig ausgewählte Portnummer handelt.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-452">Visual Studio launches a browser and navigates to `https://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="bbbb0-453">Wenn Sie in einem Dialogfeld angeben müssen, ob Sie dem IIS Express-Zertifikat vertrauen, wählen Sie **Ja** aus.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-453">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="bbbb0-454">Wählen Sie im folgenden Dialogfeld **Sicherheitswarnung** die Option **Ja** aus.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-454">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="bbbb0-455">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bbbb0-455">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="bbbb0-456">Drücken Sie STRG+F5, um die App auszuführen.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-456">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="bbbb0-457">Navigieren Sie in einem Browser zur folgenden URL: `https://localhost:5001/api/values`.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-457">In a browser, go to following URL: `https://localhost:5001/api/values`.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="bbbb0-458">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="bbbb0-458">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="bbbb0-459">Wählen Sie **Ausführen** > **Debugging starten** aus, um die App zu starten.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-459">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="bbbb0-460">Visual Studio für Mac startet einen Browser und navigiert zu `https://localhost:<port>`, wobei es sich bei `<port>` um eine zufällig ausgewählte Portnummer handelt.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-460">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="bbbb0-461">Der Fehler „HTTP 404: Nicht gefunden“ wird zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-461">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="bbbb0-462">Fügen Sie der URL `/api/values` an, d.h. ändern Sie die URL in `https://localhost:<port>/api/values`.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-462">Append `/api/values` to the URL (change the URL to `https://localhost:<port>/api/values`).</span></span>

---

<span data-ttu-id="bbbb0-463">Die folgende JSON-Datei wird zurückgegeben:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-463">The following JSON is returned:</span></span>

```json
["value1","value2"]
```

## <a name="add-a-model-class"></a><span data-ttu-id="bbbb0-464">Hinzufügen einer Modellklasse</span><span class="sxs-lookup"><span data-stu-id="bbbb0-464">Add a model class</span></span>

<span data-ttu-id="bbbb0-465">Ein *Modell* ist eine Gruppe von Klassen, die die Daten darstellen, die die App verwaltet.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-465">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="bbbb0-466">Das Modell für diese App ist eine einzelne `TodoItem`-Klasse.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-466">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="bbbb0-467">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bbbb0-467">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="bbbb0-468">Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-468">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="bbbb0-469">Klicken Sie auf **Hinzufügen** > **Neuer Ordner**.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-469">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="bbbb0-470">Geben Sie dem Ordner den Namen *Modelle*.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-470">Name the folder *Models*.</span></span>

* <span data-ttu-id="bbbb0-471">Klicken Sie mit der rechten Maustaste auf den Ordner *Modelle*, und wählen Sie **Hinzufügen** > **Klasse** aus.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-471">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="bbbb0-472">Nennen Sie die Klasse *TodoItem*, und wählen Sie **Hinzufügen** aus.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-472">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="bbbb0-473">Ersetzen Sie den Vorlagencode durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-473">Replace the template code with the following code:</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="bbbb0-474">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bbbb0-474">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="bbbb0-475">Fügen Sie einen Ordner namens *Modelle* hinzu.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-475">Add a folder named *Models*.</span></span>

* <span data-ttu-id="bbbb0-476">Fügen Sie dem Ordner *Modelle* mit dem folgenden Code eine `TodoItem`-Klasse hinzu:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-476">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="bbbb0-477">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="bbbb0-477">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="bbbb0-478">Klicken Sie mit der rechten Maustaste auf das Projekt.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-478">Right-click the project.</span></span> <span data-ttu-id="bbbb0-479">Klicken Sie auf **Hinzufügen** > **Neuer Ordner**.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-479">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="bbbb0-480">Geben Sie dem Ordner den Namen *Modelle*.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-480">Name the folder *Models*.</span></span>

  ![Neuer Ordner](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="bbbb0-482">Klicken Sie mit der rechten Maustaste auf den Ordner *Modelle*, und wählen Sie **Hinzufügen** > **Neue Datei** > **Allgemein** > **Leere Klasse** aus.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-482">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="bbbb0-483">Nennen Sie die Klasse *TodoItem*, und klicken Sie dann auf **Neu**.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-483">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="bbbb0-484">Ersetzen Sie den Vorlagencode durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-484">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="bbbb0-485">Die `Id`-Eigenschaft fungiert als eindeutiger Schlüssel in einer relationalen Datenbank.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-485">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="bbbb0-486">Modellklassen können überall im Projekt platziert werden, doch gemäß der Konvention wird der Ordner *Modelle* verwendet.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-486">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="bbbb0-487">Hinzufügen eines Datenbankkontexts</span><span class="sxs-lookup"><span data-stu-id="bbbb0-487">Add a database context</span></span>

<span data-ttu-id="bbbb0-488">Der *Datenbankkontext* ist die Hauptklasse, die die Entity Framework-Funktionen für ein Datenmodell koordiniert.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-488">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="bbbb0-489">Diese Klasse wird durch Ableiten von der `Microsoft.EntityFrameworkCore.DbContext`-Klasse erstellt.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-489">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="bbbb0-490">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bbbb0-490">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="bbbb0-491">Klicken Sie mit der rechten Maustaste auf den Ordner *Modelle*, und wählen Sie **Hinzufügen** > **Klasse** aus.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-491">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="bbbb0-492">Nennen Sie die Klasse *TodoContext*, und klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-492">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="bbbb0-493">Visual Studio Code / Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="bbbb0-493">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="bbbb0-494">Fügen Sie eine `TodoContext`-Klasse zum Ordner *Modelle* hinzu.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-494">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="bbbb0-495">Ersetzen Sie den Vorlagencode durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-495">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="bbbb0-496">Registrieren des Datenbankkontexts</span><span class="sxs-lookup"><span data-stu-id="bbbb0-496">Register the database context</span></span>

<span data-ttu-id="bbbb0-497">In ASP.NET Core müssen Dienste wie der Datenbankkontext mit dem Container [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection) registriert werden.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-497">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="bbbb0-498">Der Container stellt den Dienst für Controller bereit.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-498">The container provides the service to controllers.</span></span>

<span data-ttu-id="bbbb0-499">Aktualisieren Sie *Startup.cs* mit dem folgenden hervorgehobenen Code:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-499">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup1.cs?highlight=5,8,25-26&name=snippet_all)]

<span data-ttu-id="bbbb0-500">Der vorangehende Code:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-500">The preceding code:</span></span>

* <span data-ttu-id="bbbb0-501">Entfernt nicht benötigte `using`-Deklarationen</span><span class="sxs-lookup"><span data-stu-id="bbbb0-501">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="bbbb0-502">Fügt dem Abhängigkeitscontainer den Datenbankkontext hinzu</span><span class="sxs-lookup"><span data-stu-id="bbbb0-502">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="bbbb0-503">Gibt an, dass der Datenbankkontext eine In-Memory Database verwendet</span><span class="sxs-lookup"><span data-stu-id="bbbb0-503">Specifies that the database context will use an in-memory database.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="bbbb0-504">Hinzufügen eines Controllers</span><span class="sxs-lookup"><span data-stu-id="bbbb0-504">Add a controller</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="bbbb0-505">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bbbb0-505">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="bbbb0-506">Klicken Sie mit der rechten Maustaste auf den Ordner *Controller*.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-506">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="bbbb0-507">Wählen Sie **Hinzufügen** > **Neues Element** aus.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-507">Select **Add** > **New Item**.</span></span>
* <span data-ttu-id="bbbb0-508">Wählen Sie im Dialogfeld **Neues Element hinzufügen** die Vorlage **API-Controllerklasse** aus.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-508">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span>
* <span data-ttu-id="bbbb0-509">Benennen Sie die Klasse *TodoController*, und wählen Sie **Hinzufügen** aus.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-509">Name the class *TodoController*, and select **Add**.</span></span>

  ![Dialogfeld „Neues Element hinzufügen“ mit Controller im Suchfeld und ausgewähltem Web-API-Controller](first-web-api/_static/new_controller.png)

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="bbbb0-511">Visual Studio Code / Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="bbbb0-511">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="bbbb0-512">Erstellen Sie im Ordner *Controllers* eine Klasse namens `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-512">In the *Controllers* folder, create a class named `TodoController`.</span></span>

---

* <span data-ttu-id="bbbb0-513">Ersetzen Sie den Vorlagencode durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-513">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="bbbb0-514">Der vorangehende Code:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-514">The preceding code:</span></span>

* <span data-ttu-id="bbbb0-515">Definiert eine API-Controllerklasse ohne Methoden.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-515">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="bbbb0-516">Markiert die Klasse mit dem [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute)-Attribut.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-516">Marks the class with the [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="bbbb0-517">Dieses Attribut gibt an, dass der Controller auf Web-API-Anforderungen reagiert.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-517">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="bbbb0-518">Weitere Informationen zu bestimmten Verhaltensweisen, die das Attribut ermöglicht, finden Sie unter <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-518">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="bbbb0-519">Verwendet die Abhängigkeitsinjektion, um den Datenbankkontext (`TodoContext`) dem Controller hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-519">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="bbbb0-520">Der Datenbankkontext wird in den einzelnen [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete)-Methoden im Controller verwendet.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-520">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="bbbb0-521">Fügt der Datenbank, falls sie leer ist, ein Element mit dem Namen `Item1` hinzu.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-521">Adds an item named `Item1` to the database if the database is empty.</span></span> <span data-ttu-id="bbbb0-522">Dieser Code befindet sich im Konstruktor. Er wird bei jeder neuen HTTP-Anforderung ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-522">This code is in the constructor, so it runs every time there's a new HTTP request.</span></span> <span data-ttu-id="bbbb0-523">Wenn Sie alle Elemente löschen, erstellt der Konstruktor beim nächsten Aufruf einer API-Methode erneut `Item1`.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-523">If you delete all items, the constructor creates `Item1` again the next time an API method is called.</span></span> <span data-ttu-id="bbbb0-524">So sieht es möglicherweise so aus, als sei die Löschung fehlgeschlagen, obwohl sie erfolgreich war.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-524">So it may look like the deletion didn't work when it actually did work.</span></span>

## <a name="add-get-methods"></a><span data-ttu-id="bbbb0-525">Hinzufügen von GET-Methoden</span><span class="sxs-lookup"><span data-stu-id="bbbb0-525">Add Get methods</span></span>

<span data-ttu-id="bbbb0-526">Fügen Sie der Klasse `TodoController` die folgenden Methoden hinzu, um eine API bereitzustellen, die Aufgaben abruft:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-526">To provide an API that retrieves to-do items, add the following methods to the `TodoController` class:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

<span data-ttu-id="bbbb0-527">Diese Methoden implementieren zwei GET-Endpunkte:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-527">These methods implement two GET endpoints:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="bbbb0-528">Halten Sie die App an, falls diese noch ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-528">Stop the app if it's still running.</span></span> <span data-ttu-id="bbbb0-529">Führen Sie diese dann noch mal aus, damit die letzten Änderungen übernommen werden.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-529">Then run it again to include the latest changes.</span></span>

<span data-ttu-id="bbbb0-530">Testen Sie die App, indem Sie die beiden Endpunkte in einem Browser aufrufen.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-530">Test the app by calling the two endpoints from a browser.</span></span> <span data-ttu-id="bbbb0-531">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-531">For example:</span></span>

* `https://localhost:<port>/api/todo`
* `https://localhost:<port>/api/todo/1`

<span data-ttu-id="bbbb0-532">Die folgende HTTP-Antwort wird durch den Aufruf von `GetTodoItems` erzeugt:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-532">The following HTTP response is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

## <a name="routing-and-url-paths"></a><span data-ttu-id="bbbb0-533">Routing und URL-Pfade</span><span class="sxs-lookup"><span data-stu-id="bbbb0-533">Routing and URL paths</span></span>

<span data-ttu-id="bbbb0-534">Das Attribut [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) gibt eine Methode an, die auf eine HTTP GET-Anforderung antwortet.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-534">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="bbbb0-535">Der URL-Pfad für jede Methode wird wie folgt erstellt:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-535">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="bbbb0-536">Beginnen Sie mit der Vorlagenzeichenfolge im `Route`-Attribut des Controllers:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-536">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* <span data-ttu-id="bbbb0-537">Ersetzen Sie `[controller]` durch den Namen des Controllers, bei dem es sich standardmäßig um den Namen der Controller-Klasse ohne das Suffix „Controller“ handelt.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-537">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="bbbb0-538">Bei diesem Beispiel ist der Klassenname des Controllers „**Todo**Controller“, d.h. der Controllername lautet „todo“.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-538">For this sample, the controller class name is **Todo**Controller, so the controller name is "todo".</span></span> <span data-ttu-id="bbbb0-539">Beim ASP.NET Core-[Routing](xref:mvc/controllers/routing) wird die Groß-/Kleinschreibung nicht beachtet.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-539">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="bbbb0-540">Wenn das `[HttpGet]`-Attribut eine Routenvorlage (z. B. `[HttpGet("products")]`) hat, fügen Sie diese an den Pfad an.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-540">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="bbbb0-541">In diesem Beispiel wird keine Vorlage verwendet.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-541">This sample doesn't use a template.</span></span> <span data-ttu-id="bbbb0-542">Weitere Informationen finden Sie unter [Attributrouting mit Http[Verb]-Attributen](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="bbbb0-542">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="bbbb0-543">In der folgenden `GetTodoItem`-Methode ist `"{id}"` eine Platzhaltervariable für den eindeutigen Bezeichner des To-do-Elements.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-543">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="bbbb0-544">Wenn `GetTodoItem` aufgerufen wird, wird der Wert von `"{id}"` in der URL der Methode in ihrem Parameter `id` bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-544">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="bbbb0-545">Rückgabewerte</span><span class="sxs-lookup"><span data-stu-id="bbbb0-545">Return values</span></span>

<span data-ttu-id="bbbb0-546">Der Rückgabetyp der Methoden `GetTodoItems` und `GetTodoItem` ist der [ActionResult\<T>-Typ](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="bbbb0-546">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="bbbb0-547">ASP.NET Core serialisiert automatisch das Objekt in [JSON](https://www.json.org/) und schreibt den JSON-Code in den Text der Antwortnachricht.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-547">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="bbbb0-548">Der Antwortcode für diesen Rückgabetyp ist 200, vorausgesetzt, es gibt keine Ausnahmefehler.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-548">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="bbbb0-549">Nicht behandelte Ausnahmen werden in 5xx-Fehler übersetzt.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-549">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="bbbb0-550">`ActionResult`-Rückgabetypen können eine Vielzahl von HTTP-Statuscodes darstellen.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-550">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="bbbb0-551">Beispielsweise kann `GetTodoItem` zwei verschiedene Statuswerte zurückgeben:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-551">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="bbbb0-552">Wenn kein Element mit der angeforderten ID übereinstimmt, gibt die Methode einen 404-Fehlercode [Nicht gefunden](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) zurück.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-552">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="bbbb0-553">Andernfalls gibt die Methode 200 mit einem JSON-Antworttext zurück.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-553">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="bbbb0-554">Die Rückgabe von `item` löst eine HTTP 200-Antwort aus.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-554">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="test-the-gettodoitems-method"></a><span data-ttu-id="bbbb0-555">Testen der GetTodoItems-Methode</span><span class="sxs-lookup"><span data-stu-id="bbbb0-555">Test the GetTodoItems method</span></span>

<span data-ttu-id="bbbb0-556">Dieses Tutorial verwendet Postman zum Testen der Web-API.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-556">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="bbbb0-557">Installieren Sie [Postman](https://www.getpostman.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="bbbb0-557">Install [Postman](https://www.getpostman.com/downloads/).</span></span>
* <span data-ttu-id="bbbb0-558">Starten Sie die Web-App.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-558">Start the web app.</span></span>
* <span data-ttu-id="bbbb0-559">Starten Sie Postman.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-559">Start Postman.</span></span>
* <span data-ttu-id="bbbb0-560">Deaktivieren Sie **SSL certificate verification** (Verifizierung des SSL-Zertifikats).</span><span class="sxs-lookup"><span data-stu-id="bbbb0-560">Disable **SSL certificate verification**.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="bbbb0-561">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bbbb0-561">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="bbbb0-562">Deaktivieren Sie auf der Registerkarte **General** (Allgemein) unter File > Settings (**Datei** > **Einstellungen**) **SSL certificate verification** (Verifizierung des SSL-Zertifikats).</span><span class="sxs-lookup"><span data-stu-id="bbbb0-562">From **File** > **Settings** (**General** tab), disable **SSL certificate verification**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="bbbb0-563">Visual Studio Code / Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="bbbb0-563">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="bbbb0-564">Deaktivieren Sie auf der Registerkarte **General** (Allgemein) unter **Postman** > **Preferences** (Postman > Einstellungen) **SSL certificate verification** (Verifizierung des SSL-Zertifikats).</span><span class="sxs-lookup"><span data-stu-id="bbbb0-564">From **Postman** > **Preferences** (**General** tab), disable **SSL certificate verification**.</span></span> <span data-ttu-id="bbbb0-565">Wählen Sie alternativ den Schraubenschlüssel und **Settings** (Einstellungen) aus, und deaktivieren Sie dann die Überprüfung des SSL-Zertifikats.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-565">Alternatively, select the wrench and select **Settings**, then disable the SSL certificate verification.</span></span>

---
  
> [!WARNING]
> <span data-ttu-id="bbbb0-566">Aktivieren Sie die Verifizierung des SSL-Zertifikats wieder, nachdem Sie den Controller getestet haben.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-566">Re-enable SSL certificate verification after testing the controller.</span></span>

* <span data-ttu-id="bbbb0-567">Erstellen Sie eine neue Anforderung.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-567">Create a new request.</span></span>
  * <span data-ttu-id="bbbb0-568">Legen Sie die HTTP-Methode auf **GET** fest.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-568">Set the HTTP method to **GET**.</span></span>
  * <span data-ttu-id="bbbb0-569">Legen Sie die Anforderungs-URL auf `https://localhost:<port>/api/todo` fest.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-569">Set the request URL to `https://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="bbbb0-570">Beispielsweise `https://localhost:5001/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-570">For example, `https://localhost:5001/api/todo`.</span></span>
* <span data-ttu-id="bbbb0-571">Wählen Sie in Postman **Two pane view** (Ansicht mit zwei Bereichen) aus.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-571">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="bbbb0-572">Wählen Sie **Send** (Senden) aus.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-572">Select **Send**.</span></span>

![Postman mit GET-Anforderung](first-web-api/_static/2pv.png)

## <a name="add-a-create-method"></a><span data-ttu-id="bbbb0-574">Hinzufügen einer Methode</span><span class="sxs-lookup"><span data-stu-id="bbbb0-574">Add a Create method</span></span>

<span data-ttu-id="bbbb0-575">Fügen Sie die folgende `PostTodoItem`-Methode in *Controllers/TodoController.cs* hinzu:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-575">Add the following `PostTodoItem` method inside of *Controllers/TodoController.cs*:</span></span> 

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="bbbb0-576">Der oben stehende Code ist eine HTTP POST-Methode, wie durch das [`[HttpPost]`](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute)-Attribut angegeben.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-576">The preceding code is an HTTP POST method, as indicated by the [`[HttpPost]`](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="bbbb0-577">Die Methode ruft den Wert der Aufgabe aus dem Text der HTTP-Anforderung ab.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-577">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="bbbb0-578">Die `CreatedAtAction`-Methode:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-578">The `CreatedAtAction` method:</span></span>

* <span data-ttu-id="bbbb0-579">Gibt bei einer erfolgreichen Anforderung den Statuscode „HTTP 201“ zurück.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-579">Returns an HTTP 201 status code, if successful.</span></span> <span data-ttu-id="bbbb0-580">HTTP 201 ist die Standardantwort für eine HTTP POST-Methode, die eine neue Ressource auf dem Server erstellt.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-580">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="bbbb0-581">Fügt der Antwort einen `Location`-Header hinzu.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-581">Adds a `Location` header to the response.</span></span> <span data-ttu-id="bbbb0-582">Der `Location`-Header gibt den URI des neu erstellten To-Do-Elements zurück.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-582">The `Location` header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="bbbb0-583">Weitere Informationen finden Sie unter [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="bbbb0-583">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="bbbb0-584">Verweist auf die `GetTodoItem`-Aktion zum Erstellen des URIs des `Location`-Headers.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-584">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="bbbb0-585">Das `nameof`-Schlüsselwort von C# wird verwendet, um eine Hartcodierung des Aktionsnamens im `CreatedAtAction`-Aufruf zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-585">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="test-the-posttodoitem-method"></a><span data-ttu-id="bbbb0-586">Testen der PostTodoItem-Methode</span><span class="sxs-lookup"><span data-stu-id="bbbb0-586">Test the PostTodoItem method</span></span>

* <span data-ttu-id="bbbb0-587">Erstellen Sie das Projekt.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-587">Build the project.</span></span>
* <span data-ttu-id="bbbb0-588">Legen Sie in Postman die HTTP-Methode auf `POST` fest.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-588">In Postman, set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="bbbb0-589">Wählen Sie die Registerkarte **Body** (Text) aus.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-589">Select the **Body** tab.</span></span>
* <span data-ttu-id="bbbb0-590">Wählen Sie das Optionsfeld **raw** (Unformatiert) aus.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-590">Select the **raw** radio button.</span></span>
* <span data-ttu-id="bbbb0-591">Legen Sie den Typ auf **JSON (application/json)** fest.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-591">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="bbbb0-592">Geben Sie für die Aufgabe den Anforderungstext im JSON-Format ein:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-592">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="bbbb0-593">Wählen Sie **Send** (Senden) aus.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-593">Select **Send**.</span></span>

  ![Postman mit erstellter Anforderung](first-web-api/_static/create.png)

  <span data-ttu-id="bbbb0-595">Wenn Sie die Fehlermeldung „405: Methode nicht zulässig“ erhalten, wurde das Projekt wahrscheinlich nicht kompiliert, nachdem die Methode `PostTodoItem` hinzugefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-595">If you get a 405 Method Not Allowed error, it's probably the result of not compiling the project after adding the `PostTodoItem` method.</span></span>

### <a name="test-the-location-header-uri"></a><span data-ttu-id="bbbb0-596">Testen des Adressheader-URIs</span><span class="sxs-lookup"><span data-stu-id="bbbb0-596">Test the location header URI</span></span>

* <span data-ttu-id="bbbb0-597">Wählen Sie auf der Registerkarte **Header** (Header) den Bereich **Response** (Antwort) aus.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-597">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="bbbb0-598">Kopieren Sie den den Headerwert von **Location** (Speicherort):</span><span class="sxs-lookup"><span data-stu-id="bbbb0-598">Copy the **Location** header value:</span></span>

  ![Registerkarte „Header“ in der Postman-Konsole](first-web-api/_static/pmc2.png)

* <span data-ttu-id="bbbb0-600">Legen Sie die Methode auf „GET“ fest.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-600">Set the method to GET.</span></span>
* <span data-ttu-id="bbbb0-601">Fügen Sie den URI (z. B. `https://localhost:5001/api/Todo/2`) ein.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-601">Paste the URI (for example, `https://localhost:5001/api/Todo/2`).</span></span>
* <span data-ttu-id="bbbb0-602">Wählen Sie **Send** (Senden) aus.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-602">Select **Send**.</span></span>

## <a name="add-a-puttodoitem-method"></a><span data-ttu-id="bbbb0-603">Hinzufügen einer PutTodoItem-Methode</span><span class="sxs-lookup"><span data-stu-id="bbbb0-603">Add a PutTodoItem method</span></span>

<span data-ttu-id="bbbb0-604">Fügen Sie die folgende `PutTodoItem`-Methode hinzu:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-604">Add the following `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="bbbb0-605">`PutTodoItem` ähnelt `PostTodoItem`, verwendet allerdings HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-605">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="bbbb0-606">Die Antwort ist [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="bbbb0-606">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="bbbb0-607">Gemäß der HTTP-Spezifikation erfordert eine PUT-Anforderung, dass der Client die gesamte aktualisierte Entität (nicht nur die Änderungen) sendet.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-607">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="bbbb0-608">Verwenden Sie [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute), um Teilupdates zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-608">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="bbbb0-609">Wenn beim Aufrufen von `PutTodoItem` ein Fehler zurückgegeben wird, rufen Sie `GET` auf, um sicherzustellen, dass die Datenbank ein Element enthält.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-609">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="bbbb0-610">Testen der PutTodoItem-Methode</span><span class="sxs-lookup"><span data-stu-id="bbbb0-610">Test the PutTodoItem method</span></span>

<span data-ttu-id="bbbb0-611">In diesem Beispiel wird eine In-Memory-Datenbank verwendet, die jedes Mal initialisiert werden muss, wenn die App gestartet wird.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-611">This sample uses an in-memory database that must be initialized each time the app is started.</span></span> <span data-ttu-id="bbbb0-612">Es muss ein Element in der Datenbank vorhanden sein, bevor Sie einen PUT-Aufruf durchführen.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-612">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="bbbb0-613">Rufen Sie vor einem PUT-Aufruf GET auf, um sicherzustellen, dass ein Element in der Datenbank vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-613">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="bbbb0-614">Aktualisieren Sie die Aufgabe mit der ID = 1, und setzen Sie ihren Namen auf „feed fish“:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-614">Update the to-do item that has id = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="bbbb0-615">Die folgende Abbildung zeigt das Postman-Update:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-615">The following image shows the Postman update:</span></span>

![Postman-Konsole mit der Antwort „204 (Kein Inhalt)“](first-web-api/_static/pmcput.png)

## <a name="add-a-deletetodoitem-method"></a><span data-ttu-id="bbbb0-617">Hinzufügen einer DeleteTodoItem-Methode</span><span class="sxs-lookup"><span data-stu-id="bbbb0-617">Add a DeleteTodoItem method</span></span>

<span data-ttu-id="bbbb0-618">Fügen Sie die folgende `DeleteTodoItem`-Methode hinzu:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-618">Add the following `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="bbbb0-619">Die `DeleteTodoItem`-Antwort lautet [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="bbbb0-619">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="bbbb0-620">Testen der DeleteTodoItem-Methode</span><span class="sxs-lookup"><span data-stu-id="bbbb0-620">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="bbbb0-621">So löschen Sie mit Postman eine Aufgabe</span><span class="sxs-lookup"><span data-stu-id="bbbb0-621">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="bbbb0-622">Legen Sie die Methode auf `DELETE` fest.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-622">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="bbbb0-623">Legen Sie den URI des zu löschenden Objekts fest, z. B. `https://localhost:5001/api/todo/1`.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-623">Set the URI of the object to delete (for example, `https://localhost:5001/api/todo/1`).</span></span>
* <span data-ttu-id="bbbb0-624">Wählen Sie **Send** (Senden) aus.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-624">Select **Send**.</span></span>

<span data-ttu-id="bbbb0-625">Sie können in der Beispiel-App alle Elemente löschen.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-625">The sample app allows you to delete all the items.</span></span> <span data-ttu-id="bbbb0-626">Sobald das letzte Element gelöscht wurde, wird allerdings beim nächsten API-Aufruf vom Modellklassenkonstruktor ein neues erstellt.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-626">However, when the last item is deleted, a new one is created by the model class constructor the next time the API is called.</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="bbbb0-627">Aufrufen der Web-API mit JavaScript</span><span class="sxs-lookup"><span data-stu-id="bbbb0-627">Call the web API with JavaScript</span></span>

<span data-ttu-id="bbbb0-628">In diesem Abschnitt wird eine HTML-Seite hinzugefügt, die mithilfe von JavaScript die Web-API aufruft.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-628">In this section, an HTML page is added that uses JavaScript to call the web API.</span></span> <span data-ttu-id="bbbb0-629">jQuery initiiert die Anforderung.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-629">jQuery initiates the request.</span></span> <span data-ttu-id="bbbb0-630">JavaScript aktualisiert die Seite mit den Details aus der Antwort der Web-API.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-630">JavaScript updates the page with the details from the web API's response.</span></span>

<span data-ttu-id="bbbb0-631">Konfigurieren Sie die App so, dass sie [statische Dateien bereitstellt](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_), und [aktivieren Sie die Standarddateizuordnung](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_), indem Sie *startup.cs* mit dem unten markierten Code aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-631">Configure the app to [serve static files](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [enable default file mapping](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) by updating *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup.cs?highlight=14-15&name=snippet_configure)]

<span data-ttu-id="bbbb0-632">Erstellen Sie im Projektverzeichnis den Ordner *wwwwroot*.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-632">Create a *wwwroot* folder in the project directory.</span></span>

<span data-ttu-id="bbbb0-633">Fügen Sie dem Verzeichnis *wwwroot* eine HTML-Datei namens *index.html* hinzu.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-633">Add an HTML file named *index.html* to the *wwwroot* directory.</span></span> <span data-ttu-id="bbbb0-634">Ersetzen Sie den Inhalt durch folgendes Markup:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-634">Replace its contents with the following markup:</span></span>

[!code-html[](first-web-api/samples/2.2/TodoApi/wwwroot/index.html)]

<span data-ttu-id="bbbb0-635">Fügen Sie dem Verzeichnis *wwwroot* eine JavaScript-Datei namens *site.js* hinzu.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-635">Add a JavaScript file named *site.js* to the *wwwroot* directory.</span></span> <span data-ttu-id="bbbb0-636">Ersetzen Sie den Inhalt durch folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-636">Replace its contents with the following code:</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

<span data-ttu-id="bbbb0-637">Möglicherweise ist eine Änderung an den Starteinstellungen des ASP.NET Core-Projekts erforderlich, um die HTML-Seite lokal zu testen:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-637">A change to the ASP.NET Core project's launch settings may be required to test the HTML page locally:</span></span>

* <span data-ttu-id="bbbb0-638">Öffnen Sie *Properties\launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-638">Open *Properties\launchSettings.json*.</span></span>
* <span data-ttu-id="bbbb0-639">Entfernen Sie die `launchUrl`-Eigenschaft, um zu erzwingen, dass die App mit *index.html* als Startseite geöffnet wird. Dies ist die Standarddatei des Projekts.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-639">Remove the `launchUrl` property to force the app to open at *index.html*&mdash;the project's default file.</span></span>

<span data-ttu-id="bbbb0-640">Dieses Beispiel ruft alle CRUD-Methoden der Web-API auf.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-640">This sample calls all of the CRUD methods of the web API.</span></span> <span data-ttu-id="bbbb0-641">Im Folgenden werden die API-Aufrufe erläutert.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-641">Following are explanations of the calls to the API.</span></span>

### <a name="get-a-list-of-to-do-items"></a><span data-ttu-id="bbbb0-642">Abrufen einer Liste von To-Do-Elementen</span><span class="sxs-lookup"><span data-stu-id="bbbb0-642">Get a list of to-do items</span></span>

<span data-ttu-id="bbbb0-643">jQuery sendet eine HTTP GET-Anforderung an die Web-API, die JSON-Code mit einem Array aus To-Do-Elementen zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-643">jQuery sends an HTTP GET request to the web API, which returns JSON representing an array of to-do items.</span></span> <span data-ttu-id="bbbb0-644">Die Rückruffunktion `success` wird aufgerufen, wenn die Anforderung erfolgreich ist.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-644">The `success` callback function is invoked if the request succeeds.</span></span> <span data-ttu-id="bbbb0-645">Im Rückruf wird DOM mit den To-Do-Informationen aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-645">In the callback, the DOM is updated with the to-do information.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a><span data-ttu-id="bbbb0-646">Hinzufügen eines To-Do-Elements</span><span class="sxs-lookup"><span data-stu-id="bbbb0-646">Add a to-do item</span></span>

<span data-ttu-id="bbbb0-647">jQuery sendet eine HTTP POST-Anforderung mit dem To-Do-Element im Anforderungstext.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-647">jQuery sends an HTTP POST request with the to-do item in the request body.</span></span> <span data-ttu-id="bbbb0-648">Die Optionen `accepts` und `contentType` werden auf `application/json` festgelegt, um den gesendeten und empfangenen Medientyp anzugeben.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-648">The `accepts` and `contentType` options are set to `application/json` to specify the media type being received and sent.</span></span> <span data-ttu-id="bbbb0-649">Die Aufgabe wird mit [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify) in JSON konvertiert.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-649">The to-do item is converted to JSON by using [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span></span> <span data-ttu-id="bbbb0-650">Wenn die API den Statuscode „Erfolgreich“ zurückgibt, wird die Funktion `getData` aufgerufen, um die HTML-Tabelle zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-650">When the API returns a successful status code, the `getData` function is invoked to update the HTML table.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a><span data-ttu-id="bbbb0-651">Aktualisieren eines To-Do-Elements</span><span class="sxs-lookup"><span data-stu-id="bbbb0-651">Update a to-do item</span></span>

<span data-ttu-id="bbbb0-652">Das Aktualisieren einer Aufgabe ist vergleichbar mit dem Hinzufügen einer Aufgabe.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-652">Updating a to-do item is similar to adding one.</span></span> <span data-ttu-id="bbbb0-653">Die `url` ändert sich, um den eindeutigen Bezeichner des Elements hinzuzufügen. Der `type` lautet `PUT`.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-653">The `url` changes to add the unique identifier of the item, and the `type` is `PUT`.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a><span data-ttu-id="bbbb0-654">Löschen eines To-Do-Elements</span><span class="sxs-lookup"><span data-stu-id="bbbb0-654">Delete a to-do item</span></span>

<span data-ttu-id="bbbb0-655">Eine Aufgabe wird folgendermaßen gelöscht: im AJAX-Aufruf wird `type` auf `DELETE` festgelegt, und der eindeutige Bezeichner des Elements wird in der URL angegeben.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-655">Deleting a to-do item is accomplished by setting the `type` on the AJAX call to `DELETE` and specifying the item's unique identifier in the URL.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

::: moniker-end

<a name="auth"></a>

## <a name="add-authentication-support-to-a-web-api"></a><span data-ttu-id="bbbb0-656">Hinzufügen der Authentifizierungsunterstützung zu einer Web-API</span><span class="sxs-lookup"><span data-stu-id="bbbb0-656">Add authentication support to a web API</span></span>

[!INCLUDE[](~/includes/IdentityServer4.md)]

## <a name="additional-resources"></a><span data-ttu-id="bbbb0-657">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="bbbb0-657">Additional resources</span></span>

<span data-ttu-id="bbbb0-658">[Sie können den Beispielcode für dieses Tutorial anzeigen oder herunterladen.](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples)</span><span class="sxs-lookup"><span data-stu-id="bbbb0-658">[View or download sample code for this tutorial](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span></span> <span data-ttu-id="bbbb0-659">[Informationen zum Herunterladen](xref:index#how-to-download-a-sample) finden Sie hier.</span><span class="sxs-lookup"><span data-stu-id="bbbb0-659">See [how to download](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="bbbb0-660">Weitere Informationen finden Sie in den folgenden Ressourcen:</span><span class="sxs-lookup"><span data-stu-id="bbbb0-660">For more information, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/intro>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>
* [<span data-ttu-id="bbbb0-661">Dieses Tutorial auf YouTube</span><span class="sxs-lookup"><span data-stu-id="bbbb0-661">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=TTkhEyGBfAk)
