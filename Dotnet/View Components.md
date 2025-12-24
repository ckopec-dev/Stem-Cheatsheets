Below is a **comprehensive, end-to-end tutorial on C# View Components in ASP.NET Core**, written to be practical, production-oriented, and progressive. It starts from fundamentals and ends with real-world patterns like async data access, dependency injection, caching, theming, and packaging for reuse.

This tutorial assumes **ASP.NET Core MVC or Razor Pages (.NET 8/9/10)** and **C#**.

---

# Comprehensive Tutorial: C# View Components in ASP.NET Core

## 1. What Is a View Component?

A **View Component** is a reusable, server-side UI component in ASP.NET Core. Think of it as:

> **A mini-controller + view pair** that renders a chunk of UI.

### When to use View Components

Use a View Component when:

* UI needs **data + logic**
* Content is **reused across multiple pages**
* Partial Views are too simple
* Layouts need **dynamic data**
* You want **testable UI logic**

### View Components vs Partial Views

| Feature              | Partial View | View Component |
| -------------------- | ------------ | -------------- |
| Has C# logic         | ❌            | ✅              |
| Dependency Injection | ❌            | ✅              |
| Async support        | ❌            | ✅              |
| Testable             | ❌            | ✅              |
| Controller-like      | ❌            | ✅              |

---

## 2. Basic Structure of a View Component

A View Component has **two parts**:

1. **C# class** (logic)
2. **Razor view** (markup)

```
/ViewComponents
   └── WeatherViewComponent.cs

/Views
   └── Shared
       └── Components
           └── Weather
               └── Default.cshtml
```

---

## 3. Creating Your First View Component

### Step 1: Create the View Component Class

```csharp
using Microsoft.AspNetCore.Mvc;

public class WeatherViewComponent : ViewComponent
{
    public IViewComponentResult Invoke(string city)
    {
        var model = $"Weather for {city}";
        return View(model);
    }
}
```

### Naming rules

* Class must end with **ViewComponent**
* The component name is inferred:

  * `WeatherViewComponent` → `Weather`

---

### Step 2: Create the View

📁 **/Views/Shared/Components/Weather/Default.cshtml**

```razor
@model string

<div class="weather-widget">
    <strong>@Model</strong>
</div>
```

---

### Step 3: Invoke the View Component

#### Razor syntax

```razor
@await Component.InvokeAsync("Weather", new { city = "Chicago" })
```

#### Tag Helper syntax (recommended)

```razor
<vc:weather city="Chicago"></vc:weather>
```

> Tag Helpers are enabled automatically for View Components.

---

## 4. Using Async View Components (Best Practice)

Always prefer async.

```csharp
public class WeatherViewComponent : ViewComponent
{
    public async Task<IViewComponentResult> InvokeAsync(string city)
    {
        await Task.Delay(100); // simulate async work
        return View($"Weather for {city}");
    }
}
```

---

## 5. Passing a Strongly Typed Model

### Model

```csharp
public class WeatherModel
{
    public string City { get; set; }
    public int Temperature { get; set; }
}
```

### View Component

```csharp
public async Task<IViewComponentResult> InvokeAsync(string city)
{
    var model = new WeatherModel
    {
        City = city,
        Temperature = 72
    };

    return View(model);
}
```

### View

```razor
@model WeatherModel

<div>
    <h3>@Model.City</h3>
    <p>@Model.Temperature °F</p>
</div>
```

---

## 6. Dependency Injection in View Components

View Components fully support DI.

### Service

```csharp
public interface IWeatherService
{
    Task<int> GetTemperatureAsync(string city);
}
```

### Implementation

```csharp
public class WeatherService : IWeatherService
{
    public Task<int> GetTemperatureAsync(string city)
        => Task.FromResult(72);
}
```

### Register service

```csharp
builder.Services.AddScoped<IWeatherService, WeatherService>();
```

### Inject into View Component

```csharp
public class WeatherViewComponent : ViewComponent
{
    private readonly IWeatherService _weatherService;

    public WeatherViewComponent(IWeatherService weatherService)
    {
        _weatherService = weatherService;
    }

    public async Task<IViewComponentResult> InvokeAsync(string city)
    {
        var temp = await _weatherService.GetTemperatureAsync(city);

        return View(new WeatherModel
        {
            City = city,
            Temperature = temp
        });
    }
}
```

---

## 7. Multiple Views (Theming / Variants)

You can return a **specific view**.

```csharp
return View("Compact", model);
```

📁

```
Components/Weather/Default.cshtml
Components/Weather/Compact.cshtml
```

---

## 8. View Component Parameters & Validation

```csharp
public async Task<IViewComponentResult> InvokeAsync(
    string city,
    bool showTemperature = true)
{
    if (string.IsNullOrWhiteSpace(city))
        return Content("City not provided");

    var model = new WeatherModel
    {
        City = city,
        Temperature = showTemperature ? 72 : 0
    };

    return View(model);
}
```

Invocation:

```razor
<vc:weather city="Denver" show-temperature="false"></vc:weather>
```

---

## 9. Accessing HttpContext, User, Route Data

View Components inherit `ViewComponent`.

```csharp
var userName = User.Identity?.Name;
var path = HttpContext.Request.Path;
```

---

## 10. View Component Caching (Critical for Performance)

### Using IMemoryCache

```csharp
public class NewsViewComponent : ViewComponent
{
    private readonly IMemoryCache _cache;

    public NewsViewComponent(IMemoryCache cache)
    {
        _cache = cache;
    }

    public async Task<IViewComponentResult> InvokeAsync()
    {
        var news = await _cache.GetOrCreateAsync("latest-news", async entry =>
        {
            entry.AbsoluteExpirationRelativeToNow = TimeSpan.FromMinutes(5);
            return await LoadNewsAsync();
        });

        return View(news);
    }
}
```

---

## 11. Invoking View Components from Layouts

📁 **_Layout.cshtml**

```razor
<header>
    <vc:weather city="New York"></vc:weather>
</header>
```

Perfect for:

* Menus
* User profile widgets
* Notifications
* Footers

---

## 12. Unit Testing View Components

```csharp
[Fact]
public async Task WeatherComponent_Returns_View()
{
    var service = new Mock<IWeatherService>();
    service.Setup(x => x.GetTemperatureAsync(It.IsAny<string>()))
           .ReturnsAsync(70);

    var component = new WeatherViewComponent(service.Object);

    var result = await component.InvokeAsync("Austin");

    Assert.IsType<ViewViewComponentResult>(result);
}
```

---

## 13. View Components in Razor Pages

Works identically.

```razor
<vc:weather city="Seattle"></vc:weather>
```

---

## 14. Organizing View Components at Scale

Recommended structure:

```
/Features
   /Weather
      WeatherViewComponent.cs
      WeatherModel.cs
      /Views
         Default.cshtml
```

Then configure view discovery:

```csharp
builder.Services.AddControllersWithViews()
    .AddRazorOptions(options =>
    {
        options.ViewLocationFormats.Add(
            "/Features/{1}/Views/{0}.cshtml");
    });
```

---

## 15. Packaging View Components in a Razor Class Library (RCL)

Ideal for:

* Design systems
* Shared UI across apps
* Company-wide widgets

📦 Steps:

1. Create **Razor Class Library**
2. Add View Components + Views
3. Reference the RCL
4. Add static assets support

> This pairs perfectly with reusable UI libraries you’ve worked on before.

---

## 16. Common Mistakes

❌ Business logic in the view
❌ Synchronous `Invoke()` with I/O
❌ Overusing View Components for static HTML
❌ Not caching data-heavy components

---

## 17. When NOT to Use View Components

* Pure static UI → Partial View
* Heavy client-side interaction → Blazor / JS
* Form submission logic → Controllers

---

## 18. Summary

View Components:

* Are **server-side reusable UI**
* Support **DI, async, caching**
* Scale well in large applications
* Are ideal for **shared widgets & layouts**

They are one of ASP.NET Core’s **most underused power features**.


Below is a **real-world, production-grade View Component library** built the way you would actually ship and reuse it across multiple ASP.NET Core apps.

This goes beyond a demo and covers:

* **Razor Class Library (RCL)**
* **Multiple real components**
* **Strong typing**
* **DI, caching, theming**
* **Static assets**
* **Consumer app integration**
* **Scalable folder structure**

This mirrors how teams build internal design systems or shared UI platforms.

---

# Building a Real-World ASP.NET Core View Component Library

## 1. What We’re Building

A reusable UI library containing **real components**:

| Component   | Purpose                              |
| ----------- | ------------------------------------ |
| `Alert`     | Status messages (success/error/info) |
| `UserMenu`  | Auth-aware user profile menu         |
| `DataTable` | Sortable table                       |
| `Footer`    | Dynamic footer content               |

Packaged as:

* ✅ Razor Class Library
* ✅ Referenced by any MVC or Razor Pages app
* ✅ Supports theming & caching

---

## 2. Create the Razor Class Library

```bash
dotnet new razorclasslib -n Acme.Ui.Components
cd Acme.Ui.Components
```

Target framework (recommended):

```xml
<TargetFramework>net8.0</TargetFramework>
```

Enable MVC features:

```xml
<AddRazorSupportForMvc>true</AddRazorSupportForMvc>
```

---

## 3. Folder Structure (Production-Ready)

```
Acme.Ui.Components
│
├── Components
│   ├── Alert
│   │   ├── AlertViewComponent.cs
│   │   ├── AlertModel.cs
│   │   └── Views
│   │       └── Default.cshtml
│   │
│   ├── UserMenu
│   │   ├── UserMenuViewComponent.cs
│   │   └── Views
│   │       └── Default.cshtml
│   │
│   ├── DataTable
│   │   ├── DataTableViewComponent.cs
│   │   ├── DataTableModel.cs
│   │   └── Views
│   │       └── Default.cshtml
│
├── wwwroot
│   └── css
│       └── components.css
│
└── Extensions
    └── ServiceCollectionExtensions.cs
```

This structure scales cleanly to **dozens of components**.

---

## 4. Component #1 – Alert (Status Messages)

### Model

```csharp
namespace Acme.Ui.Components.Components.Alert;

public class AlertModel
{
    public string Message { get; set; } = "";
    public string Type { get; set; } = "info"; // success, danger, warning
}
```

---

### View Component

```csharp
using Microsoft.AspNetCore.Mvc;

namespace Acme.Ui.Components.Components.Alert;

public class AlertViewComponent : ViewComponent
{
    public IViewComponentResult Invoke(string message, string type = "info")
    {
        return View(new AlertModel
        {
            Message = message,
            Type = type
        });
    }
}
```

---

### View

📁 `/Components/Alert/Views/Default.cshtml`

```razor
@model AlertModel

<div class="alert alert-@Model.Type">
    @Model.Message
</div>
```

---

### Usage

```razor
<vc:alert message="Saved successfully" type="success"></vc:alert>
```

---

## 5. Component #2 – User Menu (Auth-Aware)

### View Component

```csharp
using Microsoft.AspNetCore.Mvc;

namespace Acme.Ui.Components.Components.UserMenu;

public class UserMenuViewComponent : ViewComponent
{
    public IViewComponentResult Invoke()
    {
        if (!User.Identity?.IsAuthenticated ?? true)
            return Content("");

        return View(User.Identity!.Name);
    }
}
```

---

### View

```razor
@model string

<div class="user-menu">
    Hello, <strong>@Model</strong>
    <a href="/Account/Logout">Logout</a>
</div>
```

---

### Usage (Layout)

```razor
<header>
    <vc:user-menu></vc:user-menu>
</header>
```

---

## 6. Component #3 – DataTable (Real-World UI)

### Model

```csharp
namespace Acme.Ui.Components.Components.DataTable;

public class DataTableModel<T>
{
    public IEnumerable<T> Items { get; set; } = [];
    public string[] Columns { get; set; } = [];
}
```

---

### View Component

```csharp
using Microsoft.AspNetCore.Mvc;

namespace Acme.Ui.Components.Components.DataTable;

public class DataTableViewComponent : ViewComponent
{
    public IViewComponentResult Invoke<T>(
        IEnumerable<T> items,
        string[] columns)
    {
        return View(new DataTableModel<T>
        {
            Items = items,
            Columns = columns
        });
    }
}
```

---

### View

```razor
@model dynamic

<table class="table">
    <thead>
        <tr>
            @foreach (var col in Model.Columns)
            {
                <th>@col</th>
            }
        </tr>
    </thead>
    <tbody>
        @foreach (var item in Model.Items)
        {
            <tr>
                @foreach (var col in Model.Columns)
                {
                    <td>@item.GetType().GetProperty(col)?.GetValue(item)</td>
                }
            </tr>
        }
    </tbody>
</table>
```

---

### Usage

```razor
<vc:data-table items="Model.Users"
               columns="new[] { \"Name\", \"Email\" }">
</vc:data-table>
```

---

## 7. Static Assets (CSS)

📁 `/wwwroot/css/components.css`

```css
.alert {
    padding: 10px;
    border-radius: 4px;
}

.alert-success { background: #d4edda; }
.alert-danger  { background: #f8d7da; }

.user-menu {
    float: right;
}
```

---

### Enable Static Assets in Consumer App

```csharp
app.UseStaticFiles();
```

Reference in `_Layout.cshtml`:

```html
<link rel="stylesheet" href="~/Acme.Ui.Components/css/components.css" />
```

---

## 8. Dependency Injection + Caching Example

```csharp
public class FooterViewComponent : ViewComponent
{
    private readonly IMemoryCache _cache;

    public FooterViewComponent(IMemoryCache cache)
    {
        _cache = cache;
    }

    public async Task<IViewComponentResult> InvokeAsync()
    {
        var year = _cache.GetOrCreate("footer-year", _ =>
        {
            _.AbsoluteExpirationRelativeToNow = TimeSpan.FromHours(1);
            return DateTime.UtcNow.Year;
        });

        return View(year);
    }
}
```

---

## 9. Service Registration Extension

📁 `Extensions/ServiceCollectionExtensions.cs`

```csharp
using Microsoft.Extensions.DependencyInjection;

namespace Acme.Ui.Components.Extensions;

public static class ServiceCollectionExtensions
{
    public static IServiceCollection AddAcmeUiComponents(
        this IServiceCollection services)
    {
        services.AddMemoryCache();
        return services;
    }
}
```

---

## 10. Consuming the Library

### Reference the project

```bash
dotnet add reference ../Acme.Ui.Components
```

### Register services

```csharp
builder.Services.AddAcmeUiComponents();
```

### Use anywhere

```razor
<vc:alert message="Welcome back!" type="info"></vc:alert>
<vc:user-menu></vc:user-menu>
```

---

## 11. Testing Strategy

* Unit test View Components (logic)
* Snapshot test rendered HTML
* Integration test with `WebApplicationFactory`

View Components are **far more testable than partial views**.

---

## 12. Why This Architecture Works

✅ Feature-based
✅ Strongly typed
✅ Async-friendly
✅ Scales to dozens of components
✅ Clean separation of UI logic
✅ Reusable across apps

This is the **same pattern used for internal UI platforms at scale**.

