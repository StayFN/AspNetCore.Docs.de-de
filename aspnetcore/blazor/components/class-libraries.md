---
title: Klassenbibliotheken für ASP.NET Core-Razor-Komponenten
author: guardrex
description: Erfahren Sie, wie Komponenten aus einer externen Komponentenbibliothek in Blazor-Apps einbezogen werden können.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/23/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/components/class-libraries
ms.openlocfilehash: b172059407f9a08dacc0fadd804864c7aee7fb90
ms.sourcegitcommit: 66fca14611eba141d455fe0bd2c37803062e439c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2020
ms.locfileid: "85944493"
---
# <a name="aspnet-core-razor-components-class-libraries"></a><span data-ttu-id="e0b58-103">Klassenbibliotheken für ASP.NET Core-Razor-Komponenten</span><span class="sxs-lookup"><span data-stu-id="e0b58-103">ASP.NET Core Razor components class libraries</span></span>

<span data-ttu-id="e0b58-104">Von [Simon Timms](https://github.com/stimms)</span><span class="sxs-lookup"><span data-stu-id="e0b58-104">By [Simon Timms](https://github.com/stimms)</span></span>

<span data-ttu-id="e0b58-105">Komponenten können in einer [Razor-Klassenbibliothek (RCL)](xref:razor-pages/ui-class) projektübergreifend gemeinsam genutzt werden.</span><span class="sxs-lookup"><span data-stu-id="e0b58-105">Components can be shared in a [Razor class library (RCL)](xref:razor-pages/ui-class) across projects.</span></span> <span data-ttu-id="e0b58-106">Eine *Klassenbibliothek für Razor-Komponenten* kann wie folgt einbezogen werden:</span><span class="sxs-lookup"><span data-stu-id="e0b58-106">A *Razor components class library* can be included from:</span></span>

* <span data-ttu-id="e0b58-107">Über ein anderes Projekt in der Projektmappe.</span><span class="sxs-lookup"><span data-stu-id="e0b58-107">Another project in the solution.</span></span>
* <span data-ttu-id="e0b58-108">Über ein NuGet-Paket.</span><span class="sxs-lookup"><span data-stu-id="e0b58-108">A NuGet package.</span></span>
* <span data-ttu-id="e0b58-109">Über eine referenzierte .NET-Bibliothek.</span><span class="sxs-lookup"><span data-stu-id="e0b58-109">A referenced .NET library.</span></span>

<span data-ttu-id="e0b58-110">Genauso wie Komponenten normale .NET-Typen sind, sind von der RCL bereitgestellte Komponenten normale .NET-Assemblys.</span><span class="sxs-lookup"><span data-stu-id="e0b58-110">Just as components are regular .NET types, components provided by an RCL are normal .NET assemblies.</span></span>

## <a name="create-an-rcl"></a><span data-ttu-id="e0b58-111">Erstellen einer RCL</span><span class="sxs-lookup"><span data-stu-id="e0b58-111">Create an RCL</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="e0b58-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e0b58-112">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="e0b58-113">Erstellen Sie ein neues Projekt.</span><span class="sxs-lookup"><span data-stu-id="e0b58-113">Create a new project.</span></span>
1. <span data-ttu-id="e0b58-114">Klicken Sie auf **Razor-Klassenbibliothek**.</span><span class="sxs-lookup"><span data-stu-id="e0b58-114">Select **Razor Class Library**.</span></span> <span data-ttu-id="e0b58-115">Klicken Sie auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="e0b58-115">Select **Next**.</span></span>
1. <span data-ttu-id="e0b58-116">Klicken Sie im Dialogfeld **Neue Razor-Klassenbibliothek erstellen** auf **Erstellen**.</span><span class="sxs-lookup"><span data-stu-id="e0b58-116">In the **Create a new Razor class library** dialog, select **Create**.</span></span>
1. <span data-ttu-id="e0b58-117">Geben Sie im Feld **Projektname** einen Projektnamen ein, oder übernehmen Sie den Standardnamen.</span><span class="sxs-lookup"><span data-stu-id="e0b58-117">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="e0b58-118">Die Beispiele in diesem Thema verwenden den Projektnamen `MyComponentLib1`.</span><span class="sxs-lookup"><span data-stu-id="e0b58-118">The examples in this topic use the project name `MyComponentLib1`.</span></span> <span data-ttu-id="e0b58-119">Wählen Sie **Erstellen** aus.</span><span class="sxs-lookup"><span data-stu-id="e0b58-119">Select **Create**.</span></span>
1. <span data-ttu-id="e0b58-120">Fügen Sie die RCL zu einer Projektmappe hinzu:</span><span class="sxs-lookup"><span data-stu-id="e0b58-120">Add the RCL to a solution:</span></span>
   1. <span data-ttu-id="e0b58-121">Klicken Sie mit der rechten Maustaste auf die Projektmappe.</span><span class="sxs-lookup"><span data-stu-id="e0b58-121">Right-click the solution.</span></span> <span data-ttu-id="e0b58-122">Wählen Sie **Hinzufügen** > **Vorhandenes Projekt** aus.</span><span class="sxs-lookup"><span data-stu-id="e0b58-122">Select **Add** > **Existing Project**.</span></span>
   1. <span data-ttu-id="e0b58-123">Navigieren Sie zur Projektdatei der RCL.</span><span class="sxs-lookup"><span data-stu-id="e0b58-123">Navigate to the RCL's project file.</span></span>
   1. <span data-ttu-id="e0b58-124">Wählen Sie die Projektdatei (`.csproj`) der RCL aus.</span><span class="sxs-lookup"><span data-stu-id="e0b58-124">Select the RCL's project file (`.csproj`).</span></span>
1. <span data-ttu-id="e0b58-125">Fügen Sie einen Verweis auf die RCL aus der App hinzu:</span><span class="sxs-lookup"><span data-stu-id="e0b58-125">Add a reference the RCL from the app:</span></span>
   1. <span data-ttu-id="e0b58-126">Klicken Sie mit der rechten Maustaste auf das App-Projekt.</span><span class="sxs-lookup"><span data-stu-id="e0b58-126">Right-click the app project.</span></span> <span data-ttu-id="e0b58-127">Wählen Sie **Hinzufügen** > **Verweis** aus.</span><span class="sxs-lookup"><span data-stu-id="e0b58-127">Select **Add** > **Reference**.</span></span>
   1. <span data-ttu-id="e0b58-128">Wählen Sie das RCL-Projekt aus.</span><span class="sxs-lookup"><span data-stu-id="e0b58-128">Select the RCL project.</span></span> <span data-ttu-id="e0b58-129">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="e0b58-129">Select **OK**.</span></span>

> [!NOTE]
> <span data-ttu-id="e0b58-130">Wenn das Kontrollkästchen **Seiten und Ansichten unterstützen** beim Erstellen der RCL aus der Vorlage aktiviert ist, fügen Sie auch eine `_Imports.razor`-Datei mit folgendem Inhalt zum Stammverzeichnis des generierten Projekts hinzu, um das Erstellen von Razor-Komponenten zu ermöglichen:</span><span class="sxs-lookup"><span data-stu-id="e0b58-130">If the **Support pages and views** check box is selected when generating the RCL from the template, then also add an `_Imports.razor` file to root of the generated project with the following contents to enable Razor component authoring:</span></span>
>
> ```razor
> @using Microsoft.AspNetCore.Components.Web
> ```
>
> <span data-ttu-id="e0b58-131">Fügen Sie die Datei manuell dem Stammverzeichnis des generierten Projekts hinzu.</span><span class="sxs-lookup"><span data-stu-id="e0b58-131">Manually add the file the root of the generated project.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="e0b58-132">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="e0b58-132">.NET Core CLI</span></span>](#tab/netcore-cli)

1. <span data-ttu-id="e0b58-133">Verwenden Sie die Vorlage **Razor-Klassenbibliothek** (`razorclasslib`) mit dem Befehl [`dotnet new`](/dotnet/core/tools/dotnet-new) in einer Befehlsshell.</span><span class="sxs-lookup"><span data-stu-id="e0b58-133">Use the **Razor Class Library** template (`razorclasslib`) with the [`dotnet new`](/dotnet/core/tools/dotnet-new) command in a command shell.</span></span> <span data-ttu-id="e0b58-134">Im folgenden Beispiel wird eine RCL mit dem Namen `MyComponentLib1` erstellt.</span><span class="sxs-lookup"><span data-stu-id="e0b58-134">In the following example, an RCL is created named `MyComponentLib1`.</span></span> <span data-ttu-id="e0b58-135">Der Ordner, der `MyComponentLib1` enthält, wird automatisch erstellt, wenn der Befehl ausgeführt wird:</span><span class="sxs-lookup"><span data-stu-id="e0b58-135">The folder that holds `MyComponentLib1` is created automatically when the command is executed:</span></span>

   ```dotnetcli
   dotnet new razorclasslib -o MyComponentLib1
   ```

   > [!NOTE]
   > <span data-ttu-id="e0b58-136">Wenn der Switch `-s|--support-pages-and-views` beim Erstellen der RCL aus der Vorlage verwendet wird, fügen Sie auch eine `_Imports.razor`-Datei mit folgendem Inhalt zum Stammverzeichnis des generierten Projekts hinzu, um das Erstellen von Razor-Komponenten zu ermöglichen:</span><span class="sxs-lookup"><span data-stu-id="e0b58-136">If the `-s|--support-pages-and-views` switch is used when generating the RCL from the template, then also add an `_Imports.razor` file to root of the generated project with the following contents to enable Razor component authoring:</span></span>
   >
   > ```razor
   > @using Microsoft.AspNetCore.Components.Web
   > ```
   >
   > <span data-ttu-id="e0b58-137">Fügen Sie die Datei manuell dem Stammverzeichnis des generierten Projekts hinzu.</span><span class="sxs-lookup"><span data-stu-id="e0b58-137">Manually add the file the root of the generated project.</span></span>

1. <span data-ttu-id="e0b58-138">Verwenden Sie den Befehl [`dotnet add reference`](/dotnet/core/tools/dotnet-add-reference) in einer Befehlsshell, um die Bibliothek zu einem bestehenden Projekt hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="e0b58-138">To add the library to an existing project, use the [`dotnet add reference`](/dotnet/core/tools/dotnet-add-reference) command in a command shell.</span></span> <span data-ttu-id="e0b58-139">Im folgenden Beispiel wird die RCL der App hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="e0b58-139">In the following example, the RCL is added to the app.</span></span> <span data-ttu-id="e0b58-140">Führen Sie den folgenden Befehl aus dem Projektordner der App mit dem Pfad zur Bibliothek aus:</span><span class="sxs-lookup"><span data-stu-id="e0b58-140">Execute the following command from the app's project folder with the path to the library:</span></span>

   ```dotnetcli
   dotnet add reference {PATH TO LIBRARY}
   ```

---

## <a name="consume-a-library-component"></a><span data-ttu-id="e0b58-141">Verwenden einer Bibliothekskomponente</span><span class="sxs-lookup"><span data-stu-id="e0b58-141">Consume a library component</span></span>

<span data-ttu-id="e0b58-142">Um Komponenten, die in einer Bibliothek definiert sind, in einem anderen Projekt zu verwenden, verwenden Sie einen der folgenden Ansätze:</span><span class="sxs-lookup"><span data-stu-id="e0b58-142">In order to consume components defined in a library in another project, use either of the following approaches:</span></span>

* <span data-ttu-id="e0b58-143">Verwenden Sie den vollständigen Typnamen mit dem Namespace.</span><span class="sxs-lookup"><span data-stu-id="e0b58-143">Use the full type name with the namespace.</span></span>
* <span data-ttu-id="e0b58-144">Verwenden Sie die Razor-Anweisung [`@using`](xref:mvc/views/razor#using).</span><span class="sxs-lookup"><span data-stu-id="e0b58-144">Use Razor's [`@using`](xref:mvc/views/razor#using) directive.</span></span> <span data-ttu-id="e0b58-145">Einzelne Komponenten können über den Namen hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="e0b58-145">Individual components can be added by name.</span></span>

<span data-ttu-id="e0b58-146">In den folgenden Beispielen ist `MyComponentLib1` eine Komponentenbibliothek, die eine `SalesReport`-Komponente enthält.</span><span class="sxs-lookup"><span data-stu-id="e0b58-146">In the following examples, `MyComponentLib1` is a component library containing a `SalesReport` component.</span></span>

<span data-ttu-id="e0b58-147">Die `SalesReport`-Komponente kann über ihren vollständigen Typnamen mit Namespace referenziert werden:</span><span class="sxs-lookup"><span data-stu-id="e0b58-147">The `SalesReport` component can be referenced using its full type name with namespace:</span></span>

```razor
<h1>Hello, world!</h1>

Welcome to your new app.

<MyComponentLib1.SalesReport />
```

<span data-ttu-id="e0b58-148">Auf die Komponente kann auch verwiesen werden, wenn die Bibliothek mit einer `@using`-Direktive in den Gültigkeitsbereich eingebunden wird:</span><span class="sxs-lookup"><span data-stu-id="e0b58-148">The component can also be referenced if the library is brought into scope with an `@using` directive:</span></span>

```razor
@using MyComponentLib1

<h1>Hello, world!</h1>

Welcome to your new app.

<SalesReport />
```

<span data-ttu-id="e0b58-149">Fügen Sie die `@using MyComponentLib1`-Direktive in die `_Import.razor`-Datei der obersten Ebene ein, um die Komponenten der Bibliothek für ein ganzes Projekt zur Verfügung zu stellen.</span><span class="sxs-lookup"><span data-stu-id="e0b58-149">Include the `@using MyComponentLib1` directive in the top-level `_Import.razor` file to make the library's components available to an entire project.</span></span> <span data-ttu-id="e0b58-150">Fügen Sie die Direktive auf beliebiger Ebene zu einer `_Import.razor`-Datei hinzu, um den Namespace auf eine einzelne Seite oder eine Reihe von Seiten innerhalb eines Ordners anzuwenden.</span><span class="sxs-lookup"><span data-stu-id="e0b58-150">Add the directive to an `_Import.razor` file at any level to apply the namespace to a single page or set of pages within a folder.</span></span>

## <a name="create-a-razor-components-class-library-with-static-assets"></a><span data-ttu-id="e0b58-151">Erstellen einer Klassenbibliothek für Razor-Komponenten mit statischen Objekten</span><span class="sxs-lookup"><span data-stu-id="e0b58-151">Create a Razor components class library with static assets</span></span>

<span data-ttu-id="e0b58-152">Eine RCL kann statische Objekte enthalten.</span><span class="sxs-lookup"><span data-stu-id="e0b58-152">An RCL can include static assets.</span></span> <span data-ttu-id="e0b58-153">Die statischen Objekte sind für jede App verfügbar, die die Bibliothek nutzt.</span><span class="sxs-lookup"><span data-stu-id="e0b58-153">The static assets are available to any app that consumes the library.</span></span> <span data-ttu-id="e0b58-154">Weitere Informationen finden Sie unter <xref:razor-pages/ui-class#create-an-rcl-with-static-assets>.</span><span class="sxs-lookup"><span data-stu-id="e0b58-154">For more information, see <xref:razor-pages/ui-class#create-an-rcl-with-static-assets>.</span></span>

## <a name="build-pack-and-ship-to-nuget"></a><span data-ttu-id="e0b58-155">Erstellen, Verpacken und Liefern an NuGet</span><span class="sxs-lookup"><span data-stu-id="e0b58-155">Build, pack, and ship to NuGet</span></span>

<span data-ttu-id="e0b58-156">Da es sich bei Komponentenbibliotheken um standardmäßige .NET-Bibliotheken handelt, unterscheidet sich das Verpacken und Liefern an NuGet nicht vom Verpacken und Liefern einer beliebigen Bibliothek an NuGet.</span><span class="sxs-lookup"><span data-stu-id="e0b58-156">Because component libraries are standard .NET libraries, packaging and shipping them to NuGet is no different from packaging and shipping any library to NuGet.</span></span> <span data-ttu-id="e0b58-157">Das Verpacken wird mit dem Befehl [`dotnet pack`](/dotnet/core/tools/dotnet-pack) in einer Befehlsshell durchgeführt:</span><span class="sxs-lookup"><span data-stu-id="e0b58-157">Packaging is performed using the [`dotnet pack`](/dotnet/core/tools/dotnet-pack) command in a command shell:</span></span>

```dotnetcli
dotnet pack
```

<span data-ttu-id="e0b58-158">Laden Sie das Paket mit dem Befehl [`dotnet nuget push`](/dotnet/core/tools/dotnet-nuget-push) in einer Befehlsshell in NuGet hoch.</span><span class="sxs-lookup"><span data-stu-id="e0b58-158">Upload the package to NuGet using the [`dotnet nuget push`](/dotnet/core/tools/dotnet-nuget-push) command in a command shell.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e0b58-159">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="e0b58-159">Additional resources</span></span>

* <xref:razor-pages/ui-class>
* [<span data-ttu-id="e0b58-160">Hinzufügen einer XML-Linkerkonfigurationsdatei zu einer Bibliothek</span><span class="sxs-lookup"><span data-stu-id="e0b58-160">Add an XML linker configuration file to a library</span></span>](xref:blazor/host-and-deploy/configure-linker#add-an-xml-linker-configuration-file-to-a-library)
