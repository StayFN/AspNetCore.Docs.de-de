---
title: Dependency Injection in ASP.NET Core
author: rick-anderson
description: Erfahren Sie, wie ASP.NET Core Dependency Injection implementiert und wie Sie dieses Muster verwenden können.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/21/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: fundamentals/dependency-injection
ms.openlocfilehash: 2074aa75029cf27922b43545ec18c0cd8a50eb02
ms.sourcegitcommit: 895e952aec11c91d703fbdd3640a979307b8cc67
ms.contentlocale: de-DE
ms.lasthandoff: 07/01/2020
ms.locfileid: "85793347"
---
# <a name="dependency-injection-in-aspnet-core"></a><span data-ttu-id="950a3-103">Dependency Injection in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="950a3-103">Dependency injection in ASP.NET Core</span></span>

<span data-ttu-id="950a3-104">Von [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com), und [Brandon Dahler](https://github.com/brandondahler)</span><span class="sxs-lookup"><span data-stu-id="950a3-104">By [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com), and [Brandon Dahler](https://github.com/brandondahler)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="950a3-105">ASP.NET Core unterstützt das Softwareentwurfsmuster Abhängigkeitsinjektion. Damit kann eine [Umkehrung der Steuerung](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) (Inversion of Control, IoC) zwischen Klassen und ihren Abhängigkeiten erreicht werden.</span><span class="sxs-lookup"><span data-stu-id="950a3-105">ASP.NET Core supports the dependency injection (DI) software design pattern, which is a technique for achieving [Inversion of Control (IoC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) between classes and their dependencies.</span></span>

<span data-ttu-id="950a3-106">Weitere Informationen zur Abhängigkeitsinjektion innerhalb von MVC-Controllern finden Sie unter <xref:mvc/controllers/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="950a3-106">For more information specific to dependency injection within MVC controllers, see <xref:mvc/controllers/dependency-injection>.</span></span>

<span data-ttu-id="950a3-107">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([Vorgehensweise zum Herunterladen](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="950a3-107">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="overview-of-dependency-injection"></a><span data-ttu-id="950a3-108">Übersicht über Abhängigkeitsinjektion</span><span class="sxs-lookup"><span data-stu-id="950a3-108">Overview of dependency injection</span></span>

<span data-ttu-id="950a3-109">Eine *Abhängigkeit* ist ein beliebiges Objekt, das ein anderes Objekt benötigt.</span><span class="sxs-lookup"><span data-stu-id="950a3-109">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="950a3-110">Überprüfen Sie die folgende `MyDependency`-Klasse mit einer `WriteMessage`-Methode, von der andere Klassen in einer App abhängen:</span><span class="sxs-lookup"><span data-stu-id="950a3-110">Examine the following `MyDependency` class with a `WriteMessage` method that other classes in an app depend upon:</span></span>

```csharp
public class MyDependency
{
    public MyDependency()
    {
    }

    public Task WriteMessage(string message)
    {
        Console.WriteLine(
            $"MyDependency.WriteMessage called. Message: {message}");

        return Task.FromResult(0);
    }
}
```

<span data-ttu-id="950a3-111">Eine Instanz der Klasse `MyDependency` kann erstellt werden, um die `WriteMessage`-Methode einer Klasse zur Verfügung zu stellen.</span><span class="sxs-lookup"><span data-stu-id="950a3-111">An instance of the `MyDependency` class can be created to make the `WriteMessage` method available to a class.</span></span> <span data-ttu-id="950a3-112">Die Klasse `MyDependency` ist eine Abhängigkeit der Klasse `IndexModel`:</span><span class="sxs-lookup"><span data-stu-id="950a3-112">The `MyDependency` class is a dependency of the `IndexModel` class:</span></span>

```csharp
public class IndexModel : PageModel
{
    MyDependency _dependency = new MyDependency();

    public async Task OnGetAsync()
    {
        await _dependency.WriteMessage(
            "IndexModel.OnGetAsync created this message.");
    }
}
```

<span data-ttu-id="950a3-113">Die Klasse erstellt die Instanz `MyDependency` und hängt direkt von dieser ab.</span><span class="sxs-lookup"><span data-stu-id="950a3-113">The class creates and directly depends on the `MyDependency` instance.</span></span> <span data-ttu-id="950a3-114">Codeabhängigkeiten (wie im vorherigen Beispiel) sind problematisch und sollten aus folgenden Gründen vermieden werden:</span><span class="sxs-lookup"><span data-stu-id="950a3-114">Code dependencies (such as the previous example) are problematic and should be avoided for the following reasons:</span></span>

* <span data-ttu-id="950a3-115">Um `MyDependency` durch eine andere Implementierung zu ersetzen, muss die Klasse geändert werden.</span><span class="sxs-lookup"><span data-stu-id="950a3-115">To replace `MyDependency` with a different implementation, the class must be modified.</span></span>
* <span data-ttu-id="950a3-116">Wenn `MyDependency` über Abhängigkeiten verfügt, müssen diese von der Klasse konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="950a3-116">If `MyDependency` has dependencies, they must be configured by the class.</span></span> <span data-ttu-id="950a3-117">In einem großen Projekt mit mehreren Klassen, die von `MyDependency` abhängig sind, wird der Konfigurationscode über die App verteilt.</span><span class="sxs-lookup"><span data-stu-id="950a3-117">In a large project with multiple classes depending on `MyDependency`, the configuration code becomes scattered across the app.</span></span>
* <span data-ttu-id="950a3-118">Diese Implementierung ist nicht für Komponententests geeignet.</span><span class="sxs-lookup"><span data-stu-id="950a3-118">This implementation is difficult to unit test.</span></span> <span data-ttu-id="950a3-119">Die App sollte eine `MyDependency`-Modell- oder Stubklasse verwenden, was mit diesem Ansatz nicht möglich ist.</span><span class="sxs-lookup"><span data-stu-id="950a3-119">The app should use a mock or stub `MyDependency` class, which isn't possible with this approach.</span></span>

<span data-ttu-id="950a3-120">Die Abhängigkeitsinjektion löst dieses Problem mithilfe der folgenden Schritte:</span><span class="sxs-lookup"><span data-stu-id="950a3-120">Dependency injection addresses these problems through:</span></span>

* <span data-ttu-id="950a3-121">Die Verwendung einer Schnittstelle oder Basisklasse zur Abstraktion der Abhängigkeitsimplementierung.</span><span class="sxs-lookup"><span data-stu-id="950a3-121">The use of an interface or base class to abstract the dependency implementation.</span></span>
* <span data-ttu-id="950a3-122">Registrierung der Abhängigkeit in einem Dienstcontainer.</span><span class="sxs-lookup"><span data-stu-id="950a3-122">Registration of the dependency in a service container.</span></span> <span data-ttu-id="950a3-123">ASP.NET Core stellt einen integrierten Dienstcontainer (<xref:System.IServiceProvider>) bereit.</span><span class="sxs-lookup"><span data-stu-id="950a3-123">ASP.NET Core provides a built-in service container, <xref:System.IServiceProvider>.</span></span> <span data-ttu-id="950a3-124">Die Dienste werden in der App-Methode `Startup.ConfigureServices` registriert.</span><span class="sxs-lookup"><span data-stu-id="950a3-124">Services are registered in the app's `Startup.ConfigureServices` method.</span></span>
* <span data-ttu-id="950a3-125">Die *Injektion* des Diensts in den Konstruktor der Klasse, wo er verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="950a3-125">*Injection* of the service into the constructor of the class where it's used.</span></span> <span data-ttu-id="950a3-126">Das Framework erstellt eine Instanz der Abhängigkeit und entfernt diese, wenn sie nicht mehr benötigt wird.</span><span class="sxs-lookup"><span data-stu-id="950a3-126">The framework takes on the responsibility of creating an instance of the dependency and disposing of it when it's no longer needed.</span></span>

<span data-ttu-id="950a3-127">In der [Beispiel-App](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) definiert die `IMyDependency`-Schnittstelle eine Methode, die der Dienst für die App bereitstellt:</span><span class="sxs-lookup"><span data-stu-id="950a3-127">In the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples), the `IMyDependency` interface defines a method that the service provides to the app:</span></span>

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

<span data-ttu-id="950a3-128">Diese Schnittstelle wird durch einen konkreten Typ (`MyDependency`) implementiert:</span><span class="sxs-lookup"><span data-stu-id="950a3-128">This interface is implemented by a concrete type, `MyDependency`:</span></span>

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

<span data-ttu-id="950a3-129">`MyDependency` fordert einen <xref:Microsoft.Extensions.Logging.ILogger`1> beim zugehörigen Konstruktor an.</span><span class="sxs-lookup"><span data-stu-id="950a3-129">`MyDependency` requests an <xref:Microsoft.Extensions.Logging.ILogger`1> in its constructor.</span></span> <span data-ttu-id="950a3-130">Die Abhängigkeitsinjektion wird häufig als Verkettung verwendet.</span><span class="sxs-lookup"><span data-stu-id="950a3-130">It's not unusual to use dependency injection in a chained fashion.</span></span> <span data-ttu-id="950a3-131">Jede angeforderte Abhängigkeit fordert wiederum ihre eigenen Abhängigkeiten an.</span><span class="sxs-lookup"><span data-stu-id="950a3-131">Each requested dependency in turn requests its own dependencies.</span></span> <span data-ttu-id="950a3-132">Der Container löst die Abhängigkeiten im Diagramm auf und gibt den vollständig aufgelösten Dienst zurück.</span><span class="sxs-lookup"><span data-stu-id="950a3-132">The container resolves the dependencies in the graph and returns the fully resolved service.</span></span> <span data-ttu-id="950a3-133">Die gesammelten aufzulösenden Abhängigkeiten werden als *Abhängigkeitsstruktur*, *Abhängigkeitsdiagramm* oder *Objektdiagramm* bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="950a3-133">The collective set of dependencies that must be resolved is typically referred to as a *dependency tree*, *dependency graph*, or *object graph*.</span></span>

<span data-ttu-id="950a3-134">`IMyDependency` und `ILogger<TCategoryName>` müssen im Dienstcontainer registriert werden.</span><span class="sxs-lookup"><span data-stu-id="950a3-134">`IMyDependency` and `ILogger<TCategoryName>` must be registered in the service container.</span></span> <span data-ttu-id="950a3-135">`IMyDependency` ist in `Startup.ConfigureServices` registriert.</span><span class="sxs-lookup"><span data-stu-id="950a3-135">`IMyDependency` is registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="950a3-136">`ILogger<TCategoryName>` wird von der Protokollierungsabstraktionsinfrastruktur registriert. Es handelt sich also um einen [von einem Framework bereitgestellten Dienst](#framework-provided-services), der standardmäßig vom Framework registriert wird.</span><span class="sxs-lookup"><span data-stu-id="950a3-136">`ILogger<TCategoryName>` is registered by the logging abstractions infrastructure, so it's a [framework-provided service](#framework-provided-services) registered by default by the framework.</span></span>

<span data-ttu-id="950a3-137">Der Container löst `ILogger<TCategoryName>` unter Verwendung der [(generischen) offenen Typen](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types) auf, wodurch die Notwendigkeit entfällt, jeden [(generischen) konstruierten Typ](/dotnet/csharp/language-reference/language-specification/types#constructed-types) zu registrieren:</span><span class="sxs-lookup"><span data-stu-id="950a3-137">The container resolves `ILogger<TCategoryName>` by taking advantage of [(generic) open types](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types), eliminating the need to register every [(generic) constructed type](/dotnet/csharp/language-reference/language-specification/types#constructed-types):</span></span>

```csharp
services.AddSingleton(typeof(ILogger<>), typeof(Logger<>));
```

<span data-ttu-id="950a3-138">In der Beispiel-App ist der Dienst `IMyDependency` mit dem konkreten Typ `MyDependency` registriert.</span><span class="sxs-lookup"><span data-stu-id="950a3-138">In the sample app, the `IMyDependency` service is registered with the concrete type `MyDependency`.</span></span> <span data-ttu-id="950a3-139">Die Registrierung schränkt die Lebensdauer des Diensts auf die Lebensdauer einer einzelnen Anforderung ein.</span><span class="sxs-lookup"><span data-stu-id="950a3-139">The registration scopes the service lifetime to the lifetime of a single request.</span></span> <span data-ttu-id="950a3-140">Auf die [Dienstlebensdauer](#service-lifetimes) wird später in diesem Artikel eingegangen.</span><span class="sxs-lookup"><span data-stu-id="950a3-140">[Service lifetimes](#service-lifetimes) are described later in this topic.</span></span>

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=5)]

> [!NOTE]
> <span data-ttu-id="950a3-141">Jede Erweiterungsmethode vom Typ `services.Add{SERVICE_NAME}` fügt Dienste hinzu (und konfiguriert diese möglicherweise).</span><span class="sxs-lookup"><span data-stu-id="950a3-141">Each `services.Add{SERVICE_NAME}` extension method adds (and potentially configures) services.</span></span> <span data-ttu-id="950a3-142">So fügt beispielsweise `services.AddMvc()` die erforderlichen Dienste Razor Pages und MVC hinzu.</span><span class="sxs-lookup"><span data-stu-id="950a3-142">For example, `services.AddMvc()` adds the services Razor Pages and MVC require.</span></span> <span data-ttu-id="950a3-143">Es wird empfohlen, dass Apps dieser Konvention folgen.</span><span class="sxs-lookup"><span data-stu-id="950a3-143">We recommended that apps follow this convention.</span></span> <span data-ttu-id="950a3-144">Platzieren Sie Erweiterungsmethoden im Namespace [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection), um Gruppen von Dienstregistrierungen zu kapseln.</span><span class="sxs-lookup"><span data-stu-id="950a3-144">Place extension methods in the [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) namespace to encapsulate groups of service registrations.</span></span>

<span data-ttu-id="950a3-145">Wenn für den Dienstkonstruktor ein [integrierter Typ](/dotnet/csharp/language-reference/keywords/built-in-types-table) erforderlich ist, z. B. `string`, kann dieser Typ über [Konfiguration](xref:fundamentals/configuration/index) oder das [Optionsmuster](xref:fundamentals/configuration/options) eingefügt werden:</span><span class="sxs-lookup"><span data-stu-id="950a3-145">If the service's constructor requires a [built in type](/dotnet/csharp/language-reference/keywords/built-in-types-table), such as a `string`, the type can be injected by using [configuration](xref:fundamentals/configuration/index) or the [options pattern](xref:fundamentals/configuration/options):</span></span>

```csharp
public class MyDependency : IMyDependency
{
    public MyDependency(IConfiguration config)
    {
        var myStringValue = config["MyStringKey"];

        // Use myStringValue
    }

    ...
}
```

<span data-ttu-id="950a3-146">Eine Instanz des Diensts wird über den Konstruktor einer Klasse angefordert, in der der Dienst verwendet wird, und einem privaten Feld zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="950a3-146">An instance of the service is requested via the constructor of a class where the service is used and assigned to a private field.</span></span> <span data-ttu-id="950a3-147">Das Feld wird verwendet, um auf den Dienst bei Bedarf über die gesamte Klasse zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="950a3-147">The field is used to access the service as necessary throughout the class.</span></span>

<span data-ttu-id="950a3-148">In der Beispiel-App wird die Instanz `IMyDependency` angefordert, und mit ihr wird die App-Methode `WriteMessage` aufgerufen:</span><span class="sxs-lookup"><span data-stu-id="950a3-148">In the sample app, the `IMyDependency` instance is requested and used to call the service's `WriteMessage` method:</span></span>

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3,6,13,29-30)]

## <a name="services-injected-into-startup"></a><span data-ttu-id="950a3-149">In den Start eingefügte Dienste</span><span class="sxs-lookup"><span data-stu-id="950a3-149">Services injected into Startup</span></span>

<span data-ttu-id="950a3-150">Bei Verwendung des generischen Hosts (<xref:Microsoft.Extensions.Hosting.IHostBuilder>) können nur die folgenden Diensttypen in den `Startup`-Konstruktor eingefügt werden:</span><span class="sxs-lookup"><span data-stu-id="950a3-150">Only the following service types can be injected into the `Startup` constructor when using the Generic Host (<xref:Microsoft.Extensions.Hosting.IHostBuilder>):</span></span>

* `IWebHostEnvironment`
* <xref:Microsoft.Extensions.Hosting.IHostEnvironment>
* <xref:Microsoft.Extensions.Configuration.IConfiguration>

<span data-ttu-id="950a3-151">Dienste können in `Startup.Configure` eingefügt werden:</span><span class="sxs-lookup"><span data-stu-id="950a3-151">Services can be injected into `Startup.Configure`:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptions<MyOptions> options)
{
    ...
}
```

<span data-ttu-id="950a3-152">Weitere Informationen finden Sie unter <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="950a3-152">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="framework-provided-services"></a><span data-ttu-id="950a3-153">Von Frameworks bereitgestellte Dienste</span><span class="sxs-lookup"><span data-stu-id="950a3-153">Framework-provided services</span></span>

<span data-ttu-id="950a3-154">Die Methode `Startup.ConfigureServices` definiert die von der App verwendeten Dienste. Dies umfasst auch Plattformfeatures wie Entity Framework Core und ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="950a3-154">The `Startup.ConfigureServices` method is responsible for defining the services that the app uses, including platform features, such as Entity Framework Core and ASP.NET Core MVC.</span></span> <span data-ttu-id="950a3-155">Zunächst enthält die in `ConfigureServices` bereitgestellte `IServiceCollection` die vom Framework definierten Dienste, abhängig davon, wie der [Host konfiguriert](xref:fundamentals/index#host) wurde.</span><span class="sxs-lookup"><span data-stu-id="950a3-155">Initially, the `IServiceCollection` provided to `ConfigureServices` has services defined by the framework depending on [how the host was configured](xref:fundamentals/index#host).</span></span> <span data-ttu-id="950a3-156">Es ist nicht ungewöhnlich, dass das Framework Hunderte von Diensten für eine auf einer ASP.NET Core-Vorlage basierende App registriert.</span><span class="sxs-lookup"><span data-stu-id="950a3-156">It's not uncommon for an app based on an ASP.NET Core template to have hundreds of services registered by the framework.</span></span> <span data-ttu-id="950a3-157">In der folgenden Tabelle finden Sie eine kleine Auswahl der Dienste, die vom Framework registriert werden können.</span><span class="sxs-lookup"><span data-stu-id="950a3-157">A small sample of framework-registered services is listed in the following table.</span></span>

| <span data-ttu-id="950a3-158">Diensttyp</span><span class="sxs-lookup"><span data-stu-id="950a3-158">Service Type</span></span> | <span data-ttu-id="950a3-159">Lebensdauer</span><span class="sxs-lookup"><span data-stu-id="950a3-159">Lifetime</span></span> |
| ------------ | -------- |
| <xref:Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory?displayProperty=fullName> | <span data-ttu-id="950a3-160">Transient (vorübergehend)</span><span class="sxs-lookup"><span data-stu-id="950a3-160">Transient</span></span> |
| `IHostApplicationLifetime` | <span data-ttu-id="950a3-161">Singleton</span><span class="sxs-lookup"><span data-stu-id="950a3-161">Singleton</span></span> |
| `IWebHostEnvironment` | <span data-ttu-id="950a3-162">Singleton</span><span class="sxs-lookup"><span data-stu-id="950a3-162">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IStartup?displayProperty=fullName> | <span data-ttu-id="950a3-163">Singleton</span><span class="sxs-lookup"><span data-stu-id="950a3-163">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IStartupFilter?displayProperty=fullName> | <span data-ttu-id="950a3-164">Transient (vorübergehend)</span><span class="sxs-lookup"><span data-stu-id="950a3-164">Transient</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.Server.IServer?displayProperty=fullName> | <span data-ttu-id="950a3-165">Singleton</span><span class="sxs-lookup"><span data-stu-id="950a3-165">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Http.IHttpContextFactory?displayProperty=fullName> | <span data-ttu-id="950a3-166">Transient (vorübergehend)</span><span class="sxs-lookup"><span data-stu-id="950a3-166">Transient</span></span> |
| <xref:Microsoft.Extensions.Logging.ILogger`1?displayProperty=fullName> | <span data-ttu-id="950a3-167">Singleton</span><span class="sxs-lookup"><span data-stu-id="950a3-167">Singleton</span></span> |
| <xref:Microsoft.Extensions.Logging.ILoggerFactory?displayProperty=fullName> | <span data-ttu-id="950a3-168">Singleton</span><span class="sxs-lookup"><span data-stu-id="950a3-168">Singleton</span></span> |
| <xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider?displayProperty=fullName> | <span data-ttu-id="950a3-169">Singleton</span><span class="sxs-lookup"><span data-stu-id="950a3-169">Singleton</span></span> |
| <xref:Microsoft.Extensions.Options.IConfigureOptions`1?displayProperty=fullName> | <span data-ttu-id="950a3-170">Transient (vorübergehend)</span><span class="sxs-lookup"><span data-stu-id="950a3-170">Transient</span></span> |
| <xref:Microsoft.Extensions.Options.IOptions`1?displayProperty=fullName> | <span data-ttu-id="950a3-171">Singleton</span><span class="sxs-lookup"><span data-stu-id="950a3-171">Singleton</span></span> |
| <xref:System.Diagnostics.DiagnosticSource?displayProperty=fullName> | <span data-ttu-id="950a3-172">Singleton</span><span class="sxs-lookup"><span data-stu-id="950a3-172">Singleton</span></span> |
| <xref:System.Diagnostics.DiagnosticListener?displayProperty=fullName> | <span data-ttu-id="950a3-173">Singleton</span><span class="sxs-lookup"><span data-stu-id="950a3-173">Singleton</span></span> |

## <a name="register-additional-services-with-extension-methods"></a><span data-ttu-id="950a3-174">Registrieren weiterer Dienste mit Erweiterungsmethoden</span><span class="sxs-lookup"><span data-stu-id="950a3-174">Register additional services with extension methods</span></span>

<span data-ttu-id="950a3-175">Wenn eine Dienstsammlungs-Erweiterungsmethode verfügbar ist, um einen Dienst (und ggf. seine abhängigen Dienste) zu registrieren, ist die Konvention, eine einzige `Add{SERVICE_NAME}`-Erweiterungsmethode zu verwenden, um alle von diesem Dienst benötigten Dienste zu registrieren.</span><span class="sxs-lookup"><span data-stu-id="950a3-175">When a service collection extension method is available to register a service (and its dependent services, if required), the convention is to use a single `Add{SERVICE_NAME}` extension method to register all of the services required by that service.</span></span> <span data-ttu-id="950a3-176">Der folgende Code ist ein Beispiel dafür, wie mit den Erweiterungsmethoden [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) und <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentityCore*> zusätzliche Dienste zum Container hinzugefügt werden:</span><span class="sxs-lookup"><span data-stu-id="950a3-176">The following code is an example of how to add additional services to the container using the extension methods [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) and <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentityCore*>:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    ...

    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));

    services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

    ...
}
```

<span data-ttu-id="950a3-177">Weitere Informationen finden Sie in der <xref:Microsoft.Extensions.DependencyInjection.ServiceCollection>-Klasse in der API-Dokumentation.</span><span class="sxs-lookup"><span data-stu-id="950a3-177">For more information, see the <xref:Microsoft.Extensions.DependencyInjection.ServiceCollection> class in the API documentation.</span></span>

## <a name="service-lifetimes"></a><span data-ttu-id="950a3-178">Dienstlebensdauer</span><span class="sxs-lookup"><span data-stu-id="950a3-178">Service lifetimes</span></span>

<span data-ttu-id="950a3-179">Wählen Sie eine geeignete Lebensdauer für jeden registrierten Dienst aus.</span><span class="sxs-lookup"><span data-stu-id="950a3-179">Choose an appropriate lifetime for each registered service.</span></span> <span data-ttu-id="950a3-180">ASP.NET Core-Dienste können mit folgender Lebensdauer konfiguriert werden:</span><span class="sxs-lookup"><span data-stu-id="950a3-180">ASP.NET Core services can be configured with the following lifetimes:</span></span>

### <a name="transient"></a><span data-ttu-id="950a3-181">Transient (vorübergehend)</span><span class="sxs-lookup"><span data-stu-id="950a3-181">Transient</span></span>

<span data-ttu-id="950a3-182">Kurzlebige Dienste (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddTransient*>) werden bei jeder Anforderung aus dem Dienstcontainer neu erstellt.</span><span class="sxs-lookup"><span data-stu-id="950a3-182">Transient lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddTransient*>) are created each time they're requested from the service container.</span></span> <span data-ttu-id="950a3-183">Diese Lebensdauer ist am besten für einfache, zustandslose Dienste geeignet.</span><span class="sxs-lookup"><span data-stu-id="950a3-183">This lifetime works best for lightweight, stateless services.</span></span>

<span data-ttu-id="950a3-184">In Apps, die Anforderungen verarbeiten, werden vorübergehende Dienste am Ende der Anforderung verworfen.</span><span class="sxs-lookup"><span data-stu-id="950a3-184">In apps that process requests, transient services are disposed at the end of the request.</span></span>

### <a name="scoped"></a><span data-ttu-id="950a3-185">Bereichsbezogen</span><span class="sxs-lookup"><span data-stu-id="950a3-185">Scoped</span></span>

<span data-ttu-id="950a3-186">Dienste mit bereichsbezogener Lebensdauer (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped*>) werden einmal pro Clientanforderung (-verbindung) erstellt.</span><span class="sxs-lookup"><span data-stu-id="950a3-186">Scoped lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped*>) are created once per client request (connection).</span></span>

<span data-ttu-id="950a3-187">In Apps, die Anforderungen verarbeiten, werden bereichsbezogene Dienste am Ende der Anforderung verworfen.</span><span class="sxs-lookup"><span data-stu-id="950a3-187">In apps that process requests, scoped services are disposed at the end of the request.</span></span>

> [!WARNING]
> <span data-ttu-id="950a3-188">Wenn Sie einen bereichsbezogenen Dienst in einer Middleware verwenden, müssen Sie den Dienst in die `Invoke`- oder `InvokeAsync`-Methode einfügen.</span><span class="sxs-lookup"><span data-stu-id="950a3-188">When using a scoped service in a middleware, inject the service into the `Invoke` or `InvokeAsync` method.</span></span> <span data-ttu-id="950a3-189">Fügen Sie ihn nicht über [Konstruktorinjektion](xref:mvc/controllers/dependency-injection#constructor-injection) ein, da hierdurch der Dienst ein Verhalten wie ein Singleton zeigt.</span><span class="sxs-lookup"><span data-stu-id="950a3-189">Don't inject via [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) because it forces the service to behave like a singleton.</span></span> <span data-ttu-id="950a3-190">Weitere Informationen finden Sie unter <xref:fundamentals/middleware/write#per-request-middleware-dependencies>.</span><span class="sxs-lookup"><span data-stu-id="950a3-190">For more information, see <xref:fundamentals/middleware/write#per-request-middleware-dependencies>.</span></span>

### <a name="singleton"></a><span data-ttu-id="950a3-191">Singleton</span><span class="sxs-lookup"><span data-stu-id="950a3-191">Singleton</span></span>

<span data-ttu-id="950a3-192">Dienste mit Singleton-Lebensdauer (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*>) werden bei der ersten Anforderung erstellt (bzw. wenn `Startup.ConfigureServices` ausgeführt wird, und eine Instanz bei der Dienstregistrierung angegeben wird).</span><span class="sxs-lookup"><span data-stu-id="950a3-192">Singleton lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*>) are created the first time they're requested (or when `Startup.ConfigureServices` is run and an instance is specified with the service registration).</span></span> <span data-ttu-id="950a3-193">Jeder nachfolgenden Anforderung verwendet die gleiche Instanz.</span><span class="sxs-lookup"><span data-stu-id="950a3-193">Every subsequent request uses the same instance.</span></span> <span data-ttu-id="950a3-194">Wenn die App ein Verhalten vom Typ „Singleton“ erfordert, wird empfohlen, den Dienstcontainer die Dienstlebensdauer verwalten zu lassen.</span><span class="sxs-lookup"><span data-stu-id="950a3-194">If the app requires singleton behavior, allowing the service container to manage the service's lifetime is recommended.</span></span> <span data-ttu-id="950a3-195">Implementieren Sie nicht das Designmuster „Singleton“ und stellen Sie Benutzercode bereit, um die Objektlebensdauer in der Klasse zu verwalten.</span><span class="sxs-lookup"><span data-stu-id="950a3-195">Don't implement the singleton design pattern and provide user code to manage the object's lifetime in the class.</span></span>

<span data-ttu-id="950a3-196">In Apps, die Anforderungen verarbeiten, werden Singleton-Dienste verworfen, wenn der <xref:Microsoft.Extensions.DependencyInjection.ServiceProvider> beim Herunterfahren der App verworfen wird.</span><span class="sxs-lookup"><span data-stu-id="950a3-196">In apps that process requests, singleton services are disposed when the <xref:Microsoft.Extensions.DependencyInjection.ServiceProvider> is disposed at app shutdown.</span></span>

> [!WARNING]
> <span data-ttu-id="950a3-197">Es ist riskant, einen bereichsbezogenen Dienst über ein Singleton aufzulösen.</span><span class="sxs-lookup"><span data-stu-id="950a3-197">It's dangerous to resolve a scoped service from a singleton.</span></span> <span data-ttu-id="950a3-198">Möglicherweise weist der Dienst bei der Verarbeitung nachfolgender Anforderungen einen falschen Status auf.</span><span class="sxs-lookup"><span data-stu-id="950a3-198">It may cause the service to have incorrect state when processing subsequent requests.</span></span>

## <a name="service-registration-methods"></a><span data-ttu-id="950a3-199">Dienstregistrierungsmethoden</span><span class="sxs-lookup"><span data-stu-id="950a3-199">Service registration methods</span></span>

<span data-ttu-id="950a3-200">Methoden zur Erweiterung der Dienstregistrierung bieten Überladungen, die in bestimmten Szenarios hilfreich sind.</span><span class="sxs-lookup"><span data-stu-id="950a3-200">Service registration extension methods offer overloads that are useful in specific scenarios.</span></span>

| <span data-ttu-id="950a3-201">Methode</span><span class="sxs-lookup"><span data-stu-id="950a3-201">Method</span></span> | <span data-ttu-id="950a3-202">Automatische</span><span class="sxs-lookup"><span data-stu-id="950a3-202">Automatic</span></span><br><span data-ttu-id="950a3-203">Objekt</span><span class="sxs-lookup"><span data-stu-id="950a3-203">object</span></span><br><span data-ttu-id="950a3-204">bereinigung</span><span class="sxs-lookup"><span data-stu-id="950a3-204">disposal</span></span> | <span data-ttu-id="950a3-205">Mehrere</span><span class="sxs-lookup"><span data-stu-id="950a3-205">Multiple</span></span><br><span data-ttu-id="950a3-206">Implementierungen</span><span class="sxs-lookup"><span data-stu-id="950a3-206">implementations</span></span> | <span data-ttu-id="950a3-207">Argumentübergabe</span><span class="sxs-lookup"><span data-stu-id="950a3-207">Pass args</span></span> |
| ------ | :-----------------------------: | :-------------------------: | :-------: |
| `Add{LIFETIME}<{SERVICE}, {IMPLEMENTATION}>()`<br><span data-ttu-id="950a3-208">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="950a3-208">Example:</span></span><br>`services.AddSingleton<IMyDep, MyDep>();` | <span data-ttu-id="950a3-209">Ja</span><span class="sxs-lookup"><span data-stu-id="950a3-209">Yes</span></span> | <span data-ttu-id="950a3-210">Ja</span><span class="sxs-lookup"><span data-stu-id="950a3-210">Yes</span></span> | <span data-ttu-id="950a3-211">Nein</span><span class="sxs-lookup"><span data-stu-id="950a3-211">No</span></span> |
| `Add{LIFETIME}<{SERVICE}>(sp => new {IMPLEMENTATION})`<br><span data-ttu-id="950a3-212">Beispiele:</span><span class="sxs-lookup"><span data-stu-id="950a3-212">Examples:</span></span><br>`services.AddSingleton<IMyDep>(sp => new MyDep());`<br>`services.AddSingleton<IMyDep>(sp => new MyDep("A string!"));` | <span data-ttu-id="950a3-213">Ja</span><span class="sxs-lookup"><span data-stu-id="950a3-213">Yes</span></span> | <span data-ttu-id="950a3-214">Ja</span><span class="sxs-lookup"><span data-stu-id="950a3-214">Yes</span></span> | <span data-ttu-id="950a3-215">Ja</span><span class="sxs-lookup"><span data-stu-id="950a3-215">Yes</span></span> |
| `Add{LIFETIME}<{IMPLEMENTATION}>()`<br><span data-ttu-id="950a3-216">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="950a3-216">Example:</span></span><br>`services.AddSingleton<MyDep>();` | <span data-ttu-id="950a3-217">Ja</span><span class="sxs-lookup"><span data-stu-id="950a3-217">Yes</span></span> | <span data-ttu-id="950a3-218">Nein</span><span class="sxs-lookup"><span data-stu-id="950a3-218">No</span></span> | <span data-ttu-id="950a3-219">Nein</span><span class="sxs-lookup"><span data-stu-id="950a3-219">No</span></span> |
| `AddSingleton<{SERVICE}>(new {IMPLEMENTATION})`<br><span data-ttu-id="950a3-220">Beispiele:</span><span class="sxs-lookup"><span data-stu-id="950a3-220">Examples:</span></span><br>`services.AddSingleton<IMyDep>(new MyDep());`<br>`services.AddSingleton<IMyDep>(new MyDep("A string!"));` | <span data-ttu-id="950a3-221">Nein</span><span class="sxs-lookup"><span data-stu-id="950a3-221">No</span></span> | <span data-ttu-id="950a3-222">Ja</span><span class="sxs-lookup"><span data-stu-id="950a3-222">Yes</span></span> | <span data-ttu-id="950a3-223">Ja</span><span class="sxs-lookup"><span data-stu-id="950a3-223">Yes</span></span> |
| `AddSingleton(new {IMPLEMENTATION})`<br><span data-ttu-id="950a3-224">Beispiele:</span><span class="sxs-lookup"><span data-stu-id="950a3-224">Examples:</span></span><br>`services.AddSingleton(new MyDep());`<br>`services.AddSingleton(new MyDep("A string!"));` | <span data-ttu-id="950a3-225">Nein</span><span class="sxs-lookup"><span data-stu-id="950a3-225">No</span></span> | <span data-ttu-id="950a3-226">Nein</span><span class="sxs-lookup"><span data-stu-id="950a3-226">No</span></span> | <span data-ttu-id="950a3-227">Ja</span><span class="sxs-lookup"><span data-stu-id="950a3-227">Yes</span></span> |

<span data-ttu-id="950a3-228">Weitere Informationen zum Löschen von Typen finden Sie im Abschnitt [Löschen von Diensten](#disposal-of-services).</span><span class="sxs-lookup"><span data-stu-id="950a3-228">For more information on type disposal, see the [Disposal of services](#disposal-of-services) section.</span></span> <span data-ttu-id="950a3-229">Ein häufiges Szenario für mehrere Implementierungen ist [Typen zu Testzwecken simulieren](xref:test/integration-tests#inject-mock-services).</span><span class="sxs-lookup"><span data-stu-id="950a3-229">A common scenario for multiple implementations is [mocking types for testing](xref:test/integration-tests#inject-mock-services).</span></span>

<span data-ttu-id="950a3-230">`TryAdd{LIFETIME}`-Methoden registrieren den Dienst nur, wenn noch keine Implementierung registriert ist.</span><span class="sxs-lookup"><span data-stu-id="950a3-230">`TryAdd{LIFETIME}` methods only register the service if there isn't already an implementation registered.</span></span>

<span data-ttu-id="950a3-231">Im folgenden Beispiel registriert die erste Zeile `MyDependency` für `IMyDependency`.</span><span class="sxs-lookup"><span data-stu-id="950a3-231">In the following example, the first line registers `MyDependency` for `IMyDependency`.</span></span> <span data-ttu-id="950a3-232">Die zweite Zeile hat keine Auswirkungen, da es für `IMyDependency` bereits eine registrierte Implementierung gibt:</span><span class="sxs-lookup"><span data-stu-id="950a3-232">The second line has no effect because `IMyDependency` already has a registered implementation:</span></span>

```csharp
services.AddSingleton<IMyDependency, MyDependency>();
// The following line has no effect:
services.TryAddSingleton<IMyDependency, DifferentDependency>();
```

<span data-ttu-id="950a3-233">Weitere Informationen finden Sie unter:</span><span class="sxs-lookup"><span data-stu-id="950a3-233">For more information, see:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAdd*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddTransient*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddScoped*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddSingleton*>

<span data-ttu-id="950a3-234">[TryAddEnumerable(ServiceDescriptor)](xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddEnumerable*)-Methoden registrieren den Dienst nur, wenn es nicht bereits eine Implementierung *des gleichen Typs* gibt.</span><span class="sxs-lookup"><span data-stu-id="950a3-234">[TryAddEnumerable(ServiceDescriptor)](xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddEnumerable*) methods only register the service if there isn't already an implementation *of the same type*.</span></span> <span data-ttu-id="950a3-235">Mehrere Dienste werden über `IEnumerable<{SERVICE}>` aufgelöst.</span><span class="sxs-lookup"><span data-stu-id="950a3-235">Multiple services are resolved via `IEnumerable<{SERVICE}>`.</span></span> <span data-ttu-id="950a3-236">Beim Registrieren von Diensten möchte der Entwickler nur eine Instanz hinzufügen, wenn nicht bereits eine Instanz vom gleichen Typ hinzugefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="950a3-236">When registering services, the developer only wants to add an instance if one of the same type hasn't already been added.</span></span> <span data-ttu-id="950a3-237">Diese Methode wird in der Regel von Bibliotheksautoren verwendet, um zu vermeiden, dass zwei Kopien einer Instanz im Container registriert werden.</span><span class="sxs-lookup"><span data-stu-id="950a3-237">Generally, this method is used by library authors to avoid registering two copies of an instance in the container.</span></span>

<span data-ttu-id="950a3-238">Im folgenden Beispiel registriert die erste Zeile `MyDep` für `IMyDep1`.</span><span class="sxs-lookup"><span data-stu-id="950a3-238">In the following example, the first line registers `MyDep` for `IMyDep1`.</span></span> <span data-ttu-id="950a3-239">Die zweite Zeile registriert `MyDep` für `IMyDep2`.</span><span class="sxs-lookup"><span data-stu-id="950a3-239">The second line registers `MyDep` for `IMyDep2`.</span></span> <span data-ttu-id="950a3-240">Die dritte Zeile hat keine Auswirkungen, da es für `IMyDep1` bereits eine registrierte Implementierung von `MyDep` gibt:</span><span class="sxs-lookup"><span data-stu-id="950a3-240">The third line has no effect because `IMyDep1` already has a registered implementation of `MyDep`:</span></span>

```csharp
public interface IMyDep1 {}
public interface IMyDep2 {}

public class MyDep : IMyDep1, IMyDep2 {}

services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep1, MyDep>());
services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep2, MyDep>());
// Two registrations of MyDep for IMyDep1 is avoided by the following line:
services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep1, MyDep>());
```

### <a name="constructor-injection-behavior"></a><span data-ttu-id="950a3-241">Verhalten von Constructor Injection</span><span class="sxs-lookup"><span data-stu-id="950a3-241">Constructor injection behavior</span></span>

<span data-ttu-id="950a3-242">Dienste können durch zwei Mechanismen aufgelöst werden:</span><span class="sxs-lookup"><span data-stu-id="950a3-242">Services can be resolved by two mechanisms:</span></span>

* <xref:System.IServiceProvider>
* <span data-ttu-id="950a3-243"><xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities>: Lässt die Erstellung von Objekten ohne Dienstregistrierung im Dependency-Injection-Container zu</span><span class="sxs-lookup"><span data-stu-id="950a3-243"><xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities>: Permits object creation without service registration in the dependency injection container.</span></span> <span data-ttu-id="950a3-244">`ActivatorUtilities` wird mit Abstraktionen für Benutzer verwendet. Dazu zählen Taghilfsprogramme, MVC-Controller und Modellbindungen.</span><span class="sxs-lookup"><span data-stu-id="950a3-244">`ActivatorUtilities` is used with user-facing abstractions, such as Tag Helpers, MVC controllers, and model binders.</span></span>

<span data-ttu-id="950a3-245">Konstruktoren können Argumente akzeptieren, die nicht durch Abhängigkeitsinjektion bereitgestellt werden. Die Argumente müssen jedoch Standardwerte zuweisen.</span><span class="sxs-lookup"><span data-stu-id="950a3-245">Constructors can accept arguments that aren't provided by dependency injection, but the arguments must assign default values.</span></span>

<span data-ttu-id="950a3-246">Wenn Dienste durch `IServiceProvider` oder `ActivatorUtilities` aufgelöst werden, benötigt die [Konstruktorinjektion](xref:mvc/controllers/dependency-injection#constructor-injection) einen *öffentlichen* Konstruktor.</span><span class="sxs-lookup"><span data-stu-id="950a3-246">When services are resolved by `IServiceProvider` or `ActivatorUtilities`, [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) requires a *public* constructor.</span></span>

<span data-ttu-id="950a3-247">Wenn Dienste durch `ActivatorUtilities` aufgelöst werden, erfordert die [Konstruktorinjektion](xref:mvc/controllers/dependency-injection#constructor-injection), dass nur ein anwendbarer Konstruktor vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="950a3-247">When services are resolved by `ActivatorUtilities`, [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) requires that only one applicable constructor exists.</span></span> <span data-ttu-id="950a3-248">Konstruktorüberladungen werden unterstützt. Es darf jedoch nur eine Überladung vorhanden sein, deren Argumente alle durch Dependency Injection erfüllt werden können.</span><span class="sxs-lookup"><span data-stu-id="950a3-248">Constructor overloads are supported, but only one overload can exist whose arguments can all be fulfilled by dependency injection.</span></span>

## <a name="entity-framework-contexts"></a><span data-ttu-id="950a3-249">Entity Framework-Kontexte</span><span class="sxs-lookup"><span data-stu-id="950a3-249">Entity Framework contexts</span></span>

<span data-ttu-id="950a3-250">Entity Framework-Kontexte werden einem Dienstcontainer normalerweise mithilfe der [bereichsbezogenen Lebensdauer](#service-lifetimes) hinzugefügt, da Datenbankvorgänge von Web-Apps normalerweise auf den Clientanforderungsbereich bezogen werden.</span><span class="sxs-lookup"><span data-stu-id="950a3-250">Entity Framework contexts are usually added to the service container using the [scoped lifetime](#service-lifetimes) because web app database operations are normally scoped to the client request.</span></span> <span data-ttu-id="950a3-251">Der Bereich einer Standardlebensdauer wird festgelegt, wenn eine Lebensdauer nicht von einer [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext)-Überladung angegeben wird, wenn der Datenbankkontext registriert wird.</span><span class="sxs-lookup"><span data-stu-id="950a3-251">The default lifetime is scoped if a lifetime isn't specified by an [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) overload when registering the database context.</span></span> <span data-ttu-id="950a3-252">Dienste einer festgelegten Lebensdauer sollten keinen Datenbankkontext mit einer kürzeren Lebensdauer als die des Dienstes verwenden.</span><span class="sxs-lookup"><span data-stu-id="950a3-252">Services of a given lifetime shouldn't use a database context with a shorter lifetime than the service.</span></span>

## <a name="lifetime-and-registration-options"></a><span data-ttu-id="950a3-253">Lebensdauer und Registrierungsoptionen</span><span class="sxs-lookup"><span data-stu-id="950a3-253">Lifetime and registration options</span></span>

<span data-ttu-id="950a3-254">Um den Unterschied zwischen der Lebensdauer und den Registrierungsoptionen zu demonstrieren, betrachten Sie die folgenden Schnittstellen, die Aufgaben als Vorgänge mit einem eindeutigen Bezeichner (`OperationId`) darstellen.</span><span class="sxs-lookup"><span data-stu-id="950a3-254">To demonstrate the difference between the lifetime and registration options, consider the following interfaces that represent tasks as an operation with a unique identifier, `OperationId`.</span></span> <span data-ttu-id="950a3-255">Je nachdem, wie die Dienstlebensdauer für die folgenden Schnittstellen konfiguriert ist, stellt der Container auf Anforderung einer Klasse entweder die gleiche oder eine andere Instanz des Diensts zur Verfügung:</span><span class="sxs-lookup"><span data-stu-id="950a3-255">Depending on how the lifetime of an operations service is configured for the following interfaces, the container provides either the same or a different instance of the service when requested by a class:</span></span>

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

<span data-ttu-id="950a3-256">Die Schnittstellen sind in die Klasse `Operation` implementiert.</span><span class="sxs-lookup"><span data-stu-id="950a3-256">The interfaces are implemented in the `Operation` class.</span></span> <span data-ttu-id="950a3-257">Der `Operation`-Konstruktor generiert eine GUID, wenn noch keine angegeben ist:</span><span class="sxs-lookup"><span data-stu-id="950a3-257">The `Operation` constructor generates a GUID if one isn't supplied:</span></span>

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

<span data-ttu-id="950a3-258">`OperationService` ist registriert und hängt von jedem anderen `Operation`-Typ ab.</span><span class="sxs-lookup"><span data-stu-id="950a3-258">An `OperationService` is registered that depends on each of the other `Operation` types.</span></span> <span data-ttu-id="950a3-259">Wenn `OperationService` über die Abhängigkeitsinjektion angefordert wird, wird entweder eine neue Instanz jedes Diensts oder eine vorhandene Instanz basierend auf der Lebensdauer des abhängigen Diensts zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="950a3-259">When `OperationService` is requested via dependency injection, it receives either a new instance of each service or an existing instance based on the lifetime of the dependent service.</span></span>

* <span data-ttu-id="950a3-260">Wenn vorübergehende Dienste bei der Anforderung aus dem Container erstellt werden, ist die `OperationId` von Dienst `IOperationTransient` anders als die `OperationId` von `OperationService`.</span><span class="sxs-lookup"><span data-stu-id="950a3-260">When transient services are created when requested from the container, the `OperationId` of the `IOperationTransient` service is different than the `OperationId` of the `OperationService`.</span></span> <span data-ttu-id="950a3-261">`OperationService` erhält eine neue Instanz der Klasse `IOperationTransient`.</span><span class="sxs-lookup"><span data-stu-id="950a3-261">`OperationService` receives a new instance of the `IOperationTransient` class.</span></span> <span data-ttu-id="950a3-262">Der `OperationId`-Wert der neuen Instanz ist anders.</span><span class="sxs-lookup"><span data-stu-id="950a3-262">The new instance yields a different `OperationId`.</span></span>
* <span data-ttu-id="950a3-263">Wenn bereichsbezogene Dienste pro Clientanforderung erstellt werden, ist die `OperationId` in Dienst `IOperationScoped` und `OperationService` identisch innerhalb einer Clientanforderung.</span><span class="sxs-lookup"><span data-stu-id="950a3-263">When scoped services are created per client request, the `OperationId` of the `IOperationScoped` service is the same as that of `OperationService` within a client request.</span></span> <span data-ttu-id="950a3-264">Clientanforderungsübergreifend haben die beiden Dienste jedoch einen anderen gemeinsamen `OperationId`-Wert.</span><span class="sxs-lookup"><span data-stu-id="950a3-264">Across client requests, both services share a different `OperationId` value.</span></span>
* <span data-ttu-id="950a3-265">Wenn Singleton- und Singletoninstanzdienste einmal erstellt und für alle Clientanforderungen und alle Dienste verwendet werden, ist `OperationId` für alle Dienstanforderungen identisch.</span><span class="sxs-lookup"><span data-stu-id="950a3-265">When singleton and singleton-instance services are created once and used across all client requests and all services, the `OperationId` is constant across all service requests.</span></span>

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

<span data-ttu-id="950a3-266">In `Startup.ConfigureServices` wird jeder Typ entsprechend seiner benannten Lebensdauer dem Container hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="950a3-266">In `Startup.ConfigureServices`, each type is added to the container according to its named lifetime:</span></span>

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=6-9,12)]

<span data-ttu-id="950a3-267">Der Dienst `IOperationSingletonInstance` verwendet eine bestimmte Instanz mit einer bekannten ID von `Guid.Empty`.</span><span class="sxs-lookup"><span data-stu-id="950a3-267">The `IOperationSingletonInstance` service is using a specific instance with a known ID of `Guid.Empty`.</span></span> <span data-ttu-id="950a3-268">Die Verwendung dieses Typs ist eindeutig (die GUID besteht ausschließlich aus Nullen (0)).</span><span class="sxs-lookup"><span data-stu-id="950a3-268">It's clear when this type is in use (its GUID is all zeroes).</span></span>

<span data-ttu-id="950a3-269">Die Beispiel-App zeigt die Objektlebensdauer innerhalb einer Anforderung und zwischen einzelnen Anforderungen an.</span><span class="sxs-lookup"><span data-stu-id="950a3-269">The sample app demonstrates object lifetimes within and between individual requests.</span></span> <span data-ttu-id="950a3-270">Ihre Klasse `IndexModel` fordert jeden `IOperation`- und `OperationService`-Typ an.</span><span class="sxs-lookup"><span data-stu-id="950a3-270">The sample app's `IndexModel` requests each kind of `IOperation` type and the `OperationService`.</span></span> <span data-ttu-id="950a3-271">Die Seite zeigt dann alle `OperationId`-Werte der Seitenmodellklasse und des Diensts über Eigenschaftenzuweisungen an:</span><span class="sxs-lookup"><span data-stu-id="950a3-271">The page then displays all of the page model class's and service's `OperationId` values through property assignments:</span></span>

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=7-11,14-18,21-25)]

<span data-ttu-id="950a3-272">Zwei folgende Ausgaben zeigen die Ergebnisse von zwei Anforderungen:</span><span class="sxs-lookup"><span data-stu-id="950a3-272">Two following output shows the results of two requests:</span></span>

<span data-ttu-id="950a3-273">**Erste Anforderung:**</span><span class="sxs-lookup"><span data-stu-id="950a3-273">**First request:**</span></span>

<span data-ttu-id="950a3-274">Controllervorgänge:</span><span class="sxs-lookup"><span data-stu-id="950a3-274">Controller operations:</span></span>

<span data-ttu-id="950a3-275">Vorübergehend: d233e165-f417-469b-a866-1cf1935d2518</span><span class="sxs-lookup"><span data-stu-id="950a3-275">Transient: d233e165-f417-469b-a866-1cf1935d2518</span></span>  
<span data-ttu-id="950a3-276">Bereichsbezogen: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="950a3-276">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="950a3-277">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="950a3-277">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="950a3-278">Instanz: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="950a3-278">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="950a3-279">`OperationService`-Vorgänge:</span><span class="sxs-lookup"><span data-stu-id="950a3-279">`OperationService` operations:</span></span>

<span data-ttu-id="950a3-280">Vorübergehend: c6b049eb-1318-4e31-90f1-eb2dd849ff64</span><span class="sxs-lookup"><span data-stu-id="950a3-280">Transient: c6b049eb-1318-4e31-90f1-eb2dd849ff64</span></span>  
<span data-ttu-id="950a3-281">Bereichsbezogen: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="950a3-281">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="950a3-282">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="950a3-282">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="950a3-283">Instanz: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="950a3-283">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="950a3-284">**Zweite Anforderung:**</span><span class="sxs-lookup"><span data-stu-id="950a3-284">**Second request:**</span></span>

<span data-ttu-id="950a3-285">Controllervorgänge:</span><span class="sxs-lookup"><span data-stu-id="950a3-285">Controller operations:</span></span>

<span data-ttu-id="950a3-286">Vorübergehend: b63bd538-0a37-4ff1-90ba-081c5138dda0</span><span class="sxs-lookup"><span data-stu-id="950a3-286">Transient: b63bd538-0a37-4ff1-90ba-081c5138dda0</span></span>  
<span data-ttu-id="950a3-287">Bereichsbezogen: 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="950a3-287">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="950a3-288">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="950a3-288">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="950a3-289">Instanz: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="950a3-289">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="950a3-290">`OperationService`-Vorgänge:</span><span class="sxs-lookup"><span data-stu-id="950a3-290">`OperationService` operations:</span></span>

<span data-ttu-id="950a3-291">Vorübergehend: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span><span class="sxs-lookup"><span data-stu-id="950a3-291">Transient: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span></span>  
<span data-ttu-id="950a3-292">Bereichsbezogen: 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="950a3-292">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="950a3-293">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="950a3-293">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="950a3-294">Instanz: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="950a3-294">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="950a3-295">Beachten Sie, welche der `OperationId`-Werte innerhalb einer Anforderung und zwischen Anforderungen variieren:</span><span class="sxs-lookup"><span data-stu-id="950a3-295">Observe which of the `OperationId` values vary within a request and between requests:</span></span>

* <span data-ttu-id="950a3-296">Objekte vom Typ *Vorübergehend* sind immer unterschiedlich.</span><span class="sxs-lookup"><span data-stu-id="950a3-296">*Transient* objects are always different.</span></span> <span data-ttu-id="950a3-297">Der vorübergehende `OperationId`-Wert für die ersten und zweiten Clientanforderungen ist für beide `OperationService`-Vorgänge und clientanforderungsübergreifend unterschiedlich.</span><span class="sxs-lookup"><span data-stu-id="950a3-297">The transient `OperationId` value for both the first and second client requests are different for both `OperationService` operations and across client requests.</span></span> <span data-ttu-id="950a3-298">Eine neue Instanz wird für jede Dienstanforderung und jede Clientanforderung bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="950a3-298">A new instance is provided to each service request and client request.</span></span>
* <span data-ttu-id="950a3-299">*Bereichsbezogene* Objekte sind innerhalb einer Clientanforderung identisch, clientanforderungsübergreifend sind sie jedoch unterschiedlich.</span><span class="sxs-lookup"><span data-stu-id="950a3-299">*Scoped* objects are the same within a client request but different across client requests.</span></span>
* <span data-ttu-id="950a3-300">Objekte vom Typ *Singleton* sind bei jedem Objekt und in jeder Anforderung identisch (unabhängig davon, ob eine `Operation`-Instanz in `Startup.ConfigureServices` bereitgestellt wird).</span><span class="sxs-lookup"><span data-stu-id="950a3-300">*Singleton* objects are the same for every object and every request regardless of whether an `Operation` instance is provided in `Startup.ConfigureServices`.</span></span>

## <a name="call-services-from-main"></a><span data-ttu-id="950a3-301">Abrufen von Diensten aus „Main“</span><span class="sxs-lookup"><span data-stu-id="950a3-301">Call services from main</span></span>

<span data-ttu-id="950a3-302">Erstellen Sie <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> mit [IServiceScopeFactory.CreateScope](xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope*), um einen bereichsbezogenen Dienst innerhalb des Anwendungsbereichs aufzulösen.</span><span class="sxs-lookup"><span data-stu-id="950a3-302">Create an <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> with [IServiceScopeFactory.CreateScope](xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope*) to resolve a scoped service within the app's scope.</span></span> <span data-ttu-id="950a3-303">Dieser Ansatz eignet sich gut dafür, beim Start auf einen bereichsbezogenen Dienst zuzugreifen und Initialisierungsaufgaben auszuführen.</span><span class="sxs-lookup"><span data-stu-id="950a3-303">This approach is useful to access a scoped service at startup to run initialization tasks.</span></span> <span data-ttu-id="950a3-304">Das folgende Beispiel veranschaulicht, wie man einen Kontext für `MyScopedService` in `Program.Main` erhält:</span><span class="sxs-lookup"><span data-stu-id="950a3-304">The following example shows how to obtain a context for the `MyScopedService` in `Program.Main`:</span></span>

```csharp
using System;
using System.Threading.Tasks;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Hosting;

public class Program
{
    public static async Task Main(string[] args)
    {
        var host = CreateHostBuilder(args).Build();

        using (var serviceScope = host.Services.CreateScope())
        {
            var services = serviceScope.ServiceProvider;

            try
            {
                var serviceContext = services.GetRequiredService<MyScopedService>();
                // Use the context here
            }
            catch (Exception ex)
            {
                var logger = services.GetRequiredService<ILogger<Program>>();
                logger.LogError(ex, "An error occurred.");
            }
        }
    
        await host.RunAsync();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
            {
                webBuilder.UseStartup<Startup>();
            });
}
```

## <a name="scope-validation"></a><span data-ttu-id="950a3-305">Bereichsvalidierung</span><span class="sxs-lookup"><span data-stu-id="950a3-305">Scope validation</span></span>

<span data-ttu-id="950a3-306">Wenn die App in der Entwicklungsumgebung ausgeführt wird und [CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) aufruft, um den Host zu erstellen, überprüft der Standarddienstanbieter, ob:</span><span class="sxs-lookup"><span data-stu-id="950a3-306">When the app is running in the Development environment and calls [CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) to build the host, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="950a3-307">Bereichsbezogene Dienste nicht direkt oder indirekt vom Stammdienstanbieter aufgelöst werden</span><span class="sxs-lookup"><span data-stu-id="950a3-307">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="950a3-308">Bereichsbezogene Dienste nicht direkt oder indirekt in Singletons eingefügt werden</span><span class="sxs-lookup"><span data-stu-id="950a3-308">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="950a3-309">Der Stammdienstanbieter wird erstellt, wenn <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider*> aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="950a3-309">The root service provider is created when <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider*> is called.</span></span> <span data-ttu-id="950a3-310">Die Lebensdauer des Stammdienstanbieters entspricht der Lebensdauer der App/des Servers, wenn der Anbieter mit der App erstellt wird und verworfen wird, wenn die App beendet wird.</span><span class="sxs-lookup"><span data-stu-id="950a3-310">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="950a3-311">Bereichsbezogene Dienste werden von dem Container verworfen, der sie erstellt hat.</span><span class="sxs-lookup"><span data-stu-id="950a3-311">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="950a3-312">Wenn ein bereichsbezogener Dienst im Stammcontainer erstellt wird, wird die Lebensdauer effektiv auf Singleton heraufgestuft, da er nur vom Stammcontainer verworfen wird, wenn die App/der Server heruntergefahren wird.</span><span class="sxs-lookup"><span data-stu-id="950a3-312">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="950a3-313">Die Überprüfung bereichsbezogener Dienste erfasst diese Situationen, wenn `BuildServiceProvider` aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="950a3-313">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="950a3-314">Weitere Informationen finden Sie unter <xref:fundamentals/host/web-host#scope-validation>.</span><span class="sxs-lookup"><span data-stu-id="950a3-314">For more information, see <xref:fundamentals/host/web-host#scope-validation>.</span></span>

## <a name="request-services"></a><span data-ttu-id="950a3-315">Anfordern von Diensten</span><span class="sxs-lookup"><span data-stu-id="950a3-315">Request Services</span></span>

<span data-ttu-id="950a3-316">Die Dienste, die innerhalb einer ASP.NET Core-Anforderung von `HttpContext` verfügbar sind, werden über die [HttpContext.RequestService](xref:Microsoft.AspNetCore.Http.HttpContext.RequestServices)-Sammlung verfügbar gemacht.</span><span class="sxs-lookup"><span data-stu-id="950a3-316">The services available within an ASP.NET Core request from `HttpContext` are exposed through the [HttpContext.RequestServices](xref:Microsoft.AspNetCore.Http.HttpContext.RequestServices) collection.</span></span>

<span data-ttu-id="950a3-317">Anforderungsdienste stellen die Dienste dar, die als Teil der App konfiguriert und angefordert werden.</span><span class="sxs-lookup"><span data-stu-id="950a3-317">Request Services represent the services configured and requested as part of the app.</span></span> <span data-ttu-id="950a3-318">Wenn Objekte Abhängigkeiten angeben, werden diese von den in `RequestServices` gefundenen Typen erfüllt (nicht in `ApplicationServices`).</span><span class="sxs-lookup"><span data-stu-id="950a3-318">When the objects specify dependencies, these are satisfied by the types found in `RequestServices`, not `ApplicationServices`.</span></span>

<span data-ttu-id="950a3-319">Generell sollte die App diese Eigenschaften nicht direkt verwenden.</span><span class="sxs-lookup"><span data-stu-id="950a3-319">Generally, the app shouldn't use these properties directly.</span></span> <span data-ttu-id="950a3-320">Fordern Sie stattdessen die von Klassen benötigten Typen über Klassenkonstruktoren an, und lassen Sie das Framework die Abhängigkeiten einfügen.</span><span class="sxs-lookup"><span data-stu-id="950a3-320">Instead, request the types that classes require via class constructors and allow the framework to inject the dependencies.</span></span> <span data-ttu-id="950a3-321">So erhalten Sie Klassen, die einfacher getestet werden können.</span><span class="sxs-lookup"><span data-stu-id="950a3-321">This yields classes that are easier to test.</span></span>

<span data-ttu-id="950a3-322">ASP.net Core erstellt einen Bereich pro Anforderung und `RequestServices` macht den bereichsbezogenen Dienstanbieter verfügbar.</span><span class="sxs-lookup"><span data-stu-id="950a3-322">ASP.NET Core creates a scope per request and `RequestServices` exposes the scoped service provider.</span></span> <span data-ttu-id="950a3-323">Alle bereichsbezogenen Dienste sind gültig, solange die Anforderung aktiv ist.</span><span class="sxs-lookup"><span data-stu-id="950a3-323">All scoped services are valid for as long as the request is active.</span></span>

> [!NOTE]
> <span data-ttu-id="950a3-324">Fordern Sie für den Zugriff auf die `RequestServices`-Sammlung Abhängigkeiten lieber als Konstruktorparameter an.</span><span class="sxs-lookup"><span data-stu-id="950a3-324">Prefer requesting dependencies as constructor parameters to accessing the `RequestServices` collection.</span></span>

## <a name="design-services-for-dependency-injection"></a><span data-ttu-id="950a3-325">Entwerfen von Diensten für die Abhängigkeitsinjektion</span><span class="sxs-lookup"><span data-stu-id="950a3-325">Design services for dependency injection</span></span>

<span data-ttu-id="950a3-326">Best Practices:</span><span class="sxs-lookup"><span data-stu-id="950a3-326">Best practices are to:</span></span>

* <span data-ttu-id="950a3-327">Entwerfen Sie Dienste zur Verwendung der Abhängigkeitsinjektion, um ihre Abhängigkeiten zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="950a3-327">Design services to use dependency injection to obtain their dependencies.</span></span>
* <span data-ttu-id="950a3-328">Vermeiden Sie zustandsbehaftete statische Klassen und Member.</span><span class="sxs-lookup"><span data-stu-id="950a3-328">Avoid stateful, static classes and members.</span></span> <span data-ttu-id="950a3-329">Entwerfen Sie Apps so, dass stattdessen Singletondienste verwendet werden. Diese vermeiden das Entstehen eines globalen Zustands.</span><span class="sxs-lookup"><span data-stu-id="950a3-329">Design apps to use singleton services instead, which avoid creating global state.</span></span>
* <span data-ttu-id="950a3-330">Vermeiden Sie die direkte Instanziierung abhängiger Klassen innerhalb von Diensten.</span><span class="sxs-lookup"><span data-stu-id="950a3-330">Avoid direct instantiation of dependent classes within services.</span></span> <span data-ttu-id="950a3-331">Die direkte Instanziierung koppelt den Code an eine bestimmte Implementierung.</span><span class="sxs-lookup"><span data-stu-id="950a3-331">Direct instantiation couples the code to a particular implementation.</span></span>
* <span data-ttu-id="950a3-332">Erstellen Sie kleine, gut gestaltete und einfach zu testende Apps.</span><span class="sxs-lookup"><span data-stu-id="950a3-332">Make app classes small, well-factored, and easily tested.</span></span>

<span data-ttu-id="950a3-333">Wenn eine Klasse zu viele eingefügte Abhängigkeiten zu haben scheint, ist dies im Allgemeinen ein Zeichen dafür, dass die Klasse zu viele Aufgaben hat und gegen das [Single-Responsibility-Prinzip (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility) (Prinzip der eindeutigen Verantwortlichkeit) verstößt.</span><span class="sxs-lookup"><span data-stu-id="950a3-333">If a class seems to have too many injected dependencies, this is generally a sign that the class has too many responsibilities and is violating the [Single Responsibility Principle (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility).</span></span> <span data-ttu-id="950a3-334">Versuchen Sie, die Klasse umzugestalten, indem Sie einige ihrer Aufgaben in eine neue Klasse verschieben.</span><span class="sxs-lookup"><span data-stu-id="950a3-334">Attempt to refactor the class by moving some of its responsibilities into a new class.</span></span> <span data-ttu-id="950a3-335">Beachten Sie, dass der Fokus der Razor Pages-Seitenmodellklassen und MVC-Controllerklassen auf der Benutzeroberfläche liegt.</span><span class="sxs-lookup"><span data-stu-id="950a3-335">Keep in mind that Razor Pages page model classes and MVC controller classes should focus on UI concerns.</span></span> <span data-ttu-id="950a3-336">Geschäftsregeln und Implementierungsdetails für den Datenzugriff sollten in Klassen aufbewahrt werden gemäß dem Prinzip [Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) (Trennung der Zuständigkeiten).</span><span class="sxs-lookup"><span data-stu-id="950a3-336">Business rules and data access implementation details should be kept in classes appropriate to these [separate concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns).</span></span>

### <a name="disposal-of-services"></a><span data-ttu-id="950a3-337">Löschen von Diensten</span><span class="sxs-lookup"><span data-stu-id="950a3-337">Disposal of services</span></span>

<span data-ttu-id="950a3-338">Der Container ruft <xref:System.IDisposable.Dispose*> für die erstellten <xref:System.IDisposable>-Typen auf.</span><span class="sxs-lookup"><span data-stu-id="950a3-338">The container calls <xref:System.IDisposable.Dispose*> for the <xref:System.IDisposable> types it creates.</span></span> <span data-ttu-id="950a3-339">Wenn eine Instanz dem Container per Benutzercode hinzugefügt wird, wird sie nicht automatisch gelöscht.</span><span class="sxs-lookup"><span data-stu-id="950a3-339">If an instance is added to the container by user code, it isn't disposed automatically.</span></span>

<span data-ttu-id="950a3-340">Im folgenden Beispiel werden die Dienste vom Dienstcontainer erstellt und automatisch verworfen:</span><span class="sxs-lookup"><span data-stu-id="950a3-340">In the following example, the services are created by the service container and disposed automatically:</span></span>

```csharp
public class Service1 : IDisposable {}
public class Service2 : IDisposable {}

public interface IService3 {}
public class Service3 : IService3, IDisposable {}

public void ConfigureServices(IServiceCollection services)
{
    services.AddScoped<Service1>();
    services.AddSingleton<Service2>();
    services.AddSingleton<IService3>(sp => new Service3());
}
```

<span data-ttu-id="950a3-341">Im folgenden Beispiel:</span><span class="sxs-lookup"><span data-stu-id="950a3-341">In the following example:</span></span>

* <span data-ttu-id="950a3-342">werden die Dienstinstanzen nicht vom Dienstcontainer erstellt.</span><span class="sxs-lookup"><span data-stu-id="950a3-342">The service instances aren't created by the service container.</span></span>
* <span data-ttu-id="950a3-343">kennt das Framework die geplante Lebensdauer der Dienste nicht.</span><span class="sxs-lookup"><span data-stu-id="950a3-343">The intended service lifetimes aren't known by the framework.</span></span>
* <span data-ttu-id="950a3-344">verwirft das Framework die Dienste nicht automatisch.</span><span class="sxs-lookup"><span data-stu-id="950a3-344">The framework doesn't dispose of the services automatically.</span></span>
* <span data-ttu-id="950a3-345">werden Dienste, die nicht explizit im Entwicklercode verworfen werden, weiter ausgeführt, bis die App beendet wird.</span><span class="sxs-lookup"><span data-stu-id="950a3-345">If the services aren't explicitly disposed in developer code, they persist until the app shuts down.</span></span>

```csharp
public class Service1 : IDisposable {}
public class Service2 : IDisposable {}

public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<Service1>(new Service1());
    services.AddSingleton(new Service2());
}
```

### <a name="idisposable-guidance-for-transient-and-shared-instances"></a><span data-ttu-id="950a3-346">IDisposable-Anleitung für vorübergehende and freigegebene Instanzen</span><span class="sxs-lookup"><span data-stu-id="950a3-346">IDisposable guidance for Transient and shared instances</span></span>

#### <a name="transient-limited-lifetime"></a><span data-ttu-id="950a3-347">Vorübergehende, eingeschränkte Lebensdauer</span><span class="sxs-lookup"><span data-stu-id="950a3-347">Transient, limited lifetime</span></span>

<span data-ttu-id="950a3-348">**Szenario**</span><span class="sxs-lookup"><span data-stu-id="950a3-348">**Scenario**</span></span>

<span data-ttu-id="950a3-349">Für die App ist eine <xref:System.IDisposable>-Instanz mit einer vorübergehenden Lebensdauer für eines der folgenden Szenarios erforderlich:</span><span class="sxs-lookup"><span data-stu-id="950a3-349">The app requires an <xref:System.IDisposable> instance with a transient lifetime for either of the following scenarios:</span></span>

* <span data-ttu-id="950a3-350">Die-Instanz wird im Stammbereich aufgelöst.</span><span class="sxs-lookup"><span data-stu-id="950a3-350">The instance is resolved in the root scope.</span></span>
* <span data-ttu-id="950a3-351">Die Instanz sollte verworfen werden, bevor der Bereich endet.</span><span class="sxs-lookup"><span data-stu-id="950a3-351">The instance should be disposed before the scope ends.</span></span>

<span data-ttu-id="950a3-352">**Lösung**</span><span class="sxs-lookup"><span data-stu-id="950a3-352">**Solution**</span></span>

<span data-ttu-id="950a3-353">Verwenden Sie das Factorymuster, um eine Instanz außerhalb des übergeordneten Bereichs zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="950a3-353">Use the factory pattern to create an instance outside of the parent scope.</span></span> <span data-ttu-id="950a3-354">In dieser Situation verfügt die App in der Regel über eine `Create`-Methode, die den Konstruktor des endgültigen Typs direkt aufruft.</span><span class="sxs-lookup"><span data-stu-id="950a3-354">In this situation, the app would generally have a `Create` method that calls the final type's constructor directly.</span></span> <span data-ttu-id="950a3-355">Wenn der endgültige Typ andere Abhängigkeiten aufweist, kann die Factory folgende Aktionen ausführen:</span><span class="sxs-lookup"><span data-stu-id="950a3-355">If the final type has other dependencies, the factory can:</span></span>

* <span data-ttu-id="950a3-356">Sie kann <xref:System.IServiceProvider> im Konstruktor empfangen.</span><span class="sxs-lookup"><span data-stu-id="950a3-356">Receive an <xref:System.IServiceProvider> in its constructor.</span></span>
* <span data-ttu-id="950a3-357">Sie kann <xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities.CreateInstance%2A?displayProperty=nameWithType> verwenden, um eine Instanz außerhalb des Containers zu instanziieren und dabei den Container für die Abhängigkeiten verwenden.</span><span class="sxs-lookup"><span data-stu-id="950a3-357">Use <xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities.CreateInstance%2A?displayProperty=nameWithType> to instantiate the instance outside the container, while using the container for its dependencies.</span></span>

#### <a name="shared-instance-limited-lifetime"></a><span data-ttu-id="950a3-358">Freigegebene Instanz, eingeschränkte Lebensdauer</span><span class="sxs-lookup"><span data-stu-id="950a3-358">Shared Instance, limited lifetime</span></span>

<span data-ttu-id="950a3-359">**Szenario**</span><span class="sxs-lookup"><span data-stu-id="950a3-359">**Scenario**</span></span>

<span data-ttu-id="950a3-360">Für die App ist eine freigegebene <xref:System.IDisposable>-Instanz über mehrere Dienste erforderlich, die <xref:System.IDisposable> sollte jedoch eine begrenzte Lebensdauer haben.</span><span class="sxs-lookup"><span data-stu-id="950a3-360">The app requires a shared <xref:System.IDisposable> instance across multiple services, but the <xref:System.IDisposable> should have a limited lifetime.</span></span>

<span data-ttu-id="950a3-361">**Lösung**</span><span class="sxs-lookup"><span data-stu-id="950a3-361">**Solution**</span></span>

<span data-ttu-id="950a3-362">Registrieren Sie die Instanz mit einer bereichsbezogenen Lebensdauer.</span><span class="sxs-lookup"><span data-stu-id="950a3-362">Register the instance with a Scoped lifetime.</span></span> <span data-ttu-id="950a3-363">Verwenden Sie <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope%2A?displayProperty=nameWithType> für den Start und zum Erstellen einer neuen <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>.</span><span class="sxs-lookup"><span data-stu-id="950a3-363">Use <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope%2A?displayProperty=nameWithType> to start and create a new <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>.</span></span> <span data-ttu-id="950a3-364">Verwenden Sie die <xref:System.IServiceProvider>-Schnittstelle des Bereichs, um die erforderlichen Dienste abzurufen.</span><span class="sxs-lookup"><span data-stu-id="950a3-364">Use the scope's <xref:System.IServiceProvider> to get required services.</span></span> <span data-ttu-id="950a3-365">Löschen Sie den Bereich, wenn die Lebensdauer beendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="950a3-365">Dispose the scope when the lifetime should end.</span></span>

#### <a name="general-guidelines"></a><span data-ttu-id="950a3-366">Allgemeine Richtlinien</span><span class="sxs-lookup"><span data-stu-id="950a3-366">General Guidelines</span></span>

* <span data-ttu-id="950a3-367">Registrieren Sie keine <xref:System.IDisposable>-Instanzen mit einem vorübergehenden Gültigkeitsbereich.</span><span class="sxs-lookup"><span data-stu-id="950a3-367">Don't register <xref:System.IDisposable> instances with a Transient scope.</span></span> <span data-ttu-id="950a3-368">Verwenden Sie stattdessen das Factorymuster.</span><span class="sxs-lookup"><span data-stu-id="950a3-368">Use the factory pattern instead.</span></span>
* <span data-ttu-id="950a3-369">Lösen Sie keine vorübergehenden oder bereichsbezogenen <xref:System.IDisposable>-Instanzen im Stammbereich auf.</span><span class="sxs-lookup"><span data-stu-id="950a3-369">Don't resolve Transient or Scoped <xref:System.IDisposable> instances in the root scope.</span></span> <span data-ttu-id="950a3-370">Die einzige allgemeine Ausnahme gilt, wenn die App die <xref:System.IServiceProvider>-Instanz erstellt bzw. erneut erstellt und freigibt, was kein ideales Muster ist.</span><span class="sxs-lookup"><span data-stu-id="950a3-370">The only general exception is when the app creates/recreates and disposes the <xref:System.IServiceProvider>, which isn't an ideal pattern.</span></span>
* <span data-ttu-id="950a3-371">Wenn eine <xref:System.IDisposable>-Abhängigkeit über DI empfangen wird, ist es nicht erforderlich, dass der Empfänger <xref:System.IDisposable> selbst implementiert.</span><span class="sxs-lookup"><span data-stu-id="950a3-371">Receiving an <xref:System.IDisposable> dependency via DI doesn't require that the receiver implement <xref:System.IDisposable> itself.</span></span> <span data-ttu-id="950a3-372">Der Empfänger der <xref:System.IDisposable>-Abhängigkeit darf <xref:System.IDisposable.Dispose%2A> auf dieser Abhängigkeit nicht abrufen.</span><span class="sxs-lookup"><span data-stu-id="950a3-372">The receiver of the <xref:System.IDisposable> dependency shouldn't call <xref:System.IDisposable.Dispose%2A> on that dependency.</span></span>
* <span data-ttu-id="950a3-373">Bereiche sollten verwendet werden, um die Lebensdauer von Diensten zu steuern.</span><span class="sxs-lookup"><span data-stu-id="950a3-373">Scopes should be used to control lifetimes of services.</span></span> <span data-ttu-id="950a3-374">Bereiche sind nicht hierarchisch, und es gibt keine besondere Verbindung zwischen den Bereichen.</span><span class="sxs-lookup"><span data-stu-id="950a3-374">Scopes aren't hierarchical, and there's no special connection among scopes.</span></span>

## <a name="default-service-container-replacement"></a><span data-ttu-id="950a3-375">Ersetzen von Standarddienstcontainern</span><span class="sxs-lookup"><span data-stu-id="950a3-375">Default service container replacement</span></span>

<span data-ttu-id="950a3-376">Der integrierte Dienstcontainer dient dazu, die Anforderungen des Frameworks und der meisten Consumer-Apps zu erfüllen.</span><span class="sxs-lookup"><span data-stu-id="950a3-376">The built-in service container is designed to serve the needs of the framework and most consumer apps.</span></span> <span data-ttu-id="950a3-377">Die Verwendung des integrierten Containers wird empfohlen, es sei denn, Sie benötigen ein bestimmtes Feature, das von diesem nicht unterstützt wird. Einige Beispiele:</span><span class="sxs-lookup"><span data-stu-id="950a3-377">We recommend using the built-in container unless you need a specific feature that the built-in container doesn't support, such as:</span></span>

* <span data-ttu-id="950a3-378">Eigenschaftsinjektion</span><span class="sxs-lookup"><span data-stu-id="950a3-378">Property injection</span></span>
* <span data-ttu-id="950a3-379">Auf Namen basierende Injektion</span><span class="sxs-lookup"><span data-stu-id="950a3-379">Injection based on name</span></span>
* <span data-ttu-id="950a3-380">Untergeordnete Container</span><span class="sxs-lookup"><span data-stu-id="950a3-380">Child containers</span></span>
* <span data-ttu-id="950a3-381">Benutzerdefinierte Verwaltung der Lebensdauer</span><span class="sxs-lookup"><span data-stu-id="950a3-381">Custom lifetime management</span></span>
* <span data-ttu-id="950a3-382">`Func<T>`-Unterstützung für die verzögerte Initialisierung</span><span class="sxs-lookup"><span data-stu-id="950a3-382">`Func<T>` support for lazy initialization</span></span>
* <span data-ttu-id="950a3-383">Konventionsbasierte Registrierung</span><span class="sxs-lookup"><span data-stu-id="950a3-383">Convention-based registration</span></span>

<span data-ttu-id="950a3-384">Die folgenden Container von Drittanbietern können mit ASP.NET Core-Apps verwendet werden:</span><span class="sxs-lookup"><span data-stu-id="950a3-384">The following 3rd party containers can be used with ASP.NET Core apps:</span></span>

* [<span data-ttu-id="950a3-385">Autofac</span><span class="sxs-lookup"><span data-stu-id="950a3-385">Autofac</span></span>](https://autofac.readthedocs.io/en/latest/integration/aspnetcore.html)
* [<span data-ttu-id="950a3-386">DryIoc</span><span class="sxs-lookup"><span data-stu-id="950a3-386">DryIoc</span></span>](https://www.nuget.org/packages/DryIoc.Microsoft.DependencyInjection)
* [<span data-ttu-id="950a3-387">Grace</span><span class="sxs-lookup"><span data-stu-id="950a3-387">Grace</span></span>](https://www.nuget.org/packages/Grace.DependencyInjection.Extensions)
* [<span data-ttu-id="950a3-388">LightInject</span><span class="sxs-lookup"><span data-stu-id="950a3-388">LightInject</span></span>](https://github.com/seesharper/LightInject.Microsoft.DependencyInjection)
* [<span data-ttu-id="950a3-389">Lamar</span><span class="sxs-lookup"><span data-stu-id="950a3-389">Lamar</span></span>](https://jasperfx.github.io/lamar/)
* [<span data-ttu-id="950a3-390">Stashbox</span><span class="sxs-lookup"><span data-stu-id="950a3-390">Stashbox</span></span>](https://github.com/z4kn4fein/stashbox-extensions-dependencyinjection)
* [<span data-ttu-id="950a3-391">Unity</span><span class="sxs-lookup"><span data-stu-id="950a3-391">Unity</span></span>](https://www.nuget.org/packages/Unity.Microsoft.DependencyInjection)

### <a name="thread-safety"></a><span data-ttu-id="950a3-392">Threadsicherheit</span><span class="sxs-lookup"><span data-stu-id="950a3-392">Thread safety</span></span>

<span data-ttu-id="950a3-393">Erstellen Sie threadsichere Singleton-Dienste.</span><span class="sxs-lookup"><span data-stu-id="950a3-393">Create thread-safe singleton services.</span></span> <span data-ttu-id="950a3-394">Wenn ein Singleton-Dienst eine Abhängigkeit von einem vorübergehenden Dienst aufweist, muss der vorübergehende Dienst abhängig von der Verwendungsweise durch das Singleton ebenfalls threadsicher sein.</span><span class="sxs-lookup"><span data-stu-id="950a3-394">If a singleton service has a dependency on a transient service, the transient service may also require thread safety depending how it's used by the singleton.</span></span>

<span data-ttu-id="950a3-395">Die Factorymethode des einzelnen Diensts, z. B. das zweite Argument für [AddSingleton\<TService>(IServiceCollection, Func\<IServiceProvider,TService>)](xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*), muss nicht threadsicher sein.</span><span class="sxs-lookup"><span data-stu-id="950a3-395">The factory method of single service, such as the second argument to [AddSingleton\<TService>(IServiceCollection, Func\<IServiceProvider,TService>)](xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*), doesn't need to be thread-safe.</span></span> <span data-ttu-id="950a3-396">Wie bei `static`-Konstruktoren erfolgt der Aufruf einmalig über einen einzelnen Thread.</span><span class="sxs-lookup"><span data-stu-id="950a3-396">Like a type (`static`) constructor, it's guaranteed to be called once by a single thread.</span></span>

## <a name="recommendations"></a><span data-ttu-id="950a3-397">Empfehlungen</span><span class="sxs-lookup"><span data-stu-id="950a3-397">Recommendations</span></span>

* <span data-ttu-id="950a3-398">`async/await`- und `Task`-basierte Dienstauflösung wird nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="950a3-398">`async/await` and `Task` based service resolution is not supported.</span></span> <span data-ttu-id="950a3-399">C# unterstützt keine asynchronen Konstruktoren. Daher wird empfohlen, asynchrone Methoden zu verwenden, nachdem der Dienst synchron aufgelöst wurde.</span><span class="sxs-lookup"><span data-stu-id="950a3-399">C# does not support asynchronous constructors; therefore, the recommended pattern is to use asynchronous methods after synchronously resolving the service.</span></span>

* <span data-ttu-id="950a3-400">Vermeiden Sie das Speichern von Daten und die direkte Konfiguration im Dienstcontainer.</span><span class="sxs-lookup"><span data-stu-id="950a3-400">Avoid storing data and configuration directly in the service container.</span></span> <span data-ttu-id="950a3-401">Der Einkaufswagen eines Benutzers sollte z. B. normalerweise nicht dem Dienstcontainer hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="950a3-401">For example, a user's shopping cart shouldn't typically be added to the service container.</span></span> <span data-ttu-id="950a3-402">Bei der Konfiguration sollte das [Optionsmuster](xref:fundamentals/configuration/options) verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="950a3-402">Configuration should use the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="950a3-403">Gleichermaßen sollten Sie „Datencontainer“-Objekte vermeiden, die nur vorhanden sind, um den Zugriff auf einige andere Objekte zuzulassen.</span><span class="sxs-lookup"><span data-stu-id="950a3-403">Similarly, avoid "data holder" objects that only exist to allow access to some other object.</span></span> <span data-ttu-id="950a3-404">Das tatsächlich benötige Element sollte besser über Dependency Injection angefordert werden.</span><span class="sxs-lookup"><span data-stu-id="950a3-404">It's better to request the actual item via DI.</span></span>

* <span data-ttu-id="950a3-405">Vermeiden Sie den statischen Zugriff auf Dienste (z. B. statische Eingabe von [IApplicationBuilder.ApplicationServices](xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices) zur Verwendung an anderer Stelle).</span><span class="sxs-lookup"><span data-stu-id="950a3-405">Avoid static access to services (for example, statically-typing [IApplicationBuilder.ApplicationServices](xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices) for use elsewhere).</span></span>

* <span data-ttu-id="950a3-406">Vermeiden Sie die Verwendung von *Dienstlocator-Mustern*.</span><span class="sxs-lookup"><span data-stu-id="950a3-406">Avoid using the *service locator pattern*.</span></span> <span data-ttu-id="950a3-407">Rufen Sie beispielsweise nicht <xref:System.IServiceProvider.GetService*> auf, um eine Dienstinstanz zu erhalten, wenn Sie stattdessen Dependency Injection verwenden können:</span><span class="sxs-lookup"><span data-stu-id="950a3-407">For example, don't invoke <xref:System.IServiceProvider.GetService*> to obtain a service instance when you can use DI instead:</span></span>

  <span data-ttu-id="950a3-408">**Falsch:**</span><span class="sxs-lookup"><span data-stu-id="950a3-408">**Incorrect:**</span></span>

  ```csharp
  public class MyClass()
  {
      public void MyMethod()
      {
          var optionsMonitor = 
              _services.GetService<IOptionsMonitor<MyOptions>>();
          var option = optionsMonitor.CurrentValue.Option;

          ...
      }
  }
  ```

  <span data-ttu-id="950a3-409">**Richtig**:</span><span class="sxs-lookup"><span data-stu-id="950a3-409">**Correct**:</span></span>

  ```csharp
  public class MyClass
  {
      private readonly IOptionsMonitor<MyOptions> _optionsMonitor;

      public MyClass(IOptionsMonitor<MyOptions> optionsMonitor)
      {
          _optionsMonitor = optionsMonitor;
      }

      public void MyMethod()
      {
          var option = _optionsMonitor.CurrentValue.Option;

          ...
      }
  }
  ```

* <span data-ttu-id="950a3-410">Eine andere Dienstlocator-Variante, die Sie vermeiden sollten, ist die Injektion einer Factory, die zur Laufzeit Abhängigkeiten auflöst.</span><span class="sxs-lookup"><span data-stu-id="950a3-410">Another service locator variation to avoid is injecting a factory that resolves dependencies at runtime.</span></span> <span data-ttu-id="950a3-411">Beide Vorgehensweisen kombinieren Strategien zur [Umkehrung der Steuerung](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion).</span><span class="sxs-lookup"><span data-stu-id="950a3-411">Both of these practices mix [Inversion of Control](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) strategies.</span></span>

* <span data-ttu-id="950a3-412">Vermeiden Sie den statischen Zugriff auf `HttpContext` (z. B. [IHttpContextAccessor.HttpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext)).</span><span class="sxs-lookup"><span data-stu-id="950a3-412">Avoid static access to `HttpContext` (for example, [IHttpContextAccessor.HttpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext)).</span></span>

<span data-ttu-id="950a3-413">Wie bei allen Empfehlungen treffen Sie möglicherweise auf Situationen, in denen eine Empfehlung ignoriert werden muss.</span><span class="sxs-lookup"><span data-stu-id="950a3-413">Like all sets of recommendations, you may encounter situations where ignoring a recommendation is required.</span></span> <span data-ttu-id="950a3-414">Es gibt nur wenige Ausnahmen, die sich meistens auf besondere Fälle innerhalb des Frameworks beziehen.</span><span class="sxs-lookup"><span data-stu-id="950a3-414">Exceptions are rare, mostly special cases within the framework itself.</span></span>

<span data-ttu-id="950a3-415">Dependency Injection stellt eine *Alternative* zu statischen bzw. globalen Objektzugriffsmustern dar.</span><span class="sxs-lookup"><span data-stu-id="950a3-415">DI is an *alternative* to static/global object access patterns.</span></span> <span data-ttu-id="950a3-416">Sie werden keinen Nutzen aus der Dependency Injection ziehen können, wenn Sie diese mit dem Zugriff auf statische Objekte kombinieren.</span><span class="sxs-lookup"><span data-stu-id="950a3-416">You may not be able to realize the benefits of DI if you mix it with static object access.</span></span>

## <a name="recommended-patterns-for-multi-tenancy-in-di"></a><span data-ttu-id="950a3-417">Empfohlene Muster für Mehrinstanzenfähigkeit in der Dependency Injection (DI)</span><span class="sxs-lookup"><span data-stu-id="950a3-417">Recommended patterns for multi-tenancy in DI</span></span>

<span data-ttu-id="950a3-418">[Orchard Core](https://github.com/OrchardCMS/OrchardCore) bietet die Mehrinstanzenfähigkeit.</span><span class="sxs-lookup"><span data-stu-id="950a3-418">[Orchard Core](https://github.com/OrchardCMS/OrchardCore) provides multi-tenancy.</span></span> <span data-ttu-id="950a3-419">Weitere Informationen finden Sie in der [Orchard Core-Dokumentation](https://docs.orchardcore.net/en/dev/).</span><span class="sxs-lookup"><span data-stu-id="950a3-419">For more information, see the [Orchard Core Documentation](https://docs.orchardcore.net/en/dev/).</span></span>

<span data-ttu-id="950a3-420">Beispiele und Vorgehensweisen zum Erstellen modularer Apps und mehrinstanzenfähigen Apps nur mit dem Orchard Core-Framework aber ohne CMS-spezifische Features finden Sie in den Beispiel-Apps unter https://github.com/OrchardCMS/OrchardCore.Samples.</span><span class="sxs-lookup"><span data-stu-id="950a3-420">See the samples apps at https://github.com/OrchardCMS/OrchardCore.Samples for examples of how to build modular and multi-tenant apps using just Orchard Core Framework without any of the CMS specific features.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="950a3-421">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="950a3-421">Additional resources</span></span>

* <xref:mvc/views/dependency-injection>
* <xref:mvc/controllers/dependency-injection>
* <xref:security/authorization/dependencyinjection>
* <xref:blazor/fundamentals/dependency-injection>
* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/extensibility>
* [<span data-ttu-id="950a3-422">Vier Möglichkeiten zum Verwerfen von IDisposables in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="950a3-422">Four ways to dispose IDisposables in ASP.NET Core</span></span>](https://andrewlock.net/four-ways-to-dispose-idisposables-in-asp-net-core/)
* [<span data-ttu-id="950a3-423">Schreiben von sauberem Code in ASP.NET Core über Dependency Injection (MSDN)</span><span class="sxs-lookup"><span data-stu-id="950a3-423">Writing Clean Code in ASP.NET Core with Dependency Injection (MSDN)</span></span>](https://msdn.microsoft.com/magazine/mt703433.aspx)
* [<span data-ttu-id="950a3-424">Prinzip der expliziten Abhängigkeiten</span><span class="sxs-lookup"><span data-stu-id="950a3-424">Explicit Dependencies Principle</span></span>](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)
* <span data-ttu-id="950a3-425">[Umkehrung von Steuerungscontainern und das Abhängigkeitsinjektionsmuster (Martin Fowler)](https://www.martinfowler.com/articles/injection.html) (in englischer Sprache)</span><span class="sxs-lookup"><span data-stu-id="950a3-425">[Inversion of Control Containers and the Dependency Injection Pattern (Martin Fowler)](https://www.martinfowler.com/articles/injection.html)</span></span>
* [<span data-ttu-id="950a3-426">Registrieren eines Diensts mit mehreren Schnittstellen in ASP.NET Core DI</span><span class="sxs-lookup"><span data-stu-id="950a3-426">How to register a service with multiple interfaces in ASP.NET Core DI</span></span>](https://andrewlock.net/how-to-register-a-service-with-multiple-interfaces-for-in-asp-net-core-di/)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="950a3-427">ASP.NET Core unterstützt das Softwareentwurfsmuster Abhängigkeitsinjektion. Damit kann eine [Umkehrung der Steuerung](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) (Inversion of Control, IoC) zwischen Klassen und ihren Abhängigkeiten erreicht werden.</span><span class="sxs-lookup"><span data-stu-id="950a3-427">ASP.NET Core supports the dependency injection (DI) software design pattern, which is a technique for achieving [Inversion of Control (IoC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) between classes and their dependencies.</span></span>

<span data-ttu-id="950a3-428">Weitere Informationen zur Abhängigkeitsinjektion innerhalb von MVC-Controllern finden Sie unter <xref:mvc/controllers/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="950a3-428">For more information specific to dependency injection within MVC controllers, see <xref:mvc/controllers/dependency-injection>.</span></span>

<span data-ttu-id="950a3-429">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([Vorgehensweise zum Herunterladen](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="950a3-429">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="overview-of-dependency-injection"></a><span data-ttu-id="950a3-430">Übersicht über Abhängigkeitsinjektion</span><span class="sxs-lookup"><span data-stu-id="950a3-430">Overview of dependency injection</span></span>

<span data-ttu-id="950a3-431">Eine *Abhängigkeit* ist ein beliebiges Objekt, das ein anderes Objekt benötigt.</span><span class="sxs-lookup"><span data-stu-id="950a3-431">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="950a3-432">Überprüfen Sie die folgende `MyDependency`-Klasse mit einer `WriteMessage`-Methode, von der andere Klassen in einer App abhängen:</span><span class="sxs-lookup"><span data-stu-id="950a3-432">Examine the following `MyDependency` class with a `WriteMessage` method that other classes in an app depend upon:</span></span>

```csharp
public class MyDependency
{
    public MyDependency()
    {
    }

    public Task WriteMessage(string message)
    {
        Console.WriteLine(
            $"MyDependency.WriteMessage called. Message: {message}");

        return Task.FromResult(0);
    }
}
```

<span data-ttu-id="950a3-433">Eine Instanz der Klasse `MyDependency` kann erstellt werden, um die `WriteMessage`-Methode einer Klasse zur Verfügung zu stellen.</span><span class="sxs-lookup"><span data-stu-id="950a3-433">An instance of the `MyDependency` class can be created to make the `WriteMessage` method available to a class.</span></span> <span data-ttu-id="950a3-434">Die Klasse `MyDependency` ist eine Abhängigkeit der Klasse `IndexModel`:</span><span class="sxs-lookup"><span data-stu-id="950a3-434">The `MyDependency` class is a dependency of the `IndexModel` class:</span></span>

```csharp
public class IndexModel : PageModel
{
    MyDependency _dependency = new MyDependency();

    public async Task OnGetAsync()
    {
        await _dependency.WriteMessage(
            "IndexModel.OnGetAsync created this message.");
    }
}
```

<span data-ttu-id="950a3-435">Die Klasse erstellt die Instanz `MyDependency` und hängt direkt von dieser ab.</span><span class="sxs-lookup"><span data-stu-id="950a3-435">The class creates and directly depends on the `MyDependency` instance.</span></span> <span data-ttu-id="950a3-436">Codeabhängigkeiten (wie im vorherigen Beispiel) sind problematisch und sollten aus folgenden Gründen vermieden werden:</span><span class="sxs-lookup"><span data-stu-id="950a3-436">Code dependencies (such as the previous example) are problematic and should be avoided for the following reasons:</span></span>

* <span data-ttu-id="950a3-437">Um `MyDependency` durch eine andere Implementierung zu ersetzen, muss die Klasse geändert werden.</span><span class="sxs-lookup"><span data-stu-id="950a3-437">To replace `MyDependency` with a different implementation, the class must be modified.</span></span>
* <span data-ttu-id="950a3-438">Wenn `MyDependency` über Abhängigkeiten verfügt, müssen diese von der Klasse konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="950a3-438">If `MyDependency` has dependencies, they must be configured by the class.</span></span> <span data-ttu-id="950a3-439">In einem großen Projekt mit mehreren Klassen, die von `MyDependency` abhängig sind, wird der Konfigurationscode über die App verteilt.</span><span class="sxs-lookup"><span data-stu-id="950a3-439">In a large project with multiple classes depending on `MyDependency`, the configuration code becomes scattered across the app.</span></span>
* <span data-ttu-id="950a3-440">Diese Implementierung ist nicht für Komponententests geeignet.</span><span class="sxs-lookup"><span data-stu-id="950a3-440">This implementation is difficult to unit test.</span></span> <span data-ttu-id="950a3-441">Die App sollte eine `MyDependency`-Modell- oder Stubklasse verwenden, was mit diesem Ansatz nicht möglich ist.</span><span class="sxs-lookup"><span data-stu-id="950a3-441">The app should use a mock or stub `MyDependency` class, which isn't possible with this approach.</span></span>

<span data-ttu-id="950a3-442">Die Abhängigkeitsinjektion löst dieses Problem mithilfe der folgenden Schritte:</span><span class="sxs-lookup"><span data-stu-id="950a3-442">Dependency injection addresses these problems through:</span></span>

* <span data-ttu-id="950a3-443">Die Verwendung einer Schnittstelle oder Basisklasse zur Abstraktion der Abhängigkeitsimplementierung.</span><span class="sxs-lookup"><span data-stu-id="950a3-443">The use of an interface or base class to abstract the dependency implementation.</span></span>
* <span data-ttu-id="950a3-444">Registrierung der Abhängigkeit in einem Dienstcontainer.</span><span class="sxs-lookup"><span data-stu-id="950a3-444">Registration of the dependency in a service container.</span></span> <span data-ttu-id="950a3-445">ASP.NET Core stellt einen integrierten Dienstcontainer (<xref:System.IServiceProvider>) bereit.</span><span class="sxs-lookup"><span data-stu-id="950a3-445">ASP.NET Core provides a built-in service container, <xref:System.IServiceProvider>.</span></span> <span data-ttu-id="950a3-446">Die Dienste werden in der App-Methode `Startup.ConfigureServices` registriert.</span><span class="sxs-lookup"><span data-stu-id="950a3-446">Services are registered in the app's `Startup.ConfigureServices` method.</span></span>
* <span data-ttu-id="950a3-447">Die *Injektion* des Diensts in den Konstruktor der Klasse, wo er verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="950a3-447">*Injection* of the service into the constructor of the class where it's used.</span></span> <span data-ttu-id="950a3-448">Das Framework erstellt eine Instanz der Abhängigkeit und entfernt diese, wenn sie nicht mehr benötigt wird.</span><span class="sxs-lookup"><span data-stu-id="950a3-448">The framework takes on the responsibility of creating an instance of the dependency and disposing of it when it's no longer needed.</span></span>

<span data-ttu-id="950a3-449">In der [Beispiel-App](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) definiert die `IMyDependency`-Schnittstelle eine Methode, die der Dienst für die App bereitstellt:</span><span class="sxs-lookup"><span data-stu-id="950a3-449">In the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples), the `IMyDependency` interface defines a method that the service provides to the app:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

<span data-ttu-id="950a3-450">Diese Schnittstelle wird durch einen konkreten Typ (`MyDependency`) implementiert:</span><span class="sxs-lookup"><span data-stu-id="950a3-450">This interface is implemented by a concrete type, `MyDependency`:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

<span data-ttu-id="950a3-451">`MyDependency` fordert einen <xref:Microsoft.Extensions.Logging.ILogger`1> beim zugehörigen Konstruktor an.</span><span class="sxs-lookup"><span data-stu-id="950a3-451">`MyDependency` requests an <xref:Microsoft.Extensions.Logging.ILogger`1> in its constructor.</span></span> <span data-ttu-id="950a3-452">Die Abhängigkeitsinjektion wird häufig als Verkettung verwendet.</span><span class="sxs-lookup"><span data-stu-id="950a3-452">It's not unusual to use dependency injection in a chained fashion.</span></span> <span data-ttu-id="950a3-453">Jede angeforderte Abhängigkeit fordert wiederum ihre eigenen Abhängigkeiten an.</span><span class="sxs-lookup"><span data-stu-id="950a3-453">Each requested dependency in turn requests its own dependencies.</span></span> <span data-ttu-id="950a3-454">Der Container löst die Abhängigkeiten im Diagramm auf und gibt den vollständig aufgelösten Dienst zurück.</span><span class="sxs-lookup"><span data-stu-id="950a3-454">The container resolves the dependencies in the graph and returns the fully resolved service.</span></span> <span data-ttu-id="950a3-455">Die gesammelten aufzulösenden Abhängigkeiten werden als *Abhängigkeitsstruktur*, *Abhängigkeitsdiagramm* oder *Objektdiagramm* bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="950a3-455">The collective set of dependencies that must be resolved is typically referred to as a *dependency tree*, *dependency graph*, or *object graph*.</span></span>

<span data-ttu-id="950a3-456">`IMyDependency` und `ILogger<TCategoryName>` müssen im Dienstcontainer registriert werden.</span><span class="sxs-lookup"><span data-stu-id="950a3-456">`IMyDependency` and `ILogger<TCategoryName>` must be registered in the service container.</span></span> <span data-ttu-id="950a3-457">`IMyDependency` ist in `Startup.ConfigureServices` registriert.</span><span class="sxs-lookup"><span data-stu-id="950a3-457">`IMyDependency` is registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="950a3-458">`ILogger<TCategoryName>` wird von der Protokollierungsabstraktionsinfrastruktur registriert. Es handelt sich also um einen [von einem Framework bereitgestellten Dienst](#framework-provided-services), der standardmäßig vom Framework registriert wird.</span><span class="sxs-lookup"><span data-stu-id="950a3-458">`ILogger<TCategoryName>` is registered by the logging abstractions infrastructure, so it's a [framework-provided service](#framework-provided-services) registered by default by the framework.</span></span>

<span data-ttu-id="950a3-459">Der Container löst `ILogger<TCategoryName>` unter Verwendung der [(generischen) offenen Typen](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types) auf, wodurch die Notwendigkeit entfällt, jeden [(generischen) konstruierten Typ](/dotnet/csharp/language-reference/language-specification/types#constructed-types) zu registrieren:</span><span class="sxs-lookup"><span data-stu-id="950a3-459">The container resolves `ILogger<TCategoryName>` by taking advantage of [(generic) open types](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types), eliminating the need to register every [(generic) constructed type](/dotnet/csharp/language-reference/language-specification/types#constructed-types):</span></span>

```csharp
services.AddSingleton(typeof(ILogger<>), typeof(Logger<>));
```

<span data-ttu-id="950a3-460">In der Beispiel-App ist der Dienst `IMyDependency` mit dem konkreten Typ `MyDependency` registriert.</span><span class="sxs-lookup"><span data-stu-id="950a3-460">In the sample app, the `IMyDependency` service is registered with the concrete type `MyDependency`.</span></span> <span data-ttu-id="950a3-461">Die Registrierung schränkt die Lebensdauer des Diensts auf die Lebensdauer einer einzelnen Anforderung ein.</span><span class="sxs-lookup"><span data-stu-id="950a3-461">The registration scopes the service lifetime to the lifetime of a single request.</span></span> <span data-ttu-id="950a3-462">Auf die [Dienstlebensdauer](#service-lifetimes) wird später in diesem Artikel eingegangen.</span><span class="sxs-lookup"><span data-stu-id="950a3-462">[Service lifetimes](#service-lifetimes) are described later in this topic.</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=5)]

> [!NOTE]
> <span data-ttu-id="950a3-463">Jede Erweiterungsmethode vom Typ `services.Add{SERVICE_NAME}` fügt Dienste hinzu (und konfiguriert diese möglicherweise).</span><span class="sxs-lookup"><span data-stu-id="950a3-463">Each `services.Add{SERVICE_NAME}` extension method adds (and potentially configures) services.</span></span> <span data-ttu-id="950a3-464">So fügt beispielsweise `services.AddMvc()` die erforderlichen Dienste Razor Pages und MVC hinzu.</span><span class="sxs-lookup"><span data-stu-id="950a3-464">For example, `services.AddMvc()` adds the services Razor Pages and MVC require.</span></span> <span data-ttu-id="950a3-465">Es wird empfohlen, dass Apps dieser Konvention folgen.</span><span class="sxs-lookup"><span data-stu-id="950a3-465">We recommended that apps follow this convention.</span></span> <span data-ttu-id="950a3-466">Platzieren Sie Erweiterungsmethoden im Namespace [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection), um Gruppen von Dienstregistrierungen zu kapseln.</span><span class="sxs-lookup"><span data-stu-id="950a3-466">Place extension methods in the [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) namespace to encapsulate groups of service registrations.</span></span>

<span data-ttu-id="950a3-467">Wenn für den Dienstkonstruktor ein [integrierter Typ](/dotnet/csharp/language-reference/keywords/built-in-types-table) erforderlich ist, z. B. `string`, kann dieser Typ über [Konfiguration](xref:fundamentals/configuration/index) oder das [Optionsmuster](xref:fundamentals/configuration/options) eingefügt werden:</span><span class="sxs-lookup"><span data-stu-id="950a3-467">If the service's constructor requires a [built in type](/dotnet/csharp/language-reference/keywords/built-in-types-table), such as a `string`, the type can be injected by using [configuration](xref:fundamentals/configuration/index) or the [options pattern](xref:fundamentals/configuration/options):</span></span>

```csharp
public class MyDependency : IMyDependency
{
    public MyDependency(IConfiguration config)
    {
        var myStringValue = config["MyStringKey"];

        // Use myStringValue
    }

    ...
}
```

<span data-ttu-id="950a3-468">Eine Instanz des Diensts wird über den Konstruktor einer Klasse angefordert, in der der Dienst verwendet wird, und einem privaten Feld zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="950a3-468">An instance of the service is requested via the constructor of a class where the service is used and assigned to a private field.</span></span> <span data-ttu-id="950a3-469">Das Feld wird verwendet, um auf den Dienst bei Bedarf über die gesamte Klasse zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="950a3-469">The field is used to access the service as necessary throughout the class.</span></span>

<span data-ttu-id="950a3-470">In der Beispiel-App wird die Instanz `IMyDependency` angefordert, und mit ihr wird die App-Methode `WriteMessage` aufgerufen:</span><span class="sxs-lookup"><span data-stu-id="950a3-470">In the sample app, the `IMyDependency` instance is requested and used to call the service's `WriteMessage` method:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3,6,13,29-30)]

## <a name="services-injected-into-startup"></a><span data-ttu-id="950a3-471">In den Start eingefügte Dienste</span><span class="sxs-lookup"><span data-stu-id="950a3-471">Services injected into Startup</span></span>

<span data-ttu-id="950a3-472">Bei Verwendung des generischen Hosts (<xref:Microsoft.Extensions.Hosting.IHostBuilder>) können nur die folgenden Diensttypen in den `Startup`-Konstruktor eingefügt werden:</span><span class="sxs-lookup"><span data-stu-id="950a3-472">Only the following service types can be injected into the `Startup` constructor when using the Generic Host (<xref:Microsoft.Extensions.Hosting.IHostBuilder>):</span></span>

* `IWebHostEnvironment`
* <xref:Microsoft.Extensions.Hosting.IHostEnvironment>
* <xref:Microsoft.Extensions.Configuration.IConfiguration>

<span data-ttu-id="950a3-473">Dienste können in `Startup.Configure` eingefügt werden:</span><span class="sxs-lookup"><span data-stu-id="950a3-473">Services can be injected into `Startup.Configure`:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptions<MyOptions> options)
{
    ...
}
```

<span data-ttu-id="950a3-474">Weitere Informationen finden Sie unter <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="950a3-474">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="framework-provided-services"></a><span data-ttu-id="950a3-475">Von Frameworks bereitgestellte Dienste</span><span class="sxs-lookup"><span data-stu-id="950a3-475">Framework-provided services</span></span>

<span data-ttu-id="950a3-476">Die Methode `Startup.ConfigureServices` definiert die von der App verwendeten Dienste. Dies umfasst auch Plattformfeatures wie Entity Framework Core und ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="950a3-476">The `Startup.ConfigureServices` method is responsible for defining the services that the app uses, including platform features, such as Entity Framework Core and ASP.NET Core MVC.</span></span> <span data-ttu-id="950a3-477">Zunächst enthält die in `ConfigureServices` bereitgestellte `IServiceCollection` die vom Framework definierten Dienste, abhängig davon, wie der [Host konfiguriert](xref:fundamentals/index#host) wurde.</span><span class="sxs-lookup"><span data-stu-id="950a3-477">Initially, the `IServiceCollection` provided to `ConfigureServices` has services defined by the framework depending on [how the host was configured](xref:fundamentals/index#host).</span></span> <span data-ttu-id="950a3-478">Es ist nicht ungewöhnlich, dass das Framework Hunderte von Diensten für eine auf einer ASP.NET Core-Vorlage basierende App registriert.</span><span class="sxs-lookup"><span data-stu-id="950a3-478">It's not uncommon for an app based on an ASP.NET Core template to have hundreds of services registered by the framework.</span></span> <span data-ttu-id="950a3-479">In der folgenden Tabelle finden Sie eine kleine Auswahl der Dienste, die vom Framework registriert werden können.</span><span class="sxs-lookup"><span data-stu-id="950a3-479">A small sample of framework-registered services is listed in the following table.</span></span>

| <span data-ttu-id="950a3-480">Diensttyp</span><span class="sxs-lookup"><span data-stu-id="950a3-480">Service Type</span></span> | <span data-ttu-id="950a3-481">Lebensdauer</span><span class="sxs-lookup"><span data-stu-id="950a3-481">Lifetime</span></span> |
| ------------ | -------- |
| <xref:Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory?displayProperty=fullName> | <span data-ttu-id="950a3-482">Transient (vorübergehend)</span><span class="sxs-lookup"><span data-stu-id="950a3-482">Transient</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime?displayProperty=fullName> | <span data-ttu-id="950a3-483">Singleton</span><span class="sxs-lookup"><span data-stu-id="950a3-483">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment?displayProperty=fullName> | <span data-ttu-id="950a3-484">Singleton</span><span class="sxs-lookup"><span data-stu-id="950a3-484">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IStartup?displayProperty=fullName> | <span data-ttu-id="950a3-485">Singleton</span><span class="sxs-lookup"><span data-stu-id="950a3-485">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IStartupFilter?displayProperty=fullName> | <span data-ttu-id="950a3-486">Transient (vorübergehend)</span><span class="sxs-lookup"><span data-stu-id="950a3-486">Transient</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.Server.IServer?displayProperty=fullName> | <span data-ttu-id="950a3-487">Singleton</span><span class="sxs-lookup"><span data-stu-id="950a3-487">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Http.IHttpContextFactory?displayProperty=fullName> | <span data-ttu-id="950a3-488">Transient (vorübergehend)</span><span class="sxs-lookup"><span data-stu-id="950a3-488">Transient</span></span> |
| <xref:Microsoft.Extensions.Logging.ILogger`1?displayProperty=fullName> | <span data-ttu-id="950a3-489">Singleton</span><span class="sxs-lookup"><span data-stu-id="950a3-489">Singleton</span></span> |
| <xref:Microsoft.Extensions.Logging.ILoggerFactory?displayProperty=fullName> | <span data-ttu-id="950a3-490">Singleton</span><span class="sxs-lookup"><span data-stu-id="950a3-490">Singleton</span></span> |
| <xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider?displayProperty=fullName> | <span data-ttu-id="950a3-491">Singleton</span><span class="sxs-lookup"><span data-stu-id="950a3-491">Singleton</span></span> |
| <xref:Microsoft.Extensions.Options.IConfigureOptions`1?displayProperty=fullName> | <span data-ttu-id="950a3-492">Transient (vorübergehend)</span><span class="sxs-lookup"><span data-stu-id="950a3-492">Transient</span></span> |
| <xref:Microsoft.Extensions.Options.IOptions`1?displayProperty=fullName> | <span data-ttu-id="950a3-493">Singleton</span><span class="sxs-lookup"><span data-stu-id="950a3-493">Singleton</span></span> |
| <xref:System.Diagnostics.DiagnosticSource?displayProperty=fullName> | <span data-ttu-id="950a3-494">Singleton</span><span class="sxs-lookup"><span data-stu-id="950a3-494">Singleton</span></span> |
| <xref:System.Diagnostics.DiagnosticListener?displayProperty=fullName> | <span data-ttu-id="950a3-495">Singleton</span><span class="sxs-lookup"><span data-stu-id="950a3-495">Singleton</span></span> |

## <a name="register-additional-services-with-extension-methods"></a><span data-ttu-id="950a3-496">Registrieren weiterer Dienste mit Erweiterungsmethoden</span><span class="sxs-lookup"><span data-stu-id="950a3-496">Register additional services with extension methods</span></span>

<span data-ttu-id="950a3-497">Wenn eine Dienstsammlungs-Erweiterungsmethode verfügbar ist, um einen Dienst (und ggf. seine abhängigen Dienste) zu registrieren, ist die Konvention, eine einzige `Add{SERVICE_NAME}`-Erweiterungsmethode zu verwenden, um alle von diesem Dienst benötigten Dienste zu registrieren.</span><span class="sxs-lookup"><span data-stu-id="950a3-497">When a service collection extension method is available to register a service (and its dependent services, if required), the convention is to use a single `Add{SERVICE_NAME}` extension method to register all of the services required by that service.</span></span> <span data-ttu-id="950a3-498">Der folgende Code ist ein Beispiel dafür, wie mit den Erweiterungsmethoden [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) und <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentityCore*> zusätzliche Dienste zum Container hinzugefügt werden:</span><span class="sxs-lookup"><span data-stu-id="950a3-498">The following code is an example of how to add additional services to the container using the extension methods [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) and <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentityCore*>:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    ...

    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));

    services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

    ...
}
```

<span data-ttu-id="950a3-499">Weitere Informationen finden Sie in der <xref:Microsoft.Extensions.DependencyInjection.ServiceCollection>-Klasse in der API-Dokumentation.</span><span class="sxs-lookup"><span data-stu-id="950a3-499">For more information, see the <xref:Microsoft.Extensions.DependencyInjection.ServiceCollection> class in the API documentation.</span></span>

## <a name="service-lifetimes"></a><span data-ttu-id="950a3-500">Dienstlebensdauer</span><span class="sxs-lookup"><span data-stu-id="950a3-500">Service lifetimes</span></span>

<span data-ttu-id="950a3-501">Wählen Sie eine geeignete Lebensdauer für jeden registrierten Dienst aus.</span><span class="sxs-lookup"><span data-stu-id="950a3-501">Choose an appropriate lifetime for each registered service.</span></span> <span data-ttu-id="950a3-502">ASP.NET Core-Dienste können mit folgender Lebensdauer konfiguriert werden:</span><span class="sxs-lookup"><span data-stu-id="950a3-502">ASP.NET Core services can be configured with the following lifetimes:</span></span>

### <a name="transient"></a><span data-ttu-id="950a3-503">Transient (vorübergehend)</span><span class="sxs-lookup"><span data-stu-id="950a3-503">Transient</span></span>

<span data-ttu-id="950a3-504">Kurzlebige Dienste (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddTransient*>) werden bei jeder Anforderung aus dem Dienstcontainer neu erstellt.</span><span class="sxs-lookup"><span data-stu-id="950a3-504">Transient lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddTransient*>) are created each time they're requested from the service container.</span></span> <span data-ttu-id="950a3-505">Diese Lebensdauer ist am besten für einfache, zustandslose Dienste geeignet.</span><span class="sxs-lookup"><span data-stu-id="950a3-505">This lifetime works best for lightweight, stateless services.</span></span>

<span data-ttu-id="950a3-506">In Apps, die Anforderungen verarbeiten, werden vorübergehende Dienste am Ende der Anforderung verworfen.</span><span class="sxs-lookup"><span data-stu-id="950a3-506">In apps that process requests, transient services are disposed at the end of the request.</span></span>

### <a name="scoped"></a><span data-ttu-id="950a3-507">Bereichsbezogen</span><span class="sxs-lookup"><span data-stu-id="950a3-507">Scoped</span></span>

<span data-ttu-id="950a3-508">Dienste mit bereichsbezogener Lebensdauer (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped*>) werden einmal pro Clientanforderung (-verbindung) erstellt.</span><span class="sxs-lookup"><span data-stu-id="950a3-508">Scoped lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped*>) are created once per client request (connection).</span></span>

<span data-ttu-id="950a3-509">In Apps, die Anforderungen verarbeiten, werden bereichsbezogene Dienste am Ende der Anforderung verworfen.</span><span class="sxs-lookup"><span data-stu-id="950a3-509">In apps that process requests, scoped services are disposed at the end of the request.</span></span>

> [!WARNING]
> <span data-ttu-id="950a3-510">Wenn Sie einen bereichsbezogenen Dienst in einer Middleware verwenden, müssen Sie den Dienst in die `Invoke`- oder `InvokeAsync`-Methode einfügen.</span><span class="sxs-lookup"><span data-stu-id="950a3-510">When using a scoped service in a middleware, inject the service into the `Invoke` or `InvokeAsync` method.</span></span> <span data-ttu-id="950a3-511">Fügen Sie ihn nicht über [Konstruktorinjektion](xref:mvc/controllers/dependency-injection#constructor-injection) ein, da hierdurch der Dienst ein Verhalten wie ein Singleton zeigt.</span><span class="sxs-lookup"><span data-stu-id="950a3-511">Don't inject via [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) because it forces the service to behave like a singleton.</span></span> <span data-ttu-id="950a3-512">Weitere Informationen finden Sie unter <xref:fundamentals/middleware/write#per-request-middleware-dependencies>.</span><span class="sxs-lookup"><span data-stu-id="950a3-512">For more information, see <xref:fundamentals/middleware/write#per-request-middleware-dependencies>.</span></span>

### <a name="singleton"></a><span data-ttu-id="950a3-513">Singleton</span><span class="sxs-lookup"><span data-stu-id="950a3-513">Singleton</span></span>

<span data-ttu-id="950a3-514">Dienste mit Singleton-Lebensdauer (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*>) werden bei der ersten Anforderung erstellt (bzw. wenn `Startup.ConfigureServices` ausgeführt wird, und eine Instanz bei der Dienstregistrierung angegeben wird).</span><span class="sxs-lookup"><span data-stu-id="950a3-514">Singleton lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*>) are created the first time they're requested (or when `Startup.ConfigureServices` is run and an instance is specified with the service registration).</span></span> <span data-ttu-id="950a3-515">Jeder nachfolgenden Anforderung verwendet die gleiche Instanz.</span><span class="sxs-lookup"><span data-stu-id="950a3-515">Every subsequent request uses the same instance.</span></span> <span data-ttu-id="950a3-516">Wenn die App ein Verhalten vom Typ „Singleton“ erfordert, wird empfohlen, den Dienstcontainer die Dienstlebensdauer verwalten zu lassen.</span><span class="sxs-lookup"><span data-stu-id="950a3-516">If the app requires singleton behavior, allowing the service container to manage the service's lifetime is recommended.</span></span> <span data-ttu-id="950a3-517">Implementieren Sie nicht das Designmuster „Singleton“ und stellen Sie Benutzercode bereit, um die Objektlebensdauer in der Klasse zu verwalten.</span><span class="sxs-lookup"><span data-stu-id="950a3-517">Don't implement the singleton design pattern and provide user code to manage the object's lifetime in the class.</span></span>

<span data-ttu-id="950a3-518">In Apps, die Anforderungen verarbeiten, werden Singleton-Dienste verworfen, wenn der <xref:Microsoft.Extensions.DependencyInjection.ServiceProvider> beim Herunterfahren der App verworfen wird.</span><span class="sxs-lookup"><span data-stu-id="950a3-518">In apps that process requests, singleton services are disposed when the <xref:Microsoft.Extensions.DependencyInjection.ServiceProvider> is disposed at app shutdown.</span></span>

> [!WARNING]
> <span data-ttu-id="950a3-519">Es ist riskant, einen bereichsbezogenen Dienst über ein Singleton aufzulösen.</span><span class="sxs-lookup"><span data-stu-id="950a3-519">It's dangerous to resolve a scoped service from a singleton.</span></span> <span data-ttu-id="950a3-520">Möglicherweise weist der Dienst bei der Verarbeitung nachfolgender Anforderungen einen falschen Status auf.</span><span class="sxs-lookup"><span data-stu-id="950a3-520">It may cause the service to have incorrect state when processing subsequent requests.</span></span>

## <a name="service-registration-methods"></a><span data-ttu-id="950a3-521">Dienstregistrierungsmethoden</span><span class="sxs-lookup"><span data-stu-id="950a3-521">Service registration methods</span></span>

<span data-ttu-id="950a3-522">Methoden zur Erweiterung der Dienstregistrierung bieten Überladungen, die in bestimmten Szenarios hilfreich sind.</span><span class="sxs-lookup"><span data-stu-id="950a3-522">Service registration extension methods offer overloads that are useful in specific scenarios.</span></span>

| <span data-ttu-id="950a3-523">Methode</span><span class="sxs-lookup"><span data-stu-id="950a3-523">Method</span></span> | <span data-ttu-id="950a3-524">Automatische</span><span class="sxs-lookup"><span data-stu-id="950a3-524">Automatic</span></span><br><span data-ttu-id="950a3-525">Objekt</span><span class="sxs-lookup"><span data-stu-id="950a3-525">object</span></span><br><span data-ttu-id="950a3-526">bereinigung</span><span class="sxs-lookup"><span data-stu-id="950a3-526">disposal</span></span> | <span data-ttu-id="950a3-527">Mehrere</span><span class="sxs-lookup"><span data-stu-id="950a3-527">Multiple</span></span><br><span data-ttu-id="950a3-528">Implementierungen</span><span class="sxs-lookup"><span data-stu-id="950a3-528">implementations</span></span> | <span data-ttu-id="950a3-529">Argumentübergabe</span><span class="sxs-lookup"><span data-stu-id="950a3-529">Pass args</span></span> |
| ------ | :-----------------------------: | :-------------------------: | :-------: |
| `Add{LIFETIME}<{SERVICE}, {IMPLEMENTATION}>()`<br><span data-ttu-id="950a3-530">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="950a3-530">Example:</span></span><br>`services.AddSingleton<IMyDep, MyDep>();` | <span data-ttu-id="950a3-531">Ja</span><span class="sxs-lookup"><span data-stu-id="950a3-531">Yes</span></span> | <span data-ttu-id="950a3-532">Ja</span><span class="sxs-lookup"><span data-stu-id="950a3-532">Yes</span></span> | <span data-ttu-id="950a3-533">Nein</span><span class="sxs-lookup"><span data-stu-id="950a3-533">No</span></span> |
| `Add{LIFETIME}<{SERVICE}>(sp => new {IMPLEMENTATION})`<br><span data-ttu-id="950a3-534">Beispiele:</span><span class="sxs-lookup"><span data-stu-id="950a3-534">Examples:</span></span><br>`services.AddSingleton<IMyDep>(sp => new MyDep());`<br>`services.AddSingleton<IMyDep>(sp => new MyDep("A string!"));` | <span data-ttu-id="950a3-535">Ja</span><span class="sxs-lookup"><span data-stu-id="950a3-535">Yes</span></span> | <span data-ttu-id="950a3-536">Ja</span><span class="sxs-lookup"><span data-stu-id="950a3-536">Yes</span></span> | <span data-ttu-id="950a3-537">Ja</span><span class="sxs-lookup"><span data-stu-id="950a3-537">Yes</span></span> |
| `Add{LIFETIME}<{IMPLEMENTATION}>()`<br><span data-ttu-id="950a3-538">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="950a3-538">Example:</span></span><br>`services.AddSingleton<MyDep>();` | <span data-ttu-id="950a3-539">Ja</span><span class="sxs-lookup"><span data-stu-id="950a3-539">Yes</span></span> | <span data-ttu-id="950a3-540">Nein</span><span class="sxs-lookup"><span data-stu-id="950a3-540">No</span></span> | <span data-ttu-id="950a3-541">Nein</span><span class="sxs-lookup"><span data-stu-id="950a3-541">No</span></span> |
| `AddSingleton<{SERVICE}>(new {IMPLEMENTATION})`<br><span data-ttu-id="950a3-542">Beispiele:</span><span class="sxs-lookup"><span data-stu-id="950a3-542">Examples:</span></span><br>`services.AddSingleton<IMyDep>(new MyDep());`<br>`services.AddSingleton<IMyDep>(new MyDep("A string!"));` | <span data-ttu-id="950a3-543">Nein</span><span class="sxs-lookup"><span data-stu-id="950a3-543">No</span></span> | <span data-ttu-id="950a3-544">Ja</span><span class="sxs-lookup"><span data-stu-id="950a3-544">Yes</span></span> | <span data-ttu-id="950a3-545">Ja</span><span class="sxs-lookup"><span data-stu-id="950a3-545">Yes</span></span> |
| `AddSingleton(new {IMPLEMENTATION})`<br><span data-ttu-id="950a3-546">Beispiele:</span><span class="sxs-lookup"><span data-stu-id="950a3-546">Examples:</span></span><br>`services.AddSingleton(new MyDep());`<br>`services.AddSingleton(new MyDep("A string!"));` | <span data-ttu-id="950a3-547">Nein</span><span class="sxs-lookup"><span data-stu-id="950a3-547">No</span></span> | <span data-ttu-id="950a3-548">Nein</span><span class="sxs-lookup"><span data-stu-id="950a3-548">No</span></span> | <span data-ttu-id="950a3-549">Ja</span><span class="sxs-lookup"><span data-stu-id="950a3-549">Yes</span></span> |

<span data-ttu-id="950a3-550">Weitere Informationen zum Löschen von Typen finden Sie im Abschnitt [Löschen von Diensten](#disposal-of-services).</span><span class="sxs-lookup"><span data-stu-id="950a3-550">For more information on type disposal, see the [Disposal of services](#disposal-of-services) section.</span></span> <span data-ttu-id="950a3-551">Ein häufiges Szenario für mehrere Implementierungen ist [Typen zu Testzwecken simulieren](xref:test/integration-tests#inject-mock-services).</span><span class="sxs-lookup"><span data-stu-id="950a3-551">A common scenario for multiple implementations is [mocking types for testing](xref:test/integration-tests#inject-mock-services).</span></span>

<span data-ttu-id="950a3-552">`TryAdd{LIFETIME}`-Methoden registrieren den Dienst nur, wenn noch keine Implementierung registriert ist.</span><span class="sxs-lookup"><span data-stu-id="950a3-552">`TryAdd{LIFETIME}` methods only register the service if there isn't already an implementation registered.</span></span>

<span data-ttu-id="950a3-553">Im folgenden Beispiel registriert die erste Zeile `MyDependency` für `IMyDependency`.</span><span class="sxs-lookup"><span data-stu-id="950a3-553">In the following example, the first line registers `MyDependency` for `IMyDependency`.</span></span> <span data-ttu-id="950a3-554">Die zweite Zeile hat keine Auswirkungen, da es für `IMyDependency` bereits eine registrierte Implementierung gibt:</span><span class="sxs-lookup"><span data-stu-id="950a3-554">The second line has no effect because `IMyDependency` already has a registered implementation:</span></span>

```csharp
services.AddSingleton<IMyDependency, MyDependency>();
// The following line has no effect:
services.TryAddSingleton<IMyDependency, DifferentDependency>();
```

<span data-ttu-id="950a3-555">Weitere Informationen finden Sie unter:</span><span class="sxs-lookup"><span data-stu-id="950a3-555">For more information, see:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAdd*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddTransient*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddScoped*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddSingleton*>

<span data-ttu-id="950a3-556">[TryAddEnumerable(ServiceDescriptor)](xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddEnumerable*)-Methoden registrieren den Dienst nur, wenn es nicht bereits eine Implementierung *des gleichen Typs* gibt.</span><span class="sxs-lookup"><span data-stu-id="950a3-556">[TryAddEnumerable(ServiceDescriptor)](xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddEnumerable*) methods only register the service if there isn't already an implementation *of the same type*.</span></span> <span data-ttu-id="950a3-557">Mehrere Dienste werden über `IEnumerable<{SERVICE}>` aufgelöst.</span><span class="sxs-lookup"><span data-stu-id="950a3-557">Multiple services are resolved via `IEnumerable<{SERVICE}>`.</span></span> <span data-ttu-id="950a3-558">Beim Registrieren von Diensten möchte der Entwickler nur eine Instanz hinzufügen, wenn nicht bereits eine Instanz vom gleichen Typ hinzugefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="950a3-558">When registering services, the developer only wants to add an instance if one of the same type hasn't already been added.</span></span> <span data-ttu-id="950a3-559">Diese Methode wird in der Regel von Bibliotheksautoren verwendet, um zu vermeiden, dass zwei Kopien einer Instanz im Container registriert werden.</span><span class="sxs-lookup"><span data-stu-id="950a3-559">Generally, this method is used by library authors to avoid registering two copies of an instance in the container.</span></span>

<span data-ttu-id="950a3-560">Im folgenden Beispiel registriert die erste Zeile `MyDep` für `IMyDep1`.</span><span class="sxs-lookup"><span data-stu-id="950a3-560">In the following example, the first line registers `MyDep` for `IMyDep1`.</span></span> <span data-ttu-id="950a3-561">Die zweite Zeile registriert `MyDep` für `IMyDep2`.</span><span class="sxs-lookup"><span data-stu-id="950a3-561">The second line registers `MyDep` for `IMyDep2`.</span></span> <span data-ttu-id="950a3-562">Die dritte Zeile hat keine Auswirkungen, da es für `IMyDep1` bereits eine registrierte Implementierung von `MyDep` gibt:</span><span class="sxs-lookup"><span data-stu-id="950a3-562">The third line has no effect because `IMyDep1` already has a registered implementation of `MyDep`:</span></span>

```csharp
public interface IMyDep1 {}
public interface IMyDep2 {}

public class MyDep : IMyDep1, IMyDep2 {}

services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep1, MyDep>());
services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep2, MyDep>());
// Two registrations of MyDep for IMyDep1 is avoided by the following line:
services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep1, MyDep>());
```

### <a name="constructor-injection-behavior"></a><span data-ttu-id="950a3-563">Verhalten von Constructor Injection</span><span class="sxs-lookup"><span data-stu-id="950a3-563">Constructor injection behavior</span></span>

<span data-ttu-id="950a3-564">Dienste können durch zwei Mechanismen aufgelöst werden:</span><span class="sxs-lookup"><span data-stu-id="950a3-564">Services can be resolved by two mechanisms:</span></span>

* <xref:System.IServiceProvider>
* <span data-ttu-id="950a3-565"><xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities>: Lässt die Erstellung von Objekten ohne Dienstregistrierung im Dependency-Injection-Container zu</span><span class="sxs-lookup"><span data-stu-id="950a3-565"><xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities>: Permits object creation without service registration in the dependency injection container.</span></span> <span data-ttu-id="950a3-566">`ActivatorUtilities` wird mit Abstraktionen für Benutzer verwendet. Dazu zählen Taghilfsprogramme, MVC-Controller und Modellbindungen.</span><span class="sxs-lookup"><span data-stu-id="950a3-566">`ActivatorUtilities` is used with user-facing abstractions, such as Tag Helpers, MVC controllers, and model binders.</span></span>

<span data-ttu-id="950a3-567">Konstruktoren können Argumente akzeptieren, die nicht durch Abhängigkeitsinjektion bereitgestellt werden. Die Argumente müssen jedoch Standardwerte zuweisen.</span><span class="sxs-lookup"><span data-stu-id="950a3-567">Constructors can accept arguments that aren't provided by dependency injection, but the arguments must assign default values.</span></span>

<span data-ttu-id="950a3-568">Wenn Dienste durch `IServiceProvider` oder `ActivatorUtilities` aufgelöst werden, benötigt die [Konstruktorinjektion](xref:mvc/controllers/dependency-injection#constructor-injection) einen *öffentlichen* Konstruktor.</span><span class="sxs-lookup"><span data-stu-id="950a3-568">When services are resolved by `IServiceProvider` or `ActivatorUtilities`, [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) requires a *public* constructor.</span></span>

<span data-ttu-id="950a3-569">Wenn Dienste durch `ActivatorUtilities` aufgelöst werden, erfordert die [Konstruktorinjektion](xref:mvc/controllers/dependency-injection#constructor-injection), dass nur ein anwendbarer Konstruktor vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="950a3-569">When services are resolved by `ActivatorUtilities`, [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) requires that only one applicable constructor exists.</span></span> <span data-ttu-id="950a3-570">Konstruktorüberladungen werden unterstützt. Es darf jedoch nur eine Überladung vorhanden sein, deren Argumente alle durch Dependency Injection erfüllt werden können.</span><span class="sxs-lookup"><span data-stu-id="950a3-570">Constructor overloads are supported, but only one overload can exist whose arguments can all be fulfilled by dependency injection.</span></span>

## <a name="entity-framework-contexts"></a><span data-ttu-id="950a3-571">Entity Framework-Kontexte</span><span class="sxs-lookup"><span data-stu-id="950a3-571">Entity Framework contexts</span></span>

<span data-ttu-id="950a3-572">Entity Framework-Kontexte werden einem Dienstcontainer normalerweise mithilfe der [bereichsbezogenen Lebensdauer](#service-lifetimes) hinzugefügt, da Datenbankvorgänge von Web-Apps normalerweise auf den Clientanforderungsbereich bezogen werden.</span><span class="sxs-lookup"><span data-stu-id="950a3-572">Entity Framework contexts are usually added to the service container using the [scoped lifetime](#service-lifetimes) because web app database operations are normally scoped to the client request.</span></span> <span data-ttu-id="950a3-573">Der Bereich einer Standardlebensdauer wird festgelegt, wenn eine Lebensdauer nicht von einer [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext)-Überladung angegeben wird, wenn der Datenbankkontext registriert wird.</span><span class="sxs-lookup"><span data-stu-id="950a3-573">The default lifetime is scoped if a lifetime isn't specified by an [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) overload when registering the database context.</span></span> <span data-ttu-id="950a3-574">Dienste einer festgelegten Lebensdauer sollten keinen Datenbankkontext mit einer kürzeren Lebensdauer als die des Dienstes verwenden.</span><span class="sxs-lookup"><span data-stu-id="950a3-574">Services of a given lifetime shouldn't use a database context with a shorter lifetime than the service.</span></span>

## <a name="lifetime-and-registration-options"></a><span data-ttu-id="950a3-575">Lebensdauer und Registrierungsoptionen</span><span class="sxs-lookup"><span data-stu-id="950a3-575">Lifetime and registration options</span></span>

<span data-ttu-id="950a3-576">Um den Unterschied zwischen der Lebensdauer und den Registrierungsoptionen zu demonstrieren, betrachten Sie die folgenden Schnittstellen, die Aufgaben als Vorgänge mit einem eindeutigen Bezeichner (`OperationId`) darstellen.</span><span class="sxs-lookup"><span data-stu-id="950a3-576">To demonstrate the difference between the lifetime and registration options, consider the following interfaces that represent tasks as an operation with a unique identifier, `OperationId`.</span></span> <span data-ttu-id="950a3-577">Je nachdem, wie die Dienstlebensdauer für die folgenden Schnittstellen konfiguriert ist, stellt der Container auf Anforderung einer Klasse entweder die gleiche oder eine andere Instanz des Diensts zur Verfügung:</span><span class="sxs-lookup"><span data-stu-id="950a3-577">Depending on how the lifetime of an operations service is configured for the following interfaces, the container provides either the same or a different instance of the service when requested by a class:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

<span data-ttu-id="950a3-578">Die Schnittstellen sind in die Klasse `Operation` implementiert.</span><span class="sxs-lookup"><span data-stu-id="950a3-578">The interfaces are implemented in the `Operation` class.</span></span> <span data-ttu-id="950a3-579">Der `Operation`-Konstruktor generiert eine GUID, wenn noch keine angegeben ist:</span><span class="sxs-lookup"><span data-stu-id="950a3-579">The `Operation` constructor generates a GUID if one isn't supplied:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

<span data-ttu-id="950a3-580">`OperationService` ist registriert und hängt von jedem anderen `Operation`-Typ ab.</span><span class="sxs-lookup"><span data-stu-id="950a3-580">An `OperationService` is registered that depends on each of the other `Operation` types.</span></span> <span data-ttu-id="950a3-581">Wenn `OperationService` über die Abhängigkeitsinjektion angefordert wird, wird entweder eine neue Instanz jedes Diensts oder eine vorhandene Instanz basierend auf der Lebensdauer des abhängigen Diensts zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="950a3-581">When `OperationService` is requested via dependency injection, it receives either a new instance of each service or an existing instance based on the lifetime of the dependent service.</span></span>

* <span data-ttu-id="950a3-582">Wenn vorübergehende Dienste bei der Anforderung aus dem Container erstellt werden, ist die `OperationId` von Dienst `IOperationTransient` anders als die `OperationId` von `OperationService`.</span><span class="sxs-lookup"><span data-stu-id="950a3-582">When transient services are created when requested from the container, the `OperationId` of the `IOperationTransient` service is different than the `OperationId` of the `OperationService`.</span></span> <span data-ttu-id="950a3-583">`OperationService` erhält eine neue Instanz der Klasse `IOperationTransient`.</span><span class="sxs-lookup"><span data-stu-id="950a3-583">`OperationService` receives a new instance of the `IOperationTransient` class.</span></span> <span data-ttu-id="950a3-584">Der `OperationId`-Wert der neuen Instanz ist anders.</span><span class="sxs-lookup"><span data-stu-id="950a3-584">The new instance yields a different `OperationId`.</span></span>
* <span data-ttu-id="950a3-585">Wenn bereichsbezogene Dienste pro Clientanforderung erstellt werden, ist die `OperationId` in Dienst `IOperationScoped` und `OperationService` identisch innerhalb einer Clientanforderung.</span><span class="sxs-lookup"><span data-stu-id="950a3-585">When scoped services are created per client request, the `OperationId` of the `IOperationScoped` service is the same as that of `OperationService` within a client request.</span></span> <span data-ttu-id="950a3-586">Clientanforderungsübergreifend haben die beiden Dienste jedoch einen anderen gemeinsamen `OperationId`-Wert.</span><span class="sxs-lookup"><span data-stu-id="950a3-586">Across client requests, both services share a different `OperationId` value.</span></span>
* <span data-ttu-id="950a3-587">Wenn Singleton- und Singletoninstanzdienste einmal erstellt und für alle Clientanforderungen und alle Dienste verwendet werden, ist `OperationId` für alle Dienstanforderungen identisch.</span><span class="sxs-lookup"><span data-stu-id="950a3-587">When singleton and singleton-instance services are created once and used across all client requests and all services, the `OperationId` is constant across all service requests.</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

<span data-ttu-id="950a3-588">In `Startup.ConfigureServices` wird jeder Typ entsprechend seiner benannten Lebensdauer dem Container hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="950a3-588">In `Startup.ConfigureServices`, each type is added to the container according to its named lifetime:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=6-9,12)]

<span data-ttu-id="950a3-589">Der Dienst `IOperationSingletonInstance` verwendet eine bestimmte Instanz mit einer bekannten ID von `Guid.Empty`.</span><span class="sxs-lookup"><span data-stu-id="950a3-589">The `IOperationSingletonInstance` service is using a specific instance with a known ID of `Guid.Empty`.</span></span> <span data-ttu-id="950a3-590">Die Verwendung dieses Typs ist eindeutig (die GUID besteht ausschließlich aus Nullen (0)).</span><span class="sxs-lookup"><span data-stu-id="950a3-590">It's clear when this type is in use (its GUID is all zeroes).</span></span>

<span data-ttu-id="950a3-591">Die Beispiel-App zeigt die Objektlebensdauer innerhalb einer Anforderung und zwischen einzelnen Anforderungen an.</span><span class="sxs-lookup"><span data-stu-id="950a3-591">The sample app demonstrates object lifetimes within and between individual requests.</span></span> <span data-ttu-id="950a3-592">Ihre Klasse `IndexModel` fordert jeden `IOperation`- und `OperationService`-Typ an.</span><span class="sxs-lookup"><span data-stu-id="950a3-592">The sample app's `IndexModel` requests each kind of `IOperation` type and the `OperationService`.</span></span> <span data-ttu-id="950a3-593">Die Seite zeigt dann alle `OperationId`-Werte der Seitenmodellklasse und des Diensts über Eigenschaftenzuweisungen an:</span><span class="sxs-lookup"><span data-stu-id="950a3-593">The page then displays all of the page model class's and service's `OperationId` values through property assignments:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=7-11,14-18,21-25)]

<span data-ttu-id="950a3-594">Zwei folgende Ausgaben zeigen die Ergebnisse von zwei Anforderungen:</span><span class="sxs-lookup"><span data-stu-id="950a3-594">Two following output shows the results of two requests:</span></span>

<span data-ttu-id="950a3-595">**Erste Anforderung:**</span><span class="sxs-lookup"><span data-stu-id="950a3-595">**First request:**</span></span>

<span data-ttu-id="950a3-596">Controllervorgänge:</span><span class="sxs-lookup"><span data-stu-id="950a3-596">Controller operations:</span></span>

<span data-ttu-id="950a3-597">Vorübergehend: d233e165-f417-469b-a866-1cf1935d2518</span><span class="sxs-lookup"><span data-stu-id="950a3-597">Transient: d233e165-f417-469b-a866-1cf1935d2518</span></span>  
<span data-ttu-id="950a3-598">Bereichsbezogen: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="950a3-598">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="950a3-599">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="950a3-599">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="950a3-600">Instanz: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="950a3-600">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="950a3-601">`OperationService`-Vorgänge:</span><span class="sxs-lookup"><span data-stu-id="950a3-601">`OperationService` operations:</span></span>

<span data-ttu-id="950a3-602">Vorübergehend: c6b049eb-1318-4e31-90f1-eb2dd849ff64</span><span class="sxs-lookup"><span data-stu-id="950a3-602">Transient: c6b049eb-1318-4e31-90f1-eb2dd849ff64</span></span>  
<span data-ttu-id="950a3-603">Bereichsbezogen: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="950a3-603">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="950a3-604">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="950a3-604">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="950a3-605">Instanz: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="950a3-605">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="950a3-606">**Zweite Anforderung:**</span><span class="sxs-lookup"><span data-stu-id="950a3-606">**Second request:**</span></span>

<span data-ttu-id="950a3-607">Controllervorgänge:</span><span class="sxs-lookup"><span data-stu-id="950a3-607">Controller operations:</span></span>

<span data-ttu-id="950a3-608">Vorübergehend: b63bd538-0a37-4ff1-90ba-081c5138dda0</span><span class="sxs-lookup"><span data-stu-id="950a3-608">Transient: b63bd538-0a37-4ff1-90ba-081c5138dda0</span></span>  
<span data-ttu-id="950a3-609">Bereichsbezogen: 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="950a3-609">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="950a3-610">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="950a3-610">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="950a3-611">Instanz: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="950a3-611">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="950a3-612">`OperationService`-Vorgänge:</span><span class="sxs-lookup"><span data-stu-id="950a3-612">`OperationService` operations:</span></span>

<span data-ttu-id="950a3-613">Vorübergehend: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span><span class="sxs-lookup"><span data-stu-id="950a3-613">Transient: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span></span>  
<span data-ttu-id="950a3-614">Bereichsbezogen: 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="950a3-614">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="950a3-615">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="950a3-615">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="950a3-616">Instanz: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="950a3-616">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="950a3-617">Beachten Sie, welche der `OperationId`-Werte innerhalb einer Anforderung und zwischen Anforderungen variieren:</span><span class="sxs-lookup"><span data-stu-id="950a3-617">Observe which of the `OperationId` values vary within a request and between requests:</span></span>

* <span data-ttu-id="950a3-618">Objekte vom Typ *Vorübergehend* sind immer unterschiedlich.</span><span class="sxs-lookup"><span data-stu-id="950a3-618">*Transient* objects are always different.</span></span> <span data-ttu-id="950a3-619">Der vorübergehende `OperationId`-Wert für die ersten und zweiten Clientanforderungen ist für beide `OperationService`-Vorgänge und clientanforderungsübergreifend unterschiedlich.</span><span class="sxs-lookup"><span data-stu-id="950a3-619">The transient `OperationId` value for both the first and second client requests are different for both `OperationService` operations and across client requests.</span></span> <span data-ttu-id="950a3-620">Eine neue Instanz wird für jede Dienstanforderung und jede Clientanforderung bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="950a3-620">A new instance is provided to each service request and client request.</span></span>
* <span data-ttu-id="950a3-621">*Bereichsbezogene* Objekte sind innerhalb einer Clientanforderung identisch, clientanforderungsübergreifend sind sie jedoch unterschiedlich.</span><span class="sxs-lookup"><span data-stu-id="950a3-621">*Scoped* objects are the same within a client request but different across client requests.</span></span>
* <span data-ttu-id="950a3-622">Objekte vom Typ *Singleton* sind bei jedem Objekt und in jeder Anforderung identisch (unabhängig davon, ob eine `Operation`-Instanz in `Startup.ConfigureServices` bereitgestellt wird).</span><span class="sxs-lookup"><span data-stu-id="950a3-622">*Singleton* objects are the same for every object and every request regardless of whether an `Operation` instance is provided in `Startup.ConfigureServices`.</span></span>

## <a name="call-services-from-main"></a><span data-ttu-id="950a3-623">Abrufen von Diensten aus „Main“</span><span class="sxs-lookup"><span data-stu-id="950a3-623">Call services from main</span></span>

<span data-ttu-id="950a3-624">Erstellen Sie <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> mit [IServiceScopeFactory.CreateScope](xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope*), um einen bereichsbezogenen Dienst innerhalb des Anwendungsbereichs aufzulösen.</span><span class="sxs-lookup"><span data-stu-id="950a3-624">Create an <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> with [IServiceScopeFactory.CreateScope](xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope*) to resolve a scoped service within the app's scope.</span></span> <span data-ttu-id="950a3-625">Dieser Ansatz eignet sich gut dafür, beim Start auf einen bereichsbezogenen Dienst zuzugreifen und Initialisierungsaufgaben auszuführen.</span><span class="sxs-lookup"><span data-stu-id="950a3-625">This approach is useful to access a scoped service at startup to run initialization tasks.</span></span> <span data-ttu-id="950a3-626">Das folgende Beispiel veranschaulicht, wie man einen Kontext für `MyScopedService` in `Program.Main` erhält:</span><span class="sxs-lookup"><span data-stu-id="950a3-626">The following example shows how to obtain a context for the `MyScopedService` in `Program.Main`:</span></span>

```csharp
using System;
using System.Threading.Tasks;
using Microsoft.AspNetCore;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Logging;

public class Program
{
    public static async Task Main(string[] args)
    {
        var host = CreateWebHostBuilder(args).Build();

        using (var serviceScope = host.Services.CreateScope())
        {
            var services = serviceScope.ServiceProvider;

            try
            {
                var serviceContext = services.GetRequiredService<MyScopedService>();
                // Use the context here
            }
            catch (Exception ex)
            {
                var logger = services.GetRequiredService<ILogger<Program>>();
                logger.LogError(ex, "An error occurred.");
            }
        }
    
        await host.RunAsync();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .UseStartup<Startup>();
}
```

## <a name="scope-validation"></a><span data-ttu-id="950a3-627">Bereichsvalidierung</span><span class="sxs-lookup"><span data-stu-id="950a3-627">Scope validation</span></span>

<span data-ttu-id="950a3-628">Wenn die App in der Entwicklungsumgebung ausgeführt wird, überprüft der Standarddienstanbieter, ob:</span><span class="sxs-lookup"><span data-stu-id="950a3-628">When the app is running in the Development environment, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="950a3-629">Bereichsbezogene Dienste nicht direkt oder indirekt vom Stammdienstanbieter aufgelöst werden</span><span class="sxs-lookup"><span data-stu-id="950a3-629">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="950a3-630">Bereichsbezogene Dienste nicht direkt oder indirekt in Singletons eingefügt werden</span><span class="sxs-lookup"><span data-stu-id="950a3-630">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="950a3-631">Der Stammdienstanbieter wird erstellt, wenn <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider*> aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="950a3-631">The root service provider is created when <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider*> is called.</span></span> <span data-ttu-id="950a3-632">Die Lebensdauer des Stammdienstanbieters entspricht der Lebensdauer der App/des Servers, wenn der Anbieter mit der App erstellt wird und verworfen wird, wenn die App beendet wird.</span><span class="sxs-lookup"><span data-stu-id="950a3-632">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="950a3-633">Bereichsbezogene Dienste werden von dem Container verworfen, der sie erstellt hat.</span><span class="sxs-lookup"><span data-stu-id="950a3-633">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="950a3-634">Wenn ein bereichsbezogener Dienst im Stammcontainer erstellt wird, wird die Lebensdauer effektiv auf Singleton heraufgestuft, da er nur vom Stammcontainer verworfen wird, wenn die App/der Server heruntergefahren wird.</span><span class="sxs-lookup"><span data-stu-id="950a3-634">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="950a3-635">Die Überprüfung bereichsbezogener Dienste erfasst diese Situationen, wenn `BuildServiceProvider` aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="950a3-635">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="950a3-636">Weitere Informationen finden Sie unter <xref:fundamentals/host/web-host#scope-validation>.</span><span class="sxs-lookup"><span data-stu-id="950a3-636">For more information, see <xref:fundamentals/host/web-host#scope-validation>.</span></span>

## <a name="request-services"></a><span data-ttu-id="950a3-637">Anfordern von Diensten</span><span class="sxs-lookup"><span data-stu-id="950a3-637">Request Services</span></span>

<span data-ttu-id="950a3-638">Die Dienste, die innerhalb einer ASP.NET Core-Anforderung von `HttpContext` verfügbar sind, werden über die [HttpContext.RequestService](xref:Microsoft.AspNetCore.Http.HttpContext.RequestServices)-Sammlung verfügbar gemacht.</span><span class="sxs-lookup"><span data-stu-id="950a3-638">The services available within an ASP.NET Core request from `HttpContext` are exposed through the [HttpContext.RequestServices](xref:Microsoft.AspNetCore.Http.HttpContext.RequestServices) collection.</span></span>

<span data-ttu-id="950a3-639">Anforderungsdienste stellen die Dienste dar, die als Teil der App konfiguriert und angefordert werden.</span><span class="sxs-lookup"><span data-stu-id="950a3-639">Request Services represent the services configured and requested as part of the app.</span></span> <span data-ttu-id="950a3-640">Wenn Objekte Abhängigkeiten angeben, werden diese von den in `RequestServices` gefundenen Typen erfüllt (nicht in `ApplicationServices`).</span><span class="sxs-lookup"><span data-stu-id="950a3-640">When the objects specify dependencies, these are satisfied by the types found in `RequestServices`, not `ApplicationServices`.</span></span>

<span data-ttu-id="950a3-641">Generell sollte die App diese Eigenschaften nicht direkt verwenden.</span><span class="sxs-lookup"><span data-stu-id="950a3-641">Generally, the app shouldn't use these properties directly.</span></span> <span data-ttu-id="950a3-642">Fordern Sie stattdessen die von Klassen benötigten Typen über Klassenkonstruktoren an, und lassen Sie das Framework die Abhängigkeiten einfügen.</span><span class="sxs-lookup"><span data-stu-id="950a3-642">Instead, request the types that classes require via class constructors and allow the framework inject the dependencies.</span></span> <span data-ttu-id="950a3-643">So erhalten Sie Klassen, die einfacher getestet werden können.</span><span class="sxs-lookup"><span data-stu-id="950a3-643">This yields classes that are easier to test.</span></span>

> [!NOTE]
> <span data-ttu-id="950a3-644">Fordern Sie für den Zugriff auf die `RequestServices`-Sammlung Abhängigkeiten lieber als Konstruktorparameter an.</span><span class="sxs-lookup"><span data-stu-id="950a3-644">Prefer requesting dependencies as constructor parameters to accessing the `RequestServices` collection.</span></span>

## <a name="design-services-for-dependency-injection"></a><span data-ttu-id="950a3-645">Entwerfen von Diensten für die Abhängigkeitsinjektion</span><span class="sxs-lookup"><span data-stu-id="950a3-645">Design services for dependency injection</span></span>

<span data-ttu-id="950a3-646">Best Practices:</span><span class="sxs-lookup"><span data-stu-id="950a3-646">Best practices are to:</span></span>

* <span data-ttu-id="950a3-647">Entwerfen Sie Dienste zur Verwendung der Abhängigkeitsinjektion, um ihre Abhängigkeiten zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="950a3-647">Design services to use dependency injection to obtain their dependencies.</span></span>
* <span data-ttu-id="950a3-648">Vermeiden Sie zustandsbehaftete statische Klassen und Member.</span><span class="sxs-lookup"><span data-stu-id="950a3-648">Avoid stateful, static classes and members.</span></span> <span data-ttu-id="950a3-649">Entwerfen Sie Apps so, dass stattdessen Singletondienste verwendet werden. Diese vermeiden das Entstehen eines globalen Zustands.</span><span class="sxs-lookup"><span data-stu-id="950a3-649">Design apps to use singleton services instead, which avoid creating global state.</span></span>
* <span data-ttu-id="950a3-650">Vermeiden Sie die direkte Instanziierung abhängiger Klassen innerhalb von Diensten.</span><span class="sxs-lookup"><span data-stu-id="950a3-650">Avoid direct instantiation of dependent classes within services.</span></span> <span data-ttu-id="950a3-651">Die direkte Instanziierung koppelt den Code an eine bestimmte Implementierung.</span><span class="sxs-lookup"><span data-stu-id="950a3-651">Direct instantiation couples the code to a particular implementation.</span></span>
* <span data-ttu-id="950a3-652">Erstellen Sie kleine, gut gestaltete und einfach zu testende Apps.</span><span class="sxs-lookup"><span data-stu-id="950a3-652">Make app classes small, well-factored, and easily tested.</span></span>

<span data-ttu-id="950a3-653">Wenn eine Klasse zu viele eingefügte Abhängigkeiten zu haben scheint, ist dies im Allgemeinen ein Zeichen dafür, dass die Klasse zu viele Aufgaben hat und gegen das [Single-Responsibility-Prinzip (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility) (Prinzip der eindeutigen Verantwortlichkeit) verstößt.</span><span class="sxs-lookup"><span data-stu-id="950a3-653">If a class seems to have too many injected dependencies, this is generally a sign that the class has too many responsibilities and is violating the [Single Responsibility Principle (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility).</span></span> <span data-ttu-id="950a3-654">Versuchen Sie, die Klasse umzugestalten, indem Sie einige ihrer Aufgaben in eine neue Klasse verschieben.</span><span class="sxs-lookup"><span data-stu-id="950a3-654">Attempt to refactor the class by moving some of its responsibilities into a new class.</span></span> <span data-ttu-id="950a3-655">Beachten Sie, dass der Fokus der Razor Pages-Seitenmodellklassen und MVC-Controllerklassen auf der Benutzeroberfläche liegt.</span><span class="sxs-lookup"><span data-stu-id="950a3-655">Keep in mind that Razor Pages page model classes and MVC controller classes should focus on UI concerns.</span></span> <span data-ttu-id="950a3-656">Geschäftsregeln und Implementierungsdetails für den Datenzugriff sollten in Klassen aufbewahrt werden gemäß dem Prinzip [Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) (Trennung der Zuständigkeiten).</span><span class="sxs-lookup"><span data-stu-id="950a3-656">Business rules and data access implementation details should be kept in classes appropriate to these [separate concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns).</span></span>

### <a name="disposal-of-services"></a><span data-ttu-id="950a3-657">Löschen von Diensten</span><span class="sxs-lookup"><span data-stu-id="950a3-657">Disposal of services</span></span>

<span data-ttu-id="950a3-658">Der Container ruft <xref:System.IDisposable.Dispose*> für die erstellten <xref:System.IDisposable>-Typen auf.</span><span class="sxs-lookup"><span data-stu-id="950a3-658">The container calls <xref:System.IDisposable.Dispose*> for the <xref:System.IDisposable> types it creates.</span></span> <span data-ttu-id="950a3-659">Wenn eine Instanz dem Container per Benutzercode hinzugefügt wird, wird sie nicht automatisch gelöscht.</span><span class="sxs-lookup"><span data-stu-id="950a3-659">If an instance is added to the container by user code, it isn't disposed automatically.</span></span>

<span data-ttu-id="950a3-660">Im folgenden Beispiel werden die Dienste vom Dienstcontainer erstellt und automatisch verworfen:</span><span class="sxs-lookup"><span data-stu-id="950a3-660">In the following example, the services are created by the service container and disposed automatically:</span></span>

```csharp
public class Service1 : IDisposable {}
public class Service2 : IDisposable {}

public interface IService3 {}
public class Service3 : IService3, IDisposable {}

public void ConfigureServices(IServiceCollection services)
{
    services.AddScoped<Service1>();
    services.AddSingleton<Service2>();
    services.AddSingleton<IService3>(sp => new Service3());
}
```

<span data-ttu-id="950a3-661">Im folgenden Beispiel:</span><span class="sxs-lookup"><span data-stu-id="950a3-661">In the following example:</span></span>

* <span data-ttu-id="950a3-662">werden die Dienstinstanzen nicht vom Dienstcontainer erstellt.</span><span class="sxs-lookup"><span data-stu-id="950a3-662">The service instances aren't created by the service container.</span></span>
* <span data-ttu-id="950a3-663">kennt das Framework die geplante Lebensdauer der Dienste nicht.</span><span class="sxs-lookup"><span data-stu-id="950a3-663">The intended service lifetimes aren't known by the framework.</span></span>
* <span data-ttu-id="950a3-664">verwirft das Framework die Dienste nicht automatisch.</span><span class="sxs-lookup"><span data-stu-id="950a3-664">The framework doesn't dispose of the services automatically.</span></span>
* <span data-ttu-id="950a3-665">werden Dienste, die nicht explizit im Entwicklercode verworfen werden, weiter ausgeführt, bis die App beendet wird.</span><span class="sxs-lookup"><span data-stu-id="950a3-665">If the services aren't explicitly disposed in developer code, they persist until the app shuts down.</span></span>

```csharp
public class Service1 : IDisposable {}
public class Service2 : IDisposable {}

public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<Service1>(new Service1());
    services.AddSingleton(new Service2());
}
```

### <a name="idisposable-guidance-for-transient-and-shared-instances"></a><span data-ttu-id="950a3-666">IDisposable-Anleitung für vorübergehende and freigegebene Instanzen</span><span class="sxs-lookup"><span data-stu-id="950a3-666">IDisposable guidance for Transient and shared instances</span></span>

#### <a name="transient-limited-lifetime"></a><span data-ttu-id="950a3-667">Vorübergehende, eingeschränkte Lebensdauer</span><span class="sxs-lookup"><span data-stu-id="950a3-667">Transient, limited lifetime</span></span>

<span data-ttu-id="950a3-668">**Szenario**</span><span class="sxs-lookup"><span data-stu-id="950a3-668">**Scenario**</span></span>

<span data-ttu-id="950a3-669">Für die App ist eine <xref:System.IDisposable>-Instanz mit einer vorübergehenden Lebensdauer für eines der folgenden Szenarios erforderlich:</span><span class="sxs-lookup"><span data-stu-id="950a3-669">The app requires an <xref:System.IDisposable> instance with a transient lifetime for either of the following scenarios:</span></span>

* <span data-ttu-id="950a3-670">Die-Instanz wird im Stammbereich aufgelöst.</span><span class="sxs-lookup"><span data-stu-id="950a3-670">The instance is resolved in the root scope.</span></span>
* <span data-ttu-id="950a3-671">Die Instanz sollte verworfen werden, bevor der Bereich endet.</span><span class="sxs-lookup"><span data-stu-id="950a3-671">The instance should be disposed before the scope ends.</span></span>

<span data-ttu-id="950a3-672">**Lösung**</span><span class="sxs-lookup"><span data-stu-id="950a3-672">**Solution**</span></span>

<span data-ttu-id="950a3-673">Verwenden Sie das Factorymuster, um eine Instanz außerhalb des übergeordneten Bereichs zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="950a3-673">Use the factory pattern to create an instance outside of the parent scope.</span></span> <span data-ttu-id="950a3-674">In dieser Situation verfügt die App in der Regel über eine `Create`-Methode, die den Konstruktor des endgültigen Typs direkt aufruft.</span><span class="sxs-lookup"><span data-stu-id="950a3-674">In this situation, the app would generally have a `Create` method that calls the final type's constructor directly.</span></span> <span data-ttu-id="950a3-675">Wenn der endgültige Typ andere Abhängigkeiten aufweist, kann die Factory folgende Aktionen ausführen:</span><span class="sxs-lookup"><span data-stu-id="950a3-675">If the final type has other dependencies, the factory can:</span></span>

* <span data-ttu-id="950a3-676">Sie kann <xref:System.IServiceProvider> im Konstruktor empfangen.</span><span class="sxs-lookup"><span data-stu-id="950a3-676">Receive an <xref:System.IServiceProvider> in its constructor.</span></span>
* <span data-ttu-id="950a3-677">Sie kann <xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities.CreateInstance%2A?displayProperty=nameWithType> verwenden, um eine Instanz außerhalb des Containers zu instanziieren und dabei den Container für die Abhängigkeiten verwenden.</span><span class="sxs-lookup"><span data-stu-id="950a3-677">Use <xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities.CreateInstance%2A?displayProperty=nameWithType> to instantiate the instance outside the container, while using the container for its dependencies.</span></span>

#### <a name="shared-instance-limited-lifetime"></a><span data-ttu-id="950a3-678">Freigegebene Instanz, eingeschränkte Lebensdauer</span><span class="sxs-lookup"><span data-stu-id="950a3-678">Shared Instance, limited lifetime</span></span>

<span data-ttu-id="950a3-679">**Szenario**</span><span class="sxs-lookup"><span data-stu-id="950a3-679">**Scenario**</span></span>

<span data-ttu-id="950a3-680">Für die App ist eine freigegebene <xref:System.IDisposable>-Instanz über mehrere Dienste erforderlich, die <xref:System.IDisposable> sollte jedoch eine begrenzte Lebensdauer haben.</span><span class="sxs-lookup"><span data-stu-id="950a3-680">The app requires a shared <xref:System.IDisposable> instance across multiple services, but the <xref:System.IDisposable> should have a limited lifetime.</span></span>

<span data-ttu-id="950a3-681">**Lösung**</span><span class="sxs-lookup"><span data-stu-id="950a3-681">**Solution**</span></span>

<span data-ttu-id="950a3-682">Registrieren Sie die Instanz mit einer bereichsbezogenen Lebensdauer.</span><span class="sxs-lookup"><span data-stu-id="950a3-682">Register the instance with a Scoped lifetime.</span></span> <span data-ttu-id="950a3-683">Verwenden Sie <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope%2A?displayProperty=nameWithType> für den Start und zum Erstellen einer neuen <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>.</span><span class="sxs-lookup"><span data-stu-id="950a3-683">Use <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope%2A?displayProperty=nameWithType> to start and create a new <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>.</span></span> <span data-ttu-id="950a3-684">Verwenden Sie die <xref:System.IServiceProvider>-Schnittstelle des Bereichs, um die erforderlichen Dienste abzurufen.</span><span class="sxs-lookup"><span data-stu-id="950a3-684">Use the scope's <xref:System.IServiceProvider> to get required services.</span></span> <span data-ttu-id="950a3-685">Löschen Sie den Bereich, wenn die Lebensdauer beendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="950a3-685">Dispose the scope when the lifetime should end.</span></span>

#### <a name="general-guidelines"></a><span data-ttu-id="950a3-686">Allgemeine Richtlinien</span><span class="sxs-lookup"><span data-stu-id="950a3-686">General Guidelines</span></span>

* <span data-ttu-id="950a3-687">Registrieren Sie keine <xref:System.IDisposable>-Instanzen mit einem vorübergehenden Gültigkeitsbereich.</span><span class="sxs-lookup"><span data-stu-id="950a3-687">Don't register <xref:System.IDisposable> instances with a Transient scope.</span></span> <span data-ttu-id="950a3-688">Verwenden Sie stattdessen das Factorymuster.</span><span class="sxs-lookup"><span data-stu-id="950a3-688">Use the factory pattern instead.</span></span>
* <span data-ttu-id="950a3-689">Lösen Sie keine vorübergehenden oder bereichsbezogenen <xref:System.IDisposable>-Instanzen im Stammbereich auf.</span><span class="sxs-lookup"><span data-stu-id="950a3-689">Don't resolve Transient or Scoped <xref:System.IDisposable> instances in the root scope.</span></span> <span data-ttu-id="950a3-690">Die einzige allgemeine Ausnahme gilt, wenn die App die <xref:System.IServiceProvider>-Instanz erstellt bzw. erneut erstellt und freigibt, was kein ideales Muster ist.</span><span class="sxs-lookup"><span data-stu-id="950a3-690">The only general exception is when the app creates/recreates and disposes the <xref:System.IServiceProvider>, which isn't an ideal pattern.</span></span>
* <span data-ttu-id="950a3-691">Wenn eine <xref:System.IDisposable>-Abhängigkeit über DI empfangen wird, ist es nicht erforderlich, dass der Empfänger <xref:System.IDisposable> selbst implementiert.</span><span class="sxs-lookup"><span data-stu-id="950a3-691">Receiving an <xref:System.IDisposable> dependency via DI doesn't require that the receiver implement <xref:System.IDisposable> itself.</span></span> <span data-ttu-id="950a3-692">Der Empfänger der <xref:System.IDisposable>-Abhängigkeit darf <xref:System.IDisposable.Dispose%2A> auf dieser Abhängigkeit nicht abrufen.</span><span class="sxs-lookup"><span data-stu-id="950a3-692">The receiver of the <xref:System.IDisposable> dependency shouldn't call <xref:System.IDisposable.Dispose%2A> on that dependency.</span></span>
* <span data-ttu-id="950a3-693">Bereiche sollten verwendet werden, um die Lebensdauer von Diensten zu steuern.</span><span class="sxs-lookup"><span data-stu-id="950a3-693">Scopes should be used to control lifetimes of services.</span></span> <span data-ttu-id="950a3-694">Bereiche sind nicht hierarchisch, und es gibt keine besondere Verbindung zwischen den Bereichen.</span><span class="sxs-lookup"><span data-stu-id="950a3-694">Scopes aren't hierarchical, and there's no special connection among scopes.</span></span>

## <a name="default-service-container-replacement"></a><span data-ttu-id="950a3-695">Ersetzen von Standarddienstcontainern</span><span class="sxs-lookup"><span data-stu-id="950a3-695">Default service container replacement</span></span>

<span data-ttu-id="950a3-696">Der integrierte Dienstcontainer dient dazu, die Anforderungen des Frameworks und der meisten Consumer-Apps zu erfüllen.</span><span class="sxs-lookup"><span data-stu-id="950a3-696">The built-in service container is designed to serve the needs of the framework and most consumer apps.</span></span> <span data-ttu-id="950a3-697">Die Verwendung des integrierten Containers wird empfohlen, es sei denn, Sie benötigen ein bestimmtes Feature, das von diesem nicht unterstützt wird. Einige Beispiele:</span><span class="sxs-lookup"><span data-stu-id="950a3-697">We recommend using the built-in container unless you need a specific feature that the built-in container doesn't support, such as:</span></span>

* <span data-ttu-id="950a3-698">Eigenschaftsinjektion</span><span class="sxs-lookup"><span data-stu-id="950a3-698">Property injection</span></span>
* <span data-ttu-id="950a3-699">Auf Namen basierende Injektion</span><span class="sxs-lookup"><span data-stu-id="950a3-699">Injection based on name</span></span>
* <span data-ttu-id="950a3-700">Untergeordnete Container</span><span class="sxs-lookup"><span data-stu-id="950a3-700">Child containers</span></span>
* <span data-ttu-id="950a3-701">Benutzerdefinierte Verwaltung der Lebensdauer</span><span class="sxs-lookup"><span data-stu-id="950a3-701">Custom lifetime management</span></span>
* <span data-ttu-id="950a3-702">`Func<T>`-Unterstützung für die verzögerte Initialisierung</span><span class="sxs-lookup"><span data-stu-id="950a3-702">`Func<T>` support for lazy initialization</span></span>
* <span data-ttu-id="950a3-703">Konventionsbasierte Registrierung</span><span class="sxs-lookup"><span data-stu-id="950a3-703">Convention-based registration</span></span>

<span data-ttu-id="950a3-704">Die folgenden Container von Drittanbietern können mit ASP.NET Core-Apps verwendet werden:</span><span class="sxs-lookup"><span data-stu-id="950a3-704">The following 3rd party containers can be used with ASP.NET Core apps:</span></span>

* [<span data-ttu-id="950a3-705">Autofac</span><span class="sxs-lookup"><span data-stu-id="950a3-705">Autofac</span></span>](https://autofac.readthedocs.io/en/latest/integration/aspnetcore.html)
* [<span data-ttu-id="950a3-706">DryIoc</span><span class="sxs-lookup"><span data-stu-id="950a3-706">DryIoc</span></span>](https://www.nuget.org/packages/DryIoc.Microsoft.DependencyInjection)
* [<span data-ttu-id="950a3-707">Grace</span><span class="sxs-lookup"><span data-stu-id="950a3-707">Grace</span></span>](https://www.nuget.org/packages/Grace.DependencyInjection.Extensions)
* [<span data-ttu-id="950a3-708">LightInject</span><span class="sxs-lookup"><span data-stu-id="950a3-708">LightInject</span></span>](https://github.com/seesharper/LightInject.Microsoft.DependencyInjection)
* [<span data-ttu-id="950a3-709">Lamar</span><span class="sxs-lookup"><span data-stu-id="950a3-709">Lamar</span></span>](https://jasperfx.github.io/lamar/)
* [<span data-ttu-id="950a3-710">Stashbox</span><span class="sxs-lookup"><span data-stu-id="950a3-710">Stashbox</span></span>](https://github.com/z4kn4fein/stashbox-extensions-dependencyinjection)
* [<span data-ttu-id="950a3-711">Unity</span><span class="sxs-lookup"><span data-stu-id="950a3-711">Unity</span></span>](https://www.nuget.org/packages/Unity.Microsoft.DependencyInjection)

### <a name="thread-safety"></a><span data-ttu-id="950a3-712">Threadsicherheit</span><span class="sxs-lookup"><span data-stu-id="950a3-712">Thread safety</span></span>

<span data-ttu-id="950a3-713">Erstellen Sie threadsichere Singleton-Dienste.</span><span class="sxs-lookup"><span data-stu-id="950a3-713">Create thread-safe singleton services.</span></span> <span data-ttu-id="950a3-714">Wenn ein Singleton-Dienst eine Abhängigkeit von einem vorübergehenden Dienst aufweist, muss der vorübergehende Dienst abhängig von der Verwendungsweise durch das Singleton ebenfalls threadsicher sein.</span><span class="sxs-lookup"><span data-stu-id="950a3-714">If a singleton service has a dependency on a transient service, the transient service may also require thread safety depending how it's used by the singleton.</span></span>

<span data-ttu-id="950a3-715">Die Factorymethode des einzelnen Diensts, z. B. das zweite Argument für [AddSingleton\<TService>(IServiceCollection, Func\<IServiceProvider,TService>)](xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*), muss nicht threadsicher sein.</span><span class="sxs-lookup"><span data-stu-id="950a3-715">The factory method of single service, such as the second argument to [AddSingleton\<TService>(IServiceCollection, Func\<IServiceProvider,TService>)](xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*), doesn't need to be thread-safe.</span></span> <span data-ttu-id="950a3-716">Wie bei `static`-Konstruktoren erfolgt der Aufruf einmalig über einen einzelnen Thread.</span><span class="sxs-lookup"><span data-stu-id="950a3-716">Like a type (`static`) constructor, it's guaranteed to be called once by a single thread.</span></span>

## <a name="recommendations"></a><span data-ttu-id="950a3-717">Empfehlungen</span><span class="sxs-lookup"><span data-stu-id="950a3-717">Recommendations</span></span>

* <span data-ttu-id="950a3-718">`async/await`- und `Task`-basierte Dienstauflösung wird nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="950a3-718">`async/await` and `Task` based service resolution is not supported.</span></span> <span data-ttu-id="950a3-719">C# unterstützt keine asynchronen Konstruktoren. Daher wird empfohlen, asynchrone Methoden zu verwenden, nachdem der Dienst synchron aufgelöst wurde.</span><span class="sxs-lookup"><span data-stu-id="950a3-719">C# does not support asynchronous constructors; therefore, the recommended pattern is to use asynchronous methods after synchronously resolving the service.</span></span>

* <span data-ttu-id="950a3-720">Vermeiden Sie das Speichern von Daten und die direkte Konfiguration im Dienstcontainer.</span><span class="sxs-lookup"><span data-stu-id="950a3-720">Avoid storing data and configuration directly in the service container.</span></span> <span data-ttu-id="950a3-721">Der Einkaufswagen eines Benutzers sollte z. B. normalerweise nicht dem Dienstcontainer hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="950a3-721">For example, a user's shopping cart shouldn't typically be added to the service container.</span></span> <span data-ttu-id="950a3-722">Bei der Konfiguration sollte das [Optionsmuster](xref:fundamentals/configuration/options) verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="950a3-722">Configuration should use the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="950a3-723">Gleichermaßen sollten Sie „Datencontainer“-Objekte vermeiden, die nur vorhanden sind, um den Zugriff auf einige andere Objekte zuzulassen.</span><span class="sxs-lookup"><span data-stu-id="950a3-723">Similarly, avoid "data holder" objects that only exist to allow access to some other object.</span></span> <span data-ttu-id="950a3-724">Das tatsächlich benötige Element sollte besser über Dependency Injection angefordert werden.</span><span class="sxs-lookup"><span data-stu-id="950a3-724">It's better to request the actual item via DI.</span></span>

* <span data-ttu-id="950a3-725">Vermeiden Sie den statischen Zugriff auf Dienste (z. B. statische Eingabe von [IApplicationBuilder.ApplicationServices](xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices) zur Verwendung an anderer Stelle).</span><span class="sxs-lookup"><span data-stu-id="950a3-725">Avoid static access to services (for example, statically-typing [IApplicationBuilder.ApplicationServices](xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices) for use elsewhere).</span></span>

* <span data-ttu-id="950a3-726">Vermeiden Sie die Verwendung des *Dienstlocatormusters*, da dieses [Steuerungsumkehrungsstrategien](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) vermischt.</span><span class="sxs-lookup"><span data-stu-id="950a3-726">Avoid using the *service locator pattern*, which mixes [Inversion of Control](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) strategies.</span></span>

  * <span data-ttu-id="950a3-727">Rufen Sie beispielsweise nicht <xref:System.IServiceProvider.GetService*> auf, um eine Dienstinstanz zu erhalten, wenn Sie stattdessen Abhängigkeitsinjektion verwenden können:</span><span class="sxs-lookup"><span data-stu-id="950a3-727">Don't invoke <xref:System.IServiceProvider.GetService*> to obtain a service instance when you can use DI instead:</span></span>

    <span data-ttu-id="950a3-728">**Falsch:**</span><span class="sxs-lookup"><span data-stu-id="950a3-728">**Incorrect:**</span></span>

    ```csharp
    public class MyClass()
    {
        public void MyMethod()
        {
            var optionsMonitor = 
                _services.GetService<IOptionsMonitor<MyOptions>>();
            var option = optionsMonitor.CurrentValue.Option;

            ...
        }
    }
    ```

    <span data-ttu-id="950a3-729">**Richtig**:</span><span class="sxs-lookup"><span data-stu-id="950a3-729">**Correct**:</span></span>

    ```csharp
    public class MyClass
    {
        private readonly IOptionsMonitor<MyOptions> _optionsMonitor;

        public MyClass(IOptionsMonitor<MyOptions> optionsMonitor)
        {
            _optionsMonitor = optionsMonitor;
        }

        public void MyMethod()
        {
            var option = _optionsMonitor.CurrentValue.Option;

            ...
        }
    }
    ```

  * <span data-ttu-id="950a3-730">Vermeiden Sie die Injektion von Factorys, die Abhängigkeiten zur Laufzeit mit <xref:System.IServiceProvider.GetService*> auflösen.</span><span class="sxs-lookup"><span data-stu-id="950a3-730">Avoid injecting a factory that resolves dependencies at runtime using <xref:System.IServiceProvider.GetService*>.</span></span>

* <span data-ttu-id="950a3-731">Vermeiden Sie den statischen Zugriff auf `HttpContext` (z. B. [IHttpContextAccessor.HttpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext)).</span><span class="sxs-lookup"><span data-stu-id="950a3-731">Avoid static access to `HttpContext` (for example, [IHttpContextAccessor.HttpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext)).</span></span>

<span data-ttu-id="950a3-732">Wie bei allen Empfehlungen treffen Sie möglicherweise auf Situationen, in denen eine Empfehlung ignoriert werden muss.</span><span class="sxs-lookup"><span data-stu-id="950a3-732">Like all sets of recommendations, you may encounter situations where ignoring a recommendation is required.</span></span> <span data-ttu-id="950a3-733">Es gibt nur wenige Ausnahmen, die sich meistens auf besondere Fälle innerhalb des Frameworks beziehen.</span><span class="sxs-lookup"><span data-stu-id="950a3-733">Exceptions are rare, mostly special cases within the framework itself.</span></span>

<span data-ttu-id="950a3-734">Dependency Injection stellt eine *Alternative* zu statischen bzw. globalen Objektzugriffsmustern dar.</span><span class="sxs-lookup"><span data-stu-id="950a3-734">DI is an *alternative* to static/global object access patterns.</span></span> <span data-ttu-id="950a3-735">Sie werden keinen Nutzen aus der Dependency Injection ziehen können, wenn Sie diese mit dem Zugriff auf statische Objekte kombinieren.</span><span class="sxs-lookup"><span data-stu-id="950a3-735">You may not be able to realize the benefits of DI if you mix it with static object access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="950a3-736">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="950a3-736">Additional resources</span></span>

* <xref:mvc/views/dependency-injection>
* <xref:mvc/controllers/dependency-injection>
* <xref:security/authorization/dependencyinjection>
* <xref:blazor/fundamentals/dependency-injection>
* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/extensibility>
* [<span data-ttu-id="950a3-737">Vier Möglichkeiten zum Verwerfen von IDisposables in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="950a3-737">Four ways to dispose IDisposables in ASP.NET Core</span></span>](https://andrewlock.net/four-ways-to-dispose-idisposables-in-asp-net-core/)
* [<span data-ttu-id="950a3-738">Schreiben von sauberem Code in ASP.NET Core über Dependency Injection (MSDN)</span><span class="sxs-lookup"><span data-stu-id="950a3-738">Writing Clean Code in ASP.NET Core with Dependency Injection (MSDN)</span></span>](https://msdn.microsoft.com/magazine/mt703433.aspx)
* [<span data-ttu-id="950a3-739">Prinzip der expliziten Abhängigkeiten</span><span class="sxs-lookup"><span data-stu-id="950a3-739">Explicit Dependencies Principle</span></span>](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)
* <span data-ttu-id="950a3-740">[Umkehrung von Steuerungscontainern und das Abhängigkeitsinjektionsmuster (Martin Fowler)](https://www.martinfowler.com/articles/injection.html) (in englischer Sprache)</span><span class="sxs-lookup"><span data-stu-id="950a3-740">[Inversion of Control Containers and the Dependency Injection Pattern (Martin Fowler)](https://www.martinfowler.com/articles/injection.html)</span></span>
* [<span data-ttu-id="950a3-741">Registrieren eines Diensts mit mehreren Schnittstellen in ASP.NET Core DI</span><span class="sxs-lookup"><span data-stu-id="950a3-741">How to register a service with multiple interfaces in ASP.NET Core DI</span></span>](https://andrewlock.net/how-to-register-a-service-with-multiple-interfaces-for-in-asp-net-core-di/)

::: moniker-end
