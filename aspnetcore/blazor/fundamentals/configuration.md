---
title: Blazor-Konfiguration in ASP.NET Core
author: guardrex
description: In diesem Artikel erhalten Sie weitere Informationen zur Konfiguration von Blazor-Apps, einschließlich der App-Einstellungen, der Authentifizierung und Protokollierungskonfiguration.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/10/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/fundamentals/configuration
ms.openlocfilehash: 9c83784676e061664b2ae980faadcbe0f84c9985
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.contentlocale: de-DE
ms.lasthandoff: 06/26/2020
ms.locfileid: "85402922"
---
# <a name="aspnet-core-blazor-configuration"></a><span data-ttu-id="3e5ad-103">Blazor-Konfiguration in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3e5ad-103">ASP.NET Core Blazor configuration</span></span>

> [!NOTE]
> <span data-ttu-id="3e5ad-104">Dieses Thema gilt für Blazor WebAssembly.</span><span class="sxs-lookup"><span data-stu-id="3e5ad-104">This topic applies to Blazor WebAssembly.</span></span> <span data-ttu-id="3e5ad-105">Eine allgemeine Anleitung zur App-Konfiguration in ASP.NET Core finden Sie unter <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="3e5ad-105">For general guidance on ASP.NET Core app configuration, see <xref:fundamentals/configuration/index>.</span></span>

Blazor WebAssembly<span data-ttu-id="3e5ad-106"> lädt Konfiguration aus:</span><span class="sxs-lookup"><span data-stu-id="3e5ad-106"> loads configuration from:</span></span>

* <span data-ttu-id="3e5ad-107">Standardmäßig aus den App-Einstellungsdateien:</span><span class="sxs-lookup"><span data-stu-id="3e5ad-107">App settings files by default:</span></span>
  * `wwwroot/appsettings.json`
  * `wwwroot/appsettings.{ENVIRONMENT}.json`
* <span data-ttu-id="3e5ad-108">Andere [Konfigurationsanbieter](xref:fundamentals/configuration/index), die von der App registriert werden.</span><span class="sxs-lookup"><span data-stu-id="3e5ad-108">Other [configuration providers](xref:fundamentals/configuration/index) registered by the app.</span></span> <span data-ttu-id="3e5ad-109">Nicht alle Anbieter sind für Blazor WebAssembly-Apps geeignet.</span><span class="sxs-lookup"><span data-stu-id="3e5ad-109">Not all providers are appropriate for Blazor WebAssembly apps.</span></span> <span data-ttu-id="3e5ad-110">Welche Anbieter für Blazor WebAssembly geeignet sind, erfahren Sie unter [Clarify configuration providers for Blazor WASM (dotnet/AspNetCore.Docs #18134)](https://github.com/dotnet/AspNetCore.Docs/issues/18134) (Konfigurationsanbieter für Blazor WASM).</span><span class="sxs-lookup"><span data-stu-id="3e5ad-110">Clarification on which providers are supported for Blazor WebAssembly is tracked by [Clarify configuration providers for Blazor WASM (dotnet/AspNetCore.Docs #18134)](https://github.com/dotnet/AspNetCore.Docs/issues/18134).</span></span>

> [!WARNING]
> <span data-ttu-id="3e5ad-111">Die Konfiguration in einer Blazor WebAssembly-App ist für Benutzer sichtbar.</span><span class="sxs-lookup"><span data-stu-id="3e5ad-111">Configuration in a Blazor WebAssembly app is visible to users.</span></span> <span data-ttu-id="3e5ad-112">**Speichern Sie keine App-Geheimnisse oder -Anmeldeinformationen in der Konfiguration.**</span><span class="sxs-lookup"><span data-stu-id="3e5ad-112">**Don't store app secrets or credentials in configuration.**</span></span>

<span data-ttu-id="3e5ad-113">Weitere Informationen zur Konfigurationsanbietern finden Sie unter <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="3e5ad-113">For more information on configuration providers, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="app-settings-configuration"></a><span data-ttu-id="3e5ad-114">Konfiguration von App-Einstellungen</span><span class="sxs-lookup"><span data-stu-id="3e5ad-114">App settings configuration</span></span>

<span data-ttu-id="3e5ad-115">`wwwroot/appsettings.json`:</span><span class="sxs-lookup"><span data-stu-id="3e5ad-115">`wwwroot/appsettings.json`:</span></span>

```json
{
  "message": "Hello from config!"
}
```

<span data-ttu-id="3e5ad-116">Fügen Sie eine <xref:Microsoft.Extensions.Configuration.IConfiguration>-Instanz in eine Komponente ein, um auf die Konfigurationsdaten zuzugreifen:</span><span class="sxs-lookup"><span data-stu-id="3e5ad-116">Inject an <xref:Microsoft.Extensions.Configuration.IConfiguration> instance into a component to access the configuration data:</span></span>

```razor
@page "/"
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<h1>Configuration example</h1>

<p>Message: @Configuration["message"]</p>
```

## <a name="provider-configuration"></a><span data-ttu-id="3e5ad-117">Anbieterkonfiguration</span><span class="sxs-lookup"><span data-stu-id="3e5ad-117">Provider configuration</span></span>

<span data-ttu-id="3e5ad-118">Im folgenden Beispiel wird ein <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationSource>-Element verwendet, um zusätzliche Konfiguration bereitzustellen:</span><span class="sxs-lookup"><span data-stu-id="3e5ad-118">The following example uses a <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationSource> to supply additional configuration:</span></span>

<span data-ttu-id="3e5ad-119">`Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="3e5ad-119">`Program.Main`:</span></span>

```csharp
using Microsoft.Extensions.Configuration.Memory;

...

var vehicleData = new Dictionary<string, string>()
{
    { "color", "blue" },
    { "type", "car" },
    { "wheels:count", "3" },
    { "wheels:brand", "Blazin" },
    { "wheels:brand:type", "rally" },
    { "wheels:year", "2008" },
};

var memoryConfig = new MemoryConfigurationSource { InitialData = vehicleData };

...

builder.Configuration.Add(memoryConfig);
```

<span data-ttu-id="3e5ad-120">Fügen Sie eine <xref:Microsoft.Extensions.Configuration.IConfiguration>-Instanz in eine Komponente ein, um auf die Konfigurationsdaten zuzugreifen:</span><span class="sxs-lookup"><span data-stu-id="3e5ad-120">Inject an <xref:Microsoft.Extensions.Configuration.IConfiguration> instance into a component to access the configuration data:</span></span>

```razor
@page "/"
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<h1>Configuration example</h1>

<h2>Wheels</h2>

<ul>
    <li>Count: @Configuration["wheels:count"]</li>
    <li>Brand: @Configuration["wheels:brand"]</li>
    <li>Type: @Configuration["wheels:brand:type"]</li>
    <li>Year: @Configuration["wheels:year"]</li>
</ul>

@code {
    var wheelsSection = Configuration.GetSection("wheels");
    
    ...
}
```

<span data-ttu-id="3e5ad-121">Um andere Konfigurationsdateien aus dem Ordner `wwwroot` in die Konfiguration einzulesen, verwenden Sie einen <xref:System.Net.Http.HttpClient>, um den Inhalt der Datei abzurufen.</span><span class="sxs-lookup"><span data-stu-id="3e5ad-121">To read other configuration files from the `wwwroot` folder into configuration, use an <xref:System.Net.Http.HttpClient> to obtain the file's content.</span></span> <span data-ttu-id="3e5ad-122">Bei diesem Ansatz kann die vorhandene <xref:System.Net.Http.HttpClient>-Dienstregistrierung den zum Lesen der Datei erstellten lokalen Client verwenden, wie im folgenden Beispiel gezeigt:</span><span class="sxs-lookup"><span data-stu-id="3e5ad-122">When using this approach, the existing <xref:System.Net.Http.HttpClient> service registration can use the local client created to read the file, as the following example shows:</span></span>

<span data-ttu-id="3e5ad-123">`wwwroot/cars.json`:</span><span class="sxs-lookup"><span data-stu-id="3e5ad-123">`wwwroot/cars.json`:</span></span>

```json
{
    "size": "tiny"
}
```

<span data-ttu-id="3e5ad-124">`Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="3e5ad-124">`Program.Main`:</span></span>

```csharp
using Microsoft.Extensions.Configuration;

...

var client = new HttpClient()
{
    BaseAddress = new Uri(builder.HostEnvironment.BaseAddress)
};

builder.Services.AddTransient(sp => client);

using var response = await client.GetAsync("cars.json");
using var stream = await response.Content.ReadAsStreamAsync();

builder.Configuration.AddJsonStream(stream);
```

## <a name="authentication-configuration"></a><span data-ttu-id="3e5ad-125">Authentifizierungskonfiguration</span><span class="sxs-lookup"><span data-stu-id="3e5ad-125">Authentication configuration</span></span>

<span data-ttu-id="3e5ad-126">`wwwroot/appsettings.json`:</span><span class="sxs-lookup"><span data-stu-id="3e5ad-126">`wwwroot/appsettings.json`:</span></span>

```json
{
  "Local": {
    "Authority": "{AUTHORITY}",
    "ClientId": "{CLIENT ID}"
  }
}
```

<span data-ttu-id="3e5ad-127">`Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="3e5ad-127">`Program.Main`:</span></span>

```csharp
builder.Services.AddOidcAuthentication(options =>
    builder.Configuration.Bind("Local", options.ProviderOptions));
```

## <a name="logging-configuration"></a><span data-ttu-id="3e5ad-128">Konfiguration der Protokollierung</span><span class="sxs-lookup"><span data-stu-id="3e5ad-128">Logging configuration</span></span>

<span data-ttu-id="3e5ad-129">Fügen Sie einen Paketverweis für [`Microsoft.Extensions.Logging.Configuration`](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Configuration/) hinzu:</span><span class="sxs-lookup"><span data-stu-id="3e5ad-129">Add a package reference for [`Microsoft.Extensions.Logging.Configuration`](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Configuration/):</span></span>

```xml
<PackageReference Include="Microsoft.Extensions.Logging.Configuration" Version="{VERSION}" />
```

<span data-ttu-id="3e5ad-130">`wwwroot/appsettings.json`:</span><span class="sxs-lookup"><span data-stu-id="3e5ad-130">`wwwroot/appsettings.json`:</span></span>

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft": "Warning",
      "Microsoft.Hosting.Lifetime": "Information"
    }
  }
}
```

<span data-ttu-id="3e5ad-131">`Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="3e5ad-131">`Program.Main`:</span></span>

```csharp
using Microsoft.Extensions.Logging;

...

builder.Logging.AddConfiguration(
    builder.Configuration.GetSection("Logging"));
```

## <a name="host-builder-configuration"></a><span data-ttu-id="3e5ad-132">Konfiguration des Host-Generators</span><span class="sxs-lookup"><span data-stu-id="3e5ad-132">Host builder configuration</span></span>

<span data-ttu-id="3e5ad-133">`Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="3e5ad-133">`Program.Main`:</span></span>

```csharp
var hostname = builder.Configuration["HostName"];
```

## <a name="cached-configuration"></a><span data-ttu-id="3e5ad-134">Zwischengespeicherte Konfiguration</span><span class="sxs-lookup"><span data-stu-id="3e5ad-134">Cached configuration</span></span>

<span data-ttu-id="3e5ad-135">Konfigurationsdateien werden zur Offlineverwendung zwischengespeichert.</span><span class="sxs-lookup"><span data-stu-id="3e5ad-135">Configuration files are cached for offline use.</span></span> <span data-ttu-id="3e5ad-136">Bei [progressiven Web-Apps (PWAs)](xref:blazor/progressive-web-app) können Sie Konfigurationsdateien nur beim Erstellen einer neuen Bereitstellung aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="3e5ad-136">With [Progressive Web Applications (PWAs)](xref:blazor/progressive-web-app), you can only update configuration files when creating a new deployment.</span></span> <span data-ttu-id="3e5ad-137">Die Bearbeitung von Konfigurationsdateien zwischen Bereitstellungen hat aus folgenden Gründen keine Auswirkungen:</span><span class="sxs-lookup"><span data-stu-id="3e5ad-137">Editing configuration files between deployments has no effect because:</span></span>

* <span data-ttu-id="3e5ad-138">Benutzer verfügen über zwischengespeicherte Versionen der Dateien, die sie weiterhin verwenden können.</span><span class="sxs-lookup"><span data-stu-id="3e5ad-138">Users have cached versions of the files that they continue to use.</span></span>
* <span data-ttu-id="3e5ad-139">Die Dateien `service-worker.js` und `service-worker-assets.js` der PWA müssen bei der Kompilierung neu erstellt werden, wodurch der App beim nächsten Onlineaufruf des Benutzers signalisiert wird, dass die App neu bereitgestellt wurde.</span><span class="sxs-lookup"><span data-stu-id="3e5ad-139">The PWA's `service-worker.js` and `service-worker-assets.js` files must be rebuilt on compilation, which signal to the app on the user's next online visit that the app has been redeployed.</span></span>

<span data-ttu-id="3e5ad-140">Weitere Informationen zur Verarbeitung von Updates im Hintergrund durch PWAs finden Sie unter <xref:blazor/progressive-web-app#background-updates>.</span><span class="sxs-lookup"><span data-stu-id="3e5ad-140">For more information on how background updates are handled by PWAs, see <xref:blazor/progressive-web-app#background-updates>.</span></span>
