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
# <a name="configure-windows-authentication-in-aspnet-core"></a><span data-ttu-id="69c1d-103">Konfigurieren der Windows-Authentifizierung in ASP.net Core</span><span class="sxs-lookup"><span data-stu-id="69c1d-103">Configure Windows Authentication in ASP.NET Core</span></span>

<span data-ttu-id="69c1d-104">Von [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="69c1d-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="69c1d-105">Die Windows-Authentifizierung (auch als Aushandlung, Kerberos oder NTLM-Authentifizierung bezeichnet) kann für ASP.net Core apps konfiguriert werden, die mit [IIS](xref:host-and-deploy/iis/index), [Kestrel](xref:fundamentals/servers/kestrel)oder [HTTP.sys](xref:fundamentals/servers/httpsys)gehostet werden.</span><span class="sxs-lookup"><span data-stu-id="69c1d-105">Windows Authentication (also known as Negotiate, Kerberos, or NTLM authentication) can be configured for ASP.NET Core apps hosted with [IIS](xref:host-and-deploy/iis/index), [Kestrel](xref:fundamentals/servers/kestrel), or [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="69c1d-106">Die Windows-Authentifizierung (auch als Aushandlung, Kerberos oder NTLM-Authentifizierung bezeichnet) kann für ASP.net Core apps konfiguriert werden, die mit [IIS](xref:host-and-deploy/iis/index) oder [HTTP.sys](xref:fundamentals/servers/httpsys)gehostet werden.</span><span class="sxs-lookup"><span data-stu-id="69c1d-106">Windows Authentication (also known as Negotiate, Kerberos, or NTLM authentication) can be configured for ASP.NET Core apps hosted with [IIS](xref:host-and-deploy/iis/index) or [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

::: moniker-end

<span data-ttu-id="69c1d-107">Die Windows-Authentifizierung basiert auf dem Betriebssystem, um Benutzer von ASP.net Core-apps zu authentifizieren.</span><span class="sxs-lookup"><span data-stu-id="69c1d-107">Windows Authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="69c1d-108">Sie können die Windows-Authentifizierung verwenden, wenn der Server in einem Unternehmensnetzwerk mit Active Directory Domänen Identitäten oder Windows-Konten ausgeführt wird, um Benutzer zu identifizieren.</span><span class="sxs-lookup"><span data-stu-id="69c1d-108">You can use Windows Authentication when your server runs on a corporate network using Active Directory domain identities or Windows accounts to identify users.</span></span> <span data-ttu-id="69c1d-109">Die Windows-Authentifizierung eignet sich am besten für Intranetumgebungen, in denen Benutzer, Client-apps und Webserver derselben Windows-Domäne angehören.</span><span class="sxs-lookup"><span data-stu-id="69c1d-109">Windows Authentication is best suited to intranet environments where users, client apps, and web servers belong to the same Windows domain.</span></span>

> [!NOTE]
> <span data-ttu-id="69c1d-110">Die Windows-Authentifizierung wird mit http/2 nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="69c1d-110">Windows Authentication isn't supported with HTTP/2.</span></span> <span data-ttu-id="69c1d-111">Authentifizierungs Herausforderungen können bei http/2-Antworten gesendet werden, aber der Client muss vor der Authentifizierung auf HTTP/1.1 herabgestuft werden.</span><span class="sxs-lookup"><span data-stu-id="69c1d-111">Authentication challenges can be sent on HTTP/2 responses, but the client must downgrade to HTTP/1.1 before authenticating.</span></span>

## <a name="proxy-and-load-balancer-scenarios"></a><span data-ttu-id="69c1d-112">Proxy-und Load Balancer-Szenarios</span><span class="sxs-lookup"><span data-stu-id="69c1d-112">Proxy and load balancer scenarios</span></span>

<span data-ttu-id="69c1d-113">Die Windows-Authentifizierung ist ein Zustands behaftetes Szenario, das in erster Linie in einem Intranet verwendet wird, in dem ein Proxy oder ein Load Balancer den Datenverkehr zwischen Clients und Servern nicht</span><span class="sxs-lookup"><span data-stu-id="69c1d-113">Windows Authentication is a stateful scenario primarily used in an intranet, where a proxy or load balancer doesn't usually handle traffic between clients and servers.</span></span> <span data-ttu-id="69c1d-114">Wenn ein Proxy oder ein Lasten Ausgleichs Modul verwendet wird, funktioniert die Windows-Authentifizierung nur, wenn der Proxy oder der Load Balancer:</span><span class="sxs-lookup"><span data-stu-id="69c1d-114">If a proxy or load balancer is used, Windows Authentication only works if the proxy or load balancer:</span></span>

* <span data-ttu-id="69c1d-115">Behandelt die-Authentifizierung.</span><span class="sxs-lookup"><span data-stu-id="69c1d-115">Handles the authentication.</span></span>
* <span data-ttu-id="69c1d-116">Übergibt die Benutzer Authentifizierungsinformationen an die APP (z. b. in einem Anforderungs Header), die die Authentifizierungsinformationen verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="69c1d-116">Passes the user authentication information to the app (for example, in a request header), which acts on the authentication information.</span></span>

<span data-ttu-id="69c1d-117">Eine Alternative zur Windows-Authentifizierung in Umgebungen, in denen Proxys und Lasten Ausgleichs Module verwendet werden, ist Active Directory Verbund Dienste (AD FS) mit OpenID Connect (oidc).</span><span class="sxs-lookup"><span data-stu-id="69c1d-117">An alternative to Windows Authentication in environments where proxies and load balancers are used is Active Directory Federated Services (ADFS) with OpenID Connect (OIDC).</span></span>

## <a name="iisiis-express"></a><span data-ttu-id="69c1d-118">IIS/IIS Express</span><span class="sxs-lookup"><span data-stu-id="69c1d-118">IIS/IIS Express</span></span>

<span data-ttu-id="69c1d-119">Fügen Sie Authentifizierungsdienste hinzu, indem Sie <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> ( <xref:Microsoft.AspNetCore.Server.IISIntegration?displayProperty=fullName> Namespace) in Aufrufen `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="69c1d-119">Add authentication services by invoking <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.IISIntegration?displayProperty=fullName> namespace) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

### <a name="launch-settings-debugger"></a><span data-ttu-id="69c1d-120">Start Einstellungen (Debugger)</span><span class="sxs-lookup"><span data-stu-id="69c1d-120">Launch settings (debugger)</span></span>

<span data-ttu-id="69c1d-121">Die Konfiguration für Start Einstellungen wirkt sich nur *auf die Eigenschaften/launchSettings.jsin* der Datei für IIS Express aus und konfiguriert IIS nicht für die Windows-Authentifizierung.</span><span class="sxs-lookup"><span data-stu-id="69c1d-121">Configuration for launch settings only affects the *Properties/launchSettings.json* file for IIS Express and doesn't configure IIS for Windows Authentication.</span></span> <span data-ttu-id="69c1d-122">Die Server Konfiguration wird im [IIS](#iis) -Abschnitt erläutert.</span><span class="sxs-lookup"><span data-stu-id="69c1d-122">Server configuration is explained in the [IIS](#iis) section.</span></span>

<span data-ttu-id="69c1d-123">Die **Webanwendungs** Vorlage, die über Visual Studio oder die .net Core-CLI verfügbar ist, kann so konfiguriert werden, dass die Windows *#b0-* Authentifizierung unterstützt wird</span><span class="sxs-lookup"><span data-stu-id="69c1d-123">The **Web Application** template available via Visual Studio or the .NET Core CLI can be configured to support Windows Authentication, which updates the *Properties/launchSettings.json* file automatically.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="69c1d-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="69c1d-124">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="69c1d-125">**Neues Projekt**</span><span class="sxs-lookup"><span data-stu-id="69c1d-125">**New project**</span></span>

1. <span data-ttu-id="69c1d-126">Erstellen Sie ein neues Projekt.</span><span class="sxs-lookup"><span data-stu-id="69c1d-126">Create a new project.</span></span>
1. <span data-ttu-id="69c1d-127">Wählen Sie **ASP.NET Core-Webanwendung** aus.</span><span class="sxs-lookup"><span data-stu-id="69c1d-127">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="69c1d-128">Klicken Sie auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="69c1d-128">Select **Next**.</span></span>
1. <span data-ttu-id="69c1d-129">Geben Sie einen Namen im Feld **Projektname** an.</span><span class="sxs-lookup"><span data-stu-id="69c1d-129">Provide a name in the **Project name** field.</span></span> <span data-ttu-id="69c1d-130">Vergewissern Sie sich, dass der Eintrag für den **Speicherort** korrekt ist, oder geben Sie einen Speicherort für das Projekt an.</span><span class="sxs-lookup"><span data-stu-id="69c1d-130">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="69c1d-131">Wählen Sie **Erstellen** aus.</span><span class="sxs-lookup"><span data-stu-id="69c1d-131">Select **Create**.</span></span>
1. <span data-ttu-id="69c1d-132">Wählen Sie unter **Authentifizierung**die Option **ändern** .</span><span class="sxs-lookup"><span data-stu-id="69c1d-132">Select **Change** under **Authentication**.</span></span>
1. <span data-ttu-id="69c1d-133">Wählen Sie im Fenster **Authentifizierung ändern** die Option **Windows-Authentifizierung**aus.</span><span class="sxs-lookup"><span data-stu-id="69c1d-133">In the **Change Authentication** window, select **Windows Authentication**.</span></span> <span data-ttu-id="69c1d-134">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="69c1d-134">Select **OK**.</span></span>
1. <span data-ttu-id="69c1d-135">Wählen Sie **Webanwendung** aus.</span><span class="sxs-lookup"><span data-stu-id="69c1d-135">Select **Web Application**.</span></span>
1. <span data-ttu-id="69c1d-136">Klicken Sie auf **Erstellen**.</span><span class="sxs-lookup"><span data-stu-id="69c1d-136">Select **Create**.</span></span>

<span data-ttu-id="69c1d-137">Führen Sie die App aus.</span><span class="sxs-lookup"><span data-stu-id="69c1d-137">Run the app.</span></span> <span data-ttu-id="69c1d-138">Der Benutzername wird in der Benutzeroberfläche der gerenderten App angezeigt.</span><span class="sxs-lookup"><span data-stu-id="69c1d-138">The username appears in the rendered app's user interface.</span></span>

<span data-ttu-id="69c1d-139">**Vorhandenes Projekt**</span><span class="sxs-lookup"><span data-stu-id="69c1d-139">**Existing project**</span></span>

<span data-ttu-id="69c1d-140">Die Projekteigenschaften aktivieren die Windows-Authentifizierung und deaktivieren die anonyme Authentifizierung:</span><span class="sxs-lookup"><span data-stu-id="69c1d-140">The project's properties enable Windows Authentication and disable Anonymous Authentication:</span></span>

1. <span data-ttu-id="69c1d-141">Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und wählen Sie **Eigenschaften** aus.</span><span class="sxs-lookup"><span data-stu-id="69c1d-141">Right-click the project in **Solution Explorer** and select **Properties**.</span></span>
1. <span data-ttu-id="69c1d-142">Klicken Sie auf die Registerkarte **Debuggen**.</span><span class="sxs-lookup"><span data-stu-id="69c1d-142">Select the **Debug** tab.</span></span>
1. <span data-ttu-id="69c1d-143">Deaktivieren Sie das Kontrollkästchen **anonyme Authentifizierung aktivieren**.</span><span class="sxs-lookup"><span data-stu-id="69c1d-143">Clear the check box for **Enable Anonymous Authentication**.</span></span>
1. <span data-ttu-id="69c1d-144">Aktivieren Sie das Kontrollkästchen **Windows-Authentifizierung aktivieren**.</span><span class="sxs-lookup"><span data-stu-id="69c1d-144">Select the check box for **Enable Windows Authentication**.</span></span>
1. <span data-ttu-id="69c1d-145">Speichern und schließen Sie die Eigenschaften Seite.</span><span class="sxs-lookup"><span data-stu-id="69c1d-145">Save and close the property page.</span></span>

<span data-ttu-id="69c1d-146">Alternativ können die Eigenschaften im `iisSettings` Knoten des *launchSettings.jsin* der Datei konfiguriert werden:</span><span class="sxs-lookup"><span data-stu-id="69c1d-146">Alternatively, the properties can be configured in the `iisSettings` node of the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

# <a name="net-core-cli"></a>[<span data-ttu-id="69c1d-147">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="69c1d-147">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="69c1d-148">**Neues Projekt**</span><span class="sxs-lookup"><span data-stu-id="69c1d-148">**New project**</span></span>

<span data-ttu-id="69c1d-149">Führen Sie den [dotnet New](/dotnet/core/tools/dotnet-new) -Befehl mit dem `webapp` Argument (ASP.net Core Web-App) und `--auth Windows` Switch aus:</span><span class="sxs-lookup"><span data-stu-id="69c1d-149">Execute the [dotnet new](/dotnet/core/tools/dotnet-new) command with the `webapp` argument (ASP.NET Core Web App) and `--auth Windows` switch:</span></span>

```dotnetcli
dotnet new webapp --auth Windows
```

<span data-ttu-id="69c1d-150">**Vorhandenes Projekt**</span><span class="sxs-lookup"><span data-stu-id="69c1d-150">**Existing project**</span></span>

<span data-ttu-id="69c1d-151">Aktualisieren Sie den `iisSettings` Knoten des *launchSettings.jsin* der Datei:</span><span class="sxs-lookup"><span data-stu-id="69c1d-151">Update the `iisSettings` node of the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

---

<span data-ttu-id="69c1d-152">Wenn Sie ein vorhandenes Projekt ändern, vergewissern Sie sich, dass die Projektdatei einen Paket Verweis für das [Metapaket Microsoft. aspnetcore. app](xref:fundamentals/metapackage-app) **oder** das nuget-Paket [Microsoft. aspnetcore. Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) enthält.</span><span class="sxs-lookup"><span data-stu-id="69c1d-152">When modifying an existing project, confirm that the project file includes a package reference for the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) **or** the [Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) NuGet package.</span></span>

### <a name="iis"></a><span data-ttu-id="69c1d-153">IIS</span><span class="sxs-lookup"><span data-stu-id="69c1d-153">IIS</span></span>

<span data-ttu-id="69c1d-154">IIS verwendet das [ASP.net Core-Modul](xref:host-and-deploy/aspnet-core-module) , um ASP.net Core-apps zu hosten.</span><span class="sxs-lookup"><span data-stu-id="69c1d-154">IIS uses the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) to host ASP.NET Core apps.</span></span> <span data-ttu-id="69c1d-155">Die Windows-Authentifizierung wird für IIS über die *web.config* -Datei konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="69c1d-155">Windows Authentication is configured for IIS via the *web.config* file.</span></span> <span data-ttu-id="69c1d-156">In den folgenden Abschnitten werden diese Aktionen gezeigt:</span><span class="sxs-lookup"><span data-stu-id="69c1d-156">The following sections show how to:</span></span>

* <span data-ttu-id="69c1d-157">Geben Sie eine lokale *web.config* Datei an, die die Windows-Authentifizierung auf dem Server aktiviert, wenn die APP bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="69c1d-157">Provide a local *web.config* file that activates Windows Authentication on the server when the app is deployed.</span></span>
* <span data-ttu-id="69c1d-158">Verwenden Sie den IIS-Manager, um die *web.config* Datei einer ASP.net Core-APP zu konfigurieren, die bereits auf dem-Server bereitgestellt wurde.</span><span class="sxs-lookup"><span data-stu-id="69c1d-158">Use the IIS Manager to configure the *web.config* file of an ASP.NET Core app that has already been deployed to the server.</span></span>

<span data-ttu-id="69c1d-159">Wenn Sie dies nicht bereits getan haben, aktivieren Sie IIS, um ASP.net Core-apps zu hosten.</span><span class="sxs-lookup"><span data-stu-id="69c1d-159">If you haven't already done so, enable IIS to host ASP.NET Core apps.</span></span> <span data-ttu-id="69c1d-160">Weitere Informationen finden Sie unter <xref:host-and-deploy/iis/index>.</span><span class="sxs-lookup"><span data-stu-id="69c1d-160">For more information, see <xref:host-and-deploy/iis/index>.</span></span>

<span data-ttu-id="69c1d-161">Aktivieren Sie den IIS-Rollen Dienst für die Windows-Authentifizierung.</span><span class="sxs-lookup"><span data-stu-id="69c1d-161">Enable the IIS Role Service for Windows Authentication.</span></span> <span data-ttu-id="69c1d-162">Weitere Informationen finden Sie unter [Aktivieren der Windows-Authentifizierung in IIS-Rollen Diensten (siehe Schritt 2)](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="69c1d-162">For more information, see [Enable Windows Authentication in IIS Role Services (see Step 2)](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

<span data-ttu-id="69c1d-163">Die [Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) für die IIS-Integration ist so konfiguriert, dass Anforderungen automatisch automatisch authentifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="69c1d-163">[IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) is configured to automatically authenticate requests by default.</span></span> <span data-ttu-id="69c1d-164">Weitere Informationen finden Sie unter [Hosten von ASP.net Core unter Windows mit IIS: IIS-Optionen (automaticauthentication)](xref:host-and-deploy/iis/index#iis-options).</span><span class="sxs-lookup"><span data-stu-id="69c1d-164">For more information, see [Host ASP.NET Core on Windows with IIS: IIS options (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span></span>

<span data-ttu-id="69c1d-165">Das ASP.net Core-Modul ist so konfiguriert, dass das Windows-Authentifizierungs Token standardmäßig an die APP weiterleiten wird.</span><span class="sxs-lookup"><span data-stu-id="69c1d-165">The ASP.NET Core Module is configured to forward the Windows Authentication token to the app by default.</span></span> <span data-ttu-id="69c1d-166">Weitere Informationen finden Sie unter [ASP.net Core Modul Konfigurations Referenz: Attribute des aspnetcore-Elements](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="69c1d-166">For more information, see [ASP.NET Core Module configuration reference: Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

<span data-ttu-id="69c1d-167">Verwenden **Sie** einen der folgenden Ansätze:</span><span class="sxs-lookup"><span data-stu-id="69c1d-167">Use **either** of the following approaches:</span></span>

* <span data-ttu-id="69c1d-168">Fügen Sie dem Projektstamm **vor dem Veröffentlichen und Bereitstellen des Projekts** die folgende *web.config* Datei hinzu:</span><span class="sxs-lookup"><span data-stu-id="69c1d-168">**Before publishing and deploying the project,** add the following *web.config* file to the project root:</span></span>

  [!code-xml[](windowsauth/sample_snapshot/web_2.config)]

  <span data-ttu-id="69c1d-169">Wenn das Projekt vom .net Core SDK veröffentlicht wird (ohne dass die- `<IsTransformWebConfigDisabled>` Eigenschaft `true` in der Projektdatei auf festgelegt ist), enthält die veröffentlichte *web.config* Datei den `<location><system.webServer><security><authentication>` Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="69c1d-169">When the project is published by the .NET Core SDK (without the `<IsTransformWebConfigDisabled>` property set to `true` in the project file), the published *web.config* file includes the `<location><system.webServer><security><authentication>` section.</span></span> <span data-ttu-id="69c1d-170">Weitere Informationen zur- `<IsTransformWebConfigDisabled>` Eigenschaft finden Sie unter <xref:host-and-deploy/iis/index#webconfig-file> .</span><span class="sxs-lookup"><span data-stu-id="69c1d-170">For more information on the `<IsTransformWebConfigDisabled>` property, see <xref:host-and-deploy/iis/index#webconfig-file>.</span></span>

* <span data-ttu-id="69c1d-171">**Nachdem Sie das Projekt veröffentlicht und bereitgestellt haben,** führen Sie die serverseitige Konfiguration mit dem IIS-Manager aus:</span><span class="sxs-lookup"><span data-stu-id="69c1d-171">**After publishing and deploying the project,** perform server-side configuration with the IIS Manager:</span></span>

  1. <span data-ttu-id="69c1d-172">Wählen Sie im IIS-Manager die IIS-Website unter dem Knoten **Sites** der Rand Leiste **Verbindungen** aus.</span><span class="sxs-lookup"><span data-stu-id="69c1d-172">In IIS Manager, select the IIS site under the **Sites** node of the **Connections** sidebar.</span></span>
  1. <span data-ttu-id="69c1d-173">Doppelklicken Sie im **IIS** -Bereich auf **Authentifizierung** .</span><span class="sxs-lookup"><span data-stu-id="69c1d-173">Double-click **Authentication** in the **IIS** area.</span></span>
  1. <span data-ttu-id="69c1d-174">Wählen Sie **anonyme Authentifizierung**aus.</span><span class="sxs-lookup"><span data-stu-id="69c1d-174">Select **Anonymous Authentication**.</span></span> <span data-ttu-id="69c1d-175">Wählen Sie in der Rand Leiste **Aktionen** die Option **Deaktivieren** aus.</span><span class="sxs-lookup"><span data-stu-id="69c1d-175">Select **Disable** in the **Actions** sidebar.</span></span>
  1. <span data-ttu-id="69c1d-176">Wählen Sie **Windows-Authentifizierung** aus.</span><span class="sxs-lookup"><span data-stu-id="69c1d-176">Select **Windows Authentication**.</span></span> <span data-ttu-id="69c1d-177">Wählen Sie in der Rand Leiste **Aktionen** die Option **aktivieren** aus.</span><span class="sxs-lookup"><span data-stu-id="69c1d-177">Select **Enable** in the **Actions** sidebar.</span></span>

  <span data-ttu-id="69c1d-178">Wenn diese Aktionen ausgeführt werden, ändert der IIS-Manager die *web.config* Datei der app.</span><span class="sxs-lookup"><span data-stu-id="69c1d-178">When these actions are taken, IIS Manager modifies the app's *web.config* file.</span></span> <span data-ttu-id="69c1d-179">Ein `<system.webServer><security><authentication>` Knoten mit den aktualisierten Einstellungen für und wurde hinzugefügt `anonymousAuthentication` `windowsAuthentication` :</span><span class="sxs-lookup"><span data-stu-id="69c1d-179">A `<system.webServer><security><authentication>` node is added with updated settings for `anonymousAuthentication` and `windowsAuthentication`:</span></span>

  [!code-xml[](windowsauth/sample_snapshot/web_1.config?highlight=4-5)]

  <span data-ttu-id="69c1d-180">Der `<system.webServer>` Abschnitt, der vom IIS-Manager zum *web.config* -Datei hinzugefügt wird, befindet sich außerhalb des App-Abschnitts, der `<location>` durch den .net Core SDK bei der Veröffentlichung der app hinzugefügt wurde</span><span class="sxs-lookup"><span data-stu-id="69c1d-180">The `<system.webServer>` section added to the *web.config* file by IIS Manager is outside of the app's `<location>` section added by the .NET Core SDK when the app is published.</span></span> <span data-ttu-id="69c1d-181">Da der Abschnitt außerhalb des Knotens hinzugefügt wird `<location>` , werden die Einstellungen von allen [untergeordneten apps](xref:host-and-deploy/iis/index#sub-applications) der aktuellen App geerbt.</span><span class="sxs-lookup"><span data-stu-id="69c1d-181">Because the section is added outside of the `<location>` node, the settings are inherited by any [sub-apps](xref:host-and-deploy/iis/index#sub-applications) to the current app.</span></span> <span data-ttu-id="69c1d-182">Um die Vererbung zu verhindern, verschieben Sie den hinzugefügten `<security>` Abschnitt innerhalb des `<location><system.webServer>` Abschnitts, den der .net Core SDK bereitgestellt hat</span><span class="sxs-lookup"><span data-stu-id="69c1d-182">To prevent inheritance, move the added `<security>` section inside of the `<location><system.webServer>` section that the .NET Core SDK provided.</span></span>

  <span data-ttu-id="69c1d-183">Wenn IIS-Manager zum Hinzufügen der IIS-Konfiguration verwendet wird, wirkt sich dies nur auf die *web.config* Datei der APP auf dem Server aus.</span><span class="sxs-lookup"><span data-stu-id="69c1d-183">When IIS Manager is used to add the IIS configuration, it only affects the app's *web.config* file on the server.</span></span> <span data-ttu-id="69c1d-184">Eine nachfolgende Bereitstellung der APP kann die Einstellungen auf dem Server überschreiben, wenn die Kopie von *web.config* des Servers durch die *web.config* Datei des Projekts ersetzt wird.</span><span class="sxs-lookup"><span data-stu-id="69c1d-184">A subsequent deployment of the app may overwrite the settings on the server if the server's copy of *web.config* is replaced by the project's *web.config* file.</span></span> <span data-ttu-id="69c1d-185">Verwenden **Sie** einen der folgenden Ansätze, um die Einstellungen zu verwalten:</span><span class="sxs-lookup"><span data-stu-id="69c1d-185">Use **either** of the following approaches to manage the settings:</span></span>

  * <span data-ttu-id="69c1d-186">Verwenden Sie IIS Manager, um die Einstellungen in der *web.config* -Datei zurückzusetzen, nachdem die Datei bei der Bereitstellung überschrieben wurde.</span><span class="sxs-lookup"><span data-stu-id="69c1d-186">Use IIS Manager to reset the settings in the *web.config* file after the file is overwritten on deployment.</span></span>
  * <span data-ttu-id="69c1d-187">Fügen Sie der App eine *web.config Datei* lokal mit den Einstellungen hinzu.</span><span class="sxs-lookup"><span data-stu-id="69c1d-187">Add a *web.config file* to the app locally with the settings.</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="kestrel"></a><span data-ttu-id="69c1d-188">Kestrel</span><span class="sxs-lookup"><span data-stu-id="69c1d-188">Kestrel</span></span>

<span data-ttu-id="69c1d-189">Das nuget-Paket [Microsoft. aspnetcore. Authentication. Aushandlungs](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) Paket kann mit [Kestrel](xref:fundamentals/servers/kestrel) verwendet werden, um die Windows-Authentifizierung mithilfe von Aushandlungs-und Kerberos unter Windows, Linux und macOS zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="69c1d-189">The [Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) NuGet package can be used with [Kestrel](xref:fundamentals/servers/kestrel) to support Windows Authentication using Negotiate and Kerberos on Windows, Linux, and macOS.</span></span>

> [!WARNING]
> <span data-ttu-id="69c1d-190">Anmelde Informationen können über Anforderungen für eine Verbindung hinweg beibehalten werden.</span><span class="sxs-lookup"><span data-stu-id="69c1d-190">Credentials can be persisted across requests on a connection.</span></span> <span data-ttu-id="69c1d-191">*Die Aushandlung der Authentifizierung darf nicht mit Proxys verwendet werden, es sei denn, der Proxy unterhält eine 1:1-Verbindungs Affinität (eine permanente Verbindung) mit Kestrel.*</span><span class="sxs-lookup"><span data-stu-id="69c1d-191">*Negotiate authentication must not be used with proxies unless the proxy maintains a 1:1 connection affinity (a persistent connection) with Kestrel.*</span></span>

> [!NOTE]
> <span data-ttu-id="69c1d-192">Der Aushandlungs Handler erkennt, ob der zugrunde liegende Server die Windows-Authentifizierung nativ unterstützt und ob er aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="69c1d-192">The Negotiate handler detects if the underlying server supports Windows Authentication natively and if it's enabled.</span></span> <span data-ttu-id="69c1d-193">Wenn der Server die Windows-Authentifizierung unterstützt, aber deaktiviert ist, wird ein Fehler ausgegeben, in dem Sie aufgefordert werden, die Server Implementierung zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="69c1d-193">If the server supports Windows Authentication but it's disabled, an error is thrown asking you to enable the server implementation.</span></span> <span data-ttu-id="69c1d-194">Wenn die Windows-Authentifizierung auf dem Server aktiviert ist, wird der Aushandlungs Handler transparent an ihn weitergeleitet.</span><span class="sxs-lookup"><span data-stu-id="69c1d-194">When Windows Authentication is enabled in the server, the Negotiate handler transparently forwards to it.</span></span>

<span data-ttu-id="69c1d-195">Fügen Sie Authentifizierungsdienste hinzu, indem Sie <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> und <xref:Microsoft.Extensions.DependencyInjection.NegotiateExtensions.AddNegotiate*> in Aufrufen `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="69c1d-195">Add authentication services by invoking <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> and <xref:Microsoft.Extensions.DependencyInjection.NegotiateExtensions.AddNegotiate*> in `Startup.ConfigureServices`:</span></span>

 ```csharp
// using Microsoft.AspNetCore.Authentication.Negotiate;
// using Microsoft.Extensions.DependencyInjection;

services.AddAuthentication(NegotiateDefaults.AuthenticationScheme)
    .AddNegotiate();
```

<span data-ttu-id="69c1d-196">Fügen Sie Authentifizierungs Middleware hinzu, indem Sie <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> in Aufrufen `Startup.Configure` :</span><span class="sxs-lookup"><span data-stu-id="69c1d-196">Add Authentication Middleware by calling <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> in `Startup.Configure`:</span></span>

 ```csharp
app.UseAuthentication();
```

<span data-ttu-id="69c1d-197">Weitere Informationen zur Middleware finden Sie unter <xref:fundamentals/middleware/index> .</span><span class="sxs-lookup"><span data-stu-id="69c1d-197">For more information on middleware, see <xref:fundamentals/middleware/index>.</span></span>

<span data-ttu-id="69c1d-198">Anonyme Anforderungen sind zulässig.</span><span class="sxs-lookup"><span data-stu-id="69c1d-198">Anonymous requests are allowed.</span></span> <span data-ttu-id="69c1d-199">Verwenden Sie [ASP.net Core Autorisierung](xref:security/authorization/introduction) , um anonyme Authentifizierungsanforderungen Aufforderungs zu fordern.</span><span class="sxs-lookup"><span data-stu-id="69c1d-199">Use [ASP.NET Core Authorization](xref:security/authorization/introduction) to challenge anonymous requests for authentication.</span></span>

### <a name="windows-environment-configuration"></a><span data-ttu-id="69c1d-200">Konfiguration der Windows-Umgebung</span><span class="sxs-lookup"><span data-stu-id="69c1d-200">Windows environment configuration</span></span>

<span data-ttu-id="69c1d-201">Die [Microsoft. aspnetcore. Authentication. Aushandlungs](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) -Komponente führt die benutzermodusauthentifizierung durch.</span><span class="sxs-lookup"><span data-stu-id="69c1d-201">The [Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) component performs User Mode authentication.</span></span> <span data-ttu-id="69c1d-202">Dienst Prinzipal Namen (SPNs) müssen dem Benutzerkonto hinzugefügt werden, auf dem der-Dienst ausgeführt wird, und nicht auf dem Computer Konto.</span><span class="sxs-lookup"><span data-stu-id="69c1d-202">Service Principal Names (SPNs) must be added to the user account running the service, not the machine account.</span></span> <span data-ttu-id="69c1d-203">Führen Sie `setspn -S HTTP/myservername.mydomain.com myuser` in einer administrativen Befehlsshell aus.</span><span class="sxs-lookup"><span data-stu-id="69c1d-203">Execute `setspn -S HTTP/myservername.mydomain.com myuser` in an administrative command shell.</span></span>

### <a name="linux-and-macos-environment-configuration"></a><span data-ttu-id="69c1d-204">Konfiguration der Linux-und macOS-Umgebung</span><span class="sxs-lookup"><span data-stu-id="69c1d-204">Linux and macOS environment configuration</span></span>

<span data-ttu-id="69c1d-205">Anweisungen zum Verknüpfen eines Linux-oder macOS-Computers mit einer Windows-Domäne finden Sie im Artikel [Connect Azure Data Studio to your SQL Server Using Windows Authentication-Kerberos](/sql/azure-data-studio/enable-kerberos?view=sql-server-2017#join-your-os-to-the-active-directory-domain-controller) .</span><span class="sxs-lookup"><span data-stu-id="69c1d-205">Instructions for joining a Linux or macOS machine to a Windows domain are available in the [Connect Azure Data Studio to your SQL Server using Windows authentication - Kerberos](/sql/azure-data-studio/enable-kerberos?view=sql-server-2017#join-your-os-to-the-active-directory-domain-controller) article.</span></span> <span data-ttu-id="69c1d-206">In den Anweisungen wird ein Computer Konto für den Linux-Computer in der Domäne erstellt.</span><span class="sxs-lookup"><span data-stu-id="69c1d-206">The instructions create a machine account for the Linux machine on the domain.</span></span> <span data-ttu-id="69c1d-207">Dem Computer Konto müssen SPNs hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="69c1d-207">SPNs must be added to that machine account.</span></span>

> [!NOTE]
> <span data-ttu-id="69c1d-208">Wenn Sie die Anweisungen im Artikel [Herstellen einer Verbindung Azure Data Studio mit Ihrem SQL Server mithilfe von Windows-Authentifizierung-Kerberos](/sql/azure-data-studio/enable-kerberos?view=sql-server-2017#join-your-os-to-the-active-directory-domain-controller) befolgen, ersetzen Sie `python-software-properties` bei `python3-software-properties` Bedarf durch.</span><span class="sxs-lookup"><span data-stu-id="69c1d-208">When following the guidance in the [Connect Azure Data Studio to your SQL Server using Windows authentication - Kerberos](/sql/azure-data-studio/enable-kerberos?view=sql-server-2017#join-your-os-to-the-active-directory-domain-controller) article, replace `python-software-properties` with `python3-software-properties` if needed.</span></span>

<span data-ttu-id="69c1d-209">Nachdem der Linux-oder macOS-Computer der Domäne beigetreten ist, sind zusätzliche Schritte erforderlich, um eine [Schlüssel Tabellendatei-Datei](https://blogs.technet.microsoft.com/pie/2018/01/03/all-you-need-to-know-about-keytab-files/) mit den SPNs bereitzustellen:</span><span class="sxs-lookup"><span data-stu-id="69c1d-209">Once the Linux or macOS machine is joined to the domain, additional steps are required to provide a [keytab file](https://blogs.technet.microsoft.com/pie/2018/01/03/all-you-need-to-know-about-keytab-files/) with the SPNs:</span></span>

* <span data-ttu-id="69c1d-210">Fügen Sie dem Computer Konto auf dem Domänen Controller neue Webdienst-SPNs hinzu:</span><span class="sxs-lookup"><span data-stu-id="69c1d-210">On the domain controller, add new web service SPNs to the machine account:</span></span>
  * `setspn -S HTTP/mywebservice.mydomain.com mymachine`
  * `setspn -S HTTP/mywebservice@MYDOMAIN.COM mymachine`
* <span data-ttu-id="69c1d-211">Verwenden Sie zum Generieren einer Schlüssel Tabellendatei-Datei " [ktpass](/windows-server/administration/windows-commands/ktpass) ":</span><span class="sxs-lookup"><span data-stu-id="69c1d-211">Use [ktpass](/windows-server/administration/windows-commands/ktpass) to generate a keytab file:</span></span>
  * `ktpass -princ HTTP/mywebservice.mydomain.com@MYDOMAIN.COM -pass myKeyTabFilePassword -mapuser MYDOMAIN\mymachine$ -pType KRB5_NT_PRINCIPAL -out c:\temp\mymachine.HTTP.keytab -crypto AES256-SHA1`
  * <span data-ttu-id="69c1d-212">Einige Felder müssen in Großbuchstaben angegeben werden, wie angegeben.</span><span class="sxs-lookup"><span data-stu-id="69c1d-212">Some fields must be specified in uppercase as indicated.</span></span>
* <span data-ttu-id="69c1d-213">Kopieren Sie die Datei Schlüssel Tabellendatei auf den Linux-oder macOS-Computer.</span><span class="sxs-lookup"><span data-stu-id="69c1d-213">Copy the keytab file to the Linux or macOS machine.</span></span>
* <span data-ttu-id="69c1d-214">Wählen Sie die Datei Schlüssel Tabellendatei über eine Umgebungsvariable aus:`export KRB5_KTNAME=/tmp/mymachine.HTTP.keytab`</span><span class="sxs-lookup"><span data-stu-id="69c1d-214">Select the keytab file via an environment variable: `export KRB5_KTNAME=/tmp/mymachine.HTTP.keytab`</span></span>
* <span data-ttu-id="69c1d-215">Rufen `klist` Sie auf, um die derzeit zur Verwendung verfügbaren SPNs anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="69c1d-215">Invoke `klist` to show the SPNs currently available for use.</span></span>

> [!NOTE]
> <span data-ttu-id="69c1d-216">Eine Schlüssel Tabellendatei-Datei enthält Anmelde Informationen für den Domänen Zugriff und muss entsprechend geschützt werden.</span><span class="sxs-lookup"><span data-stu-id="69c1d-216">A keytab file contains domain access credentials and must be protected accordingly.</span></span>

::: moniker-end

## <a name="httpsys"></a><span data-ttu-id="69c1d-217">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="69c1d-217">HTTP.sys</span></span>

<span data-ttu-id="69c1d-218">[HTTP.sys](xref:fundamentals/servers/httpsys) unterstützt die Windows-Authentifizierung im Kernel Modus mithilfe von Aushandlungs-, NTLM-oder Standard Authentifizierung.</span><span class="sxs-lookup"><span data-stu-id="69c1d-218">[HTTP.sys](xref:fundamentals/servers/httpsys) supports Kernel Mode Windows Authentication using Negotiate, NTLM, or Basic authentication.</span></span>

<span data-ttu-id="69c1d-219">Fügen Sie Authentifizierungsdienste hinzu, indem Sie <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> ( <xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> Namespace) in Aufrufen `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="69c1d-219">Add authentication services by invoking <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> namespace) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

<span data-ttu-id="69c1d-220">Konfigurieren Sie den Webhost der APP für die Verwendung von HTTP.sys mit der Windows-Authentifizierung (*Program.cs*).</span><span class="sxs-lookup"><span data-stu-id="69c1d-220">Configure the app's web host to use HTTP.sys with Windows Authentication (*Program.cs*).</span></span> <span data-ttu-id="69c1d-221"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*>befindet sich im- <xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> Namespace.</span><span class="sxs-lookup"><span data-stu-id="69c1d-221"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> is in the <xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> namespace.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](windowsauth/sample_snapshot/Program_GenericHost.cs?highlight=13-19)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](windowsauth/sample_snapshot/Program_WebHost.cs?highlight=9-15)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="69c1d-222">HTTP.sys delegiert zur Kernelmodusauthentifizierung mit dem Kerberos-Authentifizierungsprotokoll.</span><span class="sxs-lookup"><span data-stu-id="69c1d-222">HTTP.sys delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="69c1d-223">Die Benutzermodusauthentifizierung wird nicht von Kerberos und HTTP.sys unterstützt.</span><span class="sxs-lookup"><span data-stu-id="69c1d-223">User mode authentication isn't supported with Kerberos and HTTP.sys.</span></span> <span data-ttu-id="69c1d-224">Das Computerkonto muss zum Entschlüsseln des Kerberos-Tokens/-Tickets verwendet werden, das von Active Directory abgerufen und zur Authentifizierung des Benutzers vom Client an den Server weitergeleitet wird.</span><span class="sxs-lookup"><span data-stu-id="69c1d-224">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="69c1d-225">Registrieren Sie den Dienstprinzipalnamen (SPN) für den Host, nicht für den Benutzer der App.</span><span class="sxs-lookup"><span data-stu-id="69c1d-225">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

> [!NOTE]
> <span data-ttu-id="69c1d-226">HTTP.sys wird unter Nano Server, Version 1709 oder höher, nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="69c1d-226">HTTP.sys isn't supported on Nano Server version 1709 or later.</span></span> <span data-ttu-id="69c1d-227">Verwenden Sie zum Verwenden der Windows-Authentifizierung und HTTP.sys mit Nano Server einen [Server Core-Container (Microsoft/Windows Server Core)](https://hub.docker.com/r/microsoft/windowsservercore/).</span><span class="sxs-lookup"><span data-stu-id="69c1d-227">To use Windows Authentication and HTTP.sys with Nano Server, use a [Server Core (microsoft/windowsservercore) container](https://hub.docker.com/r/microsoft/windowsservercore/).</span></span> <span data-ttu-id="69c1d-228">Weitere Informationen zu Server Core finden Sie unter [Was ist die Server Core-Installationsoption in Windows Server?](/windows-server/administration/server-core/what-is-server-core).</span><span class="sxs-lookup"><span data-stu-id="69c1d-228">For more information on Server Core, see [What is the Server Core installation option in Windows Server?](/windows-server/administration/server-core/what-is-server-core).</span></span>

## <a name="authorize-users"></a><span data-ttu-id="69c1d-229">Autorisieren von Benutzern</span><span class="sxs-lookup"><span data-stu-id="69c1d-229">Authorize users</span></span>

<span data-ttu-id="69c1d-230">Der Konfigurations Status des anonymen Zugriffs bestimmt die Art und Weise, in der das `[Authorize]` -Attribut und das- `[AllowAnonymous]` Attribut in der APP verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="69c1d-230">The configuration state of anonymous access determines the way in which the `[Authorize]` and `[AllowAnonymous]` attributes are used in the app.</span></span> <span data-ttu-id="69c1d-231">In den folgenden beiden Abschnitten wird erläutert, wie die unzulässigen und zulässigen Konfigurations Zustände des anonymen Zugriffs behandelt werden.</span><span class="sxs-lookup"><span data-stu-id="69c1d-231">The following two sections explain how to handle the disallowed and allowed configuration states of anonymous access.</span></span>

### <a name="disallow-anonymous-access"></a><span data-ttu-id="69c1d-232">Anonymen Zugriff nicht zulassen</span><span class="sxs-lookup"><span data-stu-id="69c1d-232">Disallow anonymous access</span></span>

<span data-ttu-id="69c1d-233">Wenn die Windows-Authentifizierung aktiviert und der anonyme Zugriff deaktiviert ist, `[Authorize]` haben das-Attribut und das- `[AllowAnonymous]` Attribut keine Auswirkung.</span><span class="sxs-lookup"><span data-stu-id="69c1d-233">When Windows Authentication is enabled and anonymous access is disabled, the `[Authorize]` and `[AllowAnonymous]` attributes have no effect.</span></span> <span data-ttu-id="69c1d-234">Wenn eine IIS-Website so konfiguriert ist, dass anonyme Zugriffe nicht zulässig sind, erreicht die Anforderung die APP nie.</span><span class="sxs-lookup"><span data-stu-id="69c1d-234">If an IIS site is configured to disallow anonymous access, the request never reaches the app.</span></span> <span data-ttu-id="69c1d-235">Aus diesem Grund ist das- `[AllowAnonymous]` Attribut nicht anwendbar.</span><span class="sxs-lookup"><span data-stu-id="69c1d-235">For this reason, the `[AllowAnonymous]` attribute isn't applicable.</span></span>

### <a name="allow-anonymous-access"></a><span data-ttu-id="69c1d-236">Anonymen Zugriff zulassen</span><span class="sxs-lookup"><span data-stu-id="69c1d-236">Allow anonymous access</span></span>

<span data-ttu-id="69c1d-237">Wenn die Windows-Authentifizierung und der anonyme Zugriff aktiviert sind, verwenden Sie das `[Authorize]` -Attribut und das- `[AllowAnonymous]` Attribut.</span><span class="sxs-lookup"><span data-stu-id="69c1d-237">When both Windows Authentication and anonymous access are enabled, use the `[Authorize]` and `[AllowAnonymous]` attributes.</span></span> <span data-ttu-id="69c1d-238">Das- `[Authorize]` Attribut ermöglicht Ihnen das Sichern von Endpunkten der APP, für die eine Authentifizierung erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="69c1d-238">The `[Authorize]` attribute allows you to secure endpoints of the app which require authentication.</span></span> <span data-ttu-id="69c1d-239">Das- `[AllowAnonymous]` Attribut überschreibt das- `[Authorize]` Attribut in apps, die anonymen Zugriff zulassen.</span><span class="sxs-lookup"><span data-stu-id="69c1d-239">The `[AllowAnonymous]` attribute overrides the `[Authorize]` attribute in apps that allow anonymous access.</span></span> <span data-ttu-id="69c1d-240">Details zur Attribut Verwendung finden Sie unter <xref:security/authorization/simple> .</span><span class="sxs-lookup"><span data-stu-id="69c1d-240">For attribute usage details, see <xref:security/authorization/simple>.</span></span>

> [!NOTE]
> <span data-ttu-id="69c1d-241">Standardmäßig wird Benutzern, denen keine Autorisierung für den Zugriff auf eine Seite fehlt, eine leere HTTP 403-Antwort angezeigt.</span><span class="sxs-lookup"><span data-stu-id="69c1d-241">By default, users who lack authorization to access a page are presented with an empty HTTP 403 response.</span></span> <span data-ttu-id="69c1d-242">Die [Middleware "Status Codepages](xref:fundamentals/error-handling#usestatuscodepages) " kann so konfiguriert werden, dass Benutzern eine bessere "Zugriff verweigert"-Umgebung zur Verfügung steht.</span><span class="sxs-lookup"><span data-stu-id="69c1d-242">The [StatusCodePages Middleware](xref:fundamentals/error-handling#usestatuscodepages) can be configured to provide users with a better "Access Denied" experience.</span></span>

## <a name="impersonation"></a><span data-ttu-id="69c1d-243">Identitätswechsel</span><span class="sxs-lookup"><span data-stu-id="69c1d-243">Impersonation</span></span>

<span data-ttu-id="69c1d-244">ASP.net Core implementiert keinen Identitätswechsel.</span><span class="sxs-lookup"><span data-stu-id="69c1d-244">ASP.NET Core doesn't implement impersonation.</span></span> <span data-ttu-id="69c1d-245">Apps werden mit der APP-Identität für alle Anforderungen unter Verwendung des App-Pools oder der Prozess Identität ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="69c1d-245">Apps run with the app's identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="69c1d-246">Wenn die APP eine Aktion im Namen eines Benutzers ausführen soll, verwenden Sie [Windows Identity. runidentitäts](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) , das in einer [terminalinline-Middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) in verwendet wird `Startup.Configure` .</span><span class="sxs-lookup"><span data-stu-id="69c1d-246">If the app should perform an action on behalf of a user, use [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) in a [terminal inline middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) in `Startup.Configure`.</span></span> <span data-ttu-id="69c1d-247">Führen Sie eine einzelne Aktion in diesem Kontext aus, und schließen Sie dann den Kontext.</span><span class="sxs-lookup"><span data-stu-id="69c1d-247">Run a single action in this context and then close the context.</span></span>

[!code-csharp[](windowsauth/sample_snapshot/Startup.cs?highlight=10-19)]

<span data-ttu-id="69c1d-248">`RunImpersonated`unterstützt keine asynchronen Vorgänge und sollte nicht für komplexe Szenarien verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="69c1d-248">`RunImpersonated` doesn't support asynchronous operations and shouldn't be used for complex scenarios.</span></span> <span data-ttu-id="69c1d-249">Beispielsweise wird das Packen ganzer Anforderungen oder Middleware-Ketten nicht unterstützt oder empfohlen.</span><span class="sxs-lookup"><span data-stu-id="69c1d-249">For example, wrapping entire requests or middleware chains isn't supported or recommended.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="69c1d-250">Während das [Microsoft. aspnetcore. Authentication. Aushandlungs](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) -Paket die Authentifizierung unter Windows, Linux und macOS ermöglicht, wird der Identitätswechsel nur unter Windows unterstützt.</span><span class="sxs-lookup"><span data-stu-id="69c1d-250">While the [Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) package enables authentication on Windows, Linux, and macOS, impersonation is only supported on Windows.</span></span>

::: moniker-end

## <a name="claims-transformations"></a><span data-ttu-id="69c1d-251">Transformationen von Ansprüchen</span><span class="sxs-lookup"><span data-stu-id="69c1d-251">Claims transformations</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="69c1d-252">Beim Hosting mit IIS wird <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> nicht intern aufgerufen, um einen Benutzer zu initialisieren.</span><span class="sxs-lookup"><span data-stu-id="69c1d-252">When hosting with IIS, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="69c1d-253">Deshalb ist eine <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation>-Implementierung, die verwendet wird, um Ansprüche nach jeder Authentifizierung zu transformieren, nicht standardmäßig aktiviert.</span><span class="sxs-lookup"><span data-stu-id="69c1d-253">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="69c1d-254">Weitere Informationen und ein Codebeispiel, in dem Anspruchs Transformationen aktiviert werden, finden Sie unter <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model> .</span><span class="sxs-lookup"><span data-stu-id="69c1d-254">For more information and a code example that activates claims transformations, see <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="69c1d-255">Beim Hosting mit dem Prozess internen IIS-Modus wird <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> nicht intern aufgerufen, um einen Benutzer zu initialisieren.</span><span class="sxs-lookup"><span data-stu-id="69c1d-255">When hosting with IIS in-process mode, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="69c1d-256">Deshalb ist eine <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation>-Implementierung, die verwendet wird, um Ansprüche nach jeder Authentifizierung zu transformieren, nicht standardmäßig aktiviert.</span><span class="sxs-lookup"><span data-stu-id="69c1d-256">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="69c1d-257">Weitere Informationen und ein Codebeispiel zum Aktivieren von Anspruchs Transformationen beim Prozess internen Hosting finden Sie unter <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model> .</span><span class="sxs-lookup"><span data-stu-id="69c1d-257">For more information and a code example that activates claims transformations when hosting in-process, see <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="69c1d-258">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="69c1d-258">Additional resources</span></span>

* [<span data-ttu-id="69c1d-259">dotnet publish</span><span class="sxs-lookup"><span data-stu-id="69c1d-259">dotnet publish</span></span>](/dotnet/core/tools/dotnet-publish)
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/visual-studio-publish-profiles>
