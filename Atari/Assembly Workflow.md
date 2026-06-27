# Complete Tutorial: Atari 800 Development with VS Code

This tutorial will teach you how to write, build, debug, and run Atari 800 software using Visual Studio Code on Windows, Linux, or macOS.

By the end, you'll be able to:

* Write Atari 800 assembly language programs
* Build programs from VS Code
* Run them automatically in an emulator
* Understand Atari memory layout
* Draw graphics
* Handle keyboard input
* Create simple games

---

# 1. Atari 800 Architecture Overview

The Atari 400/800 computers use:

| Component | Description               |
| --------- | ------------------------- |
| CPU       | MOS Technology 6502       |
| Speed     | ~1.79 MHz                 |
| RAM       | 8K–48K                    |
| Graphics  | ANTIC + GTIA              |
| Sound     | POKEY                     |
| OS ROM    | Built-in operating system |
| BASIC     | Cartridge or built-in     |

Important chips:

| Chip  | Purpose                 |
| ----- | ----------------------- |
| 6502  | CPU                     |
| ANTIC | Display processor       |
| GTIA  | Graphics                |
| POKEY | Sound, keyboard, serial |

---

# 2. Install Required Tools

## Install VS Code

Download:

[Visual Studio Code](https://code.visualstudio.com?utm_source=chatgpt.com)

---

## Install Atari Emulator

Recommended:

### Atari800 Emulator

[Atari800 Emulator](https://atari800.github.io?utm_source=chatgpt.com)

Linux:

```bash
sudo apt install atari800
```

---

## Install CC65

CC65 provides:

* CA65 assembler
* LD65 linker
* Atari target support

Download:

[CC65 Project](https://cc65.github.io/cc65/?utm_source=chatgpt.com)

Linux:

```bash
sudo apt install cc65
```

Verify:

```bash
ca65 --version
ld65 --version
```

---

# 3. Create Project Structure

Create:

```text
AtariHello/
│
├── .vscode/
│   ├── tasks.json
│   └── launch.json
│
├── src/
│   └── hello.s
│
└── build/
```

---

# 4. Install VS Code Extensions

Recommended:

* 6502 Assembly
* Hex Editor
* Makefile Tools

Search Extensions:

```text
6502
```

Install a syntax highlighter.

---

# 5. First Atari Program

Create:

```asm
; hello.s

        .export _start

        *= $2000

_start:

        lda #0
        sta 752

loop:
        jmp loop
```

This program simply disables the cursor.

---

# 6. Understanding Atari Memory

### Cursor

```text
752 = Cursor Inhibit
```

```asm
lda #1
sta 752
```

Hide cursor.

---

### Screen Memory

Text mode starts at:

```text
$9C40
```

or

```text
40000 decimal
```

depending on mode.

---

### Useful Locations

| Address | Purpose          |
| ------- | ---------------- |
| 20      | RTCLOK           |
| 53279   | CONSOL           |
| 764     | Last key pressed |
| 752     | Cursor           |
| 710     | Background color |

---

# 7. Writing Text to Screen

Example:

```asm
        *= $2000

screen = $9C40

start:

        lda #'H'
        sta screen

        lda #'I'
        sta screen+1

loop:
        jmp loop
```

Build and run to display:

```text
HI
```

---

# 8. Build Script

Create:

```json
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "Build Atari",
      "type": "shell",
      "command": "ca65 src/hello.s -o build/hello.o && ld65 build/hello.o -C atari.cfg -o build/hello.xex"
    }
  ]
}
```

Save as:

```text
.vscode/tasks.json
```

---

# 9. Atari Linker Configuration

Create:

```text
atari.cfg
```

```cfg
MEMORY {
    RAM: start = $2000, size = $8000, type = rw;
}

SEGMENTS {
    CODE: load = RAM, type = ro;
}
```

---

# 10. Building

Press:

```text
Ctrl+Shift+B
```

or:

```text
Terminal → Run Build Task
```

Output:

```text
build/hello.xex
```

---

# 11. Running Automatically

Modify task:

```json
{
  "label": "Run Atari",
  "type": "shell",
  "command": "atari800 build/hello.xex"
}
```

Now:

```text
Ctrl+Shift+B
```

Builds and launches.

---

# 12. Understanding Display Codes

Atari uses ATASCII.

Examples:

| Character | Value |
| --------- | ----- |
| A         | 65    |
| B         | 66    |
| C         | 67    |

Example:

```asm
lda #65
sta $9C40
```

Displays:

```text
A
```

---

# 13. Reading Keyboard Input

Keyboard location:

```text
764
```

Example:

```asm
        *= $2000

start:

wait:

        lda 764
        cmp #255
        beq wait

        sta $9C40

        lda #255
        sta 764

        jmp wait
```

Pressing keys shows character codes.

---

# 14. Colors

Background color:

```asm
lda #100
sta 710
```

Try:

```asm
lda #40
sta 710
```

Different values create different colors.

---

# 15. Simple Animation

```asm
screen = $9C40

        *= $2000

start:

        ldx #0

loop:

        txa
        sta screen

        inx

        jmp loop
```

Produces rapidly changing characters.

---

# 16. Delay Routine

```asm
delay:

        ldx #255

d1:
        ldy #255

d2:
        dey
        bne d2

        dex
        bne d1

        rts
```

Use:

```asm
jsr delay
```

---

# 17. Moving a Character

```asm
screen = $9C40

        *= $2000

start:

        ldx #0

loop:

        lda #' '
        sta screen,x

        inx

        lda #'*'
        sta screen,x

        jsr delay

        jmp loop
```

Creates movement across the screen.

---

# 18. Sound Using POKEY

Frequency register:

```text
53760
```

Volume register:

```text
53761
```

Example:

```asm
lda #100
sta 53760

lda #160
sta 53761
```

Produces a tone.

---

# 19. Player/Missile Graphics

Advanced Atari graphics.

Memory areas:

```text
PMBASE
GRAFP0
HPOSP0
```

Used for hardware sprites.

Example setup:

```asm
lda #64
sta 54279
```

This enables player/missile DMA.

---

# 20. Creating a Game Loop

Typical structure:

```asm
initialize

mainloop:

    read_input

    update_player

    update_enemies

    draw_screen

    jmp mainloop
```

Most Atari games follow this pattern.

---

# 21. Sample Program: Moving Cursor

```asm
screen = $9C40

        *= $2000

start:

        ldx #0

main:

        lda #' '
        sta screen,x

        lda 764

        cmp #255
        beq main

        cmp #14
        beq left

        cmp #7
        beq right

        jmp clear

left:
        dex
        jmp clear

right:
        inx

clear:

        lda #255
        sta 764

        lda #'@'
        sta screen,x

        jmp main
```

This creates a movable character controlled by the keyboard.

---

# 22. Recommended Development Workflow

```text
VS Code
    ↓
CA65 Assembler
    ↓
LD65 Linker
    ↓
XEX File
    ↓
Atari800 Emulator
```

Automate everything with a build task so that:

```text
Ctrl+Shift+B
```

becomes:

```text
Assemble
Link
Launch Emulator
```

---

# 23. Useful Atari References

### Atari Hardware Manual

[AtariArchives Hardware Documentation](https://www.atariarchives.org?utm_source=chatgpt.com)

### Mapping the Atari

[Mapping the Atari Online](https://www.atariarchives.org/mapping/?utm_source=chatgpt.com)

### CC65 Documentation

[CC65 Documentation](https://cc65.github.io/doc/?utm_source=chatgpt.com)

### Atari800 Emulator

[Atari800 Emulator Documentation](https://atari800.github.io/documentation.html?utm_source=chatgpt.com)

---

# Next Steps

Once you're comfortable with basic assembly, learn these Atari-specific topics in order:

1. Memory-mapped hardware
2. Display lists
3. ANTIC modes
4. Display list interrupts (DLIs)
5. Vertical blank interrupts (VBIs)
6. Player/Missile graphics
7. Scrolling playfields
8. POKEY sound effects
9. Joystick handling
10. Full game architecture

Mastering ANTIC, GTIA, and display lists is what separates simple Atari programs from professional-quality Atari 800 games.
