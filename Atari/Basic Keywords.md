Below is a reference of the standard **Atari BASIC** keywords (the version supplied on the Atari BASIC cartridge for the Atari 400/800/XL/XE computers). These are the reserved words you use to build programs.

---

# Program Control

| Keyword | Description                                                     |
| ------- | --------------------------------------------------------------- |
| `END`   | Stops program execution immediately.                            |
| `STOP`  | Stops execution but allows `CONT` from direct mode to continue. |
| `RUN`   | Starts the program from the beginning.                          |
| `NEW`   | Erases the current program from memory.                         |
| `CONT`  | Continues execution after a `STOP` or a break.                  |
| `LIST`  | Displays program lines on the screen.                           |
| `ENTER` | Loads a text listing into memory.                               |
| `LOAD`  | Loads a tokenized BASIC program.                                |
| `SAVE`  | Saves a tokenized BASIC program.                                |
| `CSAVE` | Saves a program to cassette.                                    |
| `CLOAD` | Loads a program from cassette.                                  |
| `TRAP`  | Specifies an error handler line number.                         |

Example:

```basic
10 TRAP 100
20 PRINT 10/0
30 END
100 PRINT "AN ERROR OCCURRED"
```

---

# Variables and Assignment

| Keyword | Description                               |
| ------- | ----------------------------------------- |
| `LET`   | Assigns a value to a variable (optional). |
| `DIM`   | Declares strings or arrays.               |
| `CLR`   | Clears variables and arrays from memory.  |

Example

```basic
10 LET SCORE=100
20 DIM NAME$(20)
```

---

# Input and Output

| Keyword | Description                         |
| ------- | ----------------------------------- |
| `PRINT` | Displays output.                    |
| `INPUT` | Reads keyboard input.               |
| `GET`   | Reads one character from a device.  |
| `OPEN`  | Opens an I/O channel.               |
| `CLOSE` | Closes an open channel.             |
| `PUT`   | Writes one byte to a device.        |
| `NOTE`  | Gets current file position.         |
| `POINT` | Sets file position.                 |
| `XIO`   | Executes CIO extended I/O commands. |

Example

```basic
10 INPUT "NAME";N$
20 PRINT "HELLO ";N$
```

---

# Branching

| Keyword  | Description                      |
| -------- | -------------------------------- |
| `IF`     | Conditional execution.           |
| `THEN`   | Separates condition from action. |
| `GOTO`   | Unconditional jump.              |
| `GOSUB`  | Calls a subroutine.              |
| `RETURN` | Returns from a subroutine.       |
| `ON`     | Multi-way branch.                |

Example

```basic
10 IF X>10 THEN GOTO 100
```

Example

```basic
10 ON X GOTO 100,200,300
```

---

# Loops

| Keyword | Description             |
| ------- | ----------------------- |
| `FOR`   | Begins a counted loop.  |
| `TO`    | Specifies ending value. |
| `STEP`  | Specifies increment.    |
| `NEXT`  | Ends a `FOR` loop.      |

Example

```basic
10 FOR I=1 TO 10
20 PRINT I
30 NEXT I
```

---

# Data Statements

| Keyword   | Description             |
| --------- | ----------------------- |
| `DATA`    | Stores constant values. |
| `READ`    | Reads values from DATA. |
| `RESTORE` | Resets DATA pointer.    |

Example

```basic
10 DATA 5,10,15
20 READ A,B,C
```

---

# String Functions

| Keyword  | Description                             |
| -------- | --------------------------------------- |
| `CHR$`   | Converts ASCII code to character.       |
| `ASC`    | Returns ASCII value of first character. |
| `STR$`   | Converts number to string.              |
| `VAL`    | Converts string to number.              |
| `LEN`    | Returns string length.                  |
| `LEFT$`  | Left part of a string.                  |
| `RIGHT$` | Right part of a string.                 |
| `MID$`   | Middle part of a string.                |

Example

```basic
10 A$="ATARI"
20 PRINT LEFT$(A$,2)
```

Output

```
AT
```

---

# Math Functions

| Keyword | Description        |
| ------- | ------------------ |
| `ABS`   | Absolute value.    |
| `ATN`   | Arctangent.        |
| `COS`   | Cosine.            |
| `SIN`   | Sine.              |
| `TAN`   | Tangent.           |
| `EXP`   | Exponential.       |
| `LOG`   | Natural logarithm. |
| `SQR`   | Square root.       |
| `INT`   | Integer portion.   |
| `RND`   | Random number.     |

Example

```basic
10 PRINT INT(RND(0)*100)
```

---

# Memory Functions

| Keyword | Description                                  |
| ------- | -------------------------------------------- |
| `PEEK`  | Reads a byte from memory.                    |
| `POKE`  | Writes a byte to memory.                     |
| `USR`   | Calls a machine language routine.            |
| `ADR`   | Returns memory address of a string or array. |
| `FRE`   | Returns available memory.                    |

Example

```basic
10 PRINT PEEK(20)
```

---

# Graphics

| Keyword    | Description                            |
| ---------- | -------------------------------------- |
| `GRAPHICS` | Sets graphics mode.                    |
| `COLOR`    | Selects drawing color.                 |
| `PLOT`     | Draws a pixel.                         |
| `DRAWTO`   | Draws a line.                          |
| `POSITION` | Moves text cursor.                     |
| `LOCATE`   | Reads the color at a screen position.  |
| `SETCOLOR` | Changes hardware color registers.      |
| `FILLTO`   | Fills an enclosed area (Revision B/C). |

Example

```basic
10 GRAPHICS 8
20 COLOR 1
30 PLOT 20,20
40 DRAWTO 100,100
```

---

# Sound

| Keyword | Description                                          |
| ------- | ---------------------------------------------------- |
| `SOUND` | Plays a tone through one of the four sound channels. |

Example

```basic
10 SOUND 0,100,10,8
```

Parameters:

```
channel
pitch
distortion
volume
```

---

# Editing

| Keyword | Description                               |
| ------- | ----------------------------------------- |
| `REM`   | Comment. Everything after REM is ignored. |

Example

```basic
10 REM GAME INITIALIZATION
```

---

# Operators

## Arithmetic

| Operator | Meaning        |
| -------- | -------------- |
| `+`      | Addition       |
| `-`      | Subtraction    |
| `*`      | Multiplication |
| `/`      | Division       |
| `^`      | Exponentiation |

---

## Comparison

| Operator | Meaning               |
| -------- | --------------------- |
| `=`      | Equal                 |
| `<>`     | Not equal             |
| `<`      | Less than             |
| `>`      | Greater than          |
| `<=`     | Less than or equal    |
| `>=`     | Greater than or equal |

---

## Logical

| Operator | Meaning     |
| -------- | ----------- |
| `AND`    | Logical AND |
| `OR`     | Logical OR  |
| `NOT`    | Logical NOT |

---

# Special Constants

| Keyword | Description                                      |
| ------- | ------------------------------------------------ |
| `PI`    | Mathematical constant π (approximately 3.14159). |

---

# Frequently Used Keywords

If you're just getting started, these are the commands you'll use in most Atari BASIC programs:

| Keyword          | Purpose                          |
| ---------------- | -------------------------------- |
| `PRINT`          | Display text or numbers          |
| `INPUT`          | Get user input                   |
| `IF...THEN`      | Make decisions                   |
| `FOR...NEXT`     | Repeat code                      |
| `GOTO`           | Jump to another line             |
| `GOSUB...RETURN` | Call reusable routines           |
| `DIM`            | Create arrays and strings        |
| `GRAPHICS`       | Change screen mode               |
| `COLOR`          | Select drawing color             |
| `PLOT`           | Draw pixels                      |
| `DRAWTO`         | Draw lines                       |
| `SOUND`          | Play tones                       |
| `PEEK`           | Read memory                      |
| `POKE`           | Write memory                     |
| `DATA` / `READ`  | Store and retrieve constant data |
| `RND`            | Generate random numbers          |
| `REM`            | Add comments                     |

This list covers the core keywords available in standard Atari BASIC. Some keywords, such as `FILLTO`, are only available in later revisions (Revision B/C) and may not exist on the original Atari BASIC cartridge.
