---
title: Autorisieren Sie mit einem bestimmten Schema - ASP.NET Core
author: rick-anderson
description: "In diesem Artikel erläutert die Identität, die für ein bestimmtes Schema zu beschränken, bei der Arbeit mit mehrere Authentifizierungsmethoden."
ms.author: riande
manager: wpickett
ms.date: 10/12/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/limitingidentitybyscheme
ms.openlocfilehash: 099dba1a4235ef62ea298748645b99e2d6d12d44
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
---
# <a name="authorize-with-a-specific-scheme"></a><span data-ttu-id="58bb8-103">Mit einem bestimmten Schema autorisieren</span><span class="sxs-lookup"><span data-stu-id="58bb8-103">Authorize with a specific scheme</span></span>

<span data-ttu-id="58bb8-104">In einigen Szenarien, z. B. Single Page Applications (SPAs) ist es üblich, dass mehrere Authentifizierungsmethoden verwendet.</span><span class="sxs-lookup"><span data-stu-id="58bb8-104">In some scenarios, such as Single Page Applications (SPAs), it's common to use multiple authentication methods.</span></span> <span data-ttu-id="58bb8-105">Beispielsweise kann die app für JavaScript-Anforderungen Cookie-basierte Authentifizierung, um sich anzumelden und JWT-trägertoken-Authentifizierung verwenden.</span><span class="sxs-lookup"><span data-stu-id="58bb8-105">For example, the app may use cookie-based authentication to log in and JWT bearer authentication for JavaScript requests.</span></span> <span data-ttu-id="58bb8-106">In einigen Fällen kann die app mehrere Instanzen von einen authentifizierungshandler sein.</span><span class="sxs-lookup"><span data-stu-id="58bb8-106">In some cases, the app may have multiple instances of an authentication handler.</span></span> <span data-ttu-id="58bb8-107">Angenommen, wird zwei Cookie-Handler, in denen eine eine grundlegende Identität enthält, und eine erstellt, wenn ein Multi-Factor Authentication (MFA) ausgelöst wurde.</span><span class="sxs-lookup"><span data-stu-id="58bb8-107">For example, two cookie handlers where one contains a basic identity and one is created when a multi-factor authentication (MFA) has been triggered.</span></span> <span data-ttu-id="58bb8-108">MFA kann ausgelöst werden, da der Benutzer einen Vorgang angefordert, der zusätzliche Sicherheit erfordert.</span><span class="sxs-lookup"><span data-stu-id="58bb8-108">MFA may be triggered because the user requested an operation that requires extra security.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="58bb8-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="58bb8-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="58bb8-110">Ein anderes Authentifizierungsschema wird mit dem Namen, wenn der Authentifizierungsdienst während der Authentifizierung konfiguriert ist.</span><span class="sxs-lookup"><span data-stu-id="58bb8-110">An authentication scheme is named when the authentication service is configured during authentication.</span></span> <span data-ttu-id="58bb8-111">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="58bb8-111">For example:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Code omitted for brevity

    services.AddAuthentication()
        .AddCookie(options => {
            options.LoginPath = "/Account/Unauthorized/";
            options.AccessDeniedPath = "/Account/Forbidden/";
        })
        .AddJwtBearer(options => {
            options.Audience = "http://localhost:5001/";
            options.Authority = "http://localhost:5000/";
        });
```

<span data-ttu-id="58bb8-112">Im vorangehenden Code wurden zwei authentifizierungshandler hinzugefügt: eine für Cookies und eine für trägertoken.</span><span class="sxs-lookup"><span data-stu-id="58bb8-112">In the preceding code, two authentication handlers have been added: one for cookies and one for bearer.</span></span>

>[!NOTE]
><span data-ttu-id="58bb8-113">Das Standardschema Angabe führt zu der `HttpContext.User` -Eigenschaft, die auf diese Identität festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="58bb8-113">Specifying the default scheme results in the `HttpContext.User` property being set to that identity.</span></span> <span data-ttu-id="58bb8-114">Wenn dieses Verhalten nicht wünschen, deaktivieren Sie ihn durch Aufrufen der parameterlosen Formulars der `AddAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="58bb8-114">If that behavior isn't desired, disable it by invoking the parameterless form of `AddAuthentication`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="58bb8-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="58bb8-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="58bb8-116">Authentifizierungsschemas werden mit dem Namen, wenn Authentifizierung Middlewares während der Authentifizierung konfiguriert sind.</span><span class="sxs-lookup"><span data-stu-id="58bb8-116">Authentication schemes are named when authentication middlewares are configured during authentication.</span></span> <span data-ttu-id="58bb8-117">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="58bb8-117">For example:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
{
    // Code omitted for brevity

    app.UseCookieAuthentication(new CookieAuthenticationOptions()
    {
        AuthenticationScheme = "Cookie",
        LoginPath = "/Account/Unauthorized/",
        AccessDeniedPath = "/Account/Forbidden/",
        AutomaticAuthenticate = false
    });
    
    app.UseJwtBearerAuthentication(new JwtBearerOptions()
    {
        AuthenticationScheme = "Bearer",
        AutomaticAuthenticate = false,
        Audience = "http://localhost:5001/",
        Authority = "http://localhost:5000/",
        RequireHttpsMetadata = false
    });
```

<span data-ttu-id="58bb8-118">Im vorangehenden Code wurden zwei Authentifizierung Middlewares hinzugefügt: eine für Cookies und eine für trägertoken.</span><span class="sxs-lookup"><span data-stu-id="58bb8-118">In the preceding code, two authentication middlewares have been added: one for cookies and one for bearer.</span></span>

>[!NOTE]
><span data-ttu-id="58bb8-119">Das Standardschema Angabe führt zu der `HttpContext.User` -Eigenschaft, die auf diese Identität festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="58bb8-119">Specifying the default scheme results in the `HttpContext.User` property being set to that identity.</span></span> <span data-ttu-id="58bb8-120">Wenn dieses Verhalten nicht wünschen, deaktivieren Sie es durch Festlegen der `AuthenticationOptions.AutomaticAuthenticate` Eigenschaft `false`.</span><span class="sxs-lookup"><span data-stu-id="58bb8-120">If that behavior isn't desired, disable it by setting the `AuthenticationOptions.AutomaticAuthenticate` property to `false`.</span></span>

---

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a><span data-ttu-id="58bb8-121">Wählen das Schema mit dem Authorize-Attribut</span><span class="sxs-lookup"><span data-stu-id="58bb8-121">Selecting the scheme with the Authorize attribute</span></span>

<span data-ttu-id="58bb8-122">Die app zum Zeitpunkt der Autorisierung gibt an, dass der Handler verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="58bb8-122">At the point of authorization, the app indicates the handler to be used.</span></span> <span data-ttu-id="58bb8-123">Wählen Sie den Handler mit dem die app durch Übergeben einer durch Trennzeichen getrennte Liste der Authentifizierungsschemas autorisieren wird `[Authorize]`.</span><span class="sxs-lookup"><span data-stu-id="58bb8-123">Select the handler with which the app will authorize by passing a comma-delimited list of authentication schemes to `[Authorize]`.</span></span> <span data-ttu-id="58bb8-124">Die `[Authorize]` Attribut gibt an, das Authentifizierungsschema oder Schemas verwenden, unabhängig davon, ob ein Standardwert konfiguriert ist.</span><span class="sxs-lookup"><span data-stu-id="58bb8-124">The `[Authorize]` attribute specifies the authentication scheme or schemes to use regardless of whether a default is configured.</span></span> <span data-ttu-id="58bb8-125">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="58bb8-125">For example:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="58bb8-126">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="58bb8-126">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
[Authorize(AuthenticationSchemes = AuthSchemes)]
public class MixedController : Controller
    // Requires the following imports:
    // using Microsoft.AspNetCore.Authentication.Cookies;
    // using Microsoft.AspNetCore.Authentication.JwtBearer;
    private const string AuthSchemes =
        CookieAuthenticationDefaults.AuthenticationScheme + "," +
        JwtBearerDefaults.AuthenticationScheme;
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="58bb8-127">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="58bb8-127">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
[Authorize(ActiveAuthenticationSchemes = AuthSchemes)]
public class MixedController : Controller
    // Requires the following imports:
    // using Microsoft.AspNetCore.Authentication.Cookies;
    // using Microsoft.AspNetCore.Authentication.JwtBearer;
    private const string AuthSchemes =
        CookieAuthenticationDefaults.AuthenticationScheme + "," +
        JwtBearerDefaults.AuthenticationScheme;
```

---

<span data-ttu-id="58bb8-128">Im vorherigen Beispiel wird dem Cookie und den Träger-Handler ausgeführt und haben die Möglichkeit, zu erstellen, und fügen eine Identität für den aktuellen Benutzer.</span><span class="sxs-lookup"><span data-stu-id="58bb8-128">In the preceding example, both the cookie and bearer handlers run and have a chance to create and append an identity for the current user.</span></span> <span data-ttu-id="58bb8-129">Führt durch die Angabe nur eines einzelnen Schemas, die entsprechenden Handler.</span><span class="sxs-lookup"><span data-stu-id="58bb8-129">By specifying a single scheme only, the corresponding handler runs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="58bb8-130">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="58bb8-130">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
[Authorize(AuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="58bb8-131">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="58bb8-131">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
[Authorize(ActiveAuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

---

<span data-ttu-id="58bb8-132">Im vorangehenden Code ausgeführt wird nur der Handler mit dem Schema "Träger" aus.</span><span class="sxs-lookup"><span data-stu-id="58bb8-132">In the preceding code, only the handler with the "Bearer" scheme runs.</span></span> <span data-ttu-id="58bb8-133">Cookie-basierte Identitäten werden ignoriert.</span><span class="sxs-lookup"><span data-stu-id="58bb8-133">Any cookie-based identities are ignored.</span></span>

## <a name="selecting-the-scheme-with-policies"></a><span data-ttu-id="58bb8-134">Das Schema mit Richtlinien auswählen</span><span class="sxs-lookup"><span data-stu-id="58bb8-134">Selecting the scheme with policies</span></span>

<span data-ttu-id="58bb8-135">Wenn Sie es vorziehen, geben Sie die gewünschten Schemas in [Richtlinie](xref:security/authorization/policies), Sie können festlegen, die `AuthenticationSchemes` Auflistung bei Ihrer Richtlinie hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="58bb8-135">If you prefer to specify the desired schemes in [policy](xref:security/authorization/policies), you can set the `AuthenticationSchemes` collection when adding your policy:</span></span>

```csharp
services.AddAuthorization(options =>
{
    options.AddPolicy("Over18", policy =>
    {
        policy.AuthenticationSchemes.Add(JwtBearerDefaults.AuthenticationScheme);
        policy.RequireAuthenticatedUser();
        policy.Requirements.Add(new MinimumAgeRequirement());
    });
});
```

<span data-ttu-id="58bb8-136">Im vorherigen Beispiel wird die Richtlinie "Over18" nur ausgeführt, mit der Identität, die vom Handler "Träger" erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="58bb8-136">In the preceding example, the "Over18" policy only runs against the identity created by the "Bearer" handler.</span></span> <span data-ttu-id="58bb8-137">Verwenden Sie die Richtlinie durch Festlegen der `[Authorize]` des Attributs `Policy` Eigenschaft:</span><span class="sxs-lookup"><span data-stu-id="58bb8-137">Use the policy by setting the `[Authorize]` attribute's `Policy` property:</span></span>

```csharp
[Authorize(Policy = "Over18")]
public class RegistrationController : Controller
```