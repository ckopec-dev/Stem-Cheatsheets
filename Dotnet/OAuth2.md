
# 🔐 Comprehensive OAuth 2.0 Tutorial for ASP.NET Core Razor Pages

## What you will build

A Razor Pages app that can:

✅ Sign users in with an OAuth provider
✅ Store tokens securely
✅ Refresh expired tokens
✅ Call a protected API
✅ Understand the full OAuth flow end-to-end

---

# 📘 Part 1 – OAuth 2.0 in plain English

OAuth 2.0 is not “login.”
OAuth 2.0 is **delegated authorization**.

Instead of:

> “Here is my password”

You get:

> “Here is a time-limited access token that allows this app to do X.”

### Core roles

| Role                 | Meaning                          |
| -------------------- | -------------------------------- |
| Resource Owner       | User                             |
| Client               | Your Razor app                   |
| Authorization Server | Google, GitHub, Entra ID, Auth0… |
| Resource Server      | The API you want to call         |

---

# 📘 Part 2 – Which flow should Razor Pages use?

For server-side web apps:

> ✅ **Authorization Code Flow (with PKCE)**

.NET already implements this securely.

---

# 📘 Part 3 – Create the Razor Pages project

```bash
dotnet new webapp -n RazorOAuthDemo
cd RazorOAuthDemo
```

Add secrets storage:

```bash
dotnet user-secrets init
```

---

# 📘 Part 4 – Register an OAuth application

You must register your app with a provider.

Examples:

• Google Cloud Console
• GitHub Developer Settings
• Microsoft Entra ID
• Auth0 / Okta

You will get:

```
ClientId
ClientSecret
Authorization endpoint
Token endpoint
Callback URL
```

Set redirect URI:

```
https://localhost:5001/signin-oauth
```

Store secrets:

```bash
dotnet user-secrets set "OAuth:ClientId" "xxx"
dotnet user-secrets set "OAuth:ClientSecret" "xxx"
```

---

# 📘 Part 5 – Add OAuth authentication to Razor

Edit `Program.cs`

```csharp
using Microsoft.AspNetCore.Authentication.Cookies;
using Microsoft.AspNetCore.Authentication.OAuth;
using System.Security.Claims;
using System.Text.Json;

var builder = WebApplication.CreateBuilder(args);

builder.Services.AddRazorPages();

builder.Services.AddAuthentication(options =>
{
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = "MyOAuth";
})
.AddCookie()
.AddOAuth("MyOAuth", options =>
{
    options.ClientId = builder.Configuration["OAuth:ClientId"]!;
    options.ClientSecret = builder.Configuration["OAuth:ClientSecret"]!;

    options.CallbackPath = "/signin-oauth";

    options.AuthorizationEndpoint = "https://provider.com/oauth/authorize";
    options.TokenEndpoint = "https://provider.com/oauth/token";
    options.UserInformationEndpoint = "https://provider.com/userinfo";

    options.Scope.Add("openid");
    options.Scope.Add("profile");
    options.Scope.Add("email");

    options.SaveTokens = true;

    options.ClaimActions.MapJsonKey(ClaimTypes.NameIdentifier, "id");
    options.ClaimActions.MapJsonKey(ClaimTypes.Name, "name");
    options.ClaimActions.MapJsonKey(ClaimTypes.Email, "email");

    options.Events = new OAuthEvents
    {
        OnCreatingTicket = async context =>
        {
            var request = new HttpRequestMessage(HttpMethod.Get, context.Options.UserInformationEndpoint);
            request.Headers.Authorization = 
                new System.Net.Http.Headers.AuthenticationHeaderValue("Bearer", context.AccessToken);

            var response = await context.Backchannel.SendAsync(request);
            response.EnsureSuccessStatusCode();

            using var user = JsonDocument.Parse(await response.Content.ReadAsStringAsync());
            context.RunClaimActions(user.RootElement);
        }
    };
});

var app = builder.Build();

app.UseHttpsRedirection();
app.UseStaticFiles();

app.UseAuthentication();
app.UseAuthorization();

app.MapRazorPages();
app.Run();
```

---

# 📘 Part 6 – Login & Logout

Create `/Pages/Login.cshtml.cs`

```csharp
using Microsoft.AspNetCore.Authentication;
using Microsoft.AspNetCore.Mvc;
using Microsoft.AspNetCore.Mvc.RazorPages;

public class LoginModel : PageModel
{
    public IActionResult OnGet(string? returnUrl = "/")
    {
        return Challenge(new AuthenticationProperties
        {
            RedirectUri = returnUrl
        }, "MyOAuth");
    }
}
```

Create `/Pages/Logout.cshtml.cs`

```csharp
using Microsoft.AspNetCore.Authentication;
using Microsoft.AspNetCore.Mvc;
using Microsoft.AspNetCore.Mvc.RazorPages;

public class LogoutModel : PageModel
{
    public async Task<IActionResult> OnGet()
    {
        await HttpContext.SignOutAsync();
        return Redirect("/");
    }
}
```

---

# 📘 Part 7 – Protect pages

```csharp
using Microsoft.AspNetCore.Authorization;

[Authorize]
public class SecureModel : PageModel
{
    public void OnGet() {}
}
```

Now users are redirected to OAuth automatically.

---

# 📘 Part 8 – Reading OAuth tokens

```csharp
var accessToken = await HttpContext.GetTokenAsync("access_token");
var refreshToken = await HttpContext.GetTokenAsync("refresh_token");
```

These are stored encrypted in the auth cookie.

---

# 📘 Part 9 – Calling a protected API

```csharp
var token = await HttpContext.GetTokenAsync("access_token");

var client = new HttpClient();
client.DefaultRequestHeaders.Authorization =
    new System.Net.Http.Headers.AuthenticationHeaderValue("Bearer", token);

var result = await client.GetStringAsync("https://api.provider.com/user");
```

---

# 📘 Part 10 – Token refresh (important)

Add:

```csharp
options.Scope.Add("offline_access");
```

Then refresh manually:

```csharp
public async Task<string> RefreshAsync(HttpContext context)
{
    var refreshToken = await context.GetTokenAsync("refresh_token");

    var response = await new HttpClient().PostAsync("https://provider.com/oauth/token",
        new FormUrlEncodedContent(new Dictionary<string,string>
        {
            ["grant_type"] = "refresh_token",
            ["refresh_token"] = refreshToken!,
            ["client_id"] = "...",
            ["client_secret"] = "..."
        }));

    var json = JsonDocument.Parse(await response.Content.ReadAsStringAsync());
    return json.RootElement.GetProperty("access_token").GetString()!;
}
```

⚠️ Many providers rotate refresh tokens — store updated values.

---

# 📘 Part 11 – Using built-in providers (simpler)

Example: GitHub

```csharp
builder.Services.AddAuthentication()
    .AddGitHub(options =>
    {
        options.ClientId = "...";
        options.ClientSecret = "...";
        options.Scope.Add("user:email");
    });
```

Others:

```
AddGoogle()
AddMicrosoftAccount()
AddFacebook()
AddOpenIdConnect()
```

---

# 📘 Part 12 – Common Razor mistakes

❌ Using OAuth as “just login”
❌ Not validating scopes
❌ Forgetting token refresh
❌ Storing tokens in JS
❌ Missing HTTPS
❌ Not handling denied consent

---

# 📘 Part 13 – OAuth vs OpenID Connect

| OAuth2        | OpenID Connect |
| ------------- | -------------- |
| Authorization | Authentication |
| Access tokens | ID tokens      |
| API access    | User identity  |

👉 Most real systems use **OIDC on top of OAuth2**

---

# 📘 Part 14 – Production checklist

✔ Use OpenID Connect when possible
✔ HTTPS only
✔ PKCE (default)
✔ Token encryption
✔ Short token lifetimes
✔ Scope minimization
✔ CSRF protection
✔ Provider-side logout
✔ Refresh rotation handling

---

# 📘 Part 15 – Recommended architecture

Razor Pages
→ OpenID Connect login
→ OAuth access tokens
→ Backend API
→ Policy-based authorization

---

# 🧠 Mental model

OAuth answers:

> “What is this app allowed to do?”

OpenID Connect answers:

> “Who is this user?”

