---
title: Unterstützte Plattformen für ASP.NET Core Blazor
author: guardrex
description: Erfahren Sie mehr über die unterstützten Plattformen für ASP.NET Core Blazor.
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
uid: blazor/supported-platforms
ms.openlocfilehash: adf27fa84acb3929a1639b561c728c2db29723f6
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/26/2020
ms.locfileid: "85401934"
---
# <a name="aspnet-core-blazor-supported-platforms"></a><span data-ttu-id="8546f-103">Unterstützte Plattformen für ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="8546f-103">ASP.NET Core Blazor supported platforms</span></span>

<span data-ttu-id="8546f-104">Von [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="8546f-104">By [Luke Latham](https://github.com/guardrex)</span></span>

## <a name="browser-requirements"></a><span data-ttu-id="8546f-105">Browseranforderungen</span><span class="sxs-lookup"><span data-stu-id="8546f-105">Browser requirements</span></span>

### Blazor WebAssembly

| <span data-ttu-id="8546f-106">Browser</span><span class="sxs-lookup"><span data-stu-id="8546f-106">Browser</span></span>                          | <span data-ttu-id="8546f-107">Version</span><span class="sxs-lookup"><span data-stu-id="8546f-107">Version</span></span>               |
| -------------------------------- | :-------------------: |
| <span data-ttu-id="8546f-108">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="8546f-108">Microsoft Edge</span></span>                   | <span data-ttu-id="8546f-109">Aktuell</span><span class="sxs-lookup"><span data-stu-id="8546f-109">Current</span></span>               |
| <span data-ttu-id="8546f-110">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="8546f-110">Mozilla Firefox</span></span>                  | <span data-ttu-id="8546f-111">Aktuell</span><span class="sxs-lookup"><span data-stu-id="8546f-111">Current</span></span>               |
| <span data-ttu-id="8546f-112">Google Chrome, einschließlich Android</span><span class="sxs-lookup"><span data-stu-id="8546f-112">Google Chrome, including Android</span></span> | <span data-ttu-id="8546f-113">Aktuell</span><span class="sxs-lookup"><span data-stu-id="8546f-113">Current</span></span>               |
| <span data-ttu-id="8546f-114">Safari, einschließlich iOS</span><span class="sxs-lookup"><span data-stu-id="8546f-114">Safari, including iOS</span></span>            | <span data-ttu-id="8546f-115">Aktuell</span><span class="sxs-lookup"><span data-stu-id="8546f-115">Current</span></span>               |
| <span data-ttu-id="8546f-116">Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="8546f-116">Microsoft Internet Explorer</span></span>      | <span data-ttu-id="8546f-117">Nicht unterstützt&dagger;</span><span class="sxs-lookup"><span data-stu-id="8546f-117">Not Supported&dagger;</span></span> |

<span data-ttu-id="8546f-118">&dagger;Microsoft Internet Explorer unterstützt [WebAssembly](https://webassembly.org) nicht.</span><span class="sxs-lookup"><span data-stu-id="8546f-118">&dagger;Microsoft Internet Explorer doesn't support [WebAssembly](https://webassembly.org).</span></span>

### Blazor Server

| <span data-ttu-id="8546f-119">Browser</span><span class="sxs-lookup"><span data-stu-id="8546f-119">Browser</span></span>                          | <span data-ttu-id="8546f-120">Version</span><span class="sxs-lookup"><span data-stu-id="8546f-120">Version</span></span>    |
| -------------------------------- | :--------: |
| <span data-ttu-id="8546f-121">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="8546f-121">Microsoft Edge</span></span>                   | <span data-ttu-id="8546f-122">Aktuell</span><span class="sxs-lookup"><span data-stu-id="8546f-122">Current</span></span>    |
| <span data-ttu-id="8546f-123">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="8546f-123">Mozilla Firefox</span></span>                  | <span data-ttu-id="8546f-124">Aktuell</span><span class="sxs-lookup"><span data-stu-id="8546f-124">Current</span></span>    |
| <span data-ttu-id="8546f-125">Google Chrome, einschließlich Android</span><span class="sxs-lookup"><span data-stu-id="8546f-125">Google Chrome, including Android</span></span> | <span data-ttu-id="8546f-126">Aktuell</span><span class="sxs-lookup"><span data-stu-id="8546f-126">Current</span></span>    |
| <span data-ttu-id="8546f-127">Safari, einschließlich iOS</span><span class="sxs-lookup"><span data-stu-id="8546f-127">Safari, including iOS</span></span>            | <span data-ttu-id="8546f-128">Aktuell</span><span class="sxs-lookup"><span data-stu-id="8546f-128">Current</span></span>    |
| <span data-ttu-id="8546f-129">Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="8546f-129">Microsoft Internet Explorer</span></span>      | <span data-ttu-id="8546f-130">11&dagger;</span><span class="sxs-lookup"><span data-stu-id="8546f-130">11&dagger;</span></span> |

<span data-ttu-id="8546f-131">&dagger;Zusätzliche Polyfills sind erforderlich (z. B. können Zusagen über ein [`Polyfill.io`](https://polyfill.io/v3/)-Bündel hinzugefügt werden).</span><span class="sxs-lookup"><span data-stu-id="8546f-131">&dagger;Additional polyfills are required (for example, promises can be added via a [`Polyfill.io`](https://polyfill.io/v3/) bundle).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8546f-132">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="8546f-132">Additional resources</span></span>

* <xref:blazor/hosting-models>
