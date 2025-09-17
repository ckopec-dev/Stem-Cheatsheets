
# Creating a Windows Service in C\#

A Windows Service is a long-running executable that runs in its own Windows session. Services can start automatically when the computer boots, run in the background, and do not require user interaction.

## Prerequisites

* Windows machine
* Visual Studio (Community Edition works fine)
* Basic knowledge of C#

---

## Step 1: Create a Windows Service Project

1. Open **Visual Studio**.
2. Click **Create a new project**.
3. Select **Windows Service (.NET Framework)** or **Worker Service (.NET Core / .NET 6+)** depending on your preference.

   * For .NET Framework: “Windows Service (.NET Framework)” template.
   * For .NET Core/6+: “Worker Service” template.
4. Give your project a name (e.g., `MyWindowsService`) and click **Create**.

---

## Step 2: Implement the Service Logic

Open the `Service1.cs` file (or your service class). Modify the `OnStart` and `OnStop` methods to define what the service does.

```csharp
using System;
using System.ServiceProcess;
using System.Timers;

namespace MyWindowsService
{
    public partial class Service1 : ServiceBase
    {
        private Timer _timer;

        public Service1()
        {
            InitializeComponent();
        }

        protected override void OnStart(string[] args)
        {
            // Log that the service has started
            EventLog.WriteEntry("MyWindowsService started.");

            // Setup a timer to trigger every 10 seconds
            _timer = new Timer();
            _timer.Interval = 10000; // 10 seconds
            _timer.Elapsed += OnTimerElapsed;
            _timer.Start();
        }

        private void OnTimerElapsed(object sender, ElapsedEventArgs e)
        {
            // Example: Write a timestamp to the event log every 10 seconds
            EventLog.WriteEntry($"Service is running at {DateTime.Now}");
        }

        protected override void OnStop()
        {
            _timer.Stop();
            EventLog.WriteEntry("MyWindowsService stopped.");
        }
    }
}
```

---

## Step 3: Add an Installer (For .NET Framework)

To install the service, you need a Service Installer:

1. Right-click the **Service1.cs** file → **View Designer**.
2. Right-click the designer → **Add Installer**.
3. This creates a `ProjectInstaller.cs` file.
4. Configure:

   * **ServiceName**: `MyWindowsService`
   * **StartType**: `Automatic` or `Manual`

The installer generates the code needed for installing the service with `InstallUtil.exe`.

---

## Step 4: Build the Service

* Build the project (`Ctrl+Shift+B`) to produce an executable.
* The output will be in `bin\Debug` or `bin\Release` depending on your build configuration.

---

## Step 5: Install the Service

**Using Command Prompt as Administrator:**

```cmd
cd "C:\Path\To\Your\Service\bin\Debug"
InstallUtil MyWindowsService.exe
```

* To uninstall:

```cmd
InstallUtil /u MyWindowsService.exe
```

**Note:** For .NET Core/6+ Worker Services, use `sc create`:

```cmd
sc create MyWindowsService binPath= "C:\Path\To\MyWindowsService.exe"
sc start MyWindowsService
```

---

## Step 6: Verify the Service

1. Open **Services** (Win + R → `services.msc`).
2. Find your service (`MyWindowsService`) in the list.
3. Start/Stop the service from the UI, or check logs in **Event Viewer**.

---

## Step 7: Debugging

* Windows Services cannot be directly run in Visual Studio. To debug:

  1. Add a temporary console runner:

```csharp
#if DEBUG
    Service1 service = new Service1();
    service.OnStart(null);
    Console.WriteLine("Press any key to stop...");
    Console.ReadKey();
    service.OnStop();
#else
    ServiceBase.Run(new Service1());
#endif
```

* Or attach the debugger to the running service process.

---

## Summary

1. Create a Windows Service project.
2. Implement `OnStart` and `OnStop` methods.
3. Add an installer (if using .NET Framework).
4. Build the service.
5. Install and start the service.
6. Debug using a console wrapper or attach to process.

---

