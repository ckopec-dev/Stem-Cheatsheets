Below is a **complete, practical tutorial** for writing **Commodore 64 assembly** using **Visual Studio Code**, aimed at modern developers who want a real workflow (editor → build → emulator).

This tutorial assumes:

* You know basic programming concepts
* You are **new to 6502 assembly**
* You want a **modern dev environment**, not a vintage setup

---

# Commodore 64 Assembly Programming with Visual Studio Code

## 1. What You’ll Need

### Required Software

| Tool                   | Purpose                         |
| ---------------------- | ------------------------------- |
| **Visual Studio Code** | Code editor                     |
| **cc65**               | 6502 assembler + tools          |
| **VICE Emulator**      | Run C64 programs                |
| **VS Code Extensions** | Syntax highlighting & debugging |

---

## 2. Installing the Toolchain

### 2.1 Install cc65

#### Windows

1. Download from:
   [https://cc65.github.io/](https://cc65.github.io/)
2. Extract and add the `bin` folder to your PATH
3. Verify:

```bash
ca65 --version
```

#### Linux (Ubuntu/Debian)

```bash
sudo apt install cc65
```

#### macOS

```bash
brew install cc65
```

---

### 2.2 Install VICE Emulator

Download from:
[https://vice-emu.sourceforge.io/](https://vice-emu.sourceforge.io/)

Install **x64sc** (cycle-accurate emulator).

Verify it runs:

```bash
x64sc
```

---

## 3. Configure Visual Studio Code

### 3.1 Recommended Extensions

Install these from the VS Code Marketplace:

* **6502 Assembly** (syntax highlighting)
* **Kick Assembler Syntax** (optional)
* **Hex Editor** (optional)

---

### 3.2 Workspace Structure

Create a project folder:

```
c64-asm/
│
├── src/
│   └── main.asm
├── build/
│   └── main.prg
├── Makefile
└── .vscode/
    └── tasks.json
```

---

## 4. Your First C64 Assembly Program

### 4.1 The Minimal C64 Program

Create `src/main.asm`:

```asm
; BASIC loader
* = $0801
.word next
.word 10
.byte $9e
.text "2061"
.byte 0
next:
.word 0

; Machine code starts at $0810
* = $0810

start:
    lda #$93        ; Clear screen
    jsr $ffd2       ; CHROUT
    rts
```

### What This Does

* Creates a BASIC stub: `10 SYS 2061`
* Clears the screen
* Returns to BASIC

---

## 5. Assemble the Program

### 5.1 Using ca65 + ld65

Create `Makefile`:

```makefile
ASM=ca65
LD=ld65

all:
	$(ASM) src/main.asm -o build/main.o
	$(LD) build/main.o -o build/main.prg -C c64.cfg
```

Copy the default `c64.cfg` from cc65:

```bash
cp $(cc65-config-path)/c64.cfg .
```

Build:

```bash
make
```

---

## 6. Run in the Emulator

```bash
x64sc build/main.prg
```

In the emulator:

```basic
RUN
```

Screen clears — success 🎉

---

## 7. VS Code Build Task (One-Key Build)

Create `.vscode/tasks.json`:

```json
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "Build C64",
      "type": "shell",
      "command": "make",
      "group": "build",
      "problemMatcher": []
    }
  ]
}
```

Now press:

```
Ctrl + Shift + B
```

---

## 8. Understanding C64 Memory Map (Essentials)

| Address       | Purpose    |
| ------------- | ---------- |
| `$0000–$00FF` | Zero Page  |
| `$0400`       | Screen RAM |
| `$D800`       | Color RAM  |
| `$FFD2`       | CHROUT     |
| `$FFBA`       | SETLFS     |

---

## 9. Writing Text to the Screen

### 9.1 Direct Screen Memory

```asm
* = $0810

    ldx #0
loop:
    lda message,x
    beq done
    sta $0400,x
    inx
    bne loop

done:
    rts

message:
    .text "HELLO C64!"
    .byte 0
```

This writes text directly to screen RAM.

---

## 10. Colors

Color RAM starts at `$D800`:

```asm
    lda #1      ; White
    sta $D800
```

Color values:

| Value | Color |
| ----- | ----- |
| 0     | Black |
| 1     | White |
| 2     | Red   |
| 5     | Green |
| 6     | Blue  |

---

## 11. Keyboard Input (Basic)

Read last key pressed:

```asm
    jsr $ffe4    ; GETIN
    beq no_key
```

---

## 12. Raster Timing (Intro-Level)

Wait for a raster line:

```asm
wait:
    lda $d012
    cmp #100
    bne wait
```

Used for:

* Stable animation
* Music timing
* Effects

---

## 13. Debugging Tips

### Emulator Features

* **Alt + H**: Monitor
* **Alt + M**: Memory view
* **Alt + T**: Trace CPU

### Useful Monitor Commands

```
r            ; registers
m 0400 0428 ; screen memory
```

---

## 14. Recommended Next Projects

1. **Scrolling text**
2. **Sprite movement**
3. **Raster bars**
4. **Joystick input**
5. **Music playback (SID)**

---

## 15. Optional: Kick Assembler Instead

If you prefer:

* Modern macros
* Faster iteration

Use **KickAssembler**:

* Java-based
* Popular in demoscene
* Excellent VS Code integration

---

## 16. Learning Resources

* 📘 *Mapping the C64* – Sheldon Leemon
* 📘 *Compute!’s Mapping the Commodore 64*
* 🌐 [https://www.c64-wiki.com](https://www.c64-wiki.com)
* 🌐 [https://codebase64.org](https://codebase64.org)
* 🎥 YouTube: “C64 Assembly Programming”

---

## 17. Summary

You now have:

* A **modern C64 dev workflow**
* **One-key builds** in VS Code
* A **real emulator-driven workflow**
* A foundation for **games, demos, and tools**
