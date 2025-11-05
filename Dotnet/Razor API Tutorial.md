# 📝 Razor REST API with EF Core, JWT Authentication, and Swagger

## Table of Contents

1. [Project Setup](#project-setup)
2. [Models](#models)
3. [EF Core Setup](#ef-core-setup)
4. [REST API Endpoints](#rest-api-endpoints)
5. [Razor Pages Frontend](#razor-pages-frontend)
6. [Swagger Setup](#swagger-setup)
7. [Authentication with Identity & Roles](#authentication-with-identity--roles)
8. [JWT Authentication](#jwt-authentication)
9. [JWT in Swagger UI](#jwt-in-swagger-ui)
10. [JWT Auto-Refresh in Swagger](#jwt-auto-refresh-in-swagger)

---

## Project Setup

1. Install .NET SDK:

   ```bash
   dotnet --version
   ```

2. Create a new Razor Pages project:

   ```bash
   dotnet new webapp -o RazorRestApi
   cd RazorRestApi
   ```

3. Install necessary packages:

   ```bash
   dotnet add package Microsoft.EntityFrameworkCore
   dotnet add package Microsoft.EntityFrameworkCore.Sqlite
   dotnet add package Microsoft.EntityFrameworkCore.Tools
   dotnet add package Swashbuckle.AspNetCore
   dotnet add package Microsoft.AspNetCore.Identity.EntityFrameworkCore
   dotnet add package Microsoft.AspNetCore.Identity.UI
   dotnet add package Microsoft.AspNetCore.Authentication.JwtBearer
   dotnet add package System.IdentityModel.Tokens.Jwt
   ```

---

## Models

**`Models/TodoItem.cs`**:

```csharp
namespace RazorRestApi.Models
{
    public class TodoItem
    {
        public int Id { get; set; }
        public string? Title { get; set; }
        public bool IsDone { get; set; }
    }
}
```

**`Models/AuthModels.cs`**:

```csharp
namespace RazorRestApi.Models
{
    public record LoginRequest(string Username, string Password);
    public record AuthResponse(string Token, string RefreshToken, DateTime Expiration);
}
```

---

## EF Core Setup

**`Data/TodoContext.cs`**:

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore;
using RazorRestApi.Models;

namespace RazorRestApi.Data
{
    public class TodoContext : IdentityDbContext
    {
        public TodoContext(DbContextOptions<TodoContext> options) : base(options) { }

        public DbSet<TodoItem> Todos { get; set; } = default!;
    }
}
```

**`Program.cs` EF Core Configuration**:

```csharp
builder.Services.AddDbContext<TodoContext>(options =>
    options.UseSqlite(builder.Configuration.GetConnectionString("TodoConnection")));
```

**Connection String** (`appsettings.json`):

```json
"ConnectionStrings": {
    "TodoConnection": "Data Source=todos.db"
}
```

**Migrations**:

```bash
dotnet ef migrations add InitialCreate
dotnet ef database update
```

---

## REST API Endpoints

**Minimal API with CRUD** (`Program.cs`):

```csharp
app.MapGet("/api/todos", async (TodoContext db) =>
    await db.Todos.ToListAsync()).RequireAuthorization();

app.MapGet("/api/todos/{id:int}", async (int id, TodoContext db) =>
    await db.Todos.FindAsync(id) is TodoItem todo ? Results.Ok(todo) : Results.NotFound())
    .RequireAuthorization();

app.MapPost("/api/todos", async (TodoItem todo, TodoContext db) =>
{
    db.Todos.Add(todo);
    await db.SaveChangesAsync();
    return Results.Created($"/api/todos/{todo.Id}", todo);
}).RequireAuthorization("AdminOnly");

app.MapPut("/api/todos/{id:int}", async (int id, TodoItem updated, TodoContext db) =>
{
    var existing = await db.Todos.FindAsync(id);
    if (existing == null) return Results.NotFound();
    existing.Title = updated.Title;
    existing.IsDone = updated.IsDone;
    await db.SaveChangesAsync();
    return Results.NoContent();
}).RequireAuthorization("AdminOnly");

app.MapDelete("/api/todos/{id:int}", async (int id, TodoContext db) =>
{
    var todo = await db.Todos.FindAsync(id);
    if (todo == null) return Results.NotFound();
    db.Todos.Remove(todo);
    await db.SaveChangesAsync();
    return Results.NoContent();
}).RequireAuthorization("AdminOnly");
```

---

## Razor Pages Frontend

**Pages/Todos/Index.cshtml**:

```html
@page
@model RazorRestApi.Pages.Todos.IndexModel
@{
    ViewData["Title"] = "Todos";
}
<h1>Todo List</h1>
<form method="post">
    <input type="text" name="NewTodoTitle" placeholder="Enter a todo" />
    <button type="submit">Add</button>
</form>
<table>
<thead>
<tr><th>Title</th><th>Done?</th><th>Actions</th></tr>
</thead>
<tbody>
@foreach (var todo in Model.TodoList)
{
<tr>
    <td>@todo.Title</td>
    <td>@(todo.IsDone ? "✅" : "❌")</td>
    <td>
        <form method="post" asp-page-handler="Toggle" asp-route-id="@todo.Id" asp-route-isDone="@todo.IsDone" style="display:inline">
            <button type="submit">Toggle</button>
        </form>
        <form method="post" asp-page-handler="Delete" asp-route-id="@todo.Id" style="display:inline">
            <button type="submit">Delete</button>
        </form>
    </td>
</tr>
}
</tbody>
</table>
```

**Pages/Todos/Index.cshtml.cs**:

```csharp
using Microsoft.AspNetCore.Mvc;
using Microsoft.AspNetCore.Mvc.RazorPages;
using RazorRestApi.Models;
using RazorRestApi.Services;

[Authorize]
public class IndexModel : PageModel
{
    private readonly TodoApiClient _apiClient;
    public IndexModel(TodoApiClient apiClient) => _apiClient = apiClient;
    public List<TodoItem> TodoList { get; set; } = new();
    [BindProperty] public string? NewTodoTitle { get; set; }

    public async Task OnGetAsync() => TodoList = await _apiClient.GetTodosAsync();
    public async Task<IActionResult> OnPostAsync()
    {
        if (!string.IsNullOrWhiteSpace(NewTodoTitle))
            await _apiClient.AddTodoAsync(NewTodoTitle);
        return RedirectToPage();
    }
    public async Task<IActionResult> OnPostToggleAsync(int id, bool isDone)
    {
        await _apiClient.ToggleTodoAsync(id, !isDone);
        return RedirectToPage();
    }
    public async Task<IActionResult> OnPostDeleteAsync(int id)
    {
        await _apiClient.DeleteTodoAsync(id);
        return RedirectToPage();
    }
}
```

**TodoApiClient.cs**:

```csharp
using System.Net.Http.Json;
using RazorRestApi.Models;

public class TodoApiClient
{
    private readonly HttpClient _http;
    public TodoApiClient(HttpClient http) => _http = http;

    public async Task<List<TodoItem>> GetTodosAsync() =>
        await _http.GetFromJsonAsync<List<TodoItem>>("/api/todos") ?? new List<TodoItem>();

    public async Task<TodoItem?> AddTodoAsync(string title)
    {
        var todo = new TodoItem { Title = title, IsDone = false };
        var response = await _http.PostAsJsonAsync("/api/todos", todo);
        return await response.Content.ReadFromJsonAsync<TodoItem>();
    }

    public async Task ToggleTodoAsync(int id, bool isDone) =>
        await _http.PutAsJsonAsync($"/api/todos/{id}", new TodoItem { Id = id, IsDone = isDone });

    public async Task DeleteTodoAsync(int id) =>
        await _http.DeleteAsync($"/api/todos/{id}");
}
```

---

## Swagger Setup

**Swagger in Program.cs**:

```csharp
builder.Services.AddSwaggerGen(c =>
{
    c.SwaggerDoc("v1", new() { Title = "RazorRestApi", Version = "v1" });
    c.AddSecurityDefinition("Bearer", new Microsoft.OpenApi.Models.OpenApiSecurityScheme
    {
        Name = "Authorization",
        Type = Microsoft.OpenApi.Models.SecuritySchemeType.ApiKey,
        Scheme = "Bearer",
        BearerFormat = "JWT",
        In = Microsoft.OpenApi.Models.ParameterLocation.Header,
        Description = "Enter 'Bearer' [space] and then your JWT token."
    });
    c.AddSecurityRequirement(new Microsoft.OpenApi.Models.OpenApiSecurityRequirement
    {
        {
            new Microsoft.OpenApi.Models.OpenApiSecurityScheme
            {
                Reference = new Microsoft.OpenApi.Models.OpenApiReference
                {
                    Type = Microsoft.OpenApi.Models.ReferenceType.SecurityScheme,
                    Id = "Bearer"
                }
            },
            new string[] {}
        }
    });
});
```

---

## Authentication with Identity & Roles

* Configure Identity in `Program.cs`:

```csharp
builder.Services.AddIdentity<IdentityUser, IdentityRole>()
    .AddEntityFrameworkStores<TodoContext>()
    .AddDefaultTokenProviders()
    .AddDefaultUI();
```

* Seed Admin user and roles:

**SeedData.cs**:

```csharp
public static async Task InitializeAsync(IServiceProvider services)
{
    var roleManager = services.GetRequiredService<RoleManager<IdentityRole>>();
    var userManager = services.GetRequiredService<UserManager<IdentityUser>>();
    if (!await roleManager.RoleExistsAsync("Admin")) await roleManager.CreateAsync(new IdentityRole("Admin"));
    var adminEmail = "admin@example.com";
    var adminUser = await userManager.FindByEmailAsync(adminEmail);
    if (adminUser == null)
    {
        adminUser = new IdentityUser { UserName = adminEmail, Email = adminEmail, EmailConfirmed = true };
        await userManager.CreateAsync(adminUser, "Admin123$");
        await userManager.AddToRoleAsync(adminUser, "Admin");
    }
}
```

---

## JWT Authentication

**Add JWT in Program.cs**:

```csharp
var key = Encoding.UTF8.GetBytes(builder.Configuration["Jwt:Key"]!);
builder.Services

.AddAuthentication(options =>
{
options.DefaultAuthenticateScheme = JwtBearerDefaults.AuthenticationScheme;
options.DefaultChallengeScheme = JwtBearerDefaults.AuthenticationScheme;
})
.AddJwtBearer(options =>
{
options.TokenValidationParameters = new TokenValidationParameters
{
ValidateIssuer = true,
ValidateAudience = true,
ValidateLifetime = true,
ValidateIssuerSigningKey = true,
ValidIssuer = builder.Configuration\["Jwt\:Issuer"],
ValidAudience = builder.Configuration\["Jwt\:Audience"],
IssuerSigningKey = new SymmetricSecurityKey(key)
};
});
```

**AuthEndpoints.cs**:

```csharp
public static void MapAuthEndpoints(this IEndpointRouteBuilder app, IConfiguration config)
{
    app.MapPost("/api/login", async (LoginRequest request, UserManager<IdentityUser> userManager, IConfiguration config) =>
    {
        var user = await userManager.FindByNameAsync(request.Username);
        if (user == null || !await userManager.CheckPasswordAsync(user, request.Password)) return Results.Unauthorized();
        var response = GenerateTokens(user, config);
        return Results.Ok(response);
    });

    app.MapPost("/api/refresh", (string refreshToken, IConfiguration config) =>
    {
        if (!RefreshTokens.TryGetValue(refreshToken, out var username)) return Results.Unauthorized();
        var user = new IdentityUser { UserName = username };
        var response = GenerateTokens(user, config);
        return Results.Ok(response);
    });
}
````

---

## JWT in Swagger UI

* Inject JWT into Swagger requests:

**wwwroot/swagger-ui/custom-swagger.js**:

```javascript
(function () {
    window.ui.getConfigs().requestInterceptor = function (req) {
        const token = localStorage.getItem("jwt_token");
        if (token) req.headers["Authorization"] = "Bearer " + token;
        return req;
    };
    window.ui.getConfigs().responseInterceptor = function (res) {
        if (res.url.includes("/api/login") && res.status === 200) {
            const data = JSON.parse(res.text);
            localStorage.setItem("jwt_token", data.token);
            localStorage.setItem("jwt_expiration", data.expiration);
            localStorage.setItem("refresh_token", data.refreshToken);
        }
        return res;
    };
})();
```

---

## JWT Auto-Refresh in Swagger

* Automatically refresh JWT when expired:

```javascript
async function refreshJwtToken(refreshToken) {
    const res = await fetch("/api/refresh", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(refreshToken)
    });
    if (!res.ok) return null;
    return await res.json();
}

window.ui.getConfigs().requestInterceptor = async function (req) {
    let token = localStorage.getItem("jwt_token");
    let expiration = localStorage.getItem("jwt_expiration");
    let refreshToken = localStorage.getItem("refresh_token");

    if (token && expiration) {
        if (new Date(expiration) <= new Date() && refreshToken) {
            const refreshed = await refreshJwtToken(refreshToken);
            if (refreshed) {
                localStorage.setItem("jwt_token", refreshed.token);
                localStorage.setItem("jwt_expiration", refreshed.expiration);
                localStorage.setItem("refresh_token", refreshed.refreshToken);
                token = refreshed.token;
            }
        }
    }

    if (token) req.headers["Authorization"] = "Bearer " + token;
    return req;
};
```
