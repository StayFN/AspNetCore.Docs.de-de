---
title: Verwenden von ASP.NET Core SignalR mit Blazor WebAssembly
author: guardrex
description: Erstellen Sie eine Chat-App, die ASP.NET Core SignalR mit Blazor WebAssembly verwendet.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/10/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: tutorials/signalr-blazor-webassembly
ms.openlocfilehash: 5a58e7ae28842e2e8a0f3bae8f47e252839903fe
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.contentlocale: de-DE
ms.lasthandoff: 06/26/2020
ms.locfileid: "85408876"
---
# <a name="use-aspnet-core-signalr-with-blazor-webassembly"></a><span data-ttu-id="a03fa-103">Verwenden von ASP.NET Core SignalR mit Blazor WebAssembly</span><span class="sxs-lookup"><span data-stu-id="a03fa-103">Use ASP.NET Core SignalR with Blazor WebAssembly</span></span>

<span data-ttu-id="a03fa-104">Von [Daniel Roth](https://github.com/danroth27) und [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="a03fa-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="a03fa-105">In diesem Tutorial werden die Grundlagen zur Erstellung einer Echtzeit-App mit SignalR mit Blazor WebAssembly beschrieben.</span><span class="sxs-lookup"><span data-stu-id="a03fa-105">This tutorial teaches the basics of building a real-time app using SignalR with Blazor WebAssembly.</span></span> <span data-ttu-id="a03fa-106">Sie lernen Folgendes:</span><span class="sxs-lookup"><span data-stu-id="a03fa-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a03fa-107">Erstellen eines gehosteten Blazor WebAssembly-App-Projekts</span><span class="sxs-lookup"><span data-stu-id="a03fa-107">Create a Blazor WebAssembly Hosted app project</span></span>
> * <span data-ttu-id="a03fa-108">Hinzufügen der SignalR-Clientbibliothek</span><span class="sxs-lookup"><span data-stu-id="a03fa-108">Add the SignalR client library</span></span>
> * <span data-ttu-id="a03fa-109">Hinzufügen eines SignalR-Hubs</span><span class="sxs-lookup"><span data-stu-id="a03fa-109">Add a SignalR hub</span></span>
> * <span data-ttu-id="a03fa-110">Hinzufügen von SignalR-Diensten und eines Endpunkts zum SignalR-Hub</span><span class="sxs-lookup"><span data-stu-id="a03fa-110">Add SignalR services and an endpoint for the SignalR hub</span></span>
> * <span data-ttu-id="a03fa-111">Hinzufügen von Razor-Komponentencode für Chat</span><span class="sxs-lookup"><span data-stu-id="a03fa-111">Add Razor component code for chat</span></span>

<span data-ttu-id="a03fa-112">Am Ende dieses Tutorials verfügen Sie über eine funktionierende Chat-App.</span><span class="sxs-lookup"><span data-stu-id="a03fa-112">At the end of this tutorial, you'll have a working chat app.</span></span>

<span data-ttu-id="a03fa-113">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-blazor-webassembly/samples/) ([Vorgehensweise zum Herunterladen](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a03fa-113">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-blazor-webassembly/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a03fa-114">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="a03fa-114">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="a03fa-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a03fa-115">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a03fa-116">[Visual Studio 2019 Version 16.6 oder höher](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) mit der Workload **ASP.NET und Webentwicklung**</span><span class="sxs-lookup"><span data-stu-id="a03fa-116">[Visual Studio 2019 16.6 or later](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the **ASP.NET and web development** workload</span></span>
* [!INCLUDE [.NET Core 3.1 SDK](~/includes/3.1-SDK.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="a03fa-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a03fa-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="a03fa-118">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="a03fa-118">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="a03fa-119">Visual Studio für Mac Version 8.6 oder höher</span><span class="sxs-lookup"><span data-stu-id="a03fa-119">Visual Studio for Mac version 8.6 or later</span></span>](https://visualstudio.microsoft.com/vs/mac/)
* [!INCLUDE [.NET Core 3.1 SDK](~/includes/3.1-SDK.md)]

# <a name="net-core-cli"></a>[<span data-ttu-id="a03fa-120">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="a03fa-120">.NET Core CLI</span></span>](#tab/netcore-cli/)

[!INCLUDE[](~/includes/3.1-SDK.md)]

---

## <a name="create-a-hosted-blazor-webassembly-app-project"></a><span data-ttu-id="a03fa-121">Erstellen eines gehosteten Blazor WebAssembly-App-Projekts</span><span class="sxs-lookup"><span data-stu-id="a03fa-121">Create a hosted Blazor WebAssembly app project</span></span>

<span data-ttu-id="a03fa-122">Befolgen Sie die Anleitungen für die Auswahl der Tools:</span><span class="sxs-lookup"><span data-stu-id="a03fa-122">Follow the guidance for your choice of tooling:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="a03fa-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a03fa-123">Visual Studio</span></span>](#tab/visual-studio)

> [!NOTE]
> <span data-ttu-id="a03fa-124">Visual Studio 16.6 oder höher und das .NET Core SDK 3.1.300 oder höher sind erforderlich.</span><span class="sxs-lookup"><span data-stu-id="a03fa-124">Visual Studio 16.6 or later and .NET Core SDK 3.1.300 or later are required.</span></span>

1. <span data-ttu-id="a03fa-125">Erstellen Sie ein neues Projekt.</span><span class="sxs-lookup"><span data-stu-id="a03fa-125">Create a new project.</span></span>

1. <span data-ttu-id="a03fa-126">Klicken Sie auf **Blazor-App** und dann auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="a03fa-126">Select **Blazor App** and select **Next**.</span></span>

1. <span data-ttu-id="a03fa-127">Geben Sie im Feld **Projektname** `BlazorSignalRApp` ein.</span><span class="sxs-lookup"><span data-stu-id="a03fa-127">Type `BlazorSignalRApp` in the **Project name** field.</span></span> <span data-ttu-id="a03fa-128">Vergewissern Sie sich, dass der Eintrag für den **Speicherort** korrekt ist, oder geben Sie einen Speicherort für das Projekt an.</span><span class="sxs-lookup"><span data-stu-id="a03fa-128">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="a03fa-129">Wählen Sie **Erstellen** aus.</span><span class="sxs-lookup"><span data-stu-id="a03fa-129">Select **Create**.</span></span>

1. <span data-ttu-id="a03fa-130">Wählen Sie die **Blazor WebAssembly App**-Vorlage aus.</span><span class="sxs-lookup"><span data-stu-id="a03fa-130">Choose the **Blazor WebAssembly App** template.</span></span>

1. <span data-ttu-id="a03fa-131">Aktivieren Sie unter **Erweitert** das Kontrollkästchen **Von ASP.net Core gehostet**.</span><span class="sxs-lookup"><span data-stu-id="a03fa-131">Under **Advanced**, select the **ASP.NET Core hosted** check box.</span></span>

1. <span data-ttu-id="a03fa-132">Wählen Sie **Erstellen** aus.</span><span class="sxs-lookup"><span data-stu-id="a03fa-132">Select **Create**.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="a03fa-133">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a03fa-133">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="a03fa-134">Führen Sie in einer Befehlsshell den folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="a03fa-134">In a command shell, execute the following command:</span></span>

   ```dotnetcli
   dotnet new blazorwasm --hosted --output BlazorSignalRApp
   ```

1. <span data-ttu-id="a03fa-135">Öffnen Sie in Visual Studio Code den Projektordner der App.</span><span class="sxs-lookup"><span data-stu-id="a03fa-135">In Visual Studio Code, open the app's project folder.</span></span>

1. <span data-ttu-id="a03fa-136">Wenn das Dialogfeld angezeigt wird, um Assets zum Erstellen und Debuggen der App hinzuzufügen, wählen Sie **Ja**aus.</span><span class="sxs-lookup"><span data-stu-id="a03fa-136">When the dialog appears to add assets to build and debug the app, select **Yes**.</span></span> <span data-ttu-id="a03fa-137">Visual Studio Code fügt den `.vscode`-Ordner automatisch mit generierten `launch.json`- und `tasks.json`-Dateien hinzu.</span><span class="sxs-lookup"><span data-stu-id="a03fa-137">Visual Studio Code automatically adds the `.vscode` folder with generated `launch.json` and `tasks.json` files.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="a03fa-138">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="a03fa-138">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="a03fa-139">Installieren Sie die neueste Version von [Visual Studio für Mac](https://visualstudio.microsoft.com/vs/mac/), und führen Sie die folgenden Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="a03fa-139">Install the latest version of [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/) and perform the following steps:</span></span>

1. <span data-ttu-id="a03fa-140">Wählen Sie **Datei** > **Neue Projektmappe** aus, oder erstellen Sie im **Startfenster** ein **Neues Projekt**.</span><span class="sxs-lookup"><span data-stu-id="a03fa-140">Select **File** > **New Solution** or create a **New** project from the **Start Window**.</span></span>

1. <span data-ttu-id="a03fa-141">Klicken Sie auf der Randleiste auf **Web and Console** > **App** (Web und Konsole).</span><span class="sxs-lookup"><span data-stu-id="a03fa-141">In the sidebar, select **Web and Console** > **App**.</span></span>

1. <span data-ttu-id="a03fa-142">Wählen Sie die **Blazor WebAssembly App**-Vorlage aus.</span><span class="sxs-lookup"><span data-stu-id="a03fa-142">Choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="a03fa-143">Klicken Sie auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="a03fa-143">Select **Next**.</span></span>

   <span data-ttu-id="a03fa-144">Bestätigen Sie die folgenden Konfigurationen:</span><span class="sxs-lookup"><span data-stu-id="a03fa-144">Confirm the following configurations:</span></span>

   * <span data-ttu-id="a03fa-145">**Zielframework** auf **.NET Core 3.1** festgelegt</span><span class="sxs-lookup"><span data-stu-id="a03fa-145">**Target Framework** set to **.NET Core 3.1**.</span></span>
   * <span data-ttu-id="a03fa-146">**Authentifizierung** auf **Keine Authentifizierung** festgelegt</span><span class="sxs-lookup"><span data-stu-id="a03fa-146">**Authentication** set to **No Authentication**.</span></span>

   <span data-ttu-id="a03fa-147">Aktivieren Sie das Kontrollkästchen **ASP.NET Core Hosted** (Auf ASP.NET Core gehostet).</span><span class="sxs-lookup"><span data-stu-id="a03fa-147">Select the **ASP.NET Core Hosted** check box.</span></span>

   <span data-ttu-id="a03fa-148">Klicken Sie auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="a03fa-148">Select **Next**.</span></span>

1. <span data-ttu-id="a03fa-149">Benennen Sie im Feld **Projektname** die App `BlazorSignalRApp`.</span><span class="sxs-lookup"><span data-stu-id="a03fa-149">In the **Project Name** field, name the app `BlazorSignalRApp`.</span></span> <span data-ttu-id="a03fa-150">Wählen Sie **Erstellen** aus.</span><span class="sxs-lookup"><span data-stu-id="a03fa-150">Select **Create**.</span></span>

   <span data-ttu-id="a03fa-151">Wenn eine Eingabeaufforderung dem Entwicklungszertifikat zu vertrauen scheint, vertrauen Sie dem Zertifikat und fahren Sie fort.</span><span class="sxs-lookup"><span data-stu-id="a03fa-151">If a prompt appears to trust the development certificate, trust the certificate and continue.</span></span> <span data-ttu-id="a03fa-152">Das Benutzer- und Keychainkennwort sind erforderlich, um dem Zertifikat zu vertrauen.</span><span class="sxs-lookup"><span data-stu-id="a03fa-152">The user and keychain passwords are required to trust the certificate.</span></span>

1. <span data-ttu-id="a03fa-153">Öffnen Sie das Projekt, indem Sie zum Projektordner navigieren und die Projektmappendatei des Projekts (`.sln`) öffnen.</span><span class="sxs-lookup"><span data-stu-id="a03fa-153">Open the project by navigating to the project folder and opening the project's solution file (`.sln`).</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="a03fa-154">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="a03fa-154">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="a03fa-155">Führen Sie in einer Befehlsshell den folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="a03fa-155">In a command shell, execute the following command:</span></span>

```dotnetcli
dotnet new blazorwasm --hosted --output BlazorSignalRApp
```

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="a03fa-156">Hinzufügen der SignalR-Clientbibliothek</span><span class="sxs-lookup"><span data-stu-id="a03fa-156">Add the SignalR client library</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="a03fa-157">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a03fa-157">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="a03fa-158">Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das `BlazorSignalRApp.Client`-Projekt, und wählen Sie **NuGet-Pakete verwalten** aus.</span><span class="sxs-lookup"><span data-stu-id="a03fa-158">In **Solution Explorer**, right-click the `BlazorSignalRApp.Client` project and select **Manage NuGet Packages**.</span></span>

1. <span data-ttu-id="a03fa-159">Überprüfen Sie im Dialogfeld **NuGet-Pakete verwalten**, ob die **Paketquelle** auf `nuget.org` eingestellt ist.</span><span class="sxs-lookup"><span data-stu-id="a03fa-159">In the **Manage NuGet Packages** dialog, confirm that the **Package source** is set to `nuget.org`.</span></span>

1. <span data-ttu-id="a03fa-160">Geben Sie bei Auswahl von **Durchsuchen** `Microsoft.AspNetCore.SignalR.Client` in das Suchfeld ein.</span><span class="sxs-lookup"><span data-stu-id="a03fa-160">With **Browse** selected, type `Microsoft.AspNetCore.SignalR.Client` in the search box.</span></span>

1. <span data-ttu-id="a03fa-161">Wählen Sie in den Suchergebnissen das [`Microsoft.AspNetCore.SignalR.Client`](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client)-Paket aus, und klicken Sie dann auf **Installieren**.</span><span class="sxs-lookup"><span data-stu-id="a03fa-161">In the search results, select the [`Microsoft.AspNetCore.SignalR.Client`](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client) package and select **Install**.</span></span>

1. <span data-ttu-id="a03fa-162">Wenn das Dialogfeld **Vorschau der Änderungen anzeigen** angezeigt wird, wählen Sie die Option **OK** aus.</span><span class="sxs-lookup"><span data-stu-id="a03fa-162">If the **Preview Changes** dialog appears, select **OK**.</span></span>

1. <span data-ttu-id="a03fa-163">Wenn das Dialogfeld **Zustimmung zur Lizenz** erscheint, wählen Sie **Ich stimme zu** aus, wenn Sie mit den Lizenzbedingungen einverstanden sind.</span><span class="sxs-lookup"><span data-stu-id="a03fa-163">If the **License Acceptance** dialog appears, select **I Accept** if you agree with the license terms.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="a03fa-164">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a03fa-164">Visual Studio Code</span></span>](#tab/visual-studio-code/)

<span data-ttu-id="a03fa-165">Führen Sie im **integrierten Terminal** (**Ansicht** > **Terminal** in der Symbolleiste) die folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="a03fa-165">In the **Integrated Terminal** (**View** > **Terminal** from the toolbar), execute the following commands:</span></span>

```dotnetcli
dotnet add Client package Microsoft.AspNetCore.SignalR.Client
```

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="a03fa-166">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="a03fa-166">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="a03fa-167">Klicken Sie in der **Projektmappen**-Randleiste mit der rechten Maustaste auf das `BlazorSignalRApp.Client`-Projekt, und wählen Sie **NuGet-Pakete verwalten** aus.</span><span class="sxs-lookup"><span data-stu-id="a03fa-167">In the **Solution** sidebar, right-click the `BlazorSignalRApp.Client` project and select **Manage NuGet Packages**.</span></span>

1. <span data-ttu-id="a03fa-168">Überprüfen Sie im Dialogfeld **NuGet-Pakete verwalten**, ob in der Dropdownliste für die Paketquelle `nuget.org` ausgewählt ist.</span><span class="sxs-lookup"><span data-stu-id="a03fa-168">In the **Manage NuGet Packages** dialog, confirm that the source drop-down is set to `nuget.org`.</span></span>

1. <span data-ttu-id="a03fa-169">Geben Sie bei Auswahl von **Durchsuchen** `Microsoft.AspNetCore.SignalR.Client` in das Suchfeld ein.</span><span class="sxs-lookup"><span data-stu-id="a03fa-169">With **Browse** selected, type `Microsoft.AspNetCore.SignalR.Client` in the search box.</span></span>

1. <span data-ttu-id="a03fa-170">Aktivieren Sie in den Suchergebnissen das Kontrollkästchen neben dem [`Microsoft.AspNetCore.SignalR.Client`](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client)-Paket, und wählen Sie **Paket hinzufügen** aus.</span><span class="sxs-lookup"><span data-stu-id="a03fa-170">In the search results, select the check box next to the [`Microsoft.AspNetCore.SignalR.Client`](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client) package and select **Add Package**.</span></span>

1. <span data-ttu-id="a03fa-171">Wenn das Dialogfeld **Zustimmung zur Lizenz** erscheint, wählen Sie **Ich stimme zu** aus, wenn Sie mit den Lizenzbedingungen einverstanden sind.</span><span class="sxs-lookup"><span data-stu-id="a03fa-171">If the **License Acceptance** dialog appears, select **Accept** if you agree with the license terms.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="a03fa-172">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="a03fa-172">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="a03fa-173">Führen Sie in einer Befehlsshell die folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="a03fa-173">In a command shell, execute the following commands:</span></span>

```dotnetcli
cd BlazorSignalRApp
dotnet add Client package Microsoft.AspNetCore.SignalR.Client
```

---

## <a name="add-a-signalr-hub"></a><span data-ttu-id="a03fa-174">Hinzufügen eines SignalR-Hubs</span><span class="sxs-lookup"><span data-stu-id="a03fa-174">Add a SignalR hub</span></span>

<span data-ttu-id="a03fa-175">Erstellen Sie im `BlazorSignalRApp.Server`-Projekt einen `Hubs`-Ordner (Plural), und fügen Sie die folgende `ChatHub`-Klasse (`Hubs/ChatHub.cs`) hinzu:</span><span class="sxs-lookup"><span data-stu-id="a03fa-175">In the `BlazorSignalRApp.Server` project, create a `Hubs` (plural) folder and add the following `ChatHub` class (`Hubs/ChatHub.cs`):</span></span>

[!code-csharp[](signalr-blazor-webassembly/samples/3.x/BlazorSignalRApp/Server/Hubs/ChatHub.cs)]

## <a name="add-services-and-an-endpoint-for-the-signalr-hub"></a><span data-ttu-id="a03fa-176">Hinzufügen von Diensten und eines Endpunkts zum SignalR-Hub</span><span class="sxs-lookup"><span data-stu-id="a03fa-176">Add services and an endpoint for the SignalR hub</span></span>

1. <span data-ttu-id="a03fa-177">Öffnen Sie im Projekt `BlazorSignalRApp.Server` die Datei `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="a03fa-177">In the `BlazorSignalRApp.Server` project, open the `Startup.cs` file.</span></span>

1. <span data-ttu-id="a03fa-178">Fügen Sie am Anfang der Datei den Namespace für die `ChatHub`-Klasse hinzu:</span><span class="sxs-lookup"><span data-stu-id="a03fa-178">Add the namespace for the `ChatHub` class to the top of the file:</span></span>

   ```csharp
   using BlazorSignalRApp.Server.Hubs;
   ```

1. <span data-ttu-id="a03fa-179">Fügen Sie SignalR und Middlewaredienste für die Komprimierung von Antworten zu `Startup.ConfigureServices` hinzu:</span><span class="sxs-lookup"><span data-stu-id="a03fa-179">Add SignalR and Response Compression Middleware services to `Startup.ConfigureServices`:</span></span>

   [!code-csharp[](signalr-blazor-webassembly/samples/3.x/BlazorSignalRApp/Server/Startup.cs?name=snippet_ConfigureServices&highlight=3,5-9)]

1. <span data-ttu-id="a03fa-180">In `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="a03fa-180">In `Startup.Configure`:</span></span>

   * <span data-ttu-id="a03fa-181">Verwenden Sie die Middlewaredienste für die Komprimierung von Antworten am Anfang der Konfiguration der Verarbeitungspipeline.</span><span class="sxs-lookup"><span data-stu-id="a03fa-181">Use Response Compression Middleware at the top of the processing pipeline's configuration.</span></span>
   * <span data-ttu-id="a03fa-182">Fügen Sie zwischen den Endpunkten für Controller und dem clientseitigen Fallback einen Endpunkt für den Hub hinzu.</span><span class="sxs-lookup"><span data-stu-id="a03fa-182">Between the endpoints for controllers and the client-side fallback, add an endpoint for the hub.</span></span>

   [!code-csharp[](signalr-blazor-webassembly/samples/3.x/BlazorSignalRApp/Server/Startup.cs?name=snippet_Configure&highlight=3,25)]

## <a name="add-razor-component-code-for-chat"></a><span data-ttu-id="a03fa-183">Hinzufügen von Razor-Komponentencode für Chat</span><span class="sxs-lookup"><span data-stu-id="a03fa-183">Add Razor component code for chat</span></span>

1. <span data-ttu-id="a03fa-184">Öffnen Sie im Projekt `BlazorSignalRApp.Client` die Datei `Pages/Index.razor`.</span><span class="sxs-lookup"><span data-stu-id="a03fa-184">In the `BlazorSignalRApp.Client` project, open the `Pages/Index.razor` file.</span></span>

1. <span data-ttu-id="a03fa-185">Ersetzen Sie das Markup durch folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="a03fa-185">Replace the markup with the following code:</span></span>

[!code-razor[](signalr-blazor-webassembly/samples/3.x/BlazorSignalRApp/Client/Pages/Index.razor)]

## <a name="run-the-app"></a><span data-ttu-id="a03fa-186">Ausführen der App</span><span class="sxs-lookup"><span data-stu-id="a03fa-186">Run the app</span></span>

1. <span data-ttu-id="a03fa-187">Befolgen Sie die Anleitungen für die Auswahl Ihrer Tools:</span><span class="sxs-lookup"><span data-stu-id="a03fa-187">Follow the guidance for your tooling:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="a03fa-188">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a03fa-188">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="a03fa-189">Wählen Sie im **Projektmappen-Explorer** das `BlazorSignalRApp.Server`-Projekt aus.</span><span class="sxs-lookup"><span data-stu-id="a03fa-189">In **Solution Explorer**, select the `BlazorSignalRApp.Server` project.</span></span> <span data-ttu-id="a03fa-190">Drücken Sie <kbd>F5</kbd>, um die App mit Debuggen auszuführen, oder drücken Sie <kbd>STRG</kbd>+<kbd>F5</kbd>, um die App ohne Debuggen auszuführen.</span><span class="sxs-lookup"><span data-stu-id="a03fa-190">Press <kbd>F5</kbd> to run the app with debugging or <kbd>Ctrl</kbd>+<kbd>F5</kbd> to run the app without debugging.</span></span>

1. <span data-ttu-id="a03fa-191">Kopieren Sie die URL aus der Adressleiste, öffnen Sie eine neue Browserinstanz oder Registerkarte, und fügen Sie die URL in die Adressleiste ein.</span><span class="sxs-lookup"><span data-stu-id="a03fa-191">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="a03fa-192">Geben Sie in einem der beiden Browser einen Namen und eine Nachricht ein, und klicken Sie auf die Schaltfläche zum Senden der Nachricht.</span><span class="sxs-lookup"><span data-stu-id="a03fa-192">Choose either browser, enter a name and message, and select the button to send the message.</span></span> <span data-ttu-id="a03fa-193">Der Name und die Nachricht werden sofort auf beiden Seiten angezeigt:</span><span class="sxs-lookup"><span data-stu-id="a03fa-193">The name and message are displayed on both pages instantly:</span></span>

   <span data-ttu-id="a03fa-194">Die ![SignalR-Blazor WebAssembly-Beispiel-App wird in zwei Browserfenstern geöffnet, in denen die ausgetauschte Nachrichten angezeigt werden.](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)</span><span class="sxs-lookup"><span data-stu-id="a03fa-194">![SignalR Blazor WebAssembly sample app open in two browser windows showing exchanged messages.](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)</span></span>

   <span data-ttu-id="a03fa-195">Zitate: *Star Trek VI: Das unentdeckte Land* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span><span class="sxs-lookup"><span data-stu-id="a03fa-195">Quotes: *Star Trek VI: The Undiscovered Country* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="a03fa-196">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a03fa-196">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="a03fa-197">Wenn VS Code anbietet, ein Startprofil für die Server-App zu erstellen (`.vscode/launch.json`), scheint der `program`-Eintrag, ähnlich wie der folgende, auf die Assembly der App (`{APPLICATION NAME}.Server.dll`) zu zeigen:</span><span class="sxs-lookup"><span data-stu-id="a03fa-197">When VS Code offers to create a launch profile for the Server app (`.vscode/launch.json`), the `program` entry appears similar to the following to point to the app's assembly (`{APPLICATION NAME}.Server.dll`):</span></span>

   ```json
   "program": "${workspaceFolder}/Server/bin/Debug/netcoreapp3.1/{APPLICATION NAME}.Server.dll"
   ```

1. <span data-ttu-id="a03fa-198">Drücken Sie <kbd>F5</kbd>, um die App mit Debuggen auszuführen, oder drücken Sie <kbd>STRG</kbd>+<kbd>F5</kbd>, um die App ohne Debuggen auszuführen.</span><span class="sxs-lookup"><span data-stu-id="a03fa-198">Press <kbd>F5</kbd> to run the app with debugging or <kbd>Ctrl</kbd>+<kbd>F5</kbd> to run the app without debugging.</span></span>

1. <span data-ttu-id="a03fa-199">Kopieren Sie die URL aus der Adressleiste, öffnen Sie eine neue Browserinstanz oder Registerkarte, und fügen Sie die URL in die Adressleiste ein.</span><span class="sxs-lookup"><span data-stu-id="a03fa-199">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="a03fa-200">Geben Sie in einem der beiden Browser einen Namen und eine Nachricht ein, und klicken Sie auf die Schaltfläche zum Senden der Nachricht.</span><span class="sxs-lookup"><span data-stu-id="a03fa-200">Choose either browser, enter a name and message, and select the button to send the message.</span></span> <span data-ttu-id="a03fa-201">Der Name und die Nachricht werden sofort auf beiden Seiten angezeigt:</span><span class="sxs-lookup"><span data-stu-id="a03fa-201">The name and message are displayed on both pages instantly:</span></span>

   <span data-ttu-id="a03fa-202">Die ![SignalR-Blazor WebAssembly-Beispiel-App wird in zwei Browserfenstern geöffnet, in denen die ausgetauschte Nachrichten angezeigt werden.](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)</span><span class="sxs-lookup"><span data-stu-id="a03fa-202">![SignalR Blazor WebAssembly sample app open in two browser windows showing exchanged messages.](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)</span></span>

   <span data-ttu-id="a03fa-203">Zitate: *Star Trek VI: Das unentdeckte Land* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span><span class="sxs-lookup"><span data-stu-id="a03fa-203">Quotes: *Star Trek VI: The Undiscovered Country* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="a03fa-204">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="a03fa-204">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="a03fa-205">Wählen Sie in der **Projektmappen**-Randleiste das `BlazorSignalRApp.Server`-Projekt aus.</span><span class="sxs-lookup"><span data-stu-id="a03fa-205">In the **Solution** sidebar, select the `BlazorSignalRApp.Server` project.</span></span> <span data-ttu-id="a03fa-206">Drücken Sie <kbd>⌘</kbd>+<kbd>↩</kbd>, um die App mit Debuggen auszuführen, oder drücken Sie <kbd>⌥</kbd>+<kbd>⌘</kbd>+<kbd>↩</kbd>, um die App ohne Debuggen auszuführen.</span><span class="sxs-lookup"><span data-stu-id="a03fa-206">Press <kbd>⌘</kbd>+<kbd>↩</kbd> to run the app with debugging or <kbd>⌥</kbd>+<kbd>⌘</kbd>+<kbd>↩</kbd> to run the app without debugging.</span></span>

1. <span data-ttu-id="a03fa-207">Kopieren Sie die URL aus der Adressleiste, öffnen Sie eine neue Browserinstanz oder Registerkarte, und fügen Sie die URL in die Adressleiste ein.</span><span class="sxs-lookup"><span data-stu-id="a03fa-207">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="a03fa-208">Geben Sie in einem der beiden Browser einen Namen und eine Nachricht ein, und klicken Sie auf die Schaltfläche zum Senden der Nachricht.</span><span class="sxs-lookup"><span data-stu-id="a03fa-208">Choose either browser, enter a name and message, and select the button to send the message.</span></span> <span data-ttu-id="a03fa-209">Der Name und die Nachricht werden sofort auf beiden Seiten angezeigt:</span><span class="sxs-lookup"><span data-stu-id="a03fa-209">The name and message are displayed on both pages instantly:</span></span>

   <span data-ttu-id="a03fa-210">Die ![SignalR-Blazor WebAssembly-Beispiel-App wird in zwei Browserfenstern geöffnet, in denen die ausgetauschte Nachrichten angezeigt werden.](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)</span><span class="sxs-lookup"><span data-stu-id="a03fa-210">![SignalR Blazor WebAssembly sample app open in two browser windows showing exchanged messages.](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)</span></span>

   <span data-ttu-id="a03fa-211">Zitate: *Star Trek VI: Das unentdeckte Land* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span><span class="sxs-lookup"><span data-stu-id="a03fa-211">Quotes: *Star Trek VI: The Undiscovered Country* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="a03fa-212">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="a03fa-212">.NET Core CLI</span></span>](#tab/netcore-cli/)

1. <span data-ttu-id="a03fa-213">Führen Sie in einer Befehlsshell die folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="a03fa-213">In a command shell, execute the following commands:</span></span>

   ```dotnetcli
   cd Server
   dotnet run
   ```

1. <span data-ttu-id="a03fa-214">Kopieren Sie die URL aus der Adressleiste, öffnen Sie eine neue Browserinstanz oder Registerkarte, und fügen Sie die URL in die Adressleiste ein.</span><span class="sxs-lookup"><span data-stu-id="a03fa-214">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="a03fa-215">Geben Sie in einem der beiden Browser einen Namen und eine Nachricht ein, und klicken Sie auf die Schaltfläche zum Senden der Nachricht.</span><span class="sxs-lookup"><span data-stu-id="a03fa-215">Choose either browser, enter a name and message, and select the button to send the message.</span></span> <span data-ttu-id="a03fa-216">Der Name und die Nachricht werden sofort auf beiden Seiten angezeigt:</span><span class="sxs-lookup"><span data-stu-id="a03fa-216">The name and message are displayed on both pages instantly:</span></span>

   <span data-ttu-id="a03fa-217">Die ![SignalR-Blazor WebAssembly-Beispiel-App wird in zwei Browserfenstern geöffnet, in denen die ausgetauschte Nachrichten angezeigt werden.](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)</span><span class="sxs-lookup"><span data-stu-id="a03fa-217">![SignalR Blazor WebAssembly sample app open in two browser windows showing exchanged messages.](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)</span></span>

   <span data-ttu-id="a03fa-218">Zitate: *Star Trek VI: Das unentdeckte Land* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span><span class="sxs-lookup"><span data-stu-id="a03fa-218">Quotes: *Star Trek VI: The Undiscovered Country* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span></span>

---

## <a name="next-steps"></a><span data-ttu-id="a03fa-219">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="a03fa-219">Next steps</span></span>

<span data-ttu-id="a03fa-220">In diesem Tutorial haben Sie gelernt, wie die folgenden Aufgaben ausgeführt werden:</span><span class="sxs-lookup"><span data-stu-id="a03fa-220">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a03fa-221">Erstellen eines gehosteten Blazor WebAssembly-App-Projekts</span><span class="sxs-lookup"><span data-stu-id="a03fa-221">Create a Blazor WebAssembly Hosted app project</span></span>
> * <span data-ttu-id="a03fa-222">Hinzufügen der SignalR-Clientbibliothek</span><span class="sxs-lookup"><span data-stu-id="a03fa-222">Add the SignalR client library</span></span>
> * <span data-ttu-id="a03fa-223">Hinzufügen eines SignalR-Hubs</span><span class="sxs-lookup"><span data-stu-id="a03fa-223">Add a SignalR hub</span></span>
> * <span data-ttu-id="a03fa-224">Hinzufügen von SignalR-Diensten und eines Endpunkts zum SignalR-Hub</span><span class="sxs-lookup"><span data-stu-id="a03fa-224">Add SignalR services and an endpoint for the SignalR hub</span></span>
> * <span data-ttu-id="a03fa-225">Hinzufügen von Razor-Komponentencode für Chat</span><span class="sxs-lookup"><span data-stu-id="a03fa-225">Add Razor component code for chat</span></span>

<span data-ttu-id="a03fa-226">Weitere Informationen zum Erstellen von Blazor-Apps finden Sie in der Blazor-Dokumentation:</span><span class="sxs-lookup"><span data-stu-id="a03fa-226">To learn more about building Blazor apps, see the Blazor documentation:</span></span>

> [!div class="nextstepaction"]
> <xref:blazor/index>

## <a name="additional-resources"></a><span data-ttu-id="a03fa-227">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="a03fa-227">Additional resources</span></span>

* <xref:signalr/introduction>
* <span data-ttu-id="a03fa-228">[Ursprungsübergreifende SignalR-Aushandlung für die Authentifizierung](xref:blazor/fundamentals/additional-scenarios#signalr-cross-origin-negotiation-for-authentication)</span><span class="sxs-lookup"><span data-stu-id="a03fa-228">[SignalR cross-origin negotiation for authentication](xref:blazor/fundamentals/additional-scenarios#signalr-cross-origin-negotiation-for-authentication)</span></span>
