
# 🧠 PowerShell Tutorial (Beginner → Advanced)

## 1. What is PowerShell?

**PowerShell** is a task automation and configuration management framework built on .NET. Unlike traditional shells, it works with **objects instead of plain text**.

* Shell + scripting language
* Cross-platform (Windows, macOS, Linux via PowerShell Core)
* Deep integration with Windows systems

---

## 2. Getting Started

### Open PowerShell

* Windows: Start → “Windows PowerShell” or “PowerShell”
* Or run:

```powershell
powershell
```

### Check version

```powershell
$PSVersionTable
```

---

## 3. Basic Concepts

### Cmdlets

PowerShell commands are called **cmdlets** (pronounced "command-lets").

Format:

```
Verb-Noun
```

Examples:

```powershell
Get-Process
Get-Service
Set-Location
```

---

### Get Help (VERY IMPORTANT)

```powershell
Get-Help Get-Process
Get-Help Get-Process -Examples
Get-Command *process*
```

---

## 4. The Pipeline (Core Concept)

PowerShell passes **objects**, not text:

```powershell
Get-Process | Where-Object {$_.CPU -gt 100}
```

* `|` passes output to next command
* `$_` represents current object

---

## 5. Variables

```powershell
$name = "Chris"
$number = 42
```

Use:

```powershell
Write-Output $name
```

---

## 6. Data Types

```powershell
[string]$text = "Hello"
[int]$num = 10
[datetime]$now = Get-Date
```

---

## 7. Arrays & Hashtables

### Arrays

```powershell
$numbers = 1,2,3,4
$numbers[0]
```

### Hashtables

```powershell
$person = @{
    Name = "Chris"
    Age = 30
}
$person.Name
```

---

## 8. Conditional Logic

```powershell
if ($x -gt 10) {
    "Greater than 10"
} elseif ($x -eq 10) {
    "Equal to 10"
} else {
    "Less than 10"
}
```

Operators:

* `-eq` (equals)
* `-ne` (not equal)
* `-gt`, `-lt`, `-ge`, `-le`

---

## 9. Loops

### ForEach

```powershell
$numbers | ForEach-Object {
    $_ * 2
}
```

### For loop

```powershell
for ($i=0; $i -lt 5; $i++) {
    $i
}
```

### While

```powershell
while ($x -lt 10) {
    $x++
}
```

---

## 10. Functions

```powershell
function Get-Greeting {
    param($name)
    "Hello, $name"
}

Get-Greeting -name "Chris"
```

---

## 11. Working with Files

```powershell
Get-ChildItem
Get-Content file.txt
Set-Content file.txt "Hello"
Add-Content file.txt "World"
```

---

## 12. Objects (Key PowerShell Feature)

```powershell
Get-Process | Select-Object Name, CPU
```

Inspect object:

```powershell
Get-Process | Get-Member
```

---

## 13. Filtering & Sorting

```powershell
Get-Service | Where-Object {$_.Status -eq "Running"}
Get-Process | Sort-Object CPU -Descending
```

---

## 14. Formatting Output

```powershell
Get-Process | Format-Table Name, CPU
Get-Process | Format-List *
```

---

## 15. Error Handling

```powershell
try {
    Get-Content "missing.txt"
} catch {
    "File not found"
} finally {
    "Done"
}
```

---

## 16. Modules

List modules:

```powershell
Get-Module -ListAvailable
```

Install module:

```powershell
Install-Module -Name Az
```

Import:

```powershell
Import-Module Az
```

---

## 17. Remoting

Enable:

```powershell
Enable-PSRemoting -Force
```

Run command remotely:

```powershell
Invoke-Command -ComputerName Server1 -ScriptBlock { Get-Process }
```

---

## 18. Working with JSON & APIs

```powershell
$response = Invoke-RestMethod -Uri "https://api.github.com"
$response
```

Convert:

```powershell
$json = $response | ConvertTo-Json
```

---

## 19. Background Jobs

```powershell
Start-Job { Get-Process }
Get-Job
Receive-Job -Id 1
```

---

## 20. Scripting (.ps1 files)

Create a script:

```powershell
# script.ps1
Write-Output "Hello World"
```

Run:

```powershell
.\script.ps1
```

---

## 21. Execution Policy

```powershell
Get-ExecutionPolicy
Set-ExecutionPolicy RemoteSigned
```

---

## 22. Advanced: Custom Objects

```powershell
$obj = [PSCustomObject]@{
    Name = "Chris"
    Role = "Admin"
}
```

---

## 23. Advanced: Classes (PowerShell 5+)

```powershell
class Person {
    [string]$Name
    [int]$Age

    Person($name, $age) {
        $this.Name = $name
        $this.Age = $age
    }
}
```

---

## 24. Debugging

```powershell
Set-PSBreakpoint -Script script.ps1 -Line 3
```

Step through:

```powershell
Step-Into
Step-Over
```

---

## 25. Real-World Examples

### Find large files

```powershell
Get-ChildItem -Recurse | Where-Object {$_.Length -gt 1MB}
```

### Kill a process

```powershell
Stop-Process -Name notepad
```

### Monitor CPU

```powershell
Get-Process | Sort-Object CPU -Descending | Select-Object -First 5
```

---

## 26. Best Practices

* Use **Verb-Noun naming**
* Always include `-WhatIf` for destructive commands
* Use `Get-Help`
* Prefer **objects over text parsing**
* Write reusable functions

---

## 27. Learning Path (Recommended)

1. Basics (cmdlets, pipeline)
2. Objects & filtering
3. Scripting fundamentals
4. System administration tasks
5. Advanced scripting (functions, modules)
6. Automation & DevOps

---

## 28. Useful Commands Cheat Sheet

```powershell
Get-Command
Get-Help
Get-Process
Get-Service
Get-ChildItem
Where-Object
Select-Object
Sort-Object
```

---

## 29. Practice Exercises

1. List all running processes sorted by memory
2. Find all `.log` files over 10MB
3. Create a function that takes a name and prints a greeting
4. Call a public API and display results
5. Write a script to monitor disk space


