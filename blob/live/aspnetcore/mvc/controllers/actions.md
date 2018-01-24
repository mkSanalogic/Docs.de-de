---
title: Behandlung von Anforderungen mit Controller in ASP.NET Core MVC
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 07/03/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/actions
ms.openlocfilehash: cef493fc2010d1c82e5c1dfec85864539252b817
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
---
# <a name="handling-requests-with-controllers-in-aspnet-core-mvc"></a><span data-ttu-id="89724-102">Behandlung von Anforderungen mit Controller in ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="89724-102">Handling requests with controllers in ASP.NET Core MVC</span></span>

<span data-ttu-id="89724-103">Durch [Steve Smith](https://ardalis.com/) und [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="89724-103">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="89724-104">Domänencontroller, Aktionen und Aktionsergebnisse sind grundlegender Bestandteil der Entwickler apps mithilfe von ASP.NET Core MVC Aufbau auf.</span><span class="sxs-lookup"><span data-stu-id="89724-104">Controllers, actions, and action results are a fundamental part of how developers build apps using ASP.NET Core MVC.</span></span>

## <a name="what-is-a-controller"></a><span data-ttu-id="89724-105">Was ist ein Domänencontroller?</span><span class="sxs-lookup"><span data-stu-id="89724-105">What is a Controller?</span></span>

<span data-ttu-id="89724-106">Ein Controller wird verwendet, um zu definieren und eine Reihe von Aktionen zu gruppieren.</span><span class="sxs-lookup"><span data-stu-id="89724-106">A controller is used to define and group a set of actions.</span></span> <span data-ttu-id="89724-107">Eine Aktion (oder *Aktionsmethode*) ist eine Methode auf einem Domänencontroller der Anforderungen verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="89724-107">An action (or *action method*) is a method on a controller which handles requests.</span></span> <span data-ttu-id="89724-108">Logische Gruppierung Controller ähnliche Aktionen zusammen.</span><span class="sxs-lookup"><span data-stu-id="89724-108">Controllers logically group similar actions together.</span></span> <span data-ttu-id="89724-109">Diese Aggregation von Aktionen kann allgemeine Sätze von Regeln, z. B. routing, Zwischenspeichern und Autorisierung, zusammen angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="89724-109">This aggregation of actions allows common sets of rules, such as routing, caching, and authorization, to be applied collectively.</span></span> <span data-ttu-id="89724-110">Anforderungen werden über Aktionen zugeordnet [routing](xref:mvc/controllers/routing).</span><span class="sxs-lookup"><span data-stu-id="89724-110">Requests are mapped to actions through [routing](xref:mvc/controllers/routing).</span></span>

<span data-ttu-id="89724-111">Gemäß der Konvention Controllerklassen:</span><span class="sxs-lookup"><span data-stu-id="89724-111">By convention, controller classes:</span></span>
* <span data-ttu-id="89724-112">Befinden sich in der Stammebene des Projekts *Controller* Ordner</span><span class="sxs-lookup"><span data-stu-id="89724-112">Reside in the project's root-level *Controllers* folder</span></span>
* <span data-ttu-id="89724-113">Erben von`Microsoft.AspNetCore.Mvc.Controller`</span><span class="sxs-lookup"><span data-stu-id="89724-113">Inherit from `Microsoft.AspNetCore.Mvc.Controller`</span></span>

<span data-ttu-id="89724-114">Ein Controller ist eine instanziierbare Klasse, die in der mindestens eine der folgenden Bedingungen "true" ist:</span><span class="sxs-lookup"><span data-stu-id="89724-114">A controller is an instantiable class in which at least one of the following conditions is true:</span></span>
* <span data-ttu-id="89724-115">Der Name der Klasse wird mit "Controller" Formatangabe.</span><span class="sxs-lookup"><span data-stu-id="89724-115">The class name is suffixed with "Controller"</span></span>
* <span data-ttu-id="89724-116">Die Klasse erbt von einer Klasse, deren Name mit "Controller" Formatangabe ist</span><span class="sxs-lookup"><span data-stu-id="89724-116">The class inherits from a class whose name is suffixed with "Controller"</span></span>
* <span data-ttu-id="89724-117">Die Klasse wird mit ergänzt die `[Controller]` Attribut</span><span class="sxs-lookup"><span data-stu-id="89724-117">The class is decorated with the `[Controller]` attribute</span></span>

<span data-ttu-id="89724-118">Eine Controllerklasse dürfen keine zugeordnete `[NonController]` Attribut.</span><span class="sxs-lookup"><span data-stu-id="89724-118">A controller class must not have an associated `[NonController]` attribute.</span></span>

<span data-ttu-id="89724-119">Domänencontroller sollten folgen der [expliziten Abhängigkeiten Prinzip](http://deviq.com/explicit-dependencies-principle/).</span><span class="sxs-lookup"><span data-stu-id="89724-119">Controllers should follow the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/).</span></span> <span data-ttu-id="89724-120">Es gibt einige Ansätze für die Implementierung dieses Prinzips.</span><span class="sxs-lookup"><span data-stu-id="89724-120">There are a couple approaches to implementing this principle.</span></span> <span data-ttu-id="89724-121">Wenn mehrere Controlleraktionen desselben Diensts benötigen, erwägen Sie [Konstruktoreinfügung](xref:mvc/controllers/dependency-injection#constructor-injection) solcher Abhängigkeiten anfordern.</span><span class="sxs-lookup"><span data-stu-id="89724-121">If multiple controller actions require the same service, consider using [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) to request those dependencies.</span></span> <span data-ttu-id="89724-122">Wenn der Dienst nur einer einzigen Aktion-Methode erforderlich ist, erwägen Sie [Aktion Injection](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices) zum Anfordern der Abhängigkeitsverhältnis.</span><span class="sxs-lookup"><span data-stu-id="89724-122">If the service is needed by only a single action method, consider using [Action Injection](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices) to request the dependency.</span></span>

<span data-ttu-id="89724-123">Innerhalb der **M**Odel -**V**vorhandenes -**C**Ontroller-Muster, ein Controller ist verantwortlich für die ersten Verarbeitung der Anforderung und Instanziierung des Modells.</span><span class="sxs-lookup"><span data-stu-id="89724-123">Within the **M**odel-**V**iew-**C**ontroller pattern, a controller is responsible for the initial processing of the request and instantiation of the model.</span></span> <span data-ttu-id="89724-124">Im Allgemeinen sollte die geschäftlichen Entscheidungen innerhalb des Modells ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="89724-124">Generally, business decisions should be performed within the model.</span></span>

<span data-ttu-id="89724-125">Der Controller nutzt das Ergebnis der Verarbeitung des Modells, (sofern vorhanden) und gibt die richtige Ansicht und deren zugeordneten Daten oder das Ergebnis des API-Aufrufs.</span><span class="sxs-lookup"><span data-stu-id="89724-125">The controller takes the result of the model's processing (if any) and returns either the proper view and its associated view data or the result of the API call.</span></span> <span data-ttu-id="89724-126">Weitere Informationen zu [Übersicht über ASP.NET Core MVC](xref:mvc/overview) und [erste Schritte mit ASP.NET Core MVC und Visual Studio](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="89724-126">Learn more at [Overview of ASP.NET Core MVC](xref:mvc/overview) and [Getting started with ASP.NET Core MVC and Visual Studio](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="89724-127">Der Controller ist ein *-Benutzeroberflächenebene* Abstraktion.</span><span class="sxs-lookup"><span data-stu-id="89724-127">The controller is a *UI-level* abstraction.</span></span> <span data-ttu-id="89724-128">Ihren Aufgaben sind, um sicherzustellen, dass Anforderungsdaten gültig ist und auswählen, welche Ansicht (oder das Ergebnis für eine API) zurückgegeben werden sollen.</span><span class="sxs-lookup"><span data-stu-id="89724-128">Its responsibilities are to ensure request data is valid and to choose which view (or result for an API) should be returned.</span></span> <span data-ttu-id="89724-129">In gut ausgearbeitete apps ist es nicht direkt Daten zugreifen oder die Geschäftslogik enthalten.</span><span class="sxs-lookup"><span data-stu-id="89724-129">In well-factored apps, it does not directly include data access or business logic.</span></span> <span data-ttu-id="89724-130">Stattdessen werden der Controller an Diensten behandeln diese Aufgaben delegiert.</span><span class="sxs-lookup"><span data-stu-id="89724-130">Instead, the controller delegates to services handling these responsibilities.</span></span>

## <a name="defining-actions"></a><span data-ttu-id="89724-131">Definieren von Aktionen</span><span class="sxs-lookup"><span data-stu-id="89724-131">Defining Actions</span></span>

<span data-ttu-id="89724-132">Öffentliche Methoden auf einem Domänencontroller, außer denen mit ergänzt die `[NonAction]` -Attribut angegeben wird, werden die Aktionen.</span><span class="sxs-lookup"><span data-stu-id="89724-132">Public methods on a controller, except those decorated with the `[NonAction]` attribute, are actions.</span></span> <span data-ttu-id="89724-133">Parameter für Aktionen Anfordern von Daten gebunden werden und werden überprüft mithilfe von [modellbindung](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="89724-133">Parameters on actions are bound to request data and are validated using [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="89724-134">Eine modellvalidierung erfolgt für alle Elemente, das Modell gebunden ist.</span><span class="sxs-lookup"><span data-stu-id="89724-134">Model validation occurs for everything that's model-bound.</span></span> <span data-ttu-id="89724-135">Die `ModelState.IsValid` Eigenschaftswert angibt, ob die modellbindung und Überprüfung war erfolgreich.</span><span class="sxs-lookup"><span data-stu-id="89724-135">The `ModelState.IsValid` property value indicates whether model binding and validation succeeded.</span></span>

<span data-ttu-id="89724-136">Aktionsmethoden sollte die Logik für die Zuordnung von einer Anforderungs zu Besorgnis Business enthalten.</span><span class="sxs-lookup"><span data-stu-id="89724-136">Action methods should contain logic for mapping a request to a business concern.</span></span> <span data-ttu-id="89724-137">Geschäftsprobleme sollte als Dienste, die der Controller über zugreift i. d. r. dargestellt werden [Abhängigkeitsinjektion](xref:mvc/controllers/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="89724-137">Business concerns should typically be represented as services that the controller accesses through [dependency injection](xref:mvc/controllers/dependency-injection).</span></span> <span data-ttu-id="89724-138">Aktionen werden Anwendungsstatus ist dann das Ergebnis der Aktion Business zuordnen.</span><span class="sxs-lookup"><span data-stu-id="89724-138">Actions then map the result of the business action to an application state.</span></span>

<span data-ttu-id="89724-139">Aktionen können nichts zurück, aber häufig zurückgeben eine Instanz von `IActionResult` (oder `Task<IActionResult>` für asynchrone Methoden), die eine Antwort erzeugt.</span><span class="sxs-lookup"><span data-stu-id="89724-139">Actions can return anything, but frequently return an instance of `IActionResult` (or `Task<IActionResult>` for async methods) that produces a response.</span></span> <span data-ttu-id="89724-140">Die Aktionsmethode ist verantwortlich für die Auswahl *welche Art von Antwort*.</span><span class="sxs-lookup"><span data-stu-id="89724-140">The action method is responsible for choosing *what kind of response*.</span></span> <span data-ttu-id="89724-141">Das Aktionsergebnis *ist der reagiert*.</span><span class="sxs-lookup"><span data-stu-id="89724-141">The action result *does the responding*.</span></span>

### <a name="controller-helper-methods"></a><span data-ttu-id="89724-142">Controller-Hilfsmethoden</span><span class="sxs-lookup"><span data-stu-id="89724-142">Controller Helper Methods</span></span>

<span data-ttu-id="89724-143">Domänencontroller in der Regel erben [Controller](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller), obwohl dies nicht erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="89724-143">Controllers usually inherit from [Controller](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller), although this is not required.</span></span> <span data-ttu-id="89724-144">Ableiten von `Controller` ermöglicht den Zugriff auf drei Kategorien von Hilfsmethoden:</span><span class="sxs-lookup"><span data-stu-id="89724-144">Deriving from `Controller` provides access to three categories of helper methods:</span></span>

#### <a name="1-methods-resulting-in-an-empty-response-body"></a><span data-ttu-id="89724-145">1. Methoden, was zu einer leeren Antworttext</span><span class="sxs-lookup"><span data-stu-id="89724-145">1. Methods resulting in an empty response body</span></span>

<span data-ttu-id="89724-146">Nicht `Content-Type` HTTP-Antwortheader enthalten, ist, seit der Antworttext verfügt nicht über die Inhalte zu beschreiben.</span><span class="sxs-lookup"><span data-stu-id="89724-146">No `Content-Type` HTTP response header is included, since the response body lacks content to describe.</span></span>

<span data-ttu-id="89724-147">Es gibt zwei Typen in dieser Kategorie: Umleitung und HTTP-Statuscode.</span><span class="sxs-lookup"><span data-stu-id="89724-147">There are two result types within this category: Redirect and HTTP Status Code.</span></span>

* <span data-ttu-id="89724-148">**HTTP-Statuscode**</span><span class="sxs-lookup"><span data-stu-id="89724-148">**HTTP Status Code**</span></span>

    <span data-ttu-id="89724-149">Dieser Typ gibt einen HTTP-Statuscode zurück.</span><span class="sxs-lookup"><span data-stu-id="89724-149">This type returns an HTTP status code.</span></span> <span data-ttu-id="89724-150">Einige Hilfsmethoden dieses Typs sind `BadRequest`, `NotFound`, und `Ok`.</span><span class="sxs-lookup"><span data-stu-id="89724-150">A couple helper methods of this type are `BadRequest`, `NotFound`, and `Ok`.</span></span> <span data-ttu-id="89724-151">Beispielsweise `return BadRequest();` erzeugt einen Statuscode "400" bei der Ausführung.</span><span class="sxs-lookup"><span data-stu-id="89724-151">For example, `return BadRequest();` produces a 400 status code when executed.</span></span> <span data-ttu-id="89724-152">Bei Methoden, z. B. `BadRequest`, `NotFound`, und `Ok` sind überladen sind, sollten sie nicht mehr gelten als Responder Statuscode "HTTP", da inhaltsaushandlung stattfindet.</span><span class="sxs-lookup"><span data-stu-id="89724-152">When methods such as `BadRequest`, `NotFound`, and `Ok` are overloaded, they no longer qualify as HTTP Status Code responders, since content negotiation is taking place.</span></span>

* <span data-ttu-id="89724-153">**Redirect**</span><span class="sxs-lookup"><span data-stu-id="89724-153">**Redirect**</span></span>

    <span data-ttu-id="89724-154">Dieser Typ gibt eine Umleitung an eine Aktion oder ein Ziel zurück (mit `Redirect`, `LocalRedirect`, `RedirectToAction`, oder `RedirectToRoute`).</span><span class="sxs-lookup"><span data-stu-id="89724-154">This type returns a redirect to an action or destination (using `Redirect`, `LocalRedirect`, `RedirectToAction`, or `RedirectToRoute`).</span></span> <span data-ttu-id="89724-155">Beispielsweise `return RedirectToAction("Complete", new {id = 123});` leitet an `Complete`, ein anonymes Objekt übergeben.</span><span class="sxs-lookup"><span data-stu-id="89724-155">For example, `return RedirectToAction("Complete", new {id = 123});` redirects to `Complete`, passing an anonymous object.</span></span>

    <span data-ttu-id="89724-156">Der Ergebnistyp für die Umleitung unterscheidet sich von der HTTP-Statuscode-Typ in erster Linie in das Hinzufügen einer `Location` HTTP-Antwortheader.</span><span class="sxs-lookup"><span data-stu-id="89724-156">The Redirect result type differs from the HTTP Status Code type primarily in the addition of a `Location` HTTP response header.</span></span>

#### <a name="2-methods-resulting-in-a-non-empty-response-body-with-a-predefined-content-type"></a><span data-ttu-id="89724-157">2. Methoden, die in einen nicht leeren Antworttext durch einen vordefinierten Inhaltstyp resultierende</span><span class="sxs-lookup"><span data-stu-id="89724-157">2. Methods resulting in a non-empty response body with a predefined content type</span></span>

<span data-ttu-id="89724-158">Die meisten Hilfsmethoden in dieser Kategorie gehören eine `ContentType` Eigenschaft, die Sie festlegen, sodass die `Content-Type` Antwortheader zum Beschreiben des Antworttexts.</span><span class="sxs-lookup"><span data-stu-id="89724-158">Most helper methods in this category include a `ContentType` property, allowing you to set the `Content-Type` response header to describe the response body.</span></span>

<span data-ttu-id="89724-159">Es gibt zwei Typen in dieser Kategorie: [Ansicht](xref:mvc/views/overview) und [Antwort formatiert](xref:mvc/models/formatting).</span><span class="sxs-lookup"><span data-stu-id="89724-159">There are two result types within this category: [View](xref:mvc/views/overview) and [Formatted Response](xref:mvc/models/formatting).</span></span>

* <span data-ttu-id="89724-160">**Ansicht**</span><span class="sxs-lookup"><span data-stu-id="89724-160">**View**</span></span>

    <span data-ttu-id="89724-161">Dieser Typ zurückgibt, eine Sicht, die ein Modell verwendet wird, um das Rendering von HTML.</span><span class="sxs-lookup"><span data-stu-id="89724-161">This type returns a view which uses a model to render HTML.</span></span> <span data-ttu-id="89724-162">Beispielsweise `return View(customer);` übergibt ein Modell zur Ansicht für die Datenbindung.</span><span class="sxs-lookup"><span data-stu-id="89724-162">For example, `return View(customer);` passes a model to the view for data-binding.</span></span>

* <span data-ttu-id="89724-163">**Formatierte Antwort**</span><span class="sxs-lookup"><span data-stu-id="89724-163">**Formatted Response**</span></span>

    <span data-ttu-id="89724-164">Dieser Typ zurückgibt, JSON oder eine ähnliche Datenaustauschformat, um ein Objekt in einer bestimmten Weise darzustellen.</span><span class="sxs-lookup"><span data-stu-id="89724-164">This type returns JSON or a similar data exchange format to represent an object in a specific manner.</span></span> <span data-ttu-id="89724-165">Beispielsweise `return Json(customer);` serialisiert das angegebene Objekt in JSON-Format.</span><span class="sxs-lookup"><span data-stu-id="89724-165">For example, `return Json(customer);` serializes the provided object into JSON format.</span></span>
    
    <span data-ttu-id="89724-166">Dieses Typs andere übliche Methoden umfassen `File`, `PhysicalFile`, und `VirtualFile`.</span><span class="sxs-lookup"><span data-stu-id="89724-166">Other common methods of this type include `File`, `PhysicalFile`, and `VirtualFile`.</span></span> <span data-ttu-id="89724-167">Beispielsweise `return PhysicalFile(customerFilePath, "text/xml");` gibt eine XML-Datei beschrieben durch einen `Content-Type` Wert des Antwortheaders der "Text/Xml".</span><span class="sxs-lookup"><span data-stu-id="89724-167">For example, `return PhysicalFile(customerFilePath, "text/xml");` returns an XML file described by a `Content-Type` response header value of "text/xml".</span></span>

#### <a name="3-methods-resulting-in-a-non-empty-response-body-formatted-in-a-content-type-negotiated-with-the-client"></a><span data-ttu-id="89724-168">3. Methoden, die in einen nicht leeren Antworttext resultierenden in einen Inhaltstyp, der mit dem Client ausgehandelt formatiert</span><span class="sxs-lookup"><span data-stu-id="89724-168">3. Methods resulting in a non-empty response body formatted in a content type negotiated with the client</span></span>

<span data-ttu-id="89724-169">Diese Kategorie ist eine bessere Leistung als **Inhaltsaushandlung**.</span><span class="sxs-lookup"><span data-stu-id="89724-169">This category is better known as **Content Negotiation**.</span></span> <span data-ttu-id="89724-170">[Inhalts-Aushandlung](xref:mvc/models/formatting#content-negotiation) gilt immer, wenn eine Aktion gibt eine [ObjectResult](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.objectresult) Typ oder einen anderen Wert als eine [IActionResult](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.iactionresult) Implementierung.</span><span class="sxs-lookup"><span data-stu-id="89724-170">[Content negotiation](xref:mvc/models/formatting#content-negotiation) applies whenever an action returns an [ObjectResult](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.objectresult) type or something other than an [IActionResult](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.iactionresult) implementation.</span></span> <span data-ttu-id="89724-171">Eine Aktion, die ein nicht-gibt`IActionResult` Implementierung (z. B. `object`) gibt auch eine Antwort formatiert.</span><span class="sxs-lookup"><span data-stu-id="89724-171">An action that returns a non-`IActionResult` implementation (for example, `object`) also returns a Formatted Response.</span></span>

<span data-ttu-id="89724-172">Einige Hilfsmethoden dieses Typs enthalten `BadRequest`, `CreatedAtRoute`, und `Ok`.</span><span class="sxs-lookup"><span data-stu-id="89724-172">Some helper methods of this type include `BadRequest`, `CreatedAtRoute`, and `Ok`.</span></span> <span data-ttu-id="89724-173">Beispiele für diese Methoden sind `return BadRequest(modelState);`, `return CreatedAtRoute("routename", values, newobject);`, und `return Ok(value);`zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="89724-173">Examples of these methods include `return BadRequest(modelState);`, `return CreatedAtRoute("routename", values, newobject);`, and `return Ok(value);`, respectively.</span></span> <span data-ttu-id="89724-174">Beachten Sie, dass `BadRequest` und `Ok` führen Sie die inhaltsaushandlung nur, wenn ein Wert übergeben, ohne einen Wert, sie stattdessen dienen als HTTP-Statuscode: Ergebnistypen.</span><span class="sxs-lookup"><span data-stu-id="89724-174">Note that `BadRequest` and `Ok` perform content negotiation only when passed a value; without being passed a value, they instead serve as HTTP Status Code result types.</span></span> <span data-ttu-id="89724-175">Die `CreatedAtRoute` -Methode, andererseits, immer inhaltsaushandlungen seit seiner Überladungen, die alle erfordern, dass ein Wert übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="89724-175">The `CreatedAtRoute` method, on the other hand, always performs content negotiation since its overloads all require that a value be passed.</span></span>

### <a name="cross-cutting-concerns"></a><span data-ttu-id="89724-176">Querschnittliche Sicherheitsrisiken</span><span class="sxs-lookup"><span data-stu-id="89724-176">Cross-Cutting Concerns</span></span>

<span data-ttu-id="89724-177">Anwendungen gemeinsam in der Regel Teile ihrer Workflows verwenden.</span><span class="sxs-lookup"><span data-stu-id="89724-177">Applications typically share parts of their workflow.</span></span> <span data-ttu-id="89724-178">Beispiele sind eine app, die eine Authentifizierung auf dem Einkaufswagen erforderlich ist, oder eine app, die Daten auf einige Seiten zwischengespeichert.</span><span class="sxs-lookup"><span data-stu-id="89724-178">Examples include an app that requires authentication to access the shopping cart, or an app that caches data on some pages.</span></span> <span data-ttu-id="89724-179">Um die Logik vor oder nach einer Aktionsmethode ausführen, verwenden Sie eine *Filter*.</span><span class="sxs-lookup"><span data-stu-id="89724-179">To perform logic before or after an action method, use a *filter*.</span></span> <span data-ttu-id="89724-180">Mit [Filter](xref:mvc/controllers/filters) querschnittliche Bedenken können Duplikate, die ihnen ermöglicht, führen reduziert den [Don't wiederholen selbst (trocken)-Prinzip](http://deviq.com/don-t-repeat-yourself/).</span><span class="sxs-lookup"><span data-stu-id="89724-180">Using [Filters](xref:mvc/controllers/filters) on cross-cutting concerns can reduce duplication, allowing them to follow the [Don't Repeat Yourself (DRY) principle](http://deviq.com/don-t-repeat-yourself/).</span></span>

<span data-ttu-id="89724-181">Die meisten Attribute, z. B. filtern `[Authorize]`, auf der Ebene Controller bzw. die Aktionsmethode, die je nach der gewünschten Ebene der Granularität angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="89724-181">Most filter attributes, such as `[Authorize]`, can be applied at the controller or action level depending upon the desired level of granularity.</span></span>

<span data-ttu-id="89724-182">Fehlerbehandlung und Zwischenspeichern von Antworten sind häufig querschnittliche bedenken:</span><span class="sxs-lookup"><span data-stu-id="89724-182">Error handling and response caching are often cross-cutting concerns:</span></span>
   * [<span data-ttu-id="89724-183">Fehlerbehandlung</span><span class="sxs-lookup"><span data-stu-id="89724-183">Error handling</span></span>](xref:mvc/controllers/filters#exception-filters)
   * [<span data-ttu-id="89724-184">Zwischenspeichern von Antworten</span><span class="sxs-lookup"><span data-stu-id="89724-184">Response Caching</span></span>](xref:performance/caching/response)

<span data-ttu-id="89724-185">Viele Aspekte der querschnittliche können verarbeitet werden, mithilfe von Filtern oder benutzerdefinierte [Middleware](xref:fundamentals/middleware).</span><span class="sxs-lookup"><span data-stu-id="89724-185">Many cross-cutting concerns can be handled using filters or custom [middleware](xref:fundamentals/middleware).</span></span>