---
title: Schützen von Blazor Server-Apps von ASP.NET Core
author: guardrex
description: Erfahren Sie, wie Sie Blazor Server-Apps als ASP.NET Core-Anwendungen schützen.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/02/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/security/server/index
ms.openlocfilehash: 69a24fc955a0f2fb524ec817eb50372052f538a1
ms.sourcegitcommit: 66fca14611eba141d455fe0bd2c37803062e439c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2020
ms.locfileid: "85944255"
---
# <a name="secure-aspnet-core-blazor-server-apps"></a><span data-ttu-id="00ad0-103">Schützen von Blazor Server-Apps von ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="00ad0-103">Secure ASP.NET Core Blazor Server apps</span></span>

<span data-ttu-id="00ad0-104">Von [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="00ad0-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="00ad0-105">Das Schützen von Blazor Server-Apps funktioniert genau wie bei ASP.NET Core-Apps.</span><span class="sxs-lookup"><span data-stu-id="00ad0-105">Blazor Server apps are configured for security in the same manner as ASP.NET Core apps.</span></span> <span data-ttu-id="00ad0-106">Weitere Informationen finden Sie in den Artikeln unter <xref:security/index>.</span><span class="sxs-lookup"><span data-stu-id="00ad0-106">For more information, see the articles under <xref:security/index>.</span></span> <span data-ttu-id="00ad0-107">Die Themen in dieser Übersicht gelten speziell für Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="00ad0-107">Topics under this overview apply specifically to Blazor Server.</span></span> 

## <a name="blazor-server-project-template"></a>Blazor Server<span data-ttu-id="00ad0-108">-Projektvorlage</span><span class="sxs-lookup"><span data-stu-id="00ad0-108"> project template</span></span>

<span data-ttu-id="00ad0-109">Die Projektvorlage Blazor Server kann beim Erstellen des Projekts für die Authentifizierung konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="00ad0-109">The Blazor Server project template can be configured for authentication when the project is created.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="00ad0-110">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="00ad0-110">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="00ad0-111">Befolgen Sie die Visual Studio-Anleitungen in <xref:blazor/tooling>, um ein neues Blazor Server-Projekt mit einem Authentifizierungsmechanismus zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="00ad0-111">Follow the Visual Studio guidance in <xref:blazor/tooling> to create a new Blazor Server project with an authentication mechanism.</span></span>

<span data-ttu-id="00ad0-112">Nachdem Sie im Dialogfeld **Neue ASP.NET Core-Webanwendung erstellen** die Vorlage **Blazor Server-App** ausgewählt haben, wählen Sie im Dialogfeld **Authentifizierung** die Option **Ändern** aus.</span><span class="sxs-lookup"><span data-stu-id="00ad0-112">After choosing the **Blazor Server App** template in the **Create a new ASP.NET Core Web Application** dialog, select **Change** under **Authentication**.</span></span>

<span data-ttu-id="00ad0-113">Ein Dialogfeld wird geöffnet, in dem dieselben Authentifizierungsmechanismen angeboten werden, die auch für andere ASP.NET-Core-Projekte verfügbar sind:</span><span class="sxs-lookup"><span data-stu-id="00ad0-113">A dialog opens to offer the same set of authentication mechanisms available for other ASP.NET Core projects:</span></span>

* <span data-ttu-id="00ad0-114">**Keine Authentifizierung**</span><span class="sxs-lookup"><span data-stu-id="00ad0-114">**No Authentication**</span></span>
* <span data-ttu-id="00ad0-115">**Einzelne Benutzerkonten**: Benutzerkonten können wie folgt gespeichert werden:</span><span class="sxs-lookup"><span data-stu-id="00ad0-115">**Individual User Accounts**: User accounts can be stored:</span></span>
  * <span data-ttu-id="00ad0-116">Innerhalb der App anhand des Systems [Identity](xref:security/authentication/identity) von ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="00ad0-116">Within the app using ASP.NET Core's [Identity](xref:security/authentication/identity) system.</span></span>
  * <span data-ttu-id="00ad0-117">Mit [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span><span class="sxs-lookup"><span data-stu-id="00ad0-117">With [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span></span>
* <span data-ttu-id="00ad0-118">**Geschäfts-, Schul- oder Unikonten**</span><span class="sxs-lookup"><span data-stu-id="00ad0-118">**Work or School Accounts**</span></span>
* <span data-ttu-id="00ad0-119">**Windows-Authentifizierung**</span><span class="sxs-lookup"><span data-stu-id="00ad0-119">**Windows Authentication**</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="00ad0-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="00ad0-120">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="00ad0-121">Befolgen Sie die Visual Studio Code-Anleitungen in <xref:blazor/tooling>, um ein neues Blazor Server-Projekt mit einem Authentifizierungsmechanismus zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="00ad0-121">Follow the Visual Studio Code guidance in <xref:blazor/tooling> to create a new Blazor Server project with an authentication mechanism:</span></span>

```dotnetcli
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

<span data-ttu-id="00ad0-122">Zulässige Authentifizierungswerte (`{AUTHENTICATION}`) werden in der folgenden Tabelle angezeigt.</span><span class="sxs-lookup"><span data-stu-id="00ad0-122">Permissible authentication values (`{AUTHENTICATION}`) are shown in the following table.</span></span>

| <span data-ttu-id="00ad0-123">Authentifizierungsmechanismus</span><span class="sxs-lookup"><span data-stu-id="00ad0-123">Authentication mechanism</span></span> | <span data-ttu-id="00ad0-124">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="00ad0-124">Description</span></span> |
| ------------------------ | ----------- |
| <span data-ttu-id="00ad0-125">`None` (Standardwert)</span><span class="sxs-lookup"><span data-stu-id="00ad0-125">`None` (default)</span></span>         | <span data-ttu-id="00ad0-126">Keine Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="00ad0-126">No authentication</span></span> |
| `Individual`             | <span data-ttu-id="00ad0-127">In der App mit ASP.NET Core-Identität gespeicherte Benutzer Identity</span><span class="sxs-lookup"><span data-stu-id="00ad0-127">Users stored in the app with ASP.NET Core Identity</span></span> |
| `IndividualB2C`          | <span data-ttu-id="00ad0-128">In [Azure AD B2C](xref:security/authentication/azure-ad-b2c) gespeicherte Benutzer</span><span class="sxs-lookup"><span data-stu-id="00ad0-128">Users stored in [Azure AD B2C](xref:security/authentication/azure-ad-b2c)</span></span> |
| `SingleOrg`              | <span data-ttu-id="00ad0-129">Organisationsauthentifizierung für einen einzelnen Mandanten</span><span class="sxs-lookup"><span data-stu-id="00ad0-129">Organizational authentication for a single tenant</span></span> |
| `MultiOrg`               | <span data-ttu-id="00ad0-130">Organisationsauthentifizierung für mehrere Mandanten</span><span class="sxs-lookup"><span data-stu-id="00ad0-130">Organizational authentication for multiple tenants</span></span> |
| `Windows`                | <span data-ttu-id="00ad0-131">Windows-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="00ad0-131">Windows Authentication</span></span> |

<span data-ttu-id="00ad0-132">Bei Verwenden der Option `-o|--output` nutzt der Befehl den für den Platzhalter `{APP NAME}` angegebenen Wert:</span><span class="sxs-lookup"><span data-stu-id="00ad0-132">Using the `-o|--output` option, the command uses the value provided for the `{APP NAME}` placeholder to:</span></span>

* <span data-ttu-id="00ad0-133">Erstellen Sie einen Ordner für das Projekt.</span><span class="sxs-lookup"><span data-stu-id="00ad0-133">Create a folder for the project.</span></span>
* <span data-ttu-id="00ad0-134">Benennen Sie das Projekt.</span><span class="sxs-lookup"><span data-stu-id="00ad0-134">Name the project.</span></span>

<span data-ttu-id="00ad0-135">Weitere Informationen finden Sie im Befehl [`dotnet new`](/dotnet/core/tools/dotnet-new) im Leitfaden für .NET Core.</span><span class="sxs-lookup"><span data-stu-id="00ad0-135">For more information, see the [`dotnet new`](/dotnet/core/tools/dotnet-new) command in the .NET Core Guide.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="00ad0-136">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="00ad0-136">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="00ad0-137">Befolgen Sie die Anleitungen für Visual Studio für Mac in <xref:blazor/tooling>.</span><span class="sxs-lookup"><span data-stu-id="00ad0-137">Follow the Visual Studio for Mac guidance in <xref:blazor/tooling>.</span></span>

1. <span data-ttu-id="00ad0-138">Wählen Sie im Schritt **Konfigurieren Ihrer neuen Blazor Server-App** in der Dropdownliste **Authentifizierung** die Option **Individual Authentication (in-app)** (Individuelle Authentifizierung [in der App]) aus.</span><span class="sxs-lookup"><span data-stu-id="00ad0-138">On the **Configure your new Blazor Server App** step, select **Individual Authentication (in-app)** from the **Authentication** drop down.</span></span>

1. <span data-ttu-id="00ad0-139">Die Anwendung wird für einzelne Benutzer erstellt, die in der App mit ASP.NET Core-Identität gespeichert sind Identity.</span><span class="sxs-lookup"><span data-stu-id="00ad0-139">The app is created for individual users stored in the app with ASP.NET Core Identity.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="00ad0-140">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="00ad0-140">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="00ad0-141">Erstellen Sie ein neues Blazor Server-Projekt mit einem Authentifizierungsmechanismus, indem Sie folgenden Befehl an einer Befehlsshell ausführen:</span><span class="sxs-lookup"><span data-stu-id="00ad0-141">Create a new Blazor Server project with an authentication mechanism using the following command in a command shell:</span></span>

```dotnetcli
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

<span data-ttu-id="00ad0-142">Zulässige Authentifizierungswerte (`{AUTHENTICATION}`) werden in der folgenden Tabelle angezeigt.</span><span class="sxs-lookup"><span data-stu-id="00ad0-142">Permissible authentication values (`{AUTHENTICATION}`) are shown in the following table.</span></span>

| <span data-ttu-id="00ad0-143">Authentifizierungsmechanismus</span><span class="sxs-lookup"><span data-stu-id="00ad0-143">Authentication mechanism</span></span> | <span data-ttu-id="00ad0-144">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="00ad0-144">Description</span></span> |
| ------------------------ | ----------- |
| <span data-ttu-id="00ad0-145">`None` (Standardwert)</span><span class="sxs-lookup"><span data-stu-id="00ad0-145">`None` (default)</span></span>         | <span data-ttu-id="00ad0-146">Keine Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="00ad0-146">No authentication</span></span> |
| `Individual`             | <span data-ttu-id="00ad0-147">In der App mit ASP.NET Core-Identität gespeicherte Benutzer Identity</span><span class="sxs-lookup"><span data-stu-id="00ad0-147">Users stored in the app with ASP.NET Core Identity</span></span> |
| `IndividualB2C`          | <span data-ttu-id="00ad0-148">In [Azure AD B2C](xref:security/authentication/azure-ad-b2c) gespeicherte Benutzer</span><span class="sxs-lookup"><span data-stu-id="00ad0-148">Users stored in [Azure AD B2C](xref:security/authentication/azure-ad-b2c)</span></span> |
| `SingleOrg`              | <span data-ttu-id="00ad0-149">Organisationsauthentifizierung für einen einzelnen Mandanten</span><span class="sxs-lookup"><span data-stu-id="00ad0-149">Organizational authentication for a single tenant</span></span> |
| `MultiOrg`               | <span data-ttu-id="00ad0-150">Organisationsauthentifizierung für mehrere Mandanten</span><span class="sxs-lookup"><span data-stu-id="00ad0-150">Organizational authentication for multiple tenants</span></span> |
| `Windows`                | <span data-ttu-id="00ad0-151">Windows-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="00ad0-151">Windows Authentication</span></span> |

<span data-ttu-id="00ad0-152">Bei Verwenden der Option `-o|--output` nutzt der Befehl den für den Platzhalter `{APP NAME}` angegebenen Wert:</span><span class="sxs-lookup"><span data-stu-id="00ad0-152">Using the `-o|--output` option, the command uses the value provided for the `{APP NAME}` placeholder to:</span></span>

* <span data-ttu-id="00ad0-153">Erstellen Sie einen Ordner für das Projekt.</span><span class="sxs-lookup"><span data-stu-id="00ad0-153">Create a folder for the project.</span></span>
* <span data-ttu-id="00ad0-154">Benennen Sie das Projekt.</span><span class="sxs-lookup"><span data-stu-id="00ad0-154">Name the project.</span></span>

<span data-ttu-id="00ad0-155">Weitere Informationen finden Sie im Befehl [`dotnet new`](/dotnet/core/tools/dotnet-new) im Leitfaden für .NET Core.</span><span class="sxs-lookup"><span data-stu-id="00ad0-155">For more information, see the [`dotnet new`](/dotnet/core/tools/dotnet-new) command in the .NET Core Guide.</span></span>

---

## <a name="scaffold-identity"></a><span data-ttu-id="00ad0-156">Gerüst Identity</span><span class="sxs-lookup"><span data-stu-id="00ad0-156">Scaffold Identity</span></span>

<span data-ttu-id="00ad0-157">Gerüst Identity in einem Blazor Server-Projekt:</span><span class="sxs-lookup"><span data-stu-id="00ad0-157">Scaffold Identity into a Blazor Server project:</span></span>

* <span data-ttu-id="00ad0-158">[Ohne vorhandene Autorisierung](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-blazor-server-project-without-existing-authorization)</span><span class="sxs-lookup"><span data-stu-id="00ad0-158">[Without existing authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-blazor-server-project-without-existing-authorization).</span></span>
* <span data-ttu-id="00ad0-159">[Mit Autorisierung](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-blazor-server-project-with-authorization)</span><span class="sxs-lookup"><span data-stu-id="00ad0-159">[With authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-blazor-server-project-with-authorization).</span></span>
