
# Dotnet Cheatsheet

## Installing on Ubuntu 22.04

~~~bash
$ wget https://dot.net/v1/dotnet-install.sh -O dotnet-install.sh

$ chmod +x ./dotnet-install.sh

$ ./dotnet-install.sh --version latest

# Add the following line to the bottom of .bashrc:

export PATH="$PATH:/home/<!username!>/.dotnet

# Relog
~~~

## Using with VS Code on Ubuntu 22.04

- Make sure dotnet is installed via: $ dotnet --version
- Install c# dev kit extension

## Create a desktop app on Ubuntu

~~~bash
sudo apt-get install libgtk-3-dev
mkdir HelloWorldGtk
cd HelloWorldGtk

nano HelloWorldGtk.csproj
~~~

~~~xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net8.0</TargetFramework>
    <Nullable>enable</Nullable>
    <PublishAot>false</PublishAot>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="GtkSharp" Version="3.24.24.95" />
  </ItemGroup>

</Project>
~~~

`nano Program.cs`

~~~csharp
using System;
using Gtk;

namespace HelloWorldGtkApp
{
    public class MainWindow : Window
    {
        private readonly Label _lblHello;
        private readonly Button _btnClick;
        private readonly Box _vbox;

        public MainWindow() : base("Hello World GTK .NET 8")
        {
            // Set window properties
            SetDefaultSize(400, 200);
            SetPosition(WindowPosition.Center);
            Resizable = false;
            
            // Handle window close event
            DeleteEvent += OnDeleteEvent;

            // Create vertical box container with spacing
            _vbox = new Box(Orientation.Vertical, 20)
            {
                MarginTop = 30,
                MarginBottom = 30,
                MarginStart = 30,
                MarginEnd = 30
            };

            // Create Hello World label
            _lblHello = new Label("Hello, World from .NET 8!")
            {
                Halign = Align.Center,
                Valign = Align.Center
            };
            
            // Style the label with CSS
            var cssProvider = new CssProvider();
            cssProvider.LoadFromData("label { font-size: 18px; font-weight: bold; color: #2563eb; }");
            _lblHello.StyleContext.AddProvider(cssProvider, StyleProviderPriority.Application);

            // Create click button
            _btnClick = new Button("Click Me!")
            {
                Halign = Align.Center,
                WidthRequest = 120,
                HeightRequest = 40
            };
            _btnClick.Clicked += OnButtonClicked;

            // Add widgets to the vertical box
            _vbox.PackStart(_lblHello, true, true, 0);
            _vbox.PackStart(_btnClick, false, false, 0);

            // Add the vbox to the window
            Add(_vbox);

            // Show all widgets
            ShowAll();
        }

        private void OnButtonClicked(object? sender, EventArgs e)
        {
            // Create and show a message dialog
            using var dialog = new MessageDialog(
                this,
                DialogFlags.Modal,
                MessageType.Info,
                ButtonsType.Ok,
                "Hello from GTK with .NET 8 on Ubuntu!\n\nThis is running natively on Linux using modern .NET!"
            )
            {
                Title = "Greeting from .NET 8"
            };
            
            dialog.Run();
            dialog.Destroy();
        }

        private void OnDeleteEvent(object sender, DeleteEventArgs e)
        {
            // Exit the application when window is closed
            Application.Quit();
            e.RetVal = true;
        }
    }

    class Program
    {
        public static void Main(string[] args)
        {
            // Initialize GTK
            Application.Init();

            // Create and show the main window
            using var window = new MainWindow();

            // Run the GTK main loop
            Application.Run();
        }
    }
}
~~~

~~~bash
dotnet restore
dotnet run
~~~

## Adding secret environment variables to IIS

To add secret environment variables for an ASP.NET Core application hosted in IIS, follow these steps:

~~~json
// Example appsettings.json (strongly typed class not shown)
{
  "MySettings": {
    "ApiKey": "default_api_key"
  }
}
~~~

- **Open IIS Manager:** Navigate to your application's site in IIS Manager.
- **Access Configuration Editor:** In the "Management" section, double-click on "Configuration Editor."
- **Select System.webServer/aspNetCore:** From the "Section" dropdown, select system.webServer/aspNetCore.
- **Edit Environment Variables:**
  - Locate the environmentVariables collection.
  - Click the ellipsis (...) next to the (Collection) value to open the collection editor.
- **Add Secret Variables:**
  - Click "Add" in the right panel.
  - Enter the name of your environment variable (MySettings__ApiKey).
  - Enter the value of your secret (e.g., MySuperSecretValue123).
- Repeat for all necessary secret variables.
- **Apply Changes:** Close the collection editor and click "Apply" in the Actions pane of the Configuration Editor to save the changes.
- **Recycle the Application Pool:** After adding or modifying environment variables in IIS, you must recycle the application pool associated with your ASP.NET Core application for the changes to take effect. You can do this by right-clicking on the application pool and selecting Recycle.
