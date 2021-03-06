---
title: Schreiben einer Web-App für die Benutzeranmeldung und -abmeldung – Microsoft Identity Platform | Azure
description: Erfahren Sie, wie Sie eine Web-App zur Benutzeranmeldung und -abmeldung erstellen.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 10/30/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 2ab5697ceff612e65174fdb7f9ef6137e2c8b9a5
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81537065"
---
# <a name="web-app-that-signs-in-users-sign-in-and-sign-out"></a>Web-App für Benutzeranmeldungen: An- und Abmeldung

Erfahren Sie, wie Sie dem Code für Ihre Web-App eine Anmeldefunktion zur Benutzeranmeldung hinzufügen. Anschließend erfahren Sie, wie Sie auch die Abmeldung ermöglichen.

## <a name="sign-in"></a>Anmeldung

Die Anmeldung besteht aus zwei Teilen:

- Schaltfläche für die Anmeldung auf der HTML-Seite
- Anmeldeaktion im CodeBehind im Controller

### <a name="sign-in-button"></a>Schaltfläche für die Anmeldung

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

In ASP.NET Core wird die Anmeldeschaltfläche in `Views\Shared\_LoginPartial.cshtml` verfügbar gemacht. Sie wird nur angezeigt, wenn kein authentifiziertes Konto vorhanden ist. Das heißt, sie wird angezeigt, wenn sich der Benutzer noch nicht angemeldet oder sich zuvor abgemeldet hat.

```html
@using Microsoft.Identity.Web
@if (User.Identity.IsAuthenticated)
{
 // Code omitted code for clarity
}
else
{
    <ul class="nav navbar-nav navbar-right">
        <li><a asp-area="AzureAD" asp-controller="Account" asp-action="SignIn">Sign in</a></li>
    </ul>
}
```

# <a name="aspnet"></a>[ASP.NET](#tab/aspnet)

In ASP.NET MVC wird die Abmeldeschaltfläche in `Views\Shared\_LoginPartial.cshtml` verfügbar gemacht. Sie wird nur angezeigt, wenn ein authentifiziertes Konto vorhanden ist. Das heißt, sie wird angezeigt, wenn sich der Benutzer zuvor angemeldet hat.

```html
@if (Request.IsAuthenticated)
{
 // Code omitted code for clarity
}
else
{
    <ul class="nav navbar-nav navbar-right">
        <li>@Html.ActionLink("Sign in", "SignIn", "Account", routeValues: null, htmlAttributes: new { id = "loginLink" })</li>
    </ul>
}
```

# <a name="java"></a>[Java](#tab/java)

In unserem Java-Schnellstart befindet sich die Anmeldeschaltfläche in der Datei [main/resources/templates/index.html](https://github.com/Azure-Samples/ms-identity-java-webapp/blob/master/src/main/resources/templates/index.html).

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>HomePage</title>
</head>
<body>
<h3>Home Page</h3>

<form action="/msal4jsample/secure/aad">
    <input type="submit" value="Login">
</form>

</body>
</html>
```

# <a name="python"></a>[Python](#tab/python)

Im Python-Schnellstart gibt es keine Schaltfläche für die Anmeldung. Beim Erreichen des Stamms der Web-App wird der Benutzer vom CodeBehind automatisch aufgefordert, sich anzumelden. Siehe [app.py#L14-L18](https://github.com/Azure-Samples/ms-identity-python-webapp/blob/0.1.0/app.py#L14-L18).

```Python
@app.route("/")
def index():
    if not session.get("user"):
        return redirect(url_for("login"))
    return render_template('index.html', user=session["user"])
```

---

### <a name="signin-action-of-the-controller"></a>`SignIn`-Aktion des Controllers

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

+In ASP.NET wird durch Auswahl der Schaltfläche **Anmelden** in der Web-App die Aktion `SignIn` auf dem `AccountController`-Controller ausgelöst. In früheren Versionen der ASP.NET Core-Vorlagen war der `Account`-Controller in die Web-App eingebettet. Das ist nicht mehr der Fall, da der Controller jetzt Teil des ASP.NET Core-Frameworks ist.

Der Code für den `AccountController` steht im ASP.NET Core-Repository in [AccountController.cs](https://github.com/aspnet/AspNetCore/blob/master/src/Azure/AzureAD/Authentication.AzureAD.UI/src/Areas/AzureAD/Controllers/AccountController.cs) zur Verfügung. Die Kontensteuerung fordert den Benutzer auf, indem er an den Microsoft Identity Platform-Endpunkt umgeleitet wird. Weitere Informationen finden Sie unter der [SignIn](https://github.com/aspnet/AspNetCore/blob/f3e6b74623d42d5164fd5f97a288792c8ad877b6/src/Azure/AzureAD/Authentication.AzureAD.UI/src/Areas/AzureAD/Controllers/AccountController.cs#L23-L31)-Methode, die als Teil von ASP.NET Core bereitgestellt wird.

# <a name="aspnet"></a>[ASP.NET](#tab/aspnet)

In ASP.NET wird die Abmeldung von der `SignOut()`-Methode auf einem Controller ausgelöst (z. B. [AccountController.cs#L16-L23](https://github.com/Azure-Samples/ms-identity-aspnet-webapp-openidconnect/blob/a2da310539aa613b77da1f9e1c17585311ab22b7/WebApp/Controllers/AccountController.cs#L16-L23)). Diese Methode gehört nicht zum ASP.NET-Framework (im Gegensatz zu den Vorgängen in ASP.NET Core). Sie sendet eine OpenID-Anmeldeaufforderung, nachdem ein Umleitungs-URI vorgeschlagen wurde.

```csharp
public void SignIn()
{
    // Send an OpenID Connect sign-in request.
    if (!Request.IsAuthenticated)
    {
        HttpContext.GetOwinContext().Authentication.Challenge(new AuthenticationProperties { RedirectUri = "/" }, OpenIdConnectAuthenticationDefaults.AuthenticationType);
    }
}
```

# <a name="java"></a>[Java](#tab/java)

In Java wird die Abmeldung behandelt, indem der `logout`-Endpunkt der Microsoft Identity Platform direkt aufgerufen und der Wert `post_logout_redirect_uri` bereitgestellt wird. Weitere Informationen finden Sie unter [AuthPageController.java#L30-L48](https://github.com/Azure-Samples/ms-identity-java-webapp/blob/d55ee4ac0ce2c43378f2c99fd6e6856d41bdf144/src/main/java/com/microsoft/azure/msalwebsample/AuthPageController.java#L30-L48).

```Java
@Controller
public class AuthPageController {

    @Autowired
    AuthHelper authHelper;

    @RequestMapping("/msal4jsample")
    public String homepage(){
        return "index";
    }

    @RequestMapping("/msal4jsample/secure/aad")
    public ModelAndView securePage(HttpServletRequest httpRequest) throws ParseException {
        ModelAndView mav = new ModelAndView("auth_page");

        setAccountInfo(mav, httpRequest);

        return mav;
    }

    // More code omitted for simplicity
```

# <a name="python"></a>[Python](#tab/python)

Im Gegensatz zu anderen Plattformen sorgt MSAL für Python dafür, dass sich der Benutzer über die Anmeldeseite anmelden kann. Siehe [app.py#L20-L28](https://github.com/Azure-Samples/ms-identity-python-webapp/blob/e03be352914bfbd58be0d4170eba1fb7a4951d84/app.py#L20-L28).

```Python
@app.route("/login")
def login():
    session["state"] = str(uuid.uuid4())
    auth_url = _build_msal_app().get_authorization_request_url(
        app_config.SCOPE,  # Technically we can use an empty list [] to just sign in
                           # Here we choose to also collect user consent up front
        state=session["state"],
        redirect_uri=url_for("authorized", _external=True))
    return "<a href='%s'>Login with Microsoft Identity</a>" % auth_url
```

Die `_build_msal_app()`-Methode wird in [app.py#L81-L88](https://github.com/Azure-Samples/ms-identity-python-webapp/blob/e03be352914bfbd58be0d4170eba1fb7a4951d84/app.py#L81-L88) wie folgt definiert:

```Python
def _load_cache():
    cache = msal.SerializableTokenCache()
    if session.get("token_cache"):
        cache.deserialize(session["token_cache"])
    return cache

def _save_cache(cache):
    if cache.has_state_changed:
        session["token_cache"] = cache.serialize()

def _build_msal_app(cache=None):
    return msal.ConfidentialClientApplication(
        app_config.CLIENT_ID, authority=app_config.AUTHORITY,
        client_credential=app_config.CLIENT_SECRET, token_cache=cache)

def _get_token_from_cache(scope=None):
    cache = _load_cache()  # This web app maintains one cache per session
    cca = _build_msal_app(cache)
    accounts = cca.get_accounts()
    if accounts:  # So all accounts belong to the current signed-in user
        result = cca.acquire_token_silent(scope, account=accounts[0])
        _save_cache(cache)
        return result

```

---

Nachdem der Benutzer sich bei Ihrer App angemeldet hat, sollten Sie ihm auch das Abmelden ermöglichen.

## <a name="sign-out"></a>Abmeldung

Beim Abmelden von einer Web-App geht es um mehr als um das Entfernen der Informationen zum angemeldeten Konto aus dem Status der Web-App.
Die Web-App muss den Benutzer für die Abmeldung außerdem an den `logout`-Endpunkt von Microsoft Identity Platform umleiten.

Wenn Ihre Web-App den Benutzer an den `logout`-Endpunkt umleitet, löscht dieser Endpunkt die Sitzung des Benutzers aus dem Browser. Wenn Ihre App nicht den `logout`-Endpunkt erreicht hat, kann sich der Benutzer erneut bei Ihrer App authentifizieren, ohne die Anmeldeinformationen erneut eingeben zu müssen. Der Grund hierfür ist, dass er über eine gültige Sitzung für einmaliges Anmelden mit dem Microsoft Identity Platform-Endpunkt verfügt.

Weitere Informationen finden Sie im Abschnitt [Senden einer Abmeldeanforderung](v2-protocols-oidc.md#send-a-sign-out-request) in der Dokumentation zu [Microsoft Identity Platform und OpenID Connect-Protokoll](v2-protocols-oidc.md).

### <a name="application-registration"></a>Anwendungsregistrierung

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

Bei der Anwendungsregistrierung registrieren Sie einen URI nach der Abmeldung. In diesem Tutorial haben Sie die URL `https://localhost:44321/signout-oidc` auf der Seite **Authentifizierung** im Abschnitt **Erweiterte Einstellungen** im Feld **Abmelde-URL** registriert. Weitere Informationen finden Sie unter [Registrieren der webApp-App](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-1-MyOrg#register-the-webapp-app-webapp).

# <a name="aspnet"></a>[ASP.NET](#tab/aspnet)

Bei der Anwendungsregistrierung registrieren Sie einen URI nach der Abmeldung. In diesem Tutorial haben Sie die URL `https://localhost:44308/Account/EndSession` auf der Seite **Authentifizierung** im Abschnitt **Erweiterte Einstellungen** im Feld **Abmelde-URL** registriert. Weitere Informationen finden Sie unter [Registrieren der webApp-App](https://github.com/Azure-Samples/active-directory-dotnet-web-single-sign-out#register-the-service-app-webapp-distributedsignout-dotnet).

# <a name="java"></a>[Java](#tab/java)

Bei der Anwendungsregistrierung registrieren Sie einen URI nach der Abmeldung. In diesem Tutorial haben Sie die URL `http://localhost:8080/msal4jsample/sign_out` auf der Seite **Authentifizierung** im Abschnitt **Erweiterte Einstellungen** im Feld **Abmelde-URL** registriert.

# <a name="python"></a>[Python](#tab/python)

Bei der Anwendungsregistrierung müssen Sie keine zusätzliche Abmelde-URL registrieren. Die App wird über die Haupt-URL zurückgerufen.

---

### <a name="sign-out-button"></a>Abmeldeschaltfläche

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

In ASP.NET Core wird die Abmeldeschaltfläche in `Views\Shared\_LoginPartial.cshtml` verfügbar gemacht. Sie wird nur angezeigt, wenn ein authentifiziertes Konto vorhanden ist. Das heißt, sie wird angezeigt, wenn sich der Benutzer zuvor angemeldet hat.

```html
@using Microsoft.Identity.Web
@if (User.Identity.IsAuthenticated)
{
    <ul class="nav navbar-nav navbar-right">
        <li class="navbar-text">Hello @User.GetDisplayName()!</li>
        <li><a asp-area="AzureAD" asp-controller="Account" asp-action="SignOut">Sign out</a></li>
    </ul>
}
else
{
    <ul class="nav navbar-nav navbar-right">
        <li><a asp-area="AzureAD" asp-controller="Account" asp-action="SignIn">Sign in</a></li>
    </ul>
}
```

# <a name="aspnet"></a>[ASP.NET](#tab/aspnet)

In ASP.NET MVC wird die Abmeldeschaltfläche in `Views\Shared\_LoginPartial.cshtml` verfügbar gemacht. Sie wird nur angezeigt, wenn ein authentifiziertes Konto vorhanden ist. Das heißt, sie wird angezeigt, wenn sich der Benutzer zuvor angemeldet hat.

```html
@if (Request.IsAuthenticated)
{
    <text>
        <ul class="nav navbar-nav navbar-right">
            <li class="navbar-text">
                Hello, @User.Identity.Name!
            </li>
            <li>
                @Html.ActionLink("Sign out", "SignOut", "Account")
            </li>
        </ul>
    </text>
}
else
{
    <ul class="nav navbar-nav navbar-right">
        <li>@Html.ActionLink("Sign in", "SignIn", "Account", routeValues: null, htmlAttributes: new { id = "loginLink" })</li>
    </ul>
}
```

# <a name="java"></a>[Java](#tab/java)

In unserem Java-Schnellstart befindet sich die Abmeldeschaltfläche in der Datei „main/resources/templates/auth_page.html“.

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<body>

<form action="/msal4jsample/sign_out">
    <input type="submit" value="Sign out">
</form>
...
```

# <a name="python"></a>[Python](#tab/python)

Im Python-Schnellstart befindet sich die Abmeldeschaltfläche in der Datei [templates/index.html#L10](https://github.com/Azure-Samples/ms-identity-python-webapp/blob/e03be352914bfbd58be0d4170eba1fb7a4951d84/templates/index.html#L10).

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
</head>
<body>
    <h1>Microsoft Identity Python web app</h1>
    Welcome {{ user.get("name") }}!
    <li><a href='/graphcall'>Call Microsoft Graph API</a></li>
    <li><a href="/logout">Logout</a></li>
</body>
</html>
```

---

### <a name="signout-action-of-the-controller"></a>`SignOut`-Aktion des Controllers

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

In ASP.NET wird durch Auswahl der Schaltfläche **Abmelden** in der Web-App die Aktion `SignOut` auf dem `AccountController`-Controller ausgelöst. In früheren Versionen der ASP.NET Core-Vorlagen war der `Account`-Controller in die Web-App eingebettet. Das ist nicht mehr der Fall, da der Controller jetzt Teil des ASP.NET Core-Frameworks ist.

Der Code für den `AccountController` steht im ASP.NET Core-Repository in [AccountController.cs](https://github.com/aspnet/AspNetCore/blob/master/src/Azure/AzureAD/Authentication.AzureAD.UI/src/Areas/AzureAD/Controllers/AccountController.cs) zur Verfügung. Die Kontosteuerung führt folgende Aktionen durch:

- Sie legt einen OpenID-Umleitungs-URI auf `/Account/SignedOut` fest, damit der Controller zurückgerufen wird, wenn Azure AD die Abmeldung abgeschlossen hat.
- Sie ruft `Signout()` auf, damit die OpenID Connect-Middleware den `logout`-Endpunkt der Microsoft Identity Platform kontaktieren kann. Der Endpunkt führt dann folgende Aktionen aus:

  - Er löscht den Sitzungscookie im Browser.
  - Er ruft die Abmelde-URL zurück. Standardmäßig zeigt die Abmelde-URL die Abmeldeseite [SignedOut.html](https://github.com/aspnet/AspNetCore/blob/master/src/Azure/AzureAD/Authentication.AzureAD.UI/src/Areas/AzureAD/Pages/Account/SignedOut.cshtml) an. Diese Seite wird auch als Teil von ASP.NET Core bereitgestellt.

# <a name="aspnet"></a>[ASP.NET](#tab/aspnet)

In ASP.NET wird die Abmeldung von der `SignOut()`-Methode auf einem Controller ausgelöst (z. B. [AccountController.cs#L25-L31](https://github.com/Azure-Samples/ms-identity-aspnet-webapp-openidconnect/blob/a2da310539aa613b77da1f9e1c17585311ab22b7/WebApp/Controllers/AccountController.cs#L25-L31)). Diese Methode gehört im Gegensatz zu den Vorgängen in ASP.NET Core nicht zum ASP.NET-Framework. Sie hat folgende Aufgaben:

- Sie sendet eine OpenID-Abmeldeaufforderung.
- Sie löscht den Cache.
- Sie leitet auf die gewünschte Seite um.

```csharp
/// <summary>
/// Send an OpenID Connect sign-out request.
/// </summary>
public void SignOut()
{
 HttpContext.GetOwinContext()
            .Authentication
            .SignOut(CookieAuthenticationDefaults.AuthenticationType);
 Response.Redirect("/");
}
```

# <a name="java"></a>[Java](#tab/java)

In Java wird die Abmeldung behandelt, indem der `logout`-Endpunkt der Microsoft Identity Platform direkt aufgerufen und der Wert `post_logout_redirect_uri` bereitgestellt wird. Weitere Informationen finden Sie unter [AuthPageController.java#L50-L60](https://github.com/Azure-Samples/ms-identity-java-webapp/blob/d55ee4ac0ce2c43378f2c99fd6e6856d41bdf144/src/main/java/com/microsoft/azure/msalwebsample/AuthPageController.java#L50-L60).

```Java
@RequestMapping("/msal4jsample/sign_out")
    public void signOut(HttpServletRequest httpRequest, HttpServletResponse response) throws IOException {

        httpRequest.getSession().invalidate();

        String endSessionEndpoint = "https://login.microsoftonline.com/common/oauth2/v2.0/logout";

        String redirectUrl = "http://localhost:8080/msal4jsample/";
        response.sendRedirect(endSessionEndpoint + "?post_logout_redirect_uri=" +
                URLEncoder.encode(redirectUrl, "UTF-8"));
    }
```

# <a name="python"></a>[Python](#tab/python)

Der Code, mit dem der Benutzer abgemeldet wird, befindet sich in [app.py#L46-L52](https://github.com/Azure-Samples/ms-identity-python-webapp/blob/48637475ed7d7733795ebeac55c5d58663714c60/app.py#L47-L48).

```Python
@app.route("/logout")
def logout():
    session.clear()  # Wipe out the user and the token cache from the session
    return redirect(  # Also need to log out from the Microsoft Identity platform
        "https://login.microsoftonline.com/common/oauth2/v2.0/logout"
        "?post_logout_redirect_uri=" + url_for("index", _external=True))
```

---

### <a name="intercepting-the-call-to-the-logout-endpoint"></a>Abfangen des Aufrufs an den `logout`-Endpunkt

Der URI nach der Abmeldung ermöglicht Anwendungen, an der globalen Abmeldung teilzunehmen.

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

Mit der OpenID Connect-Middleware von ASP.NET Core kann Ihre App den Aufruf an den `logout`-Endpunkt der Microsoft Identity Platform durch die Bereitstellung des OpenID Connect-Ereignisses `OnRedirectToIdentityProviderForSignOut` abfangen. Ein Beispiel für das Abonnieren dieses Ereignisses (um den Tokencache zu löschen) finden Sie unter [Microsoft.Identity.Web/WebAppServiceCollectionExtensions.cs#L151-L156](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/faa94fd49c2da46b22d6694c4f5c5895795af26d/Microsoft.Identity.Web/WebAppServiceCollectionExtensions.cs#L151-L156).

```csharp
    // Handling the global sign-out
    options.Events.OnRedirectToIdentityProviderForSignOut = async context =>
    {
        // Forget about the signed-in user
    };
```

# <a name="aspnet"></a>[ASP.NET](#tab/aspnet)

In ASP.NET delegieren Sie die Ausführung der Abmeldung an die Middleware, indem Sie das Sitzungscookie löschen:

```csharp
public class AccountController : Controller
{
 ...
 public void EndSession()
 {
  Request.GetOwinContext().Authentication.SignOut();
  Request.GetOwinContext().Authentication.SignOut(Microsoft.AspNet.Identity.DefaultAuthenticationTypes.ApplicationCookie);
  this.HttpContext.GetOwinContext().Authentication.SignOut(CookieAuthenticationDefaults.AuthenticationType);
 }
}
```

# <a name="java"></a>[Java](#tab/java)

Im Java-Schnellstart zeigt der Umleitungs-URI nach der Abmeldung nur die Seite „index.html“ an.

# <a name="python"></a>[Python](#tab/python)

Im Python-Schnellstart zeigt der Umleitungs-URI nach der Abmeldung nur die Seite „index.html“ an.

---

## <a name="protocol"></a>Protocol

Weitere Informationen zum Abmelden finden Sie in der Protokolldokumentation, die über [OpenID Connect](./v2-protocols-oidc.md) verfügbar ist.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Übergang in die Produktion](scenario-web-app-sign-user-production.md)
