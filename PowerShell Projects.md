
# 🚀 PowerShell Project-Based Course

## 🧭 Structure

Each project includes:

* 🎯 Goal
* 🧠 Concepts learned
* 🛠️ Build steps
* ✅ Challenge extensions

---

# 🟢 Project 1: System Info Dashboard

## 🎯 Goal

Create a script that displays:

* CPU usage
* Memory usage
* Top processes

---

## 🧠 Concepts

* Cmdlets (`Get-Process`, `Get-CimInstance`)
* Objects & properties
* Formatting

---

## 🛠️ Build

```powershell
# CPU + Memory info
$os = Get-CimInstance Win32_OperatingSystem

$totalMem = $os.TotalVisibleMemorySize
$freeMem = $os.FreePhysicalMemory
$usedMem = $totalMem - $freeMem

Write-Output "Memory Used: $([math]::Round($usedMem/1MB,2)) GB"

# Top 5 processes by CPU
Get-Process |
    Sort-Object CPU -Descending |
    Select-Object -First 5 Name, CPU |
    Format-Table
```

---

## ✅ Challenge

* Add disk usage
* Refresh every 5 seconds (hint: `Start-Sleep`)

---

# 🟡 Project 2: File Organizer

## 🎯 Goal

Automatically organize files into folders by extension.

---

## 🧠 Concepts

* File system (`Get-ChildItem`)
* Loops
* Conditional logic

---

## 🛠️ Build

```powershell
$path = "C:\Temp"

Get-ChildItem $path -File | ForEach-Object {
    $ext = $_.Extension.TrimStart('.')

    if (-not (Test-Path "$path\$ext")) {
        New-Item -ItemType Directory -Path "$path\$ext"
    }

    Move-Item $_.FullName -Destination "$path\$ext"
}
```

---

## ✅ Challenge

* Add a `-WhatIf` safety mode
* Skip files older than 30 days

---

# 🔵 Project 3: Log Analyzer

## 🎯 Goal

Parse logs and extract errors.

---

## 🧠 Concepts

* Text processing
* Filtering
* Pattern matching (`-match`)

---

## 🛠️ Build

```powershell
$log = Get-Content "app.log"

$errors = $log | Where-Object { $_ -match "ERROR" }

$errors | Set-Content "errors.txt"

Write-Output "Found $($errors.Count) errors"
```

---

## ✅ Challenge

* Extract timestamps
* Count errors by type

---

# 🟣 Project 4: REST API Client

## 🎯 Goal

Call an API and display structured data.

---

## 🧠 Concepts

* `Invoke-RestMethod`
* JSON handling

---

## 🛠️ Build

```powershell
$response = Invoke-RestMethod "https://api.github.com/repos/microsoft/powershell"

$response | Select-Object name, stargazers_count, forks_count
```

---

## ✅ Challenge

* Query multiple repos
* Output to CSV

---

# 🔴 Project 5: Process Monitor & Killer

## 🎯 Goal

Monitor and stop high-CPU processes.

---

## 🧠 Concepts

* Automation
* Conditional logic
* System control

---

## 🛠️ Build

```powershell
$threshold = 100

Get-Process | Where-Object {
    $_.CPU -gt $threshold
} | ForEach-Object {
    Write-Output "Killing $($_.Name)"
    Stop-Process -Id $_.Id -Force
}
```

---

## ⚠️ Safety Tip

Add:

```powershell
-WhatIf
```

before actually killing processes.

---

## ✅ Challenge

* Log killed processes
* Add whitelist (don’t kill certain apps)

---

# 🟠 Project 6: Backup Automation Script

## 🎯 Goal

Create a backup system.

---

## 🧠 Concepts

* File copying
* Scheduling mindset

---

## 🛠️ Build

```powershell
$source = "C:\Important"
$dest = "D:\Backup\$(Get-Date -Format yyyy-MM-dd)"

New-Item -ItemType Directory -Path $dest -Force

Copy-Item $source\* -Destination $dest -Recurse
```

---

## ✅ Challenge

* Compress backups (`Compress-Archive`)
* Delete backups older than 7 days

---

# ⚫ Project 7: Custom Module

## 🎯 Goal

Package your functions into a reusable module.

---

## 🧠 Concepts

* Functions
* Modules

---

## 🛠️ Build

Create file:

```powershell
MyTools.psm1
```

Add:

```powershell
function Get-SystemSummary {
    Get-Process | Sort CPU -Descending | Select -First 3
}
```

Import:

```powershell
Import-Module ./MyTools.psm1
Get-SystemSummary
```

---

## ✅ Challenge

* Add multiple functions
* Export module properly

---

# 🧪 Final Project: DevOps Automation Script

## 🎯 Goal

Combine everything:

* Pull API data
* Process logs
* Generate report
* Save results

---

## 🛠️ Example

```powershell
$data = Invoke-RestMethod "https://api.github.com/repos/microsoft/powershell"

$report = [PSCustomObject]@{
    Name = $data.name
    Stars = $data.stargazers_count
    Forks = $data.forks_count
    Date = Get-Date
}

$report | Export-Csv "report.csv" -NoTypeInformation
```

---

# 🧠 How to Practice Effectively

Instead of just reading:

1. Break scripts intentionally
2. Use `Get-Member` on everything
3. Modify each project:

   * Add parameters
   * Turn into functions
   * Add logging

