---
title: ASP.NET Core-Web SDK
author: Rick-Anderson
description: Übersicht über Microsoft.NET.Sdk.Web.
ms.author: riande
ms.date: 01/25/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: razor-pages/web-sdk
ms.openlocfilehash: 2d154ebdbcb564ff5174940691b63ecce4154987
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/26/2020
ms.locfileid: "85403728"
---
# <a name="aspnet-core-web-sdk"></a><span data-ttu-id="f6494-103">ASP.NET Core-Web SDK</span><span class="sxs-lookup"><span data-stu-id="f6494-103">ASP.NET Core Web SDK</span></span>

### <a name="overview"></a><span data-ttu-id="f6494-104">Übersicht</span><span class="sxs-lookup"><span data-stu-id="f6494-104">Overview</span></span>

<span data-ttu-id="f6494-105">`Microsoft.NET.Sdk.Web` ist ein [MSBuild-Projekt-SDK](https://docs.microsoft.com/visualstudio/msbuild/how-to-use-project-sdk) zum Erstellen von ASP.NET Core-Apps.</span><span class="sxs-lookup"><span data-stu-id="f6494-105">`Microsoft.NET.Sdk.Web` is an [MSBuild project SDK](https://docs.microsoft.com/visualstudio/msbuild/how-to-use-project-sdk) for building ASP.NET Core apps.</span></span> <span data-ttu-id="f6494-106">Es ist möglich, eine ASP.NET-Core-App ohne dieses SDK zu erstellen, für das Web SDK gilt jedoch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="f6494-106">It's possible to build an ASP.NET Core app without this SDK, however, the Web SDK is:</span></span>

* <span data-ttu-id="f6494-107">Es ist darauf ausgerichtet, ein erstklassiges Erlebnis zu bieten.</span><span class="sxs-lookup"><span data-stu-id="f6494-107">Tailored towards providing a first-class experience.</span></span>
* <span data-ttu-id="f6494-108">Es ist das empfohlene Ziel für die meisten Benutzer.</span><span class="sxs-lookup"><span data-stu-id="f6494-108">The recommended target for most users.</span></span>

<span data-ttu-id="f6494-109">Verwenden Sie das Web.SDK in einem Projekt:</span><span class="sxs-lookup"><span data-stu-id="f6494-109">Use the Web.SDK in a project:</span></span>

  ```xml
  <Project Sdk="Microsoft.NET.Sdk.Web">
    <!-- omitted for brevity -->
  </Project>
  ```

<span data-ttu-id="f6494-110">Features, die durch die Verwendung des Web SDK aktiviert werden:</span><span class="sxs-lookup"><span data-stu-id="f6494-110">Features enabled by using the Web SDK:</span></span>

* <span data-ttu-id="f6494-111">Projekte, die auf .NET Core 3.0 oder höher ausgerichtet sind, verweisen implizit auf:</span><span class="sxs-lookup"><span data-stu-id="f6494-111">Projects targeting .NET Core 3.0 or later implicitly reference:</span></span>

  * <span data-ttu-id="f6494-112">Das [freigegebene ASP.NET Core-Framework](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="f6494-112">The [ASP.NET Core shared framework](xref:fundamentals/metapackage-app).</span></span>
  * <span data-ttu-id="f6494-113">[Analysetools](/visualstudio/extensibility/getting-started-with-roslyn-analyzers), die für die Erstellung von ASP.NET Core-Apps entwickelt wurden.</span><span class="sxs-lookup"><span data-stu-id="f6494-113">[Analyzers](/visualstudio/extensibility/getting-started-with-roslyn-analyzers) designed for building ASP.NET Core apps.</span></span>
* <span data-ttu-id="f6494-114">Das Web SDK importiert MSBuild-Ziele, die die Verwendung von Veröffentlichungsprofilen und die Veröffentlichung mit WebDeploy ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="f6494-114">The Web SDK imports MSBuild targets that enable the use of publish profiles and publishing using WebDeploy.</span></span>

### <a name="properties"></a><span data-ttu-id="f6494-115">Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="f6494-115">Properties</span></span>

| <span data-ttu-id="f6494-116">Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="f6494-116">Property</span></span> | <span data-ttu-id="f6494-117">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="f6494-117">Description</span></span> |
| -------- | ----------- |
| `DisableImplicitFrameworkReferences` | <span data-ttu-id="f6494-118">Deaktiviert den impliziten Verweis auf das freigegebene `Microsoft.AspNetCore.App`-Framework.</span><span class="sxs-lookup"><span data-stu-id="f6494-118">Disables implicit reference to the `Microsoft.AspNetCore.App` shared framework.</span></span> |
| `DisableImplicitAspNetCoreAnalyzers` | <span data-ttu-id="f6494-119">Deaktiviert den impliziten Verweis auf ASP.NET Core-Analysetools.</span><span class="sxs-lookup"><span data-stu-id="f6494-119">Disables implicit reference to ASP.NET Core analyzers.</span></span> |
| `DisableImplicitComponentsAnalyzers` | <span data-ttu-id="f6494-120">Deaktiviert den impliziten Verweis auf Analysetools von Razor-Komponenten beim Erstellen von Blazor-Anwendungen (Server).</span><span class="sxs-lookup"><span data-stu-id="f6494-120">Disables implicit reference to Razor Components analyzers when building Blazor (server) applications.</span></span> |
