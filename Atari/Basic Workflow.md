A modern workflow for writing Atari BASIC in VS Code and running it in the Altirra emulator is:

1. Write your BASIC source as a text file (`.bas`)
2. Convert it to an Atari tokenized BASIC file
3. Mount the file in Altirra
4. Run it

## Prerequisites

Install:

* [Visual Studio Code](https://code.visualstudio.com?utm_source=chatgpt.com)
* [Altirra Atari Emulator](https://www.virtualdub.org/altirra.html?utm_source=chatgpt.com)
* [Atari800MacX BASIC Tokenizer Tools (atbasic)](https://github.com/atarimacosx/Atari800MacX?utm_source=chatgpt.com) (or another Atari BASIC tokenizer)

---

# Step 1: Create a Project

Create a folder:

```text
atari-project/
├── hello.bas
├── build.bat
└── .vscode/
    └── tasks.json
```

Open the folder in VS Code.

---

# Step 2: Write Atari BASIC

Create `hello.bas`:

```basic
10 GRAPHICS 0
20 ? "HELLO ATARI 800!"
30 ? "WRITTEN IN VS CODE"
40 FOR I=1 TO 10
50 ? I
60 NEXT I
70 END
```

You can use any plain text editor in VS Code.

---

# Step 3: Tokenize the BASIC Program

Atari BASIC files stored on disk are tokenized.

Assume you have a tokenizer called:

```text
atbasic.exe
```

Create `build.bat`:

```bat
@echo off

atbasic.exe hello.bas HELLO.BAS

echo Build complete.
pause
```

Running:

```cmd
build.bat
```

creates:

```text
HELLO.BAS
```

which can be loaded by Atari BASIC.

---

# Step 4: Configure a VS Code Build Task

Create `.vscode/tasks.json`:

```json
{
    "version": "2.0.0",
    "tasks": [
        {
            "label": "Build Atari BASIC",
            "type": "shell",
            "command": "build.bat",
            "group": {
                "kind": "build",
                "isDefault": true
            }
        }
    ]
}
```

Now press:

```text
Ctrl+Shift+B
```

to build.

---

# Step 5: Create an Atari DOS Disk

You need a disk image.

A common choice is:

* Atari DOS 2.5
* MyDOS

Create a disk image containing:

```text
HELLO.BAS
```

Tools that can create Atari disk images include:

* [Dir2Atr](https://www.horus.com/~hias/atari/?utm_source=chatgpt.com)
* [RespeQt](https://respeqt.github.io?utm_source=chatgpt.com)

Example:

```text
MYDISK.ATR
    HELLO.BAS
```

---

# Step 6: Start Altirra

Open Altirra.

Configure:

```text
System -> Atari 800XL
```

or

```text
System -> Atari 800
```

Enable BASIC:

```text
System -> Firmware
```

Make sure Atari BASIC is installed and enabled.

---

# Step 7: Mount the Disk

In Altirra:

```text
File -> Attach Disk Image
```

Select:

```text
MYDISK.ATR
```

Boot the machine.

---

# Step 8: Load the Program

At the Atari BASIC prompt:

```basic
LOAD "D:HELLO.BAS"
```

Then:

```basic
RUN
```

Output:

```text
HELLO ATARI 800!
WRITTEN IN VS CODE
1
2
3
4
5
6
7
8
9
10
```

---

# Faster Development Loop

Altirra supports host device mapping.

Map a Windows folder as:

```text
H1:
```

Example:

```text
C:\AtariProjects
```

Then from BASIC:

```basic
LOAD "H1:HELLO.BAS"
```

This avoids rebuilding ATR images every time.

---

# Optional: One-Key Build and Run

You can create a VS Code task that:

1. Tokenizes BASIC
2. Copies it to a host folder
3. Launches Altirra

Example:

```json
{
    "label": "Build and Run",
    "type": "shell",
    "command": "build.bat && start altirra.exe"
}
```

Press:

```text
Ctrl+Shift+B
```

and Altirra starts with the newest build.

---

# Recommended Modern Setup

```text
VS Code
   ↓
Atari BASIC source (.bas)
   ↓
Tokenizer
   ↓
Host Folder (H1:)
   ↓
Altirra
```

This is the closest Atari equivalent to a modern edit-build-run workflow and avoids constantly rebuilding ATR disk images.
