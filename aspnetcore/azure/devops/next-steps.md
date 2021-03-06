---
title: Nächste Schritte – DevOps mit ASP.NET Core und Azure
author: CamSoper
description: Weitere Lernpfade für DevOps mit ASP.NET Core und Azure.
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 10/24/2018
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: azure/devops/next-steps
ms.openlocfilehash: 6e7f46a0c78575e52a8182829f7b3d2bddec70f6
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/26/2020
ms.locfileid: "85400335"
---
# <a name="next-steps"></a>Nächste Schritte

In diesem Handbuch haben Sie eine DevOps-Pipeline für eine ASP.NET Core-Beispiel-App erstellt. Herzlichen Glückwunsch! Wir hoffen, Sie hatten Spaß zu Erfahren, wie ASP.NET Core-Web-Apps in Azure App Service veröffentlicht werden und wie die kontinuierliche Integration von Änderungen automatisiert werden kann.

Über das Webhosting und DevOps hinaus bietet Azure eine breite Palette von PaaS-Diensten (Platform-as-a-Service), die für ASP.NET Core-Entwickler nützlich sind. Dieser Abschnitt enthält eine kurze Übersicht über einige der am häufigsten verwendeten Dienste.

## <a name="storage-and-databases"></a>Speicher und Datenbanken

[Redis Cache](/azure/redis-cache/) ist ein Zwischenspeicher für Daten mit hohem Durchsatz und niedriger Wartezeit, der als Dienst zur Verfügung steht. Er kann für die Zwischenspeicherung der Seitenausgabe, die Reduzierung von Datenbankanforderungen und die Bereitstellung des ASP.NET Core-Sitzungszustands über mehrere Instanzen einer App verwendet werden.

[Azure Storage](/azure/storage/) ist der extrem skalierbare Cloudspeicher von Azure. Entwickler können die Vorteile von [Warteschlangenspeicher](/azure/storage/queues/storage-queues-introduction) für zuverlässiges Message Queuing nutzen, und [Tabellenspeicher](/azure/storage/tables/table-storage-overview) ist ein NoSQL-Schlüsselwertspeicher, der für die schnelle Entwicklung mit extremen, teilweise strukturierten Datasets konzipiert ist.

[Azure SQL-Datenbank](/azure/sql-database/) bietet bekannte relationale Datenbankfunktionen als Dienst unter Verwendung der Microsoft SQL Server Engine.

[Cosmos DB](/azure/cosmos-db/) ist ein global verteilter NoSQL-Datenbankdienst mit mehreren Modellen. Es stehen mehrere APIs zur Verfügung, darunter SQL API (früher DocumentDB), Cassandra und MongoDB.

## Identity

[Azure Active Directory](/azure/active-directory/) und [Azure Active Directory B2C](/azure/active-directory-b2c/) sind beides Identitätsdienste. Azure Active Directory ist für Unternehmensszenarien konzipiert und ermöglicht die Zusammenarbeit mit Azure AD B2B (Business-to-Business), während Azure Active Directory B2C für Business-to-Customer-Szenarien vorgesehen ist, einschließlich der Anmeldung in sozialen Netzwerken.

## <a name="mobile"></a>Mobil

[Notification Hubs](/azure/notification-hubs/) ist eine skalierbare Multiplattform-Engine für Pushbenachrichtigungen, um schnell Millionen von Nachrichten an Apps auf verschiedenen Geräten zu senden.

## <a name="web-infrastructure"></a>Webinfrastruktur

[Azure Container Service](/azure/aks/) verwaltet Ihre gehostete Kubernetes-Umgebung und ermöglicht so die schnelle und einfache Bereitstellung und Verwaltung von Container-Apps – ganz ohne Kenntnisse in Sachen Containerorchestrierung.

[Azure Search](/azure/search/) wird verwendet, um eine Unternehmenslösung für die Suche über private, heterogene Inhalte zu erstellen.

[Service Fabric](/azure/service-fabric/) ist eine Plattform für verteilte Systeme, die das Packen, Bereitstellen und Verwalten skalierbarer und zuverlässiger Microservices und Container vereinfacht.
