---
title: Erstellen einer Blazor-App mit einer Aufgabenliste
author: guardrex
description: Erstellen Sie Schritt für Schritt eine Blazor-App.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: tutorials/build-a-blazor-app
ms.openlocfilehash: 726380c42c952f47d6fdff09a811f35a20462d96
ms.sourcegitcommit: 66fca14611eba141d455fe0bd2c37803062e439c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2020
ms.locfileid: "85944918"
---
# <a name="build-a-blazor-todo-list-app"></a><span data-ttu-id="67f62-103">Erstellen einer Blazor-App mit einer Aufgabenliste</span><span class="sxs-lookup"><span data-stu-id="67f62-103">Build a Blazor todo list app</span></span>

<span data-ttu-id="67f62-104">Von [Daniel Roth](https://github.com/danroth27) und [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="67f62-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="67f62-105">In diesem Tutorial erfahren Sie, wie Sie eine Blazor-App erstellen und ändern.</span><span class="sxs-lookup"><span data-stu-id="67f62-105">This tutorial shows you how to build and modify a Blazor app.</span></span> <span data-ttu-id="67f62-106">Sie lernen Folgendes:</span><span class="sxs-lookup"><span data-stu-id="67f62-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="67f62-107">Erstellen eines Aufgabenlisten-Blazor-App-Projekts</span><span class="sxs-lookup"><span data-stu-id="67f62-107">Create a todo list Blazor app project</span></span>
> * <span data-ttu-id="67f62-108">Ändern von Razor-Komponenten</span><span class="sxs-lookup"><span data-stu-id="67f62-108">Modify Razor components</span></span>
> * <span data-ttu-id="67f62-109">Verwenden von Ereignisbehandlung und Datenbindung in Komponenten</span><span class="sxs-lookup"><span data-stu-id="67f62-109">Use event handling and data binding in components</span></span>
> * <span data-ttu-id="67f62-110">Verwenden von Abhängigkeitsinjektion (DI, Dependency Injection) und Routing in einer Blazor-App</span><span class="sxs-lookup"><span data-stu-id="67f62-110">Use dependency injection (DI) and routing in a Blazor app</span></span>

<span data-ttu-id="67f62-111">Am Ende dieses Tutorials verfügen Sie über eine funktionierende Aufgabenlisten-App.</span><span class="sxs-lookup"><span data-stu-id="67f62-111">At the end of this tutorial, you'll have a working todo list app.</span></span>

1. <span data-ttu-id="67f62-112">Erstellen Sie in einer Befehlsshell eine neue Blazor-App namens `TodoList`:</span><span class="sxs-lookup"><span data-stu-id="67f62-112">Create a new Blazor app named `TodoList` in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o TodoList
   ```

   <span data-ttu-id="67f62-113">Der oben stehende Befehl erstellt einen Ordner namens `TodoList` zum Speichern der App.</span><span class="sxs-lookup"><span data-stu-id="67f62-113">The preceding command creates a folder named `TodoList` to hold the app.</span></span> <span data-ttu-id="67f62-114">Ändern Sie das Verzeichnis mit dem folgenden Befehl in den Ordner `TodoList`:</span><span class="sxs-lookup"><span data-stu-id="67f62-114">Change directories to the `TodoList` folder with the following command:</span></span>

   ```dotnetcli
   cd TodoList
   ```

1. <span data-ttu-id="67f62-115">Fügen Sie mit dem folgenden Befehl der App im Ordner `Pages` eine neue `Todo` Razor-Komponente hinzu:</span><span class="sxs-lookup"><span data-stu-id="67f62-115">Add a new `Todo` Razor component to the app in the `Pages` folder using the following command:</span></span>

   ```dotnetcli
   dotnet new razorcomponent -n Todo -o Pages
   ```

   > [!IMPORTANT]
   > <span data-ttu-id="67f62-116">Bei Razor-Komponenten-Dateinamen muss der erste Buchstabe groß geschrieben werden. Vergewissern Sie sich daher, dass der `Todo`-Komponenten-Dateiname mit dem Großbuchstaben `T` beginnt.</span><span class="sxs-lookup"><span data-stu-id="67f62-116">Razor component file names require a capitalized first letter, so confirm that the `Todo` component file name starts with a capital letter `T`.</span></span>

1. <span data-ttu-id="67f62-117">Geben Sie in `Pages/Todo.razor` das ursprüngliche Markup für die Komponente an:</span><span class="sxs-lookup"><span data-stu-id="67f62-117">In `Pages/Todo.razor` provide the initial markup for the component:</span></span>

   ```razor
   @page "/todo"

   <h3>Todo</h3>
   ```

1. <span data-ttu-id="67f62-118">Fügen Sie der Navigationsleiste die `Todo`-Komponente hinzu.</span><span class="sxs-lookup"><span data-stu-id="67f62-118">Add the `Todo` component to the navigation bar.</span></span>

   <span data-ttu-id="67f62-119">Die Komponente `NavMenu` (`Shared/NavMenu.razor`) wird im Layout der App verwendet.</span><span class="sxs-lookup"><span data-stu-id="67f62-119">The `NavMenu` component (`Shared/NavMenu.razor`) is used in the app's layout.</span></span> <span data-ttu-id="67f62-120">Layouts sind Komponenten, mit denen Sie die Duplizierung des Inhalts in der App vermeiden können.</span><span class="sxs-lookup"><span data-stu-id="67f62-120">Layouts are components that allow you to avoid duplication of content in the app.</span></span>

   <span data-ttu-id="67f62-121">Fügen Sie ein `<NavLink>`-Element für die `Todo`-Komponente hinzu, indem Sie das folgende Listenelementmarkup unterhalb der vorhandenen Listenelemente in der `Shared/NavMenu.razor`-Datei hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="67f62-121">Add a `<NavLink>` element for the `Todo` component by adding the following list item markup below the existing list items in the `Shared/NavMenu.razor` file:</span></span>

   ```razor
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. <span data-ttu-id="67f62-122">Erstellen Sie die App neu, und führen Sie sie aus.</span><span class="sxs-lookup"><span data-stu-id="67f62-122">Rebuild and run the app.</span></span> <span data-ttu-id="67f62-123">Besuchen Sie die neue Todo-Seite, um sicherzustellen, dass der Link zur `Todo`-Komponente funktioniert.</span><span class="sxs-lookup"><span data-stu-id="67f62-123">Visit the new Todo page to confirm that the link to the `Todo` component works.</span></span>

1. <span data-ttu-id="67f62-124">Fügen Sie dem Stamm des Projekts eine `TodoItem.cs`-Datei hinzu, die eine Klasse zum Darstellen eines Aufgabenelements enthalten soll.</span><span class="sxs-lookup"><span data-stu-id="67f62-124">Add a `TodoItem.cs` file to the root of the project to hold a class that represents a todo item.</span></span> <span data-ttu-id="67f62-125">Verwenden Sie den folgenden C#-Code für die `TodoItem`-Klasse:</span><span class="sxs-lookup"><span data-stu-id="67f62-125">Use the following C# code for the `TodoItem` class:</span></span>

   [!code-csharp[](build-a-blazor-app/samples_snapshot/3.x/TodoItem.cs)]

1. <span data-ttu-id="67f62-126">Kehren Sie zur `Todo`-Komponente (`Pages/Todo.razor`) zurück:</span><span class="sxs-lookup"><span data-stu-id="67f62-126">Return to the `Todo` component (`Pages/Todo.razor`):</span></span>

   * <span data-ttu-id="67f62-127">Fügen Sie ein Feld für die Aufgabenelemente in einem `@code`-Block hinzu.</span><span class="sxs-lookup"><span data-stu-id="67f62-127">Add a field for the todo items in an `@code` block.</span></span> <span data-ttu-id="67f62-128">Die `Todo`-Komponente verwaltet mit diesem Feld den Status der Aufgabenliste.</span><span class="sxs-lookup"><span data-stu-id="67f62-128">The `Todo` component uses this field to maintain the state of the todo list.</span></span>
   * <span data-ttu-id="67f62-129">Fügen Sie ein Markup für eine unsortierte Liste und eine `foreach`-Schleife hinzu, um jedes Aufgabenelement als Listenelement zu rendern (`<li>`).</span><span class="sxs-lookup"><span data-stu-id="67f62-129">Add unordered list markup and a `foreach` loop to render each todo item as a list item (`<li>`).</span></span>

   [!code-razor[](build-a-blazor-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. <span data-ttu-id="67f62-130">Die App benötigt Benutzeroberflächenelemente, damit der Liste Aufgabenelemente hinzugefügt werden können.</span><span class="sxs-lookup"><span data-stu-id="67f62-130">The app requires UI elements for adding todo items to the list.</span></span> <span data-ttu-id="67f62-131">Fügen Sie eine Texteingabe (`<input>`) und eine Schaltfläche (`<button>`) unterhalb der Liste (`<ul>...</ul>`) hinzu:</span><span class="sxs-lookup"><span data-stu-id="67f62-131">Add a text input (`<input>`) and a button (`<button>`) below the unordered list (`<ul>...</ul>`):</span></span>

   [!code-razor[](build-a-blazor-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. <span data-ttu-id="67f62-132">Erstellen Sie die App neu, und führen Sie sie aus.</span><span class="sxs-lookup"><span data-stu-id="67f62-132">Rebuild and run the app.</span></span> <span data-ttu-id="67f62-133">Bei Auswahl der Schaltfläche **`Add todo`** geschieht nichts, da kein Ereignishandler mit der Schaltfläche verknüpft ist.</span><span class="sxs-lookup"><span data-stu-id="67f62-133">When the **`Add todo`** button is selected, nothing happens because an event handler isn't wired up to the button.</span></span>

1. <span data-ttu-id="67f62-134">Fügen Sie eine `AddTodo`-Methode zur `Todo`-Komponente hinzu, und registrieren Sie sie mit dem `@onclick`-Attribut für eine Schaltflächenauswahl.</span><span class="sxs-lookup"><span data-stu-id="67f62-134">Add an `AddTodo` method to the `Todo` component and register it for button selections using the `@onclick` attribute.</span></span> <span data-ttu-id="67f62-135">Die C#-Methode `AddTodo` wird aufgerufen, wenn die Schaltfläche ausgewählt wird:</span><span class="sxs-lookup"><span data-stu-id="67f62-135">The `AddTodo` C# method is called when the button is selected:</span></span>

   [!code-razor[](build-a-blazor-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

1. <span data-ttu-id="67f62-136">Um den Titel des neuen Aufgabenelements abzurufen, fügen Sie oben im `@code`-Block ein `newTodo`-Zeichenfolgenfeld hinzu, und binden Sie es mithilfe des `bind`-Attributs im `<input>`-Element an den Wert der Texteingabe:</span><span class="sxs-lookup"><span data-stu-id="67f62-136">To get the title of the new todo item, add a `newTodo` string field at the top of the `@code` block and bind it to the value of the text input using the `bind` attribute in the `<input>` element:</span></span>

   [!code-razor[](build-a-blazor-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```razor
   <input placeholder="Something todo" @bind="newTodo" />
   ```

1. <span data-ttu-id="67f62-137">Aktualisieren Sie die `AddTodo`-Methode, um das `TodoItem` mit dem angegebenen Titel der Liste hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="67f62-137">Update the `AddTodo` method to add the `TodoItem` with the specified title to the list.</span></span> <span data-ttu-id="67f62-138">Löschen Sie den Wert der Texteingabe, indem Sie für `newTodo` eine leere Zeichenfolge festlegen:</span><span class="sxs-lookup"><span data-stu-id="67f62-138">Clear the value of the text input by setting `newTodo` to an empty string:</span></span>

   [!code-razor[](build-a-blazor-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. <span data-ttu-id="67f62-139">Erstellen Sie die App neu, und führen Sie sie aus.</span><span class="sxs-lookup"><span data-stu-id="67f62-139">Rebuild and run the app.</span></span> <span data-ttu-id="67f62-140">Fügen Sie der Aufgabenliste einige Aufgabenelemente hinzu, um den neuen Code zu testen.</span><span class="sxs-lookup"><span data-stu-id="67f62-140">Add some todo items to the todo list to test the new code.</span></span>

1. <span data-ttu-id="67f62-141">Der Titeltext für die einzelnen Aufgabenelemente kann als bearbeitbar festgelegt werden, und ein Kontrollkästchen kann dem Benutzer helfen, die erledigten Elemente nachzuverfolgen.</span><span class="sxs-lookup"><span data-stu-id="67f62-141">The title text for each todo item can be made editable, and a check box can help the user keep track of completed items.</span></span> <span data-ttu-id="67f62-142">Fügen Sie für jedes Aufgabenelement eine Kontrollkästcheneingabe hinzu, und binden Sie ihren Wert an die `IsDone`-Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="67f62-142">Add a check box input for each todo item and bind its value to the `IsDone` property.</span></span> <span data-ttu-id="67f62-143">Ändern Sie `@todo.Title` in ein `<input>`-Element, das an `@todo.Title` gebunden ist:</span><span class="sxs-lookup"><span data-stu-id="67f62-143">Change `@todo.Title` to an `<input>` element bound to `@todo.Title`:</span></span>

   [!code-razor[](build-a-blazor-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. <span data-ttu-id="67f62-144">Um sicherzustellen, dass diese Werte gebunden sind, aktualisieren Sie den `<h3>`-Header, um die Anzahl der noch nicht erledigten Aufgabenelemente anzuzeigen (`IsDone` ist `false`).</span><span class="sxs-lookup"><span data-stu-id="67f62-144">To verify that these values are bound, update the `<h3>` header to show a count of the number of todo items that aren't complete (`IsDone` is `false`).</span></span>

   ```razor
   <h3>Todo (@todos.Count(todo => !todo.IsDone))</h3>
   ```

1. <span data-ttu-id="67f62-145">Die fertige `Todo`-Komponente (`Pages/Todo.razor`):</span><span class="sxs-lookup"><span data-stu-id="67f62-145">The completed `Todo` component (`Pages/Todo.razor`):</span></span>

   [!code-razor[](build-a-blazor-app/samples_snapshot/3.x/Todo.razor)]

1. <span data-ttu-id="67f62-146">Erstellen Sie die App neu, und führen Sie sie aus.</span><span class="sxs-lookup"><span data-stu-id="67f62-146">Rebuild and run the app.</span></span> <span data-ttu-id="67f62-147">Fügen Sie Aufgabenelemente hinzu, um den neuen Code zu testen.</span><span class="sxs-lookup"><span data-stu-id="67f62-147">Add todo items to test the new code.</span></span>

## <a name="next-steps"></a><span data-ttu-id="67f62-148">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="67f62-148">Next steps</span></span>

<span data-ttu-id="67f62-149">In diesem Tutorial haben Sie gelernt, wie die folgenden Aufgaben ausgeführt werden:</span><span class="sxs-lookup"><span data-stu-id="67f62-149">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="67f62-150">Erstellen eines Aufgabenlisten-Blazor-App-Projekts</span><span class="sxs-lookup"><span data-stu-id="67f62-150">Create a todo list Blazor app project</span></span>
> * <span data-ttu-id="67f62-151">Ändern von Razor-Komponenten</span><span class="sxs-lookup"><span data-stu-id="67f62-151">Modify Razor components</span></span>
> * <span data-ttu-id="67f62-152">Verwenden von Ereignisbehandlung und Datenbindung in Komponenten</span><span class="sxs-lookup"><span data-stu-id="67f62-152">Use event handling and data binding in components</span></span>
> * <span data-ttu-id="67f62-153">Verwenden von Abhängigkeitsinjektion (DI, Dependency Injection) und Routing in einer Blazor-App</span><span class="sxs-lookup"><span data-stu-id="67f62-153">Use dependency injection (DI) and routing in a Blazor app</span></span>

<span data-ttu-id="67f62-154">Erfahren Sie mehr über die Blazor-Projektvorlagen:</span><span class="sxs-lookup"><span data-stu-id="67f62-154">Learn about the Blazor project templates:</span></span>

> [!div class="nextstepaction"]
> <xref:blazor/templates>
