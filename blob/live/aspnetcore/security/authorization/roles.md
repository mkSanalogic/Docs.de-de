---
title: Rollenbasierte Autorisierung
author: rick-anderson
description: "Dieses Dokument veranschaulicht, wie ASP.NET Core Controller- und den Zugriff einschränken, indem Sie die Rollen zum Autorisieren Attribut übergeben."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/roles
ms.openlocfilehash: 18964464fea76c91d716202d89ee3a3eb36c3078
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
---
# <a name="role-based-authorization"></a><span data-ttu-id="ba460-103">Rollenbasierte Autorisierung</span><span class="sxs-lookup"><span data-stu-id="ba460-103">Role based Authorization</span></span>

<a name="security-authorization-role-based"></a>

<span data-ttu-id="ba460-104">Beim Erstellen eine Identität kann es auf eine oder mehrere Rollen gehören.</span><span class="sxs-lookup"><span data-stu-id="ba460-104">When an identity is created it may belong to one or more roles.</span></span> <span data-ttu-id="ba460-105">Beispielsweise kann der Tracy Administrator- und Rollen gehören, Masse Scott nur der Benutzerrolle angehören.</span><span class="sxs-lookup"><span data-stu-id="ba460-105">For example, Tracy may belong to the Administrator and User roles whilst Scott may only belong to the User role.</span></span> <span data-ttu-id="ba460-106">Wie diese Rollen erstellt und verwaltet werden, hängt von dem Sicherungsspeicher des Autorisierungsprozesses ab.</span><span class="sxs-lookup"><span data-stu-id="ba460-106">How these roles are created and managed depends on the backing store of the authorization process.</span></span> <span data-ttu-id="ba460-107">Rollen werden verfügbar gemacht, für den Entwickler über die [IsInRole](https://docs.microsoft.com/dotnet/api/system.security.principal.genericprincipal.isinrole) Methode für die [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal) Klasse.</span><span class="sxs-lookup"><span data-stu-id="ba460-107">Roles are exposed to the developer through the [IsInRole](https://docs.microsoft.com/dotnet/api/system.security.principal.genericprincipal.isinrole) method on the [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal) class.</span></span>

## <a name="adding-role-checks"></a><span data-ttu-id="ba460-108">Hinzufügen von rollenüberprüfungen</span><span class="sxs-lookup"><span data-stu-id="ba460-108">Adding role checks</span></span>

<span data-ttu-id="ba460-109">Rollenbasierte autorisierungsüberprüfungen sind deklarative&mdash;Entwickler bettet sie innerhalb ihres Codes für einen Controller oder einer Aktion innerhalb eines Controllers angeben von Rollen, die der aktuelle Benutzer ein Mitglied sein muss, auf die angeforderte Ressource zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="ba460-109">Role-based authorization checks are declarative&mdash;the developer embeds them within their code, against a controller or an action within a controller, specifying roles which the current user must be a member of to access the requested resource.</span></span>

<span data-ttu-id="ba460-110">Z. B. der folgende Code, schränkt den Zugriff auf alle Aktionen auf der `AdministrationController` sind für Benutzer ein Mitglied der `Administrator` Rolle:</span><span class="sxs-lookup"><span data-stu-id="ba460-110">For example, the following code limits access to any actions on the `AdministrationController` to users who are a member of the `Administrator` role:</span></span>

```csharp
[Authorize(Roles = "Administrator")]
public class AdministrationController : Controller
{
}
```

<span data-ttu-id="ba460-111">Sie können mehrere Rollen als durch Trennzeichen getrennte Liste angeben:</span><span class="sxs-lookup"><span data-stu-id="ba460-111">You can specify multiple roles as a comma separated list:</span></span>

```csharp
[Authorize(Roles = "HRManager,Finance")]
public class SalaryController : Controller
{
}
```

<span data-ttu-id="ba460-112">Dieser Controller wäre nur zugegriffen werden kann, von Benutzern, die Mitglieder von der `HRManager` Rolle oder die `Finance` Rolle.</span><span class="sxs-lookup"><span data-stu-id="ba460-112">This controller would be only accessible by users who are members of the `HRManager` role or the `Finance` role.</span></span>

<span data-ttu-id="ba460-113">Wenn Sie mehrere Attribute gelten muss ein Zugriff auf Benutzer ein Mitglied aller angegebenen Rollen; Im folgende Beispiel erfordert, dass ein Benutzer ein Mitglied sowohl der `PowerUser` und `ControlPanelUser` Rolle.</span><span class="sxs-lookup"><span data-stu-id="ba460-113">If you apply multiple attributes then an accessing user must be a member of all the roles specified; the following sample requires that a user must be a member of both the `PowerUser` and `ControlPanelUser` role.</span></span>

```csharp
[Authorize(Roles = "PowerUser")]
[Authorize(Roles = "ControlPanelUser")]
public class ControlPanelController : Controller
{
}
```

<span data-ttu-id="ba460-114">Sie können weiter Zugriff einschränken, indem Sie zusätzliche Autorisierung benutzerrollenattribute Ebene der Aktion anwenden:</span><span class="sxs-lookup"><span data-stu-id="ba460-114">You can further limit access by applying additional role authorization attributes at the action level:</span></span>

```csharp
[Authorize(Roles = "Administrator, PowerUser")]
public class ControlPanelController : Controller
{
    public ActionResult SetTime()
    {
    }

    [Authorize(Roles = "Administrator")]
    public ActionResult ShutDown()
    {
    }
}
```

<span data-ttu-id="ba460-115">In den vorherigen Code Snippet Membern der der `Administrator` Rolle oder die `PowerUser` Rolle kann den Controller zugreifen und die `SetTime` Aktion, aber nur Mitglieder der der `Administrator` Rolle zugreifen kann die `ShutDown` Aktion.</span><span class="sxs-lookup"><span data-stu-id="ba460-115">In the previous code snippet members of the `Administrator` role or the `PowerUser` role can access the controller and the `SetTime` action, but only members of the `Administrator` role can access the `ShutDown` action.</span></span>

<span data-ttu-id="ba460-116">Sie können auch einen Controller Sperren jedoch anonyme, nicht authentifizierte Zugriff auf einzelne Aktionen zulassen.</span><span class="sxs-lookup"><span data-stu-id="ba460-116">You can also lock down a controller but allow anonymous, unauthenticated access to individual actions.</span></span>

```csharp
[Authorize]
public class ControlPanelController : Controller
{
    public ActionResult SetTime()
    {
    }

    [AllowAnonymous]
    public ActionResult Login()
    {
    }
}
```

<a name="security-authorization-role-policy"></a>

## <a name="policy-based-role-checks"></a><span data-ttu-id="ba460-117">Richtlinienbasierte rollenüberprüfungen</span><span class="sxs-lookup"><span data-stu-id="ba460-117">Policy based role checks</span></span>

<span data-ttu-id="ba460-118">Anforderungen für Rollen können auch mithilfe der neuen Richtlinie-Syntax wird registriert, in denen ein Entwickler eine Richtlinie beim Starten des als Teil der Dienstkonfiguration Autorisierung ausgedrückt werden.</span><span class="sxs-lookup"><span data-stu-id="ba460-118">Role requirements can also be expressed using the new Policy syntax, where a developer registers a policy at startup as part of the Authorization service configuration.</span></span> <span data-ttu-id="ba460-119">Dieser Fehler tritt normalerweise in `ConfigureServices()` in Ihrer *Startup.cs* Datei.</span><span class="sxs-lookup"><span data-stu-id="ba460-119">This normally occurs in `ConfigureServices()` in your *Startup.cs* file.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("RequireAdministratorRole", policy => policy.RequireRole("Administrator"));
    });
}
```

<span data-ttu-id="ba460-120">Richtlinien werden angewendet, mit der `Policy` Eigenschaft auf die `AuthorizeAttribute` Attribut:</span><span class="sxs-lookup"><span data-stu-id="ba460-120">Policies are applied using the `Policy` property on the `AuthorizeAttribute` attribute:</span></span>

```csharp
[Authorize(Policy = "RequireAdministratorRole")]
public IActionResult Shutdown()
{
    return View();
}
```

<span data-ttu-id="ba460-121">Wenn Sie mehrere zulässige Rollen in einer Anforderung angeben möchten, und klicken Sie dann als Parameter zum Angeben der `RequireRole` Methode:</span><span class="sxs-lookup"><span data-stu-id="ba460-121">If you want to specify multiple allowed roles in a requirement then you can specify them as parameters to the `RequireRole` method:</span></span>

```csharp
options.AddPolicy("ElevatedRights", policy =>
                  policy.RequireRole("Administrator", "PowerUser", "BackupAdministrator"));
```

<span data-ttu-id="ba460-122">In diesem Beispiel autorisiert Benutzer angehören der `Administrator`, `PowerUser` oder `BackupAdministrator` Rollen.</span><span class="sxs-lookup"><span data-stu-id="ba460-122">This example authorizes users who belong to the `Administrator`, `PowerUser` or `BackupAdministrator` roles.</span></span>