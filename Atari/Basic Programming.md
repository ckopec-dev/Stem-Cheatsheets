# Atari BASIC Tutorial

## A Complete Beginner's Guide to Programming the Atari 400/800/XL/XE Computers

---

# Table of Contents

1. Introduction
2. What You'll Need
3. Starting Atari BASIC
4. The BASIC Screen
5. Your First Program
6. Variables
7. Numbers and Strings
8. Input and Output
9. Math Operations
10. Decision Making
11. Loops
12. Arrays
13. String Functions
14. Graphics
15. Sound
16. Joysticks
17. Player/Missile Graphics
18. Files
19. Advanced Techniques
20. Sample Games
21. Useful Tips

---

# Chapter 1 - Introduction

Atari BASIC was released by Atari in 1979 for the Atari 400 and Atari 800 computers. It later became built into the XL and XE series.

Unlike many BASIC dialects, Atari BASIC includes:

* High-resolution graphics
* Four sound channels
* Joystick support
* Disk access
* Floating point math

Programs are entered one line at a time.

Example:

```basic
10 PRINT "HELLO"
20 GOTO 10
```

---

# Chapter 2 - Running BASIC

When BASIC starts you'll see:

```
READY
```

You can now enter commands.

Example:

```basic
PRINT "HELLO"
```

Output

```
HELLO
READY
```

---

# Chapter 3 - Program Lines

Programs consist of numbered lines.

Example

```basic
10 PRINT "FIRST"
20 PRINT "SECOND"
30 END
```

Run it:

```
RUN
```

---

# Editing

To change a line:

```basic
20 PRINT "CHANGED"
```

To delete a line:

```basic
20
```

---

# Listing Programs

```basic
LIST
```

---

# Saving

Cassette

```basic
CSAVE
```

Disk

```basic
SAVE "D:GAME.BAS"
```

---

# Loading

Cassette

```basic
CLOAD
```

Disk

```basic
LOAD "D:GAME.BAS"
```

---

# Chapter 4 - Variables

Variables hold information.

```basic
A=5
B=10
C=A+B
PRINT C
```

Output

```
15
```

---

## String Variables

Strings end with $

```basic
NAME$="CHRIS"
PRINT NAME$
```

---

# Chapter 5 - Printing

```basic
PRINT "HELLO"
```

Multiple values

```basic
PRINT A,B,C
```

Semicolon

```basic
PRINT "HELLO";" WORLD"
```

Produces

```
HELLO WORLD
```

---

# Chapter 6 - Input

```basic
INPUT A
```

Example

```basic
10 PRINT "AGE";
20 INPUT A
30 PRINT A
```

---

# Chapter 7 - Math

Addition

```basic
A+B
```

Subtraction

```basic
A-B
```

Multiplication

```basic
A*B
```

Division

```basic
A/B
```

Exponent

```basic
A^B
```

---

# Useful Functions

```basic
ABS(X)
INT(X)
RND(1)
SQR(X)
SIN(X)
COS(X)
ATN(X)
LOG(X)
EXP(X)
```

---

Random number

```basic
X=INT(RND(1)*100)
```

---

# Chapter 8 - IF Statements

```basic
IF A=5 THEN PRINT "YES"
```

Greater than

```basic
IF A>10 THEN PRINT "BIG"
```

Less than

```basic
IF A<5 THEN PRINT "SMALL"
```

Not equal

```basic
<>
```

Example

```basic
IF A<>5 THEN PRINT "NOT FIVE"
```

---

# Chapter 9 - Loops

FOR

```basic
FOR I=1 TO 10
PRINT I
NEXT I
```

Step

```basic
FOR I=0 TO 100 STEP 10
PRINT I
NEXT I
```

Backward

```basic
FOR I=10 TO 1 STEP -1
PRINT I
NEXT I
```

---

# Chapter 10 - GOTO

```basic
10 PRINT "FOREVER"
20 GOTO 10
```

---

# Chapter 11 - GOSUB

```basic
10 GOSUB 100
20 END

100 PRINT "SUBROUTINE"
110 RETURN
```

---

# Chapter 12 - Arrays

Create

```basic
DIM A(100)
```

Use

```basic
A(5)=25
PRINT A(5)
```

---

String Array

```basic
DIM NAME$(10,20)
```

---

# Chapter 13 - Strings

Length

```basic
LEN(NAME$)
```

Left

```basic
NAME$(1,3)
```

Concatenate

```basic
A$="HELLO"
B$="WORLD"

PRINT A$+" "+B$
```

---

Convert number to string

```basic
STR$(100)
```

Convert string

```basic
VAL("25")
```

---

# Chapter 14 - Graphics

Set graphics mode

```basic
GRAPHICS 0
```

Text mode

```basic
GRAPHICS 1
```

High resolution

```basic
GRAPHICS 8
```

---

Plot

```basic
PLOT 20,20
```

Draw

```basic
DRAWTO 100,50
```

Rectangle

```basic
PLOT 20,20
DRAWTO 100,20
DRAWTO 100,60
DRAWTO 20,60
DRAWTO 20,20
```

---

Circle

```basic
10 GRAPHICS 8
20 X=100
30 Y=80
40 R=40
50 FOR A=0 TO 360 STEP 5
60 PLOT X+COS(A*.01745)*R,Y+SIN(A*.01745)*R
70 NEXT A
```

---

# Colors

```basic
SETCOLOR 2,8,10
```

Format

```
SETCOLOR register,hue,luminance
```

---

# Chapter 15 - Sound

SOUND {voice},{note},{tone},{loudness}

Simple tone

```basic
SOUND 0,100,10,8
```

Silence

```basic
SOUND 0,0,0,0
```

---

Play scale

```basic
10 FOR N=50 TO 150 STEP 10
20 SOUND 0,N,10,8
30 FOR T=1 TO 50:NEXT T
40 NEXT N
50 SOUND 0,0,0,0
```

---

# Chapter 16 - Joystick

Read joystick

```basic
PRINT STICK(0)
```

Fire button

```basic
PRINT STRIG(0)
```

Move object

```basic
10 X=80
20 GRAPHICS 1
30 POSITION X,10
40 ? "*"
50 IF STICK(0)=11 THEN X=X-1
60 IF STICK(0)=7 THEN X=X+1
70 GOTO 20
```

---

# Chapter 17 - Keyboard

Wait for key

```basic
GET K
```

Example

```basic
10 GET K
20 IF K=255 THEN 10
30 PRINT K
```

---

# Chapter 18 - Files

Open

```basic
OPEN #1,8,0,"D:DATA.TXT"
```

Write

```basic
PRINT #1;"HELLO"
```

Read

```basic
INPUT #1,A$
```

Close

```basic
CLOSE #1
```

---

# Chapter 19 - Useful Commands

Clear variables

```basic
CLR
```

Erase screen

```basic
GRAPHICS 0
```

Stop execution

```basic
STOP
```

Continue

```basic
CONT
```

New program

```basic
NEW
```

---

# Chapter 20 - Example Programs

## Guessing Game

```basic
10 N=INT(RND(1)*100)+1
20 PRINT "GUESS 1-100"
30 INPUT G
40 IF G=N THEN GOTO 80
50 IF G<N THEN PRINT "HIGHER"
60 IF G>N THEN PRINT "LOWER"
70 GOTO 30
80 PRINT "CORRECT!"
```

---

## Multiplication Table

```basic
10 FOR I=1 TO 10
20 FOR J=1 TO 10
30 PRINT I*J;
40 NEXT J
50 PRINT
60 NEXT I
```

---

## Star Field

```basic
10 GRAPHICS 8
20 X=INT(RND(1)*320)
30 Y=INT(RND(1)*192)
40 COLOR 1
50 PLOT X,Y
60 GOTO 20
```

---

## Bouncing Ball

```basic
10 GRAPHICS 8
20 X=10:Y=10
30 DX=1:DY=1
40 COLOR 1
50 PLOT X,Y
60 X=X+DX
70 Y=Y+DY
80 IF X>319 THEN DX=-1
90 IF X<0 THEN DX=1
100 IF Y>191 THEN DY=-1
110 IF Y<0 THEN DY=1
120 GOTO 40
```

---

# Chapter 21 - Performance Tips

Because Atari BASIC is interpreted, performance matters.

* Use variables instead of repeated calculations.
* Avoid unnecessary floating-point math.
* Prefer `FOR...NEXT` loops over many `GOTO` statements when appropriate.
* Minimize screen updates inside tight loops.
* Use integer-like values where possible, even though Atari BASIC stores numbers as floating point.
* Keep graphics mode changes to a minimum.
* Store repeated strings in variables instead of typing them multiple times.

Example:

Instead of:

```basic
PRINT "PLAYER"
PRINT "PLAYER"
PRINT "PLAYER"
```

Use:

```basic
NAME$="PLAYER"
PRINT NAME$
PRINT NAME$
PRINT NAME$
```

---

# Common Error Messages

| Error      | Meaning                         |
| ---------- | ------------------------------- |
| `ERROR-2`  | Out of memory                   |
| `ERROR-3`  | Value error                     |
| `ERROR-9`  | Array or string dimension error |
| `ERROR-11` | Math error                      |
| `ERROR-12` | Line not found                  |
| `ERROR-16` | Syntax error                    |

---

# Suggested Practice Projects

As you build your Atari BASIC skills, try creating:

1. Number guessing game
2. Text adventure
3. Tic-Tac-Toe
4. Hangman
5. Space shooter
6. Pong
7. Snake
8. Maze game
9. Music player
10. Paint program
11. Sprite editor
12. Character animation
13. Calendar
14. Calculator
15. Lunar lander

---

## Where to Go Next

Once you're comfortable with Atari BASIC, you'll be ready to explore more advanced topics such as:

* Memory locations and the `PEEK`/`POKE` commands
* Display List Interrupts (DLIs)
* Display list programming
* Custom character sets
* Player/Missile Graphics in depth
* Machine language integration with `USR()`
* Mixing Atari BASIC with 6502 assembly language for high-performance games

These topics unlock many of the techniques used in classic commercial Atari software and represent the next step from beginner to advanced Atari programming.
