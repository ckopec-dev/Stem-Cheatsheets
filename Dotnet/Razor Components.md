<!-- markdownlint-disable MD025 -->

# Blazor Razor Components Tutorial

A comprehensive guide to building interactive web applications with Razor components in Blazor.

## What are Razor Components?

Razor components are reusable UI building blocks in Blazor that combine C# code with HTML markup using the Razor syntax. They allow you to create interactive web applications using .NET instead of JavaScript.

## Getting Started

**Prerequisites:**
- .NET 6.0 or later
- Visual Studio 2022, VS Code, or any preferred IDE
- Basic knowledge of C# and HTML

**Creating a New Blazor Project:**

For Blazor Server:

```bash
dotnet new blazorserver -o MyBlazorApp
cd MyBlazorApp
dotnet run
```

For Blazor WebAssembly:

```bash
dotnet new blazorwasm -o MyBlazorApp
```

## Basic Component Structure

**Your First Component**

Create a file named `Counter.razor` in the `Components` or `Pages` folder:

```razor
<h3>Counter Component</h3>

<p>Current count: @currentCount</p>

<button @onclick="IncrementCount">Click me</button>

@code {
    private int currentCount = 0;

    private void IncrementCount()
    {
        currentCount++;
    }
}
```

This component demonstrates the three main parts of a Razor component: HTML markup with Razor syntax (the `@` symbol), event handling (`@onclick`), and a code block (`@code { }`).

## Component Parameters

Components can accept parameters to make them reusable:

```razor
<!-- GreetingCard.razor -->
<div class="card">
    <h3>Hello, @Name!</h3>
    <p>@Message</p>
</div>

@code {
    [Parameter]
    public string Name { get; set; } = "Guest";
    
    [Parameter]
    public string Message { get; set; } = "Welcome!";
}
```

Usage in another component:

```razor
<GreetingCard Name="Alice" Message="Great to see you!" />
<GreetingCard Name="Bob" />
```

## Data Binding

**One-Way Binding**

Display data from your code:

```razor
<p>The time is: @currentTime</p>

@code {
    private string currentTime = DateTime.Now.ToLongTimeString();
}
```

**Two-Way Binding**

Use `@bind` for form inputs:

```razor
<input @bind="userName" />
<p>Hello, @userName!</p>

@code {
    private string userName = "";
}
```

**Binding with Events**

```razor
<input @bind="searchTerm" @bind:event="oninput" />
<p>Searching for: @searchTerm</p>

@code {
    private string searchTerm = "";
}
```

## Event Handling

**Common Events**

```razor
<button @onclick="HandleClick">Click</button>
<input @onchange="HandleChange" />
<input @oninput="HandleInput" />
<form @onsubmit="HandleSubmit">
    <button type="submit">Submit</button>
</form>

@code {
    private void HandleClick()
    {
        // Handle click
    }
    
    private void HandleChange(ChangeEventArgs e)
    {
        var value = e.Value?.ToString();
    }
    
    private void HandleInput(ChangeEventArgs e)
    {
        // Real-time input handling
    }
    
    private void HandleSubmit()
    {
        // Handle form submission
    }
}
```

**Passing Parameters to Event Handlers**

```razor
@foreach (var item in items)
{
    <button @onclick="() => DeleteItem(item.Id)">
        Delete @item.Name
    </button>
}

@code {
    private List<Item> items = new();
    
    private void DeleteItem(int id)
    {
        items.RemoveAll(i => i.Id == id);
    }
}
```

## Component Lifecycle

Razor components have lifecycle methods you can override:

```razor
@implements IDisposable

<p>Component content</p>

@code {
    protected override void OnInitialized()
    {
        // Called when component is initialized
        // Runs once
    }
    
    protected override async Task OnInitializedAsync()
    {
        // Async version - useful for loading data
        await LoadDataAsync();
    }
    
    protected override void OnParametersSet()
    {
        // Called when parameters are set or changed
    }
    
    protected override async Task OnParametersSetAsync()
    {
        // Async version
    }
    
    protected override void OnAfterRender(bool firstRender)
    {
        // Called after component has rendered
        if (firstRender)
        {
            // Runs only on first render
        }
    }
    
    protected override async Task OnAfterRenderAsync(bool firstRender)
    {
        // Async version - useful for JS interop
    }
    
    public void Dispose()
    {
        // Clean up resources
    }
}
```

## Conditional Rendering

**If Statements**

```razor
@if (isLoggedIn)
{
    <p>Welcome back!</p>
}
else
{
    <p>Please log in.</p>
}

@code {
    private bool isLoggedIn = false;
}
```

**Switch Expressions**

```razor
<p>Status: @status</p>
<div class="@GetStatusClass()">
    @status
</div>

@code {
    private string status = "pending";
    
    private string GetStatusClass() => status switch
    {
        "success" => "alert-success",
        "error" => "alert-danger",
        "pending" => "alert-warning",
        _ => "alert-info"
    };
}
```

## Loops and Lists

```razor
<ul>
    @foreach (var todo in todos)
    {
        <li>
            <input type="checkbox" @bind="todo.IsCompleted" />
            @todo.Title
        </li>
    }
</ul>

@code {
    private List<TodoItem> todos = new()
    {
        new TodoItem { Id = 1, Title = "Learn Blazor", IsCompleted = false },
        new TodoItem { Id = 2, Title = "Build an app", IsCompleted = false }
    };
    
    public class TodoItem
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public bool IsCompleted { get; set; }
    }
}
```

## Child Components and EventCallback

**Parent Component**

```razor
<TodoList OnTodoAdded="HandleTodoAdded" />

<p>Total todos: @todoCount</p>

@code {
    private int todoCount = 0;
    
    private void HandleTodoAdded()
    {
        todoCount++;
    }
}
```

**Child Component**

```razor
<!-- TodoList.razor -->
<input @bind="newTodo" />
<button @onclick="AddTodo">Add</button>

@code {
    [Parameter]
    public EventCallback OnTodoAdded { get; set; }
    
    private string newTodo = "";
    
    private async Task AddTodo()
    {
        if (!string.IsNullOrWhiteSpace(newTodo))
        {
            await OnTodoAdded.InvokeAsync();
            newTodo = "";
        }
    }
}
```

## Form Validation

```razor
<EditForm Model="person" OnValidSubmit="HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />
    
    <div>
        <label>Name:</label>
        <InputText @bind-Value="person.Name" />
        <ValidationMessage For="() => person.Name" />
    </div>
    
    <div>
        <label>Email:</label>
        <InputText @bind-Value="person.Email" />
        <ValidationMessage For="() => person.Email" />
    </div>
    
    <div>
        <label>Age:</label>
        <InputNumber @bind-Value="person.Age" />
        <ValidationMessage For="() => person.Age" />
    </div>
    
    <button type="submit">Submit</button>
</EditForm>

@code {
    private Person person = new();
    
    private void HandleValidSubmit()
    {
        // Process valid form
    }
    
    public class Person
    {
        [Required]
        [StringLength(50)]
        public string Name { get; set; }
        
        [Required]
        [EmailAddress]
        public string Email { get; set; }
        
        [Range(0, 120)]
        public int Age { get; set; }
    }
}
```

## Dependency Injection

**Register a Service**

In `Program.cs`:

```csharp
builder.Services.AddScoped<IWeatherService, WeatherService>();
```

**Use in Component**

```razor
@inject IWeatherService WeatherService

<h3>Weather Forecast</h3>

@if (forecasts == null)
{
    <p>Loading...</p>
}
else
{
    <ul>
        @foreach (var forecast in forecasts)
        {
            <li>@forecast.Date: @forecast.Temperature°C</li>
        }
    </ul>
}

@code {
    private List<WeatherForecast>? forecasts;
    
    protected override async Task OnInitializedAsync()
    {
        forecasts = await WeatherService.GetForecastAsync();
    }
}
```

## Component CSS Isolation

Create a file named `Counter.razor.css` alongside `Counter.razor`:

```css
/* Counter.razor.css */
h3 {
    color: blue;
    font-size: 24px;
}

button {
    background-color: #0066cc;
    color: white;
    padding: 10px 20px;
    border: none;
    border-radius: 4px;
}

button:hover {
    background-color: #0052a3;
}
```

These styles will automatically scope to only the Counter component.

## Best Practices

1. Keep components small and focused on a single responsibility
2. Use parameters to make components reusable
3. Leverage dependency injection for services and state management
4. Implement proper error handling with try-catch blocks
5. Use async/await for data loading and API calls
6. Dispose of resources properly in IDisposable
7. Optimize re-renders by using `ShouldRender()` when needed
8. Use CSS isolation to prevent style conflicts
9. Validate user input with data annotations
10. Follow naming conventions (PascalCase for components and parameters)

## Next Steps

- Explore Blazor routing and navigation
- Learn about state management with Fluxor or similar libraries
- Implement authentication and authorization
- Work with JavaScript interop for browser APIs
- Deploy your Blazor application to production

Happy coding with Blazor Razor components!
