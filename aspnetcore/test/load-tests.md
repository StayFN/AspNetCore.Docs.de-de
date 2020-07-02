---
title: Auslastungs-/Belastungstests in ASP.NET Core
author: Jeremy-Meng
description: Hier erhalten Sie weitere Informationen zu hilfreichen Tools und Ansätzen für Auslastungstests und Belastungstests für ASP.NET Core-Apps.
ms.author: riande
ms.custom: mvc
ms.date: 4/05/2019
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: test/loadtests
ms.openlocfilehash: 5df2dd906d52aaec4fc13b07f3d92c87c802f37f
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/26/2020
ms.locfileid: "85406510"
---
# <a name="aspnet-core-loadstress-testing"></a><span data-ttu-id="29272-103">Auslastungs-/Belastungstests in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="29272-103">ASP.NET Core load/stress testing</span></span>

<span data-ttu-id="29272-104">Auslastungstests und Belastungstests sind wichtig, um sicherzustellen, dass eine Web-App leistungsfähig bleibt und skalierbar ist.</span><span class="sxs-lookup"><span data-stu-id="29272-104">Load testing and stress testing are important to ensure a web app is performant and scalable.</span></span> <span data-ttu-id="29272-105">Ihre Ziele unterscheiden sich, obwohl oftmals ähnliche Tests verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="29272-105">Their goals are different even though they often share similar tests.</span></span>

<span data-ttu-id="29272-106">**Auslastungstests:** Hier wird getestet, ob die App eine bestimmte Benutzerauslastung für ein bestimmtes Szenario verarbeiten kann und dabei weiterhin reaktionsfähig bleibt.</span><span class="sxs-lookup"><span data-stu-id="29272-106">**Load tests**: Test whether the app can handle a specified load of users for a certain scenario while still satisfying the response goal.</span></span> <span data-ttu-id="29272-107">Die App wird dabei unter normalen Umständen ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="29272-107">The app is run under normal conditions.</span></span>

<span data-ttu-id="29272-108">**Belastungstests:** Hier wird die Stabilität der App getestet, wenn sie unter extremen Bedingungen ausgeführt wird, oft auch für einen längeren Zeitraum.</span><span class="sxs-lookup"><span data-stu-id="29272-108">**Stress tests**: Test app stability when running under extreme conditions, often for a long period of time.</span></span> <span data-ttu-id="29272-109">Beim Test wird die App einer hohen Benutzerauslastung ausgesetzt, entweder mit Spitzenwerten oder einer schrittweise ansteigenden Auslastung. Alternativ können die Computingressourcen der App eingeschränkt werden.</span><span class="sxs-lookup"><span data-stu-id="29272-109">The tests place high user load, either spikes or gradually increasing load, on the app, or they limit the app's computing resources.</span></span>

<span data-ttu-id="29272-110">Bei Belastungstests wird untersucht, ob sich eine App unter Belastung nach einem Fehler erholen und ordnungsgemäß zum erwarteten Verhalten zurückkehren kann.</span><span class="sxs-lookup"><span data-stu-id="29272-110">Stress tests determine if an app under stress can recover from failure and gracefully return to expected behavior.</span></span> <span data-ttu-id="29272-111">Unter Belastung wird die App nicht unter normalen Bedingungen ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="29272-111">Under stress, the app isn't run under normal conditions.</span></span>

<span data-ttu-id="29272-112">Es wurden Pläne angekündigt, [den Auslastungstest in Visual Studio 2019 einzustellen](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/).</span><span class="sxs-lookup"><span data-stu-id="29272-112">Visual Studio 2019 announced plans to [deprecate the load testing](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/).</span></span> <span data-ttu-id="29272-113">Der entsprechende cloudbasierte Azure DevOps-Auslastungstestdienst wurde geschlossen.</span><span class="sxs-lookup"><span data-stu-id="29272-113">The corresponding Azure DevOps cloud-based load testing service has been closed.</span></span>

## <a name="third-party-tools"></a><span data-ttu-id="29272-114">Tools von Drittanbietern</span><span class="sxs-lookup"><span data-stu-id="29272-114">Third-party tools</span></span>

<span data-ttu-id="29272-115">In der folgenden Liste finden Sie Webleistungstools von Drittanbietern, die verschiedene Features bieten:</span><span class="sxs-lookup"><span data-stu-id="29272-115">The following list contains third-party web performance tools with various feature sets:</span></span>

* [<span data-ttu-id="29272-116">Apache JMeter</span><span class="sxs-lookup"><span data-stu-id="29272-116">Apache JMeter</span></span>](https://jmeter.apache.org/)
* [<span data-ttu-id="29272-117">ApacheBench (ab)</span><span class="sxs-lookup"><span data-stu-id="29272-117">ApacheBench (ab)</span></span>](https://httpd.apache.org/docs/2.4/programs/ab.html)
* [<span data-ttu-id="29272-118">Gatling</span><span class="sxs-lookup"><span data-stu-id="29272-118">Gatling</span></span>](https://gatling.io/)
* [<span data-ttu-id="29272-119">k6</span><span class="sxs-lookup"><span data-stu-id="29272-119">k6</span></span>](https://k6.io)
* [<span data-ttu-id="29272-120">Locust</span><span class="sxs-lookup"><span data-stu-id="29272-120">Locust</span></span>](https://locust.io/)
* [<span data-ttu-id="29272-121">West Wind WebSurge</span><span class="sxs-lookup"><span data-stu-id="29272-121">West Wind WebSurge</span></span>](https://websurge.west-wind.com/)
* [<span data-ttu-id="29272-122">Netling</span><span class="sxs-lookup"><span data-stu-id="29272-122">Netling</span></span>](https://github.com/hallatore/Netling)
* [<span data-ttu-id="29272-123">Vegeta</span><span class="sxs-lookup"><span data-stu-id="29272-123">Vegeta</span></span>](https://github.com/tsenart/vegeta)