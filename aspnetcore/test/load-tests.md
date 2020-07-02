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
# <a name="aspnet-core-loadstress-testing"></a>Auslastungs-/Belastungstests in ASP.NET Core

Auslastungstests und Belastungstests sind wichtig, um sicherzustellen, dass eine Web-App leistungsfähig bleibt und skalierbar ist. Ihre Ziele unterscheiden sich, obwohl oftmals ähnliche Tests verwendet werden.

**Auslastungstests:** Hier wird getestet, ob die App eine bestimmte Benutzerauslastung für ein bestimmtes Szenario verarbeiten kann und dabei weiterhin reaktionsfähig bleibt. Die App wird dabei unter normalen Umständen ausgeführt.

**Belastungstests:** Hier wird die Stabilität der App getestet, wenn sie unter extremen Bedingungen ausgeführt wird, oft auch für einen längeren Zeitraum. Beim Test wird die App einer hohen Benutzerauslastung ausgesetzt, entweder mit Spitzenwerten oder einer schrittweise ansteigenden Auslastung. Alternativ können die Computingressourcen der App eingeschränkt werden.

Bei Belastungstests wird untersucht, ob sich eine App unter Belastung nach einem Fehler erholen und ordnungsgemäß zum erwarteten Verhalten zurückkehren kann. Unter Belastung wird die App nicht unter normalen Bedingungen ausgeführt.

Es wurden Pläne angekündigt, [den Auslastungstest in Visual Studio 2019 einzustellen](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/). Der entsprechende cloudbasierte Azure DevOps-Auslastungstestdienst wurde geschlossen.

## <a name="third-party-tools"></a>Tools von Drittanbietern

In der folgenden Liste finden Sie Webleistungstools von Drittanbietern, die verschiedene Features bieten:

* [Apache JMeter](https://jmeter.apache.org/)
* [ApacheBench (ab)](https://httpd.apache.org/docs/2.4/programs/ab.html)
* [Gatling](https://gatling.io/)
* [k6](https://k6.io)
* [Locust](https://locust.io/)
* [West Wind WebSurge](https://websurge.west-wind.com/)
* [Netling](https://github.com/hallatore/Netling)
* [Vegeta](https://github.com/tsenart/vegeta)