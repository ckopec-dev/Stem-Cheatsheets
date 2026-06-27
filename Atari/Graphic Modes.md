The Atari 400/800 computers support a variety of graphics modes through the operating system's `GRAPHICS` command in Atari BASIC. Each mode trades off resolution, number of colors, and memory usage.

## Atari BASIC Graphics Modes

| Mode | Resolution |        Colors | Type     | Typical Use                        |
| ---: | ---------: | ------------: | -------- | ---------------------------------- |
|    0 | 40×24 text |             2 | Text     | Standard text screen               |
|    1 | 20×24 text |             5 | Text     | Large colored text                 |
|    2 | 20×12 text |             5 | Text     | Extra large colored text           |
|    3 |      40×24 |             4 | Graphics | Low-resolution graphics            |
|    4 |      80×48 |             2 | Graphics | Medium-resolution graphics         |
|    5 |      80×48 |             4 | Graphics | Medium-resolution, more colors     |
|    6 |     160×96 |             2 | Graphics | Higher-resolution graphics         |
|    7 |     160×96 |             4 | Graphics | Higher-resolution with more colors |
|    8 |    320×192 |             2 | Graphics | High-resolution drawing            |
|    9 |     80×192 | 16 luminances | GTIA     | Monochrome with brightness         |
|   10 |     80×192 |      9 colors | GTIA     | Multiple colors                    |
|   11 |     80×192 |       16 hues | GTIA     | Hue display                        |

---

## Text Modes

### GRAPHICS 0

```
GRAPHICS 0
```

* 40 columns × 24 rows
* 8×8 character cells
* Upper/lower case
* Fastest mode
* Used by most BASIC programs

Example:

```basic
10 GRAPHICS 0
20 ? "HELLO WORLD"
```

---

### GRAPHICS 1

```
GRAPHICS 1
```

* 20 columns × 24 rows
* Double-width characters
* Four character colors plus background

Example:

```basic
10 GRAPHICS 1
20 POSITION 5,5
30 ? "ATARI"
```

---

### GRAPHICS 2

```
GRAPHICS 2
```

* 20 columns × 12 rows
* Double-width and double-height characters
* Good for titles

---

## Graphics Modes

### GRAPHICS 3

* Resolution: **40×24**
* Four colors
* Each pixel is large

Example:

```basic
10 GRAPHICS 3
20 COLOR 1
30 PLOT 10,10
40 DRAWTO 30,20
```

---

### GRAPHICS 4

* Resolution: **80×48**
* Two colors
* Better detail

---

### GRAPHICS 5

* Resolution: **80×48**
* Four colors

Example:

```basic
10 GRAPHICS 5
20 COLOR 2
30 CIRCLE 40,24,15
```

---

### GRAPHICS 6

* Resolution: **160×96**
* Two colors
* Good compromise between detail and speed

---

### GRAPHICS 7

* Resolution: **160×96**
* Four colors
* One of the most popular game modes

Example:

```basic
10 GRAPHICS 7
20 COLOR 1
30 FOR X=0 TO 159
40 PLOT X,X/2
50 NEXT X
```

---

### GRAPHICS 8

* Resolution: **320×192**
* Two colors
* Highest resolution available in BASIC
* Excellent for line drawings

Example:

```basic
10 GRAPHICS 8
20 COLOR 1
30 FOR X=0 TO 319
40 PLOT X,96
50 NEXT X
```

---

## GTIA Modes

These require a GTIA chip (standard on later Atari 800 revisions and all XL/XE computers).

### GRAPHICS 9

* 80×192
* 16 luminance levels
* Single hue
* Great for grayscale images

---

### GRAPHICS 10

* 80×192
* Nine colors
* Useful for colorful artwork

---

### GRAPHICS 11

* 80×192
* 16 hues
* Single brightness level

---

## Memory Requirements (Approximate)

| Mode | Screen Memory |
| ---: | ------------: |
|    0 |     960 bytes |
|    1 |     480 bytes |
|    2 |     240 bytes |
|    3 |     240 bytes |
|    4 |     480 bytes |
|    5 |     960 bytes |
|    6 |   1,920 bytes |
|    7 |   3,840 bytes |
|    8 |   7,680 bytes |
|    9 |   7,680 bytes |
|   10 |   7,680 bytes |
|   11 |   7,680 bytes |

---

## Choosing a Graphics Mode

| If you want...          | Use           |
| ----------------------- | ------------- |
| Standard text           | `GRAPHICS 0`  |
| Large colorful text     | `GRAPHICS 1`  |
| Huge title text         | `GRAPHICS 2`  |
| Very simple graphics    | `GRAPHICS 3`  |
| Medium-resolution games | `GRAPHICS 5`  |
| Fast arcade games       | `GRAPHICS 7`  |
| High-resolution drawing | `GRAPHICS 8`  |
| Grayscale pictures      | `GRAPHICS 9`  |
| Color artwork           | `GRAPHICS 10` |
| Hue demonstrations      | `GRAPHICS 11` |

## Common BASIC Drawing Commands

These commands work in graphics modes (3–11):

```basic
GRAPHICS 7     ; Set graphics mode
COLOR 2        ; Select drawing color
PLOT X,Y       ; Plot a pixel
DRAWTO X,Y     ; Draw a line
POSITION X,Y   ; Set text cursor
LOCATE X,Y,A   ; Read pixel color into A
FILLTO X,Y     ; Fill to a boundary (supported in compatible BASICs, not original Atari BASIC)
```

> **Note:** `CIRCLE` and `FILLTO` are **not** part of the original Atari BASIC included with the Atari 400/800. They are available in some enhanced BASICs or libraries. In original Atari BASIC, circles must be drawn manually (for example, using trigonometry or Bresenham's algorithm), and filled shapes require custom routines.
