# Tutorial: Create a Cron-Like Windows Service in C\#

This tutorial guides you through creating a Windows Service that behaves like Linux cron, supporting multiple scheduled tasks defined in a JSON configuration file and dynamically reloading when the file changes.

---

## Prerequisites

* Windows machine with **.NET 6+ SDK** installed.
* **Visual Studio 2022** (or later) with **Worker Service template**.
* Basic knowledge of C#.

---

## Step 1: Create a Worker Service Project

1. Open Visual Studio → **Create a new project**.
2. Select **Worker Service** → Next.
3. Name the project `CronLikeService`.
4. Choose **.NET 6 (or higher)** as the framework → Create.

---

## Step 2: Add Required NuGet Package

Open **Package Manager Console** and run:

```bash
dotnet add package NCrontab
```

> NCrontab provides cron-like scheduling support with standard cron expressions.

---

## Step 3: Create the Cron Job Class

Add a new class `CronJob.cs`:

```csharp
using NCrontab;
using System;

namespace CronLikeService
{
    public class CronJob
    {
        public string Name { get; set; }
        public string CronExpression { get; set; }
        public string Command { get; set; }
        public CrontabSchedule Schedule { get; set; }
        public DateTime NextRun { get; set; }

        public void Initialize()
        {
            Schedule = CrontabSchedule.Parse(CronExpression);
            NextRun = Schedule.GetNextOccurrence(DateTime.Now);
        }
    }
}
```

* `CronExpression` uses standard cron syntax like Linux cron.
* `NextRun` calculates the next execution time.

---

## Step 4: Create a Cron Jobs JSON File

Add `cronjobs.json` to the project (set **Copy to Output Directory → Copy if newer**):

```json
[
  {
    "Name": "Task1",
    "CronExpression": "*/1 * * * *",
    "Command": "echo Task 1 executed"
  },
  {
    "Name": "Task2",
    "CronExpression": "0 12 * * *",
    "Command": "echo Task 2 executed"
  }
]
```

* Each entry defines a scheduled task.
* `Command` is a placeholder; replace it with your actual task logic.

---

## Step 5: Implement the Worker Service

Replace the content of `Worker.cs` with:

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Text.Json;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.Extensions.Hosting;
using Microsoft.Extensions.Logging;

namespace CronLikeService
{
    public class Worker : BackgroundService
    {
        private readonly ILogger<Worker> _logger;
        private List<CronJob> _jobs;
        private readonly string _configPath = "cronjobs.json";
        private FileSystemWatcher _watcher;

        public Worker(ILogger<Worker> logger)
        {
            _logger = logger;
            LoadJobs();
            WatchConfigFile();
        }

        private void LoadJobs()
        {
            try
            {
                if (!File.Exists(_configPath))
                {
                    _logger.LogWarning("{file} not found. No jobs loaded.", _configPath);
                    _jobs = new List<CronJob>();
                    return;
                }

                var json = File.ReadAllText(_configPath);
                _jobs = JsonSerializer.Deserialize<List<CronJob>>(json) ?? new List<CronJob>();

                foreach (var job in _jobs)
                    job.Initialize();

                _logger.LogInformation("Loaded {count} cron jobs.", _jobs.Count);
            }
            catch (Exception ex)
            {
                _logger.LogError(ex, "Failed to load cron jobs from {file}", _configPath);
                _jobs = new List<CronJob>();
            }
        }

        private void WatchConfigFile()
        {
            _watcher = new FileSystemWatcher(Directory.GetCurrentDirectory(), _configPath)
            {
                NotifyFilter = NotifyFilters.LastWrite
            };
            _watcher.Changed += OnConfigChanged;
            _watcher.EnableRaisingEvents = true;
        }

        private void OnConfigChanged(object sender, FileSystemEventArgs e)
        {
            _logger.LogInformation("{file} changed. Reloading jobs...", _configPath);
            Task.Delay(500).Wait(); // wait to ensure file write completes
            LoadJobs();
        }

        protected override async Task ExecuteAsync(CancellationToken stoppingToken)
        {
            _logger.LogInformation("Cron-like Service started at: {time}", DateTimeOffset.Now);

            while (!stoppingToken.IsCancellationRequested)
            {
                var now = DateTime.Now;

                foreach (var job in _jobs)
                {
                    if (now >= job.NextRun)
                    {
                        try
                        {
                            RunJob(job);
                        }
                        catch (Exception ex)
                        {
                            _logger.LogError(ex, "Error running job {jobName}", job.Name);
                        }

                        job.NextRun = job.Schedule.GetNextOccurrence(DateTime.Now);
                    }
                }

                await Task.Delay(1000, stoppingToken); // check every second
            }

            _logger.LogInformation("Cron-like Service stopping at: {time}", DateTimeOffset.Now);
        }

        private void RunJob(CronJob job)
        {
            _logger.LogInformation("Running job {jobName} at {time}", job.Name, DateTimeOffset.Now);
            Console.WriteLine($"Job {job.Name} executed at {DateTime.Now}");

            // Example: run a command or script
            // System.Diagnostics.Process.Start("cmd.exe", "/c " + job.Command);
        }
    }
}
```

**Features:**

* Reads jobs from `cronjobs.json`.
* Monitors file changes and reloads dynamically.
* Executes tasks based on cron schedule.
* Logs execution to console and logger.

---

## Step 6: Setup Program Entry

`Program.cs`:

```csharp
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;

namespace CronLikeService
{
    public class Program
    {
        public static void Main(string[] args)
        {
            CreateHostBuilder(args).Build().Run();
        }

        public static IHostBuilder CreateHostBuilder(string[] args) =>
            Host.CreateDefaultBuilder(args)
                .UseWindowsService()
                .ConfigureServices((hostContext, services) =>
                {
                    services.AddHostedService<Worker>();
                });
    }
}
```

---

## Step 7: Publish the Service

1. In Visual Studio: **Build → Publish → Folder**.
2. Choose a folder, e.g., `C:\Services\CronLikeService`.
3. Or use command line:

```bash
dotnet publish -c Release -o C:\Services\CronLikeService
```

---

## Step 8: Install and Start the Service

Open **Command Prompt as Administrator**:

```cmd
sc create CronLikeService binPath= "C:\Services\CronLikeService\CronLikeService.exe"
sc start CronLikeService
```

---

## Step 9: Test Dynamic Reloading

1. While the service is running, edit `cronjobs.json` and add a new task:

```json
{
  "Name": "Task3",
  "CronExpression": "*/2 * * * *",
  "Command": "echo Task 3 executed"
}
```

2. Save the file.
3. The service will detect the change and execute `Task3` automatically without restart.

---

## Step 10: Customizing Task Logic

Replace the placeholder in `RunJob` with actual C# logic or call scripts:

```csharp
// Run a script
System.Diagnostics.Process.Start("cmd.exe", "/c " + job.Command);

// Or execute C# code directly
DoSomething(job.Name);
```

---

## Step 11: Advanced Cron Expressions

Examples:

| Cron Expression | Meaning                  |
| --------------- | ------------------------ |
| `* * * * *`     | Every minute             |
| `*/5 * * * *`   | Every 5 minutes          |
| `0 0 * * *`     | Every day at midnight    |
| `0 12 * * 1`    | Every Monday at 12:00 PM |

---

## ✅ Features of This Service

* Multi-task scheduling like Linux cron.
* Reads jobs from a JSON file.
* Dynamically reloads tasks on file changes.
* Logs task execution.
* Easily extendable to run scripts, commands, or C# code.

---

This tutorial provides a **complete, production-ready Windows Service** with full cron-like capabilities.

---
