---
title: Konfigurieren der Windows-Authentifizierung in ASP.net Core
author: scottaddie
description: Erfahren Sie, wie Sie die Windows-Authentifizierung in ASP.net Core für IIS und HTTP.sys konfigurieren.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 02/26/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/authentication/windowsauth
ms.openlocfilehash: 8f6dc8620df04bcebe996119869ca2e498cffccc
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/26/2020
ms.locfileid: "87330682"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a>Konfigurieren der Windows-Authentifizierung in ASP.net Core

Von [Scott Addie](https://twitter.com/Scott_Addie)

::: moniker range=">= aspnetcore-3.0"

Die Windows-Authentifizierung (auch als Aushandlung, Kerberos oder NTLM-Authentifizierung bezeichnet) kann für ASP.net Core apps konfiguriert werden, die mit [IIS](xref:host-and-deploy/iis/index), [Kestrel](xref:fundamentals/servers/kestrel)oder [HTTP.sys](xref:fundamentals/servers/httpsys)gehostet werden.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Die Windows-Authentifizierung (auch als Aushandlung, Kerberos oder NTLM-Authentifizierung bezeichnet) kann für ASP.net Core apps konfiguriert werden, die mit [IIS](xref:host-and-deploy/iis/index) oder [HTTP.sys](xref:fundamentals/servers/httpsys)gehostet werden.

::: moniker-end

Die Windows-Authentifizierung basiert auf dem Betriebssystem, um Benutzer von ASP.net Core-apps zu authentifizieren. Sie können die Windows-Authentifizierung verwenden, wenn der Server in einem Unternehmensnetzwerk mit Active Directory Domänen Identitäten oder Windows-Konten ausgeführt wird, um Benutzer zu identifizieren. Die Windows-Authentifizierung eignet sich am besten für Intranetumgebungen, in denen Benutzer, Client-apps und Webserver derselben Windows-Domäne angehören.

> [!NOTE]
> Die Windows-Authentifizierung wird mit http/2 nicht unterstützt. Authentifizierungs Herausforderungen können bei http/2-Antworten gesendet werden, aber der Client muss vor der Authentifizierung auf HTTP/1.1 herabgestuft werden.

## <a name="proxy-and-load-balancer-scenarios"></a>Proxy-und Load Balancer-Szenarios

Die Windows-Authentifizierung ist ein Zustands behaftetes Szenario, das in erster Linie in einem Intranet verwendet wird, in dem ein Proxy oder ein Load Balancer den Datenverkehr zwischen Clients und Servern nicht Wenn ein Proxy oder ein Lasten Ausgleichs Modul verwendet wird, funktioniert die Windows-Authentifizierung nur, wenn der Proxy oder der Load Balancer:

* Behandelt die-Authentifizierung.
* Übergibt die Benutzer Authentifizierungsinformationen an die APP (z. b. in einem Anforderungs Header), die die Authentifizierungsinformationen verarbeitet.

Eine Alternative zur Windows-Authentifizierung in Umgebungen, in denen Proxys und Lasten Ausgleichs Module verwendet werden, ist Active Directory Verbund Dienste (AD FS) mit OpenID Connect (oidc).

## <a name="iisiis-express"></a>IIS/IIS Express

Fügen Sie Authentifizierungsdienste hinzu, indem Sie <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> ( <xref:Microsoft.AspNetCore.Server.IISIntegration?displayProperty=fullName> Namespace) in Aufrufen `Startup.ConfigureServices` :

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

### <a name="launch-settings-debugger"></a>Start Einstellungen (Debugger)

Die Konfiguration für Start Einstellungen wirkt sich nur *auf die Eigenschaften/launchSettings.jsin* der Datei für IIS Express aus und konfiguriert IIS nicht für die Windows-Authentifizierung. Die Server Konfiguration wird im [IIS](#iis) -Abschnitt erläutert.

Die **Webanwendungs** Vorlage, die über Visual Studio oder die .net Core-CLI verfügbar ist, kann so konfiguriert werden, dass die Windows *#b0-* Authentifizierung unterstützt wird

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

**Neues Projekt**

1. Erstellen Sie ein neues Projekt.
1. Wählen Sie **ASP.NET Core-Webanwendung** aus. Klicken Sie auf **Weiter**.
1. Geben Sie einen Namen im Feld **Projektname** an. Vergewissern Sie sich, dass der Eintrag für den **Speicherort** korrekt ist, oder geben Sie einen Speicherort für das Projekt an. Wählen Sie **Erstellen** aus.
1. Wählen Sie unter **Authentifizierung**die Option **ändern** .
1. Wählen Sie im Fenster **Authentifizierung ändern** die Option **Windows-Authentifizierung**aus. Klicken Sie auf **OK**.
1. Wählen Sie **Webanwendung** aus.
1. Klicken Sie auf **Erstellen**.

Führen Sie die App aus. Der Benutzername wird in der Benutzeroberfläche der gerenderten App angezeigt.

**Vorhandenes Projekt**

Die Projekteigenschaften aktivieren die Windows-Authentifizierung und deaktivieren die anonyme Authentifizierung:

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und wählen Sie **Eigenschaften** aus.
1. Klicken Sie auf die Registerkarte **Debuggen**.
1. Deaktivieren Sie das Kontrollkästchen **anonyme Authentifizierung aktivieren**.
1. Aktivieren Sie das Kontrollkästchen **Windows-Authentifizierung aktivieren**.
1. Speichern und schließen Sie die Eigenschaften Seite.

Alternativ können die Eigenschaften im `iisSettings` Knoten des *launchSettings.jsin* der Datei konfiguriert werden:

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

# <a name="net-core-cli"></a>[.NET Core-CLI](#tab/netcore-cli)

**Neues Projekt**

Führen Sie den [dotnet New](/dotnet/core/tools/dotnet-new) -Befehl mit dem `webapp` Argument (ASP.net Core Web-App) und `--auth Windows` Switch aus:

```dotnetcli
dotnet new webapp --auth Windows
```

**Vorhandenes Projekt**

Aktualisieren Sie den `iisSettings` Knoten des *launchSettings.jsin* der Datei:

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

---

Wenn Sie ein vorhandenes Projekt ändern, vergewissern Sie sich, dass die Projektdatei einen Paket Verweis für das [Metapaket Microsoft. aspnetcore. app](xref:fundamentals/metapackage-app) **oder** das nuget-Paket [Microsoft. aspnetcore. Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) enthält.

### <a name="iis"></a>IIS

IIS verwendet das [ASP.net Core-Modul](xref:host-and-deploy/aspnet-core-module) , um ASP.net Core-apps zu hosten. Die Windows-Authentifizierung wird für IIS über die *web.config* -Datei konfiguriert. In den folgenden Abschnitten werden diese Aktionen gezeigt:

* Geben Sie eine lokale *web.config* Datei an, die die Windows-Authentifizierung auf dem Server aktiviert, wenn die APP bereitgestellt wird.
* Verwenden Sie den IIS-Manager, um die *web.config* Datei einer ASP.net Core-APP zu konfigurieren, die bereits auf dem-Server bereitgestellt wurde.

Wenn Sie dies nicht bereits getan haben, aktivieren Sie IIS, um ASP.net Core-apps zu hosten. Weitere Informationen finden Sie unter <xref:host-and-deploy/iis/index>.

Aktivieren Sie den IIS-Rollen Dienst für die Windows-Authentifizierung. Weitere Informationen finden Sie unter [Aktivieren der Windows-Authentifizierung in IIS-Rollen Diensten (siehe Schritt 2)](xref:host-and-deploy/iis/index#iis-configuration).

Die [Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) für die IIS-Integration ist so konfiguriert, dass Anforderungen automatisch automatisch authentifiziert werden. Weitere Informationen finden Sie unter [Hosten von ASP.net Core unter Windows mit IIS: IIS-Optionen (automaticauthentication)](xref:host-and-deploy/iis/index#iis-options).

Das ASP.net Core-Modul ist so konfiguriert, dass das Windows-Authentifizierungs Token standardmäßig an die APP weiterleiten wird. Weitere Informationen finden Sie unter [ASP.net Core Modul Konfigurations Referenz: Attribute des aspnetcore-Elements](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).

Verwenden **Sie** einen der folgenden Ansätze:

* Fügen Sie dem Projektstamm **vor dem Veröffentlichen und Bereitstellen des Projekts** die folgende *web.config* Datei hinzu:

  [!code-xml[](windowsauth/sample_snapshot/web_2.config)]

  Wenn das Projekt vom .net Core SDK veröffentlicht wird (ohne dass die- `<IsTransformWebConfigDisabled>` Eigenschaft `true` in der Projektdatei auf festgelegt ist), enthält die veröffentlichte *web.config* Datei den `<location><system.webServer><security><authentication>` Abschnitt. Weitere Informationen zur- `<IsTransformWebConfigDisabled>` Eigenschaft finden Sie unter <xref:host-and-deploy/iis/index#webconfig-file> .

* **Nachdem Sie das Projekt veröffentlicht und bereitgestellt haben,** führen Sie die serverseitige Konfiguration mit dem IIS-Manager aus:

  1. Wählen Sie im IIS-Manager die IIS-Website unter dem Knoten **Sites** der Rand Leiste **Verbindungen** aus.
  1. Doppelklicken Sie im **IIS** -Bereich auf **Authentifizierung** .
  1. Wählen Sie **anonyme Authentifizierung**aus. Wählen Sie in der Rand Leiste **Aktionen** die Option **Deaktivieren** aus.
  1. Wählen Sie **Windows-Authentifizierung** aus. Wählen Sie in der Rand Leiste **Aktionen** die Option **aktivieren** aus.

  Wenn diese Aktionen ausgeführt werden, ändert der IIS-Manager die *web.config* Datei der app. Ein `<system.webServer><security><authentication>` Knoten mit den aktualisierten Einstellungen für und wurde hinzugefügt `anonymousAuthentication` `windowsAuthentication` :

  [!code-xml[](windowsauth/sample_snapshot/web_1.config?highlight=4-5)]

  Der `<system.webServer>` Abschnitt, der vom IIS-Manager zum *web.config* -Datei hinzugefügt wird, befindet sich außerhalb des App-Abschnitts, der `<location>` durch den .net Core SDK bei der Veröffentlichung der app hinzugefügt wurde Da der Abschnitt außerhalb des Knotens hinzugefügt wird `<location>` , werden die Einstellungen von allen [untergeordneten apps](xref:host-and-deploy/iis/index#sub-applications) der aktuellen App geerbt. Um die Vererbung zu verhindern, verschieben Sie den hinzugefügten `<security>` Abschnitt innerhalb des `<location><system.webServer>` Abschnitts, den der .net Core SDK bereitgestellt hat

  Wenn IIS-Manager zum Hinzufügen der IIS-Konfiguration verwendet wird, wirkt sich dies nur auf die *web.config* Datei der APP auf dem Server aus. Eine nachfolgende Bereitstellung der APP kann die Einstellungen auf dem Server überschreiben, wenn die Kopie von *web.config* des Servers durch die *web.config* Datei des Projekts ersetzt wird. Verwenden **Sie** einen der folgenden Ansätze, um die Einstellungen zu verwalten:

  * Verwenden Sie IIS Manager, um die Einstellungen in der *web.config* -Datei zurückzusetzen, nachdem die Datei bei der Bereitstellung überschrieben wurde.
  * Fügen Sie der App eine *web.config Datei* lokal mit den Einstellungen hinzu.

::: moniker range=">= aspnetcore-3.0"

## <a name="kestrel"></a>Kestrel

Das nuget-Paket [Microsoft. aspnetcore. Authentication. Aushandlungs](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) Paket kann mit [Kestrel](xref:fundamentals/servers/kestrel) verwendet werden, um die Windows-Authentifizierung mithilfe von Aushandlungs-und Kerberos unter Windows, Linux und macOS zu unterstützen.

> [!WARNING]
> Anmelde Informationen können über Anforderungen für eine Verbindung hinweg beibehalten werden. *Die Aushandlung der Authentifizierung darf nicht mit Proxys verwendet werden, es sei denn, der Proxy unterhält eine 1:1-Verbindungs Affinität (eine permanente Verbindung) mit Kestrel.*

> [!NOTE]
> Der Aushandlungs Handler erkennt, ob der zugrunde liegende Server die Windows-Authentifizierung nativ unterstützt und ob er aktiviert ist. Wenn der Server die Windows-Authentifizierung unterstützt, aber deaktiviert ist, wird ein Fehler ausgegeben, in dem Sie aufgefordert werden, die Server Implementierung zu aktivieren. Wenn die Windows-Authentifizierung auf dem Server aktiviert ist, wird der Aushandlungs Handler transparent an ihn weitergeleitet.

Fügen Sie Authentifizierungsdienste hinzu, indem Sie <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> und <xref:Microsoft.Extensions.DependencyInjection.NegotiateExtensions.AddNegotiate*> in Aufrufen `Startup.ConfigureServices` :

 ```csharp
// using Microsoft.AspNetCore.Authentication.Negotiate;
// using Microsoft.Extensions.DependencyInjection;

services.AddAuthentication(NegotiateDefaults.AuthenticationScheme)
    .AddNegotiate();
```

Fügen Sie Authentifizierungs Middleware hinzu, indem Sie <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> in Aufrufen `Startup.Configure` :

 ```csharp
app.UseAuthentication();
```

Weitere Informationen zur Middleware finden Sie unter <xref:fundamentals/middleware/index> .

Anonyme Anforderungen sind zulässig. Verwenden Sie [ASP.net Core Autorisierung](xref:security/authorization/introduction) , um anonyme Authentifizierungsanforderungen Aufforderungs zu fordern.

### <a name="windows-environment-configuration"></a>Konfiguration der Windows-Umgebung

Die [Microsoft. aspnetcore. Authentication. Aushandlungs](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) -Komponente führt die benutzermodusauthentifizierung durch. Dienst Prinzipal Namen (SPNs) müssen dem Benutzerkonto hinzugefügt werden, auf dem der-Dienst ausgeführt wird, und nicht auf dem Computer Konto. Führen Sie `setspn -S HTTP/myservername.mydomain.com myuser` in einer administrativen Befehlsshell aus.

### <a name="linux-and-macos-environment-configuration"></a>Konfiguration der Linux-und macOS-Umgebung

Anweisungen zum Verknüpfen eines Linux-oder macOS-Computers mit einer Windows-Domäne finden Sie im Artikel [Connect Azure Data Studio to your SQL Server Using Windows Authentication-Kerberos](/sql/azure-data-studio/enable-kerberos?view=sql-server-2017#join-your-os-to-the-active-directory-domain-controller) . In den Anweisungen wird ein Computer Konto für den Linux-Computer in der Domäne erstellt. Dem Computer Konto müssen SPNs hinzugefügt werden.

> [!NOTE]
> Wenn Sie die Anweisungen im Artikel [Herstellen einer Verbindung Azure Data Studio mit Ihrem SQL Server mithilfe von Windows-Authentifizierung-Kerberos](/sql/azure-data-studio/enable-kerberos?view=sql-server-2017#join-your-os-to-the-active-directory-domain-controller) befolgen, ersetzen Sie `python-software-properties` bei `python3-software-properties` Bedarf durch.

Nachdem der Linux-oder macOS-Computer der Domäne beigetreten ist, sind zusätzliche Schritte erforderlich, um eine [Schlüssel Tabellendatei-Datei](https://blogs.technet.microsoft.com/pie/2018/01/03/all-you-need-to-know-about-keytab-files/) mit den SPNs bereitzustellen:

* Fügen Sie dem Computer Konto auf dem Domänen Controller neue Webdienst-SPNs hinzu:
  * `setspn -S HTTP/mywebservice.mydomain.com mymachine`
  * `setspn -S HTTP/mywebservice@MYDOMAIN.COM mymachine`
* Verwenden Sie zum Generieren einer Schlüssel Tabellendatei-Datei " [ktpass](/windows-server/administration/windows-commands/ktpass) ":
  * `ktpass -princ HTTP/mywebservice.mydomain.com@MYDOMAIN.COM -pass myKeyTabFilePassword -mapuser MYDOMAIN\mymachine$ -pType KRB5_NT_PRINCIPAL -out c:\temp\mymachine.HTTP.keytab -crypto AES256-SHA1`
  * Einige Felder müssen in Großbuchstaben angegeben werden, wie angegeben.
* Kopieren Sie die Datei Schlüssel Tabellendatei auf den Linux-oder macOS-Computer.
* Wählen Sie die Datei Schlüssel Tabellendatei über eine Umgebungsvariable aus:`export KRB5_KTNAME=/tmp/mymachine.HTTP.keytab`
* Rufen `klist` Sie auf, um die derzeit zur Verwendung verfügbaren SPNs anzuzeigen.

> [!NOTE]
> Eine Schlüssel Tabellendatei-Datei enthält Anmelde Informationen für den Domänen Zugriff und muss entsprechend geschützt werden.

::: moniker-end

## <a name="httpsys"></a>HTTP.sys

[HTTP.sys](xref:fundamentals/servers/httpsys) unterstützt die Windows-Authentifizierung im Kernel Modus mithilfe von Aushandlungs-, NTLM-oder Standard Authentifizierung.

Fügen Sie Authentifizierungsdienste hinzu, indem Sie <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> ( <xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> Namespace) in Aufrufen `Startup.ConfigureServices` :

```csharp
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

Konfigurieren Sie den Webhost der APP für die Verwendung von HTTP.sys mit der Windows-Authentifizierung (*Program.cs*). <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*>befindet sich im- <xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> Namespace.

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](windowsauth/sample_snapshot/Program_GenericHost.cs?highlight=13-19)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](windowsauth/sample_snapshot/Program_WebHost.cs?highlight=9-15)]

::: moniker-end

> [!NOTE]
> HTTP.sys delegiert zur Kernelmodusauthentifizierung mit dem Kerberos-Authentifizierungsprotokoll. Die Benutzermodusauthentifizierung wird nicht von Kerberos und HTTP.sys unterstützt. Das Computerkonto muss zum Entschlüsseln des Kerberos-Tokens/-Tickets verwendet werden, das von Active Directory abgerufen und zur Authentifizierung des Benutzers vom Client an den Server weitergeleitet wird. Registrieren Sie den Dienstprinzipalnamen (SPN) für den Host, nicht für den Benutzer der App.

> [!NOTE]
> HTTP.sys wird unter Nano Server, Version 1709 oder höher, nicht unterstützt. Verwenden Sie zum Verwenden der Windows-Authentifizierung und HTTP.sys mit Nano Server einen [Server Core-Container (Microsoft/Windows Server Core)](https://hub.docker.com/r/microsoft/windowsservercore/). Weitere Informationen zu Server Core finden Sie unter [Was ist die Server Core-Installationsoption in Windows Server?](/windows-server/administration/server-core/what-is-server-core).

## <a name="authorize-users"></a>Autorisieren von Benutzern

Der Konfigurations Status des anonymen Zugriffs bestimmt die Art und Weise, in der das `[Authorize]` -Attribut und das- `[AllowAnonymous]` Attribut in der APP verwendet werden. In den folgenden beiden Abschnitten wird erläutert, wie die unzulässigen und zulässigen Konfigurations Zustände des anonymen Zugriffs behandelt werden.

### <a name="disallow-anonymous-access"></a>Anonymen Zugriff nicht zulassen

Wenn die Windows-Authentifizierung aktiviert und der anonyme Zugriff deaktiviert ist, `[Authorize]` haben das-Attribut und das- `[AllowAnonymous]` Attribut keine Auswirkung. Wenn eine IIS-Website so konfiguriert ist, dass anonyme Zugriffe nicht zulässig sind, erreicht die Anforderung die APP nie. Aus diesem Grund ist das- `[AllowAnonymous]` Attribut nicht anwendbar.

### <a name="allow-anonymous-access"></a>Anonymen Zugriff zulassen

Wenn die Windows-Authentifizierung und der anonyme Zugriff aktiviert sind, verwenden Sie das `[Authorize]` -Attribut und das- `[AllowAnonymous]` Attribut. Das- `[Authorize]` Attribut ermöglicht Ihnen das Sichern von Endpunkten der APP, für die eine Authentifizierung erforderlich ist. Das- `[AllowAnonymous]` Attribut überschreibt das- `[Authorize]` Attribut in apps, die anonymen Zugriff zulassen. Details zur Attribut Verwendung finden Sie unter <xref:security/authorization/simple> .

> [!NOTE]
> Standardmäßig wird Benutzern, denen keine Autorisierung für den Zugriff auf eine Seite fehlt, eine leere HTTP 403-Antwort angezeigt. Die [Middleware "Status Codepages](xref:fundamentals/error-handling#usestatuscodepages) " kann so konfiguriert werden, dass Benutzern eine bessere "Zugriff verweigert"-Umgebung zur Verfügung steht.

## <a name="impersonation"></a>Identitätswechsel

ASP.net Core implementiert keinen Identitätswechsel. Apps werden mit der APP-Identität für alle Anforderungen unter Verwendung des App-Pools oder der Prozess Identität ausgeführt. Wenn die APP eine Aktion im Namen eines Benutzers ausführen soll, verwenden Sie [Windows Identity. runidentitäts](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) , das in einer [terminalinline-Middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) in verwendet wird `Startup.Configure` . Führen Sie eine einzelne Aktion in diesem Kontext aus, und schließen Sie dann den Kontext.

[!code-csharp[](windowsauth/sample_snapshot/Startup.cs?highlight=10-19)]

`RunImpersonated`unterstützt keine asynchronen Vorgänge und sollte nicht für komplexe Szenarien verwendet werden. Beispielsweise wird das Packen ganzer Anforderungen oder Middleware-Ketten nicht unterstützt oder empfohlen.

::: moniker range=">= aspnetcore-3.0"

Während das [Microsoft. aspnetcore. Authentication. Aushandlungs](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) -Paket die Authentifizierung unter Windows, Linux und macOS ermöglicht, wird der Identitätswechsel nur unter Windows unterstützt.

::: moniker-end

## <a name="claims-transformations"></a>Transformationen von Ansprüchen

::: moniker range=">= aspnetcore-3.0"

Beim Hosting mit IIS wird <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> nicht intern aufgerufen, um einen Benutzer zu initialisieren. Deshalb ist eine <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation>-Implementierung, die verwendet wird, um Ansprüche nach jeder Authentifizierung zu transformieren, nicht standardmäßig aktiviert. Weitere Informationen und ein Codebeispiel, in dem Anspruchs Transformationen aktiviert werden, finden Sie unter <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model> .

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Beim Hosting mit dem Prozess internen IIS-Modus wird <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> nicht intern aufgerufen, um einen Benutzer zu initialisieren. Deshalb ist eine <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation>-Implementierung, die verwendet wird, um Ansprüche nach jeder Authentifizierung zu transformieren, nicht standardmäßig aktiviert. Weitere Informationen und ein Codebeispiel zum Aktivieren von Anspruchs Transformationen beim Prozess internen Hosting finden Sie unter <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model> .

::: moniker-end

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [dotnet publish](/dotnet/core/tools/dotnet-publish)
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/visual-studio-publish-profiles>
