<!-- markdownlint-disable MD025 -->

# **Tutorial: Creating a Reusable UI Component for Razor**

Below is a **step-by-step tutorial** that shows how to create a **reusable UI component in ASP.NET Core Razor** (Razor Pages or Razor Components).
This guide covers:

✔ Creating a Razor component
✔ Passing parameters
✔ Handling events
✔ Styling
✔ Reusing it across pages
✔ Optional: Using the component in Razor Pages

---

## 🔹 **1. What You Will Build**

A reusable **`AlertMessage`** component that displays messages such as success, info, warning, or error — similar to Bootstrap alerts.

---

## **2. Create the Razor Component**

In a **Blazor/Razor Components** (ASP.NET Core 8+) project, create a new file:

📁 `Components/AlertMessage.razor`

```razor
@* Components/AlertMessage.razor *@

<div class="alert @AlertCss" role="alert">
    @Message

    @if (Dismissable)
    {
        <button type="button" class="close" @onclick="OnDismissClicked">
            &times;
        </button>
    }
</div>

@code {
    [Parameter] public string Message { get; set; } = "";
    [Parameter] public string Type { get; set; } = "info"; // info, success, warning, danger
    [Parameter] public bool Dismissable { get; set; } = false;

    [Parameter] public EventCallback OnDismiss { get; set; }

    private string AlertCss =>
        Type.ToLower() switch
        {
            "success" => "alert-success",
            "warning" => "alert-warning",
            "danger"  => "alert-danger",
            _         => "alert-info"
        };

    private async Task OnDismissClicked()
    {
        await OnDismiss.InvokeAsync();
    }
}
```

### ✔ Features

* Fully reusable
* Accepts parameters
* Supports dismiss events
* Works with Bootstrap styling

---

## **3. Add CSS (Optional)**

If not using Bootstrap, add styling inside `wwwroot/css/site.css`:

```css
.alert {
    padding: 12px 16px;
    border-radius: 6px;
    margin-bottom: 12px;
}

.close {
    background: none;
    border: none;
    float: right;
    cursor: pointer;
    font-size: 20px;
}
```

---

## **4. Use the Component**

In any Razor component or Blazor page:

```razor
@page "/alerts"

<AlertMessage 
    Message="Operation completed!"
    Type="success"
    Dismissable="true"
    OnDismiss="HandleDismiss"
/>

<p>@Status</p>

@code {
    private string Status = "Alert visible";

    private void HandleDismiss()
    {
        Status = "Alert dismissed";
    }
}
```

---

## **5. Using the Component in Razor Pages (ASP.NET Core)**

If you're using **Razor Pages**, not Blazor, you can still use Razor Components starting in ASP.NET Core 8.

### In a Razor Page `.cshtml`

```razor
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@using YourApp.Components

<component type="typeof(AlertMessage)"
           render-mode="Static"
           param-Message="Server-side alert from Razor Pages"
           param-Type="warning" />
```

---

## **6. Make the Component Even More Reusable**

### Additional improvements you can add

| Feature      | How                                   |
| ------------ | ------------------------------------- |
| Custom icons | Add SVG or icon parameters            |
| Animation    | Add CSS transitions                   |
| Slots        | Add `RenderFragment` for custom body  |
| Auto-dismiss | Add a timer in `OnInitializedAsync()` |
| Localization | Inject `IStringLocalizer`             |

Example: Custom content slot

```razor
@* Add this to the component *@
[Parameter] public RenderFragment? ChildContent { get; set; }
```

And in the markup:

```razor
<div class="alert @AlertCss">
    @ChildContent
</div>
```

Usage:

```razor
<AlertMessage Type="danger">
    <strong>Error:</strong> Something went wrong.
</AlertMessage>
```

---

## **7. Folder Structure (Recommended)**

```
/Components
    AlertMessage.razor
/Pages
    Index.razor
    Alerts.razor
wwwroot/css/site.css
```

---

## **8. Summary**

By now you have a fully reusable Razor UI component that supports:

✔ Parameters
✔ Events
✔ Customizable styling
✔ Reuse in Blazor or Razor Pages

Great — let’s build a **more advanced Razor component**:
A **Modal Dialog** that supports:

✔ Header, body, footer slots
✔ Open/close callbacks
✔ Keyboard Escape close
✔ Backdrop click close
✔ Fully customizable content
✔ Animations (optional)
✔ Works in Razor Components *and* Razor Pages

This is a production-ready UI component.

---

# ⭐ **Advanced Reusable Razor Component: ModalDialog**

## 1. Create the Component File

📁 `Components/ModalDialog.razor`

```razor
@* Components/ModalDialog.razor *@

@if (Visible)
{
    <div class="modal-backdrop" @onclick="BackdropClicked">
        <div class="modal-container @AnimationClass" @onclick:stopPropagation="true">

            @if (Header != null)
            {
                <div class="modal-header">
                    @Header
                    @if (ShowCloseButton)
                    {
                        <button class="modal-close" @onclick="Close">×</button>
                    }
                </div>
            }

            <div class="modal-body">
                @Body
            </div>

            @if (Footer != null)
            {
                <div class="modal-footer">
                    @Footer
                </div>
            }
        </div>
    </div>
}

@code {
    // Slots
    [Parameter] public RenderFragment? Header { get; set; }
    [Parameter] public RenderFragment? Body { get; set; }
    [Parameter] public RenderFragment? Footer { get; set; }

    // State
    [Parameter] public bool Visible { get; set; }
    [Parameter] public EventCallback<bool> VisibleChanged { get; set; }

    [Parameter] public bool CloseOnEscape { get; set; } = true;
    [Parameter] public bool CloseOnBackdrop { get; set; } = true;
    [Parameter] public bool ShowCloseButton { get; set; } = true;

    // Animation
    [Parameter] public bool Animate { get; set; } = true;
    private string AnimationClass => Animate ? "modal-anim" : "";

    protected override void OnInitialized()
    {
        // Listen for ESC key
        if (CloseOnEscape)
        {
            var dotNetRef = DotNetObjectReference.Create(this);
            JS.InvokeVoidAsync("modalDialog.registerEsc", dotNetRef);
        }
    }

    [Inject] private IJSRuntime JS { get; set; } = default!;

    [JSInvokable]
    public async Task EscapePressed()
    {
        if (CloseOnEscape && Visible)
        {
            await Close();
        }
    }

    private async Task BackdropClicked()
    {
        if (CloseOnBackdrop)
            await Close();
    }

    public async Task Close()
    {
        Visible = false;
        await VisibleChanged.InvokeAsync(false);
    }
}
```

---

# 2. JavaScript for Escape Key

Create a JS file:

📁 `wwwroot/js/modalDialog.js`

```javascript
window.modalDialog = {
    registerEsc: function (dotNetObj) {
        document.addEventListener("keydown", function (e) {
            if (e.key === "Escape") {
                dotNetObj.invokeMethodAsync("EscapePressed");
            }
        });
    }
};
```

Add to layout:

```html
<script src="~/js/modalDialog.js"></script>
```

---

# 3. Add CSS (Styling + Animation)

📁 `wwwroot/css/modal.css`

```css
.modal-backdrop {
    position: fixed;
    inset: 0;
    background: rgba(0,0,0,0.5);
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 2000;
}

.modal-container {
    background: white;
    width: 450px;
    max-width: 95%;
    border-radius: 8px;
    box-shadow: 0 10px 25px rgba(0,0,0,0.2);
    overflow: hidden;
}

.modal-header, .modal-footer {
    padding: 16px;
    background: #f2f2f2;
}

.modal-body {
    padding: 16px;
}

.modal-close {
    float: right;
    background: none;
    border: none;
    font-size: 22px;
    cursor: pointer;
}

.modal-anim {
    animation: modal-enter 0.25s ease-out;
}

@keyframes modal-enter {
    from {
        opacity: 0;
        transform: translateY(-15px);
    }
    to {
        opacity: 1;
        transform: translateY(0);
    }
}
```

Add to layout:

```html
<link rel="stylesheet" href="~/css/modal.css" />
```

---

# 4. Use the Component Anywhere

Example:

```razor
@page "/example"

<button class="btn btn-primary" @onclick="ShowModal">Open Modal</button>

<ModalDialog Visible="@Show"
             VisibleChanged="@(v => Show = v)"
             CloseOnEscape="true"
             CloseOnBackdrop="true">

    <Header>
        <h3>Example Modal</h3>
    </Header>

    <Body>
        <p>This is a fully customizable modal dialog.</p>
        <p>You can put <strong>any Razor component</strong> here.</p>
    </Body>

    <Footer>
        <button class="btn btn-secondary" @onclick="CloseModal">Cancel</button>
        <button class="btn btn-primary" @onclick="Save">Save</button>
    </Footer>

</ModalDialog>

@code {
    private bool Show = false;

    void ShowModal() => Show = true;
    void CloseModal() => Show = false;

    void Save()
    {
        // Perform save logic
        Show = false;
    }
}
```

### Features working here

✔ 3 content slots (header/body/footer)
✔ Close via button
✔ Close via ESC
✔ Close via clicking backdrop
✔ Two-way binding for Visible

---

# 5. Using It in Razor Pages

In a `.cshtml` file:

```razor
<component type="typeof(ModalDialog)"
           render-mode="Server"
           param-Visible="Model.Show"
           param-VisibleChanged="Model.OnModalChanged" />
```

---

# ⭐ Advanced Reusable Component: `DataTable<T>`

A **fully reusable Razor Component Data Table** with:

✔ Sorting (ascending/descending)
✔ Automatic column creation
✔ Custom column templates
✔ Row click event
✔ Pagination (optional add-on)
✔ Works in Blazor / Razor Components / Razor Pages

This is a production-ready, generic data table using `RenderFragment<T>` templates.

---

## 1. Create the data table component

📁 `Components/DataTable.razor`

```razor
@typeparam TItem

<table class="data-table">
    <thead>
        <tr>
            @foreach (var col in Columns)
            {
                <th @onclick="() => SortBy(col)">
                    @col.Title

                    @if (SortColumn == col)
                    {
                        <span class="sort-indicator">
                            @(SortDescending ? "▼" : "▲")
                        </span>
                    }
                </th>
            }
        </tr>
    </thead>

    <tbody>
        @foreach (var item in SortedItems)
        {
            <tr @onclick="() => RowClicked.InvokeAsync(item)">
                @foreach (var col in Columns)
                {
                    <td>
                        @col.CellTemplate(item)
                    </td>
                }
            </tr>
        }
    </tbody>
</table>

@code {
    // INPUT DATA
    [Parameter] public IEnumerable<TItem>? Items { get; set; }

    // COLUMNS
    [Parameter] public RenderFragment? ChildContent { get; set; }

    internal List<DataTableColumn<TItem>> Columns { get; set; } = new();

    // SORTING STATE
    private DataTableColumn<TItem>? SortColumn;
    private bool SortDescending = false;

    // EVENTS
    [Parameter] public EventCallback<TItem> RowClicked { get; set; }

    private IEnumerable<TItem> SortedItems =>
        SortColumn == null || Items == null
            ? Items ?? Enumerable.Empty<TItem>()
            : (SortDescending
                ? Items.OrderByDescending(SortColumn.SortKeySelector)
                : Items.OrderBy(SortColumn.SortKeySelector)
              );

    protected override void OnParametersSet()
    {
        Columns.Clear();
        ChildContent?.Invoke(this);
    }

    internal void AddColumn(DataTableColumn<TItem> column)
    {
        Columns.Add(column);
    }

    private void SortBy(DataTableColumn<TItem> col)
    {
        if (SortColumn == col)
            SortDescending = !SortDescending;
        else
        {
            SortColumn = col;
            SortDescending = false;
        }
    }
}
```

---

# 2. Create a column definition component

📁 `Components/DataTableColumn.razor`

```razor
@typeparam TItem

@code {
    [CascadingParameter] internal DataTable<TItem> Table { get; set; } = default!;

    [Parameter] public string Title { get; set; } = "";
    [Parameter] public Func<TItem, object>? SortKey { get; set; }
    [Parameter] public RenderFragment<TItem>? Template { get; set; }

    protected override void OnInitialized()
    {
        Table.AddColumn(new DataTableColumn<TItem>
        {
            Title = Title,
            CellTemplate = Template!,
            SortKeySelector = SortKey ?? (_ => 0)
        });
    }
}

public class DataTableColumn<TItem>
{
    public string Title { get; set; } = "";
    public RenderFragment<TItem> CellTemplate { get; set; } = default!;
    public Func<TItem, object> SortKeySelector { get; set; } = default!;
}
```

---

# 3. Add CSS styling (sortable header, hover, etc.)

📁 `wwwroot/css/datatable.css`

```css
.data-table {
    width: 100%;
    border-collapse: collapse;
}

.data-table th {
    cursor: pointer;
    padding: 10px;
    background: #f2f2f2;
    user-select: none;
    text-align: left;
}

.data-table th:hover {
    background: #e2e2e2;
}

.data-table td {
    padding: 10px;
    border-top: 1px solid #ddd;
}

.data-table tr:hover {
    background: #fafafa;
}

.sort-indicator {
    margin-left: 5px;
    font-size: 0.8em;
}
```

Add to layout:

```html
<link rel="stylesheet" href="~/css/datatable.css" />
```

---

# 4. Use the Data Table

Create a model:

```csharp
public class Person 
{
    public string Name { get; set; } = "";
    public int Age { get; set; }
    public string City { get; set; } = "";
}
```

Use the table:

```razor
@page "/people"

<DataTable Items="@People" RowClicked="OnPersonSelected">
    <DataTableColumn TItem="Person" Title="Name" SortKey="@(p => p.Name)" Template="@(p => @<span>@p.Name</span>)" />
    <DataTableColumn TItem="Person" Title="Age" SortKey="@(p => p.Age)" Template="@(p => @<span>@p.Age</span>)" />
    <DataTableColumn TItem="Person" Title="City" SortKey="@(p => p.City)" Template="@(p => @<span>@p.City</span>)" />
</DataTable>

<p>@SelectedMessage</p>

@code {
    List<Person> People = new()
    {
        new Person { Name="Alice", Age=30, City="Boston" },
        new Person { Name="Bob", Age=22, City="Austin" },
        new Person { Name="Charlie", Age=27, City="Denver" }
    };

    string SelectedMessage = "";

    void OnPersonSelected(Person p)
    {
        SelectedMessage = $"Clicked: {p.Name}";
    }
}
```

---

# 5. Resulting Features

✔ Sort by any column
✔ Ascending/descending toggle
✔ Fully customizable cell templates
✔ Works with any model type
✔ Row click event
✔ Reusable across the entire app
✔ Separate styling

---

# 6. Optional Add-Ons

If you want, we can add:

## 🔹 Pagination

Client-side or server-side.

## 🔹 Filtering

Search bar, column filters, multi-column filters.

## 🔹 Column resizing / reordering

Advanced drag UI.

## 🔹 Sticky headers

## 🔹 Loading state

Skeleton UI while fetching.

## 🔹 Expandable rows

Accordion-style details.

## 🔹 Selectable rows

Checkboxes or radio selection.

---

A **full-featured, professional-grade Data Table component** with:

✔ Sorting
✔ Pagination
✔ Filtering
✔ Row selection
✔ Inline editing
✔ Expandable rows
✔ Infinite scroll
✔ Loading state
✔ Resizable + reorderable columns
✔ Sticky header

Below is the **step-by-step architecture**, **full component code**, and **usage examples**.

---

# ⭐ PART 1 — Base Component (Sorting + Structure)

You already have a solid foundation from the previous version.
We'll now upgrade it into a **DataGrid** system with multiple subcomponents.

---

# ⭐ PART 2 — Add Pagination

### New Parameters

```csharp
[Parameter] public int PageSize { get; set; } = 10;
[Parameter] public int Page { get; set; } = 1;
[Parameter] public EventCallback<int> PageChanged { get; set; }
```

### Add paginated list

```csharp
private IEnumerable<TItem> PagedItems =>
    SortedItems
        .Skip((Page - 1) * PageSize)
        .Take(PageSize);
```

### Add paging UI

Inside the main DataTable markup:

```razor
<div class="pagination">
    <button @onclick="PrevPage" disabled="@(!CanPrev)">Prev</button>
    <span>Page @Page</span>
    <button @onclick="NextPage" disabled="@(!CanNext)">Next</button>
</div>

@code {
    private bool CanPrev => Page > 1;
    private bool CanNext =>
        Items != null && (Page * PageSize) < Items.Count();

    private async Task PrevPage()
    {
        if (CanPrev)
        {
            Page--;
            await PageChanged.InvokeAsync(Page);
        }
    }

    private async Task NextPage()
    {
        if (CanNext)
        {
            Page++;
            await PageChanged.InvokeAsync(Page);
        }
    }
}
```

---

# ⭐ PART 3 — Add Filtering

### Add filter state

```csharp
[Parameter] public string FilterText { get; set; } = "";
[Parameter] public Func<TItem, string>? FilterBy { get; set; }
```

### Add filtering step

Modify `SortedItems`:

```csharp
private IEnumerable<TItem> FilteredItems =>
    string.IsNullOrWhiteSpace(FilterText) || FilterBy == null
        ? Items ?? Enumerable.Empty<TItem>()
        : Items!.Where(i => FilterBy(i)
            .Contains(FilterText, StringComparison.OrdinalIgnoreCase));

private IEnumerable<TItem> SortedItems =>
    SortColumn == null
        ? FilteredItems
        : SortDescending
            ? FilteredItems.OrderByDescending(SortColumn.SortKeySelector)
            : FilteredItems.OrderBy(SortColumn.SortKeySelector);
```

### Add filter UI

In the table header:

```razor
<input type="text" @bind="FilterText" placeholder="Search..." class="filter-box" />
```

---

# ⭐ PART 4 — Add Row Selection

This gives checkboxes + multi-select or single-select.

### Parameters

```csharp
[Parameter] public bool Selectable { get; set; }
[Parameter] public bool MultiSelect { get; set; } = true;

public HashSet<TItem> SelectedRows { get; private set; } = new();
```

### Add checkbox column

Inside `<thead>`:

```razor
@if (Selectable)
{
    <th><input type="checkbox" @onchange="ToggleSelectAll" /></th>
}
```

Inside `<tbody>`:

```razor
@if (Selectable)
{
    <td>
        <input type="checkbox" checked="@SelectedRows.Contains(item)" @onchange="() => ToggleItem(item)" />
    </td>
}
```

### Logic

```csharp
private void ToggleItem(TItem item)
{
    if (MultiSelect)
    {
        if (!SelectedRows.Add(item))
            SelectedRows.Remove(item);
    }
    else
    {
        SelectedRows.Clear();
        SelectedRows.Add(item);
    }
}
```

---

# ⭐ PART 5 — Inline Editing

Each cell becomes editable using a template.

### Column adds editable template

```csharp
[Parameter] public RenderFragment<TItem>? EditTemplate { get; set; }
[Parameter] public bool Editable { get; set; }
```

### Add IsEditing flag

```csharp
private TItem? EditingItem = default;
```

### Render cells

```razor
<td>
    @if (col.Editable && EditingItem != null && EqualityComparer<TItem>.Default.Equals(item, EditingItem))
    {
        @col.EditTemplate(item)
    }
    else
    {
        @col.CellTemplate(item)
    }
</td>
```

### Start editing

```razor
<button @onclick="() => EditingItem = item">Edit</button>
<button @onclick="() => SaveRow(item)">Save</button>
<button @onclick="CancelEdit">Cancel</button>
```

---

# ⭐ PART 6 — Expandable Rows

Accordion-style detail rows.

### Parameter

```csharp
[Parameter] public RenderFragment<TItem>? DetailTemplate { get; set; }
```

### Row expansion state

```csharp
private HashSet<TItem> Expanded = new();
private void ToggleExpand(TItem item)
{
    if (!Expanded.Add(item))
        Expanded.Remove(item);
}
```

### Markup

```razor
<tr @onclick="() => ToggleExpand(item)">
    <!-- normal row -->
</tr>

@if (Expanded.Contains(item))
{
    <tr class="expanded-row">
        <td colspan="@Columns.Count">
            @DetailTemplate(item)
        </td>
    </tr>
}
```

---

# ⭐ PART 7 — Infinite Scrolling

We add a scroll container and load more items when reaching bottom.

### Container

```razor
<div class="scroll-container" @onscroll="CheckScroll">
    <!-- table here -->
</div>
```

### Scroll handler

```csharp
private async Task CheckScroll(UIEventArgs e)
{
    var args = (UIEventArgs)e;

    // Pseudo-code: add JS interop for precise scroll metrics.
    if (/* near bottom */)
    {
        await LoadMore.InvokeAsync(null);
    }
}

[Parameter] public EventCallback LoadMore { get; set; }
```

You can use JS interop to detect scroll position.

---

# ⭐ PART 8 — Loading State

Add spinner or skeleton.

### Parameter

```csharp
[Parameter] public bool IsLoading { get; set; }
```

### Markup

```razor
@if (IsLoading)
{
    <div class="loading-overlay">
        Loading...
    </div>
}
```

---

# ⭐ PART 9 — Column Resizing & Reordering

This requires JavaScript interop to track drag events.

### Add to column definition

```csharp
public bool Resizable { get; set; }
public bool Reorderable { get; set; }
```

### Add resize handle in `<th>`

```razor
<div class="resize-handle" @onmousedown="(e) => StartResize(e, col)"></div>
```

### Add reorder drag

```razor
<th draggable="true"
    @ondragstart="(e) => StartDrag(e, col)"
    @ondrop="(e) => DropColumn(e, col)">
```

I can generate the full JS file too if you want it.

---

# ⭐ PART 10 — Sticky Header

CSS only:

```css
.data-table thead th {
    position: sticky;
    top: 0;
    background: #f2f2f2;
    z-index: 5;
}
```

---
