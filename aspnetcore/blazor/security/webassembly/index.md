---
title: Schützen der Blazor WebAssembly von ASP.NET Core
author: guardrex
description: Erfahren Sie, wie Sie Blazor-WebAssembly-Apps als Single-Page-Anwendungen (SPAs) sichern.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/16/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/security/webassembly/index
ms.openlocfilehash: fbb3f6d254e6d294edc7af59d7980a1d67e4a801
ms.sourcegitcommit: d9ae1f352d372a20534b57e23646c1a1d9171af1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/21/2020
ms.locfileid: "86568807"
---
# <a name="secure-aspnet-core-blazor-webassembly"></a>Schützen der Blazor WebAssembly von ASP.NET Core

Von [Javier Calvarro Nelson](https://github.com/javiercn)

Blazor WebAssembly-Apps werden auf die gleiche Weise gesichert wie Single-Page-Anwendungen (SPAs). Es gibt mehrere Ansätze für die Authentifizierung von Benutzern bei SPAs, der gängigste und umfassendste Ansatz besteht jedoch darin, eine auf dem [OAuth 2.0-Protokoll](https://oauth.net/) basierende Implementierung wie [Open ID Connect (OIDC)](https://openid.net/connect/) zu verwenden.

## <a name="authentication-library"></a>Authentifizierungsbibliothek

Blazor WebAssembly unterstützt die Authentifizierung und Autorisierung von Apps mit OIDC über die [`Microsoft.AspNetCore.Components.WebAssembly.Authentication`](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Authentication/)-Bibliothek. Die Bibliothek stellt mehrere Primitive für die nahtlose Authentifizierung für ASP.NET Core-Back-Ends bereit. Die Bibliothek integriert ASP.NET Core Identity mit der API-Authentifizierungsunterstützung, die auf [Identity Server](https://identityserver.io/) basiert. Die Bibliothek kann bei einem Drittanbieter-Identitätsanbieter authentifiziert werden, der OIDC unterstützt (die OpenID-Anbieter).

Die Authentifizierungsunterstützung in Blazor WebAssembly basiert auf der `oidc-client.js`-Bibliothek, die für die Verarbeitung der zugrunde liegenden Informationen im Authentifizierungsprotokoll verwendet wird.

Eine weitere Option zum Authentifizieren von SPAs ist die Verwendung von SameSite-Cookies. Das Entwurfsdesign von Blazor WebAssembly verwendet jedoch OAuth und OIDC als bewährte Option für die Authentifizierung bei Blazor WebAssembly-Apps. Die auf [JSON Web Token](https://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) gestützte [tokenbasierte Authentifizierung](xref:security/anti-request-forgery#token-based-authentication) wurde aus Funktions- und Sicherheitsgründen der [cookiebasierten Authentifizierung](xref:security/anti-request-forgery#cookie-based-authentication) vorgezogen.

* Ein tokenbasiertes Protokoll bietet eine kleinere Angriffsfläche, da die Token nicht in allen Anforderungen gesendet werden.
* Serverendpunkte erfordern keinen Schutz gegenüber [websiteübergreifender Anforderungsfälschung (CSRF, Cross-Site Request Forgery)](xref:security/anti-request-forgery), da die Token explizit gesendet werden. Dadurch können Sie Blazor WebAssembly-Apps zusammen mit MVC- oder Razor Pages-Apps hosten.
* Token haben eingeschränktere Berechtigungen als Cookies. Beispielsweise können Token nicht zum Verwalten des Benutzerkontos oder zur Änderung eines Benutzerkennworts verwendet werden, es sei denn, diese Funktionalität wird explizit implementiert.
* Token haben eine kürzere Lebensdauer, standardmäßig eine Stunde, wodurch sich das Angriffsfenster verkleinert. Token können auch zu jeder Zeit widerrufen werden.
* Eigenständige JWTs bieten dem Client und dem Server Garantien zum Authentifizierungsprozess. Ein Client verfügt zum Beispiel über die Mittel, um zu erkennen und zu überprüfen, dass die von ihm empfangenen Token legitim sind und im Rahmen eines bestimmten Authentifizierungsprozesses ausgegeben wurden. Wenn Dritte versuchen, ein Token inmitten des Authentifizierungsprozesses zu tauschen, kann der Client das ausgetauschte Token erkennen und so die Nutzung vermeiden.
* Token mit OAuth und OIDC sind nicht vom ordnungsgemäßen Verhalten des Benutzer-Agents und davon abhängig, dass dieser gewährleistet, dass die App sicher ist.
* Tokenbasierte Protokolle wie OAuth und OIDC lassen die Authentifizierung und Autorisierung gehosteter und eigenständiger Apps mit denselben Sicherheitsmerkmalen zu.

## <a name="authentication-process-with-oidc"></a>Authentifizierungsprozess mit OIDC

Die [`Microsoft.AspNetCore.Components.WebAssembly.Authentication`](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Authentication/)-Bibliothek bietet mehrere Primitive zum Implementieren der Authentifizierung und Autorisierung mithilfe von OIDC. Im Allgemeinen funktioniert die Authentifizierung wie folgt:

* Wenn ein anonymer Benutzer auf die Anmeldeschaltfläche klickt oder eine Seite mit angewendetem [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute)-Attribut anfordert, wird der Benutzer zur Anmeldeseite (`/authentication/login`) der App weitergeleitet.
* Auf der Anmeldeseite bereitet die Authentifizierungsbibliothek eine Umleitung zum Autorisierungsendpunkt vor. Der Autorisierungsendpunkt befindet sich außerhalb der Blazor WebAssembly-App und kann als separater Ursprung gehostet werden. Der Endpunkt ist dafür verantwortlich, zu bestimmen, ob der Benutzer authentifiziert ist, und dass ein oder mehrere Token als Antwort ausgegeben werden. Die Authentifizierungsbibliothek stellt einen Anmelderückruf zum Empfangen der Authentifizierungsantwort bereit.
  * Wenn der Benutzer nicht authentifiziert ist, wird er an das zugrunde liegende Authentifizierungssystem umgeleitet (normalerweise ASP.NET Core Identity).
  * Wenn der Benutzer bereits authentifiziert wurde, generiert der Authentifizierungsendpunkt die entsprechenden Token und leitet den Browser zurück an den Anmelderückrufendpunkt (`/authentication/login-callback`).
* Wenn die Blazor WebAssembly-App den Anmelderückrufendpunkt (`/authentication/login-callback`) lädt, wird die Authentifizierungsantwort verarbeitet.
  * Sobald der Authentifizierungsprozess erfolgreich abgeschlossen wird, wird der Benutzer authentifiziert und optional an die ursprüngliche geschützte URL weitergeleitet, die vom Benutzer angefordert wurde.
  * Sollte der Authentifizierungsprozess aus irgend einem Grund fehlschlagen, wird der Benutzer auf die Seite „Fehler bei der Anmeldung“ (`/authentication/login-failed`) weitergeleitet, und ein Fehler wird angezeigt.

## <a name="authentication-component"></a>`Authentication`-Komponente

Die `Authentication`-Komponente (`Pages/Authentication.razor`) behandelt Remote-Authentifizierungsvorgänge und ermöglicht der App Folgendes:

* Konfigurieren von App-Routen für Authentifizierungsstatus.
* Festlegen von Inhalten der Benutzeroberfläche für Authentifizierungsstatus.
* Verwalten des Authentifizierungsstatus.

Authentifizierungsaktionen, wie etwa das Registrieren oder Anmelden eines Benutzers, werden der <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.RemoteAuthenticatorViewCore%601>-Komponente des Blazor-Frameworks übergeben, die für die übergreifende Dauerhaftigkeit und die Steuerung des Status über Authentifizierungsvorgänge hinweg zuständig ist.

Weitere Informationen und Beispiele finden Sie unter <xref:blazor/security/webassembly/additional-scenarios>.

## <a name="authorization"></a>Autorisierung

In Blazor WebAssembly-Apps können Autorisierungsprüfungen umgangen werden, da der gesamte clientseitige Code von Benutzern geändert werden kann. Dasselbe gilt für alle clientseitigen App-Technologien, einschließlich JavaScript SPA-Frameworks oder native Apps für jedes Betriebssystem.

**Führen Sie Autorisierungsprüfungen auf dem Server immer innerhalb aller API-Endpunkte durch, auf die Ihre clientseitige App zugreift.**

## <a name="require-authorization-for-the-entire-app"></a>Autorisierung für die gesamte App erforderlich

Wenden Sie die [`[Authorize]`-Attribute](xref:blazor/security/index#authorize-attribute) ([API-Dokumentation](xref:System.Web.Mvc.AuthorizeAttribute)) auf jede Razor-Komponente der App an, indem Sie einen der folgenden Ansätze verwenden:

* Verwenden Sie die [`@attribute`](xref:mvc/views/razor#attribute)-Direktive in der `_Imports.razor`-Datei:

  ```razor
  @using Microsoft.AspNetCore.Authorization
  @attribute [Authorize]
  ```

* Fügen Sie jeder Razor-Komponente im Ordner das `Pages`-Attribut hinzu.

> [!NOTE]
> Das Festlegen einer <xref:Microsoft.AspNetCore.Authorization.AuthorizationOptions.FallbackPolicy?displayProperty=nameWithType> auf eine Richtlinie mit <xref:Microsoft.AspNetCore.Authorization.AuthorizationPolicyBuilder.RequireAuthenticatedUser%2A> wird **nicht** unterstützt.

## <a name="refresh-tokens"></a>Aktualisierungstoken

Aktualisierungstoken können nicht auf Clientseite in Blazor WebAssembly-Apps gesichert werden. Daher sollten Aktualisierungstoken nicht zur direkten Verwendung an die App gesendet werden.

Aktualisierungstoken können von der serverseitigen App in einer gehosteten Blazor WebAssembly-Lösung verwaltet und verwendet werden, um auf APIs von Drittanbietern zuzugreifen. Weitere Informationen finden Sie unter <xref:blazor/security/webassembly/additional-scenarios#authenticate-users-with-a-third-party-provider-and-call-protected-apis-on-the-host-server-and-the-third-party>.

## <a name="implementation-guidance"></a>Implementierungsleitfaden

Artikel unter dieser *Übersicht* bieten Informationen zum Authentifizieren von Benutzern in Blazor WebAssembly-Apps für bestimmte Anbieter.

Eigenständige Blazor WebAssembly-Apps:

* [Allgemeiner Leitfaden für OIDC-Anbieter und die WebAssembly-Authentifizierungsbibliothek](xref:blazor/security/webassembly/standalone-with-authentication-library)
* [Microsoft-Konten](xref:blazor/security/webassembly/standalone-with-microsoft-accounts)
* [Azure Active Directory (AAD)](xref:blazor/security/webassembly/standalone-with-azure-active-directory)
* [Azure Active Directory (AAD) B2C](xref:blazor/security/webassembly/standalone-with-azure-active-directory-b2c)

Gehostete Blazor WebAssembly-Apps:

* [Azure Active Directory (AAD)](xref:blazor/security/webassembly/hosted-with-azure-active-directory)
* [Azure Active Directory (AAD) B2C](xref:blazor/security/webassembly/hosted-with-azure-active-directory-b2c)
* [Identity Server](xref:blazor/security/webassembly/hosted-with-identity-server)

Weitere Anleitungen zur Konfiguration finden Sie unter <xref:blazor/security/webassembly/additional-scenarios>.
