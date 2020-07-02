---
title: Verwenden der React-Redux-Projektvorlage mit ASP.NET Core
author: SteveSandersonMS
description: Erfahren Sie, wie Sie sich mit der Projektvorlage für die Einzelseitenanwendung (Single-Page Application, SPA) von ASP.NET Core für React-Redux und create-react-app vertraut machen.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/13/2019
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: spa/react-with-redux
ms.openlocfilehash: d44e1afabd909b7d46d3355a5d66848458416873
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/26/2020
ms.locfileid: "85401739"
---
# <a name="use-the-react-with-redux-project-template-with-aspnet-core"></a>Verwenden der React-Redux-Projektvorlage mit ASP.NET Core

Die aktualisierte React-Redux-Projektvorlage stellt einen geeigneten Anfangspunkt für ASP.NET Core-Apps dar, die React-, Redux- und CRA-Konventionen ([create-react-app](https://github.com/facebookincubator/create-react-app)) für die Implementierung einer umfangreichen, clientseitigen Benutzerschnittstelle (User Interface, UI) verwenden.

Mit Ausnahme des Befehls für die Projekterstellung sind sämtliche Informationen zur React-Redux-Vorlage mit denen zur React-Vorlage identisch. Führen Sie zum Erstellen dieses Projekttyps `dotnet new reactredux` anstelle von `dotnet new react` aus. Weitere Informationen zu den Funktionen beider React-basierten Vorlagen finden Sie in der [Dokumentation zu React-Vorlagen](xref:spa/react).

Informationen zum Konfigurieren einer untergeordneten React-Redux-Anwendung in IIS finden Sie unter [ReactRedux-Vorlage 2.1: SPA konnte nicht unter IIS verwendet werden (aspnet/Templating &num;555)](https://github.com/aspnet/Templating/issues/555).
