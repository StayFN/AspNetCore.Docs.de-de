---
title: Sichern einer gehosteten Blazor WebAssembly-App in ASP.NET Core mit Identity Server
author: guardrex
description: In diesem Artikel erfahren Sie, wie Sie in Visual Studio eine neue von Blazor gehostete App mit Authentifizierung erstellen, die ein [IdentityServer](https://identityserver.io/)-Back-End verwendet.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/08/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/security/webassembly/hosted-with-identity-server
ms.openlocfilehash: 001fa0885c4ef4f365d9849278d3aa36e7657c54
ms.sourcegitcommit: f7873c02c1505c99106cbc708f37e18fc0a496d1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86147730"
---
# <a name="secure-an-aspnet-core-blazor-webassembly-hosted-app-with-identity-server"></a><span data-ttu-id="bc83e-103">Sichern einer gehosteten Blazor WebAssembly-App in ASP.NET Core mit Identity Server</span><span class="sxs-lookup"><span data-stu-id="bc83e-103">Secure an ASP.NET Core Blazor WebAssembly hosted app with Identity Server</span></span>

<span data-ttu-id="bc83e-104">Von [Javier Calvarro Nelson](https://github.com/javiercn) und [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="bc83e-104">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="bc83e-105">In diesem Artikel wird erläutert, wie eine neue von Blazor gehostete App erstellt wird, die [IdentityServer](https://identityserver.io/) verwendet, um Benutzer und API-Aufrufe zu authentifizieren.</span><span class="sxs-lookup"><span data-stu-id="bc83e-105">This article explains how to create a new Blazor hosted app that uses [IdentityServer](https://identityserver.io/) to authenticate users and API calls.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="bc83e-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bc83e-106">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="bc83e-107">So erstellen Sie ein neues Blazor WebAssembly-Projekt mit einem Authentifizierungsmechanismus:</span><span class="sxs-lookup"><span data-stu-id="bc83e-107">To create a new Blazor WebAssembly project with an authentication mechanism:</span></span>

1. <span data-ttu-id="bc83e-108">Nachdem Sie im Dialogfeld **Neue ASP.NET Core-Webanwendung erstellen** die Vorlage **Blazor WebAssembly-App** ausgewählt haben, wählen Sie im Dialogfeld **Authentifizierung** die Option **Ändern** aus.</span><span class="sxs-lookup"><span data-stu-id="bc83e-108">After choosing the **Blazor WebAssembly App** template in the **Create a new ASP.NET Core Web Application** dialog, select **Change** under **Authentication**.</span></span>

1. <span data-ttu-id="bc83e-109">Wählen Sie **Einzelne Benutzerkonten** mit der Option **In-App-Speicherung von Benutzerkonten** aus, um Benutzer über das [Identity](xref:security/authentication/identity)-System von ASP.NET Core innerhalb der App zu speichern.</span><span class="sxs-lookup"><span data-stu-id="bc83e-109">Select **Individual User Accounts** with the **Store user accounts in-app** option to store users within the app using ASP.NET Core's [Identity](xref:security/authentication/identity) system.</span></span>

1. <span data-ttu-id="bc83e-110">Aktivieren Sie im Bereich **Erweitert** das Kontrollkästchen **Von ASP.NET Core gehostet**.</span><span class="sxs-lookup"><span data-stu-id="bc83e-110">Select the **ASP.NET Core hosted** check box in the **Advanced** section.</span></span>

# <a name="visual-studio-code--net-core-cli"></a>[<span data-ttu-id="bc83e-111">Visual Studio Code / .NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="bc83e-111">Visual Studio Code / .NET Core CLI</span></span>](#tab/visual-studio-code+netcore-cli)

<span data-ttu-id="bc83e-112">Um ein neues Blazor WebAssembly-Projekt mit einem Authentifizierungsmechanismus in einem leeren Ordner zu erstellen, geben Sie den Authentifizierungsmechanismus `Individual` mit der `-au|--auth`Option an, Benutzer innerhalb der App über das [Identity](xref:security/authentication/identity)-System von ASP.NET Core zu speichern:</span><span class="sxs-lookup"><span data-stu-id="bc83e-112">To create a new Blazor WebAssembly project with an authentication mechanism in an empty folder, specify the `Individual` authentication mechanism with the `-au|--auth` option to store users within the app using ASP.NET Core's [Identity](xref:security/authentication/identity) system:</span></span>

```dotnetcli
dotnet new blazorwasm -au Individual -ho -o {APP NAME}
```

| <span data-ttu-id="bc83e-113">Platzhalter</span><span class="sxs-lookup"><span data-stu-id="bc83e-113">Placeholder</span></span>  | <span data-ttu-id="bc83e-114">Beispiel</span><span class="sxs-lookup"><span data-stu-id="bc83e-114">Example</span></span>        |
| ------------ | -------------- |
| `{APP NAME}` | `BlazorSample` |

<span data-ttu-id="bc83e-115">Der mit der Option `-o|--output` angegebene Ausgabespeicherort erstellt einen Projektordner, sofern kein solcher vorhanden ist, und wird Teil des Namens der App.</span><span class="sxs-lookup"><span data-stu-id="bc83e-115">The output location specified with the `-o|--output` option creates a project folder if it doesn't exist and becomes part of the app's name.</span></span>

<span data-ttu-id="bc83e-116">Weitere Informationen finden Sie im Befehl [`dotnet new`](/dotnet/core/tools/dotnet-new) im Leitfaden für .NET Core.</span><span class="sxs-lookup"><span data-stu-id="bc83e-116">For more information, see the [`dotnet new`](/dotnet/core/tools/dotnet-new) command in the .NET Core Guide.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="bc83e-117">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="bc83e-117">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="bc83e-118">So erstellen Sie ein neues Blazor WebAssembly-Projekt mit einem Authentifizierungsmechanismus:</span><span class="sxs-lookup"><span data-stu-id="bc83e-118">To create a new Blazor WebAssembly project with an authentication mechanism:</span></span>

1. <span data-ttu-id="bc83e-119">Wählen Sie im Schritt **Konfigurieren Ihrer neuen Blazor WebAssembly-App** in der Dropdownliste **Authentifizierung** die Option **Individual Authentication (in-app)** (Individuelle Authentifizierung [in der App]) aus.</span><span class="sxs-lookup"><span data-stu-id="bc83e-119">On the **Configure your new Blazor WebAssembly App** step, select **Individual Authentication (in-app)** from the **Authentication** drop down.</span></span>

1. <span data-ttu-id="bc83e-120">Die App wird für einzelne Benutzer erstellt, die über [Identity](xref:security/authentication/identity) von ASP.NET Core in der App gespeichert sind.</span><span class="sxs-lookup"><span data-stu-id="bc83e-120">The app is created for individual users stored in the app with ASP.NET Core [Identity](xref:security/authentication/identity).</span></span>

1. <span data-ttu-id="bc83e-121">Aktivieren Sie das Kontrollkästchen **Von ASP.NET Core gehostet**.</span><span class="sxs-lookup"><span data-stu-id="bc83e-121">Select the **ASP.NET Core hosted** check box.</span></span>

---

## <a name="server-app-configuration"></a><span data-ttu-id="bc83e-122">Server-App-Konfiguration</span><span class="sxs-lookup"><span data-stu-id="bc83e-122">Server app configuration</span></span>

<span data-ttu-id="bc83e-123">In den folgenden Abschnitten werden Ergänzungen zum Projekt erläutert, wenn die Authentifizierungsunterstützung eingeschlossen wird.</span><span class="sxs-lookup"><span data-stu-id="bc83e-123">The following sections describe additions to the project when authentication support is included.</span></span>

### <a name="startup-class"></a><span data-ttu-id="bc83e-124">Startklasse</span><span class="sxs-lookup"><span data-stu-id="bc83e-124">Startup class</span></span>

<span data-ttu-id="bc83e-125">Die `Startup`-Klasse verfügt über die folgenden Ergänzungen.</span><span class="sxs-lookup"><span data-stu-id="bc83e-125">The `Startup` class has the following additions.</span></span>

* <span data-ttu-id="bc83e-126">In `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="bc83e-126">In `Startup.ConfigureServices`:</span></span>

  * <span data-ttu-id="bc83e-127">ASP.NET Core Identity:</span><span class="sxs-lookup"><span data-stu-id="bc83e-127">ASP.NET Core Identity:</span></span>

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlite(
            Configuration.GetConnectionString("DefaultConnection")));

    services.AddDefaultIdentity<ApplicationUser>(options => 
            options.SignIn.RequireConfirmedAccount = true)
        .AddEntityFrameworkStores<ApplicationDbContext>();
    ```

  * Identity<span data-ttu-id="bc83e-128">Server mit einer zusätzlichen <xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A>-Hilfsmethode, die ASP.NET Core-Standardkonventionen zusätzlich zu IdentityServer einrichtet:</span><span class="sxs-lookup"><span data-stu-id="bc83e-128">Server with an additional <xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A> helper method that sets up default ASP.NET Core conventions on top of IdentityServer:</span></span>

    ```csharp
    services.AddIdentityServer()
        .AddApiAuthorization<ApplicationUser, ApplicationDbContext>();
    ```

  * <span data-ttu-id="bc83e-129">Authentifizierung mit einer zusätzlichen <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A>-Hilfsmethode, die die App so konfiguriert, dass sie von IdentityServer generierte JWT-Token überprüft:</span><span class="sxs-lookup"><span data-stu-id="bc83e-129">Authentication with an additional <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> helper method that configures the app to validate JWT tokens produced by IdentityServer:</span></span>

    ```csharp
    services.AddAuthentication()
        .AddIdentityServerJwt();
    ```

* <span data-ttu-id="bc83e-130">In `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="bc83e-130">In `Startup.Configure`:</span></span>

  * <span data-ttu-id="bc83e-131">Die IdentityServer-Middleware macht die Open ID Connect-Endpunkte (OIDC) verfügbar:</span><span class="sxs-lookup"><span data-stu-id="bc83e-131">The IdentityServer middleware exposes the Open ID Connect (OIDC) endpoints:</span></span>

    ```csharp
    app.UseIdentityServer();
    ```

  * <span data-ttu-id="bc83e-132">Die Authentifizierungsmiddleware ist für die Überprüfung der Anforderungsanmeldeinformationen und für das Festlegen des Benutzers auf den Anforderungskontext verantwortlich:</span><span class="sxs-lookup"><span data-stu-id="bc83e-132">The Authentication middleware is responsible for validating request credentials and setting the user on the request context:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

  * <span data-ttu-id="bc83e-133">Die Autorisierungsmiddleware aktiviert Autorisierungsfunktionen:</span><span class="sxs-lookup"><span data-stu-id="bc83e-133">Authorization Middleware enables authorization capabilities:</span></span>

    ```csharp
    app.UseAuthentication();
    app.UseAuthorization();
    ```

### <a name="addapiauthorization"></a><span data-ttu-id="bc83e-134">AddApiAuthorization</span><span class="sxs-lookup"><span data-stu-id="bc83e-134">AddApiAuthorization</span></span>

<span data-ttu-id="bc83e-135">Die <xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A>-Hilfsmethode konfiguriert [IdentityServer](https://identityserver.io/) für ASP.NET Core-Szenarien.</span><span class="sxs-lookup"><span data-stu-id="bc83e-135">The <xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A> helper method configures [IdentityServer](https://identityserver.io/) for ASP.NET Core scenarios.</span></span> Identity<span data-ttu-id="bc83e-136">Server ist ein leistungsfähiges und erweiterbares Framework für Überlegungen zum Thema „App-Sicherheit“.</span><span class="sxs-lookup"><span data-stu-id="bc83e-136">Server is a powerful and extensible framework for handling app security concerns.</span></span> Identity<span data-ttu-id="bc83e-137">Server ist für die meisten gängigen Szenarien unnötig komplex.</span><span class="sxs-lookup"><span data-stu-id="bc83e-137">Server exposes unnecessary complexity for the most common scenarios.</span></span> <span data-ttu-id="bc83e-138">Als Folge werden mehrere Konventionen und Konfigurationsoptionen bereitgestellt, die sich gut als Startpunkt eignen.</span><span class="sxs-lookup"><span data-stu-id="bc83e-138">Consequently, a set of conventions and configuration options is provided that we consider a good starting point.</span></span> <span data-ttu-id="bc83e-139">Wenn sich Ihre Anforderungen an die Authentifizierung ändern, bietet IdentityServer eine Vielzahl leistungsfähiger Funktionen, mit denen Sie die Authentifizierung genau an die Anforderungen einer App anpassen können.</span><span class="sxs-lookup"><span data-stu-id="bc83e-139">Once your authentication needs change, the full power of IdentityServer is available to customize authentication to suit an app's requirements.</span></span>

### <a name="addidentityserverjwt"></a><span data-ttu-id="bc83e-140">AddIdentityServerJwt</span><span class="sxs-lookup"><span data-stu-id="bc83e-140">AddIdentityServerJwt</span></span>

<span data-ttu-id="bc83e-141">Die <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A>-Hilfsprogrammmethode konfiguriert ein Richtlinienschema für die App als Standardauthentifizierungshandler.</span><span class="sxs-lookup"><span data-stu-id="bc83e-141">The <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> helper method configures a policy scheme for the app as the default authentication handler.</span></span> <span data-ttu-id="bc83e-142">Die Richtlinie ist so konfiguriert, dass Identity alle an beliebige Unterpfade im Identity-URL-Raum `/Identity` weitergeleiteten Anforderungen verarbeiten kann.</span><span class="sxs-lookup"><span data-stu-id="bc83e-142">The policy is configured to allow Identity to handle all requests routed to any subpath in the Identity URL space `/Identity`.</span></span> <span data-ttu-id="bc83e-143"><xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerHandler> verarbeitet alle anderen Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="bc83e-143">The <xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerHandler> handles all other requests.</span></span> <span data-ttu-id="bc83e-144">Außerdem übernimmt diese Methode die folgenden Aufgaben:</span><span class="sxs-lookup"><span data-stu-id="bc83e-144">Additionally, this method:</span></span>

* <span data-ttu-id="bc83e-145">Registrieren einer `{APPLICATION NAME}API`-API-Ressource mit IdentityServer mit dem Standardbereich `{APPLICATION NAME}API`</span><span class="sxs-lookup"><span data-stu-id="bc83e-145">Registers an `{APPLICATION NAME}API` API resource with IdentityServer with a default scope of `{APPLICATION NAME}API`.</span></span>
* <span data-ttu-id="bc83e-146">Konfigurieren der JWT-Bearertoken-Middleware zum Überprüfen der von IdentityServer für die App ausgegebenen Token</span><span class="sxs-lookup"><span data-stu-id="bc83e-146">Configures the JWT Bearer Token Middleware to validate tokens issued by IdentityServer for the app.</span></span>

### <a name="weatherforecastcontroller"></a><span data-ttu-id="bc83e-147">WeatherForecastController</span><span class="sxs-lookup"><span data-stu-id="bc83e-147">WeatherForecastController</span></span>

<span data-ttu-id="bc83e-148">In `WeatherForecastController` (`Controllers/WeatherForecastController.cs`) wird das [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute)-Attribut auf die Klasse angewendet.</span><span class="sxs-lookup"><span data-stu-id="bc83e-148">In the `WeatherForecastController` (`Controllers/WeatherForecastController.cs`), the [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) attribute is applied to the class.</span></span> <span data-ttu-id="bc83e-149">Das Attribut gibt an, dass der Benutzer basierend auf der Standardrichtlinie autorisiert werden muss, um auf die Ressource zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="bc83e-149">The attribute indicates that the user must be authorized based on the default policy to access the resource.</span></span> <span data-ttu-id="bc83e-150">Die Standardautorisierungsrichtlinie ist so konfiguriert, dass das Standardauthentifizierungsschema verwendet wird, das von <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> eingerichtet wird.</span><span class="sxs-lookup"><span data-stu-id="bc83e-150">The default authorization policy is configured to use the default authentication scheme, which is set up by <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A>.</span></span> <span data-ttu-id="bc83e-151">Die Hilfsprogrammmethode konfiguriert <xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerHandler> als Standardhandler für Anforderungen an die App.</span><span class="sxs-lookup"><span data-stu-id="bc83e-151">The helper method configures <xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerHandler> as the default handler for requests to the app.</span></span>

### <a name="applicationdbcontext"></a><span data-ttu-id="bc83e-152">ApplicationDbContext</span><span class="sxs-lookup"><span data-stu-id="bc83e-152">ApplicationDbContext</span></span>

<span data-ttu-id="bc83e-153">In `ApplicationDbContext` (`Data/ApplicationDbContext.cs`) wird <xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiAuthorizationDbContext%601> durch <xref:Microsoft.EntityFrameworkCore.DbContext> erweitert, damit das Schema für IdentityServer eingeschlossen wird.</span><span class="sxs-lookup"><span data-stu-id="bc83e-153">In the `ApplicationDbContext` (`Data/ApplicationDbContext.cs`), <xref:Microsoft.EntityFrameworkCore.DbContext> extends <xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiAuthorizationDbContext%601> to include the schema for IdentityServer.</span></span> <span data-ttu-id="bc83e-154"><xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiAuthorizationDbContext%601> wird von <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext> abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="bc83e-154"><xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiAuthorizationDbContext%601> is derived from <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext>.</span></span>

<span data-ttu-id="bc83e-155">Für eine vollständige Kontrolle des Datenbankschemas wird von einer der verfügbaren Identity-<xref:Microsoft.EntityFrameworkCore.DbContext>-Klassen geerbt, und der Kontext wird so konfiguriert, dass er das Identity-Schema einschließt, indem `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` in der <xref:Microsoft.EntityFrameworkCore.DbContext.OnModelCreating%2A>-Methode aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="bc83e-155">To gain full control of the database schema, inherit from one of the available Identity <xref:Microsoft.EntityFrameworkCore.DbContext> classes and configure the context to include the Identity schema by calling `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` in the <xref:Microsoft.EntityFrameworkCore.DbContext.OnModelCreating%2A> method.</span></span>

### <a name="oidcconfigurationcontroller"></a><span data-ttu-id="bc83e-156">OidcConfigurationController</span><span class="sxs-lookup"><span data-stu-id="bc83e-156">OidcConfigurationController</span></span>

<span data-ttu-id="bc83e-157">In `OidcConfigurationController` (`Controllers/OidcConfigurationController.cs`) wird der Clientendpunkt bereitgestellt, um OIDC-Parameter bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="bc83e-157">In the `OidcConfigurationController` (`Controllers/OidcConfigurationController.cs`), the client endpoint is provisioned to serve OIDC parameters.</span></span>

### <a name="app-settings-files"></a><span data-ttu-id="bc83e-158">App-Einstellungsdateien</span><span class="sxs-lookup"><span data-stu-id="bc83e-158">App settings files</span></span>

<span data-ttu-id="bc83e-159">In der App-Einstellungsdatei (`appsettings.json`) beschreibt der Bereich `IdentityServer` auf Projektstammebene die Liste der konfigurierten Clients.</span><span class="sxs-lookup"><span data-stu-id="bc83e-159">In the app settings file (`appsettings.json`) at the project root, the `IdentityServer` section describes the list of configured clients.</span></span> <span data-ttu-id="bc83e-160">Im folgenden Beispiel gibt es einen einzelnen Client.</span><span class="sxs-lookup"><span data-stu-id="bc83e-160">In the following example, there's a single client.</span></span> <span data-ttu-id="bc83e-161">Der Clientname entspricht dem App-Namen, und er wird durch Konventionen dem OAuth-`ClientId`-Parameter zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="bc83e-161">The client name corresponds to the app name and is mapped by convention to the OAuth `ClientId` parameter.</span></span> <span data-ttu-id="bc83e-162">Das Profil gibt den App-Typ an, der konfiguriert wird.</span><span class="sxs-lookup"><span data-stu-id="bc83e-162">The profile indicates the app type being configured.</span></span> <span data-ttu-id="bc83e-163">Das Profil wird intern verwendet, um Konventionen zu etablieren, die den Konfigurationsprozess für den Server vereinfachen.</span><span class="sxs-lookup"><span data-stu-id="bc83e-163">The profile is used internally to drive conventions that simplify the configuration process for the server.</span></span> <!-- There are several profiles available, as explained in the [Application profiles](#application-profiles) section. -->

```json
"IdentityServer": {
  "Clients": {
    "{APP ASSEMBLY}.Client": {
      "Profile": "IdentityServerSPA"
    }
  }
}
```

<span data-ttu-id="bc83e-164">Der Platzhalter `{APP ASSEMBLY}` ist der Assemblyname der App (z. B. `BlazorSample.Client`).</span><span class="sxs-lookup"><span data-stu-id="bc83e-164">The placeholder `{APP ASSEMBLY}` is the app's assembly name (for example, `BlazorSample.Client`).</span></span>

## <a name="client-app-configuration"></a><span data-ttu-id="bc83e-165">Client-App-Konfiguration</span><span class="sxs-lookup"><span data-stu-id="bc83e-165">Client app configuration</span></span>

### <a name="authentication-package"></a><span data-ttu-id="bc83e-166">Authentifizierungspaket</span><span class="sxs-lookup"><span data-stu-id="bc83e-166">Authentication package</span></span>

<span data-ttu-id="bc83e-167">Wenn eine App erstellt wird, die einzelne Benutzerkonten verwendet (`Individual`), erhält die App automatisch einen Paketverweis für das [`Microsoft.AspNetCore.Components.WebAssembly.Authentication`](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Authentication/)-Paket in der Projektdatei der App.</span><span class="sxs-lookup"><span data-stu-id="bc83e-167">When an app is created to use Individual User Accounts (`Individual`), the app automatically receives a package reference for the [`Microsoft.AspNetCore.Components.WebAssembly.Authentication`](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Authentication/) package in the app's project file.</span></span> <span data-ttu-id="bc83e-168">Das Paket stellt einige Primitive bereit, die der App beim Authentifizieren von Benutzern und beim Abrufen von Token zum Aufrufen geschützter APIs helfen.</span><span class="sxs-lookup"><span data-stu-id="bc83e-168">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="bc83e-169">Wenn Sie einer App eine Authentifizierung hinzufügen, fügen Sie das Paket manuell der Projektdatei der App hinzu:</span><span class="sxs-lookup"><span data-stu-id="bc83e-169">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference 
  Include="Microsoft.AspNetCore.Components.WebAssembly.Authentication" 
  Version="3.2.0" />
```

### <a name="api-authorization-support"></a><span data-ttu-id="bc83e-170">API-Autorisierungsunterstützung</span><span class="sxs-lookup"><span data-stu-id="bc83e-170">API authorization support</span></span>

<span data-ttu-id="bc83e-171">Die Unterstützung für die Authentifizierung von Benutzern wird im Dienstcontainer von der im [`Microsoft.AspNetCore.Components.WebAssembly.Authentication`](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Authentication/)-Paket bereitgestellten -Erweiterungsmethode eingefügt.</span><span class="sxs-lookup"><span data-stu-id="bc83e-171">The support for authenticating users is plugged into the service container by the extension method provided inside the [`Microsoft.AspNetCore.Components.WebAssembly.Authentication`](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Authentication/) package.</span></span> <span data-ttu-id="bc83e-172">Diese Methode richtet die Dienste ein, die erforderlich sind, damit die App mit dem vorhandenen Autorisierungssystem interagiert.</span><span class="sxs-lookup"><span data-stu-id="bc83e-172">This method sets up the services required by the app to interact with the existing authorization system.</span></span>

```csharp
builder.Services.AddApiAuthorization();
```

<span data-ttu-id="bc83e-173">Standardmäßig wird die Konfiguration für die App durch die Konvention von `_configuration/{client-id}` geladen.</span><span class="sxs-lookup"><span data-stu-id="bc83e-173">By default, configuration for the app is loaded by convention from `_configuration/{client-id}`.</span></span> <span data-ttu-id="bc83e-174">Per Konvention wird die Client-ID auf den Assemblynamen der App festgelegt.</span><span class="sxs-lookup"><span data-stu-id="bc83e-174">By convention, the client ID is set to the app's assembly name.</span></span> <span data-ttu-id="bc83e-175">Diese URL kann so geändert werden, dass sie auf einen eigenen Endpunkt verweist, indem das Überladen mit Optionen aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="bc83e-175">This URL can be changed to point to a separate endpoint by calling the overload with options.</span></span>

### <a name="imports-file"></a><span data-ttu-id="bc83e-176">Imports-Datei</span><span class="sxs-lookup"><span data-stu-id="bc83e-176">Imports file</span></span>

[!INCLUDE[](~/includes/blazor-security/imports-file-hosted.md)]

### <a name="index-page"></a><span data-ttu-id="bc83e-177">Indexseite</span><span class="sxs-lookup"><span data-stu-id="bc83e-177">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page-authentication.md)]

### <a name="app-component"></a><span data-ttu-id="bc83e-178">App-Komponente</span><span class="sxs-lookup"><span data-stu-id="bc83e-178">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

### <a name="redirecttologin-component"></a><span data-ttu-id="bc83e-179">RedirectToLogin-Komponente</span><span class="sxs-lookup"><span data-stu-id="bc83e-179">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

### <a name="logindisplay-component"></a><span data-ttu-id="bc83e-180">LoginDisplay-Komponente</span><span class="sxs-lookup"><span data-stu-id="bc83e-180">LoginDisplay component</span></span>

<span data-ttu-id="bc83e-181">Die `LoginDisplay`-Komponente (`Shared/LoginDisplay.razor`) wird in der `MainLayout`-Komponente (`Shared/MainLayout.razor`) gerendert, und sie verwaltet die folgenden Verhaltensweisen:</span><span class="sxs-lookup"><span data-stu-id="bc83e-181">The `LoginDisplay` component (`Shared/LoginDisplay.razor`) is rendered in the `MainLayout` component (`Shared/MainLayout.razor`) and manages the following behaviors:</span></span>

* <span data-ttu-id="bc83e-182">Für authentifizierte Benutzer:</span><span class="sxs-lookup"><span data-stu-id="bc83e-182">For authenticated users:</span></span>
  * <span data-ttu-id="bc83e-183">Zeigt den aktuellen Benutzernamen an</span><span class="sxs-lookup"><span data-stu-id="bc83e-183">Displays the current user name.</span></span>
  * <span data-ttu-id="bc83e-184">Stellt einen Link zur Benutzerprofilseite in ASP.NET Core-Identity bereit</span><span class="sxs-lookup"><span data-stu-id="bc83e-184">Offers a link to the user profile page in ASP.NET Core Identity.</span></span>
  * <span data-ttu-id="bc83e-185">Bietet eine Schaltfläche zum Abmelden von der App</span><span class="sxs-lookup"><span data-stu-id="bc83e-185">Offers a button to log out of the app.</span></span>
* <span data-ttu-id="bc83e-186">Für anonyme Benutzer:</span><span class="sxs-lookup"><span data-stu-id="bc83e-186">For anonymous users:</span></span>
  * <span data-ttu-id="bc83e-187">Bietet die Option zum Registrieren</span><span class="sxs-lookup"><span data-stu-id="bc83e-187">Offers the option to register.</span></span>
  * <span data-ttu-id="bc83e-188">Bietet die Option zur Anmeldung</span><span class="sxs-lookup"><span data-stu-id="bc83e-188">Offers the option to log in.</span></span>

```razor
@using Microsoft.AspNetCore.Components.Authorization
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication
@inject NavigationManager Navigation
@inject SignOutSessionStateManager SignOutManager

<AuthorizeView>
    <Authorized>
        <a href="authentication/profile">Hello, @context.User.Identity.Name!</a>
        <button class="nav-link btn btn-link" @onclick="BeginSignOut">
            Log out
        </button>
    </Authorized>
    <NotAuthorized>
        <a href="authentication/register">Register</a>
        <a href="authentication/login">Log in</a>
    </NotAuthorized>
</AuthorizeView>

@code {
    private async Task BeginSignOut(MouseEventArgs args)
    {
        await SignOutManager.SetSignOutState();
        Navigation.NavigateTo("authentication/logout");
    }
}
```

### <a name="authentication-component"></a><span data-ttu-id="bc83e-189">Authentication-Komponente</span><span class="sxs-lookup"><span data-stu-id="bc83e-189">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

### <a name="fetchdata-component"></a><span data-ttu-id="bc83e-190">FetchData-Komponente</span><span class="sxs-lookup"><span data-stu-id="bc83e-190">FetchData component</span></span>

[!INCLUDE[](~/includes/blazor-security/fetchdata-component.md)]

## <a name="run-the-app"></a><span data-ttu-id="bc83e-191">Ausführen der App</span><span class="sxs-lookup"><span data-stu-id="bc83e-191">Run the app</span></span>

<span data-ttu-id="bc83e-192">Führen Sie die App aus dem Serverprojekt aus.</span><span class="sxs-lookup"><span data-stu-id="bc83e-192">Run the app from the Server project.</span></span> <span data-ttu-id="bc83e-193">Wenn Sie Visual Studio verwenden, haben Sie folgende beiden Möglichkeiten:</span><span class="sxs-lookup"><span data-stu-id="bc83e-193">When using Visual Studio, either:</span></span>

* <span data-ttu-id="bc83e-194">Wählen Sie in der Dropdownliste **Startprojekte** in der Symbolleiste die *Server-API-App* aus, und klicken Sie auf **Ausführen**.</span><span class="sxs-lookup"><span data-stu-id="bc83e-194">Set the **Startup Projects** drop down list in the toolbar to the *Server API app* and select the **Run** button.</span></span>
* <span data-ttu-id="bc83e-195">Wählen Sie das Serverprojekt im **Projektmappen-Explorer** aus, und klicken Sie in der Symbolleiste auf **Ausführen**, oder starten Sie die App über das Menü **Debuggen**.</span><span class="sxs-lookup"><span data-stu-id="bc83e-195">Select the Server project in **Solution Explorer** and select the **Run** button in the toolbar or start the app from the **Debug** menu.</span></span>

## <a name="name-and-role-claim-with-api-authorization"></a><span data-ttu-id="bc83e-196">Namens- und Rollenanspruch mit API-Autorisierung</span><span class="sxs-lookup"><span data-stu-id="bc83e-196">Name and role claim with API authorization</span></span>

### <a name="custom-user-factory"></a><span data-ttu-id="bc83e-197">Benutzerdefinierte Benutzerfactory</span><span class="sxs-lookup"><span data-stu-id="bc83e-197">Custom user factory</span></span>

<span data-ttu-id="bc83e-198">Erstellen Sie in der Client-App eine benutzerdefinierte Benutzerfactory.</span><span class="sxs-lookup"><span data-stu-id="bc83e-198">In the Client app, create a custom user factory.</span></span> Identity<span data-ttu-id="bc83e-199">Server sendet mehrere Rollen als JSON-Array in einem einzelnen `role`-Anspruch.</span><span class="sxs-lookup"><span data-stu-id="bc83e-199"> Server sends multiple roles as a JSON array in a single `role` claim.</span></span> <span data-ttu-id="bc83e-200">Eine einzelne Rolle wird im Anspruch als Zeichenfolgenwert gesendet.</span><span class="sxs-lookup"><span data-stu-id="bc83e-200">A single role is sent as a string value in the claim.</span></span> <span data-ttu-id="bc83e-201">Die Factory erstellt einen einzelnen `role`-Anspruch für die einzelnen Rollen des Benutzers.</span><span class="sxs-lookup"><span data-stu-id="bc83e-201">The factory creates an individual `role` claim for each of the user's roles.</span></span>

<span data-ttu-id="bc83e-202">`CustomUserFactory.cs`:</span><span class="sxs-lookup"><span data-stu-id="bc83e-202">`CustomUserFactory.cs`:</span></span>

```csharp
using System.Linq;
using System.Security.Claims;
using System.Text.Json;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Components.WebAssembly.Authentication;
using Microsoft.AspNetCore.Components.WebAssembly.Authentication.Internal;

public class CustomUserFactory
    : AccountClaimsPrincipalFactory<RemoteUserAccount>
{
    public CustomUserFactory(IAccessTokenProviderAccessor accessor)
        : base(accessor)
    {
    }

    public async override ValueTask<ClaimsPrincipal> CreateUserAsync(
        RemoteUserAccount account,
        RemoteAuthenticationUserOptions options)
    {
        var user = await base.CreateUserAsync(account, options);

        if (user.Identity.IsAuthenticated)
        {
            var identity = (ClaimsIdentity)user.Identity;
            var roleClaims = identity.FindAll(identity.RoleClaimType);

            if (roleClaims != null && roleClaims.Any())
            {
                foreach (var existingClaim in roleClaims)
                {
                    identity.RemoveClaim(existingClaim);
                }

                var rolesElem = account.AdditionalProperties[identity.RoleClaimType];

                if (rolesElem is JsonElement roles)
                {
                    if (roles.ValueKind == JsonValueKind.Array)
                    {
                        foreach (var role in roles.EnumerateArray())
                        {
                            identity.AddClaim(new Claim(options.RoleClaim, role.GetString()));
                        }
                    }
                    else
                    {
                        identity.AddClaim(new Claim(options.RoleClaim, roles.GetString()));
                    }
                }
            }
        }

        return user;
    }
}
```

<span data-ttu-id="bc83e-203">Registrieren Sie in der Client-App die Factory in `Program.Main` (`Program.cs`):</span><span class="sxs-lookup"><span data-stu-id="bc83e-203">In the Client app, register the factory in `Program.Main` (`Program.cs`):</span></span>

```csharp
builder.Services.AddApiAuthorization()
    .AddAccountClaimsPrincipalFactory<CustomUserFactory>();
```

<span data-ttu-id="bc83e-204">Rufen Sie in der Server-App <xref:Microsoft.AspNetCore.Identity.IdentityBuilder.AddRoles*> für den Identity-Generator auf, der rollenbezogene Dienste hinzufügt:</span><span class="sxs-lookup"><span data-stu-id="bc83e-204">In the Server app, call <xref:Microsoft.AspNetCore.Identity.IdentityBuilder.AddRoles*> on the Identity builder, which adds role-related services:</span></span>

```csharp
using Microsoft.AspNetCore.Identity;

...

services.AddDefaultIdentity<ApplicationUser>(options => 
    options.SignIn.RequireConfirmedAccount = true)
    .AddRoles<IdentityRole>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

### <a name="configure-identity-server"></a><span data-ttu-id="bc83e-205">Konfigurieren von IdentityServer</span><span class="sxs-lookup"><span data-stu-id="bc83e-205">Configure Identity Server</span></span>

<span data-ttu-id="bc83e-206">Verwenden Sie **einen** der folgenden Ansätze:</span><span class="sxs-lookup"><span data-stu-id="bc83e-206">Use **one** of the following approaches:</span></span>

* [<span data-ttu-id="bc83e-207">API-Autorisierungsoptionen</span><span class="sxs-lookup"><span data-stu-id="bc83e-207">API authorization options</span></span>](#api-authorization-options)
* [<span data-ttu-id="bc83e-208">Profildienst</span><span class="sxs-lookup"><span data-stu-id="bc83e-208">Profile Service</span></span>](#profile-service)

#### <a name="api-authorization-options"></a><span data-ttu-id="bc83e-209">API-Autorisierungsoptionen</span><span class="sxs-lookup"><span data-stu-id="bc83e-209">API authorization options</span></span>

<span data-ttu-id="bc83e-210">Führen Sie in der Server-App die folgenden Aktionen aus:</span><span class="sxs-lookup"><span data-stu-id="bc83e-210">In the Server app:</span></span>

* <span data-ttu-id="bc83e-211">Konfigurieren Sie IdentityServer so, dass der `name`- und der `role`-Anspruch in das ID-Token und das Zugriffstoken platziert werden.</span><span class="sxs-lookup"><span data-stu-id="bc83e-211">Configure Identity Server to put the `name` and `role` claims into the ID token and access token.</span></span>
* <span data-ttu-id="bc83e-212">Verhindern Sie die Standardzuordnung für Rollen im JWT-Tokenhandler.</span><span class="sxs-lookup"><span data-stu-id="bc83e-212">Prevent the default mapping for roles in the JWT token handler.</span></span>

```csharp
using System.IdentityModel.Tokens.Jwt;
using System.Linq;

...

services.AddIdentityServer()
    .AddApiAuthorization<ApplicationUser, ApplicationDbContext>(options => {
        options.IdentityResources["openid"].UserClaims.Add("name");
        options.ApiResources.Single().UserClaims.Add("name");
        options.IdentityResources["openid"].UserClaims.Add("role");
        options.ApiResources.Single().UserClaims.Add("role");
    });

JwtSecurityTokenHandler.DefaultInboundClaimTypeMap.Remove("role");
```

#### <a name="profile-service"></a><span data-ttu-id="bc83e-213">Profildienst</span><span class="sxs-lookup"><span data-stu-id="bc83e-213">Profile Service</span></span>

<span data-ttu-id="bc83e-214">Erstellen Sie in der Server-App eine `ProfileService`-Implementierung.</span><span class="sxs-lookup"><span data-stu-id="bc83e-214">In the Server app, create a `ProfileService` implementation.</span></span>

<span data-ttu-id="bc83e-215">`ProfileService.cs`:</span><span class="sxs-lookup"><span data-stu-id="bc83e-215">`ProfileService.cs`:</span></span>

```csharp
using IdentityModel;
using IdentityServer4.Models;
using IdentityServer4.Services;
using System.Threading.Tasks;

public class ProfileService : IProfileService
{
    public ProfileService()
    {
    }

    public Task GetProfileDataAsync(ProfileDataRequestContext context)
    {
        var nameClaim = context.Subject.FindAll(JwtClaimTypes.Name);
        context.IssuedClaims.AddRange(nameClaim);

        var roleClaims = context.Subject.FindAll(JwtClaimTypes.Role);
        context.IssuedClaims.AddRange(roleClaims);

        return Task.CompletedTask;
    }

    public Task IsActiveAsync(IsActiveContext context)
    {
        return Task.CompletedTask;
    }
}
```

<span data-ttu-id="bc83e-216">Registrieren Sie in der Server-App den Profildienst in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="bc83e-216">In the Server app, register the Profile Service in `Startup.ConfigureServices`:</span></span>

```csharp
using IdentityServer4.Services;

...

services.AddTransient<IProfileService, ProfileService>();
```

### <a name="use-authorization-mechanisms"></a><span data-ttu-id="bc83e-217">Verwenden der Autorisierungsmechanismen</span><span class="sxs-lookup"><span data-stu-id="bc83e-217">Use authorization mechanisms</span></span>

<span data-ttu-id="bc83e-218">Die Komponentenautorisierungsansätze in der Client-App sind Stand jetzt funktional.</span><span class="sxs-lookup"><span data-stu-id="bc83e-218">In the Client app, component authorization approaches are functional at this point.</span></span> <span data-ttu-id="bc83e-219">Jeder der Autorisierungsmechanismen bei den Komponenten kann eine Rolle verwenden, um den Benutzer zu autorisieren:</span><span class="sxs-lookup"><span data-stu-id="bc83e-219">Any of the authorization mechanisms in components can use a role to authorize the user:</span></span>

* <span data-ttu-id="bc83e-220">[`AuthorizeView`-Komponente](xref:blazor/security/index#authorizeview-component) (Beispiel: `<AuthorizeView Roles="admin">`)</span><span class="sxs-lookup"><span data-stu-id="bc83e-220">[`AuthorizeView` component](xref:blazor/security/index#authorizeview-component) (Example: `<AuthorizeView Roles="admin">`)</span></span>
* <span data-ttu-id="bc83e-221">[`[Authorize]`-Attributanweisung](xref:blazor/security/index#authorize-attribute) (<xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute>) (Beispiel: `@attribute [Authorize(Roles = "admin")]`)</span><span class="sxs-lookup"><span data-stu-id="bc83e-221">[`[Authorize]` attribute directive](xref:blazor/security/index#authorize-attribute) (<xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute>) (Example: `@attribute [Authorize(Roles = "admin")]`)</span></span>
* <span data-ttu-id="bc83e-222">[Prozedurale Logik](xref:blazor/security/index#procedural-logic) (Beispiel: `if (user.IsInRole("admin")) { ... }`)</span><span class="sxs-lookup"><span data-stu-id="bc83e-222">[Procedural logic](xref:blazor/security/index#procedural-logic) (Example: `if (user.IsInRole("admin")) { ... }`)</span></span>

  <span data-ttu-id="bc83e-223">Mehrere Rollentests werden unterstützt:</span><span class="sxs-lookup"><span data-stu-id="bc83e-223">Multiple role tests are supported:</span></span>

  ```csharp
  if (user.IsInRole("admin") && user.IsInRole("developer"))
  {
      ...
  }
  ```

<span data-ttu-id="bc83e-224">`User.Identity.Name` wird in der Client-App mit dem Benutzernamen des Benutzers aufgefüllt. In der Regel handelt es sich dabei um die E-Mail-Adresse für die Anmeldung.</span><span class="sxs-lookup"><span data-stu-id="bc83e-224">`User.Identity.Name` is populated in the Client app with the user's user name, which is usually their sign-in email address.</span></span>

[!INCLUDE[](~/includes/blazor-security/usermanager-signinmanager.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="bc83e-225">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="bc83e-225">Additional resources</span></span>

* [<span data-ttu-id="bc83e-226">Bereitstellen für die Produktion</span><span class="sxs-lookup"><span data-stu-id="bc83e-226">Deployment to Azure App Service</span></span>](xref:security/authentication/identity/spa#deploy-to-production)
* [<span data-ttu-id="bc83e-227">Importieren eines Zertifikats aus Key Vault</span><span class="sxs-lookup"><span data-stu-id="bc83e-227">Import a certificate from Key Vault (Azure documentation)</span></span>](/azure/app-service/configure-ssl-certificate#import-a-certificate-from-key-vault)
* <xref:blazor/security/webassembly/additional-scenarios>
* [<span data-ttu-id="bc83e-228">Nicht authentifizierte oder nicht autorisierte Web-API-Anforderungen in einer App mit einem sicheren Standardclient</span><span class="sxs-lookup"><span data-stu-id="bc83e-228">Unauthenticated or unauthorized web API requests in an app with a secure default client</span></span>](xref:blazor/security/webassembly/additional-scenarios#unauthenticated-or-unauthorized-web-api-requests-in-an-app-with-a-secure-default-client)
