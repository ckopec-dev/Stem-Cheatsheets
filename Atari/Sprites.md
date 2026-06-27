On the Atari 8-bit computers, what most people call **sprites** are actually called **Player/Missile Graphics (PMG)**. They are hardware-generated graphics that are independent of the normal graphics screen. Because the hardware draws them automatically every frame, they can move smoothly without redrawing the background.

This was one of the Atari's biggest advantages over many other home computers of the era.

---

# What are Player/Missile Graphics?

Imagine your game screen as being made of two layers:

```
+----------------------------------+
| Background Graphics              |
| (GRAPHICS mode screen)           |
|                                  |
|    Mountains     Stars           |
|                                  |
+----------------------------------+

               +

+----------------------------------+
| Player/Missile Graphics          |
|                                  |
|        @                         |
|                                  |
+----------------------------------+

               =

+----------------------------------+
| Mountains     @      Stars       |
+----------------------------------+
```

The hardware combines these layers automatically.

Unlike drawing with `PLOT` or `DRAWTO`, moving a sprite doesn't require erasing and redrawing the background.

---

# Players

The Atari has **4 Players**.

Think of a player as a narrow vertical bitmap.

```
Player 0

########
##    ##
########
##    ##
########
```

Each player is:

* 8 pixels wide
* Up to 256 pixels tall
* One independent color
* Independently movable

There are four of them:

```
Player 0
Player 1
Player 2
Player 3
```

---

# Missiles

There are also **4 Missiles**.

Missiles are much thinner.

```
#
#
#
#
#
#
```

Each missile is only 2 pixels wide.

Originally intended for bullets.

---

# Why Use Them?

Suppose you're making Space Invaders.

Without sprites:

```
Erase ship

Draw background

Draw ship

Repeat
```

Every frame.

This causes flicker.

With PMG:

```
Move Player 0
```

Done.

The Atari hardware redraws everything automatically.

---

# Hardware Overview

The hardware stores player graphics in memory.

Imagine memory contains:

```
Address

$4000

00011000
00111100
01111110
11111111
```

Each byte represents one horizontal row.

Bits set to 1 are visible pixels.

---

Example:

```
00111100
```

becomes

```
  ####
```

---

# Player Memory

Suppose Player 0 starts at address

```
$4000
```

Memory might look like

```
4000 00011000

4001 00111100

4002 01111110

4003 11111111

4004 01111110

4005 00111100

4006 00011000
```

Which appears as

```
   ##
  ####
 ######
########
 ######
  ####
   ##
```

---

# Moving a Player

Each player has an X position.

```
HPOSP0
```

Changing it moves the sprite.

```
HPOSP0 = 20

HPOSP0 = 100

HPOSP0 = 180
```

The graphic itself never changes.

Only its position.

---

# Vertical Position

There is no Y register.

Instead, the sprite is drawn wherever you place its bitmap in memory.

Example

```
Memory Row 0

Memory Row 1

Memory Row 2

Memory Row 50
   ####

Memory Row 51
 ######

Memory Row 52
########
```

The sprite appears beginning at scan line 50.

To move it vertically, you copy the bitmap higher or lower in player memory.

---

# Colors

Each player has its own color.

```
Player 0

Red

Player 1

Blue

Player 2

Green

Player 3

Yellow
```

Changing the color register changes the sprite instantly.

---

# Priorities

Sprites can appear:

In front of

```
Trees
```

or

Behind

```
Trees
```

This is controlled by priority registers.

---

# Double Width

Players can be stretched.

Normal

```
########
```

Double Width

```
################
```

Quad Width

```
################################
```

This lets one sprite become a large character.

---

# Combining Players

Two players can overlap.

Player 0

```
########
```

Player 1

```
########
```

Together they become a 16-pixel-wide sprite.

Many commercial games did this.

---

# Collision Detection

The hardware automatically detects collisions.

Examples:

Player hits player

```
@     X
```

Player hits playfield

```
@
##########
```

Missile hits player

```
------>
      @
```

You simply read collision registers.

No math required.

---

# Typical Game Usage

A game might use

| Object    | Uses      |
| --------- | --------- |
| Player 0  | Hero      |
| Player 1  | Enemy     |
| Player 2  | Enemy     |
| Player 3  | Boss      |
| Missile 0 | Bullet    |
| Missile 1 | Bullet    |
| Missile 2 | Laser     |
| Missile 3 | Explosion |

---

# Atari BASIC Support

Atari BASIC **does not include commands like**:

```basic
SPRITE
MOVE
PLAYER
```

Unlike some later BASIC dialects, PMG must be controlled by writing directly to memory with `POKE` or by calling machine-language routines.

For example, enabling Player/Missile Graphics involves setting up memory, pointing the hardware to the player data, and enabling DMA. Moving a player is then as simple as changing its horizontal position register with `POKE`.

---

# Why They Were So Fast

Suppose you have a spaceship moving across a 320×192 Graphics 8 screen.

### Without Player/Missile Graphics

Every frame you must:

1. Erase the old ship.
2. Restore the background.
3. Draw the ship in its new position.

This can involve updating hundreds of pixels each frame.

### With Player/Missile Graphics

Every frame you only need to:

1. Change the X position register (and optionally update the bitmap for animation).

The Atari's display hardware takes care of rendering the sprite over the playfield, which is why arcade-style games with many moving objects were possible even on a 1.79 MHz 6502 CPU.

---

## Next Step

The next logical topic is **programming Player/Missile Graphics in Atari BASIC**. This involves:

* Allocating PMG memory with `PMBASE`
* Enabling DMA with `GRACTL` and `SDMCTL`
* Creating sprite bitmaps in memory
* Displaying players and missiles
* Moving them with `POKE`
* Changing colors and sizes
* Reading collision registers
* Animating sprites

Once you understand these pieces, you'll be able to build smooth-moving arcade games such as **Pong**, **Space Invaders**, **Pac-Man**, and **Missile Command** in Atari BASIC.
