---
title: Sonstige ASP.net Core Datenschutz-APIs
author: rick-anderson
description: Erfahren Sie mehr über die isecret-Schnittstelle für ASP.net Core Datenschutz.
ms.author: riande
ms.date: 10/14/2016
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 2e319cdcec1e005682555c4e03c52632e6d8521a
ms.sourcegitcommit: b0fa7ff0cb158277df61bcd08058a81222c3fe10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/07/2020
ms.locfileid: "87913806"
---
# <a name="miscellaneous-aspnet-core-data-protection-apis"></a>Sonstige ASP.net Core Datenschutz-APIs

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> Typen, die eine der folgenden Schnittstellen implementieren, sollten für mehrere Aufrufer Thread sicher sein.

## <a name="isecret"></a>Isecret

Die- `ISecret` Schnittstelle stellt einen geheimen Wert dar, z. b. Material für kryptografische Schlüssel. Es enthält die folgende API-Oberfläche:

* `Length`: `int`

* `Dispose()`: `void`

* `WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`

Die- `WriteSecretIntoBuffer` Methode füllt den angegebenen Puffer mit dem unformatierten geheimen Wert auf. Der Grund, warum diese API den Puffer als Parameter annimmt, anstatt einen `byte[]` direkt zurückzugeben, besteht darin, dass der Aufrufer die Möglichkeit erhält, das Puffer Objekt anzuheften, wodurch die geheime Gefährdung des verwalteten Garbage Collector beschränkt wird.

Der- `Secret` Typ ist eine konkrete Implementierung von, `ISecret` bei der der geheime Wert im Prozess internen Speicher gespeichert wird. Auf Windows-Plattformen wird der geheime Wert über " [cryptprotectmemory](/windows/win32/api/dpapi/nf-dpapi-cryptprotectmemory)" verschlüsselt.
