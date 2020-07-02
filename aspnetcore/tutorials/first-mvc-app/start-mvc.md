---
title: Erste Schritte mit ASP.NET Core MVC
author: rick-anderson
description: Hier finden Sie Informationen zum Einstieg in ASP.NET Core MVC.
ms.author: riande
ms.date: 10/16/2019
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 01321d68defafbe79371250669f921307bcfdba6
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/26/2020
ms.locfileid: "85407043"
---
# <a name="get-started-with-aspnet-core-mvc"></a><span data-ttu-id="7c92f-103">Erste Schritte mit ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="7c92f-103">Get started with ASP.NET Core MVC</span></span>

<span data-ttu-id="7c92f-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7c92f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="7c92f-105">In diesem Tutorial lernen Sie Grundlegendes zur Erstellung einer ASP.NET Core-MVC-Web-App.</span><span class="sxs-lookup"><span data-stu-id="7c92f-105">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="7c92f-106">Die App verwaltet eine Datenbank mit Filmtiteln.</span><span class="sxs-lookup"><span data-stu-id="7c92f-106">The app manages a database of movie titles.</span></span> <span data-ttu-id="7c92f-107">Sie lernen Folgendes:</span><span class="sxs-lookup"><span data-stu-id="7c92f-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="7c92f-108">Erstellen einer Web-App</span><span class="sxs-lookup"><span data-stu-id="7c92f-108">Create a web app.</span></span>
> * <span data-ttu-id="7c92f-109">Hinzufügen eines Modells und Erstellen eines Gerüsts für das Modell</span><span class="sxs-lookup"><span data-stu-id="7c92f-109">Add and scaffold a model.</span></span>
> * <span data-ttu-id="7c92f-110">Arbeiten mit einer Datenbank</span><span class="sxs-lookup"><span data-stu-id="7c92f-110">Work with a database.</span></span>
> * <span data-ttu-id="7c92f-111">Hinzufügen von Such- und Überprüfungsfunktionen</span><span class="sxs-lookup"><span data-stu-id="7c92f-111">Add search and validation.</span></span>

<span data-ttu-id="7c92f-112">Am Ende verfügen Sie über eine App, die Filmdaten verwalten und anzeigen kann.</span><span class="sxs-lookup"><span data-stu-id="7c92f-112">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="7c92f-113">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="7c92f-113">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="7c92f-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7c92f-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="7c92f-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7c92f-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="7c92f-116">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="7c92f-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

---

## <a name="create-a-web-app"></a><span data-ttu-id="7c92f-117">Erstellen einer Web-App</span><span class="sxs-lookup"><span data-stu-id="7c92f-117">Create a web app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="7c92f-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7c92f-118">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="7c92f-119">Wählen Sie in Visual Studio die Option **Neues Projekt erstellen** aus.</span><span class="sxs-lookup"><span data-stu-id="7c92f-119">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="7c92f-120">Klicken Sie auf **ASP.NET Core-Webanwendung** und dann auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="7c92f-120">Select **ASP.NET Core Web Application** and then select **Next**.</span></span>

![neue ASP.NET Core-Webanwendung](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="7c92f-122">Geben Sie dem Projekt den Namen **MvcMovie**, und klicken Sie dann auf **Erstellen**.</span><span class="sxs-lookup"><span data-stu-id="7c92f-122">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="7c92f-123">Es ist wichtig, dem Projekt den Namen **MvcMovie** zu geben, damit beim Kopieren von Code der Namespace übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="7c92f-123">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![neue ASP.NET Core-Webanwendung](start-mvc/_static/config.png)

* <span data-ttu-id="7c92f-125">Klicken Sie auf **Webanwendung (Model View Controller)** und anschließend auf **Erstellen**.</span><span class="sxs-lookup"><span data-stu-id="7c92f-125">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="7c92f-126">Dialogfeld „Neues Projekt“, .NET Core im linken Bereich, ASP.NET Core-Web-App</span><span class="sxs-lookup"><span data-stu-id="7c92f-126">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project30.png)

<span data-ttu-id="7c92f-127">Visual Studio verwendet die Standardvorlage für das MVC-Projekt, das Sie gerade erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="7c92f-127">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="7c92f-128">Wenn Sie einen Projektnamen eingeben und einige Optionen festlegen, funktioniert Ihre App bereits.</span><span class="sxs-lookup"><span data-stu-id="7c92f-128">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="7c92f-129">Hierbei handelt es sich um ein grundlegendes Startprojekt.</span><span class="sxs-lookup"><span data-stu-id="7c92f-129">This is a basic starter project.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="7c92f-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7c92f-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="7c92f-131">Das Tutorial setzt voraus, dass Sie mit VS Code vertraut sind.</span><span class="sxs-lookup"><span data-stu-id="7c92f-131">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="7c92f-132">Weitere Informationen finden Sie unter [Erste Schritte mit VS Code](https://code.visualstudio.com/docs) und [Hilfe zu Visual Studio Code](#visual-studio-code-help).</span><span class="sxs-lookup"><span data-stu-id="7c92f-132">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="7c92f-133">Öffnen Sie das [integrierte Terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="7c92f-133">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="7c92f-134">Wechseln Sie mit `cd` zu einem Ordner, der das Projekt enthalten soll.</span><span class="sxs-lookup"><span data-stu-id="7c92f-134">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="7c92f-135">Führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="7c92f-135">Run the following command:</span></span>

   ```dotnetcli
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="7c92f-136">Es wird ein Dialogfeld mit folgender Meldung angezeigt: **Die erforderlichen Objekte zum Erstellen und Debuggen sind in "MvcMovie" nicht vorhanden. Sollen sie hinzugefügt werden?**</span><span class="sxs-lookup"><span data-stu-id="7c92f-136">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="7c92f-137">Wählen Sie **Ja** aus.</span><span class="sxs-lookup"><span data-stu-id="7c92f-137">Select **Yes**</span></span>

  * <span data-ttu-id="7c92f-138">`dotnet new mvc -o MvcMovie`: Erstellt ein neues ASP.NET Core MVC-Projekt im Ordner *MvcMovie*.</span><span class="sxs-lookup"><span data-stu-id="7c92f-138">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="7c92f-139">`code -r MvcMovie`: Lädt die Projektdatei *MvcMovie.csproj* in Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="7c92f-139">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="7c92f-140">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="7c92f-140">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="7c92f-141">Klicken Sie auf **Datei** > **Neue Projektmappe**.</span><span class="sxs-lookup"><span data-stu-id="7c92f-141">Select **File** > **New Solution**.</span></span>

  ![Neue Projektmappe in macOS](start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="7c92f-143">Navigieren Sie in Visual Studio für Mac-Versionen vor Version 8.6 zu **.NET Core** > **App** > **Webanwendung (Model-View-Controller)**  > **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="7c92f-143">In Visual Studio for Mac earlier than version 8.6, select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span> <span data-ttu-id="7c92f-144">Bei Version 8.6 oder höher klicken Sie auf **Web and Console** > **App** > **Webanwendung (Model-View-Controller)**  > **Weiter** (Web und Konsole).</span><span class="sxs-lookup"><span data-stu-id="7c92f-144">In version 8.6 or later, select **Web and Console** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![Auswählen der macOS-Web-App-Vorlage](start-mvc/_static/web_app_template_vsmac.png)

* <span data-ttu-id="7c92f-146">Bestätigen Sie die folgenden Konfigurationen:</span><span class="sxs-lookup"><span data-stu-id="7c92f-146">Confirm the following configurations:</span></span>

  * <span data-ttu-id="7c92f-147">**Zielframework** auf **.NET Core 3.1** festgelegt</span><span class="sxs-lookup"><span data-stu-id="7c92f-147">**Target Framework** set to **.NET Core 3.1**.</span></span>
  * <span data-ttu-id="7c92f-148">**Authentifizierung** auf **Keine Authentifizierung** festgelegt</span><span class="sxs-lookup"><span data-stu-id="7c92f-148">**Authentication** set to **No Authentication**.</span></span>
   
  <span data-ttu-id="7c92f-149">Klicken Sie auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="7c92f-149">Select **Next**.</span></span>

  ![Auswahl für .NET Core 3.1 in macOS](start-mvc/_static/new_project_31_vsmac.png)

* <span data-ttu-id="7c92f-151">Nennen Sie das Projekt **MvcMovie**, und wählen Sie dann **Erstellen** aus.</span><span class="sxs-lookup"><span data-stu-id="7c92f-151">Name the project **MvcMovie**, and then select **Create**.</span></span>

  ![Benennen des macOS-Projekts](start-mvc/_static/MvcMovie.png)

---

### <a name="run-the-app"></a><span data-ttu-id="7c92f-153">Ausführen der App</span><span class="sxs-lookup"><span data-stu-id="7c92f-153">Run the app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="7c92f-154">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7c92f-154">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="7c92f-155">Drücken Sie **STRG+F5**, um die App im Nicht-Debugmodus auszuführen.</span><span class="sxs-lookup"><span data-stu-id="7c92f-155">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="7c92f-156">Visual Studio startet [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) und führt die App aus.</span><span class="sxs-lookup"><span data-stu-id="7c92f-156">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="7c92f-157">Beachten Sie, dass die Adressleiste `localhost:port#` und nicht etwas wie `example.com` anzeigt.</span><span class="sxs-lookup"><span data-stu-id="7c92f-157">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="7c92f-158">Das liegt daran, dass es sich bei `localhost` um den Standard-Hostnamen für Ihren lokalen Computer handelt.</span><span class="sxs-lookup"><span data-stu-id="7c92f-158">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="7c92f-159">Wenn in Visual Studio ein Webprojekt erstellt wird, wird für den Webserver ein zufälliger Port verwendet.</span><span class="sxs-lookup"><span data-stu-id="7c92f-159">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="7c92f-160">Das Starten der App mit STRG+F5 (Nicht-Debugmodus) ermöglicht die Änderung des Codes, das Speichern der Datei, das Aktualisieren des Browsers und das Anzeigen von Codeänderungen.</span><span class="sxs-lookup"><span data-stu-id="7c92f-160">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="7c92f-161">Viele Entwickler bevorzugen den Nicht-Debugmodus, um die App schnell zu starten und Änderungen anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="7c92f-161">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="7c92f-162">Sie können die App über das Menüelement **Debuggen** im Debugmodus oder Nicht-Debugmodus starten:</span><span class="sxs-lookup"><span data-stu-id="7c92f-162">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![Menü „Debuggen“](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="7c92f-164">Sie können die App debuggen, indem Sie die Schaltfläche **IIS Express** auswählen.</span><span class="sxs-lookup"><span data-stu-id="7c92f-164">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

  <span data-ttu-id="7c92f-166">Auf der folgenden Abbildung sehen Sie die App:</span><span class="sxs-lookup"><span data-stu-id="7c92f-166">The following image shows the app:</span></span>

  ![Start- oder Indexseite](start-mvc/_static/home2.2.png)

# <a name="visual-studio-code"></a>[<span data-ttu-id="7c92f-168">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7c92f-168">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="7c92f-169">Drücken Sie STRG+F5, um die Ausführung ohne den Debugger zu starten.</span><span class="sxs-lookup"><span data-stu-id="7c92f-169">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="7c92f-170">Visual Studio Code startet [Kestrel](xref:fundamentals/servers/kestrel) und einen Browser und navigiert zu `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="7c92f-170">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="7c92f-171">Die Adressleiste zeigt `localhost:port:5001` an, nicht `example.com`.</span><span class="sxs-lookup"><span data-stu-id="7c92f-171">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="7c92f-172">Das liegt daran, dass es sich bei `localhost` um den Standardhostnamen für den lokalen Computer handelt.</span><span class="sxs-lookup"><span data-stu-id="7c92f-172">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="7c92f-173">„Localhost“ dient nur Webanforderungen vom lokalen Computer.</span><span class="sxs-lookup"><span data-stu-id="7c92f-173">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="7c92f-174">Das Starten der App mit STRG+F5 (Nicht-Debugmodus) ermöglicht die Änderung des Codes, das Speichern der Datei, das Aktualisieren des Browsers und das Anzeigen von Codeänderungen.</span><span class="sxs-lookup"><span data-stu-id="7c92f-174">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="7c92f-175">Viele Entwickler bevorzugen den Nicht-Debugmodus, um die Seite zu aktualisieren und Änderungen anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="7c92f-175">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

  ![Start- oder Indexseite](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="7c92f-177">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="7c92f-177">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="7c92f-178">Wählen Sie **Ausführen** > **Ohne Debuggen starten** aus, um die App zu starten.</span><span class="sxs-lookup"><span data-stu-id="7c92f-178">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="7c92f-179">Visual Studio für Mac startet den [Kestrel](xref:fundamentals/servers/index#kestrel)-Server und einen Browser und navigiert zu `http://localhost:port`, wobei es sich bei *port* um eine zufällig ausgewählte Portnummer handelt.</span><span class="sxs-lookup"><span data-stu-id="7c92f-179">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="7c92f-180">Die Adressleiste zeigt `localhost:port#` an, nicht `example.com`.</span><span class="sxs-lookup"><span data-stu-id="7c92f-180">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="7c92f-181">Das liegt daran, dass es sich bei `localhost` um den Standard-Hostnamen für Ihren lokalen Computer handelt.</span><span class="sxs-lookup"><span data-stu-id="7c92f-181">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="7c92f-182">Wenn in Visual Studio ein Webprojekt erstellt wird, wird für den Webserver ein zufälliger Port verwendet.</span><span class="sxs-lookup"><span data-stu-id="7c92f-182">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="7c92f-183">Wenn Sie die App ausführen, wird eine andere Portnummer angezeigt.</span><span class="sxs-lookup"><span data-stu-id="7c92f-183">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="7c92f-184">Sie können die App über das Menü **Ausführen** im Debugmodus oder Nicht-Debugmodus starten.</span><span class="sxs-lookup"><span data-stu-id="7c92f-184">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

  <span data-ttu-id="7c92f-185">Auf der folgenden Abbildung sehen Sie die App:</span><span class="sxs-lookup"><span data-stu-id="7c92f-185">The following image shows the app:</span></span>

  ![Start- oder Indexseite](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="7c92f-187">Im nächsten Teil dieses Tutorials erfahren Sie mehr über MVC und beginnen mit dem Schreiben von Code.</span><span class="sxs-lookup"><span data-stu-id="7c92f-187">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="7c92f-188">Nächste</span><span class="sxs-lookup"><span data-stu-id="7c92f-188">Next</span></span>](adding-controller.md)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="7c92f-189">In diesem Tutorial lernen Sie Grundlegendes zur Erstellung einer ASP.NET Core-MVC-Web-App.</span><span class="sxs-lookup"><span data-stu-id="7c92f-189">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="7c92f-190">Die App verwaltet eine Datenbank mit Filmtiteln.</span><span class="sxs-lookup"><span data-stu-id="7c92f-190">The app manages a database of movie titles.</span></span> <span data-ttu-id="7c92f-191">Sie lernen Folgendes:</span><span class="sxs-lookup"><span data-stu-id="7c92f-191">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="7c92f-192">Erstellen einer Web-App</span><span class="sxs-lookup"><span data-stu-id="7c92f-192">Create a web app.</span></span>
> * <span data-ttu-id="7c92f-193">Hinzufügen eines Modells und Erstellen eines Gerüsts für das Modell</span><span class="sxs-lookup"><span data-stu-id="7c92f-193">Add and scaffold a model.</span></span>
> * <span data-ttu-id="7c92f-194">Arbeiten mit einer Datenbank</span><span class="sxs-lookup"><span data-stu-id="7c92f-194">Work with a database.</span></span>
> * <span data-ttu-id="7c92f-195">Hinzufügen von Such- und Überprüfungsfunktionen</span><span class="sxs-lookup"><span data-stu-id="7c92f-195">Add search and validation.</span></span>

<span data-ttu-id="7c92f-196">Am Ende verfügen Sie über eine App, die Filmdaten verwalten und anzeigen kann.</span><span class="sxs-lookup"><span data-stu-id="7c92f-196">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="7c92f-197">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="7c92f-197">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="7c92f-198">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7c92f-198">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="7c92f-199">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7c92f-199">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="7c92f-200">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="7c92f-200">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---
## <a name="create-a-web-app"></a><span data-ttu-id="7c92f-201">Erstellen einer Web-App</span><span class="sxs-lookup"><span data-stu-id="7c92f-201">Create a web app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="7c92f-202">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7c92f-202">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="7c92f-203">Wählen Sie in Visual Studio die Option **Neues Projekt erstellen** aus.</span><span class="sxs-lookup"><span data-stu-id="7c92f-203">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="7c92f-204">Klicken Sie auf **ASP.NET Core-Webanwendung** und dann auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="7c92f-204">Select **ASP.NET Core Web Application** and then select **Next**.</span></span>

![neue ASP.NET Core-Webanwendung](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="7c92f-206">Geben Sie dem Projekt den Namen **MvcMovie**, und klicken Sie dann auf **Erstellen**.</span><span class="sxs-lookup"><span data-stu-id="7c92f-206">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="7c92f-207">Es ist wichtig, dem Projekt den Namen **MvcMovie** zu geben, damit beim Kopieren von Code der Namespace übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="7c92f-207">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![neue ASP.NET Core-Webanwendung](start-mvc/_static/config.png)


* <span data-ttu-id="7c92f-209">Klicken Sie auf **Webanwendung (Model View Controller)** und anschließend auf **Erstellen**.</span><span class="sxs-lookup"><span data-stu-id="7c92f-209">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="7c92f-210">Dialogfeld „Neues Projekt“, .NET Core im linken Bereich, ASP.NET Core-Web-App</span><span class="sxs-lookup"><span data-stu-id="7c92f-210">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="7c92f-211">Visual Studio verwendet die Standardvorlage für das MVC-Projekt, das Sie gerade erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="7c92f-211">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="7c92f-212">Wenn Sie einen Projektnamen eingeben und einige Optionen festlegen, funktioniert Ihre App bereits.</span><span class="sxs-lookup"><span data-stu-id="7c92f-212">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="7c92f-213">Dies ist ein grundlegendes Startprojekt und ein guter Einstieg.</span><span class="sxs-lookup"><span data-stu-id="7c92f-213">This is a basic starter project, and it's a good place to start.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="7c92f-214">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7c92f-214">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="7c92f-215">Das Tutorial setzt voraus, dass Sie mit VS Code vertraut sind.</span><span class="sxs-lookup"><span data-stu-id="7c92f-215">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="7c92f-216">Weitere Informationen finden Sie unter [Erste Schritte mit VS Code](https://code.visualstudio.com/docs) und [Hilfe zu Visual Studio Code](#visual-studio-code-help).</span><span class="sxs-lookup"><span data-stu-id="7c92f-216">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="7c92f-217">Öffnen Sie das [integrierte Terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="7c92f-217">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="7c92f-218">Wechseln Sie mit `cd` zu einem Ordner, der das Projekt enthalten soll.</span><span class="sxs-lookup"><span data-stu-id="7c92f-218">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="7c92f-219">Führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="7c92f-219">Run the following command:</span></span>

   ```dotnetcli
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="7c92f-220">Es wird ein Dialogfeld mit folgender Meldung angezeigt: **Die erforderlichen Objekte zum Erstellen und Debuggen sind in "MvcMovie" nicht vorhanden. Sollen sie hinzugefügt werden?**</span><span class="sxs-lookup"><span data-stu-id="7c92f-220">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="7c92f-221">Wählen Sie **Ja** aus.</span><span class="sxs-lookup"><span data-stu-id="7c92f-221">Select **Yes**</span></span>

  * <span data-ttu-id="7c92f-222">`dotnet new mvc -o MvcMovie`: Erstellt ein neues ASP.NET Core MVC-Projekt im Ordner *MvcMovie*.</span><span class="sxs-lookup"><span data-stu-id="7c92f-222">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="7c92f-223">`code -r MvcMovie`: Lädt die Projektdatei *MvcMovie.csproj* in Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="7c92f-223">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="7c92f-224">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="7c92f-224">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="7c92f-225">Klicken Sie auf **Datei** > **Neue Projektmappe**.</span><span class="sxs-lookup"><span data-stu-id="7c92f-225">Select **File** > **New Solution**.</span></span>

  ![Neue Projektmappe in macOS](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="7c92f-227">Navigieren Sie in Visual Studio für Mac-Versionen vor Version 8.6 zu **.NET Core** > **App** > **Webanwendung (Model-View-Controller)**  > **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="7c92f-227">In Visual Studio for Mac earlier than version 8.6, select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span> <span data-ttu-id="7c92f-228">Bei Version 8.6 oder höher klicken Sie auf **Web and Console** > **App** > **Webanwendung (Model-View-Controller)**  > **Weiter** (Web und Konsole).</span><span class="sxs-lookup"><span data-stu-id="7c92f-228">In version 8.6 or later, select **Web and Console** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

* <span data-ttu-id="7c92f-229">Übernehmen Sie im Dialogfeld **Neue ASP.NET Core-Web-API konfigurieren** die Standardeinstellung für **Zielframework** von **.NET Core 2.2**.</span><span class="sxs-lookup"><span data-stu-id="7c92f-229">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of **.NET Core 2.2**.</span></span>

  ![Auswahl für .NET Core 2.2 in macOS](./start-mvc/_static/new_project_22_vsmac.png)

* <span data-ttu-id="7c92f-231">Nennen Sie das Projekt **MvcMovie**, und wählen Sie dann **Erstellen** aus.</span><span class="sxs-lookup"><span data-stu-id="7c92f-231">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="7c92f-232">Ausführen der App</span><span class="sxs-lookup"><span data-stu-id="7c92f-232">Run the app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="7c92f-233">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7c92f-233">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="7c92f-234">Drücken Sie **STRG+F5**, um die App im Nicht-Debugmodus auszuführen.</span><span class="sxs-lookup"><span data-stu-id="7c92f-234">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="7c92f-235">Visual Studio startet [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) und führt die App aus.</span><span class="sxs-lookup"><span data-stu-id="7c92f-235">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="7c92f-236">Beachten Sie, dass die Adressleiste `localhost:port#` und nicht etwas wie `example.com` anzeigt.</span><span class="sxs-lookup"><span data-stu-id="7c92f-236">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="7c92f-237">Das liegt daran, dass es sich bei `localhost` um den Standard-Hostnamen für Ihren lokalen Computer handelt.</span><span class="sxs-lookup"><span data-stu-id="7c92f-237">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="7c92f-238">Wenn in Visual Studio ein Webprojekt erstellt wird, wird für den Webserver ein zufälliger Port verwendet.</span><span class="sxs-lookup"><span data-stu-id="7c92f-238">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="7c92f-239">Das Starten der App mit STRG+F5 (Nicht-Debugmodus) ermöglicht die Änderung des Codes, das Speichern der Datei, das Aktualisieren des Browsers und das Anzeigen von Codeänderungen.</span><span class="sxs-lookup"><span data-stu-id="7c92f-239">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="7c92f-240">Viele Entwickler bevorzugen den Nicht-Debugmodus, um die App schnell zu starten und Änderungen anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="7c92f-240">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="7c92f-241">Sie können die App über das Menüelement **Debuggen** im Debugmodus oder Nicht-Debugmodus starten:</span><span class="sxs-lookup"><span data-stu-id="7c92f-241">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![Menü „Debuggen“](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="7c92f-243">Sie können die App debuggen, indem Sie die Schaltfläche **IIS Express** auswählen.</span><span class="sxs-lookup"><span data-stu-id="7c92f-243">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

* <span data-ttu-id="7c92f-245">Wählen Sie **Akzeptieren** aus, um der Nachverfolgung zuzustimmen.</span><span class="sxs-lookup"><span data-stu-id="7c92f-245">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="7c92f-246">Diese App verfolgt keine personenbezogenen Informationen nach.</span><span class="sxs-lookup"><span data-stu-id="7c92f-246">This app doesn't track personal information.</span></span> <span data-ttu-id="7c92f-247">Der generierte Vorlagencode enthält Objekte, die bei der Erfüllung der [Datenschutz-Grundverordnung (DSGVO)](xref:security/gdpr) als Unterstützung dienen sollen.</span><span class="sxs-lookup"><span data-stu-id="7c92f-247">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Start- oder Indexseite](start-mvc/_static/privacy.png)

  <span data-ttu-id="7c92f-249">Die folgende Abbildung zeigt die App, nachdem die Nachverfolgung akzeptiert wurde:</span><span class="sxs-lookup"><span data-stu-id="7c92f-249">The following image shows the app after accepting tracking:</span></span>

  ![Start- oder Indexseite](start-mvc/_static/home2.2.png)

# <a name="visual-studio-code"></a>[<span data-ttu-id="7c92f-251">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7c92f-251">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="7c92f-252">Drücken Sie STRG+F5, um die Ausführung ohne den Debugger zu starten.</span><span class="sxs-lookup"><span data-stu-id="7c92f-252">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="7c92f-253">Visual Studio Code startet [Kestrel](xref:fundamentals/servers/kestrel) und einen Browser und navigiert zu `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="7c92f-253">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="7c92f-254">Die Adressleiste zeigt `localhost:port:5001` an, nicht `example.com`.</span><span class="sxs-lookup"><span data-stu-id="7c92f-254">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="7c92f-255">Das liegt daran, dass es sich bei `localhost` um den Standardhostnamen für den lokalen Computer handelt.</span><span class="sxs-lookup"><span data-stu-id="7c92f-255">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="7c92f-256">„Localhost“ dient nur Webanforderungen vom lokalen Computer.</span><span class="sxs-lookup"><span data-stu-id="7c92f-256">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="7c92f-257">Das Starten der App mit STRG+F5 (Nicht-Debugmodus) ermöglicht die Änderung des Codes, das Speichern der Datei, das Aktualisieren des Browsers und das Anzeigen von Codeänderungen.</span><span class="sxs-lookup"><span data-stu-id="7c92f-257">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="7c92f-258">Viele Entwickler bevorzugen den Nicht-Debugmodus, um die Seite zu aktualisieren und Änderungen anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="7c92f-258">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

* <span data-ttu-id="7c92f-259">Wählen Sie **Akzeptieren** aus, um der Nachverfolgung zuzustimmen.</span><span class="sxs-lookup"><span data-stu-id="7c92f-259">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="7c92f-260">Diese App verfolgt keine personenbezogenen Informationen nach.</span><span class="sxs-lookup"><span data-stu-id="7c92f-260">This app doesn't track personal information.</span></span> <span data-ttu-id="7c92f-261">Der generierte Vorlagencode enthält Objekte, die bei der Erfüllung der [Datenschutz-Grundverordnung (DSGVO)](xref:security/gdpr) als Unterstützung dienen sollen.</span><span class="sxs-lookup"><span data-stu-id="7c92f-261">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Start- oder Indexseite](start-mvc/_static/privacy.png)

  <span data-ttu-id="7c92f-263">Die folgende Abbildung zeigt die App, nachdem die Nachverfolgung akzeptiert wurde:</span><span class="sxs-lookup"><span data-stu-id="7c92f-263">The following image shows the app after accepting tracking:</span></span>

  ![Start- oder Indexseite](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="7c92f-265">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="7c92f-265">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="7c92f-266">Wählen Sie **Ausführen** > **Ohne Debuggen starten** aus, um die App zu starten.</span><span class="sxs-lookup"><span data-stu-id="7c92f-266">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="7c92f-267">Visual Studio für Mac startet den [Kestrel](xref:fundamentals/servers/index#kestrel)-Server und einen Browser und navigiert zu `http://localhost:port`, wobei es sich bei *port* um eine zufällig ausgewählte Portnummer handelt.</span><span class="sxs-lookup"><span data-stu-id="7c92f-267">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="7c92f-268">Die Adressleiste zeigt `localhost:port#` an, nicht `example.com`.</span><span class="sxs-lookup"><span data-stu-id="7c92f-268">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="7c92f-269">Das liegt daran, dass es sich bei `localhost` um den Standard-Hostnamen für Ihren lokalen Computer handelt.</span><span class="sxs-lookup"><span data-stu-id="7c92f-269">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="7c92f-270">Wenn in Visual Studio ein Webprojekt erstellt wird, wird für den Webserver ein zufälliger Port verwendet.</span><span class="sxs-lookup"><span data-stu-id="7c92f-270">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="7c92f-271">Wenn Sie die App ausführen, wird eine andere Portnummer angezeigt.</span><span class="sxs-lookup"><span data-stu-id="7c92f-271">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="7c92f-272">Sie können die App über das Menü **Ausführen** im Debugmodus oder Nicht-Debugmodus starten.</span><span class="sxs-lookup"><span data-stu-id="7c92f-272">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

* <span data-ttu-id="7c92f-273">Wählen Sie **Akzeptieren** aus, um der Nachverfolgung zuzustimmen.</span><span class="sxs-lookup"><span data-stu-id="7c92f-273">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="7c92f-274">Diese App verfolgt keine personenbezogenen Informationen nach.</span><span class="sxs-lookup"><span data-stu-id="7c92f-274">This app doesn't track personal information.</span></span> <span data-ttu-id="7c92f-275">Der generierte Vorlagencode enthält Objekte, die bei der Erfüllung der [Datenschutz-Grundverordnung (DSGVO)](xref:security/gdpr) als Unterstützung dienen sollen.</span><span class="sxs-lookup"><span data-stu-id="7c92f-275">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Start- oder Indexseite](./start-mvc/_static/output_privacy_macos.png)

  <span data-ttu-id="7c92f-277">Die folgende Abbildung zeigt die App, nachdem die Nachverfolgung akzeptiert wurde:</span><span class="sxs-lookup"><span data-stu-id="7c92f-277">The following image shows the app after accepting tracking:</span></span>

  ![Start- oder Indexseite](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="7c92f-279">Im nächsten Teil dieses Tutorials erfahren Sie mehr über MVC und beginnen mit dem Schreiben von Code.</span><span class="sxs-lookup"><span data-stu-id="7c92f-279">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="7c92f-280">Nächste</span><span class="sxs-lookup"><span data-stu-id="7c92f-280">Next</span></span>](adding-controller.md)

::: moniker-end
