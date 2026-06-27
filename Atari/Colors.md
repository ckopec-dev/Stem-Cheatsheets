In Atari BASIC, there are **two different kinds of colors**:

1. **Logical colors** (used with `COLOR` when drawing)
2. **Hardware colors** (used with `SETCOLOR` to define what the logical colors actually look like)

---

# Logical Drawing Colors

The `COLOR` statement selects which logical color is used by drawing commands such as `PLOT`, `DRAWTO`, and `FILLTO`.

The available color numbers depend on the graphics mode.

## Graphics Modes with 2 Colors

| COLOR | Meaning    |
| ----: | ---------- |
|     0 | Background |
|     1 | Foreground |

---

## Graphics Modes with 4 Colors

| COLOR | Meaning     |
| ----: | ----------- |
|     0 | Background  |
|     1 | Playfield 0 |
|     2 | Playfield 1 |
|     3 | Playfield 2 |

---

## Graphics Modes with 5 Colors (Most Common)

Graphics modes **3–8** use five logical colors.

| COLOR | Meaning     |
| ----: | ----------- |
|     0 | Background  |
|     1 | Playfield 0 |
|     2 | Playfield 1 |
|     3 | Playfield 2 |
|     4 | Playfield 3 |

Example:

```basic
10 GRAPHICS 7
20 COLOR 1
30 PLOT 20,20
40 COLOR 2
50 DRAWTO 80,80
60 COLOR 3
70 DRAWTO 120,20
```

---

# Using SETCOLOR

`SETCOLOR` defines what each logical color actually looks like.

Syntax:

```basic
SETCOLOR register,hue,luminance
```

where:

* **register** = 0–4
* **hue** = color
* **luminance** = brightness (0–15)

Example:

```basic
10 SETCOLOR 0,2,10
20 SETCOLOR 1,8,8
30 SETCOLOR 2,14,12
```

---

# Atari Hue Values

The Atari has **16 hue values** (0–15).

| Hue | Approximate Color    |
| --: | -------------------- |
|   0 | Gray / Black         |
|   1 | Gold                 |
|   2 | Orange               |
|   3 | Red-Orange           |
|   4 | Pink                 |
|   5 | Purple               |
|   6 | Purple-Blue          |
|   7 | Blue                 |
|   8 | Light Blue           |
|   9 | Turquoise            |
|  10 | Green-Blue           |
|  11 | Green                |
|  12 | Yellow-Green         |
|  13 | Orange-Green (Olive) |
|  14 | Light Orange         |
|  15 | Peach / Light Red    |

---

# Luminance Values

Brightness ranges from 0 to 15.

| Value | Brightness   |
| ----: | ------------ |
|     0 | Darkest      |
|     1 | Very Dark    |
|     2 | Dark         |
|     3 |              |
|     4 |              |
|     5 |              |
|     6 |              |
|     7 | Medium       |
|     8 |              |
|     9 |              |
|    10 | Bright       |
|    11 |              |
|    12 | Very Bright  |
|    13 |              |
|    14 | Nearly White |
|    15 | Brightest    |

---

# Common Color Combinations

| Color        | `SETCOLOR`         |
| ------------ | ------------------ |
| Black        | `SETCOLOR x,0,0`   |
| White        | `SETCOLOR x,0,15`  |
| Bright Red   | `SETCOLOR x,4,12`  |
| Dark Red     | `SETCOLOR x,4,5`   |
| Orange       | `SETCOLOR x,2,10`  |
| Yellow       | `SETCOLOR x,1,14`  |
| Bright Green | `SETCOLOR x,11,12` |
| Dark Green   | `SETCOLOR x,11,5`  |
| Cyan         | `SETCOLOR x,9,12`  |
| Sky Blue     | `SETCOLOR x,8,12`  |
| Dark Blue    | `SETCOLOR x,7,6`   |
| Purple       | `SETCOLOR x,5,10`  |
| Pink         | `SETCOLOR x,4,14`  |

Replace `x` with the color register you want to change (0–4).

---

# Example

```basic
10 GRAPHICS 7

20 SETCOLOR 0,2,12
30 SETCOLOR 1,11,10
40 SETCOLOR 2,8,12
50 SETCOLOR 3,4,12
60 SETCOLOR 4,0,0

70 COLOR 1
80 PLOT 10,10
90 DRAWTO 150,10

100 COLOR 2
110 DRAWTO 150,50

120 COLOR 3
130 DRAWTO 10,50

140 COLOR 4
150 DRAWTO 10,10
```

This program changes the palette and then draws lines using four different logical colors.

### Tip

The Atari's color system is based on **hue** (which color) and **luminance** (how bright), giving **16 × 16 = 256 possible color values**. While not all 256 are visually distinct on every television or monitor, this flexible palette was one of the Atari 8-bit family's standout graphics features.
