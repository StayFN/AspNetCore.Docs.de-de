---
title: Einführung in Identity ASP.net Core
author: rick-anderson
description: Verwenden Sie dies Identity mit einer ASP.net Core-app. Erfahren Sie, wie Sie Kenn Wort Anforderungen ("Requirements Digit", "Requirements dlength", "Requirements duniquechars" usw.) festlegen.
ms.author: riande
ms.date: 7/15/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/authentication/identity
ms.openlocfilehash: dd3296db568700a363c427398f02239846a46ada
ms.sourcegitcommit: 384833762c614851db653b841cc09fbc944da463
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/17/2020
ms.locfileid: "86445429"
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="4f627-104">Einführung in Identity ASP.net Core</span><span class="sxs-lookup"><span data-stu-id="4f627-104">Introduction to Identity on ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="4f627-105">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="4f627-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="4f627-106">ASP.NET Core Identity:</span><span class="sxs-lookup"><span data-stu-id="4f627-106">ASP.NET Core Identity:</span></span>

* <span data-ttu-id="4f627-107">Ist eine API, die die Benutzeroberflächen-Anmelde Funktionalität unterstützt.</span><span class="sxs-lookup"><span data-stu-id="4f627-107">Is an API that supports user interface (UI) login functionality.</span></span>
* <span data-ttu-id="4f627-108">Von werden Benutzer, Kenn Wörter, Profildaten, Rollen, Ansprüche, Token, e-Mail-Bestätigung und mehr verwaltet.</span><span class="sxs-lookup"><span data-stu-id="4f627-108">Manages users, passwords, profile data, roles, claims, tokens, email confirmation, and more.</span></span>

<span data-ttu-id="4f627-109">Benutzer können ein Konto mit den Anmelde Informationen erstellen, die in gespeichert Identity sind, oder Sie können einen externen Anmelde Anbieter verwenden.</span><span class="sxs-lookup"><span data-stu-id="4f627-109">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="4f627-110">Zu den unterstützten externen Anmelde Anbietern zählen [Facebook, Google, Microsoft-Konto und Twitter](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="4f627-110">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="4f627-111">Der [ Identity Quellcode](https://github.com/dotnet/AspNetCore/tree/master/src/Identity) ist auf GitHub verfügbar.</span><span class="sxs-lookup"><span data-stu-id="4f627-111">The [Identity source code](https://github.com/dotnet/AspNetCore/tree/master/src/Identity) is available on GitHub.</span></span> <span data-ttu-id="4f627-112">[Gerüst Identity ](xref:security/authentication/scaffold-identity) und zeigen Sie die generierten Dateien an, um die Vorlagen Interaktion mit zu überprüfen Identity .</span><span class="sxs-lookup"><span data-stu-id="4f627-112">[Scaffold Identity](xref:security/authentication/scaffold-identity) and view the generated files to review the template interaction with Identity.</span></span>

Identity<span data-ttu-id="4f627-113">wird in der Regel mit einer SQL Server Datenbank konfiguriert, um Benutzernamen, Kenn Wörter und Profildaten zu speichern.</span><span class="sxs-lookup"><span data-stu-id="4f627-113"> is typically configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="4f627-114">Alternativ können Sie auch einen weiteren permanenten Speicher verwenden, z. b. Azure Table Storage.</span><span class="sxs-lookup"><span data-stu-id="4f627-114">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

<span data-ttu-id="4f627-115">In diesem Thema erfahren Sie, wie Sie Identity einen Benutzer mit registrieren, anmelden und abmelden.</span><span class="sxs-lookup"><span data-stu-id="4f627-115">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="4f627-116">Hinweis: die Vorlagen behandeln Benutzername und e-Mail-Adresse für Benutzer als identisch.</span><span class="sxs-lookup"><span data-stu-id="4f627-116">Note: the templates treat username and email as the same for users.</span></span> <span data-ttu-id="4f627-117">Ausführlichere Anweisungen zum Erstellen von apps, die verwenden Identity , finden Sie unter [Nächste Schritte](#next).</span><span class="sxs-lookup"><span data-stu-id="4f627-117">For more detailed instructions about creating apps that use Identity, see [Next Steps](#next).</span></span>

<span data-ttu-id="4f627-118">Die [Microsoft Identity-Plattform](/azure/active-directory/develop/) ist:</span><span class="sxs-lookup"><span data-stu-id="4f627-118">[Microsoft identity platform](/azure/active-directory/develop/) is:</span></span>

* <span data-ttu-id="4f627-119">Eine Weiterentwicklung der Azure Active Directory (Azure AD)-Entwicklerplattform.</span><span class="sxs-lookup"><span data-stu-id="4f627-119">An evolution of the Azure Active Directory (Azure AD) developer platform.</span></span>
* <span data-ttu-id="4f627-120">Nicht im Zusammenhang mit ASP.net Core Identity .</span><span class="sxs-lookup"><span data-stu-id="4f627-120">Unrelated to ASP.NET Core Identity.</span></span>

[!INCLUDE[](~/includes/Identity<span data-ttu-id="4f627-121">Server4.MD)]</span><span class="sxs-lookup"><span data-stu-id="4f627-121">Server4.md)]</span></span>

<span data-ttu-id="4f627-122">[Anzeigen oder Herunterladen des Beispielcodes](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample) ([herunterladen)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="4f627-122">[View or download the sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample) ([how to download)](xref:index#how-to-download-a-sample)).</span></span>

<a name="adi"></a>

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="4f627-123">Erstellen einer Web-App mit Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="4f627-123">Create a Web app with authentication</span></span>

<span data-ttu-id="4f627-124">Erstellen Sie ein ASP.net Core Webanwendungs Projekt mit einzelnen Benutzerkonten.</span><span class="sxs-lookup"><span data-stu-id="4f627-124">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="4f627-125">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4f627-125">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="4f627-126">Wählen Sie **Datei** > **neu** > **Projekt**aus.</span><span class="sxs-lookup"><span data-stu-id="4f627-126">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="4f627-127">Wählen Sie **ASP.NET Core-Webanwendung** aus.</span><span class="sxs-lookup"><span data-stu-id="4f627-127">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="4f627-128">Nennen Sie das Projekt **"WebApp1"** , um den gleichen Namespace wie das Projekt herunterzuladen.</span><span class="sxs-lookup"><span data-stu-id="4f627-128">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="4f627-129">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="4f627-129">Click **OK**.</span></span>
* <span data-ttu-id="4f627-130">Wählen Sie eine ASP.net Core **Webanwendung**, und wählen Sie dann **Authentifizierung ändern**aus.</span><span class="sxs-lookup"><span data-stu-id="4f627-130">Select an ASP.NET Core **Web Application**, then select **Change Authentication**.</span></span>
* <span data-ttu-id="4f627-131">Wählen Sie **einzelne Benutzerkonten** , und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="4f627-131">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="4f627-132">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="4f627-132">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new webapp --auth Individual -o WebApp1
```

<span data-ttu-id="4f627-133">Der vorherige Befehl erstellt eine Razor Web-App mithilfe von SQLite.</span><span class="sxs-lookup"><span data-stu-id="4f627-133">The preceding command creates a Razor web app using SQLite.</span></span> <span data-ttu-id="4f627-134">Führen Sie den folgenden Befehl aus, um die Web-App mit localdb zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="4f627-134">To create the web app with LocalDB, run the following command:</span></span>

```dotnetcli
dotnet new webapp --auth Individual -uld -o WebApp1
```

---

<span data-ttu-id="4f627-135">Das generierte Projekt stellt [ASP.net Core Identity ](xref:security/authentication/identity) als [ Razor Klassenbibliothek](xref:razor-pages/ui-class)bereit.</span><span class="sxs-lookup"><span data-stu-id="4f627-135">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="4f627-136">Die Identity Razor Klassenbibliothek macht Endpunkte mit dem `Identity` Bereich verfügbar.</span><span class="sxs-lookup"><span data-stu-id="4f627-136">The Identity Razor Class Library exposes endpoints with the `Identity` area.</span></span> <span data-ttu-id="4f627-137">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="4f627-137">For example:</span></span>

* <span data-ttu-id="4f627-138">/Identity/Account/Login</span><span class="sxs-lookup"><span data-stu-id="4f627-138">/Identity/Account/Login</span></span>
* <span data-ttu-id="4f627-139">/Identity/Account/Logout</span><span class="sxs-lookup"><span data-stu-id="4f627-139">/Identity/Account/Logout</span></span>
* <span data-ttu-id="4f627-140">/Identity/Account/Manage</span><span class="sxs-lookup"><span data-stu-id="4f627-140">/Identity/Account/Manage</span></span>

### <a name="apply-migrations"></a><span data-ttu-id="4f627-141">Anwenden von Migrationen</span><span class="sxs-lookup"><span data-stu-id="4f627-141">Apply migrations</span></span>

<span data-ttu-id="4f627-142">Wenden Sie die Migrationen an, um die Datenbank zu initialisieren.</span><span class="sxs-lookup"><span data-stu-id="4f627-142">Apply the migrations to initialize the database.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="4f627-143">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4f627-143">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="4f627-144">Führen Sie den folgenden Befehl in der Paket-Manager-Konsole (PMC) aus:</span><span class="sxs-lookup"><span data-stu-id="4f627-144">Run the following command in the Package Manager Console (PMC):</span></span>

`PM> Update-Database`

# <a name="net-core-cli"></a>[<span data-ttu-id="4f627-145">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="4f627-145">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="4f627-146">In diesem Schritt sind keine Migrationen erforderlich, wenn SQLite verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="4f627-146">Migrations are not necessary at this step when using SQLite.</span></span>

[!INCLUDE [more information on the CLI for EF Core](~/includes/ef-cli.md)]

<span data-ttu-id="4f627-147">Führen Sie für localdb den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="4f627-147">For LocalDB, run the following command:</span></span>

```dotnetcli
dotnet ef database update
```

---

### <a name="test-register-and-login"></a><span data-ttu-id="4f627-148">Test Registrierung und Anmeldung</span><span class="sxs-lookup"><span data-stu-id="4f627-148">Test Register and Login</span></span>

<span data-ttu-id="4f627-149">Führen Sie die APP aus, und registrieren Sie einen Benutzer.</span><span class="sxs-lookup"><span data-stu-id="4f627-149">Run the app and register a user.</span></span> <span data-ttu-id="4f627-150">Abhängig von der Bildschirmgröße müssen Sie ggf. die Navigations Schaltfläche auswählen, um die Links **registrieren** und **Anmelden** anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="4f627-150">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>

### <a name="configure-identity-services"></a><span data-ttu-id="4f627-151">IdentityDienste konfigurieren</span><span class="sxs-lookup"><span data-stu-id="4f627-151">Configure Identity services</span></span>

<span data-ttu-id="4f627-152">Dienste werden in hinzugefügt `ConfigureServices` .</span><span class="sxs-lookup"><span data-stu-id="4f627-152">Services are added in `ConfigureServices`.</span></span> <span data-ttu-id="4f627-153">Das typische Muster besteht darin, alle `Add{Service}`-Methoden und dann alle `services.Configure{Service}`-Methoden aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="4f627-153">The typical pattern is to call all the `Add{Service}` methods, and then call all the `services.Configure{Service}` methods.</span></span>

[!code-csharp[](identity/sample/WebApp3/Startup.cs?name=snippet_configureservices&highlight=11-99)]

<span data-ttu-id="4f627-154">Der vorangehende markierte Code konfiguriert Identity mit Standard Options Werten.</span><span class="sxs-lookup"><span data-stu-id="4f627-154">The preceding highlighted code configures Identity with default option values.</span></span> <span data-ttu-id="4f627-155">Dienste werden über die [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection)der App zur Verfügung gestellt.</span><span class="sxs-lookup"><span data-stu-id="4f627-155">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

Identity<span data-ttu-id="4f627-156">wird aktiviert, indem aufgerufen wird <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> .</span><span class="sxs-lookup"><span data-stu-id="4f627-156"> is enabled by calling <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>.</span></span> <span data-ttu-id="4f627-157">`UseAuthentication`Fügt der Anforderungs Pipeline Authentifizierungs [Middleware](xref:fundamentals/middleware/index) hinzu.</span><span class="sxs-lookup"><span data-stu-id="4f627-157">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

[!code-csharp[](identity/sample/WebApp3/Startup.cs?name=snippet_configure&highlight=19)]

<span data-ttu-id="4f627-158">Die von der Vorlage generierte App verwendet keine [Autorisierung](xref:security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="4f627-158">The template-generated app doesn't use [authorization](xref:security/authorization/secure-data).</span></span> <span data-ttu-id="4f627-159">`app.UseAuthorization`ist enthalten, um sicherzustellen, dass Sie in der richtigen Reihenfolge hinzugefügt wird, wenn die APP eine Autorisierung</span><span class="sxs-lookup"><span data-stu-id="4f627-159">`app.UseAuthorization` is included to ensure it's added in the correct order should the app add authorization.</span></span> <span data-ttu-id="4f627-160">`UseRouting`, `UseAuthentication` , `UseAuthorization` und `UseEndpoints` müssen in der Reihenfolge aufgerufen werden, die im vorangehenden Code angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="4f627-160">`UseRouting`, `UseAuthentication`, `UseAuthorization`, and `UseEndpoints` must be called in the order shown in the preceding code.</span></span>

<span data-ttu-id="4f627-161">Weitere Informationen zu `IdentityOptions` und `Startup` finden Sie unter <xref:Microsoft.AspNetCore.Identity.IdentityOptions> und [Anwendungsstart](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="4f627-161">For more information on `IdentityOptions` and `Startup`, see <xref:Microsoft.AspNetCore.Identity.IdentityOptions> and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-logout-and-registerconfirmation"></a><span data-ttu-id="4f627-162">Gerüst Register, anmelden, abmelden und Register Bestätigung</span><span class="sxs-lookup"><span data-stu-id="4f627-162">Scaffold Register, Login, LogOut, and RegisterConfirmation</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="4f627-163">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4f627-163">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="4f627-164">Fügen Sie `Register` die `Login` Dateien,, `LogOut` und hinzu `RegisterConfirmation` .</span><span class="sxs-lookup"><span data-stu-id="4f627-164">Add the `Register`, `Login`, `LogOut`, and `RegisterConfirmation` files.</span></span> <span data-ttu-id="4f627-165">Befolgen Sie die [Gerüst Identität in einem Razor Projekt mit Autorisierungs](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) Anweisungen, um den in diesem Abschnitt gezeigten Code zu generieren.</span><span class="sxs-lookup"><span data-stu-id="4f627-165">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions to generate the code shown in this section.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="4f627-166">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="4f627-166">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="4f627-167">Wenn Sie das Projekt mit dem Namen **"WebApp1"** erstellt haben, führen Sie die folgenden Befehle aus.</span><span class="sxs-lookup"><span data-stu-id="4f627-167">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="4f627-168">Verwenden Sie andernfalls den korrekten Namespace für `ApplicationDbContext` :</span><span class="sxs-lookup"><span data-stu-id="4f627-168">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout;Account.RegisterConfirmation"
```

<span data-ttu-id="4f627-169">PowerShell verwendet ein Semikolon als Befehls Trennzeichen.</span><span class="sxs-lookup"><span data-stu-id="4f627-169">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="4f627-170">Wenn Sie PowerShell verwenden, führen Sie einen Escapezeichen in der Datei Liste aus, oder fügen Sie die Datei Liste in doppelte Anführungszeichen ein, wie im vorherigen Beispiel gezeigt.</span><span class="sxs-lookup"><span data-stu-id="4f627-170">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

<span data-ttu-id="4f627-171">Weitere Informationen zum Gerüstbau finden Sie unter Identity [Gerüst Identität in einem Razor Projekt mit Autorisierung](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization).</span><span class="sxs-lookup"><span data-stu-id="4f627-171">For more information on scaffolding Identity, see [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization).</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="4f627-172">Register überprüfen</span><span class="sxs-lookup"><span data-stu-id="4f627-172">Examine Register</span></span>

<span data-ttu-id="4f627-173">Wenn ein Benutzer auf die Schaltfläche **registrieren** auf der `Register` Seite klickt, `RegisterModel.OnPostAsync` wird die Aktion aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="4f627-173">When a user clicks the **Register** button on the `Register` page, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="4f627-174">Der Benutzer wird von " [kreateasync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) " für das `_userManager` Objekt erstellt:</span><span class="sxs-lookup"><span data-stu-id="4f627-174">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object:</span></span>

[!code-csharp[](identity/sample/WebApp3/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=9)]

<!-- .NET 5 fixes this, see
https://github.com/dotnet/aspnetcore/blob/master/src/Identity/UI/src/Areas/Identity/Pages/V4/Account/RegisterConfirmation.cshtml.cs#L74-L77
-->
[!INCLUDE[](~/includes/disableVer.md)]

### <a name="log-in"></a><span data-ttu-id="4f627-175">Anmelden</span><span class="sxs-lookup"><span data-stu-id="4f627-175">Log in</span></span>

<span data-ttu-id="4f627-176">Das Anmeldeformular wird angezeigt, wenn Folgendes gilt:</span><span class="sxs-lookup"><span data-stu-id="4f627-176">The Login form is displayed when:</span></span>

* <span data-ttu-id="4f627-177">Der Link " **Anmelden** " ist ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="4f627-177">The **Log in** link is selected.</span></span>
* <span data-ttu-id="4f627-178">Ein Benutzer versucht, auf eine eingeschränkte Seite zuzugreifen, für die er nicht autorisiert ist, **oder** wenn er nicht vom System authentifiziert wurde.</span><span class="sxs-lookup"><span data-stu-id="4f627-178">A user attempts to access a restricted page that they aren't authorized to access **or** when they haven't been authenticated by the system.</span></span>

<span data-ttu-id="4f627-179">Wenn das Formular auf der Anmeldeseite übermittelt wird, `OnPostAsync` wird die Aktion aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="4f627-179">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="4f627-180">`PasswordSignInAsync`wird für das- `_signInManager` Objekt aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="4f627-180">`PasswordSignInAsync` is called on the `_signInManager` object.</span></span>

[!code-csharp[](identity/sample/WebApp3/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=10-11)]

<span data-ttu-id="4f627-181">Informationen dazu, wie Sie Autorisierungs Entscheidungen treffen, finden Sie unter <xref:security/authorization/introduction> .</span><span class="sxs-lookup"><span data-stu-id="4f627-181">For information on how to make authorization decisions, see <xref:security/authorization/introduction>.</span></span>

### <a name="log-out"></a><span data-ttu-id="4f627-182">Abmelden</span><span class="sxs-lookup"><span data-stu-id="4f627-182">Log out</span></span>

<span data-ttu-id="4f627-183">Der Link " **Abmelden** " Ruft die `LogoutModel.OnPost` Aktion auf.</span><span class="sxs-lookup"><span data-stu-id="4f627-183">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/WebApp3/Areas/Identity/Pages/Account/Logout.cshtml.cs?highlight=36)]

<span data-ttu-id="4f627-184">Im vorangehenden Code muss der Code `return RedirectToPage();` eine Umleitung sein, damit der Browser eine neue Anforderung ausführt und die Identität des Benutzers aktualisiert wird.</span><span class="sxs-lookup"><span data-stu-id="4f627-184">In the preceding code, the code `return RedirectToPage();` needs to be a redirect so that the browser performs a new request and the identity for the user gets updated.</span></span>

<span data-ttu-id="4f627-185">[Signoutasync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) löscht die in einem Cookie gespeicherten Ansprüche des Benutzers.</span><span class="sxs-lookup"><span data-stu-id="4f627-185">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span>

<span data-ttu-id="4f627-186">Post wird in der Datei *pages/Shared/_LoginPartial. cshtml*angegeben:</span><span class="sxs-lookup"><span data-stu-id="4f627-186">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-cshtml[](identity/sample/WebApp3/Pages/Shared/_LoginPartial.cshtml?highlight=15)]

## <a name="test-identity"></a><span data-ttu-id="4f627-187">TestIdentity</span><span class="sxs-lookup"><span data-stu-id="4f627-187">Test Identity</span></span>

<span data-ttu-id="4f627-188">Die Standardweb Projektvorlagen ermöglichen den anonymen Zugriff auf die Startseiten.</span><span class="sxs-lookup"><span data-stu-id="4f627-188">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="4f627-189">IdentityFügen Sie zum Testen Folgendes hinzu [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) :</span><span class="sxs-lookup"><span data-stu-id="4f627-189">To test Identity, add [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute):</span></span>

[!code-csharp[](identity/sample/WebApp3/Pages/Privacy.cshtml.cs?highlight=7)]

<span data-ttu-id="4f627-190">Wenn Sie angemeldet sind, melden Sie sich ab. Führen Sie die APP aus, und wählen Sie den **Datenschutz** Link.</span><span class="sxs-lookup"><span data-stu-id="4f627-190">If you are signed in, sign out. Run the app and select the **Privacy** link.</span></span> <span data-ttu-id="4f627-191">Sie werden zur Anmeldeseite umgeleitet.</span><span class="sxs-lookup"><span data-stu-id="4f627-191">You are redirected to the login page.</span></span>

### <a name="explore-identity"></a><span data-ttu-id="4f627-192">EntdeckenIdentity</span><span class="sxs-lookup"><span data-stu-id="4f627-192">Explore Identity</span></span>

<span data-ttu-id="4f627-193">Weitere Informationen finden Sie unter Identity :</span><span class="sxs-lookup"><span data-stu-id="4f627-193">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="4f627-194">Benutzeroberflächen Quelle für vollständige Identität erstellen</span><span class="sxs-lookup"><span data-stu-id="4f627-194">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="4f627-195">Überprüfen Sie die Quelle jeder Seite, und schrittweise durch den Debugger.</span><span class="sxs-lookup"><span data-stu-id="4f627-195">Examine the source of each page and step through the debugger.</span></span>

## <a name="identity-components"></a>Identity<span data-ttu-id="4f627-196">Komponenten</span><span class="sxs-lookup"><span data-stu-id="4f627-196"> Components</span></span>

<span data-ttu-id="4f627-197">Alle Identity -abhängigen nuget-Pakete sind im ASP.net Core frei [gegebenen Framework](xref:aspnetcore-3.0#use-the-aspnet-core-shared-framework)enthalten.</span><span class="sxs-lookup"><span data-stu-id="4f627-197">All the Identity-dependent NuGet packages are included in the [ASP.NET Core shared framework](xref:aspnetcore-3.0#use-the-aspnet-core-shared-framework).</span></span>

<span data-ttu-id="4f627-198">Das primäre Paket für Identity ist [Microsoft. aspnetcore. Identity ](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/)</span><span class="sxs-lookup"><span data-stu-id="4f627-198">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="4f627-199">Dieses Paket enthält den Kernsatz von Schnittstellen für ASP.net Core Identity und ist in enthalten `Microsoft.AspNetCore.Identity.EntityFrameworkCore` .</span><span class="sxs-lookup"><span data-stu-id="4f627-199">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="4f627-200">Migrieren zu ASP.net CoreIdentity</span><span class="sxs-lookup"><span data-stu-id="4f627-200">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="4f627-201">Weitere Informationen und Anleitungen zum Migrieren Ihres vorhandenen Identity Stores finden Sie unter [Migrieren Identity der Authentifizierung und ](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="4f627-201">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="4f627-202">Festlegen der Kenn Wort Stärke</span><span class="sxs-lookup"><span data-stu-id="4f627-202">Setting password strength</span></span>

<span data-ttu-id="4f627-203">Ein Beispiel zum Festlegen der Mindestanforderungen für das Kennwort finden Sie unter [Konfiguration](#pw) .</span><span class="sxs-lookup"><span data-stu-id="4f627-203">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="adddefaultidentity-and-addidentity"></a><span data-ttu-id="4f627-204">Adddefault Identity und AddIdentity</span><span class="sxs-lookup"><span data-stu-id="4f627-204">AddDefaultIdentity and AddIdentity</span></span>

<span data-ttu-id="4f627-205"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*>wurde in ASP.net Core 2,1 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="4f627-205"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> was introduced in ASP.NET Core 2.1.</span></span> <span data-ttu-id="4f627-206">`AddDefaultIdentity`Das Aufrufen von ähnelt dem Aufrufen der folgenden:</span><span class="sxs-lookup"><span data-stu-id="4f627-206">Calling `AddDefaultIdentity` is similar to calling the following:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentity*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderExtensions.AddDefaultTokenProviders*>

<span data-ttu-id="4f627-207">Weitere Informationen finden Sie unter [adddefault Identity Source](https://github.com/dotnet/AspNetCore/blob/release/3.1/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) .</span><span class="sxs-lookup"><span data-stu-id="4f627-207">See [AddDefaultIdentity source](https://github.com/dotnet/AspNetCore/blob/release/3.1/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) for more information.</span></span>

## <a name="prevent-publish-of-static-identity-assets"></a><span data-ttu-id="4f627-208">Veröffentlichen statischer Identity Assets verhindern</span><span class="sxs-lookup"><span data-stu-id="4f627-208">Prevent publish of static Identity assets</span></span>

<span data-ttu-id="4f627-209">Identity Identity Fügen Sie die folgende `ResolveStaticWebAssetsInputsDependsOn` Eigenschaft und das `RemoveIdentityAssets` Ziel der Projektdatei der APP hinzu, um zu verhindern, dass statische Assets (Stylesheets und JavaScript-Dateien für die Benutzeroberfläche) im Webstamm Verzeichnis veröffentlicht werden:</span><span class="sxs-lookup"><span data-stu-id="4f627-209">To prevent publishing static Identity assets (stylesheets and JavaScript files for Identity UI) to the web root, add the following `ResolveStaticWebAssetsInputsDependsOn` property and `RemoveIdentityAssets` target to the app's project file:</span></span>

```xml
<PropertyGroup>
  <ResolveStaticWebAssetsInputsDependsOn>RemoveIdentityAssets</ResolveStaticWebAssetsInputsDependsOn>
</PropertyGroup>

<Target Name="RemoveIdentityAssets">
  <ItemGroup>
    <StaticWebAsset Remove="@(StaticWebAsset)" Condition="%(SourceId) == 'Microsoft.AspNetCore.Identity.UI'" />
  </ItemGroup>
</Target>
```

<a name="next"></a>

## <a name="next-steps"></a><span data-ttu-id="4f627-210">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="4f627-210">Next Steps</span></span>

* <span data-ttu-id="4f627-211">[ASP.net Core Identity Quellcode](https://github.com/dotnet/aspnetcore/tree/master/src/Identity)</span><span class="sxs-lookup"><span data-stu-id="4f627-211">[ASP.NET Core Identity source code](https://github.com/dotnet/aspnetcore/tree/master/src/Identity)</span></span>
* <span data-ttu-id="4f627-212">Weitere Informationen zum Konfigurieren von mithilfe von SQLite finden Sie in [diesem GitHub-Problem](https://github.com/dotnet/AspNetCore.Docs/issues/5131) Identity .</span><span class="sxs-lookup"><span data-stu-id="4f627-212">See [this GitHub issue](https://github.com/dotnet/AspNetCore.Docs/issues/5131) for information on configuring Identity using SQLite.</span></span>
* <span data-ttu-id="4f627-213">[Konfigurieren von Identity](xref:security/authentication/identity-configuration)</span><span class="sxs-lookup"><span data-stu-id="4f627-213">[Configure Identity](xref:security/authentication/identity-configuration)</span></span>
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="4f627-214">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="4f627-214">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="4f627-215">ASP.net Core Identity ist ein Mitgliedschaftssystem, das ASP.net Core-apps Anmelde Funktionalität hinzufügt.</span><span class="sxs-lookup"><span data-stu-id="4f627-215">ASP.NET Core Identity is a membership system that adds login functionality to ASP.NET Core apps.</span></span> <span data-ttu-id="4f627-216">Benutzer können ein Konto mit den Anmelde Informationen erstellen, die in gespeichert Identity sind, oder Sie können einen externen Anmelde Anbieter verwenden.</span><span class="sxs-lookup"><span data-stu-id="4f627-216">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="4f627-217">Zu den unterstützten externen Anmelde Anbietern zählen [Facebook, Google, Microsoft-Konto und Twitter](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="4f627-217">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

Identity<span data-ttu-id="4f627-218">kann mithilfe einer SQL Server-Datenbank zum Speichern von Benutzernamen, Kenn Wörtern und Profildaten konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="4f627-218"> can be configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="4f627-219">Alternativ können Sie auch einen weiteren permanenten Speicher verwenden, z. b. Azure Table Storage.</span><span class="sxs-lookup"><span data-stu-id="4f627-219">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

<span data-ttu-id="4f627-220">[Anzeigen oder Herunterladen des Beispielcodes](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([herunterladen)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="4f627-220">[View or download the sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([how to download)](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="4f627-221">In diesem Thema erfahren Sie, wie Sie Identity einen Benutzer mit registrieren, anmelden und abmelden.</span><span class="sxs-lookup"><span data-stu-id="4f627-221">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="4f627-222">Ausführlichere Anweisungen zum Erstellen von apps, die verwenden Identity , finden Sie im Abschnitt nächste Schritte am Ende dieses Artikels.</span><span class="sxs-lookup"><span data-stu-id="4f627-222">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

<a name="adi"></a>

## <a name="adddefaultidentity-and-addidentity"></a><span data-ttu-id="4f627-223">Adddefault Identity und AddIdentity</span><span class="sxs-lookup"><span data-stu-id="4f627-223">AddDefaultIdentity and AddIdentity</span></span>

<span data-ttu-id="4f627-224"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*>wurde in ASP.net Core 2,1 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="4f627-224"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> was introduced in ASP.NET Core 2.1.</span></span> <span data-ttu-id="4f627-225">`AddDefaultIdentity`Das Aufrufen von ähnelt dem Aufrufen der folgenden:</span><span class="sxs-lookup"><span data-stu-id="4f627-225">Calling `AddDefaultIdentity` is similar to calling the following:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentity*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderExtensions.AddDefaultTokenProviders*>

<span data-ttu-id="4f627-226">Weitere Informationen finden Sie unter [adddefault Identity Source](https://github.com/dotnet/AspNetCore/blob/release/2.1/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) .</span><span class="sxs-lookup"><span data-stu-id="4f627-226">See [AddDefaultIdentity source](https://github.com/dotnet/AspNetCore/blob/release/2.1/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) for more information.</span></span>

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="4f627-227">Erstellen einer Web-App mit Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="4f627-227">Create a Web app with authentication</span></span>

<span data-ttu-id="4f627-228">Erstellen Sie ein ASP.net Core Webanwendungs Projekt mit einzelnen Benutzerkonten.</span><span class="sxs-lookup"><span data-stu-id="4f627-228">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="4f627-229">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4f627-229">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="4f627-230">Wählen Sie **Datei** > **neu** > **Projekt**aus.</span><span class="sxs-lookup"><span data-stu-id="4f627-230">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="4f627-231">Wählen Sie **ASP.NET Core-Webanwendung** aus.</span><span class="sxs-lookup"><span data-stu-id="4f627-231">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="4f627-232">Nennen Sie das Projekt **"WebApp1"** , um den gleichen Namespace wie das Projekt herunterzuladen.</span><span class="sxs-lookup"><span data-stu-id="4f627-232">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="4f627-233">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="4f627-233">Click **OK**.</span></span>
* <span data-ttu-id="4f627-234">Wählen Sie eine ASP.net Core **Webanwendung**, und wählen Sie dann **Authentifizierung ändern**aus.</span><span class="sxs-lookup"><span data-stu-id="4f627-234">Select an ASP.NET Core **Web Application**, then select **Change Authentication**.</span></span>
* <span data-ttu-id="4f627-235">Wählen Sie **einzelne Benutzerkonten** , und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="4f627-235">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="4f627-236">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="4f627-236">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new webapp --auth Individual -o WebApp1
```

---

<span data-ttu-id="4f627-237">Das generierte Projekt stellt [ASP.net Core Identity ](xref:security/authentication/identity) als [ Razor Klassenbibliothek](xref:razor-pages/ui-class)bereit.</span><span class="sxs-lookup"><span data-stu-id="4f627-237">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="4f627-238">Die Identity Razor Klassenbibliothek macht Endpunkte mit dem `Identity` Bereich verfügbar.</span><span class="sxs-lookup"><span data-stu-id="4f627-238">The Identity Razor Class Library exposes endpoints with the `Identity` area.</span></span> <span data-ttu-id="4f627-239">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="4f627-239">For example:</span></span>

* <span data-ttu-id="4f627-240">/Identity/Account/Login</span><span class="sxs-lookup"><span data-stu-id="4f627-240">/Identity/Account/Login</span></span>
* <span data-ttu-id="4f627-241">/Identity/Account/Logout</span><span class="sxs-lookup"><span data-stu-id="4f627-241">/Identity/Account/Logout</span></span>
* <span data-ttu-id="4f627-242">/Identity/Account/Manage</span><span class="sxs-lookup"><span data-stu-id="4f627-242">/Identity/Account/Manage</span></span>

### <a name="apply-migrations"></a><span data-ttu-id="4f627-243">Anwenden von Migrationen</span><span class="sxs-lookup"><span data-stu-id="4f627-243">Apply migrations</span></span>

<span data-ttu-id="4f627-244">Wenden Sie die Migrationen an, um die Datenbank zu initialisieren.</span><span class="sxs-lookup"><span data-stu-id="4f627-244">Apply the migrations to initialize the database.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="4f627-245">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4f627-245">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="4f627-246">Führen Sie den folgenden Befehl in der Paket-Manager-Konsole (PMC) aus:</span><span class="sxs-lookup"><span data-stu-id="4f627-246">Run the following command in the Package Manager Console (PMC):</span></span>

```powershell
Update-Database
```

# <a name="net-core-cli"></a>[<span data-ttu-id="4f627-247">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="4f627-247">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet ef database update
```

---

### <a name="test-register-and-login"></a><span data-ttu-id="4f627-248">Test Registrierung und Anmeldung</span><span class="sxs-lookup"><span data-stu-id="4f627-248">Test Register and Login</span></span>

<span data-ttu-id="4f627-249">Führen Sie die APP aus, und registrieren Sie einen Benutzer.</span><span class="sxs-lookup"><span data-stu-id="4f627-249">Run the app and register a user.</span></span> <span data-ttu-id="4f627-250">Abhängig von der Bildschirmgröße müssen Sie ggf. die Navigations Schaltfläche auswählen, um die Links **registrieren** und **Anmelden** anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="4f627-250">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>

### <a name="configure-identity-services"></a><span data-ttu-id="4f627-251">IdentityDienste konfigurieren</span><span class="sxs-lookup"><span data-stu-id="4f627-251">Configure Identity services</span></span>

<span data-ttu-id="4f627-252">Dienste werden in hinzugefügt `ConfigureServices` .</span><span class="sxs-lookup"><span data-stu-id="4f627-252">Services are added in `ConfigureServices`.</span></span> <span data-ttu-id="4f627-253">Das typische Muster besteht darin, alle `Add{Service}`-Methoden und dann alle `services.Configure{Service}`-Methoden aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="4f627-253">The typical pattern is to call all the `Add{Service}` methods, and then call all the `services.Configure{Service}` methods.</span></span>

[!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configureservices)]

<span data-ttu-id="4f627-254">Der vorangehende Code konfiguriert Identity mit Standard Options Werten.</span><span class="sxs-lookup"><span data-stu-id="4f627-254">The preceding code configures Identity with default option values.</span></span> <span data-ttu-id="4f627-255">Dienste werden über die [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection)der App zur Verfügung gestellt.</span><span class="sxs-lookup"><span data-stu-id="4f627-255">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

Identity<span data-ttu-id="4f627-256">wird durch Aufrufen von [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_)aktiviert.</span><span class="sxs-lookup"><span data-stu-id="4f627-256"> is enabled by calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span> <span data-ttu-id="4f627-257">`UseAuthentication`Fügt der Anforderungs Pipeline Authentifizierungs [Middleware](xref:fundamentals/middleware/index) hinzu.</span><span class="sxs-lookup"><span data-stu-id="4f627-257">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

[!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configure&highlight=18)]

<span data-ttu-id="4f627-258">Weitere Informationen finden Sie unter der [ Identity options-Klasse](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) und dem [Anwendungsstart](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="4f627-258">For more information, see the [IdentityOptions Class](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="4f627-259">Gerüst: registrieren, anmelden und Abmelden</span><span class="sxs-lookup"><span data-stu-id="4f627-259">Scaffold Register, Login, and LogOut</span></span>

<span data-ttu-id="4f627-260">Befolgen Sie die [Gerüst Identität in einem Razor Projekt mit Autorisierungs](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) Anweisungen, um den in diesem Abschnitt gezeigten Code zu generieren.</span><span class="sxs-lookup"><span data-stu-id="4f627-260">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions to generate the code shown in this section.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="4f627-261">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4f627-261">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="4f627-262">Fügen Sie die Registrierungs-, Anmelde-und Abmelde Dateien hinzu.</span><span class="sxs-lookup"><span data-stu-id="4f627-262">Add the Register, Login, and LogOut files.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="4f627-263">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="4f627-263">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="4f627-264">Wenn Sie das Projekt mit dem Namen **"WebApp1"** erstellt haben, führen Sie die folgenden Befehle aus.</span><span class="sxs-lookup"><span data-stu-id="4f627-264">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="4f627-265">Verwenden Sie andernfalls den korrekten Namespace für `ApplicationDbContext` :</span><span class="sxs-lookup"><span data-stu-id="4f627-265">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="4f627-266">PowerShell verwendet ein Semikolon als Befehls Trennzeichen.</span><span class="sxs-lookup"><span data-stu-id="4f627-266">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="4f627-267">Wenn Sie PowerShell verwenden, führen Sie einen Escapezeichen in der Datei Liste aus, oder fügen Sie die Datei Liste in doppelte Anführungszeichen ein, wie im vorherigen Beispiel gezeigt.</span><span class="sxs-lookup"><span data-stu-id="4f627-267">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="4f627-268">Register überprüfen</span><span class="sxs-lookup"><span data-stu-id="4f627-268">Examine Register</span></span>

<span data-ttu-id="4f627-269">Wenn ein Benutzer auf den Link **registrieren** klickt, `RegisterModel.OnPostAsync` wird die Aktion aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="4f627-269">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="4f627-270">Der Benutzer wird von " [kreateasync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) " für das `_userManager` Objekt erstellt:</span><span class="sxs-lookup"><span data-stu-id="4f627-270">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object:</span></span>

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=7)]

<span data-ttu-id="4f627-271">Wenn der Benutzer erfolgreich erstellt wurde, wird der Benutzer durch den-Befehl angemeldet `_signInManager.SignInAsync` .</span><span class="sxs-lookup"><span data-stu-id="4f627-271">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

<span data-ttu-id="4f627-272">**Hinweis:** Unter [Konto Bestätigung](xref:security/authentication/accconfirm#prevent-login-at-registration) finden Sie Schritte zum Verhindern der sofortigen Anmeldung bei der Registrierung.</span><span class="sxs-lookup"><span data-stu-id="4f627-272">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="4f627-273">Anmelden</span><span class="sxs-lookup"><span data-stu-id="4f627-273">Log in</span></span>

<span data-ttu-id="4f627-274">Das Anmeldeformular wird angezeigt, wenn Folgendes gilt:</span><span class="sxs-lookup"><span data-stu-id="4f627-274">The Login form is displayed when:</span></span>

* <span data-ttu-id="4f627-275">Der Link " **Anmelden** " ist ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="4f627-275">The **Log in** link is selected.</span></span>
* <span data-ttu-id="4f627-276">Ein Benutzer versucht, auf eine eingeschränkte Seite zuzugreifen, für die er nicht autorisiert ist, **oder** wenn er nicht vom System authentifiziert wurde.</span><span class="sxs-lookup"><span data-stu-id="4f627-276">A user attempts to access a restricted page that they aren't authorized to access **or** when they haven't been authenticated by the system.</span></span>

<span data-ttu-id="4f627-277">Wenn das Formular auf der Anmeldeseite übermittelt wird, `OnPostAsync` wird die Aktion aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="4f627-277">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="4f627-278">`PasswordSignInAsync`wird für das- `_signInManager` Objekt aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="4f627-278">`PasswordSignInAsync` is called on the `_signInManager` object.</span></span>

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=10-11)]

<span data-ttu-id="4f627-279">Informationen dazu, wie Sie Autorisierungs Entscheidungen treffen, finden Sie unter <xref:security/authorization/introduction> .</span><span class="sxs-lookup"><span data-stu-id="4f627-279">For information on how to make authorization decisions, see <xref:security/authorization/introduction>.</span></span>

### <a name="log-out"></a><span data-ttu-id="4f627-280">Abmelden</span><span class="sxs-lookup"><span data-stu-id="4f627-280">Log out</span></span>

<span data-ttu-id="4f627-281">Der Link " **Abmelden** " Ruft die `LogoutModel.OnPost` Aktion auf.</span><span class="sxs-lookup"><span data-stu-id="4f627-281">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Logout.cshtml.cs)]

<span data-ttu-id="4f627-282">[Signoutasync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) löscht die in einem Cookie gespeicherten Ansprüche des Benutzers.</span><span class="sxs-lookup"><span data-stu-id="4f627-282">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span>

<span data-ttu-id="4f627-283">Post wird in der Datei *pages/Shared/_LoginPartial. cshtml*angegeben:</span><span class="sxs-lookup"><span data-stu-id="4f627-283">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-cshtml[](identity/sample/WebApp1/Pages/Shared/_LoginPartial.cshtml?highlight=16)]

## <a name="test-identity"></a><span data-ttu-id="4f627-284">TestIdentity</span><span class="sxs-lookup"><span data-stu-id="4f627-284">Test Identity</span></span>

<span data-ttu-id="4f627-285">Die Standardweb Projektvorlagen ermöglichen den anonymen Zugriff auf die Startseiten.</span><span class="sxs-lookup"><span data-stu-id="4f627-285">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="4f627-286">IdentityFügen Sie zum Testen [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) der Seite Datenschutz hinzu.</span><span class="sxs-lookup"><span data-stu-id="4f627-286">To test Identity, add [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) to the Privacy page.</span></span>

[!code-csharp[](identity/sample/WebApp1/Pages/Privacy.cshtml.cs?highlight=7)]

<span data-ttu-id="4f627-287">Wenn Sie angemeldet sind, melden Sie sich ab. Führen Sie die APP aus, und wählen Sie den **Datenschutz** Link.</span><span class="sxs-lookup"><span data-stu-id="4f627-287">If you are signed in, sign out. Run the app and select the **Privacy** link.</span></span> <span data-ttu-id="4f627-288">Sie werden zur Anmeldeseite umgeleitet.</span><span class="sxs-lookup"><span data-stu-id="4f627-288">You are redirected to the login page.</span></span>

### <a name="explore-identity"></a><span data-ttu-id="4f627-289">EntdeckenIdentity</span><span class="sxs-lookup"><span data-stu-id="4f627-289">Explore Identity</span></span>

<span data-ttu-id="4f627-290">Weitere Informationen finden Sie unter Identity :</span><span class="sxs-lookup"><span data-stu-id="4f627-290">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="4f627-291">Benutzeroberflächen Quelle für vollständige Identität erstellen</span><span class="sxs-lookup"><span data-stu-id="4f627-291">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="4f627-292">Überprüfen Sie die Quelle jeder Seite, und schrittweise durch den Debugger.</span><span class="sxs-lookup"><span data-stu-id="4f627-292">Examine the source of each page and step through the debugger.</span></span>

## <a name="identity-components"></a>Identity<span data-ttu-id="4f627-293">Komponenten</span><span class="sxs-lookup"><span data-stu-id="4f627-293"> Components</span></span>

<span data-ttu-id="4f627-294">Alle Identity abhängigen nuget-Pakete sind im [Metapaket Microsoft. aspnetcore. app](xref:fundamentals/metapackage-app)enthalten.</span><span class="sxs-lookup"><span data-stu-id="4f627-294">All the Identity dependent NuGet packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="4f627-295">Das primäre Paket für Identity ist [Microsoft. aspnetcore. Identity ](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/)</span><span class="sxs-lookup"><span data-stu-id="4f627-295">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="4f627-296">Dieses Paket enthält den Kernsatz von Schnittstellen für ASP.net Core Identity und ist in enthalten `Microsoft.AspNetCore.Identity.EntityFrameworkCore` .</span><span class="sxs-lookup"><span data-stu-id="4f627-296">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="4f627-297">Migrieren zu ASP.net CoreIdentity</span><span class="sxs-lookup"><span data-stu-id="4f627-297">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="4f627-298">Weitere Informationen und Anleitungen zum Migrieren Ihres vorhandenen Identity Stores finden Sie unter [Migrieren Identity der Authentifizierung und ](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="4f627-298">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="4f627-299">Festlegen der Kenn Wort Stärke</span><span class="sxs-lookup"><span data-stu-id="4f627-299">Setting password strength</span></span>

<span data-ttu-id="4f627-300">Ein Beispiel zum Festlegen der Mindestanforderungen für das Kennwort finden Sie unter [Konfiguration](#pw) .</span><span class="sxs-lookup"><span data-stu-id="4f627-300">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4f627-301">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="4f627-301">Next Steps</span></span>

* <span data-ttu-id="4f627-302">Weitere Informationen zum Konfigurieren von mithilfe von SQLite finden Sie in [diesem GitHub-Problem](https://github.com/dotnet/AspNetCore.Docs/issues/5131) Identity .</span><span class="sxs-lookup"><span data-stu-id="4f627-302">See [this GitHub issue](https://github.com/dotnet/AspNetCore.Docs/issues/5131) for information on configuring Identity using SQLite.</span></span>
* <span data-ttu-id="4f627-303">[Konfigurieren von Identity](xref:security/authentication/identity-configuration)</span><span class="sxs-lookup"><span data-stu-id="4f627-303">[Configure Identity](xref:security/authentication/identity-configuration)</span></span>
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>

::: moniker-end
