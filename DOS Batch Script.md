
# 🧾 DOS Batch File Tutorial

## 1. What is a Batch File?

A **batch file** is a plain text file with a `.bat` (or `.cmd`) extension that contains a series of commands executed by the Windows command interpreter (`cmd.exe`).

Think of it as a **script for automating tasks**.

---

## 2. Creating Your First Batch File

### Step 1: Create the file

1. Open Notepad
2. Save as: `hello.bat`
3. Change “Save as type” → *All Files*

### Step 2: Add code

```bat
@echo off
echo Hello, World!
pause
```

### Step 3: Run it

Double-click the file or run in Command Prompt:

```
hello.bat
```

---

## 3. Core Commands

### `@echo off`

* Hides command output (cleaner screen)

```bat
@echo off
```

### `echo`

* Displays text

```bat
echo This is a message
```

### `pause`

* Waits for user input

```bat
pause
```

### `cls`

* Clears screen

```bat
cls
```

### `title`

* Changes window title

```bat
title My Script
```

---

## 4. Variables

### Setting variables

```bat
set name=Chris
echo Hello %name%
```

### User input

```bat
set /p name=Enter your name:
echo Hello %name%
```

---

## 5. Conditional Logic (IF)

```bat
@echo off
set /p num=Enter a number:

if %num%==10 (
    echo You entered 10
) else (
    echo Not 10
)
pause
```

### Comparing numbers

```bat
if %num% GTR 10 echo Greater than 10
if %num% LSS 10 echo Less than 10
if %num% EQU 10 echo Equal to 10
```

---

## 6. GOTO and Labels

```bat
@echo off
echo 1. Say Hello
echo 2. Exit
set /p choice=Choose:

if %choice%==1 goto hello
if %choice%==2 goto end

:hello
echo Hello!
goto end

:end
echo Goodbye!
pause
```

---

## 7. Loops

### FOR loop

```bat
@echo off
for %%i in (1 2 3 4 5) do (
    echo Number: %%i
)
pause
```

### Loop through files

```bat
for %%f in (*.txt) do (
    echo Found file: %%f
)
```

---

## 8. Working with Files & Folders

### Create directory

```bat
mkdir testfolder
```

### Delete file

```bat
del file.txt
```

### Copy file

```bat
copy file.txt backup.txt
```

### Move file

```bat
move file.txt folder\
```

---

## 9. Command-Line Arguments

Run script like:

```
myscript.bat John 25
```

Script:

```bat
@echo off
echo Name: %1
echo Age: %2
pause
```

---

## 10. Functions (Using CALL)

```bat
@echo off
call :greet Chris
pause
exit /b

:greet
echo Hello %1
exit /b
```

---

## 11. Error Handling

### Check if file exists

```bat
if exist file.txt (
    echo File exists
) else (
    echo File not found
)
```

### Check error level

```bat
somecommand
if %errorlevel% neq 0 (
    echo Command failed
)
```

---

## 12. Advanced: Delayed Expansion

Used when variables change inside loops.

```bat
@echo off
setlocal enabledelayedexpansion

set count=0

for %%i in (a b c) do (
    set /a count+=1
    echo Count: !count!
)

pause
```

---

## 13. Useful System Commands

* `ipconfig` → network info
* `tasklist` → running processes
* `taskkill` → kill process
* `shutdown` → shutdown/restart
* `dir` → list files

Example:

```bat
tasklist
pause
```

---

## 14. Practical Example: Backup Script

```bat
@echo off
set source=C:\MyFiles
set destination=D:\Backup

echo Backing up files...
xcopy %source% %destination% /E /I /Y

echo Backup complete!
pause
```

---

## 15. Debugging Tips

* Remove `@echo off` to see commands
* Add `echo` statements to trace values
* Use `pause` to stop execution

---

## 16. Best Practices

* Always start with:

```bat
@echo off
```

* Use meaningful variable names
* Comment your code:

```bat
rem This is a comment
```

---

## 17. Limitations of Batch Files

Batch is powerful but limited:

* Hard to maintain for large scripts
* Limited data structures
* Slow compared to modern scripting

👉 For advanced automation, consider:

* PowerShell
* Python

